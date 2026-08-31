# Customize the Atlas CLI Output

You can customize the Atlas CLI output fields and format using a Go template or a JSON path, which makes it easier to automate processes based on the output from the Atlas CLI.

## Go templates

You can specify a Go template within any Atlas CLI command or through a separate file. To learn more about Go templates, see [Package template](https://golang.org/pkg/text/template/). To learn the types and properties available for each response, see [Atlas types](https://go.mongodb.org/atlas/mongodbatlas/).

### Syntax

You can specify a template with the command using the `--output` or `-o` option:

```shell
--output|-o go-template="{{<template>}}"
```

Alternatively, you can specify a template through a file using the `--output` or `-o` option:

```shell
--output|-o go-template-file="<path-to-template-file>"
```

### Examples

**Specify Template With the Command**

#### Retrieve the Number of Projects

The following command uses a template to retrieve a count of the number of projects in the specified organization:

```shell
atlas projects ls --orgId 5ab5cedf5g5h5i5j5kl12mn4 -o go-template="Count: {{.TotalCount}}"
```

The preceding command returns the following output:

```shell
Count: 2
```

#### Retrieve Your Atlas Cluster Connection String

The following atlas-clusters-describe command uses the template to retrieve the connection string for an Atlas cluster named `getStarted`. It uses the default profile for accessing Atlas.

```sh
atlas clusters describe getStarted -o go-template="Parse: {{.SrvAddress}}"
```

The previous command returns a string similar to the following:

```sh
Parse: mongodb+srv://getstarted.example.mongodb.net
```

You can use the MongoDB Shell, `mongosh`, to connect to the `getStarted` cluster with the `srvAddress` and the [connection string](/reference/connection-string/#connection-string-options). This example uses the connection string returned by the previous command for a user with the username `User1`.

```
mongo "mongodb+srv://getstarted.example.mongodb.net" --username User1 --password ChangeThisPasswordToSomethingSecure  
```

---

**Specify Template Through a File**

For example, consider the following file named `template.tmpl`:

```shell
Projects: {{range .Results}}{{.ID}} {{end}}
```

The following command uses the `template.tmpl` file to retrieve the IDs of the projects in the specified organization:

```shell
atlas projects ls --orgId 5ab5cedf5g5h5i5j5kl12mn4 -o go-template-file="template.tmpl"
```

The preceding command returns the following output:

```shell
Projects: 5e2211c17a3e5a48f5497de3 5f455b39749bea5fb575dccc
```

---

## `json-path` Output Type

The `json-path` output type limits the results of a command to the fields you specify.

### Usage

When you add the `--output` option to a command, you can specify the type `json-path`. You must provide `json-path` with an expression to evaluate against your results, which means you must be aware of the `json` fields returned by your command.

### Syntax

```shell
<command> --output|-o json-path='$<expression>'
```

`json-path` expressions refer to the JSON (Javascript Object Notation) element that an Atlas CLI command returns. The `$` character represents the root element, which is usually an object or an array. For a list of valid characters and their functions, see [JSONPath expressions](https://goessner.net/articles/JsonPath/index.html#e2).

### Examples
#### Return the description of the first API (Application Programming Interface) key in a list

In the following example, a user retrieves their API (Application Programming Interface) keys with atlas-organizations-apiKeys-list. The `json-path` expression limits the output to the `desc` field of the first key, rather than returning the entire list of keys.

```shell
atlas organizations apikeys list --output json-path='$[0].desc'
> owner_key
```

Running the same command with `--output json` returns the full JSON (Javascript Object Notation) element from the API (Application Programming Interface). It's important to understand the JSON (Javascript Object Notation) structure returned by a command in order to operate on it with `json-path`. Using the the following full JSON (Javascript Object Notation) output for reference, the `--output json-path` with the expression `$[0].desc` finds and returns only the value `"owner_key"`:

```json
[ //``$`` represents the outer array.
  { // ``[0]`` refers to the first element in the array (using a 0-based index).
    "id": "60e736a95d585d2c9ccf2d19",
    "desc": "owner_key", //``.desc`` refers to the ``desc`` field of that element.
    "roles": [
      {
        "orgId": "5d961a949ccf64b4e7f53bac",
        "roleName": "ORG_OWNER"
      }
    ],
    "privateKey": "********-****-****-c4e26334754f",
    "publicKey": "xtfmtguk"
  },
  {
    "id": "d2c9ccf2d1960e736a95d585",
    "desc": "member_key",
    "roles": [
      {
        "orgId": "5d961a949ccf64b4e7f53bac",
        "roleName": "ORG_MEMBER"
      }
    ],
    "privateKey": "********-****-****-c4e26334754f",
    "publicKey": "vfgcttku"
  },
]
```

#### Return the description of a specific API (Application Programming Interface) key in a list

In the following example, a user retrieves their API (Application Programming Interface) keys with atlas-organizations-apiKeys-list. The `json-path` expression limits the output to the `desc` field of the specific JSON (Javascript Object Notation) object with `id` `d2c9ccf2d1960e736a95d585`.

```shell
atlas organizations apikeys list --output json-path='$[? @.id=="d2c9ccf2d1960e736a95d585"].desc'
> member_key
```

#### Return the status of a private endpoint

In the following example, a user retrieves information for a private endpoint with atlas-privateEndpoints-aws-describe. The `json-path` expression limits the output to the `status` field of the root element.

```shell
atlas privateendpoints aws describe 601a4044da900269480a2533 --output json-path='$.status'
> WAITING_FOR_USER
```
