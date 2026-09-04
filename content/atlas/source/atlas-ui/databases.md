# Create, View, and Drop Databases

You can use the Atlas UI to manage the databases in your clusters.

## Required Roles

The following table describes the roles required to perform various actions to a database in the Atlas UI:

| Action | Required Roles |
| --- | --- |
| Create Databases | One of the following roles: - **Project Owner** or **Organization Owner** - **Project Data Access Admin** - **Project Data Access Read/Write** |
| View Databases | At least the **Project Data Access Read Only** role. |
| Drop Databases | One of the following roles: - **Project Owner** - **Project Data Access Admin** |

## Create a Database

To create a database through the Atlas UI:

1. In Atlas, go to the **Data Explorer** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Data Explorer** under the **Database** heading. The [Data Explorer](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fmetrics%2FreplicaSet%2F%3Creplset%3E%2Fexplorer) displays.

IMPORTANT: You can also go to the **Clusters** page, and click **Data Explorer** under the **Shortcuts** heading.

1. Open the **Create Database** dialog box.

   In the **Connections** sidebar, select or hover over your cluster and click the *[icon: Plus]* icon to open the **Create Database** dialog box.

1. Enter the **Database Name** and the **Collection Name**.

   Enter the **Database Name** and the **Collection Name** to create the database and its first collection.

   If you want to use [custom collation](/reference/collation/#collation-document) on the collection, select the **Use Custom Collation** checkbox and select the desired collation settings.

   > **Important:**
   > Don't include sensitive information in your database and collection names.

   For more information on MongoDB database names and collection names, see restrictions-on-db-names.

1. **Optional. Specify a time series collection.**

   Select whether the collection is a [time series collection](/core/timeseries-collections). If you select to create a time series collection, specify the time field and granularity. You can optionally specify the meta field and the time for old data in the collection to expire.

1. Click **Create Database**.

   Upon successful creation, the database and the collection appears in the **Connections** sidebar.

## View Databases

To view the databases and collections in the deployment:

1. In Atlas, go to the **Data Explorer** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Data Explorer** under the **Database** heading. The [Data Explorer](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fmetrics%2FreplicaSet%2F%3Creplset%3E%2Fexplorer) displays.

IMPORTANT: You can also go to the **Clusters** page, and click **Data Explorer** under the **Shortcuts** heading.

### Visualize Database Data

To launch MongoDB Charts to visualize data in your databases and collections:

1. In Atlas, go to the **Data Explorer** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Data Explorer** under the **Database** heading. The [Data Explorer](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fmetrics%2FreplicaSet%2F%3Creplset%3E%2Fexplorer) displays.

IMPORTANT: You can also go to the **Clusters** page, and click **Data Explorer** under the **Shortcuts** heading.

1. **Launch MongoDB Charts.**

   *[Contenido incluido desde: includes/fact-charts-activation.rst]*

## Drop a Database

To drop a database, including all its collections, through the Atlas UI:

1. In Atlas, go to the **Data Explorer** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Data Explorer** under the **Database** heading. The [Data Explorer](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fmetrics%2FreplicaSet%2F%3Creplset%3E%2Fexplorer) displays.

IMPORTANT: You can also go to the **Clusters** page, and click **Data Explorer** under the **Shortcuts** heading.

1. **Drop the database.**

   In the **Connections** sidebar, select or hover over the database to drop and click on the trash can icon. A confirmation dialog appears.

1. **Confirm the database to delete.**

   Confirm by typing the name of the database, and click **Drop Database**.
