# View Explain Plans for a Pipeline

To help you better understand the performance of your pipeline, you can view your pipeline's explain plan. You can view the explain plan at any point while creating or editing your pipeline.

## About this Task

On the **Explain** modal, you can view the explain stages as a **Visual Tree**, where each stage of the pipeline appears as a node on the tree. Alternatively, you can view the explain details in raw JSON format by selecting the **Raw Output** view. The explain plan includes a **Query Performance Summary** with information on the execution of your aggregation pipeline such as:

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
   2. In the top right of the aggregation pipeline builder, click the **Explain** button to open the **Explain Plan** modal.

   ![Explain button on aggregation pipeline](/images/atlas-ui/compass/agg-builder-explain.png)

1. **Select an aggregation pipeline**

   By default, the explain stages are are shown as a **Visual Tree**. Each stage of the pipeline appears as a node on the tree. You can click on each stage for more detailed execution information about the stage.

   ![Detailed Visual Tree view](/images/atlas-ui/compass/agg-builder-explain-tree.png)

1. (Optional) Select the **Raw Output** view

   To view your full explain plan as raw JSON, select the **Raw Output** view.

## Learn More

- [Analyze Query Performance](/tutorial/analyze-query-plan/)
- atlas-ui-explain-plans
