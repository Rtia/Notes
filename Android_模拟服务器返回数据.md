<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [【Android 模拟服务器返回数据】](#android-%E6%A8%A1%E6%8B%9F%E6%9C%8D%E5%8A%A1%E5%99%A8%E8%BF%94%E5%9B%9E%E6%95%B0%E6%8D%AE)
  - [背景](#%E8%83%8C%E6%99%AF)
    - [实现思路](#%E5%AE%9E%E7%8E%B0%E6%80%9D%E8%B7%AF)
  - [使用Charles模拟数据](#%E4%BD%BF%E7%94%A8charles%E6%A8%A1%E6%8B%9F%E6%95%B0%E6%8D%AE)
    - [准备条件](#%E5%87%86%E5%A4%87%E6%9D%A1%E4%BB%B6)
    - [配置](#%E9%85%8D%E7%BD%AE)
    - [转接服务器地址](#%E8%BD%AC%E6%8E%A5%E6%9C%8D%E5%8A%A1%E5%99%A8%E5%9C%B0%E5%9D%80)
    - [小结](#%E5%B0%8F%E7%BB%93)
  - [自定义OkHttp Interceptor模拟返回](#%E8%87%AA%E5%AE%9A%E4%B9%89okhttp-interceptor%E6%A8%A1%E6%8B%9F%E8%BF%94%E5%9B%9E)
    - [OkHttp拦截器](#okhttp%E6%8B%A6%E6%88%AA%E5%99%A8)
    - [代码实现](#%E4%BB%A3%E7%A0%81%E5%AE%9E%E7%8E%B0)
    - [小结](#%E5%B0%8F%E7%BB%93-1)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->



# 【Android 模拟服务器返回数据】

## 背景

模拟服务器返回的数据，在以下场景具有实际意义：

> 和服务器开发协商好开发接口，但服务器API尚未部署，想接口定义好就进行开发；
> 服务器已部署，返回的数据不能测试到各种情况，希望返回期待数据测试边界情况；

如果客户端开发人员能不走服务器，通过模拟数据返回，能提升开发效率和程序质量。

### 实现思路

本文主要讲解两种实现方式：

1.  使用网络分析工具拦截客户端请求，并返回伪造数据。
    优点：无需改变客户端代码；不依赖客户端平台，android和ios都通用；
    缺点：依赖网络分析工具；调试相对不灵活；

2.  使用客户端网络框架拦截请求并返回。
    优点：返回数据由客户端代码决定，灵活易于调试；
    缺点：需要改变客户端代码；需要根据客户端网络框架进行响应处理，不同的网络框架处理不一样；

对于方案一，主要使用网络分析工具Charles进行拦截并返回，对于方案二，主要讲解使用OkHttp作为网络框架，利用拦截器机制实现模拟返回。

## 使用Charles模拟数据

### 准备条件

1.  客户端需要连接到和电脑同一个网络(手机连接电脑发出的wifi)
2.  [官网下载安装](https://www.charlesproxy.com/download/)

### 配置

配置方法参考[Charles:移动端设备网络抓包](http://shinelw.com/2016/04/21/about-charles/)
完成配置后，可以在Charles中检测到手机的网络请求和响应。

### 转接服务器地址

[![Charles 拦截原理](http://img.blog.csdn.net/20180116220832800?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)](http://img.blog.csdn.net/20180116220832800?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast) 
转接服务器地址是指，当客户端请求地址B时，本应该向指定的服务器请求数据，但Charles可拦截此ip地址，使不向服务器地址请求，并且返回另外一台服务器模拟的数据。
首先，我们来生成模拟返回数据的api接口；
打开[mocky网址](http://www.mocky.io/),输入想伪造Body数据，点击**Generate my HTTP Response**按钮生成http的url地址。
[![mocky生成模拟地址](http://img.blog.csdn.net/20180116220826110?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)](http://img.blog.csdn.net/20180116220826110?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast) 
如图，当点击[http://www.mocky.io/v2/58592298240000ba087c5a92](http://www.mocky.io/v2/58592298240000ba087c5a92) 时，返回json格式的数据。
有了模拟数据的api地址，接着设置需要模拟的api接口。经过配置后，Charles可检测手机的网络请求，选择需要模拟返回数据的网络请求接口，**右键**选择**Map Remote…**
Map From为需要拦截的接口，Map To为模拟的api接口，此处我们填入**[http://www.mocky.io/v2/58592298240000ba087c5a92](http://www.mocky.io/v2/58592298240000ba087c5a92)**，如下图：
[![Charles 拦截ip地址](http://img.blog.csdn.net/20180116220820038?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)](http://img.blog.csdn.net/20180116220820038?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast) 
选择标题栏**Tool工具图标**，取消选择**Map Remote**,再勾选**Map Remote**，让设置的ip地址生效。此时，当客户端请求原地址时，都会返回模拟ip地址的数据，效果图如下：
[![Charles 返回模拟数据](http://img.blog.csdn.net/20180116220812454?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)](http://img.blog.csdn.net/20180116220812454?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast) 

### 小结

以上，使用Mocky网络和Charles工具实现模拟数据返回，无需改变客户端原有代码，但是，当需要改变客户端返回的数据时，则需要重新生成http模拟地址，再次设置Charles **Map to**内容。

## 自定义OkHttp Interceptor模拟返回

以下内容假设用户掌握OkHttp的简单使用，重点讲解自定义OkHttpInterceptor模拟返回数据。

### OkHttp拦截器

[![OkHttp 拦截器](http://img.blog.csdn.net/20180116220804324?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)](http://img.blog.csdn.net/20180116220804324?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast) 
如图，OkHttp可在Request和Response中设置任意个数的Intercepor(图中用圆圈标识)，对请求体和响应体进行处理。借助OkHttp Interceptor机制，创建一个MockIntercepor,模拟返回一个Response,虚线部分为模拟的Response。

### 代码实现

MockInterceptor代码如下：

```java
public class MockInterceptor implements Interceptor{
    @Override
    public Response intercept(Chain chain) throws IOException {
        Gson gson = new Gson();
        Response response = null;
        Response.Builder responseBuilder = new Response.Builder()
                .code(200)
                .message("")
                .request(chain.request())
                .protocol(Protocol.HTTP_1_0)
                .addHeader("content-type", "application/json");
        Request request = chain.request();
        if(request.url().equals("http://url_whitch_need_to_mock")) { //拦截指定地址
            String responseString = "{\n" +    //模拟数据返回body
                    "\t\"code\": 200,\n" +
                    "\t\"message\": \"success\",\n" +
                    "\t\"data\": {}\n" +
                    "}";
            responseBuilder.body(ResponseBody.create(MediaType.parse("application/json"), responseString.getBytes()));//将数据设置到body中
            response = responseBuilder.build(); //builder模式构建response
        }else{
            response = chain.proceed(request);
        }
        return response;
    }
}
```

在debug模式下，将此Interceptor添加到网络请求的OkHttp中，即可对指定的api地址进行拦截，并且返回特定的数据。

### 小结

使用OkHttp的拦截机制，可实现改变部分代码则可模拟返回数据，返回的数据可在代码中设置，可使用工厂模式将模拟数据的生成变动代码放到Factory中。依赖网络请求框架，若原项目使用OkHttp或Retrofit作为网络框架,可轻易实现模拟接口。


引用：
[模拟服务器返回数据](https://yuanjunli.github.io/2016/12/15/%E6%A8%A1%E6%8B%9F%E6%9C%8D%E5%8A%A1%E5%99%A8%E8%BF%94%E5%9B%9E%E6%95%B0%E6%8D%AE/)



