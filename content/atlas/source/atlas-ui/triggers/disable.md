# Disable a Trigger

Triggers may enter a **suspended** state in response to an event that prevents the Trigger's change stream from continuing, such as a network disruption or change to the underlying cluster. When a Trigger enters a suspended state, it is disabled. It does not receive change events and will not fire.

> **Note:**
> In the event of a suspended or failed Trigger, Atlas sends the project owner an email alerting them of the issue.

## Manually Disable a Trigger

You can manually disable an active Trigger from the Atlas UI or by importing an application directory with the App Services CLI.

**Atlas UI**

1.

   In Atlas, go to the **Triggers** page.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Triggers** under the **Streaming Data** heading.

   The [Triggers](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Ftriggers) page displays.

2. From the listed Triggers, find the Trigger that you want to disable.
3. Toggle the **Enabled** setting to disable it, then click **Save**.
---

**App Services CLI**

1. Authenticate a MongoDB Atlas user:

   Use your MongoDB Atlas Administration API Key to log in to the App Services CLI:

   ```shell
   appservices login --api-key="<API KEY>" --private-api-key="<PRIVATE KEY>"
   ```

2. Pull your App's latest configuration files:

   Run the following command to get a local copy of your configuration files:

   ```shell
   appservices pull --remote=<App ID>
   ```

   By default, the command pulls files into the current working directory. You can specify a directory path with the optional `--local` flag.

3. Verify the Trigger configuration file: If you exported a new copy of your application, it should already include an up-to-date configuration file for the suspended Trigger. You can confirm that the configuration file exists by looking in the `/triggers` directory for a Trigger configuration file with the same name as the Trigger.
4. Disable the Trigger: After you have verified that the Trigger configuration file exists, add a field named `"disabled"` with the value `true` to the top level of the Trigger json definition:

   ```json
   {
      "id": "6142146e2f052a39d38e1605",
      "name": "steve",
      "type": "SCHEDULED",
      "config": {
         "schedule": "*/1 * * * *"
      },
      "function_name": "myFunc",
      "disabled": true
   }
   ```

5. Deploy your changes:

   Run the following command to deploy your changes:

   ```shell
   appservices push
   ```

---

## Restoring from a Snapshot

When you restore the database from a snapshot, any Trigger that was disabled or suspended is re-enabled. The Trigger will not fire for events that have already been processed. For more information on restoring from snapshots, see restore-overview. Consider the following scenario:

1. Your database Trigger is disabled or suspended.
2. New documents are added while the Trigger is disabled.
3. You restore the database from a snapshot to a time prior to the new documents being added.
4. Atlas restarts the disabled database Trigger.
5. The restarted Trigger picks up all of the newly-added documents and fires for each document. However, it will *not* fire again for events that have already been processed.

> **Note:**
> If a previously-enabled database Trigger is running during snapshot restoration, you will see an error in the Edit Trigger section of the Atlas UI because the Trigger cannot connect to the Atlas cluster during the restore process. After the snapshot restoration completes, the error disappears and the Trigger continues to execute normally.
