# Export Your Entity-Relationship Diagram from Atlas

You can share your refactored data model with other teams to receive feedback and align on final schema design decisions. To share your refactored schema, you can export your entity-relationship diagram from Atlas. You can export your entity-relationship diagram as a:

- **Diagram File**.
- **PNG** image.
- **JSON** file.

When you export your entity-relationship diagram as a **Diagram File**, Atlas exports it as a `.mdm` file. This file format allows you to import the entity-relationship diagram into Atlas and share it with collaborators in the Compass or Atlas data modeling experience. When you export your entity-relationship diagram as a PNG image, Atlas exports the collections and their relationships as they appear in the **Data Modeling** tab. This format helps you share a visual representation of your schema in documentation or presentations. When you export your entity-relationship diagram as a JSON file, it includes:

- A `collections` array with the collection namespaces and their jsonSchema.
- A `relationships` array with the namespaces of the related collections and their linked fields.

## Before You Export Your Entity-Relationship Diagram

Before you export your schema visualization, ensure you generate a diagram.

## Procedure

1. In Atlas, go to the **Data Explorer** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Data Explorer** under the **Database** heading. The [Data Explorer](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fmetrics%2FreplicaSet%2F%3Creplset%3E%2Fexplorer) displays.

IMPORTANT: You can also go to the **Clusters** page, and click **Data Explorer** under the **Shortcuts** heading.

1. Open the **Data Modeling** tab.

   In the left sidebar, click *[icon: Diagram]* **Data Modeling**.

1. **Open your entity-relationship diagram.**

1. Open the **Export data model** modal.

   In the upper-right corner of your diagram view, click the export *[icon: Export]* icon.

1. **Select your export file format.**

   After you select the file format, click **Export**.

1. **Save your exported file.**

   Select the location on your filesystem to save your exported file.

## Learn More

- atlas-ui-data-modeling
- atlas-ui-data-modeling-import
