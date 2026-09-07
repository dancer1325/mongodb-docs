# Sparse Indexes

Sparse indexes only contain entries for documents that have the indexed
field, even if the index field contains a null value. The index skips
over any document that is missing the indexed field. The index is
"sparse" because it does not include all documents of a collection. By
contrast, non-sparse indexes contain all documents in a collection,
storing null values for those documents that do not contain the indexed
field.

> **Important**
>
> Partial indexes can function as
> sparse indexes, but also support filter expressions for conditions beyond
> whether a field exists. Use a partial index for greater
> control if you need precise filtering.

## Create a Sparse Index

To create a sparse index, use the `db.collection.createIndex()`
method with the `sparse` option set to `true`.

For example, the following operation in `mongosh` creates a
sparse index on the `xmpp_id` field of the `addresses` collection:

```javascript
db.addresses.createIndex( { "xmpp_id": 1 }, { sparse: true } )

```

The index does not index documents that do not include the `xmpp_id`
field.

> **Note**
>
> Do not confuse sparse indexes in MongoDB with [block-level]()
> indexes in other databases. Think of them as dense indexes with a
> specific filter.

## Behavior

### Sparse Index and Incomplete Results

If a sparse index would result in an incomplete result set for queries
and sort operations, MongoDB will not use that index unless a
`hint()` explicitly specifies the index.

For example, the query `{ x: { $exists: false } }` will not use a
sparse index on the `x` field unless explicitly hinted. See
sparse-index-incomplete-results for an example that details the
behavior.

### Indexes that are Sparse by Default

The following index types are always sparse:

- 2d
- 2dsphere (version 2)
- Text
- Wildcard

### Sparse Compound Indexes

### Sparse and Unique Properties

An index that is both sparse and unique
prevents a collection from having documents with duplicate values for a
field but allows multiple documents that omit the key.

## Examples

### Create a Sparse Index On A Collection

Consider a collection `scores` that contains the following documents:

```javascript
{ "_id" : ObjectId("523b6e32fb408eea0eec2647"), "userid" : "newbie" }
{ "_id" : ObjectId("523b6e61fb408eea0eec2648"), "userid" : "abby", "score" : 82 }
{ "_id" : ObjectId("523b6e6ffb408eea0eec2649"), "userid" : "nina", "score" : 90 }

```

The collection has a sparse index on the field `score`:

```javascript
db.scores.createIndex( { score: 1 } , { sparse: true } )

```

Then, the following query on the `scores` collection uses the sparse
index to return the documents that have the `score` field less than
(`\$lt`) `90`:

```javascript
db.scores.find( { score: { $lt: 90 } } )

```

Because the document for the userid `"newbie"` does not contain the
`score` field and thus does not meet the query criteria, the query
can use the sparse index to return the results:

```javascript
{ "_id" : ObjectId("523b6e61fb408eea0eec2648"), "userid" : "abby", "score" : 82 }

```

### Sparse Index On A Collection Cannot Return Complete Results

Consider a collection `scores` that contains the following documents:

```javascript
{ "_id" : ObjectId("523b6e32fb408eea0eec2647"), "userid" : "newbie" }
{ "_id" : ObjectId("523b6e61fb408eea0eec2648"), "userid" : "abby", "score" : 82 }
{ "_id" : ObjectId("523b6e6ffb408eea0eec2649"), "userid" : "nina", "score" : 90 }

```

The collection has a sparse index on the field `score`:

```javascript
db.scores.createIndex( { score: 1 } , { sparse: true } )

```

Because the document for the userid `"newbie"` does not contain the
`score` field, the sparse index does not contain an entry for that
document.

Consider the following query to return **all** documents in the `scores`
collection, sorted by the `score` field:

```javascript
db.scores.find().sort( { score: -1 } )

```

Even though the sort is by the indexed field, MongoDB will **not**
select the sparse index to fulfill the query in order to return
complete results:

```javascript
{ "_id" : ObjectId("523b6e6ffb408eea0eec2649"), "userid" : "nina", "score" : 90 }
{ "_id" : ObjectId("523b6e61fb408eea0eec2648"), "userid" : "abby", "score" : 82 }
{ "_id" : ObjectId("523b6e32fb408eea0eec2647"), "userid" : "newbie" }

```

To use the sparse index, explicitly specify the index with
\`hint()\`:

```javascript
db.scores.find().sort( { score: -1 } ).hint( { score: 1 } )

```

The use of the index results in the return of only those documents with
the `score` field:

```javascript
{ "_id" : ObjectId("523b6e6ffb408eea0eec2649"), "userid" : "nina", "score" : 90 }
{ "_id" : ObjectId("523b6e61fb408eea0eec2648"), "userid" : "abby", "score" : 82 }

```

> **See also**
>
> - `explain()`
> - /tutorial/analyze-query-plan

### Sparse Index with Unique Constraint

Consider a collection `scores` that contains the following documents:

```javascript
{ "_id" : ObjectId("523b6e32fb408eea0eec2647"), "userid" : "newbie" }
{ "_id" : ObjectId("523b6e61fb408eea0eec2648"), "userid" : "abby", "score" : 82 }
{ "_id" : ObjectId("523b6e6ffb408eea0eec2649"), "userid" : "nina", "score" : 90 }

```

You could create an index with a unique constraint and sparse filter on the `score` field using
the following operation:

```javascript
db.scores.createIndex( { score: 1 } , { sparse: true, unique: true } )

```

This index *would permit* the insertion of documents that had unique
values for the `score` field *or* did not include a `score` field.
As such, given the existing documents in the `scores` collection, the
index permits the following insert operations:

```javascript
db.scores.insertMany( [
   { "userid": "newbie", "score": 43 },
   { "userid": "abby", "score": 34 },
   { "userid": "nina" }
] )

```

However, the index *would not permit* the addition of the following
documents since documents already exists with `score` value of `82`
and `90`:

```javascript
db.scores.insertMany( [
   { "userid": "newbie", "score": 82 },
   { "userid": "abby", "score": 90 }
] )

```

### Sparse and Non-Sparse Unique Indexes
