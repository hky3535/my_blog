# NETGEAR Router R6220解决管理页面不显示远程管理

## 管理页面中打开：高级->高级设置->Web服务管理
* 看到页面中只有本地管理，没有远程管理

* F12打开监视器，刷新页面后找到当前页面的响应
> 找到响应FW_remote.htm&todo=cfg_init中看到这部分页面由基础元素和JS组成，由JS控制基础元素的显示内容与显示与否  
> 看到前面几行中var lan_https_support="1"; var display_remotemgmt="0"推测后者是控制“远程管理”部分元素的显示与否  
> 对该var变量搜索查看实际该变量对元素进行了什么操作  
> 看到35行对这个变量进行了判断，并对ID为"remote_https_2"的元素进行了赋值操作  
> 在该响应的出去JS的部分，即页面基础元素中查找到remote_https_2的元素的.style.display的初始值为None，推测得出当元素中的这个值被赋值为""这样的空字符串时，就可以显示“远程管理”的部分了  

* 实操：在“元素”栏中
> 找到remote_https_2的元素  
> 发现该固定元素\<tbody id="remote_https_2" style="display: none">  
> 直接双击修改为\<tbody id="remote_https_2" style>  
> 回车，页面中即可显示原先被隐藏的“远程管理”相关设置内容  
