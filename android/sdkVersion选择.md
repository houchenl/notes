
### minSdkVersion
应用可以运行的最低版本要求，是设备用来判断用户是否可以安装应用的标志。必须保证应用调用的所有api都可以运行在minSdkVersion指定的系统版本上，否则，在低版本手机上运行时如果调用了不存在的api，会出现运行时异常。应用所指定的minSdkVersion必须大于等于所引用库的minSdkVersion。

### targetSdkVersion

### compileSdkVersion
compileSdkVersion告诉gradle使用哪个版本的sdk进行编译。sdk版本不同，可使用的api也不同，高版本的sdk包含更新的api，废弃的api在高版本中会提示不可用。尽量总是使用最新的sdk进行编译，以使用最新的api，并对废弃的api做出提示。compileSdkVersion指定的版本需大于等于supportLibrary的版本。   
minSdkVersion <= targetSdkVersion <= compileSdkVersion   
minSdkVersion <= targetSdkVersion == compileSdkVersion

### buildToolsVersion

### 参考资料
[<uses-sdk> | Android Developers](https://developer.android.com/guide/topics/manifest/uses-sdk-element.html?utm_campaign=adp_series_sdkversion_010616&utm_source=medium&utm_medium=blog#target)