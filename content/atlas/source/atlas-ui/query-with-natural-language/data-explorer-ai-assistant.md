# Intelligent Assistant in Data Explorer

The intelligent assistant is an AI-powered tool designed for integrated MongoDB development support in Data Explorer. It answers natural language questions, assists with debugging errors, and provides guidance for performance optimization.

> **Note:**
> The intelligent assistant is currently in public preview.

.. Attention writers: If you make changes to the following file, please check .. the related page(s) in the Compass docs to see if they require changes as .. well: www.mongodb.com/docs/compass/query-with-natural-language/compass-ai-assistant/

## Enable AI Features

To use the intelligent assistant in Data Explorer, first enable AI features in settings. At the organizational level:

1. In Atlas, go to the **Organization Settings** page.

   1. If it's not already displayed, select your desired organization from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. In the sidebar, click **Organization Settings**.

   The [Organization Settings](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%23%2Forg%2F%3Corganization%3E%2Fsettings%2Fgeneral) page displays.

1. **Enable Generative AI features.**

Set the **Enable Atlas features that use generative AI** toggle to **on**.

At the project level:

1. In Atlas, go to the **Project Settings** page.

   1. If it's not already displayed, select the organization that contains your desired project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your desired project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Project Settings**.

   The [Project Settings](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fsettings%2FgroupSettings) page displays.

1. **Enable Generative AI features.**

   Set the **Enable Data Explorer Generative AI Features** toggle to **on**.

1. **(Optional) Read MongoDB's GenAI FAQs.**

   For more information about generative AI usage in Data Explorer, see:

   - atlas-ui-ai-data-usage
   - [FAQ For MongoDB Generative AI Features](https://www.mongodb.com/docs/generative-ai-faq/)

## Access the Intelligent Assistant

To access the intelligent assistant:

1. In Atlas, go to the **Data Explorer** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Data Explorer** under the **Database** heading. The [Data Explorer](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fmetrics%2FreplicaSet%2F%3Creplset%3E%2Fexplorer) displays.

IMPORTANT: You can also go to the **Clusters** page, and click **Data Explorer** under the **Shortcuts** heading.

![Data Explorer Page with collapsed AI Drawer](/images/de-collapse.png)

![Data ExplorerPage with opened AI drawer](/images/de-expand.png)

## Examples

The following scenarios show several uses of the intelligent assistant in Data Explorer.

### Explain Plan

Ask the intelligent assistant to analyze the output of your MongoDB Compass query:

**Input:**

```
Interpret this explain plan for me.
```

**Output:**

```
## Summary
- **Query Logic:** This pipeline counts the number of Italian restaurants in Manhattan by first filtering documents and then aggregating the count.
- **Performance:** Fair
- **Recommendations:** Create a compound index on `{ borough: 1, cuisine: 1 }` to optimize document retrieval.

## Details

### Query Logic
1. `$match`: Filters documents where `borough` is "Manhattan" and `cuisine` is "Italian".
2. `$count`: Counts the number of documents matching the filter and outputs the total.

### Performance Analysis
- The winning plan uses an index scan on the `cuisine` field (`cuisine_1`) and applies an in-memory filter for `borough` ("Manhattan").
- Both `totalKeysExamined` and `totalDocsExamined` are high (43,207), which implies that every document with `cuisine: "Italian"` is examined and then filtered by `borough`.
- No compound index exists, meaning MongoDB cannot efficiently combine both filter criteria in a single index lookup.
- The pipeline does properly filter before counting, but index usage is only partial, causing increased resource usage.

### Recommendations
- Create a compound index on `{ borough: 1, cuisine: 1 }` to allow MongoDB to retrieve documents matching both criteria directly from the index, minimizing documents examined.
- This optimization will reduce execution time and system load without changing the output of your aggregation pipeline.

Below is the recommended Aggregation Pipeline. This optimized Aggregation Pipeline will leverage the compound index to scan fewer documents, as both filter criteria are present in the index.

```json
[
  { "$match": { "borough": "Manhattan", "cuisine": "Italian" } },
  { "$count": "totalItalianRestaurants" }
]
```

### Follow-Up Questions
1. How do I create a compound index in MongoDB Compass or via a shell command?
2. What other fields should I consider adding to my indexes for restaurant lookups?
3. How can I further optimize aggregations when working with larger datasets?
```

### User questions

The following examples demonstrate questions you can ask the intelligent assistant about Data Explorer functions and queries.

**Input:**

```
How is MongoDB data modeling different from data modeling with a relational
database?
```

**Output:**

```
MongoDB data modeling differs from relational database (RDBMS) data 
modeling in some important ways:

1. Document-Oriented vs. Table-Oriented

  - MongoDB stores data as flexible, hierarchical JSON-like documents.
  - Relational databases use tables with fixed schemas and columns.

2. Schema Flexibility

  - In MongoDB, collections can contain documents with different structures. Schema enforcement is optional.
  - RDBMS tables have a strict schema that enforces data types and relationships.

3. Embedding vs. Normalization

  - MongoDB encourages embedding related data inside documents when appropriate (denormalization). Example: An order document may include an array of items directly inside it.
  - Relational databases emphasize normalization—splitting data across tables connected by foreign keys.

4. Relationships

  - MongoDB does support references between documents (similar to foreign keys), but joins are less central and not as efficient as in RDBMS.
  - RDBMSs use normalized data and JOIN operations to relate tables efficiently.

5. Performance & Scaling

  - MongoDB’s data model enables horizontal scaling (sharding), making it well-suited for large, distributed datasets.
  - RDBMS scaling typically relies on vertical scaling or complex clustering; JOINs can become expensive as data grows.

6. Use Cases and Design Philosophy 

  - MongoDB models are often designed to optimize common query patterns, reducing the need for joins and complex queries.
  - RDBMS models are designed for transactional consistency and for representing organized, well-structured relationships.

In summary, MongoDB encourages a more flexible, application-driven 
approach to modeling, while relational databases rely on rigid schemas 
and normalized relationships. Compass provides tools to visually explore 
your document structures and relationships, which can be especially 
helpful as your model evolves.
```

**Input:**

```
How can I combine multiple conditions in a single $search query?
```

**Output:**

```
To combine multiple conditions in a single $search query in MongoDB Atlas
Search (including Compass), you should use the compound operator. The 
compound operator allows you to specify multiple criteria using must, 
mustNot, filter, and should clauses.

Example: Combine Multiple Conditions

Suppose you want to:

- Match documents where the plot field contains "baseball"
- Exclude documents where the genres field contains "Comedy" or "Romance"

Your $search stage would look like this:

{
  "$search": {
    "compound": {
      "must": [
        {
          "text": {
            "query": "baseball",
            "path": "plot"
           }
        }
      ],
      "mustNot": [
        {
          "text": {
            "query": ["Comedy", "Romance"],
            "path": "genres"
          }
        }
      ]
   }
  }
}
```

## Learn More

- atlas-ui-ai-data-usage
