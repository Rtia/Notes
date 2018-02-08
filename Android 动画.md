# 【Android 动画】



# 动画分类

Android 中动画分为两种，一种是 Tween 动画、还有一种是 Frame 动画。

## 补间动画(Tween动画)

这种实现方式可以使视图组件**移动、放大、缩小以及产生透明度**的变化；

## 帧动画(Frame 动画)

传统的动画方法，通过顺序的**播放排列好的图片**来实现，类似电影、gif。

## 属性动画(Property animation)

自Android 3.0版本开始，系统给我们提供了一种全新的动画模式，属性动画(property animation)，它的功能非常强大，弥补了之前补间动画的一些缺陷，几乎是可以完全替代掉补间动画了。

# 补间动画(Tween动画)

## 1、透明度动画AlphaAnimation

```java
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

## 2、平移动画TranslateAnimation

```java
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

## 3、缩放动画ScaleAnimation

```java
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

## 4、旋转动画 rotate

### 创建一个Animation类型的XML文件：

```xml
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

### java中导入xml动画

```java
Animation animation1 = AnimationUtils.loadAnimation(this,R.anim.rotate);
imageView.startAnimation(animation1);
```

## 复合动画

```java
AnimationSet animationSet=new AnimationSet(false);
animationSet.addAnimation(animation1);
//这样在这里面添加就可以了；		
Animation rotateAnimation = AnimationUtils.loadAnimation(this, R.anim.rotate);
animationSet.addAnimation(rotateAnimation);
```

# 帧动画(Frame 动画)

## 方式一；使用XML的方式；

1、创建一个AnimationDrawable 的ＸＭＬ文件；

```xml
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

```java
drawable = (AnimationDrawable) imageView.getDrawable();

if (drawable.isRunning()) {
			drawable.stop();
		}else{
			drawable.start();
		}
```

## 方式二：使用代码的方式进行；

方式1：添加多个帧 Drawable

```java
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

```java
// 引用xml 帧动画drawable
mAnimationDrawable=(AnimationDrawable) getResources().getDrawable(R.drawable.frame);
mImageView.setImageDrawable(mAnimationDrawable);
```

# 属性动画(Property animation)

## 为什么要引入属性动画？

Android之前的补间动画机制其实还算是比较健全的，在android.view.animation包下面有好多的类可以供我们操作，来完成一系列的动画效果，比如说对View进行移动、缩放、旋转和淡入淡出，并且我们还可以借助AnimationSet来将这些动画效果组合起来使用，除此之外还可以通过配置Interpolator来控制动画的播放速度等等等等。那么这里大家可能要产生疑问了，既然之前的动画机制已经这么健全了，为什么还要引入属性动画呢？

其实上面所谓的健全都是相对的，如果你的需求中只需要对View进行移动、缩放、旋转和淡入淡出操作，那么补间动画确实已经足够健全了。但是很显然，这些功能是不足以覆盖所有的场景的，一旦我们的需求超出了移动、缩放、旋转和淡入淡出这四种对View的操作，那么补间动画就不能再帮我们忙了，也就是说它在功能和可扩展方面都有相当大的局限性，那么下面我们就来看看补间动画所不能胜任的场景。

注意上面我在介绍补间动画的时候都有使用“对View进行操作”这样的描述，没错，**补间动画是只能够作用在View上**的。也就是说，我们可以对一个Button、TextView、甚至是LinearLayout、或者其它任何继承自View的组件进行动画操作，但是如果我们想要对一个非View的对象进行动画操作，抱歉，补间动画就帮不上忙了。可能有的朋友会感到不能理解，我怎么会需要对一个非View的对象进行动画操作呢？这里我举一个简单的例子，比如说我们有一个自定义的View，在这个View当中有一个Point对象用于管理坐标，然后在onDraw()方法当中就是根据这个Point对象的坐标值来进行绘制的。也就是说，如果我们可以对Point对象进行动画操作，那么整个自定义View的动画效果就有了。显然，补间动画是不具备这个功能的，这是它的第一个缺陷。

然后**补间动画**还有一个缺陷，就是它**只能够实现移动、缩放、旋转和淡入淡出这四种动画操作**，那如果我们希望可以对View的背景色进行动态地改变呢？很遗憾，我们只能靠自己去实现了。说白了，之前的补间动画机制就是使用硬编码的方式来完成的，功能限定死就是这些，基本上没有任何扩展性可言。

最后，**补间动画**还有一个致命的缺陷，就是它**只是改变了View的显示效果而已，而不会真正去改变View的属性**。什么意思呢？比如说，现在屏幕的左上角有一个按钮，然后我们通过补间动画将它移动到了屏幕的右下角，现在你可以去尝试点击一下这个按钮，点击事件是绝对不会触发的，因为实际上这个按钮还是停留在屏幕的左上角，只不过补间动画将这个按钮绘制到了屏幕的右下角而已。

也正是因为这些原因，Android开发团队决定在3.0版本当中引入属性动画这个功能，那么属性动画是不是就把上述的问题全部解决掉了？下面我们就来一起看一看。

新引入的属性动画机制已经不再是针对于View来设计的了，也不限定于只能实现移动、缩放、旋转和淡入淡出这几种动画操作，同时也不再只是一种视觉上的动画效果了。它实际上是一种不断地对值进行操作的机制，并将值赋值到指定对象的指定属性上，可以是任意对象的任意属性。所以我们仍然可以将一个View进行移动或者缩放，但同时也可以对自定义View中的Point对象进行动画操作了。我们只需要告诉系统动画的运行时长，需要执行哪种类型的动画，以及动画的初始值和结束值，剩下的工作就可以全部交给系统去完成了。

既然属性动画的实现机制是通过对目标对象进行赋值并修改其属性来实现的，那么之前所说的按钮显示的问题也就不复存在了，如果我们通过属性动画来移动一个按钮，那么这个按钮就是真正的移动了，而不再是仅仅在另外一个位置绘制了而已。

## ValueAnimator

ValueAnimator是整个属性动画机制当中最核心的一个类，前面我们已经提到了，**属性动画的运行机制是通过不断地对值进行操作来实现的**，而初始值和结束值之间的**动画过渡**就是**由ValueAnimator**这个类来负责**计算**的。
它的内部使用一种时间循环的机制来计算值与值之间的动画过渡，我们只需要将初始值和结束值提供给ValueAnimator，并且告诉它动画所需运行的**时长**，那么ValueAnimator就会自动帮我们完成从初始值平滑地过渡到结束值这样的效果。除此之外，ValueAnimator还负责管理动画的播放**次数**、播放**模式**、以及**对动画设置监听器**等，确实是一个非常重要的类。
详细见下文【属性动画之ObjectAnimator】


## ObjectAnimator

**对任意对象的任意属性进行动画操作**

相比于ValueAnimator，ObjectAnimator可能才是我们最常接触到的类，因为ValueAnimator只不过是对值进行了一个平滑的动画过渡，但我们实际使用到这种功能的场景好像并不多。而ObjectAnimator则就不同了，它是可以直接任意对象的任意属性进行动画操作的，比如说View的alpha属性。

不过虽说ObjectAnimator会更加常用一些，但是它其实是**继承自ValueAnimator**的，底层的动画实现机制也是基于ValueAnimator来完成的，因此ValueAnimator仍然是整个属性动画当中最核心的一个类。
详细见下文【属性动画之ObjectAnimator】


###  

## 组合动画-AnimatorSet

实现组合动画功能主要需要借助**AnimatorSet**这个类，这个类提供了一个**play()**方法，如果我们向这个方法中传入一个Animator对象(ValueAnimator或ObjectAnimator)将会返回一个AnimatorSet.Builder的实例。

### AnimatorSet.Builder

AnimatorSet.Builder中包括以下四个方法：
**after(Animator anim)**   将现有动画插入到传入的动画之后执行
**after(long delay)**   将现有动画延迟指定毫秒后执行
**before(Animator anim)**   将现有动画插入到传入的动画之前执行
**with(Animator anim)**   将现有动画和传入的动画同时执行

好的，有了这四个方法，我们就可以完成组合动画的逻辑了，那么比如说我们想要让TextView先从屏幕外移动进屏幕，然后开始旋转360度，旋转的同时进行淡入淡出操作，就可以这样写：

```java
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

## Animator监听器

### AnimatorListener

Animator类当中提供了一个**addListener()**方法，这个方法接收一个AnimatorListener，我们只需要去实现这个**AnimatorListener**就可以监听动画的各种事件了。

大家已经知道，ObjectAnimator是继承自ValueAnimator的，而ValueAnimator又是继承自Animator的，因此不管是ValueAnimator还是ObjectAnimator都是可以使用addListener()这个方法的。另外AnimatorSet也是继承自Animator的，因此addListener()这个方法算是个通用的方法。

添加一个监听器的代码如下所示：

```java
Anim.addListener(new AnimatorListener() {  
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

### AnimatorListenerAdapter

但是也许很多时候我们并不想要监听那么多个事件，可能我只想要监听动画结束这一个事件，那么每次都要将四个接口全部实现一遍就显得非常繁琐。没关系，为此Android提供了一个适配器类，叫作AnimatorListenerAdapter，使用这个类就可以**解决掉实现接口繁琐的问题**了，如下所示：

```java
Anim.addListener(new AnimatorListenerAdapter() {  
});  
```

这里我们向addListener()方法中传入这个适配器对象，由于**AnimatorListenerAdapter中已经将每个接口都实现好了**，所以这里不用实现任何一个方法也不会报错。那么如果我想监听动画结束这个事件，就只需要**单独重写**这一个**方法**就可以了，如下所示：

```java
Anim.addListener(new AnimatorListenerAdapter() {  
    @Override  
    public void onAnimationEnd(Animator animation) {  
    }  
});  
```

## 使用XML编写动画

我们可以使用代码来编写所有的动画功能，这也是最常用的一种做法。不过，过去的补间动画除了使用代码编写之外也是可以使用XML编写的，因此属性动画也提供了这一功能，即通过XML来完成和代码一样的属性动画功能。

通过XML来编写动画可能会比通过代码来编写动画要慢一些，但是在**重用**方面将会变得非常**轻松**，比如某个将通用的动画编写到XML里面，我们就可以在各个界面当中轻松去重用它。

如果想要使用XML来编写动画，首先要在res目录下面新建一个animator文件夹，所有属性动画的XML文件都应该存放在这个文件夹当中。

### XML标签

#### animato

对应代码中的ValueAnimator

#### objectAnimator

对应代码中的ObjectAnimator

#### set

对应代码中的AnimatorSet

那么比如说我们想要实现一个从0到100平滑过渡的动画，在XML当中就可以这样写：

```xml
<animator xmlns:android="http://schemas.android.com/apk/res/android"  
    android:valueFrom="0"  
    android:valueTo="100"  
    android:valueType="intType"/>  
```

而如果我们想将一个视图的alpha属性从1变成0，就可以这样写：

```xml
<objectAnimator xmlns:android="http://schemas.android.com/apk/res/android"  
    android:valueFrom="1"  
    android:valueTo="0"  
    android:valueType="floatType"  
    android:propertyName="alpha"/>  
```

另外，我们也可以使用XML来完成复杂的组合动画操作，比如将一个视图先从屏幕外移动进屏幕，然后开始旋转360度，旋转的同时进行淡入淡出操作，就可以这样写：

```xml
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

### 在代码中加载XML文件-AnimatorInflater

在代码中把文件加载进来并将动画启动：

```java
Animator animator = AnimatorInflater.loadAnimator(context, R.animator.anim_file);  
animator.setTarget(view);  
animator.start();  
```

调用AnimatorInflater的loadAnimator来将XML动画文件加载进来，然后再调用setTarget()方法将这个动画设置到某一个对象上面，最后再调用start()方法启动动画就可以了。



###  

## ViewPropertyAnimator

它并不是在3.0系统当中引入的，而是在3.1系统当中附增的一个新的功能。为View的动画操作提供一种更加便捷的用法。

**属性动画**的机制已经不是再针对于View而进行设计的了，而是一种不断地对值进行操作的机制，它可以**将值赋值到指定对象的指定属性上**。但是，在绝大多数情况下，我相信大家主要都还是对View进行动画操作的。

那我们先来回顾一下之前的用法吧，比如我们想要让一个TextView从常规状态变成透明状态，就可以这样写：

```java
ObjectAnimator animator = ObjectAnimator.ofFloat(textview, "alpha", 0f);  
animator.start();  
```

### 用法

使用ViewPropertyAnimator来实现同样的效果，ViewPropertyAnimator提供了更加易懂、更加面向对象的API，如下所示：

```java
textview.animate().alpha(0f);  //将当前的textview变成透明状态
```

**animate()**方法就是在Android **3.1系统上新增**的一个方法，这个方法的**返回**值是一个**ViewPropertyAnimator对象**，也就是说拿到这个对象之后我们就可以调用它的各种方法来实现动画效果了，这里我们调用了alpha()方法并转入0，表示将当前的textview变成透明状态。

ViewPropertyAnimator还可以很轻松地将多个动画组合到一起，比如我们想要让textview运动到500,500这个坐标点上，就可以这样写：

```java
textview.animate().x(500).y(500);  //让textview运动到500,500这个坐标点
```

ViewPropertyAnimator是支持连缀用法的，我们想让textview移动到横坐标500这个位置上时调用了x(500)这个方法，然后让textview移动到纵坐标500这个位置上时调用了y(500)这个方法，将所有想要组合的动画通过这种连缀的方式拼接起来，这样全部动画就都会一起被执行。

设定动画的运行时长：

```java
textview.animate().x(500).y(500).setDuration(5000);  //设定动画的运行时长
```

Interpolator也可以应用在ViewPropertyAnimator上面：

```java
textview.animate().x(500).y(500).setDuration(5000)  
        .setInterpolator(new BounceInterpolator());  //使用Interpolator
```

ViewPropertyAnimator用法基本大同小异，需要用到什么功能就连缀一下，因此更多的用法大家只需要去查阅一下文档，看看还支持哪些功能，有哪些接口可以调用就可以了。

### 注意

　　整个ViewPropertyAnimator的功能都是建立在View类新增的animate()方法之上的，这个方法会创建并返回一个ViewPropertyAnimator的实例，之后的调用的所有方法，设置的所有属性都是通过这个实例完成的。
　　在使用ViewPropertyAnimator时，我们自始至终没有调用过start()方法，这是因为新的接口中使用了隐式启动动画的功能，只要我们**将动画定义完成之后，动画就会自动启动**。并且这个机制对于组合动画也同样有效，只要我们不断地连缀新的方法，那么动画就不会立刻执行，**等到所有在ViewPropertyAnimator上设置的方法都执行完毕后，动画就会自动启动**。当然如果不想使用这一默认机制的话，我们**也可以显式地调用start()方法**来启动动画。
　　ViewPropertyAnimator的**所有接口都是使用连缀的语法来设计的**，每个方法的返回值都是它自身的实例，因此调用完一个方法之后可以直接连缀调用它的另一个方法，这样把所有的功能都串接起来，我们甚至可以仅通过一行代码就完成任意复杂度的动画功能。



# 属性动画之Evaluator

## 1、概述

我们先不讲什么是Evaluator，我们先来看一张图： 
![](http://upload-images.jianshu.io/upload_images/9028834-81845d6b5c67b308.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这幅图讲述了从定义动画的数字区间到通过AnimatorUpdateListener中得到当前动画所对应数值的整个过程。下面我们对这四个步骤具体讲解一下： 
(1)、**ofInt**(0,400)表示指定动画的数字区间，是从0运动到400； 
(2)、插值器**Interpolator**：在动画开始后，通过Interpolator会返回当前动画进度所对应的数字进度，但这个数字进度是百分制的，以小数表示，如0.2 
(3)、**Evaluator**:我们知道我们通过监听器拿到的是当前动画所对应的具体数值，而不是百分制的进度。那么就必须有一个地方会根据当前的数字进度，将其转化为对应的数值，这个地方就是Evaluator；Evaluator就是将从插值器Interpolator返回的数字进度转成对应的数字值。所以上部分中，我们讲到的公式：

```java
当前的值 = 100 + （400 - 100）* 显示进度  
```

这个公式就是在Evaluator计算的；在拿到当前数字进度所对应的值以后，将其返回 
（4）、**监听器**：我们通过在AnimatorUpdateListener监听器使用animation.getAnimatedValue()函数拿到Evaluator中返回的数字值。 
讲了这么多，Evaluator其实就是一个转换器，他能把小数进度转换成对应的数值位置

## 2、各种Evaluator

首先，插值器**Interpolator**返回的小数值，表示的是当前动画的数值进度。无论是利用**ofFloat**()还是利用**ofInt**()定义的动画**都**是**适用**的。因为无论是什么动画，它的进度必然都是在0到1之间的。0表示没开始，1表示数值运动的结束，对于任何动画都是适用的。 

但Evaluator则不一样，我们知道Evaluator是根据插值器Interpolator返回的小数进度转换成当前数值进度所对应的值。这问题就来了，如果我们使用ofInt()来定义动画，动画中的值应该都是Int类型，如果我用ofFloat()来定义动画，那么动画中的值也都是Float类型。所以如果我用ofInt()来定义动画，所对应的Evaluator在返回值时，必然要返回Int类型的值。同样，我们如果用ofFloat来定义动画，那么Evaluator在返回值时也必然返回的是Float类型的值。 
所以**每种定义方式所对应的Evaluator必然是它专用的**；Evaluator专用的原因在于动画数值类型不一样，在通过Evaluator返回时会报强转错误；所以只有在动画数值类型一样时，所对应的Evaluator才能通用。所以**ofInt()对应**的Evaluator类名叫**IntEvaluator**,而**ofFloat()对应**的Evaluator类名叫**FloatEvaluator**； 
在设置Evaluator时，是通过**animator.setEvaluator()来设置**的，比如：

### Evaluator使用

```java
ValueAnimator animator = ValueAnimator.ofInt(0,600);  
  
animator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {  
    @Override  
    public void onAnimationUpdate(ValueAnimator animation) {  
        int curValue = (int)animation.getAnimatedValue();  
        tv.layout(tv.getLeft(),curValue,tv.getRight(),curValue+tv.getHeight());  
    }  
});  
animator.setDuration(1000);  
animator.setEvaluator(new IntEvaluator());  
animator.setInterpolator(new BounceInterpolator());  
animator.start();  
```

但大家会说了，在此之前，我们在使用ofInt()时，从来没有给它定义过使用IntEvaluator来转换值啊，那怎么也能正常运行呢？因为ofInt和ofFloat都是系统直接提供的函数，所以在使用时都会有默认的插值器**Interpolator**和**Evaluator**来使用的，**不指定则使用默认的**；对于Evaluator而言，ofInt()的默认Evaluator当然是IntEvaluator;而FloatEvalutar默认的则是FloatEvalutor; 
上面，我们已经弄清楚Evaluator定义和使用方法，下面我们就来看看IntEvaluator内部是怎么实现的吧：

### IntEvaluator内部实现

```java
/** 
 * This evaluator can be used to perform type interpolation between <code>int</code> values. 
 */  
public class IntEvaluator implements TypeEvaluator<Integer> {  
  
    /** 
     * This function returns the result of linearly interpolating the start and end values, with 
     * <code>fraction</code> representing the proportion between the start and end values. The 
     * calculation is a simple parametric calculation: <code>result = x0 + t * (v1 - v0)</code>, 
     * where <code>x0</code> is <code>startValue</code>, <code>x1</code> is <code>endValue</code>, 
     * and <code>t</code> is <code>fraction</code>. 
     * 
     * @param fraction   The fraction from the starting to the ending values 
     * @param startValue The start value; should be of type <code>int</code> or 
     *                   <code>Integer</code> 
     * @param endValue   The end value; should be of type <code>int</code> or <code>Integer</code> 
     * @return A linear interpolation between the start and end values, given the 
     *         <code>fraction</code> parameter. 
     */  
    public Integer evaluate(float fraction, Integer startValue, Integer endValue) {  
        int startInt = startValue;  
        return (int)(startInt + fraction * (endValue - startInt));  
    }  
}  
```

可以看到在IntEvaluator中只有一个函数evaluate(float fraction, Integer startValue, Integer endValue)
其中**fraction**就是插值器Interpolator中的返回值，表示当前动画的数值进度，百分制的小数表示。我们应该根据它来计算当前动画的值应该是多少。
**startValue**和**endValue**分别对应ofInt(int start,int end)中的start和end的数值；
比如我们假设当我们定义的动画ofInt(100,400)进行到数值进度20%的时候，那么此时在evaluate函数中，fraction的值就是0.2，startValue的值是100，endValue的值是400；理解上应该没什么难度。 

下面我们就来看看evaluate(float fraction, Integer startValue, Integer endValue) 是如何根据进度小数值来计算出具体数字的：

```java
return (int)(startInt + fraction * (endValue - startInt));  
```

大家对这个公式是否似曾相识？我们前面提到的公式：

```java
当前的值 = 100 + （400 - 100）* 显示进度  
```

在我们看懂了IntEvalutor以后，下面我们尝试自己写一个Evalutor

## 3、自定义Evalutor

### （1）、简单实现MyEvalutor

前面我们看了IntEvalutor的代码，我们仿照IntEvalutor的实现方法，我们自定义一个MyEvalutor: 
首先是实现TypeEvaluator接口：

```java
public class MyEvaluator implements TypeEvaluator<Integer> {  
    @Override  
    public Integer evaluate(float fraction, Integer startValue, Integer endValue) {  
        return null;  
    }  
}  
```

这里涉及到泛型的概念，不理解的同学可以去看看[《夯实JAVA基本之一 —— 泛型详解(1):基本使用》 ](http://blog.csdn.net/harvic880925/article/details/49872903)
在实现TypeEvaluator，我们给它指定它的返回是Integer类型，这样我们就可以在ofInt()中使用这个Evaluator了。
再说一遍原因：只有定义动画时的数值类型与Evalutor的返回值类型一样时，才能使用这个Evalutor；很显然ofInt()定义的数值类型是Integer而我们定义的MyEvaluator，它的返回值类型也是Integer；所以我们定义的MyEvaluator可以给ofInt（）来用。同理，如果我们把实现的TypeEvaluator填充为为Float类型，那么这个Evalutor也就只能给FloatEvalutor用了。 
我们来简单实现evaluate函数，代码如下：

```java
public class MyEvaluator implements TypeEvaluator<Integer> {  
    @Override  
    public Integer evaluate(float fraction, Integer startValue, Integer endValue) {
        int startInt = startValue;  
        return (int)(200+startInt + fraction * (endValue - startInt));  
    }  
}  
```

我们在IntEvaluator的基础上修改了下，让它返回值时增加了200；所以当我们定义的区间是ofInt(0,400)时，它的实际返回值区间应该是(200,600) 
我们看看MyEvaluator的使用：

```java
ValueAnimator animator = ValueAnimator.ofInt(0,400);  
  
animator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {  
    @Override  
    public void onAnimationUpdate(ValueAnimator animation) {  
        int curValue = (int)animation.getAnimatedValue();  
        tv.layout(tv.getLeft(),curValue,tv.getRight(),curValue+tv.getHeight());  
    }  
});  
animator.setDuration(1000);  
animator.setEvaluator(new MyEvaluator());  
animator.start();  
```

设置MyEvaluator前的动画效果： 
[![](http://upload-images.jianshu.io/upload_images/9028834-008bffa9ab97dd33?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-008bffa9ab97dd33?imageMogr2/auto-orient/strip)

然后再看看我们设置了MyEvaluator以后的效果： 

[![](http://upload-images.jianshu.io/upload_images/9028834-f7a620384e476987?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-f7a620384e476987?imageMogr2/auto-orient/strip)

很明显，textview的动画位置都向下移动了200个点； 
再重新看一下下面的这个流程图：

![](http://upload-images.jianshu.io/upload_images/9028834-81845d6b5c67b308.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在插值器Interpolator中，我们可以通过自定义插值器Interpolator的返回的数值进度来改变返回数值的位置。
在Evaluator中，我们又可以通过改变进度值所对应的具体数字来改变数值的位置。 

**结论**： 
我们可以通过**重写插值器Interpolator改变数值进度来改变数值位置**，**也可以通过改变Evaluator中进度所对应的数值来改变数值位置**。

下面我们就只通过重写Evaluator来实现数值的倒序输出； 

### （2）、实现倒序输出实例

我们自定义一个ReverseEvaluator:

```java
public class ReverseEvaluator implements TypeEvaluator<Integer> {  
    @Override  
    public Integer evaluate(float fraction, Integer startValue, Integer endValue) {  
        int startInt = startValue;  
        return (int) (endValue - fraction * (endValue - startInt));  
    }  
}  
```

其中 fraction * (endValue - startInt)表示动画实际运动的距离，我们用endValue减去实际运动的距离就表示随着运动距离的增加，离终点越来越远，这也就实现了从终点出发，最终运动到起点的效果了。 
使用方法：

```java
ValueAnimator animator = ValueAnimator.ofInt(0,400);  
  
animator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {  
    @Override  
    public void onAnimationUpdate(ValueAnimator animation) {  
        int curValue = (int)animation.getAnimatedValue();  
        tv.layout(tv.getLeft(),curValue,tv.getRight(),curValue+tv.getHeight());  
    }  
});  
animator.setDuration(1000);  
animator.setEvaluator(new ReverseEvaluator());  
animator.start();  
```

效果图： 
[![](http://upload-images.jianshu.io/upload_images/9028834-5cf681d8cdc3f526?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-5cf681d8cdc3f526?imageMogr2/auto-orient/strip)

## 4、关于ArgbEvalutor

### 1、使用ArgbEvalutor

我们上面讲了IntEvaluator和FloatEvalutor，还说了Evalutor一般来讲不能通用，会报强转错误，也就是说，只有在数值类型相同的情况下，Evalutor才能共用。 
其实除了IntEvaluator和FloatEvalutor，在android.animation包下，还有另外一个Evalutor叫ArgbEvalutor。 
**ArgbEvalutor**是用来做**颜色值过渡转换**的。可能是谷歌的开发人员觉得大家对颜色值变换可能并不知道要怎么做，所以特地给我们提供了这么一个过渡Evalutor； 
我们先来简单看一下ArgbEvalutor的源码：（这里先不做具体讲解原理，最后会讲原理，这里先会用）

```java
public class ArgbEvaluator implements TypeEvaluator {  
    public Object evaluate(float fraction, Object startValue, Object endValue) {  
        int startInt = (Integer) startValue;  
        int startA = (startInt >> 24);  
        int startR = (startInt >> 16) & 0xff;  
        int startG = (startInt >> 8) & 0xff;  
        int startB = startInt & 0xff;  
  
        int endInt = (Integer) endValue;  
        int endA = (endInt >> 24);  
        int endR = (endInt >> 16) & 0xff;  
        int endG = (endInt >> 8) & 0xff;  
        int endB = endInt & 0xff;  
  
        return (int)((startA + (int)(fraction * (endA - startA))) << 24) |  
                (int)((startR + (int)(fraction * (endR - startR))) << 16) |  
                (int)((startG + (int)(fraction * (endG - startG))) << 8) |  
                (int)((startB + (int)(fraction * (endB - startB))));  
    }  
}  
```

我们在这里关注两个地方，第一返回值是int类型，这说明我们可以使用ofInt()来初始化动画数值范围。第二：颜色值包括A,R,G,B四个值，每个值是8位所以用16进制表示一个颜色值应该是0xffff0000（纯红色） 
下面我们就使用一下ArgbEvaluator，并看看效果：

```java
ValueAnimator animator = ValueAnimator.ofInt(0xffffff00,0xff0000ff);  
animator.setEvaluator(new ArgbEvaluator());  
animator.setDuration(3000);  
  
animator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {  
    @Override  
    public void onAnimationUpdate(ValueAnimator animation) {  
        int curValue = (int)animation.getAnimatedValue();  
        tv.setBackgroundColor(curValue);  
  
    }  
});  
  
animator.start();  
```

在这段代码中，我们将动画的数据范围定义为(0xffffff00,0xff0000ff)，即从黄色，变为蓝色。 
在监听中，我们根据当前传回来的颜色值，将其设置为textview的背景色 
我们来看一下效果： 
[![](http://upload-images.jianshu.io/upload_images/9028834-0f15695083678751?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-0f15695083678751?imageMogr2/auto-orient/strip)

到这里，我们就已经知道ArgbEvalutor的使用方法和效果了，下面我们再来回头看看ArgbEvalutor的实现方法 

### 2、ArgbEvalutor的实现原理

先重新看源码：

```java
/** 
 * This evaluator can be used to perform type interpolation between integer 
 * values that represent ARGB colors. 
 */  
public class ArgbEvaluator implements TypeEvaluator {  
  
    /** 
     * This function returns the calculated in-between value for a color 
     * given integers that represent the start and end values in the four 
     * bytes of the 32-bit int. Each channel is separately linearly interpolated 
     * and the resulting calculated values are recombined into the return value. 
     * 
     * @param fraction The fraction from the starting to the ending values 
     * @param startValue A 32-bit int value representing colors in the 
     * separate bytes of the parameter 
     * @param endValue A 32-bit int value representing colors in the 
     * separate bytes of the parameter 
     * @return A value that is calculated to be the linearly interpolated 
     * result, derived by separating the start and end values into separate 
     * color channels and interpolating each one separately, recombining the 
     * resulting values in the same way. 
     */  
    public Object evaluate(float fraction, Object startValue, Object endValue) {  
        int startInt = (Integer) startValue;  
        int startA = (startInt >> 24);  
        int startR = (startInt >> 16) & 0xff;  
        int startG = (startInt >> 8) & 0xff;  
        int startB = startInt & 0xff;  
  
        int endInt = (Integer) endValue;  
        int endA = (endInt >> 24);  
        int endR = (endInt >> 16) & 0xff;  
        int endG = (endInt >> 8) & 0xff;  
        int endB = endInt & 0xff;  
  
        return (int)((startA + (int)(fraction * (endA - startA))) << 24) |  
                (int)((startR + (int)(fraction * (endR - startR))) << 16) |  
                (int)((startG + (int)(fraction * (endG - startG))) << 8) |  
                (int)((startB + (int)(fraction * (endB - startB))));  
    }  
}  
```

英文注释的那一坨大家有兴趣，可以看已看看，我这里就直接讲代码了 
这段代码分为三部分，第一部分根据startValue求出其中A,R,G,B中各个色彩的初始值；第二部分根据endValue求出其中A,R,G,B中各个色彩的结束值，最后是根据当前动画的百分比进度求出对应的数值 
我们先来看第一部分：根据startValue求出其中A,R,G,B中各个色彩的初始值

```java
int startInt = (Integer) startValue;  
int startA = (startInt >> 24);  
int startR = (startInt >> 16) & 0xff;  
int startG = (startInt >> 8) & 0xff;  
int startB = startInt & 0xff;  
```

这段代码就是根据位移和与运算求出颜色值中A,R,G,B各个部分对应的值;颜色值与ARGB值的对应关系如下： 
![image](http://upload-images.jianshu.io/upload_images/9028834-1e368875d6db1864?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

所以我们的初始值是0xffffff00,那么求出来的startA = 0xff,startR = oxff,startG = 0xff,startB = 0x00; 
关于通过位移和与运算如何得到指定位的值的问题，我就不再讲了，大家如果不理解，可以搜一下相关运算符使用方法的文章。 
同样，我们看看第二部分根据endValue求出其中A,R,G,B中各个色彩的结束值：

```java
int endInt = (Integer) endValue;  
int endA = (endInt >> 24);  
int endR = (endInt >> 16) & 0xff;  
int endG = (endInt >> 8) & 0xff;  
int endB = endInt & 0xff;  
```

原理与startValue求A,R,G,B对应值的一样，所以对于我们上面例子中初始值ofInt(0xffffff00,0xff0000ff)中的endValue:0xff0000ff所对应的endA = 0xff,endR = ox00;endG = 0x00;endB = 0xff; 
最后一部分到了，就是如何根据进度来求得变化的值，我们先看看下面这句是什么意思：

```java
startA + (int)(fraction * (endA - startA)))  
```

对于这个公式大家应该很容易理解，与IntEvaluator中的计算公式一样，就是根据透明度A的初始值、结束值求得当前进度下透明度A应该的数值。 
同理 
startR + (int)(fraction * (endR - startR)表示当前进度下的红色值 
startG + (int)(fraction * (endG - startG))表示当前进度下的绿色值 
startB + (int)(fraction * (endB - startB))表示当前进度下的蓝色值 
然后通过位移和或运算将当前进度下的A,R,G,B组合起来就是当前的颜色值了。

**源码下载地址**：
github:[自定义控件三部曲之动画篇(五)——ValueAnimator高级进阶（一）](https://github.com/harvic/BlogResForGitHub)

# 属性动画之ValueAnimator
## 概述
ValueAnimator是整个属性动画机制当中最核心的一个类，前面我们已经提到了，**属性动画的运行机制是通过不断地对值进行操作来实现的**，而初始值和结束值之间的**动画过渡**就是**由ValueAnimator**这个类来负责**计算**的。
它的内部使用一种时间循环的机制来计算值与值之间的动画过渡，我们只需要将初始值和结束值提供给ValueAnimator，并且告诉它动画所需运行的**时长**，那么ValueAnimator就会自动帮我们完成从初始值平滑地过渡到结束值这样的效果。除此之外，ValueAnimator还负责管理动画的播放**次数**、播放**模式**、以及**对动画设置监听器**等，确实是一个非常重要的类。


但是ValueAnimator的用法却一点都不复杂，我们先从最简单的功能看起吧，比如说想要将一个值从0平滑过渡到1，时长300毫秒，就可以这样写：

```java
ValueAnimator anim = ValueAnimator.ofFloat(0f, 1f);  
anim.setDuration(300);  
anim.start();  
```

怎么样？很简单吧，调用ValueAnimator的**ofFloat()**方法就可以构建出一个ValueAnimator的实例，ofFloat()方法当中允许传入多个float类型的参数，这里传入0和1就表示将值从0平滑过渡到1，然后调用ValueAnimator的setDuration()方法来设置动画运行的时长，最后调用start()方法启动动画。

用法就是这么简单，现在如果你运行一下上面的代码，动画就会执行了。可是这只是一个将值从0过渡到1的动画，又看不到任何界面效果，我们怎样才能知道这个动画是不是已经真正运行了呢？这就需要借助监听器来实现了，如下所示：

```java
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

```java
ValueAnimator anim = ValueAnimator.ofFloat(0f, 5f, 3f, 10f);  
anim.setDuration(5000);  
anim.start();  
```

当然也许你并不需要小数位数的动画过渡，可能你只是希望将一个整数值从0平滑地过渡到100，那么也很简单，只需要调用ValueAnimator的ofInt()方法就可以了，如下所示：

```java
ValueAnimator anim = ValueAnimator.ofInt(0, 100);  
```

### 常用方法

**ValueAnimator**当中**最常用**的应该就是**ofFloat()和ofInt()**这两个方法了，另外**还有**一个**ofObject()**方法，我会在下篇文章进行讲解。
**setStartDelay()**：动画延迟播放的时间
**setRepeatCount()**：动画循环播放的次数
**setRepeatMode()**：循环播放的模式，循环模式包括RESTART和REVERSE两种，分别表示重新播放和倒序播放的意思。

## ofObject
### 概述

ofInt()只能传入Integer类型的值，而ofFloat（）则只能传入Float类型的值。
那如果我们需要操作其它类型的变量要怎么办呢？其实ValueAnimator还有一个函数ofObject(),可以传进去任何类型的变量，定义如下：

```java
public static ValueAnimator ofObject(TypeEvaluator evaluator, Object... values);  
```

它有两个参数：
第一个是自定义的Evaluator，
第二个是可变长参数，Object类型的； 
大家可能会疑问，为什么要强制传进去自定义的Evaluator？首先，大家知道Evaluator的作用是根据当前动画的显示进度，计算出当前进度下把对应的值。那既然Object对象是我们自定的，那必然从进度到值的转换过程也必须由我们来做，不然系统哪知道你要转成个什么鬼。 
好了，现在我们先简单看一下ofObject这个怎么用。 

### 基本使用：自定义View A-Z字母变化
我们先来看看我们要实现的效果： 
[![](http://upload-images.jianshu.io/upload_images/9028834-9aa0f63b54c4afc8?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-9aa0f63b54c4afc8?imageMogr2/auto-orient/strip)

从效果图中可以看到，按钮上的**字母从A变化到Z**,刚开始变的慢，后来逐渐加速；

```java
ValueAnimator animator = ValueAnimator.ofObject(new CharEvaluator(),new Character('A'),new Character('Z'));  
animator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {  
    @Override  
    public void onAnimationUpdate(ValueAnimator animation) {  
        char text = (char)animation.getAnimatedValue();  
        tv.setText(String.valueOf(text));  
    }  
});  
animator.setDuration(10000);  
animator.setInterpolator(new AccelerateInterpolator());  
animator.start();  
```

这里注意三点： 
#### 第一，构造时：
```java
ValueAnimator animator = ValueAnimator.ofObject(new CharEvaluator(),new Character('A'),new Character('Z'));
```

我们自定义的一个**CharEvaluator**，这个类实现，后面会讲；在初始化动画时，传进去的是Character对象，一个是字母A,一个是字母Z; 
我们这里要实现的效果是，对Character对象来做动画，利用动画自动从字母A变到字母Z，具体怎么实现就是CharEvaluator的事了，这里我们只需要知道，在构造时传进去的是两个Character对象 

#### 第二：看监听：
```java
char text = (char)animation.getAnimatedValue();  
tv.setText(String.valueOf(text));  
```

通过animation.getAnimatedValue()得到当前动画的字符，然后把字符设置给textview；大家知道我们构造时传进去的值类型是Character对象，所以在动画过程中通过Evaluator返回的值类型必然跟构造时的类型是一致的，也是Character 

#### 第三：插值器
```java
animator.setInterpolator(new AccelerateInterpolator());  
```

我们使用的是加速插值器，加速插值器的特点就是随着动画的进行，速度会越来越快，这点跟我们上面的效果图是一致的。 

下面最关键的就是看CharEvaluator是怎么实现的了。
#### ASCII码中数值与字符的转换方法
我们知道在ASCII码表中，每个字符都是有数字跟他一一对应的，字母A到字母Z之间的所有字母对应的数字区间为65到90； 
而且在程序中，我们能通过数字强转成对应的字符。 
比如： 
**数字转字符**:
```java
char  temp = (char)65;//得到的temp的值就是大写字母A
```

**字符转数字**：
```java
char temp = 'A';  
int num = (int)temp;  //在这里得到的num值就是对应的ASCII码值65
```

#### CharEvaluator的实现：

```java
public class CharEvaluator implements TypeEvaluator<Character> {  
    @Override  
    public Character evaluate(float fraction, Character startValue, Character endValue) {  
        int startInt  = (int)startValue;  
        int endInt = (int)endValue;  
        int curInt = (int)(startInt + fraction *(endInt - startInt));  
        char result = (char)curInt;  
        return result;  
    }  
}  
```

在这里，我们就利用A-Z字符在**ASCII码表中对应数字是连续且递增**的原理，先求出来对应字符的数字值，然后再转换成对应的字符。代码难度不大，就不再细讲了。 

好了，到这里，有关ofObject()的使用大家应该就会了，上面我们说过，ofObject()能够初始化任何对象，下面我们就稍微加深些难度， 我们自定义一个类对象，然后利用ofObject()来构造这个对象的动画。


### 高级使用：自定义弹性圆的伸缩特效

我们先看看这部分，我们将实现的效果： 
[![](http://upload-images.jianshu.io/upload_images/9028834-b41f137a34242777?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-b41f137a34242777?imageMogr2/auto-orient/strip)

在这里，我们自定义了一个View，在这个view上画一个圆，但这个圆是有动画效果的。从效果中可以看出使用的插值器应该是回弹插值器（BounceInterpolator） 
下面就来看看这个动画是怎么做出来的

#### 1、首先，我们自定义一个类Point:

```java
public class Point {  
    private int radius;    
    public Point(int radius){  
        this.radius = radius;  
    }    
    public int getRadius() {  
        return radius;  
    }    
    public void setRadius(int radius) {  
        this.radius = radius;  
    }  
}  
```

point类内容很简单，只有一个成员变量：radius表示当前point的半径。

#### 2、然后我们自定义一个View:MyPointView

```java
public class MyPointView extends View {  
    private Point mCurPoint;  
    public MyPointView(Context context, AttributeSet attrs) {  
        super(context, attrs);  
    }  
  
    @Override  
    protected void onDraw(Canvas canvas) {  
        super.onDraw(canvas);  
        if (mCurPoint != null){  
            Paint paint = new Paint();  
            paint.setAntiAlias(true);  
            paint.setColor(Color.RED);  
            paint.setStyle(Paint.Style.FILL);  
            canvas.drawCircle(300,300,mCurPoint.getRadius(),paint);  
        }  
    }  
  
    public void doPointAnim(){  
        ValueAnimator animator = ValueAnimator.ofObject(new PointEvaluator(),new Point(20),new Point(200));  
        animator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {  
            @Override  
            public void onAnimationUpdate(ValueAnimator animation) {  
                mCurPoint = (Point)animation.getAnimatedValue();  
                invalidate();  
            }  
        });  
        animator.setDuration(1000);  
        animator.setInterpolator(new BounceInterpolator());  
        animator.start();  
    }  
}  
```

##### （1）、doPointAnim()函数

在这段代码中，首先来看看供外部调用开始动画的doPointAnim()函数：

```java
public void doPointAnim(){  
    ValueAnimator animator = ValueAnimator.ofObject(new PointEvaluator(),new Point(20),new Point(200));  
    animator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {  
        @Override  
        public void onAnimationUpdate(ValueAnimator animation) {  
            mCurPoint = (Point)animation.getAnimatedValue();  
            invalidate();  
        }  
    });  
    animator.setDuration(1000);  
    animator.setInterpolator(new BounceInterpolator());  
    animator.start();  
}  
```

同样，先来看ofObject的构造动画的方法：

```java
ValueAnimator animator = ValueAnimator.ofObject(new PointEvaluator(),new Point(20),new Point(200));  
```

在构造动画时，动画所对应的值的类型是Point对象，那说明我们自定义的PointEvaluator中的返回值也必然是Point了。有关PointEvaluator的实现后面再讲 
然后再来看看动画过程监听：

```java
animator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {  
    @Override  
    public void onAnimationUpdate(ValueAnimator animation) {  
        mCurPoint = (Point)animation.getAnimatedValue();  
        invalidate();  
    }  
});  
```

在监听过程中，先根据animation.getAnimatedValue()得到当前动画进度所对应的Point实例，保存在mCurPoint中，然后强制刷新 

##### （2）、OnDraw()函数

在强制刷新之后，就会走到OnDraw()函数下面：

```java
protected void onDraw(Canvas canvas) {  
    super.onDraw(canvas);  
    if (mCurPoint != null){  
        Paint paint = new Paint();  
        paint.setAntiAlias(true);  
        paint.setColor(Color.RED);  
        paint.setStyle(Paint.Style.FILL);  
        canvas.drawCircle(300,300,mCurPoint.getRadius(),paint);  
    }  
}  
```

onDraw函数没什么难度，就是根据mCurPoint的半径在（300,300）的位置画出来圆形，有关绘图的知识大家可以参考另一个系列[《android Graphics（一）：概述及基本几何图形绘制》 ](http://blog.csdn.net/harvic880925/article/details/38875149)

##### (3)、PointEvaluator

在构造ofObject中，我们也可以知道，初始值和动画中间值的类型都是Point类型，所以PointEvaluator输入的返回类型都应该是Point类型的，先看看PointEvaluator的完整代码：

```java
public class PointEvaluator implements TypeEvaluator<Point> {  
    @Override  
    public Point evaluate(float fraction, Point startValue, Point endValue) {  
        int start = startValue.getRadius();  
        int end  = endValue.getRadius();  
        int curValue = (int)(start + fraction * (end - start));  
        return new Point(curValue);  
    }  
}  
```

这段代码其实比较容易理解，就是根据初始半径和最终半径求出当前动画进程所对应的半径值，然后新建一个Point对象返回。

#### 3、使用MyPointView

首先在main.xml中添加对应的控件布局：从效果图中也可以看到，我们将MyPointView是布局在最下方的，布局代码如下：

```xml
<?xml version="1.0" encoding="utf-8"?>  
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"  
                android:orientation="vertical"  
                android:layout_width="fill_parent"  
                android:layout_height="fill_parent">  
  
    <Button  
            android:id="@+id/btn"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:layout_alignParentLeft="true"  
            android:padding="10dp"  
            android:text="start anim"  
            />  
  
    <Button  
            android:id="@+id/btn_cancel"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:layout_alignParentRight="true"  
            android:padding="10dp"  
            android:text="cancel anim"  
            />  
    <TextView  
            android:id="@+id/tv"  
            android:layout_width="100dp"  
            android:layout_height="wrap_content"  
            android:layout_centerHorizontal="true"  
            android:gravity="center"  
            android:padding="10dp"  
            android:background="#ffff00"  
            android:text="Hello qijian"/>  
  
    <com.harvic.BlogValueAnimator4.MyPointView  
            android:id="@+id/pointview"  
            android:layout_below="@id/tv"  
            android:layout_width="match_parent"  
            android:layout_height="match_parent"/>  
</RelativeLayout>  
```

其实也没什么难度，就是在原来的布局代码下面加一个MyPointView控件

然后我们来看看在MyActivity.java中是怎么来用的吧
```java
public class MyActivity extends Activity {  
    private Button btnStart;  
    private MyPointView mPointView;  
  
    @Override  
    public void onCreate(Bundle savedInstanceState) {  
        super.onCreate(savedInstanceState);  
        setContentView(R.layout.main);  
  
        btnStart = (Button) findViewById(R.id.btn);  
        mPointView = (MyPointView)findViewById(R.id.pointview);  
  
        btnStart.setOnClickListener(new View.OnClickListener() {  
            @Override  
            public void onClick(View v) {  
                mPointView.doPointAnim();  
            }  
        });  
    }  
}  
```

这段代码没什么难度，就是在点击start anim按钮的时候，调用mPointView.doPointAnim()方法开始动画。 



**源码下载地址：**
github:https://github.com/harvic/BlogResForGitHub

### 习题：对自定义View使用动画

目标：通过对对象进行值操作来实现动画效果

#### 实现目标

比如说我们有一个自定义的View，在这个View当中有一个Point对象用于管理坐标，然后在onDraw()方法当中就是根据这个Point对象的坐标值来进行绘制的。也就是说，如果我们可以对Point对象进行动画操作，那么整个自定义View的动画效果就有了。
效果图：
[![效果图](http://img.blog.csdn.net/20171221153249662?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)](http://img.blog.csdn.net/20171221153249662?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

#### 实现步骤

##### 先定义一个Point类：

Point类非常简单，只有x和y两个变量用于记录坐标的位置，并提供了构造方法来设置坐标，以及get方法来获取坐标。

```java
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

```java
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

```java
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

```java
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

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent"  
    >  
    <com.example.tony.myapplication.MyAnimView  
        android:layout_width="match_parent"  
        android:layout_height="match_parent" />  
</RelativeLayout>  
```


# 属性动画之ObjectAnimator
## 概述
相比于ValueAnimator，ObjectAnimator可能才是我们最常接触到的类，因为**ValueAnimator只**不过是**对值**进行了一个平滑的动画过渡，但我们实际使用到这种功能的场景好像并不多。而**ObjectAnimator**则就不同了，它是可以直接**任意对象**的**任意属性**进行动画操作的，比如说View的alpha属性。

不过虽说ObjectAnimator会更加常用一些，但是它其实是**继承自ValueAnimator**的，底层的动画实现机制也是基于ValueAnimator来完成的，因此ValueAnimator仍然是整个属性动画当中最核心的一个类。

### 1、引入
ValueAnimator有个缺点，就是只能对数值对动画计算。我们要想对哪个控件操作，需要监听动画过程，在监听中对控件操作。这样使用起来相比补间动画而言就相对比较麻烦。 
为了能让动画直接与对应控件相关联，以使我们从监听动画过程中解放出来，谷歌的开发人员在ValueAnimator的基础上，又派生了一个类ObjectAnimator; 
由于**ObjectAnimator是派生自ValueAnimator**的，所以ValueAnimator中所能使用的方法，在ObjectAnimator中都可以正常使用。 
但ObjectAnimator也重写了几个方法，比如ofInt(),ofFloat()等。我们先看看利用ObjectAnimator重写的ofFloat方法如何实现一个动画：（改变透明度）

```java
ObjectAnimator animator = ObjectAnimator.ofFloat(tv,"alpha",1,0,1);  
/*
参数1：传入一个object对象，我们想要对哪个对象进行动画操作就传入什么，这里我传入了一个tv。
参数2：想要对该对象的哪个属性进行动画操作，由于我们想要改变TextView的不透明度，因此这里传入"alpha"。
参数3：不固定长度，想要完成什么样的动画就传入什么值。
*/
animator.setDuration(2000);  
animator.start();  
```

效果图如下：
[![](http://upload-images.jianshu.io/upload_images/9028834-e2792efbd734a6ed?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-e2792efbd734a6ed?imageMogr2/auto-orient/strip)

我们这里还是直接使用上一篇的框架代码;（当点击start anim时执行动画）从上面的代码中可以看到构造ObjectAnimator的方法非常简单：

```java
public static ObjectAnimator ofFloat(Object target, String propertyName, float... values)   
```

*   第一个参数用于指定这个动画要操作的是哪个**控件**
*   第二个参数用于指定这个动画要操作这个控件的哪个**属性**
*   第三个参数是可变长参数，这个就跟ValueAnimator中的可变长参数的意义一样了，就是指这个**属性值是从哪变到哪**。像我们上面的代码中指定的就是将textview的alpha属性从0变到1再变到0；

下面我们再来看一下如何实现旋转效果：

```java
ObjectAnimator animator = ObjectAnimator.ofFloat(tv,"rotation",0,180,0);  
animator.setDuration(2000);  
animator.start();  
```

效果图如下：
[![](http://upload-images.jianshu.io/upload_images/9028834-f7ddc85df922e556?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-f7ddc85df922e556?imageMogr2/auto-orient/strip)

从代码中可以看到，我们只需要改变ofFloat()的第二个参数的值就可以实现对应的动画； 
那么问题来了，我们怎么知道第二个参数的值是啥呢？

### 2、setter函数

我们再回来看构造改变rotation值的ObjectAnimator的方法

```java
ObjectAnimator animator = ObjectAnimator.ofFloat(tv,"rotation",0,180,0);  
```

TextView控件有rotation这个属性吗？没有，不光TextView没有，连它的父类View中也没有这个属性。那它是怎么来改变这个值的呢？
其实，**ObjectAnimator**做动画，并不是根据控件xml中的属性来改变的，而是**通过指定属性所对应的set方法来改变**的。
比如，我们上面指定的改变rotation的属性值，ObjectAnimator在做动画时就会到指定控件（TextView）中去找对应的setRotation()方法来改变控件中对应的值。同样的道理，当我们在最开始的示例代码中，指定改变”alpha”属性值的时候，ObjectAnimator也会到TextView中去找对应的setAlpha()方法。那TextView中都有这些方法吗，有的，这些方法都是从View中继承过来的，在View中有关动画，总共有下面几组set方法：

```java
//1、透明度：alpha  
public void setAlpha(float alpha)  
  
//2、旋转度数：rotation、rotationX、rotationY  
public void setRotation(float rotation)  
public void setRotationX(float rotationX)  
public void setRotationY(float rotationY)  
  
//3、平移：translationX、translationY  
public void setTranslationX(float translationX)   
public void setTranslationY(float translationY)  
  
//缩放：scaleX、scaleY  
public void setScaleX(float scaleX)  
public void setScaleY(float scaleY)  
```

可以看到在View中已经实现了有关alpha,rotaion,translate,scale相关的set方法。所以我们在构造ObjectAnimator时可以直接使用。 

#### 使用注意：
在开始逐个看这些函数的使用方法前，我们先做一个**总结**： 
1、要使用ObjectAnimator来构造对画，要操作的控件中，**必须存在对应的属性的set方法** 
2、**setter 方法的命名必须以骆驼拼写法命名**，即set后每个单词首字母大写，其余字母小写，即类似于setPropertyName所对应的属性为propertyName 

下面我们就来看一下上面中各个方法的使用方法及作用。 
有关alpha的用法，上面已经讲过了，下面我们来看看其它的 

#### (1)、setRotationX、setRotationY与setRotation

*   setRotationX(float rotationX)：表示围绕X轴旋转，rotationX表示旋转度数 
*   setRotationY(rotationY):表示围绕Y轴旋转，rotationY表示旋转度数 
*   setRotation(float rotation):表示围绕Z旋转,rotation表示旋转度数 

先来看看setRotationX的效果：

```java
ObjectAnimator animator = ObjectAnimator.ofFloat(tv,"rotationX",0,270,0);  
animator.setDuration(2000);  
animator.start();  
```

效果图如下： 

[![](http://upload-images.jianshu.io/upload_images/9028834-d8c191e0991af4f6?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-d8c191e0991af4f6?imageMogr2/auto-orient/strip)

从效果图中明显看出，textview的旋转方法是围绕X轴旋转的，我们设定为从0度旋转到270度再返回0度。 
然后再来看看setRotationY的使用方法与效果：

```java
ObjectAnimator animator = ObjectAnimator.ofFloat(tv,"rotationY",0,180,0);  
animator.setDuration(2000);  
animator.start();  
```

效果图如下： 

[![](http://upload-images.jianshu.io/upload_images/9028834-a6aa186b7591a9bb?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-a6aa186b7591a9bb?imageMogr2/auto-orient/strip)

从效果图中明显可以看出围绕Y轴旋转的。 
我们再来看看setRotation的用法与效果：

```java
ObjectAnimator animator = ObjectAnimator.ofFloat(tv,"rotation",0,270,0);  
animator.setDuration(2000);  
animator.start();  
```

[![](http://upload-images.jianshu.io/upload_images/9028834-dcfae682fd4f6307?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-dcfae682fd4f6307?imageMogr2/auto-orient/strip)

setRotation是围绕Z轴旋转的，我们来看一张图： 

![](http://upload-images.jianshu.io/upload_images/9028834-3e2f1fca0cd25668?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

从这张图中，绿色框部分表示手机屏幕，很明显可以看出Z轴就是从屏幕左上角原点向外伸出的一条轴。这样，我们也就可以理解围绕Z轴旋转，为什么是这样子转了。 

#### （2）、setTranslationX与setTranslationY

*   setTranslationX(float translationX) :表示在X轴上的平移距离,以当前控件为原点，向右为正方向，参数translationX表示移动的距离。 
*   setTranslationY(float translationY) :表示在Y轴上的平移距离，以当前控件为原点，向下为正方向，参数translationY表示移动的距离。 

我们先看看setTranslationX的用法：

```java
ObjectAnimator animator = ObjectAnimator.ofFloat(tv, "translationX", 0, 200, -200,0);  
animator.setDuration(2000);  
animator.start();  
```

效果图如下： 
[![](http://upload-images.jianshu.io/upload_images/9028834-e6e07f3405fd51f9.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://upload-images.jianshu.io/upload_images/9028834-e6e07f3405fd51f9.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

所以，我们上面在构造动画时，指定的移动距离是（0, 200, -200,0），所以控件会从自身所有位置向右移动200像素，然后再移动到距离原点-200的位置，最后回到原点； 
然后我们来看看setTranslateY的用法：

```java
ObjectAnimator animator = ObjectAnimator.ofFloat(tv, "translationY", 0, 200, -100,0);  
animator.setDuration(2000);  
animator.start();  
```

效果图如下：（为了方便看到效果，将textview垂直居中） 
[![](http://upload-images.jianshu.io/upload_images/9028834-20392af8ef67323e?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-20392af8ef67323e?imageMogr2/auto-orient/strip)

同样，移动位置的坐标也都是以当前控件所在位置为中心点的。所以对应的移动位置从原点移动向下移动200像素，然后再移动到向下到距原点200像素的位置，最后再回到(0,0)从效果图中很明显可以看出来。 
从上面可以看出：每次移动距离的计算都是以原点为中心的；比如初始动画为ObjectAnimator.ofFloat(tv, “translationY”, 0, 200, -100,0)表示首先从0移动到正方向200的位置，然后再移动到负方向100的位置，最后移动到原点。 

#### (3)、setScaleX与setScaleY

*   setScaleX(float scaleX):在X轴上缩放，scaleX表示缩放倍数 
*   setScaleY(float scaleY):在Y轴上缩放，scaleY表示缩放倍数 

我们来看看setScaleX的用法：

```java
ObjectAnimator animator = ObjectAnimator.ofFloat(tv, "scaleX", 0, 3, 1);  
animator.setDuration(2000);  
animator.start();  
```

效果图如下： 
[![](http://upload-images.jianshu.io/upload_images/9028834-5475792fa42ac52a?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-5475792fa42ac52a?imageMogr2/auto-orient/strip)

在效果图中，从0倍放大到3倍，然后再还原到1倍的原始状态。 
然后再来看看setScaleY的用法

```java
ObjectAnimator animator = ObjectAnimator.ofFloat(tv, "scaleY", 0, 3, 1);  
animator.setDuration(2000);  
animator.start();  
```

为了更好的看到效果，我把textview垂直居中了，效果图如下： 
[![](http://upload-images.jianshu.io/upload_images/9028834-040700a8f990b898?imageMogr2/auto-orient/strip)
](http://upload-images.jianshu.io/upload_images/9028834-040700a8f990b898?imageMogr2/auto-orient/strip)

好了，到这里有关View中自带的set函数讲完了，我们来看看ObjectAnimator是如何实现控件动画效果的。

### 3、ObjectAnimator动画原理

我们先来看张图： 
[![](http://upload-images.jianshu.io/upload_images/9028834-88fecd8826548552.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://upload-images.jianshu.io/upload_images/9028834-88fecd8826548552.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在这张图中，将ValueAnimator的动画流程与ObjectAnimator的动画流程做了个对比。 
可以看到ObjectAnimator的动画流程中，也是首先通过加速器产生当前进度的百分比，然后再经过Evaluator生成对应百分比所对应的数字值。
这两步与ValueAnimator是完全一样的，**唯一不同的是最后一步**：
在**ValueAnimator**中，我们要通过添加**监听器**来监听当前数字值；
在**ObjectAnimator**中，则是先根据属性值拼装成对应的**set函数**的名字；
　比如这里的scaleY的拼装方法就是将属性的第一个字母强制大写后，与set拼接，所以就是setScaleY。然后通过反射找到对应控件的setScaleY(float scaleY)函数，将当前数字值做为setScaleY(float scale)的参数将其传入。 
这里在找到控件的set函数以后，是通过反射来调用这个函数的，有关反射的使用大家可以参考[《夯实JAVA基本之二 —— 反射（1）：基本类周边信息获取》 ](http://blog.csdn.net/harvic880925/article/details/50072739)
这就是**ObjectAnimator**的流程，最后一步总结起来就是**调用对应属性的set方法，将动画当前数字值做为参数传进去**。 

**根据上面的流程，这里有几个注意事项： **
**(1)、拼接set函数的方法：**上面我们也说了是首先是强制将属性的第一个字母大写，然后与set拼接，就是对应的set函数的名字。注意，只是强制将属性的第一个字母大写，后面的部分是保持不变的。反过来，如果我们的函数名命名为setScalePointX(float ),那我们在写属性时可以写成”scalePointX”或者写成“ScalePointX”都是可以的，即第一个字母大小写可以随意，但后面的部分必须与set方法后的大小写保持一致。 
**(2)、如何确定函数的参数类型：**上面我们知道了如何找到对应的函数名，那对应的参数方法的参数类型如何确定呢？我们在讲ValueAnimator的时候说过，动画过程中产生的数字值与构造时传入的值类型是一样的。由于ObjectAnimator与ValueAnimator在插值器和Evaluator这两步是完全一样的，而当前动画数值的产生是在Evaluator这一步产生的，所以ObjectAnimator的动画中产生的数值类型也是与构造时的类型一样的。那么问题来了，像我们的构造方法。

```java
ObjectAnimator animator = ObjectAnimator.ofFloat(tv, "scaleY", 0, 3, 1);  
```

由于构造时使用的是ofFloat函数，所以中间值的类型应该是Float类型的，所以在最后一步拼装出来的set函数应该是setScaleY(float xxx)的样式；这时，系统就会利用反射来找到setScaleY(float xxx)函数，并把当前的动画数值做为参数传进去。 
那问题来了，如果没有类似setScaleY(float xxx)的函数，我们只实现了一个setScaleY(int xxx)的函数怎么办？这里虽然函数名一样，但参数类型是不一样的，那么系统就会报一个错误： 
![image](http://upload-images.jianshu.io/upload_images/9028834-c1cb1f4605c5c972?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

意思就是对应函数的指定参数类型没有找到。 
**（3）、调用set函数以后怎么办？**从ObjectAnimator的流程可以看到，ObjectAnimator只负责把动画过程中的数值传到对应属性的set函数中就结束了，注意传给set函数以后就结束了！set函数就相当我们在ValueAnimator中添加的监听的作用，set函数中的对控件的操作还是需要我们自己来写的。

那我们来看看View中的setScaleY是怎么实现的吧：

```java
/** 
 * Sets the amount that the view is scaled in Y around the pivot point, as a proportion of 
 * the view's unscaled width. A value of 1 means that no scaling is applied. 
 * 
 * @param scaleY The scaling factor. 
 * @see #getPivotX() 
 * @see #getPivotY() 
 * 
 * @attr ref android.R.styleable#View_scaleY 
 */  
public void setScaleY(float scaleY) {  
    ensureTransformationInfo();  
    final TransformationInfo info = mTransformationInfo;  
    if (info.mScaleY != scaleY) {  
        invalidateParentCaches();  
        // Double-invalidation is necessary to capture view's old and new areas  
        invalidate(false);  
        info.mScaleY = scaleY;  
        info.mMatrixDirty = true;  
        mPrivateFlags |= DRAWN; // force another invalidation with the new orientation  
        invalidate(false);  
    }  
}  
```

大家不必理解这一坨代码的意义，因为这些代码是需要读懂View的整体流程以后才能看得懂的，只需要跟着我的步骤来理解就行。
这段代码总共分为两部分：第一步重新设置当前控件的参数，第二步调用Invalidate()强制重绘； 
所以在重绘时，控件就会根据最新的控件参数来绘制了，所以我们就看到当前控件被缩放了。 
**(4)、set函数调用频率是多少：**由于我们知道动画在进行时，每隔十几毫秒会刷新一次，所以我们的set函数也会每隔十几毫秒会被调用一次。 

讲了这么多，就是为了强调一点：**ObjectAnimator只负责把当前运动动画的数值传给set函数**。至于set函数里面怎么来做，是我们自己的事了。 

好了，在知道了ObjectAnimator的原理以后，下面就来看看如何自定义一个ObjectAnimator的属性吧。

## 自定义ObjectAnimator属性

我们在开始之前再来捋一下**ObjectAnimator的动画设置流程**：
ObjectAnimator需要指定操作的控件对象，在开始动画时，到控件类中去寻找设置属性所对应的set函数，然后把动画中间值做为参数传给这个set函数并执行它。 

所以，控件类中必须所要设置属性所要对应的set函数。所以为了自由控制控件的实现，我们这里自定义一个控件。大家知道在这个自定义控件中，肯定存在一个set函数与我们自定义的属性相对应。 
我们先来看看这段要实现的效果：
[![](http://upload-images.jianshu.io/upload_images/9028834-3b49afa335a17a31?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-3b49afa335a17a31?imageMogr2/auto-orient/strip)

这个控件中存在一个圆形，在动画时先将这个圆形放大，然后再将圆形还原。

### 1、保存圆形信息类——Point

为了，保存圆形的信息，我们先定义一个类：(Point.java)

```java
public class Point {  
    private int mRadius;    
    public Point(int radius){  
        mRadius = radius;  
    }    
    public int getRadius() {  
        return mRadius;  
    }    
    public void setRadius(int radius) {  
        mRadius = radius;  
    }  
}  
```

这个类很好理解，只有一个成员变量mRadius,表示圆的半径。

### 2、自定义控件——MyPointView

然后我们自定义一个控件MyPointView,完整代码如下：

```java
public class MyPointView extends View {  
    private Point mPoint = new Point(100);  
  
    public MyPointView(Context context, AttributeSet attrs) {  
        super(context, attrs);  
    }  
  
    @Override  
    protected void onDraw(Canvas canvas) {  
        if (mPoint != null){  
            Paint paint = new Paint();  
            paint.setAntiAlias(true);  
            paint.setColor(Color.RED);  
            paint.setStyle(Paint.Style.FILL);  
            canvas.drawCircle(300,300,mPoint.getRadius(),paint);  
        }  
        super.onDraw(canvas);  
    }  
  
    void setPointRadius(int radius){  
        mPoint.setRadius(radius);  
        invalidate();  
    }    
}  
```

在这段代码中，首先来看我们前面讲到的set函数：

```java
void setPointRadius(int radius){  
    mPoint.setRadius(radius);  
    invalidate();  
}  
```

- 第一点，这个set函数所对应的属性应该是pointRadius或者PointRadius。前面我们已经讲了第一个字母大小写无所谓，后面的字母必须保持与set函数完全一致。 
- 第二点，在setPointRadius中，先将当前动画传过来的值保存到mPoint中，做为当前圆形的半径。然后强制界面刷新 

在界面刷新后，就开始执行onDraw()函数：

```java
@Override  
protected void onDraw(Canvas canvas) {  
    if (mPoint != null){  
        Paint paint = new Paint();  
        paint.setAntiAlias(true);  
        paint.setColor(Color.RED);  
        paint.setStyle(Paint.Style.FILL);  
        canvas.drawCircle(300,300,mPoint.getRadius(),paint);  
    }  
    super.onDraw(canvas);  
}  
```

在onDraw函数中，就是根据当前mPoint的半径值在(300,300)点外画一个圆；有关画圆的知识，大家可以参考[《android Graphics（一）：概述及基本几何图形绘制》](http://blog.csdn.net/harvic880925/article/details/38875149)

### 3、使用MyPointView

首先，在MyActivity的布局中添加MyPointView的使用(main.xml)：

```xml
<?xml version="1.0" encoding="utf-8"?>  
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"  
                android:orientation="vertical"  
                android:layout_width="fill_parent"  
                android:layout_height="fill_parent">    
    <Button  
            android:id="@+id/btn"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:layout_alignParentLeft="true"  
            android:padding="10dp"  
            android:text="start anim"  
            />    
    <Button  
            android:id="@+id/btn_cancel"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:layout_alignParentRight="true"  
            android:padding="10dp"  
            android:text="cancel anim"  
            />  
    <TextView  
            android:id="@+id/tv"  
            android:layout_width="100dp"  
            android:layout_height="wrap_content"  
            android:layout_centerHorizontal="true"  
            android:gravity="center"  
            android:padding="10dp"  
            android:background="#ffff00"  
            android:text="Hello qijian"/>    
    <com.example.BlogObjectAnimator1.MyPointView  
            android:id="@+id/pointview"  
            android:layout_width="match_parent"  
            android:layout_height="match_parent"  
            android:layout_below="@id/tv"/>    
</RelativeLayout>  
```

然后看看在MyActivity中，点击start anim后的处理方法：

```java
public class MyActivity extends Activity {  
    private Button btnStart;  
    private MyPointView mPointView;  
  
    @Override  
    public void onCreate(Bundle savedInstanceState) {  
        super.onCreate(savedInstanceState);  
        setContentView(R.layout.main);  
  
        btnStart = (Button) findViewById(R.id.btn);  
        mPointView = (MyPointView)findViewById(R.id.pointview);  
  
        btnStart.setOnClickListener(new View.OnClickListener() {  
            @Override  
            public void onClick(View v) {  
                doPointViewAnimation();  
            }  
        });  
    }  
  …………  
}       
```

在点击start anim按钮后，开始执行doPointViewAnimation()函数，doPointViewAnimation()函数代码如下：

```java
private void doPointViewAnimation(){  
     ObjectAnimator animator = ObjectAnimator.ofInt(mPointView, "pointRadius", 0, 300, 100);  
      animator.setDuration(2000);  
      animator.start();  
}  
```

在这段代码中，着重看ObjectAnimator的构造方法，首先要操作的控件对象是mPointView，然后对应的属性是pointRadius，然后值是从0到300再到100； 
所以在动画开始以后，ObjectAnimator就会实时地把动画中产生的值做为参数传给MyPointView类中的setPointRadius(int radius)函数，然后调用setPointRadius(int radius)。由于我们在setPointRadius(int radius)中实时地设置圆形的半径值然后强制重绘当前界面，所以可以看到圆形的半径会随着动画的进行而改变。 


### 4、注意——何时需要实现对应属性的get函数 

我们再来看一下ObjectAinimator的下面三个构造方法：

```java
public static ObjectAnimator ofFloat(Object target, String propertyName, float... values)  
public static ObjectAnimator ofInt(Object target, String propertyName, int... values)  
public static ObjectAnimator ofObject(Object target, String propertyName,TypeEvaluator evaluator, Object... values)  
```

前面我们已经分别讲过三个函数的使用方法，在上面的三个构造方法中最后一个参数都是可变长参数。我们也讲了，他们的意义就是从哪个值变到哪个值的。

那么问题来了：前面我们都是定义多个值，即至少两个值之间的变化，那如果我们只定义一个值呢，如下面的方式：(同样以MyPointView为例)
```java
ObjectAnimator animator = ObjectAnimator.ofInt(mPointView, "pointRadius",100);  
```

我们在这里只传递了一个变化值100；那它从哪里开始变化呢？我们来看一下效果：
代码如下：

```java
ObjectAnimator animator = ObjectAnimator.ofInt(mPointView, "pointRadius",100);  
animator.setDuration(2000);  
animator.start();  
```

效果图如下：
[![](http://upload-images.jianshu.io/upload_images/9028834-cc791bdf1143213c?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-cc791bdf1143213c?imageMogr2/auto-orient/strip)

从效果图中看起来是从0开始的，但是看log可以看出来已经在出警告了：
[![](http://upload-images.jianshu.io/upload_images/9028834-1aca1713e9063def?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://upload-images.jianshu.io/upload_images/9028834-1aca1713e9063def?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们点了三次start anim按钮，所以这里也报了三次，意思就是没找到pointRadius属性所对应的getPointRadius()函数；

**仅当我们只给动画设置一个值时，程序才会调用属性对应的get函数来得到动画初始值**。如果动画**没有初始值，那么就会使用系统默认值**。比如ofInt（）中使用的参数类型是int类型的，而系统的Int值的默认值是0，所以动画就会从0运动到100；也就是系统虽然在找到不到属性对应的get函数时，会给出警告，但同时会用系统默认值做为动画初始值。

如果通过给自定义控件MyPointView设置了get函数，那么将会以get函数的返回值做为初始值：

```java
public class MyPointView extends View {  
    private Point mPoint = new Point(100);    
    public MyPointView(Context context, AttributeSet attrs) {  
        super(context, attrs);  
    }    
    @Override  
    protected void onDraw(Canvas canvas) {  
        if (mPoint != null){  
            Paint paint = new Paint();  
            paint.setAntiAlias(true);  
            paint.setColor(Color.RED);  
            paint.setStyle(Paint.Style.FILL);  
            canvas.drawCircle(300,300,mPoint.getRadius(),paint);  
        }  
        super.onDraw(canvas);  
    }  
  
    public int getPointRadius(){  
        return 50;  
    }    
    public void setPointRadius(int radius){  
        mPoint.setRadius(radius);  
        invalidate();  
    }    
}  
```

我们在这里添加了getPointRadius函数，返回值是Int.有些同学可能会疑惑：我怎么知道这里要返回int值呢？
我们前面说过当且仅当我们在创建ObjectAnimator时，只给他传递了一个过渡值的时候，系统才会调用属性对应的get函数来得到动画的初始值！所以做为动画的初始值，那么在创建动画时过渡值传的什么类型，这里的get函数就要返回类型

```java
public static ObjectAnimator ofObject(Object target, String propertyName,TypeEvaluator evaluator, Object... values)  
```
比如上面的ofObject,get函数所返回的类型就是与最后一个参数Object... values，相同类型的。
在我们在MyPointView添加上PointRadius所对应的get函数以后重新执行动画：

```java
ObjectAnimator animator = ObjectAnimator.ofInt(mPointView, "pointRadius",100);  
animator.setDuration(2000);  
animator.start();  
```

此时的效果图如下：
[![](http://upload-images.jianshu.io/upload_images/9028834-99f3250637517e13?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-99f3250637517e13?imageMogr2/auto-orient/strip)

从动画中可以看出，半径已经不是从0开始的了，而是从50开始的。

**最后我们总结一下：当且仅当动画的只有一个过渡值时，系统才会调用对应属性的get函数来得到动画的初始值。**


## 常用函数

有关常用函数这一节其实没有太多讲的必要。因为ObjectAnimator的函数都是从ValueAnimator中继承而来的，所以用法和效果与ValueAnimator是完全一样的。我们这里只讲解一下Evaluator的用法，其它的也就不再讲了。

### 1、使用ArgbEvaluator

我们搜一下TextView所有的函数发现，TextView有一个set函数能够改变背景色：

```java
public void setBackgroundColor(int color);  
```

大家可以回想到，我们在ValueAnimator中也曾改变过背景色，使用的是ArgbEvaluator。在这里我们再回顾下ArgbEvaluator，它的实现代码如下：

```java
public class ArgbEvaluator implements TypeEvaluator {  
    public Object evaluate(float fraction, Object startValue, Object endValue) {  
        int startInt = (Integer) startValue;  
        int startA = (startInt >> 24);  
        int startR = (startInt >> 16) & 0xff;  
        int startG = (startInt >> 8) & 0xff;  
        int startB = startInt & 0xff;  
  
        int endInt = (Integer) endValue;  
        int endA = (endInt >> 24);  
        int endR = (endInt >> 16) & 0xff;  
        int endG = (endInt >> 8) & 0xff;  
        int endB = endInt & 0xff;  
  
        return (int)((startA + (int)(fraction * (endA - startA))) << 24) |  
                (int)((startR + (int)(fraction * (endR - startR))) << 16) |  
                (int)((startG + (int)(fraction * (endG - startG))) << 8) |  
                (int)((startB + (int)(fraction * (endB - startB))));  
    }  
}  
```

ArgbEvaluator的返回值是Integer类型，所以我们要使用ArgbEvaluator的话，构造ObjectAnimator时必须使用ofInt() 

下面我们来看看**使用ArgbEvaluator的代码**：

```java
ObjectAnimator animator = ObjectAnimator.ofInt(tv, "BackgroundColor", 0xffff00ff, 0xffffff00, 0xffff00ff);  
animator.setDuration(8000);  
animator.setEvaluator(new ArgbEvaluator());  
animator.start();  
```

然后我们来看下代码效果： 
[![image](http://upload-images.jianshu.io/upload_images/9028834-a3d4cd2dd4c4fa64?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-a3d4cd2dd4c4fa64?imageMogr2/auto-orient/strip)



### 2、其它函数

下面把其它所涉及到的函数的列表列在下面，大家可以参考ValueAnimator的使用方法来使用。

#### （1）、常用函数

```java
/* 设置动画时长，单位是毫秒  */  
ValueAnimator setDuration(long duration)  
/* 获取ValueAnimator在运动时，当前运动点的值  */  
Object getAnimatedValue();  
/* 开始动画  */  
void start()  
/* 设置循环次数,设置为INFINITE表示无限循环  */  
void setRepeatCount(int value)  
/* 设置循环模式 
 * value取值有RESTART，REVERSE，  */  
void setRepeatMode(int value)  
/* 取消动画  */  
void cancel()  
```

#### （2）、监听器相关

```java
/* 监听器一：监听动画变化时的实时值  */  
public static interface AnimatorUpdateListener {  
    void onAnimationUpdate(ValueAnimator animation);  
}  
//添加方法为：public void addUpdateListener(AnimatorUpdateListener listener)  

/* 监听器二：监听动画变化时四个状态  */  
public static interface AnimatorListener {  
    void onAnimationStart(Animator animation);  
    void onAnimationEnd(Animator animation);  
    void onAnimationCancel(Animator animation);  
    void onAnimationRepeat(Animator animation);  
}  
//添加方法为：public void addListener(AnimatorListener listener)   
```

#### （3）、插值器与Evaluator

```java
/* 设置插值器  */  
public void setInterpolator(TimeInterpolator value)  
/* 设置Evaluator  */  
public void setEvaluator(TypeEvaluator value)  
```

**源码下载地址：**
**github:[https://github.com/harvic/BlogResForGitHub](https://github.com/harvic/BlogResForGitHub)**

## 习题：自定义View一边动一边变色

### 实现目标

动态改变View的颜色。
效果图：
[![效果图](http://img.blog.csdn.net/20171221162602094?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)](http://img.blog.csdn.net/20171221162602094?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### 实现步骤

#### 自定义MyAnimView属性

　　ObjectAnimator内部的工作机制是通过寻找特定属性的get和set方法，然后通过方法不断地对值进行改变，从而实现动画效果的。
　　因此我们就需要在MyAnimView中**定义一个color属性，并提供它的get和set方法**。这里我们可以将color属性设置为字符串类型，使用#RRGGBB这种格式来表示颜色值，代码如下所示：
　　在setColor()方法当中，将画笔的颜色设置成方法参数传入的颜色，然后调用了invalidate()方法。即在改变了画笔颜色之后立即刷新视图，然后onDraw()方法就会调用。在onDraw()方法当中会根据当前画笔的颜色来进行绘制，这样颜色也就会动态进行改变了。

直接使用【习题：对自定义View使用动画】中的MyAnimView
```java
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

#### 自定义TypeEvaluator：

　　要借助ObjectAnimator类让setColor()方法得到调用了，在使用ObjectAnimator之前我们还要完成一个非常重要的工作，就是编写一个用于**告知系统如何进行颜色过度的TypeEvaluator**。
创建ColorEvaluator并实现TypeEvaluator接口，代码如下所示：
　　首先在evaluate()方法当中获取到颜色的初始值和结束值，并通过字符串截取的方式将颜色分为RGB三个部分，并将RGB的值转换成十进制数字，那么每个颜色的取值范围就是0-255。接下来计算一下初始颜色值到结束颜色值之间的差值，这个差值很重要，决定着颜色变化的快慢，如果初始颜色值和结束颜色值很相近，那么颜色变化就会比较缓慢，而如果颜色值相差很大，比如说从黑到白，那么就要经历255*3这个幅度的颜色过度，变化就会非常快。

　　那么控制颜色变化的速度是通过getCurrentColor()这个方法来实现的，这个方法会根据当前的fraction值来计算目前应该过度到什么颜色，并且这里会根据初始和结束的颜色差值来控制变化速度，最终将计算出的颜色进行返回。

　　最后，由于我们计算出的颜色是十进制数字，这里还需要调用一下getHexString()方法把它们转换成十六进制字符串，再将RGB颜色拼装起来之后作为最终的结果返回。

```java
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
  
    /* 根据fraction值来计算当前的颜色。      */  
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
      
    /* 将10进制颜色值转换成16进制。      */  
    private String getHexString(int value) {  
        String hexString = Integer.toHexString(value);  
        if (hexString.length() == 1) {  
            hexString = "0" + hexString;  
        }  
        return hexString;  
    }  
  
}  
```

#### 调用

比如说我们想要实现从蓝色到红色的动画过度，历时5秒，就可以这样写：

```java
ObjectAnimator anim = ObjectAnimator.ofObject(myAnimView, "color", new ColorEvaluator(),   
    "#0000FF", "#FF0000");  
anim.setDuration(5000);  
anim.start();  
```

接下来我们需要将上面一段代码移到MyAnimView类当中，让它和刚才的Point移动动画可以结合到一起播放，这就要借助**组合动画**的技术了。修改MyAnimView中的代码，如下所示：

```java
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
        //组合动画AnimatorSet 
        AnimatorSet animSet = new AnimatorSet();  
        animSet.play(anim).with(anim2);  
        animSet.setDuration(5000);  
        animSet.start();  
    }    
}  
```
由于这段代码本身就是在MyAnimView当中执行的，因此ObjectAnimator.ofObject()的第一个参数直接传this就可以了。接着我们又创建了一个AnimatorSet，并把两个动画设置成同时播放，动画时长为五秒，最后启动动画。








# Interpolator插值器

Interpolator这个东西很难进行翻译，直译过来的话是插值器的意思，它的主要作用是可以**控制动画的变化速率**，比如去实现一种非线性运动的动画效果。那么什么叫做非线性运动的动画效果呢？就是说动画改变的速率不是一成不变的，像加速运动以及减速运动都属于非线性运动。

不过Interpolator并不是属性动画中新增的技术，实际上从Android 1.0版本开始就一直存在Interpolator接口了，而之前的补间动画当然也是支持这个功能的。只不过在**属性动画**中**新增**了一个**TimeInterpolator**接口，这个接口是用于兼容之前的Interpolator的，这使得所有过去的Interpolator实现类都可以直接拿过来放到属性动画当中使用。

TimeInterpolator接口的所有实现类：
![](http://img.blog.csdn.net/20171221225916611?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

TimeInterpolator接口已经有非常多的实现类了，这些都是Android系统内置好的并且我们可以直接使用的Interpolator。每个Interpolator都有它各自的实现效果，比如说AccelerateInterpolator就是一个加速运动的Interpolator，而DecelerateInterpolator就是一个减速运动的Interpolator。

## Interpolator常见实现类

[![Interpolator实现类](http://img.blog.csdn.net/20180117211154977?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)](http://img.blog.csdn.net/20180117211154977?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

| 实现类                              | 意义                          |
| -------------------------------- | --------------------------- |
| AccelerateDecelerateInterpolator | 在动画开始与介绍的地方速率改变比较慢，在中间的时候加速 |
| AccelerateInterpolator           | 在动画开始的地方速率改变比较慢，然后开始加速      |
| AnticipateInterpolator           | 开始的时候向后然后向前甩                |
| AnticipateOvershootInterpolator  | 开始的时候向后然后向前甩一定值后返回最后的值      |
| BounceInterpolator               | 动画结束的时候弹起                   |
| CycleInterpolator                | 动画循环播放特定的次数，速率改变沿着正弦曲线      |
| DecelerateInterpolator           | 在动画开始的地方快然后慢                |
| LinearInterpolator               | 以常量速率改变                     |
| OvershootInterpolator            | 向前甩一定值后再回到原来位置              |

## 补间动画中使用Interpolator

### scale标签

下面先看看Scale标签应用插值器后，都会变成什么样。

先看下XML代码：（从控件中心点，从0放大到1.4倍，保持结束时的状态）

```xml
<?xml version="1.0" encoding="utf-8"?>  
<scale xmlns:android="http://schemas.android.com/apk/res/android"  
    android:interpolator="@android:anim/accelerate_decelerate_interpolator"  
    android:fromXScale="0.0"  
    android:toXScale="1.4"  
    android:fromYScale="0.0"  
    android:toYScale="1.4"  
    android:pivotX="50%"  
    android:pivotY="50%"  
    android:duration="700"   
    android:fillAfter="true"  
/>  
```

下面一个个看看，每个xml值对应的scale动画是怎样的。

[![image](http://upload-images.jianshu.io/upload_images/9028834-9641948c4c015331?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-9641948c4c015331?imageMogr2/auto-orient/strip)

AccelerateInterpolator 在动画开始的地方速率改变比较慢，然后开始加速
[![image](http://upload-images.jianshu.io/upload_images/9028834-cd8bd284644b33b7?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-cd8bd284644b33b7?imageMogr2/auto-orient/strip)

DecelerateInterpolator 在动画开始的地方快然后慢
[![image](http://upload-images.jianshu.io/upload_images/9028834-f769b2a42889de76?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-f769b2a42889de76?imageMogr2/auto-orient/strip)

AnticipateInterpolator 开始的时候向后然后向前甩
[![image](http://upload-images.jianshu.io/upload_images/9028834-bacba45cacbf6314?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-bacba45cacbf6314?imageMogr2/auto-orient/strip)

AnticipateOvershootInterpolator 开始的时候向后然后向前甩一定值后返回最后的值
[![image](http://upload-images.jianshu.io/upload_images/9028834-25f6a82370c5eba0?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-25f6a82370c5eba0?imageMogr2/auto-orient/strip)

BounceInterpolator 动画结束的时候弹起
[![image](http://upload-images.jianshu.io/upload_images/9028834-9e4fd6c029c49c72?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-9e4fd6c029c49c72?imageMogr2/auto-orient/strip)

CycleInterpolator 动画循环播放特定的次数，速率改变沿着正弦曲线
[![image](http://upload-images.jianshu.io/upload_images/9028834-f2ad8e5d65a346c2?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-f2ad8e5d65a346c2?imageMogr2/auto-orient/strip)

LinearInterpolator 以常量速率改变
[![image](http://upload-images.jianshu.io/upload_images/9028834-b888a91e4c1b7473?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-b888a91e4c1b7473?imageMogr2/auto-orient/strip)

OvershootInterpolator 向前甩一定值后再回到原来位置
[![image](http://upload-images.jianshu.io/upload_images/9028834-2075c3c8ed220462?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-2075c3c8ed220462?imageMogr2/auto-orient/strip)

### rotate标签

下面先看看rotate标签应用插值器后，都会变成什么样。

先看下XML代码：（从控件中心点，从0放大到1.4倍，保持结束时的状态）

```xml
<?xml version="1.0" encoding="utf-8"?>  
<rotate xmlns:android="http://schemas.android.com/apk/res/android"  
    android:interpolator="@android:anim/accelerate_decelerate_interpolator"  
    android:fromDegrees="0"  
    android:toDegrees="360"  
    android:pivotX="50%"  
    android:pivotY="50%"  
    android:duration="700"   
    android:fillAfter="true"  
/>  
```

AccelerateDecelerateInterpolator   在动画开始与结束的地方速率改变比较慢，在中间的时候加速
[![image](http://upload-images.jianshu.io/upload_images/9028834-c630f63c76cf169a?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-c630f63c76cf169a?imageMogr2/auto-orient/strip)

AccelerateInterpolator 在动画开始的地方速率改变比较慢，然后开始加速
[![image](http://upload-images.jianshu.io/upload_images/9028834-32145a2f95789380?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-32145a2f95789380?imageMogr2/auto-orient/strip)

DecelerateInterpolator 在动画开始的地方快然后慢
[![image](http://upload-images.jianshu.io/upload_images/9028834-4f6cd07f3fb3a96a?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-4f6cd07f3fb3a96a?imageMogr2/auto-orient/strip)

AnticipateInterpolator 开始的时候向后然后向前甩
[![image](http://upload-images.jianshu.io/upload_images/9028834-0a05e2e01a7ecc81?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-0a05e2e01a7ecc81?imageMogr2/auto-orient/strip)

AnticipateOvershootInterpolator 开始的时候向后然后向前甩一定值后返回最后的值
[![image](http://upload-images.jianshu.io/upload_images/9028834-44ba06378f706507?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-44ba06378f706507?imageMogr2/auto-orient/strip)

BounceInterpolator 动画结束的时候弹起
[![image](http://upload-images.jianshu.io/upload_images/9028834-e4c5e12a5012138e?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-e4c5e12a5012138e?imageMogr2/auto-orient/strip)
CycleInterpolator 动画循环播放特定的次数，速率改变沿着正弦曲线
[![image](http://upload-images.jianshu.io/upload_images/9028834-ca90a65293512544?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-ca90a65293512544?imageMogr2/auto-orient/strip)

LinearInterpolator 以常量速率改变
[![image](http://upload-images.jianshu.io/upload_images/9028834-a35e678f3f7894cf?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-a35e678f3f7894cf?imageMogr2/auto-orient/strip)

OvershootInterpolator 向前甩一定值后再回到原来位置
[![image](http://upload-images.jianshu.io/upload_images/9028834-cc7358a08cf91248?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-cc7358a08cf91248?imageMogr2/auto-orient/strip)

### alpha标签

下面先看看alpha标签应用插值器后，都会变成什么样。

将透明度从0变成1.0，使用不同的插值器看看有什么不同（因为只是透明度的变化，所以基本看不出来有什么不同）

```xml
<?xml version="1.0" encoding="utf-8"?>  
<alpha xmlns:android="http://schemas.android.com/apk/res/android"  
    android:interpolator="@android:anim/accelerate_decelerate_interpolator"  
    android:fromAlpha="0.0"  
    android:toAlpha="1.0"  
    android:duration="3000"   
    android:fillAfter="true"  
/>  
```

AccelerateDecelerateInterpolator   在动画开始与介绍的地方速率改变比较慢，在中间的时候加速
[![image](http://upload-images.jianshu.io/upload_images/9028834-c1e12d6347797678?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-c1e12d6347797678?imageMogr2/auto-orient/strip)

AccelerateInterpolator 在动画开始的地方速率改变比较慢，然后开始加速
[![image](http://upload-images.jianshu.io/upload_images/9028834-33502f51c99ea8cd?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-33502f51c99ea8cd?imageMogr2/auto-orient/strip)

DecelerateInterpolator 在动画开始的地方快然后慢
[![image](http://upload-images.jianshu.io/upload_images/9028834-f70e63b02aa41b07?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-f70e63b02aa41b07?imageMogr2/auto-orient/strip)

AnticipateInterpolator 开始的时候向后然后向前甩
[![image](http://upload-images.jianshu.io/upload_images/9028834-ea298210d5a74356?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-ea298210d5a74356?imageMogr2/auto-orient/strip)

AnticipateOvershootInterpolator 开始的时候向后然后向前甩一定值后返回最后的值
[![image](http://upload-images.jianshu.io/upload_images/9028834-dd9aeae1d00ed4ce?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-dd9aeae1d00ed4ce?imageMogr2/auto-orient/strip)

BounceInterpolator 动画结束的时候弹起
[![image](http://upload-images.jianshu.io/upload_images/9028834-aa33ed3dd58ae787?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-aa33ed3dd58ae787?imageMogr2/auto-orient/strip)

CycleInterpolator 动画循环播放特定的次数，速率改变沿着正弦曲线
[![image](http://upload-images.jianshu.io/upload_images/9028834-84fc53abdca3eac7?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-84fc53abdca3eac7?imageMogr2/auto-orient/strip)

LinearInterpolator 以常量速率改变
[![image](http://upload-images.jianshu.io/upload_images/9028834-5e6d30c1d702d74c?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-5e6d30c1d702d74c?imageMogr2/auto-orient/strip)

OvershootInterpolator 向前甩一定值后再回到原来位置
[![image](http://upload-images.jianshu.io/upload_images/9028834-521ea1e6b27ecda8?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-521ea1e6b27ecda8?imageMogr2/auto-orient/strip)

### translate标签

下面先看看translate标签应用插值器后，都会变成什么样。

把控件从（0，0）平移到（-200，-200）的位置，保持结束时状态不变，使用不同插值器。

```xml
<?xml version="1.0" encoding="utf-8"?>  
<translate xmlns:android="http://schemas.android.com/apk/res/android"  
    android:interpolator="@android:anim/accelerate_decelerate_interpolator"  
    android:fromXDelta="0"     
    android:toXDelta="-200"    
    android:fromYDelta="0"    
    android:toYDelta="-200"    
    android:duration="2000"    
    android:fillAfter="true"  
/>  
```

AccelerateDecelerateInterpolator   在动画开始与介绍的地方速率改变比较慢，在中间的时候加速
[![image](http://upload-images.jianshu.io/upload_images/9028834-dba57e97857fae78?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-dba57e97857fae78?imageMogr2/auto-orient/strip)

AccelerateInterpolator 在动画开始的地方速率改变比较慢，然后开始加速
[![image](http://upload-images.jianshu.io/upload_images/9028834-3fa3107022b0da74?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-3fa3107022b0da74?imageMogr2/auto-orient/strip)

DecelerateInterpolator 在动画开始的地方快然后慢
[![image](http://upload-images.jianshu.io/upload_images/9028834-2611492836745934?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-2611492836745934?imageMogr2/auto-orient/strip)

AnticipateInterpolator 开始的时候向后然后向前甩
[![image](http://upload-images.jianshu.io/upload_images/9028834-2a5be60e84863285?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-2a5be60e84863285?imageMogr2/auto-orient/strip)

AnticipateOvershootInterpolator 开始的时候向后然后向前甩一定值后返回最后的值
[![image](http://upload-images.jianshu.io/upload_images/9028834-4563e39c9d0d23b0?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-4563e39c9d0d23b0?imageMogr2/auto-orient/strip)

BounceInterpolator 动画结束的时候弹起
[![image](http://upload-images.jianshu.io/upload_images/9028834-21758e835f26653a?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-21758e835f26653a?imageMogr2/auto-orient/strip)
CycleInterpolator 动画循环播放特定的次数，速率改变沿着正弦曲线
[![image](http://upload-images.jianshu.io/upload_images/9028834-32be87dac2c317ca?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-32be87dac2c317ca?imageMogr2/auto-orient/strip)

LinearInterpolator 以常量速率改变
[![image](http://upload-images.jianshu.io/upload_images/9028834-226b09b9bd1db962?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-226b09b9bd1db962?imageMogr2/auto-orient/strip)

OvershootInterpolator 向前甩一定值后再回到原来位置
[![image](http://upload-images.jianshu.io/upload_images/9028834-09ccea09569eabe4?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-09ccea09569eabe4?imageMogr2/auto-orient/strip)







## AccelerateDecelerateInterpolator

上文使用ValueAnimator所打印的值如下所示：
![](http://img.blog.csdn.net/20171221230833425?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

可以看到，一开始的值变化速度明显比较慢，仅0.0开头的就打印了4次，之后开始加速，最后阶段又开始减速，因此我们可以很明显地看出这一个先加速后减速的Interpolator。

上文中的小球也是：一开始运动速度比较慢，然后逐渐加速，中间的部分运动速度就比较快，接下来开始减速，最后缓缓停住。另外颜色变化也是这种规律，一开始颜色变化的比较慢，中间颜色变化的很快，最后阶段颜色变化的又比较慢。

从以上几点我们就可以总结出一个结论了，**使用属性动画时，系统默认的Interpolator其实就是一个先加速后减速的Interpolator，对应的实现类就是AccelerateDecelerateInterpolator**。

我们可以很轻松地修改这一默认属性，将它替换成任意一个系统内置好的Interpolator。
比如，【习题：对自定义View使用动画】中MyAnimView中的startAnimation()方法是开启动画效果的入口，这里我们对Point对象的坐标稍做一下修改，让它变成一种垂直掉落的效果，代码如下所示：
对Point构造函数中的坐标值进行了一下改动，那么现在小球运动的动画效果应该是从屏幕正中央的顶部掉落到底部。

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
    anim.setDuration(2000);  
    anim.start();  
}  
```

但是默认情况下小球的下降速度肯定是先加速后减速的，我们需要改为下降速度越来越快的。
调用**Animator的setInterpolator()**方法就可以了，这个方法要求**传入**一个实现**TimeInterpolator**接口的实例，那么比如说我们想要实现小球下降越来越快的效果，就可以使用AccelerateInterpolator，代码如下所示：

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
    anim.setInterpolator(new AccelerateInterpolator(2f));//实现小球下降越来越快  
    /*AccelerateInterpolator的构建函数可以接收一个float类型的参数，这个参数是用于控制加速度的*/
    anim.setDuration(2500);  
    anim.start();  
}  
```

效果如下：
![](http://img.blog.csdn.net/20171221231615443?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

## BounceInterpolator

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

## Interpolator内部实现机制

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

## 自定义Interpolator

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

[![](http://img.blog.csdn.net/20171221233909649?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)](http://img.blog.csdn.net/20171221233909649?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)



# 动画常见问题

## 修改 Activity 进入和退出动画

可以通过两种方式，一是通过定义 Activity 的主题，二是通过覆写 Activity 的 overridePendingTransition 方法。
方式1：通过**设置主题样式**
在 styles.xml 中编辑如下代码：

```xml
<style name="AnimationActivity" parent="@android:style/Animation.Activity">
<item name="android:activityOpenEnterAnimation">@anim/slide_in_left</item>
<item name="android:activityOpenExitAnimation">@anim/slide_out_left</item>
<item name="android:activityCloseEnterAnimation">@anim/slide_in_right</item>
<item name="android:activityCloseExitAnimation">@anim/slide_out_right</item>
</style>
```

添加 themes.xml 文件：

```xml
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
[自定义控件三部曲之动画篇（二）——Interpolator插值器](http://blog.csdn.net/harvic880925/article/details/40049763)
[自定义控件三部曲之动画篇(五)——ValueAnimator高级进阶（一）](http://blog.csdn.net/harvic880925/article/details/50546884)
[自定义控件三部曲之动画篇(六)——ValueAnimator高级进阶（二）](http://blog.csdn.net/harvic880925/article/details/50549385)
[自定义控件三部曲之动画篇(七)——ObjectAnimator基本使用](http://blog.csdn.net/harvic880925/article/details/50598322)












