

# gdb

-g 生成的执行文件可以用gdb进行源码级调试

- gcc -E x选项可以看到预处理之后编译之前的程序

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
# makefile



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
COMPILE.c = $(CC) $(CFLAGS) $(CPPFLAGS) $(TARGET_ARCH) -c # cc    -c

%.o: %.c # 模式规则 pattern rule
#  commands to execute (built-in):
        $(COMPILE.c) $(OUTPUT_OPTION) $<

main.o: main.c
	cc    -c -o main.o main.c

```

'#'在makefile中表示单行注释

cc是一个符号链接，通常指向gcc

ccp 标识C preprocessor 预处理

- $@ 取值为规则中的目标

- $< 规则中的第一个条件
  
- $? 规则中比目标新的条件组成的列表
  
- $^ 规则中的所有条件组成的列表

```
main.o: main.c
	$(CC) $(CFLAGS) $(CPPFLAGS) -c $<
```

### 常用变量

- AR 静态库打包命令的名字，缺省值是ar。
- ARFLAGS 静态库打包命令的选项，缺省值是rv。
- S 汇编器的名字，缺省值是as。
- ASFLAGS 汇编器的选项，没有定义。
- CC C编译器的名字，缺省值是cc。
- CFLAGS C编译器的选项，没有定义。
- CXX C++编译器的名字，缺省值是g++。
- CXXFLAGS C++编译器的选项，没有定义。
- CPP C预处理器的名字，缺省值是$(CC) -E。
- CPPFLAGS C预处理器的选项，没有定义。
- LD 链接器的名字，缺省值是ld。
- LDFLAGS 链接器的选项，没有定义。
- TARGET_ARCH 和目标平台相关的命令行选项，没有定义。
- OUTPUT_OPTION 输出的命令行选项，缺省值是-o $@。
- LINK.o 把.o文件链接在一起的命令行，缺省值是$(CC) $(LDFLAGS) $(TARGET_ARCH)。
- LINK.c 把.c文件链接在一起的命令行，缺省值是$(CC) $(CFLAGS) $(CPPFLAGS) $(LDFLAGS) $(TARGET_ARCH)。
- LINK.cc 把.cc文件（C++源文件）链接在一起的命令行，缺省值是$(CXX) $(CXXFLAGS) $(CPPFLAGS) $(LDFLAGS) $(TARGET_ARCH)。
- COMPILE.c 编译.c文件的命令行，缺省值是$(CC) $(CFLAGS) $(CPPFLAGS) $(TARGET_ARCH) -c。
- COMPILE.cc 编译.cc文件的命令行，缺省值是$(CXX) $(CXXFLAGS) $(CPPFLAGS) $(TARGET_ARCH) -c。
- RM 删除命令的名字，缺省值是rm -f。

### 处理头文件的依赖关系

```
$gcc -M main.c 自动生成目标文件和源文件的依赖关系（含系统头文件）

$gcc -MM *.c 不含系统头文件

all: main

main: main.o stack.o maze.o
	gcc $^ -o $@

clean:
	-rm main *.o

.PHONY: clean

sources = main.c stack.c maze.c

include $(sources:.c=.d)

%.d: %.c
	set -e; rm -f $@; \
	$(CC) -MM $(CPPFLAGS) $< > $@.$$$$; \
	sed 's,\($*\)\.o[ :]*,\1.o $@ : ,g' < $@.$$$$ > $@; \
	rm -f $@.$$$$
```

### 常用make命令行选项

- -n 打印要执行的命令，但不会真的执行
- -C 切换另一个目录执行那个目录下的makefile
- make CFLAG=-g 在命令行中修改选项而不是修改Makefule



# 语法

### static 

- 修饰函数的作用域
  - 限制在定义它的源文件内，在其他源文件中无法使用。 
  - static函数一般用于实现一些辅助功能，这些功能只在当前源文件里有用，这样可以避免函数名冲突，增强代码的模块化和封装性。


### const


- 修饰变量时表示变量为只读，一旦初始化就无法修改

### extern

- 用于声明一个函数或者变量是在其他文件中定义的，以便能进行调用
- c语言不允许嵌套定义函数，虽然可以在一个函数体声明另一个函数
- 作用域，语句块也构成一个作用域，单独使用语句块通常是为了定义比函数的局部变量更“局部”的变量
- %运算符的结果总与被除数同号

----

- 从语法上规定全局变量只能用常量表达式来初始化



# 常用C标准库函数

- getopt_long() 解析命令行参数，支持长选项--和短选项-
  
- int sscanf(const char *str, const char *format, ...);
	- 从字符串中按照指定的格式读取数据，scanf是从标准输入（键盘）读取数据
	- format是格式控制字符串，以%开头
	- ... 是可变参数列表，用于接受读取到的数据
	- optarg 全局变量，获取命令行选项的参数