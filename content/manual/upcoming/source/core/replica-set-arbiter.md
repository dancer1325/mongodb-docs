# Replica Set Arbiter

In some circumstances (such as when you have a primary and a secondary,
but cost constraints prohibit adding another secondary), you may choose
to add an arbiter to your replica set. An arbiter participates in
elections for primary but an arbiter does
**not** have a copy of the data set and **cannot** become a primary.

An arbiter has exactly `1` election vote. By default an arbiter has
priority `0`.

> **Important**
>
> Do not run an arbiter on systems that also host the primary or the
> secondary members of the replica set.

> **Warning**
>
> Using a primary-secondary-arbiter (PSA)
> architecture for shards in a sharded cluster can cause a loss of
> availability if a data-bearing secondary is unavailable. A PSA
> cluster differs from a typical replica set: In a sharded cluster,
> shards perform `w: majority` write concern operations that cannot complete if the remaining
> cluster members required to confirm an operation have an arbiter.

To add an arbiter, see /tutorial/add-replica-set-arbiter.

## Performance Issues with PSA replica sets

## Shard Keys, Transactions, and Arbiters

## Concerns with Multiple Arbiters

Use a single arbiter to avoid problems with data consistency. Multiple
arbiters prevent the reliable use of the majority write concern.

To ensure that a write will persist after the failure of a primary node,
the majority write concern requires a majority of nodes to acknowledge
a write operation. Arbiters do not store any data, but they do
contribute to the number of nodes in a replica set. When a replica set
has multiple arbiters it is less likely that a majority of data bearing
nodes will be available after a node failure.

> **Warning**
>
> If a secondary node falls behind the primary, and the cluster is
> `reconfigured`, votes from multiple arbiters
> can elect the node that had fallen behind. The new primary will not
> have the unreplicated writes even though the writes could have been
> majority committed by the old configuration. The result is data
> loss.
>
> To avoid this scenario, use at most a single arbiter.

> **New in version 5.3**
>

Starting in MongoDB 5.3, support for multiple arbiters in a replica set is
disabled by default. If you attempt to add multiple arbiters to a
replica set, the server returns an error:

```text
MongoServerError: Multiple arbiters are not allowed unless all nodes
were started with --setParameter 'allowMultipleArbiters=true'

```

To add multiple arbiters to a replica set using MongoDB 5.3 or later, start
each node with the `allowMultipleArbiters` parameter set to `true`:

## Security

### Authentication

When running with `authorization`, arbiters exchange credentials with
other members of the set to authenticate. MongoDB encrypts the
authentication process, and the MongoDB authentication exchange is
cryptographically secure.

Because arbiters do not store data, they do not possess the internal table of user and role mappings
used for authentication. Thus, the only way to log on to an arbiter with authorization active is to
use the localhost exception.

### Communication

The only communication between arbiters and other set members are:
votes during elections, heartbeats, and configuration data. These
exchanges are not encrypted.

**However**, if your MongoDB deployment uses TLS/SSL, MongoDB will encrypt
*all* communication between replica set members. See
/tutorial/configure-ssl for more information.

As with all MongoDB components, run arbiters in trusted network
environments.

## Example

For example, in the following replica set with 2 data-bearing members
(the primary and a secondary), an arbiter allows the set to have an odd
number of votes to break a tie:
