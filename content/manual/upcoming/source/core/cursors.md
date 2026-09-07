# Cursors

A cursor is a pointer to the results of a query.
Cursors allow you to iterate over database results one batch at a time.

## Use Cases

When you execute `find()` and `aggregate()` methods using `mongosh`
or a `driver`, they return a cursor containing a batch of results.
You can access the resulting documents by manually iterating the cursor or
using the `toArray()` method. For more information, see
read-operations-cursors.

If you are accessing a capped collection,
you can use a tailable cursor that retrieves new documents as they are inserted
into the collection. For more information, see
tailable-cursors-landing-page.

## Behavior

Cursors created within a client session
are closed in the following scenarios:

- The client exhausts the cursor.
- A user manually closes the cursor.
- A user manually terminates the session.
- The session times out.

The `cursorTimeoutMillis` parameter specifies the timeout for idle
cursors and has a default value of 10 minutes. MongoDB times out idle cursors
created outside of sessions after this threshold. MongoDB extends the cursor
timeout each time the cursor returns a new batch. To manually close a cursor,
use `killCursors`.

The server session timeout is specified by the `localLogicalSessionTimeoutMinutes`
parameter and has a default value of 30 minutes. To extend a session beyond 30 minutes,
use `refreshSessions`. To manually terminate a session, use
`killSessions`.

If a cursor is opened outside of a session, MongoDB drivers and `mongosh`
create an implicit session and associate it with the operation.

### Concurrent Updates While Using a Cursor

### Cursor Results for Non-Existent `mongos` Databases

## Get Started

- read-operations-cursors
- tailable-cursors-landing-page
- `MongoDB Drivers`

## Details

When you run a `find` or `aggregate` operation,
the database executes a query until it finds enough documents
to fill a batch. When a batch is filled, the query
pauses. The paused query on the server is referred to as a *cursor*
and the ID associated with the paused query is a *cursor ID*.

The database returns the resulting batch and cursor ID to the client. MongoDB
drivers and `mongosh` store this data in a client-side cursor instance.
If there are more matching documents when you reach the end of a batch, the
client-side cursor automatically retrieves the next batch from the server using
`getMore`. To see how many results remain in the current batch, use
`cursor.objsLeftInBatch()`. To check if there are any results remaining
in the current batch or on the server, use `cursor.hasNext()`.

### Cursor Batches

Cursors return results in batches. The amount of data in a batch must be smaller
than the maximum BSON document size (16 MiB). To
specify the maximum number of documents allowed in a batch, see `cursor.batchSize()`.
By default, the batch size for `find()` and `aggregate()` operations is `101`.
Subsequent `getMore` operations issued against the resulting cursor
have no default batch size, so they are limited only by the 16 mebibyte
message size.

### Sorting

For queries that include a sort operation *without* an index, the
server must load all the documents in memory to perform the sort
before returning any results.

### Cursor Information

The `db.serverStatus()` method returns a document that includes
a `metrics` field. The `metrics` field contains a `metrics.cursor`
field with detailed cursor information. To learn more, see `metrics.cursor`.

## Learn More

- `MongoDB Drivers`
- doc-cursor-methods
