# 建立ssh隧道映射端口至另外一台服务器

sshpass -p 公网服务器密码 ssh -fN -R 公网服务器IP:公网服务器port:本地服务器IP:本地服务器port 公网服务器用户名@公网服务器IP -o ServerAliveInterval=30