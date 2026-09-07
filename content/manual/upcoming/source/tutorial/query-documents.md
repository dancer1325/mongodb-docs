# Query Documents

To query documents, specify a query predicate indicating the
documents you want to return. If you specify an empty query predicate
(`{ }`), the query returns all documents in the collection.

You can query documents in MongoDB by using the following
methods:

## Select All Documents in a Collection

This operation uses a query predicate of `{}`, which corresponds to
the following SQL statement:

```sql
SELECT * FROM inventory

```

## Specify Equality Condition

The following example selects from the `inventory` collection all
documents where the `status` equals `"D"`:

This operation uses a query predicate of `{ status: "D" }`, which
corresponds to the following SQL statement:

```sql
SELECT * FROM inventory WHERE status = "D"

```

## Specify Conditions Using Query Operators

The following example retrieves all documents from the `inventory`
collection where `status` equals either `"A"` or `"D"`:

> **Note**
>
> Although you can express this query using the `\$or` operator,
> use the `\$in` operator rather than the `\$or`
> operator when performing equality checks on the same field.

The operation uses a query predicate of
`{ status: { $in: [ "A", "D" ] } }`, which corresponds to the
following SQL statement:

```sql
SELECT * FROM inventory WHERE status in ("A", "D")

```

For the complete list of MongoDB query operators, see
query-predicates-ref.

## Specify `AND` Conditions

A compound query can specify conditions for more than one field in the
collection's documents. Implicitly, a logical `AND` conjunction
connects the clauses of a compound query so that the query selects the
documents in the collection that match all the conditions.

The following example retrieves all documents in the `inventory`
collection where the `status` equals `"A"` **and** `qty` is less
than (`\$lt`) `30`:

The operation uses a query predicate of
`{ status: "A", qty: { $lt: 30 } }`, which corresponds to the
following SQL statement:

```sql
SELECT * FROM inventory WHERE status = "A" AND qty < 30

```

See comparison operators for other
MongoDB comparison operators.

## Specify `OR` Conditions

Using the `\$or` operator, you can specify a compound query
that joins each clause with a logical `OR` conjunction so that the
query selects the documents in the collection that match at least one
condition.

The following example retrieves all documents in the collection where
the `status` equals `"A"` **or** `qty` is less than
(`\$lt`) `30`:

The operation uses a query predicate of
`{ $or: [ { status: 'A' }, { qty: { $lt: 30 } } ] }`, which
corresponds to the following SQL statement:

```sql
SELECT * FROM inventory WHERE status = "A" OR qty < 30

```

> **Note**
>
> Queries that use comparison operators are subject to type-bracketing.

## Specify `AND` as well as `OR` Conditions

In the following example, the compound query document selects all
documents in the collection where the `status` equals `"A"`
**and** *either* `qty` is less than (`\$lt`) `30` *or*
`item` starts with the character `p`:

The operation uses a query predicate of:

```javascript
{
   status: 'A',
   $or: [
     { qty: { $lt: 30 } }, { item: { $regex: '^p' } }
   ]
}

```

which corresponds to the following SQL statement:

```sql
SELECT * FROM inventory WHERE status = "A" AND ( qty < 30 OR item LIKE "p%")

```

> **Note**
>
> MongoDB supports regular expressions `\$regex` queries to
> perform string pattern matches.

## Query Documents with {+atlas+}

The example in this section uses the sample movies dataset. To learn how to load the sample dataset
into your {+atlas+} deployment, see Load Sample Data.

To project fields to return from a query in {+atlas+}, follow these
steps:

**Navigate to the collection**

**Specify the Filter field**

```javascript
{ year: 1924 }
```

**Click Apply**

This query filter returns all documents
in the `sample_mflix.movies` collection where the `year`
field matches `1924`.
Additional Query Tutorials
--------------------------

For additional query examples, see:

- /tutorial/query-embedded-documents
- /tutorial/query-arrays
- /tutorial/query-array-of-documents
- /tutorial/project-fields-from-query-results
- /tutorial/query-for-null-fields

## Behavior

### Cursor

### Concurrent Updates While Using a Cursor

### Read Isolation

For reads to Replica sets and replica set
shards, read concern allows clients to choose a
level of isolation for their reads. For more information, see
/reference/read-concern.

### Query Result Format

When you run a find operation with a MongoDB driver or `mongosh`, the
command returns a cursor that manages query results. The query
results are not returned as an array of documents.

To learn how to iterate through documents in a cursor, refer to your
`driver's documentation`. If you are using `mongosh`, see
read-operations-cursors.

## Additional Methods and Options
