<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [【Android Handler机制】](#android-handler%E6%9C%BA%E5%88%B6)
  - [Handler](#handler)
  - [Message](#message)
  - [MessageQueue](#messagequeue)
  - [Looper](#looper)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 【Android Handler机制】
Android中主线程也叫UI线程，那么从名字上我们也知道主线程主要是用来创建、更新UI的，而其他耗时操作，比如网络访问，或者文件处理，多媒体处理等都需要在子线程中操作，之所以在子线程中操作是为了保证UI的流畅程度，手机显示的刷新频率是60Hz，也就是一秒钟刷新60次，每16.67毫秒刷新一次，为了不丢帧，那么主线程处理代码最好不要超过16毫秒。当子线程处理完数据后，为了防止UI处理逻辑的混乱，Android只允许主线程修改UI，那么这时候就需要Handler来充当子线程和主线程之间的桥梁了。

我们通常将Handler声明在Activity中，然后覆写Handler中的handleMessage方法,当子线程调用handler.sendMessage()方法后handleMessage方法就会在主线程中执行。
这里面除了Handler、Message外还有隐藏的Looper和MessageQueue对象。
在主线程中Android默认已经调用了Looper.preper()方法，调用该方法的目的是在Looper中创建MessageQueue成员变量并把Looper对象绑定到当前线程中。当调用Handler的sendMessage（对象）方法的时候就将Message对象添加到了Looper创建的MessageQueue队列中，同时给Message指定了target对象，其实这个target对象就是Handler对象。主线程默认执行了Looper.looper（）方法，该方法从Looper的成员变量MessageQueue中取出Message，然后调用Message的target对象的handleMessage()方法。这样就完成了整个消息机制。


Handler机制在Android多线程编程中可以说是不可或缺的角色，也是必须掌握的内容，所以深入掌握并应用Handler异步处理机制在Android开发中显得特别重要。它在使用的过程中主要与Messgae、MessageQueue、和Looper这三个对象关联密切，Handler机制的实现原理依赖于这三者。下面就来讲讲这三者和Handler之间的关系。

## Handler

```
extends Object
```

　首先来看看Handler的几个常见的构造方法，分别是：

*   `Handler()` 默认构造方法，与当前线程及其Looper实例绑定。如在主线程中执行`new Handler()`，那么该handler实例所绑定的便是 UI 线程和 UI 线程绑定的Looper实例。

*   `Handler(Handler.Callback callback)` 与当前线程及其Looper实例绑定，同时调用一个callback接口（用于实现消息处理——即在callback中重写handleMessage()方法）

*   `Handler(Looper looper)` 将该新建的handler实例与指定的looper对象绑定。

*   `Handler(Looper looper, Handler.Callback callback)` 
    指定该handler实例所绑定的looper实例并使用给定的回调接口进行消息处理。

而这些构造函数最终调用的其实都是下面这个构造方法，只是参数缺少的自动补为null或false而已。

```java
public Handler(Looper looper, Callback callback, boolean async) {
    mLooper = looper;
    mQueue = looper.mQueue;
    mCallback = callback;
    mAsynchronous = async;
}
```

　接下来我们来看看**Handler的作用**，它允许我们**将Message或Runnable对象发送到当前线程绑定的MessageQueue中，并通过Looper对象不断循环地从队列中获取Message或Runnable对象进行处理**。因此，Handler有两个主要的用途:

1.  定时执行messages 和 runnables；
2.  在将一个action入队并在其他线程中执行；

在第一个用途中，有以下几个方法可以用：

*   post(Runnable)：将runnable对象入队。

```java
public final boolean post(Runnable r){
    return  sendMessageDelayed(getPostMessage(r), 0);
}
```

*   postAtTime(Runnable, long)：将runnable对象入队，并在指定时间执行

```java
public final boolean postAtTime(Runnable r, long uptimeMillis){
    return sendMessageAtTime(getPostMessage(r), uptimeMillis);
}
```

*   postDelayed(Runnable, long)：将runnable对象入队，并经过指定时间后执行。

```java
 public final boolean postDelayed(Runnable r, long delayMillis){
    return sendMessageDelayed(getPostMessage(r), delayMillis);
}
```

*   sendEmptyMessage(int)：发送只具有`what`标志值得message。

```java
public final boolean sendEmptyMessage(int what){
    return sendEmptyMessageDelayed(what, 0);
}

...

public final boolean sendEmptyMessageDelayed(int what, long delayMillis) {
    Message msg = Message.obtain();
    msg.what = what;
    return sendMessageDelayed(msg, delayMillis);
}
```

*   sendMessage(Message)：将一个message对象入队，且允许该message对象带有一些数据，如一个bundle类型的数据或一个int类型的标志值等，这些数据将在Handler的handleMessage(Message) 方法中进行处理，当然，具体处理逻辑需要我们自己重写handleMessage()方法。

```java
public final boolean sendMessage(Message msg){
    return sendMessageDelayed(msg, 0);
}
```

*   sendMessageDelayed(Message, long)：将message入队，并在当前时间延迟指定时间长度前将该消息放在所有挂起的消息之后。

```java
public final boolean sendMessageDelayed(Message msg, long delayMillis){
    if (delayMillis < 0) {
        delayMillis = 0;
    }
    return sendMessageAtTime(msg, SystemClock.uptimeMillis() + delayMillis);
}
```

*   sendMessageAtTime(Message, long)：将message入队，并在指定时间到之前将该消息放在所有挂起的消息之后。

```java
public boolean sendMessageAtTime(Message msg, long uptimeMillis) {
        MessageQueue queue = mQueue;
        if (queue == null) {
            RuntimeException e = new RuntimeException(
                    this + " sendMessageAtTime() called with no mQueue");
            Log.w("Looper", e.getMessage(), e);
            return false;
        }
        return enqueueMessage(queue, msg, uptimeMillis);
    }
```

以上便是比较常用的方法，并且可以看出上面的所有方法最终调用的都是**`sendMessageAtTime(Message, long)`** 这个方法（如果传入的参数是runnable的话，会先调用`getPostMessage(Runnable)` 或`getPostMessage(Runnable, Object)`方法获取消息），然后由`sendMessageAtTime(Message, long)` 为message对象指定target为该handler实例，并返回一个`enqueueMessage(MessageQueue, Message, long)` 方法，该方法如下，最终通过MessageQueue的`enqueueMessage(Message, long)` 将消息成功送进消息队列中。

```
private boolean enqueueMessage(MessageQueue queue, Message msg, long uptimeMillis) {
    msg.target = this;
    if (mAsynchronous) {
        msg.setAsynchronous(true);
    }
    return queue.enqueueMessage(msg, uptimeMillis);
}
```

## Message

```
extends Object
implements Parcelable
```

　一个message对象包含一个自身的描述信息和一个可以发给handler的任意数据对象。这个对象包含了两个int 类型的extra 字段和一个object类型的extra字段。利用它们，在很多情况下我们都不需要自己做内存分配工作。 
　虽然Message的构造方法是public的，但实例化Message的最好方法是调用`Message.obtain()` 或 `Handler.obtainMessage()` ，因为这两个方法是从一个可回收利用的message对象回收池中获取Message实例。该回收池用于将每次交给handler处理的message对象进行回收。 
　Message对象包含两个额外的int类型变量和一个额外的对象，利用它们大多数情况下我们不用再做内存分配相关工作。实例化Message最好的方法是调用`Message.obtain()`或`Handler.obtainMessage()`（实际上最终调用的仍然是`Message.obtain()`），因为这两个方法都是从一个可回收利用的对象池中获取Message的。

## MessageQueue

```
extends Object
```

MessageQueue是用来存放Message的集合，并由Looper实例来分发里面的Message对象。同时，message并不是直接加入到MessageQueue中的, 而是通过与Looper对象相关联的`MessageQueue.IdleHandler` 对象来完成的。我们可以通过`Looper.myQueue()` 方法来获得当前线程的MessageQueue。 
Tips： 
从Android开发艺术探索艺术一书中，我们可以看到这样一段话：

> MessageQueue的中文翻译是消息队列，顾名思义，它的内部存储了一组消息，以队列的形式对外提供插入和删除的工作。虽然叫消息队列，但是它的内部存储结构并不是真正的队列，而是采用单链表的数据结构来存储消息列表。

## Looper

```
extends Object
```

Looper是线程用来运行消息循环(message loop)的类。默认情况下，线程并没有与之关联的Looper，可以通过在线程中调用`Looper.prepare()` 方法来获取，并通过`Looper.loop()` 无限循环地获取并分发MessageQueue中的消息，直到所有消息全部处理。典型用法如下：

```
class LooperThread extends Thread {
      public Handler mHandler;

      public void run() {
          Looper.prepare();

          mHandler = new Handler() {
              public void handleMessage(Message msg) {
                  // process incoming messages here
              }
          };

          Looper.loop();
      }
  }
```

Tips: 如果是在UI 线程中创建Handler实例的话，是不需要调用`Looper.prepare()` 和 `Looper.loop()` 方法的，因为主线程默认会自己创建对象。但如果像上面例子一样在线程中创建Handler实例，则必须显示调用`Looper.prepare()` 和 `Looper.loop()` 。

现在我们来看一下Looper的内部实现： 
首先是Looper.prepare()方法：

```
public static void prepare() {
    prepare(true);
}

private static void prepare(boolean quitAllowed) {
    if (sThreadLocal.get() != null) {
        throw new RuntimeException("Only one Looper may be created per thread");
    }
    sThreadLocal.set(new Looper(quitAllowed));
}
```

可以看到，调用prepare()方法时，会执行`prepare(boolean quitAllowed)` 方法，其中得先判断sThreadLocal是否为空，不为空才将新建的Looper实例放进sThreadLocal对象中。那么这个sThreadLocal是什么呢？它是一个本地线程存储类，所有线程共享这个对象，但是这个对象对每一个线程而言却具有不同的值，且每个线程对这个对象的访问或修改都不会影响到其他线程，即它的值对于每个线程来说都是独立的。 
而`sThreadLocal.set(new Looper(quitAllowed))`则是把一个新建的Looper实例放进sThreadLocal对象中，而Looper的构造函数内部实现如下：

```
private Looper(boolean quitAllowed) {
    mQueue = new MessageQueue(quitAllowed);
    mThread = Thread.currentThread();
}
```

即创建一个新的MessageQueue对象并绑定当前线程。 
接下来看一下Looper.loop()方法：

```
public static void loop() {
        final Looper me = myLooper();
        if (me == null) {
            throw new RuntimeException("No Looper; Looper.prepare() wasn't called on this thread.");
        }
        final MessageQueue queue = me.mQueue;

        // Make sure the identity of this thread is that of the local process,
        // and keep track of what that identity token actually is.
        Binder.clearCallingIdentity();
        final long ident = Binder.clearCallingIdentity();

        for (;;) {
            Message msg = queue.next(); // might block
            if (msg == null) {
                // No message indicates that the message queue is quitting.
                return;
            }

            // This must be in a local variable, in case a UI event sets the logger
            Printer logging = me.mLogging;
            if (logging != null) {
                logging.println(">>>>> Dispatching to " + msg.target + " " +
                        msg.callback + ": " + msg.what);
            }

            msg.target.dispatchMessage(msg);

            if (logging != null) {
                logging.println("<<<<< Finished to " + msg.target + " " + msg.callback);
            }

            // Make sure that during the course of dispatching the
            // identity of the thread wasn't corrupted.
            final long newIdent = Binder.clearCallingIdentity();
            if (ident != newIdent) {
                Log.wtf(TAG, "Thread identity changed from 0x"
                        + Long.toHexString(ident) + " to 0x"
                        + Long.toHexString(newIdent) + " while dispatching to "
                        + msg.target.getClass().getName() + " "
                        + msg.callback + " what=" + msg.what);
            }

            msg.recycleUnchecked();//将msg回收到message回收池中
        }
    }
```

可以看到，该方法显示获得一个当前线程的Looper实例（通过`myLooper()` 获得），如果该实例存在，接着获取该Looper实例的MessageQueue实例，在确保该线程的身份属于本地进程后，开启一个死循环，不断地调用queue.next()从消息队列中获取消息，该方法的实现如下：

```
Message next() {
        //Return here if the message loop has already quit and been disposed.
        // This can happen if the application tries to restart a looper after quit
        // which is not supported.
        final long ptr = mPtr;
        if (ptr == 0) {
            return null;
        }

        int pendingIdleHandlerCount = -1; // -1 only during first iteration
        int nextPollTimeoutMillis = 0;
        for (;;) {
            if (nextPollTimeoutMillis != 0) {
                Binder.flushPendingCommands();
            }

            nativePollOnce(ptr, nextPollTimeoutMillis);

            synchronized (this) {
                // Try to retrieve the next message.  Return if found.
                final long now = SystemClock.uptimeMillis();
                Message prevMsg = null;
                Message msg = mMessages;
                if (msg != null && msg.target == null) {
                    // Stalled by a barrier.  Find the next asynchronous message in the queue.
                    do {
                        prevMsg = msg;
                        msg = msg.next;
                    } while (msg != null && !msg.isAsynchronous());
                }
                if (msg != null) {
                    if (now < msg.when) {
                        // Next message is not ready.  Set a timeout to wake up when it is ready.
                        nextPollTimeoutMillis = (int) Math.min(msg.when - now, Integer.MAX_VALUE);
                    } else {
                        // Got a message.
                        mBlocked = false;
                        if (prevMsg != null) {
                            prevMsg.next = msg.next;
                        } else {
                            mMessages = msg.next;
                        }
                        msg.next = null;
                        if (DEBUG) Log.v(TAG, "Returning message: " + msg);
                        msg.markInUse();
                        return msg;
                    }
                } else {
                    // No more messages.
                    nextPollTimeoutMillis = -1;
                }

                // Process the quit message now that all pending messages have been handled.
                if (mQuitting) {
                    dispose();
                    return null;
                }

                // If first time idle, then get the number of idlers to run.
                // Idle handles only run if the queue is empty or if the first message
                // in the queue (possibly a barrier) is due to be handled in the future.
                if (pendingIdleHandlerCount < 0
                        && (mMessages == null || now < mMessages.when)) {
                    pendingIdleHandlerCount = mIdleHandlers.size();
                }
                if (pendingIdleHandlerCount <= 0) {
                    // No idle handlers to run.  Loop and wait some more.
                    mBlocked = true;
                    continue;
                }

                if (mPendingIdleHandlers == null) {
                    mPendingIdleHandlers = new IdleHandler[Math.max(pendingIdleHandlerCount, 4)];
                }
                mPendingIdleHandlers = mIdleHandlers.toArray(mPendingIdleHandlers);
            }

            // Run the idle handlers.
            // We only ever reach this code block during the first iteration.
            for (int i = 0; i < pendingIdleHandlerCount; i++) {
                final IdleHandler idler = mPendingIdleHandlers[i];
                mPendingIdleHandlers[i] = null; // release the reference to the handler

                boolean keep = false;
                try {
                    keep = idler.queueIdle();
                } catch (Throwable t) {
                    Log.wtf(TAG, "IdleHandler threw exception", t);
                }

                if (!keep) {
                    synchronized (this) {
                        mIdleHandlers.remove(idler);
                    }
                }
            }

            // Reset the idle handler count to 0 so we do not run them again.
            pendingIdleHandlerCount = 0;

            // While calling an idle handler, a new message could have been delivered
            // so go back and look again for a pending message without waiting.
            nextPollTimeoutMillis = 0;
        }
    }
```

代码虽然很长，但是我们先忽略掉其他，只看正常取消息的部分，其实就是取出单链表（我们前面已说过，MessageQueue其实是一个单链表结构）中的头结点，然后修改对应指针，再返回取到的头结点而已。因为这里采用的是无限循环，所以可能会有个疑问：该循环会不会特别消耗CPU资源？其实并不会，如果messageQueue有消息，自然是继续取消息；如果已经没有消息了，此时该线程便会阻塞在该next()方法的`nativePollOnce()` 方法中，主线程便会释放CPU资源进入休眠状态，直到下个消息到达或者有事务发生时，才通过往pipe管道写端写入数据来唤醒主线程工作。这里涉及到的是**Linux的pipe/epoll机制**，epoll机制是一种IO多路复用机制，可以同时监控多个描述符，当某个描述符就绪(读或写就绪)，则立刻通知相应程序进行读或写操作，本质同步I/O，即读写是阻塞的。

获取到待处理的message后通过`msg.target.dispatchMessage(msg)`进行消息分发，直到队列为空。这里的msg.target指的就是该Looper绑定的Handler实例，而在dispatchMessage(msg)方法中涉及到三个方法，如下：

```
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

...
private static void handleCallback(Message message) {
    message.callback.run();
}
```

这里我们可以看到，在分发消息时三个方法的优先级分别如下：

*   Message的回调方法优先级最高，即message.callback.run()；
*   Handler的回调方法优先级次之，即Handler.mCallback.handleMessage(msg)；
*   Handler的默认方法优先级最低，即Handler.handleMessage(msg)。

在分发完消息后，还会调用`msg.recycleUnchecked()` 方法将msg对象进行回收，具体如下：

```
void recycleUnchecked() {
    // Mark the message as in use while it remains in the recycled object pool.
    // Clear out all other details.
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
        if (sPoolSize < MAX_POOL_SIZE) {
            next = sPool;
            sPool = this;
            sPoolSize++;
        }
    }
}
```

到此，Handler机制便全部讲完了，如果还有疑问欢迎评论留言或继续查看源代码解疑。

引用：
[深入理解Android中的Handler机制](http://blog.csdn.net/reakingf/article/details/52054598)