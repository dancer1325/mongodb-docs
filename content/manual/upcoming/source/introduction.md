# Introduction to MongoDB

* [environments | you can create a MongoDB database](includes/fact-environments.rst)
* [how to create a MongoDB database -- via -- Atlas UI](../../../atlas/source/getting-started.md)

## Document Database

* MongoDB's record
  * == document /
    * == 💡data structure 💡
      * == field + value pairs
        * ALLOWED value pairs
          * OTHER documents
          * arrays
          * arrays of documents 
    * == JSON objects
    * benefits
      * == native data types | MANY programming languages
      * embedded documents & arrays
        * reduce need -- for -- expensive joins
      * if you use a dynamic schema -> supports fluent polymorphism

  ![](images/crud-annotated-document.svg)

### Collections/Views/On-Demand Materialized Views

* collections
  * == tables | relational databases
  * uses
    * store MongoDB's documents 

* MongoDB
  * supports
    * collections
    * [views](core/views.md)
    * [materialized views](core/materialized-views.md)

## Key Features

### High Performance

TODO: 
MongoDB provides high performance data persistence. In particular,

- Support for embedded data models reduces I/O activity on database
  system.
- Indexes support faster queries and can include keys from embedded
  documents and arrays.

### Query API

The MongoDB Query API supports read and write operations (CRUD) as well as:

- Data Aggregation
- Text Search and Geospatial Queries.

> **See also**
>
> - /reference/sql-comparison
>
> \- /reference/sql-aggregation-comparison

### High Availability

MongoDB's replication facility, called replica set, provides:

- *automatic* failover
- data redundancy.

A replica set is a group of
MongoDB servers that maintain the same data set, providing redundancy
and increasing data availability.

### Horizontal Scalability

MongoDB provides horizontal scalability as part of its *core*
functionality:

- Sharding distributes data across a
  cluster of machines.
- Starting in 3.4, MongoDB supports creating zones of data based on the shard key. In a
  balanced cluster, MongoDB directs reads and writes covered by a zone
  only to those shards inside the zone. See the zone-sharding
  manual page for more information.

### Support for Multiple Storage Engines

MongoDB supports multiple storage engines:

- /core/wiredtiger (including support for
  /core/security-encryption-at-rest)
- /core/inmemory.

In addition, MongoDB provides pluggable storage engine API that allows
third parties to develop storage engines for MongoDB.
