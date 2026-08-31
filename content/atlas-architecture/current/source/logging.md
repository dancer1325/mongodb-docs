# Guidance for Atlas Logging

.. meta: :description: Learn about logging in MongoDB Atlas.

To review Atlas platform activities, use logs.

## Features for Atlas Logging
### Accessing Audit Logs

You can use the Atlas CLI, Atlas Administration API, or Atlas UI for the following auditing activities:

- View and download audit logs to track system event actions for deployments with multiple users. Atlas administrators can configure a custom auditing filter to choose the actions, database users, Atlas roles, and LDAP (Lightweight Directory Access Protocol) groups that they want to audit.
- View and download MongoDB logs to track log events for your deployment, including incoming connections, commands run, and issues encountered. Generally, log messages are useful for diagnosing issues, monitoring your deployment, and tuning performance.
- View project and organization events in the Project Activity Feed and Organization Activity Feed. These activity feeds list all events at the organization or project level, including changes related to Atlas access, alert configurations and monitoring, billing, and more.
- View database authentication attempts that users make against your cluster in your access logs (i.e. Database access history in the Atlas UI). Atlas logs both successful and unsuccessful authentication attempts, including the timestamp of each attempt and which user tried to authenticate.

### Programmatic Access to Audit Logs

To integrate with tools beyond the built-in integrations, we recommend that you retrieve logs with the following programmatic tools and feed the JSON (Javascript Object Notation)-formatted output to your external tools:

- To continually push logs to an AWS (Amazon Web Services) S3 (Simple Storage Service) bucket, use the Atlas Administration API endpoints for Push-Based Log Export.
- To retrieve deployment logs and lists of project events, use the Atlas Administration API endpoints for Monitoring and Logs and Project and Organization Events.
- To retrieve deployment logs, use the atlas deployment logs command in the Atlas CLI.

## Recommendations for Atlas Logging

The following recommendations apply to all deployment paradigms. To perform a full audit, you can use a combination of [audit logs](/reference/audit-message/), [MongoDB log messages](/reference/log-messages/), and the project and organization activity feed. By default, audit log messages are returned in a format designed by MongoDB, called the [mongo schema](/reference/audit-message/mongo/). Audit log messages that follow the `mongo` schema always include the following information:

- Action type (`atype`)
- Timestamp
- Client connection ID (UUID)
- Client IP address and port number
- Incoming connection IP address and port number
- Username(s)
- User authentication database(s)
- User role(s)
- User role database(s)
- `param` document containing specific details for the event
- Result value or error code

For a full list of audit action types and their associated `param` details and `result` values, see [mongo Schema Audit Messages](/reference/audit-message/mongo/).

## Automation Examples: Atlas Logging

> **Tip:**
> For Terraform examples that enforce our recommendations across all pillars, see one of the following examples in GitHub:
>
> - [Staging/Prod](https://github.com/mongodb/docs/blob/main/content/atlas-architecture/current/source/includes/examples/tf-staging-prod-complete/)
> - [Dev/Test](https://github.com//mongodb/docs/blob/main/content/atlas-architecture/current/source/includes/examples/tf-dev-test-complete/)

The following examples show how to retrieve and download logs and configure auditing using Atlas tools for automation. In addition to the following examples, see the blogpost [Streamlining Log Management to Amazon S3 Using Atlas Push-based Log Exports With HashiCorp Terraform](https://www.mongodb.com/developer/products/atlas/streamlining-log-management-amazon-s3-atlas-push-based-log-exports-hashicorp-terraform/).

**CLI**

You can retrieve logs and download logs. You can also retrieve alerts.

- Retrieving logs allows you to get logs for real-time viewing and accessing of current log files.
- Downloading logs lets you create a log archive from MongoDB Atlas for later analysis or storage.

### Retrieve Logs

Retrieve logs to monitor and examine your cluster's activity in real-time and to  troubleshoot an issue in current logs. Each `mongod` and `mongos` instance in a cluster outputs its own [MongoDB log](/reference/log-messages/) and audit log messages with potentially different contents than other instances. You can view these log messages in the Atlas CLI using the atlas deployment logs command. To retrieve audit log entries for a `mongod` instance in your cluster, provide the `mongod` hostname and specify `mongodb-audit-log.gz` as the name of the audit log file:

```
atlas deployments logs --output json --type atlas --hostname cluster0-shard-00-00.a1b2c.mongodb.net --name mongodb-audit-log.gz
```

To retrieve audit log entries for a `mongos` instance in a sharded cluster, provide the `mongos` hostname and specify `mongos-audit-log.gz` as the name of the audit log file:

```
atlas deployments logs --output json --type atlas --hostname cluster0-shard-00-00.a1b2c.mongodb.net --name mongos-audit-log.gz   
```

To retrieve [MongoDB log messages](/reference/log-messages/), provide the hostname of your `mongod` or `mongos` instance, and specify the name of the log file as `mongodb.gz` or `mongos.gz`, respectively:

```
atlas deployments logs --output json --type atlas --hostname cluster0-shard-00-00.a1b2c.mongodb.net --name mongodb.gz  
```

You can also use the atlas accessLogs list command to view the access log for a node or cluster. The access log is a JSON (Javascript Object Notation)-formatted list of all authentication requests against your specified node or cluster. To retrieve the access log, run the atlas accessLogs list command and specify the hostname or cluster name of the target node or cluster:

```
atlas accessLogs list --output json --clusterName Cluster0 
```

### Download Logs

Download logs to focus on historical logs and analyze them for audits or performance, or to archive them. Each `mongod` and `mongos` instance in a cluster has its own [MongoDB log](/reference/log-messages/) and audit log with potentially different contents than other instances. Even though, from the perspective of the application, a `mongod` or a `mongos` instance behaves identically to any other MongoDB instance, their logs contain information specific to their roles in MongoDB.

- The logs for `mongod` contain data requests, data access information, and background management operations for MongoDB.
- The logs for `mongos` contain information about routing queries and write operations to the shards in a sharded cluster.

You can download each log as a compressed file using the atlas logs download Atlas CLI command. To download the audit log for a `mongod` instance in your cluster, provide the `mongod` hostname and the audit log file name `mongodb-auditlog.gz` as arguments. The name of this file is used here as an example and you can use another name.

```
atlas logs download cluster0-shard-00-00.a1b2c.mongodb.net mongodb-audit-log.gz
```

To download the audit log for a `mongos` instance in a sharded cluster deployment, provide the `mongos` hostname and the audit log file name `mongos-auditlog.gz` as arguments. The name of this file is used here as an example and you can use another name.

```
atlas logs download cluster0-shard-00-00.a1b2c.mongodb.net mongos-audit-log.gz
```

To download the [MongoDB log](/reference/log-messages/) for a `mongod` or `mongos` instance, provide as arguments the hostname of the instance and the log file names `mongodb.gz` or `mongos.gz`, respectively:

```
atlas logs download cluster0-shard-00-00.a1b2c.mongodb.net mongodb.gz
```

### Retrieve All Project Alerts

You can use the following Atlas CLI commands to return alerts triggered by events for your project or organization. Atlas provides alerts such as Replica set has no primary and User joined the project by default. These events provide a record of significant activities and changes within the project or organization, including significant database, billing, or security activities or status changes. To customize which events trigger alerts for your project and organization, see configure-alerts.

### Retrieve All Log Events for Your Project or Organization

You can use the following Atlas CLI commands to return project or organization events from your Project Activity Feed or Organization Activity Feed. To return all events for your organization, use the atlas events organizations list command and specify your organization ID. The following command returns a JSON-formatted list of events for the organization with the ID `5dd5a6b6f10fab1d71a58495`:

```
atlas events organizations list --orgId 5dd5a6b6f10fab1d71a58495 --output json
```

To return all events for your project, use the atlas events projects list command and specify your project ID. The following command returns a JSON-formatted list of events for the project with the ID `64ac57bfe9810c0263e9d655`:

```
atlas events organizations list --orgId 5dd5a6b6f10fab1d71a58495 --output json
```

### Download All Query Logs for an Entire Cluster

To download query logs for an entire cluster, obtain the required permissions, select your cluster in the Atlas UI, click Online Archive, and run:

```
<cluster-name>_cluster_archive_<start-date>_<end-date>_queries.log.gz
```

---

**Terraform**

### Retrieve Logs

You can't retrieve logs with Terraform. Instead, use the following Atlas Administration API endpoints:

- Use Access Tracking Admin API to return access logs for all authentication attempts for the database, identified by the cluster's name or hostname.
- Use Monitoring and Logs APIs to retrieve a compressed log file with log messages for the specified host.
---
