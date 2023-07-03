# 在ubuntu系统中创建service服务来守护ssh隧道

## 首先在目标公网服务器中开启sshd的公网IP权限
* 编辑sshd的配置文件
> vim /etc/ssh/sshd_config
> 找到以下字段并设置为yes
> AllowAgentForwarding yes 允许本地转发
> AllowTcpForwarding yes 允许远程转发
> GatewayPorts yes 允许本地除了127.0.0.1/0.0.0.0端口之外的IP网段（本文中特指本服务器的公网IP）接收转发请求
* 重启sshd服务
> systemctl restart sshd

## 在本地服务器准备好能执行完成的ssh隧道的建立的一句指令
* 安装sshpass
> apt install sshpass
> 这个指令加在所有ssh语句的前面可以自动帮你填写ssh的后续的一些操作
* 编辑建立隧道的语句
> sshpass -p 公网服务器密码 ssh -fN -R 公网服务器IP:公网服务器port:本地服务器IP:本地服务器port 公网服务器用户名@公网服务器IP -o ServerAliveInterval=30
> 该语句因为无法在前台存活，直接会进入后台，所以service不能直接设置这句语句为启动语句，否则service会瞬间丢失对这个持续进程的监控，认为该进程已死

## 进入到存放各种service的目录
* 打开用户可以编辑的service目录
> cd /usr/lib/systemd/system
> 其他还有两个目录也存放着几个service，但好像是系统service，所以选择在这边usr下的service进行操作

## 创建一个新的service
* vim 你的新service的名称.service
* 这个文档应该都要用英文写
```
[Unit]
Description=你的service的简介
[Service]
ExecStart=sshpass -p 公网服务器密码 ssh -N -R 公网服务器IP:公网服务器port:本地服务器IP:本地服务器port 公网服务器用户名@公网服务器IP -o ServerAliveInterval=30
[Install]
WantedBy=multi-user.target
```
> 其中：
> Description=可以填写关于这个service的介绍
> Restart=always可以监控下面ExecStart的语句是否存活，即使是被kill掉了这个语句，service也会重新执行这个
> ExecStart=填写内容为service服务启动时执行的语句
> 配套的还有service服务停止时可以触发别的语句的字段，但是这个没研究
> 注意：
> 这里ExecStart里的语句与命令行执行的部分有一点不同，就是少了-f参数，这样可以保证进程在前台运行，让service可以监控到进程正在持续

## 操作service
* 查看service状态
> service 你的新service的名称 status
* 启动service
> service 你的新service的名称 start
* 停止service
> service 你的新service的名称 stop
* 重启service
> service 你的新service的名称 restart














