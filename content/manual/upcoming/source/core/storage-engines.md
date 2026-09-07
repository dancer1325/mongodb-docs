# Storage Engines for Self-Managed Deployments

The storage engine is the component of the database that is
responsible for managing how data is stored, both in memory and on disk.
MongoDB supports multiple storage engines, as different engines perform
better for specific workloads. Choosing the appropriate storage engine
for your use case can significantly impact the performance of your
applications.

➤ WiredTiger Storage Engine (*Default*)  
WiredTiger is the default storage engine and is
recommended for new deployments. WiredTiger provides a document-level
concurrency model, checkpointing, and compression, among other features.

In MongoDB Enterprise, WiredTiger also supports
/core/security-encryption-at-rest. See
encrypted-storage-engine.

➤ In-Memory Storage Engine  
An In-Memory storage engine is available
in MongoDB Enterprise. Rather than storing documents on-disk, it
retains them in-memory for more predictable data latencies.
