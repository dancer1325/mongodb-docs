# Docker Compose Example

This complete Docker Compose example sets up a local MongoDB Atlas cluster with persistent data. Review the complete Docker Compose file, and each component, to understand its purpose and configuration. To learn more, see atlas-cli-deploy-docker.

## Docker Compose File

The following example creates a local Atlas cluster using the `mongodb/mongodb-atlas-local` image. The file configures networking, mounts the correct data volumes for seeding the database and persisting data, and Atlas Search indexes.

```sh
services:
  mongodb:
    image: mongodb/mongodb-atlas-local
    hostname: mongodb
    environment:
      - MONGODB_INITDB_ROOT_USERNAME=user
      - MONGODB_INITDB_ROOT_PASSWORD=pass
    ports:
      - 27017:27017
    volumes:
      - ./init:/docker-entrypoint-initdb.d
      - db:/data/db
      - configdb:/data/configdb
      - mongot:/data/mongot
volumes:
  db:
  configdb:
  mongot:
```

Use the following information to provision your local Atlas cluster with your own data, and persist your configuration and data.

## File Structure

The Docker Compose file has the following main parameters:

- `services` defines the containers to run. In this example, a single service named `mongodb` defines the MongoDB service. You might have other services required for your complete application to run.
- `volumes` defines the persistent storage locations that you can share among containers. This example shows the persistent storage locations required for the MongoDB service to operate.

## Service Configuration
### Image

```sh
image: mongodb/mongodb-atlas-local
```

*Required* This parameter defines the local MongoDB Atlas image. This defintion uses the latest version of the image and is equivalent to using the `:latest` tag. If your organization requires you to use a specific image build, you can append `@sha256:<digest>`. If you want to use a specific version of MongoDB with the latest OS updates, use the version number tags. For example, `mongodb/mongodb-atlas-local:8.0.6`

### Hostname

```sh
hostname: mongodb
```

*Required* This parameter defines the hostname for the container. You must specify this correctly for the local replica set to function. This parameter is required to ensure communication between the different services within the same Docker Container network. You can refer to this container by the same hostname, `mongodb`.

### Environment Setup
#### Authentication

```sh
environment:
  - MONGODB_INITDB_ROOT_USERNAME=user
  - MONGODB_INITDB_ROOT_PASSWORD=pass
```

*Recommended* The highlighted `environment` parameters set the initial root username and password for the local Atlas cluster. For simplicity, in this example, the credentials are defined in the Docker Compose file itself, but you can abstract the credientials into an environment variable.

#### Logging

```sh
environment:
  - MONGODB_INITDB_ROOT_USERNAME=user
  - MONGODB_INITDB_ROOT_PASSWORD=pass
  - MONGOT_LOG_FILE=/dev/stderr
  - RUNNER_LOG_FILE=/dev/stderr
```

*Optional* The highlighted `environment` parameters define logging to help diagnose any issues.  `mongot`, which provides the Atlas Search capabilities, generates the `MONGOT_LOG_FILE`. This example outputs the `mongot` logs to the `stderr` directory. The runner service generates the `RUNNER_LOG_FILE`. The runner service monitors the process that creates `mongod` and configures the integration with `mongot`. This example outputs the runner logs to the `stderr` directory.

### Volumes

Volumes persist data and the configuration across container restarts.

#### Initialization

```sh
volumes:
  - ./init:/docker-entrypoint-initdb.d
```

*Conditional* The highlighted `volumes` parameter mounts the local `init` directory to the container's initialization directory. Typically, the `init` directory is placed in the project, and when you run `docker-compose up`, the process searches for any initialization scripts in this directory. Supported file types include:

- `.js` MongoDB shell scripts
- `.sh` Bash scripts

For example, when you place the following script in the `init` folder:

1. The script retrieves an example sample data set.
2. Confirms the data set downloaded.
3. Uses `mongorestore`, which the local MongoDB Atlas container image provides, to restore the archive into the new local Atlas cluster.
4. Confirms the data loaded.

```sh
# init/init.sh
#!/bin/bash

curl -O https://atlas-education.s3.amazonaws.com/sampledata.archive
echo "Sample data downloaded."
mongorestore --uri "$CONNECTION_STRING" --archive=./sampledata.archive
echo "Sample data loaded successfully."
```

> **Note:**
> Since building your connection string during the seeding process might be challenging, especially when you switch between your local machine and the Docker network, the `$CONNECTION_STRING` environment variable, is automatically provided during the seeding process.

#### Data

```sh
    volumes:
      - ./init:/docker-entrypoint-initdb.d
      - db:/data/db
      - configdb:/data/configdb
      - mongot:/data/mongot
volumes:
  db:
  configdb:
  mongot:
```

*Conditional* MongoDB stores its data in the `/data/db` directory by default. In this Docker Compose file, the `/data/db` directory is then mapped to the `db` volume for the MongoDB service. The `volumes` parameters at the end of the Docker Compose file declare the volumes that other services can use, including the `db` volume mapped in the mongodb service. The `db` volume acts as a storage location outside the container, preserving data across container restarts.

#### Configuration

```sh
    volumes:
      - ./init:/docker-entrypoint-initdb.d
      - db:/data/db
      - configdb:/data/configdb
      - mongot:/data/mongot
volumes:
  db:
  configdb:
  mongot:
```

*Conditional* By default, MongoDB stores its configuration data in the `/data/configdb` directory, which persists in the same way as the stored data.

#### Atlas Search (`mongot`)

```sh
    volumes:
      - ./init:/docker-entrypoint-initdb.d
      - db:/data/db
      - configdb:/data/configdb
      - mongot:/data/mongot
volumes:
  db:
  configdb:
  mongot:
```

*Conditional* The local MongoDB Atlas image (`mongodb/mongodb-atlas-local`) comes with `mongot`, which provides the Atlas Search and Atlas Vector Search capabilities. By default, `mongot` stores indexes in the `/data/mongot` directory. This volume is mapped, mounted, and its data persists across runs in the same way as the configuration and stored data.

## Verify that Processes are Healthy

The local Atlas implementation simplifies configuring the MongoDB process (`mongod`), and Search capabilities (`mongot)`. During initialization, these processes must start and restart. In some cases the container may be running, but the process you require is not ready. The local MongoDB Atlas image also provides a health check. You can use the health check to ensure that the MongoDB, and Search capability processes, as well as any initialization scripts are fully ready. In your scripts, include the following code:

```sh
timeout 120 bash -c 'until [ "$(docker inspect --format='\''{{.State.Health.Status}}'\''<container-name>)" = "healthy" ]; do sleep 2; done'
```

You can also do a health check when you use Docker Compose.

## Use Docker Compose

To start the service, run the following command:

```sh
docker-compose up
```

You can append `-d` to run the service in detached mode. To stop the service, run the following command:

```sh
docker-compose down
```

You can append `-v` to remove the volumes and clear the data. Use this option if you don't want to persist data or configuration changes, or want to ensure you are re-starting from a fresh container. To verify whether the service is healthy when you run `docker-compose up`, add the `condition: service_healthy` parameter:

```sh
api:
    depends_on:
      atlas_local:
        condition: service_healthy
```

To learn more, see atlas-cli-deploy-docker.

## Feedback

To discuss local Atlas clusters, see the [MongoDB Developer Community](https://www.mongodb.com/community/forums/tag/local-dev-atlas-cli) forum. To get help, provide feedback, or request features, see the [MongoDB Feedback Engine](https://feedback.mongodb.com/forums/930808-atlas-cli).
