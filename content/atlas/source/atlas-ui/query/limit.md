# Limit the Number of Returned Documents

If the query bar has the **Limit** option, you can specify the maximum number of documents to return.

## Set Documents to Return

To specify the limit:

1. In Atlas, go to the **Data Explorer** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Data Explorer** under the **Database** heading. The [Data Explorer](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fmetrics%2FreplicaSet%2F%3Creplset%3E%2Fexplorer) displays.

IMPORTANT: You can also go to the **Clusters** page, and click **Data Explorer** under the **Shortcuts** heading.

1. **Specify the limit.**

   1. Select the collection.
   2. In the Query Bar, click **Options**.
   3. Enter an integer representing the number of documents to return into the **Limit** field.
   4.

      Click **Find** to run the query and view the updated results.

   ![Results of specifying query limit](/images/atlas-ui/querybar/query-limit-success.png)

## Clear the Query

To clear the query bar and the results of the query, click **Reset**.

## How Does the Atlas Query Compare to MongoDB and SQL Queries?

`$skip` corresponds to the `LIMIT ...` clause in a SQL (Structured Query Language) `SELECT` statement.

> **Example:**
> You have 3,235 articles. You would like to see a list of the first 10 articles. SQL .. code-block:: sql SELECT * FROM article LIMIT 10; MongoDB Aggregation .. code-block:: javascript db.article.aggregate( { $limit : 10 } ); Atlas Limit Option .. code-block:: javascript $limit : 10

## Learn More

See the `limit` entry in the [MongoDB Manual](/reference/method/cursor.limit).
