# Reference Data in Your MongoDB Schema

References store relationships between data by including links, called
**references**, from one document to another. Applications can resolve these
references to access the related data. In the following example,
the `contact` and `access` documents contain a reference to the
`user` document.

References result in **normalized** data models because data is divided
into multiple collections and not duplicated.

## Use Cases

Although a denormalized data models work for most use cases in MongoDB, consider
using references instead of embedded data if:

- Embedding would result in duplication of data but would not
  provide sufficient read performance advantages to outweigh the
  implications of the duplication. For example, when the embedded data
  frequently changes.
- You need to represent complex many-to-many relationships or large hierarchical
  data sets.
- You need to frequently query the related entity on its own.

### Large Number of Collections

In certain situations, you might choose to store related information in
several collections rather than in a single collection.

Consider a sample collection `logs` that stores log documents for
various environment and applications. The `logs` collection contains
documents of the following form:

```javascript
{ log: "dev", ts: ..., info: ... }
{ log: "debug", ts: ..., info: ...}

```

If the total number of documents is low, you may group documents into
collection by type. For logs, consider maintaining distinct log
collections, such as `logs_dev` and `logs_debug`.

Generally, having a large number of collections has no significant performance
penalty and results in good performance. Distinct collections are very important
for high-throughput batch processing.

When using models that have a large number of collections, consider
the following behaviors:

- Each collection has a certain minimum overhead of a few kilobytes.
- Unindexed read operations might consume a large amount of memory.
- For each database, a single namespace file (such as
  `<database>.ns`) stores all metadata for that database. Each
  index and collection has its own entry in the namespace file. See
  places namespace length limits for
  specific limitations.

## Query Normalized Data Models

To query normalized data in multiple collections, MongoDB provides the
following aggregation stages:

- `\$lookup`
- `\$graphLookup`

For an example of normalized data models, see
data-modeling-publisher-and-books.

For examples of various tree models, see
data-model-tree-structure.
