# Connect from the Atlas CLI

## Select a Connection Method

When you connect to an existing Atlas account from the Atlas CLI, you can authenticate with one of the following options for the `atlas auth login` command:

*[Contenido incluido desde: /includes/list-table-atlas-cli-auth.rst]*

> **Important:**
> For Atlas CLI versions before 1.47, API (Application Programming Interface) keys are stored in plaintext in the Atlas CLI configuration file. Your API (Application Programming Interface) keys are like passwords. Ensure that you secure the configuration file appropriately. Atlas CLI 1.47+ attempts to store credentials securely.

To create a new Atlas account or onboard an existing account that doesn't have any clusters, see atlas-cli-onboarding. Select a use case below to learn more about the available connection options:

**Non-programmatic Use**

Use the `atlas auth login` command with the `UserAccount` option to authenticate with your Atlas login credentials and a one-time authentication token. The `UserAccount` option requires manual login and verification of an authentication token, which is valid for 12 hours. API (Application Programming Interface) keys are optional when connecting with the `UserAccount` option. After you run `atlas auth login` with the `UserAccount` option, you can:

- Connect with minimum required settings and specify the `--projectId` and `--orgId` flags with each command. This is the quickest way to get started for first-time login.
- Save your connection settings in a profile. Profiles store the project IDs, organization IDs, and, optionally, API (Application Programming Interface) keys to use in future Atlas CLI sessions. To save time, you can specify a profile instead of using the `--projectId` and `--orgId` flags with each command.
---

**Service Account**

When you run the `atlas auth login` command with the `ServiceAccounts` option, the Atlas CLI prompts you to provide your client ID and secret. The `ServiceAccounts` option is good for programmatic use because it allows you to automate and manage MongoDB resources. Service Accounts, also referred to as OAuth applications, allows programmatic access through the specified secure client ID and secret. This method works well for scripting use cases and continuous integration or delivery workflows.

> **Note:**
> This process is interactive. For programmatic authentication, set the `MONGODB_ATLAS_CLIENT_ID` and `MONGODB_ATLAS_CLIENT_SECRET` environment variables before you use the {atlas-cli+}. To learn more about all the supported environment variables, see atlas-cli-env-vars.

---

**API Keys**

You must configure API keys to authenticate with this command. When you run the `atlas auth login` command with the `APIKeys` option, the Atlas CLI prompts you to provide your API (Application Programming Interface) keys and automatically creates a profile that stores the API (Application Programming Interface) keys. The `APIKeys` option is good for programmatic use because it doesn't require manual login or token verification. When you use connect using the `atlas auth login` command with the `APIKeys` option, you can:

- Connect with minimum required settings and specify the `--projectId` and `--orgId` flags with each command. This is the quickest way to get started for first-time login.
- Save additional connection settings in a profile. Profiles store the project IDs, organization IDs, and, optionally, API (Application Programming Interface) keys to use in future Atlas CLI sessions. To save time, you can specify a profile instead of using the `--projectId` and `--orgId` flags with each command.
---

## Connect With Minimum Required Settings

Select a use case and follow the steps to connect from the Atlas CLI with minimum required settings.

### Complete the Prerequisites

**Non-programmatic Use**

- Install the Atlas CLI.
- Add your host's IP address to the IP access list. If you authenticate with your Atlas user credentials and your organization's owners enable [IP access list for the Atlas UI for an organization](/tutorial/manage-organizations/#require-ip-access-list-for-the-atlas-ui), your IP address must be added to the IP access list to run commands in this organization. To learn more, see [Require IP Access List for the Atlas UI](/tutorial/manage-organizations/#require-ip-access-list-for-the-atlas-ui).
---

**Service Account**

Before you start, ensure you have:

- The Atlas CLI installed.
- An active Atlas organization.
- A Service Account created and configured. To learn more, see [Grant Programmatic Access to an Organization](/configure-api-access/#grant-programmatic-access-to-an-organization).
- The client ID and secret of your Service Account.
---

**API Keys**

- Install the Atlas CLI.
- Configure API keys.
---

### Follow These Steps

Select a use case and follow the procedure to quickly connect from the Atlas CLI.

**API Keys**

*[Contenido incluido desde: /includes/steps-atlas-cli-auth-prog-default.rst]*

1. **Select the `APIKeys` option and press `Enter`.**

1. **Enter your API (Application Programming Interface) keys.**

   Enter your public and private keys when prompted.

1. **Accept the default profile options.**

   Accept the remaining default profile options by pressing Enter when the following options display:

   - `Default Org ID`
   - `Default Project ID`
   - `Default Output Format`
   - `Default MongoDB Shell Path`

1. **Issue commands using the `--projectId` and `--orgId` flags.**

   When you run Atlas CLI commands for the duration of your Atlas CLI session, specify your Project ID and Org ID using the `--projectId` and `--orgId` flags.

   > **Example:**
   > ```sh
   > atlas alerts list --projectId 60b3c81153cf986293e2608b
   > ```
   >

---

## Take the Next Steps

Start using the Atlas CLI commands. To save connection settings by modifying the default profile or create a different profile, see atlas-cli-profiles.

- [Save Connection Settings](/atlas-cli-save-connection-settings)
- [Migrate to the Atlas CLI](/migrate-to-atlas-cli)
