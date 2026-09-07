# Create and Share Snippets

> **Warning Experimental feature**
>

You can write scripts to manipulate
data or carry out administrative tasks in `mongosh`.
Packaging a script as a snippet provides a way to easily share scripts
within your organization or across the MongoDB user community.

This page discusses:

- Preparing a snippet package.
- Publishing the snippet package to a registry.

For examples of scripts and the metadata files in snippet packages,
see the snippets in the [community snippet registry](https://github.com/mongodb-labs/mongosh-snippets) on GitHub.

> **Tip**
>
> If you plan to submit your snippet to the [community registry](https://github.com/mongodb-labs/mongosh-snippets), be sure
> to review the information in snip-contribute-a-package.

## Create a Snippet Package

The steps in this section focus on packaging a script. For more details
on writing scripts see mdb-shell-write-scripts.

### Prepare the Files

### Prepare the `package.json` File

`package.json` contains metadata that the package registry uses to
manage snippets.

A minimal `package.json` file looks like this:

```typescript
{
    "name": "@mongosh/snippet-resumetoken",
    "snippetName": "resumetoken",
    "version": "1.0.2",
    "description": "Resume token decoder script",
    "main": "index.js",
    "license": "Apache-2.0",
    "publishConfig": {
       "access": "public"
    }
}

```

The parameters are:

- - Field
  - Description
- - "name"
  - The npm package that contains the snippet.
- - "snippetName"
  - The snippet name. This is the name used with commands like `install`.
- - "version"
  - The package version. This should be incremented when you update
    your snippet.
- - "description"
  - A brief note about what your snippet does. Caution, if the
    description is more than 50 or 60 characters long it may cause
    display problems with some
    snippet commands.
- - "main"
  - This is the starting point for your code, `index.js`. Note
    that functions in other files can be scoped so that they are
    also available in the the `mongosh` shell.
- - "license"
  - The license for users of your code. If you want to contribute to
    the shared registry, the license should be from the
    [SPDX license list](https://www.npmjs.com/package/spdx). See
    also the
    [MongoDB Contributor Agreement](https://www.mongodb.com/legal/contributor-agreement).

\* - "publishConfig"  
- This value is used to control access to your snippet package.
  `public` is typical, but npm provides
  [other options](https://docs.npmjs.com/cli/v7/using-npm/config)
  as well.

Use this code to create a skeleton `package.json` file. Edit the file
and replace each `UPDATE` to insert the values for your snippet
package.

```javascript
{
    "name": "@UPDATE/UPDATE",
    "snippetName": "UPDATE",
    "version": "UPDATE",
    "description": "UPDATE",
    "main": "UPDATE",
    "license": "UPDATE",
    "publishConfig": {
       "access": "UPDATE"
    }
}


```

There are several examples of `package.json` files in the MongoDB
GitHub [repository](https://github.com/mongodb-labs/mongosh-snippets).

> **Tip**
>
> MongoDB uses npm as a package registry.
>
> npm relies on the `package.json` file to manage packages. Refer to
> the [npm package documentation](https://docs.npmjs.com/cli/v7/configuring-npm/package-json) for
> more information about `package.json`.

## Publish a Snippet

To share your snippet, you must publish you snippet package to a
registry. The package will contain:

- Your code
- `README.md`
- `LICENSE` file
- package.json

When the files are complete, follow these steps to create and publish
your snippet package.

## Install the New Snippet Package

Follow these steps to install your new snippet package:

## Contribute a Snippet Package to the MongoDB Community

If you have written a code snippet that might be
useful for other MongoDB users, you are invited to contribute it to the
[community repository](https://github.com/mongodb-labs/mongosh-snippets) hosted on GitHub.

To submit a snippet to the shared MongoDB repository:

MongoDB will review your pull request. If it is accepted, we will:

- Merge your code into our GitHub repository.
- Publish it to the npm registry.
- Add it to the snippet index.
