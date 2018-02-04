

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
2.  广播接收者中**不要做**一些**耗时**的工作，否则会弹出 **Application No Response** 错误对话框；
3.  最好也**不要**在广播接收者中**创建子线程**做耗时的工作，因为广播接收者被销毁后进程就成为了空进程，很容易被系统杀掉；
4.  **耗时**的较长的工作可以通过Intent**启动Service**来完成，但不能绑定Service。



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
>**normal** ： 低 风 险 权 限 ， 只 要 申 请 了 就 可 以 使 用 （ 在 AndroidManifest.xml 中 添 加`<uses-permission>`标签），安装时不需要用户确认；
>**dangerous**：高风险权限，安装时需要用户的确认才可使用；
>**signature**：只有当申请权限的应用程序的数字签名与声明此权限的应用程序的数字签名相同时（如果是申请系统权限，则需要与系统签名相同），才能将权限授给它；
>**signatureOrSystem**：签名相同，或者申请权限的应用为系统应用（在 system image 中）。
>上述四类权限级别同样可用于自定义权限中。如果开发者需要对自己的应用程序（或部分应用）进行 访 问 控 制 ， 则 可 以 通 过 在 AndroidManifest.xml 中 添 加 `<permission>` 标 签 ， 将 其 属 性 中 的protectionLevel 设置为上述四类级别中的某一种来实现。




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



