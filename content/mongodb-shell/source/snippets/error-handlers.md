# Error Handlers

> **Warning**
>

Error handlers allow you to specify regular expressions to catch
runtime errors and display custom error messages.

To use an error matcher, add a line to your package.json file like the highlighted line in this
excerpt from a snippet in the community [GitHub repository](https://github.com/mongodb-labs/mongosh-snippets/blob/main/snippets/mongocompat/package.json).

```shell
...
"description": "mongo compatibility script for mongosh",
"main": "index.js",
"errorMatchers": "error-matchers.js",
"license": "SSPL",
...

```

For an example of error matching code, see the [mongocompat](https://github.com/mongodb-labs/mongosh-snippets/blob/main/snippets/mongocompat/error-matchers.js)
snippet.
