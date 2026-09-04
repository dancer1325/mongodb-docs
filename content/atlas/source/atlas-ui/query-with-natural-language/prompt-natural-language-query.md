# Prompt a Natural Language Query

You can use Atlas to generate queries using natural language. Atlas uses AI to generate queries based on prompts you provide. Querying with natural language can be a helpful starting point and assist you in learning to write MongoDB queries.

> **Note:**
> When you query your data using natural language in Compass, the text of your prompts and details about your MongoDB schemas are sent to Microsoft and OpenAI for processing. Your data is not stored on any third party storage systems or used to train AI models. This software uses generative artificial intelligence. It is experimental and may give inaccurate results. Your use of this software is subject to MongoDB's:
>
> - [Terms of Use](https://www.mongodb.com/legal/terms-of-use)
> - [Acceptable Use Policy](https://www.mongodb.com/legal/acceptable-use-policy)
> - [Privacy Policy](https://www.mongodb.com/legal/privacy-policy)
>

## About this Task

You can query with natural language to create both queries and aggregations. If your prompt results in an aggregation, you are automatically redirected to the **Aggregations** tab and a pop-up message displays indicating that your prompt requires aggregation stages. You can also provide natural language prompts on the aggregations tab.

## Before you Begin

You must enable natural language querying.

## Steps

The examples on this page use the sample_mflix.movies collection from the Atlas sample dataset.

1. In Atlas, go to the **Data Explorer** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Data Explorer** under the **Database** heading. The [Data Explorer](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Fmetrics%2FreplicaSet%2F%3Creplset%3E%2Fexplorer) displays.

IMPORTANT: You can also go to the **Clusters** page, and click **Data Explorer** under the **Shortcuts** heading.

1. Navigate to the **Natural Language Query Bar**

   1. Select the collection.
   2. Select the **Documents** tab.
   3. Click the **Generate query** button.
   4.

      If you're generating a natural language query for the first time, Atlas displays a modal that states **Use natural language to generate queries and pipelines modal**. To use natural language querying, you must click the **Use Natural Language** button and accept the [MongoDB Acceptable Use Policy](https://www.mongodb.com/legal/acceptable-use-policy) and [Privacy Policy](https://www.mongodb.com/legal/privacy-policy).

      ![Accept the terms and conditions for natural language querying](/images/atlas-ui/natural-language-querying-accept.png)

1. **Type a question about your collection**

   Type a natural language prompt for your collection into the query bar. For example: `Which movies were released in 2000?`

   1. Press enter or click the **Generate query** button.
   2. A filter query populates in the **Filter** bar.

   > **Tip:**
   > It is also possible to paste SQL or queries from application code into the **Natural Language Query Bar**.

1. **Run the query**

   1. Before running the query, make sure to thoroughly review the syntax in the **Filter** bar. Ensure the returned query has the fields and operators that match your desired use case.
   2. Press enter or click the **Find** button to execute the query.

   The results populate in the documents view.

   > **Tip:**
   > You can optionally provide feedback by clicking the thumbs up *[icon: thumbs-up]* or thumbs down *[icon: thumbs-down]* icon by the **Natural Language Query Bar** and provide details on your experience. Your feedback **is not** used to train any AI models.
   >

## Example

Below are examples of prompts to help you understand expected results when using natural language prompts.

| Prompt | Response |
| --- | --- |
| `Which movies have a "PG" rating?` | .. code-block:: json {"rated": "PG"} |
| `Which movies include "David Mamet" in the writers array field?` | .. code-block:: json {"writers": "David Mamet"} |
| `Which movies have a runtime greater than 90?` | .. code-block:: json {"runtime": {$gt: 90}} |

## Next Steps

atlas-ui-prompt-natural-language-agg

## Learn More

- atlas-ui-query-natural-language
- atlas-ui-ai-data-usage
