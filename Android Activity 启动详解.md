

# 【Android Activity 启动详解】

[TOC]

  在Android开发过程中，我们几乎每天都在跟`Activity`打交道。我们循规蹈矩的调用`startActivity()`方法便可以打开一个新的界面，但是这个界面是怎样从无到有我们却不是很清楚，也有很多人根本就没有想过。我们写的`layout`布局通过`setContentView()`方法是怎样加载到`activity`中，然后又是怎样显示到界面上的？这一系列的问题相信大多数人都不清楚。

  也许是因为我们平时的开发过程根本不需要了解这些，但是如果深入了解后，对我们还是很有帮助的。接下来我们分析一下Activity从无到显示布局这一过程，首先深入研究一下`Activity`的创建过程。

# 一、Activity的创建过程

## step 1. Activity.startActivtiy()

  在**Android**系统中，我们比较熟悉的**打开Activity**通常有**两种方式**：

- 第一种是**点击应用程序图标**，**Launcher**会启动应用程序的**主Activity**。
  我们知道**Launcher**其实也是一个应用程序，他是**怎样打开**我们的**主Activity**的呢？
  在应用程序被安装的时候，系统会找到`AndroidManifest.xml`中`activity`的配置信息，并将`action=android.intent.action.MAIN`&`category=android.intent.category.LAUNCHER`的`activity`记录下来，形成应用程序与主`Activity` 的映射关系，当点击启动图标时，Launcher就会找到应用程序对应的主activity并将它启动。
- 第二种是当主Activity启动之后，在应用程序内部可以调用**startActivity**()开启新的Activity，这种方式又可分为**显示启动**和**隐式启动**。

不管使用哪种方式启动Activity，其实最终调用的都是**startActivity()**方法。所以如果要分析Activity的启动过程，我们就从startActivity()方法分析。
跟踪发现`Activity`中重载的`startActivity()`方法最终都是调用`startActivityForResult(intent, requestCode , bundle)`：

```java
public void startActivityForResult(Intent intent, int requestCode, Bundle options) {
    //一般Activity的mParent都为null，mParent常用在ActivityGroup中，ActivityGroup已废弃
    if (mParent == null) {
        //启动新的Activity
        Instrumentation.ActivityResult ar =
                mInstrumentation.execStartActivity(
                        this, mMainThread.getApplicationThread(), mToken, this,
                        intent, requestCode, options);
        if (ar != null) {
            //跟踪execStartActivity()，发现开启activity失败ar才可能为null，这时会调用onActivityResult
            mMainThread.sendActivityResult(
                    mToken, mEmbeddedID, requestCode, ar.getResultCode(),
                    ar.getResultData());
        }
        ...
    } else {
        //在ActivityGroup内部的Activity调用startActivity的时候会走到这里，处理逻辑和上面是类似的
        if (options != null) {
            mParent.startActivityFromChild(this, intent, requestCode, options);
        } else {
            // Note we want to go through this method for compatibility with
            // existing applications that may have overridden it.
            mParent.startActivityFromChild(this, intent, requestCode);
        }
    }
}
```

  从上面的代码可以发现，开启`activity`的关键代码是`mInstrumentation.execStartActivity(）`，`mInstrumentation`是`Activity`的成员变量，用于监视系统与应用的互交。

## step2. Instrumentation.execStartActivity()

```java
public ActivityResult execStartActivity(
        Context who, IBinder contextThread, IBinder token, String target,
        Intent intent, int requestCode, Bundle options) {
    IApplicationThread whoThread = (IApplicationThread) contextThread;
    //mActivityMonitors是所有ActivityMonitor的集合，用于监视应用的Activity(记录状态)
    if (mActivityMonitors != null) {
        synchronized (mSync) {
            //先查找一遍看是否存在这个activity
            final int N = mActivityMonitors.size();
            for (int i=0; i<N; i++) {
                final ActivityMonitor am = mActivityMonitors.get(i);
                if (am.match(who, null, intent)) {
                    //如果找到了就跳出循环
                    am.mHits++;
                    //如果目标activity无法打开，直接return
                    if (am.isBlocking()) {
                        return requestCode >= 0 ? am.getResult() : null;
                    }
                    break;
                }
            }
        }
    }
    try {
        intent.migrateExtraStreamToClipData();
        intent.prepareToLeaveProcess();
        //这里才是真正开启activity的地方，ActivityManagerNative中实际上调用的是ActivityManagerProxy的方法
        int result = ActivityManagerNative.getDefault()
                .startActivity(whoThread, who.getBasePackageName(), intent,
                        intent.resolveTypeIfNeeded(who.getContentResolver()),
                        token, target, requestCode, 0, null, options);
                        
//checkStartActivityResult方法是抛异常专业户，它对上面开启activity的结果进行检查，如果无法打开activity，
//则抛出诸如ActivityNotFoundException类似的各种异常
        checkStartActivityResult(result, intent);
    } catch (RemoteException e) {
        throw new RuntimeException("Failure from system", e);
    }
    return null;
}
```

  `mInstrumentation的execStartActivity(）`方法中首先检查`activity`是否能打开，如果不能打开直接返回，否则继续调用`ActivityManagerNative.getDefault().startActivity()`开启`activity`，然后检查开启结果，如果开启失败，会抛出异常（比如未在`AndroidManifest.xml`注册）。接着我们转入`ActivityManagerNative`中：

## step3. ActivityManagerNative

  在讲解此步骤之前，先大致介绍一下`ActivityManagerNative`这个抽象类。`ActivityManagerNative`继承自`Binder`(实现了`IBinder`)，实现了`IActivityManager`接口，但是没有实现他的抽象方法。 
  `ActivityManagerNative`中有一个内部类`ActivityManagerProxy`，`ActivityManagerProxy`也实现了`IActivityManager`接口，并实现了`IActivityManager`中所有的方法。`IActivityManager`里面有很多像`startActivity()`、`startService()`、`bindService()`、`registerReveicer()`等方法，它为`Activity`管理器提供了统一的**API**。 
  `ActivityManagerNative`通过`getDefault()`获取到一个`ActivityManagerProxy`的示例，并将远程代理对象`IBinder`传递给他，然后调用他的**starXxx**方法开启`Service`、`Activity`或者`registReceiver`，可以看出`ActivityManagerNative`只是一个装饰类，真正工作的是其内部类`ActivityManagerProxy`。

### ① ActivtiyManagerNative.getDefault().startActivity()

```java
static public IActivityManager getDefault() {
    //此处返回的IActivityManager示例是ActivityManagerProxy的对象
    return gDefault.get();
}
private static final Singleton<IActivityManager> gDefault = new Singleton<IActivityManager>() {
    protected IActivityManager create() {
//android.os.ServiceManager中维护了HashMap<String, IBinder> sCache，他是系统Service对应的IBinder代理对象的集合
        //通过名称获取到ActivityManagerService对应的IBinder远程代理对象
        IBinder b = ServiceManager.getService("activity");
        if (false) {
            Log.v("ActivityManager", "default service binder = " + b);
        }
        //返回一个IActivityManager对象，这个对象实际上是ActivityManagerProxy的对象
        IActivityManager am = asInterface(b);
        if (false) {
            Log.v("ActivityManager", "default service = " + am);
        }
        return am;
    }
};
static public IActivityManager asInterface(IBinder obj) {
    if (obj == null) {
        return null;
    }
    IActivityManager in =
            (IActivityManager)obj.queryLocalInterface(descriptor);
    if (in != null) {
        return in;
    }
    //返回ActivityManagerProxy对象
    return new ActivityManagerProxy(obj);
}
```

  `ActivityManagerNative`中的方法只是获取到`ActivityManagerProxy`的实例，然后调用其`startActivity`方法：

### ② ActivtiyManagerProxy.startActivity()

```java
public int startActivity(IApplicationThread caller, String callingPackage, Intent intent,
                         String resolvedType, IBinder resultTo, String resultWho, int requestCode,
                         int startFlags, ProfilerInfo profilerInfo, Bundle options) throws RemoteException {
    Parcel data = Parcel.obtain();
    Parcel reply = Parcel.obtain();
    //下面的代码将参数持久化，便于ActivityManagerService中获取
    data.writeInterfaceToken(IActivityManager.descriptor);
    data.writeStrongBinder(caller != null ? caller.asBinder() : null);
    data.writeString(callingPackage);
    intent.writeToParcel(data, 0);
    data.writeString(resolvedType);
    data.writeStrongBinder(resultTo);
    data.writeString(resultWho);
    data.writeInt(requestCode);
    data.writeInt(startFlags);
    if (profilerInfo != null) {
        data.writeInt(1);
        profilerInfo.writeToParcel(data, Parcelable.PARCELABLE_WRITE_RETURN_VALUE);
    } else {
        data.writeInt(0);
    }
    if (options != null) {
        data.writeInt(1);
        options.writeToParcel(data, 0);
    } else {
        data.writeInt(0);
    }
    //mRemote就是ActivityManagerService的远程代理对象，这句代码之后就进入到ActivityManagerService中了
    mRemote.transact(START_ACTIVITY_TRANSACTION, data, reply, 0);
    reply.readException();
    int result = reply.readInt();
    reply.recycle();
    data.recycle();
    return result;
}
```

  上面说到`ActivityManagerNative`才是真正干活的，他维护了`ActivityManagerService`的远程代理对象`mRemote` ，最终会通过`mRemote`将开启`Activity`的消息传送给`ActivityManagerService`，这样就来到了`ActivityManagerService`的`startActivity`方法中：

## step4. ActivityManagerService

```java
public final class ActivityManagerService extends ActivityManagerNative
        implements Watchdog.Monitor, BatteryStatsImpl.BatteryCallback {
    ...

    @Override
    public final int startActivity(IApplicationThread caller, String callingPackage,
                                   Intent intent, String resolvedType, IBinder resultTo, String resultWho, int requestCode,
                                   int startFlags, ProfilerInfo profilerInfo, Bundle options) {
        return startActivityAsUser(caller, callingPackage, intent, resolvedType, resultTo,
                resultWho, requestCode, startFlags, profilerInfo, options,
                UserHandle.getCallingUserId());
    }

    @Override
    public final int startActivityAsUser(IApplicationThread caller, String callingPackage,
                                         Intent intent, String resolvedType, IBinder resultTo, String resultWho, int requestCode,
                                         int startFlags, ProfilerInfo profilerInfo, Bundle options, int userId) {
        enforceNotIsolatedCaller("startActivity");
        userId = handleIncomingUser(Binder.getCallingPid(), Binder.getCallingUid(), userId,
                false, ALLOW_FULL_ONLY, "startActivity", null);
        //mStackSupervisor的类型是ActivityStackSupervisor
        return mStackSupervisor.startActivityMayWait(caller, -1, callingPackage, intent,
                resolvedType, null, null, resultTo, resultWho, requestCode, startFlags,
                profilerInfo, null, null, options, userId, null, null);
    }
    ...
}
```

  `ActivityManagerService`中`startActivity()`直接调用了`startActivityAsUser()`方法，这个方法中又调用了`ActivitySupervisor`的`startActivityMayWait()`方法：

## step5. ActivityStackSupervisor.startActivityMayWait()

```java
final int startActivityMayWait(IApplicationThread caller, int callingUid,
                               String callingPackage, Intent intent, String resolvedType,
                               IVoiceInteractionSession voiceSession, IVoiceInteractor voiceInteractor,
                               IBinder resultTo, String resultWho, int requestCode, int startFlags,
                               ProfilerInfo profilerInfo, WaitResult outResult, Configuration config,
                               Bundle options, int userId, IActivityContainer iContainer, TaskRecord inTask) {
    ...

    // Don't modify the client's object!
    intent = new Intent(intent);

    // 调用resolveActivity()根据意图intent参数，解析目标Activity的一些信息保存到aInfo中，
    // 这些信息包括activity的name、applicationInfo、processName、theme、launchMode、permission、flags等等
    // 这都是在AndroidManifest.xml中为activity配置的
    ActivityInfo aInfo = resolveActivity(intent, resolvedType, startFlags,
            profilerInfo, userId);

    ...
    synchronized (mService) {
        //下面省略的代码用于重新组织startActivityLocked()方法需要的参数
        ...
        //调用startActivityLocked开启目标activity
        int res = startActivityLocked(caller, intent, resolvedType, aInfo,
                voiceSession, voiceInteractor, resultTo, resultWho,
                requestCode, callingPid, callingUid, callingPackage,
                realCallingPid, realCallingUid, startFlags, options,
                componentSpecified, null, container, inTask);
        ...

        if (outResult != null) {
            //如果outResult不为null,则设置开启activity的结果
            outResult.result = res;
            ...

        return res;
    }
}
```

  根据上面传递的参数和应用信息重新封装一些参数，然后调用`startActivityLocked()`方法：

## step6. ActivityStackSupervisor.startActivityLocked()

```java
final int startActivityLocked(IApplicationThread caller,
                              Intent intent, String resolvedType, ActivityInfo aInfo,
                              IVoiceInteractionSession voiceSession, IVoiceInteractor voiceInteractor,
                              IBinder resultTo, String resultWho, int requestCode,
                              int callingPid, int callingUid, String callingPackage,
                              int realCallingPid, int realCallingUid, int startFlags, Bundle options,
                              boolean componentSpecified, ActivityRecord[] outActivity, ActivityContainer container,
                              TaskRecord inTask) {
    int err = ActivityManager.START_SUCCESS;
    //调用者的进程信息，也就是哪个进程要开启此Activity的
    ProcessRecord callerApp = null;
    //下面有很多if语句，用于判断一些错误信息，并给err赋值相应的错误码
    if (caller != null) {
        callerApp = mService.getRecordForAppLocked(caller);
        if (callerApp != null) {
            callingPid = callerApp.pid;
            callingUid = callerApp.info.uid;
        } else {
            err = ActivityManager.START_PERMISSION_DENIED;
        }
    }
    ...
    if (err == ActivityManager.START_SUCCESS && intent.getComponent() == null) {
        err = ActivityManager.START_INTENT_NOT_RESOLVED;
    }
    if (err == ActivityManager.START_SUCCESS && aInfo == null) {
        // 未找到需要打开的activity的class文件
        err = ActivityManager.START_CLASS_NOT_FOUND;
    }
    ...
    //上面判断完成之后，接着判断如果err不为START_SUCCESS，则说明开启activity失败，直接返回错误码
    if (err != ActivityManager.START_SUCCESS) {
        if (resultRecord != null) {
            resultStack.sendActivityResultLocked(-1,
                    resultRecord, resultWho, requestCode,
                    Activity.RESULT_CANCELED, null);
        }
        ActivityOptions.abort(options);
        return err;
    }

    //检查权限，有些activity在清单文件中注册了权限，比如要开启系统相机，就需要注册相机权限，否则此处就会跑出异常
    final int startAnyPerm = mService.checkPermission(
            START_ANY_ACTIVITY, callingPid, callingUid);
    final int componentPerm = mService.checkComponentPermission(aInfo.permission, callingPid,
            callingUid, aInfo.applicationInfo.uid, aInfo.exported);
    if (startAnyPerm != PERMISSION_GRANTED && componentPerm != PERMISSION_GRANTED) {
        if (resultRecord != null) {
            resultStack.sendActivityResultLocked(-1,
                    resultRecord, resultWho, requestCode,
                    Activity.RESULT_CANCELED, null);
        }
        String msg;
        //权限被拒绝，抛出异常
        if (!aInfo.exported) {
            msg = "Permission Denial: starting " + intent.toString()
                    + " from " + callerApp + " (pid=" + callingPid
                    + ", uid=" + callingUid + ")"
                    + " not exported from uid " + aInfo.applicationInfo.uid;
        } else {
            msg = "Permission Denial: starting " + intent.toString()
                    + " from " + callerApp + " (pid=" + callingPid
                    + ", uid=" + callingUid + ")"
                    + " requires " + aInfo.permission;
        }
        throw new SecurityException(msg);
    }

    //★★★经过上面的判断后，创建一个新的Activity记录，
    // 这个ActivityRecord就是被创建的activity在历史堆栈中的一个条目，表示一个活动
    ActivityRecord r = new ActivityRecord(mService, callerApp, callingUid, callingPackage,
            intent, resolvedType, aInfo, mService.mConfiguration, resultRecord, resultWho,
            requestCode, componentSpecified, this, container, options);
    ...
    //继续调用startActivityUncheckedLocked()
    err = startActivityUncheckedLocked(r, sourceRecord, voiceSession, voiceInteractor,
            startFlags, true, options, inTask);
    ...
    return err;
}
```

  这个方法主要是判断一些错误信息和检查权限，如果没有发现错误（`err==START_SUCCESS`）就继续开启`activity`， 否则直接返回错误码。继续查看`startActivityUnChechedLocked()`方法：

## step7 : ActivityStackSupervisor.startActivityUncheckedLocked()

```java
final int startActivityUncheckedLocked(ActivityRecord r, ActivityRecord sourceRecord,
                                       IVoiceInteractionSession voiceSession, IVoiceInteractor voiceInteractor, int startFlags,
                                       boolean doResume, Bundle options, TaskRecord inTask) {
    ...
    //① 获取并配置activity配置的启动模式
    int launchFlags = intent.getFlags();
    if ((launchFlags & Intent.FLAG_ACTIVITY_NEW_DOCUMENT) != 0 &&
            (launchSingleInstance || launchSingleTask)) {
        launchFlags &=
                ~(Intent.FLAG_ACTIVITY_NEW_DOCUMENT | Intent.FLAG_ACTIVITY_MULTIPLE_TASK);
    } else {
       ...
    }
    ...
    /* 如果调用者不是来自另一个activity（不是在activity中调用startActivity）,
     * 但是给了我们用于放入新activity的一个明确的task，将执行下面代码
     *
     * 我们往上追溯，发现inTask是step4 中 ActivityManagerService.startActivityAsUser()方法传递的null，
     * 所以if里面的不会执行
     */
    if (sourceRecord == null && inTask != null && inTask.stack != null) {
        ...
    } else {
        inTask = null;
    }
    //根据activity的设置，如果满足下列条件，将launchFlags置为FLAG_ACTIVITY_NEW_TASK（创建新进程）
    if (inTask == null) {
        if (sourceRecord == null) {
            // This activity is not being started from another...  in this
            // case we -always- start a new task.
            //如果调用者为null，将launchFlags置为 创建一个新进程
            if ((launchFlags & Intent.FLAG_ACTIVITY_NEW_TASK) == 0 && inTask == null) {
                launchFlags |= Intent.FLAG_ACTIVITY_NEW_TASK;
            }
        } else if (sourceRecord.launchMode == ActivityInfo.LAUNCH_SINGLE_INSTANCE) {
            // 如果调用者的模式是SINGLE_INSTANCE，需要开启新进程
            launchFlags |= Intent.FLAG_ACTIVITY_NEW_TASK;
        } else if (launchSingleInstance || launchSingleTask) {
            // 如果需要开启的activity的模式是SingleInstance或者SingleTask，也需要开新进程
            launchFlags |= Intent.FLAG_ACTIVITY_NEW_TASK;
        }
    }

    ActivityInfo newTaskInfo = null;   //新进程
    Intent newTaskIntent = null;
    ActivityStack sourceStack;    //调用者所在的进程
    //下面省略的代码是为上面三个变量赋值
    ...

    // ② 我们尝试将新的activity放在一个现有的任务中。但是如果activity被要求是singleTask或者singleInstance，
    // 我们会将activity放入一个新的task中.下面的if中主要处理将目标进程置于栈顶，然后将目标activity显示
    if (((launchFlags & Intent.FLAG_ACTIVITY_NEW_TASK) != 0 &&
            (launchFlags & Intent.FLAG_ACTIVITY_MULTIPLE_TASK) == 0)
            || launchSingleInstance || launchSingleTask) {
        //如果被开启的activity不是需要开启新的进程，而是single instance或者singleTask，
        if (inTask == null && r.resultTo == null) {
            //检查此activity是否已经开启了,findTaskLocked()方法用于查找目标activity所在的进程
            ActivityRecord intentActivity = !launchSingleInstance ?
                    findTaskLocked(r) : findActivityLocked(intent, r.info);
            if (intentActivity != null) {
                ...
                targetStack = intentActivity.task.stack;
                ...
                //如果目标activity已经开启，目标进程不在堆栈顶端，我们需要将它置顶
                final ActivityStack lastStack = getLastStack();
                ActivityRecord curTop = lastStack == null? null : lastStack.topRunningNonDelayedActivityLocked(notTop);
                boolean movedToFront = false;
                if (curTop != null && (curTop.task != intentActivity.task ||
                        curTop.task != lastStack.topTask())) {
                    r.intent.addFlags(Intent.FLAG_ACTIVITY_BROUGHT_TO_FRONT);
                    if (sourceRecord == null || (sourceStack.topActivity() != null &&
                            sourceStack.topActivity().task == sourceRecord.task)) {
                        ...
                        //置顶进程
                        targetStack.moveTaskToFrontLocked(intentActivity.task, r, options,
                                "bringingFoundTaskToFront");
                        ...
                        movedToFront = true;
                    }
                }
                ...
                if ((launchFlags &
                        (Intent.FLAG_ACTIVITY_NEW_TASK|Intent.FLAG_ACTIVITY_CLEAR_TASK))
                        == (Intent.FLAG_ACTIVITY_NEW_TASK|Intent.FLAG_ACTIVITY_CLEAR_TASK)) {
                    //如果调用者要求完全替代已经存在的进程
                    reuseTask = intentActivity.task;
                    reuseTask.performClearTaskLocked();
                    reuseTask.setIntent(r);
                } else if ((launchFlags&Intent.FLAG_ACTIVITY_CLEAR_TOP) != 0
                        || launchSingleInstance || launchSingleTask) {
                    //将进程堆栈中位于目标activity上面的其他activitys清理掉
                    ActivityRecord top = intentActivity.task.performClearTaskLocked(r, launchFlags);
                } else if (r.realActivity.equals(intentActivity.task.realActivity)) {
                    //如果进程最上面的activity就是目标activity,进行一些设置操作
                    ...
                } else if ((launchFlags&Intent.FLAG_ACTIVITY_RESET_TASK_IF_NEEDED) == 0) {
                    ...
                } else if (!intentActivity.task.rootWasReset) {
                    ...
                }
                if (!addingToTask && reuseTask == null) {
                    //让目标activity显示，会调用onResume
                    if (doResume) {
                        targetStack.resumeTopActivityLocked(null, options);
                    } else {
                        ActivityOptions.abort(options);
                    }
                    //直接返回
                    return ActivityManager.START_TASK_TO_FRONT;
                }
            }
        }
    }

    // ③ 判断包名是否解析成功，如果包名解析不成功无法开启activity
    if (r.packageName != null) {
        //当前处于堆栈顶端的进程和activity
        ActivityStack topStack = getFocusedStack();
        ActivityRecord top = topStack.topRunningNonDelayedActivityLocked(notTop);
        if (top != null && r.resultTo == null) {
            if (top.realActivity.equals(r.realActivity) && top.userId == r.userId) {
                if (top.app != null && top.app.thread != null) {
                    if ((launchFlags & Intent.FLAG_ACTIVITY_SINGLE_TOP) != 0
                            || launchSingleTop || launchSingleTask) {
                        ActivityStack.logStartActivity(EventLogTags.AM_NEW_INTENT, top,
                                top.task);
                        ...
                        return ActivityManager.START_DELIVERED_TO_TOP;
                    }
                }
            }
        }

    } else {
        // 包名为空，直接返回，没有找到要打开的activity
        ...
        return ActivityManager.START_CLASS_NOT_FOUND;
    }

    // ④ 判断activiy应该在那个进程中启动，如果该进程中已经存在目标activity，根据启动模式做相应处理
    ...
    // 判断是否需要开启新进程?
    boolean newTask = false;
    if (r.resultTo == null && inTask == null && !addingToTask
            && (launchFlags & Intent.FLAG_ACTIVITY_NEW_TASK) != 0) {
        ...
        newTask = true;   //如果有FLAG_ACTIVITY_NEW_TASK标志，将为目标activity开启新的进程
        targetStack = adjustStackFocus(r, newTask);
        if (!launchTaskBehind) {
            targetStack.moveToFront("startingNewTask");
        }
        if (reuseTask == null) {
            r.setTask(targetStack.createTaskRecord(getNextTaskId(),
                    newTaskInfo != null ? newTaskInfo : r.info,
                    newTaskIntent != null ? newTaskIntent : intent,
                    voiceSession, voiceInteractor, !launchTaskBehind /* toTop */),
                    taskToAffiliate);
            if (DEBUG_TASKS) Slog.v(TAG, "Starting new activity " + r + " in new task " +
                    r.task);
        } else {
            r.setTask(reuseTask, taskToAffiliate);
        }
        ...
    } else if (sourceRecord != null) {
        //调用者不为空
        final TaskRecord sourceTask = sourceRecord.task;
        //默认在调用者所在进程启动，需要将进程置前
        targetStack = sourceTask.stack;
        targetStack.moveToFront("sourceStackToFront");
        final TaskRecord topTask = targetStack.topTask();
        if (topTask != sourceTask) {
            targetStack.moveTaskToFrontLocked(sourceTask, r, options, "sourceTaskToFront");
        }
        if (!addingToTask && (launchFlags&Intent.FLAG_ACTIVITY_CLEAR_TOP) != 0) {
            //没有FLAG_ACTIVITY_CLEAR_TOP标志时，开启activity
            ActivityRecord top = sourceTask.performClearTaskLocked(r, launchFlags);
            if (top != null) {
                ActivityStack.logStartActivity(EventLogTags.AM_NEW_INTENT, r, top.task);
                top.deliverNewIntentLocked(callingUid, r.intent, r.launchedFromPackage);
                ...
                targetStack.resumeTopActivityLocked(null);
                return ActivityManager.START_DELIVERED_TO_TOP;
            }
        } else if (!addingToTask &&
                (launchFlags&Intent.FLAG_ACTIVITY_REORDER_TO_FRONT) != 0) {
            //如果activity在当前进程中已经开启，清除位于他之上的activity
            final ActivityRecord top = sourceTask.findActivityInHistoryLocked(r);
            if (top != null) {
                final TaskRecord task = top.task;
                task.moveActivityToFrontLocked(top);
                ActivityStack.logStartActivity(EventLogTags.AM_NEW_INTENT, r, task);
                ...
                return ActivityManager.START_DELIVERED_TO_TOP;
            }
        }
        r.setTask(sourceTask, null);
        if (DEBUG_TASKS) Slog.v(TAG, "Starting new activity " + r + " in existing task " + r.task + " from source " + sourceRecord);

    } else if (inTask != null) {
        //在调用者指定的确定的进程中开启目标activity
        ...
        if (DEBUG_TASKS) Slog.v(TAG, "Starting new activity " + r  + " in explicit task " + r.task);
    } else {
        ...
    }

    ...
    ActivityStack.logStartActivity(EventLogTags.AM_CREATE_ACTIVITY, r, r.task);
    targetStack.mLastPausedActivity = null;
    //⑤ 继续调用目标堆栈ActivityStack的startActivityLocked()方法，这个方法没有返回值，执行完毕之后直接返回START_SUCCESS
    targetStack.startActivityLocked(r, newTask, doResume, keepCurTransition, options);
    if (!launchTaskBehind) {
        // Don't set focus on an activity that's going to the back.
        mService.setFocusedActivityLocked(r, "startedActivity");
    }
    return ActivityManager.START_SUCCESS;
}
```

  通过第6步，新的activity记录已经创建了，接下来就是将这个`activity`放入到某个进程堆栈中； 
  `startActivityLocked()`中首先获取`activity`的启动模式（`AndroidManifest.xml`中为`activity`配置的`launchMode`属性值），

> 启动模式一共有四种： 
> `ActivityInfo.LAUNCH_MULTIPLE`（standard标准）、 
> `ActivityInfo.LAUNCH_SINGLE_INSTANCE`（全局单例）、 
> `ActivityInfo.LAUNCH_SINGLE_TASK`（进程中单例）、 
> `ActivityInfo.LAUNCH_SINGLE_TOP`（栈顶单例） 
> 不清楚的可以参考官方网站： 
> [http://developer.android.com/reference/android/content/pm/ActivityInfo.html](http://developer.android.com/reference/android/content/pm/ActivityInfo.html)
> 或见博客[Activity](http://blog.csdn.net/moira33/article/details/78850859)

  如果目标`activity`已经在某个历史进程中存在，需要根据启动模式分别判断并做相应处理。举个例子：如果启动模式为`LAUNCH_SINGLE_INSTANCE`，发现目标`activity`在某个进程中已经被启动过，这时候就将此进程置于进程堆栈栈顶，然后清除位于目标`activity`之上的`activity`，这样目标`activity`就位于栈顶了，这种情况就算是`activity`启动成功，直接返回。 
  通过上面的判断处理，发现必须创建新的`activity`，并将其放入到某个进程中，就会进一步获取需要栖息的进程堆栈`targetStack`（创建新进程or已有的进程），最后调用`(ActivityStack)targetStack.startActivityLocked()`方法：

## step8 : ActivityStack.startActivityLocked()

  `ActivityStack`用于管理`activity`的堆栈状态，`startActivityLocked()`方法就是将某个`activity`记录放入堆栈中，下面看看源码：

```java
final void startActivityLocked(ActivityRecord r, boolean newTask,
                               boolean doResume, boolean keepCurTransition, Bundle options) {
    TaskRecord rTask = r.task;
    final int taskId = rTask.taskId;
    // mLaunchTaskBehind tasks get placed at the back of the task stack.
    if (!r.mLaunchTaskBehind && (taskForIdLocked(taskId) == null || newTask)) {
        // Last activity in task had been removed or ActivityManagerService is reusing task.
        // Insert or replace.
        // Might not even be in.
        insertTaskAtTop(rTask);
        mWindowManager.moveTaskToTop(taskId);
    }
    TaskRecord task = null;
    //①.不用创建新进程的情况，需要做一些任务切换操作
    if (!newTask) {
        boolean startIt = true;
        //遍历所有的任务，找到目标activity所在的堆栈，taskNdx为所有的task的数量，肯定是大于0
        for (int taskNdx = mTaskHistory.size() - 1; taskNdx >= 0; --taskNdx) {
            task = mTaskHistory.get(taskNdx);
            if (task.getTopActivity() == null) {
                // 如果进程中activity为空，继续遍历
                continue;
            }
            if (task == r.task) {
                //如果当前task==需要开启的activity的进程
                if (!startIt) {
                    if (DEBUG_ADD_REMOVE) Slog.i(TAG, "Adding activity " + r + " to task "
                            + task, new RuntimeException("here").fillInStackTrace());
                    // 将需要启动的activity的记录放入task堆栈的顶层
                    task.addActivityToTop(r);
                    r.putInHistory();
                    mWindowManager.addAppToken(task.mActivities.indexOf(r), r.appToken,
                            r.task.taskId, mStackId, r.info.screenOrientation, r.fullscreen,
                            (r.info.flags & ActivityInfo.FLAG_SHOW_ON_LOCK_SCREEN) != 0,
                            r.userId, r.info.configChanges, task.voiceSession != null,
                            r.mLaunchTaskBehind);
                    ...
                    return;
                }
                break;
            } else if (task.numFullscreen > 0) {
                startIt = false;
            }
        }
    }

    //②. 在处于栈顶的进程中放置新的activity，这个activity将是即将和用户交互的界面
    task = r.task;
    //将activity插入历史堆栈顶层
    task.addActivityToTop(r);
    task.setFrontOfTask();
    r.putInHistory();
    if (!isHomeStack() || numActivities() > 0) {
        /* 如果我们需要切换到一个新的任务，或者下一个activity不是当前正在运行的，
         * 我们需要显示启动预览窗口，在这里可能执行一切窗口切换的动画效果         */
        boolean showStartingIcon = newTask;
        ProcessRecord proc = r.app;
        ...
        if ((r.intent.getFlags()&Intent.FLAG_ACTIVITY_NO_ANIMATION) != 0) {
            mWindowManager.prepareAppTransition(AppTransition.TRANSIT_NONE, keepCurTransition);
            mNoAnimActivities.add(r);
        } else {
            //执行切换动画
            mWindowManager.prepareAppTransition(...);
            mNoAnimActivities.remove(r);
        }
        mWindowManager.addAppToken(task.mActivities.indexOf(r),
                r.appToken, r.task.taskId, mStackId, r.info.screenOrientation, r.fullscreen,
                (r.info.flags & ActivityInfo.FLAG_SHOW_ON_LOCK_SCREEN) != 0, r.userId,
                r.info.configChanges, task.voiceSession != null, r.mLaunchTaskBehind);
        ...
    } else {
        // 如果需要启动的activity的信息已经是堆栈中第一个，不需要执行动画
        mWindowManager.addAppToken(task.mActivities.indexOf(r), r.appToken,
                r.task.taskId, mStackId, r.info.screenOrientation, r.fullscreen,
                (r.info.flags & ActivityInfo.FLAG_SHOW_ON_LOCK_SCREEN) != 0, r.userId,
                r.info.configChanges, task.voiceSession != null, r.mLaunchTaskBehind);
        ...
    }
    ...
    if (doResume) {
        //此处的doResume参数为true，继续调用ActivityStackSupervisor.resumeTopActivitiesLocked()
        mStackSupervisor.resumeTopActivitiesLocked(this, r, options);
    }
}
```

  这个方法中主要进行一些堆栈切换工作，将目标`activity`所在的堆栈置顶， 然后再栈顶放入新的`activtiy`记录，最后调用`mStackSupervisor.resumeTopActivitiesLocked(this, r, options)`方法 将位于栈顶的`activity`显示出来：

## step9 : ActivityStackSupervisor.resumeTopActivitiesLocked()

```java
boolean resumeTopActivitiesLocked(ActivityStack targetStack, ActivityRecord target,
                                  Bundle targetOptions) {
    if (targetStack == null) {
        targetStack = getFocusedStack();
    }
    // Do targetStack first.
    boolean result = false;
    //是否是栈顶的任务
    if (isFrontStack(targetStack)) {
        result = targetStack.resumeTopActivityLocked(target, targetOptions);
    }
    for (int displayNdx = mActivityDisplays.size() - 1; displayNdx >= 0; --displayNdx) {
        final ArrayList<ActivityStack> stacks = mActivityDisplays.valueAt(displayNdx).mStacks;
        for (int stackNdx = stacks.size() - 1; stackNdx >= 0; --stackNdx) {
            final ActivityStack stack = stacks.get(stackNdx);
            if (stack == targetStack) {
                // Already started above.
                continue;
            }
            if (isFrontStack(stack)) {
                //会调用resumeTopActivityLocked(ActivityRecord prev, Bundle options)
                stack.resumeTopActivityLocked(null);
            }
        }
    }
    return result;
}
```

  这一步紧接着调用`ActivityStack.resumeTopActivityLocked()`：

## step10 : ActivityStack.resumeTopActivityLocked()

```java
final boolean resumeTopActivityLocked(ActivityRecord prev, Bundle options) {
    if (mStackSupervisor.inResumeTopActivity) {
        // Don't even start recursing.
        return false;
    }

    boolean result = false;
    try {
        // Protect against recursion.
        mStackSupervisor.inResumeTopActivity = true;
        if (mService.mLockScreenShown == ActivityManagerService.LOCK_SCREEN_LEAVING) {
            mService.mLockScreenShown = ActivityManagerService.LOCK_SCREEN_HIDDEN;
            mService.updateSleepIfNeededLocked();
        }
        //继续调用resumeTopActivityInnerLocked()
        result = resumeTopActivityInnerLocked(prev, options);
    } finally {
        mStackSupervisor.inResumeTopActivity = false;
    }
    return result;
}

resumeTopActivityLocked()方法继续调用resumeTopActivityInnerLocked()方法：

final boolean resumeTopActivityInnerLocked(ActivityRecord prev, Bundle options) {
    ...

    // We need to start pausing the current activity so the top one
    // can be resumed...
    //① 需要将现在的activity置于pausing状态，然后才能将栈顶的activity处于resume状态
    boolean dontWaitForPause = (next.info.flags&ActivityInfo.FLAG_RESUME_WHILE_PAUSING) != 0;
    boolean pausing = mStackSupervisor.pauseBackStacks(userLeaving, true, dontWaitForPause);
    if (mResumedActivity != null) {
        if (DEBUG_STATES) Slog.d(TAG, "resumeTopActivityLocked: Pausing " + mResumedActivity);
        pausing |= startPausingLocked(userLeaving, false, true, dontWaitForPause);
    }
    ...

    // Launching this app's activity, make sure the app is no longer
    // considered stopped.
    //② 启动栈顶的activity
    try {
        AppGlobals.getPackageManager().setPackageStoppedState(
                next.packageName, false, next.userId); /* TODO: Verify if correct userid */
    } catch (RemoteException e1) {
    } catch (IllegalArgumentException e) {
        Slog.w(TAG, "Failed trying to unstop package "
                + next.packageName + ": " + e);
    }
    ...

    //③ 判断栈顶activity是否启动，如果已经启动将其置为resume状态，如果没有启动将重新启动activity
    if (next.app != null && next.app.thread != null) {
        //如果栈顶的activity不为空，并且其thread成员（ApplicationThread）不为空，说明activity已经启动（执行了attach()）
        if (DEBUG_SWITCH) Slog.v(TAG, "Resume running: " + next);
        // This activity is now becoming visible.
        mWindowManager.setAppVisibility(next.appToken, true);
        ...
        try {
            ...
            next.sleeping = false;
            mService.showAskCompatModeDialogLocked(next);
            next.app.pendingUiClean = true;
            next.app.forceProcessStateUpTo(ActivityManager.PROCESS_STATE_TOP);
            next.clearOptionsLocked();
            //回调activity的onResume()方法
            next.app.thread.scheduleResumeActivity(next.appToken, next.app.repProcState,
                    mService.isNextTransitionForward(), resumeAnimOptions);
            ...
        } catch (Exception e) {

            //抛异常后，需要启动activity
            mStackSupervisor.startSpecificActivityLocked(next, true, false);
            if (DEBUG_STACK) mStackSupervisor.validateTopActivitiesLocked();
            return true;
        }
        ...
    } else {
        // 否则需要重新启动activity
        ...
        mStackSupervisor.startSpecificActivityLocked(next, true, true);
    }
    return true;
}
```

在`resumeTopActivityInnerLocked ()`中，主要会经历三个步骤：
  第一步需要将当前正在显示的`activity`置于`pausing`状态; 
  然后启动栈顶的`activity`(也就是目标`activity`)，
  如果目标`activity`已经被启动过，会将其置于`resume`状态； 否则将重新启动`activity`。
由于现在我们研究的`acivity`的启动，所以继续跟踪`ActivityStackSupervisor.startSpecificActivityLocked()`：

## step11：ActivityStackSupervisor.startSpecificActivityLocked()

```java
void startSpecificActivityLocked(ActivityRecord r,
                                 boolean andResume, boolean checkConfig) {
    //判断activity所属的应用程序的进程（process + uid）是否已经启动
    ProcessRecord app = mService.getProcessRecordLocked(r.processName,
            r.info.applicationInfo.uid, true);
    r.task.stack.setLaunchTime(r);
    if (app != null && app.thread != null) {
        try {
            ...
            /* ① 如果应用已经启动，并且进程中的thread对象不为空，
             *   调用realStartActivityLocked()方法创建activity对象
             *
             *   继续跟下去，会发现调用activity的onCreate方法
             */
            realStartActivityLocked(r, app, andResume, checkConfig);
            return;
        } catch (RemoteException e) {
            Slog.w(TAG, "Exception when starting activity "
                    + r.intent.getComponent().flattenToShortString(), e);
        }
        // If a dead object exception was thrown -- fall through to
        // restart the application.
    }
    //② 如果抛出了异常或者获取的应用进程为空，需用重新启动应用程序，点击Launcher桌面上图标时走这里
    mService.startProcessLocked(r.processName, r.info.applicationInfo, true, 0,
            "activity", r.intent.getComponent(), false, false, true);
}
```

  这个方法用于判断需要开启的`activity`所在的进程是否已经启动， 如果已经启动，会执行第①中情况开启`activity`， 如果没有启动，将会走第②中情况，先去启动进程，然后在开启`activity`。 由于第②种情况是一个比较完整的过程，并且后面也会调用`realStartActivityLocked()`方法开启`activity`， 所以，我们继续分析第②种情况：

## step12: ActivityManagerService.startProcessLocked()

```java
final ProcessRecord startProcessLocked(String processName,
                                       ApplicationInfo info, boolean knownToBeDead, int intentFlags,
                                       String hostingType, ComponentName hostingName, boolean allowWhileBooting,
                                       boolean isolated, boolean keepIfLarge) {
    return startProcessLocked(processName, info, knownToBeDead, intentFlags, hostingType,
            hostingName, allowWhileBooting, isolated, 0 /* isolatedUid */, keepIfLarge,
            null /* ABI override */, null /* entryPoint */, null /* entryPointArgs */,
            null /* crashHandler */);
}
final ProcessRecord startProcessLocked(String processName, ApplicationInfo info,
                                       boolean knownToBeDead, int intentFlags, String hostingType, ComponentName hostingName,
                                       boolean allowWhileBooting, boolean isolated, int isolatedUid, boolean keepIfLarge,
                                       String abiOverride, String entryPoint, String[] entryPointArgs, Runnable crashHandler) {
    long startTime = SystemClock.elapsedRealtime();
    ProcessRecord app;
    //isolated==false
    if (!isolated) {
        //再次检查是否已经有以process + uid命名的进程存在
        app = getProcessRecordLocked(processName, info.uid, keepIfLarge);
        checkTime(startTime, "startProcess: after getProcessRecord");
    } else {
        app = null;
    }
    //接下来有一些if语句，用于判断是否需要创建新进程，如果满足下面三种情况，就不会创建新进程
    // We don't have to do anything more if:
    // (1) There is an existing application record; and
    // (2) The caller doesn't think it is dead, OR there is no thread
    //     object attached to it so we know it couldn't have crashed; and
    // (3) There is a pid assigned to it, so it is either starting or
    //     already running.
    ...

    //继续调用startProcessLocked()真正创建新进程
    startProcessLocked(app, hostingType, hostingNameStr, abiOverride, entryPoint, entryPointArgs);
    return (app.pid != 0) ? app : null;
}

private final void startProcessLocked(ProcessRecord app, String hostingType,
                                      String hostingNameStr, String abiOverride, String entryPoint, String[] entryPointArgs) {
    ...
    try {
        //下面代码主要是初始化新进程需要的参数
        int uid = app.uid;
        int[] gids = null;
        int mountExternal = Zygote.MOUNT_EXTERNAL_NONE;
        if (!app.isolated) {
            int[] permGids = null;
            try {
                final PackageManager pm = mContext.getPackageManager();
                permGids = pm.getPackageGids(app.info.packageName);

                ...
            } catch (PackageManager.NameNotFoundException e) {
            }
            if (permGids == null) {
                gids = new int[2];
            } else {
                gids = new int[permGids.length + 2];
                System.arraycopy(permGids, 0, gids, 2, permGids.length);
            }
            gids[0] = UserHandle.getSharedAppGid(UserHandle.getAppId(uid));
            gids[1] = UserHandle.getUserGid(UserHandle.getUserId(uid));
        }
        ...

        app.gids = gids;
        app.requiredAbi = requiredAbi;
        app.instructionSet = instructionSet;

        //是否是Activity所在的进程，此处entryPoint为null所以isActivityProcess为true
        boolean isActivityProcess = (entryPoint == null);
        if (entryPoint == null)
            entryPoint = "android.app.ActivityThread";
        /* 调用Process.start接口来创建一个新的进程，并会创建一个android.app.ActivityThread类的对象，
         * 并且执行它的main函数，ActivityThread是应用程序的主线程         */
        Process.ProcessStartResult startResult =
                Process.start(entryPoint,
                app.processName, uid, uid, gids, debugFlags, mountExternal,
                app.info.targetSdkVersion, app.info.seinfo, requiredAbi, instructionSet,
                app.info.dataDir, entryPointArgs);
        ...
    } catch (RuntimeException e) {
        //创建进程失败
        Slog.e(TAG, "Failure starting process " + app.processName, e);
    }
}
```

  在第二个`startProcessLocked()`方法中主要进行一些判断，判断是否需要创建新进程；紧接着调用无返回值的`startProcessLocked()`方法，在这个方法中通过**Process.start**接口创建出新的进程。我们知道线程是程序执行的最小单元，线程栖息于进程中，每个进程在创建完毕后都会有一个主线程被开启，在大多数变成语言中线程的入口都是通过`main`函数， 这里也不例外，当进程创建完毕后，进程的主线程就被创建了，并会调用其`main`方法，Android中`ActivityThread`就代表着主线程，接着我们来到`ActivityThread`:

## step13: ActivityThread

```java
public final class ActivityThread {
    final ApplicationThread mAppThread = new ApplicationThread();
    //新的进程创建后就会执行这个main方法
    public static void main(String[] args) {
        ...

        Looper.prepareMainLooper();

        //创建一个ActivityThread实例，并调用他的attach方法
        ActivityThread thread = new ActivityThread();
        thread.attach(false);
        ...

        if (false) {
            Looper.myLooper().setMessageLogging(new
                    LogPrinter(Log.DEBUG, "ActivityThread"));
        }
        ...
        //进入消息循环
        Looper.loop();

        throw new RuntimeException("Main thread loop unexpectedly exited");
    }

    private void attach(boolean system) {
        sCurrentActivityThread = this;
        mSystemThread = system;
        if (!system) {
            ...
            /* ActivityManagerNative.getDefault()方法返回的是一个ActivityManagerProxy对象，
             * ActivityManagerProxy实现了IActivityManager接口，并维护了一个mRemote，
             * 这个mRemote就是ActivityManagerService的远程代理对象             */
            final IActivityManager mgr = ActivityManagerNative.getDefault();
            try {
                //调用attachApplication()，并将mAppThread传入，mAppThread是ApplicationThread类的示例，他的作用是用来进程间通信的
                mgr.attachApplication(mAppThread);
            } catch (RemoteException ex) {
                // Ignore
            }
            ...
        } else {
           ...
        }
     ...
    }
}
```

  `ActivityThread`是应用程序进程中的主线程，他的作用是调度和执行`activities`、广播和其他操作。 `main`方法开启了消息循环机制，并调用`attach()`方法，`attach()`方法会调用`ActivityManagerNative.getDefault()`获取到一个`ActivityManagerProxy`示例，上面**step3**中我们讲解了`ActivityManagerNative`这个类，`ActivityManagerProxy`中维护了`ActivityManagerService`的远程代理对象`mRemote`； 然后会调用`attachApplication()`方法通过`mRemote`调用到`ActivityManagerService`的`attachApplication()`中， 传入的`mAppThread`是`ApplicationThread`类型，`mAppThread`实际上通过`Handler`实现`ActivityManagerService`与`ActivityThread`的消息通信。

## step14: ActivityManagerProxy.attachApplication() （ActivityManagerNative内部类）

```java
public void attachApplication(IApplicationThread app) throws RemoteException
{
    Parcel data = Parcel.obtain();
    Parcel reply = Parcel.obtain();
    data.writeInterfaceToken(IActivityManager.descriptor);
    data.writeStrongBinder(app.asBinder());
    mRemote.transact(ATTACH_APPLICATION_TRANSACTION, data, reply, 0);
    reply.readException();
    data.recycle();
    reply.recycle();
}
```

  `attachApplication()`接受`IApplicationThread`实例，**step13**中`attach()`方法传入的`ApplicationThread`实现了`IApplicationThread`，然后通过`ActivityManagerService`的远程代理对象`mRemote`，进入`ActivityManagerService`的`attachApplication()`：

## step15: ActivityManagerService.attachApplication()

```java
@Override
public final void attachApplication(IApplicationThread thread) {
    synchronized (this) {
        int callingPid = Binder.getCallingPid();
        final long origId = Binder.clearCallingIdentity();
        //接着调用attachApplicationLocked()
        attachApplicationLocked(thread, callingPid);
        Binder.restoreCallingIdentity(origId);
    }
}

private final boolean attachApplicationLocked(IApplicationThread thread,
                                              int pid) {
    //① 获取到进程
    ProcessRecord app;
    if (pid != MY_PID && pid >= 0) {
        synchronized (mPidsSelfLocked) {
            app = mPidsSelfLocked.get(pid);
        }
    } else {
        app = null;
    }
    if (app == null) {
        if (pid > 0 && pid != MY_PID) {
            ...
            Process.killProcessQuiet(pid);
        } else {
            thread.scheduleExit();
            ...
        }
        return false;
    }
    ...
    //② 对app的一些成员变量进行初始化
    app.makeActive(thread, mProcessStats);
    app.curAdj = app.setAdj = -100;
    app.curSchedGroup = app.setSchedGroup = Process.THREAD_GROUP_DEFAULT;
    app.forcingToForeground = null;
    updateProcessForegroundLocked(app, false, false);
    app.hasShownUi = false;
    app.debugging = false;
    app.cached = false;
    app.killedByAm = false;

    ...
    boolean normalMode = mProcessesReady || isAllowedWhileBooting(app.info);
    ...
    boolean badApp = false;
    boolean didSomething = false;

    // See if the top visible activity is waiting to run in this process...
    /* ③ 检查当前进程中顶端的activity是否等着被运行，这个顶端的activity就是我们要启动的activity；
     *    此处适用于需要为activity创建新进程的情况（比如点击Launcher桌面上的图标启动应用，或者打开配置了process的activity）
     *    如果应用程序已经启动，在应用程序内部启动activity(未配置process)不会创建进程，这种情况回到step11中的第①步直接开启activity     */
    if (normalMode) {
        try {
            if (mStackSupervisor.attachApplicationLocked(app)) {
                didSomething = true;
            }
        } catch (Exception e) {
            Slog.wtf(TAG, "Exception thrown launching activities in " + app, e);
            badApp = true;
        }
    }

    // Find any services that should be running in this process...
    //④ 查找当前进程中应该启动的服务，并将其启动
    if (!badApp) {
        try {
            didSomething |= mServices.attachApplicationLocked(app, processName);
        } catch (Exception e) {
            Slog.wtf(TAG, "Exception thrown starting services in " + app, e);
            badApp = true;
        }
    }

    // Check if a next-broadcast receiver is in this process...
    //⑤ 查找当前进程中应该注册的广播
    if (!badApp && isPendingBroadcastProcessLocked(pid)) {
        try {
            didSomething |= sendPendingBroadcastsLocked(app);
        } catch (Exception e) {
            // If the app died trying to launch the receiver we declare it 'bad'
            Slog.wtf(TAG, "Exception thrown dispatching broadcasts in " + app, e);
            badApp = true;
        }
    }
    // Check whether the next backup agent is in this process...
    ...

    return true;
}
```

  `attachApplication()`方法调用了`attachApplicationLocked()`方法， 在**step12**中，我们创建了一个`ProcessRecord`，这里通过进程的*pid*将他取出来，赋值给app， 并初始化app的一些成员变量，然后为当前进程启动顶层activity、一些服务和广播; 这里我们就不深入研究到底启动的是那些，我们主要研究activity的启动，所以重点看第③步，*_ step6_*中最后创建了一个`ActivityRecord`实例r，这个r只是进程堆栈中的一个活动记录， 然后再**step8**中将这个r插入到堆栈最顶端，所以这个r相当于一个占位，并不是真正启动的`Activity`， 真正启动`Activity`需要判断进程是否存在，如果存在就直接启动，如果不存在需要启动进程后再执行此处第③步调用`ActivityStackSupervisor.attachApplicationLocked(ProcessRecord)`方法：

## step16: ActivityStackSupervisor.attachApplicationLocked(ProcessRecord)

```java
boolean attachApplicationLocked(ProcessRecord app) throws RemoteException {
    final String processName = app.processName;
    boolean didSomething = false;
    for (int displayNdx = mActivityDisplays.size() - 1; displayNdx >= 0; --displayNdx) {
        ArrayList<ActivityStack> stacks = mActivityDisplays.valueAt(displayNdx).mStacks;
        for (int stackNdx = stacks.size() - 1; stackNdx >= 0; --stackNdx) {
            //遍历进程中的堆栈，找到最顶层的堆栈
            final ActivityStack stack = stacks.get(stackNdx);
            if (!isFrontStack(stack)) {
                continue;
            }
            //
            /* 获取位于顶层堆栈中栈顶的activity，这个activity就是目标activity(需要被启动的);
             * 这个hr就是step6中创建的ActivityRecord实例r             */
            ActivityRecord hr = stack.topRunningActivityLocked(null);
            if (hr != null) {
                if (hr.app == null && app.uid == hr.info.applicationInfo.uid
                        && processName.equals(hr.processName)) {
                    try {
                        //调用realStartActivityLocked()方法启动activity，同step11中的第①步
                        if (realStartActivityLocked(hr, app, true, true)) {
                            didSomething = true;
                        }
                    } catch (RemoteException e) {
                        Slog.w(TAG, "Exception in new application when starting activity "
                                + hr.intent.getComponent().flattenToShortString(), e);
                        throw e;
                    }
                }
            }
        }
    }
    if (!didSomething) {
        ensureActivitiesVisibleLocked(null, 0);
    }
    return didSomething;
}
```

  这个方法中首先遍历进程中的堆栈，找到位于顶层的堆栈，然后调用`topRunningActivityLocked()` 获取位于栈顶的`ActivityRecord`记录，最后调用`realStartActivityLocked()`方法启动`activity`：

## ★★★step17: ActivityStackSupervisor.realStartActivityLocked()

```java
final boolean realStartActivityLocked(ActivityRecord r,
                                      ProcessRecord app, boolean andResume, boolean checkConfig)
        throws RemoteException {
    ...

    r.app = app;
    ...

    int idx = app.activities.indexOf(r);
    if (idx < 0) {
        app.activities.add(r);
    }
    final ActivityStack stack = r.task.stack;

    try {
        ...
        List<ResultInfo> results = null;
        List<ReferrerIntent> newIntents = null;
        if (andResume) {
            results = r.results;
            newIntents = r.newIntents;
        }
        ...
        //
        app.thread.scheduleLaunchActivity(new Intent(r.intent), r.appToken,
                System.identityHashCode(r), r.info, new Configuration(mService.mConfiguration),
                r.compat, r.launchedFromPackage, r.task.voiceInteractor, app.repProcState,
                r.icicle, r.persistentState, results, newIntents, !andResume,
                mService.isNextTransitionForward(), profilerInfo);
       ...
    } catch (RemoteException e) {
        if (r.launchFailed) {
            //This is the second time we failed -- finish activity and give up.
            ...
            return false;
        }
        // This is the first time we failed -- restart process and retry.
        app.activities.remove(r);
        throw e;
    }
    ...

    return true;
}
```

  这个方法调用`app.thread.scheduleLaunchActivity()`真正的启动一个`activity`， 这里`thread`是`IApplicationThread`的实例，也就是`ActivityThread`中的成员变量`ApplicationThread mAppThread`； 在**step15**的第②步初始化app时调用`app.makeActive(thread, mProcessStats)`为其赋值的。 我们接着看看`ApplicationThread.scheduleLaunchActivity()`:

## step18: ApplicationThread.scheduleLaunchActivity()

```java
@Override
public final void scheduleLaunchActivity(Intent intent, IBinder token, int ident,
                                         ActivityInfo info, Configuration curConfig, Configuration overrideConfig,
                                         CompatibilityInfo compatInfo, String referrer, IVoiceInteractor voiceInteractor,
                                         int procState, Bundle state, PersistableBundle persistentState,
                                         List<ResultInfo> pendingResults, List<ReferrerIntent> pendingNewIntents,
                                         boolean notResumed, boolean isForward, ProfilerInfo profilerInfo) {
    updateProcessState(procState, false);

    ActivityClientRecord r = new ActivityClientRecord();

    r.token = token;
    r.ident = ident;
    r.intent = intent;
    r.referrer = referrer;
    r.voiceInteractor = voiceInteractor;
    r.activityInfo = info;
    r.compatInfo = compatInfo;
    r.state = state;
    r.persistentState = persistentState;

    r.pendingResults = pendingResults;
    r.pendingIntents = pendingNewIntents;

    r.startsNotResumed = notResumed;
    r.isForward = isForward;

    r.profilerInfo = profilerInfo;

    r.overrideConfig = overrideConfig;
    updatePendingConfiguration(curConfig);

    sendMessage(H.LAUNCH_ACTIVITY, r);
}
```

  `ActivityThread`中有一个**H**的成员变量，它是一个`Handler`， 专门接受`ApplicationThread`发送的消息，然后调用`ActivityThread`中的方法， 我们看看H是怎样处理消息的：

## step19:ActivityThread.H

```java
public final class ActivityThread {
    ...
    final ApplicationThread mAppThread = new ApplicationThread();
    final H mH = new H();

    private void sendMessage(int what, Object obj) {
        sendMessage(what, obj, 0, 0, false);
    }
    ...
    private void sendMessage(int what, Object obj, int arg1, int arg2, boolean async) {
        if (DEBUG_MESSAGES) Slog.v(
                TAG, "SCHEDULE " + what + " " + mH.codeToString(what)
                        + ": " + arg1 + " / " + obj);
        Message msg = Message.obtain();
        msg.what = what;
        msg.obj = obj;
        msg.arg1 = arg1;
        msg.arg2 = arg2;
        if (async) {
            msg.setAsynchronous(true);
        }
        mH.sendMessage(msg);
    }

    private class H extends Handler {
        public void handleMessage(Message msg) {
            if (DEBUG_MESSAGES) Slog.v(TAG, ">>> handling: " + codeToString(msg.what));
            switch (msg.what) {
                case LAUNCH_ACTIVITY: {     //启动activity
                    ...
                    handleLaunchActivity(r, null);
                    ...
                }
                break;
                case RELAUNCH_ACTIVITY: {   //重新启动activity
                    ...
                    handleRelaunchActivity(r);
                    ...
                }
                break;
                case PAUSE_ACTIVITY:        //activity失去焦点
                    ...
                    handlePauseActivity((IBinder) msg.obj, false, (msg.arg1 & 1) != 0, msg.arg2,
                            (msg.arg1 & 2) != 0);
                    ...
                    break;
                ...
                case RESUME_ACTIVITY:       //activity获取焦点
                    Trace.traceBegin(Trace.TRACE_TAG_ACTIVITY_MANAGER, "activityResume");
                    handleResumeActivity((IBinder) msg.obj, true, msg.arg1 != 0, true);
                    Trace.traceEnd(Trace.TRACE_TAG_ACTIVITY_MANAGER);
                    break;
                ...
            }
        }
    }
}
```

  **step18**中调用`sendMessage(H.LAUNCH_ACTIVITY, r)`之后，**mH**会收到一个`LAUNCH_ACTIVITY`消息， 然后调用了`ActivityThread.handleLaunchActivity(r, null)`：

## step20:ActivityThread.handleLaunchActivity(r, null)

```java
private void handleLaunchActivity(ActivityClientRecord r, Intent customIntent) {
    ...
    //创建Activity对象，并初始化，然后调用activity.attach()和onCreate()
    Activity a = performLaunchActivity(r, customIntent);
    if (a != null) {
        r.createdConfig = new Configuration(mConfiguration);
        Bundle oldState = r.state;
        //调用activity.onResume()
        handleResumeActivity(r.token, false, r.isForward,
                !r.activity.mFinished && !r.startsNotResumed);

        ...
    } else {
        ...
    }
}
```

  这里首先调用`performLaunchActivity()`方法创建`Activity`对象(调用它的`attach()`和`onCreate()`方法)， 然后调用`handleResumeActivity`函数来使这个`Activity`进入`Resumed`状态，并回调这个`Activity`的`onResume`函数。我们接着看看`performLaunchActivity()`方法中做了什么：

## step21: ActivityThread.performLaunchActivity()

```java
 private Activity performLaunchActivity(ActivityClientRecord r, Intent customIntent) {
    //① 收集要启动的Activity的相关信息，主要package和component
    ActivityInfo aInfo = r.activityInfo;
    ...
    ComponentName component = r.intent.getComponent();
    ...

    Activity activity = null;
    try {
        //② 通过类加载器将activity的类加载进来，然后创建activity对象
        java.lang.ClassLoader cl = r.packageInfo.getClassLoader();
        activity = mInstrumentation.newActivity(
                cl, component.getClassName(), r.intent);
        StrictMode.incrementExpectedActivityCount(activity.getClass());
        r.intent.setExtrasClassLoader(cl);
        r.intent.prepareToEnterProcess();
        if (r.state != null) {
            r.state.setClassLoader(cl);
        }
    } catch (Exception e) {
        ...
    }

    try {
        //③ 创建Application，也就是AndroidManifest.xml配置的<application/>
        Application app = r.packageInfo.makeApplication(false, mInstrumentation);
        ...
        if (activity != null) {
            //④ 创建Activity的上下文Content，并通过attach方法将这些信息设置到Activity中去
            Context appContext = createBaseContextForActivity(r, activity);
            CharSequence title = r.activityInfo.loadLabel(appContext.getPackageManager());
            Configuration config = new Configuration(mCompatConfiguration);

            //⑤ 调用activity的attach()方法，这个方法的作用主要是为activity初始化一些成员变量
            activity.attach(appContext, this, getInstrumentation(), r.token,
                    r.ident, app, r.intent, r.activityInfo, title, r.parent,
                    r.embeddedID, r.lastNonConfigurationInstances, config,
                    r.referrer, r.voiceInteractor);

            if (customIntent != null) {
                activity.mIntent = customIntent;
            }
            r.lastNonConfigurationInstances = null;
            activity.mStartedActivity = false;
            int theme = r.activityInfo.getThemeResource();
            if (theme != 0) {
                activity.setTheme(theme);
            }

            activity.mCalled = false;
            /* ⑥ 通过mInstrumentation调用activity的onCreate()方法，
             *    mInstrumentation的作用是监控Activity与系统的交互操作。
             *    这时候activity的生命周期就开始了             */
            if (r.isPersistable()) {
                mInstrumentation.callActivityOnCreate(activity, r.state, r.persistentState);
            } else {
                mInstrumentation.callActivityOnCreate(activity, r.state);
            }
            //下面主要为堆栈中的ActivityClientRecord设置一些数据
            ...
            r.activity = activity;
            r.stopped = true;
            if (!r.activity.mFinished) {
                activity.performStart();
                r.stopped = false;
            }
            ...
        }
        r.paused = true;
        ...
    } catch (SuperNotCalledException e) {
        throw e;

    } catch (Exception e) {
        if (!mInstrumentation.onException(activity, e)) {
            throw new RuntimeException(
                    "Unable to start activity " + component
                            + ": " + e.toString(), e);
        }
    }

    return activity;
}
```

  `performLaunchActivity ()`中首先根据`activity`的`className`加载类文件，并创建`activity`实例，然后初始化上下文`Context`，并调用`attach()`方法初始化`activity`，最后通过`mInstrumentation`回调`activity`的`onCreate()`方法，这样`Activity`算是创建完成了。 

  这篇博客涉及到的内容还是比较多，每个过程中主要的代码注释已经标示的比较清楚了，还有些细节问题有兴趣的可以更加深入的去研究，相信如果弄懂这个流程后，再去深入也不是什么难事了。项目中framwork层的源码都是参照android5.0版本，如果下载源码有问题的同学可以在本篇博客末尾下载博客研究的工程（主要的源码已经摘录放入到工程中），也可以参考[http://grepcode.com/project/repository.grepcode.com/java/ext/com.google.android/android/](http://grepcode.com/project/repository.grepcode.com/java/ext/com.google.android/android/)。

## 总结：

**step1-step3**：通过`Binder`机制将创建`activity`的消息传给`ActivityManagerService`

**step4-step8**：根据`activity`的启动模式判断`activity`是否应该重新创建，还是只需要置于栈顶，并将目标`activity`记录放在栈顶

**step9-step10**：判断`activity`是否已经创建，如果已经创建将直接置于`resume`状态，如果没有创建便重新开启

**step11-step12**：判断`activity`所属的进程是否创建，如果没有创建将启动新的进程，如果创建了就直接启动`activity`

**step13-step15**：进程的主线程执行`ActivityThread`的`main`方法，然后初始化进程相关的东西

**step16-step21**：实例化栈顶的activity对象，并调用Activity.attach()初始化，回调`onCreate()`生命周期方法；然后调用`ActivityThread.handleResumeActivity()`使`activity`处于`resume`状态

## 源码工程：

[https://github.com/openXu/AndroidActivityLaunchProgress](https://github.com/openXu/AndroidActivityLaunchProgress)

# 二、layout的inflate过程

  以上，我们探讨了Activity的启动，从startActivity()到**进程创建**，再到**activity的创建**，最后调用**onCreate**()方法。
  以下，我们接着onCreate()方法继续研究Activity加载layout的过程。我们写好layout布局后，在**onCreate**()方法**中**调用**setContentView**(layoutID)就能将我们的布局加载显示。这一过程到底做了什么？布局中的控件是怎样被加载并显示出来的？我们从setContentView()方法开始一步步分析 。（参考源码版本为API23 ）

## step1.Activity.setContentView()

  通常我们的做法是在`OnCreate()`方法中设置布局资源**ID**，`Activity`提供了三个设置视图的方法，如下：

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity);
    //setContentView(new TextView(this));
    //setContentView(new TextView(this),new ViewGroup.LayoutParams(...));
}
```

再看看`Activity`中的调用关系：

```java
public class Activity ...{
    private Window mWindow;

    //三个设置视图的方法
    public void setContentView(@LayoutRes int layoutResID) {
        getWindow().setContentView(layoutResID);
        //初始化ActionBar
        initWindowDecorActionBar();
    }
    public void setContentView(View view) {
        getWindow().setContentView(view);
        initWindowDecorActionBar();
    }
    public void setContentView(View view, ViewGroup.LayoutParams params) {
        getWindow().setContentView(view, params);
        initWindowDecorActionBar();
    }

    final void attach(...) {
        ...
        //初始化mWindow
        mWindow = new PhoneWindow(this);
        ...
    }
    public Window getWindow() {
        return mWindow;
    }
}
```

  原来Activity中调用setContentView()方法**最终调用**的是**PhoneWindow**对象mWindow 的**setContentView**(...)方法。 
  **mWindow**是在Activity创建的时候在**attach()**方法**中初始化**的，attach()在【Activity的创建过程】中有讲解。 
  **每一个Activity组件都有一个关联的**Window的实现类**PhoneWindow**的对象mWindow ，用来描述一个应用程序窗口，它封装了顶层窗口的外观和行为策略，它提供了标准的用户界面策略，如背景、标题区域、默认键处理等。
  **PhoneWindow**管理着整个屏幕的内容，不包括屏幕最顶部的系统状态条。所以，**PhoneWindow**或者**Window**是与应用的一个页面所相关联。

下面我们查看PhoneWindow.setContentView()方法：

## step2. PhoneWindow.setContentView()

```java
@Override
public void setContentView ( int layoutResID){
    //此处判断mContentParent是否为null，如果是null，则是第一次调用setContentView
    if (mContentParent == null) {
        //第一次需要初始化窗口和根布局
        installDecor();
    } else if (!hasFeature(FEATURE_CONTENT_TRANSITIONS)) {
        //如果不是第一次调用，需要清空根布局中的内容
        //移除该mContentParent内所有的所有子View
        mContentParent.removeAllViews();
    }
    if (hasFeature(FEATURE_CONTENT_TRANSITIONS)) {
        final Scene newScene = Scene.getSceneForLayout(mContentParent, layoutResID,
                getContext());
        transitionTo(newScene);
    } else {
        //将我们的资源文件通过LayoutInflater对象转换为View树，并且添加至mContentParent中
        mLayoutInflater.inflate(layoutResID, mContentParent);
    }
    mContentParent.requestApplyInsets();
    final Callback cb = getCallback();
    if (cb != null && !isDestroyed()) {
        cb.onContentChanged();
    }
}

@Override
public void setContentView (View view){
    setContentView(view, new ViewGroup.LayoutParams(MATCH_PARENT, MATCH_PARENT));
}

@Override
public void setContentView (View view, ViewGroup.LayoutParams params){
    if (mContentParent == null) {
        installDecor();
    } else if (!hasFeature(FEATURE_CONTENT_TRANSITIONS)) {
        mContentParent.removeAllViews();
    }
    if (hasFeature(FEATURE_CONTENT_TRANSITIONS)) {
        view.setLayoutParams(params);
        final Scene newScene = new Scene(mContentParent, view);
        transitionTo(newScene);
    } else {
        mContentParent.addView(view, params);
    }
    mContentParent.requestApplyInsets();
    final Callback cb = getCallback();
    if (cb != null && !isDestroyed()) {
        cb.onContentChanged();
    }
}
```

  PhoneWindow中setContentView(view)接着调用setContentView(view, layoutParams)，而setContentView(layoutResID)和setContentView(view, layoutParams)方法中的步骤是差不多的，唯一的区别就是：
• **setCOntentView(layoutResID)**中是通过**mLayoutInflater.inflate**(layoutResID, mContentParent)将布局填充到**mContentParent**上，
• 而**setContentView(view, layoutParams)**是将传过来的**view**直接 **add**到**mContentParent**中 。

  到这一步我们发现，我们为某个activity写的layout视图**最终**是**添加到**一个叫**mContentParent**的ViewGroup中了。 
  在加入mContentParent中之前，首先判断如果mContentParent==null时，执行了installDecor()方法，我们猜想，**installDecor**()的作用大概就是**初始化mContentParent**，如果mContentParent已经初始化，而且窗口不是透明的，就清除mContentParent中的所有视图。**mContentParent只**会**实例化一次**，所以如果我们在Activity中多次调用setContentView()只是改变了mContentParent的子视图（也就是我们写的布局文件）。

接着看看installDecor()：

## step3. PhoneWindow.installDecor(）

  上一步我们猜测installDecor()方法的作用是实例化mContentParent，接下来我们深入PhoneWindow中验证这个猜测对不对，下面是PhoneWindow的关键代码：

```java
public class PhoneWindow extends Window implements MenuBuilder.Callback {
    //id为content的容器，这个容器就是用于盛放我们写的layout视图的
    private ViewGroup mContentParent;
    //mContentRoot是整个界面内容，包括title和content等等
    private ViewGroup mContentRoot;
    //窗口顶层FrameLayout，用于盛放mContentRoot
    private DecorView mDecor;

    // DecorView是FrameLayout的子类
    private final class DecorView extends FrameLayout{
        ...
    }

    //实例化了一个DecorView对象
    protected DecorView generateDecor() {
        return new DecorView(getContext(), -1);
    }

    // 初始化顶层窗口mDecor和根布局mContentParent
    private void installDecor() {
        if (mDecor == null) {
            //①.初始化窗口
            mDecor = generateDecor();
            mDecor.setDescendantFocusability(ViewGroup.FOCUS_AFTER_DESCENDANTS);
            mDecor.setIsRootNamespace(true);
            ...
        }
        if (mContentParent == null) {
            //②.如果根布局
            mContentParent = generateLayout(mDecor);
            //③.初始化title和一些设置
            if (decorContentParent != null) {
                mDecorContentParent = decorContentParent;
                mDecorContentParent.setWindowCallback(getCallback());
                if (mDecorContentParent.getTitle() == null) {
                    mDecorContentParent.setWindowTitle(mTitle);
                }
                ...
            } else {
                mTitleView = (TextView) findViewById(R.id.title);
                if (mTitleView != null) {
                    mTitleView.setLayoutDirection(mDecor.getLayoutDirection());
                    if ((getLocalFeatures() & (1 << FEATURE_NO_TITLE)) != 0) {
                        ...
                    } else {
                        mTitleView.setText(mTitle);
                    }
                }
            }

            if (mDecor.getBackground() == null && mBackgroundFallbackResource != 0) {
                mDecor.setBackgroundFallback(mBackgroundFallbackResource);
            }
            ...
        }
    }
}
```

  PhoneWindow中引用了一个DecorView的对象，DecorView是FrameLayout的子类，相信你们应该多多少少知道Activity的顶层窗口是一个FramLayout，也正是这个DevorView的对象**mDecor**。

  在installDecor()方法中，第①步就是判断**mDecor是否为null**，如果为null，将会调用**generateDecor**()方法实例化一个DecorView对象，紧接着第②步判断**mContentParent是否为null**，如果为null，将调用**generateLayout**(mDecor)方法初始化mContentParent。

  现在就有一个疑问了，mDecor和mContentParent都是容器，他们是什么关系？各自代表的是屏幕中那一块的内容?带着问题我们看看generateLayout(mDecor)方法：

## step 4. PhoneWindow.generateLayout(mDecor )

```java
// 初始化根布局mContentParent
protected ViewGroup generateLayout(DecorView decor) {
    //①.获取AndroidManifest.xml中指定的themes主题
    TypedArray a = getWindowStyle();
    //设置当前窗口是否有标题
    if (a.getBoolean(R.styleable.Window_windowNoTitle, false)) {
        //请求指定Activity窗口的风格类型
        requestFeature(FEATURE_NO_TITLE);
    } else if (a.getBoolean(R.styleable.Window_windowActionBar, false)) {
        requestFeature(FEATURE_ACTION_BAR);
    }
    ...
    //设置窗口是否全屏
    if (a.getBoolean(R.styleable.Window_windowFullscreen, false)) {
        setFlags(FLAG_FULLSCREEN, FLAG_FULLSCREEN & (~getForcedWindowFlags()));
    }
    ...

    //②.根据上面设置的窗口属性Features, 设置相应的修饰布局文件layoutResource，这些xml文件位于frameworks/base/core/res/res/layout下
    int layoutResource;
    int features = getLocalFeatures();
    if ((features & (1 << FEATURE_SWIPE_TO_DISMISS)) != 0) {
        layoutResource = R.layout.screen_swipe_dismiss;
    } else if
        ...

    mDecor.startChanging();
    //③.将第2步选定的布局文件inflate为View树，这也是整个窗口的内容（包括title、content等等）
    View in = mLayoutInflater.inflate(layoutResource, null);
    //④.将整个窗口内容添加到根mDecor中
    decor.addView(in, new ViewGroup.LayoutParams(MATCH_PARENT, MATCH_PARENT));
    //⑤.将整个窗口内容赋值给mContentRoot
    mContentRoot = (ViewGroup) in;

    //⑥.将窗口修饰布局文件中id="@android:id/content"的View赋值给mContentParent
    ViewGroup contentParent = (ViewGroup) findViewById(ID_ANDROID_CONTENT);
    if (contentParent == null) {
        throw new RuntimeException("Window couldn't find content container view");
    }
    ...

    mDecor.finishChanging();

    return contentParent;
}
```

- 在generateLayout()中，**第①步**是**初始化**了一些**样式属性**值。
  我们发现有一个很熟悉的类**TypedArray**，没错，这在之前讲解[自定义View](http://blog.csdn.net/moira33/article/details/79017920)的【自定义控件属性】的时候用到过，其实此处也是一样的，之前自定义控件的属性是用来描述我们自定义的View，而这里的样式属性是用来**描述窗口**。
  而我们知道**窗口**实质上**也是View**，唯一的区别就是，控件的属性是在layout中设置，而窗口的**属性**是**在AndroidManifest.xml中配置**的，通过**getWindowStyle**()**获取**当前**Window**在theme中定义的**属性**，window支持的属性可以参考\frameworks\base\core\res\res\values\attrs.xml 中的\<declare-styleable name="Window"> 。

  获取到属性值之后有与大堆代码是调用**setFlags**()和**requestFeature**()给当前**window设置属性**值。
  	这就是为什么我们一般在Activity的onCreate()中设置**全屏等属性需**要在**setContentView**()**之前**设置，因为setContentView()**之后**installDecor()方法已经执行完毕，所以设置是**没有效**的。

- 第①步执行完后，Window的相关属性已经设置完毕，比如是否是全屏？是否有标题等等。

  然后**第②步**就是根据Window的各种属性，**设置相应的布局文件**，如果是全屏layoutResource（布局文件ID）是多少，如果有标题…；

- 确定layoutResource之后，**第③步**就是**mLayoutInflater**解析出layoutResource对应的**视图in**；

- **第④步**将***in***这个视图**add到mDecor**顶层窗口中；

- **第⑤步**将**in赋值给mContentRoot**；

- **第⑥步**调用findViewById()找到id为**content**的View赋值给**mContentParent**。

### mDecor 、mContentRoot 、mContentParent

通过generateLayout ()方法，我们发现了三个比较重要的视图对象mDecor 、mContentRoot 、mContentParent。他们的关系如下：

- **mDecor**是Activity的**顶层窗体**，他是FramLayout的子类对象；
- **mContentRoot**是根据设置给窗体加载的**整个Activity可见**的**视图**，这个视图包含标题栏（如果主题设置有标题），用于容纳我们自定义layout的id为content的容器，mContentRoot被**添加到**了顶层窗口**mDecor**中；
- **mContentParent**是mContentRoot中**id**为**content**的容器，这个容器就是用来**添加**我们写的**layout布局**文件的。
- **mContentParent是嵌套在mContentRoot中，mContentRoot嵌套在mDecor**。所以在上面第⑥步可以直接调用findViewById()找到mContentParent。（跟踪findViewById()方法，发现调用的是PhoneWindow中mDecor这个顶层窗口的findViewById()方法 ）

**关系图如下：** 

![mDecor 、mContentRoot 、mContentParent](http://img.blog.csdn.net/20180111230233523?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

## step5. LayoutInflater.inflate()

  经过上面的步骤，Activity的顶层窗口mDecor和用于容纳我们写的layout的容器mContentParent已经初始化完毕，接下来就是将我们的布局layout添加到mContentParent中，先把**step2** 中的PhoneWindow.setContentView(int layoutResID)方法贴出来：

```java
//PhoneWindow.setContentView
@Override
public void setContentView ( int layoutResID){
    if (mContentParent == null) {
        installDecor();
    } else if (!hasFeature(FEATURE_CONTENT_TRANSITIONS)) {
        //移除该mContentParent内所有的所有子View
        mContentParent.removeAllViews();
    }
    if (hasFeature(FEATURE_CONTENT_TRANSITIONS)) {
        final Scene newScene = Scene.getSceneForLayout(mContentParent, layoutResID,
                getContext());
        transitionTo(newScene);
    } else {
        //将我们的资源文件通过LayoutInflater对象转换为View树，并且添加至mContentParent中
        mLayoutInflater.inflate(layoutResID, mContentParent);
    }
    mContentParent.requestApplyInsets();
    final Callback cb = getCallback();
    if (cb != null && !isDestroyed()) {
        cb.onContentChanged();
    }
}
```

  在分析PhoneWindow.setContentView()方法的过程中，我们穿插讲解了installDecor()方法。

接着有一个判断hasFeature(**FEATURE_CONTENT_TRANSITIONS**)，这个属性是Android5.0引入的，这篇博客中参照的是API23的源码 ，他的意思根据设置的主题属性，判断当前窗口内容变化的过程**是否需要动画**，如果有动画标志，将执行动画。

**Scene**是场景，这个对象中**引用了**mSceneRoot和mlayout两个视图，**mSceneRoot**就是mContentParent，而**mLayout**就是我们的布局视图。
我们发现另一个setContentView(view)方法中直接将view传递进去了Scene newScene = new Scene(mContentParent, view);，而setContentView(layoutResID)是将layoutResID传递进去，可以想象在Scene中也会根据此layoutResID将我们的布局layout加载并赋值给mLayout。

由于我们主要研究的是View的加载过程，所以就不深入讲解动画了。直接看下面的**mLayoutInflater.inflate**(layoutResID, mContentParent)，这一步的目的就是**解析**我们的**布局layout**，并将布局视图**挂载到mContentParent**上，那下面看看LayoutInflater.inflate()方法：

```java
//LayoutInflater.inflate
public View inflate(@LayoutRes int resource, @Nullable ViewGroup root) {
    return inflate(resource, root, root != null);
}
public View inflate(@LayoutRes int resource, @Nullable ViewGroup root, boolean attachToRoot) {
    final Resources res = getContext().getResources();
    if (DEBUG) {
        Log.d(TAG, "INFLATING from resource: \"" + res.getResourceName(resource) + "\" ("
                + Integer.toHexString(resource) + ")");
    }

    final XmlResourceParser parser = res.getLayout(resource);
    try {
        return inflate(parser, root, attachToRoot);
    } finally {
        parser.close();
    }
}
```

这个方法有三个参数：

- **resource**：需要解析的布局layout的id
- **root**：解析layout之后得到的视图层级的父视图
- **attachToRoot**：是否将解析出来的视图添加到父视图中，如果传入true，并且root不为null，这个方法返回的是root，而且将解析出的视图添加到root中。
  而我们看到inflate(resource)方法在inflate(resource,root,attachToRoot)方法时，传入的attachToRoot是root!=null，所以inflate**返回**的是**已经将布局layout视图添加到mContentParent后的mContentParent**。

**inflate**(resource,root,attachToRoot )方法中**通过res.getLayout**(resource)将layout**关联**到一个**XmlResourceParser**中（Android内置的pull解析器），然后调用inflate(parser,root,attachToRoot )方法：

```java
// 将layout解析为view树，并附加到root（mContentParent）中
public View inflate(XmlPullParser parser, ViewGroup root, boolean attachToRoot) {
    synchronized (mConstructorArgs) {
        Trace.traceBegin(Trace.TRACE_TAG_VIEW, "inflate");

        final Context inflaterContext = mContext;
        final AttributeSet attrs = Xml.asAttributeSet(parser);
        Context lastContext = (Context) mConstructorArgs[0];
        mConstructorArgs[0] = inflaterContext;
        //①.将最终返回的View初始化为root(也就是mContentParent)
        View result = root;
        try {
            int type;
            //②.循环直到解析到开始标签<>或者结尾标签</>
            while ((type = parser.next()) != XmlPullParser.START_TAG &&
                    type != XmlPullParser.END_DOCUMENT) {
                // Empty
            }
            //第一次解析到的不是开始标签<>，说明layout文件没有<>标签,xml格式错误
            if (type != XmlPullParser.START_TAG) {
                throw new InflateException(parser.getPositionDescription()
                        + ": No start tag found!");
            }

            //③.找到第一个开始标签，这个标签对应的name就是整个layout最外层的父控件
            final String name = parser.getName();
            ...
            if (TAG_MERGE.equals(name)) {
                if (root == null || !attachToRoot) {
                    throw new InflateException("<merge /> can be used only with a valid "
                            + "ViewGroup root and attachToRoot=true");
                }
                rInflate(parser, root, inflaterContext, attrs, false);
            } else {
                // Temp is the root view that was found in the xml
                //★④.根据layout中第一个开始标签的名称创建一个View对象temp，temp就是整个xml中的根控件
                final View temp = createViewFromTag(root, name, inflaterContext, attrs);

                ViewGroup.LayoutParams params = null;
                if (root != null) {
                    // Create layout params that match root, if supplied
                    // 根据父控件获取布局参数，后面将解析的view树添加到root中是要使用
                    params = root.generateLayoutParams(attrs);
                    //如果不需要附加到root父控件中
                    if (!attachToRoot) {
                        // Set the layout params for temp if we are not
                        // attaching. (If we are, we use addView, below)
                        //为temp设置布局参数如果我们不附加。（如果我们是，我们使用addView，下同）
                        temp.setLayoutParams(params);
                    }
                }
                // Inflate all children under temp against its context.
                //★⑤.调用rInflateChildren递归解析temp中的所有子控件，通过这行代码整个layout就被解析为view树了
                rInflateChildren(parser, temp, attrs, true);

                //★⑥.如果root不为空，将view树添加到root中
                //此处root为mContentParent，也就是将layout布局添加到mContentParent中了
                if (root != null && attachToRoot) {
                    root.addView(temp, params);
                }
                if (root == null || !attachToRoot) {
                    //如果不用附加到root中，直接返回解析的view树
                    result = temp;
                }
            }
        } catch (XmlPullParserException e) {
            InflateException ex = new InflateException(e.getMessage());
            ex.initCause(e);
            throw ex;
        } catch (Exception e) {
            InflateException ex = new InflateException(
                    parser.getPositionDescription()
                            + ": " + e.getMessage());
            ex.initCause(e);
            throw ex;
        } finally {
            // Don't retain static reference on context.
            mConstructorArgs[0] = lastContext;
            mConstructorArgs[1] = null;
        }

        Trace.traceEnd(Trace.TRACE_TAG_VIEW);

        return result;
    }
}
```

  这个方法的作用就是**将layout填充到一个view树上**，然后**将view树附加到root**（也就是**mContentParent**）**中**，然后返回root。 
  方法中有6个重要步骤，上面注释已经写得很清楚了，注意签名带★的步骤是很重要的。
  在①-④步是将layout最外层的控件解析出来，在第④步中调用了**createViewFromTag**()方法（请看step6） 根据name实例化一个View对象，
  然后第⑤步调用**rInflateChildren**()方法，将剩余的控件解析出来后填充进最外层控件，这样就完成了整个layout的填充，
  最后第⑥步将解析出来的**view树添加到root中**。
  我们发现真正完成inflate的并不是这个方法，这个方法只是解析了在外层的控件，剩余的控件是由rInflateChildren()方法完成的，而rInflateChildren()中调用的是rInflate()方法（请看step7）

## step6. LayoutInflater.createViewFromTag()

```java
/* 根据控件名实例化控件对象
 * @param parent 父控件
 * @param name 控件名
 * @param context
 * @param attrs
 * @param ignoreThemeAttr
 * @return */
View createViewFromTag(View parent, String name, Context context, AttributeSet attrs,
                       boolean ignoreThemeAttr) {
    if (name.equals("view")) {
        name = attrs.getAttributeValue(null, "class");
    }

    // Apply a theme wrapper, if allowed and one is specified.
    if (!ignoreThemeAttr) {
        final TypedArray ta = context.obtainStyledAttributes(attrs, ATTRS_THEME);
        final int themeResId = ta.getResourceId(0, 0);
        if (themeResId != 0) {
            context = new ContextThemeWrapper(context, themeResId);
        }
        ta.recycle();
    }

    if (name.equals(TAG_1995)) {
        // Let's party like it's 1995!
        return new BlinkLayout(context, attrs);
    }

    try {
        View view;
        if (mFactory2 != null) {
            view = mFactory2.onCreateView(parent, name, context, attrs);
        } else if (mFactory != null) {
            view = mFactory.onCreateView(name, context, attrs);
        } else {
            view = null;
        }

        if (view == null && mPrivateFactory != null) {
            view = mPrivateFactory.onCreateView(parent, name, context, attrs);
        }

        if (view == null) {
            final Object lastContext = mConstructorArgs[0];
            mConstructorArgs[0] = context;
            try {
                //先判断name中是否有'.'字符，如果没有，此控件为android自带的View，此时会在name的前面加上包名"android.view."
                if (-1 == name.indexOf('.')) {
                    view = onCreateView(parent, name, attrs);
                } else {
                    //如果有这个'.'，则认为是自定义View，因为自定义View在使用的时候使用的全名，所以直接创建
                    view = createView(name, null, attrs);
                }
            } finally {
                mConstructorArgs[0] = lastContext;
            }
        }

        return view;
    } catch (InflateException e) {
        throw e;

    } catch (ClassNotFoundException e) {
        final InflateException ie = new InflateException(attrs.getPositionDescription()
                + ": Error inflating class " + name);
        ie.initCause(e);
        throw ie;

    } catch (Exception e) {
        final InflateException ie = new InflateException(attrs.getPositionDescription()
                + ": Error inflating class " + name);
        ie.initCause(e);
        throw ie;
    }
}
```

## step7. LayoutInflater.rInflate()

```java
/* 解析layout最外层parent中的所有子控件
 * 此方法为递归方法，layout中有多少个ViewGroup就会递归调用多少次
 * 每一次调用就会完成layout中某一个ViewGroup中所有的子控件的解析 */
final void rInflateChildren(XmlPullParser parser, View parent, AttributeSet attrs,
                            boolean finishInflate) throws XmlPullParserException, IOException {
    rInflate(parser, parent, parent.getContext(), attrs, finishInflate);
}
void rInflate(XmlPullParser parser, View parent, Context context,
              AttributeSet attrs, boolean finishInflate) throws XmlPullParserException, IOException {
    final int depth = parser.getDepth();
    int type;
    //如果遇到结束标签（</>）就结束，说明此parent中所有的子view已经解析完毕
    while (((type = parser.next()) != XmlPullParser.END_TAG ||
            parser.getDepth() > depth) && type != XmlPullParser.END_DOCUMENT) {
        if (type != XmlPullParser.START_TAG) {
            continue;
        }
        //1.找到开始标签<>
        final String name = parser.getName();
        //2.根据name类型分别解析
        if (TAG_REQUEST_FOCUS.equals(name)) {
            parseRequestFocus(parser, parent);
        } else if (TAG_TAG.equals(name)) {
            parseViewTag(parser, parent, attrs);
        } else if (TAG_INCLUDE.equals(name)) {
            if (parser.getDepth() == 0) {
                throw new InflateException("<include /> cannot be the root element");
            }

            /* 如果是<include />，调用parseInclude方法用于解析<include/>标签：
             * ①.根据include标签的name属性找到对应的layout的id
             * ②.遍历开始标签解析layout中的view
             * ③.调用rInflateChildren(childParser, view, childAttrs, true)解析view中的子控件
             * ④.将view添加（add）进parent中             */
            parseInclude(parser, context, parent, attrs);
        } else if (TAG_MERGE.equals(name)) {
            throw new InflateException("<merge /> must be the root element");
        } else {
            //如果是普通View,调用createViewFromTag创建view对象
            final View view = createViewFromTag(parent, name, context, attrs);
            final ViewGroup viewGroup = (ViewGroup) parent;
            final ViewGroup.LayoutParams params = viewGroup.generateLayoutParams(attrs);
            //★递归调用rInflateChildren解析view中的子控件
            //如果view不是ViewGroup，rInflateChildren()会在while的第一次循环结束
            //如果view是ViewGroup，并且里面有子控件，通过这行代码view中的所有子控件就被挂到view上了
            rInflateChildren(parser, view, attrs, true);
            //将view树添加到viewGroup中，到此为止完成一个view及其所有子控件的填充
            viewGroup.addView(view, params);
        }
    }
    if (finishInflate) {
        /* ★parent的所有子控件都inflate完毕后调用onFinishInflate方法
         * 这个方法在自定义ViewGroup的时候经常用到，自定义ViewGroup中
         * 不能在构造方法中find子控件，因为构造方法中并没有完成子控件的实例化，
         * 只能在onFinishInflate回调方法中findViewById来初始化子控件         */
        parent.onFinishInflate();
    }
}
```

  **rInflate**()方法无非就是**根据剩余**的**xml****找**到**开始标签的name**，然后**根据name的类型**分别**解析**，如果判断是**普通控件**，调用**createViewFromTag**()**创建**一个控件**view**，接着递归调用rInflateChildren()解析view中的所有子控件（如果view是ViewGroup），最后**将view添加到parent**中。rInflateChildren()递归调用执行完毕后，整个layout就被填充为view树了，最后在inflate()中，layout的view树会被add到root中，也就是mContentParent中，整个窗体的view树mDecor就算是填充完毕。

## 总结
  此章节分析的是**View的加载填充原理**，也就是**从调用setContentView()方法开始**，我们的布局**layout**是**怎样填充为整个View树**，并被**挂载到Activity**上的。其中有几个重要的知识点如下：

1. **每**一个**Activity**组件**都有一个**关联的Window的实现类PhoneWindow的对象**mWindow** ，mWindow**管理**着**整个屏幕**的内容，不包括屏幕最顶部的系统状态条 ，它描述一个应用程序窗口,它封装了顶层窗口的外观和行为策略，它提供了标准的用户界面策略，如背景、标题区域、默认键处理等；
2. **Activity**的**setContentView**()方法里面**调用**的是**PhoneWindow**的**setContentView**()方法；
3. **PhoneWindow**中**引用**了**mDecor**（顶层窗口，FramLayout的子类）、**mContentRoot**（整个Activity的内容，包括TitleActionBar等）、**mContentParent**（mContentRoot中id为content的容器，用于放置我们的layout的容器）；他们三者的关系是mDecor嵌套mContentRoot，mContentRoot嵌套mContentParent；
4. 如果**mContentParent不为null**，将**清空**其中的**内容**，然后**重新加载layout**到mContentParent中 ；
  如果mContentParent**为null**说明是第一次调用setContentView，这时候需要调用**installDecor**()为Activity**加载**一个顶层窗口**mDecor**，mContentParent；
5. **installDecor**()方法中**初始化**了**mDecor**，然后**调用generateLayout**(mDecor)；
6. **generateLayout**(mDecor) 中首先**设置**了**window**的**主题**样式，并根据这些样式设置为Activity**加载**一个合适的**布局**视图，并将这个视图**赋值**给**mContentRoot** ，然后将此视图**add**到**mDecor**顶层窗口中；然后通过mDecor.findViewById(R.id.content)**初始化mContentParent**。通过②-⑥步，Activity中的顶层窗体的View树算是搭建完毕了；
7. **setContentView**(layoutId)中完成上面步骤后紧接着**调用LayoutInflater.inflate**()将我们传入的**layoutId填充为View树**，**inflate**()只是**解析**了layout布局的**最外层父控件**，里面的**子控件**是**通过rInflateChildren()**方法**递归解析**完成的。在解析的过程中如果遇到开始<>标签会调用**createViewFromTag**()方法**实例化**一个**View**对象，并解析为view设置的属性attrs。inflate()方法执行完毕后，layout就被映射为View树了，然后将此View树add到mContentParent中，整个Activity的view树就形成了。

# 三、View的显示过程measure、layout、draw

  在上文**《一、Activity的创建过程》**中，我们分析了从调用`startActivtiy()`到`Activtiy`创建完成的整个过程。其中**step20：ActivtiyThread.handleLaunchActivity(r, null)**这一步中有两个重要的步骤，第一步就是调用`performLaunchActivtiy(r, customIntent)`，这个方法中首先创建了需要打开的activity对象，然后调用其`activtiy.attach()`方法完成activtiy对象的初始化，最后调用其`onCreate()`方法，这时候activity的生命周期就开始了。
  而在上文**《二、layout的inflate过程》**中，我们分析了在`onCreate()`方法中，从调用`setContentView()`到**layout布局树加载完毕**的过程，到此为止，**Activtiy并不会展示**到用户眼前，因为**layout只是被填充成一个框架**了，就像修房子，首先得画出设计图纸，布局树就像是房子的设计图，设计图画好了还得按照设计图纸一砖一瓦的盖房，所有有了布局树，还需要为上面的每一个控件测量大小，安排摆放的位置并画到屏幕上。
  以下我们一起分析**Activity窗口的测量、布局和绘制的过程**。

  在讲解之前，我们先通过关键源码了解一些重要的类之间的关系：`Activity`、`Window`、`PhoneWindow`、`ViewManager`、`WindowManager`、`WindowManagerImpl`，`WindowManagerGlobal`，`WindowmanagerService`：

```java
/**Window系列*/
public abstract class Window {
    private WindowManager mWindowManager;
    public void setWindowManager(android.view.WindowManager wm, IBinder appToken, String appName,
                                 boolean hardwareAccelerated) {
        ...
        //调用WindowManagerImpl的静态方法new一个WindowManagerImpl对象
        mWindowManager = ((WindowManagerImpl)wm).createLocalWindowManager(this);
    }
    public android.view.WindowManager getWindowManager() {
        return mWindowManager;
    }
}
public class PhoneWindow extends Window implements MenuBuilder.Callback {
    private DecorView mDecor;
}

/**WindowManager系列，用于管理窗口视图的显示、添加、移除和状态更新等*/
public interface ViewManager{
    public void addView(View view, ViewGroup.LayoutParams params);
    public void updateViewLayout(View view, ViewGroup.LayoutParams params);
    public void removeView(View view);
}
public interface WindowManager extends ViewManager {
    ...
}
public final class WindowManagerImpl implements WindowManager {
    //每个WindowManagerImpl对象都保存了一个WindowManagerGlobal的单例
    private final WindowManagerGlobal mGlobal = WindowManagerGlobal.getInstance();
    ...
    public WindowManagerImpl createLocalWindowManager(android.view.Window parentWindow) {
        return new WindowManagerImpl(mDisplay, parentWindow);
    }

    @Override
    public void addView(@NonNull View view, @NonNull ViewGroup.LayoutParams params) {
        applyDefaultToken(params);
        //调用WindowManagerGlobal对象的addView()方法
        mGlobal.addView(view, params, mDisplay, mParentWindow);
    }
    ...
}
public final class WindowManagerGlobal {
    private static WindowManagerGlobal sDefaultWindowManager;
    private static IWindowManager sWindowManagerService;
    private static IWindowSession sWindowSession;
    //单例
    private WindowManagerGlobal() {
    }
    public static WindowManagerGlobal getInstance() {
        synchronized (WindowManagerGlobal.class) {
            if (sDefaultWindowManager == null) {
                sDefaultWindowManager = new WindowManagerGlobal();
            }
            return sDefaultWindowManager;
        }
    }
    /**WindowManagerGlobal类被加载时，就获取到WindowManagerService对象*/
    public static void initialize() {
        getWindowManagerService();
    }

    public static IWindowManager getWindowManagerService() {
        //线程安全的
        synchronized (WindowManagerGlobal.class) {
            if (sWindowManagerService == null) {
                sWindowManagerService = IWindowManager.Stub.asInterface(
                        ServiceManager.getService("window"));
            }
            return sWindowManagerService;
        }
    }
    public static IWindowSession getWindowSession() {
        //线程安全的
        synchronized (WindowManagerGlobal.class) {
            if (sWindowSession == null) {
                try {
                    InputMethodManager imm = InputMethodManager.getInstance();
                    IWindowManager windowManager = getWindowManagerService();
                    sWindowSession = windowManager.openSession(
                            new IWindowSessionCallback.Stub() {
                                @Override
                                public void onAnimatorScaleChanged(float scale) {
                                    ValueAnimator.setDurationScale(scale);
                                }
                            },
                            imm.getClient(), imm.getInputContext());
                } catch (RemoteException e) {
                    Log.e(TAG, "Failed to open window session", e);
                }
            }
            return sWindowSession;
        }
    }
}
/**Activity*/
public class Activity ...{
    private Window mWindow;              //mWindow是PhoneWindow类型
    private WindowManager mWindowManager;//mWindowManager是WindowManagerImpl类型
    /* Activity被创建后，执行attach()初始化的时候，就会创建PhoneWindow对象和WindowManagerImpl对象 */
    final void attach(...) {
        ...
        //①.为activity创建一个PhoneWindow对象
        mWindow = new PhoneWindow(this);
        mWindow.setWindowManager(
                (WindowManager)context.getSystemService(Context.WINDOW_SERVICE),
                mToken, mComponent.flattenToString(),
                (info.flags & ActivityInfo.FLAG_HARDWARE_ACCELERATED) != 0);
        if (mParent != null) {
            mWindow.setContainer(mParent.getWindow());
        }
        //②.调用Window的getWindowManager()方法初始化mWindowManager，其实就是new了一个WindowManagerImpl对象
        mWindowManager = mWindow.getWindowManager();
    }
}
```

### PhoneWindow：
在上一篇文章中就讲过，这其实理解起来也比较容易，我们知道activity就是用于展示界面，控制用户交互的，所以activity中一定维护了用于**描述界面的对象**，还有就是**控制窗口显示的管理器对象**，实际上这两个对象都是**由PhoneWindow维护**的，而每个activity都拥有一个PhoneWindow对象**mWindow**；

### WindowManager：
用于**管理窗口的一些状态、属性以及窗口的添加、删除、更新、顺序等**，WindowManager是ViewManager的子接口，ViewManager接口中只有三个抽象方法（**addView、updateViewLayout、removeView**），可显而知，这个管理器的作用就是控制窗口View的添加、删除以及状态更新的。WindowManager也是一个接口，他并没有实现ViewManager中的抽象方法，其实真正的**实现类**是**WindowManagerImpl**。
在**activity**被**创建**后调用**attach**()方法初始化的时候，首先**创建**了PhoneWindow对象**mWindow**，然后调用mWindow的**setWindowManager**((WindowManager)context.getSystemService(Context.WINDOW_SERVICE) ...)为其**mWindowManager赋值**。

### WindowManagerGlobal：
由于WindowManagerGlobal的关键代码比较长，稍后讲解的时候再贴代码。WindowManagerGlobal是单例模式，Activity在创建完成调用attach()方法初始化时，会实例化mWindowManager（WindowManager的引用指向WindowManagerImpl 的对象），**WindowManagerImpl创建之后**，就**维护**了**WindowManagerGlobal**的**单例对象**，WindowManagerGlobal中有一个IWindowSession类型的变量**sWindowSession**，他就是用来**和远程服务WindowManagerService通信**的代理。

### WindowManagerService：
跟之前讲解Activity启动过程中提到的ActivityManagerService(用于管理Android组件的启动、关闭和状态信息等)一样，WMS也是一个复杂的系统服务。 
  在了解WMS之前，首先要了解窗口（**Window**）是什么，Android系统中的窗口是只屏幕上一块**用于绘制UI元素并能相应用户交互的矩形区域**，从原理上讲，窗口是独自**占有一个Surface实例的显示区域**，例如Dialog、Activity界面、比值、状态栏、Toast等都是窗口。
  **Surface**就是一块**画布**，应用可以通过Cancas或者OpenGL在其上面作画，然后通过urfaceFlinger将多块Surface的内容按照特定的顺序进行混合并输出到FrameBuffer，从而将应用界面显示给用户。 
  既然每个窗口都有一块Surface供自己涂鸦，必然需要一个角色**对所有窗口的Surface进行协调管理**，**WMS**就是用来做这个事情的，WMS掌管Surface的显示顺序（Z-order）以及位置尺寸，控制窗口动画，并且还是输入系统的重要中转站。

通过上面的分析，我们知道这几个关键类的大概作用，以及他们之间的嵌套关系，接下来，我们就开始分析Activity的绘制过程：

## step1：ActivityThread.handleResumeActivity()

首先接着**《一、Activity的创建过程》** **step20**的`handleResumeActivity()`方法分析：

```java
//获取activity的顶层视图decor
View decor = r.window.getDecorView();
//让decor显示出来
decor.setVisibility(View.INVISIBLE);
ViewManager wm = a.getWindowManager();
WindowManager.LayoutParams l = r.window.getAttributes();
a.mDecor = decor;
l.type = WindowManager.LayoutParams.TYPE_BASE_APPLICATION;
l.softInputMode |= forwardBit;
if (a.mVisibleFromClient) {
    a.mWindowAdded = true;
    //将decor放入WindowManager(WindowManagerImpl对象)中，这样activity就显示出来了
    wm.addView(decor, l);
```

`wm.addView(decor, l)`实际上就是调用`WindowManagerImpl`的`addView()`方法将Activity的根窗口**mDecor添加到窗口管理器中**的，而WindowManagerImpl的addView()方法又调用的是`WindowManagerGlobal`单例的`addView()`方法。
接下来我们就分析`WindowManagerGlobal.addView()`：

## step2：WindowManagerGlobal.addView()

```java
public void addView(View view, ViewGroup.LayoutParams params,
                        Display display, Window parentWindow) {
        ...

        ViewRootImpl root;
        View panelParentView = null;

        synchronized (mLock) {
            ...
            int index = findViewLocked(view, false);
            if (index >= 0) {
                if (mDyingViews.contains(view)) {
                    // Don't wait for MSG_DIE to make it's way through root's queue.
                    mRoots.get(index).doDie();
                } else {
                    throw new IllegalStateException("View " + view
                            + " has already been added to the window manager.");
                }
                // The previous removeView() had not completed executing. Now it has.
            }
            ...

            //创建一个视图层次结构的顶部，ViewRootImpl实现所需视图和窗口之间的协议。
            root = new ViewRootImpl(view.getContext(), display);

            view.setLayoutParams(wparams);

            mViews.add(view);
            mRoots.add(root);
            mParams.add(wparams);
        }
        try {
            //将activity的根窗口mDecor添加到root中
            root.setView(view, wparams, panelParentView);
        } catch (RuntimeException e) {
            ...
            throw e;
        }
    }
}
```

在这个方法中，首先创建了一个`ViewRootImpl`的对象**root**，然后调用`root.setView()`，将activity的根窗口视图设置给root，下面我们看看`ViewRootImpl`类：

## step3：ViewRootImpl.setView()

```java
public final class ViewRootImpl implements ViewParent,
        View.AttachInfo.Callbacks, HardwareRenderer.HardwareDrawCallbacks {

    final IWindowSession mWindowSession;

    public ViewRootImpl(Context context, Display display) {
        mWindowSession = WindowManagerGlobal.getWindowSession();
        ...
    }

    public void setView(View view, WindowManager.LayoutParams attrs, View panelParentView) {
        synchronized (this) {
            if (mView == null) {
                mView = view;
                ...

                int res; /* = WindowManagerImpl.ADD_OKAY; */
                //请求对应用程序窗口视图的UI作第一次布局,应用程序窗口的绘图表面的创建过程
                requestLayout();
                ...
                try {
                    ...
                    res = mWindowSession.addToDisplay(mWindow, mSeq, mWindowAttributes,
                            getHostVisibility(), mDisplay.getDisplayId(),
                            mAttachInfo.mContentInsets, mAttachInfo.mStableInsets,
                            mAttachInfo.mOutsets, mInputChannel);
                } catch (RemoteException e) {
                    ...
                    throw new RuntimeException("Adding window failed", e);
                }
                ...

            }
        }

    @Override
    public void requestLayout () {
        if (!mHandlingLayoutInLayoutRequest) {
            //检查当前线程是否是主线程，对视图的操作只能在UI线程中进行，否则抛异常
            checkThread();
            //应用程序进程的UI线程正在被请求执行一个UI布局操作
            mLayoutRequested = true;
            scheduleTraversals();
        }
    }

    void checkThread() {
        if (mThread != Thread.currentThread()) {
            throw new CalledFromWrongThreadException(
                    "Only the original thread that created a view hierarchy can touch its views.");
        }
    }
    void scheduleTraversals() {
        if (!mTraversalScheduled) {
            mTraversalScheduled = true;
            mTraversalBarrier = mHandler.getLooper().getQueue().postSyncBarrier();
            //Choreographer类型，用于发送消息
            mChoreographer.postCallback(
                    Choreographer.CALLBACK_TRAVERSAL, mTraversalRunnable, null);
            ...
        }
    }

    final TraversalRunnable mTraversalRunnable = new TraversalRunnable();
    final class TraversalRunnable implements Runnable {
        @Override
        public void run() {
            doTraversal();
        }
    }
    void doTraversal() {
        if (mTraversalScheduled) {
            mTraversalScheduled = false;
            mHandler.getLooper().getQueue().removeSyncBarrier(mTraversalBarrier);
            if (mProfile) {
                Debug.startMethodTracing("ViewAncestor");
            }
            performTraversals();
            if (mProfile) {
                Debug.stopMethodTracing();
                mProfile = false;
            }
        }
    }
}
```

  ViewRootImpl实现了ViewParent，它并不是真正的View（没有继承View），它定义了顶层视图的职责API，比如requestLayout()请求布局等等，在setView()方法中，调用了requestLayout()方法，请求对Activity根窗口做第一次布局。 
  requestLayout()方法中首先调用checkThread()检查当前线程是否为UI线程，如果不是则抛出异常，然后调用scheduleTraversals()方法，scheduleTraversals()方法中调用了mChoreographer.postCallback()，mChoreographer中维护了一个Handler，通过handler机制实现消息调度，并传入一个回调mTracersalRunnable。mTracersalRunnable的run方法中调用了doTraversal()方法，doTraversal()方法继续调用performTraversals():


## step4：ViewRootImpl.performTraversals()

```java
private void performTraversals() {
    final View host = mView;   //mView就是Activity的根窗口mDecor
    ...
    //下面代码主要确定Activity窗口的大小
    boolean windowSizeMayChange = false;
    //Activity窗口的宽度desiredWindowWidth和高度desiredWindowHeight
    int desiredWindowWidth;
    int desiredWindowHeight;
    ...
    //Activity窗口当前的宽度和高度是保存ViewRoot类的成员变量mWinFrame中的
    Rect frame = mWinFrame;
    ...
    /* 接下来的两段代码都是在满足下面的条件之一的情况下执行的：
     * 1. Activity窗口是第一次执行测量、布局和绘制操作，即ViewRoot类的成员变量mFirst的值等于true
     * 2. 前面得到的变量windowShouldResize的值等于true，即Activity窗口的大小的确是发生了变化。
     * 3. 前面得到的变量insetsChanged的值等于true，即Activity窗口的内容区域边衬发生了变化。
     * 4. Activity窗口的可见性发生了变化，即变量viewVisibilityChanged的值等于true。
     * 5. Activity窗口的属性发生了变化，即变量params指向了一个WindowManager.LayoutParams对象。
     */
    if (mFirst || windowShouldResize || insetsChanged ||
            viewVisibilityChanged || params != null) {
        ...
        try {

            //★请求WindowManagerService服务计算Activity窗口的大小以及内容区域边衬大小和可见区域边衬大小
            relayoutResult = relayoutWindow(params, viewVisibility, insetsPending);
            ...
        } catch (RemoteException e) {
        }

        ...
        //将计算得到的Activity窗口的宽度和高度保存在ViewRoot类的成员变量mWidth和mHeight中
        if (mWidth != frame.width() || mHeight != frame.height()) {
            mWidth = frame.width();
            mHeight = frame.height();
        }
        ...

        if (!mStopped || mReportNextDraw) {
            boolean focusChangedDueToTouchMode = ensureTouchModeLocally(
                    (relayoutResult&WindowManagerGlobal.RELAYOUT_RES_IN_TOUCH_MODE) != 0);
            if (focusChangedDueToTouchMode || mWidth != host.getMeasuredWidth()
                    || mHeight != host.getMeasuredHeight() || contentInsetsChanged) {
                int childWidthMeasureSpec = getRootMeasureSpec(mWidth, lp.width);
                int childHeightMeasureSpec = getRootMeasureSpec(mHeight, lp.height);

                // 一、测量控件大小（根窗口和其子控件树）
                performMeasure(childWidthMeasureSpec, childHeightMeasureSpec);
                int width = host.getMeasuredWidth();
                int height = host.getMeasuredHeight();
                boolean measureAgain = false;
                if (lp.horizontalWeight > 0.0f) {
                    width += (int) ((mWidth - width) * lp.horizontalWeight);
                    childWidthMeasureSpec = View.MeasureSpec.makeMeasureSpec(width,
                            View.MeasureSpec.EXACTLY);
                    measureAgain = true;
                }
                if (lp.verticalWeight > 0.0f) {
                    height += (int) ((mHeight - height) * lp.verticalWeight);
                    childHeightMeasureSpec = View.MeasureSpec.makeMeasureSpec(height,
                            View.MeasureSpec.EXACTLY);
                    measureAgain = true;
                }
                if (measureAgain) {
                    // 一、测量控件大小（根窗口和其子控件树）
                    performMeasure(childWidthMeasureSpec, childHeightMeasureSpec);
                }

                layoutRequested = true;
            }
        }
    }else{
        ...
    }

    final boolean didLayout = layoutRequested && (!mStopped || mReportNextDraw);
    boolean triggerGlobalLayoutListener = didLayout
            || mAttachInfo.mRecomputeGlobalAttributes;
    if (didLayout) {
        //二、布局过程
        performLayout(lp, desiredWindowWidth, desiredWindowHeight);
        ...
    }
    ...
    boolean skipDraw = false;
    boolean cancelDraw = mAttachInfo.mTreeObserver.dispatchOnPreDraw() ||
            viewVisibility != View.VISIBLE;

    if (!cancelDraw && !newSurface) {
        if (!skipDraw || mReportNextDraw) {
            ...
            //三、绘制过程
            performDraw();
        }
    } else {
        if (viewVisibility == View.VISIBLE) {
            // Try again
            scheduleTraversals();
        } else if (mPendingTransitions != null && mPendingTransitions.size() > 0) {
            ...
        }
    }
    mIsInTraversal = false;
}
```

  `performTraversals()`方法中，分为四部分：
  第一步：上面很大一段代码主要作用是确定Activity窗口的大小，也就是通过各种判断，是否是第一次？是否需要重新计算大小？最终确定`Activity`根窗口的大小并保存起来。
  第二步：就是遍历测量子控件的大小；
  第三步：就是布局；
  第四部：就是绘制。
于Activity窗口大小的计算，这里就不深入探讨，有兴趣可以看看。接下来我们重点分析后面三个步骤，也就是控件的测量、布局、绘制三个过程。

## step5: ViewRootImpl.performMeasure()控件测量过程

```java
private void performMeasure(int childWidthMeasureSpec, int childHeightMeasureSpec) {
    Trace.traceBegin(Trace.TRACE_TAG_VIEW, "measure");
    try {
        mView.measure(childWidthMeasureSpec, childHeightMeasureSpec);
    } finally {
        Trace.traceEnd(Trace.TRACE_TAG_VIEW);
    }
}
```

  `performMeasure()`方法中调用了`mView.measure()`方法，`mView`就是`Activity`的根窗口，他是`DecorView`类型（`FrameLayout`的子类），这个类定义在`PhoneWindow`类中，接下来我们看看`DecorView`的`measure()`方法（`measure()`方法是View中final修饰的方法，不能覆盖）：

## step6: View.measure()

```java
public final void measure(int widthMeasureSpec, int heightMeasureSpec) {
    ...
//如果View的mPrivateFlags的PFLAG_FORCE_LAYOUT位不等于0时，就表示当前视图正在请求执行一次布局操作；
//或者当前的宽高约束条件不等于视图上一次的约束条件时，需要重新测量View的大小。
    if ((mPrivateFlags & PFLAG_FORCE_LAYOUT) == PFLAG_FORCE_LAYOUT ||
            widthMeasureSpec != mOldWidthMeasureSpec ||
            heightMeasureSpec != mOldHeightMeasureSpec) {

        //首先清除所测量的维度标志
        mPrivateFlags &= ~PFLAG_MEASURED_DIMENSION_SET;

        ...

        int cacheIndex = (mPrivateFlags & PFLAG_FORCE_LAYOUT) == PFLAG_FORCE_LAYOUT ? -1 :
                mMeasureCache.indexOfKey(key);
        //如果需要强制布局操作，或者忽略测量历史，将会调用onMeasure对控件进行一次测量
        //sIgnoreMeasureCache是一个boolean值，初始化为sIgnoreMeasureCache = targetSdkVersion < KITKAT;
        //意思是如果Android版本低于19，每次都会调用onMeasure()，而19以上时sIgnoreMeasureCache==false
        if (cacheIndex < 0 || sIgnoreMeasureCache) {
            //调用onMeasure来真正执行测量宽度和高度的操作
            onMeasure(widthMeasureSpec, heightMeasureSpec);
            mPrivateFlags3 &= ~PFLAG3_MEASURE_NEEDED_BEFORE_LAYOUT;
        } else {
            ...
        }
        // flag not set, setMeasuredDimension() was not invoked, we raise
        // an exception to warn the developer
        if ((mPrivateFlags & PFLAG_MEASURED_DIMENSION_SET) != PFLAG_MEASURED_DIMENSION_SET) {
            throw new IllegalStateException("View with id " + getId() + ": "
                    + getClass().getName() + "#onMeasure() did not set the"
                    + " measured dimension by calling"
                    + " setMeasuredDimension()");
        }

        mPrivateFlags |= PFLAG_LAYOUT_REQUIRED;
    }

    //保存这次测量的宽高约束
    mOldWidthMeasureSpec = widthMeasureSpec;
    mOldHeightMeasureSpec = heightMeasureSpec;

    mMeasureCache.put(key, ((long) mMeasuredWidth) << 32 |
            (long) mMeasuredHeight & 0xffffffffL); // suppress sign extension
}
```
### measure() VS onMeasure()
  View中有两个关于测量的方法measure()和onMeasure()。
  **measure**()方法是final的，**不能被重写**。measure()方法中**调用**了**onMeasure**()方法。
  **onMeasure**()方法在View中的默认实现是**测量自身**的**大小**，所以在我们自定义ViewGroup的时候，如果我们不重写onMeasure()测量子控件的大小，子控件将会显示不出来的。ViewGroup中没有重写onMeasure()方法，在ViewGroup的子类（LinearLayout、RelativeLayout等）中会根据容器自身的布局特性，比如LinearLayout有两种布局方式，水平或者垂直，分别对onMeasure()方法进行不同的重写。

  总结一下就是measure()方法不能被重写，它会调用onMeasure()方法测量控件自身的大小，如果控件是一个容器，它需要重写onMeasure()方法遍历测量所有子控件的大小。还有当我们继承View**自定义控件时**，在布局文件中明明使用了wrap_content可结果还是填充父窗体，这是因为View的onMeasure()方法的默认实现，如果**要按照我们的需求正确的测量控件大小也需要重写onMeasure()方法**。

  由于View.measure()中调用了onMeasure()方法，所以就调用到了DecorView的onMeasure ()方法，DecorView.onMeasure()中首先对宽高约束做了一些处理后，接着调用了super.onMeasure()，也就是FrameLayout中的onMeasure()方法：


## step7: FrameLayout.onMeasure()

```java
@Override
protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
    //获得一个视图容器所包含的子视图的个数
    int count = getChildCount();

    final boolean measureMatchParentChildren =
            View.MeasureSpec.getMode(widthMeasureSpec) != View.MeasureSpec.EXACTLY ||
                    View.MeasureSpec.getMode(heightMeasureSpec) != View.MeasureSpec.EXACTLY;
    //mMatchParentChildren用于缓存子控件中布局参数设置为填充父窗体的控件
    mMatchParentChildren.clear();

    int maxHeight = 0;
    int maxWidth = 0;
    int childState = 0;

    //①、遍历根窗口下所有的子控件，挨个测量大小
    for (int i = 0; i < count; i++) {
        final View child = getChildAt(i);
        if (mMeasureAllChildren || child.getVisibility() != GONE) {
            //调用measureChildWithMargins来测量每一个子视图的宽度和高度
            measureChildWithMargins(child, widthMeasureSpec, 0, heightMeasureSpec, 0);
            final LayoutParams lp = (LayoutParams) child.getLayoutParams();
            //由于FrameLayout的布局特性（Z轴帧布局，向上覆盖），将子控件中，最大的宽高值记录下来
            maxWidth = Math.max(maxWidth,
                    child.getMeasuredWidth() + lp.leftMargin + lp.rightMargin);
            maxHeight = Math.max(maxHeight,
                    child.getMeasuredHeight() + lp.topMargin + lp.bottomMargin);
            childState = combineMeasuredStates(childState, child.getMeasuredState());
            if (measureMatchParentChildren) {
                //由于现在本容器的宽高都还没有确定下来，子控件设置为填充父窗体肯定没法计算，所以先缓存起来
                if (lp.width == LayoutParams.MATCH_PARENT ||
                        lp.height == LayoutParams.MATCH_PARENT) {
                    mMatchParentChildren.add(child);
                }
            }
        }
    }

    //②、设置FrameLayout的宽高
    //上面已经得到子控件中宽高的最大值，然后加上容器（FrameLayout）设置的padding值
    // Account for padding too
    maxWidth += getPaddingLeftWithForeground() + getPaddingRightWithForeground();
    maxHeight += getPaddingTopWithForeground() + getPaddingBottomWithForeground();

    //判断是否设置有最小宽度和高度，如果有设置，需要和上面计算的值比较，选择较大的值作为容器宽高
    // Check against our minimum height and width
    maxHeight = Math.max(maxHeight, getSuggestedMinimumHeight());
    maxWidth = Math.max(maxWidth, getSuggestedMinimumWidth());

    //是否设置有前景图，如果有，需要考虑背景的宽高
    // Check against our foreground's minimum height and width
    final Drawable drawable = getForeground();
    if (drawable != null) {
        maxHeight = Math.max(maxHeight, drawable.getMinimumHeight());
        maxWidth = Math.max(maxWidth, drawable.getMinimumWidth());
    }
    //调用resolveSizeAndState()方法根据计算出的宽高值和宽高约束参数，计算出正确的宽高值；
    //然后调用setMeasuredDimension()方法设置当前容器的宽高值
    setMeasuredDimension(resolveSizeAndState(maxWidth, widthMeasureSpec, childState),
            resolveSizeAndState(maxHeight, heightMeasureSpec,
                    childState << MEASURED_HEIGHT_STATE_SHIFT));

    //③、计算子控件中设置为填充父窗体的控件的大小
    count = mMatchParentChildren.size();
    if (count > 1) {
        for (int i = 0; i < count; i++) {
            final View child = mMatchParentChildren.get(i);
            final ViewGroup.MarginLayoutParams lp = (ViewGroup.MarginLayoutParams) child.getLayoutParams();

            final int childWidthMeasureSpec;
            if (lp.width == LayoutParams.MATCH_PARENT) {
                final int width = Math.max(0, getMeasuredWidth()
                        - getPaddingLeftWithForeground() - getPaddingRightWithForeground()
                        - lp.leftMargin - lp.rightMargin);
                childWidthMeasureSpec = View.MeasureSpec.makeMeasureSpec(
                        width, View.MeasureSpec.EXACTLY);
            } else {
                childWidthMeasureSpec = getChildMeasureSpec(widthMeasureSpec,
                        getPaddingLeftWithForeground() + getPaddingRightWithForeground() +
                                lp.leftMargin + lp.rightMargin,
                        lp.width);
            }

            final int childHeightMeasureSpec;
            if (lp.height == LayoutParams.MATCH_PARENT) {
                final int height = Math.max(0, getMeasuredHeight()
                        - getPaddingTopWithForeground() - getPaddingBottomWithForeground()
                        - lp.topMargin - lp.bottomMargin);
                childHeightMeasureSpec = View.MeasureSpec.makeMeasureSpec(
                        height, View.MeasureSpec.EXACTLY);
            } else {
                childHeightMeasureSpec = getChildMeasureSpec(heightMeasureSpec,
                        getPaddingTopWithForeground() + getPaddingBottomWithForeground() +
                                lp.topMargin + lp.bottomMargin,
                        lp.height);
            }
            child.measure(childWidthMeasureSpec, childHeightMeasureSpec);
        }
    }
}
```

代码中注释已经写的比较清楚了，简单描述一遍，`FrameLayout`的`onMeasure`方法可以分为三个步骤： 
①. 遍历所有子控件，并调用`measureChildWithMargin()`方法测量子控件的大小，`measureChildWithMargin()` 方法是从`ViewGroup`中继承下来的，它会调用`child.measure()`，如果子控件又是一个容器就会继续遍历child的子控件测量，最后`FrameLayout`的所有子控件（包括子容器中的子控件…）都完成了测量； 
②. 根据第①步中记录的子控件宽高的最大值，然后加上`padding`值以及最小宽高的比较，最终确定`FrameLayout`容器的宽高； 
③. 重新计算子控件宽高设置为`MATCH_PARENT`（填充父窗体 ）的宽高

  `FrameLayout`的`onMeasure()`方法执行完毕后，整个`Activity`窗体重所有的控件都完成了测量，这时调用`view.getMeasuredWidth()/getMeasuredHeight()`就可以获取到控件测量的宽高值了。测量的部分到此就结束了，如果想进一步深入了解控件测量的相关知识可以参考 [Android自定义View（深入解析控件测量onMeasure）](http://blog.csdn.net/moira33/article/details/79017920)。下面我们接着**step4**中的第二步**performLayout()**分析窗口的布局过程。

## step8：ViewRootImpl.performLayout()请求布局过程

```java
private void performLayout(WindowManager.LayoutParams lp, int desiredWindowWidth,
                           int desiredWindowHeight) {
    mLayoutRequested = false;
    mScrollMayChange = true;
    mInLayout = true;
    final View host = mView;
    ...

    Trace.traceBegin(Trace.TRACE_TAG_VIEW, "layout");
    try {
        host.layout(0, 0, host.getMeasuredWidth(), host.getMeasuredHeight());
        ...
    } finally {
        Trace.traceEnd(Trace.TRACE_TAG_VIEW);
    }
    mInLayout = false;
 }
```

  `ViewRootImpl.performLayout()`方法中会调用`mView`（`Activity`根窗口`mDecor`）的`layout()`方法，为窗口中所有的子控件安排显示的位置，由于不同的容器有不同的布局策略，所以在布局之前首先要确定所有子控件的大小，才能适当的为子控件安排位置，这就是为什么测量过程需要在布局过程之前完成。接着我们看看`DecorView`的`layout()`方法（layout方法继承自`View`）：

## step9：View.layout()

```java
public void layout(int l, int t, int r, int b) {
    if ((mPrivateFlags3 & PFLAG3_MEASURE_NEEDED_BEFORE_LAYOUT) != 0) {
        onMeasure(mOldWidthMeasureSpec, mOldHeightMeasureSpec);
        mPrivateFlags3 &= ~PFLAG3_MEASURE_NEEDED_BEFORE_LAYOUT;
    }

    //记录上一次布局后的左上右下的坐标值
    int oldL = mLeft;
    int oldT = mTop;
    int oldB = mBottom;
    int oldR = mRight;

    //为控件重新设置新的坐标值，并判断是否需要重新布局
    boolean changed = isLayoutModeOptical(mParent) ?
            setOpticalFrame(l, t, r, b) : setFrame(l, t, r, b);

    if (changed || (mPrivateFlags & PFLAG_LAYOUT_REQUIRED) == PFLAG_LAYOUT_REQUIRED) {
        //onLayout()方法在View中是一个空实现，各种容器需要重写onLayout()方法，为子控件布局
        onLayout(changed, l, t, r, b);
        mPrivateFlags &= ~PFLAG_LAYOUT_REQUIRED;

        ListenerInfo li = mListenerInfo;
        if (li != null && li.mOnLayoutChangeListeners != null) {
            ArrayList<OnLayoutChangeListener> listenersCopy =
                    (ArrayList<OnLayoutChangeListener>)li.mOnLayoutChangeListeners.clone();
            int numListeners = listenersCopy.size();
            for (int i = 0; i < numListeners; ++i) {
                listenersCopy.get(i).onLayoutChange(this, l, t, r, b, oldL, oldT, oldR, oldB);
            }
        }
    }
    mPrivateFlags &= ~PFLAG_FORCE_LAYOUT;
    mPrivateFlags3 |= PFLAG3_IS_LAID_OUT;
}
```

  从上面的代码中，我们注意到关于布局有两个重要的方法，View.layout()和View.onLayout()，这两个方法有什么关系？各自的作用是什么呢？他们都是定义在View中的，不同的是layout()方法中有很长一段实现的代码，而onLayout()确实一个空的实现，里面什么事也没做。 
### layout() VS onLayout()
  首先我们要明确布局的本质是什么，布局就是为View设置四个坐标值，这四个坐标值保存在View的成员变量mLeft、mTop、mRight、mBottom中，方便View在绘制（onDraw）的时候知道应该在那个区域内绘制控件。而我们看到layout()方法中实际上就是为这几个成员变量赋值的，所以到底**真正设置坐标的是layout()方法**，那onLayout()的作用是什么呢？ 
  **onLayout**()都是由**ViewGroup的子类实现**的，他的作用就是确定容器中**每个子控件的位置**，由于不同的容器有不容的布局策略，所以每个容器对onLayout()方法的实现都不同，onLayout()方法会遍历容器中所有的子控件，然后计算他们左上右下的坐标值，最后调用child.layout()方法为子控件设置坐标；由于**layout()方法中又调用了onLayout()方法**，如果子控件child也是一个容器，就会继续为它的子控件计算坐标，如果child不是容器，onLayout()方法将什么也不做，这样下来，只要Activity根窗口mDecor的layout()方法执行完毕，窗口中所有的子容器、子控件都将完成布局操作。

  其实布局过程的调用方式和测量过程是一样的，ViewGroup的子类都要重写onMeasure()方法遍历子控件调用他们的measure()方法，measure()方法又会调用onMeasure()方法，如果子控件是普通控件就完成了测量，如果是容器将会继续遍历其孙子控件。


继续查看`DecorView.onLayout()`方法：

## step10：DecorView .onLayout()

```java
@Override
protected void onLayout(boolean changed, int left, int top, int right, int bottom) {
    super.onLayout(changed, left, top, right, bottom);
    getOutsets(mOutsets);
    if (mOutsets.left > 0) {
        offsetLeftAndRight(-mOutsets.left);
    }
    if (mOutsets.top > 0) {
        offsetTopAndBottom(-mOutsets.top);
    }
}
```

  `DecorView`是`FrameLayout`的子类，`FrameLayout`又是`ViewGroup`的子类，`FrameLayout`重写了`onLayout()`方法，`DecorView`也重写了`onLayout()`方法，但是调用的是`super.onLayout()`，然后做了一些边界判断，下面我们看`FrameLayout.onLayout()`：

## step11：FrameLayout .onLayout()

```java
@Override
protected void onLayout(boolean changed, int left, int top, int right, int bottom) {
    layoutChildren(left, top, right, bottom, false /* no force left gravity */);
}

void layoutChildren(int left, int top, int right, int bottom,
                    boolean forceLeftGravity) {
    //获取子控件数量
    final int count = getChildCount();

    //获取padding值
    final int parentLeft = getPaddingLeftWithForeground();
    final int parentRight = right - left - getPaddingRightWithForeground();

    final int parentTop = getPaddingTopWithForeground();
    final int parentBottom = bottom - top - getPaddingBottomWithForeground();
    //遍历子控件，为其计算左上右下坐标，由于不同容器的布局特性，下面的计算过程都是根据容器的布局特性计算的
    for (int i = 0; i < count; i++) {
        final View child = getChildAt(i);
        if (child.getVisibility() != GONE) {
            final LayoutParams lp = (LayoutParams) child.getLayoutParams();

            final int width = child.getMeasuredWidth();
            final int height = child.getMeasuredHeight();

            int childLeft;
            int childTop;

            int gravity = lp.gravity;
            if (gravity == -1) {
                gravity = DEFAULT_CHILD_GRAVITY;
            }

            final int layoutDirection = getLayoutDirection();
            final int absoluteGravity = Gravity.getAbsoluteGravity(gravity, layoutDirection);
            final int verticalGravity = gravity & Gravity.VERTICAL_GRAVITY_MASK;

            switch (absoluteGravity & Gravity.HORIZONTAL_GRAVITY_MASK) {
                case Gravity.CENTER_HORIZONTAL:
                    childLeft = parentLeft + (parentRight - parentLeft - width) / 2 +
                            lp.leftMargin - lp.rightMargin;
                    break;
                case Gravity.RIGHT:
                    if (!forceLeftGravity) {
                        childLeft = parentRight - width - lp.rightMargin;
                        break;
                    }
                case Gravity.LEFT:
                default:
                    childLeft = parentLeft + lp.leftMargin;
            }

            switch (verticalGravity) {
                case Gravity.TOP:
                    childTop = parentTop + lp.topMargin;
                    break;
                case Gravity.CENTER_VERTICAL:
                    childTop = parentTop + (parentBottom - parentTop - height) / 2 +
                            lp.topMargin - lp.bottomMargin;
                    break;
                case Gravity.BOTTOM:
                    childTop = parentBottom - height - lp.bottomMargin;
                    break;
                default:
                    childTop = parentTop + lp.topMargin;
            }
            //调用其layout()方法为子控件设置坐标
            child.layout(childLeft, childTop, childLeft + width, childTop + height);
        }
    }
}
```

  所有的布局容器的`onLayout`方法都是一样的流程，都是先遍历子控件，然后计算子控件的坐标，最后调用子控件的`layout()`方法设置布局坐标，但是不同的布局容器有不同的布局策略，所以区别就在于计算子控件坐标时的差异。比如`LinearLayout`线性布局，如果是水平布局，第一个子控件的l值是0，r是100，那第二个子控件的l就是101（只是打个比方），而`FrameLayout`，如果没有设置`padding`，子控件也没设置margin，第一个子控件的l值就是0，第二个子控件的l还是0，这就是不同容器的计算区别。

  `FrameLayout.onLayout()`方法执行完毕后，整个`Activity`的根窗口的布局过程也就完成了。接下来进入第三个过程–绘制过程：

## step12：ViewRootImpl.performDraw()控件绘制过程

```java
private void performDraw() {
    ...
    mIsDrawing = true;
    Trace.traceBegin(Trace.TRACE_TAG_VIEW, "draw");
    try {
        draw(fullRedrawNeeded);
    } finally {
        mIsDrawing = false;
        Trace.traceEnd(Trace.TRACE_TAG_VIEW);
    }
    ...
}
private void draw(boolean fullRedrawNeeded) {
    Surface surface = mSurface;

    final Rect dirty = mDirty;
    ...
    if (!dirty.isEmpty() || mIsAnimating || accessibilityFocusDirty) {
        if (mAttachInfo.mHardwareRenderer != null && mAttachInfo.mHardwareRenderer.isEnabled()) {
            ...
            //使用硬件渲染
            mAttachInfo.mHardwareRenderer.draw(mView, mAttachInfo, this);
        } else {
            ...
            // 通过软件渲染.
            if (!drawSoftware(surface, mAttachInfo, xOffset, yOffset, scalingRequired, dirty)) {
                return;
            }
        }
    }
    ...
}
private boolean drawSoftware(Surface surface, AttachInfo attachInfo, int xoff, int yoff,
                             boolean scalingRequired, Rect dirty) {
    final Canvas canvas;

    try {
        ...
        canvas = mSurface.lockCanvas(dirty);
        ...
    } catch (Surface.OutOfResourcesException e) {
        return false;
    } catch (IllegalArgumentException e) {
        return false;
    }
    ...
    try {
        ...
        try {
            ...
            mView.draw(canvas);
            ...
        } finally {
                ...
        }
    } finally {
       ...
    }
    return true;
}
```

  `ViewRootImpl`的`performDraw()`方法调用`draw(boolean)`，在这个过程中主要完成一些条件判断，surface的设置准备，以及判断使用硬件渲染还是软件渲染等操作，由于我们主要研究绘制代码流程层面，所以直接看`drawSoftware()`方法，对于硬件渲染具体是怎样的有兴趣可以跟踪一下。`drawSoftware()`方法中，通过`mSurface.locakCanvas(dirty)`拿到画布，然后调用`mView.draw(canvas)`，这里的`mView`就是`Activity`的根窗口`DecorView`类型的对象。

## step13：DecorView.draw()

```java
public void draw(Canvas canvas) {
    super.draw(canvas);
    if (mMenuBackground != null) {
        mMenuBackground.draw(canvas);
    }
}
```

  `DecorView`重写了`View`的`draw()`方法，增加了绘制菜单背景的内容，因为`Activity`根窗口上会有一些菜单按钮（比如屏幕下方的返回键等），`draw()`方法中调用了`super.draw(cancas)`，所以我们看看`View`的`draw()`方法：

## step14：View.draw()

```java
public void draw(Canvas canvas) {
    ...
    /* 绘制遍历执行几个绘图步骤，必须以适当的顺序执行：
     * 1.绘制背景
     * 2.如果有必要，保存画布的图层，以准备失效
     * 3.绘制视图的内容
     * 4.绘制子控件
     * 5.如果必要，绘制衰落边缘和恢复层
     * 6.绘制装饰（比如滚动条）     */

    // Step 1, 绘制背景
    int saveCount;

    if (!dirtyOpaque) {
        drawBackground(canvas);
    }

    // 通常情况请跳过2和5步
    final int viewFlags = mViewFlags;
    boolean horizontalEdges = (viewFlags & FADING_EDGE_HORIZONTAL) != 0;
    boolean verticalEdges = (viewFlags & FADING_EDGE_VERTICAL) != 0;
    if (!verticalEdges && !horizontalEdges) {
        // Step 3, 绘制本控件的内容
        if (!dirtyOpaque) onDraw(canvas);

        // Step 4, 绘制子控件
        dispatchDraw(canvas);

        // Overlay is part of the content and draws beneath Foreground
        if (mOverlay != null && !mOverlay.isEmpty()) {
            mOverlay.getOverlayView().dispatchDraw(canvas);
        }

        // Step 6, draw decorations (foreground, scrollbars)
        onDrawForeground(canvas);

        // we're done...
        return;
    }
    //下面的代码是从第一步到第六步的完整流程
    ...
}
```

  `View`的`draw()`方法中有六个步骤，其中最为重要的就是第1步绘制背景、第2步绘制内容、第3步绘制子控件；绘制背景我们就不看了，我们看看绘制内容，调用的是`onDraw()`方法，`View`中的`onDraw()`方法是一个空的实现，可以想象，因为View是所有控件的超类，它并不知道他的孩子要怎样画自己，所以绘制的具体操作由孩子重写`onDraw()`方法自己绘制，所以`DecorView`重写了`onDraw()`方法来绘制`Activity`的跟窗口，具体怎么画的请跟踪`DecorView`的`onDraw()`方法:

## step15：DecorView.onDraw()

```java
@Override
public void onDraw(Canvas c) {
    super.onDraw(c);
    mBackgroundFallback.draw(mContentRoot, c, mContentParent);
}
```

  `mView`（根窗口）把自己的内容绘制完成之后，紧接着开始遍历绘制他的孩子，`View`中的`dispatchDraw()`也是一个空的实现，因为超类的后代是不一定有孩子的（比如`Button`,`TextView`等），`dispatchDraw()`是在`ViewGroup`中实现的，我们看看`ViewGroup.dispatchDraw()`:

## step16：ViewGroup.dispatchDraw()

```java
protected void dispatchDraw(Canvas canvas) {
    ...
    final int childrenCount = mChildrenCount;
    final View[] children = mChildren;

    if ((flags & FLAG_RUN_ANIMATION) != 0 && canAnimate()) {
        ...
        for (int i = 0; i < childrenCount; i++) {
            while (transientIndex >= 0 && mTransientIndices.get(transientIndex) == i) {
                final View transientChild = mTransientViews.get(transientIndex);
                if ((transientChild.mViewFlags & VISIBILITY_MASK) == VISIBLE ||
                        transientChild.getAnimation() != null) {
                    more |= drawChild(canvas, transientChild, drawingTime);
                }
                transientIndex++;
                if (transientIndex >= transientCount) {
                    transientIndex = -1;
                }
            }
            int childIndex = customOrder ? getChildDrawingOrder(childrenCount, i) : i;
            final View child = (preorderedList == null)
                    ? children[childIndex] : preorderedList.get(childIndex);
            if ((child.mViewFlags & VISIBILITY_MASK) == VISIBLE || child.getAnimation() != null) {
                more |= drawChild(canvas, child, drawingTime);
            }
        }
    }
    ...
}

protected boolean drawChild(Canvas canvas, View child, long drawingTime) {
    return child.draw(canvas, this, drawingTime);
}
```

  `dispatchDraw()`方法中首先获取子控件的数量`childrenCount`，然后遍历所有子控件，调用`drawChild()`方法，`drawChild()`方法中只是简单的调用`child.draw()`，这样就完成了子控件的绘制。细心的可以发现child的`draw()`方法又会执行之前`DecorView.draw()`的六步（`draw()`是在`View`里面实现的），所以说，所有的`控件在绘制的时候都会调用`draw()`方法，`draw()`方法中会先调用`onDraw()`方法绘制自己，然后调用`dispatchDraw()`绘制它的子控件，如果此控件不是`ViewGroup`的子类，也就是说是叶子控件，`dispatchDraw()`什么也不做。

  到此为止，`Activity`的整个显示过程就已经分析完毕，这篇文章主要讲解了`View`的**测量、布局、绘制**流程，这三个流程一共涉及到六个重要的方法`measure()`、`onMeasure()`、`layout()`、`onLayout()`、`draw()`、`onDraw()`；
  `onMeasure()`、`onLayout()`、`onDraw()`一般都由`View`的子类（具体的控件或者布局）重写，`measure()`、`layout()`、`draw()`在View中有默认的实现，他们都是xxx()调用onXxx()，这种形式就能依次完成整个布局树的绘制流程。将这篇文章的流程整理如下图：

![View的显示过程](http://img.blog.csdn.net/20180113164845838?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
[View的显示过程](http://img.blog.csdn.net/20180113164845838?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

## 参考源码工程：

[https://github.com/openXu/AndroidActivityLaunchProgress](https://github.com/openXu/AndroidActivityLaunchProgress)



引用：
[Activtiy完全解析（一、Activity的创建过程）](http://blog.csdn.net/xmxkf/article/details/52452218)
 [Activtiy完全解析（二、layout的inflate过程）](http://blog.csdn.net/xmxkf/article/details/52457893)
 [Activtiy完全解析（三、View的显示过程measure、layout、draw）](http://blog.csdn.net/xmxkf/article/details/52840065)




