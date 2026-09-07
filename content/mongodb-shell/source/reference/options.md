# Options

Use the following options to view and control various aspects of your
MongoDB Shell.

## General Options

mongosh
--build-info
Returns a JSON-formatted document with information about
your `mongosh` build and driver dependencies.

**Example: View Build Information**

`--eval <javascript>`  

Evaluates a JavaScript expression. You can use a single `--eval`
argument or multiple `--eval` arguments together.

After `mongosh` evaluates the `--eval` argument, it prints the
results to your command line. If you use multiple `--eval`
statements, `mongosh` only prints the results of the last
`--eval`.

You can use the `--json` flag with `--eval` to return
`mongosh` results in \[Extended JSON\](<https://www.mongodb.com/docs/manual/reference/mongodb-extended-json/>) format. `mongosh`
supports both `--json=canonical` and `--json=relaxed` modes. If
you omit the mode, `mongosh` defaults to the `canonical`
mode. The `--json` flag is mutually exclusive with `--shell`.

**Example: Format Output**

**Example: Multiple --eval Arguments**

**Example: --json Option**

`--file, -f <javascript>`  

Runs a script from the command line without entering the
MongoDB Shell console.

For additional details and an example, see
mdb-shell-write-scripts-command-line.
--help, -h
Returns information on the options and use of the MongoDB Shell.
--nodb
Prevents the shell from connecting to any database instances.
--no-quiet
Disables the default `--quiet` option mode for non-interactive
shell sessions. When specified, `mongosh` displays all messages during startup.
--norc
Prevents the shell from sourcing and evaluating `~/.mongoshrc.js`
on startup.
--quiet
Skips all messages during startup (such as welcome messages and startup
warnings) and goes directly to the prompt.

For non-interactive shell sessions, MongoDB enables `--quiet` by default. You can
disable this behavior using `--no-quiet`.
--skipStartupWarnings
Prevents `mongosh` from displaying server startup warnings when
creating a session. To suppress all startup messages, use the
`--quiet` option.
--shell
Enables the shell interface. If you invoke the `mongosh`
command and specify a JavaScript file as an argument, or use
`--eval` to specify JavaScript on the command line,
the `--shell` option provides the user with a shell
prompt after the file finishes executing. The `--shell` flag is
mutually exclusive with `--json`.
--verbose
Increases the verbosity of the shell output during the connection
process and when running commands.
--version
Returns the MongoDB Shell release number.

## Stable API Options

`--apiVersion <version number>`  

Specifies the apiVersion. `"1"` is
currently the only supported value.
--apiStrict
Specifies that the server will respond with APIStrictError if your application uses a command or behavior
outside of the Stable API.

If you specify `--apiStrict`, you must also specify
`--apiVersion`.
--apiDeprecationErrors
Specifies that the server will respond with APIDeprecationError if your application uses a command or behavior
that is deprecated in the specified `apiVersion`.

If you specify `--apiDeprecationErrors`, you must also
specify `--apiVersion`.

## Connection Options

`--host <hostname>`  

Specifies the name of the host machine where the
`mongod` or `mongos` is running. If this is
not specified, the MongoDB Shell attempts to connect to a MongoDB
process running on the localhost.

To connect to a replica set,  
Specify the `replica set name`
and a seed list of set members. Use the following form:

```none
<replSetName>/<hostname1><:port>,<hostname2><:port>,<...>

```

For TLS/SSL connections (`--tls`),  
The MongoDB Shell verifies that the hostname
(specified in the `--host` option or the
connection string) matches the `SAN` (or, if `SAN` is not
present, the `CN`) in the certificate presented by the
`mongod` or `mongos`. If `SAN` is
present, the MongoDB Shell does not match against the `CN`. If
the hostname does not match the `SAN` (or `CN`), the
MongoDB Shell shell fails to connect.

For \[DNS seedlist connections\](<https://www.mongodb.com/docs/manual/reference/connection-string/#dns-seedlist-connection-format/>),  
Specify the connection protocol as `mongodb+srv`, followed by
the DNS SRV hostname record and any options. The `authSource`
and `replicaSet` options, if included in the connection string,
overrides any corresponding DNS-configured options set in the
TXT record. Use of the `mongodb+srv:` connection string
implicitly enables TLS (Transport Layer Security) / SSL (Secure Sockets Layer) (normally set with `tls=true`) for
the client connection. The TLS (Transport Layer Security) option can be turned off by
setting `tls=false` in the query string.

> **Example**
>
> ```none
mongodb+srv://server.example.com/?connectionTimeoutMS=3000
```

`--port <port>`  

Specifies the port where the `mongod` or
`mongos` instance is listening. If
`--port` is not
specified, the MongoDB Shell attempts to connect to port `27017`.

### TLS Options

`--tls`  

Enables connection to a `mongod` or
`mongos` that has TLS (Transport Layer Security) / SSL (Secure Sockets Layer) support enabled.

`--tlsCertificateKeyFile <filename>`  

Specifies the `.pem` file that contains both the TLS (Transport Layer Security) / SSL (Secure Sockets Layer)
certificate and key for `mongosh`. Specify the
file name of the `.pem` file using relative or absolute paths.

This option is required when using the `--tls` option to connect to
a `mongod` or `mongos` instance that
requires client certificates. That is, the
MongoDB Shell presents this certificate to the server.

> **Note**
>

`--tlsCertificateKeyFilePassword <value>`  

Specifies the password to de-crypt the certificate-key file (i.e.
`--tlsCertificateKeyFile`).

Use the
`--tlsCertificateKeyFilePassword` option only if the
certificate-key file is encrypted. In all cases, the MongoDB Shell
redacts the password from all logging and reporting output.

If the private key in the PEM file is encrypted and you do not
specify the
`--tlsCertificateKeyFilePassword` option; the MongoDB Shell prompts
for a passphrase.

See ssl-certificate-password.

`--tlsCAFile <filename>`  

Specifies the `.pem` file that contains the root certificate
chain from the Certificate Authority. This file is used to validate
the certificate presented by the
`mongod` / `mongos` instance.

Specify the file name of the `.pem` file using relative or
absolute paths.

`--tlsCRLFile <filename>`  

Specifies the `.pem` file that contains the Certificate
Revocation List. Specify the file name of the `.pem` file
using relative or absolute paths.

`--tlsAllowInvalidHostnames`  

Disables the validation of the hostnames in the certificate presented
by the `mongod` / `mongos` instance. Allows
the MongoDB Shell to connect to MongoDB instances even if the hostname
in the server certificates do not match the server's host.

`--tlsAllowInvalidCertificates`  

> **New in version 4.2**
>

Bypasses the validation checks for the certificates presented by the
`mongod` / `mongos` instance and allows
connections to servers that present invalid certificates.

> **Note**
>
> Starting in MongoDB 4.0, if you specify
> `--tlsAllowInvalidCertificates` when using x.509
> authentication, an invalid certificate is only sufficient to
> establish a TLS (Transport Layer Security) / SSL (Secure Sockets Layer) connection but is *insufficient* for
> authentication.

> **Warning**
>
> Although available, avoid using the
> `--tlsAllowInvalidCertificates` option if possible. If the
> use of `--tlsAllowInvalidCertificates` is necessary, only
> use the option on systems where intrusion is not possible.
>
> If the MongoDB Shell shell (and other
> mongodb-tools-support-ssl) runs with the
> `--tlsAllowInvalidCertificates` option, the shell (and
> other mongodb-tools-support-ssl) do not attempt to validate
> the server certificates. This creates a vulnerability to expired
> `mongod` and `mongos` certificates as
> well as to foreign processes posing as valid `mongod`
> or `mongos` instances. If you only need to disable
> the validation of the hostname in the TLS (Transport Layer Security) / SSL (Secure Sockets Layer) certificates, see
> `--tlsAllowInvalidHostnames`.

--tlsCertificateSelector \<parameter\>=\<value\>
Available on Windows and macOS as an alternative to
`--tlsCertificateKeyFile`.

> **Important Windows and Importing Private Keys**
>
> When you import your private key, you must mark it as exportable.
> The Windows **Certificate Import Wizard** doesn't check this
> option by default.

The `--tlsCertificateKeyFile` and
`--tlsCertificateSelector`
options are mutually exclusive. You can only specify one.

Specifies a certificate property in order to select a matching
certificate from the operating system's certificate store.

`--tlsCertificateSelector`
accepts an argument of the format `<property>=<value>` where the
property can be one of the following:

- - Property
  - Value type
  - Description
- - `subject`
  - ASCII string
  - Subject name or common name on certificate

\* - `thumbprint`  
- hex string

- A sequence of bytes, expressed as hexadecimal, used to
  identify a public key by its SHA-1 digest.

  The `thumbprint` is sometimes referred to as a
  `fingerprint`.

When using the system SSL certificate store, OCSP (Online
Certificate Status Protocol) is used to validate the revocation
status of certificates.

> **Note**
>

`--tlsDisabledProtocols <string>`  

Disables the specified TLS protocols. The option recognizes the
following protocols:

- `TLS1_0`
- `TLS1_1`
- `TLS1_2`
- *(Starting in version 4.0.4, 3.6.9, 3.4.24)* `TLS1_3`
- On macOS, you cannot disable `TLS1_1` and leave both `TLS1_0`
  and `TLS1_2` enabled. You must also disable at least one of the
  other two; for example, `TLS1_0,TLS1_1`.
- To list multiple protocols, specify as a comma separated list of
  protocols. For example `TLS1_0,TLS1_1`.
- The specified disabled protocols overrides any default disabled
  protocols.

Starting in version 4.0, MongoDB disables the use of TLS 1.0 if TLS
1.1+ is available on the system. To enable the
disabled TLS 1.0, specify `none` to
`--tlsDisabledProtocols`.
--tlsUseSystemCA
Allows `mongosh` to load TLS certificates already available to the
operating system's certificate authority without explicitly specifying the
certificates to the shell. You cannot turn off this behavior.
`--tlsUseSystemCA` can
still be set for backward compatibility, but it has no effect.

> **Note**
>
> This flag applies to both MongoDB connections and OIDC identity provider connections.

## Authentication Options

`--authenticationDatabase <dbname>`  

Specifies the authentication database where the specified
`--username` has been created. See
user-authentication-database.

If you do not specify a value for
`--authenticationDatabase`,
the MongoDB Shell uses the database specified in the connection
string.

`--authenticationMechanism <name>`  

Specifies the authentication mechanism the MongoDB Shell uses to
authenticate to the `mongod` or `mongos`.
If you don't specify an `authenticationMechanism` but provide user
credentials, the MongoDB Shell and drivers attempt to use
SCRAM-SHA-256. If this fails, they fall back to SCRAM-SHA-1.

- - Value
  - Description

- - SCRAM-SHA-1
  - [RFC 5802](https://tools.ietf.org/html/rfc5802) standard
    Salted Challenge Response Authentication Mechanism using the
    SHA-1 hash function.

- - SCRAM-SHA-256

  - [RFC 7677](https://tools.ietf.org/html/rfc7677) standard
    Salted Challenge Response Authentication Mechanism using the
    SHA-256 hash function.

    Requires featureCompatibilityVersion set to `4.0`.

- - MONGODB-X509
  - MongoDB TLS (Transport Layer Security) / SSL (Secure Sockets Layer) certificate authentication.

- - GSSAPI (Kerberos)
  - External authentication using Kerberos. This mechanism is
    available only in [MongoDB Enterprise](http://www.mongodb.com/products/mongodb-enterprise-advanced).

- - PLAIN (LDAP SASL)
  - External authentication using LDAP. You can also use `PLAIN`
    for authenticating in-database users. `PLAIN` transmits
    passwords in plain text. This mechanism is
    available in [MongoDB Enterprise](http://www.mongodb.com/products/mongodb-enterprise-advanced)
    and [MongoDB Atlas](https://www.mongodb.com/atlas/database).

- - \[MONGODB-OIDC\](<https://www.mongodb.com/docs/manual/core/security-oidc/>) (OpenID Connect)
  - External authentication using OpenID Connect. This mechanism is
    available in [MongoDB Enterprise](http://www.mongodb.com/products/mongodb-enterprise-advanced)
    and [MongoDB Atlas](https://www.mongodb.com/atlas/database).

\* - `MONGODB-AWS` (AWS IAM)

> - External authentication using Amazon Web Services Identity and
>   Access Management (AWS IAM) credentials. This mechanism is
>   available in [MongoDB Enterprise](http://www.mongodb.com/products/mongodb-enterprise-advanced)
>   and [MongoDB Atlas](https://www.mongodb.com/atlas/database).

`--gssapiServiceName`  

Specify the name of the service using
\[GSSAPI/Kerberos\](<https://www.mongodb.com/docs/manual/core/kerberos/>). Only required if the service does not use the default name of `mongodb`.

This option is available only in MongoDB Enterprise.
--sspiHostnameCanonicalization \<string\>
Specifies whether or not to use Hostname Canonicalization.

`--sspiHostnameCanonicalization` has the same effect as setting the
`CANONICALIZE_HOST_NAME:true|false` key-pair in the
`authMechanismProperties` portion of the
connection string.

If `--sspiHostnameCanonicalization` is set to:

- `forwardAndReverse`, performs a forward DNS lookup and then a
  reverse lookup. New in `mongosh` 1.3.0.
- `forward`, the effect is the same as setting
  `authMechanismProperties=CANONICALIZE_HOST_NAME:true`.

\- `none`, the effect is the same as setting  
`authMechanismProperties=CANONICALIZE_HOST_NAME:false`.

<!-- -->

`--oidcFlows`  

Specifies OpenID Connect flows in a comma-separated list.
The OpenID Connect flows specify how `mongosh` interacts with the identity
provider for the authentication process. `mongosh` supports the following
OpenID Connect flows:

- - OpenID Connect Flow
  - Description
- - `auth-code`
  - Default. `mongosh` opens a browser and redirects you to the identity
    provider log-in screen.

\* - `device-auth`  
- `mongosh` provides you with a URL and code to finish authentication.
  This is considered a less secure OpenID Connect flow but can be used when
  `mongosh` is run in an environment in which it cannot open a browser.

To set `device-auth` as a fallback option to `auth-code`, see the following
example:

```bash
mongosh 'mongodb://localhost/' --authenticationMechanism MONGODB-OIDC --oidcFlows=auth-code,device-auth
```

`--oidcDumpTokens`  

Specifies whether `mongosh` prints tokens with extra debugging information.
Use this option for debugging purposes only.

The `--oidcDumpTokens` option accepts the following values:

- - Value
  - Description
- - `redacted`
  - Default when you only set `--oidcDumpTokens`. Prints token debugging
    information with sensitive data redacted.
- - `include-secrets`
  - Prints token debugging information including credentials that can
    potentially authenticate to database servers.

> **Important**
>
> The `include-secrets` value exposes credentials that attackers can use for
> authentication. Only use this option when unauthorized people cannot view
> the output of `mongosh` and the credentials are necessary for diagnostic purposes.

`--oidcIdTokenAsAccessToken`  

Specifies whether `mongosh` uses the ID token received from the identity
provider instead of the access token. Use this option with identity providers
that you can't configure to provide JWT (JSON Web Token) access
tokens.
--oidcNoNonce
By default, `mongosh` sends a nonce parameter during the
OIDC (OpenID Connect) Authorization Code Flow.

If you set the `--oidcNoNonce` option, `mongosh` does not send a
nonce parameter. Use this option if your identity provider does not
support nonce values as part of authorization.
--oidcRedirectUri

`--oidcTrustedEndpoint`  

Indicates that the current connection is to a trusted endpoint that is not
Atlas or `localhost`. This ensures that access tokens are sent to the server.
Only use this option when connecting to servers that you trust.
--browser
Set `--no-browser` to disable opening browsers entirely.
--password \<password\>, -p \<password\>
Specifies a password with which to authenticate to a MongoDB database
that uses authentication. Use in conjunction with the
`--username` and
`--authenticationDatabase`
options.

To force the MongoDB Shell to prompt for a password, enter the
`--password` option as the last option and leave out the
argument.
--username \<username\>, -u \<username\>
Specifies a username with which to authenticate to a MongoDB database
that uses authentication. Use in conjunction with the
`--password` and
`--authenticationDatabase`
options.
Session Options
---------------

`--retryWrites`  

Enables retryable-writes.

For more information on sessions, see sessions.
.. disableImplicitSessions

## Field Level Encryption Options

`--cryptSharedLibPath <string>`  

> **New in version 8.2**
>

The path to the \[Automatic Encryption Shared Library\](<https://www.mongodb.com/docs/manual/core/queryable-encryption/install-library/>). The library must be version
8.2.0 or higher. Required to use automatic
encryption for the `mongosh` shell session.
--awsAccessKeyId \<string\>
An [AWS Access Key](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html)
associated with an IAM user who has `List` and `Read` permissions
for the AWS Key Management Service (KMS). `mongosh` uses the
specified `--awsAccessKeyId` to access the KMS.

`--awsAccessKeyId` is required to enable
manual-csfle-feature for the `mongosh` shell session.
`--awsAccessKeyId` requires *both* of the following command
line options:

- `--awsSecretAccessKey`
- `--keyVaultNamespace`

If `--awsAccessKeyId` is omitted, use the `Mongo()`
constructor within the shell session to enable client-side field
level encryption.

To mitigate the risk of leaking access keys into logs, consider
specifying an environmental variable to `--awsAccessKeyId`.
--awsSecretAccessKey \<string\>
An [AWS Secret Key](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html)
associated to the specified `--awsAccessKeyId`.

`--awsSecretAccessKey` is required to enable
manual-csfle-feature for the `mongosh` session.
`--awsSecretAccessKey` requires *both* of the following
command line options:

- `--awsAccessKeyId`
- `--keyVaultNamespace`

If `--awsSecretAccessKey` and its supporting options are
omitted, use `Mongo()` within the shell session to enable
client-side field level encryption.

To mitigate the risk of leaking access keys into logs, consider
specifying an environmental variable to
`--awsSecretAccessKey`.
--awsSessionToken \<string\>
An [AWS Session Token](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html)
associated to the specified `--awsAccessKeyId`.

`--awsSessionToken` is required to enable
manual-csfle-feature for the `mongosh` shell session.
`--awsSessionToken` requires *all* of the following command
line options:

- `--awsAccessKeyId`
- `--awsSecretAccessKey`
- `--keyVaultNamespace`

If `--awsSessionToken` and its supporting options are
omitted, use `Mongo()` within the shell session to enable
client-side field level encryption.

To mitigate the risk of leaking access keys into logs, consider
specifying an environmental variable to `--awsSessionToken`.
--keyVaultNamespace \<string\>
The full namespace (`<database>.<collection>`) of the collection
used as a key vault for manual-csfle-feature.
`--keyVaultNamespace` is required for enabling client-side
field level encryption for the `mongosh` shell session.
`mongosh` creates the specified namespace if it does not
exist.

`--keyVaultNamespace` requires *both* of the following
command line options:

- `--awsAccessKeyId`
- `--awsSecretAccessKey`

If `--keyVaultNamespace` and its supporting options are
omitted, use the `Mongo()` constructor within the shell
session to enable client-side field level encryption.
