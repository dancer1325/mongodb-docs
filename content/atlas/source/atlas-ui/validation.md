# Set Validation Rules for Your Schema

## Validation Tab

The **Validation** tab allows you to manage [schema validation rules](/core/document-validation) for a collection. Schema validation ensures that all documents in a collection follow a defined set of rules, such as conforming to a specific shape or only allowing a specified range of values in fields.

![Validation view](/images/atlas-ui/compass/validation-view.png)

## View the **Validation** Tab

1. In Atlas, go to the **Data Explorer** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Data Explorer** under the **Database** heading. The [Data Explorer](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fmetrics%2FreplicaSet%2F%3Creplset%3E%2Fexplorer) displays.

IMPORTANT: You can also go to the **Clusters** page, and click **Data Explorer** under the **Shortcuts** heading.

1. Go to the **Validation** tab.

   1. Select the collection.
   2. Click the **Validation** tab.

## Validation Rules

The validation editor supports [JSON Schema validation](/core/schema-validation/#json-schema), and validation with query expressions using [query operators](/reference/operator/query). After you click the **Update** button, Atlas updates to display a document from your collection that passes the validation and a document that fails.

### JSON Schema Validation

To specify JSON Schema validation, use the [$jsonSchema](/reference/operator/query/jsonSchema) operator.

```javascript
{
   $jsonSchema: {
      required: ['name', 'borough'], // the name and borough fields are required
      properties: {
         cuisine: {
            bsonType: "string",
            description: "must be a string"
         }
      }
   }
}
```

The [$jsonSchema](/reference/operator/query/jsonSchema) operator supports various keywords to specify validation rules. For example:

- The `required` array defines required fields in your document.
- The `properties` object defines rules for specific document fields.

Consider the following example validation:

```javascript
{
   $jsonSchema: {
      bsonType: "object",
      required: [ "address", "borough", "name" ],
      properties: {
         address: {
            bsonType: "object",
            properties: {
               coord: {
                  bsonType: "array",
                  items: [
                     {
                        bsonType: "double",
                        minimum: -180,
                        maximum: 180,
                        exclusiveMaximum: false,
                        description: "must be a number in [ -180, 180 ]"
                     },
                     {
                        bsonType: "double",
                        minimum: -90,
                        maximum: 90,
                        exclusiveMaximum: false,
                        description: "must be a number in [ -90, 90 ]"
                     }
                  ]
               }
            },
            description: "must be an object"
         },
         borough: {
            bsonType: "string",
            enum: [ "Manhattan", "Brooklyn", "Queens", "Bronx", "Staten Island" ],
            description: "must be one of the enum strings"
         }
      }
   }
}
```

This validation specifies:

- The list of [required](/reference/operator/query/jsonSchema/#available-keywords) fields.
- The [bsonType](/reference/operator/query/jsonSchema/#available-keywords) for all required fields.
- The [minimum](/reference/operator/query/jsonSchema/#available-keywords) and [maximum](/reference/operator/query/jsonSchema/#available-keywords) values in the `address.coord` array.
- The acceptable values for the  `borough` field, using [enum](/reference/operator/query/jsonSchema/#available-keywords).

For all available `$jsonSchema` keywords, refer to the [$jsonSchema](/reference/operator/query/jsonSchema) page in the MongoDB manual.

### Validation using Query Operators

You can also specify validation using [query operators](/reference/operator/query), with the exception of the following query operators: `$near`, `$nearSphere`, `$text`, and `$where`.

```javascript
{ 
   $or: [
      { name: { $type: "string" } },
      { borough: {
            bsonType: "string",
            enum: [ "Manhattan", "Brooklyn", "Queens", "Bronx", "Staten Island" ],
            description: "must be one of the enum strings"
      } }
   ]
}
```

Using this validation, *one* of the following must be true:

- The `name` field must be BSON type string.
- The `borough` field must be one of the enum strings.

## Validation Actions and Levels

At the top, specify a **Validation Action** and **Validation Level**:

- The validation action determines whether to `warn` but accept invalid documents, or `error` and reject invalid documents.
- The validation level determines how strictly MongoDB applies validation rules to existing documents.

   - `Strict` validation applies your rules to all document inserts and updates.
   - `Moderate` validation only applies your rules to new documents and existing valid documents. Existing invalid documents are not affected.

For details on validation actions and levels, see [Specify Validation Rules](/core/schema-validation/#specify-validation-rules) in the MongoDB manual.

> **See also:**
> - [Schema Validation](/core/schema-validation/)

## Limitations

The **Validation** tab is not available if you are connected to [Atlas Data Federation](/data-federation).
