# Bulk Write Operations

## Overview

MongoDB provides clients the ability to perform write operations in
bulk. Starting in MongoDB 8.0, you can perform bulk write operations across
multiple databases and collections. If you are using a version
earlier than MongoDB 8.0, you can perform bulk write operations on a single
collection.

To perform bulk write operations across multiple databases and
collections in MongoDB 8.0, use the `bulkWrite`
database command or the `Mongo.bulkWrite()` `mongosh` method.

To perform bulk write operations on a single collection,
use the `db.collection.bulkWrite()` `mongosh` method. If you are
running MongoDB 8.0 or later, you can also use `bulkWrite` or
`Mongo.bulkWrite()` to write to a single collection.

## Ordered vs Unordered Operations

You can set your bulk write operations to be either *ordered* or *unordered*.

With an ordered list of operations, MongoDB executes the operations serially.
If an error occurs during the processing of one of the write
operations, MongoDB returns without processing any remaining write
operations in the list.

With an unordered list of operations, MongoDB can execute the
operations in parallel, but this behavior is not guaranteed.
If an error occurs during the processing of one
of the write operations, MongoDB will continue to process remaining
write operations in the list.

Executing an ordered list of operations on a sharded collection will
generally be slower than executing an unordered list since with an
ordered list, each operation must wait for the previous operation to
finish.

By default, all bulk write commands and methods perform ordered operations.
To specify unordered operations, set the `ordered` option to `false` when you
call your preferred command or method. To learn more about the syntax of
each command or method, see their pages linked above.

## Bulk Write Methods

All bulk write methods and commands support the following write operations:

- Insert One
- Update One
- Update Many
- Replace One
- Delete One
- Delete Many

When you call your preferred command or method, you pass each write
operation as a document in an array. To learn more about the syntax of
each command or method, see their pages linked above.

## Example

### `db.collection.bulkWrite()`

For more examples, see db.collection.bulkWrite() Examples.

### `Mongo.bulkWrite()`

## Strategies for Bulk Inserts to a Sharded Collection

Large bulk insert operations, including initial data inserts or routine
data import, can affect sharded cluster performance. For
bulk inserts, consider the following strategies:

### Pre-Split the Collection

If your sharded collection is empty and you are not using hashed
sharding for the first key of your shard key, then your collection has
only one initial chunk, which resides on a single shard. MongoDB
must then take time to receive data and distribute chunks to
the available shards. To avoid this performance cost, pre-split the
collection by creating ranges in a sharded cluster.

### Unordered Writes to `mongos`

To improve write performance to sharded clusters, perform an unordered
bulk write by setting `ordered` to `false` when you call your
preferred method or command. `mongos` can attempt to send the writes to
multiple shards simultaneously. For *empty* collections,
first pre-split the collection as described in
/tutorial/split-chunks-in-sharded-cluster.

### Avoid Monotonic Throttling

If your shard key increases monotonically during an insert, then all
inserted data goes to the last chunk in the collection, which will
always end up on a single shard. Therefore, the insert capacity of the
cluster will never exceed the insert capacity of that single shard.

If your insert volume is larger than what a single shard can process,
and if you cannot avoid a monotonically increasing shard key, then
consider the following modifications to your application:

- Reverse the binary bits of the shard key. This preserves the
  information and avoids correlating insertion order with increasing
  sequence of values.
- Swap the first and last 16-bit words to "shuffle" the inserts.

> **Example The following example, in C++, swaps the leading and**
>
> trailing 16-bit word of BSON ObjectIds
> generated so they are no longer monotonically increasing.
>
> ```cpp
using namespace mongo;
OID make_an_id() {
  OID x = OID::gen();
  const unsigned char *p = x.getData();
  swap( (unsigned short&) p[0], (unsigned short&) p[10] );
  return x;
}

void foo() {
  // create an object
  BSONObj o = BSON( "_id" << make_an_id() << "x" << 3 << "name" << "jane" );
  // now we may insert o into a sharded collection
}
```

> **See also**
>
> sharding-shard-key for information
> on choosing a sharded key. Also see Shard Key Internals (in particular,
> sharding-internals-operations-and-reliability).
