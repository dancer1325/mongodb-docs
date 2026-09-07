# Methods

The following document lists the available methods in the MongoDB Shell.
Click a method to see its documentation in the
\[MongoDB Manual\](<https://www.mongodb.com/docs/manual//>), including syntax and examples.

## Administration Methods

- - Method
  - Description
- - `db.adminCommand()`
  - Runs a command against the `admin` database.
- - `db.currentOp()`
  - Reports the current in-progress operations.
- - `db.killOp()`
  - Terminates a specified operation.
- - `db.shutdownServer()`
  - Shuts down the current `mongod` or `mongos`
    process cleanly and safely.
- - `db.fsyncLock()`
  - Flushes writes to disk and locks the database to prevent write
    operations and assist backup operations.
- - `db.fsyncUnlock()`
  - Allows writes to continue on a database locked with
    `db.fsyncLock()`.

## Atlas Search Index Methods

\[Atlas Search indexes\](<https://www.mongodb.com/docs/atlas/atlas-search/atlas-search-overview/#fts-indexes/>) let you query data
in \[Atlas Search\](<https://www.mongodb.com/docs/atlas/atlas-search/>). Atlas Search indexes enable
performant text search queries by mapping search terms to the documents
that contain those terms.

Use the following methods to manage Atlas Search indexes.

> **Important**
>
> The following methods can only be run on deployments hosted on
> \[MongoDB Atlas\](<https://www.mongodb.com/docs/atlas//>).

- - Name
  - Description
- - db.collection.createSearchIndex()
  - Creates one or more Atlas Search indexes on a specified
    collection.
- - db.collection.dropSearchIndex()
  - Deletes an existing Atlas Search index.
- - db.collection.getSearchIndexes()
  - Returns information about existing Atlas Search indexes on a specified
    collection.

\* - db.collection.updateSearchIndex()

> - Updates an existing Atlas Search index.

## Bulk Operation Methods

- - Method
  - Description
- - `db.collection.initializeOrderedBulkOp()`
  - Initializes and returns a new `Bulk()` operations builder
    for a collection. The builder constructs an ordered list of write
    operations that MongoDB executes in bulk.
- - `db.collection.initializeUnorderedBulkOp()`
  - Initializes and returns a new `Bulk()` operations builder
    for a collection. The builder constructs an unordered list of write
    operations that MongoDB executes in bulk.
- - `Bulk()`
  - Creates a bulk operations builder used to construct a list of write
    operations to perform in bulk for a single collection. To instantiate
    the builder, use either the `db.collection.initializeOrderedBulkOp()`
    or the `db.collection.initializeUnorderedBulkOp()` method.
- - `Bulk.execute()`
  - Executes the list of operations built by the `Bulk()`
    operations builder.
- - `Bulk.find()`
  - Specifies a query condition for an update or a remove operation.
- - `Bulk.find.hint()`
  - Sets the **hint** option that specifies the index to support the
    bulk operation.
- - `Bulk.find.remove()`
  - Adds a remove operation to a bulk operations list.
- - `Bulk.find.removeOne()`
  - Adds a single document remove operation to a bulk operations list.
- - `Bulk.find.replaceOne()`
  - Adds a single document replacement operation to a bulk operations
    list.
- - `Bulk.find.updateOne()`
  - Adds a single document update operation to a bulk operations list.
- - `Bulk.find.update()`
  - Adds a **multi** update operation to a bulk operations list. The
    method updates specific fields in existing documents.
- - `Bulk.find.upsert()`
  - Sets the \[upsert\](<https://www.mongodb.com/docs/manual/reference/glossary/#term-upsert/>) option
    to `true` for an update or a replacement operation.
- - `Bulk.getOperations()`
  - Returns an array of write operations executed through
    `Bulk.execute()`.
- - `Bulk.insert()`
  - Adds an insert operation to a bulk operations list.
- - `Bulk.toJSON()`
  - Returns a JSON document that contains the number of operations and
    batches in the `Bulk()` object.
- - `Bulk.toString()`
  - Returns as a string a JSON document that contains the number of
    operations and batches in the `Bulk()` object.

## Collection Methods

- - Method
  - Description
- - `db.collection.aggregate()`
  - Provides access to the
    \[aggregation pipeline\](<https://www.mongodb.com/docs/manual/core/aggregation-pipeline/>).
- - `db.collection.bulkWrite()`
  - Provides bulk write operation functionality.
- - `db.collection.count()`
  - Deprecated in `mongosh` 1.0.6. Use
    `db.collection.countDocuments()` or
    `db.collection.estimatedDocumentCount()` instead.
- - `db.collection.countDocuments()`
  - Returns a count of the number of documents in a collection or a
    view. Wraps the \[`\$group`\](<https://www.mongodb.com/docs/manual/reference/operator/aggregation/group/>) aggregation stage with a
    `\$sum` expression.
- - `db.collection.estimatedDocumentCount()`
  - Returns an approximate count of the documents in a collection or
    a view.
- - `db.collection.createIndex()`
  - Builds an index on a collection.
- - `db.collection.createIndexes()`
  - Builds one or more indexes on a collection.
- - `db.collection.dataSize()`
  - Returns the size of the collection. Wraps the
    `size` field in the output of the
    `collStats`.
- - `db.collection.deleteOne()`
  - Deletes a single document in a collection.
- - `db.collection.deleteMany()`
  - Deletes multiple documents in a collection.
- - `db.collection.distinct()`
  - Returns an array of documents that have distinct values for the
    specified field.
- - `db.collection.drop()`
  - Removes the specified collection from the database.
- - `db.collection.dropIndex()`
  - Removes a specified index on a collection.
- - `db.collection.dropIndexes()`
  - Removes all indexes on a collection.
- - `db.collection.ensureIndex()`
  - Deprecated. Use `db.collection.createIndex()`.
- - `db.collection.explain()`
  - Returns information on the query execution of various methods.
- - `db.collection.find()`
  - Performs a query on a collection or a view and returns a cursor
    object.
- - `db.collection.findAndModify()`
  - Atomically modifies and returns a single document.
- - `db.collection.findOne()`
  - Performs a query and returns a single document.
- - `db.collection.findOneAndDelete()`
  - Finds a single document and deletes it.
- - `db.collection.findOneAndReplace()`
  - Finds a single document and replaces it.
- - `db.collection.findOneAndUpdate()`
  - Finds a single document and updates it.
- - `db.collection.getIndexes()`
  - Returns an array of documents that describe the existing indexes
    on a collection.
- - `db.collection.getShardDistribution()`
  - Prints the data distribution statistics for a sharded collection.
- - `db.collection.getShardVersion()`
  - Returns information regarding the state of data in a sharded
    cluster.
- - `db.collection.insertOne()`
  - Inserts a new document in a collection.
- - `db.collection.insertMany()`
  - Inserts several new document in a collection.
- - `db.collection.isCapped()`
  - Reports if a collection is a capped collection.
- - `db.collection.mapReduce()`
  - Runs map-reduce aggregation operations on a collection.
- - `db.collection.reIndex()`
  - Rebuilds all existing indexes on a collection.
- - `db.collection.renameCollection()`
  - Changes the name of a collection.
- - `db.collection.replaceOne()`
  - Replaces a single document in a collection.
- - `db.collection.stats()`
  - Reports on the state of a collection. Provides a wrapper around
    the `collStats`.
- - `db.collection.storageSize()`
  - Reports the total size used by the collection in bytes. Provides
    a wrapper around the `storageSize` field of the
    `collStats` output.
- - `db.collection.totalIndexSize()`
  - Reports the total size used by the indexes on a collection.
    Provides a wrapper around the `totalIndexSize`
    field of the `collStats` output.
- - `db.collection.totalSize()`
  - Reports the total size of a collection, including the size of
    all documents and all indexes on a collection.
- - `db.collection.updateOne()`
  - Modifies a single document in a collection.
- - `db.collection.updateMany()`
  - Modifies multiple documents in a collection.
- - `db.collection.validate()`
  - Validates a collection.

\* - `db.collection.watch()`

> - Opens a \[change stream cursor\](<https://www.mongodb.com/docs/manual/changeStreams/>) on the
>   collection.

## Connection Methods

- - Method
  - Description

- - `Mongo()`

  - JavaScript constructor to instantiate a database connection from
    the `mongo` shell or from a JavaScript file.

    The `Mongo()` method has the following parameters:

- - Parameter
  - Type
  - Description

- - `host`

  - string

  - *Optional*

    The connection string for the target database
    connection.

    If omitted, `Mongo` instantiates a connection to the
    localhost interface on the default port `27017`.

- - `autoEncryptionOpts`
  - Document
  - *Optional*

> **New in version 4.2**
>

> Configuration parameters for enabling
> \[Client-Side Field Level Encryption\](<https://www.mongodb.com/docs/manual/core/security-client-side-encryption/>).
>
> `autoEncryptionOpts` overrides the
> existing client-side field level encryption configuration of
> the database connection. If omitted, `Mongo()`
> inherits the client-side field level encryption configuration
> of the current database connection.
>
> For documentation of usage and syntax, see
> autoEncryptionOpts.

- - `Mongo.getDB()`
  - Returns a database object.
- - `Mongo.setReadPref()`
  - Sets the read preference for the MongoDB connection.
- - `Mongo.watch()`
  - Opens a \[change stream cursor\](<https://www.mongodb.com/docs/manual/changeStreams/>) for a replica
    set or a sharded cluster to report on all its non-system collections
    across its databases, with the exception of the `admin`, `local`,
    and `config` databases.

## Cursor Methods

- - Method
  - Description

- - `cursor.addOption()`
  - Adds special wire protocol flags that modify the behavior of the
    query.

- - `cursor.batchSize()`

  - Specifies the maximum number of documents MongoDB can return to the
    client within each batch returned in a query result. By default, the initial batch
    size is the lesser of `101` documents or 16 mebibytes (MiB) worth of documents.
    Subsequent batches have a maximum size of 16 MiB. This option can enforce a smaller
    limit than 16 MiB, but not a larger one. When set, the
    `batchSize` is the lesser of `batchSize` documents or 16 MiB worth of
    documents.

    A `batchSize` of `0` means that the cursor is established, but no documents
    are returned in the first batch.

    The following example query returns results in batches of
    100:

```sh
db.myCollection.find().batchSize(100)

```

- - `cursor.close()`
  - Close a cursor and free associated server resources.
- - `cursor.collation()`
  - Specifies the collation for the cursor returned by the
    `db.collection.find()`.
- - `cursor.comment()`
  - Attaches a comment to the query to allow for traceability in the
    logs and the system.profile collection.
- - `cursor.count()`
  - Modifies the cursor to return the number of documents in the
    result set rather than the documents themselves.
- - `cursor.explain()`
  - Reports on the query execution plan for a cursor.
- - `cursor.forEach()`
  - Applies a JavaScript function for every document in a cursor.
- - `cursor.hasNext()`
  - Returns `true` if the cursor has documents and can be iterated.
- - `cursor.hint()`
  - Forces MongoDB to use a specific index for a query.
- - `cursor.isClosed()`
  - Returns `true` if the cursor is closed.
- - `cursor.isExhausted()`
  - Returns `true` if the cursor is closed *and* there are no
    objects remaining in the batch.
- - `cursor.itcount()`
  - Computes the total number of documents in the cursor client-side
    by fetching and iterating the result set.
- - `cursor.limit()`
  - Constrains the size of a cursor's result set.
- - `cursor.map()`
  - Applies a function to each document in a cursor and collects the
    return values in an array.
- - `cursor.max()`
  - Specifies an exclusive upper index bound for a cursor. For use
    with `cursor.hint()`
- - `cursor.maxTimeMS()`
  - Specifies a cumulative time limit in milliseconds for processing
    operations on a cursor.
- - `cursor.min()`
  - Specifies an inclusive lower index bound for a cursor. For use
    with `cursor.hint()`
- - `cursor.next()`
  - Returns the next document in a cursor.
- - `cursor.noCursorTimeout()`
  - Instructs the server to avoid closing a cursor automatically
    after a period of inactivity.
- - `cursor.objsLeftInBatch()`
  - Returns the number of documents left in the current cursor batch.
- - `cursor.readConcern()`
  - Specifies a read concern for a
    `db.collection.find()` operation.
- - `cursor.readPref()`
  - Specifies a read preference to a cursor to control how
    the client directs queries to a replica set.
- - `cursor.returnKey()`
  - Modifies the cursor to return index keys rather than the
    documents.
- - `cursor.showRecordId()`
  - Adds an internal storage engine ID field to each document
    returned by the cursor.
- - `cursor.size()`
  - Returns a count of the documents in the cursor after applying
    `skip()` and `limit()` methods.
- - `cursor.skip()`
  - Returns a cursor that begins returning results only after
    passing or skipping a number of documents.
- - `cursor.sort()`
  - Returns results ordered according to a sort specification.
- - `cursor.tailable()`
  - Marks the cursor as tailable. Only valid for cursors over capped
    collections.
- - `cursor.toArray()`
  - Returns an array that contains all documents returned by the
    cursor.

## Database Methods

- - Method
  - Description
- - `db.aggregate()`
  - Runs admin/diagnostic pipeline which does not require an
    underlying collection.
- - `db.createCollection()`
  - Creates a new collection or view.
- - `db.createView()`
  - Creates a view as the result of applying the specified
    aggregation pipeline to the source collection or view.
- - `db.commandHelp()`
  - Displays help text for the specified database command.
- - `db.dropDatabase()`
  - Removes the current database.
- - `db.getCollection()`
  - Returns a collection or view object. Used to access collections
    with names that are not valid in the `mongo` shell.
- - `db.getCollectionInfos()`
  - Returns collection information for all collections and views in
    the current database.
- - `db.getCollectionNames()`
  - Lists all collections and views in the current database.
- - `db.getMongo()`
  - Returns the current database connection.
- - `db.getLogComponents()`
  - Returns the current log verbosity settings.
- - `db.getName()`
  - Returns the name of the current database.
- - `db.getProfilingStatus()`
  - Returns the current \[profile level\](<https://www.mongodb.com/docs/manual/reference/command/profile/#dbcmd.profile/>), \[slowOpThresholdMs\](<https://www.mongodb.com/docs/manual/reference/configuration-options/#operationProfiling.slowOpThresholdMs/>)
    setting, and \[slowOpSampleRate\](<https://www.mongodb.com/docs/manual/reference/configuration-options/#operationProfiling.slowOpSampleRate/>)
    setting.
- - `db.getSiblingDB()`
  - Provides access to the specified database.
- - `db.listCommands()`
  - Provides a list of all database commands.
- - `db.logout()`
  - Ends an authenticated session.
- - `db.printShardingStatus()`
  - Prints a formatted report of the sharding configuration and the
    information regarding existing chunks in a sharded cluster.
- - `db.runCommand()`
  - Runs a \[database command\](<https://www.mongodb.com/docs/manual/reference/command/>).
- - `db.setLogLevel()`
  - Sets a single verbosity level for \[log messages\](<https://www.mongodb.com/docs/manual/reference/log-messages/>).
- - `db.setProfilingLevel()`
  - Configures the database \[profiler level\](<https://www.mongodb.com/docs/manual/reference/method/db.setProfilingLevel/#set-profiling-level-level/>),
    \[slowms\](<https://www.mongodb.com/docs/manual/reference/method/db.setProfilingLevel/#set-profiling-level-options-slowms/>),
    and \[sampleRate\](<https://www.mongodb.com/docs/manual/reference/method/db.setProfilingLevel/#set-profiling-level-options-samplerate/>).
- - `db.watch()`
  - Opens a \[change stream cursor\](<https://www.mongodb.com/docs/manual/change-streams/>) for a
    database to report on all its non-system collections.

## In-Use Encryption Methods

> **Note Limitations**
>
> - Automatic encryption is only available when `mongosh` is connected
>   to an Atlas cluster or a MongoDB Enterprise Server. For details,
>   see \[Automatic Encryption\](<https://www.mongodb.com/docs/manual/core/security-automatic-client-side-encryption/>). The methods
>   listed in this section are used for *manual* encryption, and are
>   supported on non-enterprise servers.
> - Automatic encryption is not available with the Homebrew installation
>   of `mongosh`.
>
> \- Field level encryption is only available in the `mongosh` binary,  
> and not available in the \[embedded Compass shell\](<https://www.mongodb.com/docs/compass/embedded-shell/>).

- - Method
  - Description
- - ClientEncryption.createEncryptedCollection()
  - Creates a collection with encrypted fields.
- - `ClientEncryption.decrypt()`
  - Decrypts the specified `encryptedValue` if the current database
    connection was configured with access to the Key Management Service
    (KMS) and key vault used to encrypt `encryptedValue`.
- - `ClientEncryption.encrypt()`
  - Encrypts the specified value using the specified `encryptionKeyId`
    and `encryptionAlgorithm`.
- - `getClientEncryption()`
  - Returns the `ClientEncryption` object for the current database
    collection.
- - `getKeyVault()`
  - Returns the `KeyVault` object for the current database connection.
- - `KeyVault.addKeyAlternateName()`
  - Adds the `keyAltName` to the `keyAltNames` array of the data
    encryption key with the specified UUID.
- - `KeyVault.createKey()`
  - Adds a data encryption key to the key vault associated to the
    database connection.
- - `KeyVault.deleteKey()`
  - Deletes a data encryption key with the specified UUID from the key
    vault associated to the database connection.
- - `KeyVault.getKey()`
  - Gets a data encryption key with the specified UUID. The data
    encryption key must exist in the key vault associated to the
    database connection.
- - `KeyVault.getKeyByAltName()`
  - Gets all data encryption keys with the specified `keyAltName`.
- - `KeyVault.getKeys()`
  - Returns all data encryption keys stored in the key vault associated
    to the database connection.

\* - `KeyVault.removeKeyAlternateName()`

> - Removes the specified `keyAltName` from the data encryption key
>   with the specified UUID. The data encryption key must exist in the
>   key vault associated to the database connection.

## Native Methods

- - Method
  - Description

- - <div id="mongosh-native-method-buildInfo()">

    `buildInfo()`

    </div>

  - 

- - <div id="mongosh-native-method-isInteractive">

    `isInteractive()`

    </div>

  - Returns a boolean indicating whether mongosh is running in
    interactive or script mode.

- - <div id="mongosh-native-method-load">

    `load()`

    </div>

  - Loads and runs a JavaScript file in the shell.

    In `mongosh`, scripts loaded with the `load()` method support
    the `__filename` and `__dirname` Node.js variables. These
    variables return the file name and directory of the loaded
    script, respectively. The returned values are always absolute
    paths.

- - <div id="mongosh-native-method-print">

    `print()`

    </div>

  - Print the specified text or variable. `print()` and `printjson()`
    are aliases for `console.log()`.

```code
> print("hello world")
hello world

> x = "example text"
> print(x)
example text

```

- - <div id="mongosh-native-method-quit">

    `quit()`

    </div>

  - Exits the current shell session.

- - <div id="mongosh-native-method-sleep">

    `sleep()`

    </div>

  - Suspends the `mongo` shell for a given period of time.

\* - .. <span id="mongosh-native-method-version">mongosh-native-method-version</span>:

> `version()`
>
> - Returns the current version of the `mongosh` instance.

## Query Plan Cache Methods

- - Method
  - Description
- - `db.collection.getPlanCache()`
  - Returns an interface to access the query plan cache object and
    associated `PlanCache` methods for a collection.
- - `PlanCache.clear()`
  - Removes all cached query plans for a collection.
- - `PlanCache.clearPlansByQuery()`
  - Clears the cached query plans for the specified
    query shape.
- - `PlanCache.help()`
  - Lists the methods available to view and modify a collection’s
    query plan cache.
- - `PlanCache.list()`
  - Returns an array of
    \[plan cache entries\](<https://www.mongodb.com/docs/manual/core/query-plans/>)
    for a collection.

## Replication Methods

- - Method
  - Description

- - `rs.add()`
  - Adds a member to the replica set. You must connect to the
    primary of the replica set to run this method.

- - `rs.addArb()`
  - Adds an arbiter to an existing replica set.

- - `rs.config()`
  - Returns a document that contains the current replica set configuration.

- - `rs.freeze()`
  - Makes the replica set member that `mongosh` is connected to
    ineligible to become primary for the specified duration. You
    must specify the duration in seconds.

- - `db.getReplicationInfo()`
  - Returns the status of the replica set from the oplog data.

- - `rs.initiate()`
  - Initializes a new replica set.

- - `db.printReplicationInfo()`
  - Returns the oplog of the replica set member that `mongosh` is
    connected to.

- - `rs.printReplicationInfo()`
  - Returns the oplog of the replica set member that `mongosh` is
    connected to.

- - `db.printSecondaryReplicationInfo`

  - Returns the status of the secondary members of the replica set.

    This is identical to the `rs.printSecondaryReplicationInfo()` method.

    The following is an example output from the
    `rs.printSecondaryReplicationInfo()` method issued on a replica set with two secondary members:

```sh
source: rs2.example.net:27017
{
  syncedTo: 'Tue Oct 13 2020  09:37:28 GMT-0700 (Pacific Daylight Time)',
  replLag: '0 secs (0 hrs) behind the primary '
}
---
source: rs3.example.net:27017
{
  syncedTo: 'Tue Oct 13 2020  09:37:28 GMT-0700 (Pacific Daylight Time)',
  replLag: '0 secs (0 hrs) behind the primary '
}

```

- - `rs.printSecondaryReplicationInfo`

  - Returns the status of the secondary members of the replica set.

    This is identical to the `db.printSecondaryReplicationInfo()`
    method.

    The following is an example output from the
    `rs.printSecondaryReplicationInfo()` method issued on a replica set with two secondary members:

```sh
source: rs2.example.net:27017
{
  syncedTo: 'Tue Oct 13 2020 09:42:18 GMT-0700 (Pacific Daylight Time)',
  replLag: '0 secs (0 hrs) behind the primary '
}
---
source: rs3.example.net:27017
{
  syncedTo: 'Tue Oct 13 2020 09:42:18 GMT-0700 (Pacific Daylight Time)',
  replLag: '0 secs (0 hrs) behind the primary '
}

```

- - \[rs.reconfig()\](<https://www.mongodb.com/docs/manual/reference/command/replSetReconfig/>)
  - Modifies the configuration of an existing replica set.
- - `rs.remove()`
  - Removes the member specified by hostname from the replica set.
- - `rs.status()`
  - Returns the status of the replica set member that `mongosh` is
    connected to.
- - `rs.stepDown()`
  - Makes the primary of the replica set a secondary. You must be
    connected to the primary to run this method.
- - `rs.syncFrom()`
  - Resets the sync target to the replica set member specified
    by hostname for the replica set member that `mongosh` is
    connected to.

## Role Management Methods

- - Method
  - Description
- - `db.createRole()`
  - Creates a role and specifies its privileges.
- - `db.dropRole()`
  - Deletes a user-defined role.
- - `db.dropAllRoles()`
  - Deletes all user-defined roles associated with a database.
- - `db.getRole()`
  - Returns information for the specified role.
- - `db.getRoles()`
  - Returns information for all the user-defined roles in a
    database.
- - `db.grantPrivilegesToRole()`
  - Assigns privileges to a user-defined role.
- - `db.revokePrivilegesFromRole()`
  - Removes the specified privileges from a user-defined role.
- - `db.grantRolesToRole()`
  - Specifies roles from which a user-defined role inherits
    privileges.
- - `db.revokeRolesFromRole()`
  - Removes inherited roles from a role.

\* - `db.updateRole()`

> - Updates a user-defined role.

## Session Object Methods

- - Method
  - Description
- - `Mongo.startSession()`
  - Starts a session for the connection.
- - \[Session.advanceOperationTime()\](<https://www.mongodb.com/docs/manual/reference/method/Session/>)
  - Updates the operation time.
- - \[Session.endSession()\](<https://www.mongodb.com/docs/manual/reference/method/Session/>)
  - Ends the session.
- - \[Session.getClusterTime()\](<https://www.mongodb.com/docs/manual/reference/method/Session/>)
  - Returns the most recent cluster time as seen by the session.
- - \[Session.getDatabase()\](<https://www.mongodb.com/docs/manual/reference/method/Session/>)
  - Access the specified database from the session in the shell.
- - \[Session.getOperationTime()\](<https://www.mongodb.com/docs/manual/reference/method/Session/>)
  - Returns the timestamp of the last acknowledged operation for the
    session.
- - \[Session.getOptions()\](<https://www.mongodb.com/docs/manual/reference/method/Session/>)
  - Returns the options for the session.
- - \[Session.hasEnded()\](<https://www.mongodb.com/docs/manual/reference/method/Session/>)
  - Returns a boolean that specifies whether the session has ended.
- - `SessionOptions()`
  - The options for a session in the shell. To access the
    `SessionOptions()` object, use `Session.getOptions()`.

## Server Status Methods

- - Method
  - Description

- - `db.hello()`

  - Returns a document that describes the role of the
    `mongod` instance.

    If the `mongod` is a member of a
    replica set, then the `isWritablePrimary` and
    `secondary` fields report if the instance is the
    primary or if it is a secondary member of the
    replica set.

- - `db.hostInfo()`
  - Returns a document with information about the system running
    the MongoDB instance.

- - `db.collection.latencyStats()`
  - Returns latency statistics for a specified collection.

- - `db.printCollectionStats()`
  - Returns statistics from every collection.

- - `db.serverBuildInfo()`
  - Returns a document that displays the compilation parameters for
    the `mongod` instance.

- - `db.serverCmdLineOpts()`
  - Returns a document with information about the runtime options
    used to start the MongoDB instance.

- - `db.serverStatus()`
  - Returns a document that provides an overview of the database
    process.

- - `db.stats()`
  - Returns a document that reports on the state of the current
    database.

- - `db.version()`
  - Returns the version of the `mongod` instance.

## Sharding Methods

- - Method
  - Description
- - `db.collection.getShardLocation()`
  - Returns a document containing the shards where the collection is
    located and whether the collection is sharded.
- - `sh.addShard()`
  - Adds a shard to a sharded cluster.
- - `sh.addShardTag()`
  - Aliases to `sh.addShardToZone()`.
- - `sh.addShardToZone()`
  - Associates a shard with a zone. Supports configuring zones in sharded
    clusters.
- - `sh.addTagRange()`
  - Aliases to `sh.updateZoneKeyRange()`.
- - `sh.balancerCollectionStatus()`
  - Returns information on whether the chunks of a sharded collection are balanced.

> **New in version 4.4**
>

- - `sh.disableAutoMerger()`
  - Disables automatic chunk merges for a namespace.

> **New in version 7.0**
>

- - `sh.disableAutoSplit()`
  - Disables auto-splitting for the sharded cluster.
- - `sh.disableBalancing()`
  - Disables balancing on a single collection in a sharded
    database. Does not affect balancing of other collections in a sharded cluster.
- - `sh.disableMigrations()`
  - Disables chunk migrations for a specific collection in a sharded
    cluster. Internally calls the `setAllowMigrations` command.
- - `sh.enableAutoMerger()`
  - Enables automatic chunk merges for a namespace.

> **New in version 7.0**
>

- - `sh.enableAutoSplit()`
  - Enables auto-splitting for the sharded cluster.
- - `sh.enableBalancing()`
  - Activates the sharded collection balancer process if
    previously disabled using `sh.disableBalancing()`.
- - `sh.enableMigrations()`
  - Enables chunk migrations for a specific collection in a sharded
    cluster that were previously disabled using `sh.disableMigrations()`.
    Internally calls the `setAllowMigrations` command.
- - `sh.enableSharding()`
  - Enables sharding on a specific database.
- - `sh.getBalancerState()`
  - Returns a boolean to report if the balancer is currently
    enabled.
- - `sh.getShardedDataDistribution()`
  - Returns data distribution information for sharded collections.
    `sh.getShardedDataDistribution()` is a shell helper method for
    the \[`\$shardedDataDistribution`\](<https://www.mongodb.com/docs/manual/reference/operator/aggregation/shardedDataDistribution/>) aggregation pipeline
    stage.
- - `sh.isBalancerRunning()`
  - Returns a boolean to report if the balancer process is
    currently migrating chunks.
- - sh.isConfigShardEnabled()
  - Returns whether a cluster has a config shard.
    If it does, `sh.isConfigShardEnabled()` also returns
    host and tag information.
- - sh.listShards()
  - Returns an array of documents describing the
    shards in a sharded cluster.
- - `sh.moveChunk()`
  - Migrates a chunk in a sharded cluster.
- - `sh.removeRangeFromZone()`
  - Removes the association between a range of shard key values and a
    zone.
- - `sh.removeShardFromZone`
  - Removes the association between a shard and a zone.
- - `sh.removeShardTag()`
  - Removes the association between a tag and a shard.
- - `sh.removeTagRange()`
  - Removes a range of shard key values to a shard tag created using
    the `sh.addShardTag()` method. This method aliases to
    `sh.removeRangeFromZone()` in MongoDB 3.4.
- - `sh.setBalancerState()`
  - Enables or disables the balancer which migrates
    chunks between shards.
- - `sh.shardCollection()`
  - Enables sharding for a collection.
- - `sh.splitAt()`
  - Divides an existing chunk into two chunks using a
    specific value of the shard key as the dividing point.
- - `sh.splitFind()`
  - Divides an existing chunk that contains a document
    matching a query into two approximately equal chunks.
- - sh.startAutoMerger()
  - Enables the AutoMerger.

> **New in version 7.0**
>

- - `sh.startBalancer()`
  - Enables the balancer.
- - `sh.status()`
  - Reports on the status of a sharded cluster.
- - sh.stopAutoMerger()
  - Disables the AutoMerger.

> **New in version 7.0**
>

- - `sh.stopBalancer()`
  - Disables the balancer. This operation does not wait for
    the balancer to complete any in progress operations, and may
    terminate ongoing operations.
- - `sh.updateZoneKeyRange()`
  - Associates a range of shard keys with a zone. Supports configuring
    zones in sharded clusters.

## Telemetry Methods

These methods configure whether `mongosh` tracks anonymous telemetry
data. Telemetry is enabled by default.

For more information on what data `mongosh` tracks with
telemetry, see telemetry.

- - Method
  - Description
- - `disableTelemetry()`
  - Disable telemetry for `mongosh`.
- - `enableTelemetry()`
  - Enable telemetry for `mongosh`.

## Transaction Methods

- - Method
  - Description
- - `Session.abortTransaction()`
  - Terminates a \[multi-document transaction\](<https://www.mongodb.com/docs/manual/core/transactions/>)
    and rolls back any data changes made by the operations within the
    transaction.
- - `Session.commitTransaction()`
  - Saves the changes made by the operations in a \[multi-document transaction\](<https://www.mongodb.com/docs/manual/core/transactions/>) and ends the transaction.
- - `Session.startTransaction()`
  - Starts a \[multi-document transaction\](<https://www.mongodb.com/docs/manual/core/transactions/>)
    associated with the session.

## User Management Methods

> **Important**
>
> The `passwordPrompt()` method is currently not
> supported in `mongosh`. As a result, when using the following
> methods you must specify the password as a parameter:
>
> - `db.auth()`
> - `db.changeUserPassword()`
> - `db.createUser()`
>
> \- `db.updateUser()`

- - Method
  - Description
- - `db.auth()`
  - Authenticates a user to a database.
- - `db.changeUserPassword()`
  - Changes an existing user’s password.
- - `db.createUser()`
  - Creates a new user.
- - `db.dropAllUsers()`
  - Deletes all users associated with a database.
- - `db.dropUser()`
  - Deletes a single user.
- - `db.getUser()`
  - Returns information about the specified user.
- - `db.getUsers()`
  - Returns information about all users associated with a database.
- - `db.updateUser()`
  - Updates a specified user's data.
- - `db.grantRolesToUser()`
  - Grants a role and its privileges to a user.
- - `db.revokeRolesFromUser()`
  - Removes a role from a user.
