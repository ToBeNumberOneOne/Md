

# gdb

-g 生成的执行文件可以用gdb进行源码级调试,还可以用objdump反汇编将c代码和汇编代码穿插显示 objdump -dS a.out

- gcc -v 可以了解详细的编译过程
- gcc -S 生成汇编代码
- gcc -c 生成目标文件
- gcc -c main.c -Wall 输出编译过程中的警告信息

- gcc -E x 选项可以看到预处理之后编译之前的程序

linux系统中安装了多个版本的gcc g++编译工具，在版本间进行切换
```bash

sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-9 90
sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-13 130

sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-9 90
sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-13 130

sudo update-alternatives --config gcc

```

都可以搭配`-o` 给输出的文件重新命名而不是gcc默认的文件名

- layout src 进入文本图形模式（TUI mode），显示源代码窗口 退出 tui disable 或者 gdb tui 可执行文件
  

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

汇编
- disassemble 可以反汇编当前函数或指定函数
- si 指令一条一条指令单步调试
- info registers 显示所有寄存器的当前值
- x/20 $esp 查看内存中从$esp地址开始的20个32位的数

---

### ELF文件分析工具

- 静态分析
	- readelf -h 文件头 -l 程序头表 -s 符号表
	- objdump -d 所有代码 -S 同时显示源代码和汇编代码 反汇编代码
  
- 动态分析
  - ldd 查看动态依赖库及其加载路径
  - gdb 调试工具，程序运行时进行动态分析



### 便捷操作

- 将终端输出到另外一个终端页面中 tty 获取当前终端路径 set inferior-tty /dev/pts/1


# Makefile



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

### 条件判断
```makefile

ifeq ($(MAKECMDGOALS),) #MAKECMDGOALS 内置变量，命令行指定的目标列表
  MAKECMDGOALS  = image
  .DEFAULT_GOAL = image
endif 

```

### 内建命令
```makefile

-include $(AM_HOME)/scripts/$(ARCH).mk #include用于包含其他makefile文件 -前缀表示即使失败也不报错



```

### 函数

- 来自对PA实验中abstract-machine项目的Makefile的解读，帮助理解如何写Makefile

```makefile

$(findstring <find>, <in>) 在字符串 <in> 中查找 <find>，若找到则返回 <find>，否则返回空字符串。

$(info ...) $(error ...) 将括号内的内容输出到标准输出, error会停止后续操作，构建过程失败

$(wildcard pattern...) 展开一个或多个通配符模式，返回所有匹配这些模式的文件列表

$(shell ...) 执行shell命令并将输出作为函数返回值

$(notdir ...) 去除文件路径中的目录部分，只保留文件名 src/main.c -> main.c

$(basename ...) 去除文件中的扩展名部分，main.c -> main

$(filter ...，...) 从第二个参数中筛选出与第一个参数匹配的元素，若不存在结果为空字符串

 $(subst <from>, <to>, <text>) 在 <text> 字符串里查找所有的 <from> 字符串，然后将其替换成 <to> 字符串，最后返回替换后的新字符串。

$(word n, text) 从一个以空格分割的单词列表提取指定位置的单词，n超出则为空

$(flavor SRCS，undefined) 检查SRCS变量是否已经定义

$(abspath ...) 将路径转为绝对路径

OBJS = $(addprefix $(DST_DIR)/, $(addsuffix .o, $(basename $(SRCS)))) 加前后缀 test/div -> build/riscv32-nemu/tests/div.o

LIBS     := $(sort $(LIBS) am klib) := 立即求值 sort对传入字符串列表安装字母进行排序并去除重复元素

$(join <lsit1>, <list2>) 将两个单词列表进行合并 list1 := a b c list2 := 1 2 3 -> a1 b2 c3
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


# 编译相关

汇编程序通常以.s文件名后缀

使用汇编器as(assemblaer)可以将汇编程序中的助记符翻译成机器指令，生成目标文件*.o

然后链接器ld(linker)将目标文件.o 链接成可执行文件，链接的作用是1 修改目标文件中的信息，对地址做重定位，2 把多个目标文件合并成一个可执行文件。

```

$ as hello.s -o hello.o

$ ld hello.0 -o hello

```

# 语法


### typedef 关键字

给类型取别名的关键字，提高代码的可读性
```C

typedef unsigned int uint;

uint x = 10; //uint 是 unsigned int 的别名

typedef struct{
		int x;
		int y;
} Point;
Point p1 = {1,2}；// 匿名结构体，只能用别名Point

typedef struct ArrayList{
		int *data;
		int sizel
}ArrayList;

struct ArrayList list1;
ArrayList list2;

```

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

***注意**

使用extern声明替代include并不可取：

1. 声明和定义不一定匹配，如果需要修改得改多个地方
2. 可读性差，在源文件中声明了一个其他模块的函数但根本不知道它来自哪儿

**建议**
1. 良好的设计习惯避免头文件的混乱，保持头文件的职责单一，每个`.h`文件都是一个功能块的声明接口，不会膨胀。
2. 只有在需要的时候才#include头文件

3. 使用extern的场合
	- 共享全局变量（不适合写进头文件的结构体或数组）
	- 声明不适合放在头文件中的变量
  
  ```C
  // iringbuf.c
ItraceEntry ringbuf[IRINGBUF_LEN];

// some_other.c
extern ItraceEntry ringbuf[];

extern int nemu_state; 只在少数几个地方使用、不适合暴露给所有模块的变量
  ```
   
----

- 从语法上规定全局变量只能用常量表达式来初始化

数据驱动编程 选择正确的数据结构来组织信息，控制流程和算法尚在其次。

### volatile 

避免编译器对变量做缓存优化，“每次都必须重新读取变量”。 变量值可能是**外部因素改变**的（如硬件，另外线程或中断服务程序）

```C
volatile int falg;

volatile unsigned int *p = (unsigned int *)0x40021000; //常用于寄存器访问

const volatile int timer_count; //可被中断改变但不允许代码写

```





# 常用C标准库函数

- errno 全局变量，最近一次系统调用或库函数调用发生的错误类型

- getopt_long() 解析命令行参数，支持长选项--和短选项-
  
- int sscanf(const char *str, const char *format, ...);
	- 从字符串中按照指定的格式读取数据，scanf是从标准输入（键盘）读取数据
	- format是格式控制字符串，以%开头
	- ... 是可变参数列表，用于接受读取到的数据
	- optarg 全局变量，获取命令行选项的参数


# 其他

### 不同进制的数的表示方式

\000 表示的是一个八进制的转义序列，代表一个字节，值为0也就是空字符对应NUL

```

0b1101 0B1211 二进制

0666 八进制

1234 十进制

0x1F4 0X1F4 十六进制

```

