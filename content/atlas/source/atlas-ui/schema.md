# Analyze Your Data Schema

The **Schema** tab provides an overview of the data type and shape of the fields in a particular collection. Databases and collections are visible in the left-side navigation. The overview is based on sampling the documents in the collection. The schema overview may include additional data about the contents of the fields, such as the minimum and maximum values of dates and integers, the frequency of occurrence of particular values, and the cardinality of the data. MongoDB has a [flexible schema model](/core/data-modeling-introduction/), which means that some fields may contain different types of data from one document to the next. For example, a field named `address` may contain strings and integers in some documents, objects in others, or some combination of all three. In the case of heterogenous fields, the **Schema** tab shows a breakdown of the various data types contained within the field with the percentage of each data type represented.

> **Example:**
> The **Schema** tab shows size information about the `test.restaurants` collection at the top, including the total number of documents in the collection, the average document size, and the total disk space occupied by the collection. The following fields are shown with details:
>
> - The `_id` field is an [ObjectId](/reference/bson-types/index.html#objectid). Each ObjectId contains a timestamp, so Atlas displays the range of creation times for the sampled documents.
> - The `address` field contains four nested fields. You can expand the field panel to see analyses of each of the nested fields.
> - The `borough` field contains a string indicating the borough in which the restaurant is located. The cardinality is low enough that Atlas can provide a graded bar of the field contents, with the most-frequently occurring string on the left.
> - The `grades` field contains arrays of strings. The analysis shows the minimum, maximum, and average array lengths.
>
> ![Example of a collection's schema](/images/atlas-ui/compass/collection-schema.png)
>

## View the **Schema** Tab

1. In Atlas, go to the **Data Explorer** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Data Explorer** under the **Database** heading. The [Data Explorer](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fmetrics%2FreplicaSet%2F%3Creplset%3E%2Fexplorer) displays.

IMPORTANT: You can also go to the **Clusters** page, and click **Data Explorer** under the **Shortcuts** heading.

1. Go to the **Schema** tab.

   1. Select the collection.
   2. Click the **Schema** tab.

## Query Bar

Using the query bar in the **Schema** tab, you can create a query filter to limit your result set. Click the **Options** button to specify query options, such as the particular fields to display and the number of results to return.

> **Note:**
> *[Contenido incluido desde: /atlas-ui/includes/shared/extracts/query-bar-results.rst]*
>

![Query bar schema view](/images/atlas-ui/compass/query-bar-schema-view.png)

> **Tip:**
> In the **Schema** tab, you can also use the atlas-ui-build-query to enter a query into the query bar.

## Field Descriptions

For each field, Atlas displays summary information about the data type or types the field contains and the range of values. Depending on the data type and the level of cardinality, Atlas displays histograms, graded bars, geographical maps, and sample data to provide a sense of the shape and scope of the data contained in each field.

### Field with a Single Data Type

Below is an example of the data type summary for a field called `last_login` which contains data of type `date`.

![Example of a field with a single data type](/images/atlas-ui/compass/field-example.png)

### Field with Multiple Data Types

For fields that contain multiple data types, Atlas displays a percentage breakdown of the various data types across documents. In the example below, the chart shows the contents of a field called `phone_no` in which 20% of documents are of type `int32`, and the remaining 80% are of type `string`.

![Example of percentage breakdown for data types](/images/atlas-ui/compass/field-percentage-breakdown.png)

### Missing Field

If a collection contains documents in which not all fields contain a value, the missing values display as `undefined`. In the example below, the field `age` has no recorded value in 40% of the sampled documents.

![Example of sparcely applied data type](/images/atlas-ui/compass/field-sparsity.png)

### Strings

Strings can appear in three different ways. If there are entirely unique strings in a field, Atlas shows a random selection of string values from the specified field. Click the circular refresh icon to see a new set of randomly selected values from the field.

![Example of string data types](/images/atlas-ui/compass/string-sample.png)

If there are only a few different string values, Atlas shows the strings in a single graded bar which shows the percentage of the population of the string values.

![Example of few string data types](/images/atlas-ui/compass/string-sample2.png)

If there are multiple string values with some duplicates, Atlas shows a histogram indicating the frequency of each string found within the field.

![Example of string data types as a histogram](/images/atlas-ui/compass/string-sample3.png)

> **Note:**
> Move the mouse over each bar to display a tooltip which shows the value of the string.

### Numbers

Numbers are similar to strings in their representation. Unique numbers are shown in the following manner:

![Example of number data type](/images/atlas-ui/compass/number-sample.png)

Duplicate numbers are shown in a histogram that indicates their frequency:

![Example of duplicate number data types](/images/atlas-ui/compass/number-sample2.png)

### Dates and ObjectIDs

Fields that represent dates (and fields that contain the ObjectID data type, which includes a timestamp) are shown across multiple bar charts. The two charts on the top row represent the day of the week and time of day of the timestamp value. The single chart on the bottom shows the first and last timestamp value, and the vertical lines represent the distribution of the timestamp across the range of first to last.

![Example of Date data types](/images/atlas-ui/compass/date-sample.png)

### Embedded Documents and Arrays

Fields that contain a sub-document or an array are displayed with a small triangle next to them and a visual representation of the data contained within the sub-document or array.

![Example of fields with embedded documents or arrays](/images/atlas-ui/compass/embedded-document-sample.png)

Click on the triangle to expand the field and view the embedded documents:

![Expanding the embedded documents](/images/atlas-ui/compass/embedded-document-sample2.png)

### View Charts of Mixed Types

If a field has mixed types, you can view different charts of each type by clicking on the `type` field. In the example below, the `age` field shows the values that are strings:

![Example of a field with mixed types](/images/atlas-ui/compass/mixed-sample.png)

Clicking on the `int32` type causes the chart to show its numeric data:

![Example that shows numeric data for number type](/images/atlas-ui/compass/mixed-sample2.png)

## Query Builder

In the **Schema** tab, you can type the filter manually into the query bar or generate the filter with the Atlas query builder. The query builder allows you to select data elements from one or more fields in your schema and construct a query matching the selected elements.

> **Tip:**
> You can compose the initial query filter by using the clickable query builder and then manually edit the generated filter to your exact requirements.

The following procedure describes the steps involved in building a complex query with the query bar.

*[Contenido incluido desde: /atlas-ui/includes/shared/steps/create-query-via-builder.rst]*

## Troubleshooting

If the analysis of your schema times out, it might be because the collection you are analyzing is very large, causing MongoDB to stop the operation  before the analysis is complete. Increase the value of `MAX TIME MS` to allow the operation time to complete. To increase the value of **MAX TIME MS**:

1. In the query bar, expand **Options**.

   ![The Options button is on the right side of the query bar,](/images/atlas-ui/compass/max-time-ms.png)

2. Increase the value of **MAX TIME MS** to accommodate your collection. **MAX TIME MS** defaults to 60000 milliseconds, or 60 seconds, but large collections might take tens of seconds to analyze.

Once you have increased the value of **MAX TIME MS**, retry your schema analysis by clicking **Analyze**.
