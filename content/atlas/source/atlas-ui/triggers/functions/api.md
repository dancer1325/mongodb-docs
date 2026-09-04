# MongoDB API Reference

## mongodb.admin()

Gets a handle for the `admin` database in a linked MongoDB data source. You can use this to run MongoDB admin commands like `admin.getDBNames()`.

```javascript
const mongodb = context.services.get("mongodb-atlas");
const admin = mongodb.admin();
```

### Parameters

```typescript
admin(): AdminDatabase
```

### Return Value

The `mongodb.admin()` method returns an `AdminDatabase` object. The object contains helper methods that wrap a subset of MongoDB database commands. See `admin.getDBNames()`.

## admin.getDBNames()

Returns a list of database names in a MongoDB data source.

```javascript
const mongodb = context.services.get("mongodb-atlas");
const admin = mongodb.admin();
const dbNames = admin.getDBNames();
```

### Parameters

```typescript
getDBNames(): string[]
```

### Return Value

The `admin.getDBNames()` method returns an array of strings where each element is the name of a database in the data source.

## mongodb.db()

Gets a handle for a database in a linked MongoDB data source.

```javascript
const mongodb = context.services.get("mongodb-atlas");
const db = mongodb.db("myDB");
```

### Parameters

```typescript
db(name: string): Database
```

| Parameter | Type | Description |
| --- | --- | --- |
| `name` | string | The name of the database. |

### Return Value

The `mongodb.db()` method returns a `Database` object that allows you to access collections in the specified database. See `database.collection()`.

## database.getCollectionNames()

Returns a list of collection names in the database.

```javascript
const mongodb = context.services.get("mongodb-atlas");
const db = mongodb.db("myDB");
const collectionNames = db.getCollectionNames();
```

### Parameters

```typescript
getCollectionNames(): string[]
```

### Return Value

The `database.getCollectionNames()` method returns an array of strings where each element is the name of a collection in the database.

## database.collection()

Gets a handle for a collection in a linked MongoDB data source from a database handle.

```javascript
const mongodb = context.services.get("mongodb-atlas");
const db = mongodb.db("myDB");
const collection = db.collection("myCollection");
```

### Parameters

```typescript
collection(name: string): Collection
```

| Parameter | Type | Description |
| --- | --- | --- |
| `name` | string | The name of the collection. |

### Return Value

The `database.collection()` method returns a collection object that allows you to query the specified collection.

## collection.find()

Finds all documents in a collection or view that match the provided query filters. Returns a cursor object that allows you to access matching documents.

```javascript
const query = { "reviews.0": { "$exists": true } };
const projection = { "_id": 0 };

return itemsCollection.find(query, projection)
  .sort({ name: 1 })
  .toArray()
  .then(items => {
    console.log(`Successfully found ${items.length} documents.`)
    items.forEach(console.log)
    return items
  })
  .catch(err => console.error(`Failed to find documents: ${err}`))
```

### Parameters

```typescript
find(
   query?: object,
   projection?: object,
   options?: object
): Cursor
```

| Parameter | Type | Description |
| --- | --- | --- |
| `query` | `object` | Optional. |
| `projection` | `object` | Optional. |
| `options` | `object` | An object that specifies additional configuration options. |
| `options.session` | `ClientSession` | .. include:: /includes/triggers/parameter-options-session.rst |

### Return Value

The `collection.find()` method returns a cursor object that points to any documents that match the specified query. You can manipulate and access documents in the query result set with the following cursor methods:

| Method | Description |
| --- | --- |
| `cursor.next()` | Iterates the cursor and returns a Promise that resolves to the next document in the cursor. If the cursor is exhausted, the promise resolves to `undefined`. collection.find().next() .then(doc => console.log("next document", doc)) |
| `cursor.toArray()` | Iterates the cursor to exhaustion and returns a Promise that resolves to an array that contains all of the iterated documents. collection.find().toArray() .then(docs => console.log("all documents", docs)) |
| `cursor.skip(amount)` | Specifies a number of matching documents to omit from the query result set. MongoDB omits documents from the result set in sort order until it has skipped the specified number. If the query also specifies a limit, skipped documents do not count towards the limit threshold. **Note** You can't call this method after retrieving one or more documents using `cursor.next()` or `cursor.toArray()`. |
| `cursor.limit(limit)` | Specifies the maximum number of documents to include in the query result set. If the result set contains more documents than the specified `limit`, the cursor will return documents in order up to the limit. **Note** You can't call this method after retrieving one or more documents using `cursor.next()` or `cursor.toArray()`. |
| `cursor.sort(sort)` | Sorts documents in the result set according to the `sort` filter. Sort documents specify one or more fields to sort on. The value of each field indicates whether MongoDB should sort it in ascending (`1`) or descending (`-1`) order. For more information, see [cursor.sort](/reference/method/cursor.sort). **Note** You can't call this method after retrieving one or more documents using `cursor.next()` or `cursor.toArray()`. The following sort document specifies that documents should be sorted first by `age` from highest to lowest. Once sorted by age, the result set should further be sorted by `name` in alphabetical order for each distinct age value. { age: -1, name: 1 } |

> **Note:**
> You can't return a cursor from a function. Instead, evaluate the cursor using `cursor.next()` or `cursor.toArray()` and return the result.

## collection.findOne()

Finds a single document from a collection or view. If multiple documents match the query, this returns the first matching document in the collection. The `findOne()` method does not support sorting. As a workaround, use `find()` with the `sort()` and `next()` cursor methods to return a single document from a sorted collection.

```javascript
collection.find({}).sort({"<Field Name>": 1}).next()
  .then(result => console.log("Found Document: ", result))
```

```javascript
const query = { "quantity": { "$gte": 25 } };
const projection = {
 "title": 1,
 "quantity": 1,
}

return itemsCollection.findOne(query, projection)
  .then(result => {
    if(result) {
      console.log(`Successfully found document: ${result}.`);
    } else {
      console.log("No document matches the provided query.");
    }
    return result;
  })
  .catch(err => console.error(`Failed to find document: ${err}`));
```

### Parameters

```typescript
findOne(
   query?: object,
   projection?: object,
   options?: object
): Promise<object | null>
```

| Parameter | Type | Description |
| --- | --- | --- |
| `query` | `object` | Optional. |
| `projection` | `object` | Optional. |
| `options` | `object` | An object that specifies additional configuration options. |
| `options.session` | `ClientSession` | .. include:: /includes/triggers/parameter-options-session.rst |

### Return Value

The `collection.findOne()` method returns a Promise that resolves to the first document in the collection that matches the query. If no documents match the specified query, the promise resolves to `null`.

```typescript
Promise<object | null>
```

## collection.findOneAndUpdate()

Updates a single document in a collection or view and returns the document in either its pre-update or post-update form.

Unlike `collection.updateOne()`, this action allows you to atomically find, modify, and return a document with the same command. This avoids the risk of other update operations changing the document between separate find and update operations.

```javascript
// Find the document that describes "lego"
const query = { "name": "lego" };
// Set some fields in that document
const update = {
  "$set": {
    "name": "blocks",
    "price": 20.99,
    "category": "toys"
  }
};
// Return the updated document instead of the original document
const options = { returnNewDocument: true };

return itemsCollection.findOneAndUpdate(query, update, options)
  .then(updatedDocument => {
    if(updatedDocument) {
      console.log(`Successfully updated document: ${updatedDocument}.`)
    } else {
      console.log("No document matches the provided query.")
    }
    return updatedDocument
  })
  .catch(err => console.error(`Failed to find and update document: ${err}`))
```

### Parameters

```typescript
findOneAndUpdate(
   query: object,
   update: object,
   options?: object
): Promise<object | null>
```

| Parameter | Type | Description |
| --- | --- | --- |
| `query` | `object` | .. include:: /includes/triggers/parameter-query.rst |
| `update` | `object` | .. include:: /includes/triggers/parameter-update.rst |
| `options` | `object` | An object that specifies additional configuration options. |
| `options.upsert` | `boolean` | .. include:: /includes/triggers/parameter-options-upsert.rst |
| `options.sort` | `boolean` | .. include:: /includes/triggers/parameter-options-sort.rst |
| `options.projection` | `boolean` | .. include:: /includes/triggers/parameter-projection.rst |
| `options.returnNewDocument` | `boolean` | .. include:: /includes/triggers/parameter-options-returnNewDocument.rst |
| `options.session` | `ClientSession` | .. include:: /includes/triggers/parameter-options-session.rst |

### Return Value

The `collection.findOneAndUpdate()` method returns a Promise that resolves to a single document that the query overwrote. If no documents match the specified query, the promise resolves to `null`.

```typescript
Promise<object | null>
```

> **Note:**
> You can specify whether to return the pre-replacement or post-replacement version of the document by setting the value of `options.returnNewDocument`. By default, `returnNewDocument` is `false`, which indicates that the promise should resolve to the *pre-update* version of the document.

## collection.findOneAndReplace()

Overwrites a single document in a collection or view and returns the document in either its pre-replacement or post-replacement form.

Unlike `collection.updateOne()`, this action allows you to atomically find, modify, and return a document with the same command. This avoids the risk of other update operations changing the document between separate find and update operations.

```javascript
// Find the document that describes "lego"
const query = { "name": "lego" };
// Replace it with a new document
const replacement = {
    "name": "blocks",
    "price": 20.99,
    "category": "toys"
};
// Return the original document as it was before being replaced
const options = { "returnNewDocument": false };

return itemsCollection.findOneAndReplace(query, replacement, options)
  .then(replacedDocument => {
    if(replacedDocument) {
      console.log(`Successfully replaced the following document: ${replacedDocument}.`)
    } else {
      console.log("No document matches the provided query.")
    }
    return updatedDocument
  })
  .catch(err => console.error(`Failed to find and replace document: ${err}`))
```

### Parameters

```typescript
findOneAndReplace(
   query: object,
   replacement: object,
   options?: object
): Promise<object | null>
```

| Parameter | Type | Description |
| --- | --- | --- |
| `query` | `object` | .. include:: /includes/triggers/parameter-query.rst |
| `replacement` | `object` | .. include:: /includes/triggers/parameter-replacement.rst |
| `options` | `object` | An object that specifies additional configuration options. |
| `options.upsert` | `boolean` | .. include:: /includes/triggers/parameter-options-upsert.rst |
| `options.sort` | `boolean` | .. include:: /includes/triggers/parameter-options-sort.rst |
| `options.projection` | `boolean` | .. include:: /includes/triggers/parameter-projection.rst |
| `options.returnNewDocument` | `boolean` | .. include:: /includes/triggers/parameter-options-returnNewDocument.rst |
| `options.session` | `ClientSession` | .. include:: /includes/triggers/parameter-options-session.rst |

### Return Value

The `collection.findOneAndReplace()` method returns a Promise that resolves to a single document that the query overwrote. If no documents match the specified query, the promise resolves to `null`.

```typescript
Promise<object | null>
```

> **Note:**
> You can specify whether to return the pre-replacement or post-replacement version of the document by setting the value of `options.returnNewDocument`. By default, `returnNewDocument` is `false`, which indicates that the promise should resolve to the *pre-update* version of the document.

## collection.findOneAndDelete()

Removes a single document from a collection and returns the deleted document as it was immediately before it was deleted.

Unlike `collection.updateOne()`, this action allows you to atomically find, modify, and return a document with the same command. This avoids the risk of other update operations changing the document between separate find and update operations.

```javascript
// Find the first document that has a quantity greater than 25
const query = { "quantity": { "$gte": 25 } };
// Sort the documents in order of descending quantity before
// deleting the first one.
const options = {
  "sort": { "quantity": -1 }
}

return itemsCollection.findOneAndDelete(query, options)
  .then(deletedDocument => {
    if(deletedDocument) {
      console.log(`Successfully deleted document that had the form: ${deletedDocument}.`)
    } else {
      console.log("No document matches the provided query.")
    }
    return deletedDocument
  })
  .catch(err => console.error(`Failed to find and delete document: ${err}`))
```

### Parameters

```typescript
findOneAndDelete(
   query: object,
   options?: object
): Promise<object | null>
```

| Parameter | Type | Description |
| --- | --- | --- |
| `query` | `object` | .. include:: /includes/triggers/parameter-query.rst |
| `options` | `object` | An object that specifies additional configuration options. |
| `options.sort` | `boolean` | .. include:: /includes/triggers/parameter-options-sort.rst |
| `options.projection` | `boolean` | .. include:: /includes/triggers/parameter-projection.rst |
| `options.session` | `ClientSession` | .. include:: /includes/triggers/parameter-options-session.rst |

### Return Value

The `collection.findOneAndDelete()` method returns a Promise that resolves to a single document that the query deleted. If no documents match the specified query, the promise resolves to `null`.

```typescript
Promise<object | null>
```

## collection.insertOne()

Inserts a single document into a collection and returns the `_id` of the inserted document.

```javascript
const newItem = {
  "name": "Plastic Bricks",
  "quantity": 10,
  "category": "toys",
  "reviews": [{ "username": "legolover", "comment": "These are awesome!" }]
};

itemsCollection.insertOne(newItem)
  .then(result => console.log(`Successfully inserted item with _id: ${result.insertedId}`))
  .catch(err => console.error(`Failed to insert item: ${err}`))
```

### Parameters

```typescript
insertOne(document: object): Promise<object>
```

| Parameter | Type | Description |
| --- | --- | --- |
| `document` | `object` | A document to insert into the collection. |

### Return Value

The `collection.insertOne()` method returns a Promise that resolves to a document that describes the insert operation.

```typescript
Promise<object>
```

| Value | Type | Description |
| --- | --- | --- |
| `result.insertedId` | `string` | The `_id` value of the document that the insert operation added to the collection. |

## collection.insertMany()

Inserts one or more documents into a collection and returns a list that contains the `_id` value for each inserted document.

```javascript
const doc1 = { "name": "basketball", "category": "sports", "quantity": 20, "reviews": [] };
const doc2 = { "name": "football",   "category": "sports", "quantity": 30, "reviews": [] };

return itemsCollection.insertMany([doc1, doc2])
  .then(result => {
    console.log(`Successfully inserted ${result.insertedIds.length} items!`);
    return result
  })
  .catch(err => console.error(`Failed to insert documents: ${err}`))
```

### Parameters

```typescript
insertMany(
  document: object,
  options?:  { ordered?: boolean },
): Promise<object>
```

| Parameter | Type | Description |
| --- | --- | --- |
| `documents` | `object` | An array of documents to insert into the collection. |
| `options` | `object` | An object that specifies additional configuration options. |
| `options.ordered` | `boolean` | Optional. A boolean specifying whether the [mongod](/reference/program/mongod/#mongodb-binary-bin.mongod) instance should perform an ordered or unordered insert. Defaults to `true`. |

### Return Value

The `collection.insertMany()` method returns a Promise that resolves to a document that describes the insert operation.

```typescript
Promise<object>
```

| Value | Type | Description |
| --- | --- | --- |
| `result.insertedIds: Array<ObjectID>` | `string` | An array that contains the `_id` values for all documents that the insert operation added to the collection in the order that they were passed to the method. |

## collection.updateOne()

Updates a single document in a collection and returns metadata about the operation.

```javascript
const query = { "name": "football" };
const update = {
  "$push": {
    "reviews": {
      "username": "tombradyfan",
      "comment": "I love football!!!"
    }
  }
};
const options = { "upsert": false };

itemsCollection.updateOne(query, update, options)
  .then(result => {
    const { matchedCount, modifiedCount } = result;
    if(matchedCount && modifiedCount) {
      console.log(`Successfully added a new review.`)
    }
  })
  .catch(err => console.error(`Failed to add review: ${err}`))
```

### Parameters

```typescript
updateOne(
   query: object,
   update: object,
   options?: object
): Promise<object>
```

| Parameter | Type | Description |
| --- | --- | --- |
| `query` | `object` | .. include:: /includes/triggers/parameter-query.rst |
| `update` | `object` | .. include:: /includes/triggers/parameter-update.rst |
| `options` | `object` | An object that specifies additional configuration options. |
| `options.upsert` | `boolean` | .. include:: /includes/triggers/parameter-options-upsert.rst |
| `options.session` | `ClientSession` | .. include:: /includes/triggers/parameter-options-session.rst |

### Return Value

The `collection.updateOne()` method returns a Promise that resolves to a document that describes the update operation.

```typescript
Promise<object>
```

| Value | Type | Description |
| --- | --- | --- |
| `result.matchedCount` | `number` | The number of documents in the collection that match the provided query. |
| `result.modifiedCount` | `number` | The number of documents in the collection that were modified by the update operation. |
| `result.upsertedId` | `string` | The `_id` value of the document inserted by an upsert operation. This value is only present when the `upsert` option is enabled and the update query does not match any documents. |

## collection.updateMany()

Updates one or more documents in a collection and returns metadata about the operation.

```javascript
const query = {};
const update = { "$mul": { "quantity": 10 } };
const options = { "upsert": false }

return itemsCollection.updateMany(query, update, options)
  .then(result => {
    const { matchedCount, modifiedCount } = result;
    console.log(`Successfully matched ${matchedCount} and modified ${modifiedCount} items.`)
    return result
  })
  .catch(err => console.error(`Failed to update items: ${err}`))
```

### Parameters

```typescript
updateMany(
   query: object,
   update: object,
   options?: object
): Promise<object>
```

| Parameter | Type | Description |
| --- | --- | --- |
| `query` | `object` | .. include:: /includes/triggers/parameter-query.rst |
| `update` | `object` | .. include:: /includes/triggers/parameter-update.rst |
| `options` | `object` | An object that specifies additional configuration options. |
| `options.upsert` | `boolean` | .. include:: /includes/triggers/parameter-options-upsert.rst |
| `options.session` | `ClientSession` | .. include:: /includes/triggers/parameter-options-session.rst |

### Return Value

The `collection.updateMany()` method returns a Promise that resolves to a document that describes the update operation.

```typescript
Promise<object>
```

| Value | Type | Description |
| --- | --- | --- |
| `result.matchedCount` | `number` | The number of documents in the collection that match the provided query. |
| `result.modifiedCount` | `number` | The number of documents in the collection that were modified by the update operation. |
| `result.upsertedId` | `string` | The `_id` value of the document inserted by an upsert operation. This value is only present when the `upsert` option is enabled and the update query does not match any documents. |

## collection.deleteOne()

Removes a single document from a collection.

```javascript
const query = { "name": "lego" };

itemsCollection.deleteOne(query)
  .then(result => console.log(`Deleted ${result.deletedCount} item.`))
  .catch(err => console.error(`Delete failed with error: ${err}`))
```

### Parameters

```typescript
deleteOne(
   query: object,
   options?: object
): Promise<object>
```

| Parameter | Type | Description |
| --- | --- | --- |
| `query` | `object` | .. include:: /includes/triggers/parameter-query.rst |
| `options` | `object` | An object that specifies additional configuration options. |
| `options.session` | `ClientSession` | .. include:: /includes/triggers/parameter-options-session.rst |

### Return Value

The `collection.deleteOne()` method returns a Promise that resolves to a document that describes the delete operation.

```typescript
Promise<object>
```

| Value | Type | Description |
| --- | --- | --- |
| `result.deletedCount` | `number` | The number of documents in the collection that were deleted by the delete operation. |

## collection.deleteMany()

Remove one or more documents from a collection.

```javascript
const query = { "reviews": { "$size": 0 } };

itemsCollection.deleteMany(query)
  .then(result => console.log(`Deleted ${result.deletedCount} item(s).`))
  .catch(err => console.error(`Delete failed with error: ${err}`))
```

### Parameters

```typescript
deleteMany(
   query: object,
   options?: object
): Promise<object>
```

| Parameter | Type | Description |
| --- | --- | --- |
| `query` | `object` | .. include:: /includes/triggers/parameter-query.rst |
| `options` | `object` | An object that specifies additional configuration options. |
| `options.session` | `ClientSession` | .. include:: /includes/triggers/parameter-options-session.rst |

### Return Value

The `collection.deleteMany()` method returns a Promise that resolves to a document that describes the delete operation.

```typescript
Promise<object>
```

| Value | Type | Description |
| --- | --- | --- |
| `result.deletedCount` | `number` | The number of documents in the collection that were deleted by the delete operation. |

## collection.aggregate()

Executes an [aggregation](/aggregation) pipeline and returns a cursor that allows you to access the pipeline's output documents.

```javascript
const pipeline = [
  { "$group": {
      "_id": "$customerId",
      "numPurchases": { "$sum": 1 },
      "numItemsPurchased": { "$sum": { "$size": "$items" } }
  } },
  { "$addFields": {
      "averageNumItemsPurchased": {
        "$divide": ["$numItemsPurchased", "$numPurchases"]
      }
  } }
]

return purchasesCollection.aggregate(pipeline).toArray()
  .then(customers => {
    console.log(`Successfully grouped purchases for ${customers.length} customers.`)
    for(const customer of customers) {
      console.log(`customer: ${customer._id}`)
      console.log(`num purchases: ${customer.numPurchases}`)
      console.log(`total items purchased: ${customer.numItemsPurchased}`)
      console.log(`average items per purchase: ${customer.averageNumItemsPurchased}`)
    }
    return customers
  })
  .catch(err => console.error(`Failed to group purchases by customer: ${err}`))
```

### Parameters

```typescript
aggregate(
   pipeline: object[],
   options?: object
): Cursor
```

| Parameter | Type | Description |
| --- | --- | --- |
| `pipeline` | `object[]` | An array of one or more [aggregation pipeline stages](/reference/operator/aggregation-pipeline). All aggregation pipeline stages are available except for $indexStats. |
| `options` | `object` | An object that specifies additional configuration options. |
| `options.session` | `ClientSession` | .. include:: /includes/triggers/parameter-options-session.rst |

### Return Value

The `collection.aggregate()` method returns a cursor object that points to any documents output from the final stage of the aggregation pipeline. You can manipulate and access documents in the aggregation result set with the following methods:

| Method | Description |
| --- | --- |
| `cursor.next()` | Iterates the cursor and returns a Promise that resolves to the next document in the cursor. If the cursor is exhausted, the promise resolves to `undefined`. collection.aggregate(pipeline).next() .then(doc => console.log("next document", doc)) |
| `cursor.toArray()` | Iterates the cursor to exhaustion and returns a Promise that resolves to an array that contains all of the iterated documents. collection.aggregate(pipeline).toArray() .then(docs => console.log("all documents", docs)) |
| `cursor.skip(amount)` | Specifies a number of matching documents to omit from the aggregation result set. MongoDB omits documents from the result set in sort order until it has skipped the specified number. You can't call this method after retrieving one or more documents using `cursor.next()` or `cursor.toArray()`. |

> **Note:**
> You can't return a cursor from a function. Instead, evaluate the cursor using `cursor.next()` or `cursor.toArray()` and return the result.

## collection.count()

Returns the number of documents in a collection or view that match a given query.

```javascript
return itemsCollection.count({ "reviews.0": { "$exists": true } })
  .then(numDocs => console.log(`${numDocs} items have a review.`))
  .catch(err => console.error("Failed to count documents: ", err))
```

### Parameters

```typescript
count(
   query?: object,
   options?: object
): Promise<number>
```

| Parameter | Type | Description |
| --- | --- | --- |
| `query` | `object` | Optional. |
| `options` | `object` | An object that specifies additional configuration options. |
| `options.session` | `ClientSession` | Optional. A session object that represents the transaction context in which the operation occurs. To learn more, see Transactions. |

### Return Value

The `collection.count()` method returns a Promise that resolves to the integer number of documents in the collection that match the query.

```javascript
Promise<number>
```

| Value | Description |
| --- | --- |
| **Count Result** | `numDocs: <integer>` | The number of documents in the collection that match the provided query. |

## collection.distinct()

Finds documents that match a given query filter and returns a list of distinct values for a specific field across all matched documents.

```javascript
const taskCollection = context.services.get("mongodb-atlas")
  .db("tracker").collection("tasks");

return taskCollection.distinct("status", {})
  .then(results => {
      console.log(JSON.stringify(results));
      console.log(results.length);
  })
  .catch(err => console.error(err))
```

### Parameters

```typescript
distinct(
   field: string,
   query: object,
   options?: object
): Promise<any[]>
```

| Parameter | Type | Description |
| --- | --- | --- |
| `field` | `string` | The name of the field in each document from which to find distinct values. |
| `query` | `object` | .. include:: /includes/triggers/parameter-query.rst |
| `options` | `object` | An object that specifies additional configuration options. |
| `options.session` | `ClientSession` | .. include:: /includes/triggers/parameter-options-session.rst |

### Return Value

The `collection.distinct()` method returns a Promise that resolves to an array of distinct values.

```typescript
Promise<any[]>
```

## collection.bulkWrite()

Runs multiple insert, update, and delete operations on a collection with a single call. Within the `bulkWrite()` function, you can specify one or more of the following write operations:

- insertOne
- updateOne
- updateMany
- deleteOne
- deleteMany
- replaceOne

> **Note:**
> A bulk write can only operate on a single collection.

```javascript
exports = async function(arg){
    const doc1 = { "name": "velvet elvis", "quantity": 20, "reviews": [] };
    const doc2 = { "name": "mock turtleneck",  "quantity": 30, "reviews": [] };

    var collection = context.services.get("mongodb-atlas")
        .db("store")
        .collection("purchases");

    return await collection.bulkWrite(
        [{ insertOne: doc1}, { insertOne: doc2}], 
        {ordered:true});
};
```

### Parameters

```typescript
bulkWrite(
  operations: object[],
  options?: object
): Promise<null>
```

| Parameter | Type | Description |
| --- | --- | --- |
| `operations` | `object[]` | An array of bulkWrite operations to perform. Examples of supported operations include the following: { insertOne: { document: { a: 1 } } } { updateOne: { filter: {a:2}, update: {$set: {a:2}}, upsert:true } } { updateMany: { filter: {a:2}, update: {$set: {a:2}}, upsert:true } } { deleteOne: { filter: {c:1} } } { deleteMany: { filter: {c:1} } } { replaceOne: { filter: {c:3}, replacement: {c:4}, upsert:true}} |
| `options` | `object` | An object that specifies additional configuration options. |
| `options.ordered` | `boolean` | .. include:: /includes/triggers/parameter-options-ordered.rst |
| `options.bypassDocumentValidation` | `boolean` | .. include:: /includes/triggers/parameter-options-bypassDocumentValidation.rst |
| `options.session` | `ClientSession` | .. include:: /includes/triggers/parameter-options-session.rst |

### Return Value

The `collection.bulkWrite()` function returns a Promise that resolves to `null`.

```typescript
Promise<null>
```
