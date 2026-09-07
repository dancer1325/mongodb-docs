# In-Memory Storage Engine for Self-Managed Deployments

The in-memory storage engine is part of general availability (GA) in 64-bit
builds. Other than some metadata and diagnostic data, the in-memory storage
engine does not maintain any on-disk data, including configuration data,
indexes, user credentials, etc.

By avoiding disk I/O, the in-memory storage engine allows for more
predictable latency of database operations.

## Specify In-Memory Storage Engine

To select the in-memory storage engine, specify:

- `inMemory` for the `--storageEngine` option, or the
  `storage.engine` setting if using a configuration file.
- `--dbpath`, or `storage.dbPath` if using a configuration
  file. Although the in-memory storage engine does not write data to
  the filesystem, it maintains in the `--dbpath` small metadata files
  and diagnostic data as well temporary files for building large
  indexes.

For example, from the command line:

```bash
mongod --storageEngine inMemory --dbpath <path> 

```

Or, if using the YAML configuration file format:

```yaml
storage:
   engine: inMemory
   dbPath: <path>

```

See cli-mongod-inmemory for configuration options specific to
this storage engine. Most `mongod` configuration options are
available for use with in-memory storage engine except for those
options that are related to data persistence, such as journaling or
encryption at rest configuration.

> **Warning**
>
> The in-memory storage engine does not persist data after process shutdown.

## Transaction (Read and Write) Concurrency

## Document Level Concurrency

The in-memory storage engine uses *document-level* concurrency control for write
operations. As a result, multiple clients can modify different
documents of a collection at the same time.

## Memory Use

In-memory storage engine requires that all its data (including indexes,
oplog if `mongod` instance is part of a replica set, etc.) must
fit into the specified `--inMemorySizeGB` command-line option
or `storage.inMemory.engineConfig.inMemorySizeGB` setting in
the YAML configuration file.

If a write operation would cause the data to exceed the specified
memory size, MongoDB returns with the error:

```bash
"WT_CACHE_FULL: operation would overflow cache"

```

To specify a new size, use the
`storage.inMemory.engineConfig.inMemorySizeGB` setting in the
YAML configuration file format:

```yaml
storage:
   engine: inMemory
   dbPath: <path>
   inMemory:
      engineConfig:
         inMemorySizeGB: <newSize>

```

Or use the command-line option \`--inMemorySizeGB\`:

```bash
mongod --storageEngine inMemory --dbpath <path> --inMemorySizeGB <newSize>

```

## Durability

The in-memory storage engine is non-persistent and does not write data
to a persistent storage. Non-persisted data includes
application data and system data, such as users, permissions, indexes,
replica set configuration, sharded cluster configuration, etc.

As such, the concept of journal or waiting for data to become
durable does not apply to the in-memory storage engine.

> **Note**
>

Write operations that specify a write concern `journaled` are acknowledged immediately. When an `mongod` instance
shuts down, either as result of the `shutdown` command or
due to a system error, recovery of in-memory data is impossible.

## Transactions

## Deployment Architectures

In addition to running as standalones, `mongod` instances that
use in-memory storage engine can run as part of a replica set or part
of a sharded cluster.

### Replica Set

You can deploy `mongod` instances that use in-memory storage
engine as part of a replica set. For example, as part of a three-member
replica set, you could have:

- two `mongod` instances run with in-memory storage engine.
- one `mongod` instance run with WiredTiger storage engine. Configure the WiredTiger member
  as a hidden member (i.e. `hidden: true`
  and `priority: 0`).

With this deployment model, only the `mongod` instances
running with the in-memory storage engine can become the primary.
Clients connect only to the in-memory storage engine `mongod`
instances. Even if both `mongod` instances running in-memory
storage engine crash and restart, they can sync from the member running
WiredTiger. The hidden `mongod` instance running with
WiredTiger persists the data to disk, including the user data, indexes,
and replication configuration information.

> **Note**
>
> In-memory storage engine requires that all its data (including oplog
> if `mongod` is part of replica set, etc.) fit into the
> specified `--inMemorySizeGB` command-line option or
> `storage.inMemory.engineConfig.inMemorySizeGB` setting. See
> inmemory-memory-use.

### Sharded Cluster

You can deploy `mongod` instances that use an in-memory
storage engine as part of a sharded cluster. The in-memory
storage engine avoids disk I/O to allow for more
predictable database operation latency. In a sharded cluster, a
shard can consist of a single `mongod` instance or a
replica set. For example, you could have one shard that
consists of the following replica set:

- two `mongod` instances run with in-memory storage engine
- one `mongod` instance run with WiredTiger storage engine. Configure the WiredTiger member
  as a hidden member (i.e. `hidden: true`
  and `priority: 0`).

To this shard, add the `tag` `inmem`. For
example, if this shard has the name `shardC`, connect to the
`mongos` and run `sh.addShardTag()`.

For example,

```javascript
sh.addShardTag("shardC", "inmem")

```

To the other shards, add a separate tag `persisted` .

```javascript
sh.addShardTag("shardA", "persisted")
sh.addShardTag("shardB", "persisted")

```

For each sharded collection that should reside on the `inmem` shard,
`assign to the entire chunk range` the tag
`inmem`:

```javascript
sh.addTagRange("test.analytics", { shardKey: MinKey }, { shardKey: MaxKey }, "inmem")

```

For each sharded collection that should reside across the
`persisted` shards, `assign to the entire chunk range` the tag
`persisted`:

```javascript
sh.addTagRange("salesdb.orders", { shardKey: MinKey }, { shardKey: MaxKey }, "persisted")

```

For the `inmem` shard, create a database or move the database.

> **Note**
>
> Read concern level `"snapshot"` is not officially supported
> with the in-memory storage engine.
