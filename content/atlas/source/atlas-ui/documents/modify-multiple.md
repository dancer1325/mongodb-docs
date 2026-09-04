# Modify Multiple Documents

You can perform bulk update operations on multiple documents in Atlas by using the **Update Documents** modal. Performing updates with the **Update Documents** modal helps you visualize updates to your data before you apply them.

## About this Task

- You can use any syntax that works with the `update` parameter of `db.collection.updateMany()`.
- The **Update Documents** modal does not support any `options` parameters such as upsert, writeConcern, or collation.
- Previews of the documents affected by bulk update operations are only visible if your database is configured to support transactions. For details, see [/core/transactions](/core/transactions).

## Steps

1. In Atlas, go to the **Data Explorer** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Data Explorer** under the **Database** heading. The [Data Explorer](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fmetrics%2FreplicaSet%2F%3Creplset%3E%2Fexplorer) displays.

IMPORTANT: You can also go to the **Clusters** page, and click **Data Explorer** under the **Shortcuts** heading.

1. **Apply a query filter**

   1. Select the collection.
   2. From the **Documents** tab, input a query into the **Query bar**. The filter criteria of the query specified applies to the documents in the **Bulk Update** modal. If you need to apply an update to all documents in a collection, leave the **Query bar** blank.

1. **Open the bulk update modal**

   On the **Documents** tab, click the *[icon: Edit]* **Update** button to display the **Update Documents** modal. The following table summarizes the UI (User Interface) of the modal:

   | UI Element | Description |
   | --- | --- |
   | **Filter** | Any filter criteria specified on the **Query Bar** applies to the **Update Documents** modal. To update the filter query, exit the **Update Documents** modal and modify the query in the **Query Bar**. |
   | **Update** | The update syntax that is applied to the documents specified in the filter criteria. You can use any syntax that works with the `update` parameter of the `db.collection.updateMany()`. |
   | **Preview** | A preview of documents with the update syntax applied. |

1. **Enter the update syntax**

   In the **Update** text field, provide the update syntax. The number of documents affected by the update displays at the top of the **Update Documents** modal.

   > **Note:**
   > The documents under the **Preview** header show how the **Update** syntax affects documents in your collection.

1. **Update your documents**

   Click **Update Documents**. Atlas applies the **Update** to the documents within the **Filter** expression.

## Example

The following example uses the sample_mflix dataset. This example updates the `tomatoes.viewer.numReviews` and `tomatoes.viewer.meter` fields with the Atlas **Update Documents** modal. Apply a filter in the **Query Bar** to filter movies which have a `year` of `1917`.

```javascript
{ 'year' : 1917 }
```

Click the *[icon: Edit]* **Update** button, the **Update Documents** modal displays. In the **Update** text box, paste the following syntax:

```javascript
{ 
   $inc: { "tomatoes.viewer.numReviews" : 1}, 
   $set: { "tomatoes.viewer.meter" : 99 } 
}
```

This syntax:

- [Increments](/reference/operator/update/inc) the `tomatoes.viewer.numReviews` field by `1`.
- [Sets](/reference/operator/update/set) the `tomatoes.viewer.meter` field to `99`.

The **Preview** section populates with sample documents affected by the update query. To view the updates to the **numReviews** and **meter** fields:

- Click the *[icon: angle-right]* right arrow icon next to **tomatoes**.
- Click the *[icon: angle-right]* right arrow icon next to **viewer**.

Click the **Update Documents** button to update the collection's data.

## Learn More

- atlas-ui-bulk-delete
