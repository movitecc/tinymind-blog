---
title: py demo9
date: 2025-03-10T16:14:14.202Z
---

# PyQt5表格实现不同列默认时间点的筛选功能

根据您的新要求和截图，我将实现一个更复杂的表格筛选功能，其中不同的Dx列具有不同的默认时间点，但共享相同的筛选选项列表。以下是完整的代码实现：

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
        self.init_ui()  # 初始化UI界面

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
        table.setHorizontalHeaderLabels(headers)  # 设置表头文本

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
        dock = QDockWidget("表格D", self)  # 创建停靠窗口
        dock.setAllowedAreas(Qt.AllDockWidgetAreas)  # 允许停靠在任何区域

        # 创建一个容器来放置表格
        wrapper = QWidget()  # 创建容器控件
        layout = QVBoxLayout(wrapper)  # 垂直布局
        layout.setContentsMargins(0, 0, 0, 0)  # 设置边距为0
        layout.setSpacing(0)  # 设置间距为0
        
        # 创建标准表格，隐藏默认表头
        self.table_d = QTableWidget(21, 6)  # 1行筛选按钮 + 20行数据
        self.table_d.horizontalHeader().setVisible(False)  # 隐藏默认表头
        self.table_d.verticalHeader().setVisible(False)    # 隐藏行号
        
        # 设置表格样式
        self.table_d.setFrameShape(QTableWidget.NoFrame)  # 移除表格边框
        self.table_d.setShowGrid(True)  # 显示网格线
        
        # 定义所有可用的时间点（格式：月日_时分）
        self.time_points = ["0610_1045", "0610_1230", "0611_0930"]  # 所有可用时间点
        
        # 定义每列的初始时间点 - D1使用0610_1230，其他使用0610_1045
        self.default_time_points = {
            0: "0610_1230",  # D1列默认使用0610_1230
            1: "0610_1045",  # D2列默认使用0610_1045
            2: "0610_1045",  # D3列默认使用0610_1045
            3: "0610_1045",  # D4列默认使用0610_1045
            4: "0610_1045",  # D5列默认使用0610_1045
            5: "0610_1045",  # D6列默认使用0610_1045
        }
        
        # 为每个列设置当前选择的时间点（初始为默认时间点）
        self.current_time_points = self.default_time_points.copy()  # 复制默认设置作为当前设置
        
        # 定义每列每个时间点的数据
        self.column_data = {}  # 存储所有列的数据
        
        # 为每列创建数据
        for col in range(6):
            col_name = f"D{col+1}"  # 列名，如D1, D2...
            time_data = {}  # 存储该列不同时间点的数据
            
            # 为每个时间点创建该列的数据
            for time_point in self.time_points:
                # 每个时间点对应20行数据，格式为"Dx_时间点_行y"
                data_rows = [f"{col_name}_{time_point}_行{row}" for row in range(20)]
                time_data[time_point] = data_rows
                
            self.column_data[col] = time_data  # 存储该列的所有时间点数据
        
        # 初始化筛选按钮行和数据
        for col in range(6):
            # 创建筛选按钮单元格（第一行）
            filter_item = QTableWidgetItem(f"D{col+1} ▼")  # 创建带下拉箭头的表头项
            filter_item.setTextAlignment(Qt.AlignCenter)  # 文本居中对齐
            filter_item.setFlags(Qt.ItemIsEnabled)  # 设为可被点击但不可编辑
            
            # 设置字体为粗体
            font = QFont()
            font.setBold(True)
            filter_item.setFont(font)
            
            # 设置背景色
            filter_item.setBackground(Qt.lightGray)  # 设置浅灰色背景
            self.table_d.setItem(0, col, filter_item)  # 将项目放入表格第一行
            
            # 填充初始数据（使用当前列的默认时间点数据）
            current_time_point = self.current_time_points[col]
            for row in range(20):
                data_item = QTableWidgetItem(self.column_data[col][current_time_point][row])  # 创建数据项
                self.table_d.setItem(row+1, col, data_item)  # 数据从表格第二行开始
        
        # 设置第一行（筛选按钮行）的高度
        self.table_d.setRowHeight(0, 30)  # 设置为30像素高
        
        # 设置列宽度平均分布
        header = self.table_d.horizontalHeader()
        header.setSectionResizeMode(QHeaderView.Stretch)  # 列宽自动伸展填满表格
        
        # 连接单元格点击事件处理函数
        self.table_d.cellClicked.connect(self.handle_cell_click)
        
        # 将表格添加到布局
        layout.addWidget(self.table_d)
        dock.setWidget(wrapper)  # 将容器设置为停靠窗口的内容
        
        return dock
        
    def handle_cell_click(self, row, column):
        """处理单元格点击事件
        参数:
            row: 被点击的行索引
            column: 被点击的列索引
        """
        # 只有点击第一行（筛选按钮行）才处理
        if row == 0:
            self.show_filter_menu(column)  # 显示筛选菜单
    
    def show_filter_menu(self, column):
        """显示筛选菜单，提供时间点选择
        参数:
            column: 被点击的列索引
        """
        # 创建菜单
        menu = QMenu(self)
        
        # 添加所有可用的时间点选项
        for time_point in self.time_points:
            action = QAction(time_point, self)  # 创建菜单项
            # 使用lambda默认参数避免延迟绑定问题
            action.triggered.connect(lambda checked=False, col=column, tp=time_point: self.switch_time_point(col, tp))
            
            # 标记当前选中的时间点
            if self.current_time_points[column] == time_point:
                action.setIcon(self.style().standardIcon(self.style().SP_DialogApplyButton))  # 添加勾选图标
                
            menu.addAction(action)  # 将菜单项添加到菜单
        
        # 在鼠标位置显示菜单
        menu.exec_(QCursor.pos())  # 在鼠标位置显示菜单
    
    def switch_time_point(self, column, time_point):
        """切换到指定时间点的数据
        参数:
            column: 列索引
            time_point: 目标时间点
        """
        if column not in self.current_time_points:  # 检查列索引是否有效
            return
        
        # 更新当前时间点
        self.current_time_points[column] = time_point
        
        # 更新表格数据（从第二行开始）
        for row in range(20):
            table_row = row + 1  # 表格行索引（跳过筛选按钮行）
            
            # 确保列和时间点有效
            if column in self.column_data and time_point in self.column_data[column]:
                # 确保数据索引有效
                if row < len(self.column_data[column][time_point]):
                    # 创建新的单元格项
                    item = QTableWidgetItem(self.column_data[column][time_point][row])
                    # 更新表格中的项
                    self.table_d.setItem(table_row, column, item)
        
        print(f"列 {column+1} 已切换到时间点 {time_point}")  # 打印调试信息


def main():
    app = QApplication(sys.argv)  # 创建QApplication实例
    window = MainWindow()  # 创建主窗口
    window.show()  # 显示主窗口
    sys.exit(app.exec_())  # 进入事件循环


if __name__ == '__main__':
    main()  # 启动应用
```

## 主要功能说明

1. **不同列的默认时间点**：
   - 代码定义了一个`default_time_points`字典，为每列设置默认时间点
   - D1列的默认时间点为"0610_1230"
   - D2-D6列的默认时间点为"0610_1045"

2. **统一的筛选选项**：
   - 所有列共享相同的筛选选项："0610_1045", "0610_1230", "0611_0930"
   - 通过`self.time_points`列表定义所有可用时间点

3. **数据格式**：
   - 按照截图所示，数据格式为"Dx_时间点_行y"
   - 例如：D1_0610_1230_行0, D2_0610_1045_行1 等

4. **筛选菜单**：
   - 点击表头区域显示筛选菜单
   - 当前选中的时间点会在菜单中显示勾选标记
   - 切换时间点会刷新对应列的所有数据

5. **表格布局**：
   - 第一行用于显示带有下拉箭头的列标题
   - 从第二行开始显示实际数据
   - 表格使用灰色背景和粗体文字突出显示筛选按钮行

这个实现满足了您提出的更复杂需求，每列可以有不同的默认时间点，但共享相同的筛选选项列表，与截图中的效果一致。代码中的每一行都有详细注释，方便您理解和进一步修改。