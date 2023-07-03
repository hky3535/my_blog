# shell指令集

## 一些基础语句
* 基础判断语句
```bash
#!/bin/bash
if [ "$1" = "delete" ]  # 如果输入的参数是delete
    then
    执行语句
    fi
```
* 特殊判断语句
```bash
#!/bin/bash
# 如果在语句ps -aux得出的结果中无法找到目标的字段，则执行循环（类似于进入等待）
# 常用于等待前面某个nohup进程被提起
while ! ps -aux | grep 锚点字段 | grep 锚点字段中的锚点字段
    do 
    sleep 1
    done
```
* 将一段指令的输出结果赋值到变量
```bash
#!/bin/bash
process_full_name=$(ps -aux | grep process_part_name)
echo $process_full_name
```

## 各种循环
* 正常死循环
```bash
#!/bin/bash
while true
    do
    sleep 1
    echo "sleep for 1 second"
    done
```
* 写在单行内的死循环
```bash
#!/bin/bash
# 常用于保持会话，包括ssh或者docker中需要检测到有持续进程的情况
"while true; do sleep 1; done"
```
* 按照数字顺序循环
```bash
#!/bin/bash
# seq 0 10 在这里就是创建一个以换行为切割的队列
# 程序会输出0 1 2 3 4 5 6 7 8 9 10
for i in $(seq 0 10)
    do
    echo $i
    done
# 或者用以下的列表创建方式
# 程序会输出3 4 5 6 7
for i in {3..7}
    do
    echo $i
    done
```
* 按照变量列表循环
```bash
#!/bin/bash
docker_0=5dfdaf7d3e30
docker_1=5c75764e2fa3
for docker in $docker_0 $docker_1 8002924d91fc ad24ec7557f4
    do
    echo $docker
    done
```

## 切分输出结果得到特定片段
* 匹配docker的完整名称
```bash
#!/bin/bash
docker_name=$(docker ps -a | grep 容器的不完整名称片段 | awk '{print $(NF)}' | tr -d '\r')
echo $docker_name
```
* 匹配ls打印的文件列表的最后一位（常用于查询最新的log文档）
```bash
#!/bin/bash
latest_log = $(ls ./logs | tail -n 1)
echo $latest_log
```
* docker build后获取到构建的镜像的id
```bash
#!/bin/bash
build_information=$(docker build .)
build_image_id=$(echo $build_information | grep -oE 'Successfully built [[:alnum:]]{12}' | awk '{print $NF}')
echo $build_image_id
```
* 通过匹配特定的进程名来杀死某个进程
```bash
#!/bin/bash
# 这里grep -v grep是用来去除掉grep自己的这一行的
ps -ef | grep 进程的不完整名称片段 | grep -v grep | awk '{print $2}' | xargs kill
# 或者以下简单操作
pkill -f '进程的不完整名称片段'
```
* 查看docker镜像中所有同名镜像的tag
```bash
docker images --format '{{.Tag}}' 镜像名称
```

## 获取到特定的系统参数
* 获取到当前日期/时间（含格式）
```bash
#!/bin/bash
# 常用于创建一个当前日期时间为命名的log文件
now="$(date +'%Y-%m-%d-%H-%M-%S').log"
echo $now
```
* 获取文件夹下文件个数
```bash
#!/bin/bash
ls -1 | wc -l
```

## 系统操作
* 清空某个log文件的内容但是不干扰正在写入这个log文件的进程（实际测试下来有一定概率会干扰后续正常写入）
```bash
#!/bin/bash
truncate -s 0 log文件的名称
```
* 直接向某个ssh服务器发送一个指令集让目标服务器执行
```bash
#!/bin/bash
sshpass -p 目标服务器密码 ssh -T 目标服务器用户名@目标服务器IP "
    指令1 &&
    指令2 &&
    指令3 ||
    指令4
"
# 注：如果在引号内执行的语句中有类似$或者其他引号类的操作，需要用转义符，例如以下操作
sshpass -p 目标服务器密码 ssh -T 目标服务器用户名@目标服务器IP "
    process_name=\$(ps -aux | grep ssh) &&
    echo \$process_name
"
```







