---
title: DC - wireload mode, topographical mode, multimode and upf mode
date: 2024-11-18T14:49:55.801Z
---

## Design Compiler Compile Modes

The sources primarily focus on **wire load mode** and **topographical mode**. These modes determine how Design Compiler estimates delays and performs optimizations during the compilation process. **Multimode** and **UPF mode** are not considered distinct tool modes but rather functionalities available within **topographical mode**.

Here's a table comparing the different Design Compiler compile modes:

| Feature              | Wire Load Mode (Default)              | Topographical Mode                   | Multimode                                                                                                                     | UPF Mode                                                                                                                                                                                |
|-----------------------|----------------------------------------|--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Description           | Uses wire load models for delay estimation. | Uses physical layout information for delay estimation. | Allows analysis and optimization across multiple operating modes (e.g., test, standby).                                                | Enables the use of Unified Power Format (UPF) commands for specifying low-power design intent in multivoltage designs.                                                                   |
| Accuracy             | Less accurate, relies on estimations.       | More accurate, considers physical placement. | Depends on the defined modes and their associated constraints.                                                                     | Accuracy depends on the accuracy of the UPF specifications and the tools used for power analysis and optimization.                                                                           |
| Memory Requirements   | Lower                                     | Higher                                    | Higher due to data for multiple modes.                                                                                           | Depends on the complexity of the UPF specifications and the design.                                                                                                                            |
| Runtime              | Faster                                     | Slower                                    | Longer due to analysis and optimization across multiple modes.                                                                     | Depends on the complexity of the UPF specifications and the power optimization tasks.                                                                                                     |
| Applications          | * Early design stages <br> * Designs where physical information is unavailable <br> * Faster turnaround time | * Later design stages <br> * Designs with available floorplan or placement data <br> * Improved correlation with physical implementation | * Designs operating in multiple modes <br> * Concurrent optimization for different operating conditions | * Multivoltage designs <br> * Low-power design methodologies <br> * Power-aware synthesis and optimization                                                                                      |
| Example Commands      | * `set_wire_load_model` <br> * `set_wire_load_mode` | * `extract_physical_constraints` <br> * `read_floorplan` | * `create_scenario` <br> * `set_active_scenarios`                                                                            | * UPF commands (see *Power Compiler User Guide*)                                                                                                                                        |

### Wire Load Mode

* **Default mode** when invoking Design Compiler with `dc_shell` or `dcnxt_shell`.
* Uses **wire load models** to estimate wire delays based on fanout and design size.
* Offers various compile commands, including `compile` for DC Expert and `compile_ultra` for DC Ultra and DC Graphical.
* Suitable for **early design stages** when physical information is unavailable.

### Topographical Mode

* Requires explicit selection when starting Design Compiler.
* Uses **physical layout information** (e.g., floorplan, placement) for more accurate delay calculation.
* Supports **multimode** and **UPF mode** functionalities.
* Enables **incremental compilation** for post-placement optimizations and design fixes.
* Offers better **correlation** with the final physical implementation.
* Commands like `extract_physical_constraints` and `read_floorplan` are used to import physical data.
* Suitable for **later design stages** when floorplan or placement information is available.

### Multimode

* Allows designs to operate under **multiple modes** (e.g., test, standby).
* Supported by **Design Compiler NXT** and **Design Compiler Graphical**.
* Enables **concurrent analysis and optimization** across multiple modes.
* Scenarios are defined using the `create_scenario` command.
* Active scenarios for optimization are set with the `set_active_scenarios` command.
* Useful for optimizing designs with **different operating conditions**.

### UPF Mode

* Uses **IEEE 1801 (Unified Power Format)** commands to specify low-power design intent.
* Requires a **Power Compiler** license.
* Supported within **topographical mode**.
* Allows specification of power domains, power switching styles, and power management cells.
* The sources recommend referring to the *Power Compiler User Guide* for details.
* Useful for **multivoltage designs** and advanced low-power methodologies.

### Usage Cases

* **Early design stages:** Use **wire load mode** for quick synthesis and exploration.
* **Later design stages:** Switch to **topographical mode** for accurate delay estimation based on physical information.
* **Designs with multiple operating modes:** Utilize **multimode** in **topographical mode** to optimize for different operating conditions.
* **Multivoltage designs:** Employ **UPF mode** in **topographical mode** to specify and implement low-power design strategies.
* **Incremental optimization:** Leverage **topographical mode's** incremental compile capability for post-placement refinements and design fixes.

Remember that this information is based on the provided sources, and the specific capabilities and usage of these modes might vary depending on the Design Compiler version and license. Always refer to the official documentation for the most up-to-date and comprehensive information.
