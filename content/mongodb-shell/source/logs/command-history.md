# View Shell Command History

The MongoDB Shell saves a history of all commands you've run across
sessions. When a new command is issued, it is added to the beginning of
the history file.

## Steps

To view the MongoDB Shell command history, open the following file in a
text editor:

- - Operating System
  - Path to History File
- - macOS and Linux
  - `~/.mongodb/mongosh/mongosh_repl_history`

\* - Windows  
- `%UserProfile%/.mongodb/mongosh/mongosh_repl_history`

Starting in MongoDB Shell 2.4.0, you can use the `history()` command to
view a list of previously executed commands. For example:

```javascript
history()

```

The following example output shows a list of commands in an array of
strings:

```javascript
[
  'db.pizzaOrders.explain()',
  'db.pizzaOrders.find()',
  'db.pizzaOrders.explain.find()',
  'db.pizzaOrders.explain.find( {} )',
  'db.pizzaOrders.explain().find( {} )',
  'db.pizzaOrders.explain().find( orderDate, totalNumber )',
  'db.pizzaOrders.explain().find( orderDate: Date( "2024-03-20T10:01:12Z" ), totalNumber: 20 )',
  'db.pizzaOrders.explain().find( totalNumber: 20 )',
  'db.pizzaOrders.explain().find( { orderDate: Date( "2024-03-20T10:01:12Z" ) }, { totalNumber: 20 } )',
  'db.pizzaOrders.find( { orderDate: Date( "2024-03-20T10:01:12Z" ) }, { totalNumber: 20 } )',
  ...
]

```

The history is returned in chronological order.

`history()` supports JavaScript array methods. You can use `slice()`
to return a section of an array. For example, to view the last 10
commands, run:

```javascript
history().slice(-10)
```
