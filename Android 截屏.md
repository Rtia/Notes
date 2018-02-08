<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Android 截屏](#android-%E6%88%AA%E5%B1%8F)
  - [一： 普通截屏的实现](#%E4%B8%80-%E6%99%AE%E9%80%9A%E6%88%AA%E5%B1%8F%E7%9A%84%E5%AE%9E%E7%8E%B0)
    - [方法1：](#%E6%96%B9%E6%B3%951)
    - [方法2：](#%E6%96%B9%E6%B3%952)
  - [二：开源方案](#%E4%BA%8C%E5%BC%80%E6%BA%90%E6%96%B9%E6%A1%88)
  - [三： ScrollView截屏](#%E4%B8%89-scrollview%E6%88%AA%E5%B1%8F)
  - [四： ListView截屏](#%E5%9B%9B-listview%E6%88%AA%E5%B1%8F)
  - [五： RecyclerView截屏](#%E4%BA%94-recyclerview%E6%88%AA%E5%B1%8F)
  - [六： 其他屏幕相关工具](#%E5%85%AD-%E5%85%B6%E4%BB%96%E5%B1%8F%E5%B9%95%E7%9B%B8%E5%85%B3%E5%B7%A5%E5%85%B7)
    - [1.获取状态栏高度常见方式](#1%E8%8E%B7%E5%8F%96%E7%8A%B6%E6%80%81%E6%A0%8F%E9%AB%98%E5%BA%A6%E5%B8%B8%E8%A7%81%E6%96%B9%E5%BC%8F)
    - [2.获取标题栏高度](#2%E8%8E%B7%E5%8F%96%E6%A0%87%E9%A2%98%E6%A0%8F%E9%AB%98%E5%BA%A6)
    - [3.获取屏幕宽高](#3%E8%8E%B7%E5%8F%96%E5%B1%8F%E5%B9%95%E5%AE%BD%E9%AB%98)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->



# Android 截屏

[TOC]

## 一： 普通截屏的实现

### 方法1：
```java
public static Bitmap getBitmapFromView(View v) {
	int w = v.getWidth();
	int h = v.getHeight();

	Bitmap bmp = Bitmap.createBitmap(w, h, Bitmap.Config.ARGB_8888);
	Canvas c = new Canvas(bmp);

	c.drawColor(Color.WHITE);
	/** 如果不设置canvas画布为白色，则生成透明 */

//        v.layout(0, 0, w, h);
	v.draw(c);

	return bmp;
}
```

### 方法2：
获取当前Window 的 DrawingCache 的方式，即**decorView**的**DrawingCache**
```java
/**
* shot the current screen ,with the status but the status is trans *
* @param ctx current activity
*/
public static Bitmap shotActivity(Activity ctx) {
    View view = ctx.getWindow().getDecorView();
    view.setDrawingCacheEnabled(true);
    view.buildDrawingCache();

    Bitmap bp = Bitmap.createBitmap(view.getDrawingCache(), 0, 0, view.getMeasuredWidth(), view.getMeasuredHeight());

    view.setDrawingCacheEnabled(false);
    view.destroyDrawingCache();

    return bp;
}
```


获取当前View的DrawingCache

```java
public static Bitmap getViewBp(View v) {
	if (null == v) {
		return null;
	}
	v.setDrawingCacheEnabled(true);
	v.buildDrawingCache();
	if (Build.VERSION.SDK_INT >= 11) {
		v.measure(MeasureSpec.makeMeasureSpec(v.getWidth(),
				MeasureSpec.EXACTLY), MeasureSpec.makeMeasureSpec(
				v.getHeight(), MeasureSpec.EXACTLY));
		v.layout((int) v.getX(), (int) v.getY(),
				(int) v.getX() + v.getMeasuredWidth(),
				(int) v.getY() + v.getMeasuredHeight());
	} else {
		v.measure(MeasureSpec.makeMeasureSpec(0, MeasureSpec.UNSPECIFIED),
				MeasureSpec.makeMeasureSpec(0, MeasureSpec.UNSPECIFIED));
		v.layout(0, 0, v.getMeasuredWidth(), v.getMeasuredHeight());
	}
	Bitmap b = Bitmap.createBitmap(v.getDrawingCache(), 0, 0, v.getMeasuredWidth(), v.getMeasuredHeight());

	v.setDrawingCacheEnabled(false);
	v.destroyDrawingCache();
	return b;
}
```



## 二：开源方案

在滚动视图中，如果当前View并没有在视图中全部绘制出来，我们可以利用View的ScrollTo()和ScrollBy()方法来移动画布，同时获取当前View的可视部分的DrawingCache，最后进行拼接得到其Bitmap，参考:[PGSSoft/scrollscreenshot](https://github.com/PGSSoft/scrollscreenshot)，原理图如下：
![](http://img.blog.csdn.net/20180107164335570?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

## 三： ScrollView截屏

三个截屏中，ScrollView最简单，因为ScrollView只有一个childView，虽然没有全部显示在界面上，但是已经全部渲染绘制，因此可以直接 调用`scrollView.draw(canvas)`来完成截图，

```java
/**
* http://blog.csdn.net/lyy1104/article/details/40048329
*/
  public static Bitmap shotScrollView(ScrollView scrollView) {
      int h = 0;
      Bitmap bitmap = null;
      for (int i = 0; i < scrollView.getChildCount(); i++) {
          h += scrollView.getChildAt(i).getHeight();
          scrollView.getChildAt(i).setBackgroundColor(Color.parseColor("#ffffff"));
      }
      bitmap = Bitmap.createBitmap(scrollView.getWidth(), h, Bitmap.Config.RGB_565);
      final Canvas canvas = new Canvas(bitmap);
      scrollView.draw(canvas);
      return bitmap;
  }
```






## 四： ListView截屏

而ListView就是会回收与重用Item，并且只会绘制在屏幕上显示的ItemView，根据stackoverflow上大神的建议，采用一个List来存储Item的视图，这种方案依然不够好，当Item足够多的时候，可能会发生oom。

```java
/**
   * http://stackoverflow.com/questions/12742343/android-get-screenshot-of-all-listview-items
   */
  public static Bitmap shotListView(ListView listview) {

    ListAdapter adapter = listview.getAdapter();
    int itemscount = adapter.getCount();
    int allitemsheight = 0;
    List<Bitmap> bmps = new ArrayList<Bitmap>();

    for (int i = 0; i < itemscount; i++) {
      View childView = adapter.getView(i, null, listview);
      childView.measure(
          View.MeasureSpec.makeMeasureSpec(listview.getWidth(), View.MeasureSpec.EXACTLY),
          View.MeasureSpec.makeMeasureSpec(0, View.MeasureSpec.UNSPECIFIED));

      childView.layout(0, 0, childView.getMeasuredWidth(), childView.getMeasuredHeight());
      childView.setDrawingCacheEnabled(true);
      childView.buildDrawingCache();
      bmps.add(childView.getDrawingCache());
      allitemsheight += childView.getMeasuredHeight();
    }

    Bitmap bigbitmap =
        Bitmap.createBitmap(listview.getMeasuredWidth(), allitemsheight, Bitmap.Config.ARGB_8888);
    Canvas bigcanvas = new Canvas(bigbitmap);

    Paint paint = new Paint();
    int iHeight = 0;

    for (int i = 0; i < bmps.size(); i++) {
      Bitmap bmp = bmps.get(i);
      bigcanvas.drawBitmap(bmp, 0, iHeight, paint);
      iHeight += bmp.getHeight();

      bmp.recycle();
      bmp = null;
    }

    return bigbitmap;
  }
```


## 五： RecyclerView截屏

我们都知道，在新的Android版本中，已经可以用RecyclerView来代替使用ListView的场景，相比较ListView，RecyclerView对Item View的缓存支持的更好。可以采用和ListView相同的方案，这里也是在stackoverflow上看到的方案。

```java
/**
   * https://gist.github.com/PrashamTrivedi/809d2541776c8c141d9a
   */
  public static Bitmap shotRecyclerView(RecyclerView view) {
    RecyclerView.Adapter adapter = view.getAdapter();
    Bitmap bigBitmap = null;
    if (adapter != null) {
      int size = adapter.getItemCount();
      int height = 0;
      Paint paint = new Paint();
      int iHeight = 0;
      final int maxMemory = (int) (Runtime.getRuntime().maxMemory() / 1024);

      // Use 1/8th of the available memory for this memory cache.
      final int cacheSize = maxMemory / 8;
      LruCache<String, Bitmap> bitmaCache = new LruCache<>(cacheSize);
      for (int i = 0; i < size; i++) {
        RecyclerView.ViewHolder holder = adapter.createViewHolder(view, adapter.getItemViewType(i));
        adapter.onBindViewHolder(holder, i);
        holder.itemView.measure(
            View.MeasureSpec.makeMeasureSpec(view.getWidth(), View.MeasureSpec.EXACTLY),
            View.MeasureSpec.makeMeasureSpec(0, View.MeasureSpec.UNSPECIFIED));
        holder.itemView.layout(0, 0, holder.itemView.getMeasuredWidth(),
            holder.itemView.getMeasuredHeight());
        holder.itemView.setDrawingCacheEnabled(true);
        holder.itemView.buildDrawingCache();
        Bitmap drawingCache = holder.itemView.getDrawingCache();
        if (drawingCache != null) {

          bitmaCache.put(String.valueOf(i), drawingCache);
        }
        height += holder.itemView.getMeasuredHeight();
      }

      bigBitmap = Bitmap.createBitmap(view.getMeasuredWidth(), height, Bitmap.Config.ARGB_8888);
      Canvas bigCanvas = new Canvas(bigBitmap);
      Drawable lBackground = view.getBackground();
      if (lBackground instanceof ColorDrawable) {
        ColorDrawable lColorDrawable = (ColorDrawable) lBackground;
        int lColor = lColorDrawable.getColor();
        bigCanvas.drawColor(lColor);
      }

      for (int i = 0; i < size; i++) {
        Bitmap bitmap = bitmaCache.get(String.valueOf(i));
        bigCanvas.drawBitmap(bitmap, 0f, iHeight, paint);
        iHeight += bitmap.getHeight();
        bitmap.recycle();
      }
    }
    return bigBitmap;
  }
```

 


## 六： 其他屏幕相关工具

### 1.获取状态栏高度常见方式

```java
public static int getStatusH(Activity ctx) {
	Rect s = new Rect();
	ctx.getWindow().getDecorView().getWindowVisibleDisplayFrame(s);
	return s.top;
}
```


```java
public static int getStatusH(Context ctx) {
    int statusHeight = -1;
    try {
        Class<?> clazz = Class.forName("com.android.internal.R$dimen");
        Object object = clazz.newInstance();
        int height = Integer.parseInt(clazz.getField("status_bar_height")
                .get(object).toString());
        statusHeight = ctx.getResources().getDimensionPixelSize(height);
    } catch (Exception e) {
        e.printStackTrace();
    }
    return statusHeight;
}
```

```java
public static int getStatusHeight(Activity activity) {
    int resourceId = activity.getResources().getIdentifier("status_bar_height", "dimen", "android");
    return resourceId > 0 ? activity.getResources().getDimensionPixelSize(resourceId) : 0;
} 
```


### 2.获取标题栏高度
```java
public static int getTitleH(Activity ctx) {
    int contentTop = ctx.getWindow()
            .findViewById(Window.ID_ANDROID_CONTENT).getTop();
    return contentTop - getStatusH(ctx);
}
```

### 3.获取屏幕宽高
```java
public static int getScreenW(Context ctx) {
    int w = 0;
    if (Build.VERSION.SDK_INT > 13) {
        Point p = new Point();
        ((WindowManager) ctx.getSystemService(Context.WINDOW_SERVICE))
                .getDefaultDisplay().getSize(p);
        w = p.x;
    } else {
        w = ((WindowManager) ctx.getSystemService(Context.WINDOW_SERVICE))
                .getDefaultDisplay().getWidth();
    }
    return w;
}
```

```java
public static int getScreenH(Context ctx) {
    int h = 0;
    if (Build.VERSION.SDK_INT > 13) {
        Point p = new Point();
        ((WindowManager) ctx.getSystemService(Context.WINDOW_SERVICE))
                .getDefaultDisplay().getSize(p);
        h = p.y;
    } else {
        h = ((WindowManager) ctx.getSystemService(Context.WINDOW_SERVICE))
                .getDefaultDisplay().getHeight();
    }
    return h;
}
```
```java
//获得屏幕高度
public static int getScreenWidth(Context context) {
    WindowManager wm = (WindowManager) context
            .getSystemService(Context.WINDOW_SERVICE);
    DisplayMetrics outMetrics = new DisplayMetrics();
    wm.getDefaultDisplay().getMetrics(outMetrics);
    return outMetrics.widthPixels;
}
```

引用：
[Android滚动截屏，ScrollView截屏，Listview截屏，Recyclerview截屏](http://www.cnblogs.com/BoBoMEe/p/4556917.html)



