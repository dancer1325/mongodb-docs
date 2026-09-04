# Create a Collection with Collation

[Collation](/reference/collation/) allows you to specify language-specific rules for string comparison, such as rules for lettercase and accent marks.

## Limitations

The following restrictions apply when the parameter `numericOrdering` is set to `true`:

- Only contiguous non-negative integer substrings of digits are considered in the comparisons. `numericOrdering` does not support:

   - `+`
   - `-`
   - exponents
- Only Unicode code points in the Number or Decimal Digit (Nd) category are treated as digits.
- If the number length exceeds 254 characters, the excess characters are treated as a separate number.

## Procedure

1. In Atlas, go to the **Data Explorer** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Data Explorer** under the **Database** heading. The [Data Explorer](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fmetrics%2FreplicaSet%2F%3Creplset%3E%2Fexplorer) displays.

IMPORTANT: You can also go to the **Clusters** page, and click **Data Explorer** under the **Shortcuts** heading.

1. Click the **Create Collection** button.

   From the **Collections** screen, click the **Create Collection** button.

1. **Enter the collection name.**

1. Click the **Additional preferences** dropdown.

   Check the **Use Custom Collaton** option.

1. Select a value for **locale**.

   You are required to select a **locale** from the [MongoDB supported languages](/reference/collation-locales-defaults/#supported-languages-and-locales). All other collation options parameters are optional. For descriptions of the fields, see [Collation](/reference/collation/).

1. Click **Create Collection** to create the collection.

## Example

Consider a collection with the following string number and decimal values:

```javascript
[
  { "n": "1" },
  { "n": "2" },
  { "n": "-2.1" },
  { "n": "2.0" },
  { "n": "2.20" },
  { "n": "10"},
  { "n": "20" },
  { "n": "20.1" },
  { "n": "-10" },
  { "n": "3" }
]
```

The following find query uses a collation document containing the `numericOrdering` parameter:

```javascript
 db.c.find(
  { }, { _id: 0 }
 ).sort(
   { n: 1 }
  ).collation( {
   locale: 'en_US',
   numericOrdering: true
} )
```

For more information on querying documents in Atlas, see Query Your Data. The operations returns the following results:

```javascript
[
   { "n": "-2.1" },
   { "n": "-10" },
   { "n": "1" },
   { "n": "2" },
   { "n": "2.0" }
   { "n": "2.20" },
   { "n": "3" },
   { "n": "10" },
   { "n": "20" },
   {"n": "20.1" }
]
```

- `numericOrdering: true` sorts the string values in ascending order as if they were numeric values.
- The two negative values `-2.1` and `-10` are not sorted in the expected sort order because they have unsupported `-` characters.
