# Sharded Cluster Balancer

The MongoDB balancer is a background process that monitors the amount of
data on each shard for each sharded collection. When the amount
of data for a sharded collection on a given shard reaches specific
migration thresholds, the balancer
attempts to automatically migrate data between shards and reach an even
amount of data per shard while respecting the zones. By default, the balancer process is always enabled.

The balancing procedure for sharded clusters
is entirely transparent to the user and application layer, though there
may be some performance impact while the procedure takes place.

The balancer runs on the primary of the config server replica set
(CSRS).

To configure collection balancing for a single collection, see
`configureCollectionBalancing`.

To manage the sharded cluster balancer, see
sharded-cluster-balancer.

## Balancer Internals

Range migrations carry some overhead in terms of bandwidth and
workload, both of which can impact database performance.
The balancer attempts to minimize the impact by:

- Restricting a shard to at most one migration at any given time.
  Specifically, a shard cannot participate in multiple data migrations
  at the same time. The balancer migrates ranges one at a time.

  MongoDB can perform parallel data migrations, but a shard can
  participate in at most one migration at a time. For a sharded cluster
  with *n* shards, MongoDB can perform at most *n/2* (rounded down)
  simultaneous migrations.

  See also range-migration-queuing.

- Starting a balancing round only when the **difference in the amount
  of data** between the shard with the most data for a sharded collection
  and the shard with the least data for that collection reaches the
  migration threshold.

You can disable the balancer temporarily for maintenance, but leaving
the balancer disabled for extended periods of time can degrade cluster
performance. For more information, see
sharding-balancing-disable-temporally.

You can also limit the window during which the balancer runs to prevent
it from impacting production traffic. See Schedule the Balancing Window for details.

> **Note**
>
> The specification of the balancing window is relative to the local
> time zone of the primary of the config server replica set.

> **See also**
>
> sharded-cluster-balancer

### Adding and Removing Shards from the Cluster

Adding a shard to a cluster creates an imbalance, since the new
shard has no data. While MongoDB begins migrating data to the new
shard immediately, it can take some time before the cluster balances.
See the Add Shards to a Cluster
tutorial for instructions on adding a shard to a cluster.

> **Tip**
>
> Starting in MongoDB 8.0, you can reshard to the same key to move data.
> If your application meets the
> resharding requirements, you can use the
> `reshardCollection` command to redistribute data across the
> cluster to include the new shards. For more information, see
> reshard-to-same-key. This process is much faster than the
> alternative range-migration-procedure.

Removing a shard from a cluster creates a similar imbalance, since data
residing on that shard must be redistributed throughout the cluster.
While MongoDB begins draining a removed shard immediately, it can take
some time before the cluster balances. *Do not* shutdown the servers
associated to the removed shard during this process.

See the Remove Shards from a Cluster tutorial for instructions on
safely removing a shard from a cluster.

> **See also**
>
> `sh.balancerCollectionStatus()`

## Range Migration Procedure

All range migrations use the following procedure:

1.  The balancer process sends the `moveRange` command to
    the source shard.
2.  The source starts the move when it receives an internal
    `moveRange` command. During the migration process,
    operations to the range are sent to the source shard. The source
    shard is responsible for incoming write operations for the range.
3.  The destination shard builds any indexes required by the source
    that do not exist on the destination.
4.  The destination shard begins requesting documents in the range and
    starts receiving copies of the data. See also
    range-migration-replication.
5.  After receiving the final document in the range, the
    destination shard starts a synchronization process to ensure that it
    has the changes to the migrated documents that occurred during the
    migration.
6.  When fully synchronized, the source shard connects to the
    config database and updates the cluster metadata with the new
    location for the range.
7.  After the source shard completes the update of the metadata,
    and once there are no open cursors on the range, the source shard
    deletes its copy of the documents.

> **Note**
>
> If the balancer needs to perform additional chunk migrations from
> the source shard, the balancer can start the next chunk migration
> without waiting for the current migration process to finish this
> deletion step. See chunk-migration-queuing.

> **See also**
>
> `shardingStatistics.countDonorMoveChunkLockTimeout`

### Migration Thresholds

To minimize the impact of balancing on the cluster, the
balancer only begins balancing after the distribution of
data for a sharded collection has reached certain thresholds.

A collection is considered balanced if the difference in data between
shards (for that collection) is less than three times the configured
range size for the collection.
For the default range size of `128MB`, two shards must have a data
size difference for a given collection of at least `384MB` for a
migration to occur.

> **See also**
>
> `sh.balancerCollectionStatus()`

### Asynchronous Range Migration Cleanup

To migrate data from a shard, the balancer migrates the
data one range at a time. However, the balancer does not wait for the
current migration's delete phase to complete before starting the next
range migration. See sharding-range-migration for the range
migration process and the delete phase.

This queuing behavior allows shards to unload data more quickly in
cases of heavily imbalanced cluster, such as when performing initial
data loads without pre-splitting and when adding new shards.

This behavior also affects the `moveRange` command, and
migration scripts that use the `moveRange` command may
proceed more quickly.

In some cases, the delete phases may persist longer. Range migrations
are enhanced to be more resilient in the event of a failover during the
delete phase. Orphaned documents are cleaned up even if a replica set's
primary crashes or restarts during this phase.

> **Important**
>

For more information, see wait-for-delete-setting.

> **Note**
>
> Range deletion is a resource intensive operation that can
> result in significant cache and I/O stress as the cluster
> deletes the documents.
>
> In cases where you plan to move a large amount of data, such
> as when adding shards to a cluster or during the initial
> distribution of a sharded collection across multiple shards,
> consider resharding the collection instead. Resharding
> operations don't require range cleanup, which makes them
> much less stressful on the cluster.
>
> For more information, see sharding-resharding.

### Range Migration and Replication

During range migration, the `_secondaryThrottle` value determines
when the migration proceeds with next document in the range.

In the `config.settings` collection:

- If the `_secondaryThrottle` setting for the balancer is set to a
  write concern, each document moved during range migration must receive
  the requested acknowledgment before proceeding with the next
  document.
- If the `_secondaryThrottle` setting is unset, the migration process
  does not wait for replication to a secondary and instead continues
  with the next document.

To update the `_secondaryThrottle` parameter for the balancer, see
sharded-cluster-config-secondary-throttle for an example.

Independent of any `_secondaryThrottle` setting, certain phases of
the range migration have the following replication policy:

- MongoDB briefly pauses all application reads and writes to the
  collection being migrated to on the source shard before updating the
  config servers with the range location. MongoDB resumes application
  reads and writes after the update. The range move requires all writes
  to be acknowledged by majority of the members of the replica set both
  before and after committing the range move to config servers.
- When an outgoing migration finishes and cleanup occurs, all
  writes must be replicated to a majority of servers before further
  cleanup (from other outgoing migrations) or new incoming migrations
  can proceed.

To update the `_secondaryThrottle` setting in the
`config.settings` collection, see
sharded-cluster-config-secondary-throttle for an example.

### Maximum Number of Documents Per Range to Migrate

### Range Deletion Performance Tuning

You can tune the performance impact of range deletions
with `rangeDeleterBatchSize` and
`rangeDeleterBatchDelayMS`.

For example:

- To limit the number of documents deleted per batch, you can set
  `rangeDeleterBatchSize` to a small value such as `32`.
- To add an additional delay between batch deletions, you can set
  `rangeDeleterBatchDelayMS` above the current default of
  `20` milliseconds.

> **Note**
>
> If there are ongoing read operations or open cursors on the
> collection targeted for deletes, range deletion processes may
> not proceed.

### Change Streams and Orphan Documents

## Shard Size

By default, MongoDB attempts to fill all available disk space with
data on every shard as the data set grows. To ensure that the cluster
always has the capacity to handle data growth, monitor disk
usage as well as other performance metrics.

## Chunk Size and Balancing

For details about defragmenting sharded collections, see
defragment-sharded-collections.

## Reshard to Balance
