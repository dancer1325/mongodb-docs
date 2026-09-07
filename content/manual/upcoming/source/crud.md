# MongoDB CRUD Operations

CRUD operations *create*, *read*, *update*, and *delete*
documents.

You can connect with driver methods and perform CRUD operations
for deployments hosted in the following environments:

## Create Operations

Create or insert operations add new documents to a collection. If the
collection does not currently exist, insert operations will create the
collection.

MongoDB provides the following methods to insert documents into a
collection:

- `db.collection.insertOne()`
- `db.collection.insertMany()`

In MongoDB, insert operations target a single collection. All
write operations in MongoDB are atomic on the level of a single
document.

For examples, see /tutorial/insert-documents.

## Read Operations

Read operations retrieve documents from a
collection; i.e. query a collection for
documents. MongoDB provides the following methods to read documents from
a collection:

- `db.collection.find()`

You can specify query filters or criteria that identify the documents to return.

For examples, see:

- /tutorial/query-documents
- /tutorial/query-embedded-documents
- /tutorial/query-arrays
- /tutorial/query-array-of-documents

## Update Operations

Update operations modify existing documents in a collection. MongoDB
provides the following methods to update documents of a collection:

- `db.collection.updateOne()`
- `db.collection.updateMany()`
- `db.collection.replaceOne()`

In MongoDB, update operations target a single collection. All write
operations in MongoDB are atomic on the level of a single document.

You can specify criteria, or filters, that identify the documents to
update. These filters use the same
syntax as read operations.

For examples, see /tutorial/update-documents.

## Delete Operations

Delete operations remove documents from a collection. MongoDB provides
the following methods to delete documents of a collection:

- `db.collection.deleteOne()`
- `db.collection.deleteMany()`

In MongoDB, delete operations target a single collection. All
write operations in MongoDB are atomic on the level of a single document.

You can specify criteria, or filters, that identify the documents to
remove. These filters use the same
syntax as read operations.

For examples, see /tutorial/remove-documents.

## Bulk Write

MongoDB provides the ability to perform write operations in bulk. For
details, see /core/bulk-write-operations.
