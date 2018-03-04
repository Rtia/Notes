<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [【JAVA IO流】](#java-io%E6%B5%81)
  - [IO(Input Output)流](#ioinput-output%E6%B5%81)
  - [IO流常用基类](#io%E6%B5%81%E5%B8%B8%E7%94%A8%E5%9F%BA%E7%B1%BB)
  - [IO程序的书写](#io%E7%A8%8B%E5%BA%8F%E7%9A%84%E4%B9%A6%E5%86%99)
  - [字符流](#%E5%AD%97%E7%AC%A6%E6%B5%81)
    - [字符流——创建文件](#%E5%AD%97%E7%AC%A6%E6%B5%81%E5%88%9B%E5%BB%BA%E6%96%87%E4%BB%B6)
      - [IO异常的处理方式](#io%E5%BC%82%E5%B8%B8%E7%9A%84%E5%A4%84%E7%90%86%E6%96%B9%E5%BC%8F)
      - [对已有文件的数据续写](#%E5%AF%B9%E5%B7%B2%E6%9C%89%E6%96%87%E4%BB%B6%E7%9A%84%E6%95%B0%E6%8D%AE%E7%BB%AD%E5%86%99)
    - [字符流——读取文件](#%E5%AD%97%E7%AC%A6%E6%B5%81%E8%AF%BB%E5%8F%96%E6%96%87%E4%BB%B6)
      - [通过字符数组读取文件](#%E9%80%9A%E8%BF%87%E5%AD%97%E7%AC%A6%E6%95%B0%E7%BB%84%E8%AF%BB%E5%8F%96%E6%96%87%E4%BB%B6)
    - [实例](#%E5%AE%9E%E4%BE%8B)
      - [习题：复制文本文件](#%E4%B9%A0%E9%A2%98%E5%A4%8D%E5%88%B6%E6%96%87%E6%9C%AC%E6%96%87%E4%BB%B6)
    - [字符流的缓冲区](#%E5%AD%97%E7%AC%A6%E6%B5%81%E7%9A%84%E7%BC%93%E5%86%B2%E5%8C%BA)
      - [BufferedWriter](#bufferedwriter)
      - [BufferedReader](#bufferedreader)
      - [习题：通过缓冲区复制一个.java文件](#%E4%B9%A0%E9%A2%98%E9%80%9A%E8%BF%87%E7%BC%93%E5%86%B2%E5%8C%BA%E5%A4%8D%E5%88%B6%E4%B8%80%E4%B8%AAjava%E6%96%87%E4%BB%B6)
      - [习题：模拟BufferedReader-使用装饰设计模式](#%E4%B9%A0%E9%A2%98%E6%A8%A1%E6%8B%9Fbufferedreader-%E4%BD%BF%E7%94%A8%E8%A3%85%E9%A5%B0%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F)
      - [习题：模拟LineNumerReader](#%E4%B9%A0%E9%A2%98%E6%A8%A1%E6%8B%9Flinenumerreader)
  - [字节流](#%E5%AD%97%E8%8A%82%E6%B5%81)
      - [习题：复制一个图片](#%E4%B9%A0%E9%A2%98%E5%A4%8D%E5%88%B6%E4%B8%80%E4%B8%AA%E5%9B%BE%E7%89%87)
    - [字节流的缓冲区](#%E5%AD%97%E8%8A%82%E6%B5%81%E7%9A%84%E7%BC%93%E5%86%B2%E5%8C%BA)
      - [练习：通过几种方式对MP3的进行拷贝，比较它们的效率](#%E7%BB%83%E4%B9%A0%E9%80%9A%E8%BF%87%E5%87%A0%E7%A7%8D%E6%96%B9%E5%BC%8F%E5%AF%B9mp3%E7%9A%84%E8%BF%9B%E8%A1%8C%E6%8B%B7%E8%B4%9D%E6%AF%94%E8%BE%83%E5%AE%83%E4%BB%AC%E7%9A%84%E6%95%88%E7%8E%87)
  - [标准输入输出流](#%E6%A0%87%E5%87%86%E8%BE%93%E5%85%A5%E8%BE%93%E5%87%BA%E6%B5%81)
    - [习题：读取键盘录入](#%E4%B9%A0%E9%A2%98%E8%AF%BB%E5%8F%96%E9%94%AE%E7%9B%98%E5%BD%95%E5%85%A5)
  - [转换流](#%E8%BD%AC%E6%8D%A2%E6%B5%81)
    - [标准输入输出流示例](#%E6%A0%87%E5%87%86%E8%BE%93%E5%85%A5%E8%BE%93%E5%87%BA%E6%B5%81%E7%A4%BA%E4%BE%8B)
  - [流的基本应用小节](#%E6%B5%81%E7%9A%84%E5%9F%BA%E6%9C%AC%E5%BA%94%E7%94%A8%E5%B0%8F%E8%8A%82)
    - [流操作的基本规律](#%E6%B5%81%E6%93%8D%E4%BD%9C%E7%9A%84%E5%9F%BA%E6%9C%AC%E8%A7%84%E5%BE%8B)
    - [示例：](#%E7%A4%BA%E4%BE%8B)
      - [习题：生成异常日志文件](#%E4%B9%A0%E9%A2%98%E7%94%9F%E6%88%90%E5%BC%82%E5%B8%B8%E6%97%A5%E5%BF%97%E6%96%87%E4%BB%B6)
      - [习题：生成系统日志文件](#%E4%B9%A0%E9%A2%98%E7%94%9F%E6%88%90%E7%B3%BB%E7%BB%9F%E6%97%A5%E5%BF%97%E6%96%87%E4%BB%B6)
    - [字符流继承体系简图](#%E5%AD%97%E7%AC%A6%E6%B5%81%E7%BB%A7%E6%89%BF%E4%BD%93%E7%B3%BB%E7%AE%80%E5%9B%BE)
    - [字节流继承体系简图](#%E5%AD%97%E8%8A%82%E6%B5%81%E7%BB%A7%E6%89%BF%E4%BD%93%E7%B3%BB%E7%AE%80%E5%9B%BE)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->



# 【JAVA IO流】

## IO(Input Output)流
　 IO流用来处理设备之间的**数据传输**
　 Java对**数据**的**操作**是通过**流**的方式
　 Java用于操作流的**对象**都在**IO包**中
　 
　 流按**操作数据分**为两种：**字节**流与**字符**流。
　 流按**流向分**为：**输入**流，**输出**流。

## IO流常用基类
**字节流的抽象基类：**
• InputStream ，OutputStream。
**字符流的抽象基类：**
• Reader ， Writer。

**注**：由这四个类**派生**出来的**子类名称**都是**以**其**父类名作**为子类名的**后缀**。
• 如：InputStream的子类FileInputStream。
• 如：Reader的子类FileReader。

## IO程序的书写
 导入IO包中的类
 进行IO异常处理
 在finally中对流进行关闭

## 字符流
既然IO流是用于操作数据的，数据的最常见体现形式是：文件。
那么先以**操作文件**为主来演示。（Java实际上是在调用其所在操作系统(Windows、IOS...)底层资源进行文件的创建书写）

 **注意：**
 定义**文件路径**时，可以用“**/**”或者“**\\\\**”。
 在**创建**一个文件时，如果目录下有**同名**文件将被**覆盖**。
在**读取**文件时，必须保证该文件**已存在**，**否则出异常**。
　
### 字符流——创建文件
 **创建流对象**，建立数据存放文件
• **FileWriter fw = new FileWriter(“Test.txt”);**
 调用流对象的写入**方法**，将数据写入流
• **fw.write(“text”);**
 **关闭流资源**，并将流中的数据清空到文件中。
• **fw.close();**
```
public class FileWriterDemo {
	public static void main(String[] args) throws IOException {
		// 创建一个FileWriter对象。该对象一被初始化就必须要明确被操作的文件。
		// 而且该文件会被创建到指定目录下。如果该目录下已有同名文件，将被覆盖。
		// 其实该步就是在明确数据要存放的目的地。
		FileWriter fw = new FileWriter("demo.txt");
		//如果不具体写路径，JVM会默认把当前java程序的目录作为demo.txt的路径
		
		// 调用write方法，将字符串写入到流中。
		fw.write("abcde");
		
		// 刷新流对象中的缓冲中的数据。将数据刷到目的地中。
		// fw.flush();
		
		// 关闭流资源，但是关闭之前会刷新一次内部的缓冲中的数据。将数据刷到目的地中。
		// 和flush区别：flush刷新后，流可以继续使用，close刷新后，会将流关闭。
		fw.close();
		// fw.write("abcde");//错误，不可close之后出现
	}
}
```

#### IO异常的处理方式
```
public class FileWriterDemo2 {
	public static void main(String[] args) {
		// try里面的变量finally用不了，所以在代码块之外创建变量
		FileWriter fw = null;
		
		try {
			fw = new FileWriter("K:demo.txt");// 当创建出错，fw依旧=null
			fw.write("abcdefg");
		} catch (IOException e) {
			System.out.println("catch: " + e.toString());// K盘不存在出现异常
		} finally {
			try {
				if (fw != null) // fw为null时调用不了close方法
					fw.close();
			} catch (IOException e) {
				System.out.println("finally: "+e.toString());
			}
		}
	}
}
```
结果：

```
catch: java.io.FileNotFoundException: K:demo.txt (系统找不到指定的路径。)
```

#### 对已有文件的数据续写
```
public class FileWriterDemo3 {
	public static void main(String[] args) {
		FileWriter fw = null;
		try {
			fw = new FileWriter("demo.txt", true);
			// 传递一个true参数，代表不覆盖已有的文件。并在已有文件的末尾处进行数据续写。
			
			fw.write("\r\n again");// \n在Linux是换行，\r\n在txt中是换行
		} catch (IOException e) {
			System.out.println("catch:" + e.toString());
		} finally {
			try {
				if (fw != null)
					fw.close();
			} catch (IOException e) {
				System.out.println(e.toString());
			}
		}
	}
}
```


### 字符流——读取文件

建立一个流对象，将已存在的一个文件加载进流。
• **FileReader fr = new FileReader(“Test.txt”);**
创建一个临时存放数据的数组。
• **char[] ch = new char[1024];**
调用流对象的读取方法将流中的数据读入到数组中。
• **fr.read(ch);**

```
public class FileReaderDemo {
	public static void main(String[] args) throws IOException {
		// 创建一个文件读取流对象，和指定名称的文件相关联。
		// 要保证该文件是已经存在的，如果不存在，会发生异常FileNotFoundException
		FileReader fr = new FileReader("demo.txt");
		
		int ch = 0;
		
		// 调用读取流对象的read方法。
		// read():一次读一个字符，自动往下读。当读到空白，会返回-1
		while ((ch = fr.read()) != -1) {
			System.out.println("ch=" + (char) ch);// (char)转成对应字符输出
		}
		
		fr.close();
	}
}
```

#### 通过字符数组读取文件

```
public class FileReaderDemo2 {
	public static void main(String[] args) throws IOException {
		FileReader fr = new FileReader("demo.txt");		
		//定义一个字符数组。用于存储读到字符。		
		char[] buf = new char[3];//当定义的字符数组长度不够时(图例2)
		
		int num = fr.read(buf);//该read(char[])返回的是读到字符个数。
		System.out.println("num=" + num + " buf=" + new String(buf));//num=3 buf=ABC
		
		int num1 = fr.read(buf);
		System.out.println("num1=" + num1 + " buf=" + new String(buf));//num1=3 buf=DEF
		
		int num2 = fr.read(buf);//读完G后面就没了，所以num2=1
		//上轮读完，数组第2第3个位置是E F，没被改变，所以打印出GEF
		System.out.println("num2=" + num2 + " buf=" + new String(buf));//num2=1 buf=GEF

		int num3 = fr.read(buf);
		//读不到内容，返回-1
		System.out.println("num3=" + num3 + " buf=" + new String(buf));//num3=-1 buf=GEF
		fr.close();
		
		FileReader fr0 = new FileReader("demo.txt");
		char[] buf0 = new char[1024]; //要定义足够长的字符数组
		int num0 = 0;
		while ((num0 = fr0.read(buf0)) != -1) {
			System.out.println(new String(buf0, 0, num0)); //只打印指定部分
		}
		fr0.close();
	}
}
```
结果：
```
num=3 buf=ABC
num1=3 buf=DEF
num2=1 buf=GEF
num3=-1 buf=GEF
ABCDEFG
```

**read图例2**

![](http://img.blog.csdn.net/20171223230917649?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### 实例
#### 习题：复制文本文件
复制的原理：
将C盘一个文本文件复制到D盘，其实就是**将C盘下的文件数据存储到D盘的一个文件**中。
步骤：
1，在D盘创建一个文件。用于存储C盘文件中的数据。
2，定义读取流和C盘文件关联。
3，通过不断的读写完成数据存储。
4，关闭资源。

```
public class CopyText {
	public static void main(String[] args) throws IOException {
		copy_2();
	}

	//方法1：从C盘读一个字符，就往D盘写一个字符。一边读一边写，慢
	public static void copy_1() throws IOException {
		//创建目的地。
		FileWriter fw = new FileWriter("RuntimeDemo_copy.txt");
		//与已有文件关联。
		FileReader fr = new FileReader("RuntimeDemo.java");
		int ch = 0;
		while ((ch = fr.read()) != -1) {
			fw.write(ch);
		}

		fw.close();
		fr.close();
	}

	//方法2：全读完再写，比方法1快
	public static void copy_2() {
		FileWriter fw = null;
		FileReader fr = null;
		try {
			fw = new FileWriter("CopyText_copy.txt");
			fr = new FileReader("CopyText.java");
			char[] buf = new char[1024];
			int len = 0;
			while ((len = fr.read(buf)) != -1) {
				fw.write(buf, 0, len);
			}
		} catch (IOException e) {
			throw new RuntimeException("读写失败");
		} finally {
			if (fr != null)
				try {
					fr.close();
				} catch (IOException e) {
				}
			if (fw != null)
				try {
					fw.close();
				} catch (IOException e) {
				}
		}
	}
}
```
 **复制文本文件图例**
![](http://img.blog.csdn.net/20171223231854225?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### 字符流的缓冲区
缓冲区的出现**提高**了**流**对**数据**的**读写效率**。
所以在创建缓冲区之前，必须要**先有流对象**。
**对应类**
• BufferedWriter
• BufferedReader
缓冲区要结合流才可以使用。
在**流的基础上**对流的功能进行了增强。


#### BufferedWriter
该缓冲区中提供了一个**跨平台的换行符**：**newLine**();

```
public class BufferedWriterDemo {
	public static void main(String[] args) throws IOException {
		//创建一个字符写入流对象。
		FileWriter fw = new FileWriter("buf.txt");
		
		//为了提高字符写入流效率。加入了缓冲技术。
		//只要将需要被提高效率的流对象作为参数传递给缓冲区的构造函数即可。
		BufferedWriter bufw = new BufferedWriter(fw);
		for (int x = 1; x < 5; x++) {
			bufw.write("abcd" + x);
			bufw.newLine();//跨平台的换行符
			bufw.flush();//写一次刷新一下，以免突然停电将内存数据清空了
		}
		//记住，只要用到缓冲区，就要记得刷新。
		//bufw.flush();
		
		bufw.close();
		//其实关闭缓冲区，就是在关闭缓冲区中的流对象。所以不需要再fw.close();
	}
}
```

#### BufferedReader

**字符读取流缓冲区：**
该缓冲区提供了一个**一次读一行**的方法 **readLine**，方便于对文本数据的获取。
**当返回null时，表示读到文件末尾。**
readLine方法返回的时候只返回回车符之前的数据内容。并**不返回回车符**。

```
public class BufferedReaderDemo {
	public static void main(String[] args) throws IOException {
		//创建一个读取流对象和文件相关联。
		FileReader fr = new FileReader("buf.txt");
		
		//为了提高效率。加入缓冲技术。将字符读取流对象作为参数传递给缓冲对象的构造函数。
		BufferedReader bufr = new BufferedReader(fr);
		String line = null;
		while ((line = bufr.readLine()) != null) {
			System.out.println(line);
		}
		bufr.close();
	}
}
```

#### 习题：通过缓冲区复制一个.java文件

```
public class CopyTextByBuf {
	public static void main(String[] args) {
		BufferedReader bufr = null;
		BufferedWriter bufw = null;
		try {
			bufr = new BufferedReader(new FileReader("BufferedWriterDemo.java"));
			bufw = new BufferedWriter(new FileWriter("bufWriter_Copy.txt"));
			String line = null;
			while ((line = bufr.readLine()) != null) {
				bufw.write(line);
				//要换行，否则原文件几行内容会复制到一行，因为readLine不返回回车符
				bufw.newLine();
				bufw.flush();
			}
		} catch (IOException e) {
			throw new RuntimeException("读写失败");
		} finally {
			try {
				if (bufr != null)
					bufr.close();
			} catch (IOException e) {
				throw new RuntimeException("读取关闭失败");
			}
			try {
				if (bufw != null)
					bufw.close();
			} catch (IOException e) {
				throw new RuntimeException("写入关闭失败");
			}
		}
	}
}
```
**readLine原理图例**
无论读一行，或者读多个字符，其实最终都是在硬盘上一个一个读取；所以最终使用的还是read方法一次读一个。
![](http://img.blog.csdn.net/20171223232714599?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)


#### 习题：模拟BufferedReader-使用装饰设计模式
可以自定义一个类中包含一个功能和readLine一致的方法，来模拟BufferedReader

```
public class MyBufferedReader extends Reader {
	private Reader r;
	MyBufferedReader(Reader r) {//【装饰设计模式】对Reader进行功能增强
		this.r = r;
	}

	//可以一次读一行数据的方法。
	public String myReadLine()throws IOException//不try抛出异常，谁调用谁处理
	{
		//定义一个临时容器。原BufferReader封装的是字符数组。为了演示方便。定义一个StringBuilder容器。因为最终还是要将数据变成字符串。
		StringBuilder sb = new StringBuilder();
		int ch = 0;
		while((ch=r.read())!=-1)
		{
			if(ch=='\r')
				continue; //continue语句是结束本次循环继续下次循环
			if(ch=='\n')
				return sb.toString();
			//当碰到换行标识返回sb，但最后一行结尾可能没有换行标识
			else
				sb.append((char)ch);
		}
		if(sb.length()!=0)//当最后一行有内容但结尾没有换行标识，返回sb
			return sb.toString();
		return null; //没内容时返回null
	}

	//覆盖Reader类中的抽象方法(继承抽象父类得覆盖其抽象方法，否则无法创建对象)
	public int read(char[] cbuf, int off, int len) throws IOException {
		return r.read(cbuf, off, len);
	}

	public void close() throws IOException {
		r.close();
	}

	public void myClose() throws IOException {
		r.close();
	}
}
```

```
public class MyBufferedReaderDemo {
	public static void main(String[] args) throws IOException {
		FileReader fr = new FileReader("buf.txt");
		MyBufferedReader myBuf = new MyBufferedReader(fr);
		String line = null;
		while ((line = myBuf.myReadLine()) != null) {
			System.out.println(line);
		}
		myBuf.myClose();
	}
}
```

#### 习题：模拟LineNumerReader

```
public class MyLineNumberReader extends MyBufferedReader {
	//private Reader r;
	private int lineNumber;

	MyLineNumberReader(Reader r) {
		super(r); /* this.r = r;*/
	}

	public String myReadLine() throws IOException {
		lineNumber++;
		return super.myReadLine();
		/*StringBuilder sb = new StringBuilder();
		int ch = 0;
		while((ch=r.read())!=-1)
		{
			if(ch==‘\r’)
				continue;
			if(ch==‘\n’)
				return sb.toString();
			else
				sb.append((char)ch);
		}
		if(sb.length()!=0)
			return sb.toString();
		return null;
		*/
	}

	public void setLineNumber(int lineNumber) {
		this.lineNumber = lineNumber;
	}
	public int getLineNumber() {
		return lineNumber;
	}
	/*public void myClose()throws IOException
	{
		r.close();
	}	*/
}
```

```
public class MyLineNumberReaderDemo {
	public static void main(String[] args) throws IOException {
		FileReader fr = new FileReader("buf.txt");
		MyLineNumberReader mylnr = new MyLineNumberReader(fr);
		String line = null;
		mylnr.setLineNumber(100);
		while ((line = mylnr.myReadLine()) != null) {
			System.out.println(mylnr.getLineNumber() + "::" + line);
		}
		mylnr.myClose();
	}
}
```


## 字节流
基本操作与字符流类相同
但它不仅可以操作字符，**还可**以操作其他**媒体文件**
比如，**想要操作图片数据，就要用到字节流**。

```
public class FileStream {
	public static void main(String[] args) throws IOException {
		writeFile();
		readFile_2();
	}

	public static void writeFile() throws IOException {
		FileOutputStream fos = new FileOutputStream("fos.txt");
		fos.write("abcde".getBytes());//字符转成字节
		//字节流不需要缓冲，一个字节一个字节直接写入；字符流底层用的是字节流，会先进行缓冲（如中文一个字符两个字节，得先缓冲，两个字节齐了查编码表才知道是什么字符）
		fos.close();
	}

	public static void readFile_1() throws IOException {
		FileInputStream fis = new FileInputStream("fos.txt");
		int ch = 0;
		while ((ch = fis.read()) != -1) {
			System.out.println((char) ch);
		}
		fis.close();
	}

	public static void readFile_2() throws IOException {
		FileInputStream fis = new FileInputStream("fos.txt");
		byte[] buf = new byte[1024];//1024整数倍
		int len = 0;
		while ((len = fis.read(buf)) != -1) {
			System.out.println(new String(buf, 0, len));
		}
		fis.close();
	}

	public static void readFile_3() throws IOException {
		FileInputStream fis = new FileInputStream("fos.txt");
		
		int num = fis.available();//返回文件中总共有多少个字节
		
		byte[] buf = new byte[num];//定义一个刚刚好的缓冲区。不用在循环了。
		//慎用，如果操作的文件很大，定义这么大的缓冲区可能造成内存溢出（文件比内存还大）
		//综合比较，选用方法2较好
		
		fis.read(buf);
		System.out.println(new String(buf));
		fis.close();
	}
}
```



#### 习题：复制一个图片
思路：
1，用字节读取流对象和图片关联。
2，用字节写入流对象创建一个图片文件。用于存储获取到的图片数据。
3，通过循环读写，完成数据的存储。
4，关闭资源。

```
public class CopyPic {
	public static void main(String[] args) {
		FileOutputStream fos = null;
		FileInputStream fis = null;
		try {
			fos = new FileOutputStream("0_COPY.png");
			fis = new FileInputStream("0.png");
			byte[] buf = new byte[1024];
			int len = 0;
			while ((len = fis.read(buf)) != -1) {
				fos.write(buf, 0, len);
			}
		} catch (IOException e) {
			throw new RuntimeException("复制文件失败");
		} finally {
			try {
				if (fis != null)
					fis.close();
			} catch (IOException e) {
				throw new RuntimeException("读取关闭失败");
			}
			try {
				if (fos != null)
					fos.close();
			} catch (IOException e) {
				throw new RuntimeException("写入关闭失败");
			}
		}
	}
}
```
### 字节流的缓冲区
同样是提高了字节流的读写效率。

#### 练习：通过几种方式对MP3的进行拷贝，比较它们的效率

```
public class CopyMp3 {
	public static void main(String[] args) throws IOException {
		long start = System.currentTimeMillis();
		copy_1();
		long end = System.currentTimeMillis();
		System.out.println((end - start) + "毫秒");//55毫秒

		long start2 = System.currentTimeMillis();
		copy_2();
		long end2 = System.currentTimeMillis();
		System.out.println((end2 - start2) + "毫秒");//52毫秒
	}

	//通过字节流的缓冲区完成复制
	public static void copy_1() throws IOException {
		BufferedInputStream bufis = new BufferedInputStream(new FileInputStream("0.mp3"));
		BufferedOutputStream bufos = new BufferedOutputStream(new FileOutputStream("0_copy1.mp3"));
		int by = 0;
		while ((by = bufis.read()) != -1) {
			bufos.write(by);
		}
		bufis.close();
		bufos.close();
	}

	//用自定义的缓冲区完成复制
	public static void copy_2() throws IOException {
		MyBufferedInputStream bufis = new MyBufferedInputStream(new FileInputStream("0.mp3"));
		BufferedOutputStream bufos = new BufferedOutputStream(new FileOutputStream("0_copy2.mp3"));
		int by = 0;
		//System.out.println("第一个字节:"+bufis.myRead());
		while ((by = bufis.myRead()) != -1) {
			bufos.write(by);
		}
		bufos.close();
		bufis.myClose();
	}
}
```

```
public class MyBufferedInputStream {
	private InputStream in;
	private byte[] buf = new byte[1024 * 4];
	private int pos = 0, count = 0;

	MyBufferedInputStream(InputStream in) {
		this.in = in;
	}

	//一次读一个字节，从缓冲区(字节数组)获取。
	public int myRead() throws IOException {
		//read方法把byte提升为int，以免11111111被读成-1
		//write做强转，写入byte数据，保证写入的数据不会比原数据大

		//通过in对象读取硬盘上数据，并存储buf中。
		if (count == 0) {
			count = in.read(buf);
			if (count < 0)
				return -1;//当读到没数据时，返回-1
			pos = 0;
			byte b = buf[pos];
			count--;
			pos++;
			return b & 255;//保证第一字节是11111111时， 不被读成-1(原理同下)
		} else if (count > 0) {
			byte b = buf[pos];
			count--;
			pos++;
			return b & 0xff;
			// 0xff是255十六进制；&00000000 00000000 00000000 11111111，即提升为int保留最后8位，前面补0；
			// 否则正好是11111111时，读成-1，copy_2()中(by=bufis.myRead())==-1，停止while循环
		}
		return -1;
	}

	public void myClose() throws IOException {
		in.close();
	}
}
```
如果直接将byte（一个字节11111111）提升为int，结果如下：
11111111因为这个字节一整个8位的第一位是1（8位的第一个位是符号位），提升为int将前面都补全1
-->11111111 11111111 11111111 11111111（对应二进制中的-1）

**为了即保留原字节byte记录的这8位数据不变，又避免-1的出现，那我们就在前面补0**
**怎么补0呢？**
　11111111 11111111 11111111 11111111
& 00000000 00000000 00000000 11111111
\------------------------------------
　00000000 00000000 00000000 11111111

**结论：**
字节流的读一个字节的read方法为什么返回值类型不是byte，而是int。
因为有可能会读到连续8个二进制1的情况，8个二进制1转成int时对应的十进制是-1.
那么就会出现数据还没有读完，就结束的情况。因为我们判断读取结束是通过结尾标记-1来确定的。
所以，为了避免这种情况将读到的字节进行int类型的提升。
并在保留原字节数据的情况前面了补了24个0，变成了int类型的数值。
而在**写入数据**时，**只写**该int类型数据的**最低8位**。（即byte记录的8位）


## 标准输入输出流

System类中的字段：in，out。
它们各代表了系统标准的输入和输出设备。
默认输入设备是键盘，输出设备是显示器。
System.**out**:对应的是标准输出设备，**控制台**。
System.**in**:对应的标准输入设备：**键盘**。
**System.in**的**类型是InputStream**.

**System.out**的**类型是PrintStream**是**OutputStream的子类**FilterOutputStream的子类.

```
public class SystemDemo {
	public static void main(String[]args) throws IOException
	{
		System.out.println('a'+0); //a对应97
		System.out.println('\r'+0); //\r对应13
		System.out.println('\n'+0); //\r对应10
		InputStream in=System.in; //输入a和一个回车，所以打印出下面3个数字
		System.out.print(in.read()+" ");
		System.out.print(in.read()+" ");
		System.out.print(in.read()+" ");
	}
}
```
结果：
```
97
13
10
a
97 13 10 
```
### 习题：读取键盘录入
需求：通过键盘录入数据。当录入一行数据后，就将该行数据进行打印。如果录入的数据是over，那么停止录入。

```
public class ReadIn {
	public static void main(String[] args) throws IOException {
		InputStream in = System.in;
		StringBuilder sb = new StringBuilder();
		while (true) {
			int ch = in.read();
			if (ch == '\r')
				continue;
			if (ch == '\n') {
				String s = sb.toString();
				if ("over".equals(s))
					break;
				System.out.println(s.toUpperCase());
				sb.delete(0, sb.length());//清空sb
			} else
				sb.append((char) ch);
		}
	}
}
```

## 转换流
**InputStreamReader**, **OutputStreamWriter**

转换流的**由来**
• 字符流与字节流之间的桥梁
• 方便了字符流与字节流之间的操作

转换流的**应用**
• **字节流中的数据都是字符时，转成字符流操作更高效**。

### 标准输入输出流示例
因为是字节流处理的是文本数据，可以转换成字符流，操作更方便。
```
BufferedReader bufr = new BufferedReader(new InputStreamReader(System.in));
BufferedWriter bufw = new BufferedWriter(new
OutputStreamWriter(System.out));
```

例:获取键盘录入数据，然后将数据流向显示器，那么显示器就是目的地。
通过System类的setIn，setOut方法**对默认设备进行改变**。
• System.**setIn**(new FileInputStream(“1.txt”));//将源改成文件1.txt。
• System.**setOut**(new FileOutputStream(“2.txt”));//将目的改成文件2.txt

```
public class TransStreamDemo {
	public static void main(String[] args) throws IOException {	
		InputStream in = System.in;		//获取键盘录入对象		
		
		//将字节流对象转成字符流对象，使用转换流。InputStreamReader
		InputStreamReader isr = new InputStreamReader(in);
		
		//为了提高效率，将字符串进行缓冲区技术高效操作。使用BufferedReader							
		BufferedReader bufr = new BufferedReader(isr);
		
		//键盘的最常见写法 （上面三步合并为此）
//		BufferedReader bufr = new BufferedReader(new InputStreamReader(System.in));		
		
		BufferedWriter bufw = new BufferedWriter(new OutputStreamWriter(System.out));
		String line = null;
		while ((line = bufr.readLine()) != null) {
			if ("over".equals(line))
				break;
			bufw.write(line.toUpperCase());
			bufw.newLine();//每输出一行插入通用换行符
			bufw.flush();//刷新，否则无输出内容
		}
		bufr.close();
	}
}
```

## 流的基本应用小节
流是用来处理数据的。
处理数据时，一定**要先明确数据源，与数据目的地**。
数据源可以是文件，可以是键盘。
数据目的地可以是文件、显示器或者其他设备。
而流只是在帮助数据进行传输,并对传输的数据进行处理，比如过滤处理.转换处理等。

### 流操作的基本规律
**通过三个明确来完成流对象的选择**
**1，明确源和目的。**
**源**：**输入**流。InputStream Reader
**目的**：**输出**流。OutputStream Writer

**2，操作的数据是否是纯文本。**
**是**：**字符流**。
不是：字节流。

**3，当体系明确后，通过设备来进行区分**，**明确要使用哪个具体的对象。**
源设备：内存，硬盘。键盘
目的设备：内存，硬盘，控制台。

### 示例：
**1，将一个文本文件中数据存储到另一个文件中。复制文件。**
**源**：因为是源，所以使用读取流。InputStream Reader

是不是操作**文本**文件。
是！这时就可以选择Reader

这样体系就明确了。
接下来明确要使用该体系中的哪个对象。

明确**设备**：硬盘，一个文件。
Reader体系中可以操作文件的对象是 FileReader

是否需要提高效率：是！。加入Reader体系中缓冲区 BufferedReader.
FileReader fr = new FileReader("a.txt");
BufferedReader bufr =new BufferedReader(fr);

**目的**：OutputStream Writer

是否是**纯文本**。
是！Writer。

**设备**：硬盘，一个文件。
Writer体系中可以操作文件的对象FileWriter。

是否需要提高效率：是！。加入Writer体系中缓冲区 BufferedWriter
FileWriter fw = new FileWriter("b.txt");
BufferedWriter bufw = new BufferedWriter(fw);


**2，需求：将键盘录入的数据保存到一个文件中。**
这个需求中有源和目的都存在。
那么分别分析
源：InputStream Reader
是不是纯文本？是！Reader
设备：键盘。对应的对象是System.in.
不是选择Reader吗？System.in对应的不是字节流吗？
为了操作键盘的文本数据方便。**转成字符流**按照字符串操作是最方便的。
所以既然明确了Reader，那么就将System.in转换成Reader。
用了Reader体系中转换流,InputStreamReader
InputStreamReader isr = new InputStreamReader(System.in);
需要提高效率吗？需要！BufferedReader
BufferedReader bufr = new BufferedReader(isr);
目的：OutputStream Writer
是否是存文本？是！Writer。
设备：硬盘。一个文件。使用 FileWriter。
FileWriter fw = **new** FileWriter("c.txt");
需要提高效率吗？需要。
BufferedWriter bufw = **new** BufferedWriter(fw);

**3.想要把录入的数据按照指定的编码表（utf-8），将数据存到文件中**。
目的：OutputStream Writer
是否是存文本？是！Writer。
设备：硬盘。一个文件。使用 FileWriter。
但是FileWriter是使用的默认编码表。GBK.
但是存储时，需要加入指定编码表utf-8。而**指定的编码表只有转换流可以指定**。
所以要使用的对象是OutputStreamWriter。
而该转换流对象要接收一个字节输出流。而且还可以操作的文件的字节输出流。FileOutputStream
OutputStreamWriter osw = new OutputStreamWriter(new
FileOutputStream("d.txt"),"UTF-8");
需要高效吗？需要。
BufferedWriter bufw = **new** BufferedWriter(osw);
所以，记住。转换流什么时候使用。字符和字节之间的桥梁，**通常，涉及到字符编码转换时，需要用到转换流**。


#### 习题：生成异常日志文件
```
public class ExceptionInfo {
	public static void main(String[] args) throws IOException {		
		try {
			int[] arr = new int[2];
			System.out.println(arr[3]);//制造一个异常
		} catch (Exception e) {
			try {
				Date d = new Date();
				SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
				String s = sdf.format(d);
				PrintStream ps = new PrintStream("exeception.log");
				
				System.setOut(ps);//设置输出到指定文件
				
				ps.println(s); //输出日期
			} catch (IOException ex) {
				throw new RuntimeException("日志文件创建失败");
			}
			e.printStackTrace(System.out); //输出异常信息
		}
	}
}
```

#### 习题：生成系统日志文件
```
public class SystemInfo {
	public static void main(String[] args) throws IOException {
		Properties prop = System.getProperties();
		//System.out.println(prop);
		prop.list(new PrintStream("sysinfo.txt"));
	}
}
```

### 字符流继承体系简图
![](http://img.blog.csdn.net/20171225164900577?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### 字节流继承体系简图
![](http://img.blog.csdn.net/20171225164952626?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

