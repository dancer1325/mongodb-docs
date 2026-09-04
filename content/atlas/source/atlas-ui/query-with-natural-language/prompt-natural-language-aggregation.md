# Prompt a Natural Language Aggregation

You can use Atlas to generate aggregation queries using natural language. Atlas uses AI to generate aggregations based on prompts you provide. Querying with natural language can be a helpful starting point and assist you in learning to write MongoDB queries.

> **Note:**
> When you query your data using natural language in Compass, the text of your prompts and details about your MongoDB schemas are sent to Microsoft and OpenAI for processing. Your data is not stored on any third party storage systems or used to train AI models. This software uses generative artificial intelligence. It is experimental and may give inaccurate results. Your use of this software is subject to MongoDB's:
>
> - [Terms of Use](https://www.mongodb.com/legal/terms-of-use)
> - [Acceptable Use Policy](https://www.mongodb.com/legal/acceptable-use-policy)
> - [Privacy Policy](https://www.mongodb.com/legal/privacy-policy)
>

## About this Task

You can also provide natural language prompts on the documents tab.

## Before you Begin

You must enable natural language querying.

## Steps

The examples on this page use the sample_mflix.movies collection from the Atlas sample dataset.

1. Navigate to the **Natural Language Query Bar**

   1. Select the **Aggregations** tab.
   2. Click the **Generate aggregation** button.
   3.

      If you're generating a natural language query for the first time, Atlas displays a modal that states **Use natural language to generate queries and pipelines modal**. To use natural language querying, you must click the **Use Natural Language** button and accept the [MongoDB Acceptable Use Policy](https://www.mongodb.com/legal/acceptable-use-policy) and [Privacy Policy](https://www.mongodb.com/legal/privacy-policy).

      ![Accept the terms and conditions for natural language querying](/images/atlas-ui/natural-language-querying-accept.png)

1. **Type a question about your collection**

   Type a natural language prompt for your collection into the query bar. Aggregation pipeline prompts usually have an aggregation verb such as count, average, or sum with logical conditions. For example: `How many movies have more than 3 writers in the writers array?`

   1. Press enter or click the **Generate aggregation** button.
   2. An aggregation pipeline populates in the **Pipeline** bar. You can scroll down to see the syntax of each stage.

1. **Run the aggregation**

   1. Before running the query, make sure to thoroughly review the syntax of each stage. Ensure the returned pipeline has the fields and stages that match your desired use case.

      > **Warning:**
      > Some aggregation operators, like `$merge` and `$out`, can modify your collection's data. If your aggregation pipeline contains operators that can modify your collection's data, you are prompted for confirmation before the pipeline is executed.
      >

   2. Click the **Run** button to execute the pipeline.

   The results populate in the aggregations view.

   > **Tip:**
   > You can optionally provide feedback by clicking the thumbs up *[icon: thumbs-up]* or thumbs down *[icon: thumbs-down]* icon by the **Natural Language Query Bar** and provide details on your experience. Your feedback **is not** used to train any AI models.
   >

## Examples

Below are examples of prompts to help you understand expected results when using natural language prompts for aggregation.

| Prompt | Response |
| --- | --- |
| `Count the movies that have a release year of 1999` |  [ { $match: { year: 1999 }, }, { $count: "total_movies", } ] |
| `Which comedy genre movie has the most awards?` |  [ { $match: { genres: "Comedy" } }, { $sort: { "awards.wins": -1, "awards.nominations": -1 } }, { $limit: 1 }, { $project: { _id: 0, title: 1, "awards.wins": 1, "awards.nominations": 1 } } ] |
| `How many movies have a imdb.rating > 4?` |  [ { $match: { "imdb.rating": { $gt: 4 } } }, { $group: { _id: null, count: { $sum: 1 } } } ] |

## Next Steps

atlas-ui-prompt-natural-language-query

## Learn More

- atlas-ui-query-natural-language
- atlas-ui-ai-data-usage
