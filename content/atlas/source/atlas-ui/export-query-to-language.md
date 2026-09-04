# Export Query to Specific Language

You can export queries created in the query bar to one of the supported languages; Java, Node, C#, Python 3, Ruby, Go, Rust, and PHP. This feature allows you to reformat and use Atlas queries in your application.

## Procedure

1. In Atlas, go to the **Data Explorer** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Data Explorer** under the **Database** heading. The [Data Explorer](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fmetrics%2FreplicaSet%2F%3Creplset%3E%2Fexplorer) displays.

IMPORTANT: You can also go to the **Clusters** page, and click **Data Explorer** under the **Shortcuts** heading.

1. Click **Export to Language** .

   1. Select the collection.
   2. In the query bar, click the `</>` icon to open the query export card.

   ![Query bar export dropdown menu](/images/atlas-ui/querybar/export-query-to-language-menu-option.png)

1. **Select your export language.**

   On the right side of the card, click the drop-down menu under **Exported Query** and select your desired programming language. The **My Query** pane on the left of the export card displays your pipeline in `~mongosh` syntax. The **Exported Query** pane to the right displays your pipeline in the selected programming language.

   ![Query bar language select](/images/atlas-ui/querybar/export-query-to-language-select.png)

1. ***(Optional)* Include import statements.**

   Click the **Include Import Statements** checkbox to include the required import statements for the selected programming language.

1. ***(Optional)* Include driver syntax.**

   Click the **Include Driver Syntax** checkbox to include application code for the selected programming language. If you include driver syntax, the copyable code reflects project, sort, maxtimems, collation, skip, and limit options.

1. **Click the copy *[icon: copy]* icon.**

   Click the *[icon: copy]* icon at the top-right corner of the formatted query to copy the query for the selected language to your clipboard. You can now integrate and execute your created query in your application.

![Copy button clicked in Export Query to Language modal](/images/atlas-ui/querybar/export-query-python-driver-syntax-copied.png)
