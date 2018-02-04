

# 【JAVA 线程】


# JAVA 线程基础
**进程**：是一个正在执行中的程序。每一个进程执行都有一个**执行顺序**。该顺序是一个**执行路径**，或者叫一个**控制单元**。

**线程**：就是进程中的一个独立的**控制单元**。线程在控制着进程的执行。**一个进程中至少有一个线程**。（如迅雷多线程下载）

**Java VM**启动的时候会有一个**进程java.exe.**
该进程中**至少一个线程负责java程序的执行**，而且这个线程运行的**代码存在于main方法中**，该线程称之为**主线程**。

扩展：其实更细节说明jvm，jvm启动不止一个线程，还有负责垃圾回收机制的线程。（主线程在执行其它对象时，无用对象在被垃圾回收）
 多线程存在的意义。（一个线程执行过程中产生垃圾，另一个在回收垃圾）
 线程的创建方式
 多线程的特性

## 程序 VS 进程 VS 线程
### 程序 VS 进程
**程序:** 一段静态的代码，一组指令的有序集合，它本身没有任何运行的含义，它只是一个静态的实体，是应用软件执行的蓝本。

   **进程**: 是程序的一次动态执行，它对应着从代码加载，执行至执行完毕的一个完整的过程，是一个动态的实体，它有自己的生命周期。它因创建而产生，因调度而运行，因等待资源或事件而被处于等待状态，因完成任务而被撤消。反映了一个程序在一定的数据 集上运行的全部动态过程。通过进程控制块(PCB)唯一的标识某个进程。同时进程占据着相应的资源（例如包括cpu的使用 ，轮转时间以及一些其它设备的权限）。是系统进行资源分配和调度的一个独立单位。

#### 程序和进程之间的主要区别：

| \\     | 状态   | 是否具有资源 | 是否有唯一标识 | 是否具有并发性 |
| ------ | ---- | ------ | ------- | ------- |
| **程序** | 静态   | 无      | 无       | 无       |
| **进程** | 动态   | 有      | 有       | 有       |

#### 进程的基本状态:

1、**就绪**（Ready）状态
当进程已分配到除CPU以外的所有必要资源后，**只要**再**获得CPU，便可立即执行**，进程这时的状态就称为就绪状态。在一个系统中处于就绪状态的进程可能有**多个**，通常将他们排成一个队列，称为**就绪队列**。

2、**执行**状态
进程已获得CPU，其程序正在执行。在**单处理机系统中，只有一个进程处于执行状态；再多处理机系统中，则有多个进程处于执行状态**。

3、**阻塞**状态
正在执行的进程由于发生某事件而暂时无法继续执行时，便**放弃处理机而处于暂停状态**，亦即程序的执行受到阻塞，把这种暂停状态称为阻塞状态，有时也称为等待状态或封锁状态。

**三种进程之间的转换**图：
![](http://upload-images.jianshu.io/upload_images/9028834-a51b1bd7a499b7b7.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 线程
**线程: **可以理解为**进程的多条执行线索，每条线索又对应着各自独立的生命周期**。**线程是进程的一个实体,是CPU调度和分派的基本单位**,它是比进程更小的能独立运行的基本单位。**一个线程可以创建和撤销另一个线程，同一个进程中的多个线程之间可以并发执行**。


#### Java中的线程要经历4个过程
1. **创建**
  创建一个Java线程常见的有两种方式:
  继承Thread类和实现Runnable接口这两种方式。

2. **执行**
    **线程创建后仅仅占有了内存资源，在JVM管理的线程中还没有该线程**，该线程必须调用**start**方法**通知JVM**，这样JVM就会知道又有一个新的线程排队等候了。如果当前线程轮到了CPU的使用权限的话，当前线程就会继续执行。

3. **中断**
    a. JVM将CPU的使用权限从当前线程**切换到其它线程**，使本线程让出CPU的使用权限而处于中断状态。
    b. 线程在执行过程中调用了**sleep**方法，使当前线程处于休眠状态。
    c. 线程在执行的过程中调用**wait**方法
    d. 线程在使用cpu资源期间，执行了某个操作而进如**阻塞**状态。

4. **死亡**
   死亡的线程不在具有执行能力。线程死亡的原因有二：
    a. 线程正常**运行结束**而引起的死亡，即run方法执行完毕。
    b. 线程被提前**强制终止**。




### 
## 线程的创建方式
通过对api的查找，java已经提供了对线程这类事物的描述，即**Thread**类。

### 创建线程方式一：继承Thread类
1. 定义**类继承Thread**
2. 子类**覆盖**父类Thread中的**run**方法。
  目的：将**自定义线程运行的代码存储在run方法**。
3. 建立子类对象的同时线程也被创建。
4. **调用**线程的**start方法**。
  该方法两个作用：启动线程，**调用run**方法。通过调用start方法**开启线程**。
```
Demo d = new Demo();//创建好一个线程。
d.start();//开启线程并执行该线程的run方法。
// d.run();//仅仅是对象调用方法。而线程创建了，并没有运行。
```


**每次运行结果都不同**：
因为多个线程都在获取cpu的执行权。cpu执行到谁，谁就运行。明确一点，**在某一个时刻，只能有一个程序在运行**。(多核除外)

**cpu在做着快速的切换**，以达到看上去是同时运行的效果。
我们可以形象把多线程的运行形容为互相抢夺cpu的执行权（资源）。
这就是**多线程**的一个**特性**：**随机性**。谁抢到谁执行，至于执行多长，cpu说的算。

**为什么要覆盖run方法**：
Thread类用于描述线程。该类就定义了一个功能，用于存储线程要运行的代码，该存储功能就是run方法。
也就是说**Thread类中的run方法，用于存储线程要运行的代码**。

**线程**都有自己**默认名称**：Thread-编号（该编号从0开始）
　　　　　getName(): 获取线程名称。
**设置线程名称**：setName或者构造函数。

**static Thread.currentThread()**:**获取当前线程对象**。

#### 习题：创建两个线程，和主线程交替运行

```
public class Test extends Thread {
	//private String name;
	Test(String name) {
		super(name); //this.name = name;
	}

	public void run() {
		for (int x = 0; x < 60; x++) {
			System.out.println((Thread.currentThread() == this) + "..." + this.getName() + " run..." + x); //当前运行线程即为调用对象线程
		}
	}
}
```

```
public class ThreadTest {
	public static void main(String[] args) {
		Test t1 = new Test("one---");
		Test t2 = new Test("two+++");
		t1.start();//开启线程并执行该线程的run方法。
		t2.start();
		for (int x = 0; x < 60; x++) {
			System.out.println("main....." + x);
		}
	}
}
```

### 线程的四种状态
![](http://img.blog.csdn.net/20171225172628892?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
sleep方法需要指定睡眠时间，单位是毫秒。

### 创建线程方式二：实现Runnable接口
1. 定义类**实现Runnable**接口
2. **覆盖Runnable接口中的run方法**。将线程要运行的代码存放在该run方法中。
3. 通过**Thread类创建线程**
4. 将**Runnable接口的子类对象作为实际参数传递给Thread类的构造函数**。
  因为，自定义的run方法所属的对象是Runnable接口的子类对象。
  所以要让线程去指定指定对象的run方法，就必须明确该run方法所属对象。
>public **Thread**([Runnable](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/lang/Runnable.html) target)
>分配新的 Thread 对象。这种构造方法与 Thread(null, target, *gname* ) 具有相同的作用，其中的 *gname* 是一个新生成的名称。自动生成的名称的形式为 “Thread-”+ *n*，其中的 *n* 为整数。
>**参数：**target - 其 run 方法被调用的对象。
>另请参见：[Thread(ThreadGroup, Runnable, String)](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/lang/Thread.html#Thread(java.lang.ThreadGroup, java.lang.Runnable, java.lang.String))

5. 调**用Thread类的start方法开启线程**并调用Runnable接口子类的run方法。
6. ​
### 实现方式VS继承方式：
**实现方式好处**：**避免单继承的局限性**。在定义线程时，**建议使用实现方式**。

**两种方式区别：**
**继承Thread**：**线程代码**存放**Thread子类run方法**中。
**实现Runnable**：**线程代码**存在**接口的子类的run方法**。


## 线程安全问题
运行发现，可能会打印出0，-1，-2等错票。**多线程的运行出现了安全问题**。
**原因**：
　　多个线程**访问**出现**延迟**。
　　 线程**随机性**。
　　(当多条语句在操作同一个线程共享数据时，**一个线程**对多条语句**只执行了一部分**，还没有执行完，**另一个线程参与进来执行**。导致**共享数据的错误**。)
**注**：线程安全问题在理想状态下，不容易出现，但一旦出现对软件的影响是非常大的。
**解决办法**：对**多条操作共享数据的语句**，只能**让一个线程都执行完**。在执行**过程中，其他线程不可以参与执行**。
Java对于多线程的安全问题提供了专业的解决方式，即**同步代码块**。（synchronized）

## 同步（synchronized）
**格式：**
```
synchronized(对象) {
	需要被同步的代码 //运行到共享数据的代码
}
```

同步可以解决安全问题的根本原因就在那个对象上，该对象如同锁的功能。**持有锁的线程可以在同步中执行**。没有持有锁的线程即使获取cpu的执行权也进不去，因为没有获取锁。（eg.火车上的卫生间-有人就锁门，其它人进不来）

### 同步的特点
**同步的前提：**
1，必须要有**两个或者两个以上的线程**。
2，必须是**多个线程使用同一个锁**。
未满足这两个条件，不能称其为同步。
必须保证同步中只能有一个线程在运行。

**好处**：**解决**了多线程的**安全问题**
**弊端**：多个线程需要判断锁，较为**消耗资源**（当线程相当多时，因为每个线程都会去判断同步上的锁，这是很耗费资源的，无形中会降低程序的运行效率。）

#### 习题：简单的卖票程序，多个窗口同时买票。

```
public class Ticket implements Runnable//extends Thread
{
	private int tick = 100;
	Object obj = new Object();//通过Object创建锁对象

	public void run() {
		while (true) {
			synchronized (obj) {//同步，防止线程安全问题
				if (tick > 0) {
					try {
						Thread.sleep(10);
					} catch (Exception e) {
					}
					//在没Synchronized的情况下用sleep方法人为让线程停在这儿，验证是否会像下面这样出现安全问题
					//如没Synchronized，可能在tick=1时，多个线程判断完停在这儿，再轮到它们运行时导致共享数据tick运行结果出现负数（tick=1 tick=0 tick=-1…），即出现线程安全问题
					System.out.println(Thread.currentThread().getName() + "....sale : " + tick--);
				}
			}
		}
	}
}
```

```
public class TicketDemo {
	public static void main(String[] args) {
		Ticket t = new Ticket();
		Thread t1 = new Thread(t);//创建了一个线程；运行Ticket t里面的run方法
		Thread t2 = new Thread(t);//创建了一个线程；
		Thread t3 = new Thread(t);//创建了一个线程；
		Thread t4 = new Thread(t);//创建了一个线程；
		Thread t5 = new Thread(t);//创建了一个线程；
		t1.start();
		t2.start();
		t3.start();
		t4.start();
		t5.start();
		
		//如用下面方法1 extends Thread，四个窗口各卖100张票共400张票，比总票数100张多，不合理；故用上面方法2 implements Runnable
		/*
		Ticket t1 = new Ticket();
		//Ticket t2 = new Ticket();
		//Ticket t3 = new Ticket();
		//Ticket t4 = new Ticket();
		t1.start();
		t2.start();
		t3.start();
		t4.start();
		*/
	}
}
```

### 同步函数

**格式**： 在函数上加上synchronized修饰符即可。

**同步函数用的是哪一个锁**：
1. 函数需要被对象调用。那么函数都有一个所属对象引用，就是this，所以**同步函数使用的锁是this**。
2. **静态**的同步方法，使用的锁是**该方法所在类的字节码文件对象**：
  **类名.class**
  因为静态方法中不可以定义this。静态进内存时，内存中没有本类对象，但是一定有该类对应的字节码文件对象：类名.class

**如何找线程安全问题：**
1，明确哪些代码是多线程运行代码。
2，明确共享数据。
3，明确多线程运行代码中哪些语句是操作共享数据的。

#### 习题：银行存储
有两个储户分别存300元，每次存100，存3次。
目的：**该程序是否有安全问题，如果有，如何解决？**
```
public class Bank {
	private int sum;

	public synchronized void add(int n) {
		sum = sum + n;
		try {
			Thread.sleep(10);
		} catch (Exception e) {
		}
		System.out.println("sum=" + sum);
	}
}
```

```
public class Cus implements Runnable {
	private Bank b = new Bank();

	public void run() {
		for (int x = 0; x < 3; x++) {
			b.add(100);
		}
	}
}
```

```
public class BankDemo {
	public static void main(String[] args) {
		Cus c = new Cus();
		new Thread(c).start();
		new Thread(c).start();
	}
}
```
### 死锁
同步中嵌套同步，用到不同的锁。
#### 习题：写一个死锁程序

```
public class Test implements Runnable {
	private boolean flag;

	Test(boolean flag) {
		this.flag = flag;
	}

	public void run() {
		if (flag) {
			while (true) {
				synchronized (MyLock.locka) {
					System.out.println(Thread.currentThread().getName() + "...if locka ");
					synchronized (MyLock.lockb) {
						System.out.println(Thread.currentThread().getName() + "..if lockb");
					}
				}
			}
		} else {
			while (true) {
				synchronized (MyLock.lockb) {
					System.out.println(Thread.currentThread().getName() + "..else lockb");
					synchronized (MyLock.locka) {
						System.out.println(Thread.currentThread().getName() + ".....else locka");
					}
				}
			}
		}
	}
}

class MyLock {
	static Object locka = new Object();
	static Object lockb = new Object();
}
```

```
class DeadLockTest {
	public static void main(String[] args) {
		/*		Thread t1 = new Thread(new Test(true));
				Thread t2 = new Thread(new Test(false));
				t1.start();
				t2.start();  */
		new Thread(new Test(true)).start();//匿名对象，对上面简化
		new Thread(new Test(false)).start();
	}
}
```

### 
## 线程间通信
![](http://img.blog.csdn.net/20171225194801564?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

**线程间通讯：**
其实就是**多个线程在操作同一个资源**，但是操作的动作不同。

**wait(),notify(),notifyAll(),用来操作线程为什么定义在了Object类中？**
1. **wait(),notify(),notifyAll()**方法**都使用在同步中**，因为这些方法要对持有监视器(锁)的线程操作（而只有同步才具有锁）
2. 使用这些方法时**必须要标识所属的同步的锁**。（因为这些方法在操作同步中线程时，都必须要标识它们所操作线程共有的锁，**只有同一个锁上的被等待线程，可以被同一个锁上notify唤醒**。不可以对不同锁中的线程进行唤醒。也就是说，等待和唤醒必须是同一个锁。）
3. **锁可以是任意对象**，所以任意对象调用的方法一定定义Object类中。

**wait(),sleep()的区别：**
wait():释放cpu执行权，释放锁。
sleep():释放cpu执行权，不释放锁。

#### 习题：交替输入输出姓名性别（2线程）

```
public class Res {
	private String name;
	private String sex;
	private boolean flag = false;

	public synchronized void set(String name, String sex) {
		if (flag)
			try {
				this.wait(); //wait等待把线程放到线程池里
			} catch (Exception e) {
			}
		this.name = name;
		this.sex = sex;
		System.out.println("【SET】" + name + sex);
		flag = true;
		this.notify();//notify唤醒一个，一般最先唤醒最早wait进线程池的
						//notifyAll();唤醒全部
	}

	public synchronized void out() {
		if (!flag)
			try {
				this.wait();
			} catch (Exception e) {
			}
		System.out.println("【OUT】" + name + sex);
		flag = false;
		this.notify();
	}
}

class Input implements Runnable {
	private Res r;

	Input(Res r) {
		this.r = r;
	}

	public void run() {
		int x = 1;
		while (x < 6) {
			r.set("丽丽" + x, "女");
			x++;
		}
	}
}

class Output implements Runnable {
	private Res r;

	Output(Res r) {
		this.r = r;
	}

	public void run() {
		while (true) {
			r.out();
		}
	}
}
```

```
public class InputOutputDemo2 {
	public static void main(String[] args) {
		Res r = new Res();
		new Thread(new Input(r)).start();
		//用匿名对象简化代码，因为只调用该对象的一个方法--start方法--一次
		new Thread(new Output(r)).start();
		/*
		Input in = new Input(r);
		Output out = new Output(r);
		Thread t1 = new Thread(in);
		Thread t2 = new Thread(out);
		t1.start();
		t2.start();
		*/
	}
}
```
结果：
```
【SET】丽丽1女
【OUT】丽丽1女
【SET】丽丽2女
【OUT】丽丽2女
【SET】丽丽3女
【OUT】丽丽3女
【SET】丽丽4女
【OUT】丽丽4女
【SET】丽丽5女
【OUT】丽丽5女
```
#### 习题：交替生产消费商品（多线程，为商品编号）
对于**多个生产者和消费者**，为什么要**定义while判断标记**。
原因：让被唤醒的线程**再一次判断标记**。

为什么**定义notifyAll**：需要唤醒对方线程。只**用notify**，**容易**出现**只唤醒本方线程**的情况。导致程序中的所有线程都等待。

```
public class ProducerConsumerDemo {
	public static void main(String[] args) {
		Resource r = new Resource();
		new Thread(new Producer(r)).start();
		new Thread(new Producer(r)).start();
		new Thread(new Consumer(r)).start();
		new Thread(new Consumer(r)).start();
	}
}

class Resource {
	private String name;
	private int count = 1; //为商品编号
	private boolean flag = false;

	//  t1    t2
	public synchronized void set(String name) {
		while (flag) {
			//如果用if，线程判断完如停在这儿等再被唤醒就不再判断直接向下运行
			try {
				this.wait();
			} catch (Exception e) {
			} //t1(放弃资格)  t2(获取资格)
		}
		this.name = name + "--" + count++;
		System.out.println(Thread.currentThread().getName() + "...生产者.." + this.name);
		flag = true;
		this.notifyAll();
	}

	//  t3   t4  
	public synchronized void out() {
		while (!flag) {
			try {
				wait();
			} catch (Exception e) {
			} //t3(放弃资格) t4(放弃资格)
		}
		System.out.println(Thread.currentThread().getName() + "...消费者........." + this.name);
		flag = false;
		this.notifyAll();
	}
}

class Producer implements Runnable {
	private Resource res;

	Producer(Resource res) {
		this.res = res;
	}

	public void run() {
		while (true) {
			res.set("+商品+");
		}
	}
}

class Consumer implements Runnable {
	private Resource res;

	Consumer(Resource res) {
		this.res = res;
	}

	public void run() {
		while (true) {
			res.out();
		}
	}
}
```

![](http://img.blog.csdn.net/20171225202407301?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


## 多线程升级解决方案
JDK1.5 中提供了**多线程升级解决方案：**

将同步**Synchronized替换成**显式**Lock**操作。

将Object中的**wait，notify, notifyAll**，**替换成Condition**对象。该对象可以通过Lock锁进行获取。

**Lock** 实现提供了比使用 synchronized方法和语句可获得的更广泛的锁定操作。此实现允许更灵活的结构，可以具有差别很大的属性，**可以支持多个相关的Condition 对象**。

**Lock:替代了Synchronized**
　　　lock
　　　unlock
　　　newCondition()

**Condition：替代了Object wait notify notifyAll**
　　　await();
　　　signal();
　　　signalAll();
　　　
#### **习题：交替生产消费**（用Lock，实现本方只唤醒对方操作）
```
public class ProducerConsumerDemo2 {
	public static void main(String[] args) {
		Resource2 r = new Resource2();
		new Thread(new Producer2(r)).start();
		new Thread(new Producer2(r)).start();
		new Thread(new Consumer2(r)).start();
		new Thread(new Consumer2(r)).start();
	}

}

class Resource2 {
	private String name;
	private int count = 1;
	private boolean flag = false;
	//  t1    t2
	private Lock lock = new ReentrantLock();
	private Condition condition_pro = lock.newCondition();
	private Condition condition_con = lock.newCondition();

	//Lock可以支持多个相关的 Condition 对象
	public void set(String name) throws InterruptedException {
		lock.lock();
		try {
			while (flag)
				condition_pro.await();//t1,t2
			this.name = name + "--" + count++;
			System.out.println(Thread.currentThread().getName() + "...生产者.." + this.name);
			flag = true;
			condition_con.signal();//唤醒对应等待线程
		} finally {
			lock.unlock();//释放锁的动作一定要执行。
		}
	}

	//  t3   t4  
	public void out() throws InterruptedException {
		lock.lock();
		try {
			while (!flag)
				condition_con.await();
			System.out.println(Thread.currentThread().getName() + "...消费者........." + this.name);
			flag = false;
			condition_pro.signal();
		} finally {
			lock.unlock();
		}
	}
}

class Producer2 implements Runnable {
	private Resource2 res;

	Producer2(Resource2 res) {
		this.res = res;
	}

	public void run() {
		while (true) {
			try {
				res.set("+商品+");
			} catch (InterruptedException e) {
			}
		}
	}
}

class Consumer2 implements Runnable {
	private Resource2 res;

	Consumer2(Resource2 res) {
		this.res = res;
	}

	public void run() {
		while (true) {
			try {
				res.out();
			} catch (InterruptedException e) {
			}
		}
	}
}
```


## 停止线程

1. 定义**循环结束**标记
  因为线程运行代码一般都是循环，只要控制了循环即可，让run方法结束，也就是线程结束。
2. 使用**interrupt（中断）**方法。
  该方法是结束线程的冻结状态，使线程回到运行状态中来。（当线程处于冻结状态，就不会读取到标记，那么线程就不会结束。当没有指定的方式让冻结的线程恢复到运行状态时，这时需要**对冻结进行清除**。强制让线程恢复到运行状态中来，这样就可以操作标记让线程结束。）

>   注：stop方法已经过时不再使用。
>   (有bug不再使用，但老程序里面有stop所以没取消)

#### 习题：停止线程（分别用循环结束标记、interrupt、setDaemon）

```
public class StopThreadDemo {
	public static void main(String[] args) {
		StopThread st = new StopThread();
		Thread t1 = new Thread(st);
		Thread t2 = new Thread(st);
		
		//守护线程，其所守护的线程结束，它本身也结束（main结束了，即使t1、t2还在wait，也随main结束而结束）
		t1.setDaemon(true);
		t2.setDaemon(true);
		
		t1.start();
		t2.start();
		int num = 0;
		while (true) {
			if (num++ == 60) {
				st.changeFlag();//num自增到60执行循环结束标记
				t1.interrupt();//对冻结进行清除
				t2.interrupt();
				break;
			}
			System.out.println(Thread.currentThread().getName() + "......." + num);
		}
		System.out.println("over");
	}

}

class StopThread implements Runnable {
	private boolean flag = true;

	public void run() {
		while (flag) {
			// try{wait();}catch (InterruptedException e) { System.out.println(Thread.currentThread().getName()+"Exception");} //线程冻结，用interrupt唤醒
			System.out.println(Thread.currentThread().getName() + "....run");
		}
	}

	public void changeFlag()//执行循环结束标记
	{
		flag = false;
	}
}
```


## 线程类的其他方法
### setDaemon(boolean on)
守护线程，其所守护的线程结束，它本身也结束
public final void **setDaemon**(boolean on)
将该线程标记为守护线程或用户线程。当正在运行的线程都是守护线程时，Java
虚拟机退出。
该方法必须在**启动线程前调用**。
该方法首先调用该线程的 checkAccess 方法，且不带任何参数。这可能抛出 SecurityException（在当前线程中）。
**参数：**
on - 如果为 true，则将该线程标记为守护线程。
**抛出：**
[IllegalThreadStateException](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/lang/IllegalThreadStateException.html) -如果该线程处于活动状态。
[SecurityException](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/lang/SecurityException.html) -如果当前线程无法修改该线程。
**另请参见：**[isDaemon()](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/lang/Thread.html#isDaemon()), [checkAccess()](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/lang/Thread.html#checkAccess())
### join()
当A线程执行到了**B线程的.join()**方法时，A就会等待，**等B线程都执行完，A才会执行**。join可以用来临时加入线程执行。
public final void **join**()
**抛出：**
[InterruptedException](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/lang/InterruptedException.html) -如果任何线程中断了当前线程。当抛出该异常时，当前线程的 *中断状态* 被清除。
### setPriority(int newPriority)
更改**线程的优先级**。总共1-10，**默认优先级是5。MAX_PRIORITY=10、MIN_PRIORITY=1、NORM_PRIORITY=5。**
public final void **setPriority**(int newPriority)
首先调用线程的 checkAccess 方法，且不带任何参数。这可能抛出 SecurityException。
在其他情况下，线程优先级被设定为指定的 newPriority 和该线程的线程组的最大允许优先级相比较小的一个。
**参数：**
newPriority - 要为线程设定的优先级
**抛出：**
[IllegalArgumentException](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/lang/IllegalArgumentException.html) -如果优先级不在 MIN_PRIORITY 到 MAX_PRIORITY 范围内。
[SecurityException](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/lang/SecurityException.html) -如果当前线程无法修改该线程。
**另请参见：**
[getPriority()](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/lang/Thread.html#getPriority()), [checkAccess()](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/lang/Thread.html#checkAccess()), [getThreadGroup()](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/lang/Thread.html#getThreadGroup()), [MAX_PRIORITY](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/lang/Thread.html#MAX_PRIORITY), [MIN_PRIORITY](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/lang/Thread.html#MIN_PRIORITY), [ThreadGroup.getMaxPriority()](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/lang/ThreadGroup.html#getMaxPriority())
| **字段摘要**   |                                          |
| ---------- | ---------------------------------------- |
| static int | [MAX_PRIORITY](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/lang/Thread.html#MAX_PRIORITY) |
|            | 线程可以具有的最高优先级。                            |
| static int | [MIN_PRIORITY](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/lang/Thread.html#MIN_PRIORITY) |
|            | 线程可以具有的最低优先级。                            |
| static int | [NORM_PRIORITY](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/lang/Thread.html#NORM_PRIORITY) |
|            | 分配给线程的默认优先级。                             |

### yield()
**暂停当前正在执行的线程对象**，并执行其他线程。
public static void **yield**()

### toString()
public [String](http://tool.oschina.net/uploads/apidocs/jdk-zh/java/lang/String.html) **toString**()
**返回**：该对象的字符串表示。
通常， toString 方法会返回一个**“以文本方式表示”此对象的字符串**。结果应是一个简明但易于读懂的信息表达式。建议所有子类都重写此方法。
Object 类的 toString 方法返回一个字符串，该字符串由类名（对象是该类的一个实例）、at标记符“@”和此对象哈希码的无符号十六进制表示组成。换句话说，该方法返回一个字符串，它的值等于：getClass().getName() + '@' + Integer.toHexString(hashCode())

#### 习题：运用toString、yield、setPriority 、join

```
public class JoinDemo {
	public static void main(String[] args) throws Exception {//join需要抛出异常
		Demo d = new Demo();
		Thread t1 = new Thread(d);
		Thread t2 = new Thread(d);
		t1.start();
		t1.setPriority(Thread.MAX_PRIORITY);//优先级 MAX NOR MIN
		t2.start();
		t1.join();//t2被激活后t1插入main，但t1插的是main，所以main会等t1结束再执行，但t2会正常跟t1抢执行权
		for (int x = 0; x < 80; x++) {
			System.out.println("main....."+x);
		}
		System.out.println("over");
	}

}

class Demo implements Runnable {
	public void run() {
		for (int x = 0; x < 70; x++) {
			System.out.println(Thread.currentThread().toString() + "....." + x);
			Thread.yield();//暂停当前
		}
	}
}
```

### 

## 线程总结：

### 线程间通信
等待/唤醒机制。也就是常见的**生产者消费者问题**。
**1. 当多个生产者消费者出现时，**
需要让获取执行权的线程判断标记。
通过while完成。
**2. 需要将对方的线程唤醒。**
仅仅用notify，是不可以的。因为有可能出现只唤醒本方。
有可能会导致，所有线程都等待。
所以可以通过notifyAll的形式来完成 。

这个程序有一个bug。就是每次notifyAll。都会唤醒本方。
可不可以**只唤醒对方**呢？ JDK1.5版本提供了一些新的对象，优化了等待唤醒机制。
1. **将synchronized 替换成了Lock接口。**
  将隐式锁，升级成了显式锁。
  **Lock**
  获取锁：lock();
  释放锁：unlock();注意：释放的动作一定要执行，所以**通常定义在finally中。**
  **获取Condition对象**：newCondition();
2. **将Object中的wait，notify，notifyAll方法都替换成了Condition的await，signal，signalAll。**
  和以前不同是：
  一个同步代码块具备一个锁，该所以具备自己的独立wait和notify方法。
  现在是**将wait，notify等方法，封装进一个特有的对象Condition，而一个Lock锁上可以有多个Condition对象**。

```java
Lock lock = new ReentrantLock();
Condition conA = lock.newCondition();
Condition conB = lock.newCondition();
con.await();//生产，，消费
con.signal();生产
set() {
	if(flag)
		conA.await();//生产者，
	code......;
	flag = true;
	conB.signal();
}
out() {
	if(!flag)
		conB.await();//消费者
	code....;
	flag = false;
	conA.signal();
}
```


**wait和sleep的区别：**
**wait**:释放cpu执行权，**释放同步中锁**。
**sleep**:释放cpu执行权，**不释放同步中锁**。

```
synchronized(锁) {
	wait();
}
```

### 停止线程：
stop过时。
原理：run方法结束。run方法中通常定义循环，指定控制住循环线程即可结束。
1，定义**结束标记**。
2，当线程处于了冻结状态，没有执行标记，程序一样无法结束。
这时可以循环，正常退出冻结状态，或者强制结束冻结状态。
**强制结束冻结状态：interrupt()**;目的是线程强制从冻结状态恢复到运行状态。
但是会发生InterruptedException异常。

### 线程中一些常见方法：
**setDaemon(boolean)**:将线程标记为**后台线程**，后台线程和前台线程一样，开启，一样抢执行权运行，只有在结束时，有区别，当前台线程都运行结束后，后台线程会自动结束。
**join()**:等待该线程结束。当A线程执行到了B的.join方法时，A就会处于冻结状态。A什么时候运行呢？当B运行结束后，A就会具备运行资格，继续运行。
加入线程，可以完成对某个线程的临时加入执行。

### 线程状态转换

![](http://upload-images.jianshu.io/upload_images/9028834-4343dd6e7b8009f6.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240 "2")

各种状态一目了然，值得一提的是”**blocked**”这个状态：
线程在Running的过程中可能会遇到阻塞(Blocked)情况
1.  调用join()和sleep()方法，sleep()时间结束或被打断，join()中断,IO完成都会回到Runnable状态，等待JVM的调度。
2.  调用wait()，使该线程处于等待池(wait blocked pool),直到notify()/notifyAll()，线程被唤醒被放到锁定池(lock blocked pool )，释放同步锁使线程回到可运行状态（Runnable）
3.  对Running状态的线程加同步锁(Synchronized)使其进入(lock blocked pool ),同步锁被释放进入可运行状态(Runnable)。



### 多线程重点：
1，多线程的**创建的两种方式**，以及区别。
2， **同步**的特点。
同步的好处：
同步的弊端：
同步的前提：
同步的表现形式以及区别。
特例：static同步函数锁是哪一个。
**死锁代码**要求写的出来。
3，**线程间通信**，看以上总结。
4，**wait和sleep， yield**：临时暂停，可以让线程是释放执行权。

# JAVA 线程池
## 简述线程池

**线程池是如何工作的：一系列任务出现后，根据自己的线程池安排任务进行。**

如图：
![](http://upload-images.jianshu.io/upload_images/9028834-53947e7e91af3736.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 线程池的好处

1.  重用线程池中的线程，避免因为线程的创建和销毁所带来的性能开销。
2.  能有效控制线程池的最大并发数，避免大量的线程之间因互相抢占系统资源而导致的阻塞现象。
3.  能对线程进行简单的管理。并提供定时执行以及指定间隔循环执行等功能。



## ThreadPoolExecutor
JAVA语言为我们提供了两种基础线程池的选择：**ScheduledThreadPoolExecutor**和**ThreadPoolExecutor**。它们都实现了ExecutorService接口（注意，ExecutorService接口本身和“线程池”并没有直接关系，它的定义更接近“执行器”，而“使用线程管理的方式进行实现”只是其中的一种实现方式）。这篇文章中，我们主要围绕ThreadPoolExecutor类进行讲解。

**线程池的具体实现为ThreadPoolExeutor,其接口为Executor**。

### ThreadPoolExecutor逻辑结构
**ThreadPoolExecutor**提供了一系列的参数用于配置线程池。

```java
public ThreadPoolExecutor(int corePoolSize, int maximumPoolSize, long keepAliveTime,
                              TimeUnit unit, BlockingQueue<Runnable> workQueue,
                              ThreadFactory threadFactory) { 
    this(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue, threadFactory, defaultHandler);
}
```

**参数含义**如下：
1.  **corePoolSize** : 线程池**核心线程数**，默认情况下，**核心线程**会在线程池中**一直存活**，即使他们处于闲置状态。
  其有一个**allowCoreThreadTimeOut**属性如果设置为true,那么核心线程池会有**超时**策略。超时的时长为第三个参数  **keepAliveTime** 。如果**超时**，核心线程会被**终结**。
2.  **maxmumPoolSize**: 线程池所能容忍的**最大线程数**，当活动线程数**达到**这个数值后，**后续**的新任务会被**阻塞**。
3.  **keepAliveTime**: 非核心线程闲置时的**超时时长**，**超过**这个时长非核心线程会**被回收**。这个参数如同第一个参数，如果设置相关属性后也会作用于核心线程。
4.  **unit**: 指定**keepAliveTime**的参数**时间单位**。这是一个枚举，常用的有MILLISECONDS（毫秒）、SECONDS（秒）等
5.  **workQueue**: 线程池的**任务队列**，通过execute()方法（执行方法）提交的Runable对象会存储在这个参数中。
6.  **threadFactory**: 线程工厂，为线程池提供创建新线程的功能。

![[]](http://upload-images.jianshu.io/upload_images/9028834-c029b22aa584c9f0?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
一定要注意一个概念，即**存在于线程池中容器的一定是Thread对象，而不是你要求运行的任务**（所以叫线程池而不叫任务池也不叫对象池）；你要求运行的**任务将被线程池分配给某一个空闲的Thread运行**。 

从上图中，我们可以看到构成线程池的几个重要元素： 
● **等待队列**：顾名思义，就是你调用线程池对象的submit()方法或者execute()方法，要求线程池运行的任务（这些任务必须实现Runnable接口或者Callable接口）。但是出于某些原因线程池并没有马上运行这些任务，而是送入一个队列等待执行。

● **核心线程**：线程池主要用于执行任务的是“核心线程”，“核心线程”的数量是你创建线程时所设置的corePoolSize参数决定的。如果不进行特别的设定，线程池中始终会保持corePoolSize数量的线程数（不包括创建阶段）。

● **非核心线程**：一旦任务数量过多（由等待队列的特性决定），线程池将创建“非核心线程”临时帮助运行任务。你设置的大于corePoolSize参数小于maximumPoolSize参数的部分，就是线程池可以临时创建的“非核心线程”的最大数量。这种情况下如果某个线程没有运行任何任务，在等待keepAliveTime时间后，这个线程将会被销毁，直到线程池的线程数量重新达到corePoolSize。

● maximumPoolSize参数也是当前线程池允许创建的最大线程数量。那么如果设置的corePoolSize参数和设置的maximumPoolSize参数一致时，线程池在任何情况下都不会回收空闲线程。keepAliveTime和timeUnit也就失去了意义。

● keepAliveTime参数和timeUnit参数也是配合使用的。keepAliveTime参数指明等待时间的量化值，timeUnit指明量化值单位。例如keepAliveTime=1，timeUnit为TimeUnit.MINUTES，代表空闲线程的回收阀值为1分钟。

### ThreadPoolExecutor工作方式

线程池是怎样处理某一个运行任务的
1. 首先可以通过线程池提供的submit()方法或者execute()方法，要求线程池执行某个任务。线程池收到这个要求执行的任务后，会有几种处理情况： 
  1.1、如果当前线程池中运行的线程数量还**没有达到corePoolSize**大小时，线程池会**创建**一个**新**的**线程**运行你的任务，无论之前已经创建的线程是否处于空闲状态。 
  1.2、如果当前线程池中运行的线程数量已经**达到**设置的**corePoolSize**大小，线程池会把你的这个任务**加入到等待队列**（workQueue）中。直到某一个的线程空闲了，线程池会根据设置的等待队列规则，从队列中取出一个新的任务执行。 
  1.3、如果根据队列规则，这个任务无法加入等待队列。（当**workQueue已满，且maximumPoolSize>corePoolSize**时）这时线程池就会**创建一个“非核心线程”**直接运行这个任务。注意，如果这种情况下任务执行成功，那么当前线程池中的线程数量一定大于corePoolSize。 
  1.4、如果这个任务，无法被“核心线程”直接执行，又无法加入等待队列，又无法创建“非核心线程”直接执行，且你没有为线程池设置RejectedExecutionHandler。（当提交**任务数超过maximumPoolSize**时，新提交任务由**RejectedExecutionHandler**处理）这时线程池会抛出RejectedExecutionException异常，即线程池拒绝接受这个任务。（实际上抛出RejectedExecutionException异常的操作，是ThreadPoolExecutor线程池中一个默认的RejectedExecutionHandler实现：AbortPolicy，这在后文会提到） 
2. 一旦线程池中某个线程完成了任务的执行，它就会试图到任务等待队列中拿去下一个等待任务（所有的等待任务都实现了BlockingQueue接口，按照接口字面上的理解，这是一个可阻塞的队列接口），它会调用等待队列的poll()方法，并停留在哪里。 
3. 当线程池中的线程超过你设置的corePoolSize参数，说明当前线程池中有所谓的“非核心线程”。那么当某个线程处理完任务后，如果等待keepAliveTime时间后仍然没有新的任务分配给它，那么这个线程将会被回收。线程池回收线程时，对所谓的“核心线程”和“非核心线程”是一视同仁的，直到线程池中线程的数量等于你设置的corePoolSize参数时，回收过程才会停止。（当线程池中超过corePoolSize线程，**空闲时间达到keepAliveTime时，关闭空闲线程**）
  当**设置allowCoreThreadTimeOut(true)**时，线程池中**corePoolSize线程空闲时间达到keepAliveTime也将关闭**。


### ThreadPoolExecutor不常用的设置

在ThreadPoolExecutor线程池中，有一些不常用的甚至不需要的设置

#### allowCoreThreadTimeOut：

线程池回收线程只会发生在当前线程池中线程数量大于corePoolSize参数的时候；当线程池中线程数量小于等于corePoolSize参数的时候，回收过程就会停止。 
allowCoreThreadTimeOut设置项可以要求线程池：将包括“核心线程”在内的，没有任务分配的任何线程，在等待keepAliveTime时间后全部进行回收：

```java
ThreadPoolExecutor poolExecutor = new ThreadPoolExecutor(5, 10, 1, TimeUnit.MINUTES, new ArrayBlockingQueue(1)); 
poolExecutor.allowCoreThreadTimeOut(true);
```

以下是设置前的效果： 
![[]](http://upload-images.jianshu.io/upload_images/9028834-faf474d98a52fa2b?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

以下是设置后的效果： 
![[]](http://upload-images.jianshu.io/upload_images/9028834-ba1a86075b48dbd2?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### prestartAllCoreThreads

前文我们还讨论到，当线程池中的线程还没有达到你设置的corePoolSize参数值的时候，如果有新的任务到来，线程池将创建新的线程运行这个任务，无论之前已经创建的线程是否处于空闲状态。这个描述可以用下面的示意图表示出来： 
![[]](http://upload-images.jianshu.io/upload_images/9028834-670b9d195e0845eb?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

prestartAllCoreThreads设置项，可以**在线程池创建，但还没有接收到任何任务的情况下，先行创建符合corePoolSize参数值的线程数**：

```java
ThreadPoolExecutor poolExecutor =new ThreadPoolExecutor(5,10,1, TimeUnit.MINUTES, new ArrayBlockingQueue<Runnable>(1));
poolExecutor.prestartAllCoreThreads();
```


上面给出的最简单的ThreadPoolExecutor线程池的使用方式中，我们只采用了ThreadPoolExecutor最简单的一个构造函数：
```java
public ThreadPoolExecutor(int corePoolSize, int maximumPoolSize, long keepAliveTime, TimeUnit unit, BlockingQueue workQueue)
```

实际上ThreadPoolExecutor线程池有很多种构造函数，其中最复杂的一种构造函数是：
```java
public ThreadPoolExecutor(int corePoolSize, int maximumPoolSize, long keepAliveTime, TimeUnit unit, BlockingQueue workQueue, ThreadFactory threadFactory, RejectedExecutionHandler handler)
```

### ThreadFactory的使用

线程池最主要的一项工作，就是在满足某些条件的情况下创建线程。而在ThreadPoolExecutor线程池中，**创建线程的工作交给ThreadFactory来完成**。要使用线程池，就必须要指定ThreadFactory。 
类似于上文中，如果我们使用的构造函数时并没有指定使用的ThreadFactory，这个时候ThreadPoolExecutor会使用一个默认的ThreadFactory：**DefaultThreadFactory**。（这个类在Executors工具类中）

当然，在某些特殊业务场景下，还可以使用一个**自定义**的**ThreadFactory**线程工厂，如下代码片段：

```java
import java.util.concurrent.ThreadFactory;
/* 测试自定义的一个线程工厂 */
public class TestThreadFactory implements ThreadFactory {
    @Override
    public Thread newThread(Runnable r) {
        return new Thread(r);
    }
}
```

### 线程池的等待队列

在使用ThreadPoolExecutor线程池的时候，需要指定一个实现了BlockingQueue接口的任务等待队列。在ThreadPoolExecutor线程池的API文档中，一共推荐了三种等待队列，它们是：SynchronousQueue、LinkedBlockingQueue和ArrayBlockingQueue；

#### 队列和栈

● **队列**：是一种特殊的线性结构，允许在线性结构的前端进行删除/读取操作；允许在线性结构的后端进行插入操作；这种线性结构具有“先进先出”的操作特点： 
![[]](http://upload-images.jianshu.io/upload_images/9028834-3c6f3437d441135e?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

但是在实际应用中，队列中的元素有可能不是以“进入的顺序”为排序依据的。例如我们将要讲到的PriorityBlockingQueue队列。 

● **栈**：栈也是一种线性结构，但是栈和队列相比只允许在线性结构的一端进行操作，入栈和出栈都是在一端完成。 
![[]](http://upload-images.jianshu.io/upload_images/9028834-566f7951fa9b6410?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 1. 有限队列

● **SynchronousQueue**：

是这样 一种阻塞队列，其中每个 put 必须等待一个 take，反之亦然。同步队列**没有任何内部容量**。
翻译一下：这是一个内部没有任何容量的阻塞队列，**任何一次插入操作的元素都要等待相对的删除/读取操作**，否则进行插入操作的线程就要一直等待，反之亦然。

```java
SynchronousQueue<Object> queue = new SynchronousQueue<Object>(); 
// 不要使用add，因为这个队列内部没有任何容量，所以会抛出异常“IllegalStateException”
// queue.add(new Object()); 
// 操作线程会在这里被阻塞，直到有其他操作线程取走这个对象
queue.put(new Object());
```


● **ArrayBlockingQueue**：

一个由数组支持的有界阻塞队列。此队列按 **FIFO（先进先出）原则对元素进行排序**。新元素插入到队列的尾部，队列获取操作则是从队列头部开始获得元素。这是一个典型的“有界缓存区”，固定大小的数组在其中保持生产者插入的元素和使用者提取的元素。一旦创建了这样的缓存区，就不能再增加其容量。试图向已满队列中放入元素会导致操作受阻塞；试图从空队列中提取元素将导致类似阻塞。

```java
// 我们创建了一个ArrayBlockingQueue，并且设置队列空间为2 
ArrayBlockingQueue<Object> arrayQueue = new ArrayBlockingQueue<Object>(2); 
// 插入第一个对象 
arrayQueue.put(new Object()); 
// 插入第二个对象 
arrayQueue.put(new Object()); 
// 插入第三个对象时，这个操作线程就会被阻塞。 
arrayQueue.put(new Object()); 
// 请不要使用add操作，和SynchronousQueue的add操作一样，它们都使用了AbstractQueue中的add实现
```

#### 2.无限队列

● **LinkedBlockingQueue**：

LinkedBlockingQueue是我们在ThreadPoolExecutor线程池中常用的等待队列。它**可以指定容量也可以不指定容量**。由于它具有“无限容量”的特性，所以我还是将它归入了无限队列的范畴（实际上任何无限容量的队列/栈都是有容量的，这个容量就是Integer.MAX_VALUE）。 
LinkedBlockingQueue的实现是**基于链表结构**，而不是类似ArrayBlockingQueue那样的数组。但实际使用过程中，不需要关心它的内部实现，**如果指定了LinkedBlockingQueue的容量大小，那么它反映出来的使用特性就和ArrayBlockingQueue类似了**。

```java
LinkedBlockingQueue<Object> linkedQueue = new LinkedBlockingQueue<Object>(2); 
linkedQueue.put(new Object()); 
// 插入第二个对象 
linkedQueue.put(new Object()); 
// 插入第三个对象时，这个操作线程就会被阻塞。 
linkedQueue.put(new Object());
```

```java
// 或者如下使用： 
LinkedBlockingQueue<Object> linkedQueue = new LinkedBlockingQueue<Object>(); 
linkedQueue.put(new Object()); 
// 插入第二个对象 
linkedQueue.put(new Object()); 
// 插入第N个对象时，都不会阻塞 linkedQueue.put(new Object());
```


● **LinkedBlockingDeque**

LinkedBlockingDeque是一个**基于链表的双端队列**。
**LinkedBlockingQueue**的内部结构决定了它**只能从队列尾部插入，从队列头部取出**元素；
但是**LinkedBlockingDeque既可以从尾部插入/取出元素，还可以从头部插入元素/取出元素**。

```java
LinkedBlockingDeque linkedDeque = new LinkedBlockingDeque(); 
// push ，可以从队列的头部插入元素 
linkedDeque.push(new TempObject(1)); 
linkedDeque.push(new TempObject(2)); 
linkedDeque.push(new TempObject(3)); 

// poll ， 可以从队列的头部取出元素 
TempObject tempObject = linkedDeque.poll(); 
// 这里会打印 tempObject.index = 3 
System.out.println("tempObject.index = " + tempObject.getIndex()); 

// put ， 可以从队列的尾部插入元素 
linkedDeque.put(new TempObject(4)); 
linkedDeque.put(new TempObject(5)); 

// pollLast , 可以从队列尾部取出元素 
tempObject = linkedDeque.pollLast(); 
// 这里会打印 tempObject.index = 5 
System.out.println("tempObject.index = " + tempObject.getIndex());
```



● **PriorityBlockingQueue**

PriorityBlockingQueue是一个**按照优先级进行内部元素排序**的无限队列。
存放在PriorityBlockingQueue中的**元素必须实现Comparable接口**，这样才能通过实现compareTo()方法进行排序。优先级最高的元素将始终排在队列的头部；PriorityBlockingQueue**不会保证优先级一样的元素的排序，也不保证当前队列中除了优先级最高的元素以外的元素，随时处于正确排序的位置**。 
这是什么意思呢？PriorityBlockingQueue并不保证除了队列头部以外的元素排序一定是正确的。请看下面的示例代码：

```java
PriorityBlockingQueue priorityQueue = new PriorityBlockingQueue();
priorityQueue.put(new TempObject(-5));
priorityQueue.put(new TempObject(5));
priorityQueue.put(new TempObject(-1));
priorityQueue.put(new TempObject(1));
// 第一个元素是5 
TempObject targetTempObject = priorityQueue.poll();
System.out.println("tempObject.index = " + targetTempObject.getIndex());
// 实际上在还没有执行priorityQueue.poll()语句的时候，队列中的第二个元素不一定是1 
// 第二个元素是1 
targetTempObject = priorityQueue.poll();
System.out.println("tempObject.index = " + targetTempObject.getIndex());
// 第三个元素是-1 
targetTempObject = priorityQueue.poll();
System.out.println("tempObject.index = " + targetTempObject.getIndex());
// 第四个元素是-5 
targetTempObject = priorityQueue.poll();
System.out.println("tempObject.index = " + targetTempObject.getIndex());
```

```java
// 这个元素类，必须实现Comparable接口 
private static class TempObject implements Comparable<TempObject> {
    private int index;
    public TempObject(int index) {
        this.index = index;
    }
    public int getIndex() {
        return index;
    }
    @Override
    public int compareTo(TempObject o) {
        return o.getIndex() - this.index;
    }
}
```


● **LinkedTransferQueue**

LinkedTransferQueue也是一个无限队列，它除了具有一般队列的操作特性外（**先进先出**），还具有一个**阻塞**特性：LinkedTransferQueue可以由一对生产者/消费者线程进行操作，当消费者将一个新的元素插入队列后，消费者线程将会一直等待，直到某一个消费者线程将这个元素取走，反之亦然。 
LinkedTransferQueue的操作特性可以由下面这段代码提现。在下面的代码片段中，有两中类型的线程：生产者和消费者，这两类线程互相等待对方的操作：

```java
/* 消费者线程 */
private static class ConsumerRunnable implements Runnable {
    private LinkedTransferQueue linkedQueue;
    public ConsumerRunnable(LinkedTransferQueue linkedQueue) {
        this.linkedQueue = linkedQueue;
    }

    @Override
    public void run() {
        Thread currentThread = Thread.currentThread();
        while (!currentThread.isInterrupted()) {
            try { // 等待，直到从LinkedTransferQueue队列中得到一个元素
                TempObject targetObject = this.linkedQueue.take();
                System.out.println("线程（" + currentThread.getId() + "）取得targetObject.index = " + targetObject.getIndex());
            } catch (InterruptedException e) {
                e.printStackTrace(System.out);
            }
        }
    }
}
```

```java
//以下是启动代码：
LinkedTransferQueue<TempObject> linkedQueue = new LinkedTransferQueue<TempObject>(); 
// 这是一个生产者线程 
Thread producerThread = new Thread(new ProducerRunnable(linkedQueue)); 
// 这里有两个消费者线程 
Thread consumerRunnable1 = new Thread(new ConsumerRunnable(linkedQueue)); 
Thread consumerRunnable2 = new Thread(new ConsumerRunnable(linkedQueue)); 
// 开始运行 
producerThread.start(); 
consumerRunnable1.start(); 
consumerRunnable2.start(); 
// 这里只是为了main不退出，没有任何演示含义 
Thread currentThread = Thread.currentThread(); 
synchronized (currentThread) { currentThread.wait(); }
```

### 拒绝任务（handler）

在ThreadPoolExecutor线程池中还有一个重要的接口：**RejectedExecutionHandler**。当提交给线程池的某一个新任务无法直接被线程池中“核心线程”直接处理，又无法加入等待队列，也无法创建新的线程执行；又或者线程池已经调用shutdown()方法停止了工作；又或者线程池不是处于正常的工作状态；这时候ThreadPoolExecutor线程池会拒绝处理这个任务，触发创建ThreadPoolExecutor线程池时定义的RejectedExecutionHandler接口的实现

在创建ThreadPoolExecutor线程池时，一定会指定RejectedExecutionHandler接口的实现。如果调用的是不需要指定RejectedExecutionHandler接口的构造函数，如：

```java
public ThreadPoolExecutor(int corePoolSize, int maximumPoolSize, long keepAliveTime, TimeUnit unit, BlockingQueue workQueue) 
public ThreadPoolExecutor(int corePoolSize, int maximumPoolSize, long keepAliveTime, TimeUnit unit, BlockingQueue workQueue, ThreadFactory threadFactory)
```

那么ThreadPoolExecutor线程池在创建时，会使用一个**默认的RejectedExecutionHandler接口实现**，源代码片段如下：


```java
public class ThreadPoolExecutor extends AbstractExecutorService { ......
    /**
     * The default rejected execution handler
     */
    private static final RejectedExecutionHandler defaultHandler = new AbortPolicy(); ......
// 可以看到，ThreadPoolExecutor中的两个没有指定RejectedExecutionHandler
// 接口的构造函数，都是使用了一个RejectedExecutionHandler接口的默认实现：AbortPolicy

    public ThreadPoolExecutor(int corePoolSize, int maximumPoolSize, long keepAliveTime, TimeUnit unit, BlockingQueue workQueue) {
        this(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue, Executors.defaultThreadFactory(), defaultHandler);
    } ......

    public ThreadPoolExecutor(int corePoolSize, int maximumPoolSize, long keepAliveTime, TimeUnit unit, BlockingQueue workQueue, ThreadFactory threadFactory) {
        this(corePoolSize, maximumPoolSize, keepAliveTime, unit, workQueue, threadFactory, defaultHandler);
    } ......
}
```

实际上，在ThreadPoolExecutor中已经提供了四种可以直接使用的RejectedExecutionHandler接口的实现：

● **CallerRunsPolicy**： 
这个拒绝处理器，将直接运行这个任务的run方法。但是，请注意并不是在ThreadPoolExecutor线程池中的线程中运行，而是直接调用这个任务实现的run方法。源代码如下：

```java
public static class CallerRunsPolicy implements RejectedExecutionHandler {
    /**
     * Creates a {@code CallerRunsPolicy}.
     */
    public CallerRunsPolicy() {
    }

    /**
     * Executes task r in the caller's thread, unless the executor * has been shut down, in which case the task is discarded. * * @param r the runnable task requested to be executed * @param e the executor attempting to execute this task
     */
    public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {
        if (!e.isShutdown()) {
            r.run();
        }
    }
}
```

● **AbortPolicy**：

这个处理器，在任务被拒绝后会创建一个RejectedExecutionException异常并抛出。这个处理过程也是ThreadPoolExecutor线程池默认的RejectedExecutionHandler实现。

● **DiscardPolicy**： 
DiscardPolicy处理器，将会默默丢弃这个被拒绝的任务，不会抛出异常，也不会通过其他方式执行这个任务的任何一个方法，更不会出现任何的日志提示。

● **DiscardOldestPolicy**： 
这个处理器很有意思。它会检查当前ThreadPoolExecutor线程池的等待队列。并调用队列的poll()方法，将当前处于等待队列列头的等待任务强行取出，然后再试图将当前被拒绝的任务提交到线程池执行：

```java
public static class DiscardOldestPolicy implements RejectedExecutionHandler {
    ......
    public void rejectedExecution(Runnable r, ThreadPoolExecutor e) {
        if (!e.isShutdown()) {
            e.getQueue().poll();
            e.execute(r);
        }
    } 
    ......
}
```

实际上查阅这四种ThreadPoolExecutor线程池自带的拒绝处理器实现，您可以发现CallerRunsPolicy、DiscardPolicy、DiscardOldestPolicy处理器针对被拒绝的任务并不是一个很好的处理方式。 
CallerRunsPolicy在非线程池以外直接调用任务的run方法，可能会造成线程安全上的问题；DiscardPolicy默默的忽略掉被拒绝任务，也没有输出日志或者提示，开发人员不会知道线程池的处理过程出现了错误；DiscardOldestPolicy中e.getQueue().poll()的方式好像是科学的，但是如果等待队列出现了容量问题，大多数情况下就是这个线程池的代码出现了BUG。
最科学的的还是AbortPolicy提供的处理方式：抛出异常，由开发人员进行处理。


## 常用的几种线程池

### 1. newCachedThreadPool

创建一个可缓存线程池，如果线程池长度超过处理需要，可灵活回收空闲线程，若无可回收，则新建线程。

这种类型的线程池特点是：

*   工作线程的**创建数量几乎没有限制**(其实也有限制的,数目为Interger. MAX_VALUE), 这样可灵活的往线程池中添加线程。
*   如果长时间没有往线程池中提交任务，即如果工作线程空闲了指定的时间(默认为1分钟)，则该工作线程将自动终止。终止后，如果你又提交了新的任务，则线程池重新创建一个工作线程。
*   在使用CachedThreadPool时，一定要注意控制任务的数量，否则，由于**大量线程同时运行，很有会造成系统瘫痪**。

示例代码如下：

```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class ThreadPoolExecutorTest {
    public static void main(String[] args) {
        //Executors.newCachedThreadPool()
        ExecutorService cachedThreadPool = Executors.newCachedThreadPool();
        for (int i = 0; i < 10; i++) {
            final int index = i;
            try {
                Thread.sleep(index * 1000);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            cachedThreadPool.execute(new Runnable() {
                public void run() {
                    System.out.println(index);
                }
            });
        }
    }
}
```

### 2. newFixedThreadPool

创建一个指定工作线程数量的线程池。每当提交一个任务就创建一个工作线程，如果工作线程数量达到线程池初始的最大数，则将提交的任务存入到池队列中。

FixedThreadPool是一个典型且优秀的线程池，它具有线程池**提高程序效率和节省创建线程时所耗的开销**的优点。但是，在线程池空闲时，即**线程池中没有可运行任务时，它不会释放工作线程，还会占用一定的系统资源**。

示例代码如下：
```java
import java.util.concurrent.ExecutorService;
import java.util.concurrent.Executors;

public class ThreadPoolExecutorTest {
    public static void main(String[] args) {
        //Executors.newFixedThreadPool
        ExecutorService fixedThreadPool = Executors.newFixedThreadPool(3);
        for (int i = 0; i < 10; i++) {
            final int index = i;
            fixedThreadPool.execute(new Runnable() {
                public void run() {
                    try {
                        System.out.println(index);
                        Thread.sleep(2000);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            });
        }
    }
}
```

因为线程池大小为3，每个任务输出index后sleep 2秒，所以每两秒打印3个数字。
定长线程池的大小最好根据系统资源进行设置如Runtime.getRuntime().availableProcessors()。

### 3. newSingleThreadExecutor

创建一个单线程化的Executor，即只创建唯一的工作者线程来执行任务，它只会用唯一的工作线程来执行任务，保证所有任务按照指定顺序(FIFO, LIFO, 优先级)执行。如果这个线程异常结束，会有另一个取代它，保证顺序执行。
单工作线程最大的特点是可**保证顺序地执行各个任务**，并且在任意给定的时间不会有多个线程是活动的。

示例代码如下：

```java
public class ThreadPoolExecutorTest {
    public static void main(String[] args) {
        ExecutorService singleThreadExecutor = Executors.newSingleThreadExecutor();
        for (int i = 0; i < 10; i++) {
            final int index = i;
            singleThreadExecutor.execute(new Runnable() {
                public void run() {
                    try {
                        System.out.println(index);
                        Thread.sleep(2000);
                    } catch (InterruptedException e) {
                        e.printStackTrace();
                    }
                }
            });
        }
    }
} 
```

### 4. newScheduleThreadPool

创建一个定长的线程池，而且支持**定时的以及周期性的任务执行**，支持定时及周期性任务执行。

延迟3秒执行，延迟执行示例代码如下：
```java
public class ThreadPoolExecutorTest {
    public static void main(String[] args) {
        ScheduledExecutorService scheduledThreadPool = Executors.newScheduledThreadPool(5);
        //scheduledThreadPool.schedule
        scheduledThreadPool.schedule(new Runnable() {
            public void run() {
                System.out.println("delay seconds");
            }
        }, 3, TimeUnit.SECONDS);//延迟3秒执行
    }
}
```

表示**延迟1秒后每3秒执行一次**，定期执行示例代码如下：
```java
public class ThreadPoolExecutorTest {
    public static void main(String[] args) {
        ScheduledExecutorService scheduledThreadPool = Executors.newScheduledThreadPool(5);
        //scheduledThreadPool.scheduleAtFixedRate
        scheduledThreadPool.scheduleAtFixedRate(new Runnable() {
            public void run() {
                System.out.println("delay seconds, and excute every seconds");
            }
        }, 1, 3, TimeUnit.SECONDS);//延迟1秒后每3秒执行一次
    }
}
```









引用：
[关于Java中的程序，进程和线程的详解...](https://www.cnblogs.com/xiohao/p/4310644.html)
[java/android线程池详解](https://www.cnblogs.com/yuhanghzsd/p/5611562.html)
[线程池的使用（ThreadPoolExecutor详解）](http://blog.csdn.net/lipc_/article/details/52025993)
[Java中的多线程你只要看这一篇就够了](http://www.importnew.com/21089.html)
[java常用的几种线程池比较](http://www.cnblogs.com/aaron911/p/6213808.html)



