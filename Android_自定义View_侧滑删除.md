<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Android ListView滑动删除及响应事件详解](#android-listview%E6%BB%91%E5%8A%A8%E5%88%A0%E9%99%A4%E5%8F%8A%E5%93%8D%E5%BA%94%E4%BA%8B%E4%BB%B6%E8%AF%A6%E8%A7%A3)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->





```java
/**
 * 侧拉删除控件
 */
public class SwipeLayout extends FrameLayout {
  //关键点：ViewDragHelper对象mDragHelper
   private ViewDragHelper mDragHelper;
  
   private View mBackView; // 后布局
   private View mFrontView; // 前布局
   private int mHeight;
   private int mWidth;
   private int mRange; // 拖拽范围     
   @Override
   protected void onFinishInflate() {
      super.onFinishInflate();      
      mBackView = getChildAt(0);
      mFrontView = getChildAt(1);
   }   
   @Override
   protected void onSizeChanged(int w, int h, int oldw, int oldh) {
      super.onSizeChanged(w, h, oldw, oldh);      
      mHeight = getMeasuredHeight();
      mWidth = getMeasuredWidth();      
      mRange = mBackView.getMeasuredWidth();
   }
  
   public SwipeLayout(Context context) {
      this(context, null);
   }
   public SwipeLayout(Context context, AttributeSet attrs) {
      this(context, attrs, 0);
   }
   public SwipeLayout(Context context, AttributeSet attrs, int defStyle) {
      super(context, attrs, defStyle);      
      // 1. 创建ViewDragHelper对象
      mDragHelper = ViewDragHelper.create(this, mCallback);
   }
   // 2. 转交拦截判断和触摸事件  
   @Override
   public boolean onInterceptTouchEvent(android.view.MotionEvent ev) {
      return mDragHelper.shouldInterceptTouchEvent(ev);
   }
   @Override
   public boolean onTouchEvent(MotionEvent event) {      
      try {
         // 交由DragHelper处理触摸事件
         mDragHelper.processTouchEvent(event);
      } catch (Exception e) {
         e.printStackTrace();
      }      
      return true;
   }

   // 3. 重写事件回调ViewDragHelper.Callback
   ViewDragHelper.Callback mCallback = new ViewDragHelper.Callback() {      
      // 返回值决定了child是否可以滑动
      @Override
      public boolean tryCaptureView(View child, int pointerId) {
         return true;
      }

     //可以滑动的范围
      @Override
      public int getViewHorizontalDragRange(View child) {
         return mRange;
      };
      
      // 修正将要移动到的位置
      @Override
      public int clampViewPositionHorizontal(View child, int left, int dx) {
         // left 建议值
         if(child == mFrontView){
            if(left < -mRange){
               // 限制前布局的左边界
               left = -mRange;
            }else if (left > 0) {
               // 限制前布局的右边界
               left = 0;
            }
         }else if (child == mBackView) {
            if(left < mWidth - mRange){
               left = mWidth - mRange;
               // 限制后布局的左边界
            }else if (left > mWidth) {
               // 限制后布局的右边界
               left = mWidth;
            }
         }
         return left;
      };
      
      // 控件移动后要做的事情
      @Override
      public void onViewPositionChanged(View changedView, int left, int top, int dx, int dy) {         
         if(changedView == mFrontView){
            // 将前布局的偏移量传递给后布局
            mBackView.offsetLeftAndRight(dx);
         }else if (changedView == mBackView) {
            // 将后布局的偏移量传递给前布局
            mFrontView.offsetLeftAndRight(dx);
         }
         
         dispathSwipeEvent();//更新状态, 执行监听
         
         invalidate();// 重绘界面, 兼容低版本
      };
      
      // 控件被释放, 做动画
      @Override
      public void onViewReleased(View releasedChild, float xvel, float yvel) {
         // xvel 水平方向速度
         if(xvel == 0 && mFrontView.getLeft() < -mRange * 0.5f){
            open();
         }else if (xvel < 0) {
            open();
         }else {
            close();
         }
      };      
   };
  
   public enum Status {
      Close, Open, Swiping;
   }
   private Status status = Status.Close;
   /** 更新状态, 执行监听 */
   protected void dispathSwipeEvent() {      
      // 记录上一个状态
      Status lastStatus = status;
      // 获取最新状态
      status = updateStatus();
      
      if(lastStatus != status && onSwipeListener != null){
         if(status == Status.Open){
            onSwipeListener.onOpen(this);
         }else if (status == Status.Close) {
            onSwipeListener.onClose(this);
         }else {
            if(lastStatus == Status.Close){
               onSwipeListener.onStartOpen(this);
            }else if (lastStatus == Status.Open) {
               onSwipeListener.onStartClose(this);
            }
         }
      }      
   }

  /** 获取最新状态 */
   private Status updateStatus() {
      int left = mFrontView.getLeft();
      if(left == 0){
         return Status.Close;
      }else if (left == -mRange) {
         return Status.Open;
      }
      return Status.Swiping;
   }   
   
   /** 滑动监听 */
   public interface OnSwipeListener{      
      void onClose(SwipeLayout layout);
      void onOpen(SwipeLayout layout);      
      void onStartOpen(SwipeLayout layout);
      void onStartClose(SwipeLayout layout);      
   }
   private OnSwipeListener onSwipeListener;
   public void setOnSwipeListener(OnSwipeListener onSwipeListener) {
      this.onSwipeListener = onSwipeListener;
   }   
   public OnSwipeListener getOnSwipeListener() {
      return onSwipeListener;
   }

   // 开启
   public void open(boolean isSmooth){
      int finalLeft = -mRange;
      if(isSmooth){
         // 1. 触发平滑动画
         if(mDragHelper.smoothSlideViewTo(mFrontView, finalLeft, 0)){
            ViewCompat.postInvalidateOnAnimation(this);
         }
      }else {
         layoutContent(true);//根据开关状态更新界面布局
      }
   }
   public void open() {
      open(true);
   }

   // 关闭
   public void close(boolean isSmooth){
      int finalLeft = 0;
      if(isSmooth){
         // 1. 触发平滑动画
         if(mDragHelper.smoothSlideViewTo(mFrontView, finalLeft, 0)){
            ViewCompat.postInvalidateOnAnimation(this);
         }
      }else {
         layoutContent(true);//根据开关状态更新界面布局
      }
   }  
   public void close() {
      close(true);
   }
   
   // 2. 维持平滑动画的继续
   @Override
   public void computeScroll() {
      super.computeScroll();      
      if(mDragHelper.continueSettling(true)){
         ViewCompat.postInvalidateOnAnimation(this);
      }
   }
   
   @Override
   protected void onLayout(boolean changed, int left, int top, int right,
         int bottom) {
//    super.onLayout(changed, left, top, right, bottom);
      layoutContent(false);
   }

   /**
    * 根据开关状态更新界面布局
    * @param isOpen
    */
   private void layoutContent(boolean isOpen) {
      // 摆放前布局
      Rect frontRect = computeFrontRect(isOpen);
      mFrontView.layout(frontRect.left, frontRect.top, frontRect.right, frontRect.bottom);
      
      // 摆放后布局
      Rect backRect = computeBackRectViaFront(frontRect);
      mBackView.layout(backRect.left, backRect.top, backRect.right, backRect.bottom);
      
      // 将某个布局前置
      bringChildToFront(mFrontView);
   }

   /**
    * 通过前布局矩形得到后布局矩形
    * @param frontRect
    * @return
    */
   private Rect computeBackRectViaFront(Rect frontRect) {
      int left = frontRect.right;
      return new Rect(left, 0, left + mRange, 0 + mHeight);
   }

   /**
    * 通过当前开关状态, 计算得到前布局的矩形区域
    * @param isOpen 
    * @return 矩形区域
    */
   private Rect computeFrontRect(boolean isOpen) {
      int left = 0;
      if(isOpen){
         left = - mRange;
      }
      return new Rect(left, 0, left + mWidth, 0 + mHeight);
   }   
}
```

# [Android ListView滑动删除及响应事件详解](http://www.cnblogs.com/ticktack/p/7028306.html)

目标：实现类似QQ，微信的消息列表滑动删除

具体操作：

**1. 主页面布局**

​    首先在布局文件(本例是**activity_main.xml**)中引入ListView控件，并指定id（如下代码中黑体部分）。

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/activity_main"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
    <ListView
        android:id="@+id/list_view"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        />
</RelativeLayout>
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

**2. 定义滑动布局类**

​    我们需有个工具类提供滑动的布局效果，即监听触摸动作，产生页面滑动效果。github上有很多强大的滑动类，博主最终引用了非常简易，容易操作的SwipeLayout.java。(如果你不关心滑动布局的实现机制，那么在工程中创建SwipeLayout.java类，拷贝**附录1**中的代码可。如果你想了解更多，可参考博客见具体实现过程：http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2015/0422/2771.html)

**3. 定制ListView子项的布局**

   ListView确定的是整体样式，即列表排列，我们还可以定制列表中每行所放内容的样式。接下来我们的目标是像QQ信息列表那样，每行以图片开头，图片旁是联系人姓名。所以，在drawable目录下准备几张头像图片以及删除按钮的图标，然后在layout目录下新建**activity_item.xml**，代码如下所示：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <!--引用步骤2中添加的SwipeLayout.java布局类，包名为你项目中SwaipeLayout.java存放的路径-->
    <com.example.whjth.swipelayout_demo.SwipeLayout
        android:layout_width="match_parent"
        android:layout_height="50dp">
        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="50dp"
            android:background="#ffffff">
            <ImageView
                android:id="@+id/image"
                android:layout_width="50dp"
                android:layout_height="50dp"
                android:padding="5dp"
                android:src="@drawable/p10"/>     <!--p10为存放在drawable目录下的头像图片名称-->
            <TextView
                android:id="@+id/name"
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:layout_gravity="center_horizontal"
                android:padding="5dp"
                android:layout_marginLeft="10dp"
                android:text= "xingming"
                />
        </LinearLayout>
        <LinearLayout
            android:layout_width="50dp"
            android:layout_height="50dp">
            <RelativeLayout
                android:id="@+id/delete_button"
                android:clickable="true"
                android:layout_width="50dp"
                android:layout_height="50dp"
                android:background="#ff0000">
                <View
                    android:layout_centerInParent="true"
                    android:layout_width="28dp"
                    android:layout_height="28dp"
                    android:background="@drawable/trash"/>  <!--trash为删除图标-->
            </RelativeLayout>
        </LinearLayout>
    </com.example.whjth.swipelayout_demo.SwipeLayout>
</LinearLayout>
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 在上述代码中，第二个LinearLayout标签的宽度属性是“match_parent"，这代表它与父布局（屏幕）宽度相同，从而遮蔽了第三个LineaLayout的内容，无法显现，只有通过滑动才可见。效果如下面左图所示；如果注释掉第二个LinearLayout内容，则可以看见第三个LinearLayout中的内容，如下面右图所示。

 ![img](https://images2015.cnblogs.com/blog/1101119/201706/1101119-20170616223641056-939076151.jpg)     ![img](https://images2015.cnblogs.com/blog/1101119/201706/1101119-20170616224513743-1531583231.jpg)

**4. 创建展示内容的类对象**

**   ** 布局文件都准备好了，下面就是往列表中写入内容了。我们发现列表中的每一项都具有同样的属性，即图片+名称。所以为了方便操作，我们把准备写入的内容当成一个类对象，为它定义图片和名称属性，实质上就是一个Java Bean。本例将其命名为**Person.java**，代码见下：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
public class Person {
    private String name;
    private int ImageId;

    public Person(String name, int ImageId){
        this.name = name;
        this.ImageId = ImageId;
    }
    public String getName(){
        return name;
    }
    public int getImageId(){
        return ImageId;
    }
}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

**5. 定义传递数据至ListView的适配器**

**   **** **让我们暂时停一下，理理思路，总结一下之前的工作：（1）我们在总布局中引入ListView控件（2）并为它定制了样式（3）载入了可以感知滑动的工具类（4）又将需要放入其中的内容整理成了类对象，这使我们可以通过数组的形式引用大量数据。

​    所以，我们剩下的工作是将数据传递到ListView中。不过，数组中的数据无法直接传递给ListView的，我们需要借助适配器完成。Android中提供了很多适配器的实现类，本例中使用的是ArrayAdapter。它可以通过泛型来指定要适配的数据类型（本例数据类型为步骤4中定义的Person），然后在构造函数中把要适配的数据传入， 然后在ArrayAdapter的构造函数中依次传入当前上下文、ListView子项布局的id，以及要适配的数据。本例将其命名为**\*PersonAdapter.java***，代码见下：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
import android.content.Context;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;
import android.widget.ArrayAdapter;
import android.widget.ImageView;
import android.widget.TextView;
import android.widget.Toast;

import java.util.List;

public class PersonAdapter extends ArrayAdapter<Person>{

    private int resourceId;
    private Context context;

    /* 重写构造函数，即上文中划横线部分的实现 */
    public PersonAdapter(Context context, int textViewResourceId, List<Person> objects){
        super(context, textViewResourceId, objects);
        resourceId = textViewResourceId;
        this.context = context;
    }
    /* 重写getView()方法，该方法在每个子项被滚动到屏幕内的时候会被调用 */
    public View getView(int position, View convertView, final ViewGroup parent){
        final Person person = getItem(position); //获取当前项的Person实例
        View view;
        ViewHolder viewHolder;  //下文中定义的内部类，用于保存image，name，delete等实例，而不是每次滑动加载时都通过findViewById的方法获得控件实例
         /* getView()方法中的converView参数表示之前加载的布局 */
        if(convertView == null){
            /* 如果converView参数值为null，则使用LayoutInflater去加载布局 */
            view = LayoutInflater.from(getContext()).inflate(resourceId, parent, false);
            viewHolder = new ViewHolder();
```

```
            /* 调用View的findViewById()方法分别获得image和name实例 */
```

```
            viewHolder.image = (ImageView) view.findViewById(R.id.image);
            viewHolder.name = (TextView) view.findViewById(R.id.name);
            viewHolder.delete = view.findViewById(R.id.delete_button);
            view.setTag(viewHolder);
        }
        else{
            /* 否则，直接对converView进行重用 */
            view = convertView;
            viewHolder = (ViewHolder) view.getTag();
        }

        /* 调用setImageResource()和setText()方法来设置显示的图片和文字 */
        viewHolder.image.setImageResource(person.getImageId());
        viewHolder.name.setText(person.getName());

        /* 以下黑体为事件监听响应部分，即点击删除图标和头像会分别显示提醒信息 */
        viewHolder.delete = view.findViewById(R.id.delete_button);
        viewHolder.delete.setOnClickListener(new View.OnClickListener(){
            public void onClick(View v){
                Toast.makeText(getContext(), "you clicked delete button", Toast.LENGTH_SHORT).show();
            }
        });
        viewHolder.image.setOnClickListener(new View.OnClickListener(){
            public void onClick(View v){
                Toast.makeText(getContext(), "you clicked image", Toast.LENGTH_SHORT).show();
            }
        });
 
        return view; //返回布局
    }
    class ViewHolder{
        ImageView image;
        TextView name;
        View delete;
    }
}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

   数据适配完成，下面只用在主程序中调用ListView的setAdapter()方法，将构建好的适配器对象传递进去，这样ListView和数据之间的关联就完成了。

**6. 编写主程序**

   在主程序中初始化列表条目，并通过适配器将其值传入ListView。参考代码**\*MainActivity.java***见下：

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.widget.ListView;

import java.util.ArrayList;
import java.util.List;

public class MainActivity extends AppCompatActivity {

    private List<Person> PersonList = new ArrayList<>();

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        initPerson();
        PersonAdapter adapter = new PersonAdapter(MainActivity.this, R.layout.activity_item, PersonList);
        ListView listView = (ListView) findViewById(R.id.list_view);
        listView.setAdapter(adapter);
    }

    private void initPerson(){
        for(int i=0; i<2; i++) {
            Person person = new Person("Alex", R.drawable.p1);
            PersonList.add(person);
            person = new Person("Brandon", R.drawable.p2);
            PersonList.add(person);
            person = new Person("Charles", R.drawable.p3);
            PersonList.add(person);
            person = new Person("Davie", R.drawable.p4);
            PersonList.add(person);
            person = new Person("Eric", R.drawable.p5);
            PersonList.add(person);
            person = new Person("Fanny", R.drawable.p6);
            PersonList.add(person);
            person = new Person("Grace", R.drawable.p7);
            PersonList.add(person);
            person = new Person("Helen", R.drawable.p8);
            PersonList.add(person);
            person = new Person("Isabelle", R.drawable.p9);
            PersonList.add(person);
            person = new Person("Jenny", R.drawable.p10);
            PersonList.add(person);
        }
    }
}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

**效果图：**

![img](https://images2015.cnblogs.com/blog/1101119/201706/1101119-20170616235051556-1880263423.jpg)![img](https://images2015.cnblogs.com/blog/1101119/201706/1101119-20170616235109681-854826958.jpg)![img](https://images2015.cnblogs.com/blog/1101119/201706/1101119-20170616235144306-1794829040.jpg)![img](https://images2015.cnblogs.com/blog/1101119/201706/1101119-20170616235205728-1357097151.jpg)

**附录1**：SwipeLayout.java

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

```
import android.content.Context;
import android.support.v4.view.ViewCompat;
import android.support.v4.widget.ViewDragHelper;
import android.util.AttributeSet;
import android.view.MotionEvent;
import android.view.View;
import android.widget.LinearLayout;

```

```
/**
 * Created by Bruce on 11/24/14.
 */
public class SwipeLayout extends LinearLayout {

    private ViewDragHelper viewDragHelper;
    private View contentView;
    private View actionView;
    private int dragDistance;
    private final double AUTO_OPEN_SPEED_LIMIT = 800.0;
    private int draggedX;

    public SwipeLayout(Context context) {
        this(context, null);
    }

    public SwipeLayout(Context context, AttributeSet attrs) {
        this(context, attrs, -1);
    }

    public SwipeLayout(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        viewDragHelper = ViewDragHelper.create(this, new DragHelperCallback());
    }

    @Override
    protected void onFinishInflate() {
        contentView = getChildAt(0);
        actionView = getChildAt(1);
        actionView.setVisibility(GONE);
    }

    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);

        dragDistance = actionView.getMeasuredWidth();
    }

    private class DragHelperCallback extends ViewDragHelper.Callback {

        @Override
        public boolean tryCaptureView(View view, int i) {
            return view == contentView || view == actionView;
        }

        @Override
        public void onViewPositionChanged(View changedView, int left, int top, int dx, int dy) {
            draggedX = left;
            if (changedView == contentView) {
                actionView.offsetLeftAndRight(dx);
            } else {
                contentView.offsetLeftAndRight(dx);
            }
            if (actionView.getVisibility() == View.GONE) {
                actionView.setVisibility(View.VISIBLE);
            }
            invalidate();
        }

        @Override
        public int clampViewPositionHorizontal(View child, int left, int dx) {
            if (child == contentView) {
                final int leftBound = getPaddingLeft();
                final int minLeftBound = -leftBound - dragDistance;
                final int newLeft = Math.min(Math.max(minLeftBound, left), 0);
                return newLeft;
            } else {
                final int minLeftBound = getPaddingLeft() + contentView.getMeasuredWidth() - dragDistance;
                final int maxLeftBound = getPaddingLeft() + contentView.getMeasuredWidth() + getPaddingRight();
                final int newLeft = Math.min(Math.max(left, minLeftBound), maxLeftBound);
                return newLeft;
            }
        }

        @Override
        public int getViewHorizontalDragRange(View child) {
            return dragDistance;
        }

        @Override
        public void onViewReleased(View releasedChild, float xvel, float yvel) {
            super.onViewReleased(releasedChild, xvel, yvel);
            boolean settleToOpen = false;
            if (xvel > AUTO_OPEN_SPEED_LIMIT) {
                settleToOpen = false;
            } else if (xvel < -AUTO_OPEN_SPEED_LIMIT) {
                settleToOpen = true;
            } else if (draggedX <= -dragDistance / 2) {
                settleToOpen = true;
            } else if (draggedX > -dragDistance / 2) {
                settleToOpen = false;
            }

            final int settleDestX = settleToOpen ? -dragDistance : 0;
            viewDragHelper.smoothSlideViewTo(contentView, settleDestX, 0);
            ViewCompat.postInvalidateOnAnimation(SwipeLayout.this);
        }
    }

    @Override
    public boolean onInterceptTouchEvent(MotionEvent ev) {
        if(viewDragHelper.shouldInterceptTouchEvent(ev)) {
            return true;
        }
        return super.onInterceptTouchEvent(ev);
    }

    @Override
    public boolean onTouchEvent(MotionEvent event) {
        viewDragHelper.processTouchEvent(event);
        return true;
    }

    @Override
    public void computeScroll() {
        super.computeScroll();
        if(viewDragHelper.continueSettling(true)) {
            ViewCompat.postInvalidateOnAnimation(this);
        }
    }
}
```

[![复制代码](https://common.cnblogs.com/images/copycode.gif)](javascript:void(0);)

 

项目完整源代码下载：https://github.com/Vera97/ListView-SwipeLayout-Demo