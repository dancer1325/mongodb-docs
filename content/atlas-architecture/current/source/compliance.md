# Features for Compliance with External Standards

Use the following Atlas features to address compliance requirements.

## Certifications and Compliance with Regulations

MongoDB Atlas offers various features and configurations to help you address security, compliance, and privacy requirements. MongoDB Atlas data platform undergoes rigorous independent third-party audits to verify its security, privacy, and organizational controls. Atlas adheres to compliance frameworks including ISO/ IEC 27001, SOC2 Type II, PCI DSS, and others listed in our [Atlas Trust Center](https://www.mongodb.com/products/platform/trust).  You can view and download attestations, compliance reports, and other compliance documentation in our [Customer Trust Portal](https://trust.mongodb.com/).

## MongoDB Atlas for Government

MongoDB [Atlas for Government](https://www.mongodb.com/products/platform/atlas-for-government) is a fully-managed multi-cloud developer data platform authorized at FedRAMP® Moderate that:

- Uses a secure, fully-managed, dedicated FedRAMP® authorized environment.
- Supports the unique requirements and missions of the U.S. Government.
- Offers a set of features and the scalability needed to modernize legacy applications.

## Atlas Resource Policies

To support your compliance requirements, Atlas Resource Policies offer organization-wide controls for configuring and managing Atlas resources in alignment with your security, compliance, and operational best practices. Organization Owners can define rules that govern user actions when creating or modifying resources such as clusters, network configurations, and project settings. We recommend that you set a resource policy according to your company's standards at Organization creation time. Atlas Resource Policies support your compliance objectives by enabling you to:

- **Enforce Minimum TLS Version:** Mandate the use of modern TLS (Transport Layer Security) protocols across all Atlas deployments, enhancing security and mitigating risks associated with older, less secure versions. This ensures adherence to contemporary encryption standards for all data in transit.
- **Customize Default TLS Ciphers:** Select a specific set of allowed TLS (Transport Layer Security) ciphers to optimize security based on operational needs while avoiding vulnerabilities associated with legacy encryption methods. This allows for fine-tuning encryption protocols to meet specific compliance requirements.
- **Restrict VPC Peering Modifications:** Enable secure cross-network communication through established VPC (Virtual Private Cloud) peering connections while preventing configuration changes. Current project-level peerings remain active with their existing routing tables and security protocols, allowing customers to view but not alter these one-to-one VPC (Virtual Private Cloud) relationships and their associated network control mechanisms.
- **Restrict Private Endpoint Modifications:** Maintain secure service connectivity through existing private endpoint configurations with read-only access. Project-level connections remain functional with their current private IP addressing scheme, while customers can view but not modify these dedicated service connection points within their VPC (Virtual Private Cloud).
- **Control IP Access Lists:** Prevent unauthorized modifications to IP access lists, supporting consistent and controlled network access to your databases. This strengthens database security by preserving carefully defined network boundaries and protecting against accidental configuration changes.
- **Set Cluster Tier Limits:** Define deployment guardrails by establishing both maximum and minimum cluster size limits that developers must adhere to when provisioning resources. This boundary-setting approach ensures teams can deploy appropriately sized environments within organization-approved parameters, optimizing infrastructure utilization while enforcing consistent resource allocation policies across all project workloads.
- **Set Maintenance Window Requirement:** Enhance platform stability by requiring a maintenance window for all projects. This governance control allows organizations to establish a predictable update period (without dictating a specific timeframe) to support consistent system maintenance according to operational needs.
- **Set Cloud Provider and Regions:** Set your cloud provider and distribute clusters across multiple regions and providers to meet your data residency requirements and ensure high availability.
- **Block Usage of the Wildcard IP Address:** Limit access to your cluster to only explicitly permitted IP addresses by not including the wildcard IP address in your IP access list or firewall rules. The wildcard IP address is 0.0.0.0/0 and allows access from anywhere.

To learn more, see [Atlas Resource Policies](/atlas-resource-policies/). The following resource policy example, allows clusters to be created on AWS (Amazon Web Services):

```
{
  "name": "Only Allow Clusters on AWS",
  "policies": [
    {
      "body": "forbid ( principal, action == cloud::Action::\"cluster.createEdit\", resource) unless { context.cluster.cloudProviders   == [cloud::cloudProvider::\"aws\"] };"
    }
  ]
}
```

The following example creates a Terraform resource policies file you can use to enforce resource policies in your application. The file enforces the following policies:

- Restricts cluster modification to only a specified cloud provider.
- Forbids cluster access from a wildcard IP address.
- Specifies a minimum and maximum cluster size.
- Prevents modification to private endpoints.

```
resource "mongodbatlas_resource_policy" "restrict_cloud_provider" {
  org_id = var.org_id
  name   = "restrict-cloud-provider"
  policies = [
    {
      body = <<EOF
        forbid (
            principal,
            action == ResourcePolicy::Action::"cluster.modify",
            resource
        )
        unless
          { context.cluster.cloudProviders == [ResourcePolicy::CloudProvider::"<cloud provider name>"] };
      EOF
    },
  ]
}

resource "mongodbatlas_resource_policy" "forbid_project_access_anywhere" {
  org_id = var.org_id
  name   = "forbid-project-access-anywhere"
  policies = [
    {
      body = <<EOF
        forbid (
            principal,
            action == ResourcePolicy::Action::"project.ipAccessList.modify",
            resource
        )
        when {context.project.ipAccessList.contains(ip("0.0.0.0/0"))};
    EOF
    },
  ]
}

resource "mongodbatlas_resource_policy" "restrict_cluster_size: {
    org_id = var.org_id
    name   = "restrict-cluster-size"
    policies = [
        {
            // restrict cluster size to a minimum of M30 and a maximum of M60
            body = <<EOF
               forbid (
                   principal,
                   action == ResourcePolicy::Action::"cluster.modify",
                   resource
               )
               when {
                   (context.cluster has minGeneralClassInstanceSizeValue && context.cluster.minGeneralClassInstanceSizeValue < 30) 
                   || (context.cluster has maxGeneralClassInstanceSizeValue && context.cluster.maxGeneralClassInstanceSize > 60)
               };
            EOF
        },
    ]
}

resource "mongodbatlas_resource_policy" "prevent-modifications-private-endpoints" {
  org_id = var.org_id
  name   = "prevent-modifications-private-endpoints"
  policies = [
    {
      body = <<EOF
        forbid (
            principal,
            action == ResourcePolicy::Action::"privateEndpoint.modify",
            resource
        )
        when {context.project.privateEndpoints == [
                \"aws:<VPC_ENDPOINT_ID>", 
                \"azure:<PRIVATE_ENDPOINT_RESOURCE_ID>:<PRIVATE_ENDPOINT_IP_ADDRESS>", 
                \"gcp:<GCP_PROJECT_ID>:<VPC_NAME>"
            ]};
        EOF
    },
  ]
}
```

## Encryption

You can implement encryption to amplify data security across all stages of data handling. Encryption is one of the most common requirements for ensuring compliance with industry security standards, and Atlas offers a robust set of encryption options to satisfy the requirements.

- By default, Atlas encrypts data in transit. Atlas requires TLS (Transport Layer Security)/SSL (Secure Sockets Layer) to encrypt the connections to your databases.
- By default, Atlas encrypts all data at rest using [cloud provider disk encryption](https://www.mongodb.com/docs/atlas/security-kms-encryption/). When using Atlas cloud backups, Atlas uses [AES-256](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingServerSideEncryption.html) encryption to encrypt all data stored in S3 (Simple Storage Service) buckets in your Atlas clusters. In addition, Atlas supports using AWS (Amazon Web Services) KMS (Key Management Service), AKV (Azure Key Vault), and GCP (Google Cloud Platform) to encrypt storage engines and cloud provider backups. To learn more, see [Encryption at Rest using your Key Management](/security-kms-encryption).
- You can use [Queryable Encryption](/core/queryable-encryption/) to secure queries on encrypted data on a select set of sensitive fields in a document stored on MongoDB. With Queryable Encryption, sensitive information remains protected even when users run queries on data. Use  well-researched non-deterministic encryption schemes to maintain a balance between security and functionality. With Queryable Encryption, you can perform the following tasks:

   - Encrypt sensitive data fields from the client-side.
   - Store sensitive data fields as fully randomized encrypted data on the database cluster-side, run with Atlas.
   - Run expressive queries on the encrypted data.

   MongoDB completes these tasks without the server having knowledge of the data it's processing. When using Queryable Encryption, sensitive data is encrypted throughout its lifecycle: in transit, at rest, in use, in logs, and backups. The data is only ever decrypted on the client-side, since only you have access to the encryption keys. You can set up Queryable Encryption using the following mechanisms:

   - [Automatic Encryption](/core/queryable-encryption/) enables you to perform encrypted read and write operations without having to add explicit calls to encrypt and decrypt fields. We recommend automatic encryption in most situations, as it streamlines the process of writing your client application. With automatic encryption, MongoDB automatically encrypts and decrypts fields in read and write operations.
   - [Explicit Encryption](/core/queryable-encryption/fundamentals/manual-encryption/) enables you to perform encrypted read and write operations through your MongoDB driver's encryption library. You must specify the logic for encryption with this library throughout your application. Explicit encryption provides fine-grained control over security, at the cost of increased complexity when configuring collections and writing code for MongoDB Drivers. With explicit encryption, you specify how to encrypt fields in your document for each operation you perform on the database, and you include this logic throughout your application. To learn more, see [Use Explicit Encryption](/core/queryable-encryption/tutorials/explicit-encryption/).

## Data Regionalization

Atlas supports over 110+ regions across AWS (Amazon Web Services), Azure (Microsoft Azure), and GCP (Google Cloud Platform). This global distribution of supported locations allows you to provision clusters and store data that complies with your data localization requirements. You can also deploy your Atlas clusters across multiple regions. When you deploy a multi-region deployment paradigm, you can partition your data to reside in distinct regions, each situated in a different geographical region. For example, you can store European customer data in Europe and U.S. customer data in the U.S. This allows you flexibility in your data localization choice, and reduces latency for users accessing data from their respective regions.

## Backup Snapshot Distribution

You can distribute backup snapshots and oplog data across multiple regions. For example, you can satisfy store backups in different geographical locations to enable disaster recovery in case of regional outages, or to maintain backups consistently with your data localization choices. To learn more, see [Snapshot Distribution](/backup/cloud-backup/snapshot-distribution/).

## Backup Compliance Policy

You can use Backup Compliance Policy in Atlas to secure business-critical data. This feature prevents all backup snapshots and oplog data stored in Atlas from being modified or deleted for a predefined retention period by any user, regardless of their Atlas role. This helps you gaurantee that your backups are fully WORM (Write Once Read Many)-compliant. Only a designated, authorized user can turn off this protection after completing a verification process with MongoDB support. This adds a mandatory manual delay and cooldown period so that an attacker cannot easily change the backup policy and export the data. To learn more, see [Configure a Backup Compliance Policy](/backup/cloud-backup/backup-compliance-policy/).

- [DORA](/compliance/dora)
- [GDPR](/compliance/gdpr)
