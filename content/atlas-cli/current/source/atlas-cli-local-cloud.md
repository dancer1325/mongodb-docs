# Manage Local and Cloud Deployments from the Atlas CLI

Use the Atlas CLI to work with Atlas, including using Atlas Search and Atlas Vector Search, throughout the entire software development lifecycle from your local environment to the cloud. With the `atlas deployments` commands, you can perform the following actions:

- Create and manage local Atlas deployments. Local Atlas deployments reside on your computer and provide a non-production environment for development.
- Create and manage Atlas cloud deployments. Cloud Atlas deployments reside in the cloud for non-production and production use.
- Manage Atlas Search and Atlas Vector Search indexes for your local and cloud Atlas deployments.

> **Note:**
> For local Atlas deployments, you can manage only deployments that you created with the `atlas deployments` command. If you created a local Atlas deployment without using the Atlas CLI, you can't manage that deployment with the Atlas CLI.

You can use only `atlas deployments` Atlas CLI commands with local Atlas deployments. You can use all Atlas CLI commands, including `atlas deployments` commands,  with cloud Atlas deployments.

## Supported Actions

You can perform the following actions with the `atlas deployments` command  for local and cloud Atlas deployments including, but not limited to:

- Set up a new Atlas deployment.
- List Atlas deployments.
- Connect with `mongosh`, [Compass](/), [MongoDB for VS Code](https://www.mongodb.com/docs/mongodb-vscode/install/) (versions 1.13.0+), or return the connection string for an Atlas deployment.
- Pause or start an Atlas deployment.
- Delete an Atlas deployment.
- Create or manage Atlas Search indexes to run a full-text search on an Atlas deployment.
- Create or manage Atlas Vector Search indexes to run a semantic search on an Atlas deployment.

To learn all of the actions that the `atlas deployments` command supports, see atlas-deployments.

## Tutorials

Use the following resources for step-by-step guidance to run `atlas deployments` commands:

| Tutorial | Objective |
| --- | --- |
| atlas-cli-deploy-local | Use the `atlas deployments` command to create a local Atlas deployment. This tutorial deploys a single-node replica set on your local computer. |
| atlas-cli-deploy-docker | Use the `atlas deployments` command to create a local Atlas deployment with Docker. |
| atlas-cli-deploy-fts | Use the `atlas deployments search indexes create` command to manage Atlas Search indexes and work with Atlas Vector Search locally and in the cloud. |

- [Create a Local Deployment](/atlas-cli-deploy-local)
- [Deploy with Docker](/atlas-cli-deploy-docker)
- [Docker Compose Example](/atlas-cli-docker-compose)
- [Deploy from Private Registry](/atlas-cli-deploy-pvt-registry)
- [Use Atlas Search](/atlas-cli-deploy-fts)
