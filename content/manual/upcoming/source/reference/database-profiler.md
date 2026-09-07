# Database Profiler Output

The database profiler captures data information about read and write
operations, cursor operations, and database commands. To configure the
database profile and set the thresholds for capturing profile data,
see the database-profiler section.

The database profiler writes data in the `system.profile \<\<database\>.system.profile\>` collection,
which is a capped collection. To view the profiler's output,
use normal MongoDB queries on the `system.profile \<\<database\>.system.profile\>` collection.

> **Note**
>
> Because the database profiler writes data to the
> `system.profile \<\<database\>.system.profile\>` collection in a
> database, the profiler profiles some write activity, even for
> databases that are otherwise read-only.

`currentOp` and the database profiler report the same
basic diagnostic information for CRUD operations, including the
following:

When using Queryable Encryption, CRUD
operations against encrypted collections are omitted from the
`system.profile \<\<database\>.system.profile\>` collection. For
details, see qe-redaction.

It is no longer possible to perform any operation, including reads, on the
`system.profile \<\<database\>.system.profile\>` collection from within a
transaction.

## Output Fields

For any single operation, the documents created by the database
profiler includes a subset of the following fields. The precise
selection of fields in these documents depends on the type of
operation.

> **Note**
>
> For the output specific to the version of your MongoDB, refer to
> the appropriate version of the MongoDB Manual.

system.profile.op
The type of operation. The possible values are:

- `command`
- `count`
- `distinct`
- `geoNear`
- `getMore`
- `group`
- `insert`
- `mapReduce`
- `query`
- `remove`

\- `update`
system.profile.ns
The namespace the operation targets. Namespaces in MongoDB
take the form of the database, followed by a dot (`.`),
followed by the name of
the collection.
system.profile.command

system.profile.originatingCommand
For `"getmore"` operations which retrieve the next batch of
results from a cursor, the `originatingCommand` field contains the
full command object (e.g. `find` or `aggregate`) which originally
created that cursor.
system.profile.cursorid
The ID of the cursor accessed by a `query` and `getmore`
operations.
system.profile.keysExamined
The number of index keys that MongoDB scanned in
order to carry out the operation.

In general, if `keysExamined` is much higher
than `nreturned`, the database is scanning many
index keys to find the result documents. Consider creating or
adjusting indexes to improve query performance..

`keysExamined` is available for the following commands and
operations:

system.profile.docsExamined
The number of documents in the collection that MongoDB scanned in
order to carry out the operation.

`docsExamined` is available for the following commands and
operations:

system.profile.inUseTrackedMemBytes

system.profile.peakTrackedMemBytes

system.profile.hasSortStage
`hasSortStage` is a boolean that is `true`
when a query **cannot** use the ordering in the index to return the
requested sorted results; i.e. MongoDB must sort the documents after
it receives the documents from a cursor. The field only appears when
the value is `true`.

`hasSortStage` is available for the following commands and
operations:

- `find` (OP_QUERY and
  `command`)
- `getMore` (OP_GET_MORE and
  `command`)
- `findAndModify`
- `mapReduce`

\- `aggregate`
system.profile.usedDisk
A boolean that indicates whether any aggregation stage wrote data to
temporary files due to memory restrictions.

Only appears if `usedDisk` is true.
system.profile.ndeleted
The number of documents deleted by the operation.
system.profile.ninserted
The number of documents inserted by the operation.
system.profile.nMatched
The number of documents that match the query
condition for the update operation.
system.profile.nModified
The number of documents modified by the update operation.
system.profile.upsert
A boolean that indicates the update operation's `upsert` option
value. Only appears if `upsert` is true.
system.profile.fromMultiPlanner
A boolean that indicates whether the query planner evaluated multiple
plans before choosing the winning execution plan for the query.

Only present when value is `true`.
system.profile.replanned
A boolean that indicates whether the query system evicted a
cached plan and re-evaluated
all candidate plans.

Only present when value is `true`.
system.profile.replanReason
A string that indicates the specific reason a
cached plan was evicted.

Only present when value for `replanned` is `true`.
system.profile.keysInserted
The number of index keys inserted for a given write operation.
system.profile.writeConflicts
The number of conflicts encountered during the write operation; e.g.
an `update` operation attempts to modify the same document as
another `update` operation. See also write conflict.
system.profile.numYield
The number of times the operation yielded to allow other operations
to complete. Typically, operations yield when they need access to
data that MongoDB has not yet fully read into memory. This allows
other operations that have data in memory to complete while MongoDB
reads in data for the yielding operation. For more information,
see the FAQ on when operations yield.
system.profile.queryHash

system.profile.queryShapeHash

system.profile.planCacheShapeHash
A hexadecimal string that represents a hash of the plan cache query shape and is dependent only on the plan cache query shape.
`planCacheShapeHash` can help identify slow queries, including the
query filter of write operations, with the same plan cache query
shape.

> **Note**
>
> As with any hash function, two different plan cache query shapes may result
> in the same hash value. However, the occurrence of hash
> collisions between different plan cache query shapes is unlikely.

For more information about `planCacheShapeHash` and
`planCacheKey`, see query-hash-plan-cache-key.

system.profile.planCacheKey
A hash of the key for the plan cache entry associated with the query.

Unlike `planCacheShapeHash`,
`planCacheKey` is a function of both the plan
cache query shape and the currently available indexes for that shape.
Specifically, if indexes that can support the query shape are
added or dropped, the `planCacheKey` value may change but
`planCacheShapeHash` would not.

For more information about `planCacheShapeHash` and
`planCacheKey`, see query-hash-plan-cache-key.

system.profile.queryFramework
The query framework used to process an operation.
system.profile.locks
The `system.profile.locks` provides information for various
lock types and lock modes held
during the operation.

The possible lock types are:

The possible locking modes for the lock types are as follows:

The returned lock information for the various lock types include:

system.profile.locks.acquireCount
Number of times the operation acquired the lock in the specified
mode.
system.profile.locks.acquireWaitCount
Number of times the operation had to wait for the
`acquireCount` lock acquisitions
because the locks were held in a conflicting mode.
`acquireWaitCount` is less than or
equal to `acquireCount`.
system.profile.locks.timeAcquiringMicros
Cumulative time in microseconds that the operation had to wait to
acquire the locks.

`timeAcquiringMicros` divided by
`acquireWaitCount` gives an
approximate average wait time for the particular lock mode.
system.profile.locks.deadlockCount
Number of times the operation encountered deadlocks while waiting
for lock acquisitions.
For more information on lock modes, see
faq-concurrency-locking.
system.profile.authorization

> **New in version 5.0.0**
>

The number of times the user cache is accessed for each operation.
These metrics are only displayed when an operation has accessed the
user cache at least once.

These metrics are only available when slow operation logging or database profiling is
enabled.

`system.profile.authorization` is not included in
`db.currentOp()` output.

system.profile.authorization.startedUserCacheAcquisitionAttempts
The number of times the operation tried to access the user cache.
system.profile.authorization.completedUserCacheAcquisitionAttempts
The number of times the operation retrieved user data from the
user cache.
system.profile.authorization.userCacheWaitTimeMicros
The total time the operation spent waiting for user cache
responses.
system.profile.storage
The `system.profile.storage` information provides metrics on
the storage engine data and wait time for the operation.

Specific storage metrics are returned only if the values are greater
than zero.

system.profile.storage.data.bytesRead
Number of bytes read by the operation from the disk to the cache.

Data read from disk into the cache includes everything needed
to execute the query. If the data is already in the cache,
then the number of bytes read from disk could be `0`.

The number of bytes read from disk includes more than the
queried documents:

- WiredTiger reads in units of pages and a page may contain one or
  several documents. If a document is in a page, all documents in that
  page are read into the cache and included in the `bytesRead` value.
- If the cache requires page management (such as eviction or rereads),
  the `bytesRead` value includes data read from disk
  in these operations.

\* If the index is not in the cache or the index in the cache is stale,  
WiredTiger reads several internal and leaf pages from disk to
reconstruct the index in cache.

system.profile.storage.data.timeReadingMicros
Time in microseconds that the operation had to spend to read from
the disk.
system.profile.storage.data.bytesWritten
Number of bytes written by the operation from the cache to the
disk.

WiredTiger typically doesn't require the query to write to disk.
Data modified by the query is written to an in-memory cache that
WiredTiger flushes to disk as part an eviction or checkpoint
operation. In such cases, `bytesWritten` shows as 0.

If the thread running the query requires forced page management
(such as eviction), WiredTiger writes the page contents to disk.
This flush likely includes data unrelated to changes made by the
query itself, which can cause `bytesWritten` to show
a higher value than expected.
system.profile.storage.data.timeWritingMicros
Time in microseconds that the operation had to spend to write to
the disk.
system.profile.storage.timeWaitingMicros.cache
Time in microseconds that the operation had to wait for space in
the cache.
system.profile.storage.timeWaitingMicros.schemaLock
Time in microseconds that the operation (if modifying the schema)
had to wait to acquire a schema lock.
system.profile.storage.timeWaitingMicros.handleLock
Time in microseconds that the operation had to wait to acquire
the a lock on the needed data handles.
system.profile.nreturned
The number of documents returned by the operation.
system.profile.responseLength
The length in bytes of the operation's result document. A large
`responseLength` can affect performance.
To limit the size of the
result document for a query operation, you can use any of the
following:

- Projections
- `The limit() method`
- `The batchSize() method`

> **Note When MongoDB writes query profile information to the log,**
>
> the `responseLength` value is in a field
> named `reslen`.

system.profile.cpuNanos

> **New in version 6.3**
>

The total CPU time spent by a query operation in nanoseconds. This field is
only available on Linux systems.
system.profile.protocol
The mongodb-wire-protocol request message format.
system.profile.millis
The time in milliseconds from the perspective of the
`mongod` from the beginning of the operation to the end of
the operation.
planningTimeMicros

> **New in version 6.2**
>

The time, in microseconds, that the `find` or `aggregate` command
spent in query planning.
system.profile.planSummary
A summary of the execution plan.
system.profile.execStats
A document that contains the execution statistics of the query
operation. For other operations, the value is an empty document.

The `system.profile.execStats` presents the statistics as a
tree; each node provides the statistics for the operation executed
during that stage of the query operation.

> **Note**
>
> The following fields list for `execStats`
> is not meant to be exhaustive as the returned fields vary per
> stage.

system.profile.execStats.stage
The descriptive name for the operation performed as part of the
query execution; e.g.

- `COLLSCAN` for a collection scan
- `IXSCAN` for scanning index keys

\- `FETCH` for retrieving documents
system.profile.execStats.inputStages
An array that contains statistics for the operations that are the
input stages of the current stage.
system.profile.ts
The timestamp of the operation.
system.profile.client
The IP address or hostname of the client connection where the
operation originates.
system.profile.appName

system.profile.allUsers
An array of authenticated user information (user name and database)
for the session. See also users.
system.profile.user
The authenticated user who ran the operation. If the operation was
not run by an authenticated user, this field's value is an empty
string.
Example `system.profile` Document
-----------------------------------

The following examples present sample documents found in the
`system.profile \<\<database\>.system.profile\>` collection for
operations on a standalone:

tabs:

> - id: query
>   name: Find Operation
>   content: \|
>   The following document in the `system.profile \<\<database\>.system.profile\>` collection shows metrics for
>   a sample query operation on the `test.report` collection:

```javascript
{
   "op" : "query",
   "ns" : "test.report",
   "command" : {
      "find" : "report",
      "filter" : { "a" : { "$lte" : 500 } },
      "lsid" : {
         "id" : UUID("5ccd5b81-b023-41f3-8959-bf99ed696ce9")
      },
      "$db" : "test"
   },
   "cursorid" : 33629063128,
   "keysExamined" : 101,
   "docsExamined" : 101,
   "inUseTrackedMemBytes" : Long(368),
   "peakTrackedMemBytes" : Long(795),
   "fromMultiPlanner" : true,
   "numYield" : 2,
   "nreturned" : 101,
   "planCacheShapeHash" : "811451DD",
   "planCacheKey" : "759981BA",
   "queryFramework" : "classic",
   "locks" : {
      "Global" : {
         "acquireCount" : {
            "r" : Long(3),
            "w" : Long(3)
         }
      },
      "Database" : {
         "acquireCount" : { "r" : Long(3) },
         "acquireWaitCount" : { "r" : Long(1) },
         "timeAcquiringMicros" : { "r" : Long(69130694) }
      },
      "Collection" : {
         "acquireCount" : { "r" : Long(3) }
      }
   },
   "storage" : {
      "data" : {
         "bytesRead" : Long(14736),
         "timeReadingMicros" : Long(17)
      }
   },
   "responseLength" : 1305014,
   "protocol" : "op_msg",
   "millis" : 69132,
   "planningTimeMicros" : 129,
   "planSummary" : "IXSCAN { a: 1, _id: -1 }",
   "execStats" : {
      "stage" : "FETCH",
      "nReturned" : 101,
      "executionTimeMillisEstimate" : 0,
      "works" : 101,
      "advanced" : 101,
      "needTime" : 0,
      "needYield" : 0,
      "saveState" : 3,
      "restoreState" : 2,
      "isEOF" : 0,
      "docsExamined" : 101,
      "alreadyHasObj" : 0,
      "inputStage" : {
         ...
      }
   },
   "ts" : ISODate("2019-01-14T16:57:33.450Z"),
   "client" : "127.0.0.1",
   "appName" : "MongoDB Shell",
   "allUsers" : [
      {
         "user" : "someuser",
         "db" : "admin"
      }
   ],
   "user" : "someuser@admin"
}

```

> - id: getmore
>   name: Get More Operation
>   content: \|
>   The `system.profile \<\<database\>.system.profile\>` collection
>   includes metrics for the `getMore` operation. In this
>   example, `getMore` returns additional documents from the
>   `test.report` collection.

```javascript
{
   "op" : "getmore",
   "ns" : "test.report",
   "command" : {
      "getMore" : Long("33629063128"),
      "collection" : "report",
      "batchSize": 3,
      "lsid" : {
         "id": new UUID("3148c569-425c-4498-9168-5b7ee260bf27")
      },
      "$db" : "test"
   },
   originatingCommand: {
      "find: "report",
      "filter" : { "a" : { "$lte" : 500 } },
      "lsid" : {
         "id" : UUID("5ccd5b81-b023-41f3-8959-bf99ed696ce9")
      },
      "$db" : "test"
   }, 
   "cursorid" : Long("33629063128"),
   "keysExamined" : 101,
   "docsExamined" : 101,
   "inUseTrackedMemBytes" : Long(368),
   "peakTrackedMemBytes" : Long(795),
   "fromMultiPlanner" : true,
   "numYield" : 2,
   "nreturned" : 3,
   "planCacheShapeHash" : "811451DD",
   "planCacheKey" : "759981BA",
   "queryFramework": "classic"
   "locks" : {
      "Global" : {
         "acquireCount" : {
            "r" : Long(3),
            "w" : Long(3)
         }
      },
      "Database" : {
         "acquireCount" : { "r" : Long(3) },
         "acquireWaitCount" : { "r" : Long(1) },
         "timeAcquiringMicros" : { "r" : Long(69130694) }
      },
      "Collection" : {
         "acquireCount" : { "r" : Long(3) }
      }
   },
   readConcern: {level: 'local', provenance: 'implicitDefault'},
   "responseLength" : 1305014,
   "protocol" : "op_msg",
   "millis" : 69132,
   "planSummary" : "IXSCAN { a: 1, _id: -1 }",
   "execStats" : {
      "stage" : "FETCH",
      "filter" : { "a" : { "$lte" : 500 } },
      "nReturned" : 101,
      "executionTimeMillisEstimate" : 0,
      "works" : 104,
      "advanced" : 104,
      "needTime" : 0,
      "needYield" : 0,
      "saveState" : 3,
      "restoreState" : 2,
      "isEOF" : 0,
      "direction": 'forward',
      "docsExamined" : 104
   },
   "ts" : ISODate("2019-01-14T16:57:33.450Z"),
   "client" : "127.0.0.1",
   "appName" : "MongoDB Shell",
   "allUsers" : [
      {
         "user" : "someuser",
         "db" : "admin"
      }
   ],
   "user" : "someuser@admin"
}

```

> - id: update
>   name: Update Operation
>   content: \|
>
>   > The profile entry for `update` (and
>   > `delete`) operation contains the entire update
>   > command.
>   >
>   > The following document in the `system.profile \<\<database\>.system.profile\>` collection shows metrics for
>   > a sample update operation on the `test.report` collection:

```javascript
{
   "op" : "update",
   "ns" : "test.report",
   "command" : {
      "q" : { },
      "u" : { "$rename" : { "a" : "b" } },
      "multi" : true,
      "upsert" : false
   },
   "keysExamined" : 0,
   "docsExamined" : 25000,
   "nMatched" : 25000,
   "nModified" : 25000,
   "keysInserted" : 25000,
   "keysDeleted" : 25000000,
   "numYield" : 6985,
   "locks" : {
      "Global" : {
         "acquireCount" : { "r" : Long(6986), "w" : Long(13972) }
      },
      "Database" : {
         "acquireCount" : { "w" : Long(6986) },
         "acquireWaitCount" : { "w" : Long(1) },
         "timeAcquiringMicros" : { "w" : Long(60899375) }
      },
      "Collection" : {
         "acquireCount" : { "w" : Long(6986) }
      },
      "Mutex" : {
         "acquireCount" : { "r" : Long(25000) }
      }
   },
   "storage" : {
      "data" : {
         "bytesRead" : Long(126344060),
         "bytesWritten" : Long(281834762),
         "timeReadingMicros" : Long(94549),
         "timeWritingMicros" : Long(139361)
      }
   },
   "millis" : 164687,
   "planningTimeMicros" : 129,
   "planSummary" : "COLLSCAN",
   "execStats" : {
      "stage" : "UPDATE",
      "nReturned" : 0,
      "executionTimeMillisEstimate" : 103771,
      "works" : 25003,
      "advanced" : 0,
      "needTime" : 25002,
      "needYield" : 0,
      "saveState" : 6985,
      "restoreState" : 6985,
      "isEOF" : 1,
      "nMatched" : 25000,
      "nWouldModify" : 25000,
      "wouldInsert" : false,
      "inputStage" : {
         "stage" : "COLLSCAN",
         "nReturned" : 25000,
         "executionTimeMillisEstimate" : 0,
         "works" : 25002,
         "advanced" : 25000,
         "needTime" : 1,
         "needYield" : 0,
         "saveState" : 31985,
         "restoreState" : 31985,
         "isEOF" : 1,
         "direction" : "forward",
         "docsExamined" : 25000
      }
   },
   "ts" : ISODate("2019-01-14T23:33:01.806Z"),
   "client" : "127.0.0.1",
   "appName" : "MongoDB Shell",
   "allUsers" : [
      {
         "user" : "someuser",
         "db" : "admin"
      }
   ],
   "user" : "someuser@admin"
}
```
