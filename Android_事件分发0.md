

# 【Android 事件分发】
之前在学习android事件方法机制的时候，看过不少文章，但是大部分都讲的不是很清楚，我自己理解的也是云里雾里，也尝试过阅读源码，看得我更是不知所措。最近阅读了《Android开发艺术探索》一书中相关的章节，茅塞顿开，写下本文作为阅读笔记，以便以后查阅。

## 三个重要的方法
### dispatchTouchEvent

```java
    public boolean dispatchTouchEvent(MotionEvent ev)
```

事件传递过来的时候这个方法**第一个被调用**，返回结果受当前View的ontouchEvent()方法或者下一级View的dispatchTouchEvent()方法返回值影响。

### onInterceptTouchEvent
```java
    public boolean onInterceptTouchEvent(MotionEvent ev)
```

这个方法是**在dispatchTouchEvent()方法内部调用**的，返回值用来判断**是否拦截当前事件**。

### onTouchEvent
```java
    public boolean onTouchEvent(MotionEvent ev)
```

也是**在dispatchTouchEvent()方法中调用**，用来处理某一事件。

## 事件的传递规则

书中用了一段伪代码来表示

```java
    public boolean dispatchTouchEvent(MotionEvent ev) {
        boolean consume = false;
        if (onInterceptTouchEvent(ev)) {
            consume = onTouchEvent(ev);
        } else {
            consume = child.dispatchTouchEvent(ev);
        }
        return consume;
    }
```

也就是说当一个事件到来的时候，当前View的**dispatchTouchEvent**方法会被调用，在**内部首先调用onInterceptTouchEvent判断是否拦截**：
　如果**拦截**，将**事件传递给自己的onTouchEvent**对事件进行处理；
　如果**不拦截**，就将事件**传递给子View**，调用**子View的dispatchTouchEvent**方法，一直到事件被消费。

## 源码分析

上面的内容讲的很抽象，不好理解，接下来配合源码来讲解，这样更加的容易深入理解事件分发机制。

### 判断是否拦截

事件到来的时候，View的第一个工作自然是判断是否拦截，下面给出dispatchTouchEvent中拦截的相关代码

```java
/* dispatchTouchEvent中拦截的相关代码 */
// Check for interception.
final boolean intercepted;
if (actionMasked == MotionEvent.ACTION_DOWN
        || mFirstTouchTarget != null) {//mFirstTouchTarget在子元素成功处理事件的时候会进行赋值    
    final boolean disallowIntercept = (mGroupFlags & FLAG_DISALLOW_INTERCEPT) != 0;
    //FLAG_DISALLOW_INTERCEPT一般是通过子View调用requestDisallowInterceptTouchEvent方法设置
    if (!disallowIntercept) {
        intercepted = onInterceptTouchEvent(ev);
        ev.setAction(action); // restore action in case it was changed
    } else {
        intercepted = false;
    }
} else {
//当事件不是DOWN，而且没有子元素成功处理的时候，直接拦截事件自己处理
    // There are no touch targets and this action is not an initial down
    // so this view group continues to intercept touches.
    intercepted = true;
}
```

这里要注意的是，事件分发机制针对的其实可以看作是一系列的事件，也就是一个事件序列，也就是说一个事件序列由一个DOWN开头，中间n个MOVE，然后以UP或者CANCEL结束。

代码中mFirstTouchTarget在子元素成功处理事件的时候会进行赋值，也就是说当事件不是DOWN，而且没有子元素成功处理的时候，直接拦截事件自己处理。这很好理解，如果不是DOWN说明事件序列已经开始传递了，那么如果子元素不处理最开始的DOWN说明它不想要这个序列，那么就自己处理，一直到新的事件序列到来（也就是新的DOWN）。也就是说**一旦我们处理一个事件就不会多次调用onInterceptTouchEvent方法**。

另一种情况是**DOWN到来**，也就是新的事件序列开始，**或**者**子View成功处理过这个序列**，就会进行判断。
　判断第一步是**判断FLAG_DISALLOW_INTERCEPT**标志位，这个标志位是**通过requestDisallowInterceptTouchEvent方法设置**的，一般是**子View调用**的。
　　如果**不允许拦截**，就**不拦截**。
　　如果**允许**，那就**调用自己的onInterceptTouchEvent**方法来判断。

值得注意的是**当DOWN事件到来**的时候，会**重置标志位**，且**清除mFirstTouchTarget**，就是**新序列到来**的时候**一切重置**。
```java
// Handle an initial down.
if (actionMasked == MotionEvent.ACTION_DOWN) {
    // Throw away all previous state when starting a new touch gesture.
    // The framework may have dropped the up or cancel event for the previous gesture
    // due to an app switch, ANR, or some other state change.
    cancelAndClearTouchTargets(ev);
    resetTouchState();
}
```

### 不拦截事件

如果最后不拦截事件，那么就应该分发下去

```java
{
         final int childIndex = customOrder
                 ? getChildDrawingOrder(childrenCount, i) : i;
         final View child = (preorderedList == null)
                 ? children[childIndex] : preorderedList.get(childIndex);

         // If there is a view that has accessibility focus we want it
         // to get the event first and if not handled we will perform a
         // normal dispatch. We may do a double iteration but this is
         // safer given the timeframe.
         if (childWithAccessibilityFocus != null) {
             if (childWithAccessibilityFocus != child) {
                 continue;
             }
             childWithAccessibilityFocus = null;
             i = childrenCount - 1;
         }

         if (!canViewReceivePointerEvents(child)
                 || !isTransformedTouchPointInView(x, y, child, null)) {
             ev.setTargetAccessibilityFocus(false);
             continue;
         }

         newTouchTarget = getTouchTarget(child);
         if (newTouchTarget != null) {
             // Child is already receiving touch within its bounds.
             // Give it the new pointer in addition to the ones it is handling.
             newTouchTarget.pointerIdBits |= idBitsToAssign;
             break;
         }

         resetCancelNextUpFlag(child);
         if (dispatchTransformedTouchEvent(ev, false, child, idBitsToAssign)) {
             // Child wants to receive touch within its bounds.
             mLastTouchDownTime = ev.getDownTime();
             if (preorderedList != null) {
                 // childIndex points into presorted list, find original index
                 for (int j = 0; j < childrenCount; j++) {
                     if (children[childIndex] == mChildren[j]) {
                         mLastTouchDownIndex = j;
                         break;
                     }
                 }
             } else {
                 mLastTouchDownIndex = childIndex;
             }
             mLastTouchDownX = ev.getX();
             mLastTouchDownY = ev.getY();
             newTouchTarget = addTouchTarget(child, idBitsToAssign);
             alreadyDispatchedToNewTouchTarget = true;
             break;
         }

         // The accessibility focus didn't handle the event, so clear
         // the flag and do a normal dispatch to all children.
         ev.setTargetAccessibilityFocus(false);
}
```

就是**遍历子View**，通过**是否在播放动画**和**事件是否落在它的范围内来获得合适的View**，如果存在就调用它的dispatchTouchEvent方法。 
我们需要获得**dispatchTouchEvent返回的值**来**判断子View是否成功消耗了事件**，如果返回的是**true**代表**成功**消费，那么就会**对mFirstTouchTarget进行赋值**

```java
    newTouchTarget = addTouchTarget(child, idBitsToAssign);
    alreadyDispatchedToNewTouchTarget = true;
    break;
```

这个赋值很重要，如果不消耗那么就不会赋值，也就是说mFirstTouchTarget== null，那么接下来的事件（同一序列，也就不会再产生DOWN了）都有本View消耗，不再分发。

当然，如果最后发现**没有合适的子View或者子View返回了false**，那么都**由本View处理**，也就是**onTouchEvent**，这也就是为什么事件到了最底层还没被消耗（返回true）就会重新向上传递到上一层的onTouchEvent处理的原因了。

### 拦截事件

那就开始自己处理事件，接下来的内容就会详细讲解。

### View对事件的处理

```java
ListenerInfo li = mListenerInfo;
if (li != null && li.mOnTouchListener != null
        && (mViewFlags & ENABLED_MASK) == ENABLED
        && li.mOnTouchListener.onTouch(this, event)) {
    result = true;
}

if (!result && onTouchEvent(event)) {
    result = true;
}
```

这里的View不包含ViewGroup，可以看到当要处理事件的时候首先判断是否设置了OnTouchListener，如果设置了就调用onTouch方法。如果onTouch返回了true，那么就直接返回，不会去调用ontouchEvent。如果返回了false，就回调用ontouchEvent，返回onTouchEvent的返回值。 
在onTouchEvent内部，如果设置了OnClickListener就会调用onClick方法。 
总的来说，就是onTouchListener级别高于onTouchEvent，onClickListener最低。

## 案例解析

针对上述的理论分析，我们通过以下的Demo来结合实践加深理解。 
首先自定义一个MyViewGroup和MyView，代码如下

```java
public class MyView extends View {
    public MyView(Context context) {
        super(context);
    }

    public MyView(Context context, AttributeSet attrs) {
        super(context, attrs);
    }
}
```

```java
public class MyViewGroup extends ViewGroup {
    private MyView mChildView;
    public MyViewGroup(Context context) {
        this(context, null);
    }
    public MyViewGroup(Context context, AttributeSet attrs) {
        super(context, attrs);
    }

    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
        measureChildren(widthMeasureSpec, heightMeasureSpec);
    }

    @Override
    protected void onLayout(boolean changed, int l, int t, int r, int b) {
        if (changed) {
            mChildView = (MyView) getChildAt(0);
            mChildView.layout(l, t, l + mChildView.getMeasuredWidth(), t + mChildView.getMeasuredHeight());
        }
    }

}

```

很简单的自定义View和ViewGroup，我们接下来在布局文件中加入就可以了

```
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools" android:layout_width="match_parent"
    android:layout_height="match_parent" android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    android:paddingBottom="@dimen/activity_vertical_margin" tools:context=".MainActivity">

    <com.wulingpeng.viewtouchdispatch.MyViewGroup
        android:layout_width="match_parent"
        android:layout_height="match_parent" >

        <com.wulingpeng.viewtouchdispatch.MyView
            android:layout_width="300dp"
            android:layout_height="300dp"
            android:background="@android:color/holo_blue_bright"/>

    </com.wulingpeng.viewtouchdispatch.MyViewGroup>
</RelativeLayout>
```

现在我们重写MyViewGroup和View的相关方法并打印结果

MyView.java

```
    @Override
    public boolean dispatchTouchEvent(MotionEvent ev) {
        boolean dispatch = super.dispatchTouchEvent(ev);
        Log.d("Debug", "MyView:dispatchTouchEvent " + dispatch);
        return dispatch;
    }

    @Override
    public boolean onTouchEvent(MotionEvent event) {
        boolean onTouchEvent = super.onTouchEvent(event);
        Log.d("Debug", "MyView:OnTouchEvent " + onTouchEvent);
        return onTouchEvent;
    }
```

MyViewGroup.java

```
    @Override
    public boolean dispatchTouchEvent(MotionEvent ev) {
        boolean dispatch = super.dispatchTouchEvent(ev);
        Log.d("Debug", "MyViewGroup:dispatchTouchEvent " + dispatch);
        return dispatch;
    }

    @Override
    public boolean onInterceptTouchEvent(MotionEvent event) {
        boolean isIntercept = false;
        switch (event.getAction()) {
            case MotionEvent.ACTION_DOWN:
                isIntercept = true;
        }
        Log.d("Debug", "MyViewGroup:onInterceptTouchEvent " + isIntercept);
        return isIntercept;
    }

    @Override
    public boolean onTouchEvent(MotionEvent event) {
        boolean onTouchEvent = super.onTouchEvent(event);
        Log.d("Debug", "MyViewGroup:OnTouchEvent " + onTouchEvent);
        return onTouchEvent;
    }
```

这里我们拦截了DOWN事件，接下来点击MyView的区域然后滑动，最后抬起。

![这里写图片描述](http://upload-images.jianshu.io/upload_images/9028834-2dcffbfe3ca09c7b?imageMogr2/auto-orient/strip)

打印结果如下

```
07-23 08:57:18.067 2831-2831/? D/Debug: MyViewGroup:onInterceptTouchEvent true
07-23 08:57:18.067 2831-2831/? D/Debug: MyViewGroup:OnTouchEvent false
07-23 08:57:18.067 2831-2831/? D/Debug: MyViewGroup:dispatchTouchEvent false
```

明明滑动了一段距离，理论上有很多个MOVE事件，为什么只有三个打印呢？其实之前就已经说明了，我们拦截了DOWN事件，那么子元素是收不到DOWN事件的，结果就是该序列接下来的事件都是我们自己消费，且不会再次掉用onInterceptTouchEvent，由自己的onTouchEvent处理。因为我们的onTouchEvent返回了false，直接导致我们的dispatchTouchEvent也返回了false。那么MyViewGroup的上一层就不会把接下来的事件传递给我们了（上一层的mFirstTouchTarget没有赋值），所以接下来的事件都不会到来。

我们再改变一下，让MyViewGroup的onTouchEvent方法返回true，进行相同的操作，打印结果如下

```
07-23 09:08:48.727 3018-3018/? D/Debug: MyViewGroup:onInterceptTouchEvent true
07-23 09:08:48.727 3018-3018/? D/Debug: MyViewGroup:OnTouchEvent true
07-23 09:08:48.727 3018-3018/? D/Debug: MyViewGroup:dispatchTouchEvent true
......
```

省略的打印信息就是第二条和第三条的多次重复，也就是说在接下来的MOVE到来的时候，由于之前拦截了DOWN，所以事件自己处理，不会再掉用onIntereptTouchEvent。

## 注意事项

- 一般在处理滑动冲突的时候重写相关方法，对于DOWN事件是不会拦截的，也就是返回false，在接下来的MOVE序列中判断是否需要拦截。因为如果拦截了DOWN，那么接下来的事件都不会传给子View了，之前已经分析过了。
- 一般也不会拦截UP事件，因为UP一般为序列的最后一个事件，拦截不拦截对自己没有什么用处，但是子View就可能因为收不到UP而无法触发click事件。




**引用：**
[Android事件分发机制完全解析，带你从源码的角度彻底理解(上)](http://blog.csdn.net/sinyu890807/article/details/9097463)
[Android事件分发机制完全解析，带你从源码的角度彻底理解(下)](http://blog.csdn.net/sinyu890807/article/details/9153747)
[Android事件分发机制详解](http://blog.csdn.net/a62321780/article/details/51986515)

