# Test Automations with Ephemeral Projects and Clusters

This tutorial demonstrates how to quickly create and delete ephemeral Atlas projects and clusters using the Atlas CLI. Ephemeral projects and clusters provide temporary testing environments that you isolate from your production projects and clusters. You can use ephemeral projects and clusters to test your automation scripts before using the scripts on production clusters.

## Prerequisites

Before you begin, complete the following tasks:

- Create an Atlas user account.
- Create an [Atlas organization](/organizations-projects/) and note its organization ID. You can find the organization ID by running the atlas-organizations-list command.
- install-atlas-cli.
- connect-atlas-cli.

## Follow These Steps

1. **Create the ephemeral project, cluster, and database user.**

   1. Run the atlas-users-describe command to find your Atlas user ID. Replace <YOUR-EMAIL> with the email address that is associated with your Atlas user account.

      ```
      atlas users describe --username <YOUR-EMAIL>
      ```

   2. Run the atlas-projects-create command to create the ephemeral project. Replace <ORG-ID> with the organization ID and replace <YOUR-USER-ID> with your Atlas user ID that you retrieved in the previous step.

      ```
      atlas projects create myEphemeralProject --orgId <ORG-ID> --ownerId <YOUR-USER-ID>
      ```

   3. Retrieve the project ID for the ephemeral project you created from the response. In the following example response, the project ID is `64933bde48add154124e343f`.

      ```
      Project '64933bde48add154124e343f' created.
      ```

      Alternatively, you can use the atlas-projects-list command to find the project ID.
   4. Run the atlas-setup command to create an `M10` cluster and a database user. Replace <YOUR-PASSWORD> with a password for the database user and replace <PROJECT-ID> with the project ID for the ephemeral project you created.

      ```
      atlas setup --clusterName myEphemeralCluster --provider AWS --region US_EAST_1 --tier M10 --username myEphemeralUser --password <YOUR-PASSWORD> --currentIp --skipSampleData --projectId <PROJECT-ID> --force
      ```

      After Atlas creates the cluster, the Atlas CLI provides the [connection string](/reference/connection-string/#connection-string-options) and connects to your cluster through `mongosh`.

1. **Test your automations on the ephemeral project and cluster.**

1. **Delete the ephemeral project, cluster, and database user.**

   1. Run the atlas-clusters-delete command to delete the ephemeral cluster and its database users. Replace <PROJECT-ID> with the project ID for your ephemeral project.

      ```
      atlas clusters delete myEphemeralCluster --projectId <PROJECT-ID> --force
      ```

   2. Run the atlas-projects-delete command to delete the ephemeral project. Replace <PROJECT-ID> with the project ID for your ephemeral project.

      ```
      atlas projects delete <PROJECT-ID> --force
      ```

      > **Note:**
      > You can't delete the ephemeral project until the ephemeral cluster finishes shutting down. If you get an error stating `CANNOT_CLOSE_GROUP_ACTIVE_ATLAS_CLUSTERS`, wait five minutes, then run the `atlas projects delete` command again.

   You can run the atlas-projects-list command to confirm successful deletion of the ephemeral project and cluster. If the ephemeral project is missing from the list, you successfully deleted both the project and the cluster.
