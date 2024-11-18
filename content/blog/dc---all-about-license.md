---
title: DC - all about license
date: 2024-11-18T15:05:31.932Z
---

## Enabling License Queuing in Design Compiler

The sources explain how to enable and configure license queuing in Design Compiler. This feature allows your Design Compiler session (`dc_shell` or `dcnxt_shell`) to wait for required licenses to become available instead of immediately failing if a license is currently in use.

### Steps to Enable License Queuing

1. **Set the Environment Variable:**
   * Before starting Design Compiler, set the environment variable `SNPSLMD_QUEUE` to `true`. This enables the license queuing functionality.

2. **Start Design Compiler:**
   * When you invoke Design Compiler, it displays a message confirming that license queuing is enabled.

     ```
     Information: License queuing is enabled. (DCSH-18)
     ```

### Configuring Wait Times for Licenses

To prevent indefinite waits in situations where multiple users require licenses held by each other, you can configure wait times using additional environment variables. You must set `SNPSLMD_QUEUE` to `true` before using these variables.

| Environment Variable   | Description                                                                                                                                                                                                                                |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `SNPS_MAX_WAITTIME`   | Specifies the maximum wait time in seconds for the **first license** required by your Design Compiler session.                                                                                                                      |
| `SNPS_MAX_QUEUETIME`  | Specifies the maximum wait time in seconds for **subsequent licenses** needed within the same Design Compiler session. This variable is used after the first license has been successfully checked out.                             |

### Timeout Message

If a required license is not available within the specified wait time (default is 8 hours), you might see a timeout message like the one below.

```
Information: Timeout while waiting for feature 'DC-Ultra-Features.' (DCSH-17)
```

### License Queuing Status Messages

While your design goes through the synthesis flow, you might encounter status messages related to license queuing, indicating various stages of license acquisition.

```
Information: Started queuing for feature 'HDL-Compiler'. (DCSH-15)
Information: Still waiting for feature 'HDL-Compiler'. (DCSH-16)
Information: Successfully checked out feature 'HDL-Compiler'. (DCSH-14)
```

### Example: Enabling License Queuing

```bash
export SNPSLMD_QUEUE=true
export SNPS_MAX_WAITTIME=3600  # Set maximum wait time for the first license to 1 hour
export SNPS_MAX_QUEUETIME=1800 # Set maximum wait time for subsequent licenses to 30 minutes

# Now start Design Compiler (dc_shell or dcnxt_shell)
```

This configuration will enable license queuing and set specific wait times for license acquisition.

Remember that the specific environment variables and their behavior might differ based on your Synopsys Common Licensing setup and Design Compiler version. Consult your system administrator or refer to the Design Compiler documentation for more information.

---

## Viewing and Releasing Licenses in Design Compiler

The sources detail the commands for listing currently used licenses and releasing those no longer needed.

### Listing Licenses in Use

To view a list of the licenses you currently have checked out within the Design Compiler (`dc_shell` or `dcnxt_shell`) environment, use the `list_licenses` command.

```
prompt> list_licenses
Licenses in use:
    DC-Extension
    Design-Compiler-NXT
    DC-Expert (3)
    DC-Ultra-Features (3)
    DC-Ultra-Opt (3)
    Design-Compiler
    DesignWare
```

The output shows the different license features currently checked out by your session. The number in parentheses indicates the number of licenses checked out for a particular feature if more than one is being used.

### Releasing Licenses

To release a specific license feature that is no longer required, use the `remove_license` command. This command takes the feature name as an argument.

```
prompt> remove_license HDL-Compiler
```

This example would release the `HDL-Compiler` license, making it available for other users or Design Compiler sessions.

**Multicore License Release:**

For features that might require multiple licenses in multicore runs, you can use the `-keep` option with `remove_license`. This allows you to specify how many licenses of that feature should be retained after the command.

```
prompt> remove_license -keep 2 \
                     {DC-Expert DC-Ultra-Opt DC-Ultra-Features}
```

This example would retain two licenses each for `DC-Expert`, `DC-Ultra-Opt`, and `DC-Ultra-Features`, releasing any additional licenses checked out for these features.

**Automatic License Check-in:**

The sources note that Design Compiler automatically checks out the necessary licenses when you invoke the tool and checks them back in when you exit. This ensures efficient license usage across your site.

### Additional Notes:

* The `license_users` command can be used to see which users have specific licenses checked out.
* You can proactively check out specific licenses using the `get_license` command, ensuring their availability when needed.
* For DesignWare components, Design Compiler automatically checks for the required licenses, and you can force the tool to wait for a DesignWare license using the `synlib_wait_for_design_license` variable.

It's important to manage licenses effectively to maximize resource utilization and avoid potential delays in your design flow.
