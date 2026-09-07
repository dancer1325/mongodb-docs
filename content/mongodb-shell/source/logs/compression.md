# Enable Log Compression

You can enable compression for MongoDB Shell log files. By default,
MongoDB Shell does not compress log files.

## About this Task

When log compression is enabled, MongoDB Shell compresses log files with
gzip.

After you toggle log compression, you must start a new MongoDB Shell
session for the change to take effect.

## Before you Begin

To check if log compression is enabled, run the following command from
MongoDB Shell:

```javascript
config.get("logCompressionEnabled")

```

## Steps

To enable log compression, set the `logCompressionEnabled`
configuration option to `true`. You can set configuration options in
the configuration API or a
configuration file.

### Enable Log Compression with the Configuration API

The following command uses the config API to enable log compression:

config.set("logCompressionEnabled", true)
Setting "logCompressionEnabled" has been changed
Enable Log Compression with a Configuration File
\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~~

The following configuration file enables log compression:

```yaml
mongosh:
  logCompressionEnabled: true

```

### Disable Log Compression

To disable log compression, set `logCompressionEnabled` to `false`.
You can perform this action through the config API or configuration
file.
