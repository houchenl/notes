
# weex

## 资源
[weex官网首页][1]  
[Release Note | Weex][3]  
[jcenter | weex][2]  


## 基础使用

创建项目： `weexpack create appName`  
创建页面： 在`项目根目录/src/`目录下创建文件`fileName.vue`，并编辑  
编译页面： 在项目根目录执行`npm run build`，编译整个应用，src/下vue文件被编译，在项目根目录/dist/下生成同名对应js文件  
集成渲染： 把dist/下的js文件复制到android应用src/main/assets/目录下，应用中加载文件，并渲染。渲染成功后，把生成的view添加到原生页面已有布局中，或直接显示view。  
```java
String fileContent = WXFileUtils.loadAsset("test.js", this);
mInstance.render("helloWeex", fileContent, null, null, WXRenderStrategy.APPEND_ASYNC);

setContentView(view);    // view为渲染生成的页面
```


## weex调用原生

1. 原生继承WXModule类，类在app中注册到wxenginesdk中。因为weex通过反射调用原生方法，所以类内方法需要定义为public。并且方法上要加注解@JSMethod(uiThread = true)，表明方法是否在UI线程执行。  
```java
public class MyModule extends WXModule {

    @JSMethod(uiThread = true)
    public void printLog(String msg) {
        Toast.makeText(mWXSDKInstance.getContext(), msg, Toast.LENGTH_SHORT).show();
    }

}
```

2. 在Application中注册自定义Module类：`WXSDKEngine.registerModule("myModule", MyModule.class);`  

3. 在vue文件中，点击文本时，调用本模块内js方法。在js方法中，获取原生注册的模块，并调用原生方法。点击文本的点击事件使用@click触发。  
```xml
<template>
  <div>
    <text @click="click">testMyModule</text>
  </div>
</template>

<script>
  module.exports = {
    methods: {
      click: function() {
        weex.requireModule('myModule').printLog("I am a weex Module");
      }
    }
  }
</script>
```


## 踩坑指南

### Environment variable $ANDROID_HOME not found !

> 15:19:50 : Environment variable $ANDROID_HOME not found !
> You should set ANDROID_HOME in your environment first.
> See https://spring.io/guides/gs/android/

在weex项目下，执行`weex run android`命令时，报上述错误。  
但实际上`ANDROID_HOME`环境变量已经配置，且`echo ANDROID_HOME`也能正常打出。  
此时，仍报上面的错误，原因不明。？？？  


### onException: createInstanceReferenceError: Vue is not defined [fixed]

安卓加载js文件时，报这个错误。  
解决：使用weex sdk的>= 0.10.0的新版本即可，如`com.taobao.android:weex_sdk:0.18.0`  


### Warning: npm 5 is not supported yet! [fixed]

We recommend using npm 4 until some bugs are resolved.  
To install npm 4, you can just run `npm i npm@4 -g` or use `n` to manage your npm version with node.  


### weex调用原生方法不起作用

vue文件中，点击事件用@click，而不能用onclick。`<text @click="click">testMyModule2</text>`  


## 注意
- android引用的`weex sdk`版本号需要大于`0.5.0`，最好使用最新版本，如`0.18.0`  
- weex不支持标签作为选择器，只支持className作为选择器  


## 总结
- 可以快速开发简单页面  
- 不适合开发复杂应用  
- 有些功能需要使用原生代码，如status bar的属性更改，开场动画的制作，内存的回收，webview的监听等  



[1]: https://weex.incubator.apache.org/cn/
[2]: http://jcenter.bintray.com/com/taobao/android/weex_sdk/
[3]: http://weex-project.io/releasenote.html