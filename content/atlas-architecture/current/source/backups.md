# Guidance for Atlas Backups

MongoDB Atlas provides fully managed and customizable backups to ensure data retention and recovery:

- Cloud Backups: Supports full-copy snapshots and localized snapshot storage using your cloud provider's native snapshot capabilities. These snapshots are always incremental in nature for low cost and fast restores. You choose a backup policy that specifies the frequency and retention period of hourly, daily, weekly, monthly and yearly snapshots.
- Continuous Cloud Backups: Enhances standard cloud backups by offering Point In Time (PIT) recovery. This additive feature stores snapshots along with the cluster's oplog to capture data changes between snapshots, enabling you to recover your data to the exact moment (a point in time) right before any failure or event. This supports Recovery Point Objectives (RPOs) as low as 1 minute.

We don't recommend enabling backup for development and test environments. For staging and production environments, we recommend developing automated deployment templates that include the backup policy recommendations described in this page.

## Features for Atlas Backups

Atlas provides fully-managed backups of your data, including point-in-time data recovery and consistent, cluster-wide snapshots of all clusters, including sharded clusters. In Atlas, you can choose from five snapshot frequencies, each with its own retention period: hourly, daily, weekly, monthly, and yearly.

| Cloud Backups | This feature provides localized backup storage using the native snapshot functionality of your cluster's cloud service provider. Benefits include a strong default backup retention schedule of 12 months, full flexibility to customize snapshot and retention schedules, and the ability to set different snapshot frequencies (such as hourly for recovery, weekly or monthly for long-term retention) to meet industry regulations. You can access your backup data instantly, which is useful for auditing, compliance, or data recovery purposes. |
| --- | --- |
| Continuous Cloud Backups | This feature can be enabled on top of Cloud Backups to provide Point In Time (PIT) recovery. Continuous Cloud Backups work by storing snapshots along with the cluster's oplog up to a customizable Point In Time Restore (PITr) window, enabling you to restore to the last snapshot and then replay all operations since the snapshot was taken. This allows you to recover your data to the exact moment (a point in time) right before any failure or data loss event, like a cyber attack. |
| Multi-region Snapshot Distribution | This feature allows you to increase resilience by distributing copies of backup snapshots and oplogs across geographic regions in addition to the default primary region. This configuration meets compliance requirements of storing backups in different geographical locations to ensure disaster recovery in case of regional outages. To learn more, see [Snapshot Distribution](/backup/cloud-backup/snapshot-distribution/). |
| Backup Compliance Policy | This feature further secures business critical data by preventing all snapshots and oplogs stored in Atlas from being modified or deleted, guaranteeing that your backups are fully WORM (Write Once Read Many) compliant. Only a designated, authorized user can turn off this protection after completing a verification process with MongoDB support. There is a mandatory cooldown period to disable this feature so that an attacker cannot change the backup policy and export the data. To learn more, see [Configure a Backup Compliance Policy](https://www.mongodb.com/docs/atlas/backup/cloud-backup/backup-compliance-policy/). |

## Recommendations for Atlas Backups
### Recommendations for Backup Strategy

You must align your backup strategy with specific Recovery Point Objectives (RPO) and Recovery Time Objectives (RTO) to meet business continuity requirements, particularly for critical applications where near-zero RPO (Recovery Point Objective) and rapid recovery times are crucial. RPO (Recovery Point Objective) defines the maximum amount of data loss acceptable during a failure or disruption, while RTO (Recovery Time Objective) defines the maximum amount of time it takes for your cluster or service to recover. You must calculate your standards for RPO (Recovery Point Objective) and RTO (Recovery Time Objective) based on the criticality of your application. For example, mission-critical data typically requires a lower RPO (Recovery Point Objective) than clickstream analytics. The nature of your disaster scenario and your method of recovery affects your achievable RPO (Recovery Point Objective) and RTO (Recovery Time Objective). The default high availability architecture in MongoDB supports automatic failover to recover from temporary cloud provider outages with a near-zero RTO (Recovery Time Objective) and RPO (Recovery Point Objective), depending on the scope of the outage and your chosen deployment paradigm. To learn more about deployment configurations that support automatic failover, see arch-center-high-availability. For disaster scenarios that require you to restore from backup, such as a code error that corrupts your entire database or an accidental cluster deletion, your deployment's RTO (Recovery Time Objective) and RPO (Recovery Point Objective) depend on the following:

- RPO (Recovery Point Objective) depends on the snapshot interval defined in your backup policy. When Continuous Cloud Backups is disabled, your RPO (Recovery Point Objective) directly corresponds to the amount of time between snapshots. If you take backup snapshots every four hours, for example, then you can lose up to four hours of data during a failure event. If you enable Continuous Cloud Backups, then you can perform PIT (Point in Time) restores that guarantee an RPO (Recovery Point Objective) as low as one minute within your customizable PITr (Point in Time Restore) window.
- RTO (Recovery Time Objective) depends on the size of your backup and the efficiency of your restore operation. Large replica sets (and shards) take longer to restore. To speed up restores, Atlas automatically performs optimized direct attach restores for clusters that are based in the same project and regions where you store snapshot copies. If there is no local snapshot available, or if the cluster uses NVMe storage instead of standard General or Low CPU storage, then Atlas defaults to slower streaming restores. Additionally, if you enable Continuous Cloud Backups, then Atlas must replay operations after restoring to the last snapshot in order to complete a PIT (Point in Time) restore. The less time between snapshots, the fewer operations must be replayed. Therefore, you can lower your RTO (Recovery Time Objective) by prioritizing optimized restores and requiring more frequent snapshots in your backup policy.

To ensure a comprehensive backup strategy, we recommend using the following default backup policy as a starting point. Adjust this policy according to your business's data retention and disaster recovery needs.

| Policy Type | Tier | Continuous Cloud Backup | Snapshot Taken | Snapshot Retained |
| --- | --- | --- | --- | --- |
| Hourly | NVMe (non-volatile memory express) | Enabled | Every 12 hours | 7 days |
| Hourly | non-NVMe (non-volatile memory express) | Enabled | Every 6 hours | 7 days |
| Daily | All | Either | Every day | 7 days |
| Weekly | All | Either | Every Saturday | 4 weeks |
| Monthly | All | Either | Last day of the month | 12 months |
| Yearly | All | Either | Every 1st of December | 1 year |

> **See also:**
> [Choosing the Right Atlas Backup Policy](/learn/article/choosing-the-right-atlas-backup-policy)

### Recommendations for Backup Distribution

To further enhance resilience in a single-region deployment, we recommend that you configure Atlas to copy snapshots from the primary region to a secondary backup region to ensure that you can still restore to previous versions if the primary region goes down. To learn more, see snapshot-distribution.

### Recommendations for Backup Compliance Policy

We recommend enforcing Atlas's [Backup Compliance Policy](https://www.mongodb.com/docs/atlas/backup/cloud-backup/backup-compliance-policy/) to prevent unauthorized modifications or deletions of backups, thereby ensuring data protection and robust disaster recovery.

### Recommendations for PIT Recovery

Continuous Cloud Backups enable precise Point In Time (PIT) recovery, which minimizes data loss during failures. Atlas can quickly recover to the exact timestamp before a failure event, guaranteeing at least a one minute RPO (Recovery Point Objective). This is because Atlas restores the most recent snapshot from before the desired point in time and then replays the oplog changes to restore to that particular point. Recovery times can vary due to cloud provider disk warming and how much of the oplog must replay during recovery. Your cluster performance might be slow until the cloud provider disk warming completes after a restore. If you can be flexible in your requirements for recovery, we recommend designing templates that identify the best compromise between reasonable recovery options and cost.

### Recommendations for Backup Costs

To optimize Atlas backup costs, you can adjust backup frequency and retention policies to align with data criticality and reduce unnecessary storage expenses. For example, we recommend that you disable backups in lower environments, such as development or test clusters where data recovery is not critical. For upper environments, we recommend balancing cross-region data transfer costs against high availability requirements when distributing backup snapshots across regions.

## Automation Examples: Atlas Backups

The following examples enable backup and restore operations using Atlas tools for automation. These examples apply only for staging and production environments where backup is enabled for the cluster.

**CLI**

Run the following command to take a backup snapshot for the cluster named myDemo and retain the snapshot for 7 days:

```shell
atlas backups snapshots create myDemo --desc "my backup snapshot" --retention 7
```

Enable backup compliance policy for your project with a designated, authorized user (`governance@example.org`) who alone can turn off this protection after completing a verification process with MongoDB support.

```shell
atlas backups compliancePolicy enable \ 
  --projectId 67212db237c5766221eb6ad9 \
  --authorizedEmail governance@example.org \
  --authorizedUserFirstName john \
  --authorizedUserLastName doe
```

Run the following command to create a compliance policy for scheduled backup snapshots that enforces the number of times snapshots must be taken, which is set to every `6` hours, and the duration for retaining the snapshots, which is set to `1` month.

```shell
atlas backups compliancePolicy policies scheduled create \ 
  --projectId 67212db237c5766221eb6ad9 \
  --frequencyInterval 6 \
  --frequencyType hourly \
  --retentionValue 1 \
  --retentionUnit months
```

---

**Terraform**

The following examples demonstrate how to configure backups during deployment. Before you can create resources with Terraform, you must:

- Create your paying organization and create an API key for the paying organization. Store your API key as environment variables by running the following command in the terminal:

   ```
   export MONGODB_ATLAS_PUBLIC_KEY="<insert your public key here>"
   export MONGODB_ATLAS_PRIVATE_KEY="<insert your private key here>"
   ```

- [Install Terraform](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli).

> **Important:**
> The following examples use MongoDB Atlas Terraform Provider version 2.x (`~> 2.2`). If you're upgrading from provider version 1.x, see the [2.0.0 Upgrade Guide](https://registry.terraform.io/providers/mongodb/mongodbatlas/latest/docs/guides/2.0.0-upgrade-guide) for breaking changes and migration steps. The examples use the `mongodbatlas_advanced_cluster` resource with v2.x syntax.
>

### Common Files

You must create the following files for each example. Place the files for each example in their own directory. Change the IDs and names to use your values. Then run the commands to initialize Terraform, view the Terraform plan, and apply the changes.

#### variables.tf

```yaml
variable "org_id" {
  description = "Atlas organization ID"
  type        = string
}

variable "project_name" {
  description = "Atlas project name"
  type        = string
}

variable "cluster_name" {
  description = "Atlas Cluster Name"
  type        = string
}

variable "point_in_time_utc_seconds" {
  description = "PIT in UTC"
  default     = 0
  type        = number
}
```

### Configure Backup Schedule for the Cluster

Use the following to configure a backup schedule for the cluster with the following snapshot frequency and retention:

- Hourly: Every 12 hours, retain for 7 days
- Daily: Once a day, retain for 7 days
- Weekly: Saturday, retain for 4 weeks
- Monthly: Last day of month, retain for 3 months

#### main.tf

```
locals {
 atlas_clusters = {
    "cluster_1" = { name = "m10-aws-1e", region = "US_EAST_1" },
    "cluster_2" = { name = "m10-aws-2e", region = "US_EAST_2" },
  }
}

resource "mongodbatlas_project" "atlas-project" {
  org_id = var.org_id
  name   = var.project_name
}

resource "mongodbatlas_advanced_cluster" "automated_backup_test_cluster" {
  for_each     = local.atlas_clusters
  project_id   = mongodbatlas_project.atlas-project.id
 name         = each.value.name
  cluster_type = "REPLICASET"

 replication_specs = [
   {
      region_configs = [
        {
          electable_specs = {
            instance_size = "M10"
            node_count    = 3
          }
          analytics_specs = {
            instance_size = "M10"
            node_count    = 1
          }

          provider_name = "AWS"
          region_name   = each.value.region
          priority      = 7
        }
      ]
    }
  ]

  backup_enabled = true # enable cloud backup snapshots
  pit_enabled    = true
}

resource "mongodbatlas_cloud_backup_schedule" "test" {
  for_each                 = local.atlas_clusters
  project_id               = mongodbatlas_project.atlas-project.id
  cluster_name             = mongodbatlas_advanced_cluster.automated_backup_test_cluster[each.key].name
  reference_hour_of_day    = 3  # backup start hour in UTC
  reference_minute_of_hour = 45 # backup start minute in UTC
  restore_window_days      = 7  # Restore window for near-zero RPO

  copy_settings {
    cloud_provider = "AWS"
    frequencies = ["HOURLY",
      "DAILY",
      "WEEKLY",
      "MONTHLY",
      "YEARLY",
    "ON_DEMAND"]
    region_name        = "US_WEST_1"
    zone_id            = mongodbatlas_advanced_cluster.automated_backup_test_cluster[each.key].replication_specs.*.zone_id[0]
    should_copy_oplogs = true
  }

  policy_item_hourly {
    frequency_interval = 12 # backup every 12 hours, accepted values = 1, 2, 4, 6, 8, 12 -> every n hours
    retention_unit     = "days"
    retention_value    = 7  # retain for 7 days
  }
  policy_item_daily {
    frequency_interval = 1 # backup every day, accepted values = 1 -> every 1 day
    retention_unit     = "days"
   retention_value     = 7 # retain for 7 days
  }
  policy_item_weekly {
    frequency_interval = 7 # every Sunday, accepted values = 1 to 7 -> every 1=Monday,2=Tuesday,3=Wednesday,4=Thursday,5=Friday,6=Saturday,7=Sunday day of the week
    retention_unit     = "weeks"
    retention_value    = 4 # retain for 4 weeks
  }
  policy_item_monthly {
    frequency_interval = 28 # accepted values = 1 to 28 -> 1 to 28 every nth day of the month  
    retention_unit  = "months"
    retention_value = 3 # retain for 3 months
  }

  depends_on = [
    mongodbatlas_advanced_cluster.automated_backup_test_cluster
  ]
}
```

### Configure Backup and PIT Restore for the Cluster

Use the following to configure cloud backup snapshot and PIT restore job.

#### main.tf

```shell
# Create a project
resource "mongodbatlas_project" "project_test" {
  name   = var.project_name
  org_id = var.org_id
}

# Create a cluster with 3 nodes
resource "mongodbatlas_advanced_cluster" "cluster_test" {
  project_id   = mongodbatlas_project.project_test.id
  name         = var.cluster_name
  cluster_type = "REPLICASET"

  backup_enabled         = true # enable cloud provider snapshots
  pit_enabled            = true
  retain_backups_enabled = true # keep the backup snapshopts once the cluster is deleted

  replication_specs = [
    {
      region_configs = [
        {
          priority      = 7
          provider_name = "AWS"
          region_name   = "US_EAST_1"
          electable_specs = {
            instance_size = "M10"
            node_count    = 3
          }
        }
      ]
    }
  ]
}

# Specify number of days to retain backup snapshots
resource "mongodbatlas_cloud_backup_snapshot" "test" {
  project_id        = mongodbatlas_advanced_cluster.cluster_test.project_id
  cluster_name      = mongodbatlas_advanced_cluster.cluster_test.name
  description       = "My description"
  retention_in_days = "1"
}

# Specify the snapshot ID to use to restore
resource "mongodbatlas_cloud_backup_snapshot_restore_job" "test" {
  count        = (var.point_in_time_utc_seconds == 0 ? 0 : 1)
  project_id   = mongodbatlas_cloud_backup_snapshot.test.project_id
  cluster_name = mongodbatlas_cloud_backup_snapshot.test.cluster_name
  snapshot_id  = mongodbatlas_cloud_backup_snapshot.test.id 

  delivery_type_config {
    point_in_time             = true
    target_cluster_name       = mongodbatlas_advanced_cluster.cluster_test.name
    target_project_id         = mongodbatlas_advanced_cluster.cluster_test.project_id
    point_in_time_utc_seconds = var.point_in_time_utc_seconds
  }
}
```

---
