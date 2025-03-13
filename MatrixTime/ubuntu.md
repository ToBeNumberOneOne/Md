# linux命令

## shell基础

可以理解为shell是一个编程环境，基于空格分隔命令并进行解析，执行第一个单词代表的程序，并将后续的单词作为程序可以访问的参数。如果您希望传递的参数中包含空格（例如一个名为 My Photos 的文件夹），您要么用使用单引号，双引号将其包裹起来，要么使用转义符号 \ 进行处理（My\ Photos）

在shell中执行命令实际上是在执行shell可以解释执行的简短代码。如果某个命令并不是shell的编程关键字，那么它会去环境变 $PATH 查找

默认终端提示符：

```jskj@jskj:~$ ```
- 主机名是jskj@jskj 
- 当前位置是~(home目录)
- $标识非root

### shell 导航

Linux和Macos上使用 `/`分隔目录，Windows是 `\` 

在linux系统下，`/`开头的路径是绝对路径，其他是相对路径（相对与当前路径`pwd`获取）

切换路径使用`cd`

在路径中，`.`标识的当前目录,`..`标识的上层目录，`/`标识的根目录

使用`ls`查看指定目录下包含哪些文件 -l参数会更加详细列出具体信息
```
missing:~$ ls -l /home
drwxr-xr-x 1 missing  users  4096 Jun 15  2019 missing
```
- d 表示目录
- rwxr-xr-x 表示3组用户的读,写，执行权限

常用基本指令： `mv`（用于重命名或移动文件）、 `cp`（拷贝文件）以及 `mkdir`（新建文件夹）

### 在程序间创建连接

在shell中，程序中主要有输入流和输出流，当程序尝试读取信息时会从输入流读取，当程序打印信息时会将信息输出到输出流。通常程序的输入和输出都是终端，但是也可以重定向流。

最简单的重定向是 `< file` 和 `> file`,将输入输出流重定向到文件

还可以使用`>>`向文件追加内容，使用管道`|`更好的利用文件重定向

管道符
- | 管道符，将前一个命令的输出作为下一个命令的输入
- history | grep *** 搜索历史指令
- ps -ef | grep *** ps 显示当前正在运行的进程 -e 显示用户所有进程 -f 完整格式显示进程
- `find . -type f -name "*.html" | xargs -d '\n' tar -cvzf html.zip
` xargs -d '\n' 将收到的数据逐行读取并传递给后续指令，以换行符为分隔而非默认空格 

### shell脚本（bash）

- bash中变量赋值 `foo=bar` 访问变量的值`$foo` 在shell脚本中使用空格会起到分割参数的作用
- bash中字符串通过`'`,`"`分隔符定义，前者为原意字符串，变量不会被转义，后者会将变量值进行替换

```
foo=bar
echo "$foo"
# 打印 bar
echo '$foo'
# 打印 $foo
```
- bash 也支持if,case,while,for等控制流关键字，也支持函数，可以接受参数并基于参数进行操作
  
```
    mcd(){
        mkdir -p "$1"
        cd "$1"
    }
```
shell脚本中的特殊变量：
- $0 脚本名
- $1-$9 脚本的参数
- $@ 所有的参数
- $# 参数个数
- $? 前一个命令的返回值
- `$$ `当前脚本的进程识别码
- !! 完整的上一条命令 sudo !!
- $_ 上一条命令的最后一个参数

命令通常使用STDOUT返回输出值，STDERR返回错误及错误码。返回码或退出状态是脚本和命令之间交流执行状态的方式，返回值为0代表正常执行，其他表示有错误发生。

返回码可以搭配 `&&`,`||` 使用来进行条件判断，绝对是否执行其他程序。

同一行的多个命令可以使用`;`分隔

**命令替换** `for file in $(ls)`shell 将首先调用ls,然后遍历得到的这些返回值

在执行脚本时，时常需要提供形式类似的参数，bash可以使用通配globbing来轻松实现

- 通配符 `?`匹配一个字符 `*`匹配任意个字符
- 花括号`{}` 有一系列的指令，其中包含一段公共子串时可以用于自动扩展，这在批量移动或转换文件时非常方便

[shellcheck](https://github.com/koalaman/shellcheck)定位脚本中的错误

***注意*** 

脚本并不一定只有bash写，例如python脚本
```
#!/user/local/bin/python  shebang 
import sys
for arg in reversed(sys.argv[1:]):
    print(arg)
```

shell函数和脚本的区别
1. 函数只能与shell使用相同的语言，脚本可以使用任何语言，shebang进行选择
2. 函数仅在定义时被加载，脚本在每次执行时加载
3. 函数会在当前的shell环境中执行，脚本会在单独的进程中执行。函数可以对环境变量进行修改如改变当前的工作目录，脚本则不行，脚本需要使用export将环境变量导出并将值传递给环境变量


### shell工具
- [Linux指令帮助搜索](https://tldr.inbrowser.app/pages/common/rm)

- find指令
    * `find . -name src -type d` 查找当前目录及其子目录中的src目录
    * -iname 不区分大小写
    * -maxdepth n 递归子目录深度
    * -exec 查找后执行操作 `find . -name src -type d -exec rm -r {} \` 
  
```
# 查找所有名称为src的文件夹
    find . -name src -type d

# 查找所有文件夹路径中包含test的python文件
    find . -path '*/test/*.py' -type f

# 查找前一天修改的所有文件
    find . -mtime -1

# 查找所有大小在500k至10M的tar.gz文件
    find . -size +500k -size -10M -name '*.tar.gz'

# 删除全部扩展名为.tmp 的文件
    find . -name '*.tmp' -exec rm {} \;

# 查找全部的 PNG 文件并将其转换为 JPG
    find . -name '*.png' -exec convert {} {}.jpg \;
```

- 查找shell命令 history `Ctrl + R` 

## 包管理 

### pip

```bash
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple onnxruntime opencv-python  指定源安装
```

### conda



## 文件管理

### SSH

```bash

ssh jskj@10.0.2.231  登录ssh

scp -r source dst 

scp -r D:\Software\ssh jskj@10.0.2.248:/data/Windows
```

## 系统指令

- 查询linux系统版本

```bash
lsb_release -a

cat /etc/os-release

hostnamectl
```

- 重启ubuntu gnome桌面服务

```bash
sudo systemctl restart gdm3

vncserver -kill :1

vncserver :2 -geometry 1400x900 -depth 24  -localhost no //开启服务
```

# 控制器

开启仿真程序脚本命令

1. conda env list 查看是否存在控制服务的python环境
2. conda activate mcv_env 
3. ./run_sim_sh -s image -e qc_y4 启动仿真脚本
4. python ./test_scripts/sample_plt.py local prb -n

# 安全模块

1. systemctl --user restart tlp 重启程序
2. ls /data/projects/tlp/data/log/ log目录
3. tail 命令查看log
4. nc -zv 172.25.16.111 502  查看北良网关是否存在问题
5. get-data 拉取log数据

nmcli connection

sudo nmcli connection modify '网口名' ipv4.addresses 172.25.8.112/24 ipv4.gateway 172.25.8.254
sudo nmcli connection up '网口名'

sudo nmcli connection modify "Wired connection 1" ipv4.method manual ipv4.addresses "172.25.8.112/24" ipv4.gateway"172.25.8.254"
 

# MQTT

- systemctl指令
    -   enable 开机自启动
    -   start 启动服务
    -   status 查看状态
    -   restart 重启服务

1. 发布消息 mosquitto_sub -h localhost -p 1883 -t test/topic -m "hello,MQT"
2. 订阅主题 mosquitto_pub -h localhost -p 1883 -t test/topic

# VirtualBox

### 虚拟机关闭时间同步

    1. 在VirtualBox安装目录中找到VBoxManage.exe文件
    2. 运行VBoxManage.exe list vms 查看虚拟机列表
    3. 运行VBoxManage.exe setextradata "虚拟机名称" "VBoxInternal/Devices/VMMDev/0/GetHostTimeDisabled" 1 关闭时间同步