<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [抓包工具 Charles](#%E6%8A%93%E5%8C%85%E5%B7%A5%E5%85%B7-charles)
  - [一、Charles是什么？](#%E4%B8%80charles%E6%98%AF%E4%BB%80%E4%B9%88)
  - [二、为什么是Charles？](#%E4%BA%8C%E4%B8%BA%E4%BB%80%E4%B9%88%E6%98%AFcharles)
  - [三、Charles基本工作原理](#%E4%B8%89charles%E5%9F%BA%E6%9C%AC%E5%B7%A5%E4%BD%9C%E5%8E%9F%E7%90%86)
    - [1.普通http请求过程](#1%E6%99%AE%E9%80%9Ahttp%E8%AF%B7%E6%B1%82%E8%BF%87%E7%A8%8B)
    - [2.加入了Charles的HTTP代理的请求与响应过程](#2%E5%8A%A0%E5%85%A5%E4%BA%86charles%E7%9A%84http%E4%BB%A3%E7%90%86%E7%9A%84%E8%AF%B7%E6%B1%82%E4%B8%8E%E5%93%8D%E5%BA%94%E8%BF%87%E7%A8%8B)
  - [四、Charles的下载与安装过程](#%E5%9B%9Bcharles%E7%9A%84%E4%B8%8B%E8%BD%BD%E4%B8%8E%E5%AE%89%E8%A3%85%E8%BF%87%E7%A8%8B)
    - [1.官网下载地址：](#1%E5%AE%98%E7%BD%91%E4%B8%8B%E8%BD%BD%E5%9C%B0%E5%9D%80)
    - [2.破解工具](#2%E7%A0%B4%E8%A7%A3%E5%B7%A5%E5%85%B7)
  - [五、Http抓包操作步骤](#%E4%BA%94http%E6%8A%93%E5%8C%85%E6%93%8D%E4%BD%9C%E6%AD%A5%E9%AA%A4)
    - [Step 1: 开启Charleshttp代理](#step-1-%E5%BC%80%E5%90%AFcharleshttp%E4%BB%A3%E7%90%86)
    - [Step 2: 手机端Wifi添加代理](#step-2-%E6%89%8B%E6%9C%BA%E7%AB%AFwifi%E6%B7%BB%E5%8A%A0%E4%BB%A3%E7%90%86)
    - [a. Android手机：](#a-android%E6%89%8B%E6%9C%BA)
    - [b. iOS手机：](#b-ios%E6%89%8B%E6%9C%BA)
    - [Step 3:开启Charles录制功能](#step-3%E5%BC%80%E5%90%AFcharles%E5%BD%95%E5%88%B6%E5%8A%9F%E8%83%BD)
    - [Step 4：启动应用开始抓包](#step-4%E5%90%AF%E5%8A%A8%E5%BA%94%E7%94%A8%E5%BC%80%E5%A7%8B%E6%8A%93%E5%8C%85)
    - [Step 5：分析抓取的数据包](#step-5%E5%88%86%E6%9E%90%E6%8A%93%E5%8F%96%E7%9A%84%E6%95%B0%E6%8D%AE%E5%8C%85)
  - [六、Https抓包操作步骤](#%E5%85%ADhttps%E6%8A%93%E5%8C%85%E6%93%8D%E4%BD%9C%E6%AD%A5%E9%AA%A4)
    - [Step 1：了解一下https的基本原理；](#step-1%E4%BA%86%E8%A7%A3%E4%B8%80%E4%B8%8Bhttps%E7%9A%84%E5%9F%BA%E6%9C%AC%E5%8E%9F%E7%90%86)
    - [Step 2：在手机端安装SSL证书](#step-2%E5%9C%A8%E6%89%8B%E6%9C%BA%E7%AB%AF%E5%AE%89%E8%A3%85ssl%E8%AF%81%E4%B9%A6)
    - [Step 3：激活Charles的SSL代理](#step-3%E6%BF%80%E6%B4%BBcharles%E7%9A%84ssl%E4%BB%A3%E7%90%86)
    - [Step 4：将指定的URL请求开启SSL代理功能](#step-4%E5%B0%86%E6%8C%87%E5%AE%9A%E7%9A%84url%E8%AF%B7%E6%B1%82%E5%BC%80%E5%90%AFssl%E4%BB%A3%E7%90%86%E5%8A%9F%E8%83%BD)
  - [七、Charles进阶---修改请求也响应的内容](#%E4%B8%83charles%E8%BF%9B%E9%98%B6---%E4%BF%AE%E6%94%B9%E8%AF%B7%E6%B1%82%E4%B9%9F%E5%93%8D%E5%BA%94%E7%9A%84%E5%86%85%E5%AE%B9)
    - [Step 1：设置Charless断点](#step-1%E8%AE%BE%E7%BD%AEcharless%E6%96%AD%E7%82%B9)
    - [Step 2：对指定的URL开启断点功能。](#step-2%E5%AF%B9%E6%8C%87%E5%AE%9A%E7%9A%84url%E5%BC%80%E5%90%AF%E6%96%AD%E7%82%B9%E5%8A%9F%E8%83%BD)
    - [Step 3：编辑请求与响应的内容。](#step-3%E7%BC%96%E8%BE%91%E8%AF%B7%E6%B1%82%E4%B8%8E%E5%93%8D%E5%BA%94%E7%9A%84%E5%86%85%E5%AE%B9)
      - [a.编辑请求内容](#a%E7%BC%96%E8%BE%91%E8%AF%B7%E6%B1%82%E5%86%85%E5%AE%B9)
      - [b.编辑服务器响应的内容](#b%E7%BC%96%E8%BE%91%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%93%8D%E5%BA%94%E7%9A%84%E5%86%85%E5%AE%B9)
  - [八、Charles进阶---弱网模拟](#%E5%85%ABcharles%E8%BF%9B%E9%98%B6---%E5%BC%B1%E7%BD%91%E6%A8%A1%E6%8B%9F)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->



# 抓包工具 Charles
## 一、Charles是什么？
Charles是在 Mac或Windows下常用的http协议网络包截取工具，是一款屌的不行的抓包工具，在平常的测试与调式过程中，掌握此工具就基本可以不用其他抓包工具了。

## 二、为什么是Charles？
为什么要用抓包工具？大家在平常移动App调试测试中是如何进行抓包的？

在我们做开发与测试的过程中，总免不了碰到网络问题，特别是重后台的产品，这个时候我们往往的处理方法是抓个网络包，看看到底应用发送和接收了些什么鬼.....这个时候Charles上场了；Charles是一款屌的不行的截包工具，好用到没朋友。

**那么Charles屌在哪里呢？？？主要特点如下：**

1.支持SSL代理，可以截取分析SSL的请求

2.支持流量控制。可以模拟慢速网络(2G,3G)，以及等待时间较长的请求。

3.支持AJAX调试。可以自动把JSON或者XML数据格式化，方便查看。

4.支持重发网络请求，方便后端调试。

5.支持修改网络请求参数。

6.支持网络请求的截取和动态修改。

7.最重要的一个优点就是有不同平台的版本（Mac，Windows、Linux）即学一个打遍天下。

## 三、Charles基本工作原理
charles是通过网络代理来进行抓包的，下面先了解一下http代理的原理：

### 1.普通http请求过程
![普通http请求过程](http://img.blog.csdn.net/20180116231444456?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### 2.加入了Charles的HTTP代理的请求与响应过程
![加入了Charles](http://img.blog.csdn.net/20180116231433827?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
中间的代理服务器就是Charles

## 四、Charles的下载与安装过程

### 1.官网下载地址：

> [http](https://link.jianshu.com?t=http://www.charlesproxy.com/download/)[://www.charlesproxy.com/download](https://link.jianshu.com?t=http://www.charlesproxy.com/download/)[/](https://link.jianshu.com?t=http://www.charlesproxy.com/download/)

### 2.[破解工具](https://www.zzzmode.com/mytools/charles/)

## 五、Http抓包操作步骤
> Step 1：开启Charleshttp代理；

> Step 2：手机端Wifi添加代理；

> Step 3：开启Charles录制功能；

> Step 4：启动应用开始抓包；

> Step 5：分析抓取的数据包。

### Step 1: 开启Charleshttp代理

a.设置Charles代理

![设置Charles代理](http://img.blog.csdn.net/20180116231422994?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
第一次启动默认会开启本机的系统代理，因为我们只是监控移动端的所以将此选去除（去掉选项前面的小钩）

b.激活http代理功能

![激活http代理功能](http://img.blog.csdn.net/20180116231416588?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### Step 2: 手机端Wifi添加代理
### a. Android手机：

1.在手机端打开你的Wifi设置然后长按已经连接的Wifi在弹出来的菜单中选择【修改网络】

2.沟上[显示高级]选项--【手动】

3.输入代理服务器的IP与端口，IP即安装了Charles的电脑IP地址，端口就是前面一步设置Charles时所设置的端口。

![Android手机](http://img.blog.csdn.net/20180116231411704?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
注意：手机所连接Wifi要与电脑在同一个LAN(局域网)。

### b. iOS手机：

1.点击你所连接的wifi

2.输入代理服务器的IP与端口，

IP即安装了Charles的电脑IP地址，端口就是前面一步设置Charles时所设置的端口。
![iOS手机](http://img.blog.csdn.net/20180116231405375?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### Step 3:开启Charles录制功能

![Charles录制](http://img.blog.csdn.net/20180116231356582?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
1.当手机连接上代理后Charles会弹出相应的提示框，点击Allow即可。
2.点击工具栏上的开始录制按钮，即启动了Charles的抓包功能了。

### Step 4：启动应用开始抓包

![开始抓包](http://img.blog.csdn.net/20180116231349979?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
1.在手机上操作相应的App进行抓包。
2.在Charles的主界面上就可看到相应的请求内容。

### Step 5：分析抓取的数据包

![分析抓取](http://img.blog.csdn.net/20180116231342799?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

1.Charles 主要提供两种查看封包的视图，分别名为 “Structure”和 “Sequence”：

    a.Structure 视图将网络请求按访问的域名分类；

    b.Sequence 视图将网络请求按访问的时间排序。

2.大家可以根据具体的需要在这两种视图之前来回切换。请求多了有些时候会看不过来，Charles提供了一个简单的Filter功能，可以输入关键字来快速筛选出URL 中带指定关键字的网络请求。

3.对于某一个具体的网络请求，你可以查看其详细的请求内容和响应内容。如果请求内容是POST 的表单，Charles 会自动帮你将表单进行分项显示。如果响应内容是 JSON 格式的，那么 Charles可以自动帮你将JSON 内容格式化，方便你查看。如果响应内容是图片，那么 Charles可以显示出图片的预览。

## 六、Https抓包操作步骤
> Step 1：了解一下https的基本原理；

> Step 2：在手机端安装SSL证书；

> Step 3：激活Charles的SSL代理；

> Step 4：将指定的URL请求开启SSL代理功能

> Step 5：其他步骤与Http抓包相同，请参考[四、Http抓包操作步骤](https://link.jianshu.com?t=http://%E5%9B%9B%E3%80%81Http%E6%8A%93%E5%8C%85%E6%93%8D%E4%BD%9C%E6%AD%A5%E9%AA%A4)

### Step 1：了解一下https的基本原理；

![https的基本原理](http://img.blog.csdn.net/20180116231335212?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
HTTPS其实是有两部分组成：HTTP+ SSL / TLS，也就是在HTTP上又加了一层处理加密信息的模块。服务端和客户端的信息传输都会通过TLS进行加密，所以传输的数据都是加密后的数据。具体是如何进行加密，解密，验证的，且看图,下面这个图的解说

详细说明，请参考：

> [http://blog.csdn.net/clh604/article/details/22179907](https://link.jianshu.com?t=http://blog.csdn.net/clh604/article/details/22179907)

### Step 2：在手机端安装SSL证书

      1.将证书文件从Charles导出

      2.然后通过adb或者其他工具将其复制到手机的SD卡中。

![从Charles导出证书文件](http://img.blog.csdn.net/20180116231328590?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

      3.将证书文件导入Android手机，找到该文件并点击安装


      4.将证书文件导入iOS手机


![导入iOS手机](http://img.blog.csdn.net/20180116231315368?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### Step 3：激活Charles的SSL代理


![激活Charles的SSL代理](http://img.blog.csdn.net/20180116231308209?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
1.选择【Proxy】--->【SSL Proxying Settings..】设置。
2.在弹出来的对话框中沟选【Enable SSL Proxying】。

### Step 4：将指定的URL请求开启SSL代理功能

![开启SSL代理](http://img.blog.csdn.net/20180116231301372?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
1.选择抓取的https链接，然后右键选择【Enable SSL Proxying】。
2.如果不激活SSL代理，所以https请求都是乱码无法查看。

![再次请求](http://img.blog.csdn.net/20180116231250373?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
再次请求这个Https时，其请求内容已经一目了然了。

## 七、Charles进阶---修改请求也响应的内容

> Step 1：设置Charless断点。

> Step 2：对指定的URL开启断点功能。

> Step 3：编辑请求与响应的内容。

### Step 1：设置Charless断点

![设置Charless断点](http://img.blog.csdn.net/20180116231241467?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
选择【Breakpoint Settings…】--->勾选【Enable Breakpoints】来激活断点功能

### Step 2：对指定的URL开启断点功能。

![开启断点](http://img.blog.csdn.net/20180116231233226?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
1.选择一个URL链接-à右键开启菜单---》选择【Breakpoints】即可开启此请求的断点。
2.这样Charles会遇到此请求时会弹出中断对话框。

### Step 3：编辑请求与响应的内容。

#### a.编辑请求内容
![编辑请求](http://img.blog.csdn.net/20180116231225726?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
在中断对话框中，用户可以点击Edit Request来编辑请求的内容，编辑完成后然后点击【Execute】发出去这个请求给服务端

#### b.编辑服务器响应的内容
![编辑服务器响应](http://img.blog.csdn.net/20180116231217737?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
在【Edit Request】对话中点击【Execute】发出请求后，服务端返回来数据后，用户点击【Edit Response】可对响应内容进行编辑完成后然后点击【Execute】发出去这个数据给客户端。

## 八、Charles进阶---弱网模拟

1.菜单中选择【Proxy】--->【Throttle Settings..】-à激活【Enable Throttling】。

2.在Throttle Configuration设置弱网的参数。

3.以下是各种网制式的速率参考文档：

[移动网络制式与网速的参考文档](https://link.jianshu.com?t=http://wenku.baidu.com/link?url=buoPWkwmfW3B216gtRzIBbLZST3EEqxnAHNEabaVu2tXlGlkCMUl_E4tor_408BRG4eRSd4p5VQd_k4xiq14VXvJIrrfZq7l9CJhU8ht7Nq)

![弱网模拟设置](http://img.blog.csdn.net/20180116231210640?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

引用：
[移动应用抓包调试利器Charles](https://www.jianshu.com/p/68684780c1b0)

