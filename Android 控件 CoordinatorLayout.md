<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [【Android 控件 CoordinatorLayout】](#android-%E6%8E%A7%E4%BB%B6-coordinatorlayout)
  - [概述](#%E6%A6%82%E8%BF%B0)
    - [定义](#%E5%AE%9A%E4%B9%89)
    - [交互行为（Behavior初探）](#%E4%BA%A4%E4%BA%92%E8%A1%8C%E4%B8%BAbehavior%E5%88%9D%E6%8E%A2)
    - [深入理解 Behavior](#%E6%B7%B1%E5%85%A5%E7%90%86%E8%A7%A3-behavior)
      - [拦截 Touch 事件](#%E6%8B%A6%E6%88%AA-touch-%E4%BA%8B%E4%BB%B6)
      - [拦截测量及布局](#%E6%8B%A6%E6%88%AA%E6%B5%8B%E9%87%8F%E5%8F%8A%E5%B8%83%E5%B1%80)
      - [View 的依赖关系的确定](#view-%E7%9A%84%E4%BE%9D%E8%B5%96%E5%85%B3%E7%B3%BB%E7%9A%84%E7%A1%AE%E5%AE%9A)
  - [玩转AppBarLayout](#%E7%8E%A9%E8%BD%ACappbarlayout)
    - [基本用法](#%E5%9F%BA%E6%9C%AC%E7%94%A8%E6%B3%95)
    - [layout_scrollFlags](#layout_scrollflags)
      - [scroll](#scroll)
      - [exitUntilCollapsed](#exituntilcollapsed)
      - [enterAlways](#enteralways)
      - [enterAlwaysCollapsed](#enteralwayscollapsed)
      - [snap](#snap)
  - [CollapsingToolBarLayout](#collapsingtoolbarlayout)
- [Behavior进阶](#behavior%E8%BF%9B%E9%98%B6)
  - [BottomSheetBehavior](#bottomsheetbehavior)
    - [① 设置behavior](#%E2%91%A0-%E8%AE%BE%E7%BD%AEbehavior)
    - [② 获取BottomSheetBehavior](#%E2%91%A1-%E8%8E%B7%E5%8F%96bottomsheetbehavior)
    - [③ 通过BottomSheetBehavior控制显示隐藏](#%E2%91%A2-%E9%80%9A%E8%BF%87bottomsheetbehavior%E6%8E%A7%E5%88%B6%E6%98%BE%E7%A4%BA%E9%9A%90%E8%97%8F)
  - [BottomSheetDialog](#bottomsheetdialog)
    - [BottomSheetDialog的神坑](#bottomsheetdialog%E7%9A%84%E7%A5%9E%E5%9D%91)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->



# 【Android 控件 CoordinatorLayout】

## 概述
### 定义
首先我们得知道 CoordinatorLayout 是什么玩意儿，到底有什么用，我们不妨看看官方文档的描述：

> CoordinatorLayout 是一个 “加强版” FrameLayout， 它主要有两个用途：
1.  用作应用的**顶层布局管理器**，也就是作为用户界面中所有 UI 控件的容器;
2.  用作**相互之间具有特定交互行为的 UI 控件的容器**，通过为 CoordinatorLayout 的子 View 指定 Behavior， 就可以实现它们之间的交互行为。
  **Behavior** 可以用来**实现**一系列的**交互行为和布局变化**，比如说侧滑菜单、可滑动删除的 UI 元素，以及跟随着其他 UI 控件移动的按钮等。

其实总结出来就是 CoordinatorLayout 是一个布局管理器，相当于一个增强版的 FrameLayout，但是它神奇在于可以实现它的子 View 之间的交互行为。

### 交互行为（Behavior初探）

先看个简单的效果图
[![](http://upload-images.jianshu.io/upload_images/9028834-154451ea327313e0.gif?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-154451ea327313e0.gif?imageMogr2/auto-orient/strip)

可能大家看到这，就自然能想到观察者模式，或者我前面写的Rx模式：[这可能是最好的RxJava 2.x 教程（完结版）](https://www.jianshu.com/p/0cd258eecf60)

我们的 Button 就是一个被观察者，TextView 作为一个观察者，当 Button 移动的时候通知 TextView， TextView 就跟着移动。看看其布局：

``` xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.CoordinatorLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/activity_coordinator"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="com.nanchen.coordinatorlayoutdemo.CoordinatorActivity">
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="观察者"
        app:layout_behavior=".FollowBehavior"/>
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="被观察者"
        android:layout_gravity="center"
        android:id="@+id/btn"/>
</android.support.design.widget.CoordinatorLayout>
```

很简单，一个 TextView， 一个 Button， 外层用 CoordinatorLayout， 然后给我们的 TextView 加一个自定义的 Behavior 文件。

**自定义 Behavior**：
重写`layoutDependOn`和`onDependentViewChanged`
```java
/* 自定义 CoordinatorLayout 的 Behavior， 泛型为观察者 View ( 要跟着别人动的那个 )
 * 必须重写两个方法，layoutDependOn和onDependentViewChanged */

public class FollowBehavior extends CoordinatorLayout.Behavior<TextView>{
    // 构造方法
    public FollowBehavior(Context context, AttributeSet attrs) {
        super(context, attrs);
    }

    /* 判断child的布局是否依赖 dependency
     * 根据逻辑来判断返回值，返回 false 表示不依赖，返回 true 表示依赖
     *
     * 在一个交互行为中，Dependent View 的变化决定了另一个相关 View 的行为。
     * 在这个例子中， Button 就是 Dependent View，因为 TextView 跟随着它。
     * 实际上 Dependent View 就相当于我们前面介绍的被观察者 */
    @Override
    public boolean layoutDependsOn(CoordinatorLayout parent, TextView child, View dependency) {
        return dependency instanceof Button;
    }

    @Override
    public boolean onDependentViewChanged(CoordinatorLayout parent, TextView child, View dependency) {
        child.setX(dependency.getX());
        child.setY(dependency.getY() + 100);
        return true;
    }
}
```

重点看看其中重写的两个方法 layoutDependsOn() 和 onDependentViewChanged() 。

在介绍这两个方法的作用前，我们先来介绍一下 **Dependent View**。在一个交互行为中，Dependent View 的变化决定了另一个相关 View 的行为。在这个例子中， Button 就是 Dependent View， 因为 TextView 跟随着它。实际上 Dependent View 就**相当于**我们前面介绍的**被观察者**。

知道了这个概念，让我们看看重写的两个方法的作用：
*   **layoutDependsOn()**：这个方法在对界面进行布局时至少会调用一次，用来确定本次交互行为中的 Dependent View，在上面的代码中，当 Dependency 是Button 类的实例时返回 true，就可以让系统知道布局文件中的 Button 就是本次交互行为中的 **Dependent View**。

*   **onDependentViewChanged()**：当 Dependent View 发生变化时，这个方法会被调用，参数中的**child**相当于本次交互行为中的**观察者**，观察者可以在这个方法中对被观察者的变化**做出响应**，从而完成一次交互行为。

所以我们现在可以开始写Activity中的代码：
```java
findViewById(R.id.btn).setOnTouchListener(new OnTouchListener() {
            @Override
            public boolean onTouch(View view, MotionEvent motionEvent) {
                if (motionEvent.getAction() == MotionEvent.ACTION_MOVE){
                    view.setX(motionEvent.getRawX()-view.getWidth()/2);
                    view.setY(motionEvent.getRawY()-view.getHeight()/2);
                }
                return true;
            }
        });
```

这样一来，我们就完成了为 TextView 和Button 设置跟随移动这个交互行为。
其实**为 CoordinatorLayout 的子 View 设置交互**行为只需三步：
- 自定义一个继承自 Behavior 类的交互行为类；

- 把观察者的 layout_behavior 属性设置为自定义行为类的类名；

- 重写 Behavior 类的相关方法来实现我们想要的交互行为。

值得注意的是，有些时候，并不需要我们自己来定义一个 Behavior 类，因为**系统为我们预定义了不少 Behavior 类**。



现在我们已经知道了怎么通过给 CoordinatorLayout 的子 View 设置 Behavior 来实现交互行为。

实际上， CoordinatorLayout 本身并没有做过多工作，实现交互行为的主要幕后推手是 CoordinatorLayout 的内部类—— Behavior。通过为 CoordinatorLayout 的**直接子 View **绑定一个 Behavior ，这个 Behavior 就会拦截发生在这个 View 上的 Touch 事件、嵌套滚动等。不仅如此，Behavior 还能拦截对与它绑定的 View 的测量及布局。

### 深入理解 Behavior

#### 拦截 Touch 事件

当我们为一个 CoordinatorLayout 的直接子 View 设置了 Behavior 时，这个 Behavior 就能拦截发生在这个 View 上的 Touch 事件，那么它是如何做到的呢？
实际上， CoordinatorLayout 重写了 onInterceptTouchEvent() 方法，并在其中给 Behavior 开了个后门，让它能够先于 View 本身处理 Touch 事件。
具体来说， **CoordinatorLayout** 的 **onInterceptTouchEvent**() 方法中会遍历所有直接子 View ，对于**绑定了 Behavior 的直接子 View 调用 Behavior 的 onInterceptTouchEvent() 方法**，若这个方法返回 true， 那么后续本该由相应子 View 处理的 Touch 事件都会交由 Behavior 处理，而 View 本身表示懵逼，完全不知道发生了什么。

#### 拦截测量及布局
CoordinatorLayout 重写了测量及布局相关的方法并为 Behavior 开了个后门。

**CoordinatorLayout** 在 **onMeasure**() 方法中，会遍历所有直接子 View ，若该子 **View 绑定了一个 Behavior** ，就会**调用**相应 **Behavior 的 onMeasureChild**() 方法，若此方法返回 true，那么 CoordinatorLayout 对该子 View 的测量就不会进行。这样一来， Behavior 就成功接管了对 View 的测量。

同样，CoordinatorLayout 在 **onLayout**() 方法中也做了与 onMeasure() 方法中**相似**的事，让 Behavior 能够接管对相关子 View 的布局。

#### View 的依赖关系的确定

现在，我们在探究一下交互行为中的两个 View 之间的依赖关系是怎么确定的。我们称 child 为交互行为中根据另一个 View 的变化做出响应的那个个体，而 Dependent View 为child所依赖的 View。

实际上，确立 child 和 Dependent View 的依赖关系有两种方式：
1.  **显式依赖**：为 child 绑定一个 Behavior，并在 Behavior 类的 layoutDependsOn() 方法中做手脚。即当传入的 dependency 为 Dependent View 时返回 true，这样就建立了 child 和 Dependent View 之间的依赖关系。

2.  **隐式依赖**：通过我们最开始提到的锚（anchor）来确立。具体做法可以这样：在 XML 布局文件中，把 child 的 layout_anchor 属性设为 Dependent View 的id，然后 child 的 layout_anchorGravity 属性用来描述为它想对 Dependent View 的变化做出什么样的响应。关于这个我们会在后续篇章给出具体示例。

无论是隐式依赖还是显式依赖，在 Dependent View 发生变化时，相应 Behavior 类的 onDependentViewChanged() 方法都会被调用，在这个方法中，我们可以让 child 做出改变以响应 Dependent View 的变化。

## 玩转AppBarLayout

实际上我们在应用中有 CoordinatorLayout 的地方通常都会有 AppBarLayout 的联用，作为同样的出自 Design 包的库，我们看看官方文档怎么说：

> AppBarLayout 是一个垂直的 LinearLayout，实现了 Material Design 中 App bar 的 Scrolling Gestures 特性。AppBarLayout 的子 View 应该声明想要具有的“滚动行为”，这可以通过 layout_scrollFlags 属性或是 setScrollFlags() 方法来指定。

> **AppBarLayout 只有作为 CoordinatorLayout 的直接子 View 时才能正常工作**，为了让 AppBarLayout 能够知道何时滚动其子 View，我们**还应该在 CoordinatorLayout 布局中提供一个可滚动 View**，我们称之为 Scrolling View。

> Scrolling View 和 AppBarLayout 之间的**关联**，通过将 **Scrolling View 的 Behavior 设为 AppBarLayout.ScrollingViewBehavior** 来建立。

### 基本用法
AppBar 是 Design 的一个概念，其实我们也可以把它看做一种 5.0 出的 ToolBar，先感受一下 AppBarLayout  +  CoordinatorLayout 的魅力。

[![](http://upload-images.jianshu.io/upload_images/9028834-972afae64ab708db.gif?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-972afae64ab708db.gif?imageMogr2/auto-orient/strip)

实际效果就是这样，当向上滑动 View 的时候，ToolBar 会小时，向下滑动的时候，ToolBar 又会出现，但别忘了，这是 AppBarLayout 的功能，**ToolBar** 可**办不到**。由于要滑动，那么我们的 AppBarLayout 一定是和可以滑动的 View 一起使用的，比如 RecyclerView，NestedScrollView等。

我们看看上面的到底怎么实现的：

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.CoordinatorLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/activity_coor_app_bar"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="com.nanchen.coordinatorlayoutdemo.CoorAppBarActivity">

    <android.support.design.widget.AppBarLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:popupTheme="@style/ThemeOverlay.AppCompat.Light">

        <android.support.v7.widget.Toolbar
            android:id="@+id/toolbar"
            android:layout_width="match_parent"
            android:layout_height="?attr/actionBarSize"
            app:layout_scrollFlags="scroll|enterAlways">
        </android.support.v7.widget.Toolbar>
    </android.support.design.widget.AppBarLayout>

    <android.support.v7.widget.RecyclerView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:id="@+id/recycler"
        app:layout_behavior="@string/appbar_scrolling_view_behavior"/>

</android.support.design.widget.CoordinatorLayout>

```

我们可以看到，上面出现了一个 app:layouy_scrollFrags 的自定义属性设置，这个属性可以定义我们不同的滚动行为。

### layout_scrollFlags

根据官方文档，layout_scrollFlags 的取值可以为以下几种。

#### scroll
设成这个值的效果就好比本 View 和 Scrolling view 是“**一体**”的。具体示例我们在上面已经给出。有一点特别需要我们的注意，为了其他的滚动行为生效，必须同时指定 Scroll 和相应的标记，比如我们想要 exitUntilCollapsed 所表现的滚动行为，必须将 layout_scrollFlags 指定为 scroll|exitUntilCollapsed 。

#### exitUntilCollapsed
当本 View 离开屏幕时，会被“折叠”直到达到其最小高度。我们可以这样理解这个效果：当我们开始向上滚动 Scrolling view 时，**本 View** 会**先**接管**滚动**事件，这样本 View 会先进行滚动，直到滚动到了最小高度（**折叠**了），**Scrolling view 才**开始实际**滚**动。而当本 View 已完全折叠后，再向下滚动 Scrolling view，直到 **Scrolling view** 顶部的内容**完全显示后**，**本 View** 才会开始**向下滚**动以显现出来。

#### enterAlways
当 Scrolling view 向下滚动时，本 View 会**一起**跟着**向下**滚动。实际上就好比我们同时对 Scrolling view 和本 View 进行向下滚动。

#### enterAlwaysCollapsed
从名字上就可以看出，这是在 enterAlways 的基础上，加上了“折叠”的效果。当我们开始**向下滚**动 Scrolling View 时，本 View 会一起跟着**滚动直到达到其“折叠高度”**（即最小高度）。然后当 **Scrolling View 滚动至顶部**内容完全显示后，再向下滚动 Scrolling View，**本 View** 会**继续滚**动到完全显示出来。

#### snap
在一次滚动结束时，本 View 很可能只处于“部分显示”的状态，加上这个标记能够达到“**要么完全隐藏，要么完全显示**”的效果。


## CollapsingToolBarLayout

这个东西，我相信很多博客和技术文章都会把 CollapsingToolBarLayout 和 CoordinatorLayout 放一起讲，这个东西的确很牛。我们同样先看看官方文档介绍：

> CollapsingToolbarLayout 通常用来在布局中包裹一个 Toolbar，以实现具有“折叠效果“”的顶部栏。它**需要是 AppBarLayout 的直接子 View，这样才能发挥出效果**。

CollapsingToolbarLayout包含以下特性：

*   **Collasping title**（可折叠标题）：当布局完全可见时，这个标题比较大；当折叠起来时，标题也会变小。标题的外观可以通过 **expandedTextAppearance** 和 **collapsedTextAppearance** 属性来调整。

*   **Content scrim**（内容纱布）：根据 CollapsingToolbarLayout 是否滚动到一个临界点，内容纱布会显示或隐藏。可以通过 **setContentScrim(Drawable)** 来设置内容纱布。

*   **Status bar scrim**（状态栏纱布）：也是根据是否滚动到临界点，来决定是否显示。可以通过 **setStatusBarScrim(Drawable)** 方法来设置。这个特性只有在 Android 5.0 及其以上版本，我们设置 fitSystemWindows 为 ture 时才能生效。

*   **Parallax scrolling children**（视差滚动子 View）：子 View 可以选择以“视差”的方式来进行滚动。（视觉效果上就是**子 View 滚动的比其他 View 稍微慢**些）

*   **Pinned position children**：**子 View** 可以选择**固定**在某一位置上。

上面的描述有些抽象，实际上对于 Content scrim 、Status bar scrim 我们可以暂时予以忽略，只要留个大概印象待以后需要时再查阅相关资料即可。下面我们通过一个常见的例子介绍下 CollapsingToolbarLayout 的基本使用姿势。

我们来看看一个常用的效果：

[![](http://upload-images.jianshu.io/upload_images/9028834-49fc9147a64734c7.gif?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-49fc9147a64734c7.gif?imageMogr2/auto-orient/strip)

看看布局：

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="cn.rt.rtcoordinatorlayout.CollapsingToolBarActivity">
    <android.support.design.widget.AppBarLayout
        android:id="@+id/appbar"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:theme="@style/AppTheme.AppBarOverlay">
        <android.support.design.widget.CollapsingToolbarLayout
            android:layout_width="match_parent"
            android:layout_height="200dp"
            android:fitsSystemWindows="true"
            app:collapsedTitleGravity="top"
            app:collapsedTitleTextAppearance="@style/TextAppearance.Design.CollapsingToolbar.Expanded"
            app:contentScrim="@color/app_color"
            app:expandedTitleGravity="left|bottom"
            app:expandedTitleMarginStart="100dp"
            app:expandedTitleTextAppearance="@style/TextAppearance.Design.CollapsingToolbar.Expanded"
            app:layout_scrollFlags="scroll|exitUntilCollapsed|snap"
            app:statusBarScrim="@color/green"
            app:title="大大大折叠">
            <!--app:layout_scrollFlags必须是exitUntilCollapsed否则CollapsingToolbarLayout没效果
                app:contentScrim 内容纱布
                app:statusBarScrim 状态栏纱布            -->

            <ImageView
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:fitsSystemWindows="true"
                android:scaleType="centerCrop"
                android:src="@mipmap/ic_launcher"
                app:layout_collapseMode="parallax"
                app:layout_collapseParallaxMultiplier="0.6" />
            <!--app:layout_collapseMode="parallax" 视差滚动           -->

            <android.support.v7.widget.Toolbar
                android:id="@+id/toolbar"
                android:layout_width="match_parent"
                android:layout_height="?attr/actionBarSize"
                android:fitsSystemWindows="true"
                app:layout_collapseMode="pin"
                app:popupTheme="@style/AppTheme.PopupOverlay"
                app:title="折叠" />
            <!--app:layout_collapseMode="pin" 固定           -->
        </android.support.design.widget.CollapsingToolbarLayout>


    </android.support.design.widget.AppBarLayout>

    <android.support.v7.widget.RecyclerView
        android:id="@+id/recycler"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_behavior="@string/appbar_scrolling_view_behavior" />

    <android.support.design.widget.FloatingActionButton
        android:id="@+id/fab"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_margin="@dimen/fab_margin"
        app:layout_anchor="@id/appbar"
        app:layout_anchorGravity="bottom|right"
        app:srcCompat="@android:drawable/ic_dialog_email" />
    <!--app:layout_anchor:意思是FAB浮动按钮显示在哪个布局区域。且设置当前锚点的位置
        app:layout_anchorGravity:意思FAB浮动按钮在这个布局区域的具体位置。
        两个属性共同作用才使的FAB 浮动按钮也能折叠消失，出现。  -->
</android.support.design.widget.CoordinatorLayout>
```

我们在 XML 文件中为 CollapsingToolBarLayout 的 layout_scrollFlags 指定为 scroll|exitUntilCollapsed|snap，这样便实现了向上滚动的折叠效果。

CollapsingToolbarLayout 本质上同样是一个 FrameLayout，我们在布局文件中指定了一个 ImageView 和一个 Toolbar。
ImageView 的layout_collapseMode 属性设为了 parallax，也就是我们前面介绍的视差滚动；
而 Toolbar 的 layout_collaspeMode 设为了 pin ，也就是 Toolbar 会始终固定在顶部。


# Behavior进阶

今天的效果在支付宝、淘宝、京东等电商App中很常见。比如支付宝输入密码弹窗、商城下单时选择商品属性时，从下面浮动上来一个`PopupWindow`，那么今天就带大家用`Behavior`来实现这两个效果，结果你会发现简直只需要一行代码。

总结下现在用的APP： 
1\. 仿支付宝弹出的输入支付密码窗口。 
2\. 仿淘宝/天猫弹出商品属性选择框。 
3\. 知乎首页上下滑动隐藏ToolBar和NavigationBar。 
4\. …

系列博客： 
1. [Material Design系列，Behavior之BottomSheetBehavior与BottomSheetDialog](http://blog.csdn.net/yanzhenjie1003/article/details/51938400) 
2. [Material Design系列，Behavior之SwipeDismissBehavior](http://blog.csdn.net/yanzhenjie1003/article/details/51938425) 
3. [Material Design系列，自定义Behavior之上滑显示返回顶部按钮](http://blog.csdn.net/yanzhenjie1003/article/details/51941288) 
4. [Material Design系列，自定义Behavior实现Android知乎首页](http://blog.csdn.net/yanzhenjie1003/article/details/51946749) 
5. [Material Design系列，自定义Behavior支持所有 View](http://blog.csdn.net/yanzhenjie1003/article/details/52205665)



## BottomSheetBehavior
[![](http://upload-images.jianshu.io/upload_images/9028834-241cbacec0a1b84e.gif?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-241cbacec0a1b84e.gif?imageMogr2/auto-orient/strip)

### ① 设置behavior
- **控制**层设为**appbar_scrolling_view_behavior**
```xml
    <LinearLayout
        ...
        app:layout_behavior="@string/appbar_scrolling_view_behavior">
        <Button
            ...
            android:text="sheet 显示/隐藏" />
    </LinearLayout>
```

- **被控制**层设为**bottom_sheet_behavior**
```xml
    <LinearLayout
        ...
        app:layout_behavior="@string/bottom_sheet_behavior">
        <Button
            ...
            android:text="第一" />
        <Button
            ...
            android:text="第二" />
        <Button
            ...
            android:text="第三" />
        <Button
            ...
            android:text="第四" />
    </LinearLayout>
```

首先`Behavior`作为`CoordinatorLayout`的子View的`LayoutParams`（原因看后文解释），所以`CoordinatorLayout`是万万不能少的，先来亮出整个布局：

```xml
<android.support.design.widget.CoordinatorLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <android.support.design.widget.AppBarLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:theme="@style/AppTheme.AppBarOverlay">
        <android.support.v7.widget.Toolbar
            android:id="@+id/toolbar"
            android:layout_width="match_parent"
            android:layout_height="?attr/actionBarSize"
            app:popupTheme="@style/AppTheme.PopupOverlay" />
    </android.support.design.widget.AppBarLayout>

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:layout_above="@+id/tab_layout"
        android:gravity="center"
        android:orientation="vertical"
        app:layout_behavior="@string/appbar_scrolling_view_behavior">
        <!--app:layout_behavior="@string/appbar_scrolling_view_behavior"-->        
        <Button
            android:id="@+id/btn_bottom_sheet_control"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="sheet 显示/隐藏" />
    </LinearLayout>

    <LinearLayout
        android:id="@+id/tab_layout"
        android:layout_width="match_parent"
        android:layout_height="?actionBarSize"
        android:layout_alignParentBottom="true"
        android:background="@android:color/holo_purple"
        app:layout_behavior="@string/bottom_sheet_behavior">
        <!--app:layout_behavior="@string/bottom_sheet_behavior"-->
        <Button
            android:layout_width="0dp"
            android:layout_height="match_parent"
            android:layout_weight="1"
            android:text="第一" />
        <Button
            android:layout_width="0dp"
            android:layout_height="match_parent"
            android:layout_weight="1"
            android:text="第二" />
        <Button
            android:layout_width="0dp"
            android:layout_height="match_parent"
            android:layout_weight="1"
            android:text="第三" />
        <Button
            android:layout_width="0dp"
            android:layout_height="match_parent"
            android:layout_weight="1"
            android:text="第四" />
    </LinearLayout>
</android.support.design.widget.CoordinatorLayout>
```

大概介绍下，页面上只能看到Toolbar和一个Button：sheet 显示/隐藏。
然后android:id="@+id/tab_layout"这个布局是横向的，给它设置了Behavior：app:layout_behavior="@string/bottom_sheet_behavior"，经过测试发现，如果不给tab_layout设置BottomSheetBehavior，它会浮动在整个页面的顶部，并在Toolbar的下面。设置了BottomSheetBehavior它会被BottomSheetBehavior自动移动到页面底部外边，所以在页面上是看不到android:id="@+id/tab_layout"这个布局的。

### ② 获取BottomSheetBehavior
BottomSheetBehavior这个货有一个静态方法**`BottomSheetBehavior.from(View)`**，会返回这个View引用的BottomSheetBehavior：
```java
private BottomSheetBehavior mBottomSheetBehavior;

@Override
protected void onCreate(@Nullable Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.bsbehavior_activity);
    Toolbar toolbar = (Toolbar) findViewById(R.id.toolbar);
    setSupportActionBar(toolbar);
    getSupportActionBar().setDisplayHomeAsUpEnabled(true);

    findViewById(R.id.btn_bottom_sheet_control).setOnClickListener(onClickListener);
    // 拿到这个tab_layout对应的BottomSheetBehavior
    mBottomSheetBehavior = BottomSheetBehavior.from(findViewById(R.id.tab_layout));
}
```

**BottomSheetBehavior.from(View)** 源码：
```java
public static <V extends View> BottomSheetBehavior<V> from(V view) {
    ViewGroup.LayoutParams params = view.getLayoutParams();
    if (!(params instanceof CoordinatorLayout.LayoutParams)) {
        throw new Exception("The view is not a child of CoordinatorLayout");
    }
    CoordinatorLayout.Behavior behavior = params.getBehavior();
    if (!(behavior instanceof BottomSheetBehavior)) {
        throw new IllegalArgumentException("...");
    }
    return (BottomSheetBehavior<V>) behavior;
}
```
这个方法会检查这个View是否是CoordinatorLayout的子View，如果是才会去拿到这个View的Behavior，所以诸位也应该明白为什么我开头说Behavior作为CoordinatorLayout子View的LayoutParams了。

### ③ 通过BottomSheetBehavior控制显示隐藏
- **`BottomSheetBehavior.getState()`**   获取状态
  它的返回值有以下几种：
```java
public static final int STATE_DRAGGING = 1;
public static final int STATE_SETTLING = 2;
public static final int STATE_EXPANDED = 3;
public static final int STATE_COLLAPSED = 4;
public static final int STATE_HIDDEN = 5;
```
- **`BottomSheetBehavior.setState`** 设置状态

```java
        if (mBottomSheetBehavior.getState() == BottomSheetBehavior.STATE_EXPANDED) {
            mBottomSheetBehavior.setState(BottomSheetBehavior.STATE_COLLAPSED);
        } else if (mBottomSheetBehavior.getState() == BottomSheetBehavior.STATE_COLLAPSED) {
            mBottomSheetBehavior.setState(BottomSheetBehavior.STATE_EXPANDED);
        }
```



## BottomSheetDialog
[![](http://upload-images.jianshu.io/upload_images/9028834-ecae19e6347d996f.gif?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-ecae19e6347d996f.gif?imageMogr2/auto-orient/strip)

new一个`BottomSheetDialog`：

```java
private BottomSheetDialog mBottomSheetDialog;

@Override
protected void onCreate(@Nullable Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    ...
    createBottomSheetDialog();
}

private void createBottomSheetDialog() {
    mBottomSheetDialog = new BottomSheetDialog(this);
    View view = LayoutInflater.from(this).inflate(R.layout.dialog_bottom_sheet, null, false);
    mBottomSheetDialog.setContentView(view);

    RecyclerView recyclerView = (RecyclerView) view.findViewById(R.id.recyclerView);
    ...
    recyclerView.setAdapter(adapter);
}
```

View里面是一个RecyclerView：

```xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.v7.widget.RecyclerView xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/recyclerView"
    android:layout_width="match_parent"
    android:layout_height="match_parent" />
```

然后用下面的代码去控制它的显示和隐藏。
- **`BottomSheetDialog.dismiss()`**
- **`BottomSheetDialog.show()`**

```java
if (mBottomSheetDialog.isShowing()) {
    mBottomSheetDialog.dismiss();
} else {
    mBottomSheetDialog.show();
}
```

当这个Dialog Show出来的时候发现它显示了一半，嗯这个效果确实不错，这样就达到了我们最初说的支付宝密码弹窗和淘宝/天猫商品属性选择。
我们滑动的时候如果下面有内容它就会`EXPANDED`，如果是一个**普通的View**（非RecyclerView、NestedScrollView）将**不会继续往上滑动**，下面的内容会继续跟着出来，但是同样可以向下滑动隐藏，也可以调用`dismiss`和`close`关闭。

### BottomSheetDialog的神坑
当这个Dilaog打开再关闭后，无法用`Dialog.show()`再次打开，为什么呢？

我去阅读了一下`BottomSheetDialog`源代码，发现了如下代码：

```
 @Override
public void setContentView(View view, ViewGroup.LayoutParams params) {
    super.setContentView(wrapInBottomSheet(0, view, params));
}

private View wrapInBottomSheet(int layoutResId, View view, ViewGroup.LayoutParams params) {
    final CoordinatorLayout coordinator = View.inflate(getContext(),R.layout...., null);
    FrameLayout bottomSheet = (FrameLayout) coordinator.findViewById(R.id.design_bottom_sheet);
    BottomSheetBehavior.from(bottomSheet).setBottomSheetCallback(mBottomSheetCallback);
    ...
    return coordinator;
}

private BottomSheetCallback mBottomSheetCallback = new BottomSheetCallback() {
    @Override
    public void onStateChanged(@NonNull View bottomSheet, int newState) {
        if (newState == BottomSheetBehavior.STATE_HIDDEN) {
            dismiss();
        }
    }

    @Override
    public void onSlide(@NonNull View bottomSheet, float slideOffset) {
    }
};
```

也就是说，系统的`BottomSheetDialog`是基于`BottomSheetBehavior`封装的，这里判断了当我们滑动隐藏了`BottomSheetBehavior`中的View后，内部是设置了`BottomSheetBehavior`的状态为`STATE_HIDDEN`，接着它替我们关闭了`Dialog`，所以我们再次调用`dialog.show()`的时候`Dialog`没法再此打开状态为HIDE的dialog了。

这里就有个疑问了： 
Google为啥没有提供我们自己设置`BottomSheetCallback`的接口呢？

没有关系，看了源码发现很简单，我们自己来实现，并且在监听到用户滑动关闭`BottomSheetDialog`后，我们把`BottomSheetBehavior`的状态设置为`BottomSheetBehavior.STATE_COLLAPSED`，也就是半个打开状态（`BottomSheetBehavior.STATE_EXPANDED`为全打开），根据源码我把设置的方法提供下：

```
private void setBehaviorCallback() {
    View view = mBottomSheetDialog.getDelegate().findViewById(android.support.design.R.id.design_bottom_sheet);
    final BottomSheetBehavior bottomSheetBehavior = BottomSheetBehavior.from(view);
    bottomSheetBehavior.setBottomSheetCallback(new BottomSheetBehavior.BottomSheetCallback() {
        @Override
        public void onStateChanged(@NonNull View bottomSheet, int newState) {
            if (newState == BottomSheetBehavior.STATE_HIDDEN) {
                mBottomSheetDialog.dismiss();
                bottomSheetBehavior.setState(BottomSheetBehavior.STATE_COLLAPSED);
            }
        }
        @Override
        public void onSlide(@NonNull View bottomSheet, float slideOffset) {
        }
    });
}
```

这样就解决了`BottomSheetDialog`关闭后不能再次打开的问题了。

源码下载：[http://download.csdn.net/detail/yanzhenjie1003/9578770](http://download.csdn.net/detail/yanzhenjie1003/9578770)



**引用：**
[一文彻底搞懂 Design 设计的 CoordinatorLayout 和 AppbarLayout 联动，让 Design 设计更简单~](https://www.jianshu.com/p/640f4ef05fb2)
[Material Design系列，Behavior之BottomSheetBehavior与BottomSheetDialog](http://blog.csdn.net/yanzhenjie1003/article/details/51938425)

**源码：**
[基本使用](https://github.com/nanchen2251/CoordinatorAppBarDemo)




