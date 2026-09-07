# Aggregation Pipeline

When you run aggregation pipelines on {+atlas+} deployments in the
{+atlas+} UI, you can preview the results at each stage.

## Complete Aggregation Pipeline Examples

The aggregation-complete-examples section contains complete
tutorials that provide detailed explanations of common aggregation tasks
in a step-by-step format. The tutorials include examples for MongoDB
Shell and each of the `official MongoDB drivers`.

## Additional Aggregation Pipeline Stage Details

An aggregation pipeline consists of one or more stages that process documents:

- A stage does not have to output one document for every input
  document. For example, some stages may produce new documents or
  filter out documents.
- The same stage can appear multiple times in the pipeline with these
  stage exceptions: `\$out`, `\$merge`, and
  `\$geoNear`.

For all aggregation stages, see
aggregation-pipeline-operator-reference.

### Expressions and Operators

Some aggregation pipeline stages accept expressions. Operators calculate values based on input expressions.

### Field Paths

Field path expressions are used to access fields in
input documents. To specify a field path, prefix the field name or the
dotted field path (if the field is in an
embedded document) with a dollar sign `$`. For example, `"$user"` to
specify the field path for the `user` field or `"$user.name"` to
specify the field path to the embedded `"user.name"` field.

`"$<field>"` is equivalent to `"$$CURRENT.<field>"` where the
`CURRENT` is a system variable that defaults to the root of
the current object, unless stated otherwise in specific stages.

For more information and examples, see agg-field-paths.

## Run an Aggregation Pipeline

To run an aggregation pipeline, use:

- `db.collection.aggregate()` or
- `aggregate`

## Update Documents Using an Aggregation Pipeline

To update documents with an aggregation pipeline, use:

## Other Considerations

### Aggregation Pipeline Limitations

An aggregation pipeline has limitations on the value types and the
result size. See /core/aggregation-pipeline-limits.

### Aggregation Pipelines and Sharded Collections

An aggregation pipeline supports operations on sharded collections.
See aggregation-pipeline-sharded-collection.

### Aggregation Pipelines as an Alternative to Map-Reduce

## Learn More

To learn more about aggregation pipelines, see:

- aggregation-expression-operators
- aggregation-pipeline-operator-reference
