# Create a View from Pipeline Results

To quickly access the results of an aggregation pipeline without having to run it, you can create a view on Atlas. Views are read-only, so they can help keep your data secure by limiting user access to a predefined set of results.

## About this Task

Creating a view does not save the aggregation pipeline itself.

## Steps

1. In Atlas, go to the **Data Explorer** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Data Explorer** under the **Database** heading. The [Data Explorer](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fmetrics%2FreplicaSet%2F%3Creplset%3E%2Fexplorer) displays.

IMPORTANT: You can also go to the **Clusters** page, and click **Data Explorer** under the **Shortcuts** heading.

1. Click the **Save** drop-down button.

   1. Select the collection.
   2. In the aggregation pipeline pane, click the **Save** drop-down button and select **Create view**.

   ![Save drop-down button](/images/atlas-ui/compass/query-save-pipeline.png)

1. **Enter a name for your view.**

   The view name must be between 6 and 1024 characters long.

1. **Create your view.**

   Click the **Create** button to create your view. Atlas creates a view from your pipeline results in the same database where the pipeline was created and displays saved views with the eye *[icon: eye]* icon.

## Learn More

- atlas-ui-views
