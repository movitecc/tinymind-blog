---
title: py demo
date: 2025-02-26T07:02:50.038Z
---

## 问题分析
根据新的草图，您希望在 PyQt5 中实现一个包含四个表格的布局：左上角为 "table A"，右上角为 "table C"，左下角为 "table B"，右下角为 "table D"。其中，"table C" 和 "table D" 应该在同一个可停靠窗口中。

## 解决方案
我们将使用 `QDockWidget` 创建两个可停靠窗口：
1. 一个可停靠窗口包含 "table A" 和 "table B"。
2. 另一个可停靠窗口包含 "table C" 和 "table D"。

每个表格将使用 `QTableWidget` 来实现。以下是实现该布局的代码：

```python
import sys
from PyQt5.QtWidgets import QApplication, QMainWindow, QDockWidget, QTableWidget, QTableWidgetItem, QVBoxLayout, QWidget, QHBoxLayout

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()

        # 设置主窗口的标题和大小
        self.setWindowTitle("Dock Widget Example")
        self.setGeometry(100, 100, 800, 600)

        # 创建一个中心部件
        central_widget = QWidget()
        self.setCentralWidget(central_widget)

        # 创建一个水平布局来放置两个可停靠窗口
        layout = QHBoxLayout(central_widget)

        # 创建第一个可停靠窗口 (包含 table A 和 table B)
        self.dock1 = QDockWidget("Tables A and B", self)
        dock1_widget = QWidget()
        dock1_layout = QVBoxLayout(dock1_widget)

        # 创建 table A
        self.tableA = QTableWidget(2, 2)  # 创建一个2行2列的表格
        self.tableA.setItem(0, 0, QTableWidgetItem("Table A - Row 1, Col 1"))
        self.tableA.setItem(0, 1, QTableWidgetItem("Table A - Row 1, Col 2"))
        self.tableA.setItem(1, 0, QTableWidgetItem("Table A - Row 2, Col 1"))
        self.tableA.setItem(1, 1, QTableWidgetItem("Table A - Row 2, Col 2"))
        dock1_layout.addWidget(self.tableA)

        # 创建 table B
        self.tableB = QTableWidget(2, 2)  # 创建一个2行2列的表格
        self.tableB.setItem(0, 0, QTableWidgetItem("Table B - Row 1, Col 1"))
        self.tableB.setItem(0, 1, QTableWidgetItem("Table B - Row 1, Col 2"))
        self.tableB.setItem(1, 0, QTableWidgetItem("Table B - Row 2, Col 1"))
        self.tableB.setItem(1, 1, QTableWidgetItem("Table B - Row 2, Col 2"))
        dock1_layout.addWidget(self.tableB)

        self.dock1.setWidget(dock1_widget)
        self.addDockWidget(Qt.LeftDockWidgetArea, self.dock1)

        # 创建第二个可停靠窗口 (包含 table C 和 table D)
        self.dock2 = QDockWidget("Tables C and D", self)
        dock2_widget = QWidget()
        dock2_layout = QVBoxLayout(dock2_widget)

        # 创建 table C
        self.tableC = QTableWidget(2, 2)  # 创建一个2行2列的表格
        self.tableC.setItem(0, 0, QTableWidgetItem("Table C - Row 1, Col 1"))
        self.tableC.setItem(0, 1, QTableWidgetItem("Table C - Row 1, Col 2"))
        self.tableC.setItem(1, 0, QTableWidgetItem("Table C - Row 2, Col 1"))
        self.tableC.setItem(1, 1, QTableWidgetItem("Table C - Row 2, Col 2"))
        dock2_layout.addWidget(self.tableC)

        # 创建 table D
        self.tableD = QTableWidget(2, 2)  # 创建一个2行2列的表格
        self.tableD.setItem(0, 0, QTableWidgetItem("Table D - Row 1, Col 1"))
        self.tableD.setItem(0, 1, QTableWidgetItem("Table D - Row 1, Col 2"))
        self.tableD.setItem(1, 0, QTableWidgetItem("Table D - Row 2, Col 1"))
        self.tableD.setItem(1, 1, QTableWidgetItem("Table D - Row 2, Col 2"))
        dock2_layout.addWidget(self.tableD)

        self.dock2.setWidget(dock2_widget)
        self.addDockWidget(Qt.RightDockWidgetArea, self.dock2)

if __name__ == "__main__":
    app = QApplication(sys.argv)
    window = MainWindow()
    window.show()
    sys.exit(app.exec_())
```

## 代码说明
1. **导入模块**：导入必要的 PyQt5 模块。
2. **创建主窗口**：`MainWindow` 类继承自 `QMainWindow`，设置窗口标题和大小。
3. **中心部件**：创建一个中心部件，并设置为主窗口的中心部件。
4. **可停靠窗口**：
   - 第一个可停靠窗口 (`dock1`) 包含 "table A" 和 "table B"，使用垂直布局 (`QVBoxLayout`)。
   - 第二个可停靠窗口 (`dock2`) 包含 "table C" 和 "table D"，同样使用垂直布局。
   - 每个表格使用 `QTableWidget` 创建，设置表格的行列数和内容。
   - 使用 `addDockWidget` 方法将可停靠窗口添加到主窗口的左侧和右侧。
5. **主程序**：创建 `QApplication` 实例并运行主窗口。

## 使用示例
运行上述代码后，将会显示一个窗口，其中包含两个可停靠窗口（"Tables A and B" 和 "Tables C and D"），每个窗口内分别显示两个表格。用户可以拖动这些窗口进行布局调整。

## 优化建议
- 可以根据需要调整表格的行列数和内容。
- 可以添加更多的功能，比如数据的编辑、保存和加载等。
- 考虑使用样式表来美化界面。