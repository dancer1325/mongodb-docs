# Aggregation Pipeline Limits

Aggregation operations with the `aggregate` command have the
following limitations.

## Result Size Restrictions

The `aggregate` command can either return a cursor or store
the results in a collection. Each document in the result set is subject
to the 16 mebibyte `BSON Document Size limit`. If any single document exceeds the `BSON Document Size limit`, the aggregation produces an error. The
limit only applies to the returned documents. During the pipeline
processing, the documents may exceed this size. The
`db.collection.aggregate()` method returns a cursor by default.

## Number of Stages Restrictions

## Memory Restrictions
