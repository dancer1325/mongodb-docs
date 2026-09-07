# Replica Set Oplog

The oplog (operations log) is a special capped collection that keeps a rolling record of all operations that modify
the data stored in your databases. If write operations do not modify any
data or fail, they do not create oplog entries.

Unlike other capped collections, the oplog
can grow past its configured size limit to avoid deleting the
`majority commit point`.

MongoDB applies database operations
on the primary and then records the operations on the
primary's oplog. The secondary members then copy and apply
these operations in an asynchronous process. All
replica set members contain a copy of the oplog, in the
`local.oplog.rs` collection, which allows them to maintain the
current state of the database.

To facilitate replication, all replica set members send heartbeats
(pings) to all other members. Any secondary member can import
oplog entries from any other member.

Each operation in the oplog is idempotent. That is, oplog
operations produce the same results whether applied once or multiple
times to the target dataset.

## Oplog Size

When you start a replica set member for the first time, MongoDB creates
an oplog of a default size if you do not specify the oplog size.

For Unix and Windows systems  
The default oplog size depends on the storage engine:

\* - Storage Engine  
- Default Oplog Size

- - storage-wiredtiger
  - 5% of free disk space

- - storage-inmemory

  \- 5% of physical memory  
  The default oplog size has the following constraints:

  - The minimum oplog size is 990 MB. If 5% of free disk space or
    physical memory (whichever is applicable based on your storage
    engine) is less than 990 MB, the default oplog size is 990 MB.
  - The maximum default oplog size is 50 GB. If 5% of free disk space or
    physical memory (whichever is applicable based on your storage
    engine) is greater than 50 GB, the default oplog size is 50 GB.

For 64-bit macOS systems  
The default oplog size is 192 MB of either free disk space or
physical memory depending on the storage engine:

\* - Storage Engine  
- Default Oplog Size

- - storage-wiredtiger
  - 192 MB of free disk space

\* - storage-inmemory

> - 192 MB of physical memory

In most cases, the default oplog size is sufficient. For example, if an
oplog is 5% of free disk space and fills up in 24 hours of operations,
then secondaries can stop copying entries from the oplog for up to 24
hours without becoming too stale to continue replicating. However, most
replica sets have much lower operation volumes, and their oplogs can
hold much higher numbers of operations.

Before `mongod` creates an oplog, you can specify its size with
the `oplogSizeMB` option. Once you have started a
replica set member for the first time, use the
`replSetResizeOplog` administrative command to change the
oplog size. `replSetResizeOplog` enables you to resize the
oplog dynamically without restarting the `mongod` process.

### Minimum Oplog Retention Period

To configure the minimum oplog retention period when starting the
`mongod`, either:

- Add the `storage.oplogMinRetentionHours` setting to the
  `mongod` configuration file.

  *-or-*

- Add the `--oplogMinRetentionHours` command line option.

To configure the minimum oplog retention period on a running
`mongod`, use `replSetResizeOplog`. Setting
the minimum oplog retention period while the `mongod`
is running overrides any values set on startup. You must update
the value of the corresponding configuration file setting or
command line option to persist those changes through a server
restart.

### Oplog Window

## Workloads that Might Require a Larger Oplog Size

If you can predict your replica set's workload to resemble one of the
following patterns, then you might want to create an oplog that is
larger than the default. Conversely, if your application predominantly
performs reads with a minimal amount of write operations, a smaller oplog
may be sufficient.

The following workloads might require a larger oplog size.

### Updates to Multiple Documents at Once

The oplog must translate multi-updates into individual operations in
order to maintain idempotency. This can use a great
deal of oplog space without a corresponding increase in data size or disk
use.

### Deletions Equal the Same Amount of Data as Inserts

If you delete roughly the same amount of data as you insert, the
database will not grow significantly in disk use, but the size
of the operation log can be quite large.

### Significant Number of In-Place Updates

If a significant portion of the workload is updates that do not
increase the size of the documents, the database records a large number
of operations but does not change the quantity of data on disk.

## Oplog Status

To view oplog status, including the size and the time range of
operations, issue the `rs.printReplicationInfo()` method. For
more information on oplog status, see
replica-set-troubleshooting-check-oplog-size.

### Replication Lag and Flow Control

Under various exceptional situations, updates to a secondary's oplog might lag behind the desired performance time. Use
`db.getReplicationInfo()` from a secondary member and the
replication status output to assess the current state of replication and
determine if there is any unintended replication delay.

See Replication Lag for more
information.

## Slow Oplog Application

Secondary members of a replica set log oplog entries that take
longer than the slow operation threshold to apply. These messages are
`logged` for the secondaries under the
`REPL` component with the text `applied op: <oplog entry> took <num>ms`.

```none
2018-11-16T12:31:35.886-05:00 I REPL   [repl writer worker 13] applied op: command { ... }, took 112ms

```

The slow oplog application logging on secondaries are:

- Not affected by the
  `logLevel`/\`systemLog.verbosity\` level (or the
  `systemLog.component.replication.verbosity` level); i.e. for
  oplog entries, the secondary logs only the slow oplog entries.
  Increasing the verbosity level does not log all oplog entries.
- Not captured by the profiler and not affected by the
  profiling level.

For more information on setting the slow operation threshold, see

- `mongod --slowms`
- `slowOpThresholdMs`
- The `profile` command or `db.setProfilingLevel()`
  shell helper method.

## Oplog Collection Behavior

You cannot `drop` the `local.oplog.rs` collection from any
replica set member if your MongoDB deployment uses the WiredTiger Storage Engine. You cannot drop
the `local.oplog.rs` collection from a standalone MongoDB instance.
`mongod` requires the oplog for
both replication and recovery of a node if the node goes down.

Starting in MongoDB 5.0, it is no longer possible to perform manual
write operations to the oplog on a
cluster running as a replica set. Performing write
operations to the oplog when running as a
standalone instance should only be done with
guidance from MongoDB Support.
