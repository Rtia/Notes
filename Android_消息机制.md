<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [【Android 消息机制】](#android-%E6%B6%88%E6%81%AF%E6%9C%BA%E5%88%B6)
  - [MessageQueue](#messagequeue)
  - [Looper](#looper)
  - [Handler](#handler)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->



# 【Android 消息机制】

在Android开发中,我们都知道不能在主线程中执行耗时的任务,避免ANR.所以当我们碰到网络请求,或者长时间的IO读写的时候,都会开启一个子线程去执行这些耗时操作,当执行完毕之后,我们可能需要去修改UI界面,但是修改UI的逻辑只能在主线程中进行,所以这个时候我们通常就会用到Handler来协调,代码如下逻辑大体如下 :

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

虽然大家都会使用这样的代码,但是Handler的整个运行机制是怎样子的很多人并不了解,这里我就带大家深入Android源码,通过Handler来分析Android的消息机制。



虽然,我们使用的时候一般只设计到Handler这个类,其实除了**Handler**这个类,还有**Looper**和**MessageQueue**这**三者构成了整个Android的消息机制**。



## MessageQueue

消息队列,用来**保存handler.sendMessage(msg)或者handler.post(r,delayTime)时的产生消息**,虽然名字叫做队列,但是在实现中,这只是一个**单链表的结构**,学过数据结构的朋友都知道,链表用于插入和删除很方便,所以MessageQueue只包含两个主要的操作,**enqueueMessage**和**next**,分别用来**插入**一条消息和**取出**一条消息并且把这条消息从消息队列中移除.要注意的是,MessageQueue只负责保存消息,并不会处理消息.在MessageQueue源码中,next函数有一段类似下面的代码 :

```java
for (;;) {  
    //balalala一大堆
}
```

可以看出,next函数是一个无线循环的函数,如果消息队列中没有消息,那么next函数会一直阻塞在这里,直到有消息过来.



## Looper

可能你并没有使用过这个类,但是也有可能你碰到过下面的异常:

```java
java.lang.RuntimeException: Can't create handler inside thread that has not called Looper.prepare()
```



你没有碰到这个类的原因一般是你在主线程里面创建了一个Handler,然后使用这个handler去处理事情.所以你没有碰到这个异常.你碰到下面的异常是因为你在子线程里面创建了一个handler,使用这个handler去处理事情.这是怎么一回事呢?



那么,为什么有时候我们会碰到Can't create handler inside thread that has not called Looper.prepare()的问题呢,这是因为**Handler的消息是由Looper进行处理**的,但是呢你并没有创建Looper对象,所以会产生这个异常,这时候你可能有疑问了,明明很多时候你并没有创建Looper对象也可以使用Handler啊?没错,那是因为你在主线程中使用了Handler,在主线程中,系统会默认为我们创建一个Looper对象,所以很多时候我们在主线程中使用Handler并没有问题,但是你要是使用下面的代码,就会出现Can't create handler inside thread that has not called Looper.prepare()异常：

```java
new Thread(new Runnable() {    
    @Override
    public void run() {        
        Handler handler = new Handler();
        handler.sendEmptyMessageDelayed(0,1000);
    }
}).start();
```

上面的代码,我们在子线程中创建了一个handler.并且直接使用,发现抛出了异常,那么我们应该怎么解决这个问题呢,很简单,我们创建一个looper对象即可,而**创建looper对象**并不是使用Looper loop = new Looper(),而是直接调用函数**Looper.parpare()**即可.我们跟踪这个函数看看：

```java
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

上面的代码,我们一直跟踪到构造函数,发现在构造函数里面Looper会创建一个MessageQueue对象.我们上面可以知道,**MessageQueue只负责保存消息**,但是不负责处理消息,**处理消息的任务是交给Looper完成**的,**Looper类中的函数loop会不断的调用MessageQueue的next函数来查看是否有新消息**,如果有马上去处理这个消息,那我们知道next函数有可能会堵塞,所以这就导致了loop函数堵塞.

```java
for (;;) {    
    Message msg = queue.next(); // might block
    //balalala一大堆
    msg.target.dispatchMessage(msg);
}
```

以上就是关于Loop的主要内容了.现在我们**在子线程中创建Handler**的时候,我们**一定要手动的创建一个Looper类来实现消息处理**.

```java
new Thread(new Runnable() {    
    @Override
    public void run() {        
        Looper.prepare();        
        Handler handler = new Handler();
        handler.sendEmptyMessageDelayed(0,1000);    
        //一定要调用loop()方法,否则我们只是单单创建了一个looper            
        Looper.loop();  
    }
}).start();
```

再看loop函数的源码,当**MessageQueue的next返回新消息**,**Looper**就会**处理**这条消息 : **`msg.target.dispatchMessage(msg)`**;这里的**msg.target就是发送这条消息的Handler**,这样子**,Handler发送的消息最终又要交给它的dispatchMessage函数来处理**。

接下来我们就分析一下Handler的内部原理吧.



## Handler

一般,我们通过Handler的post或者send的一系列函数来发送消息,但是其实不管是send还是post,最终都是通过send函数来实现的.

```java
public final boolean postDelayed(Runnable r, long delayMillis) {    
    //post函数最终通过send函数来实现.
    return sendMessageDelayed(getPostMessage(r), delayMillis);
}
```

根据postDelayed函数的源码可以发现最终post系列的函数都还是要通过send函数来发送的.那么,发送消息的原理是怎样子的呢,我们来深入一个send函数看看.send系列函数有很多个,但是最终都要进入这个函数:

```java
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
```

通过上面的源码,我们不难发现,**send函数其实就是往消息队列里面插入一条消息**,那么**Looper的loop函数又会调用MessageQueue的next函数去获取消息**,**最后在交给Handler的dispatchMessage函数处理**,这样子我们就把Handler, MessageQueue, Looper三者串起来了.那么接下来我们赶紧来看看dispatchMessage是什么样子的.

```java
public void dispatchMessage(Message msg) {    
    if (msg.callback != null) {
        handleCallback(msg);
    } else {        
        if (mCallback != null) {            
            if (mCallback.handleMessage(msg)) { 
                return;
            }
        }
        handleMessage(msg);
    }
}
```

这里有两个callBack,我们一一来分析一下是什么意思,首先msg.callback是你在调用Handler.post....()函数时候创建出来的, **msg.callback就是这里的runnbale匿名类**:

```java
handler.postDelayed(new Runnable() {    
    @Override
    public void run() {      
        //.....
    }
}, 1000);
```

如果你顺着handleCallback去找发现最后**handleCallback(msg)就是去执行runnbale中的run函数**.没错,这里**将开启一个线程**,所以我们就明白了最开始我们提到的使用Handler处理耗时操作和UI操作的原理了.那么,如果你是调用handler.sendMessage()函数的话,callBack为null。
那么第二个**mCallback**是什么意思呢?要回答这个问题,我们得看看Handler的构造函数中有一个需要传入一个callback作为参数的构造函数,这么说你就懂了,如果你**在创建Handler的时候传递了一个callback**作为参数,那么在**dispatchMessage函数中就会调用callback中的handleMessage**函数.最后如果在dispatchMessage执行到了handleMessage(msg);又是怎样的情况,我们跟踪到handleMessage(msg)函数中,发现这只是一个接口:

```java
public interface Callback {    
    public boolean handleMessage(Message msg);
}
```

于是我们应该就会想起我们在创建Handler的时候有时候会写这样子的代码:

```java
Handler handler = new Handler(){    
    @Override
    public void handleMessage(Message msg) {        
        super.handleMessage(msg);
    }
};
```

是的,这个时候dispatchMessage就会来执行这里的函数逻辑.



结语

以上,就是整个Android消息机制的原理,主要就是依靠Handler, MessageQueue, Looper这三个类来完成的.现在再来看最开始的代码:

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

我们就应该可以很清楚的说出 : handler.sendMessage函数最终还是转换成了send系列函数往MessageQueue里面插入了一条消息队列,然后主线程已经为我们创建好的Looper对象在loop函数中调用了MessageQueue的next函数来读取到这条消息,再调用Handler的dispatchMessage函数来处理消息,在dispatchMessage里面会去执行postDelayed中第一个参数runnbale开启的子线程.
在这个子线程里面最终又调用了sendMessage,经过一样的逻辑之后,被handler的实现函数handleMessage处理了.



**引用：**
[Android的消息机制](http://blog.csdn.net/c10WTiybQ1Ye3/article/details/78098843)

