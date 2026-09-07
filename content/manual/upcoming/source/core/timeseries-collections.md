# Time Series

Time series data is a sequence of data points in which insights are
gained by analyzing changes over time.

Time series data is generally composed of these components:

- **Time**: Indicates when the data point was recorded.
- **Metadata**: A label or tag that identifies a data series and rarely changes.
  Metadata is stored in a `metaField`. You cannot add a `metaField` field
  to a time series document after you create it. Metadata is also known as `source`.
  For more information, see timeseries-collections-metafield.
- **Metrics**: Individual data points tracked at increments in time, often displayed
  as key-value pairs that change over time. Metrics are also known as values.
- **Measurements**: Documents that contain data for all metrics at a
  specific point in time. A measurement includes the time, metadata,
  and all metrics recorded at that moment.

This table shows examples of time series data:

- - **Example**
  - **Metrics**
  - **Metadata**
- - Stock data
  - Stock price
  - Stock ticker, exchange
- - Weather data
  - Temperature
  - Sensor identifier, location

\* - Website visitors  
- View count
- URL

For efficient time series data storage, MongoDB provides time series
collections.

The following example shows a measurement document for weather data:

```javascript
{
  "timestamp": ISODate("2025-08-19T12:00:00Z"),
  "metaField": {
    "sensorId": "A1234",
    "location": {
      "city": "New York",
      "state": "NY"
    }
  },
  "temperature": 25.4,
  "humidity": 48.2,
  "pressure": 1012.5,
  "windSpeed": 5.2,
  "windDirection": "NW"
}

```

In this example, the measurement contains:

- A timestamp showing when the data was recorded.
- Metadata identifying the sensor and its location.
- Multiple metrics, including temperature, humidity, pressure, wind speed, and
  direction, collected at the given time.

## Time Series Collections

> **New in version 5.0**
>

Time series collections efficiently store time series data. In time
series collections, writes are organized so that data from the same
source is stored alongside other data points from a similar point in
time.

> **Important Backwards-Incompatible Feature**
>

### Benefits

Compared to normal collections, storing time series data in time series
collections improves query efficiency and reduces the disk usage for
time series data and secondary indexes.
MongoDB 6.3 and later automatically creates a compound index on the time and metadata
fields for new time series collections.

Time series collections use an underlying columnar storage format and
store data in time-order. This format provides the following benefits:

- Reduced complexity for working with time series data
- Improved query efficiency
- Reduced disk usage
- Reduced I/O for read operations
- Increased WiredTiger cache usage

### Example Use Cases

Time Series collections are optimal for analyzing data over time. The
following table illustrates use cases for time series data:

- - Industry
  - Examples
- - Internet of Things (IoT)
  - - Sensor data (for example, smart home devices or fleet logistics)
    - Machine learning and artificial intelligence scraping
- - Financial Services
  - - High frequency trading
    - Financial quantitative analysis
    - Banking data (for example, accounting of banking transactions
      over time)
    - Stock market data
- - Retail and E-Commerce
  - - Transaction, sales, and price analysis
    - Inventory management

\* - DevOps  
- - Application logging
  - Infrastructure and network monitoring

Time Series collections are not intended for the following types of data:

- Unordered data
- Data that is not time-dependent

### Behavior

Time series collections generally behave like other MongoDB collections.
You insert and query data as usual.

> **Warning**
>
> Match expressions in update commands can only specify the
> metaField. You can't update other fields in a time series document.
> For more details, see Time Series Update Limitations.

MongoDB treats time series collections as writable non-materialized
views backed by an internal collection. When
you insert data, the internal collection automatically organizes time
series data into an optimized storage format.

Starting in MongoDB 6.3: if you create a new time series collection,
MongoDB also generates a compound index
on the metaField and timeField fields. To
improve query performance, queries on time series collections use the
new compound index. The compound index also uses the optimized storage
format.

Also, starting in MongoDB 8.0, if you create a time series collection
with a shard key containing the `timeField`, a log message is added to the log file on the primary shard.
In addition, a log message is added every 12 hours on the primary node
of the config server replica set. The log messages state
that using the `timeField` as a shard key in a time series collection
is deprecated and you must reshard your collection using the
`metaField`.

### metaFields

Time series documents can contain a metaField with metadata about each
document. MongoDB uses the metaField to group sets of
documents, both for internal storage optimization and query efficiency.
For more information about the metaField, see metaField Considerations.

### Indexes

MongoDB automatically creates a compound index on both the metaField and timeField of a time
series collection.

### Zone Sharding

## Next Steps

To get started with time series collections, see the tutorials on the following
pages:

- timeseries-quick-start
- timeseries-create-query-procedures
