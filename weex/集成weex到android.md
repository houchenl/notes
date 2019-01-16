
# [集成weex到android][2]

## 接入weex
```gradle
compile 'com.android.support:recyclerview-v7:23.1.1'
compile 'com.android.support:support-v4:23.1.1'
compile 'com.android.support:appcompat-v7:23.1.1'
compile 'com.alibaba:fastjson:1.1.46.android'
compile 'com.taobao.android:weex_sdk:0.5.1@aar'
```
> `weex sdk`版本号需要大于`0.5.0`，最好使用最新版本，如`0.18.0`。[查看weex最新版本][1]  
> 版本可以高不可以低  

## 初始化

1. 添加网络访问权限  
```
<uses-permission android:name="android.permission.INTERNET" />
```

2. 实现`IWXImgLoaderAdapter`下载图片接口，在初始化时使用  

3. 在`Application`中初始化，初始时指定图片下载器。也可以在初始化时注册原生模块到weex。  
```java
@Override
public void onCreate() {
    InitConfig config = new InitConfig.Builder().setImgAdapter(new FrescoImageAdapter()).build();
    WXSDKEngine.initialize(this, config);

    try {
        WXSDKEngine.registerModule("myModule", MyModule.class);
    } catch (WXException e) {
        e.printStackTrace();
    }
}
```

## 渲染

1. 创建`WXSDKInstance`实例；  
```java
WXSDKInstance mWXSDKInstance = new WXSDKInstance(this);
```

2. 注册渲染监听器；  
```java
mWXSDKInstance.registerRenderListener(this);
```

3. 开始渲染；  
```java
// 1. 渲染本地assets中js文件
String fileContent = WXFileUtils.loadAsset("image.js", this);
mInstance.render("LoadImageActivity", fileContent, null, null, WXRenderStrategy.APPEND_ASYNC);

// 2. 渲染网络js文件
String url = "https://yulinwork.oss-cn-hangzhou.aliyuncs.com/test/image.js";
mInstance.renderByUrl("LoadImageActivity", url, null, null, WXRenderStrategy.APPEND_ASYNC);
```

4. 渲染成功后显示view；  
```java
@Override
public void onViewCreated(WXSDKInstance instance, View view) {
    // 1. 把渲染结果作为整个contentView显示
    setContentView(view);

    // 2. 把渲染结果作为原生布局的一部分显示
    mLayoutContainer.addView(view);
}
```


[1]: http://jcenter.bintray.com/com/taobao/android/weex_sdk/
[2]: https://weex.incubator.apache.org/cn/guide/integrate-to-your-app.html