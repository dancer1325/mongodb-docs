# Landing Zone Design

A landing zone is a framework for establishing a well-architected and pre-configured cloud environment. A MongoDB Atlas landing zone defines organization-specific requirements for operational efficiency, security, reliability, performance, and cost, as well as the tools, procedures, and Atlas configurations that teams must use to meet these requirements. We recommend that all enterprise customers design a landing zone before moving workloads to Atlas. Designing and implementing a landing zone can help you avoid expensive efforts to redesign initial setups later. For example, in recent discussions to move their workloads to Atlas, one longtime MongoDB enterprise customer cited concerns about peer financial services companies struggling with user data leaks and runaway cloud costs due to inconsistent encryption policies and redundant servers across business units. To avoid these risks, this enterprise customer worked with our [Professional Services](https://www.mongodb.com/services/consulting) team to design a landing zone that aligned all of their teams and stakeholders on company requirements, including encryption-at-rest with BYOK (Bring Your Own Key) and FinOps integrations to tag and track spending. You can use this Architecture Center content as a starting point for you to build your own well-architected Atlas environment. We recommend that you compile the following resources in a document and customize them to meet your organization's goals. MongoDB's [Professional Services](https://www.mongodb.com/services/consulting) can help you adjust your landing zone design to best fit your enterprise's requirements.

## What Are Some Landing Zone Considerations?

As you create your landing zone, define requirements for the following considerations:

| Consideration | Description |
| --- | --- |
| System Hierarchy | Choose a deployment hierarchy that groups your Atlas organizations, projects, and clusters to promote isolation and separation of responsibilities between business units, environments, and billable groups as needed. For example, you can group business units into individual organizations to ensure that Sales credentials cannot authenticate to Product resources. We also recommend that you separate your development and production environments into individual projects or organizations to meet different security and resiliency requirements for each environment. To get recommendations and learn more about this topic, see arch-center-hierarchy. |
| Security | By default, Atlas blocks all access to your clusters, enforces TLS (Transport Layer Security)/SSL (Secure Sockets Layer) encryption for all connections to your databases, and encrypts data at rest using cloud provider disk encryption. You must define the following security requirements to enable secure access to your clusters: - Network security configurations such as IP access list restrictions, or required private endpoints and VPC (Virtual Private Cloud) connections to limit the extension of your network trust boundary - Mechanisms for how to authenticate users and authorize their access to the Atlas control plane ({atlas-ui+} and Atlas Administration API), database resources, and database operations - Additional data encryption requirements for data in transit, at rest, and in use To get recommendations and learn more about this topic, see the following pages: - arch-center-network-security - arch-center-auth - arch-center-data-encryption - arch-center-auditing-logging |
| Compliance | Consider how your deployment's data residency determines data sovereignty and therefore which laws apply to your data. Identify and account for any specific legal and regulatory requirements not clearly articulated within other categories of requirements. Your data residency depends on which regions and geographies you choose to deploy to in your deployment paradigm, and whether you choose to partition your data between geographies. To get recommendations and learn more about this topic, see the following pages: - arch-center-compliance - arch-center-paradigms |
| Reliability and Resiliency | Define and record standards for reliability and resiliency that can be achieved through a disaster recovery plan and high availability architecture: - To begin, define an optimal RPO (Recovery Point Objective) and RTO (Recovery Time Objective) for your organization based on app criticality levels. - If your RTO and RPO are near-zero, then define a **high availability architecture** to ensure near-continuous operation during outages. This may involve requiring additional nodes, regions, or cloud providers in your deployment paradigm to increase redundancy and support automatic failover, and scheduling maintenance windows to ensure that primary node restarts occur outside of business-critical hours. To learn more, see arch-center-high-availability and arch-center-paradigms. - If your RTO or RPO allow for longer periods of outage or data loss, define a **disaster recovery plan** to ensure that your application is prepared to recover from an outage. To learn more, see arch-center-dr and arch-center-backups. A thorough disaster recovery plan does the following: - Documents and tests recovery procedures such as shifting workloads between regions or cloud providers or performing snapshot restores to previous versions to recover from possible disaster scenarios such as zonal, regional, or cloud-provider outages, resource failures, and data corruption events. - Defines a backup snapshot schedule and requirements for snapshot retention and mutli-region distribution to satisfy your RPO (Recovery Point Objective) and RTO (Recovery Time Objective). |
| Billing | Identify any specific requirements for billing, such as integrations with FinOps tools for reporting and charge-back. You can build these requirements into the automation and provisioning process for Atlas clusters to facilitate this integration. To get recommendations and learn more about this topic, see arch-center-billing-data. |
| Data Retention | Identify and record your data retention policies. This may require creating a data classification framework that integrates with automation to archive or purge data according to its nature and purpose, such as financial records, customer data, or employee information that entail different legal and regulatory requirements. Identify performance characteristics for the retrieval of archived records. To get recommendations and learn more about this topic, see the following pages: - arch-center-backups - arch-center-automation |
| Monitoring and Observability | Set standards for observability that define which logs and metrics you will monitor and how you will monitor them. For example, set up external system integrations that ingest Atlas log files, audit logs, or activity feed data, or configure alerts and reporting guidelines for certain event types. To get recommendations and learn more about this topic, see the following pages: - arch-center-monitoring-alerts. - arch-center-auditing-logging. |
| Auditing and Change Control | Define any auditing or change control requirements. This can include change approval processes and tools, along with reporting guidelines and integration into third-party systems for retention and analysis. These requirements may differ between your production and non-production environments. To get recommendations and learn more about this topic, see arch-center-auditing-logging. |

## Design Your Landing Zone

MongoDB's [Professional Services](https://www.mongodb.com/services/consulting) team partners with enterprise customers to create custom landing zones for Atlas. If you're working with MongoDB's [Professional Services](https://www.mongodb.com/services/consulting), the resources on this page can also help you plan for those discussions. Use the following resources as a starting point for your Atlas landing zone. Designing a landing zone is an iterative process that involves reviewing and realigning standards across teams. We recommend that you compile all diagrams, recommendations, and examples on this page in a document and adjust them to meet your organization's requirements.

### Example Landing Zone Diagram

The following example diagram unifies many of the architectural diagrams across the Atlas Architecture Center in one image to visualize your landing zone. You can adjust it as needed to customize it for your organization.

!["A diagram showing an example Atlas landing zone."](/includes/images/LandingZone.svg)

See the following landing zone guidance for more example setups with different cloud providers:

- [GCP's FAST MongoDB Atlas Configuration](https://github.com/GoogleCloudPlatform/cloud-foundation-fabric/tree/master/fast/project-templates/data-mongodb) is a project template that creates and configures a managed Atlas cluster and connects it to a local VPC (Virtual Private Cloud) network via Private Endpoints.
- The [AWS and MongoDB Atlas Landing Zone](https://github.com/mongodb-partners/AWS-MongoDB-Atlas-Landing-Zone) is a Terraform script that automates the setup of an Atlas project, cluster, and private endpoint alongside an AWS (Amazon Web Services) VPC (Virtual Private Cloud) with subnets and NAT gateways in multiple Availability Zones. This landing zone setup provides a starting point for enterprises to adopt MongoDB Atlas services as part of a secure cloud storage solution.

### Information In The Atlas Architecture Center

To begin, copy the relevant guidance and examples in arch-center-hierarchy, which helps you create your first foundational components in Atlas. Then, review the guidance and examples for each of the pages nested under each [Well-Architected Framework](https://www.mongodb.com/resources/products/capabilities/well-architected-framework) pillar in the Atlas Architecture Center. Copy the diagrams, recommendations, tools, and examples that are relevant for your organization. The Atlas Architecture Center pages include:

- Operational Efficiency

   - arch-center-automation
   - arch-center-monitoring-alerts
- Security

   - arch-center-network-security
   - arch-center-auth
   - arch-center-data-encryption
   - arch-center-compliance
   - arch-center-auditing-logging
- Reliability

   - arch-center-high-availability
   - arch-center-backups
   - arch-center-dr
- Performance

   - arch-center-scalability
   - arch-center-latency-strategies
- Cost Optimization

   - arch-center-cost-saving-config
   - arch-center-billing-data

### Your Organization's Requirements

Adjust the example landing zone diagram, recommendations, and examples that you copied from the Atlas Architecture Center to fit your organization's specific requirements. For example, if you use only Google Cloud as a cloud provider, your landing zone should specify that requirement and you should exclude any recommendations and examples applicable only to AWS (Amazon Web Services) and Azure (Microsoft Azure). To identify more considerations and requirements specific to your organization, see the previous section on Landing Zone Considerations.

## Next Steps

See the arch-center-migration page to plan your migration to Atlas or use the left navigation to find features and best practices for each [Well-Architected Framework](https://www.mongodb.com/resources/products/capabilities/well-architected-framework) pillar.
