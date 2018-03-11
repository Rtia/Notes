<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [【JAVA RxJava】](#java-rxjava)
  - [RxJava 到底是什么](#rxjava-%E5%88%B0%E5%BA%95%E6%98%AF%E4%BB%80%E4%B9%88)
  - [RxJava 好在哪](#rxjava-%E5%A5%BD%E5%9C%A8%E5%93%AA)
  - [API介绍和原理简析](#api%E4%BB%8B%E7%BB%8D%E5%92%8C%E5%8E%9F%E7%90%86%E7%AE%80%E6%9E%90)
    - [1. 概念：扩展的观察者模式](#1-%E6%A6%82%E5%BF%B5%E6%89%A9%E5%B1%95%E7%9A%84%E8%A7%82%E5%AF%9F%E8%80%85%E6%A8%A1%E5%BC%8F)
      - [观察者模式](#%E8%A7%82%E5%AF%9F%E8%80%85%E6%A8%A1%E5%BC%8F)
      - [RxJava 的观察者模式](#rxjava-%E7%9A%84%E8%A7%82%E5%AF%9F%E8%80%85%E6%A8%A1%E5%BC%8F)
        - [onCompleted和onError](#oncompleted%E5%92%8Conerror)
      - [](#)
    - [2. 基本实现](#2-%E5%9F%BA%E6%9C%AC%E5%AE%9E%E7%8E%B0)
      - [1) 创建 Observer](#1-%E5%88%9B%E5%BB%BA-observer)
        - [Subscriber](#subscriber)
        - [Observer  vs  Subscriber](#observer--vs--subscriber)
      - [2) 创建 Observable](#2-%E5%88%9B%E5%BB%BA-observable)
        - [创建事件队列方法](#%E5%88%9B%E5%BB%BA%E4%BA%8B%E4%BB%B6%E9%98%9F%E5%88%97%E6%96%B9%E6%B3%95)
          - [**create**()](#create)
          - [**just**(T...)](#justt)
          - [**from**(T[]) / from(Iterable<? extends T>) :](#fromt--fromiterable-extends-t-)
        - [](#-1)
      - [3) Subscribe (订阅)](#3-subscribe-%E8%AE%A2%E9%98%85)
        - [**Observable.subscribe(Subscriber)** 的**内部实现**：](#observablesubscribesubscriber-%E7%9A%84%E5%86%85%E9%83%A8%E5%AE%9E%E7%8E%B0)
        - [不完整定义的回调](#%E4%B8%8D%E5%AE%8C%E6%95%B4%E5%AE%9A%E4%B9%89%E7%9A%84%E5%9B%9E%E8%B0%83)
          - [**Action0**](#action0)
          - [**Action1**](#action1)
        - [](#-2)
      - [4) 场景示例](#4-%E5%9C%BA%E6%99%AF%E7%A4%BA%E4%BE%8B)
        - [a. 打印字符串数组](#a-%E6%89%93%E5%8D%B0%E5%AD%97%E7%AC%A6%E4%B8%B2%E6%95%B0%E7%BB%84)
        - [b. 由 id 取得图片并显示](#b-%E7%94%B1-id-%E5%8F%96%E5%BE%97%E5%9B%BE%E7%89%87%E5%B9%B6%E6%98%BE%E7%A4%BA)
      - [](#-3)
    - [3. 线程控制 —— Scheduler (一)](#3-%E7%BA%BF%E7%A8%8B%E6%8E%A7%E5%88%B6--scheduler-%E4%B8%80)
      - [1) Scheduler 的 API (一)](#1-scheduler-%E7%9A%84-api-%E4%B8%80)
        - [**Schedulers.immediate**():](#schedulersimmediate)
        - [**Schedulers.newThread**():](#schedulersnewthread)
        - [**Schedulers.io**():](#schedulersio)
        - [**Schedulers.computation**():](#schedulerscomputation)
        - [**AndroidSchedulers.mainThread**()](#androidschedulersmainthread)
        - [**subscribeOn**():](#subscribeon)
        - [**observeOn**():](#observeon)
      - [](#-4)
    - [4. 变换](#4-%E5%8F%98%E6%8D%A2)
      - [1) API](#1-api)
        - [map()](#map)
          - [map() 的示意图：](#map-%E7%9A%84%E7%A4%BA%E6%84%8F%E5%9B%BE)
        - [flatMap()](#flatmap)
          - [flatMap() 原理：](#flatmap-%E5%8E%9F%E7%90%86)
        - [throttleFirst():](#throttlefirst)
      - [2) 变换的原理：lift()](#2-%E5%8F%98%E6%8D%A2%E7%9A%84%E5%8E%9F%E7%90%86lift)
      - [3) compose: 对 Observable 整体的变换](#3-compose-%E5%AF%B9-observable-%E6%95%B4%E4%BD%93%E7%9A%84%E5%8F%98%E6%8D%A2)
    - [5. 线程控制：Scheduler (二)](#5-%E7%BA%BF%E7%A8%8B%E6%8E%A7%E5%88%B6scheduler-%E4%BA%8C)
      - [1) Scheduler 的 API (二)](#1-scheduler-%E7%9A%84-api-%E4%BA%8C)
      - [2) Scheduler 的原理（二）](#2-scheduler-%E7%9A%84%E5%8E%9F%E7%90%86%E4%BA%8C)
      - [3) 延伸：doOnSubscribe()](#3-%E5%BB%B6%E4%BC%B8doonsubscribe)
    - [](#-5)
  - [RxJava 的适用场景和使用方式](#rxjava-%E7%9A%84%E9%80%82%E7%94%A8%E5%9C%BA%E6%99%AF%E5%92%8C%E4%BD%BF%E7%94%A8%E6%96%B9%E5%BC%8F)
    - [1. 与 Retrofit 的结合](#1-%E4%B8%8E-retrofit-%E7%9A%84%E7%BB%93%E5%90%88)
    - [2. RxBinding](#2-rxbinding)
    - [3. 各种异步操作](#3-%E5%90%84%E7%A7%8D%E5%BC%82%E6%AD%A5%E6%93%8D%E4%BD%9C)
    - [4. RxBus](#4-rxbus)
  - [操作符的使用](#%E6%93%8D%E4%BD%9C%E7%AC%A6%E7%9A%84%E4%BD%BF%E7%94%A8)
    - [merge操作符，合并观察对象](#merge%E6%93%8D%E4%BD%9C%E7%AC%A6%E5%90%88%E5%B9%B6%E8%A7%82%E5%AF%9F%E5%AF%B9%E8%B1%A1)
    - [zip  操作符，合并多个观察对象的数据。并且允许 Func2（）函数重新发送合并后的数据](#zip--%E6%93%8D%E4%BD%9C%E7%AC%A6%E5%90%88%E5%B9%B6%E5%A4%9A%E4%B8%AA%E8%A7%82%E5%AF%9F%E5%AF%B9%E8%B1%A1%E7%9A%84%E6%95%B0%E6%8D%AE%E5%B9%B6%E4%B8%94%E5%85%81%E8%AE%B8-func2%E5%87%BD%E6%95%B0%E9%87%8D%E6%96%B0%E5%8F%91%E9%80%81%E5%90%88%E5%B9%B6%E5%90%8E%E7%9A%84%E6%95%B0%E6%8D%AE)
    - [scan累加器操作符](#scan%E7%B4%AF%E5%8A%A0%E5%99%A8%E6%93%8D%E4%BD%9C%E7%AC%A6)
    - [filter 过滤操作符的使用](#filter-%E8%BF%87%E6%BB%A4%E6%93%8D%E4%BD%9C%E7%AC%A6%E7%9A%84%E4%BD%BF%E7%94%A8)
    - [消息数量过滤操作符的使用](#%E6%B6%88%E6%81%AF%E6%95%B0%E9%87%8F%E8%BF%87%E6%BB%A4%E6%93%8D%E4%BD%9C%E7%AC%A6%E7%9A%84%E4%BD%BF%E7%94%A8)
      - [take ：取前n个数据](#take-%E5%8F%96%E5%89%8Dn%E4%B8%AA%E6%95%B0%E6%8D%AE)
      - [takeLast：取后n个数据](#takelast%E5%8F%96%E5%90%8En%E4%B8%AA%E6%95%B0%E6%8D%AE)
      - [first 只发送第一个数据](#first-%E5%8F%AA%E5%8F%91%E9%80%81%E7%AC%AC%E4%B8%80%E4%B8%AA%E6%95%B0%E6%8D%AE)
      - [last 只发送最后一个数据](#last-%E5%8F%AA%E5%8F%91%E9%80%81%E6%9C%80%E5%90%8E%E4%B8%80%E4%B8%AA%E6%95%B0%E6%8D%AE)
      - [skip() 跳过前n个数据发送后面的数据](#skip-%E8%B7%B3%E8%BF%87%E5%89%8Dn%E4%B8%AA%E6%95%B0%E6%8D%AE%E5%8F%91%E9%80%81%E5%90%8E%E9%9D%A2%E7%9A%84%E6%95%B0%E6%8D%AE)
      - [skipLast() 跳过最后n个数据，发送前面的数据](#skiplast-%E8%B7%B3%E8%BF%87%E6%9C%80%E5%90%8En%E4%B8%AA%E6%95%B0%E6%8D%AE%E5%8F%91%E9%80%81%E5%89%8D%E9%9D%A2%E7%9A%84%E6%95%B0%E6%8D%AE)
    - [elementAt 、elementAtOrDefault发送数据序列中第n个数据](#elementat-elementatordefault%E5%8F%91%E9%80%81%E6%95%B0%E6%8D%AE%E5%BA%8F%E5%88%97%E4%B8%AD%E7%AC%ACn%E4%B8%AA%E6%95%B0%E6%8D%AE)
    - [startWith() 插入数据](#startwith-%E6%8F%92%E5%85%A5%E6%95%B0%E6%8D%AE)
    - [delay操作符，延迟数据发送](#delay%E6%93%8D%E4%BD%9C%E7%AC%A6%E5%BB%B6%E8%BF%9F%E6%95%B0%E6%8D%AE%E5%8F%91%E9%80%81)
    - [Timer  延时操作符的使用](#timer--%E5%BB%B6%E6%97%B6%E6%93%8D%E4%BD%9C%E7%AC%A6%E7%9A%84%E4%BD%BF%E7%94%A8)
      - [delay vs timer](#delay-vs-timer)
    - [interval 轮询操作符，循环发送数据，数据从0开始递增](#interval-%E8%BD%AE%E8%AF%A2%E6%93%8D%E4%BD%9C%E7%AC%A6%E5%BE%AA%E7%8E%AF%E5%8F%91%E9%80%81%E6%95%B0%E6%8D%AE%E6%95%B0%E6%8D%AE%E4%BB%8E0%E5%BC%80%E5%A7%8B%E9%80%92%E5%A2%9E)
    - [doOnNext() 操作符，在每次 OnNext() 方法被调用前执行](#doonnext-%E6%93%8D%E4%BD%9C%E7%AC%A6%E5%9C%A8%E6%AF%8F%E6%AC%A1-onnext-%E6%96%B9%E6%B3%95%E8%A2%AB%E8%B0%83%E7%94%A8%E5%89%8D%E6%89%A7%E8%A1%8C)
    - [Buffer 操作符](#buffer-%E6%93%8D%E4%BD%9C%E7%AC%A6)
    - [throttleFirst 操作符在一段时间内，只取第一个事件，然后其他事件都丢弃。](#throttlefirst-%E6%93%8D%E4%BD%9C%E7%AC%A6%E5%9C%A8%E4%B8%80%E6%AE%B5%E6%97%B6%E9%97%B4%E5%86%85%E5%8F%AA%E5%8F%96%E7%AC%AC%E4%B8%80%E4%B8%AA%E4%BA%8B%E4%BB%B6%E7%84%B6%E5%90%8E%E5%85%B6%E4%BB%96%E4%BA%8B%E4%BB%B6%E9%83%BD%E4%B8%A2%E5%BC%83)
    - [distinct    过滤重复的数据](#distinct----%E8%BF%87%E6%BB%A4%E9%87%8D%E5%A4%8D%E7%9A%84%E6%95%B0%E6%8D%AE)
    - [debounce() 操作符一段时间内没有变化，就会发送一个数据](#debounce-%E6%93%8D%E4%BD%9C%E7%AC%A6%E4%B8%80%E6%AE%B5%E6%97%B6%E9%97%B4%E5%86%85%E6%B2%A1%E6%9C%89%E5%8F%98%E5%8C%96%E5%B0%B1%E4%BC%9A%E5%8F%91%E9%80%81%E4%B8%80%E4%B8%AA%E6%95%B0%E6%8D%AE)
    - [doOnSubscribe()](#doonsubscribe)
    - [range 操作符](#range-%E6%93%8D%E4%BD%9C%E7%AC%A6)
    - [defer 操作符](#defer-%E6%93%8D%E4%BD%9C%E7%AC%A6)
  - [生命周期控制和内存优化](#%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E6%8E%A7%E5%88%B6%E5%92%8C%E5%86%85%E5%AD%98%E4%BC%98%E5%8C%96)
    - [取消订阅 subscription.unsubscribe() ;](#%E5%8F%96%E6%B6%88%E8%AE%A2%E9%98%85-subscriptionunsubscribe-)
    - [线程调度](#%E7%BA%BF%E7%A8%8B%E8%B0%83%E5%BA%A6)
    - [rxlifecycle 框架的使用](#rxlifecycle-%E6%A1%86%E6%9E%B6%E7%9A%84%E4%BD%BF%E7%94%A8)
      - [**bindToLifecycle** 方法](#bindtolifecycle-%E6%96%B9%E6%B3%95)
      - [**bindUntilEvent**( ActivityEvent event)](#binduntilevent-activityevent-event)
      - [FragmentEvent](#fragmentevent)
    - [](#-6)
  - [RxBinding](#rxbinding)
    - [git地址](#git%E5%9C%B0%E5%9D%80)
    - [androidStudio 使用](#androidstudio-%E4%BD%BF%E7%94%A8)
    - [代码示例](#%E4%BB%A3%E7%A0%81%E7%A4%BA%E4%BE%8B)
      - [Button 防抖处理](#button-%E9%98%B2%E6%8A%96%E5%A4%84%E7%90%86)
      - [按钮的长按时间监听](#%E6%8C%89%E9%92%AE%E7%9A%84%E9%95%BF%E6%8C%89%E6%97%B6%E9%97%B4%E7%9B%91%E5%90%AC)
      - [listView 的点击事件、长按事件处理](#listview-%E7%9A%84%E7%82%B9%E5%87%BB%E4%BA%8B%E4%BB%B6%E9%95%BF%E6%8C%89%E4%BA%8B%E4%BB%B6%E5%A4%84%E7%90%86)
      - [用户登录界面，勾选同意隐私协议，登录按钮就变高亮](#%E7%94%A8%E6%88%B7%E7%99%BB%E5%BD%95%E7%95%8C%E9%9D%A2%E5%8B%BE%E9%80%89%E5%90%8C%E6%84%8F%E9%9A%90%E7%A7%81%E5%8D%8F%E8%AE%AE%E7%99%BB%E5%BD%95%E6%8C%89%E9%92%AE%E5%B0%B1%E5%8F%98%E9%AB%98%E4%BA%AE)
      - [搜索的时候，关键词联想功能 。debounce()在一定的时间内没有操作就会发送事件。](#%E6%90%9C%E7%B4%A2%E7%9A%84%E6%97%B6%E5%80%99%E5%85%B3%E9%94%AE%E8%AF%8D%E8%81%94%E6%83%B3%E5%8A%9F%E8%83%BD-debounce%E5%9C%A8%E4%B8%80%E5%AE%9A%E7%9A%84%E6%97%B6%E9%97%B4%E5%86%85%E6%B2%A1%E6%9C%89%E6%93%8D%E4%BD%9C%E5%B0%B1%E4%BC%9A%E5%8F%91%E9%80%81%E4%BA%8B%E4%BB%B6)
    - [](#-7)
  - [线程调度实例](#%E7%BA%BF%E7%A8%8B%E8%B0%83%E5%BA%A6%E5%AE%9E%E4%BE%8B)
    - [Rxjava默认运行的线程](#rxjava%E9%BB%98%E8%AE%A4%E8%BF%90%E8%A1%8C%E7%9A%84%E7%BA%BF%E7%A8%8B)
      - [结论](#%E7%BB%93%E8%AE%BA)
    - [subscribeOn、observeOn对线程影响](#subscribeonobserveon%E5%AF%B9%E7%BA%BF%E7%A8%8B%E5%BD%B1%E5%93%8D)
      - [结论](#%E7%BB%93%E8%AE%BA-1)
    - [多次切换线程](#%E5%A4%9A%E6%AC%A1%E5%88%87%E6%8D%A2%E7%BA%BF%E7%A8%8B)
    - [只规定了事件产生的线程](#%E5%8F%AA%E8%A7%84%E5%AE%9A%E4%BA%86%E4%BA%8B%E4%BB%B6%E4%BA%A7%E7%94%9F%E7%9A%84%E7%BA%BF%E7%A8%8B)
    - [只规定事件消费线程](#%E5%8F%AA%E8%A7%84%E5%AE%9A%E4%BA%8B%E4%BB%B6%E6%B6%88%E8%B4%B9%E7%BA%BF%E7%A8%8B)
      - [结论](#%E7%BB%93%E8%AE%BA-2)
    - [线程调度封装](#%E7%BA%BF%E7%A8%8B%E8%B0%83%E5%BA%A6%E5%B0%81%E8%A3%85)
      - [一般的用法：](#%E4%B8%80%E8%88%AC%E7%9A%84%E7%94%A8%E6%B3%95)
      - [简单的封装](#%E7%AE%80%E5%8D%95%E7%9A%84%E5%B0%81%E8%A3%85)
      - [改进后的封装](#%E6%94%B9%E8%BF%9B%E5%90%8E%E7%9A%84%E5%B0%81%E8%A3%85)
      - [最优改进后的封装](#%E6%9C%80%E4%BC%98%E6%94%B9%E8%BF%9B%E5%90%8E%E7%9A%84%E5%B0%81%E8%A3%85)
  - [相关推荐](#%E7%9B%B8%E5%85%B3%E6%8E%A8%E8%8D%90)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->



# 【JAVA RxJava】

**GitHub 链接**：   
https://github.com/ReactiveX/RxJava   
https://github.com/ReactiveX/RxAndroid   

**引入依赖**：   
compile 'io.reactivex:rxjava:1.0.14'   
compile 'io.reactivex:rxandroid:1.0.1'   
（版本号是文章发布时的最新稳定版）

## RxJava 到底是什么

一个词：**异步**。
RxJava 在 GitHub 主页上的自我介绍是 "a library for composing asynchronous and event-based programs using observable sequences for the Java VM"（一个在 Java VM上使用可观测的序列来组成异步的、基于事件的程序的库）。这就是 RxJava，概括得非常精准。
然而，对于初学者来说，这太难看懂了。因为它是一个『总结』，而初学者更需要一个『引言』。
其实， RxJava的本质可以压缩为异步这一个词。说到根上，它就是一个实现异步操作的库，而别的定语都是基于这之上的。

## RxJava 好在哪

换句话说，『同样是做异步，为什么人们用它，而不用现成的 AsyncTask / Handler / XXX/ ... ？』
一个词：**简洁**。
异步操作很关键的一点是程序的简洁性，因为在调度过程比较复杂的情况下，异步代码经常会既难写也难被读懂。
Android 创造的 AsyncTask 和Handler ，其实都是为了让异步代码更加简洁。RxJava的优势也是简洁，但它的简洁的与众不同之处在于，**随着程序逻辑变得越来越复杂，它依然能够保持简洁。**

**举例**：假设有这样一个需求：界面上有一个自定义的视图 imageCollectorView ，它的作用是显示多张图片，并能使用addImage(Bitmap) 方法来任意增加显示的图片。现在需要程序将一个给出的目录数组 File[]folders 中每个目录下的 png图片都加载出来并显示在 imageCollectorView 中。需要注意的是，由于读取图片的这一过程较为耗时，需要放在后台执行，而图片的显示则必须在UI 线程执行。常用的实现方式有多种，我这里贴出其中一种：

```java
new Thread() {
    @Override
    public void run() {
        super.run();
        for (File folder : folders) {
            File[] files = folder.listFiles();
            for (File file : files) {
                if (file.getName().endsWith(".png")) {
                    final Bitmap bitmap = getBitmapFromFile(file);
                    getActivity().runOnUiThread(new Runnable() {
                        @Override
                        public void run() {
                            imageCollectorView.addImage(bitmap);
                        }
                    });
                }
            }
        }
    }
}.start();
```

而如果使用 RxJava ，实现方式是这样的：

```java
Observable.from(folders)
        .flatMap(new Func1<File, Observable<File>>() {
            @Override
            public Observable<File> call(File file) {
                return Observable.from(file.listFiles());
            }
        })
        .filter(new Func1<File, Boolean>() {
            @Override
            public Boolean call(File file) {
                return file.getName().endsWith(".png");
            }
        })
        .map(new Func1<File, Bitmap>() {
            @Override
            public Bitmap call(File file) {
                return getBitmapFromFile(file);
            }
        })
        .subscribeOn(Schedulers.io())
        .observeOn(AndroidSchedulers.mainThread())
        .subscribe(new Action1<Bitmap>() {
            @Override
            public void call(Bitmap bitmap) {
                imageCollectorView.addImage(bitmap);
            }
        });
```

那位说话了：『你这代码明明变多了啊！简洁个毛啊！』大兄弟你消消气，我说的是逻辑的简洁，不是单纯的代码量少（逻辑简洁才是提升读写代码速度的必杀技对不？）。观察一下你会发现，
RxJava的这个实现，是一条从上到下的链式调用，没有任何嵌套，这在逻辑的简洁性上是具有优势的。当需求变得复杂时，这种优势将更加明显（试想如果还要求只选取前10
张图片，常规方式要怎么办？如果有更多这样那样的要求呢？再试想，在这一大堆需求实现完两个月之后需要改功能，当你翻回这里看到自己当初写下的那一片迷之缩进，你能保证自己将迅速看懂，而不是对着代码重新捋一遍思路？）。
另外，如果你的 IDE 是 Android Studio ，其实每次打开某个 Java文件的时候，你会看到被自动 Lambda 化的预览，这将让你更加清晰地看到程序逻辑：

```java
Observable.from(folders)
        .flatMap((Func1) (folder) -> { Observable.from(file.listFiles()) })
        .filter((Func1) (file) -> { file.getName().endsWith(".png") })
        .map((Func1) (file) -> { getBitmapFromFile(file) })
        .subscribeOn(Schedulers.io())
        .observeOn(AndroidSchedulers.mainThread())
        .subscribe((Action1) (bitmap) -> { imageCollectorView.addImage(bitmap) });
```

> 如果你习惯使用 Retrolambda，你也可以直接把代码写成上面这种简洁的形式。而如果你看到这里还不知道什么是Retrolambda ，我不建议你现在就去学习它。
> 原因有两点：

1. Lambda是把双刃剑，它让你的代码简洁的同时，降低了代码的可读性，因此同时学习 RxJava 和Retrolambda 可能会让你忽略 RxJava 的一些技术细节；
2. Retrolambda 是 Java 6/7 对Lambda表达式的非官方兼容方案，它的向后兼容性和稳定性是无法保障的，因此对于企业项目，使用Retrolambda 是有风险的。所以，与很多 RxJava 的推广者不同，我并不推荐在学习RxJava 的同时一起学习 Retrolambda。
   事实上，我个人虽然很欣赏Retrolambda，但我从来不用它。

在Flipboard 的 Android代码中，有一段逻辑非常复杂，包含了多次内存操作、本地文件操作和网络操作，对象分分合合，线程间相互配合相互等待，一会儿排成人字，一会儿排成一字。如果使用常规的方法来实现，肯定是要写得欲仙欲死，然而在使用RxJava 的情况下，依然只是一条链式调用就完成了。它很长，但很清晰。
所以， RxJava 好在哪？就好在简洁，好在那把什么复杂逻辑都能穿成一条线的简洁。

## API介绍和原理简析

### 1. 概念：扩展的观察者模式

RxJava 的异步实现，是通过一种扩展的观察者模式来实现的。

#### 观察者模式

观察者模式面向的需求是：**A** 对象（观察者）对 B对象（被观察者）的某种变化高度敏感，需要**在 B变化的一瞬间做出反应**。
举个例子，新闻里喜闻乐见的警察抓小偷，警察需要在小偷伸手作案的时候实施抓捕。在这个例子里，警察是观察者，小偷是被观察者，警察需要时刻盯着小偷的一举一动，才能保证不会漏过任何瞬间。
程序的观察者模式和这种真正的『观察』略有不同，观察者不需要时刻盯着被观察者（例如A 不需要每过 2ms 就检查一次 B的状态），而是采用**注册**(Register)**或者称为**订阅**(Subscribe)**的方式，告诉被观察者：我需要你的某某状态，你要在它变化的时候通知我。
Android开发中一个比较典型的例子是点击监听器 OnClickListener 。对设置 OnClickListener 来说， View 是被观察者， OnClickListener 是观察者，二者通过 setOnClickListener() 方法达成订阅关系。订阅之后用户点击按钮的瞬间，Android Framework就会将点击事件发送给已经注册的 OnClickListener 。采取这样被动的观察方式，既省去了反复检索状态的资源消耗，也能够得到最高的反馈速度。当然，这也得益于我们可以随意定制自己程序中的观察者和被观察者，而警察叔叔明显无法要求小偷『你在作案的时候务必通知我』。
OnClickListener 的模式大致如下图：
![OnClickListener 观察者模式](http://img.blog.csdn.net/20171230170355298?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
如图所示，通过 setOnClickListener() 方法，Button 持有 OnClickListener 的引用（这一过程没有在图上画出）；当用户点击时，Button 自动调用 OnClickListener 的 onClick() 方法。
另外，如果把这张图中的概念抽象出来（Button ->被观察者、OnClickListener -> 观察者、setOnClickListener() ->订阅，onClick() ->事件），就由专用的观察者模式（例如只用于监听控件点击）转变成了通用的观察者模式。如下图：
![通用观察者模式](http://img.blog.csdn.net/20171230170505487?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
而 RxJava 作为一个工具库，使用的就是通用形式的观察者模式。

#### RxJava 的观察者模式

RxJava有四个基本概念：
**Observable** (可观察者，即被观察者)、 **Observer** (观察者)、 **subscribe** (订阅)、**事件**。Observable 和 Observer 通过 subscribe() 方法实现订阅关系，从而 Observable 可以在需要的时候发出事件来通知 Observer。
与传统观察者模式不同， RxJava的事件回调方法除了普通事件 **onNext**() （相当于 onClick() / onEvent()）之外，还定义了两个特殊的事件：**onCompleted**() 和 **onError**()。

##### onCompleted和onError

- **onCompleted**(): 事件队列完结。
  RxJava不仅把每个事件单独处理，还会把它们看做一个队列。RxJava    规定，当不会再有新的 onNext() 发出时，需要触发 onCompleted() 方法作为标志。
- **onError**(): 事件队列异常。
  在事件处理过程中出异常时，onError() 会被触发，同时队列自动终止，不允许再有事件发出。
- 在一个正确运行的事件序列中, **onCompleted() 和 onError() 有且只有一个**，并且是事件序列中的最后一个。需要注意的是，onCompleted() 和 onError() 二者也是**互斥**的，即在队列中调用了其中一个，就不应该再调用另一个。

RxJava 的观察者模式大致如下图：
![RxJava 的观察者模式](http://img.blog.csdn.net/20171230170421887?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

####  

### 2. 基本实现

基于以上的概念， RxJava 的基本实现主要有三点：

#### 1) 创建 Observer

Observer 即观察者，它决定事件触发的时候将有怎样的行为。 
RxJava中的 **Observer接口**的**实现方式**：

```java
Observer<String> observer = new Observer<String>() {
    @Override
    public void onNext(String s) {
        Log.d(tag, "Item: " + s);
    }
    @Override
    public void onCompleted() {
        Log.d(tag, "Completed!");
    }
    @Override
    public void onError(Throwable e) {
        Log.d(tag, "Error!");
    }
};
```

##### Subscriber

除了 Observer 接口之外，RxJava还内置了一个实现了 Observer 的**抽象类**：**Subscriber**。
 Subscriber 对 Observer 接口进行了一些扩展，但他们的基本使用方式是完全一样的：

```java
Subscriber<String> subscriber = new Subscriber<String>() {
    @Override
    public void onNext(String s) {
        Log.d(tag, "Item: " + s);
    }
    @Override
    public void onCompleted() {
        Log.d(tag, "Completed!");
    }
    @Override
    public void onError(Throwable e) {
        Log.d(tag, "Error!");
    }
};
```

不仅基本使用方式一样，实质上，**在 RxJava 的 subscribe过程中，Observer 也总是会先被转换成一个 Subscriber 再使用**。所以如果你只想使用基本功能，选择 Observer 和 Subscriber 是完全一样的。

##### Observer  vs  Subscriber

1. **onStart**(): 这是 Subscriber 增加的方法。
   它会在 subscribe刚开始，而事件还未发送之前被调用，可以用于做一些**准备工作**，例如数据的清零或重置。
   这是一个**可选方法**，默认情况下它的实现为空。需要注意的是，
   如果对准备工作的线程有要求（例如弹出一个显示进度的对话框，这必须在主线程执行）， **onStart**() 就不适用了，因为它**总**是**在subscribe所发生的线程被调用，而不能指定线程**。要在指定的线程来做准备工作，可以使用 doOnSubscribe() 方法，具体可以在后面的文中看到。
2. **unsubscribe**(): 这是 Subscriber 所实现的另一个接口 Subscription 的方法，用于**取消订阅**。
   在这个方法被调用后，Subscriber 将不再接收事件。一般在这个方法调用前，可以使用 **isUnsubscribed() 先判断一下状态**。 
   unsubscribe() 这个方法很重要，因为在 subscribe() 之后， Observable 会持有 Subscriber 的引用，这个引用如果不能及时被释放，将有内存泄露的风险。所以最好保持一个原则：要**在不再使用的时候尽快在合适的地方（例如 onPause() onStop() 等方法中）调用 unsubscribe() 来解除引用关系，以避免内存泄露**的发生。

#### 2) 创建 Observable

Observable 即被观察者，它决定什么时候触发事件以及触发怎样的事件。 
RxJava使用 **create()** 方法来**创建**一个 Observable ，并为它定义事件触发规则：

```java
Observable observable = Observable.create(new Observable.OnSubscribe<String>()
{
    @Override
    public void call(Subscriber<? super String> subscriber) {
        subscriber.onNext("Hello");
        subscriber.onNext("Hi");
        subscriber.onNext("Aloha");
        subscriber.onCompleted();
    }
});
```

可以看到，这里传入了一个 OnSubscribe 对象作为参数。OnSubscribe 会被存储在返回的 Observable 对象中，它的作用相当于一个计划表，当 Observable 被订阅的时候，OnSubscribe 的 call() 方法会自动被调用，事件序列就会依照设定依次触发（对于上面的代码，就是观察者Subscriber 将会被调用三次 onNext() 和一次 onCompleted()）。这样，由被观察者调用了观察者的回调方法，就实现了由被观察者向观察者的事件传递，即观察者模式。

##### 创建事件队列方法

###### **create**()

是 RxJava 最基本的创造事件序列的方法。
基于这个方法， RxJava还提供了一些方法用来快捷创建事件队列，例如：

###### **just**(T...)

将传入的参数依次发送出来。

```java
Observable observable = Observable.just("Hello", "Hi", "Aloha");
// 将会依次调用：
// onNext("Hello");
// onNext("Hi");
// onNext("Aloha");
// onCompleted();
```

###### **from**(T[]) / from(Iterable<? extends T>) :

将传入的数组或 Iterable 拆分成具体对象后，依次发送出来。

```java
String[] words = {"Hello", "Hi", "Aloha"};
Observable observable = Observable.from(words);
// 将会依次调用：
// onNext("Hello");
// onNext("Hi");
// onNext("Aloha");
// onCompleted();
```

上面 just(T...) 的例子和 from(T[]) 的例子，都和之前的 create(OnSubscribe) 的例子是等价的。

#####  

#### 3) Subscribe (订阅)

创建了 Observable 和 Observer 之后，再用 subscribe() 方法将它们联结起来，整条链子就可以工作了。代码形式很简单：

```java
observable.subscribe(observer);
// 或者：
observable.subscribe(subscriber);
```

有人可能会注意到， subscribe() 这个方法有点怪：它看起来是『observalbe 订阅了 observer / subscriber』而不是『observer / subscriber 订阅了 observalbe』，这看起来就像『杂志订阅了读者』一样颠倒了对象关系。这让人读起来有点别扭，不过如果把API设计成 observer.subscribe(observable) / subscriber.subscribe(observable) ，虽然更加符合思维逻辑，但对流式API 的设计就造成影响了，比较起来明显是得不偿失的。

##### **Observable.subscribe(Subscriber)** 的**内部实现**：

注意：这不是 subscribe()的源码，而是将源码中与性能、兼容性、扩展性有关的代码剔除后的核心代码。
如果需要看源码，可以去 RxJava 的 GitHub 仓库下载。

```java
public Subscription subscribe(Subscriber subscriber) {
    subscriber.onStart();
    onSubscribe.call(subscriber);
    return subscriber;
}
```

可以看到，subscriber() 做了3件事：

1. 调用 Subscriber.onStart() 。这个方法在前面已经介绍过，是一个可选的准备方法。
2. 调用 Observable 中的 OnSubscribe.call(Subscriber) 。在这里，事件发送的逻辑开始运行。从这也可以看出，在RxJava中， Observable 并不是在创建的时候就立即开始发送事件，而是在它被订阅的时候，即当 subscribe() 方法执行的时候。
3. 将传入的 Subscriber 作为 Subscription 返回。这是为了方便 unsubscribe().
   整个过程中对象间的关系如下图：
   ![关系静图](http://img.blog.csdn.net/20171230170451933?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
   或者可以看动图：
    [![关系动图](http://img.blog.csdn.net/20171230170436683?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)](http://img.blog.csdn.net/20171230170436683?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

##### 不完整定义的回调

除了 subscribe(Observer) 和 subscribe(Subscriber) ，subscribe() 还支持不完整定义的回调，RxJava会自动根据定义创建出 Subscriber 。形式如下：

```java
Action1<String> onNextAction = new Action1<String>() {
    // onNext()
    @Override
    public void call(String s) {
        Log.d(tag, s);
    }
};
Action1<Throwable> onErrorAction = new Action1<Throwable>() {
    // onError()
    @Override
    public void call(Throwable throwable) {
// Error handling
    }
};
Action0 onCompletedAction = new Action0() {
    // onCompleted()
    @Override
    public void call() {
        Log.d(tag, "completed");
    }
};
// 自动创建 Subscriber ，并使用 onNextAction 来定义 onNext()
observable.subscribe(onNextAction);
// 自动创建 Subscriber ，并使用 onNextAction 和 onErrorAction 来定义 onNext() 和onError()
observable.subscribe(onNextAction, onErrorAction);
// 自动创建 Subscriber ，并使用 onNextAction、 onErrorAction 和onCompletedAction 来定义 onNext()、 onError() 和 onCompleted()
observable.subscribe(onNextAction, onErrorAction, onCompletedAction);
```

简单解释一下这段代码中出现的 Action1 和 Action0。 

###### **Action0**

是 RxJava的一个接口，它只有一个方法 call()，这个方法是无参无返回值的；由于 onCompleted() 方法也是无参无返回值的，因此 Action0 可以被当成一个包装对象，将 onCompleted() 的内容打包起来将自己作为一个参数传入 subscribe() 以实现不完整定义的回调。这样其实也可以看做将 onCompleted() 方法作为参数传进了 subscribe()，相当于其他某些语言中的『闭包』。 

###### **Action1**

也是一个接口，它同样只有一个方法 call(T param)，这个方法也无返回值，但有一个参数；与 Action0 同理，由于 onNext(T obj) 和 onError(Throwable error) 也是单参数无返回值的，因此 Action1可以将 onNext(obj) 和 onError(error) 打包起来传入 subscribe() 以实现不完整定义的回调。

事实上，虽然 Action0 和 Action1在API 中使用最广泛，但 RxJava 是提供了多个 ActionX 形式的接口(例如 Action2, Action3) 的，它们可以被用以包装不同的无返回值的方法。

注：正如前面所提到的，Observer 和 Subscriber 具有相同的角色，而且 Observer 在 subscribe() 过程中最终会被转换成 Subscriber对象，因此，从这里开始，后面的描述我将用 Subscriber 来代替 Observer ，这样更加严谨。

#####  

#### 4) 场景示例

下面举两个例子：
为了把原理用更清晰的方式表述出来，本文中挑选的都是功能尽可能简单的例子，以至于有些示例代码看起来会有『画蛇添足』『明明不用RxJava 可以更简便地解决问题』的感觉。当你看到这种情况，不要觉得是因为 RxJava太啰嗦，而是因为在过早的时候举出真实场景的例子并不利于原理的解析，因此我刻意挑选了简单的情景。

##### a. 打印字符串数组

将字符串数组 names 中的所有字符串依次打印出来：

```java
String[] names = ...;
Observable.from(names)
        .subscribe(new Action1<String>() {
            @Override
            public void call(String name) {
                Log.d(tag, name);
            }
        });
```

##### b. 由 id 取得图片并显示

由指定的一个 drawable 文件id drawableRes 取得图片，并显示在 ImageView 中，并在出现异常的时候打印 Toast报错：

```java
int drawableRes = ...;
ImageView imageView = ...;
Observable.create(new OnSubscribe<Drawable>() {
    @Override
    public void call(Subscriber<? super Drawable> subscriber) {
        Drawable drawable = getTheme().getDrawable(drawableRes));
        subscriber.onNext(drawable);
        subscriber.onCompleted();
    }
}).subscribe(new Observer<Drawable>() {
    @Override
    public void onNext(Drawable drawable) {
        imageView.setImageDrawable(drawable);
    }
    @Override
    public void onCompleted() {
    }
    @Override
    public void onError(Throwable e) {
        Toast.makeText(activity, "Error!", Toast.LENGTH_SHORT).show();
    }
});
```

正如上面两个例子这样，创建出 Observable 和 Subscriber ，再用 subscribe() 将它们串起来，一次RxJava 的基本使用就完成了。非常简单。
然而，在 RxJava的默认规则中，事件的发出和消费都是在同一个线程的。也就是说，如果只用上面的方法，实现出来的只是一个同步的观察者模式。观察者模式本身的目的就是『后台处理，前台回调』的异步机制，因此异步对于RxJava 是至关重要的。而要实现异步，则需要用到 RxJava 的另一个概念： Scheduler 。

####  

### 3. 线程控制 —— Scheduler (一)

在**不指定线程的情况下**， RxJava遵循的是**线程不变**的原则，即：在哪个线程调用subscribe()，就在哪个线程生产事件；在哪个线程生产事件，就在哪个线程消费事件。如果需要切换线程，就需要用到 **Scheduler （调度器）**。

#### 1) Scheduler 的 API (一)

在RxJava 中，**Scheduler** ——调度器，相当于**线程控制器**，RxJava通过它来指定每一段代码应该运行在什么样的线程。
RxJava已经内置了几个 Scheduler ，它们已经适合大多数的使用场景：

##### **Schedulers.immediate**():

直接在**当前线程**运行，相当于不指定线程。这是**默认**的 Scheduler。

##### **Schedulers.newThread**():

**总**是**启用新线程**，并在新线程执行操作。

##### **Schedulers.io**():

I/O操作（读写文件、读写数据库、网络信息交互等）所使用的 Scheduler。
行为模式和 newThread() 差不多，区别在于 **io()** 的内部实现是是用一个无数量上限的线程池，可以**重用空闲的线程**，因此多数情况下 io() **比 newThread() 更有效率**。不要把计算工作放在 io() 中，可以避免创建不必要的线程。

##### **Schedulers.computation**():

计算所使用的 Scheduler。这个计算指的是 **CPU密集型计算**，即**不会被 I/O 等操作限制性能的操作**，例如**图形的计算**。
这个 Scheduler 使用的**固定的线程池**，大小为**CPU 核数**。不要把 I/O 操作放在 computation() 中，否则 **I/O操作的等待时间会浪费 CPU**。

##### **AndroidSchedulers.mainThread**()

它指定的操作将在 Android 主线程运行。

有了这几个 Scheduler ，就可以**使用 subscribeOn() 和 observeOn() 两个方法来对线程进行控制**了。

##### **subscribeOn**():

**指定 subscribe() 所发生的线程**，即 Observable.OnSubscribe 被激活时所处的线程。或者叫做事件产生的线程。

##### **observeOn**():

**指定 Subscriber 所运行在的线程**。或者叫做事件消费的线程。

文字叙述总归难理解，上代码：

```java
Observable.just(1, 2, 3, 4)
        .subscribeOn(Schedulers.io()) // 指定 subscribe() 发生在 IO 线程
        .observeOn(AndroidSchedulers.mainThread()) // 指定 Subscriber 的回调发生在主线程
        .subscribe(new Action1<Integer>() {
            @Override
            public void call(Integer number) {
                Log.d(tag, "number:" + number);
            }
        });
```

上面这段代码中，由于 subscribeOn(Schedulers.io()) 的指定，被创建的事件的内容 1、2、3、4 将会在IO 线程发出；而由于 observeOn(AndroidScheculers.mainThread())的指定，因此 subscriber 数字的打印将发生在主线程。
事实上，这种在 subscribe() 之前写上两句 subscribeOn(Scheduler.io()) 和 observeOn(AndroidSchedulers.mainThread()) 的使用方式非常常见，它**适用于多数的『后台线程取数据，主线程显示』的程序策略**。
而前面提到的由图片 id 取得图片并显示的例子，如果也加上这两句：

```java
int drawableRes = ...;
ImageView imageView = ...;
Observable.create(new OnSubscribe<Drawable>() {
    @Override
    public void call(Subscriber<? super Drawable> subscriber) {
        Drawable drawable = getTheme().getDrawable(drawableRes));
        subscriber.onNext(drawable);
        subscriber.onCompleted();
    }
})
        .subscribeOn(Schedulers.io()) // 指定 subscribe() 发生在 IO 线程
        .observeOn(AndroidSchedulers.mainThread()) // 指定 Subscriber 的回调发生在主线程
        .subscribe(new Observer<Drawable>() {
            @Override
            public void onNext(Drawable drawable) {
                imageView.setImageDrawable(drawable);
            }
            @Override
            public void onCompleted() {
            }
            @Override
            public void onError(Throwable e) {
                Toast.makeText(activity, "Error!", Toast.LENGTH_SHORT).show();
            }
        });
```

那么，加载图片将会发生在 IO线程，而设置图片则被设定在了主线程。这就意味着，即使加载图片耗费了几十甚至几百毫秒的时间，也不会造成丝毫界面的卡顿。

####  

### 4. 变换

RxJava提供了对事件序列进行变换的支持，这是它的核心功能之一，也是大多数人说『RxJava真是太好用了』的最大原因。**所谓变换，就是将事件序列中的对象或整个序列进行加工处理，转换成不同的事件或事件序列。**概念说着总是模糊难懂的，来看
API。

#### 1) API

##### map()

首先看一个 map() 的例子：

```java
Observable.just("images/logo.png") // 输入类型 String
        .map(new Func1<String, Bitmap>() {
            @Override
            public Bitmap call(String filePath) { // 参数类型 String
                return getBitmapFromPath(filePath); // 返回类型 Bitmap
            }
        })
        .subscribe(new Action1<Bitmap>() {
            @Override
            public void call(Bitmap bitmap) { // 参数类型 Bitmap
                showBitmap(bitmap);
            }
        });
```

> 这里出现了一个叫做 **Func1** 的类。它和 Action1 非常相似，也是 RxJava的一个接口，用于包装含有一个参数的方法。 
> Func1 和 Action 的区别在于， **Func1 包装**的是**有返回值**的方法。
> 另外，和 ActionX 一样， FuncX 也有多个，用于不同参数个数的方法。FuncX和 ActionX 的区别在 FuncX 包装的是有返回值的方法。

可以看到，map() 方法将参数中的 String 对象转换成一个 Bitmap 对象后返回，而在经过 map() 方法后，事件的参数类型也由 String转为了 Bitmap。这种直接变换对象并返回的，是最常见的也最容易理解的变换。

- **map()**: **事件对象的直接变换**，具体功能上面已经介绍过。它是 RxJava**最常用的变换**。 

###### map() 的示意图：

![map() 示意图](http://img.blog.csdn.net/20171230170341396?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

不过RxJava 的变换远不止这样，它不仅可以针对事件对象，还可以针对整个事件队列，这使得RxJava 变得非常灵活。

##### flatMap()

首先假设这么一种需求：假设有一个数据结构『学生』，现在需要打印出一组学生的名字。实现方式很简单：

```java
Student[] students = ...;
Subscriber<String> subscriber = new Subscriber<String>() {
    @Override
    public void onNext(String name) {
        Log.d(tag, name);
    }
...
};
Observable.from(students)
        .map(new Func1<Student, String>() {
            @Override
            public String call(Student student) {
                return student.getName();
            }
        })
        .subscribe(subscriber);
```

很简单。那么再假设：如果要打印出每个学生所需要修的所有课程的名称呢？（需求的区别在于，每个学生只有一个名字，但却有多个课程。）首先可以这样实现：

```java
Student[] students = ...;
Subscriber<Student> subscriber = new Subscriber<Student>() {
    @Override
    public void onNext(Student student) {
        List<Course> courses = student.getCourses();
        for (int i = 0; i < courses.size(); i++) {
            Course course = courses.get(i);
            Log.d(tag, course.getName());
        }
    }
...
};
Observable.from(students)
        .subscribe(subscriber);
```

依然很简单。那么如果我不想在 Subscriber 中使用 for循环，而是希望 Subscriber 中直接传入单个的 Course 对象呢（这对于代码复用很重要）？
用 map() 显然是不行的，因为 **map() 是一对一的转化**，而我现在的要求是一对多的转化。
那怎么才能把**一个**Student **转**化成**多个** Course 呢？这个时候，就需要用 **flatMap()** 了：

```java
Student[] students = ...;
Subscriber<Course> subscriber = new Subscriber<Course>() {
    @Override
    public void onNext(Course course) {
        Log.d(tag, course.getName());
    }
...
};
Observable.from(students)
        .flatMap(new Func1<Student, Observable<Course>>() {
            @Override
            public Observable<Course> call(Student student) {
                return Observable.from(student.getCourses());
            }
        })
        .subscribe(subscriber);
```

从上面的代码可以看出， flatMap() 和 map() 有一个相同点：它也是把传入的参数转化之后返回另一个对象。
但需要注意，和 map()不同的是， **flatMap() 中返回的是个 Observable 对象**，并且这个 Observable 对象并不是被直接发送到了 Subscriber 的回调方法中。

###### flatMap() 原理：

1. 使用传入的事件对象创建一个 Observable 对象；
2. 并不发送这个 Observable，而是将它激活，于是它开始发送事件；
3. 每一个创建出来的 Observable 发送的事件，都被汇入同一个 Observable ，而这个 Observable 负责将这些事件统一交给 Subscriber 的回调方法。

这三个步骤，把事件拆成了两级，**通过一组新创建的 Observable 将初始的对象『铺平』之后通过统一路径分发了下去**。而这个『铺平』就是 flatMap() 所谓的flat。
flatMap() 示意图：
![flatMap() 示意图](http://img.blog.csdn.net/20171230170230646?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

扩展：由于可以在嵌套的 Observable 中添加异步代码， flatMap() 也常用于嵌套的异步操作，例如嵌套的网络请求。示例代码（Retrofit + RxJava）：

```java
networkClient.token() // 返回 Observable<String>，在订阅时请求token，并在响应后发送 token
.flatMap(new Func1<String, Observable<Messages>>() {
    @Override
    public Observable<Messages> call(String token) {
// 返回Observable<Messages>，在订阅时请求消息列表，并在响应后发送请求到的消息列表
        return networkClient.messages();
    }
})
        .subscribe(new Action1<Messages>() {
            @Override
            public void call(Messages messages) {
// 处理显示消息列表
                showMessages(messages);
            }
        });
```

传统的嵌套请求需要使用嵌套的 Callback来实现。而通过 flatMap() ，可以把嵌套的请求写在一条链中，从而保持程序逻辑的清晰。

##### throttleFirst():

在每次事件触发后的**一定时间间隔内丢弃新的事件**。
常用作去**抖动过滤**，例如按钮的点击监听器：

```java
RxView.clickEvents(button)    // RxBinding 代码，后面的文章有解释 
        .throttleFirst(500, TimeUnit.MILLISECONDS) // 设置防抖间隔为 500ms
        .subscribe(subscriber);//妈妈再也不怕我的用户手抖点开两个重复的界面啦。
```

此外， RxJava 还提供很多便捷的方法来实现事件序列的变换，这里就不一一举例了。

#### 2) 变换的原理：lift()

这些变换虽然功能各有不同，但实质上都是**针对事件序列的处理和再发送**。
而在RxJava的内部，它们是基于同一个基础的变换方法： lift(Operator)。
首先看一下 **lift()** 的**内部实现**（仅核心代码）：
// 注意：这不是 lift()的源码，而是将源码中与性能、兼容性、扩展性有关的代码剔除后的核心代码。
// 如果需要看源码，可以去 RxJava 的 GitHub 仓库下载。

```java
public <R> Observable<R> lift (Operator < ? extends R, ?super T > operator){
    return Observable.create(new OnSubscribe<R>() {
        @Override
        public void call(Subscriber subscriber) {
            Subscriber newSubscriber = operator.call(subscriber);
            newSubscriber.onStart();
            onSubscribe.call(newSubscriber);
        }
    });
}
```

这段代码很有意思：它生成了一个新的 Observable 并返回，而且创建新 Observable 所用的参数 OnSubscribe 的回调方法 call() 中的实现竟然看起来和前面讲过的Observable.subscribe() 一样！然而它们并不一样哟~不一样的地方关键就在于第二行 onSubscribe.call(subscriber) 中的 onSubscribe**所指代的对象不同**——

- subscribe() 中这句话的 onSubscribe 指的是 Observable 中的 onSubscribe 对象，这个没有问题，但是 lift() 之后的情况就复杂了点。
- 当含有 lift() 时：   
  1. lift() 创建了一个 Observable 后，加上之前的原始 Observable，已经有两个 Observable 了；   
  2. 而同样地，新 Observable 里的新 OnSubscribe 加上之前的原始 Observable 中的原始 OnSubscribe，也就有了两个 OnSubscribe；   
  3. 当用户调用经过 lift() 后的 Observable 的 subscribe() 的时候，使用的是 lift() 所返回的新的 Observable ，于是它所触发的 onSubscribe.call(subscriber)，也是用的新 Observable 中的新 OnSubscribe，即在 lift() 中生成的那个 OnSubscribe；   
  4. 而这个新 OnSubscribe 的 call() 方法中的 onSubscribe ，就是指的原始 Observable 中的原始 OnSubscribe ，在这个 call()方法里，新 OnSubscribe 利用 operator.call(subscriber) 生成了一个新的 Subscriber（Operator 就是在这里，通过自己的 call() 方法将新 Subscriber 和原始 Subscriber 进行关联，并插入自己的『变换』代码以实现变换），然后利用这个新 Subscriber 向原始 Observable 进行订阅。   
- 这样就实现了 lift() 过程，有点**像一种代理机制，通过事件拦截和处理实现事件序列的变换。**
  精简掉细节的话，也可以这么说：在 Observable 执行了 lift(Operator) 方法之后，会返回一个新的 Observable，这个新的 Observable 会像一个代理一样，负责接收原始的 Observable 发出的事件，并在处理后发送给 Subscriber。
  如果你更喜欢具象思维，可以看图：
  ![lift() 原理图](http://img.blog.csdn.net/20171230170948478?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
  或者可以看动图：
  ![lift 原理动图](http://img.blog.csdn.net/20171230170932345?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
  两次和多次的 lift() 同理，如下图：
  ![两次 lift](http://img.blog.csdn.net/20171230170920314?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
  举一个具体的 Operator 的实现。下面这是一个将事件中的 Integer 对象转换成 String 的例子，仅供参考：

```java
observable.lift(new Observable.Operator<String, Integer>() {
    @Override
    public Subscriber<? super Integer> call(final Subscriber<? super String> subscriber) {
// 将事件序列中的 Integer 对象转换为 String 对象
        return new Subscriber<Integer>() {
            @Override
            public void onNext(Integer integer) {
                subscriber.onNext("" + integer);
            }
            @Override
            public void onCompleted() {
                subscriber.onCompleted();
            }
            @Override
            public void onError(Throwable e) {
                subscriber.onError(e);
            }
        };
    }
});
```

讲述 lift() 的原理只是为了让你更好地了解 RxJava，从而可以更好地使用它。然而不管你是否理解了 lift() 的原理，RxJava都不建议开发者自定义 Operator 来直接使用 lift()，而是建议尽量使用已有的 lift() 包装方法（如 map() flatMap() 等）进行组合来实现需求，因为直接使用lift() 非常容易发生一些难以发现的错误。

#### 3) compose: 对 Observable 整体的变换

除了 lift() 之外， Observable 还有一个变换方法叫做 compose(Transformer)。
它和 lift() 的区别在于， **lift()**是**针对事件项和事件序列**的，而 **compose()**是**针对Observable自身**进行变换。
举个例子，假设在程序中有多个 Observable ，并且他们都需要应用一组相同的 lift() 变换。你可以这么写：

```java
observable1
        .lift1()
        .lift2()
        .lift3()
        .lift4()
        .subscribe(subscriber1);
observable2
        .lift1()
        .lift2()
        .lift3()
        .lift4()
        .subscribe(subscriber2);
observable3
        .lift1()
        .lift2()
        .lift3()
        .lift4()
        .subscribe(subscriber3);
observable4
        .lift1()
        .lift2()
        .lift3()
        .lift4()
        .subscribe(subscriber1);
```

你觉得这样太不软件工程了，于是你改成了这样：

```java
private Observable liftAll (Observable observable){
    return observable
            .lift1()
            .lift2()
            .lift3()
            .lift4();
}
...
liftAll(observable1).subscribe(subscriber1);
liftAll(observable2).subscribe(subscriber2);
liftAll(observable3).subscribe(subscriber3);
liftAll(observable4).subscribe(subscriber4);
```

可读性、可维护性都提高了。可是 Observable 被一个方法包起来，这种方式对于 Observale 的灵活性似乎还是增添了那么点限制。怎么办？这个时候，就应该用 compose() 来解决了：

```java
public class LiftAllTransformer implements Observable.Transformer<Integer, String> {
    @Override
    public Observable<String> call(Observable<Integer> observable) {
        return observable
                .lift1()
                .lift2()
                .lift3()
                .lift4();
    }
}
...
Transformer liftAll = new LiftAllTransformer();
observable1.compose(liftAll).subscribe(subscriber1);
observable2.compose(liftAll).subscribe(subscriber2);
observable3.compose(liftAll).subscribe(subscriber3);
observable4.compose(liftAll).subscribe(subscriber4);
```

像上面这样，使用 compose() 方法，Observable 可以利用传入的 Transformer 对象的 call 方法直接对自身进行处理，也就不必被包在方法的里面了。

### 5. 线程控制：Scheduler (二)

除了灵活的变换，RxJava 另一个牛逼的地方，就是**线程**的**自由控制**。

#### 1) Scheduler 的 API (二)

前面讲到了，可以利用 subscribeOn() 结合 observeOn() 来实现线程控制，让事件的产生和消费发生在不同的线程。可是在了解了 map() flatMap() 等变换方法后，有些好事的（其实就是当初刚接触
RxJava 时的我）就问了：能不能多切换几次线程？
答案是：能。因为 observeOn() 指定的是 Subscriber 的线程，而这个 Subscriber 并不是（严格说应该为『不一定是』，但这里不妨理解为『不是』）subscribe() 参数中的 Subscriber ，而是 observeOn() 执行时的当前 Observable 所对应的 Subscriber ，即它的直接下级 Subscriber 。换句话说，**observeOn() 指定的是它之后的操作所在的线程**。因此如果有**多次切换线程**的需求，只要**在每个想要切换线程的位置调用一次 observeOn() 即可**。
上代码：

```java
Observable.just(1, 2, 3, 4) // IO 线程，由 subscribeOn() 指定
        .subscribeOn(Schedulers.io())
        .observeOn(Schedulers.newThread())
        .map(mapOperator) // 新线程，由 observeOn() 指定
        .observeOn(Schedulers.io())
        .map(mapOperator2) // IO 线程，由 observeOn() 指定
        .observeOn(AndroidSchedulers.mainThread)
        .subscribe(subscriber); // Android 主线程，由 observeOn() 指定
```

如上，通过 observeOn() 的多次调用，程序实现了线程的多次切换。
不过，不同于 observeOn() ， **subscribeOn()** 的位置放在哪里都可以，但它是**只能调用一次**的。
又有好事的（其实还是当初的我）问了：如果我非要调用多次 subscribeOn() 呢？会有什么效果？
这个问题先放着，我们还是从 RxJava 线程控制的原理说起吧。

#### 2) Scheduler 的原理（二）

其实， subscribeOn() 和 observeOn() 的内部实现，也是用的 lift()。具体看图（不同颜色的箭头表示不同的线程）：
subscribeOn() 原理图：
![subscribeOn() 原理](http://img.blog.csdn.net/20171230170904588?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

observeOn() 原理图：
![observeOn() 原理](http://img.blog.csdn.net/20171230170849451?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
从图中可以看出，subscribeOn() 和 observeOn() 都做了线程切换的工作（图中的"schedule..."部位）。
不同的是：

- **subscribeOn**()的线程切换发生在 **OnSubscribe 中**，即在它通知上一级 OnSubscribe 时，这时事件还没有开始发送，因此 subscribeOn() 的线程**控制**可以**从事件发出的开端就造成影响**；
- **observeOn**() 的线程切换则发生在它内建的 **Subscriber 中**，即发生在它即将给下一级 Subscriber 发送事件时，因此 observeOn() **控制**的是**它后面的线程**。

最后，我用一张图来解释当多个 subscribeOn() 和 observeOn() 混合使用时，线程调度是怎么发生的（由于图中对象较多，相对于上面的图对结构做了一些简化调整）：
![线程控制综合调用](http://img.blog.csdn.net/20171230170833520?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
图中共有 5处含有对事件的操作。由图中可以看出，①和②两处受第一个 subscribeOn() 影响，运行在红色线程；③和④处受第一个 observeOn() 的影响，运行在绿色线程；⑤处受第二个 onserveOn() 影响，运行在紫色线程；而第二个 subscribeOn() ，由于在通知过程中线程就被第一个 subscribeOn() 截断，因此对整个流程并没有任何影响。这里也就回答了前面的问题：当使用了多个 subscribeOn() 的时候，只有第一个 subscribeOn() 起作用。

#### 3) 延伸：doOnSubscribe()

虽然超过一个的 subscribeOn() 对事件处理的流程没有影响，但在流程之前却是可以利用的。
在前面讲 Subscriber 的时候，提到过 Subscriber 的 onStart() 可以用作流程开始前的初始化。然而 **onStart()** 由于**在 subscribe() 发生时就被调用了**，因此**不能指定线程**，而是**只能执行在 subscribe() 被调用时的线程**。这就导致如果 onStart() 中含有对线程有要求的代码（例如在界面上显示一个ProgressBar，这必须在主线程执行），将会有线程非法的风险，因为有时你无法预测 subscribe() 将会在什么线程执行。
而与 Subscriber.onStart() 相对应的，有一个方法 **Observable.doOnSubscribe()** 。它和 Subscriber.onStart() 同样是**在 subscribe() 调用后而且在事件发送前执行**，但区别在于它**可**以**指定线程**。
默认情况下， doOnSubscribe() 执行在 subscribe() 发生的线程；而如果**在 doOnSubscribe() 之后有 subscribeOn() 的话，它将执行在离它最近的 subscribeOn() 所指定的线程**。
示例代码：

```java
Observable.create(onSubscribe)
        .subscribeOn(Schedulers.io())
        .doOnSubscribe(new Action0() {
            @Override
            public void call() {
                progressBar.setVisibility(View.VISIBLE); // 需要在主线程执行
            }
        })
        .subscribeOn(AndroidSchedulers.mainThread()) // 指定主线程
        .observeOn(AndroidSchedulers.mainThread())
        .subscribe(subscriber);
```

如上，在 doOnSubscribe()的后面跟一个 subscribeOn() ，就能指定准备工作的线程了。

###  

## RxJava 的适用场景和使用方式

### 1. 与 Retrofit 的结合

Retrofit 是 Square 的一个著名的网络请求库。没有用过 Retrofit的可以选择跳过这一小节也没关系，我举的每种场景都只是个例子，而且例子之间并无前后关联，只是个抛砖引玉的作用，所以你跳过这里看别的场景也可以的。
Retrofit 除了提供了传统的 Callback 形式的 API，还有 RxJava版本的 Observable 形式 API。下面我用对比的方式来介绍 Retrofit 的 RxJava 版 API和传统版本的区别。
以获取一个 User 对象的接口作为例子。
使用Retrofit 的传统API，你可以用这样的方式来定义请求：

```java
@GET("/user")
public void getUser(@Query("userId") String userId, Callback<User> callback);
```

在程序的构建过程中， Retrofit会把自动把方法实现并生成代码，然后开发者就可以利用下面的方法来获取特定用户并处理响应：

```java
getUser(userId, new Callback<User>() {
    @Override
    public void success(User user) {
        userView.setUser(user);
    }
    @Override
    public void failure(RetrofitError error) {
// Error handling
...
    }
};
```

而使用 RxJava 形式的 API，定义同样的请求是这样的：

```java
@GET("/user")
public Observable<User> getUser(@Query("userId") String userId);

```

使用的时候是这样的：

```java
getUser(userId)
        .observeOn(AndroidSchedulers.mainThread())
        .subscribe(new Observer<User>() {
            @Override
            public void onNext(User user) {
                userView.setUser(user);
            }
            @Override
            public void onCompleted() {
            }
            @Override
            public void onError(Throwable error) {
// Error handling
...
            }
        });

```

看到区别了吗？
当 RxJava 形式的时候，Retrofit把请求封装进 Observable ，在请求结束后调用 onNext() 或在请求失败后调用 onError()。
对比来看， Callback 形式和 Observable 形式长得不太一样，但本质都差不多，而且在细节上 Observable 形式似乎还比 Callback形式要差点。那Retrofit 为什么还要提供 RxJava 的支持呢？
因为它好用啊！从这个例子看不出来是因为这只是最简单的情况。而一旦情景复杂起来， Callback 形式马上就会开始让人头疼。
比如：
假设这么一种情况：你的程序取到的 User 并不应该直接显示，而是需要先与数据库中的数据进行比对和修正后再显示。使用 Callback方式大概可以这么写：

```java
getUser(userId, new Callback<User>() {
    @Override
    public void success(User user) {
        processUser(user); // 尝试修正 User 数据
        userView.setUser(user);
    }
    @Override
    public void failure(RetrofitError error) {
// Error handling
...
    }
};

```

有问题吗？
很简便，但不要这样做。为什么？因为这样做会影响性能。数据库的操作很重，一次读写操作花费
10\~20ms是很常见的，这样的耗时很容易造成界面的卡顿。所以通常情况下，如果可以的话一定要避免在主线程中处理数据库。所以为了提升性能，这段代码可以优化一下：

```java
getUser(userId, new Callback<User>() {
    @Override
    public void success(User user) {
        new Thread() {
            @Override
            public void run() {
                processUser(user); // 尝试修正 User 数据
                runOnUiThread(new Runnable() { // 切回 UI 线程
                    @Override
                    public void run() {
                        userView.setUser(user);
                    }
                });
            }).start();
        }
        @Override
        public void failure(RetrofitError error) {
// Error handling
...
        }
    };

```

性能问题解决，但……这代码实在是太乱了，迷之缩进啊！杂乱的代码往往不仅仅是美观问题，因为代码越乱往往就越难读懂，而如果项目中充斥着杂乱的代码，无疑会降低代码的可读性，造成团队开发效率的降低和出错率的升高。
这时候，如果用 RxJava 的形式，就好办多了。 RxJava 形式的代码是这样的：

```java
getUser(userId)
    .doOnNext(new Action1<User>() {
        @Override
        public void call(User user) {
            processUser(user);
        })
    .observeOn(AndroidSchedulers.mainThread())
    .subscribe(new Observer<User>() {
        @Override
        public void onNext(User user) {
            userView.setUser(user);
        }

        @Override
        public void onCompleted() {
        }

        @Override
        public void onError(Throwable error) {
            // Error handling
            ...
        }
    });

```

后台代码和前台代码全都写在一条链中，明显清晰了很多。
再举一个例子：假设 /user 接口并不能直接访问，而需要填入一个在线获取的 token ，代码应该怎么写？
Callback 方式，可以使用嵌套的 Callback：

```java
@GET("/token")
public void getToken(Callback<String> callback);

@GET("/user")
public void getUser(@Query("token") String token, @Query("userId") String userId, Callback<User> callback);

...

getToken(new Callback<String>() {
    @Override
    public void success(String token) {
        getUser(token, userId, new Callback<User>() {
            @Override
            public void success(User user) {
                userView.setUser(user);
            }

            @Override
            public void failure(RetrofitError error) {
                // Error handling
                ...
            }
        };
    }

    @Override
    public void failure(RetrofitError error) {
        // Error handling
        ...
    }
});

```

倒是没有什么性能问题，可是迷之缩进毁一生，你懂我也懂，做过大项目的人应该更懂。
而使用 RxJava 的话，代码是这样的：

```java
@GET("/token")
public Observable<String> getToken();

@GET("/user")
public Observable<User> getUser(@Query("token") String token, @Query("userId") String userId);

...

getToken()
    .flatMap(new Func1<String, Observable<User>>() {
        @Override
        public Observable<User> onNext(String token) {
            return getUser(token, userId);
        })
    .observeOn(AndroidSchedulers.mainThread())
    .subscribe(new Observer<User>() {
        @Override
        public void onNext(User user) {
            userView.setUser(user);
        }

        @Override
        public void onCompleted() {
        }

        @Override
        public void onError(Throwable error) {
            // Error handling
            ...
        }
    });

```

用一个 flatMap() 就搞定了逻辑，依然是一条链。看着就很爽，是吧？
加上我写的一个 Sample 项目：   
[rengwuxian RxJava Samples](https://github.com/rengwuxian/RxJavaSamples)
好，Retrofit 部分就到这里。

### 2. RxBinding

[RxBinding](https://github.com/JakeWharton/RxBinding) 是 Jake Wharton的一个开源库，它提供了一套在 Android 平台上的基于 RxJava 的 Binding API。所谓Binding，就是类似设置 OnClickListener 、设置 TextWatcher 这样的注册绑定对象的API。
举个设置点击监听的例子。使用 RxBinding ，可以把事件监听用这样的方法来设置：

```java
Button button = ...;
RxView.clickEvents(button) // 以 Observable 形式来反馈点击事件
    .subscribe(new Action1<ViewClickEvent>() {
        @Override
        public void call(ViewClickEvent event) {
            // Click handling
        }
    });

```

看起来除了形式变了没什么区别，实质上也是这样。甚至如果你看一下它的源码，你会发现它连实现都没什么惊喜：它的内部是直接用一个包裹着的 setOnClickListener() 来实现的。然而，仅仅这一个形式的改变，却恰好就是 RxBinding 的目的：扩展性。通过 RxBinding 把点击监听转换成 Observable 之后，就有了对它进行扩展的可能。扩展的方式有很多，根据需求而定。一个例子是前面提到过的 throttleFirst() ，用于去抖动，也就是消除手抖导致的快速连环点击：

```java
RxView.clickEvents(button)
    .throttleFirst(500, TimeUnit.MILLISECONDS)
    .subscribe(clickAction);

```

如果想对 RxBinding 有更多了解，可以去它的 [GitHub项目](https://github.com/JakeWharton/RxBinding) 下面看看。

### 3. 各种异步操作

前面举的 Retrofit 和 RxBinding 的例子，是两个可以提供现成的 Observable 的库。而如果你有某些异步操作无法用这些库来自动生成 Observable，也完全可以自己写。例如数据库的读写、大图片的载入、文件压缩/解压等各种需要放在后台工作的耗时操作，都可以用RxJava 来实现，有了之前几章的例子，这里应该不用再举例了。

### 4. RxBus

RxBus 名字看起来像一个库，但它并不是一个库，而是一种模式，它的思想是使用 RxJava来实现了 EventBus ，而让你不再需要使用 Otto 或者 GreenRobot的 EventBus。至于什么是
RxBus，可以看[这篇文章](http://nerds.weddingpartyapp.com/tech/2014/12/24/implementing-an-event-bus-with-rxjava-rxbus/)。顺便说一句，Flipboard已经用 RxBus 替换掉了 Otto ，目前为止没有不良反应。

## 操作符的使用

### merge：合并观察对象

```java
List<String> list1 = new ArrayList<>() ;
List<String> list2 = new ArrayList<>() ;

list1.add( "1" ) ;
list1.add( "2" ) ;
list1.add( "3" ) ;

list2.add( "a" ) ;
list2.add( "b" ) ;
list2.add( "c" ) ;

Observable observable1 = Observable.from( list1 ) ;
Observable observable2 = Observable.from( list2 ) ;

//合并数据  先发送observable2的全部数据，然后发送 observable1的全部数据
Observable observable = Observable.merge( observable2 , observable1 ) ;

observable.subscribe(new Action1() {
    @Override
    public void call(Object o) {
      System.out.println( "rx-- " + o );
    }
}) ;

```

运行结果

![](http://img.blog.csdn.net/20171231014104929?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### zip：合并多个观察对象的数据。并且允许 Func2（）函数重新发送合并后的数据

```java
List<String> list1 = new ArrayList<>() ;
List<String> list2 = new ArrayList<>() ;

list1.add( "1" ) ;
list1.add( "2" ) ;
list1.add( "3" ) ;

list2.add( "a" ) ;
list2.add( "b" ) ;
list2.add( "c" ) ;
list2.add( "d" ) ;

Observable observable1 = Observable.from( list1 ) ;
Observable observable2 = Observable.from( list2 ) ;

Observable observable3 =  Observable.zip(observable1, observable2, new Func2<String , String , String >() {
   @Override
   public String call(String s1 , String s2 ) {
	   return s1 + s2  ;
   }
}) ;

observable3.subscribe(new Action1() {
   @Override
   public void call(Object o) {
	   System.out.println( "zip-- " + o );
   }
}) ;
```

运行效果：**从效果图上可以看出，合并两个的观察对象数据项应该是相等的；如果出现了数据项不等的情况，合并的数据项以最小数据队列为准。**
![](http://img.blog.csdn.net/20171231013955391?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### scan：累加

```java
Observable observable = Observable.just( 1 , 2 , 3 , 4 , 5  ) ;
observable.scan(new Func2<Integer,Integer,Integer>() {
           @Override
           public Integer call(Integer o, Integer o2) {
               return o + o2 ;
           }
       })
         .subscribe(new Action1() {
             @Override
             public void call(Object o) {
                 System.out.println( "scan-- " +  o );
             }
         })   ;
```

运行效果：     
第一次发射得到1，作为结果与2相加；发射得到3，作为结果与3相加，以此类推，打印结果：
![scan累加器操作符](http://img.blog.csdn.net/20171231014859559?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### filter：过滤

```java
Observable observable = Observable.just( 1 , 2 , 3 , 4 , 5 , 6 , 7 ) ;
observable.filter(new Func1<Integer , Boolean>() {
            @Override
            public Boolean call(Integer o) {
                //数据大于4的时候才会被发送
                return o > 4 ;
            }
        })
        .subscribe(new Action1() {
            @Override
            public void call(Object o) {
                System.out.println( "filter-- " +  o );
            }
        })   ;
```

运行效果
![filter 过滤操作符](http://img.blog.csdn.net/20171231014846352?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### 消息数量过滤操作符的使用

#### take：取前n个数据

#### takeLast：取后n个数据

#### first：只发送第一个数据

#### last：只发送最后一个数据

#### skip：跳过前n个数据发送后面的数据

#### skipLast：跳过最后n个数据，发送前面的数据

```java
//take 发送前3个数据
Observable observable = Observable.just( 1 , 2 , 3 , 4 , 5 , 6 , 7 ) ;
observable.take( 3 )
	   .subscribe(new Action1() {
		   @Override
		   public void call(Object o) {
			   System.out.println( "take-- " +  o );
		   }
	   })   ;

//takeLast 发送最后三个数据
Observable observable2 = Observable.just( 1 , 2 , 3 , 4 , 5 , 6 , 7 ) ;
observable2.takeLast( 3 )
	   .subscribe(new Action1() {
		   @Override
		   public void call(Object o) {
			   System.out.println( "takeLast-- " +  o );
		   }
	   })   ;

//first 只发送第一个数据
Observable observable3 = Observable.just( 1 , 2 , 3 , 4 , 5 , 6 , 7 ) ;
observable3.first()
	   .subscribe(new Action1() {
		   @Override
		   public void call(Object o) {
			   System.out.println( "first-- " +  o );
		   }
	   })   ;

//last 只发送最后一个数据
Observable observable4 = Observable.just( 1 , 2 , 3 , 4 , 5 , 6 , 7 ) ;
observable4.last()
	   .subscribe(new Action1() {
		   @Override
		   public void call(Object o) {
			   System.out.println( "last-- " +  o );
		   }
	   })   ;

//skip() 跳过前2个数据发送后面的数据
Observable observable5 = Observable.just( 1 , 2 , 3 , 4 , 5 , 6 , 7 ) ;
observable5.skip( 2 )
	   .subscribe(new Action1() {
		   @Override
		   public void call(Object o) {
			   System.out.println( "skip-- " +  o );
		   }
	   })   ;

//skipLast() 跳过最后两个数据，发送前面的数据
Observable observable6 = Observable.just( 1 , 2 , 3 , 4 , 5 , 6 , 7 ) ;
observable5.skipLast( 2 )
	   .subscribe(new Action1() {
		   @Override
		   public void call(Object o) {
			   System.out.println( "skipLast-- " +  o );
		   }
	   })   ;

```

效果图
![消息数量过滤操作符](http://img.blog.csdn.net/20171231014837150?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### elementAt 、elementAtOrDefault：发送数据序列中第n个数据

```java
//elementAt() 发送数据序列中第n个数据 ，序列号从0开始
//如果该序号大于数据序列中的最大序列号，则会抛出异常，程序崩溃
//所以在用elementAt操作符的时候，要注意判断发送的数据序列号是否越界

Observable observable7 = Observable.just( 1 , 2 , 3 , 4 , 5 , 6 , 7 ) ;
observable7.elementAt( 3 )
		.subscribe(new Action1() {
			@Override
			public void call(Object o) {
				System.out.println( "elementAt-- " +  o );
			}
		})   ;

//elementAtOrDefault( int n , Object default ) 发送数据序列中第n个数据 ，序列号从0开始。
//如果序列中没有该序列号，则发送默认值
Observable observable9 = Observable.just( 1 , 2 , 3 , 4 , 5 ) ;
observable9.elementAtOrDefault(  8 , 666  )
		.subscribe(new Action1() {
			@Override
			public void call(Object o) {
				System.out.println( "elementAtOrDefault-- " +  o );
			}
		})   ;

```

运行结果
![elementAt](http://img.blog.csdn.net/20171231014828107?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### startWith：插入数据

```java
//插入普通数据
//startWith 数据序列的开头插入一条指定的项 , 最多插入9条数据
Observable observable = Observable.just( "aa" , "bb" , "cc" ) ;
observable
        .startWith( "11" , "22" )
        .subscribe(new Action1() {
            @Override
            public void call(Object o) {
                System.out.println( "startWith-- " + o );
            }
        }) ;
 
//插入Observable对象
List<String> list = new ArrayList<>() ;
list.add( "ww" ) ;
list.add( "tt" ) ;
observable.startWith( Observable.from( list ))
        .subscribe(new Action1() {
            @Override
            public void call(Object o) {
                System.out.println( "startWith2 -- " + o );
            }
        }) ;
```

　　运行结果
![startWith()](http://img.blog.csdn.net/20171231014818246?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### delay：延迟数据发送

```java
Observable<String> observable = Observable.just( "1" , "2" , "3" , "4" , "5" , "6" , "7" , "8" ) ;
//延迟数据发射的时间，仅仅延时一次，也就是发射第一个数据前延时。发射后面的数据不延时
observable.delay( 3 , TimeUnit.SECONDS )  //延迟3秒钟
	   .subscribe(new Action1() {
		   @Override
		   public void call(Object o) {
			   System.out.println("delay-- " + o);
		   }
	   }) ;

```

### timer：延时

使用场景：xx秒后，执行xx     

```java
//5秒后输出 hello world , 然后显示一张图片
Observable.timer( 5 , TimeUnit.SECONDS )
		.observeOn(AndroidSchedulers.mainThread() )
		.subscribe(new Action1<Long>() {
			@Override
			public void call(Long aLong) {
				System.out.println( "timer--hello world " +  aLong );
				findViewById( R.id.image).setVisibility(View.VISIBLE );
			}
		}) ;
```

```java
timer 返回一个 Observable , 它在延迟一段给定的时间后发射一个简单的数字0
timer 操作符默认在computation调度器上执行，当然也可以用 Scheduler在定义执行的线程。
```

#### delay vs timer

- 相同点：delay 、 timer 都是延时操作符。
- 不同点：
  delay延时一次，延时完成后，可以连续发射多个数据。
  timer延时一次，延时完成后，只发射一次数据。

### interval：轮询，循环发送数据，数据从0开始递增

``` java
public class IntervalActivity extends AppCompatActivity { 
    Subscription subscription ;
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_interval);
 
        //参数一：延迟时间  参数二：间隔时间  参数三：时间颗粒度
        Observable observable =  Observable.interval(3000, 3000, TimeUnit.MILLISECONDS) ;
        subscription = observable.subscribe(new Action1() {
            @Override
            public void call(Object o) {
                System.out.println( "interval-  " + o );
            }
        })  ;
    }
 
    @Override
    protected void onDestroy() {
        super.onDestroy();
        if ( subscription != null ){
            subscription.unsubscribe();
        }
    }
}
```

![interval 轮询操作符](http://img.blog.csdn.net/20171231014808440?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### doOnNext：在每次 OnNext() 方法被调用前执行

使用场景：从网络请求数据，在数据被展示前，缓存到本地

``` java
Observable observable = Observable.just( "1" , "2" , "3" , "4" ) ;
observable.doOnNext(new Action1() {
               @Override
               public void call(Object o) {
                   System.out.println( "doOnNext--缓存数据" + o  );
               }
            })
	   .subscribe(new Observer() {
		   @Override
		   public void onCompleted() {
		   }

		   @Override
		   public void onError(Throwable e) {
		   }

		   @Override
		   public void onNext(Object o) {
			   System.out.println( "onNext--" + o  );
		   }
	   }) ;
```

![doOnNext() 操作符](http://img.blog.csdn.net/20171231014756279?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### buffer：打包

- buffer( int n )      把n个数据打成一个list包，然后再次发送。
- buffer( int n , int skip)   把n个数据打成一个list包，然后跳过第skip个数据。

使用场景：一个按钮每点击3次，弹出一个toast      

``` java
List<String> list = new ArrayList<>();
for (int i = 1; i < 10; i++) {
  list.add("" + i);
}

Observable<String> observable = Observable.from(list);
observable
	  .buffer(2)   //把每两个数据为一组打成一个包，然后发送
	  .subscribe(new Action1<List<String>>() {
		  @Override
		  public void call(List<String> strings) {
			  System.out.println( "buffer---------------" );
			  Observable.from( strings ).subscribe(new Action1<String>() {
				  @Override
				  public void call(String s) {
					  System.out.println( "buffer data --" + s  );
				  }
			  }) ;
		  }
	  });

```

![Buffer 操作符](http://img.blog.csdn.net/20171231014747010?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

**例子2：** 

``` java
//第1、2 个数据打成一个数据包，跳过第三个数据 ； 第4、5个数据打成一个包，跳过第6个数据
observable.buffer( 2 , 3 )  //把每两个数据为一组打成一个包，然后发送。第三个数据跳过去
	   .subscribe(new Action1<List<String>>() {
		   @Override
		   public void call(List<String> strings) {

			   System.out.println( "buffer22---------------" );
			   Observable.from( strings ).subscribe(new Action1<String>() {
				   @Override
				   public void call(String s) {
					   System.out.println( "buffer22 data --" + s  );
				   }
			   }) ;
		   }
	   }) ;

```

![Buffer 操作符2](http://img.blog.csdn.net/20171231014734518?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### throttleFirst：在一段时间内，只取第一个事件，其他事件都丢弃

**使用场景**：
1、button按钮防抖操作，防连续点击  
2、百度关键词联想，在一段时间内只联想一次，防止频繁请求服务器   

``` java
 Observable.interval( 1 , TimeUnit.SECONDS)
			.throttleFirst( 3 , TimeUnit.SECONDS )
			.subscribe(new Action1<Long>() {
				@Override
				public void call(Long aLong) {
					System.out.println( "throttleFirst--" + aLong );
				}
			}) ;

```

**这段代码，是循环发送数据，每秒发送一个。**throttleFirst( 3 , TimeUnit.SECONDS
)   **在3秒内只取第一个事件，其他的事件丢弃。**
运行结果
![throttleFirst 操作符](http://img.blog.csdn.net/20171231014723739?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### distinct：过滤重复的数据

```java
List<String> list = new ArrayList<>() ;
list.add( "1" ) ;
list.add( "2" ) ;
list.add( "1" ) ;
list.add( "3" ) ;
list.add( "4" ) ;
list.add( "2" ) ;
list.add( "1" ) ;
list.add( "1" ) ;

Observable.from( list )
		.distinct()
		.subscribe(new Action1<String>() {
			@Override
			public void call(String s) {
				System.out.println( "distinct--" + s );
			}
		}) ;

```

![distinct过滤重复的数据](http://img.blog.csdn.net/20171231014711548?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
从结果可以看出，重复的数据已经被过滤掉了

**distinctUntilChanged**()  过滤连续重复的数据

``` java
List<String> list = new ArrayList<>() ;
list.add( "1" ) ;
list.add( "2" ) ;
list.add( "1" ) ;
list.add( "3" ) ;
list.add( "4" ) ;
list.add( "4" ) ;
list.add( "2" ) ;
list.add( "1" ) ;
list.add( "1" ) ;

Observable.from( list )
		.distinctUntilChanged()
		.subscribe(new Action1<String>() {
			@Override
			public void call(String s) {
				System.out.println( "distinctUntilChanged--" + s );
			}
		}) ;

```

 运行结果
![distinctUntilChanged()过滤连续重复的数据](http://img.blog.csdn.net/20171231014700941?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
从结果可以看出，连续重复的数据已经被过滤掉了

### debounce：一段时间内没有变化，就会发送一个数据

使用场景：百度关键词联想提示。在输入的过程中是不会从服务器拉数据的。当输入结束后，在400毫秒没有输入就会去获取数据。
 避免了，多次请求给服务器带来的压力.

### doOnSubscribe：事件发出之前做一些初始化的工作

使用场景： 可以在**事件发出之前做一些初始化的工作**，比如弹出进度条等等
**注意**：

1. doOnSubscribe()默认运行在事件产生的线程里面，然而事件产生的线程一般都会运行在 io
   线程里。那么这个时候做一些，更新UI的操作，是线程不安全的。

   >- 所以如果事件产生的线程是io线程，但是我们又要在doOnSubscribe()更新UI ，这时候就需要线程切换。

2. 如果在 doOnSubscribe() 之后有 subscribeOn() 的话，它将执行在离它最近的 subscribeOn() 所指定的线程。 

3. subscribeOn() ：事件产生的线程 ； observeOn() : 事件消费的线程

``` java
Observable.create(onSubscribe)
.subscribeOn(Schedulers.io())
.doOnSubscribe(new Action0() {
	@Override
	public void call() {
		progressBar.setVisibility(View.VISIBLE); // 需要在主线程执行
	}
})
.subscribeOn(AndroidSchedulers.mainThread()) // 指定主线程
.observeOn(AndroidSchedulers.mainThread())
.subscribe(subscriber);

```

### range：发射一个范围内的有序整数序列

首先看range 方法的源码

``` java
public static Observable<Integer> range(int start, int count) {
     if (count < 0) {
         throw new IllegalArgumentException("Count can not be negative");
     }
     if (count == 0) {
         return Observable.empty();
     }
     if (start > Integer.MAX_VALUE - count + 1) {
         throw new IllegalArgumentException("start + count can not exceed Integer.MAX_VALUE");
     }
     if(count == 1) {
         return Observable.just(start);
     }
     return Observable.create(new OnSubscribeRange(start, start + (count - 1)));
 }
 
  
 //可以通过第三个参数控制range执行的线程
 public static Observable<Integer> range(int start, int count, Scheduler scheduler) {
     return range(start, count).subscribeOn(scheduler);
 }
```

Range操作符**发射一个范围内的有序整数序列，你可以指定范围的起始和长度**。
RxJava将这个操作符实现为range函数，它接受两个参数，一个是范围的起始值，一个是范围的数据的数目。如果你将第二个参数设为0，将导致Observable不发射任何数据（如果设置为负数，会抛异常）。
range默认不在任何特定的调度器上执行。有一个变体可以通过可选参数指定Scheduler。
**例子**

``` java
Observable.range( 10 , 3 )
		  .subscribe(new Action1<Integer>() {
			  @Override
			  public void call(Integer integer) {
				  Log.v( "rx_range  " , "" + integer ) ;
			  }
		  }) ;
```

**结果**

```
/rx_range: 10  
/rx_range: 11  
/rx_range: 12

```

### defer：订阅者**订阅时**才创建Observable

例子

``` java
public class DeferActivity extends AppCompatActivity { 
    String i = "10" ;
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_defer);
 
        i = "11 " ;
 
        Observable<String> defer = Observable.defer(new Func0<Observable<String>>() {
            @Override
            public Observable<String> call() {
                return Observable.just( i ) ;
            }
        }) ;
 
        Observable test = Observable.just(  i ) ;
 
        i = "12" ;
 
        defer.subscribe(new Action1<String>() {
            @Override
            public void call(String s) {
                Log.v( "rx_defer  " , "" + s ) ;
            }
        }) ;
 
        test.subscribe(new Action1() {
            @Override
            public void call(Object o) {
                Log.v( "rx_just " , "" + o ) ;
            }
        }) ;
    }
}
```

结果

```
/rx_defer: 12  
/rx_just: 11
```

- 可以看到，**just**操作符是在**创建Observable**就进行了**赋值**操作，而**defer**是在订阅者**订阅时**才创建Observable，此时才进行真正的**赋值**操作。
- defer操作符会一直等待直到有观察者订阅它，然后它使用Observable工厂方法生成一个Observable。它对每个观察者都这样做，因此尽管每个订阅者都以为自己订阅的是同一个Observable，事实上**每个订阅者获取的是它们自己的单独的数据序列**。
- 在某些情况下，等待直到最后一分钟（就是直到订阅发生时）才生成Observable可以确保Observable包含最新的数据。

## 生命周期控制和内存优化

 RxJava使我们很方便的使用链式编程，代码看起来既简洁又优雅。但是RxJava使用起来也是有副作用的，使用**越来越多的订阅，内存开销也会变得很大**，稍不留神就会出现**内存溢出**的情况。

### 取消订阅 subscription.unsubscribe() ;

``` java
public class MainActivity extends AppCompatActivity {

    Subscription subscription ;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        subscription =  Observable.just( "123").subscribe(new Action1<String>() {
            @Override
            public void call(String s) {
                System.out.println( "tt--" + s );
            }
        }) ;
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        //取消订阅
        if ( subscription != null ){
            subscription.unsubscribe();
        }
    }
}

```

### 线程调度

**Scheduler**调度器，相当于线程控制器，介绍见上文【线程控制 —— Scheduler (一)】【线程控制：Scheduler (二)】

> 上面介绍了两种控制Rxjava生命周期的方式，第一种：取消订阅 ；第二种：线程切换。这两种方式都能有效的解决android内存的使用问题，但是在实际的项目中会出现很多订阅关系，那么取消订阅的代码也就越来越多。造成了项目很难维护。所以我们必须寻找其他可靠简单可行的方式，也就是下面要介绍的。

### rxlifecycle 框架的使用

- github地址： <https://github.com/trello/RxLifecycle>
- 在android studio 里面添加引用  
  compile 'com.trello:rxlifecycle-components:0.6.1'
- 让你的activity继承RxActivity,RxAppCompatActivity,RxFragmentActivity  
  让你的fragment继承RxFragment,RxDialogFragment;
  下面的代码就以RxAppCompatActivity举例

#### **bindToLifecycle** 方法

在子类使用Observable中的**compose**操作符，调用，完成Observable发布的事件和当前的组件绑定，实现生命周期同步。从而实现当前组件生命周期结束时，自动取消对Observable订阅。

``` java
public class MainActivity extends RxAppCompatActivity {
        TextView textView ;
        
        @Override
        protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        textView = (TextView) findViewById(R.id.textView);
    
        //循环发送数字
        Observable.interval(0, 1, TimeUnit.SECONDS)
            .subscribeOn( Schedulers.io())
            .compose(this.<Long>bindToLifecycle())   //这个订阅关系跟Activity绑定，Observable 和activity生命周期同步
            .observeOn( AndroidSchedulers.mainThread())
            .subscribe(new Action1<Long>() {
                @Override
                public void call(Long aLong) {
                    System.out.println("lifecycle--" + aLong);
                    textView.setText( "" + aLong );
                }
            });
       }
    }

```

上面的代码是Observable循环的发送数字，并且在textview中显示出来  
1、没加 compose(this.<Long>bindToLifecycle()) 当Activiry结束掉以后，Observable还是会不断的发送数字，订阅关系没有解除  
2、添加compose(this.<Long>bindToLifecycle()) 当Activity结束掉以后，Observable停止发送数据，订阅关系解除。

- 从上面的例子可以看出bindToLifecycle() 方法可以使Observable发布的事件和当前的Activity绑定，实现生命周期同步。也就是Activity的 onDestroy() 方法被调用后，Observable的订阅关系才解除。

#### **bindUntilEvent**( ActivityEvent event)

指定在Activity其他的生命状态和订阅关系保持同步

- ActivityEvent.CREATE: 在Activity的onCreate()方法执行后，解除绑定。
- ActivityEvent.START:在Activity的onStart()方法执行后，解除绑定。
- ActivityEvent.RESUME:在Activity的onResume()方法执行后，解除绑定。
- ActivityEvent.PAUSE: 在Activity的onPause()方法执行后，解除绑定。
- ActivityEvent.STOP:在Activity的onStop()方法执行后，解除绑定。
- ActivityEvent.DESTROY:在Activity的onDestroy()方法执行后，解除绑定。

``` java
 //循环发送数字
 Observable.interval(0, 1, TimeUnit.SECONDS)
		 .subscribeOn( Schedulers.io())
		 .compose(this.<Long>bindUntilEvent(ActivityEvent.STOP ))   //当Activity执行Onstop()方法是解除订阅关系
		 .observeOn( AndroidSchedulers.mainThread())
		 .subscribe(new Action1<Long>() {
			 @Override
			 public void call(Long aLong) {
				 System.out.println("lifecycle-stop-" + aLong);
				 textView.setText( "" + aLong );
			 }
		 });

```

经过测试发现，当Activity执行了onStop()方法后，订阅关系已经解除了。  

#### FragmentEvent

这个类是专门**处理订阅事件与Fragment生命周期同步**的大杀器

``` java
public enum FragmentEvent {
ATTACH,
CREATE,
CREATE_VIEW,
START,
RESUME,
PAUSE,
STOP,
DESTROY_VIEW,
DESTROY,
DETACH
}

```

可以看出FragmentEvent 和 ActivityEvent 类似，都是枚举类，用法是一样的。这里就不举例了！

###  

## RxBinding

**前言**：RxBinding 是 Jake Wharton 的一个开源库，它提供了一套在 Android
平台上的基于 RxJava的 Binding API。所谓 Binding，就是类似设置 OnClickListener
、设置 TextWatcher 这样的注册绑定对象的 API。

### git地址

<https://github.com/JakeWharton/RxBinding>

### androidStudio 使用

一般的包下面就用

```
compile 'com.jakewharton.rxbinding:rxbinding:0.4.0'
```

v4'support-v4' library bindings:

```
compile 'com.jakewharton.rxbinding:rxbinding-support-v4:0.4.0'
```

'appcompat-v7' library bindings:

```
compile 'com.jakewharton.rxbinding:rxbinding-appcompat-v7:0.4.0'

```

'design' library bindings:

```
compile 'com.jakewharton.rxbinding:rxbinding-design:0.4.0'
```

### 代码示例

#### Button 防抖处理

``` java
 button = (Button) findViewById( R.id.bt ) ;
 RxView.clicks( button )
		 .throttleFirst( 2 , TimeUnit.SECONDS )   //两秒钟之内只取一个点击事件，防抖操作
		 .subscribe(new Action1<Void>() {
			 @Override
			 public void call(Void aVoid) {
				 Toast.makeText(MainActivity.this, "点击了", Toast.LENGTH_SHORT).show();
			 }
		 }) ;

```

#### 按钮的长按时间监听

``` java
 button = (Button) findViewById( R.id.bt ) ;
 //监听长按时间
 RxView.longClicks( button)
      .subscribe(new Action1<Void>() {
          @Override
         public void call(Void aVoid) {
         Toast.makeText(MainActivity.this, "long click  ！！", Toast.LENGTH_SHORT).show();
         }
     }) ;

```

#### listView 的点击事件、长按事件处理

```java
listView = (ListView) findViewById( R.id.listview );
List<String> list = new ArrayList<>() ;
     for ( int i = 0 ; i < 40 ; i++ ){
         list.add( "sss" + i ) ;
     }
final ArrayAdapter<String> adapter = new ArrayAdapter<String>(this, android.R.layout.simple_expandable_list_item_1 );
adapter.addAll( list );
listView.setAdapter( adapter );

//item click event
RxAdapterView.itemClicks( listView )
     .subscribe(new Action1<Integer>() {
         @Override
         public void call(Integer integer) {
         Toast.makeText(ListActivity.this, "item click " + integer , Toast.LENGTH_SHORT).show();
             }
         }) ;

//item long click
RxAdapterView.itemLongClicks( listView)
     .subscribe(new Action1<Integer>() {
         @Override
         public void call(Integer integer) {
             Toast.makeText(ListActivity.this, "item long click " + integer , Toast.LENGTH_SHORT).show();
             }
         }) ;
```

#### 用户登录界面，勾选同意隐私协议，登录按钮就变高亮

```java
button = (Button) findViewById( R.id.login_bt );
checkBox = (CheckBox) findViewById( R.id.checkbox );
//checkedChanges
RxCompoundButton.checkedChanges( checkBox )
    .subscribe(new Action1<Boolean>() {
        @Override
        public void call(Boolean aBoolean) {
            button.setEnabled( aBoolean );
            button.setBackgroundResource( aBoolean ? R.color.button_yes : R.color.button_no );
            }
        }) ;
```

效果图
![](http://img.blog.csdn.net/20171231031202522?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
http://o7rvuansr.bkt.clouddn.com/rxbindingGIF.gif

#### 搜索的时候，关键词联想功能 。debounce()在一定的时间内没有操作就会发送事件。

``` java
editText = (EditText) findViewById( R.id.editText );
 listView = (ListView) findViewById( R.id.listview );

 final ArrayAdapter<String> adapter = new ArrayAdapter<String>(this, android.R.layout.simple_expandable_list_item_1 );
 listView.setAdapter( adapter );

 RxTextView.textChanges( editText )
             .debounce( 600 , TimeUnit.MILLISECONDS )
             .map(new Func1<CharSequence, String>() {
                 @Override
                 public String call(CharSequence charSequence) {
                     //get the keyword
                     String key = charSequence.toString() ;
                     return key ;
                 }
             })
             .observeOn( Schedulers.io() )
             .map(new Func1<String, List<String>>() {
                 @Override
                 public List<String> call(String keyWord ) {
                     //get list
                     List<String> dataList = new ArrayList<String>() ;
                     if ( ! TextUtils.isEmpty( keyWord )){
                         for ( String s : getData()  ) {
                             if (s != null) {
                                 if (s.contains(keyWord)) {
                                     dataList.add(s);
                                 }
                             }
                         }
                     }
                     return dataList ;
                 }
             })
             .observeOn( AndroidSchedulers.mainThread() )
             .subscribe(new Action1<List<String>>() {
                 @Override
                 public void call(List<String> strings) {
                     adapter.clear();
                     adapter.addAll( strings );
                     adapter.notifyDataSetChanged();
                 }
             }) ;

```

运行效果  

![](http://img.blog.csdn.net/20171231030222634?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
http://o7rvuansr.bkt.clouddn.com/rxbinding_edGIF.gif
**总结**：

- RxBinding就是把 发布--订阅 的模式用在了android控件的点击，文本变化上。通过RxBinding 把点击监听转换成 Observable 之后，就有了对它进行扩展的可能。
- RxBinding和rxlifecycle 结合起来使用，可以控制控件监听的生命周期。

###  

## 线程调度实例

### Rxjava默认运行的线程

**例1**

``` java
Observable
   .create(new Observable.OnSubscribe<String>() {
	   @Override
	   public void call(Subscriber<? super String> subscriber) {
		   Logger.v( "rx_call" , Thread.currentThread().getName()  );

		   subscriber.onNext( "dd");
		   subscriber.onCompleted();
	   }
   })
   .map(new Func1<String, String >() {
	   @Override
	   public String call(String s) {
		   Logger.v( "rx_map" , Thread.currentThread().getName()  );
		   return s + "88";
	   }
   })
   .subscribe(new Action1<String>() {
	   @Override
	   public void call(String s) {
		   Logger.v( "rx_subscribe" , Thread.currentThread().getName()  );
	   }
   }) ;

```

**结果**

```
/rx_call: main           -- 主线程  
/rx_map: main        --  主线程  
/rx_subscribe: main   -- 主线程

```

**例2**

``` java
 new Thread(new Runnable() {
	   @Override
	   public void run() {
		   Logger.v( "rx_newThread" , Thread.currentThread().getName()  );
		   rx();
	   }
   }).start();
 
void rx(){
   Observable
	   .create(new Observable.OnSubscribe<String>() {
		   @Override
		   public void call(Subscriber<? super String> subscriber) {
			   Logger.v( "rx_call" , Thread.currentThread().getName()  );

			   subscriber.onNext( "dd");
			   subscriber.onCompleted();
		   }
	   })
	   .map(new Func1<String, String >() {
		   @Override
		   public String call(String s) {
			   Logger.v( "rx_map" , Thread.currentThread().getName()  );
			   return s + "88";
		   }
	   })
	   .subscribe(new Action1<String>() {
		   @Override
		   public void call(String s) {
			   Logger.v( "rx_subscribe" , Thread.currentThread().getName()  );
		   }
	   }) ;
}

```

**      结果**

```
/rx_newThread: Thread-564   -- 子线程  
/rx_call: Thread-564              -- 子线程  
/rx_map: Thread-564            -- 子线程   
/rx_subscribe: Thread-564    -- 子线程

```

#### 结论

> 通过例1和例2，说明，Rxjava**默认运行在当前线程**中。如果当前线程是子线程，则rxjava运行在子线程；同样，当前线程是主线程，则rxjava运行在主线程

### subscribeOn、observeOn对线程影响

**例3**

``` java
Observable
   .create(new Observable.OnSubscribe<String>() {
	   @Override
	   public void call(Subscriber<? super String> subscriber) {
		   Logger.v( "rx_call" , Thread.currentThread().getName()  );
		   subscriber.onNext( "dd");
		   subscriber.onCompleted();
	   }
   })
   .subscribeOn(Schedulers.io())
   .observeOn(AndroidSchedulers.mainThread())
   .map(new Func1<String, String >() {
	   @Override
	   public String call(String s) {
		   Logger.v( "rx_map" , Thread.currentThread().getName()  );
		   return s + "88";
	   }
   })
   .subscribe(new Action1<String>() {
	   @Override
	   public void call(String s) {
		   Logger.v( "rx_subscribe" , Thread.currentThread().getName()  );
	   }
   }) ;

```

**结果**

```
/rx_call: RxCachedThreadScheduler-1    --io线程  
/rx_map: main                                     --主线程  
/rx_subscribe: main                              --主线程

```

**例4**

``` java
Observable
   .create(new Observable.OnSubscribe<String>() {
	   @Override
	   public void call(Subscriber<? super String> subscriber) {
		   Logger.v( "rx_call" , Thread.currentThread().getName()  );
		   subscriber.onNext( "dd");
		   subscriber.onCompleted();
	   }
   })
   .map(new Func1<String, String >() {
	   @Override
	   public String call(String s) {
		   Logger.v( "rx_map" , Thread.currentThread().getName()  );
		   return s + "88";
	   }
   })
   .subscribeOn(Schedulers.io())
   .observeOn(AndroidSchedulers.mainThread())
   .subscribe(new Action1<String>() {
	   @Override
	   public void call(String s) {
		   Logger.v( "rx_subscribe" , Thread.currentThread().getName()  );
	   }
   }) ;　

```

**结果**

```
/rx_call: RxCachedThreadScheduler-1     --io线程  
/rx_map: RxCachedThreadScheduler-1   --io线程  
/rx_subscribe: main                              --主线程

```

#### 结论

> 通过例3、例4 可以看出  .**subscribeOn**(Schedulers.io())和 .**observeOn**(AndroidSchedulers.mainThread())写的**位置不一样**，造成的**结果**也**不一样**。
>   从例4中可以看出 map()操作符默认运行在事件产生的线程之中。事件消费只是在 subscribe（） 里面。

- 对于 **create() , just() , from**()   等                 --- 事件**产生**   
- **map() , flapMap() , scan() , filter**()  等    --  事件**加工**
- **subscribe**()                                          --  事件**消费**

- 事件**产生**：默认运行在**当前线程**，可以由 **subscribeOn**()  自定义线程
  事件**加工**：默认跟事件**产生**的线程保持**一致**, 可以由 **observeOn**() 自定义线程
   事件**消费**：默认运行在**当前线程**，可以有**observeOn**() 自定义

### 多次切换线程

**例5  **

``` java
Observable
	.create(new Observable.OnSubscribe<String>() {
		@Override
		public void call(Subscriber<? super String> subscriber) {
			Logger.v( "rx_call" , Thread.currentThread().getName()  );

			subscriber.onNext( "dd");
			subscriber.onCompleted();
		}
	})

	.observeOn( Schedulers.newThread() )    //新线程
	.map(new Func1<String, String >() {
		@Override
		public String call(String s) {
			Logger.v( "rx_map" , Thread.currentThread().getName()  );
			return s + "88";
		}
	})

	.observeOn( Schedulers.io() )      //io线程
	.filter(new Func1<String, Boolean>() {
		@Override
		public Boolean call(String s) {
			Logger.v( "rx_filter" , Thread.currentThread().getName()  );
			return s != null ;
		}
	})

	.subscribeOn(Schedulers.io())     //定义事件产生线程：io线程
	.observeOn(AndroidSchedulers.mainThread())     //事件消费线程：主线程
	.subscribe(new Action1<String>() {
		@Override
		public void call(String s) {
			Logger.v( "rx_subscribe" , Thread.currentThread().getName()  );
		}
	}) ;

```

**结果**

```
/rx_call: RxCachedThreadScheduler-1           -- io 线程  
/rx_map: RxNewThreadScheduler-1             -- new出来的线程  
/rx_filter: RxCachedThreadScheduler-2        -- io线程  
/rx_subscribe: main                                   -- 主线程

```

### 只规定了事件产生的线程

**例6：**

``` java
Observable
	 .create(new Observable.OnSubscribe<String>() {
		 @Override
		 public void call(Subscriber<? super String> subscriber) {
			 Log.v( "rx--create " , Thread.currentThread().getName() ) ;
			 subscriber.onNext( "dd" ) ;
		 }
	 })
	 .subscribeOn(Schedulers.io())
	 .subscribe(new Action1<String>() {
		 @Override
		 public void call(String s) {
			 Log.v( "rx--subscribe " , Thread.currentThread().getName() ) ;
		 }
	 }) ;

```

**结果**

```
/rx--create: RxCachedThreadScheduler-4                      // io 线程  
/rx--subscribe: RxCachedThreadScheduler-4                 // io 线程

```

### 只规定事件消费线程

**例:7：**

``` java
Observable
   .create(new Observable.OnSubscribe<String>() {
	   @Override
	   public void call(Subscriber<? super String> subscriber) {
		   Log.v( "rx--create " , Thread.currentThread().getName() ) ;
		   subscriber.onNext( "dd" ) ;
	   }
   })
   .observeOn( Schedulers.newThread() )
   .subscribe(new Action1<String>() {
	   @Override
	   public void call(String s) {
		   Log.v( "rx--subscribe " , Thread.currentThread().getName() ) ;
	   }
   }) ;

```

**结果**

```
/rx--create: main                                           -- 主线程  
/rx--subscribe: RxNewThreadScheduler-1        --  new 出来的子线程 

```

#### 结论

> 从例6可以看出，如果**只规定**了事件**产生**的线程，那么事件**消费**线程将**跟随**事件**产生线程**。
> 从例7可以看出，如果**只规定**了事件**消费**的线程，那么事件**产生**的线程**和当前线程保持一致**。

### 线程调度封装

**例8：**
 在Android 常常有这样的场景，后台处理处理数据，前台展示数据。

#### 一般的用法：

``` java
Observable
	 .just( "123" )
	 .subscribeOn( Schedulers.io())
	 .observeOn( AndroidSchedulers.mainThread() )
	 .subscribe(new Action1() {
		 @Override
		 public void call(Object o) {
		 }
	 }) ;

```

#### 简单的封装

``` java
public Observable apply( Observable observable ){
   return observable.subscribeOn( Schedulers.io() )
            .observeOn( AndroidSchedulers.mainThread() ) ;
}

```

**使用**

``` java
apply( Observable.just( "123" ) )
              .subscribe(new Action1() {
                  @Override
                  public void call(Object o) {
 
                  }
              }) ;

```

**弊端**：虽然上面的这种封装可以做到线程调度的目的，但是它破坏了链式编程的结构，是编程风格变得不优雅。
改进：**Transformers**的使用（就是转化器的意思，把一种类型的Observable转换成另一种类型的Observable ）

#### 改进后的封装

```java
//Transformers转化器：把一种类型的Observable转换成另一种类型的Observable
Observable.Transformer schedulersTransformer = new  Observable.Transformer() {
    @Override public Object call(Object observable) {
        return ((Observable)  observable).subscribeOn(Schedulers.newThread())
                .observeOn(AndroidSchedulers.mainThread());
    }
};
```

**使用**

``` java
Observable
          .just( "123" )
          .compose( schedulersTransformer )
          .subscribe(new Action1() {
              @Override
              public void call(Object o) {
              }
          }) ;
```

**弊端**：虽然保持了链式编程结构的完整，但是**每次调用 .compose(schedulersTransformer ) 都是 new了一个对象**的。所以我们需要再次封装，尽量保证单例的模式。

#### 最优改进后的封装

``` java
public class RxUtil { 
    private final static Observable.Transformer schedulersTransformer 
    = new  Observable.Transformer() {
        @Override public Object call(Object observable) {
            return ((Observable)  observable).subscribeOn(Schedulers.newThread())
                    .observeOn(AndroidSchedulers.mainThread());
        }
    };
 
   public static  <T> Observable.Transformer<T, T> applySchedulers() {
        return (Observable.Transformer<T, T>) schedulersTransformer;
    } 
}
```

**使用**

``` java
Observable
	.just( "123" )
	.compose( RxUtil.<String>applySchedulers() )
	.subscribe(new Action1() {
		@Override
		public void call(Object o) {
		}
	}) ;
```

## 相关推荐

另外推荐几篇比较好的文章，有助于理解Rxjava  
[安卓客户端是如何使用 RxJava 的](http://www.apkbus.com/blog-705730-60600.html)  
[通过 RxJava 实现一个 Event Bus –RxBus](http://www.apkbus.com/blog-705730-60631.html)  
[玩透RxJava（一）基础知识](http://www.apkbus.com/blog-705730-60437.html)  
[RxJava 教程第二部分：事件流基础之过滤数据](http://www.apkbus.com/blog-705730-60617.html)  
[Meizhi Android之RxJava &Retrofit最佳实践](http://www.apkbus.com/blog-705730-60599.html)

**相关代码**： <http://git.oschina.net/zyj1609/RxAndroid_RxJava>  



引用：
[给 Android 开发者的 RxJava 详解](http://gank.io/post/560e15be2dca930e00da1083)
[RxJava 和 RxAndroid 二（操作符的使用）](http://www.cnblogs.com/zhaoyanjun/p/5502804.html)
[RxJava 和 RxAndroid 三（生命周期控制和内存优化）](http://www.cnblogs.com/zhaoyanjun/p/5523454.html)
[RxJava 和 RxAndroid 四（RxBinding的使用）](http://www.cnblogs.com/zhaoyanjun/p/5535651.html)
[RxJava 和 RxAndroid五（线程调度）](http://www.cnblogs.com/zhaoyanjun/p/5624395.html)






