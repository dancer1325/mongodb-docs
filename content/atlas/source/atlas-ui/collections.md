# Create, View, Drop, and Shard Collections

* ways to manage the collections 
  * -- via -- Atlas UI
  * -- via -- `mongosh`

## Required Roles

The following table describes the roles required to manage the collections in an Atlas project:

| Action | Required Roles |
| --- | --- |
| Create Collections | One of the following roles: - **Project Owner** or **Organization Owner** - **Project Data Access Admin** - **Project Data Access Read/Write** |
| View Collections | At least the **Project Data Access Read Only** role. |
| Drop Collections | One of the following roles: - **Project Owner** - **Project Data Access Admin** |
| Shard Collections | One of the following roles: - **Project Owner** - **Organization Owner** |

## Create a Collection

> **Tip:**
> To create the first collection in a new database, see atlas-ui-create-a-db.

> **Important:**
> You cannot create new collections on the `config` and `system` databases. Atlas will deprecate writing to existing collections on these databases in the near future.

To create a collection in an existing database through the Atlas UI:

1. In Atlas, go to the **Data Explorer** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Data Explorer** under the **Database** heading. The [Data Explorer](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fmetrics%2FreplicaSet%2F%3Creplset%3E%2Fexplorer) displays.

IMPORTANT: You can also go to the **Clusters** page, and click **Data Explorer** under the **Shortcuts** heading.

1. Open the **Create Collection** dialog box.

   Select or hover over the database, and click the *[icon: Plus]* icon to open the **Create Collection** dialog box.

1. Enter the **Collection Name**.

   In the **Create Collection** dialog box, enter the name of the collection you want to create. Atlas also provides **Additional preferences**. You can choose from the following options:

   - Create a Clustered Collection
   - Create a Collection with Collation

   > **Important:**
   > Don't include sensitive information in your collection name.

   For more information on MongoDB collection names, see restrictions-on-db-names.

1. **Optional. Specify a time series collection.**

   Select whether the collection is a [time series collection](/core/timeseries-collections). If you select to create a time series collection, specify the time field and granularity. You can optionally specify the meta field and the time for old data in the collection to expire.

1. Click **Create Collection**.

   Upon successful creation, the collection appears underneath the database in the **Connections** sidebar.

## View Collections

To view the databases and collections in the cluster through the Atlas UI:

1. In Atlas, go to the **Data Explorer** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Data Explorer** under the **Database** heading. The [Data Explorer](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fmetrics%2FreplicaSet%2F%3Creplset%3E%2Fexplorer) displays.

IMPORTANT: You can also go to the **Clusters** page, and click **Data Explorer** under the **Shortcuts** heading.

1. **View the collections in a database.**

   Click on the name of the database.

   > **Note:**
   > Atlas bases the document count that appears on this tab on cached metadata using [collStats](/reference/command/collStats/). This count might differ from the actual document count in the collection. For example, an [unexpected shutdown](/reference/command/collStats/#behavior) can throw off the count. Use the [db.collection.countDocuments()](/reference/method/db.collection.countDocuments/) method for the most accurate document count.

### Visualize Collection Data

To launch MongoDB Charts to visualize data in your databases and collections.

1. In Atlas, go to the **Data Explorer** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Data Explorer** under the **Database** heading. The [Data Explorer](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fmetrics%2FreplicaSet%2F%3Creplset%3E%2Fexplorer) displays.

IMPORTANT: You can also go to the **Clusters** page, and click **Data Explorer** under the **Shortcuts** heading.

1. **Launch MongoDB Charts.**

   *[Contenido incluido desde: includes/fact-charts-activation.rst]*

## Drop a Collection

To drop a collection, including its documents and indexes, through the Atlas UI:

1. In Atlas, go to the **Data Explorer** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Data Explorer** under the **Database** heading. The [Data Explorer](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fmetrics%2FreplicaSet%2F%3Creplset%3E%2Fexplorer) displays.

IMPORTANT: You can also go to the **Clusters** page, and click **Data Explorer** under the **Shortcuts** heading.

1. **Drop the collection.**

   In the **Connections** sidebar, click the *[icon: ellipsis-h]* icon next to the collection you want to drop. In the drop-down menu that appears, click **Drop Collection**.

1. **Confirm the collection to delete.**

   Confirm by typing the name of the collection, and click **Drop Database**.

## Shard a Collection

If you have large data sets and perform high throughput operations, you can [shard](/sharding) a collection to distribute data across the shards. You cannot shard a collection through the Atlas UI. To shard a collection, first confirm that your cluster is a sharded cluster, then take the following steps to shard the collection in `mongosh`:

*[Contenido incluido desde: /includes/steps/shard-collection.rst]*

- [Collections with Collation](/atlas-ui/collections/collation-collection)
- [Clustered Collections](/atlas-ui/collections/clustered-collection)
- [Time Series Collections](/atlas-ui/collections/time-series-collection)
