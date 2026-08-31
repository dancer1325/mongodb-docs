# Guidance for Atlas Auditing

.. meta: :description: Learn about auditing mechanisms in MongoDB Atlas.

To monitor Atlas platform activities, use auditing.

## Features for Atlas Auditing

Available on `M10+` clusters, database auditing lets you track system activity for deployments with multiple users. As an Atlas administrator, you can create a custom JSON audit filter in MongoDB to precisely control what gets audited across your system. This approach lets you specify exactly which actions, database users, and Atlas roles should be monitored, giving you more granular control over your auditing process compared with the standard filter builder in the Atlas UI. With [manual auditing configuration](/core/auditing), you can monitor almost all documented [system event actions](/reference/audit-message/mongo/#audit-event-actions--details--and-results) in Atlas. The comprehensive database auditing capabilities provide detailed tracking of:

- DDL (Data Definition Language) operations
- DML (Data Manipulation Language) operations
- DCL (Data Control Language) operations.

This provides complete visibility into database schema changes, data modifications, and permission adjustments. For implementation guidance, including complete event lists and practical configuration examples, refer to the [MongoDB auditing](/core/auditing) and [Example Auditing Filters](/database-auditing/#example-auditing-filters) documentation. Additional setup instructions are available in the [Set up Database Auditing](/database-auditing) section.

## Create and Enable an Audit Filter in Atlas

You can create audit filters to limit audits to only certain operations of concern. When you enable auditing of operations, Atlas records all auditable operations as detailed in [Audit Event Actions, Details, and Results](/reference/audit-message/mongo/#audit-event-actions--details--and-results). To limit which events Atlas should record, you can specify event audit filters. The following document defines an audit filter that restricts audits to only the authentication operations that occur against the `test` database. To learn more, see [Configure Audit Filters](/tutorial/configure-audit-filters/).

```
{ "atype": "authenticate", "param.db": "test" }
```

To enable an audit filter, run the atlas auditing update command with the `--enabled` flag and specify the audit filter document in single quotes to pass the document as a string:

```
atlas auditing update --enabled --auditFilter '{"atype": "authenticate", "param.db": "test"}'
```

Custom audit filters let you forgo the managed {atlas-ui+} auditing filter builder in favor of hand-tailored granular control of event auditing. Atlas checks only that the custom filter uses valid JSON syntax, and doesn't validate or test the filter's functionality. The audit filter document:

- Must resolve to a query that matches one or more fields in the audit event message.
- Can use combinations of query operators and equality conditions to match the desired audit messages.

Atlas supports specifying a JSON-formatted audit filter for customizing MongoDB auditing. Configuring a JSON configuration file for your custom filter is also useful because if you need to create a large filter, it is easier to manage and maintain the filter if you store the filter's configuration in a separate file. The following Atlas CLI command enables an audit filter that is defined in the specified JSON (Javascript Object Notation) configuration file:

```
atlas auditing update --enabled -f filter.json
```

## Recommendations for Atlas Auditing

The following recommendations apply to all deployment paradigms. We recommend that you [set up database auditing](/database-auditing) when you provision your clusters. Database auditing increases cluster resource usage and operational expenses.

- To maintain optimal performance and control costs, consider auditing only essential users and turning off auditing in development environments where it's not required.
- Certain industries, such as healthcare and financial services, may opt to keep auditing enabled in development environments for compliance reasons.

We recommend that you audit the following events at a minimum:

- Failed logon
- Session activity
- Logon and logoff
- Attempts to perform unauthorized functions
- Password change
- Database User Access changes
- DDL & System configuration stored procedures
- Modification of Native audit
- Running a backup or restore operation
- Altering DBMS native audit settings
- Altering security
- Running database start and stop commands

## Automation Examples: Atlas Auditing

> **Tip:**
> For Terraform examples that enforce our recommendations across all pillars, see one of the following examples in GitHub:
>
> - [Staging/Prod](https://github.com/mongodb/docs/blob/main/content/atlas-architecture/current/source/includes/examples/tf-staging-prod-complete/)
> - [Dev/Test](https://github.com//mongodb/docs/blob/main/content/atlas-architecture/current/source/includes/examples/tf-dev-test-complete/)

The following examples show how to retrieve and download logs and configure auditing using Atlas tools for automation.

**CLI**

### Update Audit Configuration

To update your project's audit configuration, use the atlas auditing update command and specify the new audit filter. The following command replaces the existing audit filter configuration with a new filter that audits all authentication events for known users in the project:

```
atlas auditing update --enabled --auditFilter '{"atype": "authenticate"}'
```

### Describe Audit Configuration

Run the atlas auditing describe command to return the auditing configuration for the specified project:

```
atlas auditing describe --output json
```

---

**Terraform**

The following example demonstrates how to enable auditing for your deployment. Before you can create resources with Terraform, you must:

- Create your paying organization and create an API key for the paying organization. Store your public and private keys as environment variables by running the following commands in the terminal:

   ```
   export MONGODB_ATLAS_PUBLIC_KEY="<insert your public key here>"
   export MONGODB_ATLAS_PRIVATE_KEY="<insert your private key here>"
   ```

- [Install Terraform](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli).

> **Important:**
> The following examples use MongoDB Atlas Terraform Provider version 2.x (`~> 2.2`). If you're upgrading from provider version 1.x, see the [2.0.0 Upgrade Guide](https://registry.terraform.io/providers/mongodb/mongodbatlas/latest/docs/guides/2.0.0-upgrade-guide) for breaking changes and migration steps. The examples use the `mongodbatlas_advanced_cluster` resource with v2.x syntax.
>

### Enable Auditing and Create an Auditing Filter

You can [configure manual auditing](/core/auditing) of most of the documented [system event actions](/reference/audit-message/mongo/) by creating audit filters. To learn more about configuring audit filters, see [Configure Audit Filters](/tutorial/configure-audit-filters/). The following Terraform script sets up a complete MongoDB Atlas infrastructure with comprehensive database auditing. The script:

- Creates a new project within an organization and provisions a three-node replica set cluster (M10 instances) deployed on AWS in the US East region.
- Enables detailed auditing capabilities using a JSON filter that monitors:

   - authentication events
   - user management operations
   - various database commands, including data manipulation and query operations

The auditing configuration specifically targets admin and external database users, while tracking critical operations, such as authentication attempts, role modifications, and common database commands, such as `find`, `insert`, `update`, and `delete` operations.

```shell
# Add the MongoDB Atlas Provider  
terraform {    
  required_providers {    
    mongodbatlas = {    
      source  = "mongodb/mongodbatlas"    
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

# Create a Project  
resource "mongodbatlas_project" "this" {    
  org_id = var.atlas_org_id    
  name   = var.atlas_project_name    
}  

# Create an Atlas Advanced Cluster  
resource "mongodbatlas_advanced_cluster" "atlas-cluster" {    
  project_id             = mongodbatlas_project.atlas-project.id    
  name                   = "ClusterPortalProd"    
  cluster_type           = "REPLICASET"    
  mongo_db_major_version = "8.0"    

  replication_specs = [
    {
      region_configs = [
        {    
          electable_specs = {    
            instance_size = "M10"    
            node_count    = 1    
          }    
          provider_name = "AWS"    
          priority      = 7    
          region_name   = "US_WEST_1"    
        }    
      ]
    }
  ]    

  # Advanced configuration  
  backup_enabled         = true    
  pit_enabled            = true    
  version_release_system = "LTS"    
}  

# Create comprehensive auditing configuration to capture all possible audit events  
resource "mongodbatlas_auditing" "atlas-auditing" {    
  project_id = mongodbatlas_project.atlas-project.id    

  # Comprehensive audit filter to capture all possible audit events  
  audit_filter = jsonencode({    
    "$or" = [    
      # Capture all authentication events    
      {    
        "atype" = {    
          "$in" = [    
            "authenticate",    
            "authCheck",    
            "logout"    
          ]    
        }    
      },    
      # Capture all authorization events    
      {    
        "atype" = "authCheck"    
      },    
      # Capture all CRUD operations    
      {    
        "atype" = "authCheck",    
        "param.command" = {    
          "$in" = [    
            # Read operations    
            "find",    
            "getMore",    
            "count",    
            "distinct",    
            "aggregate",    
            "group",    
            "mapReduce",    
            "geoNear",    
            "geoSearch",    
            "parallelCollectionScan",    
            "eval",    
            "getLastError",    
            "getPrevError",    
            "resetError",    
            # Write operations    
            "insert",    
            "update",    
            "delete",    
            "findAndModify",    
            "save",    
            # Index operations    
            "createIndexes",    
            "dropIndexes",    
            "listIndexes",    
            "reIndex",    
            # Collection operations    
            "create",    
            "drop",    
            "listCollections",    
            "collMod",    
            "convertToCapped",    
            "emptycapped",    
            "renameCollection",    
            # Database operations    
            "dropDatabase",    
            "listDatabases",    
            "copydb",    
            "clone",    
            # GridFS operations    
            "filemd5"    
          ]    
        }    
      },    
      # Capture all DDL (Data Definition Language) operations    
      {    
        "atype" = "authCheck",    
        "param.command" = {    
          "$in" = [    
            "create",    
            "drop",    
            "createIndexes",    
            "dropIndexes",    
            "collMod",    
            "renameCollection",    
            "dropDatabase",    
            "createCollection",    
            "dropCollection"    
          ]    
        }    
      },    
      # Capture all user and role management operations    
      {    
        "atype" = "authCheck",    
        "param.command" = {    
          "$in" = [    
            "createUser",    
            "dropUser",    
            "dropAllUsersFromDatabase",    
            "updateUser",    
            "grantRolesToUser",    
            "revokeRolesFromUser",    
            "createRole",    
            "updateRole",    
            "dropRole",    
            "dropAllRolesFromDatabase",    
            "grantRolesToRole",    
            "revokeRolesFromRole",    
            "grantPrivilegesToRole",    
            "revokePrivilegesFromRole"    
          ]
        }    
      },
      # Capture replica set operations    
      {    
        "atype" = "authCheck",    
        "param.command" = {    
          "$in" = [    
            "replSetGetStatus",    
            "replSetInitiate",    
            "replSetReconfig",    
            "replSetStepDown",    
            "replSetSyncFrom",    
            "replSetFreeze",    
            "replSetMaintenance",    
            "replSetGetConfig"    
          ]    
        }    
      },    
      # Capture sharding operations    
      {    
        "atype" = "authCheck",    
        "param.command" = {    
          "$in" = [    
            "shardCollection",    
            "addShard",    
            "removeShard",    
            "movePrimary",    
            "enableSharding",    
            "split",    
            "moveChunk",    
            "mergeChunks"    
          ]    
        }    
      },    
      # Capture administrative operations    
      {    
        "atype" = "authCheck",    
        "param.command" = {    
          "$in" = [    
            "shutdown",    
            "fsync",    
            "getParameter",    
            "setParameter",    
            "serverStatus",    
            "dbStats",    
            "collStats",    
            "currentOp",    
            "killOp",    
            "listCommands",    
            "buildInfo",    
            "hostInfo",    
            "connectionStatus",    
            "getCmdLineOpts",    
            "logRotate",    
            "planCacheClear",    
            "planCacheListFilters",    
            "planCacheSetFilter",    
            "planCacheClearFilters"    
          ]    
        }    
      },    
      # Capture diagnostic operations    
      {    
        "atype" = "authCheck",    
        "param.command" = {    
          "$in" = [    
            "explain",    
            "profile",    
            "validate",    
            "dbHash",    
            "ping",    
            "ismaster",    
            "isMaster",    
            "hello"    
          ]    
        }    
      },    
      # Capture connection and session events    
      {    
        "atype" = {    
          "$in" = [    
            "createSession",    
            "endSession",    
            "refreshSession"    
          ]    
        }    
      },    
      # Capture transaction events    
      {    
        "atype" = "authCheck",    
        "param.command" = {    
          "$in" = [    
            "abortTransaction",    
            "commitTransaction",    
            "startTransaction"    
          ]    
        }    
      },    
      # Capture change stream events    
      {    
        "atype" = "authCheck",    
        "param.command" = {    
          "$in" = [    
            "aggregate"    
          ]    
        },    
        "param.pipeline" = {    
          "$elemMatch" = {    
            "$changeStream" = {    
              "$exists" = true    
            }    
          }    
        }    
      }    
    ]    
  })    

  # Enable comprehensive auditing settings  
  audit_authorization_success = true # Audit both successful and failed operations    
  enabled                     = true # Enable auditing    
}  

# Variables  
variable "mongodbatlas_public_key" {    
  default     = ""    
  description = "MongoDB Atlas Public Key"    
  type        = string    
  sensitive   = true    
}  

variable "mongodbatlas_private_key" {    
  default     = ""    
  description = "MongoDB Atlas Private Key"    
  type        = string    
  sensitive   = true    
}  

variable "atlas_org_id" {    
  default     = ""    
  description = "MongoDB Atlas Organization ID"    
  type        = string    
}  

variable "atlas_project_name" {    
  description = "MongoDB Atlas Project Name"    
  type        = string    
  default     = "Atlas Auditing Example"    
}  

# Outputs  
output "cluster_connection_string" {    
  description = "Connection string for the Atlas cluster"    
  value       = mongodbatlas_advanced_cluster.atlas-cluster.connection_strings[0].standard_srv    
  sensitive   = true    
}  

output "cluster_id" {    
  description = "Atlas cluster ID"    
  value       = mongodbatlas_advanced_cluster.atlas-cluster.cluster_id    
}  

output "project_id" {    
  description = "Atlas project ID"    
  value       = mongodbatlas_project.atlas-project.id    
}  

output "auditing_enabled" {    
  description = "Whether auditing is enabled"    
  value       = mongodbatlas_auditing.atlas-auditing.enabled    
}  
```

---
