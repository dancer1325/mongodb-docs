# Use Snippets in the Console

> **Warning**
>

This page is an overview of working with snippets in the
`mongosh` console.

## Install Snippet Packages

You must install a snippet package before you use it. Once a snippet
package has been installed, it is loaded each time
`mongosh` starts.

If you know the name of the snippet you want to install, enter:

```javascript
snippet install <name>

```

Otherwise, search the repository to get a list of available snippets.

```javascript
snippet search

```

Once the snippet is installed, enter `y` to load it.

```javascript
Running install...
Installed new snippets analyze-schema. Do you want to load them now? [Y/n]: y
Finished installing snippets: analyze-schema

```

> **Note**
>
> If this is the first time you have used snippets you may see a
> warning like the following:
>
> ```javascript
This operation requires downloading a recent release of npm. Do
you want to proceed? [Y/n]:

```
>
> You must install npm to use snippets.

## Run a Snippet

Before running a new snippet, run `snippet help <SNIPPET NAME>` to
learn more about the snippet's functionality.

For example, `snippet help analyze-schema` indicates that you can
use the the `analyze-schema` by passing it a collection name.

```javascript
testDB> snippet help analyze-schema
 # analyze-schema

 Analyze the schema of a collection or a cursor.

```js
> schema(db.coll);
┌─────────┬───────┬───────────┬────────────┐
│ (index) │   0   │     1     │     2      │
├─────────┼───────┼───────────┼────────────┤
│    0    │ '_id' │ '100.0 %' │ 'ObjectID' │
│    1    │ 'a  ' │ '50.0 %'  │  'Number'  │
│    2    │ 'a  ' │ '50.0 %'  │  'String'  │
└─────────┴───────┴───────────┴────────────┘

```

Once you know how to call a snippet, you can use it as in the following
example.

Consider the `reservations` collection:

```javascript
db.reservations.insertMany( [
   {"_id": 1001, "roomNum": 1, "reserved": true },
   {"_id": 1002, "roomNum": 2, "reserved": true },
   {"_id": 1003, "roomNum": 3, "reserved": "false" },
   {"_id": 1004, "roomNum": 4, "reserved": true },
] )

```

To analyze the collection, install the
`analyze-schema` snippet if it is not already present, pass in the
collection name to run it.

```javascript
snippet install analyze-schema
schema(db.reservations)

```

The document with `"_id": 3` was misentered as a string. The analysis
shows that the `reserved` field has string elements in addition to
the expected booleans.

```none
┌─────────┬────────────┬───────────┬───────────┐
│ (index) │     0      │     1     │     2     │
├─────────┼────────────┼───────────┼───────────┤
│    0    │ '_id     ' │ '100.0 %' │ 'Number'  │
│    1    │ 'reserved' │ '75.0 %'  │ 'Boolean' │
│    2    │ 'reserved' │ '25.0 %'  │ 'String'  │
│    3    │ 'roomNum ' │ '100.0 %' │ 'Number'  │
└─────────┴────────────┴───────────┴───────────┘

```

## Uninstall Snippets

Use the `snippet uninstall` command to remove a snippet. If you are
unsure of the name, the `snippet ls` command lists all installed
snippets.

This code uninstalls the `analyze-schema` snippet.

```javascript
snippet uninstall analyze-schema

```

## Find Available Snippet Packages

The `snippet ls` command returns a list of locally installed snippets
along with some version and source information.

```shell
snippets@ /root/.mongodb/mongosh/snippets
├── mongosh:PRIVATE..DecryptCards@1.0.5
├── mongosh:analyze-schema@1.0.5
└── npm@7.23.0

```

To see the snippets that are available in the registry, first
`refresh` the local metadata cache and then `search`.

```javascript
snippet refresh
snippet search

```

`snippet search` lists available snippets, their version, and gives a
brief description.

This instance has a second, private registry configured. Since the
private registry was listed first, those snippets precede the MongoDB
snippets in the list of available snippets.

```javascript
┌─────────┬─────────────────────────────────┬─────────┬────────────────────────────────────────────────────────────────┐
│ (index) │              name               │ version │                          description                           │
├─────────┼─────────────────────────────────┼─────────┼────────────────────────────────────────────────────────────────┤
│    0    │     'PRIVATE..DecryptCards'     │ '1.0.5' │                 'Decrypt credit card numbers'                  │
│    1    │ 'PRIVATE..updateAuthentication' │ '1.0.2' │             'Update user pwds and authentication'              │
│    2    │          'resumetoken'          │ '1.0.2' │                 'Resume token decoder script'                  │
│    3    │          'mongocompat'          │ '1.0.7' │            'mongo compatibility script for mongosh'            │
│    4    │         'spawn-mongod'          │ '1.0.1' │                'Spin up a local mongod process'                │
│    5    │        'mock-collection'        │ '1.0.2' │ 'mockCollection([{ a: 1 }, { a: 2 }]).find({ a: { $gt: 2 } })' │
│    6    │        'analyze-schema'         │ '1.0.5' │                       'schema(db.coll)'                        │
└─────────┴─────────────────────────────────┴─────────┴────────────────────────────────────────────────────────────────┘


```

## Get Repository Information

Display the homepage and URL for each snippet repository:

```javascript
snippet info

```

The output lists each repository.

```javascript
Snippet repository URL:      https://compass.mongodb.com/mongosh/snippets-index.bson.br
  -->  Homepage:             https://github.com/mongodb-labs/mongosh-snippets

```

## Get Help for a Snippet

Each snippet is unique and has its own interface. The best way to find
information about how a particular snippet works is to view its
`README` file by running `snippet help`:

```javascript
snippet help mongocompat

```

This command displays the `README` file for the [mongocompat](https://github.com/mongodb-labs/mongosh-snippets/tree/main/snippets/mongocompat)
snippet in the `mongosh` console.

```javascript
# mongocompat

Provide `mongo` legacy shell compatibility APIs.

```js
> Array.sum([1, 2, 3])
6
> tojsononeline({a:1,b:2,c:3})
{  "a" : 1,  "b" : 2,  "c" : 3 }
```

```

When you create your own your own snippet
packages, be sure to include a `README.md` file that provides useful
help.
