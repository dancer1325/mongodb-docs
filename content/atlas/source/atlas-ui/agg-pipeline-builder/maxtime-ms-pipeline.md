# Set Max Time MS for Aggregation Queries

Use the **Max Time MS** option on the **Aggregations** tab to specify an upper time limit in milliseconds for aggregation pipelines that run in Atlas.

## About this Task

By default, **Max Time MS** is set to 60000 milliseconds, or 60 seconds. Consider raising this value if you have a large collection or your operations frequently time out. Alternatively, consider lowering the **Max Time MS** value to quickly identify inefficient or resource-intensive pipeline operations. If your aggregation operation goes over the time limit, Atlas raises a timeout error.

## Steps

1. In Atlas, go to the **Data Explorer** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Data Explorer** under the **Database** heading. The [Data Explorer](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fmetrics%2FreplicaSet%2F%3Creplset%3E%2Fexplorer) displays.

IMPORTANT: You can also go to the **Clusters** page, and click **Data Explorer** under the **Shortcuts** heading.

1. Click **Options**.

   1. Select the collection.
   2. On the **Aggregations** tab, click **Options**

   ![More Options dropdown.](/images/atlas-ui/compass/agg-builder-click-more-options.png)

1. Specify a **Max Time MS** value.

   Next to the **Max Time MS** field, enter a numeric value to set as the maximum amount of time in milliseconds that an aggregation pipeline can run. For example, to set a `5` second limit, enter `5000`.

## Learn More

- atlas-ui-query-bar-max-time-ms
