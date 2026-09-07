# `mongokerberos`

## Synopsis

MongoDB Enterprise provides `mongokerberos` for testing MongoDB's
Kerberos and GSSAPI configuration options
against a running Kerberos deployment. `mongokerberos` can be used
in one of two modes: *server* and *client*.

- - Mode
  - Description
- - Server
  - In *server mode*, `mongokerberos` analyzes
    Kerberos-related configurations on the server, and returns a
    report which includes error messages for any configurations that
    are problematic. For usage, see mongokeberos-usage-server

\* - Client

> - In *client mode*, `mongokerberos` tests Kerberos
>   authentication for a provided username, and returns a report
>   which includes the success or failure of each step in the
>   Kerberos authentication procedure. For usage, see
>   mongokeberos-usage-client

Error messages for both modes include information on specific errors
encountered and potential advice for resolving the error.

`mongokerberos` supports the following deployment types,
in both server and client modes:

- Linux MongoDB clients authenticating to MIT Kerberos deployments on
  supported Linux platforms.
- Windows MongoDB clients authenticating to Windows Active Directory
  deployments on
  supported Windows platforms.
- Linux MongoDB clients authenticating to Windows Active Directory
  deployments.

> **Note**
>
> MongoDB Enterprise and `mongokerberos` only support the
> [MIT implementation](https://kerberos.org/)
> of Kerberos.

Generally, when configuring options related to
Kerberos authentication, it is good practice
to verify your configuration with `mongokerberos`.

`mongokerberos` is a testing and verification tool; it does not
edit any files or configure any services. For configuring Kerberos on
your platform please consult the [MIT Kerberos documentation](https://web.mit.edu/kerberos/krb5-latest/doc/), or your platform's
documentation. For configuring MongoDB to authenticate using Kerberos,
please reference the following tutorials:

- \[control-access-to-mongodb-with-kerberos-authentication\](/tutorial/control-access-to-mongodb-with-kerberos-authentication.md)
- \[control-access-to-mongodb-windows-with-kerberos-authentication\](/tutorial/control-access-to-mongodb-windows-with-kerberos-authentication.md).

This document provides a complete overview of all command line options
for `mongokerberos`.

## Installation

The `mongokerberos` tool is part of the *MongoDB Database Tools Extra*
package, and can be installed with the MongoDB Server or as a
standalone installation.

### Install with Server

### Install as Standalone

## Usage

`mongokerberos` can be run in two modes: *server* and
*client*.

### Server Mode

Running `mongokerberos` in server mode performs a series of
verification steps against your system's Kerberos configuration,
including checking for proper DNS resolution, validation of the Kerberos
system keytab file, and testing against the MongoDB service principal
for your `mongod` or `mongos` instance.

Before you can use `mongokerberos` in server mode, you must:

1.  Configure Kerberos on your platform according to your platform's
    documentation.
2.  Create the MongoDB service principal for use with your
    `mongod` or `mongos` instance, as described
    in the following steps:
    - Configure Service Principal on Linux
    - Configure Service Principal on Windows

Once you have completed these steps, you can run
`mongokerberos` in server mode using the
`--server` flag as follows:

```bash
mongokerberos --server

```

If Kerberos has been configured properly on the server, and the service
principal created successfully, the output might resemble the following:

```none
Resolving kerberos environment...
[OK] Kerberos environment resolved without errors.

Verifying DNS resolution works with Kerberos service at <hostname>...
[OK] DNS test successful.

Getting MIT Kerberos KRB5 environment variables...
  * KRB5CCNAME: not set.
  * KRB5_CLIENT_KTNAME: not set.
  * KRB5_CONFIG: not set.
  * KRB5_KTNAME: not set.
  * KRB5_TRACE: not set.
[OK]

Verifying existence of KRB5 keytab FILE:/etc/krb5.keytab...
[OK] KRB5 keytab exists and is populated.

Checking principal(s) in KRB5 keytab...
Found the following principals for MongoDB service mongodb:
  * mongodb/server.example.com@SERVER.EXAMPLE.COM
Found the following kvnos in keytab entries for service mongodb:
  * 3
[OK] KRB5 keytab is valid.

Fetching KRB5 Config...
KRB5 config profile resolved as:
   <Your Kerberos profile file will be output here>
[OK] KRB5 config profile resolved without errors.

Attempting to initiate security context with service credentials...
[OK] Security context initiated successfully.

```

The final message indicates that the system's Kerberos configuration is
ready to be used with MongoDB. If any errors are encountered with
the configuration, they will be presented as part of the above output.

### Client Mode

Running `mongokerberos` in client mode tests authentication
against your system's Kerberos environment, performing each step in the
Kerberos authentication process, including checking for proper DNS
resolution, verification of the Kerberos client keytab file, and testing
whether a ticket can be successfully granted. Running
`mongokerberos` in client mode simulates the client
authentication procedure of `mongosh`.

Before you can use `mongokerberos` in client mode, you must
first have configured Kerberos on your platform according to your
platform's documentation. Optionally, you may also choose to run
`mongokerberos` in
server mode first to verify that your
platform's Kerberos configuration is valid before using client mode.

Once you have completed these steps, you can run
`mongokerberos` in client mode to test user authentication,
using the `--client` flag as follows:

```bash
mongokerberos --client --username <username>

```

You must provide a valid username, which is used to request a Kerberos
ticket as part of the authentication procedure. Your platform's
Kerberos infrastructure must be aware of this user.

If the provided credentials are valid, and the Kerberos options in the
configuration files are valid, the output might resemble the following:

```none
Resolving kerberos environment...
[OK] Kerberos environment resolved without errors.

Verifying DNS resolution works with Kerberos service at <hostname>...
[OK] DNS test successful.

Getting MIT Kerberos KRB5 environment variables...
  * KRB5CCNAME: not set.
  * KRB5_CLIENT_KTNAME: not set.
  * KRB5_CONFIG: not set.
  * KRB5_KTNAME: not set.
  * KRB5_TRACE: not set.
[OK]

Verifying existence of KRB5 client keytab FILE:/path/to/client.keytab...
[OK] KRB5 client keytab exists and is populated.

Checking principal(s) in KRB5 keytab...
[OK] KRB5 keytab is valid.

Fetching KRB5 Config...
KRB5 config profile resolved as:
   <Your Kerberos profile file will be output here>
[OK] KRB5 config profile resolved without errors.

Attempting client half of GSSAPI conversation...
[OK] Client half of GSSAPI conversation completed successfully.

```

The final message indicates that client authentication completed
successfully for the user provided. If any errors are encountered
during the authentication steps, they will be presented as part of the
above output.

## Options

**\`--server\`**

Runs `mongokerberos` in server mode to test that your
platform's Kerberos configuration is valid for use with MongoDB.

See mongokeberos-usage-server for example usage and expected
output.

**\`--client\`**

Runs `mongokerberos` in client mode to test client
authentication against your system's Kerberos environment. Requires
specifying a valid username with `--username` when running in
client mode. `mongokerberos` will request a Kerberos ticket
for this username as part of the validation procedure. Running
`mongokerberos` in client mode simulates the client
authentication procedure of `mongosh`.

See mongokeberos-usage-client for example usage and expected
output.

**\`--config \<filename\>, -f \<filename\>\`**

Specifies a configuration file for runtime configuration options.
The options are equivalent to the command-line
configuration options. See \[configuration-options\](/reference/configuration-options.md) for
more information.

`mongokerberos` will read the values for
`saslHostName` and `saslServiceName` from this
file if present. These values can alteratively be specified with the
`--setParameter` option instead.

Ensure the configuration file uses ASCII encoding. The
`mongokerberos` instance does not support configuration
files with non-ASCII encoding, including UTF-8.

Only valid in server mode.

**\`--setParameter \<options\>\`**

Sets a configurable parameter. You can specify multiple
`setParameter` fields.

While you can use any supported parameters with `setParameter`,
`mongokerberos` only checks for the value of the following:

- `saslHostName`
- `saslServiceName`

If using the `--config` option with a configuration file that
also contains these values, the `setParameter` values will
override the values from the configuration file.

Valid in both server mode
and client mode.

**\`--host \<hostname\>\`**

Specify the hostname of the MongoDB server to connect to when testing
authentication.

If `--host` is not specified, `mongokerberos` does
not perform any DNS validation of the hostname (i.e. PTR record
verification)

Only valid in client mode.

**\`--username \<username\>, -u \<username\>\`**

Username for `mongokerberos` to use when attempting Kerberos
authentication. This value is required when running in
client mode.

Only valid in client mode.

**\`--gssapiServiceName \<servicename\>\`**

*default: 'mongodb'*

Service principal name to use when authenticating using
GSSAPI/Kerberos.

Only valid in client mode.

**\`--gssapiHostName \<hostname\>\`**

Remote hostname to use for purpose of GSSAPI/Kerberos authentication.

Only valid in client mode.
