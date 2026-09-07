# Authentication on Self-Managed Deployments

Authentication is the process of verifying the identity of a client.
When access control (authorization) is
enabled, MongoDB requires all clients to authenticate themselves in
order to determine their access.

Although authentication and authorization
are closely connected, authentication is distinct from authorization:

- **Authentication** verifies the identity of a user.
- **Authorization** determines the verified user's access to resources
  and operations.

## Getting Started

To get started using access control, follow these tutorials:

- enable-access-control
- create-users
- authentication-auth-as-user

## Authentication Mechanisms

### SCRAM Authentication

Salted Challenge Response Authentication Mechanism (SCRAM) is the default authentication mechanism for
MongoDB.

For more information on SCRAM and MongoDB, see:

- SCRAM Authentication
- scram-client-authentication

### X.509 Certificate Authentication

MongoDB supports X.509 certificate authentication for client authentication and internal
authentication of the members of replica sets and sharded clusters.
X.509 certificate authentication requires a secure TLS/SSL connection.

To use MongoDB with X.509, you must use valid certificates generated and
signed by a certificate authority. The client X.509 certificates
must meet the client certificate requirements.

For more information on X.509 and MongoDB, see:

- X.509 Certificate Authentication
- x509-client-authentication

### Kerberos Authentication

[MongoDB Enterprise](http://www.mongodb.com/products/mongodb-enterprise-advanced)
supports Kerberos Authentication. Kerberos is
an industry standard authentication protocol for large client/server
systems that provides authentication using short-lived tokens that are
called tickets.

To use MongoDB with Kerberos, you must have a properly configured
Kerberos deployment, configured Kerberos service principals for MongoDB, and a Kerberos user principal added to MongoDB.

For more information on Kerberos and MongoDB, see:

- Kerberos Authentication
- /tutorial/control-access-to-mongodb-with-kerberos-authentication
- /tutorial/control-access-to-mongodb-windows-with-kerberos-authentication

### LDAP Proxy Authentication

[MongoDB Enterprise](http://www.mongodb.com/products/mongodb-enterprise-advanced)
and [MongoDB Atlas](https://www.mongodb.com/atlas/database) support
LDAP Proxy Authentication proxy
authentication through a Lightweight Directory Access Protocol (LDAP)
service.

For more information on Kerberos and MongoDB, see:

- LDAP Proxy Authentication
- /tutorial/configure-ldap-sasl-activedirectory
- /tutorial/configure-ldap-sasl-openldap
- /tutorial/authenticate-nativeldap-activedirectory

These mechanisms allow MongoDB to integrate into your
existing authentication system.

### OpenID Connect Authentication

For more information on OpenID Connect and MongoDB, see:

- OpenID Connect Authentication
- Configure MongoDB with OpenID Connect
- [OpenID Connect](https://auth0.com/docs/authenticate/protocols/openid-connect-protocol)

## Internal / Membership Authentication

In addition to verifying the identity of a client, MongoDB can require
members of replica sets and sharded clusters to authenticate their membership to their respective
replica set or sharded cluster. See inter-process-auth
for more information.
