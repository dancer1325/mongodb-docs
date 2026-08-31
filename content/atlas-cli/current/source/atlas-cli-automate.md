# Automate Processes with the Atlas CLI

To automate a process with the Atlas CLI in a script, use the following resources and best practices as guidance. To learn how to connect to the Atlas CLI programmatically, see the Programmatic User tabs on connect-atlas-cli.

## Resources for Automation with the Atlas CLI

| Resource | Objective |
| --- | --- |
| atlas-cli-env-vars | Set environment variables that you can define once and use across all of your scripts. |
| go-template-output | Use Go templates or JSON (Javascript Object Notation) paths to customize the output from the Atlas CLI. You can include the anticipated custom output in your scripts. |

## Best Practices for Automation with the Atlas CLI

Follow these best practices when you automate processes with the Atlas CLI:

### Use Atlas Private Keys

When you create a script to automate processes, we recommend that you use Atlas private keys to access Atlas. Atlas CLI login sessions last for twelve hours, after which you must login again to access Atlas. Use Atlas private keys for continued access to Atlas.

### Base Your Script on the Version of the Atlas CLI that You Run

When you create a script to automate processes, you should base the script on the version of the Atlas CLI that you currently run. **Don't** build automatic upgrades for the Atlas CLI into your script because new Atlas CLI releases could introduce breaking changes, which could break your automated processes. Instead, check release notes for deprecated features and breaking changes before you manually upgrade your version of the Atlas CLI.

### Redirect `stderr`

The Atlas CLI prints error messages and command deprecation warnings in the output for commands. These unanticipated error messages and warnings can cause issues for your automated processes that anticipate a specific output. To prevent issues, you can redirect `stderr` to an output file in your script. For example, the following command redirects the `stderr` output from a script called `myScript.sh` to a text file called `error.txt`:

```
myScript.sh 2> error.txt
```

In the previous example, all error messages and deprecation warnings are stored in `error.txt` and don't display in the output, so they don't disrupt your automated processes. Command deprecation messages are similar to the following text:

```
Command "describe" is deprecated, Please use atlas privateEndpoints aws interfaces describe <atlasPrivateEndpointId> [--privateEndpointId privateEndpointID] [--projectId projected]
```

### Update Scripts Regularly

You should regularly update your scripts to discontinue use of deprecated commands because they will be removed in future releases. You can learn which commands are deprecated from the atlas-cli-changelog. If you set up a redirect file for stderr, you can also check that file for deprecation warnings.

- [Environment Variables](/atlas-cli-env-variables)
- [Customize Output](/custom-output-cli)
