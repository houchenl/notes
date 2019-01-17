
# weex开发环境搭建与入门.md


## 安装工具

1. 确保安装了`nodejs`和`npm`  

2. 然后全局安装`weex-toolkit`。`weex-toolkit`是`weex`提供的命令行工具，帮助开发者创建空项目、初始化ios和android开发环境、调试、安装插件等。  
```bash
npm install weex-toolkit -g
```
安装命令向本地电脑命令行环境注册一个`weex`命令。  


## 创建项目

3. 用`weex create`命令创建一个空的模板项目，命令执行完成后，在当前目录的`awesome-app`文件夹里就有了一个空的weex+vue.js项目。  
```bash
weex create awesome-app
```
然后进入项目根目录，运行`npm install`安装依赖模块  


## 开发

4. 进入`awesome-app`目录，在`src`子目录中创建`hello.vue`页面，并开发。  

5. 开发完成后，在`awesome-app`根目录，执行`npm run build`，编译工程。编译完成后，会在工程根目录生成`dist`目录。开发的`hello.vue`页面，编译后，会在`dist`下生成对应同名`hello.js`文件。js文件渲染后生成原生页面，用于显示。  


## 渲染显示

6. 本地引用渲染。把`hello.js`文件复制到android工程的`src/main/assets/`目录下，activity中使用下面代码加载渲染：  
```java
String fileContent = WXFileUtils.loadAsset("hello.js", this);
mInstance.render("helloWeex", fileContent, null, null, WXRenderStrategy.APPEND_ASYNC);
```

7. 网络引用渲染。把`hello.js`文件上传到服务器，生成链接url，activity中使用下载代码加载渲染:  
```java
mInstance.renderByUrl("helloWeex", url, null, null, WXRenderStrategy.APPEND_ASYNC);
```

8. 显示。渲染完成后，会回调`public void onViewCreated(WXSDKInstance instance, View view)`方法，其中参数view表示渲染完成后的页面。然后把view作为contentView，或者添加到已存在容器中即可。  
```java
    @Override
    public void onViewCreated(WXSDKInstance instance, View view) {
        setContentView(view);
        mLayoutContainer.addView(view);
    }
```

