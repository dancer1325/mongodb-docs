# Get Started with Atlas

You can get started with Atlas via the Atlas CLI using a single command: `atlas setup`. This tutorial demonstrates how to use `atlas setup` to:

1. Sign up for an Atlas account.
2. Authenticate with the new Atlas account.
3. Create one free database.
4. Load [sample data](/sample-data/available-sample-datasets/) into your Atlas database.
5. Add your IP address to your project's IP access list.
6. Create a MongoDB user for your Atlas database deployment.
7. Connect to your new database deployment using the MongoDB Shell, `mongosh`.

You can also run `atlas setup` if you have an Atlas account and an organization/project but you haven't set up a cluster.

## Complete The Prerequisites

Before you begin, you must install the Atlas CLI.

## Follow These Steps

Complete the following procedure to get started with Atlas.

1. **Run the `atlas setup` command in the terminal.**

   ```sh
   atlas setup
   ```

   After you run the command, enter Y to open the default browser. A browser window displays the Create Your Account screen. If you want to log into an existing Atlas account, click Log in now and log in. If you're already logged into an existing Atlas account, proceed to step 3.

1. **Sign up and verify your account.**

   Enter your account information and click Sign Up. Follow the prompts to verify your email or register using third-party authentication.

1. **Verify your Atlas CLI session.**

   When you reach the Activation screen, copy the verification code from the Atlas CLI and paste it into the browser. Then, click Confirm Authorization and return to the terminal window.

1. **Accept the default cluster creation settings.**

   After you verify your Atlas CLI session, `atlas setup` creates an `M0` cluster. `M0` clusters have some operational limitations. If you log into an existing account and have existing organizations and projects, `atlas setup` prompts you to select a default organization and default project. Select a default organization and project and press Enter. When the Atlas CLI prompts `Do you want to set up` `your first free database in Atlas with default` `settings? It's free forever!`, enter Y to create your cluster with the default settings. The command creates a sample `M0` shared-tier cluster with the following default settings:

   - Cluster name: `Cluster<number>`
   - Cloud provider and region: `AWS - US_EAST_1`
   - Database Username: `Cluster<number>`
   - Database User Password: `abcdef12345`
   - Load Sample Data: `Yes`
   - Allow connection from IP: `<YourIPAddress>`

   **Input:**

   ```sh
   Do you want to set up your first free database in
   Atlas with default settings? It's free forever! Y
   ```

   **Output:**

   ```sh
   We are deploying Cluster9876543...
   
   Please store your database authentication access details in a secure location.
   Database User Username: Cluster9876543
   Database User Password: abcdef12345
   
   Creating your cluster... [Its safe to 'Ctrl + C']
   ```

## Take the Next Steps

Congratulations! You have successfully set up your Atlas account. Use the [connection string](/reference/connection-string/#connection-string-options) to connect to your cluster through `mongosh` or your application. To view the status of your cluster, run the atlas-clusters command.
