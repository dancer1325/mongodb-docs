# Configure Independent Shard Scaling

For Atlas CLI commands that you use to deploy and manage Atlas clusters on the cloud, you can configure how the cluster handles resource scaling by using the `--autoScalingMode` option. You can use this option  for the following commands to specify whether the cluster nodes scale together or independently:

- atlas-setup
- atlas-deployments-setup
  - atlas-deployments-delete
  - atlas-deployments-list
  - atlas-deployments-pause
  - atlas-deployments-start
- atlas-clusters-create
  - atlas-clusters-update
  - atlas-clusters-describe
  - atlas-clusters-list
  - atlas-clusters-watch
  - atlas-clusters-pause
  - atlas-clusters-start
  - atlas-clusters-delete

The `--autoScalingMode` option takes the following values:

<details>
<summary>clusterWideScaling</summary>

```javascript
atlas setup --clusterName symmetricShardCluster --provider AWS --autoScalingMode clusterWideScaling --projectId 5e2211c17a3e5a48f5497de3 --tier M10
```

For clusters configured with `clusterWideScaling`, the JSON (Javascript Object Notation) output looks similar to the following:

```json
{
  "clusterType": "SHARDED",
  "name": "symmetricShardCluster",
  "diskSizeGB": 0,
  "replicationSpecs": [
    {
      "id": "internalId",
      "numShards": 2,
      "regionConfigs": [
        {
          "electableSpecs": { ... },the 
          "readOnlySpecs": { ... },
          ...
        }
      ],
      "zoneName": "string"
    }
  ],
  ...
}
```

To learn more about the output, see the [getCluster](https://www.mongodb.com/docs/atlas/reference/api-resources-spec/v2/2024-10-23/#tag/Clusters/operation/getCluster) endpoint. If you omit the `--autoScalingMode` option, the command defaults to `clusterWideScaling` mode.
</details>

<details>
<summary>independentShardScaling</summary>

```javascript
atlas setup --clusterName asymmetricShardCluster --provider AWS --autoScalingMode independentShardScaling --projectId 5e2211c17a3e5a48f5497de3 --tier M10 
```

For clusters configured with `independentShardScaling`, the JSON (Javascript Object Notation) output looks similar to the following:

```json
{
  "clusterType": "SHARDED",
  "name": "asymmetricShardCluster",
  "replicationSpecs": [
    {
      "id": "externalId",
      "regionConfigs": [
        {
          "electableSpecs": {
              "diskSizeGB": 10,
              ...
          },
          "readOnlySpecs":  {
              "diskSizeGB": 10,
              ...
          },
        }
      ],
      "zoneId": "string",// for GET/UPDATE
      "zoneName": "string"
    },
    ...
  ]
}
```

The JSON (Javascript Object Notation) output includes the `replicationSpecs` object that describes the properties of a single shard. The `replicationSpecs`  elements define the number of shards instead of the `numShards` field. The `diskSizeGB` field is inside each shard's `replication_specs.regionConfig` object. The `zoneId` field that identifies the zone for Global cluster is returned in the output. To learn more about the output, see the getCluster endpoint.
</details>
