---
title: py demo
date: 2025-02-26T07:02:50.038Z
---

# PyQt5 Dock Widget 布局实现

## 问题分析
根据提供的草图，需要实现一个包含四个表格(table A, B, C, D)的GUI界面，这些表格应该使用PyQt5的Dock Widget组件来实现。每个表格需要使用单独的方法定义，并且布局应该按照草图所示的2x2网格排列。

## 解决方案
我将创建一个主窗口，其中包含四个可停靠的QDockWidget组件，每个组件内部包含一个QTableWidget。这些dock widget将被排列成2x2的网格布局。

```python
import sys
from PyQt5.QtWidgets import (QApplication, QMainWindow, QDockWidget, 
                             QTableWidget, QTableWidgetItem, QVBoxLayout, 
                             QWidget)
from PyQt5.QtCore import Qt, QSize

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        
        # 设置窗口标题和大小
        self.setWindowTitle("四表格Dock Widget布局")
        self.setMinimumSize(800, 600)
        
        # 创建中央部件（虽然不会显示，但QMainWindow需要一个中央部件）
        central_widget = QWidget()
        self.setCentralWidget(central_widget)
        
        # 创建四个表格的dock widget
        self.create_table_a_dock()
        self.create_table_b_dock()
        self.create_table_c_dock()
        self.create_table_d_dock()
        
        # 设置dock widget的初始位置
        self.setup_dock_layout()
    
    def create_table_a_dock(self):
        """创建表格A的dock widget"""
        # 创建dock widget
        self.dock_a = QDockWidget("表格A", self)
        self.dock_a.setObjectName("TableA")
        self.dock_a.setAllowedAreas(Qt.AllDockWidgetAreas)
        
        # 创建表格
        table_a = QTableWidget(10, 5)  # 10行5列的表格
        table_a.setHorizontalHeaderLabels(["列1", "列2", "列3", "列4", "列5"])
        
        # 添加一些示例数据
        for row in range(10):
            for col in range(5):
                table_a.setItem(row, col, QTableWidgetItem(f"A-{row},{col}"))
        
        # 将表格设置为dock widget的内容
        self.dock_a.setWidget(table_a)
        
        # 将dock widget添加到主窗口
        self.addDockWidget(Qt.LeftDockWidgetArea, self.dock_a)
    
    def create_table_b_dock(self):
        """创建表格B的dock widget"""
        # 创建dock widget
        self.dock_b = QDockWidget("表格B", self)
        self.dock_b.setObjectName("TableB")
        self.dock_b.setAllowedAreas(Qt.AllDockWidgetAreas)
        
        # 创建表格
        table_b = QTableWidget(8, 4)  # 8行4列的表格
        table_b.setHorizontalHeaderLabels(["名称", "数量", "单价", "总价"])
        
        # 添加一些示例数据
        for row in range(8):
            table_b.setItem(row, 0, QTableWidgetItem(f"产品{row+1}"))
            table_b.setItem(row, 1, QTableWidgetItem(f"{row+2}"))
            table_b.setItem(row, 2, QTableWidgetItem(f"{(row+1)*10}"))
            table_b.setItem(row, 3, QTableWidgetItem(f"{(row+1)*(row+2)*10}"))
        
        # 将表格设置为dock widget的内容
        self.dock_b.setWidget(table_b)
        
        # 将dock widget添加到主窗口
        self.addDockWidget(Qt.LeftDockWidgetArea, self.dock_b)
    
    def create_table_c_dock(self):
        """创建表格C的dock widget"""
        # 创建dock widget
        self.dock_c = QDockWidget("表格C", self)
        self.dock_c.setObjectName("TableC")
        self.dock_c.setAllowedAreas(Qt.AllDockWidgetAreas)
        
        # 创建表格
        table_c = QTableWidget(12, 3)  # 12行3列的表格
        table_c.setHorizontalHeaderLabels(["日期", "事件", "状态"])
        
        # 添加一些示例数据
        for row in range(12):
            table_c.setItem(row, 0, QTableWidgetItem(f"2023-{row+1}-01"))
            table_c.setItem(row, 1, QTableWidgetItem(f"事件{row+1}"))
            table_c.setItem(row, 2, QTableWidgetItem("完成" if row % 2 == 0 else "进行中"))
        
        # 将表格设置为dock widget的内容
        self.dock_c.setWidget(table_c)
        
        # 将dock widget添加到主窗口
        self.addDockWidget(Qt.RightDockWidgetArea, self.dock_c)
    
    def create_table_d_dock(self):
        """创建表格D的dock widget"""
        # 创建dock widget
        self.dock_d = QDockWidget("表格D", self)
        self.dock_d.setObjectName("TableD")
        self.dock_d.setAllowedAreas(Qt.AllDockWidgetAreas)
        
        # 创建表格
        table_d = QTableWidget(6, 6)  # 6行6列的表格
        table_d.setHorizontalHeaderLabels(["A", "B", "C", "D", "E", "F"])
        
        # 添加一些示例数据
        for row in range(6):
            for col in range(6):
                table_d.setItem(row, col, QTableWidgetItem(f"D-{row},{col}"))
        
        # 将表格设置为dock widget的内容
        self.dock_d.setWidget(table_d)
        
        # 将dock widget添加到主窗口
        self.addDockWidget(Qt.RightDockWidgetArea, self.dock_d)
    
    def setup_dock_layout(self):
        """设置dock widget的布局，形成2x2网格"""
        # 将dock_b放在dock_a下方
        self.splitDockWidget(self.dock_a, self.dock_b, Qt.Vertical)
        
        # 将dock_d放在dock_c下方
        self.splitDockWidget(self.dock_c, self.dock_d, Qt.Vertical)
        
        # 将右侧的dock_c和dock_d组合放在左侧的dock_a和dock_b组合的右边
        self.splitDockWidget(self.dock_a, self.dock_c, Qt.Horizontal)
        
        # 设置初始大小比例
        self.resizeDocks([self.dock_a, self.dock_b], [200, 200], Qt.Vertical)
        self.resizeDocks([self.dock_c, self.dock_d], [200, 200], Qt.Vertical)
        self.resizeDocks([self.dock_a, self.dock_c], [400, 400], Qt.Horizontal)


if __name__ == "__main__":
    app = QApplication(sys.argv)
    window = MainWindow()
    window.show()
    sys.exit(app.exec_())
```

## 代码说明

1. **主窗口设计**：
   - 创建了一个继承自`QMainWindow`的主窗口类，这是PyQt5中用于创建主应用窗口的标准类。
   - 设置了窗口标题和最小尺寸，确保用户可以看到所有内容。

2. **Dock Widget创建**：
   - 为每个表格创建了单独的方法（`create_table_a_dock()`、`create_table_b_dock()`等），使代码结构清晰。
   - 每个方法创建一个`QDockWidget`并在其中放置一个`QTableWidget`。
   - 每个表格都有不同的行列数和示例数据，以展示其灵活性。

3. **布局设置**：
   - `setup_dock_layout()`方法负责将四个dock widget排列成2x2网格。
   - 使用`splitDockWidget()`方法将dock widget分割成垂直和水平组合。
   - 使用`resizeDocks()`方法设置初始大小比例，确保各个dock widget大小合适。

4. **表格数据**：
   - 每个表格都填充了不同的示例数据，以便于区分。
   - 表格A和D使用坐标式数据，表格B模拟产品列表，表格C模拟日程表。

5. **Dock Widget特性**：
   - 所有dock widget都设置了`setAllowedAreas(Qt.AllDockWidgetAreas)`，允许用户将它们拖动到任何位置。
   - 每个dock widget都有一个标题，显示对应的表格名称。

## 使用示例

运行上述代码后，将显示一个包含四个可停靠表格的窗口，布局如草图所示。用户可以：

1. 通过拖动dock widget的标题栏来重新排列它们
2. 双击dock widget的标题栏使其浮动或重新停靠
3. 点击dock widget右上角的关闭按钮暂时隐藏它（可以通过View菜单重新显示）
4. 调整dock widget之间的分隔线来改变它们的大小

## 优化建议

1. **状态保存与恢复**：可以添加功能来保存和恢复dock widget的布局状态，使用户可以在下次启动应用时恢复自定义布局。

2. **上下文菜单**：为表格添加右键菜单，提供更多功能，如排序、筛选等。

3. **数据模型**：使用`QAbstractTableModel`替代直接填充`QTableWidget`，以实现更高效的数据管理，特别是当处理大量数据时。

4. **样式美化**：添加样式表来美化界面，使表格和dock widget更加美观。

5. **功能扩展**：根据实际需求，可以为表格添加编辑、搜索、导出等功能。