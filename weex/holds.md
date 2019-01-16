
# 填坑指南


## Environment variable $ANDROID_HOME not found !

> 15:19:50 : Environment variable $ANDROID_HOME not found !
> You should set ANDROID_HOME in your environment first.
> See https://spring.io/guides/gs/android/

在weex项目下，执行`weex run android`命令时，报上述错误。  
但实际上`ANDROID_HOME`环境变量已经配置，且`echo ANDROID_HOME`也能正常打出。  
此时，仍报上面的错误，原因不明。？？？  


## onException: createInstanceReferenceError: Vue is not defined

安卓加载js文件时，报这个错误，原因不明。？？？  
