# Generate an Entity-Relationship Diagram in Atlas

Generate a visualization of your data model as an entity-relationship diagram in Atlas. Entity-relationship diagrams can help you understand and document the relationships between data in your database and plan changes to your schema.

Atlas generates an entity-relationship diagram based on a small sample of documents from each collection you select in your database. Due to this sampling, your diagram might not reflect all fields or relationships in your data.

If you make any changes to your data after you generate a diagram, Atlas doesn't automatically update the diagram. You must create a new diagram to see your changes.

## Before You Generate an Entity-Relationship Diagram

To generate an entity-relationship diagram in Atlas, your database must have at least one collection with data.

## Procedure

1. In Atlas, go to the **Data Explorer** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Data Explorer** under the **Database** heading. The [Data Explorer](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fmetrics%2FreplicaSet%2F%3Creplset%3E%2Fexplorer) displays.

IMPORTANT: You can also go to the **Clusters** page, and click **Data Explorer** under the **Shortcuts** heading.

1. Open the **Data Modeling** tab

   In the left sidebar, click *[icon: Diagram]* **Data Modeling**.

1. Click **Generate diagram**.

   If you have existing diagrams saved in the **Data Modeling** tab, you can create a new diagram by clicking *[icon: Plus]* **Generate new diagram** in the upper-right corner of the screen.

1. **Name your diagram.**

1. **Select your MongoDB connection.**

   Use the drop-down menu to select which MongoDB connection to use to generate your diagram. The **Active** label appears under any currently active connection name.

1. **Select your database.**

   Use the drop-down menu to select which database to use for your data model diagram.

1. **Select collections.**

   Use the checkboxes to select which collections to include in your data model diagram.

1. **(Optional) Choose if Atlas automatically infers relationships between collections.**

   In the **Select collections** modal, you can choose whether you want Atlas to **Automatically infer relationships** between collections. When enabled, Atlas analyzes the selected collections and adds relationships based on indexed fields that contain references to other collections. You can also manually add and edit relationships after you generate your diagram.

   > **Note:**
   > Relationships are for annotation purposes only. Atlas does not store relationship information in your MongoDB database.
   >

1. Click **Generate**.

   Atlas generates an entity-relationship diagram with the selected collections and displays it in the current tab. After you generate a diagram, Atlas displays the existing diagrams in the **Data Modeling** tab.

## Next Steps

- atlas-ui-data-modeling-relationships
- atlas-ui-data-modeling-export
- atlas-ui-data-modeling-import

## Learn More

- manual-data-modeling-intro
- atlas-ui-data-modeling
