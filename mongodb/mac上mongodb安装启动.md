
### 下载安装
使用homebrew安装   
brew update   
brew install mongodb

### 启动
homebrew安装mongodb成功后，会自动创建/usr/local/etc/mongodb.cfg文件，文件内容包含log存放位置和数据库文件存储位置。启动时，--config属性指定为这个文件，可以使用指定的log位置和db位置启动服务。如果要在后台运行服务，加上--fork参数。
sudo mongod --fork --config /usr/local/etc/mongod.conf

### 连接
mongo
