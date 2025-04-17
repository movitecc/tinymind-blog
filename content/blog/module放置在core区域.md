---
title: module放置在core区域
date: 2025-04-17T02:57:43.514Z
---

You can place a module within the core area in Innovus using several methods, as described in the sources:

*   **Initial Placement during Design Import**: When you import your design, the initial size of each module is calculated. By default, the Innovus software uses a "footprintless" flow, so it determines cell equivalency based on function rather than footprint definitions.

*   **Using Module Constraints**: You can control the placement of modules within the core area using different types of module constraints:
    *   **Guide**: You can preplace a module in the core design area using a module guide. This guide represents the logical structure and helps guide the placement of the module's cells in its vicinity. When preplaced, a module guide typically snaps to a standard cell row vertically and a metal 2 pitch horizontally, though this default can be changed. Module guides initially appear on the left side of the core area as pink objects.
    *   **Fence**: This is a hard constraint, and the module's physical outline is rigid. The design for the module is self-contained within this outline. When a hierarchical instance becomes a partition, its module guide automatically changes to a fence.
    *   **Region**: You can create a region for a module using the `createRegion` command or the Attribute Editor. However, be aware that region groups can potentially cause overlaps.
    *   **Soft Guide/Cluster**: Similar to a guide, but without a fixed location. This provides stronger grouping for instances within the module. You can create a soft guide using the `createSoftGuide` command or the Attribute Editor. You can influence the closeness of placement for soft guides using `setPlaceMode -place_global_soft_guide_strength`.

*   **Manual Movement or `setObjFPlanBox`**: You can manually move a module within the core area in the GUI. Alternatively, you can use the `setObjFPlanBox` command to define a new boundary for the module with specific coordinates within the core area. Note that for a module to become a partition (except for blackboxes), it needs to be inside the core area.

*   **Preplacing I/Os and Macros**: While not directly placing a general module, you will typically preplace I/Os and hard macros, which are also modules, based on connectivity and other requirements before placing standard cells.

*   **Integrated Hierarchical Database (iHDB) Flow**: If you are using the iHDB flow, placing a module within the core would involve restoring the top-level design and the specific block's model (if it's a block) using commands like `restore_module_model` and ensuring these modules are positioned correctly during floorplanning.

*   **Blackboxes**: If the module is a blackbox (a placeholder for a design with undefined internal content), you can specify it as a blackbox using the `specifyBlackBox` command after importing the design. Blackboxes, being special partitions, do not have the same restriction of needing to be fully inside the core area during the initial specification. You would then typically define a LEF abstract for the blackbox.

*   **Concurrent Macro Placement**: If the module you want to place is a macro, you can use the `place_design -concurrent_macros` command, which places macros and standard cells at the same time and legalizes the macros.

*   **Module Padding**: While not directly placing a module, you can add padding within hierarchical modules using `setPlaceMode -place_global_module_padding module factor`. This spreads out the standard cell instances within the module, effectively influencing its occupied area within the core.

Remember that after importing the design, you typically proceed with floorplanning, where you define the core area and the initial placement of modules. The connectivity between modules is a high-priority constraint that influences their final placement.