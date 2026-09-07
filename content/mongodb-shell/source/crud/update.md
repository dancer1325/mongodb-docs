# Update Documents

The MongoDB shell provides the following methods to update documents in
a collection:

- To update a single document, use
  `db.collection.updateOne()`.
- To update multiple documents, use
  `db.collection.updateMany()`.
- To replace a document, use
  `db.collection.replaceOne()`.

## Update Operator Syntax

To update a document, MongoDB provides
\[update operators\](<https://www.mongodb.com/docs/manual/reference/operator/update/>), such
as `\$set`, to modify field values.

To use the update operators, pass to the update methods an
update document of the form:

```javascript
{
  <update operator>: { <field1>: <value1>, ... },
  <update operator>: { <field2>: <value2>, ... },
  ...
}

```

Some update operators, such as `\$set`, create the field if
the field does not exist. See the individual
\[update operator\](<https://www.mongodb.com/docs/manual/reference/operator/update/>) reference for
details.

## Update a Single Document

Use the `db.collection.updateOne()` method to update the *first*
document that matches a specified filter.

> **Example**
>
> To update the *first* document in the `sample_mflix.movies`
> collection where `title` equals `"Twilight"`:
>
> ```javascript
use sample_mflix

db.movies.updateOne( { title: "Twilight" },
{
  $set: {
    plot: "A teenage girl risks everything–including her life–when she falls in love with a vampire."
  },
  $currentDate: { lastUpdated: true } 
})

```
>
> The update operation:
>
> - Uses the `\$set` operator to update the value of the
>   `plot` field for the movie `Twilight`.
>
> \- Uses the `\$currentDate` operator to update the value  
> of the `lastUpdated` field to the current date. If
> `lastUpdated` field does not exist,
> `\$currentDate` will create the field. See
> `\$currentDate` for details.

## Update Multiple Documents

Use the `db.collection.updateMany()` to update all documents
that match a specified filter.

> **Example**
>
> To update all documents in the `sample_airbnb.listingsAndReviews`
> collection to update where `security_deposit` is less than `100`:
>
> ```javascript
use sample_airbnb

db.listingsAndReviews.updateMany(
  { security_deposit: { $lt: 100 } },
  {
    $set: { security_deposit: 100, minimum_nights: 1 }
  }
)

```
>
> The update operation uses the `\$set` operator to update the
> value of the `security_deposit` field to `100` and the value of
> the `minimum_nights` field to `1`.

## Replace a Document

To replace the entire content of a document except for the `_id`
field, pass an entirely new document as the second argument to
`db.collection.replaceOne()`.

When replacing a document, the replacement document must contain only
field/value pairs. Do not include \[update operators\](<https://www.mongodb.com/docs/manual/reference/operator/update/>) expressions.

The replacement document can have different fields from the original
document. In the replacement document, you can omit the `_id` field
since the `_id` field is immutable; however, if you do include the
`_id` field, it must have the same value as the current value.

> **Example**
>
> To replace the *first* document from the
> `sample_analytics.accounts` collection where
> `account_id: 371138`:
>
> ```javascript
db.accounts.replaceOne(
  { account_id: 371138 },
  { account_id: 893421, limit: 5000, products: [ "Investment", "Brokerage" ] }
)

```
>
> Run the following command to read your updated document:
>
> ```javascript
db.accounts.findOne( { account_id: 893421 } )
```

## Update Behavior

To learn more about the specific behavior of updating documents,
see \[Behavior\](<https://www.mongodb.com/docs/manual/tutorial/update-documents/#behavior/>).

## Learn More

- To learn how to update documents using an aggregation pipeline, see
  \[Updates with Aggregation Pipeline\](<https://www.mongodb.com/docs/manual/tutorial/update-documents-with-aggregation-pipeline/>).
- To see all available methods to update documents, see
  \[Update Methods\](<https://www.mongodb.com/docs/manual/reference/update-methods/>).
