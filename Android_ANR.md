<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [【Android ANR】](#android-anr)
  - [概述](#%E6%A6%82%E8%BF%B0)
    - [1.1 何为ANR](#11-%E4%BD%95%E4%B8%BAanr)
    - [1.2 为什么会产生ANR](#12-%E4%B8%BA%E4%BB%80%E4%B9%88%E4%BC%9A%E4%BA%A7%E7%94%9Fanr)
    - [1.3 如何避免ANR](#13-%E5%A6%82%E4%BD%95%E9%81%BF%E5%85%8Danr)
  - [2, ANR分析](#2-anr%E5%88%86%E6%9E%90)
    - [2.1 获取ANR产生的trace文件](#21-%E8%8E%B7%E5%8F%96anr%E4%BA%A7%E7%94%9F%E7%9A%84trace%E6%96%87%E4%BB%B6)
    - [2.2 分析traces.txt](#22-%E5%88%86%E6%9E%90tracestxt)
      - [2.2.1 普通阻塞导致的ANR](#221-%E6%99%AE%E9%80%9A%E9%98%BB%E5%A1%9E%E5%AF%BC%E8%87%B4%E7%9A%84anr)
      - [2.2.2 CPU满负荷](#222-cpu%E6%BB%A1%E8%B4%9F%E8%8D%B7)
      - [2.2.3 内存原因](#223-%E5%86%85%E5%AD%98%E5%8E%9F%E5%9B%A0)
    - [2.2 ANR的处理](#22-anr%E7%9A%84%E5%A4%84%E7%90%86)
  - [3, 深入一点](#3-%E6%B7%B1%E5%85%A5%E4%B8%80%E7%82%B9)
    - [3.1 哪些地方是执行在主线程的](#31-%E5%93%AA%E4%BA%9B%E5%9C%B0%E6%96%B9%E6%98%AF%E6%89%A7%E8%A1%8C%E5%9C%A8%E4%B8%BB%E7%BA%BF%E7%A8%8B%E7%9A%84)
    - [3.2 使用子线程的方式有哪些](#32-%E4%BD%BF%E7%94%A8%E5%AD%90%E7%BA%BF%E7%A8%8B%E7%9A%84%E6%96%B9%E5%BC%8F%E6%9C%89%E5%93%AA%E4%BA%9B)
      - [3.2.1 启Thread方式](#321-%E5%90%AFthread%E6%96%B9%E5%BC%8F)
      - [3.2.2 使用AsyncTask](#322-%E4%BD%BF%E7%94%A8asynctask)
      - [3.2.3 HandlerThread](#323-handlerthread)
      - [3.2.4 IntentService](#324-intentservice)
      - [3.2.5 Loader](#325-loader)
      - [3.2.6 特别注意](#326-%E7%89%B9%E5%88%AB%E6%B3%A8%E6%84%8F)
- [ANR定位](#anr%E5%AE%9A%E4%BD%8D)
    - [**九：如何调查并解决ANR**](#%E4%B9%9D%E5%A6%82%E4%BD%95%E8%B0%83%E6%9F%A5%E5%B9%B6%E8%A7%A3%E5%86%B3anr)
      - [案例1：关键词:ContentResolver in AsyncTask onPostExecute, high iowait](#%E6%A1%88%E4%BE%8B1%E5%85%B3%E9%94%AE%E8%AF%8Dcontentresolver-in-asynctask-onpostexecute-high-iowait)
  - [案例2：关键词:在UI线程进行网络数据的读写](#%E6%A1%88%E4%BE%8B2%E5%85%B3%E9%94%AE%E8%AF%8D%E5%9C%A8ui%E7%BA%BF%E7%A8%8B%E8%BF%9B%E8%A1%8C%E7%BD%91%E7%BB%9C%E6%95%B0%E6%8D%AE%E7%9A%84%E8%AF%BB%E5%86%99)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 【Android ANR】

## 概述

### 1.1 何为ANR
ANR全名Application Not Responding, 也就是"应用无响应". 当操作在一段时间内系统无法处理时, 系统层面会弹出上图那样的ANR对话框.

### 1.2 为什么会产生ANR
在Android里, App的响应能力是由Activity Manager和Window Manager系统服务来监控的. 通常在如下两种情况下会弹出ANR对话框:

*   5s内无法响应用户输入事件(例如键盘输入, 触摸屏幕等).

*   BroadcastReceiver在10s内无法结束.

造成以上两种情况的首要原因就是**在主线程(UI线程)里面做了太多的阻塞耗时操作**, 例如文件读写, 数据库读写, 网络查询等等.

### 1.3 如何避免ANR

知道了ANR产生的原因, 那么想要避免ANR, 也就很简单了, 就一条规则:

> 不要在主线程(UI线程)里面做繁重的操作.

这里面实际上涉及到两个问题:

1.  哪些地方是运行在主线程的?

2.  不在主线程做, 在哪儿做?

稍后解答.

## 2, ANR分析
### 2.1 获取ANR产生的trace文件

ANR产生时, 系统会生成一个traces.txt的文件放在/data/anr/下. 可以通过[adb](https://www.jianshu.com/p/5980c8c282ef)命令将其导出到本地:

```
$adb pull data/anr/traces.txt .

```

### 2.2 分析traces.txt

#### 2.2.1 普通阻塞导致的ANR

获取到的tracs.txt文件一般如下:

> 如下以[GithubApp](https://link.jianshu.com?t=https://github.com/mingjunli/GithubApp)代码为例, 强行sleep thread产生的一个ANR.

```java
----- pid 2976 at 2016-09-08 23:02:47 -----
Cmd line: com.anly.githubapp  // 最新的ANR发生的进程(包名)

...

DALVIK THREADS (41):
"main" prio=5 tid=1 Sleeping
  | group="main" sCount=1 dsCount=0 obj=0x73467fa8 self=0x7fbf66c95000
  | sysTid=2976 nice=0 cgrp=default sched=0/0 handle=0x7fbf6a8953e0
  | state=S schedstat=( 0 0 0 ) utm=60 stm=37 core=1 HZ=100
  | stack=0x7ffff4ffd000-0x7ffff4fff000 stackSize=8MB
  | held mutexes=
  at java.lang.Thread.sleep!(Native method)
  - sleeping on <0x35fc9e33> (a java.lang.Object)
  at java.lang.Thread.sleep(Thread.java:1031)
  - locked <0x35fc9e33> (a java.lang.Object)
  at java.lang.Thread.sleep(Thread.java:985) // 主线程中sleep过长时间, 阻塞导致无响应.
  at com.tencent.bugly.crashreport.crash.c.l(BUGLY:258)
  - locked <@addr=0x12dadc70> (a com.tencent.bugly.crashreport.crash.c)
  at com.tencent.bugly.crashreport.CrashReport.testANRCrash(BUGLY:166)  // 产生ANR的那个函数调用
  - locked <@addr=0x12d1e840> (a java.lang.Class<com.tencent.bugly.crashreport.CrashReport>)
  at com.anly.githubapp.common.wrapper.CrashHelper.testAnr(CrashHelper.java:23)
  at com.anly.githubapp.ui.module.main.MineFragment.onClick(MineFragment.java:80) // ANR的起点
  at com.anly.githubapp.ui.module.main.MineFragment_ViewBinding$2.doClick(MineFragment_ViewBinding.java:47)
  at butterknife.internal.DebouncingOnClickListener.onClick(DebouncingOnClickListener.java:22)
  at android.view.View.performClick(View.java:4780)
  at android.view.View$PerformClick.run(View.java:19866)
  at android.os.Handler.handleCallback(Handler.java:739)
  at android.os.Handler.dispatchMessage(Handler.java:95)
  at android.os.Looper.loop(Looper.java:135)
  at android.app.ActivityThread.main(ActivityThread.java:5254)
  at java.lang.reflect.Method.invoke!(Native method)
  at java.lang.reflect.Method.invoke(Method.java:372)
  at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:903)
  at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:698)

```

拿到trace信息, 一切好说.

如上trace信息中的**添加的中文注释**已基本说明了trace文件该怎么分析:

1.  文件最上的即为最新产生的ANR的trace信息.

2.  前面两行表明ANR发生的进程pid, 时间, 以及进程名字(包名).

3.  寻找我们的代码点, 然后往前推, 看方法调用栈, 追溯到问题产生的根源.

> 以上的ANR trace是属于相对简单, 还有可能你并没有在主线程中做过于耗时的操作, 然而还是ANR了. 这就有可能是如下两种情况了:

#### 2.2.2 CPU满负荷
这个时候你看到的trace信息可能会包含这样的信息:

```
Process:com.anly.githubapp
...
CPU usage from 3330ms to 814ms ago:
6% 178/system_server: 3.5% user + 1.4% kernel / faults: 86 minor 20 major
4.6% 2976/com.anly.githubapp: 0.7% user + 3.7% kernel /faults: 52 minor 19 major
0.9% 252/com.android.systemui: 0.9% user + 0% kernel
...

100%TOTAL: 5.9% user + 4.1% kernel + 89% iowait

```

最后一句表明了:

1.  当是CPU占用100%, 满负荷了.

2.  其中绝大数是被iowait即I/O操作占用了.

此时分析方法调用栈, 一般来说会发现是方法中有频繁的文件读写或是数据库读写操作放在主线程来做了.

#### 2.2.3 内存原因
其实内存原因有可能会导致ANR, 例如如果由于内存泄露, App可使用内存所剩无几, 我们点击按钮启动一个大图片作为背景的activity, 就可能会产生ANR, 这时trace信息可能是这样的:

```java
// 以下trace信息来自网络, 用来做个示例
Cmdline: android.process.acore

DALVIK THREADS:
"main"prio=5 tid=3 VMWAIT
|group="main" sCount=1 dsCount=0 s=N obj=0x40026240self=0xbda8
| sysTid=1815 nice=0 sched=0/0 cgrp=unknownhandle=-1344001376
atdalvik.system.VMRuntime.trackExternalAllocation(NativeMethod)
atandroid.graphics.Bitmap.nativeCreate(Native Method)
atandroid.graphics.Bitmap.createBitmap(Bitmap.java:468)
atandroid.view.View.buildDrawingCache(View.java:6324)
atandroid.view.View.getDrawingCache(View.java:6178)

...

MEMINFO in pid 1360 [android.process.acore] **
native dalvik other total
size: 17036 23111 N/A 40147
allocated: 16484 20675 N/A 37159
free: 296 2436 N/A 2732

```

可以看到free的内存已所剩无几.

> 当然这种情况可能更多的是会产生OOM的异常...

### 2.2 ANR的处理
针对三种不同的情况, 一般的处理情况如下

1.  主线程阻塞的

    开辟单独的子线程来处理耗时阻塞事务.

2.  CPU满负荷, I/O阻塞的

    I/O阻塞一般来说就是文件读写或数据库操作执行在主线程了, 也可以通过开辟子线程的方式异步执行.

3.  内存不够用的

    增大VM内存, 使用largeHeap属性, 排查内存泄露(这个在内存优化那篇细说吧)等.

## 3, 深入一点
没有人愿意在出问题之后去解决问题.

高手和新手的区别是, 高手知道怎么在一开始就避免问题的发生. 那么针对ANR这个问题, 我们需要做哪些层次的工作来避免其发生呢?

### 3.1 哪些地方是执行在主线程的
1.  [Activity的所有生命周期回调](https://www.jianshu.com/p/ec6984aa1bb4)都是执行在主线程的.

2.  Service默认是执行在主线程的.

3.  BroadcastReceiver的onReceive回调是执行在主线程的.

4.  没有使用子线程的looper的Handler的handleMessage, post(Runnable)是执行在主线程的.

5.  AsyncTask的回调中除了doInBackground, 其他都是执行在主线程的.

6.  View的post(Runnable)是执行在主线程的.

### 3.2 使用子线程的方式有哪些
上面我们几乎一直在说, 避免ANR的方法就是在子线程中执行耗时阻塞操作. 那么在Android中有哪些方式可以让我们实现这一点呢.

#### 3.2.1 启Thread方式
这个其实也是Java实现多线程的方式. 有两种实现方法, 继承Thread 或 实现Runnable接口:

**继承Thread**

```
class PrimeThread extends Thread {
    long minPrime;
    PrimeThread(long minPrime) {
        this.minPrime = minPrime;
    }

    public void run() {
        // compute primes larger than minPrime
         . . .
    }
}

PrimeThread p = new PrimeThread(143);
p.start();

```

**实现Runnable接口**

```
class PrimeRun implements Runnable {
    long minPrime;
    PrimeRun(long minPrime) {
        this.minPrime = minPrime;
    }

    public void run() {
        // compute primes larger than minPrime
         . . .
    }
}

PrimeRun p = new PrimeRun(143);
new Thread(p).start();

```

#### 3.2.2 使用AsyncTask
这个是Android特有的方式, AsyncTask顾名思义, 就是异步任务的意思.

```
private class DownloadFilesTask extends AsyncTask<URL, Integer, Long> {
    // Do the long-running work in here
    // 执行在子线程
    protected Long doInBackground(URL... urls) {
        int count = urls.length;
        long totalSize = 0;
        for (int i = 0; i < count; i++) {
            totalSize += Downloader.downloadFile(urls[i]);
            publishProgress((int) ((i / (float) count) * 100));
            // Escape early if cancel() is called
            if (isCancelled()) break;
        }
        return totalSize;
    }

    // This is called each time you call publishProgress()
    // 执行在主线程
    protected void onProgressUpdate(Integer... progress) {
        setProgressPercent(progress[0]);
    }

    // This is called when doInBackground() is finished
    // 执行在主线程
    protected void onPostExecute(Long result) {
        showNotification("Downloaded " + result + " bytes");
    }
}

// 启动方式
new DownloadFilesTask().execute(url1, url2, url3);

```

#### 3.2.3 HandlerThread
Android中结合Handler和Thread的一种方式. 前面有云, 默认情况下Handler的handleMessage是执行在主线程的, 但是如果我给这个Handler传入了子线程的looper, handleMessage就会执行在这个子线程中的. HandlerThread正是这样的一个结合体:

```
// 启动一个名为new_thread的子线程
HandlerThread thread = new HandlerThread("new_thread");
thread.start();

// 取new_thread赋值给ServiceHandler
private ServiceHandler mServiceHandler;
mServiceLooper = thread.getLooper();
mServiceHandler = new ServiceHandler(mServiceLooper);

private final class ServiceHandler extends Handler {
    public ServiceHandler(Looper looper) {
      super(looper);
    }

    @Override
    public void handleMessage(Message msg) {
      // 此时handleMessage是运行在new_thread这个子线程中了.
    }
}

```

#### 3.2.4 IntentService
Service是运行在主线程的, 然而IntentService是运行在子线程的.

实际上IntentService就是实现了一个HandlerThread + ServiceHandler的模式.

以上HandlerThread的使用代码示例也就来自于[IntentService源码](https://link.jianshu.com?t=https://github.com/android/platform_frameworks_base/blob/master/core/java/android/app/IntentService.java).

#### 3.2.5 Loader
Android 3.0引入的数据加载器, 可以在Activity/Fragment中使用. 支持异步加载数据, 并可监控数据源在数据发生变化时传递新结果. 常用的有CursorLoader, 用来加载数据库数据.

```
// Prepare the loader.  Either re-connect with an existing one,
// or start a new one.
// 使用LoaderManager来初始化Loader
getLoaderManager().initLoader(0, null, this);

//如果 ID 指定的加载器已存在，则将重复使用上次创建的加载器。
//如果 ID 指定的加载器不存在，则 initLoader() 将触发 LoaderManager.LoaderCallbacks 方法 //onCreateLoader()。在此方法中，您可以实现代码以实例化并返回新加载器

// 创建一个Loader
public Loader<Cursor> onCreateLoader(int id, Bundle args) {
    // This is called when a new Loader needs to be created.  This
    // sample only has one Loader, so we don't care about the ID.
    // First, pick the base URI to use depending on whether we are
    // currently filtering.
    Uri baseUri;
    if (mCurFilter != null) {
        baseUri = Uri.withAppendedPath(Contacts.CONTENT_FILTER_URI,
                  Uri.encode(mCurFilter));
    } else {
        baseUri = Contacts.CONTENT_URI;
    }

    // Now create and return a CursorLoader that will take care of
    // creating a Cursor for the data being displayed.
    String select = "((" + Contacts.DISPLAY_NAME + " NOTNULL) AND ("
            + Contacts.HAS_PHONE_NUMBER + "=1) AND ("
            + Contacts.DISPLAY_NAME + " != '' ))";
    return new CursorLoader(getActivity(), baseUri,
            CONTACTS_SUMMARY_PROJECTION, select, null,
            Contacts.DISPLAY_NAME + " COLLATE LOCALIZED ASC");
}

// 加载完成
public void onLoadFinished(Loader<Cursor> loader, Cursor data) {
    // Swap the new cursor in.  (The framework will take care of closing the
    // old cursor once we return.)
    mAdapter.swapCursor(data);
}

```

具体请参看[官网Loader介绍](https://link.jianshu.com?t=https://developer.android.com/guide/components/loaders.html?hl=zh-cn).

#### 3.2.6 特别注意
使用Thread和HandlerThread时, 为了使效果更好, 建议设置Thread的优先级偏低一点:

```
Process.setThreadPriority(THREAD_PRIORITY_BACKGROUND);
```

因为如果没有做任何优先级设置的话, 你创建的Thread默认和UI Thread是具有同样的优先级的, 你懂的. 同样的优先级的Thread, CPU调度上还是可能会阻塞掉你的UI Thread, 导致ANR的.



# ANR定位

**一：什么是ANR**

ANR:Application Not Responding，即应用无响应

**二：ANR的类型**

ANR一般有三种类型：

1：KeyDispatchTimeout(5 seconds) --**主要类型**

按键或触摸事件在特定时间内无响应

2**：**BroadcastTimeout(10 seconds)

BroadcastReceiver在特定时间内无法处理完成

3：ServiceTimeout(20 seconds) --**小概率类型**

Service在特定的时间内无法处理完成

**三：KeyDispatchTimeout**

Akey or touch event was not dispatched within the specified time（按键或触摸事件在特定时间内无响应）

具体的超时时间的定义在framework下的

ActivityManagerService.java

//How long we wait until we timeout on key dispatching.

staticfinal int KEY_DISPATCHING_TIMEOUT = 5*1000

**四：为什么会超时呢？**

超时时间的计数一般是从按键分发给app开始。超时的原因一般有两种**：**

(1)当前的事件没有机会得到处理（即UI线程正在处理前一个事件，没有及时的完成或者looper被某种原因阻塞住了）

(2)当前的事件正在处理，但没有及时完成

**五：如何避免KeyDispatchTimeout**

1**：**UI**线程尽量只做跟**UI**相关的工作**

2**：耗时的工作（比如数据库操作，**I/O**，连接网络或者别的有可能阻碍**UI**线程的操作）把它放入单独的线程处理**

3**：尽量用**Handler**来处理**UIthread**和别的**thread**之间的交互**

**六：UI线程**

说了那么多的UI线程，那么哪些属于UI线程呢？

UI线程主要包括如下：

1.  Activity:onCreate(), onResume(), onDestroy(), onKeyDown(), onClick(),etc

2.  AsyncTask: onPreExecute(), onProgressUpdate(), onPostExecute(), onCancel,etc

3.  Mainthread handler: handleMessage(), post*(runnable r), etc

4.  other

**七:如何去分析ANR**

先看个LOG:

04-01 13:12:11.572** I/InputDispatcher( 220): Application is not responding**:Window{2b263310com.android.email/com.android.email.activity.SplitScreenActivitypaused=false}.  5009.8ms since event, 5009.5ms since waitstarted

04-0113:12:11.572 I/WindowManager( 220): Input event dispatching timedout sending tocom.android.email/com.android.email.activity.SplitScreenActivity

04-01 **13:12:14.123 I/Process(  220): Sending signal. PID: 21404 SIG: 3---****发生**ANR**的时间和生成**trace.txt**的时间**

04-01 13:12:14.123 I/dalvikvm(21404):threadid=4: reacting to signal 3 

……

04-0113:12:15.872 E/ActivityManager(  220): ANR in com.android.email(com.android.email/.activity.SplitScreenActivity)

04-0113:12:15.872 E/ActivityManager(  220): Reason:keyDispatchingTimedOut

04-0113:12:15.872 E/ActivityManager(  220): Load: 8.68 / 8.37 / 8.53

04-0113:12:15.872 E/ActivityManager(  220): **CPUusage from 4361ms to 699ms ago** ----CPU在ANR发生前的使用情况

04-0113:12:15.872 E/ActivityManager(  220):   5.5%21404/com.android.email: 1.3% user + 4.1% kernel / faults: 10 minor

04-0113:12:15.872 E/ActivityManager(  220):   4.3%220/system_server: 2.7% user + 1.5% kernel / faults: 11 minor 2 major

04-0113:12:15.872 E/ActivityManager(  220):   0.9%52/spi_qsd.0: 0% user + 0.9% kernel

04-0113:12:15.872 E/ActivityManager(  220):   0.5%65/irq/170-cyttsp-: 0% user + 0.5% kernel

04-0113:12:15.872 E/ActivityManager(  220):   0.5%296/com.android.systemui: 0.5% user + 0% kernel

04-0113:12:15.872 E/ActivityManager(  220): **100%TOTAL: 4.8% user + 7.6% kernel + 87% iowait**

04-0113:12:15.872 E/ActivityManager(  220): **CPUusage from 3697ms to 4223ms later**:-- ANR后CPU的使用量

04-0113:12:15.872 E/ActivityManager(  220):   25%21404/com.android.email: 25% user + 0% kernel / faults: 191 minor

04-0113:12:15.872 E/ActivityManager(  220):    16% 21603/__eas(par.hakan: 16% user + 0% kernel

04-0113:12:15.872 E/ActivityManager(  220):    7.2% 21406/GC: 7.2% user + 0% kernel

04-0113:12:15.872 E/ActivityManager(  220):    1.8% 21409/Compiler: 1.8% user + 0% kernel

04-0113:12:15.872 E/ActivityManager(  220):   5.5%220/system_server: 0% user + 5.5% kernel / faults: 1 minor

04-0113:12:15.872 E/ActivityManager(  220):    5.5% 263/InputDispatcher: 0% user + 5.5% kernel

04-0113:12:15.872 E/ActivityManager(  220): **32%TOTAL: 28% user + 3.7% kernel**

从LOG可以看出ANR的类型，CPU的使用情况，如果CPU使用量接近100%，说明当前设备很忙，有可能是CPU饥饿导致了ANR

如果CPU使用量很少，说明主线程被BLOCK了

如果IOwait很高，说明ANR有可能是主线程在进行I/O操作造成的

除了看LOG，解决ANR还得需要trace.txt文件，

如何获取呢？可以用如下命令获取

1.  $chmod 777 /data/anr

2.  $rm /data/anr/traces.txt

3.  $ps

4.  $kill -3 PID

5.  adbpull data/anr/traces.txt ./mytraces.txt

从trace.txt文件，看到最多的是如下的信息：

-----pid 21404 at 2011-04-01 13:12:14 -----  
Cmdline: com.android.email

DALVIK THREADS:
(mutexes: tll=0tsl=0 tscl=0 ghl=0 hwl=0 hwll=0)
"main" prio=5 tid=1NATIVE
  | group="main" sCount=1 dsCount=0obj=0x2aad2248 self=0xcf70
  | sysTid=21404 nice=0 sched=0/0cgrp=[fopen-error:2] handle=1876218976
  **atandroid.os.MessageQueue.nativePollOnce(Native Method)
  atandroid.os.MessageQueue.next(MessageQueue.java:119)
  atandroid.os.Looper.loop(Looper.java:110**)
 at android.app.ActivityThread.main(ActivityThread.java:3688)
 at java.lang.reflect.Method.invokeNative(Native Method)
  atjava.lang.reflect.Method.invoke(Method.java:507)
  atcom.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:866)
 at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:624)
 at dalvik.system.NativeStart.main(Native Method)

说明主线程在等待下条消息进入消息队列

**八：Thread状态**

```java
ThreadState (defined at “dalvik/vm/thread.h “)  
THREAD_UNDEFINED = -1, /* makes enum compatible with int32_t */  
THREAD_ZOMBIE = 0, /* TERMINATED */  
THREAD_RUNNING = 1, /* RUNNABLE or running now */  
THREAD_TIMED_WAIT = 2, /* TIMED_WAITING in Object.wait() */  
THREAD_MONITOR = 3, /* BLOCKED on a monitor */  
THREAD_WAIT = 4, /* WAITING in Object.wait() */  
THREAD_INITIALIZING= 5, /* allocated, not yet running */  
THREAD_STARTING = 6, /* started, not yet on thread list */  
THREAD_NATIVE = 7, /* off in a JNI native method */  
THREAD_VMWAIT = 8, /* waiting on a VM resource */  
THREAD_SUSPENDED = 9, /* suspended, usually by GC or debugger */  
```

### **九：如何调查并解决ANR**

1：首先分析log

2: 从trace.txt文件查看调用stack.

3: 看代码

4：仔细查看ANR的成因（iowait?block?memoryleak?）

**十：案例**

#### 案例1：关键词:ContentResolver in AsyncTask onPostExecute, high iowait

Process:com.android.email
Activity:com.android.email/.activity.MessageView
Subject:keyDispatchingTimedOut
CPU usage from 2550ms to -2814ms ago:
5%187/system_server: 3.5% user + 1.4% kernel / faults: 86 minor 20major
4.4% 1134/com.android.email: 0.7% user + 3.7% kernel /faults: 38 minor 19 major
4% 372/com.android.eventstream: 0.7%user + 3.3% kernel / faults: 6 minor
1.1% 272/com.android.phone:0.9% user + 0.1% kernel / faults: 33 minor
0.9%252/com.android.systemui: 0.9% user + 0% kernel
0%409/com.android.eventstream.telephonyplugin: 0% user + 0% kernel /faults: 2 minor
0.1% 632/com.android.devicemonitor: 0.1% user + 0%kernel **100%TOTAL: 6.9% user + 8.2% kernel +****84%iowait** -----pid 1134 at 2010-12-17 17:46:51 -----
Cmd line:com.android.email

DALVIK THREADS:
(mutexes: tll=0 tsl=0tscl=0 ghl=0 hwl=0 hwll=0)
"main" prio=5 tid=1 WAIT
|group="main" sCount=1 dsCount=0 obj=0x2aaca180self=0xcf20
| sysTid=1134 nice=0 sched=0/0 cgrp=[fopen-error:2]handle=1876218976
at java.lang.Object.wait(Native Method)
-waiting on <0x2aaca218> (a java.lang.VMThread)
atjava.lang.Thread.parkFor(Thread.java:1424)
atjava.lang.LangAccessImpl.parkFor(LangAccessImpl.java:48)
atsun.misc.Unsafe.park(Unsafe.java:337)
atjava.util.concurrent.locks.LockSupport.park(LockSupport.java:157)
atjava.util.concurrent.locks.AbstractQueuedSynchronizer.parkAndCheckInterrupt(AbstractQueuedSynchronizer.java:808)
atjava.util.concurrent.locks.AbstractQueuedSynchronizer.acquireQueued(AbstractQueuedSynchronizer.java:841)
atjava.util.concurrent.locks.AbstractQueuedSynchronizer.acquire(AbstractQueuedSynchronizer.java:1171)
atjava.util.concurrent.locks.ReentrantLock$FairSync.lock(ReentrantLock.java:200)
atjava.util.concurrent.locks.ReentrantLock.lock(ReentrantLock.java:261)
atandroid.database.sqlite.SQLiteDatabase.lock(SQLiteDatabase.java:378)
atandroid.database.sqlite.SQLiteCursor.<init>(SQLiteCursor.java:222)
atandroid.database.sqlite.SQLiteDirectCursorDriver.query(SQLiteDirectCursorDriver.java:53)
atandroid.database.sqlite.SQLiteDatabase.rawQueryWithFactory(SQLiteDatabase.java:1356)
atandroid.database.sqlite.SQLiteDatabase.queryWithFactory(SQLiteDatabase.java:1235)
atandroid.database.sqlite.SQLiteDatabase.query(SQLiteDatabase.java:1189)
atandroid.database.sqlite.SQLiteDatabase.query(SQLiteDatabase.java:1271)
atcom.android.email.provider.EmailProvider.query(EmailProvider.java:1098)
atandroid.content.ContentProvider$Transport.query(ContentProvider.java:187)
atandroid.content.**ContentResolver.query**(ContentResolver.java:268)
atcom.android.email.provider.EmailContent$Message.restoreMessageWithId(EmailContent.java:648)
atcom.android.email.Controller.setMessageRead(Controller.java:658)
atcom.android.email.activity.MessageView.onMarkAsRead(MessageView.java:700)
atcom.android.email.activity.MessageView.access$2500(MessageView.java:98)
at**com.android.email.activity.MessageView$LoadBodyTask****.onPostExecute**(MessageView.java:1290)
atcom.android.email.activity.MessageView$LoadBodyTask.onPostExecute(MessageView.java:1255)
atandroid.os.AsyncTask.finish(AsyncTask.java:417)
atandroid.os.AsyncTask.access$300(AsyncTask.java:127)
at**android.os.****AsyncTask****$InternalHandler.handleMessage**(AsyncTask.java:429)
atandroid.os.Handler.dispatchMessage(Handler.java:99)
atandroid.os.Looper.loop(Looper.java:123)
atandroid.app.ActivityThread.main(ActivityThread.java:3652)
atjava.lang.reflect.Method.invokeNative(Native Method)
atjava.lang.reflect.Method.invoke(Method.java:507)
atcom.android.internal.os.ZygoteIn

**原因：****IOWait****很高，说明当前系统在忙于****I/O****，因此数据库操作被阻塞**

**原来：**

<colgroup style="font-family: Verdana !important;"><col width="1099" style="font-family: Verdana !important;"></colgroup>
| 

        finalMessagemessage=Message.restoreMessageWithId(mProviderContext,messageId);

 |
| 

        if(message==null){

 |
| 

           return;

 |
| 

        }

 |
| 

        Accountaccount=Account.restoreAccountWithId(mProviderContext,message.mAccountKey);

 |
| 

        if(account==null){

 |
| 

           return;//isMessagingController returns false for null, but let's make itclear.

 |
| 

        }

 |
| 

        if(isMessagingController(account)){

 |
| 

           newThread(){

 |
| 

               @Override

 |
| 

               publicvoidrun(){

 |
| 

                  mLegacyController.processPendingActions(message.mAccountKey);

 |
| 

               }

 |
| 

           }.start();

 |
| 

        }

 |

解决后：

newThread() {

<colgroup style="font-family: Verdana !important;"><col width="1099" style="font-family: Verdana !important;"></colgroup>
| 

        finalMessagemessage=Message.restoreMessageWithId(mProviderContext,messageId);

 |
| 

        if(message==null){

 |
| 

           return;

 |
| 

        }

 |
| 

        Accountaccount=Account.restoreAccountWithId(mProviderContext,message.mAccountKey);

 |
| 

        if(account==null){

 |
| 

           return;//isMessagingController returns false for null, but let's make itclear.

 |
| 

        }

 |
| 

        if(isMessagingController(account)) {

 |
| 

                  mLegacyController.processPendingActions(message.mAccountKey);

 |
|  |
| 

           }

 |

}.start();

关于**AsyncTask:**[**http://developer.android.com/reference/android/os/AsyncTask.html**](http://developer.android.com/reference/android/os/AsyncTask.html)

## 案例2：关键词:在UI线程进行网络数据的读写

ANRin process: com.android.mediascape:PhotoViewer (last incom.android.mediascape:PhotoViewer)
Annotation:keyDispatchingTimedOut
CPU usage:
Load: 6.74 / 6.89 / 6.12
CPUusage from 8254ms to 3224ms ago:
ovider.webmedia: 4% = 4% user +0% kernel / faults: 68 minor
system_server: 2% = 1% user + 0%kernel / faults: 18 minor
re-initialized>: 0% = 0% user + 0%kernel / faults: 50 minor
events/0: 0% = 0% user + 0%kernel **TOTAL:7% = 6% user + 1% kernel** DALVIKTHREADS:
""main"" prio=5 tid=3 NATIVE
|group=""main"" sCount=1 dsCount=0 s=Yobj=0x4001b240 self=0xbda8
| sysTid=2579 nice=0 sched=0/0cgrp=unknown handle=-1343993184
atorg.apache.harmony.luni.platform.OSNetworkSystem.receiveStreamImpl(NativeMethod)
atorg.apache.harmony.luni.platform.**OSNetworkSystem.receiveStream**(OSNetworkSystem.java:478)
atorg.apache.harmony.luni.net.PlainSocketImpl.read(PlainSocketImpl.java:565)
atorg.apache.harmony.luni.net.SocketInputStream.read(SocketInputStream.java:87)
atorg.apache.harmony.luni.internal.net.www.protocol.http.HttpURLConnection$LimitedInputStream.read(HttpURLConnection.java:303)
atjava.io.InputStream.read(InputStream.java:133)
atjava.io.BufferedInputStream.fillbuf(BufferedInputStream.java:157)
atjava.io.BufferedInputStream.read(BufferedInputStream.java:346)
atandroid.graphics.BitmapFactory.nativeDecodeStream(Native Method)
atandroid.graphics.**BitmapFactory.decodeStream**(BitmapFactory.java:459)
atcom.android.mediascape.activity.PhotoViewerActivity.**getPreviewImage**(PhotoViewerActivity.java:4465)
atcom.android.mediascape.activity.PhotoViewerActivity.**dispPreview**(PhotoViewerActivity.java:4406)
atcom.android.mediascape.activity.PhotoViewerActivity.access$6500(PhotoViewerActivity.java:125)  at**com.android.mediascape.activity.PhotoViewerActivity$33$****1.run**(PhotoViewerActivity.java:4558)
atandroid.os.Handler.handleCallback(Handler.java:587)
atandroid.os.Handler.dispatchMessage(Handler.java:92)
atandroid.os.Looper.loop(Looper.java:123)
atandroid.app.ActivityThread.main(ActivityThread.java:4370)
atjava.lang.reflect.Method.invokeNative(Native Method)
atjava.lang.reflect.Method.invoke(Method.java:521)
atcom.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:868)
atcom.android.internal.os.ZygoteInit.main(ZygoteInit.java:626)
atdalvik.system.NativeStart.main(Native Method)

关于网络连接，再设计的时候可以设置个timeout的时间或者放入独立的线程来处理。

关于Handler的问题，可以参考：[**http://developer.android.com/reference/android/os/Handler.html**](http://developer.android.com/reference/android/os/Handler.html)

案例3：

关键词：Memoryleak/Thread leak

11-1621:41:42.560 I/ActivityManager( 1190): ANR in process:android.process.acore (last in android.process.acore)
11-1621:41:42.560 I/ActivityManager( 1190): Annotation:keyDispatchingTimedOut
11-16 21:41:42.560 I/ActivityManager(1190): CPU usage:
11-16 21:41:42.560 I/ActivityManager( 1190):Load: 11.5 / 11.1 / 11.09
11-16 21:41:42.560 I/ActivityManager(1190): CPU usage from 9046ms to 4018ms ago:
11-16 21:41:42.560I/ActivityManager( 1190): **d.process.acore:98%**= 97% user + 0% kernel / faults: 1134 minor
11-16 21:41:42.560I/ActivityManager( 1190): system_server: 0% = 0% user + 0% kernel /faults: 1 minor
11-16 21:41:42.560 I/ActivityManager( 1190): adbd:0% = 0% user + 0% kernel
11-16 21:41:42.560 I/ActivityManager(1190): logcat: 0% = 0% user + 0% kernel
11-16 21:41:42.560I/ActivityManager( 1190): **TOTAL:100% = 98% user + 1% kernel**

Cmdline: android.process.acore

DALVIK THREADS:
"main"prio=5 tid=3 **VMWAIT** |group="main" sCount=1 dsCount=0 s=N obj=0x40026240self=0xbda8
| sysTid=1815 nice=0 sched=0/0 cgrp=unknownhandle=-1344001376
atdalvik.system.**VMRuntime.trackExternalAllocation**(NativeMethod)atandroid.graphics.Bitmap.nativeCreate(Native Method)
atandroid.graphics.**Bitmap.createBitmap**(Bitmap.java:468)
atandroid.view.View.buildDrawingCache(View.java:6324)
atandroid.view.View.getDrawingCache(View.java:6178)
atandroid.view.ViewGroup.drawChild(ViewGroup.java:1541)
……
atcom.android.internal.policy.impl.PhoneWindow$DecorView.draw(PhoneWindow.java:1830)
atandroid.view.ViewRoot.draw(ViewRoot.java:1349)
atandroid.view.ViewRoot.performTraversals(ViewRoot.java:1114)
atandroid.view.ViewRoot.handleMessage(ViewRoot.java:1633)
atandroid.os.Handler.dispatchMessage(Handler.java:99)
atandroid.os.Looper.loop(Looper.java:123)
atandroid.app.ActivityThread.main(ActivityThread.java:4370)
atjava.lang.reflect.Method.invokeNative(Native Method)
atjava.lang.reflect.Method.invoke(Method.java:521)
atcom.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:868)
atcom.android.internal.os.ZygoteInit.main(ZygoteInit.java:626)
atdalvik.system.NativeStart.main(Native Method)

"Thread-408"prio=5 tid=329 WAIT|group="main" sCount=1 dsCount=0 s=N obj=0x46910d40self=0xcd0548
| sysTid=10602 nice=0 sched=0/0 cgrp=unknownhandle=15470792
at java.lang.Object.wait(Native Method)
-waiting on <0x468cd420> (a java.lang.Object)
atjava.lang.Object.wait(Object.java:288)
atcom.android.dialer.CallLogContentHelper$UiUpdaterExecutor$1.run(CallLogContentHelper.java:289)
atjava.lang.Thread.run(Thread.java:1096)

分析：

atdalvik.system.**VMRuntime.trackExternalAllocation**(NativeMethod)内存不足导致block在创建bitmap上

**MEMINFO in pid 1360 [android.process.acore] **
native dalvik other total
size: 17036 **23111** N/A 40147
allocated: 16484 20675 N/A 37159
free: 296 2436 N/A 2732

解决：如果机器的内存族，可以修改虚拟机的内存为36M或更大，不过最好是复查代码，查看哪些内存没有释放

========================================================================================

http://www.eoeandroid.com/forum.php?mod=viewthread&tid=165974

Log的产生大家都知道 ， 大家也都知道通过DDMS来看log ， 但什么时候会产生log文件呢 ？一般在如下几种情况会产生log文件 。
1，程序异常退出 ， uncaused exception
2，程序强制关闭 ，Force Closed (简称FC)
3，程序无响应 ， Application No Response （简称ANR) ， 顺便，一般主线程超过5秒么有处理就会ANR
4，手动生成 。   
拿到一个日志文件，要分成多段来看 。 log文件很长，其中包含十几个小单元信息，但不要被吓到 ，事实上他主要由三大块儿组成 。 1，系统基本信息 ，包括 内存，CPU ，进程队列 ，虚拟内存 ， 垃圾回收等信息 。------ MEMORY INFO (/proc/meminfo) ------
------ CPU INFO (top -n 1 -d 1 -m 30 -t) ------
------ PROCRANK (procrank) ------
------ VIRTUAL MEMORY STATS (/proc/vmstat) ------
------ VMALLOC INFO (/proc/vmallocinfo) ------

格式如下：
------ MEMORY INFO (/proc/meminfo) ------
MemTotal:         347076 kB
MemFree:           56408 kB
Buffers:            7192 kB
Cached:           104064 kB
SwapCached:            0 kB
Active:           192592 kB
Inactive:          40548 kB
Active(anon):     129040 kB
Inactive(anon):     1104 kB
Active(file):      63552 kB
Inactive(file):    39444 kB
Unevictable:        7112 kB
Mlocked:               0 kB
SwapTotal:             0 kB
SwapFree:              0 kB
Dirty:                44 kB
Writeback:             0 kB
AnonPages:        129028 kB
Mapped:            73728 kB
Shmem:              1148 kB
Slab:              13072 kB
SReclaimable:       4564 kB
SUnreclaim:         8508 kB
KernelStack:        3472 kB
PageTables:        12172 kB
NFS_Unstable:          0 kB
Bounce:                0 kB
WritebackTmp:          0 kB
CommitLimit:      173536 kB
Committed_AS:    7394524 kB
VmallocTotal:     319488 kB
VmallocUsed:       90752 kB
VmallocChunk:     181252 kB

2，时间信息 ， 也是我们主要分析的信息 。
------ VMALLOC INFO (/proc/vmallocinfo) ------
------ EVENT INFO (/proc/vmallocinfo) ------

格式如下：
------ SYSTEM LOG (logcat -b system -v time -d *:v) ------
01-15 16:41:43.671 W/PackageManager( 2466): Unknown permission com.wsomacp.permission.PROVIDER in package com.android.mms
01-15 16:41:43.671 I/ActivityManager( 2466): Force stopping package com.android.mms uid=10092
01-15 16:41:43.675 I/UsageStats( 2466): Something wrong here, didn't expect com.sec.android.app.twlauncher to be paused
01-15 16:41:44.108 I/ActivityManager( 2466): Start proc com.sec.android.widgetapp.infoalarm for service com.sec.android.widgetapp.infoalarm/.engine.DataService: pid=20634 uid=10005 gids={3003, 1015, 3002}
01-15 16:41:44.175 W/ActivityManager( 2466): Activity pause timeout for HistoryRecord{48589868 com.sec.android.app.twlauncher/.Launcher}
01-15 16:41:50.864 I/KeyInputQueue( 2466): Input event
01-15 16:41:50.866 D/KeyInputQueue( 2466): screenCaptureKeyFlag setting 0
01-15 16:41:50.882 I/PowerManagerService( 2466): Ulight 0->7|0
01-15 16:41:50.882 I/PowerManagerService( 2466): Setting target 2: cur=0.0 target=70 delta=4.6666665 nominalCurrentValue=0
01-15 16:41:50.882 I/PowerManagerService( 2466): Scheduling light animator!
01-15 16:41:51.706 D/PowerManagerService( 2466): enableLightSensor true
01-15 16:41:51.929 I/KeyInputQueue( 2466): Input event
01-15 16:41:51.933 W/WindowManager( 2466): No focus window, dropping: KeyEvent{action=0 code=26 repeat=0 meta=0 scancode=26 mFlags=9}

3，虚拟机信息 ， 包括进程的，线程的跟踪信息，这是用来跟踪进程和线程具体点的好地方 。 
------ VM TRACES JUST NOW (/data/anr/traces.txt.bugreport: 2011-01-15 16:49:02) ------
------ VM TRACES AT LAST ANR (/data/anr/traces.txt: 2011-01-15 16:49:02) ------

格式如下 ：
----- pid 21161 at 2011-01-15 16:49:01 -----
Cmd line: com.android.mms

DALVIK THREADS:
"main" prio=5 tid=1 NATIVE
  | group="main" sCount=1 dsCount=0 s=N obj=0x4001d8d0 self=0xccc8
  | sysTid=21161 nice=0 sched=0/0 cgrp=default handle=-1345017808
  | schedstat=( 4151552996 5342265329 10995 )
  at android.media.MediaPlayer._reset(Native Method)
  at android.media.MediaPlayer.reset(MediaPlayer.java:1218)
  at android.widget.VideoView.release(VideoView.java:499)
  at android.widget.VideoView.access$2100(VideoView.java:50)
  at android.widget.VideoView$6.surfaceDestroyed(VideoView.java:489)
  at android.view.SurfaceView.reportSurfaceDestroyed(SurfaceView.java:572)
  at android.view.SurfaceView.updateWindow(SurfaceView.java:476)
  at android.view.SurfaceView.onWindowVisibilityChanged(SurfaceView.java:206)
  at android.view.View.dispatchDetachedFromWindow(View.java:6082)
  at android.view.ViewGroup.dispatchDetachedFromWindow(ViewGroup.java:1156)
  at android.view.ViewGroup.removeAllViewsInLayout(ViewGroup.java:2296)
  at android.view.ViewGroup.removeAllViews(ViewGroup.java:2254)
  at com.android.mms.ui.SlideView.reset(SlideView.java:687)
  at com.android.mms.ui.SlideshowPresenter.presentSlide(SlideshowPresenter.java:189)
  at com.android.mms.ui.SlideshowPresenter$3.run(SlideshowPresenter.java:531)
  at android.os.Handler.handleCallback(Handler.java:587)
  at android.os.Handler.dispatchMessage(Handler.java:92)
  at android.os.Looper.loop(Looper.java:123)
  at android.app.ActivityThread.main(ActivityThread.java:4627)
  at java.lang.reflect.Method.invokeNative(Native Method)
  at java.lang.reflect.Method.invoke(Method.java:521)
  at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:858)
  at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:616)
  at dalvik.system.NativeStart.main(Native Method)

---------------------------------------------------------------------------------------------------------------------------------------
闲话少说， 我总结了观察log文件的基本步骤 。 1，如果是ANR问题 ， 则搜索“ANR”关键词 。 快速定位到关键事件信息 。
2，如果是ForceClosed 和其它异常退出信息，则搜索"Fatal" 关键词， 快速定位到关键事件信息 。
3，定位到关键事件信息后 ， 如果信息不够明确的，再去搜索应用程序包的虚拟机信息 ，查看具体的进程和线程跟踪的日志，来定位到代码 。 

用这种方法，出现问题，根本不需要断点调试 ， 直接定位到问题，屡试不爽 。 
下面，我们就开始来分析这个例子的log 。

打开log文件 ， 由于是ANR错误，因此搜索"ANR " ， 为何要加空格呢，你加上和去掉比较一下就知道了 。 可以屏蔽掉不少保存到anr.log文件的无效信息 。 

定位到关键的事件信息如下：
01-15 16:49:02.433 E/ActivityManager( 2466): ANR in com.android.mms (com.android.mms/.ui.SlideshowActivity)
01-15 16:49:02.433 E/ActivityManager( 2466): Reason: keyDispatchingTimedOut
01-15 16:49:02.433 E/ActivityManager( 2466): Load: 0.6 / 0.61 / 0.42
01-15 16:49:02.433 E/ActivityManager( 2466): CPU usage from 1337225ms to 57ms ago:
01-15 16:49:02.433 E/ActivityManager( 2466):   sensorserver_ya: 8% = 0% user + 8% kernel / faults: 40 minor
......

01-15 16:49:02.433 E/ActivityManager( 2466):  -com.android.mms: 0% = 0% user + 0% kernel
01-15 16:49:02.433 E/ActivityManager( 2466):  -flush-179:8: 0% = 0% user + 0% kernel
01-15 16:49:02.433 E/ActivityManager( 2466): TOTAL: 25% = 10% user + 14% kernel + 0% iowait + 0% irq + 0% softirq
01-15 16:49:02.436 I/        ( 2466): dumpmesg > "/data/log/dumpstate_app_anr.log"

我们用自然语言来描述一下日志，这也算是一种能力吧 。 
01-15 16:49:02.433 E/ActivityManager( 2466): ANR in com.android.mms (com.android.mms/.ui.SlideshowActivity)
翻译：在16:49分2秒433毫秒的时候 ActivityManager （进程号为2466) 发生了如下错误：com.android.mms包下面的.ui.SlideshowActivity 无响应 。

01-15 16:49:02.433 E/ActivityManager( 2466): Reason: keyDispatchingTimedOut
翻译：原因 ， keyDispatchingTimeOut - 按键分配超时 

01-15 16:49:02.433 E/ActivityManager( 2466): Load: 0.6 / 0.61 / 0.42
翻译：5分钟，10分钟，15分钟内的平均负载分别为：0.6 , 0.61 , 0.42

在这里我们大概知道问题是什么了，结合我们之前的操作流程，我们知道问题是在点击按钮某时候可能处理不过来按钮事件，导致超时无响应 。那么现在似乎已经可以进行工作了 。 我们知道Activity中是通过重载dispatchTouchEvent(MotionEvent ev)来处理点击屏幕事件  。 然后我们可以顺藤摸瓜，一点点分析去查找原因 。 但这样够了么 ？
其实不够 ， 至少我们不能准确的知道到底问题在哪儿 ， 只是猜测 ，比如这个应用程序中，我就在顺藤摸瓜的时候发现了多个IO操作的地方都在主线程中，可能引起问题，但不好判断到底是哪个  ，所以我们目前掌握的信息还不够 。 

于是我们再分析虚拟机信息 ， 搜索“Dalvik Thread”关键词，快速定位到本应用程序的虚拟机信息日志，如下：
----- pid 2922 at 2011-01-13 13:51:07 -----
Cmd line: com.android.mms

DALVIK THREADS:
"main" prio=5 tid=1 NATIVE
  | group="main" sCount=1 dsCount=0 s=N obj=0x4001d8d0 self=0xccc8
  | sysTid=2922 nice=0 sched=0/0 cgrp=default handle=-1345017808
  | schedstat=( 3497492306 15312897923 10358 )
  at android.media.MediaPlayer._release(Native Method)
  at android.media.MediaPlayer.release(MediaPlayer.java:1206)
  at android.widget.VideoView.stopPlayback(VideoView.java:196)
  at com.android.mms.ui.SlideView.stopVideo(SlideView.java:640)
  at com.android.mms.ui.SlideshowPresenter.presentVideo(SlideshowPresenter.java:443)
  at com.android.mms.ui.SlideshowPresenter.presentRegionMedia(SlideshowPresenter.java:219)
  at com.android.mms.ui.SlideshowPresenter$4.run(SlideshowPresenter.java:516)
  at android.os.Handler.handleCallback(Handler.java:587)
  at android.os.Handler.dispatchMessage(Handler.java:92)
  at android.os.Looper.loop(Looper.java:123)
  at android.app.ActivityThread.main(ActivityThread.java:4627)
  at java.lang.reflect.Method.invokeNative(Native Method)
  at java.lang.reflect.Method.invoke(Method.java:521)
  at com.android.internal.os.ZygoteInit$MethodAndArgsCaller.run(ZygoteInit.java:858)
  at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:616)
  at dalvik.system.NativeStart.main(Native Method)

"Binder Thread #3" prio=5 tid=11 NATIVE
  | group="main" sCount=1 dsCount=0 s=N obj=0x4837f808 self=0x242280
  | sysTid=3239 nice=0 sched=0/0 cgrp=default handle=2341032
  | schedstat=( 32410506 932842514 164 )
  at dalvik.system.NativeStart.run(Native Method)

"AsyncQueryWorker" prio=5 tid=9 WAIT
  | group="main" sCount=1 dsCount=0 s=N obj=0x482f4b80 self=0x253e10
  | sysTid=3236 nice=0 sched=0/0 cgrp=default handle=2432120
  | schedstat=( 3225061 26561350 27 )
  at java.lang.Object.wait(Native Method)
  - waiting on <0x482f4da8> (a android.os.MessageQueue)
  at java.lang.Object.wait(Object.java:288)
  at android.os.MessageQueue.next(MessageQueue.java:146)
  at android.os.Looper.loop(Looper.java:110)
  at android.os.HandlerThread.run(HandlerThread.java:60)

"Thread-9" prio=5 tid=8 WAIT
  | group="main" sCount=1 dsCount=0 s=N obj=0x4836e2b0 self=0x25af70
  | sysTid=2929 nice=0 sched=0/0 cgrp=default handle=2370896
  | schedstat=( 130248 4389035 2 )
  at java.lang.Object.wait(Native Method)
  - waiting on <0x4836e240> (a java.util.ArrayList)
  at java.lang.Object.wait(Object.java:288)
  at com.android.mms.data.Contact$ContactsCache$TaskStack$1.run(Contact.java:488)
  at java.lang.Thread.run(Thread.java:1096)

"Binder Thread #2" prio=5 tid=7 NATIVE
  | group="main" sCount=1 dsCount=0 s=N obj=0x482f8ca0 self=0x130fd0
  | sysTid=2928 nice=0 sched=0/0 cgrp=default handle=1215968
  | schedstat=( 40610049 1837703846 195 )
  at dalvik.system.NativeStart.run(Native Method)

"Binder Thread #1" prio=5 tid=6 NATIVE
  | group="main" sCount=1 dsCount=0 s=N obj=0x482f4a78 self=0x128a50
  | sysTid=2927 nice=0 sched=0/0 cgrp=default handle=1201352
  | schedstat=( 40928066 928867585 190 )
  at dalvik.system.NativeStart.run(Native Method)

"Compiler" daemon prio=5 tid=5 VMWAIT
  | group="system" sCount=1 dsCount=0 s=N obj=0x482f1348 self=0x118960
  | sysTid=2926 nice=0 sched=0/0 cgrp=default handle=1149216
  | schedstat=( 753021350 3774113668 6686 )
  at dalvik.system.NativeStart.run(Native Method)

"JDWP" daemon prio=5 tid=4 VMWAIT
  | group="system" sCount=1 dsCount=0 s=N obj=0x482f12a0 self=0x132940
  | sysTid=2925 nice=0 sched=0/0 cgrp=default handle=1255680
  | schedstat=( 2827103 29553323 19 )
  at dalvik.system.NativeStart.run(Native Method)

"Signal Catcher" daemon prio=5 tid=3 RUNNABLE
  | group="system" sCount=0 dsCount=0 s=N obj=0x482f11e8 self=0x135988
  | sysTid=2924 nice=0 sched=0/0 cgrp=default handle=1173688
  | schedstat=( 11793815 12456169 7 )
  at dalvik.system.NativeStart.run(Native Method)

"HeapWorker" daemon prio=5 tid=2 VMWAIT
  | group="system" sCount=1 dsCount=0 s=N obj=0x45496028 self=0x135848
  | sysTid=2923 nice=0 sched=0/0 cgrp=default handle=1222608
  | schedstat=( 79049792 1520840200 95 )
  at dalvik.system.NativeStart.run(Native Method)

----- end 2922 -----

每一段都是一个线程 ，当然我们还是看线程号为1的主线程了。通过分析发现关键问题是这样：
  at com.android.mms.ui.SlideshowPresenter$3.run(SlideshowPresenter.java:531)
定位到代码：
mHandler.post(new Runnable() {
                    public void run() {
                        try {
                            presentRegionMedia(view, (RegionMediaModel) model, dataChanged);
                        } catch (OMADRMException e) {
                            Log.e(TAG, e.getMessage(), e);
                            Toast.makeText(mContext,
                                    mContext.getString(R.string.insufficient_drm_rights),
                                    Toast.LENGTH_SHORT).show();
                        } catch (IOException e){
                            Log.e(TAG, e.getMessage(), e);
                            Toast.makeText(mContext,
                                    mContext.getString(R.string.insufficient_drm_rights),
                                    Toast.LENGTH_SHORT).show();

                        }
                    }

很清楚了， Handler.post 方法之后执行时间太长的问题 。 继续看presentRegionMedia(view, (RegionMediaModel) model, dataChanged);方法 ， 发现最终是调用的framework 中MediaPlayer.stop方法 。
至此，我们的日志分析算是告一段落 。 可以开始思考解决办法了



引用：
[Android App优化之ANR详解](https://www.jianshu.com/p/6d855e984b99)
如何分析解决Android ANR



