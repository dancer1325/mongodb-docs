# `mongos.exe`

## Synopsis

`mongos.exe` is the build of the MongoDB Shard
(i.e. `mongos`) for the Windows
platform. `mongos.exe` has all of the features of
`mongos` on Unix-like platforms and is completely compatible
with the other builds of `mongos`. In addition,
`mongos.exe` provides several options for interacting with
the Windows platform itself.

This document only references options that are unique to
`mongos.exe`. All `mongos` options are
available. See the \[mongos\](/reference/program/mongos.md) and the
\[configuration-options\](/reference/configuration-options.md) documents for more
information regarding `mongos.exe`.

To install and use `mongos.exe`, see
install-mdb-community-windows.

## Options

**\`--install\`**

Installs `mongos.exe` as a Windows Service and exits.

If needed, you can install services for multiple instances of
`mongos.exe`. Install each service with a unique `--serviceName`
and `--serviceDisplayName`. Use multiple instances only when
sufficient system resources exist and your system design requires it.

**\`--remove\`**

Removes the `mongos.exe` Windows Service. If `mongos.exe` is
running, this operation will stop and then remove the service.

`--remove` requires the `--serviceName` if you
configured a non-default `--serviceName` during the
`--install` operation.

**\`--reinstall\`**

Removes `mongos.exe` and reinstalls `mongos.exe`
as a Windows Service.

**\`--serviceName name\`**

*Default*: MongoS

Sets the service name of `mongos.exe` when running as a
Windows Service. Use this name with the `net start <name>` and
`net stop <name>` operations.

You must use `--serviceName` in conjunction with either
the `--install` or `--remove` option.

**\`--serviceDisplayName \<name\>\`**

*Default*: Mongo DB Router

Sets the name listed for MongoDB on the Services administrative
application.

**\`--serviceDescription \<description\>\`**

*Default*: Mongo DB Sharding Router

Sets the `mongos.exe` service description.

You must use `--serviceDescription` in conjunction with the
`--install` option.

For descriptions that contain spaces, you must enclose the
description in quotes.

**\`--serviceUser \<user\>\`**

Runs the `mongos.exe` service in the context of a certain user. This
user must have "Log on as a service" privileges.

You must use `--serviceUser` in conjunction with the
`--install` option.

**\`--servicePassword \<password\>\`**

Sets the password for `<user>` for `mongos.exe` when running with
the `--serviceUser` option.

You must use `--servicePassword` in conjunction with the
`--install` option.
