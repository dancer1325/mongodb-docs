# Release Notes

## v2.5.10

*Released November 28, 2025*

New features released in this version:

- STREAMS-1136 - Added ability to drill into stream processor
  attributes.

Bug Fixes:

- Fixes an issue with pasting text in the middle of a string.
- MONGOSH-2991 - Fixes a logging credential redaction issue
  for special characters.

[Full release notes available on JIRA](https://jira.mongodb.org/issues/?jql=project%20%3D%20MONGOSH%20AND%20fixVersion%20%3D%202.5.10).

## v2.5.9

*Released October 30, 2025*

New features released in this version:

- MONGOSH-2949 - Added the `sp.listWorkspaceDefaults` command.
- MONGOSH-2139 - Added shard migration helpers.
- MONGOSH-2674 - Added functionality to disable "blocking cursor
  call" warnings.

Bug Fixes:

- Fixes OIDC-related issues.
- Includes improvements on `buildInfo()`, logging, and more.

[Full release notes available on JIRA](https://jira.mongodb.org/issues/?jql=project%20%3D%20MONGOSH%20AND%20fixVersion%20%3D%202.5.9).

## v2.5.8

*Released September 9, 2025*

`mongosh` now uses the following driver versions:

- [Node.js driver 6.19.0](https://github.com/mongodb/node-mongodb-native/releases/tag/v6.19.0).

New features released in this version:

- MONGOSH-2514 - Enable public preview support for Queryable Encryption
  prefix/suffix/substring search.

Bug Fixes:

- MONGOSH-2635 - Fixes an issue for homebrew installations using
  `Node.js` versions ^20.19.5, ^22.18.0, or ^24.3.0 where the tab autocomplete
  in `mongosh` could execute code up to and including potentially
  destructive operations.
- Fixes an issue that prevented HTTP tracking and other debug information from
  OIDC authentication flows being captured in the logs.

[Full release notes available on JIRA](https://jira.mongodb.org/issues/?jql=project%20%3D%20MONGOSH%20AND%20fixVersion%20%3D%202.5.8).

## v2.5.7

*Released August 29, 2025*

`mongosh` now uses the following versions for dependencies:

- [Node.js driver version 6.18.0](https://github.com/mongodb/node-mongodb-native/releases/tag/v6.18.0),
  which lets you use the `sort` option in
  `updateOne()` and
  `replaceOne()` commands.
- The latest OIDC (OpenID Connect) plugin, which enables support
  for the `id_token_signing_alg_values_supported` metadata value.

[Full release notes available on JIRA](https://jira.mongodb.org/issues/?jql=project%20%3D%20MONGOSH%20AND%20fixVersion%20%3D%202.5.7).

## v2.5.6

*Released July 18, 2025*

> **Important**
>
> This release includes fixes for security issues. Upgrade to `mongosh`
> 2.5.6 at your earliest convenience.

- Upgrades to Node.js version 20.19.4. For more information, see the
  [July 15 2025 Node.js security release](https://nodejs.org/en/blog/vulnerability/july-2025-security-releases).

New features released in this version:

- MONGOSH-1680 - Add RHEL9 support for `ppc64le` and `s390x` distributions.
- MONGOSH-2370 - Add `disableSchemaSampling=true` to disable schema sampling for autocomplete.
- MONGOSH-2371 - Add `skipStartupWarnings` flag to skip warnings on demand.
- New autocompleter (feature-flagged) - Enable it by setting the environment variable `USE_NEW_AUTOCOMPLETE=true`. Try it out and share your feedback!

[Full release notes available on JIRA](https://jira.mongodb.org/issues/?jql=project%20%3D%20MONGOSH%20AND%20fixVersion%20%3D%202.5.6).

## v2.5.5

*Released July 3, 2025*

Bug Fixes:

- MONGOSH-2233 - Multi-line input on homebrew made `mongosh` hang

[Full release notes available on JIRA](https://jira.mongodb.org/issues/?jql=project%20%3D%20MONGOSH%20AND%20fixVersion%20%3D%202.5.5).

## v2.5.3

*Released June 18, 2025*

New features released in this version:

- MONGOSH-1493 - Add `.finish()` method to ExplainableCursor.
- MONGOSH-1996 - Add `collection.getShardLocation` helper method.

[Full release notes available on JIRA](https://jira.mongodb.org/issues/?jql=project%20%3D%20MONGOSH%20AND%20fixVersion%20%3D%202.5.3).

## v2.5.2

*Released May 14, 2025*

> **Important**
>
> This release includes fixes for security issues. Upgrade to `mongosh`
> 2.5.2 at your earliest convenience.

- Upgrades to Node.js version 20.19.2. For more information, see the
  [May 14 2025 Node.js security release](https://nodejs.org/en/blog/vulnerability/may-2025-security-releases).

[Full release notes available on JIRA](https://jira.mongodb.org/browse/MONGOSH-2235).

## v2.5.1

*Released May 7, 2025*

`mongosh` now uses the following driver versions:

- [Node.js driver 6.16.0](https://github.com/mongodb/node-mongodb-native/releases/tag/v6.16.0).

Bug Fixes:

- MONGOSH-2153 - Added padding information to `Binary.fromPackedBits`.

[Full release notes available on JIRA](https://jira.mongodb.org/issues/?jql=project%20%3D%20MONGOSH%20AND%20fixVersion%20%3D%202.5.1).

## v2.5.0

*Released April 9, 2025*

New features released in this version:

- MONGOSH-1873 - Support new BSON vector types in mongosh
- MONGOSH-1649 - Add automerge information to sh.status()
- MONGOSH-1100 - Add Mongo bulkWrite API
- MONGOSH-1919 - Add sh.isConfigShardEnabled and listShards helpers

[Full release notes available on JIRA](https://jira.mongodb.org/issues/?jql=project%20%3D%20MONGOSH%20AND%20fixVersion%20%3D%202.5.0).

## v2.4.2

*Released March 6, 2025*

New feature released in this version:

- MONGOSH-1926 - Added the `sh.moveRange` helper method.

[Full release notes available on JIRA](https://jira.mongodb.org/issues/?jql=project%20%3D%20MONGOSH%20AND%20fixVersion%20%3D%202.4.2).

## v2.4.0

*Released February 24, 2025*

`mongosh` now uses the following driver versions:

- [Node.js driver 6.13.0](https://github.com/mongodb/node-mongodb-native/releases/tag/v6.13.0).
- [Node.js BSON package 6.10.3](https://github.com/mongodb/js-bson/releases/tag/v6.10.3).

New features released in this version:

- MONGOSH-2013 - Added a `history()` command, which returns an array of
  all previously run commands.
- MONGOSH-1090 - Added a `log.getPath()` command, which returns the path
  to the currently active log file.
- MONGOSH-1995 - Added help output for logging commands.
- Added the following logging configuration options, which you can set directly
  in the configuration file, or by calling
  the config api.
  - MONGOSH-1988 - Disable logging by setting `disableLogging: true`.
  - MONGOSH-1983, MONGOSH-2012 - Specify a custom path for
    log files by setting `logLocation`.
  - MONGOSH-1986 - Compress log files by setting
    `logCompressionEnabled: true`.
  - MONGOSH-1984 - Limit the number of days to keep log files by
    setting `logRetentionDays` (default 30).
  - MONGOSH-1985 - Limit the maximum size of the logs directory by
    setting `logRetentionGB` (default uncapped).
  - MONGOSH-1987 - Limit the maximum number of log files by setting
    `logMaxFileCount` (default 100).

Bug Fixes:

- MONGOSH-1914 - Fixed an issue where multiple `mongosh` processes
  could attempt to remove outdated log files at the same time, leading to one
  process terminating with `Error: ENOENT: no such file or directory`.
- MONGOSH-2002 - Improved error messaging when attempting to serialize
  a cursor.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=42122).

## v2.3.9

*Released February 5, 2025*

> **Important**
>
> This release includes fixes for security issues. Upgrade to `mongosh`
> 2.3.9 at your earliest convenience.

- Upgrades Node.js version to 20.18.2. Node 20.18.2 resolves security issues.
  For more information, see the [January 21, 2025 Node.js security release](https://nodejs.org/en/blog/release/v20.18.2).
- Stricter validation of user input to address
  [CVE-2025-1691](https://nvd.nist.gov/vuln/detail/CVE-2025-1691),
  [CVE-2025-1692](https://nvd.nist.gov/vuln/detail/CVE-2025-1692),
  and [CVE-2025-1693](https://nvd.nist.gov/vuln/detail/CVE-2025-1693).

New features released in this version:

- MONGOSH-1989 - Added ability to log custom messages in scripts by
  calling `log.[debug|info|warn|error|fatal]()`

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=41759).

## v2.3.8

*Released January 6, 2025*

Contains internal enhancements and improvements.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=41730).

## v2.3.7

*Released December 17, 2024*

Bug Fixes:

- MONGOSH-1943 - Fix cause of rare flaky "Mongosh not initialized yet" errors

[Full release notes available on JIRA](https://jira.mongodb.org/browse/MONGOSH-1922?jql=project%20%3D%20MONGOSH%20AND%20fixVersion%20%3D%202.3.7).

## v2.3.6

*Released December 13, 2024*

Contains internal enhancements and improvements.

[Full release notes available on JIRA](https://jira.mongodb.org/browse/MONGOSH-1922?jql=project%20%3D%20MONGOSH%20AND%20fixVersion%20%3D%202.3.6).

## v2.3.5

*Released December 12, 2024*

Bug Fixes:

- MONGOSH-1935 - Retry connection without system certificates in case
  of TLS errors
- MONGOSH-1632 - Add Node.js patch for OpenSSL Windows build fix

[Full release notes available on JIRA](https://jira.mongodb.org/browse/MONGOSH-1922?jql=project%20%3D%20MONGOSH%20AND%20fixVersion%20%3D%202.3.5).

## v2.3.4

*Released November 27, 2024*

New features released in this version:

- MONGOSH-1920 - Add options object to stream processor start, stop,
  and drop commands
- MONGOSH-1864 - Add stream processor modify command

Bug Fixes:

- MONGOSH-1917, MONGOSH-1905 - Include `nonce` in OIDC
  requests by default. Use the `--oidcNoNonce` option to suppress this
  behavior if your IdP (Identity Provider) doesn't support it.
- MONGOSH-1895 - Suppress experimental warning for Node.js 23

[Full release notes available on JIRA](https://jira.mongodb.org/browse/MONGOSH-1922?jql=project%20%3D%20MONGOSH%20AND%20fixVersion%20%3D%202.3.4).

## v2.3.3

*Released October 30, 2024*

New features released in this version:

- MONGOSH-1326 - Adds `shardedDataDistribution` to
  `sh.status`.
- MONGOSH-1838 - Accounts for orphan documents in
  `getShardDistribution()` helper.

Bug Fixes:

- MONGOSH-1868 - Aligns database and collection
  aggregate functions
- MONGOSH-1608 - `mongosh` should throw an error when
  trying to drop a non-primary index
- MONGOSH-1868 - Adds support for running aggregate
  database with a single stage
- MONGOSH-1867 - Fixes incorrect `db.createView.help`.
- MONGOSH-1697 - Updates the `help()` text for the
  `find` and
  `findOne` methods.
- MONGOSH-1703 Fixes invalid regular expression error in
  `db.currentOp`.

[Full release notes available on JIRA](https://jira.mongodb.org/issues/?jql=project%20%3D%20MONGOSH%20AND%20fixVersion%20%3D%202.3.3).

## v2.3.2

*Released October 8, 2024*

Contains internal enhancements and improvements.

- Fixes startup performance regression introduced in `v2.3.1`.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=40645).

## v2.3.1

*Released September 5, 2024*

Bug fixes in this release:

- COMPASS-8252 - Remove certificates without issuer from the TLS CA list
- MONGOSH-1859 - `ISODate()` now passes non-string arguments to `new Date()`

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=40529).

## v2.3.0

*Released August 16, 2024*

New features released in this version:

- MONGOSH-1550 - Adds support for Queryable Encryption Range queries and removes support for the Range Preview version. Removes out-of-the-box support for automatic encryption on deprecated Linux operating systems.
- MONGOSH-1827 - Adds configuration support for proxies in environment variables
- MONGOSH-1852 - `--tlsUseSystemCA` is enabled by default
- MONGOSH-1845 - Adds debug flag for dumping OIDC tokens to output

Bug fixes in this release:

- MONGOSH-1136 - mongosh should use stderr for password prompt
- MONGOSH-1425 - Expand fallback condition for `$collStats` to command form to account for ADF
- MONGOSH-1820 - `fields` option is not working as expected in findAndModify

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=40317).

## v2.2.15

*Released July 30, 2024*

New features in this release:

- MONGOSH-1848 - Added helper for `shardMoveCollection` and `unshardCollection` commands.

Bug fixes in this release:

- MONGOSH-1327 - `sh.status()` now shows a full list of tag ranges in verbose mode only.
- MONGOSH-1837 - ReadPreference options specified now apply to admin commands.
- MONGOSH-1392 - The `mongodb-redact` dependency library has been updated to version `1.1.2`. This change should
  help `mongosh` processing time for large base64 input data.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=40244).

## v2.2.12

*Released July 11, 2024*

Contains internal enhancements and improvements.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=40229).

## v2.2.11

*Released July 09, 2024*

Contains internal enhancements and improvements.

Fixes a bug that prevented users from passing options to the
`explain.find()` method:

- MONGOSH-1670 - explain().find() ignores collation

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=40061).

## v2.2.10

*Released June 24, 2024*

Contains internal enhancements and improvements.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=39987).

## v2.2.9

*Released June 14, 2024*

- `mongosh` now uses version 6.7.0 of the `Node.js driver`.
- MONGOSH-1785 - `mongosh` now supports Ubuntu 24.04.

## v2.2.6

*Released May 15, 2024*

`mongosh` now uses version 6.6.2 of the `Node.js driver`.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=39184).

## v2.2.5

*Released April 22, 2024*

Performance improvements:

- MONGOSH-1759 – Improves `mongosh` startup time approximately
  40% by disabling startup snapshot compression in boxed node.
- MONGOSH-1765 – `mongosh` now skips waiting for server command results
  during startup in non-interactive mode.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=39062).

## v2.2.4

*Released April 15, 2024*

Upgrades to Node.js version 20.12.2. Node 20.12.2 resolves security issues. Vulnerabilities
are unlikely to affect typical `mongosh` users. For more information, see the
[April 10, 2024 Node.js security release](https://nodejs.org/en/blog/vulnerability/april-2024-security-releases-2/).

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=38886).

## v2.2.3

*Released April 4, 2024*

Issues fixed:

- MONGOSH-1752 - Updates Node.js version to 20.12.1. Node 20.12.1 resolves
  security issues. Vulnerabilities are unlikely to affect typical `mongosh` users. For
  more information, see the [April 3, 2024 Node.js security release](https://nodejs.org/en/blog/vulnerability/april-2024-security-releases/).
- MONGOSH-1682 - Fixed a race condition that could lead to commands, including
  sensitive information, not being properly redacted from the history file.
- MONGOSH-1688 - Improved cursor iteration performance by ~60%.
- MONGOSH-1751 - Improved startup performance for programmatic usage by
  addressing a bug in our update notification manager.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=38351).

## v2.2.2

*Released March 26, 2024*

Fixes a bug where connections that use OIDC workforce authentication
caused an error:

- MONGOSH-1743 - use JS Proxy for forwarding "lazy-loaded"
  webpack function exports.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=38341).

## v2.2.1

*Released March 19, 2024*

Fixes a bug in 2.2.0 where `require('<module>')` caused an error in
script mode:

- MONGOSH-1738 - `require` not working in script mode.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=38287).

## v2.2.0

*Released March 11, 2024*

> **Warning**
>
> This release is affected by a bug, fixed in 2.2.1, where `require('<module>')`
> caused an error in script mode.

Performance improvements:

- MONGOSH-1605 – `mongosh` uses Node.js startup
  snapshots again to improve initialization performance.
- MONGOSH-1721 – `mongosh` now defaults to `--quiet` in
  non-interactive mode. For example, using `--json` or loading files
  from the command line without also specifying `--shell`. Users who
  do not want this behavior need to specify `--no-quiet`.
- MONGOSH-1720 – Script execution in non-interactive mode becomes
  significantly faster by replacing the underlying evaluation mechanism.

Node.js:

- `mongosh` now uses version 6.5.0 of the `Node.js driver`.
- NODE-5981 – Improved compliance for the Node.js driver. `mongosh`
  enters `directConnection=true` by default when only a single host/port
  is given on the command line. This ignores `readPreference` options
  and consistently applies a `primaryPreferred` read preference, even
  if a read preference is specified on the connection string or on the
  individual command.

OIDC functionality:

- COMPASS-7437 – `mongosh` will not request default OIDC scopes
  that are not supported by the Identity Provider.
- MONGOSH-1712 – The `--tlsUseSystemCA` flag now also applies
  to HTTP requests made to the Identity Provider, to better accommodate
  customers behind TLS-terminating firewalls.

Issues fixed:

- MONGOSH-1667 – `passwordPrompt()` works as originally intended.
- MONGOSH-1702 – No more flaky deprecation warnings showing up for
  macOS homebrew users.
- MONGOSH-1617 – Piping scripts into `mongosh` together with
  custom prompts from your `.mongoshrc.js` works consistently now.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=38080).

## v2.1.5

*Released February 19, 2024*

Upgrades to Node.js version 20.11.1. Node 20.11.1 resolves security issues.
For more information, see the [February 2024 Node.js security release](https://nodejs.org/en/blog/vulnerability/february-2024-security-releases/)
and [CVE-2024-24806](https://nvd.nist.gov/vuln/detail/CVE-2024-24806).

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?version=37926&styleName=&projectId=17381).

## v2.1.4

*Released February 7, 2024*

- MONGOSH-1198 - Shows the code for an error with the error response.
- MONGOSH-1669 - Allows OIDC device auth flow without an `id_` token.
- MONGOSH-1679 - Improves error message reading from a secondary.
- MONGOSH-1706 - Accounts for unsharded collections becoming part of
  the sharding catalog. This ensures forward compatibility with upcoming server
  versions.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?version=37881&styleName=&projectId=17381).

## v2.1.3

*Released January 29, 2024*

- MONGOSH-1631 - Adds support for the new `type` field when creating
  search indexes for `runCommand`, `createSearchIndex`, and
  `createSearchIndexes` commands.
- MONGOSH-1664 - Removes tests for the `validate` command background
  option.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=37823).

## v2.1.1

*Released December 5, 2023*

- MONGOSH-1628 - Statically linking kerberos leads to OpenSSL
  version conflict on RHEL8 distros
- MONGOSH-1651 - Sample sessions sent to telemetry

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=37380).

## v2.1.0

*Released November 21, 2023*

- MONGOSH-1621 and NODE-5709 – Homebrew users who were automatically upgraded
  to Node.js 21 stop seeing deprecation warnings.
- MONGOSH-1452 and NODE-5040 – `mongosh` now displays BSON objects in a format
  that is more consistent with other `mongosh` output. Also, many BSON objects now support syntax
  highlighting.
- MONGOSH-1527 – You can now iterate mongosh cursors with idiomatic
  syntax: `for (const doc of db.coll.find()) { }`. Previously, `mongosh` only supported `.forEach`
  syntax for iteration.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=36935).

## v2.0.2

*Released October 16, 2023*

- Upgrades to Node.js version 20.8.1. Node 20.8.1 resolves security
  issues. For more information, see
  [Node.js security releases](https://nodejs.org/en/blog/vulnerability/october-2023-security-releases) and
  [CVE-2023-45133](https://nvd.nist.gov/vuln/detail/CVE-2023-45133).
- [CVE-2023-45143](https://nvd.nist.gov/vuln/detail/CVE-2023-45143)
  affects the `fetch()` API that is available in `mongosh` 2.x.

> **Important**
>
> `mongosh` 1.x does not address the security issues in the previous list.
> For improved security, upgrade to `mongosh` 2.0.2.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=36795).

## v2.0.1

*Released September 14, 2023*

- MONGOSH-1346 - Group download center packages by platform.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=36738).

## v2.0.0

*Released September 6, 2023*

- Added support for these constructors:

  - `Binary.createFromBase64()`
  - `Binary.createFromHexString()`
  - `ObjectId.createFromBase64()`
  - `ObjectId.createFromHexString()`

- `mongosh` returns binary values as `Binary.createFromBase64( <base64String> )` values instead of `Binary( Buffer.from( <base64String> ) )` values. For example:
  `binaryValue:Binary.createFromBase64( "SGVsbG8gV29ybGQhCg==" )`

  For additional details, see `Binary.createFromBase64()`.

- 

- The following `config.version` fields were removed and aren't
  returned in the `sh.status()` output:

  - `minCompatibleVersion`
  - `currentVersion`
  - `excluding`
  - `upgradeId`
  - `upgradeState`

  To obtain version information, see the feature compatibility version (FCV).

- Removes support for Free Monitoring
  helper functions:

  - `db.getFreeMonitoringStatus`
  - `db.enableFreeMonitoring`
  - `db.disableFreeMonitoring`

### Compatibility Considerations

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=36482).

## v1.10.6

*Released August 25, 2023*

- MONGOSH-1533 - Display a notification banner when a new
  `mongosh` release is available.
- MONGOSH-923 - Disable and hide `getLastError` when
  connecting to a cluster older than 5.1.0.
- MONGOSH-1539 - Add support for Debian 12.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=36540).

## v1.10.5

*Released August 11, 2023*

Provides a programmatically accessible list of
`mongosh` [downloads](https://downloads.mongodb.com/compass/mongosh.json)
that can be accessed through your application.

## v1.10.4

*Released August 10, 2023*

- MONGOSH-1140 - `mongosh` now officially supports
  Amazon Linux 2023 on all architectures.
- MONGOSH-1142 - `mongosh` now officially supports RHEL9 on
  all architectures.
- MONGOSH-1146 - `mongosh` now officially supports Ubuntu
  22.04 and Debian 12 on all architectures.
- MONGOSH-1546 - `mongosh` now produces Linux release
  artifacts that support using OpenSSL 3 on arm64 architectures.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=36463).

## v1.10.3

*Released July 31, 2023*

Updates environment variables related to telemetry.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=36452).

## v1.10.2

*Released July 28, 2023*

- Inverts and fixes passwordless-auth-mechanism check
- MONGOSH-1495 Remove argument validation for db.killOp()
- MONGOSH-1499 Rename configureQueryAnalyzer option to match server
- MONGOSH-1449 Cap number of log files to 100
- MONGOSH-1496 Do not include crypt shared library version in buildInfo

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=36247).

## v1.10.1

*Released June 21, 2023*

- Upgrades to Node.js version 16.20.1. Node 16.20.1 addresses security
  issues. For more information, see
  [Node.js security releases](https://nodejs.org/en/blog/vulnerability/june-2023-security-releases).
- MONGOSH-1286 `mongosh --build-info` now lists driver
  dependency versions.
  - You can access driver dependency versions within the shell by
    running the new `buildInfo()` function.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=36225).

## v1.10.0

*Released June 14, 2023*

- MONGOSH-1469 Node driver for MongoDB 5.6.0.
- MONGOSH-1432 Added helper for the
  `checkMetadataConsistency` command. For details, see the
  [driver command example](https://www.mongodb.com/docs/drivers/node/v5.6/fundamentals/run-command/#example).
- MONGOSH-1442 Added helpers for shard key selection:
  - `db.collection.analyzeShardKey( key )`
  - `db.collection.configureQueryAnalyzer( { mode, sampleRate } )`

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=36119).

## v1.9.1

*Released May 25, 2023*

- Internal improvements for reporting and monitoring.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=36079).

## v1.9.0

*Released May 17, 2023*

- `mongosh` supports the new Queryable Encryption protocol. Starting
  in v1.9.0, `mongosh` is not compatible with MongoDB server versions
  **earlier than 7.0** when using Queryable Encryption.
  - When using queryable encryption on pre-7.0 servers, you can decrypt
    encrypted data, but you cannot insert or query data.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=35985).

## v1.8.1

*Released April 24, 2023*

`mongosh` now uses version 5.3.0 of the `Node.js driver`.

- MONGOSH-1304 `rs.reconfig()` will no longer automatically retry operations
- MONGOSH-1413 This is the first release that uploads to PPAs for Amazon 2023 after the distro rename

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=35689).

## v1.8.0

*Released February 28, 2023*

Autocomplete suggests completions for database level aggregation stages.

### Compatibility Changes

- `EJSON.stringify` no longer accepts a `{{strict}}` option.
- These methods are removed:
  - ObjectId.prototype.generate
  - ObjectId.prototype.getInc
  - ObjectId.prototype.get_inc
  - ObjectId.getInc
- 
- 

### Updates in 1.8.0

- MONGOSH-1358 Updates for the 5.1.0 `Node.js driver`.
- MONGOSH-1336 Performance improvement some use cases including
  the `--version` and `--build-info` flags.
- MONGOSH-1316 Surfaces the `createEncryptedCollection`
  helper method.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=35534).

## v1.7.1

*Released February 16, 2023*

- MONGOSH-1378 Fixes connectivity issues when `mongosh` is
  installed using Homebrew.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=35548).

## v1.7.0

*Released February 10, 2023*

- MONGOSH-57 Display a warning when connecting to databases
  that mimic MongoDB.
- MONGOSH-545 To get the current connection string, use
  `db.getMongo().getURI()`.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=35363).

## v1.6.2

*Released January 9, 2023*

- Improves `mongosh` startup time.
- `mongosh` now uses Node.js driver 4.13.0.
- Improves error messages.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?version=35137&projectId=17381).

## v1.6.1

*Released December 1, 2022*

- MONGOSH-1320: Fixes a startup bug related to Docker and
  similar environments.
- MONGOSH-1050: Adds support for the
  `convertShardKeyToHashed()` helper method.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=34525).

## v1.6.0

*Released September 20, 2022*

- MONGOSH-1299: `mongosh` now uses Node.js driver 4.10.0.
- MONGOSH-1254: Adds the `sh.getShardedDataDistribution()`
  helper method. This method runs the `$shardedDataDistribution`
  aggregation stage and returns a cursor for the result.
- MONGOSH-1266: The KeyVault `getKey()` and
  `getKeyByAltName()` methods now return Documents.
- MONGOSH-1249: Adds a `--json` flag for use with
  `--eval` commands.
- MONGOSH-1287: `cursor.count()` is now deprecated. Instead,
  use `countDocuments()` and
  `estimatedDocumentCount()`.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=34315).

## v1.5.4

*Released July 31, 2022*

Fixes a potential data corruption bug in
`KeyVault.rewrapManyDataKey()` when rotating encrypted data
encryption keys backed by Azure or GCP key services.

In previous versions of `mongosh`, this bug occurs when an
Azure-backed or GCP-backed data encryption key being rewrapped requires
fetching an access token for decryption of the data encryption key.

As a result of this bug, all data encryption keys being rewrapped are
replaced by new randomly generated material, destroying the original key
material.

To mitigate potential data corruption, upgrade `mongosh` to v1.5.4 or
higher before using `KeyVault.rewrapManyDataKey()` to rotate
Azure-backed or GCP-backed data encryption keys. You should always
create a backup of the key vault collection before key rotation.

## v1.5.3

*Released July 29, 2022*

Updates telemetry internals.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?version=34298&projectId=17381).

## v1.5.2

*Released July 27, 2022*

`mongosh` now uses Node.js driver 4.8.1.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=34143).

## v1.5.1

*Released July 14, 2022*

- MONGOSH-1194 - `mongosh` supports multiple `--eval` arguments.
- `mongosh` now uses [Node.js driver 4.8.0](https://github.com/mongodb/node-mongodb-native/releases/tag/v4.8.0).

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?version=33699&projectId=17381).

## v1.5.0

*Released June 2, 2022*

- MONGOSH-1138 – `mongosh` now supports Queryable Encryption.
- MONGOSH-1169 – `mongosh` now supports FIPS-compliant mode.
- `mongosh` now uses Node.js version 16.x.
- `mongosh` no longer provides per-distribution `mongosh` linux packages. You
  can still get .rpm, .deb and .tgz packages through your package manager
  but the naming convention may change slightly.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=33985).

## v1.4.2

*Released May 17, 2022*

- MONGOSH-1139 - Adds Debian 11 support for `mongosh`.
- MONGOSH-1183 - `cursor.allowDiskUse()` now accepts `true`
  or `false`.
- MONGOSH-1204 - Adds visual identifier for Queryable
  Encryption collections in `show collections`.
- MONGOSH-1207 - Adds Queryable Encryption helpers.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=33637).

## v1.4.1

*Released May 12, 2022*

- MONGOSH-1118 - Bundles and uses a CSFLE shared library in
  place of `mongocryptd`.
- MONGOSH-1217 - Introduces partial support for Queryable
  Encryption.
- MONGOSH-1178 - Uses [Node.js driver 4.6.0](https://github.com/mongodb/node-mongodb-native/releases/tag/v4.6.0).

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=33226).

## v1.3.1

*Released March 21, 2022*

- MONGOSH-1163 - `mongosh` now uses Node.js 14.19.1. Node
  14.19.1 includes an OpenSSL version that addresses
  [CVE-2022-0778](https://openssl-library.org/news/vulnerabilities/index.html#2022).

## v1.3.0

*Released March 17, 2022*

- MONGOSH-856 - Kerberos feature parity with the legacy shell
  is done now, with the last command line option now also working as it
  did in the legacy shell.
- MONGOSH-1013 - KMIP support for CSFLE. More specifically, you
  can provide per-KMS-provider TLS options when creating your
  CSFLE-enabled connections now.
- MONGOSH-1151 - Support for snapshot reads, now also in
  mongosh.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=33190).

## v1.2.3

*Released March 10, 2022*

- MONGOSH-1121 - Support the `commitQuorum` parameter for the
  `createIndexes()` method.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=32989).

## v1.2.2

*Released February 25, 2022*

- MONGOSH-1134 - Internal bug fix required to re-enable
  Homebrew installation.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=32988).

## v1.2.1

*Released February 24, 2022*

- MONGOSH-1063 - You can now create a global `mongosh`
  configuration file.
- MONGOSH-959 – You can now use the config.reset method to reset a configuration setting to
  the default value.
- MONGOSH-1133 – `mongosh` adds a `--tlsUseSystemCA`
  option that causes `mongosh` to attempt to load system
  certificates as well as the built-in certificates.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=32780).

## v1.1.9

*Released January 18, 2022*

New features in this release:

- MONGOSH-1015 – `mongosh` no longer overrides `appName` if it
  was present in the connection string.
- MONGOSH-1073 – You can now pass BSON number objects to the
  legacy BSON number constructors. For example,
  `NumberInt(NumberInt(n))` now works like it did in the legacy shell.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=32728).

## v1.1.8

*Released January 11, 2022*

New features in this release:

- `mongosh` now uses [Node.js driver 4.3.0](https://github.com/mongodb/node-mongodb-native/releases/tag/v4.3.0).
- Provides PGP signatures for uploaded tarballs.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=32594).

## v1.1.7

*Released December 14, 2021*

- `mongosh` now uses [Node.js driver 4.2.2](https://github.com/mongodb/node-mongodb-native/releases/tag/v4.2.2).
- Minor bug fixes.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=32512).

## v1.1.6

*Released December 2, 2021*

New features in this release:

- `mongosh` now uses [Node.js driver 4.2.1](https://github.com/mongodb/node-mongodb-native/releases/tag/v4.2.1).

Bug fixes in this release:

- Fixes the way `try`, `catch`, `finally` works if no exception
  was thrown in the `try` block.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=32498).

## v1.1.5

*Released December 1, 2021*

Minor bug fixes.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=32485).

## v1.1.4

*Released November 24, 2021*

Minor bug fixes.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=32336).

## v1.1.2

*Released November 5, 2021*

New features in this release:

- `mongosh` now uses the following driver versions:
  - [Node.js driver 4.1.4](https://github.com/mongodb/node-mongodb-native/releases/tag/v4.1.4).
  - [Node.js BSON package 4.5.4](https://github.com/mongodb/js-bson/releases/tag/v4.5.4).
- `mongosh` release tarballs now include manpages.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=32313).

## v1.1.1

*Released October 28, 2021*

- Provides autocompletion for additional aggregation stages.
- Minor bug fixes.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=32226).

## v1.1.0

*Released October 7, 2021*

New features in this release:

- Adds support for the `edit` command and `$EDITOR` variable.
- Auto-complete for databases and collections is now case-insensitive.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=32095).

## v1.0.7

*Released September 22, 2021*

New features in this release:

`mongosh` now uses the following driver versions:

- [Node.js driver 4.1.2](https://github.com/mongodb/node-mongodb-native/releases/tag/v4.1.2).
- [Node.js BSON package 4.5.2](https://github.com/mongodb/js-bson/releases/tag/v4.5.2).
- [Node.js BSON extension 4.0.1](https://github.com/mongodb-js/bson-ext/releases/tag/v4.0.1).
- [Node.js driver Kerberos addon 1.1.7](https://github.com/mongodb-js/kerberos/releases/tag/v1.1.7).
- [Node.js driver CSFLE addon 1.2.7](https://github.com/mongodb/libmongocrypt/releases/tag/node-v1.2.7).

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?version=32032&projectId=17381).

## v1.0.6

*Released September 14, 2021*

New features in this release:

- You can now run `sh.status()` when not connected to a
  `mongos`, for example when connected to a
  \[config server\](<https://www.mongodb.com/docs/manual/core/sharded-cluster-config-servers/>).
- `db.setSecondaryOk()`, `mongo.setSecondaryOk()`, and
  `rs.secondaryOk()` methods are reintroduced but **deprecated**.
  These methods alias to `mongo.setReadPref()`.
- When you enter multiline input into the shell, single-line
  `// comments` are now preserved as `/* comments */` in the
  history entry.
- The aggregation-pipeline parameter is now optional for
  `db.collection.watch()`, `db.watch()`, and
  `Mongo.watch()`.

Bug Fixes in this Release:

- `mongosh` now runs aggregations with \[`\$out`\](<https://www.mongodb.com/docs/manual/reference/operator/aggregation/out/>) or
  \[`\$merge`\](<https://www.mongodb.com/docs/manual/reference/operator/aggregation/merge/>) immediately, and not lazily once the result of the
  aggregation is accessed.
- Using the legacy `NumberLong()` method no longer truncates numbers
  outside of the 32-bit range.

[Full release notes available on JIRA](https://jira.mongodb.org/secure/ReleaseNote.jspa?projectId=17381&version=31793).

## v1.0.5

*Released August 12, 2021*

New features in this release:

- You can use `config.set('maxTimeMS', <number>)` to set a default
  `maxTimeMS` value for operations. `maxTimeMS` specifies a time
  limit in milliseconds within which the operation must complete.

> **Note**
>
> `config` settings persist across sessions.

- On Windows, you can start `mongosh` by double-clicking on the
  `.exe` file. When you do, `mongosh` prompts you for a connection
  string to connect to your deployment.
- The log files created by `mongosh` follow the format of the
  `mongod`, `mongos`, or `mongocryptd`. Meaning, the log files are
  newline-delimited JSON with the same set of fields used by the server.

## v1.0.4

*Released August 4, 2021*

New features in this release:

- `mongosh` now uses version `4.1.0` of the Node.js driver, with
  full support for connections to load balancers and MongoDB Atlas
  serverless instances.

Bug fixes in this release:

- The `Timestamp()` argument order is now reversed compared to
  previous `mongosh` versions.

## v1.0.3

*Released July 29, 2021*

Bug fixes in this release:

- Passing the exit code to `quit()` works like it does in the legacy
  shell.
- Instances of `MaxListenersExceededWarning` are no longer emitted
  when methods like `console.log()` are used in loops.
- When an internal error in `mongosh` occurs, the error message
  points you to the log file for the current `mongosh` session.
- When printing the name of a collection (such as in response to
  `db.coll`), the database name is included in the output.

## v1.0.1

*Released July 21, 2021*

New features in this release:

- Adds full support for the `--host` flag.
- Adds the `--build-info` flag which provides detailed information
  about the `mongosh` version.
- When using Kerberos, `mongosh` will now make use of tokens if they
  are still valid. You no longer need to specify a password when using
  valid tokens.

Bug fixes in this release:

- An issue that sporadically resulted in an `AcquireCredentialsHandle`
  error on Kerberos was fixed.
- Miscellaneous other improvements.

## v1.0

*Released July 9, 2021*

New features in this release:

- All the static methods of the Node.js driver BSON classes are now
  available. Specifically, you can use
  `ObjectId.createFromTime(unixTimestampSeconds)` instead of the
  legacy shell’s `ObjectId.fromDate(dateObj)`.
- When connected to an Atlas deployment, the default `mongosh` prompt
  displays `Atlas` instead of `Enterprise`.
- The \[cursor\](<https://www.mongodb.com/docs/manual/tutorial/iterate-a-cursor/>) referred to when
  using `it` is cleared when either `db` is reassigned or
  `db.auth()` / `db.logout()` is called.
- Minor bug fixes and improvements.

## v0.15.4

*Released July 1, 2021*

New features in this release:

- `mongosh` now color coordinates matching brackets.

## v0.15.3

*Released June 25, 2021*

New features in this release:

- `mongosh` now shows the current database name in prompt by default.

## v0.15.1

*Released June 22, 2021*

New features in this release:

- `.tar` and `.zip` `mongosh` download archives now include a
  parent directory.
- Autocomplete is now aware of the `--apiStrict` flag. When
  `--apiStrict` is `true`, autocomplete only completes methods
  that work with your defined API version. For more information, see
  \[Stable API\](<https://www.mongodb.com/docs/manual/reference/stable-api/>).
- Snippets. An experimental feature that
  allows users to create custom shell extensions.

Bug fixes in this release:

- `mongosh` can now connect to a replica set containing unhealthy nodes.

## v0.14.0

*Released May 28, 2021*

New features in this release:

- When you run `show collections`, the type of collection is shown in
  the output.
- Adds `sh.reshardCollection()` for resharding support.
- Adds `inspectCompact` option to the
  configuration API to print each
  document field on its own line.

## v0.13.1

*Released May 18, 2021*

New features in this release:

- When you use `Ctrl+C` to interrupt an operation, you interrupt
  operations that are running on the server, and not just local
  JavaScript execution.
- .editor sessions are aggregated into one
  item in shell history.
- Build and publish packages for all platforms in the current MongoDB
  5.0 server support matrix.
- Publish Windows MSI to the download center.
- Adds a customizable REPL prompt using `prompt` (or your
  .mongoshrc.js file).
- When running against a MongoDB 5.0 deployment, shows reasons for
  document validation failures.
- Adds basic support for the `--apiStrict` flag.
- New connection methods:
  - `Mongo.getDBNames()` returns a list of databases.
  - `Mongo.getDBs()` returns a document with a list of
    databases and metadata.

## v0.12.1

*Released April 30, 2021*

New features in this release:

- Adds support for the `db.hello()` shell method and
  `hello` database command. Use these commands in place of
  `isMaster`.
- Extends the shell customization API to allow controlling log
  verbosity.
- Adds autocomplete for `show` and `use` commands. For example,
  `show collections` and `use test`.

Bug fixes in this release:

- `collStats` now works properly on sharded collections.

## v0.12.0

*Released April 23, 2021*

- New async rewriter, allowing for a much wider range of JavaScript
  features in the shell.
- Connection failure response is now more prompt if a connection
  is deemed unlikely to succeed.
- Adds new API for shell customization.

## v0.11.0

*Released April 8, 2021*

Internal improvements and various bug fixes.

## v0.10.1

*Released April 1, 2021*

Internal improvements.

## v0.10.0

*Released March 31, 2021*

New features in this release:

- Support for loading a `.mongoshrc.js` file at startup. Use this
  file to bootstrap the shell with customizations and extended
  functionality.
- Ability to load scripts from the command line.
- Support for `--eval` option.
- Support for
  `--tlsCertificateSelector`
  on Windows and macOS.

Bug fixes in this release:

- Objects in \[explain output\](<https://www.mongodb.com/docs/manual/reference/explain-results/>) now
  properly expand.

## v0.9.0

*Released March 10, 2021*

New features in this release:

- Support for the load() method.
- Support for AWS IAM authentication.

Bug fixes in this release:

- Autocomplete works properly when connected to secondary node.
- `db.createUser()` on `$external` database now handles
  password properly.
- Miscellaneous other improvements.

## v0.8.2

*Released February 24, 2021*

Minor internal improvements and bug fixes.

## v0.8.1

*Released February 22, 2021*

Minor internal improvements and bug fixes.

## v0.8.0

*Released February 17, 2021*

New features in this release:

- Support for \[field-level-encryption\](field-level-encryption.md).

Bug fixes in this release:

- Running `setReadConcern` no longer reverses `db.auth()`
  authentication operations.
- Pressing the backspace key in the password prompt no longer adds an
  asterisk, and now behaves as expected.
- Running `UUID()` without a value now generates a random UUID.

## v0.7.7

*Released February 3, 2021*

New features in this release:

- `explain()` support for the following methods:
  - `count()`
  - `distinct()`
  - `findAndModify()`
  - `remove()`
  - `update()`
- Support for specifying `cursor.batchSize()`, and type `it` for
  more.
- Autocomplete for collection names.

Bug fixes in this release:

- `mongosh` no longer fails when connecting to a node in the
  `STARTUP2` state.
- `mongosh` now properly displays startup warnings.
- `explain()` on aggregations now return
  accurate and complete results.

## v0.6.1

*Released November 30, 2020*

New features in this release:

- Support for \[readPreference\](<https://www.mongodb.com/docs/manual/core/read-preference/>)
  methods.
- Support for the \[session object\](<https://www.mongodb.com/docs/manual/reference/method/Session/>)
  and related session object methods.
- Support for \[transaction\](<https://www.mongodb.com/docs/manual/core/transactions/>) methods.

Bug fixes and miscellaneous updates in this release:

- Remove support for deprecated 3.6 CRUD methods (`insert()`,
  `remove()`, `save()`, and `update()`).
- Fix an issue with loading JavaScript files into `mongosh`.
- Fix an issue where when inserting many documents via a for loop,
  the loop would abort before all documents were inserted.
- Fix issue with output when printing result of a cursor.
- Update the Node REPL to use Node version 14.

## v0.5.2

*Released November 11, 2020*

- Autocomplete now works properly when connected to a MongoDB 4.4.1
  deployment.
- The `sh.status()` method now outputs correctly in the browser
  shell.

## v0.5.0

*Released October 12, 2020*

- Adds support for replica set management methods.
- Adds support for sharded cluster management methods.

## v0.4.2

*Released October 1, 2020*

- Adds support for collection names with a dot. For example, to query
  a collection named `my.collection`, you can run:

```javascript
db.my.collection.findOne()

```

## v0.4.0

*Released September 15, 2020*

- Adds support for the following methods:
  - `db.collection.mapReduce()`
  - `db.collection.validate()`
- Adds support for `maxAwaitTimeMS` for cursors.

## v0.3.1

*Released September 14, 2020*

### Improvements

This release adds support for:

- New `cursor` methods
- Query `planCache` methods
- Error helper methods
- The following helper commands:
  - `show users`
  - `show profile`
  - `show logs`
  - `show log[<name>]`

This release includes an `.rpm` artifact which can be downloaded from
the [MongoDB Download Center](https://www.mongodb.com/download-center/community).

### Behavior Updates

Whenever a command's output includes `{ ok: 0 }`, `mongosh` throws
an exception and does not return the raw output from the server.

The legacy `mongo` shell error handling is not consistent between
commands. `mongosh` standardizes the user-facing behavior for a
more consistent experience.

### Bug Fixes

- MONGOSH-323: getUser() userId field is
  outputted as binary.
- MONGOSH-337: Linux tarball is not gzipped.
- MONGOSH-341: Wrong values with NumberLong for
  numbers \> Number.MAX_SAFE_INTEGER. As a result of this fix, values
  passed to `NumberLong` and `NumberDecimal` must be strings.

> **Important**
>
> The fix for MONGOSH-341 is a breaking
> change when compared to behavior in the legacy `mongo` shell.

- MONGOSH-346: `Ctrl+C` does not terminate the
  currently running command in the shell.

> **Note**
>
> `Ctrl+C` terminates the process in the shell, but does not
> terminate the process on the MongoDB server.

## v0.2.2

*Released August 31, 2020*

### API Additions

This release adds support for the following APIs:

- Admin commands such as `db.killOp()` and
  `db.currentOp()`. More detail in
  MONGOSH-307.
- Free monitoring commands such as `db.enableFreeMonitoring()`.
  More detail in MONGOSH-300.
- Logging and profiling helper method implementations
  (for example, `db.setLogLevel()`).
  More detail in MONGOSH-299.
- Raw command execution methods helpers (e.g.
  `db.listCommands()`). More detail in
  MONGOSH-301.
- Server stats commands such as `db.serverBuildInfo()` and
  `db.serverStatus()`. More detail in
  MONGOSH-304.
- Bulk API support. Details in MONGOSH-296.

### Bug Fixes

- Credentials are now properly redacted in logging and history.

## Past Releases

For information on past releases, refer to
[mongosh Releases on GitHub](https://github.com/mongodb-js/mongosh/releases).
