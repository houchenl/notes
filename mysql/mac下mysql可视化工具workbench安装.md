
# mac下mysql可视化工具workbench安装使用


## 下载

搜索`mysql download`，打开[MySQL Downloads][1]页面  
在[MySQL Downloads][1]页面，选择MySQL Community Edition (GPL)，打开[MySQL Community Downloads][2]页面  
在[MySQL Community Downloads][2]页面，选择`MySQL Workbench`，打开workbench下载页面[Download MySQL Workbench][3]  
在[Download MySQL Workbench][3]页面，下载dmg文件  


## 打开连接

打开workbench，在主面板，默认有一个连接，Server instance是本地数据库，端口号是3306，用户是root，密码是安装mysql时设置的密码。点击该连接即可打开数据库。  


## 创建数据库

点击面板上的创建数据库图标，输入Schema名称，再点击apply。  

在数据库上右键，可以将选定数据库设置为默认数据库。  


## 创建数据表

打开数据库，在数据表上右键，选择新建数据表。  
对表中每个字段，输入字段名，选择字段数据类型，选择字段特性(如Not NULL等)，设置默认值  
添加完字段后，点击apply，workbench自动生成sql语句，再次点击apply，表创建完成。  


## 查看数据表

在数据表上右击，选择Select Rows，即可在Result Grid中显示数据表中数据  


## 写入数据

在Result Grid中，在NULL处编辑，写入数据，点击apply，workbench自动生成sql语句，再次点击apply，数据写入完成。  


## 运行sql

在`Result Grid`上方sql面板区，输入sql语句，然后点击闪电图标的运行按钮




[1]: https://www.mysql.com/downloads/
[2]: https://dev.mysql.com/downloads/
[3]: https://dev.mysql.com/downloads/workbench/