---
title: Eeeeeeee
date: 2025-03-26T10:21:47.854Z
---

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>完整坐标计算器</title>
    <style>
        :root {
            --primary-color: #4361ee;
            --primary-light: rgba(67, 97, 238, 0.1);
            --secondary-color: #3f8efc;
            --accent-color: #2ec4b6;
            --highlight-color: #ff9e00;
            --highlight-bg: rgba(255, 158, 0, 0.15);
            --light-bg: #f8f9fa;
            --card-bg: #ffffff;
            --text-color: #333;
            --text-secondary: #666;
            --border-color: #e0e0e0;
            --border-radius: 8px;
            --box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05);
            --x-color: #4361ee;
            --y-color: #2ec4b6;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            line-height: 1.5;
            background-color: var(--light-bg);
            color: var(--text-color);
            padding: 15px;
        }
        
        .container {
            max-width: 900px;
            margin: 0 auto;
            background: var(--card-bg);
            padding: 20px;
            border-radius: var(--border-radius);
            box-shadow: var(--box-shadow);
        }
        
        h1 {
            text-align: center;
            color: var(--primary-color);
            margin-bottom: 20px;
            font-size: 1.8rem;
        }
        
        .card {
            background: var(--card-bg);
            border-radius: var(--border-radius);
            padding: 15px;
            margin-bottom: 15px;
            box-shadow: var(--box-shadow);
        }
        
        .card-header {
            display: flex;
            align-items: center;
            margin-bottom: 12px;
            padding-bottom: 8px;
            border-bottom: 1px solid var(--border-color);
            position: relative;
        }
        
        .card-title {
            font-size: 1.2rem;
            font-weight: 600;
            color: var(--primary-color);
            margin-left: 5px;
        }
        
        .card-actions {
            position: absolute;
            right: 0;
            top: 0;
            display: flex;
            gap: 10px;
        }
        
        .description {
            background-color: #f0f7ff;
            padding: 10px;
            border-radius: var(--border-radius);
            margin-bottom: 15px;
            border-left: 4px solid var(--secondary-color);
            color: var(--text-secondary);
            font-size: 0.9rem;
        }
        
        .formula {
            font-family: 'Courier New', monospace;
            font-weight: 500;
            margin: 0 3px;
        }
        
        .form-group {
            margin-bottom: 15px;
        }
        
        label {
            display: block;
            margin-bottom: 5px;
            font-weight: 500;
            color: var(--text-secondary);
            font-size: 0.9rem;
        }
        
        input {
            width: 100%;
            padding: 8px 10px;
            border: 1px solid #ddd;
            border-radius: 4px;
            font-size: 14px;
        }
        
        input:focus {
            outline: none;
            border-color: var(--secondary-color);
            box-shadow: 0 0 0 2px rgba(63, 142, 252, 0.2);
        }
        
        .row {
            display: flex;
            gap: 15px;
            margin-bottom: 8px;
        }
        
        .col {
            flex: 1;
        }
        
        .btn {
            padding: 8px 12px;
            border: none;
            border-radius: 4px;
            cursor: pointer;
            font-weight: 500;
            font-size: 0.9rem;
            transition: all 0.2s ease;
        }
        
        .btn-primary {
            background-color: var(--primary-color);
            color: white;
        }
        
        .btn-primary:hover {
            filter: brightness(1.1);
        }
        
        .btn-success {
            background-color: #38b000;
            color: white;
        }
        
        .btn-outline {
            background-color: transparent;
            border: 1px solid var(--border-color);
            color: var(--text-color);
        }
        
        .btn-outline:hover {
            background-color: var(--light-bg);
        }
        
        .btn-calculate {
            width: 100%;
            padding: 10px;
            font-size: 16px;
            font-weight: 600;
            margin-top: 10px;
        }
        
        .error {
            color: #e63946;
            background-color: rgba(230, 57, 70, 0.1);
            padding: 10px;
            border-radius: 4px;
            margin: 10px 0;
            border-left: 4px solid #e63946;
            font-size: 0.9rem;
            display: none;
        }
        
        .success-message {
            color: #38b000;
            background-color: rgba(56, 176, 0, 0.1);
            padding: 10px;
            border-radius: 4px;
            margin: 10px 0;
            border-left: 4px solid #38b000;
            font-size: 0.9rem;
            display: none;
        }
        
        .results-container {
            display: flex;
            flex-direction: column;
            gap: 15px;
            margin-top: 10px;
        }
        
        .result-summary {
            display: flex;
            gap: 15px;
            margin-bottom: 10px;
        }
        
        .result-box {
            flex: 1;
            padding: 12px;
            border-radius: var(--border-radius);
            text-align: center;
            font-weight: 600;
            box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05);
        }
        
        .x-result {
            background-color: rgba(67, 97, 238, 0.1);
            color: var(--x-color);
            border: 1px solid rgba(67, 97, 238, 0.3);
        }
        
        .y-result {
            background-color: rgba(46, 196, 182, 0.1);
            color: var(--y-color);
            border: 1px solid rgba(46, 196, 182, 0.3);
        }
        
        .result-section {
            margin-bottom: 20px;
        }
        
        .result-header {
            font-weight: 600;
            margin-bottom: 10px;
            display: flex;
            align-items: center;
            gap: 5px;
            color: var(--text-secondary);
            font-size: 0.9rem;
        }
        
        .x-series .result-header {
            color: var(--x-color);
        }
        
        .y-series .result-header {
            color: var(--y-color);
        }
        
        .grid-container {
            display: grid;
            grid-template-columns: repeat(5, 1fr);
            grid-template-rows: auto;
            gap: 8px;
            margin-bottom: 15px;
        }
        
        .grid-item {
            padding: 8px;
            background-color: #f8f9fa;
            border-radius: 4px;
            text-align: center;
            font-size: 0.9rem;
        }
        
        .grid-item.x-item {
            background-color: rgba(67, 97, 238, 0.1);
        }
        
        .grid-item.y-item {
            background-color: rgba(46, 196, 182, 0.1);
        }
        
        .grid-item.highlight {
            background-color: var(--highlight-bg);
            border: 1px solid var(--highlight-color);
            font-weight: bold;
            box-shadow: 0 0 6px rgba(255, 158, 0, 0.4);
        }
        
        .no-results {
            text-align: center;
            color: var(--text-secondary);
            padding: 10px;
            font-size: 0.9rem;
            font-style: italic;
        }
        
        .current-header {
            margin-top: 5px;
            color: var(--highlight-color);
            font-size: 0.9rem;
            font-weight: 600;
            display: flex;
            align-items: center;
            justify-content: center;
            margin-bottom: 10px;
        }
        
        .current-header::before,
        .current-header::after {
            content: "";
            flex: 1;
            height: 1px;
            background-color: rgba(255, 158, 0, 0.3);
            margin: 0 10px;
        }
        
        /* 保存设置对话框 */
        .settings-dialog {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background-color: rgba(0, 0, 0, 0.5);
            z-index: 1000;
            justify-content: center;
            align-items: center;
        }
        
        .dialog-content {
            background-color: white;
            padding: 20px;
            border-radius: var(--border-radius);
            width: 90%;
            max-width: 450px;
            box-shadow: 0 5px 20px rgba(0, 0, 0, 0.2);
        }
        
        .dialog-header {
            display: flex;
            align-items: center;
            justify-content: space-between;
            margin-bottom: 15px;
            padding-bottom: 8px;
            border-bottom: 1px solid var(--border-color);
        }
        
        .dialog-title {
            font-size: 1.1rem;
            font-weight: 600;
            color: var(--primary-color);
            margin: 0;
        }
        
        .dialog-close {
            background: none;
            border: none;
            font-size: 1.3rem;
            cursor: pointer;
            color: #888;
        }
        
        .dialog-close:hover {
            color: #e63946;
        }
        
        .dialog-footer {
            display: flex;
            justify-content: flex-end;
            gap: 10px;
            margin-top: 15px;
        }
        
        .settings-list {
            max-height: 250px;
            overflow-y: auto;
            border: 1px solid var(--border-color);
            border-radius: 4px;
        }
        
        .setting-item {
            padding: 10px;
            border-bottom: 1px solid var(--border-color);
            display: flex;
            align-items: center;
            justify-content: space-between;
        }
        
        .setting-item:last-child {
            border-bottom: none;
        }
        
        .setting-info {
            flex: 1;
            cursor: pointer;
        }
        
        .setting-name {
            font-weight: 500;
            font-size: 0.9rem;
        }
        
        .setting-details {
            font-size: 0.8rem;
            color: #888;
        }
        
        .setting-actions {
            display: flex;
            gap: 5px;
        }
        
        .setting-actions button {
            width: 28px;
            height: 28px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 0;
            min-width: 0;
            font-size: 0.8rem;
        }
        
        .settings-empty {
            text-align: center;
            padding: 15px;
            color: #888;
            font-size: 0.9rem;
        }
        
        @media (max-width: 700px) {
            .row {
                flex-direction: column;
                gap: 10px;
            }
            
            .result-summary {
                flex-direction: column;
                gap: 10px;
            }
            
            .grid-container {
                grid-template-columns: repeat(3, 1fr);
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>完整坐标计算器</h1>
        
        <div class="description">
            本计算器基于独立的X和Y坐标系公式: <span class="formula">x = a + step_x × n</span> 和 <span class="formula">y = b + step_y × n</span>，生成与当前坐标相近的值序列。
        </div>
        
        <div class="card">
            <div class="card-header">
                <div class="card-title">初始设置</div>
                <div class="card-actions">
                    <button class="btn btn-success" onclick="showSaveDialog()">💾</button>
                    <button class="btn btn-outline" onclick="showSettingsDialog()">📋</button>
                </div>
            </div>
            
            <div class="row">
                <div class="col">
                    <div class="form-group">
                        <label for="initialX">初始X坐标 (a):</label>
                        <input type="number" id="initialX" value="0" min="0" step="any" onfocus="this.select()">
                    </div>
                </div>
                <div class="col">
                    <div class="form-group">
                        <label for="stepX">X步长 (step_x):</label>
                        <input type="number" id="stepX" value="1" min="0" step="any" onfocus="this.select()">
                    </div>
                </div>
            </div>
            
            <div class="row">
                <div class="col">
                    <div class="form-group">
                        <label for="initialY">初始Y坐标 (b):</label>
                        <input type="number" id="initialY" value="0" min="0" step="any" onfocus="this.select()">
                    </div>
                </div>
                <div class="col">
                    <div class="form-group">
                        <label for="stepY">Y步长 (step_y):</label>
                        <input type="number" id="stepY" value="1" min="0" step="any" onfocus="this.select()">
                    </div>
                </div>
            </div>
            
            <div id="successMessage" class="success-message"></div>
        </div>
        
        <div class="card">
            <div class="card-header">
                <div class="card-title">当前坐标</div>
            </div>
            
            <div class="row">
                <div class="col">
                    <div class="form-group">
                        <label for="inputX">当前X坐标:</label>
                        <input type="number" id="inputX" min="0" step="any" placeholder="输入X坐标" onfocus="this.select()">
                    </div>
                </div>
                <div class="col">
                    <div class="form-group">
                        <label for="inputY">当前Y坐标:</label>
                        <input type="number" id="inputY" min="0" step="any" placeholder="输入Y坐标" onfocus="this.select()">
                    </div>
                </div>
            </div>
            
            <button class="btn btn-primary btn-calculate" onclick="calculate()">生成坐标系列</button>
        </div>
        
        <div id="errorMessage" class="error"></div>
        
        <div id="results" class="results-container" style="display: none;">
            <!-- 结果摘要 -->
            <div class="result-summary" id="resultSummary"></div>
            
            <!-- X坐标系列 -->
            <div class="result-section x-series" id="xSeriesSection" style="display:none;">
                <div class="current-header">当前最匹配X值</div>
                <div class="grid-container" id="currentXGrid"></div>
                
                <div class="result-header">📉 小于当前X坐标的系列</div>
                <div class="grid-container" id="smallerXGrid"></div>
                
                <div class="result-header">📈 大于当前X坐标的系列</div>
                <div class="grid-container" id="largerXGrid"></div>
            </div>
            
            <!-- Y坐标系列 -->
            <div class="result-section y-series" id="ySeriesSection" style="display:none;">
                <div class="current-header">当前最匹配Y值</div>
                <div class="grid-container" id="currentYGrid"></div>
                
                <div class="result-header">📉 小于当前Y坐标的系列</div>
                <div class="grid-container" id="smallerYGrid"></div>
                
                <div class="result-header">📈 大于当前Y坐标的系列</div>
                <div class="grid-container" id="largerYGrid"></div>
            </div>
        </div>
    </div>
    
    <!-- 保存设置对话框 -->
    <div id="saveSettingDialog" class="settings-dialog">
        <div class="dialog-content">
            <div class="dialog-header">
                <h3 class="dialog-title">保存当前设置</h3>
                <button class="dialog-close" onclick="closeDialog('saveSettingDialog')">&times;</button>
            </div>
            
            <div class="form-group">
                <label for="settingName">设置名称:</label>
                <input type="text" id="settingName" placeholder="例如：项目A坐标" onfocus="this.select()">
            </div>
            
            <div class="dialog-footer">
                <button class="btn btn-primary" onclick="saveSetting()">保存</button>
                <button class="btn btn-outline" onclick="closeDialog('saveSettingDialog')">取消</button>
            </div>
        </div>
    </div>
    
    <!-- 选择设置对话框 -->
    <div id="selectSettingDialog" class="settings-dialog">
        <div class="dialog-content">
            <div class="dialog-header">
                <h3 class="dialog-title">已保存的设置</h3>
                <button class="dialog-close" onclick="closeDialog('selectSettingDialog')">&times;</button>
            </div>
            
            <div id="savedSettingsList" class="settings-list">
                <div class="settings-empty">加载中...</div>
            </div>
            
            <div class="dialog-footer">
                <button class="btn btn-outline" onclick="closeDialog('selectSettingDialog')">关闭</button>
            </div>
        </div>
    </div>

    <script>
        // 页面加载完成后初始化
        document.addEventListener('DOMContentLoaded', function() {
            // 检查是否支持localStorage
            if (!storageAvailable('localStorage')) {
                showError('您的浏览器不支持本地存储，无法保存设置。');
            }
            
            // 为输入框添加回车键事件
            const inputX = document.getElementById('inputX');
            const inputY = document.getElementById('inputY');
            const settingName = document.getElementById('settingName');
            
            // 为坐标输入框添加回车事件
            inputX.addEventListener('keypress', function(e) {
                if (e.key === 'Enter') {
                    calculate();
                }
            });
            
            inputY.addEventListener('keypress', function(e) {
                if (e.key === 'Enter') {
                    calculate();
                }
            });
            
            // 为设置名称输入框添加回车事件
            settingName.addEventListener('keypress', function(e) {
                if (e.key === 'Enter') {
                    saveSetting();
                }
            });
        });
        
        // 检查浏览器是否支持localStorage
        function storageAvailable(type) {
            try {
                var storage = window[type],
                    x = '__storage_test__';
                storage.setItem(x, x);
                storage.removeItem(x);
                return true;
            } catch(e) {
                return false;
            }
        }
        
        function showError(message) {
            const errorElement = document.getElementById('errorMessage');
            errorElement.textContent = message;
            errorElement.style.display = 'block';
            document.getElementById('results').style.display = 'none';
        }
        
        function hideError() {
            document.getElementById('errorMessage').style.display = 'none';
        }
        
        function showSuccessMessage(message) {
            const successElement = document.getElementById('successMessage');
            successElement.textContent = message;
            successElement.style.display = 'block';
            
            // 3秒后自动隐藏消息
            setTimeout(() => {
                successElement.style.display = 'none';
            }, 3000);
        }
        
        function validatePositive(value, name) {
            if (value < 0) {
                return `${name} 必须为正数或零`;
            }
            return null;
        }
        
        // 打开保存设置对话框
        function showSaveDialog() {
            document.getElementById('saveSettingDialog').style.display = 'flex';
            document.getElementById('settingName').focus();
        }
        
        // 打开选择设置对话框
        function showSettingsDialog() {
            const dialog = document.getElementById('selectSettingDialog');
            dialog.style.display = 'flex';
            
            // 加载已保存的设置
            loadSavedSettings();
        }
        
        // 关闭对话框
        function closeDialog(dialogId) {
            document.getElementById(dialogId).style.display = 'none';
        }
        
        // 保存当前设置
        function saveSetting() {
            const settingName = document.getElementById('settingName').value.trim();
            if (!settingName) {
                alert('请输入设置名称');
                return;
            }
            
            // 获取当前设置值
            const initialX = parseFloat(document.getElementById('initialX').value);
            const stepX = parseFloat(document.getElementById('stepX').value);
            const initialY = parseFloat(document.getElementById('initialY').value);
            const stepY = parseFloat(document.getElementById('stepY').value);
            
            // 验证值是否合法
            if (isNaN(initialX) || isNaN(stepX) || isNaN(initialY) || isNaN(stepY)) {
                alert('请确保所有值都是有效的数字');
                return;
            }
            
            // 创建设置对象
            const setting = {
                name: settingName,
                initialX: initialX,
                stepX: stepX,
                initialY: initialY,
                stepY: stepY,
                timestamp: new Date().getTime()
            };
            
            // 获取已保存的设置
            let savedSettings = JSON.parse(localStorage.getItem('coordSettings')) || [];
            
            // 检查是否已存在同名设置
            const existingIndex = savedSettings.findIndex(s => s.name === settingName);
            if (existingIndex !== -1) {
                if (!confirm(`已存在名为"${settingName}"的设置，是否覆盖？`)) {
                    return;
                }
                savedSettings[existingIndex] = setting;
            } else {
                savedSettings.push(setting);
            }
            
            // 保存到localStorage
            localStorage.setItem('coordSettings', JSON.stringify(savedSettings));
            
            // 关闭对话框并显示成功消息
            closeDialog('saveSettingDialog');
            document.getElementById('settingName').value = '';
            showSuccessMessage(`设置 "${settingName}" 已成功保存`);
        }
        
        // 加载已保存的设置到设置列表
        function loadSavedSettings() {
            const settingsList = document.getElementById('savedSettingsList');
            const savedSettings = JSON.parse(localStorage.getItem('coordSettings')) || [];
            
            if (savedSettings.length === 0) {
                settingsList.innerHTML = `
                    <div class="settings-empty">
                        没有找到已保存的设置
                    </div>
                `;
                return;
            }
            
            // 按照名称排序
            savedSettings.sort((a, b) => a.name.localeCompare(b.name, 'zh-CN'));
            
            // 构建设置列表
            let html = '';
            savedSettings.forEach(setting => {
                html += `
                    <div class="setting-item">
                        <div class="setting-info" onclick="applySetting('${setting.name}')">
                            <div class="setting-name">${setting.name}</div>
                            <div class="setting-details">
                                X(${setting.initialX}, ${setting.stepX}) Y(${setting.initialY}, ${setting.stepY})
                            </div>
                        </div>
                        <div class="setting-actions">
                            <button class="btn btn-success" onclick="applySetting('${setting.name}')" title="应用">✓</button>
                            <button class="btn btn-danger" onclick="deleteSetting('${setting.name}')" title="删除">×</button>
                        </div>
                    </div>
                `;
            });
            
            settingsList.innerHTML = html;
        }
        
        // 应用选择的设置
        function applySetting(name) {
            const savedSettings = JSON.parse(localStorage.getItem('coordSettings')) || [];
            const setting = savedSettings.find(s => s.name === name);
            
            if (setting) {
                document.getElementById('initialX').value = setting.initialX;
                document.getElementById('stepX').value = setting.stepX;
                document.getElementById('initialY').value = setting.initialY;
                document.getElementById('stepY').value = setting.stepY;
                
                closeDialog('selectSettingDialog');
                showSuccessMessage(`已应用设置 "${name}"`);
            } else {
                alert('未找到指定的设置');
            }
        }
        
        // 删除设置
        function deleteSetting(name) {
            if (confirm(`确定要删除设置 "${name}" 吗？`)) {
                let savedSettings = JSON.parse(localStorage.getItem('coordSettings')) || [];
                savedSettings = savedSettings.filter(s => s.name !== name);
                localStorage.setItem('coordSettings', JSON.stringify(savedSettings));
                
                // 重新加载设置列表
                loadSavedSettings();
                showSuccessMessage(`设置 "${name}" 已删除`);
            }
        }
        
        // 计算最接近给定值的整数n
        function findClosestN(value, initial, step) {
            // 如果步长为0，避免除以0错误
            if (step === 0) return 0;
            
            // 计算精确的n值
            const exactN = (value - initial) / step;
            
            // 找到最接近的整数
            return Math.round(exactN);
        }
        
        // 创建网格项
        function createGridItem(value, itemClass, highlight = false) {
            const gridItem = document.createElement('div');
            gridItem.className = `grid-item ${itemClass}${highlight ? ' highlight' : ''}`;
            gridItem.textContent = value.toFixed(4);
            return gridItem;
        }
        
        // 填充结果网格
        function fillGrid(gridId, results, itemClass, emptyMessage = '没有数据') {
            const grid = document.getElementById(gridId);
            grid.innerHTML = '';
            
            if (results.length === 0) {
                const emptyItem = document.createElement('div');
                emptyItem.className = 'no-results';
                emptyItem.textContent = emptyMessage;
                emptyItem.style.gridColumn = '1 / span 5';
                grid.appendChild(emptyItem);
                return;
            }
            
            // 显示结果
            for (const result of results) {
                grid.appendChild(createGridItem(result, itemClass));
            }
        }
        
        function calculate() {
            hideError();
            
            // 获取初始值和步长
            const initialX = parseFloat(document.getElementById('initialX').value);
            const stepX = parseFloat(document.getElementById('stepX').value);
            const initialY = parseFloat(document.getElementById('initialY').value);
            const stepY = parseFloat(document.getElementById('stepY').value);
            
            // 获取当前值 (可能为空)
            const inputXElem = document.getElementById('inputX');
            const inputYElem = document.getElementById('inputY');
            
            const hasInputX = inputXElem.value.trim() !== '';
            const hasInputY = inputYElem.value.trim() !== '';
            
            // 验证至少有一个坐标
            if (!hasInputX && !hasInputY) {
                showError('请至少输入一个坐标值（X或Y）');
                return;
            }
            
            // 解析有输入的坐标
            const inputX = hasInputX ? parseFloat(inputXElem.value) : null;
            const inputY = hasInputY ? parseFloat(inputYElem.value) : null;
            
            // 验证所有输入的值是否为数字
            if (isNaN(initialX) || isNaN(stepX) || isNaN(initialY) || isNaN(stepY) || 
                (hasInputX && isNaN(inputX)) || (hasInputY && isNaN(inputY))) {
                showError('请输入有效的数值');
                return;
            }
            
            // 验证所有值为正数或零
            let errorMsg = validatePositive(initialX, '初始X坐标') || 
                          validatePositive(stepX, 'X步长') || 
                          validatePositive(initialY, '初始Y坐标') || 
                          validatePositive(stepY, 'Y步长');
            
            if (hasInputX) {
                errorMsg = errorMsg || validatePositive(inputX, '当前X坐标');
            }
            
            if (hasInputY) {
                errorMsg = errorMsg || validatePositive(inputY, '当前Y坐标');
            }
            
            if (errorMsg) {
                showError(errorMsg);
                return;
            }
            
            // 验证步长不为零
            if ((hasInputX && stepX === 0) || (hasInputY && stepY === 0)) {
                showError('步长不能为0');
                return;
            }
            
            // 初始化结果部分
            let resultSummaryHtml = '';
            document.getElementById('xSeriesSection').style.display = 'none';
            document.getElementById('ySeriesSection').style.display = 'none';
            
            // 处理X坐标
            if (hasInputX) {
                const nX = findClosestN(inputX, initialX, stepX);
                const closestX = initialX + stepX * nX;
                
                // 添加X结果摘要
                resultSummaryHtml += `
                    <div class="result-box x-result">
                        X: ${inputX} → 最接近值: ${closestX.toFixed(4)}
                    </div>
                `;
                
                // 显示当前最匹配值
                const currentXGrid = document.getElementById('currentXGrid');
                currentXGrid.innerHTML = '';
                currentXGrid.appendChild(createGridItem(closestX, 'x-item', true));
                
                // 计算小于的X系列
                const smallerXValues = [];
                for (let i = nX - 1; smallerXValues.length < 10 && i >= 0; i--) {
                    const x = initialX + stepX * i;
                    if (x < 0) continue;
                    smallerXValues.unshift(x); // 从小到大排序
                }
                
                // 计算大于的X系列
                const largerXValues = [];
                for (let i = nX + 1; largerXValues.length < 10; i++) {
                    const x = initialX + stepX * i;
                    if (x < 0) continue;
                    largerXValues.push(x);
                }
                
                // 填充X结果网格
                fillGrid('smallerXGrid', smallerXValues, 'x-item', '没有小于当前X的系列值');
                fillGrid('largerXGrid', largerXValues, 'x-item', '没有大于当前X的系列值');
                
                // 显示X结果部分
                document.getElementById('xSeriesSection').style.display = 'block';
            }
            
            // 处理Y坐标
            if (hasInputY) {
                const nY = findClosestN(inputY, initialY, stepY);
                const closestY = initialY + stepY * nY;
                
                // 添加Y结果摘要
                resultSummaryHtml += `
                    <div class="result-box y-result">
                        Y: ${inputY} → 最接近值: ${closestY.toFixed(4)}
                    </div>
                `;
                
                // 显示当前最匹配值
                const currentYGrid = document.getElementById('currentYGrid');
                currentYGrid.innerHTML = '';
                currentYGrid.appendChild(createGridItem(closestY, 'y-item', true));
                
                // 计算小于的Y系列
                const smallerYValues = [];
                for (let i = nY - 1; smallerYValues.length < 10 && i >= 0; i--) {
                    const y = initialY + stepY * i;
                    if (y < 0) continue;
                    smallerYValues.unshift(y); // 从小到大排序
                }
                
                // 计算大于的Y系列
                const largerYValues = [];
                for (let i = nY + 1; largerYValues.length < 10; i++) {
                    const y = initialY + stepY * i;
                    if (y < 0) continue;
                    largerYValues.push(y);
                }
                
                // 填充Y结果网格
                fillGrid('smallerYGrid', smallerYValues, 'y-item', '没有小于当前Y的系列值');
                fillGrid('largerYGrid', largerYValues, 'y-item', '没有大于当前Y的系列值');
                
                // 显示Y结果部分
                document.getElementById('ySeriesSection').style.display = 'block';
            }
            
            // 更新结果摘要
            document.getElementById('resultSummary').innerHTML = resultSummaryHtml;
            
            // 显示结果区域
            document.getElementById('results').style.display = 'block';
            
            // 平滑滚动到结果区域
            document.getElementById('results').scrollIntoView({ behavior: 'smooth' });
        }
    </script>
</body>
</html>
```