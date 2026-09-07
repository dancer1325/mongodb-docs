# Configure Settings Using the API

The `config` API provides methods to examine and update the
`mongosh` configuration. Updates made using the `config`
API persist between sessions.

> **Note**
>
> The `config` API can be used within the `mongosh`
> command line interface. It has no effect in the
> \[embedded Compass shell\](<https://www.mongodb.com/docs/compass/embedded-shell/>).

## Syntax

Print the current `mongosh` configuration:

```javascript
config 

```

Return the current value for `<property>`:

```javascript
config.get( "<property>" )

```

Change the current setting of `<property>` to `<value>`:

```javascript
config.set( "<property>", <value> )

```

Reset a `<property>` to the default value:

```javascript
config.reset( "<property>" )

```

### Supported property parameters

## Behavior

### Remove or Redact Sensitive Information From History

`mongosh` makes "best-effort" attempts to match patterns
that normally correspond to certain kinds of sensitive information.

There are patterns that match:

- Certificates and keys
- Email addresses
- Generic user directories
- HTTP(s) URLs
- IP addresses
- MongoDB connection strings

Certain operations, such as `connect()`, are considered
inherently sensitive. If `redactHistory` is set to `remove` or
`remove-redact`, lines with these operations will be removed from the
command line history.

Other operations, like `find()`, sometimes have
sensitive information like email addresses. The
shell history will retain these
lines as entered unless `redactHistory` is set to `remove-redact`.

### Behavior with Configuration File

Settings specified with the `config` API:

- Override settings specified in the configuration file.
- Persist across restarts.

> **Example**
>
> Consider the following configuration file that sets the
> `inspectDepth` setting to `20`:
>
> ```yaml
mongosh:
  inspectDepth: 20
 
```
>
> During your `mongosh` session you run the following command to set
> `inspectDepth` to `10`:
>
> ```javascript
config.set( "inspectDepth", 10 )

```
>
> The value of `inspectDepth` becomes `10`, and will remain `10`
> even when `mongosh` is restarted.

## Examples

### Update Number of Items Returned by a Cursor

Consider viewing a collection with a number of large documents. You can
update the `batchSize` to limit the number of items returned by a
cursor.

```javascript
config.set("displayBatchSize", 3) 


```

Future `db.collection.find()` operations will only return 3 documents
per cursor iteration.

### Turn On Stack Traces

Enable stack traces to see more detailed error reporting.

```javascript
config.set("showStackTraces", true)

```

The output differs like this:

```javascript
// showStackTraces set to 'false'
Enterprise> db.orders.find( {}, { $thisWontWork: 1 } )
MongoError: FieldPath field names may not start with '$'.

// showStackTraces set to 'true'
Enterprise> db.orders.find( {}, { $thisWontWork: 1 } )
Uncaught:
MongoError: FieldPath field names may not start with '$'.
    at MessageStream.messageHandler (/usr/bin/mongosh:58878:20)
    at MessageStream.emit (events.js:315:20)
    at MessageStream.EventEmitter.emit (domain.js:548:15)
    at processIncomingData (/usr/bin/mongosh:57954:12)
    at MessageStream._write (/usr/bin/mongosh:57850:5)
    at writeOrBuffer (_stream_writable.js:352:12)
    at MessageStream.Writable.write (_stream_writable.js:303:10)
    at Socket.ondata (_stream_readable.js:719:22)
    at Socket.emit (events.js:315:20)
    at Socket.EventEmitter.emit (domain.js:548:15)

```

### Call config API from outside mongosh

You can call the `config` API from the command line using
`--eval` with `mongosh`. In this case the
`--nodb` option means `mongosh` will update
without connecting to a MongoDB database.

> **Important**
>
> You must use different quotation marks for the `--eval`
> expression and the `config` property. That is, single quotes for
> one and double quotes for the other.

```
mongosh --nodb --eval 'config.set("enableTelemetry", true)'

```

`mongosh` returns additional information along with the
result of the API call.

```
Current Mongosh Log ID:	609583b730e14918fa0d363f
Using MongoDB:		undefined
Using Mongosh Beta:	0.12.1

For mongosh info see: https://www.mongodb.com/docs/mongodb-shell/

Setting "enableTelemetry" has been changed

```

### Redact Sensitive Information

Compare the recalled history when `redactHistory` is set to
`remove-redact` or `remove`.

Set `redactHistory` to `remove-redact` mode and enter a query
containing an email address.

```javascript
config.set( "redactHistory", "remove-redact" )
db.contacts.find( {"email": "customer@clients.com" } )

```

When you press the `up arrow` to replay the last command the email
address is redacted.

```javascript
db.contacts.find( {"email": "<email>" } )  // Redacted

```

Set `redactHistory` to `remove` mode and enter a query containing
an email address.

```javascript
config.set( "redactHistory", "remove" )
db.contacts.find( {"email": "customer@clients.com" } )

```

When you press the `up arrow` to replay the last command the email
address is present.

```javascript
db.contacts.find( {"email": "customer@clients.com" } )

```

The shell history reflects the
changes. (Note that this stores the most recent input first.)

```javascript
db.contacts.find( {"email": "customer@clients.com" } )
config.set( "redactHistory", "remove" )
db.contacts.find( {"email": "<email>" } )
config.set( "redactHistory", "remove-redact" )

```

### Reset a Configuration Setting to the Default Value

If you modified a configuration setting and want to reset it to the
default value, use `config.reset( "<property>" )`.

1.  Change the value of the `historyLength` setting to `2000`:

```javascript
config.set("historyLength", 2000)

```

1.  Verify the updated value for `historyLength`:

```javascript
config.get("historyLength")

```

1.  Reset the `historyLength` setting to the default value of `1000`:

```javascript
config.reset("historyLength")

```

1.  Verify the updated value for `historyLength`:

```javascript
config.get("historyLength")
```
