# Guidance for Atlas Automated Infrastructure Provisioning

MongoDB Atlas provides tools that enable programmatic management of the deployment, scaling, and maintenance of your Atlas clusters. Atlas provides the flexibility to implement Infrastructure as Code (IaC) using either imperative or declarative programming. For example, developers can write imperative scripts that call functions from our Atlas Go SDK (Software Development Kit) client, or manage Atlas resources using declarative IaC (Infrastructure as Code) tools like the Atlas Kubernetes Operator, Terraform, AWS (Amazon Web Services) CloudFormation, or the AWS (Amazon Web Services) CDK (Cloud Development Kit). Atlas's IaC (Infrastructure as Code) tools are especially useful at the enterprise scale. We recommend that our enterprise customers use IaC (Infrastructure as Code) tools for the following benefits:

- **Consistency**: IaC (Infrastructure as Code) tools enable repeatability across environments, so that deployments generate consistent results.
- **Scalability**: IaC (Infrastructure as Code) tools enable auto-scaling to automatically adjust the tier or storage capacity of clusters in response to real-time use.
- **Reduced Human Error**: IaC (Infrastructure as Code) tools automate operational overhead, reducing manual interventions that produce common human errors.
- **Faster Development**: IaC (Infrastructure as Code) tools streamline operations to promote more efficient development.
- **Improved Change Management**: IaC (Infrastructure as Code) tools support reviews and standardization of infrastructure, allowing for better change management practices and compliance.

## Features for Atlas Automation

You can automate the configuration, provisioning, and management of Atlas building blocks like database users and roles, and Atlas clusters, projects, and organizations. You can also automate various configuration and management tasks for cluster resources, including enabling auto-scaling compute and storage, creating and updating multi-cloud clusters, monitoring cluster performance and health, automating backups and restores, defining backup policies, and more. You can align your choice of tools with your preferred workflow to ensure seamless integration of MongoDB Atlas into your existing processes. The following MongoDB Atlas tools allow you to easily deploy and manage Atlas at scale with repeatable, accurate, and scalable processes.

### Atlas Administration API

The Atlas Administration API provides a REST (Representational State Transfer)\ful interface that allows you to leverage your preferred client such as cURL or Postman to directly interact with API (Application Programming Interface) endpoints that correspond to Atlas resources. They can also be directly called in your favorite programming language or bash script. To learn more, see atlas-admin-api-access.

### Atlas CLI

Enables you to manually or programmatically create, manage, and automate tasks related to Atlas resources from a unified command line tool. To learn more, see the following resources:

- Atlas CLI
- Quick Start
- atlas-cli-ephemeral-cluster

You can also use the Atlas CLI examples in the Atlas Architecture Center such as the Org, Project, and Cluster examples to get started.

### HashiCorp Terraform MongoDB Atlas Provider

Provisions Atlas resources across cloud providers (AWS (Amazon Web Services), Azure (Microsoft Azure), GCP (Google Cloud Platform)) in the workflow of your choice. It allows you to integrate Atlas into your continuous delivery workflows with the official plugin. Alternatively, you can use the CDKTF (Cloud Development Kit for Terraform) to deploy Atlas in preferred languages such as JavaScript, TypeScript, Python, Java, C#, and Go. To learn more, see getting-started-terraform and the [MongoDB Atlas Provider Terraform docs](https://registry.terraform.io/providers/mongodb/mongodbatlas/latest/docs). You can also use the Terraform examples in the Atlas Architecture Center such as the Org, Project, and Cluster examples to get started.

> **Tip:**
> For Terraform examples that enforce our recommendations across all pillars, see one of the following examples in GitHub:
>
> - [Staging/Prod](https://github.com/mongodb/docs/blob/main/content/atlas-architecture/current/source/includes/examples/tf-staging-prod-complete/)
> - [Dev/Test](https://github.com//mongodb/docs/blob/main/content/atlas-architecture/current/source/includes/examples/tf-dev-test-complete/)

### Atlas Go SDK (Software Development Kit) Client

The Atlas Go SDK (Software Development Kit) client simplifies interaction with the Atlas Administration API by providing pre-built functions and full API (Application Programming Interface) endpoint coverage. The SDK provides platform-specific and GoLang language-specific tools, libraries, and documentation to help build applications quickly and easily. To learn more, see Atlas Go SDK.

See all of the Atlas Architecture Center Go SDK examples in a single project in the [Atlas Architecture Go SDK GitHub repo](https://github.com/mongodb/atlas-architecture-go-sdk).

### CloudFormation Resources

Resources to manage Atlas include:

- JSON and YAML templates allow you to leverage multiple different types of Atlas resources in the AWS (Amazon Web Services) CloudFormation Public Registry.
- [AWS Cloud Development Kit (CDK)](https://constructs.dev/search?q=&keywords=MongoDB&offset=0&tags=mongodb-published) defines infrastructure using familiar programming languages such as JavaScript, TypeScript, Python, Java, C#, and Go.

To learn more, see [Deploy MongoDB Atlas with AWS CloudFormation](https://www.mongodb.com/developer/products/atlas/deploy-mongodb-atlas-aws-cloudformation/).

### Atlas Kubernetes Operator

Allows you to deploy and manage Atlas resources using you existing Kubernetes tools. The Operator enables management of Atlas through custom resources applied into Kubernetes, which the Operator uses to configure Atlas. To learn more, see ak8so-quick-start-ref.

## Recommendations for Atlas Automation

The following recommendations apply to all deployment paradigms. If you already have an existing tool integrated into your deployment workflow that you use today, we recommend that you use that tool for automation. For example, if your developers and operations team are already deploying to Kubernetes, apply Atlas configurations through the same tooling and pipelines and use the Atlas Kubernetes Operator to automate updating Atlas. If you don't already have an existing tool integrated into your development workflow, we recommend an IaC tool because they provide more robust options for infrastructure provisioning and state management. You can also use a combination of multiple tools. For example, use IaC (Infrastructure as Code) tools for provisioning and state management, and leverage the Atlas Administration API, Atlas Go SDK (Software Development Kit), and Atlas CLI for quick administrative tasks that are ephemeral in nature. The Atlas CLI is great for local development as well as integration into a suite of tests as part of your CI/CD pipeline for application development because it improves response times and reduces costs.
