---
title: remove region guide fence设定
date: 2025-04-17T05:58:56.198Z
---

Based on the sources, there isn't a direct TCL command explicitly listed to "remove" a region, guide, or fence setting after it has been created. However, here are some relevant points from the sources and potential approaches:

*   When you designate a hierarchical instance as a partition, the constraint type of a module guide automatically changes to a **fence**. If you were to unpartition it, the constraint might revert or become modifiable. The sources mention commands related to partitions such as `partition` and `Unpartition` but don't detail if this directly removes fence constraints.
*   A module guide's status changes to a **region** when it is preplaced in the core design area. If you were to unplace the module, it might revert from a region.
*   The `trimDesign` command **discards fences, regions, guides, instance groups, and power domains**. This means if you trim a portion of your design, these constraints within the trimmed area will not be preserved in the new, independent design. However, this is not a direct way to remove the constraint from the original design.
*   The sources mention using the **Attribute Editor** in the Innovus GUI to select "Fence," "Region," or "SoftGuide" as the Constraint Type. It's plausible that you could use the Attribute Editor to change the constraint type to a different setting or potentially remove it, although the sources don't explicitly state this.
*   For black boxes, the `unspecifyBlackBox` command is mentioned. If a fence was created by converting a black box, this command might be relevant.
*   The sources describe how to **create** fences (`createFence`), regions (`createRegion`), and soft guides (`createSoftGuide`). It is possible that to remove a constraint, you might need to **overwrite** it by setting a different constraint type or by redefining the floorplan object.

While the sources don't offer a specific "removeFence," "removeRegion," or "removeGuide" TCL command, you should consider exploring the following in Innovus:

1.  **Using the Attribute Editor in the GUI:** Select the floorplan object (module or group) that has the fence, region, or guide and check the Attribute Editor for options to modify or remove the "Constraint Type."
2.  **Investigating partition-related commands:** If the constraint is associated with a partition, commands like `Unpartition` might affect the constraints, although this is not explicitly stated.
3.  **Overwriting the constraint:** Try using `createFence`, `createRegion`, or `createSoftGuide` with the same object but potentially with different parameters or no constraint type specified (if that is a valid option). Consult the full command syntax using `help createFence`, etc.
4.  **Examining floorplan editing commands:** There might be general floorplan editing commands that allow modification of object properties, including constraints.

Without a direct removal command in the provided sources, the most likely methods would involve GUI interaction or potentially overwriting the existing constraint.