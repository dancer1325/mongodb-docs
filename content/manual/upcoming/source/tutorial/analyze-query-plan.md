# Interpret Explain Plan Results

You can use explain results to determine the following information about
a query:

- The amount of time a query took to complete
- Whether the query used an index
- The number of documents and index keys scanned to fulfill a query

> **Note**
>
> Explain plan results for queries are subject to change between
> MongoDB versions.

## Evaluate the Performance of a Query

Consider a collection `inventory` with the following documents:

```javascript
db.inventory.insertMany ( [ 
   { _id: 1, item: "f1", type: "food", quantity: 500 },
   { _id: 2, item: "f2", type: "food", quantity: 100 },
   { _id: 3, item: "p1", type: "paper", quantity: 200 },
   { _id: 4, item: "p2", type: "paper", quantity: 150 },
   { _id: 5, item: "f3", type: "food", quantity: 300 },
   { _id: 6, item: "t1", type: "toys", quantity: 500 },
   { _id: 7, item: "a1", type: "apparel", quantity: 250 },
   { _id: 8, item: "a2", type: "apparel", quantity: 400 },
   { _id: 9, item: "t2", type: "toys", quantity: 50 },
   { _id: 10, item: "f4", type: "food", quantity: 75 }
] )

```

### Query with No Index

The difference between the number of matching documents and the number
of examined documents may suggest that, to improve efficiency, the
query might benefit from the use of an index.

### Query with Index

To support the query on the `quantity` field, add an index on the
`quantity` field:

Without the index, the query would scan the whole collection of `10`
documents to return `3` matching documents. The query also had to
scan the entirety of each document, potentially pulling them into
memory. This results in an expensive and potentially slow query
operation.

When run with an index, the query scanned `3` index entries
and `3` documents to return `3` matching documents, resulting
in a very efficient query.
