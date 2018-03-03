# [简洁优雅地实现夜间模式](https://segmentfault.com/a/1190000007979581)

关键代码

1. 检测当前主题模式

   ```
   int mode = getResources().getConfiguration().uiMode & Configuration.UI_MODE_NIGHT_MASK;
   ```

2. 设置主题

   **AppCompatDelegate.setDefaultNightMode**(AppCompatDelegate.MODE_NIGHT_NO)

   ```
   if(mode == Configuration.UI_MODE_NIGHT_YES) {
       AppCompatDelegate.setDefaultNightMode(AppCompatDelegate.MODE_NIGHT_NO);
   } else if(mode == Configuration.UI_MODE_NIGHT_NO) {
       AppCompatDelegate.setDefaultNightMode(AppCompatDelegate.MODE_NIGHT_YES);
   } else {
       // blah blah
   }

   recreate();
   ```





### 前言

Android 6.0 Marshmallow 预览版中曾经短暂出现过相关的夜间模式的功能，只是在正式版中被移除了，在Android 7.0 Nougat上，用户们再次经历了「得而复失」的遗憾，在开发者预览版中，夜间模式和暗色模式先是开启，然后有再次被移除。而在正式版中，夜间模式也没有出现。但其实相关的代码一直存在于系统中，只是默认没有被开启。如何开启这项功能，可以参考少数派的这一篇文章，[帮你找回 Android 7.0 夜间模式的 2 款应用](http://sspai.com/35273)。

不过，今天要介绍的主要内容并不是关于系统的夜间模式，而是如何给我们开发的APP添加夜间模式的功能。毫不夸张的说，夜间模式现在已经是阅读类App的标配了。事实上，日间模式与夜间模式就是给APP定义并应用两套不同颜色的主题。用户可以自动或者手动的开启。我们先看两个我认为实现地很优雅的例子：知乎和Twitter。

![img](https://segmentfault.com/img/remote/1460000007979584?w=1280&h=720)

这两个APP在切换的工程中，并没有出现闪现黑屏的情况，切换也比较顺滑。我们的目标就是利用Support Library实现同样的效果。

### 实现

### 添加依赖

```
compile 'com.android.support:appcompat-v7:25.1.0'
```

由于Support Library在23.2.0的版本中才添加了Theme.AppCompat.DayNight主题，所以依赖的版本必须是高于23.2.0的，并且，这个特性支持的最低SDK版本为14，所以，需要兼容Android 4.0的设备，是不能使用这个特性的，在API Level 14以下的设备会默认使用亮色主题。不过现在4.0以下的设备应该比较少了吧，毕竟微信的minSdkVersion都设置为14了。

### 准备资源

1. 让我们自己的主题继承并应用DayNight主题。

   ```
   <style name="AppTheme" parent="Theme.AppCompat.DayNight.DarkActionBar">

           <item name="colorPrimary">@color/colorPrimary</item>
           <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
           <item name="colorAccent">@color/colorAccent</item>
           <!--customize your theme here-->
           
   </style>
   ```

2. 新建夜间模式资源文件夹:在`res`目录下新建`values-night`文件夹，然后在此目录下新建`colors.xml`文件在夜间模式下的应用的资源。当然也可以根据需要新建`drawable-night`,`layout-night`等后缀为`-night`的夜间资源文件夹。
   我的`values`和`values-night`目录下的`colors.xml`的内容如下:

```
<?xml version="1.0" encoding="utf-8"?>
<!--values-colors.xml-->
<resources>
    <color name="colorPrimary">#009688</color>
    <color name="colorPrimaryDark">#00796B</color>
    <color name="colorAccent">#009688</color>
    <color name="textColorPrimary">#616161</color>
    <color name="viewBackground">@android:color/white</color>
</resources>
```

```
<?xml version="1.0" encoding="utf-8"?>
<!--values-night-colors.xml>
<resources>
    <color name="colorPrimary">#35464e</color>
    <color name="colorPrimaryDark">#212a2f</color>
    <color name="colorAccent">#212a2f</color>
    <color name="textColorPrimary">#616161</color>
    <color name="viewBackground">#212a2f</color>
</resources>
```

1. 使Activity继承自AppCompatActivity。

   ```
   public class MainActivity extends AppCompatActivity {
       
       // code here
       @Override
       protected void onCreate(Bundle savedInstanceState) {
           
       }
       
   }
   ```

### 应用

#### 静态应用

在Application的继承类下设置初始主题。

```
public class App extends Application {

    @Override
    public void onCreate() {
        super.onCreate();
        
        AppCompatDelegate.setDefaultNightMode(AppCompatDelegate.MODE_NIGHT_YES);
        // other code here

}
```

这里`AppCompatDelegate.setDefaultNightMode()`方法可以接受的参数值有4个：

- MODE_NIGHT_NO. Always use the day (light) theme(一直应用日间(light)主题).
- MODE_NIGHT_YES. Always use the night (dark) theme(一直使用夜间(dark)主题).
- MODE_NIGHT_AUTO. Changes between day/night based on the time of day(根据当前时间在day/night主题间切换).
- MODE_NIGHT_FOLLOW_SYSTEM(默认选项). This setting follows the system’s setting, which is essentially MODE_NIGHT_NO(跟随系统，通常为MODE_NIGHT_NO).

我们可以在任何时候调用这个方法，因为这个方法是静态的。但是这个值并不是一直存在的，每次在开启进程时需要重新设置。在上面的代码中，我是在`onCreate()`方法中设置的，网上也有大神建议在Activity或者Application的**static**代码块中设置。如下所示：

```
public class App extends Application {

    static {
        AppCompatDelegate.setDefaultNightMode(AppCompatDelegate.MODE_NIGHT_YES);
    }

    @Override
    public void onCreate() {
        super.onCreate();
        
        // other code here

}
```

#### 动态应用

虽然上面的静态应用的设置非常简单，但是这种方式的应用场景还是太少了。我们更多的还是需要动态的根据需要动态的切换。

1. 检测当前主题模式

   ```
   int mode = getResources().getConfiguration().uiMode & Configuration.UI_MODE_NIGHT_MASK;
   ```

2. 设置主题

   ```
   if(mode == Configuration.UI_MODE_NIGHT_YES) {
       AppCompatDelegate.setDefaultNightMode(AppCompatDelegate.MODE_NIGHT_NO);
   } else if(mode == Configuration.UI_MODE_NIGHT_NO) {
       AppCompatDelegate.setDefaultNightMode(AppCompatDelegate.MODE_NIGHT_YES);
   } else {
       // blah blah
   }

   recreate();
   ```

```
在调用`recreate()`方法之前，还可以创建一些动画进行过渡。而且，众所周知，调用`recreate()`需要对一些数据进行保存，例如fragment，CheckBox,RadioBox等。如下所示:

```

```
public class MainFragment extends Fragment {
    
    @Override
    public void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
    
         if (savedInstanceState != null) {
             FragmentManager manager = getChildFragmentManager();
            doubanMomentFragment = (DoubanMomentFragment) manager.getFragment(savedInstanceState, "douban");
         } else {
             doubanMomentFragment = DoubanMomentFragment.newInstance();
         }
    }
    
    @Override
    public void onSaveInstanceState(Bundle outState) {
        super.onSaveInstanceState(outState);
        FragmentManager manager = getChildFragmentManager();
        manager.putFragment(outState, "douban", doubanMomentFragment);
}
```

```
我们也可以把主题的值存储到SharedPreference中，已便于应用在下一次启动时自动应用主题。

```

#### Q&A

- Q:系统默认的颜色不合我的口味怎么办？

A:使用主题属性,例如:`textColor:?android:attr/textColorPrimary`,`color:?attr/colorControlNormal`等。

- Q:为什么我的WebView颜色没有变化？

A:因为WebView不能使用主题属性。WebView的颜色实际上取决于网页内容颜色。网页的默认背景色是白色，所以尽管设置了主题为`AppCompatDelegate.MODE_NIGHT_YES `，网页仍然是白色，所以看起来就很不搭了。所以，网页的内容和背景色等资源也需要适配了。

- Q:为什么不直接设置为`MODE_NIGHT_AUTO `呢？

A:因为使用`MODE_NIGHT_AUTO `需要请求坐标权限，获取系统的位置。你肯定会说了，这尼玛不是坑爹吗？如果程序已经授予了坐标权限(location permission)(如果你的target SDK为23或者更高，需要考虑运行时权限)，AppCompat会试着去获取上次保存的坐标，根据坐标来计算日出与日落的时间。如果程序没有位置权限或者LocationManager没有存储上次坐标的信息呢？系统或默认设置为早上6点钟为日出，下午10点为日落。用户调整系统时间，当前的主题也会随之改变。如果我们不希望用户在设定主题后，主题还会随着时间改变,`MODE_NIGHT_AUTO`就不适用了。

### 代码

本项目代码[PaperPlane](https://github.com/marktony/PaperPlane) .
运行效果

![img](https://segmentfault.com/img/remote/1460000007979585?w=1280&h=720)

在Android 6.0及以下的设备上，本项目运行时会有切换的过渡动画效果，但是不支持Android 7.0及以上的设备。