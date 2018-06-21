
# databinding


## 配置

### 支持版本
Data Binding library 是一个support library，可以在Android 4.0(API 14)及以上版本的设备中运行。  

Data Binding 支持Android Plugin for Gradle 1.5及以上版本，最好使用最新版本。  

### 配置
要使用Data Binding，首先需要在Android SDK manager中下载Data Binding support library。然后，在项目中使用Data Binding功能的模块的build.gradle文件中，添加如下元素：
```
android {
    ...
    dataBinding {
        enabled = true
    }
}
```
所有直接或间接使用Data Binding功能的模块，都需要添加这一配置。  

### New data binding compiler for binding classes
Android Gradle plugin version 3.1.0-alpha06包含了一个新的data binding编译器。新的编译器使用增量编译，在大多数情况下，可以加快编译速度。之前的data binding编译器在managed code编译的同时生成binding类，如果managed code编译失败，就可能会造成大量的提示binding类找不到的错误，新的编译器在managed code编译之前生成building代码，可以避免这个问题。在`gradle.properties`中加入下面一行，以使用新的编译器：
```
android.databinding.enableV2=true
```


## 基础使用

### 定义ViewModel-1
layout-data中引用的java类，向layout提供简单string类型数据时，view中使用方式是`android:text="@{user.firstName}"`，java类中定义有3种方式：  

1. 将字段定义成public，这时不用提供get方法。`public String firstName;`  

2. 字段定义为private，提供get方法。
``` java
private String mFirstName;

public String getFirstName() {
    return mFirstName;
}
```

3. 字段定义为private，提供get方法，方法名为字段名。
``` java
private String mFirstName;

public String firstName() {
    return mFirstName;
}
```

3种方式同时存在时，优先级为：第2种 > 第3种 > 第1种，即`getFirstName() > firstName() > firstName`  


### 布局文件使用ViewModel类
在原来的布局外，加一层layout布局。然后把命名域移到最外层layout标签中。  
```
<layout>
    <data>
        <variable name="user" type="com.yulin.UserInfo"/>
    </data>
    // 原来的layout
    <TextView
        layout_width="wrap_content"
        layout_height="wrap_content"
        android:text="@{user.firstName}">
    </TextView>
</layout>
```

### 关联布局文件和ViewModel对象
把原来的setContentView替换为DataBindingUtils.setContentView。方法返回值是ViewDataBinding的子类型，类型名根据layout文件名生成，生成规则是去掉单词间下划线，再把每个单词首字母大写，最后加上Binding后缀。  
如果是在Fragment/ListView/RecyclerView中，获取ViewDataBinding需要使用inflate方法，如下：
``` xml
ListItemBinding binding = ListItemBinding.inflate(layoutInflater, viewGroup, false);
// or
ListItemBinding binding = DataBindingUtil.inflate(layoutInflater, R.layout.list_item, viewGroup, false);
```

通过binding可以直接访问layout中有id的控件。并且为布局-layout-data-variable设置对象，为布局文件使用的ViewModel指定实际实现对象。  
为控件设置数据，有三种方式：  
1. 通过binding拿到指定id控件，然后直接针对控件进行操作，像传统使用方式。  
2. 使用binding.setVariable(BR.variableName, userInfo)，这是通用方式。  
3. 使用binding.setEmployee(employee)，这是简易方式。  

### 更新数据
绑定数据后，再修改数据，view上显示的数据会自动随着数据变化而变化，默认具有从数据向view单向传递变动的能力。但是，反过来，当view内容变动时，数据内容不会跟着变动。



### 6. 事件监听
方法一：方法引用  
定义一个类，类中定义公有方法，方法名、参数、返回值与View类中相应的事件方法名相同。在方法中更新emplyee对象的值，更新后，必须重新绑定数据，否则其它view中引用的数据不会更新。在xml文件中定义该类的variable，并在控件中引用该类对象，引用时工具不会提示。最后，需要在代码中使用binding.setVariable，绑定事件监听处理器与事件监听器。
方法二：监听器绑定  
定义一个类，类中定义方法，方法名、参数、返回值任意指定。  
在xml定义该类的variable，在控件中，在事件参数值中使用"@{() -> variableName.functionName(param1, param2)}"，参数值为lambda表达式。  
最后，在代码中为variable绑定对象。  
这种方式更加灵活。  

## databinding原理
DataBindingUtil.setContentView(activity, layoutId)调用后。首先使用activity.setContentView(layoutId)，指定布局。然后，拿到activity的window，再拿到decorView，接着从decorView中拿到contentView，然后遍历contentView中的所有控件，拿到控件，放到map中。  
setVariable时，先保存variable，然后遍历map中view，逐个更新数值。  

使用apt，未使用反射，而且是静态的，所以性能再高。  
另外，传统的findViewById需要一个一个查找view，而databinding只统一查找一次，性能得到提高。  
数据更新时，只会更新必要的项，不会做不必要的更新操作，不影响性能。  
表达式使用过一次会缓存，不会重复计算。


## 表达式

### 普通用法
算术运行符    `+ - / * %`
字符串拼接    `+`
逻辑运算符    `&& ||`
位运算       `& | ^`
一元运算符    `+ - ! ~`
位移         `>> >>> <<`
比较运算符    `== > < >= <=`
            `instanceof`
分组         `()`
字面意思     `character, String, numeric, null`
Cast
Method calls
Field access
Array access `[]`
三元运算符    `?:`


举例：
```
android:text="@{String.valueOf(index + 1)}"
android:visibility="@{age < 13 ? View.GONE : View.VISIBLE}"
android:transitionName='@{"image_" + id}'
```

### 表达式不能使用
`this`, `super`, `new`, `Explicit generic invocation`在表达式中不能使用，需要在逻辑代码中使用  

### 空合并运算符
如果左边的操作数不为Null，`??`使用左边的操作数，如果左边为null，`??`使用右边的操作数  
```android:text="@{user.displayName ?? user.lastName}"```

### 属性引用
表达式可以使用下边的格式引用类中的属性，包手：属性、getters、ObservableField：
```android:text="@{user.lastName}"```

### 避免空指针异常
生成的data binding代码自动对空指针异常做了处理。如果引用对象为null，使用默认值null，如果引用类型是int，使用默认值0.  

### 集合
常用集合类，如arrays, lists, sparse lists, maps，可以通过[]访问；map中的数值既可以使用`map[key]`也可以使用map.key访问。
```
<data>
    <import type="android.util.SparseArray"/>
    <import type="java.util.Map"/>
    <import type="java.util.List"/>
    <variable name="list" type="List&lt;String&gt;"/>
    <variable name="sparse" type="SparseArray&lt;String&gt;"/>
    <variable name="map" type="Map&lt;String, String&gt;"/>
    <variable name="index" type="int"/>
    <variable name="key" type="String"/>
</data>
…
android:text="@{list[index]}"
…
android:text="@{sparse[index]}"
…
android:text="@{map[key]}"
…
android:text="@{map.key}"
```

### 字符串资源
```
android:text='@{map["firstName"]}'
or
android:text="@{map[`firstName`]}"
```

### 资源
```
android:padding="@{large? @dimen/largePadding : @dimen/smallPadding}"

<string name="nameFormat">firstName is %s, lastName is %s</string>
android:text="@{@string/nameFormat(user.firstName, user.lastName)}"
```

## 特殊符号
```
&&    &amp;&amp;
<     &lt;
>     $gt;
```


## import

### import
在`<data>`中可以import java类。import时如果有名称相同的类，可以为类起别名alias。`java.lang.*`包下的类默认已被import。  
```
<data>
    <import type="android.view.View"/>
    <import type="com.example.real.estate.View" alias="Vista"/>
    <import type="java.util.List"/>
    <import type="com.example.User"/>
</data>
```

### 引用
被引用的类在`<data>`和表达式中的用途有：  

1. 使用类在`<data>`中表达类型  
```
<data>
    <import type="com.example.User"/>
    <import type="java.util.List"/>
    <variable name="user" type="User"/>
    <variable name="userList" type="List&lt;User&gt;"/>
</data>
```

2. 做类型强转，`android:text="@{((User)(user.connection)).lastName}"`  

3. 引用类的静态属性和方法  
```
android:visibility="@{user.isAdult ? View.VISIBLE : View.GONE}"
android:text="@{MyStringUtils.capitalize(user.lastName)}"
```


## include
layout的根布局必须是group，不能是merge  
```
<?xml version="1.0" encoding="utf-8"?>
<layout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:bind="http://schemas.android.com/apk/res-auto">
   <data>
       <variable name="user" type="com.example.User"/>
   </data>
   <LinearLayout
       android:orientation="vertical"
       android:layout_width="match_parent"
       android:layout_height="match_parent">
       <include layout="@layout/name"
           bind:user="@{user}"/>
       <include layout="@layout/contact"
           bind:user="@{user}"/>
   </LinearLayout>
</layout>
```


## 双向绑定

data binding可以使objects, fields, collections observable。  

### Observable fields
当类中成员变量少时，使用Observable fields，有：  
```
ObservableBoolean
ObservableByte
ObservableChar
ObservableShort
ObservableInt
ObservableLong
ObservableFloat
ObservableDouble
ObservableParcelable
```
创建Observable field，需要创建成public final类型。  
``` java
    public final ObservableField<String> firstName = new ObservableField<>();
    public final ObservableField<String> lastName = new ObservableField<>();
    public final ObservableInt age = new ObservableInt();
```
访问field值，使用`get`和`set`方法：  
```
user.firstName.set("Google");
int age = user.age.get();
```

### Observable collections
当使用动态结构组织数据时，Observable collections允许通过一个key访问其中的值。  
如果key是String或引用类型时，使用`ObservableArrayMap`:  
``` java
ObservableArrayMap<String, Object> user = new ObservableArrayMap<>();
user.put("firstName", "Google");
user.put("lastName", "Inc.");
user.put("age", 17);
```
在layout中，map使用string作为key：  
``` xml
<data>
    <import type="android.databinding.ObservableMap"/>
    <variable name="user" type="ObservableMap<String, Object>"/>
</data>
…
<TextView
    android:text="@{user.lastName}"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"/>
<TextView
    android:text="@{String.valueOf(1 + (Integer)user.age)}"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"/>
```
如果key是int时，使用`ObservableArrayList`:  
```
ObservableArrayList<Object> user = new ObservableArrayList<>();
user.add("Google");
user.add("Inc.");
user.add(17);
```
在layout中，list使用index访问数据：  
```
<data>
    <import type="android.databinding.ObservableList"/>
    <import type="com.example.my.app.Fields"/>
    <variable name="user" type="ObservableList<Object>"/>
</data>
…
<TextView
    android:text='@{user[Fields.LAST_NAME]}'
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"/>
<TextView
    android:text='@{String.valueOf(1 + (Integer)user[Fields.AGE])}'
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"/>
```

### BaseObservable
令类继承BaseObservable，就提供了一种机制，可以把某些成员变量注册到Observable，达到双向绑定的目的。  

第一步：定义欲双向绑定的变量，为它提供getter和setter方法  
第二步：在getter方法上添加@Bindable注解，算是注册
第三步：在setter方法中，改变值后，调用notifyPropertyChanged(BR.xxx)，通知数据已改变，UI会自动更新。xxx值与getter方法中get后面的单词有关，与变量名无关  
``` java
private static class User extends BaseObservable {
    private String firstName;
    private String lastName;

    @Bindable
    public String getFirstName() {
        return this.firstName;
    }

    @Bindable
    public String getLastName() {
        return this.lastName;
    }

    public void setFirstName(String firstName) {
        this.firstName = firstName;
        notifyPropertyChanged(BR.firstName);
    }

    public void setLastName(String lastName) {
        this.lastName = lastName;
        notifyPropertyChanged(BR.lastName);
    }
}
```

