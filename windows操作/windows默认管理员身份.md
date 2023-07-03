# 设置默认管理员打开所有程序

## 修改安全策略组
* 打开安全策略组
> win+R
> 输入scpol.msc
* 修改安全策略组内容
> 本地策略->安全选项->用户账户控制：以管理员批准模式运行所有管理员
> 将其设置为“已禁用”

## 重启计算机完成设置

## 注：
> 注：家庭版用户没有scpol.msc
> win+R运行regedit
> 在注册表中进入注册表目录
> HKEY_LOCAL_MACHINE>SOFTWARE>Microsoft>Windows>CurrentVersion>Policies>System
> 将EnableLUA值改为0