# Manage Views in Atlas

Views are read-only results of an aggregation run against a collection. Views provide easy access to the results of an aggregation without requiring the reader of the view to execute the pipeline. Views can also help keep your data secure by only giving users access to a predefined result set, as opposed to having access to the underlying collection.

## Manage Views in the Data Explorer

To manage views in Atlas:

1. In Atlas, go to the **Data Explorer** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Data Explorer** under the **Database** heading. The [Data Explorer](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fmetrics%2FreplicaSet%2F%3Creplset%3E%2Fexplorer) displays.

IMPORTANT: You can also go to the **Clusters** page, and click **Data Explorer** under the **Shortcuts** heading.

## Collections Screen

The **Collections** screen lists the existing collections and views in the selected database. Each list item includes the name and other general information for the collection or view. To access the **Collections** screen for a database, from the Databases screen either:

- Click a **Database Name** in the main **Databases** view, or
- Click a database in the left navigation.

![Select database](/images/atlas-ui/compass/select-database.png)

Atlas displays views in the **Collections Screen** with a special icon, and indicates the collection from which the view was created.

### View Information

The **Collections** screen displays the following information for each view in the selected database:

- View name
- Collection from which the view was created

## Create a View

To create a view, you must use the Aggregation Pipeline Builder. The output of your pipeline's final stage becomes the content of the view. To create a view from your pipeline results:

1. Create an aggregation pipeline in the pipeline builder. For detailed instructions on using the pipeline builder, see atlas-ui-create-agg-pipeline.
2. Click the arrow next to the **Save** button at the top of the pipeline builder.
3. Click **Create a View**.
4. Enter a name for your view.
5. Click **Create**.

Atlas creates a view from your pipeline results in the same database where the pipeline was created.

## Open a View

To open a view, either:

- Click the desired view from the Collections screen, or
- Click the desired view in the left-hand navigation.

After you open a view, Atlas shows you that view's Documents Tab. Atlas provides the following information and functionality for the view:

- Document management
- Aggregation Pipeline Builder
- Indexes
- Data schema
- Validation rules

## Duplicate a View

You can duplicate a view to modify an existing view while retaining the original. To duplicate a view:

6. Hover over the desired view in the left navigation.
7. Click the appearing **Ellipses (...)** button.
8. In the drop-down menu, click **Duplicate View**.
9. Enter a name for the new view.
10. Click **Duplicate**.

## Modify the Source of a View

> **Note:**
> Views are read-only, and cannot inherently be modified. This procedure describes modifying the *underlying source* of a view. When you modify a view, Atlas cannot retain collation information associated with the view. Any collation information must be re-entered in the pipeline builder during modification.

To modify the source of a view:

11. Hover over the desired view in the left navigation.
12. Click the appearing **Ellipses (...)** button.
13. In the dropdown, click **Modify view**. This button opens the aggregation pipeline builder and populates the pipeline used to create the view.
14. Modify the pipeline as desired. For detailed instructions on using the pipeline builder, see atlas-ui-create-agg-pipeline.
15. Click **Update View** at the top of the pipeline builder.

## Drop a View

To drop a view from the database:

16. Hover over the desired view in the left navigation.
17. Click the appearing **Ellipses (...)** button.
18. In the dropdown, click **Drop View**.
19. In the modal, enter the name of the view.
20. Click **Drop Collection**.
