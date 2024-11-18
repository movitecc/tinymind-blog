---
title: DC - DC Expert, DC Ultar, DC Graphical, DC NXT
date: 2024-11-18T14:41:59.693Z
---

## Synopsys Design Compiler Product Family 

The Design Compiler family from Synopsys comprises various tools and features aimed at logic synthesis and optimization in digital circuit design. These tools can be accessed through command-line interfaces like `dc_shell` and `dcnxt_shell` or through the Design Vision GUI. The table below provides an overview of the features and differences between DC Expert, DC Ultra, Design Compiler NXT, and Design Compiler Graphical, drawing upon the provided sources. 

| Feature                          | DC Expert                          | DC Ultra                             | Design Compiler Graphical       | Design Compiler NXT             |
|----------------------------------|-------------------------------------|--------------------------------------|----------------------------------|-------------------------------------|
| Description                       | Provides basic logic synthesis capabilities. |  Offers advanced optimizations for timing, area, power, and test. | Adds features for multicorner-multimode design, congestion reduction, and physical guidance. | Includes all features of the other Design Compiler products, along with additional advanced optimizations, physical guidance, and faster runtime. |
| License Requirements              | DC Expert license required.  | DC Ultra license required.          | Design Compiler Graphical license required. | Design Compiler NXT license required. |
| Interface                        | Command-line (`dc_shell`)           | Command-line (`dc_shell`)            | Command-line (`dc_shell`) and GUI | Command-line (`dcnxt_shell`) and GUI |
| Compile Command                   | `compile`                   | `compile_ultra`                 | `compile_ultra`         | `compile_ultra`                  |
| Key Features                     | * Hierarchical compile (top-down or bottom-up) <br> * Full and incremental compile techniques <br> * Sequential optimization <br> * Time borrowing for latch-based designs <br> * Timing analysis | * Infrastructure for multicore execution <br> * Advanced arithmetic optimization <br> * Integrated datapath optimization <br> * Advanced timing analysis <br> * Advanced delay optimization <br> * Automatic leakage power optimization <br> * Register retiming | * Multicorner-multimode optimization <br> * Routing congestion reduction <br> * Improved correlation with IC Compiler and IC Compiler II <br> * Synopsys Physical Guidance <br> * Floorplan creation and modification | * Enhanced QoR through advanced optimizations <br> * Enhanced physical guidance for IC Compiler II <br> * Faster runtime with improved multithreading |
| Usage                            | Suited for general logic synthesis tasks and less demanding designs. | Offers a more robust set of features for designs with stringent timing constraints.  Best for timing-critical, high-performance designs. | Suitable for complex designs requiring analysis and optimization across multiple operating conditions and modes.  Facilitates a smooth transition to IC Compiler and IC Compiler II for physical implementation. | The most advanced option for designs with complex requirements, offering the most comprehensive feature set, improved QoR, and faster runtime. |

### Important Points

* **DC Ultra** and **DC Graphical** both provide advanced features compared to **DC Expert**. 
* **Design Compiler Graphical** and **Design Compiler NXT** offer GUI support in addition to the command-line interface.
* **Design Compiler NXT** builds upon the features of the other Design Compiler tools, providing the most advanced optimizations and capabilities.
* **Multicorner-Multimode** design is supported in both **Design Compiler Graphical** and **Design Compiler NXT**, allowing for analysis and optimization under various operating conditions and modes.
* **Synopsys Physical Guidance** is available in **Design Compiler Graphical** and **Design Compiler NXT**, improving correlation between synthesis and physical implementation. 

For further study, the Synopsys Design Compiler User Guide provides detailed information on each product, including specific commands, options, and workflows.  You can also access the help system within the Design Compiler environment using the commands `help`, `command_name -help`, and `man command_or_variable_name` for additional information on specific commands and variables.

| 特征         | DC Expert                  | DC Ultra                     | DC Graphical                | DC NXT                     |
|--------------|----------------------------|------------------------------|-----------------------------|----------------------------|
| 基本功能     | 基础综合功能              | 包含Expert功能+高级优化     | 包含Ultra功能+物理优化     | 新一代综合技术            |
| 时序优化     | 标准优化                  | 增强优化                    | 物理感知优化               | AI辅助优化                |
| 面积优化     | 基本优化                  | 高级优化                    | 布局感知优化               | 智能优化                  |
| 功耗优化     | 基本控制                  | 多角度优化                  | 物理功耗优化               | 全局功耗优化              |
| 物理感知     | 无                        | 有限                        | 完整支持                   | 增强支持                  |
| 运行速度     | 基准                      | 1.2x基准                    | 1.5x基准                   | 2x基准                    |
| 工艺节点     | 所有节点                  | 28nm以下                    | 16nm以下                   | 5nm以下                   |
| 许可费用     | 最低                      | 中等                        | 较高                       | 最高                      |

