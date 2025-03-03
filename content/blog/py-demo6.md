---
title: py demo6
date: 2025-03-02T14:59:58.485Z
---


```python 

import sys
from PyQt5.QtWidgets import (QApplication, QMainWindow, QWidget, QProgressBar, 
                            QTableWidget, QTableWidgetItem, QHeaderView, QDockWidget, 
                            QVBoxLayout, QSizePolicy)
from PyQt5.QtCore import Qt, QEvent, QTimer

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
、、、
        
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
        """创建并返回停靠窗口D，其中包含表格"""
        dock = QDockWidget("表格D", self)
        dock.setAllowedAreas(Qt.AllDockWidgetAreas)
        
        # 使用通用方法创建表格
        headers = ['D1', 'D2', 'D3', 'D4', 'D5', 'D6']
        table = self.create_table_widget(28, 6, headers)
        
        # 填充示例数据
        for row in range(28):
            for col in range(6):
                item = QTableWidgetItem(f'D-{row},{col}')
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

def main():
    app = QApplication(sys.argv)
    window = MainWindow()
    window.show()
    sys.exit(app.exec_())

if __name__ == '__main__':
    main()

```