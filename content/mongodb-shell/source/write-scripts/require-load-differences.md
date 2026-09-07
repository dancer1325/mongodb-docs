# Differences Between require() and load()

The `require()` and `load()` methods include files and modules in
your scripts for added functionality. However, `require()` and
`load()` differ in their behaviors and availability.

## Types of Scripts in mongosh

You can use the following types of scripts with `mongosh`:

- `mongosh` scripts, which can be any of the following:
  - Code entered directly into the REPL.
  - The mongoshrc.js file.
  - Code loaded with the load() method.
- Node.js scripts, which are any scripts loaded with `require()`,
  including npm packages. These scripts are always files.

## Availability of require() and load()

The `require()` and `load()` methods differ in availability
depending on the type of script you are using.

- In `mongosh` scripts, both `require()` and `load()` are available.
- In Node.js scripts, only `require()` is available.

## File Paths for require() and load()

The type of script determines how you specify file paths with
`require()` or `load()`.

- In `mongosh` scripts:
  - `require()` uses the standard
    [Node.js module resolution algorithm](https://nodejs.org/api/modules.html), starting from the current
    working directory of the shell.
  - `load()` takes either:
    - An absolute path, or
    - A relative path. When using a relative path, the path is always
      interpreted as relative to the current working directory of the
      shell.
- In Node.js scripts, `require()` uses the standard
  [Node.js module resolution algorithm](https://nodejs.org/api/modules.html), starting from the file
  where `require()` was called.

### Load External Code in a mongosh Script

You can load external code in a `mongosh` script file, such as an npm
package or a separate `mongosh` script.

- To load a `mongosh` script from another `mongosh` script, use
  the `__dirname` environment variable. The `__dirname` environment
  variable returns the absolute path of the directory containing the
  file being executed.

> **Example**
>
> To load a `mongosh` script named `test-suite.js` from another
> `mongosh` script, add the following line to your script:
>
> ```javascript
load(__dirname + '/test-suite.js')
```

> Using the `_dirname` variable to specify an absolute path ensures
> that the separate script you are loading is not affected by external
> factors such as where `mongosh` started.

- To load a Node.js script from a `mongosh` script, use the
  `require()` method.

> **Example**
>
> To load the [date-fns](https://www.npmjs.com/package/date-fns)
> module from a `mongosh` script called `test-suite2.js`, add the
> following lines to your script:
>
> ```javascript
const localRequire = require('date-fns').createRequire(__filename);  
const fileExports = localRequire('./test-suite2.js'); }
```

## require() Packaging Considerations

## Access to the mongosh API

- `mongosh` scripts can use the `mongosh` API.
- Node.js scripts do not have access to the `mongosh` API.

For example, the `db` global variable (used to display the current
database) is available inside of `mongosh` scripts. It is not
available inside of Node.js scripts.

> **Important**
>
> `mongosh` scripts and Node.js scripts run in different [contexts](https://nodejs.org/api/vm.html#vm_what_does_it_mean_to_contextify_an_object).
> They may exhibit different behaviors when the same command is run in
> each type of script, such as returning different data types.
> Therefore, you may observe unexpected results if you run `mongosh`
> code inside of a Node.js script.
>
> Generally, you should not keep mongosh-specific code inside Node.js
> scripts.
