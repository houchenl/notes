
# databinding

### 启用
在模块的build.gradle文件中，在android下添加  
'''
    dataBinding {
        enabled true
    }
'''

## 基础使用

### 1. 修改xml文件
在原来的布局外，加一层layout布局。然后把命名域移到最外层layout标签中。  
'''
<layout>
    // 原来的layout
</layout>
'''

### 2. 引用布局文件
把原来的setContentView替换为DataBindingUtils.setContentView。  
方法返回值是ViewDataBinding的子类型，类型名根据layout文件名生成。  
通过binding可以直接访问layout中有id的控件。  

### 3. 绑定数据
xml中绑定数据  
先在layout中定义data，然后在data中定义variable，最后在控件中引用variable中的值，引用的格式为"@{obj.xxx}";  

为控件设置数据，有三种方式：  
1. 通过binding拿到指定id控件，然后直接针对控件进行操作，像传统使用方式。
2. 使用binding.setVariable(brId, employee)，这是通用方式。
3. 使用binding.setEmployee(employee)，这是简易方式，但不能用。

### 4. 更新数据
Model类，字段定义成私有属性，为之提供get/set方法。在xml文件中，使用variable可直接方法属性名，其实是通过getXxx访问到的。  
通过后两种方式绑定数据时，绑定数据后，再修改数据，view上显示的数据会自动随着数据变化而变化，默认具有从数据向view单向传递变动的能力。但是，反过来，当view内容变动时，数据内容不会跟着变动。但是，在事件监听中更新数据项时，view显示内容不会自动变动。

### 5. 双向更新

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
二元    & | ^  
一元    + - ! ~  
移位    >>  >>> <<  
比较    ==  >   <   >=   <=  
instanceof  
grouping()  
文字   character, string, numeric, null  
cast  
方法调用，方法调用可以使用.，也可以使用::，推荐后者  
field访问  
array访问  
三元运算符  
空合并     "@{user.name ?? user.lastName}"  等价于 @{user.name != null ? user.name : user.lastName}  
例子  
"@{@dimen/a + @dimen/b}"  
"@{String.valueOf(index + 1)}"  
'@{"image_" + id}'  

 实践  
 结合viewModel，把简单直观的放在xml中，把复杂的放在viewModel中

## include

bind  
<include layout="@layout/name" bind:user="@{user}"/>  
layout的根布局必须是group，不能是merge

## 特殊符号
&&    &amp;&amp;
<     &lt;
