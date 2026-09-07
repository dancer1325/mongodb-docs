# x.509

MongoDB supports X.509 certificate authentication for client
authentication and internal authentication of the members of replica
sets and sharded clusters.

X.509 certificate authentication requires a secure TLS/SSL connection.

## Certificate Authority

## Client X.509 Certificates

To authenticate to servers, clients can use X.509 certificates instead
of usernames and passwords.

### Client Certificate Requirements

### MongoDB User and `$external` Database

To authenticate with a client certificate, you must first add the client
certificate's `subject` as a MongoDB user in the `$external` database.
The `$external` database is the authentication-database for the user.

Each unique X.509 client certificate is for one MongoDB user.
You cannot use a single client certificate to authenticate more than one
MongoDB user.

### TLS Connection X509 Certificate Startup Warning

## Member X.509 Certificates

For internal authentication between members of sharded clusters and
replica sets, you can use X.509 certificates instead of keyfiles.

### Member Certificate Requirements

### MongoDB Configuration for Membership Authentication
