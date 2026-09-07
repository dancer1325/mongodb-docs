# Replica Set Elections

Replica sets use elections to determine which
set member will become primary. Replica sets can trigger an
election in response to a variety of events, such as:

- Adding a new node to the replica set,
- `initiating a replica set`,
- performing replica set maintenance using methods such as `rs.stepDown()` or `rs.reconfig()`, and
- the secondary members losing connectivity to the primary for more than the configured `timeout` (10 seconds by default).

In the following diagram, the primary node was unavailable for longer
than the `configured timeout`
and triggers the automatic failover
process. One of the remaining secondaries calls for an election to
select a new primary and automatically resume normal operations.

The replica set cannot process write operations until the
election completes successfully. The replica set can continue to serve
read queries if such queries are configured to
run on secondaries.

## Factors and Conditions that Affect Elections

### Replication Election Protocol

Replication `protocolVersion: 1` reduces
replica set failover time and accelerate the detection of multiple
simultaneous primaries.

You can use `catchUpTimeoutMillis` to prioritize between
faster failovers and preservation of `w:1 \<\<number\>\>` writes.

For more information on `pv1`, see
/reference/replica-set-protocol-versions.

### Heartbeats

Replica set members send heartbeats (pings) to each other every two
seconds. If a heartbeat does not return within 10 seconds, the other
members mark the delinquent member as inaccessible.

### Member Priority

After a replica set has a stable primary, the election algorithm will
make a "best-effort" attempt to have the secondary with the highest
`members\[n\].priority` available call an election.
Member priority affects both the timing and the
outcome of elections; secondaries with higher priority call elections
relatively sooner than secondaries with lower
priority, and are also more likely to win. However, a lower priority
instance can be elected as primary for brief periods, even if a higher
priority secondary is available. Replica set members continue
to call elections until the highest priority member available becomes
primary.

Members with a priority value of `0` cannot become primary and do
not seek election. For details, see
/core/replica-set-priority-0-member.

### Mirrored Reads

MongoDB provides mirrored reads to pre-warm
electable secondary members' cache with the most recently accessed data.
With mirrored reads, the primary can mirror a subset of operations that it receives and send them
to a subset of electable secondaries. Pre-warming the cache of a
secondary can help restore performance more quickly after an election.

For details, see mirrored-reads.

### Loss of a Data Center

With a distributed replica set, the loss of a data center may affect
the ability of the remaining members in other data center or data
centers to elect a primary.

If possible, distribute the replica set members across data centers to
maximize the likelihood that even with a loss of a data center, one of
the remaining replica set members can become the new primary.

> **See also**
>
> /core/replica-set-architecture-geographically-distributed

### Network Partition

A network partition may segregate a primary into a partition
with a minority of nodes. When the primary detects that it can only see
a minority of voting nodes in the replica set, the primary steps down
and becomes a secondary. Independently, a member in the partition that
can communicate with a `majority` of the voting nodes (including
itself) holds an election to become the new primary.

## Voting Members

The replica set member configuration setting `members\[n\].votes`
and member `members\[n\].state` determine whether a
member votes in an election.

- All replica set members that have their `members\[n\].votes`
  setting equal to 1 vote in elections. To exclude a member from voting
  in an election, change the value of the member's
  `members\[n\].votes` configuration to `0`.
  - 
  - 
- Only voting members in the following states are eligible to vote:
  - `PRIMARY`
  - `SECONDARY`
  - `STARTUP2` (unless the member was newly added to the
    replica set)
  - `RECOVERING`
  - `ARBITER`
  - `ROLLBACK`

> **See also**
>
> - `replSetGetStatus.votingMembersCount`
>
> \- `replSetGetStatus.writableVotingMembersCount`

## Non-Voting Members

Although non-voting members do not vote in elections, these members
hold copies of the replica set's data and can accept read operations
from client applications.

Because a replica set can have up to `50 members`, but only `7 voting members`, non-voting
members allow a replica set to have more than seven members.

For instance, the following nine-member replica set has seven voting
members and two non-voting members.

A non-voting member has both `members\[n\].votes` and
`members\[n\].priority` equal to `0`:

```javascript
{
   "_id" : <num>,
   "host" : <hostname:port>,
   "arbiterOnly" : false,
   "buildIndexes" : true,
   "hidden" : false,
   "priority" : 0,
   "tags" : {
   
   },
   "secondaryDelaySecs" : Long(0),
   "votes" : 0
}

```

> **Important Do **not** alter the number of votes to control which**
>
> members will become primary. Instead, modify the
> `members\[n\].priority` option. *Only*
> alter the number of votes in exceptional cases. For example, to
> permit more than seven members.

To configure a non-voting member, see
/tutorial/configure-a-non-voting-replica-set-member.
