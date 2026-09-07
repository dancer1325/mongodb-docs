# Geospatial Queries

MongoDB supports query operations on geospatial data. This section
introduces MongoDB's geospatial features.

## Geospatial Data

In MongoDB, you can store geospatial data as GeoJSON objects or as legacy coordinate pairs.

### GeoJSON Objects

To calculate geometry over an Earth-like sphere, store your location
data as GeoJSON objects.

### Legacy Coordinate Pairs

To calculate distances on a Euclidean plane, store your location data
as legacy coordinate pairs and use a geo-2d index. MongoDB
supports spherical surface calculations on legacy coordinate pairs by using
a geo-2dsphere index if you manually convert the data to
the GeoJSON Point type.

## Geospatial Indexes

MongoDB provides the following geospatial index types to support
geospatial queries. For more information on geospatial indexes, see
geospatial-index.

### `2dsphere`

2dsphere indexes support queries that calculate
geometries on an earth-like sphere.

For more information on the `2dsphere` index, see
2dsphere-index.

### `2d`

2d indexes support queries that calculate
geometries on a two-dimensional plane.
Although the index can support `\$nearSphere` queries that
calculate on a sphere, if possible, use the geo-2dsphere index
for spherical queries.

For more information on the `2d` index, see 2d-index.

## Geospatial Queries

> **Note**
>

### Geospatial Query Operators

MongoDB provides the following geospatial query operators. For more details, including examples, see the respective reference pages.

- - Name
  - Description
- - `\$geoIntersects`
  - Selects geometries that intersect with a GeoJSON geometry.
    The `2dsphere` index supports `\$geoIntersects`.
- - `\$geoWithin`
  - Selects geometries within a bounding GeoJSON geometry. The `2dsphere` and
    `2d` indexes support `\$geoWithin`.
- - `\$near`
  - Returns geospatial objects in proximity to a point.
    Requires a geospatial index. The `2dsphere` and `2d` indexes
    support `\$near`.
- - `\$nearSphere`
  - Returns geospatial objects in proximity to a point on a sphere.
    Requires a geospatial index. The `2dsphere` and `2d` indexes
    support `\$nearSphere`.

> **Note**
>

### Geospatial Aggregation Stage

MongoDB provides the following geospatial aggregation pipeline stage:

- - Stage
  - Description
- - `\$geoNear`
  - 

For more details, including examples, see `\$geoNear`
reference page.

## Geospatial Models

MongoDB geospatial queries can interpret geometry on a flat surface or
a sphere.

`2dsphere` indexes support only spherical queries (i.e. queries that
interpret geometries on a spherical surface).

`2d` indexes support flat queries (i.e. queries that interpret
geometries on a flat surface) and some spherical queries. While `2d`
indexes support some spherical queries, the use of `2d` indexes for
these spherical queries can result in error. If possible, use
`2dsphere` indexes for spherical queries.

The following table lists the geospatial query operators, supported
query, used by each geospatial operations:

- - Operation
  - Spherical/Flat Query
  - Notes

- - `\$near` (GeoJSON centroid
    point in this line and the following line, 2dsphere index)
  - Spherical
  - See also the `\$nearSphere` operator, which provides the
    same functionality when used with GeoJSON and a 2dsphere index.

- - `\$near` (legacy coordinates, 2d index)
  - Flat
  - 

- - `\$nearSphere` (GeoJSON point, 2dsphere index)

  - Spherical

  - Provides the same functionality as `\$near` operation that
    uses GeoJSON point and a
    2dsphere index.

    For spherical queries, it may be preferable to use
    `\$nearSphere` which explicitly specifies the spherical
    queries in the name rather than `\$near` operator.

- - `\$nearSphere` (legacy coordinates, 2d index)
  - Spherical
  - Use GeoJSON points instead.

- - `\$geoWithin` : { \`\$geometry\`: ... }
  - Spherical
  - 

- - `\$geoWithin` : { \`\$box\`: ... }
  - Flat
  - 

- - `\$geoWithin` : { \`\$polygon\`: ... }
  - Flat
  - 

- - `\$geoWithin` : { \`\$center\`: ... }
  - Flat
  - 

- - `\$geoWithin` : { \`\$centerSphere\`: ... }
  - Spherical
  - 

- - `\$geoIntersects`
  - Spherical
  - 

- - `\$geoNear` aggregation stage (2dsphere index)
  - Spherical
  - 

- - `\$geoNear` aggregation stage (2d index)
  - Flat
  - 

## Perform Geospatial Queries in Atlas

You can use the {+atlas+} UI
to perform geospatial queries in Atlas.

**Create an index**

If your geospatial collection does not already have a geospatial
index, you must create one.

1.  Select the database for the collection.

    The main panel and Namespaces on the left side
    list the collections in the database.

2.  Select the collection.

    Select the collection that contains your geospatial data on
    the left-hand side or in the main panel. The main panel displays
    the Find, Indexes, and
    Aggregation views.

3.  Select the Index view.

    When you open the Index view, Atlas
    displays any indexes that exist on the collection.

4.  Define the Index for the geo Type

    Press the Create Index button.

    Define a geo Type index. Refer to
    How to Index GeoJSON Objects.

**Query the geospatial data**

1.  Select the Find view.

    From the collection that contains your geospatial
    data, select the Find tab to view your geospatial
    collection.

2.  Enter a query.

    Enter a query in the Filter text box. Use
    any of the geospatial query operators to perform the relevant query
    on your geospatial data. A geospatial query might resemble:

```javascript
{ 
  "coordinates": { 
    $geoWithin: { 
      $geometry: { 
        type: "Polygon", 
        coordinates: [ 
          [ 
            [-80.0, 10.00], [ -80.0, 9.00], [ -79.0, 9.0], [ -79.0, 10.00 ], [ -80.0, 10.0 ] 
          ] 
        ] 
      } 
    } 
  } 
}

```

#\. Press the Apply button.

> Press the Apply button to apply your query.
> Atlas filters the geospatial data to show only documents
> that match your geospatial query.

You can create and execute aggregation pipelines to perform geospatial
queries in the {+atlas+} UI.

**Access the aggregation pipeline builder**

1.  Select the database for the collection.

    The main panel and Namespaces on the left side list the
    collections in the database.

2.  Select the collection.

    Select the collection that contains your geospatial data on
    the left-hand side or in the main panel. The main panel displays
    the Find, Indexes, and
    Aggregation views.

3.  Select the Aggregation view.

    When you first open the Aggregation view, Atlas
    displays an empty aggregation pipeline.

**Create your geospatial query aggregation pipeline**

1.  Select an aggregation stage.

    Select an aggregation stage from the Select dropdown in
    the bottom-left panel.

    The toggle to the right of the dropdown dictates whether the
    stage is enabled.

    Use the `\$geoNear` stage to perform geospatial
    queries in your aggregation pipeline.

2.  Fill in your aggregation stage.

    Fill in your stage with the appropriate values.
    If Comment Mode is
    enabled, the pipeline builder provides syntactic guidelines for
    your selected stage.

    As you modify your stage, Atlas updates the preview documents on
    the right based on the results of the current stage.

    Your `\$geoNear` stage may resemble:

```javascript
{
  near: { type: "Point", coordinates: [ -73.9667, 40.78 ] },
  spherical: true,
  query: { category: "Parks" },
  distanceField: "calcDistance"
}

```

1.  Run other pipeline stages as needed.

    Add stages as needed to complete your aggregation pipeline.
    You might add `\$out` or
    `\$merge` to write the results to a
    view or the current collection.

## Examples

The `places` collection above has a `2dsphere` index.
The following query uses the `\$near` operator to return
documents that are at least 1000 meters from and at most 5000 meters
from the specified GeoJSON point, sorted in order from nearest to
farthest:

```javascript
db.places.find(
   {
     location:
       { $near:
          {
            $geometry: { type: "Point",  coordinates: [ -73.9667, 40.78 ] },
            $minDistance: 1000,
            $maxDistance: 5000
          }
       }
   }
)

```

The following operation uses the `\$geoNear` aggregation
operation to return documents that match the query filter `{ category: "Parks" }`, sorted in order of nearest to farthest to the specified
GeoJSON point:

```javascript
db.places.aggregate( [
   {
      $geoNear: {
         near: { type: "Point", coordinates: [ -73.9667, 40.78 ] },
         spherical: true,
         query: { category: "Parks" },
         distanceField: "calcDistance"
      }
   }
] )

```
