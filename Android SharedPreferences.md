

# 【Android SharedPreferences】
SharedPreferences 简称 sp，是 Android 平台上一个轻量级的存储类，一般应用程序都会提供“设置” 或者“首选项”等这样的界面，那么这些设置就可以通过 sp 来保存。
在 Android 系统中该文件保存在：/data/data/包名 /shared_prefs 目录下。
## 获取 SharedPreferences 对象
```java
/ 第一个参数：sp 文件的名字，没有则创建
* 第二个参数：文件权限*/
sp = getSharedPreferences("info", MODE_PRIVATE);
```

## 添加/修改数据

```java
// 如果想往 sp 中添加、修改、删除数据则需要通过 sp 获取到 Editor
Editor editor = sp.edit();
// 设置数据
editor.putString(“name”, name);
editor.putString(“pwd”, pwd);
// 一定要记得执行提交方法，不然前面保存的数据没有任何效果
editor.commit();
```

## 获取数据

```java
/* 第一个参数相当于 key
第二个参数是该值如果获取不到的默认值 */
String name = sp.getString(“name”, “”);
String pwd = sp.getString(“pwd”, “”);
```

## 删除数据
```java
Editor edit = sp.edit();
//清空所有
edit.clear();
//删除 key 为 name 的数据
edit.remove(“name”);
//提交
edit.commit();
```

## sp 的连点操作
Editor 的每个方法都返回了自己本身，因此支持连点操作。将添加数据使用连点操作的方式修改 后如下：
```java
//连点操作
sp.edit().putString(“name”,name).putString(“pwd”, pwd).commit();
```




