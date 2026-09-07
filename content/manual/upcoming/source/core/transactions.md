# Transactions

In MongoDB, an operation on a single document is atomic. Because you can
use embedded documents and arrays to capture relationships between data
in a single document structure instead of normalizing across multiple
documents and collections, this single-document atomicity obviates the
need for distributed transactions for many practical use cases.

For situations that require atomicity of reads and writes to multiple
documents (in a single or multiple collections), MongoDB supports
distributed transactions. With distributed transactions,
transactions can be used across multiple operations, collections,
databases, documents, and shards.

## Transactions API

------------------------------------------------------------------------

➤ Use the **Select your language** drop-down menu in the
upper-right to set the language of the following example.

------------------------------------------------------------------------

> **See also**
>
> For an example in `mongosh`, see
> txn-mongo-shell-example.

## Transactions and Atomicity

Distributed transactions are atomic:

- Transactions either apply all data changes or roll back the changes.
- If a transaction commits, all data changes made in the transaction
  are saved and are visible outside of the transaction.
- When a transaction aborts, all data changes made in the transaction
  are discarded without ever becoming visible. For example, if any
  operation in the transaction fails, the transaction aborts and all
  data changes made in the transaction are discarded without ever
  becoming visible.

> **See also**
>
> transactions-prod-consideration-outside-reads

## Transactions and Operations

Distributed transactions can be used across multiple operations,
collections, databases, documents, and shards.

For transactions:

For a list of operations not supported in transactions, see
transactions-ops-restricted.

> **See also**
>
> Transactions and Operations Reference

### Create Collections and Indexes in a Transaction

You can perform the following operations in a distributed transaction if the transaction is not a
cross-shard write transaction:

- Create collections.
- Create indexes on new empty collections created earlier in the same
  transaction.

When creating a collection inside a transaction:

- You can implicitly create a collection, such as with:
  - an insert operation
    for a non-existent collection, or
  - an update/findAndModify operation with `upsert: true`
    for a non-existent collection.
- You can explicitly create a collection using the `create`
  command or its helper `db.createCollection()`.

When creating an index inside a transaction[^1], the
index to create must be on either:

- a non-existent collection. The collection is created as part of the
  operation.
- a new empty collection created earlier in the same transaction.

#### Restrictions

- 

- 

- For explicit creation of a collection or an index inside a
  transaction, the transaction read concern level must be
  `"local"`.

  To explicitly create collections and indexes, use the following
  commands and methods:

- - Command
  - Method

- - `create`
  - `db.createCollection()`

- - `createIndexes`

  - `db.collection.createIndex()`  
    `db.collection.createIndexes()`

> **See also**
>
> transactions-ops-restricted

### Count Operation

### Distinct Operation

### Informational Operations

### Restricted Operations

> **See also**
>
> - txn-prod-considerations-ddl
>
> \- Transactions and Operations Reference

## Transactions and Sessions

- Transactions are associated with a session.
- You can have at most one open transaction at a time for a session.
- When using the drivers, each operation in the transaction must be
  associated with the session. Refer to your driver specific
  documentation for details.
- If a session ends and it has an open transaction, the transaction
  aborts.

## Read Concern/Write Concern/Read Preference

### Transactions and Read Preference

Operations in a transaction use the transaction-level read preference.

Using the drivers, you can set the transaction-level read preference at the transaction start:

- If the transaction-level read preference is unset, the transaction
  uses the session-level read preference.
- If transaction-level and the session-level read preference are unset,
  the transaction uses the client-level read preference. By default,
  the client-level read preference is `primary`.

### Transactions and Read Concern

Operations in a transaction use the transaction-level read concern. This means a read concern set at
the collection and database level is ignored inside the transaction.

You can set the transaction-level read concern at the transaction start.

- If the transaction-level read concern is unset, the transaction-level
  read concern defaults to the session-level read concern.
- If transaction-level and the session-level read concern are unset,
  the transaction-level read concern defaults to the client-level read
  concern. By default, the client-level read concern is
  `"local"` for reads on the primary. See also:
  - transactions-read-preference
  - /reference/mongodb-defaults

Transactions support the following read concern levels:

#### `"local"`

- Read concern `"local"` returns the most recent data
  available from the node but can be rolled back.
- 
- For transactions on sharded cluster, `"local"` read
  concern cannot guarantee that the data is from the same snapshot
  view across the shards. If snapshot isolation is required, use
  transactions-read-concern-snapshot read concern.
- 

#### `"majority"`

- If the transaction commits with write concern "majority", read concern `"majority"`
  returns data that has been acknowledged by a majority of the replica
  set members and can't be rolled back. Otherwise, read concern
  `"majority"` provides no guarantees that read operations
  read majority-committed data.
- For transactions on sharded cluster, read concern
  `"majority"` can't guarantee that the data is from the
  same snapshot view across the shards. If snapshot isolation is
  required, use read concern transactions-read-concern-snapshot.

#### `"snapshot"`

- Read concern `"snapshot"` returns data from a
  snapshot of majority committed data **if** the transaction commits
  with write concern "majority".
- If the transaction does not use write concern "majority" for the commit, the
  `"snapshot"` read concern provides **no** guarantee that
  read operations used a snapshot of majority-committed data.
- For transactions on sharded clusters, the
  `"snapshot"` view of the data **is** synchronized
  across shards.

### Transactions and Write Concern

Transactions use the transaction-level write concern to commit the write operations. Write
operations inside transactions must be run without an explicit write
concern specification and use the default write concern. At commit
time, the writes committed using the transaction-level write
concern.

> **Tip**
>
> Don't explicitly set the write concern for the individual write
> operations inside a transaction. Setting write concerns for the
> individual write operations inside a transaction returns an error.

You can set the transaction-level write concern at the transaction start:

- If the transaction-level write concern is unset, the
  transaction-level write concern defaults to the session-level write
  concern for the commit.
- If the transaction-level write concern and the session-level write
  concern are unset, the transaction-level write concern defaults to the
  client-level write concern of:
  - `w: "majority"` in MongoDB 5.0 and later,
    with differences for deployments containing arbiters. See
    wc-default-behavior.
  - `w: 1 \<\<number\>\>`

> **See also**
>
> /reference/mongodb-defaults

Transactions support all write concern w
values, including:

#### `w: 1`

- Write concern `w: 1 \<\<number\>\>` returns
  acknowledgment after the commit has been applied to the primary.

> **Important**
>
> When you commit with `w: 1 \<\<number\>\>`, your
> transaction can be rolled back if there is a failover.

- When you commit with `w: 1 \<\<number\>\>` write
  concern, transaction-level `"majority"` read concern
  provides **no** guarantees that read operations in the transaction
  read majority-committed data.
- When you commit with `w: 1 \<\<number\>\>` write
  concern, transaction-level `"snapshot"` read concern
  provides **no** guarantee that read operations in the transaction
  used a snapshot of majority-committed data.

#### `w: "majority"`

- Write concern `w: "majority"` returns
  acknowledgment after the commit has been applied to a majority of
  voting members.
- When you commit with `w: "majority"`
  write concern, transaction-level `"majority"` read
  concern guarantees that operations have read majority-committed
  data. For transactions on sharded clusters, this view of the
  majority-committed data is not synchronized across shards.
- When you commit with `w: "majority"`
  write concern, transaction-level `"snapshot"` read
  concern guarantees that operations have read from a synchronized
  snapshot of majority-committed data.

> **Note**
>
> Regardless of the write concern specified for the transaction, the driver applies
> `w: "majority"` as the write concern when
> retrying `commitTransaction`.

## General Information

The following sections describe more considerations for transactions.

### Production Considerations

For transactions in production environments, see
production-considerations. In addition, for sharded
clusters, see production-considerations-sharded.

### Arbiters

### Shard Configuration Restriction

> **Note**
>

### Diagnostics

To obtain transaction status and metrics, use the following methods:

- - Source
  - Returns

- - `db.serverStatus()` method  
    `serverStatus` command

  - Returns server-status-transactions metrics.

    Some `serverStatus` response fields are not returned on
    {+atlas+} Free clusters or {+flex-clusters+}. For more information, see
    free-shard-commands-with-limits in the {+atlas+}
    documentation.

- - `\$currentOp` aggregation pipeline
  - Returns:
    - `\$currentOp.transaction` if an operation is part of a
      transaction.
    - Information on inactive sessions that are holding locks as part
      of a transaction.
    - `\$currentOp.twoPhaseCommitCoordinator` metrics for
      sharded transactions that involves writes to multiple shards.

- - `db.currentOp()` method  
    `currentOp` command

  - Returns:
    - `currentOp.transaction` if an operation is part of a
      transaction.
    - `currentOp.twoPhaseCommitCoordinator` metrics for
      sharded transactions that involves writes to multiple shards.

- - `mongod` and `mongos` log messages
  - Includes information on slow transactions (which are transactions
    that exceed the `operationProfiling.slowOpThresholdMs`
    threshold) in the `TXN` log component.

### Feature Compatibility Version (FCV)

To use transactions, the featureCompatibilityVersion
for all members of the deployment must be at least:

- - Deployment
  - Minimum `featureCompatibilityVersion`
- - Replica Set
  - `4.0`

\* - Sharded Cluster  
- `4.2`

To check the FCV for a member, connect to the member and run the
following command:

```javascript
db.adminCommand( { getParameter: 1, featureCompatibilityVersion: 1 } )

```

For more information, see the
`setFeatureCompatibilityVersion` reference page.

### Storage Engines

### Limit Critical Section Wait Time

Starting in MongoDB 5.2 (and 5.0.4):

- When a query accesses a shard, a chunk migration or DDL operation may hold the critical section for the
  collection.
- To limit the time a shard waits for a critical section within a
  transaction, use the
  `metadataRefreshInTransactionMaxWaitMS` parameter.

> **Note**
>

## Learn More

- /core/transactions-in-applications
- /core/transactions-production-consideration
- /core/transactions-sharded-clusters
- /core/transactions-operations

[^1]: You can also run `db.collection.createIndex()` and
    `db.collection.createIndexes()` on existing indexes to check
    for existence. These operations return successfully without creating
    the index.
