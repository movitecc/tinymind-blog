---
title: DC - dc_shell and dcnxt_shell
date: 2024-11-18T14:37:54.239Z
---

## Overview of `dc_shell` and `dcnxt_shell`

The Design Compiler tool offers command-line interfaces `dcnxt_shell` and `dc_shell`  for synthesis and timing analysis, alongside the Design Vision GUI. Both are text-only environments where commands are entered at a prompt. DC Expert, a feature requiring a specific license, can be invoked using the `dc_shell` command in UNIX or Linux. To perform synthesis with DC Expert, use the `compile` command.

Here is a table summarizing the key points about `dc_shell` and `dcnxt_shell`:

| Feature                  | `dc_shell`                         | `dcnxt_shell`                       |
|---------------------------|-------------------------------------|--------------------------------------|
| Interface Type           | Command-line                       | Command-line                        |
| Environment              | Text-only                          | Text-only                           |
| Purpose                  | Synthesis and Timing Analysis       | Synthesis and Timing Analysis        |
|  Startup tasks           |  Creates a command log file. Reads and executes `.synopsys_dc.setup` files. Executes scripts specified with `-f` and `-x`. Displays header and prompt. | Creates a command log file. Reads and executes `.synopsys_dc.setup` files. Executes scripts specified with `-f` and `-x`. Displays header and prompt. |
| Command Entry             | Interactive commands or Tcl scripts | Interactive commands or Tcl scripts  |
| Command Language        | Tcl with Design Compiler extensions | Tcl with Design Compiler extensions  |
| Command Autocomplete    | Available using Tab key             | Available using Tab key              |
| Command History          | Available in GUI console           | Available in GUI console            |
| Output Redirection        | Supported using `>` and `>>`      | Supported using `>` and `>>`       |
| Command Interruption     | Supported using Ctrl+C            | Supported using Ctrl+C             |
| Log Files                | ASCII and HTML (configurable)      | ASCII and HTML (configurable)       |
| Script Execution          | Using `dc_shell -f script_file_name` or `source script_file_name` | Using `dcnxt_shell -f script_file_name` or `source script_file_name` |
| Help                     | `help` command for command list, `command_name -help` for options, `man command_or_variable_name` for manual pages | `help` command for command list, `command_name -help` for options, `man command_or_variable_name` for manual pages |
| License Viewing         | Using commands or GUI          | Using commands or GUI           |

**Differences:**

While the provided sources do not explicitly detail the differences between `dc_shell` and `dcnxt_shell`, some distinctions can be inferred:

* **NDM Mode:** `dcnxt_shell` supports NDM (New Data Model) mode, enabling the use of IC Compiler II reference libraries. The `shell_is_in_ndm_mode` command can check the current mode.  `dc_shell` does not appear to support NDM mode.
* **Fusion Libraries:**  `dcnxt_shell` supports the use of fusion libraries.  The sources do not mention `dc_shell` in the context of fusion libraries. 
* **Command Availability:** Some commands might be exclusive to one shell or have different behavior. For example, the source mentions that the `dcnxt_use_icc2_link` command is used in `dcnxt_shell` for specific IC Compiler II Link features.

**Usage:**

Both `dc_shell` and `dcnxt_shell` provide a powerful command-line environment for various synthesis tasks, including:

* **Design Setup:** Defining libraries, setting design environments, and specifying design constraints.
* **Design Analysis and Optimization:** Running compile commands, analyzing timing and power, and resolving design issues.
* **Hierarchical Design:** Working with hierarchical models, block abstractions, and top-level compile.
* **DFT (Design for Test):** Performing test-ready compile and generating test-related files.
* **Physical Guidance:** Using Synopsys Physical Guidance for improved correlation with physical implementation.
* **Floorplan Exploration:** Performing floorplan exploration with IC Compiler or IC Compiler II.

**Note:**

It's important to refer to the official Design Compiler documentation for the most up-to-date and complete information on the differences and usage of `dc_shell` and `dcnxt_shell`, as the provided source excerpts might not cover all aspects. 
