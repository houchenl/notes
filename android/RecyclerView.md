
# RecyclerView
RecyclerView是google推出的ListView和GridView的替代品，强制实现了ViewHolder，内部实现了view复用，更加高效。

## 1. 基础使用

### 1.1. 添加引用
`compile 'com.android.support:recyclerview-v7:25.3.1'`

### 1.2. 定义布局
``` xml
<?xml version="1.0" encoding="utf-8"?>
<layout>

    <data>
    </data>

    <android.support.v7.widget.RecyclerView xmlns:android="http://schemas.android.com/apk/res/android"
        android:id="@+id/recycler_view"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

</layout>

```

### 1.3. 定义Bean文件和item布局
Bean文件。属性定义为private，并为其提供get/set方法和构造函数，无需其它特殊处理。
``` java
public class Poem {

    private String title;
    private String author;

    public Poem() {
    }

    public Poem(String title, String author) {
        this.title = title;
        this.author = author;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public String getAuthor() {
        return author;
    }

    public void setAuthor(String author) {
        this.author = author;
    }

    @Override
    public String toString() {
        return "Poem{" +
                "title='" + title + '\'' +
                ", author='" + author + '\'' +
                '}';
    }

}
```
item布局。直接通过别名引用bean文件的属性即可。
``` xml
<?xml version="1.0" encoding="utf-8"?>
<layout>

    <data>
        <variable
            name="poem"
            type="com.hc.recycler.demo.model.Poem" />
    </data>

    <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:id="@+id/root_layout"
        android:layout_width="match_parent"
        android:layout_height="48dp"
        android:background="#fff">

        <TextView
            android:id="@+id/tv_title"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_centerVertical="true"
            android:layout_marginLeft="16dp"
            android:text="@{poem.title}"
            android:textColor="#333"
            android:textSize="16sp" />

        <TextView
            android:id="@+id/tv_author"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_alignParentRight="true"
            android:layout_centerVertical="true"
            android:layout_marginRight="16dp"
            android:text="@{poem.author}"
            android:textColor="#999"
            android:textSize="13sp" />

        <View
            android:layout_width="match_parent"
            android:layout_height="0.5dp"
            android:layout_alignParentBottom="true"
            android:background="#bbb" />

    </RelativeLayout>

</layout>
```

### 1.4. 定义ViewHolder
方法一：使用DataBinding。先通过DataBindingUtil.bind，从itemView中获取item布局对应的ViewDataBinding，然后通过为binding对象的别名设置相应对象即可刷新数据。无需获取到item布局中单个控件的id，因为是整体刷新，所以ViewDataBinding使用通过父引用即可。
``` java
class NormalViewHolder extends RecyclerView.ViewHolder {

    private ViewDataBinding mBinding;

    public NormalViewHolder(View itemView) {
        super(itemView);

        mBinding = DataBindingUtil.bind(itemView);
    }

    public ViewDataBinding getBinding() {
        return mBinding;
    }

}
```
方法二：使用传统方式生成ViewHolder。定义各子控件引用，并通过itemView find到各子控件。更新数据时，一个一个，拿到控件的引用然后刷新其数据。
```
class NormalViewHolderV2 extends RecyclerView.ViewHolder {

    private View mRootLayout;
    private TextView mTvTitle, mTvAuthor;

    public NormalViewHolderV2(View itemView) {
        super(itemView);

        mRootLayout = itemView.findViewById(R.id.root_layout);
        mTvTitle = (TextView) itemView.findViewById(R.id.tv_title);
        mTvAuthor = (TextView) itemView.findViewById(R.id.tv_author);
    }

    public View getRootLayout() {
        return mRootLayout;
    }

    public TextView getTvTitle() {
        return mTvTitle;
    }

    public TextView getTvAuthor() {
        return mTvAuthor;
    }

}
```

### 1.5. 定义Adapter
1. 继承RecyclerView.Adapter，并传入CustomViewHolder。然后实现其中3个方法。  
2. NormalViewHolder onCreateViewHolder(ViewGroup parent, int viewType)方法，主要做两件事：一、从layout文件中inflate出itemView；二、根据itemView生成CustomViewHolder对象。这个方法使用databinding和传统方式实现是一样的。
``` java
@Override
public NormalViewHolderV2 onCreateViewHolder(ViewGroup parent, int viewType) {
    View view = LayoutInflater.from(parent.getContext()).inflate(R.layout.item_normal, parent, false);
    return new NormalViewHolderV2(view);
}
```
3. void onBindViewHolder(NormalViewHolder holder, int position)，一个参数表示数据位置，一个参数表示布局对象，该方法用于使用position位置的数据刷新布局。刷新有两种方式。  

方法一：使用DataBinding。使用这种方式时，不需要一个一个控件的指定数据，为整个布局的databinding中设置的数据指定值并刷新即可，databinding会自动刷新数据。
``` java
@Override
public void onBindViewHolder(NormalViewHolder holder, int position) {
    final Poem poem = datas.get(position);

    holder.getBinding().setVariable(BR.poem, poem);
    holder.getBinding().executePendingBindings();
}
```
方法二：传统方式，逐个为控件设置值。
``` java
@Override
public void onBindViewHolder(NormalViewHolderV2 holder, int position) {
    final Poem poem = datas.get(position);

    holder.getTvTitle().setText(poem.getTitle());
    holder.getTvAuthor().setText(poem.getAuthor());
}
```

### 1.6. 配置RecyclerView并加载数据显示
1. 找到控件引用  
2. 设置LayoutManager  
3. 创建并设置adapter  
``` java
mBinding = DataBindingUtil.setContentView(this, R.layout.activity_test_recycler_view);

// 使用线性显示，类似ListView。默认垂直方向显示。
mBinding.recyclerView.setLayoutManager(new LinearLayoutManager(this));

//        mBinding.recyclerView.setAdapter(new NormalAdapter(getTestData()));
mBinding.recyclerView.setAdapter(new NormalAdapterV2(getTestData()));
```

## 参考资料
[RecyclerView Xamarin][]


[RecyclerView Xamarin]: https://developer.xamarin.com/guides/android/user_interface/recyclerview/