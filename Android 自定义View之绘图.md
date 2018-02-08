<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [【Android 自定义View之绘图】](#android-%E8%87%AA%E5%AE%9A%E4%B9%89view%E4%B9%8B%E7%BB%98%E5%9B%BE)
- [基础图形的绘制](#%E5%9F%BA%E7%A1%80%E5%9B%BE%E5%BD%A2%E7%9A%84%E7%BB%98%E5%88%B6)
  - [一、Paint与Canvas](#%E4%B8%80paint%E4%B8%8Ecanvas)
    - [Paint](#paint)
      - [Paint的基本设置函数](#paint%E7%9A%84%E5%9F%BA%E6%9C%AC%E8%AE%BE%E7%BD%AE%E5%87%BD%E6%95%B0)
      - [1 、setAntiAlias(true) 设置是否抗锯齿](#1-setantialiastrue-%E8%AE%BE%E7%BD%AE%E6%98%AF%E5%90%A6%E6%8A%97%E9%94%AF%E9%BD%BF)
      - [2、setStyle (Paint.Style style) 设置填充样式](#2setstyle-paintstyle-style-%E8%AE%BE%E7%BD%AE%E5%A1%AB%E5%85%85%E6%A0%B7%E5%BC%8F)
      - [3、setColor(@ColorInt int color) 设置画笔颜色](#3setcolorcolorint-int-color-%E8%AE%BE%E7%BD%AE%E7%94%BB%E7%AC%94%E9%A2%9C%E8%89%B2)
      - [4、setStrokeWidth(float width) 设置画笔宽度](#4setstrokewidthfloat-width-%E8%AE%BE%E7%BD%AE%E7%94%BB%E7%AC%94%E5%AE%BD%E5%BA%A6)
      - [5、setShadowLayer(float radius, float dx, float dy, int shadowColor) 设置阴影](#5setshadowlayerfloat-radius-float-dx-float-dy-int-shadowcolor-%E8%AE%BE%E7%BD%AE%E9%98%B4%E5%BD%B1)
    - [Canvas](#canvas)
  - [二、基本几何图形绘制](#%E4%BA%8C%E5%9F%BA%E6%9C%AC%E5%87%A0%E4%BD%95%E5%9B%BE%E5%BD%A2%E7%BB%98%E5%88%B6)
    - [1、画直线drawLine](#1%E7%94%BB%E7%9B%B4%E7%BA%BFdrawline)
    - [2、多条直线drawLines](#2%E5%A4%9A%E6%9D%A1%E7%9B%B4%E7%BA%BFdrawlines)
    - [3、点及多个点drawPoint、drawPoints](#3%E7%82%B9%E5%8F%8A%E5%A4%9A%E4%B8%AA%E7%82%B9drawpointdrawpoints)
    - [4、矩形drawRect、drawRoundRect](#4%E7%9F%A9%E5%BD%A2drawrectdrawroundrect)
    - [5、圆形drawCircle](#5%E5%9C%86%E5%BD%A2drawcircle)
    - [6、椭圆drawOval](#6%E6%A4%AD%E5%9C%86drawoval)
    - [7、圆弧drawArc](#7%E5%9C%86%E5%BC%A7drawarc)
  - [引用：](#%E5%BC%95%E7%94%A8)
- [路径（Path）](#%E8%B7%AF%E5%BE%84path)
  - [Path常用方法](#path%E5%B8%B8%E7%94%A8%E6%96%B9%E6%B3%95)
  - [Path方法使用详解](#path%E6%96%B9%E6%B3%95%E4%BD%BF%E7%94%A8%E8%AF%A6%E8%A7%A3)
    - [moveTo , lineTo , setLastPoint , close](#moveto--lineto--setlastpoint--close)
      - [1、lineTo](#1lineto)
      - [2、moveTo 和setLastPoint](#2moveto-%E5%92%8Csetlastpoint)
      - [3、close](#3close)
    - [quadTo,cubicTo](#quadtocubicto)
      - [1、quadTo](#1quadto)
      - [2、cubicTo](#2cubicto)
    - [addXxx和arcTo](#addxxx%E5%92%8Carcto)
      - [1、添加基本图形](#1%E6%B7%BB%E5%8A%A0%E5%9F%BA%E6%9C%AC%E5%9B%BE%E5%BD%A2)
      - [2、addPath](#2addpath)
      - [3、addArc与arcTo](#3addarc%E4%B8%8Earcto)
    - [isEmpty、 isRect、 set 和 offset](#isempty-isrect-set-%E5%92%8C-offset)
      - [isEmpty](#isempty)
      - [isRect](#isrect)
      - [set](#set)
      - [offset](#offset)
    - [FillType](#filltype)
      - [setFillType](#setfilltype)
        - [**WINDING**](#winding)
        - [**EVEN_ODD**](#even_odd)
        - [**INVERSE_WINDING**](#inverse_winding)
        - [**INVERSE_EVEN_ODD**](#inverse_even_odd)
      - [isInverseFillType](#isinversefilltype)
      - [toggleInverseFillType](#toggleinversefilltype)
  - [引用：](#%E5%BC%95%E7%94%A8-1)
- [文字（Text）](#%E6%96%87%E5%AD%97text)
  - [一、文字](#%E4%B8%80%E6%96%87%E5%AD%97)
    - [1、文本绘图样式](#1%E6%96%87%E6%9C%AC%E7%BB%98%E5%9B%BE%E6%A0%B7%E5%BC%8F)
    - [2、setTextAlign(Paint.Align align) 文字的对齐方式](#2settextalignpaintalign-align-%E6%96%87%E5%AD%97%E7%9A%84%E5%AF%B9%E9%BD%90%E6%96%B9%E5%BC%8F)
    - [3、文字样式设置](#3%E6%96%87%E5%AD%97%E6%A0%B7%E5%BC%8F%E8%AE%BE%E7%BD%AE)
    - [4、文字倾斜度设置](#4%E6%96%87%E5%AD%97%E5%80%BE%E6%96%9C%E5%BA%A6%E8%AE%BE%E7%BD%AE)
    - [5、水平拉伸设置](#5%E6%B0%B4%E5%B9%B3%E6%8B%89%E4%BC%B8%E8%AE%BE%E7%BD%AE)
  - [canvas绘制文字](#canvas%E7%BB%98%E5%88%B6%E6%96%87%E5%AD%97)
    - [1、drawText](#1drawtext)
    - [2、drawPosText](#2drawpostext)
    - [3、drawTextOnPath](#3drawtextonpath)
    - [4、Typeface（字体样式设置）](#4typeface%E5%AD%97%E4%BD%93%E6%A0%B7%E5%BC%8F%E8%AE%BE%E7%BD%AE)
      - [a、系统字体](#a%E7%B3%BB%E7%BB%9F%E5%AD%97%E4%BD%93)
      - [b、自定义字体](#b%E8%87%AA%E5%AE%9A%E4%B9%89%E5%AD%97%E4%BD%93)
  - [引用：](#%E5%BC%95%E7%94%A8-2)
- [baseLine和FontMetrics](#baseline%E5%92%8Cfontmetrics)
  - [一、baseLine 基线](#%E4%B8%80baseline-%E5%9F%BA%E7%BA%BF)
    - [1、canvas.drawText()](#1canvasdrawtext)
    - [2、mPaint.setTextAlign(Paint.Align.XXX);](#2mpaintsettextalignpaintalignxxx)
      - [（1）、Paint.Align.LEFT](#1paintalignleft)
      - [（2）、Paint.Align.CENTER](#2paintaligncenter)
      - [（3）、Paint.Align.RIGHT](#3paintalignright)
  - [二、FontMetrics](#%E4%BA%8Cfontmetrics)
    - [1、获取实例](#1%E8%8E%B7%E5%8F%96%E5%AE%9E%E4%BE%8B)
    - [2、成员变量](#2%E6%88%90%E5%91%98%E5%8F%98%E9%87%8F)
    - [3、绘制ascent，descent，top，bottom线](#3%E7%BB%98%E5%88%B6ascentdescenttopbottom%E7%BA%BF)
    - [4、绘制文字最小矩形、文字宽度、文字高度](#4%E7%BB%98%E5%88%B6%E6%96%87%E5%AD%97%E6%9C%80%E5%B0%8F%E7%9F%A9%E5%BD%A2%E6%96%87%E5%AD%97%E5%AE%BD%E5%BA%A6%E6%96%87%E5%AD%97%E9%AB%98%E5%BA%A6)
      - [（1）绘制文字最小矩形](#1%E7%BB%98%E5%88%B6%E6%96%87%E5%AD%97%E6%9C%80%E5%B0%8F%E7%9F%A9%E5%BD%A2)
      - [（2）文字高度](#2%E6%96%87%E5%AD%97%E9%AB%98%E5%BA%A6)
      - [（3）文字宽度](#3%E6%96%87%E5%AD%97%E5%AE%BD%E5%BA%A6)
    - [已知中线，获取baseline](#%E5%B7%B2%E7%9F%A5%E4%B8%AD%E7%BA%BF%E8%8E%B7%E5%8F%96baseline)
      - [结论](#%E7%BB%93%E8%AE%BA)
  - [引用：](#%E5%BC%95%E7%94%A8-3)
- [Canvas](#canvas-1)
  - [一、什么是Canvas？](#%E4%B8%80%E4%BB%80%E4%B9%88%E6%98%AFcanvas)
  - [二、Canvas 绘图](#%E4%BA%8Ccanvas-%E7%BB%98%E5%9B%BE)
  - [三、Canvas 的变换与操作](#%E4%B8%89canvas-%E7%9A%84%E5%8F%98%E6%8D%A2%E4%B8%8E%E6%93%8D%E4%BD%9C)
    - [1、平移（translate）](#1%E5%B9%B3%E7%A7%BBtranslate)
    - [屏幕显示与Canvas的关系](#%E5%B1%8F%E5%B9%95%E6%98%BE%E7%A4%BA%E4%B8%8Ecanvas%E7%9A%84%E5%85%B3%E7%B3%BB)
      - [总结：](#%E6%80%BB%E7%BB%93)
    - [2、旋转（Rotate）](#2%E6%97%8B%E8%BD%ACrotate)
    - [3、缩放（scale ）](#3%E7%BC%A9%E6%94%BEscale-)
        - [第一个构造函数：](#%E7%AC%AC%E4%B8%80%E4%B8%AA%E6%9E%84%E9%80%A0%E5%87%BD%E6%95%B0)
        - [第二个构造函数：](#%E7%AC%AC%E4%BA%8C%E4%B8%AA%E6%9E%84%E9%80%A0%E5%87%BD%E6%95%B0)
          - [原理](#%E5%8E%9F%E7%90%86)
    - [4、错切（skew）](#4%E9%94%99%E5%88%87skew)
    - [5、裁剪画布（clip系列函数）](#5%E8%A3%81%E5%89%AA%E7%94%BB%E5%B8%83clip%E7%B3%BB%E5%88%97%E5%87%BD%E6%95%B0)
    - [6、画布的保存与恢复（save()、restore()）](#6%E7%94%BB%E5%B8%83%E7%9A%84%E4%BF%9D%E5%AD%98%E4%B8%8E%E6%81%A2%E5%A4%8Dsaverestore)
    - [7、saveLayer](#7savelayer)
      - [**saveFlags**](#saveflags)
        - [**(1) MATRIX_SAVE_FLAG**](#1-matrix_save_flag)
          - [**save方法**](#save%E6%96%B9%E6%B3%95)
          - [**saveLayer方法**](#savelayer%E6%96%B9%E6%B3%95)
        - [**(2) CLIP_SAVE_FLAG**](#2-clip_save_flag)
        - [**(3) FULL_COLOR_LAYER_SAVE_FLAG 和 HAS_ALPHA_LAYER_SAVE_FLAG**](#3-full_color_layer_save_flag-%E5%92%8C-has_alpha_layer_save_flag)
        - [**(4) CLIP_TO_LAYER_SAVE_**](#4-clip_to_layer_save_)
  - [习题](#%E4%B9%A0%E9%A2%98)
    - [画一个表盘](#%E7%94%BB%E4%B8%80%E4%B8%AA%E8%A1%A8%E7%9B%98)
    - [画一个正方形螺旋图](#%E7%94%BB%E4%B8%80%E4%B8%AA%E6%AD%A3%E6%96%B9%E5%BD%A2%E8%9E%BA%E6%97%8B%E5%9B%BE)
  - [引用：](#%E5%BC%95%E7%94%A8-4)
- [综合习题](#%E7%BB%BC%E5%90%88%E4%B9%A0%E9%A2%98)
  - [实现图片圆角带边框的效果](#%E5%AE%9E%E7%8E%B0%E5%9B%BE%E7%89%87%E5%9C%86%E8%A7%92%E5%B8%A6%E8%BE%B9%E6%A1%86%E7%9A%84%E6%95%88%E6%9E%9C)
    - [引用：](#%E5%BC%95%E7%94%A8-5)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->



# 【Android 自定义View之绘图】
[TOC]
# 基础图形的绘制

## 一、Paint与Canvas

绘图需要两个工具，笔和纸。这里的 `Paint`相当于笔，而 `Canvas`相当于纸，不过需要注意的是 `Canvas`（画布）无限大，没有边界，切记理解成只有屏幕大小。我这里打个比方， `Canvas`是整个天空，而屏幕是通过窗户看到的景色。

那么我需要改变**画笔大小，粗细，颜色，透明度**，字体样式等都需要在 `Paint`里面设置；
同样要画出**圆形，矩形，不规则形状**都是在 `Canvas`里面操作的。

### Paint

#### Paint的基本设置函数

1. mPaint.setAntiAlias(true) //设置是否抗锯齿;
2. mPaint.setStyle(Paint.Style.FILL_AND_STROKE); //设置填充样式
3. mPaint.setColor(Color.GREEN);//设置画笔颜色
4. mPaint.setStrokeWidth(2);//设置画笔宽度
5. mPaint.setShadowLayer(10,15,15,Color.RED);//设置阴影

#### 1 、setAntiAlias(true) 设置是否抗锯齿

设置抗锯齿会使图像边缘更清晰一些，锯齿痕迹不会那么明显。

![](http://upload-images.jianshu.io/upload_images/9028834-842aa6fc510f3506?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 2、setStyle (Paint.Style style) 设置填充样式

`Paint.Style` 类型：

`Paint.Style.FILL_AND_STROKE` 填充且描边 
`Paint.Style.STROKE` 描边 
`Paint.Style.FILL` 填充

看下上面三种类型，这里以矩形为例：

![](http://upload-images.jianshu.io/upload_images/9028834-94680eb3c27c71d9?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 3、setColor(@ColorInt int color) 设置画笔颜色



#### 4、setStrokeWidth(float width) 设置画笔宽度



#### 5、setShadowLayer(float radius, float dx, float dy, int shadowColor) 设置阴影

先来看看参数代表的含义：

**radius** ： 表示阴影的倾斜度 
**dx** ： 水平位移 
**dy** : 垂直位移 
**shadowColor** : 阴影颜色

看一个简单的例子：

```java
paint.setShadowLayer(5,10,10,Color.parseColor("#abc133"));
```

效果图：

![](http://upload-images.jianshu.io/upload_images/9028834-7a47463e2a6ab302?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这里你可能有疑问，为啥我自己演示了一篇却看不到矩形，圆形等图形的阴影，只能看到文本的阴影呢？那么我们需要注意的是：**这个方法不支持硬件加速，所以我们要测试时必须先关闭硬件加速。**

那么请加上`setLayerType(LAYER_TYPE_SOFTWARE, null);` 并且确保你的最小`api8`以上。

### Canvas
下文【Canvas详细讲解】有Canvas进一步说明

**画布背景**设置：

```java
canvas.drawColor(Color.BLUE);
canvas.drawRGB(255, 255, 0);  
```

这两个功能一样，都是用来设置背景颜色的。

我们只需要重写`onDraw(Canvas canvas)`方法，就可以绘制你想要的图形了。

```java
@Override
protected void onDraw(Canvas canvas) {
    super.onDraw(canvas);
     //绘制图形
}
```

## 二、基本几何图形绘制

### 1、画直线drawLine

方法预览：

```java
drawLine(float startX, float startY, float stopX, float stopY, @NonNull Paint paint)
```

参数：

startX ： 开始点X坐标 
startY ： 开始点Y坐标 
stopX ： 结束点X坐标 
stopY ： 结束点Y坐标

```java
paint.setStyle(Paint.Style.FILL);
paint.setStrokeWidth(5);    
paint.setColor(Color.parseColor("#FF0000"));
canvas.drawLine(100,100,600,600,paint);
```

![](http://upload-images.jianshu.io/upload_images/9028834-b3599780e7e5ebb2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 2、多条直线drawLines

方法预览：
```java
drawLines(@Size(min=4,multiple=2) @NonNull float[] pts, int offset, int count, Paint paint)
drawLines(@Size(min=4,multiple=2) @NonNull float[] pts, @NonNull Paint paint)
```

参数：

**pts** : 是点的集合且**大小最小为4而且是2的倍数**。表示每2个点连接形成一条直线，pts 的组织方式为{x1,y1,x2,y2….} 
**offset** : 集合中跳过的数值个数，注意不是点的个数！一个点是两个数值 
**count** : 参与绘制的数值的个数，指pts[]里数值个数，而不是点的个数，因为一个点是两个数值

还是来看个例子：

```java
@Override
protected void onDraw(Canvas canvas) {
    Paint paint = new Paint();
    paint.setAntiAlias(true);
    paint.setStyle(Paint.Style.FILL);
    paint.setStrokeWidth(5);    
    
    float [] pts={50,100,100,200,200,300,300,400};
    paint.setColor(Color.RED);
    canvas.drawLines(pts,paint);

    paint.setColor(Color.BLUE);
    canvas.drawLines(pts,1,4,paint);//去掉第一个数50，取之后的4个数即100,100,200,200
}
```
红线：点（50,100）和点（100,200）连接成一条直线；点（200,300）和点（300,400）连接成直线。
蓝线：点（100,100）和点（200,200）连接成一条直线；
![](http://upload-images.jianshu.io/upload_images/9028834-953e9a50b015d4d3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 3、点及多个点drawPoint、drawPoints

方法预览：

```java
drawPoint(float x, float y, @NonNull Paint paint)

drawPoints(@Size(multiple=2) @NonNull float[] pts, @NonNull Paint paint)
drawPoints(@Size(multiple=2) @NonNull float[] pts, int offset, int count, @NonNull Paint paint)

```
点的绘制和上面直线的绘制一样，我这里就不再累诉了。

### 4、矩形drawRect、drawRoundRect

方法预览：

```java
drawRect(@NonNull RectF rect, @NonNull Paint paint)

drawRect(@NonNull Rect r, @NonNull Paint paint)

drawRect(float left, float top, float right, float bottom, @NonNull Paint paint)
```

区别`RectF` 与`Rect` ，**RectF**坐标系是**浮点型**；**Rect**坐标系是**整形**。

圆角矩形方法预览：

```java
drawRoundRect(@NonNull RectF rect, float rx, float ry, @NonNull Paint paint)

drawRoundRect(float left, float top, float right, float bottom, float rx, float ry, @NonNull Paint paint)
```

参数：
RectF ： 绘制的矩形 
rx ： 生成圆角的椭圆X轴半径 
ry ： 生成圆角的椭圆Y轴的半径

```java
RectF rect = new RectF(100, 10, 500, 300);
canvas.drawRoundRect(rect, 60, 20, paint);
```
![](http://upload-images.jianshu.io/upload_images/9028834-cb0e1f3ac77724af.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 5、圆形drawCircle

方法预览：

```java
drawCircle(float cx, float cy, float radius, @NonNull Paint paint)
```

参数：

cx ： 圆心X坐标 
cy ： 圆心Y坐标 
radius ： 半径

```java
canvas.drawCircle(400,400,300,paint);
```

![](http://upload-images.jianshu.io/upload_images/9028834-a2002a13f118a06b?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 6、椭圆drawOval

方法预览：

``` java
drawOval(@NonNull RectF oval, @NonNull Paint paint)

drawOval(float left, float top, float right, float bottom, @NonNull Paint paint)
```

![](http://upload-images.jianshu.io/upload_images/9028834-d31bdcdd3309b008?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 7、圆弧drawArc

方法预览：

```java
drawArc(@NonNull RectF oval, float startAngle, float sweepAngle, boolean useCenter, @NonNull Paint paint)

drawArc(float left, float top, float right, float bottom, float startAngle, float sweepAngle,
            boolean useCenter, @NonNull Paint paint)
```

参数：
oval ： 生成椭圆的矩形 
**startAngle** ： 弧开始的角度 （X轴正方向为0度，**顺时针弧度增大**） 
**sweepAngle** ： 绘制多少**弧度** （注意不是结束弧度） 
**useCenter** ： 是否有**弧的两边** true有两边 false无两边

画笔设置填充：

```java
RectF rect=new RectF(0,0,300,400);

paint.setStyle(Paint.Style.FILL);

paint.setColor(Color.RED);
canvas.drawArc(rect,30,30,false,paint);

paint.setColor(Color.BLUE);
canvas.drawArc(rect,120,30,true,paint);

paint.setStyle(Paint.Style.STROKE);

paint.setColor(Color.GREEN);
canvas.drawArc(rect,0,360,true,paint);

paint.setColor(Color.RED);
canvas.drawArc(rect,-30,30,false,paint);

paint.setColor(Color.BLUE);
canvas.drawArc(rect,-120,30,true,paint);
```
![](http://upload-images.jianshu.io/upload_images/9028834-b99e99b628fb8c25.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

说明：
![](http://upload-images.jianshu.io/upload_images/9028834-6681c82193213405.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 引用：
[自定义View之绘图](http://blog.csdn.net/u012551350/article/details/51323986)


# 路径（Path）

## Path常用方法

| 方法                                       | 作用     | 备注                                       |
| ---------------------------------------- | ------ | ---------------------------------------- |
| **moveTo**                               | 移动起点   | 移动下一次操作的起点位置                             |
| **lineTo**                               | 连接直线   | 连接上一个点到当前点之间的直线                          |
| **setLastPoint**                         | 设置终点   | 重置最后一个点的位置                               |
| **close**                                | 闭合路劲   | 从最后一个点连接最初的一个点，形成一个闭合区域                  |
| **addRect**                              | 添加矩形   | 添加矩形到当前Path                              |
| **addRoundRect**                         | 添加圆角矩形 | 添加圆角矩形到当前Path                            |
| **addOval**                              | 添加椭圆   | 添加椭圆到当前Path                              |
| **addCircle**                            | 添加圆    | 添加圆到当前Path                               |
| **addPah**                               | 添加路劲   | 添加路劲到当前Path                              |
| **addArc**                               | 添加圆弧   | 添加圆弧到当前Path                              |
| **arcTo**                                | 圆弧     | 绘制圆弧，注意和addArc的区别                        |
| **isEmpty**                              | 是否为空   | 判定Path是否为空                               |
| **isRect**                               | 是否为矩形  | 判定Path是否是一个矩形                            |
| **set**                                  | 替换路劲   | 用新的路劲替换当前路劲的所有内容                         |
| **offset**                               | 偏移路劲   | 对当前的路劲进行偏移                               |
| **quadTo**                               | 贝塞尔曲线  | 二次贝塞尔曲线的方法                               |
| **cubicTo**                              | 贝塞尔曲线  | 三次贝塞尔曲线的方法                               |
| **rMoveTo<br>rlineTo<br>rQuadTo<br>rCubicTo** | rXxx方法 | 不带r的方法是基于原点坐标系（偏移量），带r的基于当前点坐标系（偏移量）     |
| **op**                                   | 布尔操作   | 对两个Path进行布尔运算（交集，并集）等操作                  |
| **setFillType**                          | 填充模式   | 设置Path的填充模式                              |
| **getFillType**                          | 填充模式   | 获取Path的填充                                |
| **isInverseFillType**                    | 是否逆填充  | 判断是否是逆填充模式                               |
| **toggleInverseFillType**                | 相反模式   | 切换相反的填充模式                                |
| **getFillType**                          | 填充模式   | 获取Path的填充                                |
| **incReserve**                           | 提示方法   | 提示Path还有多少个点等待加入                         |
| **computeBounds**                        | 计算边界   | 计算Path的路劲                                |
| **reset，rewind**                         | 重置路劲   | 清除Path中的内容（reset相当于new Path , rewind 会保留Path的数据结构） |
| **transform**                            | 矩阵操作   | 矩阵变换                                     |

## Path方法使用详解

使用Path不仅可以绘制简单的图形（如圆形，矩形，直线等），也可以绘制复杂一些的图形（如正多边形，五角星等），还有绘制裁剪和绘制文本都会用到Path。由于方法比较多，我这里分组来讲下。

### moveTo , lineTo , setLastPoint , close

先创建画笔：

``` java
paint = new Paint();
paint.setAntiAlias(true);
paint.setStyle(Paint.Style.STROKE);
paint.setStrokeWidth(10);
paint.setColor(Color.parseColor("#FF0000"));
```

**注意`paint.setStyle(Paint.Style.FILL);`，设置画笔为实心。一些线条将在画布上看不见。**

#### 1、lineTo

首先我们来看看`lineTo`,如果你直接`moveTo` 将看不出效果。

``` java
Path path = new Path();

path.lineTo(200,200);
path.lineTo(400,0);

canvas.drawPath(path,paint);
```


![](http://upload-images.jianshu.io/upload_images/9028834-574dde4fdbd6c585?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

为了方便大家好观察坐标的变化，我在屏幕上画出了网格，每块网格的宽高都是100。由于**第一次**之前没有过操作，所以**默认点**就是**原点**（屏幕左上角），第一次lineTo就是坐标原点到(200,200)之间的直线。**第二次**lineTo就是**上一次结束点**位置(200,200)到(400,0)点之间的直线。

#### 2、moveTo 和setLastPoint

方法预览：

``` java
moveTo(float x, float y) 

setLastPoint(float dx, float dy)
```

这两个方法在作用上有相似之处，却是两个不同的东西，具体参考下表：

| 方法名              |      作用      | 是否影响之前的操作 | 是否影响之后的操作 |
| ---------------- | :----------: | :-------: | :-------: |
| **moveTo**       | 移动下一次操作的起点位置 |     否     |     是     |
| **setLastPoint** | 改变上一次操作点的位置  |     是     |     是     |



来看看下面的例子：

``` java
Path path = new Path();

path.lineTo(200, 200);
path.moveTo(300,300);//moveTo
path.lineTo(400, 0);

canvas.drawPath(path, paint);
```

效果图：
![](http://upload-images.jianshu.io/upload_images/9028834-68174ee0295898e7?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

``` java
Path path = new Path();

path.lineTo(200, 200);
path.setLastPoint(300,100);//setLastPoint
path.lineTo(400, 0);

canvas.drawPath(path, paint);
```

效果图：
![](http://upload-images.jianshu.io/upload_images/9028834-10b612d4dbbaccf6?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

当我们绘制线条之前，调用`moveTo` 和 `setLastPoint`效果是一样的，都是对坐标原点(0,0)进行操作。
**setLastPoint**是**重置上一次操作的最后一点**，在执行完第一次lineTo的时候，最后一个点就是(200,200),setLastPoint更改(200,200)为(300,100)，所以在执行的时候就是(300,100)到(400, 0)之间的连线了。

#### 3、close

方法预览

``` java
public void close()
```

`close`方法**连接最后一个点和最初一个点**（如果两个点不重合）形成一个**闭合**的**图形**。

``` java
path.moveTo(100,100);
path.lineTo(500,100);
path.lineTo(300,400);
path.close();
canvas.drawPath(path, paint);
```

效果图：
![](http://upload-images.jianshu.io/upload_images/9028834-4bc5e18f75a23014.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上图中可以看到`lineTo(500,100)`直线和`lineTo(300,400)`直线，而`close`方法就是连接(300,400)，(100,100)两点，形成一个闭合的区域。

注意：close的作用的封闭路径，如果连接**最后一个点和最初一个点任然无法形成闭合的区域，那么close什么也不做**。

### quadTo,cubicTo

二次贝塞尔曲线以及三次贝塞尔曲线。

#### 1、quadTo

方法预览

``` java
public void quadTo(float x1, float y1, float x2, float y2)
```

`quadTo`方法其中 (x1,y1) 为控制点，(x2,y2)为结束点。

``` java
path.moveTo(100,400);
path.quadTo(300, 100, 400, 400);

canvas.drawPath(path, paint);
```

效果图：
![](http://upload-images.jianshu.io/upload_images/9028834-418f2c31c4fa03d0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 2、cubicTo

方法预览：

``` java
public void cubicTo(float x1, float y1, float x2, float y2, float x3, float y3)
```

`cubicTo`方法比`quadTo`方法多了一个点坐标，那么其中(x1,y1) 为控制点，(x2,y2)为控制点，(x3,y3) 为结束点。

``` java
path.moveTo(100, 400);
path.cubicTo(100, 400, 300, 100, 400, 400);

canvas.drawPath(path, paint);
```

绘制的图形和上面的`quadTo`绘制的图形是一样的。我们去掉`moveTo`来看看运行的效果图：

![](http://upload-images.jianshu.io/upload_images/9028834-2d7fd3afd51d5a48?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果你想了解[贝塞尔曲线公式，请链接这里](http://baike.baidu.com/link?url=jRgMYu81F4aEnyyWXUB65Ihj0fV6rbDhzEZhO4xE6ZU7r98F-fzRewd4-Os88iX_gBlHguZWeB4k97VGKmqvVq)

### addXxx和arcTo

主要是向`Path`中添加基本图形以及区分`addArc`和`arcTo`

#### 1、添加基本图形

方法预览：

``` java
//圆形
addCircle(float x, float y, float radius, Path.Direction dir)
//椭圆
addOval(RectF oval, Path.Direction dir)
addOval(float left, float top, float right, float bottom, Path.Direction dir)
//矩形
addRect(RectF rect, Path.Direction dir)
addRect(float left, float top, float right, float bottom, Path.Direction dir)
//圆角矩形
addRoundRect(RectF rect, float rx, float ry, Path.Direction dir) 
addRoundRect(float left, float top, float right, float bottom, float rx, float ry, Path.Direction dir)
addRoundRect(RectF rect, float[] radii, Path.Direction dir)
addRoundRect(float left, float top, float right, float bottom, float[] radii, Path.Direction dir)
```

我们仔细观察上面的方法，在最后都有一个`Path.Direction`，这是个什么东东呢？
**Direction**的意思是方向，指导，趋势。点进去跟一下你会发现Direction是一个枚举类型（Enum）分别有**CW（顺时针）**，**CCW（逆时针）**两个常量。那么它的作用主要有以下两点：

| 序号    |           作用           |
| ----- | :--------------------: |
| **1** | 在添加图形时确定闭合顺序(各个点的记录顺序) |
| **2** |     对自相交图形的渲染结果有影响     |


我们先来看看闭合顺序的问题，添加一个矩形看看：

``` java
path.addRect(100, 200, 500, 400, Path.Direction.CW);

canvas.drawPath(path, paint);
```

![](http://upload-images.jianshu.io/upload_images/9028834-b8f7e6c068f3c358?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我将上面的代码`CW`改成`CCW`再运行一次，结果一模一样。
想看到区别就要用到`setLastPoint`（重置最后一个点的坐标）。我们来这样变变代码：

``` java
path.addRect(100, 200, 500, 400, Path.Direction.CW);
path.setLastPoint(200,400);

canvas.drawPath(path, paint);
```

效果立马现行：

![](http://upload-images.jianshu.io/upload_images/9028834-53f54e2e3835c59c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

为什么图形会发生奇怪的变化呢。我们先来分析一下，绘制一个矩形至少需要对角线的两个点，根据这两个点计算出四条边然后把四条边按照顺序连接起来。上图的起始坐标是（100,200）按着顺时针的方向连接（500,200），（500,400），（100,400）最后连接（100,200）形成一个矩形。`setLastPoint`是重置上一个操作点坐标及改变（100,400）为（200,400），所以出现了上图的效果。

接下来我们看看逆时针的情况：

``` java
path.addRect(100, 200, 500, 400, Path.Direction.CCW);
path.setLastPoint(400,300);

canvas.drawPath(path, paint);
```

效果图：

![](http://upload-images.jianshu.io/upload_images/9028834-7e9a3654df3c1320?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们理清楚了闭合的问题，相交问题与设置填充模式有关。

我以`addCircle`方法来讲解添加图形

``` java
path.addCircle(300,300,200, Path.Direction.CW);//（300,300）点表示圆心坐标，200 表示半径长度

canvas.drawPath(path, paint);
```
![](http://upload-images.jianshu.io/upload_images/9028834-1ee02d1c7d16ac9f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```java
path.addCircle(300, 300, 200, Path.Direction.CCW);//（300,300）点表示圆心坐标，200 表示半径长度
path.setLastPoint(300,400);
canvas.drawPath(path, paint);
```
![](http://upload-images.jianshu.io/upload_images/9028834-cdee00372a7a5b56.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 2、addPath

方法预览：

``` java
public void addPath(Path src)
public void addPath(Path src, float dx, float dy)//`dx,dy`指的是偏移量
public void addPath(Path src, Matrix matrix)//添加到当前path之前先使用Matrix进行变换
```

**addPath**方法就是将两个路径合并到一起。
``` java
Path path = new Path();
path.addRect(100,100,400,300, Path.Direction.CW);

Path src=new Path();
src.addCircle(300,300,100, Path.Direction.CW);
path.addPath(src,0,100);

canvas.drawPath(path, paint);
```

效果图：
![](http://upload-images.jianshu.io/upload_images/9028834-4ea3da589e881dd6?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 3、addArc与arcTo

方法预览：

``` java
addArc(RectF oval, float startAngle, float sweepAngle)
addArc(float left, float top, float right, float bottom, float startAngle, float sweepAngle)

arcTo(RectF oval, float startAngle, float sweepAngle)
arcTo(RectF oval, float startAngle, float sweepAngle, boolean forceMoveTo)
arcTo(float left, float top, float right, float bottom, float startAngle, float sweepAngle, boolean forceMoveTo)
```

从方法名字上面看，这两个方法都是与圆弧有关，那么他们之间肯定是有区别的：

| 名称         |     作用      |                    区别                    |
| ---------- | :---------: | :--------------------------------------: |
| **addArc** | 添加一个圆弧到Path |       直接添加一个圆弧到path中，**和上一次操作点无关**       |
| **arcTo**  | 添加一个圆弧到Path | 添加一个圆弧到path中，如果圆弧的起点和**上次操作点坐标不同就连接两个点** |


`startAngle`表示开始圆弧度数（0度与X轴方向对齐，顺时针移动，弧度增大）。
`sweepAngle`表示运动了多少弧度，并不是结束弧度。

**forceMoveTo**表示“是否强制使用moveTo”，也就是说是否使用moveTo将上一次操作点移动到圆弧的起点坐标。默认是false。

| forceMoveTo |                  含义                   |
| ----------- | :-----------------------------------: |
| **true**    |     将最后一个点移动到圆弧起点，即不连接最后一个点与圆弧起点      |
| **false**   | 不移动，而是连接最后一个点与圆弧起点(注意之前没有操作的话，不会连接原点) |


示例：

``` java
path.lineTo(200, 200);
RectF rectF = new RectF(100, 100, 400, 400);
path.arcTo(rectF, 0, 270, true);
// path.addArc(rectF,0,270);和上面一句等价

canvas.drawPath(path, paint);
```
效果图：
![](http://upload-images.jianshu.io/upload_images/9028834-847ac9c0bf8ec0ee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


我们把 `path.arcTo(rectF, 0, 270, true);`改成 `path.arcTo(rectF, 0, 270, false);`，来看看效果图：
![](http://upload-images.jianshu.io/upload_images/9028834-dc7bdcb74c512135.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

从上面两张图可以看出明显的变化。

### isEmpty、 isRect、 set 和 offset

#### isEmpty

判断path中是否包含内容。

``` java
Path path = new Path();
Log.e("-----","----"+path.isEmpty());//-----: ----true
path.lineTo(100,100);
Log.e("-----","----"+path.isEmpty());//-----: ----false

canvas.drawPath(path, paint);
```

#### isRect

方法预览：

``` java
isRect(RectF rect)
```

判断path是否是一个矩形，如果是一个矩形的话，会将矩形的信息存放进参数rect中。

``` java
Path path = new Path();
RectF rectF = new RectF();
rectF.left = 100;
rectF.top = 100;
rectF.right = 400;
rectF.bottom = 300;
path.addRect(rectF, Path.Direction.CW);
boolean isRect = path.isRect(rectF);
Log.e("-----","------"+isRect);//-----: ------true
```

#### set

方法预览：

``` java
public void set(Path src)
```

将新的path赋值到现有path。相当于运算符中的“=”，如`a=b`,把`b`赋值给`a`

还是一起来看个例子：

``` java
Path path = new Path();
path.addRect(100,100,400,300, Path.Direction.CW);
Path src=new Path();
src.addCircle(300,200,100, Path.Direction.CW);
path.set(src);
canvas.drawPath(path, paint);
```

效果图：
![](http://upload-images.jianshu.io/upload_images/9028834-30b9ebd268b5acb6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### offset

方法预览：

``` java
public void offset(float dx, float dy)
public void offset(float dx, float dy, Path dst)
```

这个方法就是对`Path`进行一段**平移**，正方向和X轴，Y轴方向一致（如果dx为正数则向右平移，反之向左平移；如果dy为正则向下平移，反之向上平移）。
我们看到第二个方法多了一个`dst`，这个又是一个什么玩意呢，其实参数**dst**是**存储平移后的path**的。

用例子来说明一下：

``` java
Path path = new Path();
path.addCircle(300, 200, 100, Path.Direction.CW);

Path dst = new Path();
dst.addCircle(500, 200, 200, Path.Direction.CW);

path.offset(-100, 100, dst);

paint.setColor(Color.RED);
canvas.drawPath(path, paint);

paint.setColor(Color.BLUE);
canvas.drawPath(dst, paint);
```

效果图：
![](http://upload-images.jianshu.io/upload_images/9028834-5e2f35dc42be9405.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
从运行效果图可以看出，虽然我们在dst中添加了一个圆形，但是并没有表现出来，所以，当dst中存在内容时，**dst中原有的内容会被清空**，而**存放平移后的path**。
而**原来的path并没有变化**。

### FillType

方法预览：

``` java
public void setFillType(Path.FillType ft)
public Path.FillType getFillType()
```

`setFillType`方法中的参数`Path.FillType`为枚举类型：

| FillType值                     |       含义        |
| ----------------------------- | :-------------: |
| **FillType.WINDING**          | 取path所有所在区域 默认值 |
| **FillType.EVEN_ODD**         |  取path所在并不相交区域  |
| **FillType.INVERSE_WINDING**  |   取path所有未占区域   |
| **FillType.INVERSE_EVEN_ODD** |  取path未占或相交区域   |

#### setFillType

##### **WINDING**

``` java
Path path = new Path();
path.addCircle(300,200,100, Path.Direction.CW);
path.addCircle(200,200,100, Path.Direction.CW);
path.setFillType(Path.FillType.WINDING);
canvas.drawPath(path, paint);
```

效果图：
![](http://upload-images.jianshu.io/upload_images/9028834-694e76b1d8ecb72c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


##### **EVEN_ODD**
![](http://upload-images.jianshu.io/upload_images/9028834-a32243dc688659db?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### **INVERSE_WINDING**

![](http://upload-images.jianshu.io/upload_images/9028834-4657b536e2e64ef5?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### **INVERSE_EVEN_ODD**

![](http://upload-images.jianshu.io/upload_images/9028834-2b40a26e92bdbea0?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### isInverseFillType

是否是逆填充模式：
WINDING 和 EVEN_ODD 返回false； 
INVERSE_WINDING 和 INVERSE_EVEN_ODD 返回true；

#### toggleInverseFillType

切换相反的填充模式，如果填充模式为`WINDING`则填充模式为`INVERSE_WINDING`，反之为`WINDING`模式；如果填充模式为`EVEN_ODD`则填充模式为`INVERSE_EVEN_ODD`，反之为`EVEN_ODD`模式。

举个例子：

``` java
Path path = new Path();
path.addCircle(300,200,100, Path.Direction.CW);
path.addCircle(200,200,100, Path.Direction.CW);
path.setFillType(Path.FillType.INVERSE_EVEN_ODD);
path.toggleInverseFillType();
canvas.drawPath(path, paint);
```

效果图和上面`EVEN_ODD`模式一模一样。
![](http://upload-images.jianshu.io/upload_images/9028834-e2889119a6588692?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 引用：
[自定义View之绘图篇（二）：路径（Path）](http://blog.csdn.net/u012551350/article/details/51245793)

# 文字（Text）

## 一、文字

相关方法预览：
``` java
//普通设置
paint.setAntiAlias(true); //指定是否使用抗锯齿功能  如果使用会使绘图速度变慢 默认false
setStyle(Paint.Style.FILL);//绘图样式  对于设文字和几何图形都有效  
setTextAlign(Align.LEFT);//设置文字对齐方式  取值：align.CENTER、align.LEFT或align.RIGHT 默认align.LEFT
paint.setTextSize(12);//设置文字大小

//样式设置  
paint.setFakeBoldText(true);//设置是否为粗体文字  
paint.setUnderlineText(true);//设置下划线  
paint.setTextSkewX((float) -0.25);//设置字体水平倾斜度  普通斜体字是-0.25  
paint.setStrikeThruText(true);//设置带有删除线效果 

//其它设置  
paint.setTextScaleX(2);//只会将水平方向拉伸  高度不会变  
```

### 1、文本绘图样式

先来看看下面这个例子：

``` java
mPaint.setStrokeWidth(5);
mPaint.setTextSize(80);
//设置绘图样式 为填充
mPaint.setStyle(Paint.Style.FILL);
canvas.drawText("我是一颗小小的石头", 100,100, mPaint);

//设置绘图样式 为描边
mPaint.setStyle(Paint.Style.STROKE);
canvas.drawText("我是一颗小小的石头", 100,300, mPaint);

//设置绘图样式 为填充且描边
mPaint.setStyle(Paint.Style.FILL_AND_STROKE);
canvas.drawText("我是一颗小小的石头", 100,500, mPaint);
```

效果图：
![](http://upload-images.jianshu.io/upload_images/9028834-60dde666d7eaec04.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 2、setTextAlign(Paint.Align align) 文字的对齐方式

``` java
mPaint.setStyle(Paint.Style.FILL);
mPaint.setTextSize(80);
//设置对齐方式  左对齐
mPaint.setTextAlign(Paint.Align.LEFT);
canvas.drawText("小小的石头", 500,100, mPaint);//点（500,100）在文本的左边

//设置对齐方式  中间对齐
mPaint.setTextAlign(Paint.Align.CENTER);
canvas.drawText("小小的石头", 500,200, mPaint);//点（500,100）在文本的中间

//设置对齐方式  右对齐
mPaint.setTextAlign(Paint.Align.RIGHT);
canvas.drawText("小小的石头", 500,300, mPaint);//点（500,100）在文本的右边
```
![](http://upload-images.jianshu.io/upload_images/9028834-835660280911a92f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 3、文字样式设置

``` java
canvas.drawText("小小的石头", 200, 100, mPaint); //不带任何效果

mPaint.setFakeBoldText(true);//是否粗体文字
mPaint.setUnderlineText(true);//设置下划线
mPaint.setStrikeThruText(true);//设置删除线效果
canvas.drawText("小小的石头", 200, 200, mPaint);
```
![](http://upload-images.jianshu.io/upload_images/9028834-0096e6f553395589?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 4、文字倾斜度设置

``` java
mPaint.setTextSkewX(-0.25f);
canvas.drawText("小小的石头", 100, 100, mPaint);

mPaint.setTextSkewX(0.25f);
canvas.drawText("小小的石头", 100, 200, mPaint);

mPaint.setTextSkewX(-0.5f);
canvas.drawText("小小的石头", 100, 300, mPaint);
```
![](http://upload-images.jianshu.io/upload_images/9028834-4742b4de15f87b2b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可见普通斜体字是`-0.25f`，大于`-0.25f` 向左倾斜，小于 `-0.25f` 向右倾斜。

### 5、水平拉伸设置

``` java
mPaint.setTextScaleX(1);//不拉伸
canvas.drawText("小小的石头", 100, 100, mPaint);

mPaint.setTextScaleX(2);//水平方向拉伸2倍
canvas.drawText("小小的石头", 100, 200, mPaint);

mPaint.setTextScaleX(3);//水平方向拉伸3倍
canvas.drawText("小小的石头", 100, 300, mPaint);
```
![](http://upload-images.jianshu.io/upload_images/9028834-f60c78ea567e447a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

由上可以发现，仅是水平方向拉伸，高度并未改变。

## canvas绘制文字

### 1、drawText

方法预览：

``` java
drawText(String text, float x, float y, Paint paint)

drawText(char[] text, int index, int count, float x, float y, Paint paint)
		//text 字节数组；index 表示第一个要绘制的文字索引；count 需要绘制的文字个数

drawText(CharSequence text, int start, int end, float x, float y, Paint paint)
//text 表示字符；start 开始截取字符的索引号；end 结束截取字符的索引号。[start , end ) 包含 start 但不包含 end

//`drawTextRun`方法是在 **skd23** 才引入的方法
drawTextRun(char[] text, int index, int count, int contextIndex, int contextCount, float x, float y, boolean isRtl, Paint paint)//isRtl 表示排列顺序，true 表示正序，false 表示倒序

drawTextRun(CharSequence text, int start, int end, int contextStart, int contextEnd, float x, float y, boolean isRtl, Paint paint)
```

第一个构造函数 ： 是最普通的。 
第二个构造函数 ： text 字节数组；index 表示第一个要绘制的文字索引；count 需要绘制的文字个数。 
第三个构造函数 ： text 表示字符 （注意与上面比较）；start 开始截取字符的索引号；end 结束截取字符的索引号。（注意和上面的区别） [start , end ) 包含 start 但不包含 end

第四个构造函数和第五个构造函数 ： contextIndex 和 index 相同 ； contextCount 大于等于 count ； isRtl 表示排列顺序，true 表示正序，false 表示倒序（这里的倒是指第一个字符变到最后一个字符，最后一个字符变到第一个字符）。 注意了`drawTextRun`方法是在 **skd23** 才引入的方法。

``` java
canvas.drawText("我是一颗小小的石头".toCharArray(), 1, 4, 100, 100, mPaint);

canvas.drawText("我是一颗小小的石头", 1, 4, 100, 200, mPaint);

//最小sdk23
canvas.drawTextRun("我是一颗小小的石头".toCharArray(), 1, 4, 1, 4, 100, 300, true, mPaint);

canvas.drawTextRun("我是一颗小小的石头".toCharArray(), 1, 4, 1, 4, 100, 400, false, mPaint);
```
![](http://upload-images.jianshu.io/upload_images/9028834-e7d881a57942fd2c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 2、drawPosText

方法预览：

``` java
drawPosText(String text, float[] pos, Paint paint)

drawPosText(char[] text, int index, int count, float[] pos, Paint paint)
```

这里的参数含义和 `drawText` 方法的参数一样。我们来看个简单的例子：

``` java
float[] pos = {100, 100, 200, 200, 300, 300, 400, 400, 500, 500, 600, 600};
canvas.drawPosText("我是一颗小小", pos, mPaint);
```

![](http://upload-images.jianshu.io/upload_images/9028834-a1957986f79301b7?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 3、drawTextOnPath

方法预览：

``` java
drawTextOnPath(String text, Path path, float hOffset, float vOffset, Paint paint)

drawTextOnPath(char[] text, int index, int count, Path path, float hOffset, float vOffset, Paint paint)
```

参数含义：

- index，count ： 和上面截取参数含义一样，这里不再累诉。

- **hOffset** ： 与路径**起点**的**水平偏移量**。
  正数向 X 轴正方形移动（右移）；负数向 X 轴负方向移动（左移）；
  如果是圆弧：正数是顺时针的偏移量；反之是逆时针的偏移量

- **vOffset** ： 与路径**中心**的**垂直偏移量**。
  正数向 Y 轴正方形移动（下移）；负数向 Y 轴负方向移动（上移）
  如果是圆弧：正数是向圆心移动；反之是远离圆心

``` java
mPath.moveTo(100,100);
mPath.lineTo(800,100);
canvas.drawTextOnPath("我是一颗小小的石头",mPath,10,-10,mPaint);
```
![](http://upload-images.jianshu.io/upload_images/9028834-6b4f3f3f7749cce5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

路径为圆弧的例子：
``` java
mPath.addCircle(500,500,200, Path.Direction.CW);
canvas.drawTextOnPath("我是一颗小小的石头",mPath,40,-20,mPaint);
```

![11](http://upload-images.jianshu.io/upload_images/9028834-6d7f42ff5ea25238?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 4、Typeface（字体样式设置）

方法预览：

``` java
setTypeface(Typeface typeface)
```

`Typeface`是用来设置字体样式的，通过`paint.setTypeface()`来指定。可以指定系统中的字体样式，也可以指定自定义的样式文件中获取。
要构建Typeface时，可以指定所用样式的正常体、斜体、粗体等，如果指定样式中，没有相关文字的样式就会用系统默认的样式来显示，一般默认是宋体。

参数类型是枚举类型，枚举值如下：

1.  Typeface.NORMAL //正常体
2.  Typeface.BOLD //粗体
3.  Typeface.ITALIC //斜体
4.  Typeface.BOLD_ITALIC //粗斜体

#### a、系统字体

方法预览：

``` java
create(String familyName, int style) //字体名

create(Typeface family, int style)  //类型

defaultFromStyle(int style)       //默认类型
```

我们来看一个简单的例子：

``` java
typeface = Typeface.create("宋体", Typeface.NORMAL);
mPaint.setTypeface(typeface);
canvas.drawText("我是一颗小小的石头", 100, 100, mPaint);

typeface = Typeface.create("楷体", Typeface.NORMAL);
mPaint.setTypeface(typeface);
canvas.drawText("我是一颗小小的石头", 100, 200, mPaint);
```

![12](http://upload-images.jianshu.io/upload_images/9028834-b6c6a34f079f6b71?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

从上图可以看出来，设置楷体根本没起作用，在系统的字体当中没有找到楷体。

#### b、自定义字体

方法预览：

``` java
createFromAsset(AssetManager mgr, String path) //Asset中获取

createFromFile(File path) //文件路径获取

createFromFile(String path) //外部路径获取
```

由于后面两个方法比较简单，主要来看一下第一个方法。

首先在`main`下创建`assets`文件夹，然后在`assets`文件夹创建`fonts`文件夹，最后在`fonts`文件夹下放入`font1.ttf`，如图：

![13](http://upload-images.jianshu.io/upload_images/9028834-a604e57b4241e562?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

``` java
typeface = Typeface.createFromAsset(mContext.getAssets(), "fonts/font1.ttf");
//Typeface.createFromFile(mContext.getFilesDir()+"/font1.ttf")
mPaint.setTypeface(typeface);
canvas.drawText("我是一颗小小的石头", 100, 100, mPaint);

typeface = Typeface.createFromAsset(mContext.getAssets(), "fonts/font2.ttf");
mPaint.setTypeface(typeface);
canvas.drawText("我是一颗小小的石头", 100, 200, mPaint);
```

效果图：
![14](http://upload-images.jianshu.io/upload_images/9028834-a4f885fbe526a525?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 引用：
[自定义View之绘图篇（三）：文字（Text）](http://blog.csdn.net/u012551350/article/details/51330308)

# baseLine和FontMetrics
了解baseLine和FontMetrics有助于我们理解drawText()绘制文字的原理
## 一、baseLine 基线

记得小时候练习字母用的是四线格本，把字母写在四线格内，如下：

![](http://upload-images.jianshu.io/upload_images/9028834-2b11647b05a3a47c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

那么在`canvas`中`drawText`绘制文字时候，也是有规则的，这个规则就是**`baseLine`（基线）**。什么又是基线了，说白了就是一条直线，我们这里理解的是确定它的位置。我们先来看一下基线：

![9](http://upload-images.jianshu.io/upload_images/9028834-413ca2ea05e411d9?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

从上图看出：基线等同四线格的第三条线，在`Android`中基线的位置定了，那么文字的位置也就定了。

### 1、canvas.drawText()

方法预览：

``` java
drawText(String text, float x, float y, Paint paint)
```

参数：
text 需要绘制的文字 
x 绘制文字原点X坐标 
y 绘制文字原点Y坐标 
paint 画笔

我们先来看一张图：

![](http://upload-images.jianshu.io/upload_images/9028834-ce07ac8e13f78439.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



需要注意的是x,y并不是文字左上角的坐标点，它比较特殊，**y所代表的是基线坐标y的坐标**。

我们具体来看看drawText()方法，这里以一个例子的形式来理解：


``` java
      mPaint.setAntiAlias(true);
      mPaint.setColor(Color.RED);
      mPaint.setStyle(Paint.Style.FILL);
      mPaint.setTextSize(120);

      canvas.drawText("abcdefghijk",200,200,mPaint);

      mPaint.setColor(Color.parseColor("#23AC3B"));
      canvas.drawLine(200,200,getWidth(),200,mPaint);
```

效果图：
![](http://upload-images.jianshu.io/upload_images/9028834-ae69246767794b55.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

证实了y是基线y的坐标点。

>结论：
>1、canvas.drawText()中参数**y是基线y的坐标** 
>2、x坐标、基线位置、文字大小确定，文字的位置就是确定的了。

### 2、mPaint.setTextAlign(Paint.Align.XXX);

我们可以从上面的例子看出，x代表的是文字开始绘制的地方。我第一次使用的时候也是这么认为的，可是我写了几个例子，发现我理解错了。那么正确的理解又是什么呢?

**x代表所要绘制文字所在矩形的相对位置**。相对位置就是指定点（x,y）在所要绘制矩形的位置。我们知道所绘制矩形的纵坐标是由Y值来确定的，而相对x坐标的位置，只有左、中、右三个位置了。也就是所绘制矩形可能是在x坐标相对于文字的左侧，中间或者右边绘制，而定义在x坐标在所绘制矩形相对位置的函数是：
```java
setTextAlign(Paint.Align align)
```

 

Paint.Align是枚举类型，值分别为 ： Paint.Align.LEFT，Paint.Align.CENTER和Paint.Align.RIGHT。

我们来分别看一看设置不同值时，绘制的结果是怎么样的。


#### （1）、Paint.Align.LEFT

``` java
mPaint.setTextAlign(Paint.Align.LEFT);//主要是这里的取值不一样
mPaint.setAntiAlias(true);
mPaint.setColor(Color.RED);
mPaint.setStyle(Paint.Style.FILL);
mPaint.setTextSize(120);

canvas.drawText("abcdefghijk", 200, 200, mPaint);

mPaint.setColor(Color.parseColor("#23AC3B"));
canvas.drawLine(0, 200, getWidth(), 200, mPaint);
canvas.drawLine(200, 0, 200, getHeight(), mPaint);
```

效果图如下：

![6](http://upload-images.jianshu.io/upload_images/9028834-4b311ea5567ca50f?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看出`（x,y）`在`文字矩形`下边的左边。

#### （2）、Paint.Align.CENTER

``` java
mPaint.setTextAlign(Paint.Align.CENTER);
mPaint.setAntiAlias(true);
mPaint.setColor(Color.RED);
mPaint.setStyle(Paint.Style.FILL);
mPaint.setTextSize(120);

canvas.drawText("abcdefghijk", 200, 200, mPaint);

mPaint.setColor(Color.parseColor("#23AC3B"));
canvas.drawLine(0, 200, getWidth(), 200, mPaint);
canvas.drawLine(200, 0, 200, getHeight(), mPaint);
```

效果图：

![7](http://upload-images.jianshu.io/upload_images/9028834-1b4e5f409eefe0d9?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看出`（x,y）`位于`文字矩形`下边的中间，换句话说，系统会根据`(x,y)`的位置和`文字矩形`大小，会计算出当前开始绘制的点。以使原点`(x,y)`正好在所要绘制的矩形下边的中间。

#### （3）、Paint.Align.RIGHT

``` java
mPaint.setTextAlign(Paint.Align.RIGHT);
mPaint.setAntiAlias(true);
mPaint.setColor(Color.RED);
mPaint.setStyle(Paint.Style.FILL);
mPaint.setTextSize(120);

canvas.drawText("abcdefghijk", 200, 200, mPaint);

mPaint.setColor(Color.parseColor("#23AC3B"));
canvas.drawLine(0, 200, getWidth(), 200, mPaint);
canvas.drawLine(200, 0, 200, getHeight(), mPaint);
```

效果图：
![](http://upload-images.jianshu.io/upload_images/9028834-ed35625a2059a31d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


可以看出`（x,y）`在`文字矩形`下边的右边。

## 二、FontMetrics

![10](http://upload-images.jianshu.io/upload_images/9028834-4ca2401c62a7eaa0?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

从图中可以知道，除了基线，还有另外的四条线，它们分别是 `top`，`ascent`，`descent`和`bottom`，它们的含义分别为：

1.  top：可绘制的最高高度所在线
2.  bottom：可绘制的最低高度所在线
3.  ascent ：系统建议的，绘制单个字符时，字符应当的最高高度所在线
4.  descent：系统建议的，绘制单个字符时，字符应当的最低高度所在线

### 1、获取实例

``` java
Paint.FontMetrics fontMetrics = mPaint.getFontMetrics();

Paint.FontMetricsInt fm=  mPaint.getFontMetricsInt();
```

两个构造方法的区别是，得到对象的成员变量的值一个为`float`类型，一个为`int`类型。

### 2、成员变量

FontMetrics，它里面有如下五个成员变量：

``` java
float ascent = fontMetrics.ascent;
float descent = fontMetrics.descent;
float top = fontMetrics.top;
float bottom = fontMetrics.bottom;
float leading = fontMetrics.leading;
```

ascent,descent,top,bottom,leading 这些线的位置要怎么计算出来呢？我们先来看个图：

![11](http://upload-images.jianshu.io/upload_images/9028834-099c5fa0ba1b12ee?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

那么它们的计算方法如下：


>**ascent** = ascent线的y坐标 **- baseline**线的y坐标；//负数
>**descent** = descent线的y坐标 **- baseline**线的y坐标；//正数
>**top** = top线的y坐标 **- baseline**线的y坐标；//负数
>**bottom** = bottom线的y坐标 **- baseline**线的y坐标；//正数

>**leading** = **top**线的y坐标 **- ascent**线的y坐标；//负数


FontMetrics的这几个变量的值都是**以baseLine为基准**的。
对于ascent来说，baseline线在ascent线之下，所以必然baseline的y值要大于ascent线的y值，所以ascent变量的值是负的。其他几个同理。

同样我们可以推算出：

>ascent线Y坐标 = baseline线的y坐标 + fontMetric.ascent；
>descent线Y坐标 = baseline线的y坐标 + fontMetric.descent；
>**top线Y坐标** = **baseline**线的y坐标 **+ fontMetric.top**；
>bottom线Y坐标 = baseline线的y坐标 + fontMetric.bottom；


### 3、绘制ascent，descent，top，bottom线

直接贴代码：

``` java
int baseLineY = 200;
mPaint.setTextSize(120);
canvas.drawText("abcdefghijkl's", 200, baseLineY, mPaint);

Paint.FontMetrics fontMetrics = mPaint.getFontMetrics();

float top = fontMetrics.top + baseLineY;
float ascent = fontMetrics.ascent + baseLineY;
float descent = fontMetrics.descent + baseLineY;
float bottom = fontMetrics.bottom + baseLineY;

//绘制基线
mPaint.setColor(Color.parseColor("#FF1493"));
canvas.drawLine(0, baseLineY, getWidth(), baseLineY, mPaint);

//绘制top直线
mPaint.setColor(Color.parseColor("#FFB90F"));
canvas.drawLine(0, top, getWidth(), top, mPaint);

//绘制ascent直线
mPaint.setColor(Color.parseColor("#b03060"));
canvas.drawLine(0, ascent, getWidth(), ascent, mPaint);

//绘制descent直线
mPaint.setColor(Color.parseColor("#912cee"));
canvas.drawLine(0, descent, getWidth(), descent, mPaint);

//绘制bottom直线
mPaint.setColor(Color.parseColor("#1E90FF"));
canvas.drawLine(0, bottom, getWidth(), bottom, mPaint);
```

在这段代码中，我们需要注意的是：
canvas.drawText()中参数y是基线y的位置； 
mPaint.setTextAlign(Paint.Align.LEFT);指点（200，200）在文字矩形的左边。然后计算各条直线的y坐标:


``` java
float top = fontMetrics.top + baseLineY;
float ascent = fontMetrics.ascent + baseLineY;
float descent = fontMetrics.descent + baseLineY;
float bottom = fontMetrics.bottom + baseLineY;
```

效果图：
![](http://upload-images.jianshu.io/upload_images/9028834-eb31f76c780a050a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 4、绘制文字最小矩形、文字宽度、文字高度

#### （1）绘制文字最小矩形

由`drawRect(float left, float top, float right, float bottom, @NonNull Paint paint)`需要绘制矩形就需要知道矩形左上角坐标点，矩形长和宽。以上面例子为例：

left = 200 
top = ascent ； 
right= 200+矩形宽度； 
bottom = descent；

这样我们就可以绘制出最小矩形：

![](http://upload-images.jianshu.io/upload_images/9028834-4043ad0562bb9560?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### （2）文字高度

``` java
float top = fontMetrics.top + baseLineY;
float bottom = fontMetrics.bottom + baseLineY;
//文字高度
float height= bottom - top; //注意top为负数
//文字中点y坐标
float center = (bottom - top) / 2;
```

当然也可以： `float height=Math.abs(top-bottom);`

#### （3）文字宽度

``` java
 String text="abcdefghijkl's";
 //文字宽度
 float width = mPaint.measureText(text);
```

### 已知中线，获取baseline

你可能会说这个还不简单：

``` java
Paint mPaint = new Paint();
mPaint.setTextSize(80);
mPaint.setColor(Color.WHITE);
mPaint.setAntiAlias(true);
String text = "FontMetrics的那些猜想";
Paint.FontMetrics fm = mPaint.getFontMetrics();
//获取文字高度
float fontHeight = fm.bottom - fm.top;
//获取文字宽度
float fontWidth = mPaint.measureText(text);
//绘制中线
canvas.drawLine(0, centerY, getWidth(), centerY, mPaint);
//绘制文本
canvas.drawText(text, centerX - fontWidth / 2, centerY + fontHeight / 2, mPaint);
```

效果图：

![font](http://upload-images.jianshu.io/upload_images/9028834-6a7d0c74cfeb1dcd?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

怎么会这样呢？

我们一起来分析下原因，先来看一张分析图：

![font](http://upload-images.jianshu.io/upload_images/9028834-eac9e570d2c9809c?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

那么我们就可以得出：

``` java
baseline=centerY+A-fm.bottom;
```

如果以：

``` java
baseline=centerY + fontHeight / 2;
```

那么就会以bottom线作为文字的基线，这样就会造成文字位于中线之下。

#### 结论

我们最终可知，当给定中间线center位置以后，那么baseline的位置为：

``` java
baseline = center + (FontMetrics.bottom - FontMetrics.top)/2 - FontMetrics.bottom;
```

`FontMetrics.bottom`注意这里为正数。

效果一览：

![font](http://upload-images.jianshu.io/upload_images/9028834-2bf7ea948be585e4?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们还可以这样获取文字高度：

``` java
public float getFontHeight(Paint paint, String str) {
    Rect rect = new Rect();
    paint.getTextBounds(str, 0, str.length(), rect);
    return rect.height();
}
```

经测试得出：

``` java
Paint.FontMetrics fm = mPaint.getFontMetrics();
```

**注意**：fm 值和手机密度没有关系，并且`fm.bottom/fm.top=4`（约等于）。

## 引用：
[自定义View之绘图篇（四）：baseLine和FontMetrics](http://blog.csdn.net/u012551350/article/details/51361778)


# Canvas


## 一、什么是Canvas？

什么是Canvas？官方文档是这么介绍的：

>The Canvas class holds the “draw” calls. To draw something, you need 4 basic components: A Bitmap to hold the pixels, a Canvas to host the draw calls (writing into the bitmap), a drawing primitive (e.g. Rect,Path, text, Bitmap), and a paint (to describe the colors and styles for the drawing).

Canvas 类是用于绘图的，绘制图形，你需要**4个基本元素**：

*   画在哪。画在Bitmap上。（相当于纸张，我们把图画在纸张上面）

*   怎么画。（调用canvas执行绘图操作。比如canvas.drawCircle()，canvas.drawLine()，canvas.drawPath()将我们需要的图像画出来。）

*   画的内容。（比如我想在纸张画一朵花，根据自己需求画圆，画直线，画路径等）

*   用什么画。（在纸张上画一朵花，肯定是用笔来画的，这里的笔指的是 Paint）

**Canvas 画布无限大**，它并没有边界。怎么来理解这句话呢？打个比方：画布就是窗外的景色，而手机屏幕就是窗口，你在窗口看到窗外的景色是有限的。同样我也可以把图形画到屏幕之外，通过对 Canvas 的变换与操作，让屏幕之外的图形显示到屏幕里面。

## 二、Canvas 绘图

Canvas 绘制一些常见的图形：

``` java
mPaint.setColor(Color.RED);
//绘制直线
canvas.drawLine(100,100,600,100,mPaint);

//绘制矩形
canvas.drawRect(100,200,600,400,mPaint);

//绘制文字
mPaint.setTextSize(60);
mPaint.setStrokeWidth(2);
mPaint.setStyle(Paint.Style.FILL);
canvas.drawText("我是一颗石头",100,500,mPaint);
```
![](http://upload-images.jianshu.io/upload_images/9028834-726a58b3974c70e8?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 三、Canvas 的变换与操作

有时候我们还需要对 Canvas 做一些操作，比如旋转，裁剪，平移等等。

*   canvas.translate 平移

*   canvas.rotate 旋转

*   canvas.scale 缩放

*   canvas.skew 错切

*   canvas.clipRect 裁剪

*   canvas.save和canvas.restore 保存和恢复

*   PorterDuffXfermode 图像混合 （paint相关方法）


### 1、平移（translate）

canvas中有一个函数translate（）是用来实现画布平移的，画布的原状是以左上角为原点，向左是X轴正方向，向下是Y轴正方向，如下图所示

![](http://upload-images.jianshu.io/upload_images/9028834-09b50cebfdee3e5f?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

translate函数其实实现的相当于平移坐标系，即平移坐标系的原点的位置。translate（）函数的原型如下：

```java
void translate(float dx, float dy)
```

参数说明：
float dx：水平方向平移的距离，正数指向正方向（向右）平移的量，负数为向负方向（向左）平移的量
flaot dy：垂直方向平移的距离，正数指向正方向（向下）平移的量，负数为向负方向（向上）平移的量

```java
protected void onDraw(Canvas canvas) {
    super.onDraw(canvas);      
    Paint paint = new Paint();  
    paint.setColor(Color.GREEN);  
    paint.setStyle(Style.FILL);  
//translate  平移,即改变坐标系原点位置      
//  canvas.translate(100, 100);  
    Rect rect1 = new Rect(0,0,400,220);  
    canvas.drawRect(rect1, paint);  
}  
```

1、上面这段代码，先把canvas.translate(100, 100);注释掉，看原来矩形的位置，然后打开注释，看平移后的位置，对比如下图：
![](http://upload-images.jianshu.io/upload_images/9028834-66b7efc1b3706ccd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 屏幕显示与Canvas的关系

很多童鞋一直以为显示所画东西的改屏幕就是Canvas，其实这是一个非常错误的理解，比如下面我们这段代码：

这段代码中，同一个矩形，在画布平移前画一次，平移后再画一次，大家会觉得结果会怎样？

```java
protected void onDraw(Canvas canvas) {
    super.onDraw(canvas);  
      
    //构造两个画笔，一个红色，一个绿色  
    Paint paint_green = generatePaint(Color.GREEN, Style.STROKE, 3);  
    Paint paint_red   = generatePaint(Color.RED, Style.STROKE, 3);  
      
    //构造一个矩形  
    Rect rect1 = new Rect(0,0,400,220);  
    //在平移画布前用绿色画下边框  
    canvas.drawRect(rect1, paint_green);  
      
    //平移画布后,再用红色边框重新画下这个矩形  
    canvas.translate(100, 100);  
    canvas.drawRect(rect1, paint_red);  
}  
private Paint generatePaint(int color, Paint.Style style, int width) {
    Paint paint = new Paint();
    paint.setColor(color);
    paint.setStyle(style);
    paint.setStrokeWidth(width);
    return paint;
} 
```

代码分析：
这段代码中，对于同一个矩形，在平移画布前利用绿色画下矩形边框，在平移后，再用红色画下矩形边框。大家是不是会觉得这两个边框会重合？实际结果是这样的。

![](http://upload-images.jianshu.io/upload_images/9028834-e4c35a2782fc2ac5?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**为什么绿色框并没有移动？**

这是由于**屏幕显示与Canvas根本不是一个概念**！
**Canvas**是一个很虚幻的概念，**相当于一个透明图层**（用过PS的同学应该都知道），每次Canvas画图时（即调用Draw系列函数），都会产生一个透明图层，然后在这个图层上画图，画完之后覆盖在屏幕上显示。所以上面的两个结果是由下面几个步骤形成的：

1、调用canvas.drawRect(rect1, paint_green);时，产生一个Canvas透明图层，由于当时还没有对坐标系平移，所以坐标原点是（0，0）；再在系统在Canvas上画好之后，覆盖到屏幕上显示出来，过程如下图：

![](http://upload-images.jianshu.io/upload_images/9028834-1eb4f422ca01bc2a?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2、然后再第二次调用canvas.drawRect(rect1, paint_red);时，又会重新产生一个全新的Canvas画布，但此时画布坐标已经改变了，即向右和向下分别移动了100像素，所以此时的绘图方式为：（合成视图，从上往下看的合成方式）

![](http://upload-images.jianshu.io/upload_images/9028834-1a24417016f91674?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上图展示了，上层的Canvas图层与底部的屏幕的合成过程，由于Canvas画布已经平移了100像素，所以在画图时是以新原点来产生视图的，然后合成到屏幕上，这就是我们上面最终看到的结果了。我们看到屏幕移动之后，有一部分超出了屏幕的范围，那超出范围的图像显不显示呢，当然不显示了！也就是说，Canvas上虽然能画上，但超出了屏幕的范围，是不会显示的。当然，我们这里也没有超出显示范围，两框框而已。

#### 总结：
1、**每次**调用**canvas.draw**XXXX系列函数来绘图进，都会产生一个**全新**的Canvas**画布**。
2、如果在DrawXXX前，调用平移、旋转等函数来对Canvas进行了操作，那么这个操作是不可逆的！每次产生的画布的最新位置都是这些操作后的位置。（关于Save()、Restore()的画布可逆问题的后面再讲）
3、在Canvas与屏幕合成时，超出屏幕范围的图像是不会显示出来的。

### 2、旋转（Rotate）

画布的旋转是默认是**围绕坐标原点**来旋转的，这里容易产生错觉，看起来觉得是图片旋转了，其实我们旋转的是画布，以后在此画布上画的东西显示出来的时候全部看起来都是旋转的。其实Roate函数有两个构造函数：

```java
void rotate(float degrees)
void rotate (float degrees, float px, float py) 
```

第一个构造函数直接输入旋转的度数，正数是顺时针旋转，负数指逆时针旋转，它的旋转中心点是原点（0，0）
第二个构造函数除了度数以外，还可以指定旋转的中心点坐标（px,py）

下面以第一个构造函数为例，旋转一个矩形，先画出未旋转前的图形，然后再画出旋转后的图形；

```java
protected void onDraw(Canvas canvas) {
    super.onDraw(canvas);  
    Paint paint_green = generatePaint(Color.GREEN, Style.FILL, 5);  
    Paint paint_red   = generatePaint(Color.RED, Style.STROKE, 5);  
      
    Rect rect1 = new Rect(300,10,500,100);  
    canvas.drawRect(rect1, paint_red); //画出原轮廓  
      
    canvas.rotate(30);//顺时针旋转画布  
    canvas.drawRect(rect1, paint_green);//画出旋转后的矩形  
}   
```

效果图是这样的：

![](http://upload-images.jianshu.io/upload_images/9028834-70eb9369b4c7c90d?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这个最终屏幕显示的构造过程是这样的：

下图显示的是第一次画图合成过程，此时仅仅调用canvas.drawRect(rect1, paint_red); 画出原轮廓

![](http://upload-images.jianshu.io/upload_images/9028834-4c347350ac78131f?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后是先将Canvas正方向依原点旋转30度，然后再与上面的屏幕合成，最后显示出我们的复合效果。

![](http://upload-images.jianshu.io/upload_images/9028834-eb392b3dc2366ba5?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

有关Canvas与屏幕的合成关系我觉得我已经讲的够详细了，后面的几个操作Canvas的函数，我就不再一一讲它的合成过程了。

### 3、缩放（scale ）

```java
public void scale (float sx, float sy) 
public final void scale (float sx, float sy, float px, float py)
```

##### 第一个构造函数：
float **sx**:**水平方向伸缩**的比例，假设原坐标轴的比例为n,**不变时为1**，在变更的X轴密度为n*sx;所以，sx为小数为缩小，sx为整数为放大
float **sy**:垂直方向伸缩的比例，同样，小数为缩小，整数为放大

注意：这里有X、Y轴的密度的改变，显示到图形上就会正好相同，比如X轴缩小，那么显示的图形也会缩小。一样的。

```java
        Paint paint = new Paint();
        paint.setAntiAlias(true);
        paint.setStyle(Paint.Style.STROKE);
        paint.setStrokeWidth(8);
        
        Rect rect = new Rect(100, 100, 200, 200);
//原图
        paint.setColor(Color.RED);
        canvas.drawRect(rect, paint);
//画布缩放方法1
        canvas.scale(0.5f, 2f);
        paint.setColor(Color.BLUE);
        canvas.drawRect(rect, paint);
```
![](http://upload-images.jianshu.io/upload_images/9028834-13112d7a64d464a5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
因为是整个画布的伸缩，对应连Stroke线的粗细也发生了变化

由图可知：
原图：Rect(100, 100, 200, 200)
移动后：Rect(50, 200, 100, 400)
**公式**：
```
原图：(l, t, r, b)
scale (sx,  sy)
移动后：(l*sx, t*sy, r*sx, b*sy)
```


##### 第二个构造函数：
>void scale (float sx, float sy, float px, float py)
>Preconcat the current matrix with the specified scale.
>Parameters
>sx
>float: The amount to scale in X
>sy
>float: The amount to scale in Y
>**px**
>float: **The x-coord for the pivot point** (unchanged by the scale)
>**py**
>float: The y-coord for the pivot point (unchanged by the scale)

**px 和 py 分别为缩放的中心点**，不设置的话默认为画布原点(0, 0)

```java
        Paint paint = new Paint();
        paint.setAntiAlias(true);
        paint.setStyle(Paint.Style.STROKE);
        paint.setStrokeWidth(4);

        Rect rect = new Rect(100, 100, 200, 200);
//原图
        canvas.save();
        paint.setColor(Color.RED);
        canvas.drawRect(rect, paint);
//画布缩放方法2
        canvas.scale(2f, 2f, 0, 0);
        paint.setColor(Color.BLUE);
        canvas.drawRect(rect, paint);
//画布缩放方法2（以正方形中心点为缩放中心）
        canvas.restore();
        canvas.scale(2f, 2f, 150, 150);
        paint.setColor(Color.GREEN);
        canvas.drawRect(rect, paint);
```
![](http://upload-images.jianshu.io/upload_images/9028834-afcea41f356fc5ab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###### 原理
**源码**如下：
``` java
public final void scale(float sx, float sy, float px, float py) {
    translate(px, py);
    scale(sx, sy);
    translate(-px, -py);
}
```

步骤：
先**将画布平移px,py，然后scale，scale结束之后再将画布平移-px,-py**。
```java
        Paint paint = new Paint();
        paint.setAntiAlias(true);
        paint.setStyle(Paint.Style.STROKE);
        paint.setStrokeWidth(8);

        Rect rect = new Rect(100, 100, 200, 200);
//原图
        paint.setColor(Color.RED);
        canvas.drawRect(rect, paint);
//画布缩放方法2
        canvas.scale(0.5f, 2f, 100, 100);//px、py
        paint.setColor(Color.BLUE);
        canvas.drawRect(rect, paint);
```
![](http://upload-images.jianshu.io/upload_images/9028834-e182faf72356fbee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**原理演示**：
```java
        Paint paint = new Paint();
        paint.setAntiAlias(true);
        paint.setStyle(Paint.Style.STROKE);
        paint.setStrokeWidth(8);
        
        Rect rect = new Rect(100, 100, 200, 200);

//原图
        paint.setColor(Color.RED);
        canvas.drawRect(rect, paint);
//以下三步模拟canvas.scale(0.5f, 2f, 100, 100);
//第①步：移动
        canvas.translate(100, 100);
        paint.setColor(Color.GREEN);
        canvas.drawRect(rect, paint);
//第②步：缩放
        canvas.scale(0.5f, 2f);
        paint.setColor(Color.YELLOW);
        canvas.drawRect(rect, paint);
//第③步：反向移动
        canvas.translate(-100, -100);
        paint.setColor(Color.BLUE);
        canvas.drawRect(rect, paint);
```
![](http://upload-images.jianshu.io/upload_images/9028834-84f0c0cac484adde.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
由图可知：
原图：Rect(100, 100, 200, 200)
移动后：Rect(100, 100, 150, 300)
**公式**：
```
原图：(l,  t,  r,  b)

scale (sx,  sy,  px,  py)	
    第①步：translate(px,  py);
	    原点(px,  py)
	    图①(l+px, t+py, r+px, b+py)
    第②步：scale(sx,  sy);
	    原点(px,  py)
	    图②(l*sx+px, t*sy+py, r*sx+px, b*sy+py)
    第③步：translate(-px,  -py);
	    原点(px+(-px)*sx,  py+(-py)*sy)即(px*(1-sx), py*(1-sy))
	    图③(l*sx+px*(1-sx), t*py*(1-sy)+(-py)*sy, r*sx+px*(1-sx), b*sy+py*(1-sy))

移动后：(l*sx+px*(1-sx), t*py*(1-sy)+(-py)*sy, r*sx+px*(1-sx), b*sy+py*(1-sy))
```
**Rt总结**：
**缩放**就是**相对于原点距离**的缩放，
**移动**就是对**原点**进行移动；
视觉坐标是距离原点的位置加上原点的坐标，
**canvas绘画的坐标是相较于原点的坐标**。

### 4、错切（skew）

它的构造函数：
```java
void skew (float sx, float sy)
```

参数说明：
float **sx**:将画布在**x方向上倾斜相应的角度**，sx倾斜角度的**tan值**，
float sy:将画布在y轴方向上倾斜相应的角度，sy为倾斜角度的tan值，

注意，这里全是倾斜角度的tan值，比如我们打算在X轴方向上倾斜30度，tan30=1/√3 约等于 0.56；tan60=根号3，小数对应1.732。

举例（在X轴方向上倾斜45度，tan45=1）：
``` java
        Paint paint = new Paint(Paint.ANTI_ALIAS_FLAG);
        paint.setStyle(Paint.Style.STROKE);
        paint.setStrokeWidth(1);
        Rect rect = new Rect(50, 50, 150, 150);
        paint.setColor(Color.RED);
        canvas.drawRect(rect, paint);

        canvas.skew(1, 0);//skew
        paint.setColor(Color.BLUE);
        canvas.drawRect(rect, paint);
```
![](http://upload-images.jianshu.io/upload_images/9028834-3afc737bb82fc575.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



可以从效果图当中看出来，我们设置 x 方向倾斜，反而 y 方向倾斜了，这就是为什么要叫做错切了。你心中一定又会有疑问？每个点的坐标又是怎么计算的呢？那下面我们一起来分析下：


**分析**：
设定在X轴上倾斜45°
![](http://upload-images.jianshu.io/upload_images/9028834-d959190bad0fe7e6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
设定在Y轴上倾斜30°
![](http://upload-images.jianshu.io/upload_images/9028834-356af9273fa4c41e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
//公式推导，以在Y轴倾斜为例skew (0, sy)
A点到旧X轴距离AX = 新点A1点到新X轴距离A1X1
XX1=OX * sy
新点A1纵坐标 = A1X1 + XX1
            = AX + OX * sy
即：新点A1纵坐标 = A点的纵坐标 + A点的横坐标*倾斜值
```


>void **skew** (float sx,  float sy)
>Preconcat the current matrix with the specified skew.
>Parameters
>**sx**
>float: The amount to skew in X
>**sy**
>float: The amount to skew in Y

在X轴方向倾斜
>**A 点横坐标**倾斜后的值 = A点的横坐标**+A点的纵坐标*倾斜值** 
>B 点横坐标倾斜后的值 = B点的横坐标+A点的纵坐标*倾斜值 
>
>**C 点横坐标**倾斜后的值 = C点的横坐标**+C点的纵坐标*倾斜值**
>D 点横坐标倾斜后的值 = D点的横坐标+C点的纵坐标*倾斜值


在Y轴方向倾斜
>**A 点纵坐标**倾斜后的值 = A点的纵坐标**+A点的横坐标*倾斜值** 
>D 点纵坐标倾斜后的值 = D点的纵坐标+A点的横坐标*倾斜值 
>
>**C 点纵坐标**倾斜后的值 = C点的纵坐标**+C点的横坐标*倾斜值**
>B 点纵坐标倾斜后的值 = B点的纵坐标+C点的横坐标*倾斜值





在y方向倾斜30度
``` java
        Paint paint = new Paint(Paint.ANTI_ALIAS_FLAG);
        paint.setStyle(Paint.Style.STROKE);
        paint.setStrokeWidth(1);
        Rect rect = new Rect(50, 100, 150, 200);
        paint.setColor(Color.RED);
        canvas.drawRect(rect, paint);

        canvas.skew(0, 0.56f);
        paint.setColor(Color.BLUE);
        canvas.drawRect(rect, paint);
```
![](http://upload-images.jianshu.io/upload_images/9028834-a9793eace1d51fdb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



### 5、裁剪画布（clip系列函数）

裁剪画布是利用Clip系列函数，通过与Rect、Path、Region取交、并、差等集合运算来获得最新的画布形状。除了调用Save、Restore函数以外，这个操作是不可逆的，一但Canvas画布被裁剪，就不能再被恢复！
Clip系列函数如下：
boolean  clipPath(Path path)
boolean  clipPath(Path path, Region.Op op)
boolean  clipRect(Rect rect, Region.Op op)
boolean  clipRect(RectF rect, Region.Op op)
boolean  clipRect(int left, int top, int right, int bottom)
boolean  clipRect(float left, float top, float right, float bottom)
boolean  clipRect(RectF rect)
boolean  clipRect(float left, float top, float right, float bottom, Region.Op op)
boolean  clipRect(Rect rect)
boolean  clipRegion(Region region)
boolean  clipRegion(Region region, Region.Op op)

以上就是根据Rect、Path、Region来取得最新画布的函数，难度都不大，就不再一一讲述。利用ClipRect() 来稍微一讲。

```java
protected void onDraw(Canvas canvas) {
    super.onDraw(canvas);  
      
    canvas.drawColor(Color.RED);  
    canvas.clipRect(new Rect(100, 100, 200, 200));  
    canvas.drawColor(Color.GREEN);  
}   
```

先把背景色整个涂成红色。显示在屏幕上
然后裁切画布，最后最新的画布整个涂成绿色。可见绿色部分，只有一小块，而不再是整个屏幕了。
关于两个画布与屏幕合成，我就不再画图了，跟上面的合成过程是一样的。
![](http://upload-images.jianshu.io/upload_images/9028834-7740082290d1927f?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 6、画布的保存与恢复（save()、restore()）

前面我们讲的所有对画布的操作都是不可逆的，这会造成很多麻烦，比如，我们为了实现一些效果不得不对画布进行操作，但操作完了，画布状态也改变了，这会严重影响到后面的画图操作。如果我们能对画布的大小和状态（旋转角度、扭曲等）进行实时保存和恢复就最好了。
这小节就给大家讲讲画布的保存与恢复相关的函数——Save（）、Restore（）。
```
int save ()
void  restore()
```

这两个函数没有任何的参数，很简单。
**Save**（）：每次调用Save（）函数，都会把**当前的画布**的状态进行**保存**，然后**放入特定的栈中**；
**restore**（）：每当调用Restore（）函数，就会把**栈中最顶层的画布状态取出来**，并按照这个状态恢复当前的画布，并在这个画布上做画。
为了更清晰的显示这两个函数的作用，下面举个例子：

```java
    canvas.drawColor(Color.RED);      
    //保存当前画布大小即整屏  
    canvas.save();   
      
    canvas.clipRect(new Rect(100, 100, 800, 800));  
    canvas.drawColor(Color.GREEN);      
    //恢复整屏画布  
    canvas.restore();
      
    canvas.drawColor(Color.BLUE);  
```

他图像的合成过程为：（最终显示为全屏幕蓝色）
![](http://upload-images.jianshu.io/upload_images/9028834-555377b249062d58.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


下面我通过一个多次利用Save（）、Restore（）来讲述有关保存Canvas画布状态的栈的概念：代码如下：

```java
    canvas.drawColor(Color.RED);  
    //保存的画布大小为全屏幕大小  
    canvas.save();  
      
    canvas.clipRect(new Rect(100, 100, 800, 800));  
    canvas.drawColor(Color.GREEN);  
    //保存画布大小为Rect(100, 100, 800, 800)  
    canvas.save();  
      
    canvas.clipRect(new Rect(200, 200, 700, 700));  
    canvas.drawColor(Color.BLUE);  
    //保存画布大小为Rect(200, 200, 700, 700)  
    canvas.save();  
      
    canvas.clipRect(new Rect(300, 300, 600, 600));  
    canvas.drawColor(Color.BLACK);  
    //保存画布大小为Rect(300, 300, 600, 600)  
    canvas.save();  
      
    canvas.clipRect(new Rect(400, 400, 500, 500));  
    canvas.drawColor(Color.WHITE);  
```

显示效果为：

![](http://upload-images.jianshu.io/upload_images/9028834-7fbbdb0235bae327?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在这段代码中，总共调用了四次Save操作。上面提到过，每调用一次Save（）操作就会将当前的画布状态保存到栈中，所以这四次Save（）所保存的状态的栈的状态如下：

![](http://upload-images.jianshu.io/upload_images/9028834-e8ad6b85daf2b7f0?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

注意在，第四次Save（）之后，我们还对画布进行了canvas.clipRect(new Rect(400, 400, 500, 500));操作，并将当前画布画成白色背景。也就是上图中最小块的白色部分，是最后的当前的画布。

如果，现在使用Restor（），会怎样呢，会把栈顶的画布取出来，当做当前画布的画图，试一下：

```java
canvas.drawColor(Color.RED);  
//保存的画布大小为全屏幕大小  
canvas.save();  
  
canvas.clipRect(new Rect(100, 100, 800, 800));  
canvas.drawColor(Color.GREEN);  
//保存画布大小为Rect(100, 100, 800, 800)  
canvas.save();  
  
canvas.clipRect(new Rect(200, 200, 700, 700));  
canvas.drawColor(Color.BLUE);  
//保存画布大小为Rect(200, 200, 700, 700)  
canvas.save();  
  
canvas.clipRect(new Rect(300, 300, 600, 600));  
canvas.drawColor(Color.BLACK);  
//保存画布大小为Rect(300, 300, 600, 600)  
canvas.save();  
  
canvas.clipRect(new Rect(400, 400, 500, 500));  
canvas.drawColor(Color.WHITE);  
  
//将栈顶的画布状态取出来，作为当前画布，并画成黄色背景  
canvas.restore();  
canvas.drawColor(Color.YELLOW);  
```

上段代码中，把栈顶的画布状态取出来，作为当前画布，然后把当前画布的背景色填充为黄色
![](http://upload-images.jianshu.io/upload_images/9028834-7622fc30b9a8db80?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

那如果我连续Restore（）三次，会怎样呢？
我们先分析一下，然后再看效果：Restore（）三次的话，会连续出栈三次，然后把第三次出来的Canvas状态当做当前画布，也就是Rect(100, 100, 800, 800)，所以如下代码：

```java
    canvas.drawColor(Color.RED);  
    //保存的画布大小为全屏幕大小  
    canvas.save();  
      
    canvas.clipRect(new Rect(100, 100, 800, 800));  
    canvas.drawColor(Color.GREEN);  
    //保存画布大小为Rect(100, 100, 800, 800)  
    canvas.save();  
      
    canvas.clipRect(new Rect(200, 200, 700, 700));  
    canvas.drawColor(Color.BLUE);  
    //保存画布大小为Rect(200, 200, 700, 700)  
    canvas.save();  
      
    canvas.clipRect(new Rect(300, 300, 600, 600));  
    canvas.drawColor(Color.BLACK);  
    //保存画布大小为Rect(300, 300, 600, 600)  
    canvas.save();  
      
    canvas.clipRect(new Rect(400, 400, 500, 500));  
    canvas.drawColor(Color.WHITE);  
      
    //连续出栈三次，将最后一次出栈的Canvas状态作为当前画布，并画成黄色背景  
    canvas.restore();  
    canvas.restore();  
    canvas.restore();  
    canvas.drawColor(Color.YELLOW);  
```

结果为：

![](http://upload-images.jianshu.io/upload_images/9028834-90f44d77a9b5cbe6?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 7、saveLayer
```
public int saveLayer(float left, float top, float right, float bottom, @Nullable Paint paint,
            @Saveflags int saveFlags)
            ...
saveLayerAlpha(float left, float top, float right, float bottom, int alpha,
            @Saveflags int saveFlags)
            ...
```

public int saveLayer(@Nullable RectF bounds, @Nullable Paint paint)
Canvas 在一般的情况下可以看作是一张画布，所有的绘图操作如drawBitmap, drawCircle都发生在这张画布上，这张画板还定义了一些属性比如Matrix，颜色等等。
但是如果需要实现一些相对复杂的绘图操作，比如**多层动画，地图**（地图可以有多个地图层叠加而成，比如：政区层，道路层，兴趣点层）。Canvas提供了**图层（Layer）支持**，缺省情况可以看作是只有一个图层Layer。如果需要按层次来绘图，Android的Canvas可以**使用SaveLayerXXX, Restore 来创建一些中间层**，对于这些Layer是按照“栈结构“来管理的：       

![](http://upload-images.jianshu.io/upload_images/9028834-2ee7eae01c5ca36e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 **创建**一个新的Layer到“栈”中，可以使用**saveLayer, savaLayerAlpha**；
 从“栈”中**推出**一个Layer，可以使用**restore,restoreToCount**。
 但Layer入栈时，后续的DrawXXX操作都发生在这个Layer上，而Layer退栈时，就会把本层绘制的图像“绘制”到上层或是Canvas上。
 在复制Layer到Canvas上时，可以指定**Layer的透明度**(Layer），这是在**创建Layer时指定**的：public int saveLayerAlpha(RectF bounds, int alpha, int saveFlags)

本例Layers 介绍了图层的基本用法：Canvas可以看做是由两个图层（Layer）构成的，为了更好的说明问题，我们将代码稍微修改一下，缺省图层绘制一个红色的圆，在新的图层画一个蓝色的圆，新图层的透明度为0×88。
public class Layers extends Activity {  


```java
@Override
protected void onDraw(Canvas canvas) {
    super.onDraw(canvas);
    Paint mPaint = new Paint();
    mPaint.setAntiAlias(true);

    canvas.drawColor(Color.RED);

    canvas.saveLayerAlpha(0, 0, 300, 300, 0x88, Canvas.ALL_SAVE_FLAG);//图层1
    canvas.drawColor(Color.BLUE);

    canvas.restore();//下面两张图，图1注释掉词句，图2没注释掉
    canvas.saveLayerAlpha(100, 100, 400, 400, 0xff, Canvas.ALL_SAVE_FLAG);//图层2
    canvas.drawColor(Color.YELLOW);
}
```
没有canvas.restore();，所以图层2是基于图层1作画，黄色还是半透明
![](http://upload-images.jianshu.io/upload_images/9028834-59e45e22bbb77212.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
运行了一次canvas.restore();，去掉栈顶一个图层即图层1露出画布，所以图层2是直接在画布上作画，黄色不透明
![](http://upload-images.jianshu.io/upload_images/9028834-686304f880bd8309.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)





#### **saveFlags**

上面我们只是粗暴的使用了ALL_SAVE_FLAG来保存的所有的信息，但是实际使用中，所有信息都保存必然增加了开销，所以，我们应该根据需要的动作，尽量的精确的保存少量的信息。这里就需要了解各个flag的意义。

首先需要知道的是，使用flag的方法除了saveFlayer还有save方法，他们都可以使用flag来指定需要保存的信息。那么来看看6中flag所对应的意义：

| Flag                       |                    意义                    | 适用方法           |
| -------------------------- | :--------------------------------------: | -------------- |
| MATRIX_SAVE_FLAG           |              只保存图层的matrix矩阵              | save，saveLayer |
| CLIP_SAVE_FLAG             |                 只保存大小信息                  | save，saveLayer |
| HAS_ALPHA_LAYER_SAVE_FLAG  |     表明该图层有透明度，和下面的标识冲突，都设置时以下面的标志为准      | saveLayer      |
| FULL_COLOR_LAYER_SAVE_FLAG | 完全保留该图层颜色（和上一图层合并时，清空上一图层的重叠区域，保留该图层的颜色） | saveLayer      |
| CLIP_TO_LAYER_SAVE_        | 创建图层时，会把canvas（所有图层）裁剪到参数指定的范围，如果省略这个flag将导致图层开销巨大（实际上图层没有裁剪，与原图层一样大） |                |
| ALL_SAVE_FLAG              |                  保存所有信息                  | save，saveLayer |

##### **(1) MATRIX_SAVE_FLAG**

只保存图层的matrix矩阵。 
canvas中的哪些方法是利用matrix完成的，这里需要明确，其实我们知道，canvas的绘制，最终是发生在bitmap上的，从canvas的构造函数中也可以看出。

在Bitmap的构造函数中可以看出bitmap的操作也是通过matrix来进行的：
```java
Bitmap createBitmap(Bitmap source, int x, int y, int width, int height,Matrix m, boolean filter)
```

那么我们可以知道canvas的canvas.translate(平移)、canvas.rotate（旋转）、canvas.scale（缩放）、canvas.skew（扭曲）其实都是通过matrix来达到的，这一点可以在代码中使用MATRIX_SAVE_FLAG来进行验证。

###### **save方法**

这里举例平移：
```java
paint.setColor(Color.BLUE);
canvas.save(Canvas.MATRIX_SAVE_FLAG);
canvas.translate(200, 200);
canvas.drawRect(100, 100, 300, 300, paint);
canvas.restore();

paint.setColor(Color.RED);
canvas.drawRect(100, 100, 300, 300, paint);
```
![](http://upload-images.jianshu.io/upload_images/9028834-1c975bc14f7e2b69?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到平移效果得到了保存,并且可以恢复。

###### **saveLayer方法**
```java
paint.setColor(Color.BLUE);
int count=canvas.saveLayer(0,0,1000,1000,paint,Canvas.MATRIX_SAVE_FLAG|Canvas.HAS_ALPHA_LAYER_SAVE_FLAG);
canvas.translate(200, 200);
canvas.drawRect(100, 100, 300, 300, paint);
canvas.restoreToCount(count);

paint.setColor(Color.RED);
canvas.drawRect(100, 100, 300, 300, paint);
```
![](http://upload-images.jianshu.io/upload_images/9028834-23c21709123d7c7e?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
图中看出效果相同

如果这里不使用MATRIX_SAVE_FLAG标志位，那么是否会出现不同的效果呢，使用CLIP_SAVE_FLAG标志来试试：
```java
paint.setColor(Color.BLUE);
int count=canvas.saveLayer(0,0,1000,1000,paint,Canvas.CLIP_SAVE_FLAG|Canvas.HAS_ALPHA_LAYER_SAVE_FLAG);
canvas.translate(200, 200);
canvas.drawRect(100, 100, 300, 300, paint);
canvas.restoreToCount(count);

paint.setColor(Color.RED);
canvas.drawRect(100, 100, 300, 300, paint);
```
![](http://upload-images.jianshu.io/upload_images/9028834-a337642275e6742f?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)





代码和上面基本相同，只是标志位改变了，这里可以看到两个图重叠了，也就是说CLIP_SAVE_FLAG标志位并没有保存相关的位移信息，导致restore的时候没能恢复。

##### **(2) CLIP_SAVE_FLAG**

看了上面的MATRIX_SAVE_FLAG，这里的意义基本知道，主要就是**保存裁剪相关的信息**。 
由于和上面的示例基本类似，这里就不再做讲解了。

##### **(3) FULL_COLOR_LAYER_SAVE_FLAG 和 HAS_ALPHA_LAYER_SAVE_FLAG**

这两个方法是saveLayer专用的方法，HAS_ALPHA_LAYER_SAVE_FLAG为layer添加一个透明通道，这样一来没有绘制的地方就是透明的，覆盖到上一个layer的时候，就会显示出上一层的图像。而FULL_COLOR_LAYER_SAVE_FLAG 则会完全展示当前layer的图像，清除掉上一层的重合图像。

来看看FULL_COLOR_LAYER_SAVE_FLAG 的示例：
```java
canvas.drawColor(Color.RED);

canvas.saveLayer(200,200,700,700,mPaint,Canvas.FULL_COLOR_LAYER_SAVE_FLAG);
mPaint.setColor(Color.GREEN);
canvas.drawRect(300,300,600,600,mPaint);
canvas.restore();
```

![](http://upload-images.jianshu.io/upload_images/9028834-9147bd75d101bfc0?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



可以看到，绿色的方块周围有白色的一圈，整个白色加上绿色区域是这个layer的区域，由于这里使用了**FULL_COLOR_LAYER_SAVE_FLAG**标志，所以这块区域的红色被layer层完全覆盖（即使是透明），由于绿色周围的颜色是透明的，所以在**清除了红色并覆盖后，就显示出了activity的背景颜色**，所以显示了白色。

如果activity背景是黑色，这一块自然变为黑色：

![](http://upload-images.jianshu.io/upload_images/9028834-d859b7c301db957c?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

那么其他代码不变，只是将标志位替换成**HAS_ALPHA_LAYER_SAVE_FLAG**会发生什么：

![](http://upload-images.jianshu.io/upload_images/9028834-819b7bda45280461?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到，绿色周围的白色不见了，可见，这就是区别。使用这个标志位不会清空上一图层的内容。

##### **(4) CLIP_TO_LAYER_SAVE_**

这个标志比较重要，官方的建议是，最好不要忽略这个标识，这个标识如果不设置将会带来很大的性能问题。

这个标识的作用是将canvas裁剪到指定的大小，并且无法回复。看下面一个例子：
```java
canvas.drawColor(Color.RED);
canvas.saveLayer(200,200,700,700,mPaint,Canvas.CLIP_TO_LAYER_SAVE_FLAG);
canvas.drawColor(Color.GREEN);
canvas.restore();
canvas.drawColor(Color.BLACK);
```
![](http://upload-images.jianshu.io/upload_images/9028834-77a36245e6e9c399?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



这里看，先将底色绘制为红色，然后开启新图层，再绘制为绿色，最后将canvas绘制为黑色，为什么最后不是全屏黑色呢，这里明明restore了，这是因为使用了**CLIP_TO_LAYER_SAVE_FLAG**标志，这样一来，**canvas被裁剪了**，并且**无法回复**了。这样也就减少了处理的区域，增加了性能。



## 习题

### 画一个表盘
![image](http://upload-images.jianshu.io/upload_images/9028834-2dc44b40d770b3a0?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
闹钟表盘其实就是在一个圆周上绘制；

既然是圆周，最简单的方式莫过于在闹钟的12点钟处划线，通过canvas的旋转绘制到对应圆周处，我们一起实现一下：

整个圆周是360 度，每隔 30 度为一个整时间刻度，整刻度与刻度之间有四个短刻度，划分出5个小段，每个段为6度，有了这些分析，我们则可以采用如下代码进行绘制：
```java
    /* 绘制刻度 */
    private void drawLines(Canvas canvas) {
        for (int degree = 0; degree <= 360; degree++) {
            if (degree % 30 == 0) {
                //时针
                mLineBottom = mLineTop + mHourLineHeight;
                mLinePaint.setStrokeWidth(mHourLineWidth);
            } else {
                mLineBottom = mLineTop + mMinuteLineHeight;
                mLinePaint.setStrokeWidth(mMinuteLineWidth);
            }

            if (degree % 6 == 0) {
                canvas.save();
                canvas.rotate(degree, mCenterX, mCenterY);
                canvas.drawLine(mLineLeft, mLineTop, mLineLeft, mLineBottom, mLinePaint);
                canvas.restore();
            }
        }
    }
```

整体代码如下：
```java
/* 表盘 */
public class Dial extends View {
    private static final int HOUR_LINE_HEIGHT = 35;
    private static final int MINUTE_LINE_HEIGHT = 25;
    private Paint mCirclePaint, mLinePaint;
    private DrawFilter mDrawFilter;
    //圆心（表盘中心）
    private int mCenterX, mCenterY, mCenterRadius;

    // 圆环线宽度
    private int mCircleLineWidth;
    // 直线刻度线宽度
    private int mHourLineWidth, mMinuteLineWidth;
    // 时针长度
    private int mHourLineHeight;
    // 分针长度
    private int mMinuteLineHeight;
    // 刻度线的左、上位置
    private int mLineLeft, mLineTop;

    // 刻度线的下边位置
    private int mLineBottom;
    // 用于控制刻度线位置
    private int mFixLineHeight;

    public Dial(Context context) {
        this(context, null);
    }

    public Dial(Context context, @Nullable AttributeSet attrs) {
        this(context, attrs, 0);
    }

    public Dial(Context context, @Nullable AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        mDrawFilter = new PaintFlagsDrawFilter(0, Paint.ANTI_ALIAS_FLAG
                | Paint.FILTER_BITMAP_FLAG);

        mCircleLineWidth = (int) TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_DIP, 8,
                getResources().getDisplayMetrics());
        mHourLineWidth = (int) TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_DIP, 4,
                getResources().getDisplayMetrics());
        mMinuteLineWidth = mHourLineWidth / 2;

        mFixLineHeight = (int) TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_DIP, 4,
                getResources().getDisplayMetrics());

        mHourLineHeight = (int) TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_DIP,
                HOUR_LINE_HEIGHT,
                getResources().getDisplayMetrics());
        mMinuteLineHeight = (int) TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_DIP,
                MINUTE_LINE_HEIGHT,
                getResources().getDisplayMetrics());
        initPaint();
    }

    private void initPaint() {
        mCirclePaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        mCirclePaint.setColor(Color.RED);
        mCirclePaint.setStyle(Paint.Style.STROKE);
        mCirclePaint.setStrokeWidth(mCircleLineWidth);

        mLinePaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        mLinePaint.setColor(Color.RED);
        mLinePaint.setStyle(Paint.Style.FILL_AND_STROKE);
        mLinePaint.setStrokeWidth(mHourLineWidth);
    }

    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
    }

    @Override
    protected void onDraw(Canvas canvas) {
        canvas.setDrawFilter(mDrawFilter);
        super.onDraw(canvas);
        // 绘制表盘
        drawCircle(canvas);
        // 绘制刻度
        drawLines(canvas);
    }

    /* 绘制刻度 */
    private void drawLines(Canvas canvas) {
        for (int degree = 0; degree <= 360; degree++) {
            if (degree % 30 == 0) {
                //时针
                mLineBottom = mLineTop + mHourLineHeight;
                mLinePaint.setStrokeWidth(mHourLineWidth);
            } else {
                mLineBottom = mLineTop + mMinuteLineHeight;
                mLinePaint.setStrokeWidth(mMinuteLineWidth);
            }

            if (degree % 6 == 0) {
                canvas.save();
                canvas.rotate(degree, mCenterX, mCenterY);
                canvas.drawLine(mLineLeft, mLineTop, mLineLeft, mLineBottom, mLinePaint);
                canvas.restore();
            }
        }
    }

    /* 绘制表盘 */
    private void drawCircle(Canvas canvas) {
        canvas.drawCircle(mCenterX, mCenterY, mCenterRadius, mCirclePaint);
    }

    @Override
    protected void onSizeChanged(int w, int h, int oldw, int oldh) {
        super.onSizeChanged(w, h, oldw, oldh);
        mCenterX = w / 2;
        mCenterY = h / 2;
        mCenterRadius = Math.min(mCenterX, mCenterY) - mCircleLineWidth / 2;

        mLineLeft = mCenterX - mMinuteLineWidth / 2;
        mLineTop = mCenterY - mCenterRadius;
    }
}
```

### 画一个正方形螺旋图
![](http://upload-images.jianshu.io/upload_images/9028834-09b4a8f8c70844a1?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

思路非常的简单：
1\. 绘制一个和屏幕等宽的正方形；
2\. 将画布以正方形中心为基准点进行缩放；
3\. 在缩放的过程中绘制原正方形；

注：每次绘制都得使用canvas.save()  和 canvas.restore()进行画布的锁定和回滚，以免除对后面绘制的影响。

先初始化画笔，注意此时画笔需要设置成空心：

```java
protected void onDraw(Canvas canvas) {
    super.onDraw(canvas);
    Paint paint = new Paint(Paint.ANTI_ALIAS_FLAG);
    paint.setStyle(Paint.Style.STROKE);
    paint.setStrokeWidth(8);
    int l = 10;
    int t = 10;
    int r = 410;
    int b = 410;
    int space = 30;
    Rect squareRect = new Rect(l, t, r, b);
    int squareCount = (r - l) / space;
    float px = l + (r - l) / 2;
    float py = t + (b - t) / 2;
    for (int i = 0; i < squareCount; i++) {
        // 保存画布
        canvas.save();
        float fraction = (float) i / squareCount;
        // 将画布以正方形中心进行缩放
        canvas.scale(fraction, fraction, px, py);
        canvas.drawRect(squareRect, paint);
        // 画布回滚
        canvas.restore();
    }
}
```

一起来看下绘制的效果：
![](http://upload-images.jianshu.io/upload_images/9028834-2df65c66756dd523.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)







## 引用：
**[自定义控件之绘图篇（四）：canvas变换与操作](http://blog.csdn.net/harvic880925/article/details/39080931)**
[自定义View之绘图篇（六）：Canvas那些你应该知道的变换](http://blog.csdn.net/u012551350/article/details/51858309)
[Canvas之translate、scale、rotate、skew方法讲解！](http://blog.csdn.net/tianjian4592/article/details/45234419)
[Android 2D Graphics学习（二）、Canvas篇1、Canvas基本使用](http://blog.csdn.net/lonelyroamer/article/details/8264189)
[android canvas layer (图层)详解与进阶](http://blog.csdn.net/cquwentao/article/details/51423371)



# 综合习题
## 实现图片圆角带边框的效果
![](http://upload-images.jianshu.io/upload_images/9028834-81ea1e0198a7cf52.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

``` java
@Override
protected void onDraw(Canvas canvas) {
    super.onDraw(canvas);
    Bitmap rawBitmap = BitmapFactory.decodeResource(getResources(), R.mipmap.cat);

    Bitmap bitmap = getRoundCornerBitmap(rawBitmap, 50);
    canvas.drawBitmap(bitmap, 0, 0, new Paint());
}

/**
 * @param bitmap 原图
 * @param pixels 圆角大小
 * @return
 */
public Bitmap getRoundCornerBitmap(Bitmap bitmap, float pixels) {
//获取bitmap的宽高
    int width = bitmap.getWidth();
    int height = bitmap.getHeight();

    Bitmap cornerBitmap = Bitmap.createBitmap(width, height, Bitmap.Config.ARGB_8888);
    Paint paint = new Paint();
    Canvas canvas = new Canvas(cornerBitmap);
    paint.setAntiAlias(true);

    canvas.drawRoundRect(new RectF(0, 0, width, height), pixels, pixels, paint);
    paint.setXfermode(new PorterDuffXfermode(PorterDuff.Mode.SRC_IN));
    canvas.drawBitmap(bitmap, null, new RectF(0, 0, width, height), paint);

//绘制边框
    paint.setStyle(Paint.Style.STROKE);
    paint.setStrokeWidth(6);
    paint.setColor(Color.GREEN);
    canvas.drawRoundRect(new RectF(0, 0, width, height), pixels, pixels, paint);

    return cornerBitmap;
}
```

1、首先通过`Bitmap cornerBitmap = Bitmap.createBitmap(width, height, Bitmap.Config.ARGB_8888);`生成`cornerBitmap` 实例，注意了`Bitmap` 只能通过静态方法来获取它的实例，并不能直接 new出来。

2、绘制圆角矩形`canvas.drawRoundRect(new RectF(0, 0, width, height), pixels, pixels, paint);`。

3、为`Paint`设置`PorterDuffXfermode`。参数 `PorterDuff.Mode.SRC_IN` 取交集。

4、绘制原图。`canvas.drawBitmap(bitmap, null, new RectF(0, 0, width, height), paint);`

5、绘制边框圆角。 `canvas.drawRoundRect(new RectF(0, 0, width, height), pixels, pixels, paint);`

### 引用：
[自定义View之绘图篇（六）：Canvas那些你应该知道的变换](http://blog.csdn.net/u012551350/article/details/51858309)


**相关文章：**

[《Android自定义控件三部曲文章索引》](http://blog.csdn.net/harvic880925/article/details/50995268): [http://blog.csdn.net/harvic880925/article/details/50995268](http://blog.csdn.net/harvic880925/article/details/50995268)




