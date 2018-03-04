<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [【Android 控件 RecyclerView】](#android-%E6%8E%A7%E4%BB%B6-recyclerview)
  - [概述](#%E6%A6%82%E8%BF%B0)
    - [RecyclerView是什么](#recyclerview%E6%98%AF%E4%BB%80%E4%B9%88)
    - [RecyclerView的优点](#recyclerview%E7%9A%84%E4%BC%98%E7%82%B9)
    - [基本使用](#%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8)
  - [基本使用](#%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8-1)
    - [引用](#%E5%BC%95%E7%94%A8)
    - [布局](#%E5%B8%83%E5%B1%80)
    - [创建适配器](#%E5%88%9B%E5%BB%BA%E9%80%82%E9%85%8D%E5%99%A8)
    - [设置RecyclerView](#%E8%AE%BE%E7%BD%AErecyclerview)
  - [四大组成](#%E5%9B%9B%E5%A4%A7%E7%BB%84%E6%88%90)
    - [Layout Manager布局管理器](#layout-manager%E5%B8%83%E5%B1%80%E7%AE%A1%E7%90%86%E5%99%A8)
      - [① LinearLayoutManager线性](#%E2%91%A0-linearlayoutmanager%E7%BA%BF%E6%80%A7)
      - [② GridLayoutManager](#%E2%91%A1-gridlayoutmanager)
      - [③ StaggeredGridLayoutManager瀑布流](#%E2%91%A2-staggeredgridlayoutmanager%E7%80%91%E5%B8%83%E6%B5%81)
      - [LayoutManager 常见 API](#layoutmanager-%E5%B8%B8%E8%A7%81-api)
      - [LinearLayoutManager源码分析](#linearlayoutmanager%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90)
    - [Adapter适配器](#adapter%E9%80%82%E9%85%8D%E5%99%A8)
      - [万能适配器](#%E4%B8%87%E8%83%BD%E9%80%82%E9%85%8D%E5%99%A8)
    - [Item Decoration间隔样式](#item-decoration%E9%97%B4%E9%9A%94%E6%A0%B7%E5%BC%8F)
      - [自定义的间隔样式的实现步骤](#%E8%87%AA%E5%AE%9A%E4%B9%89%E7%9A%84%E9%97%B4%E9%9A%94%E6%A0%B7%E5%BC%8F%E7%9A%84%E5%AE%9E%E7%8E%B0%E6%AD%A5%E9%AA%A4)
        - [① 获取listDivider](#%E2%91%A0-%E8%8E%B7%E5%8F%96listdivider)
        - [② getItemOffsets](#%E2%91%A1-getitemoffsets)
        - [③ onDraw](#%E2%91%A2-ondraw)
    - [Item Animator动画](#item-animator%E5%8A%A8%E7%94%BB)
      - [DefaultItemAnimator源码分析](#defaultitemanimator%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90)
      - [开源动画recyclerview-animators](#%E5%BC%80%E6%BA%90%E5%8A%A8%E7%94%BBrecyclerview-animators)
      - [局部刷新闪屏问题解决](#%E5%B1%80%E9%83%A8%E5%88%B7%E6%96%B0%E9%97%AA%E5%B1%8F%E9%97%AE%E9%A2%98%E8%A7%A3%E5%86%B3)
  - [网格样式](#%E7%BD%91%E6%A0%BC%E6%A0%B7%E5%BC%8F)
  - [瀑布流样式](#%E7%80%91%E5%B8%83%E6%B5%81%E6%A0%B7%E5%BC%8F)
    - [瀑布流间隔样式](#%E7%80%91%E5%B8%83%E6%B5%81%E9%97%B4%E9%9A%94%E6%A0%B7%E5%BC%8F)
  - [拓展RecyclerView](#%E6%8B%93%E5%B1%95recyclerview)
    - [添加点击事件](#%E6%B7%BB%E5%8A%A0%E7%82%B9%E5%87%BB%E4%BA%8B%E4%BB%B6)
    - [添加HeaderView和FooterView](#%E6%B7%BB%E5%8A%A0headerview%E5%92%8Cfooterview)
    - [添加setEmptyView](#%E6%B7%BB%E5%8A%A0setemptyview)
    - [拖拽、侧滑删除](#%E6%8B%96%E6%8B%BD%E4%BE%A7%E6%BB%91%E5%88%A0%E9%99%A4)
      - [① 创建ItemTouchHelper.Callback类](#%E2%91%A0-%E5%88%9B%E5%BB%BAitemtouchhelpercallback%E7%B1%BB)
      - [② 设置ItemTouchHelper给RecyclerView](#%E2%91%A1-%E8%AE%BE%E7%BD%AEitemtouchhelper%E7%BB%99recyclerview)
      - [触摸拖拽](#%E8%A7%A6%E6%91%B8%E6%8B%96%E6%8B%BD)
    - [嵌套滑动机制](#%E5%B5%8C%E5%A5%97%E6%BB%91%E5%8A%A8%E6%9C%BA%E5%88%B6)
  - [总结](#%E6%80%BB%E7%BB%93)
- [RecyclerView vs ListView](#recyclerview-vs-listview)
  - [局部刷新](#%E5%B1%80%E9%83%A8%E5%88%B7%E6%96%B0)
    - [ListView实现局部刷新](#listview%E5%AE%9E%E7%8E%B0%E5%B1%80%E9%83%A8%E5%88%B7%E6%96%B0)
    - [RecyclerView实现局部刷新](#recyclerview%E5%AE%9E%E7%8E%B0%E5%B1%80%E9%83%A8%E5%88%B7%E6%96%B0)
  - [缓存机制](#%E7%BC%93%E5%AD%98%E6%9C%BA%E5%88%B6)
    - [缓存机制对比](#%E7%BC%93%E5%AD%98%E6%9C%BA%E5%88%B6%E5%AF%B9%E6%AF%94)
      - [1. 层级不同：](#1-%E5%B1%82%E7%BA%A7%E4%B8%8D%E5%90%8C)
      - [2. 缓存不同：](#2-%E7%BC%93%E5%AD%98%E4%B8%8D%E5%90%8C)
    - [局部刷新](#%E5%B1%80%E9%83%A8%E5%88%B7%E6%96%B0-1)
    - [回收机制源码分析](#%E5%9B%9E%E6%94%B6%E6%9C%BA%E5%88%B6%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90)
      - [ListView回收机制](#listview%E5%9B%9E%E6%94%B6%E6%9C%BA%E5%88%B6)
      - [RecyclerView回收机制](#recyclerview%E5%9B%9E%E6%94%B6%E6%9C%BA%E5%88%B6)
  - [结论](#%E7%BB%93%E8%AE%BA)
  - [扩展阅读](#%E6%89%A9%E5%B1%95%E9%98%85%E8%AF%BB)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->



# 【Android 控件 RecyclerView】

## 概述

### RecyclerView是什么
从Android 5.0开始，谷歌公司推出了一个用于大量数据展示的新控件RecylerView，可以用来代替传统的ListView，更加强大和灵活。RecyclerView的官方定义如下：
> A flexible view for providing a limited window into a large data set.

从定义可以看出，flexible（可扩展性）是RecyclerView的特点。

RecyclerView是**support-v7包**中的**新组件**，是一个强大的滑动组件，与经典的ListView相比，同样拥有item回收复用的功能，这一点从它的名字Recyclerview即回收view也可以看出。



### RecyclerView的优点
RecyclerView并不会完全替代ListView（这点从ListView没有被标记为@Deprecated可以看出），两者的使用场景不一样。但是RecyclerView的出现会让很多开源项目被废弃，例如横向滚动的ListView, 横向滚动的GridView, 瀑布流控件，因为RecyclerView能够实现所有这些功能。

比如：有一个需求是**屏幕竖着**的时候的显示形式是**ListView**，屏幕**横着**的时候的显示形式是2列的**GridView**，此时如果用**RecyclerView**，则通过设置LayoutManager**一行代码实现替换**。

RecylerView相对于ListView的优点罗列如下： 
- RecyclerView**封装了viewholder**的回收复用，也就是说RecyclerView**标准化了ViewHolder**，**编写Adapter面向**的是**ViewHolder**而不再是View了，复用的逻辑被封装了，写起来更加简单。 
  直接省去了listview中convertView.setTag(holder)和convertView.getTag()这些繁琐的步骤。
- 提供了一种**插拔式的体验**，**高度的解耦**，异常的灵活，针对一个Item的显示RecyclerView专门抽取出了**相应的类**，来**控制Item的显示**，使其的扩展性非常强。
- 设置**布局管理器**以控制**Item**的**布局方式**，**横向**、**竖向**以及**瀑布流**方式
  例如：你想控制横向或者纵向滑动列表效果可以通过**LinearLayoutManager**这个类来进行控制(与GridView效果对应的是**GridLayoutManager**,与**瀑布流**对应的还**StaggeredGridLayoutManager**等)。也就是说RecyclerView不再拘泥于ListView的线性展示方式，它也可以实现GridView的效果等多种效果。
- 可设置**Item的间隔样式**（可绘制）  
  通过**继承RecyclerView的ItemDecoration**这个类，然后针对自己的业务需求去书写代码。 
- 可以控制**Item增删的动画**，可以通过**ItemAnimator**这个类进行控制，当然针对增删的动画，RecyclerView有其自己默认的实现。

但是关于Item的点击和长按事件，需要用户自己去实现。

### 基本使用

```java
recyclerView = (RecyclerView) findViewById(R.id.recyclerView);  
LinearLayoutManager layoutManager = new LinearLayoutManager(this );  
//设置布局管理器  
recyclerView.setLayoutManager(layoutManager);  
//设置为垂直布局，这也是默认的  
layoutManager.setOrientation(OrientationHelper. VERTICAL);  
//设置Adapter  
recyclerView.setAdapter(recycleAdapter);  
 //设置分隔线  
recyclerView.addItemDecoration( new DividerGridItemDecoration(this ));  
//设置增加或删除条目的动画  
recyclerView.setItemAnimator( new DefaultItemAnimator());  
```

在使用RecyclerView时候，必须指定一个适配器Adapter和一个布局管理器LayoutManager。适配器继承**`RecyclerView.Adapter`**类，具体实现类似ListView的适配器，取决于数据信息以及展示的UI。布局管理器用于确定RecyclerView中Item的展示方式以及决定何时复用已经不可见的Item，避免重复创建以及执行高成本的`findViewById()`方法。

可以看见RecyclerView相比ListView会多出许多操作，这也是RecyclerView灵活的地方，它将许多动能暴露出来，用户可以选择性的自定义属性以满足需求。



## 基本使用
### 引用
在build.gradle文件中**引入该类**。

```java
    compile 'com.android.support:recyclerview-v7:23.4.0'
```

### 布局
Activity布局文件activity_rv.xml
...

Item的布局文件item_1.xml
...

### 创建适配器
标准实现步骤如下：
① **创建Adapter**：创建一个**继承`RecyclerView.Adapter<VH>`**的Adapter类（VH是ViewHolder的类名）
② **创建ViewHolder**：在Adapter中创建一个**继承`RecyclerView.ViewHolder`**的静态内部类，记为VH。ViewHolder的实现和ListView的ViewHolder实现几乎一样。
③ 在**Adapter中实现3个方法**： 
- **onCreateViewHolder()** 
  这个方法主要**生成**为**每个Item inflater出一个View**，但是该方法**返回**的是一个**ViewHolder**。该方法把View直接封装在ViewHolder中，然后我们面向的是ViewHolder这个实例，当然这个ViewHolder需要我们自己去编写。

需要**注意**的是在`onCreateViewHolder()`中，映射**Layout必须为**
``` java
View v = LayoutInflater.from(parent.getContext()).inflate(R.layout.item_1, parent, false);
```
而不能是：
``` java
View v = LayoutInflater.from(parent.getContext()).inflate(R.layout.item_1, null);
```
- **onBindViewHolder()** 
  这个方法主要用于**适配渲染数据到View**中。方法**提供**给你了一**viewHolder**而不是原来的convertView。
- **getItemCount()** 
  这个方法就**类似**于BaseAdapter的**getCount**方法了，即总共有多少个条目。

可以看出，RecyclerView将ListView中`getView()`的功能拆分成了`onCreateViewHolder()`和`onBindViewHolder()`。

基本的Adapter实现如下：
``` java
// ① 创建Adapter
public class NormalAdapter extends RecyclerView.Adapter<NormalAdapter.VH>{
    //② 创建ViewHolder
    public static class VH extends RecyclerView.ViewHolder{
        public final TextView title;
        public VH(View v) {
            super(v);
            title = (TextView) v.findViewById(R.id.title);
        }
    }
    
    private List<String> mDatas;
    public NormalAdapter(List<String> data) {
        this.mDatas = data;
    }

    //③ 在Adapter中实现3个方法
    @Override
    public VH onCreateViewHolder(ViewGroup parent, int viewType) {
        //LayoutInflater.from指定写法
        View v = LayoutInflater.from(parent.getContext()).inflate(R.layout.item_1, parent, false);
        return new VH(v);
    }
  
    @Override
    public void onBindViewHolder(VH holder, int position) {
        holder.title.setText(mDatas.get(position));
        holder.itemView.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                //item 点击事件
            }
        });
    }

    @Override
    public int getItemCount() {
        return mDatas.size();
    }
}
```


### 设置RecyclerView
创建完Adapter，接着对RecyclerView进行设置，一般来说，需要为RecyclerView进行**四大设置**，也就是后文说的四大组成：
- Layout Manager(必选)
- Adapter(必选)
- Item Decoration(可选，默认为空)
- Item Animator(可选，默认为DefaultItemAnimator)


如果要实现ListView的效果，只需要设置Adapter和Layout Manager，如下：
``` java
List<String> data = initData();
RecyclerView rv = (RecyclerView) findViewById(R.id.rv);
rv.setLayoutManager(new LinearLayoutManager(this));
rv.setAdapter(new NormalAdapter(data));
```



## 四大组成

RecyclerView的四大组成是：
*   Layout Manager：Item的布局。
*   Adapter：为Item提供数据。
*   Item Decoration：Item之间的Divider。
*   Item Animator：添加、删除Item动画。

### Layout Manager布局管理器

在最开始就提到，RecyclerView 能够支持各种各样的布局效果，这是 ListView 所不具有的功能，那么这个功能如何实现的呢？其核心关键在于 [RecyclerView.LayoutManager](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.LayoutManager.html) 类中。从前面的基础使用可以看到，RecyclerView 在使用过程中要比 ListView 多一个 setLayoutManager 步骤，这个 LayoutManager 就是用于控制我们 RecyclerView 最终的展示效果的。

LayoutManager负责RecyclerView的布局，其中包含了Item View的获取与回收。

RecyclerView提供了**三种布局管理器**：
- **LinearLayoutManager **以垂直**或者**水平列表**方式展示Item
- **GridLayoutManager** 以**网格**方式展示Item
- **StaggeredGridLayoutManager** 以**瀑布流**方式展示Item



#### ① LinearLayoutManager线性

- LinearLayoutManager(Context context)
  - 该构造函数**默认**是**竖直**方向
- LinearLayoutManager(Context context, int orientation, boolean reverseLayout)
  - orientation方向，水平（OrientationHelper.HORIZONTAL）或者竖直（OrientationHelper.VERTICAL）
  - reverseLayout是否逆向，true：布局逆向展示，false：布局正向显示



#### ② GridLayoutManager

- GridLayoutManager(Context context, int spanCount)
  - spanCount，每列或者每行的item个数，设置为1，就是列表样式
  - 该构造函数**默认**是**竖直**方向的网格样式
- GridLayoutManager(Context context, int spanCount, int orientation,boolean reverseLayout)
  - spanCount每列或者每行的item个数，设置为1，就是列表样式
  - orientation方向，水平（OrientationHelper.HORIZONTAL）或者竖直（OrientationHelper.VERTICAL）
  - reverseLayout是否逆向，true：布局逆向展示，false：布局正向显示



#### ③ StaggeredGridLayoutManager瀑布流

StaggeredGridLayoutManager(int spanCount, int orientation)

- spanCount每列或者每行的item个数
- orientation方向，水平（OrientationHelper.HORIZONTAL）或者竖直（OrientationHelper.VERTICAL）



如果你想用 RecyclerView 来实现自己**自定义效果**，则应该去**继承实现自己的 LayoutManager**，并重写相应的方法，而不应该想着去改写 RecyclerView。

#### LayoutManager 常见 API

关于 LayoutManager 的使用有下面一些常见的 API（有些在 LayoutManager 实现的子类中）

```java
    canScrollHorizontally();//能否横向滚动
    canScrollVertically();//能否纵向滚动
    scrollToPosition(int position);//滚动到指定位置

    setOrientation(int orientation);//设置滚动的方向
    getOrientation();//获取滚动方向

    findViewByPosition(int position);//获取指定位置的Item View
    findFirstCompletelyVisibleItemPosition();//获取第一个完全可见的Item位置
    findFirstVisibleItemPosition();//获取第一个可见Item的位置
    findLastCompletelyVisibleItemPosition();//获取最后一个完全可见的Item位置
    findLastVisibleItemPosition();//获取最后一个可见Item的位置
```

上面仅仅是列出一些常用的 API 而已，更多的 API 可以查看官方文档，通常你想用 RecyclerView 实现某种效果，例如指定滚动到某个 Item 位置，但是你在 RecyclerView 中又找不到可以调用的 API 时，就可以跑到 LayoutManager 的文档去看看，基本都在那里。
另外还有一点关于瀑布流布局效果 StaggeredGridLayoutManager 想说的，看到网上有些文章写的示例代码，在设置了 StaggeredGridLayoutManager 后仍要去 Adapter 中动态设置 View 的高度，才能实现瀑布流，这种做法是完全错误的，之所以 StaggeredGridLayoutManager 的瀑布流效果出不来，基本是 item 布局的 xml 问题以及数据问题导致。如果要在 Adapter 中设置 View 的高度，则完全违背了 LayoutManager 的设计理念了。


#### LinearLayoutManager源码分析


这里我们简单分析LinearLayoutManager的实现。

对于LinearLayoutManager来说，比较重要的几个方法有：

*   `onLayoutChildren()`: 对RecyclerView进行布局的入口方法。
*   `fill()`: 负责填充RecyclerView。
*   `scrollVerticallyBy()`:根据手指的移动滑动一定距离，并调用`fill()`填充。
*   `canScrollVertically()`或`canScrollHorizontally()`: 判断是否支持纵向滑动或横向滑动。

`onLayoutChildren()`的核心实现如下：

``` java
public void onLayoutChildren(RecyclerView.Recycler recycler, RecyclerView.State state) {
    detachAndScrapAttachedViews(recycler); //将原来所有的Item View全部放到Recycler的Scrap Heap或Recycle Pool
    fill(recycler, mLayoutState, state, false); //填充现在所有的Item View
}
```

RecyclerView的回收机制有个重要的概念，即将回收站分为Scrap Heap和Recycle Pool，其中Scrap Heap的元素可以被直接复用，而不需要调用`onBindViewHolder()`。`detachAndScrapAttachedViews()`会根据情况，将原来的Item View放入Scrap Heap或Recycle Pool，从而在复用时提升效率。

`fill()`是对剩余空间不断地调用`layoutChunk()`，直到填充完为止。`layoutChunk()`的核心实现如下：

``` java
public void layoutChunk() {
    View view = layoutState.next(recycler); //调用了getViewForPosition()
    addView(view);  //加入View
    measureChildWithMargins(view, 0, 0); //计算View的大小
    layoutDecoratedWithMargins(view, left, top, right, bottom); //布局View
}
```

其中`next()`调用了`getViewForPosition(currentPosition)`，该方法是从RecyclerView的回收机制实现类Recycler中获取合适的View，在后文的回收机制中会介绍该方法的具体实现。

如果要自定义LayoutManager，可以参考：

*   [创建一个 RecyclerView LayoutManager – Part 1](https://github.com/hehonghui/android-tech-frontier/blob/master/issue-9/%E5%88%9B%E5%BB%BA-RecyclerView-LayoutManager-Part-1.md)
*   [创建一个 RecyclerView LayoutManager – Part 2](https://github.com/hehonghui/android-tech-frontier/blob/master/issue-13/%E5%88%9B%E5%BB%BA-RecyclerView-LayoutManager-Part-2.md)
*   [创建一个 RecyclerView LayoutManager – Part 3](https://github.com/hehonghui/android-tech-frontier/blob/master/issue-13/%E5%88%9B%E5%BB%BA-RecyclerView-LayoutManager-Part-3.md)

### Adapter适配器

Adapter的使用方式前面已经介绍了，功能就是为RecyclerView提供数据，这里主要介绍万能适配器的实现。其实万能适配器的概念在ListView就已经存在了，即[base-adapter-helper](https://github.com/JoanZapata/base-adapter-helper)。

这里我们只针对RecyclerView，聊聊万能适配器出现的原因。为了创建一个RecyclerView的Adapter，每次我们都需要去做重复劳动，包括重写`onCreateViewHolder()`,`getItemCount()`、创建ViewHolder，并且实现过程大同小异，因此万能适配器出现了。
#### 万能适配器
这里讲解下万能适配器的实现思路。

我们通过`public abstract class QuickAdapter<T> extends RecyclerView.Adapter<QuickAdapter.VH>`定义万能适配器QuickAdapter类，T是列表数据中每个元素的类型，QuickAdapter.VH是QuickAdapter的ViewHolder实现类，称为万能ViewHolder。

首先介绍QuickAdapter.VH的实现：

``` java
static class VH extends RecyclerView.ViewHolder{
    private SparseArray<View> mViews;
    private View mConvertView;

    private VH(View v){
        super(v);
        mConvertView = v;
        mViews = new SparseArray<>();
    }

    public static VH get(ViewGroup parent, int layoutId){
        View convertView = LayoutInflater.from(parent.getContext()).inflate(layoutId, parent, false);
        return new VH(convertView);
    }

    public <T extends View> T getView(int id){
        View v = mViews.get(id);
        if(v == null){
            v = mConvertView.findViewById(id);
            mViews.put(id, v);
        }
        return (T)v;
    }

    public void setText(int id, String value){
        TextView view = getView(id);
        view.setText(value);
    }
}
```

其中的**关键点在于通过`SparseArray<View> mViews`存储item view的控件**，`getView(int id)`的功能就是通过id获得对应的View（首先在mViews中查询是否存在，如果没有，那么`findViewById()`并放入mViews中，避免下次再执行`findViewById()`）。

**QuickAdapter的实现**如下：
``` java
public abstract class QuickAdapter<T> extends RecyclerView.Adapter<QuickAdapter.VH>{
    private List<T> mDatas;
    public QuickAdapter(List<T> datas){
        this.mDatas = datas;
    }

    @Override
    public VH onCreateViewHolder(ViewGroup parent, int viewType) {
        return VH.get(parent,getLayoutId(viewType));
    }
  //根据viewType返回布局ID
    public abstract int getLayoutId(int viewType);
  
    @Override
    public void onBindViewHolder(VH holder, int position) {
        convert(holder, mDatas.get(position), position);
    }
  //具体的bind操作
    public abstract void convert(VH holder, T data, int position);
  
    @Override
    public int getItemCount() {
        return mDatas.size();
    }

    static class VH extends RecyclerView.ViewHolder{
        private SparseArray<View> mViews;
        private View mConvertView;
        private VH(View v){
            super(v);
            mConvertView = v;
            mViews = new SparseArray<>();
        }

        public static VH get(ViewGroup parent, int layoutId){
            View convertView = LayoutInflater.from(parent.getContext()).inflate(layoutId, parent, false);
            return new VH(convertView);
        }

        public <T extends View> T getView(int id){
            View v = mViews.get(id);
            if(v == null){
                v = mConvertView.findViewById(id);
                mViews.put(id, v);
            }
            return (T)v;
        }

        public void setText(int id, String value){
            TextView view = getView(id);
            view.setText(value);
        }
    }
}
```

其中：
*   `getLayoutId(int viewType)`是根据viewType返回布局ID。
*   `convert()`做具体的bind操作。

就这样，万能适配器实现完成了。


通过万能适配器能通过以下方式快捷地创建一个Adapter：
``` java
mAdapter = new QuickAdapter<String>(data) {
    @Override
    public int getLayoutId(int viewType) {
        return R.layout.item;
    }

    @Override
    public void convert(VH holder, String data, int position) {
        holder.setText(R.id.text, data);
        //holder.itemView.setOnClickListener(); 此处还可以添加点击事件
    }
};
```

是不是很方便。当然复杂情况也可以轻松解决。
``` java
mAdapter = new QuickAdapter<Model>(data) {
    @Override
    public int getLayoutId(int viewType) {
        switch(viewType){
            case TYPE_1:
                return R.layout.item_1;
            case TYPE_2:
                return R.layout.item_2;
        }
    }

    @Override
    public int getItemViewType(int position) {
        if(position % 2 == 0){
            return TYPE_1;
        } else{
            return TYPE_2;
        }
    }

    @Override
    public void convert(VH holder, Model data, int position) {
        int type = getItemViewType(position);
        switch(type){
            case TYPE_1:
                holder.setText(R.id.text, data.text);
                break;
            case TYPE_2:
                holder.setImage(R.id.image, data.image);
                break;
        }
    }
};
```


### Item Decoration间隔样式

RecyclerView通过**`addItemDecoration()`**方法添加item之间的分割线。Android并没有提供实现好的Divider，因此**任何分割线样式都需要自己实现**。

自定义间隔样式需要继承**`RecyclerView.ItemDecoration`**类，该类是个**抽象类**，官方目前并没有提供默认的实现类，主要有三个方法。
- **onDraw**(Canvas c, RecyclerView parent, State state)，在**Item绘制之前**被调用，该方法主要用于绘制间隔样式。
- **onDrawOver**(Canvas c, RecyclerView parent, State state)，在**Item绘制之前**被调用，该方法主要用于绘制间隔样式。
- **getItemOffsets**(Rect outRect, View view, RecyclerView parent, State state)，设置**item的偏移量**，**偏移的部分用于填充间隔样式**，即设置分割线的宽、高；在RecyclerView的`onMesure()`中会调用该方法。

`onDraw()`和`onDrawOver()`这两个方法都是用于绘制间隔样式，我们只需要复写其中一个方法即可。

Google在sample中给了一个参考的实现类：[DividerItemDecoration](https://android.googlesource.com/platform/development/%2B/master/samples/Support7Demos/src/com/example/android/supportv7/widget/decorator/DividerItemDecoration.java)，这里我们通过分析这个例子来看如何自定义Item Decoration。

#### 自定义的间隔样式的实现步骤
- ① 通过读取系统主题中的 Android.R.attr.**listDivider**作为Item间的**分割线**，并且支持横向和纵向。
  该分割线是系统默认的，你可以在theme.xml中找到该属性（android:listDivider）的使用情况。
  如果要设置，则需要在value/styles.xml中设置：
``` xml
<style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
    <item name="android:listDivider">@drawable/item_divider</item>
</style>
```

- ② 获取到**listDivider**以后，该属性的值是个**Drawable**，在**getItemOffsets**中，**outRect**去设置了绘制的范围。
- ③ **onDraw**中实现了真正的绘制。

##### ① 获取listDivider
首先看构造函数，构造函数中获得系统属性`android:listDivider`，该属性是一个Drawable对象。
``` java
public class DividerItemDecoration extends RecyclerView.ItemDecoration {
    private static final int[] ATTRS = new int[]{android.R.attr.listDivider};
    private Drawable mDivider;
    public DividerItemDecoration(Context context, int orientation) {
        final TypedArray a = context.obtainStyledAttributes(ATTRS);
        mDivider = a.getDrawable(0);
        a.recycle();
        setOrientation(orientation);
    }
}
```

##### ② getItemOffsets
接着来看`getItemOffsets()`的实现：
``` java
public void getItemOffsets(Rect outRect, int position, RecyclerView parent) {
    if (mOrientation == VERTICAL_LIST) {
        outRect.set(0, 0, 0, mDivider.getIntrinsicHeight());
    } else {
        outRect.set(0, 0, mDivider.getIntrinsicWidth(), 0);
    }
}
```

这里只看`mOrientation == VERTICAL_LIST`的情况，outRect是当前item四周的间距，类似margin属性，现在设置了该item下间距为`mDivider.getIntrinsicHeight()`。

那么`getItemOffsets()`是怎么被调用的呢？

RecyclerView继承了ViewGroup，并重写了`measureChild()`，该方法在`onMeasure()`中被调用，用来计算每个child的大小，计算每个child大小的时候就需要加上`getItemOffsets()`设置的外间距：

``` java
public void measureChild(View child, int widthUsed, int heightUsed){
    final Rect insets = mRecyclerView.getItemDecorInsetsForChild(child);//调用getItemOffsets()获得Rect对象
    widthUsed += insets.left + insets.right;
    heightUsed += insets.top + insets.bottom;
    //...
}
```

##### ③ onDraw
这里我们只考虑`mOrientation == VERTICAL_LIST`的情况，DividerItemDecoration的`onDraw()`实际上调用了`drawVertical()`：

``` java
    @Override
    public void onDraw(Canvas c, RecyclerView parent, RecyclerView.State state) {
        if (mOrientation == VERTICAL_LIST) {
            drawVertical(c, parent);
        } else {
            drawHorizontal(c, parent);
        }
    }
    public void drawVertical(Canvas c, RecyclerView parent) {
        final int left = parent.getPaddingLeft();
        final int right = parent.getWidth() - parent.getPaddingRight();
        final int childCount = parent.getChildCount();
        // 画每个item的分割线
        for (int i = 0; i < childCount; i++) {
            final View child = parent.getChildAt(i);
            final RecyclerView.LayoutParams params = (RecyclerView.LayoutParams) child
                    .getLayoutParams();
            final int top = child.getBottom() + params.bottomMargin +
                    Math.round(ViewCompat.getTranslationY(child));
            //mDivider.getIntrinsicHeight() 单位dp
            final int bottom = top + mDivider.getIntrinsicHeight();
            mDivider.setBounds(left, top, right, bottom);/*规定好左上角和右下角*/
            mDivider.draw(c);
        }
    }
```

那么`onDraw()`是怎么被调用的呢？还有ItemDecoration还有一个方法`onDrawOver()`，该方法也可以被重写，那么`onDraw()`和`onDrawOver()`之间有什么关系呢？

我们来看下面的代码：

``` java
class RecyclerView extends ViewGroup{
    public void draw(Canvas c) {
        super.draw(c); //调用View的draw()，该方法会先调用onDraw()，再调用dispatchDraw()绘制children

        final int count = mItemDecorations.size();
        for (int i = 0; i < count; i++) {
            mItemDecorations.get(i).onDrawOver(c, this, mState);
        }
        ...
    }
    public void onDraw(Canvas c) {
        super.onDraw(c);
        final int count = mItemDecorations.size();
        for (int i = 0; i < count; i++) {
            mItemDecorations.get(i).onDraw(c, this, mState);
        }
    }
}
```

根据[View的绘制流程](http://a.codekk.com/detail/Android/lightSky/%E5%85%AC%E5%85%B1%E6%8A%80%E6%9C%AF%E7%82%B9%E4%B9%8B%20View%20%E7%BB%98%E5%88%B6%E6%B5%81%E7%A8%8B)，首先调用RecyclerView重写的`draw()`方法，随后`super.draw()`即调用View的`draw()`，该方法会先调用`onDraw()`（这个方法在RecyclerView重写了），再调用`dispatchDraw()`绘制children。因此：ItemDecoration的`onDraw()`在绘制Item之前调用，ItemDecoration的`onDrawOver()`在绘制Item之后调用。

当然，如果只需要实现Item之间相隔一定距离，那么只需要为Item的布局设置margin即可，没必要自己实现ItemDecoration这么麻烦。







### Item Animator动画

RecyclerView能够通过**`mRecyclerView.setItemAnimator(ItemAnimator animator)`设置添加、删除、移动、改变的动画效果**。

RecyclerView提供了**默认**的ItemAnimator实现类：**DefaultItemAnimator**。如果没有特殊的需求，默认使用这个动画即可。

```java
// 设置Item添加和移除的动画
mRecyclerView.setItemAnimator(new DefaultItemAnimator());
```

下面就添加一下删除和添加Item的动作。在**Adapter里面添加方法**。

```java
public void addNewItem() {
    if(mData == null) {
        mData = new ArrayList<>();
    }
    mData.add(0, "new Item");
  ////更新数据集不是用adapter.notifyDataSetChanged()而是notifyItemInserted(position)与notifyItemRemoved(position) 否则没有动画效果。 
    notifyItemInserted(0);
}

public void deleteItem() {
    if(mData == null || mData.isEmpty()) {
        return;
    }
    mData.remove(0);
    notifyItemRemoved(0);
}
```

添加事件的处理。

```java
public void onClick(View v) {
    int id = v.getId();
    if(id == R.id.rv_add_item_btn) {
        mAdapter.addNewItem();
        // 由于Adapter内部是直接在首个Item位置做增加操作，增加完毕后列表移动到首个Item位置
        mLayoutManager.scrollToPosition(0);
    } else if(id == R.id.rv_del_item_btn){
        mAdapter.deleteItem();
        // 由于Adapter内部是直接在首个Item位置做删除操作，删除完毕后列表移动到首个Item位置
        mLayoutManager.scrollToPosition(0);
    }
}

```

准备工作完毕后，来看一下运行的效果。

[![](http://upload-images.jianshu.io/upload_images/9028834-9487ed619db48b22.gif?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-9487ed619db48b22.gif?imageMogr2/auto-orient/strip)


#### DefaultItemAnimator源码分析
这里我们通过分析DefaultItemAnimator的源码来介绍如何自定义Item Animator。

DefaultItemAnimator继承自SimpleItemAnimator，SimpleItemAnimator继承自ItemAnimator。

首先我们介绍**ItemAnimator类**的几个重要**方法**：
*   **animateAppearance**(): 当ViewHolder出现在屏幕上时被调用（可能是**add或move**）。
*   **animateDisappearance**(): 当ViewHolder消失在屏幕上时被调用（可能是**remove或move**）。
*   **animatePersistence**(): 在没调用`notifyItemChanged()`和`notifyDataSetChanged()`的情况下布局发生改变时被调用。
*   **animateChange**(): 在显式调用`notifyItemChanged()`或`notifyDataSetChanged()`时被调用。
*   **runPendingAnimations**(): RecyclerView动画的执行方式并不是立即执行，而是每帧执行一次，比如两帧之间添加了多个Item，则会将这些将要执行的动画Pending住，保存在成员变量中，等到下一帧一起执行。该方法执行的前提是前面`animateXxx()`返回true。
*   **isRunning**(): 是否有动画要执行或正在执行。
*   **dispatchAnimationsFinished**(): 当全部动画执行完毕时被调用。

上面的方法比较难懂，不过没关系，因为Android提供了**SimpleItemAnimator**类（继承自ItemAnimator），该类提供了一系列更易懂的API，在自定义Item Animator时只需要继承SimpleItemAnimator即可：
*   **animateAdd**(ViewHolder holder): 当Item添加时被调用。
*   **animateMove**(ViewHolder holder, int fromX, int fromY, int toX, int toY): 当Item移动时被调用。
*   **animateRemove**(ViewHolder holder): 当Item删除时被调用。
*   **animateChange**(ViewHolder oldHolder, ViewHolder newHolder, int fromLeft, int fromTop, int toLeft, int toTop): 当显式调用`notifyItemChanged()`或`notifyDataSetChanged()`时被调用。

对于以上四个方法，**注意**两点：
*   当Xxx动画开始执行前（在`runPendingAnimations()`中）需要调用`dispatchXxxStarting(holder)`，执行完后需要调用`dispatchXxxFinished(holder)`。
*   这些方法的内部实际上并不是书写执行动画的代码，而是将需要执行动画的Item全部存入成员变量中，并且返回值为true，然后在`runPendingAnimations()`中一并执行。

**DefaultItemAnimator**类是RecyclerView提供的默认动画类。我们通过阅读该类源码学习如何自定义Item Animator。我们先看DefaultItemAnimator的**成员变量**：

``` java
private ArrayList<ViewHolder> mPendingAdditions = new ArrayList<>();//存放下一帧要执行的一系列add动画
ArrayList<ArrayList<ViewHolder>> mAdditionsList = new ArrayList<>();//存放正在执行的一批add动画
ArrayList<ViewHolder> mAddAnimations = new ArrayList<>(); //存放当前正在执行的add动画

private ArrayList<ViewHolder> mPendingRemovals = new ArrayList<>();
ArrayList<ViewHolder> mRemoveAnimations = new ArrayList<>();

private ArrayList<MoveInfo> mPendingMoves = new ArrayList<>();
ArrayList<ArrayList<MoveInfo>> mMovesList = new ArrayList<>();
ArrayList<ViewHolder> mMoveAnimations = new ArrayList<>();

private ArrayList<ChangeInfo> mPendingChanges = new ArrayList<>();
ArrayList<ArrayList<ChangeInfo>> mChangesList = new ArrayList<>();
ArrayList<ViewHolder> mChangeAnimations = new ArrayList<>();
```

DefaultItemAnimator实现了SimpleItemAnimator的`animateAdd()`方法，该方法只是将该item添加到mPendingAdditions中，等到`runPendingAnimations()`中执行。

``` java
public boolean animateAdd(final ViewHolder holder) {
    resetAnimation(holder);  //重置清空所有动画
    ViewCompat.setAlpha(holder.itemView, 0); //将要做动画的View先变成透明
    mPendingAdditions.add(holder);
    return true;
}
```

接着看`runPendingAnimations()`的实现，该方法是执行remove,move,change,add动画，执行顺序为：remove动画最先执行，随后move和change并行执行，最后是add动画。为了简化，我们将remove,move,change动画执行过程省略，只看执行add动画的过程，如下：

``` java
public void runPendingAnimations() {
    //1、判断是否有动画要执行，即各个动画的成员变量里是否有值。
    //2、执行remove动画
    //3、执行move动画
    //4、执行change动画，与move动画并行执行
    //5、执行add动画
    if (additionsPending) {
        final ArrayList<ViewHolder> additions = new ArrayList<>();
        additions.addAll(mPendingAdditions);
        mAdditionsList.add(additions);
        mPendingAdditions.clear();
        Runnable adder = new Runnable() {
            @Override
            public void run() {
                for (ViewHolder holder : additions) {
                    animateAddImpl(holder);  //***** 执行动画的方法 *****
                }
                additions.clear();
                mAdditionsList.remove(additions);
            }
        };
        if (removalsPending || movesPending || changesPending) {
            long removeDuration = removalsPending ? getRemoveDuration() : 0;
            long moveDuration = movesPending ? getMoveDuration() : 0;
            long changeDuration = changesPending ? getChangeDuration() : 0;
            long totalDelay = removeDuration + Math.max(moveDuration, changeDuration);
            View view = additions.get(0).itemView;
            ViewCompat.postOnAnimationDelayed(view, adder, totalDelay); //等remove，move，change动画全部做完后，开始执行add动画
        }
    }
}
```

为了防止在执行add动画时外面有新的add动画添加到mPendingAdditions中，从而导致执行add动画错乱，这里将mPendingAdditions的内容移动到局部变量additions中，然后遍历additions执行动画。

在`runPendingAnimations()`中，`animateAddImpl()`是执行add动画的具体方法，其实就是将itemView的透明度从0变到1（在`animateAdd()`中已经将view的透明度变为0），实现如下：

``` java
void animateAddImpl(final ViewHolder holder) {
    final View view = holder.itemView;
    final ViewPropertyAnimatorCompat animation = ViewCompat.animate(view);
    mAddAnimations.add(holder);
    animation.alpha(1).setDuration(getAddDuration()).
            setListener(new VpaListenerAdapter() {
                @Override
                public void onAnimationStart(View view) {
                    dispatchAddStarting(holder);  //在开始add动画前调用
                }
                @Override
                public void onAnimationCancel(View view) {
                    ViewCompat.setAlpha(view, 1);
                }

                @Override
                public void onAnimationEnd(View view) {
                    animation.setListener(null);
                    dispatchAddFinished(holder); //在结束add动画后调用
                    mAddAnimations.remove(holder);
                    if (!isRunning()) {
                        dispatchAnimationsFinished(); //结束所有动画后调用
                    }
                }
            }).start();
}
```

#### 开源动画recyclerview-animators
从DefaultItemAnimator类的实现来看，发现自定义Item Animator好麻烦，需要继承SimpleItemAnimator类，然后实现一堆方法。
别急，[recyclerview-animators](https://github.com/wasabeef/recyclerview-animators)解救你，原因如下：
- 首先，[recyclerview-animators](https://github.com/wasabeef/recyclerview-animators)提供了一系列的Animator，比如FadeInAnimator,ScaleInAnimator。
- 其次，如果该库中没有你满意的动画，该库提供了BaseItemAnimator类，该类继承自SimpleItemAnimator，进一步封装了自定义Item Animator的代码，使得自定义Item Animator更方便，你只需要关注动画本身。如果要实现DefaultItemAnimator的代码，只需要以下实现：

``` java
public class DefaultItemAnimator extends BaseItemAnimator {

  public DefaultItemAnimator() {
  }

  public DefaultItemAnimator(Interpolator interpolator) {
    mInterpolator = interpolator;
  }

  @Override protected void animateRemoveImpl(final RecyclerView.ViewHolder holder) {
    ViewCompat.animate(holder.itemView)
        .alpha(0)
        .setDuration(getRemoveDuration())
        .setListener(new DefaultRemoveVpaListener(holder))
        .setStartDelay(getRemoveDelay(holder))
        .start();
  }

  @Override protected void preAnimateAddImpl(RecyclerView.ViewHolder holder) {
    ViewCompat.setAlpha(holder.itemView, 0); //透明度先变为0
  }

  @Override protected void animateAddImpl(final RecyclerView.ViewHolder holder) {
    ViewCompat.animate(holder.itemView)
        .alpha(1)
        .setDuration(getAddDuration())
        .setListener(new DefaultAddVpaListener(holder))
        .setStartDelay(getAddDelay(holder))
        .start();
  }
}
```

是不是比继承SimpleItemAnimator方便多了。

#### 局部刷新闪屏问题解决
对于RecyclerView的Item Animator，有一个常见的坑就是“闪屏问题”。
这个问题的描述是：当Item视图中有图片和文字，当更新文字并调用`notifyItemChanged()`时，文字改变的同时图片会闪一下。这个问题的原因是当调用`notifyItemChanged()`时，会调用DefaultItemAnimator的`animateChangeImpl()`执行change动画，该动画会使得Item的透明度从0变为1，从而造成闪屏。

解决办法很简单，在`rv.setAdapter()`之前调用`((SimpleItemAnimator)rv.getItemAnimator()).setSupportsChangeAnimations(false)`**禁用change动画**。







## 网格样式

RecyclerView展示的样式由布局管理器LayoutManager来控制。
网格样式的管理器是**`GridLayoutManager`**，看一下它最常用的两个构造函数以及参数含义。

- GridLayoutManager(Context context, int spanCount)
  - spanCount，每列或者每行的item个数，设置为1，就是列表样式
  - 该构造函数默认是竖直方向的网格样式
- GridLayoutManager(Context context, int spanCount, int orientation,boolean reverseLayout)
  - spanCount，每列或者每行的item个数，设置为1，就是列表样式
  - 网格样式的方向，水平（OrientationHelper.HORIZONTAL）或者竖直（OrientationHelper.VERTICAL）
  - reverseLayout，是否逆向，true：布局逆向展示，false：布局正向显示

```java
// 竖直方向的网格样式，每行四个Item
mLayoutManager = new GridLayoutManager(this, 4, OrientationHelper.VERTICAL, false);
mRecyclerView.setLayoutManager(mLayoutManager);
```

运行效果
![](http://upload-images.jianshu.io/upload_images/9028834-ab21a898f8f3c75a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

网格样式已经显示出来了，和之前遇见的问题一样，没有间隔线，非常丑，间隔线必须加，而且要使用自定义，不使用系统自带的。

新建文件md_divider.xml，是一个灰色的矩形。

```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
       android:shape="rectangle" >
    <solid android:color="@android:color/darker_gray"/>
    <size android:height="4dp" android:width="4dp"/>
</shape>
```

在styles.xml中的自定义的应用主题里替换掉listdivider属性。

```xml
<style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
    <item name="colorPrimary">@color/colorPrimary</item>
    <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
    <item name="colorAccent">@color/colorAccent</item>
    <item name="android:listDivider">@drawable/md_divider</item>
</style>
```

然后继承**`RecyclerView.ItemDecoration`**类，在构造函数里获取自定义的间隔线，复写绘制间隔线的方法。

```java
public class MDGridRvDividerDecoration extends RecyclerView.ItemDecoration {
    private static final int[] ATTRS = new int[]{android.R.attr.listDivider};
    // 用于绘制间隔样式
    private Drawable mDivider;
    
    public MDGridRvDividerDecoration(Context context) {
        // 获取默认主题的属性
        final TypedArray a = context.obtainStyledAttributes(ATTRS);
        mDivider = a.getDrawable(0);
        a.recycle();
    }

    @Override
    public void onDraw(Canvas c, RecyclerView parent, RecyclerView.State state) {
        // 绘制间隔，每一个item，绘制右边和下方间隔样式
        int childCount = parent.getChildCount();
        int spanCount = ((GridLayoutManager)parent.getLayoutManager()).getSpanCount();
        int orientation = ((GridLayoutManager)parent.getLayoutManager()).getOrientation();
        boolean isDrawHorizontalDivider = true;
        boolean isDrawVerticalDivider = true;
        int extra = childCount % spanCount;
        extra = extra == 0 ? spanCount : extra;
        for(int i = 0; i < childCount; i++) {
            isDrawVerticalDivider = true;
            isDrawHorizontalDivider = true;
            // 如果是竖直方向，最右边一列不绘制竖直方向的间隔
            if(orientation == OrientationHelper.VERTICAL && (i + 1) % spanCount == 0) {
                isDrawVerticalDivider = false;
            }

            // 如果是竖直方向，最后一行不绘制水平方向间隔
            if(orientation == OrientationHelper.VERTICAL && i >= childCount - extra) {
                isDrawHorizontalDivider = false;
            }

            // 如果是水平方向，最下面一行不绘制水平方向的间隔
            if(orientation == OrientationHelper.HORIZONTAL && (i + 1) % spanCount == 0) {
                isDrawHorizontalDivider = false;
            }

            // 如果是水平方向，最后一列不绘制竖直方向间隔
            if(orientation == OrientationHelper.HORIZONTAL && i >= childCount - extra) {
                isDrawVerticalDivider = false;
            }

            if(isDrawHorizontalDivider) {
                drawHorizontalDivider(c, parent, i);
            }

            if(isDrawVerticalDivider) {
                drawVerticalDivider(c, parent, i);
            }
        }
    }

    @Override
    public void getItemOffsets(Rect outRect, View view, RecyclerView parent, RecyclerView.State state) {
        int spanCount = ((GridLayoutManager) parent.getLayoutManager()).getSpanCount();
        int orientation = ((GridLayoutManager)parent.getLayoutManager()).getOrientation();
        int position = parent.getChildLayoutPosition(view);
        if(orientation == OrientationHelper.VERTICAL && (position + 1) % spanCount == 0) {
            outRect.set(0, 0, 0, mDivider.getIntrinsicHeight());
            return;
        }

        if(orientation == OrientationHelper.HORIZONTAL && (position + 1) % spanCount == 0) {
            outRect.set(0, 0, mDivider.getIntrinsicWidth(), 0);
            return;
        }

        outRect.set(0, 0, mDivider.getIntrinsicWidth(), mDivider.getIntrinsicHeight());
    }

    /* 绘制竖直间隔线
     * @param canvas
     * @param parent   父布局，RecyclerView
     * @param position item在父布局中所在的位置     */
    private void drawVerticalDivider(Canvas canvas, RecyclerView parent, int position) {
        final View child = parent.getChildAt(position);
        final RecyclerView.LayoutParams params = (RecyclerView.LayoutParams) child
                .getLayoutParams();
        final int top = child.getTop() - params.topMargin;
        final int bottom = child.getBottom() + params.bottomMargin + mDivider.getIntrinsicHeight();
        final int left = child.getRight() + params.rightMargin;
        final int right = left + mDivider.getIntrinsicWidth();
        mDivider.setBounds(left, top, right, bottom);
        mDivider.draw(canvas);
    }

    /* 绘制水平间隔线
     * @param canvas
     * @param parent   父布局，RecyclerView
     * @param position item在父布局中所在的位置     */
    private void drawHorizontalDivider(Canvas canvas, RecyclerView parent, int position) {
        final View child = parent.getChildAt(position);
        final RecyclerView.LayoutParams params = (RecyclerView.LayoutParams) child
                .getLayoutParams();
        final int top = child.getBottom() + params.bottomMargin;
        final int bottom = top + mDivider.getIntrinsicHeight();
        final int left = child.getLeft() - params.leftMargin;
        final int right = child.getRight() + params.rightMargin + mDivider.getIntrinsicWidth();
        mDivider.setBounds(left, top, right, bottom);
        mDivider.draw(canvas);
    }
}
```

设置RecyclerView的间隔线。

```java
mRecyclerView.addItemDecoration(new MDGridRvDividerDecoration(this));
```

运行效果如下图。

![](http://upload-images.jianshu.io/upload_images/9028834-4355b6ececfe4bd0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

关于网格样式的RecyclerView使用大体和列表样式相同，主要在于间隔线的实现上有些不同，来看一下如果真正的使用自定义的间隔线需要做些什么。

- 实现间隔线样式，可以是xml文件也可以是图片
- 覆盖应用主题的listdivider属性，使用自定义的间隔线样式
- 继承`RecyclerView.ItemDecoration`类，并实现其中的绘制间隔线方法
- 设置RecyclerView间隔线样式

## 瀑布流样式

RecyclerView的瀑布流布局管理器是**`StaggeredGridLayoutManager`**，它最常用的构造函数就一个，StaggeredGridLayoutManager(int spanCount, int orientation)，spanCount代表每行或每列的Item个数，orientation代表列表的方向，竖直或者水平。

看在代码中的使用。

```java
// 初始化布局管理器
mLayoutManager = new StaggeredGridLayoutManager(2, OrientationHelper.VERTICAL);
// 设置布局管理器
mRecyclerView.setLayoutManager(mLayoutManager);
// 设置adapter
mRecyclerView.setAdapter(mAdapter);
// 设置间隔样式
mRecyclerView.addItemDecoration(new MDStaggeredRvDividerDecotation(this));
```

要实现瀑布流效果（仅讨论竖直方向的瀑布流样式），每一个Item的高度要有所差别，如果所有的item的高度相同，就和网格样式是一样的展示效果。示例中就实现两中不同高度的Item，一个高度为80dp,一个高度为100dp。

view_rv_staggered_item.xml布局：

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
              xmlns:tools="http://schemas.android.com/tools"
              android:orientation="vertical"
              android:layout_width="match_parent"
              android:layout_height="80dp">
    <TextView
        android:id="@+id/item_tv"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:gravity="center"
        tools:text="item"/>
</LinearLayout>
```

view_rv_staggered_item_two.xml布局：

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
              xmlns:tools="http://schemas.android.com/tools"
              android:orientation="vertical"
              android:layout_width="match_parent"
              android:layout_height="100dp">
    <TextView
        android:id="@+id/item_tv"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:gravity="center"
        tools:text="item"/>
</LinearLayout>
```

Item不同的布局是在Adapter里面绑定的，看一下Adapter的实现。

```java
public class MDStaggeredRvAdapter extends RecyclerView.Adapter<MDStaggeredRvAdapter.ViewHolder> {
    // 展示数据
    private ArrayList<String> mData;
    public MDStaggeredRvAdapter(ArrayList<String> data) {
        this.mData = data;
    }
    public void updateData(ArrayList<String> data) {
        this.mData = data;
        notifyDataSetChanged();
    }

    @Override
    public int getItemViewType(int position) {
        // 瀑布流样式外部设置spanCount为2，在这列设置两个不同的item type，以区分不同的布局
        return position % 2;
    }

    @Override
    public MDStaggeredRvAdapter.ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        // 实例化展示的view
        View v;
        if(viewType == 1) {
            v = LayoutInflater.from(parent.getContext()).inflate(R.layout.view_rv_staggered_item, parent, false);
        } else {
            v = LayoutInflater.from(parent.getContext()).inflate(R.layout.view_rv_staggered_item_two, parent, false);
        }
        // 实例化viewholder
        ViewHolder viewHolder = new ViewHolder(v);
        return viewHolder;
    }

    @Override
    public void onBindViewHolder(MDStaggeredRvAdapter.ViewHolder holder, int position) {
        // 绑定数据
        holder.mTv.setText(mData.get(position));
    }

    @Override
    public int getItemCount() {
        return mData == null ? 0 : mData.size();
    }

    public static class ViewHolder extends RecyclerView.ViewHolder {

        TextView mTv;

        public ViewHolder(View itemView) {
            super(itemView);
            mTv = (TextView) itemView.findViewById(R.id.item_tv);
        }
    }
}
```

接下来是设置瀑布流样式的间隔线样式的，上面代码中使用的是`MDStaggeredRvDividerDecotation`类，其实是直接拷贝的网格样式的间隔线绘制类。看一下运行效果。

![](http://upload-images.jianshu.io/upload_images/9028834-4bcecf86ca2b97ba.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 瀑布流间隔样式

很奇怪，间隔线并没有按照我们想象中的方式绘制，仔细看瀑布流中Item的分布，发现瀑布流样式的Item分布和网格样式的Item分布有些不同。对比一下两者Item的分布，如下图。

![](http://upload-images.jianshu.io/upload_images/9028834-aefde91beabf7701.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

网格样式的Item分布规律很明显，竖直方向的网格，Item是从左向右从上到下依次按顺序排列分布。

瀑布流样式的Item分布也是从上到下，从左到右的顺序排列，但是有一个高度的优先级，如果某一列中有一个高度最低的位置为空，最优先在此处添加Item。看第三张图的`3 item`，因为该位置最低，优先在此处添加Item。

分析出了瀑布流样式的Item的分布规律，就会发现，按照以往列表样式或者网格样式去设置间隔线是有问题的，因为不知道Item具体的位置，上下左右间隔线是否需要绘制不确定，参考第二张图，其实第三张图的间隔线也有问题，向上滑动就会展示出来。

目前能考虑到的瀑布流添加间隔线的思路：

- Item布局中设置四周间隔padding/margin
- 代码中动态修改ItemView的间隔padding/margin

设置间隔有两个方法：

- 上下左右都设置间隔
- 相邻两边设置间隔（左上/左下/右上/右下）

第一种设置间隔的方法会导致相邻的Item间距是间隔的两倍，第二种设置间隔的方法会导致Item某一个方向上的与父布局边缘无间隔，但是另一个方向与父布局边缘有间隔，例如左上相邻两边设置了间隔，最左边一列的Item左边与父布局边缘有间隔，但是最右边一列Item右边与父布局无间隔，第一行和最后一行的Item也会出现这种情况。

要解决上面的问题，父布局RecyclerView也需要根据相应的情况设置padding让整个布局的间隔都一致。下面的例子是选择在Item布局中设置间隔，因为可以自己在布局文件中控制颜色比较方便，选择右下两边设置间隔。

首先修改两个Item的布局文件。
view_rv_staggered_item.xml修改背景色和外层间距背景色。

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
              xmlns:tools="http://schemas.android.com/tools"
              android:orientation="vertical"
              android:layout_width="match_parent"
              android:layout_height="@dimen/md_common_view_height"
              android:background="@color/md_divider"
              android:paddingBottom="5dp"
              android:paddingRight="5dp">
    <TextView
        android:id="@+id/item_tv"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:gravity="center"
        android:background="@color/md_white"
        tools:text="item"/>
</LinearLayout>
```

同样修改view_rv_staggered_item_two.xml。

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
              xmlns:tools="http://schemas.android.com/tools"
              android:orientation="vertical"
              android:layout_width="match_parent"
              android:layout_height="100dp"
              android:paddingBottom="5dp"
              android:paddingRight="5dp"
              android:background="@color/md_divider">
    <TextView
        android:id="@+id/item_tv"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:gravity="center"
        android:background="@color/md_white"
        tools:text="item"/>
</LinearLayout>

```

最后修改RecyclerView的一些属性。

```xml
<android.support.v7.widget.RecyclerView
        android:id="@+id/my_recycler_view"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:background="@color/md_divider"
        android:paddingLeft="5dp"
        android:paddingTop="5dp"
        android:fadeScrollbars="true"/>

```

运行一下，看看最后的效果。
[![](http://upload-images.jianshu.io/upload_images/9028834-2d66f9dcb6c48c95.gif?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-2d66f9dcb6c48c95.gif?imageMogr2/auto-orient/strip)

差不多完美的解决了间隔线的问题，有细心的同学可能发现，在RecyclerView滑动的时候上面一直有一条灰色的间隔线，这个可以通过取消xml布局文件中RecyclerView的paddingTop属性去掉顶部灰色的间隔线。




## 拓展RecyclerView

###  添加点击事件

RecyclerView并没有像ListView一样暴露出Item点击事件或者长按事件处理的api，也就是说使用RecyclerView时候，需要我们自己来实现Item的点击和长按等事件的处理。
实现方法有很多：

- 可以**监听RecyclerView的Touch事件**然后判断手势做相应的处理，
- 也可以通过**在绑定ViewHolder的时候设置监听**，然后通过Apater回调出去

我们选择第二种方法，更加直观和简单。
看一下Adapter的完整代码。

```java
public class MyAdapter extends RecyclerView.Adapter<MyAdapter.ViewHolder>{
    // 展示数据
    private ArrayList<String> mData;
    public MyAdapter(ArrayList<String> data) {
        this.mData = data;
    }
    public void updateData(ArrayList<String> data) {
        this.mData = data;
        notifyDataSetChanged();
    }
    // 添加新的Item
    public void addNewItem() {
        if(mData == null) {
            mData = new ArrayList<>();
        }
        mData.add(0, "new Item");
        notifyItemInserted(0);
    }
    // 删除Item
    public void deleteItem() {
        if(mData == null || mData.isEmpty()) {
            return;
        }
        mData.remove(0);
        notifyItemRemoved(0);
    }

    // 事件回调监听
    private MyAdapter.OnItemClickListener onItemClickListener;
    // ① 定义点击回调接口
    public interface OnItemClickListener {
        void onItemClick(View view, int position);
        void onItemLongClick(View view, int position);
    }
    
    // ② 定义一个设置点击监听器的方法
    public void setOnItemClickListener(MyAdapter.OnItemClickListener listener) {
        this.onItemClickListener = listener;
    }

    @Override
    public ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        // 实例化展示的view
        View v = LayoutInflater.from(parent.getContext()).inflate(R.layout.view_rv_item, parent, false);
        // 实例化viewholder
        ViewHolder viewHolder = new ViewHolder(v);
        return viewHolder;
    }

    @Override
    public void onBindViewHolder(final ViewHolder holder, int position) {
        // 绑定数据
        holder.mTv.setText(mData.get(position));
        //③ 对RecyclerView的每一个itemView设置点击事件
        holder.itemView.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(final View v) {
                if(onItemClickListener != null) {
                    int pos = holder.getLayoutPosition();
                    onItemClickListener.onItemClick(holder.itemView, pos);
                }
            }
        });

        holder.itemView.setOnLongClickListener(new View.OnLongClickListener() {
            @Override
            public boolean onLongClick(View v) {
                if(onItemClickListener != null) {
                    int pos = holder.getLayoutPosition();
                    onItemClickListener.onItemLongClick(holder.itemView, pos);
                }
                //表示此事件已经消费，不会触发单击事件
                return true;
            }
        });
    }

    @Override
    public int getItemCount() {
        return mData == null ? 0 : mData.size();
    }

    public static class ViewHolder extends RecyclerView.ViewHolder {
        TextView mTv;
        public ViewHolder(View itemView) {
            super(itemView);
            mTv = (TextView) itemView.findViewById(R.id.item_tv);
        }
    }
}
```

设置Adapter的事件监听。

```java
mAdapter.setOnItemClickListener(new MyAdapter.OnItemClickListener() {
    @Override
    public void onItemClick(View view, int position) {
        Toast.makeText(MDRvActivity.this,"click " + position + " item", Toast.LENGTH_SHORT).show();
    }

    @Override
    public void onItemLongClick(View view, int position) {
        Toast.makeText(MDRvActivity.this,"long click " + position + " item", Toast.LENGTH_SHORT).show();
    }
});
```

最后的实现效果。

[![](http://upload-images.jianshu.io/upload_images/9028834-7bc274b8a4be673e.gif?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-7bc274b8a4be673e.gif?imageMogr2/auto-orient/strip)



### 添加HeaderView和FooterView

RecyclerView默认没有提供类似`addHeaderView()`和`addFooterView()`的API，因此这里介绍如何优雅地实现这两个接口。

如果你已经实现了一个Adapter，现在想为这个Adapter添加`addHeaderView()`和`addFooterView()`接口，则需要在Adapter中添加几个Item Type，然后修改`getItemViewType()`,`onCreateViewHolder()`,`onBindViewHolder()`,`getItemCount()`等方法，并添加switch语句进行判断。那么如何在不破坏原有Adapter实现的情况下完成呢？

这里引入**装饰器（Decorator）设计模式**，该设计模式通过组合的方式，在不破话原有类代码的情况下，对原有类的功能进行扩展。

具体实现思路其实很简单，创建一个继承`RecyclerView.Adapter<RecyclerView.ViewHolder>`的类，并重写常见的方法，然后**通过引入ITEM TYPE的方式实现**：
``` java
public class NormalAdapterWrapper extends RecyclerView.Adapter<RecyclerView.ViewHolder>{
//不同布局类型
    enum ITEM_TYPE{
        HEADER,
        FOOTER,
        NORMAL
    }

    private NormalAdapter mAdapter;
    private View mHeaderView;
    private View mFooterView;

    public NormalAdapterWrapper(NormalAdapter adapter){
        mAdapter = adapter;
    }

    @Override
    public int getItemViewType(int position) {
        if(position == 0){
            return ITEM_TYPE.HEADER.ordinal();
        } else if(position == mAdapter.getItemCount() + 1){
            return ITEM_TYPE.FOOTER.ordinal();
        } else{
            return ITEM_TYPE.NORMAL.ordinal();
        }
    }

    @Override
    public int getItemCount() {
        return mAdapter.getItemCount() + 2;
    }

    @Override
    public void onBindViewHolder(RecyclerView.ViewHolder holder, int position) {
        if(position == 0){
            return;
        } else if(position == mAdapter.getItemCount() + 1){
            return;
        } else{
            mAdapter.onBindViewHolder(((NormalAdapter.VH)holder), position - 1);
        }
    }

    @Override
    public RecyclerView.ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
        if(viewType == ITEM_TYPE.HEADER.ordinal()){
            return new RecyclerView.ViewHolder(mHeaderView) {};
        } else if(viewType == ITEM_TYPE.FOOTER.ordinal()){
            return new RecyclerView.ViewHolder(mFooterView) {};
        } else{
            return mAdapter.onCreateViewHolder(parent,viewType);
        }
    }

    public void addHeaderView(View view){
        this.mHeaderView = view;
    }
    public void addFooterView(View view){
        this.mFooterView = view;
    }
}
```

这恰恰满足了我们的需求。我们只需要通过以下方式为原有的Adapter（这里命名为NormalAdapter）添加`addHeaderView()`和`addFooterView()`接口：

``` java
NormalAdapter adapter = new NormalAdapter(data);
NormalAdapterWrapper newAdapter = new NormalAdapterWrapper(adapter);
View headerView = LayoutInflater.from(this).inflate(R.layout.item_header, mRecyclerView, false);
View footerView = LayoutInflater.from(this).inflate(R.layout.item_footer, mRecyclerView, false);
newAdapter.addFooterView(footerView);
newAdapter.addHeaderView(headerView);
mRecyclerView.setAdapter(newAdapter);
```

是不是看起来特别优雅。

### 添加setEmptyView

ListView提供了`setEmptyView()`设置Adapter数据为空时的View视图。RecyclerView虽然没提供直接的API，但是也可以很简单地实现。

*   创建一个继承RecyclerView的类，记为EmptyRecyclerView。
*   通过**`getParent().addView(emptyView)`**将空数据时显示的emptyView添加到当前View父控件的层次结构中。
*   通过**`AdapterDataObserver`监听RecyclerView的数据变化**，如果adapter为空，那么隐藏RecyclerView，显示EmptyView。

具体实现如下：

``` java
public class EmptyRecyclerView extends RecyclerView {
    private View mEmptyView;

    private AdapterDataObserver mObserver = new AdapterDataObserver() {
        @Override
        public void onChanged() {
            Adapter adapter = getAdapter();
            if (adapter.getItemCount() == 0) {
                mEmptyView.setVisibility(VISIBLE);
                EmptyRecyclerView.this.setVisibility(GONE);
            } else {
                mEmptyView.setVisibility(GONE);
                EmptyRecyclerView.this.setVisibility(VISIBLE);
            }
        }
        @Override
        public void onItemRangeChanged(int positionStart, int itemCount) {onChanged();}
        @Override
        public void onItemRangeMoved(int fromPosition, int toPosition, int itemCount) {onChanged();}
        @Override
        public void onItemRangeRemoved(int positionStart, int itemCount) {onChanged();}
        @Override
        public void onItemRangeInserted(int positionStart, int itemCount) {onChanged();}
        @Override
        public void onItemRangeChanged(int positionStart, int itemCount, Object payload) {onChanged();}
    };

    public EmptyRecyclerView(Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);
    }

    public void setEmptyView(View view) {
        mEmptyView = view;
        //将EmptyView加入父控件布局中
        ((ViewGroup) this.getParent()).addView(mEmptyView);
    }

    public void setAdapter(RecyclerView.Adapter adapter) {
        super.setAdapter(adapter);
        //监听数据变化
        adapter.registerAdapterDataObserver(mObserver);
        mObserver.onChanged();
    }
}
```



Activity中使用
``` java
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_5);
        mRv = (EmptyRecyclerView) findViewById(R.id.rv);
        mRv.setLayoutManager(new LinearLayoutManager(this));
        mData = new ArrayList<>();
        mAdapter = new NormalAdapter(mData);
        View view = LayoutInflater.from(this).inflate(R.layout.empty, null);
        //View view = findViewById(R.id.text_empty);
        mRv.setEmptyView(view);
        mRv.setAdapter(mAdapter);
    }
```

### 拖拽、侧滑删除

Android提供了ItemTouchHelper类，使得RecyclerView能够轻易地实现滑动和拖拽，此处我们要实现上下拖拽和侧滑删除。

#### ① 创建ItemTouchHelper.Callback类
首先**创建**一个**继承自`ItemTouchHelper.Callback`的类**，并重写以下方法：

*   **`getMovementFlags()`**: 设置**支持**的拖拽和滑动的**方向**，此处我们支持的拖拽方向为上下，滑动方向为从左到右和从右到左，内部通过`makeMovementFlags()`设置。
*   **`onMove()`**: 拖拽时回调。
*   **`onSwiped()`**: 滑动时回调。
*   **`onSelectedChanged()`**: 状态变化时回调，一共有三个状态，分别是ACTION_STATE_**IDLE**(空闲状态)，ACTION_STATE_**SWIPE**(滑动状态)，ACTION_STATE_**DRAG**(拖拽状态)。此方法中可以做一些状态变化时的处理，比如拖拽的时候修改背景色。
*   **`clearView()`**: 用户**交互结束**时回调。此方法可以做一些状态的清空，比如拖拽结束后还原背景色。
*   **`isLongPressDragEnabled()`**: 是否支持长按拖拽，**默认为true**。如果不想支持长按拖拽，则重写并返回false。

具体实现如下：
``` java
public class SimpleItemTouchCallback extends ItemTouchHelper.Callback {

    private NormalAdapter mAdapter;
    private List<ObjectModel> mData;
    public SimpleItemTouchCallback(NormalAdapter adapter, List<ObjectModel> data){
        mAdapter = adapter;
        mData = data;
    }

    //设置支持的拖拽、滑动的方向
    @Override
    public int getMovementFlags(RecyclerView recyclerView, RecyclerView.ViewHolder viewHolder) {
        int dragFlag = ItemTouchHelper.UP | ItemTouchHelper.DOWN; //s上下拖拽
        int swipeFlag = ItemTouchHelper.START | ItemTouchHelper.END; //左->右和右->左滑动
        return makeMovementFlags(dragFlag,swipeFlag);
    }

    @Override
    public boolean onMove(RecyclerView recyclerView, RecyclerView.ViewHolder viewHolder, RecyclerView.ViewHolder target) {
        int from = viewHolder.getAdapterPosition();
        int to = target.getAdapterPosition();
        Collections.swap(mData, from, to);
        mAdapter.notifyItemMoved(from, to);
        return true;
    }

    @Override
    public void onSwiped(RecyclerView.ViewHolder viewHolder, int direction) {
        int pos = viewHolder.getAdapterPosition();
        mData.remove(pos);
        mAdapter.notifyItemRemoved(pos);
    }

    //状态改变时回调
    @Override
    public void onSelectedChanged(RecyclerView.ViewHolder viewHolder, int actionState) {
        super.onSelectedChanged(viewHolder, actionState);
        if(actionState != ItemTouchHelper.ACTION_STATE_IDLE){
            NormalAdapter.VH holder = (NormalAdapter.VH)viewHolder;
            holder.itemView.setBackgroundColor(0xffbcbcbc); //设置拖拽和侧滑时的背景色
        }
    }

    //拖拽或滑动完成之后调用，用来清除一些状态
    @Override
    public void clearView(RecyclerView recyclerView, RecyclerView.ViewHolder viewHolder) {
        super.clearView(recyclerView, viewHolder);
        NormalAdapter.VH holder = (NormalAdapter.VH)viewHolder;
        holder.itemView.setBackgroundColor(0xffeeeeee); //背景色还原
    }
}
```
#### ② 设置ItemTouchHelper给RecyclerView
然后通过以下代码为RecyclerView设置该滑动、拖拽功能：

**`ItemTouchHelper.attachToRecyclerView`**(recyclerview);

``` java
ItemTouchHelper helper = new ItemTouchHelper(new SimpleItemTouchCallback(adapter, data));
helper.attachToRecyclerView(recyclerview);
```

#### 触摸拖拽
前面拖拽的触发方式只有长按，如果想支持触摸Item中的某个View实现拖拽，则核心方法为**`helper.startDrag(holder)`**。

首先定义接口：

``` java
interface OnStartDragListener{
    void startDrag(RecyclerView.ViewHolder holder);
}
```

然后让Activity实现该接口：

```java
public MainActivity extends Activity implements OnStartDragListener{
    ...
    public void startDrag(RecyclerView.ViewHolder holder) {
      //核心方法
        mHelper.startDrag(holder);
    }
}
```

如果要对ViewHolder的text对象支持触摸拖拽，则在Adapter中的`onBindViewHolder()`中添加：

```java
holder.text.setOnTouchListener(new View.OnTouchListener() {
    @Override
    public boolean onTouch(View v, MotionEvent event) {
        if(event.getAction() == MotionEvent.ACTION_DOWN){
            mListener.startDrag(holder);
        }
        return false;
    }
});
```

其中mListener是在创建Adapter时将实现OnStartDragListener接口的Activity对象作为参数传进来。

**完整代码**如下：
``` java
public class NormalAdapter extends RecyclerView.Adapter<NormalAdapter.VH>{

    private List<ObjectModel> mDatas;
    private OnStartDragListener mListener;
    public NormalAdapter(List<ObjectModel> data, OnStartDragListener listener) {
        this.mDatas = data;
        mListener = listener;
    }

    @Override
    public void onBindViewHolder(final VH holder, int position) {
        ObjectModel model = mDatas.get(position);
        holder.title.setText(model.title);
        holder.number.setOnTouchListener(new View.OnTouchListener() {
            @Override
            public boolean onTouch(View v, MotionEvent event) {
                if(event.getAction() == MotionEvent.ACTION_DOWN){
                    mListener.startDrag(holder);
                }
                return false;
            }
        });
    }

    @Override
    public int getItemCount() {
        return mDatas.size();
    }

    @Override
    public VH onCreateViewHolder(ViewGroup parent, int viewType) {
        View v = LayoutInflater.from(parent.getContext()).inflate(R.layout.item_3, parent, false);
        return new VH(v);
    }

    public static class VH extends RecyclerView.ViewHolder{
        public final TextView title;
        public final ImageView number;
        public VH(View v) {
            super(v);
            title = (TextView) v.findViewById(R.id.title);
            number = (ImageView) v.findViewById(R.id.icon);
        }
    }
}
```

``` java
public class Activity3 extends AppCompatActivity implements OnStartDragListener{
    private RecyclerView mRv;
    private NormalAdapter mAdapter;
    private ItemTouchHelper mHelper;
    private List<ObjectModel> mData;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_3);
        mRv = (RecyclerView) findViewById(R.id.rv);
        mRv.setLayoutManager(new LinearLayoutManager(this));
        mAdapter = new NormalAdapter(mData = initData(), this);
        mRv.setAdapter(mAdapter);
        mHelper = new ItemTouchHelper(new SimpleItemTouchCallback(mAdapter, mData));
        mHelper.attachToRecyclerView(mRv);

    }

    public ArrayList<ObjectModel> initData(){
        ArrayList<ObjectModel> models = new ArrayList<>();
        String[] titles = getResources().getStringArray(R.array.title_array);
        for(int i=0;i<titles.length;i++){
            ObjectModel model = new ObjectModel();
            model.number = i + 1;
            model.title = titles[i];
            models.add(model);
        }
        return models;
    }

    @Override
    public void startDrag(RecyclerView.ViewHolder holder) {
        mHelper.startDrag(holder);
    }
}

interface OnStartDragListener{
    void startDrag(RecyclerView.ViewHolder holder);
}
```

### 嵌套滑动机制
Android 5.0推出了嵌套滑动机制（NestedScrolling），在之前，一旦子View处理了触摸事件，父View就没有机会再处理这次的触摸事件，而嵌套滑动机制解决了这个问题。
- 熟悉 Android 触摸事件分发机制的童鞋肯定知道，Touch 事件在进行分发的时候，由父 View 向它的子 View 传递，一旦某个子 View 开始接收进行处理，那么接下来所有事件都将由这个 View 来进行处理，它的 ViewGroup 将不会再接收到这些事件，直到下一次手指按下。
- 而嵌套滚动机制（NestedScrolling）就是为了弥补这一机制的不足，为了让子 View 能和父 View 同时处理一个 Touch 事件。
  其关键在于NestedScrollingChild 和 NestedScrollingParent 两个接口，以及系统对这两个接口的实现类 NestedScrollingChildHelper 和 NestedScrollingParentHelper。
  为了支持嵌套滑动，**子View**必须**实现NestedScrollingChild**接口，**父View**必**须实现NestedScrollingParent**接口。
  而**RecyclerView实现了NestedScrollingChild**接口，而**CoordinatorLayout实现了NestedScrollingParent**接口。

下图是实现CoordinatorLayout嵌套RecyclerView的效果：
[![](http://upload-images.jianshu.io/upload_images/9028834-671d2a393db0fb5f.gif?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-671d2a393db0fb5f.gif?imageMogr2/auto-orient/strip)

一开始上面一大块区域就是 CollapsingToolbarLayout ，下方的列表是 RecyclerView ，当然 RecyclerView 向上滑动时，CollapsingToolbarLayout 能够同时往上收缩，直到只剩下顶部的 Toolbar。之所以能够实现这种效果，就是完全依赖于嵌套滚动机制，如果没有这套机制，按照原有的触摸事件分发逻辑， RecyclerView 内部已经把 Touch 事件消耗掉了，完全无法引起顶部的 CollapsingToolbarLayout 产生联动收缩的效果。

为了实现上图的效果，需要用到的组件有：

*   CoordinatorLayout: 布局根元素。
*   AppBarLayout: 包裹的内容作为应用的Bar。
*   CollapsingToolbarLayout: 实现可折叠的ToolBar。
*   ToolBar: 代替ActionBar。

实现中需要注意的点有：
*   我们为ToolBar的`app:layout_collapseMode`设置为pin，表示折叠之后固定在顶端，而为ImageView的`app:layout_collapseMode`设置为parallax，表示视差模式，即渐变的效果。
*   为了让RecyclerView支持嵌套滑动，还需要为它设置`app:layout_behavior="@string/appbar_scrolling_view_behavior"`。
*   为CollapsingToolbarLayout设置`app:layout_scrollFlags="scroll|exitUntilCollapsed"`，其中scroll表示滚动出屏幕，exitUntilCollapsed表示退出后折叠。
*   如果在其他代码布局都不变的情况下，我们把 RecyclerView 替换成 ListView ，则无法产生上面图中的动态效果，因为 **ListView 并不支持嵌套滚动机制**，事件在 ListView 内部已经被消耗且无法传递出来。对 AppBarLayout 的使用也是同理。
  如果你想使用类似 AppBarLayout 、 CollapsingToolbarLayout 这种需要嵌套滚动的机制才能达到效果的控件，那么 RecyclerView 将是你的不二之选，因为 ListView 在此根本无法发挥作用。同样的，**ScrollView** 也是**不支持**嵌套滚动机制，但是你可以**使用 NestedScrollView** 。

具体实现参见Demo6。
布局：
```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.CoordinatorLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <android.support.design.widget.AppBarLayout
        android:id="@+id/appbar"
        android:layout_width="match_parent"
        android:layout_height="256dp"
        android:theme="@style/ThemeOverlay.AppCompat.Dark.ActionBar"
        android:fitsSystemWindows="true">

        <android.support.design.widget.CollapsingToolbarLayout
            android:id="@+id/collapsing_toolbar"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            app:layout_scrollFlags="scroll|exitUntilCollapsed"
            android:fitsSystemWindows="true"
            app:contentScrim="?attr/colorPrimary"
            app:expandedTitleMarginStart="48dp"
            app:expandedTitleMarginEnd="64dp">
        <ImageView
            android:id="@+id/backdrop"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:scaleType="centerCrop"
            android:fitsSystemWindows="true"
            android:src="@drawable/s8"
            app:layout_collapseMode="parallax"
            />
            <android.support.v7.widget.Toolbar
                android:id="@+id/toolbar"
                android:layout_width="match_parent"
                android:layout_height="?attr/actionBarSize"
                app:popupTheme="@style/ThemeOverlay.AppCompat.Light"
                app:layout_collapseMode="pin"
                />
        </android.support.design.widget.CollapsingToolbarLayout>
    </android.support.design.widget.AppBarLayout>
    <android.support.v7.widget.RecyclerView
        android:id="@+id/rv"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_behavior="@string/appbar_scrolling_view_behavior"
        />
</android.support.design.widget.CoordinatorLayout>
```
```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical" android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:layout_margin="5dp"
    >
    <ImageView
        android:id="@+id/image"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:adjustViewBounds="true"
        android:scaleType="centerCrop"
        />
</RelativeLayout>
```

适配器：

``` java
public abstract class QuickAdapter<T> extends RecyclerView.Adapter<QuickAdapter.VH>{

    private List<T> mDatas;

    public QuickAdapter(List<T> datas){
        this.mDatas = datas;
    }

    public abstract int getLayoutId(int viewType);

    @Override
    public VH onCreateViewHolder(ViewGroup parent, int viewType) {
        return VH.get(parent,getLayoutId(viewType));
    }

    @Override
    public void onBindViewHolder(VH holder, int position) {
        convert(holder, mDatas.get(position), position);
    }

    @Override
    public int getItemCount() {
        return mDatas.size();
    }

    public abstract void convert(VH holder, T data, int position);

    static class VH extends RecyclerView.ViewHolder{
        private SparseArray<View> mViews;
        private View mConvertView;

        private VH(View v){
            super(v);
            mConvertView = v;
            mViews = new SparseArray<>();
        }

        public static VH get(ViewGroup parent, int layoutId){
            View convertView = LayoutInflater.from(parent.getContext()).inflate(layoutId, parent, false);
            return new VH(convertView);
        }

        public <T extends View> T getView(int id){
            View v = mViews.get(id);
            if(v == null){
                v = mConvertView.findViewById(id);
                mViews.put(id, v);
            }
            return (T)v;
        }

        public void setText(int id, String value){
            TextView view = getView(id);
            view.setText(value);
        }
    }
}
```

Activity：
```java
public class Activity6 extends AppCompatActivity {
    private RecyclerView mRv;
    private QuickAdapter<Integer> mAdapter;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_6);

        Toolbar toolbar = (Toolbar) findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);
        getSupportActionBar().setDisplayHomeAsUpEnabled(true);

        mRv = (RecyclerView) findViewById(R.id.rv);
        mRv.setLayoutManager(new StaggeredGridLayoutManager(3, StaggeredGridLayoutManager.VERTICAL));
        mAdapter = new QuickAdapter<Integer>(initData()) {
            @Override
            public int getLayoutId(int viewType) {
                return R.layout.item_6;
            }

            @Override
            public void convert(VH holder, Integer data, int position) {
                ImageView imageView = holder.getView(R.id.image);
                Picasso.with(Activity6.this).load(data).into(imageView);
                //holder.itemView.setOnClickListener();  此处添加点击事件
            }

            @Override
            public int getItemViewType(int position) {
                return super.getItemViewType(position);
            }
        };
        mAdapter.setHasStableIds(true);
        ((SimpleItemAnimator)mRv.getItemAnimator()).setSupportsChangeAnimations(false);
        mRv.setAdapter(mAdapter);

    }

    public List<Integer> initData(){
        Integer[] images = {R.drawable.s1, R.drawable.s2, R.drawable.s3, R.drawable.s4, R.drawable.s5,
                        R.drawable.s6, R.drawable.s7, R.drawable.s8, R.drawable.s9, R.drawable.s10
                };
        ArrayList<Integer> list = new ArrayList<>();
        for(int i=0;i<2;i++){
            for(Integer image:images){
                list.add(image);
            }
        }
        return list;
    }
}
```



## 总结

- 水平列表展示，设置LayoutManager的方向性
- 竖直列表展示，设置LayoutManager的方向性
- 自定义间隔，RecyclerView.addItemDecoration()
- Item添加和删除动画,RecyclerView.setItemAnimator()
- **网格**样式的布局管理器**GridLayoutManager**的spanCount设置为1，就是列表样式
- 瀑布流样式如果Item的布局文件是等高，竖直方向，就是竖直方向的网格样式；如果Item是等宽，水平方向，那就是水平方向的网络样式
- 如果**瀑布流**样式的布局管理器**StaggeredGridLayoutManager**的spanCount设置为1，竖直方向，是竖直方向的列表；水平方向，就是水平方向的列表



# RecyclerView vs ListView

**ListView**相比RecyclerView，有一些**优点**：
*   **`addHeaderView()`**, **`addFooterView()`**添加头视图和尾视图。
*   通过**”android:divider”**设置自定义分割线。
*   **`setOnItemClickListener()`**和**`setOnItemLongClickListener()`**设置点击事件和长按事件。

这些功能在RecyclerView中都没有直接的接口，要自己实现（虽然实现起来很简单），因此如果只是实现简单的显示功能，ListView无疑更简单。

**RecyclerView**相比ListView，有一些明显的**优点**：
*   默认**已**经**实现**了**View**的**复用**，不需要类似`if(convertView == null)`的实现，而且回收机制更加完善。
*   默认**支持局部刷新**。
*   容易实现**添加item、删除item**的**动画**效果。
*   容易实现**拖拽、侧滑删除**等功能。

RecyclerView是一个插件式的实现，对各个功能进行解耦，从而扩展性比较好。




## 局部刷新

### ListView实现局部刷新

我们都知道ListView通过`adapter.notifyDataSetChanged()`实现ListView的更新，这种更新方法的缺点是**全局更新**，即对每个Item View都进行重绘。但事实上很多时候，我们只是更新了其中一个Item的数据，其他Item其实可以不需要重绘。

这里给出ListView实现局部更新的方法：
``` java
public void updateItemView(ListView listview, int position, Data data){
    int firstPos = listview.getFirstVisiblePosition();
    int lastPos = listview.getLastVisiblePosition();
    if(position >= firstPos && position <= lastPos){  //可见才更新，不可见则在getView()时更新
        //listview.getChildAt(i)获得的是当前可见的第i个item的view
        View view = listview.getChildAt(position - firstPos);
        VH vh = (VH)view.getTag();
        vh.text.setText(data.text);
    }
}
```

可以看出，我们通过ListView的`getChildAt()`来获得需要更新的View，然后通过`getTag()`获得ViewHolder，从而实现更新。


### RecyclerView实现局部刷新
RecyclerView提供了`notifyItemInserted()`,`notifyItemRemoved()`,`notifyItemChanged()`等API更新单个或某个范围的Item视图。






## 缓存机制
ListView与RecyclerView缓存机制原理大致相似，如下图所示：

![image](http://upload-images.jianshu.io/upload_images/9028834-6f4c7d81adc5f0eb..png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

过程中，离屏的ItemView即被回收至缓存，入屏的ItemView则会优先从缓存中获取，只是ListView与RecyclerView的实现细节有差异.（这只是缓存使用的其中一个场景，还有如刷新等）

###  缓存机制对比

#### 1. 层级不同：

RecyclerView比ListView多两级缓存，支持多个离ItemView缓存，支持开发者自定义缓存处理逻辑，支持所有RecyclerView共用同一个RecyclerViewPool(缓存池)。

具体来说： 
ListView(两级缓存)：

![image](http://upload-images.jianshu.io/upload_images/9028834-25b6409391966003..png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

RecyclerView(四级缓存)：

![image](http://upload-images.jianshu.io/upload_images/9028834-1c81bcf1809591ed..jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

ListView和RecyclerView缓存机制基本一致：

1). mActiveViews和mAttachedScrap功能相似，意义在于快速重用屏幕上可见的列表项ItemView，而不需要重新createView和bindView；

2). mScrapView和mCachedViews + mReyclerViewPool功能相似，意义在于缓存离开屏幕的ItemView，目的是让即将进入屏幕的ItemView重用.

3). RecyclerView的优势在于
- mCacheViews的使用，可以做到屏幕外的列表项ItemView进入屏幕内时也无须bindView快速重用；
- mRecyclerPool可以供多个RecyclerView共同使用，在特定场景下，如viewpaper+多个列表页下有优势。

客观来说，RecyclerView在特定场景下对ListView的缓存机制做了补强和完善。

#### 2. 缓存不同：

1). RecyclerView缓存RecyclerView.ViewHolder，抽象可理解为： 
View + ViewHolder(避免每次createView时调用findViewById) + flag(标识状态)； 
2). ListView缓存View。

缓存不同，二者在缓存的使用上也略有差别，具体来说： 
ListView获取缓存的流程：

![image](http://upload-images.jianshu.io/upload_images/9028834-d3ff92d088050694..jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

RecyclerView获取缓存的流程：

![image](http://upload-images.jianshu.io/upload_images/9028834-b88a09a272c4fb14..jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

1). RecyclerView中mCacheViews(屏幕外)获取缓存时，是通过匹配pos获取目标位置的缓存，这样做的好处是，当数据源数据不变的情况下，无须重新bindView：

[![](http://upload-images.jianshu.io/upload_images/9028834-295c958dc9695b7d.gif?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-295c958dc9695b7d.gif?imageMogr2/auto-orient/strip)

而同样是离屏缓存，ListView从mScrapViews根据pos获取相应的缓存，但是并没有直接使用，而是重新getView（即必定会重新bindView），相关代码如下：

``` java
//AbsListView源码：line2345
//通过匹配pos从mScrapView中获取缓存
final View scrapView = mRecycler.getScrapView(position);
//无论是否成功都直接调用getView,导致必定会调用createView
final View child = mAdapter.getView(position, scrapView, this);
if (scrapView != null) {
    if (child != scrapView) {
        mRecycler.addScrapView(scrapView, position);
    } else {
        ...
    }
}
```

2). ListView中通过pos获取的是view，即pos–>view； 
RecyclerView中通过pos获取的是viewholder，即pos –> (view，viewHolder，flag)； 
从流程图中可以看出，标志flag的作用是判断view是否需要重新bindView，这也是RecyclerView实现局部刷新的一个核心.

### 局部刷新

由上文可知，RecyclerView的缓存机制确实更加完善，但还不算质的变化，RecyclerView更大的亮点在于提供了局部刷新的接口，通过局部刷新，就能避免调用许多无用的bindView.

[![](http://upload-images.jianshu.io/upload_images/9028834-379a7ddaaa9a3064.gif?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-379a7ddaaa9a3064.gif?imageMogr2/auto-orient/strip)
(RecyclerView和ListView添加，移除Item效果对比)

结合RecyclerView的缓存机制，看看局部刷新是如何实现的： 
以RecyclerView中notifyItemRemoved(1)为例，最终会调用requestLayout()，使整个RecyclerView重新绘制，过程为： 
onMeasure()–>onLayout()–>onDraw()

其中，onLayout()为重点，分为三步： 
1. dispathLayoutStep1()：记录RecyclerView刷新前列表项ItemView的各种信息，如Top,Left,Bottom,Right，用于动画的相关计算； 
2. dispathLayoutStep2()：真正测量布局大小，位置，核心函数为layoutChildren()； 
3. dispathLayoutStep3()：计算布局前后各个ItemView的状态，如Remove，Add，Move，Update等，如有必要执行相应的动画.

其中，layoutChildren()流程图：

![image](http://upload-images.jianshu.io/upload_images/9028834-4d213954ae9c1025..jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image](http://upload-images.jianshu.io/upload_images/9028834-f49cf82acc7d3ff7..png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

当调用notifyItemRemoved时，会对屏幕内ItemView做预处理，修改ItemView相应的pos以及flag(流程图中红色部分)：

![image](http://upload-images.jianshu.io/upload_images/9028834-82b67697a656af04..png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

当调用fill()中RecyclerView.getViewForPosition(pos)时，RecyclerView通过对pos和flag的预处理，使得bindview只调用一次.

需要指出，ListView和RecyclerView最大的区别在于数据源改变时的缓存的处理逻辑，ListView是”一锅端”，将所有的mActiveViews都移入了二级缓存mScrapViews，而RecyclerView则是更加灵活地对每个View修改标志位，区分是否重新bindView。

### 回收机制源码分析

#### ListView回收机制

ListView为了保证Item View的复用，实现了一套回收机制，该回收机制的实现类是RecycleBin，他实现了两级缓存：

*   `View[] mActiveViews`: 缓存屏幕上的View，在该缓存里的View不需要调用`getView()`。
*   `ArrayList<View>[] mScrapViews;`: 每个Item Type对应一个列表作为回收站，缓存由于滚动而消失的View，此处的View如果被复用，会以参数的形式传给`getView()`。

接下来我们通过源码分析ListView是如何与RecycleBin交互的。其实ListView和RecyclerView的layout过程大同小异，ListView的布局函数是`layoutChildren()`，实现如下：

``` java
void layoutChildren(){
    //1. 如果数据被改变了，则将所有Item View回收至scrapView  
  //（而RecyclerView会根据情况放入Scrap Heap或RecyclePool）；否则回收至mActiveViews
    if (dataChanged) {
        for (int i = 0; i < childCount; i++) {
            recycleBin.addScrapView(getChildAt(i), firstPosition+i);
        }
    } else {
        recycleBin.fillActiveViews(childCount, firstPosition);
    }
    //2. 填充
    switch(){
        case LAYOUT_XXX:
            fillXxx();
            break;
        case LAYOUT_XXX:
            fillXxx();
            break;
    }
    //3. 回收多余的activeView
    mRecycler.scrapActiveViews();
}
```

其中`fillXxx()`实现了对Item View进行填充，该方法内部调用了`makeAndAddView()`，实现如下：

``` java
View makeAndAddView(){
    if (!mDataChanged) {
        child = mRecycler.getActiveView(position);
        if (child != null) {
            return child;
        }
    }
    child = obtainView(position, mIsScrap);
    return child;
}
```

其中，`getActiveView()`是从mActiveViews中获取合适的View，如果获取到了，则直接返回，而不调用`obtainView()`，这也印证了如果从mActiveViews获取到了可复用的View，则不需要调用`getView()`。

`obtainView()`是从mScrapViews中获取合适的View，然后以参数形式传给了`getView()`，实现如下：

``` java
View obtainView(int position){
    final View scrapView = mRecycler.getScrapView(position);  //从RecycleBin中获取复用的View
    final View child = mAdapter.getView(position, scrapView, this);
}
```

接下去我们介绍`getScrapView(position)`的实现，该方法通过position得到Item Type，然后根据Item Type从mScrapViews获取可复用的View，如果获取不到，则返回null，具体实现如下：

``` java
class RecycleBin{
    private View[] mActiveViews;    //存储屏幕上的View
    private ArrayList<View>[] mScrapViews;  //每个item type对应一个ArrayList
    private int mViewTypeCount;            //item type的个数
    private ArrayList<View> mCurrentScrap;  //mScrapViews[0]

    View getScrapView(int position) {
        final int whichScrap = mAdapter.getItemViewType(position);
        if (whichScrap < 0) {
            return null;
        }
        if (mViewTypeCount == 1) {
            return retrieveFromScrap(mCurrentScrap, position);
        } else if (whichScrap < mScrapViews.length) {
            return retrieveFromScrap(mScrapViews[whichScrap], position);
        }
        return null;
    }
    private View retrieveFromScrap(ArrayList<View> scrapViews, int position){
        int size = scrapViews.size();
        if(size > 0){
            return scrapView.remove(scrapViews.size() - 1);  //从回收列表中取出最后一个元素复用
        } else{
            return null;
        }
    }
}
```

#### RecyclerView回收机制

RecyclerView和ListView的回收机制非常相似，但是**ListView是以View作为单位进行回收**，**RecyclerView是以ViewHolder作为单位进行回收**。
Recycler是RecyclerView回收机制的实现类，他实现了四级缓存：

*   mAttachedScrap: 缓存在屏幕上的ViewHolder。
*   mCachedViews: 缓存屏幕外的ViewHolder，默认为2个。ListView对于屏幕外的缓存都会调用`getView()`。
*   mViewCacheExtensions: 需要用户定制，默认不实现。
*   mRecyclerPool: 缓存池，多个RecyclerView共用。

在上文Layout Manager中已经介绍了RecyclerView的layout过程，但是一笔带过了`getViewForPosition()`，因此此处介绍该方法的实现。

``` java
View getViewForPosition(int position, boolean dryRun){
    if(holder == null){
        //从mAttachedScrap,mCachedViews获取ViewHolder
        holder = getScrapViewForPosition(position,INVALID,dryRun); //此处获得的View不需要bind
    }
    final int type = mAdapter.getItemViewType(offsetPosition);
    if (mAdapter.hasStableIds()) { //默认为false
        holder = getScrapViewForId(mAdapter.getItemId(offsetPosition), type, dryRun);
    }
    if(holder == null && mViewCacheExtension != null){
        final View view = mViewCacheExtension.getViewForPositionAndType(this, position, type); //从
        if(view != null){
            holder = getChildViewHolder(view);
        }
    }
    if(holder == null){
        holder = getRecycledViewPool().getRecycledView(type);
    }
    if(holder == null){  //没有缓存，则创建
        holder = mAdapter.createViewHolder(RecyclerView.this, type); //调用onCreateViewHolder()
    }
    if(!holder.isBound() || holder.needsUpdate() || holder.isInvalid()){
        mAdapter.bindViewHolder(holder, offsetPosition);
    }
    return holder.itemView;
}
```

从上述实现可以看出，依次从mAttachedScrap, mCachedViews, mViewCacheExtension, mRecyclerPool寻找可复用的ViewHolder，如果是从mAttachedScrap或mCachedViews中获取的ViewHolder，则不会调用`onBindViewHolder()`，mAttachedScrap和mCachedViews也就是我们所说的Scrap Heap；而如果从mViewCacheExtension或mRecyclerPool中获取的ViewHolder，则会调用`onBindViewHolder()`。

RecyclerView局部刷新的实现原理也是基于RecyclerView的回收机制，即能直接复用的ViewHolder就不调用`onBindViewHolder()`。



## 结论

1.  在一些场景下，如界面初始化，滑动等，ListView和RecyclerView都能很好地工作，两者并没有很大的差异：

2.  数据源频繁更新的场景，如弹幕：[http://www.jianshu.com/p/2232a63442d6](http://www.jianshu.com/p/2232a63442d6)等RecyclerView的优势会非常明显；

进一步来讲，结论是： 
**列表页展示界面，需要支持动画，或者频繁更新，局部刷新，建议使用RecyclerView，更加强大完善，易扩展；其它情况(如微信卡包列表页)两者都OK，但ListView在使用上会更加方便，快捷。**

## 扩展阅读

*   [Google I/O 2016: RecyclerView Ins and Outs](http://v.youku.com/v_show/id_XMTU4MTQ1ODg2NA==.html?f=27314446)
*   [RecyclerView优秀文章集](https://github.com/CymChad/CymChad.github.io)




**引用：**
★★★★[RecyclerView 必知必会](http://blog.csdn.net/tencent_bugly/article/details/54287626#t7)
★★★★[Android ListView 与 RecyclerView 对比浅析–缓存机制](http://blog.csdn.net/tencent_bugly/article/details/52981210)
★★★[RecyclerView使用完全指南，是时候体验新控件了（一）](https://www.jianshu.com/p/4fc6164e4709)
★★★[RecyclerView使用完全指南，是时候体验新控件了（二）](https://www.jianshu.com/p/7c3c549a0ec4)
[一篇博客理解Recyclerview的使用](http://blog.csdn.net/u012124438/article/details/53495951)
[RecyclerView使用全解析](http://www.cnblogs.com/anni-qianqian/p/6587329.html)

**Demo地址：**
- [RecyclerView基本用法](https://link.jianshu.com?t=https://github.com/Kyogirante/MaterialDesignDemo)
- [RecyclerViewDemo](https://github.com/xiazdong/RecyclerViewDemo)
 *   Demo1: RecyclerView添加HeaderView和FooterView，ItemDecoration范例。
 *   Demo2: ListView实现局部刷新。
 *   Demo3: RecyclerView实现拖拽、侧滑删除。
 *   Demo4: RecyclerView闪屏问题。
 *   Demo5: RecyclerView实现setEmptyView()。
 *   Demo6: RecyclerView实现万能适配器，瀑布流布局，嵌套滑动机制。

