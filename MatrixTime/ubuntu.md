# linux命令

## 基础

管道符
- | 管道符，将前一个命令的输出作为下一个命令的输入
- history | grep *** 搜索历史指令
- ps -ef | grep *** ps 显示当前正在运行的进程 -e 显示用户所有进程 -f 完整格式显示进程


## 包管理

### pip

```bash
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple onnxruntime opencv-python  指定源安装
```

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