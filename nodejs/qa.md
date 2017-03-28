
# nodejs问题集结

### 全局安装的模块引用时查找不到
- node/npm命令安装在/usr/local/bin
- npm install -g xxx，以这种方式全局安装模块，模块被安装在/usr/local/lib/node_modules中
- 查找模块时，node从当前目录搜索node_modules目录，如果查找不到，就到上级目录查找，直到根目录。如果直到根目录都查找不到，就到NODE_PATH指定的目录中查找。mac中报查找不到全局安装的模块，是因为全局安装的模块不在当前目录及各级父目录的node_modules中，而NODE_PATH为空，所以查找不到。
- 解决方法是把全局模块安装路径添加到NODE_PATH中。cd到用户主目录，创建.profile文件，添加/usr/local/lib/node_modules到NODE_PATH中，然后重新打开终端，令设置生效，就可以正常查找到模块了。
```
export NODE_PATH=/usr/local/lib/node_modules
```
