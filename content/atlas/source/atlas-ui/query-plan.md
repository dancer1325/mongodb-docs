# View Query Performance

To help you better understand the performance of your query, you can view your query's explain plan.

## About This Task

On the **Explain Plan** modal, you can view the explain stages as a **Visual Tree**, where each query operation appears as a node on the tree. You can also view the explain details in raw JSON format by selecting the **Raw Output** view.

> **Note:**
> The **Explain Plan** doesn't show aggregation pipeline stages such as `$merge` and `$out` because Atlas ignores all out stages from the aggregation before running the explain plan.

The explain plan includes a **Query Performance Summary** with information on the execution of your query such as:

- Execution time
- The number of returned documents
- The number of examined documents
- The number of examined index keys

## Steps

1. In Atlas, go to the **Data Explorer** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Data Explorer** under the **Database** heading. The [Data Explorer](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fmetrics%2FreplicaSet%2F%3Creplset%3E%2Fexplorer) displays.

IMPORTANT: You can also go to the **Clusters** page, and click **Data Explorer** under the **Shortcuts** heading.

1. Click **Explain**

   1. Select the collection.
   2. In the query bar, click the **Explain** button to open the modal.

   ![Query plan](/images/atlas-ui/compass/query-plan.png)

1. **Select a query operation**

   By default, the explain stages are are shown as a **Visual Tree**. Each query operation appears as a node on the tree. For more detailed execution information about the query operation, click the corresponding node. For example, the following explain plan provides detailed information on a query that filters for `{ title : "Jurassic Park" }`:

   ![Detailed Visual Tree view](/images/atlas-ui/compass/explain-tree.png)

1. (Optional) Select the **Raw Output** view.

   To view your full explain plan as raw JSON, select the **Raw Output** view.

## Learn More

- [Analyze Query Performance](/tutorial/analyze-query-plan/)
