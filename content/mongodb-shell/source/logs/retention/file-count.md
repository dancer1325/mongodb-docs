# Modify Maximum Log File Count

You can specify the maximum number of log files that the MongoDB Shell
retains. After the MongoDB Shell reaches the maximum log file count, it
starts deleting the oldest log files until the count is below the
threshold.

The MongoDB Shell stores one log file for each session. By default, the
MongoDB Shell stores up to 100 log files.

## About this Task

## Before you Begin

To check the current maximum log file count, run the following command:

```javascript
config.get("logMaxFileCount")

```

## Steps

To modify the maximum log file count, set the `logMaxFileCount`
configuration option. You can set configuration options in the
configuration API or a
configuration file.

### Modify Maximum Log File Count with the Configuration API

The following command uses the config API to set the maximum log file
count to 200:

config.set("logMaxFileCount", 200)
Setting "logMaxFileCount" has been changed
Modify Maximum Log File Count with a Configuration File
\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~~

The following configuration file sets the maximum log file count to 200:

```yaml
mongosh:
  logMaxFileCount: 200

```

### Disable Maximum Log File Limit

To disable the maximum log file limit, set `logMaxFileCount` to
`Infinity`. You can perform this action through the config API or
configuration file. For example:

```javascript
config.set("logMaxFileCount", Infinity)

```
