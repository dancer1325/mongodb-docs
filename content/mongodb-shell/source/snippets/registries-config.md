# Registries and Registry Configuration

> **Warning Experimental feature**
>

This page discusses different registries and how to configure your
system to use them.

## Types of Registry Configuration

### Using the MongoDB Registry

This is a public, [community registry](https://github.com/mongodb-labs/mongosh-snippets/tree/main/snippets)
that is maintained by MongoDB.

The community registry is the default registry. It provides several
useful snippets which can help you to get started. The snippets in the
community registry are also [good examples](https://github.com/mongodb-labs/mongosh-snippets/tree/main/snippets)
to use when you are ready to create your own snippets.

MongoDB users are encouraged to contribute to this public registry. To
learn how to share your code with other MongoDB users, see
snip-contribute-a-package.

### Using Private Snippet Registries

You can share code internally using a private registry.

If your snippets reveal proprietary or sensitive information, you can
store them in a private, local registry instead of the public
registry.

To create a private registry, see snip-define-a-registry.

### Using Multiple Registries

A private registry can also be used in conjunction with the
community registry and other private registries. Using multiple
registries allows you to benefit from snippets maintained by MongoDB or
third parties while also maintaining control over code you don't want
to share externally.

To configure multiple registries, see snip-multiple-urls.

## How to Configure a Registry

To use a private registry or multiple registries:

- snip-define-a-registry.
- Create a registry index file.
- Update `snippetIndexSourceURLs` to contain a link to your registry
  index file.
- Update `snippetRegistryURL` to point to your registry host
  (optional).

### Define a New Registry

The [npm public registry](https://registry.npmjs.org/) hosts the
MongoDB snippets community registry. You can use npm to host your own
public or private registry as well.

### Connecting to Registries

You can use a private registry in addition to, or instead of, the
community MongoDB registry.

`snippetIndexSourceURLs` ia a list of URLs. Each URL defines a path
to an index file that contains metadata for the snippets in that
registry.

Configure an additional registry by adding a URL to
`snippetIndexSourceURLs`.

```shell
config.set('snippetIndexSourceURLs', 
  'https://github.com/YOUR_COMPANY/PATH_TO_YOUR_REPOSITORY/index.bson.br;'
  + config.get('snippetIndexSourceURLs')
)

```

Restart `mongosh` for the update to take effect.

> **Important**
>
> If two snippets with the same name appear in multiple registries,
> local system updates will be based on the entry in the first
> registry in the `snippetIndexSourceURLs` list.
>
> Do not reuse snippet names to avoid potential conflicts.
