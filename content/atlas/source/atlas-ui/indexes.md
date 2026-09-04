# Manage Indexes

* Indexes
  * == special data structures /
    * improve query performance
    * store a portion of a collection's data | easy-to-traverse form

TODO: 
* The index stores the value of a specific field or set of fields, ordered by the value of the field
* To improve query performance, build indexes on fields that appear often in queries and for all operations that sort by a field.

- Queries on an indexed field can use the index to limit the number of documents that must be scanned to find matching documents.
- Sort operations on an indexed field can return documents pre-sorted by the index.

To learn more about indexes, see [Indexes](/indexes).

> **Note:**
> Indexes have some negative performance impact on write operations
* For collections with high write-to-read ratio, indexes are expensive since each insert must also update any indexes
* For a detailed list of considerations for indexes, see Operational Considerations for Indexes.

## Required Roles

To create, drop, or hide indexes, you must have access provided by at least one of the following roles:

- **Project Owner** or **Organization Owner**
- **Project Data Access Admin**

## Considerations

* by default,
  * \<= 3 concurrent index builds
* [MORE](/core/index-creation/#maximum-concurrent-index-builds)
  * TODO: fix the relative link

## Indexes Tab

The **Indexes** tab lists the existing indexes for a collection
* To access the **Indexes** tab for a collection:

1. In Atlas, go to the **Data Explorer** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Data Explorer** under the **Database** heading. The [Data Explorer](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fmetrics%2FreplicaSet%2F%3Creplset%3E%2Fexplorer) displays.

IMPORTANT: You can also go to the **Clusters** page, and click **Data Explorer** under the **Shortcuts** heading.

1. **View indexes.**

   1. Select the collecion.
   2. Click the **Indexes** tab.

![Indexes view](/images/atlas-ui/compass/indexes-view.png)

For each index, Atlas displays the following information:

| Name and Definition | The name of the index and keys. |
| --- | --- |
| Type | Regular, text, geospatial or hashed index. |
| Size | How large the index is. |
| Usage | Number of times the index has been used in a lookup since the time the index was created or the last server restart. WARNING: The **Usage** metrics show index usage statistics only for the primary node and should only be used for informational purposes. For more comprehensive index usage statistics, run the `$indexStats` aggregation stage on each node. |
| Properties | Any special properties (such as uniqueness, partial) of the index. |

## Create an Index

*[Contenido incluido desde: /atlas-ui/includes/shared/steps/create-index.rst]*

### MongoDB Vector Search and MongoDB Search Indexes

You can't create MongoDB Search or MongoDB Vector Search indexes in Data Explorer. To manage MongoDB Search and MongoDB Vector Search indexes for your collection, see:

- fts-manage-indexes.
- atlas-ui-create-vector-search

### Create a Wildcard Index

You can create [wildcard indexes](/core/index-wildcard/) to support queries against unknown or arbitrary fields. To create a wildcard index in Atlas, manually type the wildcard index field (`<field>.$**`) into the **Select a field name** input.

> **Example:**
> Consider a collection where documents contain a `userMetadata` object. The fields within the `userMetadata` object may vary between documents. You can create a wildcard index on `userMetadata` to account for all potential fields within the object. Type the following into the **Select a field name** input:
>
> ```javascript
> userMetadata.$**
> ```
>
> Specify a type (`ascending` or `descending`) for your wildcard index, then click **Create Index**. Atlas shows the type of your new index as **Wildcard**.

## Hide or Unhide an Index

You can [hide an index](/core/index-hidden) from the query planner to evaluate the potential impact of dropping an index without actually dropping the index.

1. **Hover over the index.**

   From the **Indexes** tab, hover over the index you want to hide.

1. Click the **Hide Index** button.

   Click the closed-eye icon on the right that appears when you hover over your selected index.

1. Click **Confirm**.

   In the dialog box, confirm the index you want to hide. After you confirm your selection, a **Hidden** badge appears under the **Properties** column. To unhide your index, repeat steps 1-3. After you unhide your index, Atlas removes the **Hidden** badge from the **Properties** column.

## Drop an Index

*[Contenido incluido desde: /atlas-ui/includes/shared/steps/drop-index.rst]*
