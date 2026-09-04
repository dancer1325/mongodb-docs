# Data Modeling in Atlas

The data modeling experience in Atlas allows you to create an entity-relationship diagram that visualizes collections, their fields, data types, and their relationships within a single database. With this, you can better understand how your data is structured and connected, making it easier to develop applications, collaborate with team members, and maintain evolving data models.

## Use Cases

The data modeling experience in Atlas can be useful in the following scenarios:

- **Data visualization**: Generate an entity-relationship diagram of your current data model to see how potential schema changes might impact related collections.
- **Data model planning**: Track and plan changes to your data structure as your application grows.
- **Cross-team collaboration**: Share visual representations of your data model with data engineers, product managers, and other stakeholders to ensure everyone understands the current state of your database structure. You can share your data model as an image, JSON file, or `.mdm` file that can be opened directly in Atlas.
- **Application development**: Identify inconsistencies, missing relationships, or optimization opportunities in your data model during the development process.
- **Team member onboarding**: Understand existing data models when onboarding onto a new project or working with collections created by other teams.

## Behavior

Atlas generates an entity-relationship diagram based on a small sample of documents from each collection you select in your database. Due to this sampling, your diagram might not reflect all fields or relationships in your data.

## Get Started

- atlas-ui-create-data-model-diagram
- atlas-ui-modify-data-model-collections
- atlas-ui-modify-data-model-fields

## Details
### Annotations

You can add comments to your diagram's collections, fields, and relationships to document definitions or data modeling decision.

### Relationships

You can define relationships between fields in different collections in your entity-relationship diagram. When you create an entity-relationship diagram, Atlas can automatically infer relationships, or you can manually add them.

> **Note:**
> Relationships are for annotation purposes only. Atlas does not store relationship information in your MongoDB database.
>

### Share Diagrams

You can export your Atlas entity-relationship diagram to collaborate with teams, receive feedback, and align on final schema design decisions. You can export your diagram as a:

- `.mdm` **Diagram File**.
- **PNG** image.
- **JSON** file.

You can also import an `.mdm` file to view or edit a diagram shared by another member of your cluster's project in Atlas.

## Learn More

- manual-data-modeling-intro

- [Generate Diagram](/atlas-ui/data-modeling/generate-diagram)
- [Modify Collections](/atlas-ui/data-modeling/modify-collections)
- [Modify Fields](/atlas-ui/data-modeling/modify-fields)
- [Manage Relationships](/atlas-ui/data-modeling/relationships)
- [Export Diagram](/atlas-ui/data-modeling/export-diagram)
- [Import Diagram](/atlas-ui/data-modeling/import-diagram)
