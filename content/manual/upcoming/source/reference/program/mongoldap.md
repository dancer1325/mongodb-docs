# `mongoldap`

*MongoDB Enterprise*

## Synopsis

MongoDB Enterprise provides
`mongoldap` for testing MongoDB's LDAP configuration options against a running LDAP server or set
of servers.

To validate the LDAP options in the configuration file, set the
`mongoldap` `--config` option to the configuration file's
path.

To test the LDAP configuration options, you must specify a `--user`
and `--password`. `mongoldap` simulates authentication to a
MongoDB server running with the provided configuration options and credentials.

`mongoldap` returns a report that includes the success or failure of
any step in the LDAP authentication or authorization procedure. Error messages
include information on specific errors encountered and potential advice for
resolving the error.

When configuring options related to LDAP authorization, `mongoldap` executes an LDAP query
constructed using the provided configuration options and username, and returns
a list of roles on the `admin` database which the user is authorized for.

You can use this information when configuring LDAP authorization roles for user access control. For example, use
`mongoldap` to ensure your configuration allows privileged users to
gain the necessary roles to perform their expected tasks. Similarly, use
`mongoldap` to ensure your configuration disallows non-privileged
users from gaining roles for accessing the MongoDB server, or performing
unauthorized actions.

When configuring options related to LDAP authentication, use `mongoldap` to ensure that the authentication
operation works as expected.

This document provides a complete overview of all command line options for
`mongoldap`.

## Installation

The `mongokerberos` tool is part of the *MongoDB Database Tools Extra*
package, and can be installed with the MongoDB Server or as a
standalone installation.

### Install with Server

### Install as Standalone

## Usage

> **Note**
>
> A full description of LDAP or Active Directory is beyond the scope of
> this documentation.

Consider the following sample configuration file, designed to support
LDAP authentication and authorization via Active Directory:

```yaml
security:
   authorization: "enabled"
   ldap:
      servers: "activedirectory.example.net"
      bind:
         queryUser: "mongodbadmin@dba.example.com"
         queryPassword: "secret123"
      userToDNMapping:
         '[
            {
               match : "(.+)",
               ldapQuery: "DC=example,DC=com??sub?(userPrincipalName={0})"
            }
         ]'
      authz:
         queryTemplate: "DC=example,DC=com??sub?(&(objectClass=group)(member:1.2.840.113556.1.4.1941:={USER}))"
setParameter:
   authenticationMechanisms: "PLAIN"
   
```

You can use `mongoldap` to validate the configuration file, which
returns a report of the procedure. You must specify a username and password
for `mongoldap`.

```bash
mongoldap --config=<path-to-config> --user="bob@dba.example.com" --password="secret123" 

```

If the provided credentials are valid, and the LDAP options in the
configuration files are valid, the output might be as follows:

```bash
Checking that an LDAP server has been specified...
[OK] LDAP server found

Connecting to LDAP server...
[OK] Connected to LDAP server

Parsing MongoDB to LDAP DN mappings..
[OK] MongoDB to LDAP DN mappings appear to be valid

Attempting to authenticate against the LDAP server...
[OK] Successful authentication performed

Checking if LDAP authorization has been enabled by configuration...
[OK] LDAP authorization enabled

Parsing LDAP query template..
[OK] LDAP query configuration template appears valid

Executing query against LDAP server...
[OK] Successfully acquired the following roles:
...

```

## Behavior

Starting in MongoDB 5.1, `mongoldap` supports prefixing LDAP
server with `srv:` and `srv_raw:`.

## Options

**\`--config=\<filename\>, -f=\<filename\>\`**

Specifies a configuration file for runtime configuration options.
The options are equivalent to the command-line
configuration options. See \[configuration-options\](/reference/configuration-options.md) for
more information.

`mongoldap` uses any configuration options related to security-ldap
or security-ldap-external for testing LDAP authentication or
authorization.

Requires specifying `--user`. May accept `--password` for
testing LDAP authentication.

Ensure the configuration file uses ASCII encoding. The `mongoldap`
instance does not support configuration files with non-ASCII encoding,
including UTF-8.

**\`--user=\<string\>\`**

Username for `mongoldap` to use when attempting LDAP authentication or
authorization.

**\`--password=\<string\>\`**

Password of the `--user` for
`mongoldap` to use when attempting LDAP authentication. Not
required for LDAP authorization.

**\`--ldapServers=\<host1\>:\<port\>,\<host2\>:\<port\>,...,\<hostN\>:\<port\>\`**

The LDAP server against which the `mongoldap` authenticates users or
determines what actions a user is authorized to perform on a given
database. If the LDAP server specified has any replicated instances,
you may specify the host and port of each replicated server in a
comma-delimited list.

If your LDAP infrastructure partitions the LDAP directory over multiple LDAP
servers, specify *one* LDAP server or any of its replicated instances to
`--ldapServers`. MongoDB supports following LDAP referrals as defined in [RFC 4511
4.1.10](https://www.rfc-editor.org/rfc/rfc4511.txt). Do not use `--ldapServers`
for listing every LDAP server in your infrastructure.

If unset, `mongoldap` cannot use \[LDAP authentication or authorization\](/core/security-ldap.md).

**\`--ldapQueryUser=\<string\>\`**

*Available in MongoDB Enterprise only.*

The identity with which `mongoldap` binds as, when connecting to or
performing queries on an LDAP server.

Only required if any of the following are true:

- Using LDAP authorization.
- Using an LDAP query for `username transformation`.
- The LDAP server disallows anonymous binds

You must use `--ldapQueryUser` with `--ldapQueryPassword`.

If unset, `mongoldap` will not attempt to bind to the LDAP server.

> **Note**
>
> Windows MongoDB deployments can use `--ldapBindWithOSDefaults`
> instead of `--ldapQueryUser` and `--ldapQueryPassword`. You cannot specify
> both `--ldapQueryUser` and `--ldapBindWithOSDefaults` at the same time.

**\`--ldapQueryPassword=\<string \| array\>\`**

**\`--ldapBindWithOSDefaults=\<bool\>\`**

*Default*: false

Available in MongoDB Enterprise for the Windows platform only.

Allows `mongoldap` to authenticate, or bind, using your Windows login
credentials when connecting to the LDAP server.

Only required if:

- Using LDAP authorization.
- Using an LDAP query for `username transformation`.
- The LDAP server disallows anonymous binds

Use `--ldapBindWithOSDefaults` to replace `--ldapQueryUser` and
`--ldapQueryPassword`.

**\`--ldapBindMethod=\<string\>\`**

*Default*: simple

*Available in MongoDB Enterprise only.*

The method `mongoldap` uses to authenticate to an LDAP
server. Use with `--ldapQueryUser` and `--ldapQueryPassword` to connect to the LDAP server.

`--ldapBindMethod` supports
the following values:

- - Value
  - Description
- - `simple`
  - `mongoldap` uses simple authentication.

\* - `sasl`  
- `mongoldap` uses SASL protocol for authentication.

If you specify `sasl`, you can configure the available SASL mechanisms
using `--ldapBindSaslMechanisms`. `mongoldap` defaults to
using `DIGEST-MD5` mechanism.

**\`--ldapBindSaslMechanisms=\<string\>\`**

*Default*: DIGEST-MD5

*Available in MongoDB Enterprise only.*

A comma-separated list of SASL mechanisms `mongoldap` can
use when authenticating to the LDAP server. The `mongoldap` and the
LDAP server must agree on at least one mechanism. The `mongoldap`
dynamically loads any SASL mechanism libraries installed on the host
machine at runtime.

Install and configure the appropriate libraries for the selected
SASL mechanism(s) on both the `mongoldap` host and the remote
LDAP server host. Your operating system may include certain SASL
libraries by default. Defer to the documentation associated with each
SASL mechanism for guidance on installation and configuration.

If using the `GSSAPI` SASL mechanism for use with
security-kerberos, verify the following for the
`mongoldap` host machine:

`Linux`  
- The `KRB5_CLIENT_KTNAME` environment
  variable resolves to the name of the client keytab-files
  for the host machine. For more on Kerberos environment
  variables, please defer to the
  [Kerberos documentation](https://web.mit.edu/kerberos/krb5-1.13/doc/admin/env_variables.html).
- The client keytab includes a
  kerberos-user-principal for the `mongoldap` to use when
  connecting to the LDAP server and execute LDAP queries.

`Windows`  
If connecting to an Active Directory server, the Windows
Kerberos configuration automatically generates a
[Ticket-Granting-Ticket](https://msdn.microsoft.com/en-us/library/windows/desktop/aa380510(v=vs.85).aspx)
when the user logs onto the system. Set `--ldapBindWithOSDefaults` to
`true` to allow `mongoldap` to use the generated credentials when
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
> - For Windows, please see the [Windows SASL documentation](https://msdn.microsoft.com/en-us/library/cc223500.aspx).

**\`--ldapTransportSecurity=\<string\>\`**

*Default*: tls

*Available in MongoDB Enterprise only.*

By default, `mongoldap` creates a TLS/SSL secured connection to the LDAP
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

Set `--ldapTransportSecurity` to `none` to disable TLS/SSL between `mongoldap` and the LDAP
server.

> **Warning**
>
> Setting `--ldapTransportSecurity` to `none` transmits plaintext information and possibly
> credentials between `mongoldap` and the LDAP server.

**\`--ldapTimeoutMS=\<int\>\`**

*Default*: 10000

*Available in MongoDB Enterprise only.*

The amount of time in milliseconds `mongoldap` should wait for an LDAP server
to respond to a request.

Increasing the value of `--ldapTimeoutMS` may prevent connection failure between the
MongoDB server and the LDAP server, if the source of the failure is a
connection timeout. Decreasing the value of `--ldapTimeoutMS` reduces the time
MongoDB waits for a response from the LDAP server.

**\`--ldapUserToDNMapping=\<string\>\`**

*Available in MongoDB Enterprise only.*

Maps the username provided to `mongoldap` for authentication to a LDAP
Distinguished Name (DN). You may need to use `--ldapUserToDNMapping` to transform a
username into an LDAP DN in the following scenarios:

- Performing LDAP authentication with simple LDAP binding, where users
  authenticate to MongoDB with usernames that are not full LDAP DNs.
- Using an `LDAP authorization query template` that requires a DN.
- Transforming the usernames of clients authenticating to Mongo DB using
  different authentication mechanisms (e.g. X.509, kerberos) to a full LDAP
  DN for authorization.

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
    `mongoldap` executes the query against the LDAP server to retrieve
    the LDAP DN for the authenticated user. `mongoldap` requires
    exactly one returned result for the transformation to be
    successful, or `mongoldap` skips this transformation.
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

When performing authentication or authorization, `mongoldap` steps through
each document in the array in the given order, checking the authentication
username against the `match` filter. If a match is found,
`mongoldap` applies the transformation and uses the output for
authenticating the user. `mongoldap` does not check the remaining documents
in the array.

If the given document does not match the provided authentication
name, `mongoldap` continues through the list of documents
to find additional matches. If no matches are found in any document,
or the transformation the document describes fails,
`mongoldap` returns an error.

`mongoldap` also returns an error if one of the transformations
cannot be evaluated due to networking or authentication failures to the
LDAP server. `mongoldap` rejects the connection request and does
not check the remaining documents in the array.

Starting in MongoDB 5.0, `--ldapUserToDNMapping`
accepts an empty string `""` or empty array `[ ]` in place of a
mapping documnent. If providing an empty string or empty array to
`--ldapUserToDNMapping`, MongoDB will map the
authenticated username as the LDAP DN. Previously, providing an
empty mapping document would cause mapping to fail.

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
`"ou=dba,dc=example,dc=com??one?(user=bob)"`. `mongoldap` executes this
query against the LDAP server, returning the result
`"cn=bob,ou=dba,dc=example,dc=com"`.


If `--ldapUserToDNMapping` is unset, `mongoldap` applies no transformations to the username
when attempting to authenticate or authorize a user against the LDAP server.

**\`--ldapAuthzQueryTemplate=\<string\>\`**

*Available in MongoDB Enterprise only.*

A relative LDAP query URL formatted conforming to [RFC4515](https://tools.ietf.org/html/rfc4515) and [RFC4516](https://tools.ietf.org/html/rfc4516) that `mongoldap` executes to obtain
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

If your query includes an attribute, `mongoldap` assumes that the query
retrieves a the DNs which this entity is member of.

If your query does not include an attribute, `mongoldap` assumes
the query retrieves all entities which the user is member of.

For each LDAP DN returned by the query, `mongoldap` assigns the authorized
user a corresponding role on the `admin` database. If a role on the on the
`admin` database exactly matches the DN, `mongoldap` grants the user the
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

If unset, `mongoldap` cannot authorize users using LDAP.

> **Note**
>
> An explanation of [RFC4515](https://tools.ietf.org/html/rfc4515),
> [RFC4516](https://tools.ietf.org/html/rfc4516) or LDAP queries is out
> of scope for the MongoDB Documentation. Please review the RFC directly or
> use your preferred LDAP resource.
