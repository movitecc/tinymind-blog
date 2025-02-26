---
title: py demo5
date: 2025-02-26T10:43:37.929Z
---

# PyQt5 Dock Widget 布局实现 - 5个Dock Widget

## 问题分析
根据您提供的图片和要求，需要实现一个包含5个dock widget的布局：
1. 顶部有一个横跨整个窗口的`dock_schedule`，占高度的10%
2. 左侧有两个垂直排列的dock widget：`dock_wd`和`dock2`，占宽度的40%
3. 右侧有两个垂直排列的dock widget：`gc_schedule`和`ac`，占宽度的60%
4. 纵向比例为1:4:5（顶部:中部:底部）
5. 横向比例为4:6（左侧:右侧）

## 解决方案
我将创建5个dock widget，并按照指定的比例进行布局。

```python
import sys
from PyQt5.QtWidgets import (QApplication, QMainWindow, QDockWidget, 
                             QTableWidget, QTableWidgetItem, QVBoxLayout, 
                             QWidget, QLabel, QCalendarWidget, QTreeWidget, 
                             QTreeWidgetItem, QListWidget, QListWidgetItem,
                             QTextEdit)
from PyQt5.QtCore import Qt, QSize

class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        
        # 设置窗口标题和大小
        self.setWindowTitle("5个Dock Widget布局示例")
        self.setMinimumSize(1000, 700)
        
        # 创建一个透明的中央部件
        central_widget = QWidget()
        central_widget.setMaximumSize(0, 0)  # 最小化中央部件
        self.setCentralWidget(central_widget)
        
        # 创建5个dock widget
        self.create_dock_schedule()
        self.create_dock_wd()
        self.create_dock2()
        self.create_gc_schedule()
        self.create_ac_dock()
        
        # 设置dock widget的布局
        self.setup_dock_layout()
    
    def create_dock_schedule(self):
        """创建顶部的dock_schedule"""
        self.dock_schedule = QDockWidget("dock_schedule", self)
        self.dock_schedule.setObjectName("dock_schedule")
        self.dock_schedule.setAllowedAreas(Qt.TopDockWidgetArea | Qt.BottomDockWidgetArea)
        self.dock_schedule.setFeatures(QDockWidget.DockWidgetMovable | QDockWidget.DockWidgetFloatable)
        
        # 使用日历组件作为内容
        calendar = QCalendarWidget()
        self.dock_schedule.setWidget(calendar)
        
        # 将dock widget添加到主窗口顶部
        self.addDockWidget(Qt.TopDockWidgetArea, self.dock_schedule)
    
    def create_dock_wd(self):
        """创建左上角的dock_wd"""
        self.dock_wd = QDockWidget("dock_wd", self)
        self.dock_wd.setObjectName("dock_wd")
        self.dock_wd.setAllowedAreas(Qt.AllDockWidgetAreas)
        self.dock_wd.setFeatures(QDockWidget.DockWidgetMovable | QDockWidget.DockWidgetFloatable)
        
        # 创建一个树形部件作为内容
        tree = QTreeWidget()
        tree.setHeaderLabels(["部件", "类型"])
        
        # 添加一些示例数据
        items = [
            ("按钮", "QPushButton"),
            ("标签", "QLabel"),
            ("输入框", "QLineEdit"),
            ("复选框", "QCheckBox"),
            ("单选按钮", "QRadioButton"),
            ("滑块", "QSlider"),
            ("进度条", "QProgressBar")
        ]
        
        for name, type_name in items:
            item = QTreeWidgetItem([name, type_name])
            tree.addTopLevelItem(item)
        
        self.dock_wd.setWidget(tree)
        
        # 将dock widget添加到主窗口左侧
        self.addDockWidget(Qt.LeftDockWidgetArea, self.dock_wd)
    
    def create_dock2(self):
        """创建左下角的dock2"""
        self.dock2 = QDockWidget("dock2", self)
        self.dock2.setObjectName("dock2")
        self.dock2.setAllowedAreas(Qt.AllDockWidgetAreas)
        self.dock2.setFeatures(QDockWidget.DockWidgetMovable | QDockWidget.DockWidgetFloatable)
        
        # 创建一个列表部件作为内容
        list_widget = QListWidget()
        
        # 添加一些示例数据
        items = [
            "项目 1",
            "项目 2",
            "项目 3",
            "项目 4",
            "项目 5",
            "项目 6",
            "项目 7",
            "项目 8",
            "项目 9",
            "项目 10",
            "项目 11",
            "项目 12",
            "项目 13",
            "项目 14",
            "项目 15"
        ]
        
        for item_text in items:
            list_widget.addItem(QListWidgetItem(item_text))
        
        self.dock2.setWidget(list_widget)
        
        # 将dock widget添加到主窗口左侧
        self.addDockWidget(Qt.LeftDockWidgetArea, self.dock2)
    
    def create_gc_schedule(self):
        """创建右上角的gc_schedule"""
        self.gc_schedule = QDockWidget("gc_schedule", self)
        self.gc_schedule.setObjectName("gc_schedule")
        self.gc_schedule.setAllowedAreas(Qt.AllDockWidgetAreas)
        self.gc_schedule.setFeatures(QDockWidget.DockWidgetMovable | QDockWidget.DockWidgetFloatable)
        
        # 创建一个表格作为内容
        table = QTableWidget(10, 4)
        table.setHorizontalHeaderLabels(["时间", "事件", "地点", "状态"])
        
        # 添加一些示例数据
        events = [
            ("09:00", "会议", "会议室A", "已确认"),
            ("10:30", "培训", "培训中心", "待确认"),
            ("12:00", "午餐", "餐厅", "已确认"),
            ("13:30", "项目讨论", "会议室B", "已确认"),
            ("15:00", "客户来访", "接待室", "待确认"),
            ("16:30", "团队总结", "会议室C", "已确认"),
            ("18:00", "晚餐", "餐厅", "待确认"),
            ("19:30", "加班", "办公室", "待确认"),
            ("21:00", "下班", "-", "-"),
            ("22:00", "休息", "家", "已确认")
        ]
        
        for row, (time, event, location, status) in enumerate(events):
            table.setItem(row, 0, QTableWidgetItem(time))
            table.setItem(row, 1, QTableWidgetItem(event))
            table.setItem(row, 2, QTableWidgetItem(location))
            table.setItem(row, 3, QTableWidgetItem(status))
        
        self.gc_schedule.setWidget(table)
        
        # 将dock widget添加到主窗口右侧
        self.addDockWidget(Qt.RightDockWidgetArea, self.gc_schedule)
    
    def create_ac_dock(self):
        """创建右下角的ac dock"""
        self.ac_dock = QDockWidget("ac", self)
        self.ac_dock.setObjectName("ac_dock")
        self.ac_dock.setAllowedAreas(Qt.AllDockWidgetAreas)
        self.ac_dock.setFeatures(QDockWidget.DockWidgetMovable | QDockWidget.DockWidgetFloatable)
        
        # 创建一个文本编辑器作为内容
        text_edit = QTextEdit()
        text_edit.setPlaceholderText("在这里输入笔记...")
        text_edit.setText("这是AC区域的示例内容。\n\n"
                         "您可以在这里添加笔记、备忘录或其他文本内容。\n\n"
                         "这个区域使用QTextEdit组件，支持富文本编辑。\n\n"
                         "- 可以创建列表\n"
                         "- 添加格式化文本\n"
                         "- 插入表格等\n\n"
                         "这个区域占据右下角，比例为60%宽度和50%高度（不包括顶部区域）。")
        
        self.ac_dock.setWidget(text_edit)
        
        # 将dock widget添加到主窗口右侧
        self.addDockWidget(Qt.RightDockWidgetArea, self.ac_dock)
    
    def setup_dock_layout(self):
        """设置dock widget的布局，按照指定比例排列"""
        # 确保dock_schedule横跨整个窗口顶部
        self.splitDockWidget(self.dock_wd, self.gc_schedule, Qt.Horizontal)
        self.splitDockWidget(self.dock_wd, self.dock2, Qt.Vertical)
        self.splitDockWidget(self.gc_schedule, self.ac_dock, Qt.Vertical)
        
        # 将dock_schedule设置为跨越整个顶部
        self.addDockWidget(Qt.TopDockWidgetArea, self.dock_schedule)
        
        # 添加窗口大小变化事件处理，以保持比例
        self.resizeEvent = self.on_resize
        
        # 初始调整大小
        self.adjust_sizes()
    
    def adjust_sizes(self):
        """调整dock widget的大小以符合指定比例"""
        window_height = self.height()
        window_width = self.width()
        
        # 横向比例 4:6
        left_width = int(window_width * 0.4)
        right_width = int(window_width * 0.6)
        
        # 纵向比例 1:4:5
        top_height = int(window_height * 0.1)  # 顶部占10%
        remaining_height = window_height - top_height
        middle_height = int(remaining_height * (4/9))
        bottom_height = int(remaining_height * (5/9))
        
        # 应用比例
        self.resizeDocks([self.dock_schedule], [top_height], Qt.Vertical)
        self.resizeDocks([self.dock_wd, self.gc_schedule], [left_width, right_width], Qt.Horizontal)
        self.resizeDocks([self.dock_wd, self.dock2], [middle_height, bottom_height], Qt.Vertical)
        self.resizeDocks([self.gc_schedule, self.ac_dock], [middle_height, bottom_height], Qt.Vertical)
    
    def on_resize(self, event):
        """窗口大小变化时保持dock widget的比例"""
        # 调用原始的resizeEvent
        super().resizeEvent(event)
        
        # 重新调整大小
        self.adjust_sizes()


if __name__ == "__main__":
    app = QApplication(sys.argv)
    window = MainWindow()
    window.show()
    sys.exit(app.exec_())
```

## 代码说明

1. **布局结构**：
   - 创建了5个dock widget，分别是：
     - `dock_schedule`：横跨窗口顶部
     - `dock_wd`：左上角
     - `dock2`：左下角
     - `gc_schedule`：右上角
     - `ac_dock`：右下角

2. **比例设置**：
   - 横向比例为4:6，即左侧占40%，右侧占60%。
   - 纵向比例为1:4:5，即顶部占10%，中部占剩余高度的4/9（约40%），底部占剩余高度的5/9（约50%）。

3. **布局方法**：
   - 使用`splitDockWidget()`方法创建基本布局结构。
   - 使用`resizeDocks()`方法设置各个dock widget的大小。
   - 创建了`adjust_sizes()`方法来计算和应用正确的比例。
   - 重写了`resizeEvent`方法，在窗口大小变化时重新调整比例。

4. **各Dock Widget的内容**：
   - `dock_schedule`：使用`QCalendarWidget`日历组件。
   - `dock_wd`：使用`QTreeWidget`树形组件，展示部件列表。
   - `dock2`：使用`QListWidget`列表组件，展示简单的数据列表。
   - `gc_schedule`：使用`QTableWidget`表格组件，展示日程安排。
   - `ac_dock`：使用`QTextEdit`文本编辑器，提供笔记功能。

5. **动态调整**：
   - 当窗口大小变化时，所有dock widget会按照指定的比例自动调整大小。
   - 这确保了无论窗口如何调整大小，布局比例都保持不变。

## 使用示例

运行上述代码后，将显示一个包含5个dock widget的窗口，布局如下：
1. 顶部是横跨整个窗口的`dock_schedule`，占总高度的10%
2. 左侧区域占窗口宽度的40%，分为上下两部分：
   - 上部是`dock_wd`，占剩余高度的4/9
   - 下部是`dock2`，占剩余高度的5/9
3. 右侧区域占窗口宽度的60%，分为上下两部分：
   - 上部是`gc_schedule`，占剩余高度的4/9
   - 下部是`ac_dock`，占剩余高度的5/9

当用户调整窗口大小时，所有dock widget会按照指定的比例自动调整，保持布局的一致性。

## 优化建议

1. **布局锁定**：添加一个选项，允许用户锁定布局，防止意外改变dock widget的位置。

2. **布局预设**：提供几种预设布局，让用户可以快速切换不同的布局方案。

3. **状态保存**：实现布局状态的保存和恢复功能，让用户可以在下次启动应用时恢复自定义布局。

4. **主题支持**：添加明暗主题切换功能，提高用户体验。

5. **内容联动**：实现dock widget之间的内容联动，例如在`gc_schedule`中选择一个事件，在`ac_dock`中显示相关笔记。

6. **性能优化**：对于大量数据的情况，考虑使用模型-视图架构（MVC）来提高性能。

7. **自定义标题栏**：为dock widget添加自定义标题栏，提供更多功能按钮和更好的视觉效果。