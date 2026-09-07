# Refine a Shard Key

Refining a collection's shard key allows for a more fine-grained
data distribution and can address situations where the existing key
has led to jumbo chunks due to insufficient
cardinality.

> **Note**
>
> Starting in MongoDB 5.0, you can also reshard your collection by providing a new shard key for the
> collection. To learn whether you should reshard your collection
> or refine your shard key, see change-a-shard-key.

To refine a collection's shard key, use the
`refineCollectionShardKey` command. The
`refineCollectionShardKey` adds a suffix field
or fields to the existing key to create the new shard key.

For example, you may have an existing `orders` collection in a
`test` database with the shard key `{ customer_id: 1 }`. You can
use the `refineCollectionShardKey` command to change the
shard key to the new shard key `{ customer_id: 1, order_id: 1 }`:

```javascript
db.adminCommand( {
   refineCollectionShardKey: "test.orders",
   key: { customer_id: 1, order_id: 1 }
} )
```
