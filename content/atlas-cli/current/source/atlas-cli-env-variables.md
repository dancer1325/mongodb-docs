# Atlas CLI Environment Variables

For easier scripting, you can specify configuration settings by using environment variables.

## Precedence

- When you run any command, each setting that you specify with an environment variable takes precedence over the profile stored in the configuration file.
- When you run a command using the `--projectId` option, the command line option takes precedence over both the environment variable and the profile stored in the configuration file.

## Supported Environment Variables

> **Important:**
> Atlas CLI supports both MongoDB CLI environment variables and Atlas CLI environment variables. You can use either MongoDB CLI environment variables or Atlas CLI environment variables, but not both together.

The Atlas CLI supports the following environment variables:

| Variable | Description |
| --- | --- |
| `DO_NOT_TRACK` | Flag that indicates whether telemetry is disabled for the Atlas CLI. Set to `1` to disable telemetry. You can also enable or disable telemetry with `MONGODB_ATLAS_TELEMETRY_ENABLED`, but you don't need to specify both. |
| `MONGODB_ATLAS_CLIENT_ID` | Sets the Service Account client ID for commands that interact with Atlas. |
| `MONGODB_ATLAS_CLIENT_SECRET` | Sets the Service Account client secret for commands that interact with Atlas. |
| `MONGODB_ATLAS_SILENCE_STORAGE_WARNING` | When set to `true`, the Atlas CLI does not warn you if secure storage is unavailable. |
| `MONGODB_ATLAS_PUBLIC_API_KEY` | Sets the public API (Application Programming Interface) key for commands that interact with Atlas. |
| `MONGODB_ATLAS_PRIVATE_API_KEY` | Sets the private API (Application Programming Interface) key for commands that interact with Atlas. |
| `MONGODB_ATLAS_ORG_ID` | Sets the organization ID for commands that require the `--orgId` option. |
| `MONGODB_ATLAS_PROJECT_ID` | Sets the project ID for commands that require the `--projectId` option. |
| `MONGODB_ATLAS_OUTPUT` | Sets the output fields and format. Valid values are: .. list-table:: :header-rows: 1 :widths: 40 60 |
| Value | Output Format |
| | Empty | *default* | Human-readable output that includes all fields that the Atlas CLI returns. |
| `json` | JSON output that includes all fields that the Atlas CLI returns. |
| `json-path` | JSON output that includes the fields that you specify. |
| `go-template` | Custom-formatted output that includes the fields that you specify in a Go template. |
| `MONGODB_ATLAS_MONGOSH_PATH` | The full path on your local system to the MongoDB Shell, `mongosh`. |
| `MONGODB_ATLAS_SKIP_UPDATE_CHECK` | When set to `yes`, the Atlas CLI does not prompt you to update to new versions. |
| `MONGODB_ATLAS_ACCESS_TOKEN` | String that grants access to your Atlas account. The access token is valid for 12 hours. |
| `MONGODB_ATLAS_REFRESH_TOKEN` | String that allows Atlas to automatically request a new access token when the current access token expires. |
| `MONGODB_ATLAS_TELEMETRY_ENABLED` | Flag that indicates whether telemetry is enabled for the Atlas CLI. Set to `false` to disable telemetry. You can also enable or disable telemetry with `DO_NOT_TRACK`, but you don't need to specify both. |
| `MONGODB_ATLAS_LOCAL_DEPLOYMENT_IMAGE` | String that specifies the container registry with the namespace and name of the repository that contains the image to use for local deployments. |
| `HTTP_PROXY` | The absolute URL (Uniform Resource Locator) or the hostname and port in the `hostname[:port]` format. The following examples show how to set up the environment variable in different situations: - If your proxy configuration doesn't require authentication: .. code-block:: sh :copyable: false HTTP_PROXY=<my.proxy.address> - If your proxy configuration requires authentication: .. code-block:: sh :copyable: false HTTP_PROXY=<username>:<password>@<my.proxy.address> - If the scheme is `socks5`: .. code-block:: sh :copyable: false HTTP_PROXY=socks5://<my.proxy.address> |
| `HTTPS_PROXY` | The absolute URL (Uniform Resource Locator). If `HTTP_PROXY` is also set, this takes precedence over `HTTP_PROXY` for all requests. The following example shows how to set up the environment variable: .. code-block:: sh :copyable: false HTTPS_PROXY=https://<my.proxy.address> |
| `NO_PROXY` | Indicates no proxy for the URL (Uniform Resource Locator) because proxy isn't configured for the URL (Uniform Resource Locator). |
