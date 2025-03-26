---
title: Ccccccccc
date: 2025-03-26T07:27:54.961Z
---

```html
<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>独立坐标计算器</title>
    <style>
        :root {
            --primary-color: #4361ee;
            --primary-dark: #3a56d4;
            --primary-light: rgba(67, 97, 238, 0.1);
            --secondary-color: #3f8efc;
            --accent-color: #2ec4b6;
            --light-bg: #f8f9fa;
            --card-bg: #ffffff;
            --text-color: #333;
            --text-secondary: #666;
            --text-muted: #888;
            --border-radius: 10px;
            --box-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
            --transition: all 0.3s ease;
            --success-color: #38b000;
            --info-color: #ffb703;
            --danger-color: #e63946;
            --danger-light: rgba(230, 57, 70, 0.1);
            --border-color: #e0e0e0;
            --warning-color: #ff9e00;
            --warning-light: rgba(255, 158, 0, 0.1);
            --formula-bg: #f5f8ff;
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            line-height: 1.6;
            background-color: var(--light-bg);
            color: var(--text-color);
            padding: 20px;
        }
        
        .container {
            max-width: 900px;
            margin: 0 auto;
            background: var(--card-bg);
            padding: 30px;
            border-radius: var(--border-radius);
            box-shadow: var(--box-shadow);
        }
        
        h1 {
            text-align: center;
            color: var(--primary-color);
            margin-bottom: 25px;
            font-size: 2.2rem;
        }
        
        .card {
            background: var(--card-bg);
            border-radius: var(--border-radius);
            padding: 25px;
            margin-bottom: 25px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05);
            transition: var(--transition);
        }
        
        .card:hover {
            box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
        }
        
        .card-header {
            display: flex;
            align-items: center;
            margin-bottom: 20px;
            padding-bottom: 10px;
            border-bottom: 1px solid #eee;
            position: relative;
        }
        
        .card-icon {
            width: 30px;
            height: 30px;
            display: flex;
            align-items: center;
            justify-content: center;
            background-color: var(--primary-color);
            color: white;
            border-radius: 50%;
            margin-right: 10px;
        }
        
        .card-title {
            font-size: 1.3rem;
            font-weight: 600;
            color: var(--primary-color);
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
            padding: 15px;
            border-radius: var(--border-radius);
            margin-bottom: 25px;
            border-left: 4px solid var(--secondary-color);
            color: var(--text-secondary);
        }
        
        .tip {
            background-color: #fff8e6;
            padding: 12px;
            border-radius: 6px;
            margin: 15px 0;
            border-left: 4px solid var(--info-color);
            color: var(--text-secondary);
            font-size: 0.9rem;
        }
        
        .warning {
            background-color: var(--warning-light);
            padding: 12px;
            border-radius: 6px;
            margin: 15px 0;
            border-left: 4px solid var(--warning-color);
            color: var(--text-secondary);
            font-size: 0.9rem;
        }
        
        .formula-box {
            background-color: var(--formula-bg);
            padding: 12px 15px;
            border-radius: 6px;
            margin: 15px 0;
            border-left: 4px solid var(--primary-color);
            color: var(--text-color);
            font-family: 'Courier New', monospace;
            font-weight: 500;
            font-size: 1rem;
            display: flex;
            flex-direction: column;
            gap: 8px;
        }
        
        .formula-title {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            font-weight: 600;
            font-size: 0.9rem;
            color: var(--text-secondary);
            margin-bottom: 5px;
        }
        
        .form-group {
            margin-bottom: 20px;
        }
        
        label {
            display: block;
            margin-bottom: 8px;
            font-weight: 500;
            color: var(--text-secondary);
        }
        
        input {
            width: 100%;
            padding: 12px;
            border: 1px solid #ddd;
            border-radius: 6px;
            font-size: 16px;
            transition: var(--transition);
        }
        
        input:focus {
            outline: none;
            border-color: var(--secondary-color);
            box-shadow: 0 0 0 3px rgba(63, 142, 252, 0.2);
        }
        
        .row {
            display: flex;
            gap: 20px;
            margin-bottom: 10px;
        }
        
        .col {
            flex: 1;
        }
        
        .btn {
            padding: 10px 15px;
            border: none;
            border-radius: 6px;
            cursor: pointer;
            font-weight: 500;
            transition: var(--transition);
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 5px;
        }
        
        .btn-primary {
            background-color: var(--primary-color);
            color: white;
            box-shadow: 0 4px 10px rgba(67, 97, 238, 0.3);
        }
        
        .btn-primary:hover {
            background-color: var(--primary-dark);
            transform: translateY(-2px);
            box-shadow: 0 6px 15px rgba(67, 97, 238, 0.4);
        }
        
        .btn-primary:active {
            transform: translateY(0);
            box-shadow: 0 2px 5px rgba(67, 97, 238, 0.4);
        }
        
        .btn-calculate {
            width: 100%;
            padding: 14px;
            font-size: 18px;
            font-weight: 600;
            letter-spacing: 0.5px;
            margin-top: 15px;
        }
        
        .btn-success {
            background-color: var(--success-color);
            color: white;
        }
        
        .btn-success:hover {
            background-color: #2e9000;
            transform: translateY(-1px);
        }
        
        .btn-danger {
            background-color: var(--danger-color);
            color: white;
        }
        
        .btn-danger:hover {
            background-color: #d32f2f;
            transform: translateY(-1px);
        }
        
        .btn-outline {
            background-color: transparent;
            border: 1px solid var(--border-color);
            color: var(--text-color);
        }
        
        .btn-outline:hover {
            background-color: var(--light-bg);
            border-color: var(--text-secondary);
        }
        
        .error {
            color: var(--danger-color);
            background-color: var(--danger-light);
            padding: 15px;
            border-radius: 6px;
            margin: 20px 0;
            border-left: 4px solid var(--danger-color);
            display: none;
        }
        
        .success-message {
            color: var(--success-color);
            background-color: rgba(56, 176, 0, 0.1);
            padding: 15px;
            border-radius: 6px;
            margin: 20px 0;
            border-left: 4px solid var(--success-color);
            display: none;
        }
        
        .results-container {
            display: flex;
            flex-direction: column;
            gap: 25px;
        }
        
        .current-value {
            padding: 15px;
            background-color: #e7f3ff;
            border-radius: var(--border-radius);
            text-align: center;
            font-weight: 500;
            color: var(--primary-color);
            margin: 10px 0 20px 0;
            border: 1px solid #c1ddff;
            font-size: 1rem;
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
        }
        
        .input-status {
            display: inline-block;
            font-size: 0.8rem;
            padding: 3px 8px;
            border-radius: 12px;
            margin-left: 8px;
            color: white;
            background-color: var(--success-color);
        }
        
        .calculation-method {
            font-size: 0.85rem;
            color: var(--text-secondary);
            margin: 5px 0;
        }
        
        .badge {
            display: inline-block;
            font-size: 0.75rem;
            padding: 2px 6px;
            border-radius: 4px;
            margin-left: 4px;
            font-weight: 500;
        }
        
        .badge-input {
            background-color: var(--info-color);
            color: white;
        }
        
        .badge-calculated {
            background-color: var(--primary-color);
            color: white;
        }
        
        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 10px;
            box-shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
            border-radius: 8px;
            overflow: hidden;
        }
        
        th, td {
            padding: 12px;
            text-align: center;
        }
        
        th {
            background-color: var(--primary-color);
            color: white;
            font-weight: 500;
        }
        
        tr:nth-child(even) {
            background-color: #f0f7ff;
        }
        
        tr:hover {
            background-color: #e2eeff;
        }
        
        .result-section {
            background-color: var(--card-bg);
            border-radius: var(--border-radius);
            padding: 20px;
            box-shadow: 0 2px 10px rgba(0, 0, 0, 0.05);
        }
        
        .result-header {
            display: flex;
            align-items: center;
            margin-bottom: 15px;
        }
        
        .result-icon {
            width: 24px;
            height: 24px;
            display: flex;
            align-items: center;
            justify-content: center;
            background-color: var(--accent-color);
            color: white;
            border-radius: 50%;
            margin-right: 10px;
        }
        
        .result-title {
            font-size: 1.1rem;
            font-weight: 600;
            color: var(--accent-color);
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
            padding: 25px;
            border-radius: var(--border-radius);
            width: 90%;
            max-width: 500px;
            box-shadow: 0 5px 20px rgba(0, 0, 0, 0.2);
        }
        
        .dialog-header {
            display: flex;
            align-items: center;
            justify-content: space-between;
            margin-bottom: 20px;
            padding-bottom: 10px;
            border-bottom: 1px solid var(--border-color);
        }
        
        .dialog-title {
            font-size: 1.2rem;
            font-weight: 600;
            color: var(--primary-color);
            margin: 0;
        }
        
        .dialog-close {
            background: none;
            border: none;
            font-size: 1.5rem;
            cursor: pointer;
            color: var(--text-muted);
        }
        
        .dialog-close:hover {
            color: var(--danger-color);
        }
        
        .dialog-footer {
            display: flex;
            justify-content: flex-end;
            gap: 10px;
            margin-top: 20px;
        }
        
        /* 保存的设置列表样式 */
        .saved-settings {
            margin-top: 20px;
        }
        
        .settings-header {
            display: flex;
            align-items: center;
            justify-content: space-between;
            margin-bottom: 15px;
        }
        
        .settings-title {
            font-size: 1.1rem;
            font-weight: 600;
            color: var(--text-color);
        }
        
        .settings-list {
            max-height: 250px;
            overflow-y: auto;
            border: 1px solid var(--border-color);
            border-radius: 6px;
        }
        
        .setting-item {
            padding: 12px 15px;
            border-bottom: 1px solid var(--border-color);
            display: flex;
            align-items: center;
            justify-content: space-between;
            transition: background-color 0.2s;
        }
        
        .setting-item:last-child {
            border-bottom: none;
        }
        
        .setting-item:hover {
            background-color: var(--primary-light);
        }
        
        .setting-info {
            flex: 1;
            cursor: pointer;
        }
        
        .setting-name {
            font-weight: 500;
            margin-bottom: 3px;
        }
        
        .setting-details {
            font-size: 0.85rem;
            color: var(--text-muted);
        }
        
        .setting-actions {
            display: flex;
            gap: 8px;
        }
        
        .setting-actions button {
            width: 30px;
            height: 30px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            padding: 0;
            min-width: 0;
            font-size: 0.85rem;
        }
        
        .no-settings {
            padding: 20px;
            text-align: center;
            color: var(--text-muted);
        }
        
        .settings-empty {
            text-align: center;
            padding: 25px;
            color: var(--text-muted);
        }
        
        /* 结果距离信息样式 */
        .distance-info {
            font-size: 0.9rem;
            color: var(--text-muted);
            margin-top: 5px;
        }
        
        .close-match {
            color: var(--success-color);
            font-weight: 500;
        }
        
        .distant-match {
            color: var(--warning-color);
            font-weight: 500;
        }
        
        /* 单独值显示样式 */
        .calculation-details {
            margin-top: 5px;
            font-size: 0.85rem;
            color: var(--text-secondary);
        }
        
        .input-coordinate {
            margin-bottom: 8px;
        }
        
        .calculated-coordinate {
            color: var(--primary-color);
        }
        
        .explanation {
            margin-top: 12px;
            padding: 8px 12px;
            background-color: rgba(46, 196, 182, 0.1);
            border-radius: 6px;
            font-size: 0.85rem;
            color: var(--text-secondary);
            border-left: 3px solid var(--accent-color);
        }
        
        .coordinate-pair {
            display: inline-block;
            padding: 3px 6px;
            background-color: rgba(63, 142, 252, 0.1);
            border-radius: 4px;
            margin: 0 2px;
        }

        .tab-container {
            margin-bottom: 15px;
        }
        
        .tab-buttons {
            display: flex;
            border-bottom: 1px solid #ddd;
        }
        
        .tab-button {
            padding: 10px 20px;
            background: none;
            border: none;
            border-bottom: 2px solid transparent;
            cursor: pointer;
            font-weight: 500;
            color: var(--text-secondary);
            transition: all 0.3s ease;
        }
        
        .tab-button.active {
            color: var(--primary-color);
            border-bottom: 2px solid var(--primary-color);
        }
        
        .tab-content {
            display: none;
            padding: 15px 0;
        }
        
        .tab-content.active {
            display: block;
        }
        
        .result-value {
            font-weight: 600;
            color: var(--primary-color);
        }
        
        @media (max-width: 768px) {
            .row {
                flex-direction: column;
                gap: 10px;
            }
            
            .container {
                padding: 20px;
            }
            
            .dialog-content {
                padding: 15px;
            }
            
            .setting-item {
                flex-direction: column;
                align-items: stretch;
            }
            
            .setting-actions {
                justify-content: flex-end;
                margin-top: 10px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>独立坐标计算器</h1>
        
        <div class="description">
            <p>本计算器基于两个独立的公式计算X和Y坐标序列。每个坐标轴有自己独立的n值。</p>
            <div class="formula-box">
                <div class="formula-title">计算公式:</div>
                <div>x = a + step_x × n_x (X轴)</div>
                <div>y = b + step_y × n_y (Y轴)</div>
            </div>
        </div>
        
        <div class="card">
            <div class="card-header">
                <div class="card-icon">⚙️</div>
                <div class="card-title">初始设置</div>
                <div class="card-actions">
                    <button class="btn btn-success" onclick="showSaveDialog()">💾 保存设置</button>
                    <button class="btn btn-outline" onclick="showSettingsDialog()">📋 选择设置</button>
                </div>
            </div>
            
            <div class="row">
                <div class="col">
                    <div class="form-group">
                        <label for="initialX">初始X坐标 (a):</label>
                        <input type="number" id="initialX" value="0" min="0" step="any" placeholder="设置起始X值">
                    </div>
                </div>
                <div class="col">
                    <div class="form-group">
                        <label for="stepX">X步长 (step_x):</label>
                        <input type="number" id="stepX" value="1" min="0" step="any" placeholder="设置X增量">
                    </div>
                </div>
            </div>
            
            <div class="row">
                <div class="col">
                    <div class="form-group">
                        <label for="initialY">初始Y坐标 (b):</label>
                        <input type="number" id="initialY" value="0" min="0" step="any" placeholder="设置起始Y值">
                    </div>
                </div>
                <div class="col">
                    <div class="form-group">
                        <label for="stepY">Y步长 (step_y):</label>
                        <input type="number" id="stepY" value="1" min="0" step="any" placeholder="设置Y增量">
                    </div>
                </div>
            </div>
            
            <div id="successMessage" class="success-message"></div>
        </div>
        
        <div class="card">
            <div class="card-header">
                <div class="card-icon">📍</div>
                <div class="card-title">当前坐标</div>
            </div>
            
            <div class="tip">
                提示：您可以只输入X坐标或只输入Y坐标，系统将分别计算各自的系列。
            </div>
            
            <div class="row">
                <div class="col">
                    <div class="form-group">
                        <label for="inputX">当前X坐标:</label>
                        <input type="number" id="inputX" min="0" step="any" placeholder="输入当前X坐标">
                    </div>
                </div>
                <div class="col">
                    <div class="form-group">
                        <label for="inputY">当前Y坐标:</label>
                        <input type="number" id="inputY" min="0" step="any" placeholder="输入当前Y坐标">
                    </div>
                </div>
            </div>
            
            <button class="btn btn-primary btn-calculate" onclick="calculate()">生成坐标系列</button>
        </div>
        
        <div id="errorMessage" class="error"></div>
        
        <div id="results" class="results-container" style="display: none;">
            <div id="currentValueContainer" class="current-value"></div>
            
            <div class="tab-container">
                <div class="tab-buttons">
                    <button class="tab-button active" onclick="switchTab('x-series')">X 坐标系列</button>
                    <button class="tab-button" onclick="switchTab('y-series')">Y 坐标系列</button>
                </div>
                
                <div id="x-series" class="tab-content active">
                    <div class="result-section">
                        <div class="result-header">
                            <div class="result-icon">↓</div>
                            <div class="result-title">小于当前X坐标的系列</div>
                        </div>
                        <table>
                            <thead>
                                <tr>
                                    <th>X 值</th>
                                    <th>倍数(n_x)</th>
                                </tr>
                            </thead>
                            <tbody id="smallerXBody"></tbody>
                        </table>
                    </div>
                    
                    <div class="result-section" style="margin-top: 20px;">
                        <div class="result-header">
                            <div class="result-icon">↑</div>
                            <div class="result-title">大于当前X坐标的系列</div>
                        </div>
                        <table>
                            <thead>
                                <tr>
                                    <th>X 值</th>
                                    <th>倍数(n_x)</th>
                                </tr>
                            </thead>
                            <tbody id="largerXBody"></tbody>
                        </table>
                    </div>
                </div>
                
                <div id="y-series" class="tab-content">
                    <div class="result-section">
                        <div class="result-header">
                            <div class="result-icon">↓</div>
                            <div class="result-title">小于当前Y坐标的系列</div>
                        </div>
                        <table>
                            <thead>
                                <tr>
                                    <th>Y 值</th>
                                    <th>倍数(n_y)</th>
                                </tr>
                            </thead>
                            <tbody id="smallerYBody"></tbody>
                        </table>
                    </div>
                    
                    <div class="result-section" style="margin-top: 20px;">
                        <div class="result-header">
                            <div class="result-icon">↑</div>
                            <div class="result-title">大于当前Y坐标的系列</div>
                        </div>
                        <table>
                            <thead>
                                <tr>
                                    <th>Y 值</th>
                                    <th>倍数(n_y)</th>
                                </tr>
                            </thead>
                            <tbody id="largerYBody"></tbody>
                        </table>
                    </div>
                </div>
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
                <input type="text" id="settingName" placeholder="例如：项目A坐标">
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
                <!-- 已保存的设置将在这里显示 -->
                <div class="settings-empty">
                    加载中...
                </div>
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
        });
        
        // 切换标签页
        function switchTab(tabId) {
            // 隐藏所有标签内容
            const tabContents = document.querySelectorAll('.tab-content');
            tabContents.forEach(tab => {
                tab.classList.remove('active');
            });
            
            // 取消所有标签按钮的激活状态
            const tabButtons = document.querySelectorAll('.tab-button');
            tabButtons.forEach(button => {
                button.classList.remove('active');
            });
            
            // 显示选择的标签内容
            document.getElementById(tabId).classList.add('active');
            
            // 激活对应的标签按钮
            const activeButton = document.querySelector(`.tab-button[onclick="switchTab('${tabId}')"]`);
            activeButton.classList.add('active');
        }
        
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
                                初始X: ${setting.initialX}, 步长X: ${setting.stepX}, 初始Y: ${setting.initialY}, 步长Y: ${setting.stepY}
                            </div>
                        </div>
                        <div class="setting-actions">
                            <button class="btn btn-success" onclick="applySetting('${setting.name}')" title="应用此设置">✓</button>
                            <button class="btn btn-danger" onclick="deleteSetting('${setting.name}')" title="删除此设置">×</button>
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
            
            // 准备结果
            let nX = null, nY = null;
            let closestX = null, closestY = null;
            
            // 显示当前值容器
            const currentValueContainer = document.getElementById('currentValueContainer');
            let displayHtml = '';
            
            // 计算X轴结果
            if (hasInputX) {
                nX = findClosestN(inputX, initialX, stepX);
                closestX = initialX + stepX * nX;
                
                displayHtml += `<div>X坐标: ${inputX} → 最接近的系列值: <span class="result-value">${closestX.toFixed(6)}</span> (n_x = ${nX})</div>`;
                
                // 生成X轴系列
                const smallerXResults = [];
                const largerXResults = [];
                
                // 计算小于当前nX的结果 (最多10个)
                for (let i = nX - 1; smallerXResults.length < 10 && i >= 0; i--) {
                    const x = initialX + stepX * i;
                    
                    // 过滤掉负数结果
                    if (x < 0) continue;
                    
                    // 插入到开头，确保值从小到大排序
                    smallerXResults.unshift({
                        value: x,
                        n: i
                    });
                }
                
                // 计算大于当前nX的结果 (最多10个)
                for (let i = nX + 1; largerXResults.length < 10; i++) {
                    const x = initialX + stepX * i;
                    
                    // 过滤掉负数结果
                    if (x < 0) continue;
                    
                    largerXResults.push({
                        value: x,
                        n: i
                    });
                }
                
                // 显示X轴小于的结果
                const smallerXBody = document.getElementById('smallerXBody');
                smallerXBody.innerHTML = '';
                
                if (smallerXResults.length === 0) {
                    const row = document.createElement('tr');
                    const cell = document.createElement('td');
                    cell.colSpan = 2;
                    cell.textContent = '没有小于该值的结果';
                    cell.style.padding = '15px';
                    row.appendChild(cell);
                    smallerXBody.appendChild(row);
                } else {
                    smallerXResults.forEach(result => {
                        const row = document.createElement('tr');
                        
                        const valueCell = document.createElement('td');
                        valueCell.textContent = result.value.toFixed(6);
                        
                        const nCell = document.createElement('td');
                        nCell.textContent = result.n;
                        
                        row.appendChild(valueCell);
                        row.appendChild(nCell);
                        
                        smallerXBody.appendChild(row);
                    });
                }
                
                // 显示X轴大于的结果
                const largerXBody = document.getElementById('largerXBody');
                largerXBody.innerHTML = '';
                
                if (largerXResults.length === 0) {
                    const row = document.createElement('tr');
                    const cell = document.createElement('td');
                    cell.colSpan = 2;
                    cell.textContent = '没有大于该值的结果';
                    cell.style.padding = '15px';
                    row.appendChild(cell);
                    largerXBody.appendChild(row);
                } else {
                    largerXResults.forEach(result => {
                        const row = document.createElement('tr');
                        
                        const valueCell = document.createElement('td');
                        valueCell.textContent = result.value.toFixed(6);
                        
                        const nCell = document.createElement('td');
                        nCell.textContent = result.n;
                        
                        row.appendChild(valueCell);
                        row.appendChild(nCell);
                        
                        largerXBody.appendChild(row);
                    });
                }
                
                // 如果只有X坐标，则自动显示X坐标标签页
                if (!hasInputY) {
                    switchTab('x-series');
                }
            }
            
            // 计算Y轴结果
            if (hasInputY) {
                nY = findClosestN(inputY, initialY, stepY);
                closestY = initialY + stepY * nY;
                
                if (hasInputX) {
                    displayHtml += `<div style="margin-top:10px;">Y坐标: ${inputY} → 最接近的系列值: <span class="result-value">${closestY.toFixed(6)}</span> (n_y = ${nY})</div>`;
                } else {
                    displayHtml += `<div>Y坐标: ${inputY} → 最接近的系列值: <span class="result-value">${closestY.toFixed(6)}</span> (n_y = ${nY})</div>`;
                }
                
                // 生成Y轴系列
                const smallerYResults = [];
                const largerYResults = [];
                
                // 计算小于当前nY的结果 (最多10个)
                for (let i = nY - 1; smallerYResults.length < 10 && i >= 0; i--) {
                    const y = initialY + stepY * i;
                    
                    // 过滤掉负数结果
                    if (y < 0) continue;
                    
                    // 插入到开头，确保值从小到大排序
                    smallerYResults.unshift({
                        value: y,
                        n: i
                    });
                }
                
                // 计算大于当前nY的结果 (最多10个)
                for (let i = nY + 1; largerYResults.length < 10; i++) {
                    const y = initialY + stepY * i;
                    
                    // 过滤掉负数结果
                    if (y < 0) continue;
                    
                    largerYResults.push({
                        value: y,
                        n: i
                    });
                }
                
                // 显示Y轴小于的结果
                const smallerYBody = document.getElementById('smallerYBody');
                smallerYBody.innerHTML = '';
                
                if (smallerYResults.length === 0) {
                    const row = document.createElement('tr');
                    const cell = document.createElement('td');
                    cell.colSpan = 2;
                    cell.textContent = '没有小于该值的结果';
                    cell.style.padding = '15px';
                    row.appendChild(cell);
                    smallerYBody.appendChild(row);
                } else {
                    smallerYResults.forEach(result => {
                        const row = document.createElement('tr');
                        
                        const valueCell = document.createElement('td');
                        valueCell.textContent = result.value.toFixed(6);
                        
                        const nCell = document.createElement('td');
                        nCell.textContent = result.n;
                        
                        row.appendChild(valueCell);
                        row.appendChild(nCell);
                        
                        smallerYBody.appendChild(row);
                    });
                }
                
                // 显示Y轴大于的结果
                const largerYBody = document.getElementById('largerYBody');
                largerYBody.innerHTML = '';
                
                if (largerYResults.length === 0) {
                    const row = document.createElement('tr');
                    const cell = document.createElement('td');
                    cell.colSpan = 2;
                    cell.textContent = '没有大于该值的结果';
                    cell.style.padding = '15px';
                    row.appendChild(cell);
                    largerYBody.appendChild(row);
                } else {
                    largerYResults.forEach(result => {
                        const row = document.createElement('tr');
                        
                        const valueCell = document.createElement('td');
                        valueCell.textContent = result.value.toFixed(6);
                        
                        const nCell = document.createElement('td');
                        nCell.textContent = result.n;
                        
                        row.appendChild(valueCell);
                        row.appendChild(nCell);
                        
                        largerYBody.appendChild(row);
                    });
                }
                
                // 如果只有Y坐标，则自动显示Y坐标标签页
                if (!hasInputX) {
                    switchTab('y-series');
                }
            }
            
            // 显示结果
            currentValueContainer.innerHTML = displayHtml;
            
            // 显示结果区域
            const results = document.getElementById('results');
            results.style.display = 'flex';
            
            // 平滑滚动到结果区域
            results.scrollIntoView({ behavior: 'smooth' });
        }
    </script>
</body>
</html>
```