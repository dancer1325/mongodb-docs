# Config Shard

A sharded cluster must have a config server, but it can be either a
config shard (embedded config server) or a dedicated config server.
Using a config shard reduces the number of nodes required and can
simplify your deployment. A config shard cluster is also called an
embedded config server cluster. You cannot use the same config server
for multiple sharded clusters.

## Use Cases

## Behavior

In an embedded config server cluster, a config shard will be used to
store cluster metadata and user data. It helps reduce the complexity of
a sharded cluster deployment.

You can store sharded and unsharded collection data in your config
shard. It has all the properties of a shard as well as acting as the
config server.

### Confirm use of Config Shard

### Commands

To configure a dedicated config server to run as a config shard, run the
`transitionFromDedicatedConfigServer` command.

To configure a config shard to run as a dedicated config server, run the
`transitionToDedicatedConfigServer` command.

## Get Started

- convert-replica-set-to-embedded-config-server
- Start a Sharded Cluster with a Config Shard

## Learn More

- Config Shards
- `transitionFromDedicatedConfigServer`
- `transitionToDedicatedConfigServer`
