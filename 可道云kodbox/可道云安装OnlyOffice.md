# 可道云安装OnlyOffice

## 首先明确
* OnlyOffice是一种在后端的php服务，他仅提供office类文件的解析和在Web页面上的渲染，以及文件的在线编辑功能，OnlyOffice本体是没有可视化界面去操作的，他只是一个服务，一种内核。
* 所以在任何平台想要使用这个功能，都需要自己手动去编写api调用器去调用这个内核的功能。

## 安装docker版的OnlyOffice后端服务（运行内核）
* 这里下载免费的社区版
> docker run -i -t -d -p 29966:80 --name OnlyOffice --restart=always -e JWT_ENABLED=false onlyoffice/documentserver
> 注意，这里的-e JWT_ENABLED=false这个环境变量很重要，因为上面的可道云插件是2019年的版本，然后现在的OnlyOffice已经有所更新，在JWT这个JSONWeb校验上面已经有了出入（用来校验文件安全性的），所以干脆用这个办法直接把这个安全性校验关掉
* 下载完容器自动启动然后已经可以使用了，完全不需要再做别的任何操作
* 这边服务的实际调用端口就是-p 29966:80里映射出来的29966

## 安装可道云中OnlyOffice服务api的调用器
* 除了有插件中心中可以高价购买的企业版OnlyOffice，其实可以有社区版的版本需要自己去安装
* 首先下载社区版的OnlyOffice可道云插件
> 这里包含有各种不同的插件
> https://github.com/mengkunsoft/KodPlugins
> 在下面的其他社区开发者的仓库中选择这个
> https://github.com/zhtengw/kodexplorer-plugins
> 直接把整个仓库用电脑把zip下载下来
> 挑选出里面的/kodexplorer-plugins/kodbox-plugins/OnlyOffice文件夹
* 正式开始安装这个插件
> 将刚刚的这个插件OnlyOffice文件夹放入可道云的容器中的/var/www/html/plugins下
> 在这个文件夹下其实已经可以看到很多别的已经下载了的插件，基本都是用php服务的格式来制作的
> 放到容器这个目录下后保持文件夹名字不变还是OnlyOffice
* 文件夹放进去就算是安装完成了（跟NextCloud的手动安装方式一致）
> 重启可道云的容器

## 将可道云中的OnlyOffice服务api调用器配置到刚刚装好的OnlyOffice后端服务（运行内核）
* 打开可道云的插件中心
* 在“已安装”栏里已经能看到“OnlyOffice在线编辑器”
* 点击“配置插件”
* 在“服务器接口”中的http://：这一栏里填上192.168.1.100:29966就行，其他的路径之类的他已经缺省帮你前后补足了
* 其他一个都不用动
* 在docx文件上右键，打开方式，可以直接设置成使用OnlyOffice打开了