# 常用命令

- | 管道符，将前一个命令的输出作为下一个命令的输入
- history | grep *** 搜索历史指令
- ps -ef | grep *** ps 显示当前正在运行的进程 -e 显示用户所有进程 -f 完整格式显示进程信息 

### pip

- pip -i https://pypi.tuna.tsinghua.edu.cn/simple numpy 指定源安装

### SSH

登录
- ssh jskj@10.0.2.231 

传文件
- scp -r source dst 
- scp -r D:\Software\ssh jskj@10.0.2.248:/data/Windows


# 控制器

开启仿真程序脚本命令

1. conda env list 查看是否存在控制服务的python环境
2. conda activate mcv_env 
3. ./run_sim_sh -s image -e qcv2 启动仿真脚本
4. python ./test_scripts/sample_plt.py local prb -n

# 安全模块

1. systemctl --user restart tlp 重启程序
2. ls /data/projects/tlp/data/log/ log目录
3. tail 命令查看log

# MQTT

- systemctl指令
    -   enable 开机自启动
    -   start 启动服务
    -   status 查看状态
    -   restart 重启服务

1. 发布消息 mosquitto_sub -h localhost -p 1883 -t test/topic -m "hello,MQT"
2. 订阅主题 mosquitto_pub -h localhost -p 1883 -t test/topic