# Create a Time Series Collection

[Time series collections](/core/timeseries-collections/) efficiently store sequences of measurements over a period of time.

## Limitations

The following restrictions and limitations apply when creating a time series collection:

- **Custom collation** is the only **Advanced Collection Option** that you can use with your time series collection.
- See [Time Series Collection Limitations](/core/timeseries/timeseries-limitations/) for all time series collection limitations.

## Procedure

1. In Atlas, go to the **Data Explorer** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Data Explorer** under the **Database** heading. The [Data Explorer](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fmetrics%2FreplicaSet%2F%3Creplset%3E%2Fexplorer) displays.

IMPORTANT: You can also go to the **Clusters** page, and click **Data Explorer** under the **Shortcuts** heading.

1. Click the **Create Collection** button.

   From the **Collections** screen, click the **Create Collection** button.

1. **Enter the collection name.**

1. Check the **Time Series Collection** option.

1. Specify a **timeField**.

   Specify which field should be used as the `timeField` for the time series collection. This field must have a [BSON type date](/reference/bson-types/).

1. *Optional*. Specify a **metaField**.

   Specify the name of the field that contains metadata in each time series document. The metadata in the specified field should be data that is used to label a unique series of documents.

1. *Optional*. Select a **granularity** from the dropdown.

   Specify a coarser granularity so measurements over a longer time span can be more efficiently stored and queried. The default value is `"seconds"`. If you set the `granularity` parameter, you can't set the `bucketMaxSpanSeconds` and `bucketRoundingSeconds` parameters.

1. ***Optional*. Specify a numeric value for the following fields.**

   | Field | Type | Description |
   | --- | --- | --- |
   | `bucketMaxSpanSeconds` | number | Specifies the maximum time span between measurements in a bucket. The value of `bucketMaxSpanSeconds` must be the same as `bucketRoundingSeconds`. If you set the `bucketMaxSpanSeconds`, parameter, you can't set the `granularity` parameter. |
   | `bucketRoundingSeconds` | number | Specifies the time interval that determines the starting timestamp for a new bucket. The value of `bucketRoundingSeconds` must be the same as `bucketMaxSpanSeconds`. If you set the `bucketRoundingSeconds`, parameter, you can't set the `granularity` parameter. |
   | `expireAfterSeconds` | number | Enables the automatic deletion of documents that are older than the specified number of seconds. |

1. Click **Create Collection** to create the collection.

   Your collection will be marked by a **time series** badge.

For more information on time series fields, see [Time Series Object Fields](/core/timeseries/timeseries-procedures/#timeseries-object-fields).
