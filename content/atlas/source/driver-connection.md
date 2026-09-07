# Connect to a Cluster via Drivers

The Connect dialog box for a {+database-deployment+} provides the details to
connect to a {+database-deployment+} with an application using a
`MongoDB driver`.

------------------------------------------------------------------------

➤ Use the **Select your language** drop-down menu to set the
language of the example on this page.

------------------------------------------------------------------------

## Prerequisites

### Driver Version

Your driver version must be compatible with your version of the MongoDB
server. We recommend choosing the latest driver that is compatible with
your MongoDB server version to use the latest database features and
prepare for future version upgrades.

For a list of driver versions that contain the full set of
functionality for your version of the MongoDB server, check the
compatibility matrix for your `MongoDB driver`.

### Optimized Connection Strings for Sharded {+Clusters+} Behind a Private Endpoint

You can connect to your sharded {+cluster+} using a driver and an
optimized connection string.

### [\|tls\|](##SUBST##|tls|)

Clients must support [\|tls\|](##SUBST##|tls|) to connect to an [\|service\|](##SUBST##|service|) {+database-deployment+}.

Clients must support the SNI [\|tls\|](##SUBST##|tls|) extension to
connect to an [\|service\|](##SUBST##|service|) `M0` {+Free-cluster+} or {+Flex-cluster+}.

## Connect Your Application

## Driver Examples

In the following example, you authenticate and connect to [\|a-service\|](##SUBST##|a-service|)
{+database-deployment+} by using a URI connection string. Replace the placeholders in the example
with your credentials and deployment details.

## Troubleshooting

> **See also**
>
> connection-limits
