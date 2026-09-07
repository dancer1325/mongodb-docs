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

### High Performance data persistence

* _Examples:_
  * support -- for -- embedded data models
    * Reason: 🧠reduces I/O activity | database system🧠
  * indexes
    * Reason:🧠enable
      * faster queries
      * keys -- from -- embedded documents & arrays🧠

### Query API

* supports
  * CRUD
  * Data Aggregation
  * Text Search
  * Geospatial Queries

* [vs SQL](reference/sql-comparison.md)
* [vs SQL aggregation](reference/sql-aggregation-comparison.md)

### High Availability / Replica set

* replica set
  * == MongoDB servers /
    * SAME data set
    * provides
      * *automatic* failover
      * data redundancy

### Horizontal Scalability

* Sharding
  * == distribute data | cluster of machines
  * | MongoDB v3.4,
    * you can create -- , based on the shard key, -- zones of data 
      * == MongoDB directs reads & writes / covered by a zone -- to -- those shards | the zone
  * [MORE](sharding.md)

### MULTIPLE Storage Engines

* supported storage engines
  * [wiredtiger](core/wiredtiger.md)
  * [inmemory](core/inmemory.md)

* pluggable storage engine API
  * enable
    * third parties can develop storage engines -- for MongoDB
