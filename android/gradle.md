
# gradle
如果创建一个使用gradle构建的android project，需要写一个构建脚本，供gradle使用，这个脚本命名为build.gradle。项目根目录和各子级模块都有一个build.gradle。  
gradle脚本，是一种基于groovy的动态DSL，而groovy是一种基于jvm的动态语言。

### gradle与android-plugin-gradle
- gradle在指定根目录gradle-wrapper.properties中指定。
- android-plugin-gradle在项目根目录的build.gradle文件中指定。在buildscript -> dependencies -> classpath中，指定android-plugin-gradle的版本，如`'com.android.tools.build:gradle:2.3.1'`。
- android-plugin-gradle与gradle之间有一定的依赖关系，不能随便指定。