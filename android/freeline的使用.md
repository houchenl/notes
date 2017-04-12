
# freeline的使用

freeline是蚂蚁聚宝出品的快速编译工具，作为instant run的替代品。

## 集成方法：

### 1. 安装freeline插件
在android studio设置中，安装freeline插件。安装完成后，会在工具栏中显示freeeline的工具图标。

### 2. 根目录build.gradle
修改项目根目录的build.gradle
```
buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.antfortune.freeline:gradle:0.8.7'
    }
}
```

### 3. 模块build.gradle
修改可运行模块的build.gradle，使用freeline的plugin。  
`apply plugin: 'com.antfortune.freeline'`

## 注意

- 若工程中有多个可运行模块，同一时间，只能有一个可运行模块的build.gradle中的`apply plugin: 'com.antfortune.freeline'`处于打开状态。若多个可运行模块同时应用该plugin，会报错。

## 使用

使用时，点击studio工具栏中freeline图标即可。

