# Define and Manage Secrets

A **Secret** is a private value that is stored on the Atlas backend and hidden from users. Secrets are useful for storing sensitive information such as an API key or an internal identifier. You cannot directly read the value of a Secret after defining it. Instead, you link the Secret to another Value, then access the value from a Trigger Function.

## Define a Secret

You can define a new Secret from the UI or using the App Services CLI.

**Atlas UI**

Note that the UI refers to creating a new Value when defining a Secret. This is because a Secret is a special Value type, whose value is hidden after you create it.

1. In Atlas, go to the **Triggers** page for your project.

   1. If it's not already displayed, select the organization that contains your project from the *[icon: office]* **Organizations** menu in the navigation bar.
   2. If it's not already displayed, select your project from the **Projects** menu in the navigation bar.
   3. In the sidebar, click **Triggers** under the **Streaming Data** heading.

   The [Triggers](https://cloud.mongodb.com/go?l=https%3A%2F%2Fcloud.mongodb.com%2Fv2%2F%3Cproject%3E%23%2Ftriggers) page displays.

1. Navigate to the **Values** Page

   1. Click the **Linked App Service: Triggers** link.
   2. In the sidebar, click **Values** under the **Build** heading.
   3. Click **Create a Value**.

1. **Name the Secret Value**

   Enter a name for the Secret. This name is how you refer to the Secret in Functions and must be unique within the project.

   > **Note:**
   > Value names cannot exceed 64 characters and may only contain ASCII letters, numbers, underscores, and hyphens. The first character must be a letter or number.

1. **Define the Secret Value**

   1. Select **Secret** type.
   2. Enter the new Secret's value in the **Add Content** input box. Secret values may not exceed 500 characters.

   > **Warning:**
   > You cannot directly read the value of a Secret after saving it.

1. **Save and Deploy**

   After you've defined the Secret, click **Save**. If deployment drafts are enabled for your application, click **Review & Deploy** to deploy the changes.

---

**App Services CLI**

1. **Authenticate a MongoDB Atlas User**

   Use your MongoDB Atlas Administration API Key to log in to the App Services CLI:

   ```shell
   appservices login --api-key="<API KEY>" --private-api-key="<PRIVATE KEY>"
   ```

1. **Pull Your App's Latest Configuration Files**

   Run the following command to get a local copy of your configuration files:

   ```shell
   appservices pull --remote=<App ID>
   ```

   By default, the command pulls files into the current working directory. You can specify a directory path with the optional `--local` flag.

1. **Create a New Secret**

   Run the following command to define a new Secret:

   ```shell
   appservices secrets create --app=<Your App ID> \
     --name="<Secret Name>" \
     --value="<Secret Value>"
   ```

1. **Deploy Your Changes**

   Run the following command to deploy your changes:

   ```shell
   appservices push
   ```

---

## View Secrets

You can view a list of all Secrets in an app from the UI or using the App Services CLI.

**Atlas UI**

1. From the **Triggers** page, click the **Linked App Service: Triggers** link.
2. In the sidebar, click **Values** under the **Build** heading.

The table lists all Values, including Secrets, and indicates each Value's type in its row.
---

**App Services CLI**

To list the names and IDs of all Secrets, run the following command:

```shell
appservices secrets list --app=<Your App ID>
```

---

## Update a Secret

You can update a Secret from the UI or using the App Services CLI.

**Atlas UI**

To update a Secret from the Atlas UI:

1. From the **Triggers** page, click the **Linked App Service: Triggers** link.
2. In the sidebar, click **Values** under the **Build** heading.
3. Find the Value that you want to update in the table, open its **Actions** menu, and select **Edit Secret**.
4. You can change both the name and value for the Secret.
5. Click **Save** and then, if needed, deploy your changes.
---

**App Services CLI**

To update the value of a Secret using the App Services CLI, run the following command:

```shell
appservices secrets update --app=<Your App ID> \
  --secret="<Secret ID or Name>" \
  --name="<Updated Secret Name>" \
  --value="<Updated Value>"
```

---

## Use a Secret

You *cannot* directly read the value of a Secret after defining it. To use a Secret in a Triggers Function:

1. Create a new Value that links to the Secret.
2. Use the context.values module to access the Secret's value in your Function.

## Delete a Secret

You can delete a Secret from the UI or using the App Services CLI.

**Atlas UI**

To delete a Secret from the Atlas UI:

1. From the **Triggers** page, click the **Linked App Service: Triggers** link.
2. In the sidebar, click **Values** under the **Build** heading.
3. Find the Value that you want to delete in the table, open its **Actions** menu, and select **Delete Secret**.
4. Confirm that you want to delete the Secret.
---

**App Services CLI**

To delete a Secret using the App Services CLI, run the following command:

```shell
appservices secrets delete --app=<Your App ID> --secret=<Secret ID>
```

> **Tip:**
> You can delete multiple Secrets with a single command by specifying their `name` or `id` values as a comma-separated list.
>
> ```bash
> appservices secrets delete --app=<Your App ID> \
> --secret=some-api-key,609af850b78eca4a8db4303f,another-key
> ```
>

---
