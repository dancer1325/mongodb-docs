# Choose a Shard Key

When you choose your shard key, consider:

- the cardinality of the shard key
- the frequency with which shard key values
  occur
- whether a potential shard key grows monotonically
- sharding-query-patterns
- limits-shard-keys

> **Note**
>
> - Starting in MongoDB 5.0, you can change your shard key and redistribute your data using the
>   `reshardCollection` command.
> - You can use the `refineCollectionShardKey` command to refine a
>   collection's shard key. The `refineCollectionShardKey`
>   command adds a suffix field or fields to the existing key to create
>   the new shard key.
>
> \- You can update a document's shard key value unless the shard key field is  
> the immutable `_id` field.

> **Important**
>
> If you regularly change a document's shard key value so that the
> value is in a shard key range owned by a different shard, it may
> impact cluster performance due to the additional resources involved
> in migrating the document between shards. For details, see
> sharding-data-partitioning and db.collection.updateOne().

## Shard Key Cardinality

The cardinality of a shard key determines the maximum number of
chunks the balancer can create. Where possible, choose a shard key with
high cardinality. A shard key with low cardinality reduces the
effectiveness of horizontal scaling in the cluster.

Each unique shard key value can exist on no more than a single chunk at
any given time. Consider a dataset that contains user data with a
`continent` field. If you chose to shard on `continent`, the shard
key would have a cardinality of `7`. A cardinality of `7` means
there can be no more than `7` chunks within the sharded cluster, each
storing one unique shard key value. This constrains the number of
effective shards in the cluster to `7` as well - adding more than
seven shards would not provide any benefit.

The following image illustrates a sharded cluster using the field `X`
as the shard key. If `X` has low cardinality, the distribution of
inserts may look similar to the following:

If your data model requires sharding on a key that has low cardinality,
consider using an indexed compound of fields to
increase cardinality.

A shard key with high cardinality does not, on its own, guarantee even
distribution of data across the sharded cluster. The frequency of the shard key and the potential for
monotonically changing shard key values
also contribute to the distribution of the data.

## Shard Key Frequency

The `frequency` of the shard key represents how often a given shard
key value occurs in the data. If the majority of documents contain only
a subset of the possible shard key values, then the chunks storing the
documents with those values can become a bottleneck within the cluster.
Furthermore, as those chunks grow, they may become indivisible chunks as they cannot be split any further. This reduces
the effectiveness of horizontal scaling within the cluster.

The following image illustrates a sharded cluster using the field `X` as the
shard key. If a subset of values for `X` occur with high frequency, the
distribution of inserts may look similar to the following:

If your data model requires sharding on a key that has high frequency
values, consider using a compound index using a unique or
low frequency value.

A shard key with low frequency does not, on its own, guarantee even
distribution of data across the sharded cluster. The cardinality of the shard key and the potential for
monotonically changing shard key values
also contribute to the distribution of the data.

## Monotonically Changing Shard Keys

A shard key on a value that increases or decreases monotonically is more
likely to distribute inserts to a single chunk within the cluster.

This occurs because every cluster has a chunk that captures a range with
an upper bound of `MaxKey`. `maxKey`
always compares as higher than all other values. Similarly, there is a
chunk that captures a range with a lower bound of
`MinKey`. `minKey` always compares as
lower than all other values.

If the shard key value is always increasing, all new inserts are routed
to the chunk with `maxKey` as the upper bound. If the shard key value
is always decreasing, all new inserts are routed to the chunk with
`minKey` as the lower bound. The shard containing that chunk becomes
the bottleneck for write operations.

To optimize data distribution, the chunks that contain the global
`maxKey` (or `minKey`) do not stay on the same shard. When a chunk
is split, the new chunk with the `maxKey` (or `minKey`) chunk is
located on a different shard.

The following image illustrates a sharded cluster using the field `X`
as the shard key. If the values for `X` are monotonically increasing, the
distribution of inserts may look similar to the following:

If the shard key value was monotonically decreasing, then all inserts
would route to `Chunk A` instead.

If your data model requires sharding on a key that changes
monotonically, consider using /core/hashed-sharding.

A shard key that does not change monotonically does not, on its own,
guarantee even distribution of data across the sharded cluster. The
cardinality and
frequency of the shard key also contribute
to the distribution of the data.

## Sharding Query Patterns

The ideal shard key distributes data evenly across the sharded cluster
while also facilitating common query patterns. When you choose a shard
key, consider your most common query patterns and whether a given shard
key covers them.

In a sharded cluster, the `mongos` routes queries to only
the shards that contain the relevant data if the queries contain the
shard key. When the queries do not contain the shard key, the queries
are broadcast to all shards for evaluation. These types of queries are
called scatter-gather queries. Queries that involve multiple shards for
each request are less efficient and do not scale linearly when more
shards are added to the cluster.

This does not apply for aggregation queries that operate on a large
amount of data. In these cases, scatter-gather can be a useful approach
that allows the query to run in parallel on all shards.

## Use Shard Key Analyzer in 7.0 to Find Your Shard Key
