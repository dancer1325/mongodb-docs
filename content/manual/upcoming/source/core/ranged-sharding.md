# Ranged Sharding

Range-based sharding involves dividing data into contiguous ranges determined
by the shard key values. In this model, documents with "close" shard key
values are likely to be in the same chunk or shard. This
allows for efficient queries where reads target documents within a contiguous
range. However, both read and write performance may decrease with poor shard
key selection. See sharding-ranged-shard-key.

Range-based sharding is the default sharding methodology if no other options
such as those required for /core/hashed-sharding or
zones are configured.

## Shard Key Selection

Ranged sharding is most efficient when the shard key displays the following
traits:

- Large shard-key-range
- Low shard-key-frequency
- Non-shard-key-monotonic

The following image illustrates a sharded cluster using the field `X` as the
shard key. If the values for `X` have a large range, low frequency, and
change at a non-monotonic rate, the distribution of inserts may look similar
to the following:

## Shard a Collection

Use the `sh.shardCollection()` method, specifying the full namespace
of the collection and the target index or compound index
to use as the shard key.

```javascript
sh.shardCollection( "database.collection", { <shard key> } )

```

> **Important**
>
> - Starting in MongoDB 5.0, you can reshard a collection by changing a collection's shard key.
>
> \- You can refine a shard key by adding a suffix  
> field or fields to the existing shard key.

### Shard a Populated Collection

If you shard a populated collection, only one chunk is created
initially. The balancer then migrates ranges from that chunk if
necessary according to the configured range size.

### Shard an Empty Collection

If you shard an empty collection:

- With no zones and zone ranges specified for the
  empty or non-existing collection:
  - The sharding operation creates a single empty chunk to cover the
    entire range of the shard key values.
  - After the initial chunk creation, the balancer migrates the initial
    chunk across the shards as appropriate as well as manages the chunk
    distribution going forward.
- With zones and zone ranges specified for the
  empty or a non-existing collections:
  - The sharding operation creates empty chunks for the defined zone
    ranges as well as any additional chunks to cover the entire range
    of the shard key values and performs an initial chunk distribution
    based on the zone ranges. This initial creation and distribution of
    chunks allows for faster setup of zoned sharding.
  - After the initial distribution, the balancer manages the chunk
    distribution going forward.

> **See also**
>
> To learn how to deploy a sharded cluster and implement ranged
> sharding, see sharding-procedure-setup.
