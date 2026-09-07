# Retryable Writes

Retryable writes allow MongoDB drivers to automatically retry certain
write operations a single time if they encounter network errors, or if
they cannot find a healthy primary in the replica set or sharded cluster.

## Prerequisites

Retryable writes have the following requirements:

Supported Deployment Topologies  
Retryable writes require a replica set
or sharded cluster, and do **not**
support standalone instances.

Supported Storage Engine  
Retryable writes require a storage engine supporting document-level
locking, such as the WiredTiger or
in-memory storage engines.

3.6+ MongoDB Drivers  
Clients require MongoDB drivers updated for MongoDB 3.6 or greater:

MongoDB Version  
The MongoDB version of every node in the cluster must be `3.6` or
greater, and the `featureCompatibilityVersion` of each node in the
cluster must be `3.6` or greater. See
`setFeatureCompatibilityVersion` for more information on
the `featureCompatibilityVersion` flag.

Write Acknowledgment  
Write operations issued with a /reference/write-concern of `0`
are **not** retryable.

## Retryable Writes and Multi-Document Transactions

The transaction commit and abort operations
are retryable write operations. If the commit operation or the abort
operation encounters an error, MongoDB drivers retry the operation a
single time regardless of whether `retryWrites` is set to
`false`.

The write operations inside the transaction are not individually
retryable, regardless of value of `retryWrites`.

For more information on transactions, see /core/transactions.

## Enabling Retryable Writes

MongoDB Drivers
`mongosh`
Retryable writes are enabled by default in `mongosh`. To
disable retryable writes, use the `--retryWrites=false` command line option:

```bash
mongosh --retryWrites=false

```

## Retryable Write Operations

The following write operations are retryable when issued with
acknowledged write concern; e.g., /reference/write-concern
cannot be `{w: 0} \<\<number\>\>`.

> **Note**
>
> The write operations inside the transactions are not individually retryable.

- - Methods
  - Descriptions

- - `db.collection.insertOne()`  
    `db.collection.insertMany()`

  - Insert operations

- - `db.collection.updateOne()`  
    `db.collection.replaceOne()`

  - Single-document update operations

- - `db.collection.deleteOne()`  
    `db.collection.remove()` where `justOne` is `true`

  - Single document delete operations

- - `db.collection.findAndModify()`  
    `db.collection.findOneAndDelete()`  
    `db.collection.findOneAndReplace()`  
    `db.collection.findOneAndUpdate()`

  - `findAndModify` operations. All `findAndModify` operations
    are single document operations.

- - `db.collection.bulkWrite()` with the following write
    operations:
    - bulkwrite-write-operations-insertOne
    - updateOne
    - bulkwrite-write-operations-replaceOne
    - deleteOne
  - Bulk write operations that only consist of the single-document
    write operations. A retryable bulk operation can include any
    combination of the specified write operations but cannot include
    any multi-document write operations, such as `updateMany`.

\* - `Bulk` operations for:

> - `Bulk.find.removeOne()`
> - `Bulk.find.replaceOne()`
> - `Bulk.find.updateOne()`
>
> \- Bulk write operations that only consist of the single-document
> write operations. A retryable bulk operation can include any
> combination of the specified write operations but cannot include
> any multi-document write operations, such as `update` which
> specifies `true` for the `multi` option.

## Behavior

### Persistent Network Errors

MongoDB retryable writes make only **one** retry attempt. This helps
address transient network errors and
replica set elections, but not persistent
network errors.

### Failover Period

If the driver cannot find a healthy primary in the destination
replica set or sharded cluster shard, the drivers wait
`serverSelectionTimeoutMS` milliseconds to determine the new
primary before retrying. Retryable writes do not address instances where
the failover period exceeds `serverSelectionTimeoutMS`.

> **Warning**
>
> If the client application becomes temporarily unresponsive for more
> than the `localLogicalSessionTimeoutMinutes` after
> issuing a write operation, there is a chance that when the client
> applications starts responding (without a restart), the write
> operation may be retried and applied again.

### Diagnostics

The `serverStatus` command, and its `mongosh`
shell helper `db.serverStatus()` includes statistics on
retryable writes in the `transactions` section.

### Retryable Writes Against `local` Database

The official MongoDB drivers enable retryable writes by default.
Applications which write to the `local`
database will encounter write errors
*unless* retryable writes are explicitly disabled.

To disable retryable writes, specify
`retryWrites=false` in the
connection string for the MongoDB cluster.

### Error Handling
