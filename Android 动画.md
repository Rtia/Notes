#Android 动画
[TOC]
# 动画基础概念
## 动画分类
Android 中动画分为两种，一种是 Tween 动画、还有一种是 Frame 动画。
### 补间动画(Tween动画)
这种实现方式可以使视图组件**移动、放大、缩小以及产生透明度**的变化；
### 帧动画(Frame 动画)
传统的动画方法，通过顺序的**播放排列好的图片**来实现，类似电影、gif。
### 属性动画(Property animation)
自Android 3.0版本开始，系统给我们提供了一种全新的动画模式，属性动画(property animation)，它的功能非常强大，弥补了之前补间动画的一些缺陷，几乎是可以完全替代掉补间动画了。


## 补间动画(Tween动画)
### 1、透明度动画AlphaAnimation 
```
	// 透明度动画
	public void alpha(View v) {
		AlphaAnimation alphaAnimation = new AlphaAnimation(0, 1);
/*		AlphaAnimation (float fromAlpha, float toAlpha)
		fromAlpha: 动画的起始alpha值 （范围：0:完全透明 -1:完全不透明）
		toAlpha：终止的值，动画结束的值	*/		
		alphaAnimation.setDuration(3000);// 每次动画持续时间3秒
		alphaAnimation.setFillAfter(true);// 动画最后是否停留在终止的状态
		alphaAnimation.setRepeatCount(3);// 动画重复的次数
		alphaAnimation.setRepeatMode(Animation.REVERSE);// REVERSE： 反转模式
														// RESTART：重新开始
		// 动画监听
		alphaAnimation.setAnimationListener(new AnimationListener() {

			@Override
			public void onAnimationStart(Animation animation) {
				System.out.println("动画开始回调");
			}

			@Override
			public void onAnimationRepeat(Animation animation) {
				System.out.println("动画重复回调");

			}

			@Override
			public void onAnimationEnd(Animation animation) {
				System.out.println("动画结束回调");
				Toast.makeText(getApplicationContext(), "动画结束，进入陌陌关心你界面",
						Toast.LENGTH_LONG).show();

			}
		});
		iconIv.startAnimation(alphaAnimation);

	}
```

### 2、平移动画TranslateAnimation
```	
/* TranslateAnimation (int fromXType, 
                float fromXValue, 
                int toXType, 
                float toXValue, 
                int fromYType, 
                float fromYValue, 
                int toYType, 
                float toYValue)
	原点：控件第一次绘制的左上角的坐标点
	fromXType（起点，相对于原点偏移方式）：
			Animation.ABSOLUTE 绝对值，像素值
			Animation.RELATIVE_TO_SELF 相对于自己
			Animation.RELATIVE_TO_PARENT 相对于父控件.
	fromXValue（起点，相对于原点偏移量）：
			绝对值/百分比		
*/		
		TranslateAnimation translateAnimation = new TranslateAnimation(
				Animation.ABSOLUTE,
				iconIv.getWidth(), // 当前屏幕密度 :240 标准的屏幕密度：160 则dp转px ：
									// px=dp*240/160
				Animation.ABSOLUTE, iconIv.getWidth(), 
				Animation.ABSOLUTE, 0,
				Animation.RELATIVE_TO_SELF, 1);
		translateAnimation.setDuration(3000);// 每次动画持续时间3秒
		translateAnimation.setFillAfter(true);// 动画最后停留在终止的状态
		translateAnimation.setRepeatCount(3);// 动画重复的次数
		translateAnimation.setRepeatMode(Animation.REVERSE);// REVERSE： 反转模式
															// RESTART：重新开始
		translateAnimation.setInterpolator(new BounceInterpolator());// 设置特效，弹簧效果
		iconIv.startAnimation(translateAnimation);
		System.out.println("控件的宽度" + iconIv.getWidth());

	}

```

### 3、缩放动画ScaleAnimation 
```
/* ScaleAnimation (float fromX, 
                float toX, 
                float fromY, 
                float toY, 
                int pivotXType, 
                float pivotXValue, 
                int pivotYType, 
                float pivotYValue)
	fromX: 缩放起始比例-水平方向
	toX: 缩放最终比例-水平方向
	pivotXType（中心点相较于原点 x方向的类型）: 
			Animation.ABSOLUTE
			Animation.RELATIVE_TO_SELF
			RELATIVE_TO_PARENT.
	pivotXValue: 绝对值/百分比	
*/
	public void scale(View v) {
		ScaleAnimation scaleAnimation =new ScaleAnimation
				(0, 2, 0, 2, Animation.RELATIVE_TO_SELF, 0.5f, Animation.RELATIVE_TO_SELF, 0.5f);
		scaleAnimation.setDuration(3000);// 每次动画持续时间3秒
		scaleAnimation.setFillAfter(true);// 动画最后停留在终止的状态
		scaleAnimation.setRepeatCount(1);// 动画重复的次数
		iconIv.startAnimation(scaleAnimation);

	}

```

### 4、旋转动画 rotate
创建一个Animation类型的XML文件；
```
<?xml version="1.0" encoding="utf-8"?>
<rotate xmlns:android="http://schemas.android.com/apk/res/android"
    android:fromDegrees="0"
    android:toDegrees="180"
    android:duration="3000"
    android:interpolator="@android:anim/overshoot_interpolator"
    android:fillAfter="true"
    android:repeatCount="2"
    android:repeatMode="reverse"
    android:pivotX="50%"
    android:pivotY="50%"
    >
    <!--fromDegrees:起始的度数
      toDegrees：终止的度数
      infinite：无限次数 
      起始度数大于终止度数，则能逆时针旋转，否则顺时针
      android:pivotX="50%":旋转围绕的轴心，x方向位置，相对于自己的宽度的一半
      android:pivotX="50%p":相对于父控件宽度的一半
      -->
    

</rotate>
```
```
Animation animation1 = AnimationUtils.loadAnimation(this,R.anim.rotate);
imageView.startAnimation(animation1);
```
### 复合动画
```
AnimationSet animationSet=new AnimationSet(false);
animationSet.addAnimation(animation1);
//这样在这里面添加就可以了；		
Animation rotateAnimation = AnimationUtils.loadAnimation(this, R.anim.rotate);
animationSet.addAnimation(rotateAnimation);
```


## 帧动画(Frame 动画)
### 方式一；使用XML的方式；

1、创建一个AnimationDrawable 的ＸＭＬ文件；
```
<?xml version="1.0" encoding="utf-8"?>
<animation-list xmlns:android="http://schemas.android.com/apk/res/android" android:oneshot="false" >//这个是反复执行的设置；
     <item android:drawable="@drawable/emoji_088" android:duration="150" />
    <item android:drawable="@drawable/emoji_095" android:duration="150" />
    <item android:drawable="@drawable/emoji_096" android:duration="150" />
    <item android:drawable="@drawable/emoji_097" android:duration="150" />
    <item android:drawable="@drawable/emoji_098" android:duration="150" />
    <item android:drawable="@drawable/emoji_099" android:duration="150" />
    <item android:drawable="@drawable/emoji_100" android:duration="150" />
    <item android:drawable="@drawable/emoji_101" android:duration="150" />
    <item android:drawable="@drawable/emoji_102" android:duration="50" />
    <item android:drawable="@drawable/emoji_103" android:duration="50" />
    <item android:drawable="@drawable/emoji_104" android:duration="50" />
</animation-list>
<!--android:drawable[drawable]//加载Drawable对象
    android:duration[long]//每一帧动画的持续时间（单位ms）
    android:oneshot[boolean]//动画是否只运行一次，true运行一次，false重复运行
    android:visible[boolean]//Drawable对象的初始能见度状态，true可见，false不可见（默认为false）-->
```

２、第二步就是需要将这个帧动画设置到一个容器中去；imageview;
 android:src="@drawable/animation" />

3、可以从控件中获取到这个帧动画的图片然后再对他进行操作；
```
drawable = (AnimationDrawable) imageView.getDrawable();

if (drawable.isRunning()) {
			drawable.stop();
		}else{
			drawable.start();
		}
```

### 方式二：使用代码的方式进行；
方式1：添加多个帧 Drawable
```
		mAnimationDrawable = new AnimationDrawable();
		mAnimationDrawable.setOneShot(false);//是否执行一次
		//添加多个帧 Drawable
		for(int i=0;i<11;i++){
			Drawable frame = getResources().getDrawable(R.drawable.girl_1+i);
			mAnimationDrawable.addFrame(frame, 200);
		}

		mImageView.setImageDrawable(mAnimationDrawable);//设置帧动画，默认是停止状态

```		
方式2：引用xml 帧动画drawable
```
		
		// 引用xml 帧动画drawable
		mAnimationDrawable=(AnimationDrawable) getResources().getDrawable(R.drawable.frame);
		mImageView.setImageDrawable(mAnimationDrawable);
```

## 属性动画(Property animation)

### 为什么要引入属性动画？
Android之前的补间动画机制其实还算是比较健全的，在android.view.animation包下面有好多的类可以供我们操作，来完成一系列的动画效果，比如说对View进行移动、缩放、旋转和淡入淡出，并且我们还可以借助AnimationSet来将这些动画效果组合起来使用，除此之外还可以通过配置Interpolator来控制动画的播放速度等等等等。那么这里大家可能要产生疑问了，既然之前的动画机制已经这么健全了，为什么还要引入属性动画呢？

其实上面所谓的健全都是相对的，如果你的需求中只需要对View进行移动、缩放、旋转和淡入淡出操作，那么补间动画确实已经足够健全了。但是很显然，这些功能是不足以覆盖所有的场景的，一旦我们的需求超出了移动、缩放、旋转和淡入淡出这四种对View的操作，那么补间动画就不能再帮我们忙了，也就是说它在功能和可扩展方面都有相当大的局限性，那么下面我们就来看看补间动画所不能胜任的场景。

注意上面我在介绍补间动画的时候都有使用“对View进行操作”这样的描述，没错，**补间动画是只能够作用在View上**的。也就是说，我们可以对一个Button、TextView、甚至是LinearLayout、或者其它任何继承自View的组件进行动画操作，但是如果我们想要对一个非View的对象进行动画操作，抱歉，补间动画就帮不上忙了。可能有的朋友会感到不能理解，我怎么会需要对一个非View的对象进行动画操作呢？这里我举一个简单的例子，比如说我们有一个自定义的View，在这个View当中有一个Point对象用于管理坐标，然后在onDraw()方法当中就是根据这个Point对象的坐标值来进行绘制的。也就是说，如果我们可以对Point对象进行动画操作，那么整个自定义View的动画效果就有了。显然，补间动画是不具备这个功能的，这是它的第一个缺陷。

然后**补间动画**还有一个缺陷，就是它**只能够实现移动、缩放、旋转和淡入淡出这四种动画操作**，那如果我们希望可以对View的背景色进行动态地改变呢？很遗憾，我们只能靠自己去实现了。说白了，之前的补间动画机制就是使用硬编码的方式来完成的，功能限定死就是这些，基本上没有任何扩展性可言。

最后，**补间动画**还有一个致命的缺陷，就是它**只是改变了View的显示效果而已，而不会真正去改变View的属性**。什么意思呢？比如说，现在屏幕的左上角有一个按钮，然后我们通过补间动画将它移动到了屏幕的右下角，现在你可以去尝试点击一下这个按钮，点击事件是绝对不会触发的，因为实际上这个按钮还是停留在屏幕的左上角，只不过补间动画将这个按钮绘制到了屏幕的右下角而已。

也正是因为这些原因，Android开发团队决定在3.0版本当中引入属性动画这个功能，那么属性动画是不是就把上述的问题全部解决掉了？下面我们就来一起看一看。

新引入的属性动画机制已经不再是针对于View来设计的了，也不限定于只能实现移动、缩放、旋转和淡入淡出这几种动画操作，同时也不再只是一种视觉上的动画效果了。它实际上是一种不断地对值进行操作的机制，并将值赋值到指定对象的指定属性上，可以是任意对象的任意属性。所以我们仍然可以将一个View进行移动或者缩放，但同时也可以对自定义View中的Point对象进行动画操作了。我们只需要告诉系统动画的运行时长，需要执行哪种类型的动画，以及动画的初始值和结束值，剩下的工作就可以全部交给系统去完成了。

既然属性动画的实现机制是通过对目标对象进行赋值并修改其属性来实现的，那么之前所说的按钮显示的问题也就不复存在了，如果我们通过属性动画来移动一个按钮，那么这个按钮就是真正的移动了，而不再是仅仅在另外一个位置绘制了而已。


### ValueAnimator

ValueAnimator是整个属性动画机制当中最核心的一个类，前面我们已经提到了，**属性动画的运行机制是通过不断地对值进行操作来实现的**，而初始值和结束值之间的**动画过渡**就是**由ValueAnimator**这个类来负责**计算**的。
它的内部使用一种时间循环的机制来计算值与值之间的动画过渡，我们只需要将初始值和结束值提供给ValueAnimator，并且告诉它动画所需运行的**时长**，那么ValueAnimator就会自动帮我们完成从初始值平滑地过渡到结束值这样的效果。除此之外，ValueAnimator还负责管理动画的播放**次数**、播放**模式**、以及**对动画设置监听器**等，确实是一个非常重要的类。

但是ValueAnimator的用法却一点都不复杂，我们先从最简单的功能看起吧，比如说想要将一个值从0平滑过渡到1，时长300毫秒，就可以这样写：
```
ValueAnimator anim = ValueAnimator.ofFloat(0f, 1f);  
anim.setDuration(300);  
anim.start();  
```
怎么样？很简单吧，调用ValueAnimator的**ofFloat()**方法就可以构建出一个ValueAnimator的实例，ofFloat()方法当中允许传入多个float类型的参数，这里传入0和1就表示将值从0平滑过渡到1，然后调用ValueAnimator的setDuration()方法来设置动画运行的时长，最后调用start()方法启动动画。

用法就是这么简单，现在如果你运行一下上面的代码，动画就会执行了。可是这只是一个将值从0过渡到1的动画，又看不到任何界面效果，我们怎样才能知道这个动画是不是已经真正运行了呢？这就需要借助监听器来实现了，如下所示：
```
ValueAnimator anim = ValueAnimator.ofFloat(0f, 1f);  
anim.setDuration(300);  
anim.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {  
    @Override  
    public void onAnimationUpdate(ValueAnimator animation) {  
        float currentValue = (float) animation.getAnimatedValue();  
        Log.d("TAG", "cuurent value is " + currentValue);  
    }  
});  
anim.start();  
```
可以看到，这里我们通过**addUpdateListener()**方法来添加一个动画的**监听器**，在动画执行的过程中会不断地进行回调，我们只需要在回调方法当中将当前的值取出并打印出来，就可以知道动画有没有真正运行了。运行上述代码，控制台打印如下所示：
![](http://img.blog.csdn.net/20171221143810600?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


从打印日志的值我们就可以看出，ValueAnimator确实已经在正常工作了，值在300毫秒的时间内从0平滑过渡到了1，而这个计算工作就是由ValueAnimator帮助我们完成的。另外ofFloat()方法当中是可以传入任意多个参数的，因此我们还可以构建出更加复杂的动画逻辑，比如说将一个值在5秒内从0过渡到5，再过渡到3，再过渡到10，就可以这样写：
```
ValueAnimator anim = ValueAnimator.ofFloat(0f, 5f, 3f, 10f);  
anim.setDuration(5000);  
anim.start();  
```
当然也许你并不需要小数位数的动画过渡，可能你只是希望将一个整数值从0平滑地过渡到100，那么也很简单，只需要调用ValueAnimator的ofInt()方法就可以了，如下所示：
```
ValueAnimator anim = ValueAnimator.ofInt(0, 100);  
```
#### 常用方法
**ValueAnimator**当中**最常用**的应该就是**ofFloat()和ofInt()**这两个方法了，另外**还有**一个**ofObject()**方法，我会在下篇文章进行讲解。

**setStartDelay()**：动画延迟播放的时间
**setRepeatCount()**：动画循环播放的次数
**setRepeatMode()**：循环播放的模式，循环模式包括RESTART和REVERSE两种，分别表示重新播放和倒序播放的意思。


### ObjectAnimator
**对任意对象的任意属性进行动画操作**

相比于ValueAnimator，ObjectAnimator可能才是我们最常接触到的类，因为ValueAnimator只不过是对值进行了一个平滑的动画过渡，但我们实际使用到这种功能的场景好像并不多。而ObjectAnimator则就不同了，它是可以直接任意对象的任意属性进行动画操作的，比如说View的alpha属性。

不过虽说ObjectAnimator会更加常用一些，但是它其实是**继承自ValueAnimator**的，底层的动画实现机制也是基于ValueAnimator来完成的，因此ValueAnimator仍然是整个属性动画当中最核心的一个类。

#### ObjectAnimator.ofFloat
```
ObjectAnimator ofFloat (Object target, 
                String propertyName, 
                float... values)
Constructs and returns an ObjectAnimator that animates between float values. A single value implies that that value is the one being animated to, in which case the start value will be derived from the property being animated and the target object when start() is called for the first time. Two values imply starting and ending values. More than two values imply a starting value, values to animate through along the way, and an ending value (these values will be distributed evenly across the duration of the animation).
Parameters
target
	Object: The object whose property is to be animated. This object should have a public method on it called setName(), where name is the value of the propertyName parameter.
propertyName
	String: The name of the property being animated.
values
	float: A set of values that the animation will animate between over time.
	
Returns
	ObjectAnimator
An ObjectAnimator object that is set up to animate between the given values.
```
那么既然是继承关系，说明ValueAnimator中可以使用的方法在ObjectAnimator中也是可以正常使用的，它们的用法也非常类似。
##### **alpha**
这里如果我们想要将一个TextView在5秒中内从常规变换成全透明，再从全透明变换成常规，就可以这样写：
```
ObjectAnimator animator = ObjectAnimator.ofFloat(textview, "alpha", 1f, 0f, 1f);  
/*
参数1：传入一个object对象，我们想要对哪个对象进行动画操作就传入什么，这里我传入了一个textview。
参数2：想要对该对象的哪个属性进行动画操作，由于我们想要改变TextView的不透明度，因此这里传入"alpha"。
参数3：不固定长度，想要完成什么样的动画就传入什么值，这里传入的值就表示将TextView从常规变换成全透明，再从全透明变换成常规。
*/
animator.setDuration(5000);  
animator.start();  
```
![](http://img.blog.csdn.net/20171221145655283?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

##### **rotation**
学会了这一个用法之后，其它的用法我们就可以举一反三了，那比如说我们想要将TextView进行一次360度的旋转，就可以这样写：
```
ObjectAnimator animator = ObjectAnimator.ofFloat(textview, "rotation", 0f, 360f);  
animator.setDuration(5000);  
animator.start();  
```
可以看到，这里我们将第二个参数改成了"rotation"，然后将动画的初始值和结束值分别设置成0和360，现在运行一下代码，效果如下图所示：
![](http://img.blog.csdn.net/20171221145730579?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

##### **translationX**
那么如果想要将TextView先向左移出屏幕，然后再移动回来，就可以这样写：
```
float curTranslationX = textview.getTranslationX();  
ObjectAnimator animator = ObjectAnimator.ofFloat(textview, "translationX", curTranslationX, -500f, curTranslationX);  
animator.setDuration(5000);  
animator.start();  
```
这里我们先是调用了TextView的getTranslationX()方法来获取到当前TextView的translationX的位置，然后ofFloat()方法的第二个参数传入"translationX"，紧接着后面三个参数用于告诉系统TextView应该怎么移动，现在运行一下代码，效果如下图所示：
![](http://img.blog.csdn.net/20171221145800271?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

##### **scaleY**
然后我们还可以TextView进行缩放操作，比如说将TextView在垂直方向上放大3倍再还原，就可以这样写：
```
ObjectAnimator animator = ObjectAnimator.ofFloat(textview, "scaleY", 1f, 3f, 1f);  
animator.setDuration(5000);  
animator.start();  
```
这里将ofFloat()方法的第二个参数改成了"scaleY"，表示在垂直方向上进行缩放，现在重新运行一下程序，效果如下图所示：
![](http://img.blog.csdn.net/20171221145833214?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

##### **工作机制**
ofFloat()方法的第二个参数到底可以传哪些值呢？目前我们使用过了alpha、rotation、translationX和scaleY这几个值，分别可以完成淡入淡出、旋转、水平移动、垂直缩放这几种动画，那么还有哪些值是可以使用的呢？

我们可以传入任意的值到ofFloat()方法的第二个参数当中。

因为ObjectAnimator在设计的时候就没有针对于View来进行设计，而是针对于任意对象的，它所负责的工作就是不断地向某个对象中的某个属性进行赋值，然后对象根据属性值的改变再来决定如何展现出来。

那么比如说我们调用下面这样一段代码：
```
ObjectAnimator.ofFloat(textview, "alpha", 1f, 0f);  
```
其实这段代码的意思就是ObjectAnimator会帮我们不断地改变textview对象中alpha属性的值，从1f变化到0f。然后textview对象需要根据alpha属性值的改变来不断刷新界面的显示，从而让用户可以看出淡入淡出的动画效果。

那么textview对象中是不是有alpha属性这个值呢？没有，不仅textview没有这个属性，连它所有的父类也是没有这个属性的！这就奇怪了，textview当中并没有alpha这个属性，ObjectAnimator是如何进行操作的呢？其实**ObjectAnimator内部的工作机制并不是直接对我们传入的属性名进行操作的，而是会去寻找这个属性名对应的get和set方法**，因此alpha属性所对应的get和set方法应该就是：
```
public void setAlpha(float value);  
public float getAlpha();  
```
那么textview对象中是否有这两个方法呢？确实有，并且这两个方法是由View对象提供的，也就是说不仅TextView可以使用这个属性来进行淡入淡出动画操作，任何继承自View的对象都可以的。

既然alpha是这个样子，相信大家一定已经明白了，前面我们所用的所有属性都是这个工作原理，那么View当中一定也存在着setRotation()、getRotation()、setTranslationX()、getTranslationX()、setScaleY()、getScaleY()这些方法，不信的话你可以到View当中去找一下。
####  
### 组合动画-AnimatorSet
实现组合动画功能主要需要借助**AnimatorSet**这个类，这个类提供了一个**play()**方法，如果我们向这个方法中传入一个Animator对象(ValueAnimator或ObjectAnimator)将会返回一个AnimatorSet.Builder的实例。
#### AnimatorSet.Builder
AnimatorSet.Builder中包括以下四个方法：
**after(Animator anim)**   将现有动画插入到传入的动画之后执行
**after(long delay)**   将现有动画延迟指定毫秒后执行
**before(Animator anim)**   将现有动画插入到传入的动画之前执行
**with(Animator anim)**   将现有动画和传入的动画同时执行


好的，有了这四个方法，我们就可以完成组合动画的逻辑了，那么比如说我们想要让TextView先从屏幕外移动进屏幕，然后开始旋转360度，旋转的同时进行淡入淡出操作，就可以这样写：
```
ObjectAnimator moveIn = ObjectAnimator.ofFloat(textview, "translationX", -500f, 0f);  
ObjectAnimator rotate = ObjectAnimator.ofFloat(textview, "rotation", 0f, 360f);  
ObjectAnimator fadeInOut = ObjectAnimator.ofFloat(textview, "alpha", 1f, 0f, 1f);  
AnimatorSet animSet = new AnimatorSet();  
animSet.play(rotate).with(fadeInOut).after(moveIn);  
animSet.setDuration(5000);  
animSet.start();  
```
可以看到，这里我们先是把三个动画的对象全部创建出来，然后new出一个AnimatorSet对象之后将这三个动画对象进行播放排序，让旋转和淡入淡出动画同时进行，并把它们插入到了平移动画的后面，最后是设置动画时长以及启动动画。运行一下上述代码，效果如下图所示：
![](http://img.blog.csdn.net/20171221150531181?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


### Animator监听器
#### AnimatorListener
Animator类当中提供了一个**addListener()**方法，这个方法接收一个AnimatorListener，我们只需要去实现这个**AnimatorListener**就可以监听动画的各种事件了。

大家已经知道，ObjectAnimator是继承自ValueAnimator的，而ValueAnimator又是继承自Animator的，因此不管是ValueAnimator还是ObjectAnimator都是可以使用addListener()这个方法的。另外AnimatorSet也是继承自Animator的，因此addListener()这个方法算是个通用的方法。

添加一个监听器的代码如下所示：
```
anim.addListener(new AnimatorListener() {  
    @Override  
    public void onAnimationStart(Animator animation) {  
	    //动画开始的时候调用
    }  
  
    @Override  
    public void onAnimationRepeat(Animator animation) {  
	    //动画重复执行的时候调用
    }  
  
    @Override  
    public void onAnimationEnd(Animator animation) {  
	    //动画结束的时候调用
    }  
  
    @Override  
    public void onAnimationCancel(Animator animation) {  
	    //动画被取消的时候调用
    }  
});  
```
可以看到，我们需要实现接口中的四个方法，onAnimationStart()方法会在动画开始的时候调用，onAnimationRepeat()方法会在动画重复执行的时候调用，onAnimationEnd()方法会在动画结束的时候调用，onAnimationCancel()方法会在动画被取消的时候调用。

#### AnimatorListenerAdapter
但是也许很多时候我们并不想要监听那么多个事件，可能我只想要监听动画结束这一个事件，那么每次都要将四个接口全部实现一遍就显得非常繁琐。没关系，为此Android提供了一个适配器类，叫作AnimatorListenerAdapter，使用这个类就可以**解决掉实现接口繁琐的问题**了，如下所示：
```
anim.addListener(new AnimatorListenerAdapter() {  
});  
```
这里我们向addListener()方法中传入这个适配器对象，由于**AnimatorListenerAdapter中已经将每个接口都实现好了**，所以这里不用实现任何一个方法也不会报错。那么如果我想监听动画结束这个事件，就只需要**单独重写**这一个**方法**就可以了，如下所示：
```
anim.addListener(new AnimatorListenerAdapter() {  
    @Override  
    public void onAnimationEnd(Animator animation) {  
    }  
});  
```

### 使用XML编写动画
我们可以使用代码来编写所有的动画功能，这也是最常用的一种做法。不过，过去的补间动画除了使用代码编写之外也是可以使用XML编写的，因此属性动画也提供了这一功能，即通过XML来完成和代码一样的属性动画功能。

通过XML来编写动画可能会比通过代码来编写动画要慢一些，但是在**重用**方面将会变得非常**轻松**，比如某个将通用的动画编写到XML里面，我们就可以在各个界面当中轻松去重用它。

如果想要使用XML来编写动画，首先要在res目录下面新建一个animator文件夹，所有属性动画的XML文件都应该存放在这个文件夹当中。
#### XML标签
##### animato
对应代码中的ValueAnimator
##### objectAnimator
对应代码中的ObjectAnimator
##### set
对应代码中的AnimatorSet

那么比如说我们想要实现一个从0到100平滑过渡的动画，在XML当中就可以这样写：
```
<animator xmlns:android="http://schemas.android.com/apk/res/android"  
    android:valueFrom="0"  
    android:valueTo="100"  
    android:valueType="intType"/>  
```

而如果我们想将一个视图的alpha属性从1变成0，就可以这样写：
```
<objectAnimator xmlns:android="http://schemas.android.com/apk/res/android"  
    android:valueFrom="1"  
    android:valueTo="0"  
    android:valueType="floatType"  
    android:propertyName="alpha"/>  
```


另外，我们也可以使用XML来完成复杂的组合动画操作，比如将一个视图先从屏幕外移动进屏幕，然后开始旋转360度，旋转的同时进行淡入淡出操作，就可以这样写：
```
<set xmlns:android="http://schemas.android.com/apk/res/android"  
    android:ordering="sequentially" >  
  
    <objectAnimator  
        android:duration="2000"  
        android:propertyName="translationX"  
        android:valueFrom="-500"  
        android:valueTo="0"  
        android:valueType="floatType" >  
    </objectAnimator>  
  
    <set android:ordering="together" >  
        <objectAnimator  
            android:duration="3000"  
            android:propertyName="rotation"  
            android:valueFrom="0"  
            android:valueTo="360"  
            android:valueType="floatType" >  
        </objectAnimator>  
  
        <set android:ordering="sequentially" >  
            <objectAnimator  
                android:duration="1500"  
                android:propertyName="alpha"  
                android:valueFrom="1"  
                android:valueTo="0"  
                android:valueType="floatType" >  
            </objectAnimator>  
            <objectAnimator  
                android:duration="1500"  
                android:propertyName="alpha"  
                android:valueFrom="0"  
                android:valueTo="1"  
                android:valueType="floatType" >  
            </objectAnimator>  
        </set>  
    </set>  
  
</set>  
```
这段XML实现的效果和我们刚才通过代码来实现的组合动画的效果是一模一样的。

#### 在代码中加载XML文件-AnimatorInflater
在代码中把文件加载进来并将动画启动：
```
Animator animator = AnimatorInflater.loadAnimator(context, R.animator.anim_file);  
animator.setTarget(view);  
animator.start();  
```
调用AnimatorInflater的loadAnimator来将XML动画文件加载进来，然后再调用setTarget()方法将这个动画设置到某一个对象上面，最后再调用start()方法启动动画就可以了。


### ValueAnimator的高级用法
目标：通过对对象进行值操作来实现动画效果

#### 实现目标
比如说我们有一个自定义的View，在这个View当中有一个Point对象用于管理坐标，然后在onDraw()方法当中就是根据这个Point对象的坐标值来进行绘制的。也就是说，如果我们可以对Point对象进行动画操作，那么整个自定义View的动画效果就有了。
效果图：![效果图](http://img.blog.csdn.net/20171221153249662?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

#### TypeEvaluator
简单来说，就是告诉动画系统如何从初始值过度到结束值。

ValueAnimator.ofFloat()方法就是实现了初始值与结束值之间的平滑过度，那么这个平滑过度是怎么做到的呢？
其实就是系统内置了一个FloatEvaluator，它通过计算告知动画系统如何从初始值过度到结束值，我们来看一下FloatEvaluator的代码实现：
```
public class FloatEvaluator implements TypeEvaluator {  
    public Object evaluate(float fraction, Object startValue, Object endValue) {  
        float startFloat = ((Number) startValue).floatValue();  
        return startFloat + fraction * (((Number) endValue).floatValue() - startFloat);  
    }  
}  
```
FloatEvaluator实现了TypeEvaluator接口，然后重写evaluate()方法。
evaluate()方法当中传入了三个参数：
参数1：fraction用于表示动画的完成度的，我们应该根据它来计算当前动画的值应该是多少，
参数2、3：分别表示动画的初始值和结束值。
代码逻辑：用结束值减去初始值，算出它们之间的差值，然后乘以fraction这个系数，再加上初始值，那么就得到当前动画的值了。

ValueAnimator的ofFloat()和ofInt()方法，分别用于对浮点型和整型的数据进行动画操作的，ValueAnimator中还有一个ofObject()方法，是用于对任意对象进行动画操作的。
但是相比于浮点型或整型数据，对象的动画操作明显要更复杂一些，因为系统将完全无法知道如何从初始对象过度到结束对象，因此这个时候我们就需要**实现一个自己的TypeEvaluator来告知系统如何进行过度**。

#### 实现步骤
##### 先定义一个Point类：
Point类非常简单，只有x和y两个变量用于记录坐标的位置，并提供了构造方法来设置坐标，以及get方法来获取坐标。
```
public class Point {  
  
    private float x;  
  
    private float y;  
  
    public Point(float x, float y) {  
        this.x = x;  
        this.y = y;  
    }  
  
    public float getX() {  
        return x;  
    }  
  
    public float getY() {  
        return y;  
    }  
  
}  
```
##### 自定义TypeEvaluator：
实现TypeEvaluator接口并重写了evaluate()方法：先是将startValue和endValue强转成Point对象，然后同样根据fraction来计算当前动画的x和y的值，最后组装到一个新的Point对象当中并返回。
```
public class PointEvaluator implements TypeEvaluator{  
  
    @Override  
    public Object evaluate(float fraction, Object startValue, Object endValue) {  
        Point startPoint = (Point) startValue;  
        Point endPoint = (Point) endValue;  
        float x = startPoint.getX() + fraction * (endPoint.getX() - startPoint.getX());  
        float y = startPoint.getY() + fraction * (endPoint.getY() - startPoint.getY());  
        Point point = new Point(x, y);  
        return point;  
    }  
  
}  
```


这样我们就将PointEvaluator编写完成了，接下来我们就可以非常轻松地对Point对象进行动画操作了，比如说我们有两个Point对象，现在需要将Point1通过动画平滑过度到Point2，就可以这样写：
需要注意的是，ofObject()方法要求多传入一个TypeEvaluator参数，这里我们只需要传入刚才定义好的PointEvaluator的实例就可以了。
```
Point point1 = new Point(0, 0);  
Point point2 = new Point(300, 300);  
ValueAnimator anim = ValueAnimator.ofObject(new PointEvaluator(), point1, point2);  
anim.setDuration(5000);  
anim.start();  
```


好的，这就是自定义TypeEvaluator的全部用法，掌握了这些知识之后，我们就可以来尝试一下如何通过对Point对象进行动画操作，从而实现整个自定义View的动画效果。

##### 自定义MyAnimView：
首先在自定义View的构造方法当中初始化了一个Paint对象作为画笔，并将画笔颜色设置为蓝色，接着在onDraw()方法当中进行绘制。
这里我们绘制的逻辑是由currentPoint这个对象控制的，如果currentPoint对象不等于空，那么就调用drawCircle()方法在currentPoint的坐标位置画出一个半径为50的圆，如果currentPoint对象是空，那么就调用startAnimation()方法来启动动画。
```
public class MyAnimView extends View {  
  
    public static final float RADIUS = 50f;  
  
    private Point currentPoint;  
  
    private Paint mPaint;  
  
    public MyAnimView(Context context, AttributeSet attrs) {  
        super(context, attrs);  
        mPaint = new Paint(Paint.ANTI_ALIAS_FLAG);  
        mPaint.setColor(Color.BLUE);  
    }  
  
    @Override  
    protected void onDraw(Canvas canvas) {  
        if (currentPoint == null) {  
            currentPoint = new Point(RADIUS, RADIUS);  
            drawCircle(canvas);  
            startAnimation();  
        } else {  
            drawCircle(canvas);  
        }  
    }  
  
    private void drawCircle(Canvas canvas) {  
        float x = currentPoint.getX();  
        float y = currentPoint.getY();  
        canvas.drawCircle(x, y, RADIUS, mPaint);  
    }  
  
    private void startAnimation() {  
        Point startPoint = new Point(RADIUS, RADIUS);  
        Point endPoint = new Point(getWidth() - RADIUS, getHeight() - RADIUS);  
        ValueAnimator anim = ValueAnimator.ofObject(new PointEvaluator(), startPoint, endPoint);  
        anim.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {  
            @Override  
            public void onAnimationUpdate(ValueAnimator animation) {  
                currentPoint = (Point) animation.getAnimatedValue();  
                invalidate();  
            }  
        });  
        anim.setDuration(5000);  
        anim.start();  
    }  
  
}  
```
startAnimation()方法中的代码：
就是对Point对象进行了一个动画操作而已。
这里我们定义了一个startPoint和一个endPoint，坐标分别是View的左上角和右下角，并将动画的时长设为5秒。然后有一点需要大家注意的，就是我们通过监听器对动画的过程进行了监听，每当Point值有改变的时候都会回调onAnimationUpdate()方法。在这个方法当中，我们对currentPoint对象进行了重新赋值，并调用了invalidate()方法，这样的话onDraw()方法就会重新调用，并且由于currentPoint对象的坐标已经改变了，那么绘制的位置也会改变，于是一个平移的动画效果也就实现了。

##### 在布局文件当中引入这个自定义控件：
```
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent"  
    >  
  
    <com.example.tony.myapplication.MyAnimView  
        android:layout_width="match_parent"  
        android:layout_height="match_parent" />  
  
</RelativeLayout>  
```





####  
### ObjectAnimator的高级用法

#### 实现目标
动态改变View的颜色。
效果图：![效果图](http://img.blog.csdn.net/20171221162602094?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

#### 实现步骤
##### 自定义MyAnimView属性
　　ObjectAnimator内部的工作机制是通过寻找特定属性的get和set方法，然后通过方法不断地对值进行改变，从而实现动画效果的。
　　因此我们就需要在MyAnimView中**定义一个color属性，并提供它的get和set方法**。这里我们可以将color属性设置为字符串类型，使用#RRGGBB这种格式来表示颜色值，代码如下所示：
　　在setColor()方法当中，将画笔的颜色设置成方法参数传入的颜色，然后调用了invalidate()方法。即在改变了画笔颜色之后立即刷新视图，然后onDraw()方法就会调用。在onDraw()方法当中会根据当前画笔的颜色来进行绘制，这样颜色也就会动态进行改变了。
```
public class MyAnimView extends View {  
  
    ...  
  
    private String color;  
  
    public String getColor() {  
        return color;  
    }  
  
    public void setColor(String color) {  
        this.color = color;  
        mPaint.setColor(Color.parseColor(color));  
        invalidate();  
    }  
  
    ...  
  
}  
```

##### 自定义TypeEvaluator：
　　要借助ObjectAnimator类让setColor()方法得到调用了，在使用ObjectAnimator之前我们还要完成一个非常重要的工作，就是编写一个用于**告知系统如何进行颜色过度的TypeEvaluator**。
创建ColorEvaluator并实现TypeEvaluator接口，代码如下所示：
　　首先在evaluate()方法当中获取到颜色的初始值和结束值，并通过字符串截取的方式将颜色分为RGB三个部分，并将RGB的值转换成十进制数字，那么每个颜色的取值范围就是0-255。接下来计算一下初始颜色值到结束颜色值之间的差值，这个差值很重要，决定着颜色变化的快慢，如果初始颜色值和结束颜色值很相近，那么颜色变化就会比较缓慢，而如果颜色值相差很大，比如说从黑到白，那么就要经历255*3这个幅度的颜色过度，变化就会非常快。

　　那么控制颜色变化的速度是通过getCurrentColor()这个方法来实现的，这个方法会根据当前的fraction值来计算目前应该过度到什么颜色，并且这里会根据初始和结束的颜色差值来控制变化速度，最终将计算出的颜色进行返回。

　　最后，由于我们计算出的颜色是十进制数字，这里还需要调用一下getHexString()方法把它们转换成十六进制字符串，再将RGB颜色拼装起来之后作为最终的结果返回。

```
public class ColorEvaluator implements TypeEvaluator {  
  
    private int mCurrentRed = -1;  
  
    private int mCurrentGreen = -1;  
  
    private int mCurrentBlue = -1;  
  
    @Override  
    public Object evaluate(float fraction, Object startValue, Object endValue) {  
        String startColor = (String) startValue;  
        String endColor = (String) endValue;  
        int startRed = Integer.parseInt(startColor.substring(1, 3), 16);  
        int startGreen = Integer.parseInt(startColor.substring(3, 5), 16);  
        int startBlue = Integer.parseInt(startColor.substring(5, 7), 16);  
        int endRed = Integer.parseInt(endColor.substring(1, 3), 16);  
        int endGreen = Integer.parseInt(endColor.substring(3, 5), 16);  
        int endBlue = Integer.parseInt(endColor.substring(5, 7), 16);  
        // 初始化颜色的值  
        if (mCurrentRed == -1) {  
            mCurrentRed = startRed;  
        }  
        if (mCurrentGreen == -1) {  
            mCurrentGreen = startGreen;  
        }  
        if (mCurrentBlue == -1) {  
            mCurrentBlue = startBlue;  
        }  
        // 计算初始颜色和结束颜色之间的差值  
        int redDiff = Math.abs(startRed - endRed);  
        int greenDiff = Math.abs(startGreen - endGreen);  
        int blueDiff = Math.abs(startBlue - endBlue);  
        int colorDiff = redDiff + greenDiff + blueDiff;  
        if (mCurrentRed != endRed) {  
            mCurrentRed = getCurrentColor(startRed, endRed, colorDiff, 0,  
                    fraction);  
        } else if (mCurrentGreen != endGreen) {  
            mCurrentGreen = getCurrentColor(startGreen, endGreen, colorDiff,  
                    redDiff, fraction);  
        } else if (mCurrentBlue != endBlue) {  
            mCurrentBlue = getCurrentColor(startBlue, endBlue, colorDiff,  
                    redDiff + greenDiff, fraction);  
        }  
        // 将计算出的当前颜色的值组装返回  
        String currentColor = "#" + getHexString(mCurrentRed)  
                + getHexString(mCurrentGreen) + getHexString(mCurrentBlue);  
        return currentColor;  
    }  
  
    /** 
     * 根据fraction值来计算当前的颜色。 
     */  
    private int getCurrentColor(int startColor, int endColor, int colorDiff,  
            int offset, float fraction) {  
        int currentColor;  
        if (startColor > endColor) {  
            currentColor = (int) (startColor - (fraction * colorDiff - offset));  
            if (currentColor < endColor) {  
                currentColor = endColor;  
            }  
        } else {  
            currentColor = (int) (startColor + (fraction * colorDiff - offset));  
            if (currentColor > endColor) {  
                currentColor = endColor;  
            }  
        }  
        return currentColor;  
    }  
      
    /** 
     * 将10进制颜色值转换成16进制。 
     */  
    private String getHexString(int value) {  
        String hexString = Integer.toHexString(value);  
        if (hexString.length() == 1) {  
            hexString = "0" + hexString;  
        }  
        return hexString;  
    }  
  
}  
```

##### 调用
比如说我们想要实现从蓝色到红色的动画过度，历时5秒，就可以这样写：
```
ObjectAnimator anim = ObjectAnimator.ofObject(myAnimView, "color", new ColorEvaluator(),   
    "#0000FF", "#FF0000");  
anim.setDuration(5000);  
anim.start();  
```

接下来我们需要将上面一段代码移到MyAnimView类当中，让它和刚才的Point移动动画可以结合到一起播放，这就要借助我们在上篇文章当中学到的**组合动画**的技术了。修改MyAnimView中的代码，如下所示：
```
public class MyAnimView extends View {  
  
    ...  
  
    private void startAnimation() {  
        Point startPoint = new Point(RADIUS, RADIUS);  
        Point endPoint = new Point(getWidth() - RADIUS, getHeight() - RADIUS);  
        ValueAnimator anim = ValueAnimator.ofObject(new PointEvaluator(), startPoint, endPoint);  
        anim.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {  
            @Override  
            public void onAnimationUpdate(ValueAnimator animation) {  
                currentPoint = (Point) animation.getAnimatedValue();  
                invalidate();  
            }  
        });  
        ObjectAnimator anim2 = ObjectAnimator.ofObject(this, "color", new ColorEvaluator(),   
                "#0000FF", "#FF0000");  
        AnimatorSet animSet = new AnimatorSet();  
        animSet.play(anim).with(anim2);  
        animSet.setDuration(5000);  
        animSet.start();  
    }  
  
}  
```

可以看到，我们并没有改动太多的代码，重点只是修改了startAnimation()方法中的部分内容。这里先是将颜色过度的代码逻辑移动到了startAnimation()方法当中，注意由于这段代码本身就是在MyAnimView当中执行的，因此ObjectAnimator.ofObject()的第一个参数直接传this就可以了。接着我们又创建了一个AnimatorSet，并把两个动画设置成同时播放，动画时长为五秒，最后启动动画。

####  
### Interpolator的用法
Interpolator这个东西很难进行翻译，直译过来的话是补间器的意思，它的主要作用是可以**控制动画的变化速率**，比如去实现一种非线性运动的动画效果。那么什么叫做非线性运动的动画效果呢？就是说动画改变的速率不是一成不变的，像加速运动以及减速运动都属于非线性运动。

不过Interpolator并不是属性动画中新增的技术，实际上从Android 1.0版本开始就一直存在Interpolator接口了，而之前的补间动画当然也是支持这个功能的。只不过在**属性动画**中**新增**了一个**TimeInterpolator**接口，这个接口是用于兼容之前的Interpolator的，这使得所有过去的Interpolator实现类都可以直接拿过来放到属性动画当中使用。

TimeInterpolator接口的所有实现类：
![](http://img.blog.csdn.net/20171221225916611?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


TimeInterpolator接口已经有非常多的实现类了，这些都是Android系统内置好的并且我们可以直接使用的Interpolator。每个Interpolator都有它各自的实现效果，比如说AccelerateInterpolator就是一个加速运动的Interpolator，而DecelerateInterpolator就是一个减速运动的Interpolator。

#### AccelerateDecelerateInterpolator
上文使用ValueAnimator所打印的值如下所示：
![](http://img.blog.csdn.net/20171221230833425?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

可以看到，一开始的值变化速度明显比较慢，仅0.0开头的就打印了4次，之后开始加速，最后阶段又开始减速，因此我们可以很明显地看出这一个先加速后减速的Interpolator。

上文中的小球也是：一开始运动速度比较慢，然后逐渐加速，中间的部分运动速度就比较快，接下来开始减速，最后缓缓停住。另外颜色变化也是这种规律，一开始颜色变化的比较慢，中间颜色变化的很快，最后阶段颜色变化的又比较慢。

从以上几点我们就可以总结出一个结论了，**使用属性动画时，系统默认的Interpolator其实就是一个先加速后减速的Interpolator，对应的实现类就是AccelerateDecelerateInterpolator**。

我们可以很轻松地修改这一默认属性，将它替换成任意一个系统内置好的Interpolator。
比如，上文MyAnimView中的startAnimation()方法是开启动画效果的入口，这里我们对Point对象的坐标稍做一下修改，让它变成一种垂直掉落的效果，代码如下所示：
对Point构造函数中的坐标值进行了一下改动，那么现在小球运动的动画效果应该是从屏幕正中央的顶部掉落到底部。
```
private void startAnimation() {  
    Point startPoint = new Point(getWidth() / 2, RADIUS);  
    Point endPoint = new Point(getWidth() / 2, getHeight() - RADIUS);  
    ValueAnimator anim = ValueAnimator.ofObject(new PointEvaluator(), startPoint, endPoint);  
    anim.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {  
        @Override  
        public void onAnimationUpdate(ValueAnimator animation) {  
            currentPoint = (Point) animation.getAnimatedValue();  
            invalidate();  
        }  
    });  
    anim.setDuration(2000);  
    anim.start();  
}  
```
但是默认情况下小球的下降速度肯定是先加速后减速的，我们需要改为下降速度越来越快的。
调用**Animator的setInterpolator()**方法就可以了，这个方法要求**传入**一个实现**TimeInterpolator**接口的实例，那么比如说我们想要实现小球下降越来越快的效果，就可以使用AccelerateInterpolator，代码如下所示：
```
private void startAnimation() {  
    Point startPoint = new Point(getWidth() / 2, RADIUS);  
    Point endPoint = new Point(getWidth() / 2, getHeight() - RADIUS);  
    ValueAnimator anim = ValueAnimator.ofObject(new PointEvaluator(), startPoint, endPoint);  
    anim.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {  
        @Override  
        public void onAnimationUpdate(ValueAnimator animation) {  
            currentPoint = (Point) animation.getAnimatedValue();  
            invalidate();  
        }  
    });  
    anim.setInterpolator(new AccelerateInterpolator(2f));//实现小球下降越来越快  
    /*AccelerateInterpolator的构建函数可以接收一个float类型的参数，这个参数是用于控制加速度的*/
    anim.setDuration(2500);  
    anim.start();  
}  
```
效果如下：
![](http://img.blog.csdn.net/20171221231615443?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

#### BounceInterpolator
现在要让小球撞击到地面之后应该要反弹起来，然后再次落下，接着再反弹起来，又再次落下，以此反复，最后静止。这个功能我们当然可以自己去写，只不过比较复杂，所幸的是，Android系统中已经提供好了这样一种Interpolator，我们只需要简单地替换一下就可以完成上面的描述的效果，代码如下所示：
将设置的Interpolator换成了BounceInterpolator的实例，而BounceInterpolator就是一种可以**模拟物理规律，实现反复弹起效果**的Interpolator。
```java
private void startAnimation() {  
    Point startPoint = new Point(getWidth() / 2, RADIUS);  
    Point endPoint = new Point(getWidth() / 2, getHeight() - RADIUS);  
    ValueAnimator anim = ValueAnimator.ofObject(new PointEvaluator(), startPoint, endPoint);  
    anim.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {  
        @Override  
        public void onAnimationUpdate(ValueAnimator animation) {  
            currentPoint = (Point) animation.getAnimatedValue();  
            invalidate();  
        }  
    });  
    anim.setInterpolator(new BounceInterpolator());  
    anim.setDuration(3000);  
    anim.start();  
}  
```
效果如下：
![](http://img.blog.csdn.net/20171221231840395?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

#### Interpolator内部实现机制
首先看一下TimeInterpolator的接口定义，代码如下所示：
```java
/** 
 * A time interpolator defines the rate of change of an animation. This allows animations 
 * to have non-linear motion, such as acceleration and deceleration. 
 */  
public interface TimeInterpolator {  
  
    /** 
     * Maps a value representing the elapsed fraction of an animation to a value that represents 
     * the interpolated fraction. This interpolated value is then multiplied by the change in 
     * value of an animation to derive the animated value at the current elapsed animation time. 
     * 
     * @param input A value between 0 and 1.0 indicating our current point 
     *        in the animation where 0 represents the start and 1.0 represents 
     *        the end 
     * @return The interpolation value. This value can be more than 1.0 for 
     *         interpolators which overshoot their targets, or less than 0 for 
     *         interpolators that undershoot their targets. 
     */  
    float getInterpolation(float input);  
}  
```
只有一个getInterpolation()方法。
getInterpolation()方法中接收一个input参数，这个参数的值会随着动画的运行而不断变化，不过它的变化是非常有规律的，就是根据设定的动画时长**匀速增加**，变化范围是0到1。也就是说当动画一开始的时候input的值是0，到动画结束的时候input的值是1，而中间的值则是随着动画运行的时长在0到1之间变化的。

input的值决定了上文中fraction的值。
**input**的值是由**系统**经过**计算**后**传入**到**getInterpolation()**方法中的，然后我们可以自己实现getInterpolation()方法中的算法，根据input的值来计算出一个返回值，而这个**返回值就是fraction**了。
因此，最简单的情况就是input值和fraction值是相同的，这种情况由于input值是匀速增加的，因而fraction的值也是匀速增加的，所以动画的运动情况也是匀速的。

系统中内置的**LinearInterpolator**就是一种匀速运动的Interpolator，那么我们来看一下它的源码是怎么实现的：
```java
/** 
 * An interpolator where the rate of change is constant 
 */  
@HasNativeInterpolator  
public class LinearInterpolator extends BaseInterpolator implements NativeInterpolatorFactory {  
  
    public LinearInterpolator() {  
    }  
  
    public LinearInterpolator(Context context, AttributeSet attrs) {  
    }  
  
    public float getInterpolation(float input) {  
        return input;  //把参数中传递的input值直接返回了，因此fraction的值就是等于input的值的，这就是匀速运动的Interpolator的实现方式。
    }  
  
    /** @hide */  
    @Override  
    public long createNativeInterpolator() {  
        return NativeInterpolatorFactoryHelper.createLinearInterpolator();  
    }  
}  
```

当然这是最简单的一种Interpolator的实现了，我们再来看一个稍微复杂一点的。既然现在大家都知道了系统在默认情况下使用的是**AccelerateDecelerateInterpolator**，那我们就来看一下它的源码吧，如下所示：
```java
/** 
 * An interpolator where the rate of change starts and ends slowly but 
 * accelerates through the middle. 
 *  
 */  
@HasNativeInterpolator  
public class AccelerateDecelerateInterpolator implements Interpolator, NativeInterpolatorFactory {  
    public AccelerateDecelerateInterpolator() {  
    }  
      
    @SuppressWarnings({"UnusedDeclaration"})  
    public AccelerateDecelerateInterpolator(Context context, AttributeSet attrs) {  
    }  
      
    public float getInterpolation(float input) {  
        return (float)(Math.cos((input + 1) * Math.PI) / 2.0f) + 0.5f;  
        /*算法中主要使用了余弦函数，由于input的取值范围是0到1，那么cos函数中的取值范围就是π到2π。而cos(π)的结果是-1，cos(2π)的结果是1，那么这个值再除以2加上0.5之后，getInterpolation()方法最终返回的结果值还是在0到1之间。只不过经过了余弦运算之后，最终的结果不再是匀速增加的了，而是经历了一个先加速后减速的过程。*/
    }  
  
    /** @hide */  
    @Override  
    public long createNativeInterpolator() {  
        return NativeInterpolatorFactoryHelper.createAccelerateDecelerateInterpolator();  
    }  
}  
```
getInterpolation()方法中的逻辑变复杂了，我们可以将这个算法的执行情况通过曲线图的方式绘制出来，结果如下图所示：
![](http://img.blog.csdn.net/20171221232939801?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


可以看到，这是一个S型的曲线图，当横坐标从0变化到0.2的时候，纵坐标的变化幅度很小，但是之后就开始明显加速，最后横坐标从0.8变化到1的时候，纵坐标的变化幅度又变得很小。

#### 自定义Interpolator
实现：先减速后加速Interpolator
新建DecelerateAccelerateInterpolator类，让它实现TimeInterpolator接口，代码如下所示：
```java
public class DecelerateAccelerateInterpolator implements TimeInterpolator{  
    @Override  
    public float getInterpolation(float input) {  
        float result;  
        if (input <= 0.5) {  
            result = (float) (Math.sin(Math.PI * input)) / 2;  
        } else {  
            result = (float) (2 - Math.sin(Math.PI * input)) / 2;  
        }  
        return result;  
    }  
}  
```
这段代码是使用正弦函数来实现先减速后加速的功能的，因为正弦函数初始弧度的变化值非常大，刚好和余弦函数是相反的，而随着弧度的增加，正弦函数的变化值也会逐渐变小，这样也就实现了减速的效果。当弧度大于π/2之后，整个过程相反了过来，现在正弦函数的弧度变化值非常小，渐渐随着弧度继续增加，变化值越来越大，弧度到π时结束，这样从0过度到π，也就实现了先减速后加速的效果。

同样我们可以将这个算法的执行情况通过曲线图的方式绘制出来，结果如下图所示：
![](http://img.blog.csdn.net/20171221233812422?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


可以看到，这也是一个S型的曲线图，只不过曲线的方向和刚才是相反的。从上图中我们可以很清楚地看出来，一开始纵坐标的变化幅度很大，然后逐渐变小，横坐标到0.5的时候纵坐标变化幅度趋近于零，之后随着横坐标继续增加纵坐标的变化幅度又开始变大，的确是先减速后加速的效果。

那么现在我们将DecelerateAccelerateInterpolator在代码中进行替换，如下所示：

```java
private void startAnimation() {  
    Point startPoint = new Point(getWidth() / 2, RADIUS);  
    Point endPoint = new Point(getWidth() / 2, getHeight() - RADIUS);  
    ValueAnimator anim = ValueAnimator.ofObject(new PointEvaluator(), startPoint, endPoint);  
    anim.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {  
        @Override  
        public void onAnimationUpdate(ValueAnimator animation) {  
            currentPoint = (Point) animation.getAnimatedValue();  
            invalidate();  
        }  
    });  
    anim.setInterpolator(new DecelerateAccelerateInterpolator());//替换成自定义Interpolator
    anim.setDuration(3000);  
    anim.start();  
}  
```

非常简单，就是将DecelerateAccelerateInterpolator的实例传入到setInterpolator()方法当中。重新运行一下代码，效果如下图所示：
![](http://img.blog.csdn.net/20171221233909649?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


### ViewPropertyAnimator

它并不是在3.0系统当中引入的，而是在3.1系统当中附增的一个新的功能。为View的动画操作提供一种更加便捷的用法。

**属性动画**的机制已经不是再针对于View而进行设计的了，而是一种不断地对值进行操作的机制，它可以**将值赋值到指定对象的指定属性上**。但是，在绝大多数情况下，我相信大家主要都还是对View进行动画操作的。

那我们先来回顾一下之前的用法吧，比如我们想要让一个TextView从常规状态变成透明状态，就可以这样写：
```java
ObjectAnimator animator = ObjectAnimator.ofFloat(textview, "alpha", 0f);  
animator.start();  
```
#### 用法
使用ViewPropertyAnimator来实现同样的效果，ViewPropertyAnimator提供了更加易懂、更加面向对象的API，如下所示：
```
textview.animate().alpha(0f);  //将当前的textview变成透明状态
```
**animate()**方法就是在Android **3.1系统上新增**的一个方法，这个方法的**返回**值是一个**ViewPropertyAnimator对象**，也就是说拿到这个对象之后我们就可以调用它的各种方法来实现动画效果了，这里我们调用了alpha()方法并转入0，表示将当前的textview变成透明状态。

ViewPropertyAnimator还可以很轻松地将多个动画组合到一起，比如我们想要让textview运动到500,500这个坐标点上，就可以这样写：
```
textview.animate().x(500).y(500);  //让textview运动到500,500这个坐标点
```

ViewPropertyAnimator是支持连缀用法的，我们想让textview移动到横坐标500这个位置上时调用了x(500)这个方法，然后让textview移动到纵坐标500这个位置上时调用了y(500)这个方法，将所有想要组合的动画通过这种连缀的方式拼接起来，这样全部动画就都会一起被执行。

设定动画的运行时长：
```
textview.animate().x(500).y(500).setDuration(5000);  //设定动画的运行时长
```

Interpolator也可以应用在ViewPropertyAnimator上面：

```
textview.animate().x(500).y(500).setDuration(5000)  
        .setInterpolator(new BounceInterpolator());  //使用Interpolator
```

ViewPropertyAnimator用法基本大同小异，需要用到什么功能就连缀一下，因此更多的用法大家只需要去查阅一下文档，看看还支持哪些功能，有哪些接口可以调用就可以了。

#### 注意
　　整个ViewPropertyAnimator的功能都是建立在View类新增的animate()方法之上的，这个方法会创建并返回一个ViewPropertyAnimator的实例，之后的调用的所有方法，设置的所有属性都是通过这个实例完成的。
　　在使用ViewPropertyAnimator时，我们自始至终没有调用过start()方法，这是因为新的接口中使用了隐式启动动画的功能，只要我们**将动画定义完成之后，动画就会自动启动**。并且这个机制对于组合动画也同样有效，只要我们不断地连缀新的方法，那么动画就不会立刻执行，**等到所有在ViewPropertyAnimator上设置的方法都执行完毕后，动画就会自动启动**。当然如果不想使用这一默认机制的话，我们**也可以显式地调用start()方法**来启动动画。
　　ViewPropertyAnimator的**所有接口都是使用连缀的语法来设计的**，每个方法的返回值都是它自身的实例，因此调用完一个方法之后可以直接连缀调用它的另一个方法，这样把所有的功能都串接起来，我们甚至可以仅通过一行代码就完成任意复杂度的动画功能。

# 动画常见问题
## 修改 Activity 进入和退出动画
可以通过两种方式，一是通过定义 Activity 的主题，二是通过覆写 Activity 的 overridePendingTransition 方法。
方式1：通过**设置主题样式**
在 styles.xml 中编辑如下代码：
```
<style name="AnimationActivity" parent="@android:style/Animation.Activity">
<item name="android:activityOpenEnterAnimation">@anim/slide_in_left</item>
<item name="android:activityOpenExitAnimation">@anim/slide_out_left</item>
<item name="android:activityCloseEnterAnimation">@anim/slide_in_right</item>
<item name="android:activityCloseExitAnimation">@anim/slide_out_right</item>
</style>
```

添加 themes.xml 文件：
```
<style name="ThemeActivity">
<item name="android:windowAnimationStyle">@style/AnimationActivity</item>
<item name="android:windowNoTitle">true</item>
</style>
```

在 AndroidManifest.xml 中给指定的 Activity 指定 theme。

方式2：**覆写 overridePendingTransition** 方法
overridePendingTransition(R.anim.fade, R.anim.hold);

## 属性动画，例如一个 button 从 A 移动到 B 点，B 点还是可以响应点击事件，这个原理是什么？
补间动画只是显示的位置变动，View 的实际位置未改变，表现为 View 移动到其他地方，点击事件仍在原处才能响应。而属性动画控件移动后事件相应就在控件移动后本身进行处理。



引用：
[Android属性动画完全解析(上)，初识属性动画的基本用法](http://blog.csdn.net/guolin_blog/article/details/43536355)
[Android属性动画完全解析(中)，ValueAnimator和ObjectAnimator的高级用法](http://blog.csdn.net/guolin_blog/article/details/43816093)
[Android属性动画完全解析(下)，Interpolator和ViewPropertyAnimator的用法](http://blog.csdn.net/guolin_blog/article/details/44171115)
