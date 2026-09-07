# Delete Documents

The MongoDB shell provides the following methods to delete documents
from a collection:

- To delete multiple documents, use
  `db.collection.deleteMany()`.
- To delete a single document, use `db.collection.deleteOne()`.

## Delete All Documents

To delete all documents from a collection, pass an empty
filter document `{}` to the
`db.collection.deleteMany()` method.

> **Example**
>
> To delete all documents from the `sample_mflix.movies` collection:
>
> ```javascript
use sample_mflix

db.movies.deleteMany({})
 
```
>
> The method returns a document with the status of the operation. For
> more information and examples, see
> `deleteMany()`.

> **Note**
>
> If you want to delete all documents from a large collection, dropping
> with the `db.collection.drop` method. and recreating the
> collection may have faster performance than deleting documents with
> the `db.collection.deleteMany()` method. When you recreate
> the collection, you must also recreate any specified collection
> parameters such as collection indexes.

## Delete All Documents that Match a Condition

You can specify criteria, or filters, that identify the documents to
delete. The filters use the same syntax
as read operations.

To specify equality conditions, use `<field>:<value>` expressions in
the query filter document.

To delete all documents that match a deletion criteria, pass a filter
parameter to the `deleteMany()` method.

> **Example**
>
> To delete all documents from the `sample_mflix.movies` collection
> where the `title` equals `"Titanic"`:
>
> ```javascript
use sample_mflix

db.movies.deleteMany( { title: "Titanic" } )

```
>
> The method returns a document with the status of the operation. For
> more information and examples, see
> `deleteMany()`.

## Delete Only One Document that Matches a Condition

To delete at most a single document that matches a specified filter
(even though multiple documents may match the specified filter) use the
`db.collection.deleteOne()` method.

> **Example**
>
> To delete the *first* document from the `sample_mflix.movies`
> collection where the `cast` array contains `"Brad Pitt"`:
>
> ```javascript
use sample_mflix

db.movies.deleteOne( { cast: "Brad Pitt" } )

```

## Delete Behavior

To learn more about the specific behavior of deleting documents,
see \[Behavior\](<https://www.mongodb.com/docs/manual/tutorial/remove-documents/#delete-behavior/>).

## Learn More

- To see additional examples of deleting documents, see the following
  method pages:
  - `db.collection.deleteMany()`
  - `db.collection.deleteOne()`
- To see all available methods to delete documents, see
  \[Delete Methods\](<https://www.mongodb.com/docs/manual/reference/delete-methods/>).
