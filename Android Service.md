<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [【Android Service】](#android-service)
  - [Service 简介（★★★）](#service-%E7%AE%80%E4%BB%8B%E2%98%85%E2%98%85%E2%98%85)
  - [Android 中的进程（★★）](#android-%E4%B8%AD%E7%9A%84%E8%BF%9B%E7%A8%8B%E2%98%85%E2%98%85)
    - [1. Android 中进程的种类](#1-android-%E4%B8%AD%E8%BF%9B%E7%A8%8B%E7%9A%84%E7%A7%8D%E7%B1%BB)
    - [2. 进程的回收机制](#2-%E8%BF%9B%E7%A8%8B%E7%9A%84%E5%9B%9E%E6%94%B6%E6%9C%BA%E5%88%B6)
    - [3. 为什么用服务而不是线程](#3-%E4%B8%BA%E4%BB%80%E4%B9%88%E7%94%A8%E6%9C%8D%E5%8A%A1%E8%80%8C%E4%B8%8D%E6%98%AF%E7%BA%BF%E7%A8%8B)
  - [Service 的生命周期（★★★★）](#service-%E7%9A%84%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E2%98%85%E2%98%85%E2%98%85%E2%98%85)
    - [A started service（标准开启模式）](#a-started-service%E6%A0%87%E5%87%86%E5%BC%80%E5%90%AF%E6%A8%A1%E5%BC%8F)
    - [A bound service（绑定模式）](#a-bound-service%E7%BB%91%E5%AE%9A%E6%A8%A1%E5%BC%8F)
      - [Service 的这两种生命周期并不是完全分开的](#service-%E7%9A%84%E8%BF%99%E4%B8%A4%E7%A7%8D%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E5%B9%B6%E4%B8%8D%E6%98%AF%E5%AE%8C%E5%85%A8%E5%88%86%E5%BC%80%E7%9A%84)
    - [Service 的生命周期图](#service-%E7%9A%84%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E5%9B%BE)
    - [整体生命周期（The entire lifetime）](#%E6%95%B4%E4%BD%93%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9Fthe-entire-lifetime)
    - [积极活动的生命时间(The active lifetime)](#%E7%A7%AF%E6%9E%81%E6%B4%BB%E5%8A%A8%E7%9A%84%E7%94%9F%E5%91%BD%E6%97%B6%E9%97%B4the-active-lifetime)
    - [Service 的生命周期回调函数](#service-%E7%9A%84%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E5%9B%9E%E8%B0%83%E5%87%BD%E6%95%B0)
      - [onCreate()](#oncreate)
      - [onBind()](#onbind)
      - [onStartCommand()](#onstartcommand)
        - [onStartCommand的参数](#onstartcommand%E7%9A%84%E5%8F%82%E6%95%B0)
        - [onStartCommand的返回值](#onstartcommand%E7%9A%84%E8%BF%94%E5%9B%9E%E5%80%BC)
      - [onDestroy()](#ondestroy)
    - [管理生命周期](#%E7%AE%A1%E7%90%86%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F)
  - [Service在清单文件中的声明](#service%E5%9C%A8%E6%B8%85%E5%8D%95%E6%96%87%E4%BB%B6%E4%B8%AD%E7%9A%84%E5%A3%B0%E6%98%8E)
  - [后台服务](#%E5%90%8E%E5%8F%B0%E6%9C%8D%E5%8A%A1)
    - [不可交互的后台服务（启动服务）](#%E4%B8%8D%E5%8F%AF%E4%BA%A4%E4%BA%92%E7%9A%84%E5%90%8E%E5%8F%B0%E6%9C%8D%E5%8A%A1%E5%90%AF%E5%8A%A8%E6%9C%8D%E5%8A%A1)
      - [① 服务端：创建服务类](#%E2%91%A0-%E6%9C%8D%E5%8A%A1%E7%AB%AF%E5%88%9B%E5%BB%BA%E6%9C%8D%E5%8A%A1%E7%B1%BB)
      - [② 配置服务](#%E2%91%A1-%E9%85%8D%E7%BD%AE%E6%9C%8D%E5%8A%A1)
      - [③ 客户端：启动服务和停止服务](#%E2%91%A2-%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%90%AF%E5%8A%A8%E6%9C%8D%E5%8A%A1%E5%92%8C%E5%81%9C%E6%AD%A2%E6%9C%8D%E5%8A%A1)
      - [④ 运行代码](#%E2%91%A3-%E8%BF%90%E8%A1%8C%E4%BB%A3%E7%A0%81)
    - [可交互的后台服务（绑定服务）](#%E5%8F%AF%E4%BA%A4%E4%BA%92%E7%9A%84%E5%90%8E%E5%8F%B0%E6%9C%8D%E5%8A%A1%E7%BB%91%E5%AE%9A%E6%9C%8D%E5%8A%A1)
      - [1. 扩展 Binder 类](#1-%E6%89%A9%E5%B1%95-binder-%E7%B1%BB)
        - [① 服务端：创建服务类](#%E2%91%A0-%E6%9C%8D%E5%8A%A1%E7%AB%AF%E5%88%9B%E5%BB%BA%E6%9C%8D%E5%8A%A1%E7%B1%BB-1)
        - [② 配置服务](#%E2%91%A1-%E9%85%8D%E7%BD%AE%E6%9C%8D%E5%8A%A1-1)
        - [③ 客户端：绑定服务和解除绑定服务](#%E2%91%A2-%E5%AE%A2%E6%88%B7%E7%AB%AF%E7%BB%91%E5%AE%9A%E6%9C%8D%E5%8A%A1%E5%92%8C%E8%A7%A3%E9%99%A4%E7%BB%91%E5%AE%9A%E6%9C%8D%E5%8A%A1)
        - [ServiceConnection](#serviceconnection)
        - [④ 运行代码](#%E2%91%A3-%E8%BF%90%E8%A1%8C%E4%BB%A3%E7%A0%81-1)
      - [2. 使用Messenger](#2-%E4%BD%BF%E7%94%A8messenger)
        - [① 服务端](#%E2%91%A0-%E6%9C%8D%E5%8A%A1%E7%AB%AF)
        - [② 配置服务](#%E2%91%A1-%E9%85%8D%E7%BD%AE%E6%9C%8D%E5%8A%A1-2)
        - [③ 客户端](#%E2%91%A2-%E5%AE%A2%E6%88%B7%E7%AB%AF)
        - [④ 运行代码](#%E2%91%A3-%E8%BF%90%E8%A1%8C%E4%BB%A3%E7%A0%81-2)
      - [3. AIDL](#3-aidl)
        - [实例-远程服务调用商城支付](#%E5%AE%9E%E4%BE%8B-%E8%BF%9C%E7%A8%8B%E6%9C%8D%E5%8A%A1%E8%B0%83%E7%94%A8%E5%95%86%E5%9F%8E%E6%94%AF%E4%BB%98)
      - [关于绑定服务的注意点](#%E5%85%B3%E4%BA%8E%E7%BB%91%E5%AE%9A%E6%9C%8D%E5%8A%A1%E7%9A%84%E6%B3%A8%E6%84%8F%E7%82%B9)
    - [混合性交互的后台服务](#%E6%B7%B7%E5%90%88%E6%80%A7%E4%BA%A4%E4%BA%92%E7%9A%84%E5%90%8E%E5%8F%B0%E6%9C%8D%E5%8A%A1)
  - [前台服务](#%E5%89%8D%E5%8F%B0%E6%9C%8D%E5%8A%A1)
      - [① 服务端：创建服务类](#%E2%91%A0-%E6%9C%8D%E5%8A%A1%E7%AB%AF%E5%88%9B%E5%BB%BA%E6%9C%8D%E5%8A%A1%E7%B1%BB-2)
      - [② 配置服务](#%E2%91%A1-%E9%85%8D%E7%BD%AE%E6%9C%8D%E5%8A%A1-3)
      - [③ 客户端：](#%E2%91%A2-%E5%AE%A2%E6%88%B7%E7%AB%AF)
  - [IntentService](#intentservice)
  - [AccessibilityService无障碍服务](#accessibilityservice%E6%97%A0%E9%9A%9C%E7%A2%8D%E6%9C%8D%E5%8A%A1)
  - [系统服务](#%E7%B3%BB%E7%BB%9F%E6%9C%8D%E5%8A%A1)
      - [判断Wifi是否开启](#%E5%88%A4%E6%96%ADwifi%E6%98%AF%E5%90%A6%E5%BC%80%E5%90%AF)
      - [获取系统最大音量](#%E8%8E%B7%E5%8F%96%E7%B3%BB%E7%BB%9F%E6%9C%80%E5%A4%A7%E9%9F%B3%E9%87%8F)
      - [获取当前音量](#%E8%8E%B7%E5%8F%96%E5%BD%93%E5%89%8D%E9%9F%B3%E9%87%8F)
      - [判断网络是否有连接](#%E5%88%A4%E6%96%AD%E7%BD%91%E7%BB%9C%E6%98%AF%E5%90%A6%E6%9C%89%E8%BF%9E%E6%8E%A5)
  - [如何保证服务不被杀死](#%E5%A6%82%E4%BD%95%E4%BF%9D%E8%AF%81%E6%9C%8D%E5%8A%A1%E4%B8%8D%E8%A2%AB%E6%9D%80%E6%AD%BB)
      - [onStartCommand方法，返回START_STICKY或START_REDELIVER_INTENT](#onstartcommand%E6%96%B9%E6%B3%95%E8%BF%94%E5%9B%9Estart_sticky%E6%88%96start_redeliver_intent)
      - [将服务设为前台服务startForeground](#%E5%B0%86%E6%9C%8D%E5%8A%A1%E8%AE%BE%E4%B8%BA%E5%89%8D%E5%8F%B0%E6%9C%8D%E5%8A%A1startforeground)
      - [开启两个服务，相互监听，相互启动](#%E5%BC%80%E5%90%AF%E4%B8%A4%E4%B8%AA%E6%9C%8D%E5%8A%A1%E7%9B%B8%E4%BA%92%E7%9B%91%E5%90%AC%E7%9B%B8%E4%BA%92%E5%90%AF%E5%8A%A8)
      - [onDestroy方法里重启service](#ondestroy%E6%96%B9%E6%B3%95%E9%87%8C%E9%87%8D%E5%90%AFservice)
      - [监听系统广播判断Service状态](#%E7%9B%91%E5%90%AC%E7%B3%BB%E7%BB%9F%E5%B9%BF%E6%92%AD%E5%88%A4%E6%96%ADservice%E7%8A%B6%E6%80%81)
      - [Application加上Persistent属性](#application%E5%8A%A0%E4%B8%8Apersistent%E5%B1%9E%E6%80%A7)
      - [提升service优先级（可能无效）](#%E6%8F%90%E5%8D%87service%E4%BC%98%E5%85%88%E7%BA%A7%E5%8F%AF%E8%83%BD%E6%97%A0%E6%95%88)
  - [服务Service与线程Thread的区别](#%E6%9C%8D%E5%8A%A1service%E4%B8%8E%E7%BA%BF%E7%A8%8Bthread%E7%9A%84%E5%8C%BA%E5%88%AB)
  - [Android 中服务的调用实例（★★★）](#android-%E4%B8%AD%E6%9C%8D%E5%8A%A1%E7%9A%84%E8%B0%83%E7%94%A8%E5%AE%9E%E4%BE%8B%E2%98%85%E2%98%85%E2%98%85)
    - [电话窃听器（★★★）](#%E7%94%B5%E8%AF%9D%E7%AA%83%E5%90%AC%E5%99%A8%E2%98%85%E2%98%85%E2%98%85)
    - [本地服务调用音乐播放器](#%E6%9C%AC%E5%9C%B0%E6%9C%8D%E5%8A%A1%E8%B0%83%E7%94%A8%E9%9F%B3%E4%B9%90%E6%92%AD%E6%94%BE%E5%99%A8)
- [Service 面试题](#service-%E9%9D%A2%E8%AF%95%E9%A2%98)
  - [Service和Activity交互的方式有哪些？](#service%E5%92%8Cactivity%E4%BA%A4%E4%BA%92%E7%9A%84%E6%96%B9%E5%BC%8F%E6%9C%89%E5%93%AA%E4%BA%9B)
    - [1、startService方式交互](#1startservice%E6%96%B9%E5%BC%8F%E4%BA%A4%E4%BA%92)
    - [2、bindService交互](#2bindservice%E4%BA%A4%E4%BA%92)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->



# 【Android Service】

## Service 简介（★★★）

很多情况下，一些与用户很少需要产生交互的应用程序，我们一般让它们在后台运 行就行了，而且在它们运行期间我们仍然能运行其他的应用。为了处理这种后台进程， Android 引入了 Service 的概念。

- Service 在 Android 中是一种**长生命周期的组件**，它不实现任何用户界面，是一个没有界面的 Activity
- Service 长期在后台运行, 执行**不关乎界面的一些操作**比如: 网易新闻服务,每隔 1 分钟去服务查看是否有最新新闻
- Service 和 Thread 有点相似，但是使用 Thread 不安全不严谨
- Service 和其他组件一样，都是**运行在主线程**中，因此不能用它来做耗时的操作。（Service默认并不会运行在子线程中，它也不运行在一个独立的进程中，它同样执行在UI线程中，因此，不要在Service中执行耗时的操作，除非你在Service中**创建子线程来完成耗时操作**）

## Android 中的进程（★★）

### 1. Android 中进程的种类

进程优先级由高到低，依次为：

1. Foreground process 前台进程
2. Visible process 可视进程, 可以看见, 但不可以交互.   
3. Service process 服务进程
4. Background process 后台进程
5. Empty process 空进程(当程序退出时, 进程没有被销毁, 而是变成了空进程)

### 2. 进程的回收机制

Android 系统有一套内存回收机制,会根据优先级进行回收。Android 系统会尽可能的维持程序的进程, 但是终究还是需要回收一些旧的进程节省内存提供给新的或者重要的进 程使用。

- 进程的回收顺序是：从低到高
- 当系统内存不够用时, 会把空进程一个一个回收掉
- 当系统回收所有的完空进程不够用时, 继续向上回收后台进程, 依次类推
- 但是当回收服务, 可视, 前台这三种进程时, 系统非必要情况下不会轻易回收, 如果需要回收掉这三种进程, 那么在系统内存够用时, 会再给重新启动进程;但是服务进程如果用户手动的关闭服务, 这时服务不会再重启了。

### 3. 为什么用服务而不是线程

进程中运行着线程， Android 应用程序刚启动都会开启一个进程给这个程序来使用。 
Android 一个应用程序把所有的**界面关闭**时, **进程**这时还没有被销毁, 现在**处于**的是**空进程**状态,**Thread 运行在空进程中, 很容易被销毁**。
**服务不容易被销毁**, 如果非法状态下被销毁了, 系统会在内存够用时, 重新启动。

## Service 的生命周期（★★★★）

service 的生命周期，从它被创建开始，到它被销毁为止，可以有两条不同的路径。

### A started service（标准开启模式）

  当应用组件（如 Activity）通过调用 **startService**() 启动服务时，服务即处于“启动”状态。
  一旦启动，服务即可**在后台无限期运行**，即使启动服务的组件已被销毁也不受影响，除非**手动调用才能停止服务**。（调用 **stopSelf**()方法或者其他组件调用 **stopService**()方法）
   已启动的服务通常是执行单一操作，而且**不会将结果返回给调用方**。
  

### A bound service（绑定模式）

  当应用组件通过调用 **bindService**() 绑定到服务时，服务即处于“绑定”状态。
  绑定服务提供了一个**客户端-服务器接口** IBinder 接口，**允许组件与服务进行交互、发送请求、获取结果**，甚至是利用**进程间通信 (IPC)** 跨进程执行这些操作。  
  仅当与另一个应用组件绑定时，绑定服务才会运行。多个组件可以同时绑定到该服务，但**全部取消绑定**后，该服务即会被**销毁**。（通过 unbindService()方法来关闭这种连接）

#### Service 的这两种生命周期并不是完全分开的

也就是说，你**可以和一个已经调用了 startService()而被开启的 service 进行绑定**。
比如，一个后台音乐 service 可能因调用 startService()方法而被开启了，稍后， 可能用户想要控制播放器或者得到一些当前歌曲的信息，可以通过 bindService()将一 个 activity 和 service 绑定。这种情况下，stopService()或 stopSelf()实际上并不能停止这个 service，除非所有的客户都解除绑定。

### Service 的生命周期图

![Service 的生命周期图](http://upload-images.jianshu.io/upload_images/9028834-4eb2be72632f96ae.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
这个图说明了 service 典型的回调方法，尽管这个图中将开启的 service 和绑定的service 分开，但是你需要记住，任何service 都潜在地允许绑定。所以，**一个被开启的 service 仍然可能被绑定**。（你可以看到两层嵌套的 service 的生命周期）

### 整体生命周期（The entire lifetime）

service 整体的生命时间是**从 onCreate()被调用开始，到 onDestroy()方法返回为止**。
和 activity 一样，service 在 onCreate()中进行它的初始化工作，在 onDestroy()中释放残留的资源。

比如，一个音乐播放 service 可以在 onCreate()中创建播放音乐的线程，在onDestory()中停止这个线程。

**onCreate() 和 onDestroy()会被所有的 service 调用**，不论 service 是通过startService()还是 bindService()建立。



### 积极活动的生命时间(The active lifetime)

service 积极活动的生命时间（active lifetime）是**从 onStartCommand() 或onBind()被调用开始**，它们各自处理由 startService()或 bindService()方法传过来的 Intent 对象。

如果 service 是被**开启**的，那么它的活动生命周期**和整个生命周期一同结束**。
如果 service 是被**绑定**的，它们它的活动生命周期是**在 onUnbind()方法返回后结束**。

> 尽管一个被开启的 service 是通过调用 stopSelf() 或 stopService()来停止的，没有一个对应的回调函数与之对应，即没有 onStop()回调方法。所以，当**调用了停止的方法**，**除非**这个 service 和客户组件**绑定**，否则系统将会**直接销毁**它，onDestory()方法会被调用，并且是这个时候唯一会被调用的回调方法。

### Service 的生命周期回调函数

和 activity 一样，service 也有一系列的生命周期回调函数，你可以实现它们来监测 service 状态的变化，并且在适当的时候执行适当的工作。

下面的 service 展示了每一个生命周期的方法：
不像是 activity 的生命周期回调函数，我们**不需要调用基类的实现**。

```java
public class TestService extends Service {
    int mStartMode; // indicates how to behave if the service is killed
    IBinder mBinder; // interface for clients that bind
    boolean mAllowRebind; // indicates whether onRebind should be used
    
    @Override
    public void onCreate() {
// The service is being created
    }
    
    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
// The service is starting, due to a call to startService()
        return mStartMode;
    }

    @Override
    public IBinder onBind(Intent intent) {
// A client is binding to the service with bindService()
        return mBinder;
    }

    @Override
    public boolean onUnbind(Intent intent) {
// All clients have unbound with unbindService()
        return mAllowRebind;
    }

    @Override
    public void onRebind(Intent intent) {
// A client is binding to the service with bindService(),
// after onUnbind() has already been called
    }

    @Override
    public void onDestroy() {
// The service is no longer used and is being destroyed
    }
}
```

#### onCreate()

  首次创建服务时，系统将调用此方法来执行**一次性设置程序**（在调用 onStartCommand() 或onBind() 之前）。如果服务**已在运行，则不会调用此方法**，该方法只调用一次

#### onBind()

  当另一个组件想通过调用 **bindService**() 与服务绑定（例如执行 RPC）时，系统将调用此方法。在此方法的实现中，必须**返回 一个IBinder 接口的实现类，供客户端用来与服务进行通信**。
  无论是启动状态还是绑定状态，此方法必须重写，但在**启动状态的情况下直接返回 null**。
  

#### onStartCommand()

  当另一个组件（如 Activity）通过调用 **startService**() 请求启动服务时，系统将调用此方法。（**启动服务**使用**startService**(Intent intent)方法，仅需要**传递一个Intent**对象即可，在Intent对象**中指定需要启动的服务**。）  
  对于启动服务，**一旦启动**将与访问它的组件无任何关联，即使访问它的组件被销毁了，这个服务也**一直运行**下去，直到手动调用停止服务才被销毁。在**服务**的**外部**，必须使用**stopService**()方法停止，在**服务**的**内部**可以调用**stopSelf**()方法停止当前服务。
  第一次调用startService方法时，onCreate方法、onStartCommand方法将依次被调用，而多次调用startService时，只有onStartCommand方法被调用，最后我们调用stopService方法停止服务时onDestory方法被回调，这就是启动状态下Service的执行周期。

##### onStartCommand的参数

  接着我们重新回过头来进一步分析onStartCommand（Intent intent, int flags, int startId），这个方法有3个传入参数，它们的含义如下：
**onStartCommand（Intent intent, int flags, int startId）**

- **intent** ：启动时，启动组件传递过来的Intent，如Activity可利用Intent封装所需要的参数并传递给Service
- **flags**：表示启动请求时是否有额外数据，可选值有 0，START_FLAG_REDELIVERY，START_FLAG_RETRY。
  - ***0***代表没有
  - ***START_FLAG_REDELIVERY*** 
    这个值代表了onStartCommand方法的返回值为START_REDELIVER_INTENT，而且在上一次服务被杀死前会去调用stopSelf方法停止服务。其中START_REDELIVER_INTENT意味着**当Service因内存不足而被系统kill后，则会重建服务**，并通过传递给服务的最后一个 Intent 调用 onStartCommand()，此时Intent时有值的。
  - ***START_FLAG_RETRY*** 
    该flag代表当**onStartCommand**调用后一直**没有返回值时**，会尝试重新去调用**onStartCommand**()。
- **startId** ： 指明当前服务的唯一ID，**与stopSelfResult** (int startId)**配合**使用，stopSelfResult 可以更安全**地根据ID停止服务**。

##### onStartCommand的返回值

  实际上onStartCommand的返回值int类型才是最最值得注意的，它有三种可选值， START_STICKY，START_NOT_STICKY，START_REDELIVER_INTENT，它们具体含义如下：

- **START_STICKY** 
  当Service因内存不足而被系统kill后，一段时间后内存再次空闲时，系统将会尝试重新创建此Service，一旦创建成功后将回调onStartCommand方法，但其中的Intent将是null，除非有挂起的Intent，如pendingintent，这个状态下比较适用于不执行命令、但无限期运行并等待作业的媒体播放器或类似服务。
- **START_NOT_STICKY** 
  当Service因内存不足而被系统kill后，即使系统内存再次空闲时，系统也不会尝试重新创建此Service。除非程序中再次调用startService启动此Service，这是最安全的选项，可以避免在不必要时以及应用能够轻松重启所有未完成的作业时运行服务。
- **START_REDELIVER_INTENT** 
  当Service因内存不足而被系统kill后，则会重建服务，并通过传递给服务的最后一个 Intent 调用 onStartCommand()，任何挂起 Intent均依次传递。与START_STICKY不同的是，其中的传递的Intent将是非空，是最后一次调用startService中的intent。这个值适用于主动执行应该立即恢复的作业（例如下载文件）的服务。

由于**每次启动服务**（调用startService）时，**onStartCommand方法都会被调用**，因此我们可以**通过该方法使用Intent给Service传递所需要的参数**，然后**在onStartCommand方法中处理的事件**，最后根据需求选择不同的Flag返回值，以达到对程序更友好的控制。
  

#### onDestroy()

  当服务不再使用且将被销毁时，系统将调用此方法。服务应该实现此方法来清理所有资源，如线程、注册的侦听器、接收器等，这是服务接收的最后一个调用。
  

### 管理生命周期

当绑定 service 和所有客户端解除绑定之后，Android 系统将会销毁它，（除非它同时被 onStartCommand()方法开启）。因此，如果你的 service 是一个纯粹的**绑定 service**，那么你**不需要管理它的生命周期**。

然而，如果你选择实现 **onStartCommand**()回调方法，那么你**必须显式地停止 service**，因为 service 此时被看做是开启的。这种情况下，service 会一直运行到它 自己调用 stopSelf()或另一个组件调用 stopService()，不论它是否和客户端绑定。

另外，如果你的 service 被开启并且接受绑定，那么当系统调用你的 onUnbind() 方法时，如果你想要在**下次**客户端**绑定**的时候接受一个 **onRebind**()的调用（而不是调用 onBind()），你可以选择**在 onUnbind()中返回 true**。

onRebind()的返回值为 void，但是客户端仍然在它的 **onServiceConnected**()回调方法中**得到 IBinder 对象**。

下图展示了这种 service（被开启，还允许绑定）的生命周期：
 ![service（被开启，还允许绑定）的生命周期](http://upload-images.jianshu.io/upload_images/9028834-2664736e0912dcc0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## Service在清单文件中的声明

  前面说过Service分为启动状态和绑定状态两种，但无论哪种具体的Service启动类型，都是通过继承Service基类自定义而来，也都需要在AndroidManifest.xml中声明。
  我们先来了解一下Service在AndroidManifest.xml中的声明语法，其格式如下：

```xml
<service android:enabled=["true" | "false"]
    android:exported=["true" | "false"]
    android:icon="drawable resource"
    android:isolatedProcess=["true" | "false"]
    android:label="string resource"
    android:name="string"
    android:permission="string"
    android:process="string" >
    . . .
</service>
```

- android:**exported**：代表**是否能被其他应用隐式调用**。
  其默认值是由service中有无intent-filter决定的，如果有intent-filter，默认值为true，否则为false。为false的情况下，即使有intent-filter匹配，也无法打开，即无法被其他应用隐式调用。
- android:**name**：对应Service类名
- android:**permission**：是权限声明
- android:**process**：是否需要在单独的进程中运行,当设置为android:process=”:remote”时，代表Service在单独的进程中运行。
  注意：“:”很重要，它的意思是指要在当前进程名称前面附加上当前的包名，所以“remote”和”:remote”不是同一个意思，前者的进程名称为：remote，而后者的进程名称为：App-packageName:remote。
- android:**isolatedProcess** ：设置 true 意味着，服务会在一个特殊的进程下运行，这个进程与系统其他进程分开且没有自己的权限。与其通信的唯一途径是通过服务的API(bind and start)。
- android:**enabled**：是否可以被系统实例化，默认为 true因为父标签也有 enable 属性，所以必须两个都为默认值 true 的情况下服务才会被激活，否则不会激活。 

## 后台服务

后台服务可交互性主要是体现在不同的启动服务方式，startService()和bindService()。
**bindService**()可以**返回一个代理对象**，可调用Service中的方法和获取返回结果等操作；
而startService()不行。

### 不可交互的后台服务（启动服务）

不可交互的后台服务即是普通的Service。
Service的生命周期很简单，分别为onCreate、onStartCommand、onDestroy这三个。
当我们**startService**()的时候，**首次**创建Service会回调**onCreate**()方法，然后回调**onStartCommand**()方法；**再次startService**()的时候，就**只会执行一次onStartCommand**()。
服务一旦开启后，我们就需要**通过stopService**()方法或者**stopSelf**()方法，就能把**服务关闭**，这时就会**回调onDestroy**()

#### ① 服务端：创建服务类

创建一个服务非常简单，只要继承Service，并实现onBind()方法

```java
public class BackGroupService extends Service {
    /* 綁定服务时调用 */
    @Nullable
    @Override
    public IBinder onBind(Intent intent) {
        Log.e("Service", "onBind");
        return null;
    }

    /* 服务创建时调用 */
    @Override
    public void onCreate() {
        Log.e("Service", "onCreate");
        super.onCreate();
    }

    /* 执行startService时调用 */
    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
        Log.e("Service", "onStartCommand");
        //这里执行耗时操作
        new Thread() {
            @Override
            public void run() {
                while (true){
                    try {
                        Log.e("Service", "doSomething");
                        Thread.sleep(2000);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            }
        }.start();
        return super.onStartCommand(intent, flags, startId);
    }

    /* 服务被销毁时调用 */
    @Override
    public void onDestroy() {
        Log.e("Service", "onDestroy");
        super.onDestroy();
    }
}
```

#### ② 配置服务

Service也是四大组件之一，所以必须在manifests中配置

```xml
<service android:name=".Service.BackGroupService"/>
```

#### ③ 客户端：启动服务和停止服务

我们通过两个按钮分别演示启动服务和停止服务，通过**startService**()开启服务，通过**stopService**()停止服务

```java
public class MainActivity extends AppCompatActivity {
    Button bt_open, bt_close;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        bt_open = (Button) findViewById(R.id.open);
        bt_close = (Button) findViewById(R.id.close);
        //自定义Service的意图
        final Intent intent = new Intent(this, BackGroupService.class);
        bt_open.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                //启动服务
                startService(intent);
            }
        });
        bt_close.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                //停止服务
                stopService(intent);
            }
        });
    }
}
```

当你开启服务后，还有一种方法可以关闭服务，在设置中，通过应用->找到自己应用->停止
![](http://upload-images.jianshu.io/upload_images/9028834-b34ef88699ce7a75?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### ④ 运行代码

运行程序后，我们点击开始服务，然后一段时间后关闭服务。我们以Log信息来验证普通Service的生命周期：onCreate->onStartCommand->onDestroy

```
11-24 00:19:51.483 16407-16407/com.handsome.boke2 E/Service: onCreate
11-24 00:19:51.483 16407-16407/com.handsome.boke2 E/Service: onStartCommand
11-24 00:19:51.485 16407-16613/com.handsome.boke2 E/Service: doSomething
11-24 00:19:53.490 16407-16613/com.handsome.boke2 E/Service: doSomething
11-24 00:19:55.491 16407-16613/com.handsome.boke2 E/Service: doSomething
11-24 00:19:57.491 16407-16613/com.handsome.boke2 E/Service: doSomething
11-24 00:19:58.056 16407-16407/com.handsome.boke2 E/Service: onDestroy
11-24 00:19:59.492 16407-16613/com.handsome.boke2 E/Service: doSomething
11-24 00:20:01.494 16407-16613/com.handsome.boke2 E/Service: doSomething
11-24 00:20:03.495 16407-16613/com.handsome.boke2 E/Service: doSomething
```

其中你会发现我们的子线程进行的耗时操作是一直存在的，而我们Service已经被关闭了，关闭该子线程的方法需要直接通过Home键关闭该应用程序

### 可交互的后台服务（绑定服务）

  可交互的后台服务是指**前台页面可以调用后台服务的方法**。
  可交互的后台服务实现步骤是和不可交互的后台服务实现步骤是一样的，**区别在于启动的方式和获得Service的代理对象**。
  与启动服务不同的是绑定服务的生命周期通常只在为其他应用组件(如Activity)服务时处于活动状态，不会无限期在后台运行，也就是说宿主(如Activity)解除绑定后，绑定服务就会被销毁。
  那么在提供绑定的服务时，该如何实现呢？实际上我们**必须提供一个 IBinder接口的实现类**，该类用以**提供客户端用来与服务进行交互的编程接口**，该接口可以通过三种方法定义接口：

- 扩展 **Binder** 类 
  如果服务是**提供给自有应用专用**的，并且**Service(服务端)与客户端相同的进程中运行**（常见情况），则应通过扩展 Binder 类并从 onBind() 返回它的一个实例来创建接口。
    客户端**收到 Binder 后**，可利用它**直接访问 Binder 实现中以及Service 中可用的公共方法**。
    如果我们的服务只是自有应用的后台工作线程，则**优先采用**这种方法。 不采用该方式创建接口的唯一原因是，服务被其他应用或不同的进程调用。
- 使用 **Messenger** 
  Messenger可以翻译为信使，通过它可以在不同的进程中共传递Message对象（Handler中的Messager，因此 Handler 是 Messenger 的基础），在**Message中**可以**存放我们需要传递的数据**，然后**在进程间传递**。
    如果需要让接口跨不同的进程工作，则可使用 Messenger 为服务创建接口，**客户端**就可**利用 Message 对象向服务发送命令**。同时**客户端**也**可定义自有 Messenger**，以便服务**回传消息**。
    这是执行**进程间通信 (IPC) 的最简单方法**，因为 Messenger 会在单一线程中创建包含所有请求的队列，也就是说Messenger是以**串行**的方式**处理客户端发来的消息**，这样我们就不必对服务进行线程安全设计了。
- 使用 **AIDL** 
  由于Messenger是以串行的方式处理客户端发来的消息，如果当前有大量消息同时发送到Service(服务端)，Service仍然**只能一个个处理**，这也就是**Messenger**跨进程通信的**缺点**了，因此如果有大量并发请求，Messenger就会显得力不从心了，这时AIDL（Android 接口定义语言）就派上用场了， 但实际上Messenger 的跨进程方式其底层实现 就是AIDL，只不过android系统帮我们封装成透明的Messenger罢了 。
    因此，如果我们想**让服务同时处理多个请求**，则应该**使用 AIDL**。 在此情况下，**服务必须**具备**多线程处理**能力，并**采用线程安全式设计**。
    使用AIDL必须创建一个定义编程接口的 .aidl 文件。Android SDK 工具利用该文件生成一个实现接口并处理 IPC 的抽象类，随后可在服务内对其进行扩展。

以上3种实现方式，我们可以根据需求自由的选择，但需要注意的是大多数应用“都不会”使用 AIDL 来创建绑定服务，因为它可能要求具备多线程处理能力，并可能导致实现的复杂性增加。因此，AIDL 并不适合大多数应用，本篇中也不打算阐述如何使用AIDL（后面会另开一篇分析AIDL），接下来我们分别针对扩展 Binder 类和Messenger的使用进行分析。

#### 1. 扩展 Binder 类

  前面描述过，如果我们的服务仅供本地应用使用，不需要跨进程工作，则可以实现自有 Binder 类，让客户端通过该类直接访问服务中的公共方法。其使用开发**步骤**如下

1. 创建BindService服务端，继承自Service并在类中，创建一个实现IBinder 接口的实例对象并提供公共方法给客户端调用
2. 从 onBind() 回调方法返回此 Binder 实例。
3. **在客户端中，从 onServiceConnected() 回调方法接收 Binder，并使用提供的方法调用绑定服务**。

注意：此方式只有在客户端和服务位于同一应用和进程内才有效，如对于需要将 Activity 绑定到在后台播放音乐的自有服务的音乐应用，此方式非常有效。另一点之所以要求服务和客户端必须在同一应用内，是为了便于客户端转换返回的对象和正确调用其 API。服务和客户端还必须在同一进程内，因为此方式不执行任何跨进程编组。

##### ① 服务端：创建服务类

  LocalService类继承自Service，在该类中创建了一个LocalBinder继承自Binder类，**LocalBinder中声明了一个getService方法，客户端可访问该方法获取LocalService对象的实例**，只要客户端获取到LocalService对象的实例就可调用LocalService服务端的公共方法，如getCount方法。
  值得注意的是，我们**在onBind方法中返回了binder对象，该对象便是LocalBinder的具体实例**，而binder对象最终会返回给客户端，客户端通过返回的binder对象便可以与服务端实现交互。

```java
package com.zejian.ipctest.service;
import android.app.Service;
import android.content.Intent;
import android.os.Binder;
import android.os.IBinder;
import android.support.annotation.Nullable;
import android.util.Log;
/* 绑定服务简单实例--服务端 */
public class LocalService extends Service{
    private final static String TAG = "wzj";
    private int count;
    private boolean quit;
    private Thread thread;
    private LocalBinder binder = new LocalBinder();

    /* 创建Binder对象，返回给客户端即Activity使用，提供数据交换的接口 */
    public class LocalBinder extends Binder {
        // 声明一个方法，getService。（提供给客户端调用）
        LocalService getService() {
            // 返回当前对象LocalService,这样我们就可在客户端端调用Service的公共方法了
            return LocalService.this;
        }
    }

    /* 把Binder类返回给客户端 */
    @Nullable
    @Override
    public IBinder onBind(Intent intent) {
        return binder;
    }

    @Override
    public void onCreate() {
        super.onCreate();
        Log.i(TAG, "Service is invoke Created");
        thread = new Thread(new Runnable() {
            @Override
            public void run() {
                // 每间隔一秒count加1 ，直到quit为true。
                while (!quit) {
                    try {
                        Thread.sleep(1000);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                    count++;
                }
            }
        });
        thread.start();
    }

    /* 公共方法 */
    public int getCount(){
        return count;
    }
    /* 解除绑定时调用 */
     @Override
    public boolean onUnbind(Intent intent) {
        Log.i(TAG, "Service is invoke onUnbind");
        return super.onUnbind(intent);
    }

    @Override
    public void onDestroy() {
        Log.i(TAG, "Service is invoke Destroyed");
        this.quit = true;
        super.onDestroy();
    }
}
```

##### ② 配置服务

...

##### ③ 客户端：绑定服务和解除绑定服务

接着看看客户端BindActivity的实现：

```java
package com.zejian.ipctest.service;

import android.app.Activity;
import android.app.Service;
import android.content.ComponentName;
import android.content.Intent;
import android.content.ServiceConnection;
import android.os.Bundle;
import android.os.IBinder;
import android.util.Log;
import android.view.View;
import android.widget.Button;

import com.zejian.ipctest.R;

/* 绑定服务实例--客户端 */
public class BindActivity extends Activity {
    protected static final String TAG = "wzj";
    Button btnBind;
    Button btnUnBind;
    Button btnGetDatas;
    private ServiceConnection conn;
    private LocalService mService;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_bind);
        btnBind = (Button) findViewById(R.id.BindService);
        btnUnBind = (Button) findViewById(R.id.unBindService);
        btnGetDatas = (Button) findViewById(R.id.getServiceDatas);
        //创建绑定对象
        final Intent intent = new Intent(this, LocalService.class);
        // 开启绑定
        btnBind.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Log.d(TAG, "绑定调用：bindService");
                //调用绑定方法
                bindService(intent, conn, Service.BIND_AUTO_CREATE);
            }
        });
        // 解除绑定
        btnUnBind.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Log.d(TAG, "解除绑定调用：unbindService");
                // 解除绑定
                if(mService!=null) {
                    mService = null;
                    unbindService(conn);
                }
            }
        });

        // 获取数据
        btnGetDatas.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                if (mService != null) {
                    // 通过绑定服务传递的Binder对象，获取Service暴露出来的数据
                    Log.d(TAG, "从服务端获取数据：" + mService.getCount());
                } else {
                    Log.d(TAG, "还没绑定呢，先绑定,无法从服务端获取数据");
                }
            }
        });

    /* ServiceConnection代表与服务的连接，它只有两个方法：onServiceConnected和onServiceDisconnected，前者是在操作者在连接一个服务成功时被调用，而后者是在服务崩溃或被杀死导致的连接中断时被调用 */
        conn = new ServiceConnection() {
            /* onServiceConnected 绑定服务的时候被回调，在这个方法获取绑定Service传递过来的IBinder对象，通过这个IBinder对象，实现宿主和Service的交互。 */
            @Override
            public void onServiceConnected(ComponentName name, IBinder service) {
                Log.d(TAG, "绑定成功调用：onServiceConnected");
                // 获取Binder
                LocalService.LocalBinder binder = (LocalService.LocalBinder) service;
                mService = binder.getService();
            }
            /* onServiceDisconnected 当取消绑定的时候被回调。但正常情况下是不被调用的，它的调用时机是当Service服务被意外销毁时，例如内存的资源不足时这个方法才被自动调用。 */
            @Override
            public void onServiceDisconnected(ComponentName name) {
                mService=null;
            }
        };
    }
}
```

##### ServiceConnection

  在客户端中我们创建了一个ServiceConnection对象，该代表与服务的连接，它只有两个方法， onServiceConnected和onServiceDisconnected，其含义如下：

- **onServiceConnected(ComponentName name, IBinder service)** 
  系统会调用该方法以**传递**服务的onBind() 方法返回的 **IBinder**。
  其中service便是服务端返回的IBinder实现类对象，通过该对象我们便可以调用获取LocalService实例对象，进而调用服务端的公共方法。而ComponentName是一个封装了组件(Activity, Service, BroadcastReceiver, or ContentProvider)信息的类，如包名，组件描述等信息，较少使用该参数。
- **onServiceDisconnected(ComponentName name)** 
  Android 系统会在与服务的连接意外中断时（例如当服务崩溃或被终止时）调用该方法。注意:当客户端取消绑定时，系统“绝对不会”调用该方法。

  在onServiceConnected()被回调前，我们还需先把当前Activity绑定到服务LocalService上，绑定服务是通过通过bindService()方法，解绑服务则使用unbindService()方法，这两个方法解析如下：

- **bindService(Intent service, ServiceConnection conn, int flags)** 
  该方法执行绑定服务操作，其中Intent是我们要绑定的服务(也就是LocalService)的意图，而ServiceConnection代表与服务的连接，它只有两个方法，前面已分析过，flags则是指定绑定时是否自动创建Service。0代表不自动创建、BIND_AUTO_CREATE则代表自动创建。
- **unbindService(ServiceConnection conn)** 
  该方法执行解除绑定的操作



Activity通过bindService()绑定到LocalService后，ServiceConnection#onServiceConnected()便会被回调并可以获取到LocalService实例对象mService，之后我们就可以调用LocalService服务端的公共方法了。

而客户端布局文件实现如下：

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical" android:layout_width="match_parent"
    android:layout_height="match_parent">

    <Button
        android:id="@+id/BindService"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="绑定服务器"
        />

    <Button
        android:id="@+id/unBindService"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="解除绑定"
        />

    <Button
        android:id="@+id/getServiceDatas"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="获取服务方数据"
        />
</LinearLayout>
```

##### ④ 运行代码

  我们运行程序，点击绑定服务并多次点击绑定服务接着多次调用LocalService中的getCount()获取数据，最后调用解除绑定的方法移除服务，其结果如下： 
![这里写图片描述](http://upload-images.jianshu.io/upload_images/9028834-23c2cc055e6e846c?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  通过Log可知，当我们第一次点击绑定服务时，LocalService服务端的onCreate()、onBind方法会依次被调用，此时客户端的ServiceConnection#onServiceConnected()被调用并返回LocalBinder对象，接着调用LocalBinder#getService方法返回LocalService实例对象，此时客户端便持有了LocalService的实例对象，也就可以任意调用LocalService类中的声明公共方法了。
  更值得注意的是，我们**多次调用bindService**方法绑定LocalService服务端，而LocalService得**onBind方法只调用了一次**，那就是在第一次调用bindService时才会回调onBind方法。
  接着我们点击获取服务端的数据，从Log中看出我们点击了3次通过getCount()获取了服务端的3个不同数据，最后点击解除绑定，此时LocalService的onUnBind、onDestroy方法依次被回调，并且多次绑定只需一次解绑即可。此情景也就说明了绑定状态下的Service生命周期方法的调用依次为onCreate()、onBind、onUnBind、onDestroy。
  以上便是同一应用同一进程中客户端与服务端的绑定回调方式。



#### 2. 使用Messenger

  前面了解了如何使用IBinder应用内同一进程的通信后，我们接着来了解服务与远程进程（即不同进程间）通信，而不同进程间的通信，最简单的方式就是使用 Messenger 服务提供通信接口，利用此方式，我们无需使用 AIDL 便可执行进程间通信 (IPC)。以下是 Messenger 使用的主要**步骤**：

1. 服务实现一个 Handler，由其接收来自客户端的每个调用的回调
2. Handler 用于创建 Messenger 对象（对 Handler 的引用）
3. Messenger 创建一个 IBinder，服务通过 onBind() 使其返回客户端
4. 客户端使用 IBinder 将 Messenger（引用服务的 Handler）实例化，然后使用Messenger将 Message 对象发送给服务
5. 服务在其 Handler 中（在 handleMessage() 方法中）接收每个 Message

##### ① 服务端

以下是一个使用 Messenger 接口的简单服务示例，服务端进程实现如下：
  首先我们同样需要**创建**一个**服务类**MessengerService继承自Service。
  同时**创建**一个继承自**Handler**的IncomingHandler对象来**接收客户端进程发送过来的消息并通过其handleMessage(Message msg)进行消息处理**。
  接着通过Incoming**Handler**对象**创建**一个**Messenger**对象，该对象是**与客户端交互**的特殊对象。
  然后在Service的**onBind**中**返回**这个**Messenger对象的底层Binder**即可。

```java
package com.zejian.ipctest.messenger;

import android.app.Service;
import android.content.Intent;
import android.os.Handler;
import android.os.IBinder;
import android.os.Message;
import android.os.Messenger;
import android.util.Log;

/* 服务端简单实例,服务端进程 */
public class MessengerService extends Service {
    /** Command to the service to display a message */
    static final int MSG_SAY_HELLO = 1;
    private static final String TAG ="wzj" ;

    /* 用于接收从客户端传递过来的数据 */
    class IncomingHandler extends Handler {
        @Override
        public void handleMessage(Message msg) {
            switch (msg.what) {
                case MSG_SAY_HELLO:
                    Log.i(TAG, "thanks,Service had receiver message from client!");
                    break;
                default:
                    super.handleMessage(msg);
            }
        }
    }

    /* 创建Messenger并传入Handler实例对象 */
    final Messenger mMessenger = new Messenger(new IncomingHandler());

    /* 当绑定Service时,该方法被调用,将通过mMessenger返回一个实现IBinder接口的实例对象 */
    @Override
    public IBinder onBind(Intent intent) {
        Log.i(TAG, "Service is invoke onBind");
        return mMessenger.getBinder();
    }
}
```

##### ② 配置服务

  在清单文件声明Service和Activity，由于要测试不同进程的交互，则需要将Service放在单独的进程中，因此Service声明如下：

```xml
<service android:name=".messenger.MessengerService"
         android:process=":remote"
        />
```

其中`android:process=":remote"`代表该Service在单独的进程中创建。

##### ③ 客户端

下面看看客户端进程的实现：

```java
package com.zejian.ipctest.messenger;

import android.app.Activity;
import android.content.ComponentName;
import android.content.Context;
import android.content.Intent;
import android.content.ServiceConnection;
import android.os.Bundle;
import android.os.IBinder;
import android.os.Message;
import android.os.Messenger;
import android.os.RemoteException;
import android.util.Log;
import android.view.View;
import android.widget.Button;
import com.zejian.ipctest.R;

/* 与服务器交互的客户端 */
public class ActivityMessenger extends Activity {
    /* 与服务端交互的Messenger */
    Messenger mService = null;
    /** Flag indicating whether we have called bind on the service. */
    boolean mBound;

    /* 实现与服务端链接的对象ServiceConnection */
    private ServiceConnection mConnection = new ServiceConnection() {
        public void onServiceConnected(ComponentName className, IBinder service) {
            /* 通过服务端传递的IBinder对象,创建相应的Messenger
             * 通过该Messenger对象与服务端进行交互 */
            mService = new Messenger(service);
            mBound = true;
        }

        public void onServiceDisconnected(ComponentName className) {
            // This is called when the connection with the service has been
            // unexpectedly disconnected -- that is, its process crashed.
            mService = null;
            mBound = false;
        }
    };

    public void sayHello(View v) {
        if (!mBound) return;
        // 创建与服务交互的消息实体Message
        Message msg = Message.obtain(null, MessengerService.MSG_SAY_HELLO, 0, 0);
        try {
            //发送消息
            mService.send(msg);
        } catch (RemoteException e) {
            e.printStackTrace();
        }
    }

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_messenager);
        Button bindService= (Button) findViewById(R.id.bindService);
        Button unbindService= (Button) findViewById(R.id.unbindService);
        Button sendMsg= (Button) findViewById(R.id.sendMsgToService);

        bindService.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                Log.d("zj","onClick-->bindService");
                //当前Activity绑定服务端
                bindService(new Intent(ActivityMessenger.this, MessengerService.class), mConnection,
                        Context.BIND_AUTO_CREATE);
            }
        });

        //发送消息给服务端
        sendMsg.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                sayHello(v);
            }
        });

        unbindService.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                // Unbind from the service
                if (mBound) {
                    Log.d("zj","onClick-->unbindService");
                    unbindService(mConnection);
                    mBound = false;
                }
            }
        });
    }
}
```

  在客户端进程中，我们需要创建一个ServiceConnection对象，该对象代表与服务端的链接，当调用bindService方法将当前Activity绑定到MessengerService时，onServiceConnected方法被调用，利用服务端传递给来的底层Binder对象构造出与服务端交互的Messenger对象，接着创建与服务交互的消息实体Message，将要发生的信息封装在Message中并通过Messenger实例对象发送给服务端。

##### ④ 运行代码

最后我们运行程序，结果如下： 
![](http://upload-images.jianshu.io/upload_images/9028834-e5dad2ae5f0cb362?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

接着多次点击绑定服务，然后发送信息给服务端，最后解除绑定，Log打印如下： 
![这里写图片描述](http://upload-images.jianshu.io/upload_images/9028834-02d3c68c10191889?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  通过上述例子可知Service服务端确实收到了客户端发送的信息，而且在Messenger中进行数据传递必须将数据封装到Message中，因为Message和Messenger都实现了Parcelable接口，可以轻松跨进程传递数据（关于Parcelable接口可以看博主的另一篇文章：[序列化与反序列化之Parcelable和Serializable浅析](http://blog.csdn.net/javazejian/article/details/52665164)）。
  **Message可以传递的信息载体**有，what,arg1,arg2,Bundle以及replyTo。至于object字段，对于同一进程中的数据传递确实很实用，但对于进程间的通信，则显得相当尴尬，在android2.2前，object不支持跨进程传输，但即便是android2.2之后也只能传递android系统提供的实现了Parcelable接口的对象，也就是说我们通过自定义实现Parcelable接口的对象无法通过object字段来传递，因此object字段的实用性在跨进程中也变得相当低了。不过所幸我们还有Bundle对象，Bundle可以支持大量的数据类型。
  接着从Log我们也看出无论是使用拓展Binder类的实现方式还是使用Messenger的实现方式，它们的生命周期方法的调用顺序基本是一样的，即onCreate()、onBind、onUnBind、onDestroy，而且多次绑定中也只有第一次时才调用onBind()。
  **以上**的例子演示了如何**在服务端接收客户端发送的消息**，但有时候我们可能还需要服务端能回应客户端，这时便需要提供双向消息传递了，**下面**就来**实现一个简单服务端与客户端双向消息传递的简单例子**。 
  先来看看服务端的修改，在服务端，我们只需修改IncomingHandler，收到消息后，给客户端回复一条信息。

```java
  /* 用于接收从客户端传递过来的数据 */
    class IncomingHandler extends Handler {
        @Override
        public void handleMessage(Message msg) {
            switch (msg.what) {
                case MSG_SAY_HELLO:
                    Log.i(TAG, "thanks,Service had receiver message from client!");
                    //回复客户端信息,该对象由客户端传递过来
                    Messenger client=msg.replyTo;
                    //获取回复信息的消息实体
                    Message replyMsg=Message.obtain(null,MessengerService.MSG_SAY_HELLO);
                    Bundle bundle=new Bundle();
                    bundle.putString("reply","ok~,I had receiver message from you! ");
                    replyMsg.setData(bundle);
                    //向客户端发送消息
                    try {
                        client.send(replyMsg);
                    } catch (RemoteException e) {
                        e.printStackTrace();
                    }
                    break;
                default:
                    super.handleMessage(msg);
            }
        }
    }
```

  接着修改客户端，为了接收服务端的回复，客户端也需要一个接收消息的Messenger和Handler，其实现如下：

```java
  /* 用于接收服务器返回的信息 */
    private Messenger mRecevierReplyMsg= new Messenger(new ReceiverReplyMsgHandler());

    private static class ReceiverReplyMsgHandler extends Handler{
        private static final String TAG = "zj";

        @Override
        public void handleMessage(Message msg) {
            switch (msg.what) {
                //接收服务端回复
                case MessengerService.MSG_SAY_HELLO:
                    Log.i(TAG, "receiver message from service:"+msg.getData().getString("reply"));
                    break;
                default:
                    super.handleMessage(msg);
            }
        }
    }
```

  除了添加以上代码，还需要在发送信息时把接收服务器端的回复的Messenger通过Message的replyTo参数传递给服务端，以便作为同学桥梁，代码如下：

```
 public void sayHello(View v) {
        if (!mBound) return;
        // 创建与服务交互的消息实体Message
        Message msg = Message.obtain(null, MessengerService.MSG_SAY_HELLO, 0, 0);
        //把接收服务器端的回复的Messenger通过Message的replyTo参数传递给服务端
        msg.replyTo=mRecevierReplyMsg;
        try {
            //发送消息
            mService.send(msg);
        } catch (RemoteException e) {
            e.printStackTrace();
        }
    }
```

  到此服务端与客户端双向消息传递的简单例子修改完成，我们运行一下代码，看看Log打印，如下： 
![这里写图片描述](http://upload-images.jianshu.io/upload_images/9028834-c50bd0313827be30?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

  通过Messenge方式进行进程间通信的原理图： 
![这里写图片描述](http://upload-images.jianshu.io/upload_images/9028834-fd208b16af9fe801?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 3. AIDL

关于AIDL跨进程服务的使用和原理分析，可以见我另一篇博客：[Android基础——初学者必知的AIDL在应用层上的Binder机制](http://blog.csdn.net/qq_30379689/article/details/52253413)

##### 实例-远程服务调用商城支付

在 Android 平台中，各个组件运行在自己的进程中，他们之间是不能相互访问的，但是在程序之间是不可避免的要传递一些对象，在进程之间相互通信。为了实现进程之间的相互 通信，Android 采用了一种轻量级的实现方式**RPC(Remote Procedure Call 远程进程调用) 来完成进程之间的通信**，并且 Android **通过接口定义语言（Android Interface Definition Language ,AIDL）来生成两个进程之间相互访问的代码**，例如，你在 Activity 里的代码需要访问 Service 中的一个方法，那么就可以通过这种方式来实现了。

**AIDL** 是 Android 的一种接口描述语言; 编译器可以通过 aidl 文件生成一段代码，通过预先定义的接口达到两个进程内部通信进程的目的。如果需要在一个 Activity 中, 访问另一个 Service 中的某个对象, 需要先将对象转化成 AIDL 可识别的参数(可能是多个参数), 然后 使用 AIDL 来传递这些参数, 在消息的接收端, 使用这些参数组装成自己需要的对象。
AIDL RPC 机制是通过接口来实现的，类似 Windows 中的 COM 或者 Corba，但他是轻量级的，客户端和被调用实现之间是通过代理模式实现的，代理类和被代理类实现同一个接 口 IBinder 接口。

下面是**案例**-商城支付的步骤： 
需求：分别创建两个工程，模拟一个支付平台，暂且叫支付宝，模拟一个商户端，叫商户。 商户可以调用支付宝发布的远程服务进行收款操作。

- **①** 新创建一个 Android 工程《支付宝》，包名：com.itheima.alipay。在 src 目录 下创建 com.itheima.alipay.aidl 包，然后在该包下**创建 AlipayRemoteService.aidl 文件**。
  在该文件中只声明一个接口，在接口里声明一个方法。文件清单如下：

```java
package com.itheima.alipay.aidl;
interface AlipayRemoteService{
    boolean forwardPayMoney(float money);
}
```

当该 aidl 文件创建好以后 ADT 会自动在 gen 目录下创建对应的类
![](http://upload-images.jianshu.io/upload_images/9028834-21e5ad73e9aca20e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- **②** 在《支付宝》src 目录下创建  com.itheima.alipay.service 包，在该包中**新建一个Service**，叫  AlipayService，该类实现付款功能。代码清单如下：

```java
public class AlipayService extends Service {
    @Override
    public IBinder onBind(Intent intent) {
        return new PayController();
    }
    public boolean pay(float money) {
        System.out.println("成功付款" + money);
        return true;
    }
    /* 因为 Stub 已经继承了 IBinder 接口，因此 PayController 类也间接继承了该接口 */
    public class PayController extends Stub {
        @Override
        public boolean forwardPayMoney(float money) throws RemoteException {
            return pay(money);
        }
    }
}
```

- **③** 在《支付宝》工程的 AndroidManifest.xml 中**注册该 AlipayService**

```xml
<service android:name="com.itheima.alipay.service.AlipayService">
    <intent-filter>
        <action android:name="com.itheima.alipay"></action>
    </intent-filter>
</service>
```

- **④** 创建一个**新 Android 工程**，名字叫《商户》，包名：com.itheima.shop。
  将《支付宝》工程中的 **AlipayRemoteService.aidl 文件拷贝到**《商户》工程的 **src 目 录下**，同时注意添加对应的包名，要求**包名必须跟该文件在原工程中的包名严格一致**。
  《商户》src 目录结构如下图：
  ![](http://upload-images.jianshu.io/upload_images/9028834-854c33ad4a947cce.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- **⑤** 编辑 activity_main.xm **布局文件**

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity">
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center_horizontal"
        android:text="商城支付-调用远程服务"
        android:textColor="#ff0000"
        android:textSize="28sp" />
    <EditText
        android:id="@+id/et"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="请输入要转正的金额"
        android:inputType="number" />
    <Button
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="right"
        android:onClick="pay"
        android:text="确定支付" />
</LinearLayout>
```

- **⑥** 编写 **MainActivity** 类，在该类中**实现核心方法**

```java
public class MainActivity extends Activity {
    //声明一个 AlipayRemoteService 对象，该类是根据 aidl 文件自动生成
    private AlipayRemoteService alipayRemoteService;
    private EditText et;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        et = (EditText) findViewById(R.id.et);
        //创建一个隐式意图，用于启动《支付宝》中的 Service
        Intent intent = new Intent();
        intent.setAction("com.itheima.alipay");
        //绑定远程服务
        boolean bindService = bindService(intent, new MyConnection(), Context.BIND_AUTO_CREATE);
        if (bindService) {
            Toast.makeText(this, "服务绑定成功", 1).show();
            System.out.println("服务绑定成功");
        } else {
            Toast.makeText(this, "服务绑定失败", 1).show();
            System.out.println("服务绑定失败");
        }
    }

    public void pay(View view) {
        float money = Float.valueOf(et.getText().toString());
        try {
            alipayRemoteService.forwardPayMoney(money);
        } catch (RemoteException e) {
            e.printStackTrace();
            Toast.makeText(this, "付款失败", 1).show();
        }
        Toast.makeText(this, "成功转账：" + money + "元！", 0).show();
    }

    class MyConnection implements ServiceConnection {
        /* 通过 Stub 的静态方法 asInterface 将 IBinder 对象转化为本地AlipayRemoteService 对象 */
        @Override
        public void onServiceConnected(ComponentName name, IBinder service) {
            System.out.println("服务已经连接。。。");
            alipayRemoteService = Stub.asInterface(service);
        }

        @Override
        public void onServiceDisconnected(ComponentName name) {
            System.out.println("服务已经关闭。");
        }
    }
}
```

- ⑦ 先将《支付宝》部署到模拟器，然后将《商户》部署到模拟器，然后在《商户》界面输入一个金额，然后点击确定支付，发现《商户》工程已经成功通过远程服务调用了《支付宝》中的服务。运行图如下：
  ![](http://upload-images.jianshu.io/upload_images/9028834-918a6ae86d441321.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 关于绑定服务的注意点

1. 多个客户端可同时连接到一个服务。不过，只有在第一个客户端绑定时，系统才会调用服务的 onBind() 方法来检索 IBinder。系统随后无需再次调用 onBind()，便可将同一 IBinder 传递至任何其他绑定的客户端。当最后一个客户端取消与服务的绑定时，系统会将服务销毁（除非 startService() 也启动了该服务）。
2. 通常情况下我们应该在客户端生命周期（如Activity的生命周期）的引入 (bring-up) 和退出 (tear-down) 时刻设置绑定和取消绑定操作，以便控制绑定状态下的Service，一般有以下两种情况：

- 如果只需要在 Activity 可见时与服务交互，则应在 onStart() 期间绑定，在 onStop() 期间取消绑定。
- 如果希望 Activity 在后台停止运行状态下仍可接收响应，则可在 onCreate() 期间绑定，在 onDestroy() 期间取消绑定。需要注意的是，这意味着 Activity 在其整个运行过程中（甚至包括后台运行期间）都需要使用服务，因此如果服务位于其他进程内，那么当提高该进程的权重时，系统很可能会终止该进程。

1. 通常情况下(注意)，**切勿在 Activity 的 onResume() 和 onPause() 期间绑定和取消绑定**，因为每一次生命周期转换都会发生这些回调，这样反复绑定与解绑是不合理的。此外，如果应用内的多个 Activity 绑定到同一服务，并且其中两个 Activity 之间发生了转换，则如果当前 Activity 在下一次绑定（恢复期间）之前取消绑定（暂停期间），系统可能会销毁服务并重建服务，因此服务的绑定不应该发生在 Activity 的 onResume() 和 onPause()中。
2. 我们应该始终捕获 DeadObjectException 异常，该异常是在连接中断时引发的，表示调用的对象已死亡，也就是Service对象已销毁，这是远程方法引发的唯一异常，DeadObjectException继承自RemoteException，因此我们也可以捕获RemoteException异常。
3. 应用组件（客户端）可通过调用 bindService() 绑定到服务，Android 系统随后调用服务的 onBind() 方法，该方法返回用于与服务交互的 IBinder，而该绑定是异步执行的。

### 混合性交互的后台服务

  虽然服务的状态有启动和绑定两种，但实际上一个服务可以同时是这两种状态，也就是说，它既可以是启动服务（以无限期运行），也可以是绑定服务。
  有点需要注意的是Android系统仅会为一个Service创建一个实例对象，所以**不管是启动服务还是绑定服务，操作的是同一个Service实例**。
  既要宿主解除绑定，又要条用停止服务，该服务才会销毁。

## 前台服务

由于**后台服务优先级相对比较低**，当系统出现内存不足的情况下，它就有可能会被回收掉，所以**前台服务**就是来弥补这个缺点的，它**可以一直保持运行状态而不被系统回收**。

  前台服务被认为是用户主动意识到的一种服务，因此在内存不足时，系统也不会考虑将其终止。 
  前台服务必须为状态栏提供通知，状态栏位于“正在进行”标题下方，这意味着除非服务停止或从前台删除，否则不能清除通知。例如将从服务播放音乐的音乐播放器设置为在前台运行，这是因为用户明确意识到其操作。 状态栏中的通知可能表示正在播放的歌曲，并允许用户启动 Activity 来与音乐播放器进行交互。
  如果需要设置服务运行于前台， 我们该如何才能实现呢？Android官方给我们提供了两个方法，分别是startForeground()和stopForeground()，这两个方式解析如下：

- **startForeground(int id, Notification notification)** 
  该方法的作用是把当前服务设置为前台服务，其中id参数代表唯一标识通知的整型数，需要注意的是提供给 startForeground() 的整型 ID 不得为 0，而notification是一个状态栏的通知。
- **stopForeground(boolean removeNotification)** 
  该方法是用来从前台删除服务，此方法传入一个布尔值，指示是否也删除状态栏通知，true为删除。 注意该方法并不会停止服务。 但是，如果在服务正在前台运行时将其停止，则通知也会被删除。

下面我们结合一个简单案例来使用以上两个方法

#### ① 服务端：创建服务类

ForegroundService代码如下：
在ForegroundService类中，创建了一个notification的通知，并通过启动Service时传递过来的参数判断是启动前台服务还是关闭前台服务，最后在onDestroy方法被调用时，也应该移除前台服务。

```java
package com.zejian.ipctest.foregroundService;

import android.app.Notification;
import android.app.Service;
import android.content.Intent;
import android.graphics.BitmapFactory;
import android.os.IBinder;
import android.support.annotation.Nullable;
import android.support.v4.app.NotificationCompat;

import com.zejian.ipctest.R;

/* 启动前台服务Demo */
public class ForegroundService extends Service {
    //id不可设置为0,否则不能设置为前台service
    private static final int NOTIFICATION_DOWNLOAD_PROGRESS_ID = 0x0001;
    private boolean isRemove=false;//是否需要移除
    //Notification
    public void createNotification(){
        //使用兼容版本
        NotificationCompat.Builder builder=new NotificationCompat.Builder(this);
        //设置状态栏的通知图标
        builder.setSmallIcon(R.mipmap.ic_launcher);
        //设置通知栏横条的图标
        builder.setLargeIcon(BitmapFactory.decodeResource(getResources(),R.drawable.screenflash_logo));
        //禁止用户点击删除按钮删除
        builder.setAutoCancel(false);
        //禁止滑动删除
        builder.setOngoing(true);
        //右上角的时间显示
        builder.setShowWhen(true);
        //设置通知栏的标题内容
        builder.setContentTitle("I am Foreground Service!!!");
        //创建通知
        Notification notification = builder.build();
        //设置为前台服务
        startForeground(NOTIFICATION_DOWNLOAD_PROGRESS_ID,notification);
    }

    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
        int i=intent.getExtras().getInt("cmd");
        if(i==0){
            if(!isRemove) {
                createNotification();
            }
            isRemove=true;
        }else {
            //移除前台服务
            if (isRemove) {
                stopForeground(true);
            }
            isRemove=false;
        }

        return super.onStartCommand(intent, flags, startId);
    }

    @Override
    public void onDestroy() {
        //移除前台服务
        if (isRemove) {
            stopForeground(true);
        }
        isRemove=false;
        super.onDestroy();
    }

    @Nullable
    @Override
    public IBinder onBind(Intent intent) {
        return null;
    }
}
```

#### ② 配置服务

...

#### ③ 客户端：

以下是ForegroundActivity的实现：

```java
package com.zejian.ipctest.foregroundService;

import android.app.Activity;
import android.content.Intent;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;
import com.zejian.ipctest.R;

public class ForegroundActivity extends Activity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_foreground);
        Button btnStart= (Button) findViewById(R.id.startForeground);
        Button btnStop= (Button) findViewById(R.id.stopForeground);
        final Intent intent = new Intent(this,ForegroundService.class);

        btnStart.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                intent.putExtra("cmd",0);//0,开启前台服务,1,关闭前台服务
                startService(intent);
            }
        });
        btnStop.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                intent.putExtra("cmd",1);//0,开启前台服务,1,关闭前台服务
                startService(intent);
            }
        });
    }
}
```

  代码比较简单，我们直接运行程序看看结果：

[![](http://upload-images.jianshu.io/upload_images/9028834-89e6c7a3df47152e?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-89e6c7a3df47152e?imageMogr2/auto-orient/strip)

当我们将该程序退出并杀掉的时候，通过设置->应用->选择正在运行中的应用，我们可以发现，我们的程序退出杀掉了，而服务还在进行着





## IntentService

IntentService是专门用来解决Service中不能执行耗时操作这一问题的，创建一个IntentService也很简单，只要**继承IntentService并覆写onHandlerIntent函数，在该函数中就可以执行耗时操作**了

```java
public class TheIntentService extends IntentService {
    public TheIntentService(String name) {
        super(name);
    }
    @Override
    protected void onHandleIntent(Intent intent) {
        //在这里执行耗时操作
    }
}
```

## AccessibilityService无障碍服务

关于AccessibilityService无障碍服务的使用和实例，可以见我另一篇博客：[Android进阶——学习AccessibilityService实现微信抢红包插件](http://blog.csdn.net/qq_30379689/article/details/53242953)

## 系统服务

系统服务提供了很多便捷服务，可以查询Wifi、网络状态、查询电量、查询音量、查询包名、查询Application信息等等等相关多的服务，具体大家可以自信查询文档，这里举例几个常见的服务

#### 判断Wifi是否开启

```java
WifiManager wm = (WifiManager) getSystemService(WIFI_SERVICE);
boolean enabled = wm.isWifiEnabled();
```

需要权限

```xml
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE"/>
<uses-permission android:name="android.permission.CHANGE_WIFI_STATE"/>
```

#### 获取系统最大音量

```java
AudioManager am = (AudioManager) getSystemService(AUDIO_SERVICE);
int max = am.getStreamMaxVolume(AudioManager.STREAM_SYSTEM);
```

#### 获取当前音量

```java
AudioManager am = (AudioManager) getSystemService(AUDIO_SERVICE);
int current = am.getStreamMaxVolume(AudioManager.STREAM_RING);
```

#### 判断网络是否有连接

```java
ConnectivityManager cm = (ConnectivityManager) getSystemService(CONNECTIVITY_SERVICE);
NetworkInfo info = cm.getActiveNetworkInfo();
boolean isAvailable = info.isAvailable();
```

需要权限

```xml
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE"/>
```

[部分源码下载](http://download.csdn.net/detail/qq_30379689/9692631)

- ​

## 如何保证服务不被杀死

#### onStartCommand方法，返回START_STICKY或START_REDELIVER_INTENT

该值表示服务在内存资源紧张时被杀死后，在内存资源足够时再恢复。
【结论】 手动返回START_STICKY，亲测当service因内存不足被kill，当内存又有的时候，service又被重新创建，比较不错，但是不能保证任何情况下都被重建，比如进程被干掉了....

```java
@Override  
public int onStartCommand(Intent intent, int flags, int startId) {  
    flags = START_STICKY;  
    return super.onStartCommand(intent, flags, startId);  
}  
```

#### 将服务设为前台服务startForeground

可以使用startForeground 将service放到前台状态。这样在低内存时被kill的几率会低一些。
【结论】如果在极度极度低内存的压力下，该service还是会被kill掉，并且不一定会restart。

在onStartCommand方法内添加如下代码：

```java
Notification notification = new Notification(R.drawable.ic_launcher, 
				getString(R.string.app_name), System.currentTimeMillis());  
PendingIntent pendingintent = PendingIntent.getActivity(this, 0, 
				new Intent(this, AppMain.class), 0);  
notification.setLatestEventInfo(this, "uploadservice", "请保持程序在后台运行", pendingintent);  
startForeground(0x0001, notification);
```

注意在onDestroy里还需要stopForeground(true)，运行时在下拉列表会看到自己的APP在：
![](http://upload-images.jianshu.io/upload_images/9028834-847eef96eabbb5a2?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 开启两个服务，相互监听，相互启动

服务A监听B的广播来启动B，服务B监听A的广播来启动A。这里给出第一种方式的代码实现如下：

```java
package com.zejian.ipctest.neverKilledService;

import android.app.Service;
import android.content.BroadcastReceiver;
import android.content.Context;
import android.content.Intent;
import android.content.IntentFilter;
import android.os.IBinder;
import android.support.annotation.Nullable;

/* 用户通过 settings -> Apps -> Running -> Stop 方式杀死Service */
public class ServiceKilledByAppStop extends Service{
    private BroadcastReceiver mReceiver;
    private IntentFilter mIF;
    @Nullable
    @Override
    public IBinder onBind(Intent intent) {
        return null;
    }
    @Override
    public void onCreate() {
        super.onCreate();
        mReceiver = new BroadcastReceiver() {
            @Override
            public void onReceive(Context context, Intent intent) {
                Intent a = new Intent(ServiceKilledByAppStop.this, ServiceKilledByAppStop.class);
                startService(a);
            }
        };
        mIF = new IntentFilter();
        //自定义action
        mIF.addAction("com.restart.service");
        //注册广播接者
        registerReceiver(mReceiver, mIF);
    }

    @Override
    public void onDestroy() {
        super.onDestroy();
        Intent intent = new Intent();
        intent.setAction("com.restart.service");
        //发送广播
        sendBroadcast(intent);
        unregisterReceiver(mReceiver);
    }
}
```

#### onDestroy方法里重启service

**service +broadcast**  方式，就是当service走ondestory的时候，发送一个自定义的广播，当收到广播的时候，重新启动service；
【结论】当使用类似口口管家等第三方应用或是在setting里-应用-强制停止时，APP进程可能就直接被干掉了，onDestroy方法都进不来，所以还是无法保证。

```xml
<receiver android:name="com.dbjtech.acbxt.waiqin.BootReceiver" >  
    <intent-filter>  
        <action android:name="android.intent.action.BOOT_COMPLETED" />  
        <action android:name="android.intent.action.USER_PRESENT" />  
        <action android:name="com.dbjtech.waiqin.destroy" />//这个就是自定义的action  
    </intent-filter>  
</receiver>  
```

在onDestroy时：

```java
@Override  
public void onDestroy() {  
    stopForeground(true);  
    Intent intent = new Intent("com.dbjtech.waiqin.destroy");  
    sendBroadcast(intent);  
    super.onDestroy();  
}  
```

在BootReceiver里

```java
public class BootReceiver extends BroadcastReceiver {  
    @Override  
    public void onReceive(Context context, Intent intent) {  
        if (intent.getAction().equals("com.dbjtech.waiqin.destroy")) {  
            //TODO  
            //在这里写重新启动service的相关操作  
                startUploadService(context);  
        }  
    }  
}  
```

也可以直接在onDestroy（）里startService

```java
@Override  
public void onDestroy() {  
     Intent sevice = new Intent(this, MainService.class);  
     this.startService(sevice);  
    super.onDestroy();  
}  
```

#### 监听系统广播判断Service状态

通过系统的一些广播，比如：手机重启、界面唤醒、应用状态改变等等监听并捕获到，然后判断我们的Service是否还存活，别忘记加权限。
【结论】这也能算是一种措施，不过感觉监听多了会导致Service很混乱，带来诸多不便

```xml
<receiver android:name="com.dbjtech.acbxt.waiqin.BootReceiver" >  
    <intent-filter>  
        <action android:name="android.intent.action.BOOT_COMPLETED" />  
        <action android:name="android.intent.action.USER_PRESENT" />  
        <action android:name="android.intent.action.PACKAGE_RESTARTED" />  
        <action android:name="com.dbjtech.waiqin.destroy" />  
    </intent-filter>  
</receiver>  
```

BroadcastReceiver中：

```java
@Override  
public void onReceive(Context context, Intent intent) {  
    if (Intent.ACTION_BOOT_COMPLETED.equals(intent.getAction())) {  
        System.out.println("手机开机了....");  
        startUploadService(context);  
    }  
    if (Intent.ACTION_USER_PRESENT.equals(intent.getAction())) {  
            startUploadService(context);  
    }  
}  
```



#### Application加上Persistent属性

看Android的文档知道，当进程长期不活动，或系统需要资源时，会自动清理门户，杀死一些Service，和不可见的Activity等所在的进程。但是如果某个进程不想被杀死（如数据缓存进程，或状态监控进程，或远程服务进程），可以这么做：android:persistent="true"
【结论】据说这个属性不能乱设置，不过设置后，的确发现优先级提高不少，或许是相当于系统级的进程，但是还是无法保证存活

```xml
<application  
    android:name="com.test.Application"  
    android:allowBackup="true"  
    android:icon="@drawable/ic_launcher"  
    android:label="@string/app_name"  
    android:persistent="true"
    android:theme="@style/AppTheme" >  
</application>  
```

#### 提升service优先级（可能无效）

在AndroidManifest.xml文件中对于intent-filter可以通过android:priority = "1000"这个属性设置最高优先级，1000是最高值，如果数字越小则优先级越低，同时适用于广播。
【结论】目前看来，priority这个属性貌似只适用于broadcast，对于Service来说可能无效

```xml
<service  
    android:name="com.dbjtech.acbxt.waiqin.UploadService"  
    android:enabled="true" >  
    <intent-filter android:priority="1000" >  
        <action android:name="com.dbjtech.myservice" />  
    </intent-filter>  
</service>  
```

## 服务Service与线程Thread的区别

- **两者概念的迥异**

  - Thread 是程序执行的最小单元，它是分配CPU的基本单位，android系统中UI线程也是线程的一种，当然Thread还可以用于执行一些耗时异步的操作。
  - Service是Android的一种机制，服务是运行在主线程上的，它是由系统进程托管。它与其他组件之间的通信类似于client和server，是一种轻量级的IPC通信，这种通信的载体是binder，它是在linux层交换信息的一种IPC，而所谓的Service后台任务只不过是指没有UI的组件罢了。

- **两者的执行任务迥异**

  - 在android系统中，线程一般指的是工作线程(即后台线程)，而主线程是一种特殊的工作线程，它负责将事件分派给相应的用户界面小工具，如绘图事件及事件响应，因此为了保证应用 UI 的响应能力主线程上不可执行耗时操作。如果执行的操作不能很快完成，则应确保它们在单独的工作线程执行。
  - Service 则是android系统中的组件，一般情况下它运行于主线程中，因此在Service中是不可以执行耗时操作的，否则系统会报ANR异常，之所以称Service为后台服务，大部分原因是它本身没有UI，用户无法感知(当然也可以利用某些手段让用户知道)，但如果需要让Service执行耗时任务，可在Service中开启单独线程去执行。

- **两者使用场景**

  - 当要执行耗时的网络或者数据库查询以及其他阻塞UI线程或密集使用CPU的任务时，都应该使用工作线程(Thread)，这样才能保证UI线程不被占用而影响用户体验。
  - 在应用程序中，如果需要长时间的在后台运行，而且不需要交互的情况下，使用服务。比如播放音乐，通过Service+Notification方式在后台执行同时在通知栏显示着。

- **两者的最佳使用方式**

  在大部分情况下，Thread和Service都会结合着使用，比如下载文件，一般会通过Service在后台执行+Notification在通知栏显示+Thread异步下载，再如应用程序会维持一个Service来从网络中获取推送服务。在Android官方看来也是如此，所以官网提供了一个Thread与Service的结合来方便我们执行后台耗时任务，它就是IntentService，(如果想更深入了解IntentService，可以看博主的另一篇文章：[Android 多线程之IntentService 完全详解](http://blog.csdn.net/javazejian/article/details/52426425))，当然 IntentService并不适用于所有的场景，但它的优点是使用方便、代码简洁，不需要我们创建Service实例并同时也创建线程，某些场景下还是非常赞的！由于IntentService是单个worker thread，所以任务需要排队，因此不适合大多数的多任务情况。

- **两者的真正关系**

  - 两者没有半毛钱关系。

## Android 中服务的调用实例（★★★）

### 电话窃听器（★★★）

需求：
开启一个服务监听用户电话，当电话被接通时开始录音，电话挂断时停止录音。

**① 新建一个 Android 工程**《电话窃听器》，包名：com.itheima.listenCall。
**② 在 src 目录下新创建一个 MyService 类继承 Service 类**，在该类中实现核心业务方法， 实现监听电话，以及完成录音的功能。

```java
public class MyService extends Service {
    /* 绑定服务时调用     */
    @Override
    public IBinder onBind(Intent intent) {
        return null;
    }

    /* 服务被创建时调用     */
    @Override
    public void onCreate() {
        super.onCreate();
        System.out.println("服务已经被创建。");
        TelephonyManager telephonyManager = (TelephonyManager)
                getSystemService(TELEPHONY_SERVICE);
        telephonyManager.listen(new MyPhoneListener(), PhoneStateListener.LISTEN_CALL_STATE);
    }

    class MyPhoneListener extends PhoneStateListener {
        MediaRecorder recorder;
        boolean isCalling = false;

        @Override
        public void onCallStateChanged(int state, String incomingNumber) {
            super.onCallStateChanged(state, incomingNumber);

            if (TelephonyManager.CALL_STATE_OFFHOOK == state) {
                System.out.println("开始通话。。。");
                isCalling = true;
			//新建一个 MediaRecorder 对象
                recorder = new MediaRecorder();
			//设置声音来源
                recorder.setAudioSource(AudioSource.MIC);
			//设置输入格式
                recorder.setOutputFormat(OutputFormat.THREE_GPP);
			//格式化日期，作为文件名称
                SimpleDateFormat format = new
                        SimpleDateFormat("yyyy-MM-dd_hh_mm_ss");
                String date = format.format(new Date());
			//设置输出到的文件
                recorder.setOutputFile(getFilesDir() + "/" + date + ".3gp");
			//设置音频编码
                recorder.setAudioEncoder(AudioEncoder.DEFAULT);
                try {
			//录音准备
                    recorder.prepare();
			//录音开始
                    recorder.start();
                } catch (Exception e) {
                    e.printStackTrace();
                }
            } else if (TelephonyManager.CALL_STATE_IDLE == state && isCalling) {
                recorder.stop();
                recorder.release();
                isCalling = false;
                System.out.println("录音结束。");
            }
        }
    }
}
```

**③ 在 MainActivity 类中启动 Service**

```java
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    Intent service = new Intent();
    service.setClass(this, MyService.class);
    startService(service);
}
```

**④ 在 AndroidManifest.xml 文件中注册该 Service**

> Tip：在 Android 中四大组件都需要在清单文件中进行注册。

```xml
<service android:name="com.itheima.listenCall.MyService"/>
```



**⑤ 在 AndroidManifest.xml 中添加权限**。

Tip：在该案例中，我们把录音文件存储在 data/data/com.itheima.listenCall/files 目录中，因此不需要声明外部存储的写权限。

```xml
<uses-permission android:name="android.permission.READ_PHONE_STATE"/>
<uses-permission android:name="android.permission.RECORD_AUDIO"/>
```

⑥ 将本工程部署到模拟器中，然后通过 DDMS 给该模拟器拨打电话，当我们接听一段时间并关闭后，发现控制台成功打印出了录音信息。
![](http://upload-images.jianshu.io/upload_images/9028834-16584e3a4cd39c77.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

打开 data/data/com.itheima.listenCall/files 目录发现，录音文件被成功保存了。我们将该文件导出到电脑上，发现声音可以正常播放。
![](http://upload-images.jianshu.io/upload_images/9028834-469b66095ee75ace.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 本地服务调用音乐播放器

- **①** 新创建一个 Android 工程《音乐播放器》，包名：com.itheima.musicPlayer。
  在 **res 目录下新建一个文件夹 raw**（名字必须为 raw，约定大于配置的原则），然后在raw 目录中拷贝进一个音乐文件，注意文件名必须遵循 Android 资源文件的命名规则。 目录结构如下图：
  ![目录结构](http://upload-images.jianshu.io/upload_images/9028834-44fbe618d3551962.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
- **②** 在 src 目录下，**新建一个 MediaService 继承 Service 类**，在该类中实现核心服务的方法。

```java
public class MediaService extends Service {
    //声明一个 MediaPlayer 对象
    private MediaPlayer player;

    @Override
    public IBinder onBind(Intent intent) {
        System.out.println("服务返回 MediaController 对象了......");
        return new MediaController();
    }

    @Override
    public void onCreate() {
        System.out.println("音乐服务已经被创建......");
		//初始化音乐播放器
        player = MediaPlayer.create(this, R.raw.m);
    }

    //自定义一个 Binder 对象，Binder 是 IBinder 接口的子类
    class MediaController extends Binder {
        public void play() {
            player.start();
        }
        public void pause() {
            player.pause();
        }
        public void stop() {
            player.stop();
        }
        //获取音乐的总时长
        public int getDuration() {
            return player.getDuration();
        }
        //获取当前播放位置
        public int getCurrentPostion() {
            return player.getCurrentPosition();
        }
        //判断是否在播放
        public boolean isPlaying() {
            return player.isPlaying();
        }
    }
}
```

- **③** **布局文件**activity_main.xml 清单如下：

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    tools:context=".MainActivity">
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center_horizontal"
        android:text="音乐播放器"
        android:textColor="#ff0000"
        android:textSize="28sp" />
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="horizontal">
        <Button
            android:id="@+id/bt_play"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:onClick="play"
            android:text="播放" />
        <Button
            android:id="@+id/bt_pause"
            android:layout_width="0dp"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:onClick="pause"
            android:text="暂停" />
    </LinearLayout>
</LinearLayout>
```

- **④** **MainActivity**，在该类中**完成业务的控制**，代码清单如下：

```java
public class MainActivity extends Activity {
    //声明进度条
    private ProgressBar pb;
    //声明自定义的 MediaController 对象
    private MediaController mediaController;
    private boolean isRunning;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
		//实例化进度条
        pb = (ProgressBar) findViewById(R.id.pb);
		//创建一个用于启动服务的显示意图，指向我们自定义的 MediaService 类
        Intent intent = new Intent(this, MediaService.class);
		//绑定服务，同时服务开启，如果成功则返回 true 否则返回 false
        isRunning = bindService(intent, new MediaConnection(), BIND_AUTO_CREATE);
        if (isRunning) {
            System.out.println("音乐播放器服务绑定成功！");
        } else {
            System.out.println("音乐播放器服务绑定失败！");
        }
    }

    //用于循环更新当前播放进度
    private void updateProgressBar() {
        new Thread(new Runnable() {
            @Override
            public void run() {
                while (true) {
                    SystemClock.sleep(400);
                    pb.setProgress(mediaController.getCurrentPostion());
                    if (mediaController.getDuration() == mediaController.getCurrentPostion()){
                        break;
                    }
                }
            }
        }).start();
    }

    public void play(View view) {
        if (mediaController != null) {
			//如果音乐正在播放则不能再次播放
            if (mediaController.isPlaying()) {
                Toast.makeText(this, "音乐播放中", 0).show();
                return;
            } else {
                mediaController.play();
                Toast.makeText(this, "音乐开是播放", 0).show();
            }
        }
    }

    //暂停
    public void pause(View view) {
        if (mediaController != null) {
            mediaController.pause();
        }
    }

    //停止
    public void stop(View view) {
        if (mediaController != null) {
			//停止的时候将进度条设置为初始位置
            pb.setProgress(0);
            mediaController.stop();
            Toast.makeText(this, "音乐已经关闭！", 0).show();
        }
    }

    //新建一个 ServiceConnection 类
    class MediaConnection implements ServiceConnection {
        /* 当 service 被绑定的时候回调该函数         */
        @Override
        public void onServiceConnected(ComponentName name, IBinder service) {
		//返回的 IBinder 对象其实就是我们自定义的 MediaController 类对象
        }

        /**
         * mediaController = (MediaController) service;
         * //给进度条设置最大值
         * pb.setMax(mediaController.getDuration());
         * //更新进度条
         * updateProgressBar(); System.out.println("服务已经连接......");
         * 服务被关闭或者断开的时候调用该方法
         */
        @Override
        public void onServiceDisconnected(ComponentName name) {
            System.out.println("服务已经断开......");
        }
    }
}
```

- **⑤** 在 AndroidManifest.xml 中**注册Service**

```xml
<service android:name="com.itheima.musicPlayer.MediaService"/>
```

- ⑥ 将工程部署到模拟器上，点击播放，发现成功播放了音乐。点击暂停，发现音乐暂停了，然后点击播放，音乐再次响起。点击停止，问题来了，我们发现点击停止后再次点击播放音乐没能再次播放，因为这里面直接调用MediaPlayer 的 stop 方法是有 bug 的。因此 为了解决这样的问题，我们应该将停止调用层 pause 方法，同时只需调用 MediaPlayer 的seekTo（int）方法将音乐设置到开始位置。
  ![播放器](http://upload-images.jianshu.io/upload_images/9028834-b670e265a8594cf3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
  ![控制台](http://upload-images.jianshu.io/upload_images/9028834-d4945b254c9ec5bd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

# Service 面试题
## Service和Activity交互的方式有哪些？
### 1、startService方式交互
Activity是不能很直接的与Service进行交互，需要借助与其它组件来完成。常见的就是利用**广播接收器**。
在Service的onStartCommand方法里发送广播，Activity接受广播。


### 2、bindService交互
使用Binder、Message、AIDL类进行交互。
简单来说，就是在Service的onBind方法中把这些对象return出去，再在Activity里的ServiceConnection的onServiceConnected方法中获取这些对象。

| 方式      | Service                | Activity                                 | 备注                                       |
| ------- | ---------------------- | ---------------------------------------- | ---------------------------------------- |
|         | **在onBind方法返回**        | **重写ServiceConnection类的<br />onServiceConnected方法中获取** |                                          |
| Binder  | binder                 | LocalService.LocalBinder binder = (LocalService.LocalBinder) service;<br />mService = binder.getService();<br />即可调用Service中的方法 | Service中自定义的Binder中自定义了getService返回当前Service |
| Message | mMessenger.getBinder() | new Messenger(service);<br />即可通过这个Message向Service发消息 | Service中自定义了 Handler处理消息                 |
| AIDL    | Stub                   | AlipayRemoteService  alipayRemoteService = Stub.asInterface(service); |                                          |




引用：
[Android四大组件——Service后台服务、前台服务、IntentService、跨进程服务、无障碍服务、系统服务](http://blog.csdn.net/qq_30379689/article/details/53318861)
[关于Android Service真正的完全详解，你需要知道的一切](http://blog.csdn.net/javazejian/article/details/52709857)
[Android开发之如何保证Service不被杀掉（broadcast+system/app）](http://blog.csdn.net/mad1989/article/details/22492519)


















