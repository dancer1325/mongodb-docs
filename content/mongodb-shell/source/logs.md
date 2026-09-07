# Shell Logs

The MongoDB Shell stores a log for each session. The log contains a record
of commands run, server connections, and other useful information about
actions performed in the MongoDB Shell.

## Get Started

You can view or tail the logs for a MongoDB Shell session based on its log
ID.

To learn how to view and customize MongoDB Shell logs, see these pages:

- mdb-shell-view-logs
- mdb-shell-command-history
- mongosh-log-location
- mongosh-custom-log-entries
- mongosh-log-compression
- mongosh-logs-disable

## Details

See the following details about MongoDB Shell logging behavior:

### Format

The MongoDB Shell uses [Newline delimited JSON](https://github.com/ndjson/ndjson-spec) to store session logs, which
is the same log format used for MongoDB server logs.

### Redaction

### Retention

By default, the MongoDB Shell retains up to 100 log files, and individual
log files are retained for 30 days. To customize log file retention, see
mongosh-logs-retention.
