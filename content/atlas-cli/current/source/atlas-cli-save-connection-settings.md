# Save Connection Settings

You can save your frequently-used connection settings as profiles. Profiles store the project IDs, organization IDs, and, optionally, API (Application Programming Interface) keys to use in future Atlas CLI sessions. To save time, you can specify a profile instead of using the `--projectId` and `--orgId` flags with each command. The Atlas CLI stores your profiles in a configuration file called `config.toml`.

> **Note:**
> Any settings stored in environment variables take precedence over settings stored in profiles. Any project or organization specified with the `--projectId` and `--orgId` flags take precedence over both the profile and the environment variables.

## Locate the Configuration File

The Atlas CLI saves the configuration file to the following location depending on your operating system:

****

```
/Users/{username}/Library/Application Support/atlascli
```

---

****

```
%AppData/atlascli
```

---

****

```
$XDG_CONFIG_HOME/atlascli
```

By default, Atlas CLI saves the configuration file in the path defined in the `$XDG_CONFIG_HOME` environment variable. You can modify the path defined in the `$XDG_CONFIG_HOME` variable to your preferred location. To learn more about modifying the `$XDG_CONFIG_HOME` variable, see [XDG Base Directory Specification](https://specifications.freedesktop.org/basedir-spec/basedir-spec-latest.html). If `$XDG_CONFIG_HOME` is not set, the Atlas CLI uses:

```
$HOME/.config/atlascli
```

---

The Atlas CLI grants the user who ran the command read and write access to the file.

## Create a Profile

The first time you run the `atlas auth login` or `atlas config init` command, the Atlas CLI automatically creates the `config.toml` file and a default profile. If you run a command without specifying a profile, environment variables, or `--projectId` and `--orgId` flags, the Atlas CLI uses the default profile for the command.

### Select Your Use Case

Select a connection method based on your use case:

*[Contenido incluido desde: /includes/list-table-atlas-cli-auth.rst]*

To learn more, see Select a Connection Method.

### Complete the Prerequisites

- Install the Atlas CLI.
- Add your host's IP address to the IP access list.
- If you select `atlas config init` as your connection method, you must Configure API keys.
- If your Atlas CLI installation is behind a firewall and you want to use a proxy URL (Uniform Resource Locator), set up the `HTTP_PROXY` or `HTTPS_PROXY` environment variable.

   > **Important:**
   > Atlas CLI supports `http`, `https`, and `socks5` schemes. You must specify `cloud.mongodb.com/` as the main target URL in the proxy service's access list. You must also specify the username and password if your proxy configuration enables authentication. To learn more, see [Proxy server](https://en.wikipedia.org/wiki/Proxy_server).

### Follow These Steps

Select a use case and follow the procedure to create a profile.

**Non-programmatic Use**

*[Contenido incluido desde: /includes/fact-default-vs-named-profile.rst]*

**Default Profile**

Follow these steps to create the default profile. If the default profile already exists, these commands update the default profile's values.

*[Contenido incluido desde: /includes/steps-atlas-cli-auth-nonprog-default.rst]*

*[Contenido incluido desde: /includes/steps-atlas-cli-add-profile-nonprogrammatic.rst]*

---

**Named Profile**

Follow these steps to create a profile with a custom name.

1. **Run the authentication command.**

   Run the `atlas auth login` command in your terminal with the `profile <profileName>` flag. `<profileName>` should be the desired name for your new profile.

   ```sh
   atlas auth login --profile myProfile
   ```

   The command opens a browser window and returns a one-time activation code. This code expires after 10 minutes.

*[Contenido incluido desde: /includes/steps-atlas-cli-add-profile-nonprogrammatic.rst]*

---

---

**Programmatic Use**

*[Contenido incluido desde: /includes/fact-default-vs-named-profile.rst]*

**Default Profile**

Follow these steps to create the default profile. If the default profile already exists, these commands update the default profile's values.

*[Contenido incluido desde: /includes/steps-atlas-cli-auth-prog-default.rst]*

*[Contenido incluido desde: /includes/steps-atlas-cli-add-profile-programmatic.rst]*

---

**Named Profile**

Follow these steps to create a profile with a custom name.

1. **Run the authentication command.**

   Run the `atlas config init` command in your terminal with the `profile <profileName>` flag. `<profileName>` should be the desired name for your new profile.

   ```sh
   atlas config init --profile myProfile
   ```

*[Contenido incluido desde: /includes/steps-atlas-cli-add-profile-programmatic.rst]*

---

---

## Update a Profile

You can update the settings stored in your configuration file in the following ways:

- Edit the `config.toml` file with a text editor.
- Run the `atlas config set` command for a setting. This edits an individual value in the `config.toml` file.

## Run a Command with a Profile

To run an Atlas CLI command using a profile:

- atlas-cli-set-profile.
- Append the `--profile <profileName>` flag to a command or omit the `--profile <profileName>` flag to use the default profile.

> **Example:**
> This command uses a profile named `myProfile`:
>
> ```
> atlas <command> --profile myProfile
> ```
>
> This command uses the default profile:
>
> ```
> atlas <command>
> ```
>
