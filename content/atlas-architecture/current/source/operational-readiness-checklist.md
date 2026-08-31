# MongoDB Atlas Operational Readiness Checklist

This checklist is designed to help you prepare your environment and team for a successful deployment and operation of MongoDB Atlas. Use this checklist to track your progress. For example, you can print it and check off each item as you complete the tasks. Consult the official MongoDB Atlas documentation for detailed guidance on each of these aspects.

## Account and Organization Setup

| Check | Action |
| --- | --- |
| square-o | Create a MongoDB Atlas account, set up your Atlas organizations according to your internal structure, and configure a root user with appropriate access. To get recommendations and learn more about this topic, see arch-center-hierarchy. |
| square-o | Set up projects based on your environment and application needs. Isolate environments by setting up production and non-production projects, at a minimum. To get recommendations and learn more about this topic, see arch-center-orgs-projects-clusters-recs. |
| square-o | Consider cross-organization billing, if applicable. To get recommendations and learn more about this topic, see arch-center-billing-data. |

## Network and Security Configuration

| Check | Action |
| --- | --- |
| square-o | Select cloud providers and regions for your Atlas clusters. Consider data sovereignty requirements and latency. To get recommendations and learn more about this topic, see arch-center-paradigms. |
| square-o | Configure network security based on your organization's needs. To get recommendations and learn more about this topic, see arch-center-auth. |
| square-o | Choose a network connectivity method. As a general recommendation, we advise that you set up and use Private Endpoints: - AWS PrivateLink - Azure Private Link, or - Google Cloud Private Service Connect Private Endpoints allow a one-way private connection from your VPC to Atlas. For multi-region clusters, enable private endpoints in each region. To learn more, see Recommendations for Multi-Region Deployments. In addition, you can choose one of the following network connectivity methods: - VPC Peering to set up private secure traffic routing within your network boundaries. - Public IP Access Lists to restrict inbound connections to specific IP addresses or CIDR (Classless Inter-Domain Routing) blocks. As an alternative to endpoints, consider IP Allow lists, if necessary. |
| square-o | TLS is mandatory and enabled by default. You can't disable it. TLS 1.2+ is the default, which ensures support for the next version once Atlas supports it. Review your TLS configuration to ensure that it's set in a way that meets your internal standards. To learn more, see arch-center-tls. |
| square-o | Configure authentication and authorization. To get recommendations and learn more about this topic, see arch-center-auth. - For database access in cloud environments, consider Workforce and Workload Identity Federation, such as OIDC, OAuth 2.0, AWS IAM roles, or Azure Managed Identities, for passwordless access. |
| square-o | Implement robust encryption. To learn more, see arch-center-data-encryption. - Encryption at rest is enabled by default using cloud providers' transparent disk encryption (AES-256). - We strongly recommend that you enable "Bring Your Own Key (BYOK)" encryption using Key Management Service (KMS) providers (AWS KMS, Azure Key Vault, or GCP KMS). Atlas can't rotate customer-managed encryption keys. - Consider Client-Side Field Level Encryption (CSFLE) to encrypt data within your application before transmitting it to Atlas. - Explore [Queryable Encryption](/core/queryable-encryption/) for applications that run queries on encrypted data. |
| square-o | Configure database auditing to track database access and actions. Create custom filters if needed. To get recommendations and learn more about this topic, see arch-center-auditing-logging. |
| square-o | Atlas regularly rotates certificates to remain in compliance with security standards and various authorities. - Ensure that you are not pinning a low-level certificate. - Be aware of hard-coded certificate authority certificates. - Ensure that you set up your applications in a way that lets you handle potential CA certificate updates. Atlas clusters use TLS certificates signed by a widely trusted Certificate Authority (CA). While applications using recent MongoDB drivers handle certificate validation automatically, older applications or those with custom TLS configurations might require updates to trust the new CA certificates, if MongoDB updates its certificate provider. To learn more, see [Hard-coded certificate authority](/reference/faq/security/#hard-coded-certificate-authority). |
| square-o | Understand and plan for compliance with relevant standards and regulations, such as ISO/IEC 27001, HIPAA, GDPR, PCI DSS, FedRAMP, and others. To learn more, see arch-center-compliance-atlas-gov. |

## Backup and Restore Strategy

| Check | Action |
| --- | --- |
| square-o | Enable Atlas Cloud Backup, which provides localized backup storage using the cloud provider's native snapshot functionality. To get recommendations and learn more about this topic, see arch-center-backups. |
| square-o | Enable Continuous Cloud Backup with a restore window that meets your your Recovery Point Objective (RPO). We recommend having a restore window of 7 days to allow for Point In Time (PIT) recovery using the oplog. |
| square-o | Define a backup schedule and retention policy that aligns with your business continuity and compliance requirements. Consider hourly, daily, weekly, and monthly snapshots with appropriate retention periods. |
| square-o | Consider multi-region snapshot distribution for increased resilience by copying snapshots to other geographic regions. |
| square-o | Enable Backup Compliance Policy to prevent unauthorized modifications or deletions of backups and comply with strict data protection requirements. |
| square-o | Understand the process for restoring from scheduled or on-demand snapshots. To learn more, see arch-center-backups-recs. |
| square-o | Learn about the process for restoring from Continuous Cloud Backup to a specific point in time. To learn more, see arch-center-backups-recs. |
| square-o | Plan and test your Disaster Recovery (DR) strategy. Understand Recovery Time Objective (RTO) and Recovery Point Objective (RPO). Consider testing application's resilience in Atlas. To learn more, see arch-center-dr. |
| square-o | Consider options for downloading and archiving snapshots, if required, using the Atlas UI, Atlas Administration API, or Atlas CLI. To learn more, see arch-center-backups. |

## Maintenance and Patching

| Check | Action |
| --- | --- |
| square-o | Be aware that Atlas deploys major version upgrades in a rolling manner to minimize downtime. This means that Atlas upgrades the secondary cluster nodes and fails over the primary cluster nodes to a newer version. |
| square-o | Define a Maintenance Window for Atlas automated systems to apply automatic minor version updates. Configure the day and hour of allowed maintenance using the `mongodbatlas_maintenance_window` resource. To learn more, see arch-center-high-availability. |
| square-o | Understand that Atlas has non-deferrable maintenance hours for critical security patches or operational necessities. Configure Protected Hours for your project and [define a daily window](/tutorial/cluster-maintenance-window/) when standard updates cannot begin. Atlas performs standard updates that don't involve cluster restarts or resyncs outside of these hours. |

## Monitoring and Alerts

| Check | Action |
| --- | --- |
| square-o | Use built-in monitoring capabilities in Atlas via the Metrics tab to track cluster health and performance. |
| square-o | Configure alerts for various cluster metrics and events to proactively identify and respond to potential issues. As a starting point, review and configure recommended alerts. Consider setting up multiple alerts for different severity levels. |
| square-o | Integrate Atlas monitoring with your existing enterprise monitoring and observability tools if required. |
| square-o | Familiarize yourself with Performance Advisor, Real-Time Performance Panel (RTPP), and Query Profiler for performance tuning and optimization. |

To get recommendations and learn more about monitoring performance and alerts, see arch-center-monitoring-alerts.

## Operational Procedures and Team Readiness

| Check | Action |
| --- | --- |
| square-o | Define roles and responsibilities for managing and operating MongoDB Atlas. |
| square-o | Establish change control and auditability processes. To learn more, see arch-center-auditing-logging. |
| square-o | Develop a clear Disaster Recovery Process Documentation specific to your applications and Atlas setup. To learn more, see arch-center-dr. |
| square-o | Ensure your team is trained on MongoDB Atlas fundamentals, security best practices, and operational procedures. Consider [MongoDB University](https://university.mongodb.com/) and Professional Services for training and enablement. |
| square-o | Establish a process for engaging with [MongoDB Support](https://support.mongodb.com/welcome) for production issues or when MongoDB's access level is required. |
| square-o | Plan for performance improvement using tools like Query Profiler and Performance Advisor. To learn more, see arch-center-monitoring-alerts. |
| square-o | Define how you will handle data lifecycle management. Configure archival strategies, such as [TTL indexes](/core/index-ttl/), or online archive. Ensure that the application development team, (and not the operations team), handles decisions for archiving. |
| square-o | Consider how your developers will connect to and develop with Atlas clusters. Useful tools include: - Atlas UI - [Compass](/) - [MongoDB Shell](/) - [Atlas CLI](/install-atlas-cli/) - [MCP Server](https://www.mongodb.com/docs/mcp-server/) - [MongoDB for VSCode](https://www.mongodb.com/docs/mongodb-vscode/) - [MongoDB for IntelliJ](https://www.mongodb.com/docs/mongodb-intellij/) Ensure that your developers can easily install, access, and integrate your tools in your toolchain. Set up appropriate access. |
| square-o | Establish integration strategies with other tooling and services, such as Datadog, Prometheus, PagerDuty, and other tools. To learn more, see arch-center-monitoring-alerts. |
| square-o | Consider establishing a MongoDB Center of Excellence (CoE) within your organization to foster best practices and knowledge sharing. |

By completing these checklist actions, you will enhance your operational readiness for deploying and managing MongoDB Atlas. This will ensure that you set up a reliable, secure, and performant database environment.

## Next Steps

Use the left navigation to find features and best practices for each [Well-Architected Framework](https://www.mongodb.com/resources/products/capabilities/well-architected-framework) pillar.

- arch-center-monitoring-alerts
- arch-center-network-security
- arch-center-backups
