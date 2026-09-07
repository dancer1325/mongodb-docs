# Hashed Sharding

Hashed sharding uses either a single field hashed index or a compound hashed index as the shard key to
partition data across your sharded cluster.

Sharding on a Single Field Hashed Index  
Hashed sharding provides a more even data distribution across the
sharded cluster at the cost of reducing
sharding-query-isolation. Post-hash, documents with "close"
shard key values are unlikely to be on the same chunk or shard - the
`mongos` is more likely to perform
sharding-mongos-broadcast to fulfill a given ranged query.
`mongos` can target queries with equality matches to a
single shard.

Hashed indexes compute the hash value of a single field as the index
value; this value is used as your shard key.[^1]

Sharding on a Compound Hashed Index  
MongoDB includes support for creating compound indexes with a single
hashed field. To create a compound hashed
index, specify `hashed` as the value of any single index key when
creating the index.

Compound hashed index compute the hash value of a single field in the
compound index; this value is used along with the other fields in the
index as your shard key.

Compound hashed sharding supports features like zone sharding, where the prefix (i.e. first) non-hashed field or
fields support zone ranges while the hashed field supports more even
distribution of the sharded data. Compound hashed sharding also
supports shard keys with a hashed prefix for resolving data
distribution issues related to monotonically increasing fields.

## Hashed Sharding Shard Key

The field you choose as your hashed shard key should have a good
cardinality, or large number of different values.
Hashed keys are ideal for shard keys with fields that change
monotonically like ObjectId values or
timestamps. A good example of this is the default `_id` field, assuming
it only contains ObjectId values.

To shard a collection using a hashed shard key, see
deploy-hashed-sharded-cluster-shard-collection.

## Hashed vs Ranged Sharding

Given a collection using a monotonically increasing value `X` as the
shard key, using ranged sharding results in a distribution of incoming
inserts similar to the following:

Since the value of `X` is always increasing, the chunk with an upper bound
of `MaxKey` receives the majority incoming writes. This
restricts insert operations to the single shard containing this chunk, which
reduces or removes the advantage of distributed writes in a sharded cluster.

By using a hashed index on `X`, the distribution of inserts is similar
to the following:

Since the data is now distributed more evenly, inserts are efficiently
distributed throughout the cluster.

## Shard the Collection

Use the `sh.shardCollection()` method, specifying the full namespace
of the collection and the target hashed index
to use as the shard key.

```javascript
sh.shardCollection( "database.collection", { <field> : "hashed" } )

```

To shard a collection on a
compound hashed index, specify
the full namespace of the collection and the target compound
hashed index to use as the shard key:

```javascript
sh.shardCollection(
  "database.collection",
  { "fieldA" : 1, "fieldB" : 1, "fieldC" : "hashed" }
)

```

> **Important**
>
> - Starting in MongoDB 5.0, you can reshard a collection by changing a collection's shard key.
>
> \- You can refine a shard key by adding a suffix  
> field or fields to the existing shard key.

### Shard a Populated Collection

If you shard a populated collection using a hashed shard key:

- The sharding operation creates an initial chunk to cover all of the
  shard key values.
- After the initial chunk creation, the balancer moves ranges of the
  initial chunk when it needs to balance data.

### Shard an Empty Collection

Sharding Empty Collection on Single Field Hashed Shard Key  
- With no zones and zone ranges specified
  for the empty or non-existing collection:
  - The sharding operation creates an empty chunk to cover the entire
    range of the shard key values. Starting in version 8.0, the
    operation creates 1 chunk per shard by default and migrates
    across the cluster. This initial creation and distribution of
    chunks allows for faster setup of sharding.
  - After the initial distribution, the balancer manages the chunk
    distribution going forward.
- With zones and zone ranges specified
  for the empty or a non-existing collection:
  - The sharding operation creates empty chunks for the defined zone
    ranges as well as any additional chunks to cover the entire range
    of the shard key values and performs an initial chunk distribution
    based on the zone ranges. This initial creation and distribution
    of chunks allows for faster setup of zoned sharding.
  - After the initial distribution, the balancer manages the chunk
    distribution going forward.

Sharding Empty Collection on Compound Hashed Shard Key with Hashed Field Prefix  
If the compound hashed shard key has the hashed field as the prefix
(the hashed field is the first field in the shard key):

- With no zones and zone ranges specified
  for the empty or non-existing collection:
  - The sharding operation creates empty chunks to cover the entire
    range of the shard key values and performs an initial chunk
    distribution. The value of all non-hashed fields is `MinKey` at
    each split point. Starting in version 8.0, the operation creates
    1 chunk per shard by default and migrates across the cluster. This
    initial creation and distribution of chunks allows for faster
    setup of sharding.
  - After the initial distribution, the balancer manages the chunk
    distribution going forward.
- With a *single* zone with a
  range from `MinKey` to `MaxKey` specified
  for the empty or a non-existing collection *and*
  the `presplitHashedZones` option specified to
  \`sh.shardCollection()\`:
  - The sharding operation creates empty chunks for the defined zone
    range as well as any additional chunks to cover the entire range
    of the shard key values and performs an initial chunk
    distribution based on the zone ranges. This initial creation and
    distribution of chunks allows for faster setup of zoned sharding.
  - After the initial distribution, the balancer manages the chunk
    distribution going forward.

Sharding Empty Collection on Compound Hashed Shard Key with Non-Hashed Prefix  
If the compound hashed shard key has one or more non-hashed fields as
the prefix (i.e. the hashed field is *not* the first field in the
shard key):

- With no zones and zone ranges specified
  for the empty or non-existing collection *and*
  preSplitHashedZones is `false` or
  omitted, MongoDB does not perform any initial chunk creation or
  distribution when sharding the collection.

- With no zones and zone ranges specified
  for the empty or non-existing collection *and*
  preSplitHashedZones,
  `sh.shardCollection()` / `shardCollection`
  returns an error.

- With zones and zone ranges specified
  for the empty or a non-existing collection *and* the
  preSplitHashedZones option specified to
  \`sh.shardCollection()\`:

  - The sharding operation creates empty chunks for the defined zone
    ranges as well as any additional chunks to cover the entire range
    of the shard key values.
  - The sharding operation further subdivides the initial chunk for
    each range, such that each shard in the zone is allocated an
    equal number of chunks.
  - This initial creation and distribution of chunks allows for
    faster setup of zoned sharding. After the initial distribution,
    the balancer manages the chunk distribution going forward.

  The defined ranges for each zone *must* meet certain requirements.
  For a description of the requirements and a complete example, see
  pre-define-zone-range-hashed-example.

### Drop a Hashed Shard Key Index

> **See also**
>
> To learn how to deploy a sharded cluster and implement hashed
> sharding, see sharding-procedure-setup.

[^1]: `mongosh` provides the `convertShardKeyToHashed()`
    method. This method uses the same hashing function as the hashed index and
    can be used to see what the hashed value would be for a key.
