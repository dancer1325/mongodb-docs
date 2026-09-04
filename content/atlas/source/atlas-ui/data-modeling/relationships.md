# Manage Relationships in Your Atlas Entity-Relationship Diagram

You can manually define relationships between fields across different collections in your Atlas entity-relationship diagram.

> **Note:**
> Relationships are for annotation purposes only. Atlas does not store relationship information in your MongoDB database.
>

You can manually add relationships to your entity-relationship diagram by using one of the following methods:

- **Side Panel**: Manually add a relationship by selecting a source collection.
- **Drag and Drop**: Create a relationship by clicking and dragging from one collection to another.

## Before You Add Relationships

To add relationships, ensure you have already generated a diagram.

## Procedure

To learn how to add relationships to your entity-relationship diagram, select the tab corresponding to your preferred method:

**Side Panel**

1. **Open your entity-relationship diagram.**

   From the *[icon: Diagram]* **Data Modeling** tab, open your entity-relationship diagram.

1. Open the **Collection Properties** side panel.

   Click one of your collections in the diagram. The collection side panel opens on the right side of the screen.

1. Click **Add Relationship**.

   In the side panel's **Relationship** tab, click **Add Relationship**.

1. Specify **Relationship Properties**.

   Specify values for the following fields under **Relationship Properties**:

   - **Local collection**: The primary collection in the relationship.
   - **Local field**: The field in the primary collection.
   - **Local cardinality**: The number of unique values in the local field.
   - **Foreign collection**: The target collection in the relationship.
   - **Foreign field**: The field in the target collection.
   - **Foreign cardinality**: The number of unique values in the foreign field.

   Atlas automatically saves and updates your changes as you make them.

1. **Add annotations for your relationship.**

   In the side panel's **Notes** tab, you can add annotations for your relationship. This can be helpful for documenting the purpose and details of the relationship.

---

**Drag and Drop**

1. **Open your entity-relationship diagram**

   From the *[icon: Diagram]* **Data Modeling** tab, open your entity-relationship diagram.

1. **Click the relationship *[icon: Relationship]* icon**

   In the upper-left corner of the diagram view, click the relationship *[icon: Relationship]* icon to enable drag-and-drop relationship creation.

1. **Click one collection and drag it to another**

   When you click a collection and drag it to another, Atlas opens the side panel and displays the **Relationship Properties** tab.

1. Specify **Relationship Properties**.

   Specify values for the following fields under **Relationship Properties**:

   - **Local collection**: The primary collection in the relationship.
   - **Local field**: The field in the primary collection.
   - **Local cardinality**: The number of unique values in the local field.
   - **Foreign collection**: The target collection in the relationship.
   - **Foreign field**: The field in the target collection.
   - **Foreign cardinality**: The number of unique values in the foreign field.

   Atlas automatically saves and updates your changes as you make them.

1. **Add annotations for your relationship.**

   In the side panel's **Notes** tab, you can add annotations for your relationship. This can be helpful for documenting the purpose and details of the relationship.

---

After you create a relationship, Atlas displays each relationship in the **Relationships** tab.

### Delete Relationships

To delete a relationship from your entity-relationship diagram:

1. **Open the relationship side panel**

   Click on the relationship in the diagram. The relationship side panel opens on the right side of the screen.

1. **Select the relationship to delete**

   In the **Relationship Properties** tab, click the *[icon: Trash]* **Delete** button.

## Next Steps

- atlas-ui-data-modeling-export

## Learn More

- atlas-ui-data-modeling
