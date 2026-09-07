# EJSON.parse()

The `EJSON.parse()` method converts string values to JSON.

## Syntax

The `EJSON.parse()` method takes a string as input and an optional
modifier that controls the output format.

```javascript
EJSON.parse(string, [options])

```

## Command Fields

The `EJSON.parse()` method takes these fields:

- - Field
  - Type
  - Necessity
  - Description
- - `value`
  - string
  - Required
  - String `EJSON.parse()` transforms into JSON key-value pairs
- - `options`
  - string
  - Optional
  - Modifies output types. The only
    option is `{ relaxed: <boolean> }`.
- - Value
  - Effect
- - `true`
  - Returns native JavaScript types when possible

\* - `false`  
- Prefer to return BSON types

## Behavior

You can call `EJSON.parse()` from inside an interactive `mongosh`
session or from the system command line using `--eval`.

Call `EJSON.parse()` from an interactive session:

```javascript
EJSON.parse(string)

```

Call `EJSON.parse()` from the system command line:

```shell
mongosh --eval "EJSON.parse(string)"

```

## Examples

To try these examples, first create the `sales` collection:

```javascript
db.sales.insertMany( [ 
   { custId: 345, purchaseDate: ISODate("2023-07-04"), quantity: 4, cost: Decimal128("100.60"), },
   { custId: 346, purchaseDate: ISODate("2023-07-12"), quantity: 3, cost: Decimal128("175.45"), },
   { custId: 486, purchaseDate: ISODate("2023-08-01"), quantity: 9, cost: Decimal128("200.53"), },
] )

```

### Format Input with EJSON.parse()

`EJSON.parse()` accepts a string as input. For this example, use the
EJSON.stringify() method to export the
`sales` collection as a string.

```javascript
let salesCollection = EJSON.stringify( db.sales.find().toArray() )   

```

Use `EJSON.parse()` to format the string for methods like
`db.collection.insertMany()` that expect JSON pairs:

```javascript
db.salesRestored.insertMany( EJSON.parse( salesCollection ) )

```

- `EJSON.parse()` formats the values in `salesCollection` as JSON
  pairs.
- `db.salesRestored.insertMany()`
  uses the JSON pairs to create the `salesRestored` collection.

### Use EJSON.parse() from the command line

To import string data from an external source such as a file or an API
call, use `EJSON.parse()` with the `mongosh --eval` method.

For this example, save the `sales` collection as a file.

```javascript
let salesCollection = EJSON.stringify( db.sales.find().toArray() )   

fs.writeFileSync( 'sales.json', salesCollection ) 

```

The code creates a file on your local system called `sales.json`. To
import the file and create a new collection, exit `mongosh` and run an
`--eval` operation from the command line.

```javascript
# Note: This example is formatted to fit on the page.

mongosh --quiet \
        --eval "db.salesFromFile.insertMany( \
           EJSON.parse( fs.readFileSync( 'sales.json', 'utf8' ) ) )"

```

- `EJSON.parse()` takes a string as input. This example uses
  `fs.readFileSync()` to read the `sale.json` file as a string.
- `EJSON.parse()` formats the input string as JSON pairs.
- `db.salesFromFile.insertMany()` creates the `salesFromFile`
  collection from the JSON pairs.

## Learn More

- \[EJSON\](<https://www.npmjs.com/package/bson#EJSON>) documentation
- Mozilla Developer Network \[JSON.parse\](<https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse>) documentation
