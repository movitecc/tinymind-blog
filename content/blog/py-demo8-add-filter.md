---
title: py demo8 add filter
date: 2025-03-10T15:54:42.374Z
---

# 极简PyQt5表格筛选功能实现

根据您的建议，如果将"Dx ▼"直接放在表格的第一行而不是表头，程序可以大幅简化。下面是一个更简洁、更稳定的实现方案：

```python
import sys
from PyQt5.QtWidgets import (QApplication, QMainWindow, QWidget, QProgressBar,
                           QTableWidget, QTableWidgetItem, QHeaderView, QDockWidget,
                           QVBoxLayout, QSizePolicy, QMenu, QAction)
from PyQt5.QtCore import Qt, QEvent, QTimer
from PyQt5.QtGui import QFont, QCursor


class MainWindow(QMainWindow):
    def __init__(self):
        super().__init__()
        self.init_ui()

    def init_ui(self):
        # 设置窗口标题和默认大小
        self.setWindowTitle('停靠窗口表格演示')
        self.resize(1200, 800)

        # 创建一个空的中央部件
        central_widget = QWidget(self)
        self.setCentralWidget(central_widget)

        # 创建顶部停靠窗口，用于放置进度条
        self.progress_dock = QDockWidget(self)
        self.progress_dock.setFeatures(QDockWidget.NoDockWidgetFeatures)  # 禁用标题栏和移动/浮动功能
        self.progress_dock.setTitleBarWidget(QWidget())  # 隐藏标题栏

        # 创建进度条并放入顶部停靠窗口
        progress_widget = QWidget()
        progress_layout = QVBoxLayout(progress_widget)
        progress_layout.setContentsMargins(5, 5, 5, 5)  # 设置边距
        self.progress_bar = self.create_progress_bar()
        progress_layout.addWidget(self.progress_bar)
        self.progress_dock.setWidget(progress_widget)

        # 添加顶部停靠窗口
        self.addDockWidget(Qt.TopDockWidgetArea, self.progress_dock)

        # 创建并添加四个停靠窗口和表格
        self.dock_a = self.create_dock_widget_a()
        self.dock_b = self.create_dock_widget_b()
        self.dock_c = self.create_dock_widget_c()
        self.dock_d = self.create_dock_widget_d()

        # 将停靠窗口添加到主窗口
        self.addDockWidget(Qt.LeftDockWidgetArea, self.dock_a)  # 左上
        self.addDockWidget(Qt.RightDockWidgetArea, self.dock_b)  # 右上
        self.addDockWidget(Qt.LeftDockWidgetArea, self.dock_c)  # 左下
        self.addDockWidget(Qt.RightDockWidgetArea, self.dock_d)  # 右下

        # 设置停靠窗口的初始位置关系
        self.splitDockWidget(self.dock_a, self.dock_c, Qt.Vertical)  # A在C上方
        self.splitDockWidget(self.dock_b, self.dock_d, Qt.Vertical)  # B在D上方

        # 设置停靠窗口的大小
        self.resizeDocks([self.dock_a, self.dock_b], [300, 300], Qt.Vertical)  # 设置上部两个dock的高度
        self.resizeDocks([self.dock_c, self.dock_d], [500, 500], Qt.Vertical)  # 设置下部两个dock的高度

        # 设置左右区域的比例 - 使用明确的值来匹配您的理想布局
        total_width = self.width()
        left_width = int(total_width * 0.33)  # 左侧约1/3宽度
        right_width = total_width - left_width  # 右侧约2/3宽度
        self.resizeDocks([self.dock_a, self.dock_b], [left_width, right_width], Qt.Horizontal)
        self.resizeDocks([self.dock_c, self.dock_d], [left_width, right_width], Qt.Horizontal)

        # 确保进度条宽度占满整个窗口
        self.resizeDocks([self.progress_dock], [30], Qt.Vertical)  # 设置进度条dock的高度

        # 禁用停靠区域交叉混合，并允许嵌套停靠
        self.setDockOptions(QMainWindow.AllowNestedDocks | QMainWindow.ForceTabbedDocks)

        # 设置全局间隙为0
        self.setContentsMargins(0, 0, 0, 0)

        # 安装事件过滤器以监控大小变化
        self.installEventFilter(self)

    def eventFilter(self, obj, event):
        """事件过滤器，用于捕获窗口大小变化事件"""
        if obj == self and event.type() == QEvent.Resize:
            # 延迟执行调整大小的操作，确保所有布局计算完成
            QTimer.singleShot(0, self.adjustDockSizes)
        return super().eventFilter(obj, event)

    def adjustDockSizes(self):
        """调整停靠窗口大小，确保紧密贴合"""
        # 计算理想宽度分配
        total_width = self.width()
        left_width = int(total_width * 0.33)  # 左侧约1/3宽度
        right_width = total_width - left_width  # 右侧约2/3宽度

        # 应用宽度分配
        self.resizeDocks([self.dock_a, self.dock_b], [left_width, right_width], Qt.Horizontal)
        self.resizeDocks([self.dock_c, self.dock_d], [left_width, right_width], Qt.Horizontal)

    def create_progress_bar(self):
        """创建并返回一个进度条"""
        progress_bar = QProgressBar()
        progress_bar.setMinimum(0)
        progress_bar.setMaximum(100)
        progress_bar.setValue(50)  # 设置初始值为50%
        progress_bar.setFixedHeight(30)  # 设置固定高度
        return progress_bar

    def create_table_widget(self, rows, columns, headers):
        """创建一个通用的表格部件，包含隐藏行号的设置"""
        table = QTableWidget(rows, columns)
        table.setHorizontalHeaderLabels(headers)

        # 隐藏行号
        table.verticalHeader().setVisible(False)

        # 调整表格大小策略
        table.horizontalHeader().setSectionResizeMode(QHeaderView.Stretch)

        # 确保表格紧密填充其容器
        table.setFrameShape(QTableWidget.NoFrame)  # 移除表格的边框
        table.setShowGrid(True)  # 显示网格线

        return table

    def create_dock_widget_a(self):
        """创建并返回停靠窗口A，其中包含表格"""
        dock = QDockWidget("表格A", self)
        dock.setAllowedAreas(Qt.AllDockWidgetAreas)

        # 使用通用方法创建表格
        headers = ['列标题1', '列标题2', '列标题3']
        table = self.create_table_widget(5, 3, headers)

        # 填充示例数据
        for row in range(5):
            for col in range(3):
                item = QTableWidgetItem(f'A-{row},{col}')
                table.setItem(row, col, item)

        # 设置内容边距为0，确保表格填满整个区域
        wrapper = QWidget()
        layout = QVBoxLayout(wrapper)
        layout.setContentsMargins(0, 0, 0, 0)
        layout.addWidget(table)
        dock.setWidget(wrapper)

        # 确保停靠窗口紧凑
        dock.setContentsMargins(0, 0, 0, 0)

        return dock

    def create_dock_widget_b(self):
        """创建并返回停靠窗口B，其中包含表格"""
        dock = QDockWidget("表格B", self)
        dock.setAllowedAreas(Qt.AllDockWidgetAreas)

        # 使用通用方法创建表格，只有2行
        headers = ['B1', 'B2', 'B3', 'B4', 'B5', 'B6']
        table = self.create_table_widget(2, 6, headers)

        # 填充示例数据
        for row in range(2):
            for col in range(6):
                item = QTableWidgetItem(f'B-{row},{col}')
                table.setItem(row, col, item)

        # 增加行高以使表格B的内容占据更多空间
        for row in range(2):
            table.setRowHeight(row, 50)  # 设置更大的行高

        # 使用特殊的布局处理来确保表格B和D之间无空白
        wrapper = QWidget()
        layout = QVBoxLayout(wrapper)
        layout.setContentsMargins(0, 0, 0, 0)
        layout.addWidget(table)

        # 添加一个会自动伸展以填满剩余空间的空白部件
        spacer = QWidget()
        spacer.setMinimumHeight(0)
        spacer.setSizePolicy(
            QSizePolicy.Preferred,
            QSizePolicy.Expanding
        )
        layout.addWidget(spacer)

        dock.setWidget(wrapper)

        # 确保停靠窗口紧凑
        dock.setContentsMargins(0, 0, 0, 0)

        return dock

    def create_dock_widget_c(self):
        """创建并返回停靠窗口C，其中包含表格"""
        dock = QDockWidget("表格C", self)
        dock.setAllowedAreas(Qt.AllDockWidgetAreas)

        # 使用通用方法创建表格
        headers = ['列标题1', '列标题2', '列标题3']
        table = self.create_table_widget(8, 3, headers)

        # 填充示例数据
        for row in range(8):
            for col in range(3):
                item = QTableWidgetItem(f'C-{row},{col}')
                table.setItem(row, col, item)

        # 设置内容边距为0，确保表格填满整个区域
        wrapper = QWidget()
        layout = QVBoxLayout(wrapper)
        layout.setContentsMargins(0, 0, 0, 0)
        layout.addWidget(table)
        dock.setWidget(wrapper)

        # 确保停靠窗口紧凑
        dock.setContentsMargins(0, 0, 0, 0)

        return dock

    def create_dock_widget_d(self):
        """创建并返回停靠窗口D，其中包含表格和筛选按钮"""
        dock = QDockWidget("表格D", self)
        dock.setAllowedAreas(Qt.AllDockWidgetAreas)

        # 创建一个容器来放置表格
        wrapper = QWidget()
        layout = QVBoxLayout(wrapper)
        layout.setContentsMargins(0, 0, 0, 0)
        layout.setSpacing(0)
        
        # 创建标准表格，隐藏默认表头
        self.table_d = QTableWidget(29, 6)  # 1行筛选按钮 + 28行数据
        self.table_d.horizontalHeader().setVisible(False)  # 隐藏默认表头
        self.table_d.verticalHeader().setVisible(False)    # 隐藏行号
        
        # 设置表格样式
        self.table_d.setFrameShape(QTableWidget.NoFrame)
        self.table_d.setShowGrid(True)
        
        # 为表格每列创建三组数据（三个版本）
        self.current_versions = {}
        self.column_data = {}
        
        # 初始化筛选按钮行和数据
        for col in range(6):
            # 创建筛选按钮单元格（第一行）
            filter_item = QTableWidgetItem(f"D{col+1} ▼")
            filter_item.setTextAlignment(Qt.AlignCenter)
            filter_item.setFlags(Qt.ItemIsEnabled)  # 使项目可以被单击，但不能编辑
            font = QFont()
            font.setBold(True)
            filter_item.setFont(font)
            filter_item.setBackground(Qt.lightGray)  # 设置浅灰色背景
            self.table_d.setItem(0, col, filter_item)
            
            # 初始化当前版本
            self.current_versions[col] = 1
            
            # 创建三个版本的数据
            col_name = f"D{col+1}"
            ver_data = {
                1: [f'{col_name}_ver1_{row}' for row in range(28)],
                2: [f'{col_name}_ver2_{row}' for row in range(28)],
                3: [f'{col_name}_ver3_{row}' for row in range(28)]
            }
            self.column_data[col] = ver_data
            
            # 填充初始数据（版本1）
            for row in range(28):
                item = QTableWidgetItem(ver_data[1][row])
                self.table_d.setItem(row+1, col, item)  # 数据从第二行开始
        
        # 第一行（筛选按钮行）的高度
        self.table_d.setRowHeight(0, 30)
        
        # 设置列宽度平均分布
        header = self.table_d.horizontalHeader()
        header.setSectionResizeMode(QHeaderView.Stretch)
        
        # 连接单元格点击事件
        self.table_d.cellClicked.connect(self.handle_cell_click)
        
        layout.addWidget(self.table_d)
        dock.setWidget(wrapper)
        
        return dock
        
    def handle_cell_click(self, row, column):
        """处理单元格点击事件"""
        # 只有点击第一行（筛选按钮行）才处理
        if row == 0:
            self.show_filter_menu(column)
    
    def show_filter_menu(self, column):
        """显示筛选菜单，提供版本选择"""
        # 创建菜单
        menu = QMenu(self)
        
        # 添加三个版本选项
        for ver in range(1, 4):
            action = QAction(f"版本 {ver}", self)
            # 使用lambda默认参数避免延迟绑定问题
            action.triggered.connect(lambda checked=False, col=column, v=ver: self.switch_version(col, v))
            
            # 标记当前选中版本
            if self.current_versions[column] == ver:
                action.setIcon(self.style().standardIcon(self.style().SP_DialogApplyButton))
                
            menu.addAction(action)
        
        # 在鼠标位置显示菜单
        menu.exec_(QCursor.pos())
    
    def switch_version(self, column, version):
        """切换到指定版本的数据"""
        if column not in self.current_versions:
            return
        
        self.current_versions[column] = version
        
        # 更新表格数据（从第二行开始）
        for row in range(28):
            table_row = row + 1  # 表格行索引（跳过筛选按钮行）
            if column in self.column_data and version in self.column_data[column]:
                if row < len(self.column_data[column][version]):
                    item = QTableWidgetItem(self.column_data[column][version][row])
                    self.table_d.setItem(table_row, column, item)
        
        print(f"列 {column+1} 已切换到版本 {version}")


def main():
    app = QApplication(sys.argv)
    window = MainWindow()
    window.show()
    sys.exit(app.exec_())


if __name__ == '__main__':
    main()
```

## 极简设计的关键优点：

1. **完全使用标准组件**：
   - 使用普通的QTableWidget，没有自定义表头或视图
   - 使用标准的cellClicked信号处理点击事件
   - 不需要重写任何组件的绘制方法

2. **简洁明了的筛选按钮设计**：
   - 第一行作为筛选按钮行，显示"D1 ▼"，"D2 ▼"等
   - 筛选按钮单元格使用灰色背景和粗体文字以醒目显示
   - 点击第一行任意单元格显示对应列的筛选菜单

3. **高效的数据管理**：
   - 数据结构简单直观，版本信息易于管理
   - 切换版本时只更新必要的单元格
   - 数据从表格第二行开始显示，布局清晰

4. **极高的稳定性**：
   - 没有复杂的自定义绘制代码，降低崩溃风险
   - 事件处理逻辑简单，易于调试和维护
   - 使用PyQt标准事件机制，可靠性高

这个方案将筛选按钮集成到表格的第一行，而不是自定义表头或额外的控件，大大简化了程序结构，提高了代码的稳定性和可维护性，同时保持了您所期望的Excel式筛选功能。