# Create an Atlas Cluster Using a Configuration File

This tutorial demonstrates how to use Atlas CLI commands to create a new Atlas cluster from a configuration file. Specifically, it demonstrates how to:

1. Get the configuration settings of an existing Atlas cluster and save the settings to a configuration file using the atlas-clusters-describe command.
2. Create an Atlas cluster from the configuration file using the atlas-clusters-create command.

## Prerequisites

Before you begin, you must have the following:

- An Atlas cluster
- Atlas CLI
- A profile that contains the IDs of the Atlas organization and project from where you wish to retrieve existing cluster settings and where you wish to create the new cluster.

## Create an Atlas Cluster From a Configuration File

You can use the procedures in this section to easily create a new cluster by exporting settings from an existing cluster instead of manually creating a configuration file yourself.

### Export Existing Cluster Configuration Settings to a File

1. **Connect to your Atlas account for programmatic access if you haven't connected yet.**

   To learn more, see connect-atlas-cli.

1. **Run the following command to export the details of an existing cluster to a JSON (Javascript Object Notation) configuration file named `myCluster`.**

   ```shell
   atlas clusters describe <cluster-name> --output json > myCluster.json
   ```

   Replace <cluster-name> in the preceding command with the name of the existing cluster that you wish to clone.

### (Optional) Edit the Configuration File for the new Cluster

1. **Open the JSON (Javascript Object Notation) file in a text editor to view the configuration settings.**

   > **Example:**
   > The following example uses the `vi` editor to view the replica set settings for an `M10` cluster named `mySandbox` in the `myCluster.json` file.
   >
   > **Input:**
   >
   > ```shell
   > vi myCluster.json 
   > ```
   >
   > **Output:**
   >
   > ```json
   > {
   >   "backupEnabled": true,
   >   "biConnector": {
   >     "enabled": false,
   >     "readPreference": "secondary"
   >   },
   >   "clusterType": "REPLICASET",
   >   "connectionStrings": {
   >     "standard": "<connection-string>"
   >   },
   >   "diskSizeGB": 10,
   >   "encryptionAtRestProvider": "NONE",
   >   "groupId": "<group-id>",
   >   "id": "<64403dd1f2a6b45e71527d5a>",
   >   "mongoDBMajorVersion": "6.0",
   >   "mongoDBVersion": "6.0.5",
   >   "name": "mySandbox",
   >   "paused": false,
   >   "pitEnabled": true,
   >   "stateName": "IDLE",
   >   "replicationSpecs": [
   >     {
   >       "numShards": 1,
   >       "id": "64403dbb0a052449df3d04ae",
   >       "zoneName": "Zone 1",
   >       "regionConfigs": [
   >         {
   >           "analyticsAutoScaling": {
   >             "diskGB": {
   >               "enabled": true
   >             },
   >             "compute": {
   >               "enabled": true,
   >               "scaleDownEnabled": true,
   >               "minInstanceSize": "M10",
   >               "maxInstanceSize": "M40"
   >             }
   >           },
   >           "analyticsSpecs": {
   >             "diskIOPS": 3000,
   >             "ebsVolumeType": "STANDARD",
   >             "instanceSize": "M10",
   >             "nodeCount": 0
   >           },
   >           "electableSpecs": {
   >             "diskIOPS": 3000,
   >             "ebsVolumeType": "STANDARD",
   >             "instanceSize": "M10",
   >             "nodeCount": 3
   >           },
   >           "readOnlySpecs": {
   >             "diskIOPS": 3000,
   >             "ebsVolumeType": "STANDARD",
   >             "instanceSize": "M10",
   >             "nodeCount": 0
   >           },
   >           "autoScaling": {
   >             "diskGB": {
   >               "enabled": true
   >             },
   >             "compute": {
   >               "enabled": true,
   >               "scaleDownEnabled": true,
   >               "minInstanceSize": "M10",
   >               "maxInstanceSize": "M40"
   >             }
   >           },
   >           "priority": 7,
   >           "providerName": "AWS",
   >           "regionName": "US_EAST_1"
   >         }
   >       ]
   >     }
   >   ],
   >   "createDate": "2023-04-19T19:15:29Z",
   >   "rootCertType": "ISRGROOTX1",
   >   "versionReleaseSystem": "LTS",
   >   "terminationProtectionEnabled": false
   > }
   > ```
   >
   >

1. **(Optional) Make changes to the settings in the configuration file as needed.**

   To learn more about the optional and required settings, see atlas-cli-cluster-config-file.

1. **Save and close the configuration file.**

### Create a New Cluster Using the Configuration File

1. **Connect to your Atlas account for programmatic access if you aren't already connected to your Atlas account.**

   To learn more, see connect-atlas-cli.

1. **Run the following command to create an Atlas cluster using the configuration file.**

   ```
   atlas clusters create <new-cluster-name> -f myCluster.json
   ```

   Replace `<new-cluster-name>` in the preceding command with the name of the new cluster you wish to create.

1. **Run the following command to check the status of the cluster.**

   ```shell
   atlas clusters watch <new-cluster-name>
   ```

   Replace <new-cluster-name> in the preceding command with the name of the new cluster. This command checks the cluster\'s status periodically until it reaches an `IDLE` state. Once  the cluster reaches the expected state, the command prints "Cluster available."
