# Use an Editor for Commands

The `mongosh` console is line oriented. However, you can
also use an editor to work with multiline functions. There are two
options:

- Use the `edit` command with an external editor.
- Use the `.editor` command, a built-in editor.

## Using an External Editor

The `mongosh` `edit` command works with an external
editor. You can configure an external editor in the shell that runs
`mongosh` or from within `mongosh`.

If editors are configured in both locations, the editor configured
within `mongosh` will take precedence.

To set an editor within `mongosh`, use the
config.set() command.

```javascript
config.set( "editor", "vi" )

```

See setting the external editor
for more examples.

You can use `edit` in three ways:

- Start a new Editing Session
- Edit a Variable
- Edit a Statement

### Start a New Editing Session

Enter `edit` by itself to start a new editing session.

```javascript
edit
  
```

If you start an editing session without any arguments, the editor opens
with the last edit loaded. See the example, mongosh-ex-edit-last.

### Edit a Variable

If an argument exists in the console namespace, you can use `edit` to
update it.

```javascript
var albums = [ ];
edit albums

```

- The variable `albums` is set in the first line.
- The second line opens the external editor to edit the value of
  `albums`.

### Edit a Statement

To edit a statement in the external editor, invoke `edit` with a
statement such as `db.collection.insertMany()`.

```javascript
edit db.digits.insertMany( [] )

```

After editing `db.music.insertMany( [] )` and exiting the external
editor, the `mongosh` console might look like this:

```javascript
prompt> db.digits.insertMany([{ "zero": 0 }, { "one": 1 }, { "two": 2 }])

```

When you exit the external editor, the statement is copied to the
console input line, ready to run. It does not run automatically.
Press `<enter>` to run the statement or `<ctrl> + c` to cancel it.

## Using the Built-in Editor

The `.editor` command provides basic multiline editing capabilities.

The editor does not save code. When you close the built-in editor, your
edits are loaded into the global scope. If your edit calls any
functions or commands, they will run when you close the editor.

To start the built-in editor:

```javascript
.editor

```

Enter `<ctrl> + d` to exit and run your function.

See mongosh-ex-built-in-editor.

## Examples

### Set the External Editor

If the `EDITOR` environment variable is set in the shell running
`mongosh`, the `edit` command will use that editor.

If the `mongosh` `editor` property is also set,
`mongosh` will use that program instead. The `editor`
property overrides the `EDITOR` environment variable.

#### Set the `EDITOR` environment variable

The environment variable should be set before starting
`mongosh`.

Set an environment variable in `bash` or `zsh`:

```shell
export EDITOR=vi

```

The `vi` editor will open when you run `edit` in the
`mongosh` console.

> **Note**
>
> You can also set environment variables from within
> `mongosh` using `process.env.<VARIABLE>`.
>
> Set the EDITOR environment variable from \`mongosh\`:
>
> ```javascript
process.env.EDITOR = 'nano'

```
>
> The environment variable is only updated for the current
> `mongosh`. The update does not persist when
> `mongosh` exits.

#### Set the `editor` Property

To set `nano` as the editor from within `mongosh`, use
the config.set() command.

```javascript
config.set( "editor", "nano" )

```

The `nano` editor will open when you run `edit` in the
`mongosh` console.

> **Note**
>
> `mongosh` will attempt to use whatever program is
> configured. A program like `less` will work. Other programs, such
> as `grep`, may crash or have unexpected results.

### Editing a Command

Use `edit` to start an editing session. If the editor was already
used in the current console session, the editor opens the last edit.

The following statement has a syntax error. The highlighted line is
missing a comma:

```javascript
// WARNING: This code contains an error

db.users.insertMany( [
   { "name": "Joey", "group": "sales" }
   { "name": "Marie", "group": "sales" },
   { "name": "Elton", "group": "accounting" },
   { "name": "Paola", "group": "marketing" }
] )

```

To set up the example:

1.  Copy the example code.
2.  Enter `edit` to start an editing session.
3.  Paste the example code into the editor.
4.  Exit the editor.
5.  Press `enter`.

When you exit the editor, it copies the example code to the command
line. `mongosh` returns an error when the code runs.

To reload the example code, enter `edit` without any arguments.

```javascript
// WARNING: This code contains an error

db.users.insertMany([{
        "name": "Joey",
        "group": "sales"
   } {
        "name": "Marie",
        "group": "sales"
   },
   {
        "name": "Elton",
        "group": "accounting"
   },
   {
        "name": "Paola",
        "group": "marketing"
   }
])

```

The code is reformatted for easier editing. In this case the missing
comma in the highlighted line causes the documents to be misaligned.

### Using Visual Studio as an External Editor

Visual Studio requires a special parameter to work as an external
editor. Use `--wait` with Visual Studio.

Set an environment variable:

```javascript
export EDITOR="/usr/local/bin/code --wait"

```

You can also set the editor with config.set(). If Visual Studio is in your `PATH`, open
`mongosh` and run:

```javascript
config.set("editor", "code --wait")

```

If you use Visual Studio, you can also use the [MongoDB VS Code
Extension](https://www.mongodb.com/products/vs-code).

### Unset the External Editor

Unset the `editor` variable in \`mongosh\`:

```javascript
config.set("editor", null)

```

If the `EDITOR` environment is configured, unset it as well. From
`mongosh`, run:

```javascript
process.env.EDITOR = ''

```

If you unset `EDITOR` using `process.env` the change will not
persist after exiting `mongosh`. To make the change
persistent, unset `EDITOR` from your shell.

### Using the Built-In Editor

Start the editor:

```javascript
.editor

```

`mongosh` enters editor mode. Enter your code:

```javascript
// Entering editor mode (^D to finish, ^C to cancel)
var albums =
    [
       { "artist": "Beatles", "album": "Revolver" },
       { "artist": "The Monkees", "album": "Head"}
    ] 

db.music.insertMany( albums )


```

To leave the editor,

- Press `<ctrl> + d` to exit and run your function
- Press `<ctrl> + c` to exit without running your function

Objects which are declared using `.editor`, like `albums` in this
example, are added to the global scope. They are available after
`.editor` closes.
