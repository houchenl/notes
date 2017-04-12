
# mac 常见操作

### 1. 改变文件默认打开方式
右键点击文件 - 显示简介 - 打开文件 - 选择打开方式 - 全部更改 - 确定

### 2. 在sublime新标签页中打开文件
Sublime Text - Preferences - Settings User，添加"open_files_in_new_window": false

### 3. shell 退出
mac默认情况下，在shell中输入exit时，不会自动关闭shell窗口。为了实现自动关闭，需要做如下设置：终端 - 偏好设置 - 描述文件 - shell - 当shell退出时 - 关闭窗口。

### 4. 定时关机
关机
sudo shutdown -h yymmddhhmm
重启
sudo shutdown -r  yymmddhhmm
休眠
sudo shutdown -s yymmddhhmm

### 5. sublime tab设置为4个空格
// 注意只有一个大括号，如果之前有属性，如在之前的属性前确保有 ，(逗号)
"tab_size": 4,
"translate_tabs_to_spaces": true,
//若要在保存时自动把tab 转换成空格，请把下面一行设置成 true，如不需要: 设置成 false
"expand_tabs_on_save": true