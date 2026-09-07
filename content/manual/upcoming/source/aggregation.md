# Aggregation Operations

Aggregation operations process multiple documents and return computed
results. You can use aggregation operations to:

- Group values from multiple documents together.
- Perform operations on the grouped data to return a single result.
- Analyze data changes over time.
- Query the most up-to-date version of your data.

By using the built-in aggregation operators in MongoDB, you can perform
analytics on your cluster without having to move your data to another platform.

## Get Started

To perform aggregation operations, you can use:

- Aggregation pipelines, which are
  the preferred method for performing aggregations.
- Single purpose aggregation methods, which are simple but lack the
  capabilities of an aggregation pipeline.

## Aggregation Pipelines

### Aggregation Pipeline Example

This pipeline finds the top three directors who have directed
the most movies in the database.

First, add a `\$match` stage to filter the documents to movies
that have directors listed (excluding documents where directors field is null or empty):

The `$match` stage reduces the number of documents in our pipeline by
filtering out movies without director information. Next, use `\$unwind`
to deconstruct the directors array so we can count movies per individual director:

Then, `\$group` the documents by director name and count
the number of movies each director has made:

To find the directors with the most movies, use the `\$sort`
stage to sort the remaining documents in descending order by movie count:

After you sort your documents, use the `\$limit` stage to return the
top three directors who have directed the most movies:

The full pipeline is given in this example:

This pipeline returns these results:

For runnable examples containing sample input documents, see
Complete Aggregation Pipeline Examples.

### Learn More About Aggregation Pipelines

To learn more about aggregation pipelines, see
aggregation-pipeline.

## Single Purpose Aggregation Methods

The single purpose aggregation methods aggregate documents from a single
collection. The methods are simple but lack the capabilities of an
aggregation pipeline.

- - Method
  - Description
- - `db.collection.estimatedDocumentCount()`
  - Returns an approximate count of the documents in a collection or
    a view.
- - `db.collection.count()`
  - Returns a count of the number of documents in a collection or a
    view.
- - `db.collection.distinct()`
  - Returns an array of documents that have distinct values for the
    specified field.
