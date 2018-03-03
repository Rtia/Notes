http://www.jb51.net/article/88663.htm

首先呈上Android循环滚轮效果图：

 ![img](http://files.jb51.net/file_images/article/201607/2016715113642164.gif?2016615113651)

现在很多地方都用到了滚轮布局WheelView，比如在选择生日的时候，风格类似系统提供的DatePickerDialog，开源的控件也有很多，不过大部分都是根据当前项目的需求绘制的界面，因此我就自己写了一款比较符合自己项目的WheelView。
首先这个控件有以下的**需求**：
 1、能够循环滚动，当向上或者向下滑动到临界值的时候，则循环开始滚动
 2、中间的一块有一块半透明的选择区，滑动结束时，哪一块在这个选择区，就选择这快。
 3、继承自View进行绘制 

然后进行**一些关键点**的讲解： 
1、整体控件继承自View，在onDraw中进行绘制。整体包含三个模块，整个View、每一块的条目、中间选择区的条目（额外绘制一块灰色区域）。 
2、通过动态设置或者默认设置的可显示条目数，在最上和最下再各加入一块，意思就是一共绘制showCount+2个条目。 
3、当最上面的条目数滑动超过条目高度的一半时，进行动态条目更新：将最下面的条目删除加入第一个条目、将第一个条目删除加入最下面的条目。 
4、外界可设置条目显示数、字体大小、颜色、选择区提示文字（图中那个年字）、默认选择项、padding补白等等。 
5、在onTouchEvent中，得到手指滑动的渐变值，动态更新当前所有的条目。 
6、在onMeasure中动态计算宽度，所有条目的宽度、高度、起始Y坐标等等。 
7、通过当前条目和被选择条目的坐标，超过一半则视为被选择，并且滑动到对应的位置。 

下面的是WheelView代码，主要是计算初始值、得到外面设置的值： 

```java
package cc.wxf.view.wheel;

import android.content.Context;
import android.graphics.Canvas;
import android.graphics.Color;
import android.graphics.Paint;
import android.util.AttributeSet;
import android.view.MotionEvent;
import android.view.View;

import java.util.ArrayList;
import java.util.List;

/**
 * Created by ccwxf on 2016/3/31.
 */
public class WheelView extends View {

 public static final int FONT_COLOR = Color.BLACK;
 public static final int FONT_SIZE = 30;
 public static final int PADDING = 10;
 public static final int SHOW_COUNT = 3;
 public static final int SELECT = 0;
 //总体宽度、高度、Item的高度
 private int width;
 private int height;
 private int itemHeight;
 //需要显示的行数
 private int showCount = SHOW_COUNT;
 //当前默认选择的位置
 private int select = SELECT;
 //字体颜色、大小、补白
 private int fontColor = FONT_COLOR;
 private int fontSize = FONT_SIZE;
 private int padding = PADDING;
 //文本列表
 private List<String> lists;
 //选中项的辅助文本，可为空
 private String selectTip;
 //每一项Item和选中项
 private List<WheelItem> wheelItems = new ArrayList<WheelItem>();
 private WheelSelect wheelSelect = null;
 //手点击的Y坐标
 private float mTouchY;
 //监听器
 private OnWheelViewItemSelectListener listener;

 public WheelView(Context context) {
 super(context);
 }

 public WheelView(Context context, AttributeSet attrs) {
 super(context, attrs);
 }

 public WheelView(Context context, AttributeSet attrs, int defStyleAttr) {
 super(context, attrs, defStyleAttr);
 }

 /**
 * 设置字体的颜色，不设置的话默认为黑色
 * @param fontColor
 * @return
 */
 public WheelView fontColor(int fontColor){
 this.fontColor = fontColor;
 return this;
 }

 /**
 * 设置字体的大小，不设置的话默认为30
 * @param fontSize
 * @return
 */
 public WheelView fontSize(int fontSize){
 this.fontSize = fontSize;
 return this;
 }

 /**
 * 设置文本到上下两边的补白，不合适的话默认为10
 * @param padding
 * @return
 */
 public WheelView padding(int padding){
 this.padding = padding;
 return this;
 }

 /**
 * 设置选中项的复制文本，可以不设置
 * @param selectTip
 * @return
 */
 public WheelView selectTip(String selectTip){
 this.selectTip = selectTip;
 return this;
 }

 /**
 * 设置文本列表，必须且必须在build方法之前设置
 * @param lists
 * @return
 */
 public WheelView lists(List<String> lists){
 this.lists = lists;
 return this;
 }

 /**
 * 设置显示行数，不设置的话默认为3
 * @param showCount
 * @return
 */
 public WheelView showCount(int showCount){
 if(showCount % 2 == 0){
  throw new IllegalStateException("the showCount must be odd");
 }
 this.showCount = showCount;
 return this;
 }

 /**
 * 设置默认选中的文本的索引，不设置默认为0
 * @param select
 * @return
 */
 public WheelView select(int select){
 this.select = select;
 return this;
 }

 /**
 * 最后调用的方法，判断是否有必要函数没有被调用
 * @return
 */
 public WheelView build(){
 if(lists == null){
  throw new IllegalStateException("this method must invoke after the method [lists]");
 }
 return this;
 }

 @Override
 protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
 //得到总体宽度
 width = MeasureSpec.getSize(widthMeasureSpec) - getPaddingLeft() - getPaddingRight();
 // 得到每一个Item的高度
 Paint mPaint = new Paint();
 mPaint.setTextSize(fontSize);
 Paint.FontMetrics metrics = mPaint.getFontMetrics();
 itemHeight = (int) (metrics.bottom - metrics.top) + 2 * padding;
 //初始化每一个WheelItem
 initWheelItems(width, itemHeight);
 //初始化WheelSelect
 wheelSelect = new WheelSelect(showCount / 2 * itemHeight, width, itemHeight, selectTip, fontColor, fontSize, padding);
 //得到所有的高度
 height = itemHeight * showCount;
 super.onMeasure(widthMeasureSpec, MeasureSpec.makeMeasureSpec(height, MeasureSpec.EXACTLY));
 }

 /**
 * 创建显示个数+2个WheelItem
 * @param width
 * @param itemHeight
 */
 private void initWheelItems(int width, int itemHeight) {
 wheelItems.clear();
 for(int i = 0; i < showCount + 2; i++){
  int startY = itemHeight * (i - 1);
  int stringIndex = select - showCount / 2 - 1 + i;
  if(stringIndex < 0){
  stringIndex = lists.size() + stringIndex;
  }
  wheelItems.add(new WheelItem(startY, width, itemHeight, fontColor, fontSize, lists.get(stringIndex)));
 }
 }

 @Override
 public boolean onTouchEvent(MotionEvent event) {
 switch (event.getAction()){
  case MotionEvent.ACTION_DOWN:
  mTouchY = event.getY();
  return true;
  case MotionEvent.ACTION_MOVE:
  float dy = event.getY() - mTouchY;
  mTouchY = event.getY();
  handleMove(dy);
  break;
  case MotionEvent.ACTION_UP:
  handleUp();
  break;
 }
 return super.onTouchEvent(event);
 }

 /**
 * 处理移动操作
 * @param dy
 */
 private void handleMove(float dy) {
 //调整坐标
 for(WheelItem item : wheelItems){
  item.adjust(dy);
 }
 invalidate();
 //调整
 adjust();
 }

 /**
 * 处理抬起操作
 */
 private void handleUp(){
 int index = -1;
 //得到应该选择的那一项
 for(int i = 0; i < wheelItems.size(); i++){
  WheelItem item = wheelItems.get(i);
  //如果startY在selectItem的中点上面，则将该项作为选择项
  if(item.getStartY() > wheelSelect.getStartY() && item.getStartY() < (wheelSelect.getStartY() + itemHeight / 2)){
  index = i;
  break;
  }
  //如果startY在selectItem的中点下面，则将上一项作为选择项
  if(item.getStartY() >= (wheelSelect.getStartY() + itemHeight / 2) && item.getStartY() < (wheelSelect.getStartY() + itemHeight)){
  index = i - 1;
  break;
  }
 }
 //如果没找到或者其他因素，直接返回
 if(index == -1){
  return;
 }
 //得到偏移的位移
 float dy = wheelSelect.getStartY() - wheelItems.get(index).getStartY();
 //调整坐标
 for(WheelItem item : wheelItems){
  item.adjust(dy);
 }
 invalidate();
 // 调整
 adjust();
 //设置选择项
 int stringIndex = lists.indexOf(wheelItems.get(index).getText());
 if(stringIndex != -1){
  select = stringIndex;
  if(listener != null){
  listener.onItemSelect(select);
  }
 }
 }

 /**
 * 调整Item移动和循环显示
 */
 private void adjust(){
 //如果向下滑动超出半个Item的高度，则调整容器
 if(wheelItems.get(0).getStartY() >= -itemHeight / 2 ){
  //移除最后一个Item重用
  WheelItem item = wheelItems.remove(wheelItems.size() - 1);
  //设置起点Y坐标
  item.setStartY(wheelItems.get(0).getStartY() - itemHeight);
  //得到文本在容器中的索引
  int index = lists.indexOf(wheelItems.get(0).getText());
  if(index == -1){
  return;
  }
  index -= 1;
  if(index < 0){
  index = lists.size() + index;
  }
  //设置文本
  item.setText(lists.get(index));
  //添加到最开始
  wheelItems.add(0, item);
  invalidate();
  return;
 }
 //如果向上滑超出半个Item的高度，则调整容器
 if(wheelItems.get(0).getStartY() <= (-itemHeight / 2 - itemHeight)){
  //移除第一个Item重用
  WheelItem item = wheelItems.remove(0);
  //设置起点Y坐标
  item.setStartY(wheelItems.get(wheelItems.size() - 1).getStartY() + itemHeight);
  //得到文本在容器中的索引
  int index = lists.indexOf(wheelItems.get(wheelItems.size() - 1).getText());
  if(index == -1){
  return;
  }
  index += 1;
  if(index >= lists.size()){
  index = 0;
  }
  //设置文本
  item.setText(lists.get(index));
  //添加到最后面
  wheelItems.add(item);
  invalidate();
  return;
 }
 }

 /**
 * 得到当前的选择项
 */
 public int getSelectItem(){
 return select;
 }

 @Override
 protected void onDraw(Canvas canvas) {
 //绘制每一项Item
 for(WheelItem item : wheelItems){
  item.onDraw(canvas);
 }
 //绘制阴影
 if(wheelSelect != null){
  wheelSelect.onDraw(canvas);
 }
 }

 /**
 * 设置监听器
 * @param listener
 * @return
 */
 public WheelView listener(OnWheelViewItemSelectListener listener){
 this.listener = listener;
 return this;
 }

 public interface OnWheelViewItemSelectListener{
 void onItemSelect(int index);
 }
}


```

然后是每一个条目类，根据当前的坐标进行绘制，根据渐变值改变坐标等： 

```java
package cc.wxf.view.wheel;

import android.graphics.Canvas;
import android.graphics.Paint;
import android.graphics.RectF;

/**
 * Created by ccwxf on 2016/3/31.
 */
public class WheelItem {
 // 起点Y坐标、宽度、高度
 private float startY;
 private int width;
 private int height;
 //四点坐标
 private RectF rect = new RectF();
 //字体大小、颜色
 private int fontColor;
 private int fontSize;
 private String text;
 private Paint mPaint = new Paint(Paint.ANTI_ALIAS_FLAG);

 public WheelItem(float startY, int width, int height, int fontColor, int fontSize, String text) {
 this.startY = startY;
 this.width = width;
 this.height = height;
 this.fontColor = fontColor;
 this.fontSize = fontSize;
 this.text = text;
 adjust(0);
 }

 /**
 * 根据Y坐标的变化值，调整四点坐标值
 * @param dy
 */
 public void adjust(float dy){
 startY += dy;
 rect.left = 0;
 rect.top = startY;
 rect.right = width;
 rect.bottom = startY + height;
 }

 public float getStartY() {
 return startY;
 }

 /**
 * 直接设置Y坐标属性，调整四点坐标属性
 * @param startY
 */
 public void setStartY(float startY) {
 this.startY = startY;
 rect.left = 0;
 rect.top = startY;
 rect.right = width;
 rect.bottom = startY + height;
 }

 public void setText(String text) {
 this.text = text;
 }

 public String getText() {
 return text;
 }

 public void onDraw(Canvas mCanvas){
 //设置钢笔属性
 mPaint.setTextSize(fontSize);
 mPaint.setColor(fontColor);
 //得到字体的宽度
 int textWidth = (int)mPaint.measureText(text);
 //drawText的绘制起点是左下角,y轴起点为baseLine
 Paint.FontMetrics metrics = mPaint.getFontMetrics();
 int baseLine = (int)(rect.centerY() + (metrics.bottom - metrics.top) / 2 - metrics.bottom);
 //居中绘制
 mCanvas.drawText(text, rect.centerX() - textWidth / 2, baseLine, mPaint);
 }
}


```

 最后是选择项，就是额外得在中间区域绘制一块灰色区域： 

```java
package cc.wxf.view.wheel;

import android.graphics.Canvas;
import android.graphics.Color;
import android.graphics.Paint;
import android.graphics.Rect;

/**
 * Created by ccwxf on 2016/4/1.
 */
public class WheelSelect {
 //黑框背景颜色
 public static final int COLOR_BACKGROUND = Color.parseColor("#77777777");
 //黑框的Y坐标起点、宽度、高度
 private int startY;
 private int width;
 private int height;
 //四点坐标
 private Rect rect = new Rect();
 //需要选择文本的颜色、大小、补白
 private String selectText;
 private int fontColor;
 private int fontSize;
 private int padding;
 private Paint mPaint = new Paint(Paint.ANTI_ALIAS_FLAG);

 public WheelSelect(int startY, int width, int height, String selectText, int fontColor, int fontSize, int padding) {
 this.startY = startY;
 this.width = width;
 this.height = height;
 this.selectText = selectText;
 this.fontColor = fontColor;
 this.fontSize = fontSize;
 this.padding = padding;
 rect.left = 0;
 rect.top = startY;
 rect.right = width;
 rect.bottom = startY + height;
 }

 public int getStartY() {
 return startY;
 }

 public void setStartY(int startY) {
 this.startY = startY;
 }

 public void onDraw(Canvas mCanvas) {
 //绘制背景
 mPaint.setStyle(Paint.Style.FILL);
 mPaint.setColor(COLOR_BACKGROUND);
 mCanvas.drawRect(rect, mPaint);
 //绘制提醒文字
 if(selectText != null){
  //设置钢笔属性
  mPaint.setTextSize(fontSize);
  mPaint.setColor(fontColor);
  //得到字体的宽度
  int textWidth = (int)mPaint.measureText(selectText);
  //drawText的绘制起点是左下角,y轴起点为baseLine
  Paint.FontMetrics metrics = mPaint.getFontMetrics();
  int baseLine = (int)(rect.centerY() + (metrics.bottom - metrics.top) / 2 - metrics.bottom);
  //在靠右边绘制文本
  mCanvas.drawText(selectText, rect.right - padding - textWidth, baseLine, mPaint);
 }
 }
}


```

 源代码就三个文件，很简单，注释也很详细，接下来就是使用文件了： 

```java
 final WheelView wheelView = (WheelView) findViewById(R.id.wheelView);
 final List<String> lists = new ArrayList<>();
 for(int i = 0; i < 20; i++){
  lists.add("test:" + i);
 }
 wheelView.lists(lists).fontSize(35).showCount(5).selectTip("年").select(0).listener(new WheelView.OnWheelViewItemSelectListener() {
  @Override
  public void onItemSelect(int index) {
  Log.d("cc", "current select:" + wheelView.getSelectItem() + " index :" + index + ",result=" + lists.get(index));
  }
 }).build();

```

这个控件说简单也简单，说复杂也挺复杂，从最基础的onDraw实现，可以非常高灵活度地定制各自的需求。