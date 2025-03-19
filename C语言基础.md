

# gdb

-g 生成的执行文件可以用gdb进行源码级调试

- layout src 进入文本图形模式（TUI mode），显示源代码窗口 退出 tui disable

### 常用的gdb指令

- list n / l n 从某个位置开始列出源码，单次10行 函数定义
- 在提示符下回车标识重复上一条指令
- quit 退出gdb环境
- start开始执行程序 next执行下一条语句 step步进
- backtrace 查看函数调用的栈帧
- frame 选择栈帧
- info locals查看局部变量的值
- finish 程序运行到当前函数返回为止
- set var修改变量值 set var sum = 0
- print 显示变量值，修改变量值或调用函数

中断相关

- brake 行号、函数名
- break if 设计条件断点
- continue 从当前位置继续运行程序
- delete breakpoints n 删除断点
- display / undisplay 变量名 跟踪变量名
- disable breakpoints n 禁用断点
- enable n 启用断点
- info breakpoints 
- run 重头开始运行程序

观察点
- x/7b input 打印指定存储单元的内容 7b为7个字节
- watch 设置观察点 info watchpoints 查看观察点

---
# makefile基本规则


  