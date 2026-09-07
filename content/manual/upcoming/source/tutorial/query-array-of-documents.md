# Query an Array of Embedded Documents

You can query documents in MongoDB by using the following
methods:

## Query for a Document Nested in an Array

The following example selects all documents where an element in the
`instock` array matches the specified document:

Equality matches on the whole embedded/nested document require an
*exact* match of the specified document, including the field order. For
example, the following query does not match any documents in the
`inventory` collection:

## Specify a Query Condition on a Field in an Array of Documents

### Specify a Query Condition on a Field Embedded in an Array of Documents

If you do not know the index position of the document nested in the
array, concatenate the name of the array field, with a dot (`.`) and
the name of the field in the nested document.

The following example selects all documents where the `instock` array
has at least one embedded document that contains the field `qty`
whose value is less than or equal to `20`:

### Use the Array Index to Query for a Field in the Embedded Document

Using dot notation, you can specify query conditions for
field in a document at a particular index or position of the array. The
array uses zero-based indexing.

> **Note**
>
> When querying using dot notation, the field and index must be
> inside quotation marks.

The following example selects all documents where the `instock` array
has as its first element a document that contains the field `qty`
whose value is less than or equal to `20`:

## Specify Multiple Conditions for Array of Documents

When specifying conditions on more than one field nested in an array of
documents, you can specify the query such that either a single document
meets these condition or any combination of documents (including a
single document) in the array meets the conditions.

### A Single Nested Document Meets Multiple Query Conditions on Nested Fields

Use `\$elemMatch` operator to specify multiple criteria on an
array of embedded documents such that at least one embedded document
satisfies all the specified criteria.

The following example queries for documents where the `instock` array
has at least one embedded document that contains both the field
`qty` equal to `5` and the field `warehouse` equal
to `A`:

The following example queries for documents where the `instock` array
has at least one embedded document that contains the field `qty` that
is greater than `10` and less than or equal to `20`:

### Combination of Elements Satisfies the Criteria

If the compound query conditions on an array field do not use the
`\$elemMatch` operator, the query selects those documents whose
array contains any combination of elements that satisfies the
conditions.

For example, the following query matches documents where any document
nested in the `instock` array has the `qty` field greater than
`10` and any document (but not necessarily the same embedded
document) in the array has the `qty` field less than or equal to
`20`:

The following example queries for documents where the `instock` array
has at least one embedded document that contains the field `qty`
equal to `5` and at least one embedded document (but not necessarily
the same embedded document) that contains the field `warehouse` equal
to `A`:

## Query an Array of Documents with {+atlas+}

The example in this section uses the sample training dataset. To learn how to load the sample
dataset into your {+atlas+} deployment, see Load Sample Data.

To query an array of documents in {+atlas+}, follow these steps:

**Navigate to the collection**

**Specify the Filter field**

```javascript
{"scores.type": "exam"}
```

**Click Apply**

This query filter returns all documents in the `sample_training.grades`
collection that contain a subdocument in the `scores` array where
`type` is set to `exam`. The full document, including the entire
`scores` array, is returned. For more information on modifying the
returned array, see project-array-elements-in-returned-array.
Additional Query Tutorials
--------------------------

For additional query examples, see:

- /tutorial/query-arrays
- /tutorial/query-documents
- /tutorial/query-embedded-documents
