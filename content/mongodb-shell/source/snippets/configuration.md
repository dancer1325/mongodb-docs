# Configuration Options

> **Warning Experimental feature**
>

These options control the interaction between `mongosh`
and the package manager that tracks individual snippet packages. For
more details about how a particular snippet works, see the
documentation for that snippet.

To modify the snippet configuration settings, use the following method:

```javascript
config.set('<OPTION>', '<VALUE>')

```

## Configuration Options

- - Option
  - Type
  - Default
  - Description

- - `snippetAutoload`
  - boolean
  - true
  - Automatically load installed snippets at startup.

- - `snippetIndexSourceURLs`

  - list

  - [MongoDB Repository Index File](https://compass.mongodb.com/mongosh/snippets-index.bson.br)

  - A semi-colon (`;`) separated list of one or more URLs. Each
    URL links to metadata about available snippets. See
    multiple source URLs.

    To disable snippets, unset this value. See an
    example.

\* - `snippetRegistryURL`  
- string
- [MongoDB npm Registry](https://registry.npmjs.org)
- The npm registry that the `mongosh` npm client
  uses to install snippets

Update the configuration options using the
config command, then restart
`mongosh` for the updates to take effect.

## Examples

### Add a Second Registry

Configure a second, private registry for sensitive snippets by adding a
URL to `snippetIndexSourceURLs`.

```shell
config.set('snippetIndexSourceURLs', 
  'https://github.com/YOUR_COMPANY/PATH_TO_YOUR_REGISTRY/index.bson.br;'
  + config.get('snippetIndexSourceURLs')
)

```

Restart `mongosh` for the update to take effect.

### Disable Snippets

The snippets feature requires an index source URL to function. Unset
this value then restart `mongosh` to disable snippets.

```javascript
config.set('snippetIndexSourceURLs', '')

```

Snippets can also be disabled from outside `mongosh`. If
you cannot start `mongosh` because of a corrupt snippets
configuration, disable snippets and restart `mongosh`.

```javascript
mongosh --nodb --eval 'config.set("snippetIndexSourceURLs", "")'

```
