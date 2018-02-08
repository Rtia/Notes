<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [【JAVA 工具类】](#java-%E5%B7%A5%E5%85%B7%E7%B1%BB)
- [System](#system)
- [Runtime](#runtime)
- [Date](#date)
  - [日期和时间模式](#%E6%97%A5%E6%9C%9F%E5%92%8C%E6%97%B6%E9%97%B4%E6%A8%A1%E5%BC%8F)
- [Calendar](#calendar)
  - [DAY_OF_WEEK](#day_of_week)
  - [MONTH](#month)
  - [Calendar.set](#calendarset)
  - [习题: 获取任意年的二月有多少天](#%E4%B9%A0%E9%A2%98-%E8%8E%B7%E5%8F%96%E4%BB%BB%E6%84%8F%E5%B9%B4%E7%9A%84%E4%BA%8C%E6%9C%88%E6%9C%89%E5%A4%9A%E5%B0%91%E5%A4%A9)
- [Math](#math)
  - [random](#random)
  - [习题：给定一个小数，保留该小数的后指定位数](#%E4%B9%A0%E9%A2%98%E7%BB%99%E5%AE%9A%E4%B8%80%E4%B8%AA%E5%B0%8F%E6%95%B0%E4%BF%9D%E7%95%99%E8%AF%A5%E5%B0%8F%E6%95%B0%E7%9A%84%E5%90%8E%E6%8C%87%E5%AE%9A%E4%BD%8D%E6%95%B0)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->



# 【JAVA 工具类】


# System

System:类中的**方法**和**属性**都是**静态**的。

**out**:标准**输出**，默认是控制台(屏幕)。

**in**：标准**输入**，默认是键盘。

获取**系统属性**信息：**Properties getProperties**();

```java
import java.util.*;
class SystemDemo 
{
	public static void main(String[] args) 
	{
		Properties prop = System.getProperties();
		//因为Properties是Hashtable的子类，也就是Map集合的一个子类对象。那么可以通过Map的方法取出该集合中的元素。获取所有属性信息。
		for(Object obj : prop.keySet())//Map方法keySet
				//可用Properties方法stringPropertyNames代替keySet
		{
			String value = (String)prop.get(obj);
				//该集合中存储都是字符串。没有泛型定义。
			System.out.println(obj+"对应"+value);//①
		}
		System.out.println(prop);//②
		//在系统中自定义特有信息
		System.setProperty("mykey","myvalue");
		//获取指定属性信息。
		String value = System.getProperty("os.name");
		System.out.println("value="+value);
		//在jvm启动时，动态加载属性信息：					DOS命令：java -Dhaha=sth SystemDemo
		String v = System.getProperty("haha");
		System.out.println("v="+v);//③
	}
}
```

运行结果

①
![1](http://img.blog.csdn.net/20180115164933539?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

②
![2](http://img.blog.csdn.net/20180115164942670?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

③
![3](http://img.blog.csdn.net/20180115164952016?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

# Runtime

该类并**没有提供构造函数**，说明**不可以new对象**。那么会直接想到该类中的方法都是**静态**的。

发现该类中**还有非静态方法**。说明该类肯定会**提供了方法获取本类对象**。而且该方法是静态的，并返回值类型是本类类型。

由这个特点可以看出该类使用了**单例设计模式**完成。

该方式是static Runtime getRuntime();

```java
class  RuntimeDemo
{
	public static void main(String[] args) throws Exception
				//exec 应抛IOException，不想导入IO故抛其父类Exception
	{
		Runtime r = Runtime.getRuntime();
		//r.exec("E:\\Program Files (x86)\\SogouExplorer\\SogouExplorer.exe");
		//路径’\’换成’\\’，因\要转义字符，不写路径则在环境变量路径里面找程序
		Process p = r.exec("notepad.exe  SystemDemo.java");
		//通过记事本程序打开SystemDemo.java文件
		Thread.sleep(4000); //等4s
		p.destroy();//杀掉子进程
	}
}
```


# Date

```java
import java.util.*;
import java.text.*;
class DateDemo 
{
	public static void main(String[] args) 
	{
		Date d = new Date();
		System.out.println(d);// Sun Aug 30 17:02:27 CST 2015
		//打印的时间看不懂，希望有些格式。
		//将模式封装到SimpleDateformat对象中。
		SimpleDateFormat sdf = new SimpleDateFormat("yy年M月d日 E 全年第D天 ah:m:s:S z");//15年8月30日 星期日 全年第242天 下午5:13:48:42 CST
		//调用format方法让模式格式化指定Date对象。
		String time = sdf.format(d);
		System.out.println("time="+time);
	}
}
```
## 日期和时间模式
| **字母** | **日期或时间元素**       | **表示**                                   | **示例**                                |
| ------ | ----------------- | ---------------------------------------- | ------------------------------------- |
| G      | Era 标志符           | [Text](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/text/SimpleDateFormat.html#text) | AD                                    |
| y      | 年                 | [Year](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/text/SimpleDateFormat.html#year) | 1996; 96                              |
| M      | 年中的月份             | [Month](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/text/SimpleDateFormat.html#month) | July; Jul; 07                         |
| w      | 年中的周数             | [Number](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/text/SimpleDateFormat.html#number) | 27                                    |
| W      | 月份中的周数            | [Number](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/text/SimpleDateFormat.html#number) | 2                                     |
| D      | 年中的天数             | [Number](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/text/SimpleDateFormat.html#number) | 189                                   |
| d      | 月份中的天数            | [Number](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/text/SimpleDateFormat.html#number) | 10                                    |
| F      | 月份中的星期            | [Number](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/text/SimpleDateFormat.html#number) | 2                                     |
| E      | 星期中的天数            | [Text](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/text/SimpleDateFormat.html#text) | Tuesday; Tue                          |
| a      | Am/pm 标记          | [Text](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/text/SimpleDateFormat.html#text) | PM                                    |
| H      | 一天中的小时数（0-23）     | [Number](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/text/SimpleDateFormat.html#number) | 0                                     |
| k      | 一天中的小时数（1-24）     | [Number](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/text/SimpleDateFormat.html#number) | 24                                    |
| K      | am/pm 中的小时数（0-11） | [Number](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/text/SimpleDateFormat.html#number) | 0                                     |
| h      | am/pm 中的小时数（1-12） | [Number](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/text/SimpleDateFormat.html#number) | 12                                    |
| m      | 小时中的分钟数           | [Number](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/text/SimpleDateFormat.html#number) | 30                                    |
| s      | 分钟中的秒数            | [Number](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/text/SimpleDateFormat.html#number) | 55                                    |
| S      | 毫秒数               | [Number](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/text/SimpleDateFormat.html#number) | 978                                   |
| z      | 时区                | [General time zone](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/text/SimpleDateFormat.html#timezone) | Pacific Standard Time; PST; GMT-08:00 |
| Z      | 时区                | [RFC 822 time zone](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/text/SimpleDateFormat.html#rfc822timezone) | \-0800                                |


# Calendar
## DAY_OF_WEEK
注意：Calendar.**DAY_OF_WEEK**返回的星期几，**要减1**才是我们通常意义上的星期几（返回的星期天对应数值1，星期一对应2，以此类推）
>int java.util.Calendar.**DAY_OF_WEEK**
>Field number for get and set indicating the day of the week. This field takes values SUNDAY, MONDAY, TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, and SATURDAY.

>int java.util.Calendar.**SUNDAY : 1** [0x1]
>Value of the DAY_OF_WEEK field indicating Sunday.

>int java.util.Calendar.**MONDAY : 2** [0x2]
>Value of the DAY_OF_WEEK field indicating Monday.

## MONTH
注意：Calendar.**MONTH**返回的月份，**要加1**才是我们通常意义上的月份（返回的一月对应数值0，二月对应1，以此类推）
>int java.util.Calendar.**MONTH**
>Field number for get and set indicating the month. This is a calendar-specific value. The first month of the year in the Gregorian and Julian calendars is **JANUARY which is 0**; the last depends on the number of months in a year.


```java
import java.util.*;
import java.text.*;
import static java.util.Calendar.*;//静态导入，下面的Calendar.都可去掉了

class CalendarDemo {
	public static void main(String[] args) {
		Calendar c = Calendar.getInstance();
		System.out.println(c.get(Calendar.YEAR) + "年");//->2018年
		System.out.println((c.get(Calendar.MONTH) + 1) + "月");//->1月
		System.out.println(c.get(Calendar.DAY_OF_MONTH) + "日");//->15日
		System.out.println("星期" + (c.get(Calendar.DAY_OF_WEEK) - 1));//->星期1
		//查表法
		String[] mons = { "一月", "二月", "三月", "四月", "五月", "六月", "七月", "八月", "九月", "十月", "十一月", "十二月" };
		String[] weeks = { "", "星期日", "星期一", "星期二", "星期三", "星期四", "星期五", "星期六" };
		int index = c.get(MONTH);//因为有静态导入，可以不用写Calendar.MONTH
		int index1 = c.get(DAY_OF_WEEK);
		System.out.println(mons[index]);//->一月
		System.out.println(weeks[index1]);//->星期一
	}
}
```
## Calendar.set
>void java.util.**Calendar.set**(int year, int month, int date)
>Sets the values for the calendar fields YEAR, MONTH, and DAY_OF_MONTH. Previous values of other calendar fields are retained. If this is not desired, call clear() first.
>Parameters:
>　year：the value used to set the YEAR calendar field.
>　**month**：the value used to set the MONTH calendar field. **Month value is 0-based. e.g., 0 for January.**
>　date：the value used to set the DAY_OF_MONTH calendar field.
```java
import java.util.*;

class CalendarDemo2 {
	public static void main(String[] args) {
		Calendar c = Calendar.getInstance();
		c.set(2015, 5, 26);
		printCalendar(c); //->2015年六月26日星期五
		c.add(Calendar.MONTH, -1);
		printCalendar(c);//->2015年五月26日星期二
	}

	public static void printCalendar(Calendar c) {
		String[] mons = { "一月", "二月", "三月", "四月", "五月", "六月", "七月", "八月", "九月", "十月", "十一月", "十二月" };
		String[] weeks = { "星期日", "星期一", "星期二", "星期三", "星期四", "星期五", "星期六" };
		int index = c.get(Calendar.MONTH);
		int index1 = c.get(Calendar.DAY_OF_WEEK) - 1;
		System.out.print(c.get(Calendar.YEAR) + "年");
		System.out.print(mons[index]);
		System.out.print(c.get(Calendar.DAY_OF_MONTH) + "日");
		System.out.print(weeks[index1]);
		System.out.println();
	}
}
```
## Calendar.add VS .roll
```java
class CalendarDemo3 {
	public static void main(String[] args) {
		Calendar c = Calendar.getInstance();
		c.set(2000, 0, 1);
		printCalendar(c);//->2000年1月1日星期6
		c.add(Calendar.MONTH, 15);
		printCalendar(c); //->2001年4月1日星期0（通常意义上的星期天）

		c.set(2000, 0, 1);
		c.roll(Calendar.MONTH, 15);
		printCalendar(c); //->2000年4月1日星期6
		
		c.set(2000, 0, 0);
		printCalendar(c);//->1999年12月31日星期5
	}

	public static void printCalendar(Calendar c) {
		System.out.println(c.get(Calendar.YEAR) + "年" 
				+ (c.get(Calendar.MONTH) + 1) + "月" 
				+ c.get(Calendar.DAY_OF_MONTH)+ "日" 
				+ "星期" + (c.get(Calendar.DAY_OF_WEEK) - 1));
	}
}
```

## 习题: 获取任意年的二月有多少天

**思路：**
- 方式一：
  根据指定年设置一个时间：c.set(year,2,1)//某一年的3月1日。
  c.add(Calendar.DAY_OF_MONTH,-1);//3月1日，往前推一天，就是2月最后一天。
- 方式二：
  根据指定年设置一个时间：c.set(year,2,0)//某一年的3月1日的前一天，也就是2月最后一天。

```java
class CalendarTest {
	public static void main(String[] args) {
		febDays(2016);
		febDays2(2016);
	}

	public static void febDays(int y) {
		Calendar c = Calendar.getInstance();
		c.set(y, 2, 1);
		c.add(Calendar.DAY_OF_MONTH, -1);
		int d = c.get(Calendar.DAY_OF_MONTH);
		System.out.println(y + "年2月有" + d + "天");
	}

	public static void febDays2(int y) {
		Calendar c = Calendar.getInstance();
		c.set(y, 2, 0);
		int d = c.get(Calendar.DAY_OF_MONTH);
		System.out.println(y + "年2月有" + d + "天");
	}
}
```

# Math

```java
import java.util.*;
class  MathDemo
{
	public static void main(String[] args) 
	{
		double d = Math.ceil(16.34);//ceil返回大于指定数据的最小整数。
		double d1 = Math.floor(12.34);//floor返回小于指定数据的最大整数。
		long l = Math.round(12.54);//四舍五入
		sop("d="+d);// d=17.0
		sop("d1="+d1);// d1=12.0
		sop("l="+l);// l=13
		double d2 = Math.pow(2,3);//返回第一个参数的第二个参数次幂的值
		sop("d2="+d2);// d2=8.0
		Random r=new Random();
		for (int x=0; x<10; x++)
		{
			int d=(int)(Math.random()*10+1);//[1,10]随机数
			//int d=r.nextInt(10)+1;//[1,10]随机数
			sop(d);
		}
	}
	public static void sop(Object obj)
	{
		System.out.println(obj);
	}
}
```

## random

public static double **random**()
**返回： 大于等于 0.0 且小于 1.0** 的伪随机 double 值。
>double java.lang.Math.random()
>Returns a double value with a positive sign, greater than or equal to 0.0 and less than 1.0. Returned values are chosen pseudorandomly with (approximately) uniform distribution from that range. 
>When this method is first called, it creates a single new pseudorandom-number generator, exactly as if by the expression `new java.util.Random()`
>This new pseudorandom-number generator is used thereafter for all calls to this method and is used nowhere else. 
>This method is properly synchronized to allow correct use by more than one thread. However, if many threads need to generate pseudorandom numbers at a great rate, it may reduce contention for each thread to have its own pseudorandom-number generator.
>Returns:a pseudorandom double greater than or equal to 0.0 and less than 1.0.
>**See Also:Random.nextDouble()**


>**nextInt**(int n) //**Random类方法**
>**返回：**一个**伪随机**数，它是取自此随机数生成器序列的、**在0（包括）和指定值（不包括）之间**均匀分布的 int 值。


## 习题：给定一个小数，保留该小数的后指定位数
```java
import java.util.*;
public class MathTest {
	public static void main(String[] args) {
		saveAs(12.3456, 3, true);//12.346
		saveAs(12.3456, 3, false);//12.345
	}

	//给定一个小数d, 保留小数点后scale位, 是否四舍五入
	public static void saveAs(double d, int scale, boolean isRound) {
		double base = Math.pow(10, scale);
		double num = isRound ?
				Math.round(d * base) / base 
				: ((int) (d * base)) / base;
		System.out.println("num=" + num);
	}
}
```




