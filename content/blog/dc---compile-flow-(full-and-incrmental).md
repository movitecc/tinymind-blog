---
title: DC - compile flow (full and incrmental)
date: 2024-11-17T13:18:51.188Z
---

## Full Compile vs. Incremental Compile in Design Compiler:

The sources offer detailed explanations of full and incremental compile techniques within the context of Synopsys Design Compiler.  Here's a breakdown of these compilation flows, highlighting their differences, advantages, and disadvantages:

### Full Compile:

*   **Definition:** During a full compile, Design Compiler performs a complete synthesis and optimization of the entire design from scratch. It maps the design's functionality, described in an HDL like Verilog or VHDL, to a gate-level netlist using cells from the specified technology library.
*   **Process:** 
    *   **Unmaps any previously mapped cells**: This ensures a fresh optimization starting from the design's logical representation.
    *   **No attempt to preserve existing netlist structures**: This allows for a more comprehensive exploration of the design space to achieve optimal results.
    *   **Performs architectural, logic-level, and gate-level optimizations**: These stages ensure the design meets timing, area, and power constraints while adhering to design rules.
*   **Pros:**
    *   **Thorough Optimization:** Explores a wider range of implementation possibilities, potentially leading to better overall quality of results (QoR) in terms of area, timing, and power.
    *   **Suitable for significant design changes:** A full compile is necessary when major modifications are made to the RTL, constraints, or target technology library.
*   **Cons:**
    *   **Longer Runtime:**  Processing the entire design from scratch can be time-consuming, especially for large and complex designs.
    *   **Higher Memory Usage:**  Analyzing and optimizing the entire design requires significant memory resources.

### Incremental Compile:

*   **Definition:**  An incremental compile builds upon a previously compiled design, aiming to improve specific aspects of the design without performing a full resynthesis. 
*   **Process:**
    *   **Uses the existing gate-level netlist as a starting point**: This preserves portions of the design that already meet constraints and focuses optimization efforts on areas requiring improvement.
    *   **Primarily performs gate-level optimizations**: It avoids unmapping the design back to GTECH, focusing on refinements at the gate level.
    *   **Accepts optimizations that enhance area or speed**: It maintains the existing structure where constraints are met, ensuring that improvements directly contribute to better QoR.
*   **Pros:**
    *   **Faster Runtime:**  By targeting specific areas for optimization, it avoids the time-consuming process of recompiling the entire design.
    *   **Lower Memory Consumption:** Working with an existing netlist requires fewer memory resources compared to a full compile.
    *   **Useful for design refinement:**  Effective for fixing timing violations, addressing design rule issues, or making minor modifications after steps like DFT insertion or retiming.
*   **Cons:**
    *   **Limited Scope of Optimization:**  Might not achieve the same level of overall QoR improvement as a full compile, as it works within the constraints of the existing netlist.
    *   **Not suitable for major design changes:** Extensive RTL modifications, significant constraint changes, or technology library updates usually require a full compile.

### Table Comparing Full Compile and Incremental Compile:

| Feature            | Full Compile                                                                                                  | Incremental Compile                                                                                                                                    |
| ------------------ | ----------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Process**         | Complete resynthesis and optimization from scratch                                                           | Optimizes specific areas of a previously compiled design, focusing on gate-level refinements                                                         |
| **Netlist**         | Unmaps existing cells, doesn't preserve existing structure                                                 | Uses existing gate-level netlist as a starting point, preserves well-optimized portions                                                                |
| **Optimization**   | Architectural, Logic, Gate-level                                                                            | Primarily Gate-level                                                                                                                                 |
| **Runtime**        | Longer, especially for large designs                                                                         | Faster, targeted optimization                                                                                                                      |
| **Memory Usage**    | Higher, requires more memory to process the entire design                                                     | Lower, utilizes existing netlist, reducing memory requirements                                                                                      |
| **QoR Potential**  | Potentially higher, explores a wider design space                                                             | Potentially lower, limited by the existing netlist structure                                                                                        |
| **Suitability**     | Major design changes, initial compilation, comprehensive optimization                                         | Design refinement, fixing violations, post-DFT/retiming optimization, minor constraint adjustments, scenarios where runtime and memory are critical |
| **Command Options** | `compile` (DC Expert), `compile_ultra` (DC Ultra/Graphical)                                                 | `compile -incremental_mapping` (DC Expert), `compile_ultra -incremental` (DC Ultra/Graphical)                                                       |

### Key Considerations from the Sources:

*   **Design Compiler supports both full and incremental compilation flows, providing flexibility in optimizing designs.**
*   **Choosing the appropriate compile strategy depends on the design's size, complexity, the extent of modifications, and desired QoR targets.**
*   **Incremental compilation can be particularly valuable in topographical mode, where preserving placement information is crucial.**
*   **The sources also provide insights into techniques for analyzing the results of both full and incremental compiles using log files, QoR reports, and formal verification tools.**

Understanding the nuances of full and incremental compilation techniques is fundamental for efficient design optimization in Design Compiler. The sources offer comprehensive guidance on these flows, their applications, and considerations for achieving desired results in various design scenarios. 
