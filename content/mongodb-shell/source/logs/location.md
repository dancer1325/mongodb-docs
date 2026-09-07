# Specify Log File Location

You can specify where MongoDB Shell writes log files. By default,
MongoDB Shell saves the log for each session to your user's
`.mongodb/mongosh` directory, which depends on your operating system:

- - Operating System
  - Default Log Location
- - macOS and Linux
  - `~/.mongodb/mongosh/<LogID>_log`

\* - Windows  
- `%UserProfile%/AppData/Local/mongodb/mongosh/<LogID>_log`

## About this Task

To view the current log file location, use the config API to return the `logLocation` value:

```javascript
config.get("logLocation")

```

After you modify your log file location, you must start a new
MongoDB Shell session for the change to take effect.

### Log File Location

### Log File Names

If you modify the default log file location, log files have a
`mongosh_` prefix before the session ID. For example, the log for
session ID `67be0c0eb6227e211a1979e8` is saved as
`mongosh_67be0c0eb6227e211a1979e8_log`.

If you use the default log file location, the file name does not include
the `mongosh_` prefix. For example, the log for session ID
`67be0c0eb6227e211a1979e8` is saved as
`67be0c0eb6227e211a1979e8_log`.

## Steps

To change the log file location, modify the `logLocation`
configuration option. You can modify configuration options with the
configuration API or a configuration file.

> **Important**
>
> Specify `logLocation` as an absolute filepath.

### Modify Log Location with the API

The following command uses the config API to set the `logLocation`
setting to `/path/to/log/directory`:

config.set("logLocation", "/path/to/log/directory")
Setting "logLocation" has been changed
Modify Log Location with a Configuration File
\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~\~~

The following configuration file sets
the `logLocation` setting to `/path/to/log/directory`:

```yaml
mongosh:
  logLocation: "/path/to/log/directory"
```
