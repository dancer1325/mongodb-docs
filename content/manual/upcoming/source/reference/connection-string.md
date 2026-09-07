# Connection Strings

You can use connection strings to define connections between
MongoDB instances and the following destinations:

- Your applications when you connect using `drivers`.
- Tools such as MongoDB Compass and
  `MongoDB Shell (mongosh)`.

To connect to your cluster, you can use one of these connection
string formats:

- **SRV connection strings** use the `mongodb+srv://` prefix and
  provide a simplified way to connect to a cluster. SRV connection
  strings automatically include all the seed list hosts, which can
  change the servers in rotation without requiring client
  reconfiguration. When possible, use SRV connection strings instead
  of the standard connection string format.
- **Standard connection strings** use the `mongodb://` prefix and
  require you to include all cluster members in replica sets and
  sharded clusters.

Use the selectors at the top of the page to choose your deployment
type and connection string format. Complete the following steps to
find your connection string.

## Find Your {+atlas+} Connection String

To find your {+atlas+} connection string using the `Atlas CLI`, `install` and
`connect` from the Atlas CLI, then
run the following command. Replace `<clusterName>` with the name
of the {+atlas+} cluster and replace `<projectId>` with the
project ID.

```
atlas clusters connectionStrings describe <clusterName>
--projectId <projectId>

```

Your {+atlas+} connection string resembles the following
example:

```bash
mongodb+srv://myDatabaseUser:D1fficultP%40ssw0rd@cluster0.example.mongodb.net/?retryWrites=true&w=majority

```

To learn more, see `atlas clusters connectionStrings describe`.

## Atlas Cluster that Authenticates with AWS IAM Credentials

## Find Your {+atlas+} Connection String

To find your {+atlas+} connection string in the Atlas UI, follow
these steps:

Your {+atlas+} connection string resembles the following
example:

```bash
mongodb+srv://myDatabaseUser:D1fficultP%40ssw0rd@cluster0.example.mongodb.net/?retryWrites=true&w=majority

```

## Atlas Cluster that Authenticates with AWS IAM Credentials

## Find Your Self-Hosted Deployment's Connection String

```bash
mongodb+srv://myDatabaseUser:D1fficultP%40ssw0rd@mongodb0.example.com/?authSource=admin&replicaSet=myRepl

```

## Find Your Self-Hosted Deployment's Connection String

```bash
mongodb://myDatabaseUser:D1fficultP%40ssw0rd@mongodb0.example.com:27017,mongodb1.example.com:27017,mongodb2.example.com:27017/?authSource=admin&replicaSet=myRepl

```

## Find Your Self-Hosted Deployment's Connection String

```bash
mongodb+srv://myDatabaseUser:D1fficultP%40ssw0rd@mongos0.example.com/?authSource=admin

```

## Find Your Self-Hosted Deployment's Connection String

```bash
mongodb://myDatabaseUser:D1fficultP%40ssw0rd@mongos0.example.com:27017,mongos1.example.com:27017,mongos2.example.com:27017/?authSource=admin

```

## Find Your Self-Hosted Deployment's Connection String

```bash
mongodb+srv://myDatabaseUser:D1fficultP%40ssw0rd@mongodb0.example.com/?authSource=admin

```

## Find Your Self-Hosted Deployment's Connection String

```bash
mongodb://myDatabaseUser:D1fficultP%40ssw0rd@mongodb0.example.com:27017/?authSource=admin

```

## Learn More

For a full list of connection string options, see
connections-connection-options.
