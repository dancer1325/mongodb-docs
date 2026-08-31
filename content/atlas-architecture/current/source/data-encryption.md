# Guidance for Atlas Data Encryption

Atlas offers several encryption features to protect data while in transit, at rest, and in use to safeguard data through its full lifecycle.

## Features for Atlas Data Encryption
### Encryption in Transit

Encryption in transit secures data during transmission between clients and servers, ensuring that your data cannot be inspected while in motion. In Atlas, all network traffic to clusters is protected by Transport Layer Security (TLS), which is enabled by default and cannot be disabled. Data transmitted to and between nodes is encrypted in transit using TLS, ensuring secure communication throughout. You can select which TLS version to use in Atlas. TLS 1.2 and a minimum key length of 128 bits are the recommended default settings. All encryption in transit is supported by the OpenSSL FIPS (Federal Information Processing Standards) Object Module.

![An image showing encryption in transit with TLS between client applications and MongoDB Atlas.](/includes/images/encryption-in-transit.svg)

### Encryption at Rest

Encryption at rest ensures that all data on disk is encrypted and only visible once decrypted by an authorized process or application. In Atlas, customer data is automatically encrypted at rest using AES-256. This process utilizes your cloud provider's disk encryption, with the provider managing the encryption keys. This process cannot be disabled. By default, Encryption at Rest is volume-level encryption. Additionally, you can enable database-level encryption by bringing your own key (BYOK). This is the customer-managed key (CMK) that you add with a key management service (KMS), such as AWS KMS, Google Cloud KMS, or Azure Key Vault. The BYOK feature provides file-level encryption and is equivalent to Transparent Data Encryption (TDE), meeting enterprise TDE requirements. Encryption with customer key management adds another layer of security for additional confidentiality and data segmentation. To learn more, see security-kms-encryption.

![An image showing encryption at rest with an additional customer-managed key.](/includes/images/encryption-with-cmk.svg)

### Encryption in Use

Encryption in use secures data while it's being processed. MongoDB has two features for encryption in use to meet your data protection needs: Client-Side Field-Level Encryption and Queryable Encryption.

#### Client-Side Field-Level Encryption

Client-Side Field-Level Encryption (CSFLE) is an in-use encryption capability that enables a client application to encrypt sensitive data before storing it in the MongoDB database. Sensitive data is transparently encrypted, remains encrypted throughout its lifecycle, and is only decrypted on the client side. You can selectively encrypt individual fields within a document, multiple fields within the document, or the entire document. You can optionally secure each field with its own key and decrypt them seamlessly on the client by using a MongoDB driver. CSFLE uses AES-256 in authenticated CBC mode to encrypt data. Additionally, you can select [randomized encryption](/core/csfle/fundamentals/encryption-algorithms/#randomized-encryption), which is not queryable, but might be required for certain security requirements. The following diagram demonstrates a CSFLE workflow where user records are stored in a MongoDB database and queried by the client. The user's social security number (SSN) is encrypted before being stored in the database. When the application submits a basic equality query on the field, the MongoDB driver uses the key to encrypt the query on the client side and sends the encrypted query to the server. The server returns the encrypted results to the client, which then decrypts the results before returning them to the authenticated client as readable plaintext. CSFLE supports all major cloud and on-premises key management services.

![An image showing an example client-side field-level encryption (CSFLE) workflow.](/includes/images/csfle-encryption.svg)

#### Queryable Encryption

Queryable Encryption helps organizations protect sensitive data when it is queried. Like CSFLE, it allows applications to encrypt your data on the client side before storing it in the MongoDB database. It also enables applications to perform expressive queries such as equality queries directly on the encrypted data by using an encrypted search algorithm. Queryable Encryption ensures protection for sensitive information without sacrificing the ability to perform queries on it. Queryable Encryption always uses non-deterministic encryption. To learn about the algorithms used in Queryable Encryption, see [Queryable Encryption White Paper](/resources/products/capabilities/queryable-encryption-technical-paper). The following diagram demonstrates a Queryable Encryption workflow where user records are stored in a MongoDB database and queried by the client. The user's date of birth (DOB) is encrypted before being stored in the database. When the application submits an expressive range query on the field, the MongoDB driver uses the key to encrypt the query and passes a cryptographic token with it to the MongoDB server. The server uses the encrypted search algorithm to process the query without knowing the actual data. Finally, the driver uses the key to decrypt the query results and returns them to the authenticated client as readable plaintext.

![An image showing an example Queryable Encryption workflow.](/includes/images/queryable-encryption.svg)

## Recommendations for Atlas Data Encryption

<details>
<summary>Single-Region Deployment Recommendations</summary>

Single-region deployments have no unique considerations for data encryption. See the next section for "All Deployment Paradigm Recommendations".
</details>

<details>
<summary>Multi-Region and Multi-Cloud Deployment Recommendations</summary>

In multi-region and multi-cloud deployments, you can enable database-level encryption by bringing your own customer-managed key (BYOK). Some KMS providers support multi-region capabilities. For example:

- GCP KMS supports multi-region deployment and automatic key replication.
- In AWS KMS, you need to explicitly set the keys to be multi-region and replicate them to every region Atlas is deployed in. To learn more, see [Create multi-Region replica keys in AWS](https://docs.aws.amazon.com/kms/latest/developerguide/multi-region-keys-replicate.html).

However, in the event of a regional outage affecting the configured KMS endpoint, the Atlas integration with a KMS provider **doesn't provide automatic failover** of the connection or access mechanism to the KMS across regions. If the configured KMS provider becomes unavailable, Atlas performs periodic validation of your KMS configuration. If your KMS provider credentials become invalid or the encryption key is deleted or disabled, Atlas shuts down the `mongod` and `mongos` processes on the next scheduled validation check and notifies you via email, reflecting the status in the Atlas UI. When Atlas can't connect to your key management provider, it doesn't shut down your processes. Therefore, if access to the KMS is disrupted due to a regional issue, the cluster might continue running until you manually restart it or until validation fails more critically. Because of this, if the KMS goes down, you can change the configuration. To recover access to the encryption key in a multi-region outage scenario, you must manually change the configuration in Atlas to point to a different, accessible KMS instance or configuration. If you use Private Endpoints to connect to KMS, you must establish a private endpoint from each region where Atlas nodes are deployed back to the primary KMS. This is to ensure the configuration is still valid during periodic checks and during initial configurations or key rotations each node needs to access the customer master key stored in the primary KMS. In summary, Atlas integration with the BYOK providers for Encryption at Rest doesn't support multi-region failover automatically and you must manually change the configuration.
</details>

### All Deployment Paradigm Recommendations

The following recommendations apply to all deployment paradigms.

#### Encryption with Customer Key Management

You enable encryption with customer key management at the project level. After you enable it, the setting automatically applies to all clusters created within the project, which ensures consistent data protection across your environment. We recommend that you use a key management service (KMS) such as AWS KMS, Google Cloud KMS, or Azure Key Vault. For **staging and production environments**, we recommend that you enable encryption with customer key management when you provision your clusters to avoid relying on application development teams to configure it later on. For **development and testing environments**, consider skipping encryption with customer key management to save costs. However, if you store sensitive data in Atlas, such as for healthcare or financial services industries, consider enabling encryption with customer key management in development and testing environments as well.

Use the following methods to enable encryption with customer key management:

- [Atlas UI](/security-kms-encryption)
- Atlas Administration API
- [Terraform](https://registry.terraform.io/providers/mongodb/mongodbatlas/latest/docs/resources/encryption_at_rest)

To learn how to configure encryption with customer key management when provisioning a new Atlas organization, project, and cluster, see arch-center-create-hierarchy-example.

#### Data Classification

During the provisioning process, we also recommend assessing the sensitivity of certain fields in your data and classifying them to determine which data requires encryption and what global restrictions to apply to these groups. As a general guideline, we recommend that you apply Queryable Encryption on all sensitive fields, in addition to following [data modeling](/data-modeling/) best practices. Consider the following data classification levels as a starting point:

- **Public Data**: Data that represents little to no risk to the company if unauthorized disclosure, alteration, or destruction of data occurs. While confidentiality is less of a concern, you should still apply authorization controls to prevent unauthorized modification or destruction of public data. Examples: Products, Brochures, Training Information
- **Private Data**: Data that represents a moderate risk to the company if unauthorized disclosure, alteration, or destruction of data occurs. By default, all institutional data that is not explicitly classified as restricted or public data should be treated as private data. Apply CSFLE or Queryable Encryption on any fields that carry private data such as PII (Personally Identifiable Information). Examples: Customer Information, Contracts, Product Costs
- **Restricted Data**: Data that represents significant risk to the company if unauthorized disclosure, alteration, or destruction of data occurs. Apply the highest level of security controls to restricted data, including CSFLE or Queryable Encryption on all fields and encryption with customer key management for additional security. Examples: Revenue information, Payroll, Security Risks

## Automation Examples: Atlas Data Encryption

> **Tip:**
> For Terraform examples that enforce our recommendations across all pillars, see one of the following examples in GitHub:
>
> - [Staging/Prod](https://github.com/mongodb/docs/blob/main/content/atlas-architecture/current/source/includes/examples/tf-staging-prod-complete/)
> - [Dev/Test](https://github.com//mongodb/docs/blob/main/content/atlas-architecture/current/source/includes/examples/tf-dev-test-complete/)

The following examples configure encryption with customer key management using Atlas tools for automation. Before you configure encryption with customer key management, you must create your organizations, projects, and clusters. To learn more, see arch-center-create-hierarchy-example.

### Configure Encryption with Customer Key Management

**Dev and Test Environments**

For your development and testing environments, consider skipping encryption with customer key management to save costs, unless you're in a highly-regulated industry or storing sensitive data. To learn more, see arch-center-orgs-projects-clusters-recs. To enable encryption with customer key management with Terraform, create the following resources. Change the IDs and names to use your values:

**AWS**

> **Tip:**
> For a complete configuration example, see Atlas Terraform Provider Example.

Alternatively, to simplify the configuration process, you can use the [encryption at rest Terraform module](https://registry.terraform.io/modules/terraform-mongodbatlas-modules/encryption-at-rest/mongodbatlas/latest).

```
resource "mongodbatlas_cloud_provider_access_setup" "setup_only" {
  project_id    = var.atlas_project_id
  provider_name = "AWS"
}

resource "mongodbatlas_cloud_provider_access_authorization" "auth_role" {
  project_id = var.atlas_project_id
  role_id    = mongodbatlas_cloud_provider_access_setup.setup_only.role_id

  aws {
    iam_assumed_role_arn = aws_iam_role.test_role.arn
  }
}

resource "mongodbatlas_encryption_at_rest" "test" {
  project_id = var.atlas_project_id

  aws_kms_config {
    enabled                = true
    customer_master_key_id = aws_kms_key.kms_key.id
    region                 = var.atlas_region
    role_id                = mongodbatlas_cloud_provider_access_authorization.auth_role.role_id
  }
}

data "mongodbatlas_encryption_at_rest" "test" {
  project_id = mongodbatlas_encryption_at_rest.test.project_id
}

output "is_aws_kms_encryption_at_rest_valid" {
  value = data.mongodbatlas_encryption_at_rest.test.aws_kms_config.valid
}
```

---

**Azure**

> **Tip:**
> For a complete configuration example, see Atlas Terraform Provider Example.

```
 :copyable: true

 resource "mongodbatlas_encryption_at_rest" "test" {
  project_id = var.atlas_project_id

  azure_key_vault_config {
    enabled           = true
    azure_environment = "AZURE"

    tenant_id       = var.azure_tenant_id
    subscription_id = var.azure_subscription_id
    client_id       = var.azure_client_id
    secret          = var.azure_client_secret

    resource_group_name = var.azure_resource_group_name
    key_vault_name      = var.azure_key_vault_name
    key_identifier      = var.azure_key_identifier
  }
}

data "mongodbatlas_encryption_at_rest" "test" {
  project_id = mongodbatlas_encryption_at_rest.test.project_id
}

output "is_azure_encryption_at_rest_valid" {
  value = data.mongodbatlas_encryption_at_rest.test.azure_key_vault_config.valid
}
```

---

**GCP**

```
resource "mongodbatlas_encryption_at_rest" "test" {
  project_id = var.atlas_project_id

  google_cloud_kms_config {
    enabled                 = true
    service_account_key     = "{\"type\": \"service_account\",\"project_id\": \"my-project-common-0\",\"private_key_id\": \"e120598ea4f88249469fcdd75a9a785c1bb3\",\"private_key\": \"-----BEGIN PRIVATE KEY-----\\nMIIEuwIBA(truncated)SfecnS0mT94D9\\n-----END PRIVATE KEY-----\\n\",\"client_email\": \"my-email-kms-0@my-project-common-0.iam.gserviceaccount.com\",\"client_id\": \"10180967717292066\",\"auth_uri\": \"https://accounts.google.com/o/oauth2/auth\",\"token_uri\": \"https://accounts.google.com/o/oauth2/token\",\"auth_provider_x509_cert_url\": \"https://www.googleapis.com/oauth2/v1/certs\",\"client_x509_cert_url\": \"https://www.googleapis.com/robot/v1/metadata/x509/my-email-kms-0%40my-project-common-0.iam.gserviceaccount.com\"}"
    key_version_resource_id = "projects/my-project-common-0/locations/us-east4/keyRings/my-key-ring-0/cryptoKeys/my-key-0/cryptoKeyVersions/1"
  }
}
```

For more configuration options and info about this example, see [Terraform documentation](https://registry.terraform.io/providers/mongodb/mongodbatlas/latest/docs/resources/encryption_at_rest).
---

---

**Staging and Prod Environments**

For your staging and production environments, we recommend that you enable encryption with customer key management when you provision your clusters. To learn more, see arch-center-orgs-projects-clusters-recs. To enable encryption with customer key management with Terraform, create the following resources. Change the IDs and names to use your values:

**AWS**

> **Tip:**
> For a complete configuration example, see Atlas Terraform Provider Example. Alternatively, to simplify the configuration process, you can use the [encryption at rest Terraform module](https://registry.terraform.io/modules/terraform-mongodbatlas-modules/encryption-at-rest/mongodbatlas/latest).

```
resource "mongodbatlas_cloud_provider_access_setup" "setup_only" {
  project_id    = var.atlas_project_id
  provider_name = "AWS"
}

resource "mongodbatlas_cloud_provider_access_authorization" "auth_role" {
  project_id = var.atlas_project_id
  role_id    = mongodbatlas_cloud_provider_access_setup.setup_only.role_id

  aws {
    iam_assumed_role_arn = aws_iam_role.test_role.arn
  }
}

resource "mongodbatlas_encryption_at_rest" "test" {
  project_id = var.atlas_project_id

  aws_kms_config {
    enabled                = true
    customer_master_key_id = aws_kms_key.kms_key.id
    region                 = var.atlas_region
    role_id                = mongodbatlas_cloud_provider_access_authorization.auth_role.role_id
  }
}

data "mongodbatlas_encryption_at_rest" "test" {
  project_id = mongodbatlas_encryption_at_rest.test.project_id
}

output "is_aws_kms_encryption_at_rest_valid" {
  value = data.mongodbatlas_encryption_at_rest.test.aws_kms_config.valid
}
```

---

**Azure**

> **Tip:**
> For a complete configuration example, see Atlas Terraform Provider Example.

```
 :copyable: true

 resource "mongodbatlas_encryption_at_rest" "test" {
  project_id = var.atlas_project_id

  azure_key_vault_config {
    enabled           = true
    azure_environment = "AZURE"

    tenant_id       = var.azure_tenant_id
    subscription_id = var.azure_subscription_id
    client_id       = var.azure_client_id
    secret          = var.azure_client_secret

    resource_group_name = var.azure_resource_group_name
    key_vault_name      = var.azure_key_vault_name
    key_identifier      = var.azure_key_identifier
  }
}

data "mongodbatlas_encryption_at_rest" "test" {
  project_id = mongodbatlas_encryption_at_rest.test.project_id
}

output "is_azure_encryption_at_rest_valid" {
  value = data.mongodbatlas_encryption_at_rest.test.azure_key_vault_config.valid
}
```

---

**GCP**

```
resource "mongodbatlas_encryption_at_rest" "test" {
  project_id = var.atlas_project_id

  google_cloud_kms_config {
    enabled                 = true
    service_account_key     = "{\"type\": \"service_account\",\"project_id\": \"my-project-common-0\",\"private_key_id\": \"e120598ea4f88249469fcdd75a9a785c1bb3\",\"private_key\": \"-----BEGIN PRIVATE KEY-----\\nMIIEuwIBA(truncated)SfecnS0mT94D9\\n-----END PRIVATE KEY-----\\n\",\"client_email\": \"my-email-kms-0@my-project-common-0.iam.gserviceaccount.com\",\"client_id\": \"10180967717292066\",\"auth_uri\": \"https://accounts.google.com/o/oauth2/auth\",\"token_uri\": \"https://accounts.google.com/o/oauth2/token\",\"auth_provider_x509_cert_url\": \"https://www.googleapis.com/oauth2/v1/certs\",\"client_x509_cert_url\": \"https://www.googleapis.com/robot/v1/metadata/x509/my-email-kms-0%40my-project-common-0.iam.gserviceaccount.com\"}"
    key_version_resource_id = "projects/my-project-common-0/locations/us-east4/keyRings/my-key-ring-0/cryptoKeys/my-key-0/cryptoKeyVersions/1"
  }
}
```

For more configuration options and info about this example, see [Terraform documentation](https://registry.terraform.io/providers/mongodb/mongodbatlas/latest/docs/resources/encryption_at_rest).
---

---
