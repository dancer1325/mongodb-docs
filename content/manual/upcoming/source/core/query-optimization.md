# Query Optimization

Query optimization improves the efficiency of read operations by
reducing the amount of data that query operations need to process.
Use indexes, projections, and query limits to enhance query
performance and reduce resource consumption.

Query optimization can occur both during development and later as your
data usage and demand changes. As collections grow, a periodic review of
query performance can help determine when clusters need to scale up or
scale out.

## Create Indexes to Support Queries

Indexes store values from individual fields or sets of
fields from a collection in a separate data structure. In read
operations, they allow MongoDB to search in the index to identify
relevant documents instead of the entire collection. In write
operations, MongoDB must both write the change to the collection and
update the index.

Create indexes for commonly issued queries. If a query searches multiple
fields, create a compound index.

For example, consider the following query on the `type` field in the
`inventory` collection:

```javascript
let typeValue = <someUserInput>;
db.inventory.find( { type: typeValue } );

```

To improve performance for this query, add an index to the `inventory`
collection on the `type` field.[^1] In
`mongosh`, create indexes using the
`db.collection.createIndex()` method:

```javascript
db.inventory.createIndex( { type: 1 } )

```

To analyze query performance, see /tutorial/analyze-query-plan.

## Create Selective Queries

Query selectivity refers to how well the query predicate filters out
documents in a collection. Query selectivity determines whether queries
can use indexes effectively.

More selective queries match a smaller percentage of documents. For
instance, an equality match on the unique `_id` field is highly
selective as it can match at most one document.

Less selective queries match a larger percentage of documents and
cannot use indexes effectively.

The selectivity of `regular expressions` depends on the
expressions themselves. For details, see regular expression and index use.

## Project Only Necessary Data

When you need a subset of fields from documents, you can improve
performance by returning only the fields you need. Projections reduce
network traffic and processing time.

For example, if your query to the `posts` collection needs only the
`timestamp`, `title`, `author`, and `abstract` fields, specify
those fields in the projection:

```javascript
db.posts.find( 
   {}, 
   { timestamp : 1, title : 1, author : 1, abstract : 1} 
).sort( { timestamp : -1 } )

```

For more information on using projections, see
read-operations-projection.

### Example

To achieve a covered query, you must index projected fields. The ESR (Equality, Sort, Range) rule
applies to the order of fields in the index.

For example, consider the following index on an `inventory` collection:

```javascript
db.inventory.createIndex( { type: 1, _id: 1, price: 1, item: 1, expiryDate: 1} )

```

The above query, while technically correct, is not structured to optimize query
performance.

The following query uses the ESR (Equality, Sort, Range) rule to create a more efficient
compound index and improve query response times.

```javascript
db.inventory.aggregate([
  { $match: {type: "food", expiryDate: { $gt: ISODate("2025-07-10T00:00:00Z") }}},
  { $sort: { item: 1 }},
  { $project: { _id: 1, price: 1} }
])

```

The index and query follow the ESR rule:

- `type` is used for an equality match (E), so it is the first field in the index.
- `item` is used for sorting (S), so it is after `type` in the index.
- `expiryDate` is used for a range query (R), so it is the last field in the index.

## Limit Query Results

MongoDB cursors return results in batches. If you know
the number of results you want, specify that value in the
`limit()` method. Limiting results reduces the demand on
network resources.

Generally, limiting results is most useful when results are sorted so
you know which documents will be returned. For example, if you need only
10 results from your query to the `posts` collection, run the
following query:

```javascript
db.posts.find().sort( { timestamp : -1 } ).limit(10)

```

For more information on limiting results, see `limit()`.

## Use Index Hints

The query optimizer
typically selects the optimal index for a specific operation. However,
you can force MongoDB to use a specific index using the
`hint()` method. Use `hint()` to support
performance testing or when you are querying a field that appears in
several indexes to guarantee that MongoDB uses the correct index.

## Use Server-Side Operations

Use MongoDB's `\$inc` operator to increment or decrement
values in documents. The operator increments the value of the field on
the server side, as an alternative to selecting a document, making
simple modifications in the client, and then writing the entire
document to the server. The `\$inc` operator can also help
avoid race conditions that occur when two application instances query
for a document, manually increment a field, and save the entire
document back at the same time.

## Run Covered Queries

A covered query is a query that can be satisfied entirely using an
index and does not have to examine any documents. An index covers a
query when all of the following apply:

- All the fields in the query (both as specified by the application and
  as needed internally such as for sharding purposes) are part of an
  index.
- All the fields returned in the results are in the same index.
- No fields in the query are equal to `null`. For example, the
  following query predicates cannot result in covered queries:
  - `{ "field": null }`
  - `{ "field": { $eq: null } }`

### Example

An `inventory` collection has the following index on the `type` and
`item` fields:

```javascript
db.inventory.createIndex( { type: 1, item: 1 } )

```

The index covers the following operation which queries on the `type`
and `item` fields and returns only the `item` field:

```javascript
db.inventory.find(
   { type: "food", item:/^c/ },
   { item: 1, _id: 0 }
)

```

For the specified index to cover the query, the projection document
must explicitly specify `_id: 0` to exclude the `_id` field from
the result since the index does not include the `_id` field.

### Embedded Documents

An index can cover a query on fields within embedded documents.

For example, consider a `userdata` collection with documents of the
following form:

```javascript
db.userdata.insertOne(
   { _id: 1, user: { login: "tester" } }
)

```

The collection has the following index:

```javascript
db.userdata.createIndex(
   { "user.login": 1 }
)

```

The `{ "user.login": 1 }` index covers the following query:

```javascript
db.userdata.find(
   { "user.login": "tester" },
   { "user.login": 1, _id: 0 }
)

```

> **Note**
>
> To index fields in embedded documents, use dot notation. See
> index-embedded-fields.

### Multikey Covering

Multikey indexes can cover queries over the non-array fields
if the index tracks which field or fields cause the index to be multikey.

For an example of a covered query with a multikey index, see
multikey-covered-queries on the multikey indexes page.

### Performance

Because the index contains all fields required by the query, MongoDB can both
match the query conditions
and return the results using only the index.

Querying *only* the index can be much faster than querying documents
outside of the index. Index keys are typically smaller than the
documents they catalog, and indexes are typically available in RAM or
located sequentially on disk.

### Limitations

#### Index Types

Not all index types can cover queries. For details
on covered index support, refer to the documentation page for the
corresponding index type.

#### Sharded Collections

### Explain Results

To determine whether a query is a covered query, use the
`db.collection.explain()` or the `explain()`
method. See explain-output-covered-queries.

[^1]: For single-field indexes, the order of the index does not matter. For
    compound indexes, the field order impacts what queries the index
    supports. For details, see index-ascending-and-descending.
