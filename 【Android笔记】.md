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
- [【Android 广播】](#android-%E5%B9%BF%E6%92%AD)
  - [BroadcastReceiver简介](#broadcastreceiver%E7%AE%80%E4%BB%8B)
    - [BroadcastReceiver作用](#broadcastreceiver%E4%BD%9C%E7%94%A8)
    - [广播应用场景](#%E5%B9%BF%E6%92%AD%E5%BA%94%E7%94%A8%E5%9C%BA%E6%99%AF)
  - [实现原理](#%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86)
      - [采用的模型](#%E9%87%87%E7%94%A8%E7%9A%84%E6%A8%A1%E5%9E%8B)
      - [模型讲解](#%E6%A8%A1%E5%9E%8B%E8%AE%B2%E8%A7%A3)
  - [使用流程](#%E4%BD%BF%E7%94%A8%E6%B5%81%E7%A8%8B)
      - [1、自定义广播接收器BroadcastReceiver](#1%E8%87%AA%E5%AE%9A%E4%B9%89%E5%B9%BF%E6%92%AD%E6%8E%A5%E6%94%B6%E5%99%A8broadcastreceiver)
      - [2、广播接收器注册](#2%E5%B9%BF%E6%92%AD%E6%8E%A5%E6%94%B6%E5%99%A8%E6%B3%A8%E5%86%8C)
        - [1）静态注册](#1%E9%9D%99%E6%80%81%E6%B3%A8%E5%86%8C)
        - [2）动态注册](#2%E5%8A%A8%E6%80%81%E6%B3%A8%E5%86%8C)
        - [特别注意](#%E7%89%B9%E5%88%AB%E6%B3%A8%E6%84%8F)
        - [两种注册方式的区别](#%E4%B8%A4%E7%A7%8D%E6%B3%A8%E5%86%8C%E6%96%B9%E5%BC%8F%E7%9A%84%E5%8C%BA%E5%88%AB)
      - [3、发送广播](#3%E5%8F%91%E9%80%81%E5%B9%BF%E6%92%AD)
  - [无序广播](#%E6%97%A0%E5%BA%8F%E5%B9%BF%E6%92%AD)
    - [1、普通广播（Normal Broadcast）](#1%E6%99%AE%E9%80%9A%E5%B9%BF%E6%92%ADnormal-broadcast)
      - [① 自定义广播接收器](#%E2%91%A0-%E8%87%AA%E5%AE%9A%E4%B9%89%E5%B9%BF%E6%92%AD%E6%8E%A5%E6%94%B6%E5%99%A8)
      - [② 注册广播接收器](#%E2%91%A1-%E6%B3%A8%E5%86%8C%E5%B9%BF%E6%92%AD%E6%8E%A5%E6%94%B6%E5%99%A8)
      - [③ 发送广播](#%E2%91%A2-%E5%8F%91%E9%80%81%E5%B9%BF%E6%92%AD)
    - [2、系统广播（System Broadcast）](#2%E7%B3%BB%E7%BB%9F%E5%B9%BF%E6%92%ADsystem-broadcast)
    - [3、本地广播（Local Broadcast）](#3%E6%9C%AC%E5%9C%B0%E5%B9%BF%E6%92%ADlocal-broadcast)
      - [注销Receiver](#%E6%B3%A8%E9%94%80receiver)
      - [发送异步广播](#%E5%8F%91%E9%80%81%E5%BC%82%E6%AD%A5%E5%B9%BF%E6%92%AD)
      - [发送同步广播](#%E5%8F%91%E9%80%81%E5%90%8C%E6%AD%A5%E5%B9%BF%E6%92%AD)
    - [4、粘性广播（Sticky Broadcast）](#4%E7%B2%98%E6%80%A7%E5%B9%BF%E6%92%ADsticky-broadcast)
  - [有序广播](#%E6%9C%89%E5%BA%8F%E5%B9%BF%E6%92%AD)
    - [5、有序广播（Ordered Broadcast）](#5%E6%9C%89%E5%BA%8F%E5%B9%BF%E6%92%ADordered-broadcast)
        - [① 自定义广播接收器](#%E2%91%A0-%E8%87%AA%E5%AE%9A%E4%B9%89%E5%B9%BF%E6%92%AD%E6%8E%A5%E6%94%B6%E5%99%A8-1)
        - [② 注册广播接收器](#%E2%91%A1-%E6%B3%A8%E5%86%8C%E5%B9%BF%E6%92%AD%E6%8E%A5%E6%94%B6%E5%99%A8-1)
        - [③ 发送广播](#%E2%91%A2-%E5%8F%91%E9%80%81%E5%B9%BF%E6%92%AD-1)
      - [拦截广播](#%E6%8B%A6%E6%88%AA%E5%B9%BF%E6%92%AD)
      - [最终广播接收者](#%E6%9C%80%E7%BB%88%E5%B9%BF%E6%92%AD%E6%8E%A5%E6%94%B6%E8%80%85)
  - [特别注意](#%E7%89%B9%E5%88%AB%E6%B3%A8%E6%84%8F-1)
  - [BroadCastReceiver生命周期](#broadcastreceiver%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F)
    - [BroadcastReceiver对象的生命周期](#broadcastreceiver%E5%AF%B9%E8%B1%A1%E7%9A%84%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F)
      - [静态注册的BroadcastReceiver](#%E9%9D%99%E6%80%81%E6%B3%A8%E5%86%8C%E7%9A%84broadcastreceiver)
      - [动态注册的BroadcastReceiver](#%E5%8A%A8%E6%80%81%E6%B3%A8%E5%86%8C%E7%9A%84broadcastreceiver)
    - [BroadcastReceiver所在进程的生命周期](#broadcastreceiver%E6%89%80%E5%9C%A8%E8%BF%9B%E7%A8%8B%E7%9A%84%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F)
    - [BroadCastReceiver生命周期总结](#broadcastreceiver%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E6%80%BB%E7%BB%93)
- [Android 广播面试题](#android-%E5%B9%BF%E6%92%AD%E9%9D%A2%E8%AF%95%E9%A2%98)
  - [请描述一下 BroadcastReceiver](#%E8%AF%B7%E6%8F%8F%E8%BF%B0%E4%B8%80%E4%B8%8B-broadcastreceiver)
  - [如何注册和使用 BroadcastReceiver](#%E5%A6%82%E4%BD%95%E6%B3%A8%E5%86%8C%E5%92%8C%E4%BD%BF%E7%94%A8-broadcastreceiver)
  - [BroadCastReceiver生命周期](#broadcastreceiver%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F-1)
  - [如何让自己的广播只让指定的app接收](#%E5%A6%82%E4%BD%95%E8%AE%A9%E8%87%AA%E5%B7%B1%E7%9A%84%E5%B9%BF%E6%92%AD%E5%8F%AA%E8%AE%A9%E6%8C%87%E5%AE%9A%E7%9A%84app%E6%8E%A5%E6%94%B6)
  - [什么是最终广播接收者？](#%E4%BB%80%E4%B9%88%E6%98%AF%E6%9C%80%E7%BB%88%E5%B9%BF%E6%92%AD%E6%8E%A5%E6%94%B6%E8%80%85)
  - [广播的优先级对无序广播生效吗？](#%E5%B9%BF%E6%92%AD%E7%9A%84%E4%BC%98%E5%85%88%E7%BA%A7%E5%AF%B9%E6%97%A0%E5%BA%8F%E5%B9%BF%E6%92%AD%E7%94%9F%E6%95%88%E5%90%97)
  - [动态注册的广播优先级谁高？](#%E5%8A%A8%E6%80%81%E6%B3%A8%E5%86%8C%E7%9A%84%E5%B9%BF%E6%92%AD%E4%BC%98%E5%85%88%E7%BA%A7%E8%B0%81%E9%AB%98)
  - [如何判断当前BroadcastReceiver接收到的是有序广播还是无序广播 ？](#%E5%A6%82%E4%BD%95%E5%88%A4%E6%96%AD%E5%BD%93%E5%89%8Dbroadcastreceiver%E6%8E%A5%E6%94%B6%E5%88%B0%E7%9A%84%E6%98%AF%E6%9C%89%E5%BA%8F%E5%B9%BF%E6%92%AD%E8%BF%98%E6%98%AF%E6%97%A0%E5%BA%8F%E5%B9%BF%E6%92%AD-)
  - [Android 引入广播机制的用意](#android-%E5%BC%95%E5%85%A5%E5%B9%BF%E6%92%AD%E6%9C%BA%E5%88%B6%E7%9A%84%E7%94%A8%E6%84%8F)
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
- [【Android ContentProvider】](#android-contentprovider)
  - [概述](#%E6%A6%82%E8%BF%B0)
    - [为何使用ContentProvider](#%E4%B8%BA%E4%BD%95%E4%BD%BF%E7%94%A8contentprovider)
    - [为何使用ContentResolver](#%E4%B8%BA%E4%BD%95%E4%BD%BF%E7%94%A8contentresolver)
  - [ContentProvider及其URI](#contentprovider%E5%8F%8A%E5%85%B6uri)
    - [引入ContentProvider的URI](#%E5%BC%95%E5%85%A5contentprovider%E7%9A%84uri)
    - [ContentProvider与对应的URI](#contentprovider%E4%B8%8E%E5%AF%B9%E5%BA%94%E7%9A%84uri)
    - [ContentProvider声明方式](#contentprovider%E5%A3%B0%E6%98%8E%E6%96%B9%E5%BC%8F)
    - [**ContentURI的全局流程图**](#contenturi%E7%9A%84%E5%85%A8%E5%B1%80%E6%B5%81%E7%A8%8B%E5%9B%BE)
    - [UriMatcher](#urimatcher)
  - [ContentResolver](#contentresolver)
  - [使用步骤](#%E4%BD%BF%E7%94%A8%E6%AD%A5%E9%AA%A4)
    - [ContentProvider提供数据库查询接口](#contentprovider%E6%8F%90%E4%BE%9B%E6%95%B0%E6%8D%AE%E5%BA%93%E6%9F%A5%E8%AF%A2%E6%8E%A5%E5%8F%A3)
      - [1、利用SQLiteOpenHelper创建数据库、数据表](#1%E5%88%A9%E7%94%A8sqliteopenhelper%E5%88%9B%E5%BB%BA%E6%95%B0%E6%8D%AE%E5%BA%93%E6%95%B0%E6%8D%AE%E8%A1%A8)
      - [2、利用ContentProvider提供数据库操作接口](#2%E5%88%A9%E7%94%A8contentprovider%E6%8F%90%E4%BE%9B%E6%95%B0%E6%8D%AE%E5%BA%93%E6%93%8D%E4%BD%9C%E6%8E%A5%E5%8F%A3)
      - [3、使用UriMatcher匹配URI](#3%E4%BD%BF%E7%94%A8urimatcher%E5%8C%B9%E9%85%8Duri)
        - [**addUri**](#adduri)
      - [4、insert方法](#4insert%E6%96%B9%E6%B3%95)
      - [5、update方法](#5update%E6%96%B9%E6%B3%95)
      - [6、query方法](#6query%E6%96%B9%E6%B3%95)
      - [7、delete方法](#7delete%E6%96%B9%E6%B3%95)
      - [8、getType()](#8gettype)
      - [9、AndroidManifest.xml中声明provider](#9androidmanifestxml%E4%B8%AD%E5%A3%B0%E6%98%8Eprovider)
    - [第三方应用通过URI操作共享数据库](#%E7%AC%AC%E4%B8%89%E6%96%B9%E5%BA%94%E7%94%A8%E9%80%9A%E8%BF%87uri%E6%93%8D%E4%BD%9C%E5%85%B1%E4%BA%AB%E6%95%B0%E6%8D%AE%E5%BA%93)
      - [1、ContentResolver操作URI](#1contentresolver%E6%93%8D%E4%BD%9Curi)
      - [2、全局操作](#2%E5%85%A8%E5%B1%80%E6%93%8D%E4%BD%9C)
      - [3、query()查询操作](#3query%E6%9F%A5%E8%AF%A2%E6%93%8D%E4%BD%9C)
      - [4、insert()插入操作](#4insert%E6%8F%92%E5%85%A5%E6%93%8D%E4%BD%9C)
      - [5、update()更新操作](#5update%E6%9B%B4%E6%96%B0%E6%93%8D%E4%BD%9C)
      - [6、delete()删除操作](#6delete%E5%88%A0%E9%99%A4%E6%93%8D%E4%BD%9C)
    - [运行结果](#%E8%BF%90%E8%A1%8C%E7%BB%93%E6%9E%9C)
  - [ContentProvider中的getType](#contentprovider%E4%B8%AD%E7%9A%84gettype)
    - [MIME类型](#mime%E7%B1%BB%E5%9E%8B)
      - [什么是MIME类型](#%E4%BB%80%E4%B9%88%E6%98%AFmime%E7%B1%BB%E5%9E%8B)
      - [MIME类型有什么用](#mime%E7%B1%BB%E5%9E%8B%E6%9C%89%E4%BB%80%E4%B9%88%E7%94%A8)
    - [getType()概述](#gettype%E6%A6%82%E8%BF%B0)
    - [getType()与Activity的关系](#gettype%E4%B8%8Eactivity%E7%9A%84%E5%85%B3%E7%B3%BB)
    - [新建通过URI启动的Activity](#%E6%96%B0%E5%BB%BA%E9%80%9A%E8%BF%87uri%E5%90%AF%E5%8A%A8%E7%9A%84activity)
  - [数据库读写权限](#%E6%95%B0%E6%8D%AE%E5%BA%93%E8%AF%BB%E5%86%99%E6%9D%83%E9%99%90)
    - [概述](#%E6%A6%82%E8%BF%B0-1)
    - [有关自定义权限](#%E6%9C%89%E5%85%B3%E8%87%AA%E5%AE%9A%E4%B9%89%E6%9D%83%E9%99%90)
    - [实例：带有权限的ContentProvder](#%E5%AE%9E%E4%BE%8B%E5%B8%A6%E6%9C%89%E6%9D%83%E9%99%90%E7%9A%84contentprovder)
  - [ContentObserver](#contentobserver)
    - [数据监听步骤](#%E6%95%B0%E6%8D%AE%E7%9B%91%E5%90%AC%E6%AD%A5%E9%AA%A4)
      - [(1)生成一个类派生自ContentObserver](#1%E7%94%9F%E6%88%90%E4%B8%80%E4%B8%AA%E7%B1%BB%E6%B4%BE%E7%94%9F%E8%87%AAcontentobserver)
      - [(2)注册和反注册监听](#2%E6%B3%A8%E5%86%8C%E5%92%8C%E5%8F%8D%E6%B3%A8%E5%86%8C%E7%9B%91%E5%90%AC)
      - [(3)在ContentProvider中通知数据库变动](#3%E5%9C%A8contentprovider%E4%B8%AD%E9%80%9A%E7%9F%A5%E6%95%B0%E6%8D%AE%E5%BA%93%E5%8F%98%E5%8A%A8)
    - [registerContentObserver](#registercontentobserver)
  - [SetProjectionMap——投影映射](#setprojectionmap%E6%8A%95%E5%BD%B1%E6%98%A0%E5%B0%84)
  - [实例](#%E5%AE%9E%E4%BE%8B)
    - [自动获取短信验证码](#%E8%87%AA%E5%8A%A8%E8%8E%B7%E5%8F%96%E7%9F%AD%E4%BF%A1%E9%AA%8C%E8%AF%81%E7%A0%81)
        - [1.自定义监听类](#1%E8%87%AA%E5%AE%9A%E4%B9%89%E7%9B%91%E5%90%AC%E7%B1%BB)
        - [2.在页面注册监听类](#2%E5%9C%A8%E9%A1%B5%E9%9D%A2%E6%B3%A8%E5%86%8C%E7%9B%91%E5%90%AC%E7%B1%BB)
        - [3.声明读取短信权限](#3%E5%A3%B0%E6%98%8E%E8%AF%BB%E5%8F%96%E7%9F%AD%E4%BF%A1%E6%9D%83%E9%99%90)
        - [4.为了防止内存泄漏，记得注销监听](#4%E4%B8%BA%E4%BA%86%E9%98%B2%E6%AD%A2%E5%86%85%E5%AD%98%E6%B3%84%E6%BC%8F%E8%AE%B0%E5%BE%97%E6%B3%A8%E9%94%80%E7%9B%91%E5%90%AC)
    - [短信的备份和恢复](#%E7%9F%AD%E4%BF%A1%E7%9A%84%E5%A4%87%E4%BB%BD%E5%92%8C%E6%81%A2%E5%A4%8D)
      - [准备知识](#%E5%87%86%E5%A4%87%E7%9F%A5%E8%AF%86)
      - [实现步骤](#%E5%AE%9E%E7%8E%B0%E6%AD%A5%E9%AA%A4)
        - [获取短信列表](#%E8%8E%B7%E5%8F%96%E7%9F%AD%E4%BF%A1%E5%88%97%E8%A1%A8)
        - [将短信数据保存到xml文件中](#%E5%B0%86%E7%9F%AD%E4%BF%A1%E6%95%B0%E6%8D%AE%E4%BF%9D%E5%AD%98%E5%88%B0xml%E6%96%87%E4%BB%B6%E4%B8%AD)
        - [恢复短信](#%E6%81%A2%E5%A4%8D%E7%9F%AD%E4%BF%A1)
    - [操作系统联系人](#%E6%93%8D%E4%BD%9C%E7%B3%BB%E7%BB%9F%E8%81%94%E7%B3%BB%E4%BA%BA)
      - [准备知识](#%E5%87%86%E5%A4%87%E7%9F%A5%E8%AF%86-1)
      - [实现步骤](#%E5%AE%9E%E7%8E%B0%E6%AD%A5%E9%AA%A4-1)
- [Android ContentProvider面试题](#android-contentprovider%E9%9D%A2%E8%AF%95%E9%A2%98)
  - [ContentProvider如何实现数据共享？](#contentprovider%E5%A6%82%E4%BD%95%E5%AE%9E%E7%8E%B0%E6%95%B0%E6%8D%AE%E5%85%B1%E4%BA%AB)
  - [为什么要用 ContentProvider？它和 sql 的实现上有什么差别？](#%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E7%94%A8-contentprovider%E5%AE%83%E5%92%8C-sql-%E7%9A%84%E5%AE%9E%E7%8E%B0%E4%B8%8A%E6%9C%89%E4%BB%80%E4%B9%88%E5%B7%AE%E5%88%AB)
  - [ContentProvider、ContentResolver、ContentObserver 之间的关系](#contentprovidercontentresolvercontentobserver-%E4%B9%8B%E9%97%B4%E7%9A%84%E5%85%B3%E7%B3%BB)
  - [如何访问 asserts 资源目录下的数据库？](#%E5%A6%82%E4%BD%95%E8%AE%BF%E9%97%AE-asserts-%E8%B5%84%E6%BA%90%E7%9B%AE%E5%BD%95%E4%B8%8B%E7%9A%84%E6%95%B0%E6%8D%AE%E5%BA%93)
  - [如何在高并发下进行数据库查询？](#%E5%A6%82%E4%BD%95%E5%9C%A8%E9%AB%98%E5%B9%B6%E5%8F%91%E4%B8%8B%E8%BF%9B%E8%A1%8C%E6%95%B0%E6%8D%AE%E5%BA%93%E6%9F%A5%E8%AF%A2)
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
- [【Android SQLite】](#android-sqlite)
  - [SQLite 简介](#sqlite-%E7%AE%80%E4%BB%8B)
  - [使用 SQLiteOpenHelper 创建数据库](#%E4%BD%BF%E7%94%A8-sqliteopenhelper-%E5%88%9B%E5%BB%BA%E6%95%B0%E6%8D%AE%E5%BA%93)
    - [创建 SQLiteOpenHelper 类](#%E5%88%9B%E5%BB%BA-sqliteopenhelper-%E7%B1%BB)
    - [使用 SQLiteOpenHelper 类](#%E4%BD%BF%E7%94%A8-sqliteopenhelper-%E7%B1%BB)
  - [数据库的增删改查](#%E6%95%B0%E6%8D%AE%E5%BA%93%E7%9A%84%E5%A2%9E%E5%88%A0%E6%94%B9%E6%9F%A5)
    - [1、使用纯 SQL 语句实现](#1%E4%BD%BF%E7%94%A8%E7%BA%AF-sql-%E8%AF%AD%E5%8F%A5%E5%AE%9E%E7%8E%B0)
      - [添加数据](#%E6%B7%BB%E5%8A%A0%E6%95%B0%E6%8D%AE)
      - [删除数据](#%E5%88%A0%E9%99%A4%E6%95%B0%E6%8D%AE)
      - [修改数据](#%E4%BF%AE%E6%94%B9%E6%95%B0%E6%8D%AE)
      - [查询数据](#%E6%9F%A5%E8%AF%A2%E6%95%B0%E6%8D%AE)
    - [2、使用特有 API 实现](#2%E4%BD%BF%E7%94%A8%E7%89%B9%E6%9C%89-api-%E5%AE%9E%E7%8E%B0)
      - [添加数据](#%E6%B7%BB%E5%8A%A0%E6%95%B0%E6%8D%AE-1)
      - [删除数据](#%E5%88%A0%E9%99%A4%E6%95%B0%E6%8D%AE-1)
      - [修改数据](#%E4%BF%AE%E6%94%B9%E6%95%B0%E6%8D%AE-1)
      - [查询数据](#%E6%9F%A5%E8%AF%A2%E6%95%B0%E6%8D%AE-1)
    - [两种 SQLiteDatabase 的不同](#%E4%B8%A4%E7%A7%8D-sqlitedatabase-%E7%9A%84%E4%B8%8D%E5%90%8C)
  - [数据库的升级和事务操作](#%E6%95%B0%E6%8D%AE%E5%BA%93%E7%9A%84%E5%8D%87%E7%BA%A7%E5%92%8C%E4%BA%8B%E5%8A%A1%E6%93%8D%E4%BD%9C)
    - [数据库的升级](#%E6%95%B0%E6%8D%AE%E5%BA%93%E7%9A%84%E5%8D%87%E7%BA%A7)
    - [事务的操作](#%E4%BA%8B%E5%8A%A1%E7%9A%84%E6%93%8D%E4%BD%9C)
  - [sqlite3 工具的使用](#sqlite3-%E5%B7%A5%E5%85%B7%E7%9A%84%E4%BD%BF%E7%94%A8)
- [【Android 自定义View】](#android-%E8%87%AA%E5%AE%9A%E4%B9%89view)
- [自定义View基础](#%E8%87%AA%E5%AE%9A%E4%B9%89view%E5%9F%BA%E7%A1%80)
- [自定义TextView](#%E8%87%AA%E5%AE%9A%E4%B9%89textview)
  - [1. 继承View，重写onDraw方法](#1-%E7%BB%A7%E6%89%BFview%E9%87%8D%E5%86%99ondraw%E6%96%B9%E6%B3%95)
    - [View的构造方法](#view%E7%9A%84%E6%9E%84%E9%80%A0%E6%96%B9%E6%B3%95)
  - [2. 自定义属性](#2-%E8%87%AA%E5%AE%9A%E4%B9%89%E5%B1%9E%E6%80%A7)
    - [创建attrs.xml文件](#%E5%88%9B%E5%BB%BAattrsxml%E6%96%87%E4%BB%B6)
    - [在构造方法中获取自定义属性的值：](#%E5%9C%A8%E6%9E%84%E9%80%A0%E6%96%B9%E6%B3%95%E4%B8%AD%E8%8E%B7%E5%8F%96%E8%87%AA%E5%AE%9A%E4%B9%89%E5%B1%9E%E6%80%A7%E7%9A%84%E5%80%BC)
  - [3. onMeasure方法](#3-onmeasure%E6%96%B9%E6%B3%95)
    - [**① MeasureSpec**](#%E2%91%A0-measurespec)
    - [**② 分析为什么我们自定义的MyTextView设置了wrap_content却填充屏幕**](#%E2%91%A1-%E5%88%86%E6%9E%90%E4%B8%BA%E4%BB%80%E4%B9%88%E6%88%91%E4%BB%AC%E8%87%AA%E5%AE%9A%E4%B9%89%E7%9A%84mytextview%E8%AE%BE%E7%BD%AE%E4%BA%86wrap_content%E5%8D%B4%E5%A1%AB%E5%85%85%E5%B1%8F%E5%B9%95)
    - [**③ 重写onMeasure方法**](#%E2%91%A2-%E9%87%8D%E5%86%99onmeasure%E6%96%B9%E6%B3%95)
  - [4. 自动换行](#4-%E8%87%AA%E5%8A%A8%E6%8D%A2%E8%A1%8C)
  - [源码下载：](#%E6%BA%90%E7%A0%81%E4%B8%8B%E8%BD%BD)
- [自定义控件的基本步骤：](#%E8%87%AA%E5%AE%9A%E4%B9%89%E6%8E%A7%E4%BB%B6%E7%9A%84%E5%9F%BA%E6%9C%AC%E6%AD%A5%E9%AA%A4)
- [深入解析自定义属性](#%E6%B7%B1%E5%85%A5%E8%A7%A3%E6%9E%90%E8%87%AA%E5%AE%9A%E4%B9%89%E5%B1%9E%E6%80%A7)
  - [1. 为什么要自定义属性](#1-%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E8%87%AA%E5%AE%9A%E4%B9%89%E5%B1%9E%E6%80%A7)
  - [2. 怎样自定义属性](#2-%E6%80%8E%E6%A0%B7%E8%87%AA%E5%AE%9A%E4%B9%89%E5%B1%9E%E6%80%A7)
  - [3. 属性值的类型format](#3-%E5%B1%9E%E6%80%A7%E5%80%BC%E7%9A%84%E7%B1%BB%E5%9E%8Bformat)
    - [**(1) reference：参考某一资源ID**](#1-reference%E5%8F%82%E8%80%83%E6%9F%90%E4%B8%80%E8%B5%84%E6%BA%90id)
    - [**(2) color：颜色值**](#2-color%E9%A2%9C%E8%89%B2%E5%80%BC)
    - [**(3) boolean：布尔值**](#3-boolean%E5%B8%83%E5%B0%94%E5%80%BC)
    - [**(4) dimension：尺寸值**](#4-dimension%E5%B0%BA%E5%AF%B8%E5%80%BC)
    - [**(5) float：浮点值**](#5-float%E6%B5%AE%E7%82%B9%E5%80%BC)
    - [**(6) integer：整型值**](#6-integer%E6%95%B4%E5%9E%8B%E5%80%BC)
    - [**(7) string：字符串**](#7-string%E5%AD%97%E7%AC%A6%E4%B8%B2)
    - [**(8) fraction：百分数**](#8-fraction%E7%99%BE%E5%88%86%E6%95%B0)
    - [**(9) enum：枚举值**](#9-enum%E6%9E%9A%E4%B8%BE%E5%80%BC)
    - [**(10) flag：位或运算**](#10-flag%E4%BD%8D%E6%88%96%E8%BF%90%E7%AE%97)
    - [**(11) 混合类型：属性定义时可以指定多种类型值**](#11-%E6%B7%B7%E5%90%88%E7%B1%BB%E5%9E%8B%E5%B1%9E%E6%80%A7%E5%AE%9A%E4%B9%89%E6%97%B6%E5%8F%AF%E4%BB%A5%E6%8C%87%E5%AE%9A%E5%A4%9A%E7%A7%8D%E7%B1%BB%E5%9E%8B%E5%80%BC)
  - [4. 类中获取属性值](#4-%E7%B1%BB%E4%B8%AD%E8%8E%B7%E5%8F%96%E5%B1%9E%E6%80%A7%E5%80%BC)
  - [5. Attributeset和TypedArray以及declare-styleable](#5-attributeset%E5%92%8Ctypedarray%E4%BB%A5%E5%8F%8Adeclare-styleable)
- [深入解析控件测量onMeasure](#%E6%B7%B1%E5%85%A5%E8%A7%A3%E6%9E%90%E6%8E%A7%E4%BB%B6%E6%B5%8B%E9%87%8Fonmeasure)
  - [1. onMeasure什么时候会被调用](#1-onmeasure%E4%BB%80%E4%B9%88%E6%97%B6%E5%80%99%E4%BC%9A%E8%A2%AB%E8%B0%83%E7%94%A8)
  - [2. onMeasure方法执行流程](#2-onmeasure%E6%96%B9%E6%B3%95%E6%89%A7%E8%A1%8C%E6%B5%81%E7%A8%8B)
  - [3. MeasureSpec类](#3-measurespec%E7%B1%BB)
  - [4. 从ViewGroup的onMeasure到View的onMeasure](#4-%E4%BB%8Eviewgroup%E7%9A%84onmeasure%E5%88%B0view%E7%9A%84onmeasure)
    - [① ViewGroup中三个测量子控件的方法：](#%E2%91%A0-viewgroup%E4%B8%AD%E4%B8%89%E4%B8%AA%E6%B5%8B%E9%87%8F%E5%AD%90%E6%8E%A7%E4%BB%B6%E7%9A%84%E6%96%B9%E6%B3%95)
    - [② getChildMeasureSpec方法](#%E2%91%A1-getchildmeasurespec%E6%96%B9%E6%B3%95)
    - [③ View的onMeasure](#%E2%91%A2-view%E7%9A%84onmeasure)
    - [④ setMeasuredDimension](#%E2%91%A3-setmeasureddimension)
  - [总结：](#%E6%80%BB%E7%BB%93)
    - [measure() VS onMeasure()](#measure-vs-onmeasure)
- [自定义布局容器](#%E8%87%AA%E5%AE%9A%E4%B9%89%E5%B8%83%E5%B1%80%E5%AE%B9%E5%99%A8)
  - [1. 简单实现水平排列效果](#1-%E7%AE%80%E5%8D%95%E5%AE%9E%E7%8E%B0%E6%B0%B4%E5%B9%B3%E6%8E%92%E5%88%97%E6%95%88%E6%9E%9C)
  - [2. 自定义LayoutParams](#2-%E8%87%AA%E5%AE%9A%E4%B9%89layoutparams)
    - [① 大致明确布局容器的需求，初步定义布局属性](#%E2%91%A0-%E5%A4%A7%E8%87%B4%E6%98%8E%E7%A1%AE%E5%B8%83%E5%B1%80%E5%AE%B9%E5%99%A8%E7%9A%84%E9%9C%80%E6%B1%82%E5%88%9D%E6%AD%A5%E5%AE%9A%E4%B9%89%E5%B8%83%E5%B1%80%E5%B1%9E%E6%80%A7)
    - [② 继承LayoutParams，定义布局参数类](#%E2%91%A1-%E7%BB%A7%E6%89%BFlayoutparams%E5%AE%9A%E4%B9%89%E5%B8%83%E5%B1%80%E5%8F%82%E6%95%B0%E7%B1%BB)
    - [③ 重写generateLayoutParams()](#%E2%91%A2-%E9%87%8D%E5%86%99generatelayoutparams)
    - [④ 在onMeasure和onLayout中使用布局参数](#%E2%91%A3-%E5%9C%A8onmeasure%E5%92%8Conlayout%E4%B8%AD%E4%BD%BF%E7%94%A8%E5%B8%83%E5%B1%80%E5%8F%82%E6%95%B0)
    - [⑤ 在布局文件中使用布局属性](#%E2%91%A4-%E5%9C%A8%E5%B8%83%E5%B1%80%E6%96%87%E4%BB%B6%E4%B8%AD%E4%BD%BF%E7%94%A8%E5%B8%83%E5%B1%80%E5%B1%9E%E6%80%A7)
  - [3. 支持layout_margin属性](#3-%E6%94%AF%E6%8C%81layout_margin%E5%B1%9E%E6%80%A7)
  - [总结：](#%E6%80%BB%E7%BB%93-1)
    - [**自定义ViewGroup的步骤：**](#%E8%87%AA%E5%AE%9A%E4%B9%89viewgroup%E7%9A%84%E6%AD%A5%E9%AA%A4)
    - [**为布局容器自定义布局属性：**](#%E4%B8%BA%E5%B8%83%E5%B1%80%E5%AE%B9%E5%99%A8%E8%87%AA%E5%AE%9A%E4%B9%89%E5%B8%83%E5%B1%80%E5%B1%9E%E6%80%A7)
  - [自定义控件方法调用流程](#%E8%87%AA%E5%AE%9A%E4%B9%89%E6%8E%A7%E4%BB%B6%E6%96%B9%E6%B3%95%E8%B0%83%E7%94%A8%E6%B5%81%E7%A8%8B)
  - [layout() VS onLayout()](#layout-vs-onlayout)
- [【Android 自定义View之绘图】](#android-%E8%87%AA%E5%AE%9A%E4%B9%89view%E4%B9%8B%E7%BB%98%E5%9B%BE)
- [基础图形的绘制](#%E5%9F%BA%E7%A1%80%E5%9B%BE%E5%BD%A2%E7%9A%84%E7%BB%98%E5%88%B6)
  - [一、Paint与Canvas](#%E4%B8%80paint%E4%B8%8Ecanvas)
    - [Paint](#paint)
      - [Paint的基本设置函数](#paint%E7%9A%84%E5%9F%BA%E6%9C%AC%E8%AE%BE%E7%BD%AE%E5%87%BD%E6%95%B0)
      - [1 、setAntiAlias(true) 设置是否抗锯齿](#1-setantialiastrue-%E8%AE%BE%E7%BD%AE%E6%98%AF%E5%90%A6%E6%8A%97%E9%94%AF%E9%BD%BF)
      - [2、setStyle (Paint.Style style) 设置填充样式](#2setstyle-paintstyle-style-%E8%AE%BE%E7%BD%AE%E5%A1%AB%E5%85%85%E6%A0%B7%E5%BC%8F)
      - [3、setColor(@ColorInt int color) 设置画笔颜色](#3setcolorcolorint-int-color-%E8%AE%BE%E7%BD%AE%E7%94%BB%E7%AC%94%E9%A2%9C%E8%89%B2)
      - [4、setStrokeWidth(float width) 设置画笔宽度](#4setstrokewidthfloat-width-%E8%AE%BE%E7%BD%AE%E7%94%BB%E7%AC%94%E5%AE%BD%E5%BA%A6)
      - [5、setShadowLayer(float radius, float dx, float dy, int shadowColor) 设置阴影](#5setshadowlayerfloat-radius-float-dx-float-dy-int-shadowcolor-%E8%AE%BE%E7%BD%AE%E9%98%B4%E5%BD%B1)
    - [Canvas](#canvas)
  - [二、基本几何图形绘制](#%E4%BA%8C%E5%9F%BA%E6%9C%AC%E5%87%A0%E4%BD%95%E5%9B%BE%E5%BD%A2%E7%BB%98%E5%88%B6)
    - [1、画直线drawLine](#1%E7%94%BB%E7%9B%B4%E7%BA%BFdrawline)
    - [2、多条直线drawLines](#2%E5%A4%9A%E6%9D%A1%E7%9B%B4%E7%BA%BFdrawlines)
    - [3、点及多个点drawPoint、drawPoints](#3%E7%82%B9%E5%8F%8A%E5%A4%9A%E4%B8%AA%E7%82%B9drawpointdrawpoints)
    - [4、矩形drawRect、drawRoundRect](#4%E7%9F%A9%E5%BD%A2drawrectdrawroundrect)
    - [5、圆形drawCircle](#5%E5%9C%86%E5%BD%A2drawcircle)
    - [6、椭圆drawOval](#6%E6%A4%AD%E5%9C%86drawoval)
    - [7、圆弧drawArc](#7%E5%9C%86%E5%BC%A7drawarc)
  - [引用：](#%E5%BC%95%E7%94%A8)
- [路径（Path）](#%E8%B7%AF%E5%BE%84path)
  - [Path常用方法](#path%E5%B8%B8%E7%94%A8%E6%96%B9%E6%B3%95)
  - [Path方法使用详解](#path%E6%96%B9%E6%B3%95%E4%BD%BF%E7%94%A8%E8%AF%A6%E8%A7%A3)
    - [moveTo , lineTo , setLastPoint , close](#moveto--lineto--setlastpoint--close)
      - [1、lineTo](#1lineto)
      - [2、moveTo 和setLastPoint](#2moveto-%E5%92%8Csetlastpoint)
      - [3、close](#3close)
    - [quadTo,cubicTo](#quadtocubicto)
      - [1、quadTo](#1quadto)
      - [2、cubicTo](#2cubicto)
    - [addXxx和arcTo](#addxxx%E5%92%8Carcto)
      - [1、添加基本图形](#1%E6%B7%BB%E5%8A%A0%E5%9F%BA%E6%9C%AC%E5%9B%BE%E5%BD%A2)
      - [2、addPath](#2addpath)
      - [3、addArc与arcTo](#3addarc%E4%B8%8Earcto)
    - [isEmpty、 isRect、 set 和 offset](#isempty-isrect-set-%E5%92%8C-offset)
      - [isEmpty](#isempty)
      - [isRect](#isrect)
      - [set](#set)
      - [offset](#offset)
    - [FillType](#filltype)
      - [setFillType](#setfilltype)
        - [**WINDING**](#winding)
        - [**EVEN_ODD**](#even_odd)
        - [**INVERSE_WINDING**](#inverse_winding)
        - [**INVERSE_EVEN_ODD**](#inverse_even_odd)
      - [isInverseFillType](#isinversefilltype)
      - [toggleInverseFillType](#toggleinversefilltype)
  - [引用：](#%E5%BC%95%E7%94%A8-1)
- [文字（Text）](#%E6%96%87%E5%AD%97text)
  - [一、文字](#%E4%B8%80%E6%96%87%E5%AD%97)
    - [1、文本绘图样式](#1%E6%96%87%E6%9C%AC%E7%BB%98%E5%9B%BE%E6%A0%B7%E5%BC%8F)
    - [2、setTextAlign(Paint.Align align) 文字的对齐方式](#2settextalignpaintalign-align-%E6%96%87%E5%AD%97%E7%9A%84%E5%AF%B9%E9%BD%90%E6%96%B9%E5%BC%8F)
    - [3、文字样式设置](#3%E6%96%87%E5%AD%97%E6%A0%B7%E5%BC%8F%E8%AE%BE%E7%BD%AE)
    - [4、文字倾斜度设置](#4%E6%96%87%E5%AD%97%E5%80%BE%E6%96%9C%E5%BA%A6%E8%AE%BE%E7%BD%AE)
    - [5、水平拉伸设置](#5%E6%B0%B4%E5%B9%B3%E6%8B%89%E4%BC%B8%E8%AE%BE%E7%BD%AE)
  - [canvas绘制文字](#canvas%E7%BB%98%E5%88%B6%E6%96%87%E5%AD%97)
    - [1、drawText](#1drawtext)
    - [2、drawPosText](#2drawpostext)
    - [3、drawTextOnPath](#3drawtextonpath)
    - [4、Typeface（字体样式设置）](#4typeface%E5%AD%97%E4%BD%93%E6%A0%B7%E5%BC%8F%E8%AE%BE%E7%BD%AE)
      - [a、系统字体](#a%E7%B3%BB%E7%BB%9F%E5%AD%97%E4%BD%93)
      - [b、自定义字体](#b%E8%87%AA%E5%AE%9A%E4%B9%89%E5%AD%97%E4%BD%93)
  - [引用：](#%E5%BC%95%E7%94%A8-2)
- [baseLine和FontMetrics](#baseline%E5%92%8Cfontmetrics)
  - [一、baseLine 基线](#%E4%B8%80baseline-%E5%9F%BA%E7%BA%BF)
    - [1、canvas.drawText()](#1canvasdrawtext)
    - [2、mPaint.setTextAlign(Paint.Align.XXX);](#2mpaintsettextalignpaintalignxxx)
      - [（1）、Paint.Align.LEFT](#1paintalignleft)
      - [（2）、Paint.Align.CENTER](#2paintaligncenter)
      - [（3）、Paint.Align.RIGHT](#3paintalignright)
  - [二、FontMetrics](#%E4%BA%8Cfontmetrics)
    - [1、获取实例](#1%E8%8E%B7%E5%8F%96%E5%AE%9E%E4%BE%8B)
    - [2、成员变量](#2%E6%88%90%E5%91%98%E5%8F%98%E9%87%8F)
    - [3、绘制ascent，descent，top，bottom线](#3%E7%BB%98%E5%88%B6ascentdescenttopbottom%E7%BA%BF)
    - [4、绘制文字最小矩形、文字宽度、文字高度](#4%E7%BB%98%E5%88%B6%E6%96%87%E5%AD%97%E6%9C%80%E5%B0%8F%E7%9F%A9%E5%BD%A2%E6%96%87%E5%AD%97%E5%AE%BD%E5%BA%A6%E6%96%87%E5%AD%97%E9%AB%98%E5%BA%A6)
      - [（1）绘制文字最小矩形](#1%E7%BB%98%E5%88%B6%E6%96%87%E5%AD%97%E6%9C%80%E5%B0%8F%E7%9F%A9%E5%BD%A2)
      - [（2）文字高度](#2%E6%96%87%E5%AD%97%E9%AB%98%E5%BA%A6)
      - [（3）文字宽度](#3%E6%96%87%E5%AD%97%E5%AE%BD%E5%BA%A6)
    - [已知中线，获取baseline](#%E5%B7%B2%E7%9F%A5%E4%B8%AD%E7%BA%BF%E8%8E%B7%E5%8F%96baseline)
      - [结论](#%E7%BB%93%E8%AE%BA)
  - [引用：](#%E5%BC%95%E7%94%A8-3)
- [Canvas](#canvas-1)
  - [一、什么是Canvas？](#%E4%B8%80%E4%BB%80%E4%B9%88%E6%98%AFcanvas)
  - [二、Canvas 绘图](#%E4%BA%8Ccanvas-%E7%BB%98%E5%9B%BE)
  - [三、Canvas 的变换与操作](#%E4%B8%89canvas-%E7%9A%84%E5%8F%98%E6%8D%A2%E4%B8%8E%E6%93%8D%E4%BD%9C)
    - [1、平移（translate）](#1%E5%B9%B3%E7%A7%BBtranslate)
    - [屏幕显示与Canvas的关系](#%E5%B1%8F%E5%B9%95%E6%98%BE%E7%A4%BA%E4%B8%8Ecanvas%E7%9A%84%E5%85%B3%E7%B3%BB)
      - [总结：](#%E6%80%BB%E7%BB%93-2)
    - [2、旋转（Rotate）](#2%E6%97%8B%E8%BD%ACrotate)
    - [3、缩放（scale ）](#3%E7%BC%A9%E6%94%BEscale-)
        - [第一个构造函数：](#%E7%AC%AC%E4%B8%80%E4%B8%AA%E6%9E%84%E9%80%A0%E5%87%BD%E6%95%B0)
        - [第二个构造函数：](#%E7%AC%AC%E4%BA%8C%E4%B8%AA%E6%9E%84%E9%80%A0%E5%87%BD%E6%95%B0)
          - [原理](#%E5%8E%9F%E7%90%86)
    - [4、错切（skew）](#4%E9%94%99%E5%88%87skew)
    - [5、裁剪画布（clip系列函数）](#5%E8%A3%81%E5%89%AA%E7%94%BB%E5%B8%83clip%E7%B3%BB%E5%88%97%E5%87%BD%E6%95%B0)
    - [6、画布的保存与恢复（save()、restore()）](#6%E7%94%BB%E5%B8%83%E7%9A%84%E4%BF%9D%E5%AD%98%E4%B8%8E%E6%81%A2%E5%A4%8Dsaverestore)
    - [7、saveLayer](#7savelayer)
      - [**saveFlags**](#saveflags)
        - [**(1) MATRIX_SAVE_FLAG**](#1-matrix_save_flag)
          - [**save方法**](#save%E6%96%B9%E6%B3%95)
          - [**saveLayer方法**](#savelayer%E6%96%B9%E6%B3%95)
        - [**(2) CLIP_SAVE_FLAG**](#2-clip_save_flag)
        - [**(3) FULL_COLOR_LAYER_SAVE_FLAG 和 HAS_ALPHA_LAYER_SAVE_FLAG**](#3-full_color_layer_save_flag-%E5%92%8C-has_alpha_layer_save_flag)
        - [**(4) CLIP_TO_LAYER_SAVE_**](#4-clip_to_layer_save_)
  - [习题](#%E4%B9%A0%E9%A2%98)
    - [画一个表盘](#%E7%94%BB%E4%B8%80%E4%B8%AA%E8%A1%A8%E7%9B%98)
    - [画一个正方形螺旋图](#%E7%94%BB%E4%B8%80%E4%B8%AA%E6%AD%A3%E6%96%B9%E5%BD%A2%E8%9E%BA%E6%97%8B%E5%9B%BE)
  - [引用：](#%E5%BC%95%E7%94%A8-4)
- [综合习题](#%E7%BB%BC%E5%90%88%E4%B9%A0%E9%A2%98)
  - [实现图片圆角带边框的效果](#%E5%AE%9E%E7%8E%B0%E5%9B%BE%E7%89%87%E5%9C%86%E8%A7%92%E5%B8%A6%E8%BE%B9%E6%A1%86%E7%9A%84%E6%95%88%E6%9E%9C)
    - [引用：](#%E5%BC%95%E7%94%A8-5)
- [【Android 动画】](#android-%E5%8A%A8%E7%94%BB)
- [动画分类](#%E5%8A%A8%E7%94%BB%E5%88%86%E7%B1%BB)
  - [补间动画(Tween动画)](#%E8%A1%A5%E9%97%B4%E5%8A%A8%E7%94%BBtween%E5%8A%A8%E7%94%BB)
  - [帧动画(Frame 动画)](#%E5%B8%A7%E5%8A%A8%E7%94%BBframe-%E5%8A%A8%E7%94%BB)
  - [属性动画(Property animation)](#%E5%B1%9E%E6%80%A7%E5%8A%A8%E7%94%BBproperty-animation)
- [补间动画(Tween动画)](#%E8%A1%A5%E9%97%B4%E5%8A%A8%E7%94%BBtween%E5%8A%A8%E7%94%BB-1)
  - [1、透明度动画AlphaAnimation](#1%E9%80%8F%E6%98%8E%E5%BA%A6%E5%8A%A8%E7%94%BBalphaanimation)
  - [2、平移动画TranslateAnimation](#2%E5%B9%B3%E7%A7%BB%E5%8A%A8%E7%94%BBtranslateanimation)
  - [3、缩放动画ScaleAnimation](#3%E7%BC%A9%E6%94%BE%E5%8A%A8%E7%94%BBscaleanimation)
  - [4、旋转动画 rotate](#4%E6%97%8B%E8%BD%AC%E5%8A%A8%E7%94%BB-rotate)
    - [创建一个Animation类型的XML文件：](#%E5%88%9B%E5%BB%BA%E4%B8%80%E4%B8%AAanimation%E7%B1%BB%E5%9E%8B%E7%9A%84xml%E6%96%87%E4%BB%B6)
    - [java中导入xml动画](#java%E4%B8%AD%E5%AF%BC%E5%85%A5xml%E5%8A%A8%E7%94%BB)
  - [复合动画](#%E5%A4%8D%E5%90%88%E5%8A%A8%E7%94%BB)
- [帧动画(Frame 动画)](#%E5%B8%A7%E5%8A%A8%E7%94%BBframe-%E5%8A%A8%E7%94%BB-1)
  - [方式一；使用XML的方式；](#%E6%96%B9%E5%BC%8F%E4%B8%80%E4%BD%BF%E7%94%A8xml%E7%9A%84%E6%96%B9%E5%BC%8F)
  - [方式二：使用代码的方式进行；](#%E6%96%B9%E5%BC%8F%E4%BA%8C%E4%BD%BF%E7%94%A8%E4%BB%A3%E7%A0%81%E7%9A%84%E6%96%B9%E5%BC%8F%E8%BF%9B%E8%A1%8C)
- [属性动画(Property animation)](#%E5%B1%9E%E6%80%A7%E5%8A%A8%E7%94%BBproperty-animation-1)
  - [为什么要引入属性动画？](#%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E5%BC%95%E5%85%A5%E5%B1%9E%E6%80%A7%E5%8A%A8%E7%94%BB)
  - [ValueAnimator](#valueanimator)
  - [ObjectAnimator](#objectanimator)
  - [组合动画-AnimatorSet](#%E7%BB%84%E5%90%88%E5%8A%A8%E7%94%BB-animatorset)
    - [AnimatorSet.Builder](#animatorsetbuilder)
  - [Animator监听器](#animator%E7%9B%91%E5%90%AC%E5%99%A8)
    - [AnimatorListener](#animatorlistener)
    - [AnimatorListenerAdapter](#animatorlisteneradapter)
  - [使用XML编写动画](#%E4%BD%BF%E7%94%A8xml%E7%BC%96%E5%86%99%E5%8A%A8%E7%94%BB)
    - [XML标签](#xml%E6%A0%87%E7%AD%BE)
      - [animato](#animato)
      - [objectAnimator](#objectanimator)
      - [set](#set-1)
    - [在代码中加载XML文件-AnimatorInflater](#%E5%9C%A8%E4%BB%A3%E7%A0%81%E4%B8%AD%E5%8A%A0%E8%BD%BDxml%E6%96%87%E4%BB%B6-animatorinflater)
  - [ViewPropertyAnimator](#viewpropertyanimator)
    - [用法](#%E7%94%A8%E6%B3%95)
    - [注意](#%E6%B3%A8%E6%84%8F)
- [属性动画之Evaluator](#%E5%B1%9E%E6%80%A7%E5%8A%A8%E7%94%BB%E4%B9%8Bevaluator)
  - [1、概述](#1%E6%A6%82%E8%BF%B0)
  - [2、各种Evaluator](#2%E5%90%84%E7%A7%8Devaluator)
    - [Evaluator使用](#evaluator%E4%BD%BF%E7%94%A8)
    - [IntEvaluator内部实现](#intevaluator%E5%86%85%E9%83%A8%E5%AE%9E%E7%8E%B0)
  - [3、自定义Evalutor](#3%E8%87%AA%E5%AE%9A%E4%B9%89evalutor)
    - [（1）、简单实现MyEvalutor](#1%E7%AE%80%E5%8D%95%E5%AE%9E%E7%8E%B0myevalutor)
    - [（2）、实现倒序输出实例](#2%E5%AE%9E%E7%8E%B0%E5%80%92%E5%BA%8F%E8%BE%93%E5%87%BA%E5%AE%9E%E4%BE%8B)
  - [4、关于ArgbEvalutor](#4%E5%85%B3%E4%BA%8Eargbevalutor)
    - [1、使用ArgbEvalutor](#1%E4%BD%BF%E7%94%A8argbevalutor)
    - [2、ArgbEvalutor的实现原理](#2argbevalutor%E7%9A%84%E5%AE%9E%E7%8E%B0%E5%8E%9F%E7%90%86)
- [属性动画之ValueAnimator](#%E5%B1%9E%E6%80%A7%E5%8A%A8%E7%94%BB%E4%B9%8Bvalueanimator)
  - [概述](#%E6%A6%82%E8%BF%B0-2)
    - [常用方法](#%E5%B8%B8%E7%94%A8%E6%96%B9%E6%B3%95)
  - [ofObject](#ofobject)
    - [概述](#%E6%A6%82%E8%BF%B0-3)
    - [基本使用：自定义View A-Z字母变化](#%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8%E8%87%AA%E5%AE%9A%E4%B9%89view-a-z%E5%AD%97%E6%AF%8D%E5%8F%98%E5%8C%96)
      - [第一，构造时：](#%E7%AC%AC%E4%B8%80%E6%9E%84%E9%80%A0%E6%97%B6)
      - [第二：看监听：](#%E7%AC%AC%E4%BA%8C%E7%9C%8B%E7%9B%91%E5%90%AC)
      - [第三：插值器](#%E7%AC%AC%E4%B8%89%E6%8F%92%E5%80%BC%E5%99%A8)
      - [ASCII码中数值与字符的转换方法](#ascii%E7%A0%81%E4%B8%AD%E6%95%B0%E5%80%BC%E4%B8%8E%E5%AD%97%E7%AC%A6%E7%9A%84%E8%BD%AC%E6%8D%A2%E6%96%B9%E6%B3%95)
      - [CharEvaluator的实现：](#charevaluator%E7%9A%84%E5%AE%9E%E7%8E%B0)
    - [高级使用：自定义弹性圆的伸缩特效](#%E9%AB%98%E7%BA%A7%E4%BD%BF%E7%94%A8%E8%87%AA%E5%AE%9A%E4%B9%89%E5%BC%B9%E6%80%A7%E5%9C%86%E7%9A%84%E4%BC%B8%E7%BC%A9%E7%89%B9%E6%95%88)
      - [1、首先，我们自定义一个类Point:](#1%E9%A6%96%E5%85%88%E6%88%91%E4%BB%AC%E8%87%AA%E5%AE%9A%E4%B9%89%E4%B8%80%E4%B8%AA%E7%B1%BBpoint)
      - [2、然后我们自定义一个View:MyPointView](#2%E7%84%B6%E5%90%8E%E6%88%91%E4%BB%AC%E8%87%AA%E5%AE%9A%E4%B9%89%E4%B8%80%E4%B8%AAviewmypointview)
        - [(1)、doPointAnim()函数](#1dopointanim%E5%87%BD%E6%95%B0)
        - [(2)、OnDraw()函数](#2ondraw%E5%87%BD%E6%95%B0)
        - [(3)、PointEvaluator](#3pointevaluator)
      - [3、使用MyPointView](#3%E4%BD%BF%E7%94%A8mypointview)
    - [习题：对自定义View使用动画](#%E4%B9%A0%E9%A2%98%E5%AF%B9%E8%87%AA%E5%AE%9A%E4%B9%89view%E4%BD%BF%E7%94%A8%E5%8A%A8%E7%94%BB)
      - [实现目标](#%E5%AE%9E%E7%8E%B0%E7%9B%AE%E6%A0%87)
      - [实现步骤](#%E5%AE%9E%E7%8E%B0%E6%AD%A5%E9%AA%A4-2)
        - [先定义一个Point类：](#%E5%85%88%E5%AE%9A%E4%B9%89%E4%B8%80%E4%B8%AApoint%E7%B1%BB)
        - [自定义TypeEvaluator：](#%E8%87%AA%E5%AE%9A%E4%B9%89typeevaluator)
        - [自定义MyAnimView：](#%E8%87%AA%E5%AE%9A%E4%B9%89myanimview)
        - [在布局文件当中引入这个自定义控件：](#%E5%9C%A8%E5%B8%83%E5%B1%80%E6%96%87%E4%BB%B6%E5%BD%93%E4%B8%AD%E5%BC%95%E5%85%A5%E8%BF%99%E4%B8%AA%E8%87%AA%E5%AE%9A%E4%B9%89%E6%8E%A7%E4%BB%B6)
- [属性动画之ObjectAnimator](#%E5%B1%9E%E6%80%A7%E5%8A%A8%E7%94%BB%E4%B9%8Bobjectanimator)
  - [概述](#%E6%A6%82%E8%BF%B0-4)
    - [1、引入](#1%E5%BC%95%E5%85%A5)
    - [2、setter函数](#2setter%E5%87%BD%E6%95%B0)
      - [使用注意：](#%E4%BD%BF%E7%94%A8%E6%B3%A8%E6%84%8F)
      - [(1)、setRotationX、setRotationY与setRotation](#1setrotationxsetrotationy%E4%B8%8Esetrotation)
      - [（2）、setTranslationX与setTranslationY](#2settranslationx%E4%B8%8Esettranslationy)
      - [(3)、setScaleX与setScaleY](#3setscalex%E4%B8%8Esetscaley)
    - [3、ObjectAnimator动画原理](#3objectanimator%E5%8A%A8%E7%94%BB%E5%8E%9F%E7%90%86)
  - [自定义ObjectAnimator属性](#%E8%87%AA%E5%AE%9A%E4%B9%89objectanimator%E5%B1%9E%E6%80%A7)
    - [1、保存圆形信息类——Point](#1%E4%BF%9D%E5%AD%98%E5%9C%86%E5%BD%A2%E4%BF%A1%E6%81%AF%E7%B1%BBpoint)
    - [2、自定义控件——MyPointView](#2%E8%87%AA%E5%AE%9A%E4%B9%89%E6%8E%A7%E4%BB%B6mypointview)
    - [3、使用MyPointView](#3%E4%BD%BF%E7%94%A8mypointview-1)
    - [4、注意——何时需要实现对应属性的get函数](#4%E6%B3%A8%E6%84%8F%E4%BD%95%E6%97%B6%E9%9C%80%E8%A6%81%E5%AE%9E%E7%8E%B0%E5%AF%B9%E5%BA%94%E5%B1%9E%E6%80%A7%E7%9A%84get%E5%87%BD%E6%95%B0)
  - [常用函数](#%E5%B8%B8%E7%94%A8%E5%87%BD%E6%95%B0)
    - [1、使用ArgbEvaluator](#1%E4%BD%BF%E7%94%A8argbevaluator)
    - [2、其它函数](#2%E5%85%B6%E5%AE%83%E5%87%BD%E6%95%B0)
      - [（1）、常用函数](#1%E5%B8%B8%E7%94%A8%E5%87%BD%E6%95%B0)
      - [（2）、监听器相关](#2%E7%9B%91%E5%90%AC%E5%99%A8%E7%9B%B8%E5%85%B3)
      - [（3）、插值器与Evaluator](#3%E6%8F%92%E5%80%BC%E5%99%A8%E4%B8%8Eevaluator)
  - [习题：自定义View一边动一边变色](#%E4%B9%A0%E9%A2%98%E8%87%AA%E5%AE%9A%E4%B9%89view%E4%B8%80%E8%BE%B9%E5%8A%A8%E4%B8%80%E8%BE%B9%E5%8F%98%E8%89%B2)
    - [实现目标](#%E5%AE%9E%E7%8E%B0%E7%9B%AE%E6%A0%87-1)
    - [实现步骤](#%E5%AE%9E%E7%8E%B0%E6%AD%A5%E9%AA%A4-3)
      - [自定义MyAnimView属性](#%E8%87%AA%E5%AE%9A%E4%B9%89myanimview%E5%B1%9E%E6%80%A7)
      - [自定义TypeEvaluator：](#%E8%87%AA%E5%AE%9A%E4%B9%89typeevaluator-1)
      - [调用](#%E8%B0%83%E7%94%A8)
- [Interpolator插值器](#interpolator%E6%8F%92%E5%80%BC%E5%99%A8)
  - [Interpolator常见实现类](#interpolator%E5%B8%B8%E8%A7%81%E5%AE%9E%E7%8E%B0%E7%B1%BB)
  - [补间动画中使用Interpolator](#%E8%A1%A5%E9%97%B4%E5%8A%A8%E7%94%BB%E4%B8%AD%E4%BD%BF%E7%94%A8interpolator)
    - [scale标签](#scale%E6%A0%87%E7%AD%BE)
    - [rotate标签](#rotate%E6%A0%87%E7%AD%BE)
    - [alpha标签](#alpha%E6%A0%87%E7%AD%BE)
    - [translate标签](#translate%E6%A0%87%E7%AD%BE)
  - [AccelerateDecelerateInterpolator](#acceleratedecelerateinterpolator)
  - [BounceInterpolator](#bounceinterpolator)
  - [Interpolator内部实现机制](#interpolator%E5%86%85%E9%83%A8%E5%AE%9E%E7%8E%B0%E6%9C%BA%E5%88%B6)
  - [自定义Interpolator](#%E8%87%AA%E5%AE%9A%E4%B9%89interpolator)
- [动画常见问题](#%E5%8A%A8%E7%94%BB%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98)
  - [修改 Activity 进入和退出动画](#%E4%BF%AE%E6%94%B9-activity-%E8%BF%9B%E5%85%A5%E5%92%8C%E9%80%80%E5%87%BA%E5%8A%A8%E7%94%BB)
  - [属性动画，例如一个 button 从 A 移动到 B 点，B 点还是可以响应点击事件，这个原理是什么？](#%E5%B1%9E%E6%80%A7%E5%8A%A8%E7%94%BB%E4%BE%8B%E5%A6%82%E4%B8%80%E4%B8%AA-button-%E4%BB%8E-a-%E7%A7%BB%E5%8A%A8%E5%88%B0-b-%E7%82%B9b-%E7%82%B9%E8%BF%98%E6%98%AF%E5%8F%AF%E4%BB%A5%E5%93%8D%E5%BA%94%E7%82%B9%E5%87%BB%E4%BA%8B%E4%BB%B6%E8%BF%99%E4%B8%AA%E5%8E%9F%E7%90%86%E6%98%AF%E4%BB%80%E4%B9%88)
- [【Android MVP】](#android-mvp)
  - [一、老的MVC架构](#%E4%B8%80%E8%80%81%E7%9A%84mvc%E6%9E%B6%E6%9E%84)
  - [二、新的MVP架构](#%E4%BA%8C%E6%96%B0%E7%9A%84mvp%E6%9E%B6%E6%9E%84)
  - [三、一个demo](#%E4%B8%89%E4%B8%80%E4%B8%AAdemo)
    - [（1）RequestBiz](#1requestbiz)
    - [（2）传统MVC的写法](#2%E4%BC%A0%E7%BB%9Fmvc%E7%9A%84%E5%86%99%E6%B3%95)
    - [（3）MVP的写法](#3mvp%E7%9A%84%E5%86%99%E6%B3%95)
  - [四、一点心得](#%E5%9B%9B%E4%B8%80%E7%82%B9%E5%BF%83%E5%BE%97)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

[TOC]

【Android Activity】
【
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
当前activity的窗口获得或失去焦点的时候会调用。
这个函数是最好的方向标对于activity是否对用户可见，它默认的实现是清除键跟踪状态，所以应该总是被调用。
它也提供了全局的焦点状态，它的管理是独立于activity生命周期的。当焦点改变时一般都伴随着生命周期的改变，你不应该依赖onWindowFocusChanged 调用和其他生命周期的方法(例如onResume) 的先后顺序，来处理我们要做的事情。
通常的规则是，当一个activity被唤醒，那么就拥有窗口焦点。除非这个窗口已经显示了对话框或者其他弹出框抢占焦点，这个窗口只有等到这些对话框关闭后，才能获取焦点，同理，当系统显示系统级的窗口，系统级的窗口会临时的获取窗口输入焦点同时不会暂停前景 activity。

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
启动方式|启动Activity|执行函数|task中Activity
-|-|-|-
Standard | StandardActivity@96267d3 | onCreate
| | | onStart
| | | onResume TaskId: 187 | StandardActivity@96267d3 TaskId【187】
SingleInstance | SingleInstanceActivity@a2e46b7 | onCreate
| | | onStart
| | | onResume TaskId: 188 | StandardActivity@96267d3【187】<br>SingleInstanceActivity@a2e46b7【188】
SingleInstance | SingleInstanceActivity@a2e46b7 | **onNewIntent**
| | | onResume TaskId: 188 | StandardActivity@96267d3【187】<br>SingleInstanceActivity@a2e46b7【188】
SingleTask | SingleTaskActivity@e0f0b0a | onCreate
| | | onStart
| | | onResume TaskId: 187 | SingleTaskActivity@e0f0b0a<br>StandardActivity@96267d3【187】<br>SingleInstanceActivity@a2e46b7【188】
SingleTask | SingleTaskActivity@e0f0b0a | **onNewIntent**
| | | onResume TaskId: 187 | SingleTaskActivity@e0f0b0a<br>StandardActivity@96267d3【187】<br>SingleInstanceActivity@a2e46b7【188】
SingleTop | SingleTopActivity@21bfd59 | onCreate
| | | onStart
| | | onResume TaskId: 187 | SingleTopActivity@21bfd59<br>SingleTaskActivity@e0f0b0a<br>StandardActivity@96267d3【187】<br>SingleInstanceActivity@a2e46b7【188】
SingleTop | SingleTopActivity@21bfd59 | **onNewIntent**
| | | onResume TaskId: 187 | SingleTopActivity@21bfd59<br>SingleTaskActivity@e0f0b0a<br>StandardActivity@96267d3【187】<br>SingleInstanceActivity@a2e46b7【188】
Standard | StandardActivity@9f84b54 | onCreate
| | | onStart
| | | onResume TaskId: 187 | StandardActivity@9f84b54<br>SingleTopActivity@21bfd59<br>SingleTaskActivity@e0f0b0a<br>StandardActivity@96267d3【187】<br>SingleInstanceActivity@a2e46b7【188】
Standard | StandardActivity@a364129 | onCreate
| | | onStart
| | | onResume TaskId: 187 | StandardActivity@a364129<br>StandardActivity@9f84b54<br>SingleTopActivity@21bfd59<br>SingleTaskActivity@e0f0b0a<br>StandardActivity@96267d3【187】<br>SingleInstanceActivity@a2e46b7【188】
SingleTop | SingleTopActivity@a9c182a | onCreate
| | | onStart
| | | onResume TaskId: 187 | SingleTopActivity@a9c182a<br>StandardActivity@a364129<br>StandardActivity@9f84b54<br>SingleTopActivity@21bfd59<br>SingleTaskActivity@e0f0b0a<br>StandardActivity@96267d3【187】<br>SingleInstanceActivity@a2e46b7【188】
SingleTask | SingleTopActivity@21bfd59 | onDestroy
| | StandardActivity@9f84b54 | onDestroy
| | StandardActivity@a364129 | onDestroy
| | SingleTaskActivity@e0f0b0a | onNewIntent
| | | onRestart
| | | onStart
| | | onResume TaskId: 187
| | SingleTopActivity@a9c182a | onDestroy | SingleTaskActivity@e0f0b0a<br>StandardActivity@96267d3【187】<br>SingleInstanceActivity@a2e46b7【188】
back | StandardActivity@96267d3 | onRestart
| | | onStart
| | | onResume TaskId: 187
| | SingleTaskActivity@e0f0b0a | onDestroy | SingleTaskActivity@e0f0b0a<br>StandardActivity@96267d3【187】<br>SingleInstanceActivity@a2e46b7【188】
back | StandardActivity@96267d3 | onDestroy
| | SingleInstanceActivity@a2e46b7 | onRestart
| | | onStart
| | | onResume TaskId: 188 | SingleInstanceActivity@a2e46b7【188】

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
】


【Android 广播】
【
# 【Android 广播】


<span id="BroadcastReceiver简介"/>
## BroadcastReceiver简介
**BroadcastReceiver**（广播接收器），是一个全局的监听器，属于 Android **四大组件之一**。
`Android` 广播分为两个角色：广播发送者、广播接收者。

在 Android 中，Broadcast 是一种广泛运用的在应用程序之间传输信息的机制。而 BroadcastReceiver 是对发送出来的 Broadcast 进行过滤接受并响应的一类组件。

广播接收者（BroadcastReceiver）用于接收广播 Intent 的, 广播 Intent 的发送是通过调用 sendBroadcast/sendOrderedBroadcast 来实现的。通常一个广播 Intent 可以被订阅了此 Intent 的多个广播接收者所接收。

**广播机制是一个典型的发布—订阅模式，也就是我们所说的观察者模式**。广播最大的特点就是发送方并不关心接收方是否接到数据，也不关心接收方是如何处理数据的，通过这样的形式来达到接、收双方的完全解耦合。



### BroadcastReceiver作用
监听 / 接收 应用 `App` 发出的广播消息，并 做出响应


### 广播应用场景

- `Android`不同组件间的通信（含 ：应用内 / 不同应用之间）
- 多线程通信
- 与 `Android` 系统在特定情况下的通信

> 如：电话呼入时、网络可用时



## 实现原理

#### 采用的模型

- `Android`中的广播使用了设计模式中的**观察者模式**：基于消息的发布 / 订阅事件模型

> 因此，Android将广播的**发送者 和 接收者 解耦**，使得系统方便集成，更易扩展

#### 模型讲解

- 模型中有3个角色：
  1. 消息订阅者（广播接收者）
  2. 消息发布者（广播发布者）
  3. 消息中心（`AMS`，即`Activity Manager Service`）
- 示意图 & 原理如下

[![模型讲解](http://img.blog.csdn.net/20180126210210820?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)](http://img.blog.csdn.net/20180126210210820?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)



## 使用流程

- 使用流程如下：

[![流程](http://img.blog.csdn.net/20180126210204101?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)](http://img.blog.csdn.net/20180126210204101?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


- 下面，我将一步步介绍如何使用`BroadcastReceiver`
即上图中的 **开发者手动完成部分**

#### 1、自定义广播接收器BroadcastReceiver

- 继承**BroadcastReceivre**基类
- 必须复写抽象方法**onReceive()**方法
 1. 广播接收器接**收到**相应**广播**后，会**自动回调 onReceive**() 方法
 2. 一般情况下，onReceive方法会涉及 与其他组件之间的交互，如发送Notification、启动Service等
 3. **默认**情况下，广播接收器**运行在 UI 线程**，因此，onReceive()方法不能执行耗时操作，否则将导致ANR

- 代码范例*mBroadcastReceiver.java*

```java
// 继承BroadcastReceivre基类
public class mBroadcastReceiver extends BroadcastReceiver {
  // 复写onReceive()方法
  // 接收到广播后，则自动调用该方法
  @Override
  public void onReceive(Context context, Intent intent) {
   //写入接收广播后的操作
    }
}
```

<span id="2、广播接收器注册"/>
#### 2、广播接收器注册
注册的方式分为两种：静态注册、动态注册

##### 1）静态注册
- 注册方式：在**AndroidManifest**.xml里通过**`<receive>`**标签声明
- 属性说明：
```java
<receiver 
    android:enabled=["true" | "false"]
//exported：此broadcastReceiver能否接收其他App的发出的广播
//默认值是由receiver中有无intent-filter决定的：如果有intent-filter，默认值为true，否则为false
    android:exported=["true" | "false"]
    android:icon="drawable resource"
    android:label="string resource"
//继承BroadcastReceiver子类的类名
    android:name=".mBroadcastReceiver"
//具有相应权限的广播发送者发送的广播才能被此BroadcastReceiver所接收；
    android:permission="string"
//BroadcastReceiver运行所处的进程
//默认为app的进程，可以指定独立的进程
//注：Android四大基本组件都可以通过此属性指定自己的独立进程
    android:process="string" >

//用于指定此广播接收器将接收的广播类型
//本示例中给出的是用于接收网络状态改变时发出的广播
 <intent-filter>
<action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
    </intent-filter>
</receiver>
```

- 注册示例

```java
<receiver 
    //此广播接收者类是mBroadcastReceiver
    android:name=".mBroadcastReceiver" >
    //用于接收网络状态改变时发出的广播
    <intent-filter>
        <action android:name="android.net.conn.CONNECTIVITY_CHANGE" />
    </intent-filter>
</receiver>
```

当此 `App`首次启动时，系统会**自动**实例化`mBroadcastReceiver`类，并注册到系统中。

##### 2）动态注册

- 注册方式：在代码中调用**Context.registerReceiver()**方法
- 具体代码如下：

```java
// 选择在Activity生命周期方法中的onResume()中注册
@Override
  protected void onResume(){
      super.onResume();
    // 1. 实例化BroadcastReceiver子类 &  IntentFilter
     mBroadcastReceiver mBroadcastReceiver = new mBroadcastReceiver();
     IntentFilter intentFilter = new IntentFilter();
    // 2. 设置接收广播的类型
    intentFilter.addAction(android.net.conn.CONNECTIVITY_CHANGE);
    // 3. 动态注册：调用Context的registerReceiver（）方法
     registerReceiver(mBroadcastReceiver, intentFilter);
 }

// 注册广播后，要在相应位置记得销毁广播
// 即在onPause() 中unregisterReceiver(mBroadcastReceiver)
// 当此Activity实例化时，会动态将MyBroadcastReceiver注册到系统中
// 当此Activity销毁时，动态注册的MyBroadcastReceiver将不再接收到相应的广播。
 @Override
 protected void onPause() {
     super.onPause();
      //销毁在onResume()方法中的广播
     unregisterReceiver(mBroadcastReceiver);
     }
}
```

##### 特别注意

- 动态广播**最好**在Activity 的 **onResume()注册、onPause()注销**。
- 原因：
  1. 对于动态广播，有注册就必然得有注销，否则会导致**内存泄露**
 重复注册、重复注销也不允许

1. Activity生命周期如下：
![Activity生命周期](http://img.blog.csdn.net/20180126210158278?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
Activity生命周期的方法是成对出现的：

- onCreate() & onDestory()
- onStart() & onStop()
- onResume() & onPause()

**在onResume()注册、onPause()注销是因为onPause()在App死亡前一定会被执行，从而保证广播在App死亡前一定会被注销，从而防止内存泄露。**

> 1. 不在onCreate() & onDestory() 或 onStart() & onStop()注册、注销是因为：当系统因为内存不足（优先级更高的应用需要内存，请看上图红框）要回收Activity占用的资源时，Activity在执行完onPause()方法后就会被销毁，有些生命周期方法onStop()，onDestory()就不会执行。当再回到此Activity时，是从onCreate方法开始执行。
> 2. 假设我们将广播的注销放在onStop()，onDestory()方法里的话，有可能在Activity被销毁后还未执行onStop()，onDestory()方法，即广播仍还未注销，从而导致内存泄露。
> 3. 但是，onPause()一定会被执行，从而保证了广播在App死亡前一定会被注销，从而防止内存泄露。

PS. 服务Service相反，切勿在 Activity 的 onResume() 和 onPause() 期间绑定和取消绑定服务。

##### 两种注册方式的区别
[![两种注册方式的区别](http://img.blog.csdn.net/20180126210147290?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)](http://img.blog.csdn.net/20180126210147290?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


#### 3、发送广播
- 广播 是 **用”意图（Intent）“标识**
- 定义广播的本质 = 定义广播所具备的“意图（Intent）”
- 广播发送 = 广播发送者 将此广播的“意图（Intent）”通过**sendBroadcast()方法**发送出去

在Android系统中，根据广播的执行顺序不同，可将其分为有序广播和无序广播。

## 无序广播

### 1、普通广播（Normal Broadcast）
**普通广播（Normal Broadcast）**即 开发者自身定义 intent的广播（最常用）。
普通广播是完全**异步**的，通过Context的sendBroadcast()方法来发送，消息传递效率比较高，但所有receivers（接收器）的执行顺序不确定。
缺点是：接收者不能将处理结果传递给下一个接收者，并且无法终止广播Intent的传播，直到没有与之匹配的广播接收器为止。下面以自定义的普通广播进行演示

#### ① 自定义广播接收器
只要继承BroadcastReceiver并实现onReceive()方法
```java
public class MyBroadcastReceiver extends BroadcastReceiver {
    @Override
    public void onReceive(Context context, Intent intent) {
        Log.e("receive","onReceive");
    }
}
```

#### ② 注册广播接收器
BroadcastReceiver是四大组件之一，所以毫不疑问需要注册，BroadcastReceiver的注册有两种方法：

- 通过manifests配置
- 通过代码动态配置

**1、方法一：通过manifests配置**

```xml
<receiver android:name=".BroadcastReceiver.MyBroadcastReceiver">
    <intent-filter>
        <action android:name="com.handsome.hensen" />
    </intent-filter>
</receiver>
```

这里需要加入**intent-filter**的**action**中的**name**属性，表示我们监听的内容。
当有广播发送时，需要**判断该广播是否和我们监听的内容一致**，如果一致则接收
>若发送广播有相应权限，那么广播接收者也需要相应权限

**2、方法二：通过代码动态配置**
```java
//创建广播
MyBroadcastReceiver receiver = new MyBroadcastReceiver();
//注册广播
registerReceiver(receiver, new IntentFilter("com.handsome.hensen"));
```
如果你是使用动态注册广播的则需要在Activity的onDestroy的时候注销广播接收器
```java
@Override
protected void onDestroy() {
    unregisterReceiver(receiver);
    super.onDestroy();
}
```

#### ③ 发送广播

这里我们以一个按钮来发送广播，通过sendBroadcast()方法发送我们的创建的Intent自定义广播

```
final Intent intent = new Intent();
//广播内容
intent.setAction("com.handsome.hensen");

bt_send.setOnClickListener(new View.OnClickListener() {
   @Override
   public void onClick(View v) {
       sendBroadcast(intent);
   }
});
```

**运行代码**
运行程序后，我们点击发送广播。我们以Log信息来验证发出的广播被我们准确的接收
```
11-25 10:27:43.341 5188-5188/com.handsome.boke2 E/receive: onReceive
```


### 2、系统广播（System Broadcast）

- Android中内置了多个系统广播：只要涉及到手机的基本操作（如开机、网络状态变化、拍照等等），都会发出相应的广播
- **每个广播都有特定的Intent - Filter**（包括具体的action），Android系统广播action如下：

| 系统操作                                     | action                               |
| ---------------------------------------- | ------------------------------------ |
| 监听网络变化                                   | android.net.conn.CONNECTIVITY_CHANGE |
| 关闭或打开飞行模式                                | Intent.ACTION_AIRPLANE_MODE_CHANGED  |
| 充电时或电量发生变化                               | Intent.ACTION_BATTERY_CHANGED        |
| 电池电量低                                    | Intent.ACTION_BATTERY_LOW            |
| 电池电量充足（即从电量低变化到饱满时会发出广播                  | Intent.ACTION_BATTERY_OKAY           |
| 系统启动完成后(仅广播一次)                           | Intent.ACTION_BOOT_COMPLETED         |
| 按下照相时的拍照按键(硬件按键)时                        | Intent.ACTION_CAMERA_BUTTON          |
| 屏幕锁屏                                     | Intent.ACTION_CLOSE_SYSTEM_DIALOGS   |
| 设备当前设置被改变时(界面语言、设备方向等)                   | Intent.ACTION_CONFIGURATION_CHANGED  |
| 插入耳机时                                    | Intent.ACTION_HEADSET_PLUG           |
| 未正确移除SD卡但已取出来时(正确移除方法:设置--SD卡和设备内存--卸载SD卡) | Intent.ACTION_MEDIA_BAD_REMOVAL      |
| 插入外部储存装置（如SD卡）                           | Intent.ACTION_MEDIA_CHECKING         |
| 成功安装APK                                  | Intent.ACTION_PACKAGE_ADDED          |
| 成功删除APK                                  | Intent.ACTION_PACKAGE_REMOVED        |
| 重启设备                                     | Intent.ACTION_REBOOT                 |
| 屏幕被关闭                                    | Intent.ACTION_SCREEN_OFF             |
| 屏幕被打开                                    | Intent.ACTION_SCREEN_ON              |
| 关闭系统时                                    | Intent.ACTION_SHUTDOWN               |
| 重启设备                                     | Intent.ACTION_REBOOT                 |

注：当使用系统广播时，只需要在注册广播接收者时定义相关的action即可，并不需要手动发送广播，当系统有相关操作时会自动进行系统广播
```xml
<receiver android:name=".BroadcastReceiver.MyBroadcastReceiver">
    <intent-filter>
        <!--重启设备-->
        <action android:name="android.intent.action.REBOOT" />
    </intent-filter>
</receiver>
```




### 3、本地广播（Local Broadcast）

- 由于Android中的广播可以跨App直接通信，所有应用程序都可以接收到（exported对于有intent-filter情况下默认值为true）
- 可能出现的问题：
  - 其他App针对性发出与当前App intent-filter相匹配的广播，由此导致当前App不断接收广播并处理；
  - 其他App注册与当前App一致的intent-filter用于接收广播，获取广播具体信息；即会出现安全性 & 效率性的问题。
- 解决方案：使用本地广播（Local Broadcast）
 1. 本地广播可理解为一种**局部广播**，广播的**发送者和接收者都同属于一个App**。
 2. 相比于全局广播（普通广播），App应用内广播优势体现在：**安全性高 & 效率高**

- 具体使用1——将全局广播**设置成局部广播**
  1. 注册广播时将**exported**属性设置为***false***，使得非本App内部发出的此广播不被接收；
  2. 在广播发送和接收时，增设相应权限permission，用于权限验证；
  3. 发送广播时指定该广播接收器所在的包名，此广播将只会发送到此包中的App内与之相匹配的有效广播接收器中。
通过**intent.setPackage(packageName)** **指定报名**

- 具体使用2 - 使用封装好的**LocalBroadcastManager**类使用方式上与全局广播几乎相同，只是注册/取消注册广播接收器和发送广播时将参数的context变成了LocalBroadcastManager的单一实例
注：对于LocalBroadcastManager方式发送的应用内广播，**只能**通过LocalBroadcastManager**动态注册**，不能静态注册
#### 注册Receiver
```java
 //创建广播
receiver = new MyBroadcastReceiver();
//注册本地广播
LocalBroadcastManager.getInstance(ReceiverActivity.this).registerReceiver(receiver,
        new IntentFilter("com.handsome.hensen"));
```
#### 注销Receiver
```java
LocalBroadcastManager.getInstance(ReceiverActivity.this).unregisterReceiver(receiver);
```

#### 发送异步广播

```java
final Intent intent = new Intent();
intent.setAction("com.handsome.hensen2");
LocalBroadcastManager.getInstance(ReceiverActivity.this).sendBroadcast(intent);
```

#### 发送同步广播
```java
LocalBroadcastManager.getInstance(ReceiverActivity.this).sendBroadcastSync(intent);
```






### 4、粘性广播（Sticky Broadcast）
由于在**Android5.0 & API 21中已经失效**，所以不建议使用。
sticky广播通过Context.sendStickyBroadcast()函数来发送，用此函数发送的广播会一直滞留，当有匹配此广播的广播接收器被注册后，该广播接收器就会收到此条信息。使用此函数需要发送广播时，需要获得BROADCAST_STICKY权限
```xml
<uses-permission android:name="android.permission.BROADCAST_STICKY"/>
```

sendStickyBroadcast只保留最后一条广播，并且一直保留下去，这样即使已经有广播接收器处理了该广播，当再有匹配的广播接收器被注册时，此广播仍会被接收。如果你只想处理一遍该广播，可以通过removeStickyBroadcast()函数来实现。

## 有序广播
### 5、有序广播（Ordered Broadcast）

- 定义发送出去的广播被广播接收者按照先后顺序接收

> 有序是**针对**广播**接收者**而言的

- 广播接受者接收广播的顺序规则（同时面向静态和动态注册的广播接受者）
  1. 按照Priority属性值从大-小排序；
  2. Priority属性相同者，动态注册的广播优先；
- 特点
  1. 接收广播**按顺序接收**
  2. 先接收的广播接收者可以对广播进行**截断**（**abortBroadcast**()），即后接收的广播接收者不再接收到此广播；
  3. 先接收的广播接收者可以对广播进行**修改**，再使用**setResult**()函数来结果**传给下一个广播接收器**接收，那么后接收的广播接收者将通过**getResult**()函数来取得上个广播接收器接收返回的结果
- 具体使用有序广播的使用过程与普通广播非常类似，差异仅在于广播的发送方式：通过**Context.sendOrderedBroadcast()**来发送
```java
public abstract void sendOrderedBroadcast(@RequiresPermission Intent intent,
        @Nullable String receiverPermission);
/* 第一个参数 Intent 类型：意图
 * 第二个参数 String 类型 receiverPermission，接收器需要的权限
 * 第三个参数 BroadcastReceiver 类型，自己定义的接收器作为最终接收器
 * 第四个参数 Handler 类型，用于执行接收器的回调，如果为 null 则在主线程中执行
 * 第五个参数 int 类型，结果代码的初始码
 * 第六个参数初始化参数
 * 第七个参数 Bundle 类型，额外的数据*/
public abstract void sendOrderedBroadcast(@RequiresPermission @NonNull Intent intent,
        @Nullable String receiverPermission, @Nullable BroadcastReceiver resultReceiver,
        @Nullable Handler scheduler, int initialCode, @Nullable String initialData,
        @Nullable Bundle initialExtras);
```

##### ① 自定义广播接收器
我们创建一个类，存放三个有优先级的广播接收者，并在最高级广播中传递结果到下一个广播

```java
public class PriorityBroadcastReceiver {
    public static class HighPriority extends BroadcastReceiver {
        //高级广播接收者
        @Override
        public void onReceive(Context context, Intent intent) {
            Log.e("receive", "High");
            //setResult传递结果到下一个广播接收器
            int code = 0;
            String data = "hello";
            Bundle bundle = null;
            setResult(code, data, bundle);
        }
    }

    public static class MidPriority extends BroadcastReceiver {
        //中级广播接收者
        @Override
        public void onReceive(Context context, Intent intent) {
            Log.e("receive", "Mid");
            //获取上一个广播接收器结果
            int code = getResultCode();
            String data = getResultData();
            Log.e("receive", "获取到上一个广播接收器结果：" + "code=" + code + "data=" + data);
        }
    }

    public static class LowPriority extends BroadcastReceiver {
        //低级广播接收者
        @Override
        public void onReceive(Context context, Intent intent) {
            Log.e("receive", "Low");
        }
    }
}
```

注意：**内部类**的BroadcastReceiver必须**由public static修饰**，否则会报错

##### ② 注册广播接收器
这里的注册方式和普通广播是一样的，这里的区别在于**priority属性**，确定了他们之间的优先级

```xml
<receiver android:name=".BroadcastReceiver.PriorityBroadcastReceiver$HighPriority">
    <intent-filter android:priority="3000">
        <action android:name="com.handsome.hensen2" />
    </intent-filter>
</receiver>
<receiver android:name=".BroadcastReceiver.PriorityBroadcastReceiver$MidPriority">
    <intent-filter android:priority="2000">
        <action android:name="com.handsome.hensen2" />
    </intent-filter>
</receiver>
<receiver android:name=".BroadcastReceiver.PriorityBroadcastReceiver$LowPriority">
    <intent-filter android:priority="1000">
        <action android:name="com.handsome.hensen2" />
    </intent-filter>
</receiver>
```

注意：BroadcastReceiver**类名与内部类的名字之间用$符号隔开**，否则会报错

##### ③ 发送广播
和之前的不一样的地方，这里是使用**sendOrderedBroadcast**()发送有序广播
```java
final Intent intent = new Intent();
intent.setAction("com.handsome.hensen2");
bt_send.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        sendOrderedBroadcast(intent,null);
    }
});
```

注意：这里需要发送的是有序广播，否则在接收者中通过setResult()和getResult()方法会报错，因为只有有序广播才能传递结果

**运行代码**
运行程序后，我们点击发送广播。我们以Log信息来验证发出的广播被我们准确的接收，数据被我们准确的传递
```
11-25 11:50:07.207 12777-12777/com.handsome.boke2 E/receive: High
11-25 11:50:07.217 12777-12777/com.handsome.boke2 E/receive: Mid
11-25 11:50:07.218 12777-12777/com.handsome.boke2 E/receive: 获取到上一个广播接收器结果：code=0data=hello
11-25 11:50:07.233 12777-12777/com.handsome.boke2 E/receive: Low
```
#### 拦截广播
上面我们提到过有序广播中可以拦截广播，那么我们在上面程序的基础上修改代码，在HighPriority接收器中加上拦截广播

**① 创建广播**
通过在BroadcastReceiver中，执行**abortBroadcast**()方法，广播就不会继续往下传递了
```java
public static class HighPriority extends BroadcastReceiver {
        //高级广播接收者
        @Override
        public void onReceive(Context context, Intent intent) {
            Log.e("receive", "High");
            //拦截广播
            abortBroadcast();
            //传递结果到下一个广播接收器
            int code = 0;
            String data = "hello";
            Bundle bundle = null;
            setResult(code, data, bundle);
        }
    }
```

**运行代码**
运行程序后，我们点击发送广播。我们以Log信息来验证我们拦截了广播
```
11-25 12:12:36.405 30867-30867/com.handsome.boke2 E/receive: High
```
可以看到，后面的Mid和Low广播都没有Log信息，说明我们拦截成功了

#### 最终广播接收者
现在有这样的一个应用场景，按照上面的程序走，只能在第一个广播中被拦截住了，后面的广播则不执行。如果这个时候我们需要一个**不管有没有被拦截都必须执行的广播**，我们称为最终广播接收者，那应该怎么办。同样的，发送有序广播也考虑到这一点，通过以下代码来发送广播，并指定我们不管有没有被拦截都必须执行的最终广播接收者
```java
bt_send.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        sendOrderedBroadcast(intent, null, new PriorityBroadcastReceiver.LowPriority(),
                new Handler(), 0, null, null);
    }
});
```

运行代码，我们查看Log信息
```
11-25 12:22:33.466 4764-4764/com.handsome.boke2 E/receive: High
11-25 12:22:33.468 4764-4764/com.handsome.boke2 E/receive: Low
```

可以发现，之前只是有High的Log信息，因为是被拦截了，而Log信息多了一条Low，说明我们拦截后，还要执行终结广播

## 特别注意

对于**不同注册方式**的广播接收器回调**OnReceive**（Context context，Intent intent）中的context**返回值**是**不一样**的：

- 对于**静态注册**（全局+本地广播），回调onReceive(context, intent)中的context返回值是：**ReceiverRestrictedContext**；
- 对于**全局**广播的**动态注册**，回调onReceive(context, intent)中的context返回值是：**Activity Context**；
- 对于**本地**广播的动态注册（**LocalBroadcastManager**方式），回调onReceive(context, intent)中的context返回值是：**Application Context**。
- 对于**本地**广播的动态注册（**非LocalBroadcastManager**方式），回调onReceive(context, intent)中的context返回值是：**Activity Context**；


## BroadCastReceiver生命周期

### BroadcastReceiver对象的生命周期
#### 静态注册的BroadcastReceiver
在AndroidManifest.xml中注册的BroadcastReceiver, 每次收到一个Intent, 也就是**onReceive被回调的时候**, 这个**BroadcastReceiver**都是**新创建**出来的, 官方文档中写:
> A BroadcastReceiver object is only valid for the duration of the call to onReceive(Context, Intent). Once your code returns from this function, the system considers the object to be finished and no longer active.

也就是说, **出了onReceive**, 这个BroadcastReceiver**对象的生命周期就已经到头了**, 这也是为什么我们**不能在onReceive中进行一些异步操作**的原因, 有可能异步操作还没完成, BroadcastReceiver所在的进程就被kill了。
表现出来的结果就是, **BroadcastReceiver中的成员变量无法保存它们的值**, 因为它们每次都是重新创建的, 之前的已经随着BroadcastReceiver对象被销毁了。

#### 动态注册的BroadcastReceiver
但是有一种情况, **BroadcastReceiver的成员变量是可用的**, 那就是动态注册的BroadcastReceiver. 动态注册的BroadcastReceiver对象的生命其实是受我们控制的. 
实际测试, 使用Context.registerReceiver和Context.unregisterReceiver注册的BroadcastReceiver每次收到广播都是使用我们注册时传入的对象处理的, 这也是符合我们代码上的逻辑的. 当然, 此时静态变量也是可用的.


### BroadcastReceiver所在进程的生命周期
对于那种在AndroidManifest.xml中静态注册的BroadcastReceiver, 成员变量是没法用了, 有人说, 是不是用static变量就可以了呢, 某些情况下是可以的, 什么情况呢, 就是进程不会被kill的情况. 
官方文档里面有一段
> Once you return from onReceive(), the BroadcastReceiver is no longer active, and its hosting process is only as important as any other application components that are running in it.

在AndroidManifest.xml中静态注册的BroadcastReceiver的onReceive被回调时, 有可能这个进程只承载了这个BroadcastReceiver, 比如我们的应用没有运行的情况, 等onReceive返回, 这个时候我们的进程的会被视为空进程(empty process), 此时Android有极大可能回收掉空进程, 这种情况下静态成员变量也无法保存值了.

如果我们的程序正在运行, 则Android不一定会回收掉我们的进程, 因为此时我们的进程级别会以进程中承载的级别最高的组件为准. 在我的实际项目中, 我是在播放歌曲的情况下监听线控耳机的按下Intent, 这个时候我的应用是有一个前台服务播放歌曲的, 此时我的进程至少是可见进程(visible process)级别, 几乎不会被Android kill掉, 所以我可以在BroadcastReceiver中使用一个静态成员变量记录上一次点击的时间.

<span id="BroadCastReceiver生命周期总结"/>
### BroadCastReceiver生命周期总结
1.  广播接收者的生命周期非常短暂的，在接收到广播的时候创建，**onReceive()方法结束之后销毁**；
2. 广播接收者中**不要做**一些**耗时**的工作，否则会弹出 **Application No Response** 错误对话框；
3. 最好也**不要**在广播接收者中**创建子线程**做耗时的工作，因为广播接收者被销毁后进程就成为了空进程，很容易被系统杀掉；
4. **耗时**的较长的工作可以通过Intent**启动Service**来完成，但不能绑定Service。



# Android 广播面试题
## 请描述一下 BroadcastReceiver
BroadCastReceiver 是 Android **四大组件之一**，主要用于**接收系统或者 app 发送的广播事件**。 
内部通信实现机制：通过 Android 系统的 Binder 机制实现通信。 
广播分两种：有序广播和无序广播。
- **无序广播**：完全异步，逻辑上可以被任何广播接收者接收到。优点是效率较高。缺点是一个接收者不能将处理结果传递给下一个接收者，并无法终止广播 intent 的传播。 
- **有序广播**：按照被接收者的优先级顺序，在被接收者中依次传播。比如有三个广播接收者 A，B，C，优先级是 A >B > C。那这个消息先传给 A，再传给 B，最后传给 C。每个接收者有权终止广播，比如 B 终止广播，C 就无法接收到。 此外 A 接收到广播后可以对结果对象进行操作，当广播传给 B 时，B 可以从结果对象中取得 A 存入的数据。
在 通 过 Context.**sendOrderedBroadcast**(intent, receiverPermission, resultReceiver, scheduler, initialCode, initialData, initialExtras)时我们可以指定 resultReceiver 广播接收者，这个接收者我们可以认为是最终接收者，通常情况下如果比他优先级更高的接收者如果没有终止广播，那么他的 onReceive 会被执行两次，第一次是正常的按照优先级顺序执行，第二次是作为最终接收者接收。如果比他优先级高的接收者终止了广播，那么他依然能接收到广播。 

在我们的项目中经常使用广播接收者接收系统通知，比如开机启动、sd 挂载、低电量、外播电话、锁屏等。

详细见上文#[BroadcastReceiver简介](#BroadcastReceiver简介)

## 如何注册和使用 BroadcastReceiver

在清单文件中注册广播接收者称为静态注册，在代码中注册称为动态注册。
静态注册的广播接收者只要 app 在系统中运行则一直可以接收到广播消息；
动态注册的广播接收者当注册的 Activity 或者 Service 销毁了那么就接收不到 广播了。
静态注册：在清单文件中进行如下配置

```xml
<receiver android:name=".BroadcastReceiver1" >
	<intent-filter>
		<action android:name="android.intent.action.CALL" >
		</action>
	</intent-filter>
</receiver>
```

动态注册：在代码中进行如下注册
```java
receiver = new BroadcastReceiver();
IntentFilter intentFilter = new IntentFilter(); intentFilter.addAction(CALL_ACTION); context.registerReceiver(receiver, intentFilter);
```
详细见上文#[2、广播接收器注册](#2、广播接收器注册)


## BroadCastReceiver生命周期
见上文#[BroadCastReceiver生命周期总结](#BroadCastReceiver生命周期总结)


## 如何让自己的广播只让指定的app接收
（2015.09.02）


1、自己的应用（假设名称为应用 A）在发送广播的时候给自己发送的广播添加自定义权限，假设权限名为：

com.itheima.android.permission，然后需要在应用 A 的 AndroidManifest.xml 中声明如下权限：

```xml
<permission android:name="com.itheima.android.permission"
	android:protectionLevel="normal">
</permission>
<uses-permission android:name="com.itheima.android.permission"/>
```


2、其他应用（假设名称诶应用 B）如果想接收该广播，那么就必须知道应用 A 广播使用的权限。然后在应用 B 的清单文件中如下配置：

```xml
<uses-permission android:name="com.itheima.android.permission"/>
```

或者在应用 AndroidManifest.xml 中的`<receiver>`标签中进行如下配置：

```xml
<receiver android:name="com.itheima.android.broadcastReceiver.MyReceiver"
android:permission="com.itheima.android.permission">
	<intent-filter >
		<action android:name="com.itheima.mybroadcast"></action>
	</intent-filter>
</receiver>
```

>每个权限通过 **protectionLevel** 来标识**保护级别**:
**normal** ： 低 风 险 权 限 ， 只 要 申 请 了 就 可 以 使 用 （ 在 AndroidManifest.xml 中 添 加`<uses-permission>`标签），安装时不需要用户确认；
**dangerous**：高风险权限，安装时需要用户的确认才可使用；
**signature**：只有当申请权限的应用程序的数字签名与声明此权限的应用程序的数字签名相同时（如果是申请系统权限，则需要与系统签名相同），才能将权限授给它；
**signatureOrSystem**：签名相同，或者申请权限的应用为系统应用（在 system image 中）。
上述四类权限级别同样可用于自定义权限中。如果开发者需要对自己的应用程序（或部分应用）进行 访 问 控 制 ， 则 可 以 通 过 在 AndroidManifest.xml 中 添 加 `<permission>` 标 签 ， 将 其 属 性 中 的protectionLevel 设置为上述四类级别中的某一种来实现。
 



## 什么是最终广播接收者？
最终广播是我们自己应用发送有序广播时通过 ContextWrapper.sendOrderedBroadcast()方法指定的当前应用 下的广播，该广播可能会被执行两次，第一次是作为普通广播按照优先级接收广播，第二次是作为 final receiver 必须 接收一次。

## 广播的优先级对无序广播生效吗？
生效的。广播的优先级推荐的范围是：[-1000,+1000]，但是如果设置的优先级值超过这个范围也是可以的。


## 动态注册的广播优先级谁高？
谁先注册谁优先级高。


## 如何判断当前BroadcastReceiver接收到的是有序广播还是无序广播 ？
（2015-10-16）
在 BroadcastReceiver 类中 onReceive（）方法中，可以调用 boolean b = **isOrderedBroadcast**（）;该方法是BroadcastReceiver 类中提供的方法，用于告诉我们当前的接收到的广播是否为有序广播。

## Android 引入广播机制的用意
（2017-2-23）
1. 从 MVC 的角度考虑(应用程序内) 其实回答这个问题的时候还可以这样问，android 为什么要有那 4 大组件， 现在的移动开发模型基本上也是照搬的 web 那一套 MVC 架构，只不过是改了点嫁妆而已。android 的四大组件本质 上就是为了实现移动或者说嵌入式设备上的 MVC 架构，它们之间有时候是一种相互依存的关系，有时候又是一种补 充关系，引入广播机制可以方便几大组件的信息和数据交互。
2. 程序间互通消息(例如在自己的应用程序内监听系统来电)
3. 效率上(参考 UDP 的广播协议在局域网的方便性)
4. 设计模式上(反转控制的一种应用，类似监听者模式)


引用：
[Android四大组件：BroadcastReceiver史上最全面解析](https://www.jianshu.com/p/ca3d87a4cdf3)
[Android四大组件——BroadcastReceiver普通广播、有序广播、拦截广播、本地广播、Sticky广播、系统广播](http://blog.csdn.net/qq_30379689/article/details/53341313)
[BroadcastReceiver生命周期探讨](http://blog.csdn.net/shaw1994/article/details/48342455)
[Android开发之BroadcastReceiver详解](https://www.2cto.com/kf/201408/324155.html)
[BroadcastReceiver生命周期探讨](http://blog.csdn.net/shaw1994/article/details/48342455)

】


【Android Service】
【
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
|         | **在onBind方法返回**            | **重写ServiceConnection类的<br />onServiceConnected方法中获取** |                                          |
| Binder  | binder                 | LocalService.LocalBinder binder = (LocalService.LocalBinder) service;<br />mService = binder.getService();<br />即可调用Service中的方法 | Service中自定义的Binder中自定义了getService返回当前Service |
| Message | mMessenger.getBinder() | new Messenger(service);<br />即可通过这个Message向Service发消息 | Service中自定义了 Handler处理消息                 |
| AIDL    | Stub                   | AlipayRemoteService  alipayRemoteService = Stub.asInterface(service); |                                          |




引用：
[Android四大组件——Service后台服务、前台服务、IntentService、跨进程服务、无障碍服务、系统服务](http://blog.csdn.net/qq_30379689/article/details/53318861)
[关于Android Service真正的完全详解，你需要知道的一切](http://blog.csdn.net/javazejian/article/details/52709857)
[Android开发之如何保证Service不被杀掉（broadcast+system/app）](http://blog.csdn.net/mad1989/article/details/22492519)
















】


【Android ContentProvider】
【
# 【Android ContentProvider】

## 概述
**ContentProvider内容提供者**是Android系统**四大组件之一**，用于**保存和检索数据**，是Android系统中**不同应用程序之间共享数据的接口**。

在Android系统中，**应用程序之间**是**相互独立**的，分别运行在自己的进程中，相互之间没有数据交换，如果应用程序之间需要**共享或交换数据**，就需要**用内容提供者（ContentProvider）**。

ContentProvider是不同应用程序之间进行数据交换的标准API，它**以Uri的形式对外提供数据**，允许其它应用程序操作本应用的数据，**其它应用**程序能**通过ContentResolver**，并根据ContentProvider提供的**Uri** **操作**指定的**数据**。


### 为何使用ContentProvider

ContentProvider一般为存储和获取数据提供统一的接口，可以在不同的应用程序之间共享数据。

之所以使用ContentProvider，主要有以下几个理由：

1. ContentProvider**提供了对底层数据存储方式的抽象**。
比如下图中，底层使用了SQLite数据库，在用了ContentProvider封装后，即使你把数据库换成MongoDB，也不会对上层数据使用层代码产生影响。

- ![](http://upload-images.jianshu.io/upload_images/9028834-e28fd0ee96960332.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2. Android框架中的一些类**需要ContentProvider类型数据**。如果你想让你的数据可以使用在如SyncAdapter, Loader, CursorAdapter等类上，那么你就需要为你的数据做一层ContentProvider封装。

3. 第三个原因也是最主要的原因，是ContentProvider**为应用间的数据交互提供了一个安全的环境**。它准许你把自己的应用数据根据需求开放给其他应用进行增、删、改、查，而不用担心直接开放数据库权限而带来的安全问题。

我们知道了ContentProvider是对数据层的封装后，那么大家可能会问我们要如何对ContentProvider进行增，删，改，查的操作呢？下面我们来介绍一个新的类ContentResolver，我们可以通过它，来对不同的ContentProvider进行操作。

### 为何使用ContentResolver
有些人可能会疑惑，为什么我们不直接访问Provider，而是又**在上面加了一层ContentResolver来进行对其的操作**，这样岂不是更复杂了吗？其实不然，大家要知道一台手机中可不是只有一个Provider内容，它可能安装了很多含有Provider的应用，比如联系人应用，日历应用，字典应用等等。
有如此多的Provider，如果你开发一款应用要使用其中多个，如果让你去了解每个ContentProvider的不同实现，岂不是要头都大了。
所以Android为我们提供了**ContentResolver**来**统一管理与不同ContentProvider间的操作**。
[![](http://upload-images.jianshu.io/upload_images/9028834-5585d3e9a4406738.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://upload-images.jianshu.io/upload_images/9028834-5585d3e9a4406738.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在Context.java的源码中有一段
```java
/** Return a ContentResolver instance for your application's package. */
 public abstract ContentResolver getContentResolver();
```

所以我们可以通过在**所有继承Context的类**中通过调用**getContentResolver**()来获得ContentResolver。

那ContentResolver是如何来区别不同的ContentProvider的呢？这就涉及到URI（Uniform Resource Identifier）。

## ContentProvider及其URI

### 引入ContentProvider的URI

思考两应用间如何通信，首先，我们最先想到的方法应该是如下面这张图这样的，自身应用写好各个数据库接口，供其它应用调用

![](http://upload-images.jianshu.io/upload_images/9028834-1c476e9d28a1873d?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

那么，问题来了，首先，其它应用如何调起这个应用的数据库接口；要知道你要调这个接口的应用很可能根本就没有运行。

咦，好像有一种方法就能**调起没有运行的应用**——**隐式Intent**！！！

隐式Intent的调用方法如下，如果这个Activity要供其它应用调用，那么这个Activity在这个应用AndroidManifest.xml中声明方式为：

```xml
<activity  
    android:name=".SecondActivity"  
    android:label="@string/title_activity_second" >  
    <intent-filter>  
        <action android:name="harvic.test.qijian"/>  
        <category android:name="android.intent.category.DEFAULT"/>  
    </intent-filter>  
</activity>  
```

在其它APP中，要调这个Activity时，就使用：

```java
Intent intent = new Intent("harvic.test.qijian");  
startActivity(intent);  
```

我们先分析一下**隐式Intent是如何做到**的：
1. 首先，我们将filter信息写在了AndroidManifest.xml中；

2. 当有一个隐式Intent要匹配时，系统（注意是系统！）会搜索整个手机上所有APP的Acitivity进行匹配，如果有匹配的并且有使用权限的，就起起来；大家想想当你在应用中点击网址链接的时候，是不是会转到浏览器中，这就是用的隐式Intent匹配。如果你手机上有多个应用可以匹配，那么就会以列表的形式列出来供你选择开哪一个。具体有关隐式Intent的内容，可以参看[《Intent详解一》](http://blog.csdn.net/harvic880925/article/details/38399723)和[《Intent详解二》](http://blog.csdn.net/harvic880925/article/details/38406421)

根据隐式Intent，我们是不是有受到启发，那我们也可额外再开出来一个东东，**仿照隐式Intent**的方式来进行**全局匹配**，如果匹配成功就执行操作。对，这就是那帮谷歌老头的设计方法，他们设计的东东就是**ContentProvider的URI**。

### ContentProvider与对应的URI
ContentProvider中的URI有固定格式，如下图：
![](http://upload-images.jianshu.io/upload_images/9028834-c913fb02ee92bafd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
主要分三个部分：scheme, authority and path。
Scheme表示上图中的content://，Authority表示B部分，Path表示C和D部分。 
- **Scheme：**表示是一个Android内容URI，说明由ContentProvider控制数据，该部分是**固定形式**携程**`content://`**，不可更改的。 
- **Authority：**是URI的授权部分，是**唯一标识符**，用来定位区别不同ContentProvider。
格式一般是自定义**ContentProvider类**的**完全限定名**称，注册时需要用到。
如：com.example.transportationprovider。
- **Path：**是每个**ContentProvider内部的路径**部分，C和D部分称为路径片段。
**C部分**指向一个对象集合，一般用**表**的**名**字，如：/trains表示一个笔记集合；
**D部分**指向特定的记录，如：/trains/122表示**id**为122的单条记录，如果没有指定D部分，则返回全部记录。
表名，用以区分ContentProvider中不同的数据表；



### ContentProvider声明方式
上面这个URI就是用来进行全局匹配的，那AndroidManifest.xml里又要怎么写呢？
再回想下隐式Intent，在隐式Intent中，Intent-fliter是Activity的一部分，专门过滤隐式Intent的匹配请求的，如果Intent-fliter匹配后，就启动对应的Activity；
那我们也可以设计一个东东，把这个URI作为它的一部分，当匹配成功以后，就进这个东东里操作数据库。这个东东就是ContentProvider。

**ContentProvider在AndroidManifest.xml中的声明方式**为：（与上面的URI对应）

```xml
<provider  
    android:name=".NoteContentProvider"  
    android:authorities="com.example.transportationprovider"  
    android:exported="true"/>  
```

这里的**android:authorities**必须**与上面URI中的B部分一样**，因为这个就是用来全局匹配的authority。只有URI的authority与provider的android:authorities匹配上了，才会进入后面的操作。

### **ContentURI的全局流程图**

![](http://upload-images.jianshu.io/upload_images/9028834-ca57f7fb226300f7?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上面，我们说到了第一步和第二步，当匹配成功ContentProvider以后，就开始进入我们指定的ContentProvider进行处理；大家估计也注意到了，我们的ContentURI的完整部分为：content://com.example.transportionprovider/trains/122

到第二步，我们已经匹配到了content://com.example.transportionprovider，那后面的/trains/122的匹配工作就只有交由ContentProvider来处理了。

**在ContentProvder中的匹配是利用UriMatcher来完成的**。

### UriMatcher

UriMatcher的匹配工作的第一步就是先将所需要的匹配的URI使用addURI()添加到UriMatcher中，

```java
/*authority:就是URI对应的authority
path:就是我们在URI中 authority后的那一串
code:表示匹配成功以后的返回值；*/
public void addURI(String authority, String path, int code)  
```



下面以我们的URI=content://com.example.transportionprovider/trains/122来演示一下

```java
public static final String AUTHORITY = "com.alexzhou.provider.NoteProvider";  
private static final UriMatcher sUriMatcher;  
static {  
    sUriMatcher = new UriMatcher(UriMatcher.NO_MATCH);  
    sUriMatcher.addURI(AUTHORITY, "trains", 1);  //表示匹配content://com.example.transportionprovider/trains，如果匹配成功返回1
    sUriMatcher.addURI(AUTHORITY, "trains/#", 2);  //#表示匹配任意数据ID *表示匹配任意文本
}  
```
所以这句的意思就是匹配content://com.example.transportionprovider/trains/任意数字ID  比如我们的content://com.example.transportionprovider/trains/122

## ContentResolver
第三方应用如何根据URI来指定操作的，是哪个函数来操作URI的呢？

它就是ContentResolver；

下面简单先说一下ContentResolver的函数：

插入（insert）：

```java
//在第三方中，我们要向指定应用的数据库中插入一条记录，其中title字段的值为hello,content字段的值为my name is harvic。代码如下：
String CONTENT_URI = "content://com.example.transportionprovider/trains/122;"  
ContentResolver cr =getContentResolver();  
ContentValues values = new ContentValues();  
values.put("title", "hello");//数据库的键值对  
values.put("content", "my name is harvic");  
  
Uri uri = cr.insert(CONTENT_URI, values);  
```



这段代码一调用，那可就吊了，系统会搜索手机上所有APP的AndroidManifest.xml，看哪个provider的authority匹配，在匹配之后，转到对应的类中，再让UriMatcher匹配后面的PATH字段，都完全匹配之后，就执行ContentProvider中的insert方法。

当然ContentResolver除了insert方法还有query()、update()、delete()方法，可以执行第三方数据库的任何操作。

## 使用步骤
### ContentProvider提供数据库查询接口

#### 1、利用SQLiteOpenHelper创建数据库、数据表

在这里，我们在一个数据库(“harvic.db”)中创建两个数据表”first”与”second”;每个表都有多出一个字段“table_name”，来保存当前数据表的名称 
代码如下：

```java
public class DatabaseHelper extends SQLiteOpenHelper {  
    public static final String DATABASE_NAME = "harvic.db";  
    public static final int DATABASE_VERSION = 1;  
    public static final String TABLE_FIRST_NAME = "first";  
    public static final String TABLE_SECOND_NAME = "second";  
    public static final String SQL_CREATE_TABLE_FIRST = "CREATE TABLE " +TABLE_FIRST_NAME +"("  
            + BaseColumns._ID + " INTEGER PRIMARY KEY AUTOINCREMENT,"  
            + "table_name" +" VARCHAR(50) default 'first',"  
            + "name" + " VARCHAR(50),"  
            + "detail" + " TEXT"  
            + ");" ;  
    public static final String SQL_CREATE_TABLE_SECOND = "CREATE TABLE "+TABLE_SECOND_NAME+" ("  
            + BaseColumns._ID + " INTEGER PRIMARY KEY AUTOINCREMENT,"  
            + "table_name" +" VARCHAR(50) default 'second',"  
            + "name" + " VARCHAR(50),"  
            + "detail" + " TEXT"  
            + ");" ;  
  
    public DatabaseHelper(Context context) {  
        super(context, DATABASE_NAME, null, DATABASE_VERSION);  
    }  
  
    @Override  
    public void onCreate(SQLiteDatabase db) {  
        Log.e("harvic", "create table: " + SQL_CREATE_TABLE_FIRST);  
        db.execSQL(SQL_CREATE_TABLE_FIRST);  
        db.execSQL(SQL_CREATE_TABLE_SECOND);  
    }  
  
    @Override  
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {  
        db.execSQL("DROP TABLE IF EXISTS first");  
        db.execSQL("DROP TABLE IF EXISTS second");  
        onCreate(db);  
    }  
}  
```

#### 2、利用ContentProvider提供数据库操作接口
新建一个类PeopleContentProvider，派生自ContentProvider基类，写好之后，就会自动生成query(),insert(),update(),delete()和getType()方法；这些方法就是根据传过来的URI来操作数据库的接口。此时的PeopleContentProvider是这样的：

```java
public class PeopleContentProvider extends ContentProvider {  
    @Override  
    public boolean onCreate() {  
        return false;  
    }  
  
    @Override  
    public Cursor query(Uri uri, String[] projection, String selection, String[] selectionArgs, String sortOrder) {  
        return null;  
    }  
  
    @Override  
    public String getType(Uri uri) {  
        return null;  
    }  
  
    @Override  
    public Uri insert(Uri uri, ContentValues values) {  
        return null;  
    }  
  
    @Override  
    public int delete(Uri uri, String selection, String[] selectionArgs) {  
        return 0;  
    }  
  
    @Override  
    public int update(Uri uri, ContentValues values, String selection, String[] selectionArgs) {  
        return 0;  
    }  
```

#### 3、使用UriMatcher匹配URI

我们先不管这几个函数的具体操作，先想想在上节中我提到的当一个URI逐级匹配到了ContentProvider类里以后，会怎么做——利用UriMatcher进行再次匹配。
**UriMatcher匹配成功以后，才会执行对应的操作**。所以上面的那些操作是在UriMatcher匹配之后。 
好，我们就先看看UriMatcher是怎么匹配的。

```java
public class PeopleContentProvider extends ContentProvider {  
    private static final UriMatcher sUriMatcher;  
    private static final int MATCH_FIRST = 1;  
    private static final int MATCH_SECOND = 2;  
    public static final String AUTHORITY = "com.harvic.provider.PeopleContentProvider";  
    public static final Uri CONTENT_URI_FIRST = Uri.parse("content://" + AUTHORITY + "/frist");  
    public static final Uri CONTENT_URI_SECOND = Uri.parse("content://" + AUTHORITY + "/second");  
  
    //UriMatcher
    static {  
        //如果匹配不成功，返回UriMatcher.NO_MATCH
        sUriMatcher = new UriMatcher(UriMatcher.NO_MATCH);  
        sUriMatcher.addURI(AUTHORITY, "first", MATCH_FIRST);//传来content://com.harvic.provider.PeopleContentProvider/frist时，就返回code：MATCH_FIRST  
        sUriMatcher.addURI(AUTHORITY, "second", MATCH_SECOND);
    }  
  
    private DatabaseHelper mDbHelper;  
  
    @Override  
    public boolean onCreate() {  
        mDbHelper = new DatabaseHelper(getContext());  
        return false;  
    }  
    …………  
}  
```

上面的代码是最重要的一句话：

```java
sUriMatcher.addURI(AUTHORITY, "first", MATCH_FIRST);  
```
##### **addUri**
addUri的官方声明为：
```java
public void addURI (String authority, String path, int code)  
```

*   **authority：**这个参数就是ContentProvider的authority参数，这个参数必须与AndroidManifest.xml中的对应provider的authorities值一样；
*   **path:**就匹配在URI中authority后的那一坨，在这个例子中，我们构造了两个URI 
    (1)、content://com.harvic.provider.PeopleContentProvider/frist 
    (2)、content://com.harvic.provider.PeopleContentProvider/second 
    而path匹配的就是authority后面的/first或者/second
*   **code：**这个值就是在匹配path后，返回的对应的数字匹配值;

#### 4、insert方法
下面先看看insert方法,主要功能为：
当URI匹配content://com.harvic.provider.PeopleContentProvider/frist时，将数据插入first数据库；
当URI匹配content://com.harvic.provider.PeopleContentProvider/second时，将数据插入second数据库。

```java
@Override  
public Uri insert(Uri uri, ContentValues values) {  
    SQLiteDatabase db = mDbHelper.getWritableDatabase();  
    switch (sUriMatcher.match(uri)){  //获取上面设定sUriMatcher时，sUriMatcher.addURI (String authority, String path, int code)中对应返回的code
        case MATCH_FIRST:{  
            long rowID = db.insert(DatabaseHelper.TABLE_FIRST_NAME, null, values);//将数据键值对（values）插入到first表中。插入之后，会返回新插入记录的当前所在行号
            if(rowID > 0) {  
                Uri retUri = ContentUris.withAppendedId(CONTENT_URI_FIRST, rowID);
                return retUri;  
            }  
        }  
        break;  
        case MATCH_SECOND:{  
            long rowID = db.insert(DatabaseHelper.TABLE_SECOND_NAME, null, values);  
            if(rowID > 0) {  
                Uri retUri = ContentUris.withAppendedId(CONTENT_URI_SECOND, rowID);  
                return retUri;  
            }  
        }  
        break;  
        default:  
            throw new IllegalArgumentException("Unknown URI " + uri);  
    }  
    return null;  
}  
```

#### 5、update方法

在看了insert方法之后，update方法难度也不大，也是根据UriMatcher.match(uri)的返回值来判断当前与哪个URI匹配，根据匹配的URI来操作对应的数据库，代码如下：

```java
public int update(Uri uri, ContentValues values, String selection, String[] selectionArgs) {  
    SQLiteDatabase db = mDbHelper.getWritableDatabase();  
    int count = 0;  
    switch(sUriMatcher.match(uri)) {  
        case MATCH_FIRST:  
            count = db.update(DatabaseHelper.TABLE_FIRST_NAME, values, selection, selectionArgs);  
            break;  
        case MATCH_SECOND:  
            count = db.update(DatabaseHelper.TABLE_SECOND_NAME, values, selection, selectionArgs);  
            break;  
  
        default:  
            throw new IllegalArgumentException("Unknow URI : " + uri);  
    }  
    this.getContext().getContentResolver().notifyChange(uri, null);//通知当前的数据库有改变，让监听这个数据库的所有应用执行对应的操作
    return count;  
}  
```
在最后调用**getContentResolver().notifyChange**(uri, null);来**通知当前的数据库有改变**，让监听这个数据库的所有应用执行对应的操作。

#### 6、query方法

至于query()方法就不再细讲了，跟上面的一样，根据不同的URI来操作不同的查询操作而已，代码如下：

```java
public Cursor query(Uri uri, String[] projection, String selection, String[] selectionArgs, String sortOrder) {  
    SQLiteQueryBuilder queryBuilder = new SQLiteQueryBuilder();  
    switch (sUriMatcher.match(uri)) {  
        case MATCH_FIRST:  
            // 设置查询的表  
            queryBuilder.setTables(DatabaseHelper.TABLE_FIRST_NAME);  
            break;  
  
        case MATCH_SECOND:  
            queryBuilder.setTables(DatabaseHelper.TABLE_SECOND_NAME);  
            break;  
  
        default:  
            throw new IllegalArgumentException("Unknow URI: " + uri);  
    }  
  
    SQLiteDatabase db = mDbHelper.getReadableDatabase();  
    Cursor cursor = queryBuilder.query(db, projection, selection, selectionArgs, null, null, null);  
    return cursor;  
}  
```

#### 7、delete方法

delete()方法如下：

```java
public int delete(Uri uri, String selection, String[] selectionArgs) {  
    SQLiteDatabase db = mDbHelper.getWritableDatabase();  
    int count = 0;  
    switch(sUriMatcher.match(uri)) {  
        case MATCH_FIRST:  
            count = db.delete(DatabaseHelper.TABLE_FIRST_NAME, selection, selectionArgs);  
            break;  
  
        case MATCH_SECOND:  
            count = db.delete(DatabaseHelper.TABLE_SECOND_NAME, selection, selectionArgs);  
            break;  
        default:  
            throw new IllegalArgumentException("Unknow URI :" + uri);  
  
    }  
    return count;  
}  
```

#### 8、getType()

这个函数，我们这里暂时用不到，直接返回NULL，下篇我们会专门来讲这个函数的作用与意义。（[ContentProvider中的getType()](#ContentProvider中的getType)）

```java
public String getType(Uri uri) {  
    return null;  
}  
```

#### 9、AndroidManifest.xml中声明provider

在AndroidManifest.xml中要声明我们创建的Provider:

```xml
<provider  
    android:authorities="com.harvic.provider.PeopleContentProvider"  
    android:name=".PeopleContentProvider"  
    android:exported="true"/>  
```

### 第三方应用通过URI操作共享数据库

#### 1、ContentResolver操作URI

在第三方应用中，我们要如何利用URI来执行共享数据数的操作呢，这是利用ContentResolver这个类来完成的。
获取ContentResolver实例的方法为：
```java
ContentResolver cr = this.getContentResolver();  
```

ContentResolver有下面几个数据库操作：查询、插入、更新、删除
```java
public final Cursor query (Uri uri, String[] projection, String selection, String[] selectionArgs, String sortOrder)  
public final Uri insert (Uri url, ContentValues values)  
public final int update (Uri uri, ContentValues values, String where, String[] selectionArgs)  
public final int delete (Uri url, String where, String[] selectionArgs)  
```

第一个参数都是传入要指定的URI，然后再后面的几个参数指定数据库条件——where语句和对应的参数；下面我们就用户实例来看看这几个函数到底是怎么用的。

#### 2、全局操作

新建一个工程，命名为“UseProvider”，界面长这个样子：

![image](http://upload-images.jianshu.io/upload_images/9028834-c04631d0858b971f?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

最上头有两个按钮，用来切换当前使用哪个URI来增、删、改、查操作；由于不同的URI会操作不同的数据表，所以我们使用不同的URI，会在不同的数据表中操作；
先列出来那两个要匹配的URI，以及全局当前要使用的URI(mCurrentURI ),mCurrentURI 默认是/first对应的URI，如果要切换，使用界面上最上头的那两个按钮来切换当前所使用的URI。

```java
public static final String AUTHORITY = "com.harvic.provider.PeopleContentProvider";  
public static final Uri CONTENT_URI_FIRST = Uri.parse("content://" + AUTHORITY + "/first");  
public static final Uri CONTENT_URI_SECOND = Uri.parse("content://" + AUTHORITY + "/second");  
public static Uri mCurrentURI = CONTENT_URI_FIRST;  
```

#### 3、query()查询操作

下面先来看看查询操作的过程：

```java
private void query() {  
    Cursor cursor = this.getContentResolver().query(mCurrentURI, null, null, null, null);//由于没有加任何的限定语句，所以是查询出此数据表中的所有记录  
    Log.e("test ", "count=" + cursor.getCount());  
    //使用返回的数据库记录指针Cursor，逐个读出所有的记录
    cursor.moveToFirst();  
    while (!cursor.isAfterLast()) {  
        String table = cursor.getString(cursor.getColumnIndex("table_name"));  
        String name = cursor.getString(cursor.getColumnIndex("name"));  
        String detail = cursor.getString(cursor.getColumnIndex("detail"));  
        Log.e("test", "table_name:" + table);  
        Log.e("test ", "name: " + name);  
        Log.e("test ", "detail: " + detail);  
        cursor.moveToNext();  
    }  
    cursor.close();  
}  
```

#### 4、insert()插入操作

```java
public final Uri insert (Uri url, ContentValues values)//第二个函数要求传入要插入的ContentValues的键值对
```

```java
private void insert() {  
    ContentValues values = new ContentValues();  
    values.put("name", "hello");  
    values.put("detail", "my name is harvic");  
    Uri uri = this.getContentResolver().insert(mCurrentURI, values);  
    Log.e("test ", uri.toString());  
}  
```

#### 5、update()更新操作

```java
public final int update (Uri uri, ContentValues values, String where, String[] selectionArgs)  
```

这个函数的意思就是，先用where语句找出要更新的记录，然后将values键值对更新到对应的记录中；

*   **uri**:即要匹配的URI
*   **values**:这是要更新的键值对
*   **where**:SQL中对应的where语句
*   **selectionArgs**:where语句中如果有可变参数，可以放在selectionArgs这个字符串数组中；这些与数据库中的用法一样。

```java
//将_id = 1的记录的detail字符更新
private void update() {  
    ContentValues values = new ContentValues();  
    values.put("detail", "my name is harvic !!!!!!!!!!!!!!!!!!!!!!!!!!");  
    int count = this.getContentResolver().update(mCurrentURI, values, "_id = 1", null);  
    Log.e("test ", "count=" + count);  
    query();  
}  
```
#### 6、delete()删除操作

```java
public final int delete (Uri url, String where, String[] selectionArgs)  
```
第二个参数是SQL中的WHERE语句的过滤条件，selectionArgs同样是Where语句中的可变参数；

```java
//删除_id = 1的记录
private void delete() {  
    int count = this.getContentResolver().delete(mCurrentURI, "_id = 1", null);  
    Log.e("test ", "count=" + count);  
    query();  
}  
```

### 运行结果

1、我们先运行ContentProvider对应的APP：ContentProviderBlog，然后再运行UseProvider；
2、然后先用content://com.harvic.provider.PeopleContentProvider/frist 来操作ContentProviderBlog的数据库：
3、点两个insert操作，看返回的URI，在每个URI后都添加上了当前新插入记录的行号 

![image](http://upload-images.jianshu.io/upload_images/9028834-6e418674ec5e89d0?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

4、然后做下查询操作——query() 
由于我们的URI是针对first记录的，所以在这里的table_name，可以看到是“first”，即我们操作的是first表，如果我们把URI改成second对应的URI，那操作的就会变成second表 

![image](http://upload-images.jianshu.io/upload_images/9028834-83909f2dfe1154fa?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

5、更新操作——update()

执行Update()操作，会将_id = 1的记录的detail字段更新为“my name is harvic !!!!!!!!!!!!!!!!!!!!!!!!!!”；其它记录的值不变，结果如下：

![image](http://upload-images.jianshu.io/upload_images/9028834-10331dee32f0f3d9?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

6、删除操作——delete()

同样，删除操作也会只删除_id = 1的记录，所以操作之后的query()结果如下：

![image](http://upload-images.jianshu.io/upload_images/9028834-001ee9b3cd2f53cf?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**总结：**在这篇文章中，我们写了两个应用ContentProviderBlog和UseProvider,其中ContentProviderBlog派生自ContentProvider，提供第三方操作它数据库的接口；而UseProvider就是所谓第三方应用，在UseProvider中通过URI来操作ContentProviderBlog的数据库；

**源码下载地址：[http://download.csdn.net/detail/harvic880925/8528507](http://download.csdn.net/detail/harvic880925/8528507)**




<span id="ContentProvider中的getType"/>
## ContentProvider中的getType 

### MIME类型
#### 什么是MIME类型
根据百度百科的解释：MIME：全称Multipurpose Internet Mail Extensions，多功能Internet邮件扩充服务。它是一种多用途网际邮件扩充协议，在1992年最早应用于电子邮件系统，但后来也应用到浏览器。
MIME类型就是**设定某种扩展名的文件用一种应用程序来打开的方式类型**，当该扩展名文件被访问的时候，浏览器会自动使用指定应用程序来打开。多用于指定一些客户端自定义的文件名，以及一些媒体文件打开方式。
简单来讲，MIME类型就是用来**标识当前的Activity所能打开的文件类型**！

**下面简单列出来系统中自带的几种文件类型和对应的MIME类型：**

（前面是文件名，后面是对应的MIME类型字符串）

{".bmp", "image/bmp"}
{".c", "text/plain"}
{".class", "application/octet-stream"}
{".conf", "text/plain"}
{".cpp", "text/plain"}
{".doc", "application/msword"}

#### MIME类型有什么用
那现在看看在android中，MIME类型是用来干什么的呢？
首先，MIME类型主要是**Activity的Intent-filter的data域**;
比如下面这个Activity:
```xml
<activity  
    android:name=".SecondActivity"  
    android:label="@string/title_activity_second" >  
    <intent-filter>  
        <action android:name="harvic.test.qijian"/>  
        <category android:name="android.intent.category.DEFAULT"/>  
        <data android:mimeType="image/bmp"/>  
    </intent-filter>  
</activity>  
```
这里指定了data域的MimeType值是"image/bmp"，即在利用隐式Intent匹配时，只有指定MimeType是"image/bmp"时，才能启用这个Activity，也就是说，这个Activity只能打开image/bmp类型的文件！才是MIME类型匹配的重点；
所以**MIME类型在Activity中是用来指定，当前的Activity所支持打开的文件类型**！

### getType()概述
getType()的官方说明：
>public abstract **String getType (Uri uri)**    
Implement this to handle requests for the MIME type of the data at the given URI. The returned MIME type should start with vnd.android.cursor.item for a single record, or vnd.android.cursor.dir/ for multiple items. This method can be called from multiple threads, as described in Processes and Threads.    
Parameters  
uri the URI to query.  
Returns  
a MIME type string, or null if there is no type.  

现在再回过来看看ContentProvider中的getType()函数，这个函数会根据传进来的URI，生成一个代表MimeType的字符串；而此字符串的生成也有规则：
*   如果是单条记录应该返回以vnd.android.cursor.item/ 为首的字符串
*   如果是多条记录，应该返回vnd.android.cursor.dir/ 为首的字符串

至于自符串/后的字符就随便定义了。

这里考虑一个问题，为什么我们返回的MimeType，要以vnd.android.cursor.item/ 或vnd.android.cursor.dir/ 开头？
我们知道，MIME类型其实就是一个字符串，中间有一个 “/” 来隔开，“/”前面的部分是系统识别的部分，就相当于我们定义一个变量时的变量数据类型，通过这个“数据类型”，系统能够知道我们所要表示的是个什么东西。至于 “/” 后面的部分就是我们自已来随便定义的“变量名”了。

### getType()与Activity的关系

上面我们讲了MIME存在于Activity的intent-filter中，那我们的getType() 跟Activity的intent-filter之间又有什么关系呢？
其实，**getType()返回的MIME类型**，主要就是用来**隐式匹配Intent的MIMETYPE域来启动Activity**的。
下面来看看通过URI来启用Activity的方式：

```java
Intent intent = new Intent();  
intent.setAction("harvic.test.qijian");  
intent.setData(mCurrentURI);  
startActivity(intent);  
```

其中：
```java
public static final String AUTHORITY = "com.harvic.provider.PeopleContentProvider";  
public static final Uri CONTENT_URI_FIRST = Uri.parse("content://" + AUTHORITY + "/first");  
public static Uri mCurrentURI = CONTENT_URI_FIRST;  
```

在上面的代码中，我们设置了action 和 content uri;
这里利用Content URI来启用隐式启用Activity又是怎样一个流程呢？
![](http://upload-images.jianshu.io/upload_images/9028834-b56bb6159bc1109a?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

*   (1)首先，第三方应用通过content Uri和action来隐式匹配Intent来启用Activity.

```java
Intent intent = new Intent();  
intent.setAction("harvic.test.qijian");  
intent.setData(mCurrentURI);  
startActivity(intent);  
```

*   (2)、系统通过URI中的Authority来匹配ContentProvider，从而找到我们的PeopleContentProvider。
*   (3)在找到PeopleContentProvider，由于我们是来匹配Intent的，所以这时候会调用getType(uri)来返回URI类型：

```java
static {  
     sUriMatcher = new UriMatcher(UriMatcher.NO_MATCH);  
     sUriMatcher.addURI(AUTHORITY, "first", MATCH_FIRST);  
     sUriMatcher.addURI(AUTHORITY, "second", MATCH_SECOND);  
    }  
```

上面是UriMather的构造方法，由上面的代码可知，当匹配"/first"时返回MATCH_FIRST即数值1，匹配“/second”时返回MATCH_SECOND，即数值2
所以：
1、当匹配"/fist"时，我们返回自定义的MIME类型：vnd.android.cursor.dir/harvic.first
2、当匹配“/second”时，返回MIME类型：vnd.android.cursor.item/harvic.second
代码如下：

```java
public static final String CONTENT_FIRST_TYPE = "vnd.android.cursor.dir/harvic.first";  
public static final String CONTENT_SECOND_TYPE = "vnd.android.cursor.item/harvic.second";
```

```java
public String getType(Uri uri) {  
    switch (sUriMatcher.match(uri)){  
        case MATCH_FIRST:{  
            return CONTENT_FIRST_TYPE;  
        }  
        case MATCH_SECOND:{  
            return CONTENT_SECOND_TYPE;  
        }  
    }  
    return null;  
}  
```

*   (4)下面就是根据Action和MIME类型来匹配Intent了

我们到现在在ContentProviderBlog项目中还没有一个Activity能匹配这个Action和MIME类型的，所以我们新建一个SecondActivity;

### 新建通过URI启动的Activity
在**AndroidManifest.xml**中，为SecondActivity**添加上隐式匹配所需**要的**Intent-filter**;
注意我们在getType()里根据不同的URI返回了两种MIME类型，而这里的SecondActivity的data域只添加一个**mimeType**:vnd.android.cursor.dir/harvic.first;即当我们使用content://com.harvic.provider.PeopleContentProvider/second来隐式匹配Intent时，是没办法启用SecondActivity的，因为MIME类型不匹配！

```xml
<activity  
    android:name=".SecondActivity"  
    android:label="@string/title_activity_second">  
    <intent-filter>  
        <action android:name="harvic.test.qijian" />  
        <category android:name="android.intent.category.DEFAULT" />  
        <data android:mimeType="vnd.android.cursor.dir/harvic.first" />  
    </intent-filter>  
</activity>  
```

**结果展示**

下面我们看看在使用不同的URI来启用Activity时，会出现什么结果；

**使用content://com.harvic.provider.PeopleContentProvider/first，结果如下：**
点击“thirdPart”,通过URI调起Activity

![image](http://upload-images.jianshu.io/upload_images/9028834-dd225c9f97263f09?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)  ![image](http://upload-images.jianshu.io/upload_images/9028834-62a840853c3b442d?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**使用content://com.harvic.provider.PeopleContentProvider/second,由于MIME不匹配，导致无法调起Activity**

![image](http://upload-images.jianshu.io/upload_images/9028834-822715c8cbbd8dc3?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) ![image](http://upload-images.jianshu.io/upload_images/9028834-e3781e1bce116f4e?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**同样，源码包含两部分内容：**
(先装ContentProviderBlog，再装UseProvider；利用UseProvider操作ContentProviderBlog的数据库，看打出来的LOG)
1、《ContentProviderBlog》：这个是提供共享数据库接口的APP；
2、《UseProvider》：第三方通过URI来操作数据库的APP；

**源码地址：[http://download.csdn.net/detail/harvic880925/8532205](http://download.csdn.net/detail/harvic880925/8532205)**

## 数据库读写权限
### 概述
在AndroidManifest.xml中provider标签中有三个额外的参数**permission、readPermission、writePermission**;
先看下面这段代码：
```xml
<provider  
    android:name=".PeopleContentProvider"  
    android:authorities="com.harvic.provider.PeopleContentProvider"  
    android:exported="true"  
    android:permission="com.harvic.contentProviderBlog"  
    android:readPermission="com.harvic.contentProviderBlog.read"  
    android:writePermission="com.harvic.cotentProviderBlog.write"/>  
```

在这段代码中有几个参数要特别注意一下：
*   **exported:** 这个属性用于指示该服务是否能被其他程序应用组件调用或跟他交互； 取值为（true | false），如果设置成true，则能够被调用或交互，否则不能；设置为false时，只有同一个应用程序的组件或带有相同用户ID的应用程序才能启动或绑定该服务。具体参见：[《Permission Denial: opening provider 隐藏的android:exported属性的含义》](http://blog.csdn.net/guoxiao20sun/article/details/8646024)
*   **readPermission:** 使用Content Provider的查询功能所必需的权限，即**使用ContentProvider**里的**query**()函数的**权限**；

*   **writePermission：** 使用ContentProvider的修改功能所必须的权限，即**使用ContentProvider**的**insert()、update()、delete()**函数的**权限**；

*   **permission：** 客户端读、写 Content Provider 中的数据所必需的权限名称。 
本属性为**一次性设置读和写权限**提供了快捷途径。 不过，readPermission和writePermission属性优先于本设置。 如果同时设置了readPermission属性，则其将控制对 Content Provider 的读取。 如果设置了writePermission属性，则其也将控制对 Content Provider 数据的修改。也就是说如果只设置permission权限，那么拥有这个权限的应用就可以实现对这里的ContentProvider进行读写；如果同时设置了permission和readPermission那么具有readPermission权限的应用才可以读，拥有permission权限的才能写！也就是说只拥有permission权限是不能读的，因为readPermission的优先级要高于permission；如果同时设置了readPermission、writePermission、permission那么permission就无效了。

### 有关自定义权限

由于在privoder标签中要用到**自定义权限**，比如：

```xml
<provider  
    android:name=".PeopleContentProvider"  
    android:authorities="com.harvic.provider.PeopleContentProvider"  
    android:exported="true"  
    android:readPermission="com.harvic.contentProviderBlog.read"/>  
```

比如，在这里我们定义一个readPermission字符串，单纯写一个字符串是毫无意义的，因为没有申请过的一串String代表的字符串系统根本无法识别，也就是说谁进来都会被挡在外面！

所以在application标签的同级目录，写上**申请permission的代码**：
```xml
<permission  
    android:name="com.harvic.contentProviderBlog.read"  
    android:label="provider pomission"  
    android:protectionLevel="normal" />  
```

这样，我们的permission才会在系统中注册，在第三方应用中使用<uses-permission 来声明使用权限时，才有意义；
如果不申请，那么系统中是不存在这个权限的，当第三方应用使用
了<uses-permission 来声明使用权限，因为系统中根本找不到这个权限，所以到provider匹配时，就会出现permission-deny的错误。

```xml
<uses-permission android:name="com.harvic.contentProviderBlog.read"/>  
```

有关自定义权限的内容，请参考[《声明、使用与自定义权限》](http://blog.csdn.net/harvic880925/article/details/38683625)

### 实例：带有权限的ContentProvder
首先在ContentProviderBlog中首先向系统申请两个权限：
分别对应读数据库权限和写数据库权限

```xml
<permission  
    android:name="com.harvic.contentProviderBlog.read"  
    android:label="provider read pomission"  
    android:protectionLevel="normal" />  
<permission  
    android:name="com.harvic.contentProviderBlog.write"  
    android:label="provider write pomission"  
    android:protectionLevel="normal" />  
```

**情况一：使用读写权限**
然后在provider中，我们这里同时使用读、写权限：

```xml
<provider  
    android:name=".PeopleContentProvider"  
    android:authorities="com.harvic.provider.PeopleContentProvider"  
    android:exported="true"  
    android:readPermission="com.harvic.contentProviderBlog.read"  
    android:writePermission="com.harvic.cotentProviderBlog.write" />  
```

在这种情况下，使用第三方应用同时申请使用这两个权限才可以对数据库读写，即：

```xml
<uses-permission android:name="com.harvic.contentProviderBlog.read"/>  
<uses-permission android:name="com.harvic.contentProviderBlog.write"/>  
```

**情况二：仅添加读权限**
如果我们在provider中，仅添加读共享数据库的权限，那第三方应该怎么申请权限才能读写数据库呢？

```xml
<provider  
    android:name=".PeopleContentProvider"  
    android:authorities="com.harvic.provider.PeopleContentProvider"  
    android:exported="true"  
    android:readPermission="com.harvic.contentProviderBlog.read"/>  
```

从上面可以看到，我们只添加了readPermission，那第三方应用在不申请任何权限的情况下是可以写数据库，但当读数据库时就需要权限了；即如果要读数据库需要添加如下使用权限声明：

```
<uses-permission android:name="com.harvic.contentProviderBlog.read"/>  
```

在上一篇的基础上，代码不需要动，只需要在AndroidManifest.xml中写上权限定义与声明，即可；

## ContentObserver
定义：内容观察者
作用：观察 Uri引起 ContentProvider 中的数据变化 & 通知外界（即访问该数据访问者）

### 数据监听步骤

要监听指定URI上的数据库变化，在监听方需要两步：

#### (1)生成一个类派生自ContentObserver
```java
public class DataBaseObserver extends ContentObserver {  
    public DataBaseObserver(Handler handler) {  
        super(handler);  
    }  
  
    @Override  
    public void onChange(boolean selfChange) {  
        super.onChange(selfChange);  
        Log.d("harvic","received first database changed");  
    }  
}  
```

注意这里会自动生成一个onChange()方法，当有改变到来时就会跑到onChange()里，所以我们在这里面打一个LOG，注意LOG内容：“received first database changed”，很明显这里我只用DataBaseObserver来监听"content://com.harvic.provider.PeopleContentProvider/frist "所对应的数据库进行监听，至于如何指定URI往下看

#### (2)注册和反注册监听

*   **在onCreate()时，注册**

在使用URI来操作第三方数据之前，先进行ContentObserver注册，这里我们在MainActivity的OnCreate()函数里注册：

```java
private DataBaseObserver observer;  
  
@Override  
protected void onCreate(Bundle savedInstanceState) {  
    super.onCreate(savedInstanceState);  
    setContentView(R.layout.activity_main);  
    observer = new DataBaseObserver(new Handler());  
    //注册指定URI 变动监听  
    getContentResolver().registerContentObserver(CONTENT_URI_FIRST,true,observer);  
}  
```

这里先完整说完流程，等下再讲registerContentObserver()的参数

*   **在onDestory()时，反注册：**

```java
@Override  
protected void onDestroy() {  
    super.onDestroy();  
    //取消监听  
    getContentResolver().unregisterContentObserver(observer);  
}  
```

#### (3)在ContentProvider中通知数据库变动
在ContentProvider中指定URI上有数据库数据变动时，及时利用下面的函数来通知

```java
getContext().getContentResolver().notifyChange(uri, null);  
```
好了，到这数据库变动通知和监听的过程就讲完了。

### registerContentObserver
下面看看registerContentObserver()注册监听函数的用法：

```java
public final void registerContentObserver(Uri uri, boolean notifyForDescendents, ContentObserver observer)
```

功能：**为指定的Uri注册一个ContentObserver派生类实例**，当**给定**的Uri发生改变时，回调该实例对象去处理。

**参数：**
*   **uri ：**     需要观察的Uri
*   **notifyForDescendents：**  为false 表示精确匹配，即只匹配该Uri; 为true 表示可以同时匹配其派生的Uri。
举例如下:假设UriMatcher 里注册的Uri共有一下类型：
1、content://com.qin.cb/student (学生)
2、content://com.qin.cb/student/# 
3、content://com.qin.cb/student/schoolchild(小学生，派生的Uri) 
假设我们当前需要观察的Uri为content://com.qin.cb/student，如果发生数据变化的 Uri 为 content://com.qin.cb/student/schoolchild；那么当notifyForDescendents为 false，那么该ContentObserver会监听不到， 但是当notifyForDescendents 为ture，能捕捉该Uri的数据库变化。

*   **observer：** ContentObserver的派生类实例



## SetProjectionMap——投影映射

在上面的实例中，我们对外提供的数据库列名和内部使用的是一样的，但这样很容易被对方知道我们的数据库结构，那要想自己本地使用一套列名，给外部提供另一套对应的列名，这样别人不就猜不出我们的列名了么，针对这个问题，在SQLiteQueryBuilder类中，为我们提供了内外部列名映射函数，以允许我们在外部和内部列名不同时的提供映射功能。

主要使用的函数是setProjectionMap(HashMap map)

使用方法如下：

```java
SQLiteQueryBuilder queryBuilder = new SQLiteQueryBuilder();  
Map projectionMap = new HashMap<String,String>();  
projectionMap.put("out_column_name_1","inside_column_name_1");  
projectionMap.put("out_column_name_2","inside_column_name_2");  
projectionMap.put("out_column_name_3","inside_column_name_3");  
queryBuilder.setTables(DatabaseHelper.TABLE_FIRST_NAME);  
queryBuilder.setProjectionMap(projectionMap);  
```

**源码来了，源码内容：**
1、第一部分：数据库读写权限
2、第二部分：数据监听（把读写权限代码删除，只有监听相关的代码）
**源码下载地址：[http://download.csdn.net/detail/harvic880925/8535963](http://download.csdn.net/detail/harvic880925/8535963)**


## 实例


### 自动获取短信验证码
##### 1.自定义监听类
```java
//短信监听器，用于自动填充验证码
public class SMSContentObserver extends ContentObserver {
  public final String SMS_URI_INBOX = "content://sms/inbox";//收信箱
  private Activity activity = null;
  private String smsContent = "";//验证码
  private EditText verifyText = null;//验证码编辑框
  private String SMS_ADDRESS_PRNUMBER = "10690329013589";//短息发送提供商
  private String smsID = "";
  //短信观察者 收到一条短信时 onchange方法会执行两次，所以比较短信id，如果一致则不处理
  public SMSContentObserver(Activity activity, Handler handler, EditText verifyText) {
    super(handler);
    this.activity = activity;
    this.verifyText = verifyText;
  }
  @Override
  public void onChange(boolean selfChange) {
    super.onChange(selfChange);
    Cursor cursor = null;// 光标
    // 读取收件箱中指定号码的短信
    cursor = activity.getContentResolver().query(Uri.parse(SMS_URI_INBOX),
      new String[]{"_id", "address", "body", "read"}, //要读取的属性
      "address=? and read=?", //查询条件是什么
      new String[]{SMS_ADDRESS_PRNUMBER, "0"},//查询条件赋值
      "date desc");//排序
    if (cursor != null) {
      cursor.moveToFirst();
      if (cursor.moveToFirst()) {
        //比较和上次接收到短信的ID是否相等
        if (!smsID.equals(cursor.getString(cursor.getColumnIndex("_id")))) {
          String smsbody = cursor.getString(cursor.getColumnIndex("body"));
          //用正则表达式匹配验证码
          Pattern pattern = Pattern.compile("[0-9]{6}");
          Matcher matcher = pattern.matcher(smsbody);
          if (matcher.find()) {//匹配到6位的验证码
            smsContent = matcher.group();
            if (verifyText != null && null != smsContent && !"".equals(smsContent)) {
              verifyText.requestFocus();//获取焦点
              verifyText.setText(smsContent);//设置文本
              verifyText.setSelection(smsContent.length());//设置光标位置
            }
          }
          smsID = cursor.getString(cursor.getColumnIndex("_id"));
        }
      }
    }
  }
}
```

##### 2.在页面注册监听类

```java
//实例化短信监听器
SMSContentObserver mObserver = new SMSContentObserver(getActivity(), new Handler(), mEt_auth_code);
// 注册短信变化监听
mContext.getContentResolver().registerContentObserver(Uri.parse("content://sms/"), true, mObserver);
```

##### 3.声明读取短信权限
```xml
<uses-permission android:name="android.permission.RECEIVE_SMS" />
<uses-permission android:name="android.permission.READ_SMS" />
<uses-permission android:name="android.permission.WRITE_SMS" />
```

##### 4.为了防止内存泄漏，记得注销监听
```java
@Override
public void onDestroy() {
super.onDestroy();
  //注销短信监听   
  mContext.getContentResolver().unregisterContentObserver(mObserver);
} 
```

小结：去短信库获取短信比较不容易被拦截


### 短信的备份和恢复
Android 系统中提供了一系列的内容提供者，通过调用他们，可以获取一些系统的信息，比如:短信信息，联系人信息等。

#### 准备知识
打开 Android 源码，查看 packages\providers\路径下的工程，这些就是 Android 系 统中的内容提供者，其中 TelephonyProvider 就是短信的内容提供者文件。
- 打开 TelephonyProvider 下的 src 文件，查看 java 文件，其中的 [SmsProvider.java](http://androidxref.com/8.0.0_r4/xref/packages/providers/TelephonyProvider/src/com/android/providers/telephony/SmsProvider.java)即短信息内容提供者逻辑代码。UriMatcher 一般在静态代码块中进行初始化操作，查找静态代码块，找到的逻辑代码如下：
```java
private static final UriMatcher sURLMatcher =
        new UriMatcher(UriMatcher.NO_MATCH);
static {
    sURLMatcher.addURI("sms", null, SMS_ALL);
    sURLMatcher.addURI("sms", "#", SMS_ALL_ID);
    sURLMatcher.addURI("sms", "inbox", SMS_INBOX);
    sURLMatcher.addURI("sms", "inbox/#", SMS_INBOX_ID);
    sURLMatcher.addURI("sms", "sent", SMS_SENT);
    sURLMatcher.addURI("sms", "sent/#", SMS_SENT_ID);
    sURLMatcher.addURI("sms", "draft", SMS_DRAFT);
    sURLMatcher.addURI("sms", "draft/#", SMS_DRAFT_ID);
    sURLMatcher.addURI("sms", "outbox", SMS_OUTBOX);
    sURLMatcher.addURI("sms", "outbox/#", SMS_OUTBOX_ID);
    sURLMatcher.addURI("sms", "undelivered", SMS_UNDELIVERED);
    sURLMatcher.addURI("sms", "failed", SMS_FAILED);
    sURLMatcher.addURI("sms", "failed/#", SMS_FAILED_ID);
    sURLMatcher.addURI("sms", "queued", SMS_QUEUED);
    sURLMatcher.addURI("sms", "conversations", SMS_CONVERSATIONS);
    sURLMatcher.addURI("sms", "conversations/*", SMS_CONVERSATIONS_ID);
    sURLMatcher.addURI("sms", "raw", SMS_RAW_MESSAGE);
    sURLMatcher.addURI("sms", "raw/permanentDelete", SMS_RAW_MESSAGE_PERMANENT_DELETE);
    sURLMatcher.addURI("sms", "attachments", SMS_ATTACHMENT);
    sURLMatcher.addURI("sms", "attachments/#", SMS_ATTACHMENT_ID);
    sURLMatcher.addURI("sms", "threadID", SMS_NEW_THREAD_ID);
    sURLMatcher.addURI("sms", "threadID/*", SMS_QUERY_THREAD_ID);
    sURLMatcher.addURI("sms", "status/#", SMS_STATUS_ID);
    sURLMatcher.addURI("sms", "sr_pending", SMS_STATUS_PENDING);
    sURLMatcher.addURI("sms", "icc", SMS_ALL_ICC);
    sURLMatcher.addURI("sms", "icc/#", SMS_ICC);
    //we keep these for not breaking old applications
    sURLMatcher.addURI("sms", "sim", SMS_ALL_ICC);
    sURLMatcher.addURI("sms", "sim/#", SMS_ICC);
}
```
在数据库中 sms 表就是用于存储短信的，所以通过查找系统源码，可以确定短信息内容提供者的 Uri 应该为:”content://sms"

- 查 看 Android 模 拟 器 下 的 com.android.providers.telephony ， 查看其mmssms.db 文件：
![mmssms.db 文件.png](http://upload-images.jianshu.io/upload_images/9028834-303de8d497e671fa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

打开数据库，其中 sms 表存储的就是短信的数据，其存储格式如下：
[![存储格式.png](http://upload-images.jianshu.io/upload_images/9028834-89af28a518eec02c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://upload-images.jianshu.io/upload_images/9028834-89af28a518eec02c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
其中，address 存储的是联系人号码，date 是发送日期，type 对应短信的类型（发送/接收）,body 是短信的主体内容，准备备份这四项。

#### 实现步骤
##### 获取短信列表
根据content://sms/这个Uri去查询手机中短信的数据库，得到一个Cursor。这个Cursor就包含了一条一条的短信。然后去遍历这个Cursor将我们需要的数据添加到smsList中。
```java
//获取短信列表 
 public List<SmsData> getSmsList() {  
       //获取所有短信的 Uri  
      Uri uri = Uri. parse( "content://sms/");  
       //获取ContentResolver对象  
      ContentResolver contentResolver = mContext.getContentResolver();  
       //根据Uri 查询短信数据  
      Cursor cursor = contentResolver.query(uri, null, null, null, null);  
       if ( null != cursor) {  
           Log. i( TAG, "cursor.getCount():" + cursor.getCount());  
            //根据得到的Cursor一条一条的添加到smsList(短信列表)中  
            while (cursor.moveToNext()) {  
                 int _id = cursor.getInt(cursor.getColumnIndex("_id" ));  
                 int type = cursor.getInt(cursor.getColumnIndex("type" ));  
                String address = cursor.getString(cursor.getColumnIndex( "address"));  
                String body = cursor.getString(cursor.getColumnIndex("body" ));  
                String date = cursor.getString(cursor.getColumnIndex("date" ));  
                SmsData smsData = new SmsData(_id, type, address, body, date);  
                 smsList.add(smsData);  
           }  
           cursor.close();  
      }  
       return smsList;  
}  
```
##### 将短信数据保存到xml文件中
使用XmlSerializer将smsList序列化，待我们需要恢复的时候使用XmlPullParser 将其反序列化，就可以拿到备份的数据。

```java
//将短信数据保存到 sd卡中
public void saveSmsToSdCard(){  
            smsList=getSmsList();  
            //获得一个序列化对象  
           XmlSerializer xmlSerializer=Xml. newSerializer();  
            //将生成的 xml文件保存到sd 卡中名字为"sms.xml"  
           File file= new File(Environment.getExternalStorageDirectory(), "sms.xml");  
           FileOutputStream fos;  
             
            try {  
                fos = new FileOutputStream(file);  
                xmlSerializer.setOutput(fos, "utf-8");  
                xmlSerializer.startDocument( "utf-8", true);  
                xmlSerializer.startTag( null, "smss");  
                  
                xmlSerializer.startTag( null, "count");  
                xmlSerializer.text( smsList.size()+ "");  
                xmlSerializer.endTag( null, "count");  
                  
                 for(SmsData smsData: smsList){  
                       
                     xmlSerializer.startTag( null, "sms");  
                       
                     xmlSerializer.startTag( null, "_id");  
                     xmlSerializer.text(smsData.get_id()+ "");  
                     System. out.println( "smsData.get_id()=" +smsData.get_id());  
                     xmlSerializer.endTag( null, "_id");  
                       
                     xmlSerializer.startTag( null, "type");  
                     xmlSerializer.text(smsData.getType()+ "");  
                     System. out.println( "smsData.getType=" +smsData.getType());  
                     xmlSerializer.endTag( null, "type");  
                       
                     xmlSerializer.startTag( null, "address");  
                     xmlSerializer.text(smsData.getAddress()+ "");  
                     System. out.println( "smsData.getAddress()=" +smsData.getAddress());  
                     xmlSerializer.endTag( null, "address");  
                       
                     xmlSerializer.startTag( null, "body");  
                     xmlSerializer.text(smsData.getBody()+ "");  
                     System. out.println( "smsData.getBody()=" +smsData.getBody());  
                     xmlSerializer.endTag( null, "body");  
                       
                     xmlSerializer.startTag( null, "date");  
                     xmlSerializer.text(smsData.getDate()+ "");  
                     System. out.println( "smsData.getDate()=" +smsData.getDate());  
                     xmlSerializer.endTag( null, "date");  
                       
                     xmlSerializer.endTag( null, "sms");  
                }  
                xmlSerializer.endTag( null, "smss");  
                xmlSerializer.endDocument();  
                  
                fos.flush();  
                fos.close();  
                Toast. makeText( mContext, "备份完成", Toast.LENGTH_SHORT ).show();  
           } catch (FileNotFoundException e) {  
                e.printStackTrace();  
           } catch (IllegalArgumentException e) {  
                e.printStackTrace();  
           } catch (IllegalStateException e) {  
                e.printStackTrace();  
           } catch (IOException e) {  
                e.printStackTrace();  
           }        
     }  
```
通过调用以上方法就将短信以xml的形式保存到了本地。运行这个方法后可以再sd卡中看到sms.xml文件，将其导出来可以看到它的格式如下：
![](http://upload-images.jianshu.io/upload_images/9028834-d1171cde3362d20c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
注：实际上xml文件中它是一行这里为了让大家更清楚的看清结构，我手动将其改成上述格式了。

##### 恢复短信
判断xml的END_TAG是不是"sms"，如果是的话说明一条短信的"type"、"address"、"body"、"date"这些信息已经拿到，然后就根据Uri将这条数据插入到数据库中，就完成一条短信的恢复，待读到 END_DOCUMENT说明xml文件已经读完，此时所备份的短信就全部恢复了。
```java
//将指定路径的 xml文件中的数据插入到短信数据库中 
 public void restoreSms(String path) {  
      File file = new File(path);  
       //得到一个解析 xml的对象  
      XmlPullParser parser = Xml. newPullParser();  
       try {  
            fis = new FileInputStream(file);  
           parser.setInput( fis, "utf-8");  
           ContentValues values = null;  
            int type = parser.getEventType();  
            while (type != XmlPullParser. END_DOCUMENT) {  
                 switch (type) {  
                 case XmlPullParser. START_TAG:  
                       if ( "count".equals(parser.getName())) {  
                      } else if ("sms" .equals(parser.getName())) {  
                           values = new ContentValues();  
                      } else if ("type" .equals(parser.getName())) {  
                           values.put( "type", parser.nextText());  
                      } else if ("address" .equals(parser.getName())) {  
                           values.put( "address", parser.nextText());  
                      } else if ("body" .equals(parser.getName())) {  
                           values.put( "body", parser.nextText());  
                      } else if ("date" .equals(parser.getName())) {  
                           values.put( "date", parser.nextText());  
                      }  
                       break;  
                 case XmlPullParser. END_TAG:  
                       if ( "sms".equals(parser.getName())) {// 如果节点是 sms  
                           Uri uri = Uri.parse( "content://sms/");  
                           ContentResolver resolver = mContext.getContentResolver();  
                           resolver.insert(uri, values);//向数据库中插入数据  
                           System. out.println( "插入成功" );  
                           values = null; //  
                      }  
                       break;  
                }  
                type=parser.next();  
           }  
      } catch (FileNotFoundException e) {  
           e.printStackTrace();  
      } catch (XmlPullParserException e) {  
           e.printStackTrace();  
      } catch (NumberFormatException e) {  
           e.printStackTrace();  
      } catch (IOException e) {  
           e.printStackTrace();  
      }  
}  
```
Tips：在该工程中，短信的恢复逻辑比较简单，只是把 sms.xml 文件中的所有短信插入到
短信数据库中，其实如果短信数据库中还有相同的短信的时候我们这样处理的结果是数据库
中有了 2 份相同的短信，因此比较好的做法是在插入数据库之前先判断该短信是否存在，
如果存在则不插入。

### 操作系统联系人
#### 准备知识
- 通过 DDMS，查看 Android 模拟器下的 com.android.providers.contacts 包下的数据库，查看其 contact2.db 数据库的内容。
　![contact2.db.png](http://upload-images.jianshu.io/upload_images/9028834-aa555d1657afa5a0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 查看数据库，其中 raw_contacts 表存放的是联系人条数信息，data 表中存放的是raw_contacts 中的每一条 id 对应的具体信息，不同类型的信息由 mimetype_id 来标识。
**raw_contacts 表**：
[![raw_contacts 表.png](http://upload-images.jianshu.io/upload_images/9028834-7a9e55337cca92c1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://upload-images.jianshu.io/upload_images/9028834-7a9e55337cca92c1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
．
**data 表**：
[![data 表.png](http://upload-images.jianshu.io/upload_images/9028834-cb6711a2f0bfa72c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://upload-images.jianshu.io/upload_images/9028834-cb6711a2f0bfa72c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
．
**mimetypes 表**：
[![mimetypes 表.png](http://upload-images.jianshu.io/upload_images/9028834-322cff830d7ef938.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://upload-images.jianshu.io/upload_images/9028834-322cff830d7ef938.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


- 打 开 Android 源 码 ， 查 看 packages\providers\ 路 径 下 的 文 件 ， 其 中[ContactsProvider](http://androidxref.com/8.0.0_r4/xref/packages/providers/ContactsProvider/) 就是联系人的内容提供者。
1. 打开清单文件，寻找联系人的内容提供者对应的是哪个 java 文件
```xml
<provider android:name="ContactsProvider2"
    android:authorities="contacts;com.android.contacts"
    android:label="@string/provider_label"
    android:multiprocess="false"
    android:exported="true"
    android:grantUriPermissions="true"
    android:readPermission="android.permission.READ_CONTACTS"
    android:writePermission="android.permission.WRITE_CONTACTS"
    android:visibleToInstantApps="true">
    <path-permission
            android:pathPrefix="/search_suggest_query"
            android:readPermission="android.permission.GLOBAL_SEARCH" />
    <path-permission
            android:pathPrefix="/search_suggest_shortcut"
            android:readPermission="android.permission.GLOBAL_SEARCH" />
    <path-permission
            android:pathPattern="/contacts/.*/photo"
            android:readPermission="android.permission.GLOBAL_SEARCH" />
    <grant-uri-permission android:pathPattern=".*" />
</provider>
```
2. 打开 ContactsProvider2.java 文件，查看此内容提供者的 uri 路径信息

```java
matcher.addURI(ContactsContract.AUTHORITY, "contacts", CONTACTS);
matcher.addURI(ContactsContract.AUTHORITY, "contacts/#", CONTACTS_ID);
matcher.addURI(ContactsContract.AUTHORITY, "contacts/#/data", CONTACTS_DATA);
matcher.addURI(ContactsContract.AUTHORITY, "raw_contacts", RAW_CONTACTS);
matcher.addURI(ContactsContract.AUTHORITY, "raw_contacts/#", RAW_CONTACTS_ID);
matcher.addURI(ContactsContract.AUTHORITY, "raw_contacts/#/data", RAW_CONTACTS_DATA);
```
3. 根据源码，确定内容提供者的 Uri 信息为：
操作 raw_contacts 表的 Uri:
```
content://com.android.contacts/raw_contacts
```

操作 data 表的 Uri：
```
content://com.android.contacts/data
```

4. 操作数据库表时 注意：由于 contacts2.db 数据库使用了视图，所以操作数据库表时，表结构有所改变，注意操作时要操纵的列的列名已经改变。
比如：data 表在查询的时候没有 mimetype_id,取代的是 mimetype


#### 实现步骤
1. 操作 raw_contacts 表，获取全部的 id
2. 根据获取到的每一条 id 去查询 data 表中的数据
3. 将查询到的展示是用户界面
4. 通过代码给系统联系人插入一条联系人信

```java
public class MainActivity extends Activity {
    private ListView lv;
    private List<Contact> list;
    private MyAdapter adapter;
    private Handler handler = new Handler() {
        public void handleMessage(android.os.Message msg) {
            Toast.makeText(MainActivity.this, "数据获取成功。", 1).show();
            //更新数据
            adapter.notifyDataSetChanged();
        };
    };

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        lv = (ListView) findViewById(R.id.lv);
        list = new ArrayList<Contact>();
        adapter = new MyAdapter();
        lv.setAdapter(adapter);
    }

    //查询所有的系统联系人，这里的业务放在子线程中操作
    public void queryContacts(View view) {
        new Thread(new Runnable() {
            @Override
            public void run() {
                ContentResolver contentResolver = getContentResolver();
                Uri uri = Uri.parse("content://com.android.contacts/raw_contacts");
                Cursor cursor = contentResolver.query(uri, new String[]{"contact_id"}, null, null, null);
                list.clear();
                int i = 0;
                while (cursor.moveToNext()) {
                    i++;
                    //先从 raw_contacts 表中获取 contact_id
                    String contact_id = cursor.getString(0);
                    if (TextUtils.isEmpty(contact_id)) {
                        continue;
                    }
                    //根据 contact_id 从 data 表中查询具体的数据
                    Cursor cursor2 = contentResolver.query(
                                    Uri.parse("content://com.android.contacts/data"),
                                    new String[]{ " data1", " mimetype" },
                                    " contact_id = ? ", new String[]{contact_id}, null);
                    Contact contact = new Contact();
                    while (cursor2.moveToNext()) {
                        String data = cursor2.getString(0);
                        String mimetype = cursor2.getString(1);
                        if ("vnd.android.cursor.item/name".equals(mimetype)) {
                            contact.setName(data);
                        } else if ("vnd.android.cursor.item/phone_v2".equals(mimetype)) {
                            contact.setPhone(data);
                        } else if ("vnd.android.cursor.item/email_v2".equals(mimetype)) {
                            contact.setEmail(data);
                        } else {
                            contact.setOther(data);
                        }
                    }
                    list.add(contact);
                    cursor2.close();
                }
                cursor.close();
                //发送一个空消息，更新 ListView
                handler.sendEmptyMessage(RESULT_OK);
            }
        }).start();
    }

    class MyAdapter extends BaseAdapter {
        @Override
        public int getCount() {
            return list.size();
        }

        @Override
        public Object getItem(int position) {
            return null;
        }

        @Override
        public long getItemId(int position) {
            return position;
        }

        //这里面通过 ViewHolder 类将其他子属性值绑定在 View 上面，这里对ListView 作了进一步的优化处理
        @Override
        public View getView(int position, View convertView, ViewGroup parent) {
            View view;
            ViewHolder holder;
            if (convertView != null) {
                view = convertView;
                holder = (ViewHolder) view.getTag();
            } else {
                view = View.inflate(MainActivity.this, R.layout.list_item, null);
                holder = new ViewHolder();
                holder.tv_name = (TextView) view.findViewById(R.id.tv_name);
                holder.tv_phone = (TextView) view.findViewById(R.id.tv_phone);
                holder.tv_email = (TextView) view.findViewById(R.id.tv_email);
                view.setTag(holder);
            }
            Contact contact = list.get(position);
            holder.tv_name.setText(contact.getName());
            holder.tv_email.setText(contact.getEmail());
            holder.tv_phone.setText(contact.getPhone());
            return view;
        }
    }

    class ViewHolder {
        TextView tv_name;
        TextView tv_email;
        TextView tv_phone;
    }

    //往系统联系人表中插入一条数据，这里为了方便演示，我们直接插入一条固定的数据
    public void insertContacts(View view) {
        //创建一个自定义的 Contact 类，将要往系统联系人表中插入的字段封装起来
        Contact contact = new Contact();
        contact.setEmail("itheima@itcast.cn");
        contact.setName("王二麻子" + new Random().nextInt(1000));
        contact.setPhone("9999999" + new Random().nextInt(100));
        contact.setOther("北京市中关村软件园");
        //获取 ContentResolver 对象
        ContentResolver resolver = getContentResolver();
        //操作 raw_contacts 表的 uri
        Uri raw_uri = Uri.parse("content://com.android.contacts/raw_contacts");
        //操作 data 表的 uri
        Uri data_uri = Uri.parse("content://com.android.contacts/data");
        //在插入数据之前先查询出当前最大的 id
        Cursor cursor = resolver.query(raw_uri, new String[]{"contact_id"}, null, null, "contact_id desc limit 1");
        int id = 1;
        if (cursor != null) {
            boolean moveToFirst = cursor.moveToFirst();
            if (moveToFirst) {
                id = cursor.getInt(0);
            }
        }
        cursor.close();
        //要插入数据的 contact_id 值
        int newId = id + 1;
        // 给 raw_contact 表中添加一天记录
        ContentValues values = new ContentValues();
        values.put("contact_id", newId);
        resolver.insert(raw_uri, values);
	// 在 data 表中添加数据
        // 添加 name
        values = new ContentValues();
        values.put("raw_contact_id", newId);
        values.put("mimetype", "vnd.android.cursor.item/name");
        values.put("data1", contact.getName());
        resolver.insert(data_uri, values);
        // 添加 phone
        values = new ContentValues();
        values.put("raw_contact_id", newId);
        values.put("mimetype", "vnd.android.cursor.item/phone_v2");
        values.put("data1", contact.getPhone());
        resolver.insert(data_uri, values);
        // 添加地址
        values = new ContentValues();
        values.put("raw_contact_id", newId);
        values.put("mimetype", "vnd.android.cursor.item/postal-address_v2");
        values.put("data1", contact.getOther());
        resolver.insert(data_uri, values);
        // 添加 email
        values = new ContentValues();
        values.put("raw_contact_id", newId);
        values.put("mimetype", "vnd.android.cursor.item/email_v2");
        values.put("data1", contact.getEmail());
        resolver.insert(data_uri, values);
        Toast.makeText(this, "成功插入联系人" + contact, 0).show();
    }
}
```
在 AndroidManifest.xml 中添加添加如下**权限**：
```xml
<uses-permission android:name="android.permission.READ_CONTACTS"/>
<uses-permission android:name="android.permission.WRITE_CONTACTS"/>
```

# Android ContentProvider面试题
## ContentProvider如何实现数据共享？
在 Android 中如果想将自己应用的数据（一般多为数据库中的数据）提供给第三发应用，那么我们只能通过 ContentProvider 来实现了。
ContentProvider 是应用程序之间共享数据的接口。使用的时候首先自定义一个类继承 ContentProvider，然后 覆写 query、insert、update、delete 等方法。因为其是四大组件之一因此必须在 AndroidManifest 文件中进行注册。
```xml
<provider android:exported="true" 
	android:name="com.itheima.contenProvider.provider.PersonContentProvider"
	android:authorities="com.itheima.person" />
```
第三方可以通过 ContentResolver 来访问该 Provider


## 为什么要用 ContentProvider？它和 sql 的实现上有什么差别？

ContentProvider 屏蔽了数据存储的细节,内部实现对用户完全透明,用户只需要关心操作数据的 uri 就可以了， ContentProvider 可以实现不同 app 之间共享。
Sql 也有增删改查的方法，但是 sql 只能查询本应用下的数据库。而 ContentProvider 还可以去增删改查本地文件. xml 文件的读取等。

## ContentProvider、ContentResolver、ContentObserver 之间的关系
**ContentProvider** 内容提供者，用于对外提供数据
ContentResolver.notifyChange(uri)发出消息
**ContentResolver** 内容解析者，用于获取内容提供者提供的数据
**ContentObserver** 内容监听器，可以监听数据的改变状态
ContentResolver.registerContentObserver()监听消息。

## 如何访问 asserts 资源目录下的数据库？

```java
//获取到 assert 目录下的 db 文件
    AssetManager assetManager = getAssets();
    InputStream is = assetManager.open("myuser.db");
//将文件拷贝到/data / data / com.itheima.android.asserts.sqlite / databases / myuser.db
//如果 databases 目录不存在则创建
    File file = new File("/data/data/com.itheima.android.asserts.sqlite/databases");
    if (!file.exists()) {
        file.mkdirs();
    }
    FileOutputStream fos = new FileOutputStream(new File(file, "myuser.db"));
    byte[] buff = new byte[1024 * 8];
    int len = -1;
    while ((len = is.read(buff)) != -1) {
        fos.write(buff, 0, len);
    }
    fos.close();
    is.close();
//访问数据库
    SQLiteDatabase database = openOrCreateDatabase("myuser.db", MODE_PRIVATE, null);
    String sql = "select c_name from t_user";
    Cursor cursor = database.rawQuery(sql, null);
    while (cursor.moveToNext()) {
        String string = cursor.getString(0);
        Log.d("tag", string);
    }
    cursor.close();
    database.close();
}
```


## 如何在高并发下进行数据库查询？
（2015-11-25）
（这个问题的回答很广泛，可以自由发挥） 比如：不要关联多表查询，减少链接时间，创建索引、将查询到的数据采用缓存策略等等。






引用：
★★★[1、《ContentProvider数据库共享之——概述》](http://blog.csdn.net/harvic880925/article/details/44521461)
★★★[2、《ContentProvider数据库共享之——实例讲解》](http://blog.csdn.net/harvic880925/article/details/44591631)
★★★[3、《ContentProvider数据库共享之——MIME类型与getType()》](http://blog.csdn.net/harvic880925/article/details/44620851)
★★★[4、《ContentProvider数据库共享之——读写权限与数据监听》](http://blog.csdn.net/harvic880925/article/details/44651967)

[Android开发之调用系统的ContentProvider——短信的备份和恢复](http://blog.csdn.net/dmk877/article/details/50518464)
[Android自动获取短信验证码功能](http://www.jb51.net/article/111074.htm)






】


【Android 进程和线程】
【
# 【Android 进程和线程】

>相关文章：[JAVA 线程](http://blog.csdn.net/moira33/article/details/78894952)


我们都知道，在操作系统中**进程是OS分配资源的最小单位**，而**线程是执行任务的最小单位**。
一个进程可以拥有多个线程执行任务，这些线程可以共享该进程分配到的资源。

当我们的**app启动**运行后，在该app**没有其他组件正在运行**的前提下，Android系统会**启动一个新Linux进程来运行app**，这个进程只包含了一个线程在运行。在**默认**情况下，app的组件都运行在该进程中，最初就包含的这个线程也被称为**主线程或者UI线程**。如果我们启动该app的时候，系统中已经有一个进程在运行该app的组件，那么该app也会在该进程中运行。

当然，我们也可以让app中不同的组件运行在不同的进程中，也可以在任意进程中新开线程执行任务。

# Android 进程
## 进程生命周期
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



】


【Android SQLite】
【
# 【Android SQLite】
## SQLite 简介
SQLite 是一款内置到移动设备上的轻量型的数据库，是遵守 ACID（原子性、一致性、隔离性、持久 性）的关联式数据库管理系统，多用于嵌入式系统中。
SQLite 数据库是无类型的，可以向一个 integer 的列中添加一个字符串，但它又支持常见的类型比如: NULL，VARCHAR, TEXT, INTEGER, BLOB, CLOB 等。
Android 系统内置了 SQLite，并提供了一系列 API 方便对其进行操作。
## 使用 SQLiteOpenHelper 创建数据库
SQLiteOpenHelper 是 Android 提供的一个抽象工具类，负责管理数据库的创建、升级工作。如果我们 想创建数据库，就需要自定义一个类继承 SQLiteOpenHelper，然后覆写其中的抽象方法。
### 创建 SQLiteOpenHelper 类
【文件 1-1】 MySQLiteOpenHelper.java
```java
public class MySQLiteOpenHelper extends SQLiteOpenHelper {
    private static final String TAG = "MySQLiteOpenHelper";
    //定义数据库文件名
    public static final String TABLE_NAME = "myuser.db";

    /**
     * @param context
     * @param name    数据库文件的名称
     * @param factory null
     * @param version 数据库文件的版本
     */
    private MySQLiteOpenHelper(Context context, String name, SQLiteDatabase.CursorFactory factory,
                               int version) {
        super(context, name, factory, version);
    }

    // 对外提供构造函数
    public MySQLiteOpenHelper(Context context, int version) {
		//调用该类中的私有构造函数
        this(context, TABLE_NAME, null, version);
    }

    // 当第一次创建数据的时候回调方法
    @Override
    public void onCreate(SQLiteDatabase db) {
        Log.d(TAG, "onCreate");
//创建数据库表的语句
        String sql = "create table t_user(uid integer primary key not null, c_name varchar (20), c_age integer, c_phone varchar(20))";
        db.execSQL(sql);
    }

    // 当数据库升级是回调该方法
    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
        Log.d(TAG, "onUpgrade: oldVersion" + oldVersion + " newVersion=" + newVersion);
        String sql = "alter table t_user add c_money float";
        db.execSQL(sql);
    }

    // 当数据库被打开时回调该方法
    @Override
    public void onOpen(SQLiteDatabase db) {
        Log.d(TAG, "onOpen");
    }
}
```

注意：
上面的代码我们只是定义了一个 MySQLiteOpenHelper 类继承了 SQLiteOpenHelper 类。在 onCreate()方法中通过执行 sql 语句实现表的创建。

### 使用 SQLiteOpenHelper 类
在 Activity 中可以执行如【文件 1-2】所示代码。如果第一次执行，则会创建一个数据库文件，创建的数据库文件位/data/data/包名/databases/目录中。
【文件 1-2】 代码片段
```java
	/* 通过构造函数创建一个 MySQLiteOpenHelper 对象，
	* 此时数据库文件还未创建	*/
	MySQLiteOpenHelper openHelper = new MySQLiteOpenHelper(this, VERSION);
	// 调用以下任一方法可使数据库文件得以创建
	openHelper.getWritableDatabase();
 //	openHelper.getReadableDatabase();
	//关闭资源
	openHelper.close();
```

注意：
如果 openHelper.getWritableDatabase();或者 openHelper.getReadableDatabase();是**第一次被调用**，那么数据库文件**才**会被**创建**，否则只打开不创建。

创建的数据库文件如图 1-1 所示，包含两个文件，一个就是我们自定义的名称 myuser.db，另外一个 myuser.db-journal，该文件会被自动创建，是 sqlite 的一个临时的日志文件，主要用于 sqlite 数据库的事务 回滚操作。
![](http://upload-images.jianshu.io/upload_images/9028834-e7bbfa1aff4f1a07.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
图 1-1	database 文件

## 数据库的增删改查
以上创建了 MySQLiteOpenHelper 类，通过该类可以获取 SQLiteDatabase 对象。而 **SQLiteDatabase**对象则可是实现**对数据库**的**增删改查**操作。
 
对数据库的增删改查我分为两种方式：
### 1、使用纯 SQL 语句实现
#### 添加数据
```java
// 纯 SQL 方式添加数据
//通过 SQLiteOpenHelper 对象获取 SQLiteDatabase
SQLiteDatabase database = mOpenHelper.getWritableDatabase();
String sql = "insert into t_user (c_name,c_age,c_phone) values (?,?,?)";
// 执行数据库，参数以 Object[]的形式传递
database.execSQL(sql, new Object[] { 
				"zhangsan" + new Random().nextInt(100),
				new Random().nextInt(30), 
				"" + (5550 + new Random().nextInt(100)) });
//关闭数据库释放资源
database.close();
```

#### 删除数据
```java
// 删除年龄小于 21 的用户
SQLiteDatabase database = mOpenHelper.getReadableDatabase();
String sql = "delete from t_user where c_age<?";
database.execSQL(sql, new Object[] { 21 });
database.close();
```

#### 修改数据
```java
SQLiteDatabase database = mOpenHelper.getWritableDatabase();
String sql = "update t_user set c_name=? where c_age=?"; 
database.execSQL(sql, new Object[] { "lisi", 25 });
database.close();
```

#### 查询数据
【文件 1-6】 查询数据代码

```java
public void query1(View view) {
    //将查询到的数据封装到 User 集合中
    ArrayList<User> users = new ArrayList<User>();
    //获取 SQLiteDatabase
    SQLiteDatabase database = mOpenHelper.getReadableDatabase();
    String sql = "select uid,c_name,c_age,c_phone from t_user where c_age<?";
	// 执行查询语句，返回游标对象，可以将游标看做指向结果集的指针
    Cursor cursor = database.rawQuery(sql, new String[]{"130"});
    // 游标的默认位置位于第一行数据的前面
    while (cursor.moveToNext()) {
        User user = new User();
        //从第 1 列获取 int 型数据，字段的顺序是由查询语句决定的
        int uid = cursor.getInt(0);
        //从第 2 列获取字符串数据
        String name = cursor.getString(1);
        //从第 3 列获取 int 型数据
        int age = cursor.getInt(2);
        String phone = cursor.getString(3);
        user.setAge(age);
        user.setName(name);
        user.setPhone(phone);
        user.setUid(uid);
        users.add(user);
    }
    cursor.close();
    database.close();
}
```
注意：
使用纯 SQL 语句操作适合 SQL 比较熟练的程序员。如果 SQL 掌握的不好，没关系，Android 提供了一套 API 可以帮助我们完成以上操作。

### 2、使用特有 API 实现
#### 添加数据
```java
//获取 SQLiteDatabase
SQLiteDatabase database = mOpenHelper.getWritableDatabase();
/* ContentValues底层是 Map 数据结构
* key 对应数据库表中字段
* value 是想插入的值 */
ContentValues values = new ContentValues();
values.put("c_name", "王五" + new Random().nextInt(10));
values.put("c_age", new Random().nextInt(50));
values.put("c_phone", "5556");
/*
* 第一个参数：表名，注意不是数据库名！
* 第二个参数：如果 ContentValues 为空，那么默认情况下是不允许插入空值的，
* 但是如果给该参数设置了一个指定的列名，那么就允许 ContentValues 为空，
*同时给该列插入 null 值。
* 第三个参数：要插入的数值，封装成了 ContentValues 对象\
* 返回 long 类型的值，代表该条记录在数据库中的 id */
long insert = database.insert(TABLE_NAME, null, values);
//释放资源
database.close();
```

####  删除数据
```java
SQLiteDatabase database = mOpenHelper.getReadableDatabase();
/*
* 第一个参数：表名
* 第二个参数：删除条件，比如 c_age=?或者 c_age<?
* 第三个参数：参数，用于替换？
* 返回值：删除成功的个数 */
int delete = database.delete(TABLE_NAME, "c_age<?", new String[] { "25" });
database.close();
```

#### 修改数据
```java
SQLiteDatabase database = mOpenHelper.getWritableDatabase();
ContentValues values = new ContentValues();
values.put("c_name", "wangwu");
// 将年龄等于 28 的姓名改为 wangwu
int update = database.update(TABLE_NAME, values, "c_age=?", new String[] { "28" });
database.close();
```

#### 查询数据
```java
ArrayList<User> users = new ArrayList<User>();
SQLiteDatabase database = mOpenHelper.getReadableDatabase();
/*
* 参数 1：表名
* 参数 2：要查询的字段
* 参数 3：查询条件
* 参数 4：条件中？对应的值
* 参数 5：分组查询参数
* 参数 6：分组查询条件
* 参数 7：排序字段和规则 */
Cursor cursor = database.query(TABLE_NAME, 
				new String[] {"uid", "c_name", "c_age", "c_phone" }, 
				"c_age<?", new String[] { "130" }, null, null, "c_age desc");
//对游标 Cursor 的遍历
while (cursor.moveToNext()) {
	User user = new User();
	int uid = cursor.getInt(0);
	String name = cursor.getString(1);
	int age = cursor.getInt(2);
	String phone = cursor.getString(3);
	user.setAge(age);
	user.setName(name);
	user.setPhone(phone);
	user.setUid(uid);
	users.add(user);
}
cursor.close();
database.close();
```

### 两种 SQLiteDatabase 的不同
SQLiteOpenHelper 有两个方法均可返回 SQLiteDatabase 对象：
一、getWritableDatabase()
该方法返回的对象和另外一个方法返回的对象没有任何差异，返回的对象对数据库都可以进行读、写操作，当磁盘已满或者权限不足的情况下该方法会抛出异常。

二、getReadableDatabase()
跟另外一个方法相比，在磁盘已满的情况下，该方法不会抛出异常，而是返回一个只读的数据库操作对象。 
根据这两种方法返回对象的差异，如果需要对数据库进行**查询操作则推荐使用后者**，如果**添加、修改、删除数据则推荐使用前者**。
 
## 数据库的升级和事务操作
### 数据库的升级
在创建 MySQLiteOpenHelper 对象的时候需要传递一个 int 类型的 version 参数，代表数据库的版本号，数值从 1 开始。如果在 new	MySQLiteOpenHelper 对象的时候传递的 version 大于先去创建的 version，那 么就会导致系统回调 onUpgrade 方法，从而实现了数据库的升级。

### 事务的操作
在 SQLiteDatabase 提供了对事务的支持，**处于事务中的操作都是“临时性”的**，只有事务**提交了 才会将数据保存到数据库**。
事务的使用不仅可以**保证数据**的**一致性**，也可以**提高批处理**时的执行**效率**。

SQLiteDatabase 提供的 **beginTransaction()打开事务**，**endTransaction()结束事务**。
注意：结束 事务并不代表事务提交，如果想让数据写入的数据库需要在结束事务前执行 **setTransactionSuccessful**()方法。这是**事务提交**的唯一方式。

如下：展示了当开启事务后，如果因为异常导致事务没有提提交，那么整个转账过程都不会成功。
```
//数据库的事务操作
SQLiteDatabase database = mOpenHelper.getReadableDatabase();
String sql = "update t_user set c_money=c_money-500 where c_name=?";
// 模拟转账
// 开启了事务，那么之后的操作都是在缓存中执行
database.beginTransaction();
database.execSQL(sql, new String[] { "wangwu" });
 
// 在事务中模拟一个异常的发生
int a = 1 / 0;
String sql2 = "update t_user set c_money=c_money+500 where c_name=?";
database.execSQL(sql2, new String[] { "lisi" });

// 只有执行该代码，才将缓存中的数据写到数据库中
database.setTransactionSuccessful();
database.endTransaction();
database.close();
```

如下：展示了如果批处理数据时，使用事务可以显著提高效率。运行结果见图 1-2。
【文件 1-13】	代码片段

```java
// 测试批处理效率
public void testBatch(View view) {
    SQLiteDatabase database = mOpenHelper.getWritableDatabase();
// 普通方式添加 10000 条记录
    String sql = "insert into t_user (c_name,c_age,c_phone) values (?,?,?)";
    long startTime = SystemClock.currentThreadTimeMillis();
    for (int i = 0; i < 1000; i++) {
        database.execSQL(sql, new Object[] { "zhangsan" + new Random().nextInt(100),
                new Random().nextInt(30), "" + (5550 + new Random().nextInt(100)) });
    }
    System.out.println("普通模式耗时："+(SystemClock.currentThreadTimeMillis()-startTime)+"毫秒");
// 开启事务的方式添加 1000 条记录
    startTime = SystemClock.currentThreadTimeMillis();
    database.beginTransaction();
    for(int i=0;i<1000;i++){
        database.execSQL(sql, new Object[] { "zhangsan" + new Random().nextInt(100),
                new Random().nextInt(30), "" + (5550 + new Random().nextInt(100)) });
    }
    database.setTransactionSuccessful();
    database.endTransaction();
    System.out.println("事务模式耗时："+(SystemClock.currentThreadTimeMillis()-startTime)+"毫秒");
    // 关闭数据库释放资源
    database.close();
}
```
![](http://upload-images.jianshu.io/upload_images/9028834-f1603995bbae51db.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
图 1-2	事务执行效率对比
 
通过如图 1-2 的测试结果我们发现使用事务大大提高了批量处理的效率。

## sqlite3 工具的使用
sqlite3 是 Android 内置的操作数据库工具，使用该工具可以直接对 SQLite 数据库进行操作。 操作步骤（如图 1-3）：
1、在命令行界面使用 adb shell 命令进入 linux 内核
2、使用 cd 命令进入数据库所在目录（数据库的路径为”/data/data/应用包名/databases/数据库”）
3、使用”sqlite3 数据库名”进入数据库操作模式
![](http://upload-images.jianshu.io/upload_images/9028834-e859939b8d4f14af.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) 
图 1-3	sqlite3 使用截图

】


【Android 自定义View】
【
# 【Android 自定义View】

# 自定义View基础

接触到一个类，你不太了解他，如果贸然翻阅源码只会让你失去方向，不知从哪里下手；所以我们应该从文档着手，看看它是个什么东西，里面有哪些属性和方法，都是用来干嘛的。下面我们看看官方文档对View的介绍：

> View这个类代表用户界面组件的基本构建块。View在屏幕上占据一个矩形区域，并负责绘制和事件处理。View是用于创建交互式用户界面组件（按钮、文本等）的基础类。它的子类ViewGroup是所有布局的父类，它是一个可以包含其他view或者viewGroup并定义它们的布局属性的看不见的容器。 
> 实现一个自定义View，你通常会覆盖一些framework层在所有view上调用的标准方法。你不需要重写所有这些方法。事实上，你可以只是重写**onDraw**(android.graphics.Canvas)。

| 分类        | 方法                                       | 说明                                       |
| :-------- | :--------------------------------------- | :--------------------------------------- |
| 构造        | **Constructors**                         | 两种形式：1.使用代码创建时构造方法被调用，2.View被布局文件填充时构造方法被调用。<br>第二种形式应该解析和使用一些定义在布局文件中属性 |
|           | **onFinishInflate**()                    | 当View和他的所有子控件被XML布局文件填充完成时被调用。（这个方法里面可以完成一些**初始化**，比如初始化子控件） |
| 布局        | **onMeasure**(int, int)                  | 当决定view和他的孩子的**尺寸**需求时被调用（也就是测量控件大小时调用）  |
|           | **onLayout**(boolean, int, int, int, int) | 当View给他的孩子**分配大小和位置**的时候调用（摆放子控件）        |
|           | **onSizeChanged**(int, int, int, int)    | 当view**大小**发生**变化**时调用                   |
| 绘制        | **onDraw**(android.graphics.Canvas)      | 当视图应该呈现其内容时调用（绘制）                        |
| 事件处理      | **onKeyDown**(int, KeyEvent)             | 按键时被调用                                   |
|           | **onKeyUp**(int, KeyEvent)               | 按键被抬起时调用                                 |
|           | **onTrackballEvent**(MotionEvent)        | Called when a trackball motion event occurs. |
|           | **onTouchEvent**(MotionEvent)            | 触摸屏幕时调用                                  |
| 焦点        | **onFocusChanged**(boolean, int, android.graphics.Rect) | 获取到或者失去焦点是调用                             |
|           | **onWindowFocusChanged**(boolean)        | 窗口获取或者失去焦点是调用                            |
| Attaching | **onAttachedToWindow**()                 | 当视图被连接到一个窗口时调用                           |
|           | **onDetachedFromWindow**()               | 当视图从窗口分离时调用                              |
|           | **onWindowVisibilityChanged**(int)       | 当View的窗口的可见性发生改变时调用                      |

　　从上面官方文档介绍我们可以知道，View是所有控件（包括ViewGroup）的父类，它里面有一些常见的方法（上表），如果我们要自定义View，最简单的只需要重写onDraw(android.graphics.Canvas)即可。 


# 自定义TextView

## 1. 继承View，重写onDraw方法

　　创建一个类MyTextView继承View，发现报错，因为要覆盖他的构造方法（因为**View中没有参数为空的构造方法**）。
　　View有四种形式的构造方法，其中四个参数的构造方法是API 21才出现，所以一般我们只需要重写其他三个构造方法即可。

### View的构造方法

它们的参数不一样分别对应不同的创建方式。

1. 只有一个Context参数的构造方法：通常是通过代码初始化控件时使用；
2. 两个参数的构造方法：通常对应布局文件中控件被映射成对象时调用（需要解析属性）；
3. 通常我们让这两个构造方法最终调用三个参数的构造方法，然后在第三个构造方法中进行一些初始化操作。

```java
public class MyView extends View {
    // 需要绘制的文字
    private String mText;
    // 文本的颜色
    private int mTextColor;
    // 文本的大小
    private int mTextSize;
    // 绘制时控制文本绘制的范围
    private Rect mBound;
    private Paint mPaint;

    public MyTextView(Context context) {
        this(context, null);
    }
    public MyTextView(Context context, AttributeSet attrs) {
        this(context, attrs, 0);
    }
    public MyTextView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
 //初始化
        mText = "Udf32fA";
        mTextColor = Color.BLACK;
        mTextSize = 100;

        mPaint = new Paint();
        mPaint.setTextSize(mTextSize);
        mPaint.setColor(mTextColor);
        //获得绘制文本的宽和高
        mBound = new Rect();
        mPaint.getTextBounds(mText, 0, mText.length(), mBound);
    }
    //API21
//    public MyTextView(Context context, AttributeSet attrs, int defStyleAttr, int defStyleRes) {
//        super(context, attrs, defStyleAttr, defStyleRes);
//        init();
//    }

    @Override
    protected void onDraw(Canvas canvas) {
        //绘制文字
        canvas.drawText(mText, getWidth() / 2 - mBound.width() / 2, getHeight() / 2 + mBound.height() / 2, mPaint);
    }
}
```

布局文件：

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

        <view.openxu.com.mytextview.MyTextView
            android:layout_width="200dip"
            android:layout_height="100dip"
            android:textSize="20sp"
            android:text="按钮"
            android:background="#ff0000"/>

</LinearLayout>
```

运行结果： 
![](http://img.blog.csdn.net/20180109164844927?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

　　上面我只是重写了一个onDraw方法，文本已经绘制出来，说明到此为止这个自定义控件已经算成功了。可是发现了一个问题，如果我要绘制另外的文本呢？比如写i love you，那是不是又得重新定义一个自定义控件？跟上面一样，只是需要修改mText就可以了；行，再写一遍，那如果我现在又想改变文字颜色为蓝色呢？在写一遍？这时候就用到了新的知识点：自定义属性

## 2. 自定义属性

### 创建attrs.xml文件

　　在**res/values/**下创建一个名为**attrs.xml**的文件，然后定义如下属性： 
format的意思是该属性的取值是什么类型（支持的类型有string,color,demension,integer,enum,reference,float,boolean,fraction,flag）

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <attr name="mText" format="string" />
    <attr name="mTextColor" format="color" />
    <attr name="mTextSize" format="dimension" />
    <declare-styleable name="MyTextView">
        <attr name="mText"/>
        <attr name="mTextColor"/>
        <attr name="mTextSize"/>
    </declare-styleable>
</resources>
```

### 在构造方法中获取自定义属性的值：

```java
public MyTextView(Context context, AttributeSet attrs, int defStyleAttr) {
    super(context, attrs, defStyleAttr);
    //获取自定义属性的值
    TypedArray a = context.getTheme().obtainStyledAttributes(attrs, R.styleable.MyTextView, defStyleAttr, 0);
    mText = a.getString(R.styleable.MyTextView_mText);
    mTextColor = a.getColor(R.styleable.MyTextView_mTextColor, Color.BLACK);
    mTextSize = a.getDimension(R.styleable.MyTextView_mTextSize, 100);
    a.recycle();  //注意回收
    mPaint = new Paint();
    mPaint.setTextSize(mTextSize);
    mPaint.setColor(mTextColor);
    //获得绘制文本的宽和高
    mBound = new Rect();
    mPaint.getTextBounds(mText, 0, mText.length(), mBound);
}
```

然后在**布局文件中使用自定义属性**，记住一定要引入我们的命名空间`xmlns:openxu="http://schemas.android.com/apk/res-auto"`

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:openxu="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

        <view.openxu.com.mytextview.MyTextView
            android:layout_width="200dip"
            android:layout_height="100dip"
            openxu:mTextSize="25sp"
            openxu:mText="i love you"
            openxu:mTextColor ="#0000ff"
            android:background="#ff0000"/>

</LinearLayout>
```

运行结果： 
![1](http://img.blog.csdn.net/20180109165521122?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

　　通过运行结果，我们已经成功为MyTextView定义了属性，并获取到值，至于自定义属性的详细知识点到后面再介绍。 
到此为止，发现自定义控件还是比较简单的嘛。看看结果，跟原生的TextView还有什么差别？接下来做一点小变化： 
让绘制的文本长一点`openxu:mText="i love you i love you i love you"`，运行结果： 

![2](http://img.blog.csdn.net/20180109165513201?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
　　有没有发现不和谐的现象？文本超度超出了控件边界，控件太小，不足以显示辣么长的文本，我们将宽高改为`wrap_content`试试： 

![3](http://img.blog.csdn.net/20180109165503881?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
　　什么鬼？不是包裹内容吗？怎么填充整个屏幕了？根据顶部官方文档的说明，我们猜想肯定是控件的测量onMeasure方法出了问题，接下来我们学习onMeasure方法。

## 3. onMeasure方法

### **① MeasureSpec**

　　在学习onMasure方法之前，我们要先了解他的参数中的一个类MeasureSpec。 
跟踪一下源码，发现它是View中的一个静态内部类，是由**尺寸和模式组合**而成的一个值，用来描述**父控件对子控件尺寸的约束**，看看他的部分源码，一共有三种模式，然后提供了合成和分解的方法： 

```java
/**
 * measurespec封装了父控件对他的孩子的布局要求。
 * 一个measurespec由大小和模式。有三种可能的模式：
 */
public static class MeasureSpec {
    private static final int MODE_SHIFT = 30;
    private static final int MODE_MASK  = 0x3 << MODE_SHIFT;

    //父控件不强加任何约束给子控件，它可以是它想要任何大小。
    public static final int UNSPECIFIED = 0 << MODE_SHIFT;  //0

    //父控件决定给孩子一个精确的尺寸
    public static final int EXACTLY     = 1 << MODE_SHIFT;  //1073741824

    //父控件会给子控件尽可能大的尺寸
    public static final int AT_MOST     = 2 << MODE_SHIFT;   //-2147483648
    
    // 根据给定的尺寸和模式创建一个约束规范
    public static int makeMeasureSpec(int size, int mode) {
        if (sUseZeroUnspecifiedMeasureSpec && mode == UNSPECIFIED) {
            return 0;
        }
        return makeMeasureSpec(size, mode);
    }
    
    // 从约束规范中获取模式
    public static int getMode(int measureSpec) {
        return (measureSpec & MODE_MASK);
    }
    
    // 从约束规范中获取尺寸
    public static int getSize(int measureSpec) {
        return (measureSpec & ~MODE_MASK);
    }
}
```

　　这样说起来还是有点抽象，举一个小栗子大家就知道这三种约束到底是什么意思。我们自定义一个View，为了方便起见，让它继承Button，布局文件中设置不同的宽高条件，然后在onMeasure方法中打印一下他的参数（int widthMeasureSpec, int heightMeasureSpec）到底是个什么鬼

```java
/**
 * Created by openXu on 16/5/19.
 */
public class MyView extends Button {
    public MyView(Context context) {
        this(context, null);
    }
    public MyView(Context context, AttributeSet attrs) {
        this(context, attrs, 0);
    }
    public MyView(Context context, AttributeSet attrs, int defStyle) {
        super(context, attrs, defStyle);
    }
    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
        int widthMode = MeasureSpec.getMode(widthMeasureSpec);   //获取宽的模式
        int heightMode = MeasureSpec.getMode(heightMeasureSpec); //获取高的模式
        int widthSize = MeasureSpec.getSize(widthMeasureSpec);   //获取宽的尺寸
        int heightSize = MeasureSpec.getSize(heightMeasureSpec); //获取高的尺寸
        Log.v("openxu", "宽的模式:"+widthMode);
        Log.v("openxu", "高的模式:"+heightMode);
        Log.v("openxu", "宽的尺寸:"+widthSize);
        Log.v("openxu", "高的尺寸:"+heightSize);
    }
}
```

**情形1，让按钮包裹内容:**

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
        <com.example.openxu.myview.MyView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:textSize="20sp"
            android:text="按钮"
            android:background="#ff0000"/>
</LinearLayout>
```

log打印：

```
05-19 06:02:55.112: V/openxu(15599): 宽的模式:-2147483648 
05-19 06:02:55.112: V/openxu(15599): 高的模式:-2147483648 
05-19 06:02:55.112: V/openxu(15599): 宽的尺寸:1080 
05-19 06:02:55.112: V/openxu(15599): 高的尺寸:1860
```

**情形2，让按钮填充父窗体:**

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
        <com.example.openxu.myview.MyView
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:textSize="20sp"
            android:text="按钮"
            android:background="#ff0000"/>
</LinearLayout>
```

log打印：

```
05-19 06:05:37.302: V/openxu(15960): 宽的模式:1073741824 
05-19 06:05:37.302: V/openxu(15960): 高的模式:1073741824 
05-19 06:05:37.302: V/openxu(15960): 宽的尺寸:1080 
05-19 06:05:37.302: V/openxu(15960): 高的尺寸:1860
```

**情形3，给按钮的宽设置为具体的值:**

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
        <com.example.openxu.myview.MyView
            android:layout_width="100dip"
            android:layout_height="wrap_content"
            android:textSize="20sp"
            android:text="按钮"
            android:background="#ff0000"/>
</LinearLayout>
```

log打印： 

```
05-19 06:07:48.932: V/openxu(16105): 宽的模式:1073741824 
05-19 06:07:48.932: V/openxu(16105): 高的模式:-2147483648 
05-19 06:07:48.932: V/openxu(16105): 宽的尺寸:300 
05-19 06:07:48.932: V/openxu(16105): 高的尺寸:1860
```

　　根据上面的测试，我们发现，约束中分离出来的尺寸就是父控件剩余的宽高大小（除了设置具体的宽高值外）；而几种约束中的模式不就是对应我们在布局文件中设置给按钮的几种情况吗？如下：

| 约束                   |        布局参数        | 输出值         | 说明                                       |
| :------------------- | :----------------: | ----------- | :--------------------------------------- |
| **UNSPECIFIED**(未指定) |                    | 0           | 父控件没有对子控件施加任何约束，子控件可以得到任意想要的大小。但是布局文件好像必须设置宽高，目前还没找到与之对应的布局参数，使用较少 |
| **EXACTLY**(指定)      | match_parent/具体宽高值 | 1073741824  | 父控件给子控件决定了确切大小，子控件将被限定在给定的边界里而忽略它本身大小。特别说明如果是填充父窗体，说明父控件已经明确知道子控件想要多大的尺寸了（就是剩余的空间都要了） |
| **AT_MOST**(至多)      |    wrap_content    | -2147483648 | 子控件至多达到指定大小的值。包裹内容就是父窗体并不知道子控件到底需要多大尺寸（具体值），需要子控件自己测量之后再让父控件给他一个尽可能大的尺寸以便让内容全部显示但不能超过包裹内容的大小 |

### **② 分析为什么我们自定义的MyTextView设置了wrap_content却填充屏幕**

　　通过上面对MeasureSpec的了解，我们现在就有能看懂View的onMeasure方法默认是怎样为控件测量大小的了 
看View中onMeasure的源码：

```java
protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
    setMeasuredDimension(getDefaultSize(getSuggestedMinimumWidth(), widthMeasureSpec),
            getDefaultSize(getSuggestedMinimumHeight(), heightMeasureSpec));
}

public static int getDefaultSize(int size, int measureSpec) {
    int result = size;
    int specMode = MeasureSpec.getMode(measureSpec);
    int specSize = MeasureSpec.getSize(measureSpec);

    switch (specMode) {
    case MeasureSpec.UNSPECIFIED:
        result = size;
        break;
    case MeasureSpec.AT_MOST:
    case MeasureSpec.EXACTLY:
        result = specSize;
        break;
    }
    return result;
}
```

　　 onMeasure方法调用了setMeasuredDimension(int measuredWidth, int measuredHeight)方法，而传入的参数已经是测量过的默认宽和高的值了；我们看看getDefaultSize 方法是怎么计算测量宽高的。根据父控件给予的约束，发现AT_MOST （相当于wrap_content ）和EXACTLY （相当于match_parent ）两种情况返回的测量宽高都是specSize，而这个specSize正是我们上面说的父控件剩余的宽高，所以默认onMeasure方法中wrap_content 和match_parent 的效果是一样的，都是填充剩余的空间。

### **③ 重写onMeasure方法**

　　我们先忽略掉UNSPECIFIED 的情况（使用极少），只考虑AT_MOST 和EXACTLY ，现在的问题是设置wrap_content 时，控件却使用了match_parent 的效果，看下面怎么重写onMeasure： 

```java
@Override
protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
    super.onMeasure(widthMeasureSpec, heightMeasureSpec);
    int widthMode = MeasureSpec.getMode(widthMeasureSpec);   //获取宽的模式
    int heightMode = MeasureSpec.getMode(heightMeasureSpec); //获取高的模式
    int widthSize = MeasureSpec.getSize(widthMeasureSpec);   //获取宽的尺寸
    int heightSize = MeasureSpec.getSize(heightMeasureSpec); //获取高的尺寸
    Log.v("openxu", "宽的模式:"+widthMode);
    Log.v("openxu", "高的模式:"+heightMode);
    Log.v("openxu", "宽的尺寸:"+widthSize);
    Log.v("openxu", "高的尺寸:"+heightSize);
    int width;
    int height ;
    if (widthMode == MeasureSpec.EXACTLY) {
        //如果match_parent或者具体的值，直接赋值
        width = widthSize;
    } else {
        //如果是wrap_content，我们要得到控件需要多大的尺寸
        float textWidth = mBound.width();   //文本的宽度
        //控件的宽度就是文本的宽度加上两边的内边距。内边距就是padding值，在构造方法执行完就被赋值
        width = (int) (getPaddingLeft() + textWidth + getPaddingRight());
        Log.v("openxu", "文本的宽度:"+textWidth + "控件的宽度："+width);
    }
    //高度跟宽度处理方式一样
    if (heightMode == MeasureSpec.EXACTLY) {
        height = heightSize;
    } else {
        float textHeight = mBound.height();
        height = (int) (getPaddingTop() + textHeight + getPaddingBottom());
        Log.v("openxu", "文本的高度:"+textHeight + "控件的高度："+height);
    }
    //保存测量宽度和测量高度
    setMeasuredDimension(width, height);
}
```

布局文件：

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:openxu="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
        <view.openxu.com.mytextview.MyTextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:padding="20dip"
            openxu:mTextSize="25sp"
            openxu:mText="i love you i love you i love you"
            openxu:mTextColor ="#0000ff"
            android:background="#ff0000"/>
</LinearLayout>
```

下面是输出的log：

```
05-19 01:29:12.662 27380-27380/view.openxu.com.mytextview V/openxu: 宽的模式:-2147483648 
05-19 01:29:12.662 27380-27380/view.openxu.com.mytextview V/openxu: 高的模式:-2147483648 
05-19 01:29:12.662 27380-27380/view.openxu.com.mytextview V/openxu: 宽的尺寸:720 
05-19 01:29:12.666 27380-27380/view.openxu.com.mytextview V/openxu: 高的尺寸:1230 
05-19 01:29:12.678 27380-27380/view.openxu.com.mytextview V/openxu: 文本的宽度:652.0控件的宽度：732 
05-19 01:29:12.690 27380-27380/view.openxu.com.mytextview V/openxu: 文本的高度:49.0控件的高度：129
```

　　我的模拟器是720x1280的，根据log显示，文本的宽度是652，加上两边的内边距，控件的宽度为732，确实实现了包裹内容的效果，运行程序结果如下： 
![4](http://img.blog.csdn.net/20180109165453766?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

　　但是发现宽度已经超出了屏幕，还不能像TextView一样换行；下面我们简单的模拟一下换行的功能，做的不够好，但有这个效果，不是重点，不需要重点掌握

## 4. 自动换行

　　只需要在测量的时候，根据文字的总长度和控件的宽度，就可以知道需要绘制几行，然后将文本分割成小段放入集合中，在onDraw方法中分别绘制； 
　　需要注意的是，onMeasure方法不只调用一次，所以在分段文本是需要判断，不要重复分段，否则会报错。代码如下（仅供参考）：

```java
public class MyTextView extends View {

    // 需要绘制的文字/
    private String mText;
    private ArrayList<String> mTextList;
    // 文本的颜色/
    private int mTextColor;
    // 文本的大小/
    private float mTextSize;

    // 绘制时控制文本绘制的范围
    private Rect mBound;
    private Paint mPaint;

    public MyTextView(Context context) {
        this(context, null);
    }
    public MyTextView(Context context, AttributeSet attrs) {
        this(context, attrs, 0);
    }
    public MyTextView(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        mTextList = new ArrayList<String>();
        //获取自定义属性的值
        TypedArray a = context.getTheme().obtainStyledAttributes(attrs, R.styleable.MyTextView, defStyleAttr, 0);
        mText = a.getString(R.styleable.MyTextView_mText);
        mTextColor = a.getColor(R.styleable.MyTextView_mTextColor, Color.BLACK);
        mTextSize = a.getDimension(R.styleable.MyTextView_mTextSize, 100);
        a.recycle();  //注意回收
        Log.v("openxu", "文本总长度:"+mText);
        mPaint = new Paint();
        mPaint.setTextSize(mTextSize);
        mPaint.setColor(mTextColor);
        //获得绘制文本的宽和高
        mBound = new Rect();
        mPaint.getTextBounds(mText, 0, mText.length(), mBound);
    }
    //API21
//    public MyTextView(Context context, AttributeSet attrs, int defStyleAttr, int defStyleRes) {
//        super(context, attrs, defStyleAttr, defStyleRes);
//        init();
//    }

    @Override
    protected void onDraw(Canvas canvas) {
        //绘制文字
        for(int i = 0; i<mTextList.size(); i++){
            mPaint.getTextBounds(mTextList.get(i), 0, mTextList.get(i).length(), mBound);
            
            Log.v("openxu", "mBound.h:"+mBound.height());
            Log.v("openxu", "在X:" + (getWidth() / 2 - mBound.width() / 2)+"  Y:"+(getPaddingTop() + (mBound.height() *i))+"  绘制："+mTextList.get(i));
            
            canvas.drawText(mTextList.get(i), (getWidth() / 2 - mBound.width() / 2), (getPaddingTop() + (mBound.height() *i)), mPaint);
        }
    }

    boolean isOneLines = true;
    float lineNum;
    float spLineNum;
    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
        int widthMode = MeasureSpec.getMode(widthMeasureSpec);   //获取宽的模式
        int heightMode = MeasureSpec.getMode(heightMeasureSpec); //获取高的模式
        int widthSize = MeasureSpec.getSize(widthMeasureSpec);   //获取宽的尺寸
        int heightSize = MeasureSpec.getSize(heightMeasureSpec); //获取高的尺寸
        Log.v("openxu", "宽的模式:"+widthMode);
        Log.v("openxu", "高的模式:"+heightMode);
        Log.v("openxu", "宽的尺寸:"+widthSize);
        Log.v("openxu", "高的尺寸:"+heightSize);

        float textWidth = mBound.width();   //文本的宽度
        if(mTextList.size()==0){
            //将文本分段
            int padding = getPaddingLeft() + getPaddingRight();
            int specWidth = widthSize - padding; //能够显示文本的最大宽度
            if(textWidth<specWidth){
                //说明一行足矣显示
                lineNum = 1;
                mTextList.add(mText);
            }else{
                //超过一行
                isOneLines = false;
                spLineNum = textWidth/specWidth;
                if((spLineNum+"").contains(".")){
                    lineNum = Integer.parseInt((spLineNum+"").substring(0,(spLineNum+"").indexOf(".") ))+1;
                }else{
                    lineNum = spLineNum;
                }
                int lineLength = (int)(mText.length()/spLineNum);
                Log.v("openxu", "文本总长度:"+mText);
                Log.v("openxu", "文本总长度:"+mText.length());
                Log.v("openxu", "能绘制文本的宽度:"+lineLength);
                Log.v("openxu", "需要绘制:"+lineNum+"行");
                Log.v("openxu", "lineLength:"+lineLength);
                for(int i = 0; i<lineNum; i++){
                    String lineStr;
                    if(mText.length()<lineLength){
                        lineStr = mText.substring(0, mText.length());
                    }else{
                        lineStr = mText.substring(0, lineLength);
                    }
                    Log.v("openxu", "lineStr:"+lineStr);
                    mTextList.add(lineStr);
                    if(!TextUtils.isEmpty(mText)) {
                        if(mText.length()<lineLength){
                            mText = mText.substring(0, mText.length());
                        }else{
                            mText = mText.substring(lineLength, mText.length());
                        }
                    }else{
                        break;
                    }
                }
            }
        }
        int width;
        int height ;
        if (widthMode == MeasureSpec.EXACTLY) {
            //如果match_parent或者具体的值，直接赋值
            width = widthSize;
        } else {
            //如果是wrap_content，我们要得到控件需要多大的尺寸
            if(isOneLines){
                //控件的宽度就是文本的宽度加上两边的内边距。内边距就是padding值，在构造方法执行完就被赋值
                width = (int) (getPaddingLeft() + textWidth + getPaddingRight());
            }else{
                //如果是多行，说明控件宽度应该填充父窗体
                width = widthSize;
            }
            Log.v("openxu", "文本的宽度:"+textWidth + "控件的宽度："+width);
        }
        //高度跟宽度处理方式一样
        if (heightMode == MeasureSpec.EXACTLY) {
            height = heightSize;
        } else {
            float textHeight = mBound.height();
            if(isOneLines){
                height = (int) (getPaddingTop() + textHeight + getPaddingBottom());
            }else{
                //如果是多行
                height = (int) (getPaddingTop() + textHeight*lineNum + getPaddingBottom());;
            }

            Log.v("openxu", "文本的高度:"+textHeight + "控件的高度："+height);
        }
        //保存测量宽度和测量高度
        setMeasuredDimension(width, height);
    }
}
```

布局文件：

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:openxu="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
        <view.openxu.com.mytextview.MyTextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:padding="20dip"
            openxu:mTextSize="25sp"
            openxu:mText="门前大桥下，游过一群鸭，快来快来数一数，二四六七八。帽儿破，鞋儿破，身上滴袈裟破，你笑我"
            openxu:mTextColor ="#0000ff"
            android:background="#ff0000"/>
</LinearLayout>
```

运行效果： 
![5](http://img.blog.csdn.net/20180109165445957?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

## 源码下载：

[https://github.com/openXu/MyTextView](https://github.com/openXu/MyTextView "下载")

# 自定义控件的基本步骤：

1. 继承View，重写构造方法 
2. 自定义属性，在构造方法中初始化属性 
3. 重写onMeasure方法测量宽高 
4. 重写onDraw方法绘制控件

# 深入解析自定义属性

## 1. 为什么要自定义属性

  要使用属性，首先这个属性应该存在，所以如果我们要使用自己的属性，必须要先把他定义出来才能使用。但我们平时在写布局文件的时候好像没有自己定义属性，但我们照样可以用很多属性，这是为什么？我想大家应该都知道：系统定义好的属性我们就可以拿来用呗，但是你们知道系统定义了哪些属性吗？哪些属性是我们自定义控件可以直接使用的，哪些不能使用？什么样的属性我们能使用？这些问题我想大家不一定都弄得清除，下面我们去一一解开这些谜团。 
  **系统定义的**所有**属性**我们可以在**\sdk\platforms\android-xx\data\res\values**目录下找到**attrs.xml**这个文件，这就是系统自带的所有属性，打开看看一些比较熟悉的：

```xml
<declare-styleable name="View">
    <attr name="id" format="reference" />
    <attr name="background" format="reference|color" />
    <attr name="padding" format="dimension" />
     ...
    <attr name="focusable" format="boolean" />
     ...
</declare-styleable>

<declare-styleable name="TextView">
    <attr name="text" format="string" localization="suggested" />
    <attr name="hint" format="string" />
    <attr name="textColor" />
    <attr name="textColorHighlight" />
    <attr name="textColorHint" />
     ...
</declare-styleable>

<declare-styleable name="ViewGroup_Layout">
    <attr name="layout_width" format="dimension">
        <enum name="fill_parent" value="-1" />
        <enum name="match_parent" value="-1" />
        <enum name="wrap_content" value="-2" />
    </attr>
    <attr name="layout_height" format="dimension">
        <enum name="fill_parent" value="-1" />
        <enum name="match_parent" value="-1" />
        <enum name="wrap_content" value="-2" />
    </attr>
</declare-styleable>

<declare-styleable name="LinearLayout_Layout">
    <attr name="layout_width" />
    <attr name="layout_height" />
    <attr name="layout_weight" format="float" />
    <attr name="layout_gravity" />
</declare-styleable>

<declare-styleable name="RelativeLayout_Layout">
    <attr name="layout_centerInParent" format="boolean" />
    <attr name="layout_centerHorizontal" format="boolean" />
    <attr name="layout_centerVertical" format="boolean" />
     ...
</declare-styleable>
```

  看看上面attrs.xml文件中的属性，发现他们都是有规律的分组的形式组织的。以declare-styleable 为一个组合，后面有一个name属性，属性的值为View 、TextView 等等，有没有想到什么？没错，属性值为View的那一组就是为View定义的属性，属性值为TextView的就是为TextView定义的属性…。

  因为所有的控件都是View的子类，所以为View定义的属性所有的控件都能使用，这就是为什么我们的自定义控件没有定义属性就能使用一些系统属性。

  但是并不是每个控件都能使用所有属性，比如TextView是View的子类，所以为View定义的所有属性它都能使用，但是子类肯定有自己特有的属性，得单独为它扩展一些属性，而单独扩展的这些属性只有它自己能有，View是不能使用的，比如View中不能使用android:text=“”。又比如，LinearLayout中能使用layout_weight属性，而RelativeLayout却不能使用，因为layout_weight是为LinearLayout的LayoutParams定义的。

  综上所述，自定义控件如果不自定义属性，就只能使用VIew的属性，但为了给我们的控件扩展一些属性，我们就必须自己去定义。

## 2. 怎样自定义属性

这两种的区别就是attr标签后面带不带**format**属性，如果带format的就是在定义属性，如果不带format的就是在使用已有的属性，name的值就是属性的名字，format是限定当前定义的属性能接受什么值。

  打个比方，比如系统已经定义了android:text属性，我们的自定义控件也需要一个文本的属性，可以有两种方式：

**第一种**：我们并不知道系统定义了此名称的属性，我们自己定义一个名为text或者mText的属性（属性名称可以随便起的）

```xml
<resources>
    <declare-styleable name="MyTextView">
        <attr name=“text" format="string" />
    </declare-styleable>
</resources>
```

**第二种**：我们知道系统已经定义过名称为text的属性，我们不用自己定义，只需要在自定义属性中申明，我要使用这个text属性 
（注意加上android命名空间，这样才知道使用的是系统的text属性）

```xml
<resources>
    <declare-styleable name="MyTextView">
        <attr name=“android:text"/>
    </declare-styleable>
</resources>
```

  为什么系统定义了此属性，我们在使用的时候还要声明？因为，系统定义的text属性是给TextView使用的，如果我们不申明，就不能使用text属性。

## 3. 属性值的类型format

**format支持的类型一共有11种：** 

### **(1) reference：参考某一资源ID**

- 属性定义：

```xml
<declare-styleable name = "名称">
     <attr name = "background" format = "reference" />
</declare-styleable>
```

- 属性使用：

```xml
<ImageView android:background = "@drawable/图片ID"/>
```

### **(2) color：颜色值**

- 属性定义：

```xml
<attr name = "textColor" format = "color" />
```

- 属性使用：

```xml
<TextView android:textColor = "#00FF00" />
```

### **(3) boolean：布尔值**

- 属性定义：

```xml
<attr name = "focusable" format = "boolean" />
```

- 属性使用：

```xml
<Button android:focusable = "true"/>
```

### **(4) dimension：尺寸值**

- 属性定义：

```xml
<attr name = "layout_width" format = "dimension" />
```

- 属性使用：

```xml
<Button android:layout_width = "42dip"/>
```

### **(5) float：浮点值**

- 属性定义：

```xml
<attr name = "fromAlpha" format = "float" />
```

- 属性使用：

```xml
<alpha android:fromAlpha = "1.0"/>
```

### **(6) integer：整型值**

- 属性定义：

```xml
<attr name = "framesCount" format="integer" />
```

- 属性使用：

```xml
<animated-rotate android:framesCount = "12"/>
```

### **(7) string：字符串**

- 属性定义：

```xml
<attr name = "text" format = "string" />
```

- 属性使用：

```xml
<TextView android:text = "我是文本"/>
```

### **(8) fraction：百分数**

- 属性定义：

```xml
<attr name = "pivotX" format = "fraction" />

```

- 属性使用：

```xml
<rotate android:pivotX = "200%"/>

```

### **(9) enum：枚举值**

- 属性定义：

```xml
<declare-styleable name="名称">
    <attr name="orientation">
        <enum name="horizontal" value="0" />
        <enum name="vertical" value="1" />
    </attr>
</declare-styleable>

```

- 属性使用：

```xml
<LinearLayout  
    android:orientation = "vertical">
</LinearLayout>

```

> 注意：枚举类型的属性在使用的过程中**只能同时使用其中一个**，不能 `android:orientation = “horizontal｜vertical"`

### **(10) flag：位或运算**

- 属性定义：

```xml
<declare-styleable name="名称">
    <attr name="gravity">
            <flag name="top" value="0x30" />
            <flag name="bottom" value="0x50" />
            <flag name="left" value="0x03" />
            <flag name="right" value="0x05" />
            <flag name="center_vertical" value="0x10" />
            ...
    </attr>
</declare-styleable>

```

- 属性使用：

```xml
<TextView android:gravity="bottom|left"/>

```

> 注意：位运算类型的属性在使用的过程中**可以使用多个值**

### **(11) 混合类型：属性定义时可以指定多种类型值**

- 属性定义：

```xml
<declare-styleable name = "名称">
     <attr name = "background" format = "reference|color" />
</declare-styleable>

```

- 属性使用：

```xml
<ImageView
android:background = "@drawable/图片ID" />
或者：
<ImageView
android:background = "#00FF00" />

```

  通过上面的学习我们已经知道怎么定义各种类型的属性，以及怎么使用它们，但是我们写好布局文件之后，要在控件中使用这些属性还需要将它解析出来。

## 4. 类中获取属性值

  在这之前，顺带讲一下**命名空间**，我们在布局文件中使用属性的时候（`android:layout_width="match_parent"`）发现前面都带有一个android：，这个android就是上面引入的命名空间`xmlns:android="http://schemas.android.com/apk/res/android”`，表示到android系统中查找该属性来源。只有引入了命名空间，XML文件才知道下面使用的属性应该去哪里找（哪里定义的，不能凭空出现，要有根据）。

  如果我们自定义属性，这个属性应该去我们的应用程序包中找，所以要引入我们应用包的命名空间`xmlns:openxu="http://schemas.android.com/apk/res-auto”`，res-auto表示自动查找，还有一种写法`xmlns:openxu="http://schemas.android.com/apk/com.example.openxu.myview"`，`com.example.openxu.myview`为我们的应用程序包名。

  按照上面学习的知识，我们先定义一些属性，并写好布局文件。 
先在res\values目录下创建attrs.xml，定义自己的属性：

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <declare-styleable name="MyTextView">
        <!--声明MyTextView需要使用系统定义过的text属性,注意前面需要加上android命名-->
        <attr name="android:text" />
        <attr name="mTextColor" format="color" />
        <attr name="mTextSize" format="dimension" />
    </declare-styleable>
</resources>
```

在布局文件中，使用属性（注意引入我们应用程序的命名空间，这样在能找到我们包中的attrs）：

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    xmlns:openxu="http://schemas.android.com/apk/res-auto"
    android:orientation="horizontal"
    android:layout_width="match_parent"
    android:layout_height="match_parent">
        <com.example.openxu.myview.MyTextView
            android:layout_width="200dip"
            android:layout_height="100dip"
            openxu:mTextSize="25sp"
            android:text="我是文字"
            openxu:mTextColor ="#0000ff"
            android:background="#ff0000"/>
</LinearLayout>

```

在构造方法中获取属性值：

```java
public MyTextView(Context context, AttributeSet attrs, int defStyleAttr) {
    super(context, attrs, defStyleAttr);
    TypedArray ta = context.obtainStyledAttributes(attrs, R.styleable.MyTextView);
    String text = ta.getString(R.styleable.MyTextView_android_text);
    int mTextColor = ta.getColor(R.styleable.MyTextView_mTextColor, Color.BLACK);
    int mTextSize = ta.getDimensionPixelSize(R.styleable.MyTextView_mTextSize, 100);
    ta.recycle();  //注意回收
    Log.v("openxu", “text属性值:"+mText);
    Log.v("openxu", "mTextColor属性值:"+mTextColor);
    Log.v("openxu", "mTextSize属性值:"+mTextSize);
}
```

log输出：

```
05-21 00:14:07.192: V/openxu(25652): mText属性值:我是文字
05-21 00:14:07.192: V/openxu(25652): mTextColor属性值:-16776961
05-21 00:14:07.192: V/openxu(25652): mTextSize属性值:75
```

到此为止，是不是发现自定义属性是如此简单？

属性的定义我们应该学的差不多了，但有没有发现构造方法中获取属性值的时候有两个比较陌生的类`AttributeSet`和`TypedArray`，这两个类是怎么把属性值从布局文件中解析出来的？

## 5. Attributeset和TypedArray以及declare-styleable

  Attributeset看名字就知道是一个属性的集合，实际上，它内部就是一个XML解析器，帮我们将布局文件中该控件的所有属性解析出来，并以key-value的键值对形式维护起来。其实我们完全可以只用他通过下面的代码来获取我们的属性就行。

```java
public MyTextView(Context context, AttributeSet attrs, int defStyleAttr) {
    super(context, attrs, defStyleAttr);
    int count = attrs.getAttributeCount();
    for (int i = 0; i < count; i++) {
        String attrName = attrs.getAttributeName(i);
        String attrVal = attrs.getAttributeValue(i);
        Log.e("openxu", "attrName = " + attrName + " , attrVal = " + attrVal);
    }
}
```

log输出：

```
05-21 02:18:09.052: E/openxu(14704): attrName = background , attrVal = @2131427347
05-21 02:18:09.052: E/openxu(14704): attrName = layout_width , attrVal = 200.0dip
05-21 02:18:09.052: E/openxu(14704): attrName = layout_height , attrVal = 100.0dip
05-21 02:18:09.052: E/openxu(14704): attrName = text , attrVal = 我是文字
05-21 02:18:09.052: E/openxu(14704): attrName = mTextSize , attrVal = 25sp
05-21 02:18:09.052: E/openxu(14704): attrName = mTextColor , attrVal = #0000ff


```

  发现通过Attributeset获取属性的值时，它将我们布局文件中的值原原本本的获取出来的，比如宽度200.0dip，其实这并不是我们想要的，如果我们接下来要使用宽度值，我们还需要将dip去掉，然后转换成整形，这多麻烦。其实这都不算什么，更恶心的是，backgroud我应用了一个color资源ID，它直接给我拿到了这个ID值，前面还加了个@，接下来我要自己获取资源，并通过这个ID值获取到真正的颜色。 

我们再换**TypedArray**试试。 
  在这里，穿插一个知识点，定义属性的时候有一个**declare-styleable**，他是用来干嘛的，如果不要它可不可以？答案是可以的，我们自定义属性完全可以写成下面的形式：

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
        <attr name="mTextColor" format="color" />
        <attr name="mTextSize" format="dimension" />
</resources>
```

之前的形式是这样的：

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <declare-styleable name="MyTextView">
        <attr name="android:text" />
        <attr name="android:layout_width" />
        <attr name="android:layout_height" />
        <attr name="android:background" />
        <attr name="mTextColor" format="color" />
        <attr name="mTextSize" format="dimension" />
    </declare-styleable>
</resources>

或者：
<?xml version="1.0" encoding="utf-8"?>
<resources>
          <!--定义属性-->
       <attr name="mTextColor" format="color" />
        <attr name="mTextSize" format="dimension" />
    <declare-styleable name="MyTextView">
        <!--生成索引-->
        <attr name="android:text" />
        <attr name="android:layout_width" />
        <attr name="android:layout_height" />
        <attr name="android:background" />
        <attr name=“mTextColor" />
        <attr name="mTextSize"  />
    </declare-styleable>
</resources>
```

  我们都知道所有的资源文件在R中都会对应一个整型常量，我们可以通过这个ID值找到资源文件。 
  属性在R中对应的类是`public static final class attr`，如果我们写了**declare-styleable**，在R文件中就会生成**styleable**类，这个类其实就是将每个控件的属性分组，然后记录属性的索引值，而TypedArray正好需要通过此索引值获取属性。

```java
public static final class styleable

          public static final int[] MyTextView = {
            0x0101014f, 0x7f010038, 0x7f010039
        };
        public static final int MyTextView_android_text = 0;
        public static final int MyTextView_mTextColor = 1;
        public static final int MyTextView_mTextSize = 2;
｝
```

使用TypedArray获取属性值：

```java
public MyTextView(Context context, AttributeSet attrs, int defStyleAttr) {
    super(context, attrs, defStyleAttr);
    TypedArray ta = context.obtainStyledAttributes(attrs, R.styleable.MyTextView);
    String mText = ta.getString(R.styleable.MyTextView_android_text);
    int mTextColor = ta.getColor(R.styleable.MyTextView_mTextColor, Color.BLACK);
    int mTextSize = ta.getDimensionPixelSize(R.styleable.MyTextView_mTextSize, 100);
    float width = ta.getDimension(R.styleable.MyTextView_android_layout_width, 0.0f);
    float hight = ta.getDimension(R.styleable.MyTextView_android_layout_height,0.0f);
    int backgroud = ta.getColor(R.styleable.MyTextView_android_background, Color.BLACK);
    ta.recycle();  //注意回收
    Log.v("openxu", "width:"+width);
    Log.v("openxu", "hight:"+hight);
    Log.v("openxu", "backgroud:"+backgroud);
    Log.v("openxu", "mText:"+mText);
    Log.v("openxu", "mTextColor:"+mTextColor);
    Log.v("openxu", "mTextSize:"+mTextSize);ext, 0, mText.length(), mBound);
}
```

log输出：

```
05-21 02:56:49.162: V/openxu(22630): width:600.0
05-21 02:56:49.162: V/openxu(22630): hight:300.0
05-21 02:56:49.162: V/openxu(22630): backgroud:-12627531
05-21 02:56:49.162: V/openxu(22630): mText:我是文字
05-21 02:56:49.162: V/openxu(22630): mTextColor:-16777216
05-21 02:56:49.162: V/openxu(22630): mTextSize:100
```

  看看多么舒服的结果，我们得到了想要的宽高（float型），背景颜色（color的十进制）等，TypedArray提供了一系列获取不同类型属性的方法，这样就可以直接得到我们想要的数据类型，而不用像Attributeset获取属性后还要一个个处理才能得到具体的数据，实际上TypedArray是为我们获取属性值提供了方便，注意一点，TypedArray使用完毕后记得调用 `ta.recycle();`回收 。

# 深入解析控件测量onMeasure

## 1. onMeasure什么时候会被调用

  onMeasure方法的作用时测量控件的大小，什么时候需要测量控件的大小呢？我们举个栗子，做饭的时候我们炒一碗菜，炒菜的过程我们并不要求知道这道菜有多少分量，只有在菜做熟了我们要拿个碗盛放的时候，我们才需要掂量拿多大的碗盛放，这时候我们就要对菜的分量进行估测。 
  而我们的控件也正是如此，**创建一个View(执行构造方法)的时候不需要测量控件的大小**，只有将这个view**放入**一个容器（**父控件**）中的**时**候**才需要测量**，而这个**测量方法**就是**父控件唤起调用**的。当控件的父控件要放置该控件的时候，**父控件**会**调用子控件的onMeasure**方法询问子控件：“你有多大的尺寸，我要给你多大的地方才能容纳你？”，然后传入两个参数（widthMeasureSpec和heightMeasureSpec），这两个参数就是父控件告诉子控件可获得的空间以及关于这个空间的约束条件（好比我在思考需要多大的碗盛菜的时候我要看一下碗柜里最大的碗有多大，菜的分量不能超过这个容积，这就是碗对菜的约束），子控件拿着这些条件就能正确的测量自身的宽高了。 

## 2. onMeasure方法执行流程

  上面说到onMeasure方法是由父控件调用的，所有父控件都是ViewGroup的子类，**ViewGroup**是一个抽象类，它里面有一个抽象方法**onLayout**，这个方法的作用就是**摆放**它所有的**子控件（安排位置）**，因为是抽象类，不能直接new对象，所以我们在布局文件中可以使用View但是不能直接使用 ViewGroup。

  在给子控件确定位置之前，必须要获取到子控件的大小（只有确定了子控件的大小才能正确的确定上下左右四个点的坐标），而ViewGroup并没有重写View的onMeasure方法，也就是说抽象类ViewGroup没有为子控件测量大小的能力，它只能测量自己的大小。但是既然ViewGroup是一个能容纳子控件的容器，系统当然也考虑到测量子控件的问题，所以**ViewGroup**提供了三个**测量子控件**相关的**方法**(**measuireChildren**\\**measuireChild**\\**measureChildWithMargins**)，只是在ViewGroup中没有调用它们，所以它本身不具备为子控件测量大小的能力，但是他有这个潜力哦。

  为什么都有测量子控件的方法了而ViewGroup中不直接重写onMeasure方法，然后在onMeasure中调用呢？因为不同的容器摆放子控件的方式不同，比如RelativeLayout，LinearLayout这两个ViewGroup的子类，它们摆放子控件的方式不同，有的是线性摆放，而有的是叠加摆放，这就导致测量子控件的方式会有所差别，所以ViewGroup就干脆不直接测量子控件，他的子类要测量子控件就根据自己的布局特性重写onMeasure方法去测量。这么看来ViewGroup提供的三个测量子控件的方法岂不是没有作用？答案是NO，既然提供了就肯定有作用，这三个方法只是按照一种通用的方式去测量子控件，很多ViewGruop的子类测量子控件的时候就使用了ViewGroup的measureChildxxx系列方法；还有一个作用就是为我们自定义ViewGroup提供方便咯，自定义ViewGroup我会在后续专门探讨。

  **测量**的**时**候**父控件的onMeasure**方法会**遍历**他所有的**子控件**，挨个**调用子控件的measure方法**，**measure方法会调用onMeasure**，然后会**调用setMeasureDimension方法保存测量的大小**，一次遍历下来，第一个子控件以及这个子控件中的所有子控件都会完成测量工作；然后开始测量第二个子控件…；最后**父控件**所有的子控件都**完成测量**以后会**调用setMeasureDimension方法保存自己的测量大小**。值得注意的是，这个过程**不只执行一次**，也就是说有可能重复执行，因为有的时候，一轮测量下来，父控件发现某一个子控件的尺寸不符合要求，就会重新测量一遍。

举个栗子，看下图： 
![onMeasure方法执行流程](http://img.blog.csdn.net/20180109205020644?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

下面是测量的时序图： 
![测量的时序图](http://img.blog.csdn.net/20180109205011689?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

## 3. MeasureSpec类

  上面说到MeasureSpec约束是由父控件传递给子控件的，这个类里面到底封装了什么东西？我们看一看源码：

```java
public static class MeasureSpec {
    private static final int MODE_SHIFT = 30;
    private static final int MODE_MASK = 0x3 << MODE_SHIFT;
    // 父控件不强加任何约束给子控件，它可以是它想要任何大小
    public static final int UNSPECIFIED = 0 << MODE_SHIFT;
    // 父控件已为子控件确定了一个确切的大小，孩子将被给予这些界限，不管子控件自己希望的是多大
    public static final int EXACTLY = 1 << MODE_SHIFT;
    // 父控件会给子控件尽可能大的尺寸
    public static final int AT_MOST = 2 << MODE_SHIFT;

    // 根据所提供的大小和模式创建一个测量规范
    public static int makeMeasureSpec(int size, int mode) {
        if (sUseBrokenMakeMeasureSpec) {
            return size + mode;
        } else {
            return (size & ~MODE_MASK) | (mode & MODE_MASK);
        }
    }
    // 从所提供的测量规范中提取模式
    public static int getMode(int measureSpec) {
        return (measureSpec & MODE_MASK);
    }
    // 从所提供的测量规范中提取尺寸
    public static int getSize(int measureSpec) {
        return (measureSpec & ~MODE_MASK);
    }
    ...
}
```

  从源码中我们知道，**MeasureSpec**其实就是尺寸和模式通过各种位运算计算出的一个整型值，它提供了三种模式，还有三个方法（合成约束、分离模式、分离尺寸）。这三种模式对应的意思和布局文件中的参数值的对应关系（父控件填充屏幕*MATCH_PARENT*的情况，如果其他情况，请参考下面getChildMeasureSpec方法的结果）：

| **约束**           |      **布局参数**      |    **值**    |                  **说明**                  |
| ---------------- | :----------------: | :---------: | :--------------------------------------: |
| UNSPECIFIED(未指定) |                    |      0      |  父控件没有对子控件施加任何约束，子控件可以得到任意想要的大小（使用较少）。   |
| EXACTLY(完全)      | match_parent/具体宽高值 | 1073741824  | 父控件给子控件决定了确切大小，子控件将被限定在给定的边界里而忽略它本身大小。特别说明如果是填充父窗体，说明父控件已经明确知道子控件想要多大的尺寸了（就是剩余的空间都要了）栗子：碗柜最大的碗就这么大，菜有多少都只能盛到这个最大的碗里，盛不下的我就管不了了（吃掉或者倒掉） |
| AT_MOST(至多)      |    wrap-content    | -2147483648 | 子控件至多达到指定大小的值。包裹内容就是父窗体并不知道子控件到底需要多大尺寸（具体值），需要子控件自己测量之后再让父控件给他一个尽可能大的尺寸以便让内容全部显示但不能超过包裹内容的大小栗子：碗柜有各种大小的碗，菜少就拿小碗放，菜多就拿大碗放，但是不能浪费碗的容积，要保证碗刚好盛满菜 |

## 4. 从ViewGroup的onMeasure到View的onMeasure

### ① ViewGroup中三个测量子控件的方法：

  通过上面的介绍，我们知道，如果要自定义ViewGroup就必须重写onMeasure方法，在这里测量子控件的尺寸。子控件的尺寸怎么测量呢？ViewGroup中提供了三个关于测量子控件的方法：

```
 // 遍历ViewGroup中所有的子控件，调用measuireChild测量宽高
 protected void measureChildren (int widthMeasureSpec, int heightMeasureSpec) {
    final int size = mChildrenCount;
    final View[] children = mChildren;
    for (int i = 0; i < size; ++i) {
        final View child = children[i];
        if ((child.mViewFlags & VISIBILITY_MASK) != GONE) {
            //测量某一个子控件宽高
            measureChild(child, widthMeasureSpec, heightMeasureSpec);
        }
    }
}

// 测量某一个child的宽高
protected void measureChild (View child, int parentWidthMeasureSpec,
       int parentHeightMeasureSpec) {
   final LayoutParams lp = child.getLayoutParams();
   //获取子控件的宽高约束规则
   final int childWidthMeasureSpec = getChildMeasureSpec(parentWidthMeasureSpec,
           mPaddingLeft + mPaddingRight, lp. width);
   final int childHeightMeasureSpec = getChildMeasureSpec(parentHeightMeasureSpec,
           mPaddingTop + mPaddingBottom, lp. height);

   child.measure(childWidthMeasureSpec, childHeightMeasureSpec);
}

// 测量某一个child的宽高，考虑margin值
protected void measureChildWithMargins (View child,
       int parentWidthMeasureSpec, int widthUsed,
       int parentHeightMeasureSpec, int heightUsed) {
   final MarginLayoutParams lp = (MarginLayoutParams) child.getLayoutParams();
   //获取子控件的宽高约束规则
   final int childWidthMeasureSpec = getChildMeasureSpec(parentWidthMeasureSpec,
           mPaddingLeft + mPaddingRight + lp. leftMargin + lp.rightMargin
                   + widthUsed, lp. width);
   final int childHeightMeasureSpec = getChildMeasureSpec(parentHeightMeasureSpec,
           mPaddingTop + mPaddingBottom + lp. topMargin + lp.bottomMargin
                   + heightUsed, lp. height);
   //测量子控件
   child.measure(childWidthMeasureSpec, childHeightMeasureSpec);
}


```

  这三个方法分别做了那些工作大家应该比较清楚了吧？measureChildren 就是遍历所有子控件挨个测量，最终测量子控件的方法就是measureChild 和measureChildWithMargins 了，我们先了解几个知识点：

- **measureChildWithMargins跟measureChild的区别就是父控件支不支持margin属性**

  支不支持margin属性对子控件的测量是有影响的，比如我们的屏幕是1080x1920的，子控件的宽度为填充父窗体，如果使用了marginLeft并设置值为100； 
在测量子控件的时候，如果用measureChild，计算的宽度是1080，而如果是使用measureChildWithMargins，计算的宽度是1080-100 = 980。

- **怎样让ViewGroup支持margin属性？**

  ViewGroup中有两个内部类ViewGroup.LayoutParams和ViewGroup. MarginLayoutParams，MarginLayoutParams继承自LayoutParams ，这两个内部类就是VIewGroup的布局参数类，比如我们在LinearLayout等布局中使用的layout_width\layout_hight等以“layout_ ”开头的属性都是布局属性。在View中有一个mLayoutParams的变量用来保存这个View的所有布局属性。

- **LayoutParams和MarginLayoutParams 的关系：** 
  LayoutParams 中定义了两个属性（现在知道我们用的layout_width\layout_hight的来头了吧？）：

```xml
<declare-styleable name= "ViewGroup_Layout">
    <attr name ="layout_width" format="dimension">
        <enum name ="fill_parent" value="-1" />
        <enum name ="match_parent" value="-1" />
        <enum name ="wrap_content" value="-2" />
    </attr >
    <attr name ="layout_height" format="dimension">
        <enum name ="fill_parent" value="-1" />
        <enum name ="match_parent" value="-1" />
        <enum name ="wrap_content" value="-2" />
    </attr >
</declare-styleable >

```

MarginLayoutParams 是LayoutParams的子类，它当然也延续了layout_width\layout_hight 属性，但是它扩充了其他属性：

```xml
< declare-styleable name ="ViewGroup_MarginLayout">
    <attr name ="layout_width" />   <!--使用已经定义过的属性-->
    <attr name ="layout_height" />
    <attr name ="layout_margin" format="dimension"  />
    <attr name ="layout_marginLeft" format= "dimension"  />
    <attr name ="layout_marginTop" format= "dimension" />
    <attr name ="layout_marginRight" format= "dimension"  />
    <attr name ="layout_marginBottom" format= "dimension"  />
    <attr name ="layout_marginStart" format= "dimension"  />
    <attr name ="layout_marginEnd" format= "dimension"  />
</declare-styleable >

```

是不是对布局属性有了一个全新的认识？原来我们使用的margin属性是这么来的。

- **为什么LayoutParams 类要定义在ViewGroup中？** 
  大家都知道ViewGroup是所有容器的基类，一个控件需要被包裹在一个容器中，这个容器必须提供一种规则控制子控件的摆放，比如你的宽高是多少，距离那个位置多远等。所以ViewGroup有义务提供一个布局属性类，用于控制子控件的布局属性。
- **为什么View中会有一个mLayoutParams 变量？** 
  我们在之前学习自定义控件的时候学过自定义属性，我们在构造方法中，初始化布局文件中的属性值，我们姑且把属性分为两种。一种是本View的绘制属性，比如TextView的文本、文字颜色、背景等，这些属性是跟View的绘制相关的。另一种就是以“layout_”打头的叫做布局属性，这些属性是父控件对子控件的大小及位置的一些描述属性，这些属性在父控件摆放它的时候会使用到，所以先保存起来，而这些属性都是ViewGroup.LayoutParams定义的，所以用一个变量保存着。
- **怎样让ViewGroup支持margin属性？** 
  这一部分知识点在后续《自定义ViewGroup》中再去讲解 

### ② getChildMeasureSpec方法

  measureChildWithMargins跟measureChild 都调用了这个方法，其作用就是**通过父控件的宽高约束规则和父控件加在子控件上的宽高布局参数生成一个子控件的约束**。我们知道View的onMeasure方法需要两个参数（父控件对View的宽高约束），这个宽高约束就是通过这个方法生成的。有人会问为什么不直接拿着子控件的宽高参数去测量子控件呢？打个比方，父控件的宽高约束为wrap_content，而子控件为match_perent，是不是很有意思，父控件说我的宽高就是包裹我的子控件，我的子控件多大我就多大，而子控件说我的宽高填充父窗体，父控件多大我就多大。最后该怎么确定大小呢？所以我们需要为子控件重新生成一个新的约束规则。只要记住，**子控件的宽高约束规则是父控件调用getChildMeasureSpec方法生成**。 
![getChildMeasureSpec](http://img.blog.csdn.net/20180109204957470?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

getChildMeasure方法代码不多，也比较简单，就是几个switch将各种情况考虑后生成一个子控件的新的宽高约束，这个方法的结果能够用一个表来概括：

| 父控件的约束规则                         | 子控件的宽高属性                      | 子控件的约束规则    | 说明                                       |
| :------------------------------- | :---------------------------- | :---------- | :--------------------------------------- |
| EXACTLY<br>（父控件是填充父窗体，或者具体size值） | 具体的size/<br>MATCH_PARENT      | EXACTLY     | 子控件如果是具体值，约束尺寸就是这个值，模式为确定的；<br>子控件为填充父窗体，约束尺寸是父控件剩余大小，模式为确定的。 |
|                                  | WRAP-CONTENT                  | AT_MOST     | 子控件如果是包裹内容，约束尺寸值为父控件剩余大小 ，模式为至多          |
| AT_MOST<br>（父控件是包裹内容）            | 具体的size                       | EXACTLY     | 子控件如果是具体值，约束尺寸就是这个值，模式为确定的；              |
|                                  | MATCH_PARENT/<br>WRAP_CONTENT | AT_MOST     | 子控件为填充父窗体或者包裹内容 ，约束尺寸是父控件剩余大小 ，模式为至多     |
| UNSPECIFIED<br>（父控件未指定）          | 具体的size                       | EXACTLY     | 子控件如果是具体值，约束尺寸就是这个值，模式为确定的；              |
|                                  | MATCH_PARENT/<br>WRAP_CONTENT | UNSPECIFIED | 子控件为填充父窗体或者包裹内容 ，约束尺寸0，模式为未指定            |

getChildMeasureSpec源码：

```java
public static int getChildMeasureSpec(int spec, int padding, int childDimension) {
        int specMode = MeasureSpec.getMode(spec);
        int specSize = MeasureSpec.getSize(spec);

        int size = Math.max(0, specSize - padding);

        int resultSize = 0;
        int resultMode = 0;

        switch (specMode) {
        // Parent has imposed an exact size on us
        case MeasureSpec.EXACTLY:
            if (childDimension >= 0) {
                resultSize = childDimension;
                resultMode = MeasureSpec.EXACTLY;
            } else if (childDimension == LayoutParams.MATCH_PARENT) {
                // Child wants to be our size. So be it.
                resultSize = size;
                resultMode = MeasureSpec.EXACTLY;
            } else if (childDimension == LayoutParams.WRAP_CONTENT) {
                // Child wants to determine its own size. It can't be
                // bigger than us.
                resultSize = size;
                resultMode = MeasureSpec.AT_MOST;
            }
            break;

        // Parent has imposed a maximum size on us
        case MeasureSpec.AT_MOST:
            if (childDimension >= 0) {
                // Child wants a specific size... so be it
                resultSize = childDimension;
                resultMode = MeasureSpec.EXACTLY;
            } else if (childDimension == LayoutParams.MATCH_PARENT) {
                // Child wants to be our size, but our size is not fixed.
                // Constrain child to not be bigger than us.
                resultSize = size;
                resultMode = MeasureSpec.AT_MOST;
            } else if (childDimension == LayoutParams.WRAP_CONTENT) {
                // Child wants to determine its own size. It can't be
                // bigger than us.
                resultSize = size;
                resultMode = MeasureSpec.AT_MOST;
            }
            break;

        // Parent asked to see how big we want to be
        case MeasureSpec.UNSPECIFIED:
            if (childDimension >= 0) {
                // Child wants a specific size... let him have it
                resultSize = childDimension;
                resultMode = MeasureSpec.EXACTLY;
            } else if (childDimension == LayoutParams.MATCH_PARENT) {
                // Child wants to be our size... find out how big it should
                // be
                resultSize = View.sUseZeroUnspecifiedMeasureSpec ? 0 : size;
                resultMode = MeasureSpec.UNSPECIFIED;
            } else if (childDimension == LayoutParams.WRAP_CONTENT) {
                // Child wants to determine its own size.... find out how
                // big it should be
                resultSize = View.sUseZeroUnspecifiedMeasureSpec ? 0 : size;
                resultMode = MeasureSpec.UNSPECIFIED;
            }
            break;
        }
        //noinspection ResourceType
        return MeasureSpec.makeMeasureSpec(resultSize, resultMode);
    }
```

进行了上面的步骤，**接下来**就是在measureChildWithMarginsh或者measureChild中 **调用子控件的measure**方法测量子控件的尺寸了。

### ③ View的onMeasure

  View中onMeasure方法已经默认为我们的控件测量了宽高，我们看看它做了什么工作：

```java
protected void onMeasure( int widthMeasureSpec, int heightMeasureSpec) {
    setMeasuredDimension( getDefaultSize(getSuggestedMinimumWidth(), widthMeasureSpec),
            getDefaultSize(getSuggestedMinimumHeight(), heightMeasureSpec));
}
// 为宽度获取一个建议最小值
protected int getSuggestedMinimumWidth () {
    return (mBackground == null) ? mMinWidth : max(mMinWidth , mBackground.getMinimumWidth());
}
// 获取默认的宽高值
public static int getDefaultSize (int size, int measureSpec) {
    int result = size;
    int specMode = MeasureSpec. getMode(measureSpec);
    int specSize = MeasureSpec. getSize(measureSpec);
    switch (specMode) {
    case MeasureSpec. UNSPECIFIED:
        result = size;
        break;
    case MeasureSpec. AT_MOST:
    case MeasureSpec. EXACTLY:
        result = specSize;
        break;
    }
    return result;
}
```

从源码我们了解到：

- 如果View的宽高模式为 未指定，他的宽高将设置为android:minWidth/Height =”“值与背景宽高值中较大的一个；
- 如果View的宽高模式为 EXACTLY （具体的size ），最终宽高就是这个size值；
- 如果View的宽高模式为 EXACTLY （填充父控件 ），最终宽高将为填充父控件；
- 如果View的宽高模式为 AT_MOST （包裹内容），最终宽高也是填充父控件。

也就是说如果我们的自定义控件在布局文件中，只需要设置指定的具体宽高，或者MATCH_PARENT 的情况，我们可以不用重写onMeasure方法。

但如果自定义控件需要设置包裹内容WRAP_CONTENT ，我们需要重写onMeasure方法，为控件设置需要的尺寸；默认情况下WRAP_CONTENT 的处理也将填充整个父控件。

### ④ setMeasuredDimension

  onMeasure方法最后需要调用setMeasuredDimension方法来保存测量的宽高值，如果不调用这个方法，可能会产生不可预测的问题。

## 总结：

> 1. 测量控件大小是父控件发起的
> 2. 父控件要测量子控件大小，需要重写onMeasure方法，然后调用measureChildren或者measureChildWithMargin方法
> 3. onMeasure方法的参数是通过getChildMeasureSpec生成的
> 4. 如果我们自定义控件需要使用wrap_content，我们需要重写onMeasure方法
> 5. 测量控件的步骤： 
>    父控件`onMeasure`->`measureChildren` /`measureChildWithMargin`->`getChildMeasureSpec`-> 
>    子控件的`measure`->`onMeasure`->`setMeasureDimension`-> 
>    父控件onMeasure结束调用`setMeasureDimension`保存自己的大小

### measure() VS onMeasure()

  View中有两个关于测量的方法measure()和onMeasure()。
  **measure**()方法是final的，**不能被重写**。measure()方法中**调用**了**onMeasure**()方法。
  **onMeasure**()方法在View中的默认实现是**测量自身**的**大小**，所以在我们自定义ViewGroup的时候，如果我们不重写onMeasure()测量子控件的大小，子控件将会显示不出来的。ViewGroup中没有重写onMeasure()方法，在ViewGroup的子类（LinearLayout、RelativeLayout等）中会根据容器自身的布局特性，比如LinearLayout有两种布局方式，水平或者垂直，分别对onMeasure()方法进行不同的重写。

  总结一下就是measure()方法不能被重写，它会调用onMeasure()方法测量控件自身的大小，如果控件是一个容器，它需要重写onMeasure()方法遍历测量所有子控件的大小。还有当我们继承View**自定义控件时**，在布局文件中明明使用了wrap_content可结果还是填充父窗体，这是因为View的onMeasure()方法的默认实现，如果**要按照我们的需求正确的测量控件大小也需要重写onMeasure()方法**。

# 自定义布局容器

　　为了实现一些比较牛X的效果，比如侧滑菜单、滑动卡片等等，我们还需要了解自定义ViewGroup。官方文档中对ViewGroup这样描述的：

> ViewGroup是一种可以包含其他视图的特殊视图，他是各种布局和所有容器的基类，这些类也定义了ViewGroup.LayoutParams类作为类的布局参数。

　　之前，我们只是学习过自定义View，其实自定义ViewGroup和自定义View的步骤差不了多少，他们的的区别主要来自各自的作用不同，ViewGroup是容器，用来包含其他控件，而View是真正意义上看得见摸得着的，它需要将自己画出来。ViewGroup需要重写onMeasure方法测量子控件的宽高和自己的宽高，然后实现onLayout方法摆放子控件。而 View则是需要重写onMeasure根据测量模式和父控件给出的建议的宽高值计算自己的宽高，然后再父控件为其指定的区域绘制自己的图形。 

根据以往经验我们初步将自定义ViewGroup的步骤定为下面几步：

```
1. 继承ViewGroup，覆盖构造方法
2. 重写onMeasure方法测量子控件和自身宽高
3. 实现onLayout方法摆放子控件
```

## 1. 简单实现水平排列效果

我们先自定义一个ViewGroup作为布局容器，实现一个从左往右水平排列（排满换行）的效果：

```java
// 自定义布局管理器的示例。
public class CustomLayout extends ViewGroup {
      private static final String TAG = "CustomLayout";

    public CustomLayout(Context context) {
        super(context);
    }

    public CustomLayout(Context context, AttributeSet attrs) {
        this(context, attrs, 0);
    }

    public CustomLayout(Context context, AttributeSet attrs, int defStyle) {
        super(context, attrs, defStyle);
    }

    // 要求所有的孩子测量自己的大小，然后根据这些孩子的大小完成自己的尺寸测量
    @SuppressLint("NewApi") @Override
    protected void onMeasure( int widthMeasureSpec, int heightMeasureSpec) {
        // 计算出所有的childView的宽和高 
        measureChildren(widthMeasureSpec, heightMeasureSpec); 
        
        //测量并保存layout的宽高(使用getDefaultSize时，wrap_content和match_perent都是填充屏幕)
        //稍后会重新写这个方法，能达到wrap_content的效果
        setMeasuredDimension( getDefaultSize(getSuggestedMinimumWidth(), widthMeasureSpec),
                getDefaultSize(getSuggestedMinimumHeight(), heightMeasureSpec));
    }

    // 为所有的子控件摆放位置.
    @Override
    protected void onLayout( boolean changed, int left, int top, int right, int bottom) {
        final int count = getChildCount();
        int childMeasureWidth = 0;
        int childMeasureHeight = 0;
        int layoutWidth = 0;    // 容器已经占据的宽度
        int layoutHeight = 0;   // 容器已经占据的宽度
        int maxChildHeight = 0; //一行中子控件最高的高度，用于决定下一行高度应该在目前基础上累加多少
        for(int i = 0; i<count; i++){
            View child = getChildAt(i);
             //注意此处不能使用getWidth和getHeight，这两个方法必须在onLayout执行完，才能正确获取宽高
            childMeasureWidth = child.getMeasuredWidth(); 
            childMeasureHeight = child.getMeasuredHeight(); 
            if(layoutWidth<getWidth()){
                   //如果一行没有排满，继续往右排列
                  left = layoutWidth;
                  right = left+childMeasureWidth;
                  top = layoutHeight;
                  bottom = top+childMeasureHeight;
            } else{
                   //排满后换行
                  layoutWidth = 0;
                  layoutHeight += maxChildHeight;
                  maxChildHeight = 0;

                  left = layoutWidth;
                  right = left+childMeasureWidth;
                  top = layoutHeight;
                  bottom = top+childMeasureHeight;
            }

            layoutWidth += childMeasureWidth;  //宽度累加
             if(childMeasureHeight>maxChildHeight){
                  maxChildHeight = childMeasureHeight;
            }

             //确定子控件的位置，四个参数分别代表（左上右下）点的坐标值
            child.layout(left, top, right, bottom);
        }
    }
}

```

**布局文件：**

```xml
<?xml version="1.0" encoding= "utf-8"?>
<com.openxu.costomlayout.CustomLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width= "wrap_content"
    android:layout_height= "wrap_content" >

    <Button
        android:layout_width= "wrap_content"
        android:layout_height= "wrap_content"
        android:background= "#FF8247"
        android:textColor= "#ffffff"
        android:textSize="20dip"
        android:padding= "20dip"
        android:text="按钮1" />

    <Button
        android:layout_width= "wrap_content"
        android:layout_height= "wrap_content"
        android:background= "#8B0A50"
        android:textColor= "#ffffff"
        android:textSize="20dip"
        android:padding= "10dip"
        android:text="按钮2222222222222" />

    <Button
        android:layout_width= "wrap_content"
        android:layout_height= "wrap_content"
        android:background= "#7CFC00"
        android:textColor= "#ffffff"
        android:textSize="20dip"
        android:padding= "15dip"
        android:text="按钮333333" />

    <Button
        android:layout_width= "wrap_content"
        android:layout_height= "wrap_content"
        android:background= "#1E90FF"
        android:textColor= "#ffffff"
        android:textSize="10dip"
        android:padding= "10dip"
        android:text="按钮4" />

    <Button
        android:layout_width= "wrap_content"
        android:layout_height= "wrap_content"
        android:background= "#191970"
        android:textColor= "#ffffff"
        android:textSize="20dip"
        android:padding= "15dip"
        android:text="按钮5" />

    <Button
        android:layout_width= "wrap_content"
        android:layout_height= "wrap_content"
        android:background= "#7A67EE"
        android:textColor= "#ffffff"
        android:textSize="20dip"
        android:padding= "20dip"
        android:text="按钮6" />

</com.openxu.costomlayout.CustomLayout>
```

**运行效果：** 
![1](http://img.blog.csdn.net/20180110105939530?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

　　运行成功，是不是略有成就感？这个布局就是简单版的`LinearLayout`设置`android:orientation ="horizontal"`的效果，比他还牛X一点，还能自动换行（哈哈）。接下来我们要实现一个功能，只需要在布局文件中指定布局属性，就能控制子控件在什么位置（类似相对布局`RelativeLayout`）。

## 2. 自定义LayoutParams

　　回想一下我们平时使用`RelativeLayout`的时候，在布局文件中使用`android:layout_alignParentRight="true"`、`android:layout_centerInParent="true"`等各种属性，就能控制子控件显示在父控件的上下左右、居中等效果。
　　ViewGroup中有两个内部类`ViewGroup.LayoutParams`和`ViewGroup.MarginLayoutParams`，`MarginLayoutParams`继承自`LayoutParams`，这两个内部类就是`ViewGroup`的布局参数类，比如我们在`LinearLayout`等布局中使用的`layout_width\layout_hight`等以“layout_ ”开头的属性都是布局属性。 
　　在View中有一个`mLayoutParams`的变量用来保存这个View的所有布局属性。`ViewGroup.LayoutParams`有两个属性`layout_width`和`layout_height`，因为所有的容器都需要设置子控件的宽高，所以这个`LayoutParams`是所有布局参数的基类，如果需要扩展其他属性，都应该继承自它。比如`RelativeLayout`中就提供了它自己的布局参数类`RelativeLayout.LayoutParams`，并扩展了很多布局参数，我们平时在`RelativeLayout`中使用的布局属性都来自它 ：

```xml
<declare-styleable name= "RelativeLayout_Layout">
        <attr name ="layout_toLeftOf" format= "reference" />
        <attr name ="layout_toRightOf" format= "reference" />
        <attr name ="layout_above" format="reference" />
        <attr name ="layout_below" format="reference" />
        <attr name ="layout_alignBaseline" format= "reference" />
        <attr name ="layout_alignLeft" format= "reference" />
        <attr name ="layout_alignTop" format= "reference" />
        <attr name ="layout_alignRight" format= "reference" />
        <attr name ="layout_alignBottom" format= "reference" />
        <attr name ="layout_alignParentLeft" format= "boolean" />
        <attr name ="layout_alignParentTop" format= "boolean" />
        <attr name ="layout_alignParentRight" format= "boolean" />
        <attr name ="layout_alignParentBottom" format= "boolean" />
        <attr name ="layout_centerInParent" format= "boolean" />
        <attr name ="layout_centerVertical" format= "boolean" />
        <attr name ="layout_alignWithParentIfMissing" format= "boolean" />
        <attr name ="layout_toStartOf" format= "reference" />
        <attr name ="layout_toEndOf" format="reference" />
        <attr name ="layout_alignStart" format= "reference" />
        <attr name ="layout_alignEnd" format= "reference" />
        <attr name ="layout_alignParentStart" format= "boolean" />
        <attr name ="layout_alignParentEnd" format= "boolean" />
    </declare-styleable >

```

看了上面的介绍，我们大概知道怎么为我们的布局容器定义自己的布局属性了吧，就不绕弯子了，按照下面的步骤做： 

### ① 大致明确布局容器的需求，初步定义布局属性

　　在定义属性之前要弄清楚，我们自定义的布局容器需要满足那些需求，需要哪些属性，比如，我们现在要实现像相对布局一样，为子控件设置一个位置属性layout_position=""，来控制子控件在布局中显示的位置。暂定位置有五种：左上、左下、右上、右下、居中。有了需求，我们就在attrs.xml定义自己的布局属性（和之前讲的自定义属性一样的操作，不太了解的可以翻阅上文**《深入解析自定义属性》**。

```xml
<?xml version="1.0" encoding= "utf-8"?>
<resources> 
    <declare-styleable name ="CustomLayout">
    <attr name ="layout_position">
        <enum name ="center" value="0" />
        <enum name ="left" value="1" />
        <enum name ="right" value="2" />
        <enum name ="bottom" value="3" />
        <enum name ="rightAndBottom" value="4" />
    </attr >
    </declare-styleable>
</resources>
```

left就代表是左上（按常理默认就是左上方开始，就不用写leftTop了，简洁一点），bottom左下，right 右上，rightAndBottom右下，center居中。属性类型是枚举，同时只能设置一个值。 

### ② 继承LayoutParams，定义布局参数类

　　我们可以选择继承`ViewGroup.LayoutParams`，这样的话我们的布局只是简单的支持`layout_width`和`layout_height`；也可以继承`MarginLayoutParams`，就能使用layout_marginxxx属性了。因为后面我们还要用到margin属性，所以这里方便起见就直接继承`MarginLayoutParams`了。 
　　覆盖构造方法，然后在有AttributeSet参数的构造方法中初始化参数值，这个构造方法才是布局文件被映射为对象的时候被调用的。

```java
public static class CustomLayoutParams extends MarginLayoutParams {
       public static final int POSITION_MIDDLE = 0; // 中间
       public static final int POSITION_LEFT = 1; // 左上方
       public static final int POSITION_RIGHT = 2; // 右上方
       public static final int POSITION_BOTTOM = 3; // 左下角
       public static final int POSITION_RIGHTANDBOTTOM = 4; // 右下角

       public int position = POSITION_LEFT;  // 默认我们的位置就是左上角

       public CustomLayoutParams(Context c, AttributeSet attrs) {
             super(c, attrs);
            TypedArray a = c.obtainStyledAttributes(attrs,R.styleable.CustomLayout );
             //获取设置在子控件上的位置属性
             position = a.getInt(R.styleable.CustomLayout_layout_position ,position );

            a.recycle();
      }

       public CustomLayoutParams( int width, int height) {
             super(width, height);
      }

       public CustomLayoutParams(ViewGroup.LayoutParams source) {
             super(source);
      }
}
```

### ③ 重写generateLayoutParams()

　　在`ViewGroup`中有下面几个关于`LayoutParams`的方法，`generateLayoutParams (AttributeSet attrs)`是在布局文件被填充为对象的时候调用的，这个方法是下面几个方法中最重要的，如果不重写它，我么布局文件中设置的布局参数都不能拿到。RTRT后面我也会专门写一篇博客来介绍布局文件被添加到activity窗口的过程，里面会讲到这个方法被调用的来龙去脉。其他几个方法我们最好也能重写一下，将里面的`LayoutParams`换成我们自定义的`CustomLayoutParams`类，避免以后会遇到布局参数类型转换异常。

```java
@Override
public LayoutParams generateLayoutParams(AttributeSet attrs) {
    return new CustomLayoutParams(getContext(), attrs);
}
@Override
protected ViewGroup.LayoutParams generateLayoutParams(ViewGroup.LayoutParams p) {
    return new CustomLayoutParams (p);
}
@Override
protected LayoutParams generateDefaultLayoutParams() {
    return new CustomLayoutParams (LayoutParams.MATCH_PARENT , LayoutParams.MATCH_PARENT);
}
@Override
protected boolean checkLayoutParams(ViewGroup.LayoutParams p) {
    return p instanceof CustomLayoutParams ;
}
```

### ④ 在onMeasure和onLayout中使用布局参数

　　经过上面几步之后，我们运行程序，就能获取子控件的布局参数了，在onMeasure方法和onLayout方法中，我们按照自定义布局容器的特殊需求，对宽度和位置坐特殊处理。这里我们需要注意一下，如果布局容器被设置为包裹类容，我们只需要保证能将最大的子控件包裹住就ok，代码注释比较详细，就不多说了。

```java
 @Override
protected void onMeasure( int widthMeasureSpec, int heightMeasureSpec) { 
  //获得此ViewGroup上级容器为其推荐的宽和高，以及计算模式  
 int widthMode = MeasureSpec. getMode(widthMeasureSpec); 
 int heightMode = MeasureSpec. getMode(heightMeasureSpec); 
 int sizeWidth = MeasureSpec. getSize(widthMeasureSpec); 
 int sizeHeight = MeasureSpec. getSize(heightMeasureSpec); 
 int layoutWidth = 0;
 int layoutHeight = 0;
      // 计算出所有的childView的宽和高
     measureChildren(widthMeasureSpec, heightMeasureSpec);

      int cWidth = 0;
      int cHeight = 0;
      int count = getChildCount(); 

      if(widthMode == MeasureSpec. EXACTLY){
      //如果布局容器的宽度模式是确定的（具体的size或者match_parent），直接使用父窗体建议的宽度
           layoutWidth = sizeWidth;
     } else{
     //如果是未指定或者wrap_content，我们都按照包裹内容做，宽度方向上只需要拿到所有子控件中宽度做大的作为布局宽度
           for ( int i = 0; i < count; i++)  { 
               View child = getChildAt(i); 
               cWidth = child.getMeasuredWidth(); 
               //获取子控件最大宽度
               layoutWidth = cWidth > layoutWidth ? cWidth : layoutWidth;
           }
     }
      //高度跟宽度处理思想一样
      if(heightMode == MeasureSpec. EXACTLY){
           layoutHeight = sizeHeight;
     } else{
            for ( int i = 0; i < count; i++)  { 
                  View child = getChildAt(i); 
                  cHeight = child.getMeasuredHeight();
                  layoutHeight = cHeight > layoutHeight ? cHeight : layoutHeight;
           }
     }

      // 测量并保存layout的宽高
     setMeasuredDimension(layoutWidth, layoutHeight);
}

@Override
protected void onLayout( boolean changed, int left, int top, int right,
            int bottom) {
      final int count = getChildCount();
      int childMeasureWidth = 0;
      int childMeasureHeight = 0;
     CustomLayoutParams params = null;
      for ( int i = 0; i < count; i++) {
           View child = getChildAt(i);
            // 注意此处不能使用getWidth和getHeight，这两个方法必须在onLayout执行完，才能正确获取宽高
           childMeasureWidth = child.getMeasuredWidth();
           childMeasureHeight = child.getMeasuredHeight();

           params = (CustomLayoutParams) child.getLayoutParams(); 
           switch (params. position) {
            case CustomLayoutParams. POSITION_MIDDLE:    // 中间
                 left = (getWidth()-childMeasureWidth)/2;
                 top = (getHeight()-childMeasureHeight)/2;
                  break;
            case CustomLayoutParams. POSITION_LEFT:      // 左上方
                 left = 0;
                 top = 0;
                  break;
            case CustomLayoutParams. POSITION_RIGHT:     // 右上方
                 left = getWidth()-childMeasureWidth;
                 top = 0;
                  break;
            case CustomLayoutParams. POSITION_BOTTOM:    // 左下角
                 left = 0;
                 top = getHeight()-childMeasureHeight;
                  break;
            case CustomLayoutParams. POSITION_RIGHTANDBOTTOM:// 右下角
                 left = getWidth()-childMeasureWidth;
                 top = getHeight()-childMeasureHeight;
                  break;
            default:
                  break;
           }

            // 确定子控件的位置，四个参数分别代表（左上右下）点的坐标值
           child.layout(left, top, left+childMeasureWidth, top+childMeasureHeight);
     }
}
```

### ⑤ 在布局文件中使用布局属性

　　注意引入命名空间`xmlns:openxu= "http://schemas.android.com/apk/res/包名"`

```xml
<?xml version="1.0" encoding= "utf-8"?>
<com.openxu.costomlayout.CustomLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:openxu= "http://schemas.android.com/apk/res/com.openxu.costomlayout"
    android:background="#33000000"
    android:layout_width= "match_parent "
    android:layout_height= "match_parent" >

    <Button
        android:layout_width= "wrap_content"
        android:layout_height= "wrap_content"
        openxu:layout_position= "left"
        android:background= "#FF8247"
        android:textColor= "#ffffff"
         android:textSize="20dip"
        android:padding= "20dip"
        android:text= "按钮1" />

    <Button
        android:layout_width= "wrap_content"
        android:layout_height= "wrap_content"
        openxu:layout_position= "right"
        android:background= "#8B0A50"
        android:textColor= "#ffffff"
        android:textSize= "18dip"
        android:padding= "10dip"
        android:text= "按钮2222222222222" />

    <Button
        android:layout_width= "wrap_content"
        android:layout_height= "wrap_content"
        openxu:layout_position= "bottom"
        android:background= "#7CFC00"
        android:textColor= "#ffffff"
        android:textSize= "20dip"
        android:padding= "15dip"
        android:text= "按钮333333" />

    <Button
        android:layout_width= "wrap_content"
        android:layout_height= "wrap_content"
        openxu:layout_position= "rightAndBottom"
        android:background= "#1E90FF"
        android:textColor= "#ffffff"
        android:textSize= "15dip"
        android:padding= "10dip"
        android:text= "按钮4" />

    <Button
        android:layout_width= "wrap_content"
        android:layout_height= "wrap_content"
        openxu:layout_position= "center"
        android:background= "#191970"
        android:textColor= "#ffffff"
        android:textSize= "20dip"
        android:padding= "15dip"
        android:text= "按钮5" />

</com.openxu.costomlayout.CustomLayout>
```



**运行效果：** 
　　下面几个效果分别对应布局容器宽高设置不同的属性的情况（设置match_parent 、设置200dip、设置wrap_content）： 
![2](http://img.blog.csdn.net/20180110105924973?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
从运行结果看，我们自定义的布局容器在各种宽高设置下都能很好的测量大小和摆放子控件。现在我们让他支持margin属性

## 3. 支持layout_margin属性

　　如果我们自定义的布局参数类继承自`MarginLayoutParams`，就自动支持了layout_margin属性了，我们需要做的就是直接在布局文件中使用layout_margin属性，然后再`onMeasure`和`onLayout`中使用`margin`属性值测量和摆放子控件。需要注意的是我们测量子控件的时候应该调用`measureChildWithMargin()`方法。



**onMeasure和onLayout：**

```java
protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) { 
   // 获得此ViewGroup上级容器为其推荐的宽和高，以及计算模式   
  int widthMode = MeasureSpec. getMode(widthMeasureSpec); 
  int heightMode = MeasureSpec. getMode(heightMeasureSpec); 
  int sizeWidth = MeasureSpec. getSize(widthMeasureSpec); 
  int sizeHeight = MeasureSpec. getSize(heightMeasureSpec); 
  int layoutWidth = 0;
  int layoutHeight = 0;
       int cWidth = 0;
       int cHeight = 0;
       int count = getChildCount(); 

       // 计算出所有的childView的宽和高
       for( int i = 0; i < count; i++){
            View child = getChildAt(i); 
            measureChildWithMargins(child, widthMeasureSpec, 0, heightMeasureSpec, 0);
      }
      CustomLayoutParams params = null;
       if(widthMode == MeasureSpec. EXACTLY){
             //如果布局容器的宽度模式时确定的（具体的size或者match_parent）
            layoutWidth = sizeWidth;
      } else{
             //如果是未指定或者wrap_content，我们都按照包裹内容做，宽度方向上只需要拿到所有子控件中宽度做大的作为布局宽度
             for ( int i = 0; i < count; i++)  { 
                   View child = getChildAt(i); 
               cWidth = child.getMeasuredWidth(); 
               params = (CustomLayoutParams) child.getLayoutParams(); 
               //获取子控件宽度和左右边距之和，作为这个控件需要占据的宽度
               int marginWidth = cWidth+params.leftMargin+params.rightMargin ;
               layoutWidth = marginWidth > layoutWidth ? marginWidth : layoutWidth;
            }
      }
       //高度跟宽度处理思想一样
       if(heightMode == MeasureSpec. EXACTLY){
            layoutHeight = sizeHeight;
      } else{
             for ( int i = 0; i < count; i++)  { 
                   View child = getChildAt(i); 
                   cHeight = child.getMeasuredHeight();
                   params = (CustomLayoutParams) child.getLayoutParams(); 
                   int marginHeight = cHeight+params.topMargin+params.bottomMargin ;
                   layoutHeight = marginHeight > layoutHeight ? marginHeight : layoutHeight;
            }
      }

       // 测量并保存layout的宽高
      setMeasuredDimension(layoutWidth, layoutHeight);
}

@Override
protected void onLayout( boolean changed, int left, int top, int right,
             int bottom) {
       final int count = getChildCount();
       int childMeasureWidth = 0;
       int childMeasureHeight = 0;
      CustomLayoutParams params = null;
       for ( int i = 0; i < count; i++) {
            View child = getChildAt(i);
             // 注意此处不能使用getWidth和getHeight，这两个方法必须在onLayout执行完，才能正确获取宽高
            childMeasureWidth = child.getMeasuredWidth();
            childMeasureHeight = child.getMeasuredHeight();
            params = (CustomLayoutParams) child.getLayoutParams(); 
      switch (params. position) {
             case CustomLayoutParams. POSITION_MIDDLE:    // 中间
                  left = (getWidth()-childMeasureWidth)/2 - params.rightMargin + params.leftMargin ;
                  top = (getHeight()-childMeasureHeight)/2 + params.topMargin - params.bottomMargin ;
                   break;
             case CustomLayoutParams. POSITION_LEFT:      // 左上方
                  left = 0 + params. leftMargin;
                  top = 0 + params. topMargin;
                   break;
             case CustomLayoutParams. POSITION_RIGHT:     // 右上方
                  left = getWidth()-childMeasureWidth - params.rightMargin;
                  top = 0 + params. topMargin;
                   break;
             case CustomLayoutParams. POSITION_BOTTOM:    // 左下角
                  left = 0 + params. leftMargin;
                  top = getHeight()-childMeasureHeight-params.bottomMargin ;
                   break;
             case CustomLayoutParams. POSITION_RIGHTANDBOTTOM:// 右下角
                  left = getWidth()-childMeasureWidth - params.rightMargin;
                  top = getHeight()-childMeasureHeight-params.bottomMargin ;
                   break;
             default:
                   break;
            }

             // 确定子控件的位置，四个参数分别代表（左上右下）点的坐标值
            child.layout(left, top, left+childMeasureWidth, top+childMeasureHeight);
      }
}
```

**布局文件：**

```xml
<?xml version="1.0" encoding= "utf-8"?>
<com.openxu.costomlayout.CustomLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:openxu= "http://schemas.android.com/apk/res/com.openxu.costomlayout"
    android:background="#33000000"
    android:layout_width= "match_parent"
    android:layout_height= "match_parent" >

    <Button
        android:layout_width= "wrap_content"
        android:layout_height= "wrap_content"
        openxu:layout_position= "left"
        android:layout_marginLeft = "20dip"
        android:background= "#FF8247"
        android:textColor= "#ffffff"
        android:textSize="20dip"
        android:padding= "20dip"
        android:text="按钮1" />

    <Button
        android:layout_width= "wrap_content"
        android:layout_height= "wrap_content"
        android:layout_marginTop = "30dip"
        openxu:layout_position= "right"
        android:background= "#8B0A50"
        android:textColor= "#ffffff"
        android:textSize="18dip"
        android:padding= "10dip"
        android:text="按钮2222222222222" />

    <Button
        android:layout_width= "wrap_content"
        android:layout_height= "wrap_content"
        android:layout_marginLeft = "30dip"
        android:layout_marginBottom = "10dip"
        openxu:layout_position= "bottom"
        android:background= "#7CFC00"
        android:textColor= "#ffffff"
        android:textSize="20dip"
        android:padding= "15dip"
        android:text="按钮333333" />

    <Button
        android:layout_width= "wrap_content"
        android:layout_height= "wrap_content"
        openxu:layout_position= "rightAndBottom"
        android:layout_marginBottom = "30dip"
        android:background= "#1E90FF"
        android:textColor= "#ffffff"
        android:textSize="15dip"
        android:padding= "10dip"
        android:text="按钮4" />

    <Button
        android:layout_width= "wrap_content"
        android:layout_height= "wrap_content"
        openxu:layout_position= "center"
        android:layout_marginBottom = "30dip"
        android:layout_marginRight = "30dip"
        android:background= "#191970"
        android:textColor= "#ffffff"
        android:textSize="20dip"
        android:padding= "15dip"
        android:text="按钮5" />

</com.openxu.costomlayout.CustomLayout>
```

**运行效果：** 
![3](http://img.blog.csdn.net/20180110105917729?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
　　好了，就写到这里，如果想尝试设置其他属性，比如above、below等，感兴趣的同学可以尝试一下哦~。其实也没什么难的，无非就是如果布局属性定义的多，那么在onMeasure和onLayout中考虑的问题就更多更复杂，自定义布局容器就是根据自己的需求，让容器满足我们特殊的摆放要求。

## 总结：

### **自定义ViewGroup的步骤：**

 ①. 继承ViewGroup，覆盖构造方法 
 ②. 重写onMeasure方法测量子控件和自身宽高 
 ③. 实现onLayout方法摆放子控件

### **为布局容器自定义布局属性：**

 ①. 大致明确布局容器的需求，初步定义布局属性 
 ②. 继承LayoutParams，定义布局参数类 
 ③. 重写获取布局参数的方法 
 ④. 在onMeasure和onLayout中使用布局参数
 ⑤. 在布局文件中使用布局属性 

## 自定义控件方法调用流程
[![自定义控件方法调用流程](http://img.blog.csdn.net/20180118122927663?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)](http://img.blog.csdn.net/20180118122927663?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

## layout() VS onLayout()

  首先我们要明确布局的本质是什么，布局就是为View设置四个坐标值，这四个坐标值保存在View的成员变量mLeft、mTop、mRight、mBottom中，方便View在绘制（onDraw）的时候知道应该在那个区域内绘制控件。而我们看到layout()方法中实际上就是为这几个成员变量赋值的，所以到底**真正设置坐标的是layout()方法**，那onLayout()的作用是什么呢？ 
  **onLayout**()都是由**ViewGroup的子类实现**的，他的作用就是确定容器中**每个子控件的位置**，由于不同的容器有不容的布局策略，所以每个容器对onLayout()方法的实现都不同，onLayout()方法会遍历容器中所有的子控件，然后计算他们左上右下的坐标值，最后调用child.layout()方法为子控件设置坐标；由于**layout()方法中又调用了onLayout()方法**，如果子控件child也是一个容器，就会继续为它的子控件计算坐标，如果child不是容器，onLayout()方法将什么也不做，这样下来，只要Activity根窗口mDecor的layout()方法执行完毕，窗口中所有的子容器、子控件都将完成布局操作。

  其实布局过程的调用方式和测量过程是一样的，ViewGroup的子类都要重写onMeasure()方法遍历子控件调用他们的measure()方法，measure()方法又会调用onMeasure()方法，如果子控件是普通控件就完成了测量，如果是容器将会继续遍历其孙子控件。　　
  
**源码下载：**
[https://github.com/openXu/View-CustomLayout](https://github.com/openXu/View-CustomLayout)









引用：
[Android自定义View（一、初体验自定义TextView）](http://blog.csdn.net/xmxkf/article/details/51454685)
[Android自定义View（二、深入解析自定义属性）](http://blog.csdn.net/xmxkf/article/details/51468648)
[Android自定义View（三、深入解析控件测量onMeasure）](http://blog.csdn.net/xmxkf/article/details/51490283)
[Android自定义ViewGroup（四、打造自己的布局容器）](http://blog.csdn.net/xmxkf/article/details/51500304)
】



【Android 自定义View之绘图】
【
# 【Android 自定义View之绘图】
# 基础图形的绘制

## 一、Paint与Canvas

绘图需要两个工具，笔和纸。这里的 `Paint`相当于笔，而 `Canvas`相当于纸，不过需要注意的是 `Canvas`（画布）无限大，没有边界，切记理解成只有屏幕大小。我这里打个比方， `Canvas`是整个天空，而屏幕是通过窗户看到的景色。

那么我需要改变**画笔大小，粗细，颜色，透明度**，字体样式等都需要在 `Paint`里面设置；
同样要画出**圆形，矩形，不规则形状**都是在 `Canvas`里面操作的。

### Paint

#### Paint的基本设置函数

1. mPaint.setAntiAlias(true) //设置是否抗锯齿;
2. mPaint.setStyle(Paint.Style.FILL_AND_STROKE); //设置填充样式
3. mPaint.setColor(Color.GREEN);//设置画笔颜色
4. mPaint.setStrokeWidth(2);//设置画笔宽度
5. mPaint.setShadowLayer(10,15,15,Color.RED);//设置阴影

#### 1 、setAntiAlias(true) 设置是否抗锯齿

设置抗锯齿会使图像边缘更清晰一些，锯齿痕迹不会那么明显。

![](http://upload-images.jianshu.io/upload_images/9028834-842aa6fc510f3506?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 2、setStyle (Paint.Style style) 设置填充样式

`Paint.Style` 类型：

`Paint.Style.FILL_AND_STROKE` 填充且描边 
`Paint.Style.STROKE` 描边 
`Paint.Style.FILL` 填充

看下上面三种类型，这里以矩形为例：

![](http://upload-images.jianshu.io/upload_images/9028834-94680eb3c27c71d9?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 3、setColor(@ColorInt int color) 设置画笔颜色



#### 4、setStrokeWidth(float width) 设置画笔宽度



#### 5、setShadowLayer(float radius, float dx, float dy, int shadowColor) 设置阴影

先来看看参数代表的含义：

**radius** ： 表示阴影的倾斜度 
**dx** ： 水平位移 
**dy** : 垂直位移 
**shadowColor** : 阴影颜色

看一个简单的例子：

```java
paint.setShadowLayer(5,10,10,Color.parseColor("#abc133"));
```

效果图：

![](http://upload-images.jianshu.io/upload_images/9028834-7a47463e2a6ab302?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这里你可能有疑问，为啥我自己演示了一篇却看不到矩形，圆形等图形的阴影，只能看到文本的阴影呢？那么我们需要注意的是：**这个方法不支持硬件加速，所以我们要测试时必须先关闭硬件加速。**

那么请加上`setLayerType(LAYER_TYPE_SOFTWARE, null);` 并且确保你的最小`api8`以上。

### Canvas
下文【Canvas详细讲解】有Canvas进一步说明

**画布背景**设置：

```java
canvas.drawColor(Color.BLUE);
canvas.drawRGB(255, 255, 0);  
```

这两个功能一样，都是用来设置背景颜色的。

我们只需要重写`onDraw(Canvas canvas)`方法，就可以绘制你想要的图形了。

```java
@Override
protected void onDraw(Canvas canvas) {
    super.onDraw(canvas);
     //绘制图形
}
```

## 二、基本几何图形绘制

### 1、画直线drawLine

方法预览：

```java
drawLine(float startX, float startY, float stopX, float stopY, @NonNull Paint paint)
```

参数：

startX ： 开始点X坐标 
startY ： 开始点Y坐标 
stopX ： 结束点X坐标 
stopY ： 结束点Y坐标

```java
paint.setStyle(Paint.Style.FILL);
paint.setStrokeWidth(5);    
paint.setColor(Color.parseColor("#FF0000"));
canvas.drawLine(100,100,600,600,paint);
```

![](http://upload-images.jianshu.io/upload_images/9028834-b3599780e7e5ebb2.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 2、多条直线drawLines

方法预览：
```java
drawLines(@Size(min=4,multiple=2) @NonNull float[] pts, int offset, int count, Paint paint)
drawLines(@Size(min=4,multiple=2) @NonNull float[] pts, @NonNull Paint paint)
```

参数：

**pts** : 是点的集合且**大小最小为4而且是2的倍数**。表示每2个点连接形成一条直线，pts 的组织方式为{x1,y1,x2,y2….} 
**offset** : 集合中跳过的数值个数，注意不是点的个数！一个点是两个数值 
**count** : 参与绘制的数值的个数，指pts[]里数值个数，而不是点的个数，因为一个点是两个数值

还是来看个例子：

```java
@Override
protected void onDraw(Canvas canvas) {
    Paint paint = new Paint();
    paint.setAntiAlias(true);
    paint.setStyle(Paint.Style.FILL);
    paint.setStrokeWidth(5);    
    
    float [] pts={50,100,100,200,200,300,300,400};
    paint.setColor(Color.RED);
    canvas.drawLines(pts,paint);

    paint.setColor(Color.BLUE);
    canvas.drawLines(pts,1,4,paint);//去掉第一个数50，取之后的4个数即100,100,200,200
}
```
红线：点（50,100）和点（100,200）连接成一条直线；点（200,300）和点（300,400）连接成直线。
蓝线：点（100,100）和点（200,200）连接成一条直线；
![](http://upload-images.jianshu.io/upload_images/9028834-953e9a50b015d4d3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 3、点及多个点drawPoint、drawPoints

方法预览：

```java
drawPoint(float x, float y, @NonNull Paint paint)

drawPoints(@Size(multiple=2) @NonNull float[] pts, @NonNull Paint paint)
drawPoints(@Size(multiple=2) @NonNull float[] pts, int offset, int count, @NonNull Paint paint)

```
点的绘制和上面直线的绘制一样，我这里就不再累诉了。

### 4、矩形drawRect、drawRoundRect

方法预览：

```java
drawRect(@NonNull RectF rect, @NonNull Paint paint)

drawRect(@NonNull Rect r, @NonNull Paint paint)

drawRect(float left, float top, float right, float bottom, @NonNull Paint paint)
```

区别`RectF` 与`Rect` ，**RectF**坐标系是**浮点型**；**Rect**坐标系是**整形**。

圆角矩形方法预览：

```java
drawRoundRect(@NonNull RectF rect, float rx, float ry, @NonNull Paint paint)

drawRoundRect(float left, float top, float right, float bottom, float rx, float ry, @NonNull Paint paint)
```

参数：
RectF ： 绘制的矩形 
rx ： 生成圆角的椭圆X轴半径 
ry ： 生成圆角的椭圆Y轴的半径

```java
RectF rect = new RectF(100, 10, 500, 300);
canvas.drawRoundRect(rect, 60, 20, paint);
```
![](http://upload-images.jianshu.io/upload_images/9028834-cb0e1f3ac77724af.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 5、圆形drawCircle

方法预览：

```java
drawCircle(float cx, float cy, float radius, @NonNull Paint paint)
```

参数：

cx ： 圆心X坐标 
cy ： 圆心Y坐标 
radius ： 半径

```java
canvas.drawCircle(400,400,300,paint);
```

![](http://upload-images.jianshu.io/upload_images/9028834-a2002a13f118a06b?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 6、椭圆drawOval

方法预览：

``` java
drawOval(@NonNull RectF oval, @NonNull Paint paint)

drawOval(float left, float top, float right, float bottom, @NonNull Paint paint)
```

![](http://upload-images.jianshu.io/upload_images/9028834-d31bdcdd3309b008?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 7、圆弧drawArc

方法预览：

```java
drawArc(@NonNull RectF oval, float startAngle, float sweepAngle, boolean useCenter, @NonNull Paint paint)

drawArc(float left, float top, float right, float bottom, float startAngle, float sweepAngle,
            boolean useCenter, @NonNull Paint paint)
```

参数：
oval ： 生成椭圆的矩形 
**startAngle** ： 弧开始的角度 （X轴正方向为0度，**顺时针弧度增大**） 
**sweepAngle** ： 绘制多少**弧度** （注意不是结束弧度） 
**useCenter** ： 是否有**弧的两边** true有两边 false无两边

画笔设置填充：

```java
RectF rect=new RectF(0,0,300,400);

paint.setStyle(Paint.Style.FILL);

paint.setColor(Color.RED);
canvas.drawArc(rect,30,30,false,paint);

paint.setColor(Color.BLUE);
canvas.drawArc(rect,120,30,true,paint);

paint.setStyle(Paint.Style.STROKE);

paint.setColor(Color.GREEN);
canvas.drawArc(rect,0,360,true,paint);

paint.setColor(Color.RED);
canvas.drawArc(rect,-30,30,false,paint);

paint.setColor(Color.BLUE);
canvas.drawArc(rect,-120,30,true,paint);
```
![](http://upload-images.jianshu.io/upload_images/9028834-b99e99b628fb8c25.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

说明：
![](http://upload-images.jianshu.io/upload_images/9028834-6681c82193213405.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 引用：
[自定义View之绘图](http://blog.csdn.net/u012551350/article/details/51323986)


# 路径（Path）

## Path常用方法

| 方法 | 作用 | 备注 |
| --- | - | -- |
| **moveTo** | 移动起点 | 移动下一次操作的起点位置 |
| **lineTo** | 连接直线 | 连接上一个点到当前点之间的直线 |
| **setLastPoint** | 设置终点 | 重置最后一个点的位置 |
| **close** | 闭合路劲 | 从最后一个点连接最初的一个点，形成一个闭合区域 |
| **addRect** | 添加矩形 | 添加矩形到当前Path |
| **addRoundRect** | 添加圆角矩形 | 添加圆角矩形到当前Path |
| **addOval** | 添加椭圆 | 添加椭圆到当前Path |
| **addCircle** | 添加圆 | 添加圆到当前Path |
| **addPah** | 添加路劲 | 添加路劲到当前Path |
| **addArc** | 添加圆弧 | 添加圆弧到当前Path |
| **arcTo** | 圆弧 | 绘制圆弧，注意和addArc的区别 |
| **isEmpty** | 是否为空 | 判定Path是否为空 |
| **isRect** | 是否为矩形 | 判定Path是否是一个矩形 |
| **set** | 替换路劲 | 用新的路劲替换当前路劲的所有内容 |
| **offset** | 偏移路劲 | 对当前的路劲进行偏移 |
| **quadTo** | 贝塞尔曲线 | 二次贝塞尔曲线的方法 |
| **cubicTo** | 贝塞尔曲线 | 三次贝塞尔曲线的方法 |
| **rMoveTo<br>rlineTo<br>rQuadTo<br>rCubicTo** | rXxx方法 | 不带r的方法是基于原点坐标系（偏移量），带r的基于当前点坐标系（偏移量） |
| **op** | 布尔操作 | 对两个Path进行布尔运算（交集，并集）等操作 |
| **setFillType** | 填充模式 | 设置Path的填充模式 |
| **getFillType** | 填充模式 | 获取Path的填充 |
| **isInverseFillType** | 是否逆填充 | 判断是否是逆填充模式 |
| **toggleInverseFillType** | 相反模式 | 切换相反的填充模式 |
| **getFillType** | 填充模式 | 获取Path的填充 |
| **incReserve** | 提示方法 | 提示Path还有多少个点等待加入 |
| **computeBounds** | 计算边界 | 计算Path的路劲 |
| **reset，rewind** | 重置路劲 | 清除Path中的内容（reset相当于new Path , rewind 会保留Path的数据结构） |
| **transform** | 矩阵操作 | 矩阵变换 |

## Path方法使用详解

使用Path不仅可以绘制简单的图形（如圆形，矩形，直线等），也可以绘制复杂一些的图形（如正多边形，五角星等），还有绘制裁剪和绘制文本都会用到Path。由于方法比较多，我这里分组来讲下。

### moveTo , lineTo , setLastPoint , close

先创建画笔：

``` java
paint = new Paint();
paint.setAntiAlias(true);
paint.setStyle(Paint.Style.STROKE);
paint.setStrokeWidth(10);
paint.setColor(Color.parseColor("#FF0000"));
``` 

**注意`paint.setStyle(Paint.Style.FILL);`，设置画笔为实心。一些线条将在画布上看不见。**

#### 1、lineTo

首先我们来看看`lineTo`,如果你直接`moveTo` 将看不出效果。

``` java
Path path = new Path();

path.lineTo(200,200);
path.lineTo(400,0);

canvas.drawPath(path,paint);
``` 


![](http://upload-images.jianshu.io/upload_images/9028834-574dde4fdbd6c585?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

为了方便大家好观察坐标的变化，我在屏幕上画出了网格，每块网格的宽高都是100。由于**第一次**之前没有过操作，所以**默认点**就是**原点**（屏幕左上角），第一次lineTo就是坐标原点到(200,200)之间的直线。**第二次**lineTo就是**上一次结束点**位置(200,200)到(400,0)点之间的直线。

#### 2、moveTo 和setLastPoint

方法预览：

``` java
moveTo(float x, float y) 

setLastPoint(float dx, float dy)
``` 

这两个方法在作用上有相似之处，却是两个不同的东西，具体参考下表：

| 方法名 | 作用 | 是否影响之前的操作 | 是否影响之后的操作 |
| --- | :-: | :--: | :--: |
| **moveTo** | 移动下一次操作的起点位置 | 否 | 是 |
| **setLastPoint** | 改变上一次操作点的位置 | 是 | 是 |



来看看下面的例子：

``` java
Path path = new Path();

path.lineTo(200, 200);
path.moveTo(300,300);//moveTo
path.lineTo(400, 0);

canvas.drawPath(path, paint);
``` 

效果图：
![](http://upload-images.jianshu.io/upload_images/9028834-68174ee0295898e7?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

``` java
Path path = new Path();

path.lineTo(200, 200);
path.setLastPoint(300,100);//setLastPoint
path.lineTo(400, 0);

canvas.drawPath(path, paint);
``` 

效果图：
![](http://upload-images.jianshu.io/upload_images/9028834-10b612d4dbbaccf6?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

当我们绘制线条之前，调用`moveTo` 和 `setLastPoint`效果是一样的，都是对坐标原点(0,0)进行操作。
**setLastPoint**是**重置上一次操作的最后一点**，在执行完第一次lineTo的时候，最后一个点就是(200,200),setLastPoint更改(200,200)为(300,100)，所以在执行的时候就是(300,100)到(400, 0)之间的连线了。

#### 3、close

方法预览

``` java
public void close()
``` 

`close`方法**连接最后一个点和最初一个点**（如果两个点不重合）形成一个**闭合**的**图形**。

``` java
path.moveTo(100,100);
path.lineTo(500,100);
path.lineTo(300,400);
path.close();
canvas.drawPath(path, paint);
``` 

效果图：
![](http://upload-images.jianshu.io/upload_images/9028834-4bc5e18f75a23014.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上图中可以看到`lineTo(500,100)`直线和`lineTo(300,400)`直线，而`close`方法就是连接(300,400)，(100,100)两点，形成一个闭合的区域。

注意：close的作用的封闭路径，如果连接**最后一个点和最初一个点任然无法形成闭合的区域，那么close什么也不做**。

### quadTo,cubicTo

二次贝塞尔曲线以及三次贝塞尔曲线。

#### 1、quadTo

方法预览

``` java
public void quadTo(float x1, float y1, float x2, float y2)
``` 

`quadTo`方法其中 (x1,y1) 为控制点，(x2,y2)为结束点。

``` java
path.moveTo(100,400);
path.quadTo(300, 100, 400, 400);

canvas.drawPath(path, paint);
``` 

效果图：
![](http://upload-images.jianshu.io/upload_images/9028834-418f2c31c4fa03d0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 2、cubicTo

方法预览：

``` java
public void cubicTo(float x1, float y1, float x2, float y2, float x3, float y3)
``` 

`cubicTo`方法比`quadTo`方法多了一个点坐标，那么其中(x1,y1) 为控制点，(x2,y2)为控制点，(x3,y3) 为结束点。

``` java
path.moveTo(100, 400);
path.cubicTo(100, 400, 300, 100, 400, 400);

canvas.drawPath(path, paint);
``` 

绘制的图形和上面的`quadTo`绘制的图形是一样的。我们去掉`moveTo`来看看运行的效果图：

![](http://upload-images.jianshu.io/upload_images/9028834-2d7fd3afd51d5a48?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果你想了解[贝塞尔曲线公式，请链接这里](http://baike.baidu.com/link?url=jRgMYu81F4aEnyyWXUB65Ihj0fV6rbDhzEZhO4xE6ZU7r98F-fzRewd4-Os88iX_gBlHguZWeB4k97VGKmqvVq)

### addXxx和arcTo

主要是向`Path`中添加基本图形以及区分`addArc`和`arcTo`

#### 1、添加基本图形

方法预览：

``` java
//圆形
addCircle(float x, float y, float radius, Path.Direction dir)
//椭圆
addOval(RectF oval, Path.Direction dir)
addOval(float left, float top, float right, float bottom, Path.Direction dir)
//矩形
addRect(RectF rect, Path.Direction dir)
addRect(float left, float top, float right, float bottom, Path.Direction dir)
//圆角矩形
addRoundRect(RectF rect, float rx, float ry, Path.Direction dir) 
addRoundRect(float left, float top, float right, float bottom, float rx, float ry, Path.Direction dir)
addRoundRect(RectF rect, float[] radii, Path.Direction dir)
addRoundRect(float left, float top, float right, float bottom, float[] radii, Path.Direction dir)
``` 

我们仔细观察上面的方法，在最后都有一个`Path.Direction`，这是个什么东东呢？
**Direction**的意思是方向，指导，趋势。点进去跟一下你会发现Direction是一个枚举类型（Enum）分别有**CW（顺时针）**，**CCW（逆时针）**两个常量。那么它的作用主要有以下两点：

| 序号 | 作用 |
| --- | :-: |
| **1** | 在添加图形时确定闭合顺序(各个点的记录顺序) |
| **2** | 对自相交图形的渲染结果有影响 |


我们先来看看闭合顺序的问题，添加一个矩形看看：

``` java
path.addRect(100, 200, 500, 400, Path.Direction.CW);

canvas.drawPath(path, paint);
``` 

![](http://upload-images.jianshu.io/upload_images/9028834-b8f7e6c068f3c358?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我将上面的代码`CW`改成`CCW`再运行一次，结果一模一样。
想看到区别就要用到`setLastPoint`（重置最后一个点的坐标）。我们来这样变变代码：

``` java
path.addRect(100, 200, 500, 400, Path.Direction.CW);
path.setLastPoint(200,400);

canvas.drawPath(path, paint);
``` 

效果立马现行：

![](http://upload-images.jianshu.io/upload_images/9028834-53f54e2e3835c59c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

为什么图形会发生奇怪的变化呢。我们先来分析一下，绘制一个矩形至少需要对角线的两个点，根据这两个点计算出四条边然后把四条边按照顺序连接起来。上图的起始坐标是（100,200）按着顺时针的方向连接（500,200），（500,400），（100,400）最后连接（100,200）形成一个矩形。`setLastPoint`是重置上一个操作点坐标及改变（100,400）为（200,400），所以出现了上图的效果。

接下来我们看看逆时针的情况：

``` java
path.addRect(100, 200, 500, 400, Path.Direction.CCW);
path.setLastPoint(400,300);

canvas.drawPath(path, paint);
``` 

效果图：

![](http://upload-images.jianshu.io/upload_images/9028834-7e9a3654df3c1320?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们理清楚了闭合的问题，相交问题与设置填充模式有关。

我以`addCircle`方法来讲解添加图形

``` java
path.addCircle(300,300,200, Path.Direction.CW);//（300,300）点表示圆心坐标，200 表示半径长度

canvas.drawPath(path, paint);
``` 
![](http://upload-images.jianshu.io/upload_images/9028834-1ee02d1c7d16ac9f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```java
path.addCircle(300, 300, 200, Path.Direction.CCW);//（300,300）点表示圆心坐标，200 表示半径长度
path.setLastPoint(300,400);
canvas.drawPath(path, paint);
```        
![](http://upload-images.jianshu.io/upload_images/9028834-cdee00372a7a5b56.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 2、addPath

方法预览：

``` java
public void addPath(Path src)
public void addPath(Path src, float dx, float dy)//`dx,dy`指的是偏移量
public void addPath(Path src, Matrix matrix)//添加到当前path之前先使用Matrix进行变换
``` 

**addPath**方法就是将两个路径合并到一起。
``` java
Path path = new Path();
path.addRect(100,100,400,300, Path.Direction.CW);

Path src=new Path();
src.addCircle(300,300,100, Path.Direction.CW);
path.addPath(src,0,100);

canvas.drawPath(path, paint);
``` 

效果图：
![](http://upload-images.jianshu.io/upload_images/9028834-4ea3da589e881dd6?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 3、addArc与arcTo

方法预览：

``` java
addArc(RectF oval, float startAngle, float sweepAngle)
addArc(float left, float top, float right, float bottom, float startAngle, float sweepAngle)

arcTo(RectF oval, float startAngle, float sweepAngle)
arcTo(RectF oval, float startAngle, float sweepAngle, boolean forceMoveTo)
arcTo(float left, float top, float right, float bottom, float startAngle, float sweepAngle, boolean forceMoveTo)
``` 

从方法名字上面看，这两个方法都是与圆弧有关，那么他们之间肯定是有区别的：

| 名称 | 作用 | 区别 |
| --- | :-: | :-: |
| **addArc** | 添加一个圆弧到Path | 直接添加一个圆弧到path中，**和上一次操作点无关** |
| **arcTo** | 添加一个圆弧到Path | 添加一个圆弧到path中，如果圆弧的起点和**上次操作点坐标不同就连接两个点** |


`startAngle`表示开始圆弧度数（0度与X轴方向对齐，顺时针移动，弧度增大）。
`sweepAngle`表示运动了多少弧度，并不是结束弧度。

**forceMoveTo**表示“是否强制使用moveTo”，也就是说是否使用moveTo将上一次操作点移动到圆弧的起点坐标。默认是false。

| forceMoveTo | 含义 |
| --- | :-: |
| **true** | 将最后一个点移动到圆弧起点，即不连接最后一个点与圆弧起点 |
| **false** | 不移动，而是连接最后一个点与圆弧起点(注意之前没有操作的话，不会连接原点) |


示例：

``` java
path.lineTo(200, 200);
RectF rectF = new RectF(100, 100, 400, 400);
path.arcTo(rectF, 0, 270, true);
// path.addArc(rectF,0,270);和上面一句等价

canvas.drawPath(path, paint);
``` 
效果图：
![](http://upload-images.jianshu.io/upload_images/9028834-847ac9c0bf8ec0ee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


我们把 `path.arcTo(rectF, 0, 270, true);`改成 `path.arcTo(rectF, 0, 270, false);`，来看看效果图：
![](http://upload-images.jianshu.io/upload_images/9028834-dc7bdcb74c512135.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

从上面两张图可以看出明显的变化。

### isEmpty、 isRect、 set 和 offset

#### isEmpty

判断path中是否包含内容。

``` java
Path path = new Path();
Log.e("-----","----"+path.isEmpty());//-----: ----true
path.lineTo(100,100);
Log.e("-----","----"+path.isEmpty());//-----: ----false

canvas.drawPath(path, paint);
``` 

#### isRect

方法预览：

``` java
isRect(RectF rect)
``` 

判断path是否是一个矩形，如果是一个矩形的话，会将矩形的信息存放进参数rect中。

``` java
Path path = new Path();
RectF rectF = new RectF();
rectF.left = 100;
rectF.top = 100;
rectF.right = 400;
rectF.bottom = 300;
path.addRect(rectF, Path.Direction.CW);
boolean isRect = path.isRect(rectF);
Log.e("-----","------"+isRect);//-----: ------true
``` 

#### set

方法预览：

``` java
public void set(Path src)
``` 

将新的path赋值到现有path。相当于运算符中的“=”，如`a=b`,把`b`赋值给`a`

还是一起来看个例子：

``` java
Path path = new Path();
path.addRect(100,100,400,300, Path.Direction.CW);
Path src=new Path();
src.addCircle(300,200,100, Path.Direction.CW);
path.set(src);
canvas.drawPath(path, paint);
``` 

效果图：
![](http://upload-images.jianshu.io/upload_images/9028834-30b9ebd268b5acb6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### offset

方法预览：

``` java
public void offset(float dx, float dy)
public void offset(float dx, float dy, Path dst)
``` 

这个方法就是对`Path`进行一段**平移**，正方向和X轴，Y轴方向一致（如果dx为正数则向右平移，反之向左平移；如果dy为正则向下平移，反之向上平移）。
我们看到第二个方法多了一个`dst`，这个又是一个什么玩意呢，其实参数**dst**是**存储平移后的path**的。

用例子来说明一下：

``` java
Path path = new Path();
path.addCircle(300, 200, 100, Path.Direction.CW);

Path dst = new Path();
dst.addCircle(500, 200, 200, Path.Direction.CW);

path.offset(-100, 100, dst);

paint.setColor(Color.RED);
canvas.drawPath(path, paint);

paint.setColor(Color.BLUE);
canvas.drawPath(dst, paint);
``` 

效果图：
![](http://upload-images.jianshu.io/upload_images/9028834-5e2f35dc42be9405.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
从运行效果图可以看出，虽然我们在dst中添加了一个圆形，但是并没有表现出来，所以，当dst中存在内容时，**dst中原有的内容会被清空**，而**存放平移后的path**。
而**原来的path并没有变化**。

### FillType

方法预览：

``` java
public void setFillType(Path.FillType ft)
public Path.FillType getFillType()
``` 

`setFillType`方法中的参数`Path.FillType`为枚举类型：

| FillType值 | 含义 |
| --- | :-: |
| **FillType.WINDING** | 取path所有所在区域 默认值 |
| **FillType.EVEN_ODD** | 取path所在并不相交区域 |
| **FillType.INVERSE_WINDING** | 取path所有未占区域 |
| **FillType.INVERSE_EVEN_ODD** | 取path未占或相交区域 |

#### setFillType

##### **WINDING**

``` java
Path path = new Path();
path.addCircle(300,200,100, Path.Direction.CW);
path.addCircle(200,200,100, Path.Direction.CW);
path.setFillType(Path.FillType.WINDING);
canvas.drawPath(path, paint);
``` 

效果图：
![](http://upload-images.jianshu.io/upload_images/9028834-694e76b1d8ecb72c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


##### **EVEN_ODD**
![](http://upload-images.jianshu.io/upload_images/9028834-a32243dc688659db?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### **INVERSE_WINDING**

![](http://upload-images.jianshu.io/upload_images/9028834-4657b536e2e64ef5?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

##### **INVERSE_EVEN_ODD**

![](http://upload-images.jianshu.io/upload_images/9028834-2b40a26e92bdbea0?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### isInverseFillType

是否是逆填充模式：
WINDING 和 EVEN_ODD 返回false； 
INVERSE_WINDING 和 INVERSE_EVEN_ODD 返回true；

#### toggleInverseFillType

切换相反的填充模式，如果填充模式为`WINDING`则填充模式为`INVERSE_WINDING`，反之为`WINDING`模式；如果填充模式为`EVEN_ODD`则填充模式为`INVERSE_EVEN_ODD`，反之为`EVEN_ODD`模式。

举个例子：

``` java
Path path = new Path();
path.addCircle(300,200,100, Path.Direction.CW);
path.addCircle(200,200,100, Path.Direction.CW);
path.setFillType(Path.FillType.INVERSE_EVEN_ODD);
path.toggleInverseFillType();
canvas.drawPath(path, paint);
``` 

效果图和上面`EVEN_ODD`模式一模一样。
![](http://upload-images.jianshu.io/upload_images/9028834-e2889119a6588692?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 引用：
[自定义View之绘图篇（二）：路径（Path）](http://blog.csdn.net/u012551350/article/details/51245793)

# 文字（Text）

## 一、文字

相关方法预览：
``` java
//普通设置
paint.setAntiAlias(true); //指定是否使用抗锯齿功能  如果使用会使绘图速度变慢 默认false
setStyle(Paint.Style.FILL);//绘图样式  对于设文字和几何图形都有效  
setTextAlign(Align.LEFT);//设置文字对齐方式  取值：align.CENTER、align.LEFT或align.RIGHT 默认align.LEFT
paint.setTextSize(12);//设置文字大小

//样式设置  
paint.setFakeBoldText(true);//设置是否为粗体文字  
paint.setUnderlineText(true);//设置下划线  
paint.setTextSkewX((float) -0.25);//设置字体水平倾斜度  普通斜体字是-0.25  
paint.setStrikeThruText(true);//设置带有删除线效果 

//其它设置  
paint.setTextScaleX(2);//只会将水平方向拉伸  高度不会变  
``` 

### 1、文本绘图样式

先来看看下面这个例子：

``` java
mPaint.setStrokeWidth(5);
mPaint.setTextSize(80);
//设置绘图样式 为填充
mPaint.setStyle(Paint.Style.FILL);
canvas.drawText("我是一颗小小的石头", 100,100, mPaint);

//设置绘图样式 为描边
mPaint.setStyle(Paint.Style.STROKE);
canvas.drawText("我是一颗小小的石头", 100,300, mPaint);

//设置绘图样式 为填充且描边
mPaint.setStyle(Paint.Style.FILL_AND_STROKE);
canvas.drawText("我是一颗小小的石头", 100,500, mPaint);
``` 

效果图：
![](http://upload-images.jianshu.io/upload_images/9028834-60dde666d7eaec04.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 2、setTextAlign(Paint.Align align) 文字的对齐方式

``` java
mPaint.setStyle(Paint.Style.FILL);
mPaint.setTextSize(80);
//设置对齐方式  左对齐
mPaint.setTextAlign(Paint.Align.LEFT);
canvas.drawText("小小的石头", 500,100, mPaint);//点（500,100）在文本的左边

//设置对齐方式  中间对齐
mPaint.setTextAlign(Paint.Align.CENTER);
canvas.drawText("小小的石头", 500,200, mPaint);//点（500,100）在文本的中间

//设置对齐方式  右对齐
mPaint.setTextAlign(Paint.Align.RIGHT);
canvas.drawText("小小的石头", 500,300, mPaint);//点（500,100）在文本的右边
``` 
![](http://upload-images.jianshu.io/upload_images/9028834-835660280911a92f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 3、文字样式设置

``` java
canvas.drawText("小小的石头", 200, 100, mPaint); //不带任何效果

mPaint.setFakeBoldText(true);//是否粗体文字
mPaint.setUnderlineText(true);//设置下划线
mPaint.setStrikeThruText(true);//设置删除线效果
canvas.drawText("小小的石头", 200, 200, mPaint);
``` 
![](http://upload-images.jianshu.io/upload_images/9028834-0096e6f553395589?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 4、文字倾斜度设置

``` java
mPaint.setTextSkewX(-0.25f);
canvas.drawText("小小的石头", 100, 100, mPaint);

mPaint.setTextSkewX(0.25f);
canvas.drawText("小小的石头", 100, 200, mPaint);

mPaint.setTextSkewX(-0.5f);
canvas.drawText("小小的石头", 100, 300, mPaint);
``` 
![](http://upload-images.jianshu.io/upload_images/9028834-4742b4de15f87b2b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可见普通斜体字是`-0.25f`，大于`-0.25f` 向左倾斜，小于 `-0.25f` 向右倾斜。

### 5、水平拉伸设置

``` java
mPaint.setTextScaleX(1);//不拉伸
canvas.drawText("小小的石头", 100, 100, mPaint);

mPaint.setTextScaleX(2);//水平方向拉伸2倍
canvas.drawText("小小的石头", 100, 200, mPaint);

mPaint.setTextScaleX(3);//水平方向拉伸3倍
canvas.drawText("小小的石头", 100, 300, mPaint);
``` 
![](http://upload-images.jianshu.io/upload_images/9028834-f60c78ea567e447a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

由上可以发现，仅是水平方向拉伸，高度并未改变。

## canvas绘制文字

### 1、drawText

方法预览：

``` java
drawText(String text, float x, float y, Paint paint)

drawText(char[] text, int index, int count, float x, float y, Paint paint)
		//text 字节数组；index 表示第一个要绘制的文字索引；count 需要绘制的文字个数

drawText(CharSequence text, int start, int end, float x, float y, Paint paint)
//text 表示字符；start 开始截取字符的索引号；end 结束截取字符的索引号。[start , end ) 包含 start 但不包含 end

//`drawTextRun`方法是在 **skd23** 才引入的方法
drawTextRun(char[] text, int index, int count, int contextIndex, int contextCount, float x, float y, boolean isRtl, Paint paint)//isRtl 表示排列顺序，true 表示正序，false 表示倒序

drawTextRun(CharSequence text, int start, int end, int contextStart, int contextEnd, float x, float y, boolean isRtl, Paint paint)
``` 

第一个构造函数 ： 是最普通的。 
第二个构造函数 ： text 字节数组；index 表示第一个要绘制的文字索引；count 需要绘制的文字个数。 
第三个构造函数 ： text 表示字符 （注意与上面比较）；start 开始截取字符的索引号；end 结束截取字符的索引号。（注意和上面的区别） [start , end ) 包含 start 但不包含 end

第四个构造函数和第五个构造函数 ： contextIndex 和 index 相同 ； contextCount 大于等于 count ； isRtl 表示排列顺序，true 表示正序，false 表示倒序（这里的倒是指第一个字符变到最后一个字符，最后一个字符变到第一个字符）。 注意了`drawTextRun`方法是在 **skd23** 才引入的方法。

``` java
canvas.drawText("我是一颗小小的石头".toCharArray(), 1, 4, 100, 100, mPaint);

canvas.drawText("我是一颗小小的石头", 1, 4, 100, 200, mPaint);

//最小sdk23
canvas.drawTextRun("我是一颗小小的石头".toCharArray(), 1, 4, 1, 4, 100, 300, true, mPaint);

canvas.drawTextRun("我是一颗小小的石头".toCharArray(), 1, 4, 1, 4, 100, 400, false, mPaint);
``` 
![](http://upload-images.jianshu.io/upload_images/9028834-e7d881a57942fd2c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 2、drawPosText

方法预览：

``` java
drawPosText(String text, float[] pos, Paint paint)

drawPosText(char[] text, int index, int count, float[] pos, Paint paint)
``` 

这里的参数含义和 `drawText` 方法的参数一样。我们来看个简单的例子：

``` java
float[] pos = {100, 100, 200, 200, 300, 300, 400, 400, 500, 500, 600, 600};
canvas.drawPosText("我是一颗小小", pos, mPaint);
``` 

![](http://upload-images.jianshu.io/upload_images/9028834-a1957986f79301b7?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 3、drawTextOnPath

方法预览：

``` java
drawTextOnPath(String text, Path path, float hOffset, float vOffset, Paint paint)

drawTextOnPath(char[] text, int index, int count, Path path, float hOffset, float vOffset, Paint paint)
``` 

参数含义：

- index，count ： 和上面截取参数含义一样，这里不再累诉。

- **hOffset** ： 与路径**起点**的**水平偏移量**。
正数向 X 轴正方形移动（右移）；负数向 X 轴负方向移动（左移）；
如果是圆弧：正数是顺时针的偏移量；反之是逆时针的偏移量

- **vOffset** ： 与路径**中心**的**垂直偏移量**。
正数向 Y 轴正方形移动（下移）；负数向 Y 轴负方向移动（上移）
如果是圆弧：正数是向圆心移动；反之是远离圆心

``` java
mPath.moveTo(100,100);
mPath.lineTo(800,100);
canvas.drawTextOnPath("我是一颗小小的石头",mPath,10,-10,mPaint);
``` 
![](http://upload-images.jianshu.io/upload_images/9028834-6b4f3f3f7749cce5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

路径为圆弧的例子：
``` java
mPath.addCircle(500,500,200, Path.Direction.CW);
canvas.drawTextOnPath("我是一颗小小的石头",mPath,40,-20,mPaint);
``` 

![11](http://upload-images.jianshu.io/upload_images/9028834-6d7f42ff5ea25238?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 4、Typeface（字体样式设置）

方法预览：

``` java
setTypeface(Typeface typeface)
``` 

`Typeface`是用来设置字体样式的，通过`paint.setTypeface()`来指定。可以指定系统中的字体样式，也可以指定自定义的样式文件中获取。
要构建Typeface时，可以指定所用样式的正常体、斜体、粗体等，如果指定样式中，没有相关文字的样式就会用系统默认的样式来显示，一般默认是宋体。

参数类型是枚举类型，枚举值如下：

1.  Typeface.NORMAL //正常体
2.  Typeface.BOLD //粗体
3.  Typeface.ITALIC //斜体
4.  Typeface.BOLD_ITALIC //粗斜体

#### a、系统字体

方法预览：

``` java
create(String familyName, int style) //字体名

create(Typeface family, int style)  //类型

defaultFromStyle(int style)       //默认类型
``` 

我们来看一个简单的例子：

``` java
typeface = Typeface.create("宋体", Typeface.NORMAL);
mPaint.setTypeface(typeface);
canvas.drawText("我是一颗小小的石头", 100, 100, mPaint);

typeface = Typeface.create("楷体", Typeface.NORMAL);
mPaint.setTypeface(typeface);
canvas.drawText("我是一颗小小的石头", 100, 200, mPaint);
``` 

![12](http://upload-images.jianshu.io/upload_images/9028834-b6c6a34f079f6b71?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

从上图可以看出来，设置楷体根本没起作用，在系统的字体当中没有找到楷体。

#### b、自定义字体

方法预览：

``` java
createFromAsset(AssetManager mgr, String path) //Asset中获取

createFromFile(File path) //文件路径获取

createFromFile(String path) //外部路径获取
``` 

由于后面两个方法比较简单，主要来看一下第一个方法。

首先在`main`下创建`assets`文件夹，然后在`assets`文件夹创建`fonts`文件夹，最后在`fonts`文件夹下放入`font1.ttf`，如图：

![13](http://upload-images.jianshu.io/upload_images/9028834-a604e57b4241e562?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

``` java
typeface = Typeface.createFromAsset(mContext.getAssets(), "fonts/font1.ttf");
//Typeface.createFromFile(mContext.getFilesDir()+"/font1.ttf")
mPaint.setTypeface(typeface);
canvas.drawText("我是一颗小小的石头", 100, 100, mPaint);

typeface = Typeface.createFromAsset(mContext.getAssets(), "fonts/font2.ttf");
mPaint.setTypeface(typeface);
canvas.drawText("我是一颗小小的石头", 100, 200, mPaint);
``` 

效果图：
![14](http://upload-images.jianshu.io/upload_images/9028834-a4f885fbe526a525?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


## 引用：
[自定义View之绘图篇（三）：文字（Text）](http://blog.csdn.net/u012551350/article/details/51330308)

# baseLine和FontMetrics
了解baseLine和FontMetrics有助于我们理解drawText()绘制文字的原理
## 一、baseLine 基线

记得小时候练习字母用的是四线格本，把字母写在四线格内，如下：

![](http://upload-images.jianshu.io/upload_images/9028834-2b11647b05a3a47c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

那么在`canvas`中`drawText`绘制文字时候，也是有规则的，这个规则就是**`baseLine`（基线）**。什么又是基线了，说白了就是一条直线，我们这里理解的是确定它的位置。我们先来看一下基线：

![9](http://upload-images.jianshu.io/upload_images/9028834-413ca2ea05e411d9?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

从上图看出：基线等同四线格的第三条线，在`Android`中基线的位置定了，那么文字的位置也就定了。

### 1、canvas.drawText()

方法预览：

``` java
drawText(String text, float x, float y, Paint paint)
``` 

参数：
text 需要绘制的文字 
x 绘制文字原点X坐标 
y 绘制文字原点Y坐标 
paint 画笔

我们先来看一张图：

![](http://upload-images.jianshu.io/upload_images/9028834-ce07ac8e13f78439.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



需要注意的是x,y并不是文字左上角的坐标点，它比较特殊，**y所代表的是基线坐标y的坐标**。

我们具体来看看drawText()方法，这里以一个例子的形式来理解：


``` java
      mPaint.setAntiAlias(true);
      mPaint.setColor(Color.RED);
      mPaint.setStyle(Paint.Style.FILL);
      mPaint.setTextSize(120);

      canvas.drawText("abcdefghijk",200,200,mPaint);

      mPaint.setColor(Color.parseColor("#23AC3B"));
      canvas.drawLine(200,200,getWidth(),200,mPaint);
``` 

效果图：
![](http://upload-images.jianshu.io/upload_images/9028834-ae69246767794b55.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

证实了y是基线y的坐标点。

>结论：
1、canvas.drawText()中参数**y是基线y的坐标** 
2、x坐标、基线位置、文字大小确定，文字的位置就是确定的了。

### 2、mPaint.setTextAlign(Paint.Align.XXX);

我们可以从上面的例子看出，x代表的是文字开始绘制的地方。我第一次使用的时候也是这么认为的，可是我写了几个例子，发现我理解错了。那么正确的理解又是什么呢?

**x代表所要绘制文字所在矩形的相对位置**。相对位置就是指定点（x,y）在所要绘制矩形的位置。我们知道所绘制矩形的纵坐标是由Y值来确定的，而相对x坐标的位置，只有左、中、右三个位置了。也就是所绘制矩形可能是在x坐标相对于文字的左侧，中间或者右边绘制，而定义在x坐标在所绘制矩形相对位置的函数是：
```java
setTextAlign(Paint.Align align)
```

 

Paint.Align是枚举类型，值分别为 ： Paint.Align.LEFT，Paint.Align.CENTER和Paint.Align.RIGHT。

我们来分别看一看设置不同值时，绘制的结果是怎么样的。


#### （1）、Paint.Align.LEFT

``` java
mPaint.setTextAlign(Paint.Align.LEFT);//主要是这里的取值不一样
mPaint.setAntiAlias(true);
mPaint.setColor(Color.RED);
mPaint.setStyle(Paint.Style.FILL);
mPaint.setTextSize(120);

canvas.drawText("abcdefghijk", 200, 200, mPaint);

mPaint.setColor(Color.parseColor("#23AC3B"));
canvas.drawLine(0, 200, getWidth(), 200, mPaint);
canvas.drawLine(200, 0, 200, getHeight(), mPaint);
``` 

效果图如下：

![6](http://upload-images.jianshu.io/upload_images/9028834-4b311ea5567ca50f?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看出`（x,y）`在`文字矩形`下边的左边。

#### （2）、Paint.Align.CENTER

``` java
mPaint.setTextAlign(Paint.Align.CENTER);
mPaint.setAntiAlias(true);
mPaint.setColor(Color.RED);
mPaint.setStyle(Paint.Style.FILL);
mPaint.setTextSize(120);

canvas.drawText("abcdefghijk", 200, 200, mPaint);

mPaint.setColor(Color.parseColor("#23AC3B"));
canvas.drawLine(0, 200, getWidth(), 200, mPaint);
canvas.drawLine(200, 0, 200, getHeight(), mPaint);
``` 

效果图：

![7](http://upload-images.jianshu.io/upload_images/9028834-1b4e5f409eefe0d9?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看出`（x,y）`位于`文字矩形`下边的中间，换句话说，系统会根据`(x,y)`的位置和`文字矩形`大小，会计算出当前开始绘制的点。以使原点`(x,y)`正好在所要绘制的矩形下边的中间。

#### （3）、Paint.Align.RIGHT

``` java
mPaint.setTextAlign(Paint.Align.RIGHT);
mPaint.setAntiAlias(true);
mPaint.setColor(Color.RED);
mPaint.setStyle(Paint.Style.FILL);
mPaint.setTextSize(120);

canvas.drawText("abcdefghijk", 200, 200, mPaint);

mPaint.setColor(Color.parseColor("#23AC3B"));
canvas.drawLine(0, 200, getWidth(), 200, mPaint);
canvas.drawLine(200, 0, 200, getHeight(), mPaint);
``` 

效果图：
![](http://upload-images.jianshu.io/upload_images/9028834-ed35625a2059a31d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


可以看出`（x,y）`在`文字矩形`下边的右边。

## 二、FontMetrics

![10](http://upload-images.jianshu.io/upload_images/9028834-4ca2401c62a7eaa0?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

从图中可以知道，除了基线，还有另外的四条线，它们分别是 `top`，`ascent`，`descent`和`bottom`，它们的含义分别为：

1.  top：可绘制的最高高度所在线
2.  bottom：可绘制的最低高度所在线
3.  ascent ：系统建议的，绘制单个字符时，字符应当的最高高度所在线
4.  descent：系统建议的，绘制单个字符时，字符应当的最低高度所在线

### 1、获取实例

``` java
Paint.FontMetrics fontMetrics = mPaint.getFontMetrics();

Paint.FontMetricsInt fm=  mPaint.getFontMetricsInt();
``` 

两个构造方法的区别是，得到对象的成员变量的值一个为`float`类型，一个为`int`类型。

### 2、成员变量

FontMetrics，它里面有如下五个成员变量：

``` java
float ascent = fontMetrics.ascent;
float descent = fontMetrics.descent;
float top = fontMetrics.top;
float bottom = fontMetrics.bottom;
float leading = fontMetrics.leading;
``` 

ascent,descent,top,bottom,leading 这些线的位置要怎么计算出来呢？我们先来看个图：

![11](http://upload-images.jianshu.io/upload_images/9028834-099c5fa0ba1b12ee?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

那么它们的计算方法如下：


>**ascent** = ascent线的y坐标 **- baseline**线的y坐标；//负数
**descent** = descent线的y坐标 **- baseline**线的y坐标；//正数
**top** = top线的y坐标 **- baseline**线的y坐标；//负数
**bottom** = bottom线的y坐标 **- baseline**线的y坐标；//正数

>**leading** = **top**线的y坐标 **- ascent**线的y坐标；//负数


FontMetrics的这几个变量的值都是**以baseLine为基准**的。
对于ascent来说，baseline线在ascent线之下，所以必然baseline的y值要大于ascent线的y值，所以ascent变量的值是负的。其他几个同理。

同样我们可以推算出：

>ascent线Y坐标 = baseline线的y坐标 + fontMetric.ascent；
descent线Y坐标 = baseline线的y坐标 + fontMetric.descent；
**top线Y坐标** = **baseline**线的y坐标 **+ fontMetric.top**；
bottom线Y坐标 = baseline线的y坐标 + fontMetric.bottom；


### 3、绘制ascent，descent，top，bottom线

直接贴代码：

``` java
int baseLineY = 200;
mPaint.setTextSize(120);
canvas.drawText("abcdefghijkl's", 200, baseLineY, mPaint);

Paint.FontMetrics fontMetrics = mPaint.getFontMetrics();

float top = fontMetrics.top + baseLineY;
float ascent = fontMetrics.ascent + baseLineY;
float descent = fontMetrics.descent + baseLineY;
float bottom = fontMetrics.bottom + baseLineY;

//绘制基线
mPaint.setColor(Color.parseColor("#FF1493"));
canvas.drawLine(0, baseLineY, getWidth(), baseLineY, mPaint);

//绘制top直线
mPaint.setColor(Color.parseColor("#FFB90F"));
canvas.drawLine(0, top, getWidth(), top, mPaint);

//绘制ascent直线
mPaint.setColor(Color.parseColor("#b03060"));
canvas.drawLine(0, ascent, getWidth(), ascent, mPaint);

//绘制descent直线
mPaint.setColor(Color.parseColor("#912cee"));
canvas.drawLine(0, descent, getWidth(), descent, mPaint);

//绘制bottom直线
mPaint.setColor(Color.parseColor("#1E90FF"));
canvas.drawLine(0, bottom, getWidth(), bottom, mPaint);
``` 

在这段代码中，我们需要注意的是：
canvas.drawText()中参数y是基线y的位置； 
mPaint.setTextAlign(Paint.Align.LEFT);指点（200，200）在文字矩形的左边。然后计算各条直线的y坐标:


``` java
float top = fontMetrics.top + baseLineY;
float ascent = fontMetrics.ascent + baseLineY;
float descent = fontMetrics.descent + baseLineY;
float bottom = fontMetrics.bottom + baseLineY;
``` 

效果图：
![](http://upload-images.jianshu.io/upload_images/9028834-eb31f76c780a050a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


### 4、绘制文字最小矩形、文字宽度、文字高度

#### （1）绘制文字最小矩形

由`drawRect(float left, float top, float right, float bottom, @NonNull Paint paint)`需要绘制矩形就需要知道矩形左上角坐标点，矩形长和宽。以上面例子为例：

left = 200 
top = ascent ； 
right= 200+矩形宽度； 
bottom = descent；

这样我们就可以绘制出最小矩形：

![](http://upload-images.jianshu.io/upload_images/9028834-4043ad0562bb9560?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### （2）文字高度

``` java
float top = fontMetrics.top + baseLineY;
float bottom = fontMetrics.bottom + baseLineY;
//文字高度
float height= bottom - top; //注意top为负数
//文字中点y坐标
float center = (bottom - top) / 2;
``` 

当然也可以： `float height=Math.abs(top-bottom);`

#### （3）文字宽度

``` java
 String text="abcdefghijkl's";
 //文字宽度
 float width = mPaint.measureText(text);
``` 

### 已知中线，获取baseline

你可能会说这个还不简单：

``` java
Paint mPaint = new Paint();
mPaint.setTextSize(80);
mPaint.setColor(Color.WHITE);
mPaint.setAntiAlias(true);
String text = "FontMetrics的那些猜想";
Paint.FontMetrics fm = mPaint.getFontMetrics();
//获取文字高度
float fontHeight = fm.bottom - fm.top;
//获取文字宽度
float fontWidth = mPaint.measureText(text);
//绘制中线
canvas.drawLine(0, centerY, getWidth(), centerY, mPaint);
//绘制文本
canvas.drawText(text, centerX - fontWidth / 2, centerY + fontHeight / 2, mPaint);
``` 

效果图：

![font](http://upload-images.jianshu.io/upload_images/9028834-6a7d0c74cfeb1dcd?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

怎么会这样呢？

我们一起来分析下原因，先来看一张分析图：

![font](http://upload-images.jianshu.io/upload_images/9028834-eac9e570d2c9809c?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

那么我们就可以得出：

``` java
baseline=centerY+A-fm.bottom;
``` 

如果以：

``` java
baseline=centerY + fontHeight / 2;
``` 

那么就会以bottom线作为文字的基线，这样就会造成文字位于中线之下。

#### 结论

我们最终可知，当给定中间线center位置以后，那么baseline的位置为：

``` java
baseline = center + (FontMetrics.bottom - FontMetrics.top)/2 - FontMetrics.bottom;
``` 

`FontMetrics.bottom`注意这里为正数。

效果一览：

![font](http://upload-images.jianshu.io/upload_images/9028834-2bf7ea948be585e4?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们还可以这样获取文字高度：

``` java
public float getFontHeight(Paint paint, String str) {
    Rect rect = new Rect();
    paint.getTextBounds(str, 0, str.length(), rect);
    return rect.height();
}
``` 

经测试得出：

``` java
Paint.FontMetrics fm = mPaint.getFontMetrics();
``` 

**注意**：fm 值和手机密度没有关系，并且`fm.bottom/fm.top=4`（约等于）。

## 引用：
[自定义View之绘图篇（四）：baseLine和FontMetrics](http://blog.csdn.net/u012551350/article/details/51361778)


# Canvas


## 一、什么是Canvas？

什么是Canvas？官方文档是这么介绍的：

>The Canvas class holds the “draw” calls. To draw something, you need 4 basic components: A Bitmap to hold the pixels, a Canvas to host the draw calls (writing into the bitmap), a drawing primitive (e.g. Rect,Path, text, Bitmap), and a paint (to describe the colors and styles for the drawing).

Canvas 类是用于绘图的，绘制图形，你需要**4个基本元素**：

*   画在哪。画在Bitmap上。（相当于纸张，我们把图画在纸张上面）

*   怎么画。（调用canvas执行绘图操作。比如canvas.drawCircle()，canvas.drawLine()，canvas.drawPath()将我们需要的图像画出来。）

*   画的内容。（比如我想在纸张画一朵花，根据自己需求画圆，画直线，画路径等）

*   用什么画。（在纸张上画一朵花，肯定是用笔来画的，这里的笔指的是 Paint）

**Canvas 画布无限大**，它并没有边界。怎么来理解这句话呢？打个比方：画布就是窗外的景色，而手机屏幕就是窗口，你在窗口看到窗外的景色是有限的。同样我也可以把图形画到屏幕之外，通过对 Canvas 的变换与操作，让屏幕之外的图形显示到屏幕里面。

## 二、Canvas 绘图

Canvas 绘制一些常见的图形：

``` java
mPaint.setColor(Color.RED);
//绘制直线
canvas.drawLine(100,100,600,100,mPaint);

//绘制矩形
canvas.drawRect(100,200,600,400,mPaint);

//绘制文字
mPaint.setTextSize(60);
mPaint.setStrokeWidth(2);
mPaint.setStyle(Paint.Style.FILL);
canvas.drawText("我是一颗石头",100,500,mPaint);
``` 
![](http://upload-images.jianshu.io/upload_images/9028834-726a58b3974c70e8?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 三、Canvas 的变换与操作

有时候我们还需要对 Canvas 做一些操作，比如旋转，裁剪，平移等等。

*   canvas.translate 平移

*   canvas.rotate 旋转

*   canvas.scale 缩放

*   canvas.skew 错切

*   canvas.clipRect 裁剪

*   canvas.save和canvas.restore 保存和恢复

*   PorterDuffXfermode 图像混合 （paint相关方法）


### 1、平移（translate）

canvas中有一个函数translate（）是用来实现画布平移的，画布的原状是以左上角为原点，向左是X轴正方向，向下是Y轴正方向，如下图所示

![](http://upload-images.jianshu.io/upload_images/9028834-09b50cebfdee3e5f?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

translate函数其实实现的相当于平移坐标系，即平移坐标系的原点的位置。translate（）函数的原型如下：

```java
void translate(float dx, float dy)
```

参数说明：
float dx：水平方向平移的距离，正数指向正方向（向右）平移的量，负数为向负方向（向左）平移的量
flaot dy：垂直方向平移的距离，正数指向正方向（向下）平移的量，负数为向负方向（向上）平移的量

```java
protected void onDraw(Canvas canvas) {
    super.onDraw(canvas);      
    Paint paint = new Paint();  
    paint.setColor(Color.GREEN);  
    paint.setStyle(Style.FILL);  
//translate  平移,即改变坐标系原点位置      
//  canvas.translate(100, 100);  
    Rect rect1 = new Rect(0,0,400,220);  
    canvas.drawRect(rect1, paint);  
}  
```

1、上面这段代码，先把canvas.translate(100, 100);注释掉，看原来矩形的位置，然后打开注释，看平移后的位置，对比如下图：
![](http://upload-images.jianshu.io/upload_images/9028834-66b7efc1b3706ccd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 屏幕显示与Canvas的关系

很多童鞋一直以为显示所画东西的改屏幕就是Canvas，其实这是一个非常错误的理解，比如下面我们这段代码：

这段代码中，同一个矩形，在画布平移前画一次，平移后再画一次，大家会觉得结果会怎样？

```java
protected void onDraw(Canvas canvas) {
    super.onDraw(canvas);  
      
    //构造两个画笔，一个红色，一个绿色  
    Paint paint_green = generatePaint(Color.GREEN, Style.STROKE, 3);  
    Paint paint_red   = generatePaint(Color.RED, Style.STROKE, 3);  
      
    //构造一个矩形  
    Rect rect1 = new Rect(0,0,400,220);  
    //在平移画布前用绿色画下边框  
    canvas.drawRect(rect1, paint_green);  
      
    //平移画布后,再用红色边框重新画下这个矩形  
    canvas.translate(100, 100);  
    canvas.drawRect(rect1, paint_red);  
}  
private Paint generatePaint(int color, Paint.Style style, int width) {
    Paint paint = new Paint();
    paint.setColor(color);
    paint.setStyle(style);
    paint.setStrokeWidth(width);
    return paint;
} 
```

代码分析：
这段代码中，对于同一个矩形，在平移画布前利用绿色画下矩形边框，在平移后，再用红色画下矩形边框。大家是不是会觉得这两个边框会重合？实际结果是这样的。

![](http://upload-images.jianshu.io/upload_images/9028834-e4c35a2782fc2ac5?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


**为什么绿色框并没有移动？**

这是由于**屏幕显示与Canvas根本不是一个概念**！
**Canvas**是一个很虚幻的概念，**相当于一个透明图层**（用过PS的同学应该都知道），每次Canvas画图时（即调用Draw系列函数），都会产生一个透明图层，然后在这个图层上画图，画完之后覆盖在屏幕上显示。所以上面的两个结果是由下面几个步骤形成的：

1、调用canvas.drawRect(rect1, paint_green);时，产生一个Canvas透明图层，由于当时还没有对坐标系平移，所以坐标原点是（0，0）；再在系统在Canvas上画好之后，覆盖到屏幕上显示出来，过程如下图：

![](http://upload-images.jianshu.io/upload_images/9028834-1eb4f422ca01bc2a?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

2、然后再第二次调用canvas.drawRect(rect1, paint_red);时，又会重新产生一个全新的Canvas画布，但此时画布坐标已经改变了，即向右和向下分别移动了100像素，所以此时的绘图方式为：（合成视图，从上往下看的合成方式）

![](http://upload-images.jianshu.io/upload_images/9028834-1a24417016f91674?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

上图展示了，上层的Canvas图层与底部的屏幕的合成过程，由于Canvas画布已经平移了100像素，所以在画图时是以新原点来产生视图的，然后合成到屏幕上，这就是我们上面最终看到的结果了。我们看到屏幕移动之后，有一部分超出了屏幕的范围，那超出范围的图像显不显示呢，当然不显示了！也就是说，Canvas上虽然能画上，但超出了屏幕的范围，是不会显示的。当然，我们这里也没有超出显示范围，两框框而已。

#### 总结：
1、**每次**调用**canvas.draw**XXXX系列函数来绘图进，都会产生一个**全新**的Canvas**画布**。
2、如果在DrawXXX前，调用平移、旋转等函数来对Canvas进行了操作，那么这个操作是不可逆的！每次产生的画布的最新位置都是这些操作后的位置。（关于Save()、Restore()的画布可逆问题的后面再讲）
3、在Canvas与屏幕合成时，超出屏幕范围的图像是不会显示出来的。

### 2、旋转（Rotate）

画布的旋转是默认是**围绕坐标原点**来旋转的，这里容易产生错觉，看起来觉得是图片旋转了，其实我们旋转的是画布，以后在此画布上画的东西显示出来的时候全部看起来都是旋转的。其实Roate函数有两个构造函数：

```java
void rotate(float degrees)
void rotate (float degrees, float px, float py) 
```

第一个构造函数直接输入旋转的度数，正数是顺时针旋转，负数指逆时针旋转，它的旋转中心点是原点（0，0）
第二个构造函数除了度数以外，还可以指定旋转的中心点坐标（px,py）

下面以第一个构造函数为例，旋转一个矩形，先画出未旋转前的图形，然后再画出旋转后的图形；

```java
protected void onDraw(Canvas canvas) {
    super.onDraw(canvas);  
    Paint paint_green = generatePaint(Color.GREEN, Style.FILL, 5);  
    Paint paint_red   = generatePaint(Color.RED, Style.STROKE, 5);  
      
    Rect rect1 = new Rect(300,10,500,100);  
    canvas.drawRect(rect1, paint_red); //画出原轮廓  
      
    canvas.rotate(30);//顺时针旋转画布  
    canvas.drawRect(rect1, paint_green);//画出旋转后的矩形  
}   
```

效果图是这样的：

![](http://upload-images.jianshu.io/upload_images/9028834-70eb9369b4c7c90d?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这个最终屏幕显示的构造过程是这样的：

下图显示的是第一次画图合成过程，此时仅仅调用canvas.drawRect(rect1, paint_red); 画出原轮廓

![](http://upload-images.jianshu.io/upload_images/9028834-4c347350ac78131f?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后是先将Canvas正方向依原点旋转30度，然后再与上面的屏幕合成，最后显示出我们的复合效果。

![](http://upload-images.jianshu.io/upload_images/9028834-eb392b3dc2366ba5?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

有关Canvas与屏幕的合成关系我觉得我已经讲的够详细了，后面的几个操作Canvas的函数，我就不再一一讲它的合成过程了。

### 3、缩放（scale ）

```java
public void scale (float sx, float sy) 
public final void scale (float sx, float sy, float px, float py)
```

##### 第一个构造函数：
float **sx**:**水平方向伸缩**的比例，假设原坐标轴的比例为n,**不变时为1**，在变更的X轴密度为n*sx;所以，sx为小数为缩小，sx为整数为放大
float **sy**:垂直方向伸缩的比例，同样，小数为缩小，整数为放大

注意：这里有X、Y轴的密度的改变，显示到图形上就会正好相同，比如X轴缩小，那么显示的图形也会缩小。一样的。

```java
        Paint paint = new Paint();
        paint.setAntiAlias(true);
        paint.setStyle(Paint.Style.STROKE);
        paint.setStrokeWidth(8);
        
        Rect rect = new Rect(100, 100, 200, 200);
//原图
        paint.setColor(Color.RED);
        canvas.drawRect(rect, paint);
//画布缩放方法1
        canvas.scale(0.5f, 2f);
        paint.setColor(Color.BLUE);
        canvas.drawRect(rect, paint);
```
![](http://upload-images.jianshu.io/upload_images/9028834-13112d7a64d464a5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
因为是整个画布的伸缩，对应连Stroke线的粗细也发生了变化

由图可知：
原图：Rect(100, 100, 200, 200)
移动后：Rect(50, 200, 100, 400)
**公式**：
```
原图：(l, t, r, b)
scale (sx,  sy)
移动后：(l*sx, t*sy, r*sx, b*sy)
```


##### 第二个构造函数：
>void scale (float sx, float sy, float px, float py)
Preconcat the current matrix with the specified scale.
Parameters
sx
float: The amount to scale in X
sy
float: The amount to scale in Y
**px**
float: **The x-coord for the pivot point** (unchanged by the scale)
**py**
float: The y-coord for the pivot point (unchanged by the scale)

**px 和 py 分别为缩放的中心点**，不设置的话默认为画布原点(0, 0)

```java
        Paint paint = new Paint();
        paint.setAntiAlias(true);
        paint.setStyle(Paint.Style.STROKE);
        paint.setStrokeWidth(4);

        Rect rect = new Rect(100, 100, 200, 200);
//原图
        canvas.save();
        paint.setColor(Color.RED);
        canvas.drawRect(rect, paint);
//画布缩放方法2
        canvas.scale(2f, 2f, 0, 0);
        paint.setColor(Color.BLUE);
        canvas.drawRect(rect, paint);
//画布缩放方法2（以正方形中心点为缩放中心）
        canvas.restore();
        canvas.scale(2f, 2f, 150, 150);
        paint.setColor(Color.GREEN);
        canvas.drawRect(rect, paint);
```
![](http://upload-images.jianshu.io/upload_images/9028834-afcea41f356fc5ab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

###### 原理
**源码**如下：
``` java
public final void scale(float sx, float sy, float px, float py) {
    translate(px, py);
    scale(sx, sy);
    translate(-px, -py);
}
``` 

步骤：
先**将画布平移px,py，然后scale，scale结束之后再将画布平移-px,-py**。
```java
        Paint paint = new Paint();
        paint.setAntiAlias(true);
        paint.setStyle(Paint.Style.STROKE);
        paint.setStrokeWidth(8);

        Rect rect = new Rect(100, 100, 200, 200);
//原图
        paint.setColor(Color.RED);
        canvas.drawRect(rect, paint);
//画布缩放方法2
        canvas.scale(0.5f, 2f, 100, 100);//px、py
        paint.setColor(Color.BLUE);
        canvas.drawRect(rect, paint);
```
![](http://upload-images.jianshu.io/upload_images/9028834-e182faf72356fbee.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**原理演示**：
```java
        Paint paint = new Paint();
        paint.setAntiAlias(true);
        paint.setStyle(Paint.Style.STROKE);
        paint.setStrokeWidth(8);
        
        Rect rect = new Rect(100, 100, 200, 200);

//原图
        paint.setColor(Color.RED);
        canvas.drawRect(rect, paint);
//以下三步模拟canvas.scale(0.5f, 2f, 100, 100);
//第①步：移动
        canvas.translate(100, 100);
        paint.setColor(Color.GREEN);
        canvas.drawRect(rect, paint);
//第②步：缩放
        canvas.scale(0.5f, 2f);
        paint.setColor(Color.YELLOW);
        canvas.drawRect(rect, paint);
//第③步：反向移动
        canvas.translate(-100, -100);
        paint.setColor(Color.BLUE);
        canvas.drawRect(rect, paint);
```
![](http://upload-images.jianshu.io/upload_images/9028834-84f0c0cac484adde.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
由图可知：
原图：Rect(100, 100, 200, 200)
移动后：Rect(100, 100, 150, 300)
**公式**：
```
原图：(l,  t,  r,  b)

scale (sx,  sy,  px,  py)	
    第①步：translate(px,  py);
	    原点(px,  py)
	    图①(l+px, t+py, r+px, b+py)
    第②步：scale(sx,  sy);
	    原点(px,  py)
	    图②(l*sx+px, t*sy+py, r*sx+px, b*sy+py)
    第③步：translate(-px,  -py);
	    原点(px+(-px)*sx,  py+(-py)*sy)即(px*(1-sx), py*(1-sy))
	    图③(l*sx+px*(1-sx), t*py*(1-sy)+(-py)*sy, r*sx+px*(1-sx), b*sy+py*(1-sy))

移动后：(l*sx+px*(1-sx), t*py*(1-sy)+(-py)*sy, r*sx+px*(1-sx), b*sy+py*(1-sy))
```
**Rt总结**：
**缩放**就是**相对于原点距离**的缩放，
**移动**就是对**原点**进行移动；
视觉坐标是距离原点的位置加上原点的坐标，
**canvas绘画的坐标是相较于原点的坐标**。

### 4、错切（skew）

它的构造函数：
```java
void skew (float sx, float sy)
```

参数说明：
float **sx**:将画布在**x方向上倾斜相应的角度**，sx倾斜角度的**tan值**，
float sy:将画布在y轴方向上倾斜相应的角度，sy为倾斜角度的tan值，

注意，这里全是倾斜角度的tan值，比如我们打算在X轴方向上倾斜30度，tan30=1/√3 约等于 0.56；tan60=根号3，小数对应1.732。

举例（在X轴方向上倾斜45度，tan45=1）：
``` java
        Paint paint = new Paint(Paint.ANTI_ALIAS_FLAG);
        paint.setStyle(Paint.Style.STROKE);
        paint.setStrokeWidth(1);
        Rect rect = new Rect(50, 50, 150, 150);
        paint.setColor(Color.RED);
        canvas.drawRect(rect, paint);

        canvas.skew(1, 0);//skew
        paint.setColor(Color.BLUE);
        canvas.drawRect(rect, paint);
``` 
![](http://upload-images.jianshu.io/upload_images/9028834-3afc737bb82fc575.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



可以从效果图当中看出来，我们设置 x 方向倾斜，反而 y 方向倾斜了，这就是为什么要叫做错切了。你心中一定又会有疑问？每个点的坐标又是怎么计算的呢？那下面我们一起来分析下：


**分析**：
设定在X轴上倾斜45°
![](http://upload-images.jianshu.io/upload_images/9028834-d959190bad0fe7e6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
设定在Y轴上倾斜30°
![](http://upload-images.jianshu.io/upload_images/9028834-356af9273fa4c41e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

```
//公式推导，以在Y轴倾斜为例skew (0, sy)
A点到旧X轴距离AX = 新点A1点到新X轴距离A1X1
XX1=OX * sy
新点A1纵坐标 = A1X1 + XX1
            = AX + OX * sy
即：新点A1纵坐标 = A点的纵坐标 + A点的横坐标*倾斜值
```


>void **skew** (float sx,  float sy)
Preconcat the current matrix with the specified skew.
Parameters
**sx**
float: The amount to skew in X
**sy**
float: The amount to skew in Y

在X轴方向倾斜
>**A 点横坐标**倾斜后的值 = A点的横坐标**+A点的纵坐标*倾斜值** 
B 点横坐标倾斜后的值 = B点的横坐标+A点的纵坐标*倾斜值 
>
**C 点横坐标**倾斜后的值 = C点的横坐标**+C点的纵坐标*倾斜值**
D 点横坐标倾斜后的值 = D点的横坐标+C点的纵坐标*倾斜值


在Y轴方向倾斜
>**A 点纵坐标**倾斜后的值 = A点的纵坐标**+A点的横坐标*倾斜值** 
D 点纵坐标倾斜后的值 = D点的纵坐标+A点的横坐标*倾斜值 
>
**C 点纵坐标**倾斜后的值 = C点的纵坐标**+C点的横坐标*倾斜值**
B 点纵坐标倾斜后的值 = B点的纵坐标+C点的横坐标*倾斜值





在y方向倾斜30度
``` java
        Paint paint = new Paint(Paint.ANTI_ALIAS_FLAG);
        paint.setStyle(Paint.Style.STROKE);
        paint.setStrokeWidth(1);
        Rect rect = new Rect(50, 100, 150, 200);
        paint.setColor(Color.RED);
        canvas.drawRect(rect, paint);

        canvas.skew(0, 0.56f);
        paint.setColor(Color.BLUE);
        canvas.drawRect(rect, paint);
``` 
![](http://upload-images.jianshu.io/upload_images/9028834-a9793eace1d51fdb.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



### 5、裁剪画布（clip系列函数）

裁剪画布是利用Clip系列函数，通过与Rect、Path、Region取交、并、差等集合运算来获得最新的画布形状。除了调用Save、Restore函数以外，这个操作是不可逆的，一但Canvas画布被裁剪，就不能再被恢复！
Clip系列函数如下：
boolean  clipPath(Path path)
boolean  clipPath(Path path, Region.Op op)
boolean  clipRect(Rect rect, Region.Op op)
boolean  clipRect(RectF rect, Region.Op op)
boolean  clipRect(int left, int top, int right, int bottom)
boolean  clipRect(float left, float top, float right, float bottom)
boolean  clipRect(RectF rect)
boolean  clipRect(float left, float top, float right, float bottom, Region.Op op)
boolean  clipRect(Rect rect)
boolean  clipRegion(Region region)
boolean  clipRegion(Region region, Region.Op op)

以上就是根据Rect、Path、Region来取得最新画布的函数，难度都不大，就不再一一讲述。利用ClipRect() 来稍微一讲。

```java
protected void onDraw(Canvas canvas) {
    super.onDraw(canvas);  
      
    canvas.drawColor(Color.RED);  
    canvas.clipRect(new Rect(100, 100, 200, 200));  
    canvas.drawColor(Color.GREEN);  
}   
```

先把背景色整个涂成红色。显示在屏幕上
然后裁切画布，最后最新的画布整个涂成绿色。可见绿色部分，只有一小块，而不再是整个屏幕了。
关于两个画布与屏幕合成，我就不再画图了，跟上面的合成过程是一样的。
![](http://upload-images.jianshu.io/upload_images/9028834-7740082290d1927f?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 6、画布的保存与恢复（save()、restore()）

前面我们讲的所有对画布的操作都是不可逆的，这会造成很多麻烦，比如，我们为了实现一些效果不得不对画布进行操作，但操作完了，画布状态也改变了，这会严重影响到后面的画图操作。如果我们能对画布的大小和状态（旋转角度、扭曲等）进行实时保存和恢复就最好了。
这小节就给大家讲讲画布的保存与恢复相关的函数——Save（）、Restore（）。
```
int save ()
void  restore()
```

这两个函数没有任何的参数，很简单。
**Save**（）：每次调用Save（）函数，都会把**当前的画布**的状态进行**保存**，然后**放入特定的栈中**；
**restore**（）：每当调用Restore（）函数，就会把**栈中最顶层的画布状态取出来**，并按照这个状态恢复当前的画布，并在这个画布上做画。
为了更清晰的显示这两个函数的作用，下面举个例子：

```java
    canvas.drawColor(Color.RED);      
    //保存当前画布大小即整屏  
    canvas.save();   
      
    canvas.clipRect(new Rect(100, 100, 800, 800));  
    canvas.drawColor(Color.GREEN);      
    //恢复整屏画布  
    canvas.restore();
      
    canvas.drawColor(Color.BLUE);  
```

他图像的合成过程为：（最终显示为全屏幕蓝色）
![](http://upload-images.jianshu.io/upload_images/9028834-555377b249062d58.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


下面我通过一个多次利用Save（）、Restore（）来讲述有关保存Canvas画布状态的栈的概念：代码如下：

```java
    canvas.drawColor(Color.RED);  
    //保存的画布大小为全屏幕大小  
    canvas.save();  
      
    canvas.clipRect(new Rect(100, 100, 800, 800));  
    canvas.drawColor(Color.GREEN);  
    //保存画布大小为Rect(100, 100, 800, 800)  
    canvas.save();  
      
    canvas.clipRect(new Rect(200, 200, 700, 700));  
    canvas.drawColor(Color.BLUE);  
    //保存画布大小为Rect(200, 200, 700, 700)  
    canvas.save();  
      
    canvas.clipRect(new Rect(300, 300, 600, 600));  
    canvas.drawColor(Color.BLACK);  
    //保存画布大小为Rect(300, 300, 600, 600)  
    canvas.save();  
      
    canvas.clipRect(new Rect(400, 400, 500, 500));  
    canvas.drawColor(Color.WHITE);  
```

显示效果为：

![](http://upload-images.jianshu.io/upload_images/9028834-7fbbdb0235bae327?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在这段代码中，总共调用了四次Save操作。上面提到过，每调用一次Save（）操作就会将当前的画布状态保存到栈中，所以这四次Save（）所保存的状态的栈的状态如下：

![](http://upload-images.jianshu.io/upload_images/9028834-e8ad6b85daf2b7f0?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

注意在，第四次Save（）之后，我们还对画布进行了canvas.clipRect(new Rect(400, 400, 500, 500));操作，并将当前画布画成白色背景。也就是上图中最小块的白色部分，是最后的当前的画布。

如果，现在使用Restor（），会怎样呢，会把栈顶的画布取出来，当做当前画布的画图，试一下：

```java
canvas.drawColor(Color.RED);  
//保存的画布大小为全屏幕大小  
canvas.save();  
  
canvas.clipRect(new Rect(100, 100, 800, 800));  
canvas.drawColor(Color.GREEN);  
//保存画布大小为Rect(100, 100, 800, 800)  
canvas.save();  
  
canvas.clipRect(new Rect(200, 200, 700, 700));  
canvas.drawColor(Color.BLUE);  
//保存画布大小为Rect(200, 200, 700, 700)  
canvas.save();  
  
canvas.clipRect(new Rect(300, 300, 600, 600));  
canvas.drawColor(Color.BLACK);  
//保存画布大小为Rect(300, 300, 600, 600)  
canvas.save();  
  
canvas.clipRect(new Rect(400, 400, 500, 500));  
canvas.drawColor(Color.WHITE);  
  
//将栈顶的画布状态取出来，作为当前画布，并画成黄色背景  
canvas.restore();  
canvas.drawColor(Color.YELLOW);  
```

上段代码中，把栈顶的画布状态取出来，作为当前画布，然后把当前画布的背景色填充为黄色
![](http://upload-images.jianshu.io/upload_images/9028834-7622fc30b9a8db80?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

那如果我连续Restore（）三次，会怎样呢？
我们先分析一下，然后再看效果：Restore（）三次的话，会连续出栈三次，然后把第三次出来的Canvas状态当做当前画布，也就是Rect(100, 100, 800, 800)，所以如下代码：

```java
    canvas.drawColor(Color.RED);  
    //保存的画布大小为全屏幕大小  
    canvas.save();  
      
    canvas.clipRect(new Rect(100, 100, 800, 800));  
    canvas.drawColor(Color.GREEN);  
    //保存画布大小为Rect(100, 100, 800, 800)  
    canvas.save();  
      
    canvas.clipRect(new Rect(200, 200, 700, 700));  
    canvas.drawColor(Color.BLUE);  
    //保存画布大小为Rect(200, 200, 700, 700)  
    canvas.save();  
      
    canvas.clipRect(new Rect(300, 300, 600, 600));  
    canvas.drawColor(Color.BLACK);  
    //保存画布大小为Rect(300, 300, 600, 600)  
    canvas.save();  
      
    canvas.clipRect(new Rect(400, 400, 500, 500));  
    canvas.drawColor(Color.WHITE);  
      
    //连续出栈三次，将最后一次出栈的Canvas状态作为当前画布，并画成黄色背景  
    canvas.restore();  
    canvas.restore();  
    canvas.restore();  
    canvas.drawColor(Color.YELLOW);  
```

结果为：

![](http://upload-images.jianshu.io/upload_images/9028834-90f44d77a9b5cbe6?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 7、saveLayer
```
public int saveLayer(float left, float top, float right, float bottom, @Nullable Paint paint,
            @Saveflags int saveFlags)
            ...
saveLayerAlpha(float left, float top, float right, float bottom, int alpha,
            @Saveflags int saveFlags)
            ...
```

public int saveLayer(@Nullable RectF bounds, @Nullable Paint paint)
Canvas 在一般的情况下可以看作是一张画布，所有的绘图操作如drawBitmap, drawCircle都发生在这张画布上，这张画板还定义了一些属性比如Matrix，颜色等等。
但是如果需要实现一些相对复杂的绘图操作，比如**多层动画，地图**（地图可以有多个地图层叠加而成，比如：政区层，道路层，兴趣点层）。Canvas提供了**图层（Layer）支持**，缺省情况可以看作是只有一个图层Layer。如果需要按层次来绘图，Android的Canvas可以**使用SaveLayerXXX, Restore 来创建一些中间层**，对于这些Layer是按照“栈结构“来管理的：       

![](http://upload-images.jianshu.io/upload_images/9028834-2ee7eae01c5ca36e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

 **创建**一个新的Layer到“栈”中，可以使用**saveLayer, savaLayerAlpha**；
 从“栈”中**推出**一个Layer，可以使用**restore,restoreToCount**。
 但Layer入栈时，后续的DrawXXX操作都发生在这个Layer上，而Layer退栈时，就会把本层绘制的图像“绘制”到上层或是Canvas上。
 在复制Layer到Canvas上时，可以指定**Layer的透明度**(Layer），这是在**创建Layer时指定**的：public int saveLayerAlpha(RectF bounds, int alpha, int saveFlags)

本例Layers 介绍了图层的基本用法：Canvas可以看做是由两个图层（Layer）构成的，为了更好的说明问题，我们将代码稍微修改一下，缺省图层绘制一个红色的圆，在新的图层画一个蓝色的圆，新图层的透明度为0×88。
public class Layers extends Activity {  
  

```java
@Override
protected void onDraw(Canvas canvas) {
    super.onDraw(canvas);
    Paint mPaint = new Paint();
    mPaint.setAntiAlias(true);

    canvas.drawColor(Color.RED);

    canvas.saveLayerAlpha(0, 0, 300, 300, 0x88, Canvas.ALL_SAVE_FLAG);//图层1
    canvas.drawColor(Color.BLUE);

    canvas.restore();//下面两张图，图1注释掉词句，图2没注释掉
    canvas.saveLayerAlpha(100, 100, 400, 400, 0xff, Canvas.ALL_SAVE_FLAG);//图层2
    canvas.drawColor(Color.YELLOW);
}
```
没有canvas.restore();，所以图层2是基于图层1作画，黄色还是半透明
![](http://upload-images.jianshu.io/upload_images/9028834-59e45e22bbb77212.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
运行了一次canvas.restore();，去掉栈顶一个图层即图层1露出画布，所以图层2是直接在画布上作画，黄色不透明
![](http://upload-images.jianshu.io/upload_images/9028834-686304f880bd8309.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)





#### **saveFlags**

上面我们只是粗暴的使用了ALL_SAVE_FLAG来保存的所有的信息，但是实际使用中，所有信息都保存必然增加了开销，所以，我们应该根据需要的动作，尽量的精确的保存少量的信息。这里就需要了解各个flag的意义。

首先需要知道的是，使用flag的方法除了saveFlayer还有save方法，他们都可以使用flag来指定需要保存的信息。那么来看看6中flag所对应的意义：

| Flag | 意义 | 适用方法 |
| --- | :-: | --- |
| MATRIX_SAVE_FLAG | 只保存图层的matrix矩阵 | save，saveLayer |
| CLIP_SAVE_FLAG | 只保存大小信息 | save，saveLayer |
| HAS_ALPHA_LAYER_SAVE_FLAG | 表明该图层有透明度，和下面的标识冲突，都设置时以下面的标志为准 | saveLayer |
| FULL_COLOR_LAYER_SAVE_FLAG | 完全保留该图层颜色（和上一图层合并时，清空上一图层的重叠区域，保留该图层的颜色） | saveLayer |
| CLIP_TO_LAYER_SAVE_ | 创建图层时，会把canvas（所有图层）裁剪到参数指定的范围，如果省略这个flag将导致图层开销巨大（实际上图层没有裁剪，与原图层一样大） |  |
| ALL_SAVE_FLAG | 保存所有信息 | save，saveLayer |

##### **(1) MATRIX_SAVE_FLAG**

只保存图层的matrix矩阵。 
canvas中的哪些方法是利用matrix完成的，这里需要明确，其实我们知道，canvas的绘制，最终是发生在bitmap上的，从canvas的构造函数中也可以看出。

在Bitmap的构造函数中可以看出bitmap的操作也是通过matrix来进行的：
```java
Bitmap createBitmap(Bitmap source, int x, int y, int width, int height,Matrix m, boolean filter)
```

那么我们可以知道canvas的canvas.translate(平移)、canvas.rotate（旋转）、canvas.scale（缩放）、canvas.skew（扭曲）其实都是通过matrix来达到的，这一点可以在代码中使用MATRIX_SAVE_FLAG来进行验证。

###### **save方法**

这里举例平移：
```java
paint.setColor(Color.BLUE);
canvas.save(Canvas.MATRIX_SAVE_FLAG);
canvas.translate(200, 200);
canvas.drawRect(100, 100, 300, 300, paint);
canvas.restore();

paint.setColor(Color.RED);
canvas.drawRect(100, 100, 300, 300, paint);
```
![](http://upload-images.jianshu.io/upload_images/9028834-1c975bc14f7e2b69?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到平移效果得到了保存,并且可以恢复。

###### **saveLayer方法**
```java
paint.setColor(Color.BLUE);
int count=canvas.saveLayer(0,0,1000,1000,paint,Canvas.MATRIX_SAVE_FLAG|Canvas.HAS_ALPHA_LAYER_SAVE_FLAG);
canvas.translate(200, 200);
canvas.drawRect(100, 100, 300, 300, paint);
canvas.restoreToCount(count);

paint.setColor(Color.RED);
canvas.drawRect(100, 100, 300, 300, paint);
```
![](http://upload-images.jianshu.io/upload_images/9028834-23c21709123d7c7e?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
图中看出效果相同

如果这里不使用MATRIX_SAVE_FLAG标志位，那么是否会出现不同的效果呢，使用CLIP_SAVE_FLAG标志来试试：
```java
paint.setColor(Color.BLUE);
int count=canvas.saveLayer(0,0,1000,1000,paint,Canvas.CLIP_SAVE_FLAG|Canvas.HAS_ALPHA_LAYER_SAVE_FLAG);
canvas.translate(200, 200);
canvas.drawRect(100, 100, 300, 300, paint);
canvas.restoreToCount(count);

paint.setColor(Color.RED);
canvas.drawRect(100, 100, 300, 300, paint);
```
![](http://upload-images.jianshu.io/upload_images/9028834-a337642275e6742f?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)





代码和上面基本相同，只是标志位改变了，这里可以看到两个图重叠了，也就是说CLIP_SAVE_FLAG标志位并没有保存相关的位移信息，导致restore的时候没能恢复。

##### **(2) CLIP_SAVE_FLAG**

看了上面的MATRIX_SAVE_FLAG，这里的意义基本知道，主要就是**保存裁剪相关的信息**。 
由于和上面的示例基本类似，这里就不再做讲解了。

##### **(3) FULL_COLOR_LAYER_SAVE_FLAG 和 HAS_ALPHA_LAYER_SAVE_FLAG**

这两个方法是saveLayer专用的方法，HAS_ALPHA_LAYER_SAVE_FLAG为layer添加一个透明通道，这样一来没有绘制的地方就是透明的，覆盖到上一个layer的时候，就会显示出上一层的图像。而FULL_COLOR_LAYER_SAVE_FLAG 则会完全展示当前layer的图像，清除掉上一层的重合图像。

来看看FULL_COLOR_LAYER_SAVE_FLAG 的示例：
```java
canvas.drawColor(Color.RED);

canvas.saveLayer(200,200,700,700,mPaint,Canvas.FULL_COLOR_LAYER_SAVE_FLAG);
mPaint.setColor(Color.GREEN);
canvas.drawRect(300,300,600,600,mPaint);
canvas.restore();
```

![](http://upload-images.jianshu.io/upload_images/9028834-9147bd75d101bfc0?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



可以看到，绿色的方块周围有白色的一圈，整个白色加上绿色区域是这个layer的区域，由于这里使用了**FULL_COLOR_LAYER_SAVE_FLAG**标志，所以这块区域的红色被layer层完全覆盖（即使是透明），由于绿色周围的颜色是透明的，所以在**清除了红色并覆盖后，就显示出了activity的背景颜色**，所以显示了白色。

如果activity背景是黑色，这一块自然变为黑色：

![](http://upload-images.jianshu.io/upload_images/9028834-d859b7c301db957c?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

那么其他代码不变，只是将标志位替换成**HAS_ALPHA_LAYER_SAVE_FLAG**会发生什么：

![](http://upload-images.jianshu.io/upload_images/9028834-819b7bda45280461?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到，绿色周围的白色不见了，可见，这就是区别。使用这个标志位不会清空上一图层的内容。

##### **(4) CLIP_TO_LAYER_SAVE_**

这个标志比较重要，官方的建议是，最好不要忽略这个标识，这个标识如果不设置将会带来很大的性能问题。

这个标识的作用是将canvas裁剪到指定的大小，并且无法回复。看下面一个例子：
```java
canvas.drawColor(Color.RED);
canvas.saveLayer(200,200,700,700,mPaint,Canvas.CLIP_TO_LAYER_SAVE_FLAG);
canvas.drawColor(Color.GREEN);
canvas.restore();
canvas.drawColor(Color.BLACK);
```
![](http://upload-images.jianshu.io/upload_images/9028834-77a36245e6e9c399?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



这里看，先将底色绘制为红色，然后开启新图层，再绘制为绿色，最后将canvas绘制为黑色，为什么最后不是全屏黑色呢，这里明明restore了，这是因为使用了**CLIP_TO_LAYER_SAVE_FLAG**标志，这样一来，**canvas被裁剪了**，并且**无法回复**了。这样也就减少了处理的区域，增加了性能。



## 习题

### 画一个表盘
![image](http://upload-images.jianshu.io/upload_images/9028834-2dc44b40d770b3a0?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
闹钟表盘其实就是在一个圆周上绘制；

既然是圆周，最简单的方式莫过于在闹钟的12点钟处划线，通过canvas的旋转绘制到对应圆周处，我们一起实现一下：

整个圆周是360 度，每隔 30 度为一个整时间刻度，整刻度与刻度之间有四个短刻度，划分出5个小段，每个段为6度，有了这些分析，我们则可以采用如下代码进行绘制：
```java
    /* 绘制刻度 */
    private void drawLines(Canvas canvas) {
        for (int degree = 0; degree <= 360; degree++) {
            if (degree % 30 == 0) {
                //时针
                mLineBottom = mLineTop + mHourLineHeight;
                mLinePaint.setStrokeWidth(mHourLineWidth);
            } else {
                mLineBottom = mLineTop + mMinuteLineHeight;
                mLinePaint.setStrokeWidth(mMinuteLineWidth);
            }

            if (degree % 6 == 0) {
                canvas.save();
                canvas.rotate(degree, mCenterX, mCenterY);
                canvas.drawLine(mLineLeft, mLineTop, mLineLeft, mLineBottom, mLinePaint);
                canvas.restore();
            }
        }
    }
```

整体代码如下：
```java
/* 表盘 */
public class Dial extends View {
    private static final int HOUR_LINE_HEIGHT = 35;
    private static final int MINUTE_LINE_HEIGHT = 25;
    private Paint mCirclePaint, mLinePaint;
    private DrawFilter mDrawFilter;
    //圆心（表盘中心）
    private int mCenterX, mCenterY, mCenterRadius;

    // 圆环线宽度
    private int mCircleLineWidth;
    // 直线刻度线宽度
    private int mHourLineWidth, mMinuteLineWidth;
    // 时针长度
    private int mHourLineHeight;
    // 分针长度
    private int mMinuteLineHeight;
    // 刻度线的左、上位置
    private int mLineLeft, mLineTop;

    // 刻度线的下边位置
    private int mLineBottom;
    // 用于控制刻度线位置
    private int mFixLineHeight;

    public Dial(Context context) {
        this(context, null);
    }

    public Dial(Context context, @Nullable AttributeSet attrs) {
        this(context, attrs, 0);
    }

    public Dial(Context context, @Nullable AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        mDrawFilter = new PaintFlagsDrawFilter(0, Paint.ANTI_ALIAS_FLAG
                | Paint.FILTER_BITMAP_FLAG);

        mCircleLineWidth = (int) TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_DIP, 8,
                getResources().getDisplayMetrics());
        mHourLineWidth = (int) TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_DIP, 4,
                getResources().getDisplayMetrics());
        mMinuteLineWidth = mHourLineWidth / 2;

        mFixLineHeight = (int) TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_DIP, 4,
                getResources().getDisplayMetrics());

        mHourLineHeight = (int) TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_DIP,
                HOUR_LINE_HEIGHT,
                getResources().getDisplayMetrics());
        mMinuteLineHeight = (int) TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_DIP,
                MINUTE_LINE_HEIGHT,
                getResources().getDisplayMetrics());
        initPaint();
    }

    private void initPaint() {
        mCirclePaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        mCirclePaint.setColor(Color.RED);
        mCirclePaint.setStyle(Paint.Style.STROKE);
        mCirclePaint.setStrokeWidth(mCircleLineWidth);

        mLinePaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        mLinePaint.setColor(Color.RED);
        mLinePaint.setStyle(Paint.Style.FILL_AND_STROKE);
        mLinePaint.setStrokeWidth(mHourLineWidth);
    }

    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        super.onMeasure(widthMeasureSpec, heightMeasureSpec);
    }

    @Override
    protected void onDraw(Canvas canvas) {
        canvas.setDrawFilter(mDrawFilter);
        super.onDraw(canvas);
        // 绘制表盘
        drawCircle(canvas);
        // 绘制刻度
        drawLines(canvas);
    }

    /* 绘制刻度 */
    private void drawLines(Canvas canvas) {
        for (int degree = 0; degree <= 360; degree++) {
            if (degree % 30 == 0) {
                //时针
                mLineBottom = mLineTop + mHourLineHeight;
                mLinePaint.setStrokeWidth(mHourLineWidth);
            } else {
                mLineBottom = mLineTop + mMinuteLineHeight;
                mLinePaint.setStrokeWidth(mMinuteLineWidth);
            }

            if (degree % 6 == 0) {
                canvas.save();
                canvas.rotate(degree, mCenterX, mCenterY);
                canvas.drawLine(mLineLeft, mLineTop, mLineLeft, mLineBottom, mLinePaint);
                canvas.restore();
            }
        }
    }

    /* 绘制表盘 */
    private void drawCircle(Canvas canvas) {
        canvas.drawCircle(mCenterX, mCenterY, mCenterRadius, mCirclePaint);
    }

    @Override
    protected void onSizeChanged(int w, int h, int oldw, int oldh) {
        super.onSizeChanged(w, h, oldw, oldh);
        mCenterX = w / 2;
        mCenterY = h / 2;
        mCenterRadius = Math.min(mCenterX, mCenterY) - mCircleLineWidth / 2;

        mLineLeft = mCenterX - mMinuteLineWidth / 2;
        mLineTop = mCenterY - mCenterRadius;
    }
}
```

### 画一个正方形螺旋图
![](http://upload-images.jianshu.io/upload_images/9028834-09b4a8f8c70844a1?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

思路非常的简单：
1\. 绘制一个和屏幕等宽的正方形；
2\. 将画布以正方形中心为基准点进行缩放；
3\. 在缩放的过程中绘制原正方形；

注：每次绘制都得使用canvas.save()  和 canvas.restore()进行画布的锁定和回滚，以免除对后面绘制的影响。

先初始化画笔，注意此时画笔需要设置成空心：

```java
protected void onDraw(Canvas canvas) {
    super.onDraw(canvas);
    Paint paint = new Paint(Paint.ANTI_ALIAS_FLAG);
    paint.setStyle(Paint.Style.STROKE);
    paint.setStrokeWidth(8);
    int l = 10;
    int t = 10;
    int r = 410;
    int b = 410;
    int space = 30;
    Rect squareRect = new Rect(l, t, r, b);
    int squareCount = (r - l) / space;
    float px = l + (r - l) / 2;
    float py = t + (b - t) / 2;
    for (int i = 0; i < squareCount; i++) {
        // 保存画布
        canvas.save();
        float fraction = (float) i / squareCount;
        // 将画布以正方形中心进行缩放
        canvas.scale(fraction, fraction, px, py);
        canvas.drawRect(squareRect, paint);
        // 画布回滚
        canvas.restore();
    }
}
```

一起来看下绘制的效果：
![](http://upload-images.jianshu.io/upload_images/9028834-2df65c66756dd523.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)







## 引用：
**[自定义控件之绘图篇（四）：canvas变换与操作](http://blog.csdn.net/harvic880925/article/details/39080931)**
[自定义View之绘图篇（六）：Canvas那些你应该知道的变换](http://blog.csdn.net/u012551350/article/details/51858309)
[Canvas之translate、scale、rotate、skew方法讲解！](http://blog.csdn.net/tianjian4592/article/details/45234419)
[Android 2D Graphics学习（二）、Canvas篇1、Canvas基本使用](http://blog.csdn.net/lonelyroamer/article/details/8264189)
[android canvas layer (图层)详解与进阶](http://blog.csdn.net/cquwentao/article/details/51423371)



# 综合习题
## 实现图片圆角带边框的效果
![](http://upload-images.jianshu.io/upload_images/9028834-81ea1e0198a7cf52.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

``` java
@Override
protected void onDraw(Canvas canvas) {
    super.onDraw(canvas);
    Bitmap rawBitmap = BitmapFactory.decodeResource(getResources(), R.mipmap.cat);

    Bitmap bitmap = getRoundCornerBitmap(rawBitmap, 50);
    canvas.drawBitmap(bitmap, 0, 0, new Paint());
}

/**
 * @param bitmap 原图
 * @param pixels 圆角大小
 * @return
 */
public Bitmap getRoundCornerBitmap(Bitmap bitmap, float pixels) {
//获取bitmap的宽高
    int width = bitmap.getWidth();
    int height = bitmap.getHeight();

    Bitmap cornerBitmap = Bitmap.createBitmap(width, height, Bitmap.Config.ARGB_8888);
    Paint paint = new Paint();
    Canvas canvas = new Canvas(cornerBitmap);
    paint.setAntiAlias(true);

    canvas.drawRoundRect(new RectF(0, 0, width, height), pixels, pixels, paint);
    paint.setXfermode(new PorterDuffXfermode(PorterDuff.Mode.SRC_IN));
    canvas.drawBitmap(bitmap, null, new RectF(0, 0, width, height), paint);

//绘制边框
    paint.setStyle(Paint.Style.STROKE);
    paint.setStrokeWidth(6);
    paint.setColor(Color.GREEN);
    canvas.drawRoundRect(new RectF(0, 0, width, height), pixels, pixels, paint);

    return cornerBitmap;
}
``` 

1、首先通过`Bitmap cornerBitmap = Bitmap.createBitmap(width, height, Bitmap.Config.ARGB_8888);`生成`cornerBitmap` 实例，注意了`Bitmap` 只能通过静态方法来获取它的实例，并不能直接 new出来。

2、绘制圆角矩形`canvas.drawRoundRect(new RectF(0, 0, width, height), pixels, pixels, paint);`。

3、为`Paint`设置`PorterDuffXfermode`。参数 `PorterDuff.Mode.SRC_IN` 取交集。

4、绘制原图。`canvas.drawBitmap(bitmap, null, new RectF(0, 0, width, height), paint);`

5、绘制边框圆角。 `canvas.drawRoundRect(new RectF(0, 0, width, height), pixels, pixels, paint);`

### 引用：
[自定义View之绘图篇（六）：Canvas那些你应该知道的变换](http://blog.csdn.net/u012551350/article/details/51858309)


**相关文章：**

[《Android自定义控件三部曲文章索引》](http://blog.csdn.net/harvic880925/article/details/50995268): [http://blog.csdn.net/harvic880925/article/details/50995268](http://blog.csdn.net/harvic880925/article/details/50995268)


】



【Android 动画】
【
# 【Android 动画】

# 动画分类

Android 中动画分为两种，一种是 Tween 动画、还有一种是 Frame 动画。

## 补间动画(Tween动画)

这种实现方式可以使视图组件**移动、放大、缩小以及产生透明度**的变化；

## 帧动画(Frame 动画)

传统的动画方法，通过顺序的**播放排列好的图片**来实现，类似电影、gif。

## 属性动画(Property animation)

自Android 3.0版本开始，系统给我们提供了一种全新的动画模式，属性动画(property animation)，它的功能非常强大，弥补了之前补间动画的一些缺陷，几乎是可以完全替代掉补间动画了。

# 补间动画(Tween动画)

## 1、透明度动画AlphaAnimation

```java
	// 透明度动画
	public void alpha(View v) {
		AlphaAnimation alphaAnimation = new AlphaAnimation(0, 1);
/*		AlphaAnimation (float fromAlpha, float toAlpha)
		fromAlpha: 动画的起始alpha值 （范围：0:完全透明 -1:完全不透明）
		toAlpha：终止的值，动画结束的值	*/		
		alphaAnimation.setDuration(3000);// 每次动画持续时间3秒
		alphaAnimation.setFillAfter(true);// 动画最后是否停留在终止的状态
		alphaAnimation.setRepeatCount(3);// 动画重复的次数
		alphaAnimation.setRepeatMode(Animation.REVERSE);// REVERSE： 反转模式
														// RESTART：重新开始
		// 动画监听
		alphaAnimation.setAnimationListener(new AnimationListener() {

			@Override
			public void onAnimationStart(Animation animation) {
				System.out.println("动画开始回调");
			}

			@Override
			public void onAnimationRepeat(Animation animation) {
				System.out.println("动画重复回调");

			}

			@Override
			public void onAnimationEnd(Animation animation) {
				System.out.println("动画结束回调");
				Toast.makeText(getApplicationContext(), "动画结束，进入陌陌关心你界面",
						Toast.LENGTH_LONG).show();

			}
		});
		iconIv.startAnimation(alphaAnimation);
	}
```

## 2、平移动画TranslateAnimation

```java
/* TranslateAnimation (int fromXType, 
                float fromXValue, 
                int toXType, 
                float toXValue, 
                int fromYType, 
                float fromYValue, 
                int toYType, 
                float toYValue)
	原点：控件第一次绘制的左上角的坐标点
	fromXType（起点，相对于原点偏移方式）：
			Animation.ABSOLUTE 绝对值，像素值
			Animation.RELATIVE_TO_SELF 相对于自己
			Animation.RELATIVE_TO_PARENT 相对于父控件.
	fromXValue（起点，相对于原点偏移量）：
			绝对值/百分比		
*/		
		TranslateAnimation translateAnimation = new TranslateAnimation(
				Animation.ABSOLUTE,
				iconIv.getWidth(), // 当前屏幕密度 :240 标准的屏幕密度：160 则dp转px ：
									// px=dp*240/160
				Animation.ABSOLUTE, iconIv.getWidth(), 
				Animation.ABSOLUTE, 0,
				Animation.RELATIVE_TO_SELF, 1);
		translateAnimation.setDuration(3000);// 每次动画持续时间3秒
		translateAnimation.setFillAfter(true);// 动画最后停留在终止的状态
		translateAnimation.setRepeatCount(3);// 动画重复的次数
		translateAnimation.setRepeatMode(Animation.REVERSE);// REVERSE： 反转模式
															// RESTART：重新开始
		translateAnimation.setInterpolator(new BounceInterpolator());// 设置特效，弹簧效果
		iconIv.startAnimation(translateAnimation);
		System.out.println("控件的宽度" + iconIv.getWidth());
	}
```

## 3、缩放动画ScaleAnimation

```java
/* ScaleAnimation (float fromX, 
                float toX, 
                float fromY, 
                float toY, 
                int pivotXType, 
                float pivotXValue, 
                int pivotYType, 
                float pivotYValue)
	fromX: 缩放起始比例-水平方向
	toX: 缩放最终比例-水平方向
	pivotXType（中心点相较于原点 x方向的类型）: 
			Animation.ABSOLUTE
			Animation.RELATIVE_TO_SELF
			RELATIVE_TO_PARENT.
	pivotXValue: 绝对值/百分比	
*/
	public void scale(View v) {
		ScaleAnimation scaleAnimation =new ScaleAnimation
				(0, 2, 0, 2, Animation.RELATIVE_TO_SELF, 0.5f, Animation.RELATIVE_TO_SELF, 0.5f);
		scaleAnimation.setDuration(3000);// 每次动画持续时间3秒
		scaleAnimation.setFillAfter(true);// 动画最后停留在终止的状态
		scaleAnimation.setRepeatCount(1);// 动画重复的次数
		iconIv.startAnimation(scaleAnimation);

	}

```

## 4、旋转动画 rotate

### 创建一个Animation类型的XML文件：

```xml
<?xml version="1.0" encoding="utf-8"?>
<rotate xmlns:android="http://schemas.android.com/apk/res/android"
    android:fromDegrees="0"
    android:toDegrees="180"
    android:duration="3000"
    android:interpolator="@android:anim/overshoot_interpolator"
    android:fillAfter="true"
    android:repeatCount="2"
    android:repeatMode="reverse"
    android:pivotX="50%"
    android:pivotY="50%"
    >
    <!--fromDegrees:起始的度数
      toDegrees：终止的度数
      infinite：无限次数 
      起始度数大于终止度数，则能逆时针旋转，否则顺时针
      android:pivotX="50%":旋转围绕的轴心，x方向位置，相对于自己的宽度的一半
      android:pivotX="50%p":相对于父控件宽度的一半
      -->
</rotate>
```

### java中导入xml动画

```java
Animation animation1 = AnimationUtils.loadAnimation(this,R.anim.rotate);
imageView.startAnimation(animation1);
```

## 复合动画

```java
AnimationSet animationSet=new AnimationSet(false);
animationSet.addAnimation(animation1);
//这样在这里面添加就可以了；		
Animation rotateAnimation = AnimationUtils.loadAnimation(this, R.anim.rotate);
animationSet.addAnimation(rotateAnimation);
```

# 帧动画(Frame 动画)

## 方式一；使用XML的方式；

1、创建一个AnimationDrawable 的ＸＭＬ文件；

```xml
<?xml version="1.0" encoding="utf-8"?>
<animation-list xmlns:android="http://schemas.android.com/apk/res/android" android:oneshot="false" >//这个是反复执行的设置；
     <item android:drawable="@drawable/emoji_088" android:duration="150" />
    <item android:drawable="@drawable/emoji_095" android:duration="150" />
    <item android:drawable="@drawable/emoji_096" android:duration="150" />
    <item android:drawable="@drawable/emoji_097" android:duration="150" />
    <item android:drawable="@drawable/emoji_098" android:duration="150" />
    <item android:drawable="@drawable/emoji_099" android:duration="150" />
    <item android:drawable="@drawable/emoji_100" android:duration="150" />
    <item android:drawable="@drawable/emoji_101" android:duration="150" />
    <item android:drawable="@drawable/emoji_102" android:duration="50" />
    <item android:drawable="@drawable/emoji_103" android:duration="50" />
    <item android:drawable="@drawable/emoji_104" android:duration="50" />
</animation-list>
<!--android:drawable[drawable]//加载Drawable对象
    android:duration[long]//每一帧动画的持续时间（单位ms）
    android:oneshot[boolean]//动画是否只运行一次，true运行一次，false重复运行
    android:visible[boolean]//Drawable对象的初始能见度状态，true可见，false不可见（默认为false）-->
```

２、第二步就是需要将这个帧动画设置到一个容器中去；imageview;
 android:src="@drawable/animation" />

3、可以从控件中获取到这个帧动画的图片然后再对他进行操作；

```java
drawable = (AnimationDrawable) imageView.getDrawable();

if (drawable.isRunning()) {
			drawable.stop();
		}else{
			drawable.start();
		}
```

## 方式二：使用代码的方式进行；

方式1：添加多个帧 Drawable

```java
		mAnimationDrawable = new AnimationDrawable();
		mAnimationDrawable.setOneShot(false);//是否执行一次
		//添加多个帧 Drawable
		for(int i=0;i<11;i++){
			Drawable frame = getResources().getDrawable(R.drawable.girl_1+i);
			mAnimationDrawable.addFrame(frame, 200);
		}

		mImageView.setImageDrawable(mAnimationDrawable);//设置帧动画，默认是停止状态

```

方式2：引用xml 帧动画drawable

```java
// 引用xml 帧动画drawable
mAnimationDrawable=(AnimationDrawable) getResources().getDrawable(R.drawable.frame);
mImageView.setImageDrawable(mAnimationDrawable);
```

# 属性动画(Property animation)

## 为什么要引入属性动画？

Android之前的补间动画机制其实还算是比较健全的，在android.view.animation包下面有好多的类可以供我们操作，来完成一系列的动画效果，比如说对View进行移动、缩放、旋转和淡入淡出，并且我们还可以借助AnimationSet来将这些动画效果组合起来使用，除此之外还可以通过配置Interpolator来控制动画的播放速度等等等等。那么这里大家可能要产生疑问了，既然之前的动画机制已经这么健全了，为什么还要引入属性动画呢？

其实上面所谓的健全都是相对的，如果你的需求中只需要对View进行移动、缩放、旋转和淡入淡出操作，那么补间动画确实已经足够健全了。但是很显然，这些功能是不足以覆盖所有的场景的，一旦我们的需求超出了移动、缩放、旋转和淡入淡出这四种对View的操作，那么补间动画就不能再帮我们忙了，也就是说它在功能和可扩展方面都有相当大的局限性，那么下面我们就来看看补间动画所不能胜任的场景。

注意上面我在介绍补间动画的时候都有使用“对View进行操作”这样的描述，没错，**补间动画是只能够作用在View上**的。也就是说，我们可以对一个Button、TextView、甚至是LinearLayout、或者其它任何继承自View的组件进行动画操作，但是如果我们想要对一个非View的对象进行动画操作，抱歉，补间动画就帮不上忙了。可能有的朋友会感到不能理解，我怎么会需要对一个非View的对象进行动画操作呢？这里我举一个简单的例子，比如说我们有一个自定义的View，在这个View当中有一个Point对象用于管理坐标，然后在onDraw()方法当中就是根据这个Point对象的坐标值来进行绘制的。也就是说，如果我们可以对Point对象进行动画操作，那么整个自定义View的动画效果就有了。显然，补间动画是不具备这个功能的，这是它的第一个缺陷。

然后**补间动画**还有一个缺陷，就是它**只能够实现移动、缩放、旋转和淡入淡出这四种动画操作**，那如果我们希望可以对View的背景色进行动态地改变呢？很遗憾，我们只能靠自己去实现了。说白了，之前的补间动画机制就是使用硬编码的方式来完成的，功能限定死就是这些，基本上没有任何扩展性可言。

最后，**补间动画**还有一个致命的缺陷，就是它**只是改变了View的显示效果而已，而不会真正去改变View的属性**。什么意思呢？比如说，现在屏幕的左上角有一个按钮，然后我们通过补间动画将它移动到了屏幕的右下角，现在你可以去尝试点击一下这个按钮，点击事件是绝对不会触发的，因为实际上这个按钮还是停留在屏幕的左上角，只不过补间动画将这个按钮绘制到了屏幕的右下角而已。

也正是因为这些原因，Android开发团队决定在3.0版本当中引入属性动画这个功能，那么属性动画是不是就把上述的问题全部解决掉了？下面我们就来一起看一看。

新引入的属性动画机制已经不再是针对于View来设计的了，也不限定于只能实现移动、缩放、旋转和淡入淡出这几种动画操作，同时也不再只是一种视觉上的动画效果了。它实际上是一种不断地对值进行操作的机制，并将值赋值到指定对象的指定属性上，可以是任意对象的任意属性。所以我们仍然可以将一个View进行移动或者缩放，但同时也可以对自定义View中的Point对象进行动画操作了。我们只需要告诉系统动画的运行时长，需要执行哪种类型的动画，以及动画的初始值和结束值，剩下的工作就可以全部交给系统去完成了。

既然属性动画的实现机制是通过对目标对象进行赋值并修改其属性来实现的，那么之前所说的按钮显示的问题也就不复存在了，如果我们通过属性动画来移动一个按钮，那么这个按钮就是真正的移动了，而不再是仅仅在另外一个位置绘制了而已。

## ValueAnimator

ValueAnimator是整个属性动画机制当中最核心的一个类，前面我们已经提到了，**属性动画的运行机制是通过不断地对值进行操作来实现的**，而初始值和结束值之间的**动画过渡**就是**由ValueAnimator**这个类来负责**计算**的。
它的内部使用一种时间循环的机制来计算值与值之间的动画过渡，我们只需要将初始值和结束值提供给ValueAnimator，并且告诉它动画所需运行的**时长**，那么ValueAnimator就会自动帮我们完成从初始值平滑地过渡到结束值这样的效果。除此之外，ValueAnimator还负责管理动画的播放**次数**、播放**模式**、以及**对动画设置监听器**等，确实是一个非常重要的类。
详细见下文【属性动画之ObjectAnimator】


## ObjectAnimator

**对任意对象的任意属性进行动画操作**

相比于ValueAnimator，ObjectAnimator可能才是我们最常接触到的类，因为ValueAnimator只不过是对值进行了一个平滑的动画过渡，但我们实际使用到这种功能的场景好像并不多。而ObjectAnimator则就不同了，它是可以直接任意对象的任意属性进行动画操作的，比如说View的alpha属性。

不过虽说ObjectAnimator会更加常用一些，但是它其实是**继承自ValueAnimator**的，底层的动画实现机制也是基于ValueAnimator来完成的，因此ValueAnimator仍然是整个属性动画当中最核心的一个类。
详细见下文【属性动画之ObjectAnimator】


## 组合动画-AnimatorSet

实现组合动画功能主要需要借助**AnimatorSet**这个类，这个类提供了一个**play()**方法，如果我们向这个方法中传入一个Animator对象(ValueAnimator或ObjectAnimator)将会返回一个AnimatorSet.Builder的实例。

### AnimatorSet.Builder

AnimatorSet.Builder中包括以下四个方法：
**after(Animator anim)**   将现有动画插入到传入的动画之后执行
**after(long delay)**   将现有动画延迟指定毫秒后执行
**before(Animator anim)**   将现有动画插入到传入的动画之前执行
**with(Animator anim)**   将现有动画和传入的动画同时执行

好的，有了这四个方法，我们就可以完成组合动画的逻辑了，那么比如说我们想要让TextView先从屏幕外移动进屏幕，然后开始旋转360度，旋转的同时进行淡入淡出操作，就可以这样写：

```java
ObjectAnimator moveIn = ObjectAnimator.ofFloat(textview, "translationX", -500f, 0f);  
ObjectAnimator rotate = ObjectAnimator.ofFloat(textview, "rotation", 0f, 360f);  
ObjectAnimator fadeInOut = ObjectAnimator.ofFloat(textview, "alpha", 1f, 0f, 1f);  
AnimatorSet animSet = new AnimatorSet();  
animSet.play(rotate).with(fadeInOut).after(moveIn);  
animSet.setDuration(5000);  
animSet.start();  
```

可以看到，这里我们先是把三个动画的对象全部创建出来，然后new出一个AnimatorSet对象之后将这三个动画对象进行播放排序，让旋转和淡入淡出动画同时进行，并把它们插入到了平移动画的后面，最后是设置动画时长以及启动动画。运行一下上述代码，效果如下图所示：
![](http://img.blog.csdn.net/20171221150531181?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

## Animator监听器

### AnimatorListener

Animator类当中提供了一个**addListener()**方法，这个方法接收一个AnimatorListener，我们只需要去实现这个**AnimatorListener**就可以监听动画的各种事件了。

大家已经知道，ObjectAnimator是继承自ValueAnimator的，而ValueAnimator又是继承自Animator的，因此不管是ValueAnimator还是ObjectAnimator都是可以使用addListener()这个方法的。另外AnimatorSet也是继承自Animator的，因此addListener()这个方法算是个通用的方法。

添加一个监听器的代码如下所示：

```java
Anim.addListener(new AnimatorListener() {  
    @Override  
    public void onAnimationStart(Animator animation) {  
	    //动画开始的时候调用
    }  
  
    @Override  
    public void onAnimationRepeat(Animator animation) {  
	    //动画重复执行的时候调用
    }  
  
    @Override  
    public void onAnimationEnd(Animator animation) {  
	    //动画结束的时候调用
    }  
  
    @Override  
    public void onAnimationCancel(Animator animation) {  
	    //动画被取消的时候调用
    }  
});  
```

可以看到，我们需要实现接口中的四个方法，onAnimationStart()方法会在动画开始的时候调用，onAnimationRepeat()方法会在动画重复执行的时候调用，onAnimationEnd()方法会在动画结束的时候调用，onAnimationCancel()方法会在动画被取消的时候调用。

### AnimatorListenerAdapter

但是也许很多时候我们并不想要监听那么多个事件，可能我只想要监听动画结束这一个事件，那么每次都要将四个接口全部实现一遍就显得非常繁琐。没关系，为此Android提供了一个适配器类，叫作AnimatorListenerAdapter，使用这个类就可以**解决掉实现接口繁琐的问题**了，如下所示：

```java
Anim.addListener(new AnimatorListenerAdapter() {  
});  
```

这里我们向addListener()方法中传入这个适配器对象，由于**AnimatorListenerAdapter中已经将每个接口都实现好了**，所以这里不用实现任何一个方法也不会报错。那么如果我想监听动画结束这个事件，就只需要**单独重写**这一个**方法**就可以了，如下所示：

```java
Anim.addListener(new AnimatorListenerAdapter() {  
    @Override  
    public void onAnimationEnd(Animator animation) {  
    }  
});  
```

## 使用XML编写动画

我们可以使用代码来编写所有的动画功能，这也是最常用的一种做法。不过，过去的补间动画除了使用代码编写之外也是可以使用XML编写的，因此属性动画也提供了这一功能，即通过XML来完成和代码一样的属性动画功能。

通过XML来编写动画可能会比通过代码来编写动画要慢一些，但是在**重用**方面将会变得非常**轻松**，比如某个将通用的动画编写到XML里面，我们就可以在各个界面当中轻松去重用它。

如果想要使用XML来编写动画，首先要在res目录下面新建一个animator文件夹，所有属性动画的XML文件都应该存放在这个文件夹当中。

### XML标签

#### animato

对应代码中的ValueAnimator

#### objectAnimator

对应代码中的ObjectAnimator

#### set

对应代码中的AnimatorSet

那么比如说我们想要实现一个从0到100平滑过渡的动画，在XML当中就可以这样写：

```xml
<animator xmlns:android="http://schemas.android.com/apk/res/android"  
    android:valueFrom="0"  
    android:valueTo="100"  
    android:valueType="intType"/>  
```

而如果我们想将一个视图的alpha属性从1变成0，就可以这样写：

```xml
<objectAnimator xmlns:android="http://schemas.android.com/apk/res/android"  
    android:valueFrom="1"  
    android:valueTo="0"  
    android:valueType="floatType"  
    android:propertyName="alpha"/>  
```

另外，我们也可以使用XML来完成复杂的组合动画操作，比如将一个视图先从屏幕外移动进屏幕，然后开始旋转360度，旋转的同时进行淡入淡出操作，就可以这样写：

```xml
<set xmlns:android="http://schemas.android.com/apk/res/android"  
    android:ordering="sequentially" >  
  
    <objectAnimator  
        android:duration="2000"  
        android:propertyName="translationX"  
        android:valueFrom="-500"  
        android:valueTo="0"  
        android:valueType="floatType" >  
    </objectAnimator>  
  
    <set android:ordering="together" >  
        <objectAnimator  
            android:duration="3000"  
            android:propertyName="rotation"  
            android:valueFrom="0"  
            android:valueTo="360"  
            android:valueType="floatType" >  
        </objectAnimator>  
  
        <set android:ordering="sequentially" >  
            <objectAnimator  
                android:duration="1500"  
                android:propertyName="alpha"  
                android:valueFrom="1"  
                android:valueTo="0"  
                android:valueType="floatType" >  
            </objectAnimator>  
            <objectAnimator  
                android:duration="1500"  
                android:propertyName="alpha"  
                android:valueFrom="0"  
                android:valueTo="1"  
                android:valueType="floatType" >  
            </objectAnimator>  
        </set>  
    </set>  
  
</set>  
```

这段XML实现的效果和我们刚才通过代码来实现的组合动画的效果是一模一样的。

### 在代码中加载XML文件-AnimatorInflater

在代码中把文件加载进来并将动画启动：

```java
Animator animator = AnimatorInflater.loadAnimator(context, R.animator.anim_file);  
animator.setTarget(view);  
animator.start();  
```

调用AnimatorInflater的loadAnimator来将XML动画文件加载进来，然后再调用setTarget()方法将这个动画设置到某一个对象上面，最后再调用start()方法启动动画就可以了。





## ViewPropertyAnimator

它并不是在3.0系统当中引入的，而是在3.1系统当中附增的一个新的功能。为View的动画操作提供一种更加便捷的用法。

**属性动画**的机制已经不是再针对于View而进行设计的了，而是一种不断地对值进行操作的机制，它可以**将值赋值到指定对象的指定属性上**。但是，在绝大多数情况下，我相信大家主要都还是对View进行动画操作的。

那我们先来回顾一下之前的用法吧，比如我们想要让一个TextView从常规状态变成透明状态，就可以这样写：

```java
ObjectAnimator animator = ObjectAnimator.ofFloat(textview, "alpha", 0f);  
animator.start();  
```

### 用法

使用ViewPropertyAnimator来实现同样的效果，ViewPropertyAnimator提供了更加易懂、更加面向对象的API，如下所示：

```java
textview.animate().alpha(0f);  //将当前的textview变成透明状态
```

**animate()**方法就是在Android **3.1系统上新增**的一个方法，这个方法的**返回**值是一个**ViewPropertyAnimator对象**，也就是说拿到这个对象之后我们就可以调用它的各种方法来实现动画效果了，这里我们调用了alpha()方法并转入0，表示将当前的textview变成透明状态。

ViewPropertyAnimator还可以很轻松地将多个动画组合到一起，比如我们想要让textview运动到500,500这个坐标点上，就可以这样写：

```java
textview.animate().x(500).y(500);  //让textview运动到500,500这个坐标点
```

ViewPropertyAnimator是支持连缀用法的，我们想让textview移动到横坐标500这个位置上时调用了x(500)这个方法，然后让textview移动到纵坐标500这个位置上时调用了y(500)这个方法，将所有想要组合的动画通过这种连缀的方式拼接起来，这样全部动画就都会一起被执行。

设定动画的运行时长：

```java
textview.animate().x(500).y(500).setDuration(5000);  //设定动画的运行时长
```

Interpolator也可以应用在ViewPropertyAnimator上面：

```java
textview.animate().x(500).y(500).setDuration(5000)  
        .setInterpolator(new BounceInterpolator());  //使用Interpolator
```

ViewPropertyAnimator用法基本大同小异，需要用到什么功能就连缀一下，因此更多的用法大家只需要去查阅一下文档，看看还支持哪些功能，有哪些接口可以调用就可以了。

### 注意

　　整个ViewPropertyAnimator的功能都是建立在View类新增的animate()方法之上的，这个方法会创建并返回一个ViewPropertyAnimator的实例，之后的调用的所有方法，设置的所有属性都是通过这个实例完成的。
　　在使用ViewPropertyAnimator时，我们自始至终没有调用过start()方法，这是因为新的接口中使用了隐式启动动画的功能，只要我们**将动画定义完成之后，动画就会自动启动**。并且这个机制对于组合动画也同样有效，只要我们不断地连缀新的方法，那么动画就不会立刻执行，**等到所有在ViewPropertyAnimator上设置的方法都执行完毕后，动画就会自动启动**。当然如果不想使用这一默认机制的话，我们**也可以显式地调用start()方法**来启动动画。
　　ViewPropertyAnimator的**所有接口都是使用连缀的语法来设计的**，每个方法的返回值都是它自身的实例，因此调用完一个方法之后可以直接连缀调用它的另一个方法，这样把所有的功能都串接起来，我们甚至可以仅通过一行代码就完成任意复杂度的动画功能。



# 属性动画之Evaluator

## 1、概述

我们先不讲什么是Evaluator，我们先来看一张图： 
![](http://upload-images.jianshu.io/upload_images/9028834-81845d6b5c67b308.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这幅图讲述了从定义动画的数字区间到通过AnimatorUpdateListener中得到当前动画所对应数值的整个过程。下面我们对这四个步骤具体讲解一下： 
(1)、**ofInt**(0,400)表示指定动画的数字区间，是从0运动到400； 
(2)、插值器**Interpolator**：在动画开始后，通过Interpolator会返回当前动画进度所对应的数字进度，但这个数字进度是百分制的，以小数表示，如0.2 
(3)、**Evaluator**:我们知道我们通过监听器拿到的是当前动画所对应的具体数值，而不是百分制的进度。那么就必须有一个地方会根据当前的数字进度，将其转化为对应的数值，这个地方就是Evaluator；Evaluator就是将从插值器Interpolator返回的数字进度转成对应的数字值。所以上部分中，我们讲到的公式：

```java
当前的值 = 100 + （400 - 100）* 显示进度  
```

这个公式就是在Evaluator计算的；在拿到当前数字进度所对应的值以后，将其返回 
（4）、**监听器**：我们通过在AnimatorUpdateListener监听器使用animation.getAnimatedValue()函数拿到Evaluator中返回的数字值。 
讲了这么多，Evaluator其实就是一个转换器，他能把小数进度转换成对应的数值位置

## 2、各种Evaluator

首先，插值器**Interpolator**返回的小数值，表示的是当前动画的数值进度。无论是利用**ofFloat**()还是利用**ofInt**()定义的动画**都**是**适用**的。因为无论是什么动画，它的进度必然都是在0到1之间的。0表示没开始，1表示数值运动的结束，对于任何动画都是适用的。 

但Evaluator则不一样，我们知道Evaluator是根据插值器Interpolator返回的小数进度转换成当前数值进度所对应的值。这问题就来了，如果我们使用ofInt()来定义动画，动画中的值应该都是Int类型，如果我用ofFloat()来定义动画，那么动画中的值也都是Float类型。所以如果我用ofInt()来定义动画，所对应的Evaluator在返回值时，必然要返回Int类型的值。同样，我们如果用ofFloat来定义动画，那么Evaluator在返回值时也必然返回的是Float类型的值。 
所以**每种定义方式所对应的Evaluator必然是它专用的**；Evaluator专用的原因在于动画数值类型不一样，在通过Evaluator返回时会报强转错误；所以只有在动画数值类型一样时，所对应的Evaluator才能通用。所以**ofInt()对应**的Evaluator类名叫**IntEvaluator**,而**ofFloat()对应**的Evaluator类名叫**FloatEvaluator**； 
在设置Evaluator时，是通过**animator.setEvaluator()来设置**的，比如：

### Evaluator使用

```java
ValueAnimator animator = ValueAnimator.ofInt(0,600);  
  
animator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {  
    @Override  
    public void onAnimationUpdate(ValueAnimator animation) {  
        int curValue = (int)animation.getAnimatedValue();  
        tv.layout(tv.getLeft(),curValue,tv.getRight(),curValue+tv.getHeight());  
    }  
});  
animator.setDuration(1000);  
animator.setEvaluator(new IntEvaluator());  
animator.setInterpolator(new BounceInterpolator());  
animator.start();  
```

但大家会说了，在此之前，我们在使用ofInt()时，从来没有给它定义过使用IntEvaluator来转换值啊，那怎么也能正常运行呢？因为ofInt和ofFloat都是系统直接提供的函数，所以在使用时都会有默认的插值器**Interpolator**和**Evaluator**来使用的，**不指定则使用默认的**；对于Evaluator而言，ofInt()的默认Evaluator当然是IntEvaluator;而FloatEvalutar默认的则是FloatEvalutor; 
上面，我们已经弄清楚Evaluator定义和使用方法，下面我们就来看看IntEvaluator内部是怎么实现的吧：

### IntEvaluator内部实现

```java
/** 
 * This evaluator can be used to perform type interpolation between <code>int</code> values. 
 */  
public class IntEvaluator implements TypeEvaluator<Integer> {  
  
    /** 
     * This function returns the result of linearly interpolating the start and end values, with 
     * <code>fraction</code> representing the proportion between the start and end values. The 
     * calculation is a simple parametric calculation: <code>result = x0 + t * (v1 - v0)</code>, 
     * where <code>x0</code> is <code>startValue</code>, <code>x1</code> is <code>endValue</code>, 
     * and <code>t</code> is <code>fraction</code>. 
     * 
     * @param fraction   The fraction from the starting to the ending values 
     * @param startValue The start value; should be of type <code>int</code> or 
     *                   <code>Integer</code> 
     * @param endValue   The end value; should be of type <code>int</code> or <code>Integer</code> 
     * @return A linear interpolation between the start and end values, given the 
     *         <code>fraction</code> parameter. 
     */  
    public Integer evaluate(float fraction, Integer startValue, Integer endValue) {  
        int startInt = startValue;  
        return (int)(startInt + fraction * (endValue - startInt));  
    }  
}  
```

可以看到在IntEvaluator中只有一个函数evaluate(float fraction, Integer startValue, Integer endValue)
其中**fraction**就是插值器Interpolator中的返回值，表示当前动画的数值进度，百分制的小数表示。我们应该根据它来计算当前动画的值应该是多少。
**startValue**和**endValue**分别对应ofInt(int start,int end)中的start和end的数值；
比如我们假设当我们定义的动画ofInt(100,400)进行到数值进度20%的时候，那么此时在evaluate函数中，fraction的值就是0.2，startValue的值是100，endValue的值是400；理解上应该没什么难度。 

下面我们就来看看evaluate(float fraction, Integer startValue, Integer endValue) 是如何根据进度小数值来计算出具体数字的：

```java
return (int)(startInt + fraction * (endValue - startInt));  
```

大家对这个公式是否似曾相识？我们前面提到的公式：

```java
当前的值 = 100 + （400 - 100）* 显示进度  
```

在我们看懂了IntEvalutor以后，下面我们尝试自己写一个Evalutor

## 3、自定义Evalutor

### （1）、简单实现MyEvalutor

前面我们看了IntEvalutor的代码，我们仿照IntEvalutor的实现方法，我们自定义一个MyEvalutor: 
首先是实现TypeEvaluator接口：

```java
public class MyEvaluator implements TypeEvaluator<Integer> {  
    @Override  
    public Integer evaluate(float fraction, Integer startValue, Integer endValue) {  
        return null;  
    }  
}  
```

这里涉及到泛型的概念，不理解的同学可以去看看[《夯实JAVA基本之一 —— 泛型详解(1):基本使用》 ](http://blog.csdn.net/harvic880925/article/details/49872903)
在实现TypeEvaluator，我们给它指定它的返回是Integer类型，这样我们就可以在ofInt()中使用这个Evaluator了。
再说一遍原因：只有定义动画时的数值类型与Evalutor的返回值类型一样时，才能使用这个Evalutor；很显然ofInt()定义的数值类型是Integer而我们定义的MyEvaluator，它的返回值类型也是Integer；所以我们定义的MyEvaluator可以给ofInt（）来用。同理，如果我们把实现的TypeEvaluator填充为为Float类型，那么这个Evalutor也就只能给FloatEvalutor用了。 
我们来简单实现evaluate函数，代码如下：

```java
public class MyEvaluator implements TypeEvaluator<Integer> {  
    @Override  
    public Integer evaluate(float fraction, Integer startValue, Integer endValue) {
        int startInt = startValue;  
        return (int)(200+startInt + fraction * (endValue - startInt));  
    }  
}  
```

我们在IntEvaluator的基础上修改了下，让它返回值时增加了200；所以当我们定义的区间是ofInt(0,400)时，它的实际返回值区间应该是(200,600) 
我们看看MyEvaluator的使用：

```java
ValueAnimator animator = ValueAnimator.ofInt(0,400);  
  
animator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {  
    @Override  
    public void onAnimationUpdate(ValueAnimator animation) {  
        int curValue = (int)animation.getAnimatedValue();  
        tv.layout(tv.getLeft(),curValue,tv.getRight(),curValue+tv.getHeight());  
    }  
});  
animator.setDuration(1000);  
animator.setEvaluator(new MyEvaluator());  
animator.start();  
```

设置MyEvaluator前的动画效果： 
[![](http://upload-images.jianshu.io/upload_images/9028834-008bffa9ab97dd33?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-008bffa9ab97dd33?imageMogr2/auto-orient/strip)

然后再看看我们设置了MyEvaluator以后的效果： 

[![](http://upload-images.jianshu.io/upload_images/9028834-f7a620384e476987?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-f7a620384e476987?imageMogr2/auto-orient/strip)

很明显，textview的动画位置都向下移动了200个点； 
再重新看一下下面的这个流程图：

![](http://upload-images.jianshu.io/upload_images/9028834-81845d6b5c67b308.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在插值器Interpolator中，我们可以通过自定义插值器Interpolator的返回的数值进度来改变返回数值的位置。
在Evaluator中，我们又可以通过改变进度值所对应的具体数字来改变数值的位置。 

**结论**： 
我们可以通过**重写插值器Interpolator改变数值进度来改变数值位置**，**也可以通过改变Evaluator中进度所对应的数值来改变数值位置**。

下面我们就只通过重写Evaluator来实现数值的倒序输出； 

### （2）、实现倒序输出实例

我们自定义一个ReverseEvaluator:

```java
public class ReverseEvaluator implements TypeEvaluator<Integer> {  
    @Override  
    public Integer evaluate(float fraction, Integer startValue, Integer endValue) {  
        int startInt = startValue;  
        return (int) (endValue - fraction * (endValue - startInt));  
    }  
}  
```

其中 fraction * (endValue - startInt)表示动画实际运动的距离，我们用endValue减去实际运动的距离就表示随着运动距离的增加，离终点越来越远，这也就实现了从终点出发，最终运动到起点的效果了。 
使用方法：

```java
ValueAnimator animator = ValueAnimator.ofInt(0,400);  
  
animator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {  
    @Override  
    public void onAnimationUpdate(ValueAnimator animation) {  
        int curValue = (int)animation.getAnimatedValue();  
        tv.layout(tv.getLeft(),curValue,tv.getRight(),curValue+tv.getHeight());  
    }  
});  
animator.setDuration(1000);  
animator.setEvaluator(new ReverseEvaluator());  
animator.start();  
```

效果图： 
[![](http://upload-images.jianshu.io/upload_images/9028834-5cf681d8cdc3f526?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-5cf681d8cdc3f526?imageMogr2/auto-orient/strip)

## 4、关于ArgbEvalutor

### 1、使用ArgbEvalutor

我们上面讲了IntEvaluator和FloatEvalutor，还说了Evalutor一般来讲不能通用，会报强转错误，也就是说，只有在数值类型相同的情况下，Evalutor才能共用。 
其实除了IntEvaluator和FloatEvalutor，在android.animation包下，还有另外一个Evalutor叫ArgbEvalutor。 
**ArgbEvalutor**是用来做**颜色值过渡转换**的。可能是谷歌的开发人员觉得大家对颜色值变换可能并不知道要怎么做，所以特地给我们提供了这么一个过渡Evalutor； 
我们先来简单看一下ArgbEvalutor的源码：（这里先不做具体讲解原理，最后会讲原理，这里先会用）

```java
public class ArgbEvaluator implements TypeEvaluator {  
    public Object evaluate(float fraction, Object startValue, Object endValue) {  
        int startInt = (Integer) startValue;  
        int startA = (startInt >> 24);  
        int startR = (startInt >> 16) & 0xff;  
        int startG = (startInt >> 8) & 0xff;  
        int startB = startInt & 0xff;  
  
        int endInt = (Integer) endValue;  
        int endA = (endInt >> 24);  
        int endR = (endInt >> 16) & 0xff;  
        int endG = (endInt >> 8) & 0xff;  
        int endB = endInt & 0xff;  
  
        return (int)((startA + (int)(fraction * (endA - startA))) << 24) |  
                (int)((startR + (int)(fraction * (endR - startR))) << 16) |  
                (int)((startG + (int)(fraction * (endG - startG))) << 8) |  
                (int)((startB + (int)(fraction * (endB - startB))));  
    }  
}  
```

我们在这里关注两个地方，第一返回值是int类型，这说明我们可以使用ofInt()来初始化动画数值范围。第二：颜色值包括A,R,G,B四个值，每个值是8位所以用16进制表示一个颜色值应该是0xffff0000（纯红色） 
下面我们就使用一下ArgbEvaluator，并看看效果：

```java
ValueAnimator animator = ValueAnimator.ofInt(0xffffff00,0xff0000ff);  
animator.setEvaluator(new ArgbEvaluator());  
animator.setDuration(3000);  
  
animator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {  
    @Override  
    public void onAnimationUpdate(ValueAnimator animation) {  
        int curValue = (int)animation.getAnimatedValue();  
        tv.setBackgroundColor(curValue);  
  
    }  
});  
  
animator.start();  
```

在这段代码中，我们将动画的数据范围定义为(0xffffff00,0xff0000ff)，即从黄色，变为蓝色。 
在监听中，我们根据当前传回来的颜色值，将其设置为textview的背景色 
我们来看一下效果： 
[![](http://upload-images.jianshu.io/upload_images/9028834-0f15695083678751?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-0f15695083678751?imageMogr2/auto-orient/strip)

到这里，我们就已经知道ArgbEvalutor的使用方法和效果了，下面我们再来回头看看ArgbEvalutor的实现方法 

### 2、ArgbEvalutor的实现原理

先重新看源码：

```java
/** 
 * This evaluator can be used to perform type interpolation between integer 
 * values that represent ARGB colors. 
 */  
public class ArgbEvaluator implements TypeEvaluator {  
  
    /** 
     * This function returns the calculated in-between value for a color 
     * given integers that represent the start and end values in the four 
     * bytes of the 32-bit int. Each channel is separately linearly interpolated 
     * and the resulting calculated values are recombined into the return value. 
     * 
     * @param fraction The fraction from the starting to the ending values 
     * @param startValue A 32-bit int value representing colors in the 
     * separate bytes of the parameter 
     * @param endValue A 32-bit int value representing colors in the 
     * separate bytes of the parameter 
     * @return A value that is calculated to be the linearly interpolated 
     * result, derived by separating the start and end values into separate 
     * color channels and interpolating each one separately, recombining the 
     * resulting values in the same way. 
     */  
    public Object evaluate(float fraction, Object startValue, Object endValue) {  
        int startInt = (Integer) startValue;  
        int startA = (startInt >> 24);  
        int startR = (startInt >> 16) & 0xff;  
        int startG = (startInt >> 8) & 0xff;  
        int startB = startInt & 0xff;  
  
        int endInt = (Integer) endValue;  
        int endA = (endInt >> 24);  
        int endR = (endInt >> 16) & 0xff;  
        int endG = (endInt >> 8) & 0xff;  
        int endB = endInt & 0xff;  
  
        return (int)((startA + (int)(fraction * (endA - startA))) << 24) |  
                (int)((startR + (int)(fraction * (endR - startR))) << 16) |  
                (int)((startG + (int)(fraction * (endG - startG))) << 8) |  
                (int)((startB + (int)(fraction * (endB - startB))));  
    }  
}  
```

英文注释的那一坨大家有兴趣，可以看已看看，我这里就直接讲代码了 
这段代码分为三部分，第一部分根据startValue求出其中A,R,G,B中各个色彩的初始值；第二部分根据endValue求出其中A,R,G,B中各个色彩的结束值，最后是根据当前动画的百分比进度求出对应的数值 
我们先来看第一部分：根据startValue求出其中A,R,G,B中各个色彩的初始值

```java
int startInt = (Integer) startValue;  
int startA = (startInt >> 24);  
int startR = (startInt >> 16) & 0xff;  
int startG = (startInt >> 8) & 0xff;  
int startB = startInt & 0xff;  
```

这段代码就是根据位移和与运算求出颜色值中A,R,G,B各个部分对应的值;颜色值与ARGB值的对应关系如下： 
![image](http://upload-images.jianshu.io/upload_images/9028834-1e368875d6db1864?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

所以我们的初始值是0xffffff00,那么求出来的startA = 0xff,startR = oxff,startG = 0xff,startB = 0x00; 
关于通过位移和与运算如何得到指定位的值的问题，我就不再讲了，大家如果不理解，可以搜一下相关运算符使用方法的文章。 
同样，我们看看第二部分根据endValue求出其中A,R,G,B中各个色彩的结束值：

```java
int endInt = (Integer) endValue;  
int endA = (endInt >> 24);  
int endR = (endInt >> 16) & 0xff;  
int endG = (endInt >> 8) & 0xff;  
int endB = endInt & 0xff;  
```

原理与startValue求A,R,G,B对应值的一样，所以对于我们上面例子中初始值ofInt(0xffffff00,0xff0000ff)中的endValue:0xff0000ff所对应的endA = 0xff,endR = ox00;endG = 0x00;endB = 0xff; 
最后一部分到了，就是如何根据进度来求得变化的值，我们先看看下面这句是什么意思：

```java
startA + (int)(fraction * (endA - startA)))  
```

对于这个公式大家应该很容易理解，与IntEvaluator中的计算公式一样，就是根据透明度A的初始值、结束值求得当前进度下透明度A应该的数值。 
同理 
startR + (int)(fraction * (endR - startR)表示当前进度下的红色值 
startG + (int)(fraction * (endG - startG))表示当前进度下的绿色值 
startB + (int)(fraction * (endB - startB))表示当前进度下的蓝色值 
然后通过位移和或运算将当前进度下的A,R,G,B组合起来就是当前的颜色值了。

**源码下载地址**：
github:[自定义控件三部曲之动画篇(五)——ValueAnimator高级进阶（一）](https://github.com/harvic/BlogResForGitHub)

# 属性动画之ValueAnimator
## 概述
ValueAnimator是整个属性动画机制当中最核心的一个类，前面我们已经提到了，**属性动画的运行机制是通过不断地对值进行操作来实现的**，而初始值和结束值之间的**动画过渡**就是**由ValueAnimator**这个类来负责**计算**的。
它的内部使用一种时间循环的机制来计算值与值之间的动画过渡，我们只需要将初始值和结束值提供给ValueAnimator，并且告诉它动画所需运行的**时长**，那么ValueAnimator就会自动帮我们完成从初始值平滑地过渡到结束值这样的效果。除此之外，ValueAnimator还负责管理动画的播放**次数**、播放**模式**、以及**对动画设置监听器**等，确实是一个非常重要的类。


但是ValueAnimator的用法却一点都不复杂，我们先从最简单的功能看起吧，比如说想要将一个值从0平滑过渡到1，时长300毫秒，就可以这样写：

```java
ValueAnimator anim = ValueAnimator.ofFloat(0f, 1f);  
anim.setDuration(300);  
anim.start();  
```

怎么样？很简单吧，调用ValueAnimator的**ofFloat()**方法就可以构建出一个ValueAnimator的实例，ofFloat()方法当中允许传入多个float类型的参数，这里传入0和1就表示将值从0平滑过渡到1，然后调用ValueAnimator的setDuration()方法来设置动画运行的时长，最后调用start()方法启动动画。

用法就是这么简单，现在如果你运行一下上面的代码，动画就会执行了。可是这只是一个将值从0过渡到1的动画，又看不到任何界面效果，我们怎样才能知道这个动画是不是已经真正运行了呢？这就需要借助监听器来实现了，如下所示：

```java
ValueAnimator anim = ValueAnimator.ofFloat(0f, 1f);  
anim.setDuration(300);  
anim.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {  
    @Override  
    public void onAnimationUpdate(ValueAnimator animation) {  
        float currentValue = (float) animation.getAnimatedValue();  
        Log.d("TAG", "cuurent value is " + currentValue);  
    }  
});  
anim.start();  
```

可以看到，这里我们通过**addUpdateListener()**方法来添加一个动画的**监听器**，在动画执行的过程中会不断地进行回调，我们只需要在回调方法当中将当前的值取出并打印出来，就可以知道动画有没有真正运行了。运行上述代码，控制台打印如下所示：
![](http://img.blog.csdn.net/20171221143810600?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

从打印日志的值我们就可以看出，ValueAnimator确实已经在正常工作了，值在300毫秒的时间内从0平滑过渡到了1，而这个计算工作就是由ValueAnimator帮助我们完成的。另外ofFloat()方法当中是可以传入任意多个参数的，因此我们还可以构建出更加复杂的动画逻辑，比如说将一个值在5秒内从0过渡到5，再过渡到3，再过渡到10，就可以这样写：

```java
ValueAnimator anim = ValueAnimator.ofFloat(0f, 5f, 3f, 10f);  
anim.setDuration(5000);  
anim.start();  
```

当然也许你并不需要小数位数的动画过渡，可能你只是希望将一个整数值从0平滑地过渡到100，那么也很简单，只需要调用ValueAnimator的ofInt()方法就可以了，如下所示：

```java
ValueAnimator anim = ValueAnimator.ofInt(0, 100);  
```

### 常用方法

**ValueAnimator**当中**最常用**的应该就是**ofFloat()和ofInt()**这两个方法了，另外**还有**一个**ofObject()**方法，我会在下篇文章进行讲解。
**setStartDelay()**：动画延迟播放的时间
**setRepeatCount()**：动画循环播放的次数
**setRepeatMode()**：循环播放的模式，循环模式包括RESTART和REVERSE两种，分别表示重新播放和倒序播放的意思。

## ofObject
### 概述

ofInt()只能传入Integer类型的值，而ofFloat（）则只能传入Float类型的值。
那如果我们需要操作其它类型的变量要怎么办呢？其实ValueAnimator还有一个函数ofObject(),可以传进去任何类型的变量，定义如下：

```java
public static ValueAnimator ofObject(TypeEvaluator evaluator, Object... values);  
```

它有两个参数：
第一个是自定义的Evaluator，
第二个是可变长参数，Object类型的； 
大家可能会疑问，为什么要强制传进去自定义的Evaluator？首先，大家知道Evaluator的作用是根据当前动画的显示进度，计算出当前进度下把对应的值。那既然Object对象是我们自定的，那必然从进度到值的转换过程也必须由我们来做，不然系统哪知道你要转成个什么鬼。 
好了，现在我们先简单看一下ofObject这个怎么用。 

### 基本使用：自定义View A-Z字母变化
我们先来看看我们要实现的效果： 
[![](http://upload-images.jianshu.io/upload_images/9028834-9aa0f63b54c4afc8?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-9aa0f63b54c4afc8?imageMogr2/auto-orient/strip)

从效果图中可以看到，按钮上的**字母从A变化到Z**,刚开始变的慢，后来逐渐加速；

```java
ValueAnimator animator = ValueAnimator.ofObject(new CharEvaluator(),new Character('A'),new Character('Z'));  
animator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {  
    @Override  
    public void onAnimationUpdate(ValueAnimator animation) {  
        char text = (char)animation.getAnimatedValue();  
        tv.setText(String.valueOf(text));  
    }  
});  
animator.setDuration(10000);  
animator.setInterpolator(new AccelerateInterpolator());  
animator.start();  
```

这里注意三点： 
#### 第一，构造时：
```java
ValueAnimator animator = ValueAnimator.ofObject(new CharEvaluator(),new Character('A'),new Character('Z'));
```

我们自定义的一个**CharEvaluator**，这个类实现，后面会讲；在初始化动画时，传进去的是Character对象，一个是字母A,一个是字母Z; 
我们这里要实现的效果是，对Character对象来做动画，利用动画自动从字母A变到字母Z，具体怎么实现就是CharEvaluator的事了，这里我们只需要知道，在构造时传进去的是两个Character对象 

#### 第二：看监听：
```java
char text = (char)animation.getAnimatedValue();  
tv.setText(String.valueOf(text));  
```

通过animation.getAnimatedValue()得到当前动画的字符，然后把字符设置给textview；大家知道我们构造时传进去的值类型是Character对象，所以在动画过程中通过Evaluator返回的值类型必然跟构造时的类型是一致的，也是Character 

#### 第三：插值器
```java
animator.setInterpolator(new AccelerateInterpolator());  
```

我们使用的是加速插值器，加速插值器的特点就是随着动画的进行，速度会越来越快，这点跟我们上面的效果图是一致的。 

下面最关键的就是看CharEvaluator是怎么实现的了。
#### ASCII码中数值与字符的转换方法
我们知道在ASCII码表中，每个字符都是有数字跟他一一对应的，字母A到字母Z之间的所有字母对应的数字区间为65到90； 
而且在程序中，我们能通过数字强转成对应的字符。 
比如： 
**数字转字符**:
```java
char  temp = (char)65;//得到的temp的值就是大写字母A
```

**字符转数字**：
```java
char temp = 'A';  
int num = (int)temp;  //在这里得到的num值就是对应的ASCII码值65
```

#### CharEvaluator的实现：

```java
public class CharEvaluator implements TypeEvaluator<Character> {  
    @Override  
    public Character evaluate(float fraction, Character startValue, Character endValue) {  
        int startInt  = (int)startValue;  
        int endInt = (int)endValue;  
        int curInt = (int)(startInt + fraction *(endInt - startInt));  
        char result = (char)curInt;  
        return result;  
    }  
}  
```

在这里，我们就利用A-Z字符在**ASCII码表中对应数字是连续且递增**的原理，先求出来对应字符的数字值，然后再转换成对应的字符。代码难度不大，就不再细讲了。 

好了，到这里，有关ofObject()的使用大家应该就会了，上面我们说过，ofObject()能够初始化任何对象，下面我们就稍微加深些难度， 我们自定义一个类对象，然后利用ofObject()来构造这个对象的动画。


### 高级使用：自定义弹性圆的伸缩特效

我们先看看这部分，我们将实现的效果： 
[![](http://upload-images.jianshu.io/upload_images/9028834-b41f137a34242777?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-b41f137a34242777?imageMogr2/auto-orient/strip)

在这里，我们自定义了一个View，在这个view上画一个圆，但这个圆是有动画效果的。从效果中可以看出使用的插值器应该是回弹插值器（BounceInterpolator） 
下面就来看看这个动画是怎么做出来的

#### 1、首先，我们自定义一个类Point:

```java
public class Point {  
    private int radius;    
    public Point(int radius){  
        this.radius = radius;  
    }    
    public int getRadius() {  
        return radius;  
    }    
    public void setRadius(int radius) {  
        this.radius = radius;  
    }  
}  
```

point类内容很简单，只有一个成员变量：radius表示当前point的半径。

#### 2、然后我们自定义一个View:MyPointView

```java
public class MyPointView extends View {  
    private Point mCurPoint;  
    public MyPointView(Context context, AttributeSet attrs) {  
        super(context, attrs);  
    }  
  
    @Override  
    protected void onDraw(Canvas canvas) {  
        super.onDraw(canvas);  
        if (mCurPoint != null){  
            Paint paint = new Paint();  
            paint.setAntiAlias(true);  
            paint.setColor(Color.RED);  
            paint.setStyle(Paint.Style.FILL);  
            canvas.drawCircle(300,300,mCurPoint.getRadius(),paint);  
        }  
    }  
  
    public void doPointAnim(){  
        ValueAnimator animator = ValueAnimator.ofObject(new PointEvaluator(),new Point(20),new Point(200));  
        animator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {  
            @Override  
            public void onAnimationUpdate(ValueAnimator animation) {  
                mCurPoint = (Point)animation.getAnimatedValue();  
                invalidate();  
            }  
        });  
        animator.setDuration(1000);  
        animator.setInterpolator(new BounceInterpolator());  
        animator.start();  
    }  
}  
```

##### (1)、doPointAnim()函数

在这段代码中，首先来看看供外部调用开始动画的doPointAnim()函数：

```java
public void doPointAnim(){  
    ValueAnimator animator = ValueAnimator.ofObject(new PointEvaluator(),new Point(20),new Point(200));  
    animator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {  
        @Override  
        public void onAnimationUpdate(ValueAnimator animation) {  
            mCurPoint = (Point)animation.getAnimatedValue();  
            invalidate();  
        }  
    });  
    animator.setDuration(1000);  
    animator.setInterpolator(new BounceInterpolator());  
    animator.start();  
}  
```

同样，先来看ofObject的构造动画的方法：

```java
ValueAnimator animator = ValueAnimator.ofObject(new PointEvaluator(),new Point(20),new Point(200));  
```

在构造动画时，动画所对应的值的类型是Point对象，那说明我们自定义的PointEvaluator中的返回值也必然是Point了。有关PointEvaluator的实现后面再讲 
然后再来看看动画过程监听：

```java
animator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {  
    @Override  
    public void onAnimationUpdate(ValueAnimator animation) {  
        mCurPoint = (Point)animation.getAnimatedValue();  
        invalidate();  
    }  
});  
```

在监听过程中，先根据animation.getAnimatedValue()得到当前动画进度所对应的Point实例，保存在mCurPoint中，然后强制刷新 

##### (2)、OnDraw()函数

在强制刷新之后，就会走到OnDraw()函数下面：

```java
protected void onDraw(Canvas canvas) {  
    super.onDraw(canvas);  
    if (mCurPoint != null){  
        Paint paint = new Paint();  
        paint.setAntiAlias(true);  
        paint.setColor(Color.RED);  
        paint.setStyle(Paint.Style.FILL);  
        canvas.drawCircle(300,300,mCurPoint.getRadius(),paint);  
    }  
}  
```

onDraw函数没什么难度，就是根据mCurPoint的半径在（300,300）的位置画出来圆形，有关绘图的知识大家可以参考另一个系列[《android Graphics（一）：概述及基本几何图形绘制》 ](http://blog.csdn.net/harvic880925/article/details/38875149)

##### (3)、PointEvaluator

在构造ofObject中，我们也可以知道，初始值和动画中间值的类型都是Point类型，所以PointEvaluator输入的返回类型都应该是Point类型的，先看看PointEvaluator的完整代码：

```java
public class PointEvaluator implements TypeEvaluator<Point> {  
    @Override  
    public Point evaluate(float fraction, Point startValue, Point endValue) {  
        int start = startValue.getRadius();  
        int end  = endValue.getRadius();  
        int curValue = (int)(start + fraction * (end - start));  
        return new Point(curValue);  
    }  
}  
```

这段代码其实比较容易理解，就是根据初始半径和最终半径求出当前动画进程所对应的半径值，然后新建一个Point对象返回。

#### 3、使用MyPointView

首先在main.xml中添加对应的控件布局：从效果图中也可以看到，我们将MyPointView是布局在最下方的，布局代码如下：

```xml
<?xml version="1.0" encoding="utf-8"?>  
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"  
                android:orientation="vertical"  
                android:layout_width="fill_parent"  
                android:layout_height="fill_parent">  
  
    <Button  
            android:id="@+id/btn"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:layout_alignParentLeft="true"  
            android:padding="10dp"  
            android:text="start anim"  
            />  
  
    <Button  
            android:id="@+id/btn_cancel"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:layout_alignParentRight="true"  
            android:padding="10dp"  
            android:text="cancel anim"  
            />  
    <TextView  
            android:id="@+id/tv"  
            android:layout_width="100dp"  
            android:layout_height="wrap_content"  
            android:layout_centerHorizontal="true"  
            android:gravity="center"  
            android:padding="10dp"  
            android:background="#ffff00"  
            android:text="Hello qijian"/>  
  
    <com.harvic.BlogValueAnimator4.MyPointView  
            android:id="@+id/pointview"  
            android:layout_below="@id/tv"  
            android:layout_width="match_parent"  
            android:layout_height="match_parent"/>  
</RelativeLayout>  
```

其实也没什么难度，就是在原来的布局代码下面加一个MyPointView控件

然后我们来看看在MyActivity.java中是怎么来用的吧
```java
public class MyActivity extends Activity {  
    private Button btnStart;  
    private MyPointView mPointView;  
  
    @Override  
    public void onCreate(Bundle savedInstanceState) {  
        super.onCreate(savedInstanceState);  
        setContentView(R.layout.main);  
  
        btnStart = (Button) findViewById(R.id.btn);  
        mPointView = (MyPointView)findViewById(R.id.pointview);  
  
        btnStart.setOnClickListener(new View.OnClickListener() {  
            @Override  
            public void onClick(View v) {  
                mPointView.doPointAnim();  
            }  
        });  
    }  
}  
```

这段代码没什么难度，就是在点击start anim按钮的时候，调用mPointView.doPointAnim()方法开始动画。 



**源码下载地址：**
github:https://github.com/harvic/BlogResForGitHub

### 习题：对自定义View使用动画

目标：通过对对象进行值操作来实现动画效果

#### 实现目标

比如说我们有一个自定义的View，在这个View当中有一个Point对象用于管理坐标，然后在onDraw()方法当中就是根据这个Point对象的坐标值来进行绘制的。也就是说，如果我们可以对Point对象进行动画操作，那么整个自定义View的动画效果就有了。
效果图：
[![效果图](http://img.blog.csdn.net/20171221153249662?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)](http://img.blog.csdn.net/20171221153249662?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

#### 实现步骤

##### 先定义一个Point类：

Point类非常简单，只有x和y两个变量用于记录坐标的位置，并提供了构造方法来设置坐标，以及get方法来获取坐标。

```java
public class Point {    
    private float x;    
    private float y;    
    public Point(float x, float y) {  
        this.x = x;  
        this.y = y;  
    }    
    public float getX() {  
        return x;  
    }    
    public float getY() {  
        return y;  
    }    
}  
```

##### 自定义TypeEvaluator：

实现TypeEvaluator接口并重写了evaluate()方法：先是将startValue和endValue强转成Point对象，然后同样根据fraction来计算当前动画的x和y的值，最后组装到一个新的Point对象当中并返回。

```java
public class PointEvaluator implements TypeEvaluator{  
  
    @Override  
    public Object evaluate(float fraction, Object startValue, Object endValue) {  
        Point startPoint = (Point) startValue;  
        Point endPoint = (Point) endValue;  
        float x = startPoint.getX() + fraction * (endPoint.getX() - startPoint.getX());  
        float y = startPoint.getY() + fraction * (endPoint.getY() - startPoint.getY());  
        Point point = new Point(x, y);  
        return point;  
    }  
  
}  
```

这样我们就将PointEvaluator编写完成了，接下来我们就可以非常轻松地对Point对象进行动画操作了，比如说我们有两个Point对象，现在需要将Point1通过动画平滑过度到Point2，就可以这样写：
需要注意的是，ofObject()方法要求多传入一个TypeEvaluator参数，这里我们只需要传入刚才定义好的PointEvaluator的实例就可以了。

```java
Point point1 = new Point(0, 0);  
Point point2 = new Point(300, 300);  
ValueAnimator anim = ValueAnimator.ofObject(new PointEvaluator(), point1, point2);  
anim.setDuration(5000);  
anim.start();  
```

好的，这就是自定义TypeEvaluator的全部用法，掌握了这些知识之后，我们就可以来尝试一下如何通过对Point对象进行动画操作，从而实现整个自定义View的动画效果。

##### 自定义MyAnimView：

首先在自定义View的构造方法当中初始化了一个Paint对象作为画笔，并将画笔颜色设置为蓝色，接着在onDraw()方法当中进行绘制。
这里我们绘制的逻辑是由currentPoint这个对象控制的，如果currentPoint对象不等于空，那么就调用drawCircle()方法在currentPoint的坐标位置画出一个半径为50的圆，如果currentPoint对象是空，那么就调用startAnimation()方法来启动动画。

```java
public class MyAnimView extends View {  
  
    public static final float RADIUS = 50f;  
  
    private Point currentPoint;  
  
    private Paint mPaint;  
  
    public MyAnimView(Context context, AttributeSet attrs) {  
        super(context, attrs);  
        mPaint = new Paint(Paint.ANTI_ALIAS_FLAG);  
        mPaint.setColor(Color.BLUE);  
    }  
  
    @Override  
    protected void onDraw(Canvas canvas) {  
        if (currentPoint == null) {  
            currentPoint = new Point(RADIUS, RADIUS);  
            drawCircle(canvas);  
            startAnimation();  
        } else {  
            drawCircle(canvas);  
        }  
    }  
  
    private void drawCircle(Canvas canvas) {  
        float x = currentPoint.getX();  
        float y = currentPoint.getY();  
        canvas.drawCircle(x, y, RADIUS, mPaint);  
    }  
  
    private void startAnimation() {  
        Point startPoint = new Point(RADIUS, RADIUS);  
        Point endPoint = new Point(getWidth() - RADIUS, getHeight() - RADIUS);  
        ValueAnimator anim = ValueAnimator.ofObject(new PointEvaluator(), startPoint, endPoint);  
        anim.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {  
            @Override  
            public void onAnimationUpdate(ValueAnimator animation) {  
                currentPoint = (Point) animation.getAnimatedValue();  
                invalidate();  
            }  
        });  
        anim.setDuration(5000);  
        anim.start();  
    }    
}  
```

startAnimation()方法中的代码：
就是对Point对象进行了一个动画操作而已。
这里我们定义了一个startPoint和一个endPoint，坐标分别是View的左上角和右下角，并将动画的时长设为5秒。然后有一点需要大家注意的，就是我们通过监听器对动画的过程进行了监听，每当Point值有改变的时候都会回调onAnimationUpdate()方法。在这个方法当中，我们对currentPoint对象进行了重新赋值，并调用了invalidate()方法，这样的话onDraw()方法就会重新调用，并且由于currentPoint对象的坐标已经改变了，那么绘制的位置也会改变，于是一个平移的动画效果也就实现了。

##### 在布局文件当中引入这个自定义控件：

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent"  
    >  
    <com.example.tony.myapplication.MyAnimView  
        android:layout_width="match_parent"  
        android:layout_height="match_parent" />  
</RelativeLayout>  
```


# 属性动画之ObjectAnimator
## 概述
相比于ValueAnimator，ObjectAnimator可能才是我们最常接触到的类，因为**ValueAnimator只**不过是**对值**进行了一个平滑的动画过渡，但我们实际使用到这种功能的场景好像并不多。而**ObjectAnimator**则就不同了，它是可以直接**任意对象**的**任意属性**进行动画操作的，比如说View的alpha属性。

不过虽说ObjectAnimator会更加常用一些，但是它其实是**继承自ValueAnimator**的，底层的动画实现机制也是基于ValueAnimator来完成的，因此ValueAnimator仍然是整个属性动画当中最核心的一个类。

### 1、引入
ValueAnimator有个缺点，就是只能对数值对动画计算。我们要想对哪个控件操作，需要监听动画过程，在监听中对控件操作。这样使用起来相比补间动画而言就相对比较麻烦。 
为了能让动画直接与对应控件相关联，以使我们从监听动画过程中解放出来，谷歌的开发人员在ValueAnimator的基础上，又派生了一个类ObjectAnimator; 
由于**ObjectAnimator是派生自ValueAnimator**的，所以ValueAnimator中所能使用的方法，在ObjectAnimator中都可以正常使用。 
但ObjectAnimator也重写了几个方法，比如ofInt(),ofFloat()等。我们先看看利用ObjectAnimator重写的ofFloat方法如何实现一个动画：（改变透明度）

```java
ObjectAnimator animator = ObjectAnimator.ofFloat(tv,"alpha",1,0,1);  
/*
参数1：传入一个object对象，我们想要对哪个对象进行动画操作就传入什么，这里我传入了一个tv。
参数2：想要对该对象的哪个属性进行动画操作，由于我们想要改变TextView的不透明度，因此这里传入"alpha"。
参数3：不固定长度，想要完成什么样的动画就传入什么值。
*/
animator.setDuration(2000);  
animator.start();  
```

效果图如下：
[![](http://upload-images.jianshu.io/upload_images/9028834-e2792efbd734a6ed?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-e2792efbd734a6ed?imageMogr2/auto-orient/strip)

我们这里还是直接使用上一篇的框架代码;（当点击start anim时执行动画）从上面的代码中可以看到构造ObjectAnimator的方法非常简单：

```java
public static ObjectAnimator ofFloat(Object target, String propertyName, float... values)   
```

*   第一个参数用于指定这个动画要操作的是哪个**控件**
*   第二个参数用于指定这个动画要操作这个控件的哪个**属性**
*   第三个参数是可变长参数，这个就跟ValueAnimator中的可变长参数的意义一样了，就是指这个**属性值是从哪变到哪**。像我们上面的代码中指定的就是将textview的alpha属性从0变到1再变到0；

下面我们再来看一下如何实现旋转效果：

```java
ObjectAnimator animator = ObjectAnimator.ofFloat(tv,"rotation",0,180,0);  
animator.setDuration(2000);  
animator.start();  
```

效果图如下：
[![](http://upload-images.jianshu.io/upload_images/9028834-f7ddc85df922e556?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-f7ddc85df922e556?imageMogr2/auto-orient/strip)

从代码中可以看到，我们只需要改变ofFloat()的第二个参数的值就可以实现对应的动画； 
那么问题来了，我们怎么知道第二个参数的值是啥呢？

### 2、setter函数

我们再回来看构造改变rotation值的ObjectAnimator的方法

```java
ObjectAnimator animator = ObjectAnimator.ofFloat(tv,"rotation",0,180,0);  
```

TextView控件有rotation这个属性吗？没有，不光TextView没有，连它的父类View中也没有这个属性。那它是怎么来改变这个值的呢？
其实，**ObjectAnimator**做动画，并不是根据控件xml中的属性来改变的，而是**通过指定属性所对应的set方法来改变**的。
比如，我们上面指定的改变rotation的属性值，ObjectAnimator在做动画时就会到指定控件（TextView）中去找对应的setRotation()方法来改变控件中对应的值。同样的道理，当我们在最开始的示例代码中，指定改变”alpha”属性值的时候，ObjectAnimator也会到TextView中去找对应的setAlpha()方法。那TextView中都有这些方法吗，有的，这些方法都是从View中继承过来的，在View中有关动画，总共有下面几组set方法：

```java
//1、透明度：alpha  
public void setAlpha(float alpha)  
  
//2、旋转度数：rotation、rotationX、rotationY  
public void setRotation(float rotation)  
public void setRotationX(float rotationX)  
public void setRotationY(float rotationY)  
  
//3、平移：translationX、translationY  
public void setTranslationX(float translationX)   
public void setTranslationY(float translationY)  
  
//缩放：scaleX、scaleY  
public void setScaleX(float scaleX)  
public void setScaleY(float scaleY)  
```

可以看到在View中已经实现了有关alpha,rotaion,translate,scale相关的set方法。所以我们在构造ObjectAnimator时可以直接使用。 

#### 使用注意：
在开始逐个看这些函数的使用方法前，我们先做一个**总结**： 
1、要使用ObjectAnimator来构造对画，要操作的控件中，**必须存在对应的属性的set方法** 
2、**setter 方法的命名必须以骆驼拼写法命名**，即set后每个单词首字母大写，其余字母小写，即类似于setPropertyName所对应的属性为propertyName 

下面我们就来看一下上面中各个方法的使用方法及作用。 
有关alpha的用法，上面已经讲过了，下面我们来看看其它的 

#### (1)、setRotationX、setRotationY与setRotation

*   setRotationX(float rotationX)：表示围绕X轴旋转，rotationX表示旋转度数 
*   setRotationY(rotationY):表示围绕Y轴旋转，rotationY表示旋转度数 
*   setRotation(float rotation):表示围绕Z旋转,rotation表示旋转度数 

先来看看setRotationX的效果：

```java
ObjectAnimator animator = ObjectAnimator.ofFloat(tv,"rotationX",0,270,0);  
animator.setDuration(2000);  
animator.start();  
```

效果图如下： 

[![](http://upload-images.jianshu.io/upload_images/9028834-d8c191e0991af4f6?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-d8c191e0991af4f6?imageMogr2/auto-orient/strip)

从效果图中明显看出，textview的旋转方法是围绕X轴旋转的，我们设定为从0度旋转到270度再返回0度。 
然后再来看看setRotationY的使用方法与效果：

```java
ObjectAnimator animator = ObjectAnimator.ofFloat(tv,"rotationY",0,180,0);  
animator.setDuration(2000);  
animator.start();  
```

效果图如下： 

[![](http://upload-images.jianshu.io/upload_images/9028834-a6aa186b7591a9bb?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-a6aa186b7591a9bb?imageMogr2/auto-orient/strip)

从效果图中明显可以看出围绕Y轴旋转的。 
我们再来看看setRotation的用法与效果：

```java
ObjectAnimator animator = ObjectAnimator.ofFloat(tv,"rotation",0,270,0);  
animator.setDuration(2000);  
animator.start();  
```

[![](http://upload-images.jianshu.io/upload_images/9028834-dcfae682fd4f6307?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-dcfae682fd4f6307?imageMogr2/auto-orient/strip)

setRotation是围绕Z轴旋转的，我们来看一张图： 

![](http://upload-images.jianshu.io/upload_images/9028834-3e2f1fca0cd25668?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

从这张图中，绿色框部分表示手机屏幕，很明显可以看出Z轴就是从屏幕左上角原点向外伸出的一条轴。这样，我们也就可以理解围绕Z轴旋转，为什么是这样子转了。 

#### （2）、setTranslationX与setTranslationY

*   setTranslationX(float translationX) :表示在X轴上的平移距离,以当前控件为原点，向右为正方向，参数translationX表示移动的距离。 
*   setTranslationY(float translationY) :表示在Y轴上的平移距离，以当前控件为原点，向下为正方向，参数translationY表示移动的距离。 

我们先看看setTranslationX的用法：

```java
ObjectAnimator animator = ObjectAnimator.ofFloat(tv, "translationX", 0, 200, -200,0);  
animator.setDuration(2000);  
animator.start();  
```

效果图如下： 
[![](http://upload-images.jianshu.io/upload_images/9028834-e6e07f3405fd51f9.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://upload-images.jianshu.io/upload_images/9028834-e6e07f3405fd51f9.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

所以，我们上面在构造动画时，指定的移动距离是（0, 200, -200,0），所以控件会从自身所有位置向右移动200像素，然后再移动到距离原点-200的位置，最后回到原点； 
然后我们来看看setTranslateY的用法：

```java
ObjectAnimator animator = ObjectAnimator.ofFloat(tv, "translationY", 0, 200, -100,0);  
animator.setDuration(2000);  
animator.start();  
```

效果图如下：（为了方便看到效果，将textview垂直居中） 
[![](http://upload-images.jianshu.io/upload_images/9028834-20392af8ef67323e?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-20392af8ef67323e?imageMogr2/auto-orient/strip)

同样，移动位置的坐标也都是以当前控件所在位置为中心点的。所以对应的移动位置从原点移动向下移动200像素，然后再移动到向下到距原点200像素的位置，最后再回到(0,0)从效果图中很明显可以看出来。 
从上面可以看出：每次移动距离的计算都是以原点为中心的；比如初始动画为ObjectAnimator.ofFloat(tv, “translationY”, 0, 200, -100,0)表示首先从0移动到正方向200的位置，然后再移动到负方向100的位置，最后移动到原点。 

#### (3)、setScaleX与setScaleY

*   setScaleX(float scaleX):在X轴上缩放，scaleX表示缩放倍数 
*   setScaleY(float scaleY):在Y轴上缩放，scaleY表示缩放倍数 

我们来看看setScaleX的用法：

```java
ObjectAnimator animator = ObjectAnimator.ofFloat(tv, "scaleX", 0, 3, 1);  
animator.setDuration(2000);  
animator.start();  
```

效果图如下： 
[![](http://upload-images.jianshu.io/upload_images/9028834-5475792fa42ac52a?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-5475792fa42ac52a?imageMogr2/auto-orient/strip)

在效果图中，从0倍放大到3倍，然后再还原到1倍的原始状态。 
然后再来看看setScaleY的用法

```java
ObjectAnimator animator = ObjectAnimator.ofFloat(tv, "scaleY", 0, 3, 1);  
animator.setDuration(2000);  
animator.start();  
```

为了更好的看到效果，我把textview垂直居中了，效果图如下： 
[![](http://upload-images.jianshu.io/upload_images/9028834-040700a8f990b898?imageMogr2/auto-orient/strip)
](http://upload-images.jianshu.io/upload_images/9028834-040700a8f990b898?imageMogr2/auto-orient/strip)

好了，到这里有关View中自带的set函数讲完了，我们来看看ObjectAnimator是如何实现控件动画效果的。

### 3、ObjectAnimator动画原理

我们先来看张图： 
[![](http://upload-images.jianshu.io/upload_images/9028834-88fecd8826548552.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://upload-images.jianshu.io/upload_images/9028834-88fecd8826548552.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在这张图中，将ValueAnimator的动画流程与ObjectAnimator的动画流程做了个对比。 
可以看到ObjectAnimator的动画流程中，也是首先通过加速器产生当前进度的百分比，然后再经过Evaluator生成对应百分比所对应的数字值。
这两步与ValueAnimator是完全一样的，**唯一不同的是最后一步**：
在**ValueAnimator**中，我们要通过添加**监听器**来监听当前数字值；
在**ObjectAnimator**中，则是先根据属性值拼装成对应的**set函数**的名字；
　比如这里的scaleY的拼装方法就是将属性的第一个字母强制大写后，与set拼接，所以就是setScaleY。然后通过反射找到对应控件的setScaleY(float scaleY)函数，将当前数字值做为setScaleY(float scale)的参数将其传入。 
这里在找到控件的set函数以后，是通过反射来调用这个函数的，有关反射的使用大家可以参考[《夯实JAVA基本之二 —— 反射（1）：基本类周边信息获取》 ](http://blog.csdn.net/harvic880925/article/details/50072739)
这就是**ObjectAnimator**的流程，最后一步总结起来就是**调用对应属性的set方法，将动画当前数字值做为参数传进去**。 

**根据上面的流程，这里有几个注意事项： **
**(1)、拼接set函数的方法：**上面我们也说了是首先是强制将属性的第一个字母大写，然后与set拼接，就是对应的set函数的名字。注意，只是强制将属性的第一个字母大写，后面的部分是保持不变的。反过来，如果我们的函数名命名为setScalePointX(float ),那我们在写属性时可以写成”scalePointX”或者写成“ScalePointX”都是可以的，即第一个字母大小写可以随意，但后面的部分必须与set方法后的大小写保持一致。 
**(2)、如何确定函数的参数类型：**上面我们知道了如何找到对应的函数名，那对应的参数方法的参数类型如何确定呢？我们在讲ValueAnimator的时候说过，动画过程中产生的数字值与构造时传入的值类型是一样的。由于ObjectAnimator与ValueAnimator在插值器和Evaluator这两步是完全一样的，而当前动画数值的产生是在Evaluator这一步产生的，所以ObjectAnimator的动画中产生的数值类型也是与构造时的类型一样的。那么问题来了，像我们的构造方法。

```java
ObjectAnimator animator = ObjectAnimator.ofFloat(tv, "scaleY", 0, 3, 1);  
```

由于构造时使用的是ofFloat函数，所以中间值的类型应该是Float类型的，所以在最后一步拼装出来的set函数应该是setScaleY(float xxx)的样式；这时，系统就会利用反射来找到setScaleY(float xxx)函数，并把当前的动画数值做为参数传进去。 
那问题来了，如果没有类似setScaleY(float xxx)的函数，我们只实现了一个setScaleY(int xxx)的函数怎么办？这里虽然函数名一样，但参数类型是不一样的，那么系统就会报一个错误： 
![image](http://upload-images.jianshu.io/upload_images/9028834-c1cb1f4605c5c972?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

意思就是对应函数的指定参数类型没有找到。 
**（3）、调用set函数以后怎么办？**从ObjectAnimator的流程可以看到，ObjectAnimator只负责把动画过程中的数值传到对应属性的set函数中就结束了，注意传给set函数以后就结束了！set函数就相当我们在ValueAnimator中添加的监听的作用，set函数中的对控件的操作还是需要我们自己来写的。

那我们来看看View中的setScaleY是怎么实现的吧：

```java
/** 
 * Sets the amount that the view is scaled in Y around the pivot point, as a proportion of 
 * the view's unscaled width. A value of 1 means that no scaling is applied. 
 * 
 * @param scaleY The scaling factor. 
 * @see #getPivotX() 
 * @see #getPivotY() 
 * 
 * @attr ref android.R.styleable#View_scaleY 
 */  
public void setScaleY(float scaleY) {  
    ensureTransformationInfo();  
    final TransformationInfo info = mTransformationInfo;  
    if (info.mScaleY != scaleY) {  
        invalidateParentCaches();  
        // Double-invalidation is necessary to capture view's old and new areas  
        invalidate(false);  
        info.mScaleY = scaleY;  
        info.mMatrixDirty = true;  
        mPrivateFlags |= DRAWN; // force another invalidation with the new orientation  
        invalidate(false);  
    }  
}  
```

大家不必理解这一坨代码的意义，因为这些代码是需要读懂View的整体流程以后才能看得懂的，只需要跟着我的步骤来理解就行。
这段代码总共分为两部分：第一步重新设置当前控件的参数，第二步调用Invalidate()强制重绘； 
所以在重绘时，控件就会根据最新的控件参数来绘制了，所以我们就看到当前控件被缩放了。 
**(4)、set函数调用频率是多少：**由于我们知道动画在进行时，每隔十几毫秒会刷新一次，所以我们的set函数也会每隔十几毫秒会被调用一次。 

讲了这么多，就是为了强调一点：**ObjectAnimator只负责把当前运动动画的数值传给set函数**。至于set函数里面怎么来做，是我们自己的事了。 

好了，在知道了ObjectAnimator的原理以后，下面就来看看如何自定义一个ObjectAnimator的属性吧。

## 自定义ObjectAnimator属性

我们在开始之前再来捋一下**ObjectAnimator的动画设置流程**：
ObjectAnimator需要指定操作的控件对象，在开始动画时，到控件类中去寻找设置属性所对应的set函数，然后把动画中间值做为参数传给这个set函数并执行它。 

所以，控件类中必须所要设置属性所要对应的set函数。所以为了自由控制控件的实现，我们这里自定义一个控件。大家知道在这个自定义控件中，肯定存在一个set函数与我们自定义的属性相对应。 
我们先来看看这段要实现的效果：
[![](http://upload-images.jianshu.io/upload_images/9028834-3b49afa335a17a31?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-3b49afa335a17a31?imageMogr2/auto-orient/strip)

这个控件中存在一个圆形，在动画时先将这个圆形放大，然后再将圆形还原。

### 1、保存圆形信息类——Point

为了，保存圆形的信息，我们先定义一个类：(Point.java)

```java
public class Point {  
    private int mRadius;    
    public Point(int radius){  
        mRadius = radius;  
    }    
    public int getRadius() {  
        return mRadius;  
    }    
    public void setRadius(int radius) {  
        mRadius = radius;  
    }  
}  
```

这个类很好理解，只有一个成员变量mRadius,表示圆的半径。

### 2、自定义控件——MyPointView

然后我们自定义一个控件MyPointView,完整代码如下：

```java
public class MyPointView extends View {  
    private Point mPoint = new Point(100);  
  
    public MyPointView(Context context, AttributeSet attrs) {  
        super(context, attrs);  
    }  
  
    @Override  
    protected void onDraw(Canvas canvas) {  
        if (mPoint != null){  
            Paint paint = new Paint();  
            paint.setAntiAlias(true);  
            paint.setColor(Color.RED);  
            paint.setStyle(Paint.Style.FILL);  
            canvas.drawCircle(300,300,mPoint.getRadius(),paint);  
        }  
        super.onDraw(canvas);  
    }  
  
    void setPointRadius(int radius){  
        mPoint.setRadius(radius);  
        invalidate();  
    }    
}  
```

在这段代码中，首先来看我们前面讲到的set函数：

```java
void setPointRadius(int radius){  
    mPoint.setRadius(radius);  
    invalidate();  
}  
```

- 第一点，这个set函数所对应的属性应该是pointRadius或者PointRadius。前面我们已经讲了第一个字母大小写无所谓，后面的字母必须保持与set函数完全一致。 
- 第二点，在setPointRadius中，先将当前动画传过来的值保存到mPoint中，做为当前圆形的半径。然后强制界面刷新 

在界面刷新后，就开始执行onDraw()函数：

```java
@Override  
protected void onDraw(Canvas canvas) {  
    if (mPoint != null){  
        Paint paint = new Paint();  
        paint.setAntiAlias(true);  
        paint.setColor(Color.RED);  
        paint.setStyle(Paint.Style.FILL);  
        canvas.drawCircle(300,300,mPoint.getRadius(),paint);  
    }  
    super.onDraw(canvas);  
}  
```

在onDraw函数中，就是根据当前mPoint的半径值在(300,300)点外画一个圆；有关画圆的知识，大家可以参考[《android Graphics（一）：概述及基本几何图形绘制》](http://blog.csdn.net/harvic880925/article/details/38875149)

### 3、使用MyPointView

首先，在MyActivity的布局中添加MyPointView的使用(main.xml)：

```xml
<?xml version="1.0" encoding="utf-8"?>  
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"  
                android:orientation="vertical"  
                android:layout_width="fill_parent"  
                android:layout_height="fill_parent">    
    <Button  
            android:id="@+id/btn"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:layout_alignParentLeft="true"  
            android:padding="10dp"  
            android:text="start anim"  
            />    
    <Button  
            android:id="@+id/btn_cancel"  
            android:layout_width="wrap_content"  
            android:layout_height="wrap_content"  
            android:layout_alignParentRight="true"  
            android:padding="10dp"  
            android:text="cancel anim"  
            />  
    <TextView  
            android:id="@+id/tv"  
            android:layout_width="100dp"  
            android:layout_height="wrap_content"  
            android:layout_centerHorizontal="true"  
            android:gravity="center"  
            android:padding="10dp"  
            android:background="#ffff00"  
            android:text="Hello qijian"/>    
    <com.example.BlogObjectAnimator1.MyPointView  
            android:id="@+id/pointview"  
            android:layout_width="match_parent"  
            android:layout_height="match_parent"  
            android:layout_below="@id/tv"/>    
</RelativeLayout>  
```

然后看看在MyActivity中，点击start anim后的处理方法：

```java
public class MyActivity extends Activity {  
    private Button btnStart;  
    private MyPointView mPointView;  
  
    @Override  
    public void onCreate(Bundle savedInstanceState) {  
        super.onCreate(savedInstanceState);  
        setContentView(R.layout.main);  
  
        btnStart = (Button) findViewById(R.id.btn);  
        mPointView = (MyPointView)findViewById(R.id.pointview);  
  
        btnStart.setOnClickListener(new View.OnClickListener() {  
            @Override  
            public void onClick(View v) {  
                doPointViewAnimation();  
            }  
        });  
    }  
  …………  
}       
```

在点击start anim按钮后，开始执行doPointViewAnimation()函数，doPointViewAnimation()函数代码如下：

```java
private void doPointViewAnimation(){  
     ObjectAnimator animator = ObjectAnimator.ofInt(mPointView, "pointRadius", 0, 300, 100);  
      animator.setDuration(2000);  
      animator.start();  
}  
```

在这段代码中，着重看ObjectAnimator的构造方法，首先要操作的控件对象是mPointView，然后对应的属性是pointRadius，然后值是从0到300再到100； 
所以在动画开始以后，ObjectAnimator就会实时地把动画中产生的值做为参数传给MyPointView类中的setPointRadius(int radius)函数，然后调用setPointRadius(int radius)。由于我们在setPointRadius(int radius)中实时地设置圆形的半径值然后强制重绘当前界面，所以可以看到圆形的半径会随着动画的进行而改变。 


### 4、注意——何时需要实现对应属性的get函数 

我们再来看一下ObjectAinimator的下面三个构造方法：

```java
public static ObjectAnimator ofFloat(Object target, String propertyName, float... values)  
public static ObjectAnimator ofInt(Object target, String propertyName, int... values)  
public static ObjectAnimator ofObject(Object target, String propertyName,TypeEvaluator evaluator, Object... values)  
```

前面我们已经分别讲过三个函数的使用方法，在上面的三个构造方法中最后一个参数都是可变长参数。我们也讲了，他们的意义就是从哪个值变到哪个值的。

那么问题来了：前面我们都是定义多个值，即至少两个值之间的变化，那如果我们只定义一个值呢，如下面的方式：(同样以MyPointView为例)
```java
ObjectAnimator animator = ObjectAnimator.ofInt(mPointView, "pointRadius",100);  
```

我们在这里只传递了一个变化值100；那它从哪里开始变化呢？我们来看一下效果：
代码如下：

```java
ObjectAnimator animator = ObjectAnimator.ofInt(mPointView, "pointRadius",100);  
animator.setDuration(2000);  
animator.start();  
```

效果图如下：
[![](http://upload-images.jianshu.io/upload_images/9028834-cc791bdf1143213c?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-cc791bdf1143213c?imageMogr2/auto-orient/strip)

从效果图中看起来是从0开始的，但是看log可以看出来已经在出警告了：
[![](http://upload-images.jianshu.io/upload_images/9028834-1aca1713e9063def?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)](http://upload-images.jianshu.io/upload_images/9028834-1aca1713e9063def?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们点了三次start anim按钮，所以这里也报了三次，意思就是没找到pointRadius属性所对应的getPointRadius()函数；

**仅当我们只给动画设置一个值时，程序才会调用属性对应的get函数来得到动画初始值**。如果动画**没有初始值，那么就会使用系统默认值**。比如ofInt（）中使用的参数类型是int类型的，而系统的Int值的默认值是0，所以动画就会从0运动到100；也就是系统虽然在找到不到属性对应的get函数时，会给出警告，但同时会用系统默认值做为动画初始值。

如果通过给自定义控件MyPointView设置了get函数，那么将会以get函数的返回值做为初始值：

```java
public class MyPointView extends View {  
    private Point mPoint = new Point(100);    
    public MyPointView(Context context, AttributeSet attrs) {  
        super(context, attrs);  
    }    
    @Override  
    protected void onDraw(Canvas canvas) {  
        if (mPoint != null){  
            Paint paint = new Paint();  
            paint.setAntiAlias(true);  
            paint.setColor(Color.RED);  
            paint.setStyle(Paint.Style.FILL);  
            canvas.drawCircle(300,300,mPoint.getRadius(),paint);  
        }  
        super.onDraw(canvas);  
    }  
  
    public int getPointRadius(){  
        return 50;  
    }    
    public void setPointRadius(int radius){  
        mPoint.setRadius(radius);  
        invalidate();  
    }    
}  
```

我们在这里添加了getPointRadius函数，返回值是Int.有些同学可能会疑惑：我怎么知道这里要返回int值呢？
我们前面说过当且仅当我们在创建ObjectAnimator时，只给他传递了一个过渡值的时候，系统才会调用属性对应的get函数来得到动画的初始值！所以做为动画的初始值，那么在创建动画时过渡值传的什么类型，这里的get函数就要返回类型

```java
public static ObjectAnimator ofObject(Object target, String propertyName,TypeEvaluator evaluator, Object... values)  
```
比如上面的ofObject,get函数所返回的类型就是与最后一个参数Object... values，相同类型的。
在我们在MyPointView添加上PointRadius所对应的get函数以后重新执行动画：

```java
ObjectAnimator animator = ObjectAnimator.ofInt(mPointView, "pointRadius",100);  
animator.setDuration(2000);  
animator.start();  
```

此时的效果图如下：
[![](http://upload-images.jianshu.io/upload_images/9028834-99f3250637517e13?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-99f3250637517e13?imageMogr2/auto-orient/strip)

从动画中可以看出，半径已经不是从0开始的了，而是从50开始的。

**最后我们总结一下：当且仅当动画的只有一个过渡值时，系统才会调用对应属性的get函数来得到动画的初始值。**


## 常用函数

有关常用函数这一节其实没有太多讲的必要。因为ObjectAnimator的函数都是从ValueAnimator中继承而来的，所以用法和效果与ValueAnimator是完全一样的。我们这里只讲解一下Evaluator的用法，其它的也就不再讲了。

### 1、使用ArgbEvaluator

我们搜一下TextView所有的函数发现，TextView有一个set函数能够改变背景色：

```java
public void setBackgroundColor(int color);  
```

大家可以回想到，我们在ValueAnimator中也曾改变过背景色，使用的是ArgbEvaluator。在这里我们再回顾下ArgbEvaluator，它的实现代码如下：

```java
public class ArgbEvaluator implements TypeEvaluator {  
    public Object evaluate(float fraction, Object startValue, Object endValue) {  
        int startInt = (Integer) startValue;  
        int startA = (startInt >> 24);  
        int startR = (startInt >> 16) & 0xff;  
        int startG = (startInt >> 8) & 0xff;  
        int startB = startInt & 0xff;  
  
        int endInt = (Integer) endValue;  
        int endA = (endInt >> 24);  
        int endR = (endInt >> 16) & 0xff;  
        int endG = (endInt >> 8) & 0xff;  
        int endB = endInt & 0xff;  
  
        return (int)((startA + (int)(fraction * (endA - startA))) << 24) |  
                (int)((startR + (int)(fraction * (endR - startR))) << 16) |  
                (int)((startG + (int)(fraction * (endG - startG))) << 8) |  
                (int)((startB + (int)(fraction * (endB - startB))));  
    }  
}  
```

ArgbEvaluator的返回值是Integer类型，所以我们要使用ArgbEvaluator的话，构造ObjectAnimator时必须使用ofInt() 

下面我们来看看**使用ArgbEvaluator的代码**：

```java
ObjectAnimator animator = ObjectAnimator.ofInt(tv, "BackgroundColor", 0xffff00ff, 0xffffff00, 0xffff00ff);  
animator.setDuration(8000);  
animator.setEvaluator(new ArgbEvaluator());  
animator.start();  
```

然后我们来看下代码效果： 
[![image](http://upload-images.jianshu.io/upload_images/9028834-a3d4cd2dd4c4fa64?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-a3d4cd2dd4c4fa64?imageMogr2/auto-orient/strip)



### 2、其它函数

下面把其它所涉及到的函数的列表列在下面，大家可以参考ValueAnimator的使用方法来使用。

#### （1）、常用函数

```java
/* 设置动画时长，单位是毫秒  */  
ValueAnimator setDuration(long duration)  
/* 获取ValueAnimator在运动时，当前运动点的值  */  
Object getAnimatedValue();  
/* 开始动画  */  
void start()  
/* 设置循环次数,设置为INFINITE表示无限循环  */  
void setRepeatCount(int value)  
/* 设置循环模式 
 * value取值有RESTART，REVERSE，  */  
void setRepeatMode(int value)  
/* 取消动画  */  
void cancel()  
```

#### （2）、监听器相关

```java
/* 监听器一：监听动画变化时的实时值  */  
public static interface AnimatorUpdateListener {  
    void onAnimationUpdate(ValueAnimator animation);  
}  
//添加方法为：public void addUpdateListener(AnimatorUpdateListener listener)  

/* 监听器二：监听动画变化时四个状态  */  
public static interface AnimatorListener {  
    void onAnimationStart(Animator animation);  
    void onAnimationEnd(Animator animation);  
    void onAnimationCancel(Animator animation);  
    void onAnimationRepeat(Animator animation);  
}  
//添加方法为：public void addListener(AnimatorListener listener)   
```

#### （3）、插值器与Evaluator

```java
/* 设置插值器  */  
public void setInterpolator(TimeInterpolator value)  
/* 设置Evaluator  */  
public void setEvaluator(TypeEvaluator value)  
```

**源码下载地址：**
**github:[https://github.com/harvic/BlogResForGitHub](https://github.com/harvic/BlogResForGitHub)**

## 习题：自定义View一边动一边变色

### 实现目标

动态改变View的颜色。
效果图：
[![效果图](http://img.blog.csdn.net/20171221162602094?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)](http://img.blog.csdn.net/20171221162602094?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### 实现步骤

#### 自定义MyAnimView属性

　　ObjectAnimator内部的工作机制是通过寻找特定属性的get和set方法，然后通过方法不断地对值进行改变，从而实现动画效果的。
　　因此我们就需要在MyAnimView中**定义一个color属性，并提供它的get和set方法**。这里我们可以将color属性设置为字符串类型，使用#RRGGBB这种格式来表示颜色值，代码如下所示：
　　在setColor()方法当中，将画笔的颜色设置成方法参数传入的颜色，然后调用了invalidate()方法。即在改变了画笔颜色之后立即刷新视图，然后onDraw()方法就会调用。在onDraw()方法当中会根据当前画笔的颜色来进行绘制，这样颜色也就会动态进行改变了。

直接使用【习题：对自定义View使用动画】中的MyAnimView
```java
public class MyAnimView extends View {    
    ...  
  
    private String color;    
    public String getColor() {  
        return color;  
    }    
    public void setColor(String color) {  
        this.color = color;  
        mPaint.setColor(Color.parseColor(color));  
        invalidate();  
    }    
    ...    
}  
```

#### 自定义TypeEvaluator：

　　要借助ObjectAnimator类让setColor()方法得到调用了，在使用ObjectAnimator之前我们还要完成一个非常重要的工作，就是编写一个用于**告知系统如何进行颜色过度的TypeEvaluator**。
创建ColorEvaluator并实现TypeEvaluator接口，代码如下所示：
　　首先在evaluate()方法当中获取到颜色的初始值和结束值，并通过字符串截取的方式将颜色分为RGB三个部分，并将RGB的值转换成十进制数字，那么每个颜色的取值范围就是0-255。接下来计算一下初始颜色值到结束颜色值之间的差值，这个差值很重要，决定着颜色变化的快慢，如果初始颜色值和结束颜色值很相近，那么颜色变化就会比较缓慢，而如果颜色值相差很大，比如说从黑到白，那么就要经历255*3这个幅度的颜色过度，变化就会非常快。

　　那么控制颜色变化的速度是通过getCurrentColor()这个方法来实现的，这个方法会根据当前的fraction值来计算目前应该过度到什么颜色，并且这里会根据初始和结束的颜色差值来控制变化速度，最终将计算出的颜色进行返回。

　　最后，由于我们计算出的颜色是十进制数字，这里还需要调用一下getHexString()方法把它们转换成十六进制字符串，再将RGB颜色拼装起来之后作为最终的结果返回。

```java
public class ColorEvaluator implements TypeEvaluator {    
    private int mCurrentRed = -1;    
    private int mCurrentGreen = -1;    
    private int mCurrentBlue = -1;    
    @Override  
    public Object evaluate(float fraction, Object startValue, Object endValue) {  
        String startColor = (String) startValue;  
        String endColor = (String) endValue;  
        int startRed = Integer.parseInt(startColor.substring(1, 3), 16);  
        int startGreen = Integer.parseInt(startColor.substring(3, 5), 16);  
        int startBlue = Integer.parseInt(startColor.substring(5, 7), 16);  
        int endRed = Integer.parseInt(endColor.substring(1, 3), 16);  
        int endGreen = Integer.parseInt(endColor.substring(3, 5), 16);  
        int endBlue = Integer.parseInt(endColor.substring(5, 7), 16);  
        // 初始化颜色的值  
        if (mCurrentRed == -1) {  
            mCurrentRed = startRed;  
        }  
        if (mCurrentGreen == -1) {  
            mCurrentGreen = startGreen;  
        }  
        if (mCurrentBlue == -1) {  
            mCurrentBlue = startBlue;  
        }  
        // 计算初始颜色和结束颜色之间的差值  
        int redDiff = Math.abs(startRed - endRed);  
        int greenDiff = Math.abs(startGreen - endGreen);  
        int blueDiff = Math.abs(startBlue - endBlue);  
        int colorDiff = redDiff + greenDiff + blueDiff;  
        if (mCurrentRed != endRed) {  
            mCurrentRed = getCurrentColor(startRed, endRed, colorDiff, 0,  
                    fraction);  
        } else if (mCurrentGreen != endGreen) {  
            mCurrentGreen = getCurrentColor(startGreen, endGreen, colorDiff,  
                    redDiff, fraction);  
        } else if (mCurrentBlue != endBlue) {  
            mCurrentBlue = getCurrentColor(startBlue, endBlue, colorDiff,  
                    redDiff + greenDiff, fraction);  
        }  
        // 将计算出的当前颜色的值组装返回  
        String currentColor = "#" + getHexString(mCurrentRed)  
                + getHexString(mCurrentGreen) + getHexString(mCurrentBlue);  
        return currentColor;  
    }  
  
    /* 根据fraction值来计算当前的颜色。      */  
    private int getCurrentColor(int startColor, int endColor, int colorDiff,  
            int offset, float fraction) {  
        int currentColor;  
        if (startColor > endColor) {  
            currentColor = (int) (startColor - (fraction * colorDiff - offset));  
            if (currentColor < endColor) {  
                currentColor = endColor;  
            }  
        } else {  
            currentColor = (int) (startColor + (fraction * colorDiff - offset));  
            if (currentColor > endColor) {  
                currentColor = endColor;  
            }  
        }  
        return currentColor;  
    }  
      
    /* 将10进制颜色值转换成16进制。      */  
    private String getHexString(int value) {  
        String hexString = Integer.toHexString(value);  
        if (hexString.length() == 1) {  
            hexString = "0" + hexString;  
        }  
        return hexString;  
    }  
  
}  
```

#### 调用

比如说我们想要实现从蓝色到红色的动画过度，历时5秒，就可以这样写：

```java
ObjectAnimator anim = ObjectAnimator.ofObject(myAnimView, "color", new ColorEvaluator(),   
    "#0000FF", "#FF0000");  
anim.setDuration(5000);  
anim.start();  
```

接下来我们需要将上面一段代码移到MyAnimView类当中，让它和刚才的Point移动动画可以结合到一起播放，这就要借助**组合动画**的技术了。修改MyAnimView中的代码，如下所示：

```java
public class MyAnimView extends View {    
    ...    
    private void startAnimation() {  
        Point startPoint = new Point(RADIUS, RADIUS);  
        Point endPoint = new Point(getWidth() - RADIUS, getHeight() - RADIUS);  
        ValueAnimator anim = ValueAnimator.ofObject(new PointEvaluator(), startPoint, endPoint);  
        anim.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {  
            @Override  
            public void onAnimationUpdate(ValueAnimator animation) {  
                currentPoint = (Point) animation.getAnimatedValue();  
                invalidate();  
            }  
        });  
        ObjectAnimator anim2 = ObjectAnimator.ofObject(this, "color", new ColorEvaluator(),   
                "#0000FF", "#FF0000");  
        //组合动画AnimatorSet 
        AnimatorSet animSet = new AnimatorSet();  
        animSet.play(anim).with(anim2);  
        animSet.setDuration(5000);  
        animSet.start();  
    }    
}  
```
由于这段代码本身就是在MyAnimView当中执行的，因此ObjectAnimator.ofObject()的第一个参数直接传this就可以了。接着我们又创建了一个AnimatorSet，并把两个动画设置成同时播放，动画时长为五秒，最后启动动画。








# Interpolator插值器

Interpolator这个东西很难进行翻译，直译过来的话是插值器的意思，它的主要作用是可以**控制动画的变化速率**，比如去实现一种非线性运动的动画效果。那么什么叫做非线性运动的动画效果呢？就是说动画改变的速率不是一成不变的，像加速运动以及减速运动都属于非线性运动。

不过Interpolator并不是属性动画中新增的技术，实际上从Android 1.0版本开始就一直存在Interpolator接口了，而之前的补间动画当然也是支持这个功能的。只不过在**属性动画**中**新增**了一个**TimeInterpolator**接口，这个接口是用于兼容之前的Interpolator的，这使得所有过去的Interpolator实现类都可以直接拿过来放到属性动画当中使用。

TimeInterpolator接口的所有实现类：
![](http://img.blog.csdn.net/20171221225916611?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

TimeInterpolator接口已经有非常多的实现类了，这些都是Android系统内置好的并且我们可以直接使用的Interpolator。每个Interpolator都有它各自的实现效果，比如说AccelerateInterpolator就是一个加速运动的Interpolator，而DecelerateInterpolator就是一个减速运动的Interpolator。

## Interpolator常见实现类

[![Interpolator实现类](http://img.blog.csdn.net/20180117211154977?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)](http://img.blog.csdn.net/20180117211154977?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

| 实现类                              | 意义                          |
| -------------------------------- | --------------------------- |
| AccelerateDecelerateInterpolator | 在动画开始与介绍的地方速率改变比较慢，在中间的时候加速 |
| AccelerateInterpolator           | 在动画开始的地方速率改变比较慢，然后开始加速      |
| AnticipateInterpolator           | 开始的时候向后然后向前甩                |
| AnticipateOvershootInterpolator  | 开始的时候向后然后向前甩一定值后返回最后的值      |
| BounceInterpolator               | 动画结束的时候弹起                   |
| CycleInterpolator                | 动画循环播放特定的次数，速率改变沿着正弦曲线      |
| DecelerateInterpolator           | 在动画开始的地方快然后慢                |
| LinearInterpolator               | 以常量速率改变                     |
| OvershootInterpolator            | 向前甩一定值后再回到原来位置              |

## 补间动画中使用Interpolator

### scale标签

下面先看看Scale标签应用插值器后，都会变成什么样。

先看下XML代码：（从控件中心点，从0放大到1.4倍，保持结束时的状态）

```xml
<?xml version="1.0" encoding="utf-8"?>  
<scale xmlns:android="http://schemas.android.com/apk/res/android"  
    android:interpolator="@android:anim/accelerate_decelerate_interpolator"  
    android:fromXScale="0.0"  
    android:toXScale="1.4"  
    android:fromYScale="0.0"  
    android:toYScale="1.4"  
    android:pivotX="50%"  
    android:pivotY="50%"  
    android:duration="700"   
    android:fillAfter="true"  
/>  
```

下面一个个看看，每个xml值对应的scale动画是怎样的。

[![image](http://upload-images.jianshu.io/upload_images/9028834-9641948c4c015331?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-9641948c4c015331?imageMogr2/auto-orient/strip)

AccelerateInterpolator 在动画开始的地方速率改变比较慢，然后开始加速
[![image](http://upload-images.jianshu.io/upload_images/9028834-cd8bd284644b33b7?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-cd8bd284644b33b7?imageMogr2/auto-orient/strip)

DecelerateInterpolator 在动画开始的地方快然后慢
[![image](http://upload-images.jianshu.io/upload_images/9028834-f769b2a42889de76?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-f769b2a42889de76?imageMogr2/auto-orient/strip)

AnticipateInterpolator 开始的时候向后然后向前甩
[![image](http://upload-images.jianshu.io/upload_images/9028834-bacba45cacbf6314?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-bacba45cacbf6314?imageMogr2/auto-orient/strip)

AnticipateOvershootInterpolator 开始的时候向后然后向前甩一定值后返回最后的值
[![image](http://upload-images.jianshu.io/upload_images/9028834-25f6a82370c5eba0?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-25f6a82370c5eba0?imageMogr2/auto-orient/strip)

BounceInterpolator 动画结束的时候弹起
[![image](http://upload-images.jianshu.io/upload_images/9028834-9e4fd6c029c49c72?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-9e4fd6c029c49c72?imageMogr2/auto-orient/strip)

CycleInterpolator 动画循环播放特定的次数，速率改变沿着正弦曲线
[![image](http://upload-images.jianshu.io/upload_images/9028834-f2ad8e5d65a346c2?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-f2ad8e5d65a346c2?imageMogr2/auto-orient/strip)

LinearInterpolator 以常量速率改变
[![image](http://upload-images.jianshu.io/upload_images/9028834-b888a91e4c1b7473?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-b888a91e4c1b7473?imageMogr2/auto-orient/strip)

OvershootInterpolator 向前甩一定值后再回到原来位置
[![image](http://upload-images.jianshu.io/upload_images/9028834-2075c3c8ed220462?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-2075c3c8ed220462?imageMogr2/auto-orient/strip)

### rotate标签

下面先看看rotate标签应用插值器后，都会变成什么样。

先看下XML代码：（从控件中心点，从0放大到1.4倍，保持结束时的状态）

```xml
<?xml version="1.0" encoding="utf-8"?>  
<rotate xmlns:android="http://schemas.android.com/apk/res/android"  
    android:interpolator="@android:anim/accelerate_decelerate_interpolator"  
    android:fromDegrees="0"  
    android:toDegrees="360"  
    android:pivotX="50%"  
    android:pivotY="50%"  
    android:duration="700"   
    android:fillAfter="true"  
/>  
```

AccelerateDecelerateInterpolator   在动画开始与结束的地方速率改变比较慢，在中间的时候加速
[![image](http://upload-images.jianshu.io/upload_images/9028834-c630f63c76cf169a?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-c630f63c76cf169a?imageMogr2/auto-orient/strip)

AccelerateInterpolator 在动画开始的地方速率改变比较慢，然后开始加速
[![image](http://upload-images.jianshu.io/upload_images/9028834-32145a2f95789380?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-32145a2f95789380?imageMogr2/auto-orient/strip)

DecelerateInterpolator 在动画开始的地方快然后慢
[![image](http://upload-images.jianshu.io/upload_images/9028834-4f6cd07f3fb3a96a?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-4f6cd07f3fb3a96a?imageMogr2/auto-orient/strip)

AnticipateInterpolator 开始的时候向后然后向前甩
[![image](http://upload-images.jianshu.io/upload_images/9028834-0a05e2e01a7ecc81?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-0a05e2e01a7ecc81?imageMogr2/auto-orient/strip)

AnticipateOvershootInterpolator 开始的时候向后然后向前甩一定值后返回最后的值
[![image](http://upload-images.jianshu.io/upload_images/9028834-44ba06378f706507?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-44ba06378f706507?imageMogr2/auto-orient/strip)

BounceInterpolator 动画结束的时候弹起
[![image](http://upload-images.jianshu.io/upload_images/9028834-e4c5e12a5012138e?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-e4c5e12a5012138e?imageMogr2/auto-orient/strip)
CycleInterpolator 动画循环播放特定的次数，速率改变沿着正弦曲线
[![image](http://upload-images.jianshu.io/upload_images/9028834-ca90a65293512544?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-ca90a65293512544?imageMogr2/auto-orient/strip)

LinearInterpolator 以常量速率改变
[![image](http://upload-images.jianshu.io/upload_images/9028834-a35e678f3f7894cf?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-a35e678f3f7894cf?imageMogr2/auto-orient/strip)

OvershootInterpolator 向前甩一定值后再回到原来位置
[![image](http://upload-images.jianshu.io/upload_images/9028834-cc7358a08cf91248?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-cc7358a08cf91248?imageMogr2/auto-orient/strip)

### alpha标签

下面先看看alpha标签应用插值器后，都会变成什么样。

将透明度从0变成1.0，使用不同的插值器看看有什么不同（因为只是透明度的变化，所以基本看不出来有什么不同）

```xml
<?xml version="1.0" encoding="utf-8"?>  
<alpha xmlns:android="http://schemas.android.com/apk/res/android"  
    android:interpolator="@android:anim/accelerate_decelerate_interpolator"  
    android:fromAlpha="0.0"  
    android:toAlpha="1.0"  
    android:duration="3000"   
    android:fillAfter="true"  
/>  
```

AccelerateDecelerateInterpolator   在动画开始与介绍的地方速率改变比较慢，在中间的时候加速
[![image](http://upload-images.jianshu.io/upload_images/9028834-c1e12d6347797678?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-c1e12d6347797678?imageMogr2/auto-orient/strip)

AccelerateInterpolator 在动画开始的地方速率改变比较慢，然后开始加速
[![image](http://upload-images.jianshu.io/upload_images/9028834-33502f51c99ea8cd?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-33502f51c99ea8cd?imageMogr2/auto-orient/strip)

DecelerateInterpolator 在动画开始的地方快然后慢
[![image](http://upload-images.jianshu.io/upload_images/9028834-f70e63b02aa41b07?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-f70e63b02aa41b07?imageMogr2/auto-orient/strip)

AnticipateInterpolator 开始的时候向后然后向前甩
[![image](http://upload-images.jianshu.io/upload_images/9028834-ea298210d5a74356?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-ea298210d5a74356?imageMogr2/auto-orient/strip)

AnticipateOvershootInterpolator 开始的时候向后然后向前甩一定值后返回最后的值
[![image](http://upload-images.jianshu.io/upload_images/9028834-dd9aeae1d00ed4ce?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-dd9aeae1d00ed4ce?imageMogr2/auto-orient/strip)

BounceInterpolator 动画结束的时候弹起
[![image](http://upload-images.jianshu.io/upload_images/9028834-aa33ed3dd58ae787?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-aa33ed3dd58ae787?imageMogr2/auto-orient/strip)

CycleInterpolator 动画循环播放特定的次数，速率改变沿着正弦曲线
[![image](http://upload-images.jianshu.io/upload_images/9028834-84fc53abdca3eac7?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-84fc53abdca3eac7?imageMogr2/auto-orient/strip)

LinearInterpolator 以常量速率改变
[![image](http://upload-images.jianshu.io/upload_images/9028834-5e6d30c1d702d74c?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-5e6d30c1d702d74c?imageMogr2/auto-orient/strip)

OvershootInterpolator 向前甩一定值后再回到原来位置
[![image](http://upload-images.jianshu.io/upload_images/9028834-521ea1e6b27ecda8?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-521ea1e6b27ecda8?imageMogr2/auto-orient/strip)

### translate标签

下面先看看translate标签应用插值器后，都会变成什么样。

把控件从（0，0）平移到（-200，-200）的位置，保持结束时状态不变，使用不同插值器。

```xml
<?xml version="1.0" encoding="utf-8"?>  
<translate xmlns:android="http://schemas.android.com/apk/res/android"  
    android:interpolator="@android:anim/accelerate_decelerate_interpolator"  
    android:fromXDelta="0"     
    android:toXDelta="-200"    
    android:fromYDelta="0"    
    android:toYDelta="-200"    
    android:duration="2000"    
    android:fillAfter="true"  
/>  
```

AccelerateDecelerateInterpolator   在动画开始与介绍的地方速率改变比较慢，在中间的时候加速
[![image](http://upload-images.jianshu.io/upload_images/9028834-dba57e97857fae78?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-dba57e97857fae78?imageMogr2/auto-orient/strip)

AccelerateInterpolator 在动画开始的地方速率改变比较慢，然后开始加速
[![image](http://upload-images.jianshu.io/upload_images/9028834-3fa3107022b0da74?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-3fa3107022b0da74?imageMogr2/auto-orient/strip)

DecelerateInterpolator 在动画开始的地方快然后慢
[![image](http://upload-images.jianshu.io/upload_images/9028834-2611492836745934?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-2611492836745934?imageMogr2/auto-orient/strip)

AnticipateInterpolator 开始的时候向后然后向前甩
[![image](http://upload-images.jianshu.io/upload_images/9028834-2a5be60e84863285?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-2a5be60e84863285?imageMogr2/auto-orient/strip)

AnticipateOvershootInterpolator 开始的时候向后然后向前甩一定值后返回最后的值
[![image](http://upload-images.jianshu.io/upload_images/9028834-4563e39c9d0d23b0?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-4563e39c9d0d23b0?imageMogr2/auto-orient/strip)

BounceInterpolator 动画结束的时候弹起
[![image](http://upload-images.jianshu.io/upload_images/9028834-21758e835f26653a?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-21758e835f26653a?imageMogr2/auto-orient/strip)
CycleInterpolator 动画循环播放特定的次数，速率改变沿着正弦曲线
[![image](http://upload-images.jianshu.io/upload_images/9028834-32be87dac2c317ca?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-32be87dac2c317ca?imageMogr2/auto-orient/strip)

LinearInterpolator 以常量速率改变
[![image](http://upload-images.jianshu.io/upload_images/9028834-226b09b9bd1db962?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-226b09b9bd1db962?imageMogr2/auto-orient/strip)

OvershootInterpolator 向前甩一定值后再回到原来位置
[![image](http://upload-images.jianshu.io/upload_images/9028834-09ccea09569eabe4?imageMogr2/auto-orient/strip)](http://upload-images.jianshu.io/upload_images/9028834-09ccea09569eabe4?imageMogr2/auto-orient/strip)







## AccelerateDecelerateInterpolator

上文使用ValueAnimator所打印的值如下所示：
![](http://img.blog.csdn.net/20171221230833425?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

可以看到，一开始的值变化速度明显比较慢，仅0.0开头的就打印了4次，之后开始加速，最后阶段又开始减速，因此我们可以很明显地看出这一个先加速后减速的Interpolator。

上文中的小球也是：一开始运动速度比较慢，然后逐渐加速，中间的部分运动速度就比较快，接下来开始减速，最后缓缓停住。另外颜色变化也是这种规律，一开始颜色变化的比较慢，中间颜色变化的很快，最后阶段颜色变化的又比较慢。

从以上几点我们就可以总结出一个结论了，**使用属性动画时，系统默认的Interpolator其实就是一个先加速后减速的Interpolator，对应的实现类就是AccelerateDecelerateInterpolator**。

我们可以很轻松地修改这一默认属性，将它替换成任意一个系统内置好的Interpolator。
比如，【习题：对自定义View使用动画】中MyAnimView中的startAnimation()方法是开启动画效果的入口，这里我们对Point对象的坐标稍做一下修改，让它变成一种垂直掉落的效果，代码如下所示：
对Point构造函数中的坐标值进行了一下改动，那么现在小球运动的动画效果应该是从屏幕正中央的顶部掉落到底部。

```java
private void startAnimation() {  
    Point startPoint = new Point(getWidth() / 2, RADIUS);  
    Point endPoint = new Point(getWidth() / 2, getHeight() - RADIUS);  
    ValueAnimator anim = ValueAnimator.ofObject(new PointEvaluator(), startPoint, endPoint);  
    anim.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {  
        @Override  
        public void onAnimationUpdate(ValueAnimator animation) {  
            currentPoint = (Point) animation.getAnimatedValue();  
            invalidate();  
        }  
    });  
    anim.setDuration(2000);  
    anim.start();  
}  
```

但是默认情况下小球的下降速度肯定是先加速后减速的，我们需要改为下降速度越来越快的。
调用**Animator的setInterpolator()**方法就可以了，这个方法要求**传入**一个实现**TimeInterpolator**接口的实例，那么比如说我们想要实现小球下降越来越快的效果，就可以使用AccelerateInterpolator，代码如下所示：

```java
private void startAnimation() {  
    Point startPoint = new Point(getWidth() / 2, RADIUS);  
    Point endPoint = new Point(getWidth() / 2, getHeight() - RADIUS);  
    ValueAnimator anim = ValueAnimator.ofObject(new PointEvaluator(), startPoint, endPoint);  
    anim.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {  
        @Override  
        public void onAnimationUpdate(ValueAnimator animation) {  
            currentPoint = (Point) animation.getAnimatedValue();  
            invalidate();  
        }  
    });  
    anim.setInterpolator(new AccelerateInterpolator(2f));//实现小球下降越来越快  
    /*AccelerateInterpolator的构建函数可以接收一个float类型的参数，这个参数是用于控制加速度的*/
    anim.setDuration(2500);  
    anim.start();  
}  
```

效果如下：
![](http://img.blog.csdn.net/20171221231615443?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

## BounceInterpolator

现在要让小球撞击到地面之后应该要反弹起来，然后再次落下，接着再反弹起来，又再次落下，以此反复，最后静止。这个功能我们当然可以自己去写，只不过比较复杂，所幸的是，Android系统中已经提供好了这样一种Interpolator，我们只需要简单地替换一下就可以完成上面的描述的效果，代码如下所示：
将设置的Interpolator换成了BounceInterpolator的实例，而BounceInterpolator就是一种可以**模拟物理规律，实现反复弹起效果**的Interpolator。

```java
private void startAnimation() {  
    Point startPoint = new Point(getWidth() / 2, RADIUS);  
    Point endPoint = new Point(getWidth() / 2, getHeight() - RADIUS);  
    ValueAnimator anim = ValueAnimator.ofObject(new PointEvaluator(), startPoint, endPoint);  
    anim.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {  
        @Override  
        public void onAnimationUpdate(ValueAnimator animation) {  
            currentPoint = (Point) animation.getAnimatedValue();  
            invalidate();  
        }  
    });  
    anim.setInterpolator(new BounceInterpolator());  
    anim.setDuration(3000);  
    anim.start();  
}  
```

效果如下：
![](http://img.blog.csdn.net/20171221231840395?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

## Interpolator内部实现机制

首先看一下TimeInterpolator的接口定义，代码如下所示：

```java
/** 
 * A time interpolator defines the rate of change of an animation. This allows animations 
 * to have non-linear motion, such as acceleration and deceleration. 
 */  
public interface TimeInterpolator {  
  
    /** 
     * Maps a value representing the elapsed fraction of an animation to a value that represents 
     * the interpolated fraction. This interpolated value is then multiplied by the change in 
     * value of an animation to derive the animated value at the current elapsed animation time. 
     * 
     * @param input A value between 0 and 1.0 indicating our current point 
     *        in the animation where 0 represents the start and 1.0 represents 
     *        the end 
     * @return The interpolation value. This value can be more than 1.0 for 
     *         interpolators which overshoot their targets, or less than 0 for 
     *         interpolators that undershoot their targets. 
     */  
    float getInterpolation(float input);  
}  
```

只有一个getInterpolation()方法。
getInterpolation()方法中接收一个input参数，这个参数的值会随着动画的运行而不断变化，不过它的变化是非常有规律的，就是根据设定的动画时长**匀速增加**，变化范围是0到1。也就是说当动画一开始的时候input的值是0，到动画结束的时候input的值是1，而中间的值则是随着动画运行的时长在0到1之间变化的。

input的值决定了上文中fraction的值。
**input**的值是由**系统**经过**计算**后**传入**到**getInterpolation()**方法中的，然后我们可以自己实现getInterpolation()方法中的算法，根据input的值来计算出一个返回值，而这个**返回值就是fraction**了。
因此，最简单的情况就是input值和fraction值是相同的，这种情况由于input值是匀速增加的，因而fraction的值也是匀速增加的，所以动画的运动情况也是匀速的。

系统中内置的**LinearInterpolator**就是一种匀速运动的Interpolator，那么我们来看一下它的源码是怎么实现的：

```java
/** 
 * An interpolator where the rate of change is constant 
 */  
@HasNativeInterpolator  
public class LinearInterpolator extends BaseInterpolator implements NativeInterpolatorFactory {  
  
    public LinearInterpolator() {  
    }  
  
    public LinearInterpolator(Context context, AttributeSet attrs) {  
    }  
  
    public float getInterpolation(float input) {  
        return input;  //把参数中传递的input值直接返回了，因此fraction的值就是等于input的值的，这就是匀速运动的Interpolator的实现方式。
    }  
  
    /** @hide */  
    @Override  
    public long createNativeInterpolator() {  
        return NativeInterpolatorFactoryHelper.createLinearInterpolator();  
    }  
}  
```

当然这是最简单的一种Interpolator的实现了，我们再来看一个稍微复杂一点的。既然现在大家都知道了系统在默认情况下使用的是**AccelerateDecelerateInterpolator**，那我们就来看一下它的源码吧，如下所示：

```java
/** 
 * An interpolator where the rate of change starts and ends slowly but 
 * accelerates through the middle. 
 *  
 */  
@HasNativeInterpolator  
public class AccelerateDecelerateInterpolator implements Interpolator, NativeInterpolatorFactory {  
    public AccelerateDecelerateInterpolator() {  
    }  
      
    @SuppressWarnings({"UnusedDeclaration"})  
    public AccelerateDecelerateInterpolator(Context context, AttributeSet attrs) {  
    }  
      
    public float getInterpolation(float input) {  
        return (float)(Math.cos((input + 1) * Math.PI) / 2.0f) + 0.5f;  
        /*算法中主要使用了余弦函数，由于input的取值范围是0到1，那么cos函数中的取值范围就是π到2π。而cos(π)的结果是-1，cos(2π)的结果是1，那么这个值再除以2加上0.5之后，getInterpolation()方法最终返回的结果值还是在0到1之间。只不过经过了余弦运算之后，最终的结果不再是匀速增加的了，而是经历了一个先加速后减速的过程。*/
    }  
  
    /** @hide */  
    @Override  
    public long createNativeInterpolator() {  
        return NativeInterpolatorFactoryHelper.createAccelerateDecelerateInterpolator();  
    }  
}  
```

getInterpolation()方法中的逻辑变复杂了，我们可以将这个算法的执行情况通过曲线图的方式绘制出来，结果如下图所示：
![](http://img.blog.csdn.net/20171221232939801?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

可以看到，这是一个S型的曲线图，当横坐标从0变化到0.2的时候，纵坐标的变化幅度很小，但是之后就开始明显加速，最后横坐标从0.8变化到1的时候，纵坐标的变化幅度又变得很小。

## 自定义Interpolator

实现：先减速后加速Interpolator
新建DecelerateAccelerateInterpolator类，让它实现TimeInterpolator接口，代码如下所示：

```java
public class DecelerateAccelerateInterpolator implements TimeInterpolator{  
    @Override  
    public float getInterpolation(float input) {  
        float result;  
        if (input <= 0.5) {  
            result = (float) (Math.sin(Math.PI * input)) / 2;  
        } else {  
            result = (float) (2 - Math.sin(Math.PI * input)) / 2;  
        }  
        return result;  
    }  
}  
```

这段代码是使用正弦函数来实现先减速后加速的功能的，因为正弦函数初始弧度的变化值非常大，刚好和余弦函数是相反的，而随着弧度的增加，正弦函数的变化值也会逐渐变小，这样也就实现了减速的效果。当弧度大于π/2之后，整个过程相反了过来，现在正弦函数的弧度变化值非常小，渐渐随着弧度继续增加，变化值越来越大，弧度到π时结束，这样从0过度到π，也就实现了先减速后加速的效果。

同样我们可以将这个算法的执行情况通过曲线图的方式绘制出来，结果如下图所示：
![](http://img.blog.csdn.net/20171221233812422?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

可以看到，这也是一个S型的曲线图，只不过曲线的方向和刚才是相反的。从上图中我们可以很清楚地看出来，一开始纵坐标的变化幅度很大，然后逐渐变小，横坐标到0.5的时候纵坐标变化幅度趋近于零，之后随着横坐标继续增加纵坐标的变化幅度又开始变大，的确是先减速后加速的效果。

那么现在我们将DecelerateAccelerateInterpolator在代码中进行替换，如下所示：

```java
private void startAnimation() {  
    Point startPoint = new Point(getWidth() / 2, RADIUS);  
    Point endPoint = new Point(getWidth() / 2, getHeight() - RADIUS);  
    ValueAnimator anim = ValueAnimator.ofObject(new PointEvaluator(), startPoint, endPoint);  
    anim.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {  
        @Override  
        public void onAnimationUpdate(ValueAnimator animation) {  
            currentPoint = (Point) animation.getAnimatedValue();  
            invalidate();  
        }  
    });  
    anim.setInterpolator(new DecelerateAccelerateInterpolator());//替换成自定义Interpolator
    anim.setDuration(3000);  
    anim.start();  
}  
```

非常简单，就是将DecelerateAccelerateInterpolator的实例传入到setInterpolator()方法当中。重新运行一下代码，效果如下图所示：

[![](http://img.blog.csdn.net/20171221233909649?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)](http://img.blog.csdn.net/20171221233909649?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)



# 动画常见问题

## 修改 Activity 进入和退出动画

可以通过两种方式，一是通过定义 Activity 的主题，二是通过覆写 Activity 的 overridePendingTransition 方法。
方式1：通过**设置主题样式**
在 styles.xml 中编辑如下代码：

```xml
<style name="AnimationActivity" parent="@android:style/Animation.Activity">
<item name="android:activityOpenEnterAnimation">@anim/slide_in_left</item>
<item name="android:activityOpenExitAnimation">@anim/slide_out_left</item>
<item name="android:activityCloseEnterAnimation">@anim/slide_in_right</item>
<item name="android:activityCloseExitAnimation">@anim/slide_out_right</item>
</style>
```

添加 themes.xml 文件：

```xml
<style name="ThemeActivity">
<item name="android:windowAnimationStyle">@style/AnimationActivity</item>
<item name="android:windowNoTitle">true</item>
</style>
```

在 AndroidManifest.xml 中给指定的 Activity 指定 theme。

方式2：**覆写 overridePendingTransition** 方法
overridePendingTransition(R.anim.fade, R.anim.hold);

## 属性动画，例如一个 button 从 A 移动到 B 点，B 点还是可以响应点击事件，这个原理是什么？

补间动画只是显示的位置变动，View 的实际位置未改变，表现为 View 移动到其他地方，点击事件仍在原处才能响应。而属性动画控件移动后事件相应就在控件移动后本身进行处理。



引用：
[Android属性动画完全解析(上)，初识属性动画的基本用法](http://blog.csdn.net/guolin_blog/article/details/43536355)
[Android属性动画完全解析(中)，ValueAnimator和ObjectAnimator的高级用法](http://blog.csdn.net/guolin_blog/article/details/43816093)
[Android属性动画完全解析(下)，Interpolator和ViewPropertyAnimator的用法](http://blog.csdn.net/guolin_blog/article/details/44171115)
[自定义控件三部曲之动画篇（二）——Interpolator插值器](http://blog.csdn.net/harvic880925/article/details/40049763)
[自定义控件三部曲之动画篇(五)——ValueAnimator高级进阶（一）](http://blog.csdn.net/harvic880925/article/details/50546884)
[自定义控件三部曲之动画篇(六)——ValueAnimator高级进阶（二）](http://blog.csdn.net/harvic880925/article/details/50549385)
[自定义控件三部曲之动画篇(七)——ObjectAnimator基本使用](http://blog.csdn.net/harvic880925/article/details/50598322)










】



【Android MVP】
【
# 【Android MVP】

![](http://upload-images.jianshu.io/upload_images/9028834-db31eb322f813150?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

> 声明：本文由作者**还不走A**投稿。
> 
> 还不走A的博客：http://blog.csdn.net/dantestones
> 
> 本文是作者对MVP架构的一点心得，并且提供了一个简单的例子，看起来不会很吃力，希望对大家有帮助。

* * *

## 一、老的MVC架构

刚开始接触Android的时候会觉得Android的整个代码架构就是一个MVC。

*   M : 业务层和模型层，相当与javabean和我们的业务请求代码

*   V : 视图层，对应Android的layout.xml布局文件

*   C : 控制层，对应于Activity中对于UI 的各种操作

看起来MVC架构很清晰，但是实际的开发中，请求的业务代码往往被丢到了Activity里面，大家都知道`layout.xml`的布局文件只能提供默认的UI设置，所以开发中视图层的变化也被丢到了Activity里面，再加上`Activity`本身承担着控制层的责任。所以Activity达成了MVC集合的成就，最终我们的Activity就变得越来越难看，从几百行变成了几千行。维护的成本也越来越高

## 二、新的MVP架构

*   M : 还是业务层和模型层

*   V : 视图层的责任由Activity来担当

*   P : 新成员Prensenter 用来代理 C(control) 控制层

MVP与MVC最大的不同，其实是Activity职责的变化，由原来的C (控制层) 变成了 V(视图层)，不再管控制层的问题，只管如何去显示。控制层的角色就由我们的新人 Presenter来担当，这种架构就解决了Activity过度耦合控制层和视图层的问题。

## 三、一个demo

理念终归要用代码来实现，来看一个很典型的例子，打开一个有列表的Activity界面，请求数据然后刷新界面的过程。先来看看工程的架构 ：![](http://upload-images.jianshu.io/upload_images/9028834-0788438776cef3a6?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我分了MVC 和 MVP2种写法的目录，MVP与MVC不同就在于多了2个结构presenter和view。 这里业务层大家就公用了一个biz，先来看看相同的biz 也就是业务层；

### （1）RequestBiz

声明了一个接口，带有请求数据业务的方法：

```
public interface RequestBiz {
    //请求数据业务
    void requestForData(OnRequestListener listener);
}
```

对应的实现类为：RequestBizImpl

请求的实现类为了模拟网络请求，开启了一个会sleep2秒的线程，然后装填请求的数据，通过OnRequestListener 接口回调出去，与我们平时开发的方式一致。

![](http://upload-images.jianshu.io/upload_images/9028834-2ba9a672e8a63eaa?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

对应的接口为OnRequestListener

![image.gif](http://upload-images.jianshu.io/upload_images/9028834-a531b42091fc72ed.gif?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

到此业务层完成，上面已经提到业务层不管对于MVC,MVP都是公用的。

这里先看MVC的写法：

### （2）传统MVC的写法

![](http://upload-images.jianshu.io/upload_images/9028834-db3bfefb1e57cbea?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

请求数据的代码：

![](http://upload-images.jianshu.io/upload_images/9028834-0f027f40e7efaa52?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) 

比较常见的Activity里面的写法：

1.  界面的初始化

2.  发起请求以及请求完成后的界面更新

3.  点击的监听设置方法。

C: 控制层(点击事件，网络请求), V : 视图层(界面刷新) 。这样代码都耦合到了Activity里面。

### （3）MVP的写法

由于Activity变成了view层不再去控制界面，但是具体的界面的改变api其实还是由Activity来提供的，所以在写MVP之前需要思考，View层需要哪些方法。

*   显示loading

*   隐藏loading

*   listview的初始化

*   弹出Toast消息

首先看抽取的接口，MVPView

**1.MVPView**

**![](http://upload-images.jianshu.io/upload_images/9028834-a11edb914bf2ccfc?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)** 

通过上面的MVC的例子，我们可以总结出View层需要的接口。

View的接口抽取完毕，就可以可以编写presenter层了， 在presenter中包含MvpView以及我们的业务类RequestBiz，毕竟它相当于View与Model(业务层)的桥梁。

**2. MvpPresenter**

![](http://upload-images.jianshu.io/upload_images/9028834-c8d76400e44a9f70?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

Presenter完成，现在就剩下一件事，Activity中使用Presenter

**3. MvpActivity**

![](http://upload-images.jianshu.io/upload_images/9028834-2b3709340cc4c3c1?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

静静的享受MVP的感觉吧，就是这么清爽，没有乱七八糟的业务，没有各种点击处理逻辑，Activity只需要提供View层的方法就可以了。

## 四、一点心得

我们的实际发开中需求往往瞬息万变，每次还要等美工出界面，这样往往会浪费大量时间，但是在MVP的世界这些都不是事，视图层与控制层完全分离，可以让我们在界面还是很粗糙的情况下，先进行控制层的开发，甚至可以先让View层先提供方法出来，这样可以节省很多时间。后面的复杂需求变化，也就是构建接口的时候会变的复杂，但是在有这么多优势的情况下，这点复杂度完全可以接受 **还等什么快来加入MVP吧!!!**

源码下载地址：https://github.com/haibuzou/MVPSample/tree/master


引用：
[Android MVP架构的自述](https://mp.weixin.qq.com/s/opTZJT80s2VEZUTJaAyo9g)
】
