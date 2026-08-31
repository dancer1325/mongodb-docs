# Troubleshoot Errors

## Troubleshoot Local Atlas Deployment Issues
### Local Machine Issues

If the Atlas CLI slows down to an unusable state after you create multiple local Atlas deployments and load data, you might have reached the limits of your machine. If you're working on Docker Desktop for [Windows](https://docs.docker.com/desktop/settings/windows/#resources) or [MacOS](https://docs.docker.com/desktop/settings/mac/#resources), consider allocating more memory.

### Docker Issues

The Atlas CLI uses [Docker](https://www.docker.com/) for `atlas deployments` commands.

- For MacOS or Windows, install [Docker Desktop v4.31+](https://docs.docker.com/desktop/release-notes/#4310).
- For Linux, install [Docker Engine v27.0+](https://docs.docker.com/engine/release-notes/27.0/).

If your local Atlas deployment doesn't work, you might need to clean up your Docker environment and start fresh:

```
docker stop $(docker ps -a -q) && docker system prune -a
```

### Failed to Install or Update the AtlasCLI Plugin

If the Atlas CLI plugin fails to install or update, make sure that you have access to the [GitHub API](https://docs.github.com/en/rest), as GitHub API access is required to install or update the Atlas CLI plugin.

### Run Diagnostics

If you run into issues and with `atlas deployments` commands and need support, run the following command to provide detailed diagnostics:

```sh
atlas deployments diagnostics <deploymentName> --output json > out.json
```

## Command Errors
### Error: missing credentials

Ensure that you either:

- Run `atlas config init` and add your API keys to your profile or add your API keys to your environment variables. If you created a profile with a custom name or are using more than one profile, specify the correct profile with the `--profile` flag.
- Run `atlas auth login` to authenticate using your Atlas login credentials and an authentication token.

To learn more, see connect-atlas-cli.

### atlas: command not found

The `atlas` executable might be in a directory that isn't in your $PATH. You can either add the directory to your $PATH, move the executable to a directory which is in your $PATH, or run the executable directly from its location.

### 400 (request "TENANT_ATTRIBUTE_READ_ONLY") The attribute pitEnabled is read-only for tenant clusters and cannot be changed by the user.

This error might appear if you try to run atlas-clusters-create with the `--backup` argument for a shared cluster. The `--backup` argument is unavailable for clusters smaller than `M10`.

### 401 (request "Unauthorized") You are not authorized for this resource.

The credentials you provided aren't valid for the project specified in your Atlas CLI command. Check your public and private key strings for accuracy. If your credentials are stored in a configuration file, make sure that the configuration file is in the correct location. To learn more, see config-toml-location. If the Atlas CLI can't find your configuration file and you don't store credentials in environment variables, a `401` error will result.

### 401 (request "Unauthorized") Current user is not authorized to perform this action.

The user account or API key that you used to connect to the Atlas CLI doesn't have permission to perform the requested action. User accounts and API keys must have the appropriate [user roles](/reference/user-roles) to run Atlas CLI commands. To assign or change a user's roles, see:

- [Manage Organization Users](/access/manage-org-users/)
- [Manage Access to a Project](/access/manage-project-access/)
- [API configuration](/configure-api-access)

### 401 (request "Unauthorized") The currently logged in user does not have the group creator role in organization <org-id>.

This error might appear when trying to create a new [project](/tutorial/manage-projects/). The user account or API key that you use to authenticate must have the `Organization Project Creator` role at the [organization level](/tutorial/manage-organizations/) in order to create new projects.

### 403 (request "Forbidden") IP address <ip-address> is not allowed to access this resource.

The user's IP address that you use to authenticate is not on the [access list](/configure-api-access) for the requested project. Add your IP address to the [access list](/configure-api-access) to run commands. To learn more, see the following pages:

- For project access lists, see [Configure IP Access List Entries](/security/ip-access-list/).
- For API key access lists, see [Get Started with the Atlas Administration API](/configure-api-access).

To add your IP address to an API key's access list:

1. **Go to the Access Manager page for your project.**

   1. If it isn't already displayed, select the organization that contains your desired project from the office Organizations menu in the navigation bar.
   2. Select your desired project from the list of projects in the Projects page.
   3. Next to the Projects menu, expand the ellipsis-v Options menu, then click Project Settings.
   4. Click Access Manager in the navigation bar.

1. **Go to the API key's access list.**

   1. Click the API Keys tab.
   2. Click ellipsis to the right of the API (Application Programming Interface) Key.
   3. Click Edit Permissions and click Next.

1. **Add your IP address to the API key's access list.**

   1. Do one of the following tasks in the API Access List section:

      - Click Add Access List Entry and type an IP (Internet Protocol) address.
      - If your current host for accessing Atlas will also make API (Application Programming Interface) requests with this API (Application Programming Interface) key, click Use Current IP Address .
   2. Click Save.

### 404 (request "Not Found") An invalid group ID <group-id> was specified.

The project ID entered with the command does not exist. Check your project ID by navigating to the Settings sub-section of the Project section in the Atlas left-side navigation.

> **Note:**
> `group ID` and `project ID` are synonymous in MongoDB cloud services.

### Alert Config Not Deleted

This error might appear if the Atlas CLI can't delete the alert configuration specified by the ID.

### podman not found

This error appears if you try to run an `atlas deployments` command inside our official docker container, `mongodb/atlas` in v1.26 or later. Instead, follow the steps decribed in atlas-cli-deploy-docker. You should inspect past containers in your cluster with the following command:

```
podman ps -a
```

Then, remove any that start with `mongod` or `mongot` with the following command:

```
podman container rm -f -v <name or ID>
```

> **Tip:**
> You can safely uninstall podman if you're on MacOS. If you installed podman with homebrew, use this command to uninstall:
>
> ```
> brew uninstall podman
> ```
>

## Configuration Errors
### Blank output when reading home directory

This error might appear if the Atlas CLI can't access your home directory.

### HOMEDRIVE, HOMEPATH, or USERPROFILE are blank

This error might appear if the Atlas CLI can't access your home directory.
