# Count Pipeline Results Documents

You can view the number of documents outputted by your pipeline with the **count results** button.

## About this Task

When you delete or add a document, you must manually refresh the **count results** value on the **Aggregations** tab to reflect the new document count.

## Before You Begin

To count result documents, you must first create and run your aggregation pipeline. You can't count result documents while editing your pipeline.

## Steps

1. In Atlas, go to the **Data Explorer** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Data Explorer** under the **Database** heading. The [Data Explorer](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fmetrics%2FreplicaSet%2F%3Creplset%3E%2Fexplorer) displays.

IMPORTANT: You can also go to the **Clusters** page, and click **Data Explorer** under the **Shortcuts** heading.

1. **Create and run your aggregation pipeline.**

1. Click **count results**

   After you run your pipeline, click **count results**, which appears under the **Run** button. The **count results** button will update with the count of resulting documents.

   ![Click the count results link](/images/atlas-ui/compass/agg-builder-click-count-results.png)

1. **(Optional) Click the refresh icon**

   If you edit your pipeline, click refresh *[icon: refresh]* to update your document count.

## Learn More

- atlas-ui-agg-builder
- atlas-ui-documents
