# Deploy from a Private Registry

The Atlas CLI pulls a Docker image and creates a container in order to deploy the local cluster. By default, the Atlas CLI pulls from the public Docker Hub [repository](https://hub.docker.com/r/mongodb/mongodb-atlas-local/tags) `mongodb/mongodb-atlas-local`. You can configure it to pull a specific image from an internal, typically private, registry. This page explains how to pull and re-tag the `mongodb-atlas-local` container image, push it into a private registry, and then configure the Atlas CLI to use the private images for deploying local clusters.

## Prerequisites

Before you begin, ensure that you have the following:

- Docker command line tools
- Private container registry for which you have write permission
- A terminal
- Atlas CLI

## Procedure

1. **Pull and re-tag the MongoDB Local image for local deployment.**

   To pull and re-tag, run the following command after replacing the following placeholder values:

   | `<tag>` | The tag, which specifies the MongoDB version, that you want to use in your deployment. |
   | --- | --- |
   | `<container-registry-namespace>` | The fully qualified path and name of the container registry namespace for which you have write access. |

   ```shell
   docker tag mongodb/mongodb-atlas-local:<tag> <container-registry-namespace>/mongodb-atlas-local:<tag>
   ```

   > **Note:**
   > Atlas CLI supports deploying MongoDB version 7 and 8. In order to support deploying either version from your registry using the Atlas CLI, you must repeat this step for 7.0 and 8.0 tags. For the list of available tags, see the [docker hub repository](https://hub.docker.com/r/mongodb/mongodb-atlas-local/tags).

1. **Push the image on your machine to your container registry.**

   1. Run the following command after replacing the following placeholder values to push the image to your container registry:

      | `<tag>` | The tag, which specifies the MongoDB version, that you want to use in your deployment. |
      | --- | --- |
      | `<container-registry-namespace>` | The fully qualified path and name of the container registry namespace for which you have write access. |

      ```shell
      docker push <container-registry-namespace>/mongodb-atlas-local:<tag>
      ```

      It might take a few minutes for this operation to complete.

      > **Note:**
      > Atlas CLI supports deploying MongoDB version 7 and 8. In order to support deploying either version from your registry using the Atlas CLI, you must repeat this step for 7.0 and 8.0 tags.

   2. Verify that the image was successfully uploaded. You can verify in the following ways:

      - Log in to your container registry and verify that the image was successfully uploaded.
      - Run the `docker pull` command to pull the image from your registry to your machine.

1. **Configure Atlas CLI to use the image in the registry.**

   1. Set the `MONGODB_ATLAS_LOCAL_DEPLOYMENT_IMAGE` environment variable after replacing `<container-registry-namespace>` with the fully qualified path and name of the container registry namespace that you created in step 2.

      ```shell
      export MONGODB_ATLAS_LOCAL_DEPLOYMENT_IMAGE=<container-registry-namespace>/mongodb-atlas-local
      ```

      > **Note:**
      > This method only sets the environment variable for the current terminal session. To persist this setting across terminal sessions, set this environment variable in your profile stored in the configuration file.

   2. Verify that the environment variable is set correctly.

      ```shell
      echo $MONGODB_ATLAS_LOCAL_DEPLOYMENT_IMAGE
      ```

   After you set the environment variable, Atlas CLI uses the value of the variable instead of the default location for all deployments.

1. **Deploy Atlas with your image.**

   1. Run the following command to start the deployment after replacing the `<deploymentName>` with a name for the deployment.

      ```shell
      atlas deployments setup <deploymentName>
      ```

   2. Enter `local` when prompted to specify the type of deployment.
   3. Complete the deployment by selecting the appropriate settings when prompted. To learn more, see atlas-cli-deploy-local.
