# Documents

MongoDB stores data records as BSON documents. BSON is a binary
representation of JSON documents, though it contains more data
types than JSON. For the BSON spec, see [bsonspec.org](http://bsonspec.org/). See also bson-types.

## Document Structure

MongoDB documents are composed of field-and-value pairs and have the
following structure:

```javascript
{
   field1: value1,
   field2: value2,
   field3: value3,
   ...
   fieldN: valueN
}

```

The value of a field can be any of the BSON data types, including other documents, arrays, and arrays
of documents. For example, the following document contains values of varying types:

```javascript
var mydoc = {
               _id: ObjectId("5099803df3f4948bd2f98391"),
               name: { first: "Alan", last: "Turing" },
               birth: new Date('Jun 23, 1912'),
               death: new Date('Jun 07, 1954'),
               contribs: [ "Turing machine", "Turing test", "Turingery" ],
               views : Long(1250000)
            }

```

The above fields have the following data types:

- `_id` holds an objectid.
- `name` holds an *embedded document* that contains the fields
  `first` and `last`.
- `birth` and `death` hold values of the *Date* type.
- `contribs` holds an *array of strings*.
- `views` holds a value of the *NumberLong* type.

### Field Names

Field names are strings.

Documents have the following restrictions on field
names:

- 

## Dot Notation

MongoDB uses the *dot notation* to access the elements of an array and
to access the fields of an embedded document.

### Arrays

To specify or access an element of an array by the zero-based index
position, concatenate the array name with the dot (`.`) and
zero-based index position, and enclose in quotes:

```javascript
"<array>.<index>"

```

For example, given the following field in a document:

```javascript
{
   ...
   contribs: [ "Turing machine", "Turing test", "Turingery" ],
   ...
}

```

To specify the third element in the `contribs` array, use the dot
notation `"contribs.2"`.

For examples querying arrays, see:

- /tutorial/query-arrays
- /tutorial/query-array-of-documents/

> **See also**
>
> - `\$\[\]` all positional operator for update operations,
> - `\$\[\<identifier\>\]` filtered positional operator for update operations,
> - `\$` positional operator for update operations,
> - `\$` projection operator when array index position is
>   unknown
>
> \- read-operations-arrays for dot notation examples with arrays.

### Embedded Documents

To specify or access a field of an embedded document with dot notation,
concatenate the embedded document name with the dot (`.`) and
the field name, and enclose in quotes:

```javascript
"<embedded document>.<field>"

```

For example, given the following field in a document:

```javascript
{
   ...
   name: { first: "Alan", last: "Turing" },
   contact: { phone: { type: "cell", number: "111-222-3333" } },
   ...
}

```

- To specify the field named `last` in the `name` field, use the
  dot notation `"name.last"`.
- To specify the `number` in the `phone` document in the
  `contact` field, use the dot notation `"contact.phone.number"`.

> **Warning**
>
> Partition fields cannot use field names that contain a dot (`.`).

For examples querying embedded documents, see:

- /tutorial/query-embedded-documents
- /tutorial/query-array-of-documents/

## Document Limitations

Documents have the following attributes:

### Document Size Limit

### Document Field Order

Unlike JavaScript objects, the fields in a BSON document are ordered.

#### Field Order in Queries

For queries, the field order behavior is as follows:

- When comparing documents, field ordering is significant. For
  example, when comparing documents with fields `a` and `b` in a
  query:
  - `{a: 1, b: 1}` is equal to `{a: 1, b: 1}`
  - `{a: 1, b: 1}` is not equal to `{b: 1, a: 1}`
- For efficient query execution, the query engine may reorder fields
  during query processing. Among other cases, reordering fields may
  occur when processing these projection operators:
  `\$project`, `\$addFields`, `\$set`, and
  `\$unset`.
  - Field reordering may occur in intermediate results as well as the
    final results returned by a query.
  - Because some operations may reorder fields, you should not rely on
    specific field ordering in the results returned by a query that
    uses the projection operators listed earlier.

#### Field Order in Write Operations

### The `_id` Field

> **Note**
>

The `_id` field has the following behavior and constraints:

- By default, MongoDB creates a unique index on the `_id` field
  during the creation of a collection.
- The `_id` field is always the first field in the documents. If the
  server receives a document that does not have the `_id` field
  first, then the server will move the field to the beginning.
- If the `_id` contains subfields, the subfield names cannot begin
  with a (`$`) symbol.
- The `_id` field may contain values of any BSON data type, other than an array, regex, or undefined.

> **Warning To ensure functioning replication, do not store values**
>
> that are of the BSON regular expression type in the `_id`
> field.

The following are common options for storing values for `_id`:

- Use an objectid.
- Use a natural unique identifier, if available. This saves space and
  avoids an additional index.
- Generate an auto-incrementing number.
- Generate a UUID in your application code. For a more efficient
  storage of the UUID values in the collection and in the `_id`
  index, store the UUID as a value of the BSON `BinData` type.
- Use your driver's BSON UUID facility to generate UUIDs. Be aware
  that driver implementations may implement UUID serialization and
  deserialization logic differently, which may not be fully compatible
  with other drivers. See your `driver documentation` for
  information concerning UUID interoperability.

## Other Uses of the Document Structure

In addition to defining data records, MongoDB uses the document
structure throughout, including but not limited to: query filters, update specifications documents, and index specification documents

### Query Filter Documents

Query filter documents specify the conditions that determine which
records to select for read, update, and delete operations.

You can use `<field>:<value>` expressions to specify the equality
condition and query operator
expressions.

```javascript
{
  <field1>: <value1>,
  <field2>: { <operator>: <value> },
  ...
}

```

For examples, see:

- /tutorial/query-documents
- /tutorial/query-embedded-documents
- /tutorial/query-arrays
- /tutorial/query-array-of-documents/

### Update Specification Documents

Update specification documents use update operators to specify the data modifications to perform on
specific fields during an update operation.

```javascript
{
  <operator1>: { <field1>: <value1>, ... },
  <operator2>: { <field2>: <value2>, ... },
  ...
}


```

For examples, see Update specifications.

### Index Specification Documents

Index specification documents define the field to index and the index
type:

```javascript
{ <field1>: <type1>, <field2>: <type2>, ...  }
```
