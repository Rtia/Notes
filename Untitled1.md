# Android 源码分析之旅3.1--消息机制源码分析

[![96](http://upload-images.jianshu.io/upload_images/9028834-74dff67b4b793abc?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](https://www.jianshu.com/u/70c12759d4fe)  

[小楠总](https://www.jianshu.com/u/70c12759d4fe) 关注

2017.03.03 18:06* 字数 5020 阅读 1196评论 13喜欢 21

本篇文章已授权微信公众号 guolin_blog （郭霖）独家发布
本人小楠——一位励志的Android开发者。

### 前言

在分析Application Framework的时候，经常会看到Handler的使用，尤其见得最多的是“H”这个系统Handler的使用。因此有必要先学习Android中的消息机制。

### 应用程序的入口分析

应用程序的入口是在ActivityThread的main方法中的（当应用程序启动的时候，会通过底层的C/C++去调用main方法），这个方法在ActivityThread类的最后一个函数里面，核心代码如下：

```
public static void main(String[] args) {

    Environment.initForCurrentUser();

    Looper.prepareMainLooper();

    ActivityThread thread = new ActivityThread();
    thread.attach(false);

    if (sMainThreadHandler == null) {
        sMainThreadHandler = thread.getHandler();
    }
    Looper.loop();

}

```

###### 在分析源码的时候，你可能会发现一些if(false){}之类的语句，这种写法是方便调试的，通过一个标志就可以控制某些代码是否执行，比如说是否输出一些系统的Log。

在main方法里面，首先初始化了我们的Environment对象，然后创建了Looper，然后开启消息循环。根据我们的常识知道，如果程序没有死循环的话，执行完main函数（比如构建视图等等代码）以后就会立马退出了。之所以我们的APP能够一直运行着，就是因为Looper.loop()里面是一个死循环：

```
public static void loop() {
    for (;;) {
    }
}

```

###### 这里有一个小小的知识，就是之所以用for (;;)而不是用while(true)是因为防止一些人通过黑科技去修改这个循环的标志（比如通过反射的方式）

###### 在非主线程里面我们也可以搞一个Handler，但是需要我们主动去为当前的子线程绑定一个Looper，并且启动消息循环。

Looper主要有两个核心的方法，一是prepare，而是开始loop循环。
通过Looper、Handler、Message、MessageQueue等组成了Android的消息处理机制，也叫事件、反馈机制。

### 为什么需要这样一个消息机制？

我们知道每一个应用程序都有一个主线程，主线程一直循环的话，那么我们的自己的代码就无法执行了。而系统在主线程绑定一个Looper循环器以及消息队列，Looper就像是一个水泵一样不断把消息发送到主线程。如果没有消息机制，我们的代码需要直接与主线程进行访问，操作，切换，访问主线程的变量等等，这样做会带来不安全的问题，另外APP的开发的难度也会提高，同时也不利于整个Android系统的运作。有了消息机制，我们可以简单地通过发送消息，然后Looper把消息发送到主线程，然后就可以执行了。

消息其中包括：
我们自己的操作消息（客户端的Handler）
系统的操作消息（系统Handler）：比如启动Activity等四大组件（例如突然来电话的时候跳转到电话界面）
我们的思路是先分析系统的Handler，然后再去深入理解消息机制里面各个部件。

![image](http://upload-images.jianshu.io/upload_images/9028834-f3a48e287efebb17?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

举个例子，广播：AMS发送消息到MessageQueue，然后Looper循环，系统的Handler取出来以后才处理。（AMS是处理四大组件的生命周期的一个比较重要的类，在以后我们分析IPC机制以及Activity启动流程的时候会提到）

### 系统的Handler在哪里？

在ActivityThread的成员变量里面有一个这样的大H（继承Handler），这个就是系统的Handler：

```
final H mH = new H();

```

回顾一下ActivityThread的main方法可以知道，在new ActivityThread的时候，系统的Handler就就初始化了，这是一种饿加载的方法，也就是在类被new的时候就初始化成员变量了。另外还有一种懒加载，就是在需要的时候才去初始化，这两种方式在单例设计模式里面比较常见。

```
public static void main(String[] args) {

    Environment.initForCurrentUser();

    Looper.prepareMainLooper();

    //new 的时候已经把成员变量Handler初始化了
    ActivityThread thread = new ActivityThread();
    thread.attach(false);

    if (sMainThreadHandler == null) {
        sMainThreadHandler = thread.getHandler();
    }
    Looper.loop();

}

```

下面看系统Handler的定义（看的时候可以跳过一些case，粗略地看即可）：

```
    private class H extends Handler {
    public static final int LAUNCH_ACTIVITY         = 100;
    public static final int PAUSE_ACTIVITY          = 101;
    public static final int PAUSE_ACTIVITY_FINISHING= 102;
    public static final int STOP_ACTIVITY_SHOW      = 103;
    public static final int STOP_ACTIVITY_HIDE      = 104;
    public static final int SHOW_WINDOW             = 105;
    public static final int HIDE_WINDOW             = 106;
    public static final int RESUME_ACTIVITY         = 107;
    public static final int SEND_RESULT             = 108;
    public static final int DESTROY_ACTIVITY        = 109;
    public static final int BIND_APPLICATION        = 110;
    public static final int EXIT_APPLICATION        = 111;
    public static final int NEW_INTENT              = 112;
    public static final int RECEIVER                = 113;
    public static final int CREATE_SERVICE          = 114;
    public static final int SERVICE_ARGS            = 115;
    public static final int STOP_SERVICE            = 116;

    public static final int CONFIGURATION_CHANGED   = 118;
    public static final int CLEAN_UP_CONTEXT        = 119;
    public static final int GC_WHEN_IDLE            = 120;
    public static final int BIND_SERVICE            = 121;
    public static final int UNBIND_SERVICE          = 122;
    public static final int DUMP_SERVICE            = 123;
    public static final int LOW_MEMORY              = 124;
    public static final int ACTIVITY_CONFIGURATION_CHANGED = 125;
    public static final int RELAUNCH_ACTIVITY       = 126;
    public static final int PROFILER_CONTROL        = 127;
    public static final int CREATE_BACKUP_AGENT     = 128;
    public static final int DESTROY_BACKUP_AGENT    = 129;
    public static final int SUICIDE                 = 130;
    public static final int REMOVE_PROVIDER         = 131;
    public static final int ENABLE_JIT              = 132;
    public static final int DISPATCH_PACKAGE_BROADCAST = 133;
    public static final int SCHEDULE_CRASH          = 134;
    public static final int DUMP_HEAP               = 135;
    public static final int DUMP_ACTIVITY           = 136;
    public static final int SLEEPING                = 137;
    public static final int SET_CORE_SETTINGS       = 138;
    public static final int UPDATE_PACKAGE_COMPATIBILITY_INFO = 139;
    public static final int TRIM_MEMORY             = 140;
    public static final int DUMP_PROVIDER           = 141;
    public static final int UNSTABLE_PROVIDER_DIED  = 142;
    public static final int REQUEST_ASSIST_CONTEXT_EXTRAS = 143;
    public static final int TRANSLUCENT_CONVERSION_COMPLETE = 144;
    public static final int INSTALL_PROVIDER        = 145;
    public static final int ON_NEW_ACTIVITY_OPTIONS = 146;
    public static final int CANCEL_VISIBLE_BEHIND = 147;
    public static final int BACKGROUND_VISIBLE_BEHIND_CHANGED = 148;
    public static final int ENTER_ANIMATION_COMPLETE = 149;
    public static final int START_BINDER_TRACKING = 150;
    public static final int STOP_BINDER_TRACKING_AND_DUMP = 151;
    public static final int MULTI_WINDOW_MODE_CHANGED = 152;
    public static final int PICTURE_IN_PICTURE_MODE_CHANGED = 153;
    public static final int LOCAL_VOICE_INTERACTION_STARTED = 154;

    public void handleMessage(Message msg) {
        if (DEBUG_MESSAGES) Slog.v(TAG, ">>> handling: " + codeToString(msg.what));
        switch (msg.what) {
            case LAUNCH_ACTIVITY: {
                Trace.traceBegin(Trace.TRACE_TAG_ACTIVITY_MANAGER, "activityStart");
                final ActivityClientRecord r = (ActivityClientRecord) msg.obj;

                r.packageInfo = getPackageInfoNoCheck(
                        r.activityInfo.applicationInfo, r.compatInfo);
                handleLaunchActivity(r, null, "LAUNCH_ACTIVITY");
                Trace.traceEnd(Trace.TRACE_TAG_ACTIVITY_MANAGER);
            } break;
            case RELAUNCH_ACTIVITY: {
                Trace.traceBegin(Trace.TRACE_TAG_ACTIVITY_MANAGER, "activityRestart");
                ActivityClientRecord r = (ActivityClientRecord)msg.obj;
                handleRelaunchActivity(r);
                Trace.traceEnd(Trace.TRACE_TAG_ACTIVITY_MANAGER);
            } break;
            case PAUSE_ACTIVITY: {
                Trace.traceBegin(Trace.TRACE_TAG_ACTIVITY_MANAGER, "activityPause");
                SomeArgs args = (SomeArgs) msg.obj;
                handlePauseActivity((IBinder) args.arg1, false,
                        (args.argi1 & USER_LEAVING) != 0, args.argi2,
                        (args.argi1 & DONT_REPORT) != 0, args.argi3);
                maybeSnapshot();
                Trace.traceEnd(Trace.TRACE_TAG_ACTIVITY_MANAGER);
            } break;
            case PAUSE_ACTIVITY_FINISHING: {
                Trace.traceBegin(Trace.TRACE_TAG_ACTIVITY_MANAGER, "activityPause");
                SomeArgs args = (SomeArgs) msg.obj;
                handlePauseActivity((IBinder) args.arg1, true, (args.argi1 & USER_LEAVING) != 0,
                        args.argi2, (args.argi1 & DONT_REPORT) != 0, args.argi3);
                Trace.traceEnd(Trace.TRACE_TAG_ACTIVITY_MANAGER);
            } break;
            case STOP_ACTIVITY_SHOW: {
                Trace.traceBegin(Trace.TRACE_TAG_ACTIVITY_MANAGER, "activityStop");
                SomeArgs args = (SomeArgs) msg.obj;
                handleStopActivity((IBinder) args.arg1, true, args.argi2, args.argi3);
                Trace.traceEnd(Trace.TRACE_TAG_ACTIVITY_MANAGER);
            } break;
            case STOP_ACTIVITY_HIDE: {
                Trace.traceBegin(Trace.TRACE_TAG_ACTIVITY_MANAGER, "activityStop");
                SomeArgs args = (SomeArgs) msg.obj;
                handleStopActivity((IBinder) args.arg1, false, args.argi2, args.argi3);
                Trace.traceEnd(Trace.TRACE_TAG_ACTIVITY_MANAGER);
            } break;
            case SHOW_WINDOW:
                Trace.traceBegin(Trace.TRACE_TAG_ACTIVITY_MANAGER, "activityShowWindow");
                handleWindowVisibility((IBinder)msg.obj, true);
                Trace.traceEnd(Trace.TRACE_TAG_ACTIVITY_MANAGER);
                break;
            case HIDE_WINDOW:
                Trace.traceBegin(Trace.TRACE_TAG_ACTIVITY_MANAGER, "activityHideWindow");
                handleWindowVisibility((IBinder)msg.obj, false);
                Trace.traceEnd(Trace.TRACE_TAG_ACTIVITY_MANAGER);
                break;
            case RESUME_ACTIVITY:
                Trace.traceBegin(Trace.TRACE_TAG_ACTIVITY_MANAGER, "activityResume");
                SomeArgs args = (SomeArgs) msg.obj;
                handleResumeActivity((IBinder) args.arg1, true, args.argi1 != 0, true,
                        args.argi3, "RESUME_ACTIVITY");
                Trace.traceEnd(Trace.TRACE_TAG_ACTIVITY_MANAGER);
                break;
            case SEND_RESULT:
                Trace.traceBegin(Trace.TRACE_TAG_ACTIVITY_MANAGER, "activityDeliverResult");
                handleSendResult((ResultData)msg.obj);
                Trace.traceEnd(Trace.TRACE_TAG_ACTIVITY_MANAGER);
                break;
            case DESTROY_ACTIVITY:
                Trace.traceBegin(Trace.TRACE_TAG_ACTIVITY_MANAGER, "activityDestroy");
                handleDestroyActivity((IBinder)msg.obj, msg.arg1 != 0,
                        msg.arg2, false);
                Trace.traceEnd(Trace.TRACE_TAG_ACTIVITY_MANAGER);
                break;
            case BIND_APPLICATION:
                Trace.traceBegin(Trace.TRACE_TAG_ACTIVITY_MANAGER, "bindApplication");
                AppBindData data = (AppBindData)msg.obj;
                handleBindApplication(data);
                Trace.traceEnd(Trace.TRACE_TAG_ACTIVITY_MANAGER);
                break;
            case EXIT_APPLICATION:
                if (mInitialApplication != null) {
                    mInitialApplication.onTerminate();
                }
                Looper.myLooper().quit();
                break;
            case NEW_INTENT:
                Trace.traceBegin(Trace.TRACE_TAG_ACTIVITY_MANAGER, "activityNewIntent");
                handleNewIntent((NewIntentData)msg.obj);
                Trace.traceEnd(Trace.TRACE_TAG_ACTIVITY_MANAGER);
                break;
            case RECEIVER:
                Trace.traceBegin(Trace.TRACE_TAG_ACTIVITY_MANAGER, "broadcastReceiveComp");
                handleReceiver((ReceiverData)msg.obj);
                maybeSnapshot();
                Trace.traceEnd(Trace.TRACE_TAG_ACTIVITY_MANAGER);
                break;
            case CREATE_SERVICE:
                Trace.traceBegin(Trace.TRACE_TAG_ACTIVITY_MANAGER, ("serviceCreate: " + String.valueOf(msg.obj)));
                handleCreateService((CreateServiceData)msg.obj);
                Trace.traceEnd(Trace.TRACE_TAG_ACTIVITY_MANAGER);
                break;
            case BIND_SERVICE:
                Trace.traceBegin(Trace.TRACE_TAG_ACTIVITY_MANAGER, "serviceBind");
                handleBindService((BindServiceData)msg.obj);
                Trace.traceEnd(Trace.TRACE_TAG_ACTIVITY_MANAGER);
                break;
            case UNBIND_SERVICE:
                Trace.traceBegin(Trace.TRACE_TAG_ACTIVITY_MANAGER, "serviceUnbind");
                handleUnbindService((BindServiceData)msg.obj);
                Trace.traceEnd(Trace.TRACE_TAG_ACTIVITY_MANAGER);
                break;
            case SERVICE_ARGS:
                Trace.traceBegin(Trace.TRACE_TAG_ACTIVITY_MANAGER, ("serviceStart: " + String.valueOf(msg.obj)));
                handleServiceArgs((ServiceArgsData)msg.obj);
                Trace.traceEnd(Trace.TRACE_TAG_ACTIVITY_MANAGER);
                break;
            case STOP_SERVICE:
                Trace.traceBegin(Trace.TRACE_TAG_ACTIVITY_MANAGER, "serviceStop");
                handleStopService((IBinder)msg.obj);
                maybeSnapshot();
                Trace.traceEnd(Trace.TRACE_TAG_ACTIVITY_MANAGER);
                break;
            case CONFIGURATION_CHANGED:
                Trace.traceBegin(Trace.TRACE_TAG_ACTIVITY_MANAGER, "configChanged");
                mCurDefaultDisplayDpi = ((Configuration)msg.obj).densityDpi;
                handleConfigurationChanged((Configuration)msg.obj, null);
                Trace.traceEnd(Trace.TRACE_TAG_ACTIVITY_MANAGER);
                break;
            case CLEAN_UP_CONTEXT:
                ContextCleanupInfo cci = (ContextCleanupInfo)msg.obj;
                cci.context.performFinalCleanup(cci.who, cci.what);
                break;
            case GC_WHEN_IDLE:
                scheduleGcIdler();
                break;
            case DUMP_SERVICE:
                handleDumpService((DumpComponentInfo)msg.obj);
                break;
            case LOW_MEMORY:
                Trace.traceBegin(Trace.TRACE_TAG_ACTIVITY_MANAGER, "lowMemory");
                handleLowMemory();
                Trace.traceEnd(Trace.TRACE_TAG_ACTIVITY_MANAGER);
                break;
            case ACTIVITY_CONFIGURATION_CHANGED:
                Trace.traceBegin(Trace.TRACE_TAG_ACTIVITY_MANAGER, "activityConfigChanged");
                handleActivityConfigurationChanged((ActivityConfigChangeData) msg.obj,
                        msg.arg1 == 1 ? REPORT_TO_ACTIVITY : !REPORT_TO_ACTIVITY);
                Trace.traceEnd(Trace.TRACE_TAG_ACTIVITY_MANAGER);
                break;
            case PROFILER_CONTROL:
                handleProfilerControl(msg.arg1 != 0, (ProfilerInfo)msg.obj, msg.arg2);
                break;
            case CREATE_BACKUP_AGENT:
                Trace.traceBegin(Trace.TRACE_TAG_ACTIVITY_MANAGER, "backupCreateAgent");
                handleCreateBackupAgent((CreateBackupAgentData)msg.obj);
                Trace.traceEnd(Trace.TRACE_TAG_ACTIVITY_MANAGER);
                break;
            case DESTROY_BACKUP_AGENT:
                Trace.traceBegin(Trace.TRACE_TAG_ACTIVITY_MANAGER, "backupDestroyAgent");
                handleDestroyBackupAgent((CreateBackupAgentData)msg.obj);
                Trace.traceEnd(Trace.TRACE_TAG_ACTIVITY_MANAGER);
                break;
            case SUICIDE:
                Process.killProcess(Process.myPid());
                break;
            case REMOVE_PROVIDER:
                Trace.traceBegin(Trace.TRACE_TAG_ACTIVITY_MANAGER, "providerRemove");
                completeRemoveProvider((ProviderRefCount)msg.obj);
                Trace.traceEnd(Trace.TRACE_TAG_ACTIVITY_MANAGER);
                break;
            case ENABLE_JIT:
                ensureJitEnabled();
                break;
            case DISPATCH_PACKAGE_BROADCAST:
                Trace.traceBegin(Trace.TRACE_TAG_ACTIVITY_MANAGER, "broadcastPackage");
                handleDispatchPackageBroadcast(msg.arg1, (String[])msg.obj);
                Trace.traceEnd(Trace.TRACE_TAG_ACTIVITY_MANAGER);
                break;
            case SCHEDULE_CRASH:
                throw new RemoteServiceException((String)msg.obj);
            case DUMP_HEAP:
                handleDumpHeap(msg.arg1 != 0, (DumpHeapData)msg.obj);
                break;
            case DUMP_ACTIVITY:
                handleDumpActivity((DumpComponentInfo)msg.obj);
                break;
            case DUMP_PROVIDER:
                handleDumpProvider((DumpComponentInfo)msg.obj);
                break;
            case SLEEPING:
                Trace.traceBegin(Trace.TRACE_TAG_ACTIVITY_MANAGER, "sleeping");
                handleSleeping((IBinder)msg.obj, msg.arg1 != 0);
                Trace.traceEnd(Trace.TRACE_TAG_ACTIVITY_MANAGER);
                break;
            case SET_CORE_SETTINGS:
                Trace.traceBegin(Trace.TRACE_TAG_ACTIVITY_MANAGER, "setCoreSettings");
                handleSetCoreSettings((Bundle) msg.obj);
                Trace.traceEnd(Trace.TRACE_TAG_ACTIVITY_MANAGER);
                break;
            case UPDATE_PACKAGE_COMPATIBILITY_INFO:
                handleUpdatePackageCompatibilityInfo((UpdateCompatibilityData)msg.obj);
                break;
            case TRIM_MEMORY:
                Trace.traceBegin(Trace.TRACE_TAG_ACTIVITY_MANAGER, "trimMemory");
                handleTrimMemory(msg.arg1);
                Trace.traceEnd(Trace.TRACE_TAG_ACTIVITY_MANAGER);
                break;
            case UNSTABLE_PROVIDER_DIED:
                handleUnstableProviderDied((IBinder)msg.obj, false);
                break;
            case REQUEST_ASSIST_CONTEXT_EXTRAS:
                handleRequestAssistContextExtras((RequestAssistContextExtras)msg.obj);
                break;
            case TRANSLUCENT_CONVERSION_COMPLETE:
                handleTranslucentConversionComplete((IBinder)msg.obj, msg.arg1 == 1);
                break;
            case INSTALL_PROVIDER:
                handleInstallProvider((ProviderInfo) msg.obj);
                break;
            case ON_NEW_ACTIVITY_OPTIONS:
                Pair<IBinder, ActivityOptions> pair = (Pair<IBinder, ActivityOptions>) msg.obj;
                onNewActivityOptions(pair.first, pair.second);
                break;
            case CANCEL_VISIBLE_BEHIND:
                handleCancelVisibleBehind((IBinder) msg.obj);
                break;
            case BACKGROUND_VISIBLE_BEHIND_CHANGED:
                handleOnBackgroundVisibleBehindChanged((IBinder) msg.obj, msg.arg1 > 0);
                break;
            case ENTER_ANIMATION_COMPLETE:
                handleEnterAnimationComplete((IBinder) msg.obj);
                break;
            case START_BINDER_TRACKING:
                handleStartBinderTracking();
                break;
            case STOP_BINDER_TRACKING_AND_DUMP:
                handleStopBinderTrackingAndDump((ParcelFileDescriptor) msg.obj);
                break;
            case MULTI_WINDOW_MODE_CHANGED:
                handleMultiWindowModeChanged((IBinder) msg.obj, msg.arg1 == 1);
                break;
            case PICTURE_IN_PICTURE_MODE_CHANGED:
                handlePictureInPictureModeChanged((IBinder) msg.obj, msg.arg1 == 1);
                break;
            case LOCAL_VOICE_INTERACTION_STARTED:
                handleLocalVoiceInteractionStarted((IBinder) ((SomeArgs) msg.obj).arg1,
                        (IVoiceInteractor) ((SomeArgs) msg.obj).arg2);
                break;
        }
        Object obj = msg.obj;
        if (obj instanceof SomeArgs) {
            ((SomeArgs) obj).recycle();
        }
        if (DEBUG_MESSAGES) Slog.v(TAG, "<<< done: " + codeToString(msg.what));
    }
}

```

从系统的Handler中，在handleMessage我们可以看到很多关于四大组件的生命周期操作，比如创建、销毁、切换、跨进程通信，也包括了整个Application进程的销毁等等。
比如说我们有一个应用程序A通过Binder去跨进程启动另外一个应用程序B的Service（或者同一个应用程序中不同进程的Service）：

如图：

![image](http://upload-images.jianshu.io/upload_images/9028834-d67e0c13f12025e1?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

最后是AMS接收到消息以后，发送消息到MessageQueue里面，最后由系统的Handler处理启动Service的操作：

```
case CREATE_SERVICE:
    Trace.traceBegin(Trace.TRACE_TAG_ACTIVITY_MANAGER, ("serviceCreate: " + String.valueOf(msg.obj)));
    handleCreateService((CreateServiceData)msg.obj);
    Trace.traceEnd(Trace.TRACE_TAG_ACTIVITY_MANAGER);
    break;

```

在handleCreateService里通过反射的方式去newInstance()，并且回调了Service的onCreate方法：

```
private void handleCreateService(CreateServiceData data) {
    // If we are getting ready to gc after going to the background, well
    // we are back active so skip it.
    unscheduleGcIdler();

    LoadedApk packageInfo = getPackageInfoNoCheck(
            data.info.applicationInfo, data.compatInfo);
    Service service = null;
    try {
        //通过反射的方式去创建Service
        java.lang.ClassLoader cl = packageInfo.getClassLoader();
        service = (Service) cl.loadClass(data.info.name).newInstance();
    } catch (Exception e) {
        if (!mInstrumentation.onException(service, e) {
            throw new RuntimeE)xception(
                "Unable to instantiate service " + data.info.name
                + ": " + e.toString(), e);
        }
    }

    try {
        if (localLOGV) Slog.v(TAG, "Creating service " + data.info.name);

        ContextImpl context = ContextImpl.createAppContext(this, packageInfo);
        context.setOuterContext(service);

        Application app = packageInfo.makeApplication(false, mInstrumentation);
        service.attach(context, this, data.info.name, data.token, app,
                ActivityManagerNative.getDefault());
        //回调了Service的onCreate方法
        service.onCreate();
        mServices.put(data.token, service);
        try {
            ActivityManagerNative.getDefault().serviceDoneExecuting(
                    data.token, SERVICE_DONE_EXECUTING_ANON, 0, 0);
        } catch (RemoteException e) {
            throw e.rethrowFromSystemServer();
        }
    } catch (Exception e) {
        if (!mInstrumentation.onException(service, e)) {
            throw new RuntimeException(
                "Unable to create service " + data.info.name
                + ": " + e.toString(), e);
        }
    }
}

```

又例如我们可以通过发SUICIDE消息可以自杀，这样来退出应用程序。

```
case SUICIDE:
    Process.killProcess(Process.myPid());
    break;

```

##### 应用程序的退出过程

实际上我们要退出应用程序的话，就是让主线程结束，换句话说就是要让Looper的循环结束。这里是直接结束Looper循环，因此我们四大组件的生命周期方法可能就不会执行了，因为四大组件的生命周期方法就是通过Handler去处理的，Looper循环都没有了，四大组件还玩毛线！因此我们平常写程序的时候就要注意了，onDestroy方法是不一定能够回调的。

```
case EXIT_APPLICATION:
    if (mInitialApplication != null) {
        mInitialApplication.onTerminate();
    }
    //退出Looper的循环
    Looper.myLooper().quit();
    break;

```

这里实际上是调用了MessageQueue的quit，清空所有Message。

```
public void quit() {
    mQueue.quit(false);
}

```

###### tips：看源码一定不要慌，也不要一行一行看，要抓住核心的思路去看即可。

### 消息机制的分析

##### 消息对象Message的分析

提到消息机制，在MessageQueue里面存在的就是我们的Message对象：

```
public final class Message implements Parcelable {

    public int what;
    public int arg1; 
    public int arg2;
    public Object obj;
    long when;
    Bundle data;
    Handler target;
    Runnable callback;Message next;

    private static final Object sPoolSync = new Object();
    private static Message sPool;
    private static int sPoolSize = 0;

    private static final int MAX_POOL_SIZE = 50;

    private static boolean gCheckRecycle = true;

    public static Message obtain() {
        synchronized (sPoolSync) {
            if (sPool != null) {
                Message m = sPool;
                sPool = m.next;
                m.next = null;
                m.flags = 0; // clear in-use flag
                sPoolSize--;
                return m;
            }
        }
        return new Message();
    }

    public void recycle() {
        if (isInUse()) {
            if (gCheckRecycle) {
                throw new IllegalStateException("This message cannot be recycled because it "
                        + "is still in use.");
            }
            return;
        }
        recycleUnchecked();
    }

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
}

```

首先我们可以看到Message对象是实现了Parcelable接口的，因为Message消息可能需要跨进程通信，这时候就需要进程序列化以及反序列化操作了。

Message里面有一些我们常见的参数，arg1 arg2 obj callback when等等。这里要提一下的就是这个target对象，这个对象就是发送这个消息的Handler对象，最终这条消息也是通过这个Handler去处理掉的。

##### Message Pool消息池的概念——重复利用Message

Message里面中一个非常重要的概念，就是消息池Pool：

```
    public static Message obtain() {
        synchronized (sPoolSync) {
            if (sPool != null) {
                Message m = sPool;
                sPool = m.next;
                m.next = null;
                m.flags = 0; // clear in-use flag
                sPoolSize--;
                return m;
            }
        }
        return new Message();
    }

```

我们通过obtain方法取出一条消息的时候，如果发现当前的消息池不为空，那就直接重复利用Message（已经被创建过和handle过的）；如果为空就重新new 一个消息。这就是一种享元设计模式的概念。例如在游戏里面，发子弹，如果一个子弹是一个对象，一按下按键就发很多个子弹，那么这时候就需要利用享元模式去循环利用了。

这个消息池是通过链表的实现的，通过上面的代码可以知道，sPool永远指向这个消息池的头，取消息的时候，先拿到当前的头sPool，然后使得sPool指向下一个结点，最后返回刚刚取出来的结点，如下图所示：

![image](http://upload-images.jianshu.io/upload_images/9028834-c9ccf385b06ff9b4?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上面我们知道了消息可以直接创建，也可以通过obtain方法循环利用。所以我们平常编程的时候就要养成好的习惯，循环利用。

##### 消息的回收机制

有消息的创建，必然有回收利用，下面两个是Message的回收相关的核心方法：

```
public void recycle() {
    if (isInUse()) {
        if (gCheckRecycle) {
            throw new IllegalStateException("This message cannot be recycled because it "
                    + "is still in use.");
        }
        return;
    }
    recycleUnchecked();
}

void recycleUnchecked() {
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

recycleUnchecked中拿到消息池，清空当前的消息，next指向当前的头指针，头指针指向当前的Message对象，也就是在消息池头部插入当前的消息。

关于消息的回收还有一点需要注意的就是，我们平时写Handler的时候不需要我们手动回收，因为谷歌的工程师已经有考虑到这方面的问题了。消息是在Handler分发处理之后就会被自动回收的：
我们回到Looper的loop方法里面：

```
public static void loop() {
    final Looper me = myLooper();
    if (me == null) {
        throw new RuntimeException("No Looper; Looper.prepare() wasn't called on this thread.");
    }

    final MessageQueue queue = me.mQueue;

    for (;;) {
        Message msg = queue.next(); // might block
        if (msg == null) {
            // No message indicates that the message queue is quitting.
            return;
        }
        //处理消息
        try {
            msg.target.dispatchMessage(msg);
        } finally {              
            }
        }

        msg.recycleUnchecked();//回收消息
    }
}

```

msg.target.dispatchMessage(msg)就是处理消息，紧接着在loop方法的最后调用了msg.recycleUnchecked()这就是回收了Message。

##### 消息的循环过程分析

下面我们继续分析这个死循环：

1、首先拿到Looper对象（me），如果当前的线程没有Looper，那么就会抛出异常，这就是为什么在子线程里面创建Handler如果不手动创建和启动Looper会报错的原因。

2、然后拿到Looper的成员变量MessageQueue，在MessageQueue里面不断地去取消息，关于MessageQueue的next方法如下：

这里可以看到消息的取出用到了一些native方法，这样做是为了获得更高的效率，消息的去取出并不是直接就从队列的头部取出的，而是根据了消息的when时间参数有关的，因为我们可以发送延时消息、也可以发送一个指定时间点的消息。因此这个函数有点复杂，我们点到为止即可。

```
Message next() {

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
            //拿到当前的时间戳
            final long now = SystemClock.uptimeMillis();
            Message prevMsg = null;
            Message msg = mMessages;
            //判断头指针的Target（Handler是否为空（因为头指针只是一个指针的作用））
            if (msg != null && msg.target == null) {
                // Stalled by a barrier.  Find the next asynchronous message in the queue.
                do {
                    //遍历下一条Message
                    prevMsg = msg;
                    msg = msg.next;
                } while (msg != null && !msg.isAsynchronous());
            }
            if (msg != null) {
                if (now < msg.when) {
                    //还没有到执行的时间
                    nextPollTimeoutMillis = (int) Math.min(msg.when - now, Integer.MAX_VALUE);
                } else {
                    //到了执行时间，直接返回
                    mBlocked = false;
                    if (prevMsg != null) {
                        //拿出消息，断开链表
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

3、继续分析loop方法：如果已经没有消息了，那么就可以退出循环，那么整个应用程序就退出了。什么情况下会发生呢？还记得我们分析应用退出吗？

在系统Handler收到EXIT_APPLICATION消息的时候，就会调用Looper的quit方法：

```
case EXIT_APPLICATION:
    if (mInitialApplication != null) {
        mInitialApplication.onTerminate();
    }
    Looper.myLooper().quit();
    break;

```

Looper的quit方法如下，实际上就是调用了消息队列的quit方法：

```
public void quit() {
    mQueue.quit(false);
}

```

而消息队列的quit方法实际上就是执行了消息的清空操作，然后在Looper循环里面如果取出消息为空的时候，程序就退出了：

```
void quit(boolean safe) {
    if (!mQuitAllowed) {
        throw new IllegalStateException("Main thread not allowed to quit.");
    }

    synchronized (this) {
        if (mQuitting) {
            return;
        }
        //置位正在退出的标志
        mQuitting = true;

        //清空所有消息
        if (safe) {
            //安全的（系统的），未来未处理的消息都移除
            removeAllFutureMessagesLocked();
        } else {
            //如果是不安全的，例如我们自己定义的消息，就一次性全部移除掉
            removeAllMessagesLocked();
        }

        // We can assume mPtr != 0 because mQuitting was previously false.
        nativeWake(mPtr);
    }
}

```

removeAllFutureMessagesLocked方法如下：

```
private void removeAllFutureMessagesLocked() {
    final long now = SystemClock.uptimeMillis();
    Message p = mMessages;
    if (p != null) {
        if (p.when > now) {
            //如果所有消息都处理完了，就一次性把全部消息移除掉
            removeAllMessagesLocked();
        } else {
            //否则就通过for循环拿到还没有把还没有执行的Message，利用do循环
            //把这些未处理的消息通过recycleUnchecked方法回收，放回到消息池里面
            Message n;
            for (;;) {
                n = p.next;
                if (n == null) {
                    return;
                }
                if (n.when > now) {
                    break;
                }
                p = n;
            }
            p.next = null;
            do {
                p = n;
                n = p.next;
                p.recycleUnchecked();
            } while (n != null);
        }
    }
}

```

4、msg.target.dispatchMessage(msg)就是处理消息，这里就会调用Handler的dispatchMessage方法：

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

```

在这个方法里面会先去判断Message的callback是否为空，这个callback是在Message类里面定义的：

```
Runnable callback;

```

这是一个Runnable对象，handleCallback方法里面做的事情就是拿到这个Runnable对象，然后在Handler所创建的线程（例如主线程）执行run方法：

```
private static void handleCallback(Message message) {
    message.callback.run();
}

```

###### Handler（Looper）在哪个线程创建的，就在哪个线程回调，没毛病，哈哈！

这就是我们平常使用post系列的方法：
post、postAtFrontOfQueue、postAtTime、postDelayed
其实最终也是通过Message包装一个Runnable实现的，我们看其中一个即可：

```
public final boolean post(Runnable r)
{
   return  sendMessageDelayed(getPostMessage(r), 0);
}

```

通过post一个Runnable的方式我们可以很简单地做一个循环，比如无限轮播的广告条Banner：

```
Handler mHandler = new Handler() {
    @Override
    public void handleMessage(Message msg) {

    }
};

Runnable run = new Runnable() {
    @Override
    public void run() {
        //广告条切换
        mBanner.next();
        //两秒钟之后继续下一次的轮播，其中tihs代表自身，也就是Runnable对象
        mHandler.postDelayed(this, 2000);
    }
};

//在需要的地方开始广播条的轮播
mHandler.postDelayed(run, 1000);
//在需要的地方停止广播条的轮播
mHandler.removeCallbacks(run);

```

当然，我们的Handler自己也可以有一个mCallback对象：

```
public interface Callback {
    public boolean handleMessage(Message msg);
}

final Callback mCallback;

```

如果自身的Callback不为空的话，就会回调Callback的方法。例如我们创建Handler的时候可以带上Callback：

```
Handler mHandler = new Handler(new Handler.Callback() {
    @Override
    public boolean handleMessage(Message msg) {
        //处理些东西，这种一般用于一些预处理，每次有消息来都需要执行的代码
        //返回值代表是否拦截消息的下面写的handleMessage，从源码里面可以看出来
        return false;
    }
}) {
    @Override
    public void handleMessage(Message msg) {

    }
};

```

如果自身的Callback执行之后没有返回true（没有拦截），那么最后才会回调我们经常需要复写的handleMessage方法，这个方法的默认实现是空处理：

```
public void handleMessage(Message msg) {

}

```

5、最后是回收消息：msg.recycleUnchecked()。所以说：我们平时在处理完handleMessage之后并不需要我们程序员手动去进行回收哈！系统已经帮我们做了这一步操作了。

```
Message msg = Message.obtain();
//不需要我们程序员去回收，这样反而会更加耗性能
msg.recycle();

```

6、通过上面就完成了一次消息的循环。

##### 消息的发送

分析完消息的分发与处理，最后我们来看看消息的发送：

![image](http://upload-images.jianshu.io/upload_images/9028834-a568953a9e374b41?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

消息的发送有这一系列方法，甚至我们的一系列post方法（封装了带Runnable的Message），最终都是调用sendMessageAtTime方法，把消息放到消息队列里面：

```
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

MessageQueue的进入队列的方法如下，核心思想就是时间比较小的（越是需要马上执行的消息）就越防到越靠近头指针的位置：

```
boolean enqueueMessage(Message msg, long when) {
    if (msg.target == null) {
        throw new IllegalArgumentException("Message must have a target.");
    }
    if (msg.isInUse()) {
        throw new IllegalStateException(msg + " This message is already in use.");
    }

    synchronized (this) {
        if (mQuitting) {
            IllegalStateException e = new IllegalStateException(
                    msg.target + " sending message to a Handler on a dead thread");
            Log.w(TAG, e.getMessage(), e);
            msg.recycle();
            return false;
        }

        msg.markInUse();
        msg.when = when;
        Message p = mMessages;
        boolean needWake;
        if (p == null || when == 0 || when < p.when) {
            // New head, wake up the event queue if blocked.
            msg.next = p;
            mMessages = msg;
            needWake = mBlocked;
        } else {
            // Inserted within the middle of the queue.  Usually we don't have to wake
            // up the event queue unless there is a barrier at the head of the queue
            // and the message is the earliest asynchronous message in the queue.
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
            msg.next = p; // invariant: p == prev.next
            prev.next = msg;
        }

        // We can assume mPtr != 0 because mQuitting is false.
        if (needWake) {
            nativeWake(mPtr);
        }
    }
    return true;
}

```

消息并不是一直在队列的尾部添加的，而是可以指定时间，如果是立马需要执行的消息，就会插到队列的头部，就会立马处理，如此类推。

关于这一点这里我们可以从MessageQueue的next方法知道，next是考虑消息的时间when变量的，下面回顾一下MessageQueue的next方法里面的一些核心代码：next方法并不是直接从头部取出来的，而是会去遍历所有消息，根据时间戳参数等信息来取消息的。

```
Message next() {
    for (;;) {
        if (nextPollTimeoutMillis != 0) {
            Binder.flushPendingCommands();
        }

        nativePollOnce(ptr, nextPollTimeoutMillis);

        synchronized (this) {

            if (msg != null) {
                if (now < msg.when) {
                    //如果当前的时间还没到达消息指定的时间，先计算出下一次需要处理的时间戳，然后保存起来
                    nextPollTimeoutMillis = (int) Math.min(msg.when - now, Integer.MAX_VALUE);
                } else {
                    //否则的话直接从消息队列的头部拿出一条消息
                    mBlocked = false;
                    if (prevMsg != null) {
                        prevMsg.next = msg.next;
                    } else {
                        mMessages = msg.next;
                    }
                    msg.next = null;
                    if (DEBUG) Log.v(TAG, "Returning message: " + msg);
                    msg.markInUse();
                    //返回取出来的消息
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
        }
    }
}

```

##### 线程与Looper的绑定

线程里面默认情况下是没有Looper循环器的，因此我们需要调用prepare方法来关联线程和Looper：

```
//Looper的prepare方法，并且关联到主线程
public static void prepareMainLooper() {
    //false意思不允许我们程序员退出（面向我们开发者），因为这是在主线程里面
    prepare(false);
    synchronized (Looper.class) {
        if (sMainLooper != null) {
            throw new IllegalStateException("The main Looper has already been prepared.");
        }
        //把Looper设置为主线程的Looper
        sMainLooper = myLooper();
    }
}

//Looper一般的prepare方法
private static void prepare(boolean quitAllowed) {

    //一个线程只能绑定一个Looper，否则的话就会抛出如下的异常
    if (sThreadLocal.get() != null) {
        throw new RuntimeException("Only one Looper may be created per thread");
    }

    sThreadLocal.set(new Looper(quitAllowed));
}

```

此处调用了ThreadLocal的set方法，并且new了一个Looper放进去。

```
Looper的成员变量sThreadLocal
static final ThreadLocal<Looper> sThreadLocal = new ThreadLocal<Looper>();

在prepare方法中new了一个Looper并且设置到sThreadLocal里面
sThreadLocal.set(new Looper(quitAllowed));

```

可以看到Looper与线程的关联是通过ThreadLocal来进行的，如下图所示：

![image](http://upload-images.jianshu.io/upload_images/9028834-2a6de515a6940a62?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

ThreadLocal是JDK提供的一个解决线程不安全的类，线程不安全问题归根结底主要涉及到变量的多线程访问问题，例如变量的临界问题、值错误、并发问题等。这里利用ThreadLocal绑定了Looper以及线程，就可以避免其他线程去访问当前线程的Looper了。

ThreadLocal通过get以及set方法就可以绑定线程和Looper了，这里只需要传入Value即可，因为线是可以通过Thread.currentThread()去拿到的：

```
public void set(T value) {
    Thread t = Thread.currentThread();
    ThreadLocalMap map = getMap(t);
    if (map != null)
        map.set(this, value);
    else
        createMap(t, value);
}

```

为什么可以绑定线程了呢？

map.set(this, value)通过把自身（ThreadLocal以及值（Looper）放到了一个Map里面，如果再放一个的话，就会覆盖，因为map不允许键值对中的键是重复的）

因此ThreadLocal绑定了线程以及Looper。

因为这里实际上把变量（这里是指Looper）放到了Thread一个成员变量Map里面，关键的代码如下：

```
//这是ThreadLocal的getMap方法
ThreadLocalMap getMap(Thread t) {
    return t.threadLocals;
}

//这是Thread类中定义的MAP
ThreadLocal.ThreadLocalMap threadLocals = null;

```

ThreadLocal的getMap方法实际上是拿到线程的MAP，底层是通过数组（实际上数据结构是一种散列列表）实现的，具体的实现就点到为止了。

###### 如果android系统主线程Looper可以随随便便被其他线程访问到的话就会很麻烦了，啊哈哈，你懂的。

##### Handler、Looper是怎么关联起来的呢？

我们知道，Looper是与线程相关联的（通过ThreadLocal），而我们平常使用的Handler是这样的：

```
Handler mHandler = new Handler() {
    @Override
    public void handleMessage(Message msg) {

    }
};

```

其实Handler在构造的时候，有多个重载方法，根据调用关系链，所以最终会调用下面这个构造方法：

```
public Handler(Callback callback, boolean async) {
    mLooper = Looper.myLooper();
    //如果当前线程（子线程）没有Looper，就需要我们程序要去手动prepare以及启动loop方法了
    //子线程里面默认没有Looper循环器
    if (mLooper == null) {
        throw new RuntimeException(
            "Can't create handler inside thread that has not called Looper.prepare()");
    }
    mQueue = mLooper.mQueue;
    mCallback = callback;
    mAsynchronous = async;
}

```

这里只给出了核心的代码，可以看到我们在构造Handler的时候，是通过Looper的静态方法myLooper()去拿到一个Looper对象的：

```
public static @Nullable Looper myLooper() {
    return sThreadLocal.get();
}

```

看，我们的又出现了ThreadLocal，这里就是通过ThreadLocal的get方法去拿到当前线程的Looper，因此Handler就跟线程绑定在一起了，在一起，在一起，啊哈哈。

###### 一般们是在Activity里面使用Handler的，而Activity的生命周期是在主线程回调的，因此我们一般使用的Handler是跟主线程绑定在一起的。

##### 主线程一直在循环，为什么没有卡死，还能响应我们的点击之类的呢？

1.  通过子线程去访问主线程的代码，有代码注入、回调机制嘛。
2.  切入到消息队列里面的消息去访问主线程，例如传消息，然后回调四大组件的生命周期等等。
3.  IPC跨进程的方式也可以实现。

虽然主线程一直在执行，但是我们可以通过外部条件、注入的方法来执行自己的代码，而不是一直死循环。

### 总结

![image](http://upload-images.jianshu.io/upload_images/9028834-31f40d3d3c08753f?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如图所示，在主线程ActivityThread中的main方法入口中，先是创建了系统的Handler（H），创建主线程的Looper，将Looper与主线程绑定，调用了Looper的loop方法之后开启整个应用程序的主循环。Looper里面有一个消息队列，通过Handler发送消息到消息队列里面，然后通过Looper不断去循环取出消息，交给Handler去处理。通过系统的Handler，或者说Android的消息处理机制就确保了整个Android系统有条不紊地运作，这是Android系统里面的一个比较重要的机制。

我们的APP也可以创建自己的Handler，可以是在主线程里面创建，也可以在子线程里面创建，但是需要手动创建子线程的Looper并且手动启动消息循环。

花了一天的时间，整个Android消息机制源码分析就到这里结束了，今天的天气真不错，但是我选择了在自己的房间学习Android的消息机制，我永远相信，付出总会有所收获的！

### 扩展阅读：论Handler的正确使用姿势

典型错误的使用示例：

```
public class LeakActivity extends AppCompatActivity {

    private int a = 10;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_leak);

        mHandler.sendEmptyMessageDelayed(0, 5000);
    }

    //也是匿名内部类，也会引用外部
    private Handler mHandler = new Handler() {
        @Override
        public void handleMessage(Message msg) {
            switch (msg.what) {
            case 0:
                a = 20;
                break;
            }
        }
    };

}

```

分析：这是我们用得最多的用法，Handler隐式地引用了Activity（通过变量a）。Handler的生命周期有可能与Activity的生命周期不一致，比如栗子中的sendEmptyMessageDelayed，在5000毫秒之后才发送消息，但是很有可能这时候Activity被返回了，这样会造成Handler比Activity还要长寿，这样会导致Activity发生暂时性的内存泄漏。

姿势一：
为了解决这个问题，我们可以把Handler改为static的，但是这样会造成Handler无法访问Activity的非静态变量a，但是实际开发中我们

```
private static Handler mHandler = new Handler() {
    @Override
    public void handleMessage(Message msg) {
        switch (msg.what) {
        case 0:
            //a = 20;   //不能访问得到
            break;
        }
    }
};

```

姿势二：
通过把Activity作为Handler成员变量，在Handler构造的时候传进来即可。这时候我们不能使用匿名内部类了，需要把Handler单独抽取成一个类，这样就可以访问Activity的非静态变量了。但是我们的问题又回来了，这时候Handler持有了Activity的强引用了，这样不就是回到我们的原点了吗？（内存泄漏问题依然没有解决）

```
private static class UIHandler extends Handler {

    private LeakActivity mActivity;//外部类的强引用

    public UIHandler(LeakActivity activity) {
        mActivity = activity;

    }

    @Override
    public void handleMessage(Message msg) {
        super.handleMessage(msg);

        mActivity.a = 20;
    }
}

```

姿势三（最终版本）：把Activity通过弱引用来作为成员变量。虽然我们把Activity作为弱引用，但是Activity不一定就是会在GC的时候被回收，因为可能还有其他对象引用了Activity。在处理消息的时候就要注意了，当Activity回收或者正在finish的时候，就不能继续处理消息了，再说了，Activity都回收了，Handler还玩个屁！

```
private static class UIHandler extends Handler {

    private WeakReference<LeakActivity> mActivityRef;//GC的时候会回收

    public UIHandler(LeakActivity activity) {
        mActivityRef = new WeakReference<>(activity);
    }

    @Override
    public void handleMessage(Message msg) {
        super.handleMessage(msg);

        //当使用弱引用的时候，会回收Activity吗？
        //虽然用的是弱引用，但是并不代表不存在其他的对象没有引用Activity，因此不一定会被回收
        //Activity都回收了，Handler还玩个屁！
        LeakActivity activity = mActivityRef.get();
        if (activity == null || activity.isFinishing()) {
            return;
        }

        mActivityRef.get().a = 20;
    }
}

```

关于更多的Handler使用，请参考我朋友写的文章：
[说说Handler的一些使用姿势](https://www.jianshu.com/p/8e9a54f1826e)

如果觉得我的文字对你有所帮助的话，欢迎关注我的公众号：