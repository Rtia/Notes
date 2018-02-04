

# 【Android 自定义View】

[TOC]

# 自定义View基础

接触到一个类，你不太了解他，如果贸然翻阅源码只会让你失去方向，不知从哪里下手；所以我们应该从文档着手，看看它是个什么东西，里面有哪些属性和方法，都是用来干嘛的。下面我们看看官方文档对View的介绍：

> View这个类代表用户界面组件的基本构建块。View在屏幕上占据一个矩形区域，并负责绘制和事件处理。View是用于创建交互式用户界面组件（按钮、文本等）的基础类。它的子类ViewGroup是所有布局的父类，它是一个可以包含其他view或者viewGroup并定义它们的布局属性的看不见的容器。 
> 实现一个自定义View，你通常会覆盖一些framework层在所有view上调用的标准方法。你不需要重写所有这些方法。事实上，你可以只是重写**onDraw**(android.graphics.Canvas)。

| 分类        | 方法                                       | 说明                                       |
| :-------- | :--------------------------------------- | :--------------------------------------- |
| 构造        | **Constructors**                         | 两种形式：1.使用代码创建时构造方法被调用，2.View被布局文件填充时构造方法被调用。<br>第二种形式应该解析和使用一些定义在布局文件中属性 |
|           | **onFinishInflate**()                    | 当View和他的所有子控件被XML布局文件填充完成时被调用。（这个方法里面可以完成一些**初始化**，比如初始化子控件） |
| 布局        | **onMeasure**(int, int)                  | 当决定view和他的孩子的**尺寸**需求时被调用（也就是测量控件大小时调用）  |
|           | **onLayout**(boolean, int, int, int, int) | 当View给他的孩子**分配大小和位置**的时候调用（摆放子控件）        |
|           | **onSizeChanged**(int, int, int, int)    | 当view**大小**发生**变化**时调用                   |
| 绘制        | **onDraw**(android.graphics.Canvas)      | 当视图应该呈现其内容时调用（绘制）                        |
| 事件处理      | **onKeyDown**(int, KeyEvent)             | 按键时被调用                                   |
|           | **onKeyUp**(int, KeyEvent)               | 按键被抬起时调用                                 |
|           | **onTrackballEvent**(MotionEvent)        | Called when a trackball motion event occurs. |
|           | **onTouchEvent**(MotionEvent)            | 触摸屏幕时调用                                  |
| 焦点        | **onFocusChanged**(boolean, int, android.graphics.Rect) | 获取到或者失去焦点是调用                             |
|           | **onWindowFocusChanged**(boolean)        | 窗口获取或者失去焦点是调用                            |
| Attaching | **onAttachedToWindow**()                 | 当视图被连接到一个窗口时调用                           |
|           | **onDetachedFromWindow**()               | 当视图从窗口分离时调用                              |
|           | **onWindowVisibilityChanged**(int)       | 当View的窗口的可见性发生改变时调用                      |

　　从上面官方文档介绍我们可以知道，View是所有控件（包括ViewGroup）的父类，它里面有一些常见的方法（上表），如果我们要自定义View，最简单的只需要重写onDraw(android.graphics.Canvas)即可。 


# 自定义TextView

## 1. 继承View，重写onDraw方法

　　创建一个类MyTextView继承View，发现报错，因为要覆盖他的构造方法（因为**View中没有参数为空的构造方法**）。
　　View有四种形式的构造方法，其中四个参数的构造方法是API 21才出现，所以一般我们只需要重写其他三个构造方法即可。

### View的构造方法

它们的参数不一样分别对应不同的创建方式。

1. 只有一个Context参数的构造方法：通常是通过代码初始化控件时使用；
2. 两个参数的构造方法：通常对应布局文件中控件被映射成对象时调用（需要解析属性）；
3. 通常我们让这两个构造方法最终调用三个参数的构造方法，然后在第三个构造方法中进行一些初始化操作。

```java
public class MyView extends View {
    // 需要绘制的文字
    private String mText;
    // 文本的颜色
    private int mTextColor;
    // 文本的大小
    private int mTextSize;
    // 绘制时控制文本绘制的范围
    private Rect mBound;
    private Paint mPaint;

    public MyTextView(Context context) {
        this(context, null);
    }
    public MyTextView(Context context, AttributeSet attrs) {
        this(context, attrs, 0);
    }
    public MyTextView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
 //初始化
        mText = "Udf32fA";
        mTextColor = Color.BLACK;
        mTextSize = 100;

        mPaint = new Paint();
        mPaint.setTextSize(mTextSize);
        mPaint.setColor(mTextColor);
        //获得绘制文本的宽和高
        mBound = new Rect();
        mPaint.getTextBounds(mText, 0, mText.length(), mBound);
    }
    //API21
//    public MyTextView(Context context, AttributeSet attrs, int defStyleAttr, int defStyleRes) {
//        super(context, attrs, defStyleAttr, defStyleRes);
//        init();
//    }

    @Override
    protected void onDraw(Canvas canvas) {
        //绘制文字
        canvas.drawText(mText, getWidth() / 2 - mBound.width() / 2, getHeight() / 2 + mBound.height() / 2, mPaint);
    }
}
```

布局文件：

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

        <view.openxu.com.mytextview.MyTextView
            android:layout_width="200dip"
            android:layout_height="100dip"
            android:textSize="20sp"
            android:text="按钮"
            android:background="#ff0000"/>

</LinearLayout>
```

运行结果： 
![](http://img.blog.csdn.net/20180109164844927?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

　　上面我只是重写了一个onDraw方法，文本已经绘制出来，说明到此为止这个自定义控件已经算成功了。可是发现了一个问题，如果我要绘制另外的文本呢？比如写i love you，那是不是又得重新定义一个自定义控件？跟上面一样，只是需要修改mText就可以了；行，再写一遍，那如果我现在又想改变文字颜色为蓝色呢？在写一遍？这时候就用到了新的知识点：自定义属性

## 2. 自定义属性

### 创建attrs.xml文件

　　在**res/values/**下创建一个名为**attrs.xml**的文件，然后定义如下属性： 
format的意思是该属性的取值是什么类型（支持的类型有string,color,demension,integer,enum,reference,float,boolean,fraction,flag）

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <attr name="mText" format="string" />
    <attr name="mTextColor" format="color" />
    <attr name="mTextSize" format="dimension" />
    <declare-styleable name="MyTextView">
        <attr name="mText"/>
        <attr name="mTextColor"/>
        <attr name="mTextSize"/>
    </declare-styleable>
</resources>
```

### 在构造方法中获取自定义属性的值：

```java
public MyTextView(Context context, AttributeSet attrs, int defStyleAttr) {
    super(context, attrs, defStyleAttr);
    //获取自定义属性的值
    TypedArray a = context.getTheme().obtainStyledAttributes(attrs, R.styleable.MyTextView, defStyleAttr, 0);
    mText = a.getString(R.styleable.MyTextView_mText);
    mTextColor = a.getColor(R.styleable.MyTextView_mTextColor, Color.BLACK);
    mTextSize = a.getDimension(R.styleable.MyTextView_mTextSize, 100);
    a.recycle();  //注意回收
    mPaint = new Paint();
    mPaint.setTextSize(mTextSize);
    mPaint.setColor(mTextColor);
    //获得绘制文本的宽和高
    mBound = new Rect();
    mPaint.getTextBounds(mText, 0, mText.length(), mBound);
}
```

然后在**布局文件中使用自定义属性**，记住一定要引入我们的命名空间`xmlns:openxu="http://schemas.android.com/apk/res-auto"`

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:openxu="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

        <view.openxu.com.mytextview.MyTextView
            android:layout_width="200dip"
            android:layout_height="100dip"
            openxu:mTextSize="25sp"
            openxu:mText="i love you"
            openxu:mTextColor ="#0000ff"
            android:background="#ff0000"/>

</LinearLayout>
```

运行结果： 
![1](http://img.blog.csdn.net/20180109165521122?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

　　通过运行结果，我们已经成功为MyTextView定义了属性，并获取到值，至于自定义属性的详细知识点到后面再介绍。 
到此为止，发现自定义控件还是比较简单的嘛。看看结果，跟原生的TextView还有什么差别？接下来做一点小变化： 
让绘制的文本长一点`openxu:mText="i love you i love you i love you"`，运行结果： 

![2](http://img.blog.csdn.net/20180109165513201?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
　　有没有发现不和谐的现象？文本超度超出了控件边界，控件太小，不足以显示辣么长的文本，我们将宽高改为`wrap_content`试试： 

![3](http://img.blog.csdn.net/20180109165503881?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
　　什么鬼？不是包裹内容吗？怎么填充整个屏幕了？根据顶部官方文档的说明，我们猜想肯定是控件的测量onMeasure方法出了问题，接下来我们学习onMeasure方法。

## 3. onMeasure方法

### **① MeasureSpec**

　　在学习onMasure方法之前，我们要先了解他的参数中的一个类MeasureSpec。 
跟踪一下源码，发现它是View中的一个静态内部类，是由**尺寸和模式组合**而成的一个值，用来描述**父控件对子控件尺寸的约束**，看看他的部分源码，一共有三种模式，然后提供了合成和分解的方法： 

```java
/**
 * measurespec封装了父控件对他的孩子的布局要求。
 * 一个measurespec由大小和模式。有三种可能的模式：
 */
public static class MeasureSpec {
    private static final int MODE_SHIFT = 30;
    private static final int MODE_MASK  = 0x3 << MODE_SHIFT;

    //父控件不强加任何约束给子控件，它可以是它想要任何大小。
    public static final int UNSPECIFIED = 0 << MODE_SHIFT;  //0

    //父控件决定给孩子一个精确的尺寸
    public static final int EXACTLY     = 1 << MODE_SHIFT;  //1073741824

    //父控件会给子控件尽可能大的尺寸
    public static final int AT_MOST     = 2 << MODE_SHIFT;   //-2147483648
    
    // 根据给定的尺寸和模式创建一个约束规范
    public static int makeMeasureSpec(int size, int mode) {
        if (sUseZeroUnspecifiedMeasureSpec && mode == UNSPECIFIED) {
            return 0;
        }
        return makeMeasureSpec(size, mode);
    }
    
    // 从约束规范中获取模式
    public static int getMode(int measureSpec) {
        return (measureSpec & MODE_MASK);
    }
    
    // 从约束规范中获取尺寸
    public static int getSize(int measureSpec) {
        return (measureSpec & ~MODE_MASK);
    }
}
```

　　这样说起来还是有点抽象，举一个小栗子大家就知道这三种约束到底是什么意思。我们自定义一个View，为了方便起见，让它继承Button，布局文件中设置不同的宽高条件，然后在onMeasure方法中打印一下他的参数（int widthMeasureSpec, int heightMeasureSpec）到底是个什么鬼

```java
/**
 * Created by openXu on 16/5/19.
 */
public class MyView extends Button {
    public MyView(Context context) {
        this(context, null);
    }
    public MyView(Context context, AttributeSet attrs) {
        this(context, attrs, 0);
    }
    public MyView(Context context, AttributeSet attrs, int defStyle) {
        super(context, attrs, defStyle);
    }
    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
        int widthMode = MeasureSpec.getMode(widthMeasureSpec);   //获取宽的模式
        int heightMode = MeasureSpec.getMode(heightMeasureSpec); //获取高的模式
        int widthSize = MeasureSpec.getSize(widthMeasureSpec);   //获取宽的尺寸
        int heightSize = MeasureSpec.getSize(heightMeasureSpec); //获取高的尺寸
        Log.v("openxu", "宽的模式:"+widthMode);
        Log.v("openxu", "高的模式:"+heightMode);
        Log.v("openxu", "宽的尺寸:"+widthSize);
        Log.v("openxu", "高的尺寸:"+heightSize);
    }
}
```

**情形1，让按钮包裹内容:**

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
        <com.example.openxu.myview.MyView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textSize="20sp"
            android:text="按钮"
            android:background="#ff0000"/>
</LinearLayout>
```

log打印：

```
05-19 06:02:55.112: V/openxu(15599): 宽的模式:-2147483648 
05-19 06:02:55.112: V/openxu(15599): 高的模式:-2147483648 
05-19 06:02:55.112: V/openxu(15599): 宽的尺寸:1080 
05-19 06:02:55.112: V/openxu(15599): 高的尺寸:1860
```

**情形2，让按钮填充父窗体:**

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
        <com.example.openxu.myview.MyView
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:textSize="20sp"
            android:text="按钮"
            android:background="#ff0000"/>
</LinearLayout>
```

log打印：

```
05-19 06:05:37.302: V/openxu(15960): 宽的模式:1073741824 
05-19 06:05:37.302: V/openxu(15960): 高的模式:1073741824 
05-19 06:05:37.302: V/openxu(15960): 宽的尺寸:1080 
05-19 06:05:37.302: V/openxu(15960): 高的尺寸:1860
```

**情形3，给按钮的宽设置为具体的值:**

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
        <com.example.openxu.myview.MyView
            android:layout_width="100dip"
            android:layout_height="wrap_content"
            android:textSize="20sp"
            android:text="按钮"
            android:background="#ff0000"/>
</LinearLayout>
```

log打印： 

```
05-19 06:07:48.932: V/openxu(16105): 宽的模式:1073741824 
05-19 06:07:48.932: V/openxu(16105): 高的模式:-2147483648 
05-19 06:07:48.932: V/openxu(16105): 宽的尺寸:300 
05-19 06:07:48.932: V/openxu(16105): 高的尺寸:1860
```

　　根据上面的测试，我们发现，约束中分离出来的尺寸就是父控件剩余的宽高大小（除了设置具体的宽高值外）；而几种约束中的模式不就是对应我们在布局文件中设置给按钮的几种情况吗？如下：

| 约束                   |        布局参数        | 输出值         | 说明                                       |
| :------------------- | :----------------: | ----------- | :--------------------------------------- |
| **UNSPECIFIED**(未指定) |                    | 0           | 父控件没有对子控件施加任何约束，子控件可以得到任意想要的大小。但是布局文件好像必须设置宽高，目前还没找到与之对应的布局参数，使用较少 |
| **EXACTLY**(指定)      | match_parent/具体宽高值 | 1073741824  | 父控件给子控件决定了确切大小，子控件将被限定在给定的边界里而忽略它本身大小。特别说明如果是填充父窗体，说明父控件已经明确知道子控件想要多大的尺寸了（就是剩余的空间都要了） |
| **AT_MOST**(至多)      |    wrap_content    | -2147483648 | 子控件至多达到指定大小的值。包裹内容就是父窗体并不知道子控件到底需要多大尺寸（具体值），需要子控件自己测量之后再让父控件给他一个尽可能大的尺寸以便让内容全部显示但不能超过包裹内容的大小 |

### **② 分析为什么我们自定义的MyTextView设置了wrap_content却填充屏幕**

　　通过上面对MeasureSpec的了解，我们现在就有能看懂View的onMeasure方法默认是怎样为控件测量大小的了 
看View中onMeasure的源码：

```java
protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
    setMeasuredDimension(getDefaultSize(getSuggestedMinimumWidth(), widthMeasureSpec),
            getDefaultSize(getSuggestedMinimumHeight(), heightMeasureSpec));
}

public static int getDefaultSize(int size, int measureSpec) {
    int result = size;
    int specMode = MeasureSpec.getMode(measureSpec);
    int specSize = MeasureSpec.getSize(measureSpec);

    switch (specMode) {
    case MeasureSpec.UNSPECIFIED:
        result = size;
        break;
    case MeasureSpec.AT_MOST:
    case MeasureSpec.EXACTLY:
        result = specSize;
        break;
    }
    return result;
}
```

　　 onMeasure方法调用了setMeasuredDimension(int measuredWidth, int measuredHeight)方法，而传入的参数已经是测量过的默认宽和高的值了；我们看看getDefaultSize 方法是怎么计算测量宽高的。根据父控件给予的约束，发现AT_MOST （相当于wrap_content ）和EXACTLY （相当于match_parent ）两种情况返回的测量宽高都是specSize，而这个specSize正是我们上面说的父控件剩余的宽高，所以默认onMeasure方法中wrap_content 和match_parent 的效果是一样的，都是填充剩余的空间。

### **③ 重写onMeasure方法**

　　我们先忽略掉UNSPECIFIED 的情况（使用极少），只考虑AT_MOST 和EXACTLY ，现在的问题是设置wrap_content 时，控件却使用了match_parent 的效果，看下面怎么重写onMeasure： 

```java
@Override
protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
    super.onMeasure(widthMeasureSpec, heightMeasureSpec);
    int widthMode = MeasureSpec.getMode(widthMeasureSpec);   //获取宽的模式
    int heightMode = MeasureSpec.getMode(heightMeasureSpec); //获取高的模式
    int widthSize = MeasureSpec.getSize(widthMeasureSpec);   //获取宽的尺寸
    int heightSize = MeasureSpec.getSize(heightMeasureSpec); //获取高的尺寸
    Log.v("openxu", "宽的模式:"+widthMode);
    Log.v("openxu", "高的模式:"+heightMode);
    Log.v("openxu", "宽的尺寸:"+widthSize);
    Log.v("openxu", "高的尺寸:"+heightSize);
    int width;
    int height ;
    if (widthMode == MeasureSpec.EXACTLY) {
        //如果match_parent或者具体的值，直接赋值
        width = widthSize;
    } else {
        //如果是wrap_content，我们要得到控件需要多大的尺寸
        float textWidth = mBound.width();   //文本的宽度
        //控件的宽度就是文本的宽度加上两边的内边距。内边距就是padding值，在构造方法执行完就被赋值
        width = (int) (getPaddingLeft() + textWidth + getPaddingRight());
        Log.v("openxu", "文本的宽度:"+textWidth + "控件的宽度："+width);
    }
    //高度跟宽度处理方式一样
    if (heightMode == MeasureSpec.EXACTLY) {
        height = heightSize;
    } else {
        float textHeight = mBound.height();
        height = (int) (getPaddingTop() + textHeight + getPaddingBottom());
        Log.v("openxu", "文本的高度:"+textHeight + "控件的高度："+height);
    }
    //保存测量宽度和测量高度
    setMeasuredDimension(width, height);
}
```

布局文件：

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:openxu="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
        <view.openxu.com.mytextview.MyTextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:padding="20dip"
            openxu:mTextSize="25sp"
            openxu:mText="i love you i love you i love you"
            openxu:mTextColor ="#0000ff"
            android:background="#ff0000"/>
</LinearLayout>
```

下面是输出的log：

```
05-19 01:29:12.662 27380-27380/view.openxu.com.mytextview V/openxu: 宽的模式:-2147483648 
05-19 01:29:12.662 27380-27380/view.openxu.com.mytextview V/openxu: 高的模式:-2147483648 
05-19 01:29:12.662 27380-27380/view.openxu.com.mytextview V/openxu: 宽的尺寸:720 
05-19 01:29:12.666 27380-27380/view.openxu.com.mytextview V/openxu: 高的尺寸:1230 
05-19 01:29:12.678 27380-27380/view.openxu.com.mytextview V/openxu: 文本的宽度:652.0控件的宽度：732 
05-19 01:29:12.690 27380-27380/view.openxu.com.mytextview V/openxu: 文本的高度:49.0控件的高度：129
```

　　我的模拟器是720x1280的，根据log显示，文本的宽度是652，加上两边的内边距，控件的宽度为732，确实实现了包裹内容的效果，运行程序结果如下： 
![4](http://img.blog.csdn.net/20180109165453766?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

　　但是发现宽度已经超出了屏幕，还不能像TextView一样换行；下面我们简单的模拟一下换行的功能，做的不够好，但有这个效果，不是重点，不需要重点掌握

## 4. 自动换行

　　只需要在测量的时候，根据文字的总长度和控件的宽度，就可以知道需要绘制几行，然后将文本分割成小段放入集合中，在onDraw方法中分别绘制； 
　　需要注意的是，onMeasure方法不只调用一次，所以在分段文本是需要判断，不要重复分段，否则会报错。代码如下（仅供参考）：

```java
public class MyTextView extends View {

    // 需要绘制的文字/
    private String mText;
    private ArrayList<String> mTextList;
    // 文本的颜色/
    private int mTextColor;
    // 文本的大小/
    private float mTextSize;

    // 绘制时控制文本绘制的范围
    private Rect mBound;
    private Paint mPaint;

    public MyTextView(Context context) {
        this(context, null);
    }
    public MyTextView(Context context, AttributeSet attrs) {
        this(context, attrs, 0);
    }
    public MyTextView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        mTextList = new ArrayList<String>();
        //获取自定义属性的值
        TypedArray a = context.getTheme().obtainStyledAttributes(attrs, R.styleable.MyTextView, defStyleAttr, 0);
        mText = a.getString(R.styleable.MyTextView_mText);
        mTextColor = a.getColor(R.styleable.MyTextView_mTextColor, Color.BLACK);
        mTextSize = a.getDimension(R.styleable.MyTextView_mTextSize, 100);
        a.recycle();  //注意回收
        Log.v("openxu", "文本总长度:"+mText);
        mPaint = new Paint();
        mPaint.setTextSize(mTextSize);
        mPaint.setColor(mTextColor);
        //获得绘制文本的宽和高
        mBound = new Rect();
        mPaint.getTextBounds(mText, 0, mText.length(), mBound);
    }
    //API21
//    public MyTextView(Context context, AttributeSet attrs, int defStyleAttr, int defStyleRes) {
//        super(context, attrs, defStyleAttr, defStyleRes);
//        init();
//    }

    @Override
    protected void onDraw(Canvas canvas) {
        //绘制文字
        for(int i = 0; i<mTextList.size(); i++){
            mPaint.getTextBounds(mTextList.get(i), 0, mTextList.get(i).length(), mBound);
            
            Log.v("openxu", "mBound.h:"+mBound.height());
            Log.v("openxu", "在X:" + (getWidth() / 2 - mBound.width() / 2)+"  Y:"+(getPaddingTop() + (mBound.height() *i))+"  绘制："+mTextList.get(i));
            
            canvas.drawText(mTextList.get(i), (getWidth() / 2 - mBound.width() / 2), (getPaddingTop() + (mBound.height() *i)), mPaint);
        }
    }

    boolean isOneLines = true;
    float lineNum;
    float spLineNum;
    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
        int widthMode = MeasureSpec.getMode(widthMeasureSpec);   //获取宽的模式
        int heightMode = MeasureSpec.getMode(heightMeasureSpec); //获取高的模式
        int widthSize = MeasureSpec.getSize(widthMeasureSpec);   //获取宽的尺寸
        int heightSize = MeasureSpec.getSize(heightMeasureSpec); //获取高的尺寸
        Log.v("openxu", "宽的模式:"+widthMode);
        Log.v("openxu", "高的模式:"+heightMode);
        Log.v("openxu", "宽的尺寸:"+widthSize);
        Log.v("openxu", "高的尺寸:"+heightSize);

        float textWidth = mBound.width();   //文本的宽度
        if(mTextList.size()==0){
            //将文本分段
            int padding = getPaddingLeft() + getPaddingRight();
            int specWidth = widthSize - padding; //能够显示文本的最大宽度
            if(textWidth<specWidth){
                //说明一行足矣显示
                lineNum = 1;
                mTextList.add(mText);
            }else{
                //超过一行
                isOneLines = false;
                spLineNum = textWidth/specWidth;
                if((spLineNum+"").contains(".")){
                    lineNum = Integer.parseInt((spLineNum+"").substring(0,(spLineNum+"").indexOf(".") ))+1;
                }else{
                    lineNum = spLineNum;
                }
                int lineLength = (int)(mText.length()/spLineNum);
                Log.v("openxu", "文本总长度:"+mText);
                Log.v("openxu", "文本总长度:"+mText.length());
                Log.v("openxu", "能绘制文本的宽度:"+lineLength);
                Log.v("openxu", "需要绘制:"+lineNum+"行");
                Log.v("openxu", "lineLength:"+lineLength);
                for(int i = 0; i<lineNum; i++){
                    String lineStr;
                    if(mText.length()<lineLength){
                        lineStr = mText.substring(0, mText.length());
                    }else{
                        lineStr = mText.substring(0, lineLength);
                    }
                    Log.v("openxu", "lineStr:"+lineStr);
                    mTextList.add(lineStr);
                    if(!TextUtils.isEmpty(mText)) {
                        if(mText.length()<lineLength){
                            mText = mText.substring(0, mText.length());
                        }else{
                            mText = mText.substring(lineLength, mText.length());
                        }
                    }else{
                        break;
                    }
                }
            }
        }
        int width;
        int height ;
        if (widthMode == MeasureSpec.EXACTLY) {
            //如果match_parent或者具体的值，直接赋值
            width = widthSize;
        } else {
            //如果是wrap_content，我们要得到控件需要多大的尺寸
            if(isOneLines){
                //控件的宽度就是文本的宽度加上两边的内边距。内边距就是padding值，在构造方法执行完就被赋值
                width = (int) (getPaddingLeft() + textWidth + getPaddingRight());
            }else{
                //如果是多行，说明控件宽度应该填充父窗体
                width = widthSize;
            }
            Log.v("openxu", "文本的宽度:"+textWidth + "控件的宽度："+width);
        }
        //高度跟宽度处理方式一样
        if (heightMode == MeasureSpec.EXACTLY) {
            height = heightSize;
        } else {
            float textHeight = mBound.height();
            if(isOneLines){
                height = (int) (getPaddingTop() + textHeight + getPaddingBottom());
            }else{
                //如果是多行
                height = (int) (getPaddingTop() + textHeight*lineNum + getPaddingBottom());;
            }

            Log.v("openxu", "文本的高度:"+textHeight + "控件的高度："+height);
        }
        //保存测量宽度和测量高度
        setMeasuredDimension(width, height);
    }
}
```

布局文件：

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:openxu="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
        <view.openxu.com.mytextview.MyTextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:padding="20dip"
            openxu:mTextSize="25sp"
            openxu:mText="门前大桥下，游过一群鸭，快来快来数一数，二四六七八。帽儿破，鞋儿破，身上滴袈裟破，你笑我"
            openxu:mTextColor ="#0000ff"
            android:background="#ff0000"/>
</LinearLayout>
```

运行效果： 
![5](http://img.blog.csdn.net/20180109165445957?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

## 源码下载：

[https://github.com/openXu/MyTextView](https://github.com/openXu/MyTextView "下载")

# 自定义控件的基本步骤：

1. 继承View，重写构造方法 
2. 自定义属性，在构造方法中初始化属性 
3. 重写onMeasure方法测量宽高 
4. 重写onDraw方法绘制控件

# 深入解析自定义属性

## 1. 为什么要自定义属性

  要使用属性，首先这个属性应该存在，所以如果我们要使用自己的属性，必须要先把他定义出来才能使用。但我们平时在写布局文件的时候好像没有自己定义属性，但我们照样可以用很多属性，这是为什么？我想大家应该都知道：系统定义好的属性我们就可以拿来用呗，但是你们知道系统定义了哪些属性吗？哪些属性是我们自定义控件可以直接使用的，哪些不能使用？什么样的属性我们能使用？这些问题我想大家不一定都弄得清除，下面我们去一一解开这些谜团。 
  **系统定义的**所有**属性**我们可以在**\sdk\platforms\android-xx\data\res\values**目录下找到**attrs.xml**这个文件，这就是系统自带的所有属性，打开看看一些比较熟悉的：

```xml
<declare-styleable name="View">
    <attr name="id" format="reference" />
    <attr name="background" format="reference|color" />
    <attr name="padding" format="dimension" />
     ...
    <attr name="focusable" format="boolean" />
     ...
</declare-styleable>

<declare-styleable name="TextView">
    <attr name="text" format="string" localization="suggested" />
    <attr name="hint" format="string" />
    <attr name="textColor" />
    <attr name="textColorHighlight" />
    <attr name="textColorHint" />
     ...
</declare-styleable>

<declare-styleable name="ViewGroup_Layout">
    <attr name="layout_width" format="dimension">
        <enum name="fill_parent" value="-1" />
        <enum name="match_parent" value="-1" />
        <enum name="wrap_content" value="-2" />
    </attr>
    <attr name="layout_height" format="dimension">
        <enum name="fill_parent" value="-1" />
        <enum name="match_parent" value="-1" />
        <enum name="wrap_content" value="-2" />
    </attr>
</declare-styleable>

<declare-styleable name="LinearLayout_Layout">
    <attr name="layout_width" />
    <attr name="layout_height" />
    <attr name="layout_weight" format="float" />
    <attr name="layout_gravity" />
</declare-styleable>

<declare-styleable name="RelativeLayout_Layout">
    <attr name="layout_centerInParent" format="boolean" />
    <attr name="layout_centerHorizontal" format="boolean" />
    <attr name="layout_centerVertical" format="boolean" />
     ...
</declare-styleable>
```

  看看上面attrs.xml文件中的属性，发现他们都是有规律的分组的形式组织的。以declare-styleable 为一个组合，后面有一个name属性，属性的值为View 、TextView 等等，有没有想到什么？没错，属性值为View的那一组就是为View定义的属性，属性值为TextView的就是为TextView定义的属性…。

  因为所有的控件都是View的子类，所以为View定义的属性所有的控件都能使用，这就是为什么我们的自定义控件没有定义属性就能使用一些系统属性。

  但是并不是每个控件都能使用所有属性，比如TextView是View的子类，所以为View定义的所有属性它都能使用，但是子类肯定有自己特有的属性，得单独为它扩展一些属性，而单独扩展的这些属性只有它自己能有，View是不能使用的，比如View中不能使用android:text=“”。又比如，LinearLayout中能使用layout_weight属性，而RelativeLayout却不能使用，因为layout_weight是为LinearLayout的LayoutParams定义的。

  综上所述，自定义控件如果不自定义属性，就只能使用VIew的属性，但为了给我们的控件扩展一些属性，我们就必须自己去定义。

## 2. 怎样自定义属性

这两种的区别就是attr标签后面带不带**format**属性，如果带format的就是在定义属性，如果不带format的就是在使用已有的属性，name的值就是属性的名字，format是限定当前定义的属性能接受什么值。

  打个比方，比如系统已经定义了android:text属性，我们的自定义控件也需要一个文本的属性，可以有两种方式：

**第一种**：我们并不知道系统定义了此名称的属性，我们自己定义一个名为text或者mText的属性（属性名称可以随便起的）

```xml
<resources>
    <declare-styleable name="MyTextView">
        <attr name=“text" format="string" />
    </declare-styleable>
</resources>
```

**第二种**：我们知道系统已经定义过名称为text的属性，我们不用自己定义，只需要在自定义属性中申明，我要使用这个text属性 
（注意加上android命名空间，这样才知道使用的是系统的text属性）

```xml
<resources>
    <declare-styleable name="MyTextView">
        <attr name=“android:text"/>
    </declare-styleable>
</resources>
```

  为什么系统定义了此属性，我们在使用的时候还要声明？因为，系统定义的text属性是给TextView使用的，如果我们不申明，就不能使用text属性。

## 3. 属性值的类型format

**format支持的类型一共有11种：** 

### **(1) reference：参考某一资源ID**

- 属性定义：

```xml
<declare-styleable name = "名称">
     <attr name = "background" format = "reference" />
</declare-styleable>
```

- 属性使用：

```xml
<ImageView android:background = "@drawable/图片ID"/>
```

### **(2) color：颜色值**

- 属性定义：

```xml
<attr name = "textColor" format = "color" />
```

- 属性使用：

```xml
<TextView android:textColor = "#00FF00" />
```

### **(3) boolean：布尔值**

- 属性定义：

```xml
<attr name = "focusable" format = "boolean" />
```

- 属性使用：

```xml
<Button android:focusable = "true"/>
```

### **(4) dimension：尺寸值**

- 属性定义：

```xml
<attr name = "layout_width" format = "dimension" />
```

- 属性使用：

```xml
<Button android:layout_width = "42dip"/>
```

### **(5) float：浮点值**

- 属性定义：

```xml
<attr name = "fromAlpha" format = "float" />
```

- 属性使用：

```xml
<alpha android:fromAlpha = "1.0"/>
```

### **(6) integer：整型值**

- 属性定义：

```xml
<attr name = "framesCount" format="integer" />
```

- 属性使用：

```xml
<animated-rotate android:framesCount = "12"/>
```

### **(7) string：字符串**

- 属性定义：

```xml
<attr name = "text" format = "string" />
```

- 属性使用：

```xml
<TextView android:text = "我是文本"/>
```

### **(8) fraction：百分数**

- 属性定义：

```xml
<attr name = "pivotX" format = "fraction" />

```

- 属性使用：

```xml
<rotate android:pivotX = "200%"/>

```

### **(9) enum：枚举值**

- 属性定义：

```xml
<declare-styleable name="名称">
    <attr name="orientation">
        <enum name="horizontal" value="0" />
        <enum name="vertical" value="1" />
    </attr>
</declare-styleable>

```

- 属性使用：

```xml
<LinearLayout  
    android:orientation = "vertical">
</LinearLayout>

```

> 注意：枚举类型的属性在使用的过程中**只能同时使用其中一个**，不能 `android:orientation = “horizontal｜vertical"`

### **(10) flag：位或运算**

- 属性定义：

```xml
<declare-styleable name="名称">
    <attr name="gravity">
            <flag name="top" value="0x30" />
            <flag name="bottom" value="0x50" />
            <flag name="left" value="0x03" />
            <flag name="right" value="0x05" />
            <flag name="center_vertical" value="0x10" />
            ...
    </attr>
</declare-styleable>

```

- 属性使用：

```xml
<TextView android:gravity="bottom|left"/>

```

> 注意：位运算类型的属性在使用的过程中**可以使用多个值**

### **(11) 混合类型：属性定义时可以指定多种类型值**

- 属性定义：

```xml
<declare-styleable name = "名称">
     <attr name = "background" format = "reference|color" />
</declare-styleable>

```

- 属性使用：

```xml
<ImageView
android:background = "@drawable/图片ID" />
或者：
<ImageView
android:background = "#00FF00" />

```

  通过上面的学习我们已经知道怎么定义各种类型的属性，以及怎么使用它们，但是我们写好布局文件之后，要在控件中使用这些属性还需要将它解析出来。

## 4. 类中获取属性值

  在这之前，顺带讲一下**命名空间**，我们在布局文件中使用属性的时候（`android:layout_width="match_parent"`）发现前面都带有一个android：，这个android就是上面引入的命名空间`xmlns:android="http://schemas.android.com/apk/res/android”`，表示到android系统中查找该属性来源。只有引入了命名空间，XML文件才知道下面使用的属性应该去哪里找（哪里定义的，不能凭空出现，要有根据）。

  如果我们自定义属性，这个属性应该去我们的应用程序包中找，所以要引入我们应用包的命名空间`xmlns:openxu="http://schemas.android.com/apk/res-auto”`，res-auto表示自动查找，还有一种写法`xmlns:openxu="http://schemas.android.com/apk/com.example.openxu.myview"`，`com.example.openxu.myview`为我们的应用程序包名。

  按照上面学习的知识，我们先定义一些属性，并写好布局文件。 
先在res\values目录下创建attrs.xml，定义自己的属性：

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <declare-styleable name="MyTextView">
        <!--声明MyTextView需要使用系统定义过的text属性,注意前面需要加上android命名-->
        <attr name="android:text" />
        <attr name="mTextColor" format="color" />
        <attr name="mTextSize" format="dimension" />
    </declare-styleable>
</resources>
```

在布局文件中，使用属性（注意引入我们应用程序的命名空间，这样在能找到我们包中的attrs）：

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:openxu="http://schemas.android.com/apk/res-auto"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
        <com.example.openxu.myview.MyTextView
            android:layout_width="200dip"
            android:layout_height="100dip"
            openxu:mTextSize="25sp"
            android:text="我是文字"
            openxu:mTextColor ="#0000ff"
            android:background="#ff0000"/>
</LinearLayout>

```

在构造方法中获取属性值：

```java
public MyTextView(Context context, AttributeSet attrs, int defStyleAttr) {
    super(context, attrs, defStyleAttr);
    TypedArray ta = context.obtainStyledAttributes(attrs, R.styleable.MyTextView);
    String text = ta.getString(R.styleable.MyTextView_android_text);
    int mTextColor = ta.getColor(R.styleable.MyTextView_mTextColor, Color.BLACK);
    int mTextSize = ta.getDimensionPixelSize(R.styleable.MyTextView_mTextSize, 100);
    ta.recycle();  //注意回收
    Log.v("openxu", “text属性值:"+mText);
    Log.v("openxu", "mTextColor属性值:"+mTextColor);
    Log.v("openxu", "mTextSize属性值:"+mTextSize);
}
```

log输出：

```
05-21 00:14:07.192: V/openxu(25652): mText属性值:我是文字
05-21 00:14:07.192: V/openxu(25652): mTextColor属性值:-16776961
05-21 00:14:07.192: V/openxu(25652): mTextSize属性值:75
```

到此为止，是不是发现自定义属性是如此简单？

属性的定义我们应该学的差不多了，但有没有发现构造方法中获取属性值的时候有两个比较陌生的类`AttributeSet`和`TypedArray`，这两个类是怎么把属性值从布局文件中解析出来的？

## 5. Attributeset和TypedArray以及declare-styleable

  Attributeset看名字就知道是一个属性的集合，实际上，它内部就是一个XML解析器，帮我们将布局文件中该控件的所有属性解析出来，并以key-value的键值对形式维护起来。其实我们完全可以只用他通过下面的代码来获取我们的属性就行。

```java
public MyTextView(Context context, AttributeSet attrs, int defStyleAttr) {
    super(context, attrs, defStyleAttr);
    int count = attrs.getAttributeCount();
    for (int i = 0; i < count; i++) {
        String attrName = attrs.getAttributeName(i);
        String attrVal = attrs.getAttributeValue(i);
        Log.e("openxu", "attrName = " + attrName + " , attrVal = " + attrVal);
    }
}
```

log输出：

```
05-21 02:18:09.052: E/openxu(14704): attrName = background , attrVal = @2131427347
05-21 02:18:09.052: E/openxu(14704): attrName = layout_width , attrVal = 200.0dip
05-21 02:18:09.052: E/openxu(14704): attrName = layout_height , attrVal = 100.0dip
05-21 02:18:09.052: E/openxu(14704): attrName = text , attrVal = 我是文字
05-21 02:18:09.052: E/openxu(14704): attrName = mTextSize , attrVal = 25sp
05-21 02:18:09.052: E/openxu(14704): attrName = mTextColor , attrVal = #0000ff


```

  发现通过Attributeset获取属性的值时，它将我们布局文件中的值原原本本的获取出来的，比如宽度200.0dip，其实这并不是我们想要的，如果我们接下来要使用宽度值，我们还需要将dip去掉，然后转换成整形，这多麻烦。其实这都不算什么，更恶心的是，backgroud我应用了一个color资源ID，它直接给我拿到了这个ID值，前面还加了个@，接下来我要自己获取资源，并通过这个ID值获取到真正的颜色。 

我们再换**TypedArray**试试。 
  在这里，穿插一个知识点，定义属性的时候有一个**declare-styleable**，他是用来干嘛的，如果不要它可不可以？答案是可以的，我们自定义属性完全可以写成下面的形式：

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
        <attr name="mTextColor" format="color" />
        <attr name="mTextSize" format="dimension" />
</resources>
```

之前的形式是这样的：

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <declare-styleable name="MyTextView">
        <attr name="android:text" />
        <attr name="android:layout_width" />
        <attr name="android:layout_height" />
        <attr name="android:background" />
        <attr name="mTextColor" format="color" />
        <attr name="mTextSize" format="dimension" />
    </declare-styleable>
</resources>

或者：
<?xml version="1.0" encoding="utf-8"?>
<resources>
          <!--定义属性-->
       <attr name="mTextColor" format="color" />
        <attr name="mTextSize" format="dimension" />
    <declare-styleable name="MyTextView">
        <!--生成索引-->
        <attr name="android:text" />
        <attr name="android:layout_width" />
        <attr name="android:layout_height" />
        <attr name="android:background" />
        <attr name=“mTextColor" />
        <attr name="mTextSize"  />
    </declare-styleable>
</resources>
```

  我们都知道所有的资源文件在R中都会对应一个整型常量，我们可以通过这个ID值找到资源文件。 
  属性在R中对应的类是`public static final class attr`，如果我们写了**declare-styleable**，在R文件中就会生成**styleable**类，这个类其实就是将每个控件的属性分组，然后记录属性的索引值，而TypedArray正好需要通过此索引值获取属性。

```java
public static final class styleable

          public static final int[] MyTextView = {
            0x0101014f, 0x7f010038, 0x7f010039
        };
        public static final int MyTextView_android_text = 0;
        public static final int MyTextView_mTextColor = 1;
        public static final int MyTextView_mTextSize = 2;
｝
```

使用TypedArray获取属性值：

```java
public MyTextView(Context context, AttributeSet attrs, int defStyleAttr) {
    super(context, attrs, defStyleAttr);
    TypedArray ta = context.obtainStyledAttributes(attrs, R.styleable.MyTextView);
    String mText = ta.getString(R.styleable.MyTextView_android_text);
    int mTextColor = ta.getColor(R.styleable.MyTextView_mTextColor, Color.BLACK);
    int mTextSize = ta.getDimensionPixelSize(R.styleable.MyTextView_mTextSize, 100);
    float width = ta.getDimension(R.styleable.MyTextView_android_layout_width, 0.0f);
    float hight = ta.getDimension(R.styleable.MyTextView_android_layout_height,0.0f);
    int backgroud = ta.getColor(R.styleable.MyTextView_android_background, Color.BLACK);
    ta.recycle();  //注意回收
    Log.v("openxu", "width:"+width);
    Log.v("openxu", "hight:"+hight);
    Log.v("openxu", "backgroud:"+backgroud);
    Log.v("openxu", "mText:"+mText);
    Log.v("openxu", "mTextColor:"+mTextColor);
    Log.v("openxu", "mTextSize:"+mTextSize);ext, 0, mText.length(), mBound);
}
```

log输出：

```
05-21 02:56:49.162: V/openxu(22630): width:600.0
05-21 02:56:49.162: V/openxu(22630): hight:300.0
05-21 02:56:49.162: V/openxu(22630): backgroud:-12627531
05-21 02:56:49.162: V/openxu(22630): mText:我是文字
05-21 02:56:49.162: V/openxu(22630): mTextColor:-16777216
05-21 02:56:49.162: V/openxu(22630): mTextSize:100
```

  看看多么舒服的结果，我们得到了想要的宽高（float型），背景颜色（color的十进制）等，TypedArray提供了一系列获取不同类型属性的方法，这样就可以直接得到我们想要的数据类型，而不用像Attributeset获取属性后还要一个个处理才能得到具体的数据，实际上TypedArray是为我们获取属性值提供了方便，注意一点，TypedArray使用完毕后记得调用 `ta.recycle();`回收 。

# 深入解析控件测量onMeasure

## 1. onMeasure什么时候会被调用

  onMeasure方法的作用时测量控件的大小，什么时候需要测量控件的大小呢？我们举个栗子，做饭的时候我们炒一碗菜，炒菜的过程我们并不要求知道这道菜有多少分量，只有在菜做熟了我们要拿个碗盛放的时候，我们才需要掂量拿多大的碗盛放，这时候我们就要对菜的分量进行估测。 
  而我们的控件也正是如此，**创建一个View(执行构造方法)的时候不需要测量控件的大小**，只有将这个view**放入**一个容器（**父控件**）中的**时**候**才需要测量**，而这个**测量方法**就是**父控件唤起调用**的。当控件的父控件要放置该控件的时候，**父控件**会**调用子控件的onMeasure**方法询问子控件：“你有多大的尺寸，我要给你多大的地方才能容纳你？”，然后传入两个参数（widthMeasureSpec和heightMeasureSpec），这两个参数就是父控件告诉子控件可获得的空间以及关于这个空间的约束条件（好比我在思考需要多大的碗盛菜的时候我要看一下碗柜里最大的碗有多大，菜的分量不能超过这个容积，这就是碗对菜的约束），子控件拿着这些条件就能正确的测量自身的宽高了。 

## 2. onMeasure方法执行流程

  上面说到onMeasure方法是由父控件调用的，所有父控件都是ViewGroup的子类，**ViewGroup**是一个抽象类，它里面有一个抽象方法**onLayout**，这个方法的作用就是**摆放**它所有的**子控件（安排位置）**，因为是抽象类，不能直接new对象，所以我们在布局文件中可以使用View但是不能直接使用 ViewGroup。

  在给子控件确定位置之前，必须要获取到子控件的大小（只有确定了子控件的大小才能正确的确定上下左右四个点的坐标），而ViewGroup并没有重写View的onMeasure方法，也就是说抽象类ViewGroup没有为子控件测量大小的能力，它只能测量自己的大小。但是既然ViewGroup是一个能容纳子控件的容器，系统当然也考虑到测量子控件的问题，所以**ViewGroup**提供了三个**测量子控件**相关的**方法**(**measuireChildren**\\**measuireChild**\\**measureChildWithMargins**)，只是在ViewGroup中没有调用它们，所以它本身不具备为子控件测量大小的能力，但是他有这个潜力哦。

  为什么都有测量子控件的方法了而ViewGroup中不直接重写onMeasure方法，然后在onMeasure中调用呢？因为不同的容器摆放子控件的方式不同，比如RelativeLayout，LinearLayout这两个ViewGroup的子类，它们摆放子控件的方式不同，有的是线性摆放，而有的是叠加摆放，这就导致测量子控件的方式会有所差别，所以ViewGroup就干脆不直接测量子控件，他的子类要测量子控件就根据自己的布局特性重写onMeasure方法去测量。这么看来ViewGroup提供的三个测量子控件的方法岂不是没有作用？答案是NO，既然提供了就肯定有作用，这三个方法只是按照一种通用的方式去测量子控件，很多ViewGruop的子类测量子控件的时候就使用了ViewGroup的measureChildxxx系列方法；还有一个作用就是为我们自定义ViewGroup提供方便咯，自定义ViewGroup我会在后续专门探讨。

  **测量**的**时**候**父控件的onMeasure**方法会**遍历**他所有的**子控件**，挨个**调用子控件的measure方法**，**measure方法会调用onMeasure**，然后会**调用setMeasureDimension方法保存测量的大小**，一次遍历下来，第一个子控件以及这个子控件中的所有子控件都会完成测量工作；然后开始测量第二个子控件…；最后**父控件**所有的子控件都**完成测量**以后会**调用setMeasureDimension方法保存自己的测量大小**。值得注意的是，这个过程**不只执行一次**，也就是说有可能重复执行，因为有的时候，一轮测量下来，父控件发现某一个子控件的尺寸不符合要求，就会重新测量一遍。

举个栗子，看下图： 
![onMeasure方法执行流程](http://img.blog.csdn.net/20180109205020644?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

下面是测量的时序图： 
![测量的时序图](http://img.blog.csdn.net/20180109205011689?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

## 3. MeasureSpec类

  上面说到MeasureSpec约束是由父控件传递给子控件的，这个类里面到底封装了什么东西？我们看一看源码：

```java
public static class MeasureSpec {
    private static final int MODE_SHIFT = 30;
    private static final int MODE_MASK = 0x3 << MODE_SHIFT;
    // 父控件不强加任何约束给子控件，它可以是它想要任何大小
    public static final int UNSPECIFIED = 0 << MODE_SHIFT;
    // 父控件已为子控件确定了一个确切的大小，孩子将被给予这些界限，不管子控件自己希望的是多大
    public static final int EXACTLY = 1 << MODE_SHIFT;
    // 父控件会给子控件尽可能大的尺寸
    public static final int AT_MOST = 2 << MODE_SHIFT;

    // 根据所提供的大小和模式创建一个测量规范
    public static int makeMeasureSpec(int size, int mode) {
        if (sUseBrokenMakeMeasureSpec) {
            return size + mode;
        } else {
            return (size & ~MODE_MASK) | (mode & MODE_MASK);
        }
    }
    // 从所提供的测量规范中提取模式
    public static int getMode(int measureSpec) {
        return (measureSpec & MODE_MASK);
    }
    // 从所提供的测量规范中提取尺寸
    public static int getSize(int measureSpec) {
        return (measureSpec & ~MODE_MASK);
    }
    ...
}
```

  从源码中我们知道，**MeasureSpec**其实就是尺寸和模式通过各种位运算计算出的一个整型值，它提供了三种模式，还有三个方法（合成约束、分离模式、分离尺寸）。这三种模式对应的意思和布局文件中的参数值的对应关系（父控件填充屏幕*MATCH_PARENT*的情况，如果其他情况，请参考下面getChildMeasureSpec方法的结果）：

| **约束**           |      **布局参数**      |    **值**    |                  **说明**                  |
| ---------------- | :----------------: | :---------: | :--------------------------------------: |
| UNSPECIFIED(未指定) |                    |      0      |  父控件没有对子控件施加任何约束，子控件可以得到任意想要的大小（使用较少）。   |
| EXACTLY(完全)      | match_parent/具体宽高值 | 1073741824  | 父控件给子控件决定了确切大小，子控件将被限定在给定的边界里而忽略它本身大小。特别说明如果是填充父窗体，说明父控件已经明确知道子控件想要多大的尺寸了（就是剩余的空间都要了）栗子：碗柜最大的碗就这么大，菜有多少都只能盛到这个最大的碗里，盛不下的我就管不了了（吃掉或者倒掉） |
| AT_MOST(至多)      |    wrap-content    | -2147483648 | 子控件至多达到指定大小的值。包裹内容就是父窗体并不知道子控件到底需要多大尺寸（具体值），需要子控件自己测量之后再让父控件给他一个尽可能大的尺寸以便让内容全部显示但不能超过包裹内容的大小栗子：碗柜有各种大小的碗，菜少就拿小碗放，菜多就拿大碗放，但是不能浪费碗的容积，要保证碗刚好盛满菜 |

## 4. 从ViewGroup的onMeasure到View的onMeasure

### ① ViewGroup中三个测量子控件的方法：

  通过上面的介绍，我们知道，如果要自定义ViewGroup就必须重写onMeasure方法，在这里测量子控件的尺寸。子控件的尺寸怎么测量呢？ViewGroup中提供了三个关于测量子控件的方法：

```
 // 遍历ViewGroup中所有的子控件，调用measuireChild测量宽高
 protected void measureChildren (int widthMeasureSpec, int heightMeasureSpec) {
    final int size = mChildrenCount;
    final View[] children = mChildren;
    for (int i = 0; i < size; ++i) {
        final View child = children[i];
        if ((child.mViewFlags & VISIBILITY_MASK) != GONE) {
            //测量某一个子控件宽高
            measureChild(child, widthMeasureSpec, heightMeasureSpec);
        }
    }
}

// 测量某一个child的宽高
protected void measureChild (View child, int parentWidthMeasureSpec,
       int parentHeightMeasureSpec) {
   final LayoutParams lp = child.getLayoutParams();
   //获取子控件的宽高约束规则
   final int childWidthMeasureSpec = getChildMeasureSpec(parentWidthMeasureSpec,
           mPaddingLeft + mPaddingRight, lp. width);
   final int childHeightMeasureSpec = getChildMeasureSpec(parentHeightMeasureSpec,
           mPaddingTop + mPaddingBottom, lp. height);

   child.measure(childWidthMeasureSpec, childHeightMeasureSpec);
}

// 测量某一个child的宽高，考虑margin值
protected void measureChildWithMargins (View child,
       int parentWidthMeasureSpec, int widthUsed,
       int parentHeightMeasureSpec, int heightUsed) {
   final MarginLayoutParams lp = (MarginLayoutParams) child.getLayoutParams();
   //获取子控件的宽高约束规则
   final int childWidthMeasureSpec = getChildMeasureSpec(parentWidthMeasureSpec,
           mPaddingLeft + mPaddingRight + lp. leftMargin + lp.rightMargin
                   + widthUsed, lp. width);
   final int childHeightMeasureSpec = getChildMeasureSpec(parentHeightMeasureSpec,
           mPaddingTop + mPaddingBottom + lp. topMargin + lp.bottomMargin
                   + heightUsed, lp. height);
   //测量子控件
   child.measure(childWidthMeasureSpec, childHeightMeasureSpec);
}


```

  这三个方法分别做了那些工作大家应该比较清楚了吧？measureChildren 就是遍历所有子控件挨个测量，最终测量子控件的方法就是measureChild 和measureChildWithMargins 了，我们先了解几个知识点：

- **measureChildWithMargins跟measureChild的区别就是父控件支不支持margin属性**

  支不支持margin属性对子控件的测量是有影响的，比如我们的屏幕是1080x1920的，子控件的宽度为填充父窗体，如果使用了marginLeft并设置值为100； 
在测量子控件的时候，如果用measureChild，计算的宽度是1080，而如果是使用measureChildWithMargins，计算的宽度是1080-100 = 980。

- **怎样让ViewGroup支持margin属性？**

  ViewGroup中有两个内部类ViewGroup.LayoutParams和ViewGroup. MarginLayoutParams，MarginLayoutParams继承自LayoutParams ，这两个内部类就是VIewGroup的布局参数类，比如我们在LinearLayout等布局中使用的layout_width\layout_hight等以“layout_ ”开头的属性都是布局属性。在View中有一个mLayoutParams的变量用来保存这个View的所有布局属性。

- **LayoutParams和MarginLayoutParams 的关系：** 
  LayoutParams 中定义了两个属性（现在知道我们用的layout_width\layout_hight的来头了吧？）：

```xml
<declare-styleable name= "ViewGroup_Layout">
    <attr name ="layout_width" format="dimension">
        <enum name ="fill_parent" value="-1" />
        <enum name ="match_parent" value="-1" />
        <enum name ="wrap_content" value="-2" />
    </attr >
    <attr name ="layout_height" format="dimension">
        <enum name ="fill_parent" value="-1" />
        <enum name ="match_parent" value="-1" />
        <enum name ="wrap_content" value="-2" />
    </attr >
</declare-styleable >

```

MarginLayoutParams 是LayoutParams的子类，它当然也延续了layout_width\layout_hight 属性，但是它扩充了其他属性：

```xml
< declare-styleable name ="ViewGroup_MarginLayout">
    <attr name ="layout_width" />   <!--使用已经定义过的属性-->
    <attr name ="layout_height" />
    <attr name ="layout_margin" format="dimension"  />
    <attr name ="layout_marginLeft" format= "dimension"  />
    <attr name ="layout_marginTop" format= "dimension" />
    <attr name ="layout_marginRight" format= "dimension"  />
    <attr name ="layout_marginBottom" format= "dimension"  />
    <attr name ="layout_marginStart" format= "dimension"  />
    <attr name ="layout_marginEnd" format= "dimension"  />
</declare-styleable >

```

是不是对布局属性有了一个全新的认识？原来我们使用的margin属性是这么来的。

- **为什么LayoutParams 类要定义在ViewGroup中？** 
  大家都知道ViewGroup是所有容器的基类，一个控件需要被包裹在一个容器中，这个容器必须提供一种规则控制子控件的摆放，比如你的宽高是多少，距离那个位置多远等。所以ViewGroup有义务提供一个布局属性类，用于控制子控件的布局属性。
- **为什么View中会有一个mLayoutParams 变量？** 
  我们在之前学习自定义控件的时候学过自定义属性，我们在构造方法中，初始化布局文件中的属性值，我们姑且把属性分为两种。一种是本View的绘制属性，比如TextView的文本、文字颜色、背景等，这些属性是跟View的绘制相关的。另一种就是以“layout_”打头的叫做布局属性，这些属性是父控件对子控件的大小及位置的一些描述属性，这些属性在父控件摆放它的时候会使用到，所以先保存起来，而这些属性都是ViewGroup.LayoutParams定义的，所以用一个变量保存着。
- **怎样让ViewGroup支持margin属性？** 
  这一部分知识点在后续《自定义ViewGroup》中再去讲解 

### ② getChildMeasureSpec方法

  measureChildWithMargins跟measureChild 都调用了这个方法，其作用就是**通过父控件的宽高约束规则和父控件加在子控件上的宽高布局参数生成一个子控件的约束**。我们知道View的onMeasure方法需要两个参数（父控件对View的宽高约束），这个宽高约束就是通过这个方法生成的。有人会问为什么不直接拿着子控件的宽高参数去测量子控件呢？打个比方，父控件的宽高约束为wrap_content，而子控件为match_perent，是不是很有意思，父控件说我的宽高就是包裹我的子控件，我的子控件多大我就多大，而子控件说我的宽高填充父窗体，父控件多大我就多大。最后该怎么确定大小呢？所以我们需要为子控件重新生成一个新的约束规则。只要记住，**子控件的宽高约束规则是父控件调用getChildMeasureSpec方法生成**。 
![getChildMeasureSpec](http://img.blog.csdn.net/20180109204957470?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

getChildMeasure方法代码不多，也比较简单，就是几个switch将各种情况考虑后生成一个子控件的新的宽高约束，这个方法的结果能够用一个表来概括：

| 父控件的约束规则                         | 子控件的宽高属性                      | 子控件的约束规则    | 说明                                       |
| :------------------------------- | :---------------------------- | :---------- | :--------------------------------------- |
| EXACTLY<br>（父控件是填充父窗体，或者具体size值） | 具体的size/<br>MATCH_PARENT      | EXACTLY     | 子控件如果是具体值，约束尺寸就是这个值，模式为确定的；<br>子控件为填充父窗体，约束尺寸是父控件剩余大小，模式为确定的。 |
|                                  | WRAP-CONTENT                  | AT_MOST     | 子控件如果是包裹内容，约束尺寸值为父控件剩余大小 ，模式为至多          |
| AT_MOST<br>（父控件是包裹内容）            | 具体的size                       | EXACTLY     | 子控件如果是具体值，约束尺寸就是这个值，模式为确定的；              |
|                                  | MATCH_PARENT/<br>WRAP_CONTENT | AT_MOST     | 子控件为填充父窗体或者包裹内容 ，约束尺寸是父控件剩余大小 ，模式为至多     |
| UNSPECIFIED<br>（父控件未指定）          | 具体的size                       | EXACTLY     | 子控件如果是具体值，约束尺寸就是这个值，模式为确定的；              |
|                                  | MATCH_PARENT/<br>WRAP_CONTENT | UNSPECIFIED | 子控件为填充父窗体或者包裹内容 ，约束尺寸0，模式为未指定            |

getChildMeasureSpec源码：

```java
public static int getChildMeasureSpec(int spec, int padding, int childDimension) {
        int specMode = MeasureSpec.getMode(spec);
        int specSize = MeasureSpec.getSize(spec);

        int size = Math.max(0, specSize - padding);

        int resultSize = 0;
        int resultMode = 0;

        switch (specMode) {
        // Parent has imposed an exact size on us
        case MeasureSpec.EXACTLY:
            if (childDimension >= 0) {
                resultSize = childDimension;
                resultMode = MeasureSpec.EXACTLY;
            } else if (childDimension == LayoutParams.MATCH_PARENT) {
                // Child wants to be our size. So be it.
                resultSize = size;
                resultMode = MeasureSpec.EXACTLY;
            } else if (childDimension == LayoutParams.WRAP_CONTENT) {
                // Child wants to determine its own size. It can't be
                // bigger than us.
                resultSize = size;
                resultMode = MeasureSpec.AT_MOST;
            }
            break;

        // Parent has imposed a maximum size on us
        case MeasureSpec.AT_MOST:
            if (childDimension >= 0) {
                // Child wants a specific size... so be it
                resultSize = childDimension;
                resultMode = MeasureSpec.EXACTLY;
            } else if (childDimension == LayoutParams.MATCH_PARENT) {
                // Child wants to be our size, but our size is not fixed.
                // Constrain child to not be bigger than us.
                resultSize = size;
                resultMode = MeasureSpec.AT_MOST;
            } else if (childDimension == LayoutParams.WRAP_CONTENT) {
                // Child wants to determine its own size. It can't be
                // bigger than us.
                resultSize = size;
                resultMode = MeasureSpec.AT_MOST;
            }
            break;

        // Parent asked to see how big we want to be
        case MeasureSpec.UNSPECIFIED:
            if (childDimension >= 0) {
                // Child wants a specific size... let him have it
                resultSize = childDimension;
                resultMode = MeasureSpec.EXACTLY;
            } else if (childDimension == LayoutParams.MATCH_PARENT) {
                // Child wants to be our size... find out how big it should
                // be
                resultSize = View.sUseZeroUnspecifiedMeasureSpec ? 0 : size;
                resultMode = MeasureSpec.UNSPECIFIED;
            } else if (childDimension == LayoutParams.WRAP_CONTENT) {
                // Child wants to determine its own size.... find out how
                // big it should be
                resultSize = View.sUseZeroUnspecifiedMeasureSpec ? 0 : size;
                resultMode = MeasureSpec.UNSPECIFIED;
            }
            break;
        }
        //noinspection ResourceType
        return MeasureSpec.makeMeasureSpec(resultSize, resultMode);
    }
```

进行了上面的步骤，**接下来**就是在measureChildWithMarginsh或者measureChild中 **调用子控件的measure**方法测量子控件的尺寸了。

### ③ View的onMeasure

  View中onMeasure方法已经默认为我们的控件测量了宽高，我们看看它做了什么工作：

```java
protected void onMeasure( int widthMeasureSpec, int heightMeasureSpec) {
    setMeasuredDimension( getDefaultSize(getSuggestedMinimumWidth(), widthMeasureSpec),
            getDefaultSize(getSuggestedMinimumHeight(), heightMeasureSpec));
}
// 为宽度获取一个建议最小值
protected int getSuggestedMinimumWidth () {
    return (mBackground == null) ? mMinWidth : max(mMinWidth , mBackground.getMinimumWidth());
}
// 获取默认的宽高值
public static int getDefaultSize (int size, int measureSpec) {
    int result = size;
    int specMode = MeasureSpec. getMode(measureSpec);
    int specSize = MeasureSpec. getSize(measureSpec);
    switch (specMode) {
    case MeasureSpec. UNSPECIFIED:
        result = size;
        break;
    case MeasureSpec. AT_MOST:
    case MeasureSpec. EXACTLY:
        result = specSize;
        break;
    }
    return result;
}
```

从源码我们了解到：

- 如果View的宽高模式为 未指定，他的宽高将设置为android:minWidth/Height =”“值与背景宽高值中较大的一个；
- 如果View的宽高模式为 EXACTLY （具体的size ），最终宽高就是这个size值；
- 如果View的宽高模式为 EXACTLY （填充父控件 ），最终宽高将为填充父控件；
- 如果View的宽高模式为 AT_MOST （包裹内容），最终宽高也是填充父控件。

也就是说如果我们的自定义控件在布局文件中，只需要设置指定的具体宽高，或者MATCH_PARENT 的情况，我们可以不用重写onMeasure方法。

但如果自定义控件需要设置包裹内容WRAP_CONTENT ，我们需要重写onMeasure方法，为控件设置需要的尺寸；默认情况下WRAP_CONTENT 的处理也将填充整个父控件。

### ④ setMeasuredDimension

  onMeasure方法最后需要调用setMeasuredDimension方法来保存测量的宽高值，如果不调用这个方法，可能会产生不可预测的问题。

## 总结：

> 1. 测量控件大小是父控件发起的
> 2. 父控件要测量子控件大小，需要重写onMeasure方法，然后调用measureChildren或者measureChildWithMargin方法
> 3. onMeasure方法的参数是通过getChildMeasureSpec生成的
> 4. 如果我们自定义控件需要使用wrap_content，我们需要重写onMeasure方法
> 5. 测量控件的步骤： 
>    父控件`onMeasure`->`measureChildren` /`measureChildWithMargin`->`getChildMeasureSpec`-> 
>    子控件的`measure`->`onMeasure`->`setMeasureDimension`-> 
>    父控件onMeasure结束调用`setMeasureDimension`保存自己的大小

### measure() VS onMeasure()

  View中有两个关于测量的方法measure()和onMeasure()。
  **measure**()方法是final的，**不能被重写**。measure()方法中**调用**了**onMeasure**()方法。
  **onMeasure**()方法在View中的默认实现是**测量自身**的**大小**，所以在我们自定义ViewGroup的时候，如果我们不重写onMeasure()测量子控件的大小，子控件将会显示不出来的。ViewGroup中没有重写onMeasure()方法，在ViewGroup的子类（LinearLayout、RelativeLayout等）中会根据容器自身的布局特性，比如LinearLayout有两种布局方式，水平或者垂直，分别对onMeasure()方法进行不同的重写。

  总结一下就是measure()方法不能被重写，它会调用onMeasure()方法测量控件自身的大小，如果控件是一个容器，它需要重写onMeasure()方法遍历测量所有子控件的大小。还有当我们继承View**自定义控件时**，在布局文件中明明使用了wrap_content可结果还是填充父窗体，这是因为View的onMeasure()方法的默认实现，如果**要按照我们的需求正确的测量控件大小也需要重写onMeasure()方法**。

# 自定义布局容器

　　为了实现一些比较牛X的效果，比如侧滑菜单、滑动卡片等等，我们还需要了解自定义ViewGroup。官方文档中对ViewGroup这样描述的：

> ViewGroup是一种可以包含其他视图的特殊视图，他是各种布局和所有容器的基类，这些类也定义了ViewGroup.LayoutParams类作为类的布局参数。

　　之前，我们只是学习过自定义View，其实自定义ViewGroup和自定义View的步骤差不了多少，他们的的区别主要来自各自的作用不同，ViewGroup是容器，用来包含其他控件，而View是真正意义上看得见摸得着的，它需要将自己画出来。ViewGroup需要重写onMeasure方法测量子控件的宽高和自己的宽高，然后实现onLayout方法摆放子控件。而 View则是需要重写onMeasure根据测量模式和父控件给出的建议的宽高值计算自己的宽高，然后再父控件为其指定的区域绘制自己的图形。 

根据以往经验我们初步将自定义ViewGroup的步骤定为下面几步：

```
1. 继承ViewGroup，覆盖构造方法
2. 重写onMeasure方法测量子控件和自身宽高
3. 实现onLayout方法摆放子控件
```

## 1. 简单实现水平排列效果

我们先自定义一个ViewGroup作为布局容器，实现一个从左往右水平排列（排满换行）的效果：

```java
// 自定义布局管理器的示例。
public class CustomLayout extends ViewGroup {
      private static final String TAG = "CustomLayout";

    public CustomLayout(Context context) {
        super(context);
    }

    public CustomLayout(Context context, AttributeSet attrs) {
        this(context, attrs, 0);
    }

    public CustomLayout(Context context, AttributeSet attrs, int defStyle) {
        super(context, attrs, defStyle);
    }

    // 要求所有的孩子测量自己的大小，然后根据这些孩子的大小完成自己的尺寸测量
    @SuppressLint("NewApi") @Override
    protected void onMeasure( int widthMeasureSpec, int heightMeasureSpec) {
        // 计算出所有的childView的宽和高 
        measureChildren(widthMeasureSpec, heightMeasureSpec); 
        
        //测量并保存layout的宽高(使用getDefaultSize时，wrap_content和match_perent都是填充屏幕)
        //稍后会重新写这个方法，能达到wrap_content的效果
        setMeasuredDimension( getDefaultSize(getSuggestedMinimumWidth(), widthMeasureSpec),
                getDefaultSize(getSuggestedMinimumHeight(), heightMeasureSpec));
    }

    // 为所有的子控件摆放位置.
    @Override
    protected void onLayout( boolean changed, int left, int top, int right, int bottom) {
        final int count = getChildCount();
        int childMeasureWidth = 0;
        int childMeasureHeight = 0;
        int layoutWidth = 0;    // 容器已经占据的宽度
        int layoutHeight = 0;   // 容器已经占据的宽度
        int maxChildHeight = 0; //一行中子控件最高的高度，用于决定下一行高度应该在目前基础上累加多少
        for(int i = 0; i<count; i++){
            View child = getChildAt(i);
             //注意此处不能使用getWidth和getHeight，这两个方法必须在onLayout执行完，才能正确获取宽高
            childMeasureWidth = child.getMeasuredWidth(); 
            childMeasureHeight = child.getMeasuredHeight(); 
            if(layoutWidth<getWidth()){
                   //如果一行没有排满，继续往右排列
                  left = layoutWidth;
                  right = left+childMeasureWidth;
                  top = layoutHeight;
                  bottom = top+childMeasureHeight;
            } else{
                   //排满后换行
                  layoutWidth = 0;
                  layoutHeight += maxChildHeight;
                  maxChildHeight = 0;

                  left = layoutWidth;
                  right = left+childMeasureWidth;
                  top = layoutHeight;
                  bottom = top+childMeasureHeight;
            }

            layoutWidth += childMeasureWidth;  //宽度累加
             if(childMeasureHeight>maxChildHeight){
                  maxChildHeight = childMeasureHeight;
            }

             //确定子控件的位置，四个参数分别代表（左上右下）点的坐标值
            child.layout(left, top, right, bottom);
        }
    }
}

```

**布局文件：**

```xml
<?xml version="1.0" encoding= "utf-8"?>
<com.openxu.costomlayout.CustomLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width= "wrap_content"
    android:layout_height= "wrap_content" >

    <Button
        android:layout_width= "wrap_content"
        android:layout_height= "wrap_content"
        android:background= "#FF8247"
        android:textColor= "#ffffff"
        android:textSize="20dip"
        android:padding= "20dip"
        android:text="按钮1" />

    <Button
        android:layout_width= "wrap_content"
        android:layout_height= "wrap_content"
        android:background= "#8B0A50"
        android:textColor= "#ffffff"
        android:textSize="20dip"
        android:padding= "10dip"
        android:text="按钮2222222222222" />

    <Button
        android:layout_width= "wrap_content"
        android:layout_height= "wrap_content"
        android:background= "#7CFC00"
        android:textColor= "#ffffff"
        android:textSize="20dip"
        android:padding= "15dip"
        android:text="按钮333333" />

    <Button
        android:layout_width= "wrap_content"
        android:layout_height= "wrap_content"
        android:background= "#1E90FF"
        android:textColor= "#ffffff"
        android:textSize="10dip"
        android:padding= "10dip"
        android:text="按钮4" />

    <Button
        android:layout_width= "wrap_content"
        android:layout_height= "wrap_content"
        android:background= "#191970"
        android:textColor= "#ffffff"
        android:textSize="20dip"
        android:padding= "15dip"
        android:text="按钮5" />

    <Button
        android:layout_width= "wrap_content"
        android:layout_height= "wrap_content"
        android:background= "#7A67EE"
        android:textColor= "#ffffff"
        android:textSize="20dip"
        android:padding= "20dip"
        android:text="按钮6" />

</com.openxu.costomlayout.CustomLayout>
```

**运行效果：** 
![1](http://img.blog.csdn.net/20180110105939530?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

　　运行成功，是不是略有成就感？这个布局就是简单版的`LinearLayout`设置`android:orientation ="horizontal"`的效果，比他还牛X一点，还能自动换行（哈哈）。接下来我们要实现一个功能，只需要在布局文件中指定布局属性，就能控制子控件在什么位置（类似相对布局`RelativeLayout`）。

## 2. 自定义LayoutParams

　　回想一下我们平时使用`RelativeLayout`的时候，在布局文件中使用`android:layout_alignParentRight="true"`、`android:layout_centerInParent="true"`等各种属性，就能控制子控件显示在父控件的上下左右、居中等效果。
　　ViewGroup中有两个内部类`ViewGroup.LayoutParams`和`ViewGroup.MarginLayoutParams`，`MarginLayoutParams`继承自`LayoutParams`，这两个内部类就是`ViewGroup`的布局参数类，比如我们在`LinearLayout`等布局中使用的`layout_width\layout_hight`等以“layout_ ”开头的属性都是布局属性。 
　　在View中有一个`mLayoutParams`的变量用来保存这个View的所有布局属性。`ViewGroup.LayoutParams`有两个属性`layout_width`和`layout_height`，因为所有的容器都需要设置子控件的宽高，所以这个`LayoutParams`是所有布局参数的基类，如果需要扩展其他属性，都应该继承自它。比如`RelativeLayout`中就提供了它自己的布局参数类`RelativeLayout.LayoutParams`，并扩展了很多布局参数，我们平时在`RelativeLayout`中使用的布局属性都来自它 ：

```xml
<declare-styleable name= "RelativeLayout_Layout">
        <attr name ="layout_toLeftOf" format= "reference" />
        <attr name ="layout_toRightOf" format= "reference" />
        <attr name ="layout_above" format="reference" />
        <attr name ="layout_below" format="reference" />
        <attr name ="layout_alignBaseline" format= "reference" />
        <attr name ="layout_alignLeft" format= "reference" />
        <attr name ="layout_alignTop" format= "reference" />
        <attr name ="layout_alignRight" format= "reference" />
        <attr name ="layout_alignBottom" format= "reference" />
        <attr name ="layout_alignParentLeft" format= "boolean" />
        <attr name ="layout_alignParentTop" format= "boolean" />
        <attr name ="layout_alignParentRight" format= "boolean" />
        <attr name ="layout_alignParentBottom" format= "boolean" />
        <attr name ="layout_centerInParent" format= "boolean" />
        <attr name ="layout_centerVertical" format= "boolean" />
        <attr name ="layout_alignWithParentIfMissing" format= "boolean" />
        <attr name ="layout_toStartOf" format= "reference" />
        <attr name ="layout_toEndOf" format="reference" />
        <attr name ="layout_alignStart" format= "reference" />
        <attr name ="layout_alignEnd" format= "reference" />
        <attr name ="layout_alignParentStart" format= "boolean" />
        <attr name ="layout_alignParentEnd" format= "boolean" />
    </declare-styleable >

```

看了上面的介绍，我们大概知道怎么为我们的布局容器定义自己的布局属性了吧，就不绕弯子了，按照下面的步骤做： 

### ① 大致明确布局容器的需求，初步定义布局属性

　　在定义属性之前要弄清楚，我们自定义的布局容器需要满足那些需求，需要哪些属性，比如，我们现在要实现像相对布局一样，为子控件设置一个位置属性layout_position=""，来控制子控件在布局中显示的位置。暂定位置有五种：左上、左下、右上、右下、居中。有了需求，我们就在attrs.xml定义自己的布局属性（和之前讲的自定义属性一样的操作，不太了解的可以翻阅上文**《深入解析自定义属性》**。

```xml
<?xml version="1.0" encoding= "utf-8"?>
<resources> 
    <declare-styleable name ="CustomLayout">
    <attr name ="layout_position">
        <enum name ="center" value="0" />
        <enum name ="left" value="1" />
        <enum name ="right" value="2" />
        <enum name ="bottom" value="3" />
        <enum name ="rightAndBottom" value="4" />
    </attr >
    </declare-styleable>
</resources>
```

left就代表是左上（按常理默认就是左上方开始，就不用写leftTop了，简洁一点），bottom左下，right 右上，rightAndBottom右下，center居中。属性类型是枚举，同时只能设置一个值。 

### ② 继承LayoutParams，定义布局参数类

　　我们可以选择继承`ViewGroup.LayoutParams`，这样的话我们的布局只是简单的支持`layout_width`和`layout_height`；也可以继承`MarginLayoutParams`，就能使用layout_marginxxx属性了。因为后面我们还要用到margin属性，所以这里方便起见就直接继承`MarginLayoutParams`了。 
　　覆盖构造方法，然后在有AttributeSet参数的构造方法中初始化参数值，这个构造方法才是布局文件被映射为对象的时候被调用的。

```java
public static class CustomLayoutParams extends MarginLayoutParams {
       public static final int POSITION_MIDDLE = 0; // 中间
       public static final int POSITION_LEFT = 1; // 左上方
       public static final int POSITION_RIGHT = 2; // 右上方
       public static final int POSITION_BOTTOM = 3; // 左下角
       public static final int POSITION_RIGHTANDBOTTOM = 4; // 右下角

       public int position = POSITION_LEFT;  // 默认我们的位置就是左上角

       public CustomLayoutParams(Context c, AttributeSet attrs) {
             super(c, attrs);
            TypedArray a = c.obtainStyledAttributes(attrs,R.styleable.CustomLayout );
             //获取设置在子控件上的位置属性
             position = a.getInt(R.styleable.CustomLayout_layout_position ,position );

            a.recycle();
      }

       public CustomLayoutParams( int width, int height) {
             super(width, height);
      }

       public CustomLayoutParams(ViewGroup.LayoutParams source) {
             super(source);
      }
}
```

### ③ 重写generateLayoutParams()

　　在`ViewGroup`中有下面几个关于`LayoutParams`的方法，`generateLayoutParams (AttributeSet attrs)`是在布局文件被填充为对象的时候调用的，这个方法是下面几个方法中最重要的，如果不重写它，我么布局文件中设置的布局参数都不能拿到。RTRT后面我也会专门写一篇博客来介绍布局文件被添加到activity窗口的过程，里面会讲到这个方法被调用的来龙去脉。其他几个方法我们最好也能重写一下，将里面的`LayoutParams`换成我们自定义的`CustomLayoutParams`类，避免以后会遇到布局参数类型转换异常。

```java
@Override
public LayoutParams generateLayoutParams(AttributeSet attrs) {
    return new CustomLayoutParams(getContext(), attrs);
}
@Override
protected ViewGroup.LayoutParams generateLayoutParams(ViewGroup.LayoutParams p) {
    return new CustomLayoutParams (p);
}
@Override
protected LayoutParams generateDefaultLayoutParams() {
    return new CustomLayoutParams (LayoutParams.MATCH_PARENT , LayoutParams.MATCH_PARENT);
}
@Override
protected boolean checkLayoutParams(ViewGroup.LayoutParams p) {
    return p instanceof CustomLayoutParams ;
}
```

### ④ 在onMeasure和onLayout中使用布局参数

　　经过上面几步之后，我们运行程序，就能获取子控件的布局参数了，在onMeasure方法和onLayout方法中，我们按照自定义布局容器的特殊需求，对宽度和位置坐特殊处理。这里我们需要注意一下，如果布局容器被设置为包裹类容，我们只需要保证能将最大的子控件包裹住就ok，代码注释比较详细，就不多说了。

```java
 @Override
protected void onMeasure( int widthMeasureSpec, int heightMeasureSpec) { 
  //获得此ViewGroup上级容器为其推荐的宽和高，以及计算模式  
 int widthMode = MeasureSpec. getMode(widthMeasureSpec); 
 int heightMode = MeasureSpec. getMode(heightMeasureSpec); 
 int sizeWidth = MeasureSpec. getSize(widthMeasureSpec); 
 int sizeHeight = MeasureSpec. getSize(heightMeasureSpec); 
 int layoutWidth = 0;
 int layoutHeight = 0;
      // 计算出所有的childView的宽和高
     measureChildren(widthMeasureSpec, heightMeasureSpec);

      int cWidth = 0;
      int cHeight = 0;
      int count = getChildCount(); 

      if(widthMode == MeasureSpec. EXACTLY){
      //如果布局容器的宽度模式是确定的（具体的size或者match_parent），直接使用父窗体建议的宽度
           layoutWidth = sizeWidth;
     } else{
     //如果是未指定或者wrap_content，我们都按照包裹内容做，宽度方向上只需要拿到所有子控件中宽度做大的作为布局宽度
           for ( int i = 0; i < count; i++)  { 
               View child = getChildAt(i); 
               cWidth = child.getMeasuredWidth(); 
               //获取子控件最大宽度
               layoutWidth = cWidth > layoutWidth ? cWidth : layoutWidth;
           }
     }
      //高度跟宽度处理思想一样
      if(heightMode == MeasureSpec. EXACTLY){
           layoutHeight = sizeHeight;
     } else{
            for ( int i = 0; i < count; i++)  { 
                  View child = getChildAt(i); 
                  cHeight = child.getMeasuredHeight();
                  layoutHeight = cHeight > layoutHeight ? cHeight : layoutHeight;
           }
     }

      // 测量并保存layout的宽高
     setMeasuredDimension(layoutWidth, layoutHeight);
}

@Override
protected void onLayout( boolean changed, int left, int top, int right,
            int bottom) {
      final int count = getChildCount();
      int childMeasureWidth = 0;
      int childMeasureHeight = 0;
     CustomLayoutParams params = null;
      for ( int i = 0; i < count; i++) {
           View child = getChildAt(i);
            // 注意此处不能使用getWidth和getHeight，这两个方法必须在onLayout执行完，才能正确获取宽高
           childMeasureWidth = child.getMeasuredWidth();
           childMeasureHeight = child.getMeasuredHeight();

           params = (CustomLayoutParams) child.getLayoutParams(); 
           switch (params. position) {
            case CustomLayoutParams. POSITION_MIDDLE:    // 中间
                 left = (getWidth()-childMeasureWidth)/2;
                 top = (getHeight()-childMeasureHeight)/2;
                  break;
            case CustomLayoutParams. POSITION_LEFT:      // 左上方
                 left = 0;
                 top = 0;
                  break;
            case CustomLayoutParams. POSITION_RIGHT:     // 右上方
                 left = getWidth()-childMeasureWidth;
                 top = 0;
                  break;
            case CustomLayoutParams. POSITION_BOTTOM:    // 左下角
                 left = 0;
                 top = getHeight()-childMeasureHeight;
                  break;
            case CustomLayoutParams. POSITION_RIGHTANDBOTTOM:// 右下角
                 left = getWidth()-childMeasureWidth;
                 top = getHeight()-childMeasureHeight;
                  break;
            default:
                  break;
           }

            // 确定子控件的位置，四个参数分别代表（左上右下）点的坐标值
           child.layout(left, top, left+childMeasureWidth, top+childMeasureHeight);
     }
}
```

### ⑤ 在布局文件中使用布局属性

　　注意引入命名空间`xmlns:openxu= "http://schemas.android.com/apk/res/包名"`

```xml
<?xml version="1.0" encoding= "utf-8"?>
<com.openxu.costomlayout.CustomLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:openxu= "http://schemas.android.com/apk/res/com.openxu.costomlayout"
    android:background="#33000000"
    android:layout_width= "match_parent "
    android:layout_height= "match_parent" >

    <Button
        android:layout_width= "wrap_content"
        android:layout_height= "wrap_content"
        openxu:layout_position= "left"
        android:background= "#FF8247"
        android:textColor= "#ffffff"
         android:textSize="20dip"
        android:padding= "20dip"
        android:text= "按钮1" />

    <Button
        android:layout_width= "wrap_content"
        android:layout_height= "wrap_content"
        openxu:layout_position= "right"
        android:background= "#8B0A50"
        android:textColor= "#ffffff"
        android:textSize= "18dip"
        android:padding= "10dip"
        android:text= "按钮2222222222222" />

    <Button
        android:layout_width= "wrap_content"
        android:layout_height= "wrap_content"
        openxu:layout_position= "bottom"
        android:background= "#7CFC00"
        android:textColor= "#ffffff"
        android:textSize= "20dip"
        android:padding= "15dip"
        android:text= "按钮333333" />

    <Button
        android:layout_width= "wrap_content"
        android:layout_height= "wrap_content"
        openxu:layout_position= "rightAndBottom"
        android:background= "#1E90FF"
        android:textColor= "#ffffff"
        android:textSize= "15dip"
        android:padding= "10dip"
        android:text= "按钮4" />

    <Button
        android:layout_width= "wrap_content"
        android:layout_height= "wrap_content"
        openxu:layout_position= "center"
        android:background= "#191970"
        android:textColor= "#ffffff"
        android:textSize= "20dip"
        android:padding= "15dip"
        android:text= "按钮5" />

</com.openxu.costomlayout.CustomLayout>
```



**运行效果：** 
　　下面几个效果分别对应布局容器宽高设置不同的属性的情况（设置match_parent 、设置200dip、设置wrap_content）： 
![2](http://img.blog.csdn.net/20180110105924973?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
从运行结果看，我们自定义的布局容器在各种宽高设置下都能很好的测量大小和摆放子控件。现在我们让他支持margin属性

## 3. 支持layout_margin属性

　　如果我们自定义的布局参数类继承自`MarginLayoutParams`，就自动支持了layout_margin属性了，我们需要做的就是直接在布局文件中使用layout_margin属性，然后再`onMeasure`和`onLayout`中使用`margin`属性值测量和摆放子控件。需要注意的是我们测量子控件的时候应该调用`measureChildWithMargin()`方法。



**onMeasure和onLayout：**

```java
protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) { 
   // 获得此ViewGroup上级容器为其推荐的宽和高，以及计算模式   
  int widthMode = MeasureSpec. getMode(widthMeasureSpec); 
  int heightMode = MeasureSpec. getMode(heightMeasureSpec); 
  int sizeWidth = MeasureSpec. getSize(widthMeasureSpec); 
  int sizeHeight = MeasureSpec. getSize(heightMeasureSpec); 
  int layoutWidth = 0;
  int layoutHeight = 0;
       int cWidth = 0;
       int cHeight = 0;
       int count = getChildCount(); 

       // 计算出所有的childView的宽和高
       for( int i = 0; i < count; i++){
            View child = getChildAt(i); 
            measureChildWithMargins(child, widthMeasureSpec, 0, heightMeasureSpec, 0);
      }
      CustomLayoutParams params = null;
       if(widthMode == MeasureSpec. EXACTLY){
             //如果布局容器的宽度模式时确定的（具体的size或者match_parent）
            layoutWidth = sizeWidth;
      } else{
             //如果是未指定或者wrap_content，我们都按照包裹内容做，宽度方向上只需要拿到所有子控件中宽度做大的作为布局宽度
             for ( int i = 0; i < count; i++)  { 
                   View child = getChildAt(i); 
               cWidth = child.getMeasuredWidth(); 
               params = (CustomLayoutParams) child.getLayoutParams(); 
               //获取子控件宽度和左右边距之和，作为这个控件需要占据的宽度
               int marginWidth = cWidth+params.leftMargin+params.rightMargin ;
               layoutWidth = marginWidth > layoutWidth ? marginWidth : layoutWidth;
            }
      }
       //高度跟宽度处理思想一样
       if(heightMode == MeasureSpec. EXACTLY){
            layoutHeight = sizeHeight;
      } else{
             for ( int i = 0; i < count; i++)  { 
                   View child = getChildAt(i); 
                   cHeight = child.getMeasuredHeight();
                   params = (CustomLayoutParams) child.getLayoutParams(); 
                   int marginHeight = cHeight+params.topMargin+params.bottomMargin ;
                   layoutHeight = marginHeight > layoutHeight ? marginHeight : layoutHeight;
            }
      }

       // 测量并保存layout的宽高
      setMeasuredDimension(layoutWidth, layoutHeight);
}

@Override
protected void onLayout( boolean changed, int left, int top, int right,
             int bottom) {
       final int count = getChildCount();
       int childMeasureWidth = 0;
       int childMeasureHeight = 0;
      CustomLayoutParams params = null;
       for ( int i = 0; i < count; i++) {
            View child = getChildAt(i);
             // 注意此处不能使用getWidth和getHeight，这两个方法必须在onLayout执行完，才能正确获取宽高
            childMeasureWidth = child.getMeasuredWidth();
            childMeasureHeight = child.getMeasuredHeight();
            params = (CustomLayoutParams) child.getLayoutParams(); 
      switch (params. position) {
             case CustomLayoutParams. POSITION_MIDDLE:    // 中间
                  left = (getWidth()-childMeasureWidth)/2 - params.rightMargin + params.leftMargin ;
                  top = (getHeight()-childMeasureHeight)/2 + params.topMargin - params.bottomMargin ;
                   break;
             case CustomLayoutParams. POSITION_LEFT:      // 左上方
                  left = 0 + params. leftMargin;
                  top = 0 + params. topMargin;
                   break;
             case CustomLayoutParams. POSITION_RIGHT:     // 右上方
                  left = getWidth()-childMeasureWidth - params.rightMargin;
                  top = 0 + params. topMargin;
                   break;
             case CustomLayoutParams. POSITION_BOTTOM:    // 左下角
                  left = 0 + params. leftMargin;
                  top = getHeight()-childMeasureHeight-params.bottomMargin ;
                   break;
             case CustomLayoutParams. POSITION_RIGHTANDBOTTOM:// 右下角
                  left = getWidth()-childMeasureWidth - params.rightMargin;
                  top = getHeight()-childMeasureHeight-params.bottomMargin ;
                   break;
             default:
                   break;
            }

             // 确定子控件的位置，四个参数分别代表（左上右下）点的坐标值
            child.layout(left, top, left+childMeasureWidth, top+childMeasureHeight);
      }
}
```

**布局文件：**

```xml
<?xml version="1.0" encoding= "utf-8"?>
<com.openxu.costomlayout.CustomLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:openxu= "http://schemas.android.com/apk/res/com.openxu.costomlayout"
    android:background="#33000000"
    android:layout_width= "match_parent"
    android:layout_height= "match_parent" >

    <Button
        android:layout_width= "wrap_content"
        android:layout_height= "wrap_content"
        openxu:layout_position= "left"
        android:layout_marginLeft = "20dip"
        android:background= "#FF8247"
        android:textColor= "#ffffff"
        android:textSize="20dip"
        android:padding= "20dip"
        android:text="按钮1" />

    <Button
        android:layout_width= "wrap_content"
        android:layout_height= "wrap_content"
        android:layout_marginTop = "30dip"
        openxu:layout_position= "right"
        android:background= "#8B0A50"
        android:textColor= "#ffffff"
        android:textSize="18dip"
        android:padding= "10dip"
        android:text="按钮2222222222222" />

    <Button
        android:layout_width= "wrap_content"
        android:layout_height= "wrap_content"
        android:layout_marginLeft = "30dip"
        android:layout_marginBottom = "10dip"
        openxu:layout_position= "bottom"
        android:background= "#7CFC00"
        android:textColor= "#ffffff"
        android:textSize="20dip"
        android:padding= "15dip"
        android:text="按钮333333" />

    <Button
        android:layout_width= "wrap_content"
        android:layout_height= "wrap_content"
        openxu:layout_position= "rightAndBottom"
        android:layout_marginBottom = "30dip"
        android:background= "#1E90FF"
        android:textColor= "#ffffff"
        android:textSize="15dip"
        android:padding= "10dip"
        android:text="按钮4" />

    <Button
        android:layout_width= "wrap_content"
        android:layout_height= "wrap_content"
        openxu:layout_position= "center"
        android:layout_marginBottom = "30dip"
        android:layout_marginRight = "30dip"
        android:background= "#191970"
        android:textColor= "#ffffff"
        android:textSize="20dip"
        android:padding= "15dip"
        android:text="按钮5" />

</com.openxu.costomlayout.CustomLayout>
```

**运行效果：** 
![3](http://img.blog.csdn.net/20180110105917729?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
　　好了，就写到这里，如果想尝试设置其他属性，比如above、below等，感兴趣的同学可以尝试一下哦~。其实也没什么难的，无非就是如果布局属性定义的多，那么在onMeasure和onLayout中考虑的问题就更多更复杂，自定义布局容器就是根据自己的需求，让容器满足我们特殊的摆放要求。

## 总结：

### **自定义ViewGroup的步骤：**

 ①. 继承ViewGroup，覆盖构造方法 
 ②. 重写onMeasure方法测量子控件和自身宽高 
 ③. 实现onLayout方法摆放子控件

### **为布局容器自定义布局属性：**

 ①. 大致明确布局容器的需求，初步定义布局属性 
 ②. 继承LayoutParams，定义布局参数类 
 ③. 重写获取布局参数的方法 
 ④. 在onMeasure和onLayout中使用布局参数
 ⑤. 在布局文件中使用布局属性 

## 自定义控件方法调用流程
[![自定义控件方法调用流程](http://img.blog.csdn.net/20180118122927663?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)](http://img.blog.csdn.net/20180118122927663?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

## layout() VS onLayout()

  首先我们要明确布局的本质是什么，布局就是为View设置四个坐标值，这四个坐标值保存在View的成员变量mLeft、mTop、mRight、mBottom中，方便View在绘制（onDraw）的时候知道应该在那个区域内绘制控件。而我们看到layout()方法中实际上就是为这几个成员变量赋值的，所以到底**真正设置坐标的是layout()方法**，那onLayout()的作用是什么呢？ 
  **onLayout**()都是由**ViewGroup的子类实现**的，他的作用就是确定容器中**每个子控件的位置**，由于不同的容器有不容的布局策略，所以每个容器对onLayout()方法的实现都不同，onLayout()方法会遍历容器中所有的子控件，然后计算他们左上右下的坐标值，最后调用child.layout()方法为子控件设置坐标；由于**layout()方法中又调用了onLayout()方法**，如果子控件child也是一个容器，就会继续为它的子控件计算坐标，如果child不是容器，onLayout()方法将什么也不做，这样下来，只要Activity根窗口mDecor的layout()方法执行完毕，窗口中所有的子容器、子控件都将完成布局操作。

  其实布局过程的调用方式和测量过程是一样的，ViewGroup的子类都要重写onMeasure()方法遍历子控件调用他们的measure()方法，measure()方法又会调用onMeasure()方法，如果子控件是普通控件就完成了测量，如果是容器将会继续遍历其孙子控件。　　
  
**源码下载：**
[https://github.com/openXu/View-CustomLayout](https://github.com/openXu/View-CustomLayout)









引用：
[Android自定义View（一、初体验自定义TextView）](http://blog.csdn.net/xmxkf/article/details/51454685)
[Android自定义View（二、深入解析自定义属性）](http://blog.csdn.net/xmxkf/article/details/51468648)
[Android自定义View（三、深入解析控件测量onMeasure）](http://blog.csdn.net/xmxkf/article/details/51490283)
[Android自定义ViewGroup（四、打造自己的布局容器）](http://blog.csdn.net/xmxkf/article/details/51500304)

