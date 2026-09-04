# Create a Clustered Collection

[Clustered collections](/core/clustered-collections/) are collections with a clustered index. Clustered collections store documents ordered by [clustered index](/reference/method/db.createCollection/#std-label-db.createCollection.clusteredIndex) key value. You can use clustered collections when only one clustered index is necessary.

## Limitations

Clustered collection limitations:

- The clustered index key must be on the `_id` field.
- Clustered collections may not be [capped collections](/core/capped-collections).

## Steps

1. In Atlas, go to the **Data Explorer** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Data Explorer** under the **Database** heading. The [Data Explorer](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fmetrics%2FreplicaSet%2F%3Creplset%3E%2Fexplorer) displays.

IMPORTANT: You can also go to the **Clusters** page, and click **Data Explorer** under the **Shortcuts** heading.

1. Open the **Create Collection** dialog box.

   Select a database and from the **Collections** screen, click the **Create Collection** button. You can also click the `+` next to the name of the database you select to open the **Create Collection** dialog box.

1. **Enter the collection name.**

1. **Select the type of collection you want to create.**

   From the **Additional preferences** drop-down, select **Clustered Collections**.

1. **(Optional) Name your clustered index.**

   You can enter a name for the clustered index or use the automatically generated name.

1. (Optional) Enter the number of seconds for the **expireAfterSeconds** field.

   The **expireAfterSeconds** field is a [TTL index](/tutorial/expire-data/) that enables automatic deletion of documents older than the specified number of seconds. The **expireAfterSeconds** field must be a positive, non-zero value.

1. Click **Create Collection** to create your new collection.

   In the **Collections** screen, your new collection is marked by a **Clustered** badge next to the collection name.

## Next Steps

- Manage Documents
- Query Your Data
- Analyze Your Data Scheme
