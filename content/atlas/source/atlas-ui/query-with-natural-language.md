# Query with Natural Language

You can use Atlas to ask natural language questions about your data. Atlas uses AI to generate filter queries and aggregations based on the prompts you provide.

## Use Cases

You may want to use natural language to query in Atlas to:

- Ask plain text questions about your data.
- Create an initial query or aggregation pipeline that you can modify to suit your requirements.
- Learn how to write complex queries with multiple aggregation stages.

## Behavior

- Natural language querying utilizes [Azure Open AI](https://azure.microsoft.com/en-us/products/ai-services/openai-service) as its current provider. This provider may be subject to change in the future.
- The Atlas natural language querying feature is on a rolling release schedule. As a result, some users may temporarily have functionality that other users do not.

## Get Started

- atlas-ui-enable-natural-language-querying
- atlas-ui-prompt-natural-language-query
- atlas-ui-prompt-natural-language-agg

## Details

When you query your data using natural language in Atlas, the text of your prompts and details about your MongoDB schemas are sent to Microsoft and OpenAI for processing. Your data is not stored on any third party storage systems or used to train AI models. This software uses generative artificial intelligence. It is experimental and may give inaccurate results. Your use of this software is subject to MongoDB's:

- [Terms of Use](https://www.mongodb.com/legal/terms-of-use)
- [Acceptable Use Policy](https://www.mongodb.com/legal/acceptable-use-policy)
- [Privacy Policy](https://www.mongodb.com/legal/privacy-policy)

- [Enable](/atlas-ui/query-with-natural-language/enable-natural-language-querying)
- [Prompt Query](/atlas-ui/query-with-natural-language/prompt-natural-language-query)
- [Prompt Aggregation](/atlas-ui/query-with-natural-language/prompt-natural-language-aggregation)
- [Intelligent Assistant in Data Explorer](/atlas-ui/query-with-natural-language/data-explorer-ai-assistant)
