# Insert Documents

This page provides examples of insert operations in MongoDB.

You can insert documents in MongoDB by using the following
methods:

> **Note Creating a Collection**
>
> If the collection does not currently exist, insert operations will
> create the collection.

## Insert Documents in the {+atlas+} UI

To insert a document in the {+atlas+} UI, complete the following steps.
To learn more about working with documents in the {+atlas+} UI, see
Create, View, Update, and Delete Documents.

**Navigate to the collection**

1.  For the cluster to which you want to add documents,
    click Browse Collections.
2.  In the left navigation pane, select the database.
3.  In the left navigation pane, select the collection.

**Add the documents**

1.  Click Insert Document.
2.  Click the {} icon, which opens the JSON view.
3.  Paste the document array into the text entry field. For
    example, the following entry creates four documents, each of
    which contain three fields:

```
[
   { "prodId": 100, "price": 20, "quantity": 125 },
   { "prodId": 101, "price": 10, "quantity": 234 },
   { "prodId": 102, "price": 15, "quantity": 432 },
   { "prodId": 103, "price": 17, "quantity": 320 }
]
```

**Click Insert.**

{+atlas+} adds the documents to the collection.

## Insert a Single Document

## Insert Multiple Documents

## Insert Behavior

### Collection Creation

If the collection does not currently exist, insert operations
create the collection.

### `_id` Field

### Atomicity

All write operations in MongoDB are atomic on the level of a single
document. For more information on MongoDB and atomicity, see
/core/write-operations-atomicity.

### Write Acknowledgement

With write concerns, you can specify the level of acknowledgment
requested from MongoDB for write operations. For more information, see
/reference/write-concern.
