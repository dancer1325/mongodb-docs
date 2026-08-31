# Configure Telemetry

Atlas CLI's telemetry collects anonymous, aggregate usage data to identify improvements with the greatest impact. The Atlas CLI enables telemetry by default. Telemetry settings for the Atlas CLI also apply to [telemetry for mongosh](https://www.mongodb.com/docs/mongodb-shell/telemetry/)  if you installed the Atlas CLI and `mongosh` together in the same package. The following information also applies to local MongoDB deployments. To learn more, see atlas-cli-local-cloud.

## Learn What the Atlas CLI Tracks

Atlas CLI telemetry tracks non-Personally-Identifiable Information (PII), which includes but is not limited to the following information:

| Data | Example Value |
| --- | --- |
| Atlas CLI version number | `1.0.0` |
| Installation source | `Homebrew` |
| Operating System (OS) and OS version | `Windows 11.5` |
| Authentication method. Atlas CLI telemetry does *not* track the values for the API (Application Programming Interface) keys and login credentials. | `API key` |
| Details for commands you run. Atlas CLI telemetry tracks arguments only if they use pre-set allowable values, such as cluster region, or interactive command responses such as default project ID. | `timestamp: 2022-04-11T11:35:46.794119+01:00` `command: atlas cluster create` `--provider AWS --region US_EAST_1` |
| Performance information, such as the amount of time it takes for the Atlas CLI to execute a command. | `completion timestamp: 2022-04-11T11:35:49.456719+01:00` |
| Errors you encounter, including the command you run and the parameters you use. | `atlas rgister` `Error: unknown command "rgister" for "atlas"` |

## Learn What the Atlas CLI Doesn't Track

Atlas CLI telemetry *doesn't* track:

| Data | Example |
| --- | --- |
| PII and values that could potentially contain PII, including all free-text fields such as custom names or database user names. | `--clusterName MyCluster` |
| API (Application Programming Interface) key values or Atlas login credentials. | `private_api_key abcdefghi123456789` |

## Disable Telemetry for the Atlas CLI

To disable telemetry for the Atlas CLI, run the following command in the terminal:

```sh
atlas config set telemetry_enabled false
```

You can also disable telemetry in the following ways:

- Navigate to the configuration file and enter `telemetry_enabled = false`.
- Set the MONGODB_ATLAS_TELEMETRY_ENABLE environment variable to `false`.

## Enable Telemetry for the Atlas CLI

The Atlas CLI enables telemetry by default. If telemetry is currently disabled, you can enable telemetry by running the following command in the terminal:

```sh
atlas config set telemetry_enabled true
```

You can also enable telemetry in the following ways:

- Navigate to the configuration file and remove `telemetry_enabled = false`.
- Set the MONGODB_ATLAS_TELEMETRY_ENABLE environment variable to `true`.
