# Forward Logs to a Service

## Overview

You can configure a log forwarder to automatically store your Trigger and Function server-side logs in a MongoDB collection or send them to an external service. You can also forward logs individually as they're created, or batch logs together to reduce overhead. A log forwarder consists of the following components:

- An **action** that controls how and where logs are forwarded.
- A **filter** that controls which logs are forwarded.
- A **policy** that controls whether logs are forwarded individally or in batches.

### Why Should I Configure Log Forwarding?

Consider setting up a log forwarder if you need to do any of the following actions:

- Store logs for longer than the retention period of 10 days.
- Integrate logs into an external logging service.
- Access logs in MongoDB Search, Online Archive, and Charts.

## Set Up a Log Forwarder

1. **Create a Log Forwarder**

   **Atlas UI**

   To create a log forwarder:

   In Atlas, go to the **Triggers** page.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Triggers** under the **Streaming Data** heading.

   The [Triggers](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Ftriggers) page displays.

   1. Click the **View All Apps** button.
   2. Click into the app that contains the Trigger or Function whose logs you'd like to forward.
   3. Navigate to the **Logs** page.
   4. Select the **Forwarding** tab.
   5. Click the **Create a Log Forwarder** button.
   6. Specify a unique name for your log forwarder.
   ---

   **App Services CLI**

   To create a new log forwarder, add a new configuration file to the `log_forwarders` directory of your app. The file name should match the value in the configuration's `name` field.

   ```json
   {
   "name": "<name>"
   }
   ```

   ---

1. **Choose Which Logs to Forward**

   You can forward all of your Trigger or Function logs or send only a subset to the target collection or service. You control this subset for each log forwarder by defining filters for the log type (e.g. functions, triggers, etc.) and status (i.e. success or error) that invoke the forwarder's action.

   **Atlas UI**

   Choose one or more types of log to forward in the **Log Type** dropdown. Then, choose one or more statuses to forward in the **Log Status** dropdown.
   ---

   **App Services CLI**

   Specify one or more types and one or more statuses for the forwarder to match and forward:

   ```json
   {
   "name": "<name>",
   "log_types": [ "<type>", ... ],
   "log_statuses": [ "<status>", ... ]
   }
   ```

   You can forward the following log types:

   - `auth`
   - `endpoint`
   - `function`
   - `graphql`
   - `push`
   - `schema`
   - `service`
   - `trigger`
   - `trigger_error_handler`

   You can forward the following log statuses:

   - `error`
   - `success`

   ---

   > **Important:**
   > A log forwarder only forwards a given log if both its type *and* status are specified in the filter. For example, consider a forwarder that filters for `triggers` logs with an `error` status. The filter *would* forward the following log:
   >
   > ```json
   > { "type": "triggers", "status": "error", ... }
   > ```
   >
   > The filter *would not* forward the following logs:
   >
   > ```json
   > { "type": "triggers", "status": "success", ... }
   > { "type": "functions", "status": "error", ... }
   > ```
   >

1. **Configure Log Batching**

   You can combine multiple logs into a single batched request to reduce overhead. The batching policy you select determines how a log forwarder groups logs. You can choose between the following batching policies:

   - **No Batching:** The log forwarder forwards logs individually as their corresponding requests occur.
   - **Batching:** The log forwarder groups documents into a batch as they happen. Each batch may include up to 100 log entries. When a batch is full, the log forwarder forwards the entire batch in a single request. The log forwarder forwards logs at least once a minute regardless of the number of logs in the current batch.

   **Atlas UI**

   To configure batching, select either the **No batch** or **Batching** policy.
   ---

   **App Services CLI**

   To configure batching, specify the policy type, either `single` or `batch`, in the `policy` field:

   ```json
   {
   "name": "<name>",
   "log_types": [ "<type>", ... ],
   "log_statuses": [ "<status>", ... ],
   "policy": { "type": "<single|batch>" }
   }
   ```

   ---

1. **Define an Action**

   A log forwarder can automatically store logs in a linked MongoDB collection or call a custom function that sends the logs to an external service.

### Store Logs in a MongoDB Collection

   **Atlas UI**

   To store logs in a collection, select the **To Collection** action and enter the names of the linked cluster, database, and collection that should hold the forwarded logs.
   ---

   **App Services CLI**

   To store logs in a collection, specify an `action` of type `collection` that includes the names of the linked cluster, database, and collection that should hold the forwarded logs.

   ```json
   {
      "name": "<name>",
      "log_types": [ "<type>", ... ],
      "log_statuses": [ "<status>", ... ],
      "policy": { "type": "<single|batch>" },
      "action": {
         "type": "collection",
         "data_source": "<data source name>",
         "database": "<database name>",
         "collection": "<collection name>"
      }
   }
   ```

   ---

### Forward Logs with a Custom Function

   To forward logs to an external service, write a function that accepts an array of log objects and calls the service through an API, SDK, or library.

   > **Note:**
   > Depending on your batching policy and log frequency, the log forwarder may call a log forwarding function with an array of up to 100 log objects.

   The function should have the same signature as the following example:

   ```javascript
   exports = async function(logs) {
   // `logs` is an array of 1-100 log objects
   // Use an API or library to send the logs to another service.
      await context.http.post({
         url: "https://api.example.com/logs",
         body: logs,
         encodeBodyAsJSON: true
      });
   }
   ```

   After you write the log forwarding function, you can assign it to a log forwarder by name.

   **Atlas UI**

   To assign a function to a log forwarder, select the **To Function** action and then select the function from the dropdown input.
   ---

   **App Services CLI**

   To assign a function to a log forwarder, specify an `action` of type `function` that includes the name of the log forwarding function.

   ```json
   {
      "name": "<name>",
      "log_types": [ "<type>", ... ],
      "log_statuses": [ "<status>", ... ],
      "policy": { "type": "<single|batch>" },
      "action": {
         "type": "function",
         "name": "<function name>"
      }
   }
   ```

   ---

1. **Save and Deploy your Changes**

   **Atlas UI**

   After you configure the log forwarder, click **Save**. If you have deployment drafts enabled, make sure to deploy your changes.
   ---

   **App Services CLI**

   After you configure the log forwarder, save the configuration file and then push your updated app configuration:

   ```sh
   appservices push
   ```

   ---

## Restart a Suspended Log Forwarder

A log forwarder might suspend in response to an event that prevents it from continuing, such as a network disruption or a change to the underlying cluster that stores the logs. Once suspended, a forwarder cannot be invoked and does not forward any logs. You can restart a suspended log forwarder from the **Logs > Forwarding** screen of the Atlas UI.

> **Note:**
> If a log forwarder is suspended, an email is sent to the Project Owner to notify them about the issue.
