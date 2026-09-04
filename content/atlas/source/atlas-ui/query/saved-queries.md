# Manage Saved Queries and Aggregations in Atlas

You can load saved queries and aggregations from the **My Queries** view once you are connected to your cluster. This page explains how to add and view favorite queries and aggregations.

## Save an Aggregation Pipeline

You can save a pipeline for future use. If you load a saved pipeline, you can change it without changing the original saved copy. You can also create a view from your pipeline results. To save your pipeline:

1. **Click the save dropdown button.**

   In the **Aggregations** pane, open the **Save** drop-down menu and click `Save as`.

   ![Save pipeline as](/images/atlas-ui/compass/query-save-pipeline.png)

1. **Enter a name for your pipeline.**

1. **Save the pipeline.**

   Click the **Save** button to save your pipeline. Your pipeline will be saved under the folder *[icon: Folder]* icon in the top-left corner of the pipeline builder.

## Save a Favorite Query

You can favorite a query for future use. If you load a favorite query, you can change it without changing the original saved copy. To add a query to your favorites:

1. **Open query history.**

   Click the clock *[icon: Clock]* icon on the query bar in the **Documents** pane.

1. **Select favorites.**

   Hover over your query and click the star **Star** button.

1. **Name your query.**

   Enter a name for your query.

1. Click **Save**.

## View Saved Queries

You can view your saved queries and aggregation pipelines on the **My Queries** tab once connected to your cluster.

### Use the **My Queries** Tab

When you click a saved or favorite query tile, Atlas opens the **Documents** tab with the filter loaded. When you click a saved or favorite pipeline tile, Atlas opens the **Aggregations** tab with the pipeline loaded.

### Use the **Favorites** Tab

You can also view favorite queries from the **Favorites** tab from in the **Documents** view. To open the **Favorites** tab:

1. **Open query history**

   Click the clock *[icon: Clock]* icon at the top of the **Documents** tab.

1. Click the **Favorites** button in the past queries pane.

   Select your favorited operation.

   - If your favorited operation is a query, the **Favorites** list view only displays a **Filter** statement, copy *[icon: Copy]* icon, and delete *[icon: Trash]* icon.

      > **Note:**
      > Clicking a favorited query in the **Favorites** list view populates your **Query Bar** with that query.

   - If your favorited operation is a bulk update statement, the **Favorites** list view only displays a **Filter** statement, **Update** statement, copy *[icon: Copy]* icon, delete *[icon: Trash]* icon, and open new tab *[icon: OpenNewTab]* icon.

      > **Note:**
      > If you click the open new tab *[icon: OpenNewTab]* icon, the **Update Documents** modal opens with the bulk update statement and the filter criteria. For details on bulk update statements in Atlas, see atlas-ui-bulk-update.

## View Query History

For details on how to view query history see viewing recent query history.

- [View Recent Queries](/atlas-ui/query/recent)
