# Aggregation Pipeline Builder Settings

You can adjust your Atlas Aggregation Pipeline Builder settings to customize your aggregation experience and improve pipeline performance.

## Settings

To view and change your aggregation pipeline settings:

1. In Atlas, go to the **Data Explorer** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Data Explorer** under the **Database** heading. The [Data Explorer](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fmetrics%2FreplicaSet%2F%3Creplset%3E%2Fexplorer) displays.

IMPORTANT: You can also go to the **Clusters** page, and click **Data Explorer** under the **Shortcuts** heading.

1. Open the **Settings** panel.

   1. Select the collection.
   2. Click the **Aggregations** tab.
   3. Click the gear icon at the upper right of the pipeline builder to open the **Settings** panel.

| Option | Description | Default Value |
| --- | --- | --- |
| Comment Mode | When enabled, adds helper comments to each stage. | Enabled |
| Number of Preview Documents | Sets number of documents to show in the preview. | 10 |
| Limit | Specifies the number of documents passed to `$group`, `$bucket`, and `$bucketAuto` pipeline stages. Lower limits improve pipeline run time but might result in missing documents. This setting is only applied to document previews. It is not applied when the pipeline is run. | 100000 |

## Learn More

- atlas-ui-create-agg-pipeline
- atlas-ui-pipeline-custom-collation
- atlas-ui-set-maxtime-ms-agg
