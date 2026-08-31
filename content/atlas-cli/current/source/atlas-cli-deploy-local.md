# Create a Local Atlas Deployment

This tutorial shows you how to use the `atlas deployments` command to create a local Atlas deployment. In this tutorial, we will deploy a single-node replica set on your local computer. You can then manage your deployment, and use Atlas Search and Atlas Vector Search.

## Supported OS for Local Atlas Deployments

| Operating System | Operating System Version | Architecture | Minimum CPU Cores | Minimum Free RAM (GB) |
| --- | --- | --- | --- | --- |
| MacOS | 13.2 and later | x86-64, ARM | 2 | 2 |
| Red Hat Enterprise Linux / CentOS | 8, 9 | x86-64, ARM | 2 | 2 |
| Ubuntu | 22.04, 24.04 | x86-64, ARM | 2 | 2 |
| Debian | 11, 12 | x86-64, ARM | 2 | 2 |
| Amazon Linux | 2023 | x86-64, ARM | 2 | 2 |
| Windows | 10, 11 | x86 | 2 | 2 |

## Complete the Prerequisites

Before you begin, complete the following prerequisites:

> **Important:**
> For compatibility information on each product in the dependencies list, see the product's installation documentation.

*[Contenido incluido desde: /includes/steps-atlas-cli-deploy-local-prereqs.rst]*

## Create a Local Atlas Deployment

Use the `atlas deployments` command to create a local Atlas deployment. You can run this command in the following ways:

- **Interactive Mode (Default)**: the command prompts you for the deployment settings and provides default values.
- **Interactive Mode (Custom)**: the command prompts you for the deployment settings and lets you provide custom values.
- **Non-Interactive Mode**: you run the command with the specified options. The command does not prompt you to provide further values. To learn all of the actions that `atlas deployments` supports, see atlas-deployments.

Click one of the following tabs to see the command for your preferred mode.

**Interactive (Default)**

1. **Run the `atlas deployments` command in interactive mode.**

   ```sh
   atlas deployments setup
   ```

   To initialize the local Atlas deployment with your own data and indexes:

   1. Copy the following command:

      ```sh
      atlas deployments setup --initdb {folder}
      ```

   2. Replace the `{folder}` placeholder with the directory that contains the `.js` and `.sh` files to run inside the local container in alphanumeric order.
   3. Run the command.

1. **Specify what to deploy.**

   **Example:** Specify `local - Local Database` and press Enter..

   **Input:**

   ```sh
   ? What would you like to deploy?  [Use arrows to move, type to filter, ? for more help]
   > local - Local Database
     atlas - Atlas Database
   ```

   **Output:**

   ```sh
   [Default Settings]
   Deployment Name   local50
   MongoDB Version   7.0
   Port              27017
   ```

1. **Specify how to set up your local Atlas database.**

   **Example:** Specify `default - With default settings` and press Enter..

   **Input:**

   ```sh
   ? How do you want to setup your local MongoDB database?  [Use arrows to move, type to filter]
   > default - With default settings
     custom - With custom settings
     cancel - Cancel set up
   ```

   **Output:**

   ```sh
   Creating your deployment local50 [this might take several minutes]
   1/4: Downloading and completing configuration...
   2/4: Starting your local environment...
   3/4: Downloading MongoDB binaries to your local environment...
   4/4: Creating your deployment local50...
   Deployment created!
   Connection string: mongodb://localhost:27017/?directConnection=true
   ```

---

**Interactive (Custom)**

1. **Run the `atlas deployments` command in interactive mode.**

   ```sh
   atlas deployments setup
   ```

   To initialize the local Atlas deployment with your own data and indexes:

   1. Copy the following command:

      ```sh
      atlas deployments setup --initdb {folder}
      ```

   2. Replace the `{folder}` placeholder with the directory that contains the `.js` and `.sh` files to run inside the local container in alphanumeric order.
   3. Run the command.

1. **Specify what to deploy.**

   Specify `local - Local Database` and press Enter.. **Example:**

   **Input:**

   ```sh
   ? What would you like to deploy?  [Use arrows to move, type to filter, ? for more help]
   > local - Local Database
     atlas - Atlas Database
   ```

   **Output:**

   ```sh
   [Default Settings]
   Deployment Name   local50
   MongoDB Version   7.0
   Port              27017
   ```

1. **Specify how to set up your local Atlas database.**

   **Example:** Specify `custom - With custom settings` and press Enter..

   ```sh
   ? How do you want to setup your local MongoDB database?  [Use arrows to move, type to filter]
     default - With default settings
   > custom - With custom settings
     cancel - Cancel set up
   ```

1. **Specify a deployment name.**

   **Example:** Specify `myLocalRs` and press Enter..

   ```sh
   ? Deployment Name [This can't be changed later] (local3612) myLocalRs
   ```

1. **Specify a MongoDB Version.**

   **Example:** Specify `7.0` and press Enter..

   ```sh
   ? MongoDB Version  [Use arrows to move, type to filter]
   > 7.0
     6.0
   ```

1. **Specify a port.**

   **Example:** Specify `37018` and press Enter..

   **Input:**

   ```sh
   ? Specify a port (49469) 37018
   ```

   **Output:**

   ```sh
   Creating your deployment myLocalRs
   1/2: Starting your local environment...
   2/2: Creating your deployment myLocalRs...
   Deployment created!
   Connection string: mongodb://localhost:37018/?directConnection=true
   ```

---

**Non-Interactive**

1. **Run the `atlas deployments` command with the options.**

   **Example:**

   **Input:**

   ```sh
   atlas deployments setup myLocalRs1 --type local --force
   ```

   **Output:**

   ```sh
   [Default Settings]
   Deployment Name      myLocalRs1
   MongoDB Version   7.0
   Port              49684
   
   Creating your deployment myLocalRs1
   1/2: Starting your local environment...
   2/2: Creating your deployment myLocalRs1...
   Deployment created!
   Connection string: mongodb://localhost:49684/?directConnection=true
   
   connection skipped
   ```

   To initialize the local Atlas deployment with your own data and indexes:

   1. Copy the following command:

      ```sh
      atlas deployments setup myLocalRs1 --type local --force --initdb {folder}
      ```

   2. Replace the `{folder}` placeholder with the directory that contains the `.js` and `.sh` files to run inside the local container in alphanumeric order.
   3. Run the command.

---

## Manage a Local Atlas Deployment

Use the `atlas deployments` command to manage a local Atlas deployment. You can use the following commands for both local and cloud Atlas deployments. You can use `--type local` or `--type atlas` to run the command for local or cloud atlas deployments respectively.

1. **List the available deployments.**

   **Example:**

   **Input:**

   ```sh
   atlas deployments list
   ```

   **Output:**

   ```sh
   NAME        TYPE    MDB VER   STATE
   local50     LOCAL   7.0.1     IDLE
   local62     LOCAL   7.0.1     IDLE
   myLocalRs   LOCAL   7.0.1     IDLE
   myLocalRs1  LOCAL   7.0.1     IDLE
   ```

1. **Download and load the sample data.**

   1. Run the following command to download the sample data:

      ```sh
      curl  https://atlas-education.s3.amazonaws.com/sampledata.archive -o sampledata.archive
      ```

   2. Copy and paste the following command into your terminal and replace `{port-number}` with the port for your deployment:

      ```sh
      mongorestore --archive=sampledata.archive --port={port-number}
      ```

1. **Connect to a deployment.**

   1. Run the following command to connect to a deployment:

      ```sh
      atlas deployments connect
      ```

   2. Specify the deployment to connect to and press Enter..
   3. Specify how you want to connect to the deployment and press Enter.. You can retrieve the connection string or connect to the following clients: connection string:

      - `mongosh` if you installed `mongosh`
      - MongoDB Compass if you installed [Compass](/)
      - Visual Studio Code if you installed [Visual Studio Code](https://code.visualstudio.com/download), [Visual Studio Code CLI](https://code.visualstudio.com/download), and [MongoDB for VS Code Extension](https://www.mongodb.com/docs/mongodb-vscode/install/)

1. **Pause a deployment.**

   1. Run the following command to pause a deployment:

      ```sh
      atlas deployments pause
      ```

   2. Specify the deployment to pause and press Enter..

1. **Start a deployment**

   1. Run the following command to start a deployment:

      ```sh
      atlas deployments start
      ```

   2. Specify the deployment to start and press Enter..

1. **Return the logs for the deployment.**

   1. Run the following command to return the logs for a deployment:

      ```sh
      atlas deployments logs
      ```

   2. Specify the deployment to return logs for and press Enter..

## Delete an Atlas Deployment

1. Run the following command to delete a deployment:

   ```sh
   atlas deployments delete
   ```

2. Specify the deployment to delete and press Enter..
3. Specify `y` and press Enter. to confirm.

## Move a Local Atlas Deployment to a Cloud Atlas Deployment

You can use [Docker](https://www.docker.com/) and [MongoDB Database Tools](/installation/installation/) to move a local Atlas deployment to a cloud Atlas deployment.

1. **Create a cloud Atlas deployment.**

   ```sh
   atlas deployments setup --type atlas
   ```

1. **Create a local Atlas deployment.**

   ```sh
   atlas deployments setup --type local
   ```

   To initialize the local Atlas deployment with your own data and indexes:

   1. Copy the following command:

      ```sh
      atlas deployments setup --type local --initdb {folder}
      ```

   2. Replace the `{folder}` placeholder with the directory that contains the `.js` and `.sh` files to run inside the local container in alphanumeric order.
   3. Run the command.

1. **Create a binary file of the data.**

   1. Copy the following command:

      ```sh
      docker exec -u root -it {local_deployment_name} sh -c "mkdir -p /data/dump && chown -R mongod:mongod /data/dump && mongodump --archive=/data/dump/dump.archive"
      ```

   2. Replace the `{local-deployment-name}` placeholder with the name of your local Atlas deployment.
   3. Run the command.

1. **Copy the archive.**

   1. Copy the following command:

      ```sh
      docker cp <local deployment name>:/data/dump/dump.archive .
      ```

   2. Replace the `{local-deployment-name}` placeholder with the name of your local Atlas deployment.
   3. Run the command.

1. **Return the connection string.**

   ```sh
   atlas deployments connect --type atlas --connectWith connectionString
   ```

1. **Restore from the archived file.**

   1. Copy the following command:

      ```sh
      mongorestore --uri={connection-string} --archive=./dump.archive
      ```

   2. Replace the `{connection-string}` placeholder with the connection string.
   3. Run the command.

1. **(Optional) Delete the local Atlas deployment.**

   ```sh
   atlas deployments delete --type local
   ```

## Update a Local Atlas Deployment

You can use [Docker](https://www.docker.com/) and [MongoDB Database Tools](/installation/installation/) to update a local Atlas deployment to a newer version of the image.

1. **Create a new local Atlas deployment.**

   ```sh
   atlas deployments setup --type local
   ```

   To initialize the local Atlas deployment with your own data and indexes:

   1. Copy the following command:

      ```sh
      atlas deployments setup --type local --initdb {folder}
      ```

   2. Replace the `{folder}` placeholder with the directory that contains the `.js` and `.sh` files to run inside the local container in alphanumeric order.
   3. Run the command.

1. **Create a binary file of the data.**

   1. Copy the following command:

      ```sh
      docker exec -u root -it {old-local-deployment-name} sh -c "mkdir -p /data/dump && chown -R mongod:mongod /data/dump && mongodump --archive=/data/dump/dump.archive"
      ```

   2. Replace the `{old-local-deployment-name}` placeholder with the name of your old local Atlas deployment.
   3. Run the command.

1. **Copy the archive.**

   1. Copy the following command:

      ```sh
      docker cp {old-local-deployment-name}:/data/dump/dump.archive .
      ```

   2. Replace the `{old-local-deployment-name}` placeholder with the name of your old local Atlas deployment.
   3. Run the command.

1. **Return the new connection string.**

   ```sh
   atlas deployments connect --type local --connectWith connectionString
   ```

1. **Restore from the archived file.**

   1. Copy the following command:

      ```sh
      mongorestore --uri={connection-string} --archive=./dump.archive
      ```

   2. Replace the `{connection-string}` placeholder with the connection sring.
   3. Run the command.

1. **(Optional) Delete the old local Atlas deployment.**

   ```sh
   atlas deployments delete --type local
   ```

## Use Atlas Search with a Local Atlas Deployment

Use the `atlas deployments search indexes create` command to create an Atlas Search search index. You can then run Atlas Search queries. To learn more, see [Atlas Search](/atlas-search). You can run this command with local and cloud Atlas deployments. For detailed steps, see atlas-cli-fts-index-query.

## Use Atlas Vector Search with a Local Atlas Deployment

Use the `atlas deployments search indexes create` command to work with Atlas Vector Search. To learn more, see How to Index Vector Embeddings for Vector Search. You can run this command with local and cloud Atlas deployments. For detailed steps, see atlas-cli-deploy-avs.

*[Contenido incluido desde: /includes/fact-avs-mdb-version.rst]*

## Supported Actions

To learn all of the actions that `atlas deployments` supports, see atlas-deployments.

## Troubleshoot Errors

To learn more about troubleshooting local Atlas deployment issues, see local-deploy-errors.
