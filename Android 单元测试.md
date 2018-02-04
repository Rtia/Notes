

# 【Android 单元测试】



## 什么是单元测试

首先总结一下什么是单元测试，单元测试中的单元在Android或Java中可以理解为某个类中的某一个方法，因此单元测试就是针对Android或Java中某个类中的某一个方法中的逻辑代码进行验证即测试该方法是不是可以正常工作。

还有一点就是要区分单元测试与集成测试（功能测试、UI测试），**单元测试**是针对单元即**方法**的**测试**，被测单元粒度要小并且具备独立性，而**集成测试**是测试**多个单元（方法）组合成的功能模块**。

## 为什么需要进行单元测试

*   **单元测试的测试相对于集成测试的测试成本较低**

　　单元测试相对于集成测试有运行时间短、投入成本低的优势即[**Test Pyramid**](https://link.jianshu.com?t=http://martinfowler.com/bliki/TestPyramid.html)理论：
![Test Pyramid](http://img.blog.csdn.net/20180115173138340?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

　　从上图可以看出单元测试，测试速度快投入成本少

　　因此我们要将大部分精力投放在单元测试中，保证单元测试的质量之后再进行集成测试与UI测试来提高测试效率

*   **提高开发效率**

　　开发Android App的小伙伴可能都会有这样一个体会，就是当App项目逐渐增大，运行App进行调试会花费大量时间在项目的构建、编译、打包、安装上。这个过程的持续时间与App的规模成线性相关即App项目规模越大持续时间就越久。因此随着我们的的项目逐渐增大，运行App的进行调试时，我们的调试成本也在逐渐增加。

　　而单元测试正好能解决这个问题。

　　举个例子：

　　在登录Activity中有个checkPhoneNum方法，这个方法的功能是在点击登录按钮时，对用户输入的登录账号进行本地的合法性验证避免不必要的网络请求，如果是通过运行App来验证checkPhoneNum方法是否能够正常运行，需要经过构建、编译、打包、安装的过程，程序运行之后还需要人工操作进入登陆页面，输入账号密码，点击登录按钮，触发checkPhoneNum方法，这个过程可能需要几十秒甚至一分多钟，如果通过MVP架构将checkPhoneNum作为纯Java代码抽离出来，屏蔽对Android平台的依赖，就能将单元测试运行在JVM上，并针对checkPhoneNum方法进行测试，免去了构建、编译、打包、安装的过程，整个验证过程就在一秒之内，开发效率将大幅提升。（大致的测试流程在下个章节进行说明）

```java
public boolean checkPhoneNum(String phoneNum){
  //判断phoneNum是否为空（实际的判断会稍微复杂一点,为了举例做了简化）       
  if(phoneNum == null || "".equals(phoneNum)){
    return false;
  }
  return true;
}
```

*   **提升项目工程代码质量**

　　进行单元测试前提之一就是被测单元具备可测性，以上面checkPhoneNum方法为例，如果checkPhoneNum方法中的代码直接写在登录按钮的点击事件中，而没有抽取为checkPhoneNum方法，那么对这段代码进行单元测试是会非常困难的，极端情况甚至无法测试。所以为了写出可测试的代码可以锻炼开发人员对的代码的抽象能力和加强对项目架构的把控，从而提升项目工程代码质量。

*   **快速定位Bug**

　　由于单元测试对被测项目中的被测单元的独立性的要求，因此在被测单元的执行结果与预期结果不一致时我们就能快速的定位到出现Bug的方法。（在下个章节中会举例说明）
　　
# 在Android Studio中进行单元测试
在现在的Android studio里头，当你创建项目的时候 ，会自动地给你添加测试依赖的，不需要手动添加。
默认添加的依赖：
![依赖](http://img.blog.csdn.net/20180116164115800?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
app/build.gradle
```java
android {
    defaultConfig {
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
    }
}
dependencies {
    testImplementation 'junit:junit:4.12'
    androidTestImplementation 'com.android.support.test:runner:1.0.1'
    androidTestImplementation 'com.android.support.test.espresso:espresso-core:3.0.1'
}
```

文件夹有三个：
- 一个是普通的代码文件夹
- 一个JVM里头的测试的（**test**）
  是测试不涉及Activity，UI组件的纯Java方法。直接在电脑上直接测试
- 一个是真机或者模拟器的测试文件夹（**androidTest**）：
  涉及UI，Android组件的都在该路径下测试。需要连接真机，或者模拟器进行测试。


![文件夹](http://img.blog.csdn.net/20180116170431814?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


## Java方法单元测试

### 首先，创建一个用于测试的类：

```java
public class Caculation {
    public double sum(double numA, double numB) {
        return numA + numB;
    }
    public double multiply(double numA, double numB) {
        return numA * numB;
    }
}
```
### 创建测试类
将鼠标放之类名之上 按下 **ALT+ENTER** 便可创建测试文件
![create](http://img.blog.csdn.net/20180116165042216?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![create2](http://img.blog.csdn.net/20180116165108280?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
**setUp**():为测试做准备，比如类的实例化，文件的读取等等。
**tearDown**()：方法是为测试做扫尾工作，比如io的关闭等等。

![create3](http://img.blog.csdn.net/20180116165114910?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
放在test文件夹下


```java
import org.junit.Before;
import org.junit.Test;
import static junit.framework.Assert.assertEquals;
public class CaculationTest {
    private Caculation mCaculation;
    @Before
    public void setUp() throws Exception {
        //Here we should new a instance
        mCaculation = new Caculation();
    }

    @Test
    public void testSum() throws Exception {
        assertEquals(2,mCaculation.sum(1,1),0);
    }

    @Test
    public void testMultiply() throws Exception {
        assertEquals(10,mCaculation.multiply(2,5),0);
    }
}
```

代码很简单，仅仅是几个方法而已。在之前的话，需要创建对象嘛，这个可以理解吧，所以先创建一个对象，后面的就用断言的方式来测试即可。

### 运行
![运行](http://img.blog.csdn.net/20180116170638056?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

结果如下：如果是绿条，明测试通过；红色，则是结果与预期不一致。

## UI单元测试

### 新增被测试界面

接下来呢，就是添加一些简易的测试指令。先是复制下面的布局到activity_main.xml里

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
                xmlns:tools="http://schemas.android.com/tools"
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:paddingBottom="@dimen/activity_vertical_margin"
                android:paddingLeft="@dimen/activity_horizontal_margin"
                android:paddingRight="@dimen/activity_horizontal_margin"
                android:paddingTop="@dimen/activity_vertical_margin"
                tools:context=".MainActivity">

    <TextView
        android:id="@+id/textView"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="hello world!"/>

    <EditText
        android:id="@+id/editText"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@+id/textView"
        android:hint="Enter your name here"/>

    <Button
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@+id/editText"
        android:onClick="sayHello"
        android:text="Say hello!"/>
</RelativeLayout>
```

它看起来就这个样子的哈：

![界面](http://img.blog.csdn.net/20180116170630101?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

以下是MainActivity的代码：

```java
public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
    }

    public void sayHello(View v){
        TextView textView = (TextView) findViewById(R.id.textView);
        EditText editText = (EditText) findViewById(R.id.editText);
        textView.setText("Hello, " + editText.getText().toString() + "!");
    }
}
```

### 创建测试类
与上面一致，但是放在androidTest文件夹下
```java
import android.support.test.rule.ActivityTestRule;
import android.support.test.runner.AndroidJUnit4;
import android.test.suitebuilder.annotation.LargeTest;

import org.junit.Rule;
import org.junit.Test;
import org.junit.runner.RunWith;

import static android.support.test.espresso.Espresso.onView;
import static android.support.test.espresso.action.ViewActions.click;
import static android.support.test.espresso.action.ViewActions.closeSoftKeyboard;
import static android.support.test.espresso.action.ViewActions.typeText;
import static android.support.test.espresso.assertion.ViewAssertions.matches;
import static android.support.test.espresso.matcher.ViewMatchers.withId;
import static android.support.test.espresso.matcher.ViewMatchers.withText;

@RunWith(AndroidJUnit4.class)
@LargeTest
public class MainActivityInstrumentationTest {

    private static final String STRING_TO_BE_TYPED = "Peter";

    @Rule
    public ActivityTestRule<MainActivity> mActivityRule = new ActivityTestRule<>(
            MainActivity.class);

    @Test
    public void sayHello() {
        onView(withId(R.id.editText)).perform(typeText(STRING_TO_BE_TYPED), closeSoftKeyboard()); 

        onView(withText("Say hello!")).perform(click()); 

        String expectedText = "Hello, " + STRING_TO_BE_TYPED + "!";
        onView(withId(R.id.textView)).check(matches(withText(expectedText))); 
    }

}
```
#### 代码说明
**@Rule**：是让我们先定好规则，该规则所关联的界面是MainActivity;

由于在UI测试中我们需要操作UI元素，但是我们的测试类中并未引入Activity实例，而espresso为我们提供了 onView 来注入组件。
**onView**(**withId**(R.id.editText)).**perform**(**typeText**(STRING_TO_BE_TYPED), closeSoftKeyboard());
上面是通过withId找到组件，写入Str 所指向的字符串，并关闭软键盘。

 onView(withId(R.id.textView)).**check**(**matches**(**withText**(expectedText)));
判断这个组件中的文本是否为之前输入的文本。


执行一下，你就会发现，这个应用跑起来，然后会自动地输入设定的内容，接着就消失了。绿条表示测试通过：
![运行UI1](http://img.blog.csdn.net/20180116170622874?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
![运行UI2](http://img.blog.csdn.net/20180116170615774?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

## Mockito测试
Mockito官网：http://site.mockito.org/
依赖：
```java
dependencies { testCompile "org.mockito:mockito-core:2.+" }
```

我们模拟一个场景：我们在学校教务软件中要获取学生的信息，获取方式有两种第一种是通过本地数据库获取，第二种是通过网络获取。

为此我们新建一个Student类来描述学生信息如下：

```java
public class Student {
    public int id ;
    public String name;
    @Override
    public String toString() {
        return "Student{" +
                "id=" + id +
                ", name='" + name + '\'' +
                '}';
    }
}
```

接下来我们建一个接口用来获取学生的信息。

```java
public interface StudentDao {
    Student getStudentFromDB(int sid);
}
```

但是此时我们不去实现这个接口，因为工作时很多情况都是分工合作，你做的功能需要别人的配合，往往会影响开发效率，那么我们该怎么去测试自己的代码呢，下面我们建立一个StuController类来控制是从何处获取学生信息。

```java
public class StuController {
    private StudentDao studentDao;

    public Student getStudentInfo(int sid){
        Student student = null;
        if(studentDao != null){
            student = studentDao.getStudentFromDB(sid);
        }
        if(student == null){
            student = fetchStudent(sid);
        }
        return student;
    }

    private Student fetchStudent(int sid) {
        System.out.print("模拟网络获取学生信息");
        Student student = new Student();
        student.id = 12;
        student.name = "网络名";
        return student;
    }

    public void setStudentDao(StudentDao studentDao) {
        this.studentDao = studentDao;
    }
}
```

以上代码相信大家都能看懂，下面我们建立一个**Student测试类**
```java
public class StuControllerTest {
    StuController stuController;
    StudentDao studentDao;
    @Before
    public void setUp() throws Exception {
        stuController = new StuController();
        studentDao = mock(StudentDao.class);
        stuController.setStudentDao(studentDao);
    }

    @After
    public void tearDown() throws Exception {

    }

    @Test
    public void getStudentInfo() throws Exception {
        Student returnStudent = new Student();
        returnStudent.id = 123;
        returnStudent.name = "Mock-user";

        when(studentDao.getStudentFromDB(anyInt())).thenReturn(returnStudent);

        Student student = stuController.getStudentInfo(123);
        TestCase.assertEquals(123,student.id);
        TestCase.assertEquals("Mock-user",student.name);

        System.out.print(studentDao.getStudentFromDB(1));

    }

    @Test
    public void testGetStudentInfoFromSever(){
        when(studentDao.getStudentFromDB(anyInt())).thenReturn(null);
        Student student = stuController.getStudentInfo(456);
        TestCase.assertEquals(12,student.id);
        TestCase.assertEquals("网络名",student.name);
    }
}
```

从上面代码可以看到，我们并没有实现StudentDao,我们只是应用Mock 创建了一个studentDao的实例。

下面这个方法是测试数据库获取数据是否成功。

```java
@Test
public void getStudentInfo() throws Exception {
```

而这个方法就是测试网络数据是否获取成功。

```java
 @Test
public void testGetStudentInfoFromSever(){
```

以下便是[测试结果.png](http://img.blog.csdn.net/20180116171543916?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

![测试结果](http://img.blog.csdn.net/20180116171543916?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


引用：
[Android studio中单元测试和UI测试](http://blog.csdn.net/dengpeng_/article/details/54908473)
[AndroidStudio测试（二）UI测试](https://www.jianshu.com/p/90095c989311)
[AndroidStudio测试（四）单元测试Mock以及Mockito的使用](https://www.jianshu.com/p/d61a1e9acc0e)
[Android单元测试——初探](https://www.jianshu.com/p/79addb29b06d)

