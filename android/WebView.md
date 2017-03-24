
### WebView重定向

### WebView与原生交互

#### 1. 原生调h5
```
String url = "javascript:functionName(param1, param2)";
mWebView.loadUrl(url);
```

#### 2. h5调原生
```
aliasName.methodName(param1, param2);
```

#### 3. 原生支持h5调用需要做的事情
- 定义类
- 类中方法加public void修饰，且加上@JavascriptInterface注释
- 类方法中如调用UI组件，需把调用放在activity.runOnUIThread()中
- 初始化时，创建类的对象[JSCallLocal]，并为其起个别名[external]。
```
mWebView.addJavascriptInterface(new JSCallLocal(), "external")
```
