<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [【Android SQLite】](#android-sqlite)
  - [SQLite 简介](#sqlite-%E7%AE%80%E4%BB%8B)
  - [使用 SQLiteOpenHelper 创建数据库](#%E4%BD%BF%E7%94%A8-sqliteopenhelper-%E5%88%9B%E5%BB%BA%E6%95%B0%E6%8D%AE%E5%BA%93)
    - [创建 SQLiteOpenHelper 类](#%E5%88%9B%E5%BB%BA-sqliteopenhelper-%E7%B1%BB)
    - [使用 SQLiteOpenHelper 类](#%E4%BD%BF%E7%94%A8-sqliteopenhelper-%E7%B1%BB)
  - [数据库的增删改查](#%E6%95%B0%E6%8D%AE%E5%BA%93%E7%9A%84%E5%A2%9E%E5%88%A0%E6%94%B9%E6%9F%A5)
    - [1、使用纯 SQL 语句实现](#1%E4%BD%BF%E7%94%A8%E7%BA%AF-sql-%E8%AF%AD%E5%8F%A5%E5%AE%9E%E7%8E%B0)
      - [添加数据](#%E6%B7%BB%E5%8A%A0%E6%95%B0%E6%8D%AE)
      - [删除数据](#%E5%88%A0%E9%99%A4%E6%95%B0%E6%8D%AE)
      - [修改数据](#%E4%BF%AE%E6%94%B9%E6%95%B0%E6%8D%AE)
      - [查询数据](#%E6%9F%A5%E8%AF%A2%E6%95%B0%E6%8D%AE)
    - [2、使用特有 API 实现](#2%E4%BD%BF%E7%94%A8%E7%89%B9%E6%9C%89-api-%E5%AE%9E%E7%8E%B0)
      - [添加数据](#%E6%B7%BB%E5%8A%A0%E6%95%B0%E6%8D%AE-1)
      - [删除数据](#%E5%88%A0%E9%99%A4%E6%95%B0%E6%8D%AE-1)
      - [修改数据](#%E4%BF%AE%E6%94%B9%E6%95%B0%E6%8D%AE-1)
      - [查询数据](#%E6%9F%A5%E8%AF%A2%E6%95%B0%E6%8D%AE-1)
    - [两种 SQLiteDatabase 的不同](#%E4%B8%A4%E7%A7%8D-sqlitedatabase-%E7%9A%84%E4%B8%8D%E5%90%8C)
  - [数据库的升级和事务操作](#%E6%95%B0%E6%8D%AE%E5%BA%93%E7%9A%84%E5%8D%87%E7%BA%A7%E5%92%8C%E4%BA%8B%E5%8A%A1%E6%93%8D%E4%BD%9C)
    - [数据库的升级](#%E6%95%B0%E6%8D%AE%E5%BA%93%E7%9A%84%E5%8D%87%E7%BA%A7)
    - [事务的操作](#%E4%BA%8B%E5%8A%A1%E7%9A%84%E6%93%8D%E4%BD%9C)
  - [sqlite3 工具的使用](#sqlite3-%E5%B7%A5%E5%85%B7%E7%9A%84%E4%BD%BF%E7%94%A8)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->



# 【Android SQLite】
## SQLite 简介
SQLite 是一款内置到移动设备上的轻量型的数据库，是遵守 ACID（原子性、一致性、隔离性、持久 性）的关联式数据库管理系统，多用于嵌入式系统中。
SQLite 数据库是无类型的，可以向一个 integer 的列中添加一个字符串，但它又支持常见的类型比如: NULL，VARCHAR, TEXT, INTEGER, BLOB, CLOB 等。
Android 系统内置了 SQLite，并提供了一系列 API 方便对其进行操作。
## 使用 SQLiteOpenHelper 创建数据库
SQLiteOpenHelper 是 Android 提供的一个抽象工具类，负责管理数据库的创建、升级工作。如果我们 想创建数据库，就需要自定义一个类继承 SQLiteOpenHelper，然后覆写其中的抽象方法。
### 创建 SQLiteOpenHelper 类
【文件 1-1】 MySQLiteOpenHelper.java
```java
public class MySQLiteOpenHelper extends SQLiteOpenHelper {
    private static final String TAG = "MySQLiteOpenHelper";
    //定义数据库文件名
    public static final String TABLE_NAME = "myuser.db";

    /**
     * @param context
     * @param name    数据库文件的名称
     * @param factory null
     * @param version 数据库文件的版本
     */
    private MySQLiteOpenHelper(Context context, String name, SQLiteDatabase.CursorFactory factory,
                               int version) {
        super(context, name, factory, version);
    }

    // 对外提供构造函数
    public MySQLiteOpenHelper(Context context, int version) {
		//调用该类中的私有构造函数
        this(context, TABLE_NAME, null, version);
    }

    // 当第一次创建数据的时候回调方法
    @Override
    public void onCreate(SQLiteDatabase db) {
        Log.d(TAG, "onCreate");
//创建数据库表的语句
        String sql = "create table t_user(uid integer primary key not null, c_name varchar (20), c_age integer, c_phone varchar(20))";
        db.execSQL(sql);
    }

    // 当数据库升级是回调该方法
    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
        Log.d(TAG, "onUpgrade: oldVersion" + oldVersion + " newVersion=" + newVersion);
        String sql = "alter table t_user add c_money float";
        db.execSQL(sql);
    }

    // 当数据库被打开时回调该方法
    @Override
    public void onOpen(SQLiteDatabase db) {
        Log.d(TAG, "onOpen");
    }
}
```

注意：
上面的代码我们只是定义了一个 MySQLiteOpenHelper 类继承了 SQLiteOpenHelper 类。在 onCreate()方法中通过执行 sql 语句实现表的创建。

### 使用 SQLiteOpenHelper 类
在 Activity 中可以执行如【文件 1-2】所示代码。如果第一次执行，则会创建一个数据库文件，创建的数据库文件位/data/data/包名/databases/目录中。
【文件 1-2】 代码片段
```java
	/* 通过构造函数创建一个 MySQLiteOpenHelper 对象，
	* 此时数据库文件还未创建	*/
	MySQLiteOpenHelper openHelper = new MySQLiteOpenHelper(this, VERSION);
	// 调用以下任一方法可使数据库文件得以创建
	openHelper.getWritableDatabase();
 //	openHelper.getReadableDatabase();
	//关闭资源
	openHelper.close();
```

注意：
如果 openHelper.getWritableDatabase();或者 openHelper.getReadableDatabase();是**第一次被调用**，那么数据库文件**才**会被**创建**，否则只打开不创建。

创建的数据库文件如图 1-1 所示，包含两个文件，一个就是我们自定义的名称 myuser.db，另外一个 myuser.db-journal，该文件会被自动创建，是 sqlite 的一个临时的日志文件，主要用于 sqlite 数据库的事务 回滚操作。
![](http://upload-images.jianshu.io/upload_images/9028834-e7bbfa1aff4f1a07.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
图 1-1	database 文件

## 数据库的增删改查
以上创建了 MySQLiteOpenHelper 类，通过该类可以获取 SQLiteDatabase 对象。而 **SQLiteDatabase**对象则可是实现**对数据库**的**增删改查**操作。

对数据库的增删改查我分为两种方式：
### 1、使用纯 SQL 语句实现
#### 添加数据
```java
// 纯 SQL 方式添加数据
//通过 SQLiteOpenHelper 对象获取 SQLiteDatabase
SQLiteDatabase database = mOpenHelper.getWritableDatabase();
String sql = "insert into t_user (c_name,c_age,c_phone) values (?,?,?)";
// 执行数据库，参数以 Object[]的形式传递
database.execSQL(sql, new Object[] { 
				"zhangsan" + new Random().nextInt(100),
				new Random().nextInt(30), 
				"" + (5550 + new Random().nextInt(100)) });
//关闭数据库释放资源
database.close();
```

#### 删除数据
```java
// 删除年龄小于 21 的用户
SQLiteDatabase database = mOpenHelper.getReadableDatabase();
String sql = "delete from t_user where c_age<?";
database.execSQL(sql, new Object[] { 21 });
database.close();
```

#### 修改数据
```java
SQLiteDatabase database = mOpenHelper.getWritableDatabase();
String sql = "update t_user set c_name=? where c_age=?"; 
database.execSQL(sql, new Object[] { "lisi", 25 });
database.close();
```

#### 查询数据
【文件 1-6】 查询数据代码

```java
public void query1(View view) {
    //将查询到的数据封装到 User 集合中
    ArrayList<User> users = new ArrayList<User>();
    //获取 SQLiteDatabase
    SQLiteDatabase database = mOpenHelper.getReadableDatabase();
    String sql = "select uid,c_name,c_age,c_phone from t_user where c_age<?";
	// 执行查询语句，返回游标对象，可以将游标看做指向结果集的指针
    Cursor cursor = database.rawQuery(sql, new String[]{"130"});
    // 游标的默认位置位于第一行数据的前面
    while (cursor.moveToNext()) {
        User user = new User();
        //从第 1 列获取 int 型数据，字段的顺序是由查询语句决定的
        int uid = cursor.getInt(0);
        //从第 2 列获取字符串数据
        String name = cursor.getString(1);
        //从第 3 列获取 int 型数据
        int age = cursor.getInt(2);
        String phone = cursor.getString(3);
        user.setAge(age);
        user.setName(name);
        user.setPhone(phone);
        user.setUid(uid);
        users.add(user);
    }
    cursor.close();
    database.close();
}
```
注意：
使用纯 SQL 语句操作适合 SQL 比较熟练的程序员。如果 SQL 掌握的不好，没关系，Android 提供了一套 API 可以帮助我们完成以上操作。

### 2、使用特有 API 实现
#### 添加数据
```java
//获取 SQLiteDatabase
SQLiteDatabase database = mOpenHelper.getWritableDatabase();
/* ContentValues底层是 Map 数据结构
* key 对应数据库表中字段
* value 是想插入的值 */
ContentValues values = new ContentValues();
values.put("c_name", "王五" + new Random().nextInt(10));
values.put("c_age", new Random().nextInt(50));
values.put("c_phone", "5556");
/*
* 第一个参数：表名，注意不是数据库名！
* 第二个参数：如果 ContentValues 为空，那么默认情况下是不允许插入空值的，
* 但是如果给该参数设置了一个指定的列名，那么就允许 ContentValues 为空，
*同时给该列插入 null 值。
* 第三个参数：要插入的数值，封装成了 ContentValues 对象\
* 返回 long 类型的值，代表该条记录在数据库中的 id */
long insert = database.insert(TABLE_NAME, null, values);
//释放资源
database.close();
```

####  删除数据
```java
SQLiteDatabase database = mOpenHelper.getReadableDatabase();
/*
* 第一个参数：表名
* 第二个参数：删除条件，比如 c_age=?或者 c_age<?
* 第三个参数：参数，用于替换？
* 返回值：删除成功的个数 */
int delete = database.delete(TABLE_NAME, "c_age<?", new String[] { "25" });
database.close();
```

#### 修改数据
```java
SQLiteDatabase database = mOpenHelper.getWritableDatabase();
ContentValues values = new ContentValues();
values.put("c_name", "wangwu");
// 将年龄等于 28 的姓名改为 wangwu
int update = database.update(TABLE_NAME, values, "c_age=?", new String[] { "28" });
database.close();
```

#### 查询数据
```java
ArrayList<User> users = new ArrayList<User>();
SQLiteDatabase database = mOpenHelper.getReadableDatabase();
/*
* 参数 1：表名
* 参数 2：要查询的字段
* 参数 3：查询条件
* 参数 4：条件中？对应的值
* 参数 5：分组查询参数
* 参数 6：分组查询条件
* 参数 7：排序字段和规则 */
Cursor cursor = database.query(TABLE_NAME, 
				new String[] {"uid", "c_name", "c_age", "c_phone" }, 
				"c_age<?", new String[] { "130" }, null, null, "c_age desc");
//对游标 Cursor 的遍历
while (cursor.moveToNext()) {
	User user = new User();
	int uid = cursor.getInt(0);
	String name = cursor.getString(1);
	int age = cursor.getInt(2);
	String phone = cursor.getString(3);
	user.setAge(age);
	user.setName(name);
	user.setPhone(phone);
	user.setUid(uid);
	users.add(user);
}
cursor.close();
database.close();
```

### 两种 SQLiteDatabase 的不同
SQLiteOpenHelper 有两个方法均可返回 SQLiteDatabase 对象：
一、getWritableDatabase()
该方法返回的对象和另外一个方法返回的对象没有任何差异，返回的对象对数据库都可以进行读、写操作，当磁盘已满或者权限不足的情况下该方法会抛出异常。

二、getReadableDatabase()
跟另外一个方法相比，在磁盘已满的情况下，该方法不会抛出异常，而是返回一个只读的数据库操作对象。 
根据这两种方法返回对象的差异，如果需要对数据库进行**查询操作则推荐使用后者**，如果**添加、修改、删除数据则推荐使用前者**。

## 数据库的升级和事务操作
### 数据库的升级
在创建 MySQLiteOpenHelper 对象的时候需要传递一个 int 类型的 version 参数，代表数据库的版本号，数值从 1 开始。如果在 new	MySQLiteOpenHelper 对象的时候传递的 version 大于先去创建的 version，那 么就会导致系统回调 onUpgrade 方法，从而实现了数据库的升级。

### 事务的操作
在 SQLiteDatabase 提供了对事务的支持，**处于事务中的操作都是“临时性”的**，只有事务**提交了 才会将数据保存到数据库**。
事务的使用不仅可以**保证数据**的**一致性**，也可以**提高批处理**时的执行**效率**。

SQLiteDatabase 提供的 **beginTransaction()打开事务**，**endTransaction()结束事务**。
注意：结束 事务并不代表事务提交，如果想让数据写入的数据库需要在结束事务前执行 **setTransactionSuccessful**()方法。这是**事务提交**的唯一方式。

如下：展示了当开启事务后，如果因为异常导致事务没有提提交，那么整个转账过程都不会成功。
```
//数据库的事务操作
SQLiteDatabase database = mOpenHelper.getReadableDatabase();
String sql = "update t_user set c_money=c_money-500 where c_name=?";
// 模拟转账
// 开启了事务，那么之后的操作都是在缓存中执行
database.beginTransaction();
database.execSQL(sql, new String[] { "wangwu" });
 
// 在事务中模拟一个异常的发生
int a = 1 / 0;
String sql2 = "update t_user set c_money=c_money+500 where c_name=?";
database.execSQL(sql2, new String[] { "lisi" });

// 只有执行该代码，才将缓存中的数据写到数据库中
database.setTransactionSuccessful();
database.endTransaction();
database.close();
```

如下：展示了如果批处理数据时，使用事务可以显著提高效率。运行结果见图 1-2。
【文件 1-13】	代码片段

```java
// 测试批处理效率
public void testBatch(View view) {
    SQLiteDatabase database = mOpenHelper.getWritableDatabase();
// 普通方式添加 10000 条记录
    String sql = "insert into t_user (c_name,c_age,c_phone) values (?,?,?)";
    long startTime = SystemClock.currentThreadTimeMillis();
    for (int i = 0; i < 1000; i++) {
        database.execSQL(sql, new Object[] { "zhangsan" + new Random().nextInt(100),
                new Random().nextInt(30), "" + (5550 + new Random().nextInt(100)) });
    }
    System.out.println("普通模式耗时："+(SystemClock.currentThreadTimeMillis()-startTime)+"毫秒");
// 开启事务的方式添加 1000 条记录
    startTime = SystemClock.currentThreadTimeMillis();
    database.beginTransaction();
    for(int i=0;i<1000;i++){
        database.execSQL(sql, new Object[] { "zhangsan" + new Random().nextInt(100),
                new Random().nextInt(30), "" + (5550 + new Random().nextInt(100)) });
    }
    database.setTransactionSuccessful();
    database.endTransaction();
    System.out.println("事务模式耗时："+(SystemClock.currentThreadTimeMillis()-startTime)+"毫秒");
    // 关闭数据库释放资源
    database.close();
}
```
![](http://upload-images.jianshu.io/upload_images/9028834-f1603995bbae51db.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
图 1-2	事务执行效率对比

通过如图 1-2 的测试结果我们发现使用事务大大提高了批量处理的效率。

## sqlite3 工具的使用
sqlite3 是 Android 内置的操作数据库工具，使用该工具可以直接对 SQLite 数据库进行操作。 操作步骤（如图 1-3）：
1、在命令行界面使用 adb shell 命令进入 linux 内核
2、使用 cd 命令进入数据库所在目录（数据库的路径为”/data/data/应用包名/databases/数据库”）
3、使用”sqlite3 数据库名”进入数据库操作模式
![](http://upload-images.jianshu.io/upload_images/9028834-e859939b8d4f14af.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) 
图 1-3	sqlite3 使用截图



