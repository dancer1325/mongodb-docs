# Guidance for Atlas Monitoring and Alerts

[**MongoDB Monitoring Tooling**](https://learn.mongodb.com/skills?openTab=monitoring%2C+tuning%2C+and+automation)

MongoDB Atlas has a robust set of built-in metrics, telemetry, and logs that you can leverage from Atlas or integrate into your third-party observability and alerting stack. This enables you to monitor and manage your Atlas deployments and respond to incidents proactively and in real time. Monitoring your deployment allows you to:

- Understand the health and status of your cluster
- Learn how operations running on the cluster are impacting the database
- Learn if your hardware is resource constrained
- Perform workload and query optimization
- Detect and react to real-time issues to improve your application stack

Atlas provides several metrics for monitoring and alerting. You can track the health, availability, consumption, and performance of your deployments in visual dashboards and by API (Application Programming Interface). You can also view various cluster metrics, monitor your database performance, configure alerts and alert notification, and download activity logs.

## Features for Atlas Monitoring and Alerts

| Metrics | Deployment metrics provide insight into hardware performance and database operation efficiency. Atlas collects metrics for your servers, databases, and MongoDB processes and stores metrics data at [various granularity levels](/monitor-cluster-metrics/#premium-monitoring-granularity). For each granularity level, Atlas computes metrics as averages of the reported metrics at the next finer granularity level. Many metrics have a burst reporting equivalent. You can monitor metrics in the Atlas UI using the Metrics view, Real-Time Performance Panel, Query Profiler, Performance Advisor and Namespace Insights page. You can also use the Atlas CLI or Atlas Administration API to funnel metrics into a tool of your choice. The following Atlas UI Metrics view shows the variety of metrics available to monitor for a sample three-node replica set: |
| --- | --- |
| Alerts | Atlas provides alerts for over 200 event types, allowing you to tailor alerts for precise monitoring. Atlas issues alerts for the database and server conditions that you configure in your alert settings. When a condition triggers an alert, Atlas displays a warning symbol on the cluster and sends alert notifications. You can use the Atlas UI, Atlas Administration API, Atlas CLI, and integrated Terraform resource to configure alerts and alert notification. |
| Monitoring | Atlas monitoring visualizations provide insights into various key metrics, including hardware performance and database operation efficiency. Tools such as real-time performance panels for network and operation visibility, query profilers for tracking efficiency trends, and automated index suggestions allow you to monitor and troubleshoot issues more effectively, driving greater operational efficiency. For example, these charts can help you understand the impact of server restarts and elections on database performance. |
| Logs | Atlas provides logs for each process in the cluster. Each process keeps an account of its activity in its own log file. You can download logs by using the Atlas UI, Atlas CLI, and Atlas Administration API. To learn more, see arch-center-auditing-logging. |

## Recommendations for Atlas Monitoring and Alerts

<details>
<summary>Single-Region Deployment Recommendations</summary>

Single-region deployments have no unique considerations for integrations with third-party alerting tools, such as Datadog and Prometheus. See arch-center-monitoring-alerts-recs-for-any-deployment.
</details>

<details>
<summary>Multi-Region and Multi-Cloud Deployment Recommendations</summary>

In multi-region clusters, consider potential data transfer costs associated with sending logs and metrics across regions. We recommend that you configure your logging and auditing system to minimize inter-region data transfers, possibly by keeping logs local to the region where they are generated.
</details>

### All Deployment Paradigm Recommendations

The following recommendations apply to all deployment paradigms.

#### Monitor by Using Metrics

To monitor your cluster or database performance, you can view the cluster metrics such as historical throughput, performance, and utilization metrics. The following table lists some (but not all) important categories of metrics to monitor:

| Atlas Cluster Operations and Connection Metrics | - Opcounters - Operation Execution Times - Query Executors and Query Targeting - Connections - Queues - Scan and Order |
| --- | --- |
| Hardware Metrics | - Normalized System CPU - Normalized Process CPU - Disk Latency - Disk IOPS - Disk Space Free - System Memory - Swap Usage |
| Replication Metrics | - Replication Lag - Replication Oplog Window - Replication Headroom - Oplog GB/Hour - Opcounters -repl |

You can use the Atlas UI, Atlas Administration API, and Atlas CLI to view Atlas cluster metrics. Additionally, Atlas provides built-in integrations with third-party tools such as Datadog and Prometheus, and you can also leverage the Atlas Administration API to integrate with other custom metrics tools. To learn more, see monitor-cluster-metrics.

#### Monitor by Configuring Alerts

Atlas extends into your existing observability stack so that you can get alerts and make data-driven decisions without replacing your current tools or changing your workflows. Atlas can send alert notifications with third-party tools like [Microsoft Teams](/tutorial/integrate-msft-teams), PagerDuty, DataDog, [Opsgenie](/command/atlas-integrations-create-OPS_GENIE/), [Splunk On-Call](/command/atlas-integrations-create-VICTOR_OPS/), and [other tools](/tutorial/third-party-service-integrations/) to give you visibility into both database and full-stack performance in the same place. Configure alerts and notifications for security events, such as failed login attempts, unusual access patterns, and data breaches. In dev and test environments, we recommend configuring alerts for clusters that have been inactive for seven or more days to help you identify which clusters can be shut down to save costs. When you [view alerts in the Atlas UI](/alert-resolutions/#view-alerts), we recommend that you use available filters to limit results by host, replica set, cluster, shard, and more to help focus on the most critical alerts.

##### Recommended Atlas Alert Configurations

At the minimum, we recommend configuring the following alerts. These alerts recommendations provide a baseline, but you should adjust them based on your workload characteristics. Where "high priority" conditions are specified in the following table, we recommend that you configure multiple alerts for the same condition, one for a low priority level of severity, and one for a high priority level of severity. This allows you to configure alert notification settings for each separately. Atlas configures some alerts by default. To learn about the default alert settings, see default-alert-settings.

| Condition | Recommended Alert Threshold: Low Priority | Recommended Alert Threshold: High Priority | Key Insights |
| --- | --- | --- | --- |
| Oplog Window | < 24h for 5 minutes | < 1h for 10 minutes | .. include:: /includes/cloud-docs/shared-metric-description-oplog-window.rst |
| [Election](/core/replica-set-elections/) events | > 3 for 5 minutes | > 30 for 5 minutes | Monitor election events, which occur when a primary node steps down and a secondary node is elected as the new primary. Frequent election events can disrupt operations and impact availability, causing temporary write unavailability and possible rollback of data. Keeping election events to a minimum ensures consistent write operations and stable cluster performance. |
| Read [IOPS](/reference/alert-resolutions/disk-io-utilization/) | > 4000 for 2 minutes | > 9000 for 5 minutes | .. include:: /includes/cloud-docs/shared-metric-description-iops.rst |
| Write [IOPS](/reference/alert-resolutions/disk-io-utilization/) | > 4000 for 2 minutes | > 9000 for 5 minutes | .. include:: /includes/cloud-docs/shared-metric-description-iops.rst |
| Read Latency | > 20ms for 5 minutes | > 50 s for 5 minutes | .. include:: /includes/cloud-docs/shared-metric-description-latency.rst |
| Write Latency | > 20ms for 5 minutes | > 50ms for more than 5 minutes | .. include:: /includes/cloud-docs/shared-metric-description-latency.rst |
| Swap use | > 2GB for 15 minutes | > 2GB for 15 minutes | .. include:: /includes/cloud-docs/shared-metric-description-memory.rst |
| Host down | 15 minutes | 24 hours | Monitor your hosts to detect downtime promptly. A host down for more than 15 minutes can impact availability, while downtime exceeding 24 hours is critical, risking data accessibility and application performance. |
| No primary | 5 minutes | 5 minutes | Monitor the status of your replica sets to identify instances where there is no primary node. A lack of a primary for more than 5 minutes can halt write operations and impact application functionality. |
| Missing active `mongos` | 15 minutes | 15 minutes | Monitor the status of active `mongos` processes to ensure effective query routing in sharded clusters. A missing `mongos` can disrupt query routing. |
| Page faults | > 50/second for 5 minutes | > 100/second for 5 minutes | .. include:: /includes/cloud-docs/shared-metric-description-page-faults.rst |
| Replication lag | > 240 second for 5 minutes | > 1 hour for 5 minutes | .. include:: /includes/cloud-docs/shared-metric-description-replication-lag.rst |
| Failed backup | Any occurrence | None | Track backup operations to ensure data integrity. A failed backup can compromise data availability. |
| Restored backup | Any occurrence | None | Verify restored backups to ensure data integrity and system functionality. |
| Fallback snapshot failed | Any occurrence | None | Monitor fallback snapshot operations to ensure data redundancy and recovery capability. |
| Backup schedule behind | > 12 hours | > 12 hours | Check backup schedules to ensure they are on track. Falling behind can risk data loss and compromise recovery plans. |
| Queued Reads | > 0-10 | > 10+ | Monitor queued reads to ensure efficient data retrieval. High levels of queued reads may indicate resource constraints or performance bottlenecks, requiring optimization to maintain system responsiveness. |
| Queued Writes | > 0-10 | > 10+ | Monitor queued writes to maintain efficient data processing. High levels of queued writes may signal resource constraints or performance bottlenecks, requiring optimization to maintain system responsiveness. |
| Restarts last hour | > 2 | > 2 | Track the number of restarts in the last hour to detect instability or configuration issues. Frequent restarts can indicate underlying problems that require immediate investigation to maintain system reliability and uptime. |
| [Primary election](/core/replica-set-elections/) | Any occurrence | None | Monitor primary elections to ensure stable cluster operations. Frequent elections can indicate network issues or resource constraints, potentially impacting the availability and performance of the database. |
| Maintenance no longer needed | Any occurrence | None | Review unnecessary maintenance tasks to optimize resources and minimize disruptions. |
| Maintenance started | Any occurrence | None | Track the start of maintenance tasks to ensure planned activities proceed smoothly. Proper oversight helps maintain system performance and minimize downtime during maintenance. |
| Maintenance scheduled | Any occurrence | None | Monitor scheduled maintenance to prepare for potential system impacts. |
| [Steal](/alert-basics/#cpu-steal) | > 5% for 5 minutes | > 20% for 5 minutes | Monitor CPU steal on AWS EC2 clusters with Burstable Performance to identify when CPU usage exceeds the guaranteed baseline due to shared cores. High steal percentages indicate the CPU credit balance is depleted, affecting performance. |
| CPU | > 75% for 5 minutes | > 75% for 5 minutes | .. include:: /includes/cloud-docs/shared-metric-description-cpu.rst |
| Disk partition usage | > 90% | > 95% for 5 minutes | Monitor disk partition usage to ensure sufficient storage availability. High usage levels can lead to performance degradation and potential system outages. |

To learn more, see [Configure and Resolve Alerts](/alerts).

#### Monitor by Using Atlas Built-In Tools

Atlas provides several tools that allow you to proactively monitor and improve the performance of your database.

##### Real-Time Performance Panel

The Real-Time Performance Panel (RTPP) in the Atlas UI provides insights into current network traffic, database operations, and hardware statistics about the hosts at a one second granularity in the Atlas UI. We recommend that you use RTPP (Real-Time Performance Panel) to:

- Visually identify relevant database operations
- Evaluate query execution times
- Evaluate the ratio of documents scanned to documents returned
- Monitor network load and throughput
- Discover potential replication lag on secondary members of replica sets
- Kill operations before they have completed to free up valuable resources

The RTPP is not available to monitor from the Atlas Administration API, but you can enable and disable the RTPP from the Atlas Administration API with Update One Project Settings. To learn more, see real-time-metrics-status-tab.

##### Query Profiler

The Query Profiler identifies slow queries and bottlenecks, and suggests index refinement and query restructuring to improve the performance of your database. It provides visibility into the slowest operations over a 24-hour window in the Atlas UI, making it easier to identify trends and outliers in query efficiency. We recommend that you use this data to pinpoint and troubleshoot poorly performing queries, reducing performance overhead. You can return log lines for slow queries that the Query Profiler identifies from the Atlas Administration API with Return Slow Queries. To learn more, see query-profiler.

##### Performance Advisor

The Performance Advisor automatically analyzes logs for slow-running queries and recommends indexes to create and drop. It analyzes slow queries and provides index suggestions for individual collections, ranked by a calculated impact score, and tailored to your workload. This gives you an easy, instantaneous way to make high-impact performance improvements. We recommend that you monitor regularly, focus on slow queries, and enable the profiler selectively to minimize overhead. You can use the Atlas UI, Atlas CLI, and the Atlas Administration API to view slow queries and suggestions for improving the performance of your queries from the Performance Advisor. You can return log lines for slow queries that the Performance Advisor identifies from the Atlas Administration API with Return Slow Queries. To return suggested indexes and more with the Atlas Administration API, see Performance Advisor. To learn more, see performance-advisor.

##### Namespace Insights

The Namespace Insights page in the Atlas UI allows you to monitor collection-level performance and usage metrics. It displays metrics (such as the number of CRUD operations on the collection) and statistics (like average query execution time) for certain hosts and operation types for the collections that you [pin for monitoring](/namespace-insights/#manage-pinned-namespaces-from-the-dialog). This gives you more granular visibility into collection-level performance, which you can use to optimize database performance, resolve issues, and make decisions about scaling, indexing, and query tuning. To learn more, see namespace-insights.

##### Activity Feed

The Organization Activity Feed and Project Activity Feed in the Atlas UI list all events that occur for a given Atlas organization or project, respectively. You can filter each activity feed for event type and time range to monitor events such as API access updates, alert configuration changes, and more. This allows you to review activity records for your organization or project at your desired level of granularity. You use the Atlas UI, Atlas CLI, and Atlas Administration API to retrieve events from each activity feed. To learn more, see activity-feed.

#### Monitor by Using Logs

Atlas retains the last 30 days of log messages and system event audit messages. You can download Atlas logs at any point until the end of their retention periods by using the Atlas UI, Atlas Administration API, and Atlas CLI. To learn more, see mongodb-logs.

You can also [push logs to an AWS S3 bucket](/push-logs/). When you configure this feature, Atlas continually pushes logs from `mongod`, `mongos`, and audit logs to an AWS (Amazon Web Services) S3 bucket. Atlas exports logs every five minutes.

## Automation Examples: Atlas Monitoring and Logging

The following examples demonstrate how to enable monitoring using Atlas tools for automation.

**Dev and Test Environments**

---

**Staging and Prod Environments**

---

**Dev and Test Environments**

**CLI**

### View Cluster Metrics

Run the following command to retrieve the amount of used and free space on the specified disk. This metric can be used to determine if the system is running out of free space.

```
atlas metrics disks describe atlas-lnmtkm-shard-00-00.ajlj3.mongodb.net:27017 data \
  --granularity P1D \ 
  --period P1D \
  --type DISK_PARTITION_SPACE_FREE,DISK_PARTITION_SPACE_USED \
  --projectId 6698000acf48197e089e4085 \
```

### Configure Alerts

Run the following command to create an alert notification to an email address when your deployment doesn't have a primary.

```
atlas alerts settings create \
  --enabled \
  --event "NO_PRIMARY" \
  --matcherFieldName CLUSTER_NAME \
  --matcherOperator EQUALS \
  --matcherValue ftsTest \
  --notificationType EMAIL \
  --notificationEmailEnabled \
  --notificationEmailAddress "myName@example.com" \
  --notificationIntervalMin 5 \
  --projectId 6698000acf48197e089e4085
```

### Monitor Database Performance

Run the following command to enable Atlas-managed slow operation threshold for your project.

```
atlas performanceAdvisor slowOperationThreshold enable --projectId 56fd11f25f23b33ef4c2a331 
```

### Download Logs

Run the following command to download a compressed file that contains the MongoDB logs for the specified host in your project.

```
atlas logs download cluster0-shard-00-00.a1b2c.mongodb.net mongodb.gz
```

---

**Atlas Go SDK**

See all of the Atlas Architecture Center Go SDK examples in a single project in the [Atlas Architecture Go SDK GitHub repo](https://github.com/mongodb/atlas-architecture-go-sdk).

Before you can authenticate and run the example scripts using the Atlas Go SDK, you must:

- Create an Atlas service account. Store your client ID and secret as environment variables by running the following command in the terminal:

   ```shell
   export MONGODB_ATLAS_SERVICE_ACCOUNT_ID="<insert your client ID here>"
   export MONGODB_ATLAS_SERVICE_ACCOUNT_SECRET="<insert your client secret here>"
   ```

- Set the following configuration variables in your Go project:

   **configs/config.json**

   ```json
   {
     "MONGODB_ATLAS_BASE_URL": "https://cloud.mongodb.com",
     "ATLAS_ORG_ID": "32b6e34b3d91647abb20e7b8",
     "ATLAS_PROJECT_ID": "67212db237c5766221eb6ad9",
     "ATLAS_CLUSTER_NAME": "myCluster",
     "ATLAS_PROCESS_ID": "myCluster-shard-00-00.ajlj3.mongodb.net:27017"
   }
   ```

### View Cluster Metrics

The following example script demonstrates how to retrieve the amount of used and free space on the specified disk. This metric can be used to determine if the system is running out of free space.

**get-cluster-metrics/main.go**

```go
// See entire project at https://github.com/mongodb/atlas-architecture-go-sdk
package main

import (
	"atlas-sdk-go/internal/auth"
	"context"
	"encoding/json"
	"fmt"
	"go.mongodb.org/atlas-sdk/v20250219001/admin"
	"log"
)

// getDiskMetrics fetches metrics for a specified disk partition in a project and prints results to the console
func getDiskMetrics(ctx context.Context, atlasClient admin.APIClient, params *admin.GetDiskMeasurementsApiParams) (*admin.ApiMeasurementsGeneralViewAtlas, error) {

	resp, _, err := atlasClient.MonitoringAndLogsApi.GetDiskMeasurementsWithParams(ctx, params).Execute()
	if err != nil {
		if apiError, ok := admin.AsError(err); ok {
			return nil, fmt.Errorf("failed to get metrics for partition: %s (API error: %v)", err, apiError.GetDetail())
		}
		return nil, fmt.Errorf("failed to get metrics: %w", err)
	}
	if resp == nil || resp.HasMeasurements() == false {
		return nil, fmt.Errorf("no metrics found for partition %s in project %s", params.PartitionName, params.GroupId)
	}
	jsonData, err := json.MarshalIndent(resp, "", "  ")
	if err != nil {
		return nil, fmt.Errorf("failed to marshal response: %w", err)
	}
	fmt.Println(string(jsonData))
	return resp, nil
}

func main() {
	ctx := context.Background()

	// Create an Atlas client authenticated using OAuth2 with service account credentials
	atlasClient, _, config, err := auth.CreateAtlasClient()
	if err != nil {
		log.Fatalf("Failed to create Atlas client: %v", err)
	}

	// Fetch disk metrics using the following parameters:
	partitionName := "data"
	diskMetricsGranularity := admin.PtrString("P1D")
	diskMetricsPeriod := admin.PtrString("P1D")
	diskMetrics := []string{
		"DISK_PARTITION_SPACE_FREE", "DISK_PARTITION_SPACE_USED",
	}

	diskMeasurementsParams := &admin.GetDiskMeasurementsApiParams{
		GroupId:       config.ProjectID,
		ProcessId:     config.ProcessID,
		PartitionName: partitionName,
		M:             &diskMetrics,
		Granularity:   diskMetricsGranularity,
		Period:        diskMetricsPeriod,
	}
	_, err = getDiskMetrics(ctx, *atlasClient, diskMeasurementsParams)
	if err != nil {
		fmt.Printf("Error fetching disk metrics: %v", err)
	}
}
```

### Download Logs

The following example script demonstrates how to download and unzip a compressed file that contains the MongoDB logs for the specified host in your Atlas project:

**get-logs/main.go**

```go
// See entire project at https://github.com/mongodb/atlas-architecture-go-sdk
package main

import (
	"atlas-sdk-go/internal/auth"
	"compress/gzip"
	"context"
	"fmt"
	"io"
	"log"
	"os"
	"strings"

	"go.mongodb.org/atlas-sdk/v20250219001/admin"
)

func SafeClose(c io.Closer) {
	if c != nil {
		if err := c.Close(); err != nil {
			log.Printf("Warning: failed to close resource: %v", err)
		}
	}
}

// getHostLogs downloads a compressed .gz file that contains the MongoDB logs for
// the specified host in your project.
func getHostLogs(ctx context.Context, atlasClient admin.APIClient, params *admin.GetHostLogsApiParams) (string, error) {
	logFileName := fmt.Sprintf("logs_%s_%s.gz", params.GroupId, params.HostName)
	fmt.Printf("Fetching %s log for host %s in project %s\n", params.LogName, params.HostName, params.GroupId)

	if err := downloadLogs(ctx, atlasClient, params, logFileName); err != nil {
		return "", err
	}

	fmt.Printf("Logs saved to %s\n", logFileName)
	return logFileName, nil
}

func downloadLogs(ctx context.Context, atlasClient admin.APIClient, params *admin.GetHostLogsApiParams, filePath string) error {
	resp, _, err := atlasClient.MonitoringAndLogsApi.GetHostLogsWithParams(ctx, params).Execute()
	if err != nil {
		return fmt.Errorf("fetch logs: %w", err)
	}
	defer SafeClose(resp)

	file, err := os.Create(filePath)
	if err != nil {
		return fmt.Errorf("create %q: %w", filePath, err)
	}
	defer SafeClose(file)

	if _, err := io.Copy(file, resp); err != nil {
		return fmt.Errorf("write to %q: %w", filePath, err)
	}

	return nil
}

func unzipGzFile(srcPath, destPath string) error {
	srcFile, err := os.Open(srcPath)
	if err != nil {
		return fmt.Errorf("open gz file: %w", err)
	}
	defer SafeClose(srcFile)

	gzReader, err := gzip.NewReader(srcFile)
	if err != nil {
		return fmt.Errorf("create gzip reader: %w", err)
	}
	defer SafeClose(gzReader)

	destFile, err := os.Create(destPath)
	if err != nil {
		return fmt.Errorf("create destination file: %w", err)
	}
	defer SafeClose(destFile)

	if _, err := io.Copy(destFile, gzReader); err != nil {
		return fmt.Errorf("unzip copy error: %w", err)
	}

	fmt.Printf("Unzipped logs to %s\n", destPath)
	return nil
}

func main() {
	ctx := context.Background()

	// Create an Atlas client authenticated using OAuth2 with service account credentials
	client, _, config, err := auth.CreateAtlasClient()
	if err != nil {
		log.Fatalf("Failed to create Atlas client: %v", err)
	}

	params := &admin.GetHostLogsApiParams{
		GroupId:  config.ProjectID,
		HostName: config.HostName,
		LogName:  "mongodb",       // The type of log to get ("mongodb" or "mongos")
	}

	logFileName, err := getHostLogs(ctx, *client, params)
	if err != nil {
		log.Fatalf("Failed to download logs: %v", err)
	}

	plainTextLog := strings.TrimSuffix(logFileName, ".gz") + ".log"
	if err := unzipGzFile(logFileName, plainTextLog); err != nil {
		log.Fatalf("Failed to unzip log file: %v", err)
	}

}
```

---

**Terraform**

> **Tip:**
> For Terraform examples that enforce our recommendations across all pillars, see one of the following examples in GitHub:
>
> - [Staging/Prod](https://github.com/mongodb/docs/blob/main/content/atlas-architecture/current/source/includes/examples/tf-staging-prod-complete/)
> - [Dev/Test](https://github.com//mongodb/docs/blob/main/content/atlas-architecture/current/source/includes/examples/tf-dev-test-complete/)

Before you can create resources with Terraform, you must:

- Create your paying organization and create an API key for the paying organization. Store your API (Application Programming Interface) key as environment variables by running the following command in the terminal:

   ```shell
   export MONGODB_ATLAS_PUBLIC_KEY="<insert your public key here>"
   export MONGODB_ATLAS_PRIVATE_KEY="<insert your private key here>"
   ```

- [Install Terraform](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)

We also recommend [creating a workspace for your environment](https://developer.hashicorp.com/terraform/cloud-docs/recommended-practices/part1#one-workspace-per-environment-per-terraform-configuration).

### Configure Alerts

The following examples demonstrate how to configure alerts and alert notifications. You must create the following files for each example. Place the files for each example in their own directory. Change the IDs and names to use your values:

#### variables.tf

```
variable "atlas_org_id" {
  type        = string
  description = "MongoDB Atlas Organization ID"
}
variable "atlas_project_name" {
  type        = string
  description = "The MongoDB Atlas Project Name"
}
variable "atlas_project_id" {
  description = "MongoDB Atlas project id"
  type        = string
}
variable "atlas_cluster_name" {
  description = "MongoDB Atlas Cluster Name"
  default     = "datadog-test-cluster"
  type        = string
}
```

#### terraform.tfvars

```
atlas_org_id = "32b6e34b3d91647abb20e7b8"
atlas_project_name = "Customer Portal - Prod"
atlas_project_id = "67212db237c5766221eb6ad9"
atlas_cluster_name = "myCluster"
```

**Example:** Use the following to send an alert notification by email to users with the `GROUP_CLUSTER_MANAGER` role when there is a replication lag, which could result in data inconsistencies.

#### main.tf

```
resource "mongodbatlas_alert_configuration" "test" {
  project_id = var.atlas_project_id
  event_type = "REPLICATION_OPLOG_WINDOW_RUNNING_OUT"
  enabled    = true

  notification {
    type_name     = "GROUP"
    interval_min  = 10
    delay_min     = 0
    sms_enabled   = false
    email_enabled = true
    roles         = ["GROUP_CLUSTER_MANAGER"]
  }

  matcher {
    field_name = "CLUSTER_NAME"
    operator   = "EQUALS"
    value      = "myCluster"
  }

  threshold_config {
    operator    = "LESS_THAN"
    threshold   = 1
    units       = "HOURS"
  }
}
```

---

---

**Staging and Prod Environments**

**CLI**

### View Cluster Metrics

Run the sample command to retrieve the following metrics:

- OPCOUNTERS - Monitor the amount of queries, updates, inserts, and deletes occurring at peak load and ensure that load doesn't increase unexpectedly.
- CONNECTIONS - Ensure that the number of sockets used for heartbeats and replication between members isn't above the set limit.
- QUERY TARGETING - Ensure the number of keys and documents scanned to the number of documents returned, averaged by second, aren't too high.
- SYSTEM CPU - Ensure that the CPU usage is steady.
- GLOBAL LOCK QUEUE - Monitor the number of read and write operations that are currently queued and waiting for the read and write lock, and ensure that load doesn't increase unexpectedly.

```
atlas metrics processes atlas-lnmtkm-shard-00-00.ajlj3.mongodb.net:27017 \ 
  --projectId 56fd11f25f23b33ef4c2a331 \ 
  --granularity PT1H \ 
  --period P7D \ 
  --type
  OPCOUNTER_DELETE,OPCOUNTER_INSERT,OPCOUNTER_QUERY,OPCOUNTER_UPDATE,CONNECTIONS,QUERY_TARGETING_SCANNED_OBJECTS_PER_RETURNED,QUERY_TARGETING_SCANNED_PER_RETURNED,SYSTEM_CPU_GUEST,SYSTEM_CPU_IOWAIT,SYSTEM_CPU_IRQ,SYSTEM_CPU_KERNEL,SYSTEM_CPU_NICE,SYSTEM_CPU_SOFTIRQ,SYSTEM_CPU_STEAL,SYSTEM_CPU_USER,GLOBAL_LOCK_CURRENT_QUEUE_TOTAL,GLOBAL_LOCK_CURRENT_QUEUE_READERS,GLOBAL_LOCK_CURRENT_QUEUE_WRITERS \
  --output json
```

### Configure Alerts

Run the following command to send alerts to a group by email when there are possible connection storms based on the number of connections in your project.

```
atlas alerts settings create \ 
  --enabled \
  --event "OUTSIDE_METRIC_THRESHOLD" \ 
  --metricName CONNECTIONS \ 
  --metricOperator LESS_THAN \ 
  --metricThreshold 1 \ 
  --metricUnits RAW \ 
  --notificationType GROUP \ 
  --notificationRole "GROUP_DATA_ACCESS_READ_ONLY","GROUP_CLUSTER_MANAGER","GROUP_DATA_ACCESS_ADMIN" \
  --notificationEmailEnabled \ 
  --notificationEmailAddress "user@example.com" \
  --notificationIntervalMin 5 \ 
  --projectId 6698000acf48197e089e4085 
```

### Monitor Database Performance

Run the following command to retrieve the suggested indexes for collections experiencing slow queries.

```
atlas performanceAdvisor suggestedIndexes list \
  --projectId 56fd11f25f23b33ef4c2a331 \ 
  --processName atlas-zqva9t-shard-00-02.2rnul.mongodb.net:27017
```

### Download Logs

Run the following command to download a compressed file that contains the MongoDB logs for the specified host in your project.

```
atlas logs download cluster0-shard-00-00.a1b2c.mongodb.net mongodb.gz
```

---

**Atlas Go SDK**

See all of the Atlas Architecture Center Go SDK examples in a single project in the [Atlas Architecture Go SDK GitHub repo](https://github.com/mongodb/atlas-architecture-go-sdk).

Before you can authenticate and run the example scripts using the Atlas Go SDK, you must:

- Create an Atlas service account. Store your client ID and secret as environment variables by running the following command in the terminal:

   ```shell
   export MONGODB_ATLAS_SERVICE_ACCOUNT_ID="<insert your client ID here>"
   export MONGODB_ATLAS_SERVICE_ACCOUNT_SECRET="<insert your client secret here>"
   ```

- Set the following configuration variables in your Go project:

   **configs/config.json**

   ```json
   {
     "MONGODB_ATLAS_BASE_URL": "https://cloud.mongodb.com",
     "ATLAS_ORG_ID": "32b6e34b3d91647abb20e7b8",
     "ATLAS_PROJECT_ID": "67212db237c5766221eb6ad9",
     "ATLAS_CLUSTER_NAME": "myCluster",
     "ATLAS_PROCESS_ID": "myCluster-shard-00-00.ajlj3.mongodb.net:27017"
   }
   ```

For more information on authenticating and creating a client, see the complete Atlas SDK for Go example project [in GitHub](https://github.com/mongodb/atlas-architecture-go-sdk).

### View Cluster Metrics

The following example script demonstrates how to retrieve the following metrics:

- OPCOUNTERS - Monitor the amount of queries, updates, inserts, and deletes occurring at peak load and ensure that load doesn't increase unexpectedly.
- CONNECTIONS - Ensure that the number of sockets used for heartbeats and replication between members isn't above the set limit.
- QUERY TARGETING - Ensure the number of keys and documents scanned to the number of documents returned, averaged by second, aren't too high.
- SYSTEM CPU - Ensure that the CPU usage is steady.
- GLOBAL LOCK QUEUE - Monitor the number of read and write operations that are currently queued and waiting for the read and write lock, and ensure that load doesn't increase unexpectedly.

**get-disk-metrics/main.go**

```go
// See entire project at https://github.com/mongodb/atlas-architecture-go-sdk
package main

import (
	"atlas-sdk-go/internal/auth"
	"context"
	"encoding/json"
	"fmt"
	"log"

	"go.mongodb.org/atlas-sdk/v20250219001/admin"
)

// getProcessMetrics fetches metrics for a specified host process in a project and prints results to the console
func getProcessMetrics(ctx context.Context, atlasClient admin.APIClient, params *admin.GetHostMeasurementsApiParams) (*admin.ApiMeasurementsGeneralViewAtlas, error) {
	fmt.Printf("Fetching metrics for host process %s in project %s", params.ProcessId, params.GroupId)

	resp, _, err := atlasClient.MonitoringAndLogsApi.GetHostMeasurementsWithParams(ctx, params).Execute()
	if err != nil {
		if apiError, ok := admin.AsError(err); ok {
			return nil, fmt.Errorf("failed to get metrics for process in host: %s (API error: %v)", err, apiError.GetDetail())
		}
		return nil, fmt.Errorf("failed to get metrics: %w", err)
	}

	if resp == nil || resp.HasMeasurements() == false {
		return nil, fmt.Errorf("no metrics found for host process %s in project %s", params.ProcessId, params.GroupId)
	}
	jsonData, err := json.MarshalIndent(resp, "", "  ")
	if err != nil {
		return nil, fmt.Errorf("failed to marshal response: %w", err)
	}
	fmt.Println(string(jsonData))
	return resp, nil
}

func main() {
	ctx := context.Background()

	// Create an Atlas client authenticated using OAuth2 with service account credentials
	atlasClient, _, config, err := auth.CreateAtlasClient()
	if err != nil {
		log.Fatalf("Failed to create Atlas client: %v", err)
	}

	// Fetch process metrics using the following parameters:
	processMetricGranularity := admin.PtrString("PT1H")
	processMetricPeriod := admin.PtrString("P7D")
	processMetrics := []string{
		"OPCOUNTER_DELETE", "OPCOUNTER_INSERT", "OPCOUNTER_QUERY", "OPCOUNTER_UPDATE",
		"CONNECTIONS", "QUERY_TARGETING_SCANNED_OBJECTS_PER_RETURNED",
		"QUERY_TARGETING_SCANNED_PER_RETURNED", "SYSTEM_CPU_GUEST", "SYSTEM_CPU_IOWAIT",
		"SYSTEM_CPU_IRQ", "SYSTEM_CPU_KERNEL", "SYSTEM_CPU_NICE", "SYSTEM_CPU_SOFTIRQ",
		"SYSTEM_CPU_STEAL", "SYSTEM_CPU_USER", "GLOBAL_LOCK_CURRENT_QUEUE_TOTAL",
		"GLOBAL_LOCK_CURRENT_QUEUE_READERS", "GLOBAL_LOCK_CURRENT_QUEUE_WRITERS",
	}
	hostMeasurementsParams := &admin.GetHostMeasurementsApiParams{
		GroupId:     config.ProjectID,
		ProcessId:   config.ProcessID,
		M:           &processMetrics,
		Granularity: processMetricGranularity,
		Period:      processMetricPeriod,
	}
	_, err = getProcessMetrics(ctx, *atlasClient, hostMeasurementsParams)
	if err != nil {
		fmt.Printf("Error fetching host process metrics: %v", err)
	}
}
```

### Download Logs

The following example script demonstrates how to download and unzip a compressed file that contains the MongoDB logs for the specified host in your Atlas project:

**get-logs/main.go**

```go
// See entire project at https://github.com/mongodb/atlas-architecture-go-sdk
package main

import (
	"atlas-sdk-go/internal/auth"
	"compress/gzip"
	"context"
	"fmt"
	"io"
	"log"
	"os"
	"strings"

	"go.mongodb.org/atlas-sdk/v20250219001/admin"
)

func SafeClose(c io.Closer) {
	if c != nil {
		if err := c.Close(); err != nil {
			log.Printf("Warning: failed to close resource: %v", err)
		}
	}
}

// getHostLogs downloads a compressed .gz file that contains the MongoDB logs for
// the specified host in your project.
func getHostLogs(ctx context.Context, atlasClient admin.APIClient, params *admin.GetHostLogsApiParams) (string, error) {
	logFileName := fmt.Sprintf("logs_%s_%s.gz", params.GroupId, params.HostName)
	fmt.Printf("Fetching %s log for host %s in project %s\n", params.LogName, params.HostName, params.GroupId)

	if err := downloadLogs(ctx, atlasClient, params, logFileName); err != nil {
		return "", err
	}

	fmt.Printf("Logs saved to %s\n", logFileName)
	return logFileName, nil
}

func downloadLogs(ctx context.Context, atlasClient admin.APIClient, params *admin.GetHostLogsApiParams, filePath string) error {
	resp, _, err := atlasClient.MonitoringAndLogsApi.GetHostLogsWithParams(ctx, params).Execute()
	if err != nil {
		return fmt.Errorf("fetch logs: %w", err)
	}
	defer SafeClose(resp)

	file, err := os.Create(filePath)
	if err != nil {
		return fmt.Errorf("create %q: %w", filePath, err)
	}
	defer SafeClose(file)

	if _, err := io.Copy(file, resp); err != nil {
		return fmt.Errorf("write to %q: %w", filePath, err)
	}

	return nil
}

func unzipGzFile(srcPath, destPath string) error {
	srcFile, err := os.Open(srcPath)
	if err != nil {
		return fmt.Errorf("open gz file: %w", err)
	}
	defer SafeClose(srcFile)

	gzReader, err := gzip.NewReader(srcFile)
	if err != nil {
		return fmt.Errorf("create gzip reader: %w", err)
	}
	defer SafeClose(gzReader)

	destFile, err := os.Create(destPath)
	if err != nil {
		return fmt.Errorf("create destination file: %w", err)
	}
	defer SafeClose(destFile)

	if _, err := io.Copy(destFile, gzReader); err != nil {
		return fmt.Errorf("unzip copy error: %w", err)
	}

	fmt.Printf("Unzipped logs to %s\n", destPath)
	return nil
}

func main() {
	ctx := context.Background()

	// Create an Atlas client authenticated using OAuth2 with service account credentials
	client, _, config, err := auth.CreateAtlasClient()
	if err != nil {
		log.Fatalf("Failed to create Atlas client: %v", err)
	}

	params := &admin.GetHostLogsApiParams{
		GroupId:  config.ProjectID,
		HostName: config.HostName,
		LogName:  "mongodb",       // The type of log to get ("mongodb" or "mongos")
	}

	logFileName, err := getHostLogs(ctx, *client, params)
	if err != nil {
		log.Fatalf("Failed to download logs: %v", err)
	}

	plainTextLog := strings.TrimSuffix(logFileName, ".gz") + ".log"
	if err := unzipGzFile(logFileName, plainTextLog); err != nil {
		log.Fatalf("Failed to unzip log file: %v", err)
	}

}
```

---

**Terraform**

> **Tip:**
> For Terraform examples that enforce our recommendations across all pillars, see one of the following examples in GitHub:
>
> - [Staging/Prod](https://github.com/mongodb/docs/blob/main/content/atlas-architecture/current/source/includes/examples/tf-staging-prod-complete/)
> - [Dev/Test](https://github.com//mongodb/docs/blob/main/content/atlas-architecture/current/source/includes/examples/tf-dev-test-complete/)

Before you can create resources with Terraform, you must:

- Create your paying organization and create an API key for the paying organization. Store your API (Application Programming Interface) key as environment variables by running the following command in the terminal:

   ```shell
   export MONGODB_ATLAS_PUBLIC_KEY="<insert your public key here>"
   export MONGODB_ATLAS_PRIVATE_KEY="<insert your private key here>"
   ```

- [Install Terraform](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)

We also recommend [creating a workspace for your environment](https://developer.hashicorp.com/terraform/cloud-docs/recommended-practices/part1#one-workspace-per-environment-per-terraform-configuration).

### Configure Alerts

The following examples demonstrate how to configure alerts and alert notifications. You must create the following files for each example. Place these files for each example in their own directory and replace only the `main.tf` file. Change the IDs and names to use your values:

#### variables.tf

```
variable "atlas_org_id" {
  type        = string
  description = "MongoDB Atlas Organization ID"
}
variable "atlas_project_name" {
  type        = string
  description = "The MongoDB Atlas Project Name"
}
variable "atlas_project_id" {
  description = "MongoDB Atlas project id"
  type        = string
}
variable "atlas_cluster_name" {
  description = "MongoDB Atlas Cluster Name"
  default     = "datadog-test-cluster"
  type        = string
}
variable "datadog_api_key" {
  description = "Datadog api key"
 type        = string
}
variable "datadog_region" {
  description = "Datadog region"
  default     = "US5"
  type        = string
}
variable "prometheus_user_name" {
  type        = string
  description = "The Prometheus User Name"
  default     = "puser"
}
variable "prometheus_password" {
  type        = string
  description = "The Prometheus Password"
 default     = "ppassword"
}
```

#### terraform.tfvars

```
atlas_org_id = "32b6e34b3d91647abb20e7b8"
atlas_project_name = "Customer Portal - Prod"
atlas_project_id = "67212db237c5766221eb6ad9"
atlas_cluster_name = "myCluster"
datadog_api_key = "1234567890abcdef1234567890abcdef"
datadog_region = "US5"
prometheus_user_name = "prometheus_user"
prometheus_password = "secure_prometheus_password"
```

**Example 1:** Use the following to integrate with third-party services like Datadog and Prometheus for alert notification.

#### main.tf

```shell
resource "mongodbatlas_third_party_integration" "test_datadog" {
  project_id = var.atlas_project_id
  type = "DATADOG"
  api_key = var.datadog_api_key
  region = var.datadog_region
}

resource "mongodbatlas_third_party_integration" "test_prometheus" {
  project_id        = var.atlas_project_id
  type              = "PROMETHEUS"
  user_name         = var.prometheus_user_name
  password          = var.prometheus_password
  service_discovery = "http"
  enabled           = true
}

output "datadog.id" { value = mongodbatlas_third_party_integration.test_datadog.id }  
output "prometheus.id" { value = mongodbatlas_third_party_integration.test_prometheus.id }
```

**Example 2:** Use the following to send alert notification to third-party services like Datadog and Prometheus when there is no primary on the replica set for more than 5 minutes.

#### main.tf

```
resource "mongodbatlas_alert_configuration" "test_alert_notification" {
  project_id = var.atlas_project_id
  event_type = "NO_PRIMARY"
  enabled    = true

  notification {
    type_name     = "PROMETHEUS"
    integration_id = mongodbatlas_third_party_integration.test_datadog.id # ID of the Atlas Prometheus integration
  }

  notification {
    type_name     = "DATADOG"
    integration_id = mongodbatlas_third_party_integration.test_prometheus.id # ID of the Atlas Datadog integration
  }

  matcher {
    field_name = "REPLICA_SET_NAME"
    operator   = "EQUALS"
   value      = "myReplSet"
  }

  threshold_config {
    operator    = "GREATER_THAN"
    threshold   = 5
    units       = "MINUTES"
  }
}
```

**Example 3:** Use the following to send an alert notification by email to users with the `GROUP_CLUSTER_MANAGER` role when there is a replication lag, which could result in data inconsistencies.

#### main.tf

```
resource "mongodbatlas_alert_configuration" "test_replication_lag_alert" {
  project_id = var.atlas_project_id
  event_type = "OUTSIDE_METRIC_THRESHOLD"
  enabled    = true

  notification {
    type_name     = "GROUP"
    interval_min  = 10
    delay_min     = 0
    sms_enabled   = false
    email_enabled = true
    roles         = ["GROUP_CLUSTER_MANAGER"]
  }

  matcher {
    field_name = "CLUSTER_NAME"
    operator   = "EQUALS"
    value      = "myCluster"
  }

  metric_threshold_config {
    metric_name = "OPLOG_SLAVE_LAG_MASTER_TIME"
    operator    = "GREATER_THAN"
    threshold   = 1
    units       = "HOURS"
  }
}
```

---

---
