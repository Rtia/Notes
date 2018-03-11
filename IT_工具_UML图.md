<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [【IT 工具 UML图】](#it-%E5%B7%A5%E5%85%B7-uml%E5%9B%BE)
  - [为什么需要类图？类图的作用](#%E4%B8%BA%E4%BB%80%E4%B9%88%E9%9C%80%E8%A6%81%E7%B1%BB%E5%9B%BE%E7%B1%BB%E5%9B%BE%E7%9A%84%E4%BD%9C%E7%94%A8)
  - [怎么画类图？用什么工具？](#%E6%80%8E%E4%B9%88%E7%94%BB%E7%B1%BB%E5%9B%BE%E7%94%A8%E4%BB%80%E4%B9%88%E5%B7%A5%E5%85%B7)
    - [1 类（Class)](#1-%E7%B1%BBclass)
    - [2 接口（Interface）](#2-%E6%8E%A5%E5%8F%A3interface)
    - [3 类图中关系（relation）](#3-%E7%B1%BB%E5%9B%BE%E4%B8%AD%E5%85%B3%E7%B3%BBrelation)
      - [1\. 泛化（Generalization）](#1%5C-%E6%B3%9B%E5%8C%96generalization)
      - [2\. 实现（Realization）](#2%5C-%E5%AE%9E%E7%8E%B0realization)
      - [3\. 关联（Association)](#3%5C-%E5%85%B3%E8%81%94association)
      - [4\. 聚合（Aggregation）](#4%5C-%E8%81%9A%E5%90%88aggregation)
      - [5\. 组合(Composition)](#5%5C-%E7%BB%84%E5%90%88composition)
      - [6\. 依赖(Dependency)](#6%5C-%E4%BE%9D%E8%B5%96dependency)
      - [各种关系的强弱顺序：](#%E5%90%84%E7%A7%8D%E5%85%B3%E7%B3%BB%E7%9A%84%E5%BC%BA%E5%BC%B1%E9%A1%BA%E5%BA%8F)
    - [类图绘制的要点](#%E7%B1%BB%E5%9B%BE%E7%BB%98%E5%88%B6%E7%9A%84%E8%A6%81%E7%82%B9)
  - [类图的分类](#%E7%B1%BB%E5%9B%BE%E7%9A%84%E5%88%86%E7%B1%BB)
    - [领域UML类图vs实现UML类图](#%E9%A2%86%E5%9F%9Fuml%E7%B1%BB%E5%9B%BEvs%E5%AE%9E%E7%8E%B0uml%E7%B1%BB%E5%9B%BE)
      - [领域UML类图](#%E9%A2%86%E5%9F%9Fuml%E7%B1%BB%E5%9B%BE)
      - [实现UML类图](#%E5%AE%9E%E7%8E%B0uml%E7%B1%BB%E5%9B%BE)
      - [总结](#%E6%80%BB%E7%BB%93)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

# 【IT 工具 UML图】

产品经理的必备技能之一是画UML图，本文就告诉你怎么画标准的类图吧。本文结合网络资料和个人心得所成，不当之处，请多指教。

## 为什么需要类图？类图的作用

我们做项目的需求分析，最开始往往得到的是一堆文字，请看下面这堆文字：

本项目是在一期的基础上增加对电缆、通讯工程的管理和施工详细数据的记录和统计，使整个系统更好的管理各工程项目从中标开始到竣工验收的全部过程和资料和分析施工过程的数据。

本系统将一条或一个标段的架空电力线路工程定为一个单位工程，即系统中的一个工程项目；每个单位工程分为若干个分部工程；每个分部工程分为若干个分项工程；每个分项工程中又分为若干相同单元工程。

这是关于系统情况的一段概述，里面充斥了大量的术语、概念，如果你不是专业人士，恐怕难以读懂上述文字。

项目初期，我们往往对业务一无所知，我们最急迫需要解决的问题就是理清楚这些业务概念以及它们的关系，如果能用好类图，你将能深入地剖析系统业务。

用下面这个**UML图来描述**是否**清晰**了许多呢？

![](http://upload-images.jianshu.io/upload_images/9028834-67db3f0dcb02b665..png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在上图中，各个类之间是关联关系，也就是拥有的关系。

类图(Class diagram)主要用于描述系统的结构化设计。类图也是最常用的UML图，用类图可以显示出类、接口以及它们之间的静态结构和关系。

## 怎么画类图？用什么工具？

使用工具：**Visio**或者**processon**在线作图

　在类图中一共包含了以下几种模型元素，分别是：**类**（Class）、**接口**（Interface）以及**类之间的关系**。

### 1 类（Class)

　　在面向对象（OO) 编程中，类是对现实世界中一组具有相同特征的物体的抽象。

![](http://upload-images.jianshu.io/upload_images/9028834-30cde076c5f7e8bd..png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 2 接口（Interface）

　　接口是一种特殊的类，具有类的结构但不可被实例化，只可以被实现（继承）。在UML中，接口使用一个带有名称的小圆圈来进行表示。

![](http://upload-images.jianshu.io/upload_images/9028834-5a529bfcb7b0cdc6..png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 3 类图中关系（relation）

在UML类图中，常见的有以下几种关系: 泛化（Generalization）, 实现（Realization），关联（Association)，聚合（Aggregation），组合(Composition)，依赖(Dependency)

#### 1\. 泛化（Generalization）

【泛化关系】：是一种**继承**关系，表示一般与特殊的关系，它指定了**子类如何特化父类**的所有特征和行为。
　　例如：老虎是动物的一种，即有老虎的特性也有动物的共性。

【箭头指向】：带三角箭头的实线，箭头指向父类

![](http://upload-images.jianshu.io/upload_images/9028834-78b94ac9c07281db..png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 2\. 实现（Realization）

【实现关系】：是一种**类与接口**的关系，表示**类是接口所有特征和行为的实现**.

【箭头指向】：带三角箭头的虚线，箭头指向接口

![](http://upload-images.jianshu.io/upload_images/9028834-c43534d92c3d05ff..png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 3\. 关联（Association)

【关联关系】：是一种**拥有**的关系，它使**一个类知道另一个类的属性和方法**；
　　如：老师与学生，丈夫与妻子关联可以是双向的，也可以是单向的。
**双向**的关联可以有**两个箭头**或者**没有箭头**，**单向**的关联有**一个箭头**。

【代码体现】：成员变量

【箭头及指向】：带普通箭头的实心线，指向被拥有者

![](http://upload-images.jianshu.io/upload_images/9028834-3f16198bf379513d..png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
上图中，老师与学生是双向关联，老师有多名学生，学生也可能有多名老师。
但学生与某课程间的关系为单向关联，一名学生可能要上多门课程，课程是个抽象的东西他不拥有学生。

下图为自身关联:

![](http://upload-images.jianshu.io/upload_images/9028834-077f913a553bf5a2..png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 4\. 聚合（Aggregation）

【聚合关系】：是**整体与部分**的关系，且**部分可以离开整体**而单独存在。
　　如：车和轮胎是整体和部分的关系，轮胎离开车仍然可以存在。
聚合关系是关联关系的一种，是强的关联关系；关联和聚合在语法上无法区分，必须考察具体的逻辑关系。

【代码体现】：成员变量

【箭头及指向】：带空心菱形的实心线，菱形指向整体

![](http://upload-images.jianshu.io/upload_images/9028834-535faacf6536b1cf..png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 5\. 组合(Composition)

【组合关系】：是**整体与部分**的关系，但**部分不能离开整体**而单独存在。
　　如：公司和部门是整体和部分的关系，没有公司就不存在部门。
组合关系是关联关系的一种，是比聚合关系还要强的关系，它要求普通的聚合关系中代表整体的对象负责代表部分的对象的生命周期。

【代码体现】：成员变量

【箭头及指向】：带实心菱形的实线，菱形指向整体

![](http://upload-images.jianshu.io/upload_images/9028834-a0dd718b236381c6..png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 6\. 依赖(Dependency)

【依赖关系】：是一种**使用**的关系，即**一个类的实现需要另一个类的协助**，所以要尽量不使用双向的互相依赖.

【代码表现】：局部变量、方法的参数或者对静态方法的调用

【箭头及指向】：带箭头的虚线，指向被使用者

![](http://upload-images.jianshu.io/upload_images/9028834-44c1149acbbe82e7..png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 各种关系的强弱顺序：

泛化 = 实现 > 组合 > 聚合 > 关联 > 依赖

下面这张UML图，比较形象地展示了各种类图关系：

![](http://upload-images.jianshu.io/upload_images/9028834-e45d7b34d376cca4..png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 类图绘制的要点

1. **类的操作是针对类自身的操作**，而不是它去操作人家。
  比如书这个类有上架下架的操作，是书自己被上架下架，不能因为上架下架是管理员的动作而把它放在管理员的操作里。

2. **两个相关联的类，需要在关联的类中加上被关联类的ID**，并且箭头指向被关联类。可以理解为数据表中的外键。
  比如借书和书，借书需要用到书的信息，因此借书类需包含书的ID，箭头指向书。

3. 由于业务复杂性，一个显示中的实体可能会被分为多个类，这是很正常的，类不是越少越好。类的设计取决于怎样让后台程序的操作更加简单。
  比如单看逻辑，借书类可以不存在，它的信息可以放在书这个类里。然而借还书和书的上架下架完全不是一回事，借书类对借书的操作更加方便，不需要去重复改动书这个类中的内容。此外，如果书和借书是1对多的关系，那就必须分为两个类。

4. 类图中的规范问题，比如不同关系需要不同的箭头，可见性符号等。

## 类图的分类

软件在分析与设计两个阶段各自会绘制一套UML类图，而且是由分析师和设计师两个不同的角色绘制的。那么这两套UML类图有什么异同呢？下面将解释这个问题。

### 领域UML类图vs实现UML类图

上文提到，在软件分析与设计过程中，会由两种角色产生两套UML类图。
一般情况下，**分析师**绘制的UML类图叫做“**领域UML类图**”，而**设计师**绘制的UML类图叫做“**实现UML类图**”。
这里要声明，这两个名词是我的习惯性叫法，并不是大家都认同的通用叫法。
下面，我对这两种UML类图给出我的定义：

#### 领域UML类图
领域UML类图：产生于分析阶段，由系统分析师绘制，主要作用是描述业务实体的静态结构，包括业务实体、各个业务实体所具有的业务属性及业务操作、业务实体之间具有的关系。

虽然这个UML类图也叫“UML类图”，但是说实话，它和编程中的“类”实在是没啥关系，因为最后的系统中可能根本没有类和它们对应，而且很多最后系统中的类如控制类和界面类这套UML类图中也没有。也就是说这套图和具体技术无关，也不是画给程序员看的，它只是表达业务领域中的一个静态结构。下面给个例子：

![](http://upload-images.jianshu.io/upload_images/9028834-81ff841464bfc887..png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

这是一个选课系统的简单领域分析UML类图。可以看到，主要实体有教师、学生、课程和开课安排。每个实体标注了其在业务上具有的属性和方法。而且图中还标明了实体间的关系。

但是，最终系统中可能没有一个学生类和其对应。因为最终系统中有哪些类、各个类有什么属性、方法依赖于所选择的平台和架构。例如，如果使用了Struts2，则会存在很多Action类，而使用了ASP.NETMVC，则会有很多Controller类等，所以，领域UML类图只于业务有关，和具体实现及编码等计算机技术无关。

#### 实现UML类图

实现UML类图：产生于设计阶段，由系统设计师绘制，其作用是描述系统的架构结构、指导程序员编码。它包括系统中所有有必要指明的实体类、控制类、界面类及与具体平台有关的所有技术性信息。

就像上面的领域UML类图，如果你把它交给程序员编码，我想程序员会疯掉，因为它没有提供任何编码的依据。假如我们使用的是.NET平台分层架构，并使用ASP.NETMVC，则设计师应该在实现UML类图中绘制出所有的实体类、数据访问类、业务逻辑类和界面类，界面类又分为视图类、控制器类等等，还要表示出IoC和Aop等信息，并明确指出各个类的属性、方法，不能有遗漏，因为最终程序员实现程序的依据就是实现UML类图。

#### 总结

最后，我们总结一下要点：

1.软件分析与设计是编码前的两个阶段，其中分析仅与业务有关，而与技术无关。设计以分析为基础，主要与具体技术有关。

2.分析阶段由分析师绘制领域UML类图，设计阶段由设计师绘制实现UML类图。

3.领域UML类图表示系统的静态领域结构，其中的类不与最终程序中的类对应；设计UML类图表示系统的技术架构，是程序员的编码依据，其中的类与系统中的类对应。

4.领域UML类图中类的属性与操作仅关注与业务相关的部分，实现UML类图中的属性与操作要包括最终需要实现的全部方法与操作。

**引用：**
[详解UML图之类图](http://www.uml.org.cn/oobject/201610282.asp)

