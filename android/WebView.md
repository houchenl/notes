
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

### webview页面异常处理
webView页面加载异常分为三种情况，一是网络不可用，二是连接服务器异常，包括超时，三是服务器返回errorCode。   
第一种情况，移动端检查网络连接，如果网络不可用，隐藏webview，显示异常提醒。   
第二种情况，重写onReceivedError方法，在这个方法中，隐藏webview，显示异常提醒。这个方法只会在服务器连接异常时才会调用。这个方法有两个重载方法，一个老方法，已废弃，一个新方法，但只可在23以上系统中才能运行。为避免两个方法同时被执行，在老方法中添加判断，只在系统版本小于23时才执行操作。新方法不仅在页面加载失败时会调用，而页面中的小组件加载失败才会调用，为避免重复调用，需要判断失败时的url。示例代码如下：
``` java
/**
 * 无法连接到服务器，可能是网络中途原因，也可能是服务器连接网络异常原因。
 * 在sdk<23的系统版本中，该方法不会被调用
 */
@TargetApi(Build.VERSION_CODES.M)
@Override
public void onReceivedError(WebView view, WebResourceRequest request, WebResourceError error) {
    super.onReceivedError(view, request, error);

    if (request.isForMainFrame()) {// 或者： if(request.getUrl().toString().equals(getUrl()))
        // 在这里显示自定义错误页
//      view.loadUrl("file:///android_asset/connection_error.html");//添加显示本地文件
        onLoadFailRefresh("网络连接失败，请检查网络连接", R.drawable.img_network_unavailable);
    }
}

/**
 * 在所有系统版本中，该方法都会被调用
 * 只有当连接不上服务器时，该方法才会被调用。如果网址ip:port正确，但path不正确，该方法不会被调用。
 * 只有当ip:port不正确，连接不上服务器时，onReceivedError才会被调用。
 */
@SuppressWarnings("deprecation")
@Override
public void onReceivedError(WebView view, int errorCode, String description, String failingUrl) {
    super.onReceivedError(view, errorCode, description, failingUrl);

    // 避免在sdk>=23时，两个方法同时执行，造成重复设置
    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) {
        return;
    }

//  view.loadUrl("file:///android_asset/connection_error.html");//添加显示本地文件
    onLoadFailRefresh("网络连接失败，请检查网络连接", R.drawable.img_network_unavailable);
}
```
第三种情况，当成功连接到服务器，但是服务器返回异常时执行onReceivedHttpError方法。但该方法只在系统版本大于21时才会执行，在低版本手机上如何监测这种异常还待查找方法。
``` java
/**
 * http请求资源过程中服务器返回的错误，http errors 的statusCode >= 400。
 * 只有在有网络连接，请连接服务器成功的情况下，才会有服务器返回这类错误。
 * 这是第三种错误，第一种是本机无网络连接，第二种是有网络连接但连不上服务器，可能是网络原因也可能是服务器原因，第三种是连上了服务器但服务器无法返回正确资源。
 * 简单说就是：无网络、网络异常、服务器异常。
 * statusCode只有在api大于等于21时才能获得，所以需要做服务器异常的能用处理。
 * 在sdk < 21版本的系统中，该方法不会被调用。
 */
@TargetApi(Build.VERSION_CODES.LOLLIPOP)
@Override
public void onReceivedHttpError(WebView view, WebResourceRequest request, WebResourceResponse errorResponse) {
    super.onReceivedHttpError(view, request, errorResponse);

    if (request.isForMainFrame()) {
//      view.loadUrl("file:///android_asset/http_error.html");//添加显示本地文件
        onLoadFailRefresh("服务器开小差了，请重新尝试", R.drawable.img_network_unavailable);
    }
}
```

### shouldOverrideUrlLoading
