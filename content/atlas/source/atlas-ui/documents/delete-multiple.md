# Delete Multiple Documents

You can perform bulk delete operations on multiple documents in Atlas by using the **Delete Documents** modal. This helps you visualize deletes before applying them.

## About this Task

Deleting documents is a permanent action and cannot not be undone. Validate documents in the **Preview** of the **Delete Documents** modal before confirming the delete operation.

## Steps

1. In Atlas, go to the **Data Explorer** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Data Explorer** under the **Database** heading. The [Data Explorer](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fmetrics%2FreplicaSet%2F%3Creplset%3E%2Fexplorer) displays.

IMPORTANT: You can also go to the **Clusters** page, and click **Data Explorer** under the **Shortcuts** heading.

1. **Apply a query filter**

   1. Select the collection.
   2. From the **Documents** tab, input a query into the **Query Bar** to filter deleted documents. To delete all documents in the collection, leave the **Query Bar** blank.

1. Open the **Delete Documents** modal

   On the **Documents** tab, click the *[icon: Trash]* **Delete** button to display the **Delete Documents** modal. The following table summarizes the UI (User Interface) of the modal:

   | UI Element | Description |
   | --- | --- |
   | **Query** | Any filter criteria specified on the **Query Bar** applies to the **Delete Documents** modal. To update the **Query**, exit the **Delete Documents** modal and modify the query in the **Query Bar**. |
   | **Export** | Opens the **Export Delete Query To Language** modal, where you can convert the query to a supported driver language. |
   | **Preview** | A preview of the documents that will be deleted. |

1. (Optional) Export the **Delete**

   You can export the **Delete** query to a supported driver language using the **Export** button on the **Delete Documents** modal.

   1. On the **Delete Documents** modal, click **Export**. The **Export Delete Query To Language** modal displays with the delete syntax populated under **My Delete Query**.
   2. Select a programming language from the drop-down under **Exported Delete Query**. You can convert the command to C#, Go, Java, Node, PHP, Python, Ruby, or Rust. The field below displays the converted syntax.
   3. (Optional) Click the **Include Import Statements** checkbox to include the required import statements for the selected programming language.
   4. Click the copy *[icon: Copy]* icon to copy the converted syntax.
   5. Click **Close**.

1. **Delete your documents**

   1. On the **Delete Documents** modal, click **Delete Documents**.
   2. Click the red **Delete Documents** button to confirm the operation.

   Atlas deletes the documents that match the filter expression.

## Example

The following example deletes two documents from the `movies` collection in the sample_mflix dataset. In the **Query Bar**, enter a filter for movies with a `year` of `1919`.

```javascript
{ 'year' : 1919 }
```

Click the *[icon: Trash]* **Delete** button, the **Delete Documents** modal displays. The **Preview** pane shows the documents included in the delete operation. Click **Delete Documents**. A confirmation modal displays. Click the red **Delete Documents** button to confirm the operation.

## Learn More

- atlas-ui-bulk-update
