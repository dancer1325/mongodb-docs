# Delayed Replica Set Members

Delayed members contain copies of a replica set's data set. However, a delayed member's data set reflects an
earlier, or delayed, state of the set. For example, if the current
time is 09:52 and a member has a delay of an hour, the delayed member
has no operation more recent than 08:52.

Because delayed members are a "rolling backup" or a running
"historical" snapshot of the data set, they may help you recover from
various kinds of human error. For example, a delayed member can make
it possible to recover from unsuccessful application upgrades and
operator errors including dropped databases and collections.

## Considerations

### Requirements

Delayed members:

- **Must be** priority 0
  members. Set the priority to 0 to prevent a delayed member from
  becoming primary.
- **Must be** hidden
  members. Always prevent applications from seeing and querying
  delayed members.
- *Do* vote in elections for primary, if
  `members\[n\].votes` is set to 1. Ensuring that delayed members
  are non-voting by setting `members\[n\].votes` to 0 can help
  improve performance.

### Behavior

Delayed members copy and apply operations from the source oplog on a delay.
When choosing the amount of delay, consider that the amount of delay:

- must be equal to or greater than your expected maintenance window durations.
- must be *smaller* than the capacity of the oplog. For more
  information on oplog size, see replica-set-oplog-sizing.

### Write Concern

Delayed replica set members can acknowledge write operations issued with either:

- `w: \<number\> \<\<number\>\>`. In this case, delayed members can
  acknowledge write operations even if they are non-voting members.
- `w : "majority"`. In this case,
  delayed members must be voting members (that is, `members\[n\].votes` greater
  than `0`) to acknowledge the write operation. Non-voting replica
  set members (that is, `members\[n\].votes` is `0`) cannot contribute to
  acknowledging write operations with `majority` write concern.

Delayed secondaries can return write acknowledgment no earlier than the
configured `members\[n\].secondaryDelaySecs`.

For more information, refer to the write concern "w" Option.

### Sharding

In sharded clusters, delayed members have limited utility when the
balancer is enabled. Because delayed members replicate chunk
migrations with a delay, the state of delayed members in a sharded
cluster are not useful for recovering to a previous state of the
sharded cluster if any migrations occur during the delay window.

## Example

In the following 5-member replica set, the primary and all secondaries
have copies of the data set. One member applies operations with a
delay of 3600 seconds (one hour). This delayed member is also
*hidden* and is a *priority 0 member*.

## Configuration

A delayed member has its
`members\[n\].priority` equal to `0`,
`members\[n\].hidden` equal to `true`, and
its `members\[n\].secondaryDelaySecs` equal to the
number of seconds of delay:

```javascript
{
   "_id" : <num>,
   "host" : <hostname:port>,
   "priority" : 0,
   "secondaryDelaySecs" : <seconds>,
   "hidden" : true
}

```

To configure a delayed member, see
/tutorial/configure-a-delayed-replica-set-member.
