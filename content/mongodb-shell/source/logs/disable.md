# Disable Logging

To specify whether MongoDB Shell writes log messages, use the
`disableLogging` configuration option. Logging is enabled by default.

## About this Task

When you toggle logging, the change affects the current MongoDB Shell
session. You don't need to start a new session for the change to take
effect.

## Before you Begin

To check if logging is currently disabled, run the following command:

```javascript
config.get("disableLogging")

```

- If the command returns `true`, logging is disabled.
- If the command returns `false`, logging is enabled.

## Steps

You can set the `disableLogging` configuration option with the
configuration API or a
configuration file.

### Disable Logging with the Configuration API

The following command uses the config API to disable logging for
MongoDB Shell:

config.set("disableLogging", true)
Setting "disableLogging" has been changed
Disable Logging with a Configuration File
\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~~

The following configuration file disables logging for MongoDB Shell:

```yaml
mongosh:
  disableLogging: true

```

### Re-Enable Logging

To enable logging, set `disableLogging` to `false`. You can perform
this action through the config API or configuration file.
