# Modify Fields in Your Atlas Entity-Relationship Diagram

You can modify your existing entity-relationship diagram in Atlas to plan changes to your data model *without* affecting your actual data.

In the **Data Modeling** tab, you can:

- Add or remove fields
- Change field data types
- Rename fields

> **Note:**
> Any changes you make to your entity-relationship diagram don't affect your actual database.

## Procedure

The following steps describe how you can add, edit, and remove fields in your entity-relationship diagram:

1. In Atlas, go to the **Data Explorer** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Data Explorer** under the **Database** heading. The [Data Explorer](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fmetrics%2FreplicaSet%2F%3Creplset%3E%2Fexplorer) displays.

IMPORTANT: You can also go to the **Clusters** page, and click **Data Explorer** under the **Shortcuts** heading.

1. Open the **Data Modeling** tab.

   In the left sidebar, click *[icon: Diagram]* **Data Modeling**.

1. **Open your entity-relationship diagram.**

   From the *[icon: Diagram]* **Data Modeling** tab, open your diagram.

1. **Add a new field.**

   To add a new field, click the plus *[icon: Plus]* icon in the upper-right corner of one of your collection nodes. A new field appears in the collection node. Once you add a field, you can edit the field's properties, including:

   - Renaming the field.
   - Changing the field's data type.
   - Adding relationships.

1. Open the **Field Properties** side panel.

   Click one of your fields in the diagram. The field side panel opens on the right side of the screen.

1. ***(Optional)* Rename a field.**

   Under the **Name** section, specify a new name for your selected field.

1. ***(Optional)* Change a field's data type.**

   In the **Datatype** drop-down menu, select the datatypes that you want to assign to the field. Because data in MongoDB has a flexible schema model, you can specify multiple data types for a field. If a field contains multiple data types, Atlas labels the field as **(mixed)** in the diagram.

1. ***(Optional)* Delete a field.**

   In the side panel, click the trash *[icon: Trash]* icon.

## Next Steps

- atlas-ui-modify-data-model-collections
- atlas-ui-data-modeling-relationships
- atlas-ui-data-modeling-export
- atlas-ui-data-modeling-import

## Learn More

- manual-data-modeling-intro
- atlas-ui-data-modeling
