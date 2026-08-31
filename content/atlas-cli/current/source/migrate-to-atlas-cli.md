# Migrate to the Atlas CLI

> **Note:**
> `mongocli atlas` commands for MongoCLI are now deprecated. Follow the steps on this page to migrate to the Atlas CLI and run Atlas CLI commands instead of `mongocli atlas` commands.

If you have already configured MongoDB CLI, you can migrate your settings to the Atlas CLI easily. Migrating to the Atlas CLI preserves existing MongoDB CLI profiles and saved API (Application Programming Interface) keys. If you haven't already configured MongoDB CLI, you can proceed to install-atlas-cli. Complete the following steps to migrate from the MongoDB CLI to the Atlas CLI:

1. **Install the Atlas CLI.**

   install-atlas-cli. If you have an existing `mongocli.toml` configuration file from the MongoDB CLI, Atlas CLI automatically creates a new configuration file, `config.toml`. The Atlas CLI populates `config.toml` with the profile content and API (Application Programming Interface) keys from `mongocli.toml`.

1. **Update any automation scripts.**

   If you use any automation scripts that rely on `mongocli.toml` or use MongoDB CLI Atlas commands, update the scripts to point to the new configuration file and use Atlas CLI commands instead.

   > **Note:**
   > Atlas CLI supports both MongoDB CLI environment variables and Atlas CLI environment variables, so changing existing environment variables is optional. However, you can use either MongoDB CLI environment variables or Atlas CLI environment variables, but not both together. If you plan to use Atlas CLI environment variables in the future, change your existing MongoDB CLI environment variables.

1. **Choose an authentication method and connect to Atlas.**

   You can authenticate using either `atlas auth login` or `atlas config init`. To learn more, see connect-atlas-cli.

1. **Use the new Atlas CLI commands.**

   Use Atlas CLI commands (starting with `atlas`) wherever you previously used MongoDB CLI Atlas commands (starting with `mongocli atlas`). You can use profiles that Atlas CLI migrates from the MongoCLI by using the --profile flag.
