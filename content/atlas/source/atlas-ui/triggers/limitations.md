# Triggers Limitations

There are several guidelines to keep in mind when architecting how your tools and clients interact with MongoDB through Triggers. Keep this guidance in mind when deciding how to structure queries, selecting which CRUD and aggregation operations to use, and determining how to handle concurrent workloads.

## Aggregation

Triggers support all [aggregation pipeline stages](/reference/operator/aggregation-pipeline/) in system functions except for `$currentOp` and `$indexStats`. For security reasons, only a subset of aggregation pipeline stages are supported in user functions. For a list of pipeline stages that are available and their allowed function context, see Aggregation.

## Batch Loading

When data is bulk/batch loaded into MongoDB Atlas, you may see a delay in data appearing on devices while Atlas processes changes.

## Time Series Collections

You *cannot* define Database Triggers on a [time series collections](/core/timeseries-collections/). This is because time series collections do not yet support [change streams](/changeStreams/).

## Flex Clusters

You *cannot* define Database Triggers on a Flex Cluster. This is because Flex Clusters don't support [change streams](/changeStreams/).

## Federated Database Instances

You *cannot* define Database Triggers on a federated database instance because federated database instances don't support [change streams](/changeStreams/).

## Change Streams

Atlas limits the total number of change streams open against a given cluster based on the cluster's size. The following table lists the limitations for each cluster size:

| Cluster Size | Maximum Number of Change Streams |
| --- | --- |
| Free Tier (`M0`) | 5 |
| Flex Clusters | 10 |
| Small, Dedicated Clusters (`M10`/`M20`) | 100 |
| Standard Clusters (`M30`/`M40`) | 1000 |
| Standard Clusters (`M50` - `M90`) | 1000 |
| High-Power Clusters (`M100+`) | 1000 |

> **Note:**
> Atlas opens a single change stream on each collection that is associated with a Database Trigger.

## Database Commands

You can call a limited subset of database commands when connected to a MongoDB cluster over the [wire protocol](/reference/mongodb-wire-protocol). For a list of supported commands, see Database Commands.

> **Note:**
> App Services does *not* support any database commands in Atlas Functions.

## MongoDB Version Requirements

You can access most of the CRUD and Aggregation functionality of MongoDB version 3.6. However, Triggers do not support all operations and features available in standard tools and clients. For a list of specific MongoDB operations that are available when you connect to MongoDB through Triggers, see the CRUD & Aggregation API reference.

## Query Options

Triggers support all query options in system functions. For a list of specific options that are available when you connect to MongoDB through Triggers, see Query Options.

## Query Results

MongoDB queries executed through Triggers can return a maximum of 50,000 documents. If you need to return more documents, consider paginating your query.

## Request Traffic

Atlas limits request traffic to the following defaults:

- 10,000 concurrent requests. Any requests made beyond the concurrent request limit receive an HTTP response status code of [429 - Too Many Requests](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/429).

Atlas can handle requests many times the above limits. However, these limits are put in place to ensure applications scale responsibly and to prevent DOS attacks and unintended billing charges. You can request a higher limit by [filing a support ticket](https://support.mongodb.com/welcome).

## Connection Pooling

Atlas uses connection pooling to reduce the overhead of frequently opening and closing connections between requests and Trigger executions. Connections are opened as needed. Connection pooling is dependent on several factors:

- Cluster Tier. The higher the cluster tier, the more connections available in the pool.
- Deployment Mode. Global deployments use multiple servers in each region, and therefore have an overall larger connection pool.
- Services. Each service has an independent connection pool, so the number of services in your app does not impact the number of connections available.
