# `mongod.exe`

## Synopsis

`mongod.exe` is the build of the MongoDB daemon
(i.e. `mongod`) for the Windows
platform. `mongod.exe` has all of the features of
`mongod` on Unix-like platforms and is completely compatible
with the other builds of `mongod`. In addition,
`mongod.exe` provides several options for interacting with
the Windows platform itself.

This document *only* references options that are unique to
`mongod.exe`. `mongod.exe` supports all
`mongod` options *except* those with documented Windows
incompatibility. See the \[mongod\](/reference/program/mongod.md) and the
\[configuration-options\](/reference/configuration-options.md) documents for more information
on `mongod` options not listed here.

To install and use `mongod.exe`, see
install-mdb-community-windows.

## Options

**\`--install\`**

Installs `mongod.exe` as a Windows Service and exits.

If needed, you can install services for multiple instances of
`mongod.exe`. Install each service with a unique `--serviceName`
and `--serviceDisplayName`. Use multiple instances only when
sufficient system resources exist and your system design requires it.

**\`--remove\`**

Removes the `mongod.exe` Windows Service. If `mongod.exe` is
running, this operation will stop and then remove the service.

`--remove` requires the `--serviceName` if you
configured a non-default `--serviceName` during the
`--install` operation.

**\`--reinstall\`**

Removes `mongod.exe` and reinstalls `mongod.exe`
as a Windows Service.

**\`--serviceName name\`**

*Default*: MongoDB

Sets the service name of `mongod.exe` when running as a
Windows Service. Use this name with the `net start <name>` and
`net stop <name>` operations.

You must use `--serviceName` in conjunction with either
the `--install` or `--remove` option.

**\`--serviceDisplayName \<name\>\`**

*Default*: MongoDB

Sets the name listed for MongoDB on the Services administrative
application.

**\`--serviceDescription \<description\>\`**

*Default*: MongoDB Server

Sets the `mongod.exe` service description.

You must use `--serviceDescription` in conjunction with the
`--install` option.

For descriptions that contain spaces, you must enclose the
description in quotes.

**\`--serviceUser \<user\>\`**

Runs the `mongod.exe` service in the context of a certain user. This
user must have "Log on as a service" privileges.

You must use `--serviceUser` in conjunction with the
`--install` option.

**\`--servicePassword \<password\>\`**

Sets the password for `<user>` for `mongod.exe` when running with
the `--serviceUser` option.

You must use `--servicePassword` in conjunction with the
`--install` option.
