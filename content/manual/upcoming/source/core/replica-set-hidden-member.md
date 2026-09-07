# Hidden Replica Set Members

A hidden member maintains a copy of the primary's
data set but is **invisible** to client applications. Hidden members
are good for workloads with different usage patterns from the other
members in the replica set. Hidden members must always be
priority 0 members and
so **cannot become primary**. The `db.hello()` method does not
display hidden members. Hidden members, however, **may vote** in
elections.

## Behavior

### Read Operations

You can only read from a hidden member if you directly connect to the
node. If you connect to a cluster without directly connecting to the hidden
node, you cannot run queries on the hidden node. As a result, these
members receive no traffic other than basic replication. Use hidden
members for dedicated tasks such as reporting and
backups.

In sharded clusters, you cannot access hidden nodes through
`mongos`. Directly connecting to these nodes to read data
can result in data inconsistency or loss. Instead, to achieve workload
isolation, use
tag-based read preferences.

> **Note**
>

### Voting

Hidden members *may* vote in replica set elections. If you stop a
voting hidden member, ensure that the set has an active majority or the
primary will step down.

For the purposes of backups,

- 

### Write Concern

Hidden replica set members can acknowledge write operations issued
with `w: \<number\> \<\<number\>\>`. For write operations
issued with `w : "majority"`, however,
hidden members must also be voting members (i.e. `members\[n\].votes`
greater than `0`) to acknowledge the `"majority"` write operation.
Non-voting replica set members (i.e. `members\[n\].votes`
is `0`) cannot contribute to acknowledging write operations with
`majority` write concern.

## Further Reading

For more information about backing up MongoDB databases,
see /core/backups. To configure a hidden member, see
/tutorial/configure-a-hidden-replica-set-member.
