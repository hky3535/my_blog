# 玩客云安装docker版OpenWrt

## （必须）apt install docker.io
## （可选）安装casaos curl -fsSL https://get.casaos.io | bash
## 安装openwrt
* 开启网卡混杂模式以允许docker创建虚拟网卡
> ip link set eth0 promisc on  

* 在物理网卡eth0上创建docker虚拟网卡
> docker network create -d macvlan --subnet=192.168.10.0/24 --gateway=192.168.10.1 -o parent=eth0 macnet  

* 拉取并运行docker版OpenWrt并将容器的网卡设置为刚刚创建的虚拟网卡，并赋予一个该网段下的固定IP地址
> docker run -itd --name=OpenWrt --restart always --network macnet --ip 192.168.10.103 --privileged wfnb/onecloud:23-01-25-beta /sbin/init  

* 在刚刚创建好的容器中配置IP地址
> 打开网络配置文件  
> vim /etc/config/network  
> 文件中含有以下内容  
> option ipaddr '10.1.1.13'  
> option netmask '255.255.255.0'  
> list dns '223.5.5.5'  
> 将以上默认内容改为以下内容  
> option ipaddr '192.168.10.103'  
> option netmask '255.255.255.0'  
> list dns '192.168.10.101'  

* 配置密码
> passwd  
> 输入新密码（不能直接留空）  

* 在面板内更新插件库或下载插件之前可能需要在面板中做好网络配置
> 在面板中打开：网络->接口->编辑  
> 设置 静态地址 + ipv4 + ipv4网关  

* 在容器后台更新插件库
> 首先通过添加关闭opkg的认证的参数做update更新  
> opkg update --no-check-certificate  
> 然后通过添加关闭opkg的认证的参数安装认证，从而摆脱添加--no-check-certificate参数  
> opkg install ca-certificates --no-check-certificate  
> 之后就是正常更新操作了  
> opkg list-upgradable | cut -f 1 -d ' ' | xargs opkg upgrade 更新所有包  

* 下载界面主题
> 在面板中打开：系统->软件包  
> 筛选luci-theme找到所有可以下载的主体界面  
> 下载luci-theme-argon  
> 在面板中打开：系统->系统->语言和界面以设置新的界面主题  
