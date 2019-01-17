
# weex

## 资源
[weex官网首页][1]  
[Release Note | Weex][3]  
[jcenter | weex][2]  
[vue - playground][4]  


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

## 模块
对于不依赖于UI的功能，weex推荐将它们包装到模块中，然后使用`weex.requireModule('xxx')`引入。模块分为weex内置模块和原生模块，原生模块通过注册集成到weex中。weex通过原生模块调用原生功能，如网络、存储、剪贴板和页面导航等。  


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
[4]: http://dotwe.org/vue