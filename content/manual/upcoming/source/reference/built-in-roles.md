# Built-In Roles

MongoDB grants access to data and commands through role-based authorization. MongoDB provides built-in roles that grant
the different levels of access commonly needed in a database system.

A role grants privileges to perform sets of actions on defined resources. A given role applies to the database on which
it is defined and can grant access down to a collection level of
granularity.

Each of MongoDB's built-in roles defines access at the database level
for all non-system collections in the role's database and at the
collection level for all system collections.

*System collections* include those in:

- `<database>.system.*` namespace
- `local.replset.*` replica set namespace

*Non-system collections* are those not in namespaces in the previous
list.

Although {+atlas+} database users have different built-in roles than
self-hosted deployment users, MongoDB builds the built-in roles for
each type of deployment from the same set of privilege actions.

Use the selector at the top of the page to choose your deployment
type and see the available built-in roles.

## {+atlas+} Built-In Roles

This section describes the [\|service\|](##SUBST##|service|) built-in roles and the
MongoDB Roles they
represent.

You can create database users and assign built-in roles in the
{+atlas+} user interface. To learn more, see Add Database Users.

To learn more about common commands that [\|service\|](##SUBST##|service|) doesn't support
with the current [\|service\|](##SUBST##|service|) user privileges, see
paid-tier-command-limitations.

### Protected MongoDB Database Namespaces

Avoid writing to the `admin`, `local`, and `config`
databases, or any database prefixed with `__mdb_internal_`.
[\|service\|](##SUBST##|service|) manages multiple collections in these databases. To
learn more, see cloud-system-databases and
metadata-system-collections.

`atlasAdmin` has the `update` privilege on
the `config.settings` collection to manage the balancer.

### Built-in Roles and Inherited Privileges

In the following table:

- cog indicates that the operation also supports
  timeseries collections.

- star indicates that the operation is supported on
  `config.settings`.

- - [\|service\|](##SUBST##|service|) Built-in Role
  - MongoDB Role
  - Inherited Roles or Privilege Actions

- - <div class="atlasrole">

    Atlas admin

    </div>

  - <div class="atlasrole">

    atlasAdmin

    </div>

  - This role allows you to use `setQuerySettings`,
    `removeQuerySettings`, and
    `\$querySettings`.

    `analyzeShardKey` cog  
    `autoCompact`  
    `backup`  
    `checkMetadataConsistency`  
    `cleanupOrphaned`  
    `clearJumboFlag` cog  
    `clusterMonitor`  
    `compact`  
    `dbAdminAnyDatabase`  
    `enableSharding` cog  
    `flushRouterConfig` cog  
    `killAnyCursor`  
    `moveChunk` cog  
    `querySettings`  
    `readWriteAnyDatabase`  
    `refineCollectionShardKey` cog  
    `reshardCollection` cog  
    `shardedDataDistribution` cog  
    `splitChunk` cog  
    `viewRole`

- - <div class="atlasrole">

    backup

    </div>

  - `backup`

  - `backup`

- - <div class="atlasrole">

    clusterMonitor

    </div>

  - `clusterMonitor`

  - `clusterMonitor`

- - <div class="atlasrole">

    dbAdmin

    </div>

  - `dbAdmin`

  - `dbAdmin`

- - <div class="atlasrole">

    dbAdminAnyDatabase

    </div>

  - `dbAdminAnyDatabase`

  - `dbAdminAnyDatabase`

- - <div class="atlasrole">

    enableSharding

    </div>

  - `enableSharding`

  - `enableSharding`

- - <div class="atlasrole">

    read

    </div>

  - `read`

  - `read`

- - <div class="atlasrole">

    readWrite

    </div>

  - `readWrite`

  - `readWrite`

- - <div class="atlasrole">

    readWriteAnyDatabase

    </div>

  - `readWriteAnyDatabase`

  - `readWriteAnyDatabase`

- - <div class="atlasrole">

    readAnyDatabase

    </div>

  - `readAnyDatabase`

  - `readAnyDatabase`

- - <div class="atlasrole">

    killOpSession

    </div>

  - 

  - `inprog`  
    `killop`  
    `killAnySession`  
    `killCursors`  
    `listSessions`

- - <div class="atlasrole">

    autoCompact

    </div>

  - 

  - `autoCompact`

- - <div class="atlasrole">

    manageShardBalancer

    </div>

  - 

  - `find` star  
    `insert` star  
    `update` star

## Learn More

If built-in roles don't meet your requirements, you can create
user-defined roles. For more information, see
mongodb-roles.
Self-Managed Deployment Built-In Roles
--------------------------------------

MongoDB provides the following built-in roles in self-managed
deployments.

To view the privileges for a built-in role, run the
`rolesInfo` command with the `showPrivileges` and
`showBuiltinRoles` fields both set to `true`. For example, the
following command shows the privileges for the `readWrite` role:

db.getRole("readWrite", { showPrivileges: true, showBuiltinRoles: true } )
{
db: 'test',
role: 'readWrite',
roles: \[\],
privileges: \[
{
resource: { db: 'test', collection: '' },
actions: \[
'changeStream',
'cleanupStructuredEncryptionData',
'collStats',
'compactStructuredEncryptionData',
'convertToCapped',
'createCollection',
'createIndex',
'createSearchIndexes',
'dbHash',
'dbStats',
'dropCollection',
'dropIndex',
'dropSearchIndex',
'find',
'insert',
'killCursors',
'listCollections',
'listIndexes',
'listSearchIndexes',
'updateSearchIndex',
'planCacheRead',
'performRawDataOperations',
'remove',
'renameCollectionSameDB',
'update'
\]
},
{
resource: { db: 'test', collection: 'system.js' },
actions: \[
'changeStream',
'cleanupStructuredEncryptionData',
'collStats',
'compactStructuredEncryptionData',
'convertToCapped',
'createCollection',
'createIndex',
'createSearchIndexes',
'dbHash',
'dbStats',
'dropCollection',
'dropIndex',
'dropSearchIndex',
'find',
'insert',
'killCursors',
'listCollections',
'listIndexes',
'listSearchIndexes',
'updateSearchIndex',
'planCacheRead',
'performRawDataOperations',
'remove',
'renameCollectionSameDB',
'update'
\]
}
\],
inheritedRoles: \[\],
inheritedPrivileges: \[
{
resource: { db: 'test', collection: '' },
actions: \[
'changeStream',
'cleanupStructuredEncryptionData',
'collStats',
'compactStructuredEncryptionData',
'convertToCapped',
'createCollection',
'createIndex',
'createSearchIndexes',
'dbHash',
'dbStats',
'dropCollection',
'dropIndex',
'dropSearchIndex',
'find',
'insert',
'killCursors',
'listCollections',
'listIndexes',
'listSearchIndexes',
'updateSearchIndex',
'planCacheRead',
'performRawDataOperations',
'remove',
'renameCollectionSameDB',
'update'
\]
},
{
resource: { db: 'test', collection: 'system.js' },
actions: \[
'changeStream',
'cleanupStructuredEncryptionData',
'collStats',
'compactStructuredEncryptionData',
'convertToCapped',
'createCollection',
'createIndex',
'createSearchIndexes',
'dbHash',
'dbStats',
'dropCollection',
'dropIndex',
'dropSearchIndex',
'find',
'insert',
'killCursors',
'listCollections',
'listIndexes',
'listSearchIndexes',
'updateSearchIndex',
'planCacheRead',
'performRawDataOperations',
'remove',
'renameCollectionSameDB',
'update'
\]
}
\],
isBuiltin: true
}

### Database User Roles

Every database includes the following client roles:

read
The role provides read access by granting the following actions:

- `changeStream`
- `collStats`
- `dbHash`
- `dbStats`
- `find`
- `killCursors`
- `listCollections`
- `listIndexes`
- `listSearchIndexes`

readWrite

The role provides the following actions on those collections:

- `changeStream`
- `collStats`
- `convertToCapped`
- `createCollection`
- `createIndex`
- `createSearchIndexes`
- `dbHash`
- `dbStats`
- `dropCollection`
- `dropIndex`
- `dropSearchIndex`
- `find`
- `insert`
- `killCursors`
- `listCollections`
- `listIndexes`
- `listSearchIndexes`
- `remove`
- `renameCollectionSameDB`
- `update`
- `updateSearchIndex`

### Database Administration Roles

Every database includes the following database administration roles:

dbAdmin
Specifically, the role provides the following privileges:

- - Resource
  - Permitted Actions

- - `system.profile \<\<database\>.system.profile\>`

  - <div class="hlist" columns="2">

    - `changeStream`
    - `collStats`
    - `convertToCapped`
    - `createCollection`
    - `dbHash`
    - `dbStats`
    - `dropCollection`
    - `find`
    - `killCursors`
    - `listCollections`
    - `listIndexes`
    - `listSearchIndexes`
    - `planCacheRead`

    </div>

\* - All *non*-system collections (i.e. database resource)

> - <div class="hlist" columns="2">
>
>   - `bypassDocumentValidation`
>   - `collMod`
>   - `collStats`
>   - `compact`
>   - `convertToCapped`
>   - `createCollection`
>   - `createIndex`
>   - `createSearchIndexes`
>   - `dbStats`
>   - `dropCollection`
>   - `dropDatabase`
>   - `dropIndex`
>   - `dropSearchIndex`
>   - `enableProfiler`
>   - `listCollections`
>   - `listIndexes`
>   - `listSearchIndexes`
>   - `planCacheIndexFilter`
>   - `planCacheRead`
>   - `planCacheWrite`
>   - `reIndex`
>   - `renameCollectionSameDB`
>   - `updateSearchIndex`
>   - `validate`
>
>   </div>
>
>   For these collections, `dbAdmin` *does not* include
>   full read access (i.e. `find`).

dbOwner

userAdmin  
The `userAdmin` role explicitly provides the following actions:

- `changeCustomData`
- `changePassword`
- `createRole`
- `createUser`
- `dropRole`
- `dropUser`
- `grantRole`
- `revokeRole`
- `setAuthenticationRestriction`
- `viewRole`
- `viewUser`

> **Warning**
>
> It is important to understand the security implications of granting the
> `userAdmin` role: a user with this role for a database can
> assign themselves any privilege on that database. Granting the
> `userAdmin` role on the `admin` database has further
> security implications as this indirectly provides
> superuser access to a cluster. With `admin`
> scope a user with the `userAdmin` role can grant cluster-wide
> roles or privileges including `userAdminAnyDatabase`.

### Cluster Administration Roles

clusterAdmin

clusterManager
\* - Resource
- Actions

- - cluster

  - <div class="hlist" columns="2">

    - `addShard`
    - `appendOplogNote`
    - `applicationMessage`
    - `checkMetadataConsistency` (New in version 7.0)
    - `cleanupOrphaned`
    - `flushRouterConfig`
    - `getClusterParameter`
    - `getDefaultRWConcern`
    - `listSessions`
    - `listShards`
    - `moveCollection` (MongoDB 8.0 and later)
    - `removeShard`
    - `replSetConfigure`
    - `replSetGetConfig`
    - `replSetGetStatus`
    - `replSetStateChange`
    - `resync`
    - `setClusterParameter`
    - `setDefaultRWConcern`
    - `setFeatureCompatibilityVersion`
    - `transitionFromDedicatedConfigServer`
    - `transitionToDedicatedConfigServer`
    - `unshardCollection` (MongoDB 8.0 and later)

    </div>

\* - *All* databases

> - <div class="hlist" columns="1">
>
>   - `analyzeShardKey` (New in version 7.0)
>   - `clearJumboFlag`
>   - `configureQueryAnalyzer`
>   - `enableSharding`
>   - `moveChunk`
>   - `refineCollectionShardKey`
>   - `reshardCollection`
>
>   </div>
>
>   `clusterManager` provides additional privileges for the
>   `config` and `local` databases.

On the `config` database, permits the following actions:

- - Resource
  - Actions

- - All non-system collections in the `config` database

  - <div class="hlist" columns="2">

    - `collStats`
    - `dbHash`
    - `dbStats`
    - `enableSharding`
    - `find`
    - `insert`
    - `killCursors`
    - `listCollections`
    - `listIndexes`
    - `listSearchIndexes`
    - `moveChunk`
    - `planCacheRead`
    - `remove`
    - `update`

    </div>

\* - `system.js \<\<database\>.system.js\>`

> - <div class="hlist" columns="2">
>
>   - `collStats`
>   - `dbHash`
>   - `dbStats`
>   - `find`
>   - `killCursors`
>   - `listCollections`
>   - `listIndexes`
>   - `listSearchIndexes`
>   - `planCacheRead`
>
>   </div>

On the `local` database, permits the following actions:

- - Resource
  - Actions

- - All non-system collections in the `local` database

  - <div class="hlist" columns="2">

    - `enableSharding`
    - `insert`
    - `moveChunk`
    - `remove`
    - `update`

    </div>

\* - `system.replset` collection  
- <div class="hlist" columns="2">

  - `collStats`
  - `dbHash`
  - `dbStats`
  - `find`
  - `killCursors`
  - `listCollections`
  - `listIndexes`
  - `listSearchIndexes`
  - `planCacheRead`

  </div>

clusterMonitor
Permits the following actions on the cluster as a whole:

- `connPoolStats`
- `getCmdLineOpts`
- `getDefaultRWConcern`
- `getLog`
- `getParameter`
- `getShardMap`
- `hostInfo`
- `inprog`
- `listClusterCatalog`
- `listDatabases`
- `listSessions`
- `listShards`
- `replSetGetConfig`
- `replSetGetStatus`
- `serverStatus`
- `shardingState`

\- `top`
Permits the following actions on *all* databases in the cluster:

- `collStats`
- `dbStats`
- `indexStats`
- `useUUID`

Permits the `find` action on all `system.profile \<\<database\>.system.profile\>` collections in the cluster.

On the `config` database, permits the following actions:

- - Resource
  - Actions

- - All non-system collections in the `config` database

  - `collStats`  
    `dbHash`  
    `dbStats`  
    `find`  
    `indexStats`  
    `killCursors`  
    `listCollections`  
    `listIndexes`  
    `listSearchIndexes`  
    `planCacheRead`

\* - `system.js \<\<database\>.system.js\>` collection

> - `collStats`  
>   `dbHash`  
>   `dbStats`  
>   `find`  
>   `killCursors`  
>   `listCollections`  
>   `listIndexes`  
>   `planCacheRead`

On the `local` database, permits the following actions:

- - Resource
  - Actions

- - All non-system collections in the `local` database

  - `collStats`  
    `dbHash`  
    `dbStats`  
    `find`  
    `indexStats`  
    `killCursors`  
    `listCollections`  
    `listIndexes`  
    `listSearchIndexes`  
    `planCacheRead`

- - `system.js \<\<database\>.system.js\>` collection

  - `collStats`  
    `dbHash`  
    `dbStats`  
    `find`  
    `killCursors`  
    `listCollections`  
    `listIndexes`  
    `listSearchIndexes`  
    `planCacheRead`

\* - \| `system.replset`,  
`system.profile \<\<database\>.system.profile\>`,

- `find`

directShardOperations

enableSharding
Provides the ability to enable sharding for a collection and modify
existing shard keys.

Provides the following actions on all non-system collections:

- `analyzeShardKey`
- `enableSharding`
- `moveCollection` (MongoDB 8.0 and later)
- `refineCollectionShardKey`
- `reshardCollection`

\- `unshardCollection` (MongoDB 8.0 and later)
hostManager
On the cluster as a whole, provides the following actions:

- `applicationMessage`
- `closeAllDatabases`
- `compact` (New in version 7.3)
- `connPoolSync`
- `flushRouterConfig`
- `fsync`
- `invalidateUserCache`
- `killAnyCursor`
- `killAnySession`
- `killop`
- `logRotate`
- `oidReset`
- `resync`
- `rotateCertificates` (New in version 5.0)
- `setParameter`
- `shutdown`
- `touch`

\- `unlock`
On *all* databases in the cluster, provides the following actions:

\- `killCursors`
searchCoordinator
Provides `readAnyDatabase` privileges and write permissions on
the `__mdb_internal_search` database.

On the cluster as a whole, provides the following action:

- `bypassDefaultMaxTimeMS`

### Backup and Restoration Roles

backup
.. todo: should we document the mms.backup collection in the
system-collections document?

Provides the `insert` and `update` actions
on the `settings` collection in the
`config` database.

On anyResource, provides the

- `listDatabases` action
- `listCollections` action
- `listIndexes` action
- `listSearchIndexes` action

On the cluster as a whole, provides the

- `appendOplogNote`
- `getParameter`
- `listDatabases`
- `serverStatus`
- `setUserWriteBlockMode` (Starting in MongoDB 6.0)

Provides the `find` action on the following:

- all *non*-system collections in the cluster, including those in
  the `config` and `local` databases

- The following system collections in the cluster:

  `system.js \<\<database\>.system.js\>`, and
  `system.profile \<\<database\>.system.profile\>`

- The `admin.system.users` and `admin.system.roles` collections

- The `config.settings` collection

- Legacy `system.users` collections from versions of MongoDB prior to 2.6

Provides the `insert` and `update` actions
on the `config.settings` collection.

restore
Provides the following action on the cluster as a whole:

- `getParameter`

Provides the following actions on all *non*-system collections:

- `bypassDocumentValidation`
- `changeCustomData`
- `changePassword`
- `collMod`
- `convertToCapped`
- `createCollection`
- `createIndex`
- `createRole`
- `createSearchIndexes`
- `createUser`
- `dropCollection`
- `dropRole`
- `dropUser`
- `grantRole`
- `insert`
- `revokeRole`
- `updateSearchIndex`
- `viewRole`
- `viewUser`

Provides the following actions on `system.js \<\<database\>.system.js\>` collection:

- `bypassDocumentValidation`
- `collMod`
- `createCollection`
- `createIndex`
- `dropCollection`
- `insert`
- `updateSearchIndex`

Provides the following action on anyResource:

- `listCollections`

Provides the following actions on all non-system collections on the
`config` and the `local` databases:

- `bypassDocumentValidation`
- `collMod`
- `createCollection`
- `createIndex`
- `dropCollection`
- `insert`
- `updateSearchIndex`

Provides the following actions on `admin.system.version`

- `bypassDocumentValidation`
- `collMod`
- `createCollection`
- `createIndex`
- `dropCollection`
- `find`
- `insert`
- `updateSearchIndex`

Provides the following action on `admin.system.roles`

- `createIndex`

Provides the following actions on `admin.system.users`
and legacy `system.users` collections:

- `bypassDocumentValidation`
- `collMod`
- `createCollection`
- `createIndex`
- `dropCollection`
- `find`
- `insert`
- `remove`
- `update`
- `updateSearchIndex`

Although `restore` includes the ability to modify the
documents in the `admin.system.users` collection using
normal modification operations, *only* modify these data using
the user management methods.

Provides the following action on the `\<database\>.system.views`
collection:

- `dropCollection` (Starting in MongoDB 6.0)

On the cluster as a whole, provides the
following actions:

- `bypassWriteBlockingMode` (Staring in MongoDB 6.0)
- `setUserWriteBlockMode` (Starting in MongoDB 6.0)

### All-Database Roles

readAnyDatabase

readWriteAnyDatabase

userAdminAnyDatabase
`userAdminAnyDatabase` also provides the
following privilege actions on the cluster:

- `authSchemaUpgrade`
- `invalidateUserCache`
- `listDatabases`

The role provides the following privilege actions on the
`system.users` and
`system.roles` collections on the
`admin` database, and on legacy `system.users` collections from
versions of MongoDB prior to 2.6:

- `collStats`
- `createIndex`
- `createSearchIndexes`
- `dbHash`
- `dbStats`
- `dropIndex`
- `dropSearchIndex`
- `find`
- `killCursors`
- `planCacheRead`

The `userAdminAnyDatabase` role does not restrict the privileges
that a user can grant. As a result, `userAdminAnyDatabase` users
can grant themselves privileges in excess of their current
privileges and even can grant themselves *all privileges*, even though the
role does not explicitly authorize privileges beyond user administration.
This role is effectively a MongoDB system superuser.

dbAdminAnyDatabase
Starting in MongoDB 5.0, `dbAdminAnyDatabase` includes the
applyOps privilege action.

### Superuser Roles

Several roles provide either indirect or direct system-wide superuser access.

The following roles provide the ability to assign any user any privilege on
any database, which means that users with one of these roles can assign
*themselves* any privilege on any database:

- `dbOwner` role, when scoped to the `admin` database
- `userAdmin` role, when scoped to the `admin` database
- `userAdminAnyDatabase` role

The following role provides full privileges on all resources:

root

> **Changed in version 6.0**
>
> The `root` role includes `find` and
> `remove` privileges on the `system.preimages`
> collection in the `config` database.

### Internal Role

\_\_system
MongoDB assigns this role to user objects that represent cluster members,
such as replica set members and `mongos` instances. The role
entitles its holder to take any action against any object in the database.

**Do not** assign this role to user objects representing applications or
human administrators, other than in exceptional circumstances.

If you need access to all actions on all resources, for example to
run `applyOps` commands, do not assign this role.
Instead, create a user-defined role that
grants `anyAction` on resource-anyresource and
ensure that only the users who need access to these operations have
this access.
Learn More
----------

If built-in roles don't meet your requirements, you can create
user-defined roles. For more information, see
user-defined-roles.
