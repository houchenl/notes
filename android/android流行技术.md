
# android 流行技术


## 图片加载库

### Universal-Image-Loader
早期广泛被用的一个可重复使用的异步图像加载、缓存、显示。作者已经停止维护。  

### Picasso
谐音"毕加索",听起来就很艺术，是 Square开源的项目，主导者是是Android大神JakeWharton  

### Glide
是google员工在Picasso基础上进行优化，总体比Picasso更优秀，在Google很多项目在用。  

### Fresco
FaceBook的明星项目，也是去年最火的项目之一，匿名共享缓存等机制保证低端机表现极佳，但是源代码基于C/C++。  


## 异步分发通信库

### EventBus
是一个发布、订阅的轻量级事件总线框架，基于观察者模式的实现的线程通信框架。  

### RxJava
一个在 Java VM 上使用可观测的序列来组成异步的、基于观察者模式的实现的库。  

### RxAndroid
函数响应式编程， 把 RxJava 带到 Android 环境中。很多时候，编写 Android 程序，你也可以看成是数据的处理和流动，换一种思想编程，曾经看起来很棘手的问题，瞬间就很优雅的解决了，相信你会被这种build模式的开发会越来越爱。  

### RxBinding
是 Jake Wharton 的一个开源库，它提供了一套在 Android 平台上的基于 RxJava的 Binding API。所谓 Binding，就是类似设置 OnClickListener 、设置 TextWatcher 这样的注册绑定对象的 API。  


## 新技术语言

### Kotlin
作为 Android 领域的 Swift，绝对让你如沐新风。抛弃沉重的 Java 语法，Kotlin 融入了很多现代编程语言的思想，作为开发者，接受新的语言，了解新语言的发展趋势，更有利于开阔你的思路和加深对语言的理解。在 Android 开发上，使用 Kotlin 并不会让你付出什么代价，为什么不来试试？ 使用Kotlin进行Android开发。  

### React Native
跨平台一直是开发者的梦想，而且移动应用的跨平台解决方案目前也很多，在Facebook 的参与和力推下，让这个解决方案带上了光环。第一个用 React Native 开发的 App 已经在 Google Play 上架 Facebook 广告管理工具，听说 Android 的 SDK 也马上会到来，国内天猫团队以及在去年10月首次实现，携程也基于React Native推出mouse, 相信不久后会有更多的框架封装的出现。但是，在2018年6月20号，Airbnb 技术团队在 Medium 上宣布，Airbnb 放弃使用 React Native，将回归到使用基于原生技术的自有框架开发 App。  

### flutter
是一款能够简单、高效地开发优美的移动APP的UI框架。在2018年2月27日，在2018世界移动大会上，Google发布了Flutter的第一个Beta版本。Flutter是Google用以帮助开发者在IOS和Android两个平台开发高质量原生应用的全新移动UI框架。
Sky，与 React Native 类似，使用 Web 开发语言来做移动平台的开发，虽然这个只是一个尝试，但是这是 Google 自身推出的，特别是在 Java 语言的使用上败诉之后，这可能会有一些作为呢。  

### Hybrid
完全使用 H5 开发 App，目前已很成熟，但是体现并不很好。可以短时间内更新APP UI，适配能力超强，但是基于流量严重，但是折中方案在很多情况下是非常适合的，典型的就是淘宝微信，大部分信息展示都是通过 H5 来完成，同时通过 Hybird 方式，把 Web 和 Native 打通，提供给网页访问Native的能力。  

### 插件化
针对大型 Android 项目，很多 App 开始使用插件来分模块构建相对独立的功能。  

### 热修复
线上更新app


## 注入注解框架

### Dagger2
与Spring 的IOC差不多吧。这个框架它的好处是它没有采用反射技术（Spring是用反射的）,而是用预编译技术，因为基于反射的DI非常地耗用资源（空间，时间）。  

### Butterknife
出自大神JakeWharton，绑定视图和回调字段和方法。例如，减少了findViewById()的繁琐操作。  


## 设计模式

### MVP
因为 Android 并没有严格的业务和界面区分，项目一庞大，就很容易使代码结构显得越来越乱。现在 Android 端对 MVP 模式讨论越来越热，谷歌6.0API以及更多的体现了MVP设计思维，觉得 MVP 是非常适合 Android 上的APP 开发。  

### MVVM
这是因为开始官方支持 DataBinding，把 MVVM 直接带到 Android 中。数据绑定在 Windows WPF 和 Web （尤其JSP中）已经非常常见，它非常高效的开发效率，让你只关心你的数据和业务。这也对 Android 开发来说，无疑是一个非常重大的里程碑  

### Android Architecture Component
Android架构组件是一些库的集合，它能够帮助你构建出健壮、可测试、稳定的app，它们可以被用来管理你的Ui组件生命周期和处理数据持久化存储。  

### Android Jetpack
Google发布了Android Jetpack，这是我们的新一代 组件、工具和架构指导 ，旨在加快开发者的 Android 应用开发速度。  

这对 Android原生开发 来讲，无疑是个 重磅炸弹，众所周知，2017年的GoogleIO大会上，官方为Android开发者推出了 一系列的 架构组件，比如 Lifecycle， LiveData，Room，ViewModel等，旨在推出  官方认证 的 Android架构 方案。Android Jetpack的推出，从 架构（Architecture）， UI，基础（Foundation）及行为（Behavior）四个方面，推出了一套  官方认证 的 开发体系。  


## UI框架

### BaseRecyclerViewAdapterHelper
RecyclerView万能适配器。  
- PinnedSectionItemDecoration：强大的粘性标签库  
- EasyRefreshLayout：    轻松实现下拉刷新和上拉更多  
- EasySwipeMenuLayout：仿IOS侧滑删除  

### SmartRefreshLayout
下拉刷新、上拉加载、二级刷新、淘宝二楼、RefreshLayout、OverScroll，Android智能下拉刷新框架，支持越界回弹、越界拖动，具有极强的扩展性，集成了几十种炫酷的Header和 Footer。 也吸取了现在流行的各种刷新布局的优点，包括谷歌官方的 SwipeRefreshLayout，其他第三方的 Ultra-Pull-To-Refresh、TwinklingRefreshLayout 。还集成了各种炫酷的 Header 和 Footer。  

### android-gif-drawable
用于在Android上显示动画GIF的视图和Drawable。  

### PhotoView
用于在Android上通过各种触摸手势实现支持缩放的图片的框架。  


## 网络请求库

### okhttp
在Android开发中，它已经成为眼下最火的http请求框架了。  

### Retrofit
与okhttp共同出自于Square公司，retrofit就是对okhttp做了一层封装。把网络请求都交给给了Okhttp，我们只需要通过简单的配置就能使用retrofit来进行网络请求了，其主要作者也是Android大神JakeWharton。  


## 日志打印库

### logger
简单,漂亮的android和强大的记录器  


## 权限请求库

### RxPermissions
API23以上Android 6.0项目分为普通权限和危险权限，该库在项目运行时动态进行权限请求，支持RxJava2。  


## SQLite数据库

### LitePal
一个Android库,使得开发人员使用SQLite数据库非常容易。  

