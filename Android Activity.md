<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [【Android Activity】](#android-activity)
  - [什么是 Activity?](#%E4%BB%80%E4%B9%88%E6%98%AF-activity)
  - [Activity 生命周期](#activity-%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F)
    - [](#)
      - [什么情况下Activity走了onCreat()，而不走onStart()；](#%E4%BB%80%E4%B9%88%E6%83%85%E5%86%B5%E4%B8%8Bactivity%E8%B5%B0%E4%BA%86oncreat%E8%80%8C%E4%B8%8D%E8%B5%B0onstart)
      - [何时调用onRestart](#%E4%BD%95%E6%97%B6%E8%B0%83%E7%94%A8onrestart)
      - [onPause vs onStop](#onpause-vs-onstop)
    - [onResume](#onresume)
      - [onWindowFocusChanged](#onwindowfocuschanged)
      - [onStart()和onResume()有什么区别？](#onstart%E5%92%8Conresume%E6%9C%89%E4%BB%80%E4%B9%88%E5%8C%BA%E5%88%AB)
    - [横竖屏切换时 Activity 的生命周期](#%E6%A8%AA%E7%AB%96%E5%B1%8F%E5%88%87%E6%8D%A2%E6%97%B6-activity-%E7%9A%84%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F)
    - [两个Activity之间跳转的生命周期过程](#%E4%B8%A4%E4%B8%AAactivity%E4%B9%8B%E9%97%B4%E8%B7%B3%E8%BD%AC%E7%9A%84%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E8%BF%87%E7%A8%8B)
  - [Activity 的状态](#activity-%E7%9A%84%E7%8A%B6%E6%80%81)
    - [如何保存 Activity 状态](#%E5%A6%82%E4%BD%95%E4%BF%9D%E5%AD%98-activity-%E7%8A%B6%E6%80%81)
    - [各种情况下函数调用过程](#%E5%90%84%E7%A7%8D%E6%83%85%E5%86%B5%E4%B8%8B%E5%87%BD%E6%95%B0%E8%B0%83%E7%94%A8%E8%BF%87%E7%A8%8B)
      - [第一次进入ActivityA：](#%E7%AC%AC%E4%B8%80%E6%AC%A1%E8%BF%9B%E5%85%A5activitya)
      - [下拉系统菜单（已开启程序，从屏幕上往下拉）](#%E4%B8%8B%E6%8B%89%E7%B3%BB%E7%BB%9F%E8%8F%9C%E5%8D%95%E5%B7%B2%E5%BC%80%E5%90%AF%E7%A8%8B%E5%BA%8F%E4%BB%8E%E5%B1%8F%E5%B9%95%E4%B8%8A%E5%BE%80%E4%B8%8B%E6%8B%89)
      - [收回系统下拉菜单（已开启程序，且下拉菜单已显示）](#%E6%94%B6%E5%9B%9E%E7%B3%BB%E7%BB%9F%E4%B8%8B%E6%8B%89%E8%8F%9C%E5%8D%95%E5%B7%B2%E5%BC%80%E5%90%AF%E7%A8%8B%E5%BA%8F%E4%B8%94%E4%B8%8B%E6%8B%89%E8%8F%9C%E5%8D%95%E5%B7%B2%E6%98%BE%E7%A4%BA)
      - [从ActivityA中退出：](#%E4%BB%8Eactivitya%E4%B8%AD%E9%80%80%E5%87%BA)
      - [ActivityA启动ActivityB（普通Activity）：](#activitya%E5%90%AF%E5%8A%A8activityb%E6%99%AE%E9%80%9Aactivity)
      - [ActivityA启动ActivityB（对话框Activity）：](#activitya%E5%90%AF%E5%8A%A8activityb%E5%AF%B9%E8%AF%9D%E6%A1%86activity)
      - [从AcitivityB（普通Activity）返回到ActivityA：](#%E4%BB%8Eacitivityb%E6%99%AE%E9%80%9Aactivity%E8%BF%94%E5%9B%9E%E5%88%B0activitya)
      - [从ActivityB（对话框Activity）返回到ActivityA：](#%E4%BB%8Eactivityb%E5%AF%B9%E8%AF%9D%E6%A1%86activity%E8%BF%94%E5%9B%9E%E5%88%B0activitya)
      - [手机黑屏时：](#%E6%89%8B%E6%9C%BA%E9%BB%91%E5%B1%8F%E6%97%B6)
      - [手机亮屏时：](#%E6%89%8B%E6%9C%BA%E4%BA%AE%E5%B1%8F%E6%97%B6)
      - [按home键（已启动ActivityA）](#%E6%8C%89home%E9%94%AE%E5%B7%B2%E5%90%AF%E5%8A%A8activitya)
      - [多任务切回程序（开启程序，home键切换程序到后台）](#%E5%A4%9A%E4%BB%BB%E5%8A%A1%E5%88%87%E5%9B%9E%E7%A8%8B%E5%BA%8F%E5%BC%80%E5%90%AF%E7%A8%8B%E5%BA%8Fhome%E9%94%AE%E5%88%87%E6%8D%A2%E7%A8%8B%E5%BA%8F%E5%88%B0%E5%90%8E%E5%8F%B0)
      - [点击应用图标重启程序（开启程序，home键切换到后台）](#%E7%82%B9%E5%87%BB%E5%BA%94%E7%94%A8%E5%9B%BE%E6%A0%87%E9%87%8D%E5%90%AF%E7%A8%8B%E5%BA%8F%E5%BC%80%E5%90%AF%E7%A8%8B%E5%BA%8Fhome%E9%94%AE%E5%88%87%E6%8D%A2%E5%88%B0%E5%90%8E%E5%8F%B0)
      - [点击back键（已开启程序，back键未自己处理）](#%E7%82%B9%E5%87%BBback%E9%94%AE%E5%B7%B2%E5%BC%80%E5%90%AF%E7%A8%8B%E5%BA%8Fback%E9%94%AE%E6%9C%AA%E8%87%AA%E5%B7%B1%E5%A4%84%E7%90%86)
      - [点击多任务键（已开启程序）](#%E7%82%B9%E5%87%BB%E5%A4%9A%E4%BB%BB%E5%8A%A1%E9%94%AE%E5%B7%B2%E5%BC%80%E5%90%AF%E7%A8%8B%E5%BA%8F)
      - [切回程序（已开启程序，且已点击多任务键）](#%E5%88%87%E5%9B%9E%E7%A8%8B%E5%BA%8F%E5%B7%B2%E5%BC%80%E5%90%AF%E7%A8%8B%E5%BA%8F%E4%B8%94%E5%B7%B2%E7%82%B9%E5%87%BB%E5%A4%9A%E4%BB%BB%E5%8A%A1%E9%94%AE)
      - [横竖屏切换（未配置android:configChanges）](#%E6%A8%AA%E7%AB%96%E5%B1%8F%E5%88%87%E6%8D%A2%E6%9C%AA%E9%85%8D%E7%BD%AEandroidconfigchanges)
      - [横竖屏切换（配置android:configChanges="orientation"）](#%E6%A8%AA%E7%AB%96%E5%B1%8F%E5%88%87%E6%8D%A2%E9%85%8D%E7%BD%AEandroidconfigchangesorientation)
      - [横竖屏切换（配置android:configChanges="orientation|scre](#%E6%A8%AA%E7%AB%96%E5%B1%8F%E5%88%87%E6%8D%A2%E9%85%8D%E7%BD%AEandroidconfigchangesorientationscre)
      - [ActivityA启动ActivityB,再从ActivityB回到ActivityA,此时ActivityB的onDestory先调用还是ActivityA的onResume先调用？](#activitya%E5%90%AF%E5%8A%A8activityb%E5%86%8D%E4%BB%8Eactivityb%E5%9B%9E%E5%88%B0activitya%E6%AD%A4%E6%97%B6activityb%E7%9A%84ondestory%E5%85%88%E8%B0%83%E7%94%A8%E8%BF%98%E6%98%AFactivitya%E7%9A%84onresume%E5%85%88%E8%B0%83%E7%94%A8)
    - [](#-1)
  - [Activity 之间的跳转](#activity-%E4%B9%8B%E9%97%B4%E7%9A%84%E8%B7%B3%E8%BD%AC)
    - [1、显式跳转](#1%E6%98%BE%E5%BC%8F%E8%B7%B3%E8%BD%AC)
    - [2、隐式跳转](#2%E9%9A%90%E5%BC%8F%E8%B7%B3%E8%BD%AC)
      - [跳转到浏览器界面](#%E8%B7%B3%E8%BD%AC%E5%88%B0%E6%B5%8F%E8%A7%88%E5%99%A8%E7%95%8C%E9%9D%A2)
      - [跳转到发送短信界面](#%E8%B7%B3%E8%BD%AC%E5%88%B0%E5%8F%91%E9%80%81%E7%9F%AD%E4%BF%A1%E7%95%8C%E9%9D%A2)
    - [](#-2)
  - [Activity 的任务栈](#activity-%E7%9A%84%E4%BB%BB%E5%8A%A1%E6%A0%88)
  - [Activity 启动模式](#activity-%E5%90%AF%E5%8A%A8%E6%A8%A1%E5%BC%8F)
    - [1 standard](#1-standard)
    - [2 singleTop](#2-singletop)
    - [3 singleTask](#3-singletask)
    - [4 singleInstance](#4-singleinstance)
    - [onNewIntent](#onnewintent)
    - [一个启动模式为 singleTop 的 activity，如果再试图启动会怎样？](#%E4%B8%80%E4%B8%AA%E5%90%AF%E5%8A%A8%E6%A8%A1%E5%BC%8F%E4%B8%BA-singletop-%E7%9A%84-activity%E5%A6%82%E6%9E%9C%E5%86%8D%E8%AF%95%E5%9B%BE%E5%90%AF%E5%8A%A8%E4%BC%9A%E6%80%8E%E6%A0%B7)
    - [实际演示：](#%E5%AE%9E%E9%99%85%E6%BC%94%E7%A4%BA)
    - [代码设置启动模式](#%E4%BB%A3%E7%A0%81%E8%AE%BE%E7%BD%AE%E5%90%AF%E5%8A%A8%E6%A8%A1%E5%BC%8F)
      - [FLAG_ACTIVITY_BROUGHT_TO_FRONT](#flag_activity_brought_to_front)
      - [FLAG_ACTIVITY_CLEAR_TOP](#flag_activity_clear_top)
      - [FLAG_ACTIVITY_CLEAR_WHEN_TASK_RESET](#flag_activity_clear_when_task_reset)
      - [FLAG_ACTIVITY_EXCLUDE_FROM_RECENTS](#flag_activity_exclude_from_recents)
      - [FLAG_ACTIVITY_FORWARD_RESULT](#flag_activity_forward_result)
      - [FLAG_ACTIVITY_LAUNCHED_FROM_HISTORY](#flag_activity_launched_from_history)
      - [FLAG_ACTIVITY_MULTIPLE_TASK](#flag_activity_multiple_task)
      - [FLAG_ACTIVITY_NEW_TASK](#flag_activity_new_task)
      - [FLAG_ACTIVITY_NO_ANIMATION](#flag_activity_no_animation)
      - [FLAG_ACTIVITY_NO_HISTORY](#flag_activity_no_history)
      - [FLAG_ACTIVITY_NO_USER_ACTION](#flag_activity_no_user_action)
      - [FLAG_ACTIVITY_PREVIOUS_IS_TOP](#flag_activity_previous_is_top)
      - [FLAG_ACTIVITY_REORDER_TO_FRONT](#flag_activity_reorder_to_front)
      - [FLAG_ACTIVITY_RESET_TASK_IF_NEEDED](#flag_activity_reset_task_if_needed)
      - [FLAG_ACTIVITY_SINGLE_TOP](#flag_activity_single_top)
      - [taskAffinity](#taskaffinity)
      - [若是已经启动了四个Activity：A，B，C和D，在D Activity里，想再启动一个Actvity B，但不变成A，B，C，D，B，而是A，C，D，B，则可以像下面写代码：](#%E8%8B%A5%E6%98%AF%E5%B7%B2%E7%BB%8F%E5%90%AF%E5%8A%A8%E4%BA%86%E5%9B%9B%E4%B8%AAactivityabc%E5%92%8Cd%E5%9C%A8d-activity%E9%87%8C%E6%83%B3%E5%86%8D%E5%90%AF%E5%8A%A8%E4%B8%80%E4%B8%AAactvity-b%E4%BD%86%E4%B8%8D%E5%8F%98%E6%88%90abcdb%E8%80%8C%E6%98%AFacdb%E5%88%99%E5%8F%AF%E4%BB%A5%E5%83%8F%E4%B8%8B%E9%9D%A2%E5%86%99%E4%BB%A3%E7%A0%81)
- [Android Activity面试题](#android-activity%E9%9D%A2%E8%AF%95%E9%A2%98)
  - [什么是 Activity?](#%E4%BB%80%E4%B9%88%E6%98%AF-activity-1)
  - [描述一下 Activity 生命周期](#%E6%8F%8F%E8%BF%B0%E4%B8%80%E4%B8%8B-activity-%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F)
  - [横竖屏切换时 Activity 的生命周期](#%E6%A8%AA%E7%AB%96%E5%B1%8F%E5%88%87%E6%8D%A2%E6%97%B6-activity-%E7%9A%84%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F-1)
  - [两个 Activity 之间跳转时必然会执行的是哪几个方法？（重要）](#%E4%B8%A4%E4%B8%AA-activity-%E4%B9%8B%E9%97%B4%E8%B7%B3%E8%BD%AC%E6%97%B6%E5%BF%85%E7%84%B6%E4%BC%9A%E6%89%A7%E8%A1%8C%E7%9A%84%E6%98%AF%E5%93%AA%E5%87%A0%E4%B8%AA%E6%96%B9%E6%B3%95%E9%87%8D%E8%A6%81)
  - [Activity 的状态都有哪些？](#activity-%E7%9A%84%E7%8A%B6%E6%80%81%E9%83%BD%E6%9C%89%E5%93%AA%E4%BA%9B)
  - [如何保存 Activity 状态](#%E5%A6%82%E4%BD%95%E4%BF%9D%E5%AD%98-activity-%E7%8A%B6%E6%80%81-1)
  - [请描述一下 Activity 的启动模式都有哪些以及各自的特点](#%E8%AF%B7%E6%8F%8F%E8%BF%B0%E4%B8%80%E4%B8%8B-activity-%E7%9A%84%E5%90%AF%E5%8A%A8%E6%A8%A1%E5%BC%8F%E9%83%BD%E6%9C%89%E5%93%AA%E4%BA%9B%E4%BB%A5%E5%8F%8A%E5%90%84%E8%87%AA%E7%9A%84%E7%89%B9%E7%82%B9)
  - [将一个 Activity 设置成窗口的样式：](#%E5%B0%86%E4%B8%80%E4%B8%AA-activity-%E8%AE%BE%E7%BD%AE%E6%88%90%E7%AA%97%E5%8F%A3%E7%9A%84%E6%A0%B7%E5%BC%8F)
  - [如何退出 Activity？如何安全退出已调用多个 Activity 的 Application？](#%E5%A6%82%E4%BD%95%E9%80%80%E5%87%BA-activity%E5%A6%82%E4%BD%95%E5%AE%89%E5%85%A8%E9%80%80%E5%87%BA%E5%B7%B2%E8%B0%83%E7%94%A8%E5%A4%9A%E4%B8%AA-activity-%E7%9A%84-application)
  - [两个 Activity 之间传递数据，有哪些方式？(2017-2-23)](#%E4%B8%A4%E4%B8%AA-activity-%E4%B9%8B%E9%97%B4%E4%BC%A0%E9%80%92%E6%95%B0%E6%8D%AE%E6%9C%89%E5%93%AA%E4%BA%9B%E6%96%B9%E5%BC%8F2017-2-23)
    - [1、通过intent传递数据](#1%E9%80%9A%E8%BF%87intent%E4%BC%A0%E9%80%92%E6%95%B0%E6%8D%AE)
      - [1）intent.putExtra(key, value)](#1intentputextrakey-value)
        - [序列化对象Seriazable](#%E5%BA%8F%E5%88%97%E5%8C%96%E5%AF%B9%E8%B1%A1seriazable)
        - [Parcelable（最常用，传递数据效率更高）](#parcelable%E6%9C%80%E5%B8%B8%E7%94%A8%E4%BC%A0%E9%80%92%E6%95%B0%E6%8D%AE%E6%95%88%E7%8E%87%E6%9B%B4%E9%AB%98)
      - [2）intent.putExtras(bundle)](#2intentputextrasbundle)
    - [2、广播接收者](#2%E5%B9%BF%E6%92%AD%E6%8E%A5%E6%94%B6%E8%80%85)
    - [3、content provider](#3content-provider)
    - [4、Application共享数据](#4application%E5%85%B1%E4%BA%AB%E6%95%B0%E6%8D%AE)
    - [5、使用单例](#5%E4%BD%BF%E7%94%A8%E5%8D%95%E4%BE%8B)
    - [6、静态Statis](#6%E9%9D%99%E6%80%81statis)
      - [1）数据结构体中](#1%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84%E4%BD%93%E4%B8%AD)
      - [2）activity中](#2activity%E4%B8%AD)
    - [7、持久化数据](#7%E6%8C%81%E4%B9%85%E5%8C%96%E6%95%B0%E6%8D%AE)
    - [8、如果不跨进程，可以使用 EventBus](#8%E5%A6%82%E6%9E%9C%E4%B8%8D%E8%B7%A8%E8%BF%9B%E7%A8%8B%E5%8F%AF%E4%BB%A5%E4%BD%BF%E7%94%A8-eventbus)
    - [9、startActivityForResult](#9startactivityforresult)
    - [10、使用剪切板传递数据](#10%E4%BD%BF%E7%94%A8%E5%89%AA%E5%88%87%E6%9D%BF%E4%BC%A0%E9%80%92%E6%95%B0%E6%8D%AE)
      - [传递String](#%E4%BC%A0%E9%80%92string)
      - [传递对象](#%E4%BC%A0%E9%80%92%E5%AF%B9%E8%B1%A1)
    - [](#-3)
  - [怎样在两个 Activity 之间传递一张图片（2017-2-23）](#%E6%80%8E%E6%A0%B7%E5%9C%A8%E4%B8%A4%E4%B8%AA-activity-%E4%B9%8B%E9%97%B4%E4%BC%A0%E9%80%92%E4%B8%80%E5%BC%A0%E5%9B%BE%E7%89%872017-2-23)
  - [如何实现切换主题功能？（2017-2-23）](#%E5%A6%82%E4%BD%95%E5%AE%9E%E7%8E%B0%E5%88%87%E6%8D%A2%E4%B8%BB%E9%A2%98%E5%8A%9F%E8%83%BD2017-2-23)
    - [1.内置主题](#1%E5%86%85%E7%BD%AE%E4%B8%BB%E9%A2%98)
      - [1.1定义属性](#11%E5%AE%9A%E4%B9%89%E5%B1%9E%E6%80%A7)
      - [1.2定义主题](#12%E5%AE%9A%E4%B9%89%E4%B8%BB%E9%A2%98)
      - [1.3在布局文件中使用](#13%E5%9C%A8%E5%B8%83%E5%B1%80%E6%96%87%E4%BB%B6%E4%B8%AD%E4%BD%BF%E7%94%A8)
      - [1.4设置主题](#14%E8%AE%BE%E7%BD%AE%E4%B8%BB%E9%A2%98)
      - [1.5切换主题](#15%E5%88%87%E6%8D%A2%E4%B8%BB%E9%A2%98)
    - [2.APK主题](#2apk%E4%B8%BB%E9%A2%98)
  - [Android 中 Activity 是如何启动的？（2017-2-24）](#android-%E4%B8%AD-activity-%E6%98%AF%E5%A6%82%E4%BD%95%E5%90%AF%E5%8A%A8%E7%9A%842017-2-24)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->





# 【Android Activity】

<span id="什么是 Activity?"/>
## 什么是 Activity?
四大组件之一,通常一个用户交互界面对应一个 activity。activity 是**Context 的子类**,同时**实现了 window.callback和 keyevent.callback**, 可以处理与窗体用户交互的事件。

常见的 Activity 类型有 **FragmentActivitiy，ListActivity，TabAcitivty** 等。 如果界面有共同的特点或者功能的时候,还会自己定义一个 BaseActivity。

<span id="Activity 生命周期"/>
## Activity 生命周期
![](http://upload-images.jianshu.io/upload_images/9028834-e0a718821b61fa15.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
两两对应：onCreate 创建与 onDestroy 销毁；onStart 可见与 onStop 不可见；onResume 可编辑（即焦点）与 onPause；

### 

<span id="什么情况下Activity走了onCreat()，而不走onStart()；"/>
####  什么情况下Activity走了onCreat()，而不走onStart()；
```
当onCreate中发生Crash的时候。
```

####  何时调用onRestart 
在 Activity 被 onStop 后，但是没有被 onDestroy，在再次启动此 Activity 时就调用 onRestart（而不再调用 onCreate）方法；如果被 onDestroy 了，则是调用 onCreate 方法。


####  onPause vs onStop
(a)  只要activity还可见，就不会调用onStop。所以我们可以知道onStop调用时机是：当一个activity对用户已经不可见的时候就会调用。
(b)  官方说明：**onStop可能永远不会被调用**：当处于低内存状态下，系统没有足够的内存保持你的activity的处理运行在activity的onPause调用之后。
(c)   从(b)知道onStop在低内存情况下不会被调用，但是：**onPause一定会被调用**。


### onResume
  对于你的activity来说，**onResume调用后就可以和用户交互**，这是很好的地方去开始动画，打开互斥访问设备元件（例如：照相机），etc.
  但是有一点我们必须认清：**onResume不是最好的指向标说明你的activity对于用户是可见的**，例如系统的窗口可能在你的activity前面。所以应该用：onWindowFocusChanged来知道我们的activity是否对用户可见。
####  onWindowFocusChanged
当前activity的窗口获得或失去焦点的时候会调用。（activity是否对用户可见）

大多数情况下只要调用了onResume 就会调用 onWindowFocusChanged，也有例外，比如下拉系统菜单的时候只会调用onWindowFocusChanged。
**应用**：我们在下拉菜单中改变了网络的状态（开启或者关闭），我们这时候就不能在onResume()中处理更新网络状态，而应该将更新网络状态放到onWindowFocusChanged中处理。

>**官方解释**：
>当前activity的窗口获得或失去焦点的时候会调用。
>这个函数是最好的方向标对于activity是否对用户可见，它默认的实现是清除键跟踪状态，所以应该总是被调用。
>它也提供了全局的焦点状态，它的管理是独立于activity生命周期的。当焦点改变时一般都伴随着生命周期的改变，你不应该依赖onWindowFocusChanged 调用和其他生命周期的方法(例如onResume) 的先后顺序，来处理我们要做的事情。
>通常的规则是，当一个activity被唤醒，那么就拥有窗口焦点。除非这个窗口已经显示了对话框或者其他弹出框抢占焦点，这个窗口只有等到这些对话框关闭后，才能获取焦点，同理，当系统显示系统级的窗口，系统级的窗口会临时的获取窗口输入焦点同时不会暂停前景 activity。

####  onStart()和onResume()有什么区别？
```
在onStart()中视图不可见，在onResume()中视图可见；onStart()属于可见进程，onResume()属于前台进程；
```
<span id="横竖屏切换时 Activity 的生命周期"/>
###  横竖屏切换时 Activity 的生命周期
此时的生命周期跟清单文件里的配置有关系。
1、**不设置** Activity 的 **android:configChanges** 时，切屏会重新调用各个生命周期 默认首先**销毁**当前 activity，然后**重新加载**。
如下图，当横竖屏切换时先执行 onPause/onStop 方法
![](http://upload-images.jianshu.io/upload_images/9028834-190e1a47936c04c6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
2、**设置** Activity 的 **android:configChanges="orientation|screenSize**"时（两个都要设置），切屏不会重新调用各个生命周期，**只**会执行 **onConfigurationChanged** 方法。
3、**设置**Activity的**android:screenOrientation**属性时，切屏任何方法都**不调用**
注意：
1.如果`<activity>`配置了android:screenOrientation属性，则会使android:configChanges="orientation"失效。
2.模拟器与真机差别很大：模拟器中如果不配置android:configChanges属性或配置值为orientation，切到横屏执行一次销毁->重建，切到竖屏执行两次。真机均为一次。模拟其中如果配置android:configChanges="orientation|keyboardHidden"，切竖屏执行一次onConfigurationChanged，切竖屏执行两次。真机均为一次。

###  两个Activity之间跳转的生命周期过程
一般情况下比如说有两个 activity,分别叫 A,B,当在 A 里面激活 B 组件的时候, A 会调用 onPause()方法,然后 B 调 用 onCreate() ,onStart(), onResume()。
这个时候 **B 覆盖了窗体, A 会调用 onStop()方法.	如果 B 是个透明的,或者是对话框的样式, 就不会调用 A 的onStop()方法**。

## Activity 的状态
a) foreground activity 
b) visible activity
c) background activity 
d) empty process

<span id="如何保存 Activity 状态"/>
###  如何保存 Activity 状态
Activity 的状态**通常情况下系统会自动保存**。 
一般来说, **调用 onPause()和 onStop()方法后的 activity 实例仍然存在于内存中**, activity 的所有信息和状态数据不会消失, 当 activity 重新回到前台之后, 所有的改变都会得到保留。
但是当系统**内存不足**时, **调用 onPause()和 onStop()方法后的 activity 可能会被系统摧毁**, 此时内存中就不会存有该 activity 的实例对象了。如果之后这个 activity 重新回到前台, 之前所作的改变就会消失。

**解决方案**：覆写 onSaveInstanceState()方法。
onSaveInstanceState()方法接受一个 Bundle 类型的参数, 开发者可以 将状态数据存储到这个 Bundle 对象中, 这样即使 activity 被系统摧毁, 当用户重新启动这个 activity 而调用它的 onCreate()方法时, 上述的 Bundle 对象会作为实参传递给 onCreate()方法, 开发者可以从 Bundle 对象中取出保存的 数据, 然后利用这些数据将 activity 恢复到被摧毁之前的状态。

需要注意的是, onSaveInstanceState()方法并不是一定会被调用的, 因为有些场景是不需要保存状态数据的。比如用户按下 BACK 键退出 activity 时, 用户显然想要关闭这个 activity, 此时是没有必要保存数据以供下次恢复的, 也就是 onSaveInstanceState()方法不会被调用。
如果调用 onSaveInstanceState()方法, **调用将发生在 onPause()或onStop()方法之前**。

```java
@Override
protected void onSaveInstanceState(Bundle outState) {
// TODO Auto-generated method stub
super.onSaveInstanceState(outState);
}
```


<span id="各种情况下函数调用过程"/>
###  各种情况下函数调用过程

####  第一次进入ActivityA：
A | onCreate
A | onStart
A | onResume
A | onWindowFocusChanged | hasFocus：true

####  下拉系统菜单（已开启程序，从屏幕上往下拉）
A | onWindowFocusChanged | hasFocus：false

####  收回系统下拉菜单（已开启程序，且下拉菜单已显示）
A | onWindowFocusChanged | hasFocus：true

####  从ActivityA中退出：
A | onPause
A | onWindowFocusChanged | hasFocus：false
A | onStop
A | onDestory


####  ActivityA启动ActivityB（普通Activity）：
A | onWindowFocusChanged | hasFocus：false
A | onPause
B | onCreate
B | onStart
B | onResume
B | onWindowFocusChanged | hasFocus：true
A | onSaveInstanceState | param: 1
A | onStop


####  ActivityA启动ActivityB（对话框Activity）：
A | onWindowFocusChanged | hasFocus：false
A | onPause
B | onCreate
B | onStart
B | onResume
B | onWindowFocusChanged | hasFocus：true


####  从AcitivityB（普通Activity）返回到ActivityA：
B | onPause
A | onRestart
A | onStart
A | onResume
A | onWindowFocusChanged | hasFocus：true
B | onWindowFocusChanged | hasFocus：false
B | onStop
B | onDestory



####  从ActivityB（对话框Activity）返回到ActivityA：
B | onPause
A | onResume
A | onWindowFocusChanged | hasFocus：true
B | onWindowFocusChanged | hasFocus：false
B | onStop
B | onDestory



####  手机黑屏时：
A | onPause
A | onSaveInstanceState | param: 1
A | onStop
A | onWindowFocusChanged | hasFocus：false



####  手机亮屏时：
A | onWindowFocusChanged | hasFocus：true
A | onRestart
A | onStart
A | onResume

####  按home键（已启动ActivityA）
A | onPause
A | onWindowFocusChanged | hasFocus：false
A | onSaveInstanceState | param: 1
A | onStop

####  多任务切回程序（开启程序，home键切换程序到后台）
A | onRestart
A | onStart
A | onResume
A | onWindowFocusChanged | hasFocus：true

####  点击应用图标重启程序（开启程序，home键切换到后台）
A | onRestart
A | onStart
A | onResume
A | onWindowFocusChanged | hasFocus：true

####  点击back键（已开启程序，back键未自己处理）
A | onPause
A | onWindowFocusChanged | hasFocus：false
A | onStop
A | onDestory

####  点击多任务键（已开启程序）
A | onWindowFocusChanged | hasFocus：false
A | onPause
A | onSaveInstanceState | param: 1
A | onStop

####  切回程序（已开启程序，且已点击多任务键）
A | onRestart
A | onStart
A | onResume
A | onWindowFocusChanged | hasFocus：true

####  横竖屏切换（未配置android:configChanges）
A | onPause
A | onSaveInstanceState | param: 1
A | onStop
A | onDestory
A | onCreate
A | onStart
A | onRestoreInstanceState | param: 1
A | onResume
A | onWindowFocusChanged | hasFocus：true

####  横竖屏切换（配置android:configChanges="orientation"）
竖切横
A | onConfigurationChanged | newConfig：{1.0 460mcc3mnc [zh_CN_#Hans] ldltr sw360dp w592dp h336dp 320dpi nrml land finger -keyb/v/h -nav/h suim:1 s.22}
A | onPause
A | onSaveInstanceState | param: 1
A | onStop
A | onDestory
A | onCreate
A | onStart
A | onRestoreInstanceState | param: 1
A | onResume
A | onWindowFocusChanged | hasFocus：true
横切竖
A | onConfigurationChanged | newConfig：{1.0 460mcc3mnc [zh_CN_#Hans] ldltr sw360dp w360dp h580dp 320dpi nrml port finger -keyb/v/h -nav/h suim:1 s.23}

####  横竖屏切换（配置android:configChanges="orientation|scre
竖切横
A | onConfigurationChanged | newConfig：{1.15 460mcc3mnc [zh_CN_#Hans] ldltr sw360dp w592dp h336dp 320dpi nrml land finger -keyb/v/h -nav/h suim:1 s.46}
横切竖
A | onConfigurationChanged | newConfig：{1.15 460mcc3mnc [zh_CN_#Hans] ldltr sw360dp w360dp h580dp 320dpi nrml port finger -keyb/v/h -nav/h suim:1 s.47}



####  ActivityA启动ActivityB,再从ActivityB回到ActivityA,此时ActivityB的onDestory先调用还是ActivityA的onResume先调用？
```
ActivityA的onResume()先调用，ActivityB的onDestory后调用。
```
### 
## Activity 之间的跳转
Activity 之间的跳转分为 2 种：
**显式跳转**：在可以引用到那个类, 并且可以引用到那个类的字节码时可以使用。一般**用于自己程序的内部**。显式跳转不可以跳转到其他程序的页面中。
**隐式跳转**：可以在当前程序跳转到另一个程序的页面。隐式跳转不需要引用到那个类,但是必须**得知道那个界面的动作(action)和信息(category)**。

Activity 之间通过 Intent 进行通信。Intent 即意图，用于描述一个页面的信息,同时也是一个数据的载体。

### 1、显式跳转
```java
//创建一个 Intent 对象，并传递当前对象（Context 对象）和要跳转的Activity 类字节码
Intent intent = new Intent(this, SecondActivity.class);
//启动第二个 Activity
startActivity(intent);
```
### 2、隐式跳转
隐式跳转可以跳转到其他程序的 Activity，只要知道 Activity 的动作(action)以及信息(category)。
因此，能够**被隐式跳转的 Activity**，在清单文件中声明时必须**指定动作和信息**这两个属性。
修改工程清单文件中 SecondActivity 的配置信息。

```xml
<activity android:name="com.itheima.activitySkip.SecondActivity">
    <!-- 配置意图过滤器 -->
    <intent-filter >
        <!-- 在意图过滤器中设置 action 和 category，当有匹配的 action 和 category的时候启动该 Activity。这里使用 Android 提供的默认 category 即可 -->
        <action android:name="com.itheima.activitySkip.SecondActivity"/>
        <category android:name="android.intent.category.DEFAULT"/>
    </intent-filter>
</activity>
```

```java
//创建一个 Intent 对象
Intent intent = new Intent();
//设置 Action
intent.setAction("com.itheima.activitySkip.SecondActivity");
//对于 android.intent.category.DEFAULT 类型的信息为 Android 系统默认的信息，省略也可以
intent.addCategory("android.intent.category.DEFAULT");
//启动 Activity
startActivity(intent);
```
#### 跳转到浏览器界面
```java
    //跳转到浏览器界面
    public void skip2Browser(View view) {
//创建一个 Intent 对象
        Intent intent = new Intent();
//设置 Action
        intent.setAction("android.intent.action.VIEW");
//设置 category
        intent.addCategory("android.intent.category.BROWSABLE");
//设置参数
        intent.setData(Uri.parse("http://www.itheima.com"));//通过浏览器打开此链接
//启动 Activity
        startActivity(intent);
    }
```
#### 跳转到发送短信界面
```java
//跳转到发送短信界面
public void skip2Mms(View view){
//创建一个 Intent 对象
    Intent intent = new Intent();
//设置 Action
    intent.setAction("android.intent.action.VIEW");
//设置 category
    intent.addCategory("android.intent.category.BROWSABLE");
//设置参数
    intent.setData(Uri.parse("sms:10086"));
//设置短信内容
    intent.putExtra("sms_body", "301");
//启动 Activity
    startActivity(intent);
}
```


### 
## Activity 的任务栈
任务栈是用来提升用户体验而设计的:
1. 程序打开时就创建了一个任务栈, 用于存储当前程序的 activity,所有的 activity 属于一个任务栈。
2. **一个任务栈包含了一个 activity 的集合**, 去有序的选择哪一个 activity 和用户进行交互:
  只有在任务栈栈顶的 activity 才可以跟用户进行交互。
3. 任务栈可以移动到后台, 并且保留了每一个 activity 的状态. 并且有序的给用户列出它们的任务, 而且还不丢失它们状态信息。
4. 退出应用程序时：当把所有的任务栈中所有的 activity 清除出栈时,任务栈会被销毁,程序退出。

任务栈的**缺点**：
1. 每开启一次页面都会在任务栈中添加一个 Activity,而只有任务栈中的 Activity 全部清除出栈时，任务栈被销毁，程序才会退出,这样就造成了用户体验差, 需要点击多次返回才可以把程序退出了。
2. 每开启一次页面都会在任务栈中添加一个Activity还会造成数据冗余, 重复数据太多, 会导致内存溢出的问题(OOM)。

为了解决任务栈产生的问题，Android 为 Activity 设计了启动模式。

<span id="Activity 启动模式"/>
## Activity 启动模式
启动模式（launchMode）在多个 Activity 跳转的过程中扮演着重要的角色，它可以决定是否生成新的 Activity 实例，是否重用已存在的 Activity 实例，是否和其他 Activity 实例公用一个 task 里。
>**task** 是一个具有栈结构的对象，一个 task 可以管理多个 Activity，启动一个应用，也就创建一个与之对应的 task。


Activity 一共有以下四种 launchMode：
1.standard
2.singleTop
3.singleTask
4.singleInstance
可以在 AndroidManifest.xml 配置<activity>的 android:launchMode 属性为以上四种之一即可。

### 1 standard
默认启动模式，每次激活Activity时都会创建Activity，并放入任务栈中。

standard 模式是默认的启动模式，不用为<activity>配置 android:launchMode 属性即可，当然也可以指定值 为 standard。

原理如下：
![](http://upload-images.jianshu.io/upload_images/9028834-bb38380bd43aeaf3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
如图所示，**每次跳转系统都会在 task 中生成一个新的 FirstActivity 实例，并且放于栈结构的顶部**，当我们按下后退键时，才能看到原来的 FirstActivity 实例。

### 2 singleTop
如果在任务的栈顶正好存在该Activity的实例， 就重用该实例，否者就会创建新的实例并放入栈顶(即使栈中已经存在该Activity实例，只要不在栈顶，都会创建实例)。
原理：
![](http://upload-images.jianshu.io/upload_images/9028834-e0eb53c4a50b1300.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/9028834-acd2e306fd3cf50e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 3 singleTask
如果在栈中已经有该Activity的实例，就重用该实例(会调用实例的onNewIntent())。重用时，会让该实例回到栈顶，因此在它上面的实例将会被移除栈。如果栈中不存在该实例，将会创建新的实例放入栈中。 
原理：
![](http://upload-images.jianshu.io/upload_images/9028834-e3696bc0a119d301.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
在图中的下半部分是 SecondActivity 跳转到 FirstActivity 后的栈结构变化的结果，我们注意到，SecondActivity消失了，没错，在这个跳转过程中系统发现有存在的 FirstActivity 实例，于是不再生成新的实例，而是将 FirstActivity之上的 Activity 实例统统出栈，将 FirstActivity 变为栈顶对象，显示到幕前。

### 4 singleInstance
在一个新栈中创建该Activity实例，并让多个应用共享改栈中的该Activity实例。一旦该模式的Activity的实例存在于某个栈中，任何应用再激活改Activity时都会重用该栈中的实例，其效果相当于**多个应用程序共享一个应用，不管谁激活该Activity都会进入同一个应用中**。


我们修改 FirstActivity 的 launchMode="standard"，SecondActivity 的launchMode="singleInstance"，由于涉及到了多个栈结构，我们需要在每个 Activity 中显示当前栈结构的 id，所以我们为每个 Activity 添加如下代码：
```
textView.setText(this.toString());
taskIdView.setText("current task id: " + this.getTaskId());
```
然后我们再演示一下这个流程：
![](http://upload-images.jianshu.io/upload_images/9028834-a9e35c023f1a999e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
我们发现这两个 Activity 实例分别被放置在不同的栈结构中，关于 singleInstance 的原理图如下
![](http://upload-images.jianshu.io/upload_images/9028834-efcb051af1d178d2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
我们看到从 FirstActivity 跳转到 SecondActivity 时，重新启用了一个新的栈结构，来放置 SecondActivity 实例，然后按下后退键，再次回到原始栈结构；图中下半部分显示的在 SecondActivity 中再次跳转到 FirstActivity，这个时候系统会在原始栈结构中生成一个 FirstActivity 实例，然后回退两次，注意，并没有退出，而是回到了 SecondActivity，


为什么呢？是因为从 SecondActivity 跳转到 FirstActivity 的时候，我们的起点变成了 SecondActivity 实例所在的栈结构，这样一来，我们需要“回归”到这个栈结构。


如果我们修改 FirstActivity 的 launchMode 值为 singleTop、singleTask、singleInstance 中的任意一个，流程将会如图所示：
![](http://upload-images.jianshu.io/upload_images/9028834-1be112409ab05727.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
singleInstance 启动模式可能是最复杂的一种模式，为了帮助大家理解，我举一个例子，假如我们有一个 share 应用， 其中的 ShareActivity 是入口 Activity，也是可供其他应用调用的 Activity，我们把这个 Activity 的启动模式设置为 singleInstance，然后在其他应用中调用。我们编辑 ShareActivity 的配置：
```
        <activity
            android:name=".ShareActivity"
            android:launchMode="singleInstance">
            <intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
            <intent-filter>
                <action android:name="android.intent.action.SINGLE_INSTANCE_SHARE" />
                <category android:name="android.intent.category.DEFAULT" />
            </intent-filter>
        </activity>
```
然后我们在其他应用中这样启动该 Activity：
```
Intent intent = new Intent("android.intent.action.SINGLE_INSTANCE_SHARE");
startActivity(intent);
```
当我们打开 ShareActivity 后再按后退键回到原来界面时，ShareActivity 做为一个独立的个体存在，如果这时我 们打开 share 应用，无需创建新的 ShareActivity 实例即可看到结果，因为系统会自动查找，存在则直接利用。大家 可以在 ShareActivity 中打印一下 taskId，看看效果。关于这个过程，原理图如下：
![](http://upload-images.jianshu.io/upload_images/9028834-631ce871ae14128d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### onNewIntent
**重复启动同一个Activity实例时会调用onNewIntent**()
Activity第一次启动的时候执行onCreate()---->onStart()---->onResume()等后续生命周期函数，也就时说第一次启动Activity并不会执行到onNewIntent(). 而后面如果再有想启动该Activity的时候，那就是执行onNewIntent()---->onResart()--->onStart()----->onResume()。如果android系统由于内存不足把已存在Activity释放掉了，那么再次调用的时候会重新启动Activity即执行onCreate()---->onStart()---->onResume()等。

当调用到onNewIntent(intent)的时候，需**要在onNewIntent**() 中使用**setIntent**(intent)赋值给Activity的Intent.**否则**，后续的**getIntent**()都是**得到老的Intent**。

### 一个启动模式为 singleTop 的 activity，如果再试图启动会怎样？
面试官想问的是**onNewIntent()**

Activity 有一个 onNewIntent(Intent intent)回调方法，该方法我们几乎很少使用，导致已经将其忽略掉。该方法的官方解释如下：
This is called  for activities that set launchMode to "singleTop" in their  package, or if a client  used
the Intent.FLAG_ACTIVITY_SINGLE_TOP flag when calling startActivity. In either  case, when the activity is re-launched while  at the top of the activity stack  instead of a new instance of the activity being started, onNewIntent() will be called  on the existing instance with the Intent  that was used to re-launch it.
An activity will always be paused before  receiving a new intent, so you can count on onResume being called after  this method.
Note that getIntent still  returns the original Intent. You can use setIntent to update  it to this new
Intent.

上文大概意思如下：

该方法被启动模式设置为“singleTop”的 Activity 回调，或者当通过设置 Intent.FLAG_ACTIVITY_SINGLE_TOP 的 Intent 启动 Activity 时被回调。在任何情况下，只要当栈顶的 Activity 被重新启动时没有重新创建一个新的 Activity 实例而是依然使用该 Activity 对象，那么 onNewIntent(Intent)方法就会被回调。
当一个 Activity 接收到新 Intent 的时候会处于暂停状态，因此你可以统计到 onResume()方法会被再次执行，当 然这个执行是在 onNewIntent 之后的。
注意：如果我们在 Activity 中调用了 getIntent（）方法，那么返回的 Intent 对象还是老的 Intent（也就是第一 次启动该 Activity 时的传入的 Intent 对象），但是如果想让 getIntent（）返回最新的 Intent，那么我们可以通过 setIntent(Intent)方法设置。

### 实际演示：
| 启动方式           | 启动Activity                     | 执行函数                           | task中Activity        |
| -------------- | ------------------------------ | ------------------------------ | -------------------- |
| Standard       | StandardActivity@96267d3       | onCreate                       |                      |
|                |                                |                                | onStart              |
|                |                                |                                | onResume TaskId: 187 |
| SingleInstance | SingleInstanceActivity@a2e46b7 | onCreate                       |                      |
|                |                                |                                | onStart              |
|                |                                |                                | onResume TaskId: 188 |
| SingleInstance | SingleInstanceActivity@a2e46b7 | **onNewIntent**                |                      |
|                |                                |                                | onResume TaskId: 188 |
| SingleTask     | SingleTaskActivity@e0f0b0a     | onCreate                       |                      |
|                |                                |                                | onStart              |
|                |                                |                                | onResume TaskId: 187 |
| SingleTask     | SingleTaskActivity@e0f0b0a     | **onNewIntent**                |                      |
|                |                                |                                | onResume TaskId: 187 |
| SingleTop      | SingleTopActivity@21bfd59      | onCreate                       |                      |
|                |                                |                                | onStart              |
|                |                                |                                | onResume TaskId: 187 |
| SingleTop      | SingleTopActivity@21bfd59      | **onNewIntent**                |                      |
|                |                                |                                | onResume TaskId: 187 |
| Standard       | StandardActivity@9f84b54       | onCreate                       |                      |
|                |                                |                                | onStart              |
|                |                                |                                | onResume TaskId: 187 |
| Standard       | StandardActivity@a364129       | onCreate                       |                      |
|                |                                |                                | onStart              |
|                |                                |                                | onResume TaskId: 187 |
| SingleTop      | SingleTopActivity@a9c182a      | onCreate                       |                      |
|                |                                |                                | onStart              |
|                |                                |                                | onResume TaskId: 187 |
| SingleTask     | SingleTopActivity@21bfd59      | onDestroy                      |                      |
|                |                                | StandardActivity@9f84b54       | onDestroy            |
|                |                                | StandardActivity@a364129       | onDestroy            |
|                |                                | SingleTaskActivity@e0f0b0a     | onNewIntent          |
|                |                                |                                | onRestart            |
|                |                                |                                | onStart              |
|                |                                |                                | onResume TaskId: 187 |
|                |                                | SingleTopActivity@a9c182a      | onDestroy            |
| back           | StandardActivity@96267d3       | onRestart                      |                      |
|                |                                |                                | onStart              |
|                |                                |                                | onResume TaskId: 187 |
|                |                                | SingleTaskActivity@e0f0b0a     | onDestroy            |
| back           | StandardActivity@96267d3       | onDestroy                      |                      |
|                |                                | SingleInstanceActivity@a2e46b7 | onRestart            |
|                |                                |                                | onStart              |
|                |                                |                                | onResume TaskId: 188 |

### 代码设置启动模式
```
Intent intent = new Intent（this， B.class）; 
intent.setFlags（Intent.FLAG_ACTIVITY_CLEAR_TOP）; 
startActivity（intent）; 
```
Task就像一个容器，而Activity就相当与填充这个容器的东西，第一个东西（Activity）则会处于最下面，最后添加的东西（Activity）则会在最上面。从Task中取出东西（Activity）是从最顶端取出，也就是说最先取出的是最后添加的东西（Activity），以此类推，最后取出的是第一次添加的Activity，而Activity在Task中的顺序是可以控制的，在Activity跳转时用到Intent Flag可以设置新建activity的创建方式；

Intent常用标识：
#### FLAG_ACTIVITY_BROUGHT_TO_FRONT 
这个标志一般不是由程序代码设置的，如在launchMode中设置**singleTask**模式时系统帮你设定。

#### FLAG_ACTIVITY_CLEAR_TOP
&emsp;    **+FLAG_ACTIVITY_SINGLE_TOP=singleTask**：如果设置，并且这个Activity已经在当前的Task中运行，因此，不再是重新启动一个这个Activity的实例，而是在这个Activity上方的所有Activity都将关闭，然后这个Intent会作为一个新的Intent投递到老的Activity（现在位于顶端）中。
&emsp;    例如，假设一个Task中包含这些Activity：A，B，C，D。如果D调用了startActivity()，并且包含一个指向Activity B的Intent，那么，C和D都将结束，然后B接收到这个Intent，因此，目前stack的状况是：A，B。
&emsp;    上例中正在运行的Activity B**既可以在onNewIntent()中接收到这个新的Intent，也可以把自己关闭然后重新启动来接收这个Intent**。如果它的启动模式声明为 “multiple”(默认值)，并且你没有在这个Intent中设置FLAG_ACTIVITY_SINGLE_TOP标志，那么它将关闭然后重新创建；对于其它的启动模式，或者在这个Intent中设置**FLAG_ACTIVITY_SINGLE_TOP**标志，都将把这个Intent投递到当前这个实例的**onNewIntent()**中。
&emsp;    这个启动模式还可以与**FLAG_ACTIVITY_NEW_TASK**结合起来使用：用于启动一个Task中的根Activity，它会**把那个Task中任何运行的实例带入前台，然后清除它直到根Activity**。这非常有用，例如，当从Notification Manager处启动一个Activity。

#### FLAG_ACTIVITY_CLEAR_WHEN_TASK_RESET
>API 21，被FLAG_ACTIVITY_NEW_DOCUMENT代替

&emsp;    如果设置，这将在**Task的Activity stack中设置一个还原点**，当Task恢复时，需要清理Activity。也就是说，下一次Task带着 **FLAG_ACTIVITY_RESET_TASK_IF_NEEDED**标记进入前台时（经过测试发现，对于一个处于后台的应用，如果**在launcher中点击应用**，这个动作中含有FLAG_ACTIVITY_RESET_TASK_IF_NEEDED标记；长按Home键，然后点击最近记录，这个动作不含FLAG_ACTIVITY_RESET_TASK_IF_NEEDED标记），**这个Activity和它之上的都将关闭**，以至于用户不能再返回到它们，但是可以回到之前的Activity。
&emsp;    这在你的程序有分割点的时候很有用。例如，一个e-mail应用程序可能有一个操作是查看一个附件，需要启动图片浏览Activity来显示。这个 Activity应该作为e-mail应用程序Task的一部分，因为这是用户在这个Task中触发的操作。然而，当用户离开这个Task，然后从主画面选择e-mail app，我们可能希望回到查看的会话中，但不是查看图片附件，因为这让人困惑。通过在启动图片浏览时设定这个标志，浏览及其它启动的Activity在下次用户返回到mail程序时都将全部清除。

#### FLAG_ACTIVITY_EXCLUDE_FROM_RECENTS
&emsp;    如果设置，新的Activity不会在最近启动的Activity的列表中保存。

#### FLAG_ACTIVITY_FORWARD_RESULT
&emsp;    如果设置，并且这个Intent用于从一个存在的Activity启动一个新的Activity，那么，这个作为答复目标的Activity将会传到这个新的Activity中。这种方式下，**新的Activity可以调用setResult(int)**，并且这个结果值将发送给那个作为答复目标的 Activity。

#### FLAG_ACTIVITY_LAUNCHED_FROM_HISTORY 
&emsp;    这个标志一般不由应用程序代码设置，如果这个Activity是从历史记录里启动的（常按HOME键），那么，系统会帮你设定。

#### FLAG_ACTIVITY_MULTIPLE_TASK 
&emsp;    不要使用这个标志，除非你自己实现了应用程序启动器。与**FLAG_ACTIVITY_NEW_TASK**结合起来使用，可以禁用把已存的Task送入前台的行为。当设置时，**新的Task总是会启动**来处理Intent，而不管这是是否已经有一个Task可以处理相同的事情。
&emsp;    由于默认的系统不包含图形Task管理功能，因此，你不应该使用这个标志，除非你提供给用户一种方式可以返回到已经启动的Task。
&emsp;    如果FLAG_ACTIVITY_NEW_TASK标志没有设置，这个标志被忽略。

#### FLAG_ACTIVITY_NEW_TASK 
&emsp;    如果设置，这个Activity会成为历史stack中一个新Task的开始。一个Task（从启动它的Activity到下一个Task中的 Activity）定义了用户可以迁移的Activity原子组。Task可以移动到前台和后台；在某个特定Task中的所有Activity总是保持相同的次序。
&emsp;    这个标志一般用于呈现“启动”类型的行为：它们提供用户一系列可以单独完成的事情，与启动它们的Activity完全无关。
&emsp;    使用这个标志，如果正在启动的Activity的Task已经在运行的话，那么，新的Activity将不会启动；代替的，当前Task会简单的移入前台，保持栈中的状态不变，即栈中的顺序不变。
&emsp;    这个标志不能用于调用方对已经启动的Activity请求结果。

>设置此状态，记住以下原则，首先会查找**是否存在和被启动的Activity具有相同的亲和性的任务栈**（即taskAffinity，注意同一个应用程序中的activity的亲和性一样，所以下面的a情况会在同一个栈中，前面这句话有点拗口，请多读几遍），**如果有，刚直接把这个栈整体移动到前台**，并保持栈中的状态不变，即栈中的activity顺序不变，**如果没有，则新建一个栈来存放被启动的activity**

**实例**：
a. 前提: Activity A和Activity B在同一个应用中。 
&emsp;    操作: Activity A启动开僻Task堆栈(堆栈状态: A)， 在Activity A中启动Activity B， 启动Activity B的Intent的Flag设为FLAG_ACTIVITY_NEW_TASK
==>Activity B被压入Activity A所在堆栈(堆栈状态: AB)。
&emsp;    原因: 默认情况下同一个应用中的所有Activity拥有相同的关系(taskAffinity)。

b. 前提: Activity A在名称为"TaskOne应用"的应用中， Activity C和Activity D在名称为"TaskTwo应用"的应用中。
&emsp;    操作1: 在Launcher中单击"TaskOne应用"图标， Activity A启动开僻Task堆栈， 命名为TaskA(TaskA堆栈状态: A)，在Activity A中启动Activity C， 启动Activity C的Intent的Flag设为FLAG_ACTIVITY_NEW_TASK，Android系统会为Activity C开僻一个新的Task， 命名为TaskB(TaskB堆栈状态: C)， 长按Home键， 选择TaskA，Activity A回到前台， 再次启动Activity C（两种情况1。从桌面启动；2。从Activity A启动，两种情况一样）， 这时TaskB回到前台， Activity C显示， 供用户使用， 即:
==>包含FLAG_ACTIVITY_NEW_TASK的Intent启动Activity的Task正在运行， 则不会为该Activity创建新的Task，而是将原有的Task返回到前台显示。

&emsp;    操作2: 在Launcher中单击"TaskOne应用"图标， Activity A启动开僻Task堆栈， 命名为TaskA(TaskA堆栈状态: A)，在Activity A中启动Activity C，启动Activity C的Intent的Flag设为FLAG_ACTIVITY_NEW_TASK，Android系统会为Activity C开僻一个新的Task， 命名为TaskB(TaskB堆栈状态: C)，  在Activity C中启动Activity D(TaskB的状态: CD) 长按Home键， 选择TaskA， Activity A回到前台， 再次启动Activity C(从桌面或者ActivityA启动，也是一样的)，这时TaskB回到前台， Activity D显示，供用户使用。
==>说明了在此种情况下设置FLAG_ACTIVITY_NEW_TASK后，会先查找是不是有Activity C存在的栈，根据亲和性(taskAffinity)，如果有，刚直接把这个栈整体移动到前台，并保持栈中的状态不变，即栈中的顺序不变



#### FLAG_ACTIVITY_NO_ANIMATION 
&emsp;    如果在Intent中设置，并传递给Context.startActivity()的话，这个标志将阻止系统进入下一个Activity时应用 Acitivity迁移动画。这并不意味着动画将永不运行——如果另一个Activity在启动显示之前，没有指定这个标志，那么，动画将被应用。这个标志可以很好的用于执行一连串的操作，而动画被看作是更高一级的事件的驱动。

#### FLAG_ACTIVITY_NO_HISTORY 
&emsp;    如果设置，新的Activity将不再历史stack中保留。用户一离开它，这个Activity就关闭了。这也可以通过设置noHistory特性。

#### FLAG_ACTIVITY_NO_USER_ACTION 
&emsp;    如果设置，作为新启动的Activity进入前台时，这个标志将在Activity暂停之前阻止从最前方的Activity回调的onUserLeaveHint()。
&emsp;    典型的，一个Activity可以依赖这个回调指明显式的用户动作引起的Activity移出后台。这个回调在Activity的生命周期中标记一个合适的点，并关闭一些Notification。
&emsp;    如果一个Activity通过非用户驱动的事件，如来电或闹钟，启动的，这个标志也应该传递给Context.startActivity，保证暂停的Activity不认为用户已经知晓其Notification。

#### FLAG_ACTIVITY_PREVIOUS_IS_TOP 
&emsp;    If set and this intent is being used to launch a new activity from an existing one, the current activity will not be counted as the top activity for deciding whether the new intent should be delivered to the top instead of starting a new one. The previous activity will be used as the top, with the assumption being that the current activity will finish itself immediately. 

#### FLAG_ACTIVITY_REORDER_TO_FRONT
&emsp;    如果在Intent中设置，并传递给Context.startActivity()，这个标志将引发**已经运行的Activity移动到历史stack的顶端**。
&emsp;    例如，假设一个Task由四个Activity组成：A,B,C,D。如果D调用startActivity()来启动Activity B，那么，B会移动到历史stack的顶端，现在的次序变成A,C,D,B。如果**FLAG_ACTIVITY_CLEAR_TOP**标志也设置的话，那么**这个标志将被忽略**。

#### FLAG_ACTIVITY_RESET_TASK_IF_NEEDED

&emsp;    If set, and this activity is either being started in a new task or bringing to the top an existing task, then it will be launched as the front door of the task. This will result in the application of any affinities needed to have that task in the proper state (either moving activities to or from it), or simply resetting that task to its initial state if needed. 
一般为系统使用,比如要把一个应用从后台移到前台,有两种方式:从多任务列表中恢复(不包含该flag);从启动器中点击icon恢复(包含该flag);需结合` FLAG_ACTIVITY_CLEAR_WHEN_TASK_RESET | FLAG_ACTIVITY_NEW_DOCUMENT (API21)`理解

#### FLAG_ACTIVITY_SINGLE_TOP
&emsp;    如果设置，当这个Activity**位于**历史stack的**顶端**运行时，**不再启动**一个**新的**。 
注意：如果是从BroadcastReceiver启动一个新的Activity，或者是从Service往一个Activity跳转时，不要忘记添加Intent的Flag为FLAG_ACTIVITY_NEW_TASK。

#### taskAffinity
&emsp;　每个Activity都有taskAffinity属性，这个属性**指出了它希望进入的Task**。如果一个Activity没有显式的指明该 Activity的taskAffinity，那么它的这个属性就等于Application指明的taskAffinity，如果 Application也没有指明，那么该taskAffinity的值就等于包名。而Task也有自己的affinity属性，它的值等于它的根 Activity的taskAffinity的值。

&emsp;　TaskAffinity属性主要和SingleTask启动模式或者allowTaskReparenting属性配对使用，在其他情况下没有意义。当TaskAffinity和singleTask启动模式配对使用的时候，他是具有该模式的Activity的目前任务栈的名字，待启动的Activity会运行在名字和TaskAffinity相同的任务栈中。allowTaskReparenting用于配置是否允许该activity可以更换从属task，通常情况二者连在一起使用，用于实现把一个应用程序的Activity移到另一个应用程序的Task中。allowTaskReparenting用来标记Activity能否从启动的Task移动到taskAffinity指定的Task，默认是继承至application中的allowTaskReparenting=false，如果为true，则表示可以更换；false表示不可以。

&emsp;　如果加载某个Activity的intent，Flag被设置成FLAG_ACTIVITY_NEW_TASK时，它会首先检查是否存在与自己taskAffinity相同的Task，如果存在，那么它会直接宿主到该Task中，如果不存在则重新创建Task。



#### 若是已经启动了四个Activity：A，B，C和D，在D Activity里，想再启动一个Actvity B，但不变成A，B，C，D，B，而是A，C，D，B，则可以像下面写代码：
```
Intent intent = new Intent（this， MainActivity.class）;  
intent.addFlags（Intent.FLAG_ACTIVITY_REORDER_TO_FRONT）;   
startActivity（intent）;
```


# Android Activity面试题
## 什么是 Activity?
见上文#[什么是 Activity?](#什么是 Activity?)

## 描述一下 Activity 生命周期
Activity 从创建到销毁有多种状态，从一种状态到另一种状态时会激发相应的回调方法，这些回调方法包括：
onCreate onStart onResume onPause onStop onDestroy
其实这些方法都是两两对应的，onCreate 创建与 onDestroy 销毁；
onStart 可见与 onStop 不可见；onResume 可编辑（即焦点）与 onPause；

这 6 个方法是相对应的，那么就只剩下一个 onRestart 方法了，这个方法在什么时候调用呢？
答案就是：在 Activity 被 onStop 后，但是没有被 onDestroy，在再次启动此 Activity 时就调用 onRestart（而不再调用 onCreate）方法；如果被 onDestroy 了，则是调用 onCreate 方法。

更多见上文#[Activity 生命周期](#Activity 生命周期)

## 横竖屏切换时 Activity 的生命周期
见上文#[横竖屏切换时 Activity 的生命周期](#横竖屏切换时 Activity 的生命周期)

## 两个 Activity 之间跳转时必然会执行的是哪几个方法？（重要）
一般情况下比如说有两个 activity,分别叫 A,B,当在 A 里面激活 B 组件的时候, A 会调用 onPause()方法,然后 B 调
用 onCreate() ,onStart(), onResume()。
这个时候 B 覆盖了窗体, A 会调用 onStop()方法. 如果 B 是个透明的,或者是对话框的样式, 就不会调用 A 的
onStop()方法。
第一次进入ActivityA：
A | onCreate 
A | onStart 
A | onResume 

ActivityA启动ActivityB（普通Activity）：
A | onPause 
B | onCreate 
B | onStart 
B | onResume 
A | onStop

ActivityA启动ActivityB（对话框Activity）：
A | onPause 
B | onCreate 
B | onStart 
B | onResume 

更多情况见上文#[各种情况下函数调用过程](#各种情况下函数调用过程)

## Activity 的状态都有哪些？
a) foreground activity
b) visible activity
c) background activity
d) empty process

## 如何保存 Activity 状态
见上文#[如何保存 Activity 状态](#如何保存 Activity 状态)

## 请描述一下 Activity 的启动模式都有哪些以及各自的特点
见上文#[Activity 启动模式](#Activity 启动模式)

## 将一个 Activity 设置成窗口的样式：
只需要给我们的 Activity 配置如下属性即可。
注意：当前Activity继承自Activity，而不是AppCompatActivity，否则会报主题的错误。
```
android:theme="@android:style/Theme.Dialog"
```



## 如何退出 Activity？如何安全退出已调用多个 Activity 的 Application？
方法1、通常情况用户退出一个 Activity 只需按返回键，我们写代码想退出 activity 直接调用 finish()方法就行。

方法2、**application 记录打开的 Activity**：
每打开一个 Activity，就记录下来。在需要退出时，关闭每一个 Activity 即可。
```
private List<Activity> mActList = new LinkedList<>();// 在 application 全局的变量里面
    public void addActivity(Activity activity) {
        if (mActList.contains(activity)) {
            mActList.remove(activity);
        }
        mActList.add(activity);
    }

    public void exit() {
        try {
            for (Activity activity : mActList) {
                if (activity != null)
                    activity.finish();
            }
        } catch (Exception e) {
            e.printStackTrace();
        } finally {
            System.exit(0);
        }
    }
```

方法3、**发送特定广播**：
在需要结束应用时，发送一个特定的广播，每个 Activity 收到广播后，关闭即可。
```
//给每个 activity 注册接受接受广播的意图

registerReceiver(receiver, filter)

//如果过接受到的是 关闭 activity 的广播   就调用 finish()方法 把当前的 activity finish()掉
```
或者eventBus
```
//发送方
EventBus.getDefault().post(map);

//接收方
    @Override
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
        EventBus.getDefault().register(this);
        ...
    }

    @Override
    public void onDestroyView() {
        EventBus.getDefault().unregister(this);
        super.onDestroyView();
    }

    public void onEventMainThread(HashMap<String, String> map) {
      finish();
    }
```
方法4、**递归退出**
在打开新的 Activity 时使用 startActivityForResult，然后自己加标志，在 **onActivityResult** 中处理，递归关闭。

方法5、其实 也可以通过 intent 的 flag 来实现 intent.setFlags(Intent.FLAG_ACTIVITY_CLEAR_TOP)激活 一个新的 activity。此时如果该任务栈中已经有该 Activity，那么系统会把这个 Activity 上面的所有 Activity 干掉。其实相当于给 Activity 配置的启动模式为 SingleTop。




## 两个 Activity 之间传递数据，有哪些方式？(2017-2-23) 
###  1、通过intent传递数据
（1）直接传递，intent.putExtra(key, value)
（2）通过bundle，intent.putExtras(bundle);

注意：
（1）这两种都要求传递的对象必须可序列化（[Parcel](http://developer.android.com/reference/android/os/Parcel.html)able、Serializable）
（2）Parcelable实现相对复杂
（3）关于Parcelable和Serializable，官方说法：
```
`Serializable`: it's error prone and horribly slow. So in general: stay away from `Serializable` if possible.
```
也就是说**和Parcelable相比**，**Seriaizable容易出错并且速度相当慢**。是否这样，可参见[下一篇博客](http://blog.csdn.net/rflyee/article/details/47441405)说明。

（4）通过intent传递数据是有大小限制滴，超过限制，要么抛异常，要么新的Activity启动失败，所以还是很严重的啊，可参见[下一篇博客](http://blog.csdn.net/rflyee/article/details/47441405)说明。

#### 1）intent.putExtra(key, value)
```java
// 传递
 Intent intent = new Intent(this,TwoActivity.class);
 intent.putExtra("data",str);
 startActivity(intent);
```
```java
// 接收
 Intent intent = getIntent();
 String str = intent.getStringExtra("data");
```

##### 序列化对象Seriazable
首先创建一个Person类，让Person**类实现Serializable接口**。Person类中有name，sex，age三个属性，实现Person类的 get 、set 方法。（！注意:该对象的类必需实现Serializable接口）

把要传递到下一个Activity的数据存放在Person类的对象中，启动Activity把数据传递出去    

```java
//序列化需要传递的对象
public class Personimplements Serializable {
...
```

```java
//传递
Intent i = new Intent();
Person p = new Person("钱多多","男",22); 
i.putExtra("key", p); //向Intent对象中存入Person对象P  
startActivity(i);  
```

```java
//接收
Intent i = getIntent();
Serializable p = i.getSerializableExtra("key"); //反序列化数据对象  
if (p instanceof Person) {   
	Person person = (Person) p; //获取到携带数据的Person对象p
}
```
##### Parcelable（最常用，传递数据效率更高）
实现Parcelable步骤
① implements **Parcelable**
② 重写**writeToParcel**方法，将你的对象序列化为一个Parcel对象，即：将类的数据写入外部提供的Parcel中，打包需要传递的数据到Parcel容器保存，以便从 Parcel容器获取数据
③ 重写**describeContents**方法，内容接口描述，默认返回0就可以
④ 实例化**静态内部对象CREATOR**实现接口Parcelable.Creator
```java
public static final Parcelable.Creator<T> CREATOR
```
注：其中public static final一个都不能少，内部对象CREATOR的名称也不能改变，必须全部大写。需重写本接口中的两个方法：createFromParcel(Parcel in) 实现从Parcel容器中读取传递数据值，封装成Parcelable对象返回逻辑层，newArray(int size) 创建一个类型为T，长度为size的数组，仅一句话即可（return new T[size]），供外部类反序列化本类数组使用。

简而言之：
       **通过writeToParcel将你的对象映射成Parcel对象，再通过createFromParcel将Parcel对象映射成你的对象**。也可以将Parcel看成是一个流，通过writeToParcel把对象写到流里面，在通过createFromParcel从流里读取对象，只不过这个过程需要你来实现，因此写的顺序和读的顺序必须一致。

```java
//序列化信息类
public class InfoBean implements Parcelable {//① implements Parcelable

    public InfoBean(String name) {//构造方法
        description = name;
    }

    public String description;//描述 -- 取一个字段（信息类InfoBean的一个属性）

// ② 重写writeToParcel方法，通过writeToParcel将你的对象映射成Parcel对象
    @Override
    public void writeToParcel(Parcel dest, int flags) {
        //通过writeToParcel把对象写到流里面
        dest.writeString(description);
    }
//③ 重写describeContents方法，内容接口描述，默认返回0就可以
    @Override
    public int describeContents() {
        return 0;
    }
//④ 实例化静态内部对象CREATOR实现接口Parcelable.Creator，通过createFromParcel将Parcel对象映射成你的对象
    public static final Parcelable.Creator<InfoBean> CREATOR
            = new Parcelable.Creator<InfoBean>() {
        @Override
        public InfoBean createFromParcel(Parcel source) {
            //再通过createFromParcel从流里读取对象
            String description = source.readString();
            //把对象保存在数据类对象的构造参数中，返回包含有数据的对象
            return new InfoBean(description);
        }
        @Override
        public InfoBean[] newArray(int size) {
            return new InfoBean[size];
        }
    };
}
```


#### 2）intent.putExtras(bundle)
```java
// 传递
 Intent intent = new Intent(MainActivity.this,TwoActivity.class);
 //用数据捆传递数据
 Bundle bundle = new Bundle();
 bundle.putString("data", str);
 //把数据捆设置改意图
 intent.putExtra("bun", bundle);
 startActivity(intent);
```
```java
// 接收
//获取Bundle
 Intent intent = getIntent();
 Bundle bundle = intent.getBundleExtra("bun");
 String str = bundle.getString("data");
```
### 2、广播接收者

### 3、content provider

### 4、Application共享数据
将数据保存到全局Application中，随整个应用的存在而存在，这样很多地方都能访问。
```java
//创建一个Application的类 ：MyApplication继承Application
```
```java
//传值
MyApplication application = (MyApplication) MainActivity.this.getApplication();
application.setData("abc");
```
```java
//取值
MyApplication application = (MyApplication) getApplication();
String data = application.getData();
```

**注意**：
  当由于某些原因（比如系统内存不足），我们的app会被系统强制杀死，此时再次点击进入应用时，系统会直接进入被杀死前的那个界面，制造一种从来没有被杀死的假象。那么问题来了，系统强制停止了应用，进程死了，那么再次启动时Application自然新的，那里边的数据自然木有啦，如果直接使用很可能报空指针或者其他错误。

  因此还是要考虑好这种情况的：
  （1）使用时一定要做好非空判断
  （2）如果数据为空，可以考虑逻辑上让应用直接返回到最初的activity，比如用 `FLAG_ACTIVITY_CLEAR_TASK` 或者 `BroadcastReceiver 杀掉其他的activity。`

### 5、使用单例

比如一种常见的写法：
```
public class DataHolder {
  private String data;
  public String getData() {return data;}
  public void setData(String data) {this.data = data;}
  private static final DataHolder holder = new DataHolder();
  public static DataHolder getInstance() {return holder;}
}
```

这样在启动activity之前：

```
DataHolder.getInstance().setData(data);
```

新的activity中获取数据：

```
String data = DataHolder.getInstance().getData();
```

### 6、静态Statis

这个可以直接在activity中也可以单独一个数据结构体，就和单例差不多了。
#### 1）数据结构体中
注意：这些情况如果数据很大很多，比如bitmap，处理不当是很容易导致内存泄露或者内存溢出的。

所以可以考虑使用WeakReferences 将数据包装起来。

比如：
```java
public class DataHolder {
  Map<String, WeakReference<Object>> data = new HashMap<String, WeakReference<Object>>();

  void save(String id, Object object) {
    data.put(id, new WeakReference<Object>(object));
  }

  Object retrieve(String id) {
    WeakReference<Object> objectWeakReference = data.get(id);
    return objectWeakReference.get();
  }
}
```

启动之前：

```
DataHolder.getInstance().save(someId, someObject);
```

新activity中：

```
DataHolder.getInstance().retrieve(someId);
```

这里可能需要通过intent传递id，如果数据唯一，id都可以不传递的。save() retrieve()中id都固定即可。

#### 2）activity中
在接收端的Avtivity里面设置static的变量，在发送端这边改变静态变量的值，然后启动意图

```java
//发送端
//定义一个意图  
Intent intent = new Intent(MainActivity.this,OtherActivity.class);
//改变OtherActivity的三个静态变量的值  
OtherActivity.name = "wulianghuan";
OtherActivity.age = "22";
OtherActivity.address = "上海闵行";
startActivity(intent);
```

```java
//接收端
public class OtherActivity extends Activity {  
    //定义静态变量
    public static String name;  
    public static String age;  
    public static String address;  
}  
```



### 7、持久化数据
例如：File 文件存储、SharedPreferences 首选项、Sqlite 数据库
优点：
（1）应用中所有地方都可以访问
（2）即使应用被强杀也不是问题了
缺点：
（1）操作麻烦
（2）效率低下
（3）io读写嘛，其实还是比较容易出错的

### 8、如果不跨进程，可以使用 EventBus

### 9、startActivityForResult
通过onActivityResult得到Activity的回传值


### 10、使用剪切板传递数据
#### 传递String 
主要**步骤**：
通过getSystemService获取ClipboardManager对象cm。
使用cm.setPrimaryClip()方法设置ClipData数据对象。
在新Activity中获取ClipboardManager对象cm。
使用cm.getPrimaryClip()方法获取剪切板的ClipData数据对象，cd。
通过cd.getItemAt(0)获取到传递进来的数据。
```java
//传递数据
//获取剪切板
ClipboardManager cm = (ClipboardManager) getSystemService(Context.CLIPBOARD_SERVICE);
cm.setPrimaryClip(ClipData.newPlainText("data", "Jack"));
Intent intent = new Intent(MainActivity.this, otherActivity.class);
startActivity(intent);
```

```java
//获取数据：
ClipboardManager cm = (ClipboardManager)getSystemService(Context.CLIPBOARD_SERVICE);
ClipData cd=cm.getPrimaryClip();
String msg=cd.getItemAt(0).getText().toString();
```

#### 传递对象
　　以上方式使用剪切板传递的为String类型的数据，如果需要传递一个对象，那么被传递的对象必须可**序列化**，序列化通过实现Serializable接口来标记。

主要**步骤**：
创建一个实现了Serializable接口的类MyData。
存入数据：获取ClipboardManager，并对通过Base64类对MyData对象进行序列化，再存入剪切板中。
取出数据：在新Activity中，获取ClipboardManager，对被序列化的数据进行反序列化，同样使用Base64类。然后对反序列化的数据进行处理。
示例代码：
```java
//序列化需要传递的对象
public class MyData implements Serializable {
    private String name;
    private int age;
    public MyData(String name, int age) {
        super();
        this.name = name;
        this.age = age;
    }
    public String getName() {
        return name;
    }
    public void setName(String name) {
        this.name = name;
    }
    public int getAge() {
        return age;
    }
    public void setAge(int age) {
        this.age = age;
    }
}
```
```java
/* 传递对象 */
MyData mydata = new MyData("jack", 24);
String baseToString = "";
ByteArrayOutputStream bArr = new ByteArrayOutputStream();
try {
    ObjectOutputStream oos = new ObjectOutputStream(bArr);
    oos.writeObject(mydata);
    baseToString = Base64.encodeToString(bArr.toByteArray(), Base64.DEFAULT);
    oos.close();
} catch (Exception e) {
    e.printStackTrace();

}
//获取剪切板
ClipboardManager cm = (ClipboardManager) getSystemService(Context.CLIPBOARD_SERVICE);
cm.setPrimaryClip(ClipData.newPlainText("data", baseToString));
Intent intent = new Intent(MainActivity.this, otherActivity.class);
startActivity(intent);
```
```java
/* 获取对象 */
ClipboardManager cm = (ClipboardManager) getSystemService(Context.CLIPBOARD_SERVICE);
ClipData cd = cm.getPrimaryClip();
String msg = cd.getItemAt(0).getText().toString();
byte[] base64_btye = Base64.decode(msg, Base64.DEFAULT);
ByteArrayInputStream bais = new ByteArrayInputStream(base64_btye);
try {
    ObjectInputStream ois = new ObjectInputStream(bais);
    MyData mydata = (MyData) ois.readObject();

    TextView tv = (TextView) findViewById(R.id.msg);
    tv.setText(mydata.toString());
} catch (Exception e) {
    // TODO Auto-generated catch block
    e.printStackTrace();
}
```

注意：
　　使用剪切板传递数据有利有弊，剪切板为Android系统管理的，所以在一个地方存入的数据，在这个Android设备上任何应用都可以访问的到，但是正是因为此设备访问的都是同一个剪切板，可能会导致当前程序存入的数据，在使用前被其他程序覆盖掉了，导致无法保证正确获取数据。



### 
## 怎样在两个 Activity 之间传递一张图片（2017-2-23）


a)     Intent 可以传递基本数据类型、Uri 和序列化对象 
b)    Bitmap 对象实现了 Parcelable 序列化接口，但是不建议放到 Intent 里传递。因为 Pacelable 对象序列化过 程是将对象 A 的属性暂存一份到内存里，反序列化时再使用暂存的数据，创建一个属性完全相同的对象 B。
c)     对于 Bitmap 而言，就是把图片的二进制数据 0 复制一份到内存里构成二进制数据 1，反序列化时根据二进 制数据 1 创建 Bitmap 对象，此时会生成二进制数据 2。也就是同一个图片的数据在内存里存放了三份。
d)     也就是说把 Bitmap 放到 Intent 里会导致巨大的内存损耗，所以在**传递图片时应该是传递 URI 地址**，新界面根据 URI 生成新图片。同时还可以到图片缓存里使用 URI 查找已有图片，节约内存。

 

## 如何实现切换主题功能？（2017-2-23） 
### 1.内置主题

#### 1.1定义属性
确认哪些属性是需要根据主题变化而改变的，在values文件夹下，新建attrs.xml文件，在attrs.xml里自定义：
```xml
<?xml version="1.0" encoding="utf-8"?>  
<resources>  
    <attr name="colorValue" format="color" />  
    <attr name="floatValue" format="float" />  
    <attr name="integerValue" format="integer" />  
    <attr name="booleanValue" format="boolean" />  
    <attr name="dimensionValue" format="dimension" />  
    <attr name="stringValue" format="string" />  
    <attr name="referenceValue" format="reference" />  
</resources>  
```

#### 1.2定义主题
接着，我们需要在资源文件中定义若干套主题。并且在主题中设置各个属性的值。

本例中，我在styles.xml里定义了SwitchTheme1与SwitchTheme2。

```xml
<style name="SwitchTheme1" parent="@android:style/Theme.Black">  
    <item name="colorValue">#FF00FF00</item>  
    <item name="floatValue">0.35</item>  
    <item name="integerValue">33</item>  
    <item name="booleanValue">true</item>  
    <item name="dimensionValue">76dp</item>  
    <!-- 如果string类型不是填的引用而是直接放一个字符串,在布局文件中使用正常,但代码里获取的就有问题 -->  
    <item name="stringValue">@string/hello_world</item>  
    <item name="referenceValue">@drawable/hand</item>  
</style>  
<style name="SwitchTheme2" parent="@android:style/Theme.Wallpaper">  
    <item name="colorValue">#FFFFFF00</item>  
    <item name="floatValue">1.44</item>  
    <item name="integerValue">55</item>  
    <item name="booleanValue">false</item>  
    <item name="dimensionValue">76px</item>  
    <item name="stringValue">@string/action_settings</item>  
    <item name="referenceValue">@drawable/ic_launcher</item>  
</style>  
```

#### 1.3在布局文件中使用

定义好了属性，我们接下来就要在布局文件中使用了。

为了使用主题中的属性来配置界面，我定义了一个名为activity_theme_switch.xml布局文件。

```xml
<?xml version="1.0" encoding="utf-8"?>  
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent" >  
    <TextView  
        android:id="@+id/textView1"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:layout_alignParentLeft="true"  
        android:layout_alignParentTop="true"  
        android:text="@string/theme_text" />  
    <TextView  
        android:id="@+id/textView3"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:layout_alignParentLeft="true"  
        android:layout_below="@+id/textView1"  
        android:text="@string/theme_color" />        
    <TextView  
        android:id="@+id/themeColor"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:layout_alignLeft="@+id/themeText"  
        android:layout_below="@+id/themeText"  
        android:text="TextView"  
        android:textColor="?attr/colorValue" />
        <!-- ?attr/colorValue 引用主题中的颜色值-->    
    <TextView  
        android:id="@+id/textView5"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:layout_alignParentLeft="true"  
        android:layout_alignTop="@+id/theme_image"  
        android:text="@string/theme_image" />  
    <TextView  
        android:id="@+id/themeText"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:layout_alignParentTop="true"  
        android:layout_centerHorizontal="true"  
        android:text="?attr/stringValue" />  
    <ImageView  
        android:id="@+id/theme_image"  
        android:layout_width="wrap_content"  
        android:layout_height="wrap_content"  
        android:layout_alignLeft="@+id/themeColor"  
        android:layout_below="@+id/themeColor"  
        android:src="?attr/referenceValue" />  
</RelativeLayout>  
```

 从这个布局文件中可以看到，我在id为themeColor、themeText及theme_image的控件上，分别使用了?attr/colorValue、?attr/stringValue与?attr/referenceValue来引用主题中的颜色值、字符串以及图片。

#### 1.4设置主题
关键代码：
**`context.setTheme`** (int styleResid)

>**setTheme**
>Added in API level 1
>void setTheme (int resid)
>Set the base theme for this context. Note that this should be called before any views are instantiated in the Context (for example before calling setContentView(View) or inflate(int, ViewGroup)).
>Parameters
>resid
>int: The style resource describing the theme.

 布局文件与主题都写好了，接下来我们就要在Activity的onCreate方法里使用了

```java
public class DemoStyleThemeActivity extends Activity {
    @Override  
    protected void onCreate(Bundle savedInstanceState) {  
        super.onCreate(savedInstanceState); 
        setTheme(R.style.SwitchTheme1); 
        //setTheme一定要在setContentView之前被调用
        setContentView(R.layout.activity_theme_switch);  
    }  
}  
```

 当然，要想主题生效，有一点非常重要：“**setTheme一定要在setContentView之前被调用**”。要不然，界面都解析完了，再设置主题也不会触发重新创建界面。

更严重的是，要是默认主题里没那些属性，解析布局文件时候是会挂的啊！这点在配置多个不同style时要主题，属性可以多，但一定不能少。 

上面几步只是在 Activity 使用自己的主题，实现动态切换主题的思路是把 setTheme 里面的主题作为一个变量，当需要切换主题时，改变这个变量的值，然后重新创建当前 Activity。 

#### 1.5切换主题 
① 在自定义 Application 中记录当前主题
```java
public class MyApp extends Application {
    private int currentTheme = R.style.SwitchTheme1;
    public void changeTheme(int theme) {
        this.currentTheme = theme;
    }
    public void initTheme(Context context) {
        context.setTheme(currentTheme);
    }
}
```
② Activity 的 oncreated 方法首先设置主题
```java
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    myApp = (MyApp) getApplication();
    myApp.initTheme(this);
    setContentView(R.layout.activity_main);
}
```
③ 当想要切换主题时，先替换 MyApp 中的当前主题，然后重新创建当前 Activity，新的 Activity 创建后就使用了改变后的主题
```java
myApp.changeTheme(R.style.BlueTheme);
recreate();//该方法是 Activity 中的方法，可以让当前 Activity 重新创建实例
```
因为setTheme()方法必须要在setContentView()方法之前调用，所以为了使当前Activity的主题切换成功，需要调用recreate()方法来重新调用onCreate()方法。这样也导致了当前Activity被销毁，并重新启动，所以会出现闪屏的现象。


### 2.APK主题
**上面**虽然可以切换 Activity 的主题，但是由于每次都需要杀死当前 Activity 重新创建新的 Activity，因此用户体验并不好。 上面的**更改主题方案，属于内置主题**，该方案虽然可以很方便地修改界面，并且不需要怎么改代码。但它有一个比较致命的缺陷，就是一旦程序发布后，应用所支持的主题风格就固定了。要想增加更多的效果，只能发布新的版本。 

如何实现**通过网络下载更换主题**呢？下面给大家提供一种思路。
为了解决这种问题，便有了 **APK 主题方案**。

APK 主题方案的基本思路是：在 Android 中，所有的资源都是基于包的。资源以 id 进
行标识，在同一个应用中，每个资源都有唯一标识。但在不同的应用中，可以有相同的 id。因此，只要获取到了其他应用的 Context 对象，就可以通过它的 getRsources 获取到其绑定的资源对象。然后，就可以使用 Resources 的 getXXX 方法获取字符串、颜色、dimension、图片等。

要 想 获 取 其 他 应 用 的  Context 对 象 ， Android  已 经 为 我 们 提 供 好 了 接 口 。 那 就 是`android.content.ContextWrapper.createPackageContext(String packageName, int flags)`方法。
示例代码如下：
```java
public class DemoRemoteThemeActivity extends Activity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_theme);
        TextView text = (TextView) findViewById(R.id.remoteText);
        TextView color = (TextView) findViewById(R.id.remoteColor);
        ImageView image = (ImageView) findViewById(R.id.remote_image);
        try {
	        //① 通过 createPackageContext 获取到了包名为 com.xxx.themepackage 的应用的上下文
            String remotePackage = "com.xxx.themepackage";
            Context remoteContext = 
			            createPackageContext(remotePackage, CONTEXT_IGNORE_SECURITY);
            //② 通过 Resources 的getIdentifier 方法获取到相应资源名在该应用中的 id
            //③ 通过 Resources 的getXXX 方法获取资源
            Resources remoteResources = remoteContext.getResources();
            text.setText(remoteResources.getText(
			            remoteResources.getIdentifier("application_name", "string", remotePackage)));
            color.setTextColor(remoteResources.getColor(
			            remoteResources.getIdentifier("delete_target_hover_tint", "color", remotePackage)));
            image.setImageDrawable(remoteResources.getDrawable(
			            remoteResources.getIdentifier("ic_launcher_home", "drawable", remotePackage)));
        } catch (PackageManager.NameNotFoundException e) {
            e.printStackTrace();
        }
    }
}
```

首先，我通过 createPackageContext 获取到了包名为 com.xxx.themepackage 的应用的上下文。
然后，通过 Resources 的getIdentifier 方法获取到相应资源名在该应用中的 id。当然，有的人也可以通过使用自身应用的 id 的方式，不过这有一个前提，那就是同名资源在主题包应用与当前应用中的 id 相同。这貌似可以通过修改 编译流程来实现，就像 framework 里的 public.xml 那样。不过这不在本文的讨论范畴内。
最后，就是通过 Resources 的getXXX 方法获取资源了。


## Android 中 Activity 是如何启动的？（2017-2-24）

 Activity 启动时的概要交互流程如下图：
[![流程图](http://img.blog.csdn.net/20180126172405287?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)](http://img.blog.csdn.net/20180126172405287?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

用户在 Launcher 程序里点击应用图标时，会通知 ActivityManagerService 启动应用的入口 Activity，ActivityManagerService 发现这个应用还未启动，则会通知 Zygote 进程孵化出应用进程，然后在这个 dalvik 应用进程里执行 ActivityThread 的 main 方法。在该方法里会先准备好 Looper 和消息队列，然后调用 attach 方法将应用进程绑定到 ActivityManagerService，然后进入 loop 循环，不断地读取消息队列里的消息，并分发消息。

```java
    //ActivityThread 类public static void main(String[] args) {
        //...
        Looper.prepareMainLooper();
        ActivityThread thread = new ActivityThread();
        thread.attach(false);
        if (sMainThreadHandler == null) {
            sMainThreadHandler = thread.getHandler();
        }
        AsyncTask.init();
        //...
        Looper.loop();
        //...}
```

在 ActivityThread 的 main 方法里调用 thread.attach(false);attach 方法的主要代码如下所示:

```java
//ActivityThread 类 private void attach(boolean system) {
sThreadLocal.set(this);
mSystemThread = system;
if (!system) {
    //...
    IActivityManager mgr = ActivityManagerNative.getDefault();
    try {
        //调用 ActivityManagerService 的 attachApplication 方法
        //将 ApplicationThread 对象绑定至 ActivityManagerService，
        //这样 ActivityManagerService 就可以
        //通过 ApplicationThread 代理对象控制应用进程
        //调用的是 ActivityManagerService 的 attachApplication
        mgr.attachApplication(mAppThread);
    } catch (RemoteException ex) {
        // Ignore
    }
} else {
    //...
}
//... }
```

ActivityManagerService 的 attachApplication 方法执行 attachApplicationLocked(thread, callingPid)进行绑定 。 attachApplicationLocked 方 法 有 两 个 重 要 的 函 数 调 用 thread.bindApplication 和mMainStack.realStartActivityLocked。thread.bindApplication 将应用进程的 ApplicationThread 对象绑定到ActivityManagerService ， 也 就 是 说 获 得 ApplicationThread 对 象 的 代 理 对 象 。
mMainStack.realStartActivityLocked 通知应用进程启动 Activity。

```java
//ActivityManagerService 类
private final boolean attachApplicationLocked(IApplicationThread thread,int pid) {
    ProcessRecord app;
    //...
    app.thread = thread;
    //...
    try {
        //...
        //thread 对象其实是 ActivityThread 里 ApplicationThread
        //对象在 ActivityManagerService 的代理对象，故此执行
        //thread.bindApplication，最终会调用 ApplicationThread 的 bindApplication 方法
        //最后会调用 queueOrSendMessage 会往 ActivityThread 的消息队列发送消息，
        //消息的用途是 BIND_APPLICATION 这样会在 handler 里处理 BIND_APPLICATION 消息，
        //接着调用 handleBindApplication 方法处理绑定消息。
        thread.bindApplication(processName, appInfo, providers,
                app.instrumentationClass, profileFile, profileFd, profileAutoStop,
                app.instrumentationArguments, app.instrumentationWatcher, testMode,
                enableOpenGlTrace, isRestrictedBackupMode || !normalMode, app.persistent,
                new Configuration(mConfiguration), app.compat, getCommonServicesLocked(),
                mCoreSettingsObserver.getCoreSettingsLocked());
        //...
    } catch (Exception e) {
        //...
    }
    //...
    ActivityRecord hr = mMainStack.topRunningActivityLocked(null);
    if (hr != null && normalMode) {
        if (hr.app == null && app.uid == hr.info.applicationInfo.uid
                && processName.equals(hr.processName)) {
            try {
                if (mHeadless) {
                    Slog.e(TAG, "Starting activities not supported on headless device: " + hr);
                    // realStartActivity 会调用 scheduleLaunchActivity 启动 activity
                    //最终会调用 ApplicationThread 的 scheduleLaunchActivity 方法。
                    //调用了 queueOrSendMessage 往 ActivityThread 的消息队列发送了消息
                    //，消息的用途是启动 Activity
                } else if (mMainStack.realStartActivityLocked(hr, app, true, true)) {
                    //mMainStack.realStartActivityLocked 真正启动 activity
                    didSomething = true;
                }
            } catch (Exception e) {
                //...
            }
        } else {//...} }//... Return true；} 
```


综上时序图为：

[![时序图](http://img.blog.csdn.net/20180126172358913?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)](http://img.blog.csdn.net/20180126172358913?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

ActivityThread 的 handler 调用 handleLaunchActivity 处理启动 Activity 的消息，handleLaunchActivity 的主要代码如下所示:

```java
//ActivityThread 类
private void handleLaunchActivity(ActivityClientRecord r, Intent customIntent) {
    //...
    Activity a = performLaunchActivity(r, customIntent);
    if (a != null) {
        //...
        handleResumeActivity(r.token, false, r.isForward,
                !r.activity.mFinished && !r.startsNotResumed);
        //...
    } else {
        //...
    }
}
```

handleLaunchActivity 方 法 里 有 两 个 重 要 的 函 数 调 用 ,performLaunchActivity 和
handleResumeActivity,performLaunchActivity 会调用 Activity 的onCreate,onStart,onResotreInstanceState 方法,handleResumeActivity 会调用 Activity 的 onResume 方法.代码如下：
performLaunchActivity 的主要代码如下所示:

```java
//ActivityThread 类
private Activity performLaunchActivity(ActivityClientRecord r, Intent customIntent) {
    //...
    //会调用 Activity 的 onCreate 方法
    mInstrumentation.callActivityOnCreate(activity, r.state);
    //...
    //...
    //调用 Activity 的 onStart 方法
    if (!r.activity.mFinished) {
        activity.performStart();
        r.stopped = false;
    }
    if (!r.activity.mFinished) {
        if (r.state != null) {
            //会调用 Activity 的 onRestoreInstanceState 方法
            mInstrumentation.callActivityOnRestoreInstanceState(activity, r.state);
        }
        //....
    }
```


启动 Activity 主要涉及到的类：
[![启动Activity主要涉及的类](http://img.blog.csdn.net/20180126172351292?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)](http://img.blog.csdn.net/20180126172351292?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

Activity 的管理采用 binder 机制，管理 Activity 的接口是 IActivityManager. ActivityManagerService 实现了Activity 管理功能,位于 system_server 进程，ActivityManagerProxy 对象是 ActivityManagerService 在普通应用进程的一个代理对象，应用进程通过 ActivityManagerProxy 对象调用 ActivityManagerService 提供的功能。应用进程并不会直接创建 ActivityManagerProxy 对象，而是通过调用 ActiviyManagerNative 类的工具方法 getDefault 方法得到 ActivityManagerProxy 对象。所以在应用进程里通常这样启动 Activty。

```java
 ActivityManagerNative.getDefault().startActivity()
```

ActivityManagerService 也需要主动调用应用进程以控制应用进程并完成指定操作。这样
ActivityManagerService 也需要应用进程的一个 Binder 代理对象，而这个代理对象就是ApplicationThreadProxy对象。 ActivityManagerService 通过 IApplicationThread 接口管理应用进程，ApplicationThread 类实现了IApplicationThread 接口，实现了管理应用的操作，ApplicationThread 对象运行在应用进程里。
ApplicationThreadProxy 对象是 ApplicationThread 对象在 ActivityManagerService 线程
(ActivityManagerService 线程运行在 system_server 进程)内的代理对象，ActivityManagerService 通过ApplicationThreadProxy 对象调用 ApplicationThread 提供的功能，比如让应用进程启动某个 Activity。
public static void main(String[] args) {

```java
Process.setArgV0("<pre-initialized>");
//创建 looper
Looper.prepareMainLooper();
//会通过 thread.attach(false)来初始化应用程序的运行环境
ActivityThread thread = new ActivityThread();
thread.attach(false);
```

在建立消息循环之前，会通过 thread.attach(false)来初始化应用程序的运行环境，并建立 activityThread 和ActivityManagerService 之间的桥 mAppThread， mAppThread 是 IApplicationThread 的一个实例。

```java
RuntimeInit.setApplicationObject(mAppThread.asBinder());
final IActivityManager mgr = ActivityManagerNative.getDefault();
try {
    mgr.attachApplication(mAppThread);
} catch (RemoteException ex) {
    throw ex.rethrowFromSystemServer();
}
```

注意：每个应用程序对应着一个 ActivityThread 实例，应用程序由 ActivityThread.main 打开消息循环。每个应用程序同时也对应着一个 ApplicationThread 对象。该对象是 activityThread 和 ActivityManagerService 之间的桥梁。在 attach 中还做了一件事情，就是通过代理调用 attachApplication，并利用 binder 的 transact 机制，在 ActivityManagerService 中建立了 ProcessRecord 信息。 

[![ProcessRecord](http://img.blog.csdn.net/20180126172344447?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)](http://img.blog.csdn.net/20180126172344447?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)




ActivityManagerService:

在 ActivityManagerService 中，也有一个用来管理 activity 的地方：mHistory 栈，这个 mHistory 栈里存放的 是服务端的 activity 记录 HistoryActivity（class HistoryRecord extendsIApplicationToken.Stub）。处于栈顶的就是当前running 状态的 activity。
我们来看一下 Activity 的 startActivity 方法的请求过程：
[![startActivity请求过程](http://img.blog.csdn.net/20180126172327410?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)](http://img.blog.csdn.net/20180126172327410?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
从该时序图中可以看出，Activity.startActivity()方法最终是通过代理类和 Binder 机制，在ActivityManagerService.startActivity 方法中执行的。那么在 ActivityManagerService 的 startActivity 中，主要做了那些事情？我们来看下里面比较重要的代码段：
根据 activity、ProcessRecord 等信息创建 HistoryRecord 实例 r

```java
HistoryRecord r = new HistoryRecord(
        this, callerApp, callingUid,intent, resolvedType, aInfo,
        mConfiguration,resultRecord, resultWho, requestCode, componentSpecified);

//把 r 加入到 mHistory 中。
mHistory.add(addPos, r);

//activity 被加入到 mHistory 之后，只是说明在服务端可以找到该 activity 记录了，但是在客户端目前还没有该 activity记录。还需要通过 ProcessRecord 中的 thread（IApplication）变量，调用它的 scheduleLaunchActivity 方法在ActivityThread 中创建新的 ActivityRecord 记录（之前我们说过，客户端的 activity 是用 ActivityRecord 记录的，并放在 mActivities 中）。
app.thread.scheduleLaunchActivity(
        new Intent(r.intent), r,System.identityHashCode(r),
        r.info, r.icicle, results, newIntents, !andResume, isNextTransitionForward());
```

在这个里面主要是根据服务端返回回来的信息创建客户端 activity 记录 ActivityRecord. 并通过 Handler 发送消息到消息队列，进入消息循环。在 ActivityThread.handleMessage()中处理消息。最终在 handleLaunchActivity 方法中把 ActivityRecord 记录加入到 mActivities（mActivities.put(r.token,r)）中，并启动 activity。

总结：
1）在客户端和服务端分别有一个管理 activity 的地方，服务端是在 mHistory 中，处于 mHistory 栈顶的就是当前处于 running 状态的 activity，客户端是在 mActivities 中。
2）在 startActivity 时，首先会在ActivityManagerService 中建立 HistoryRecord，并加入到 mHistory 中，然后通过 scheduleLaunchActivity 在客户端创建 ActivityRecord 记录并加入到 mActivities 中。最终在 ActivityThread 发起请求，进入消息循环，完成 activity的启动和窗口的管理等。 








引用：
[关于Activity的生命周期](http://www.jianshu.com/p/50d0edbbc561)
[onCreate & onStart & onResume & onStop & onPause & onDestroy & onRestart & onWindowFocusChanged](http://blog.csdn.net/qweewqpkn/article/details/45890981)
[Activity的四种启动模式和onNewIntent()](http://blog.csdn.net/linghu_java/article/details/17266603)
[关于代码实现activity的启动模式](http://blog.csdn.net/u010897392/article/details/38413037)
[Intent相关FLAG介绍和Activity启动模式](http://blog.csdn.net/miao309410364/article/details/47262869)
[android深入解析Activity的launchMode启动模式，Intent Flag，taskAffinity](http://blog.csdn.net/self_study/article/details/48055011)
[Activity的启动方式和flag详解](http://blog.csdn.net/singwhatiwanna/article/details/9294285)
[Android Intent.FLAG_NEW_TASK详解，包括其他的标记的一些解释](http://www.cnblogs.com/xiaoQLu/archive/2012/07/17/2595294.html)
[Android--使用剪切板在Activity中传值](https://www.cnblogs.com/plokmju/p/3140099.html)
[Android入门篇三：使用静态变量在Activity之间传递数据](http://blog.csdn.net/wulianghuan/article/details/8583310)
[Activity之间传递数据的方式及常见问题总结](http://blog.csdn.net/rflyee/article/details/47431633)
[关于Android Activity之间传递数据的6种方式](http://www.jb51.net/article/108903.htm)
[Android基础之Activity系列 - Activity间的数据传递](http://blog.csdn.net/pangrongxian/article/details/50166835)
[Android主题切换方案总结](http://blog.csdn.net/xingfeng2010/article/details/22854977)

