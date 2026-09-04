# Set Which Fields Are Returned

If the query bar displays the **Project** option, you can specify which fields to return in the resulting data. By default, all fields are returned.

## Set a Projection

1. In Atlas, go to the **Data Explorer** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Data Explorer** under the **Database** heading. The [Data Explorer](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fmetrics%2FreplicaSet%2F%3Creplset%3E%2Fexplorer) displays.

IMPORTANT: You can also go to the **Clusters** page, and click **Data Explorer** under the **Shortcuts** heading.

1. **Set the projection.**

   1. Select the collection.
   2. In the Query Bar, click **Options**.
   3. Enter the projection document into the **Project** field. To include fields: Specify the field name and set to `1` in the project document.

      > **Example:**
      > ```javascript
      > { year: 1, name: 1 }
      > ```
      >
      > Only the fields specified in the project document are returned. The `_id` field is returned unless it is set to  `0` in the **Project** document.

      To exclude fields: Specify the field name and set to `0` in the project document.

      > **Example:**
      > ```javascript
      > { year: 0, name: 0 }
      > ```
      >
      > All fields except for the fields specified in the project document are returned.

      As you type, the **Find** button is disabled and the **Project** label turns red until a valid query is entered.
   4.

      Click **Find** to run the query and view the updated results.

## How Does the Atlas Query Compare to MongoDB and SQL Queries?

`$project` corresponds to choosing specific fields to return in a SQL (Structured Query Language) `SELECT` statement.

> **Example:**
> You have 3,235 articles. You would like to see only the headlines and authors of those articles. SQL .. code-block:: sql SELECT headline, author FROM article; MongoDB Aggregation .. code-block:: javascript db.article.aggregate( { $project : { headline : 1, author : 1 } } ); Atlas Project Option .. code-block:: javascript { headline : 1, author : 1 }

## Learn More

To learn how project works, see the `project` entry in the MongoDB Manual.
