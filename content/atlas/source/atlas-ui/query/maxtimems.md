# Adjust Maximum Time for Query Operations

The **MAX TIME MS** query bar option sets the cumulative time limit in milliseconds to process query bar operations. If the time limit is reached before the operation completes, Atlas interrupts the operation.

![MaxTimeMS Option](/images/atlas-ui/compass/max-time-ms.png)

The default **MAX TIME MS** value is 60000, or 60 seconds. Consider raising this value if you meet one of the following conditions:

- You have a [large collection](/reference/limits/#data).
- Your operations frequently time out.
- You query data archived from the Atlas cluster using Online Archive.

You can also consider creating indexes to improve query performance.

## Set MAX TIME MS

1. In Atlas, go to the **Data Explorer** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Data Explorer** under the **Database** heading. The [Data Explorer](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fmetrics%2FreplicaSet%2F%3Creplset%3E%2Fexplorer) displays.

IMPORTANT: You can also go to the **Clusters** page, and click **Data Explorer** under the **Shortcuts** heading.

1. **Set the maximum time in milliseconds.**

   1. Select the collection.
   2. Click **Options**.
   3. Adjust **MAX TIME MS** to the desired value in milliseconds.

## Learn More

To learn more about **MAX TIME MS**, see [cursor.maxTimeMS()](/reference/method/cursor.maxTimeMS/) in the MongoDB manual.
