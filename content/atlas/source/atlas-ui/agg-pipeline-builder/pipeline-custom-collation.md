# Specify Custom Collation For Your Pipeline

Use custom collation to specify language-specific rules for string comparison, such as rules for letter case and accent marks, within your aggregation pipeline.

## About this Task

When entering a collation document, the `locale` field is mandatory. Default collation field values vary depending on which locale you specify. To learn more about supported languages and locales, see [Collation Locales and Default Parameters](/reference/collation-locales-defaults).

## Steps

1. In Atlas, go to the **Data Explorer** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Data Explorer** under the **Database** heading. The [Data Explorer](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fmetrics%2FreplicaSet%2F%3Creplset%3E%2Fexplorer) displays.

IMPORTANT: You can also go to the **Clusters** page, and click **Data Explorer** under the **Shortcuts** heading.

1. Click **Options**

   1. Select the collection.
   2. In the top-right corner of the pipeline builder, click **Options**.

   ![More Options](/images/atlas-ui/compass/agg-builder-click-more-options.png)

1. **Enter your collation document**

   Next to the **Collation** field, enter your [collation document](/reference/collation/). After you enter your collation document, the aggregation pipeline builder considers the language-specific rules that you specified in your document.

## Example

The following sample collation document specifies French as the chosen `locale` and sorts uppercase letters before lowercase letters with the `caseFirst` field:

```javascript
{
  locale: "fr", 
  caseFirst: "upper"
} 
```

## Learn More

- [Collation](/reference/collation/)
- [Collation Locales and Default Parameters](/reference/collation-locales-defaults)
