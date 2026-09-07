# Query Plans

For any given query, the MongoDB query planner chooses and caches the
most efficient query plan given the available indexes. To evaluate the
efficiency of query plans, the query planner runs all candidate plans
during a trial period. In general, the winning plan is the query plan
that produces the most results during the trial period while performing
the least amount of work.

The associated plan cache entry is used for subsequent queries with the
same plan cache query shape.

The following diagram illustrates the query planner logic:

## Plan Cache Entry State

Each plan cache query shape is associated with one of three states in
the cache:

- - State
  - Description

- - Missing

  - <div id="cache-entry-missing">

    No entry for this shape exists in the cache.

    </div>

    For a query, if the cache entry state for a plan cache query shape is
    Missing:

    1.  Candidate plans are evaluated and a winning plan is selected.
    2.  The cache creates an entry for the plan cache query shape in state
        Inactive with a value that
        quantifies the amount of work required by the plan.

- - Inactive

  - <div id="cache-entry-inactive">

    The entry in the cache is a placeholder entry for this shape.
    That is, the planner has seen the shape, calculated a value that
    quantifies the amount of work required by the plan and stored the
    shape placeholder entry but the plan cache query shape is **not** used to
    generate query plans.

    </div>

    For a query, if the cache entry state for a shape is
    Inactive:

    1.  Candidate plans are evaluated and a winning plan is selected.
    2.  The selected plan's value that quantifies the amount of work
        required by the plan is compared to the Inactive entry's. If the selected plan's value is:
        - Less than or equal to the Inactive entry's:  
          The selected plan replaces the placeholder Inactive entry and has an Active state.

          If before the replacement happens, the Inactive entry becomes Active (for example, due to another query
          operation), the newly active entry will only be replaced
          if its value that quantifies the amount of work required
          by the plan is greater than the selected plan.

        - Greater than the Inactive entry's:  
          The Inactive entry remains
          but its value that quantifies the amount of work required
          by the plan is incremented.

\* - Active

> - <div id="cache-entry-active">
>
>   The entry in the cache is for the winning plan. The planner can
>   use this entry to generate query plans.
>
>   </div>
>
>   For a query, if the cache entry state for a shape is Active:
>
>   The active entry is used to generate query plans.
>
>   The planner also evaluates the entry's performance and if its
>   value that quantifies the amount of work required by the plan no
>   longer meets the selection criterion, it will transition to
>   Inactive state.

See query-plans-plan-cache-flushes for additional scenarios that trigger
changes to the plan cache.

## Query Plan and Cache Information

To view the query plan information for a given query, you can use
`db.collection.explain()` or the `cursor.explain()` .

To view plan cache information for a collection, you can use the
`\$planCacheStats` aggregation stage.

## Plan Cache Flushes

The query plan cache does not persist if a `mongod`
restarts or shuts down. In addition:

- Any DDL event clears the plan cache for the relevant collection.
  Example DDL events include dropping a collection, and creating,
  deleting, or hiding an index.
- Least recently used (LRU) cache replacement mechanism clears the
  least recently accessed cache entry, regardless of state.

Users can also:

- Manually clear the entire plan cache using the
  `PlanCache.clear()` method.
- Manually clear specific plan cache entries using the
  `PlanCache.clearPlansByQuery()` method.

> **See also**
>
> query-hash-plan-cache-key

## Plan Cache Debug Info Size Limit

Starting in MongoDB 5.0, the
plan cache will save full
`plan cache` entries only if the cumulative size of the
`plan caches` for all collections is lower than 0.5 GB. When the
cumulative size of the `plan caches` for all collections exceeds this
threshold, additional `plan cache` entries are stored without the
following debug information:

- createdFromQuery
- cachedPlan
- creationExecStats
- candidatePlanScores

The estimated size in bytes of a `plan cache` entry is available in
the output of `\$planCacheStats`.

## planCacheShapeHash and planCacheKey

### planCacheShapeHash

### planCacheKey

For example, consider a collection `foo` with the following indexes:

```javascript
db.foo.createIndex( { x: 1 } )
db.foo.createIndex( { x: 1, y: 1 } )
db.foo.createIndex( { x: 1, z: 1 }, { partialFilterExpression: { x: { $gt: 10 } } } )

```

The following queries on the collection have the same shape:

```javascript
db.foo.explain().find( { x: { $gt: 5 } } )  // Query Operation 1
db.foo.explain().find( { x: { $gt: 20 } } ) // Query Operation 2

```

Given these queries, the index with the partial filter expression can support query operation 2 but *not*
support query operation 1. Since the indexes available to support query operation 1
differs from query operation 2, the two queries have different
`planCacheKey`.

If one of the indexes were dropped, or if a new index `{ x: 1, a: 1 }` were added, the `planCacheKey` for both query operations will
change.

### Availability

The `planCacheShapeHash` and `planCacheKey` are available in:

- explain() output fields:
  - `queryPlanner.planCacheShapeHash`
  - `queryPlanner.planCacheKey`
- profiler log messages
  and diagnostic log messages (i.e. mongod/mongos log messages) when logging slow queries.
- `\$planCacheStats` aggregation stage
- `PlanCache.listQueryShapes()`
  method/`planCacheListQueryShapes` command
- `PlanCache.getPlansByQuery()`
  method/`planCacheListPlans` command

## Index Filters

Index filters are set with the `planCacheSetFilter` command
and determine which indexes the planner evaluates for a query shape. A plan cache query shape consists of a combination of query, sort, and
projection specifications. If an index filter exists for a given query
shape, the planner only considers those indexes specified in the
filter.

When an index filter exists for the plan cache query shape, MongoDB ignores the
`hint()`. To see whether MongoDB applied an index
filter for a query shape, check the `indexFilterSet`
field of either the `db.collection.explain()` or the
`cursor.explain()` method.

Index filters only affect which indexes the planner evaluates; the
planner may still select the collection scan as the winning plan for
a given plan cache query shape.

Index filters exist for the duration of the server process and do not
persist after shutdown. MongoDB also provides a command to manually remove
filters.

Because index filters override the expected behavior of the planner
as well as the `hint()` method, use index filters
sparingly.

> **See also**
>
> - `planCacheListFilters`
> - `planCacheClearFilters`
> - `planCacheSetFilter`
>
> \- /applications/indexes
