---
title: DC - design rule constraint and optimize constraint
date: 2024-11-17T13:50:47.059Z
---

## Contrasting Design Rule Constraints and Optimization Constraints in Design Compiler

The sources primarily focus on design constraints within the Synopsys Design Compiler environment. They offer a detailed look at the two main types: design rule constraints and optimization constraints.  Here's a breakdown of their differences and usage:

### Design Rule Constraints:

*   **Source**: Design rule constraints are **implicit constraints derived directly from the technology-specific logic library** used for the design.
*   **Purpose**:  They enforce **mandatory electrical and physical limitations** to guarantee the **design's functionality and manufacturability**.
*   **Violation**:  **Design Compiler cannot violate these constraints under any circumstances**, even if it leads to not meeting optimization goals. 
*   **Modification**:  While the **default values from the library cannot be relaxed, designers can set stricter limits** if needed.
*   **Examples**:
    *   **Maximum Transition Time (`set_max_transition`)**:  Limits the rise and fall times of signals on nets, ensuring signal integrity.
    *   **Maximum Fanout (`set_max_fanout`)**: Restricts the number of inputs a single output can drive, preventing signal degradation.
    *   **Maximum Capacitance (`set_max_capacitance`)**: Sets an upper bound on the total capacitance a cell output can drive, maintaining signal quality.
    *   **Minimum Capacitance (`set_min_capacitance`)**: Defines the minimum capacitance required on a net, often crucial for dynamic circuit operation.
    *   **Cell Degradation (`set_cell_degradation`)**:  Accounts for performance degradation of cells due to high input transition times, ensuring accurate timing analysis.
    *   **Connection Class (`set_connection_class`)**: Dictates the compatibility of drivers and loads based on their electrical characteristics, ensuring proper signal transmission.

### Optimization Constraints:

*   **Source**:  Optimization constraints are **explicitly defined by the designer** to guide Design Compiler towards specific performance goals.
*   **Purpose**: They represent **design objectives**, aiming to achieve desired speed, area, and power consumption for the circuit.
*   **Violation**: Unlike design rule constraints, **optimization constraints can be violated** if achieving them conflicts with design rules.
*   **Modification**:  Designers have **complete control over these constraints**, setting target values, weights, and even overriding their priority relative to design rules in specific situations.
*   **Examples**:
    *   **Maximum Delay (`set_max_delay`)**:  Sets the maximum allowed delay for a path or path group, aiming for target clock frequencies.
    *   **Minimum Delay (`set_min_delay`)**:  Specifies the minimum required delay, primarily used to address hold time violations.
    *   **Area (`set_max_area`)**: Defines the maximum allowable area for the design, promoting compact implementations.
    *   **Power (`set_max_dynamic_power`, `set_max_leakage_power`)**:  Sets limits on dynamic and leakage power consumption, aiming for power efficiency.

### Table Highlighting the Differences:

| Feature             | Design Rule Constraints                                                                                                                                                                   | Optimization Constraints                                                                                                                                                                                                  |
| ------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Source**          | Implicitly defined within the logic library.                                                                                                                                                  | Explicitly set by the designer.                                                                                                                                                                                               |
| **Purpose**         | Guarantee functionality and manufacturability based on technology limitations.                                                                                                                | Guide optimization to achieve performance goals related to speed, area, and power.                                                                                                                                          |
| **Violation**       | Cannot be violated, even if optimization goals are not met.                                                                                                                                | Can be violated if meeting them conflicts with design rule constraints.                                                                                                                                                    |
| **Modification**    | Default values from the library cannot be relaxed, but stricter limits can be set.                                                                                                             | Designers have full control; can set target values, weights, and even override default priorities.                                                                                                                           |
| **Examples**        | Max Transition, Max Fanout, Max Capacitance, Min Capacitance, Cell Degradation, Connection Class                                                                                            | Max Delay, Min Delay, Area, Power                                                                                                                                                                                        |
| **Command Prefix** | `set_` (e.g., `set_max_fanout`)                                                                                                                                                              | Varies; often `set_` or `create_` (e.g., `set_max_delay`, `create_clock`)                                                                                                                                                 |
| **Priority**         | Generally higher than optimization constraints, but can be modified in certain situations using the `set_cost_priority` command. See the cost vector in source for the default order. | Lower than design rules by default, but can be prioritized in specific cases by the designer. For example, to prioritize `max_delay` over design rules, use: `prompt> set_cost_priority -delay`.                         |

### Key Points:

*   Understanding the distinction between these constraint types is **fundamental for successful design optimization** in Design Compiler.
*   Design rule constraints act as **non-negotiable boundaries**, ensuring the final circuit adheres to the technology's limitations.
*   Optimization constraints express the designer's intent, **guiding the tool towards achieving desired performance targets** while remaining within the bounds of design rules.

By effectively defining and managing both types of constraints, designers can leverage Design Compiler's optimization capabilities to create circuits that meet both functional and performance requirements.
