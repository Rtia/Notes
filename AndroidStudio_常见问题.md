<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [**【AndroidStudio 常见问题】**](#androidstudio-%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98)
- [**软件问题：**](#%E8%BD%AF%E4%BB%B6%E9%97%AE%E9%A2%98)
  - [高速下载网址](#%E9%AB%98%E9%80%9F%E4%B8%8B%E8%BD%BD%E7%BD%91%E5%9D%80)
  - [配置环境变量](#%E9%85%8D%E7%BD%AE%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F)
  - [缓存文件夹配置](#%E7%BC%93%E5%AD%98%E6%96%87%E4%BB%B6%E5%A4%B9%E9%85%8D%E7%BD%AE)
  - [SDK Manager.exe 无法打开，一闪而过](#sdk-managerexe-%E6%97%A0%E6%B3%95%E6%89%93%E5%BC%80%E4%B8%80%E9%97%AA%E8%80%8C%E8%BF%87)
  - [Gradle手动下载](#gradle%E6%89%8B%E5%8A%A8%E4%B8%8B%E8%BD%BD)
  - [No JVM installation found-->需要安装jdk7](#no-jvm-installation-found--%E9%9C%80%E8%A6%81%E5%AE%89%E8%A3%85jdk7)
  - [安装完启动时提示下载sdk](#%E5%AE%89%E8%A3%85%E5%AE%8C%E5%90%AF%E5%8A%A8%E6%97%B6%E6%8F%90%E7%A4%BA%E4%B8%8B%E8%BD%BDsdk)
  - [查看task栈情况](#%E6%9F%A5%E7%9C%8Btask%E6%A0%88%E6%83%85%E5%86%B5)
- [**编译报错**](#%E7%BC%96%E8%AF%91%E6%8A%A5%E9%94%99)
  - [导入项目一直Building](#%E5%AF%BC%E5%85%A5%E9%A1%B9%E7%9B%AE%E4%B8%80%E7%9B%B4building)
  - [Failed to open zip file](#failed-to-open-zip-file)
  - [The android gradle plugin version 2.3.0-beta4 is too old, please update to the latest version.](#the-android-gradle-plugin-version-230-beta4-is-too-old-please-update-to-the-latest-version)
  - [The APK file  **.apk does not exist on disk.](#the-apk-file--apk-does-not-exist-on-disk)
  - [Could not find com.android.tools.build:gradle](#could-not-find-comandroidtoolsbuildgradle)
  - [Error:Execution failed for task ':xutils:mergeDebugAndroidTestResources'. > No slave process to process jobs, aborting](#errorexecution-failed-for-task-xutilsmergedebugandroidtestresources--no-slave-process-to-process-jobs-aborting)
  - [Caused by: java.lang.IllegalStateException: This Activity already has an action bar supplied by the window decor. Do not request Window.FEATURE_SUPPORT_ACTION_BAR and set windowActionBar to false in your theme to use a Toolbar instead.](#caused-by-javalangillegalstateexception-this-activity-already-has-an-action-bar-supplied-by-the-window-decor-do-not-request-windowfeature_support_action_bar-and-set-windowactionbar-to-false-in-your-theme-to-use-a-toolbar-instead)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->





# **【AndroidStudio 常见问题】**

# **软件问题：**

## 高速下载网址

[Android Developer Tools Download : ADT/JDK/ Gradle/AndroidStudio](http://tools.android-studio.org/index.php)
[Gradle](https://services.gradle.org/distributions/)

------

## 配置环境变量

**Path**：	PATH属性已存在，在原来变量后追加：
;D:\Develop\Program\android-sdk-windows\platform-tools

------

## [缓存文件夹配置](http://blog.csdn.net/moira33/article/details/78553131)

------

## [SDK Manager.exe 无法打开，一闪而过](http://blog.csdn.net/moira33/article/details/78553111)

重装SDK-->下载SDK Tools（[android SDKinstaller_r24.4.1-windows.exe](https://link.jianshu.com/?t=http://pan.baidu.com/s/1hsI5Yao)）直接覆盖安装在Tools文件夹下

------

## [Gradle手动下载](http://blog.csdn.net/moira33/article/details/78549351)

下载好的压缩包和解压后的文件夹复制到gradle-2.14.1-all --->8bnwg5hd3w55iofp58khbp6yv文件夹下；
将gradle-2.14.1-all.zip.part文件删除；
复制一份gradle-2.14.1-all.zip.lck文件，重命名为gradle-2.14.1-all.zip.ok；
重启as。

------

## No JVM installation found-->需要安装jdk7

 ![这里写图片描述](http://img.blog.csdn.net/20171115220930250?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

------

## 安装完启动时提示下载sdk

 ![这里写图片描述](http://img.blog.csdn.net/20171115220946603?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

解决方式：
点击取消，如果android studio还进不去，按以下方式进行设置：
（a）进入刚安装的Android Studio目录下的bin目录。找到idea.properties文件，用文本编辑器打开。
（b）在idea.properties文件末尾添加一行，然后保存文件:
disable.android.first.run=true
（c）关闭Android Studio后重新启动，便可进入界面。

------

## [查看task栈情况](http://blog.csdn.net/moira33/article/details/78845935)

------

------





# **编译报错**

## 导入项目一直Building

AndroidStudio导入项目一直卡在Building gradle project info，实际上是因为你导入的这个项目使用的gradle与你已经拥有的gradle版本不一致，导致需要下载该项目需要的gradle版本,会一直卡住，直至下载完成。
-->[手动下载所需Gradle](http://blog.csdn.net/moira33/article/details/78549351)，或者设置翻墙让他自己下载。

------

## [Failed to open zip file](http://blog.csdn.net/moira33/article/details/78553093)

[手动下载所需Gradle](http://blog.csdn.net/moira33/article/details/78549351)

------

## The android gradle plugin version 2.3.0-beta4 is too old, please update to the latest version.

To override this check from the command line please set the ANDROID_DAILY_OVERRIDE environment variable to "8aeac5c96ae3671c8f785edf28017bc33e1650e3"
方案一：更新AndroidStudio
[方案二](http://blog.csdn.net/jw20082009jw/article/details/69267413)：找到插件的本地目录`$Android_studio_home\gradle\m2repository\com\android\tools\build\gradle `，把项目根目录build.gradle的gradle改为本地版本

```
dependencies {  
        classpath 'com.android.tools.build:gradle:2.2.0'
}  
```

------

## The APK file  **.apk does not exist on disk.

Error while Installing APK
Android Studio编译应用后安装APK的时候，报错：

```
The APK file build\outputs\apk\OYP_2.3.4_I2Base_6476_official_debug.apk does not exist on disk.
Error while Installing APK
```

> 解决方案：刷新Gradle
> ![刷新Gradle](http://img.blog.csdn.net/20171124210829684?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

------

## [Could not find com.android.tools.build:gradle](http://blog.csdn.net/moira33/article/details/78850806)

------

## Error:Execution failed for task ':xutils:mergeDebugAndroidTestResources'. > No slave process to process jobs, aborting

导入module时报错Error:Execution failed for task ':xutils:mergeDebugAndroidTestResources'. > No slave process to process jobs, aborting，不知道什么原因，重新启动 AS后再build就好了

------

## Caused by: java.lang.IllegalStateException: This Activity already has an action bar supplied by the window decor. Do not request Window.FEATURE_SUPPORT_ACTION_BAR and set windowActionBar to false in your theme to use a Toolbar instead.
给其所在Activity添加
``` xml
<item name="windowNoTitle">true</item>
```
即可

