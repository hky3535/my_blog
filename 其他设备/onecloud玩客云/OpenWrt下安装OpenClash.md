# OpenWrt下安装OpenClash

## 拉取OpenClash安装包
* 从github下载OpenClash源码
> wget https://github.com/vernesong/OpenClash/releases
> wget下载这个网址下最新的安装包后放入/tmp/目录下（先不要安装）

## 安装预置依赖
* 安装nftables相关依赖
> opkg install coreutils-nohup bash dnsmasq-full curl ca-certificates ipset ip-full libcap libcap-bin ruby ruby-yaml kmod-tun kmod-inet-diag unzip kmod-nft-tproxy luci-compat luci luci-base

## 安装OpenClash本体
* 通过opkg在OpenWrt后台安装OpenClash的ipk文件
> opkg install /tmp/luci-app-openclash_xxxxxxxxxx.ipk

## 重启容器并进入OpenClash网页
* 从网页面板完成OpenClash内核服务的安装
> 在面板中打开：服务->OpenClash
> 在OpenClash中打开：Plugin Settings->版本更新
> 检查并更新所有内核（不用下载到本地）这一步耗时较长
* 添加节点配置文件
> 在OpenClash中打开：Config Subscribe
> 添加节点配置文件并设置自动刷新时间