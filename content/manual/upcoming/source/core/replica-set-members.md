# Replica Set Members

A *replica set* in MongoDB is a group of `mongod` processes
that provide redundancy and high availability. The members of a
replica set are:

replica-set-primary-member  
The primary receives all write operations.

replica-set-secondary-members  
Secondaries replicate operations from the primary to maintain an
identical data set. Secondaries may have additional configurations
for special usage profiles. For example, secondaries may be
non-voting or
priority 0.

The minimum recommended configuration for a replica set is a three
member replica set with three data-bearing members: one primary and two secondary members. In some circumstances (such
as you have a primary and a secondary but cost constraints prohibit
adding another secondary), you may choose to include an arbiter. An arbiter participates in elections but does not hold data (i.e. does not provide
data redundancy).

A replica set can have up to 50 members but only 7 voting members.

> **See also**
>
> - `replSetGetStatus.votingMembersCount`
> - `replSetGetStatus.writableVotingMembersCount`

## Primary

## Secondaries

## Arbiter

For considerations when using an arbiter, see /core/replica-set-arbiter.
