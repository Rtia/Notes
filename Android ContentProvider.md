

# 【Android ContentProvider】

## 概述
**ContentProvider内容提供者**是Android系统**四大组件之一**，用于**保存和检索数据**，是Android系统中**不同应用程序之间共享数据的接口**。

在Android系统中，**应用程序之间**是**相互独立**的，分别运行在自己的进程中，相互之间没有数据交换，如果应用程序之间需要**共享或交换数据**，就需要**用内容提供者（ContentProvider）**。

ContentProvider是不同应用程序之间进行数据交换的标准API，它**以Uri的形式对外提供数据**，允许其它应用程序操作本应用的数据，**其它应用**程序能**通过ContentResolver**，并根据ContentProvider提供的**Uri** **操作**指定的**数据**。


### 为何使用ContentProvider

ContentProvider一般为存储和获取数据提供统一的接口，可以在不同的应用程序之间共享数据。

之所以使用ContentProvider，主要有以下几个理由：

1. ContentProvider**提供了对底层数据存储方式的抽象**。
  比如下图中，底层使用了SQLite数据库，在用了ContentProvider封装后，即使你把数据库换成MongoDB，也不会对上层数据使用层代码产生影响。

- ![](http://upload-images.jianshu.io/upload_images/9028834-e28fd0ee96960332.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. Android框架中的一些类**需要ContentProvider类型数据**。如果你想让你的数据可以使用在如SyncAdapter, Loader, CursorAdapter等类上，那么你就需要为你的数据做一层ContentProvider封装。

3. 第三个原因也是最主要的原因，是ContentProvider**为应用间的数据交互提供了一个安全的环境**。它准许你把自己的应用数据根据需求开放给其他应用进行增、删、改、查，而不用担心直接开放数据库权限而带来的安全问题。

我们知道了ContentProvider是对数据层的封装后，那么大家可能会问我们要如何对ContentProvider进行增，删，改，查的操作呢？下面我们来介绍一个新的类ContentResolver，我们可以通过它，来对不同的ContentProvider进行操作。

### 为何使用ContentResolver
有些人可能会疑惑，为什么我们不直接访问Provider，而是又**在上面加了一层ContentResolver来进行对其的操作**，这样岂不是更复杂了吗？其实不然，大家要知道一台手机中可不是只有一个Provider内容，它可能安装了很多含有Provider的应用，比如联系人应用，日历应用，字典应用等等。
有如此多的Provider，如果你开发一款应用要使用其中多个，如果让你去了解每个ContentProvider的不同实现，岂不是要头都大了。
所以Android为我们提供了**ContentResolver**来**统一管理与不同ContentProvider间的操作**。
[![](http://upload-images.jianshu.io/upload_images/9028834-5585d3e9a4406738.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://upload-images.jianshu.io/upload_images/9028834-5585d3e9a4406738.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在Context.java的源码中有一段
```java
/** Return a ContentResolver instance for your application's package. */
 public abstract ContentResolver getContentResolver();
```

所以我们可以通过在**所有继承Context的类**中通过调用**getContentResolver**()来获得ContentResolver。

那ContentResolver是如何来区别不同的ContentProvider的呢？这就涉及到URI（Uniform Resource Identifier）。

## ContentProvider及其URI

### 引入ContentProvider的URI

思考两应用间如何通信，首先，我们最先想到的方法应该是如下面这张图这样的，自身应用写好各个数据库接口，供其它应用调用

![](http://upload-images.jianshu.io/upload_images/9028834-1c476e9d28a1873d?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

那么，问题来了，首先，其它应用如何调起这个应用的数据库接口；要知道你要调这个接口的应用很可能根本就没有运行。

咦，好像有一种方法就能**调起没有运行的应用**——**隐式Intent**！！！

隐式Intent的调用方法如下，如果这个Activity要供其它应用调用，那么这个Activity在这个应用AndroidManifest.xml中声明方式为：

```xml
<activity  
    android:name=".SecondActivity"  
    android:label="@string/title_activity_second" >  
    <intent-filter>  
        <action android:name="harvic.test.qijian"/>  
        <category android:name="android.intent.category.DEFAULT"/>  
    </intent-filter>  
</activity>  
```

在其它APP中，要调这个Activity时，就使用：

```java
Intent intent = new Intent("harvic.test.qijian");  
startActivity(intent);  
```

我们先分析一下**隐式Intent是如何做到**的：
1. 首先，我们将filter信息写在了AndroidManifest.xml中；

2. 当有一个隐式Intent要匹配时，系统（注意是系统！）会搜索整个手机上所有APP的Acitivity进行匹配，如果有匹配的并且有使用权限的，就起起来；大家想想当你在应用中点击网址链接的时候，是不是会转到浏览器中，这就是用的隐式Intent匹配。如果你手机上有多个应用可以匹配，那么就会以列表的形式列出来供你选择开哪一个。具体有关隐式Intent的内容，可以参看[《Intent详解一》](http://blog.csdn.net/harvic880925/article/details/38399723)和[《Intent详解二》](http://blog.csdn.net/harvic880925/article/details/38406421)

根据隐式Intent，我们是不是有受到启发，那我们也可额外再开出来一个东东，**仿照隐式Intent**的方式来进行**全局匹配**，如果匹配成功就执行操作。对，这就是那帮谷歌老头的设计方法，他们设计的东东就是**ContentProvider的URI**。

### ContentProvider与对应的URI
ContentProvider中的URI有固定格式，如下图：
![](http://upload-images.jianshu.io/upload_images/9028834-c913fb02ee92bafd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
主要分三个部分：scheme, authority and path。
Scheme表示上图中的content://，Authority表示B部分，Path表示C和D部分。 
- **Scheme：**表示是一个Android内容URI，说明由ContentProvider控制数据，该部分是**固定形式**携程**`content://`**，不可更改的。 
- **Authority：**是URI的授权部分，是**唯一标识符**，用来定位区别不同ContentProvider。
  格式一般是自定义**ContentProvider类**的**完全限定名**称，注册时需要用到。
  如：com.example.transportationprovider。
- **Path：**是每个**ContentProvider内部的路径**部分，C和D部分称为路径片段。
  **C部分**指向一个对象集合，一般用**表**的**名**字，如：/trains表示一个笔记集合；
  **D部分**指向特定的记录，如：/trains/122表示**id**为122的单条记录，如果没有指定D部分，则返回全部记录。
  表名，用以区分ContentProvider中不同的数据表；



### ContentProvider声明方式
上面这个URI就是用来进行全局匹配的，那AndroidManifest.xml里又要怎么写呢？
再回想下隐式Intent，在隐式Intent中，Intent-fliter是Activity的一部分，专门过滤隐式Intent的匹配请求的，如果Intent-fliter匹配后，就启动对应的Activity；
那我们也可以设计一个东东，把这个URI作为它的一部分，当匹配成功以后，就进这个东东里操作数据库。这个东东就是ContentProvider。

**ContentProvider在AndroidManifest.xml中的声明方式**为：（与上面的URI对应）

```xml
<provider  
    android:name=".NoteContentProvider"  
    android:authorities="com.example.transportationprovider"  
    android:exported="true"/>  
```

这里的**android:authorities**必须**与上面URI中的B部分一样**，因为这个就是用来全局匹配的authority。只有URI的authority与provider的android:authorities匹配上了，才会进入后面的操作。

### **ContentURI的全局流程图**

![](http://upload-images.jianshu.io/upload_images/9028834-ca57f7fb226300f7?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上面，我们说到了第一步和第二步，当匹配成功ContentProvider以后，就开始进入我们指定的ContentProvider进行处理；大家估计也注意到了，我们的ContentURI的完整部分为：content://com.example.transportionprovider/trains/122

到第二步，我们已经匹配到了content://com.example.transportionprovider，那后面的/trains/122的匹配工作就只有交由ContentProvider来处理了。

**在ContentProvder中的匹配是利用UriMatcher来完成的**。

### UriMatcher

UriMatcher的匹配工作的第一步就是先将所需要的匹配的URI使用addURI()添加到UriMatcher中，

```java
/*authority:就是URI对应的authority
path:就是我们在URI中 authority后的那一串
code:表示匹配成功以后的返回值；*/
public void addURI(String authority, String path, int code)  
```



下面以我们的URI=content://com.example.transportionprovider/trains/122来演示一下

```java
public static final String AUTHORITY = "com.alexzhou.provider.NoteProvider";  
private static final UriMatcher sUriMatcher;  
static {  
    sUriMatcher = new UriMatcher(UriMatcher.NO_MATCH);  
    sUriMatcher.addURI(AUTHORITY, "trains", 1);  //表示匹配content://com.example.transportionprovider/trains，如果匹配成功返回1
    sUriMatcher.addURI(AUTHORITY, "trains/#", 2);  //#表示匹配任意数据ID *表示匹配任意文本
}  
```
所以这句的意思就是匹配content://com.example.transportionprovider/trains/任意数字ID  比如我们的content://com.example.transportionprovider/trains/122

## ContentResolver
第三方应用如何根据URI来指定操作的，是哪个函数来操作URI的呢？

它就是ContentResolver；

下面简单先说一下ContentResolver的函数：

插入（insert）：

```java
//在第三方中，我们要向指定应用的数据库中插入一条记录，其中title字段的值为hello,content字段的值为my name is harvic。代码如下：
String CONTENT_URI = "content://com.example.transportionprovider/trains/122;"  
ContentResolver cr =getContentResolver();  
ContentValues values = new ContentValues();  
values.put("title", "hello");//数据库的键值对  
values.put("content", "my name is harvic");  
  
Uri uri = cr.insert(CONTENT_URI, values);  
```



这段代码一调用，那可就吊了，系统会搜索手机上所有APP的AndroidManifest.xml，看哪个provider的authority匹配，在匹配之后，转到对应的类中，再让UriMatcher匹配后面的PATH字段，都完全匹配之后，就执行ContentProvider中的insert方法。

当然ContentResolver除了insert方法还有query()、update()、delete()方法，可以执行第三方数据库的任何操作。

## 使用步骤
### ContentProvider提供数据库查询接口

#### 1、利用SQLiteOpenHelper创建数据库、数据表

在这里，我们在一个数据库(“harvic.db”)中创建两个数据表”first”与”second”;每个表都有多出一个字段“table_name”，来保存当前数据表的名称 
代码如下：

```java
public class DatabaseHelper extends SQLiteOpenHelper {  
    public static final String DATABASE_NAME = "harvic.db";  
    public static final int DATABASE_VERSION = 1;  
    public static final String TABLE_FIRST_NAME = "first";  
    public static final String TABLE_SECOND_NAME = "second";  
    public static final String SQL_CREATE_TABLE_FIRST = "CREATE TABLE " +TABLE_FIRST_NAME +"("  
            + BaseColumns._ID + " INTEGER PRIMARY KEY AUTOINCREMENT,"  
            + "table_name" +" VARCHAR(50) default 'first',"  
            + "name" + " VARCHAR(50),"  
            + "detail" + " TEXT"  
            + ");" ;  
    public static final String SQL_CREATE_TABLE_SECOND = "CREATE TABLE "+TABLE_SECOND_NAME+" ("  
            + BaseColumns._ID + " INTEGER PRIMARY KEY AUTOINCREMENT,"  
            + "table_name" +" VARCHAR(50) default 'second',"  
            + "name" + " VARCHAR(50),"  
            + "detail" + " TEXT"  
            + ");" ;  
  
    public DatabaseHelper(Context context) {  
        super(context, DATABASE_NAME, null, DATABASE_VERSION);  
    }  
  
    @Override  
    public void onCreate(SQLiteDatabase db) {  
        Log.e("harvic", "create table: " + SQL_CREATE_TABLE_FIRST);  
        db.execSQL(SQL_CREATE_TABLE_FIRST);  
        db.execSQL(SQL_CREATE_TABLE_SECOND);  
    }  
  
    @Override  
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {  
        db.execSQL("DROP TABLE IF EXISTS first");  
        db.execSQL("DROP TABLE IF EXISTS second");  
        onCreate(db);  
    }  
}  
```

#### 2、利用ContentProvider提供数据库操作接口
新建一个类PeopleContentProvider，派生自ContentProvider基类，写好之后，就会自动生成query(),insert(),update(),delete()和getType()方法；这些方法就是根据传过来的URI来操作数据库的接口。此时的PeopleContentProvider是这样的：

```java
public class PeopleContentProvider extends ContentProvider {  
    @Override  
    public boolean onCreate() {  
        return false;  
    }  
  
    @Override  
    public Cursor query(Uri uri, String[] projection, String selection, String[] selectionArgs, String sortOrder) {  
        return null;  
    }  
  
    @Override  
    public String getType(Uri uri) {  
        return null;  
    }  
  
    @Override  
    public Uri insert(Uri uri, ContentValues values) {  
        return null;  
    }  
  
    @Override  
    public int delete(Uri uri, String selection, String[] selectionArgs) {  
        return 0;  
    }  
  
    @Override  
    public int update(Uri uri, ContentValues values, String selection, String[] selectionArgs) {  
        return 0;  
    }  
```

#### 3、使用UriMatcher匹配URI

我们先不管这几个函数的具体操作，先想想在上节中我提到的当一个URI逐级匹配到了ContentProvider类里以后，会怎么做——利用UriMatcher进行再次匹配。
**UriMatcher匹配成功以后，才会执行对应的操作**。所以上面的那些操作是在UriMatcher匹配之后。 
好，我们就先看看UriMatcher是怎么匹配的。

```java
public class PeopleContentProvider extends ContentProvider {  
    private static final UriMatcher sUriMatcher;  
    private static final int MATCH_FIRST = 1;  
    private static final int MATCH_SECOND = 2;  
    public static final String AUTHORITY = "com.harvic.provider.PeopleContentProvider";  
    public static final Uri CONTENT_URI_FIRST = Uri.parse("content://" + AUTHORITY + "/frist");  
    public static final Uri CONTENT_URI_SECOND = Uri.parse("content://" + AUTHORITY + "/second");  
  
    //UriMatcher
    static {  
        //如果匹配不成功，返回UriMatcher.NO_MATCH
        sUriMatcher = new UriMatcher(UriMatcher.NO_MATCH);  
        sUriMatcher.addURI(AUTHORITY, "first", MATCH_FIRST);//传来content://com.harvic.provider.PeopleContentProvider/frist时，就返回code：MATCH_FIRST  
        sUriMatcher.addURI(AUTHORITY, "second", MATCH_SECOND);
    }  
  
    private DatabaseHelper mDbHelper;  
  
    @Override  
    public boolean onCreate() {  
        mDbHelper = new DatabaseHelper(getContext());  
        return false;  
    }  
    …………  
}  
```

上面的代码是最重要的一句话：

```java
sUriMatcher.addURI(AUTHORITY, "first", MATCH_FIRST);  
```
##### **addUri**
addUri的官方声明为：
```java
public void addURI (String authority, String path, int code)  
```

*   **authority：**这个参数就是ContentProvider的authority参数，这个参数必须与AndroidManifest.xml中的对应provider的authorities值一样；
*   **path:**就匹配在URI中authority后的那一坨，在这个例子中，我们构造了两个URI 
    (1)、content://com.harvic.provider.PeopleContentProvider/frist 
    (2)、content://com.harvic.provider.PeopleContentProvider/second 
    而path匹配的就是authority后面的/first或者/second
*   **code：**这个值就是在匹配path后，返回的对应的数字匹配值;

#### 4、insert方法
下面先看看insert方法,主要功能为：
当URI匹配content://com.harvic.provider.PeopleContentProvider/frist时，将数据插入first数据库；
当URI匹配content://com.harvic.provider.PeopleContentProvider/second时，将数据插入second数据库。

```java
@Override  
public Uri insert(Uri uri, ContentValues values) {  
    SQLiteDatabase db = mDbHelper.getWritableDatabase();  
    switch (sUriMatcher.match(uri)){  //获取上面设定sUriMatcher时，sUriMatcher.addURI (String authority, String path, int code)中对应返回的code
        case MATCH_FIRST:{  
            long rowID = db.insert(DatabaseHelper.TABLE_FIRST_NAME, null, values);//将数据键值对（values）插入到first表中。插入之后，会返回新插入记录的当前所在行号
            if(rowID > 0) {  
                Uri retUri = ContentUris.withAppendedId(CONTENT_URI_FIRST, rowID);
                return retUri;  
            }  
        }  
        break;  
        case MATCH_SECOND:{  
            long rowID = db.insert(DatabaseHelper.TABLE_SECOND_NAME, null, values);  
            if(rowID > 0) {  
                Uri retUri = ContentUris.withAppendedId(CONTENT_URI_SECOND, rowID);  
                return retUri;  
            }  
        }  
        break;  
        default:  
            throw new IllegalArgumentException("Unknown URI " + uri);  
    }  
    return null;  
}  
```

#### 5、update方法

在看了insert方法之后，update方法难度也不大，也是根据UriMatcher.match(uri)的返回值来判断当前与哪个URI匹配，根据匹配的URI来操作对应的数据库，代码如下：

```java
public int update(Uri uri, ContentValues values, String selection, String[] selectionArgs) {  
    SQLiteDatabase db = mDbHelper.getWritableDatabase();  
    int count = 0;  
    switch(sUriMatcher.match(uri)) {  
        case MATCH_FIRST:  
            count = db.update(DatabaseHelper.TABLE_FIRST_NAME, values, selection, selectionArgs);  
            break;  
        case MATCH_SECOND:  
            count = db.update(DatabaseHelper.TABLE_SECOND_NAME, values, selection, selectionArgs);  
            break;  
  
        default:  
            throw new IllegalArgumentException("Unknow URI : " + uri);  
    }  
    this.getContext().getContentResolver().notifyChange(uri, null);//通知当前的数据库有改变，让监听这个数据库的所有应用执行对应的操作
    return count;  
}  
```
在最后调用**getContentResolver().notifyChange**(uri, null);来**通知当前的数据库有改变**，让监听这个数据库的所有应用执行对应的操作。

#### 6、query方法

至于query()方法就不再细讲了，跟上面的一样，根据不同的URI来操作不同的查询操作而已，代码如下：

```java
public Cursor query(Uri uri, String[] projection, String selection, String[] selectionArgs, String sortOrder) {  
    SQLiteQueryBuilder queryBuilder = new SQLiteQueryBuilder();  
    switch (sUriMatcher.match(uri)) {  
        case MATCH_FIRST:  
            // 设置查询的表  
            queryBuilder.setTables(DatabaseHelper.TABLE_FIRST_NAME);  
            break;  
  
        case MATCH_SECOND:  
            queryBuilder.setTables(DatabaseHelper.TABLE_SECOND_NAME);  
            break;  
  
        default:  
            throw new IllegalArgumentException("Unknow URI: " + uri);  
    }  
  
    SQLiteDatabase db = mDbHelper.getReadableDatabase();  
    Cursor cursor = queryBuilder.query(db, projection, selection, selectionArgs, null, null, null);  
    return cursor;  
}  
```

#### 7、delete方法

delete()方法如下：

```java
public int delete(Uri uri, String selection, String[] selectionArgs) {  
    SQLiteDatabase db = mDbHelper.getWritableDatabase();  
    int count = 0;  
    switch(sUriMatcher.match(uri)) {  
        case MATCH_FIRST:  
            count = db.delete(DatabaseHelper.TABLE_FIRST_NAME, selection, selectionArgs);  
            break;  
  
        case MATCH_SECOND:  
            count = db.delete(DatabaseHelper.TABLE_SECOND_NAME, selection, selectionArgs);  
            break;  
        default:  
            throw new IllegalArgumentException("Unknow URI :" + uri);  
  
    }  
    return count;  
}  
```

#### 8、getType()

这个函数，我们这里暂时用不到，直接返回NULL，下篇我们会专门来讲这个函数的作用与意义。（[ContentProvider中的getType()](#ContentProvider中的getType)）

```java
public String getType(Uri uri) {  
    return null;  
}  
```

#### 9、AndroidManifest.xml中声明provider

在AndroidManifest.xml中要声明我们创建的Provider:

```xml
<provider  
    android:authorities="com.harvic.provider.PeopleContentProvider"  
    android:name=".PeopleContentProvider"  
    android:exported="true"/>  
```

### 第三方应用通过URI操作共享数据库

#### 1、ContentResolver操作URI

在第三方应用中，我们要如何利用URI来执行共享数据数的操作呢，这是利用ContentResolver这个类来完成的。
获取ContentResolver实例的方法为：
```java
ContentResolver cr = this.getContentResolver();  
```

ContentResolver有下面几个数据库操作：查询、插入、更新、删除
```java
public final Cursor query (Uri uri, String[] projection, String selection, String[] selectionArgs, String sortOrder)  
public final Uri insert (Uri url, ContentValues values)  
public final int update (Uri uri, ContentValues values, String where, String[] selectionArgs)  
public final int delete (Uri url, String where, String[] selectionArgs)  
```

第一个参数都是传入要指定的URI，然后再后面的几个参数指定数据库条件——where语句和对应的参数；下面我们就用户实例来看看这几个函数到底是怎么用的。

#### 2、全局操作

新建一个工程，命名为“UseProvider”，界面长这个样子：

![image](http://upload-images.jianshu.io/upload_images/9028834-c04631d0858b971f?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

最上头有两个按钮，用来切换当前使用哪个URI来增、删、改、查操作；由于不同的URI会操作不同的数据表，所以我们使用不同的URI，会在不同的数据表中操作；
先列出来那两个要匹配的URI，以及全局当前要使用的URI(mCurrentURI ),mCurrentURI 默认是/first对应的URI，如果要切换，使用界面上最上头的那两个按钮来切换当前所使用的URI。

```java
public static final String AUTHORITY = "com.harvic.provider.PeopleContentProvider";  
public static final Uri CONTENT_URI_FIRST = Uri.parse("content://" + AUTHORITY + "/first");  
public static final Uri CONTENT_URI_SECOND = Uri.parse("content://" + AUTHORITY + "/second");  
public static Uri mCurrentURI = CONTENT_URI_FIRST;  
```

#### 3、query()查询操作

下面先来看看查询操作的过程：

```java
private void query() {  
    Cursor cursor = this.getContentResolver().query(mCurrentURI, null, null, null, null);//由于没有加任何的限定语句，所以是查询出此数据表中的所有记录  
    Log.e("test ", "count=" + cursor.getCount());  
    //使用返回的数据库记录指针Cursor，逐个读出所有的记录
    cursor.moveToFirst();  
    while (!cursor.isAfterLast()) {  
        String table = cursor.getString(cursor.getColumnIndex("table_name"));  
        String name = cursor.getString(cursor.getColumnIndex("name"));  
        String detail = cursor.getString(cursor.getColumnIndex("detail"));  
        Log.e("test", "table_name:" + table);  
        Log.e("test ", "name: " + name);  
        Log.e("test ", "detail: " + detail);  
        cursor.moveToNext();  
    }  
    cursor.close();  
}  
```

#### 4、insert()插入操作

```java
public final Uri insert (Uri url, ContentValues values)//第二个函数要求传入要插入的ContentValues的键值对
```

```java
private void insert() {  
    ContentValues values = new ContentValues();  
    values.put("name", "hello");  
    values.put("detail", "my name is harvic");  
    Uri uri = this.getContentResolver().insert(mCurrentURI, values);  
    Log.e("test ", uri.toString());  
}  
```

#### 5、update()更新操作

```java
public final int update (Uri uri, ContentValues values, String where, String[] selectionArgs)  
```

这个函数的意思就是，先用where语句找出要更新的记录，然后将values键值对更新到对应的记录中；

*   **uri**:即要匹配的URI
*   **values**:这是要更新的键值对
*   **where**:SQL中对应的where语句
*   **selectionArgs**:where语句中如果有可变参数，可以放在selectionArgs这个字符串数组中；这些与数据库中的用法一样。

```java
//将_id = 1的记录的detail字符更新
private void update() {  
    ContentValues values = new ContentValues();  
    values.put("detail", "my name is harvic !!!!!!!!!!!!!!!!!!!!!!!!!!");  
    int count = this.getContentResolver().update(mCurrentURI, values, "_id = 1", null);  
    Log.e("test ", "count=" + count);  
    query();  
}  
```
#### 6、delete()删除操作

```java
public final int delete (Uri url, String where, String[] selectionArgs)  
```
第二个参数是SQL中的WHERE语句的过滤条件，selectionArgs同样是Where语句中的可变参数；

```java
//删除_id = 1的记录
private void delete() {  
    int count = this.getContentResolver().delete(mCurrentURI, "_id = 1", null);  
    Log.e("test ", "count=" + count);  
    query();  
}  
```

### 运行结果

1、我们先运行ContentProvider对应的APP：ContentProviderBlog，然后再运行UseProvider；
2、然后先用content://com.harvic.provider.PeopleContentProvider/frist 来操作ContentProviderBlog的数据库：
3、点两个insert操作，看返回的URI，在每个URI后都添加上了当前新插入记录的行号 

![image](http://upload-images.jianshu.io/upload_images/9028834-6e418674ec5e89d0?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4、然后做下查询操作——query() 
由于我们的URI是针对first记录的，所以在这里的table_name，可以看到是“first”，即我们操作的是first表，如果我们把URI改成second对应的URI，那操作的就会变成second表 

![image](http://upload-images.jianshu.io/upload_images/9028834-83909f2dfe1154fa?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

5、更新操作——update()

执行Update()操作，会将_id = 1的记录的detail字段更新为“my name is harvic !!!!!!!!!!!!!!!!!!!!!!!!!!”；其它记录的值不变，结果如下：

![image](http://upload-images.jianshu.io/upload_images/9028834-10331dee32f0f3d9?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

6、删除操作——delete()

同样，删除操作也会只删除_id = 1的记录，所以操作之后的query()结果如下：

![image](http://upload-images.jianshu.io/upload_images/9028834-001ee9b3cd2f53cf?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**总结：**在这篇文章中，我们写了两个应用ContentProviderBlog和UseProvider,其中ContentProviderBlog派生自ContentProvider，提供第三方操作它数据库的接口；而UseProvider就是所谓第三方应用，在UseProvider中通过URI来操作ContentProviderBlog的数据库；

**源码下载地址：[http://download.csdn.net/detail/harvic880925/8528507](http://download.csdn.net/detail/harvic880925/8528507)**




<span id="ContentProvider中的getType"/>
## ContentProvider中的getType 

### MIME类型
#### 什么是MIME类型
根据百度百科的解释：MIME：全称Multipurpose Internet Mail Extensions，多功能Internet邮件扩充服务。它是一种多用途网际邮件扩充协议，在1992年最早应用于电子邮件系统，但后来也应用到浏览器。
MIME类型就是**设定某种扩展名的文件用一种应用程序来打开的方式类型**，当该扩展名文件被访问的时候，浏览器会自动使用指定应用程序来打开。多用于指定一些客户端自定义的文件名，以及一些媒体文件打开方式。
简单来讲，MIME类型就是用来**标识当前的Activity所能打开的文件类型**！

**下面简单列出来系统中自带的几种文件类型和对应的MIME类型：**

（前面是文件名，后面是对应的MIME类型字符串）

{".bmp", "image/bmp"}
{".c", "text/plain"}
{".class", "application/octet-stream"}
{".conf", "text/plain"}
{".cpp", "text/plain"}
{".doc", "application/msword"}

#### MIME类型有什么用
那现在看看在android中，MIME类型是用来干什么的呢？
首先，MIME类型主要是**Activity的Intent-filter的data域**;
比如下面这个Activity:
```xml
<activity  
    android:name=".SecondActivity"  
    android:label="@string/title_activity_second" >  
    <intent-filter>  
        <action android:name="harvic.test.qijian"/>  
        <category android:name="android.intent.category.DEFAULT"/>  
        <data android:mimeType="image/bmp"/>  
    </intent-filter>  
</activity>  
```
这里指定了data域的MimeType值是"image/bmp"，即在利用隐式Intent匹配时，只有指定MimeType是"image/bmp"时，才能启用这个Activity，也就是说，这个Activity只能打开image/bmp类型的文件！才是MIME类型匹配的重点；
所以**MIME类型在Activity中是用来指定，当前的Activity所支持打开的文件类型**！

### getType()概述
getType()的官方说明：
>public abstract **String getType (Uri uri)**    
>Implement this to handle requests for the MIME type of the data at the given URI. The returned MIME type should start with vnd.android.cursor.item for a single record, or vnd.android.cursor.dir/ for multiple items. This method can be called from multiple threads, as described in Processes and Threads.    
>Parameters  
>uri the URI to query.  
>Returns  
>a MIME type string, or null if there is no type.  

现在再回过来看看ContentProvider中的getType()函数，这个函数会根据传进来的URI，生成一个代表MimeType的字符串；而此字符串的生成也有规则：
*   如果是单条记录应该返回以vnd.android.cursor.item/ 为首的字符串
*   如果是多条记录，应该返回vnd.android.cursor.dir/ 为首的字符串

至于自符串/后的字符就随便定义了。

这里考虑一个问题，为什么我们返回的MimeType，要以vnd.android.cursor.item/ 或vnd.android.cursor.dir/ 开头？
我们知道，MIME类型其实就是一个字符串，中间有一个 “/” 来隔开，“/”前面的部分是系统识别的部分，就相当于我们定义一个变量时的变量数据类型，通过这个“数据类型”，系统能够知道我们所要表示的是个什么东西。至于 “/” 后面的部分就是我们自已来随便定义的“变量名”了。

### getType()与Activity的关系

上面我们讲了MIME存在于Activity的intent-filter中，那我们的getType() 跟Activity的intent-filter之间又有什么关系呢？
其实，**getType()返回的MIME类型**，主要就是用来**隐式匹配Intent的MIMETYPE域来启动Activity**的。
下面来看看通过URI来启用Activity的方式：

```java
Intent intent = new Intent();  
intent.setAction("harvic.test.qijian");  
intent.setData(mCurrentURI);  
startActivity(intent);  
```

其中：
```java
public static final String AUTHORITY = "com.harvic.provider.PeopleContentProvider";  
public static final Uri CONTENT_URI_FIRST = Uri.parse("content://" + AUTHORITY + "/first");  
public static Uri mCurrentURI = CONTENT_URI_FIRST;  
```

在上面的代码中，我们设置了action 和 content uri;
这里利用Content URI来启用隐式启用Activity又是怎样一个流程呢？
![](http://upload-images.jianshu.io/upload_images/9028834-b56bb6159bc1109a?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

*   (1)首先，第三方应用通过content Uri和action来隐式匹配Intent来启用Activity.

```java
Intent intent = new Intent();  
intent.setAction("harvic.test.qijian");  
intent.setData(mCurrentURI);  
startActivity(intent);  
```

*   (2)、系统通过URI中的Authority来匹配ContentProvider，从而找到我们的PeopleContentProvider。
*   (3)在找到PeopleContentProvider，由于我们是来匹配Intent的，所以这时候会调用getType(uri)来返回URI类型：

```java
static {  
     sUriMatcher = new UriMatcher(UriMatcher.NO_MATCH);  
     sUriMatcher.addURI(AUTHORITY, "first", MATCH_FIRST);  
     sUriMatcher.addURI(AUTHORITY, "second", MATCH_SECOND);  
    }  
```

上面是UriMather的构造方法，由上面的代码可知，当匹配"/first"时返回MATCH_FIRST即数值1，匹配“/second”时返回MATCH_SECOND，即数值2
所以：
1、当匹配"/fist"时，我们返回自定义的MIME类型：vnd.android.cursor.dir/harvic.first
2、当匹配“/second”时，返回MIME类型：vnd.android.cursor.item/harvic.second
代码如下：

```java
public static final String CONTENT_FIRST_TYPE = "vnd.android.cursor.dir/harvic.first";  
public static final String CONTENT_SECOND_TYPE = "vnd.android.cursor.item/harvic.second";
```

```java
public String getType(Uri uri) {  
    switch (sUriMatcher.match(uri)){  
        case MATCH_FIRST:{  
            return CONTENT_FIRST_TYPE;  
        }  
        case MATCH_SECOND:{  
            return CONTENT_SECOND_TYPE;  
        }  
    }  
    return null;  
}  
```

*   (4)下面就是根据Action和MIME类型来匹配Intent了

我们到现在在ContentProviderBlog项目中还没有一个Activity能匹配这个Action和MIME类型的，所以我们新建一个SecondActivity;

### 新建通过URI启动的Activity
在**AndroidManifest.xml**中，为SecondActivity**添加上隐式匹配所需**要的**Intent-filter**;
注意我们在getType()里根据不同的URI返回了两种MIME类型，而这里的SecondActivity的data域只添加一个**mimeType**:vnd.android.cursor.dir/harvic.first;即当我们使用content://com.harvic.provider.PeopleContentProvider/second来隐式匹配Intent时，是没办法启用SecondActivity的，因为MIME类型不匹配！

```xml
<activity  
    android:name=".SecondActivity"  
    android:label="@string/title_activity_second">  
    <intent-filter>  
        <action android:name="harvic.test.qijian" />  
        <category android:name="android.intent.category.DEFAULT" />  
        <data android:mimeType="vnd.android.cursor.dir/harvic.first" />  
    </intent-filter>  
</activity>  
```

**结果展示**

下面我们看看在使用不同的URI来启用Activity时，会出现什么结果；

**使用content://com.harvic.provider.PeopleContentProvider/first，结果如下：**
点击“thirdPart”,通过URI调起Activity

![image](http://upload-images.jianshu.io/upload_images/9028834-dd225c9f97263f09?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  ![image](http://upload-images.jianshu.io/upload_images/9028834-62a840853c3b442d?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**使用content://com.harvic.provider.PeopleContentProvider/second,由于MIME不匹配，导致无法调起Activity**

![image](http://upload-images.jianshu.io/upload_images/9028834-822715c8cbbd8dc3?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) ![image](http://upload-images.jianshu.io/upload_images/9028834-e3781e1bce116f4e?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**同样，源码包含两部分内容：**
(先装ContentProviderBlog，再装UseProvider；利用UseProvider操作ContentProviderBlog的数据库，看打出来的LOG)
1、《ContentProviderBlog》：这个是提供共享数据库接口的APP；
2、《UseProvider》：第三方通过URI来操作数据库的APP；

**源码地址：[http://download.csdn.net/detail/harvic880925/8532205](http://download.csdn.net/detail/harvic880925/8532205)**

## 数据库读写权限
### 概述
在AndroidManifest.xml中provider标签中有三个额外的参数**permission、readPermission、writePermission**;
先看下面这段代码：
```xml
<provider  
    android:name=".PeopleContentProvider"  
    android:authorities="com.harvic.provider.PeopleContentProvider"  
    android:exported="true"  
    android:permission="com.harvic.contentProviderBlog"  
    android:readPermission="com.harvic.contentProviderBlog.read"  
    android:writePermission="com.harvic.cotentProviderBlog.write"/>  
```

在这段代码中有几个参数要特别注意一下：
*   **exported:** 这个属性用于指示该服务是否能被其他程序应用组件调用或跟他交互； 取值为（true | false），如果设置成true，则能够被调用或交互，否则不能；设置为false时，只有同一个应用程序的组件或带有相同用户ID的应用程序才能启动或绑定该服务。具体参见：[《Permission Denial: opening provider 隐藏的android:exported属性的含义》](http://blog.csdn.net/guoxiao20sun/article/details/8646024)
*   **readPermission:** 使用Content Provider的查询功能所必需的权限，即**使用ContentProvider**里的**query**()函数的**权限**；

*   **writePermission：** 使用ContentProvider的修改功能所必须的权限，即**使用ContentProvider**的**insert()、update()、delete()**函数的**权限**；

*   **permission：** 客户端读、写 Content Provider 中的数据所必需的权限名称。 
  本属性为**一次性设置读和写权限**提供了快捷途径。 不过，readPermission和writePermission属性优先于本设置。 如果同时设置了readPermission属性，则其将控制对 Content Provider 的读取。 如果设置了writePermission属性，则其也将控制对 Content Provider 数据的修改。也就是说如果只设置permission权限，那么拥有这个权限的应用就可以实现对这里的ContentProvider进行读写；如果同时设置了permission和readPermission那么具有readPermission权限的应用才可以读，拥有permission权限的才能写！也就是说只拥有permission权限是不能读的，因为readPermission的优先级要高于permission；如果同时设置了readPermission、writePermission、permission那么permission就无效了。

### 有关自定义权限

由于在privoder标签中要用到**自定义权限**，比如：

```xml
<provider  
    android:name=".PeopleContentProvider"  
    android:authorities="com.harvic.provider.PeopleContentProvider"  
    android:exported="true"  
    android:readPermission="com.harvic.contentProviderBlog.read"/>  
```

比如，在这里我们定义一个readPermission字符串，单纯写一个字符串是毫无意义的，因为没有申请过的一串String代表的字符串系统根本无法识别，也就是说谁进来都会被挡在外面！

所以在application标签的同级目录，写上**申请permission的代码**：
```xml
<permission  
    android:name="com.harvic.contentProviderBlog.read"  
    android:label="provider pomission"  
    android:protectionLevel="normal" />  
```

这样，我们的permission才会在系统中注册，在第三方应用中使用<uses-permission 来声明使用权限时，才有意义；
如果不申请，那么系统中是不存在这个权限的，当第三方应用使用
了<uses-permission 来声明使用权限，因为系统中根本找不到这个权限，所以到provider匹配时，就会出现permission-deny的错误。

```xml
<uses-permission android:name="com.harvic.contentProviderBlog.read"/>  
```

有关自定义权限的内容，请参考[《声明、使用与自定义权限》](http://blog.csdn.net/harvic880925/article/details/38683625)

### 实例：带有权限的ContentProvder
首先在ContentProviderBlog中首先向系统申请两个权限：
分别对应读数据库权限和写数据库权限

```xml
<permission  
    android:name="com.harvic.contentProviderBlog.read"  
    android:label="provider read pomission"  
    android:protectionLevel="normal" />  
<permission  
    android:name="com.harvic.contentProviderBlog.write"  
    android:label="provider write pomission"  
    android:protectionLevel="normal" />  
```

**情况一：使用读写权限**
然后在provider中，我们这里同时使用读、写权限：

```xml
<provider  
    android:name=".PeopleContentProvider"  
    android:authorities="com.harvic.provider.PeopleContentProvider"  
    android:exported="true"  
    android:readPermission="com.harvic.contentProviderBlog.read"  
    android:writePermission="com.harvic.cotentProviderBlog.write" />  
```

在这种情况下，使用第三方应用同时申请使用这两个权限才可以对数据库读写，即：

```xml
<uses-permission android:name="com.harvic.contentProviderBlog.read"/>  
<uses-permission android:name="com.harvic.contentProviderBlog.write"/>  
```

**情况二：仅添加读权限**
如果我们在provider中，仅添加读共享数据库的权限，那第三方应该怎么申请权限才能读写数据库呢？

```xml
<provider  
    android:name=".PeopleContentProvider"  
    android:authorities="com.harvic.provider.PeopleContentProvider"  
    android:exported="true"  
    android:readPermission="com.harvic.contentProviderBlog.read"/>  
```

从上面可以看到，我们只添加了readPermission，那第三方应用在不申请任何权限的情况下是可以写数据库，但当读数据库时就需要权限了；即如果要读数据库需要添加如下使用权限声明：

```
<uses-permission android:name="com.harvic.contentProviderBlog.read"/>  
```

在上一篇的基础上，代码不需要动，只需要在AndroidManifest.xml中写上权限定义与声明，即可；

## ContentObserver
定义：内容观察者
作用：观察 Uri引起 ContentProvider 中的数据变化 & 通知外界（即访问该数据访问者）

### 数据监听步骤

要监听指定URI上的数据库变化，在监听方需要两步：

#### (1)生成一个类派生自ContentObserver
```java
public class DataBaseObserver extends ContentObserver {  
    public DataBaseObserver(Handler handler) {  
        super(handler);  
    }  
  
    @Override  
    public void onChange(boolean selfChange) {  
        super.onChange(selfChange);  
        Log.d("harvic","received first database changed");  
    }  
}  
```

注意这里会自动生成一个onChange()方法，当有改变到来时就会跑到onChange()里，所以我们在这里面打一个LOG，注意LOG内容：“received first database changed”，很明显这里我只用DataBaseObserver来监听"content://com.harvic.provider.PeopleContentProvider/frist "所对应的数据库进行监听，至于如何指定URI往下看

#### (2)注册和反注册监听

*   **在onCreate()时，注册**

在使用URI来操作第三方数据之前，先进行ContentObserver注册，这里我们在MainActivity的OnCreate()函数里注册：

```java
private DataBaseObserver observer;  
  
@Override  
protected void onCreate(Bundle savedInstanceState) {  
    super.onCreate(savedInstanceState);  
    setContentView(R.layout.activity_main);  
    observer = new DataBaseObserver(new Handler());  
    //注册指定URI 变动监听  
    getContentResolver().registerContentObserver(CONTENT_URI_FIRST,true,observer);  
}  
```

这里先完整说完流程，等下再讲registerContentObserver()的参数

*   **在onDestory()时，反注册：**

```java
@Override  
protected void onDestroy() {  
    super.onDestroy();  
    //取消监听  
    getContentResolver().unregisterContentObserver(observer);  
}  
```

#### (3)在ContentProvider中通知数据库变动
在ContentProvider中指定URI上有数据库数据变动时，及时利用下面的函数来通知

```java
getContext().getContentResolver().notifyChange(uri, null);  
```
好了，到这数据库变动通知和监听的过程就讲完了。

### registerContentObserver
下面看看registerContentObserver()注册监听函数的用法：

```java
public final void registerContentObserver(Uri uri, boolean notifyForDescendents, ContentObserver observer)
```

功能：**为指定的Uri注册一个ContentObserver派生类实例**，当**给定**的Uri发生改变时，回调该实例对象去处理。

**参数：**
*   **uri ：**     需要观察的Uri
*   **notifyForDescendents：**  为false 表示精确匹配，即只匹配该Uri; 为true 表示可以同时匹配其派生的Uri。
  举例如下:假设UriMatcher 里注册的Uri共有一下类型：
  1、content://com.qin.cb/student (学生)
  2、content://com.qin.cb/student/# 
  3、content://com.qin.cb/student/schoolchild(小学生，派生的Uri) 
  假设我们当前需要观察的Uri为content://com.qin.cb/student，如果发生数据变化的 Uri 为 content://com.qin.cb/student/schoolchild；那么当notifyForDescendents为 false，那么该ContentObserver会监听不到， 但是当notifyForDescendents 为ture，能捕捉该Uri的数据库变化。

*   **observer：** ContentObserver的派生类实例



## SetProjectionMap——投影映射

在上面的实例中，我们对外提供的数据库列名和内部使用的是一样的，但这样很容易被对方知道我们的数据库结构，那要想自己本地使用一套列名，给外部提供另一套对应的列名，这样别人不就猜不出我们的列名了么，针对这个问题，在SQLiteQueryBuilder类中，为我们提供了内外部列名映射函数，以允许我们在外部和内部列名不同时的提供映射功能。

主要使用的函数是setProjectionMap(HashMap map)

使用方法如下：

```java
SQLiteQueryBuilder queryBuilder = new SQLiteQueryBuilder();  
Map projectionMap = new HashMap<String,String>();  
projectionMap.put("out_column_name_1","inside_column_name_1");  
projectionMap.put("out_column_name_2","inside_column_name_2");  
projectionMap.put("out_column_name_3","inside_column_name_3");  
queryBuilder.setTables(DatabaseHelper.TABLE_FIRST_NAME);  
queryBuilder.setProjectionMap(projectionMap);  
```

**源码来了，源码内容：**
1、第一部分：数据库读写权限
2、第二部分：数据监听（把读写权限代码删除，只有监听相关的代码）
**源码下载地址：[http://download.csdn.net/detail/harvic880925/8535963](http://download.csdn.net/detail/harvic880925/8535963)**


## 实例


### 自动获取短信验证码
##### 1.自定义监听类
```java
//短信监听器，用于自动填充验证码
public class SMSContentObserver extends ContentObserver {
  public final String SMS_URI_INBOX = "content://sms/inbox";//收信箱
  private Activity activity = null;
  private String smsContent = "";//验证码
  private EditText verifyText = null;//验证码编辑框
  private String SMS_ADDRESS_PRNUMBER = "10690329013589";//短息发送提供商
  private String smsID = "";
  //短信观察者 收到一条短信时 onchange方法会执行两次，所以比较短信id，如果一致则不处理
  public SMSContentObserver(Activity activity, Handler handler, EditText verifyText) {
    super(handler);
    this.activity = activity;
    this.verifyText = verifyText;
  }
  @Override
  public void onChange(boolean selfChange) {
    super.onChange(selfChange);
    Cursor cursor = null;// 光标
    // 读取收件箱中指定号码的短信
    cursor = activity.getContentResolver().query(Uri.parse(SMS_URI_INBOX),
      new String[]{"_id", "address", "body", "read"}, //要读取的属性
      "address=? and read=?", //查询条件是什么
      new String[]{SMS_ADDRESS_PRNUMBER, "0"},//查询条件赋值
      "date desc");//排序
    if (cursor != null) {
      cursor.moveToFirst();
      if (cursor.moveToFirst()) {
        //比较和上次接收到短信的ID是否相等
        if (!smsID.equals(cursor.getString(cursor.getColumnIndex("_id")))) {
          String smsbody = cursor.getString(cursor.getColumnIndex("body"));
          //用正则表达式匹配验证码
          Pattern pattern = Pattern.compile("[0-9]{6}");
          Matcher matcher = pattern.matcher(smsbody);
          if (matcher.find()) {//匹配到6位的验证码
            smsContent = matcher.group();
            if (verifyText != null && null != smsContent && !"".equals(smsContent)) {
              verifyText.requestFocus();//获取焦点
              verifyText.setText(smsContent);//设置文本
              verifyText.setSelection(smsContent.length());//设置光标位置
            }
          }
          smsID = cursor.getString(cursor.getColumnIndex("_id"));
        }
      }
    }
  }
}
```

##### 2.在页面注册监听类

```java
//实例化短信监听器
SMSContentObserver mObserver = new SMSContentObserver(getActivity(), new Handler(), mEt_auth_code);
// 注册短信变化监听
mContext.getContentResolver().registerContentObserver(Uri.parse("content://sms/"), true, mObserver);
```

##### 3.声明读取短信权限
```xml
<uses-permission android:name="android.permission.RECEIVE_SMS" />
<uses-permission android:name="android.permission.READ_SMS" />
<uses-permission android:name="android.permission.WRITE_SMS" />
```

##### 4.为了防止内存泄漏，记得注销监听
```java
@Override
public void onDestroy() {
super.onDestroy();
  //注销短信监听   
  mContext.getContentResolver().unregisterContentObserver(mObserver);
} 
```

小结：去短信库获取短信比较不容易被拦截


### 短信的备份和恢复
Android 系统中提供了一系列的内容提供者，通过调用他们，可以获取一些系统的信息，比如:短信信息，联系人信息等。

#### 准备知识
打开 Android 源码，查看 packages\providers\路径下的工程，这些就是 Android 系 统中的内容提供者，其中 TelephonyProvider 就是短信的内容提供者文件。
- 打开 TelephonyProvider 下的 src 文件，查看 java 文件，其中的 [SmsProvider.java](http://androidxref.com/8.0.0_r4/xref/packages/providers/TelephonyProvider/src/com/android/providers/telephony/SmsProvider.java)即短信息内容提供者逻辑代码。UriMatcher 一般在静态代码块中进行初始化操作，查找静态代码块，找到的逻辑代码如下：
```java
private static final UriMatcher sURLMatcher =
        new UriMatcher(UriMatcher.NO_MATCH);
static {
    sURLMatcher.addURI("sms", null, SMS_ALL);
    sURLMatcher.addURI("sms", "#", SMS_ALL_ID);
    sURLMatcher.addURI("sms", "inbox", SMS_INBOX);
    sURLMatcher.addURI("sms", "inbox/#", SMS_INBOX_ID);
    sURLMatcher.addURI("sms", "sent", SMS_SENT);
    sURLMatcher.addURI("sms", "sent/#", SMS_SENT_ID);
    sURLMatcher.addURI("sms", "draft", SMS_DRAFT);
    sURLMatcher.addURI("sms", "draft/#", SMS_DRAFT_ID);
    sURLMatcher.addURI("sms", "outbox", SMS_OUTBOX);
    sURLMatcher.addURI("sms", "outbox/#", SMS_OUTBOX_ID);
    sURLMatcher.addURI("sms", "undelivered", SMS_UNDELIVERED);
    sURLMatcher.addURI("sms", "failed", SMS_FAILED);
    sURLMatcher.addURI("sms", "failed/#", SMS_FAILED_ID);
    sURLMatcher.addURI("sms", "queued", SMS_QUEUED);
    sURLMatcher.addURI("sms", "conversations", SMS_CONVERSATIONS);
    sURLMatcher.addURI("sms", "conversations/*", SMS_CONVERSATIONS_ID);
    sURLMatcher.addURI("sms", "raw", SMS_RAW_MESSAGE);
    sURLMatcher.addURI("sms", "raw/permanentDelete", SMS_RAW_MESSAGE_PERMANENT_DELETE);
    sURLMatcher.addURI("sms", "attachments", SMS_ATTACHMENT);
    sURLMatcher.addURI("sms", "attachments/#", SMS_ATTACHMENT_ID);
    sURLMatcher.addURI("sms", "threadID", SMS_NEW_THREAD_ID);
    sURLMatcher.addURI("sms", "threadID/*", SMS_QUERY_THREAD_ID);
    sURLMatcher.addURI("sms", "status/#", SMS_STATUS_ID);
    sURLMatcher.addURI("sms", "sr_pending", SMS_STATUS_PENDING);
    sURLMatcher.addURI("sms", "icc", SMS_ALL_ICC);
    sURLMatcher.addURI("sms", "icc/#", SMS_ICC);
    //we keep these for not breaking old applications
    sURLMatcher.addURI("sms", "sim", SMS_ALL_ICC);
    sURLMatcher.addURI("sms", "sim/#", SMS_ICC);
}
```
在数据库中 sms 表就是用于存储短信的，所以通过查找系统源码，可以确定短信息内容提供者的 Uri 应该为:”content://sms"

- 查 看 Android 模 拟 器 下 的 com.android.providers.telephony ， 查看其mmssms.db 文件：
  ![mmssms.db 文件.png](http://upload-images.jianshu.io/upload_images/9028834-303de8d497e671fa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

打开数据库，其中 sms 表存储的就是短信的数据，其存储格式如下：
[![存储格式.png](http://upload-images.jianshu.io/upload_images/9028834-89af28a518eec02c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://upload-images.jianshu.io/upload_images/9028834-89af28a518eec02c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
其中，address 存储的是联系人号码，date 是发送日期，type 对应短信的类型（发送/接收）,body 是短信的主体内容，准备备份这四项。

#### 实现步骤
##### 获取短信列表
根据content://sms/这个Uri去查询手机中短信的数据库，得到一个Cursor。这个Cursor就包含了一条一条的短信。然后去遍历这个Cursor将我们需要的数据添加到smsList中。
```java
//获取短信列表 
 public List<SmsData> getSmsList() {  
       //获取所有短信的 Uri  
      Uri uri = Uri. parse( "content://sms/");  
       //获取ContentResolver对象  
      ContentResolver contentResolver = mContext.getContentResolver();  
       //根据Uri 查询短信数据  
      Cursor cursor = contentResolver.query(uri, null, null, null, null);  
       if ( null != cursor) {  
           Log. i( TAG, "cursor.getCount():" + cursor.getCount());  
            //根据得到的Cursor一条一条的添加到smsList(短信列表)中  
            while (cursor.moveToNext()) {  
                 int _id = cursor.getInt(cursor.getColumnIndex("_id" ));  
                 int type = cursor.getInt(cursor.getColumnIndex("type" ));  
                String address = cursor.getString(cursor.getColumnIndex( "address"));  
                String body = cursor.getString(cursor.getColumnIndex("body" ));  
                String date = cursor.getString(cursor.getColumnIndex("date" ));  
                SmsData smsData = new SmsData(_id, type, address, body, date);  
                 smsList.add(smsData);  
           }  
           cursor.close();  
      }  
       return smsList;  
}  
```
##### 将短信数据保存到xml文件中
使用XmlSerializer将smsList序列化，待我们需要恢复的时候使用XmlPullParser 将其反序列化，就可以拿到备份的数据。

```java
//将短信数据保存到 sd卡中
public void saveSmsToSdCard(){  
            smsList=getSmsList();  
            //获得一个序列化对象  
           XmlSerializer xmlSerializer=Xml. newSerializer();  
            //将生成的 xml文件保存到sd 卡中名字为"sms.xml"  
           File file= new File(Environment.getExternalStorageDirectory(), "sms.xml");  
           FileOutputStream fos;  
             
            try {  
                fos = new FileOutputStream(file);  
                xmlSerializer.setOutput(fos, "utf-8");  
                xmlSerializer.startDocument( "utf-8", true);  
                xmlSerializer.startTag( null, "smss");  
                  
                xmlSerializer.startTag( null, "count");  
                xmlSerializer.text( smsList.size()+ "");  
                xmlSerializer.endTag( null, "count");  
                  
                 for(SmsData smsData: smsList){  
                       
                     xmlSerializer.startTag( null, "sms");  
                       
                     xmlSerializer.startTag( null, "_id");  
                     xmlSerializer.text(smsData.get_id()+ "");  
                     System. out.println( "smsData.get_id()=" +smsData.get_id());  
                     xmlSerializer.endTag( null, "_id");  
                       
                     xmlSerializer.startTag( null, "type");  
                     xmlSerializer.text(smsData.getType()+ "");  
                     System. out.println( "smsData.getType=" +smsData.getType());  
                     xmlSerializer.endTag( null, "type");  
                       
                     xmlSerializer.startTag( null, "address");  
                     xmlSerializer.text(smsData.getAddress()+ "");  
                     System. out.println( "smsData.getAddress()=" +smsData.getAddress());  
                     xmlSerializer.endTag( null, "address");  
                       
                     xmlSerializer.startTag( null, "body");  
                     xmlSerializer.text(smsData.getBody()+ "");  
                     System. out.println( "smsData.getBody()=" +smsData.getBody());  
                     xmlSerializer.endTag( null, "body");  
                       
                     xmlSerializer.startTag( null, "date");  
                     xmlSerializer.text(smsData.getDate()+ "");  
                     System. out.println( "smsData.getDate()=" +smsData.getDate());  
                     xmlSerializer.endTag( null, "date");  
                       
                     xmlSerializer.endTag( null, "sms");  
                }  
                xmlSerializer.endTag( null, "smss");  
                xmlSerializer.endDocument();  
                  
                fos.flush();  
                fos.close();  
                Toast. makeText( mContext, "备份完成", Toast.LENGTH_SHORT ).show();  
           } catch (FileNotFoundException e) {  
                e.printStackTrace();  
           } catch (IllegalArgumentException e) {  
                e.printStackTrace();  
           } catch (IllegalStateException e) {  
                e.printStackTrace();  
           } catch (IOException e) {  
                e.printStackTrace();  
           }        
     }  
```
通过调用以上方法就将短信以xml的形式保存到了本地。运行这个方法后可以再sd卡中看到sms.xml文件，将其导出来可以看到它的格式如下：
![](http://upload-images.jianshu.io/upload_images/9028834-d1171cde3362d20c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
注：实际上xml文件中它是一行这里为了让大家更清楚的看清结构，我手动将其改成上述格式了。

##### 恢复短信
判断xml的END_TAG是不是"sms"，如果是的话说明一条短信的"type"、"address"、"body"、"date"这些信息已经拿到，然后就根据Uri将这条数据插入到数据库中，就完成一条短信的恢复，待读到 END_DOCUMENT说明xml文件已经读完，此时所备份的短信就全部恢复了。
```java
//将指定路径的 xml文件中的数据插入到短信数据库中 
 public void restoreSms(String path) {  
      File file = new File(path);  
       //得到一个解析 xml的对象  
      XmlPullParser parser = Xml. newPullParser();  
       try {  
            fis = new FileInputStream(file);  
           parser.setInput( fis, "utf-8");  
           ContentValues values = null;  
            int type = parser.getEventType();  
            while (type != XmlPullParser. END_DOCUMENT) {  
                 switch (type) {  
                 case XmlPullParser. START_TAG:  
                       if ( "count".equals(parser.getName())) {  
                      } else if ("sms" .equals(parser.getName())) {  
                           values = new ContentValues();  
                      } else if ("type" .equals(parser.getName())) {  
                           values.put( "type", parser.nextText());  
                      } else if ("address" .equals(parser.getName())) {  
                           values.put( "address", parser.nextText());  
                      } else if ("body" .equals(parser.getName())) {  
                           values.put( "body", parser.nextText());  
                      } else if ("date" .equals(parser.getName())) {  
                           values.put( "date", parser.nextText());  
                      }  
                       break;  
                 case XmlPullParser. END_TAG:  
                       if ( "sms".equals(parser.getName())) {// 如果节点是 sms  
                           Uri uri = Uri.parse( "content://sms/");  
                           ContentResolver resolver = mContext.getContentResolver();  
                           resolver.insert(uri, values);//向数据库中插入数据  
                           System. out.println( "插入成功" );  
                           values = null; //  
                      }  
                       break;  
                }  
                type=parser.next();  
           }  
      } catch (FileNotFoundException e) {  
           e.printStackTrace();  
      } catch (XmlPullParserException e) {  
           e.printStackTrace();  
      } catch (NumberFormatException e) {  
           e.printStackTrace();  
      } catch (IOException e) {  
           e.printStackTrace();  
      }  
}  
```
Tips：在该工程中，短信的恢复逻辑比较简单，只是把 sms.xml 文件中的所有短信插入到
短信数据库中，其实如果短信数据库中还有相同的短信的时候我们这样处理的结果是数据库
中有了 2 份相同的短信，因此比较好的做法是在插入数据库之前先判断该短信是否存在，
如果存在则不插入。

### 操作系统联系人
#### 准备知识
- 通过 DDMS，查看 Android 模拟器下的 com.android.providers.contacts 包下的数据库，查看其 contact2.db 数据库的内容。
   ![contact2.db.png](http://upload-images.jianshu.io/upload_images/9028834-aa555d1657afa5a0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 查看数据库，其中 raw_contacts 表存放的是联系人条数信息，data 表中存放的是raw_contacts 中的每一条 id 对应的具体信息，不同类型的信息由 mimetype_id 来标识。
  **raw_contacts 表**：
  [![raw_contacts 表.png](http://upload-images.jianshu.io/upload_images/9028834-7a9e55337cca92c1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://upload-images.jianshu.io/upload_images/9028834-7a9e55337cca92c1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
  ．
  **data 表**：
  [![data 表.png](http://upload-images.jianshu.io/upload_images/9028834-cb6711a2f0bfa72c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://upload-images.jianshu.io/upload_images/9028834-cb6711a2f0bfa72c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
  ．
  **mimetypes 表**：
  [![mimetypes 表.png](http://upload-images.jianshu.io/upload_images/9028834-322cff830d7ef938.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://upload-images.jianshu.io/upload_images/9028834-322cff830d7ef938.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


- 打 开 Android 源 码 ， 查 看 packages\providers\ 路 径 下 的 文 件 ， 其 中[ContactsProvider](http://androidxref.com/8.0.0_r4/xref/packages/providers/ContactsProvider/) 就是联系人的内容提供者。
1. 打开清单文件，寻找联系人的内容提供者对应的是哪个 java 文件
```xml
<provider android:name="ContactsProvider2"
    android:authorities="contacts;com.android.contacts"
    android:label="@string/provider_label"
    android:multiprocess="false"
    android:exported="true"
    android:grantUriPermissions="true"
    android:readPermission="android.permission.READ_CONTACTS"
    android:writePermission="android.permission.WRITE_CONTACTS"
    android:visibleToInstantApps="true">
    <path-permission
            android:pathPrefix="/search_suggest_query"
            android:readPermission="android.permission.GLOBAL_SEARCH" />
    <path-permission
            android:pathPrefix="/search_suggest_shortcut"
            android:readPermission="android.permission.GLOBAL_SEARCH" />
    <path-permission
            android:pathPattern="/contacts/.*/photo"
            android:readPermission="android.permission.GLOBAL_SEARCH" />
    <grant-uri-permission android:pathPattern=".*" />
</provider>
```
2. 打开 ContactsProvider2.java 文件，查看此内容提供者的 uri 路径信息

```java
matcher.addURI(ContactsContract.AUTHORITY, "contacts", CONTACTS);
matcher.addURI(ContactsContract.AUTHORITY, "contacts/#", CONTACTS_ID);
matcher.addURI(ContactsContract.AUTHORITY, "contacts/#/data", CONTACTS_DATA);
matcher.addURI(ContactsContract.AUTHORITY, "raw_contacts", RAW_CONTACTS);
matcher.addURI(ContactsContract.AUTHORITY, "raw_contacts/#", RAW_CONTACTS_ID);
matcher.addURI(ContactsContract.AUTHORITY, "raw_contacts/#/data", RAW_CONTACTS_DATA);
```
3. 根据源码，确定内容提供者的 Uri 信息为：
  操作 raw_contacts 表的 Uri:
```
content://com.android.contacts/raw_contacts
```

操作 data 表的 Uri：
```
content://com.android.contacts/data
```

4. 操作数据库表时 注意：由于 contacts2.db 数据库使用了视图，所以操作数据库表时，表结构有所改变，注意操作时要操纵的列的列名已经改变。
  比如：data 表在查询的时候没有 mimetype_id,取代的是 mimetype


#### 实现步骤
1. 操作 raw_contacts 表，获取全部的 id
2. 根据获取到的每一条 id 去查询 data 表中的数据
3. 将查询到的展示是用户界面
4. 通过代码给系统联系人插入一条联系人信

```java
public class MainActivity extends Activity {
    private ListView lv;
    private List<Contact> list;
    private MyAdapter adapter;
    private Handler handler = new Handler() {
        public void handleMessage(android.os.Message msg) {
            Toast.makeText(MainActivity.this, "数据获取成功。", 1).show();
            //更新数据
            adapter.notifyDataSetChanged();
        };
    };

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        lv = (ListView) findViewById(R.id.lv);
        list = new ArrayList<Contact>();
        adapter = new MyAdapter();
        lv.setAdapter(adapter);
    }

    //查询所有的系统联系人，这里的业务放在子线程中操作
    public void queryContacts(View view) {
        new Thread(new Runnable() {
            @Override
            public void run() {
                ContentResolver contentResolver = getContentResolver();
                Uri uri = Uri.parse("content://com.android.contacts/raw_contacts");
                Cursor cursor = contentResolver.query(uri, new String[]{"contact_id"}, null, null, null);
                list.clear();
                int i = 0;
                while (cursor.moveToNext()) {
                    i++;
                    //先从 raw_contacts 表中获取 contact_id
                    String contact_id = cursor.getString(0);
                    if (TextUtils.isEmpty(contact_id)) {
                        continue;
                    }
                    //根据 contact_id 从 data 表中查询具体的数据
                    Cursor cursor2 = contentResolver.query(
                                    Uri.parse("content://com.android.contacts/data"),
                                    new String[]{ " data1", " mimetype" },
                                    " contact_id = ? ", new String[]{contact_id}, null);
                    Contact contact = new Contact();
                    while (cursor2.moveToNext()) {
                        String data = cursor2.getString(0);
                        String mimetype = cursor2.getString(1);
                        if ("vnd.android.cursor.item/name".equals(mimetype)) {
                            contact.setName(data);
                        } else if ("vnd.android.cursor.item/phone_v2".equals(mimetype)) {
                            contact.setPhone(data);
                        } else if ("vnd.android.cursor.item/email_v2".equals(mimetype)) {
                            contact.setEmail(data);
                        } else {
                            contact.setOther(data);
                        }
                    }
                    list.add(contact);
                    cursor2.close();
                }
                cursor.close();
                //发送一个空消息，更新 ListView
                handler.sendEmptyMessage(RESULT_OK);
            }
        }).start();
    }

    class MyAdapter extends BaseAdapter {
        @Override
        public int getCount() {
            return list.size();
        }

        @Override
        public Object getItem(int position) {
            return null;
        }

        @Override
        public long getItemId(int position) {
            return position;
        }

        //这里面通过 ViewHolder 类将其他子属性值绑定在 View 上面，这里对ListView 作了进一步的优化处理
        @Override
        public View getView(int position, View convertView, ViewGroup parent) {
            View view;
            ViewHolder holder;
            if (convertView != null) {
                view = convertView;
                holder = (ViewHolder) view.getTag();
            } else {
                view = View.inflate(MainActivity.this, R.layout.list_item, null);
                holder = new ViewHolder();
                holder.tv_name = (TextView) view.findViewById(R.id.tv_name);
                holder.tv_phone = (TextView) view.findViewById(R.id.tv_phone);
                holder.tv_email = (TextView) view.findViewById(R.id.tv_email);
                view.setTag(holder);
            }
            Contact contact = list.get(position);
            holder.tv_name.setText(contact.getName());
            holder.tv_email.setText(contact.getEmail());
            holder.tv_phone.setText(contact.getPhone());
            return view;
        }
    }

    class ViewHolder {
        TextView tv_name;
        TextView tv_email;
        TextView tv_phone;
    }

    //往系统联系人表中插入一条数据，这里为了方便演示，我们直接插入一条固定的数据
    public void insertContacts(View view) {
        //创建一个自定义的 Contact 类，将要往系统联系人表中插入的字段封装起来
        Contact contact = new Contact();
        contact.setEmail("itheima@itcast.cn");
        contact.setName("王二麻子" + new Random().nextInt(1000));
        contact.setPhone("9999999" + new Random().nextInt(100));
        contact.setOther("北京市中关村软件园");
        //获取 ContentResolver 对象
        ContentResolver resolver = getContentResolver();
        //操作 raw_contacts 表的 uri
        Uri raw_uri = Uri.parse("content://com.android.contacts/raw_contacts");
        //操作 data 表的 uri
        Uri data_uri = Uri.parse("content://com.android.contacts/data");
        //在插入数据之前先查询出当前最大的 id
        Cursor cursor = resolver.query(raw_uri, new String[]{"contact_id"}, null, null, "contact_id desc limit 1");
        int id = 1;
        if (cursor != null) {
            boolean moveToFirst = cursor.moveToFirst();
            if (moveToFirst) {
                id = cursor.getInt(0);
            }
        }
        cursor.close();
        //要插入数据的 contact_id 值
        int newId = id + 1;
        // 给 raw_contact 表中添加一天记录
        ContentValues values = new ContentValues();
        values.put("contact_id", newId);
        resolver.insert(raw_uri, values);
	// 在 data 表中添加数据
        // 添加 name
        values = new ContentValues();
        values.put("raw_contact_id", newId);
        values.put("mimetype", "vnd.android.cursor.item/name");
        values.put("data1", contact.getName());
        resolver.insert(data_uri, values);
        // 添加 phone
        values = new ContentValues();
        values.put("raw_contact_id", newId);
        values.put("mimetype", "vnd.android.cursor.item/phone_v2");
        values.put("data1", contact.getPhone());
        resolver.insert(data_uri, values);
        // 添加地址
        values = new ContentValues();
        values.put("raw_contact_id", newId);
        values.put("mimetype", "vnd.android.cursor.item/postal-address_v2");
        values.put("data1", contact.getOther());
        resolver.insert(data_uri, values);
        // 添加 email
        values = new ContentValues();
        values.put("raw_contact_id", newId);
        values.put("mimetype", "vnd.android.cursor.item/email_v2");
        values.put("data1", contact.getEmail());
        resolver.insert(data_uri, values);
        Toast.makeText(this, "成功插入联系人" + contact, 0).show();
    }
}
```
在 AndroidManifest.xml 中添加添加如下**权限**：
```xml
<uses-permission android:name="android.permission.READ_CONTACTS"/>
<uses-permission android:name="android.permission.WRITE_CONTACTS"/>
```

# Android ContentProvider面试题
## ContentProvider如何实现数据共享？
在 Android 中如果想将自己应用的数据（一般多为数据库中的数据）提供给第三发应用，那么我们只能通过 ContentProvider 来实现了。
ContentProvider 是应用程序之间共享数据的接口。使用的时候首先自定义一个类继承 ContentProvider，然后 覆写 query、insert、update、delete 等方法。因为其是四大组件之一因此必须在 AndroidManifest 文件中进行注册。
```xml
<provider android:exported="true" 
	android:name="com.itheima.contenProvider.provider.PersonContentProvider"
	android:authorities="com.itheima.person" />
```
第三方可以通过 ContentResolver 来访问该 Provider


## 为什么要用 ContentProvider？它和 sql 的实现上有什么差别？

ContentProvider 屏蔽了数据存储的细节,内部实现对用户完全透明,用户只需要关心操作数据的 uri 就可以了， ContentProvider 可以实现不同 app 之间共享。
Sql 也有增删改查的方法，但是 sql 只能查询本应用下的数据库。而 ContentProvider 还可以去增删改查本地文件. xml 文件的读取等。

## ContentProvider、ContentResolver、ContentObserver 之间的关系
**ContentProvider** 内容提供者，用于对外提供数据
ContentResolver.notifyChange(uri)发出消息
**ContentResolver** 内容解析者，用于获取内容提供者提供的数据
**ContentObserver** 内容监听器，可以监听数据的改变状态
ContentResolver.registerContentObserver()监听消息。

## 如何访问 asserts 资源目录下的数据库？

```java
//获取到 assert 目录下的 db 文件
    AssetManager assetManager = getAssets();
    InputStream is = assetManager.open("myuser.db");
//将文件拷贝到/data / data / com.itheima.android.asserts.sqlite / databases / myuser.db
//如果 databases 目录不存在则创建
    File file = new File("/data/data/com.itheima.android.asserts.sqlite/databases");
    if (!file.exists()) {
        file.mkdirs();
    }
    FileOutputStream fos = new FileOutputStream(new File(file, "myuser.db"));
    byte[] buff = new byte[1024 * 8];
    int len = -1;
    while ((len = is.read(buff)) != -1) {
        fos.write(buff, 0, len);
    }
    fos.close();
    is.close();
//访问数据库
    SQLiteDatabase database = openOrCreateDatabase("myuser.db", MODE_PRIVATE, null);
    String sql = "select c_name from t_user";
    Cursor cursor = database.rawQuery(sql, null);
    while (cursor.moveToNext()) {
        String string = cursor.getString(0);
        Log.d("tag", string);
    }
    cursor.close();
    database.close();
}
```


## 如何在高并发下进行数据库查询？
（2015-11-25）
（这个问题的回答很广泛，可以自由发挥） 比如：不要关联多表查询，减少链接时间，创建索引、将查询到的数据采用缓存策略等等。






引用：
★★★[1、《ContentProvider数据库共享之——概述》](http://blog.csdn.net/harvic880925/article/details/44521461)
★★★[2、《ContentProvider数据库共享之——实例讲解》](http://blog.csdn.net/harvic880925/article/details/44591631)
★★★[3、《ContentProvider数据库共享之——MIME类型与getType()》](http://blog.csdn.net/harvic880925/article/details/44620851)
★★★[4、《ContentProvider数据库共享之——读写权限与数据监听》](http://blog.csdn.net/harvic880925/article/details/44651967)

[Android开发之调用系统的ContentProvider——短信的备份和恢复](http://blog.csdn.net/dmk877/article/details/50518464)
[Android自动获取短信验证码功能](http://www.jb51.net/article/111074.htm)








