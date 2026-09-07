# Sharded Cluster Components

## Production Configuration

In a production cluster, ensure that data is redundant and
that your systems are highly available. Consider the following
for a production sharded cluster deployment:

- Deploy Config Servers as a 3 member replica set
- Deploy each Shard as a 3 member replica set
- Deploy one or more `mongos` routers

### Replica Set Distribution

For production deployments, we recommend deplying config server and shard replica
sets on at least three data centers. This configuration provides high availability in case
a single data center goes down.

### Number of Shards

Sharding requires at least two shards to distribute sharded data. Single
shard sharded clusters may be useful if you plan on enabling sharding in
the near future, but do not need to at the time of deployment.

### Number of `mongos` and Distribution

`mongos` routers support high availability and scalability
when deploying multiple `mongos` instances. If a proxy or load
balancer is between the application and the `mongos` routers, you
must configure it for client affinity. Client affinity allows
every connection from a single client to reach the same `mongos`.
For shard-level high availability, either:

- Add `mongos` instances on the same hardware where `mongod`
  instances are already running.
- Embed `mongos` routers on the same hardware where the application is hosted.

`mongos` routers communicate frequently with your config
servers. As you increase the number of routers, performance may degrade.
If performance degrades, reduce the number of routers.

The following diagram shows a common sharded cluster architecture used
in production:

> containing multiple shards and mongos routers.

## Development Configuration

For testing and development, you can deploy a sharded cluster with a
minimum number of components. These **non-production** clusters have the
following components:

- One `mongos` instance.
- A single shard replica set.
- A replica set config server.

The following diagram shows a sharded cluster architecture used for
**development only**:

> containing a single shard and mongos router.

> **Warning Use the test cluster architecture for testing and**
>
> development only.

> **See also**
>
> /tutorial/deploy-shard-cluster/
