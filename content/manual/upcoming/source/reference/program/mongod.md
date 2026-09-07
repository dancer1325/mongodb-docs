# `mongod`

## Synopsis

`mongod` is the primary daemon process for the MongoDB
system. It handles data requests, manages data access, and performs
background management operations.

This document provides a complete overview of all command line options
for `mongod`. These command line options are primarily useful
for testing: In common operation, use the \[configuration file options\](/reference/configuration-options.md) to control the behavior of
your database.

> **See also**
>
> conf-file-command-line-mapping

> **Note**
>

## Compatibility

Deployments hosted in the following environments use `mongod`:

> **Note**
>
> MongoDB Atlas manages the `mongod` for all MongoDB Atlas deployments.

## Considerations

- 

## Options

> **Changed in version 6.1**
>
> \- .. include:: /includes/journal-always-enabled-change.rst

> **Changed in version 5.2**
>
> \- MongoDB removes the `--cpu` command-line option.

> **Changed in version 5.0**
>
> \- MongoDB removes the `--serviceExecutor` command-line option and the  
> corresponding `net.serviceExecutor` configuration option.

### Core Options

**\`--auth\`**

Enables authorization to control user's access to database resources
and operations. When authorization is enabled, MongoDB requires all
clients to authenticate themselves first in order to determine the
access for the client.

To configure users, use the `mongosh` client. If no users
exist, the localhost interface has access to the
database until you create the first user.

See Security for more information.

**\`--bind_ip \<hostnames\|ipaddresses\|Unix domain socket paths\>\`**

*Default*: localhost

The hostnames and/or IP addresses and/or full Unix domain socket
paths on which `mongod` should listen for client connections. You
may attach `mongod` to any interface. To bind to multiple
addresses, enter a list of comma-separated values.

> **Example `localhost,/tmp/mongod.sock`**
>

You can specify both IPv4 and IPv6 addresses, or hostnames that
resolve to an IPv4 or IPv6 address.

> **Example `localhost, 2001:0DB8:e132:ba26:0d5c:2774:e7f9:d513`**
>

> **Note**
>
> If specifying an IPv6 address *or* a hostname that resolves to an
> IPv6 address to `--bind_ip`, you must start `mongod` with
> `--ipv6` to enable IPv6 support. Specifying an IPv6 address
> to `--bind_ip` does not enable IPv6 support.

If specifying a
[link-local IPv6 address](https://en.wikipedia.org/wiki/Link-local_address#IPv6)
(`fe80::/10`), you must append the
[zone index](https://en.wikipedia.org/wiki/IPv6_address#Scoped_literal_IPv6_addresses_(with_zone_index))
to that address (i.e. `fe80::<address>%<adapter-name>`).

> **Example `localhost,fe80::a00:27ff:fee0:1fcf%enp0s3`**
>

For more information about IP Binding, refer to the
\[security-mongodb-configuration\](/core/security-mongodb-configuration.md) documentation.

To bind to all IPv4 addresses, enter `0.0.0.0`.

To bind to all IPv4 and IPv6 addresses, enter `::,0.0.0.0` or
an asterisk `"*"` (enclose the asterisk in quotes to avoid filename
pattern expansion). Alternatively, use the `net.bindIpAll` setting.

> **Note**
>
> - `--bind_ip` and `--bind_ip_all` are mutually exclusive.
>   Specifying both options causes `mongod` to throw an error and
>   terminate.
>
> \- The command-line option `--bind` overrides the configuration  
> file setting `net.bindIp`.

**\`--bind_ip_all\`**

If specified, the `mongod` instance binds to all IPv4
addresses (i.e. `0.0.0.0`). If `mongod` starts with
`--ipv6`, `--bind_ip_all` also binds to all IPv6 addresses
(i.e. `::`).

`mongod` only supports IPv6 if started with `--ipv6`. Specifying
`--bind_ip_all` alone does not enable IPv6 support.

For more information about IP Binding, refer to the
\[security-mongodb-configuration\](/core/security-mongodb-configuration.md) documentation.

Alternatively, you can set the `--bind_ip` option to `::,0.0.0.0`
or to an asterisk `"*"` (enclose the asterisk in quotes to avoid filename
pattern expansion).

> **Note**
>
> `--bind_ip` and `--bind_ip_all` are mutually exclusive. That
> is, you can specify one or the other, but not both.

**\`--clusterIpSourceAllowlist \<string\>\`**

> **New in version 5.0**
>

A list of IP addresses/CIDR ([Classless Inter-Domain Routing](https://tools.ietf.org/html/rfc4632)) ranges against which the
`mongod` validates authentication requests from other members of
the replica set and, if part of a sharded cluster, the `mongos`
instances. The `mongod` verifies that the originating IP is
either explicitly in the list or belongs to a CIDR range in the list. If the
IP address is not present, the server does not authenticate the
`mongod` or `mongos`.

`--clusterIpSourceAllowlist` has no effect on a `mongod` started without
authentication.

`--clusterIpSourceAllowlist` accepts multiple comma-separated IPv4/6 addresses or Classless
Inter-Domain Routing ([CIDR](https://tools.ietf.org/html/rfc4632)) ranges:

```bash
mongod --clusterIpSourceAllowlist 192.0.2.0/24,127.0.0.1,::1


```

> **Important**
>
> Ensure `--clusterIpSourceAllowlist` includes the IP address *or* CIDR ranges that include the
> IP address of each replica set member or `mongos` in the
> deployment to ensure healthy communication between cluster components.

**\`--config \<filename\>, -f \<filename\>\`**

Specifies a configuration file for runtime configuration options. The
configuration file is the preferred method for runtime configuration of
`mongod`. The options are equivalent to the command-line
configuration options. See \[configuration-options\](/reference/configuration-options.md) for
more information.

Ensure the configuration file uses ASCII encoding. The `mongod`
instance does not support configuration files with non-ASCII encoding,
including UTF-8.

**\`--configExpand \<none\|rest\|exec\>\`**

*Default*: none

Enables using Expansion Directives
in configuration files. Expansion directives allow you to set
externally sourced values for configuration file options.

`--configExpand` supports the following expansion directives:

- - Value
  - Description
- - `none`
  - Default. `mongod` does not expand expansion directives.
    `mongod` fails to start if any configuration file settings
    use expansion directives.
- - `rest`
  - `mongod` expands `__rest` expansion directives when
    parsing the configuration file.

\* - `exec`

> - `mongod` expands `__exec` expansion directives when
>   parsing the configuration file.

You can specify multiple expansion directives as a comma-separated
list, for example: `rest, exec`. If the configuration file contains
expansion directives not specified to `--configExpand`, the `mongod`
returns an error and terminates.

See externally-sourced-values for configuration files
for more information on expansion directives.

**\`--filePermissions \<path\>\`**

*Default*: `0700`

Sets the permission for the UNIX domain socket file.

`--filePermissions` applies only to Unix-based systems.

**\`--fork\`**

Enables a daemon mode that runs the `mongod` process in the
background. The `--fork` option is not supported on Windows.

By default `mongod` does not run as a daemon. You run `mongod` as
a daemon by using either `--fork` or a controlling process
that handles daemonization, such as `upstart` or `systemd`.

To use `--fork`, configure log output for the `mongod` with one of the following:

- `--logpath`
- `--syslog`

**\`--help, -h\`**

Returns information on the options and use of `mongod`.

**\`--ipv6\`**

Enables IPv6 support. `mongod` disables IPv6 support by default.

Setting `--ipv6` does *not* direct the `mongod` to listen on any
local IPv6 addresses or interfaces. To configure the `mongod` to
listen on an IPv6 interface, you must either:

- Configure `--bind_ip` with one or more IPv6 addresses or
  hostnames that resolve to IPv6 addresses, **or**
- Set `--bind_ip_all` to `true`.

**\`--keyFile \<file\>\`**

Specifies the path to a key file that stores the shared secret
that MongoDB instances use to authenticate to each other in a
sharded cluster or replica set. `--keyFile` implies
`--auth`. See inter-process-auth for more
information.

**\`--logappend\`**

Appends new entries to the end of the existing log file when the `mongod`
instance restarts. Without this option, `mongod` backs up the
existing log and create a new file.

**\`--logpath \<path\>\`**

Sends all diagnostic logging information to a log file instead of to
standard output or to the host's syslog system. MongoDB creates
the log file at the path you specify.

By default, MongoDB moves any existing log file rather than overwriting
it. To instead append to the log file, set the `--logappend` option.

**\`--logRotate \<string\>\`**

*Default*: rename

Determines the behavior for the `logRotate` command when
rotating the server log and/or the audit log. Specify either
`rename` or `reopen`:

- `rename` renames the log file.

- `reopen` closes and reopens the log file following the typical
  Linux/Unix log rotate behavior. Use `reopen` when using the
  Linux/Unix logrotate utility to avoid log loss.

  If you specify `reopen`, you must also use `--logappend`.

**\`--maxConns \<number\>\`**

The maximum number of simultaneous connections that `mongod`
accepts. This setting has no effect if it is higher than your operating
system's configured maximum connection tracking threshold.

Do not assign too low of a value to this option, or you will
encounter errors during normal application operation.

**\`--networkMessageCompressors \<string\>\`**

*Default*: snappy,zstd,zlib

Specifies the default compressor(s) to use for
communication between this `mongod` instance and:

- other members of the deployment if the instance is part of a replica set or a sharded cluster
- `mongosh`
- drivers that support the `OP_COMPRESSED` message format.

MongoDB supports the following compressors:

- snappy
- zlib
- zstd

> **Note**
>
> Both `mongod` and `mongos` instances default to
> `snappy,zstd,zlib` compressors, in that order.

To disable network compression, set the value to `disabled`.

**\`--noauth\`**

Disables authentication. Currently the default. Exists for future
compatibility and clarity.

**\`--noscripting\`**

Disables the scripting engine.

**\`--notablescan\`**

Forbids operations that require a collection scan. See `notablescan` for additional information.

**\`--nounixsocket\`**

Disables listening on the UNIX domain socket. `--nounixsocket` applies only
to Unix-based systems.

The `mongod` process
always listens on the UNIX socket unless one of the following is true:

- `--nounixsocket` is set
- `net.bindIp` is not set
- `net.bindIp` does not specify `localhost` or its associated IP address

**\`--outputConfig\`**

Outputs the `mongod` instance's configuration options, formatted
in YAML, to `stdout` and exits the `mongod` instance. For
configuration options that uses externally-sourced-values,
`--outputConfig` returns the resolved value for those options.

> **Warning**
>
> This may include any configured passwords or secrets previously
> obfuscated through the external source.

For usage examples, see:

- expansion-directive-output
- \[convert-command-line-options-to-yaml\](/tutorial/convert-command-line-options-to-yaml.md)

**\`--pidfilepath \<path\>\`**

Specifies a file location to store the process ID (PID) of the `mongod`
process. The user running the `mongod` or `mongos`
process must be able to write to this path. If the `--pidfilepath` option is not
specified, the process does not create a PID file. This option is generally
only useful in combination with the `--fork` option.

> **Note Linux**
>
> On Linux, PID file management is generally the responsibility of
> your distro's init system: usually a service file in the `/etc/init.d`
> directory, or a systemd unit file registered with `systemctl`. Only
> use the `--pidfilepath` option if you are not using one of these init
> systems. For more information, please see the respective
> Installation Guide for your operating system.

> **Note macOS**
>
> On macOS, PID file management is generally handled by `brew`. Only use
> the `--pidfilepath` option if you are not using `brew` on your macOS system.
> For more information, please see the respective Installation
> Guide for your operating system.

**\`--port \<port\>\`**

*Default*:

- 27017 if `mongod` is not a shard member or a config server member
- 27018 if `mongod` is a `shard member`
- 27019 if `mongod` is a `config server member`

The TCP port on which the MongoDB instance listens for
client connections.

**\`--quiet\`**

Runs `mongod` in a quiet mode that attempts to limit the amount
of output.

This option suppresses:

- output from database commands
- replication activity
- connection accepted events
- connection closed events
- client metadata

**\`--redactClientLogData\`**

*Available in MongoDB Enterprise only.*

A `mongod` running with `--redactClientLogData` redacts any message accompanying a given
log event before logging. This prevents the `mongod` from writing
potentially sensitive data stored on the database to the diagnostic log.
Metadata such as error or operation codes, line numbers, and source file
names are still visible in the logs.

Use `--redactClientLogData` in conjunction with
\[security-encryption-at-rest\](/core/security-encryption-at-rest.md) and
\[security-transport-encryption\](/core/security-transport-encryption.md) to assist compliance with
regulatory requirements.

For example, a MongoDB deployment might store Personally Identifiable
Information (PII) in one or more collections. The `mongod` logs events
such as those related to CRUD operations, sharding metadata, etc. It is
possible that the `mongod` may expose PII as a part of these logging
operations. A `mongod` running with `--redactClientLogData` removes any message
accompanying these events before being output to the log, effectively
removing the PII.

Diagnostics on a `mongod` running with `--redactClientLogData` may be more difficult
due to the lack of data related to a log event. See the
process logging manual page for an
example of the effect of `--redactClientLogData` on log output.

On a running `mongod`, use `setParameter` with the
`redactClientLogData` parameter to configure this setting.

**\`--setParameter \<options\>\`**

Specifies one of the MongoDB parameters described in
\[parameters\](/reference/parameters.md). You can specify multiple `setParameter`
fields.

**\`--shutdown\`**

The `--shutdown` option cleanly and safely terminates the `mongod`
process. When invoking `mongod` with this option you must set the
`--dbpath` option either directly or by way of the
configuration file and the
`--config` option.

The `--shutdown` option is available only on Linux systems.

For additional ways to shut down, see also terminate-mongod-processes.

**\`--sysinfo\`**

Returns diagnostic system information and then exits. The
information provides the page size, the number of physical pages,
and the number of available physical pages.

**\`--syslog\`**

Sends all logging output to the host's syslog system rather
than to standard output or to a log file (`--logpath`).

The `--syslog` option is not supported on Windows.

> **Warning**
>
> The `syslog` daemon generates timestamps when it logs a message, not
> when MongoDB issues the message. This can lead to misleading timestamps
> for log entries, especially when the system is under heavy load. We
> recommend using the `--logpath` option for production systems to
> ensure accurate timestamps.

```none
...  ACCESS   [repl writer worker 5] Unsupported modification to roles collection ...

```

**\`--syslogFacility \<string\>\`**

*Default*: user

Specifies the facility level used when logging messages to syslog.
The value you specify must be supported by your
operating system's implementation of syslog. To use this option, you
must enable the `--syslog` option.

**\`--timeStampFormat \<string\>\`**

*Default*: iso8601-local

The time format for timestamps in log messages. Specify one of the
following values:

- - Value
  - Description
- - `iso8601-utc`
  - Displays timestamps in Coordinated Universal Time (UTC) in the
    ISO-8601 format. For example, for New York at the start of the
    Epoch: `1970-01-01T00:00:00.000Z`
- - `iso8601-local`
  - Displays timestamps in local time in the ISO-8601
    format. For example, for New York at the start of the Epoch:
    `1969-12-31T19:00:00.000-05:00`

> **Note**
>

**\`--timeZoneInfo \<path\>\`**

```bash
wget https://downloads.mongodb.org/olson_tz_db/timezonedb-latest.zip
unzip timezonedb-latest.zip
mongod --timeZoneInfo timezonedb-2017b/

```

> **See also**
>
> `processManagement.timeZoneInfo`.

**\`--traceExceptions\`**

For internal diagnostic use only.

**\`--transitionToAuth\`**

Allows the `mongod` to accept and create authenticated and
non-authenticated connections to and from other `mongod`
and `mongos` instances in the deployment. Used for
performing rolling transition of replica sets or sharded clusters
from a no-auth configuration to internal authentication. Requires specifying a internal authentication mechanism such as
`--keyFile`.

For example, if using keyfiles for
internal authentication, the `mongod` creates
an authenticated connection with any `mongod` or `mongos`
in the deployment using a matching keyfile. If the security mechanisms do
not match, the `mongod` utilizes a non-authenticated connection instead.

A `mongod` running with `--transitionToAuth` does not enforce user access controls. Users may connect to your deployment without any
access control checks and perform read, write, and administrative operations.

> **Note**
>
> A `mongod` running with internal authentication and *without* `--transitionToAuth` requires clients to connect
> using user access controls. Update clients to
> connect to the `mongod` using the appropriate user
> prior to restarting `mongod` without `--transitionToAuth`.

**\`--unixSocketPrefix \<path\>\`**

*Default*: /tmp

The path for the UNIX socket. `--unixSocketPrefix` applies only
to Unix-based systems.

If this option has no value, the
`mongod` process creates a socket with `/tmp` as a prefix. MongoDB
creates and listens on a UNIX socket unless one of the following is true:

- `net.unixDomainSocket.enabled` is `false`
- `--nounixsocket` is set
- `net.bindIp` is not set
- `net.bindIp` does not specify `localhost` or its associated IP address

**\`--verbose, -v\`**

Increases the amount of internal reporting returned on standard output
or in log files. Increase the verbosity with the `-v` form by
including the option multiple times, for example: `-vvvvv`.

> **Note**
>

**\`--version\`**

Returns the `mongod` release number.

### LDAP Authentication or Authorization Options

**\`--ldapServers \<host1\>:\<port\>,\<host2\>:\<port\>,...,\<hostN\>:\<port\>\`**

*Available in MongoDB Enterprise only.*

The LDAP server against which the `mongod` authenticates users or
determines what actions a user is authorized to perform on a given
database. If the LDAP server specified has any replicated instances,
you may specify the host and port of each replicated server in a
comma-delimited list.

If your LDAP infrastructure partitions the LDAP directory over multiple LDAP
servers, specify *one* LDAP server or any of its replicated instances to
`--ldapServers`. MongoDB supports following LDAP referrals as defined in [RFC 4511
4.1.10](https://www.rfc-editor.org/rfc/rfc4511.txt). Do not use `--ldapServers`
for listing every LDAP server in your infrastructure.

This setting can be configured on a running `mongod` using
`setParameter`.

If unset, `mongod` cannot use \[LDAP authentication or authorization\](/core/security-ldap.md).

**\`--ldapValidateLDAPServerConfig \<boolean\>\`**

*Available in MongoDB Enterprise*

A flag that determines if the `mongod` instance checks
the availability of the `LDAP server(s)` as part of its startup:

- If `true`, the `mongod` instance performs the
  availability check and only continues to start up if the LDAP
  server is available.
- If `false`, the `mongod` instance skips the
  availability check; i.e. the instance starts up even if the LDAP
  server is unavailable.

**\`--ldapQueryUser \<string\>\`**

*Available in MongoDB Enterprise only.*

The identity with which `mongod` binds as, when connecting to or
performing queries on an LDAP server.

Only required if any of the following are true:

- Using LDAP authorization.
- Using an LDAP query for `username transformation`.
- The LDAP server disallows anonymous binds

You must use `--ldapQueryUser` with `--ldapQueryPassword`.

If unset, `mongod` doesn't attempt to bind to the LDAP server.

This setting can be configured on a running `mongod` using
`setParameter`.

> **Note**
>
> Windows MongoDB deployments can use `--ldapBindWithOSDefaults`
> instead of `--ldapQueryUser` and `--ldapQueryPassword`. You cannot specify
> both `--ldapQueryUser` and `--ldapBindWithOSDefaults` at the same time.

**\`--ldapQueryPassword \<string \| array\>\`**

**\`--ldapBindWithOSDefaults \<bool\>\`**

*Default*: false

Available in MongoDB Enterprise for the Windows platform only.

Allows `mongod` to authenticate, or bind, using your Windows login
credentials when connecting to the LDAP server.

Only required if:

- Using LDAP authorization.
- Using an LDAP query for `username transformation`.
- The LDAP server disallows anonymous binds

Use `--ldapBindWithOSDefaults` to replace `--ldapQueryUser` and
`--ldapQueryPassword`.

**\`--ldapBindMethod \<string\>\`**

*Default*: simple

*Available in MongoDB Enterprise only.*

The method `mongod` uses to authenticate to an LDAP server.
Use with `--ldapQueryUser` and `--ldapQueryPassword` to
connect to the LDAP server.

`--ldapBindMethod` supports the following values:

- `simple` - `mongod` uses simple authentication.
- `sasl` - `mongod` uses SASL protocol for authentication

If you specify `sasl`, you can configure the available SASL mechanisms
using `--ldapBindSaslMechanisms`. `mongod` defaults to
using `DIGEST-MD5` mechanism.

**\`--ldapBindSaslMechanisms \<string\>\`**

*Default*: DIGEST-MD5

*Available in MongoDB Enterprise only.*

A comma-separated list of SASL mechanisms `mongod` can
use when authenticating to the LDAP server. The `mongod` and the
LDAP server must agree on at least one mechanism. The `mongod`
dynamically loads any SASL mechanism libraries installed on the host
machine at runtime.

Install and configure the appropriate libraries for the selected
SASL mechanism(s) on both the `mongod` host and the remote
LDAP server host. Your operating system may include certain SASL
libraries by default. Defer to the documentation associated with each
SASL mechanism for guidance on installation and configuration.

If using the `GSSAPI` SASL mechanism for use with
security-kerberos, verify the following for the
`mongod` host machine:

`Linux`  
- The `KRB5_CLIENT_KTNAME` environment
  variable resolves to the name of the client keytab-files
  for the host machine. For more on Kerberos environment
  variables, please defer to the
  [Kerberos documentation](https://web.mit.edu/kerberos/krb5-1.13/doc/admin/env_variables.html).
- The client keytab includes a
  kerberos-user-principal for the `mongod` to use when
  connecting to the LDAP server and execute LDAP queries.

`Windows`  
If connecting to an Active Directory server, the Windows
Kerberos configuration automatically generates a
[Ticket-Granting-Ticket](https://msdn.microsoft.com/en-us/library/windows/desktop/aa380510(v=vs.85).aspx)
when the user logs onto the system. Set `--ldapBindWithOSDefaults` to
`true` to allow `mongod` to use the generated credentials when
connecting to the Active Directory server and execute queries.

Set `--ldapBindMethod` to `sasl` to use this option.

> **Note**
>
> For a complete list of SASL mechanisms see the
> [IANA listing](http://www.iana.org/assignments/sasl-mechanisms/sasl-mechanisms.xhtml).
> Defer to the documentation for your LDAP or Active Directory
> service for identifying the SASL mechanisms compatible with the
> service.
>
> MongoDB is not a source of SASL mechanism libraries, nor
> is the MongoDB documentation a definitive source for
> installing or configuring any given SASL mechanism. For
> documentation and support, defer to the SASL mechanism
> library vendor or owner.
>
> For more information on SASL, defer to the following resources:
>
> - For Linux, please see the [Cyrus SASL documentation](https://www.cyrusimap.org/sasl/).
>
> \- For Windows, please see the [Windows SASL documentation](https://msdn.microsoft.com/en-us/library/cc223500.aspx).

**\`--ldapTransportSecurity \<string\>\`**

*Default*: tls

*Available in MongoDB Enterprise only.*

By default, `mongod` creates a TLS/SSL secured connection to the LDAP
server.

For Linux deployments, you must configure the appropriate TLS Options in
`/etc/openldap/ldap.conf` file. Your operating system's package manager
creates this file as part of the MongoDB Enterprise installation, via the
`libldap` dependency. See the documentation for `TLS Options` in the
[ldap.conf OpenLDAP documentation](http://www.openldap.org/software/man.cgi?query=ldap.conf&manpath=OpenLDAP+2.4-Release)
for more complete instructions.

For Windows deployment, you must add the LDAP server CA certificates to the
Windows certificate management tool. The exact name and functionality of the
tool may vary depending on operating system version. Please see the
documentation for your version of Windows for more information on
certificate management.

Set `--ldapTransportSecurity` to `none` to disable TLS/SSL between `mongod` and the LDAP
server.

> **Warning**
>
> Setting `--ldapTransportSecurity` to `none` transmits plaintext information and possibly
> credentials between `mongod` and the LDAP server.

**\`--ldapTimeoutMS \<int\>\`**

*Default*: 10000

*Available in MongoDB Enterprise only.*

The amount of time in milliseconds `mongod` should wait for an LDAP server
to respond to a request.

Increasing the value of `--ldapTimeoutMS` may prevent connection failure between the
MongoDB server and the LDAP server, if the source of the failure is a
connection timeout. Decreasing the value of `--ldapTimeoutMS` reduces the time
MongoDB waits for a response from the LDAP server.

This setting can be configured on a running `mongod` using
`setParameter`.

**\`--ldapRetryCount \<int\>\`**

> **New in version 6.1**
>

*Default*: 0

*Available in MongoDB Enterprise only.*

Number of operation retries by the server LDAP manager after a
network error.

**\`--ldapUserToDNMapping \<string\>\`**

*Available in MongoDB Enterprise only.*

Maps the username provided to `mongod` for authentication to a LDAP
Distinguished Name (DN). You may need to use `--ldapUserToDNMapping` to transform a
username into an LDAP DN in the following scenarios:

- Performing LDAP authentication with simple LDAP binding, where users
  authenticate to MongoDB with usernames that are not full LDAP DNs.
- Using an `LDAP authorization query template` that requires a DN.
- Transforming the usernames of clients authenticating to Mongo DB
  using different authentication mechanisms, such as x.509 or
  kerberos, to a full LDAP DN for authorization.

`--ldapUserToDNMapping` expects a quote-enclosed JSON-string representing an ordered array
of documents. Each document contains a regular expression `match` and
either a `substitution` or `ldapQuery` template used for transforming the
incoming username.

Each document in the array has the following form:

```javascript
{
  match: "<regex>"
  substitution: "<LDAP DN>" | ldapQuery: "<LDAP Query>"
}

```

- - Field
  - Description
  - Example

- - `match`
  - An ECMAScript-formatted regular expression (regex) to match against a
    provided username. Each parenthesis-enclosed section represents a
    regex capture group used by `substitution` or `ldapQuery`.
  - `"(.+)ENGINEERING"`
    `"(.+)DBA"`

- - `substitution`

  - An LDAP distinguished name (DN) formatting template that converts the
    authentication name matched by the `match` regex into a LDAP DN.
    Each curly bracket-enclosed numeric value is replaced by the
    corresponding [regex capture group](http://www.regular-expressions.info/refcapture.html) extracted
    from the authentication username via the `match` regex.

    The result of the substitution must be an [RFC4514](https://www.ietf.org/rfc/rfc4514.txt) escaped string.

  - `"cn={0},ou=engineering, dc=example,dc=com"`

- - `ldapQuery`
  - A LDAP query formatting template that inserts the authentication
    name matched by the `match` regex into an LDAP query URI encoded
    respecting RFC4515 and RFC4516. Each curly bracket-enclosed numeric
    value is replaced by the corresponding [regex capture group](http://www.regular-expressions.info/refcapture.html) extracted
    from the authentication username via the `match` expression.
    `mongod` executes the query against the LDAP server to retrieve
    the LDAP DN for the authenticated user. `mongod` requires
    exactly one returned result for the transformation to be
    successful, or `mongod` skips this transformation.
  - `"ou=engineering,dc=example, dc=com??one?(user={0})"`

> **Note**
>
> An explanation of [RFC4514](https://www.ietf.org/rfc/rfc4514.txt),
> [RFC4515](https://tools.ietf.org/html/rfc4515),
> [RFC4516](https://tools.ietf.org/html/rfc4516), or LDAP queries is out
> of scope for the MongoDB Documentation. Please review the RFC directly or
> use your preferred LDAP resource.

For each document in the array, you must use either `substitution` or
`ldapQuery`. You *cannot* specify both in the same document.

When performing authentication or authorization, `mongod` steps through
each document in the array in the given order, checking the authentication
username against the `match` filter. If a match is found,
`mongod` applies the transformation and uses the output for
authenticating the user. `mongod` does not check the remaining documents
in the array.

If the given document does not match the provided authentication
name, `mongod` continues through the list of documents
to find additional matches. If no matches are found in any document,
or the transformation the document describes fails,
`mongod` returns an error.

`mongod` also returns an error if one of the transformations cannot be
evaluated due to networking or authentication failures to the LDAP server.
`mongod` rejects the connection request and does not check the remaining
documents in the array.

Starting in MongoDB 5.0, `--ldapUserToDNMapping`
accepts an empty string `""` or empty array `[ ]` in place of a
mapping documnent. If providing an empty string or empty array to
`--ldapUserToDNMapping`, MongoDB maps the
authenticated username as the LDAP DN. In earlier versions, providing
an empty mapping document causes mapping to fail.

> **Example**
>
> The following shows two transformation documents. The first
> document matches against any string ending in `@ENGINEERING`, placing
> anything preceeding the suffix into a regex capture group. The
> second document matches against any string ending in `@DBA`, placing
> anything preceeding the suffix into a regex capture group.
>
> 

```text
"[
   {
      match: "(.+)@ENGINEERING.EXAMPLE.COM",
      substitution: "cn={0},ou=engineering,dc=example,dc=com"
   },
   {
      match: "(.+)@DBA.EXAMPLE.COM",
      ldapQuery: "ou=dba,dc=example,dc=com??one?(user={0})"

   }

]"

```

A user with username `alice@ENGINEERING.EXAMPLE.COM` matches the first
document. The regex capture group `{0}` corresponds to the string
`alice`. The resulting output is the DN
`"cn=alice,ou=engineering,dc=example,dc=com"`.

A user with username `bob@DBA.EXAMPLE.COM` matches the second document.
The regex capture group `{0}` corresponds to the string `bob`. The
resulting output is the LDAP query
`"ou=dba,dc=example,dc=com??one?(user=bob)"`. `mongod` executes this
query against the LDAP server, returning the result
`"cn=bob,ou=dba,dc=example,dc=com"`.


If `--ldapUserToDNMapping` is unset, `mongod` applies no transformations to the username
when attempting to authenticate or authorize a user against the LDAP server.

This setting can be configured on a running `mongod` using the
`setParameter` database command.

**\`--ldapAuthzQueryTemplate \<string\>\`**

*Available in MongoDB Enterprise only.*

A relative LDAP query URL formatted conforming to [RFC4515](https://tools.ietf.org/html/rfc4515) and [RFC4516](https://tools.ietf.org/html/rfc4516) that `mongod` executes to obtain
the LDAP groups to which the authenticated user belongs to. The query is
relative to the host or hosts specified in `--ldapServers`.

In the URL, you can use the following substituion tokens:

- - Substitution Token
  - Description
- - `{USER}`
  - Substitutes the authenticated username, or the
    `transformed`
    username if a `username mapping` is specified.

\* - `{PROVIDED_USER}`

> - Substitutes the supplied username, i.e. before either
>   authentication or `LDAP transformation`.

When constructing the query URL, ensure that the order of LDAP parameters
respects RFC4516:

```bash
[ dn  [ ? [attributes] [ ? [scope] [ ? [filter] [ ? [Extensions] ] ] ] ] ]

```

If your query includes an attribute, `mongod` assumes that the query
retrieves a the DNs which this entity is member of.

If your query does not include an attribute, `mongod` assumes
the query retrieves all entities which the user is member of.

For each LDAP DN returned by the query, `mongod` assigns the authorized
user a corresponding role on the `admin` database. If a role on the on the
`admin` database exactly matches the DN, `mongod` grants the user the
roles and privileges assigned to that role. See the
`db.createRole()` method for more information on creating roles.

> **Example**
>
> This LDAP query returns any groups listed in the LDAP user object's
> `memberOf` attribute.
>
> ```bash
"{USER}?memberOf?base"

```
>
> Your LDAP configuration may not include the `memberOf` attribute as part
> of the user schema, may possess a different attribute for reporting group
> membership, or may not track group membership through attributes.
> Configure your query with respect to your own unique LDAP configuration.

If unset, `mongod` cannot authorize users using LDAP.

This setting can be configured on a running `mongod` using the
`setParameter` database command.

> **Note**
>
> An explanation of [RFC4515](https://tools.ietf.org/html/rfc4515),
> [RFC4516](https://tools.ietf.org/html/rfc4516) or LDAP queries is out
> of scope for the MongoDB Documentation. Please review the RFC directly or
> use your preferred LDAP resource.

### Storage Options

**\`--storageEngine string\`**

*Default*: `wiredTiger`

Specifies the storage engine for the `mongod` database. Available
values include:

- - Value
  - Description
- - `wiredTiger`
  - To specify the \[wiredtiger\](/core/wiredtiger.md).

\* - `inMemory`  
- To specify the \[inmemory\](/core/inmemory.md).

  *Available in MongoDB Enterprise only.*

If you attempt to start a `mongod` with a
`--dbpath` that contains data files produced by a
storage engine other than the one specified by
`--storageEngine`, `mongod` doesn't start.

**\`--dbpath \<path\>\`**

*Default*: `/data/db` on Linux and macOS, `\data\db` on Windows

The directory where the `mongod` instance stores its data.

If using the default
Configuration File
included with a package manager installation of MongoDB, the
corresponding `storage.dbPath` setting uses a different
default.

The files in `--dbpath` must correspond to the storage engine
specified in `--storageEngine`. If the data files do not
correspond to `--storageEngine`, `mongod` doesn't start.

**\`--directoryperdb\`**

Uses a separate directory to store data for each database. The
directories are under the `--dbpath` directory, and each subdirectory
name corresponds to the database name.

Starting in MongoDB 5.0, dropping the final collection in a database
(or dropping the database itself) when `--directoryperdb` is
enabled deletes the newly empty subdirectory for that database.

To change the `--directoryperdb` option for existing
deployments:

- For standalone instances:
  1.  Use `mongodump` on the existing
      `mongod` instance to generate a backup.
  2.  Stop the `mongod` instance.
  3.  Add the `--directoryperdb` value **and**
      configure a new data directory
  4.  Restart the `mongod` instance.
  5.  Use `mongorestore` to populate the new data
      directory.
- For replica sets:
  1.  Stop a secondary member.
  2.  Add the `--directoryperdb` value **and**
      configure a new data directory to that secondary member.
  3.  Restart that secondary.
  4.  Use initial sync to populate
      the new data directory.
  5.  Update remaining secondaries in the same fashion.
  6.  Step down the primary, and update the stepped-down member in the
      same fashion.

**\`--syncdelay \<value\>\`**

*Default*: 60

Controls how much time can pass before MongoDB flushes data to the data
files.

**Do not set this value on
production systems.** In almost every situation, you should use the
default setting.

The `mongod` process writes data very quickly to the journal and
lazily to the data files. `--syncdelay` has no effect on
journaling, but if `--syncdelay` is set to
`0` the journal eventually consumes all available disk space.

**\`--upgrade\`**

Upgrades the on-disk data format of the files specified by the
`--dbpath` to the latest version, if needed.

This option only affects the operation of the `mongod` if the data
files are in an old format.

In most cases you should not set this value, so you can exercise the
most control over your upgrade process. See the MongoDB release notes
for more information about the upgrade process.

**\`--repair\`**

Runs a repair routine on all databases for a `mongod`
instance.

Starting in MongoDB 5.0:

- The repair operation validates the collections to find any
  inconsistencies and fixes them if possible, which avoids
  rebuilding the indexes.
- If a collection's data file is salvaged or if the collection has
  inconsistencies that the validate step is unable to fix, then all
  indexes are rebuilt.

> **Tip**
>

**\`--journalCommitInterval \<value\>\`**

*Default*: 100

The maximum amount of time in milliseconds that
the `mongod` process allows between
journal operations. Values can range from 1 to 500 milliseconds. Lower
values increase the durability of the journal, at the expense of disk
performance.

On WiredTiger, the default journal commit interval is 100
milliseconds. A write that includes or implies
`j:true` causes an immediate sync of the journal. For details
and additional conditions that affect the frequency of the sync, see
journal-process.

### WiredTiger Options

**\`--wiredTigerCacheSizeGB \<float\>\`**

Defines the maximum size of the internal cache that WiredTiger
uses for all data. The memory consumed by an index build (see
`maxIndexBuildMemoryUsageMegabytes`) is separate from the
WiredTiger cache memory.

With WiredTiger, MongoDB utilizes both the WiredTiger internal cache
and the filesystem cache.

**\`--wiredTigerCacheSizePct \<float\>\`**

Defines the maximum amount of memory to allocate for cache as a
percentage of physical RAM. The memory that an index build consumes (see
`maxIndexBuildMemoryUsageMegabytes`) is separate from the
WiredTiger cache memory.

You can specify a percentage of up to 80% of available memory.
Values range from `0.25` GB to `10000` GB.

With WiredTiger, MongoDB utilizes both the WiredTiger internal cache
and the filesystem cache.

**\`--wiredTigerJournalCompressor \<compressor\>\`**

*Default*: snappy

Specifies the type of compression to use to compress WiredTiger
journal data.

Available compressors are:

- `none`
- snappy
- zlib
- zstd

**\`--wiredTigerDirectoryForIndexes\`**

When you start `mongod` with `--wiredTigerDirectoryForIndexes`, `mongod` stores indexes and collections in separate
subdirectories under the data (i.e. `--dbpath`) directory.
Specifically, `mongod` stores the indexes in a subdirectory named
`index` and the collection data in a subdirectory named
`collection`.

By using a symbolic link, you can specify a different location for
the indexes. Specifically, when `mongod` instance is **not**
running, move the `index` subdirectory to the destination and
create a symbolic link named `index` under the data directory to
the new destination.

**\`--wiredTigerCollectionBlockCompressor \<compressor\>\`**

*Default*: snappy

Specifies the default compression for collection data. You can
override this on a per-collection basis when creating collections.

Available compressors are:

- `none`
- snappy
- zlib
- zstd

`--wiredTigerCollectionBlockCompressor` affects all collections created. If you change
the value of `--wiredTigerCollectionBlockCompressor` on an existing MongoDB deployment, all new
collections use the specified compressor. Existing collections
continue to use the compressor specified when they were
created, or the default compressor at that time.

**\`--wiredTigerIndexPrefixCompression \<boolean\>\`**

*Default*: true

Enables or disables prefix compression for index data.

Specify `true` for `--wiredTigerIndexPrefixCompression` to enable prefix compression for
index data, or `false` to disable prefix compression for index data.

The `--wiredTigerIndexPrefixCompression` setting affects all indexes created. If you change
the value of `--wiredTigerIndexPrefixCompression` on an existing MongoDB deployment, all new
indexes use prefix compression. Existing indexes
are not affected.

### Replication Options

**\`--replSet \<setname\>\`**

Configures replication. Specify a replica set name as an argument to
this set. All hosts in the replica set must have the same set name.

**\`--oplogSize \<value\>\`**

> **Note**
>

By default, the `mongod` process creates an oplog based on
the maximum amount of space available. For 64-bit systems, the oplog
is typically 5% of available disk space.

Once the `mongod` has created the oplog for the first time,
changing the `--oplogSize` option doesn't affect the size of
the oplog. To change the minimum oplog retention period after
starting the `mongod`, use
`replSetResizeOplog`. `replSetResizeOplog`
enables you to resize the oplog dynamically without restarting the
`mongod` process. To persist the changes made using
`replSetResizeOplog` through a restart, update the value
of `--oplogSize`.

See replica-set-oplog-sizing for more information.

**\`--oplogMinRetentionHours \<value\>\`**

Specifies the minimum number of hours to preserve an oplog entry,
where the decimal values represent the fractions of an hour. For
example, a value of `1.5` represents one hour and thirty
minutes.

The value must be greater than or equal to `0`. A value of `0`
indicates that the `mongod` should truncate the oplog
starting with the oldest entries to maintain the configured
maximum oplog size.

Defaults to `0`.

A `mongod` started with `--oplogMinRetentionHours`
only removes an oplog entry *if*:

- The oplog has reached the maximum configured oplog size *and*
- The oplog entry is older than the configured number of hours based
  on the host system clock.

The `mongod` has the following behavior when configured
with a minimum oplog retention period:

- The oplog can grow without constraint so as to retain oplog entries
  for the configured number of hours. This may result in reduction or
  exhaustion of system disk space due to a combination of high write
  volume and large retention period.
- If the oplog grows beyond its maximum size, the
  `mongod` may continue to hold that disk space even if
  the oplog returns to its maximum size *or* is configured for a
  smaller maximum size. See replSetResizeOplog-cmd-compact.
- The `mongod` compares the system wall clock to an
  oplog entry creation wall clock time when enforcing oplog entry
  retention. Clock drift between cluster components may result in
  unexpected oplog retention behavior. See
  production-notes-clock-synchronization for more information on
  clock synchronization across cluster members.

To change the minimum oplog retention period after starting the
`mongod`, use `replSetResizeOplog`.
`replSetResizeOplog` enables you to resize the oplog
dynamically without restarting the `mongod` process. To
persist the changes made using `replSetResizeOplog`
through a restart, update the value of
`--oplogMinRetentionHours`.

**\`--enableMajorityReadConcern\`**

*Default*: true

Configures support for `"majority"` read concern.

Starting in MongoDB 5.0,
`--enableMajorityReadConcern` cannot be changed
and is always set to `true`. In earlier versions of MongoDB,
`--enableMajorityReadConcern` was configurable.

> **Warning**
>

### Sharded Cluster Options

**\`--configsvr\`**

*Required if starting a config server.*

Declares that this `mongod` instance serves as the config server of a sharded cluster. When
running with this option, clients (i.e. other cluster components)
cannot write data to any database other than `config`
and `admin`. The default port for a `mongod` with this option is
`27019` and the default `--dbpath` directory is
`/data/configdb`, unless specified.

> **Important**
>
> When starting a MongoDB server with `--configsvr`, you must also
> specify a `--replSet`.

The use of the deprecated mirrored `mongod` instances as
config servers (SCCC) is no longer supported.

The replica set config servers (CSRS) must run the
WiredTiger storage engine.

The `--configsvr` option creates a local oplog.

Do not use the `--configsvr` option with `--shardsvr`. Config
servers cannot be a shard server.

Do not use the `--configsvr` with the
`skipShardingConfigurationChecks` parameter. That is, if
you are temporarily starting the `mongod` as a
standalone for maintenance operations, include the parameter
`skipShardingConfigurationChecks` and exclude `--configsvr`.
Once maintenance has completed, remove the
`skipShardingConfigurationChecks` parameter and restart
with `--configsvr`.

**\`--shardsvr\`**

*Required if starting a shard server.*

Configures this `mongod` instance as a shard in a
sharded cluster. The default port for these instances is
`27018`.

> **Important**
>
> When starting a MongoDB server with `--shardsvr`, you must also
> specify a `--replSet`.

Do not use the `--shardsvr` with the
`skipShardingConfigurationChecks` parameter. That is, if
you are temporarily starting the `mongod` as a
standalone for maintenance operations, include the parameter
`skipShardingConfigurationChecks` and exclude `--shardsvr`.
Once maintenance has completed, remove the
`skipShardingConfigurationChecks` parameter and restart
with `--shardsvr`.

### TLS Options

\[configure-ssl\](/tutorial/configure-ssl.md) for full
documentation of MongoDB's support.

**\`--tlsMode \<mode\>\`**

Enables TLS used for all network connections. The
argument to the `--tlsMode` option can be one of the following:

- - Value
  - Description
- - `disabled`
  - The server does not use TLS.
- - `allowTLS`
  - Connections between servers do not use TLS. For incoming
    connections, the server accepts both TLS and non-TLS.
- - `preferTLS`
  - Connections between servers use TLS. For incoming
    connections, the server accepts both TLS and non-TLS.
- - `requireTLS`
  - The server uses and accepts only TLS encrypted connections.

**\`--tlsCertificateKeyFile \<filename\>\`**

Specifies the `.pem` file that contains both the TLS certificate and
key.

On macOS or Windows, you can use the
`--tlsCertificateSelector` option to specify a
certificate from the operating system's secure certificate store
instead of a PEM key file. `--tlsCertificateKeyFile` and
`--tlsCertificateSelector` options are mutually exclusive.
You can only specify one.

- On Linux/BSD, you must specify `--tlsCertificateKeyFile`
  when TLS/SSL is enabled.
- On Windows or macOS, you must specify either
  `--tlsCertificateKeyFile` or
  `--tlsCertificateSelector` when TLS/SSL is enabled.

> **Important**
>
> For Windows **only**, MongoDB does not support
> encrypted PEM files. The `mongod` fails to start if
> it encounters an encrypted PEM file. To securely store and
> access a certificate for use with TLS on Windows,
> use `--tlsCertificateSelector`.

**\`--tlsCertificateKeyFilePassword \<value\>\`**

Specifies the password to decrypt the certificate-key file (i.e.
`--tlsCertificateKeyFile`). Use the
`--tlsCertificateKeyFilePassword` option only if the
certificate-key file is encrypted. In all cases, the
`mongod` redacts the password from all logging and
reporting output.

- On Linux/BSD, if the private key in the PEM file is encrypted and
  you do not specify the `--tlsCertificateKeyFilePassword` option, MongoDB prompts for a
  passphrase. See ssl-certificate-password.
- On macOS, if the private key in the PEM file is
  encrypted, you must explicitly specify the
  `--tlsCertificateKeyFilePassword` option. Alternatively,
  you can use a certificate from the secure system store (see
  `--tlsCertificateSelector`) instead of a PEM file or use an
  unencrypted PEM file.
- On Windows, MongoDB does not support encrypted certificates.
  The `mongod` fails if it encounters an encrypted
  PEM file. Use `--tlsCertificateSelector` instead.

**\`--clusterAuthMode \<option\>\`**

*Default*: keyFile

The authentication mode used for cluster authentication. If you use
internal X.509 authentication,
specify so here. This option can have one of the following values:

- - Value
  - Description
- - `keyFile`
  - Use a keyfile for authentication.
    Accept only keyfiles.
- - `sendKeyFile`
  - For rolling upgrade purposes. Send a keyfile for
    authentication but can accept both keyfiles and X.509
    certificates.
- - `sendX509`
  - For rolling upgrade purposes. Send the X.509 certificate for
    authentication but can accept both keyfiles and X.509
    certificates.
- - `x509`
  - Recommended. Send the X.509 certificate for authentication and
    accept only X.509 certificates.

**\`--tlsClusterFile \<filename\>\`**

Specifies the `.pem` file that contains the X.509
certificate-key file for membership authentication for the cluster or replica set.

On macOS or Windows, you can use the
`--tlsClusterCertificateSelector` option to specify a
certificate from the operating system's secure certificate store
instead of a PEM key file. `--tlsClusterFile` and
`--tlsClusterCertificateSelector` options are mutually
exclusive. You can only specify one.

If `--tlsClusterFile` does not specify the `.pem` file for
internal cluster authentication or the alternative
`--tlsClusterCertificateSelector`, the cluster uses the
`.pem` file specified in the `--tlsCertificateKeyFile`
option or the certificate returned by the
`--tlsCertificateSelector`.

> **Important**
>
> For Windows **only**, MongoDB does not support
> encrypted PEM files. The `mongod` fails to start if
> it encounters an encrypted PEM file. To securely store and
> access a certificate for use with membership authentication on
> Windows, use `--tlsClusterCertificateSelector`.

**\`--tlsCertificateSelector \<parameter\>=\<value\>\`**

> **Note**
>
> Available on Windows and macOS as an alternative to
> `--tlsCertificateKeyFile`.

Specifies a certificate property in order to select a matching
certificate from the operating system's certificate store to use for
TLS.

The `--tlsCertificateKeyFile` and
`--tlsCertificateSelector` options are mutually exclusive.
You can only specify one.

`--tlsCertificateSelector` accepts an argument of the format
`<property>=<value>` where the property can be one of the
following:

The `mongod` searches the operating system's secure
certificate store for the CA certificates required to validate the
full certificate chain of the specified TLS certificate.
Specifically, the secure certificate store must contain the root CA
and any intermediate CA certificates required to build the full
certificate chain to the TLS certificate. Do **not** use
`--tlsCAFile` or `--tlsClusterCAFile` to specify the
root and intermediate CA certificate

For example, if the TLS/SSL certificate was signed with a single root
CA certificate, the secure certificate store must contain that root
CA certificate. If the TLS/SSL certificate was signed with an
intermediate CA certificate, the secure certificate store must
contain the intermedia CA certificate *and* the root CA certificate.

**\`--tlsClusterCertificateSelector \<parameter\>=\<value\>\`**

> **Note**
>
> Available on Windows and macOS as an alternative to
> `--tlsClusterFile`.

Specifies a certificate property in order to select a matching
certificate from the operating system's certificate store
for internal X.509 membership authentication.

`--tlsClusterFile` and
`--tlsClusterCertificateSelector` options are mutually
exclusive. You can only specify one.

`--tlsClusterCertificateSelector` accepts an argument of the
format `<property>=<value>` where the property can be one of the
following:

The `mongod` searches the operating system's secure
certificate store for the CA certificates required to validate the
full certificate chain of the specified cluster certificate.
Specifically, the secure certificate store must contain the root CA
and any intermediate CA certificates required to build the full
certificate chain to the cluster certificate. Do **not** use
`--tlsCAFile` or `--tlsClusterCAFile` to specify the
root and intermediate CA certificate.

For example, if the cluster certificate was signed with a single root
CA certificate, the secure certificate store must contain that root
CA certificate. If the cluster certificate was signed with an
intermediate CA certificate, the secure certificate store must
contain the intermedia CA certificate *and* the root CA certificate.

**\`--tlsClusterPassword \<value\>\`**

Specifies the password to decrypt the X.509 certificate-key file
specified with `--tlsClusterFile`. Use the
`--tlsClusterPassword` option only if the certificate-key
file is encrypted. In all cases, the `mongod` redacts
the password from all logging and reporting output.

- On Linux/BSD, if the private key in the X.509 file is encrypted and
  you do not specify the `--tlsClusterPassword` option,
  MongoDB prompts for a passphrase. See
  ssl-certificate-password.
- On macOS, if the private key in the X.509 file is
  encrypted, you must explicitly specify the
  `--tlsClusterPassword` option. Alternatively, you can
  either use a certificate from the secure system store (see
  `--tlsClusterCertificateSelector`) instead of a cluster PEM
  file or use an unencrypted PEM file.
- On Windows, MongoDB does not support encrypted certificates.
  The `mongod` fails if it encounters an encrypted
  PEM file. Use `--tlsClusterCertificateSelector` instead.

**\`--tlsCAFile \<filename\>\`**

Specifies the `.pem` file that contains the root certificate
chain from the Certificate Authority. Specify the file name of the
`.pem` file using relative or absolute paths.

> **Important**
>

Windows/macOS Only  
If using `--tlsCertificateSelector` and/or
`--tlsClusterCertificateSelector`, do **not** use
`--tlsCAFile` to specify the root and intermediate CA
certificates. Store all CA certificates required to validate the
full trust chain of the `--tlsCertificateSelector` and/or
`--tlsClusterCertificateSelector` certificates in the
secure certificate store.

**\`--tlsClusterCAFile \<filename\>\`**

Specifies the `.pem` file that contains the root certificate
chain from the Certificate Authority used to validate the certificate
presented by a client establishing a connection. Specify the file
name of the `.pem` file using relative or absolute paths.
`--tlsClusterCAFile` requires that
`--tlsCAFile` is set.

If `--tlsClusterCAFile` does not specify the `.pem`
file for validating the certificate from a client establishing a
connection, the cluster uses the `.pem` file specified in the
`--tlsCAFile` option.

`--tlsClusterCAFile` lets you use separate Certificate
Authorities to verify the client to server and server to client
portions of the TLS handshake.

Windows/macOS Only  
If using `--tlsCertificateSelector` and/or
`--tlsClusterCertificateSelector`, do **not** use
`--tlsClusterCAFile` to specify the root and
intermediate CA certificates. Store all CA certificates required to
validate the full trust chain of the
`--tlsCertificateSelector` and/or
`--tlsClusterCertificateSelector` certificates in the
secure certificate store.

**\`--tlsCRLFile \<filename\>\`**

Specifies the `.pem` file that contains the Certificate Revocation
List. Specify the file name of the `.pem` file using relative or
absolute paths.

> **Note**
>
> - You cannot specify a CRL file on
>   macOS. Instead, you can use the system SSL certificate store,
>   which uses OCSP (Online Certificate Status Protocol) to
>   validate the revocation status of certificates. See
>   `--tlsCertificateSelector` to use the
>   system SSL certificate store.
> - To check for certificate revocation,
>   MongoDB `enables` the use of OCSP
>   (Online Certificate Status Protocol) by default as an
>   alternative to specifying a CRL file or using the system SSL
>   certificate store.

**\`--tlsAllowInvalidCertificates\`**

Bypasses the validation checks for TLS certificates on other
servers in the cluster and allows the use of invalid certificates to
connect.

> **Note**
>

When using
the `--tlsAllowInvalidCertificates` setting, MongoDB
logs a warning regarding the use of the invalid certificate.

**\`--tlsAllowInvalidHostnames\`**

Disables the validation of the hostnames in TLS certificates,
when connecting to other members of the replica set or sharded cluster
for inter-process authentication. This allows `mongod` to connect
to other members if the hostnames in their certificates do not match
their configured hostname.

**\`--tlsAllowConnectionsWithoutCertificates\`**

By default, the server bypasses client certificate validation unless
the server is configured to use a CA file. If a CA file is provided, the
following rules apply:

- 
- For clients that present a certificate, `mongod` performs
  certificate validation using the root certificate chain specified by
  `--tlsCAFile` and reject clients with invalid
  certificates.

Use the `--tlsAllowConnectionsWithoutCertificates` option if you have
a mixed deployment that includes clients that do not or cannot present
certificates to the `mongod`.

**\`--tlsDisabledProtocols \<protocol(s)\>\`**

Prevents a MongoDB server running with TLS from accepting
incoming connections that use a specific protocol or protocols. To
specify multiple protocols, use a comma separated list of protocols.

`--tlsDisabledProtocols` recognizes the following protocols: `TLS1_0`, `TLS1_1`,
`TLS1_2`, and `TLS1_3`.

- On macOS, you cannot disable `TLS1_1` and leave both `TLS1_0` and
  `TLS1_2` enabled. You must disable at least one of the other
  two, for example, `TLS1_0,TLS1_1`.
- To list multiple protocols, specify as a comma separated list of
  protocols. For example `TLS1_0,TLS1_1`.
- Specifying an unrecognized protocol prevents the server from
  starting.
- The specified disabled protocols overrides any default disabled
  protocols.

MongoDB disables the use of TLS 1.0 if TLS
1.1+ is available on the system. To enable the disabled TLS 1.0,
specify `none` to `--tlsDisabledProtocols`.

Members of replica sets and sharded clusters must speak at least one
protocol in common.

> **See also**
>
> ssl-disallow-protocols

**\`--tlsFIPSMode\`**

Directs the `mongod` to use the FIPS mode of the TLS
library. Your system must have a FIPS
compliant library to use the `--tlsFIPSMode` option.

### Profiler Options

**\`--profile \<level\>\`**

*Default*: 0

Configures the \[database profiler\](/tutorial/manage-the-database-profiler.md) level.
The following profiler levels are available:

**\`--slowms \<integer\>\`**

*Default*: 100

For `mongod` instances, `--slowms` affects the diagnostic log
and, if enabled, the profiler.

> **See also**
>
> \[manage-the-database-profiler\](/tutorial/manage-the-database-profiler.md)

**\`--slowOpSampleRate \<double\>\`**

*Default*: 1.0

The fraction of *slow* operations that should be profiled or logged.
`--slowOpSampleRate` accepts values between 0 and 1, inclusive.

`--slowOpSampleRate` does not affect the slow oplog entry logging
by the secondary members of a replica set. Secondary
members log all oplog entries that take longer than the slow
operation threshold regardless of the `--slowOpSampleRate`.

For `mongod` instances, `--slowOpSampleRate` affects the
diagnostic log and, if enabled, the profiler.

### Audit Options

**\`--auditCompressionMode\`**

**\`--auditDestination\`**

Enables auditing and specifies where
`mongod` sends all audit events.

`--auditDestination` can have one of the following values:

- - Value
  - Description

- - `syslog`

  - Output the audit events to syslog in JSON format. Not available on
    Windows. Audit messages have a syslog severity level of `info`
    and a facility level of `user`.

    The syslog message limit can result in the truncation of
    audit messages. The auditing system neither detects the
    truncation nor errors upon its occurrence.

- - `console`
  - Output the audit events to `stdout` in JSON format.

- - `file`
  - Output the audit events to the file specified in
    `--auditPath` in the format specified in
    `--auditFormat`.

**\`--auditEncryptionKeyUID\`**

**\`--auditFormat\`**

Specifies the format of the output file for \[auditing\](/core/auditing.md) if `--auditDestination` is `file`. The
`--auditFormat` option can have one of the following values:

- - Value
  - Description
- - `JSON`
  - Output the audit events in JSON format to the file specified
    in `--auditPath`.

\* - `BSON`

> - Output the audit events in BSON binary format to the file
>   specified in `--auditPath`.

Printing audit events to a file in JSON format degrades server
performance more than printing to a file in BSON format.

**\`--auditLocalKeyFile\`**

**\`--auditPath\`**

Specifies the output file for auditing if
`--auditDestination` has value of `file`. The
`--auditPath` option can take either a full path name or a
relative path name.

**\`--auditFilter\`**

Specifies the filter to limit the types of operations the \[audit system\](/core/auditing.md) records. The option takes a string representation
of a query document of the form:

```javascript
{ <field1>: <expression1>, ... }

```

The `<field>` can be \[any field in the audit message\](/reference/audit-message.md), including fields returned in the
param document. The
`<expression>` is a query condition expression.

**\`--auditSchema\`**

*Default*: `mongo`

> **New in version 8.0**
>

### inMemory Options

**\`--inMemorySizeGB \<float\>\`**

*Default*: 50% of physical RAM minus 1 GB.

Maximum amount of memory to allocate for the \[in-memory storage engine\](/core/inmemory.md) data, including indexes, the oplog (if the
`mongod` is part of a replica set), sharded
cluster metadata, etc.

Values can range from 256MB to 10TB and can be a float.

### Encryption Key Management Options

**\`--enableEncryption\`**

*Default*: false

Enables encryption for the WiredTiger storage engine. This option
must be enabled in order to pass in encryption keys and
configurations.

**\`--encryptionCipherMode \<string\>\`**

*Default*: AES256-CBC

The cipher mode to use for encryption at rest:

**\`--encryptionKeyFile \<string\>\`**

The path to the local keyfile when managing keys via process *other
than* KMIP. Only set when managing keys via process other than KMIP.
If data is already encrypted using KMIP, MongoDB throws an error.

The keyfile can contain only a single key. The key is either a 16 or
32 character string.

Requires `--enableEncryption`.

**\`--kmipKeyIdentifier \<string\>\`**

Unique KMIP identifier for an existing key within the KMIP server.
Include to use the key associated with the identifier as the system
key. You can only use the setting the first time you enable
encryption for the `mongod` instance. Requires
`--enableEncryption`.

If unspecified, MongoDB requests that the KMIP server create a
new key to utilize as the system key.

If the KMIP server cannot locate a key with the specified identifier
or the data is already encrypted with a key, MongoDB throws an
error

**\`--kmipRotateMasterKey \<boolean\>\`**

*Default*: false

If true, rotate the master key and re-encrypt the internal
keystore.

> **See also**
>
> kmip-master-key-rotation

**\`--kmipServerName \<string\>\`**

Hostname or IP address of the KMIP server to connect to. Requires
`--enableEncryption`.

You can specify multiple KMIP servers as a comma-separated list, for example:
`server1.example.com,server2.example.com`. On startup, the
`mongod` attempts to establish a connection to each
server in the order listed, and selects the first server to
which it can successfully establish a connection. KMIP server
selection occurs only at startup.

When connecting to a KMIP server, the `mongod`
verifies that the specified `--kmipServerName` matches the
Subject Alternative Name `SAN` (or, if `SAN` is not present, the
Common Name `CN`) in the certificate presented by the KMIP server.
If `SAN` is present, `mongod` does not match against
the `CN`. If the hostname does not match the `SAN` (or `CN`),
the `mongod` fails to connect.

**\`--kmipPort \<number\>\`**

*Default*: 5696

Port number to use to communicate with the KMIP server.
Requires `--kmipServerName`. Requires
`--enableEncryption`.

If specifying multiple KMIP servers with `--kmipServerName`,
the `mongod` uses the port specified with
`--kmipPort` for all provided KMIP servers.

**\`--kmipConnectRetries \<number\>\`**

*Default*: 0

How many times to retry the initial connection to the KMIP server.
Use together with `--kmipConnectTimeoutMS` to
control how long the `mongod` waits for a response
between each retry.

**\`--kmipConnectTimeoutMS \<number\>\`**

*Default*: 5000

Timeout in milliseconds to wait for a response from the KMIP server.
If the `--kmipConnectRetries` setting is specified,
the `mongod` waits for the specified interval between retries.

Value must be `1000` or greater.

**\`--kmipClientCertificateSelector \<string\>\`**

> **New in version 5.0**
>
> Available on Windows and macOS as an alternative to
> `--kmipClientCertificateFile`.
>
> `--kmipClientCertificateFile` and `--kmipClientCertificateSelector` options are mutually exclusive. You can only
> specify one.

Specifies a certificate property in order to select a matching
certificate from the operating system's certificate store to
authenticate MongoDB to the KMIP server.

`--kmipClientCertificateSelector` accepts an argument of the format `<property>=<value>`
where the property can be one of the following:

**\`--kmipClientCertificateFile \<string\>\`**

Path to the `.pem` file used to authenticate MongoDB to the KMIP
server. The specified `.pem` file must contain both the TLS/SSL
certificate and key.

To use this option, you must also specify the
`--kmipServerName` option.

> **Note**
>
> On macOS or Windows, you can use a certificate
> from the operating system's secure store instead of a PEM key
> file. See `--kmipClientCertificateSelector`.

**\`--kmipClientCertificatePassword \<string\>\`**

**\`--kmipServerCAFile \<string\>\`**

Path to CA File. Used for validating secure client connection to
KMIP server.

> **Note**
>
> On macOS or Windows, you can use a certificate
> from the operating system's secure store instead of a PEM key
> file. See `--kmipClientCertificateSelector`. When using the secure
> store, you do not need to, but can, also specify the `--kmipServerCAFile`.

**\`--kmipActivateKeys \<boolean\>\`**

*Default*: true

> **New in version 5.3**
>

Activates all newly created KMIP keys upon creation and then periodically
checks those keys are in an active state.

When `--kmipActivateKeys` is `true` and you have existing keys on a
KMIP server, the key must be activated first or the `mongod`
node fails to start.

If the key being used by the mongod transitions into a non-active state,
the `mongod` node shuts down unless `kmipActivateKeys` is
false. To ensure you have an active key, rotate the KMIP master key by
using `--kmipRotateMasterKey`.

**\`--kmipKeyStatePollingSeconds \<integer\>\`**

*Default*: 900 seconds

> **New in version 5.3**
>

Frequency in seconds at which `mongod` polls the KMIP server for
active keys.

To disable disable polling, set the value to `-1`.

**\`--kmipUseLegacyProtocol \<boolean\>\`**

*Default*: false

> **New in version 7.0 (and 6.0.6)**
>

**\`--eseDatabaseKeyRollover\`**

Roll over the encrypted storage engine database keys configured with
`AES256-GCM` cipher.

When `mongod` instance is started with this option, the
instance rotates the keys and exits.
