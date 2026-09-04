# Export Pipeline to Specific Language

You can use the Aggregation Pipeline Builder to format and export finished pipelines. You can export pipelines to a chosen language to use in your application.

## About this Task

You can export your pipeline to the following languages:

- C#
- Go
- Java
- Node
- PHP
- Python
- Ruby
- Rust

## Steps

1. In Atlas, go to the **Data Explorer** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Data Explorer** under the **Database** heading. The [Data Explorer](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fmetrics%2FreplicaSet%2F%3Creplset%3E%2Fexplorer) displays.

IMPORTANT: You can also go to the **Clusters** page, and click **Data Explorer** under the **Shortcuts** heading.

1. Click **Export to Language**.

   1. Select the collection.
   2. In the aggregation pipeline pane, click the **Export to Language** button to open the pipeline export card.

   ![Aggregation Builder export button](/images/atlas-ui/compass/agg-builder-export-dropdown.png)

1. **Select your export language.**

   On the right side of the card, click the drop-down menu under **Exported Pipeline** and select your desired programming language. The **My Pipeline** pane on the left of the export card displays your pipeline in `mongosh` syntax. The **Exported Pipeline** pane to the right displays your pipeline in the selected programming language.

1. **(Optional) Include import statements.**

   Click the **Include Import Statements** checkbox to include the required import statements for the selected programming language.

1. **(Optional) Include driver syntax.**

   Click the **Include Driver Syntax** checkbox to include application code for the selected programming language. If you include driver syntax, the copyable code reflects project, sort, maxtimems, collation, skip and limit options.

1. **Click the file *[icon: files-o]* icon.**

   Click the *[icon: files-o]* icon at the top-right of the pipeline to copy your pipeline for the selected programming language. You can now integrate and execute your created pipeline in your application.

## Learn More

- Aggregation Pipeline Builder
- [MongoDB Driver Documentation](/)
