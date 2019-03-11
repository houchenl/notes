
# 安装卸载python3


## 安装
从官网下载安装包安装  


## 配置Sublime
启动Sublime Text，并选择菜单ToolsBuild SystemNew Build System，这将打开一 个新的配置文件。删除其中的所有内容，再输入如下内容:  
```json
{
    "cmd": ["/usr/local/bin/python3", "-u", "$file"]
}
```
这些代码让Sublime Text使用命令python3来运行当前打开的文件。请确保其中的路径为你在 前一步使用命令type -a python3获悉的路径。将这个配置文件命名为Python3.sublime-build，并 将其保存到默认目录——你选择菜单Save时Sublime Text打开的目录。  
最后使用`command + B`命令，在sublime中运行代码  


## 查看
输入`type -a python3`，查看已安装`python3`  
```python
python3 is /Library/Frameworks/Python.framework/Versions/3.6/bin/python3
python3 is /Library/Frameworks/Python.framework/Versions/3.7/bin/python3
python3 is /usr/local/bin/python3
```

查看已安装`python3`版本: `python3 --version`，输出`Python 3.6.8`  

## 卸载python3

> 删除`python3.x`框架  
```bash
ls /Library/Frameworks/Python.framework/Versions/
3.6 3.7

sudo rm -rf /Library/Frameworks/Python.framework/Versions/3.6
sudo rm -rf /Library/Frameworks/Python.framework/Versions/3.7
```

> 删除`Python 3.x` 应用目录  
```bash
sudo rm -rf /Application/Python 3.6
```
此时，launchpad中python3的IDLE就被删除了  

> 删除/usr/local/bin 目录下指向的`Python3.x` 的连接  
```bash
cd /usr/local/bin/
ls -l /usr/local/bin
rm Python3.x相关的文件和链接
```

> 删除python的环境路径  
```bash
vi ~/.bash_profile
``
```

> 确认python是否已经删除  
```bash
~ python3
zsh: command not found: python3
```

