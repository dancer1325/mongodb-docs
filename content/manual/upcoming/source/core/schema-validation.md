# Schema Validation

Schema validation lets you create validation rules for your fields, such
as allowed data types and value ranges.

MongoDB uses a flexible schema model, which means that documents in a
collection do not need to have the same fields or data types by default.
Once you've established an application schema, you can use schema
validation to ensure there are no unintended schema changes or improper
data types.

## When to Use Schema Validation

## When MongoDB Checks Validation

After you add schema validation rules to a collection:

- All document inserts must match the rules.
- The schema validation level defines how the rules are applied to
  existing documents and document updates. To learn more, see
  schema-specify-validation-level.

To find documents in a collection that don't match the schema validation
rules, see use-json-schema-query-conditions-find-documents.

## What Happens When a Document Fails Validation

By default, when an insert or update operation would result in an
invalid document, MongoDB rejects the operation and does not write the
document to the collection.

Alternatively, you can configure MongoDB to allow invalid documents and
log warnings when schema violations occur.

To learn more, see schema-validation-handle-invalid-docs.

## Get Started

For common tasks involving schema validation, see the following pages:

- schema-validation-json
- schema-validation-polymorphic-collections
- schema-validation-query-expression
- schema-allowed-field-values
- schema-view-validation-rules
- schema-update-validation
- use-json-schema-query-conditions
- schema-bypass-document-validation

## Learn More

To learn about MongoDB's flexible schema model, see
manual-data-modeling-intro.
