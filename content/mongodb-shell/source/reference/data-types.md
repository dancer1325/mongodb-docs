# Data Types

This document highlights changes in type usage between `mongosh` and
the legacy `mongo` shell. See the
\[Extended JSON\](<https://www.mongodb.com/docs/manual/reference/mongodb-extended-json/>) reference for
additional information on supported types.

## Date

`mongosh` provides various methods to return the date, either as a
string or as a `Date` object:

- `Date()` method which returns the current date as a string.
- `new Date()` constructor which returns a `Date` object using the
  `ISODate()` wrapper.
- `ISODate()` constructor which returns a `Date` object using the
  `ISODate()` wrapper.

## ObjectId

`mongosh` provides the `ObjectId()` wrapper class around the
objectid data type. To generate a new ObjectId, use the following
operation in `mongosh`:

```javascript
new ObjectId

```

Starting in 1.8.0, the `ObjectId` wrapper no longer accepts:

- ObjectId.prototype.generate
- ObjectId.prototype.getInc
- ObjectId.prototype.get_inc
- ObjectId.getInc

> **See also**
>
> `ObjectId`

## Double

The `Double()` constructor can be used to
explicitly specify a double:

```javascript
db.types.insertOne(
   {
      "_id": 2,
      "value": Double(1),
      "expectedType": "Double" 
   }
)

```

> **Note**
>
> If field's value is a number that can be converted to a 32-bit integer,
> `mongosh` will store it as `Int32`. If not, `mongosh` defaults to
> storing the number as a `Double`. To specify the value type, use the
> `Double()` or `Int32()` constructors.

## Int32

The `Int32()` constructor can be used to
explicitly specify 32-bit integers.

```javascript
db.types.insertOne(
   {
       "_id": 1,
       "value": Int32(1),
       "expectedType": "Int32" 
   }
)

```

> **Warning**
>
> Default `Int32` and `Double` types may be stored inconsistently
> if you connect to the same collection using both `mongosh` and the
> legacy `mongo` shell.

## Long

The `Long()` constructor can be used to
explicitly specify a 64-bit integer.

```javascript
db.types.insertOne(
   {
      "_id": 3,
      "value": Long(1),
      "expectedType": "Long" 
   }
)

```

> **Note**
>
> In the legacy `mongo` shell `NumberLong()` accepted
> either a string or integer value. In `mongosh`, `NumberLong()`
> only accepts string values. `Long()` provides
> methods to manage conversions to and from 64-bit values.

## Decimal128

`Decimal128()` values are 128-bit
decimal-based floating-point numbers that emulate decimal rounding with
exact precision.

This functionality is intended for applications that handle
\[monetary data\](<https://www.mongodb.com/docs/manual/tutorial/model-monetary-data/>), such as
financial, tax, and scientific computations.

The `Decimal128` \[BSON type\](<https://www.mongodb.com/docs/manual/reference/bson-types/>)
uses the IEEE 754 decimal128 floating-point numbering format which
supports 34 decimal digits (i.e. significant digits) and an exponent
range of −6143 to +6144.

```javascript
db.types.insertOne(
   {
       "_id": 5,
       "value": Decimal128("1"),
       "expectedType": "Decimal128" 
   }
)


```

> **Note To use the `Decimal128` data type with a**
>
> MongoDB driver, be sure to use a driver
> version that supports it.

### Equality and Sort Order

Values of the `Decimal128` type are compared and sorted with other
numeric types based on their actual numeric value. Numeric values
of the binary-based `Double` type generally have approximate
representations of decimal-based values and may not be exactly
equal to their decimal representations.

## Timestamp

MongoDB uses a
\[BSON Timestamp\](<https://www.mongodb.com/docs/manual/reference/bson-types/#timestamps/>) internally
in the oplog. The `Timestamp` type works similarly to the
Java Timestamp type. Use the Date type
for operations involving dates.

A `Timestamp` signature has two optional parameters.

```javascript
Timestamp( { "t": <integer>, "i": <integer> } )

```

- - Parameter
  - Type
  - Default
  - Definition
- - `t`
  - integer
  - Current time since UNIX epoch.
  - Optional. A time in seconds.

\* - `i`  
- integer
- 1
- Optional. Used for ordering when there are multiple operations
  within a given second. `i` has no effect if used without
  `t`.

For usage examples, see timestamp-ex-default,
timestamp-ex-custom.

## Type Checking

Use the `\$type` query operator or examine the object constructor
to determine types.

The Javascript `typeof` operator returns generic values such as
`number` or `object` rather than the more specific `Int32` or
`ObjectId`.

Javascript's `instanceof` operator is not reliable. For example,
`instanceof` assigns BSON values in a server response to a different
base class than user supplied values.

For usage examples, see type-check-ex-type and
type-check-ex-constructor.

## Examples

### Return Date as a String

To return the date as a string, use the `Date()` method, as in the
following example:

```javascript
var myDateString = Date();

```

To print the value of the variable, type the variable name in the
shell, as in the following:

```javascript
myDateString

```

The result is the value of `myDateString`:

```javascript
Wed Dec 19 2012 01:03:25 GMT-0500 (EST)

```

To verify the type, use the `typeof` operator, as in the following:

```javascript
typeof myDateString

```

The operation returns `string`.

### Return Date

`mongosh` wraps objects of `Date` type with the
`ISODate` helper; however, the objects remain of type `Date`.

The following example uses both the `new Date()` constructor and the
`ISODate()` constructor to return `Date` objects.

```javascript
var myDate = new Date();
var myDateInitUsingISODateWrapper = ISODate();

```

You can use the `new` operator with the `ISODate()` constructor as
well.

To print the value of the variable, type the variable name in the
shell, as in the following:

```javascript
myDate

```

The result is the `Date` value of `myDate` wrapped in the
`ISODate()` helper:

```javascript
ISODate("2012-12-19T06:01:17.171Z")

```

To verify the type:

```javascript
var myDate = ISODate("2021-03-21T06:00:00.171Z")
Object.prototype.toString.call(myDate) === "[object Date]"

```

The operation returns `true`.

### Numeric Types

Consider the `types` collection:

```javascript
{ _id: 1, value: 1, expectedType: 'Int32' },
{ _id: 2, value: Long("1"), expectedType: 'Long' },
{ _id: 3, value: 1.01, expectedType: 'Double' },
{ _id: 4, value: Decimal128("1.01"), expectedType: 'Decimal128' },
{ _id: 5, value: 3200000001, expectedType: 'Double' }

```

This table shows the results of the `db.types.find( <QUERY> )`
command for the corresponding `<QUERY>`. The type names and aliases
are given on the \[BSON types\](<https://www.mongodb.com/docs/manual/reference/bson-types/>) page.

- - Query
  - Results

- - ``` javascript
    {
       "value":
          {
             $type: "int"
          }
    }
    ```

  - ``` javascript
    {
       _id: 1,
       value: 1,
       expectedType: 'Int32'
    }
    ```

- - ``` javascript
    {
       "value":
          {
             $type: "long"
          }
    }
    ```

  - ``` javascript
    {
       _id: 2,
       value: Long("1"),
       expectedType: 'Long'
    }
    ```

- - ``` javascript
    {
       "value":
          {
             $type: "decimal"
          }
    }
    ```

  - ``` javascript
    {
       _id: 4,
       value: Decimal128("1"),
       expectedType: 'Decimal128'
    }
    ```

- - ``` javascript
    {
       "value":
          {
             $type: "double"
          }
    }
    ```

  - ``` javascript
    {
       _id: 3,
       value: 1.01,
       expectedType: 'Double'
    }
    {
       _id: 5,
       value: 3200000001,
       expectedType: 'Double'
    }
    ```

- - ``` javascript
    {
       "value":
          {
             $type: "number"
          }
    }
    ```

  - ``` javascript
    {
        _id: 1,
        value: 1,
        expectedType: 'Int32'
    }
    {
        _id: 2,
        value: Long("1"),
        expectedType: 'Long'
    }
    {
        _id: 3,
        value: 1.01,
        expectedType: 'Double'
    }
    {
        _id: 4,
        value: Decimal128("1.01"),
        expectedType: 'Decimal128'
    }
    {
        _id: 5,
        value: 3200000001,
        expectedType: 'Double'
    }
    ```

- - ``` javascript
    {
        "value": 1.01 
    }
    ```

  - ``` javascript
    { 
        _id: 3,
        value: 1.01,
        expectedType: 'Double'
    }
    ```

\* - .. code-block:: javascript  
copyable  
false

{  
"value": 1

}

\- .. code-block:: javascript
:copyable: false

{  
<span id="id">id</span>: 1,
value: 1,
expectedType: 'Int32'

#### }

> <span id="id">id</span>: 2,
> value: Long("1"),
> expectedType: 'Long'

}

The query `{ "value": 1.01 }` implicitly searches for the
`Double` representation of `1.01`. Document `_id: 4` is a
`Decimal128` so it is not selected.

Note, however, that `{ "value": 1 }` returns both `Int32` and
`Long` types.

### Default Numeric Type Consistency

Consider the `typeExample` collection. This collection consists of
two identical documents, `{ "a": 1 }`. The first document was created
in the legacy `mongo` shell, the second document was created in
`mongosh`.

We can use the `\$type` operator in an aggregation pipeline
to see the type that was assigned in each shell.

```javascript
db.typeExample.aggregate( 
  [ 
    {
       $project: 
         { 
           "valueType": 
             {
               "$type": "$a"
             },
           "_id": 0
         }
    }
  ]
)

```

In the first document, created in the legacy `mongo` shell, the value
was stored as a `double`. In the `mongosh` document the value was
stored as type `int`.

```javascript
.. :copyable: false

[
  { 
     valueType: 'double'  // inserted in legacy mongo shell
  },
  {
     valueType: 'int'     // inserted in mongosh
  }
]

```

### Timestamp a New Document

Use `Timestamp()` without parameters to insert multiple documents
using the default settings:

```javascript
db.flights.insertMany(
   [
      { arrival: "true", ts: Timestamp() },
      { arrival: "true", ts: Timestamp() },
      { arrival: "true", ts: Timestamp() }
   ]
)

```

Run `db.flights.find({})` to see the timestamps. Notice that even
though all three entries were stamped in the same second, the interval
was incremented on each one.

```javascript
[
   {
      _id: ObjectId("6114216907d84f5370391919"),
      arrival: 'true',
      ts: Timestamp({ t: 1628709225, i: 1 })
   },
   {
      _id: ObjectId("6114216907d84f537039191a"),
      arrival: 'true',
      ts: Timestamp({ t: 1628709225, i: 2 })
   },
   {
      _id: ObjectId("6114216907d84f537039191b"),
      arrival: 'true',
      ts: Timestamp({ t: 1628709225, i: 3 })
   }
]

```

### Create a Custom Timestamp

Use custom parameters to insert multiple documents with a specific
`Timestamp`.

This operation inserts three documents into the `flights` collection
and uses the UNIX epoch value `1627811580` to
set the `ts` times to 9:53 GMT on August 1, 2021.

```javascript
db.flights.insertMany(
   [
      { arrival: "true", ts: Timestamp(1627811580, 10) },
      { arrival: "true", ts: Timestamp(1627811580, 20) },
      { arrival: "true", ts: Timestamp(1627811580, 30) }
   ]
)

```

The resulting documents look like this:

```javascript
[
   {
      _id: ObjectId("6123d8315e6bba6f61a1031c"),
      arrival: 'true',
      ts: Timestamp({ t: 1627811580, i: 10 })
   },
   {
      _id: ObjectId("6123d8315e6bba6f61a1031d"),
      arrival: 'true',
      ts: Timestamp({ t: 1627811580, i: 20 })
   },
   {
      _id: ObjectId("6123d8315e6bba6f61a1031e"),
      arrival: 'true',
      ts: Timestamp({ t: 1627811580, i: 30 })
   }
]

```

### Type Checking with \$type()

The `\$type` query operator accepts either a string alias or a
numeric code corresponding to the data type. See
\[BSON Types\](<https://www.mongodb.com/docs/manual/reference/bson-types/>) for a list of BSON data
types and their corresponding numeric codes.

For example, these checks for the `Decimal128` type are equivalent:

```javascript
db.types.find( { "value": { $type: "decimal" } } )
db.types.find( { "value": { $type: 19 } } )

```

### Type Checking with a Constructor

Examine the object `constructor` to determine the type. For example,
the output of `db.collection.find()` is a `Cursor`.

```javascript
var findResults = db.housing.find({"multiUnit": true} )
findResults.constructor.name     // Returns the type
```
