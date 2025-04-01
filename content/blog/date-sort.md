---
title: date sort
date: 2025-04-01T01:44:35.376Z
---

```python
from datetime import datetime, date

def convert_to_datetime(date_str):
    """
    尝试将各种格式的日期字符串转换为datetime对象
    """
    formats = ["%Y-%m-%d", "%d-%m-%Y", "%m-%d-%Y", "%d/%m/%Y", 
               "%m/%d/%Y", "%Y/%m/%d", "%b %d, %Y", "%d %b %Y",
               "%Y年%m月%d日", "%m-%Y", "%m/%Y"]
              
    # 如果输入已经是datetime或date对象，直接返回
    if isinstance(date_str, (datetime, date)):
        return date_str if isinstance(date_str, datetime) else datetime(date_str.year, date_str.month, date_str.day)
    
    # 尝试不同的格式解析
    for fmt in formats:
        try:
            return datetime.strptime(date_str, fmt)
        except ValueError:
            continue
    
    # 如果所有格式都失败，抛出异常
    raise ValueError(f"无法解析日期格式: {date_str}")

def find_next_date(date_list, current_date):
    """
    查找列表中当前日期的下一个日期
    """
    # 确保所有日期都是datetime对象
    date_objects = []
    for d in date_list:
        try:
            date_objects.append(convert_to_datetime(d))
        except ValueError:
            print(f"警告: 无法解析日期 '{d}'，已跳过")
    
    # 确保当前日期是datetime对象
    if not isinstance(current_date, datetime):
        current_date = convert_to_datetime(current_date)
    
    # 排序日期列表
    date_objects.sort()
    
    # 寻找比当前日期大的最小日期
    next_date = None
    for d in date_objects:
        if d > current_date:
            next_date = d
            break
    
    return next_date

# 示例使用
if __name__ == "__main__":
    # 包含不同格式的日期列表
    dates = [
        "2023-01-15",        # ISO格式
        "05-04-2023",        # DD-MM-YYYY
        "12/25/2022",        # MM/DD/YYYY
        "Mar 20, 2023",      # 文字月份
        "2022年12月30日",     # 中文格式
        "06-2023"            # MM-YYYY
    ]
    
    # 获取当前日期并添加到列表中
    today = datetime.now().date()
    dates.append(today)
    
    # 打印原始列表
    print("原始日期列表:")
    for d in dates:
        print(f"  {d}")
    
    # 转换为datetime对象进行排序
    sorted_dates = sorted(dates, key=convert_to_datetime)
    
    # 打印排序后的列表
    print("\n排序后的日期列表:")
    for d in sorted_dates:
        print(f"  {d}")
    
    # 查找当前日期的下一个日期
    next_date = find_next_date(dates, today)
    
    # 打印结果
    print(f"\n当前日期: {today}")
    if next_date:
        print(f"下一个日期: {next_date.strftime('%Y-%m-%d')}")
    else:
        print("没有找到比当前日期更晚的日期")
```