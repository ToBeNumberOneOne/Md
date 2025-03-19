

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



makefile有一组规则组成，格式是：
```
target ... : prerequisites ... #目标 条件
	command1
	command2
	...
main: main.o stack.o maze.o
	gcc main.o stack.o maze.o -o main    
```
目标和条件之间的关系是：欲更新目标，必须首先更新它的所有条件；所有条件中只要有一个条件被更新了，目标也必须随之被更新。更新即执行一遍命令列表。

如果一条规则的目标属于以下情况之一，则称为需要更新：
- 目标没有生成
- 某个条件需要更新
- 某个条件的修改时间比目标晚

通常会有clean规则，用于清除编译过程中产生的二进制文件
```
.PHONY:clean /#把clean声明为一个伪目标，避免存在clean这个文件导致的问题

clean:
	@echo "cleanning project"
	-rm main *.o
	@echo "clean completed"

$:make clean 在make的命令行指定目标（如clean），则更新这个目标，不指定则更新第一条规则的目标

@echo 不显示命令本身只显示其结果
-rm -mkdir 即使命令出错也继续执行后续命令
```

make处理Makefile的过程分为2阶段
- 阶段1 从前到后读取所有规则，建立起完整的依赖关系图
- 阶段2 从缺省的目标或命令行目标，根据依赖关系选择适当的规则执行，并不是顺序执行，也不是所有规则都要执行

类似clean的目标名字
- all 执行主要的编译工作，通常用作缺省目标。
- install 执行编译后的安装工作，把可执行文件、配置文件、文档等分别拷到不同的安装目录。
- distclean 不仅删除编译生成的二进制文件，也删除其它生成的文件，例如配置文件和格式转换后的文档，执行make distclean之后应该清除所有这些文件，只留下源文件。



***注意***
1. 命令列表的每条指令必须以TAB开头，不能是空格
2. 格式较为严格，以Tab开头的命令，make会创建一个shell进程区执行
3. 第一条目标为缺省目标，完成即结束
  

### 隐含规则和模式规则

如果一个目标在Makefile中的所有规则都没有命令列表，则会尝试在内建的隐含规则中查找适用规则 make -p 可将其打印
```
# default
OUTPUT_OPTION = -o $@

# default
CC = cc

# default
COMPILE.c = $(CC) $(CFLAGS) $(CPPFLAGS) $(TARGET_ARCH) -c

%.o: %.c
#  commands to execute (built-in):
        $(COMPILE.c) $(OUTPUT_OPTION) $<
```

'#'在makefile中表示单行注释

$@ 取值为规则中的目标

$< 规则中的第一个条件

