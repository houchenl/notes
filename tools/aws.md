
# 亚马逊服务器使用指南

### 1. 在EC2中创建密钥地

### 2. mac 终端连接服务器


### 公网IP
13.113.0.31
弹性IP    52.69.202.39
IPV4 公有IP    52.69.202.39

### root用户
创建root用户密码    'sudo passwd root'  
切换到root身份    'su root'

### 安装启动连接shdowsocks
- 切换到root用户
- yum install python-pip
- pip install shadowsocks
- touch /etc/shadowsocks.json
- vim /etc/shadowsocks.json，添加如下内容
```
{
    "server":"0.0.0.0",
    "server_port":443,
    "local_address":"127.0.0.1",
    "local_port":9001,
    "password":"连接密码",
    "timeout":300,
    "method":"aes-256-cfb",
    "fast_open":false
}

```
- ssserver -c /etc/shadowsocks.json -d start //启动shadowsocks
- 