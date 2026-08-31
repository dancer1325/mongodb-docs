# Guidance for Atlas Network Security

Atlas provides secure network configuration defaults for your database deployments, such as:

- Mandatory TLS (Transport Layer Security)/SSL (Secure Sockets Layer) connection encryption
- VPC (Virtual Private Cloud)\s for all projects with one-or-more Dedicated clusters
- Authentication that uses IP access lists and only accepts database connections from sources you explicitly declare

You can further configure these protections to meet your unique security needs and preferences. Use the recommendations on this page to plan for the network security configuration of your clusters.

## Features for Atlas Network Security

Atlas enforces TLS (Transport Layer Security)/SSL (Secure Sockets Layer) encryption for all connections to your databases. We recommend using M10+ dedicated clusters, because all Atlas projects with one or more M10+ dedicated clusters receive their own dedicated:

- vpc| on AWS (Amazon Web Services) or Google Cloud.
- vnet| on Azure (Microsoft Azure).

Atlas deploys all dedicated clusters inside this VPC (Virtual Private Cloud) or VNet (Virtual Network). By default, all access to your clusters is blocked. You must explicitly allow an inbound connection by one of the following methods:

- Add private endpoints, which Atlas adds automatically to your IP access list. No other access is automatically added.
- Use VPC (Virtual Private Cloud) or VNet (Virtual Network) peering to add private IP addresses.
- Add public IP addresses to your IP access list.

You can also use multiple methods together for added security.

### TLS (Transport Layer Security)

Atlas enforces mandatory TLS (Transport Layer Security) encryption of connections to your databases. TLS (Transport Layer Security) 1.2 is the default protocol. To learn more, see the Set Minimum TLS Protocol Version section of Configure Additional Settings.

### IP access lists

As an Atlas administrator, you can: Configure IP access lists to limit which IP addresses can attempt authentication to your database. Allow access only from the IP addresses and CIDR (Classless Inter-Domain Routing) block IP ranges that you add to your IP access list. We recommend that you permit access to the smallest network segments possible, such as an individual `/32` address. Deny application servers and other clients access to your Atlas clusters if their IP addresses aren't included in your IP access list. Configure [temporary access list entries](/security/ip-access-list/#add-ip-access-list-entries) that expire automatically after a user-defined period.

### Firewall Configuration

When connecting from your client application servers to Atlas and passing through a firewall that blocks outbound network connections, you must also configure your firewall to allow your applications to make outbound connections to TCP (Transmission Control Protocol) traffic on Atlas hosts. This grants your applications access to your clusters. Atlas cluster public IPs remain the same in the majority of cases of cluster changes such as vertical scaling, [topology](/reference/glossary/#std-term-topology) changes, or maintenance events. However, certain topology changes, such as a conversion from replica set to sharded cluster, the addition of shards, or a region change require that you use new IP addresses. In the case of converting from a replica set to a sharded cluster, the failure to reconnect the application clients might cause your application to suffer from data outages. If you use a DNS (Domain Name System) seed list connection string, your application automatically connects to the `mongos` for your sharded cluster. If you use a standard connection string, you must update your connection string to reflect your new cluster topology. In the case of adding new shards, the failure to reconnect the application clients may cause your application to suffer from a data outage.

### Private Endpoints

A private endpoint facilitates a one-way connection from a VPC (Virtual Private Cloud), that you manage directly, to your Atlas VPC (Virtual Private Cloud), without permitting Atlas to initiate a reciprocal connection. This allows you to make use of secure connections to Atlas without extending your network trust boundary. The following private endpoints are available:

- AWS (Amazon Web Services) [PrivateLink](/vpc/latest/userguide/endpoint-services-overview.html), for connections from AWS (Amazon Web Services) VPC (Virtual Private Cloud)\s
- Microsoft Azure [Private Link](/private-link/private-link-overview), for connections from Microsoft Azure VNets
- [Private Service Connect](/vpc/docs/private-service-connect), for connections from Google Cloud VPC (Virtual Private Cloud)\s

!["An image representing how MongoDB Atlas private endpoints work."](/includes/images/private-link.svg)

### VPC/VNet Peering

Network peering allows you to connect your own VPC (Virtual Private Cloud)\s with an Atlas VPC (Virtual Private Cloud) to route traffic privately and isolate your data flow from the public internet. Atlas maps VPC (Virtual Private Cloud)\s one-to-one to Atlas projects. Most operations performed over a VPC (Virtual Private Cloud) connection originate from your application environment, minimizing the need for Atlas to make outbound access requests to peer VPC (Virtual Private Cloud)\s. However, if you configure Atlas to use LDAP (Lightweight Directory Access Protocol) authentication, you must enable Atlas to connect outbound to the authentication endpoint of your peer VPC (Virtual Private Cloud) over the LDAP (Lightweight Directory Access Protocol) protocol. Note that LDAP (Lightweight Directory Access Protocol) authentication is deprecated on Atlas with 8.0. We recommend that you use [Workforce Identity Federation](https://www.mongodb.com/docs/manual/core/oidc/workforce/) and [Workload Identity Federation](https://www.mongodb.com/docs/manual/core/oidc/workload/) instead. You can choose your Atlas CIDR (Classless Inter-Domain Routing) block with the VPC (Virtual Private Cloud) peering wizard before you deploy your first cluster. The Atlas VPC (Virtual Private Cloud) CIDR (Classless Inter-Domain Routing) block must not overlap with the CIDR (Classless Inter-Domain Routing) block of any VPC (Virtual Private Cloud) you intend to peer to. Atlas limits the number of MongoDB instances per VPC (Virtual Private Cloud) based on the CIDR (Classless Inter-Domain Routing) block. For example, a project with a CIDR (Classless Inter-Domain Routing) block of `/24` is limited to the equivalent of 27 3-node replica sets.

!["An image representing how MongoDB Atlas VPC/VNet peering works."](/includes/images/vpc-vnet-peering.svg)

## Recommendations for Atlas Network Security

<details>
<summary>Single-Region Deployment Recommendations</summary>

Single-region deployments have no unique considerations for Atlas network security.
</details>

<details>
<summary>Multi-Region and Multi-Cloud Deployment Recommendations</summary>

Multi-region deployments secured with private endpoints have the following unique considerations:

- For global private endpoints, Atlas automatically generates a SRV (DNS Service Record) record that points to all Atlas cluster nodes. The MongoDB driver attempts to connect to each SRV (DNS Service Record) record from your application. This allows the driver to handle a failover event without waiting for DNS (Domain Name System) replication and without requiring you to update the driver's connection string.
- In order to facilitate automatic SRV (DNS Service Record) record generation for all nodes in your Atlas cluster, you must establish a VPC (Virtual Private Cloud) peering connection between your application VPC (Virtual Private Cloud)\s, and you must connect your application VPC (Virtual Private Cloud)\s to your MongoDB VPC (Virtual Private Cloud) using PrivateLink or equivalent.
- You must enable private endpoints in every region that you have an Atlas cluster deployed.
- Google Cloud Private Service Connect is region-specific. However, you can configure [global access](/vpc/docs/about-accessing-vpc-hosted-services-endpoints#global-access) to access private endpoints from a different region. To learn more, see Multi-Region Support.
</details>

### All Deployment Paradigm Recommendations

The following recommendations apply to all deployment paradigms.

#### Private Endpoints

We recommend that you set up private endpoints for all new staging and production projects to limit the extension of your network trust boundary. In general, we recommend using private endpoints for every Atlas project, because this approach provides the most granular security and eases the administrative burden that can come from managing IP access list\s and large blocks of IP addresses as your cloud network scales. There is a cost associated with each endpoint. So, you may not need private endpoints in lower environments, but you should leverage them in higher environments to limit the extension of your network trust boundary. If VPCs or VNets in which your application is deployed can't be peered with one another, potentially due to a combination of on-premises and cloud deployments, you might want to consider a regional private endpoint. With regional private endpoints, you can do the following:

- Connect a single private endpoint to multiple VNets or VPCs without peering them directly to each other.
- Mitigate partial region failures in which one or more services within a region fails.

To network with regional endpoints, you must do the following:

- Perform regular and robust health checks to help validate successful connectivity and operations between cluster and application. You can either ping your cluster with the `db.runCommand("ping")` command to confirm connectivity quickly, or you can run `rs.conf()` to get detailed information about each node in your cluster.
- Use a distinct connection string for each region.
- Use cross-region routing to Atlas to maintain availability in case of an Atlas VPC (Virtual Private Cloud) disconnection.

To learn more about private endpoints in Atlas, including limitations and considerations, see [Learn About Private Endpoints in Atlas](/security-private-endpoint/). To learn how to set up private endpoints for your clusters, see [Set Up a Private Endpoint for a Dedicated Cluster](/security-cluster-private-endpoint/).

#### Cloud Provider-Specific Guidance

- aws|: We recommend VPC (Virtual Private Cloud) peering across all of your self-managed VPCs that
- azure|: We recommend VNet peering across all of your self-managed VNets that
- gcp|: Peering is not required across your self-managed VPCs when using

#### GCP (Google Cloud Platform) Private Endpoints Considerations and Limitations

Atlas services are accessed through GCP Private Service Connect endpoints on ports 27015 through 27017. The ports can change under specific circumstances, including (but not limited to) cluster changes.

- GCP Private Service Connect must be active in all regions into which you deploy a multi-region cluster. You will receive an error if GCP Private Service Connect is active in some, but not all, targeted regions.
- In order to maintain a manageable number of internally stored connection strings that allow a driver to connect to all nodes in a multi-region cluster, which is required in order to assure the driver is connected to a dynamically assigned primary node within the cluster and can thus perform all operations against the database, you can do only one of the following:

   - Deploy nodes in more than one region, and have one private endpoint per region.
   - Have multiple private endpoints in one region, and no other private endpoints.

      > **Important:**
      > This limitation applies across cloud providers. For example, if you create more than one private endpoint in a single region in GCP (Google Cloud Platform), you can't create private endpoints in AWS (Amazon Web Services) or any other GCP (Google Cloud Platform) region.

   To learn more, see Why are regionalized endpoints only available for sharded clusters?. In sharded clusters, inter-node traffic is routed through mongos processes, which are connected to all nodes within the cluster by default. As such, you can create any number of private endpoints in a given region. See (Optional) Regionalized Private Endpoints for Multi-Region Sharded Clusters to learn more.
- service| creates 50 service attachments, each with a
- You can have up to 50 nodes when you create Atlas projects that use GCP Private Service Connect in a **single region**. If you need to change the number of nodes, perform one of the following actions:

   - Remove existing private endpoints and then change the limit using the Set One Project Limit Atlas Administration API endpoint.
   - Contact MongoDB Support.
   - Use additional projects or regions to connect to nodes beyond this limit.

> **Important:**
> - Each private endpoint in GCP (Google Cloud Platform) reserves an IP address within your GCP (Google Cloud Platform) VPC (Virtual Private Cloud) and forwards traffic from the endpoints' IP addresses to the [service attachments](/vpc/docs/private-service-connect#service-attachments). You must create an equal number of private endpoints to the number of service attachments. The number of service attachments defaults to 50.
>
> Addressable targets include:
>
> - Each `mongod` instance in a replica set deployment (sharded clusters excluded).
> - Each `mongos` instance in a sharded cluster deployment.
> - Each BI Connector for Atlas instance across all dedicated clusters in the project.

- You can have up to 40 nodes when you create Atlas projects that use GCP Private Service Connect across **multiple regions**. This total excludes the following instances:

   - gcp| regions communicating with each other
   - Free clusters or Shared clusters
- gcp| Private Service Connect supports up to 1024 outgoing
- gcp| Private Service Connect is region-specific. However, you

#### IP Access Lists

We recommend that you configure an IP access list for your API keys and programmatic access to allow access only from trusted IP addresses such as your CI/CD pipeline or orchestration system. These IP access list\s are set on the Atlas control plane upon provisioning a service account and are separate from IP access list\s which can be set on the Atlas project data plane for connections to the clusters. When you configure your IP access list, we recommend that you:

- Use temporary access list entries in situations where team members require access to your environment from temporary work locations or during break-glass scenarios where production access to humans is required to resolve a production-down scenario. We recommend that you build an automation script to quickly add temporary access to prepare for these incidents.
- Define IP access list entries covering the smallest network segments possible. To do this, favor individual IP addresses where possible, and avoid large CIDR (Classless Inter-Domain Routing) blocks.

#### VPC/VNet Peering

If you configure VPC (Virtual Private Cloud) or VNet peering, we recommend that you:

- To maintain tight network trust boundaries, configure security groups and [network ACLs](/vpc/latest/userguide/vpc-network-acls.html) to prevent inbound access to systems inside your application VPC (Virtual Private Cloud)\s from the Atlas-side VPC (Virtual Private Cloud).
- Create new VPC (Virtual Private Cloud)\s to act as intermediaries between sensitive application infrastructure and your Atlas VPC (Virtual Private Cloud)\s. VPC (Virtual Private Cloud)\s are intransitive, allowing you to only expose those components of your application that need access to Atlas.

## Automation Examples: Atlas Network Security

The following examples configure connections between your application environment and your Atlas clusters using IP access lists, VPC (Virtual Private Cloud) Peering, and Private Endpoints. These examples also apply other recommended configurations, including:

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
> Before you can configure connections with the Atlas CLI, you must:
>
> - [Create your paying organization](/billing/#configure-a-paying-organization) and [create an API key](/configure-api-access/) for the paying organization.
> - [Install the Atlas CLI](/install-atlas-cli/)
> - [Connect from the Atlas CLI](/connect-atlas-cli/) using the steps for Programmatic Use.

### Create an IP access list Entry

Run the following command for each connection you want to allow. Change the entries to use the appropriate options and your actual values:

```
atlas accessList create 192.0.2.15 --type ipAddress --projectId 5e2211c17a3e5a48f5497de3 --comment "IP address for app server 2" --output json
```

For more configuration options and information about this example, see atlas-accessLists-create. For information on how to create an IP access list entry with AWS (Amazon Web Services), GCP (Google Cloud Platform) and Azure (Microsoft Azure), see [Set Up a Private Endpoint for a Dedicated Cluster](/security-cluster-private-endpoint/#follow-these-steps)

### Create a VPC Peering Connection

Run the following code for each VPC (Virtual Private Cloud) you want to peer to your Atlas VPC (Virtual Private Cloud). Replace `aws` with `azure` or `gcp` as appropriate, and change the options and values to the appropriate ones for your VPC (Virtual Private Cloud) or VNet:

```
```

---

atlas networking peering create aws --accountId 854333054055 --atlasCidrBlock 192.168.0.0/24 --region us-east-1 --routeTableCidrBlock 10.0.0.0/24 --vpcId vpc-078ac381aa90e1e63 For more configuration options and information about this example, see: - atlas-networking-peering-create-aws, for AWS (Amazon Web Services) VPC (Virtual Private Cloud)\s - atlas-networking-peering-create-azure, for Microsoft Azure VNets - atlas-networking-peering-create-gcp, for Google Cloud VPC (Virtual Private Cloud)\s Create a Private Endpoint ~~~~~~~~~~~~~~~~~~~~~~~~~ Run the following command for each private endpoint you want to create. Replace `aws` with `azure` or `gcp` as appropriate, and change the options and values to the appropriate ones for your VPC (Virtual Private Cloud) or VNet: .. code-block:: :copyable: true atlas privateEndpoints aws create --region us-east-1 --projectId 5e2211c17a3e5a48f5497de3 --output json For more configuration options and information about this example, see: - atlas-privateEndpoints-aws-create, for connections from AWS (Amazon Web Services) VPC (Virtual Private Cloud)\s - atlas-privateEndpoints-azure-create, for connections from Microsoft Azure VNets - atlas-privateEndpoints-gcp-create, for connections from GCP Private Service Connect .. tab:: Terraform :tabid: Terraform .. note:: Before you can create resources with Terraform, you must: - [Create your paying organization](/billing/#configure-a-paying-organization) and [create an API key](/configure-api-access/) for the paying organization. Store your API key as environment variables by running the following command in the terminal: .. code-block:: export MONGODB_ATLAS_PUBLIC_KEY="<insert your public key here>" export MONGODB_ATLAS_PRIVATE_KEY="<insert your private key here>" - [Install Terraform](https://developer.hashicorp.com/terraform/tutorials/aws-get-started/install-cli) We also suggest [creating a workspace for your enviornment](https://developer.hashicorp.com/terraform/cloud-docs/recommended-practices/part1#one-workspace-per-environment-per-terraform-configuration). Create an IP access list Entry ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ To add an entry to your IP access list, create the following file and place it in the directory of the project you want to grant access to. Change the IDs and names to use your values: accessEntryForAddress1.tf ````````````````````````````` .. code-block:: terraform # Add an entry to your IP Access List resource "mongodbatlas_access_list_api_key" "address_1" { org_id = "<org-id>" ip_address = "2.3.4.5" api_key_id = "a29120e123cd" } After you create the files, navigate to your project directory and run the following command to initialize Terraform: .. code-block:: terraform init Run the following command to view the Terraform plan: .. code-block:: terraform plan Run the following command to add one entry to the IP access list for your project. The command uses the file and the [MongoDB & HashiCorp Terraform](https://www.mongodb.com/atlas/hashicorp-terraform) to add the entry. .. code-block:: terraform apply When prompted, type `yes` and press Enter to apply the configuration. Create a VPC Peering Connection ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~ To create a peering connection between your application VPC (Virtual Private Cloud) and your Atlas VPC (Virtual Private Cloud), create the following file and place it in the directory of the project you want to grant access to. Change the IDs and names to use your values: vpcConnection.tf ```````````````` .. code-block:: terraform # Define your application VPC resource "aws_default_vpc" "default" { tags = { Name = "Default VPC" } } # Create the peering connection request resource "mongodbatlas_network_peering" "mongo_peer" { accepter_region_name   = "us-east-2" project_id             = local.project_id container_id           = one(values(mongodbatlas_advanced_cluster.test.container_id)) provider_name          = "AWS" route_table_cidr_block = "172.31.0.0/16" vpc_id                 = aws_default_vpc.default.id aws_account_id         = local.AWS_ACCOUNT_ID } # Accept the connection resource "aws_vpc_peering_connection_accepter" "aws_peer" { vpc_peering_connection_id = mongodbatlas_network_peering.mongo_peer.connection_id auto_accept               = true tags = { Side = "Accepter" } } After you create the file, navigate to your project directory and run the following command to initialize Terraform: .. code-block:: terraform init Run the following command to view the Terraform plan: .. code-block:: terraform plan Run the following command to add a VPC (Virtual Private Cloud) peering connection from your application to your project. The command uses the file and the [MongoDB & HashiCorp Terraform](https://www.mongodb.com/atlas/hashicorp-terraform) to add the entry. .. code-block:: terraform apply When prompted, type `yes` and press Enter to apply the configuration. Create a Private Link ~~~~~~~~~~~~~~~~~~~~~ To create a PrivateLink from your application VPC (Virtual Private Cloud) to your Atlas VPC (Virtual Private Cloud), create the following file and place it in the directory of the project you want to connect to. Change the IDs and names to use your values: privateLink.tf `````````````` .. code-block:: terraform resource "mongodbatlas_privatelink_endpoint" "test" { project_id    = "<project-id>" provider_name = "AWS/AZURE" region        = "US_EAST_1" timeouts { create = "30m" delete = "20m" } } After you create the file, navigate to your project directory and run the following command to initialize Terraform: .. code-block:: terraform init Run the following command to view the Terraform plan: .. code-block:: terraform plan Run the following command to add a PrivateLink endpoint from your application to your project. The command uses the file and the [MongoDB & HashiCorp Terraform](https://www.mongodb.com/atlas/hashicorp-terraform) to add the entry. .. code-block:: terraform apply When prompted, type `yes` and press Enter to apply the configuration.
