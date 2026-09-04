# Performance Insights

When Atlas determines that your schema or queries can be improved, it displays a performance insight. Performance insights show ways to improve your schema and data modeling practices. Use performance insights to learn best schema design practices and improve application performance.

## Use Cases

Performance insights are best followed early in your application development process. Starting your application with good data modeling practices helps prevent schema and performance issues as your application grows. Although Atlas provides performance insights at any stage of development, it can be difficult to make schema modifications in large-scale schemas that are used in production. Before you modify your schema based on performance insights, ensure that the suggestion makes sense for your application. For example, if Atlas suggests creating an index, make sure that index supports queries that are run frequently.

## Behavior

Performance insights are enabled automatically. Performance insights are generic, and do not use properties specific to your schema such as database or collection names. Atlas shows performance insights in the following scenarios:

| Scenario | Performance insight |
| --- | --- |
| You run a query or aggregation without an index. | Add an index to support the operation. |
| You run an aggregation pipeline that uses a `$lookup` stage. | Embed related data to avoid the need for a `$lookup` operation. |
| You run a `$text` or `$regex` query. | If possible, use [MongoDB Search](/atlas-search) to improve performance for text search queries. |
| Your database contains too many collections. | Reduce the number of collections. |
| Your documents contain an array field with too many elements. | Avoid unbounded arrays. |
| The data size of individual documents is too large. | Break up large documents into separate collections. |
| Your collection contains too many indexes. | Review your indexes and remove any that are unnecessary. |

## Learn More

- To learn more about data modeling in MongoDB, see manual-data-modeling-intro.
- To learn how to create effective indexes for your application, see [Indexing Strategies](/applications/indexes).
