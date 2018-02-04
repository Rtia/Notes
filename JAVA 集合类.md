

# 【JAVA 集合类]
## 为什么出现集合类？
• 面向对象语言对事物的体现都是以对象的形式，所以为了方便对多个对象的操作，就对对象进行存储，集合就是**存储对象最常用的一种方式**。

## 数组和集合类同是容器，有何不同？
| 数组                 | 集合类    |
| ------------------ | ------ |
| 可以存储基本数据类型，也可以存储对象 | 只能存储对象 |
| 长度固定               | 长度可变   |

## 集合类的特点
集合只用于存储对象
集合长度是可变的
集合可以存储不同类型的对象。

1，add方法的参数类型是Object。以便于接收任意类型对象。
2，集合中存储的都是对象的引用(地址) 
　　当我们把一个对象放入集合中后，系统会把所有集合元素都当成Object类的实例进行处理。从JDK1.5以后，这种状态得到了改进：可以使用泛型来限制集合里元素的类型，并让集合记住所有集合元素的类型（参见具体泛型的内容）。


## 集合体系
　　Java的集合类主要由两个接口派生而出：**Collection**和**Map**，Collection和Map是Java集合框架的根接口，这两个接口又包含了一些接口或实现类。
### Collection
**Set**和**List**接口是Collection接口派生的两个子接口，Queue是Java提供的队列实现，类似于List。
![](http://img.blog.csdn.net/20171222133113650?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
### Map
Map集合中保存**Key-value**对形式的元素，访问时只能根据每项元素的key来访问其value。
![](http://img.blog.csdn.net/20171222133135702?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### 集合类关系图
![](http://img.blog.csdn.net/20171222132653028?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
![](http://img.blog.csdn.net/20171222132626850?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### Set、List和Map可以看做集合的三大类。
对于Set、List和Map三种集合，最常用的实现类分别是HashSet、ArrayList和HashMap三个实现类。




## Collection

### List vs Set 
| 一级   | 二级   | 特点                                       | 线程同步                                     |
| ---- | ---- | ---------------------------------------- | ---------------------------------------- |
| List |      | 元素存取是**有序**的，元素可以**重复**。<br>访问集合中的元素：根据元素的**索引**来访问。<br>**取**出List集合中**元素**的**方式**：<br>　get(int index)：即通过角标获取元素（支持for循环）<br>　iterator()：通过迭代方法获取迭代器对象。 |                                          |
|      |      | ArrayList                                | 底层使用的是**数组**数据结构。<br>特点：**查询**速度很**快**。但是增删稍慢。(元素越多越明显) |
|      |      | LinkedList                               | 底层使用的**链表**数据结构。<br>特点：**增删**速度很**快**，查询稍慢 |
|      |      | Vector                                   | 底层是**数组**数据结构。<br>被ArrayList替代了。因为效率低。   |
| Set  |      | 元素存取是**无序**(存入和取出的顺序不一定一致)，元素**不**可以**重复**。<br>访问集合中的元素：只能根据**元素本身来访问**（也是集合里元素不允许重复的原因）。<br>**取**出List集合中**元素**的**方式**：<br>　iterator()：只可用迭代器 |                                          |
|      |      | HashSet                                  | 底层数据结构是**哈希表**。是线程不安全的<br>**元素唯一性**依据：<br>　通过元素的两个方法，**hashCode**和**equals**（Array是通过equals）<br>　如果元素的HashCode值相同，才会判断equals是否为true。<br>　如果元素的hashcode值不同，不会调用equals。<br>　对于判断元素是否存在，以及删除等操作，依赖的方法是元素的hashcode和equals方法，先hashcode再equals。 |
|      |      | TreeSet                                  | 底层数据结构是**二叉树**。是线程不安全的。<br>可以对Set集合中的元素进行**排序**。<br>**元素唯一性**依据：	**compareTo**方法return 0视为相同不存入 |

### TreeSet排序的方式：
**方式一**：让元素自身具备比较性。
　　元素需要**实现Comparable接口，覆盖compareTo方法**。
　　这种方式也称为元素的**自然顺序**，或者叫做默认顺序。

**方式二**：
　　当元素自身不具备比较性时，或者具备的比较性不是所需要的，
　　让集合自身具备比较性——定义**比较器**（**实现Comparator接口，覆盖compare方法**。），将比较器对象作为参数传递给TreeSet集合的构造函数。
　　
当两种排序都存在时，**以比较器为主**。

记住，排序时，当主要条件相同时，一定判断一下次要条件。

### List特有方法
凡是可以**操作角标**的方法都是该体系特有的方法。
**增**：add(index,element);
　　addAll(index,Collection);
**删**：remove(index);
**改**：set(index,element);
**查**：get(index):
　　subList(from,to);
　　listIterator();
　　int indexOf(obj):获取指定元素的位置。
　　ListIterator listIterator();
#### **LinkedList**:
**特有方法**：
void addFirst(Object e);
void addLast(Object e);

Object getFirst();
Object getLast();
　　获取元素，但不删除元素。如果集合中没有元素，会出现NoSuchElementException

Object removeFirst();
Object removeLast();
　　获取元素，但是元素被删除。如果集合中没有元素，会出现NoSuchElementException


在**JDK1.6**出现了**替代方法**：
boolean offerFirst(Object e);
boolean offerLast(Object e);
　　Inserts the specified element at the front of this list.

Object peekFirst();
Object peekLast();
　　获取元素，但不删除元素。如果集合中没有元素，会返回null。

Object pollFirst();
Object pollLast();
　　获取元素，但是元素被删除。如果集合中没有元素，会返回null。
　　
### 实例
#### Collection
```java
public class TestCollection {
    public static void main(String[] args) {
        Collection c = new ArrayList();//ArrayList

        c.add("孙悟空");//添加元素
        c.add(6);//虽然集合里不能放基本类型的值，但Java支持自动装箱
        System.out.println("c集合的元素个数为:" + c.size());

        c.remove(6);//删除指定元素
        System.out.println("c集合的元素个数为:" + c.size());
        System.out.println("c集合的是否包含孙悟空字符串:" + c.contains("孙悟空"));//判断是否包含指定字符串

        c.add("轻量级J2EE企业应用实战");
        System.out.println("c集合的元素：" + c);

        Collection books = new HashSet();//HashSet
        books.add("轻量级J2EE企业应用实战");
        books.add("Struts2权威指南");
        System.out.println("c集合是否完全包含books集合？" + c.containsAll(books));

        c.removeAll(books);//用c集合减去books集合里的元素
        System.out.println("c集合的元素：" + c);

        c.clear();//删除c集合里所有元素
        System.out.println("c集合的元素：" + c);

        books.retainAll(c);//books集合里只剩下c集合里也同时包含的元素
        System.out.println("books集合的元素:" + books);
    }
}
```

程序输出结果：
```xml
c集合的元素个数为:2 
c集合的元素个数为:1 
c集合的是否包含孙悟空字符串:true 
c集合的元素：[孙悟空, 轻量级J2EE企业应用实战] 
c集合是否完全包含books集合？false 
c集合的元素：[孙悟空] 
c集合的元素：[] 
books集合的元素:[]
```
#### **习题：使用LinkedList模拟一个堆栈或者队列数据结构**
**堆栈：先进后出**  如同一个杯子。
**队列：先进先出** First in First out  FIFO 如同一个水管。
要点：使用LinkedList的pollFirst、pollLast

```
import java.util.*;
public class DuiLie// 把方法封装起来
{
	private LinkedList link;

	DuiLie() {
		link = new LinkedList();
	}

	public void myAdd(Object obj) {
		link.addFirst(obj);
	}

	public Object myGet() {
//		return link.removeFirst();//堆栈：先进后出
		//return link.removeLast();//队列：先进先出
	
		//1.6之后替代方法
		return link.pollFirst();// 堆栈：先进后出
//		 return link.pollLast();//队列：先进先出
	}

	public boolean isNull() {
		return link.isEmpty();
	}
}
```
```
class  LinkedListTest
{
	public static void main(String[] args) 
	{
		DuiLie dl = new DuiLie();
		dl.myAdd("java01");
		dl.myAdd("java02");
		dl.myAdd("java03");
		dl.myAdd("java04");
		while(!dl.isNull())
		{
			System.out.println(dl.myGet());
		}
	}
}
```
运行结果：
```
java04
java03
java02
java01
```


#### 习题：**去除ArrayList集合中的重复元素**

要点：定义一个临时容器newAl ，迭代时，通过ArrayList的contains找出newAl中没有的元素加进去
```
public class ArrayListTest {
	public static void main(String[] args) {
		ArrayList al = new ArrayList();
		al.add("java01");
		al.add("java02");
		al.add("java01");
		al.add("java02");
		al.add("java01");
		al.add("java03");
		/*在迭代时循环中next调用一次，就要hasNext判断一次。下面这样两次next不可取
        Iterator it = al.iterator();
        while(it.hasNext())
        {
            sop(it.next()+"...."+it.next());
            //取两次next再判断，易导致无元素异常；如元素是奇数，最后一次只剩一个元素，第2个next元素就是空，导致异常
        }*/
		System.out.println(al);

		al = singleElement(al);
		System.out.println(al);
	}

	public static ArrayList singleElement(ArrayList al) { // 定义一个临时容器。
		ArrayList newAl = new ArrayList();
		Iterator it = al.iterator();
		while (it.hasNext()) {
			Object obj = it.next();
			if (!newAl.contains(obj))
				newAl.add(obj);
		}
		return newAl;
	}
}
```
运行结果：
```
[java01, java02, java01, java02, java01, java03]
[java01, java02, java03]
```

#### 习题：**将自定义对象作为元素存到ArrayList集合中，并去除重复元素** 
比如：存人对象。同姓名同年龄，视为同一个人。为重复元素。

思路：
1，对人描述，将数据封装进人对象。
2，定义容器，将人存入。
3，取出。
List集合判断元素是否相同，依据是元素的**equals**方法。

```
class ArrayListTest2 {

	public static void main(String[] args) {
		ArrayList al = new ArrayList();
		al.add(new Person("lisi01", 30));// al.add(Object obj);//Object obj = new Person("lisi01",30);
		al.add(new Person("lisi02", 32));
		al.add(new Person("lisi02", 32));
		al.add(new Person("lisi04", 35));
		al.add(new Person("lisi03", 33));
		al.add(new Person("lisi04", 35));
		al = singleElement(al);// 取出非重复内容

		System.out.println("remove 03 :" + al.remove(new Person("lisi03", 33)));
		// remove方法底层也是依赖于元素的equals方法。用"lisi03",33与集合中元素一个个比较，找到相同的就把它删除

		Iterator it = al.iterator();
		while (it.hasNext())
		// it里面的元素是Person类类型的,直接System.out.println打印出的是哈希地址值,故用next打印
		{
			/*
			 * Object obj=it.next(); 
			 * Person p=(Person)obj;
			 * 因getName getAge都是Object子类Person特有的方法，p需要向下转型；此2句转成下面1句
			 */
			Person p = (Person) it.next();// 简化向下转型
			System.out.println(p.getName() + "::" + p.getAge());
		}
	}

	public static ArrayList singleElement(ArrayList al) {
		// 定义一个临时容器。
		ArrayList newAl = new ArrayList();
		Iterator it = al.iterator();
		while (it.hasNext()) {
			System.out.println("=======next=======");
			Object obj = it.next();
			if (!newAl.contains(obj)) {
				// contains在底层调用equals，所以在子类Person中复写了equals方法，否则比较的是对象，而不是其内的姓名年龄
				System.out.println("不重复");

				newAl.add(obj);
			} else {
				System.out.println("重复");
			}
			System.out.println("临时容器：" + newAl);
		}
		return newAl;
	}
}

```

```
class Person {
	private String name;
	private int age;

	Person(String name, int age) {
		this.name = name;
		this.age = age;
	}

	public boolean equals(Object obj)// 复写掉Object的equals方法
	{
		if (!(obj instanceof Person))
			return false;// 类型都不同，根本不同比较
		Person p = (Person) obj;
//		System.out.println("this.name=" + this.name + " age=" + this.age + ".....p.name=" + p.name + " age=" + p.age);// 显示比较的过程
		System.out.println("this = " + this.name + " " + this.age + ".....p = " + p.name + " " + p.age);// 显示比较的过程
		return this.name.equals(p.name) && this.age == p.age;// 子符串equals方法，当姓名年龄相同时返回true
	}

	public String getName() {
		return name;
	}

	public int getAge() {
		return age;
	}
	
	@Override
	public String toString() {
		return this.name + " " + this.age;
	}
}
```
运行结果：

```
=======next=======
不重复
临时容器：[lisi01 30]
=======next=======
this = lisi02 32.....p = lisi01 30
不重复
临时容器：[lisi01 30, lisi02 32]
=======next=======
this = lisi02 32.....p = lisi01 30
this = lisi02 32.....p = lisi02 32
重复
临时容器：[lisi01 30, lisi02 32]
=======next=======
this = lisi04 35.....p = lisi01 30
this = lisi04 35.....p = lisi02 32
不重复
临时容器：[lisi01 30, lisi02 32, lisi04 35]
=======next=======
this = lisi03 33.....p = lisi01 30
this = lisi03 33.....p = lisi02 32
this = lisi03 33.....p = lisi04 35
不重复
临时容器：[lisi01 30, lisi02 32, lisi04 35, lisi03 33]
=======next=======
this = lisi04 35.....p = lisi01 30
this = lisi04 35.....p = lisi02 32
this = lisi04 35.....p = lisi04 35
重复
临时容器：[lisi01 30, lisi02 32, lisi04 35, lisi03 33]
this = lisi03 33.....p = lisi01 30
this = lisi03 33.....p = lisi02 32
this = lisi03 33.....p = lisi04 35
this = lisi03 33.....p = lisi03 33
remove 03 :true
lisi01::30
lisi02::32
lisi04::35
```

#### HashSet
```
public class HashSetDemo {
	public static void main(String[] args) {
		HashSet hs = new HashSet();
		System.out.println(hs.add("java01"));
		System.out.println(hs.add("java01"));// 元素不可以重复，此处为false
		
		hs.add("java04");
		hs.add("java03");
		hs.add("java03");
		hs.add("java02");
		Iterator it = hs.iterator();
		while (it.hasNext()) {
			System.out.println(it.next());// 元素是无序(存入和取出的顺序不一定一致)，根据哈希值排序
		}
	}
}
```
结果：

```xml
true
false
java04
java03
java02
java01
```
#### 习题：**往hashSet集合中存入自定对象，姓名和年龄相同不重复存**
姓名和年龄相同为同一个人，重复元素，复写Object的hashCode、equals。
```
public class Human {
	private String name;
	private int age;

	Human(String name, int age) {
		this.name = name;
		this.age = age;
	}

	public int hashCode()
	// 复写，否则不同对象不同哈希值直接存到集合中，还轮不到运行equals方法
	// HashSet通过hashcode和equals方法保证元素唯一性，所以复写这两个方法，以自定义判断唯一性的标准
	{
		System.out.println("调用hashCode：" + this.name);
		return name.hashCode() + age * 39;// *39尽量保证返回哈希值惟一（39为任意数，也不能太大以防超出int的范围）
	}

	public boolean equals(Object obj) {// 复写Object的equals，此处不能用泛型
		if (!(obj instanceof Human))
			return false;
		Human p = (Human) obj;
		System.out.println("调用equals：" + this.name + "...equals.." + p.name);
		return this.name.equals(p.name) && this.age == p.age;
	}

	public String getName() {
		return name;
	}

	public int getAge() {
		return age;
	}
}
```
```
public class HashSetTest {
	public static void main(String[] args) {
		HashSet hs = new HashSet();
		System.out.println("add操作");
		System.out.println("add \"a1\", 11");
		hs.add(new Human("a1", 11));
		System.out.println("add \"a2\", 12");
		hs.add(new Human("a2", 12));
		System.out.println("add \"a3\", 13");
		hs.add(new Human("a3", 13));
		System.out.println("add \"a2\", 12");
		hs.add(new Human("a2", 12));

		System.out.println("contains操作");
		System.out.println("contains \"a2\", 12:" + hs.contains(new Human("a2", 12)));

		System.out.println("remove操作");
		System.out.println("remove \"a4\", 13:" + hs.remove(new Human("a4", 13)));
		System.out.println("remove \"a2\", 12:" + hs.remove(new Human("a2", 12)));
		
		Iterator it = hs.iterator();
		while (it.hasNext()) {
			Human p = (Human) it.next();
			System.out.println(p.getName() + "::" + p.getAge());
		}
	}
}
```

结果：
```xml
add操作
add "a1", 11
调用hashCode：a1
add "a2", 12
调用hashCode：a2
add "a3", 13
调用hashCode：a3
add "a2", 12
调用hashCode：a2
调用equals：a2...equals..a2
contains操作
调用hashCode：a2
调用equals：a2...equals..a2
contains "a2", 12:true
remove操作
调用hashCode：a4
remove "a4", 13:false
调用hashCode：a2
调用equals：a2...equals..a2
remove "a2", 12:true
a1::11
a3::13
```
#### 习题：往TreeSet集合中存储自定义对象学生，按照年龄排序

```java
//Comparable接口强制让学生具备比较性。否则会出现类型转换异常ClassCastException（因为Students没比较性，无Comparable）
public class Student implements Comparable{
	private String name;
	private int age;

	Student(String name, int age) {
		this.name = name;
		this.age = age;
	}

	@Override
	public int compareTo(Object obj)
	// 它复写父类方法，父类没声明异常，所以它不能声明异常。compareTo返回0，视为相同不存入；返-1，新元素存在this左边；返1，新元素存在this右边
	{
		// 按自定义方法判断唯一性及排序
		if (!(obj instanceof Student))
			throw new RuntimeException("不是学生对象");
		
		Student s = (Student) obj;

		if (this.age > s.age)
			return 1;// 新元素存在this右边
		if (this.age == s.age) {
			return this.name.compareTo(s.name);
		}
		return -1;// 新元素存在this左边

		// 全视为相同，只存入第一个
//		 return 0;

		// 按录入顺序排序（先进先出）
//		 return 1;

		// 按录入顺序倒序
//		 return -1;
	}

	public String getName() {
		return name;
	}

	public int getAge() {
		return age;
	}
}
```

```
public class TreeSetDemo {
	public static void main(String[] args) {
		TreeSet ts = new TreeSet();		
		ts.add(new Student("lisi01", 1));
		ts.add(new Student("lisi03", 3));
		ts.add(new Student("lisi02", 2));
		ts.add(new Student("lisi05", 5));
		ts.add(new Student("lisi04", 4));
		ts.add(new Student("lisi07", 7));
		ts.add(new Student("lisi06", 6));
		print(ts);

		System.out.println("\"add\" lisi00, 0：" + ts.add(new Student("lisi00", 0)));
		print(ts);

		System.out.println("\"add\" lisi07, 7：" + ts.add(new Student("lisi07", 7)));
		print(ts);
		
		System.out.println("\"add\" lisi02, 3：" + ts.add(new Student("lisi02", 3)));
		print(ts);

	}

	public static void print(TreeSet ts) {
		System.out.print("【TreeSet】");
		Iterator it = ts.iterator();
		while (it.hasNext()) {
			Student stu = (Student) it.next();
			System.out.print(stu.getName() + " " + stu.getAge() + ", ");
		}
		System.out.println();
		System.out.println();
	}
}
```
结果：
```xml
【TreeSet】lisi01 1, lisi02 2, lisi03 3, lisi04 4, lisi05 5, lisi06 6, lisi07 7, 

"add" lisi00, 0：true
【TreeSet】lisi00 0, lisi01 1, lisi02 2, lisi03 3, lisi04 4, lisi05 5, lisi06 6, lisi07 7, 

"add" lisi07, 7：false
【TreeSet】lisi00 0, lisi01 1, lisi02 2, lisi03 3, lisi04 4, lisi05 5, lisi06 6, lisi07 7, 

"add" lisi02, 3：true
【TreeSet】lisi00 0, lisi01 1, lisi02 2, lisi02 3, lisi03 3, lisi04 4, lisi05 5, lisi06 6, lisi07 7, 


```

#### 习题：往TreeSet集合中存储自定义对象学生，通过比较器按录入顺序倒序
**比较器优先**
```
Student 与上题一样
```
```
public class MyCompare implements Comparator {
	public int compare(Object o1, Object o2) {
		return -1;
	}
}
```

```
public class TreeSetDemo2 {
	public static void main(String[] args) {
		TreeSet ts = new TreeSet(new MyCompare());// 比较器优先
		// TreeSet ts = new TreeSet();//Student的排序方法
		ts.add(new Student("lisi01", 1));
		ts.add(new Student("lisi03", 3));
		ts.add(new Student("lisi02", 2));
		ts.add(new Student("lisi05", 5));
		ts.add(new Student("lisi04", 4));
		ts.add(new Student("lisi07", 7));
		ts.add(new Student("lisi06", 6));
		print(ts);

		System.out.println("\"add\" lisi00, 0：" + ts.add(new Student("lisi00", 0)));
		print(ts);

		System.out.println("\"add\" lisi07, 7：" + ts.add(new Student("lisi07", 7)));
		print(ts);

		System.out.println("\"add\" lisi02, 3：" + ts.add(new Student("lisi02", 3)));
		print(ts);

	}

	public static void print(TreeSet ts) {
		System.out.print("【TreeSet】");
		Iterator it = ts.iterator();
		while (it.hasNext()) {
			Student stu = (Student) it.next();
			System.out.print(stu.getName() + " " + stu.getAge() + ", ");
		}
		System.out.println();
		System.out.println();
	}
}
```

结果：
```xml
【TreeSet】lisi06 6, lisi07 7, lisi04 4, lisi05 5, lisi02 2, lisi03 3, lisi01 1, 

"add" lisi00, 0：true
【TreeSet】lisi00 0, lisi06 6, lisi07 7, lisi04 4, lisi05 5, lisi02 2, lisi03 3, lisi01 1, 

"add" lisi07, 7：true
【TreeSet】lisi07 7, lisi00 0, lisi06 6, lisi07 7, lisi04 4, lisi05 5, lisi02 2, lisi03 3, lisi01 1, 

"add" lisi02, 3：true
【TreeSet】lisi02 3, lisi07 7, lisi00 0, lisi06 6, lisi07 7, lisi04 4, lisi05 5, lisi02 2, lisi03 3, lisi01 1, 


```



#### 习题：使用比较器，按照字符串长度排序

```
// class LenComparator implements Comparator<String>
// 如泛型在比较器中的运用用泛型，下面就不用强转了（Object强转为String）
public class StrLenComparator implements Comparator {
	public int compare(Object o1, Object o2) {
		String s1 = (String) o1;// 需要新增的对象
		String s2 = (String) o2;// 集合中比较对象
		int num = new Integer(s1.length()).compareTo(new Integer(s2.length()));
		if (num == 0)
			return s1.compareTo(s2);
		return num;
	}
}
```

```
public class TreeSetTest {
	public static void main(String[] args) {
		TreeSet ts = new TreeSet(new StrLenComparator());
		ts.add("abcd");
		ts.add("cc");
		ts.add("cba");
		ts.add("aaa");
		ts.add("z");
		ts.add("hahaha");
		ts.add("aaa");
		
		Iterator it = ts.iterator();
		while (it.hasNext()) {
			System.out.println(it.next());
		}
	}
}
```
结果：
```xml
z
cc
aaa
cba
abcd
hahaha
```
### 
## Map集合
Map集合：该集合**存储键值对**。一对一对往里存。而且要保证**键**的**唯一**性。
1，添加。
　　**put**(K key, V value) //如果添加时，出现相同的键，后添加的值会覆盖原有键对应值，put方法会返回被覆盖的值。
　　**putAll**(Map<? extends K,? extends V> m) 
2，删除。
　　**clear**() 
　　**remove**(Object key) 
3，判断。
　　**containsValue**(Object value) 
　　**containsKey**(Object key) 
　　**isEmpty**() 
4，获取。
　　**get**(Object key) //可通过get能否获取到值,以判断是否存在
　　**size**() 
　　**values**() //返回此映射中包含的值的 Collection 视图
　　**entrySet**() 
　　**keySet**() 

|                 返回值 | 方法摘要                                     |
| ------------------: | ---------------------------------------- |
|                void | **clear**() <br>          从此映射中移除所有映射关系（可选操作）。 |
|             boolean | **containsKey**(Object key) <br>          如果此映射包含指定键的映射关系，则返回 true。 |
|             boolean | **containsValue**(Object value) <br>          如果此映射将一个或多个键映射到指定值，则返回 true。 |
| Set<Map.Entry<K,V>> | **entrySet**() <br>          返回此映射中包含的映射关系的 *Set* 视图。 |
|             boolean | **equals**(Object o) <br>          比较指定的对象与此映射是否相等。 |
|                   V | **get**(Object key) <br>          返回指定键所映射的值；如果此映射不包含该键的映射关系，则返回 null。 |
|                 int | **hashCode**() <br>          返回此映射的哈希码值。 |
|             boolean | **isEmpty**() <br>          如果此映射未包含键-值映射关系，则返回 true。 |
|           Set&lt;K> | **keySet**() <br>          返回此映射中包含的键的 *Set* 视图。 |
|                   V | **put**(K key, V value) <br>          将指定的值与此映射中的指定键关联（可选操作）。 |
|                void | **putAll**(Map&lt;? extends K,? extends V> m) <br>          从指定映射中将所有映射关系复制到此映射中（可选操作）。 |
|                   V | **remove**(Object key) <br>          如果存在一个键的映射关系，则将其从此映射中移除（可选操作）。 |
|                 int | **size**() <br>          返回此映射中的键-值映射关系数。 |
|  Collection&lt;V>             | **values**() <br>          返回此映射中包含的值的 *Collection* 视图。 



### Map集合常用类
#### Hashtable
 \|--**Hashtable**:底层是**哈希表数据结构**，**不**可以**存**入**null**键null值。该集合是**线程同步**的。jdk1.0.效率低。
#### HashMap
 \|--**HashMap**：底层是**哈希表数据结构**，**允许**使用 **null** 值和 null键，该集合是**不同步**的。**替代hashtable**，jdk1.2.效率高。（看到HashMap想到HashSet，故HashMap也是通过**hashCode**、**equals**两个方法保证**键惟一**性）
#### TreeMap
 \|--**TreeMap**：底层是**二叉树**数据结构。线程**不同步**。**可**以用于给map集合中的**键**进行**排序**。对键进行排序，排序原理与TreeSet相同。

Map和Set很像。其实，**Set底层就是使用了Map集合**。

```java
class  MapDemo
{
	public static void main(String[] args) 
	{
		Map<String,String> map = new HashMap<String,String>();
//<String,String>泛型限定键K和值V的类型。多态。
//添加元素，如果添加时，出现相同的键。那么后添加的值会覆盖原有键对应值，put方法会返回被覆盖的值。
		System.out.println("put:"+map.put("01","zhangsan1"));//put: null
		System.out.println("put:"+map.put("01","wnagwu"));//put: zhangsan1
		map.put("02","zhangsan2");
		map.put("03","zhangsan3");
		System.out.println("containsKey:"+map.containsKey("022"));
		System.out.println("get:"+map.get("02"));//get:zhangsan2
		System.out.println("remove:"+map.remove("02"));//remove:zhangsan2
		System.out.println("get:"+map.get("02"));//get:null
//可以通过get方法的返回值来判断一个键是否存在。通过是否返回null来判断。
//特殊情况：如下某键存入对应值为null时，也会返回null；但该情况无意义，实际不会出现
		map.put("04",null);
		map.put(null,"空");//HashMap可以存入null值&null键
		System.out.println("get:"+map.get("04"));
		//获取map集合中所有的值。
		System.out.println(map.values());
		/*Collection<String> coll = map.values();
		System.out.println(coll);*/
		// 打印值：[空, wnagwu, zhangsan3, null]
		System.out.println(map); 
		//打印键=值：{null=空, 01=wnagwu, 03=zhangsan3, 04=null}
		System.out.println(map.size());//4
	}
}
```

### map集合的两种取出方式：

#### 1，keySet
**Set&lt;k>keySet**
将map中所有的**键**存入到Set集合。因为set具备迭代器。以**迭代**方式取出所有的键，再根据**get**方法，获取每一个键对应的**值**。

Map集合的**取出原理**：将map集合**转成set集合**，在**通过迭代器取出**。

#### 2，entrySet
**Set&lt;Map.Entry&lt;k,v>>entrySet**
将map集合中的**映射关系**存入到了set集合中，而这个关系的数据类型就是：**Map.Entry**

**Entry**其实就是**Map**中的一个static**内部接口**。

为什么要定义在**内部**呢？
因为只有有了Map集合，有了键值对，才会有键值的映射关系。
关系属于Map集合中的一个内部事物。
而且该事物在直接访问Map集合中的元素。

#### keySet方法图例
![keySet方法图例](http://img.blog.csdn.net/20180204000049682?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

#### entrySet方法图例
![entrySet方法图例](http://img.blog.csdn.net/20180204000109870?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

#### 实例
```
public class MapDemo2 {
	// 方法一：keyset
	public static void keyset(Map<String, String> map) {
		// 先获取map集合的所有键的Set集合,keySet();
		Set<String> keySet = map.keySet();
		
		// 有了Set集合。就可以获取其迭代器。
		Iterator<String> it = keySet.iterator();
		
		while (it.hasNext()) {
			String key = it.next();
			
			// 有了键可以通过map集合的get方法获取其对应的值。
			String value = map.get(key);
			System.out.println("key:" + key + ",value:" + value);
		}
	}

	// 方法二：entrySet
	public static void entrySet(Map<String, String> map) {
		// 将Map集合中的映射关系取出。存入到Set集合中。
		Set<Map.Entry<String, String>> entrySet = map.entrySet();
		
		Iterator<Map.Entry<String, String>> it = entrySet.iterator();
		
		while (it.hasNext()) {
			Map.Entry<String, String> me = it.next();
			
			String key = me.getKey();
			String value = me.getValue();
			System.out.println(key + ":" + value);
		}
	}
	public static void main(String[] args) {
		Map<String, String> map = new HashMap<String, String>();
		map.put("02", "zhangsan2");
		map.put("03", "zhangsan3");
		map.put("01", "zhangsan1");
		map.put("04", "zhangsan4");
		keyset(map);
		entrySet(map);
	}
}
```
##### 习题：每一个学生都有对应的归属地
学生Student，地址String，学生属性：姓名，年龄。
注意：姓名和年龄相同的视为同一个学生，保证学生的唯一性。
思路：
1，描述学生。(复写compareTo， hashCode和equals;保证学生唯一性)
2，定义map容器。将学生作为键，地址作为值存入。
3，获取map集合中的元素。

```
class Student implements Comparable<Student> {
	// Student最后存成什么数据结构并不知道，可能是二叉树数据结构，也可能是哈希表数据结构；
	// 故既要实现Comparable复写compareTo，又要复写hashCode和equals

	public int compareTo(Student s) {
		int num = new Integer(this.age).compareTo(new Integer(s.age));
		if (num == 0)
			return this.name.compareTo(s.name);
		return num;
	}

	public int hashCode() {
		return name.hashCode() + age * 34;
	}

	// 复写Object的equals方法；不写泛型，因Object的equals方法中没写，是固定上面这种写法
	public boolean equals(Object obj) {
		if (!(obj instanceof Student))
			throw new ClassCastException("类型不匹配");
		Student s = (Student) obj;
		return this.name.equals(s.name) && this.age == s.age;
	}
	
	private String name;
	private int age;
	Student(String name, int age) {
		this.name = name;
		this.age = age;
	}
	public String getName() {
		return name;
	}
	public int getAge() {
		return age;
	}
	public String toString() {
		return name + ":" + age;
	}
}
```

```
class MapTest {
	public static void main(String[] args) {
		HashMap<Student, String> hm = new HashMap<Student, String>();
		hm.put(new Student("lisi1", 21), "beijing");
		hm.put(new Student("lisi1", 21), "tianjin");// 同一学生，覆盖，地址为tianjin
		hm.put(new Student("lisi2", 20), "shanghai");
		hm.put(new Student("lisi3", 20), "nanjing");
		hm.put(new Student("lisi3", 24), "wuhan");
		
		// 第一种取出方式 keySet
		Iterator<Student> it = hm.keySet().iterator();// 下两句合并为此
	/* Set<Student> keySet = hm.keySet();
		Iterator<Student> it = keySet.iterator();	*/
		
		while (it.hasNext()) {
			Student stu = it.next();// next获取键
			String addr = hm.get(stu);// 通过键获取对应值
			System.out.println(stu + ".." + addr);
		}

		// 第二种取出方式 entrySet
		Iterator<Map.Entry<Student, String>> iter = hm.entrySet().iterator();
		while (iter.hasNext()) {
			Map.Entry<Student, String> me = iter.next();// 下四句合并为此
			System.out.println(me.getKey() + "住" + me.getValue());
		/* Map.Entry<Student,String> me = iter.next();
			Student stu = me.getKey();
			String addr = me.getValue();
			System.out.println(stu+"........."+addr); */ 	
		}
	}
}
```
运行结果：
```xml
lisi3:24..wuhan
lisi2:20..shanghai
lisi1:21..tianjin
lisi3:20..nanjing
lisi3:24住wuhan
lisi2:20住shanghai
lisi1:21住tianjin
lisi3:20住nanjing
```



##### 习题：对学生对象的姓名进行升序排序(上题中学生对象)
分析：因为数据以键值对形式存在，所以使用**可以排序的Map集合——TreeMap**。
比较器Comparator优先于compareTo（上题中Student先年龄排序再姓名排序）

```
//比较器优先于Student类中的自然顺序
public class StuNameComparator implements Comparator<Student> {
	public int compare(Student s1, Student s2) {
		int num = s1.getName().compareTo(s2.getName());
		if (num == 0)
			return new Integer(s1.getAge()).compareTo(new Integer(s2.getAge()));
		return num;
	}
}
```
```
public class MapTest2 {
	public static void main(String[] args) {
		TreeMap<Student, String> tm = new TreeMap<Student, String>(new StuNameComparator());// 使用比较器
		tm.put(new Student("blisi3", 23), "nanjing");
		tm.put(new Student("lisi1", 21), "beijing");
		tm.put(new Student("alisi4", 24), "wuhan");
		tm.put(new Student("lisi1", 21), "tianjin");
		tm.put(new Student("lisi2", 22), "shanghai");
		Set<Map.Entry<Student, String>> entrySet = tm.entrySet();
		Iterator<Map.Entry<Student, String>> it = entrySet.iterator();
		while (it.hasNext()) {
			Map.Entry<Student, String> me = it.next();
			Student stu = me.getKey();
			String addr = me.getValue();
			System.out.println(stu + ":::" + addr);
		}
	}
}
```

运行结果
```xml
alisi4:24:::wuhan
blisi3:23:::nanjing
lisi1:21:::tianjin
lisi2:22:::shanghai
```

##### 习题："ak+abAf1c,dCkaAbc-defa"获取该字符串中的字母出现的次数希望打印结果：a(4) b(2)c(2).....
分析：通过结果发现，每一个字母都有对应的次数。说明字母和次数之间都有映射关系。
注意，**当数据之间存在映射关系，就要先想map集合**。因map集合中存放就是映射关系。
思路：
1，将字符串转换成`字符数组`。因为要对每一个字母进行操作。
2，定义一个map集合，因为打印结果的字母有`顺序`，所以使用`treemap`集合。
3，`遍历字符数组`。
　　将每一个字母作为键去查map集合。　　
　　如果返回null，将该字母和1存入到map集合中。
　　如果返回不是null，说明该字母在map集合已经存在并有对应次数。
　　那么就获取该次数并进行自增，然后将该字母和自增后的次数存入到map集合中。覆盖调用原键所对应的值。
4，将map集合中的数据变成指定的字符串形式返回。

```
public class MapTest3 {
	public static void main(String[] args) {
		String s = charCount("ak+abAf1c,dCkaAbc-defa");
		System.out.println(s);
	}

	public static String charCount(String str) {
		char[] chs = str.toCharArray();// 将字符串转换成字符数组

		TreeMap<Character, Integer> tm = new TreeMap<Character, Integer>();
		// 泛型类的泛型接收引用数据类型，得用char和int对应的基本数据包装类
		// Character继承了Comparable，所以键进行了排序

		for (int x = 0; x < chs.length; x++) {
			if (!(chs[x] >= 'a' && chs[x] <= 'z' || chs[x] >= 'A' && chs[x] <= 'Z')) {
				continue;// 不是字母的，就继续循环；出现字母就往下走
			}

			Integer value = tm.get(chs[x]);
			// 方式一：
			if (value == null) {
				tm.put(chs[x], 1);
				// 直接往集合中存储字符和数字，为什么可以，因为自动装箱。
			} else {
				tm.put(chs[x], ++value); // 下两句合并为此
				/*	value = value + 1;
					tm.put(chs[x], value); */
			}
		}

		StringBuilder sb = new StringBuilder();
		// 缓冲区什么都能放，且可toString
		Set<Map.Entry<Character, Integer>> entrySet = tm.entrySet();
		Iterator<Map.Entry<Character, Integer>> it = entrySet.iterator();
		while (it.hasNext()) {
			Map.Entry<Character, Integer> me = it.next();
			Character ch = me.getKey();
			Integer value = me.getValue();
			sb.append(ch + "(" + value + ")");
		}
		return sb.toString();
	}
}
```

结果：
```
A(2)C(1)a(4)b(2)c(2)d(2)e(1)f(2)k(2)
```

### map扩展知识：多映射关系
一个学校有多个教室，每个教室都有名称；每个教室都有多个学生，学生有学号姓名

```
public class MapDemo3 {
	public static void main(String[] args) {
		HashMap<String, List<Student3>> xx = new HashMap<String, List<Student3>>();// 学校<教室名，教室<学生>>
		List<Student3> bj1 = new ArrayList<Student3>();// 教室<学生>
		List<Student3> bj2 = new ArrayList<Student3>();
		xx.put("bj1", bj1);// 把教室放学校里
		xx.put("bj2", bj2);
		
		bj1.add(new Student3("01", "zhagnsa"));// 把学生放教室里
		bj1.add(new Student3("04", "wangwu"));
		bj2.add(new Student3("01", "zhouqi"));
		bj2.add(new Student3("02", "zhaoli"));
		
		Iterator<String> it = xx.keySet().iterator();
		while (it.hasNext()) {
			String roomName = it.next();
			List<Student3> room = xx.get(roomName);
			System.out.println(roomName);
			getInfos(room);
		}
	}

	public static void getInfos(List<Student3> list) {
		Iterator<Student3> it = list.iterator();
		while (it.hasNext()) {
			Student3 s = it.next();
			System.out.println(s);
		}
	}
}
```
```
public class Student3 {
	// 复写toString，否则打印的是对象（如：Student@15db9742）而非id+name
	public String toString() {
		return id + ":::" + name;
	}

	private String id;
	private String name;
	Student3(String id, String name) {
		this.id = id;
		this.name = name;
	}
}
```
结果：
```
bj1
01:::zhagnsa
04:::wangwu
bj2
01:::zhouqi
02:::zhaoli
```


## Map vs Collection
Map与Collection在集合框架中属并列存在
| **比较** | **Map**              | **Collection** |
| ------ | -------------------- | -------------- |
| 特点     | 存储的是键值对 <br> 键要保证唯一性 |                |
| 存储元素   | 使用put方法              | 使用add方法        |
| 取出元素   | 先转成Set集合，再通过迭代获取元素   | 直接get或迭代       |


## 集合框架中的工具类
### Collections
**集合**框架的工具类，里面定义的**都是静态方法**。
• 对集合进行查找
• 取出集合中的最大值，最小值
• 对List集合进行排序
• ……
#### Collections和Collection有什么区别？
**Collection**是集合框架中的一个**顶层接口**，它里面定义了**单列集合的共性方法**。
　它有**两个常用子接口**，
　　**List**：对元素都有定义索引。有序的。可以重复元素。
　　**Set**：不可以重复元素。无序。

**Collections**是集合框架中的一个**工具类**。该类中的**方法都是静态**的
　提供的方法中有可以对list集合进行**排序**，**二分查找等**方法。
　通常常用的集合都是**线程不安全**的，因为要提高效率。
　如果多线程操作这些集合时，可以通过该**工具类中的同步方法**，将线程不安全的集合，**转换成安全**的。

|                **方法摘要** |                                          |
| ----------------------: | ---------------------------------------- |
|                  static | **binarySearch**(List&lt;? extends T> list, T key, Comparator&lt;? super T> c) |
|              &lt;T> int | 使用二分搜索法搜索指定列表，以获得指定对象。                   |
|                  static | **fill**(List&lt;? super T> list, T obj) |
|             &lt;T> void | 使用指定元素替换指定列表中的所有元素。                      |
|                  static | **max**(Collection&lt;? extends T> coll, Comparator&lt;? super T> comp) |
|                &lt;T> T | 根据指定比较器产生的顺序，返回给定 collection 的最大元素。      |
|                  static | **reverseOrder**(Comparator&lt;T> cmp)   |
| &lt;T> Comparator&lt;T> | 返回一个比较器，它强行逆转指定比较器的顺序。                   |
|             static void | **shuffle**(List&lt;?> list, Random rnd) |
|                         | 使用指定的随机源对指定列表进行置换。                       |
|                  static | **sort**(List&lt;T> list, Comparator&lt;? super T> c) |
|             &lt;T> void | 根据指定比较器产生的顺序对指定列表进行排序。                   |
|                  static | **synchronizedList**(List&lt;T> list)    |
|       &lt;T> List&lt;T> | 返回指定列表支持的同步（线程安全的）列表。                    |

**binarySearch**：如果搜索键包含在列表中，则返回搜索键的索引；**否则返回(-(插入点) - 1)**。
**插入点**被定义为将键插入列表的那一点：即**第一个大于此键的元素索引**；如果列表中的所有元素都小于指定的键，则为list.size()。

#### Collections方法实例
##### sort、binarySearch、max

```java
public class CollectionsDemo {
	public static void main(String[] args) {
		List<String> list = new ArrayList<String>();
		list.add("abcd");
		list.add("aaa");
		list.add("zz");
		list.add("kkkkk");
		list.add("qq");
		list.add("z");
		Collections.sort(list);// Collections.sort
		System.out.println(list);// [aaa, abcd, kkkkk, qq, z, zz]

		int index = Collections.binarySearch(list, "aaaa");// Collections.binarySearch
		System.out.println("binarySearch aaaa=" + index); // -2

		int index1 = halfSearch(list, "aaaa");
		System.out.println("halfSearch cc=" + index1); // -2

		Collections.sort(list, new StrLenComparator()); //按照比较器排序
		System.out.println(list);// [z, qq, zz, aaa, abcd, kkkkk]

		int index2 = halfSearch2(list, "aaaa", new StrLenComparator());
		// 按new StrLenComparator()排序，也要按它进行查找
		System.out.println("halfSearch2 aaaa=" + index2);// -5
		System.out.println(Collections.binarySearch(list, "aaaa", new StrLenComparator()));// -5

		//Collections.max
		System.out.println(Collections.max(list));// zz

		// 根据指定比较器产生的顺序，返回给定 collection 的最大元素。
		System.out.println(Collections.max(list, new StrLenComparator()));// kkkk
	}

	// 模拟binarySearch
	public static int halfSearch(List<String> list, String key) {
		int max, min, mid;
		max = list.size() - 1;
		min = 0;
		while (min <= max) {
			mid = (max + min) >> 1;// /2;
			String str = list.get(mid);
			int num = str.compareTo(key);
			if (num > 0)
				max = mid - 1;
			else if (num < 0)
				min = mid + 1;
			else
				return mid;
		}
		return -min - 1;
	}

	public static int halfSearch2(List<String> list, String key, Comparator<String> cmp) {// 以比较器Comparator中自定义的方法进行比较
		int max, min, mid;
		max = list.size() - 1;
		min = 0;
		while (min <= max) {
			mid = (max + min) >> 1;// /2;
			String str = list.get(mid);
			int num = cmp.compare(str, key);
			if (num > 0)
				max = mid - 1;
			else if (num < 0)
				min = mid + 1;
			else
				return mid;
		}
		return -min - 1; // 搜索键不包含在列表中，返回 (-(插入点) - 1)
	}
}
```
```java
//比较器
public class StrLenComparator implements Comparator<String> {
	public int compare(String s1, String s2) {
		if (s1.length() > s2.length())
			return 1;
		if (s1.length() < s2.length())
			return -1;
		return s1.compareTo(s2);// 长度一样则内容进行比较
	}
}
```

##### replaceAll、reverse、shuffle、fill、reverseOrder

```java
public class CollectionsDemo2 {
	public static void main(String[] args) {
		List<String> list = new ArrayList<String>();
		list.add("abcd");
		list.add("aaa");
		list.add("zz");
		list.add("kkkkk");
		list.add("qq");
		list.add("z");
		list.add("aaa");
		System.out.println(list);// [abcd, aaa, zz, kkkkk, qq, z, aaa]

		Collections.replaceAll(list, "aaa", "w");// Collections.replaceAll替换，boolean
		System.out.println(list + "replaceAll");// [abcd, w, zz, kkkkk, qq, z, w]replaceAll

		Collections.reverse(list); // Collections.reverse反转，void
		System.out.println(list + "reverse");// [w, z, qq, kkkkk, zz, w, abcd]reverse

		Collections.shuffle(list); // Collections.shuffle随机排序，void
		System.out.println(list + "shuffle");// [z, abcd, w, qq, w, zz, kkkkk]shuffle

		Collections.fill(list, "rt");// Collections.fill：将list集合中所有元素替换成指定元素,void
		System.out.println(list + "fill");// [rt, rt, rt, rt, rt, rt, rt]fill

		TreeSet<String> ts = new TreeSet<String>(
				Collections.reverseOrder(new StrLenComparator()));
				//Collections.reverseOrder 按照比较器倒序
		ts.add("abcde");
		ts.add("aaa");
		ts.add("k");
		ts.add("cc");
		Iterator it = ts.iterator();
		while (it.hasNext()) {
			System.out.println(it.next());
		}
	}
}
```
```java
public class StrLenComparator implements Comparator<String> {
	public int compare(String s1, String s2) {
		if (s1.length() > s2.length())
			return 1;
		if (s1.length() < s2.length())
			return -1;
		return s1.compareTo(s2);// 长度一样则内容进行比较
	}
}
```

##### 习题：用fill将list集合中部分元素替换成指定元素

```
public class TestDemo {
	public static void main(String[] args) {
		List<String> list = new ArrayList<String>();
		list.add("aaaaa");
		list.add("bbbbb");
		list.add("ccccc");
		list.add("ddddd");
		list.add("fffff");
		fillDemo(list, "ee", 1, 3);// 将list集合中部分元素替换。
	}// [aaaaa, ee, ee, ddddd, fffff]

	public static void fillDemo(List<String> list, String str, int start, int end) {
		// 调用list中的subList方法。
		List<String> sublist = list.subList(start, end);
		
		// 将sublist中全部替换为str。
		Collections.fill(sublist, str);		
		System.out.println(list);
	}
}
```
### Arrays

用于操作**数组**的**工具类**，里面都是静态方法。

#### Arrays.asList
将数组变成list集合

**把数组变成list集合有什么好处？**
　　可以**使用集合的思想和方法来操作数组**中的元素。

**注意**：将数组变成集合，**不可以使用集合的增删**方法。
　　　因为**数组的长度是固定**的。如果增删，就会发生UnsupportedOperationException

**可使用**：contains、get、indexOf()、subList();

如数组中的元素都是**对象**。变成集合时，数组中的**元素**就**直接转成集合中的元素**。
如数组中的元素都是**基本数据类型**，那么会将该**数组**作为**集合中的元素**存在。

```
public class ArraysDemo {
	public static void main(String[] args) {
		int[] arr1 = { 7, 6, 8 };
		System.out.println(Arrays.toString(arr1) + "toString");// [7, 6, 8]
		
		String[] arr = { "abc", "cc", "kkkk" };
		List<String> list = Arrays.asList(arr); //Arrays.asList
		System.out.println("contains: " + list.contains("abc"));// true		
		// 集合可以直接.contains，但数组需自己做方法myContains实现(如下)
		
		// list.add("qq");//UnsupportedOperationException
		
		/* 如数组中的元素都是对象。变成集合时，数组中的元素就直接转成集合中的元素。
		如数组中的元素都是基本数据类型(int)，那么会将该数组作为集合中的元素存在。*/		
		Integer[] nums = { 2, 4, 5 };// Integer: int对应基本数据包装类，元素是对象
		List<Integer> li = Arrays.asList(nums);
		System.out.println(li);// [2, 4, 5]
		
		int[] nums1 = { 2, 4, 5 };// int: 基本数据类型
		List<int[]> li1 = Arrays.asList(nums1);
		System.out.println(li1);// [[I@659e0bfd]
	}

	// 数组模拟集合的contains方法
	public static boolean myContains(String[] arr, String key) {
		for (int x = 0; x < arr.length; x++) {
			if (arr[x].equals(key))
				return true;
		}
		return false;
	}
}
```

#### Collection.toArray
**集合变数组**。Collection接口中的toArray方法。

**1,指定类型的数组要定义多长呢？**
当指定类型的数组长度**小于**了集合的size，那么该方法内部会**创建**一个**新**的**数组**。长度为集合的size。
当指定类型的数组长度**大于**了集合的size，就**不会创建新数组**。而是**使用传递进来的数组**。
所以**创建一个刚刚好的数组最优**。

**2,为什么要将集合变数组？**
为了**限定对元素的操作**。**不**需要进行**增删**了。

```
public class CollectionToArray {
	public static void main(String[] args) {
		ArrayList<String> al = new ArrayList<String>();
		al.add("abc1");
		al.add("abc2");
		al.add("abc3");
		String[] arr = al.toArray(new String[al.size()]);
									//[abc1, abc2, abc3]
		// String[] arr = al.toArray(new String[5]);
									//[abc1, abc2, abc3, null, null]
		// String[] arr = al.toArray(new String[0]);
									//[abc1, abc2, abc3]
		System.out.println(Arrays.toString(arr));
	}
}
```








# 迭代器Iterator
Iterator，也称为迭代器，也是Java集合框架的成员，主要用于遍历（即迭代访问）Collection集合中的元素，是取出集合中元素的一种方式。（如同抓娃娃游戏机中的夹子。）
迭代器是取出方式，会直接访问集合中的元素。所以将**迭代器通过内部类的形式来进行描述**。
通过**容器的iterator()方法获取**该内部类的对象。
因为**Collection中有iterator()方法**，所以每一个子类集合对象都具备迭代器。

## 主要方法：
boolean **hasNext**():是否有下一个元素。
Object **next**():返回集合里下一个元素。
void **remove**();删除集合里上一次next方法返回的元素。
![](http://img.blog.csdn.net/20171222155206746?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

## 用法： 

```
        for (Iterator iter = iterator(); iter.hasNext(); ) {//boolean hasNext():是否有下一个元素。
            System.out.println(iter.next());//Object next():返回集合里下一个元素。
        }

        Iterator iter = l.iterator();
        while (iter.hasNext()) {
            System.out.println(iter.next());
        }
```
### 实例
```
public class TestIterator {
    public static void main(String[] args) {        
        Collection books = new HashSet();//创建一个集合 
        books.add("轻量级J2EE企业应用实战");
        books.add("Struts2权威指南");
        books.add("基于J2EE的Ajax宝典");

        Iterator it = books.iterator();//获取books集合对应的迭代器 
        while (it.hasNext()) {//是否有下一个元素。
            String book = (String) it.next();//未使用泛型，需要强制转换
            System.out.println(book);

            if (book.equals("Struts2权威指南")) {
                it.remove();//删除集合里上一次next方法返回的元素。
                //books.remove(book);//使用Iterator迭代过程中，不可修改集合元素,此代码引发异常
            }
            book = "测试字符串";//对book变量赋值，不会改变集合元素本身 
        }
        System.out.println(books);
    }
}
```
程序运行结果：
```xml
Struts2权威指南 
基于J2EE的Ajax宝典 
轻量级J2EE企业应用实战 
[基于J2EE的Ajax宝典, 轻量级J2EE企业应用实战]
```


## 迭代器内部实现
 ![](http://img.blog.csdn.net/20171222151020682?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


## 迭代注意事项

 - 迭代器**在Collcection接口中**是**通用**的，它**替代**了Vector类中的**Enumeration**(枚举)。
 - 迭代器的**next方法是自动向下取元素**，要避免出现NoSuchElementException。
 - 迭代器的**next方法返回值类型是Object**，所以要记得类型转换。
 - 在迭代时，不可以通过集合对象的方法操作集合中的元素。（会发生ConcurrentModificationException异常）在迭代器时，只能用迭代器的方法操作元素。

## 列表迭代器ListIterator
**List集合特有**的迭代器。ListIterator是Iterator的子接口。

 - 在迭代器时，只能用迭代器的方法操作元素，可是Iterator方法是有限的，只能对元素进行判断，取出，删除的操作，如果想要其他的操作如添加，修改等，就需要使用其子接口ListIterator。
 - ListIterator接口只能通过**List集合的listIterator方法获取**。
```java
	public static void method() {
		ArrayList al = new ArrayList();
		// 添加元素
		al.add("java01");
		al.add("java02");
		al.add("java03");
		al.add("java04");
		al.add("java05");
		System.out.println("原集合：" + al);

		al.add(4, "java09");// 在指定位置添加元素。
		al.remove(3);// 删除指定位置的元素。
		al.set(2, "java007");// 修改元素。
		System.out.println("get(1):" + al.get(1));// 通过角标获取元素。
		System.out.println("修改后集合：" + al);

		// 取出List集合中元素的方式1：get
		System.out.print("取出List集合中元素的方式1：get：");
		for (int x = 0; x < al.size(); x++)// size长度
		{
			System.out.print(" " + al.get(x));
		}
		System.out.println();

		// 取出List集合中元素的方式2：迭代
		System.out.print("取出List集合中元素的方式2：迭代：");
		Iterator iter = al.iterator();// 迭代器
		while (iter.hasNext()) {
			System.out.print(iter.next());
		}
		System.out.println();

		// 通过indexOf获取对象的位置。
		System.out.println("java02的index=" + al.indexOf("java02"));
		List sub = al.subList(1, 3);
		System.out.println("subList(1, 3)=" + sub);

		// 在迭代过程中，准备添加或者删除元素。
		System.out.println("迭代");
		Iterator it = al.iterator();
		while (it.hasNext()) {
			Object obj = it.next();
			if (obj.equals("java02")) {
				// al.add("java008");
				//对同一组元素(al)既用集合方法(al.add)，又用迭代器方法(it.remove)，可能引发”并发修改异常”,应如下用ListIterator
				it.remove();// 删除集合里上一次next方法返回的元素
			}
			System.out.println("obj=" + obj);// 但java02元素还在被obj引用，所以把java02打印了
		}
		System.out.println(al);

		// Iterator子类ListIterator列表迭代器
		ListIterator li = al.listIterator();
		System.out.println("hasNext():" + li.hasNext());
		System.out.println("hasPrevious():" + li.hasPrevious());
		System.out.println(al);

		while (li.hasNext())// 正向遍历
		{
			Object obj = li.next();
			if (obj.equals("java01")) {
				li.add("java009");
				// li.set("java006");
			}
		}
		System.out.println("hasNext():" + li.hasNext());
		System.out.println("hasPrevious():" + li.hasPrevious());
		System.out.println(al);

		while (li.hasPrevious())// 逆向遍历
		{
			Object pre = li.previous();
			if (pre.equals("java01")) {
				li.remove();
			}
			System.out.println("pre=" + pre);
		}
		System.out.println("hasNext():" + li.hasNext());
		System.out.println("hasPrevious():" + li.hasPrevious());
		System.out.println(al);
	}
```
运行结果
```xml
原集合：[java01, java02, java03, java04, java05]
get(1):java02
修改后集合：[java01, java02, java007, java09, java05]
取出List集合中元素的方式1：get： java01 java02 java007 java09 java05
取出List集合中元素的方式2：迭代：java01java02java007java09java05
java02的index=1
subList(1, 3)=[java02, java007]
迭代
obj=java01
obj=java02
obj=java007
obj=java09
obj=java05
[java01, java007, java09, java05]
hasNext():true
hasPrevious():false
[java01, java007, java09, java05]
hasNext():false
hasPrevious():true
[java01, java009, java007, java09, java05]
pre=java05
pre=java09
pre=java007
pre=java009
pre=java01
hasNext():true
hasPrevious():false
[java009, java007, java09, java05]
```


## 枚举Enumeration
就是**Vector特有**的取出方式。
枚举和迭代是一样的。因为枚举的名称以及方法的名称都过长。所以**被迭代器取代**了。

```java
		Vector v = new Vector();
		v.add("java01");
		v.add("java02");
		v.add("java03");
		v.add("java04");
		Enumeration en = v.elements();
		while(en.hasMoreElements())
		{
			System.out.println(en.nextElement());
		}
```

# for each循环
Collection在**JDK1.5后**出现的父接口Iterable，就是提供了这个for语句。

**格式：**
for(数据类型 变量名 **:** 被遍历的集合(**Collection**)或者**数组**)
{
	执行语句；
}


**简化了对数组、集合的遍历**。
只能获取集合元素。但是**不能对集合进行操作**。

**迭代器**除了遍历，**还可**以进行**remove**集合中元素的**动作**。如果是用**ListIterator**，还可以在**遍历过程中**对集合进行**增删改查**的动作。

**传统for和for each循环有什么区别呢？**
**for each循环有局限性**，必须**有被遍历的目标**。

建议在**遍历数组**的时候，还是希望是**用传统for**，因为传统for**可**以**定义脚标**。

```
class ForEachDemo 
{
	public static void main(String[] args) 
	{
		ArrayList<String> al = new ArrayList<String>();
		//ArrayList al = new ArrayList ();
		al.add("abc1");
		al.add("abc2");
		al.add("abc3");
		System.out.println(al);// [abc1, abc2, abc3]
		for(String s : al)
		//for(Object s : al)
		//上面没用泛型String限定的话，此处用Object，但不安全
		{
			//s = "kk";//改变了s，但对al的原数据无影响
			System.out.println(s);
		}
		/*Iterator<String> it = al.iterator();//上面for语句相当于是迭代的简化形式
		while(it.hasNext())
		{
			System.out.println(it.next());
		}	*/
		int[] arr = {3,5,1};
		System.out.println(arr);// [I@659e0bfd
		for(int i : arr)
		{ 
			System.out.println("i:"+i);
		}
		/*for(int x=0; x<arr.length; x++)
		{//上面for语句相当于此for循环简化形式
			System.out.println(arr[x]);
		}	*/
		HashMap<Integer,String> hm = new HashMap<Integer,String>();
		hm.put(1,"a");
		hm.put(2,"b");
		hm.put(3,"c");
		System.out.println(hm);// {1=a, 2=b, 3=c}
		Set<Integer> keySet = hm.keySet();
		for(Integer i : keySet)
		{
			System.out.println(i+":"+hm.get(i));
		}
//		Set<Map.Entry<Integer,String>> entrySet = hm.entrySet();
//		for(Map.Entry<Integer,String> me : entrySet)
		for(Map.Entry<Integer,String> me : hm.entrySet())
								//上两句合并为此一句
		{
			System.out.println(me.getKey()+"------"+me.getValue());
		}
	}
}
```

运行结果：

```
[abc1, abc2, abc3]
abc1
abc2
abc3
[I@659e0bfd
i:3
i:5
i:1
{1=a, 2=b, 3=c}
1:a
2:b
3:c
1------a
2------b
3------c 
```

# 方法的可变参数
**JDK1.5**版本出现的新特性。

使用时注意：**可变参数**一定要定义在参数列表**最后面**。

**格式：**
返回值类型 函数名(参数类型… 形式参数)
{
执行语句；
}

其实接收的是一个数组，可以指定实际参数个数。

```
class  ParamMethodDemo {
	public static void main(String[]args)
	{
		show1(1,2); //1,2
		show1(1,2,3);//多方法重载实现，麻烦。1,2,3
		int[]arr={2,5};
		show2(arr);//2 5
		int[]arr1={2,4,5};
		show2(arr1);//虽然少定义了多个方法。但是每次都要定义一个数组作为实际参数。
		System.out.println();
		show(0,6,7);// 067
		show("w",0,6);// w2
		show("s",0,6,7);// s3
	}
	public static void show(int...arr) //可变参数
	{
		for (int a:arr )
		{
			System.out.print(a);
		}
	}
	public static void show(String s,int...arr)
	{					//可变参数一定要定义在参数列表最后面。
		System.out.println(s+arr.length);
	}
/*	可变参数。其实就是下面数组参数的简写形式。
	不用每一次都手动的建立数组对象。只要将要操作的元素作为参数传递即可。
	隐式将这些参数封装成了数组。		*/
	public static void show2(int[]arr)
	{
		for (int x=0; x<arr.length; x++)
		{
			System.out.print(arr[x]+" ");
		}
	}
	public static void show1(int a,int b)
	{
		System.out.println(a+","+b);
	}
	public static void show1(int a,int b,int c)
	{
		System.out.println(a+","+b+","+c);
	}
}
```







引用：
[Java集合框架的知识总结](https://www.cnblogs.com/zhxxcq/archive/2012/03/11/2389611.html)

