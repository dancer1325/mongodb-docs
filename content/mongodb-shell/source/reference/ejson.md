# EJSON

MongoDB uses \[BSON\](<https://www.mongodb.com/basics/bson>), a binary serialization format, to store
documents and to exchange data. BSON is a rich format and has data types
that aren't included in the JSON standard. Extended JSON (EJSON) adds support for the additional types. EJSON
is a JSON compatible way to represent BSON values.

`mongosh` exposes the \[EJSON\](<https://www.npmjs.com/package/bson#EJSON>) interface from the \[Node.js BSON parser\](<https://www.npmjs.com/package/bson>) to
help you transform your data. Use the `EJSON` interface when you need
to transform BSON data.

- - EJSON Method
  - Use
- - EJSON.deserialize()
  - Convert Extended JSON objects to BSON objects. This method
    is useful to import JSON data from external applications.
- - EJSON.stringify()
  - Convert BSON objects to strings. This method is useful to
    transform `mongosh` output.
- - EJSON.serialize()
  - Convert BSON objects to Extended JSON representation as
    JavaScript objects. This method is useful to export JSON data for
    external data transformation applications.

\* - EJSON.parse()  
- Convert strings to JSON. This method is useful to transform
  inputs.

For additional EJSON capabilities, see the [npm EJSON documentation](https://www.npmjs.com/package/bson#EJSON).

## Learn More

[BSON specification](http://bsonspec.org/)
