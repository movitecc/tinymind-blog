---
title: region guide fence设定
date: 2025-04-17T05:58:18.151Z
---

You can define different types of placement constraints in Innovus, such as **fences**, **regions**, and **soft guides** (also known as clusters) to control the placement of instances.

**Fence:**

*   A **fence** is a **hard constraint** that defines a rigid physical outline in the core design area.
*   **Only child instances** belonging to a specific partition or group are allowed to be placed within the boundaries of a fence. Non-child blocks or modules are excluded.
*   When you specify a hierarchical instance as a partition, the constraint type of a module guide is automatically changed to a **fence**.
*   You can **create a fence** for a module or a group of instances (hierarchical, leaf, or other groups) using the `createFence` command. Alternatively, you can select "Fence" from the Attribute Editor's Constraint Type pulldown menu.
*   Instance groups often have a **fence constraint**, meaning standard cells belonging to that group cannot be placed outside, and only cells belonging to that group can be placed inside. This maintains physical-logical coherence. In multi-level designs, all fences can be defined as partitions.
*   The **physical outline of a fence is rigid** and is not moved by the same factors that might move module guides.
*   You can display the **effective utilization (EU)** value for fences, which considers the actual cells, hard macros, blockages, partition cuts, and other floorplan constraints within the fence. It's recommended to update the EU value before running placement.

**Region:**

*   A **region** constraint is similar to a fence, but it is a **softer constraint**.
*   **Instances from other modules can be placed within the physical outline** of a region by the placement engine.
*   A module guide's status changes to a **region** when it is preplaced in the core design area.
*   You can **create a region** using the `createRegion` command or by selecting "Region" from the Attribute Editor.
*   Similar to fences, you can display the **effective utilization (EU)** for regions.

**Soft Guide/Cluster:**

*   A **soft guide**, also referred to as a **cluster**, provides a **stronger grouping** for instances without assigning them to fixed locations.
*   It is **less restrictive** than fences or regions, so some instances within a soft guide might be placed further away if they have strong connections to other modules.
*   You can **create a soft guide** using the `createSoftGuide` command or by selecting "SoftGuide" from the Attribute Editor.
*   The `setPlaceMode -place_global_soft_guide_strength` command can be used to instruct the placement engine to place modules with soft guides closer together. The degree of closeness depends on the strength specified.

**Guide (Module Guide):**

*   The sources primarily mention **module guides** as a preliminary concept. When a hierarchical instance is designated as a partition, its module guide becomes a **fence**. When preplaced in the core area, a module guide becomes a **region**. The sources do not provide a direct command to set a constraint specifically as a "guide" in isolation, suggesting that guides are more of an initial state or a general concept that evolves into the more defined fence or region constraints. Their position is more flexible than fences and regions.

**General Notes:**

*   Module constraints (guide, fence, region, and soft guide) are allowed to be **out of the core area but must be within the die area**.
*   For hierarchical designs, if a fence is logically contained within another fence, it must also be physically placed inside it.
*   You can use the Attribute Editor in the Innovus GUI to set or modify these constraint types.

In summary, to set these constraints, you would typically use the TCL commands `createFence`, `createRegion`, and `createSoftGuide` or interact with the Attribute Editor in the Innovus GUI. Remember that fences provide the strictest containment, followed by regions, while soft guides offer the most flexibility in grouping instances. Module guides are often the initial representation that then gets defined as a more rigid constraint like a fence or a region.