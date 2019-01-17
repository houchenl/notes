
# 填坑指南

## Environment variable $ANDROID_HOME not found !
> 15:19:50 : Environment variable $ANDROID_HOME not found !
> You should set ANDROID_HOME in your environment first.
> See https://spring.io/guides/gs/android/

在weex项目下，执行`weex run android`命令时，报上述错误。  
但实际上`ANDROID_HOME`环境变量已经配置，且`echo ANDROID_HOME`也能正常打出。  
此时，仍报上面的错误，原因不明。？？？  


## onException: createInstanceReferenceError: Vue is not defined [fixed]
安卓加载js文件时，报这个错误。  
解决：使用weex sdk的>= 0.10.0的新版本即可，如`com.taobao.android:weex_sdk:0.18.0`  


## Warning: npm 5 is not supported yet! [fixed]
We recommend using npm 4 until some bugs are resolved.  
To install npm 4, you can just run `npm i npm@4 -g` or use `n` to manage your npm version with node.  


## weex调用原生方法不起作用
vue文件中，点击事件用@click，而不能用onclick。`<text @click="click">testMyModule2</text>`  


## android虚拟机运行正常，在小米手机上运行界面空白
原因不明？？？    


## java.io.IOException: Cleartext HTTP traffic to yulinwork.oss-cn-hangzhou.aliyuncs.com not permitted
加载服务器js文件时，如果url协议头为http://，会报上面错误，因为从Android 6.0开始引入了对Https的推荐支持，与以往不同，Android P的系统上面默认所有Http的请求都被阻止了。所以服务器js文件的url头文件需要为https://。也可以使用http://头文件的url，同时在androidmanifest.xml的application中添加`android:usesCleartextTraffic="true"`，默认为false。  



