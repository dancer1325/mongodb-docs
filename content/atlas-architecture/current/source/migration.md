# Migration

You can migrate data from your on-premises MongoDB deployments to Atlas using one of a variety of methods. We recommend using Atlas [live migration](/import/) when possible because it automates many of the tasks with the least amount of downtime, but you can use other tools that accommodate the variety and complexity inherent to database migration.

## Live Migration Overview

Atlas live migration automates moving data from on-premises MongoDB databases to Atlas. Atlas live migration includes the following features:

- The migration host always encrypts traffic to the Atlas cluster. Traffic to, from, and between Atlas nodes is always encrypted, and this feature can't be disabled. Only users with specific Role-Based Access Control (RBAC) database roles (such as backup, readAnyDatabase, or clusterMonitor), and Atlas Project Owner can initiate a live migration. Users authenticate to clusters using [SCRAM-SHA-1 or SCRAM-SHA-256](/core/security-scram/).
- Live migration automates most tasks. For the fully-managed "pull" method, live migration monitors key metrics, provisions the migration servers and performs the strict sequencing of the migration commands. Furthermore, you can also select what Atlas Destination cluster tier configurations you would like to migrate to.
- Detailed instructions help you scale destination clusters to control costs. Recommendations include appropriate cluster sizing and temporary scaling, followed by resizing to optimal levels post-migration.
- Live migration uses [Mongosync](https://www.mongodb.com/docs/mongosync/current/) behind the scenes, which facilitates fast cutover through parallel data copying. Processes manage temporary network interruptions and cluster elections, using continuous data synchronization and a final cutover phase to achieve minimal downtime. Retry mechanisms and pre-migration validations enhance resilience against interruptions.
- Monitor migrations with real-time status updates and notifications.

## Live Migration Methods

You can have a live migration server pull data into Atlas. The pull live migration method supports migration paths between specific MongoDB versions. See [Supported Migration Paths](/import/c2c-pull-live-migration/) to learn more. To migrate data from databases using unsupported versions of MongoDB, see [Migrate or Import Data](/import/) or arch-center-manual-migration.

- **Pull data into Atlas.** Atlas pulls data from the source MongoDB deployment and requires access to the source deployment through the deployment's firewall. When the clusters are fully synced, you must follow the cutover process of stopping write operations on the source, redirect applications to the Atlas cluster, and restart them. The following considerations apply:

   - Best for deployments not monitored by Cloud Manager or Ops Manager.
   - The source database must be publicly accessible to allow inbound access from the live migration server.
   - Doesn't support VPC peering or private endpoints for either the source or destination cluster.
   - Source and destination cluster topologies must match. For example, both must be replica sets or sharded clusters with the same number of shards.
   - Plan for minimal downtime during cutover, to stop writes and restart applications with a new connection string. The migration process is CPU-intensive on the destination cluster and requires significant network bandwidth.
   - To ensure a smooth migration process, confirm that the source cluster's oplog size is adequate to cover the entire migration duration. For the source cluster, the live migration's lag window should stay within the bounds of the oplog replication lag window. You can fulfill this requirement by increasing the minimum oplog window or by increasing the fixed oplog size. For the destination cluster, MongoDB recommends that you choose a destination cluster tier that is at least two tiers above the source cluster for the duration of the migration. If storage auto-scaling is disabled on the destination cluster, increase the oplog size on the destination cluster to a high enough fixed value. If storage auto-scaling is enabled on the destination cluster, set a high enough minimum oplog window on the destination cluster. See [Oplog Requirements](/import/c2c-pull-live-migration/#oplog-requirements/) to learn more.
   - For full migration recommendations and instructions, see c2c-pull-live-migration.

## Monitoring Migrations

To review both ongoing and past migrations, navigate to the Migration Hub page in Atlas. You can click each migration process for more detailed information, including the initial data copy time estimate and comprehensive progress reports. Use the cluster card to create, cutover, or cancel a migration.

To learn more, see monitor-migrations.

## Manual Migration Methods

If Atlas live migration can't satisfy the constraints of your migration requirements, you can bring data from existing MongoDB deployments, `JSON`, or `CSV` files into Atlas using one of the following tools that you run outside of Atlas.

| Tool | Description |
| --- | --- |
| [Mongosync](https://www.mongodb.com/docs/mongosync/current/) | The [Mongosync](https://www.mongodb.com/docs/mongosync/current/) binary is the primary process used by Atlas live migration. You can use standalone `mongosync` to migrate data from one cluster to a cluster in Atlas. Atlas syncs data from the source to the destination cluster until you cut your applications over to the destination Atlas cluster. |
| [mongomirror](/import/mongomirror/) | Migrate from a MongoDB *replica set* into an Atlas cluster without shutting down your existing replica set or applications. [mongomirror](/import/mongomirror/) does not import user/role data or copy the `config` database. |
| [mongorestore](/import/mongorestore/) | Seed an Atlas cluster with a `BSON` data backup dump taken from mongodump, of an existing MongoDB deployment. [mongorestore](/import/mongorestore/) does not restore `system.profile` collection data. |
| [mongoimport](/import/mongoimport/) | Load data from a `JSON` or a `CSV` file into an Atlas cluster. `mongoimport` uses [strict mode representation for certain BSON types](/reference/mongodb-extended-json). Note that using mongoimport should be limited to small datasets for testing and/or development purposes. |
| [MongoDB Compass](/import-export/) | Use a GUI (Graphical User Interface) to load data from a `JSON` or a `CSV` file into an Atlas cluster. Note that using MongoDB Compass should be limited to small datasets for testing and/or development purposes. |

You can also restore from an Atlas cluster backup data to another Atlas cluster. For information, see restore-overview.

If you are required to use either Atlas VNet peering or Private Link configurations, you don't want to allow direct connection from a third party to its source cluster, or if you don't already have or don't want to import the source cluster in Ops Manager or Cloud Manager, then MongoDB recommends the standalone mongosync approach. If you have relatively small datasets (<300 GB) to migrate, and can afford application downtime for an extended time period, then MongoDB recommends the [mongodump](/mongodump/) and [mongorestore](/mongorestore/) approach. If you have relatively small datasets (<300 GB) to migrate, no index concerns, and can afford application downtime for an extended time period, then MongoDB recommends the [mongoexport](/mongoexport/) and [mongoimport](/mongoimport/) approach.

## Cutover

When a migration reaches the "Ready for Cutover" status, click Cutover on target cluster followed by the Prepare to Cutover on the cluster card to initiate the cutover process. Upon successful completion of the cutover, reconfigure your application to point to the new destination cluster.

To learn more, see monitor-migrations.

## Next Steps

See the arch-center-hierarchy page to learn about the building blocks of your Atlas enterprise estate or use the left navigation to find features and best practices for each [Well-Architected Framework](https://www.mongodb.com/resources/products/capabilities/well-architected-framework) pillar.
