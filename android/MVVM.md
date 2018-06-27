
# MVVM

参考：  
[浅谈 MVC、MVP 和 MVVM 架构模式](https://draveness.me/mvx)  
[如何构建Android MVVM应用程序](https://www.jianshu.com/p/2fc41a310f79)  


## 三种模式

### MVC
<img src="https://raw.githubusercontent.com/houchenl/notes/master/images/mvvm_mvc.png">

>**View**: 视图，用户界面，`xml`文件  
>**Model**: 数据保存，实体模型，`bean`文件  
>**Controllor**: 业务逻辑、数据处理、UI处理，`Activity`/`Fragment`  

**互动模式**：`View` 传送指令到 `Controller`，`Controller` 完成业务逻辑后，要求 `Model` 改变状态，`Model` 将新的数据发送到 `View`，用户得到反馈，所有通信都是单向的。  

`Android`框架本身就是`MVC`模式，但其中作为`View`的`xml`视图功能太弱，`Activity`/`Fragment`实际是`View`和`Controllor`的合体，既要负责视图显示，又要加入控制逻辑，承担功能过多，代码量太大。  


### MVP
<img src="https://raw.githubusercontent.com/houchenl/notes/master/images/mvvm_mvp.png">

>**View**: 对应`Activity`和`xml`，负责`View`的绘制和与用户交互  
>**Model**: 实体模型  
>**Presenter**: 负责完成`View`与`Model`之间的交互和业务逻辑  

**互动模式**：各部分之间的通信，都是双向的；`View` 与 `Model` 不发生联系，都通过 `Presenter` 传递；`View` 非常薄，不部署任何业务逻辑，称为"被动视图"（Passive View），即没有任何主动性，而 `Presenter`非常厚，所有逻辑都部署在那里。  

因为MVC模式中Activity/Fragment中代码过多，在MVP模式中，把部分业务逻辑代码从Activity/Fragment中移出，放在Presenter中，在Presenter中持有View(Activity/Fragment)的引用，然后在Presenter调用View暴露的接口对视图进行操作，这样实现了把视图操作和业务逻辑区分开来。MVP能够让Activity成为真正的View而不是View和Control的合体，Activity只做UI相关的事。但是这个模式还是存在一些不好的地方，比较如说：  
* Activity需要实现各种跟UI相关的接口，同时要在Activity中编写大量的事件，然后在事件处理中调用presenter的业务处理方法，View和Presenter只是互相持有引用并互相做回调,代码不美观。  
* 这种模式中，程序的主角是UI，通过UI事件的触发对数据进行处理，更新UI就有考虑线程的问题。而且UI改变后牵扯的逻辑耦合度太高，一旦控件更改（比较TextView 替换 EditText等）牵扯的更新UI的接口就必须得换。  
* 复杂的业务同时会导致presenter层太大，代码臃肿的问题。  


### MVVM
<img src="https://raw.githubusercontent.com/houchenl/notes/master/images/mvvm_mvvm.png">

>**View**: 对应于`Activity`和`xml`，负责`View`的绘制以及与用户交互  
>**Model**: 实体模型, 处理业务数据的获取、存储、数据状态变化、校验，vm会根据需要从`Model`中获取数据  
>**ViewModel**: 负责完成`View`与`Model`间的交互，负责业务逻辑  

**互动模式**：`MVVM` 模式将 `Presenter` 改名为 `ViewModel`，基本上与 `MVP` 模式完全一致。唯一的区别是，它采用双向绑定（data-binding）：`View`的变动，自动反映在 `ViewModel`，反之亦然。  

MVVM的关键技术是`Data Binding`。View的变化可以自动的反应在ViewModel，ViewModel的数据变化也会自动反应到View上。这样开发者就不用处理接收事件和View更新的工作，框架已经帮你做好了。MVVM模式的优势有：  
* 数据驱动: 在MVVM中，以前开发模式中必须先处理业务数据，然后根据的数据变化，去获取UI的引用然后更新UI，同样也是通过UI来获取用户输入，而在MVVM中，数据和业务逻辑处于一个独立的View Model中，ViewModel只要关注数据和业务逻辑，不需要和UI或者控件打交道。由数据自动去驱动UI去自动更新UI，UI的改变又同时自动反馈到数据，数据成为主导因素，这样使得在业务逻辑处理只要关心数据，方便而且简单很多。
* 低耦合度： MVVM模式中，数据是独立于UI的，ViewModel只负责处理和提供数据，UI想怎么处理数据都由UI自己决定，ViewModel 不涉及任何和UI相关的事也不持有UI控件的引用，即使控件改变（TextView 换成 EditText）ViewModel 几乎不需要更改任何代码，专注自己的数据处理就可以了，如果是MVP遇到UI更改，就可能需要改变获取UI的方式，改变更新UI的接口，改变从UI上获取输入的代码，可能还需要更改访问UI对象的属性代码等等。视图（View）可以独立于Model变化和修改，一个ViewModel可以绑定到不同的”View”上，当View变化的时候Model可以不变，当Model变化的时候View也可以不变。  
* 更新 UI:  在MVVM中，我们可以在工作线程中直接修改View Model的数据（只要数据是线程安全的），剩下的数据绑定框架帮你搞定，很多事情都不需要你去关心。  
* 团队协作: MVVM的分工是非常明显的，由于View和View Model之间是松散耦合的。一个是处理业务和数据，一个是专门的UI处理。完全有两个人分工来做，一个做UI（xml 和 Activity）一个写ViewModel，效率更高。  
* 可复用性: 一个View Model复用到多个View中，同样的一份数据，用不同的UI去做展示，对于版本迭代频繁的UI改动，只要更换View层就行，对于如果想在UI上的做AbTest 更是方便的多。  
* 单元测试: `View Model`里面是数据和业务逻辑，`View`中关注的是`UI`，完全没有彼此的依赖，可以对`ViewModel`进行单元测试。  



## MVVM和dataBinding

`dataBinding`是一种框架，`MVVM`是一种开发模式，两者概念不同。  
`dataBinding`是一个`google`提供的实现数据和UI绑定的框架，是一个实现`MVMM`模式的工具。有了`dataBinding`，`ViewModel`和`View`通过`dataBinding`来实现单向绑定和双向绑定，数据和UI之间实现动态监听和动态更新，在`android`中使用`MVVM`模式成为可能。  



## MVVM实现

<img src="https://raw.githubusercontent.com/houchenl/notes/master/images/mvvm_1.png">

### 1. 分工

#### View
View层做的就是和UI相关的工作，我们只在XML和Activity或Fragment写View层的代码，View层不做和业务相关的事，也就是我们的Activity 不写和业务逻辑相关代码，也不写需要根据业务逻辑来更新UI的代码，因为更新UI通过Binding实现，更新UI在ViewModel里面做（更新绑定的数据源即可），Activity 要做的事就是初始化一些控件（如控件的颜色，添加 RecyclerView 的分割线），Activity可以更新UI，但是更新的UI必须和业务逻辑和数据是没有关系的，只是单纯的根据点击或者滑动等事件更新UI(如 根据滑动颜色渐变、根据点击隐藏等单纯UI逻辑)，Activity（View层）是可以处理UI事件，但是处理的只是处理UI自己的事情，View层只处理View层的事。简单的说：View层不做任何业务逻辑、不涉及操作数据、不处理数据、UI和数据严格的分开。  

#### ViewModel
ViewModel层做的事情刚好和View层相反，ViewModel 只做和业务逻辑和业务数据相关的事，不做任何和UI、控件相关的事，ViewModel 层不会持有任何控件的引用，更不会在ViewModel中通过UI控件的引用去做更新UI的事情。ViewModel就是专注于业务的逻辑处理，操作的也都是对数据进行操作，这些个数据源绑定在相应的控件上会自动去更改UI，开发者不需要关心更新UI的事情。DataBinding 框架已经支持双向绑定，这使得我们在可以通过双向绑定获取View层反馈给ViewModel层的数据，并进行操作。再强调一遍ViewModel 不做和UI相关的事。  

#### Model
Model 的职责很简单，基本就是实体模型（Bean）同时包括Retrofit 的Service ，ViewModel 可以根据Model 获取一个Bean的Observable<Bean>( RxJava ),然后做一些数据转换操作和映射到ViewModel 中的一些字段，最后把这些字段绑定到View层上。从数据库和网络上获取数据的操作在Model中进行处理，ViewModel从Model中获取数据。  

### 2. 协作

#### ViewModel和View协作
<img src="https://raw.githubusercontent.com/houchenl/notes/master/images/mvvm_2.png">

ViewModel 和View 是通过数据绑定的方式连接在一起的。数据的绑定 DataBinding 已经提供好了，简单的定义一些ObservableField就能把数据和控件绑定在一起了（如TextView的text属性），但是DataBinding框架提供的不够全面，比如说如何让一个URL绑定到一个ImageView让这个ImageView能自动去加载url指定的图片，如何把数据源和布局模板绑定到一个ListView，让ListView可以不需要去写Adapter和ViewHolder 相关的东西，而只是通过简单的绑定的方式把ViewModel的数据源绑定到Xml的控件里面就能快速的展示列表呢？这些就需要我们做一些工作和简单的封装。前者可以通过BindingAdapter实现，后者可以使用自定义Adapter实现。  

#### ViewModel和Model协作
Model 是通过Retrofit 去获取网络数据的，返回的数据是一个Observable<Bean>( RxJava ),Model 层其实做的就是这些。那么ViewModel 做的就是通过传参数到Model层获取到网络数据（数据库同理）然后把Model的部分数据映射到ViewModel的一些字段（ObservableField），这些字段与View绑定  

#### ViewModel和ViewModel协作
ViewModel主要是用来处理业务和数据的，每个ViewModel都有相应的业务职责，但是在业务复杂的情况下，可能存在交叉业务，这时候就需要ViewModel和ViewModel交换数据和通信，这时候一个全局的消息通道就很重要的。  
比如，你的MainActivity对应一个MainViewModel，MainActivity 里面除了自己的内容还包含一个Fragment，这个Fragment 的业务处理对应于一个FragmentViewModel,FragmentViewModel请求服务器并获取数据，刚好这个数据MainViewModel也需要用到，我们不可能在MainViewModel重新请求数据，这样不太合理，这时候就需要把数据传给MainViewModel,那么应该怎么传，彼此没有引用或者回调。那么只能通过全局的消息通道Messenger。  


## Rxjava
推荐使用`MVVM`和 `RxJava`一块使用，虽然两者皆有观察者模式的概念，但是`RxJava`不使用在针对View的监听，更多是业务数据流的转换和处理。`DataBinding`框架其实是专用于`View-ViewModel`的动态绑定的，它使得我们的`ViewModel`只需要关注数据，而`RxJava`提供的强大数据流转换函数刚好可以用来处理`ViewModel`中的种种数据，得到很好的用武之地，使`ViewModel`的代码非常简洁同时易读易懂。



## ViewModel

ViewModel是MVVM框架的核心，包含下面4个部分：
> * Context （上下文）
> * Model (数据模型Bean)
> * Data Field （数据绑定）
> * Child ViewModel （子ViewModel）

### Context
`ViewModel`调用工具类、帮助类可能需要`context`参数:  
* 做网络请求我们必须把`Retrofit Service`返回的`Observable<Bean>`绑定到`Context`的生命周期上，防止在请求回来时`Activity`已经销毁等异常  
* 两个`ViewModel`之间的联系是通过`Messenger`来做，这个`Messenger`需要用到`Context`  

### Model
`Model`其实就是数据原型，也就是我们用`Json`转过来的`Java Bean`，我们可能都知道，ViewModel要把数据映射到View中可能需要大量对Model的数据拷贝，拿Model 的字段去生成对应的ObservableField（我们不会直接拿Model的数据去做展示），这里其实是有必要在一个ViewModel 保留原始的Model引用，这对于我们是非常有用的，因为可能用户的某些操作和输入需要我们去改变数据源，可能我们需要把一个Bean 从列表页点击后传给详情页，可能我们需要把这个model 当做表单提交到服务器。这些都需要我们的ViewModel持有相应的model。

### Data Field
`Data Field`就是需要绑定到控件上的`ObservableField`字段，无可厚非这是`ViewModel`的必须品。  

### Child ViewModel
子`ViewModel`的概念就是在`ViewModel`里面嵌套其他的`ViewModel`，这种场景还是很常见的。比如说你一个`Activity`里面有两个`Fragment`，`ViewModel` 是以业务划分的，两个`Fragment`做的业务不一样，自然是由两个`ViewModel`来处理，`Activity`本身可能就有个`ViewModel`来做它自己的业务，这时候`Activity`的这个`ViewModel`里面可能包含了两个`Fragment`分别的`ViewModel`。这就是嵌套的子`ViewModel`。还有另外一种就是对于`AdapterView` 如`ListView`, `RecyclerView`, `ViewPager`等。  
如果一个页面业务非常复杂，不要把所有逻辑都写在一个`ViewModel`，可以把页面做业务划分，把不同的业务放到不同的`ViewModel`，然后整合到一个总的`ViewModel`，这样做起来可以使我们的代码业务清晰，简短意赅，也方便后人的维护。  



## 要求
1. 每个`ViewModel`的行为应当是可预期的，可以使用`JUnit`对其进行单元测试。  
2. 业务部分分为一个个`MVVM`单元，每个单元由ViewModel(VM)、ViewModel的状态(M)和布局资源文件组成(V)。MVVM单元可以嵌套。  
3. `ViewModel`只处理数据，不能持有任何`UI`组件的引用和操作。  
4. `Activity`只处理UI相关操作，不处理任何数据操作。  
5. `ViewModel`与`Activity`相互独立，一个`ViewModel`可供多个`Activity`使用。  

