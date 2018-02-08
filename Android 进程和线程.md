<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [【Android 进程和线程】](#android-%E8%BF%9B%E7%A8%8B%E5%92%8C%E7%BA%BF%E7%A8%8B)
- [Android 进程](#android-%E8%BF%9B%E7%A8%8B)
  - [进程生命周期](#%E8%BF%9B%E7%A8%8B%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F)
    - [1、前台进程foreground process](#1%E5%89%8D%E5%8F%B0%E8%BF%9B%E7%A8%8Bforeground-process)
    - [2、可见进程Visable process](#2%E5%8F%AF%E8%A7%81%E8%BF%9B%E7%A8%8Bvisable-process)
    - [3、服务进程service process](#3%E6%9C%8D%E5%8A%A1%E8%BF%9B%E7%A8%8Bservice-process)
    - [4、后台进程background process](#4%E5%90%8E%E5%8F%B0%E8%BF%9B%E7%A8%8Bbackground-process)
    - [5、空进程    empty process](#5%E7%A9%BA%E8%BF%9B%E7%A8%8B----empty-process)
  - [多进程](#%E5%A4%9A%E8%BF%9B%E7%A8%8B)
    - [多进程好处](#%E5%A4%9A%E8%BF%9B%E7%A8%8B%E5%A5%BD%E5%A4%84)
    - [多进程实现](#%E5%A4%9A%E8%BF%9B%E7%A8%8B%E5%AE%9E%E7%8E%B0)
    - [有哪些陷阱](#%E6%9C%89%E5%93%AA%E4%BA%9B%E9%99%B7%E9%98%B1)
      - [Application的多次重建](#application%E7%9A%84%E5%A4%9A%E6%AC%A1%E9%87%8D%E5%BB%BA)
      - [静态成员的失效](#%E9%9D%99%E6%80%81%E6%88%90%E5%91%98%E7%9A%84%E5%A4%B1%E6%95%88)
      - [文件共享问题](#%E6%96%87%E4%BB%B6%E5%85%B1%E4%BA%AB%E9%97%AE%E9%A2%98)
      - [断点调试问题](#%E6%96%AD%E7%82%B9%E8%B0%83%E8%AF%95%E9%97%AE%E9%A2%98)
      - [总结](#%E6%80%BB%E7%BB%93)
- [Android 线程](#android-%E7%BA%BF%E7%A8%8B)
  - [UI线程](#ui%E7%BA%BF%E7%A8%8B)
  - [worker线程](#worker%E7%BA%BF%E7%A8%8B)
  - [异步任务ASYNC TASK](#%E5%BC%82%E6%AD%A5%E4%BB%BB%E5%8A%A1async-task)
  - [线程安全方法](#%E7%BA%BF%E7%A8%8B%E5%AE%89%E5%85%A8%E6%96%B9%E6%B3%95)
  - [进程间通信](#%E8%BF%9B%E7%A8%8B%E9%97%B4%E9%80%9A%E4%BF%A1)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->



# 【Android 进程和线程】
>相关文章：[JAVA 线程](http://blog.csdn.net/moira33/article/details/78894952)


我们都知道，在操作系统中**进程是OS分配资源的最小单位**，而**线程是执行任务的最小单位**。
一个进程可以拥有多个线程执行任务，这些线程可以共享该进程分配到的资源。

当我们的**app启动**运行后，在该app**没有其他组件正在运行**的前提下，Android系统会**启动一个新Linux进程来运行app**，这个进程只包含了一个线程在运行。在**默认**情况下，app的组件都运行在该进程中，最初就包含的这个线程也被称为**主线程或者UI线程**。如果我们启动该app的时候，系统中已经有一个进程在运行该app的组件，那么该app也会在该进程中运行。

当然，我们也可以让app中不同的组件运行在不同的进程中，也可以在任意进程中新开线程执行任务。

# Android 进程



##     进程生命周期
Android系统启动后会载入通用的framework的代码与资源之后，启动一个Zygote进程。为了启动一个新的程序进程，系统会fork Zygote进程生成一个新的进程，然后在新的进程中加载并运行应用程序的代码。这就使得大多数的RAM pages被用来分配给framework的代码，同时促使RAM资源能够在应用的所有进程之间进行共享。

 Android系统在**内存不足的情况下会杀死一些进程来满足那些直接和用户交互的进程**，在这些被杀死的进程中运行的组件也会被注销掉。当这些组件重新运行时， 才会启动该线程。那么，系统将会如何决定要杀死哪个进程呢？Android系统需要根据进程与用户的相关重要性来判断的。例如，与那些正在显示activity的进程相比，系统更倾向于杀死那些不再显示的activity所在的进程。

 Android系统会尽可能长时间的维持一个进程的运行，但是最终会回收旧进程的内存空间提供给新的进程或是更重要的进程使用。Android系统采用了基于组件运行的进程以及组件状态的“重要性层级”的策略，根据重要性逐层清除进程。

 重要性层级从高到低共分为5层：

### 1、前台进程foreground process

 当前正在与用户交互的进程。如果一个进程P满足如下任意一个条件，则进程P被称为前台进程：

*   当前**正在与用户交互**(调用过**resume**方法)的activity在进程P中运行
*   某个**service**与当前正在与**用户交互的activity相互绑定**，该service运行于进程P中
*   某个**service**调用了**startForeground**()方法，该service运行在进程P中
*   某个**service**正在执行某个**生命周期的回调**方法，该service运行在进程P中
*   某个**broadcastReceiver**正在执行它的**onReceive**函数，该broadcastReceiver运行在进程P中

 通常情况下，某时刻系统只会有很少一部分前台进程存在。它们只会在内存非常低的情况下才会被杀死，在这种情况下，设备达到了“memory paging state”状态，只有杀死一些前台进程才能保证系统的快速响应。

### 2、可见进程Visable process

没有任何前台组件在此进程中运行，但是仍然可以影响到用户所看到的屏幕。如果进程P满足以下任意一个条件，则进程P被称为可见进程：

*   某个**activity**并**不运行于前台**，但仍能**被用户所见**(调用了**pause**方法)，activity运行于进程P中。
  例如某activity启动了一个dialog，仍然可以看到该activity。
*   如果某个**service与visible activity或foreground activity绑定**，该service运行于进程P中。

 可见进程相对而言比较重要，但在某些情况下为了保证前台进程的运行，系统还是会杀死这些可见进程的。

### 3、服务进程service process

 如果一个**service通过startservice方法启动**，并且不属于以上两种更高级别的情况，那么运行该service的进程被称为service process。尽管该service并不与任何能被用户看到的组件绑定，但是它们做的工作是用户关心的，例如音乐播放，文件下载等等。

### 4、后台进程background process

 某个当前**不可见的activity**(回调了**onStop**方法)运行于该线程。此类线程无法直接影响到用户体验，系统可能随时杀死此类进程回收内存供以上三种进程使用。通常情况下，系统中有**多个后台进程**在运行，所以它们**被存于**一个**LRU列表中**，从而最不常被用户用到的进程会先被杀死。如果一个activity正常回调了它的生命周期的函数并存储了相应的状态数据，那么杀死该后台进程是不会影响到用户体验的。因为当用户试图返回到该activity时，系统会恢复该activity所有的状态。

### 5、空进程    empty process

 该进程中**没有运行任何组件**，保持此类进程存在的唯一原因就是**等待任务**。一旦有组件需要运行，则可以缩短进程启动时间。所以系统往往会杀死这些进程用来平衡进程缓存和底层内核缓存之间的系统资源。

 Android系统中，一个进程的等级是可以动态提升的，因为其他的进程可能会依赖于该进程。某个进程为其他进程提供服务，那么**该进程的等级一定不会低于它所服务的进程**。例如，某content provider 运行于进程A中，它为处于进程B中的客户端B提供服务；或者如果处于进程A的service绑定于位于进程B中的组件，那么A进程的重要等级只会高于或等于进程B。

由于一个运行service的进程等级要高于那些运行处于后台activity的进程，如果我们需要有长时间执行的操作，那么从一个activity中启动一个service来完成这些操作就比在activity中新开子线程来完成这些操作效果要好（特别是这些子线程的持续时间要比activity长的情况下）。例如，一个activity想要上传一张图片给服务器，那么应当开启一个service在后台来完成上传操作，即使用户离开了当前activity，这些操作也能够在后台完成。使用service可以保证这些操作至少具有service优先级，无论当前activity的状态是否改变。这也是为什么broadcast receiver应当使用service而不是简单的把耗时操作放在子线程中的原因。

## 多进程
正常情况下，一个apk启动后只会运行在一个进程中，其进程名为AndroidManifest.xml文件中指定的应用包名，所有的基本组件都会在这个进程中运行。
但是如果需要将某些组件（如Service、Activity等）**运行在单独的进程中**，就需要用到**manifest文件**中的**android:process属性**了。我们可以为android的基础组件指定process属性来指定它们运行在指定进程中。

### 多进程好处

1. 我们知道**Android系统对每个应用进程的内存占用是有限制的**，而且**占用内存越大**的进程，通常**被系统杀死**的可能性**越大**。让一个组件运行在单独的进程中，可以减少主进程所占用的内存，降低被系统杀死的概率.

2. 如果**子进程**因为某种原因**崩溃**了，**不会直接导致主程序的崩溃**，可以降低我们程序的崩溃率。

3. 即使**主进程退出**了，我们的**子进程仍然可以继续工作**，假设子进程是**推送服务**，在主进程退出的情况下，仍然能够保证用户可以收到推送消息。

### 多进程实现

对process属性的设置有两种形式：

- 第一种形式如 android:process=":remote"，以冒号开头，冒号后面的字符串原则上是可以随意指定的。如果我们的包名为“com.example.processtest”，则实际的进程名为“com.example.processtest:remote”。这种设置形式表示该进程为当前应用的私有进程，其他应用的组件不可以和它跑在同一个进程中。

- 第二种情况如 android:process="com.example.processtest.remote"，以小写字母开头，表示运行在一个以这个名字命名的全局进程中，**其他应用通过设置相同的ShareUID可以和它跑在同一个进程**。

### 有哪些陷阱

我们已经开启了应用内多进程，那么，开启多进程是不是只是我们看到的这么简单呢？其实这里面会有一些陷阱，稍微不注意就会陷入其中。我们首先要明确的一点是进程间的内存空间时不可见的。从而，开启多进程后，我们需要面临这样几个问题：

1）Application的多次重建。

2）静态成员的失效。

3）文件共享问题。

4）断点调试问题。

Manifest文件
```xml
<?xml version="1.0" encoding="utf-8"?>  
<manifest xmlns:android="http://schemas.android.com/apk/res/android"  
    package="com.example.processtest"  
    android:versionCode="1"  
    android:versionName="1.0" >  
  
    <uses-sdk  
        android:minSdkVersion="8"  
        android:targetSdkVersion="19" />  
  
    <application  
        android:name="com.example.processtest.MyApplication"  
        android:icon="@drawable/ic_launcher"  
        android:label="@string/app_name">  
        <activity  
            android:name=".ProcessTestActivity"  
            android:label="@string/app_name" >  
            <intent-filter>  
                <action android:name="android.intent.action.MAIN" />  
  
                <category android:name="android.intent.category.LAUNCHER" />  
            </intent-filter>  
        </activity>  
          
        <service  
            android:name=".ProcessTestService"  
            android:process=":remote">  
        </service>  
    </application>  
  
</manifest>  
```



#### Application的多次重建
我们先通过一个简单的例子来看一下第一种情况。

Manifest文件如上面提到的，定义了两个类：ProcessTestActivity和ProcessTestService,我们只是在Activity的onCreate方法中直接启动了该Service，同时，我们自定义了自己的Application类。代码如下：

```java
public class MyApplication extends Application {  
    public static final String TAG = "viclee";  
    @Override  
    public void onCreate() {  
        super.onCreate();  
        int pid = android.os.Process.myPid();  
        Log.d(TAG, "MyApplication onCreate");  
        Log.d(TAG, "MyApplication pid is " + pid);  
    }  
}  
```

```java
public class ProcessTestActivity extends Activity {  
    public final static String TAG = "viclee";  
  
    @Override  
    protected void onCreate(Bundle savedInstanceState) {  
        super.onCreate(savedInstanceState);  
        setContentView(R.layout.activity_process_test);  
  
        Log.i(TAG, "ProcessTestActivity onCreate");  
        this.startService(new Intent(this, ProcessTestService.class));  
    }  
}  
```

  

```java
public class ProcessTestService extends Service {  
    public static final String TAG = "viclee";  
  
    @Override  
    public void onCreate() {  
        Log.i(TAG, "ProcessTestService onCreate");  
    }  
  
    @Override  
    public IBinder onBind(Intent arg0) {  
        return null;  
    }    
}  
```
执行上面这段代码，查看打印信息：
![](http://upload-images.jianshu.io/upload_images/9028834-b93305d4498ca407?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) 

我们发现**MyApplication的onCreate方法调用了两次**，分别是在启动ProcessTestActivity和ProcessTestService的时候，而且我们发现打印出来的pid也不相同。由于通常会在Application的onCreate方法中做一些全局的初始化操作，它被初始化多次是完全没有必要的。
出现这种情况，是由于即使是**通过指定process属性启动新进程的情况下，系统也会新建一个独立的虚拟机，自然需要重新初始化一遍Application**。那么怎么来解决这个问题呢？

我们可以通过在自定义的Application中通过进程名来区分当前是哪个进程，然后单独进行相应的逻辑处理。

```java
public class MyApplication extends Application {  
    public static final String TAG = "viclee";    
    @Override  
    public void onCreate() {  
        super.onCreate();  
        int pid = android.os.Process.myPid();  
        Log.d(TAG, "MyApplication onCreate");  
        Log.d(TAG, "MyApplication pid is " + pid);  
  
        ActivityManager am = (ActivityManager) getSystemService(Context.ACTIVITY_SERVICE);  
        List<ActivityManager.RunningAppProcessInfo> runningApps = am.getRunningAppProcesses();  
        if (runningApps != null && !runningApps.isEmpty()) {  
            for (ActivityManager.RunningAppProcessInfo procInfo : runningApps) {  
                if (procInfo.pid == pid) {  
                     if (procInfo.processName.equals("com.example.processtest")) {  
                         Log.d(TAG, "process name is " + procInfo.processName);  
                     } else if (procInfo.processName.equals("com.example.processtest:remote")) {  
                         Log.d(TAG, "process name is " + procInfo.processName);  
                     }  
                }  
            }  
        }  
    }  
}  
```

运行之后，查看Log信息，
![](http://upload-images.jianshu.io/upload_images/9028834-554746e5efcfc306?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) 
图中可以看出，不同的进程执行了不同的代码逻辑，可以通过这种方式来区分不同的进程需要完成的初始化工作。

#### 静态成员的失效
下面我们来看第二个问题，将之前定义的Activity和Service的代码进行简单的修改，代码如下：

```java
public class ProcessTestActivity extends Activity {  
    public final static String TAG = "viclee";  
    public static boolean processFlag = false;  
  
    @Override  
    protected void onCreate(Bundle savedInstanceState) {  
        super.onCreate(savedInstanceState);  
        setContentView(R.layout.activity_process_test);  
  
        processFlag = true;  
        Log.i(TAG, "ProcessTestActivity onCreate");  
        this.startService(new Intent(this, ProcessTestService.class));  
    }  
}  
```

```java
public class ProcessTestService extends Service {  
    public static final String TAG = "viclee";  
  
    @Override  
    public void onCreate() {  
        Log.i(TAG, "ProcessTestService onCreate");  
        Log.i(TAG, "ProcessTestActivity.processFlag is " + ProcessTestActivity.processFlag);  
    }  
  
    @Override  
    public IBinder onBind(Intent arg0) {  
        return null;  
    }    
}  
```
重新执行代码，打印Log
![](http://upload-images.jianshu.io/upload_images/9028834-df886c82e504c795?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

从上面的代码和执行结果看，我们在Activity中定义了一个标志processFlag并在onCreate中修改了它的值为true，然后启动Service，但是在Service中读到这个值却为false。
按照正常的逻辑，**静态变量是可以在应用的所有地方共享的，但是设置了process属性后，产生了两个隔离的内存空间，一个内存空间里值的修改并不会影响到另外一个内存空间**。

#### 文件共享问题
第三个问题是文件共享问题。**多进程情况下会出现两个进程在同一时刻访问同一个数据库文件的情况**。这就可能造成资源的竞争访问，导致诸如数据库损坏、数据丢失等。在多线程的情况下我们有锁机制控制资源的共享，但是在多进程中比较难，虽然有文件锁、排队等机制，但是在Android里很难实现。解决办法就是**多进程的时候不并发访问同一个文件**，比如子进程涉及到操作数据库，就可以考虑调用主进程进行数据库的操作。

#### 断点调试问题
最后是断点调试的问题。调试就是跟踪程序运行过程中的堆栈信息，由于**每个进程都有自己独立的内存空间和各自的堆栈**，**无法实现在不同的进程间调试**。不过可以通过下面的方式实现：调试时去掉AndroidManifest.xml中android:process标签，这样保证调试状态下是在同一进程中，堆栈信息是连贯的。待调试完成后，再将标签复原。 

#### 总结
从上面的例子中我们可以看到，android实现应用内多进程并不是简单的设置属性process就可以了，而是会产生很多特殊的问题。像前面提到的，android启动多进程模式后，不仅静态变量会失效，而且类似的如**同步锁机制、单例模式也会存在同样的问题**。这就需要我们在使用的时候多加注意。而且**设置多进程之后，各个进程间就无法直接相互访问数据，只能通过[AIDL](http://blog.csdn.net/moira33/article/details/78924305)等进程间通信方式来交换数据**。
[**例子源代码下载**](http://download.csdn.net/detail/goodlixueyong/9272723)


# Android 线程
## UI线程
 当应用程序启动后，系统将会创建一个主线程来运行应用程序。主线程非常重要，它负责为适当的用户控件分发任务和事件，包括绘制任务等等。同时，主线程也负责UI组件和应用程序的交互，所以我们也称主线程为UI线程。

 系统并不会为每个组件单独开启一个线程来运行，所有的组件都会在主线程中初始化并运行运行在同一个进程中，系统通过主线程来调用每个组件。所以，系统回调方法（例如onKeyDown，生命周期回调方法等）通常运行于主线程。

例如，当用户点击屏幕上的按钮，UI线程会将点击事件分发给控件。控件就会设置自身的按下状态，并将重绘请求添加到事件请求队列。UI线程从事件队列中取出该重绘请求后，通知该控件重绘。

当用户和app交互频繁时，单线程的模式可能会导致响应速度慢，用户体验不尽人意。如果在主线程中进行网络或数据库请求等耗时操作，则会导致线程阻塞，主线程将无法调度分发事件和任务。当超过5s的阻塞会使系统弹出ANR窗口。另外,UI控件都不是线程安全的，所以系统规定只能在UI线程中修改控件。我们需要遵循两个规则：

1.  不要使UI线程阻塞
2.  不要在UI线程之外修改控件

## worker线程

上面讨论了只有UI线程工作的情况。为了提高应用程序UI的响应速度，获得更好的用户体验，我们需要把耗时操作放在子线程中来完成。但是我们需要注意的是，不要在子线程中操作UI控件。我们通常使用Android的Handler机制来解决线程间通信的问题，详细请参看之前的文章[Android线程间异步通信机制源码分析](http://www.cnblogs.com/cqumonk/p/4752843.html)。同时，Android也提供了async task来完成异步任务。

## 异步任务ASYNC TASK

async task在子线程中执行耗时任务，然后将结果返回给UI线程，无需自己手动创建handler。关于async task的使用就不在这里介绍了，在使用async task的过程中，我们需要注意的是多线程问题。由于运行配置的问题（例如屏幕横竖方向改变），会导致子线程任务未经过我们允许就重新启动执行。

## 线程安全方法

多数情况下，我们的方法有可能被多个线程所调用，所以我们必须考虑到线程安全的问题。特别是对于那些可以被远程调用的方法更是如此，例如，绑定service的方法。当我们试图调用在IBinder中实现的方法，如果调用者和IBinder处于同一个进程，那么方法将会在调用者所在线程中执行。如果调用者与IBinder并不处于同一个进程中，那么系统从所维护的线程池中取出一个线程来执行该方法，该线程池与IBinder运行在同一个进程中（并非在UI线程中执行）。举个栗子，尽管service的进程的UI线程将会调用service的onBind方法，然而在onBind方法所返回的IBinder对象中实现的那些方法就会被线程池中线程执行。因为一个service可以由多个客户端访问，线程池中的多个线程可以在同一时刻调用同一方法。所以IBinder对象中实现的方法需要是线程安全的。

类似的，一个ContentProvider可以接收到来自不同进程的数据请求，虽然CP和CR类中隐藏了进程间通信管理的细节，但是CP中对应的查询，删除，修改，插入等请求方法将会被交给CP所在进程的线程池中线程来执行。这些方法可能在同一时刻被多个进程所调用，所以这些方法必须是线程安全的。

## 进程间通信

Android系统提供了远程调用RPC机制来完成进程通信IPC，通过RPC机制，应用程序中的组件（例如activity）作为调用者在本地调用某个方法，该方法在远程（另外一个进程中）执行，然后将结果返回给调用者。这就需要将所调用方法和它的数据解析为操作系统可以理解的程度，然后从本地进程和地址空间传递给远程的进程和地址空间后，再进行重组和执行。






引用：
[Android进程与线程](https://www.cnblogs.com/cqumonk/p/4828616.html)
[Android应用内多进程分析和研究](http://blog.csdn.net/goodlixueyong/article/details/49853079)
[Android进程和线程的区别](http://blog.csdn.net/qq_17475155/article/details/50899382)
[Android开发——Android中常见的4种线程池（保证你能看懂并理解）](http://blog.csdn.net/seu_calvin/article/details/52415337)





