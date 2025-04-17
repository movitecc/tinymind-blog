---
title: power grid概念
date: 2025-04-17T02:43:30.293Z
---

Based on the sources, the **power grid (PG)** is a crucial aspect of the design implementation process in Innovus, responsible for delivering power and ground to all the components of the integrated circuit. Several concepts related to the power grid are discussed throughout the provided documents:

*   **Purpose**: The power grid needs to be defined as part of finalizing the floorplan. Its optimization is essential to free up routing resources for better Power, Performance, and Area (PPA). A stable voltage between power and ground, maintained by the power grid, is important to prevent IR drop on power nets and bouncing on ground nets.

*   **Components**: The power grid is constructed using various shapes and routing layers. Key components include:
    *   **Rings**: These can be added around the core area (`CORERING`), around blocks (`BLOCKRING`), or around power domains. The `addRing` command is used for this purpose.
    *   **Stripes**: These are generated using the `addStripe` command with the `STRIPE` shape. They can be routed over power domain boundaries or within the core area on different metal layers. Stripes can connect to power and ground bumps.
    *   **Vias**: These are inserted to connect different metal layers in the power grid. The VIAGEN Engine controls special via insertion during the `addRing`, `addStripe`, and `sroute` commands. The `editPowerVia` command can be used to add or modify vias, including those connecting to shielding wires. Stacked vias connecting low and high layer stripes can impact routing congestion.
    *   **Followpins**: These connect standard cell power pins within the row area using the `COREWIRE` shape outside the row.

*   **Power Planning and Routing Flow**: The typical flow involves:
    *   Connecting all Power/Ground (PG) pins or tie-hi/tie-low pins with a global power/ground net. This can be done using the `globalNetConnect` command or by reading a CPF file.
    *   Adding power rings and stripes using the `addRing` and `addStripe` commands. Mode setup commands like `setAddStripeMode` and `setViaGenMode` control the characteristics of these action commands.
    *   Routing power using the `sroute` command to connect various PG elements.

*   **Power Grid Optimization**: Redundant power grid stripes and vias can be trimmed using the `trim_pg` command based on IR drop analysis reports to improve area, timing, and power. Exclusion areas can be defined to prevent trimming in specific regions. A threshold for stripe merging can be set to treat slightly misaligned stripes as a single entity during trimming.

*   **Power Domains**: In low power designs, power domains are defined, and the power grid needs to supply each domain appropriately. Power planning should consider power stripes for feedthrough buffering inside power domains and PG rings to reduce IR drop within them. The `update_power_domain` command in CPF or IEEE1801 can specify the internal power and ground nets for a power domain.

*   **Power and Ground Checking**: It is important to perform power and ground checking before routing and extraction to verify the geometry and connectivity of the power grid. The `checkDesign` command can be used for this purpose, identifying off-grid power/ground preroutes.

*   **Metal Fill Connection**: Metal fill can be connected to the power/ground mesh to carry current and improve IR drop. The `addMetalFill` command with the `-net` option is used to specify the PG net for connection. Power strapping mode in metal fill insertion makes mesh connections to the power and ground bus wiring.

*   **Flip Chip Designs**: For flip chip methodologies, power and ground bumps need to be connected to the power grid, either to I/O pads or to rings and stripes using commands like `fcroute`.

In summary, the power grid is a fundamental network of power and ground সরবরাহ lines that ensures reliable power delivery to all active components in a chip. It involves careful planning, routing, and optimization using specific Innovus commands, especially in the context of floorplanning and low power design with power domains.