# Sort the Returned Documents

If the query bar displays the **Sort** option, you can specify the sort order of the returned documents.

## Set the Sort Order

To set the sort order:

1. In Atlas, go to the **Data Explorer** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Data Explorer** under the **Database** heading. The [Data Explorer](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fmetrics%2FreplicaSet%2F%3Creplset%3E%2Fexplorer) displays.

IMPORTANT: You can also go to the **Clusters** page, and click **Data Explorer** under the **Shortcuts** heading.

1. **Set the sort order.**

   1. Select the collection.
   2. In the Query Bar, click **Options**.
   3. Enter the `sort` document into the **Sort** field.

      - To specify ascending order for a field, set the field to `1` in the sort document.
      - To specify descending order for a field, set the field and `-1` in the sort documents.

      > **Example:**
      > The following `sort` document sorts results first by `year` in descending order, and within each year, sort by `name` in ascending order.
      >
      > ```javascript
      > { year: -1, name: 1 }
      > ```
      >

      As you type, the **Find** button is disabled and the **Sort** label turns red until a valid query is entered.
   4.

      Click **Find** to run the query and view the updated results.

## Clear the Query

To clear the query bar and the results of the query, click **Reset**.

## How Does the Atlas Query Compare to MongoDB and SQL Queries?

`$sort` corresponds to the `ORDER BY ...` clause in a SQL (Structured Query Language) `SELECT` statement.

> **Example:**
> You have 3,235 articles. You would like to see a list of articles sorted alphabetically by headline. SQL .. code-block:: sql SELECT * FROM article ORDER BY headline ASC; MongoDB Aggregation .. code-block:: javascript db.article.aggregate( { $sort : { headline : 1 } } ); Atlas Sort Option .. code-block:: javascript $sort : { headline : 1 }

## Learn More

See the `sort` entry in the [MongoDB Manual](/reference/method/cursor.sort).
