# SCRAM

Salted Challenge Response Authentication Mechanism (SCRAM) is the
default authentication mechanism for MongoDB.

When a user authenticates
themselves, MongoDB uses SCRAM to verify the supplied user credentials
against the user's `name`,
`password` and
`authentication database`.

SCRAM is based on the IETF [RFC 5802](https://tools.ietf.org/html/rfc5802) standard that defines best
practices for the implementation of challenge-response mechanisms for
authenticating users with passwords.

> **Important**
>

## Features

MongoDB's implementation of SCRAM provides:

- A tunable work factor (the iteration count)
- Per-user random salts
- Bi-directional authentication between server and client

### SCRAM Mechanisms

MongoDB supports the following SCRAM mechanisms:

- - SCRAM Mechanism
  - Description

- - `SCRAM-SHA-1`

  - Uses the SHA-1 hashing function.

    To modify the iteration count for `SCRAM-SHA-1`, see
    `scramIterationCount`.

\* - `SCRAM-SHA-256`

> - Uses the SHA-256 hashing function.
>
>   To modify the iteration count for `SCRAM-SHA-256`, see
>   `scramSHA256IterationCount`.

When you create or update a SCRAM user, you can indicate:

- the SCRAM mechanism to use
- whether the server or the client digests the password

When you use `SCRAM-SHA-256`, MongoDB requires server-side password
hashing, which means that the server digests the password. For more
information, see `db.createUser()` and
`db.updateUser()`.

## Driver Support

The minimum driver versions that support `SCRAM` are:

## Additional Information
