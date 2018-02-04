#【Android AIDL】

## 简介

<font size="4" color="#D2691E" style="box-sizing: border-box; font-family: Verdana !important;">A</font> [android]

<font size="4" color="#D2691E" style="box-sizing: border-box; font-family: Verdana !important;">I</font> [Interface]

<font size="4" color="#D2691E" style="box-sizing: border-box; font-family: Verdana !important;">D</font> [Definition]

<font size="4" color="#D2691E" style="box-sizing: border-box; font-family: Verdana !important;">L</font> [Language]

<font size="4" color="#D2691E" style="box-sizing: border-box; font-family: Verdana !important;">Android接口定义语言。</font>

作用：方便系统为我们生成代码从而实现跨进程通讯，也就是说这个AIDL就只是一个快速跨进程通讯的工具。

## 快速上手

本篇文章意在快速实现AIDL项目，更多详细内容下篇继续阐述。

*   在服务端创建AIDL文件，用来声明java Bean以及传输调用的接口。【声明文件】
*   在服务端创建Service并且实现上面的接口。【创建服务】
*   客户端绑定Service。【绑定服务】
*   客户端调用服务端接口。【跨进程调用】

### 服务端

创建服务端项目

首先我们在app/src/main 目录下创建AIDL文件夹。
![](http://img.blog.csdn.net/20171228163410960?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

##### 创建载体MessageBean

首先我们在这个AIDL文件夹里创建用来传输的java Bean对象（包名并不重要），并且实现`Parcelable`接口（建议使用Parcelable插件），因为进程间通讯需要将该对象转化为字节序列，用于传输或者存储。（传递的载体）

```java
public class MessageBean implements Parcelable {
    private String content;//需求内容
    private int level;//重要等级

    ``````
    get set方法
    ``````

    @Override
    public int describeContents() {
        return 0;
    }

    @Override
    public void writeToParcel(Parcel dest, int flags) {
        dest.writeString(this.content);
        dest.writeInt(this.level);
    }

    //如果需要支持定向tag为out,inout，就要重写该方法
    public void readFromParcel(Parcel dest) {
        //注意，此处的读值顺序应当是和writeToParcel()方法中一致的
        this.content = dest.readString();
        this.level = dest.readInt();
    }
    protected MessageBean(Parcel in) {
        this.content = in.readString();
        this.level = in.readInt();
    }
    public MessageBean() {

    public static final Creator<MessageBean> CREATOR = new Creator<MessageBean>() {
        @Override
        public MessageBean createFromParcel(Parcel source) {
            return new MessageBean(source);
        }

        @Override
        public MessageBean[] newArray(int size) {
            return new MessageBean[size];
        }
    };
}
```

##### 创建AIDL文件MessageBean.AIDL

因为AIDL这个语言的规范就是aidl文件，所以我们必须将`MessageBean`转为aidl文件，供其它aidl的调用与交互。就这么简单，一个文件里面两行代码。(这个文件要与java Bean放置同一包下)

```java
package qdx.aidlserver;//手动导包
parcelable MessageBean;//parcelable是小写
```

[AIDL文件与java文件的区别](http://blog.csdn.net/luoyanglizi/article/details/51980630 "optional title")

*   文件类型：用AIDL书写的文件的后缀是 .aidl，而不是 .java。
*   数据类型：AIDL默认支持一些数据类型，在使用这些数据类型的时候是不需要导包的，但是除了这些类型之外的数据类型，在使用之前必须导包，就算目标文件与当前正在编写的 .aidl 文件在同一个包下——在 Java 中，这种情况是不需要导包的。比如，现在我们编写了两个文件，一个叫做 `IDemandManager.java`，另一个叫做 `IDemandManager.aidl`，它们都在`qdx.aidlserver`包下 ，现在我们需要在 .aidl 文件里使用`MessageBean` 对象，那么我们就必须在 .aidl 文件里面写上 import qdx.aidlserver.MessageBean; 哪怕 .java 文件和 .aidl 文件就在一个包下。

*   默认支持的数据类型
    Java中的八种基本数据类型( byte，~~short~~(不支持short，编译不通过)，int，long，float，double，boolean，char)
    String 和 CharSequence类型
    List ： List中的所有元素必须是AIDL支持的类型之一，里面的每个元素都必须能够被AIDL支持
    Map ： Map中的所有元素必须是AIDL支持的类型之一，包括key和value
    Parcelabel : 所有实现了Parcelabel 接口的对象
    AILD : 所有的AIDL接口本身也可以在AIDL文件中使用

##### 创建AIDL文件IDemandManager.AIDL

我们创建`IDemandManager`接口用来实现传递方法。
另外方法内如果有传输载体，就必须指明定向tag([in,out,inout](http://blog.csdn.net/luoyanglizi/article/details/51958091 "optional title"))

*   in : 客户端数据对象流向服务端，并且服务端对该数据对象的修改不会影响到客户端。
*   out : 数据对象由服务端流向客户端，（客户端传递的数据对象此时服务端收到的对象内容为空，服务端可以对该数据对象修改，并传给客户端）
*   inout : 以上两种数据流向的结合体。（但是不建议用此tag,会增加开销）

```java
package qdx.aidlserver;

import qdx.aidlserver.MessageBean;
import qdx.aidlserver.IDemandListener;

interface IDemandManager {
     MessageBean getDemand();

     void setDemandIn(in MessageBean msg);//客户端->服务端

     //out和inout都需要重写MessageBean的readFromParcel方法
     void setDemandOut(out MessageBean msg);//服务端->客户端

     void setDemandInOut(inout MessageBean msg);//客户端<->服务端
}
```

##### 注意

1. xxx.aidl 中不能存在同方法名不同参数的方法。

2. xxx.aidl 中实体类必须要有指定的tag。

3. 在Android Studio里写完aidl文件还需要在build.gradle文件中android{}方法内添加aidl路径。

```java
sourceSets {
    main {
        java.srcDirs = ['src/main/java', 'src/main/aidl']
    }
}
```

##### 创建Service

最后我们还需要创建一个服务，用来处理客户端发来的请求，或者是定时推信息到客户端。

```java
public class AIDLService extends Service {

    @Override
    public IBinder onBind(Intent intent) {
        return demandManager;
    }

    //Stub内部继承Binder，具有跨进程传输能力
    IDemandManager.Stub demandManager = new IDemandManager.Stub() {
        @Override
        public MessageBean getDemand() throws RemoteException {
            MessageBean demand = new MessageBean("首先，看到我要敬礼", 1);
            return demand;
        }

        @Override
        public void setDemandIn(MessageBean msg) throws RemoteException {//客户端数据流向服务端
            Log.i(TAG, "程序员:" + msg.toString());
        }

        @Override
        public void setDemandOut(MessageBean msg) throws RemoteException {//服务端数据流向客户端
            Log.i(TAG, "程序员:" + msg.toString());//msg内容一定为空

            msg.setContent("我不想听解释，下班前把所有工作都搞好！");
            msg.setLevel(5);
        }

        @Override
        public void setDemandInOut(MessageBean msg) throws RemoteException {//数据互通
            Log.i(TAG, "程序员:" + msg.toString());

            msg.setContent("把用户交互颜色都改成粉色");
            msg.setLevel(3);
        }
    };
}
```

最后我们在清单文件中注册服务

action 为服务名称，客户端可以通过此(com.tengxun.aidl)隐式启动该服务。

<font size="4" color="#D2691E" style="box-sizing: border-box; font-family: Verdana !important;">在魅族的手机上，系统禁止了隐式方法启动服务的权限，所以务必在手机管家/权限管理/ 中，开启该项目的自启权限。</font>

```xml
        <service
            android:name=".AIDLService"
            android:exported="true">
            <intent-filter>
                <action android:name="com.tengxun.aidl" />
                <category android:name="android.intent.category.DEFAULT" />
            </intent-filter>

        </service>
```

### 客户端

创建客户端项目

##### 拷贝AIDL文件夹

将服务端创建的aidl文件夹拷贝至客户端app/src/main 目录下，并且在gradle.build中关联aidl路径。

```java
    sourceSets {
        main {
            java.srcDirs = ['src/main/java', 'src/main/aidl']
        }
    }
```

##### 开启服务

```java
        Intent intent = new Intent();
        intent.setAction("com.tengxun.aidl");//service的action
        intent.setPackage("qdx.aidlserver");//aidl文件夹里面aidl文件的包名
        bindService(intent, connection, Context.BIND_AUTO_CREATE);
```

##### 关联对象，调用方法

服务绑定成功之后，我们将服务端的`IDemandManager.Stub`对象，通过`asInterface`转化成为我们客户端需要的AIDL接口类型对象，并且这种转化过程是区分进程的（下篇详细叙述）。

```java
    private IDemandManager demandManager;

    private ServiceConnection connection = new ServiceConnection() {
        @Override
        public void onServiceConnected(ComponentName name, IBinder service) {
            demandManager = IDemandManager.Stub.asInterface(service);//得到该对象之后，我们就可以用来进行进程间的方法调用和传输啦。
        }

        @Override
        public void onServiceDisconnected(ComponentName name) {
        }
    };
```

至此我们就可以通过AIDL来进行跨进程通讯了。

## 附加技能（定时推送消息）

上面的步骤已经可以实现正常的跨进程通讯了，这时候如果我们想服务端项目想要定时推送消息到客户端项目，那么跨进程中如何完成呢?

——————使用观察者模式(多个回调方法的集合)——————

### 服务端项目(推送消息)

与通常的回调方法差不多，首先我们在AIDL文件夹里创建回调接口`IDemandListener.aidl`

但是这里的定向tag要稍加注意，可能有些童鞋觉得我们在服务端里给客户端发消息，照理不应该是设置out标记吗？

其实所谓的“服务端”和”客户端”在Binder通讯中是相对的，因为我们在客户端项目中拷贝了aidl文件，所以我们客户端项目其实不仅可以发送消息，充当”Client”，同时我们也可以收到服务端项目推送的消息，从而升级为”Server”，这个功能依赖于AIDL的Stub和Proxy……

所以服务端`onDemandReceiver`推送消息的过程实际上相当于”Client”

```java
package qdx.aidlserver;

import qdx.aidlserver.MessageBean;

interface IDemandListener {

     void onDemandReceiver(in MessageBean msg);//客户端->服务端

}
```

同时我们在`IDemandManager.aidl`文件中，加上绑定/解绑监听方法

```java
     void registerListener(IDemandListener listener);

     void unregisterListener(IDemandListener listener);
```

最后Clean Project，在`IDemandManager.Stub`中多处绑定/解绑两个方法。

```java
    IDemandManager.Stub demandManager = new IDemandManager.Stub() {
        ``````
        setDemandIn/out等方法
        ``````

        @Override
        public void registerListener(IDemandListener listener) throws RemoteException {
        }

        @Override
        public void unregisterListener(IDemandListener listener) throws RemoteException {

        }
    };

```

那么，关键的步骤来了，我们通常使用`List<IDemandListener>`来存放监听接口集合
但是！
<font size="4" color="#D2691E" style="box-sizing: border-box; font-family: Verdana !important;">由于跨进程传输客户端的同一对象会在服务端生成不同的对象！</font>

上面这句话说明跨进程通讯的过程中，这个传递的对象载体并不是像寄快递一样，从客户端传给服务端。而是经过中间人<font size="4" color="#D2691E" style="box-sizing: border-box; font-family: Verdana !important;">狸猫换太子</font>的一些手段传递的。

不过传递过程中间人(binder)对象都是同一个，所以Android通过这个特性提供<font size="4" color="#D2691E" style="box-sizing: border-box; font-family: Verdana !important;">RemoteCallbackList</font>，让我们用来存储监听接口集合。

这个<font size="4" color="#D2691E" style="box-sizing: border-box; font-family: Verdana !important;">RemoteCallbackList</font>内部自动实现了线程同步的功能，而且它的本质是一个`ArrayMap`，所以我们用它来绑定/解绑时，不需要做额外的线程同步操作。

```java
        private RemoteCallbackList<IDemandListener> demandList = new RemoteCallbackList<>();

        @Override
        public void registerListener(IDemandListener listener) throws RemoteException {
            demandList.register(listener);//
        }

        @Override
        public void unregisterListener(IDemandListener listener) throws RemoteException {
            demandList.unregister(listener);
        }
```

最后，我们通过Handler来进行定时发送消息。(handler并不能精准的做定时任务，因为handler在发送和接收的过程中会有时间损耗)

另外我们需要通过<font size="4" color="#D2691E" style="box-sizing: border-box; font-family: Verdana !important;">beginBroadcast()</font>来获取<font size="4" color="#D2691E" style="box-sizing: border-box; font-family: Verdana !important;">RemoteCallbackList中元素的个数</font>，同时<font size="4" color="#D2691E" style="box-sizing: border-box; font-family: Verdana !important;">beginBroadcast()和finishBroadcast()必须要配对使用</font>，哪怕仅仅只是获取一下这个集合的元素个数。

```java
    private Handler mHandler = new Handler() {
        @Override
        public void handleMessage(Message msg) {
            super.handleMessage(msg);

            if (demandList != null) {
                int nums = demandList.beginBroadcast();
                for (int i = 0; i < nums; i++) {
                    MessageBean messageBean = new MessageBean("我丢", count);
                    count++;
                    try {
                        demandList.getBroadcastItem(i).onDemandReceiver(messageBean);
                    } catch (RemoteException e) {
                        e.printStackTrace();
                    }
                }
                demandList.finishBroadcast();
            }

            mHandler.sendEmptyMessageDelayed(WHAT_MSG, 3000);//每3s推一次消息
        }
    };
```

### 客户端项目(接收定时推送)

在我们创建的客户端项目就不用过多的处理了，创建`IDemandListener.Stub`（此时我们这个客户端项目相对推送过程而言是”Server”），并且将它加入至`demandManager`即可。

```java
    IDemandListener.Stub listener = new IDemandListener.Stub() {
        @Override
        public void onDemandReceiver(final MessageBean msg) throws RemoteException {
            //该方法运行在Binder线程池中，是非ui线程

        }
    };

    demandManager.registerListener(listener);
```

### 项目下载
[http://download.csdn.net/download/qian520ao/9992461](http://download.csdn.net/download/qian520ao/9992461)




## AIDL工作流程
![](http://img.blog.csdn.net/20171228164803330?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

先用上图整体描述这个AIDL从客户端(Client)发起请求至服务端(Server)相应的工作流程，我们可以看出整体的核心就是<font size="4" color="#D2691E" style="box-sizing: border-box; font-family: Verdana !important;">Binder</font>

## AIDL工作流程解析

### asInterface
用于将服务端的Binder对象转换成客户端所需的AIDL接口类型的对象，这种转换过程是区分进程的【如果客户端和服务端位于同一进程，那么此方法返回的就是服务端的Stub对象本身，否则返回的是系统封装后的Stub.proxy对象】

```java
    private ServiceConnection connection = new ServiceConnection() {
        @Override
        public void onServiceConnected(ComponentName name, IBinder service) {

            IDemandManager demandManager = IDemandManager.Stub.asInterface(service);
        }
    };

```

在`ServiceConnection`绑定服务的方法里，我们通过`IDemandManager.Stub.asInterface(service)`方法获得`IDemandManager`对象，我们跟着`asInterface`步伐看看里面有什么名堂。

PS :
`onServiceConnected`方法的(IBinder)service参数在ActivityThread创建，他们之间还会扯出ActivityManagerService，ManagerService等，有兴趣的可以通过[Activity 的启动过程](http://blog.csdn.net/qian520ao/article/details/78156214 "optional title")加深功力，深入了解。

![](http://img.blog.csdn.net/20171228165008097?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
从上图的类结构图中我们可以看出，这个IDemandManager.aidl文件通过编译成为一个接口类，而这个类最核心的成员是<font size="4" color="#D2691E" style="box-sizing: border-box; font-family: Verdana !important;">Stub</font>类和Stub的内部代理类<font size="4" color="#D2691E" style="box-sizing: border-box; font-family: Verdana !important;">Proxy</font>。

顺着`asInterface`方法，结合上面对该方法的描述，可以看出通过`DESCRIPTOR`标识判断

<font size="4" color="#1d953f" style="box-sizing: border-box; font-family: Verdana !important;">如果是同一进程，那么就返回Stub对象本身(`obj.queryLocalInterface(DESCRIPTOR)`)，否则如果是跨进程则返回Stub的代理内部类Proxy。</font>

也就是说这个`asInterface`方法返回的是<font size="4" color="#D2691E" style="box-sizing: border-box; font-family: Verdana !important;">一个远程接口具备的能力</font>（有什么方法可以调用），在我们项目里，`asInterface`的能力就是get/setDemand和注册/解绑监听接口。

### asBinder

紧接着`asInterface`，我们看到一个简洁的方法`asBinder`

> 顾名思义，asBinder用于返回当前Binder对象。

```java
//Stub
        @Override
        public android.os.IBinder asBinder() {
            return this;
        }
```

```java
//Proxy
        private android.os.IBinder mRemote;

        @Override
        public android.os.IBinder asBinder() {
            return mRemote;
        }
```

根据代码我们可以追溯到，proxy的这个mRemote就是绑定服务(bindService)时候

由`IDemandManager.Stub.asInterface(binder);`传入的`IBinder`对象。

<font size="4" color="#D2691E" style="box-sizing: border-box; font-family: Verdana !important;">这个Binder对象具有跨进程能力，在Stub类里面（也就是本进程）直接就是Binder本地对象，在Proxy类里面返回的是远程代理对象(Binder代理对象)。</font>[所以跨进程的谜团，会随着对Binder的分析和研究，逐渐变得清晰起来。]

因为我们这个编译生成的`IDemandManager`接口继承了`android.os.IInterface`接口,所以我们先分析`IInterface`接口。

```java
public interface IDemandManager extends android.os.IInterface
```

### IInterface

而这个`IInterface`接口就只声明了一个方法，但是Stub和Proxy都分别间接的实现了该接口。

```java
/**
 * Base class for Binder interfaces.  When defining a new interface,
 * you must derive it from IInterface.
 */
public interface IInterface
{
    /**
     * Retrieve the Binder object associated with this interface.
     * You must use this instead of a plain cast, so that proxy objects
     * can return the correct result.
     */
    public IBinder asBinder();
}
```

> Binder接口的基类。 定义新接口时，你必须实现IInterface接口。
>
> 检索与此界面关联的Binder对象。
>
>  
>
> 你必须使用它而不是一个简单的转换
>
>  
>
> 这样代理对象才可以返回正确的结果。

从上面的系统注释中我们可以理解出：

1.  要声明(或者是手动diy创建)AIDL性质的接口，就要继承`IInterface`
2.  代表远程server对象具有的能力，具体是由`Binder`表达出这个能力。

### DESCRIPTOR

```java
private static final java.lang.String DESCRIPTOR = "qdx.aidlserver.IDemandManager";//系统生成的
```

Binder的唯一标识，一般用当前Binder的类名表示。

### onTransact(服务端接收)

我们发现`IDemandManager`接口，实际上并没有太多复杂的方法，看完了`asInterface`和`asBinder`方法，我们再来分析`onTransact`方法。

> `onTransact`方法运行在服务端中的<font size="4" color="#D2691E" style="box-sizing: border-box; font-family: Verdana !important;">Binder线程池中</font>
>
>  
>
> 客户端发起跨进程请求时，远程请求会通过系统底层封装后交给此方法来处理。
>
>  
>
> 如果此方法返回false,那么客户端的请求就会失败。

*   code : 确定客户端请求的目标方法是什么。（项目中的getDemand或者是setDemandIn方法）
*   data : 如果目标方法有参数的话，就从data取出目标方法所需的参数。
*   reply : 当目标方法执行完毕后，如果目标方法有返回值，就向reply中写入返回值。
*   flag : Additional operation flags. Either 0 for a normal RPC, or FLAG_ONEWAY for a one-way RPC.（暂时还没有发现用处，先标记上英文注释）
  ![](http://img.blog.csdn.net/20171228165528509?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

也就是说，这个`onTransact`方法就是服务端处理的核心，接收到客户端的请求，并且通过客户端携带的参数，执行完服务端的方法，返回结果。下面通过系统生成的代码，我们简要的分析一下`onTransact`方法里我们项目写的`setDemandIn`和`setDemandOut`方法。

```java
                case TRANSACTION_setDemandIn: {
                    data.enforceInterface(DESCRIPTOR);
                    qdx.aidlserver.MessageBean _arg0;
                    if ((0 != data.readInt())) {
                        _arg0 = qdx.aidlserver.MessageBean.CREATOR.createFromParcel(data);
                    } else {
                        _arg0 = null;
                    }
                    this.setDemandIn(_arg0);
                    reply.writeNoException();
                    return true;
                }
                case TRANSACTION_setDemandOut: {
                    data.enforceInterface(DESCRIPTOR);
                    qdx.aidlserver.MessageBean _arg0;
                    _arg0 = new qdx.aidlserver.MessageBean();
                    this.setDemandOut(_arg0);
                    reply.writeNoException();
                    if ((_arg0 != null)) {
                        reply.writeInt(1);
                        _arg0.writeToParcel(reply,android.os.Parcelable.PARCELABLE_WRITE_RETURN_VALUE);
                    } else {
                        reply.writeInt(0);
                    }
                    return true;
                }
```

此段代码并没有多少玄机，就是负责数据的读写，以及结果的返回。

但是有一个知识点可以在此验证，就是定向tag的作用，上面的方法定向tag分别是in和out，上篇文章介绍定向tag的作用就是跨进程中数据的流向，`setDemandIn`方法中我们可以看到我们读取了客户端传递过来的数据参数data，即客户端->服务端。out方法可以同理自行分析。[AIDL中的in,out,inout分析](http://www.jianshu.com/p/ddbb40c7a251 "optional title")

### transact(客户端调用)

分析完了Stub，就剩下Stub的内部代理类Proxy

惊奇的发现Proxy类主要是用来方法调用，也就是用来客户端的跨进程调用。

![](http://img.blog.csdn.net/20171228170537146?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


> `transact`方法运行在客户端，首先它创建该方法所需要的输入型Parcel对象_data、输出型Parcel对象_reply;
>
>  
>
> 接着调用绑定服务传来的IBinder对象的transact方法来发起<font size="4" color="#D2691E" style="box-sizing: border-box; font-family: Verdana !important;">远程过程调用</font>（RPC）请求，同时<font size="4" color="#D2691E" style="box-sizing: border-box; font-family: Verdana !important;">当前线程挂起</font>;
>
>  
>
> 然后服务端的`onTransact`方法会被调用，直到RPC过程返回后，当前线程继续执行，并从_reply中取出RPC过程的返回结果，也就是返回_reply中的数据。

我们看到获得`Parcel`的方法为`Parcel.obtain()`，按照套路这个应该就是从`Parcel`池中获取该对象，减少创建对象的开支，跟进方法我们可以看得的确是创建了一个`POOL_SIZE`为6的池用来获取`Parcel`对象。

<font size="4" color="#1d953f" style="box-sizing: border-box; font-family: Verdana !important;">所以在跨进程通讯中Parcel是通讯的基本单元，传递载体。</font>

而这个`transact`方法是一个本地方法，在native层中实现，功力不足，点到为止。

分析到了这里，感觉顿悟了许多，再一次引用开头概述的图片来看大概，整体的思路便浮在脑海中。
![](http://img.blog.csdn.net/20171228170716879?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

## 总结
通过对AIDL的分析，我们发现原来这一切围绕着<font size="4" color="#D2691E" style="box-sizing: border-box; font-family: Verdana !important;">Binder</font>有序的展开

AIDL通过Stub类用来接收并处理数据，Proxy代理类用来发送数据，而这两个类也只是通过对Binder的处理和调用。
![](http://img.blog.csdn.net/20171228170616175?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
所以所谓的服务端和客户端，我们拆开看之后发现原来竟是不同类的处理

Stub充当服务端角色，持有Binder实体（本地对象）。

*   获取客户端传过来的数据，根据方法 ID 执行相应操作。
*   将传过来的数据取出来，调用本地写好的对应方法。
*   将需要回传的数据写入 reply 流，传回客户端。

Proxy代理类充当客户端角色，持有Binder引用（句柄）。

*   生成 _data 和 _reply 数据流，并向 _data 中存入客户端的数据。
*   通过 transact() 方法将它们传递给服务端，并请求服务端调用指定方法。
*   接收 _reply 数据流，并从中取出服务端传回来的数据。

而且所谓的服务端和客户端都是相对而言的，服务端不仅可以接收和处理消息，而且可以定时往客户端发送数据，与此同时服务端使用Proxy类跨进程调用，相当于充当了”Client”。

并且有一点要理解是，跨进程通讯的时候，传递的数据对象并不是从进程A原原本本的发给进程B。库克说要送你苹果，而你收到的iphone X就真的是美国生产的吗？不，它也可能是made in China.

## 注意事项：
### 编写Aidl文件时，需要注意下面几点:
    1. 接口名和aidl文件名相同。
    2. 接口和方法前不用加访问权限修饰符public,private,protected等,也不能用final,static。
    3. Aidl默认支持的类型包话java基本类型（int、long、boolean等）和（String、List、Map、 CharSequence），使用这些类型时不需要import声明。对于List和Map中的元素类型必须是Aidl支持的类型。如果使用自定义类型作 为参数或返回值，自定义类型必须实现Parcelable接口。
    4. 自定义类型和AIDL生成的其它接口类型在aidl描述文件中，应该显式import，即便在该类和定义的包在同一个包中。
    5. 在aidl文件中所有非Java基本类型参数必须加上in、out、inout标记，以指明参数是输入参数、输出参数还是输入输出参数。
    6. Java原始类型默认的标记为in,不能为其它标记。


## 习题：
### 使用AIDL完成远程service方法调用下列说法不正确的是
A. aidl对应的接口名称不能与aidl文件名相同
B. aidl的文件的内容类似java代码
C. 创建一个Service（服务），在服务的onBind(Intent intent)方法中返回实现了aidl接口的对象
D. aidl对应的接口的方法前面不能加访问权限修饰符

正确答案: A 

引用：
[Android 深入浅出AIDL（一）](http://blog.csdn.net/qian520ao/article/details/78072250)
[Android 深入浅出AIDL（二）](http://blog.csdn.net/qian520ao/article/details/78074983)

