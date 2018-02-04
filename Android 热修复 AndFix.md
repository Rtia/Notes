

# 【Android 热修复 AndFix】
一般我们开发了的APP在上线之前都会进行全面的测试，等APP测试基本稳定后，公司会进行封版，待上线。这时如果开发人员又发现了bug，但是又封版了，不能再修复，防止引入新的问题。这时这个bug的修复就只能是在下一个版本再修复。但是，如果是一个小小的问题，我们就总是不停的修复后，发布新版本，用户就要不停的去下载安装。这样给用户的体验是很不好的，如果可以在用户不需要卸载旧的版本再安装新版本就能进行bug的修复。这时热修复就出现了。这里我们就讲解阿里实现的热修复功能的使用。 

阿里的实现逻辑是生成一个差分包**.apatch，合并修复bug。 

[阿里apatch工具下载地址](https://github.com/alibaba/AndFix) 

**实行逻辑**：每次启动的时候，去后台获取差分包fix.apatch，保存到本地，然后直接获取本地内存卡里面的fix.apatch修复本地的Bug 

## 实现步骤
**1、依赖阿里的热修复包，有两种方式：方式一是通过上面的下载链接，直接下载源码依赖包，把依赖包导入到我们的工程里去。方式二是直接线上依赖，直接在build.gradle中加入依赖** 
`dependencies { compile 'com.alipay.euler:andfix:0.5.0@aar' }` 
**2、在我们的Application中初始化PatchManager**

```java
import android.app.Application;
import android.content.pm.PackageManager;
import android.util.Log;

import com.alipay.euler.andfix.patch.PatchManager;
import com.taobao.sophix.PatchStatus;
import com.taobao.sophix.SophixManager;
import com.taobao.sophix.listener.PatchLoadStatusListener;

/**
 * @author : willkong
 * @date : 2017/12/25 18:23
 * @Description :
 */

public class MyApplication extends Application{
    public static PatchManager mPatchManager;
    @Override
    public void onCreate() {
        super.onCreate();
        //初始化阿里的热修复
        mPatchManager = new PatchManager(this);
        //初始化版本，获取当前应用的版本
        try {
            String appVersion = this.getPackageManager().getPackageInfo(this.getPackageName(),0).versionName;
            mPatchManager.init(appVersion);
        } catch (PackageManager.NameNotFoundException e) {
            e.printStackTrace();
        }
        //加载之前的apatch包
        mPatchManager.loadPatch();
    }
 }
```

**3、在AndroidManifest中申明权限** 

```xml
<!-- 网络权限 --> <uses-permission android:name="android.permission.INTERNET" /> <uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" /> <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" /> <!-- 外部存储读权限，调试工具加载本地补丁需要 --> <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>`
```

在MainActivity中获取到fix.apatch，修复bug

```java
import android.content.Context;
import android.net.ConnectivityManager;
import android.net.NetworkInfo;
import android.os.Bundle;
import android.os.Environment;
import android.support.v7.app.AppCompatActivity;
import android.util.Log;
import android.view.View;
import android.widget.Button;
import android.widget.TextView;
import android.widget.Toast;

import java.io.File;
import java.io.IOException;

public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        Button bt = findViewById(R.id.bt);
        bt.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                Toast.makeText(MainActivity.this,2/0+"测试",Toast.LENGTH_SHORT).show();
            }
        });

        File fixFile = new File(Environment.getExternalStorageDirectory(),"fix.apatch");
        if (fixFile.exists()){
            //修复bug
            try {
                MyApplication.mPatchManager.addPatch(fixFile.getAbsolutePath());
                Toast.makeText(this,"修复成功",Toast.LENGTH_SHORT).show();
            } catch (IOException e) {
                e.printStackTrace();
                Toast.makeText(this,"修复失败",Toast.LENGTH_SHORT).show();
            }
        }
    }
}
```

**4、生成一个密钥jks，签名生成一个有bug和一个没bug的两个包** 
**5、从上面的网址下载得到生成差分包工具解压包apkpatch-1.0.3.zip** 
**6、在命令台输入命令：**

```
C:\Users\willkong\Desktop\AndFix-master\tools>apkpatch.bat -f new.apk -t old.apk
 -o out -k **.jks -p ****** -a ** -e ******
```

C:\Users\willkong\Desktop\AndFix-master\tools>是apkpatch.bat差分包生成工具的路径。 
生成差分包的命令是： 

```
apkpatch.bat -f -t -o -k -p<*> -a -e<*>
```

-f : 没有bug的新版本apk 
-t : 有bug的旧版本apk 
-o : 生成的补丁文件所放的文件夹 
-k : 签名打包密钥 
-p ：签名打包密钥密码 
-a : 签名密钥别名 
-e : 签名别名密码（这里一般和密钥密码一致）

**7、把生成 的差分包改名为fix.apatch,复制到手机内存中，重新打开程序，则程序就自动修复了bug**



引用：
[阿里热修复AndFix的使用教程](http://blog.csdn.net/jiang547860818/article/details/78909688)
