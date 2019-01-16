
# mac下mysql安装配置运行

## 下载

搜索`mysql download`  
选择`MySQL Community Edition`  
在[Download MySQL Community Server][1]页面，选择macox平台，选择dmg文件(`macOS 10.14 (x86, 64-bit), DMG Archive`)下载  

## 安装

安装下载的dmg文件：`mysql-8.0.13-macos10.14-x86_64.dmg`  
使用新的密码方式，手机输入密码：liulei_0905，作为本地数据库root用户的密码  


## 配置面板

鼠标点击屏幕上左角屏幕图标，打开设置面板  
打开设置面板下方的mysql  
mysql控制面板分左右两部分，左部分控制mysql的启动、停止、数据库初始化、卸载等，右部分配置mysql，包括数据库目标、插件目标、日志目录等  
mysql安装和数据存储都在`/usr/local/mysql`目录  


## PATH

打开`~/.bash_profile`，添加`export PATH=${PATH}:/usr/local/mysql/bin`，把mysql的bin目录添加到PATH中  


## 命令行登录数据库

`mysql -u root -p`，指明以root用户登录本地数据库，输入安装时设置的密码  

本命令用于修改数据库登录密码
`ALTER USER 'root'@'localhost' IDENTIFIED BY 'MyNewPass'`;


## 坑

### mysql8 ：客户端连接caching-sha2-password问题
在安装mysql8的时候如果选择了密码加密，之后用客户端连接比如navicate，会提示客户端连接caching-sha2-password,是由于客户端不支持这种插件，可以通过如下方式进行修改：  
```sql
#修改加密规则  
ALTER USER 'root'@'localhost' IDENTIFIED BY 'password' PASSWORD EXPIRE NEVER; 
#更新密码（mysql_native_password模式）    
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '{NewPassword}';
```


[1]: https://dev.mysql.com/downloads/mysql/