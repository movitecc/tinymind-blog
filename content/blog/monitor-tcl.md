---
title: monitor tcl
date: 2024-12-04T00:40:59.340Z
---

#!/usr/bin/tclsh

# 检查命令行参数
if {$argc != 1} {
    puts "Usage: tclsh monitor.tcl <script_to_monitor.tcl>"
    exit 1
}

# 获取要监控的脚本文件名
set script_file [lindex $argv 0]

# 确保文件存在
if {![file exists $script_file]} {
    puts "Error: File '$script_file' does not exist"
    exit 1
}

# 定义执行命令的过程
proc execute_command {cmd} {
    if {[string trim $cmd] eq ""} {
        return
    }
    
    # 检测命令类型
    set cmd_type "Simple command"
    if {[regexp {\{|\}} $cmd]} {
        set cmd_type "Block command"
    } elseif {[regexp {;} $cmd]} {
        set cmd_type "Multiple commands"
    }
    
    # 打印当前要执行的命令
    puts "\n>>> Executing ($cmd_type):"
    puts "$cmd"
    
    # 使用catch捕获可能的错误
    if {[catch {eval $cmd} result errorDict]} {
        report_error $cmd $result $errorDict
    } else {
        # 如果命令有输出，显示输出结果
        if {$result ne ""} {
            puts "Result: $result"
        }
    }
}

# 错误报告过程
proc report_error {cmd error_msg error_dict} {
    global line_map
    set line_info [dict get $line_map $cmd]
    set start_line [lindex $line_info 0]
    set end_line [lindex $line_info 1]
    
    puts "\nError in command (lines $start_line-$end_line):"
    puts "Command: $cmd"
    puts "Error message: $error_msg"
    puts "Error info: [dict get $error_dict -errorinfo]"
    puts "Error code: [dict get $error_dict -errorcode]"
    puts "Stack trace:"
    puts [dict get $error_dict -errorinfo]
}

# 读取并执行脚本
puts "\nStarting to monitor and execute: $script_file\n"
puts "----------------------------------------"

# 打开文件
set fp [open $script_file r]

# 初始化变量
set current_block ""
set brace_count 0
set line_number 1
set block_start_line 0
set line_map [dict create]

# 逐行读取并处理
while {[gets $fp line] >= 0} {
    set trimmed_line [string trim $line]
    
    # 跳过空行和注释
    if {$trimmed_line eq "" || [string index $trimmed_line 0] eq "#"} {
        incr line_number
        continue
    }
    
    # 计算行中的花括号数量
    set open_braces [regsub -all {\{} $line {} count_open]
    set close_braces [regsub -all {\}} $line {} count_close]
    
    # 更新花括号计数
    incr brace_count $count_open
    incr brace_count -$count_close
    
    # 如果我们不在代码块中且这行不开始新的代码块
    if {$brace_count == 0 && $current_block eq "" && ![string match *\{ $trimmed_line]} {
        # 执行单行命令
        puts "\nLine $line_number:"
        execute_command $trimmed_line
        track_line $trimmed_line $line_number $line_number
    } else {
        # 我们在代码块中或开始新的代码块
        if {$current_block eq ""} {
            set block_start_line $line_number
        }
        append current_block $line \n
        
        # 如果代码块结束
        if {$brace_count == 0} {
            puts "\nBlock starting at line $block_start_line:"
            execute_command $current_block
            track_line $current_block $block_start_line $line_number
            set current_block ""
        }
    }
    
    incr line_number
}

# 检查是否有未闭合的代码块
if {$brace_count != 0} {
    puts "\nError: Unclosed code block. Missing $brace_count closing brace(s)"
}

# 关闭文件
close $fp
puts "\n----------------------------------------"
puts "Monitoring completed"


========================


#################################################
# 1. 基础命令测试
#################################################
set test_var 100
puts "Basic variable: $test_var"

#################################################
# 2. 复杂控制结构测试
#################################################
# 2.1 嵌套if结构
if {$test_var > 50} {
    puts "Level 1: > 50"
    if {$test_var > 75} {
        puts "Level 2: > 75"
        if {$test_var > 90} {
            puts "Level 3: > 90"
        }
    }
}

# 2.2 if-elseif-else结构
if {$test_var < 50} {
    puts "Less than 50"
} elseif {$test_var < 80} {
    puts "Between 50 and 80"
} else {
    puts "Greater than or equal to 80"
}

# 2.3 switch结构
switch $test_var {
    100 {
        puts "Exactly 100"
    }
    default {
        puts "Not 100"
    }
}

#################################################
# 3. 循环结构测试
#################################################
# 3.1 while循环
set i 0
while {$i < 3} {
    puts "While loop: $i"
    incr i
}

# 3.2 for循环
for {set i 0} {$i < 3} {incr i} {
    puts "For loop: $i"
}

# 3.3 foreach循环
foreach item {apple banana cherry} {
    puts "Foreach loop: $item"
}

#################################################
# 4. 过程定义和调用测试
#################################################
proc test_proc {arg1 {arg2 "default"}} {
    puts "Arg1: $arg1, Arg2: $arg2"
    return [list $arg1 $arg2]
}

test_proc "test1"
test_proc "test1" "test2"

#################################################
# 5. 错误处理测试
#################################################
# 5.1 catch错误处理
if {[catch {
    set undefined_var
} error_msg]} {
    puts "Caught error: $error_msg"
}

# 5.2 try-catch结构
try {
    open "nonexistent_file.txt"
} trap {POSIX ENOENT} {error_msg} {
    puts "File not found: $error_msg"
}

#################################################
# 6. 特殊字符和转义测试
#################################################
set special_var "Quote\"Test"
puts $special_var

set multiline "Line 1\nLine 2\nLine 3"
puts $multiline

#################################################
# 7. 命名空间测试
#################################################
namespace eval TestNS {
    variable ns_var 123
    proc ns_proc {} {
        variable ns_var
        puts "Namespace var: $ns_var"
    }
}

TestNS::ns_proc

#################################################
# 8. 正则表达式测试
#################################################
set test_string "Hello, World!"
if {[regexp {^Hello} $test_string]} {
    puts "String starts with Hello"
}

#################################################
# 9. 列表操作测试
#################################################
set test_list [list 1 2 3 4 5]
puts "List length: [llength $test_list]"
puts "List element 2: [lindex $test_list 2]"
lappend test_list 6
puts "After append: $test_list"

#################################################
# 10. 文件操作测试
#################################################
set temp_file "test_temp.txt"
set fh [open $temp_file w]
puts $fh "Test content"
close $fh

set fh [open $temp_file r]
set content [read $fh]
close $fh
file delete $temp_file
puts "File content: $content"

#################################################
# 11. 特殊情况测试
#################################################
# 11.1 空行和注释混合
if {1} {

    # 内部注释
    puts "Test 1"

    # 另一个注释
    puts "Test 2"
}

# 11.2 单行多命令
set a 1; set b 2; set c 3

# 11.3 续行符测试
set long_string "This is a very \
long string that spans \
multiple lines"

# 11.4 大括号嵌套测试
set nested_cmd {
    if {1} {
        if {1} {
            if {1} {
                puts "Deeply nested"
            }
        }
    }
}
eval $nested_cmd