# Production Considerations (Sharded Clusters)

You can perform multi-document transactions on sharded clusters.

The following page lists concerns specific to running transactions on a
sharded cluster. These concerns are in addition to those listed in
/core/transactions-production-consideration.

## Performance

### Single Shard

Transactions that target a single shard should have the same
performance as replica-set transactions.

### Multiple Shards

Transactions that affect multiple shards incur a greater performance
cost.

> **Note**
>
> On a sharded cluster, transactions that span multiple shards will
> error and abort if any involved shard contains an arbiter.

### Time Limit

To specify a time limit, specify a `maxTimeMS` limit on
`commitTransaction`.

If `maxTimeMS` is unspecified, MongoDB will use the
`transactionLifetimeLimitSeconds`.

If `maxTimeMS` is specified but would result in transaction that
exceeds `transactionLifetimeLimitSeconds`, MongoDB will use
the `transactionLifetimeLimitSeconds`.

To modify `transactionLifetimeLimitSeconds` for a sharded
cluster, the parameter must be modified for all shard replica set
members.

## Read Concerns

Multi-document transactions support `"local"`,
`"majority"`, and `"snapshot"` read concern
levels.

For transactions on a sharded cluster, only the
`"snapshot"` read concern provides a consistent snapshot
across multiple shards.

For more information on read concern and transactions, see
transactions-read-concern.

## Write Concerns

> **Note**
>

## Arbiters

## Backups and Restores

> **Warning**
>

## Chunk Migrations

> **See also**
>
> `shardingStatistics.countDonorMoveChunkLockTimeout`

## Outside Reads During Commit

> **See also**
>
> transactions-atomicity

## Additional Information

See also /core/transactions-production-consideration.
