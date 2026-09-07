# Modify Log Retention Duration

You can modify how long MongoDB Shell log files are retained. A log
cleanup process automatically deletes log files older than the specified
retention period. By default, log files are retained for 30 days.

## About this Task

## Before you Begin

To check the current log retention duration, run the following command:

```javascript
config.get("logRetentionDays")

```

## Steps

To modify how long log files are retained, set the `logRetentionDays`
configuration option. You can set configuration options in the
configuration API or a configuration file.

### Modify Log Duration with the Configuration API

The following command uses the config API to set log retention to 60
days:

config.set("logRetentionDays", 60)
Setting "logRetentionDays" has been changed
Modify Log Duration with a Configuration File
\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~~

The following configuration file sets log retention to 60
days:

```yaml
mongosh:
  logRetentionDays: 60

```

### Disable Duration-Based Log Cleanup

To instruct MongoDB Shell to not delete logs based on file age, set
`logRetentionDays` to `Infinity`. You can perform this action
through the config API or configuration file. For example:

```javascript
config.set("logRetentionDays", Infinity)

```
