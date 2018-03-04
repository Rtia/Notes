<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [【Kotlin 入门】](#kotlin-%E5%85%A5%E9%97%A8)
  - [引入](#%E5%BC%95%E5%85%A5)
  - [变量](#%E5%8F%98%E9%87%8F)
  - [常量](#%E5%B8%B8%E9%87%8F)
  - [函数](#%E5%87%BD%E6%95%B0)
      - [定义一个函数接受两个 Int 型参数，返回值类型为 Int ：](#%E5%AE%9A%E4%B9%89%E4%B8%80%E4%B8%AA%E5%87%BD%E6%95%B0%E6%8E%A5%E5%8F%97%E4%B8%A4%E4%B8%AA-int-%E5%9E%8B%E5%8F%82%E6%95%B0%E8%BF%94%E5%9B%9E%E5%80%BC%E7%B1%BB%E5%9E%8B%E4%B8%BA-int-)
      - [只有一个表达式作为函数体，以及自推导型的返回值：](#%E5%8F%AA%E6%9C%89%E4%B8%80%E4%B8%AA%E8%A1%A8%E8%BE%BE%E5%BC%8F%E4%BD%9C%E4%B8%BA%E5%87%BD%E6%95%B0%E4%BD%93%E4%BB%A5%E5%8F%8A%E8%87%AA%E6%8E%A8%E5%AF%BC%E5%9E%8B%E7%9A%84%E8%BF%94%E5%9B%9E%E5%80%BC)
      - [函数的参数可以指定默认值：](#%E5%87%BD%E6%95%B0%E7%9A%84%E5%8F%82%E6%95%B0%E5%8F%AF%E4%BB%A5%E6%8C%87%E5%AE%9A%E9%BB%98%E8%AE%A4%E5%80%BC)
      - [Unit 表示无返回值，对应 java 中 void：](#unit-%E8%A1%A8%E7%A4%BA%E6%97%A0%E8%BF%94%E5%9B%9E%E5%80%BC%E5%AF%B9%E5%BA%94-java-%E4%B8%AD-void)
      - [返回类型为Unit的可以省略不写：](#%E8%BF%94%E5%9B%9E%E7%B1%BB%E5%9E%8B%E4%B8%BAunit%E7%9A%84%E5%8F%AF%E4%BB%A5%E7%9C%81%E7%95%A5%E4%B8%8D%E5%86%99)
  - [空安全](#%E7%A9%BA%E5%AE%89%E5%85%A8)
      - [**默认定义的变量不能为 null** ：](#%E9%BB%98%E8%AE%A4%E5%AE%9A%E4%B9%89%E7%9A%84%E5%8F%98%E9%87%8F%E4%B8%8D%E8%83%BD%E4%B8%BA-null-)
      - [指定一个变量可null是**通过在类型的最后增加一个问号**：](#%E6%8C%87%E5%AE%9A%E4%B8%80%E4%B8%AA%E5%8F%98%E9%87%8F%E5%8F%AFnull%E6%98%AF%E9%80%9A%E8%BF%87%E5%9C%A8%E7%B1%BB%E5%9E%8B%E7%9A%84%E6%9C%80%E5%90%8E%E5%A2%9E%E5%8A%A0%E4%B8%80%E4%B8%AA%E9%97%AE%E5%8F%B7)
      - [当变量声明为可空时，在调用它的属性时无法通过编译：](#%E5%BD%93%E5%8F%98%E9%87%8F%E5%A3%B0%E6%98%8E%E4%B8%BA%E5%8F%AF%E7%A9%BA%E6%97%B6%E5%9C%A8%E8%B0%83%E7%94%A8%E5%AE%83%E7%9A%84%E5%B1%9E%E6%80%A7%E6%97%B6%E6%97%A0%E6%B3%95%E9%80%9A%E8%BF%87%E7%BC%96%E8%AF%91)
      - [当变量声明为可空时，在调用它的属性时需使用**安全操作符 `?.`**：](#%E5%BD%93%E5%8F%98%E9%87%8F%E5%A3%B0%E6%98%8E%E4%B8%BA%E5%8F%AF%E7%A9%BA%E6%97%B6%E5%9C%A8%E8%B0%83%E7%94%A8%E5%AE%83%E7%9A%84%E5%B1%9E%E6%80%A7%E6%97%B6%E9%9C%80%E4%BD%BF%E7%94%A8%E5%AE%89%E5%85%A8%E6%93%8D%E4%BD%9C%E7%AC%A6-)
        - [**`?:`** 操作符](#-%E6%93%8D%E4%BD%9C%E7%AC%A6)
      - [如确定该变量不为空，可以使用 **`!!` 操作符**：](#%E5%A6%82%E7%A1%AE%E5%AE%9A%E8%AF%A5%E5%8F%98%E9%87%8F%E4%B8%8D%E4%B8%BA%E7%A9%BA%E5%8F%AF%E4%BB%A5%E4%BD%BF%E7%94%A8--%E6%93%8D%E4%BD%9C%E7%AC%A6)
  - [类定义](#%E7%B1%BB%E5%AE%9A%E4%B9%89)
  - [类继承](#%E7%B1%BB%E7%BB%A7%E6%89%BF)
    - [声明父类](#%E5%A3%B0%E6%98%8E%E7%88%B6%E7%B1%BB)
    - [子类有主构造函数](#%E5%AD%90%E7%B1%BB%E6%9C%89%E4%B8%BB%E6%9E%84%E9%80%A0%E5%87%BD%E6%95%B0)
    - [子类没有主构造函数](#%E5%AD%90%E7%B1%BB%E6%B2%A1%E6%9C%89%E4%B8%BB%E6%9E%84%E9%80%A0%E5%87%BD%E6%95%B0)
    - [重写函数](#%E9%87%8D%E5%86%99%E5%87%BD%E6%95%B0)
    - [子类继承父类的函数](#%E5%AD%90%E7%B1%BB%E7%BB%A7%E6%89%BF%E7%88%B6%E7%B1%BB%E7%9A%84%E5%87%BD%E6%95%B0)
    - [子类继承父类的成员变量](#%E5%AD%90%E7%B1%BB%E7%BB%A7%E6%89%BF%E7%88%B6%E7%B1%BB%E7%9A%84%E6%88%90%E5%91%98%E5%8F%98%E9%87%8F)
  - [数据类](#%E6%95%B0%E6%8D%AE%E7%B1%BB)
  - [接口定义](#%E6%8E%A5%E5%8F%A3%E5%AE%9A%E4%B9%89)
  - [冒号](#%E5%86%92%E5%8F%B7)
  - [可见性](#%E5%8F%AF%E8%A7%81%E6%80%A7)
  - [扩展函数](#%E6%89%A9%E5%B1%95%E5%87%BD%E6%95%B0)
  - [Anko](#anko)
    - [添加Anko的依赖：](#%E6%B7%BB%E5%8A%A0anko%E7%9A%84%E4%BE%9D%E8%B5%96)
    - [执行Activity的跳转：](#%E6%89%A7%E8%A1%8Cactivity%E7%9A%84%E8%B7%B3%E8%BD%AC)
    - [在Activity中显示Toast：](#%E5%9C%A8activity%E4%B8%AD%E6%98%BE%E7%A4%BAtoast)
    - [线程切换：](#%E7%BA%BF%E7%A8%8B%E5%88%87%E6%8D%A2)
  - [对象表达式和对象声明](#%E5%AF%B9%E8%B1%A1%E8%A1%A8%E8%BE%BE%E5%BC%8F%E5%92%8C%E5%AF%B9%E8%B1%A1%E5%A3%B0%E6%98%8E)
    - [对象表达式（Object expressions）](#%E5%AF%B9%E8%B1%A1%E8%A1%A8%E8%BE%BE%E5%BC%8Fobject-expressions)
    - [对象声明（Object declarations）](#%E5%AF%B9%E8%B1%A1%E5%A3%B0%E6%98%8Eobject-declarations)
      - [伴随对象（Companion Objects）](#%E4%BC%B4%E9%9A%8F%E5%AF%B9%E8%B1%A1companion-objects)
    - [对象表达式和对象声明在语义上的区别](#%E5%AF%B9%E8%B1%A1%E8%A1%A8%E8%BE%BE%E5%BC%8F%E5%92%8C%E5%AF%B9%E8%B1%A1%E5%A3%B0%E6%98%8E%E5%9C%A8%E8%AF%AD%E4%B9%89%E4%B8%8A%E7%9A%84%E5%8C%BA%E5%88%AB)
  - [Lambda表达式](#lambda%E8%A1%A8%E8%BE%BE%E5%BC%8F)
  - [When表达式](#when%E8%A1%A8%E8%BE%BE%E5%BC%8F)
  - [with函数](#with%E5%87%BD%E6%95%B0)
  - [内联函数](#%E5%86%85%E8%81%94%E5%87%BD%E6%95%B0)
  - [Kotlin Android Extensions](#kotlin-android-extensions)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 【Kotlin 入门】
本文介绍了Kotlin入门应该知道一些基本语法概念。包括变量、常量、函数、空安全、类定义、类继承、数据类、接口定义、冒号、可见性、扩展函数、Anko、对象表达式和声明、Lambda表达式、when表达式、with函数、内联函数、Kotlin Android Extensions等。

本文所有用例基于Android Studio 3.0.1、Kotlin 1.2版本。

## 引入

在项目根目录下 `build.gradle` 文件中添加 kotlin 插件依赖：
``` java
buildscript {
    ext.gradle_plugin_version = '3.0.1'
    ext.kotlin_version = '1.2.0'
    repositories {
        jcenter()
        google()
    }
    dependencies {
        classpath "com.android.tools.build:gradle:$gradle_plugin_version"
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
    }
}
``` 

在主 module 下 `build.gradle` 文件中添加 kotlin 依赖：

``` java
apply plugin: 'com.android.application'
apply plugin: 'kotlin-android'
...
...
dependencies {
    implementation "org.jetbrains.kotlin:kotlin-stdlib:$kotlin_version"
}
``` 

如果开启了 Data Binding，还需要添加如下依赖：

``` java
apply plugin: 'com.android.application'
apply plugin: 'kotlin-android'
apply plugin: 'kotlin-kapt'

android {
    dataBinding {
        enabled = true
    }
}

dependencies {
    implementation "org.jetbrains.kotlin:kotlin-stdlib:$kotlin_version"
    kapt "com.android.databinding:compiler:$gradle_plugin_version"
}
``` 

## 变量
在 kotlin 中**一切皆为对象**，**没有**像 Java 中的**原始基本类型**。在 kotlin 中使用 **`var` 修饰的为变量**。
例如我们定义一个 Int 类型的变量并赋值为1：

``` kotlin
var a: Int = 1
a += 1
``` 

由于 kotlin 编译器**可**以**自动推断**出**变量**的**类型**，所以我们**通常不需要指定变量的类型**：
``` kotlin
var s = "String" //类型为String
var a = 1 //类型为Int
``` 

在 kotlin 中**分号不是必须**的，不使用分号是一个不错的实践。

## 常量
在 kotlin 中使用 **`val` 修饰的为常量**。这和 java 中的 `final` 很相似。
在 kotlin 中有一个重要的概念是：**尽可能地使用 `val`**。
``` kotlin
val s = "String" //类型为String
val ll = 22L //类型为Long
val d = 2.5 //类型为Double
val f = 5.5F //类型为Float
``` 

## 函数

#### 定义一个函数接受两个 Int 型参数，返回值类型为 Int ：
``` kotlin
fun sum(a: Int, b: Int): Int {
    return a + b
}
``` 

#### 只有一个表达式作为函数体，以及自推导型的返回值：

``` kotlin
fun sum(a: Int, b: Int) = a + b
``` 

#### 函数的参数可以指定默认值：

``` kotlin
fun sum(a: Int, b: Int = 10) = a + b
var c = sum(10) //调用
``` 

#### Unit 表示无返回值，对应 java 中 void：

``` kotlin
fun printSum(a: Int, b: Int): Unit {
    println("sum of $a and $b is ${a + b}")
}
``` 

#### 返回类型为Unit的可以省略不写：
``` kotlin
fun printSum(a: Int, b: Int) {
    println("sum of $a and $b is ${a + b}")
}
``` 

## 空安全

#### **默认定义的变量不能为 null** ：
这可以避免很多的 NullPointerException。
``` kotlin
var a: String ="abc"
a = null //编译错误
``` 

#### 指定一个变量可null是**通过在类型的最后增加一个问号**：

``` kotlin
var b: String? = "abc"
b = null
``` 

#### 当变量声明为可空时，在调用它的属性时无法通过编译：
``` kotlin
var b: String? = "abc"
val l = b.length //编译错误
``` 

#### 当变量声明为可空时，在调用它的属性时需使用**安全操作符 `?.`**：

``` kotlin
var b: String? = "abc"
val l = b?.length//如果 b 不为空则返回长度，否则返回空，这个表达式的的类型是 `Int?`。
``` 
#####  **`?:`** 操作符
我们还可以使用 `?:` 操作符，当**前面的值不为空取前面的值，否则取后面的值**，这和java中三目运算符类似。
``` kotlin
val a:Int? = null
val myString = a?.toString() ?: ""
``` 

因为在Kotlin中 throw 和 return 都是表达式，他们可以用在Elvis operator操作符的右边：

``` kotlin
val myString = a?.toString() ?: return false
val myString = a?.toString() ?: throw IllegalStateException()
``` 

#### 如确定该变量不为空，可以使用 **`!!` 操作符**：
``` kotlin
var b: String? = "abc"
val l = b!!.length
``` 
使用 `!!` 操作符可以跳过限制检查通过编译，此时**如果变量为空会抛出空指针异常**。如果大量使用此操作符，显然不是很好的处理。

## 类定义
使用 class 定义一个类。类的声明**包含类名，类头(指定类型参数，主构造函数等等)，以及类主体**，用大括号包裹。
**类头**和**类体**是**可选**的；如果**没**有**类体**可以**省略大括号**。

``` kotlin
class MainActivity{
}
``` 

在 Kotlin 中类可以有一个主构造函数以及多个二级构造函数。
**主构造函数是类头的一部分**：跟在类名后面(可以有可选的类型参数)。
``` kotlin
class Person constructor(firstName: String) {
}
``` 

如果主构造函数没有注解或可见性说明，则 **`constructor` 关键字是可以省略**：
``` kotlin
class Person(name: String, surname: String)
``` 

构造函数的函数体可以写在 `init` 块中：
``` kotlin
class Customer(name: String) {
    init {
        logger.info("Customer initialized with value ${name}")
    }
}
``` 

注意主构造函数的参数可以用在初始化块内，也可以用在类的属性初始化声明处：
``` kotlin
class Customer(name: String) {
    val customerKry = name.toUpperCase()
}
``` 

事实上，声明属性并在主构造函数中初始化,在 Kotlin 中有更简单的语法：
``` kotlin
class Person(val firstName: String, val lastName: String, var age: Int) {
}
``` 

就像普通的属性，在主构造函数中的属性可以是可变的( var )或只读的( val )。

## 类继承
Kotlin 中所有的类都有共同的父类 `Any`，它是一个没有父类声明的类的默认父类：
``` kotlin
class Example //　隐式继承于 Any
``` 
Any 不是 java.lang.Object ；事实上它除了 equals() , hashCode() 以及 toString() 外没有任何成员了。
**默认情况下，kotlin 中所有的类都是不可继承 (final) 的**，所以我们**只能继承**那些明确声明为 **`open`** 或 **`abstract`** 的类。

### 声明父类
声明一个明确的父类， 在类头后加冒号再加父类即可
``` kotlin
open class Base(p: Ont)
class Derived(p: Int) : Base(p)
```
### 子类有主构造函数
如果子类有主构造函数， 则基类必须在主构造函数中立即初始化。
```kotlin
open class Person(var name : String, var age : Int){
}
class Student(name : String, age : Int, var no : String, var score : Int) : Person(name, age) {
}
```

### 子类没有主构造函数
如果子类没有主构造函数，则必须在每一个二级构造函数中用 super 关键字初始化基类，或者在代理另一个构造函数。初始化基类时，可以调用基类的不同构造方法。
``` kotlin
calss MyView : View {
    constructor(ctx: Context) : super(ctx) {
    } 
    constructor(ctx: Context, attrs: AttributeSet) : super(ctx,attrs) {
    }
}
```

### 重写函数
在父类中，使用fun声明**函数**时，此函数**默认为final**修饰，**不能被**子类**重写**。
如果**允许**子类**重写**该函数，那么就要手动**添加open**修饰它。
``` kotlin
open class Person(var name : String, var age : Int) : Any() {
    override fun toString(): String{
        return "Person(name='$name', age=$age)"
    }
}
```

### 子类继承父类的函数
在kotlin中， 实现继承通常遵循如下规则：如果一个类从它的直接父类继承了同一个函数的多个实现，那么它必须重写这个函数并且提供自己的实现(或许只是直接用了继承来的实现) 为表示使用父类中提供的方法我们用 super 表示。
``` kotlin
open class A {
    open fun f () { print("A") }
    fun a() { print("a") }
} 
interface B {
    fun f() { print("B") } //接口的成员变量默认是 open 的
    fun b() { print("b") }
} 
class C() : A() , B{
    override fun f() {
        super<A>.f()//调用 A.f()
        super<B>.f()//调用 B.f()
    }
}
```
C继承自a()或 b(),C不仅可以从A或则B中继承函数，而且C可以继承A()、B()中共有的函数。此时该函数在中只有一个实现，为了消除歧义，该函数必须调用A()和B()中该函数的实现，并提供自己的实现。

### 子类继承父类的成员变量
当子类继承了某个类之后，便可以使用父类中的成员变量，但是并不是完全继承父类的所有成员变量。具体的原则如下：
1. 能够继承父类的public和protected成员变量；不能够继承父类的private成员变量；
2. 对于父类的包访问权限成员变量，如果子类和父类在同一个包下，则子类能够继承；否则，子类不能够继承；
3. 对于子类可以继承的父类成员变量，如果在子类中出现了同名称的成员变量，则会发生隐藏现象，即子类的成员变量会屏蔽掉父类的同名成员变量。如果要在子类中访问父类中同名成员变量，需要使用super关键字来进行引用。



## 数据类

数据类是一种非常强大的类，它可以让你避免创建Java中的用于保存状态但又操作非常简单的POJO（*Plain Ordinary Java Object简单的Java对象*）的模版代码。它们通常只提供了用于访问它们属性的简单的getter和setter。定义一个新的数据类非常简单：
数据类用 **`data class`** 来定义：
``` kotlin
data class Forecast(val date: Date, val temperature: Float, val details: String)
``` 

编译器会自动根据主构造函数中声明的所有属性添加如下方法：
- equals(): 它可以比较两个对象的属性来确保他们是相同的。
- hashCode(): 我们可以得到一个hash值，也是从属性中计算出来的。
- toString(): 格式是 "User(name=john, age=42)"
- copy(): 你可以拷贝一个对象，可以根据你的需要去修改里面的属性。
- [componentN()函数](https://link.jianshu.com?t=http://kotlinlang.org/docs/reference/multideclarations.html) 对应按声明顺序出现的所有属性

定义数据类需要注意的地方：
- 主构造函数应该至少有一个参数。
- 数据类的变量属性只能是 `var` 或 `val` 的。
- 数据类不能是 abstract，open，sealed，或者 inner 。

复制数据类并修改某一属性值：
``` kotlin
val f1 = Forecast(Date(), 27.5f, "Shiny day")
val f2 = f1.copy(temperature = 30f)
``` 

映射对象的每一个属性到一个变量中，这个过程就是我们知道的**多声明**。
这就是为什么会有 componentN 函数被自动创建。使用上面的 Forecast 类举个例子：
``` kotlin
val f1 = Forecast(Date(), 27.5f, "Shiny day")
val (date, temperature, details) = f1
``` 

上面这个多声明会被编译成下面的代码：
``` kotlin
val date = f1.component1()
val temperature = f1.component2()
val details = f1.component3()
``` 

这个特性背后的逻辑是非常强大的，它可以在很多情况下帮助我们简化代码。个例子， Map 类含有一些扩展函数的实现，允许它在迭代时使用key和value：
``` kotlin
for ((key, value) in map) {
    Log.d("map", "key:$key, value:$value")
}
``` 

## 接口定义
Kotlin 的接口很像 java 8。它们都可以包含抽象方法，以及方法的实现。
和抽象类不同的是，**接口不能保存状态**。可以有属性但必须是抽象的，或者提供访问器的实现。
接口用关键字 **`interface`** 来定义：
``` kotlin
interface Bar {
    fun bar()
    fun foo() {
        //函数体是可选的
    }
}
``` 

## 冒号

在冒号区分类型和父类型中要有空格，在实例和类型之间是没有空格的：
``` kotlin
interface Foo<out T : Any> : Bar {
    fun foo(a: Int): T
}
``` 

## 可见性

在 kotlin 中，默认修饰符为 `public`。

| 修饰符       | 说明            |
| --------- | ------------- |
| private   | 当前类可见         |
| protected | 成员自己和继承它的成员可见 |
| internal  | 当前 module 可见  |
| public    | 所有地方可见        |

## 扩展函数

扩展函数数是指**在一个类上增加一种新的行为**，甚至我们没有这个类代码的访问权限。这是一个在缺少有用函的类上扩展的方法。在Java中，通常会实现很多带有static方法的工具类。Kotlin中扩展函数的一个优势是我们不需要在调用方法的时候把整个对象当作参数传入。扩展函数表现得就像是属于这个类的一样，而且我们可以使用 this 关键字和调用所有public方法。

举个例子，我们可以创建一个toast函数，这个函数不需要传入任何context，它可以被任何Context或者它的子类调用，比如Activity或者Service：
``` kotlin
fun Context.toast(message: CharSequence, duration: Int = Toast.LENGTH_SHORT) {
    Toast.makeText(this, message, duration).show()
}
``` 

这个方法可以在Activity内部直接调用：
``` kotlin
toast("Hello world!")
toast("Hello world!", Toast.LENGTH_LONG)
``` 

扩展函数**也可以是一个属性**。所以我们可以通过相似的方法来扩展属性。
下面的例子展示了使用他自己的getter/setter生成一个属性的方式。Kotlin由于互操作性的特性已经提供了这个属性，但理解扩展属性背后的思想是一个很不错的练习：

``` kotlin
public var TextView.text: CharSequence
    get() = getText()
    set(v) = setText(v)
``` 

扩展函数并不是真正地修改了原来的类，它是以静态导入的方式来实现的。**扩展函数可以被声明在任何文件中**，因此有个通用的实践是把一系列有关的函数放在一个新建的文件里。

## Anko
Anko是JetBrains开发的一个强大的库。它主要的目的是用来替代以前XML的方式来使用代码生成UI布局。Anko包含了很多的非常有帮助的函数和属性来避免让你写很多的模版代码。通过查看Anko源码学习kotlin语言是一种不错的方法。Anko能帮助我们**简化代码**，比如，**实例化Intent，Activity之间的跳转，Fragment的创建，数据库的访问，Alert的创建等**等。

github地址：[https://github.com/Kotlin/anko](https://link.jianshu.com?t=https://github.com/Kotlin/anko)

### 添加Anko的依赖：
``` kotlin
// 主工程目录下build.gradle文件中声明版本
buildscript {
    ext.anko_version = '0.10.0'
}

// module的下build.gradle文件中添加依赖
dependencies {
    compile "org.jetbrains.anko:anko:$anko_version"
}
``` 

### 执行Activity的跳转：
``` kotlin
startActivity<MainActivity>()
//传递Intent参数
startActivity<NewHomeActivity>("name1" to "value1","name2" to "value2")
``` 

### 在Activity中显示Toast：
``` kotlin
toast("Hello world!")
longToast(R.id.hello_world)
``` 

### 线程切换：
``` kotlin
async {
    val response = URL("http://yuweiguocn.github.io").readText()
    uiThread {
        textView.text = response
    }
}
``` 
## 对象表达式和对象声明

有时，需要修改一个类的部分功能，可以不通过显式实现一个该类的子类方式来实现。
在Java中，通过匿名内部类来实现；在Kotlin中，概括为对象表达式和对象声明（*object expressions* and *object declarations*）。 

### 对象表达式（Object expressions）
 创建继承一个或多个类型的**匿名类**：
```kotlin
 window.addMouseListener(object : MouseAdapter() {  
   override fun mouseClicked(e: MouseEvent) {  
 ​    // ...  
   }  
   override fun mouseEntered(e: MouseEvent) {  
 ​    // ...  
   }  
 })  
```

如果父类型有构造函数，则必须将构造函数的参数赋值；多个父类通过“,”分割：
```kotlin
 open class A(x: Int) {  
   public open val y: Int = x  
 }  
   
 interface B {...}  
   
 val ab: A = object : A(1), B {  
   override val y = 15  
 }  
```

有时，只需要一个对象表达式，不想继承任何的父类型，实现如下：
```kotlin
 val adHoc = object {  
   var x: Int = 0  
   var y: Int = 0  
 }  
 print(adHoc.x + adHoc.y)  
```

类似于Java的匿名内部类，对象表达式也可以访问闭合范围内局部变量（跟Java不同，变量不用声明为 final）：
```kotlin
 fun countClicks(window: JComponent) {  
   var clickCount = 0  
   var enterCount = 0  
   
   window.addMouseListener(object : MouseAdapter() {  
 ​    override fun mouseClicked(e: MouseEvent) {  
 ​      clickCount++  
 ​    }  
 ​    override fun mouseEntered(e: MouseEvent) {  
 ​      enterCount++  
 ​    }  
   })  
   // ...  
 }  
```

### 对象声明（Object declarations）
单例（Singleton）是一个非常有用的设计模式，在Kotlin中，可以通过下面方式很容易去实现：
```kotlin
 object DataProviderManager {  
   fun registerDataProvider(provider: DataProvider) {  
 ​    // ...  
   }  
   val allDataProviders: Collection<DataProvider>  
 ​    get() = // ...  
 }  
```

这种方式称为对象声明（object declaration），通过在**`object`**关键字后面跟上定义的名称即可；它也不再称为一个表达式。不能把它赋值给一个变量，可以通过它的名字来指向它。另外，也可以继承父类：
```kotlin
 object DefaultListener : MouseAdapter() {  
   override fun mouseClicked(e: MouseEvent) {  
 ​    // ...  
   }  
   override fun mouseEntered(e: MouseEvent) {  
 ​    // ...  
   }  
 }  
```

注：对象声明（object declaration）**不能**够**定义为局部**的（如嵌套在一个函数中），但**可**以**嵌套到**其他的**对象声明**（object declaration）或**非内部类中**。

 

#### 伴随对象（Companion Objects）

使用**`companion`**关键字修饰，定义在一个类中的对象声明。
```kotlin
 class MyClass {  
   companion object Factory {  
 ​    fun create(): MyClass = MyClass()  
   }  
 }  
```

伴随对象的成员，可以**通过外部类的类名直接访问**：
Companion Objects中定义的成员类似于Java中的静态成员，因为Kotlin中没有static成员
```kotlin
 val instance = MyClass.create()  
```

伴随对象的名称，也可以省略，通过`Companion`关键字访问该伴随对象：
```kotlin
 class MyClass {  
   companion object {  
   }  
 }  

 val x = MyClass.Companion  
```
尽管伴随对象的成员看起来像其他语言（如Java）的static成员类型；
但是在运行时，它是真正的对象的成员实例；还可以实现接口：
```kotlin
 interface Factory<T> {  
   fun create(): T  
 }  
   
 class MyClass {  
   companion object : Factory<MyClass> {  
 ​    override fun create(): MyClass = MyClass()  
   }  
 }  
```

​         注：在Java虚拟机（JVM ）中，可以将伴随对象的成员使用“@JvmStatic”注解，就可以当做一个真正的静态变量或方法。

### 对象表达式和对象声明在语义上的区别
- 对象表达式，在它们使用的地方，是立即（**immediately**）执行（或初始化）。

- 对象声明，会延迟初始化（**lazily**懒加载）；但第一次访问该对象声明时才执行。

- 伴随对象（Companion Objects）在对应的类加载时初始化的，和 Java 的静态初始是对应的。

注意：**在kotlin中没有 new 关键字**。

## Lambda表达式

Lambda表达式是一种很简单的方法，去定义一个匿名函数。Lambda是非常有用的，因为它们避免我们去写一些包含了某些函数的抽象类或者接口，然后在类中去实现它们。在Kotlin，我们把一个函数作为另一个函数的参数。

我们用Android中非常典型的例子去解释它是怎么工作的： View.setOnClickListener() 方法。

``` java
//使用Java
//首先要编写一个 OnClickListener 接口：
public interface OnClickListener {
    void onClick(View v);
}


//然后我们要编写一个匿名内部类去实现这个接口：
view.setOnClickListener(new OnClickListener(){
    @Override
    public void onClick(View v) {
        Toast.makeText(v.getContext(), "Click", Toast.LENGTH_SHORT).show();
    }
});
``` 

``` kotlin
//使用Kotlin
view.setOnClickListener(object : OnClickListener {
    override fun onClick(v: View) {
        toast("Click")//使用了Anko的toast函数
    }
}
``` 

Kotlin允许Java库的一些优化，Interface中包含单个函数可以被替代为一个函数。如果我们这么去定义了，它会正常执行：

``` kotlin
fun setOnClickListener(listener: (View) -> Unit)
``` 

一个lambda表达式通过**参数**的形式被定义在**箭头**的**左边**（普通圆括号包围），然后在**箭头**的**右边**返回**结果**值。
当我们定义了一个**方法**，我们必须使用**大括号包围**。
如果左边的**参数没**有**用到**，我们甚至**可**以**省略左边**的参数。

``` kotlin
view.setOnClickListener({ view -> toast("Click")})
view.setOnClickListener({ toast("Click") })
``` 

如果这个函数**只接收一个参数**，我们可以**使用it引用**，而不用去指定左边的参数：

``` kotlin
view.setOnClickListener({ toast("Click" + it.id)})
``` 

如果这个函数的最后一个参数是一个函数，我们可以把这个函数移动到圆括号外面：

``` kotlin
view.setOnClickListener() { toast("Click") }
```

并且，最后，如果这个函数只有一个参数，我们可以省略这个圆括号：

``` kotlin
view.setOnClickListener { toast("Click") }
```

## When表达式

when 表达式与Java中的 **switch/case 类似**，但是要强大得多。
这个表达式会去试图匹配所有可能的分支直到找到满意的一项。然后它会运行右边的表达式。
与Java的 switch/case **不同之处**是**参数**可以是**任何类型**，并且分支也可以是一个条件。

对于默认的选项，我们可以增加一个 else 分支，它会在前面没有任何条件匹配时再执行。条件匹配成功后执行的代码也可以是代码块：
``` kotlin
when (x){
    1 -> print("x == 1")
    2 -> print("x == 2")
    else -> {
        print("I'm a block")
        print("x is neither 1 nor 2")
    }
}
```

因为它是一个表达式，它也可以返回一个值。我们需要考虑什么时候作为一个表达式使用，它必须要覆盖所有分支的可能性或者实现 else 分支。否则它不会被编译成功：

``` kotlin
val result = when (x) {
    0, 1 -> "binary"
    else -> "error"
}
```

## with函数

with是一个非常有用的函数，包含在Kotlin的标准库中。
它**接收一个对象**和**一个扩展函数**作为它的参数，然后**使这个对象扩展这个函数**。这表示所有我们在括号中编写的代码都是作为对象（第一个参数）的一个扩展函数，我们可以就像作为this一样使用所有它的public方法和属性。
当我们针对同一个对象做很多操作的时候这个非常有利于简化代码。

``` kotlin
data class Person(val name: String, val age: Int)

val p = Person("growth",25)
with(p){
    var info = “$name - $age” 
}

```

## 内联函数

下面是with函数的定义：

``` kotlin
inline fun <T> with(t: T, body: T.() -> Unit) { t.body() }
```

这个函数接收一个 T 类型的对象和一个被作为扩展函数的函数。它的实现仅仅是让这个对象去执行这个函数。因为第二个参数是一个函数，所以我们可以把它放在圆括号外面，所以我们可以创建一个代码块，在这这个代码块中我们可以使用 this 和直接访问所有的public的方法和属性。

内联函数与普通的函数有点不同。一个内联函数会在编译的时候被替换掉，而不是真正的方法调用。这在一些情况下可以减少内存分配和运行时开销。
举个例子，如果我们有一个函数，只接收一个函数作为它的参数。如果是一个普通的函数，内部会创建一个含有那个函数的对象。另一方面，内联函数会把我们调用这个函数的地方替换掉，所以它不需要为此生成一个内部的对象。

``` kotlin
inline fun supportsLollipop(code: () -> Unit) {
    if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.LOLLIPOP) {
        code()
    }
}
```

它只是检查版本，然后如果满足条件则去执行。现在我们可以这么做：
``` kotlin
supportsLollipop {
    window.setStatusBarColor(Color.BLACK)
}
```

## Kotlin Android Extensions

Kotlin Android Extensions是另一个kotlin团队研发的可以让开发更简单的插件。该插件依赖于 kotlin 标准库，当前仅仅包括了view的绑定，这可以让我们省去findViewById操作。

使用该插件非常简单，修改module的build.gradle文件：

``` kotlin
apply plugin: 'com.android.application'
apply plugin: 'kotlin-android'
apply plugin: 'kotlin-android-extensions'
apply plugin: 'kotlin-kapt'
```

例如在布局文件中定义一了个id为tvTest的TextView，在Activity的setContentView之后就可以直接使用该TextView了：

![](http://upload-images.jianshu.io/upload_images/9028834-1bfe740113b2f207.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

``` kotlin
class MainActivity : AppCompatActivity(){
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        tvTest.text = "hello world"
    }
}
``` 


引用：
[Kotlin学习笔记（一）](https://www.jianshu.com/p/37a8fcdf6a73)
[Kotlin - 继承](http://blog.csdn.net/io_field/article/details/52856659)
[Kotlin语法（十五）-对象表达式和声明](http://blog.csdn.net/tangxl2008008/article/details/53045454)
