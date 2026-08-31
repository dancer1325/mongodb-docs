# Guidance for Atlas Disaster Recovery

It is critical for enterprises to plan for disaster recovery. We strongly recommend that you prepare a comprehensive disaster recovery (DR) plan that includes elements such as:

- Your designated recovery point objective (RPO)
- Your designated recovery time objective (RTO)
- Automated processes that facilitate alignment with these objectives

Use the recommendations on this page to prepare for and respond to disasters. To learn more about proactive high availability configurations that can help with disaster recovery, see arch-center-ha-recs.

## Features for Atlas Disaster Recovery

To learn about Atlas features that support disaster recovery, see the following pages in the Atlas Architecture Center:

- arch-center-high-availability
- arch-center-backups

## Recommendations for Atlas Disaster Recovery

Use the following disaster recovery recommendations to create a DR (Disaster Recovery) plan for your organization. These recommendations provide information on steps to take in the event of a disaster. It is imperative that you test the plans in this section regularly (ideally quarterly, but at least semi-annually). Testing often helps prepare the Enterprise Database Management (EDM) team to respond to disasters while also helping to keep the instructions up to date. Some disaster recovery testing might require actions that cannot be performed by EDM users. In these cases, open a support case for the purpose of performing artificial outages at least a week in advance of when you plan on running a test exercise. The following diagram compares different disaster recovery scenarios and deployment configurations. The table shows the relative Recovery Time Objective (RTO) and Recovery Point Objective (RPO) benefits versus the deployment complexity and cost for each configuration. Note that replica set elections (automatic failover) do not result in data loss, while recovery from backups might involve some data loss depending on the backup frequency. "Control plane fault" refers to issues with the Atlas management infrastructure rather than your data nodes.

![An image showing relative complexity and RTO/RPO trade-offs for different disaster recovery configurations.](/includes/images/rto-rpo-table.png)

<details>
<summary>Single-Region Deployment Recommendations</summary>

Each cloud provider on which you can deploy Atlas clusters provides default data redundancy that helps to mitigate any outages:

- AWS (Amazon Web Services) stores objects on multiple devices across a minimum of three Availability Zones in an AWS (Amazon Web Services) Region.
- Microsoft Azure uses locally redundant storage (LRS) that replicates your data three times within a single data center in the selected region.
- Google Cloud spreads your data across multiple zones in the backup region.

To enhance your disaster recovery, you can configure Atlas to automatically create copies of your snapshots and oplogs in other regions. This ensures that, even if the primary region experiences an outage, you can restore your cluster using snapshot copies stored in other regions. Atlas optimizes restore speeds by selecting the most efficient option based on region availability, using copied snapshots if restoring to a region where those copies exist. Additionally, if the original snapshot is inaccessible due to a regional outage, Atlas will restore using the nearest available snapshot copy, minimizing downtime and improving recovery resilience. To learn more, see [Export Cloud Backup Snapshot](/backup/cloud-backup/export).
</details>

<details>
<summary>Multi-Region and Multi-Cloud Deployment Recommendations</summary>

Multi-region and multi-cloud deployments provide enhanced disaster recovery capabilities by distributing your cluster nodes across different geographic locations or cloud providers. This distribution helps ensure that if one region or cloud provider experiences an outage, your application can continue operating using nodes in unaffected locations. When configuring multi-region or multi-cloud deployments, ensure your backup strategy accounts for the distributed nature of your deployment, including setting appropriate backup retention periods based on your specific recovery requirements.
</details>

### All Deployment Paradigm Recommendations

The following recommendations apply to all deployment paradigms. This section covers the following disaster recovery procedures:

- arch-center-single-node-outage
- arch-center-regional-outage
- arch-center-provider-outage
- arch-center-atlas-outage
- arch-center-resource-capacity
- arch-center-resource-failure
- arch-center-data-deletion
- arch-center-driver-failure
- arch-center-data-corruption

#### Single Node Outage

If a single node in your replica set fails due to a partial regional outage, your deployment should still be available, assuming you have followed best practices. If you are reading from secondaries, you might experience degraded performance or potential outages in the event that a secondary node should fail, because of the increased load on the then underprovisioned cluster. You can test a primary node outage in Atlas using the Atlas UI\'s [Test Primary Failover](/tutorial/test-resilience/test-primary-failover/) feature or the Test Failover Atlas Administration API endpoint.

#### Regional Outage

Multi-region clusters, in the event of a regional outage, will automatically hold an election and identify a new primary node if necessary. This topology change will be automatically communicated to the application allowing it to take any necessary corrective action. In order to maintain application uptime in the event of a regional outage, your application itself must also be deployed with a multi-region topology. This requirement extends to include any third-party service your application may be integrated with. To learn more, see arch-center-paradigms-multi-region. If a single region outage or multi-region outage degrades the state of your cluster, follow these steps:

1. **Identify the region experiencing issues**

1. **Identify how many nodes are still online**

   You can find information about cluster health in the cluster\'s Overview tab of the Atlas UI.

1. **Determine how many nodes you require**

   Based on how many nodes are left online, determine how many new nodes you require to restore the replica set to a normal state. A normal state is a state in which the majority of nodes are available.

1. **Determine which regions are unlikely to be affected by the current outage**

   Depending on the cause of the outage, there may be additional regions in the near future that will also experience unscheduled outages. For example, if the outages were caused by a natural disaster on the east coast of the United States, you should avoid regions on the east coast of the United States in case there are additional issues.

1. **Add nodes to the regions you identified**

   Add the required number of nodes for a normal state across regions that are unlikely to be affected by the cause of the outage. To reconfigure a replica set during an outage by adding regions or nodes, see [Reconfigure a Replica Set During a Regional Outage](/reconfigure-replica-set-during-regional-outage/).

1. **(Optional) Add additional nodes**

   In addition to adding nodes to restore your replica set to a normal state, you can add additional nodes to match the topology of your replica set before the disaster.

You can test a region outage in Atlas using the Atlas UI\'s [Simulate Outage](/tutorial/test-resilience/simulate-regional-outage/) feature or the Start an Outage Simulation Atlas Administration API endpoint.

#### Cloud Provider Outage

With multi-cloud clusters, you can select electable nodes across cloud providers to maintain high availability. Should the provider in which your primary node is deployed become unavailable, Atlas automatically elects new primary nodes to ensure continuous operation. For example, you can create electable nodes on AWS (Amazon Web Services), Google Cloud, and Microsoft Azure to ensure that if one cloud provider experiences an outage, an electable node on a separate provider can automatically take over as your cluster's primary node. To learn more, see arch-center-paradigms-multi-cloud. Most multi-region Atlas clusters will recover automatically from a single region outage. To learn more, see the High Availability section and Multi-region Deployment page. In the case that regional outages have knocked out a majority of nodes, you must determine how many more nodes you need to add in order for a majority of nodes to be healthy. In the highly unlikely event that an entire cloud provider is unavailable, follow these steps to bring your deployment back online:

1. **Determine when the cloud provider outage began**

   You need this information later in this procedure to restore your deployment.

1. **Identify the alternative cloud provider you would like to deploy your new cluster on**

   For a list of cloud providers and information, see [Cloud Providers](/reference/cloud-providers).

1. **If you store backups across multiple cloud providers, as a cloud provider outage implies that any backups stored on the primary cloud provider are necessarily unavailable, find the most recent available snapshot taken of the cluster before the outage began**

   To learn how to view your backup snapshots, see [View M10+ Backup Snapshots](/backup/cloud-backup/dedicated-cluster-backup/#view-m10--backup-snapshots).

1. **Create a new cluster with the alternative cloud provider**

   Your new cluster must have an identical topology of the original cluster. Alternatively, instead of creating a full new cluster, you can also add new nodes hosted by an alternative cloud provider to the existing cluster.

1. **Restore the most recent snapshot from the previous step into the new cluster**

   To learn how to restore your snapshot, see [Restore Your Cluster](/backup/cloud-backup/restore-overview/).

1. **Switch any applications that connect to the old cluster to the newly-created cluster**

   To find the new connection string, see [Connect via Drivers](/driver-connection). Review your application stack as you likely need to redeploy it onto the new cloud provider.

#### Atlas Outage

In the highly unlikely event that the Atlas Control Plane and the Atlas UI are unavailable, your cluster is still available and accessible. To learn more, see [Platform Reliability](https://www.mongodb.com/products/platform/trust#reliability). Open a high-priority [support ticket](/support/#request-support) to investigate this further.

#### Resource Capacity Issues

Computational resource (such as disk space, RAM, or CPU) capacity issues can result from poor planning or unexpected database traffic. This behavior might not be a result of a disaster. If a computational resource reaches the maximum allocated amount and causes a disaster, follow these steps:

1. **Identify which computational resource is maxed out by using the Real Time Performance Panel or Atlas metrics**

   To view your resource utilization in the Atlas UI, see [Monitor Real-Time Performance](/real-time-performance-panel). To view metrics with the Atlas Administration API, see Monitoring and Logs.

1. **Determine how much more of the maxed-out resource you need to alleviate performance issues**

1. **Allocate the necessary resources**

   Note that Atlas will perform this change in a rolling manner, so it should not have any major impact on your applications. To learn how to allocate more resources, see [Edit a Cluster](/scale-cluster/#edit-a-cluster).

1. **Monitor your cluster to determine whether any other issues exist after the change**

#### Resource Failure

> **Important:**
> This is a temporary solution intended to shorten overall system downtime. Once the underlying issue resolves, merge the data from the newly-created cluster into the original cluster and point all applications back to the original cluster.

If a computational resource fails and causes your cluster to become unavailable, follow these steps:

1. **Open a high-priority [support ticket](/support/#request-support)**

1. **Create a new cluster with the same topology as the failing cluster**

1. **Restore the most recent backup into the newly-created cluster**

   To learn how to restore your snapshot, see [Restore Your Cluster](/backup/cloud-backup/restore-overview/).

1. **Point all applications using the failing cluster to the newly-created cluster**

#### Deletion of Production Data

Production data might be accidentally deleted due to human error or a bug in the application built on top of the database. If the cluster itself was accidentally deleted, Atlas might retain the volume temporarily. If the contents of a collection or database have been deleted, follow these steps to restore your data:

1. **Determine the date and time or oplog timestamp when the data was deleted**

1. **Create a copy of the current state of the collection or database, if it contains any data**

   You can use [mongoexport](https://www.mongodb.com/docs/database-tools/mongoexport/) to create a copy.

1. **Restore your data**

   If the deletion occurred within the last 72 hours, and you configured continuous backup, use Point in Time (PIT) restore to restore from the point in time right before the deletion occurred. If the deletion did not occur in the past 72 hours, restore the most recent backup from before the deletion occurred into the cluster. To learn more, see [Restore Your Cluster](/backup/cloud-backup/restore-overview/).

1. **If you created a copy of your data, import the new data you exported**

   You can use [mongoimport](https://www.mongodb.com/docs/database-tools/mongoimport/) with [upsert mode](https://www.mongodb.com/docs/database-tools/mongoimport/#std-option-mongoimport.--mode) to import your data and ensure that any data that was modified or added is reflected properly in the collection or database.

#### Driver Failure

If a driver fails, follow these steps:

1. **Determine the issue**

   You can work with the technical support team during this step. Determine whether the issue is related to an outdated driver version or a recently updated driver version.

1. **Identify the appropriate driver version to resolve the issue**

   - If you are using an outdated driver, check if upgrading to a newer version resolves the issue. Most driver problems are fixed in newer releases.
   - If you recently upgraded your driver and suspect the new version introduced the issue, consider reverting to the previous working version.

1. **Evaluate if any other changes are required to move to the target driver version**

   This might include application code or query changes. For example, there might be breaking changes if you are moving between major versions, or new features available if upgrading.

1. **Test the changes in a non-production environment**

1. **If you don't encounter issues while testing, deploy the new driver version**

   Ensure that any other changes from the previous step are reflected in the production environment.

#### Data Corruption

> **Important:**
> This is a temporary solution intended to shorten overall system downtime. Once the underlying issue resolves, merge the data from the newly-created cluster into the original cluster and point all applications back to the original cluster.

If your underlying data becomes corrupted, follow these steps:

1. **Open a high-priority [support ticket](/support/#request-support)**

1. **Create a new cluster with the same topology as the failing cluster**

1. **Restore the most recent backup into the newly-created cluster**

   To learn how to restore your snapshot, see [Restore Your Cluster](/backup/cloud-backup/restore-overview/).

1. **Check the restored data to confirm that the corruption does not exist**

1. **Point all applications using the failing cluster to the newly-created cluster**
