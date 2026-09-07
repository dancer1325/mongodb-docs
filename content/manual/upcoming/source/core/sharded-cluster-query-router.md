# Routing with mongos

MongoDB `mongos` instances route queries and write operations
to shards in a sharded cluster. `mongos` provides the
only interface to a sharded cluster from the perspective of
applications. Applications never connect or communicate directly with
the shards.

The `mongos` tracks what data is on which shard by caching
the metadata from the config servers. The `mongos` uses the
metadata to route operations from applications and clients to the
`mongod` instances. A `mongos` has no *persistent*
state and consumes minimal system resources.

The most common practice is to run `mongos` instances on the
same systems as your application servers, but you can maintain
`mongos` instances on the shards or on other dedicated
resources. See also sharded-cluster-components-distribution.

## Routing And Results Process

A `mongos` instance routes a query to a cluster by:

1.  Determining the list of shards that must receive the
    query.
2.  Establishing a cursor on all targeted shards.

The `mongos` then merges the data from each of the
targeted shards and returns the result document. Certain
query modifiers, such as sorting,
are performed on each shard before `mongos`
retrieves the results.

Aggregation operations running on multiple
shards may route results back to the `mongos` to merge results if they don't need to run on the database's primary shard.

There are two cases in which a pipeline is ineligible to run on
`mongos`.

The first case occurs when the merge part of the split pipeline
contains a stage which *must* run on a specific shard. For instance,
if `$lookup` requires access to an unsharded collection in the same
database as the sharded collection on which the aggregation is running,
the merge runs on the shard that hosts the unsharded collection.

The second case occurs when the merge part of the split pipeline
contains a stage which may write temporary data to disk, such as
`$group`, and the client has specified `allowDiskUse:true`. In this
case, assuming that there are no other stages in the merge pipeline
which require the primary shard, the merge runs on a
randomly-selected shard in the set of shards targeted by the aggregation.

For more information on how the work of aggregation is split among
components of a sharded cluster query, use `explain:true` as a
parameter to the `aggregate()` call. The
return includes three JSON objects:

- `mergeType` shows where the
  stage of the merge happens ("anyShard", "specificShard",
  or "router"). When `mergeType` is `specificShard`, the aggregate
  output includes a `mergeShard` property that contains the shard ID of the
  merging shard.
- `splitPipeline` shows which operations in your pipeline have
  run on individual shards.
- `shards` shows the work each shard has done.

In some cases, when the shard key or a prefix of the shard key
is a part of the query, the `mongos` performs a
targeted operation, routing queries to
a subset of shards in the cluster.

`mongos` performs a broadcast operation for queries that do *not* include the
shard key, routing queries to *all* shards in the cluster. Some
queries that do include the shard key may still result in a broadcast
operation depending on the distribution of data in the cluster and the
selectivity of the query.

See sharding-query-isolation for more on targeted and
broadcast operations.

## How `mongos` Handles Query Modifiers

### Sorting

If the result of the query is not sorted, the `mongos`
instance opens a result cursor that "round robins" results from all
cursors on the shards.

### Limits

If the query limits the size of the result set using the
`limit()` cursor method, the `mongos`
instance passes that limit to the shards and then re-applies the limit
to the result before returning the result to the client.

### Skips

If the query specifies a number of records to *skip* using the
`skip()` cursor method, the `mongos` *cannot*
pass the skip to the shards, but rather retrieves unskipped results
from the shards and skips the appropriate number of documents when assembling
the complete result.

When used in conjunction with a `limit()`, the
`mongos` passes the *limit* plus the value of the
`skip()` to the shards to improve the efficiency of these
operations.

## Read Preference and Shards

For sharded clusters, `mongos` applies the read preference when reading from the shards. The
member selected is governed by both the read preference and
`replication.localPingThresholdMs` settings, and is
re-evaluated for each operation.

For details on read preference and sharded clusters, see
read-preference-mechanics-sharded-cluster.

## Confirm Connection to `mongos` Instances

To detect if the MongoDB instance that your client is connected
to is `mongos`, use the `hello` command. When a
client connects to a `mongos`, `hello` returns
a document with a `msg` field that holds the string
`isdbgrid`. For example:

```javascript
{
   "isWritablePrimary" : true,
   "msg" : "isdbgrid",
   "maxBsonObjectSize" : 16777216,
   "ok" : 1,
   ...
}

```

If the application is instead connected to a `mongod`, the
returned document does not include the `isdbgrid` string.

## Targeted Operations vs. Broadcast Operations

Generally, the fastest queries in a sharded environment are those that
`mongos` route to a single shard, using the shard key and the
cluster meta data from the config server.
These targeted operations use the
shard key value to locate the shard or subset of shards that satisfy the
query document.

For queries that don't include the shard key, `mongos` must query all
shards, wait for their responses and then return the result to the
application. These "scatter/gather" queries can be long running operations.

### Broadcast Operations

`mongos` instances broadcast queries to all shards for the
collection **unless** the `mongos` can
determine which shard or subset of shards stores this data.

After the `mongos` receives responses from all shards, it merges
the data and returns the result document. The performance of a broadcast
operation depends on the overall load of the cluster, as well as variables
like network latency, individual shard load, and number of documents returned
per shard. Whenever possible, favor operations that result in targeted operation over those that result in a broadcast
operation.

Multi-update operations are always broadcast operations.

The `updateMany()` and
`deleteMany()` methods are broadcast
operations, unless the query document specifies the shard key in full.

### Targeted Operations

`mongos` can route queries that include the shard key or the prefix
of a compound shard key a specific shard or set of
shards. `mongos` uses the shard key value to locate the
chunk whose range includes the shard key value and directs the
query at the shard containing that chunk.

For example, if the shard key is:

```javascript
{ a: 1, b: 1, c: 1 }

```

The `mongos` program *can* route queries that include the full
shard key or either of the following shard key prefixes at a
specific shard or set of shards:

```javascript
{ a: 1 }
{ a: 1, b: 1 }

```

All `insertOne()` operations target to one shard. Each
document in the `insertMany()` array targets to a
single shard, but there is no guarantee all documents in the array insert into
a single shard.

All `updateOne()`,
`replaceOne()` and `deleteOne()`
operations *must* include the shard key or `_id` in the query
document. MongoDB returns an error if these methods are used without
the shard key or `_id`.

Depending on the distribution of data in the cluster and the selectivity of
the query, `mongos` may still perform a broadcast operation to fulfill these queries.

### Index Use

When a shard receives a query, it uses the most efficient index
available to fulfill that query. The index used may be either the
shard key index or another eligible
index present on the shard.

## Sharded Cluster Security

Use /core/security-internal-authentication to enforce intra-cluster
security and prevent unauthorized cluster components from accessing the
cluster. You must start each `mongod` or `mongos` in the
cluster with the appropriate security settings in order to enforce internal
authentication.

See /tutorial/deploy-sharded-cluster-with-keyfile-access-control for a
tutorial on deploying a secured sharded cluster.

### Cluster Users

Sharded clusters support /core/authorization *(RBAC)* for restricting
unauthorized access to cluster data and operations. You must start each
`mongod` in the cluster, including the config servers, with the `--auth` option in order to enforce RBAC.
Alternatively, enforcing /core/security-internal-authentication for
inter-cluster security also enables user access controls via RBAC.

With RBAC enforced, clients must specify a `--username`,
`--password`, and
`--authenticationDatabase` when
connecting to the `mongos` in order to access cluster resources.

Each cluster has its own cluster users. These users cannot be used
to access individual shards.

See /tutorial/enable-authentication for a tutorial on enabling
adding users to an RBAC-enabled MongoDB deployment.

## Metadata Operations

## Additional Information

### FCV Compatibility

### Full Time Diagnostic Data Capture Requirements

### Connection Pools

### Using Aggregation Pipelines with Clusters

For more information on how sharding works with aggregations, read the sharding chapter in the [Practical
MongoDB Aggregations](https://www.practical-mongodb-aggregations.com/guides/sharding.html)
e-book.
