# Modify Maximum Log Storage Size

You can specify a maximum combined storage size for MongoDB Shell log
files. If the total size of all log files exceeds the maximum, log files
are deleted until the combined size is below the threshold, starting
with the oldest log files. By default, there is no maximum log storage
size.

## About this Task

To specify a maximum log storage size, set the `logRetentionGB`
configuration option. `logRetentionGB` can be any positive float
value (including less than `1`).

To check the current storage size of log files, check the size of the
log folder. To see the current log folder, run the following command
from the MongoDB Shell:

```javascript
config.get('logLocation')

```

## Before you Begin

To check the current maximum log storage size, run the following
command:

```javascript
config.get("logRetentionGB")

```

## Steps

To modify maximum log storage size, set the `logRetentionGB`
configuration option. You can set configuration options in the
configuration API or a
configuration file.

### Modify Maximum Log Storage Size with the API

The following command uses the config API to set the maximum log storage
size to 3.5 GB:

config.set("logRetentionGB", 3.5)
Setting "logRetentionGB" has been changed
Modify Maximum Log Storage Size with a Configuration File
\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~~

The following configuration file sets the maximum log storage size to
3.5 GB:

```yaml
mongosh:
  logRetentionGB: 3.5

```

### Disable Maximum Log Storage Size

To instruct the MongoDB Shell to not delete logs based on storage size,
set `logRetentionGB` to `Infinity`. You can perform this action
through the config API or configuration file. For example:

```javascript
config.set("logRetentionGB", Infinity)

```
