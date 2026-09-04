# Set Language Specific Rules for String Comparison

Use the [Collation](/reference/collation/) query bar option to specify language-specific rules for string comparison, such as rules for lettercase and accent marks.

> **Important:**
> Flex clusters don't support this feature at this time. To learn more, see flex-cluster-limitations.

## Set Collation

1. In Atlas, go to the **Data Explorer** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Data Explorer** under the **Database** heading. The [Data Explorer](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fmetrics%2FreplicaSet%2F%3Creplset%3E%2Fexplorer) displays.

IMPORTANT: You can also go to the **Clusters** page, and click **Data Explorer** under the **Shortcuts** heading.

1. **Set the collation.**

   1. Select the collection.
   2. In the Query Bar, click **Options**.
   3. Enter the `locale` field in the collation document to specify the [ICU Locale code](http://userguide.icu-project.org/locale) for the desired language in the **Collation** field.

      > **Example:**
      > To use the `pinyin` variant of the Chinese collation, use the following collation document:
      >
      > ```javascript
      > { "locale" : "zh@collation=pinyin" }
      > ```
      >

      As you type, the **Find** button is disabled and the **Collation** label turns red until a valid query is entered.
   4.

      Click **Find** to run the query and view the updated results.

## Clear the Query

To clear the query bar and the results of the query, click **Reset**.

## To Learn More

- See the supported languages and locales section in the [MongoDB Manual](/reference/collation-locales-defaults/#collation-languages-locales).
- See the possible fields in a collation document in the [MongoDB Manual](/reference/collation#collation-document-fields).
