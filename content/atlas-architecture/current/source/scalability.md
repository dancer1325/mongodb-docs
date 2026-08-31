# Guidance for Atlas Scalability

## Features for Atlas Scalability

Auto-scaling enables clusters to automatically adjust their tier, storage capacity, or both, in response to real-time use. Atlas analyzes the CPU and memory utilization to determine when and whether to scale the cluster tier up and down. See Cluster tier scaling to learn more about the conditions under which Atlas scales up or down your cluster nodes. You can also specify a range of maximum and minimum cluster sizes that your cluster can automatically scale to in order to guarantee minimum performance or control costs. Atlas won't scale a cluster if the new tier falls outside of your specified size range or if your memory usage would exceed the capacity of the new tier. Auto-scaling is throttled with a delay to scale a cluster tier up or down to ensure that auto-scaling doesn't cause any application impact. Therefore, it is best suited for a steadily growing or declining application load, not sudden spikes in which the database is being inundated with usage. If your workload experiences frequent spikes or if you are expecting a large increase in traffic because of an event or a launch, MongoDB recommends that you pre-scale programmatically. Atlas deployment templates, as referenced in the Recommended Deployment Topologies, provide you with horizontal and vertical scaling options. Specifically, sharding distributes data across numerous machines, which is useful when no single server can handle your workloads. Sharding follows a shared-nothing architecture, a distributed computing architecture where none of the nodes share any resources with each other. See [Choose a Shard Key](/core/sharding-choose-a-shard-key) to learn more about the ideal choice of shard key that allows MongoDB to distribute documents evenly throughout your cluster while facilitating common query patterns. Furthermore, see [Performance Best Practices: Sharding](https://www.mongodb.com/blog/post/performance-best-practices-sharding) to learn about the key sharding strategies, such as ranged sharding, hashed sharding, and zoned sharding. Upgrading an Atlas cluster to the next available Atlas tier is available through the Atlas control plane GUI, the [Atlas Administration API](https://www.mongodb.com/docs/atlas/reference/api-resources-spec/v2/#tag/Clusters/operation/upgradeSharedCluster), or through IaC tools, such as the Atlas Kubernetes Operator, the MongoDB & HashiCorp Terraform, or the Atlas CLI. See arch-center-automation to learn more. Changing an Atlas tier, either upscaling or downscaling, allows zero downtime. The tier changes in a rolling fashion, which involves electing a secondary member as a replacement, promoting this secondary member to become the new primary, then restoring or replacing the failing member to ensure that the cluster is returned to its target configuration as soon as possible. Horizontal scaling occurs post-deployment based on Administrator action, which can be triggered from a programmatic script. Some cluster templates require sharded clusters. Starting with MongoDB version 8.0, you may make use of [embedded config servers](/reference/command/transitionFromDedicatedConfigServer) to reduce costs associated with config servers on small sharded clusters. The low CPU option in Atlas helps applications that require higher memory but not as much processing power. This option provides instances with half the vCPUs compared to the General tier of the same cluster size, reducing costs for workloads that are memory-intensive but not CPU-bound. Data tiering and archival allows you to archive data in low-cost storage while still enabling queries alongside live cluster data, which is particularly useful for long-term record retention. To optimize this process, MongoDB recommends that you automate data archiving with simple, configurable rules. See Archive Data to learn more about the criteria that you can specify in an archiving rule. For scenarios where data retention is not a priority, Atlas offers the option to automatically delete unused data based on date criteria. For infrequently accessed data, [TTL indexes](/core/index-ttl/) are special single-field indexes that automatically delete documents from a collection after a specified period or at a set clock time. This is particularly useful for data like logs, session information, or event data that only needs to be retained for a limited time. To create a TTL index, you can define an index on a field that holds date values and specify a time-to-live duration in seconds. Atlas also provides you with automated tools, such as the [Performance Advisor](/tutorial/performance-advisor/), to identify and optimize inefficient queries by adding or removing an index or changing your client's query structure. You can reduce unnecessary compute time and resource consumption by following Performance Advisor's actionable recommendations to enhance your query performance. Additionally, you can leverage intelligent index recommendations provided by Atlas to further improve data retrieval efficiency and minimize the resources needed for database operations.

## Recommendations for Atlas Scalability

<details>
<summary>Single-Region Deployment Recommendations</summary>

For single-region deployments, focus on vertical scaling strategies that optimize performance within a single cloud region:

- Enable auto-scaling for both compute and storage to handle gradual traffic increases without manual intervention.
- Use sharding when your dataset exceeds the capacity of a single server, even within a single region, to distribute load across multiple shards.
- Consider the low CPU instance option for memory-intensive workloads that do not require high processing power.
- Implement data tiering and archival for historical data to reduce storage costs while maintaining query capabilities.
- Monitor query performance with the Performance Advisor and implement recommended indexes to optimize resource utilization.
- Pre-scale clusters before expected traffic spikes rather than relying solely on auto-scaling for sudden load increases.
</details>

<details>
<summary>Multi-Region and Multi-Cloud Deployment Recommendations</summary>

For multi-region and multi-cloud deployments, implement scaling strategies that account for geographic distribution and cross-cloud complexity:

- Configure read replicas in regions closest to your users to reduce latency and distribute read workloads geographically.
- Use zone sharding to partition data based on geographic or logical boundaries, ensuring data locality and compliance requirements.
- Implement global clusters with local reads and writes to optimize performance across regions while maintaining data consistency.
- Monitor network latency between regions and adjust replica priorities to ensure optimal failover behavior.
- Consider the impact of cross-region data transfer costs when implementing auto-scaling across multiple regions.
- Use different cluster tiers in different regions based on regional usage patterns and cost optimization strategies.
- Implement region-specific backup and archival strategies to comply with data residency requirements.
</details>

### All Deployment Paradigm Recommendations

The following recommendations apply to all deployment paradigms. For development and testing environments, do not enable auto-scaling compute and auto-scaling storage. This saves costs in your non-production environments. For staging and production environments, we recommend that you:

- Enable auto-scaling for compute and storage for instances where your application grows organically from small to medium. If you use IaC (Infrastructure as Code) tools, leverage settings to ignore resource drift caused by auto-scaling. For example, in Terraform, if `disk_gb_enabled` is true, Atlas will automatically scale disk size up and down. This will cause the value of `disk_size_gb` returned to potentially be different than what is specified in the Terraform config and if one then applies a plan, not noting this, Terraform will scale the cluster disk size back to the original `disk_size_gb` value. To prevent this a lifecycle customization should be used, such as: `lifecycle { ignore_changes = [disk_size_gb] }`. Similarly, in Terraform, if `compute_enabled` is true, then Atlas will automatically scale up to the maximum provided and down to the minimum, if provided. This will cause the value of `provider_instance_size_name` returned to potentially be different than what is specified in the Terraform config, and if one then applies a plan, not noting this, Terraform will scale the cluster back to the original `instanceSizeName` value. To prevent this a lifecycle customization should be used, such as: `lifecycle { ignore_changes = [provider_instance_size_name] }`.

## Automation Examples: Atlas Scalability

> **Tip:**
> For Terraform examples that enforce our recommendations across all pillars, see one of the following examples in GitHub:
>
> - [Staging/Prod](https://github.com/mongodb/docs/blob/main/content/atlas-architecture/current/source/includes/examples/tf-staging-prod-complete/)
> - [Dev/Test](https://github.com//mongodb/docs/blob/main/content/atlas-architecture/current/source/includes/examples/tf-dev-test-complete/)

The following examples enable auto-scaling compute and storage using Atlas tools for automation. These examples also apply other recommended configurations, including:

**Dev and Test Environments**

- Cluster tier set to `M10` for a dev/test environment. Use the cluster size guide to learn the recommended cluster tier for your application size.
- Single Region, 3-Node Replica Set / Shard deployment topology.

Our examples use AWS (Amazon Web Services), Azure (Microsoft Azure), and Google Cloud interchangeably. You can use any of these three cloud providers, but you must change the region name to match the cloud provider. To learn about the cloud providers and their regions, see [Cloud Providers](/reference/cloud-providers/).

---

**Staging and Prod Environments**

- Cluster tier set to `M30` for a medium-sized application. Use the cluster size guide to learn the recommended cluster tier for your application size.
- Single Region, 3-Node Replica Set / Shard deployment topology.

Our examples use AWS (Amazon Web Services), Azure (Microsoft Azure), and Google Cloud interchangeably. You can use any of these three cloud providers, but you must change the region name to match the cloud provider. To learn about the cloud providers and their regions, see [Cloud Providers](/reference/cloud-providers/).

---

**CLI**

> **Note:**
> Before you can create resources with the Atlas CLI, you must:
>
> - [Create your paying organization](/billing/#configure-a-paying-organization) and [create an API key](/configure-api-access/) for the paying organization.
> - [Install the Atlas CLI](/install-atlas-cli/)
> - [Connect from the Atlas CLI](/connect-atlas-cli/) using the steps for Programmatic Use.

### Create One Deployment Per Project

**Dev and Test Environments**

For your development and testing environments, auto-scaling compute and storage is disabled to save costs.
---

**Staging and Prod Environments**

For your staging and production environments, create the following `cluster.json` file for each project. Change the IDs and names to use your values:

```
{
    "clusterType": "REPLICASET",
    "links": [],
    "name": "CustomerPortalProd",
    "mongoDBMajorVersion": "8.0",
    "replicationSpecs": [
      {
        "numShards": 1,
        "regionConfigs": [
          {
            "electableSpecs": {
              "instanceSize": "M30",
              "nodeCount": 3
            },
            "priority": 7,
            "providerName": "GCP",
            "regionName": "EASTERN_US",
            "analyticsSpecs": {
              "nodeCount": 0,
              "instanceSize": "M30"
            },
            "autoScaling": {
              "compute": {
                "enabled": true,
                "scaleDownEnabled": true
              },
              "diskGB": {
                "enabled": true
              }
            },
            "readOnlySpecs": {
              "nodeCount": 0,
              "instanceSize": "M30"
            }
          }
        ],
        "zoneName": "Zone 1"
      }
    ]
  }
```

After you create the `cluster.json` file, run the following command for each project. The command uses the `cluster.json` file to create a cluster.

```
atlas cluster create --projectId 5e2211c17a3e5a48f5497de3 --file cluster.json
```

---

For more configuration options and info about this example, see atlas-clusters-create.
---

**Terraform**

> **Note:**
> Before you can create resources with Terraform, you must:
>
> - [Create your paying organization](/billing/#configure-a-paying-organization) and [create an API key](/configure-api-access/) for the paying organization. Store your API key as environment variables by running the following command in the terminal:
>
>    ```
>    export MONGODB_ATLAS_PUBLIC_KEY="<insert your public key here>"
>    export MONGODB_ATLAS_PRIVATE_KEY="<insert your private key here>"
>    ```
>
> - [Install Terraform](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli)

> **Important:**
> The following examples use MongoDB Atlas Terraform Provider version 2.x (`~> 2.2`). If you're upgrading from provider version 1.x, see the [2.0.0 Upgrade Guide](https://registry.terraform.io/providers/mongodb/mongodbatlas/latest/docs/guides/2.0.0-upgrade-guide) for breaking changes and migration steps. The examples use the `mongodbatlas_advanced_cluster` resource with v2.x syntax.
>

### Create the Projects and Deployments

**Dev and Test Environments**

For your development and testing environments, auto-scaling compute and storage is disabled to save costs.
---

**Staging and Prod Environments**

For your staging and production environments, create the following files for each application and environment pair. Place the files for each application and environment pair in their own directory. Change the IDs, names, and disk size to use your values.

#### main.tf

```
# Create a Group to Assign to Project 
resource "mongodbatlas_team" "project_group" {
  org_id = var.atlas_org_id
  name   = var.atlas_group_name
  usernames = [
    "user1@example.com",
    "user2@example.com"
  ]
}

# Create a Project
resource "mongodbatlas_project" "atlas-project" {
  org_id = var.atlas_org_id
  name = var.atlas_project_name
}

# Assign the team to project with specific roles
resource "mongodbatlas_team_project_assignment" "project_team" {
  project_id = mongodbatlas_project.atlas-project.id
  team_id    = mongodbatlas_team.project_group.team_id
  role_names = ["GROUP_READ_ONLY", "GROUP_CLUSTER_MANAGER"]
}

# Create an Atlas Advanced Cluster 
resource "mongodbatlas_advanced_cluster" "atlas-cluster" {
  project_id = mongodbatlas_project.atlas-project.id
  name = "ClusterPortalProd"
  cluster_type = "REPLICASET"
  mongo_db_major_version = var.mongodb_version
  replication_specs = [
    {
      region_configs = [
        {
          electable_specs = {
            instance_size = var.cluster_instance_size_name
            node_count    = 3
            disk_size_gb  = var.disk_size_gb
          }
          auto_scaling = {
            disk_gb_enabled = var.auto_scaling_disk_gb_enabled
            compute_enabled = var.auto_scaling_compute_enabled
            compute_max_instance_size = var.compute_max_instance_size
          }
          priority      = 7
          provider_name = var.cloud_provider
          region_name   = var.atlas_region
        }
      ]
    }
  ]
  # Prevent Terraform from reverting auto-scaling changes
  lifecycle {
    ignore_changes = [
      replication_specs[0].region_configs[0].electable_specs.instance_size,
      replication_specs[0].region_configs[0].electable_specs.disk_size_gb
    ]
  }
  tags = {
    BU       = "ConsumerProducts"
    TeamName = "TeamA"
    AppName  = "ProductManagementApp"
    Env      = "Production"
    Version  = "8.0"
   Email    = "marissa@example.com"
  }
}

# Outputs to Display
output "atlas_cluster_connection_string" { value = mongodbatlas_advanced_cluster.atlas-cluster.connection_strings.standard_srv }
output "project_name"      { value = mongodbatlas_project.atlas-project.name }
```

> **Note:**
> To create a multi-region cluster, specify each region in its own `region_configs` object and nest them in the `replication_specs` object, as shown in the following example:

```
replication_specs = [
  {
    region_configs = [
      {
        electable_specs = {
          instance_size = "M10"
          node_count    = 2
        }
        provider_name = "GCP"
        priority      = 7
        region_name   = "NORTH_AMERICA_NORTHEAST_1"
      },
      {
        electable_specs = {
          instance_size = "M10"
          node_count    = 3
        }
        provider_name = "GCP"
        priority      = 6
        region_name   = "WESTERN_US"
      }
    ]
  }
]
```

#### variables.tf

```
# Atlas Organization ID 
variable "atlas_org_id" {
  type        = string
  description = "Atlas Organization ID"
}

# Atlas Project Name
variable "atlas_project_name" {
  type        = string
  description = "Atlas Project Name"
}

# Atlas Group Name
variable "atlas_group_name" {
  type        = string
  description = "Atlas Group Name"
}

# Atlas Project Environment
variable "environment" {
  type        = string
  description = "The environment to be built"
}

# Cluster Instance Size Name 
variable "cluster_instance_size_name" {
  type        = string
  description = "Cluster instance size name"
}

# Cloud Provider to Host Atlas Cluster
variable "cloud_provider" {
  type        = string
  description = "AWS or GCP or Azure"
}

# Atlas Region
variable "atlas_region" {
  type        = string
  description = "Atlas region where resources will be created"
}

# MongoDB Version 
variable "mongodb_version" {
  type        = string
  description = "MongoDB Version"
}

# Storage Auto-scaling Enablement Flag
variable "auto_scaling_disk_gb_enabled" {
  type        = bool
  description = "Flag that specifies whether disk auto-scaling is enabled"
}

# Compute Auto-scaling Enablement Flag
variable "auto_scaling_compute_enabled" {
  type        = bool
  description = "Flag that specifies whether cluster tier auto-scaling is enabled"
}

# Disk Size in GB
variable "disk_size_gb" {
  type        = int
  description = "Disk Size in GB"
}
```

#### terraform.tfvars

```
atlas_org_id = "32b6e34b3d91647abb20e7b8"
atlas_project_name = "Customer Portal - Prod"
atlas_group_name = "Atlas Group"
environment = "prod"
cluster_instance_size_name = "M30"
cloud_provider = "AWS"
atlas_region = "US_WEST_2"
mongodb_version = "8.0"
auto_scaling_disk_gb_enabled = true
auto_scaling_compute_enabled = true
disk_size_gb = 40000
```

#### provider.tf

```
# Define the MongoDB Atlas Provider
terraform {
  required_providers {
    mongodbatlas = {
      source = "mongodb/mongodbatlas"
      version = "~> 2.2"
    }
  }
  required_version = ">= 1.0"
}

# Configure the MongoDB Atlas Provider
provider "mongodbatlas" {
  # Legacy API key authentication (backward compatibility)
  public_key  = var.mongodbatlas_public_key
  private_key = var.mongodbatlas_private_key

  # Recommended: Service account authentication
  # Uncomment and configure the following for service account auth:
  # service_account_id = var.mongodb_service_account_id
  # private_key_file   = var.mongodb_service_account_key_file
}
```

After you create the files, navigate to each application and environment pair's directory and run the following command to initialize Terraform:

```
terraform init
```

Run the following command to view the Terraform plan:

```
terraform plan
```

After adding the `lifecycle` block to explicitly change `disk_size_gb` and `instant_size`, comment out the `lifecycle` block and run `terraform apply`. Please be sure to uncomment the `lifecycle` block once done to prevent any accidental changes. Run the following command to create one project and one deployment for the application and environment pair. The command uses the files and the [MongoDB & HashiCorp Terraform](https://www.mongodb.com/atlas/hashicorp-terraform) to create the projects and clusters:

```
terraform apply
```

When prompted, type `yes` and press Enter to apply the configuration.
---

For more configuration options and info about this example, see [MongoDB & HashiCorp Terraform](https://www.mongodb.com/atlas/hashicorp-terraform) and the [MongoDB Terraform Blog Post](https://www.mongodb.com/developer/products/atlas/deploy-mongodb-atlas-terraform-aws/).
---
