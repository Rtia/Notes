



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

