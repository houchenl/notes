
# guide

学习android官网guide时学到的新知识

1. 打开开发者选项的方法：在关于手机中，找到**Build number**，连续点击7下。

2. 使用布局编辑器设计布局：
   1. **Select Design Surface**：控制设计显示方式：预览、蓝图、预览+蓝图
   2. **Autoconnect**：控制是否打开自动对齐
   3. **Default Margins**：设置默认margin
   4. **Component Tree**：显示视图层次结构
   5. **Palette**：显示组件列表
   6. 在蓝图中点击组件，会显示调节大小手柄和锚点
   7. **Attributes**：显示组件属性，可以设置点击函数、字符串、边距、宽高

3. **Open editor**中可以自动添加字符串

4. Intent携带参数的key值使用包名作为前缀

5. 创建Activity时，在模块文件夹上右击，选择**New > Activity > Empty Activity**，使用模板创建Activity

6. 为非主入口Activity添加向上导航。
``` xml
<activity android:name=".DisplayMessageActivity"
          android:parentActivityName=".MainActivity" >
    <!-- The meta-data tag is required if you support API level 15 and lower -->
    <meta-data
        android:name="android.support.PARENT_ACTIVITY"
        android:value=".MainActivity" />
</activity>
```

7. 非首次运行时，使用**Apply Changes**，再次运行应用。

