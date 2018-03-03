[如何用一周时间开发一款Android APP并在Google Play上线](https://tonnyl.github.io/2017/02/08/Develop-an-Android-App-and-publish-it-on-Google-Play-in-a-week/)

目标：实现[纸飞机](https://github.com/TonnyL/PaperPlane)App - 采用MVP架构，集合了知乎日报、果壳精选和豆瓣一刻的综合性阅读客户端。效果图如下所示：

![PaperPlane](http://upload-images.jianshu.io/upload_images/2440049-75f9c938a87c3c46.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

本次教程分为7天，内容分别为：

- 第一天，准备
  - 功能需求
  - 可行性分析
  - 其他准备
- 第二天，UI
  - 选择合适的UI
- 第三天，整体架构
- 第四天，首页列表
  - 界面编写
  - 实体类
  - 显示数据
  - 缓存内容
- 第五天，详情页与其他
  - 界面编写
  - 实体类
  - 显示数据
  - 设置与关于
- 第六天，高级功能
  - 夜间模式
  - 版本适配
- 第七天，发布与开源
  - 在Google Play上线
  - 在GitHub开源
  - 思考

好了，废话不多说了。现在就开始吧。

### DAY 1

俗话说，万事开头难，准备工作做好了，可以起到事半功倍的作用。磨刀不误砍柴工嘛。

#### Day 1,功能需求

在开始正式编码之前，咱们还是得先把要实现的功能一一列出来，后面实现起来才有方向嘛。我认为咱们需要实现的功能有：

- 正确获取消息列表并展示
- 能够获取历史消息
- 展示内容详情
- 后台自动缓存内容详情，方便用户在无网络连接时查看
- 收藏特定消息
- 夜间模式

一共6个大的需求，不多，但是我们仔细的研究一下，实际上这6个需求涉及到了网络，UI，数据存储，后台服务等内容。相信对于聪明的你不算困难，现在我们来研究一下可行性。

#### Day 1，可行性分析

我们首先需要考虑的问题就是：**数据从哪里来？**感谢开源，GitHub上[izzyleung](https://github.com/izzyleung)大神分析了知乎日报的API并开源了，项目地址请戳这里：[知乎日报 API 分析](https://github.com/izzyleung/ZhihuDailyPurify/wiki/%E7%9F%A5%E4%B9%8E%E6%97%A5%E6%8A%A5-API-%E5%88%86%E6%9E%90)，分析的非常详细，纸飞机项目在初期，也就是版本3.0之前也只使用了这一个API，在3.0之后还使用果壳精选和豆瓣一刻的API。如果你还想要展示更多的内容，可以戳这里：[Awsome_API](https://github.com/TonnyL/Awesome_APIs)，收集了一些国内外常用的API。

我们来粗略的看一下数据的内容。获取知乎日报2017年1月22日的消息列表：

```
http://news-at.zhihu.com/api/4/news/before/20170122
```

服务器向我们返回JSON格式的内容：

```
{
  "date": "20170121",
  "stories": [
    {
      "images": [
        "http://pic1.zhimg.com/ffcca2b2853f2af791310e6a6d694e80.jpg"
      ],
      "type": 0,
      "id": 9165434,
      "ga_prefix": "012121",
      "title": "谁说普通人的生活就不能精彩有趣呢？"
    },
    ...
    ]
}
```

OK，获取到了列表之后，我们就可以获取详细的内容了，例如，我们获取id为9165434的内容，只需要将id拼接到`http://news-at.zhihu.com/api/4/news/`之后：

```
http://news-at.zhihu.com/api/4/news/9165434
```

获取到的内容为：

```
{
  "body": "html格式的内容",
  "image_source": "《帕特森》",
  "title": "谁说普通人的生活就不能精彩有趣呢？",
  "image": "http://pic4.zhimg.com/e39083107b7324c6dbb725da83b1d7fb.jpg",
  "share_url": "http://daily.zhihu.com/story/9165434",
  "js": [],
  "ga_prefix": "012121",
  "section": {
    "thumbnail": "http://pic1.zhimg.com/ffcca2b2853f2af791310e6a6d694e80.jpg",
    "id": 28,
    "name": "放映机"
  },
  "images": [
    "http://pic1.zhimg.com/ffcca2b2853f2af791310e6a6d694e80.jpg"
  ],
  "type": 0,
  "id": 9165434,
  "css": [
    "http://news-at.zhihu.com/css/news_qa.auto.css?v=4b3e3"
  ]
}
```

`body`字段中就是html格式的内容详情，我们就可以使用WebView来展示了。当然，知乎日报的API接口不止上面的两个，你可以点击上面的链接查看。获取果壳精选和豆瓣一刻的内容，你可以在我的项目中直接查看文件[Api](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/util/Api.java)。

#### Day 1，其他准备

工欲善其事，必先利其器。工具准备好总是没错的。

- 一台电脑 这个怎么说呢，没有这个的话，要进行开发工作还是很难的，咱们总不能用石器写代码吧。
- 软件：
  - [Android Studio](https://developer.android.com/studio/index.html) 标配
  - [Chrome](https://www.google.com/chrome/) 程序员用360浏览器，百度浏览器什么的总觉得有点不够GEEK。
  - [Postman](https://www.getpostman.com/docs/introduction) 一款功能强大的网页调试与发送网页HTTP请求的Chrome插件，我们做网络请求分析时需要用到。
  - [Genymotion](https://www.genymotion.com/) 如果你嫌AS自带的模拟器慢的话，可以试试这个。
  - [Git](https://git-scm.com/) 版本控制，命令行敲起来炒鸡带感哦。
- 最好是能有一台Android手机。
- 科学上网，确保能够正常访问Google和StackOverFlow。让百度去死吧。

好了，第一天的工作差不多就是这么多，熟悉一下API，把工具备好，收拾一下心情，准备明天的工作。

### DAY 2 UI设计

今天主要完成的是UI设计。你可能会问了，这不是设计师的工作么。然而，我在开发纸飞机的过程中，并没有射鸡湿这种生物，UI就我自己完成了。相信大多数的程序员，美术方面应该不是那么地擅长。

当然，有美术和相关基础的同学可以试试用Sketch或者PS把原型图画出来，对于没有美术基础的童鞋，最简单的方法当然就是模仿现成的APP了。当然，你也可以在下列网站寻找合适的设计图：

- [Dribbble](https://dribbble.com/)
- [UpLabs](https://www.uplabs.com/)
- [UI中国](http://www.ui.cn/)
- [站酷ZCOOL](http://www.zcool.com.cn/)

另外，还有一些小的注意事项：

- 遵守[Material Design设计规范](https://material.io/guidelines/) - 这不是强制性的要求，但是，既然我们是开发一款Android App，如果我们自己都不遵守规范，还怎么指望Android环境变好呢。
- **正确使用BottomNavigation** - BottomNavigation作为Google的打脸之作，诞生之初就倍受争议。我个人的建议是使用TabLayout代替底部导航，这是涉及到信仰的大事情。如果一定要用，请不要把iOS上的标准直接放在Android上使用，请参考这一篇文章:[Material Design 中的 Bottom Navigation 并不是无脑移植 iOS 导航模式的许可证](https://zhuanlan.zhihu.com/p/22005972),并且，我向你投来一个鄙视的眼神。
- 使用正确的图标 - 尽量使用 <https://material.io/icons/> 网站上的图标，如果你使用iOS版本的图标，我再次向你投来一个鄙视的眼神。

纸飞机的最终设计效果如下：

![PaperPlane](http://upload-images.jianshu.io/upload_images/2440049-75f9c938a87c3c46.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

首页使用Drawer作为顶级导航，Tab为二级导航，列表项使用卡牌布局，使用FloatingActionButton作为日期选择按钮；详情页面使用可收缩的Toolbar，图片搭配文字的形式。其他高深的我也不懂了。(到后面你会发现，这里我犯了一个错误，卡牌布局用在这里是不合适的。参见：<https://material.io/guidelines/components/cards.html#cards-usage>)

### DAY 3 创建项目

现在开始就要真正的写代码了。

新建Android Studio项目什么的就不说了，下面的是我的项目结构图：

![项目结构](https://ww2.sinaimg.cn/large/006y8lVagy1fcfh31l5g1j30ni14qdk8.jpg)

```
·
├── app
|   ├── libs 存放相关的jar文件等
|   ├── src
|   |   ├── androidTest 测试相关目录
|   |   ├── main
|   |   |   ├── assets 存放资源原文件
|   |   |   ├── java
|   |   |   |   ├── com.marktony.zhihudaily java包
|   |   |   |   |   ├── about 关于页面
|   |   |   |   |   ├── adapter RecyclerView与ViewPager等控件的Adapter
|   |   |   |   |   ├── app Application
|   |   |   |   |   ├── bean 存放实体类
|   |   |   |   |   ├── bookmarks 收藏页面
|   |   |   |   |   ├── customtabs Chrome Custom Tabs相关
|   |   |   |   |   ├── db 数据库相关
|   |   |   |   |   ├── detail 详细内容页面
|   |   |   |   |   ├── homepage 首页页面
|   |   |   |   |   ├── innerbrowser 内置浏览器页面
|   |   |   |   |   ├── interfaze 接口集合
|   |   |   |   |   ├── license 开源许可证页面
|   |   |   |   |   ├── search 搜索页面
|   |   |   |   |   ├── service Service集合
|   |   |   |   |   ├── settings 设置页面
|   |   |   |   |   ├── util 工具类集合
|   |   |   |   |   ├── BasePresenter.java Presenter基类
|   |   |   |   |   ├── BaseView.java View基类
|   |   |   ├── res
|   |   |   ├── AndroidManifest.xml 清单文件
```

(不难看出，我是按照页面和功能进行分包的。)

包建立完成后，我们开始导入第三方的开源库，便于简化代码的编写和实现特定的效果。找到工程目录下app文件夹，打开`build.gradle`文件，添加如下内容。

```
dependencies {
    compile fileTree(include: ['*.jar'], dir: 'libs')
    // 使用volley简化网络请求
    compile files('libs/library-1.0.19.jar')
    // appcompat兼容包
    compile 'com.android.support:appcompat-v7:25.1.0'
    // material design 设计包
    compile 'com.android.support:design:25.1.0'
    // recycler view控件
    compile 'com.android.support:recyclerview-v7:25.1.0'
    // preference screen 设置和关于页面的配置
    compile 'com.android.support:preference-v14:25.1.0'
    // 支持Chrome Custom Tabs
    compile 'com.android.support:customtabs:25.1.0'
    // card view 控件
    compile 'com.android.support:cardview-v7:25.1.0'
    // 解析JSON数据
    compile 'com.google.code.gson:gson:2.7'
    // 图片加载
    compile 'com.github.bumptech.glide:glide:3.7.0'
    // 为了保持在低版本SDK中的UI一致性，引入material data time picker库
    compile 'com.wdullaer:materialdatetimepicker:2.5.0'
    testCompile 'junit:junit:4.12'
```

由于一些历史遗留问题，我并没有使用OkHttp作为网络请求包，而是选择了volley。如果你有一定的基础，可以选择使用OkHttp。

导入volley有两种方式：

- 在`app`目录下的`lib`目录下粘贴volley的jar包，你可以在这里下载到：[Volley](https://github.com/TonnyL/PaperPlane/blob/master/app/libs/library-1.0.19.jar)。
- 当然也可以通过gradle引入。

```
compile 'com.android.volley:volley:1.0.0'
```

然后点击Sync Project with Gradle files。

首先是整体的架构：MVP。关于整体架构的选择以及更加详细的介绍部分，可以戳这篇文章:[重构！将Google-MVP应用于已有项目](https://tonnyl.github.io/2016/09/27/%E9%87%8D%E6%9E%84%EF%BC%81%E5%B0%86Google-MVP%E5%BA%94%E7%94%A8%E4%BA%8E%E5%B7%B2%E6%9C%89%E9%A1%B9%E7%9B%AE/)。这里我们仿照Google的[Android Architecture Blueprints [beta\]](https://github.com/googlesamples/android-architecture)中的[todo-mvp](https://github.com/googlesamples/android-architecture/tree/todo-mvp/)。

1. 首先创建最基本的BaseView和BasePresenter,他们分别是所有View和Presenter的基类。

   [Baseview.java](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/BaseView.java)

   ```
   public interface BaseView<T> {
   	// 为View设置Presenter
       void setPresenter(T presenter);
      // 初始化界面控件
       void initViews(View view);
   }
   ```

[BasePresenter.java](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/BasePresenter.java)

```
public interface BasePresenter {
	// 获取数据并改变界面显示，在todo-mvp的项目中的调用时机为Fragment的OnResume()方法中
    void start();
}
```

1. 然后创建一个契约类，用于同一管理View和Presenter。这里以知乎日报的部分为例(如果没有特别说明，后面的代码均以知乎日报的部分为例，果壳精选与豆瓣一刻的代码类似，详细代码可以在GitHub的repo中找到)。

   [ZhihuDailyContract.java](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/homepage/ZhihuDailyContract.java)

   ```
   public interface ZhihuDailyContract {

       interface View extends BaseView<Presenter> {

   		// 显示加载或其他类型的错误
           void showError();
   		// 显示正在加载
           void showLoading();
   		// 停止显示正在加载
           void stopLoading();
   		// 成功获取到数据后，在界面中显示
           void showResults(ArrayList<ZhihuDailyNews.Question> list);
   		// 显示用于加载指定日期的date picker dialog
           void showPickDialog();

       }

       interface Presenter extends BasePresenter {
   		// 请求数据
           void loadPosts(long date, boolean clearing);
   		// 刷新数据
           void refresh();
   		// 加载更多文章
           void loadMore(long date);
   		// 显示详情
           void startReading(int position);
   		// 随便看看
           void feelLucky();

       }

   }
   ```

2. 在上面已经分好的子包中，创建相应的子类View和Presenter。

   [ZhihuDailyFragment.java](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/homepage/ZhihuDailyFragment.java)

   ```
   public class ZhihuDailyFragment extends Fragment
       implements ZhihuDailyContract.View {
       
       public ZhihuDailyFragment() {}
   	
       public static ZhihuDailyFragment newInstance() {
           return new ZhihuDailyFragment();
       }
   	
       
       public void onCreate(Bundle savedInstanceState) {
       	super.onCreate(savedInstanceState);
   	}
   	
   	@Nullable
       @Override
       public View onCreateView(LayoutInflater inflater, ViewGroup container, Bundle savedInstanceState) {
           return null;
   	}
   	
   	@Override
       public void setPresenter(ZhihuDailyContract.Presenter presenter) {
           
       }
       
       @Override
       public void initViews(View view) {
   	
       }
       
   	@Override
       public void showError() {
          
       }
   	
       @Override
       public void showLoading() {
          
       }
   	
       @Override
       public void stopLoading() {
           
       }
   	
       @Override
       public void showResults(ArrayList<ZhihuDailyNews.Question> list) {
   	
       }
   	
       @Override
       public void showPickDialog() {
   	
       }
   	
   }
   ```

`[ZhihuDailyPresenter.java](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/homepage/ZhihuDailyPresenter.java)public class ZhihuDailyPresenter implements ZhihuDailyContract.Presenter {			public ZhihuDailyPresenter(Context context, ZhihuDailyContract.View view) {	}		@Override    public void loadPosts(long date, final boolean clearing) {	    }        @Override    public void refresh() {            }    @Override    public void loadMore(long date) {           }    @Override    public void startReading(int position) {	    }    @Override    public void feelLucky() {           }    @Override    public void start() {           }		}然后完成果壳精选页面，豆瓣一刻的内容，就可以进行下面的工作了。`

1. 创建VolleySingleton，即Volley的单例。这样，整个应用就可以只维护一个请求队列，加入新的网络请求也会更加的方便。

   [VolleySingleton.java](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/app/VolleySingleton.java)

   ```
   	public class VolleySingleton {

       private static VolleySingleton volleySingleton;
       private RequestQueue requestQueue;

       private VolleySingleton(Context context){
           requestQueue = Volley.newRequestQueue(context.getApplicationContext());
       }

       public static synchronized VolleySingleton getVolleySingleton(Context context){
           if(volleySingleton == null){
               volleySingleton = new VolleySingleton(context);
           }
           return volleySingleton;
       }

       public RequestQueue getRequestQueue(){
           return this.requestQueue;
       }

       public <T> void addToRequestQueue(Request<T> req){
           getRequestQueue().add(req);
       }

   }
   ```

2. 然后是Model层的实现。使用了Gson之后，对JSON的转换更加方便了，所以，我们只需要返回类型为String即可。

   [OnStringListener.java](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/interfaze/OnStringListener.java)

   ```
   public interface OnStringListener {
       /**
        * 请求成功时回调
        * @param result
        */
       void onSuccess(String result);
       /**
        * 请求失败时回调
        * @param error
        */
       void onError(VolleyError error);
   }
   ```

   定义了两个方法，分别为请求成功时和请求失败时的回调。

   然后定义一个StringModel的实现类–StringModelImpl。

   [StringModelImpl.java](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/bean/StringModelImpl.java)

   ```
   public class StringModelImpl {
       private Context context;
       public StringModelImpl(Context context) {
           this.context = context;
       }
       public void load(String url, final OnStringListener listener) {
           StringRequest request = new StringRequest(url, new Response.Listener<String>() {
               
   @Override
               public void onResponse(String s) {
                   listener.onSuccess(s);
               }
           }, new Response.ErrorListener() {
               
   @Override
               public void onErrorResponse(VolleyError volleyError) {
                   listener.onError(volleyError);
               }
           });
           VolleySingleton.getVolleySingleton(context).addToRequestQueue(request);
       }
   }
   ```

3. 到这里，基本的架构就搭建完成了。现在可以喝杯咖啡，然后完成今天的最后一点工作，为后面的工作做准备。

   创建`Api.java`文件，用于存储app所用到的所有API。

   [Api.java](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/util/Api.java)

   ```
   public class Api {

   	// 消息内容获取与离线下载
   	// 在最新消息中获取到的id，拼接到这个NEWS之后，可以获得对应的JSON格式的内容
   	public static final String ZHIHU_NEWS = "http://news-at.zhihu.com/api/4/news/";
   	
   	// 过往消息
   	// 若要查询的11月18日的消息，before后面的数字应该为20161118
      	// 知乎日报的生日为2013 年 5 月 19 日，如果before后面的数字小于20130520，那么只能获取到空消息
   	public static final String ZHIHU_HISTORY = "http://news.at.zhihu.com/api/4/news/before/";
   	
   	// 获取果壳精选的文章列表,通过组合相应的参数成为完整的url
      	public static final String GUOKR_ARTICLES = "http://apis.guokr.com/handpick/article.json?retrieve_type=by_since&category=all&limit=25&ad=1";

      	// 获取果壳文章的具体信息 V1
       public static final String GUOKR_ARTICLE_LINK_V1 = "http://jingxuan.guokr.com/pick/";

   	// 豆瓣一刻
       // 根据日期查询消息列表
       public static final String DOUBAN_MOMENT = "https://moment.douban.com/api/stream/date/";

       // 获取文章具体内容
       public static final String DOUBAN_ARTICLE_DETAIL = "https://moment.douban.com/api/post/";

   }
   ```

`创建`NetworkState.java`文件，判断当前的网络状态，是否有网络连接，WiFi或者是移动数据。[NetworkState.java](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/util/NetworkState.java)public class NetworkState {    // 检查是否连接到网络    public static boolean networkConnected(Context context){        if (context != null){            ConnectivityManager manager = (ConnectivityManager) context.getSystemService(Context.CONNECTIVITY_SERVICE);            NetworkInfo info = manager.getActiveNetworkInfo();            if (info != null)                return info.isAvailable();        }        return false;    }    // 检查WiFi是否连接    public static boolean wifiConnected(Context context){        if (context != null){            ConnectivityManager manager = (ConnectivityManager) context.getSystemService(Context.CONNECTIVITY_SERVICE);            NetworkInfo info = manager.getActiveNetworkInfo();            if (info != null){                if (info.getType() == ConnectivityManager.TYPE_WIFI)                    return info.isAvailable();            }        }        return false;    }    // 检查移动网络是否连接    public static boolean mobileDataConnected(Context context){        if (context != null){            ConnectivityManager manager = (ConnectivityManager) context.getSystemService(Context.CONNECTIVITY_SERVICE);            NetworkInfo info = manager.getActiveNetworkInfo();            if (info != null){                if (info.getType() == ConnectivityManager.TYPE_MOBILE)                    return true;            }        }        return false;    }}创建`DateFormatter .java`文件，方便将`long`类型的日期转换为`String`类型。[DateFormatter.java](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/util/DateFormatter.java)public class DateFormatter {    /**     * 将long类date转换为String类型     * @param date date     * @return String date     */    public String ZhihuDailyDateFormat(long date) {        String sDate;        Date d = new Date(date + 24*60*60*1000);        SimpleDateFormat format = new SimpleDateFormat("yyyyMMdd");        sDate = format.format(d);        return sDate;    }    public String DoubanDateFormat(long date){        String sDate;        Date d = new Date(date);        SimpleDateFormat format = new SimpleDateFormat("yyyy-MM-dd");        sDate = format.format(d);        return sDate;    }}OK，day 3工作完成。`

### Day 4

今天的只要任务是完成首页。

#### Day 4，界面编写

我们的首页，使用的是Activity + Fragment搭配的方式，即一个MainActivity + MainFragment + BookmarksFragment的方式。其中，MainActivity的布局文件中包含了DrawerLayout, Toolbar以及Fragment所在的容器。

MainActivity对应布局文件如下:

[activity_main.xml](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/res/layout/activity_main.xml)

```
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.widget.DrawerLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/drawer_layout"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true"
    tools:openDrawer="start">

    <include layout="@layout/app_bar_main" />

    <android.support.design.widget.NavigationView
        android:id="@+id/nav_view"
        android:layout_width="wrap_content"
        android:layout_height="match_parent"
        android:layout_gravity="start"
        android:fitsSystemWindows="true"
        app:headerLayout="@layout/nav_header_main"
        app:menu="@menu/activity_main_drawer" />

</android.support.v4.widget.DrawerLayout>
```

[nav_header_main.xml](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/res/layout/nav_header_main.xml)

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="@dimen/nav_header_height"
    android:background="@drawable/nav_header"
    android:gravity="bottom"
    android:orientation="vertical"
    android:theme="@style/ThemeOverlay.AppCompat.Dark">

</LinearLayout>
```

nav_header实际上就只是一个简单的ImageView。

[app_bar_main.xml](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/res/layout/app_bar_main.xml)

```
<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.CoordinatorLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true"
    tools:context=".homepage.MainActivity">

    <android.support.design.widget.AppBarLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:elevation="0dp"
        android:theme="@style/AppTheme.AppBarOverlay">

        <android.support.v7.widget.Toolbar
            android:id="@+id/toolbar"
            android:layout_width="match_parent"
            android:layout_height="?attr/actionBarSize"
            android:background="@color/colorPrimary"
            app:popupTheme="@style/AppTheme.PopupOverlay" />

    </android.support.design.widget.AppBarLayout>

    <FrameLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:id="@+id/layout_fragment"
        android:layout_marginTop="?actionBarSize"/>

</android.support.design.widget.CoordinatorLayout>
```

OK，Activity的布局文件完成。然后就可以写java代码了。

[MainActivity.java](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/homepage/MainActivity.java)

```
public class MainActivity extends AppCompatActivity
        implements NavigationView.OnNavigationItemSelectedListener{

    private MainFragment mainFragment;
    private BookmarksFragment bookmarksFragment;

    private NavigationView navigationView;
    private DrawerLayout drawer;
    private Toolbar toolbar;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

		// 初始化控件
        initViews();

		// 恢复fragment的状态
        if (savedInstanceState != null) {
            mainFragment = (MainFragment) getSupportFragmentManager().getFragment(savedInstanceState, "MainFragment");
            bookmarksFragment = (BookmarksFragment) getSupportFragmentManager().getFragment(savedInstanceState, "BookmarksFragment");
        } else {
            mainFragment = MainFragment.newInstance();
            bookmarksFragment = BookmarksFragment.newInstance();
        }

        if (!mainFragment.isAdded()) {
            getSupportFragmentManager().beginTransaction()
                    .add(R.id.layout_fragment, mainFragment, "MainFragment")
                    .commit();
        }

        if (!bookmarksFragment.isAdded()) {
            getSupportFragmentManager().beginTransaction()
                    .add(R.id.layout_fragment, bookmarksFragment, "BookmarksFragment")
                    .commit();
        }
		
		// 实例化BookmarksPresenter
        new BookmarksPresenter(MainActivity.this, bookmarksFragment);

		// 默认显示首页内容
		showMainFragment();
		
    }

	// 初始化控件
    private void initViews() {

        toolbar = (Toolbar) findViewById(R.id.toolbar);
        setSupportActionBar(toolbar);

        drawer = (DrawerLayout) findViewById(R.id.drawer_layout);
        ActionBarDrawerToggle toggle = new ActionBarDrawerToggle(
                this,
                drawer,
                toolbar,
                R.string.navigation_drawer_open,
                R.string.navigation_drawer_close);
        drawer.setDrawerListener(toggle);
        toggle.syncState();

        navigationView = (NavigationView) findViewById(R.id.nav_view);
        navigationView.setNavigationItemSelectedListener(this);

    }

	// 显示MainFragment并设置Title
    private void showMainFragment() {

        FragmentTransaction fragmentTransaction = getSupportFragmentManager().beginTransaction();
        fragmentTransaction.show(mainFragment);
        fragmentTransaction.hide(bookmarksFragment);
        fragmentTransaction.commit();

        toolbar.setTitle(getResources().getString(R.string.app_name));

    }

	// 显示BookmarksFragment并设置Title
    private void showBookmarksFragment() {

        FragmentTransaction fragmentTransaction = getSupportFragmentManager().beginTransaction();
        fragmentTransaction.show(bookmarksFragment);
        fragmentTransaction.hide(mainFragment);
        fragmentTransaction.commit();

        toolbar.setTitle(getResources().getString(R.string.nav_bookmarks));

    }

    @Override
    public boolean onNavigationItemSelected(@NonNull MenuItem item) {

        drawer.closeDrawer(GravityCompat.START);

        int id = item.getItemId();
        if (id == R.id.nav_home) {
            showMainFragment();
        } else if (id == R.id.nav_bookmarks) {
            showBookmarksFragment();
        } else if (id == R.id.nav_change_theme) {

        } else if (id == R.id.nav_settings) {
            
        } else if (id == R.id.nav_about) {
            
        }

        return true;
    }

	// 存储Fragment的状态
    @Override
    protected void onSaveInstanceState(Bundle outState) {
        super.onSaveInstanceState(outState);
        if (mainFragment.isAdded()) {
            getSupportFragmentManager().putFragment(outState, "MainFragment", mainFragment);
        }

        if (bookmarksFragment.isAdded()) {
            getSupportFragmentManager().putFragment(outState, "BookmarksFragment", bookmarksFragment);
        }
    }

}
```

从代码中可以看出,MainActivity负责处理DrawerLayout的点击事件，即控制显示或者隐藏特定的Fragment。而Fragment的状态的保存与恢复也是在这里进行的。

[MainFragment.java](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/homepage/MainFragment.java)

```
public class MainFragment extends Fragment {

    private Context context;
    private MainPagerAdapter adapter;

    private TabLayout tabLayout;

    private ZhihuDailyFragment zhihuDailyFragment;
    private GuokrFragment guokrFragment;
    private DoubanMomentFragment doubanMomentFragment;

    private ZhihuDailyPresenter zhihuDailyPresenter;
    private GuokrPresenter guokrPresenter;
    private DoubanMomentPresenter doubanMomentPresenter;

    public MainFragment() {}

    public static MainFragment newInstance() {
        return new MainFragment();
    }

    @Override
    public void onAttach(Context context) {
        super.onAttach(context);
    }

    @Override
    public void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);

        this.context = getActivity();

		// Fragment状态恢复
        if (savedInstanceState != null) {
            FragmentManager manager = getChildFragmentManager();
            zhihuDailyFragment = (ZhihuDailyFragment) manager.getFragment(savedInstanceState, "zhihu");
            guokrFragment = (GuokrFragment) manager.getFragment(savedInstanceState, "guokr");
            doubanMomentFragment = (DoubanMomentFragment) manager.getFragment(savedInstanceState, "douban");
        } else {
        	// 创建View实例
            zhihuDailyFragment = ZhihuDailyFragment.newInstance();
            guokrFragment = GuokrFragment.newInstance();
            doubanMomentFragment = DoubanMomentFragment.newInstance();
        }

		// 创建Presenter实例
        zhihuDailyPresenter = new ZhihuDailyPresenter(context, zhihuDailyFragment);
        guokrPresenter = new GuokrPresenter(context, guokrFragment);
        doubanMomentPresenter = new DoubanMomentPresenter(context, doubanMomentFragment);

    }

    @Nullable
    @Override
    public View onCreateView(LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.fragment_main, container, false);

		// 初始化控件
        initViews(view);

		// 显示菜单
        setHasOptionsMenu(true);

        // 当tab layout位置为果壳精选时，隐藏fab
        tabLayout.addOnTabSelectedListener(new TabLayout.OnTabSelectedListener() {
            @Override
            public void onTabSelected(TabLayout.Tab tab) {
                FloatingActionButton fab = (FloatingActionButton) getActivity().findViewById(R.id.fab);
                if (tab.getPosition() == 1) {
                    fab.hide();
                } else {
                    fab.show();
                }

            }

            @Override
            public void onTabUnselected(TabLayout.Tab tab) {

            }

            @Override
            public void onTabReselected(TabLayout.Tab tab) {

            }

        });

        return view;
    }


	// 初始化控件
    private void initViews(View view) {

        tabLayout = (TabLayout) view.findViewById(R.id.tab_layout);
        ViewPager viewPager = (ViewPager) view.findViewById(R.id.view_pager);
        // 设置离线数为3
        viewPager.setOffscreenPageLimit(3);

        adapter = new MainPagerAdapter(
                getChildFragmentManager(),
                context,
                zhihuDailyFragment,
                guokrFragment,
                doubanMomentFragment);

        viewPager.setAdapter(adapter);
        tabLayout.setupWithViewPager(viewPager);

    }

    @Override
    public void onCreateOptionsMenu(Menu menu, MenuInflater inflater) {
        super.onCreateOptionsMenu(menu, inflater);
        inflater.inflate(R.menu.main, menu);
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        int id = item.getItemId();

        if (id == R.id.action_feel_lucky) {
            feelLucky();
        }
        return true;
    }

	// 保存状态
    @Override
    public void onSaveInstanceState(Bundle outState) {
        super.onSaveInstanceState(outState);
        FragmentManager manager = getChildFragmentManager();
        manager.putFragment(outState, "zhihu", zhihuDailyFragment);
        manager.putFragment(outState, "guokr", guokrFragment);
        manager.putFragment(outState, "douban", doubanMomentFragment);
    }

	// 随便看看
    public void feelLucky() {
        Random random = new Random();
        int type = random.nextInt(3);
        switch (type) {
            case 0:
                zhihuDailyPresenter.feelLucky();
                break;
            case 1:
                guokrPresenter.feelLucky();
                break;
            default:
                doubanMomentPresenter.feelLucky();
                break;
        }
    }

    public MainPagerAdapter getAdapter() {
        return adapter;
    }
}
```

首页的MainFragment主要负责显示与TabLayout + ViewPager相关的内容。

OK，终于把首页的UI框架搭建好了，喝杯咖啡，休息一下，冷静冷静。

现在开始实现具体的`ZhihuDailyFragment`的布局。仔细观察，实际上，ZhihuDailyFragment所包含的控件就只有一个`RecyclerView`，将获取到的内容以列表的形式显示出来。并且，不难发现，果壳精选与豆瓣一刻的布局与知乎日报的列表布局相同，可以复用。

[fragment_list.xml](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/res/layout/fragment_list.xml)

```
<?xml version="1.0" encoding="utf-8"?>
<android.support.v4.widget.SwipeRefreshLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:id="@+id/refreshLayout">

    <FrameLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:focusable="true"
        android:clickable="true">

        <android.support.v7.widget.RecyclerView
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:id="@+id/recyclerView"
            android:scrollbars="vertical"
            android:scrollbarFadeDuration="1"
            android:fadeScrollbars="true"/>

    </FrameLayout>

</android.support.v4.widget.SwipeRefreshLayout>
```

布局实际上还包含了SwipeRefreshLayout，用于显示正在加载和手动刷新。

列表子项的布局有很多种，分别是:

1. 普通仅文字
2. 普通文字 + 图片
3. 头部项，用于显示子项类型(如知乎日报，在收藏页面会用到)
4. 底部项，加载更多等

[home_list_item_without_image.xml](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/res/layout/home_list_item_without_image.xml) - 普通仅文字

```
<?xml version="1.0" encoding="utf-8"?>
<android.support.v7.widget.CardView xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_height="96dp"
    android:layout_width="match_parent"
    android:focusable="true"
    android:clickable="true"
    android:foreground="?android:attr/selectableItemBackground"
    app:cardCornerRadius="4dp"
    app:cardElevation="1dp"
    app:cardPreventCornerOverlap="true"
    android:layout_marginTop="8dp"
    android:layout_marginLeft="8dp"
    android:layout_marginRight="8dp">

    <TextView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:id="@+id/textViewTitle"
        android:paddingTop="8dp"
        android:paddingBottom="8dp"
        android:paddingLeft="8dp"
        android:paddingRight="8dp"
        android:gravity="center_vertical"
        android:maxLines="3"
        android:ellipsize="end"
        android:textSize="18sp" />

</android.support.v7.widget.CardView>
```

[home_list_item_layout.xml](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/res/layout/home_list_item_layout.xml) - 普通文字 + 图片

```
<?xml version="1.0" encoding="utf-8"?>
<android.support.v7.widget.CardView xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_height="96dp"
    android:layout_width="match_parent"
    android:focusable="true"
    android:clickable="true"
    android:foreground="?android:attr/selectableItemBackground"
    app:cardCornerRadius="4dp"
    app:cardElevation="1dp"
    app:cardPreventCornerOverlap="true"
    android:layout_marginTop="8dp"
    android:layout_marginLeft="8dp"
    android:layout_marginRight="8dp">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="horizontal"
        android:paddingLeft="8dp"
        android:paddingRight="8dp" >

        <TextView
            android:layout_width="0dp"
            android:layout_height="match_parent"
            android:layout_weight="1"
            android:id="@+id/textViewTitle"
            android:paddingTop="8dp"
            android:paddingBottom="8dp"
            android:layout_marginRight="8dp"
            android:layout_marginEnd="8dp"
            android:gravity="center_vertical"
            android:maxLines="3"
            android:ellipsize="end"
            android:textSize="18sp" />

        <ImageView
            android:layout_width="80dp"
            android:layout_height="80dp"
            android:id="@+id/imageViewCover"
            android:layout_gravity="center_vertical" />

    </LinearLayout>

</android.support.v7.widget.CardView>
```

[bookmark_header.xml](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/res/layout/bookmark_header.xml) - 头部项

```
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:id="@+id/textViewType"
    android:paddingLeft="8dp"
    android:paddingStart="8dp"
    android:paddingRight="8dp"
    android:paddingEnd="8dp"
    android:paddingTop="8dp"
    android:gravity="center_vertical"
    android:textColor="@color/colorPrimary"
    android:textAllCaps="true"/>
```

[list_footer.xml](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/res/layout/list_footer.xml) - 底部项，加载更多

```
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="48dp"
    android:layout_marginTop="8dp"
    android:layout_marginBottom="8dp"
    android:gravity="center_horizontal"
    android:background="@color/viewBackground">

    <android.support.v4.widget.ContentLoadingProgressBar
        android:id="@+id/address_looking_up"
        style="?android:attr/progressBarStyleInverse"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center_horizontal"
        android:visibility="visible" />

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="match_parent"
        android:text="@string/loading_more"
        android:layout_marginLeft="16dp"
        android:layout_marginStart="8dp"
        android:gravity="center_vertical"/>

</LinearLayout>
```

布局文件到这里基本就完成了。

#### Day 4，实体类

我们可以直接通过JSON格式的返回数据设计实体类。可以手动编写代码，也可以利用Android Studio插件[GsonFormat](https://github.com/zzz40500/GsonFormat)实现。

Json格式数据:

```
{
  "date": "20170121",
  "stories": [
    {
      "images": [
        "http://pic1.zhimg.com/ffcca2b2853f2af791310e6a6d694e80.jpg"
      ],
      "type": 0,
      "id": 9165434,
      "ga_prefix": "012121",
      "title": "谁说普通人的生活就不能精彩有趣呢？"
    },
    ...
    ]
}
```

对应的bean:[ZhihuDailyNews.java](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/bean/ZhihuDailyNews.java)

```
public class ZhihuDailyNews {

    private String date;
    private ArrayList<Question> stories;

    public String getDate() {
        return date;
    }

    public void setDate(String date) {
        this.date = date;
    }

    public ArrayList<Question> getStories() {
        return stories;
    }

    public void setStories(ArrayList<Question> stories) {
        this.stories = stories;
    }

    public class Question {

        private ArrayList<String> images;
        private int type;
        private int id;
        private String ga_prefix;
        private String title;

        public ArrayList<String> getImages() {
            return images;
        }

        public void setImages(ArrayList<String> images) {
            this.images = images;
        }

        public int getType() {
            return type;
        }

        public void setType(int type) {
            this.type = type;
        }

        public int getId() {
            return id;
        }

        public void setId(int id) {
            this.id = id;
        }

        public String getGa_prefix() {
            return ga_prefix;
        }

        public void setGa_prefix(String ga_prefix) {
            this.ga_prefix = ga_prefix;
        }

        public String getTitle() {
            return title;
        }

        public void setTitle(String title) {
            this.title = title;
        }

    }

}
```

#### Day 4，显示数据

首先，我们得有一个adapter。

[ZhihuDailyNewsAdapter.java](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/adapter/ZhihuDailyNewsAdapter.java)

```
public class ZhihuDailyNewsAdapter extends RecyclerView.Adapter<RecyclerView.ViewHolder> {

    private final Context context;
    private final LayoutInflater inflater;
    private List<ZhihuDailyNews.Question> list = new ArrayList<ZhihuDailyNews.Question>();
    private OnRecyclerViewOnClickListener mListener;

	// 文字 + 图片
    private static final int TYPE_NORMAL = 0;
    // footer，加载更多
    private static final int TYPE_FOOTER = 1;

    public ZhihuDailyNewsAdapter(Context context, List<ZhihuDailyNews.Question> list){
        this.context = context;
        this.list = list;
        this.inflater = LayoutInflater.from(context);
    }

    @Override
    public RecyclerView.ViewHolder onCreateViewHolder(ViewGroup parent, int viewType) {
    	// 根据ViewType加载不同布局
        switch (viewType) {
            case TYPE_NORMAL:
                return new NormalViewHolder(inflater.inflate(R.layout.home_list_item_layout, parent, false), mListener);
            case TYPE_FOOTER:
                return new FooterViewHolder(inflater.inflate(R.layout.list_footer, parent, false));
        }
        return null;
    }

    @Override
    public void onBindViewHolder(RecyclerView.ViewHolder holder, int position) {

		// 对不同的ViewHolder做不同的处理
        if (holder instanceof NormalViewHolder) {

            ZhihuDailyNews.Question item = list.get(position);

            if (item.getImages().get(0) == null){
                ((NormalViewHolder)holder).itemImg.setImageResource(R.drawable.placeholder);
            } else {
                Glide.with(context)
                        .load(item.getImages().get(0))
                        .asBitmap()
                        .placeholder(R.drawable.placeholder)
                        .diskCacheStrategy(DiskCacheStrategy.SOURCE)
                        .error(R.drawable.placeholder)
                        .centerCrop()
                        .into(((NormalViewHolder)holder).itemImg);
            }
            ((NormalViewHolder)holder).tvLatestNewsTitle.setText(item.getTitle());
        }
        
    }

	// 因为含有footer，返回值需要 + 1
    @Override
    public int getItemCount() {
        return list.size() + 1;
    }

    @Override
    public int getItemViewType(int position) {
        if (position == list.size()) {
            return ZhihuDailyNewsAdapter.TYPE_FOOTER;
        }
        return ZhihuDailyNewsAdapter.TYPE_NORMAL;
    }

    public void setItemClickListener(OnRecyclerViewOnClickListener listener){
        this.mListener = listener;
    }

    public class NormalViewHolder extends RecyclerView.ViewHolder implements View.OnClickListener {

        private ImageView itemImg;
        private TextView tvLatestNewsTitle;
        private OnRecyclerViewOnClickListener listener;

        public NormalViewHolder(View itemView, OnRecyclerViewOnClickListener listener) {
            super(itemView);
            itemImg = (ImageView) itemView.findViewById(R.id.imageViewCover);
            tvLatestNewsTitle = (TextView) itemView.findViewById(R.id.textViewTitle);
            this.listener = listener;
            itemView.setOnClickListener(this);
        }

        @Override
        public void onClick(View v) {
            if (listener != null){
                listener.OnItemClick(v,getLayoutPosition());
            }
        }
    }

    public class FooterViewHolder extends RecyclerView.ViewHolder{

        public FooterViewHolder(View itemView) {
            super(itemView);
        }

    }

}
```

adapter中含有两个常量，`TYPE_NORMAL`,`TYPE_FOOTER`,用于区别item的类型，从而加载不同的布局。众所周知，RecyclerView原生并没有设置item点击事件的方法，所有我们需要自己定义一个接口–`OnRecyclerViewOnClickListener`。

[OnRecyclerViewOnClickListener.java](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/interfaze/OnRecyclerViewOnClickListener.java)

```
package com.marktony.zhihudaily.interfaze;

import android.view.View;

public interface OnRecyclerViewOnClickListener {

    void OnItemClick(View v,int position);

}
```

[ZhihuDailyPresenter.java](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/homepage/ZhihuDailyPresenter.java)

实现`ZhihuDailyPresenter`中的`loadPosts`方法，记得要在manifest清单文件中添加网络访问权限：

```
model.load(Api.ZHIHU_HISTORY + formatter.ZhihuDailyDateFormat(date), new OnStringListener() {
                @Override
                public void onSuccess(String result) {

                    try {
                        ZhihuDailyNews post = gson.fromJson(result, ZhihuDailyNews.class);

                        if (clearing) {
                            list.clear();
                        }

                        for (ZhihuDailyNews.Question item : post.getStories()) {
                            list.add(item);                          
                        }
                        view.showResults(list);
                        
                    } catch (JsonSyntaxException e) {
                        view.showError();
                    }

                    view.stopLoading();
                }

                @Override
                public void onError(VolleyError error) {
                    view.stopLoading();
                    view.showError();
                }
            });
```

我们通过Gson，可以很简单将JSON格式数据转换为Java对象。

[ZhihuDailyFragment](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/homepage/ZhihuDailyFragment.java)

实现`ZhihuDailyFragment`的`showResults`方法。

```
@Override
public void showResults(ArrayList<ZhihuDailyNews.Question> list) {
    if (adapter == null) {
        adapter = new ZhihuDailyNewsAdapter(getContext(), list);
        adapter.setItemClickListener(new OnRecyclerViewOnClickListener() {
            @Override
            public void OnItemClick(View v, int position) {
                presenter.startReading(position);
            }
        });
        recyclerView.setAdapter(adapter);
    } else {
        adapter.notifyDataSetChanged();
    }
}
```

#### Day 4，缓存内容

完成上面的代码，我们还只是实现了在有网络状态下的正常运行，如果用户并没有那么畅通无阻的网络连接呢？这个时候缓存就派上用场了，只要用户加载过一次，以后就算没有网络连接，用户也能查看之前已经离线的内容。我们选择使用Android原生SQLite数据库来存储数据(当然你也可以选择[Realm](https://realm.io/))。

首先当然是要建立数据库了(由于纸飞机已经进行多个版本的迭代，所以你创建数据库的SQL语句或其他内容和我的文件应该不完全相同)。

[DatabaseHelper.java](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/db/DatabaseHelper.java)

```
public class DatabaseHelper extends SQLiteOpenHelper {


    public DatabaseHelper(Context context, String name, SQLiteDatabase.CursorFactory factory, int version) {
        super(context, name, factory, version);
    }

    @Override
    public void onCreate(SQLiteDatabase db) {

        db.execSQL("create table if not exists Zhihu("
                + "id integer primary key autoincrement,"
                + "zhihu_id integer not null,"
                + "zhihu_news text,"
                + "zhihu_time real,"
                + "zhihu_content text)");

        db.execSQL("alter table Zhihu add column bookmark integer default 0");

    }

    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
    
   }
}
```

相信大牛应该看出来了，这数据库设计的真心不怎么样😂，因为我数据库学的确实很一般。求大牛不喷。

| 字段            | 类型      | 含义          | 备注                                   |
| ------------- | ------- | ----------- | ------------------------------------ |
| id            | integer | 主键          | 自增长                                  |
| zhihu_id      | integer | 知乎日报消息id    | 由知乎提供                                |
| zhihu_news    | text    | 知乎日报消息内容    | 与Java实体类对应                           |
| zhihu_time    | real    | 知乎日报消息发布的时间 | 由知乎提供                                |
| zhihu_content | text    | 知乎日报消息详细内容  | 与Java实体类对应                           |
| bookmark      | integer | 是否被收藏       | 由于SQLite并没有boolean类型，使用integer的不同值代替 |

OK,当我们正确请求到数据后，就可以进行存储了。

[ZhihuDailyPresenter.java](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/homepage/ZhihuDailyPresenter.java)

```
if ( !queryIfIDExists(item.getId())) {
    db.beginTransaction();
    try {
        DateFormat format = new SimpleDateFormat("yyyyMMdd");
        Date date = format.parse(post.getDate());
        values.put("zhihu_id", item.getId());
        values.put("zhihu_news", gson.toJson(item));
        values.put("zhihu_content", "");
        values.put("zhihu_time", date.getTime() / 1000);
        db.insert("Zhihu", null, values);
        values.clear();
        db.setTransactionSuccessful();
    } catch (Exception e) {
        e.printStackTrace();
    } finally {
        db.endTransaction();
    }
}

// 查询数据库表中是否已经存在了此id
private boolean queryIfIDExists(int id){

    Cursor cursor = db.query("Zhihu",null,null,null,null,null,null);
    if (cursor.moveToFirst()){
        do {
            if (id == cursor.getInt(cursor.getColumnIndex("zhihu_id"))){
                return true;
            }
        } while (cursor.moveToNext());
    }
    cursor.close();

    return false;
}
```

细心的童鞋可能发现了，诶，数据表中还有一个字段–zhihu_content，你没有存储呀。这是因为我们在请求知乎消息列表的时候，并没有返回消息的详细内容呀。不过详细内容我们还是需要缓存的，网络请求在UI线程上进行可能会引起ANR，那更好的解决办法就是在Service里面完成了。

我们先将一些必须的数据通过本地广播的形式，发送出去。

[ZhihuDailyPresenter.java](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/homepage/ZhihuDailyPresenter.java)

```
Intent intent = new Intent("com.marktony.zhihudaily.LOCAL_BROADCAST");
intent.putExtra("type", CacheService.TYPE_ZHIHU);
intent.putExtra("id", item.getId());
LocalBroadcastManager.getInstance(context).sendBroadcast(intent);
```

然后在`CacheService`里接收广播，获取传送的数据，然后进行网络请求和数据存储。

[CacheService.java](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/service/CacheService.java)

```
public class CacheService extends Service {

    private DatabaseHelper dbHelper;
    private SQLiteDatabase db;

    private static final String TAG = CacheService.class.getSimpleName();

    public static final int TYPE_ZHIHU = 0x00;
    public static final int TYPE_GUOKR = 0x01;
    public static final int TYPE_DOUBAN = 0x02;

    @Override
    public void onCreate() {
        super.onCreate();
        dbHelper = new DatabaseHelper(this, "History.db", null, 5);
        db = dbHelper.getWritableDatabase();

        IntentFilter filter = new IntentFilter();
        filter.addAction("com.marktony.zhihudaily.LOCAL_BROADCAST");
        LocalBroadcastManager manager = LocalBroadcastManager.getInstance(this);
        manager.registerReceiver(new LocalReceiver(), filter);

    }

    @Nullable
    @Override
    public IBinder onBind(Intent intent) {
        return null;
    }

    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
        return super.onStartCommand(intent, flags, startId);
    }

    @Override
    public boolean onUnbind(Intent intent) {
        return super.onUnbind(intent);
    }

    /**
     * 网络请求id对应的知乎日报的内容主体
     * 当type为0时，存储body中的数据
     * 当type为1时，再次请求share url中的内容并储存
     * @param id 所要获取的知乎日报消息内容对应的id
     */
    private void startZhihuCache(final int id) {

        Cursor cursor = db.query("Zhihu", null, null, null, null, null, null);
        if (cursor.moveToFirst()) {
            do {
                if ((cursor.getInt(cursor.getColumnIndex("zhihu_id")) == id)
                        && (cursor.getString(cursor.getColumnIndex("zhihu_content")).equals(""))) {
                    StringRequest request = new StringRequest(Request.Method.GET, Api.ZHIHU_NEWS + id, new Response.Listener<String>() {
                        @Override
                        public void onResponse(String s) {
                            Gson gson = new Gson();
                            ZhihuDailyStory story = gson.fromJson(s, ZhihuDailyStory.class);
                            if (story.getType() == 1) {
                                StringRequest request = new StringRequest(Request.Method.GET, story.getShare_url(), new Response.Listener<String>() {
                                    @Override
                                    public void onResponse(String s) {
                                        ContentValues values = new ContentValues();
                                        values.put("zhihu_content", s);
                                        db.update("Zhihu", values, "zhihu_id = ?", new String[] {String.valueOf(id)});
                                        values.clear();
                                    }
                                }, new Response.ErrorListener() {
                                    @Override
                                    public void onErrorResponse(VolleyError volleyError) {

                                    }
                                });
                                request.setTag(TAG);
                                VolleySingleton.getVolleySingleton(CacheService.this).addToRequestQueue(request);
                            } else {
                                ContentValues values = new ContentValues();
                                values.put("zhihu_content", s);
                                db.update("Zhihu", values, "zhihu_id = ?", new String[] {String.valueOf(id)});
                                values.clear();
                            }

                        }
                    }, new Response.ErrorListener() {
                        @Override
                        public void onErrorResponse(VolleyError volleyError) {

                        }
                    });
                    request.setTag(TAG);
                    VolleySingleton.getVolleySingleton(CacheService.this).addToRequestQueue(request);
                }
            } while (cursor.moveToNext());
        }
        cursor.close();
    }

    @Override
    public void onDestroy() {
        super.onDestroy();
        VolleySingleton.getVolleySingleton(this).getRequestQueue().cancelAll(TAG);
    }

    class LocalReceiver extends BroadcastReceiver {

        @Override
        public void onReceive(Context context, Intent intent) {
            int id = intent.getIntExtra("id", 0);
            switch (intent.getIntExtra("type", -1)) {
                case TYPE_ZHIHU:
                    startZhihuCache(id);
                    break;
                case TYPE_GUOKR:
                    startGuokrCache(id);
                    break;
                case TYPE_DOUBAN:
                    startDoubanCache(id);
                    break;
                default:
                case -1:
                    break;
            }
        }
    }

}
```

我们先遍历一下数据库，如果数据库中指定id的消息详情内容已经不为空，那我们就直接跳过了，可以节省用户的流量以及电量。

到这里，数据的存储是完成了。可是怎么读取出来呢？哈，其实也简单，我们判断一下当前的网络状态，如果用户设备没有连接到网路，我们就直接去数据库中读取，然后解析就行了。

[ZhihuDailyPresenter.java](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/homepage/ZhihuDailyPresenter.java)

```
if (NetworkState.networkConnected(context)) {
	// balabala
} else {
	Cursor cursor = db.query("Zhihu", null, null, null, null, null, null);
	if (cursor.moveToFirst()) {
	    do {
	        ZhihuDailyNews.Question question = gson.fromJson(cursor.getString(cursor.getColumnIndex("zhihu_news")), ZhihuDailyNews.Question.class);
	        list.add(question);
	    } while (cursor.moveToNext());
	}
	cursor.close();
	view.stopLoading();
	view.showResults(list);
}
```

到这里，今天的工作差不多已经完成了，等等，是不是忘了什么？我们的Service并没有启动呀。

[MainActivity.java](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/homepage/MainActivity.java)

```
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);

    initViews(); 

	// 启动服务
    startService(new Intent(this, CacheService.class));

}

@Override
protected void onDestroy() {
    ActivityManager manager = (ActivityManager) getSystemService(Context.ACTIVITY_SERVICE);
    for (ActivityManager.RunningServiceInfo service : manager.getRunningServices(Integer.MAX_VALUE)) {
        if (CacheService.class.getName().equals(service.service.getClassName())) {
            stopService(new Intent(this, CacheService.class));
        }
    }
    super.onDestroy();
}
```

到这里，今天的内容就算结束了，内容是一周之中最多的一天，可能比前几天的总和还要多，可能需要你加班才能完全完成，之前Activity, Presenter, Fragment中各还有一部分内容没有完成，需要你自行补充完成。不过，看到自己的App正确的跑了起来，有木有很兴奋呢？休息休息，准备明天的工作吧。

### DAY 5

今天的内容是显示消息详情内容。因为我们的消息内容实际上有三种类型，这里就不再重复。怎么区分呢？我的方法是定义了一个枚举类型:

[BeanType.java](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/bean/BeanType.java)

```
public enum BeanType {

    TYPE_ZHIHU,TYPE_GUOKR,TYPE_DOUBAN;

}
```

这样，我们就能根据不同的消息类型，获取和加载不同的消息详情内容了。

[ZhihuDailyPresenter.java](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/homepage/ZhihuDailyPresenter.java)

```
@Override
public void startReading(int position) {

    context.startActivity(new Intent(context, DetailActivity.class)
            .putExtra("type", BeanType.TYPE_ZHIHU)
            .putExtra("id", list.get(position).getId())
            .putExtra("title", list.get(position).getTitle())
            .putExtra("coverUrl", list.get(position).getImages().get(0)));

}
```

#### Day 5,界面编写

从知乎给我们的详情内容为HTML格式来看，用WebView作为显示控件最合适不过了(实际上果壳和豆瓣的详情页内容要么是返回了HTML格式，要么是直接给出了详情页的网页地址，做简单处理即可，可以实现复用)。

先看布局文件代码：

[universal_read_layout.xml](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/res/layout/universal_read_layout.xml)

```
<?xml version="1.0" encoding="utf-8"?>
<android.support.design.widget.CoordinatorLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:fitsSystemWindows="true"
    android:id="@+id/coordinatorLayout">

    <android.support.design.widget.AppBarLayout
        android:id="@+id/app_bar"
        android:layout_width="match_parent"
        android:layout_height="@dimen/app_bar_height"
        android:fitsSystemWindows="true"
        android:theme="@style/AppTheme.AppBarOverlay">

        <android.support.design.widget.CollapsingToolbarLayout
            android:id="@+id/toolbar_layout"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:fitsSystemWindows="true"
            app:contentScrim="@color/colorPrimary"
            app:layout_scrollFlags="scroll|enterAlwaysCollapsed|enterAlways">

            <ImageView
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:id="@+id/image_view"
                android:fitsSystemWindows="true"
                android:scaleType="centerCrop"
                android:scrollbarStyle="insideInset"
                android:scrollbarAlwaysDrawVerticalTrack="true" />

            <android.support.v7.widget.Toolbar
                android:id="@+id/toolbar"
                android:layout_width="match_parent"
                android:layout_height="?attr/actionBarSize"
                app:layout_collapseMode="pin"
                app:popupTheme="@style/AppTheme.PopupOverlay"
                app:background="@color/colorPrimary"/>

        </android.support.design.widget.CollapsingToolbarLayout>

    </android.support.design.widget.AppBarLayout>

    <android.support.v4.widget.SwipeRefreshLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        app:layout_behavior="@string/appbar_scrolling_view_behavior"
        android:id="@+id/refreshLayout">

        <android.support.v4.widget.NestedScrollView
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:id="@+id/scrollView"
            android:scrollbars="vertical"
            android:scrollbarFadeDuration="1"
            android:fadeScrollbars="true">

            <WebView
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:id="@+id/web_view" />

        </android.support.v4.widget.NestedScrollView>

    </android.support.v4.widget.SwipeRefreshLayout>

</android.support.design.widget.CoordinatorLayout>
```

将`ImageView`嵌入Toolbar中，然后搭配`CollapsingToolbarLayout`可收缩的ToolbarLayout，实现收缩和展开效果。`SwipeRefreshLayout`仍然用于显示加载状态和刷新。然后就齐活了。

接着是Activity。

[DetailActivity.java](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/detail/DetailActivity.java)

```
public class DetailActivity extends AppCompatActivity {

    private DetailFragment fragment;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.frame);

        if (savedInstanceState != null) {
            fragment = (DetailFragment) getSupportFragmentManager().getFragment(savedInstanceState,"detailFragment");
        } else {
            fragment = new DetailFragment();
            getSupportFragmentManager().beginTransaction()
                    .replace(R.id.container, fragment)
                    .commit();
        }

        Intent intent = getIntent();

        DetailPresenter presenter = new DetailPresenter(DetailActivity.this, fragment);

        presenter.setType((BeanType) intent.getSerializableExtra("type"));
        presenter.setId(intent.getIntExtra("id", 0));
        presenter.setTitle(intent.getStringExtra("title"));
        presenter.setCoverUrl(intent.getStringExtra("coverUrl"));

    }

    @Override
    protected void onSaveInstanceState(Bundle outState) {
        super.onSaveInstanceState(outState);
        if (fragment.isAdded()) {
            getSupportFragmentManager().putFragment(outState, "detailFragment", fragment);
        }
    }
}
```

内容与首页类似，相信你能理解，也是通过Activity + Fragment搭配的方式进行的。那么，View层也就是Fragment需要完成那些功能呢？我们可以直接在契约类中定义好。

[DetailContract.java](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/detail/DetailContract.java)

```
public class DetailContract {

    interface View extends BaseView<Presenter> {

		// 显示正在加载
        void showLoading();
		// 停止加载
        void stopLoading();
		// 显示加载错误
        void showLoadingError();
		// 显示分享时错误
        void showSharingError();
		// 正确获取数据后显示内容
        void showResult(String result);
		// 对于body字段的消息，直接接在url的内容
        void showResultWithoutBody(String url);
		// 设置顶部大图
        void showCover(String url);
		// 设置标题
        void setTitle(String title);
		// 设置是否显示图片
        void setImageMode(boolean showImage);
		// 用户选择在浏览器中打开时，如果没有安装浏览器，显示没有找到浏览器错误
        void showBrowserNotFoundError();
		// 显示已复制文字内容
        void showTextCopied();
		// 显示文字复制失败
        void showCopyTextError();
		// 显示已添加至收藏夹
        void showAddedToBookmarks();
		// 显示已从收藏夹中移除
        void showDeletedFromBookmarks();

    }
    
    interface Presenter extends BasePresenter{

		// 在浏览器中打开
        void openInBrowser();
		// 作为文字分享
        void shareAsText();
		// 打开文章中的链接
        void openUrl(WebView webView, String url);
		// 复制文字内容
        void copyText();
		// 复制文章链接
        void copyLink();
		// 添加至收藏夹或者从收藏夹中删除
        void addToOrDeleteFromBookmarks();
		// 查询是否已经被收藏了
        boolean queryIfIsBookmarked();
		// 请求数据
        void requestData();

    }
    
}
```

[DetailFragment.java](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/detail/DetailFragment.java)

```
public class DetailFragment extends Fragment
        implements DetailContract.View {
        
    public DetailFragment() {}


    @Override
    public void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
    }

    @Nullable
    @Override
    public View onCreateView(LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.universal_read_layout, container, false);

        initViews(view);

        setHasOptionsMenu(true);

        return view;
    }

    @Override
    public void onCreateOptionsMenu(Menu menu, MenuInflater inflater) {
        inflater.inflate(R.menu.menu_more, menu);
        super.onCreateOptionsMenu(menu, inflater);
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        int id = item.getItemId();
        if (id == android.R.id.home) {
            getActivity().onBackPressed();
        } else if (id == R.id.action_more) {

        }
        return true;
    }

    @Override
    public void showLoading() {
        
    }

    @Override
    public void stopLoading() {
        
    }

    @Override
    public void showLoadingError() {
        
    }

    @Override
    public void showSharingError() {
        
    }

    @Override
    public void showResult(String result) {
        
    }

    @Override
    public void showResultWithoutBody(String url) {
        
    }

    @Override
    public void showCover(String url) {
        
    }

    @Override
    public void setTitle(String title) {
		
    }

	// WebView 提供了是否显示图片的方法
    @Override
    public void setImageMode(boolean showImage) {
		webView.getSettings().setBlockNetworkImage(showImage);
    }

    @Override
    public void showBrowserNotFoundError() {

    }

    @Override
    public void showTextCopied() {

    }

    @Override
    public void showCopyTextError() {

    }

    @Override
    public void showAddedToBookmarks() {

    }

    @Override
    public void showDeletedFromBookmarks() {

    }

    @Override
    public void setPresenter(DetailContract.Presenter presenter) {

    }

    @Override
    public void initViews(View view) {

    }

    // to change the title's font size of toolbar layout
    private void setCollapsingToolbarLayoutTitle(String title) {
		toolbarLayout.setTitle(title);
        toolbarLayout.setExpandedTitleTextAppearance(R.style.ExpandedAppBar);
        toolbarLayout.setCollapsedTitleTextAppearance(R.style.CollapsedAppBar);
        toolbarLayout.setExpandedTitleTextAppearance(R.style.ExpandedAppBarPlus1);
        toolbarLayout.setCollapsedTitleTextAppearance(R.style.CollapsedAppBarPlus1);
    }

}
```

#### Day 5,实体类

布局文件完成了，现在开始写实体类。方法和昨天写列表项实体类一样，可以手动编写，也可以用插件直接生成。直接放代码。

JSON格式数据:

```
{
  "body": "HTML格式内容",
  "image_source": "《那些年，我们一起追的女孩》",
  "title": "瞎扯 · 如何正确地吐槽",
  "image": "http://pic1.zhimg.com/13ee386166c53553ea6997d821609e0c.jpg",
  "share_url": "http://daily.zhihu.com/story/9195072",
  "js": [],
  "ga_prefix": "020706",
  "section": {
    "thumbnail": "http://pic2.zhimg.com/1dc9cf1556c7b0b1527c18476698c5cd.jpg",
    "id": 2,
    "name": "瞎扯"
  },
  "images": [
    "http://pic2.zhimg.com/1dc9cf1556c7b0b1527c18476698c5cd.jpg"
  ],
  "type": 0,
  "id": 9195072,
  "css": [
    "http://news-at.zhihu.com/css/news_qa.auto.css?v=4b3e3"
  ]
}
```

对应的实体类：
[ZhihuDailyStory.java](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/bean/ZhihuDailyStory.java)

```
public class ZhihuDailyStory {

    private String body;
    private String image_source;
    private String title;
    private String image;
    private String share_url;
    private ArrayList<String> js;
    private String ga_prefix;
    private ArrayList<String> images;
    private int type;
    private int id;
    private ArrayList<String> css;

    public String getBody() {
        return body;
    }

    public void setBody(String body) {
        this.body = body;
    }

    public String getImage_source() {
        return image_source;
    }

    public void setImage_source(String image_source) {
        this.image_source = image_source;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public String getImage() {
        return image;
    }

    public void setImage(String image) {
        this.image = image;
    }

    public String getShare_url() {
        return share_url;
    }

    public void setShare_url(String share_url) {
        this.share_url = share_url;
    }

    public ArrayList<String> getJs() {
        return js;
    }

    public void setJs(ArrayList<String> js) {
        this.js = js;
    }

    public String getGa_prefix() {
        return ga_prefix;
    }

    public void setGa_prefix(String ga_prefix) {
        this.ga_prefix = ga_prefix;
    }

    public ArrayList<String> getImages() {
        return images;
    }

    public void setImages(ArrayList<String> images) {
        this.images = images;
    }

    public int getType() {
        return type;
    }

    public void setType(int type) {
        this.type = type;
    }

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public ArrayList<String> getCss() {
        return css;
    }

    public void setCss(ArrayList<String> css) {
        this.css = css;
    }

}
```

#### Day 5,显示数据

嘻嘻，首先当然是获取数据了☺️，需要考虑网络连接的情况，如果网络通畅，则直接从网络中获取，否则去数据库中获取。

[DetailPresenter.java](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/detail/DetailPresenter.java)

```
if (NetworkState.networkConnected(context)) {
    model.load(Api.ZHIHU_NEWS + id, new OnStringListener() {
        @Override
        public void onSuccess(String result) {
            {
                Gson gson = new Gson();
                try {
                    zhihuDailyStory = gson.fromJson(result, ZhihuDailyStory.class);
                    if (zhihuDailyStory.getBody() == null) {
                        view.showResultWithoutBody(zhihuDailyStory.getShare_url());
                    } else {
                        view.showResult(convertZhihuContent(zhihuDailyStory.getBody()));
                    }
                } catch (JsonSyntaxException e) {
                    view.showLoadingError();
                }
                view.stopLoading();
            }
        }

        @Override
        public void onError(VolleyError error) {
            view.stopLoading();
            view.showLoadingError();
        }
    });
} else {
    Cursor cursor = dbHelper.getReadableDatabase()
            .query("Zhihu", null, null, null, null, null, null);
    if (cursor.moveToFirst()) {
        do {
            if (cursor.getInt(cursor.getColumnIndex("zhihu_id")) == id) {
                String content = cursor.getString(cursor.getColumnIndex("zhihu_content"));
                try {
                    zhihuDailyStory = gson.fromJson(content, ZhihuDailyStory.class);
                } catch (JsonSyntaxException e) {
                    view.showResult(content);
                }
                view.showResult(convertZhihuContent(zhihuDailyStory.getBody()));
            }
        } while (cursor.moveToNext());
    }
    cursor.close();
}

private String convertZhihuContent(String preResult) {

    preResult = preResult.replace("<div class=\"img-place-holder\">", "");
    preResult = preResult.replace("<div class=\"headline\">", "");

    // 在api中，css的地址是以一个数组的形式给出，这里需要设置
    // api中还有js的部分，这里不再解析js
    // 不再选择加载网络css，而是加载本地assets文件夹中的css
    String css = "<link rel=\"stylesheet\" href=\"file:///android_asset/zhihu_daily.css\" type=\"text/css\">";

    String theme = "<body className=\"\" onload=\"onLoaded()\">";

    return new StringBuilder()
            .append("<!DOCTYPE html>\n")
            .append("<html lang=\"en\" xmlns=\"http://www.w3.org/1999/xhtml\">\n")
            .append("<head>\n")
            .append("\t<meta charset=\"utf-8\" />")
            .append(css)
            .append("\n</head>\n")
            .append(theme)
            .append(preResult)
            .append("</body></html>").toString();
}
```

对获取的数据进行一下拼接，组成一个完整的HTML页面的内容。需要注意的是CSS文件，它负责整个HTML的样式，可以在[这里](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/assets/zhihu_daily.css)查看整个CSS文件的内容或下载CSS文件。

最后的显示就非常简单了：

[DetailFragment.java](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/detail/DetailFragment.java)

```
@Override
public void showResult(String result) {
    webView.loadDataWithBaseURL("x-data://base",result,"text/html","utf-8",null);
}
```

至此，最基本的显示详情内容的部分就已经完成了。实际上，我们还有很多的工作细微的工作没有完成，喝杯咖啡，休息一下，再回来继续吧。

#### Day 5,设置与关于

设置与关于也和首页及详情相同，采用的是Activity + Fragment搭配的形式。不过，这里的Fragment并不是我们前面所见到的`android.support.v4.app.Fragment`下的Fragment，而是`android.support.v7.preference.PreferenceFragmentCompat`。通过
`PreferenceFragmentCompat`，我们可以很快的实现设置与关于页面。(由于二者的实现方法类似，我就以实现关于页面为例)

首先，我们需要在`res`目录下新建`xml`文件夹，新建`about_preference_fragment.xml`文件，作为设置和关于页面的布局文件。

[about_preference_fragment.xml](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/res/xml/about_preference_fragment.xml)

```
<?xml version="1.0" encoding="utf-8"?>
<android.support.v7.preference.PreferenceScreen
    xmlns:android="http://schemas.android.com/apk/res/android">

    <android.support.v7.preference.PreferenceCategory android:title="@string/app_name">

        <android.support.v7.preference.Preference android:title="@string/version" />

        <android.support.v7.preference.Preference android:title="@string/rate"
            android:key="rate"
            android:summary="@string/rate_description" />

    </android.support.v7.preference.PreferenceCategory>

    <android.support.v7.preference.PreferenceCategory android:title="@string/author">

        <android.support.v7.preference.Preference android:title="@string/author_name"
            android:key="author"
            android:summary="@string/author_description"/>

        <android.support.v7.preference.Preference
            android:title="@string/follow_me_on_github"
            android:key="follow_me_on_github"
            android:summary="@string/github_url"/>

        <android.support.v7.preference.Preference
            android:title="@string/follow_me_on_zhihu"
            android:key="follow_me_on_zhihu"
            android:summary="@string/zhihu_account"/>

    </android.support.v7.preference.PreferenceCategory>

    <android.support.v7.preference.PreferenceCategory android:title="@string/support">

        <android.support.v7.preference.Preference android:title="@string/feedback"
            android:key="feedback"
            android:summary="@string/feedback_description"/>

        <android.support.v7.preference.Preference android:title="@string/coffee"
            android:key="coffee"
            android:summary="@string/coffee_description"/>

        <android.support.v7.preference.Preference android:title="@string/open_source_license"
            android:key="open_source_license" />

    </android.support.v7.preference.PreferenceCategory>

</android.support.v7.preference.PreferenceScreen>
```

这样，布局文件就已经完成了。接下来是和首页等类似的，分别完成`Contract`,`Activity`,`Fragment`,`Presenter`。

[AboutContract.java](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/about/AboutContract.java)

```
public interface AboutContract {

    interface View extends BaseView<Presenter>{

		// 如果用户设备没有安装商店应用，提示此错误
        void showRateError();
		// 如果用户设备没有安装邮件应用，提示此错误
        void showFeedbackError();
		// 如果用户没有安装浏览器，提示此错误
        void showBrowserNotFoundError();

    }

    interface Presenter extends BasePresenter {
		// 在应用商店中评分
        void rate();
		// 展示开源许可页
        void openLicense();
		// 在GitHub上关注我
        void followOnGithub();
		// 在知乎上关注我
        void followOnZhihu();
		// 通过邮件反馈
        void feedback();
		// 捐赠
        void donate();
		// 显示小彩蛋
        void showEasterEgg();

    }

}
```

[AboutPreferenceActivity.java](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/about/AboutPreferenceActivity.java)

```
public class AboutPreferenceActivity extends AppCompatActivity {

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_about);

        initViews();

        AboutPreferenceFragment fragment = new AboutPreferenceFragment();

        getSupportFragmentManager()
                .beginTransaction()
                .add(R.id.about_container,fragment)
                .commit();

        new AboutPresenter(AboutPreferenceActivity.this, fragment);

    }

    private void initViews() {
        setSupportActionBar((Toolbar) findViewById(R.id.toolbar));
        getSupportActionBar().setDisplayHomeAsUpEnabled(true);
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        if (item.getItemId() == android.R.id.home){
            onBackPressed();
        }
        return super.onOptionsItemSelected(item);
    }

}
```

是不是有种似曾相识的感觉呢😉？

[AboutPreferenceFragment.java](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/about/AboutPreferenceFragment.java)

```
public class AboutPreferenceFragment extends PreferenceFragmentCompat
        implements AboutContract.View {

    private Toolbar toolbar;
    private AboutContract.Presenter presenter;

    public void onCreatePreferences(Bundle savedInstanceState, String rootKey) {

        addPreferencesFromResource(R.xml.about_preference_fragment);

        initViews(getView());

        findPreference("rate").setOnPreferenceClickListener(new android.support.v7.preference.Preference.OnPreferenceClickListener() {
            @Override
            public boolean onPreferenceClick(android.support.v7.preference.Preference preference) {
                presenter.rate();
                return false;
            }
        });

        findPreference("open_source_license").setOnPreferenceClickListener(new android.support.v7.preference.Preference.OnPreferenceClickListener() {
            @Override
            public boolean onPreferenceClick(android.support.v7.preference.Preference preference) {
                return false;
            }
        });

        findPreference("follow_me_on_github").setOnPreferenceClickListener(new android.support.v7.preference.Preference.OnPreferenceClickListener() {
            @Override
            public boolean onPreferenceClick(android.support.v7.preference.Preference preference) {
                return false;
            }
        });

        findPreference("follow_me_on_zhihu").setOnPreferenceClickListener(new android.support.v7.preference.Preference.OnPreferenceClickListener() {
            @Override
            public boolean onPreferenceClick(android.support.v7.preference.Preference preference) {
                return false;
            }
        });

        findPreference("feedback").setOnPreferenceClickListener(new android.support.v7.preference.Preference.OnPreferenceClickListener() {
            @Override
            public boolean onPreferenceClick(android.support.v7.preference.Preference preference) {
                return false;
            }
        });

        findPreference("coffee").setOnPreferenceClickListener(new android.support.v7.preference.Preference.OnPreferenceClickListener() {
            @Override
            public boolean onPreferenceClick(android.support.v7.preference.Preference preference) {
                return false;
            }
        });

        findPreference("author").setOnPreferenceClickListener(new Preference.OnPreferenceClickListener() {
            @Override
            public boolean onPreferenceClick(Preference preference) {
                return false;
            }
        });

    }

    @Override
    public void onResume() {
        super.onResume();
        presenter.start();
    }

    @Override
    public void setPresenter(AboutContract.Presenter presenter) {
        if (presenter != null){
            this.presenter = presenter;
        }
    }

    @Override
    public void initViews(View view) {
        
    }

    @Override
    public void showRateError() {
        
    }

    @Override
    public void showFeedbackError() {
        
    }

    @Override
    public void showBrowserNotFoundError() {
        
    }

}
```

不知道你主要到没有，这里有一些代码和我们常见的有所不同。例如，`onCreatePreferences`方法，`addPreferencesFromResource`方法等。

[AboutPresenter.java](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/about/AboutPresenter.java)

```
public class AboutPresenter implements AboutContract.Presenter {

    public AboutPresenter(AppCompatActivity activity, AboutContract.View view) {
        
    }

    @Override
    public void start() {

    }


    @Override
    public void rate() {
        try {
            Uri uri = Uri.parse("market://details?id=" + activity.getPackageName());
            Intent intent = new Intent(Intent.ACTION_VIEW,uri);
            intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
            activity.startActivity(intent);
        } catch (android.content.ActivityNotFoundException ex){
            view.showRateError();
        }

    }

    @Override
    public void openLicense() {
		activity.startActivity(new Intent(activity,OpenSourceLicenseActivity.class));
    }

   @Override
    public void followOnGithub() {
        if (sp.getBoolean("in_app_browser",true)){
            CustomTabActivityHelper.openCustomTab(
                    activity,
                    customTabsIntent.build(),
                    Uri.parse(activity.getString(R.string.github_url)),
                    new CustomFallback() {
                        @Override
                        public void openUri(Activity activity, Uri uri) {
                            super.openUri(activity, uri);
                        }
                    });
        } else {
            try{
                activity.startActivity(new Intent(Intent.ACTION_VIEW).setData(Uri.parse( activity.getString(R.string.github_url))));
            } catch (android.content.ActivityNotFoundException ex){
                view.showBrowserNotFoundError();
            }
        }
    }

    @Override
    public void followOnZhihu() {

    }

    @Override
    public void feedback() {
        try{
            Uri uri = Uri.parse(activity.getString(R.string.sendto));
            Intent intent = new Intent(Intent.ACTION_SENDTO,uri);
            intent.putExtra(Intent.EXTRA_SUBJECT, activity.getString(R.string.mail_topic));
            intent.putExtra(Intent.EXTRA_TEXT,
                    activity.getString(R.string.device_model) + Build.MODEL + "\n"
                            + activity.getString(R.string.sdk_version) + Build.VERSION.RELEASE + "\n"
                            + activity.getString(R.string.version));
            activity.startActivity(intent);
        }catch (android.content.ActivityNotFoundException ex){
            view.showFeedbackError();
        }
    }

    @Override
    public void donate() {
        
    }

    @Override
    public void showEasterEgg() {
        
    }

}
```

具体的实现逻辑我没有给出，你可以在源代码中找到。需要注意的一些小细节，例如，在反馈操作中，我们是通过调用邮件App实现的。如下:

```
@Override
public void feedback() {
    try{
        Uri uri = Uri.parse(activity.getString(R.string.sendto));
        Intent intent = new Intent(Intent.ACTION_SENDTO,uri);
        intent.putExtra(Intent.EXTRA_SUBJECT, activity.getString(R.string.mail_topic));
        intent.putExtra(Intent.EXTRA_TEXT,
                activity.getString(R.string.device_model) + Build.MODEL + "\n"
                        + activity.getString(R.string.sdk_version) + Build.VERSION.RELEASE + "\n"
                        + activity.getString(R.string.version));
        activity.startActivity(intent);
    }catch (android.content.ActivityNotFoundException ex){
        view.showFeedbackError();
    }
}
```

为毛要做try…catch的操作呢？难道用户的设备上会连邮件App都没有安装吗？当然会。有的设备上甚至连浏览器都没有安装，所以，try…catch还是很有必要的。

需要提起的是，设置页面，我们需要对用户的偏好进行存储，然后在需要的地方获取这个值就好了，而PreferenceScreen本身就具有这样的功能，不再需要额外的SharedPreference去存储。在我的代码你可能会看到这样的情况，这是因为我并不是在项目的最初就引进了`PreferenceScreen`，当时就是直接用不同的控件搭配成的设置界面，用`SharedPreference`存储信息，后来引入了支持库之后，为了不破坏用户的体验(例如，某次版本升级直接导致之前的设置偏好全部失效)，坚持使用了这样一个‘多此一举’的方法。

至此，今天的工作就完成的差不多了，好好休息一下，工作最多的两天已经过去了。冬天过去了，春天还会远吗？在正式结束今天的工作之前，请先看一下 **DAY 7** 中 **在Google Play上线** 第一小节的内容，我们有一项任务需要完成–注册Google Play开发者账号，因为GP对开发者账号的审核48小时(实际体验不需要那么久，大概24小时左右，看人品罗)，所以，咱们先做好准备工作吧。

### DAY 6

终于来到了Day 6，还有一天就要完成此次教程了。加油！

#### Day 6,文章收藏

我们在之前设计数据库时，就在表中插入了一个`bookmark`字段，用于标示当前一行是否被收藏。我们先看看如何添加收藏和取消收藏。

[DetailPresenter.java](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/detail/DetailPresenter.java)

```
if (queryIfIsBookmarked()) {
    // delete
    // update Zhihu set bookmark = 0 where zhihu_id = id
    ContentValues values = new ContentValues();
    values.put("bookmark", 0);
    dbHelper.getWritableDatabase().update(tmpTable, values, tmpId + " = ?", new String[]{String.valueOf(id)});
    values.clear();
} else {
    // add
    // update Zhihu set bookmark = 1 where zhihu_id = id
    ContentValues values = new ContentValues();
    values.put("bookmark", 1);
    dbHelper.getWritableDatabase().update(tmpTable, values, tmpId + " = ?", new String[]{String.valueOf(id)});
    values.clear();
}
```

那么如果在收藏页面中展示出来呢？套路，仍然是之前的套路。还是熟悉的配方，还是原来的味道。

布局文件也就是简单的一个列表，代码和之前首页的代码相同，不再写出。

[BookmarksContract.java](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/bookmarks/BookmarksContract.java)

```
public interface BookmarksContract {

    interface View extends BaseView<Presenter> {

		// 显示结果
        void showResults(ArrayList<ZhihuDailyNews.Question> zhihuList,
                         ArrayList<GuokrHandpickNews.result> guokrList,
                         ArrayList<DoubanMomentNews.posts> doubanList,
                         ArrayList<Integer> types);

		// 提示数据变化
        void notifyDataChanged();
        
		// 显示正在加载
        void showLoading();

		// 停止加载
        void stopLoading();

    }

    interface Presenter extends BasePresenter {

		// 请求结果
        void loadResults(boolean refresh);

		// 跳转到详情页面
        void startReading(BeanType type, int position);

		// 请求新数据
        void checkForFreshData();

		// 随便看看
        void feelLucky();

    }

}
```

[BookmarksFragment.java](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/bookmarks/BookmarksFragment.java)

```
public class BookmarksFragment extends Fragment
        implements BookmarksContract.View {

    private RecyclerView recyclerView;
    private SwipeRefreshLayout refreshLayout;
    private BookmarksAdapter adapter;
    private BookmarksContract.Presenter presenter;

    public BookmarksFragment() {}

    public static BookmarksFragment newInstance() {
        return new BookmarksFragment();
    }

    @Override
    public void onCreate(@Nullable Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
    }

    @Nullable
    @Override
    public View onCreateView(LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
        View view = inflater.inflate(R.layout.fragment_list, container, false);

        initViews(view);

        setHasOptionsMenu(true);

        presenter.loadResults(false);

        refreshLayout.setOnRefreshListener(new SwipeRefreshLayout.OnRefreshListener() {
            @Override
            public void onRefresh() {
                presenter.loadResults(true);
            }
        });

        return view;
    }

    @Override
    public void setPresenter(BookmarksContract.Presenter presenter) {
        if (presenter != null) {
            this.presenter = presenter;
        }
    }

    @Override
    public void initViews(View view) {

        recyclerView = (RecyclerView) view.findViewById(R.id.recyclerView);
        recyclerView.setLayoutManager(new LinearLayoutManager(getActivity()));

        refreshLayout = (SwipeRefreshLayout) view.findViewById(R.id.refreshLayout);
        refreshLayout.setColorSchemeResources(R.color.colorPrimary);

    }

    @Override
    public void onCreateOptionsMenu(Menu menu, MenuInflater inflater) {
        inflater.inflate(R.menu.menu_bookmarks, menu);
        super.onCreateOptionsMenu(menu, inflater);
    }

    @Override
    public boolean onOptionsItemSelected(MenuItem item) {
        int id = item.getItemId();
        if (id == R.id.action_search) {
            startActivity(new Intent(getActivity(), SearchActivity.class));
        } else if (id == R.id.action_feel_lucky) {
            presenter.feelLucky();
        }
        return true;
    }

    @Override
    public void showResults(ArrayList<ZhihuDailyNews.Question> zhihuList,
                            ArrayList<GuokrHandpickNews.result> guokrList,
                            ArrayList<DoubanMomentNews.posts> doubanList,
                            ArrayList<Integer> types) {

        if (adapter == null) {

            adapter = new BookmarksAdapter(getActivity(), zhihuList, guokrList, doubanList, types);
            adapter.setItemListener(new OnRecyclerViewOnClickListener() {
                @Override
                public void OnItemClick(View v, int position) {
                    int type = recyclerView.findViewHolderForLayoutPosition(position).getItemViewType();
                    if (type == BookmarksAdapter.TYPE_ZHIHU_NORMAL) {
                        presenter.startReading(BeanType.TYPE_ZHIHU, position);
                    } else if (type == BookmarksAdapter.TYPE_GUOKR_NORMAL) {
                        presenter.startReading(BeanType.TYPE_GUOKR, position);
                    } else if (type == BookmarksAdapter.TYPE_DOUBAN_NORMAL) {
                        presenter.startReading(BeanType.TYPE_DOUBAN, position);
                    }
                }
            });
            recyclerView.setAdapter(adapter);
        } else {
            adapter.notifyDataSetChanged();
        }

    }

    @Override
    public void notifyDataChanged() {
        presenter.loadResults(true);
        adapter.notifyDataSetChanged();
    }

    @Override
    public void showLoading() {
        refreshLayout.setRefreshing(true);
    }

    @Override
    public void stopLoading() {
        refreshLayout.setRefreshing(false);
    }

}
```

[BookmarksPresenter.java](https://github.com/TonnyL/PaperPlane/blob/master/app/src/main/java/com/marktony/zhihudaily/bookmarks/BookmarksPresenter.java)

```
public class BookmarksPresenter implements BookmarksContract.Presenter {

    private BookmarksContract.View view;
    private Context context;
    private Gson gson;

    private ArrayList<DoubanMomentNews.posts> doubanList;
    private ArrayList<GuokrHandpickNews.result> guokrList;
    private ArrayList<ZhihuDailyNews.Question> zhihuList;

    private ArrayList<Integer> types;

    private DatabaseHelper dbHelper;
    private SQLiteDatabase db;

    public BookmarksPresenter(Context context, BookmarksContract.View view) {
        this.context = context;
        this.view = view;
        this.view.setPresenter(this);
        gson = new Gson();
        dbHelper = new DatabaseHelper(context, "History.db", null, 5);
        db = dbHelper.getWritableDatabase();

        zhihuList = new ArrayList<>();
        guokrList = new ArrayList<>();
        doubanList = new ArrayList<>();

        types = new ArrayList<>();

    }

    @Override
    public void start() {

    }

    @Override
    public void loadResults(boolean refresh) {

        if (!refresh) {
            view.showLoading();
        } else {
            zhihuList.clear();
            guokrList.clear();
            doubanList.clear();
            types.clear();
        }

        checkForFreshData();

        view.showResults(zhihuList, guokrList, doubanList, types);

        view.stopLoading();

    }

    @Override
    public void startReading(BeanType type, int position) {
        Intent intent = new Intent(context, DetailActivity.class);
        switch (type) {
            case TYPE_ZHIHU:
                ZhihuDailyNews.Question q = zhihuList.get(position - 1);
                intent.putExtra("type", BeanType.TYPE_ZHIHU);
                intent.putExtra("id",q.getId());
                intent.putExtra("title", q.getTitle());
                intent.putExtra("coverUrl", q.getImages().get(0));
                break;

            case TYPE_GUOKR:
                GuokrHandpickNews.result r = guokrList.get(position - zhihuList.size() - 2);
                intent.putExtra("type", BeanType.TYPE_GUOKR);
                intent.putExtra("id", r.getId());
                intent.putExtra("title", r.getTitle());
                intent.putExtra("coverUrl", r.getHeadline_img());
                break;
            case TYPE_DOUBAN:
                DoubanMomentNews.posts p = doubanList.get(position - zhihuList.size() - guokrList.size() - 3);
                intent.putExtra("type", BeanType.TYPE_DOUBAN);
                intent.putExtra("id", p.getId());
                intent.putExtra("title", p.getTitle());
                if (p.getThumbs().size() == 0){
                    intent.putExtra("coverUrl", "");
                } else {
                    intent.putExtra("image", p.getThumbs().get(0).getMedium().getUrl());
                }
                break;
            default:
                break;
        }
        context.startActivity(intent);
    }

    @Override
    public void checkForFreshData() {

        // every first one of the 3 lists is with header
        // add them in advance

        types.add(TYPE_ZHIHU_WITH_HEADER);
        Cursor cursor = db.rawQuery("select * from Zhihu where bookmark = ?", new String[]{"1"});
        if (cursor.moveToFirst()) {
            do {
                ZhihuDailyNews.Question question = gson.fromJson(cursor.getString(cursor.getColumnIndex("zhihu_news")), ZhihuDailyNews.Question.class);
                zhihuList.add(question);
                types.add(TYPE_ZHIHU_NORMAL);
            } while (cursor.moveToNext());
        }

        types.add(TYPE_GUOKR_WITH_HEADER);
        cursor = db.rawQuery("select * from Guokr where bookmark = ?", new String[]{"1"});
        if (cursor.moveToFirst()) {
            do {
                GuokrHandpickNews.result result = gson.fromJson(cursor.getString(cursor.getColumnIndex("guokr_news")), GuokrHandpickNews.result.class);
                guokrList.add(result);
                types.add(TYPE_GUOKR_NORMAL);
            } while (cursor.moveToNext());
        }

        types.add(TYPE_DOUBAN_WITH_HEADER);
        cursor = db.rawQuery("select * from Douban where bookmark = ?", new String[]{"1"});
        if (cursor.moveToFirst()) {
            do {
                DoubanMomentNews.posts post = gson.fromJson(cursor.getString(cursor.getColumnIndex("douban_news")), DoubanMomentNews.posts.class);
                doubanList.add(post);
                types.add(TYPE_DOUBAN_NORMAL);
            } while (cursor.moveToNext());
        }

        cursor.close();

    }

    @Override
    public void feelLucky() {
        Random random = new Random();
        int p = random.nextInt(types.size());
        while (true) {
            if (types.get(p) == BookmarksAdapter.TYPE_ZHIHU_NORMAL) {
                startReading(BeanType.TYPE_ZHIHU, p);
                break;
            } else if (types.get(p) == BookmarksAdapter.TYPE_GUOKR_NORMAL) {
                startReading(BeanType.TYPE_GUOKR, p);
                break;
            } else if (types.get(p) == BookmarksAdapter.TYPE_DOUBAN_NORMAL) {
                startReading(BeanType.TYPE_DOUBAN, p);
                break;
            } else {
                p = random.nextInt(types.size());
            }
        }
    }

}
```

🤔，用户可以从收藏列表页面查看内容详情，可能在这里，用户取消了收藏，而我们就需要及时地刷新界面数据，否则就会影响用户的体验。

#### Day 6,夜间模式

关于夜间模式的实现，请查看文章[简洁优雅地实现夜间模式](https://tonnyl.github.io/2016/12/31/%E7%AE%80%E6%B4%81%E4%BC%98%E9%9B%85%E5%9C%B0%E5%AE%9E%E7%8E%B0%E5%A4%9C%E9%97%B4%E6%A8%A1%E5%BC%8F/)或者[这里](http://www.jianshu.com/p/dcfcfcbda7ac)。

#### Day 6,版本适配

事实上，版本适配的范围非常的广泛。例如，多语言适配，高低Android版本的适配，还有对多屏幕的适配，特殊用户的适配等等。

1. 多语言适配

   在`res`目录下新建不同的`values`目录，例如，需要适配英语就新建`values-en`目录，简体中文`values-zh-rCN`，繁体中文(台湾)`values-zh-rTW`，繁体中文(香港)`values-zh-rHK`，等等。(这些代码不区分大小写；r 前缀用于区分区域码。 不能单独指定区域。)

   ```
   .
   ├── app
   ├──├── res
   ├──  ── ├── values
   ├──  ──  ── ├── strings.xml
   ├──  ── ├── values-en
   ├──  ──  ── ├── strings.xml
   ├──  ── ├── values-zh-rCN
   ├──  ──  ── ├── strings.xml
   ├──  ── ├── values-zh-rHK
   ├──  ──  ── ├── strings.xml
   ```

```
![Multi-language](http://upload-images.jianshu.io/upload_images/2440049-ce4abc2ef966df77.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

更多内容请点击[这里](https://developer.android.com/training/basics/supporting-devices/languages.html)。

```

1. 高低Android版本适配

   关于Android碎片化的讨论已经非常多了，这里我就不再鞭尸了。只简单的举两个例子。在Android 4.1 Jelly Bean系统的华为手机上，系统的DatePicker的样式是这样的。

   ![JellyBean](https://ww2.sinaimg.cn/large/006tKfTcgy1fcity9flprj30dc0nqmxv.jpg)

   很明显，这和我们Material Design的设计语言很违和，使用开源库`materialdatetimepicker`就可以在这样的低版本的设备上实现MD版本的Date Picker Dialog。实现UI的统一对提升用户体验还是很有帮助的。

   对于高版本，例如Android 7.x Nougat，有很多的新特性，例如新的通知栏，Shortcuts等等，可以参考我的另外一篇文章[老司机带你吃牛轧糖–适配Android 7.1 Nougat新特性](http://www.jianshu.com/p/f56f2e709ad8)，虽然用上最新系统的用户量可能不大，但是当这些用户看到我们的应用适配了这些新特性时，应该也会感觉到眼前一亮吧，起到了锦上添花的作用。

2. 支持多种屏幕

   和多语言适配类似，我们也可以通过提供不同资源文件的方式实现适配。例如，以下应用资源目录 为不同屏幕尺寸和不同可绘制对象提供不同的布局设计。使用 mipmap/ 文件夹放置 启动器图标。

   ```
   res/layout/my_layout.xml              // layout for normal screen size ("default")
   res/layout-large/my_layout.xml        // layout for large screen size
   res/layout-xlarge/my_layout.xml       // layout for extra-large screen size
   res/layout-xlarge-land/my_layout.xml  // layout for extra-large in landscape orientation

   res/drawable-mdpi/graphic.png         // bitmap for medium-density
   res/drawable-hdpi/graphic.png         // bitmap for high-density
   res/drawable-xhdpi/graphic.png        // bitmap for extra-high-density
   res/drawable-xxhdpi/graphic.png       // bitmap for extra-extra-high-density

   res/mipmap-mdpi/my_icon.png         // launcher icon for medium-density
   res/mipmap-hdpi/my_icon.png         // launcher icon for high-density
   res/mipmap-xhdpi/my_icon.png        // launcher icon for extra-high-density
   res/mipmap-xxhdpi/my_icon.png       // launcher icon for extra-extra-high-density
   res/mipmap-xxxhdpi/my_icon.png      // launcher icon for extra-extra-extra-high-density
   ```

```
更多内容请点击[这里](https://developer.android.com/guide/practices/screens_support.html)。

```

1. 其他适配

   例如，[提升体验-支持Chrome Custom Tabs](http://www.jianshu.com/p/f952d9c71d91)。

哈，做完今天的工作，编码的工作就完成的差不多了。好好的睡一觉，准备明天的工作。

### DAY 7

#### Day 7,在Google Play上线

1. 注册Google Play开发者账号

   工具准备：

   - 科学上网，你懂的

   - Chrome浏览器或Firefox浏览器

   - $25，25刀的注册费用

   - 支持国际支付功能(VISA, Master等)的信用卡，便于支付25刀的注册费

     好了，我们现在开始正式的搞事情。

   1. 注册Google账号

      如果你已经有了Google账号，就直接跳过这一小步吧。

      我们先去 <https://accounts.google.com/SignUp> 注册账号。按照自身的信息填写即可。
      ![创建您的 Google 帐号](https://ww2.sinaimg.cn/large/006tKfTcgy1fcixwzqtxcj31hv23eqi8.jpg)

   2. 登录开发者后台

      登录 <https://play.google.com/apps/publish/signup/> 。
      ![Google Play Console](https://ww3.sinaimg.cn/large/006tKfTcgy1fciycauuj7j31kw106drh.jpg)

      勾选同意并点击继续付款。需要注意的是，我们要先进到付款页面，然后再绑定Google Wallet。否则的话，就不能保证付款成功了。

   3. 付款

      点击添加新的付款方式，一路按提示输入即可(由于我之前已经注册过了，这里盗用一下被人的图，原作者请不要打我😂)。
      ![Payment](https://ww3.sinaimg.cn/large/006tKfTcgy1fciyt3vjxuj310i0nhgnd.jpg)
      ![Payment](https://ww2.sinaimg.cn/large/006tKfTcgy1fciytuwh7wj310i0nhwg8.jpg)

      如果绑定成功，Google可能会先从信用卡中扣除$1进行授权。

   4. 审核

      Google最多需要48小时进行审核。我们可以通过[Google Wallet](https://wallet.google.com/)查看该订单的支付状态。如果显示`已完成`，就说明GP账号申请成功了。

   5. 没有信用卡怎么办？

      相信有很多像我一样的学生党，没有信用卡或者信用卡不支持国际支付功能，该怎么解决呢？这个时候，就是万能的某宝发挥作用的时候了。有一种信用卡叫做`虚拟信用卡`，我们可以通过向虚拟信用卡充值，然后用这样的卡去支付那25刀。具体的地址等咨询店小二即可。如果你觉得这样的方法太繁琐，或者我有钱任性，那么直接在马爸爸的网站上直接买一个开发者账号吧，不过一般情况下，费用肯定是高于25美刀的，而且安全性也值得检验(如果你打算买账号，那么务必在拿到账号之后第一时间修改密码和认证信息等等)。

      通过上面的步骤，我们申请到的账号还只能发布免费的应用。如果对应用进行收费，你可以查看控制台中`财务报表`，获取更多商家账户的信息。

      更多信息，请点击[这里](https://support.google.com/googleplay/android-developer/answer/6112435?hl=zh-Hans)。

2. 有了账号，我们就需要生成APK文件了。

   在保证项目正确运行的情况下(记得更换应用图标)，我们点击菜单项中的`Build` –> `Generate Signed APK...`，对APK进行签名。

   ![Generate Signed APK...](https://ww3.sinaimg.cn/large/006tKfTcgy1fcizu57nbqj30k40iygq3.jpg)

   选择生成APK的Module。

   ![app](https://ww4.sinaimg.cn/large/006tKfTcgy1fcizxsepj2j30uo0mqwh6.jpg)

   这时候需要我们选择key，用于对APK签名。

   ![key](https://ww1.sinaimg.cn/large/006tKfTcgy1fcizygdo80j30uo0mqgoz.jpg)

   Key的作用是为了保证每个应用程序开发商合法ID，防止部分开放商可能通过使用相同的Package Name来混淆替换已经安装的程序，我们需要对我们发布的APK文件进行唯一签名，保证我们每次发布的版本的一致性(如自动更新不会因为版本不一致而无法安装)。

   如果没有Key，我们就需要创建一个。选择`Create new...`创建。

   ![Create new key](https://ww4.sinaimg.cn/large/006tKfTcgy1fcj095fzroj30ze0vqdl0.jpg)

   各种信息对应如下：

   名称 | 描述
   — | —
   Key store path | key的存储路径
   Password | key的密码
   Confirm | 确认密码
   Alias | 别名
   Validity(years) | 有效期限(年)
   First and Last Name | 姓名
   Organizational Unit | 组织单位
   Organization | 组织
   City of Location | 所在城市
   State or Province | 省
   Country Code(XX) | 国家代码

   填写完信息后，点击OK生成。这里生成的key一定要妥善保管，以后我们对应用进行版本更新时，需要用到。

   新建成功后，我们选择刚刚生成的key，输入密码，点击`Next` –> `Finish`。

   ![Next](https://ww4.sinaimg.cn/large/006tKfTcgy1fcj0nfxqiaj30uo0mqtcd.jpg)

   ![Finish](https://ww3.sinaimg.cn/large/006tKfTcgy1fcj0o691u1j30uo0mqadm.jpg)

3. 上传应用

   现在我们就可以把应用上传到Google Play了。

   3.1. 添加APK

   - 3.1.1. 转到 [Google Play Developer Console](https://play.google.com/apps/publish/)。

   - 3.1.2. 依次选择`所有应用` –> `+ 创建应用`。

     ![Google Play Console](https://ww1.sinaimg.cn/large/006tKfTcgy1fcj10u1etcj31kw08y40b.jpg)

   - 3.1.3 使用下拉菜单选择默认语言，并为您的应用添加标题。输入您想要在 Google Play 中显示的应用名称。

     ![Create new app](https://ww2.sinaimg.cn/large/006tKfTcgy1fcj1bo68lpj31kw106aj8.jpg)

   - 3.1.4 选择上传 APK。

     ![img](https://ww4.sinaimg.cn/large/006tKfTcgy1fcj1k3x3yuj31kw10648b.jpg)

     3.2. 设置商品详情

     ![img](https://ww3.sinaimg.cn/large/006tKfTcgy1fcj1h9a33lj31kw106k1i.jpg)

     我们需要为我们的应用设置`商品详情`，`图片资源`，`语言和翻译`，`分类`，`详细联系信息`，`隐私权政策`，等。对于程序员来说，最困难的应该就是各种图片了吧，在没有设计师的情况下，就让我们程序员发挥灵魂画师的功力吧，哈哈😆。

     3.3. 后续步骤

     我们还需要完成的步骤有：

   - 填写应用的内容分级问卷

   - 了解如何将应用发布到不同的国家/地区以及Android计划

   - 使用标准或定时发布应用

   - 通过实验优化商品详情

     更多信息，请点击[这里](https://support.google.com/googleplay/android-developer/answer/113469?hl=zh-Hans)。

     哈，到这里，应用上传就完成了，现在等待应用发布审核成功就好了。

#### Day 7,在GitHub开源

1. 注册GitHub

   [GitHub](https://github.com/)是一个 ~~同性交友社区~~ 面向开源及私有软件项目的托管平台，作为开源代码库以及版本控制系统，Github拥有超过900万开发者用户。随着越来越多的应用程序转移到了云上，Github已经成为了管理软件开发以及发现已有代码的首选方法。

   我们先注册账号，地址为: [https://github.com](https://github.com/) 。

   ![GitHub](https://ww4.sinaimg.cn/large/006y8lVagy1fcj2es7qjbj31kw106x6p.jpg)

   账号注册成功后，进入 ~~GayHub~~ GitHub 个人信息页，大概是这个样子的。

   ![MyGitHub](https://ww1.sinaimg.cn/large/006y8lVagy1fcj2kj9rahj31kw106gxt.jpg)

   第一步的工作就完成了。

2. 安装Git

   [Git](https://git-scm.com/)是一款免费、开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。官网的介绍是这样的：

   > Git is a free and open source distributed version control system designed to handle everything from small to very large projects with speed and efficiency.

   下载Git，地址为 <https://git-scm.com/downloads> ，下载对应版本即可。

   - 在macOS上，在 Mac 上安装 Git 有多种方式。

     - 最简单的方法是安装 Xcode Command Line Tools。 Mavericks （10.9） 或更高版本的系统中，在 Terminal 里尝试首次运行 git 命令即可。 如果没有安装过命令行开发者工具，将会提示你安装。
     - 如果你想安装更新的版本，可以使用二进制安装程序。 官方维护的 OSX Git 安装程序可以在 Git 官方网站下载，网址为 [http://git-scm.com/download/mac。](http://git-scm.com/download/mac%E3%80%82)

   - 在Windows上，在 Windows 上安装 Git 也有几种安装方法。

     - 官方版本可以在 Git 官方网站下载。 打开 [http://git-scm.com/download/win，下载会自动开始。](http://git-scm.com/download/win%EF%BC%8C%E4%B8%8B%E8%BD%BD%E4%BC%9A%E8%87%AA%E5%8A%A8%E5%BC%80%E5%A7%8B%E3%80%82) 要注意这是一个名为 Git for Windows的项目（也叫做 msysGit），和 Git 是分别独立的项目；更多信息请访问 [http://msysgit.github.io/。](http://msysgit.github.io/%E3%80%82)
     - 另一个简单的方法是安装 GitHub for Windows。 该安装程序包含图形化和命令行版本的 Git。 它也能支持 Powershell，提供了稳定的凭证缓存和健全的 CRLF 设置。 稍后我们会对这方面有更多了解，现在只要一句话就够了，这些都是你所需要的。 你可以在 GitHub for Windows 网站下载，网址为 [http://windows.github.com。](http://windows.github.com./)

   - 在Linux上，我们可以通过下面的方法安装。

     如果你想在 Linux 上用二进制安装程序来安装 Git，可以使用发行版包含的基础软件包管理工具来安装。

     - 如果以 Fedora 上为例，你可以使用 yum:

       ```
       $ sudo yum install git
       ```

`    - 如果你在基于 Debian 的发行版上，请尝试用 apt-get：        $ sudo apt-get install git    要了解更多选择，Git 官方网站上有在各种 Unix 风格的系统上安装步骤，网址为 http://git-scm.com/download/linux。更多信息，请点击[这里](https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%AE%89%E8%A3%85-Git)。OK，我们可以测试一下Git是否安装成功。在命令行中输入命令`git --version`查看git 的版本信息：![Git Version](https://ww4.sinaimg.cn/large/006y8lVagy1fcj3gn7ux4j312q0qk77y.jpg)`

1. 在Android Studio中配置Git和GitHub

   1. 打开Android Stuido，进入`Android Studio` –> `Preferences` –> `Version Control` –> `Git`，(Windows为`File` –> `Settings` –> `Version Control` –> `Git`)，在`Path to Git executable`中定位到你的Git安装目录，然后点击Test，如果成功你将会看到下面的提示信息。

      ![Init Git](https://ww3.sinaimg.cn/large/006y8lVagy1fcj46ixzqmj31kw131ah9.jpg)

      ![Git test success](https://ww1.sinaimg.cn/large/006tNc79gy1fcj497m0pqj30oo0a4t9s.jpg)

   2. 然后在左侧设置项中选择GitHub，然后输入你刚刚注册好的GitHub账号信息，点击test，如果成功你将会看到下面的提示信息。

      ![Init GitHub](https://ww4.sinaimg.cn/large/006tNc79gy1fcj4gvunlbj31kw13144p.jpg)

      ![GitHub test success](https://ww1.sinaimg.cn/large/006tNc79gy1fcj4j85oz5j30oo0a474y.jpg)

2. 托管代码

   1. 为当前工程创建一个实用且漂亮的`README.MD`文件吧。

      在项目根目录下新建一个`README.MD`文件，`MD`表示这是一份`Markdown`文件。

      ![Add a readme file](https://ww2.sinaimg.cn/large/006tNc79gy1fcj55v5oqlj30lw0ls413.jpg)

      `README`文件作为说明文件，作用是让浏览者能够快速地了解项目。
      因此，我们在写作README时，应该包括以下几点：

      - 为什么会有这个项目，介绍项目开发的背景
      - 项目的用途是什么，介绍项目所解决的问题
      - 怎样使用该项目
      - 项目的开发历程，版本变化(可选)
      - 未来的开发计划(可选)
      - Q&A(可选)
      - 项目所使用的许可条款文件

      (我的建议是提供一份英文版的README.MD文档，让我们的项目不仅仅帮助同胞，也帮助歪果仁吧。)

   2. 将当前工程导入版本控制，创建Git仓库(可选)

      ![Create Git Repository](https://ww1.sinaimg.cn/large/006tNc79gy1fcj4ql3vs3j30yi0okwmo.jpg)

   3. 分享到GitHub上

      ![Share Project on GitHub](https://ww3.sinaimg.cn/large/006tNc79gy1fcj5jifdx3j30y20q0n5l.jpg)

      ![Share Project on GitHub](https://ww2.sinaimg.cn/large/006tNc79gy1fcj5o47zsqj30tm0jcq60.jpg)

      然后我们就可以在GitHub的网站上看到我们的项目了。下面是我的[纸飞机](https://github.com/TonnyL/PaperPlane)的项目主页。

      ![GitHub Repository](https://ww2.sinaimg.cn/large/006tNc79gy1fcj5vq0sqaj31kw106qfk.jpg)

#### Day 7,Q&A

至此，项目完成，教程也接近尾声。泡杯咖啡，我们来聊聊代码之外的事情。

- Q: 为什么会有这篇文章？
- A: 一方面是受到各种大牛的影响，迫切地想要为开源贡献自己的力量；另一方面，[纸飞机](https://github.com/TonnyL/PaperPlane)项目的维护时间已经接近一年，这篇文章也算是一个小小的总结；然后是希望通过我的文章，能够让后面的童鞋们少踩一些坑。
- Q: 一周时间并没有完成项目，怎么办？
- A: 项目的代码量还是很大的，而项目现在的代码也是我用MVP架构重构之后的。就我自己而言，理解MVP架构我就花了一段时间，而且，MVP较MVC，代码量本身也是增加的。没有完成的话，就多花点时间吧。(文章的标题似乎有点标题党的嫌疑呢)
- Q: 版权问题？
- A: 恩，上线未经版权所有方如知乎等的许可，我们的确是侵权了。所以，请务必知晓可能承担的后果。(貌似是挖了个坑呢😂)
- Q: 为什么是Google Play，而不是几60应用商店，某度应用商店呢？
- A: 瞧不上。(我不是针对在座的某一个应用商店，我是说在座的各个应用商店，除了Google Play，都是那啥)
- Q: 我有问题需要探讨，怎么联系？
- A:
  - marktonymengyi#gmail.com
  - [知乎](https://www.zhihu.com/people/kirin-momo)
  - [微博](http://weibo.com/5313690193/profile?topnav=1&wvr=6)
  - [GitHub](https://github.com/TonnyL)
  - 我的个人博客地址:[https://TonnyL.github.io/](https://tonnyl.github.io/)
- Q: 最后有什么想说的？
- A: 如果文章对你有帮助的话，请给文章点一个赞，或者给项目一个Star，土豪请随意打赏，集齐30块钱我想要买本关于Git的书😂。(如果有大牛有实习机会的话，请推荐一下我呀)

感谢您的阅读~~~

本文由[TonnyL](http://www.jianshu.com/u/49606f4d970f)原创，转载请注明作者及出处。