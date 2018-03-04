<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [【AndroidStudio 调试技巧】](#androidstudio-%E8%B0%83%E8%AF%95%E6%8A%80%E5%B7%A7)
  - [基本使用](#%E5%9F%BA%E6%9C%AC%E4%BD%BF%E7%94%A8)
  - [条件断点](#%E6%9D%A1%E4%BB%B6%E6%96%AD%E7%82%B9)
  - [日志断点](#%E6%97%A5%E5%BF%97%E6%96%AD%E7%82%B9)
  - [变量赋值](#%E5%8F%98%E9%87%8F%E8%B5%8B%E5%80%BC)
  - [变量观察](#%E5%8F%98%E9%87%8F%E8%A7%82%E5%AF%9F)
  - [对象求值](#%E5%AF%B9%E8%B1%A1%E6%B1%82%E5%80%BC)
  - [方法断点](#%E6%96%B9%E6%B3%95%E6%96%AD%E7%82%B9)
  - [变量断点](#%E5%8F%98%E9%87%8F%E6%96%AD%E7%82%B9)
  - [异常断点](#%E5%BC%82%E5%B8%B8%E6%96%AD%E7%82%B9)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 【AndroidStudio 调试技巧】

## 基本使用

Debug App有两种途径，第一种是直接点击下图运行按钮右侧的小虫状图标，运行并调试当前Project，这个我想大家都知道。

![Debug App](http://upload-images.jianshu.io/upload_images/9028834-9827963a058515e4.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

第二种就是调试当前已经处于运行状态下的App，这也是我们用的更多的一种调试手段，即`Attach debugger to Android process`。点击运行按钮右侧第三个按钮，弹出`Choose Process`窗口，选择对应的进程，点击OK按钮即可进入调试模式，此时，我们便可以在需要的地方直接下断点调试代码了：

![Attach debugger to Android process](http://upload-images.jianshu.io/upload_images/9028834-dbab525dc3b31124.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

接下来就是常见的调试方法了，在Debug窗口顶部工具栏有一排操作按钮，比如`Step Over`（单步执行）、`Step Into`（进入方法）等，如图所示：

![Debug窗口](http://upload-images.jianshu.io/upload_images/9028834-cb708a5203c2b1b1.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

打断点和取消断点最直接的方式就是单击目标代码行的行号右侧空白处，然后在Debug窗口左侧有个断点浏览按钮`View Breakpoints`，位于停止按钮下方第一个，可以**浏览Project中的所有断点**，同时可以添加删除断点：

![View Breakpoint](http://upload-images.jianshu.io/upload_images/9028834-6c4e5c936629be83.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 条件断点



有时候，我们的断点打在了循环体里面，但是我们**只想看某一特定循环次数下的运行情况**，难道要使用`Run to Cursor`功能不停地跳至下一次断点直至满足我们的要求吗？

![循环里的断点](http://upload-images.jianshu.io/upload_images/9028834-fc774878efdb0170.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果你知道条件断点的话，一定会悔不当初。条件断点可以满足开发人员自己输入条件，比如`fori`循环中输入`i == 5`即可让程序直接运行至第六次循环，`for each`循环中针对list某一元素下的断点调试。只需要右键点击断点，在弹出的窗口中输入Condiction条件，点击`Done`按钮，然后当程序执行到循环体时，会在满足条件的一次循环中停下来，供我们调试：

![条件断点](http://upload-images.jianshu.io/upload_images/9028834-223baf1ef33a0d73.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 日志断点



打印日志也是跟踪程序分析问题的一个非常有效的手段，**但是如果我们的程序已经运行并且处于调试模式，此时如果想打印日志更加直观的分析代码**，难道还要停止调试、添加Log代码并重新编译运行吗？

如果你知道日志断点，就不用如此大费周折，费时费力了。还是右键点击断点，在弹出的窗口中取消勾选`Suspend`复选框（即表示程序运行至此断点时不会停下来供开发者调试），然后勾选`Log evaluated expression:`，并输入打印语句即可。这样，当Debug模式下的程序执行至此，不会停下来，而是在控制台中打印对应信息，如：

![日志断点](http://upload-images.jianshu.io/upload_images/9028834-dd9f1070161aad26.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 变量赋值



比如，我们的**代码里有一个变量，这个变量的值会影响到程序的执行结果**。如果我们**想观察这个变量在不同的赋值下程序的执行结果**怎么办呢？难道要一遍遍的在代码里修改变量值，然后重新运行程序吗？显然这是非常麻烦的操作。其实，如果利用Debug模式下的变量赋值（Set Value），只需要运行一次，就能达到我们的观察效果。在使用该变量的代码处打个断点，然后在`Variables`窗口找到对应的变量，修改变量值并执行即可。

![Set Value](http://upload-images.jianshu.io/upload_images/9028834-6c2e8f64b70ce30a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 变量观察



在`Variables`变量区和`Watches`观察区可以查看Debug模式下，程序执行到断点处的变量值或者对象的各属性值，但是多多少少查看起来还是有些不方便。其实可以通过弹出窗口的形式查看属性值，只要将光标定位至断点代码行所用到的变量，IDE会自动弹出一个小窗口，如下图所示，此时，使用对应的快捷键或者点击这个小窗口里的变量即可弹出变量属性值窗口，Mac下的快捷键位`command + F1`，如图所示：

![变量观察01](http://upload-images.jianshu.io/upload_images/9028834-0e16d90bc84f36f0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![变量观察02](http://upload-images.jianshu.io/upload_images/9028834-5656561972dabec5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 对象求值



在断点处，如果有变量对象，系统提供了表达式求值功能，针对`Variables`视图中的变量对象，我们可以**输入任何计算语句，实时查看表达式计算结果**。具体操作为，右键`Variables`视图中的变量对象，选择`Evaluate Expression`，弹出表达式窗口，输入任何你想要的计算语句，点击`Evaluate`计算按钮，即可显示Result结果：

![Evaluate Expression01](http://upload-images.jianshu.io/upload_images/9028834-0aef109e237d4ac3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![Evaluate Expression02](http://upload-images.jianshu.io/upload_images/9028834-97eb2bde6e57f79e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 方法断点



通常我们会对方法里的代码添加断点调试，很少对方法本身调试。其实，如果只是为了看到方法的参数和返回结果，我们可以在定义方法的第一行打断点，直接对方法本身调试，此时断点的展示图标样式也会与众不同：

![方法断点](http://upload-images.jianshu.io/upload_images/9028834-aa5a65945a76e348.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 变量断点



有时候，我们想知道自定义的变量的何时何地发生了改变，就可以使用变量断点。变量断点的图标样式也与众不同，在变量定义行打断点，开启Debug模式，在程序执行的过程中，如果该变量的值发生改变，程序会自动停下来，并定位在改变变量值的地方，供开发者调试：

![变量断点](http://upload-images.jianshu.io/upload_images/9028834-be6d49f31f62a25d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 异常断点



程序在执行的过程中可能会出现各种各样的未知性异常，如果能在发生异常的时候第一时间让程序停下来，并定位到异常出现的地方，供开发者调试，那当然是极好的。而万能的Android Studio就提供了这样的功能。

打开断点管理器，这里有两种方式打开：点击工具栏菜单`Run`，选择`View Breakpoints`；在Debug窗口直接点击`View Breakpoints`图标。点击左上角加号按钮，可以添加各种断点，包括前文提到的`Method Breakpoints`和`Field Watchpoints`断点，这里我们选择**`Exception Breakpoints`异常断点**，在弹出的`Enter Exception Class`窗口中输入需要监控的异常类别即可：

![Breakpoints](http://upload-images.jianshu.io/upload_images/9028834-8e89a1ae2fd88206.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![Exception Breakpoints](http://upload-images.jianshu.io/upload_images/9028834-f2dea04512c9af56.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



引用：
[Android Studio 掌握这些调试技巧，Debug能力不能再高啦](https://www.jianshu.com/p/985f788fae2c)