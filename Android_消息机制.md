<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [【Android 消息机制】](#android-%E6%B6%88%E6%81%AF%E6%9C%BA%E5%88%B6)
  - [前言](#%E5%89%8D%E8%A8%80)
    - [Handler使用方法](#handler%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95)
  - [消息机制](#%E6%B6%88%E6%81%AF%E6%9C%BA%E5%88%B6)
    - [MessageQueue](#messagequeue)
      - [enqueueMessage](#enqueuemessage)
      - [next](#next)
    - [Looper](#looper)
      - [子线程创建Handler](#%E5%AD%90%E7%BA%BF%E7%A8%8B%E5%88%9B%E5%BB%BAhandler)
      - [parpare](#parpare)
      - [loop](#loop)
    - [Handler](#handler)
      - [sendMessageAtTime](#sendmessageattime)
      - [dispatchMessage](#dispatchmessage)
        - [① msg.callback](#%E2%91%A0-msgcallback)
        - [① handleCallback(msg)](#%E2%91%A0-handlecallbackmsg)
        - [② mCallback](#%E2%91%A1-mcallback)
        - [② mCallback.handleMessage(msg)](#%E2%91%A1-mcallbackhandlemessagemsg)
        - [③ handleMessage(msg)](#%E2%91%A2-handlemessagemsg)
    - [总结](#%E6%80%BB%E7%BB%93)
    - [架构图](#%E6%9E%B6%E6%9E%84%E5%9B%BE)
- [源码分析](#%E6%BA%90%E7%A0%81%E5%88%86%E6%9E%90)
  - [主线程默认创建Looper对象](#%E4%B8%BB%E7%BA%BF%E7%A8%8B%E9%BB%98%E8%AE%A4%E5%88%9B%E5%BB%BAlooper%E5%AF%B9%E8%B1%A1)
  - [系统的Handler](#%E7%B3%BB%E7%BB%9F%E7%9A%84handler)
    - [H](#h)
    - [应用程序的退出过程](#%E5%BA%94%E7%94%A8%E7%A8%8B%E5%BA%8F%E7%9A%84%E9%80%80%E5%87%BA%E8%BF%87%E7%A8%8B)
  - [1、Looper](#1looper)
    - [1.1 prepare()](#11-prepare)
      - [ThreadLocal](#threadlocal)
    - [1.2 loop()](#12-loop)
    - [1.3 quit()](#13-quit)
    - [1.4 常用方法](#14-%E5%B8%B8%E7%94%A8%E6%96%B9%E6%B3%95)
      - [1.4.1 myLooper](#141-mylooper)
  - [2、Handle](#2handle)
    - [2.1 创建Handler](#21-%E5%88%9B%E5%BB%BAhandler)
      - [2.1.1 无参构造](#211-%E6%97%A0%E5%8F%82%E6%9E%84%E9%80%A0)
      - [2.1.2 有参构造](#212-%E6%9C%89%E5%8F%82%E6%9E%84%E9%80%A0)
    - [2.2 消息分发](#22-%E6%B6%88%E6%81%AF%E5%88%86%E5%8F%91)
    - [2.3 消息发送](#23-%E6%B6%88%E6%81%AF%E5%8F%91%E9%80%81)
      - [2.3.1 sendEmptyMessage](#231-sendemptymessage)
      - [2.3.2 sendEmptyMessageDelayed](#232-sendemptymessagedelayed)
      - [2.3.3 sendMessageDelayed](#233-sendmessagedelayed)
      - [2.3.4 sendMessageAtTime](#234-sendmessageattime)
      - [2.3.5 sendMessageAtFrontOfQueue](#235-sendmessageatfrontofqueue)
      - [2.3.6 post](#236-post)
      - [2.3.7 postAtFrontOfQueue](#237-postatfrontofqueue)
      - [2.3.8 enqueueMessage](#238-enqueuemessage)
    - [2.4 其他方法](#24-%E5%85%B6%E4%BB%96%E6%96%B9%E6%B3%95)
      - [2.4.1 obtainMessage](#241-obtainmessage)
      - [2.4.2 removeMessages](#242-removemessages)
  - [3、MessageQueue](#3messagequeue)
    - [3.1 创建MessageQueue](#31-%E5%88%9B%E5%BB%BAmessagequeue)
    - [3.2 next()](#32-next)
    - [3.3 enqueueMessage](#33-enqueuemessage)
    - [3.4 removeMessages](#34-removemessages)
    - [3.5 postSyncBarrier](#35-postsyncbarrier)
  - [4、 Message](#4-message)
    - [4.1 创建消息](#41-%E5%88%9B%E5%BB%BA%E6%B6%88%E6%81%AF)
    - [4.2 消息池](#42-%E6%B6%88%E6%81%AF%E6%B1%A0)
      - [4.2.1 obtain](#421-obtain)
      - [4.2.2 recycle](#422-recycle)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->



# 【Android 消息机制】

## 前言
在Android开发中，我们都知道**不能在主线程中执行耗时的任务**，**避免ANR**。
>Android中**主线程**也叫**UI线程**，那么从名字上我们也知道主线程主要是用来**创建、更新UI**的，而其他耗时操作，比如网络访问，或者文件处理，多媒体处理等都需要在子线程中操作。
>之所以**在子线程中操作是为了保证UI的流畅程度**，手机显示的刷新频率是60Hz，也就是一秒钟刷新60次，**每16.67毫秒刷新一次**，为了不丢帧，那么主线程处理代码最好不要超过16毫秒。

当子线程处理完数据后，为了防止UI处理逻辑的混乱，Android只允许主线程修改UI，那么这时候就需要Handler来充当子线程和主线程之间的桥梁了。**Handler**机制在Android**多线程**编程中可以说是不可或缺的角色。

### Handler使用方法
我们通常将Handler声明在Activity中，然后覆写Handler中的handleMessage方法，当子线程调用handler.sendMessage()方法后handleMessage方法就会在主线程中执行。
```java
private Handler handler = new Handler() {    
    @Override
    public void handleMessage(Message msg) {        
        Toast.makeText(MainActivity.this, 
            msg.what + "", Toast.LENGTH_SHORT).show();
    }
};
new Runnable(){    
    @Override
    public void run() {        
        Message msg = Message.obtain();
        msg.what = 1;
        handler.sendMessage(msg);
    }
};
```



虽然,我们使用的时候一般只设计到Handler这个类,其实除了**Handler**这个类,还有**Looper**和**MessageQueue**这**三者构成了整个Android的消息机制**。


## 消息机制
### MessageQueue
消息队列，用来**保存handler.sendMessage(msg)或者handler.post(r，delayTime)时的产生消息**，虽然名字叫做队列，但是在实现中，这只是一个**单链表的结构**，学过数据结构的朋友都知道，链表用于插入和删除很方便。
要注意的是，MessageQueue**只负责保存**消息，并**不**会**处理**消息。

**MessageQueue**只包含**两个主要**的**操作**：
#### enqueueMessage
**插入**一条消息
```java
boolean enqueueMessage(Message msg, long when)
```

#### next
**取出**一条消息并且把这条消息从消息队列中移除
```java
Message next()
```
```java
/* MessageQueue源码 */
Message next() {
    ...
    for (;;) {
        ...
    }
}
```
可以看出，**next函数是一个无限循环的函数**，如果消息队列中没有消息，那么next函数会一直阻塞在这里，直到有消息过来。





### Looper
#### 子线程创建Handler
```java
//如下代码会报异常：java.lang.RuntimeException: Can't create handler inside thread that has not called Looper.prepare()
new Thread(new Runnable() {    
    @Override
    public void run() {        
        Handler handler = new Handler();
        handler.sendEmptyMessageDelayed(0,1000);
    }
}).start();
```
我们在主线程中使用Handler并没有问题，但如上代码在**子线程**里面创建了一个**handler**，使用这个handler去处理事情，就会出现**异常**：`java.lang.RuntimeException: Can't create handler inside thread that has not called Looper.prepare()`。因为**Handler的消息是由Looper进行处理**的，但是你并没有创建Looper对象，所以会产生这个异常。
在主线程中使用了Handler没问题，是因为在**主线程**中，**系统**会**默认**为我们**创建一个Looper对象**。


上面的代码**如何解决**呢？我们创建一个Looper并调用loop即可。
```java
//子线程中创建Handler正确方式
new Thread(new Runnable() {    
    @Override
    public void run() {        
        Looper.prepare();//创建Looper
        Handler handler = new Handler();
        handler.sendEmptyMessageDelayed(0,1000);
        Looper.loop();//一定要调用loop()方法,否则我们只是单单创建了一个Looper
    }
}).start();
```

#### parpare
**创建looper对象**并不是使用Looper loop = new Looper()，而是直接调用函数**`Looper.parpare()`**即可。我们跟踪这个函数看看：
```java
/* Looper.parpare() */
public static void prepare() {
    prepare(true);
}
private static void prepare(boolean quitAllowed) {    
    if (sThreadLocal.get() != null) {        
        throw new RuntimeException("Only one Looper may be created per thread");
    }
    sThreadLocal.set(new Looper(quitAllowed));
}
private Looper(boolean quitAllowed) {    
    //在这里创建了一个消息队列
    mQueue = new MessageQueue(quitAllowed);
    mThread = Thread.currentThread();
}
```

在**Looper构造函数里**面Looper会**创建一个MessageQueue**对象。

#### loop
**Looper**类中的函数**`loop()`**不断的**调用MessageQueue**的**`next()`**函数来查看**是否有新消息**，如果**有**马上去**处理**这个**消息**。
既然MessageQueue的**next()**函数有**可能**会**堵塞**，所以这就导致了**loop()**函数**堵塞**。
```java
/* Looper.parpare() */
public static void loop() {
    ...
    for (;;) {
        Message msg = queue.next(); // might block
        if (msg == null) {
            // No message indicates that the message queue is quitting.
            return;
        }
        ...
            msg.target.dispatchMessage(msg);//处理消息
        ...
    }
}
```
看loop函数的源码可知，当MessageQueue的next返回新消息,**Looper**就会**处理**这条**消息** : **`msg.target.dispatchMessage(msg);`**
这里的**msg.target就是发送这条消息的Handler**，这样子Handler发送的消息最终又要交给它的dispatchMessage函数来处理。

接下来我们就分析一下Handler的内部原理。
### Handler
一般,我们通过Handler的**post**或者**send**的一系列函数来发送消息，但是其实不管是send还是post，**最终都是通过send**函数来实现的。
**send**系列函数有很多个，但是**最终**都是**调用`sendMessageAtTime`**(Message msg, long uptimeMillis)
```java
/* Handler源码 */
//post最终都是调用send
public final boolean post(Runnable r){
   return  sendMessageDelayed(getPostMessage(r), 0);
}
public final boolean postAtTime(Runnable r, long uptimeMillis){
    return sendMessageAtTime(getPostMessage(r), uptimeMillis);
}
public final boolean postAtTime(Runnable r, Object token, long uptimeMillis){
    return sendMessageAtTime(getPostMessage(r, token), uptimeMillis);
}

public final boolean postDelayed(Runnable r, long delayMillis){
    return sendMessageDelayed(getPostMessage(r), delayMillis);
}
```
```java
public final boolean sendMessage(Message msg){
    return sendMessageDelayed(msg, 0);
}
public final boolean sendEmptyMessage(int what){
    return sendEmptyMessageDelayed(what, 0);
}
public final boolean sendEmptyMessageDelayed(int what, long delayMillis) {
    Message msg = Message.obtain();
    msg.what = what;
    return sendMessageDelayed(msg, delayMillis);
}
public final boolean sendEmptyMessageAtTime(int what, long uptimeMillis) {
    Message msg = Message.obtain();
    msg.what = what;
    return sendMessageAtTime(msg, uptimeMillis);
}
public final boolean sendMessageDelayed(Message msg, long delayMillis){
    if (delayMillis < 0) { delayMillis = 0; }
    return sendMessageAtTime(msg, SystemClock.uptimeMillis() + delayMillis);
}
//最终都是调用这个方法
public boolean sendMessageAtTime(Message msg, long uptimeMillis) {
	...
}
```

#### sendMessageAtTime
```java
/* Handler源码 */
public boolean sendMessageAtTime(Message msg, long uptimeMillis) {
    MessageQueue queue = mQueue;
    if (queue == null) {
        RuntimeException e = new RuntimeException(
                this + " sendMessageAtTime() called with no mQueue");
        Log.w("Looper", e.getMessage(), e);
        return false;
    }
    //关键在这里
    return enqueueMessage(queue, msg, uptimeMillis);
}
private boolean enqueueMessage(MessageQueue queue, Message msg, long uptimeMillis) {
    msg.target = this;//证实了msg.target就是发送条消息的Handler
    if (mAsynchronous) {
        msg.setAsynchronous(true);
    }
    return queue.enqueueMessage(msg, uptimeMillis);//往消息队列里面插入一条消息
}
```

如上源码可知，**send**函数**其实就是往消息队列**里面**插入**一条**消息**。

同时**Looper的loop函数又会调用MessageQueue的next函数去获取消息**，**最后在交给Handler的dispatchMessage函数处理**。
这样子我们就把Handler, MessageQueue, Looper三者串起来了。

那么接下来我们赶紧来看看dispatchMessage是什么样子的。
#### dispatchMessage
```java
/* Handler源码 */
public void dispatchMessage(Message msg) {    
    if (msg.callback != null) {//msg.callback
        handleCallback(msg);//①
    } else {        
        if (mCallback != null) {//mCallback
            if (mCallback.handleMessage(msg)) {//②
                return;
            }
        }
        handleMessage(msg);//③
    }
}
```

这里有两个callBack，我们一一来分析一下是什么意思。
##### ① msg.callback
msg.callback是你在调用Handler.post....()函数时候创建出来的, **msg.callback就是这里的runnbale匿名类**:
```java
handler.postDelayed(new Runnable() {//postDelayed具体实现见下面源码
    @Override
    public void run() {      
        //.....
    }
}, 1000);
```
```java
/* Handler源码 */
public final boolean postDelayed(Runnable r, long delayMillis){
    return sendMessageDelayed(getPostMessage(r), delayMillis);//getPostMessage
}
private static Message getPostMessage(Runnable r) {
    Message m = Message.obtain();
    m.callback = r;//msg.callback就是postDelayed传进来的runnbale匿名类
    return m;
}
```

##### ① handleCallback(msg)
由下源码可知，**handleCallback(msg)就是去执行runnbale中的run函数**。没错,这里**将开启一个线程**。
所以我们就明白了最开始我们提到的使用Handler处理耗时操作和UI操作的原理了。
```java
/* Handler源码 */
private static void handleCallback(Message message) {
    message.callback.run();
}
```

那么，如果你是调用handler.sendMessage()函数的话，callBack为null。

##### ② mCallback
如下，由Handler的构造函数可知，如果**mCallback**是在**创建Handler时传递进来callback**。
```java
/* Handler源码 */
public Handler(boolean async) { this(null, async); }
public Handler(Callback callback, boolean async) {
    ...
    mLooper = Looper.myLooper();
    if (mLooper == null) {
        throw new RuntimeException(
            "Can't create handler inside thread that has not called Looper.prepare()");
    }
    mQueue = mLooper.mQueue;
    mCallback = callback;//mCallback = 创建Handler时传递进来callback
    mAsynchronous = async;
}
```

##### ② mCallback.handleMessage(msg)
追溯源码，发现Callback只是一个接口:
```java
/* Handler源码 */
public interface Callback {    
    public boolean handleMessage(Message msg);
}
```

于是我们应该就会想起我们在创建Handler的时候有时候会写这样子的代码:
```
private Handler mhandler=new Handler(new Handler.Callback() {
    @Override
    //mCallback.handleMessage(msg)其实调用的就是这个方法
    public boolean handleMessage(Message msg) {
        ...//这个时候Handler的dispatchMessage就会来执行这里的函数逻辑
        return true;
    }
});
```

##### ③ handleMessage(msg)
当dispatchMessage做完一系列判断，走到了handleMessage(msg)这一步，调用的是Handler自己handleMessage方法。
```java
/* Handler源码 */
public void handleMessage(Message msg) {
	//空实现，让子类实现
}
```

让我们想起了**创建Handler常用写法**：
```java
Handler handler = new Handler(){    
    @Override
    public void handleMessage(Message msg) {
        ...//这个时候Handler的dispatchMessage就会来执行这里的函数逻辑
    }
};
```


### 总结
最后用一张图，来表示整个消息机制

![handler_java](http://upload-images.jianshu.io/upload_images/9028834-b4040b28d6b7cae1..jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**图解：**

- Handler通过sendMessage()发送Message到MessageQueue队列；
- Looper通过loop()，不断提取出达到触发条件的Message，并将Message交给target来处理；
- 经过dispatchMessage()后，交回给Handler的handleMessage()来进行相应地处理。
- 将Message加入MessageQueue时，处往管道写入字符，可以会唤醒loop线程；如果MessageQueue中没有Message，并处于Idle状态，则会执行IdelHandler接口中的方法，往往用于做一些清理性地工作。

**消息分发的优先级：**

1. Message的回调方法：`message.callback.run()`，优先级最高；
2. Handler的回调方法：`Handler.mCallback.handleMessage(msg)`，优先级仅次于1；
3. Handler的默认方法：`Handler.handleMessage(msg)`，优先级最低。

### 架构图
![handler_java](http://upload-images.jianshu.io/upload_images/9028834-82ef8abde6366e0f..jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- **Looper**有一个MessageQueue消息队列；
- **MessageQueue**有一组待处理的Message；
- **Message**中有一个用于处理消息的Handler；
- **Handler**中有Looper和MessageQueue。


# 源码分析
## 主线程默认创建Looper对象
**应用程序的入口**是在**ActivityThread**的**main**方法中的（当应用程序启动的时候，会通过底层的C/C++去调用main方法），这个方法在ActivityThread类的最后一个函数里面，核心代码如下：
```java
/* ActivityThread源码 */
public static void main(String[] args) {
    ...
    Environment.initForCurrentUser();
    ...
    Looper.prepareMainLooper();//Looper
    ...
    Looper.loop();//Looper
    ...
}
```
```
/* Looper源码 */
public static void prepareMainLooper() {    
    prepare(false);//false意思不允许我们程序员退出（面向我们开发者），因为这是在主线程里面
    synchronized (Looper.class) {
        if (sMainLooper != null) {//每个线程只允许执行一次
            throw new IllegalStateException("The main Looper has already been prepared.");
        }        
        sMainLooper = myLooper();//将当前的Looper保存为主Looper
    }
}
```


在main方法里面，首先初始化了我们的Environment对象，然后创建了Looper，然后开启消息循环。
根据我们的常识知道，如果程序没有死循环的话，执行完main函数（比如构建视图等等代码）以后就会立马退出了。**之所以**我们**的APP能够一直运行**着，就是**因为Looper.loop()里面是一个死循环**：
```java
public static void loop() {
    for (;;) {
    }
}
```
>之所以用for (;;)而不是用while(true)是因为防止一些人通过黑科技去修改这个循环的标志（比如通过反射的方式）。
>在分析源码的时候，你可能会发现一些if(false){}之类的语句，这种写法是方便调试的，通过一个标志就可以控制某些代码是否执行，比如说是否输出一些系统的Log。
>看源码一定不要慌，也不要一行一行看，要抓住核心的思路去看即可。


## 系统的Handler
在ActivityThread的成员变量里面有一个这样的大**H**（继承Handler），这个就是**系统的Handler**：
```java
final H mH = new H();
```

回顾一下ActivityThread的main方法可以知道，在new ActivityThread的时候，系统的Handler就就初始化了，这是一种饿加载的方法，也就是在类被new的时候就初始化成员变量了。
```java
/* ActivityThread源码 */
public static void main(String[] args) {
    ...
    Environment.initForCurrentUser();
    ...
    Looper.prepareMainLooper();
    
    ActivityThread thread = new ActivityThread();
    thread.attach(false);
    if (sMainThreadHandler == null) {
        sMainThreadHandler = thread.getHandler();//初始化系统的Handler
    }
    ...
    Looper.loop();
    ...
}
final Handler getHandler() {
    return mH;
}
final H mH = new H();
```
### H
下面看系统Handler的定义（看的时候可以跳过一些case，粗略地看即可）：
从系统的Handler中，在**handleMessage**我们可以看到很多关于**四大组件的生命周期操作**，比如创建、销毁、切换、跨进程通信，也包括了整个**Application进程**的销毁等等。
```java
/* ActivityThread源码 */
private class H extends Handler {
	...
    public void handleMessage(Message msg) {
        switch (msg.what) {
            case LAUNCH_ACTIVITY: { ... } break;
            case RELAUNCH_ACTIVITY: { ... } break;
            case PAUSE_ACTIVITY: { ... } break;
            case PAUSE_ACTIVITY_FINISHING: { ... } break;
            case STOP_ACTIVITY_SHOW: { ... } break;
            case STOP_ACTIVITY_HIDE: { ... } break;
            case SHOW_WINDOW: { ... } break;
            case HIDE_WINDOW: { ... } break;
            case RESUME_ACTIVITY: { ... } break;
            case SEND_RESULT: { ... } break;
            case DESTROY_ACTIVITY: { ... } break;
            case BIND_APPLICATION: { ... } break;
            case EXIT_APPLICATION://应用程序的退出过程
                if (mInitialApplication != null) {
                    mInitialApplication.onTerminate();
                }
                Looper.myLooper().quit();
                break;
            case NEW_INTENT: { ... } break;
            case RECEIVER: { ... } break;
            case CREATE_SERVICE: { ... } break;
            case BIND_SERVICE: { ... } break;
            case UNBIND_SERVICE: { ... } break;
            case SERVICE_ARGS: { ... } break;
            case STOP_SERVICE: { ... } break;
            case CONFIGURATION_CHANGED: { ... } break;
            case CLEAN_UP_CONTEXT: { ... } break;
            case GC_WHEN_IDLE: { ... } break;
            case DUMP_SERVICE: { ... } break;
            case LOW_MEMORY: { ... } break;
            case ACTIVITY_CONFIGURATION_CHANGED: { ... } break;
            case PROFILER_CONTROL: { ... } break;
            case CREATE_BACKUP_AGENT: { ... } break;
            case DESTROY_BACKUP_AGENT: { ... } break;
            case SUICIDE://我们可以通过发SUICIDE消息自杀，这样来退出应用程序
                Process.killProcess(Process.myPid());
                break;
            case REMOVE_PROVIDER: { ... } break;
            case ENABLE_JIT: { ... } break;
            case DISPATCH_PACKAGE_BROADCAST: { ... } break;
            case SCHEDULE_CRASH: { ... } break;
            case DUMP_HEAP: { ... } break;
            case DUMP_ACTIVITY: { ... } break;
            case DUMP_PROVIDER: { ... } break;
            case SLEEPING: { ... } break;
            case SET_CORE_SETTINGS: { ... } break;
            case UPDATE_PACKAGE_COMPATIBILITY_INFO: { ... } break;
            case TRIM_MEMORY: { ... } break;
            case UNSTABLE_PROVIDER_DIED: { ... } break;
            case REQUEST_ASSIST_CONTEXT_EXTRAS: { ... } break;
            case TRANSLUCENT_CONVERSION_COMPLETE: { ... } break;
            case INSTALL_PROVIDER: { ... } break;
            case ON_NEW_ACTIVITY_OPTIONS: { ... } break;
            case CANCEL_VISIBLE_BEHIND: { ... } break;
            case BACKGROUND_VISIBLE_BEHIND_CHANGED: { ... } break;
            case ENTER_ANIMATION_COMPLETE: { ... } break;
            case START_BINDER_TRACKING: { ... } break;
            case STOP_BINDER_TRACKING_AND_DUMP: { ... } break;
            case MULTI_WINDOW_MODE_CHANGED: { ... } break;
            case PICTURE_IN_PICTURE_MODE_CHANGED: { ... } break;
            case LOCAL_VOICE_INTERACTION_STARTED: { ... } break;
        }
        Object obj = msg.obj;
        if (obj instanceof SomeArgs) {
            ((SomeArgs) obj).recycle();
        }
        if (DEBUG_MESSAGES) Slog.v(TAG, "<<< done: " + codeToString(msg.what));
    }
	...
}
```
### 应用程序的退出过程
实际上我们要**退出应用程序**的话，就是让主线程结束，换句话说就是要**让Looper的循环结束**。
这里是直接结束Looper循环，因此我们四大组件的生命周期方法可能就不会执行了，因为四大组件的生命周期方法就是通过Handler去处理的，Looper循环都没有了，四大组件还玩毛线！因此我们平常写程序的时候就要注意了，onDestroy方法是不一定能够回调的。
```java
/* ActivityThread源码 */
//H的handleMessage中部分代码
case EXIT_APPLICATION:
    if (mInitialApplication != null) {
        mInitialApplication.onTerminate();
    }
    //退出Looper的循环
    Looper.myLooper().quit();
    break;
```

这里实际上是调用了MessageQueue的quit，清空所有Message。
```java
/* Looper源码 */
public void quit() {
    mQueue.quit(false);
}
```

## 1、Looper

### 1.1 prepare()

对于无参的情况，默认调用`prepare(true)`，表示的是这个Looper运行退出，而对于false的情况则表示当前Looper不运行退出。
```java
/* Looper源码 */
private static void prepare(boolean quitAllowed) {
    //每个线程只允许执行一次该方法，第二次执行时线程的TLS已有数据，则会抛出异常。
    if (sThreadLocal.get() != null) {
        throw new RuntimeException("Only one Looper may be created per thread");
    }
    //创建Looper对象，并保存到当前线程的TLS区域
    sThreadLocal.set(new Looper(quitAllowed));
}
//Looper的成员变量sThreadLocal
static final ThreadLocal<Looper> sThreadLocal = new ThreadLocal<Looper>();
```
#### ThreadLocal
这里的`sThreadLocal`是ThreadLocal类型，下面，先说说ThreadLocal。

**ThreadLocal**： **线程本地存储区**（Thread Local Storage，简称为TLS），每个线程都有自己的**私有**的本地存储区域，不同线程之间彼此不能访问对方的TLS区域。
ThreadLocal是JDK提供的一个**解决线程不安全**的类，线程不安全问题归根结底主要涉及到变量的多线程访问问题，例如变量的临界问题、值错误、并发问题等。这里利用ThreadLocal绑定了Looper以及线程，就可以避免其他线程去访问当前线程的Looper了。

TLS常用的操作方法：
- **`ThreadLocal.set(T value)`**：**将value存储到当前线程的TLS区域**，源码如下：
  map.set(this, value)：通过把**键-ThreadLocal自身**&**值-Looper** 放到了一个Map里面，如果再放一个的话，就会覆盖（因为map不允许键值对中的键是重复的），因此ThreadLocal**绑定了线程以及Looper**。
  ```java
  /* ThreadLocal源码 */
  public void set(T value) {
      Thread t = Thread.currentThread();//获取当前线程
      ThreadLocalMap map = getMap(t);
      if (map != null)
          map.set(this, value);//把键-ThreadLocal自身&值-Looper 放到了一个Map里面
      else
          createMap(t, value);
  }
  ThreadLocalMap getMap(Thread t) {
  	return t.threadLocals;
  }
  void createMap(Thread t, T firstValue) {
      t.threadLocals = new ThreadLocalMap(this, firstValue);
  }
  ```

  ```java
  /* Thread源码 */
  ThreadLocal.ThreadLocalMap threadLocals = null;
  ```



- **`ThreadLocal.get()`**：**获取当前线程TLS区域的数据**，源码如下：

  ```java
    public T get() {
        Thread t = Thread.currentThread();//获取当前线程
        ThreadLocalMap map = getMap(t);
        if (map != null) {
            ThreadLocalMap.Entry e = map.getEntry(this);
            if (e != null)
                return (T)e.value;//返回当前线程储存区中的数据
        }
        return setInitialValue();//目标线程存储区没有查询到则返回null
    }
    private T setInitialValue() {
        T value = initialValue();
        Thread t = Thread.currentThread();
        ThreadLocalMap map = getMap(t);
        if (map != null)
            map.set(this, value);
        else
            createMap(t, value);
        return value;
    }
    protected T initialValue() {
        return null;
    }
  ```

ThreadLocal的get()和set()方法操作的类型都是泛型，接着回到前面提到的`sThreadLocal`变量，其定义如下：
```java
static final ThreadLocal<Looper> sThreadLocal = new ThreadLocal<Looper>();
```

可见`sThreadLocal`的get()和set()操作的类型都是`Looper`类型。

**Looper.prepare()**
Looper.prepare()在**每个线程只允许执行一次**，该方法会创建Looper对象，Looper的构造方法中会创建一个MessageQueue对象，再将Looper对象保存到当前线程TLS。

对于**Looper**类型的**构造方法**如下：
```java
private Looper(boolean quitAllowed) {
    mQueue = new MessageQueue(quitAllowed);  //创建MessageQueue对象. //【见3.1】
    mThread = Thread.currentThread();  //记录当前线程.
}
```

另外，与prepare()相近功能的，还有一个**`prepareMainLooper()`**方法，该方法主要在ActivityThread类中使用。
```java
public static void prepareMainLooper() {
    prepare(false); //设置不允许退出的Looper
    synchronized (Looper.class) {
        if (sMainLooper != null) {//每个线程只允许执行一次
            throw new IllegalStateException("The main Looper has already been prepared.");
        }
        sMainLooper = myLooper();//将当前的Looper保存为主Looper
    }
}
public static @Nullable Looper myLooper() {
    return sThreadLocal.get();
}
static final ThreadLocal<Looper> sThreadLocal = new ThreadLocal<Looper>();
```




### 1.2 loop()
loop()进入循环模式，不断重复下面的操作，直到没有消息时退出循环
- 读取MessageQueue的下一条Message；
- 把Message分发给相应的target；
- 再把分发后的Message回收到消息池，以便重复利用。

这是这个消息处理的核心部分。

另外，源码中可以看到有logging方法，这是用于debug的，默认情况下`logging == null`，通过设置setMessageLogging()用来开启debug工作。
```java
/* Looper源码 */
public static void loop() {
    final Looper me = myLooper();  //获取TLS存储的Looper对象 //【见1.4】
    if (me == null) {
        throw new RuntimeException("No Looper; Looper.prepare() wasn't called on this thread.");
    }
    final MessageQueue queue = me.mQueue;  //获取Looper对象中的消息队列

    Binder.clearCallingIdentity();
    //确保在权限检查时基于本地进程，而不是基于最初调用进程。
    final long ident = Binder.clearCallingIdentity();

    for (;;) { //进入loop的主循环方法
        Message msg = queue.next(); //可能会阻塞 //【见3.2】
        if (msg == null) { //没有消息，则退出循环
            return;
        }

        Printer logging = me.mLogging;  //默认为null，可通过setMessageLogging()方法来指定输出，用于debug功能
        if (logging != null) {
            logging.println(">>>>> Dispatching to " + msg.target + " " +
                    msg.callback + ": " + msg.what);
        }
        final long traceTag = me.mTraceTag;
        if (traceTag != 0) {
            Trace.traceBegin(traceTag, msg.target.getTraceName(msg));
        }
        try {
            msg.target.dispatchMessage(msg);//用于分发Message //【见2.2】
        } finally {
            if (traceTag != 0) {
                Trace.traceEnd(traceTag);
            }
        }
        if (logging != null) {
            logging.println("<<<<< Finished to " + msg.target + " " + msg.callback);
        }

        final long newIdent = Binder.clearCallingIdentity(); //确保分发过程中identity不会损坏
        if (ident != newIdent) {
             //打印identity改变的log，在分发消息过程中是不希望身份被改变的。
        }
        msg.recycleUnchecked();  //回收消息，将Message放入消息池 //【见4.2】
    }
}
```

### 1.3 quit()
```java
/* Looper源码 */
public void quit() {
    mQueue.quit(false); //消息移除
}

public void quitSafely() {
    mQueue.quit(true); //安全地消息移除
}
```

Looper.quit()方法的实现最终调用的是MessageQueue.quit()方法

**MessageQueue.quit()**
消息退出的方式：
- 当safe =true时，只移除尚未触发的所有消息，对于正在触发的消息并不移除；
- 当safe =flase时，移除所有的消息
```java
/* MessageQueue源码 */
void quit(boolean safe) {        
        if (!mQuitAllowed) {// 当mQuitAllowed为false，表示不运行退出，强行调用quit()会抛出异常
            throw new IllegalStateException("Main thread not allowed to quit.");
        }
        synchronized (this) {
            if (mQuitting) { //防止多次执行退出操作
                return;
            }
            mQuitting = true;
            if (safe) {
                removeAllFutureMessagesLocked(); //移除尚未触发的所有消息
            } else {
                removeAllMessagesLocked(); //移除所有的消息
            }
            //如mQuitting=false，那么认定为 mPtr != 0
            nativeWake(mPtr);
        }
    }
```
### 1.4 常用方法

#### 1.4.1 myLooper[](http://gityuan.com/2015/12/26/handler-message-framework/#241-mylooper)

用于获取TLS存储的Looper对象
```java
public static @Nullable Looper myLooper() {
        return sThreadLocal.get();
    }
```


## 2、Handle

### 2.1 创建Handler

#### 2.1.1 无参构造
```java
public Handler() {
    this(null, false);
}

public Handler(Callback callback, boolean async) {
    //匿名类、内部类或本地类都必须申明为static，否则会警告可能出现内存泄露
    if (FIND_POTENTIAL_LEAKS) {
        final Class<? extends Handler> klass = getClass();
        if ((klass.isAnonymousClass() || klass.isMemberClass() || klass.isLocalClass()) &&
                (klass.getModifiers() & Modifier.STATIC) == 0) {
            Log.w(TAG, "The following Handler class should be static or leaks might occur: " +
                klass.getCanonicalName());
        }
    }
    //必须先执行Looper.prepare()，才能获取Looper对象，否则为null.
    mLooper = Looper.myLooper();  //从当前线程的TLS中获取Looper对象//【见1.1】
    if (mLooper == null) {
        throw new RuntimeException(
                "Can't create handler inside thread that has not called Looper.prepare()");
    }
    mQueue = mLooper.mQueue; //消息队列，来自Looper对象
    mCallback = callback;  //回调方法
    mAsynchronous = async; //设置消息是否为异步处理方式
}
```

对于Handler的无参构造方法，默认采用当前线程TLS中的Looper对象，并且callback回调方法为null，且消息为同步处理方式。只要执行的Looper.prepare()方法，那么便可以获取有效的Looper对象。

#### 2.1.2 有参构造
```java
public Handler(Looper looper) {
    this(looper, null, false);
}

public Handler(Looper looper, Callback callback, boolean async) {
    mLooper = looper;
    mQueue = looper.mQueue;
    mCallback = callback;
    mAsynchronous = async;
}
```

Handler类在构造方法中，可指定Looper，Callback回调方法以及消息的处理方式(同步或异步)，对于无参的handler，默认是当前线程的Looper。

### 2.2 消息分发
在Looper.loop()中，当发现有消息时，调用消息的目标handler，执行dispatchMessage()方法来分发消息。

**分发消息流程：**

1. 当`Message`的回调方法不为空时，则回调方法`msg.callback.run()`，其中callBack数据类型为Runnable,否则进入步骤2；
2. 当`Handler`的`mCallback`成员变量不为空时，则回调方法`mCallback.handleMessage(msg)`,否则进入步骤3；
3. 调用`Handler`自身的回调方法`handleMessage()`，该方法默认为空，Handler子类通过覆写该方法来完成具体的逻辑。

对于很多情况下，消息分发后的处理方法是第3种情况，即Handler.handleMessage()，一般地往往通过覆写该方法从而实现自己的业务逻辑。
```java
/* Handler源码 */
public void dispatchMessage(Message msg) {
    if (msg.callback != null) {
        //当Message存在回调方法，回调msg.callback.run()方法；
        handleCallback(msg);
    } else {
        if (mCallback != null) {
            //当Handler存在Callback成员变量时，回调方法handleMessage()；
            if (mCallback.handleMessage(msg)) {
                return;
            }
        }
        //Handler自身的回调方法handleMessage()
        handleMessage(msg);
    }
}
```

### 2.3 消息发送
发送消息调用链：
![java_sendmessage](http://upload-images.jianshu.io/upload_images/9028834-4ad8aa65e85502e6..png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

从上图，可以发现所有的发消息方式，最终都是调用`MessageQueue.enqueueMessage()`;
`Handler.sendEmptyMessage()`等系列方法最终调用`MessageQueue.enqueueMessage(msg, uptimeMillis)`，将消息添加到消息队列中，其中uptimeMillis为系统当前的运行时间，不包括休眠时间。

#### 2.3.1 sendEmptyMessage
```java
public final boolean sendEmptyMessage(int what) {
    return sendEmptyMessageDelayed(what, 0);
}
```

#### 2.3.2 sendEmptyMessageDelayed
```java
public final boolean sendEmptyMessageDelayed(int what, long delayMillis) {
    Message msg = Message.obtain();
    msg.what = what;
    return sendMessageDelayed(msg, delayMillis);
}
```

#### 2.3.3 sendMessageDelayed
```java
public final boolean sendMessageDelayed(Message msg, long delayMillis) {
    if (delayMillis < 0) {
        delayMillis = 0;
    }
    return sendMessageAtTime(msg, SystemClock.uptimeMillis() + delayMillis);
}
```

#### 2.3.4 sendMessageAtTime
```java
public boolean sendMessageAtTime(Message msg, long uptimeMillis) {
    MessageQueue queue = mQueue;
    if (queue == null) {
        return false;
    }
    return enqueueMessage(queue, msg, uptimeMillis);
}
```

#### 2.3.5 sendMessageAtFrontOfQueue
该方法通过设置消息的触发时间为0，从而使Message加入到消息队列的队头。
```java
public final boolean sendMessageAtFrontOfQueue(Message msg) {
    MessageQueue queue = mQueue;
    if (queue == null) {
        return false;
    }
    return enqueueMessage(queue, msg, 0);
}
```

#### 2.3.6 post
```java
public final boolean post(Runnable r) {
   return  sendMessageDelayed(getPostMessage(r), 0);
}

private static Message getPostMessage(Runnable r) {
    Message m = Message.obtain();
    m.callback = r;
    return m;
}
```

#### 2.3.7 postAtFrontOfQueue
```java
public final boolean postAtFrontOfQueue(Runnable r) {
    return sendMessageAtFrontOfQueue(getPostMessage(r));
}
```

#### 2.3.8 enqueueMessage
```java
private boolean enqueueMessage(MessageQueue queue, Message msg, long uptimeMillis) {
    msg.target = this;
    if (mAsynchronous) {
        msg.setAsynchronous(true);
    }
    return queue.enqueueMessage(msg, uptimeMillis); //【见3.3】
}
```


### 2.4 其他方法
`Handler`是消息机制中非常重要的辅助类，更多的实现都是`MessageQueue`, `Message`中的方法，Handler的目的是为了更加方便的使用消息机制。

#### 2.4.1 obtainMessage

获取消息
```java
public final Message obtainMessage() {
    return Message.obtain(this); //【见4.2】
}
```

`Handler.obtainMessage()`方法，最终调用`Message.obtainMessage(this)`，其中this为当前的Handler对象。

#### 2.4.2 removeMessages
```java
public final void removeMessages(int what) {
    mQueue.removeMessages(this, what, null); //【见 3.5】
}
```


## 3、MessageQueue

MessageQueue是消息机制的**Java层和C++层的连接纽带**，大部分**核心方法都交给native层来处理**，其中MessageQueue类中涉及的native方法如下：
```java
private native static long nativeInit();
private native static void nativeDestroy(long ptr);
private native void nativePollOnce(long ptr, int timeoutMillis);
private native static void nativeWake(long ptr);
private native static boolean nativeIsPolling(long ptr);
private native static void nativeSetFileDescriptorEvents(long ptr, int fd, int events);
```

关于这些native方法的介绍，见[Android消息机制2-Handler(native篇)](http://gityuan.com/2015/12/27/handler-message-native/)。

### 3.1 创建MessageQueue
```java
MessageQueue(boolean quitAllowed) {
    mQuitAllowed = quitAllowed;
    //通过native方法初始化消息队列，其中mPtr是供native代码使用
    mPtr = nativeInit();
}
```

### 3.2 next()
提取下一条message。
消息的取出并不是直接就从队列的头部取出的，而是根据了消息的when时间参数有关的，因为我们可以发送延时消息、也可以发送一个指定时间点的消息。

`nativePollOnce`是阻塞操作，其中`nextPollTimeoutMillis`代表下一个消息到来前，还需要等待的时长；当nextPollTimeoutMillis = -1时，表示消息队列中无消息，会一直等待下去。

当处于空闲时，往往会执行`IdleHandler`中的方法。当nativePollOnce()返回后，next()从`mMessages`中提取一个消息。

`nativePollOnce()`在native做了大量的工作，想进一步了解可查看 [Android消息机制2-Handler(native篇)](http://gityuan.com/2015/12/27/handler-message-native/#nativepollonce)。

从源码可以看到消息的取出用到了一些native方法，这样做是为了获得更高的效率，
```java
/* MessageQueue源码 */
Message next() {
    final long ptr = mPtr;
    if (ptr == 0) { //当消息循环已经退出，则直接返回
        return null;
    }
    int pendingIdleHandlerCount = -1; // 循环迭代的首次为-1
    int nextPollTimeoutMillis = 0;
    for (; ; ) {
        if (nextPollTimeoutMillis != 0) {
            Binder.flushPendingCommands();
        }
        //阻塞操作，当等待nextPollTimeoutMillis时长，或者消息队列被唤醒，都会返回
        nativePollOnce(ptr, nextPollTimeoutMillis);//nativePollOnce
        synchronized (this) {
            final long now = SystemClock.uptimeMillis();//拿到当前的时间戳
            Message prevMsg = null;
            Message msg = mMessages;
            if (msg != null && msg.target == null) {
                //当消息Handler为空时，查询MessageQueue中的下一条异步消息msg，则退出循环。
                do {
                    prevMsg = msg;
                    msg = msg.next;
                } while (msg != null && !msg.isAsynchronous());
            }
            if (msg != null) {
                if (now < msg.when) {
                //当异步消息触发时间大于当前时间（还没有到执行的时间），则设置下一次轮询的超时时长
                    nextPollTimeoutMillis = (int) Math.min(msg.when - now, Integer.MAX_VALUE);
                } else {
                    // 到了执行时间，获取一条消息，并返回
                    mBlocked = false;
                    if (prevMsg != null) {
                        prevMsg.next = msg.next;
                    } else {
                        mMessages = msg.next;
                    }
                    msg.next = null;
                    //设置消息的使用状态，即flags |= FLAG_IN_USE
                    msg.markInUse();
                    return msg;   //成功地获取MessageQueue中的下一条即将要执行的消息
                }
            } else {
                //没有消息
                nextPollTimeoutMillis = -1;
            }
            //消息正在退出，返回null
            if (mQuitting) {
                dispose();
                return null;
            }
            //当消息队列为空，或者是消息队列的第一个消息时
            if (pendingIdleHandlerCount < 0 && (mMessages == null || now < mMessages.when)) {
                pendingIdleHandlerCount = mIdleHandlers.size();
            }
            if (pendingIdleHandlerCount <= 0) {
                //没有idle handlers 需要运行，则循环并等待。
                mBlocked = true;
                continue;
            }
            if (mPendingIdleHandlers == null) {
                mPendingIdleHandlers = new IdleHandler[Math.max(pendingIdleHandlerCount, 4)];
            }
            mPendingIdleHandlers = mIdleHandlers.toArray(mPendingIdleHandlers);
        }
        //只有第一次循环时，会运行idle handlers，执行完成后，重置pendingIdleHandlerCount为0.
        for (int i = 0; i < pendingIdleHandlerCount; i++) {
            final IdleHandler idler = mPendingIdleHandlers[i];
            mPendingIdleHandlers[i] = null; //去掉handler的引用
            boolean keep = false;
            try {
                keep = idler.queueIdle();  //idle时执行的方法
            } catch (Throwable t) {
                Log.wtf(TAG, "IdleHandler threw exception", t);
            }
            if (!keep) {
                synchronized (this) {
                    mIdleHandlers.remove(idler);
                }
            }
        }
        //重置idle handler个数为0，以保证不会再次重复运行
        pendingIdleHandlerCount = 0;
        //当调用一个空闲handler时，一个新message能够被分发，因此无需等待可以直接查询pending message.
        nextPollTimeoutMillis = 0;
    }
}
```



### 3.3 enqueueMessage
添加一条消息到消息队列
`MessageQueue`是按照Message触发时间的先后顺序排列的，队头的消息是将要最早触发的消息。当有消息需要加入消息队列时，会从队列头开始遍历，直到找到消息应该插入的合适位置，以保证所有消息的时间顺序。
```java
boolean enqueueMessage(Message msg, long when) {
    // 每一个普通Message必须有一个target（即Handler）
    if (msg.target == null) {
        throw new IllegalArgumentException("Message must have a target.");
    }
    if (msg.isInUse()) {
        throw new IllegalStateException(msg + " This message is already in use.");
    }
    synchronized (this) {
        if (mQuitting) {  //正在退出时，回收msg，加入到消息池
            msg.recycle();
            return false;
        }
        msg.markInUse();
        msg.when = when;
        Message p = mMessages;
        boolean needWake;
        if (p == null || when == 0 || when < p.when) {
            //p为null(代表MessageQueue没有消息） 或者msg的触发时间是队列中最早的， 则进入该该分支
            msg.next = p;
            mMessages = msg;
            needWake = mBlocked; //当阻塞时需要唤醒
        } else {
            //将消息按时间顺序插入到MessageQueue。一般地，不需要唤醒事件队列，
            //除非消息队头存在barrier，并且同时Message是队列中最早的异步消息。
            needWake = mBlocked && p.target == null && msg.isAsynchronous();
            Message prev;
            for (;;) {
                prev = p;
                p = p.next;
                if (p == null || when < p.when) {
                    break;
                }
                if (needWake && p.isAsynchronous()) {
                    needWake = false;
                }
            }
            msg.next = p;
            prev.next = msg;
        }
        //消息没有退出，我们认为此时mPtr != 0
        if (needWake) {
            nativeWake(mPtr);
        }
    }
    return true;
}
```



### 3.4 removeMessages
这个移除消息的方法，采用了两个while循环，第一个循环是从队头开始，移除符合条件的消息，第二个循环是从头部移除完连续的满足条件的消息之后，再从队列后面继续查询是否有满足条件的消息需要被移除。
```java
void removeMessages(Handler h, int what, Object object) {
    if (h == null) {
        return;
    }
    synchronized (this) {
        Message p = mMessages;
        //从消息队列的头部开始，移除所有符合条件的消息
        while (p != null && p.target == h && p.what == what
               && (object == null || p.obj == object)) {
            Message n = p.next;
            mMessages = n;
            p.recycleUnchecked();
            p = n;
        }
        //移除剩余的符合要求的消息
        while (p != null) {
            Message n = p.next;
            if (n != null) {
                if (n.target == h && n.what == what
                    && (object == null || n.obj == object)) {
                    Message nn = n.next;
                    n.recycleUnchecked();
                    p.next = nn;
                    continue;
                }
            }
            p = n;
        }
    }
}
```



### 3.5 postSyncBarrier
前面小节[3.3]已说明每一个普通Message必须有一个target，对于特殊的message是没有target，即同步barrier token。 这个消息的价值就是用于拦截同步消息，所以并不会唤醒Looper。
postSyncBarrier只对同步消息产生影响，对于异步消息没有任何差别。

```java
private int postSyncBarrier(long when) {
    // Enqueue a new sync barrier token.
    // We don't need to wake the queue because the purpose of a barrier is to stall it.
    synchronized (this) {
        final int token = mNextBarrierToken++;
        final Message msg = Message.obtain();
        msg.markInUse();
        msg.when = when;
        msg.arg1 = token;

        Message prev = null;
        Message p = mMessages;
        if (when != 0) {
            while (p != null && p.when <= when) {
                prev = p;
                p = p.next;
            }
        }
        if (prev != null) { // invariant: p == prev.next
            msg.next = p;
            prev.next = msg;
        } else {
            msg.next = p;
            mMessages = msg;
        }
        return token;
    }
}
```


```
public void removeSyncBarrier(int token) {
     synchronized (this) {
         Message prev = null;
         Message p = mMessages;
         //从消息队列找到 target为空,并且token相等的Message
         while (p != null && (p.target != null || p.arg1 != token)) {
             prev = p;
             p = p.next;
         }
         if (p == null) {
             throw new IllegalStateException("The specified message queue synchronization "
                     + " barrier token has not been posted or has already been removed.");
         }
         final boolean needWake;
         if (prev != null) {
             prev.next = p.next;
             needWake = false;
         } else {
             mMessages = p.next;
             needWake = mMessages == null || mMessages.target != null;
         }
         p.recycleUnchecked();

         if (needWake && !mQuitting) {
             nativeWake(mPtr);
         }
     }
 }
```



## 4、 Message
### 4.1 创建消息

每个消息用`Message`表示，`Message`主要包含以下内容：

| 数据类型     | 成员变量     | 解释     |
| -------- | -------- | ------ |
| int      | what     | 消息类别   |
| long     | when     | 消息触发时间 |
| int      | arg1     | 参数1    |
| int      | arg2     | 参数2    |
| Object   | obj      | 消息内容   |
| Handler  | target   | 消息响应方  |
| Runnable | callback | 回调方法   |

创建消息的过程，就是填充消息的上述内容的一项或多项。

### 4.2 消息池
在代码中，可能经常看到recycle()方法，咋一看，可能是在做虚拟机的gc()相关的工作，其实不然，这是用于把消息加入到消息池的作用。这样的好处是，当消息池不为空时，可以直接从消息池中获取Message对象，而不是直接创建，提高效率。

静态变量`sPool`的数据类型为Message，通过next成员变量，维护一个消息池；静态变量`MAX_POOL_SIZE`代表消息池的可用大小；消息池的默认大小为50。

消息池常用的操作方法是obtain()和recycle()。

#### 4.2.1 obtain

从消息池中获取消息
obtain()，从消息池取Message，就是把消息池表头的Message取走，再把表头指向next;
```java
public static Message obtain() {
    synchronized (sPoolSync) {
        if (sPool != null) {
            Message m = sPool;
            sPool = m.next;
            m.next = null; //从sPool中取出一个Message对象，并消息链表断开
            m.flags = 0; // 清除in-use flag
            sPoolSize--; //消息池的可用大小进行减1操作
            return m;
        }
    }
    return new Message(); // 当消息池为空时，直接创建Message对象
}
```



#### 4.2.2 recycle
把不再使用的消息加入消息池
recycle()，将Message加入到消息池的过程，就是把Message加到链表的表头。
关于消息的回收还有一点需要注意的就是，我们**平时**写Handler的时候**不需要我们手动回收**，因为谷歌的工程师已经有考虑到这方面的问题了。消息在Handler分发处理之后就会被自动回收的——见Looper的loop【见1.2】
```java
public void recycle() {
    if (isInUse()) { //判断消息是否正在使用
        if (gCheckRecycle) { //Android 5.0以后的版本默认为true,之前的版本默认为false.
            throw new IllegalStateException("This message cannot be recycled because it is still in use.");
        }
        return;
    }
    recycleUnchecked();
}
boolean isInUse() {
    return ((flags & FLAG_IN_USE) == FLAG_IN_USE);
}
//对于不再使用的消息，加入到消息池
void recycleUnchecked() {
    //将消息标示位置为IN_USE，并清空消息所有的参数。
    flags = FLAG_IN_USE;
    what = 0;
    arg1 = 0;
    arg2 = 0;
    obj = null;
    replyTo = null;
    sendingUid = -1;
    when = 0;
    target = null;
    callback = null;
    data = null;
    synchronized (sPoolSync) {
        if (sPoolSize < MAX_POOL_SIZE) { //当消息池没有满时，将Message对象加入消息池
            next = sPool;
            sPool = this;
            sPoolSize++; //消息池的可用大小进行加1操作
        }
    }
}
```







**引用：**
★★★★★[Android的消息机制](http://blog.csdn.net/c10WTiybQ1Ye3/article/details/78098843)
★★★★[Android消息机制1-Handler(Java层)](http://gityuan.com/2015/12/26/handler-message-framework/)
★[Android 源码分析之旅3.1--消息机制源码分析](https://www.jianshu.com/p/ac50ba6ba3a2)




