# Use the Atlas Administration API from the Atlas CLI

The Atlas CLI provides the `api` subcommand with which you can access every Atlas Administration API endpoint directly from the Atlas CLI. This feature is a robust and reliable way to use every capability of the Atlas Administration API. It provides access to the entire Atlas Administration API so that you can script or automate any task, with the benefits that come from using a command-line interface:

- Full feature parity with the Atlas Administration API.
- Quicker access to new Atlas Administration API resources and endpoints.
- A unified, predictable command structure for automation.
- Ability to pin a desired API version, ensuring your scripts remain reliable, even if you update the CLI.
- Ability to watch a command until the operation completes.

This tutorial demonstrates how to use the Atlas Administration API from the Atlas CLI commands.

> **Note:**
> The API (Application Programming Interface) subcommands are automatically generated from the Atlas Administration API and provide access to the entire Atlas Administration API (including preview resources). You must have some familiarity with the Atlas Administration API as well as an understanding that the default input and output is a direct mapping of the API media type to benefit from this Atlas CLI feature.

## Syntax

To use Atlas CLI with the Atlas Administration API, run the command in the following format:

```shell
atlas api <tag> <operationId> [options]|--file <fileName>.json --version <api-resource-version>
```

### Arguments

| Argument | Necessity | Description |
| --- | --- | --- |
| `<tag>` | Required | The name of the tag used in the Atlas Administration API documentation URL (Uniform Resource Locator) for the API (Application Programming Interface) resource. The tag is hyphen-separated in the Atlas Administration API documentation URL (Uniform Resource Locator). However, you must convert it to camelcase in the Atlas CLI command syntax. For example, consider the following URL (Uniform Resource Locator) for an Atlas Administration API resource: .. code-block:: shell https://www.mongodb.com/docs/atlas/reference/api-resources-spec/v2/#tag/Example-Tag-Name/ For accessing the resource in the preceding URL (Uniform Resource Locator), replace `<tag>` with `exampleTagName` in the command syntax: .. code-block:: shell atlas api exampleTagName <operationId> For more examples, see atlas-cli-admin-api-examples. |
| `<operationId>` | Required | The identifier of the operation in the Atlas Administration API documentation URL (Uniform Resource Locator) for the API (Application Programming Interface) endpoint. The value is in camelcase format. For example, consider the following URL (Uniform Resource Locator) for an Atlas Administration API endpoint operation: .. code-block:: shell https://www.mongodb.com/docs/atlas/reference/api-resources-spec/v2/#tag/Example-Tag-Name/operation/exampleEndpointOperationId For performing the operation supported by the endpoint in the preceding URL (Uniform Resource Locator), replace `<tag>` with `exampleTagName` and use the ID of the operation, `exampleEndpointOperationId`, as shown the command. .. code-block:: shell atlas api exampleTagName exampleEndpointOperationId [options] For more examples, see atlas-cli-admin-api-examples. |

### Options

You can pass the API (Application Programming Interface) path, query, and request body parameters as options with the command. You can specify the options directly with the command or using a JSON (Javascript Object Notation) file. The command also supports the following options:

| Option | Necessity | Description |
| --- | --- | --- |
| `--file` | Conditional | JSON (Javascript Object Notation) file that contains the API (Application Programming Interface) path, query, and request body parameters for the operation. This is required only if there are required path, query, or request body parameters for the operation that you aren't specifying directly with the command. |
| `--version` | Optional | API (Application Programming Interface) resource version to use. We recommend using it to pin your scripts to specific API (Application Programming Interface) versions. If omitted, the command defaults to the latest version (or your profile's configured version). However, we recommend explicitly setting the version to ensure your scripts remain stable. This protects your scripts from breaking when new API (Application Programming Interface) versions are released with potentially incompatible changes. |
| `--watch` | Optional | Flag that specifies whether to watch the operation until it completes. |

## Examples

The following Atlas CLI command with the Atlas Administration API demonstrates how to retrieve a compressed (`.gz`) log file that contains a range of log messages for the specified host for the specified project:

```shell
atlas api monitoringAndLogs getHostLogs --groupId 5e2211c17a3e5a48f5497de3 --hostName mycluster-shard-00-02.7hgjn.mongodb.net --logName mongodb --output gzip --version 2025-03-12
```

The following Atlas CLI command with the Atlas Administration API demonstrates how to create a cluster by using the `--file` option.

```shell
atlas api clusters createCluster --groupId 5e2211c17a3e5a48f5497de3 --file cluster-config.json --version 2025-03-12
```

To learn more about creating a configuration file for a cluster, see atlas-cli-cluster-config-file. The following Atlas CLI command with the Atlas Administration API demonstrates how to simulate regional cloud provider outages. This simulation lets you test your application's failover behavior and disaster recovery procedures in a controlled environment separate from production. The command uses a file named `outage_simulation.json` with the following settings:

```
{
   "outageFilters": [
      {
         "cloudProvider": "AWS",
         "regionName": "US_EAST_1",
         "type": "REGION"
      }
   ]
}
```

**Input:**

```shell
atlas api clusterOutageSimulation startOutageSimulation --groupId 5e2211c17a3e5a48f5497de3 --clusterName myCluster --file outage_simulation.json --version 2025-03-12
```

**Output:**

```shell
{"clusterName":"myCluster","groupId":"5e2211c17a3e5a48f5497de3","id":"6808ed9bed0b0b51caee336b","outageFilters":[{"cloudProvider":"AWS","regionName":"US_EAST_1","type":"REGION"}],"startRequestDate":"2025-04-23T13:39:39Z","state":"START_REQUESTED"}
```
