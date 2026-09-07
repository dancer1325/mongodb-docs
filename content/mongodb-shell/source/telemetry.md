# Configure Telemetry Options

`mongosh` collects anonymous aggregated usage data to improve
MongoDB products. `mongosh` collects this information by default, but
you can disable this data collection at any time.

## Data Tracked by `mongosh`

`mongosh` tracks the following data:

- The type of MongoDB `mongosh` is connected to. For example,
  Enterprise Edition or Community Edition.
- The methods run in `mongosh`. `mongosh` only tracks the names of
  the methods, and does not track any arguments supplied to methods.
- Whether users run scripts with `mongosh`.
- Errors.

`mongosh` *does not* track:

- IP addresses, hostnames, usernames, or credentials.
- Queries run in `mongosh`.
- Any data stored in your MongoDB deployment.
- Personal identifiable information.

For more information, see MongoDB's [Privacy Policy](https://www.mongodb.com/legal/privacy-policy).

## Toggle Telemetry Collection

Use the following methods in `mongosh` to toggle telemetry data
collection.

disableTelemetry()
Disable telemetry for `mongosh`.

```javascript
disableTelemetry()

```

The command response confirms that telemetry is disabled:

```javascript
Telemetry is now disabled.

```

> **Tip**
>
> You can also disable telemetry at startup by using the
> `--eval` startup option.
>
> The following command starts `mongosh` with telemetry disabled:
>
> ```javascript
mongosh --nodb --eval "disableTelemetry()"
```

enableTelemetry()
Enable telemetry for `mongosh`.

```javascript
enableTelemetry()

```

The command response confirms that telemetry is enabled:

```javascript
Telemetry is now enabled.
```
