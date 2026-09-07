# Back Up and Restore a Self-Managed Deployment with MongoDB Tools

This tutorial describes the process for creating backups and restoring data
using the command-line utilities `mongorestore` and `mongodump`
provided with MongoDB.

To restore a backup of your self-hosted deployment
to a managed [{+atlas+} deployment](https://www.mongodb.com/docs/atlas),
see Seed with mongorestore.

For a fully-managed backup method, use Cloud Backups
in MongoDB Atlas, which provide localized backup storage using the native snapshot
functionality of the cluster's cloud service provider.

## Considerations

### Deployments

The `mongorestore` and `mongodump` utilities
work with BSON data dumps, and are
useful for creating backups of small deployments. For resilient and
non-disruptive backups, use file system snapshots
or block-level disk snapshots with
Cloud Backups from {+atlas+}.

> **Note Back Up Sharded Clusters with {+atlas+}**
>

### Performance Impacts

### Output Format

`mongorestore` and `mongodump` can output data
to an archive file, which is a single-file alternative to multiple BSON
files. Archive files are special-purpose formats that support
non-contiguous file writes. They enable concurrent backups from MongoDB,
as well as restores to MongoDB. Using archive files optimizes disk I/O
while backup and restore operations execute.

You can also output archive files to the standard output (`stdout`).
Writing to the standard output allows for data migration over networks,
reduced disk I/O footprint, and concurrency gains in both the MongoDB
tools and your storage engine.

For more information on archive files, see the
`--archive` option.

### Stale Backups

## Procedures

### Back Up a Database with `mongodump`

> **Note Back Up Sharded Clusters with {+atlas+}**
>

#### Exclude `local` Database

`mongodump` excludes the content of the `local` database in its output.

#### Required Access

#### Basic `mongodump` Operations

The `mongodump` utility backs up data by connecting to a
running `mongod`.

The utility can create a backup for an entire server, database or collection,
or can use a query to backup just part of a collection.

When you run `mongodump` without any arguments, the command
connects to the MongoDB instance on the local system
(e.g. `localhost`) on port `27017` and creates a
database backup named `dump/` in the current directory.

To backup data from a `mongod` instance
running on the same machine and on the default port of `27017`,
use the following command:

```bash
mongodump

```

To specify the host and port of the MongoDB instance, you can either:

- Specify the hostname and port in the `--uri` string, using either an
  SRV or
  standard connection string:

```bash
mongodump --uri="mongodb+srv://username:password@cluster0.example.mongodb.net" <additional_options>

```

- Specify the hostname and port in the `--host` string:

```bash
mongodump --host="mongodb0.example.com:27017" <additional_options>

```

- Specify the hostname and port in the `--host` and `--port`:

```bash
mongodump --host="mongodb0.example.com" --port=27017 <additional_options>

```

`mongodump` will write BSON files that hold a copy of
data accessible via the `mongod` listening on port `27017` of
the `mongodb.example.net` host. See backup-from-non-local for more
information.

To specify a different output directory, you can use the `--out or -o` option:

```bash
mongodump --out=/opt/backup/mongodump-1

```

To limit the amount of data included in the database dump, you can
specify `--db` and
`--collection` as options to
`mongodump`. For example:

```none
mongodump --collection=myCollection --db=test

```

This operation creates a dump of the collection named `myCollection`
from the database `test` in a `dump/` subdirectory of the
current working directory.

`mongodump` overwrites output files if they exist in the
backup data folder. Before running the `mongodump` command
multiple times, either ensure that you no longer need the files in the
output folder (the default is the `dump/` folder) or rename the
folders or files.

#### Create Backups Using Oplogs

The `--oplog` option with
`mongodump` collects the oplog entries and allows
you to perform a backup on a live database. If you later restore the
database from the backup, the database will be the same as it was when
the backup process completed.

With `--oplog`, `mongodump`
copies all the data from the source database as well as all of the
oplog entries from the beginning to the end of the backup
procedure. This operation, in conjunction with `mongorestore --oplogReplay`, allows you to restore a
backup that reflects the specific moment in time that corresponds to
when `mongodump` completed creating the dump file.

#### Create Backups from Non-Local `mongod` Instances

The `--host` and
`--port` options for
`mongodump` allow you to connect to and backup from a remote host.
Consider the following example:

```bash
mongodump \
   --host=mongodb1.example.net \
   --port=3017 \
   --username=user \
   --password="pass" \
   --out=/opt/backup/mongodump-1

```

On any `mongodump` command you may, as above, specify username
and password credentials to specify database authentication.

### Restore a Database with `mongorestore`

> **Note Back Up Sharded Clusters with {+atlas+}**
>

#### Access Control

To restore data to a MongoDB deployment that has access control enabled, the `restore` role provides
the necessary privileges to restore data from backups *if* the data does
not include `system.profile \<\<database\>.system.profile\>`
collection data and you run `mongorestore` without the
`--oplogReplay` option.

#### Basic `mongorestore` Operations

The `mongorestore` utility restores a binary backup created by
`mongodump`. By default, `mongorestore` looks for a
database backup in the `dump/` directory.

The `mongorestore` utility restores data by connecting to a
running `mongod` directly.

`mongorestore` can restore either an entire database backup
or a subset of the backup.

To use `mongorestore` to connect to an active
`mongod`, use a command with the following prototype form:

```bash
mongorestore --uri <connection string> <path to the backup>


```

Consider the following example:

```bash
mongorestore /opt/backup/mongodump-1

```

Here, `mongorestore` imports the database backup in
the `/opt/backup/mongodump-1` directory to the `mongod` instance
running on the localhost interface on the default port `27017`.

#### Use an Oplog File to Backup and Restore Data

To capture writes that may occur while `mongodump` is running, use
`mongodump --oplog`. `mongodump` creates an `oplog.bson`
file with oplog entries for each write that occurred during the
run. You can apply the oplog operations with `mongorestore --oplogReplay`.

For examples, see mongodump-examples and
mongorestore-examples.

All of the data from the `oplog.bson` file is restored.

`mongorestore --oplogReplay` doesn't allow you to restore data to an
arbitrary point in time. Use `mongorestore --oplogReplay` to
ensure the restored data is up to date with any writes that occurred
during the `mongodump --oplog` run.

> **Note**
>
> `--oplog` is intended for use with replica sets. For sharded
> clusters, including replica sets that are part of a sharded
> environment, see backup-sharded-dumps.

You may also consider using the `mongorestore --objcheck`
option to check the integrity of objects while inserting them into the
database, or you may consider the `mongorestore --drop` option to drop each
collection from the database before restoring from
backups.

#### Restore Backups to Non-Local `mongod` Instances

By default, `mongorestore` connects to a MongoDB instance
running on the localhost interface and on the
default port (`27017`). If you want to restore to a different host or
port, use the `--host` and `--port` options.

The following example that specifies the `--host` and `--port`
options:

```bash
mongorestore --host=mongodb1.example.net --port=3017

```

If restoring to an instance that enforces access control, include the
`--username` and the
`--authenticationDatabase` as well. Omit the
`--password` option to have
`mongorestore` prompt for the password:

```bash
mongorestore \
   --host=mongodb1.example.net \
   --port=3017 \
   --username=user \
   --authenticationDatabase=admin \
   /opt/backup/mongodump-1
```
