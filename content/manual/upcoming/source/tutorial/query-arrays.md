# Query an Array

You can query arrays in MongoDB using the following methods:

## Match an Array

The following example queries for all documents where the field `tags`
value is an array with exactly two elements, `"red"` and `"blank"`,
in the specified order:

If, instead, you wish to find an array that contains both the elements
`"red"` and `"blank"`, without regard to order or other elements in
the array, use the `\$all` operator:

## Query an Array for an Element

The following example queries for all documents where `tags` is an
array that contains the string `"red"` as one of its elements:

For example, the following operation queries for all documents where the array
`dim_cm` contains at least one element whose value is greater than
`25`.

## Specify Multiple Conditions for Array Elements

When specifying compound conditions on array elements, you can specify
the query such that either a single array element meets these condition
or any combination of array elements meets the conditions.

### Query an Array with Compound Filter Conditions on the Array Elements

The following example queries for documents where the `dim_cm` array
contains elements that in some combination satisfy the query
conditions; e.g., one element can satisfy the greater than `15`
condition and another element can satisfy the less than `20`
condition, or a single element can satisfy both:

### Query for an Array Element that Meets Multiple Criteria

Use `\$elemMatch` operator to specify multiple criteria on the
elements of an array such that at least one array element satisfies all
the specified criteria.

The following example queries for documents where the `dim_cm` array
contains at least one element that is both greater than (`\$gt`)
`22` and less than (`\$lt`) `30`:

### Query for an Element by the Array Index Position

Using dot notation, you can specify query conditions for an
element at a particular index or position of the array. The array uses
zero-based indexing.

> **Note**
>
> When querying using dot notation, the field and nested field must be
> inside quotation marks.

The following example queries for all documents where the second
element in the array `dim_cm` is greater than `25`:

### Query an Array by Array Length

Use the `\$size` operator to query for arrays by number of
elements. For example, the following selects documents where the array
`tags` has 3 elements.

## Query an Array with {+atlas+}

The example in this section uses the sample movies dataset. To learn how to load the sample dataset
into your {+atlas+} deployment, see Load Sample Data.

To query an array in {+atlas+}, follow these steps:

**Navigate to the collection.**

**Specify a query filter document.**

To query a document that contains an array,
specify a query filter document.
A query filter document uses query operators to specify search conditions.
Use the following example documents to query array fields in the
`sample_mflix.movies` collection.

To apply a query filter, copy an example document into the
Filter search bar and click Apply.

To specify an equality condition on an array, use the query
document `{ <field>: <value> }` where `<value>` is
the exact array to match, including the order of the elements.
The following example finds documents that have a `genres`
field that contains the `["Action", "Comedy"]` array in the
specified order:

```
{ genres: ["Action", "Comedy"] }

```

To find an array that contains both the elements `Action` and
`Comedy`, without regard to order or other elements
in the array, use the `\$all` operator:

```
{ genres: { $all: ["Action", "Comedy"] } }
```

To query if the array field contains at least one element with the
specified value, use the filter `{ <field>: <value> }` where
`<value>` is the element value.

The following example queries for all documents where the
`genres` field contains the string `Short` as one
of its elements:

```
{ genres: "Short" }

```

To specify conditions on the elements in the array field,
use query operators in the
query filter document:

```
{ <array field>: { <operator1>: <value1>, ... } }

```

For example, the following operation uses the
`\$nin` operator to query for all documents
where the `genres` field does not contain `Drama`.

```
{ genres: { $nin: ["Drama"] } }
```

When specifying compound conditions on array elements, you can specify
the query such that either a single array element meets these condition
or any combination of array elements meets the conditions.

### Query an Array with Compound Filter Conditions on the Array Elements

The following example queries for documents where the `cast`
array contains elements that in some combination satisfy the query
conditions. For example, the following filter uses the `\$regex`
and `\$eq` operators to return documents where a single array element
ends in `Olsen` and another element equals `Mary-Kate Olsen` or
a single element that satisfies both conditions:

```
{ cast: { $regex: "Olsen$", $eq: "Mary-Kate Olsen" } }

```

This query filter returns movies that include `Mary-Kate Olsen` in
their cast, and movies that include both `Mary-Kate Olsen` and
`Ashley Olsen` in their cast.

### Query for an Array Element that Meets Multiple Criteria

Use `\$elemMatch` operator to specify multiple criteria on the
elements of an array such that at least one array element satisfies all
the specified criteria.

The following example uses the `\$elemMatch` and `\$ne`
operators to query for documents where the `languages` array contains
at least one element that is both not `null` and does not equal `English`.

```
{ languages: { $elemMatch: { $ne: null, $ne: "English" } } }

```

### Query for an Element by the Array Index Position

Using dot notation, you can specify query conditions for an
element at a particular index or position of the array. The array uses
zero-based indexing.

> **Note**
>
> When querying using dot notation, the field and nested field must be
> inside quotation marks.

The following example uses the `\$ne` operator to query
for all documents where the first element in the `countries`
array is not equal to `USA`:

```
{ "countries.0": { $ne: "USA" } }

```

### Query an Array by Array Length

Use the `\$size` operator to query for arrays by number of
elements. For example, the following selects documents where the array
`genres` has 3 elements.

```
{ genres: { $size: 3 } }
```

## Additional Query Tutorials

For additional query examples, see:

- /tutorial/query-documents
- /tutorial/query-embedded-documents
- /tutorial/query-array-of-documents
