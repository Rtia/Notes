<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [【IT 工具 查看native层源码】](#it-%E5%B7%A5%E5%85%B7-%E6%9F%A5%E7%9C%8Bnative%E5%B1%82%E6%BA%90%E7%A0%81)
  - [使用AndroidXRef查看native层源码](#%E4%BD%BF%E7%94%A8androidxref%E6%9F%A5%E7%9C%8Bnative%E5%B1%82%E6%BA%90%E7%A0%81)
    - [1登录AndroidXRef网站](#1%E7%99%BB%E5%BD%95androidxref%E7%BD%91%E7%AB%99)
    - [2 选择SDK版本](#2-%E9%80%89%E6%8B%A9sdk%E7%89%88%E6%9C%AC)
    - [3 进入搜索界面](#3-%E8%BF%9B%E5%85%A5%E6%90%9C%E7%B4%A2%E7%95%8C%E9%9D%A2)
    - [4 如何查看native源码](#4-%E5%A6%82%E4%BD%95%E6%9F%A5%E7%9C%8Bnative%E6%BA%90%E7%A0%81)
      - [4.1 如果你的native方法声明了static](#41-%E5%A6%82%E6%9E%9C%E4%BD%A0%E7%9A%84native%E6%96%B9%E6%B3%95%E5%A3%B0%E6%98%8E%E4%BA%86static)
      - [4.2 如果你的native方法没有声明static](#42-%E5%A6%82%E6%9E%9C%E4%BD%A0%E7%9A%84native%E6%96%B9%E6%B3%95%E6%B2%A1%E6%9C%89%E5%A3%B0%E6%98%8Estatic)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 【IT 工具 查看native层源码】

大多数源码，我们都能看到整个函数内部处理的过程。

但是有一些源码却是标明了native，在java中是找不到具体实现的。
native方法的具体实现是用C语言实现的，因为jdk就是用C语言编写的。当有一些需要和硬件打交道的方法，java是做不了的，于是它就偷懒声明一个native方法让c去写一个方法去和硬件打交道，c写好之后java直接调用即可。
![](http://upload-images.jianshu.io/upload_images/9028834-92023c85e927e615?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



## 使用AndroidXRef查看native层源码
### 1登录AndroidXRef网站
[官网地址](http://androidxref.com/)

### 2 选择SDK版本
![](http://upload-images.jianshu.io/upload_images/9028834-39d97ee7824046fa?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 3 进入搜索界面
![](http://upload-images.jianshu.io/upload_images/9028834-72f7628b69a97df2?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 4 如何查看native源码
####  4.1 如果你的native方法声明了static
例如下面
```java
private static native String getDlWarning();
```

直接在3中的搜索页面**Full Search**中输入“getDlWarning”，右边的In Projects选择“**select all**”,接着点击“**search**”,从搜索结果中找到后缀名带**有c的文件**（.cpp，.cc等等）即可

![](http://upload-images.jianshu.io/upload_images/9028834-e8d6eea7766800a2?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

####  4.2 如果你的native方法没有声明static
例如下面

```java
public native char charAt(int index);
```

这个时候要注意了，搜索词就不是charAt而是“**类名_方法名**”，我这个类是String类，因此搜索的时候就是“String_charAt”，其他的步骤和4.1一致

![](http://upload-images.jianshu.io/upload_images/9028834-5c2345cbd816e608?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**引用：**
[如何查看Java中的native源码？](http://blog.csdn.net/LosingCarryJie/article/details/78244823)



