<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [【JAVA 运算符】](#java-%E8%BF%90%E7%AE%97%E7%AC%A6)
  - [1 算术运算符](#1-%E7%AE%97%E6%9C%AF%E8%BF%90%E7%AE%97%E7%AC%A6)
    - [**转义字符**](#%E8%BD%AC%E4%B9%89%E5%AD%97%E7%AC%A6)
    - [算术运算符的注意问题](#%E7%AE%97%E6%9C%AF%E8%BF%90%E7%AE%97%E7%AC%A6%E7%9A%84%E6%B3%A8%E6%84%8F%E9%97%AE%E9%A2%98)
  - [2 赋值运算符](#2-%E8%B5%8B%E5%80%BC%E8%BF%90%E7%AE%97%E7%AC%A6)
  - [3 比较运算符](#3-%E6%AF%94%E8%BE%83%E8%BF%90%E7%AE%97%E7%AC%A6)
  - [4 逻辑运算符](#4-%E9%80%BB%E8%BE%91%E8%BF%90%E7%AE%97%E7%AC%A6)
  - [5 位运算符](#5-%E4%BD%8D%E8%BF%90%E7%AE%97%E7%AC%A6)
    - [左移右移](#%E5%B7%A6%E7%A7%BB%E5%8F%B3%E7%A7%BB)
    - [^异或运算](#%5E%E5%BC%82%E6%88%96%E8%BF%90%E7%AE%97)
    - [~ 非](#-%E9%9D%9E)
        - [附：负数计算](#%E9%99%84%E8%B4%9F%E6%95%B0%E8%AE%A1%E7%AE%97)
    - [位运算符练习：](#%E4%BD%8D%E8%BF%90%E7%AE%97%E7%AC%A6%E7%BB%83%E4%B9%A0)
      - [1.最有效率的方式算出2乘以8等于几？](#1%E6%9C%80%E6%9C%89%E6%95%88%E7%8E%87%E7%9A%84%E6%96%B9%E5%BC%8F%E7%AE%97%E5%87%BA2%E4%B9%98%E4%BB%A58%E7%AD%89%E4%BA%8E%E5%87%A0)
      - [2.**对两个整数变量的值进行互换**(不需要第三方变量)](#2%E5%AF%B9%E4%B8%A4%E4%B8%AA%E6%95%B4%E6%95%B0%E5%8F%98%E9%87%8F%E7%9A%84%E5%80%BC%E8%BF%9B%E8%A1%8C%E4%BA%92%E6%8D%A2%E4%B8%8D%E9%9C%80%E8%A6%81%E7%AC%AC%E4%B8%89%E6%96%B9%E5%8F%98%E9%87%8F)
  - [6 三元运算符](#6-%E4%B8%89%E5%85%83%E8%BF%90%E7%AE%97%E7%AC%A6)
- [【JAVA 集合类]](#java-%E9%9B%86%E5%90%88%E7%B1%BB)
  - [为什么出现集合类？](#%E4%B8%BA%E4%BB%80%E4%B9%88%E5%87%BA%E7%8E%B0%E9%9B%86%E5%90%88%E7%B1%BB)
  - [数组和集合类同是容器，有何不同？](#%E6%95%B0%E7%BB%84%E5%92%8C%E9%9B%86%E5%90%88%E7%B1%BB%E5%90%8C%E6%98%AF%E5%AE%B9%E5%99%A8%E6%9C%89%E4%BD%95%E4%B8%8D%E5%90%8C)
  - [集合类的特点](#%E9%9B%86%E5%90%88%E7%B1%BB%E7%9A%84%E7%89%B9%E7%82%B9)
  - [集合体系](#%E9%9B%86%E5%90%88%E4%BD%93%E7%B3%BB)
    - [Collection](#collection)
    - [Map](#map)
    - [集合类关系图](#%E9%9B%86%E5%90%88%E7%B1%BB%E5%85%B3%E7%B3%BB%E5%9B%BE)
    - [Set、List和Map可以看做集合的三大类。](#setlist%E5%92%8Cmap%E5%8F%AF%E4%BB%A5%E7%9C%8B%E5%81%9A%E9%9B%86%E5%90%88%E7%9A%84%E4%B8%89%E5%A4%A7%E7%B1%BB)
  - [Collection](#collection-1)
    - [List vs Set](#list-vs-set)
    - [TreeSet排序的方式：](#treeset%E6%8E%92%E5%BA%8F%E7%9A%84%E6%96%B9%E5%BC%8F)
    - [List特有方法](#list%E7%89%B9%E6%9C%89%E6%96%B9%E6%B3%95)
      - [**LinkedList**:](#linkedlist)
    - [实例](#%E5%AE%9E%E4%BE%8B)
      - [Collection](#collection-2)
      - [**习题：使用LinkedList模拟一个堆栈或者队列数据结构**](#%E4%B9%A0%E9%A2%98%E4%BD%BF%E7%94%A8linkedlist%E6%A8%A1%E6%8B%9F%E4%B8%80%E4%B8%AA%E5%A0%86%E6%A0%88%E6%88%96%E8%80%85%E9%98%9F%E5%88%97%E6%95%B0%E6%8D%AE%E7%BB%93%E6%9E%84)
      - [习题：**去除ArrayList集合中的重复元素**](#%E4%B9%A0%E9%A2%98%E5%8E%BB%E9%99%A4arraylist%E9%9B%86%E5%90%88%E4%B8%AD%E7%9A%84%E9%87%8D%E5%A4%8D%E5%85%83%E7%B4%A0)
      - [习题：**将自定义对象作为元素存到ArrayList集合中，并去除重复元素**](#%E4%B9%A0%E9%A2%98%E5%B0%86%E8%87%AA%E5%AE%9A%E4%B9%89%E5%AF%B9%E8%B1%A1%E4%BD%9C%E4%B8%BA%E5%85%83%E7%B4%A0%E5%AD%98%E5%88%B0arraylist%E9%9B%86%E5%90%88%E4%B8%AD%E5%B9%B6%E5%8E%BB%E9%99%A4%E9%87%8D%E5%A4%8D%E5%85%83%E7%B4%A0)
      - [HashSet](#hashset)
      - [习题：**往hashSet集合中存入自定对象，姓名和年龄相同不重复存**](#%E4%B9%A0%E9%A2%98%E5%BE%80hashset%E9%9B%86%E5%90%88%E4%B8%AD%E5%AD%98%E5%85%A5%E8%87%AA%E5%AE%9A%E5%AF%B9%E8%B1%A1%E5%A7%93%E5%90%8D%E5%92%8C%E5%B9%B4%E9%BE%84%E7%9B%B8%E5%90%8C%E4%B8%8D%E9%87%8D%E5%A4%8D%E5%AD%98)
      - [习题：往TreeSet集合中存储自定义对象学生，按照年龄排序](#%E4%B9%A0%E9%A2%98%E5%BE%80treeset%E9%9B%86%E5%90%88%E4%B8%AD%E5%AD%98%E5%82%A8%E8%87%AA%E5%AE%9A%E4%B9%89%E5%AF%B9%E8%B1%A1%E5%AD%A6%E7%94%9F%E6%8C%89%E7%85%A7%E5%B9%B4%E9%BE%84%E6%8E%92%E5%BA%8F)
      - [习题：往TreeSet集合中存储自定义对象学生，通过比较器按录入顺序倒序](#%E4%B9%A0%E9%A2%98%E5%BE%80treeset%E9%9B%86%E5%90%88%E4%B8%AD%E5%AD%98%E5%82%A8%E8%87%AA%E5%AE%9A%E4%B9%89%E5%AF%B9%E8%B1%A1%E5%AD%A6%E7%94%9F%E9%80%9A%E8%BF%87%E6%AF%94%E8%BE%83%E5%99%A8%E6%8C%89%E5%BD%95%E5%85%A5%E9%A1%BA%E5%BA%8F%E5%80%92%E5%BA%8F)
      - [习题：使用比较器，按照字符串长度排序](#%E4%B9%A0%E9%A2%98%E4%BD%BF%E7%94%A8%E6%AF%94%E8%BE%83%E5%99%A8%E6%8C%89%E7%85%A7%E5%AD%97%E7%AC%A6%E4%B8%B2%E9%95%BF%E5%BA%A6%E6%8E%92%E5%BA%8F)
    - [](#)
  - [Map集合](#map%E9%9B%86%E5%90%88)
    - [Map集合常用类](#map%E9%9B%86%E5%90%88%E5%B8%B8%E7%94%A8%E7%B1%BB)
      - [Hashtable](#hashtable)
      - [HashMap](#hashmap)
      - [TreeMap](#treemap)
    - [map集合的两种取出方式：](#map%E9%9B%86%E5%90%88%E7%9A%84%E4%B8%A4%E7%A7%8D%E5%8F%96%E5%87%BA%E6%96%B9%E5%BC%8F)
      - [1，keySet](#1keyset)
      - [2，entrySet](#2entryset)
      - [keySet方法图例](#keyset%E6%96%B9%E6%B3%95%E5%9B%BE%E4%BE%8B)
      - [entrySet方法图例](#entryset%E6%96%B9%E6%B3%95%E5%9B%BE%E4%BE%8B)
      - [实例](#%E5%AE%9E%E4%BE%8B-1)
        - [习题：每一个学生都有对应的归属地](#%E4%B9%A0%E9%A2%98%E6%AF%8F%E4%B8%80%E4%B8%AA%E5%AD%A6%E7%94%9F%E9%83%BD%E6%9C%89%E5%AF%B9%E5%BA%94%E7%9A%84%E5%BD%92%E5%B1%9E%E5%9C%B0)
        - [习题：对学生对象的姓名进行升序排序(上题中学生对象)](#%E4%B9%A0%E9%A2%98%E5%AF%B9%E5%AD%A6%E7%94%9F%E5%AF%B9%E8%B1%A1%E7%9A%84%E5%A7%93%E5%90%8D%E8%BF%9B%E8%A1%8C%E5%8D%87%E5%BA%8F%E6%8E%92%E5%BA%8F%E4%B8%8A%E9%A2%98%E4%B8%AD%E5%AD%A6%E7%94%9F%E5%AF%B9%E8%B1%A1)
        - [习题："ak+abAf1c,dCkaAbc-defa"获取该字符串中的字母出现的次数希望打印结果：a(4) b(2)c(2).....](#%E4%B9%A0%E9%A2%98akabaf1cdckaabc-defa%E8%8E%B7%E5%8F%96%E8%AF%A5%E5%AD%97%E7%AC%A6%E4%B8%B2%E4%B8%AD%E7%9A%84%E5%AD%97%E6%AF%8D%E5%87%BA%E7%8E%B0%E7%9A%84%E6%AC%A1%E6%95%B0%E5%B8%8C%E6%9C%9B%E6%89%93%E5%8D%B0%E7%BB%93%E6%9E%9Ca4-b2c2)
    - [map扩展知识：多映射关系](#map%E6%89%A9%E5%B1%95%E7%9F%A5%E8%AF%86%E5%A4%9A%E6%98%A0%E5%B0%84%E5%85%B3%E7%B3%BB)
  - [Map vs Collection](#map-vs-collection)
  - [集合框架中的工具类](#%E9%9B%86%E5%90%88%E6%A1%86%E6%9E%B6%E4%B8%AD%E7%9A%84%E5%B7%A5%E5%85%B7%E7%B1%BB)
    - [Collections](#collections)
      - [Collections和Collection有什么区别？](#collections%E5%92%8Ccollection%E6%9C%89%E4%BB%80%E4%B9%88%E5%8C%BA%E5%88%AB)
      - [Collections方法实例](#collections%E6%96%B9%E6%B3%95%E5%AE%9E%E4%BE%8B)
        - [sort、binarySearch、max](#sortbinarysearchmax)
        - [replaceAll、reverse、shuffle、fill、reverseOrder](#replaceallreverseshufflefillreverseorder)
        - [习题：用fill将list集合中部分元素替换成指定元素](#%E4%B9%A0%E9%A2%98%E7%94%A8fill%E5%B0%86list%E9%9B%86%E5%90%88%E4%B8%AD%E9%83%A8%E5%88%86%E5%85%83%E7%B4%A0%E6%9B%BF%E6%8D%A2%E6%88%90%E6%8C%87%E5%AE%9A%E5%85%83%E7%B4%A0)
    - [Arrays](#arrays)
      - [Arrays.asList](#arraysaslist)
      - [Collection.toArray](#collectiontoarray)
- [迭代器Iterator](#%E8%BF%AD%E4%BB%A3%E5%99%A8iterator)
  - [主要方法：](#%E4%B8%BB%E8%A6%81%E6%96%B9%E6%B3%95)
  - [用法：](#%E7%94%A8%E6%B3%95)
    - [实例](#%E5%AE%9E%E4%BE%8B-2)
  - [迭代器内部实现](#%E8%BF%AD%E4%BB%A3%E5%99%A8%E5%86%85%E9%83%A8%E5%AE%9E%E7%8E%B0)
  - [迭代注意事项](#%E8%BF%AD%E4%BB%A3%E6%B3%A8%E6%84%8F%E4%BA%8B%E9%A1%B9)
  - [列表迭代器ListIterator](#%E5%88%97%E8%A1%A8%E8%BF%AD%E4%BB%A3%E5%99%A8listiterator)
  - [枚举Enumeration](#%E6%9E%9A%E4%B8%BEenumeration)
- [for each循环](#for-each%E5%BE%AA%E7%8E%AF)
- [方法的可变参数](#%E6%96%B9%E6%B3%95%E7%9A%84%E5%8F%AF%E5%8F%98%E5%8F%82%E6%95%B0)
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
    - [实例](#%E5%AE%9E%E4%BE%8B-3)
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
- [【JAVA 线程】](#java-%E7%BA%BF%E7%A8%8B)
- [JAVA 线程基础](#java-%E7%BA%BF%E7%A8%8B%E5%9F%BA%E7%A1%80)
  - [程序 VS 进程 VS 线程](#%E7%A8%8B%E5%BA%8F-vs-%E8%BF%9B%E7%A8%8B-vs-%E7%BA%BF%E7%A8%8B)
    - [程序 VS 进程](#%E7%A8%8B%E5%BA%8F-vs-%E8%BF%9B%E7%A8%8B)
      - [程序和进程之间的主要区别：](#%E7%A8%8B%E5%BA%8F%E5%92%8C%E8%BF%9B%E7%A8%8B%E4%B9%8B%E9%97%B4%E7%9A%84%E4%B8%BB%E8%A6%81%E5%8C%BA%E5%88%AB)
      - [进程的基本状态:](#%E8%BF%9B%E7%A8%8B%E7%9A%84%E5%9F%BA%E6%9C%AC%E7%8A%B6%E6%80%81)
    - [线程](#%E7%BA%BF%E7%A8%8B)
      - [Java中的线程要经历4个过程](#java%E4%B8%AD%E7%9A%84%E7%BA%BF%E7%A8%8B%E8%A6%81%E7%BB%8F%E5%8E%864%E4%B8%AA%E8%BF%87%E7%A8%8B)
    - [](#-1)
  - [线程的创建方式](#%E7%BA%BF%E7%A8%8B%E7%9A%84%E5%88%9B%E5%BB%BA%E6%96%B9%E5%BC%8F)
    - [创建线程方式一：继承Thread类](#%E5%88%9B%E5%BB%BA%E7%BA%BF%E7%A8%8B%E6%96%B9%E5%BC%8F%E4%B8%80%E7%BB%A7%E6%89%BFthread%E7%B1%BB)
      - [习题：创建两个线程，和主线程交替运行](#%E4%B9%A0%E9%A2%98%E5%88%9B%E5%BB%BA%E4%B8%A4%E4%B8%AA%E7%BA%BF%E7%A8%8B%E5%92%8C%E4%B8%BB%E7%BA%BF%E7%A8%8B%E4%BA%A4%E6%9B%BF%E8%BF%90%E8%A1%8C)
    - [线程的四种状态](#%E7%BA%BF%E7%A8%8B%E7%9A%84%E5%9B%9B%E7%A7%8D%E7%8A%B6%E6%80%81)
    - [创建线程方式二：实现Runnable接口](#%E5%88%9B%E5%BB%BA%E7%BA%BF%E7%A8%8B%E6%96%B9%E5%BC%8F%E4%BA%8C%E5%AE%9E%E7%8E%B0runnable%E6%8E%A5%E5%8F%A3)
  - [线程安全问题](#%E7%BA%BF%E7%A8%8B%E5%AE%89%E5%85%A8%E9%97%AE%E9%A2%98)
  - [同步（synchronized）](#%E5%90%8C%E6%AD%A5synchronized)
    - [同步的特点](#%E5%90%8C%E6%AD%A5%E7%9A%84%E7%89%B9%E7%82%B9)
      - [习题：简单的卖票程序，多个窗口同时买票。](#%E4%B9%A0%E9%A2%98%E7%AE%80%E5%8D%95%E7%9A%84%E5%8D%96%E7%A5%A8%E7%A8%8B%E5%BA%8F%E5%A4%9A%E4%B8%AA%E7%AA%97%E5%8F%A3%E5%90%8C%E6%97%B6%E4%B9%B0%E7%A5%A8)
    - [同步函数](#%E5%90%8C%E6%AD%A5%E5%87%BD%E6%95%B0)
      - [习题：银行存储](#%E4%B9%A0%E9%A2%98%E9%93%B6%E8%A1%8C%E5%AD%98%E5%82%A8)
    - [死锁](#%E6%AD%BB%E9%94%81)
      - [习题：写一个死锁程序](#%E4%B9%A0%E9%A2%98%E5%86%99%E4%B8%80%E4%B8%AA%E6%AD%BB%E9%94%81%E7%A8%8B%E5%BA%8F)
    - [](#-2)
  - [线程间通信](#%E7%BA%BF%E7%A8%8B%E9%97%B4%E9%80%9A%E4%BF%A1)
      - [习题：交替输入输出姓名性别（2线程）](#%E4%B9%A0%E9%A2%98%E4%BA%A4%E6%9B%BF%E8%BE%93%E5%85%A5%E8%BE%93%E5%87%BA%E5%A7%93%E5%90%8D%E6%80%A7%E5%88%AB2%E7%BA%BF%E7%A8%8B)
      - [习题：交替生产消费商品（多线程，为商品编号）](#%E4%B9%A0%E9%A2%98%E4%BA%A4%E6%9B%BF%E7%94%9F%E4%BA%A7%E6%B6%88%E8%B4%B9%E5%95%86%E5%93%81%E5%A4%9A%E7%BA%BF%E7%A8%8B%E4%B8%BA%E5%95%86%E5%93%81%E7%BC%96%E5%8F%B7)
  - [多线程升级解决方案](#%E5%A4%9A%E7%BA%BF%E7%A8%8B%E5%8D%87%E7%BA%A7%E8%A7%A3%E5%86%B3%E6%96%B9%E6%A1%88)
      - [**习题：交替生产消费**（用Lock，实现本方只唤醒对方操作）](#%E4%B9%A0%E9%A2%98%E4%BA%A4%E6%9B%BF%E7%94%9F%E4%BA%A7%E6%B6%88%E8%B4%B9%E7%94%A8lock%E5%AE%9E%E7%8E%B0%E6%9C%AC%E6%96%B9%E5%8F%AA%E5%94%A4%E9%86%92%E5%AF%B9%E6%96%B9%E6%93%8D%E4%BD%9C)
  - [停止线程](#%E5%81%9C%E6%AD%A2%E7%BA%BF%E7%A8%8B)
      - [习题：停止线程（分别用循环结束标记、interrupt、setDaemon）](#%E4%B9%A0%E9%A2%98%E5%81%9C%E6%AD%A2%E7%BA%BF%E7%A8%8B%E5%88%86%E5%88%AB%E7%94%A8%E5%BE%AA%E7%8E%AF%E7%BB%93%E6%9D%9F%E6%A0%87%E8%AE%B0interruptsetdaemon)
  - [线程类的其他方法](#%E7%BA%BF%E7%A8%8B%E7%B1%BB%E7%9A%84%E5%85%B6%E4%BB%96%E6%96%B9%E6%B3%95)
    - [setDaemon(boolean on)](#setdaemonboolean-on)
    - [join()](#join)
    - [setPriority(int newPriority)](#setpriorityint-newpriority)
    - [yield()](#yield)
    - [toString()](#tostring)
      - [习题：运用toString、yield、setPriority 、join](#%E4%B9%A0%E9%A2%98%E8%BF%90%E7%94%A8tostringyieldsetpriority-join)
    - [](#-3)
  - [线程总结：](#%E7%BA%BF%E7%A8%8B%E6%80%BB%E7%BB%93)
    - [线程间通信](#%E7%BA%BF%E7%A8%8B%E9%97%B4%E9%80%9A%E4%BF%A1-1)
    - [停止线程：](#%E5%81%9C%E6%AD%A2%E7%BA%BF%E7%A8%8B)
    - [线程中一些常见方法：](#%E7%BA%BF%E7%A8%8B%E4%B8%AD%E4%B8%80%E4%BA%9B%E5%B8%B8%E8%A7%81%E6%96%B9%E6%B3%95)
    - [线程状态转换](#%E7%BA%BF%E7%A8%8B%E7%8A%B6%E6%80%81%E8%BD%AC%E6%8D%A2)
    - [多线程重点：](#%E5%A4%9A%E7%BA%BF%E7%A8%8B%E9%87%8D%E7%82%B9)
- [JAVA 线程池](#java-%E7%BA%BF%E7%A8%8B%E6%B1%A0)
  - [简述线程池](#%E7%AE%80%E8%BF%B0%E7%BA%BF%E7%A8%8B%E6%B1%A0)
  - [线程池的好处](#%E7%BA%BF%E7%A8%8B%E6%B1%A0%E7%9A%84%E5%A5%BD%E5%A4%84)
  - [ThreadPoolExecutor](#threadpoolexecutor)
    - [ThreadPoolExecutor逻辑结构](#threadpoolexecutor%E9%80%BB%E8%BE%91%E7%BB%93%E6%9E%84)
    - [ThreadPoolExecutor工作方式](#threadpoolexecutor%E5%B7%A5%E4%BD%9C%E6%96%B9%E5%BC%8F)
    - [ThreadPoolExecutor不常用的设置](#threadpoolexecutor%E4%B8%8D%E5%B8%B8%E7%94%A8%E7%9A%84%E8%AE%BE%E7%BD%AE)
      - [allowCoreThreadTimeOut：](#allowcorethreadtimeout)
      - [prestartAllCoreThreads](#prestartallcorethreads)
    - [ThreadFactory的使用](#threadfactory%E7%9A%84%E4%BD%BF%E7%94%A8)
    - [线程池的等待队列](#%E7%BA%BF%E7%A8%8B%E6%B1%A0%E7%9A%84%E7%AD%89%E5%BE%85%E9%98%9F%E5%88%97)
      - [队列和栈](#%E9%98%9F%E5%88%97%E5%92%8C%E6%A0%88)
      - [1. 有限队列](#1-%E6%9C%89%E9%99%90%E9%98%9F%E5%88%97)
      - [2.无限队列](#2%E6%97%A0%E9%99%90%E9%98%9F%E5%88%97)
    - [拒绝任务（handler）](#%E6%8B%92%E7%BB%9D%E4%BB%BB%E5%8A%A1handler)
  - [常用的几种线程池](#%E5%B8%B8%E7%94%A8%E7%9A%84%E5%87%A0%E7%A7%8D%E7%BA%BF%E7%A8%8B%E6%B1%A0)
    - [1. newCachedThreadPool](#1-newcachedthreadpool)
    - [2. newFixedThreadPool](#2-newfixedthreadpool)
    - [3. newSingleThreadExecutor](#3-newsinglethreadexecutor)
    - [4. newScheduleThreadPool](#4-newschedulethreadpool)
- [【JAVA RxJava】](#java-rxjava)
  - [RxJava 到底是什么](#rxjava-%E5%88%B0%E5%BA%95%E6%98%AF%E4%BB%80%E4%B9%88)
  - [RxJava 好在哪](#rxjava-%E5%A5%BD%E5%9C%A8%E5%93%AA)
  - [API介绍和原理简析](#api%E4%BB%8B%E7%BB%8D%E5%92%8C%E5%8E%9F%E7%90%86%E7%AE%80%E6%9E%90)
    - [1. 概念：扩展的观察者模式](#1-%E6%A6%82%E5%BF%B5%E6%89%A9%E5%B1%95%E7%9A%84%E8%A7%82%E5%AF%9F%E8%80%85%E6%A8%A1%E5%BC%8F)
      - [观察者模式](#%E8%A7%82%E5%AF%9F%E8%80%85%E6%A8%A1%E5%BC%8F)
      - [RxJava 的观察者模式](#rxjava-%E7%9A%84%E8%A7%82%E5%AF%9F%E8%80%85%E6%A8%A1%E5%BC%8F)
        - [onCompleted和onError](#oncompleted%E5%92%8Conerror)
      - [](#-4)
    - [2. 基本实现](#2-%E5%9F%BA%E6%9C%AC%E5%AE%9E%E7%8E%B0)
      - [1) 创建 Observer](#1-%E5%88%9B%E5%BB%BA-observer)
        - [Subscriber](#subscriber)
        - [Observer  vs  Subscriber](#observer--vs--subscriber)
      - [2) 创建 Observable](#2-%E5%88%9B%E5%BB%BA-observable)
        - [创建事件队列方法](#%E5%88%9B%E5%BB%BA%E4%BA%8B%E4%BB%B6%E9%98%9F%E5%88%97%E6%96%B9%E6%B3%95)
          - [**create**()](#create)
          - [**just**(T...)](#justt)
          - [**from**(T[]) / from(Iterable<? extends T>) :](#fromt--fromiterable-extends-t-)
        - [](#-5)
      - [3) Subscribe (订阅)](#3-subscribe-%E8%AE%A2%E9%98%85)
        - [**Observable.subscribe(Subscriber)** 的**内部实现**：](#observablesubscribesubscriber-%E7%9A%84%E5%86%85%E9%83%A8%E5%AE%9E%E7%8E%B0)
        - [不完整定义的回调](#%E4%B8%8D%E5%AE%8C%E6%95%B4%E5%AE%9A%E4%B9%89%E7%9A%84%E5%9B%9E%E8%B0%83)
          - [**Action0**](#action0)
          - [**Action1**](#action1)
        - [](#-6)
      - [4) 场景示例](#4-%E5%9C%BA%E6%99%AF%E7%A4%BA%E4%BE%8B)
        - [a. 打印字符串数组](#a-%E6%89%93%E5%8D%B0%E5%AD%97%E7%AC%A6%E4%B8%B2%E6%95%B0%E7%BB%84)
        - [b. 由 id 取得图片并显示](#b-%E7%94%B1-id-%E5%8F%96%E5%BE%97%E5%9B%BE%E7%89%87%E5%B9%B6%E6%98%BE%E7%A4%BA)
      - [](#-7)
    - [3. 线程控制 —— Scheduler (一)](#3-%E7%BA%BF%E7%A8%8B%E6%8E%A7%E5%88%B6--scheduler-%E4%B8%80)
      - [1) Scheduler 的 API (一)](#1-scheduler-%E7%9A%84-api-%E4%B8%80)
        - [**Schedulers.immediate**():](#schedulersimmediate)
        - [**Schedulers.newThread**():](#schedulersnewthread)
        - [**Schedulers.io**():](#schedulersio)
        - [**Schedulers.computation**():](#schedulerscomputation)
        - [**AndroidSchedulers.mainThread**()](#androidschedulersmainthread)
        - [**subscribeOn**():](#subscribeon)
        - [**observeOn**():](#observeon)
      - [](#-8)
    - [4. 变换](#4-%E5%8F%98%E6%8D%A2)
      - [1) API](#1-api)
        - [map()](#map)
          - [map() 的示意图：](#map-%E7%9A%84%E7%A4%BA%E6%84%8F%E5%9B%BE)
        - [flatMap()](#flatmap)
          - [flatMap() 原理：](#flatmap-%E5%8E%9F%E7%90%86)
        - [throttleFirst():](#throttlefirst)
      - [2) 变换的原理：lift()](#2-%E5%8F%98%E6%8D%A2%E7%9A%84%E5%8E%9F%E7%90%86lift)
      - [3) compose: 对 Observable 整体的变换](#3-compose-%E5%AF%B9-observable-%E6%95%B4%E4%BD%93%E7%9A%84%E5%8F%98%E6%8D%A2)
    - [5. 线程控制：Scheduler (二)](#5-%E7%BA%BF%E7%A8%8B%E6%8E%A7%E5%88%B6scheduler-%E4%BA%8C)
      - [1) Scheduler 的 API (二)](#1-scheduler-%E7%9A%84-api-%E4%BA%8C)
      - [2) Scheduler 的原理（二）](#2-scheduler-%E7%9A%84%E5%8E%9F%E7%90%86%E4%BA%8C)
      - [3) 延伸：doOnSubscribe()](#3-%E5%BB%B6%E4%BC%B8doonsubscribe)
    - [](#-9)
  - [RxJava 的适用场景和使用方式](#rxjava-%E7%9A%84%E9%80%82%E7%94%A8%E5%9C%BA%E6%99%AF%E5%92%8C%E4%BD%BF%E7%94%A8%E6%96%B9%E5%BC%8F)
    - [1. 与 Retrofit 的结合](#1-%E4%B8%8E-retrofit-%E7%9A%84%E7%BB%93%E5%90%88)
    - [2. RxBinding](#2-rxbinding)
    - [3. 各种异步操作](#3-%E5%90%84%E7%A7%8D%E5%BC%82%E6%AD%A5%E6%93%8D%E4%BD%9C)
    - [4. RxBus](#4-rxbus)
  - [操作符的使用](#%E6%93%8D%E4%BD%9C%E7%AC%A6%E7%9A%84%E4%BD%BF%E7%94%A8)
    - [merge操作符，合并观察对象](#merge%E6%93%8D%E4%BD%9C%E7%AC%A6%E5%90%88%E5%B9%B6%E8%A7%82%E5%AF%9F%E5%AF%B9%E8%B1%A1)
    - [zip  操作符，合并多个观察对象的数据。并且允许 Func2（）函数重新发送合并后的数据](#zip--%E6%93%8D%E4%BD%9C%E7%AC%A6%E5%90%88%E5%B9%B6%E5%A4%9A%E4%B8%AA%E8%A7%82%E5%AF%9F%E5%AF%B9%E8%B1%A1%E7%9A%84%E6%95%B0%E6%8D%AE%E5%B9%B6%E4%B8%94%E5%85%81%E8%AE%B8-func2%E5%87%BD%E6%95%B0%E9%87%8D%E6%96%B0%E5%8F%91%E9%80%81%E5%90%88%E5%B9%B6%E5%90%8E%E7%9A%84%E6%95%B0%E6%8D%AE)
    - [scan累加器操作符](#scan%E7%B4%AF%E5%8A%A0%E5%99%A8%E6%93%8D%E4%BD%9C%E7%AC%A6)
    - [filter 过滤操作符的使用](#filter-%E8%BF%87%E6%BB%A4%E6%93%8D%E4%BD%9C%E7%AC%A6%E7%9A%84%E4%BD%BF%E7%94%A8)
    - [消息数量过滤操作符的使用](#%E6%B6%88%E6%81%AF%E6%95%B0%E9%87%8F%E8%BF%87%E6%BB%A4%E6%93%8D%E4%BD%9C%E7%AC%A6%E7%9A%84%E4%BD%BF%E7%94%A8)
      - [take ：取前n个数据](#take-%E5%8F%96%E5%89%8Dn%E4%B8%AA%E6%95%B0%E6%8D%AE)
      - [takeLast：取后n个数据](#takelast%E5%8F%96%E5%90%8En%E4%B8%AA%E6%95%B0%E6%8D%AE)
      - [first 只发送第一个数据](#first-%E5%8F%AA%E5%8F%91%E9%80%81%E7%AC%AC%E4%B8%80%E4%B8%AA%E6%95%B0%E6%8D%AE)
      - [last 只发送最后一个数据](#last-%E5%8F%AA%E5%8F%91%E9%80%81%E6%9C%80%E5%90%8E%E4%B8%80%E4%B8%AA%E6%95%B0%E6%8D%AE)
      - [skip() 跳过前n个数据发送后面的数据](#skip-%E8%B7%B3%E8%BF%87%E5%89%8Dn%E4%B8%AA%E6%95%B0%E6%8D%AE%E5%8F%91%E9%80%81%E5%90%8E%E9%9D%A2%E7%9A%84%E6%95%B0%E6%8D%AE)
      - [skipLast() 跳过最后n个数据，发送前面的数据](#skiplast-%E8%B7%B3%E8%BF%87%E6%9C%80%E5%90%8En%E4%B8%AA%E6%95%B0%E6%8D%AE%E5%8F%91%E9%80%81%E5%89%8D%E9%9D%A2%E7%9A%84%E6%95%B0%E6%8D%AE)
    - [elementAt 、elementAtOrDefault发送数据序列中第n个数据](#elementat-elementatordefault%E5%8F%91%E9%80%81%E6%95%B0%E6%8D%AE%E5%BA%8F%E5%88%97%E4%B8%AD%E7%AC%ACn%E4%B8%AA%E6%95%B0%E6%8D%AE)
    - [startWith() 插入数据](#startwith-%E6%8F%92%E5%85%A5%E6%95%B0%E6%8D%AE)
    - [delay操作符，延迟数据发送](#delay%E6%93%8D%E4%BD%9C%E7%AC%A6%E5%BB%B6%E8%BF%9F%E6%95%B0%E6%8D%AE%E5%8F%91%E9%80%81)
    - [Timer  延时操作符的使用](#timer--%E5%BB%B6%E6%97%B6%E6%93%8D%E4%BD%9C%E7%AC%A6%E7%9A%84%E4%BD%BF%E7%94%A8)
      - [delay vs timer](#delay-vs-timer)
    - [interval 轮询操作符，循环发送数据，数据从0开始递增](#interval-%E8%BD%AE%E8%AF%A2%E6%93%8D%E4%BD%9C%E7%AC%A6%E5%BE%AA%E7%8E%AF%E5%8F%91%E9%80%81%E6%95%B0%E6%8D%AE%E6%95%B0%E6%8D%AE%E4%BB%8E0%E5%BC%80%E5%A7%8B%E9%80%92%E5%A2%9E)
    - [doOnNext() 操作符，在每次 OnNext() 方法被调用前执行](#doonnext-%E6%93%8D%E4%BD%9C%E7%AC%A6%E5%9C%A8%E6%AF%8F%E6%AC%A1-onnext-%E6%96%B9%E6%B3%95%E8%A2%AB%E8%B0%83%E7%94%A8%E5%89%8D%E6%89%A7%E8%A1%8C)
    - [Buffer 操作符](#buffer-%E6%93%8D%E4%BD%9C%E7%AC%A6)
    - [throttleFirst 操作符在一段时间内，只取第一个事件，然后其他事件都丢弃。](#throttlefirst-%E6%93%8D%E4%BD%9C%E7%AC%A6%E5%9C%A8%E4%B8%80%E6%AE%B5%E6%97%B6%E9%97%B4%E5%86%85%E5%8F%AA%E5%8F%96%E7%AC%AC%E4%B8%80%E4%B8%AA%E4%BA%8B%E4%BB%B6%E7%84%B6%E5%90%8E%E5%85%B6%E4%BB%96%E4%BA%8B%E4%BB%B6%E9%83%BD%E4%B8%A2%E5%BC%83)
    - [14、distinct    过滤重复的数据](#14distinct----%E8%BF%87%E6%BB%A4%E9%87%8D%E5%A4%8D%E7%9A%84%E6%95%B0%E6%8D%AE)
    - [debounce() 操作符一段时间内没有变化，就会发送一个数据](#debounce-%E6%93%8D%E4%BD%9C%E7%AC%A6%E4%B8%80%E6%AE%B5%E6%97%B6%E9%97%B4%E5%86%85%E6%B2%A1%E6%9C%89%E5%8F%98%E5%8C%96%E5%B0%B1%E4%BC%9A%E5%8F%91%E9%80%81%E4%B8%80%E4%B8%AA%E6%95%B0%E6%8D%AE)
    - [doOnSubscribe()](#doonsubscribe)
    - [range 操作符](#range-%E6%93%8D%E4%BD%9C%E7%AC%A6)
    - [defer 操作符](#defer-%E6%93%8D%E4%BD%9C%E7%AC%A6)
  - [生命周期控制和内存优化](#%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F%E6%8E%A7%E5%88%B6%E5%92%8C%E5%86%85%E5%AD%98%E4%BC%98%E5%8C%96)
    - [取消订阅 subscription.unsubscribe() ;](#%E5%8F%96%E6%B6%88%E8%AE%A2%E9%98%85-subscriptionunsubscribe-)
    - [线程调度](#%E7%BA%BF%E7%A8%8B%E8%B0%83%E5%BA%A6)
    - [rxlifecycle 框架的使用](#rxlifecycle-%E6%A1%86%E6%9E%B6%E7%9A%84%E4%BD%BF%E7%94%A8)
      - [**bindToLifecycle** 方法](#bindtolifecycle-%E6%96%B9%E6%B3%95)
      - [**bindUntilEvent**( ActivityEvent event)](#binduntilevent-activityevent-event)
      - [FragmentEvent](#fragmentevent)
    - [](#-10)
  - [RxBinding](#rxbinding)
    - [git地址](#git%E5%9C%B0%E5%9D%80)
    - [androidStudio 使用](#androidstudio-%E4%BD%BF%E7%94%A8)
    - [代码示例](#%E4%BB%A3%E7%A0%81%E7%A4%BA%E4%BE%8B)
      - [Button 防抖处理](#button-%E9%98%B2%E6%8A%96%E5%A4%84%E7%90%86)
      - [按钮的长按时间监听](#%E6%8C%89%E9%92%AE%E7%9A%84%E9%95%BF%E6%8C%89%E6%97%B6%E9%97%B4%E7%9B%91%E5%90%AC)
      - [listView 的点击事件、长按事件处理](#listview-%E7%9A%84%E7%82%B9%E5%87%BB%E4%BA%8B%E4%BB%B6%E9%95%BF%E6%8C%89%E4%BA%8B%E4%BB%B6%E5%A4%84%E7%90%86)
      - [用户登录界面，勾选同意隐私协议，登录按钮就变高亮](#%E7%94%A8%E6%88%B7%E7%99%BB%E5%BD%95%E7%95%8C%E9%9D%A2%E5%8B%BE%E9%80%89%E5%90%8C%E6%84%8F%E9%9A%90%E7%A7%81%E5%8D%8F%E8%AE%AE%E7%99%BB%E5%BD%95%E6%8C%89%E9%92%AE%E5%B0%B1%E5%8F%98%E9%AB%98%E4%BA%AE)
      - [搜索的时候，关键词联想功能 。debounce()在一定的时间内没有操作就会发送事件。](#%E6%90%9C%E7%B4%A2%E7%9A%84%E6%97%B6%E5%80%99%E5%85%B3%E9%94%AE%E8%AF%8D%E8%81%94%E6%83%B3%E5%8A%9F%E8%83%BD-debounce%E5%9C%A8%E4%B8%80%E5%AE%9A%E7%9A%84%E6%97%B6%E9%97%B4%E5%86%85%E6%B2%A1%E6%9C%89%E6%93%8D%E4%BD%9C%E5%B0%B1%E4%BC%9A%E5%8F%91%E9%80%81%E4%BA%8B%E4%BB%B6)
    - [](#-11)
  - [线程调度实例](#%E7%BA%BF%E7%A8%8B%E8%B0%83%E5%BA%A6%E5%AE%9E%E4%BE%8B)
    - [Rxjava默认运行的线程](#rxjava%E9%BB%98%E8%AE%A4%E8%BF%90%E8%A1%8C%E7%9A%84%E7%BA%BF%E7%A8%8B)
      - [结论](#%E7%BB%93%E8%AE%BA)
    - [subscribeOn、observeOn对线程影响](#subscribeonobserveon%E5%AF%B9%E7%BA%BF%E7%A8%8B%E5%BD%B1%E5%93%8D)
      - [结论](#%E7%BB%93%E8%AE%BA-1)
    - [多次切换线程](#%E5%A4%9A%E6%AC%A1%E5%88%87%E6%8D%A2%E7%BA%BF%E7%A8%8B)
    - [只规定了事件产生的线程](#%E5%8F%AA%E8%A7%84%E5%AE%9A%E4%BA%86%E4%BA%8B%E4%BB%B6%E4%BA%A7%E7%94%9F%E7%9A%84%E7%BA%BF%E7%A8%8B)
    - [只规定事件消费线程](#%E5%8F%AA%E8%A7%84%E5%AE%9A%E4%BA%8B%E4%BB%B6%E6%B6%88%E8%B4%B9%E7%BA%BF%E7%A8%8B)
      - [结论](#%E7%BB%93%E8%AE%BA-2)
    - [线程调度封装](#%E7%BA%BF%E7%A8%8B%E8%B0%83%E5%BA%A6%E5%B0%81%E8%A3%85)
      - [一般的用法：](#%E4%B8%80%E8%88%AC%E7%9A%84%E7%94%A8%E6%B3%95)
      - [简单的封装](#%E7%AE%80%E5%8D%95%E7%9A%84%E5%B0%81%E8%A3%85)
      - [改进后的封装](#%E6%94%B9%E8%BF%9B%E5%90%8E%E7%9A%84%E5%B0%81%E8%A3%85)
      - [最优改进后的封装](#%E6%9C%80%E4%BC%98%E6%94%B9%E8%BF%9B%E5%90%8E%E7%9A%84%E5%B0%81%E8%A3%85)
  - [相关推荐](#%E7%9B%B8%E5%85%B3%E6%8E%A8%E8%8D%90)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

[TOC]


【JAVA 运算符】
【

# 【JAVA 运算符】

## 1 算术运算符
| **运算符** | **运算**                | **范例**                  | **结果**      |
| ------- | --------------------- | ----------------------- | ----------- |
| **+**   | 正号                    | \+3                     | 3           |
| **-**   | 负号                    | b=3;-b;                 | \-3         |
| **+**   | 加                     | 5+5                     | 10          |
| **-**   | 减                     | 6-4                     | 2           |
| **\***  | 乘                     | 3\*4                    | 12          |
| **/**   | 除                     | 5/5                     | 1           |
| **%**   | **取模**（相当于相除后的**余数**） | 5&3（5/3得1**余2**）        | 2           |
| **++**  | 自增（前）                 | a=2;b=++a; （a自增后赋值给b）   | a=3;**b=3** |
| **++**  | 自增（后）                 | a=2;b= a ++; （a赋值给b后自增） | a=3;**b=2** |
| `--`    | 自减（前）                 | a=2;b=--a;              | a=1;**b=1** |
| `--`    | 自减（后）                 | a=2;b=a--;              | a=1;**b=2** |
| **+**   | 字符串相加                 | “he”+”llo”              | hello       |
**取模技巧：**
左边&lt;右边，结果=左边
左边=右边，结果=0
右边=1，结果=0
结果正负**由左边数（被模数）正负决定**

**自增**：相当于+1

char c = '你';//char是2字节，一个中文也是2字节，可以赋值（“你好”超2字节，不能赋值）
### **转义字符**
通过“\\”来转变后面字母或符号的含义
**\n**：换行
**\r**：按下回车键；Windows系统中，回车符由两个字符表示\\r\\n
**\b**：退格
**\t**：制表符，相当于tab键

### 算术运算符的注意问题
• 如果对负数取模，可以把模数负号忽略不记，如：5%-2=1。但被模
数是负数就另当别论。
• 对于**除号“/”**，它的整数除和小数除是有区别的：**整数之间做除法时**
**，只保留整数部分而舍弃小数部分**。
　　例如：`int x = 3510; x = x / 1000 * 1000;`　结果x=3000
• “**+**”除字符串相加功能外，还能把非字符串转换成字符串，**字符串数据和任何数据使用“+”都是相连接，最终都会变成字符串**

## 2 赋值运算符
**符号**：
　　= , +=, -=, *=, /=, %=
　　Eg. x**+=**4;//x=x+4，把**右边和左边的和赋予左边**
**示例**：
　　int a,b,c; a=b=c =3;
　　int a = 3; a+=5;等同运算a=a+5;
**思考**：
`short s = 3; s=s+2;` 
`short s = 3; s+=2;`
有什么区别？
s=s+2//编译失败，因为s会被提升为int类型，运算后的结果还是int类型。无法赋值给short类型。
s+=2//编译通过，因为**+=运算符在给s赋值时，自动完成了强转操作**。

## 3 比较运算符
| **运算符**        | **运算**    | **范例**                     | **结果** |
| -------------- | --------- | -------------------------- | ------ |
| **==**         | 相等于       | 4==3                       | false  |
| **!=**         | 不等于       | 4！=3                       | true   |
| **&lt;**       | 小于        | 4&lt;3                     | false  |
| **>**          | 大于        | 4\>3                       | true   |
| **&lt;=**      | 小于等于      | 4&lt;=3                    | false  |
| **>=**         | 大于等于      | 4\>=3                      | true   |
| **instanceof** | 检查是否是类的对象 | “hello” instanceof Strihng | true   |
注1：比较运算符的**结果**都是boolean型，也就是**要么是true，要么是false**。
注2：比较运算符“==”不能误写成“=” 。

## 4 逻辑运算符
| **运算符**  | **运算**  | **范例**        | **结果** |
| -------- | ------- | ------------- | ------ |
| **&**    | AND（与）  | false&true    | false  |
| **\|**   | OR（或）   | false\|true   | true   |
| **^**    | XOR（异或） | false^true    | true   |
|          |         | true^true     | false  |
| **!**    | Not（非）  | !true         | false  |
| **&&**   | AND（短路） | false&&true   | false  |
| **\|\|** | OR（短路）  | false\|\|true | true   |
逻辑运算符用于连接布尔型表达式，在Java中不可以 写成3&lt;x&lt;6，应该写成x\>3 &x&lt;6 。

“&”和“&&”的区别：
• 单&时，左边无论真假，右边都进行运算；
• 双&时，**如果左边为真，右边参与运算，如果左边为假，那 么右边不参与运算。**
“|”和“**||**”的区别同理，**双或时，左边为真，右边不参与运算**。

**异或( ^ )与或( \| )的不同之处是：当左右都为true时， 结果为false。**

## 5 位运算符
| **运算符**      | **运算**                                   | **范例**                            |
| ------------ | ---------------------------------------- | --------------------------------- |
| **&lt;&lt;** | **左移**<br>空位补0，被移除的高位丢弃，空缺位补0。           | 3 &lt;&lt; 2 = 12 ‐‐\> 3\*2\*2=12 |
| **>>**       | **右移**；**最高位补0或1由原最高位决定**<br>被移位的二进制最高位是0，右移后，空缺位补0； 最高位是1，空缺位补1。 | 3 \>\> 1 = 1 ‐‐\> 3/2=1           |
| **>>>**      | **无符号右移**；**最高位只补0**<br>被移位二进制最高位无论是0或者是1，空缺位都用0补。 | 3 \>\>\> 1 = 1 ‐‐\> 3/2=1         |
| **&**        | **与运算**<br>二进制位进行&运算，只有1&1时结果是1，否则是0;    | 6 & 3 = 2                         |
| **\|**       | **或运算**<br>二进制位进行 \| 运算，只有0 \| 0时结果是0，否则是1; | 6 \| 3 = 7                        |
| **^**        | **异或运算**<br>任何相同二进制位进行 \^ 运算，结果是0；1\^1=0 , 0\^0=0<br>不相同二进制位 ^ 运算结果是1。1\^0=1 , 0\^1=1 | 6 \^ 3 = 5                        |
| **~**        | **非（反码）**（相当于数值加1再加负号 \~-9=8）<br>二进制值：1变0 0变1 | \~6 = ‐7                          |

位运算是直接对**二进制进行运算**。
### 左移右移
**&lt;&lt;：相当于乘与2的移动位数次幂**
**>>：相当于除以2的移动位数次幂**
**移n位，就是对乘以或者除以2的n次幂。**
**Eg.-**8**\>\>**3=-1 （相当于-8/23=-1）
*1*1111111 11111111 11111111 11111000
*111*11111111 11111111 11111111 11111*000* 右移三位，最高位补0或1由原最高位决定

### ^异或运算
5^7：
　　101
　& 111 **1相当于是true，0相当于false**
　\----------
　　010 =2
**一个数异或另一数两次，结果还是原数**（7^3^3=7，实际应用：3先异或对数据7上锁，再用3异或才能读取数据7,3此时相当于**安全秘钥**~）

### ~ 非
~9=-10（相当于数值加1再加负号 ~-9=8）
00000000 00000000 00000000 00001001（原数9）
11111111 11111111 11111111 11110110（~非运算后的值-二进制中的1变0 0变1）
##### 附：负数计算
11111111 11111111 11111111 11110110（负数，上面非运算后的值）
下面推导这个二进制值对应十进制负数具体是多少：
11111111 11111111 11111111 11110101 减1
00000000 00000000 00000000 00001010 取反，得知结果为**-10**

### 位运算符练习：
#### 1.最有效率的方式算出2乘以8等于几？
　　2&lt;&lt;3;
#### 2.**对两个整数变量的值进行互换**(不需要第三方变量)
目的：互换x和y的值
方法1：通过第三方变量（平常较常用）
方法2：通过加减运算，如果x和y值非常大，容易超出int范围
方法3：通过**异或运算**（答题技巧）
**方法1**
```
int z;
z=x;
x=y;
y=z;
System.out.println("x1="+x+" y1="+y);
```

**方法2**
```
x=x+y;//如果x和y值非常大，容易超出int范围
y=x-y;
x=x-y;
System.out.println("x2="+x+" y2="+y);
```

**方法3**
```
x=x^y;
y=x^y;//x两次^y，返回x的值
x=x^y;
System.out.println("x3="+x+" y3="+y);
```

## 6 三元运算符
格式
**• (条件表达式)?表达式1：表达式2；**
• 如果条件为true，运算后的结果是表达式1；
• 如果条件为false，运算后的结果是表达式2；

示例：
• 获取两个数中大数。
```
int x=3,y=4,z;
z = (x\>y)?x:y;//z变量存储的就是两个数的大数。
```



】


【JAVA 集合类]
【
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
】


【JAVA 工具类】
【
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


】


【JAVA IO流】
【
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
】


【JAVA 线程】
【
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

】


【JAVA RxJava】
【
# 【JAVA RxJava】


**GitHub 链接**：   
https://github.com/ReactiveX/RxJava   
https://github.com/ReactiveX/RxAndroid   

**引入依赖**：   
compile 'io.reactivex:rxjava:1.0.14'   
compile 'io.reactivex:rxandroid:1.0.1'   
（版本号是文章发布时的最新稳定版）

## RxJava 到底是什么

一个词：**异步**。
RxJava 在 GitHub 主页上的自我介绍是 "a library for composing asynchronous and event-based programs using observable sequences for the Java VM"（一个在 Java VM上使用可观测的序列来组成异步的、基于事件的程序的库）。这就是 RxJava，概括得非常精准。
然而，对于初学者来说，这太难看懂了。因为它是一个『总结』，而初学者更需要一个『引言』。
其实， RxJava的本质可以压缩为异步这一个词。说到根上，它就是一个实现异步操作的库，而别的定语都是基于这之上的。

## RxJava 好在哪

换句话说，『同样是做异步，为什么人们用它，而不用现成的 AsyncTask / Handler / XXX/ ... ？』
一个词：**简洁**。
异步操作很关键的一点是程序的简洁性，因为在调度过程比较复杂的情况下，异步代码经常会既难写也难被读懂。
Android 创造的 AsyncTask 和Handler ，其实都是为了让异步代码更加简洁。RxJava的优势也是简洁，但它的简洁的与众不同之处在于，**随着程序逻辑变得越来越复杂，它依然能够保持简洁。**

**举例**：假设有这样一个需求：界面上有一个自定义的视图 imageCollectorView ，它的作用是显示多张图片，并能使用addImage(Bitmap) 方法来任意增加显示的图片。现在需要程序将一个给出的目录数组 File[]folders 中每个目录下的 png图片都加载出来并显示在 imageCollectorView 中。需要注意的是，由于读取图片的这一过程较为耗时，需要放在后台执行，而图片的显示则必须在UI 线程执行。常用的实现方式有多种，我这里贴出其中一种：

```java
new Thread() {
    @Override
    public void run() {
        super.run();
        for (File folder : folders) {
            File[] files = folder.listFiles();
            for (File file : files) {
                if (file.getName().endsWith(".png")) {
                    final Bitmap bitmap = getBitmapFromFile(file);
                    getActivity().runOnUiThread(new Runnable() {
                        @Override
                        public void run() {
                            imageCollectorView.addImage(bitmap);
                        }
                    });
                }
            }
        }
    }
}.start();
```

而如果使用 RxJava ，实现方式是这样的：

```java
Observable.from(folders)
        .flatMap(new Func1<File, Observable<File>>() {
            @Override
            public Observable<File> call(File file) {
                return Observable.from(file.listFiles());
            }
        })
        .filter(new Func1<File, Boolean>() {
            @Override
            public Boolean call(File file) {
                return file.getName().endsWith(".png");
            }
        })
        .map(new Func1<File, Bitmap>() {
            @Override
            public Bitmap call(File file) {
                return getBitmapFromFile(file);
            }
        })
        .subscribeOn(Schedulers.io())
        .observeOn(AndroidSchedulers.mainThread())
        .subscribe(new Action1<Bitmap>() {
            @Override
            public void call(Bitmap bitmap) {
                imageCollectorView.addImage(bitmap);
            }
        });
```

那位说话了：『你这代码明明变多了啊！简洁个毛啊！』大兄弟你消消气，我说的是逻辑的简洁，不是单纯的代码量少（逻辑简洁才是提升读写代码速度的必杀技对不？）。观察一下你会发现，
RxJava的这个实现，是一条从上到下的链式调用，没有任何嵌套，这在逻辑的简洁性上是具有优势的。当需求变得复杂时，这种优势将更加明显（试想如果还要求只选取前10
张图片，常规方式要怎么办？如果有更多这样那样的要求呢？再试想，在这一大堆需求实现完两个月之后需要改功能，当你翻回这里看到自己当初写下的那一片迷之缩进，你能保证自己将迅速看懂，而不是对着代码重新捋一遍思路？）。
另外，如果你的 IDE 是 Android Studio ，其实每次打开某个 Java文件的时候，你会看到被自动 Lambda 化的预览，这将让你更加清晰地看到程序逻辑：

```java
Observable.from(folders)
        .flatMap((Func1) (folder) -> { Observable.from(file.listFiles()) })
        .filter((Func1) (file) -> { file.getName().endsWith(".png") })
        .map((Func1) (file) -> { getBitmapFromFile(file) })
        .subscribeOn(Schedulers.io())
        .observeOn(AndroidSchedulers.mainThread())
        .subscribe((Action1) (bitmap) -> { imageCollectorView.addImage(bitmap) });
```

> 如果你习惯使用 Retrolambda，你也可以直接把代码写成上面这种简洁的形式。而如果你看到这里还不知道什么是Retrolambda ，我不建议你现在就去学习它。
> 原因有两点：

1. Lambda是把双刃剑，它让你的代码简洁的同时，降低了代码的可读性，因此同时学习 RxJava 和Retrolambda 可能会让你忽略 RxJava 的一些技术细节；
2. Retrolambda 是 Java 6/7 对Lambda表达式的非官方兼容方案，它的向后兼容性和稳定性是无法保障的，因此对于企业项目，使用Retrolambda 是有风险的。所以，与很多 RxJava 的推广者不同，我并不推荐在学习RxJava 的同时一起学习 Retrolambda。
   事实上，我个人虽然很欣赏Retrolambda，但我从来不用它。

在Flipboard 的 Android代码中，有一段逻辑非常复杂，包含了多次内存操作、本地文件操作和网络操作，对象分分合合，线程间相互配合相互等待，一会儿排成人字，一会儿排成一字。如果使用常规的方法来实现，肯定是要写得欲仙欲死，然而在使用RxJava 的情况下，依然只是一条链式调用就完成了。它很长，但很清晰。
所以， RxJava 好在哪？就好在简洁，好在那把什么复杂逻辑都能穿成一条线的简洁。

## API介绍和原理简析

### 1. 概念：扩展的观察者模式

RxJava 的异步实现，是通过一种扩展的观察者模式来实现的。

#### 观察者模式

观察者模式面向的需求是：**A** 对象（观察者）对 B对象（被观察者）的某种变化高度敏感，需要**在 B变化的一瞬间做出反应**。
举个例子，新闻里喜闻乐见的警察抓小偷，警察需要在小偷伸手作案的时候实施抓捕。在这个例子里，警察是观察者，小偷是被观察者，警察需要时刻盯着小偷的一举一动，才能保证不会漏过任何瞬间。
程序的观察者模式和这种真正的『观察』略有不同，观察者不需要时刻盯着被观察者（例如A 不需要每过 2ms 就检查一次 B的状态），而是采用**注册**(Register)**或者称为**订阅**(Subscribe)**的方式，告诉被观察者：我需要你的某某状态，你要在它变化的时候通知我。
Android开发中一个比较典型的例子是点击监听器 OnClickListener 。对设置 OnClickListener 来说， View 是被观察者， OnClickListener 是观察者，二者通过 setOnClickListener() 方法达成订阅关系。订阅之后用户点击按钮的瞬间，Android Framework就会将点击事件发送给已经注册的 OnClickListener 。采取这样被动的观察方式，既省去了反复检索状态的资源消耗，也能够得到最高的反馈速度。当然，这也得益于我们可以随意定制自己程序中的观察者和被观察者，而警察叔叔明显无法要求小偷『你在作案的时候务必通知我』。
OnClickListener 的模式大致如下图：
![OnClickListener 观察者模式](http://img.blog.csdn.net/20171230170355298?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
如图所示，通过 setOnClickListener() 方法，Button 持有 OnClickListener 的引用（这一过程没有在图上画出）；当用户点击时，Button 自动调用 OnClickListener 的 onClick() 方法。
另外，如果把这张图中的概念抽象出来（Button ->被观察者、OnClickListener -> 观察者、setOnClickListener() ->订阅，onClick() ->事件），就由专用的观察者模式（例如只用于监听控件点击）转变成了通用的观察者模式。如下图：
![通用观察者模式](http://img.blog.csdn.net/20171230170505487?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
而 RxJava 作为一个工具库，使用的就是通用形式的观察者模式。

#### RxJava 的观察者模式

RxJava有四个基本概念：
**Observable** (可观察者，即被观察者)、 **Observer** (观察者)、 **subscribe** (订阅)、**事件**。Observable 和 Observer 通过 subscribe() 方法实现订阅关系，从而 Observable 可以在需要的时候发出事件来通知 Observer。
与传统观察者模式不同， RxJava的事件回调方法除了普通事件 **onNext**() （相当于 onClick() / onEvent()）之外，还定义了两个特殊的事件：**onCompleted**() 和 **onError**()。

##### onCompleted和onError

- **onCompleted**(): 事件队列完结。
  RxJava不仅把每个事件单独处理，还会把它们看做一个队列。RxJava    规定，当不会再有新的 onNext() 发出时，需要触发 onCompleted() 方法作为标志。
- **onError**(): 事件队列异常。
  在事件处理过程中出异常时，onError() 会被触发，同时队列自动终止，不允许再有事件发出。
- 在一个正确运行的事件序列中, **onCompleted() 和 onError() 有且只有一个**，并且是事件序列中的最后一个。需要注意的是，onCompleted() 和 onError() 二者也是**互斥**的，即在队列中调用了其中一个，就不应该再调用另一个。

RxJava 的观察者模式大致如下图：
![RxJava 的观察者模式](http://img.blog.csdn.net/20171230170421887?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

####  

### 2. 基本实现

基于以上的概念， RxJava 的基本实现主要有三点：

#### 1) 创建 Observer

Observer 即观察者，它决定事件触发的时候将有怎样的行为。 
RxJava中的 **Observer接口**的**实现方式**：

```
Observer<String> observer = new Observer<String>() {
    @Override
    public void onNext(String s) {
        Log.d(tag, "Item: " + s);
    }
    @Override
    public void onCompleted() {
        Log.d(tag, "Completed!");
    }
    @Override
    public void onError(Throwable e) {
        Log.d(tag, "Error!");
    }
};
```

##### Subscriber

除了 Observer 接口之外，RxJava还内置了一个实现了 Observer 的**抽象类**：**Subscriber**。
 Subscriber 对 Observer 接口进行了一些扩展，但他们的基本使用方式是完全一样的：

```
Subscriber<String> subscriber = new Subscriber<String>() {
    @Override
    public void onNext(String s) {
        Log.d(tag, "Item: " + s);
    }
    @Override
    public void onCompleted() {
        Log.d(tag, "Completed!");
    }
    @Override
    public void onError(Throwable e) {
        Log.d(tag, "Error!");
    }
};
```

不仅基本使用方式一样，实质上，**在 RxJava 的 subscribe过程中，Observer 也总是会先被转换成一个 Subscriber 再使用**。所以如果你只想使用基本功能，选择 Observer 和 Subscriber 是完全一样的。

##### Observer  vs  Subscriber

1. **onStart**(): 这是 Subscriber 增加的方法。
   它会在 subscribe刚开始，而事件还未发送之前被调用，可以用于做一些**准备工作**，例如数据的清零或重置。
   这是一个**可选方法**，默认情况下它的实现为空。需要注意的是，
   如果对准备工作的线程有要求（例如弹出一个显示进度的对话框，这必须在主线程执行）， **onStart**() 就不适用了，因为它**总**是**在subscribe所发生的线程被调用，而不能指定线程**。要在指定的线程来做准备工作，可以使用 doOnSubscribe() 方法，具体可以在后面的文中看到。
2. **unsubscribe**(): 这是 Subscriber 所实现的另一个接口 Subscription 的方法，用于**取消订阅**。
   在这个方法被调用后，Subscriber 将不再接收事件。一般在这个方法调用前，可以使用 **isUnsubscribed() 先判断一下状态**。 
   unsubscribe() 这个方法很重要，因为在 subscribe() 之后， Observable 会持有 Subscriber 的引用，这个引用如果不能及时被释放，将有内存泄露的风险。所以最好保持一个原则：要**在不再使用的时候尽快在合适的地方（例如 onPause() onStop() 等方法中）调用 unsubscribe() 来解除引用关系，以避免内存泄露**的发生。

#### 2) 创建 Observable

Observable 即被观察者，它决定什么时候触发事件以及触发怎样的事件。 
RxJava使用 **create()** 方法来**创建**一个 Observable ，并为它定义事件触发规则：

```
Observable observable = Observable.create(new Observable.OnSubscribe<String>()
{
    @Override
    public void call(Subscriber<? super String> subscriber) {
        subscriber.onNext("Hello");
        subscriber.onNext("Hi");
        subscriber.onNext("Aloha");
        subscriber.onCompleted();
    }
});
```

可以看到，这里传入了一个 OnSubscribe 对象作为参数。OnSubscribe 会被存储在返回的 Observable 对象中，它的作用相当于一个计划表，当 Observable 被订阅的时候，OnSubscribe 的 call() 方法会自动被调用，事件序列就会依照设定依次触发（对于上面的代码，就是观察者Subscriber 将会被调用三次 onNext() 和一次 onCompleted()）。这样，由被观察者调用了观察者的回调方法，就实现了由被观察者向观察者的事件传递，即观察者模式。

##### 创建事件队列方法

###### **create**()

是 RxJava 最基本的创造事件序列的方法。
基于这个方法， RxJava还提供了一些方法用来快捷创建事件队列，例如：

###### **just**(T...)

将传入的参数依次发送出来。

```java
Observable observable = Observable.just("Hello", "Hi", "Aloha");
// 将会依次调用：
// onNext("Hello");
// onNext("Hi");
// onNext("Aloha");
// onCompleted();
```

###### **from**(T[]) / from(Iterable<? extends T>) :

将传入的数组或 Iterable 拆分成具体对象后，依次发送出来。

```java
String[] words = {"Hello", "Hi", "Aloha"};
Observable observable = Observable.from(words);
// 将会依次调用：
// onNext("Hello");
// onNext("Hi");
// onNext("Aloha");
// onCompleted();
```

上面 just(T...) 的例子和 from(T[]) 的例子，都和之前的 create(OnSubscribe) 的例子是等价的。

#####  

#### 3) Subscribe (订阅)

创建了 Observable 和 Observer 之后，再用 subscribe() 方法将它们联结起来，整条链子就可以工作了。代码形式很简单：

```
observable.subscribe(observer);
// 或者：
observable.subscribe(subscriber);
```

有人可能会注意到， subscribe() 这个方法有点怪：它看起来是『observalbe 订阅了 observer / subscriber』而不是『observer / subscriber 订阅了 observalbe』，这看起来就像『杂志订阅了读者』一样颠倒了对象关系。这让人读起来有点别扭，不过如果把API设计成 observer.subscribe(observable) / subscriber.subscribe(observable) ，虽然更加符合思维逻辑，但对流式API 的设计就造成影响了，比较起来明显是得不偿失的。

##### **Observable.subscribe(Subscriber)** 的**内部实现**：

注意：这不是 subscribe()的源码，而是将源码中与性能、兼容性、扩展性有关的代码剔除后的核心代码。
如果需要看源码，可以去 RxJava 的 GitHub 仓库下载。

```java
public Subscription subscribe(Subscriber subscriber) {
    subscriber.onStart();
    onSubscribe.call(subscriber);
    return subscriber;
}
```

可以看到，subscriber() 做了3件事：

1. 调用 Subscriber.onStart() 。这个方法在前面已经介绍过，是一个可选的准备方法。
2. 调用 Observable 中的 OnSubscribe.call(Subscriber) 。在这里，事件发送的逻辑开始运行。从这也可以看出，在RxJava中， Observable 并不是在创建的时候就立即开始发送事件，而是在它被订阅的时候，即当 subscribe() 方法执行的时候。
3. 将传入的 Subscriber 作为 Subscription 返回。这是为了方便 unsubscribe().
   整个过程中对象间的关系如下图：
   ![关系静图](http://img.blog.csdn.net/20171230170451933?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
   或者可以看动图：
    [![关系动图](http://img.blog.csdn.net/20171230170436683?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)](http://img.blog.csdn.net/20171230170436683?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

##### 不完整定义的回调

除了 subscribe(Observer) 和 subscribe(Subscriber) ，subscribe() 还支持不完整定义的回调，RxJava会自动根据定义创建出 Subscriber 。形式如下：

```java
Action1<String> onNextAction = new Action1<String>() {
    // onNext()
    @Override
    public void call(String s) {
        Log.d(tag, s);
    }
};
Action1<Throwable> onErrorAction = new Action1<Throwable>() {
    // onError()
    @Override
    public void call(Throwable throwable) {
// Error handling
    }
};
Action0 onCompletedAction = new Action0() {
    // onCompleted()
    @Override
    public void call() {
        Log.d(tag, "completed");
    }
};
// 自动创建 Subscriber ，并使用 onNextAction 来定义 onNext()
observable.subscribe(onNextAction);
// 自动创建 Subscriber ，并使用 onNextAction 和 onErrorAction 来定义 onNext() 和onError()
observable.subscribe(onNextAction, onErrorAction);
// 自动创建 Subscriber ，并使用 onNextAction、 onErrorAction 和onCompletedAction 来定义 onNext()、 onError() 和 onCompleted()
observable.subscribe(onNextAction, onErrorAction, onCompletedAction);
```

简单解释一下这段代码中出现的 Action1 和 Action0。 

###### **Action0**

是 RxJava的一个接口，它只有一个方法 call()，这个方法是无参无返回值的；由于 onCompleted() 方法也是无参无返回值的，因此 Action0 可以被当成一个包装对象，将 onCompleted() 的内容打包起来将自己作为一个参数传入 subscribe() 以实现不完整定义的回调。这样其实也可以看做将 onCompleted() 方法作为参数传进了 subscribe()，相当于其他某些语言中的『闭包』。 

###### **Action1**

也是一个接口，它同样只有一个方法 call(T param)，这个方法也无返回值，但有一个参数；与 Action0 同理，由于 onNext(T obj) 和 onError(Throwable error) 也是单参数无返回值的，因此 Action1可以将 onNext(obj) 和 onError(error) 打包起来传入 subscribe() 以实现不完整定义的回调。

事实上，虽然 Action0 和 Action1在API 中使用最广泛，但 RxJava 是提供了多个 ActionX 形式的接口(例如 Action2, Action3) 的，它们可以被用以包装不同的无返回值的方法。

注：正如前面所提到的，Observer 和 Subscriber 具有相同的角色，而且 Observer 在 subscribe() 过程中最终会被转换成 Subscriber对象，因此，从这里开始，后面的描述我将用 Subscriber 来代替 Observer ，这样更加严谨。

#####  

#### 4) 场景示例

下面举两个例子：
为了把原理用更清晰的方式表述出来，本文中挑选的都是功能尽可能简单的例子，以至于有些示例代码看起来会有『画蛇添足』『明明不用RxJava 可以更简便地解决问题』的感觉。当你看到这种情况，不要觉得是因为 RxJava太啰嗦，而是因为在过早的时候举出真实场景的例子并不利于原理的解析，因此我刻意挑选了简单的情景。

##### a. 打印字符串数组

将字符串数组 names 中的所有字符串依次打印出来：

```
String[] names = ...;
Observable.from(names)
        .subscribe(new Action1<String>() {
            @Override
            public void call(String name) {
                Log.d(tag, name);
            }
        });
```

##### b. 由 id 取得图片并显示

由指定的一个 drawable 文件id drawableRes 取得图片，并显示在 ImageView 中，并在出现异常的时候打印 Toast报错：

```
int drawableRes = ...;
ImageView imageView = ...;
Observable.create(new OnSubscribe<Drawable>() {
    @Override
    public void call(Subscriber<? super Drawable> subscriber) {
        Drawable drawable = getTheme().getDrawable(drawableRes));
        subscriber.onNext(drawable);
        subscriber.onCompleted();
    }
}).subscribe(new Observer<Drawable>() {
    @Override
    public void onNext(Drawable drawable) {
        imageView.setImageDrawable(drawable);
    }
    @Override
    public void onCompleted() {
    }
    @Override
    public void onError(Throwable e) {
        Toast.makeText(activity, "Error!", Toast.LENGTH_SHORT).show();
    }
});
```

正如上面两个例子这样，创建出 Observable 和 Subscriber ，再用 subscribe() 将它们串起来，一次RxJava 的基本使用就完成了。非常简单。
然而，在 RxJava的默认规则中，事件的发出和消费都是在同一个线程的。也就是说，如果只用上面的方法，实现出来的只是一个同步的观察者模式。观察者模式本身的目的就是『后台处理，前台回调』的异步机制，因此异步对于RxJava 是至关重要的。而要实现异步，则需要用到 RxJava 的另一个概念： Scheduler 。

####  

### 3. 线程控制 —— Scheduler (一)

在**不指定线程的情况下**， RxJava遵循的是**线程不变**的原则，即：在哪个线程调用subscribe()，就在哪个线程生产事件；在哪个线程生产事件，就在哪个线程消费事件。如果需要切换线程，就需要用到 **Scheduler （调度器）**。

#### 1) Scheduler 的 API (一)

在RxJava 中，**Scheduler** ——调度器，相当于**线程控制器**，RxJava通过它来指定每一段代码应该运行在什么样的线程。
RxJava已经内置了几个 Scheduler ，它们已经适合大多数的使用场景：

##### **Schedulers.immediate**():

直接在**当前线程**运行，相当于不指定线程。这是**默认**的 Scheduler。

##### **Schedulers.newThread**():

**总**是**启用新线程**，并在新线程执行操作。

##### **Schedulers.io**():

I/O操作（读写文件、读写数据库、网络信息交互等）所使用的 Scheduler。
行为模式和 newThread() 差不多，区别在于 **io()** 的内部实现是是用一个无数量上限的线程池，可以**重用空闲的线程**，因此多数情况下 io() **比 newThread() 更有效率**。不要把计算工作放在 io() 中，可以避免创建不必要的线程。

##### **Schedulers.computation**():

计算所使用的 Scheduler。这个计算指的是 **CPU密集型计算**，即**不会被 I/O 等操作限制性能的操作**，例如**图形的计算**。
这个 Scheduler 使用的**固定的线程池**，大小为**CPU 核数**。不要把 I/O 操作放在 computation() 中，否则 **I/O操作的等待时间会浪费 CPU**。

##### **AndroidSchedulers.mainThread**()

它指定的操作将在 Android 主线程运行。

有了这几个 Scheduler ，就可以**使用 subscribeOn() 和 observeOn() 两个方法来对线程进行控制**了。

##### **subscribeOn**():

**指定 subscribe() 所发生的线程**，即 Observable.OnSubscribe 被激活时所处的线程。或者叫做事件产生的线程。

##### **observeOn**():

**指定 Subscriber 所运行在的线程**。或者叫做事件消费的线程。

文字叙述总归难理解，上代码：

```
Observable.just(1, 2, 3, 4)
        .subscribeOn(Schedulers.io()) // 指定 subscribe() 发生在 IO 线程
        .observeOn(AndroidSchedulers.mainThread()) // 指定 Subscriber 的回调发生在主线程
        .subscribe(new Action1<Integer>() {
            @Override
            public void call(Integer number) {
                Log.d(tag, "number:" + number);
            }
        });
```

上面这段代码中，由于 subscribeOn(Schedulers.io()) 的指定，被创建的事件的内容 1、2、3、4 将会在IO 线程发出；而由于 observeOn(AndroidScheculers.mainThread())的指定，因此 subscriber 数字的打印将发生在主线程。
事实上，这种在 subscribe() 之前写上两句 subscribeOn(Scheduler.io()) 和 observeOn(AndroidSchedulers.mainThread()) 的使用方式非常常见，它**适用于多数的『后台线程取数据，主线程显示』的程序策略**。
而前面提到的由图片 id 取得图片并显示的例子，如果也加上这两句：

```
int drawableRes = ...;
ImageView imageView = ...;
Observable.create(new OnSubscribe<Drawable>() {
    @Override
    public void call(Subscriber<? super Drawable> subscriber) {
        Drawable drawable = getTheme().getDrawable(drawableRes));
        subscriber.onNext(drawable);
        subscriber.onCompleted();
    }
})
        .subscribeOn(Schedulers.io()) // 指定 subscribe() 发生在 IO 线程
        .observeOn(AndroidSchedulers.mainThread()) // 指定 Subscriber 的回调发生在主线程
        .subscribe(new Observer<Drawable>() {
            @Override
            public void onNext(Drawable drawable) {
                imageView.setImageDrawable(drawable);
            }
            @Override
            public void onCompleted() {
            }
            @Override
            public void onError(Throwable e) {
                Toast.makeText(activity, "Error!", Toast.LENGTH_SHORT).show();
            }
        });
```

那么，加载图片将会发生在 IO线程，而设置图片则被设定在了主线程。这就意味着，即使加载图片耗费了几十甚至几百毫秒的时间，也不会造成丝毫界面的卡顿。

####  

### 4. 变换

RxJava提供了对事件序列进行变换的支持，这是它的核心功能之一，也是大多数人说『RxJava真是太好用了』的最大原因。**所谓变换，就是将事件序列中的对象或整个序列进行加工处理，转换成不同的事件或事件序列。**概念说着总是模糊难懂的，来看
API。

#### 1) API

##### map()

首先看一个 map() 的例子：

```
Observable.just("images/logo.png") // 输入类型 String
        .map(new Func1<String, Bitmap>() {
            @Override
            public Bitmap call(String filePath) { // 参数类型 String
                return getBitmapFromPath(filePath); // 返回类型 Bitmap
            }
        })
        .subscribe(new Action1<Bitmap>() {
            @Override
            public void call(Bitmap bitmap) { // 参数类型 Bitmap
                showBitmap(bitmap);
            }
        });
```

> 这里出现了一个叫做 **Func1** 的类。它和 Action1 非常相似，也是 RxJava的一个接口，用于包装含有一个参数的方法。 
> Func1 和 Action 的区别在于， **Func1 包装**的是**有返回值**的方法。
> 另外，和 ActionX 一样， FuncX 也有多个，用于不同参数个数的方法。FuncX和 ActionX 的区别在 FuncX 包装的是有返回值的方法。

可以看到，map() 方法将参数中的 String 对象转换成一个 Bitmap 对象后返回，而在经过 map() 方法后，事件的参数类型也由 String转为了 Bitmap。这种直接变换对象并返回的，是最常见的也最容易理解的变换。

- **map()**: **事件对象的直接变换**，具体功能上面已经介绍过。它是 RxJava最常用的变换。 

###### map() 的示意图：

![map() 示意图](http://img.blog.csdn.net/20171230170341396?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

不过RxJava 的变换远不止这样，它不仅可以针对事件对象，还可以针对整个事件队列，这使得RxJava 变得非常灵活。

##### flatMap()

首先假设这么一种需求：假设有一个数据结构『学生』，现在需要打印出一组学生的名字。实现方式很简单：

```
Student[] students = ...;
Subscriber<String> subscriber = new Subscriber<String>() {
    @Override
    public void onNext(String name) {
        Log.d(tag, name);
    }
...
};
Observable.from(students)
        .map(new Func1<Student, String>() {
            @Override
            public String call(Student student) {
                return student.getName();
            }
        })
        .subscribe(subscriber);
```

很简单。那么再假设：如果要打印出每个学生所需要修的所有课程的名称呢？（需求的区别在于，每个学生只有一个名字，但却有多个课程。）首先可以这样实现：

```java
Student[] students = ...;
Subscriber<Student> subscriber = new Subscriber<Student>() {
    @Override
    public void onNext(Student student) {
        List<Course> courses = student.getCourses();
        for (int i = 0; i < courses.size(); i++) {
            Course course = courses.get(i);
            Log.d(tag, course.getName());
        }
    }
...
};
Observable.from(students)
        .subscribe(subscriber);
```

依然很简单。那么如果我不想在 Subscriber 中使用 for循环，而是希望 Subscriber 中直接传入单个的 Course 对象呢（这对于代码复用很重要）？
用 map() 显然是不行的，因为 **map() 是一对一的转化**，而我现在的要求是一对多的转化。
那怎么才能把**一个**Student **转**化成**多个** Course 呢？这个时候，就需要用 **flatMap()** 了：

```
Student[] students = ...;
Subscriber<Course> subscriber = new Subscriber<Course>() {
    @Override
    public void onNext(Course course) {
        Log.d(tag, course.getName());
    }
...
};
Observable.from(students)
        .flatMap(new Func1<Student, Observable<Course>>() {
            @Override
            public Observable<Course> call(Student student) {
                return Observable.from(student.getCourses());
            }
        })
        .subscribe(subscriber);
```

从上面的代码可以看出， flatMap() 和 map() 有一个相同点：它也是把传入的参数转化之后返回另一个对象。
但需要注意，和 map()不同的是， **flatMap() 中返回的是个 Observable 对象**，并且这个 Observable 对象并不是被直接发送到了 Subscriber 的回调方法中。

###### flatMap() 原理：

1. 使用传入的事件对象创建一个 Observable 对象；
2. 并不发送这个 Observable，而是将它激活，于是它开始发送事件；
3. 每一个创建出来的 Observable 发送的事件，都被汇入同一个 Observable ，而这个 Observable 负责将这些事件统一交给 Subscriber 的回调方法。

这三个步骤，把事件拆成了两级，**通过一组新创建的 Observable 将初始的对象『铺平』之后通过统一路径分发了下去**。而这个『铺平』就是 flatMap() 所谓的flat。
flatMap() 示意图：
![flatMap() 示意图](http://img.blog.csdn.net/20171230170230646?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

扩展：由于可以在嵌套的 Observable 中添加异步代码， flatMap() 也常用于嵌套的异步操作，例如嵌套的网络请求。示例代码（Retrofit + RxJava）：

```
networkClient.token() // 返回 Observable<String>，在订阅时请求token，并在响应后发送 token
.flatMap(new Func1<String, Observable<Messages>>() {
    @Override
    public Observable<Messages> call(String token) {
// 返回Observable<Messages>，在订阅时请求消息列表，并在响应后发送请求到的消息列表
        return networkClient.messages();
    }
})
        .subscribe(new Action1<Messages>() {
            @Override
            public void call(Messages messages) {
// 处理显示消息列表
                showMessages(messages);
            }
        });
```

传统的嵌套请求需要使用嵌套的 Callback来实现。而通过 flatMap() ，可以把嵌套的请求写在一条链中，从而保持程序逻辑的清晰。

##### throttleFirst():

在每次事件触发后的**一定时间间隔内丢弃新的事件**。
常用作去**抖动过滤**，例如按钮的点击监听器：

```
RxView.clickEvents(button)    // RxBinding 代码，后面的文章有解释 
        .throttleFirst(500, TimeUnit.MILLISECONDS) // 设置防抖间隔为 500ms
        .subscribe(subscriber);//妈妈再也不怕我的用户手抖点开两个重复的界面啦。
```

此外， RxJava 还提供很多便捷的方法来实现事件序列的变换，这里就不一一举例了。

#### 2) 变换的原理：lift()

这些变换虽然功能各有不同，但实质上都是**针对事件序列的处理和再发送**。
而在RxJava的内部，它们是基于同一个基础的变换方法： lift(Operator)。
首先看一下 **lift()** 的**内部实现**（仅核心代码）：
// 注意：这不是 lift()的源码，而是将源码中与性能、兼容性、扩展性有关的代码剔除后的核心代码。
// 如果需要看源码，可以去 RxJava 的 GitHub 仓库下载。

```java
public <R> Observable<R> lift (Operator < ? extends R, ?super T > operator){
    return Observable.create(new OnSubscribe<R>() {
        @Override
        public void call(Subscriber subscriber) {
            Subscriber newSubscriber = operator.call(subscriber);
            newSubscriber.onStart();
            onSubscribe.call(newSubscriber);
        }
    });
}
```

这段代码很有意思：它生成了一个新的 Observable 并返回，而且创建新 Observable 所用的参数 OnSubscribe 的回调方法 call() 中的实现竟然看起来和前面讲过的Observable.subscribe() 一样！然而它们并不一样哟~不一样的地方关键就在于第二行 onSubscribe.call(subscriber) 中的 onSubscribe**所指代的对象不同**——

- subscribe() 中这句话的 onSubscribe 指的是 Observable 中的 onSubscribe 对象，这个没有问题，但是 lift() 之后的情况就复杂了点。
- 当含有 lift() 时：   
  1. lift() 创建了一个 Observable 后，加上之前的原始 Observable，已经有两个 Observable 了；   
  2. 而同样地，新 Observable 里的新 OnSubscribe 加上之前的原始 Observable 中的原始 OnSubscribe，也就有了两个 OnSubscribe；   
  3. 当用户调用经过 lift() 后的 Observable 的 subscribe() 的时候，使用的是 lift() 所返回的新的 Observable ，于是它所触发的 onSubscribe.call(subscriber)，也是用的新 Observable 中的新 OnSubscribe，即在 lift() 中生成的那个 OnSubscribe；   
  4. 而这个新 OnSubscribe 的 call() 方法中的 onSubscribe ，就是指的原始 Observable 中的原始 OnSubscribe ，在这个 call()方法里，新 OnSubscribe 利用 operator.call(subscriber) 生成了一个新的 Subscriber（Operator 就是在这里，通过自己的 call() 方法将新 Subscriber 和原始 Subscriber 进行关联，并插入自己的『变换』代码以实现变换），然后利用这个新 Subscriber 向原始 Observable 进行订阅。   
- 这样就实现了 lift() 过程，有点**像一种代理机制，通过事件拦截和处理实现事件序列的变换。**
  精简掉细节的话，也可以这么说：在 Observable 执行了 lift(Operator) 方法之后，会返回一个新的 Observable，这个新的 Observable 会像一个代理一样，负责接收原始的 Observable 发出的事件，并在处理后发送给 Subscriber。
  如果你更喜欢具象思维，可以看图：
  ![lift() 原理图](http://img.blog.csdn.net/20171230170948478?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
  或者可以看动图：
  ![lift 原理动图](http://img.blog.csdn.net/20171230170932345?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
  两次和多次的 lift() 同理，如下图：
  ![两次 lift](http://img.blog.csdn.net/20171230170920314?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
  举一个具体的 Operator 的实现。下面这是一个将事件中的 Integer 对象转换成 String 的例子，仅供参考：

```
observable.lift(new Observable.Operator<String, Integer>() {
    @Override
    public Subscriber<? super Integer> call(final Subscriber<? super String> subscriber) {
// 将事件序列中的 Integer 对象转换为 String 对象
        return new Subscriber<Integer>() {
            @Override
            public void onNext(Integer integer) {
                subscriber.onNext("" + integer);
            }
            @Override
            public void onCompleted() {
                subscriber.onCompleted();
            }
            @Override
            public void onError(Throwable e) {
                subscriber.onError(e);
            }
        };
    }
});
```

讲述 lift() 的原理只是为了让你更好地了解 RxJava，从而可以更好地使用它。然而不管你是否理解了 lift() 的原理，RxJava都不建议开发者自定义 Operator 来直接使用 lift()，而是建议尽量使用已有的 lift() 包装方法（如 map() flatMap() 等）进行组合来实现需求，因为直接使用lift() 非常容易发生一些难以发现的错误。

#### 3) compose: 对 Observable 整体的变换

除了 lift() 之外， Observable 还有一个变换方法叫做 compose(Transformer)。
它和 lift() 的区别在于， **lift()**是**针对事件项和事件序列**的，而 **compose()**是**针对Observable自身**进行变换。
举个例子，假设在程序中有多个 Observable ，并且他们都需要应用一组相同的 lift() 变换。你可以这么写：

```
observable1
        .lift1()
        .lift2()
        .lift3()
        .lift4()
        .subscribe(subscriber1);
observable2
        .lift1()
        .lift2()
        .lift3()
        .lift4()
        .subscribe(subscriber2);
observable3
        .lift1()
        .lift2()
        .lift3()
        .lift4()
        .subscribe(subscriber3);
observable4
        .lift1()
        .lift2()
        .lift3()
        .lift4()
        .subscribe(subscriber1);
```

你觉得这样太不软件工程了，于是你改成了这样：

```
private Observable liftAll (Observable observable){
    return observable
            .lift1()
            .lift2()
            .lift3()
            .lift4();
}
...
liftAll(observable1).subscribe(subscriber1);
liftAll(observable2).subscribe(subscriber2);
liftAll(observable3).subscribe(subscriber3);
liftAll(observable4).subscribe(subscriber4);
```

可读性、可维护性都提高了。可是 Observable 被一个方法包起来，这种方式对于 Observale 的灵活性似乎还是增添了那么点限制。怎么办？这个时候，就应该用 compose() 来解决了：

```
public class LiftAllTransformer implements Observable.Transformer<Integer, String> {
    @Override
    public Observable<String> call(Observable<Integer> observable) {
        return observable
                .lift1()
                .lift2()
                .lift3()
                .lift4();
    }
}
...
Transformer liftAll = new LiftAllTransformer();
observable1.compose(liftAll).subscribe(subscriber1);
observable2.compose(liftAll).subscribe(subscriber2);
observable3.compose(liftAll).subscribe(subscriber3);
observable4.compose(liftAll).subscribe(subscriber4);
```

像上面这样，使用 compose() 方法，Observable 可以利用传入的 Transformer 对象的 call 方法直接对自身进行处理，也就不必被包在方法的里面了。

### 5. 线程控制：Scheduler (二)

除了灵活的变换，RxJava 另一个牛逼的地方，就是**线程**的**自由控制**。

#### 1) Scheduler 的 API (二)

前面讲到了，可以利用 subscribeOn() 结合 observeOn() 来实现线程控制，让事件的产生和消费发生在不同的线程。可是在了解了 map() flatMap() 等变换方法后，有些好事的（其实就是当初刚接触
RxJava 时的我）就问了：能不能多切换几次线程？
答案是：能。因为 observeOn() 指定的是 Subscriber 的线程，而这个 Subscriber 并不是（严格说应该为『不一定是』，但这里不妨理解为『不是』）subscribe() 参数中的 Subscriber ，而是 observeOn() 执行时的当前 Observable 所对应的 Subscriber ，即它的直接下级 Subscriber 。换句话说，**observeOn() 指定的是它之后的操作所在的线程**。因此如果有**多次切换线程**的需求，只要**在每个想要切换线程的位置调用一次 observeOn() 即可**。
上代码：

```
Observable.just(1, 2, 3, 4) // IO 线程，由 subscribeOn() 指定
        .subscribeOn(Schedulers.io())
        .observeOn(Schedulers.newThread())
        .map(mapOperator) // 新线程，由 observeOn() 指定
        .observeOn(Schedulers.io())
        .map(mapOperator2) // IO 线程，由 observeOn() 指定
        .observeOn(AndroidSchedulers.mainThread)
        .subscribe(subscriber); // Android 主线程，由 observeOn() 指定
```

如上，通过 observeOn() 的多次调用，程序实现了线程的多次切换。
不过，不同于 observeOn() ， **subscribeOn()** 的位置放在哪里都可以，但它是**只能调用一次**的。
又有好事的（其实还是当初的我）问了：如果我非要调用多次 subscribeOn() 呢？会有什么效果？
这个问题先放着，我们还是从 RxJava 线程控制的原理说起吧。

#### 2) Scheduler 的原理（二）

其实， subscribeOn() 和 observeOn() 的内部实现，也是用的 lift()。具体看图（不同颜色的箭头表示不同的线程）：
subscribeOn() 原理图：
![subscribeOn() 原理](http://img.blog.csdn.net/20171230170904588?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

observeOn() 原理图：
![observeOn() 原理](http://img.blog.csdn.net/20171230170849451?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
从图中可以看出，subscribeOn() 和 observeOn() 都做了线程切换的工作（图中的"schedule..."部位）。
不同的是：

- **subscribeOn**()的线程切换发生在 **OnSubscribe 中**，即在它通知上一级 OnSubscribe 时，这时事件还没有开始发送，因此 subscribeOn() 的线程**控制**可以**从事件发出的开端就造成影响**；
- **observeOn**() 的线程切换则发生在它内建的 **Subscriber 中**，即发生在它即将给下一级 Subscriber 发送事件时，因此 observeOn() **控制**的是**它后面的线程**。

最后，我用一张图来解释当多个 subscribeOn() 和 observeOn() 混合使用时，线程调度是怎么发生的（由于图中对象较多，相对于上面的图对结构做了一些简化调整）：
![线程控制综合调用](http://img.blog.csdn.net/20171230170833520?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
图中共有 5处含有对事件的操作。由图中可以看出，①和②两处受第一个 subscribeOn() 影响，运行在红色线程；③和④处受第一个 observeOn() 的影响，运行在绿色线程；⑤处受第二个 onserveOn() 影响，运行在紫色线程；而第二个 subscribeOn() ，由于在通知过程中线程就被第一个 subscribeOn() 截断，因此对整个流程并没有任何影响。这里也就回答了前面的问题：当使用了多个 subscribeOn() 的时候，只有第一个 subscribeOn() 起作用。

#### 3) 延伸：doOnSubscribe()

虽然超过一个的 subscribeOn() 对事件处理的流程没有影响，但在流程之前却是可以利用的。
在前面讲 Subscriber 的时候，提到过 Subscriber 的 onStart() 可以用作流程开始前的初始化。然而 **onStart()** 由于**在 subscribe() 发生时就被调用了**，因此**不能指定线程**，而是**只能执行在 subscribe() 被调用时的线程**。这就导致如果 onStart() 中含有对线程有要求的代码（例如在界面上显示一个ProgressBar，这必须在主线程执行），将会有线程非法的风险，因为有时你无法预测 subscribe() 将会在什么线程执行。
而与 Subscriber.onStart() 相对应的，有一个方法 **Observable.doOnSubscribe()** 。它和 Subscriber.onStart() 同样是**在 subscribe() 调用后而且在事件发送前执行**，但区别在于它**可**以**指定线程**。
默认情况下， doOnSubscribe() 执行在 subscribe() 发生的线程；而如果**在 doOnSubscribe() 之后有 subscribeOn() 的话，它将执行在离它最近的 subscribeOn() 所指定的线程**。
示例代码：

```
Observable.create(onSubscribe)
        .subscribeOn(Schedulers.io())
        .doOnSubscribe(new Action0() {
            @Override
            public void call() {
                progressBar.setVisibility(View.VISIBLE); // 需要在主线程执行
            }
        })
        .subscribeOn(AndroidSchedulers.mainThread()) // 指定主线程
        .observeOn(AndroidSchedulers.mainThread())
        .subscribe(subscriber);
```

如上，在 doOnSubscribe()的后面跟一个 subscribeOn() ，就能指定准备工作的线程了。

###  

## RxJava 的适用场景和使用方式

### 1. 与 Retrofit 的结合

Retrofit 是 Square 的一个著名的网络请求库。没有用过 Retrofit的可以选择跳过这一小节也没关系，我举的每种场景都只是个例子，而且例子之间并无前后关联，只是个抛砖引玉的作用，所以你跳过这里看别的场景也可以的。
Retrofit 除了提供了传统的 Callback 形式的 API，还有 RxJava版本的 Observable 形式 API。下面我用对比的方式来介绍 Retrofit 的 RxJava 版 API和传统版本的区别。
以获取一个 User 对象的接口作为例子。
使用Retrofit 的传统API，你可以用这样的方式来定义请求：

```
@GET("/user")
public void getUser(@Query("userId") String userId, Callback<User> callback);
```

在程序的构建过程中， Retrofit会把自动把方法实现并生成代码，然后开发者就可以利用下面的方法来获取特定用户并处理响应：

```
getUser(userId, new Callback<User>() {
    @Override
    public void success(User user) {
        userView.setUser(user);
    }
    @Override
    public void failure(RetrofitError error) {
// Error handling
...
    }
};
```

而使用 RxJava 形式的 API，定义同样的请求是这样的：

```
@GET("/user")
public Observable<User> getUser(@Query("userId") String userId);

```

使用的时候是这样的：

```
getUser(userId)
        .observeOn(AndroidSchedulers.mainThread())
        .subscribe(new Observer<User>() {
            @Override
            public void onNext(User user) {
                userView.setUser(user);
            }
            @Override
            public void onCompleted() {
            }
            @Override
            public void onError(Throwable error) {
// Error handling
...
            }
        });

```

看到区别了吗？
当 RxJava 形式的时候，Retrofit把请求封装进 Observable ，在请求结束后调用 onNext() 或在请求失败后调用 onError()。
对比来看， Callback 形式和 Observable 形式长得不太一样，但本质都差不多，而且在细节上 Observable 形式似乎还比 Callback形式要差点。那Retrofit 为什么还要提供 RxJava 的支持呢？
因为它好用啊！从这个例子看不出来是因为这只是最简单的情况。而一旦情景复杂起来， Callback 形式马上就会开始让人头疼。
比如：
假设这么一种情况：你的程序取到的 User 并不应该直接显示，而是需要先与数据库中的数据进行比对和修正后再显示。使用 Callback方式大概可以这么写：

```
getUser(userId, new Callback<User>() {
    @Override
    public void success(User user) {
        processUser(user); // 尝试修正 User 数据
        userView.setUser(user);
    }
    @Override
    public void failure(RetrofitError error) {
// Error handling
...
    }
};

```

有问题吗？
很简便，但不要这样做。为什么？因为这样做会影响性能。数据库的操作很重，一次读写操作花费
10\~20ms是很常见的，这样的耗时很容易造成界面的卡顿。所以通常情况下，如果可以的话一定要避免在主线程中处理数据库。所以为了提升性能，这段代码可以优化一下：

```
getUser(userId, new Callback<User>() {
    @Override
    public void success(User user) {
        new Thread() {
            @Override
            public void run() {
                processUser(user); // 尝试修正 User 数据
                runOnUiThread(new Runnable() { // 切回 UI 线程
                    @Override
                    public void run() {
                        userView.setUser(user);
                    }
                });
            }).start();
        }
        @Override
        public void failure(RetrofitError error) {
// Error handling
...
        }
    };

```

性能问题解决，但……这代码实在是太乱了，迷之缩进啊！杂乱的代码往往不仅仅是美观问题，因为代码越乱往往就越难读懂，而如果项目中充斥着杂乱的代码，无疑会降低代码的可读性，造成团队开发效率的降低和出错率的升高。
这时候，如果用 RxJava 的形式，就好办多了。 RxJava 形式的代码是这样的：

```
getUser(userId)
    .doOnNext(new Action1<User>() {
        @Override
        public void call(User user) {
            processUser(user);
        })
    .observeOn(AndroidSchedulers.mainThread())
    .subscribe(new Observer<User>() {
        @Override
        public void onNext(User user) {
            userView.setUser(user);
        }

        @Override
        public void onCompleted() {
        }

        @Override
        public void onError(Throwable error) {
            // Error handling
            ...
        }
    });

```

后台代码和前台代码全都写在一条链中，明显清晰了很多。
再举一个例子：假设 /user 接口并不能直接访问，而需要填入一个在线获取的 token ，代码应该怎么写？
Callback 方式，可以使用嵌套的 Callback：

```
@GET("/token")
public void getToken(Callback<String> callback);

@GET("/user")
public void getUser(@Query("token") String token, @Query("userId") String userId, Callback<User> callback);

...

getToken(new Callback<String>() {
    @Override
    public void success(String token) {
        getUser(token, userId, new Callback<User>() {
            @Override
            public void success(User user) {
                userView.setUser(user);
            }

            @Override
            public void failure(RetrofitError error) {
                // Error handling
                ...
            }
        };
    }

    @Override
    public void failure(RetrofitError error) {
        // Error handling
        ...
    }
});

```

倒是没有什么性能问题，可是迷之缩进毁一生，你懂我也懂，做过大项目的人应该更懂。
而使用 RxJava 的话，代码是这样的：

```
@GET("/token")
public Observable<String> getToken();

@GET("/user")
public Observable<User> getUser(@Query("token") String token, @Query("userId") String userId);

...

getToken()
    .flatMap(new Func1<String, Observable<User>>() {
        @Override
        public Observable<User> onNext(String token) {
            return getUser(token, userId);
        })
    .observeOn(AndroidSchedulers.mainThread())
    .subscribe(new Observer<User>() {
        @Override
        public void onNext(User user) {
            userView.setUser(user);
        }

        @Override
        public void onCompleted() {
        }

        @Override
        public void onError(Throwable error) {
            // Error handling
            ...
        }
    });

```

用一个 flatMap() 就搞定了逻辑，依然是一条链。看着就很爽，是吧？
加上我写的一个 Sample 项目：   
[rengwuxian RxJava Samples](https://github.com/rengwuxian/RxJavaSamples)
好，Retrofit 部分就到这里。

### 2. RxBinding

[RxBinding](https://github.com/JakeWharton/RxBinding) 是 Jake Wharton的一个开源库，它提供了一套在 Android 平台上的基于 RxJava 的 Binding API。所谓Binding，就是类似设置 OnClickListener 、设置 TextWatcher 这样的注册绑定对象的API。
举个设置点击监听的例子。使用 RxBinding ，可以把事件监听用这样的方法来设置：

```
Button button = ...;
RxView.clickEvents(button) // 以 Observable 形式来反馈点击事件
    .subscribe(new Action1<ViewClickEvent>() {
        @Override
        public void call(ViewClickEvent event) {
            // Click handling
        }
    });

```

看起来除了形式变了没什么区别，实质上也是这样。甚至如果你看一下它的源码，你会发现它连实现都没什么惊喜：它的内部是直接用一个包裹着的 setOnClickListener() 来实现的。然而，仅仅这一个形式的改变，却恰好就是 RxBinding 的目的：扩展性。通过 RxBinding 把点击监听转换成 Observable 之后，就有了对它进行扩展的可能。扩展的方式有很多，根据需求而定。一个例子是前面提到过的 throttleFirst() ，用于去抖动，也就是消除手抖导致的快速连环点击：

```
RxView.clickEvents(button)
    .throttleFirst(500, TimeUnit.MILLISECONDS)
    .subscribe(clickAction);

```

如果想对 RxBinding 有更多了解，可以去它的 [GitHub项目](https://github.com/JakeWharton/RxBinding) 下面看看。

### 3. 各种异步操作

前面举的 Retrofit 和 RxBinding 的例子，是两个可以提供现成的 Observable 的库。而如果你有某些异步操作无法用这些库来自动生成 Observable，也完全可以自己写。例如数据库的读写、大图片的载入、文件压缩/解压等各种需要放在后台工作的耗时操作，都可以用RxJava 来实现，有了之前几章的例子，这里应该不用再举例了。

### 4. RxBus

RxBus 名字看起来像一个库，但它并不是一个库，而是一种模式，它的思想是使用 RxJava来实现了 EventBus ，而让你不再需要使用 Otto 或者 GreenRobot的 EventBus。至于什么是
RxBus，可以看[这篇文章](http://nerds.weddingpartyapp.com/tech/2014/12/24/implementing-an-event-bus-with-rxjava-rxbus/)。顺便说一句，Flipboard已经用 RxBus 替换掉了 Otto ，目前为止没有不良反应。

## 操作符的使用

### merge操作符，合并观察对象

```
List<String> list1 = new ArrayList<>() ;
List<String> list2 = new ArrayList<>() ;

list1.add( "1" ) ;
list1.add( "2" ) ;
list1.add( "3" ) ;

list2.add( "a" ) ;
list2.add( "b" ) ;
list2.add( "c" ) ;

Observable observable1 = Observable.from( list1 ) ;
Observable observable2 = Observable.from( list2 ) ;

//合并数据  先发送observable2的全部数据，然后发送 observable1的全部数据
Observable observable = Observable.merge( observable2 , observable1 ) ;

observable.subscribe(new Action1() {
    @Override
    public void call(Object o) {
      System.out.println( "rx-- " + o );
    }
}) ;

```

运行结果

![](http://img.blog.csdn.net/20171231014104929?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### zip  操作符，合并多个观察对象的数据。并且允许 Func2（）函数重新发送合并后的数据

```
List<String> list1 = new ArrayList<>() ;
List<String> list2 = new ArrayList<>() ;

list1.add( "1" ) ;
list1.add( "2" ) ;
list1.add( "3" ) ;

list2.add( "a" ) ;
list2.add( "b" ) ;
list2.add( "c" ) ;
list2.add( "d" ) ;

Observable observable1 = Observable.from( list1 ) ;
Observable observable2 = Observable.from( list2 ) ;

Observable observable3 =  Observable.zip(observable1, observable2, new Func2<String , String , String >() {
   @Override
   public String call(String s1 , String s2 ) {
	   return s1 + s2  ;
   }
}) ;

observable3.subscribe(new Action1() {
   @Override
   public void call(Object o) {
	   System.out.println( "zip-- " + o );
   }
}) ;

```

运行效果：**从效果图上可以看出，合并两个的观察对象数据项应该是相等的；如果出现了数据项不等的情况，合并的数据项以最小数据队列为准。**
![](http://img.blog.csdn.net/20171231013955391?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### scan累加器操作符

```
Observable observable = Observable.just( 1 , 2 , 3 , 4 , 5  ) ;
observable.scan(new Func2<Integer,Integer,Integer>() {
           @Override
           public Integer call(Integer o, Integer o2) {
               return o + o2 ;
           }
       })
         .subscribe(new Action1() {
             @Override
             public void call(Object o) {
                 System.out.println( "scan-- " +  o );
             }
         })   ;

```

运行效果：     
第一次发射得到1，作为结果与2相加；发射得到3，作为结果与3相加，以此类推，打印结果：
![scan累加器操作符](http://img.blog.csdn.net/20171231014859559?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### filter 过滤操作符的使用

```
Observable observable = Observable.just( 1 , 2 , 3 , 4 , 5 , 6 , 7 ) ;
observable.filter(new Func1<Integer , Boolean>() {
    @Override
    public Boolean call(Integer o) {
        //数据大于4的时候才会被发送
        return o > 4 ;
    }
})
        .subscribe(new Action1() {
            @Override
            public void call(Object o) {
                System.out.println( "filter-- " +  o );
            }
        })   ;

```

运行效果
![filter 过滤操作符](http://img.blog.csdn.net/20171231014846352?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### 消息数量过滤操作符的使用

#### take ：取前n个数据

#### takeLast：取后n个数据

#### first 只发送第一个数据

#### last 只发送最后一个数据

#### skip() 跳过前n个数据发送后面的数据

#### skipLast() 跳过最后n个数据，发送前面的数据

```
//take 发送前3个数据
Observable observable = Observable.just( 1 , 2 , 3 , 4 , 5 , 6 , 7 ) ;
observable.take( 3 )
	   .subscribe(new Action1() {
		   @Override
		   public void call(Object o) {
			   System.out.println( "take-- " +  o );
		   }
	   })   ;

//takeLast 发送最后三个数据
Observable observable2 = Observable.just( 1 , 2 , 3 , 4 , 5 , 6 , 7 ) ;
observable2.takeLast( 3 )
	   .subscribe(new Action1() {
		   @Override
		   public void call(Object o) {
			   System.out.println( "takeLast-- " +  o );
		   }
	   })   ;

//first 只发送第一个数据
Observable observable3 = Observable.just( 1 , 2 , 3 , 4 , 5 , 6 , 7 ) ;
observable3.first()
	   .subscribe(new Action1() {
		   @Override
		   public void call(Object o) {
			   System.out.println( "first-- " +  o );
		   }
	   })   ;

//last 只发送最后一个数据
Observable observable4 = Observable.just( 1 , 2 , 3 , 4 , 5 , 6 , 7 ) ;
observable4.last()
	   .subscribe(new Action1() {
		   @Override
		   public void call(Object o) {
			   System.out.println( "last-- " +  o );
		   }
	   })   ;

//skip() 跳过前2个数据发送后面的数据
Observable observable5 = Observable.just( 1 , 2 , 3 , 4 , 5 , 6 , 7 ) ;
observable5.skip( 2 )
	   .subscribe(new Action1() {
		   @Override
		   public void call(Object o) {
			   System.out.println( "skip-- " +  o );
		   }
	   })   ;

//skipLast() 跳过最后两个数据，发送前面的数据
Observable observable6 = Observable.just( 1 , 2 , 3 , 4 , 5 , 6 , 7 ) ;
observable5.skipLast( 2 )
	   .subscribe(new Action1() {
		   @Override
		   public void call(Object o) {
			   System.out.println( "skipLast-- " +  o );
		   }
	   })   ;

```

效果图
![消息数量过滤操作符](http://img.blog.csdn.net/20171231014837150?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### elementAt 、elementAtOrDefault发送数据序列中第n个数据

```
//elementAt() 发送数据序列中第n个数据 ，序列号从0开始
//如果该序号大于数据序列中的最大序列号，则会抛出异常，程序崩溃
//所以在用elementAt操作符的时候，要注意判断发送的数据序列号是否越界

Observable observable7 = Observable.just( 1 , 2 , 3 , 4 , 5 , 6 , 7 ) ;
observable7.elementAt( 3 )
		.subscribe(new Action1() {
			@Override
			public void call(Object o) {
				System.out.println( "elementAt-- " +  o );
			}
		})   ;

//elementAtOrDefault( int n , Object default ) 发送数据序列中第n个数据 ，序列号从0开始。
//如果序列中没有该序列号，则发送默认值
Observable observable9 = Observable.just( 1 , 2 , 3 , 4 , 5 ) ;
observable9.elementAtOrDefault(  8 , 666  )
		.subscribe(new Action1() {
			@Override
			public void call(Object o) {
				System.out.println( "elementAtOrDefault-- " +  o );
			}
		})   ;

```

运行结果
![elementAt](http://img.blog.csdn.net/20171231014828107?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### startWith() 插入数据

```
//插入普通数据
//startWith 数据序列的开头插入一条指定的项 , 最多插入9条数据
Observable observable = Observable.just( "aa" , "bb" , "cc" ) ;
observable
        .startWith( "11" , "22" )
        .subscribe(new Action1() {
            @Override
            public void call(Object o) {
                System.out.println( "startWith-- " + o );
            }
        }) ;
 
//插入Observable对象
List<String> list = new ArrayList<>() ;
list.add( "ww" ) ;
list.add( "tt" ) ;
observable.startWith( Observable.from( list ))
        .subscribe(new Action1() {
            @Override
            public void call(Object o) {
                System.out.println( "startWith2 -- " + o );
            }
        }) ;

```

　　运行结果
![startWith()](http://img.blog.csdn.net/20171231014818246?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### delay操作符，延迟数据发送

```
Observable<String> observable = Observable.just( "1" , "2" , "3" , "4" , "5" , "6" , "7" , "8" ) ;
//延迟数据发射的时间，仅仅延时一次，也就是发射第一个数据前延时。发射后面的数据不延时
observable.delay( 3 , TimeUnit.SECONDS )  //延迟3秒钟
	   .subscribe(new Action1() {
		   @Override
		   public void call(Object o) {
			   System.out.println("delay-- " + o);
		   }
	   }) ;

```

### Timer  延时操作符的使用

使用场景：xx秒后，执行xx     

```
//5秒后输出 hello world , 然后显示一张图片
Observable.timer( 5 , TimeUnit.SECONDS )
		.observeOn(AndroidSchedulers.mainThread() )
		.subscribe(new Action1<Long>() {
			@Override
			public void call(Long aLong) {
				System.out.println( "timer--hello world " +  aLong );
				findViewById( R.id.image).setVisibility(View.VISIBLE );
			}
		}) ;

```

```
timer 返回一个 Observable , 它在延迟一段给定的时间后发射一个简单的数字0
timer 操作符默认在computation调度器上执行，当然也可以用 Scheduler在定义执行的线程。

```

#### delay vs timer

- 相同点：delay 、 timer 都是延时操作符。
- 不同点：
  delay延时一次，延时完成后，可以连续发射多个数据。
  timer延时一次，延时完成后，只发射一次数据。

### interval 轮询操作符，循环发送数据，数据从0开始递增

```
public class IntervalActivity extends AppCompatActivity { 
    Subscription subscription ;
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_interval);
 
        //参数一：延迟时间  参数二：间隔时间  参数三：时间颗粒度
        Observable observable =  Observable.interval(3000, 3000, TimeUnit.MILLISECONDS) ;
        subscription = observable.subscribe(new Action1() {
            @Override
            public void call(Object o) {
                System.out.println( "interval-  " + o );
            }
        })  ;
    }
 
    @Override
    protected void onDestroy() {
        super.onDestroy();
        if ( subscription != null ){
            subscription.unsubscribe();
        }
    }
}

```

![interval 轮询操作符](http://img.blog.csdn.net/20171231014808440?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### doOnNext() 操作符，在每次 OnNext() 方法被调用前执行

使用场景：从网络请求数据，在数据被展示前，缓存到本地

```
Observable observable = Observable.just( "1" , "2" , "3" , "4" ) ;
observable.doOnNext(new Action1() {
   @Override
   public void call(Object o) {
	   System.out.println( "doOnNext--缓存数据" + o  );
   }
})
	   .subscribe(new Observer() {
		   @Override
		   public void onCompleted() {

		   }

		   @Override
		   public void onError(Throwable e) {

		   }

		   @Override
		   public void onNext(Object o) {
			   System.out.println( "onNext--" + o  );
		   }
	   }) ;

```

![doOnNext() 操作符](http://img.blog.csdn.net/20171231014756279?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### Buffer 操作符

- Buffer( int n )      把n个数据打成一个list包，然后再次发送。
- Buffer( int n , int skip)   把n个数据打成一个list包，然后跳过第skip个数据。

使用场景：一个按钮每点击3次，弹出一个toast      

```
List<String> list = new ArrayList<>();
for (int i = 1; i < 10; i++) {
  list.add("" + i);
}

Observable<String> observable = Observable.from(list);
observable
	  .buffer(2)   //把每两个数据为一组打成一个包，然后发送
	  .subscribe(new Action1<List<String>>() {
		  @Override
		  public void call(List<String> strings) {
			  System.out.println( "buffer---------------" );
			  Observable.from( strings ).subscribe(new Action1<String>() {
				  @Override
				  public void call(String s) {
					  System.out.println( "buffer data --" + s  );
				  }
			  }) ;
		  }
	  });

```

![Buffer 操作符](http://img.blog.csdn.net/20171231014747010?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

**例子2：** 

```
//第1、2 个数据打成一个数据包，跳过第三个数据 ； 第4、5个数据打成一个包，跳过第6个数据
observable.buffer( 2 , 3 )  //把每两个数据为一组打成一个包，然后发送。第三个数据跳过去
	   .subscribe(new Action1<List<String>>() {
		   @Override
		   public void call(List<String> strings) {

			   System.out.println( "buffer22---------------" );
			   Observable.from( strings ).subscribe(new Action1<String>() {
				   @Override
				   public void call(String s) {
					   System.out.println( "buffer22 data --" + s  );
				   }
			   }) ;
		   }
	   }) ;

```

![Buffer 操作符2](http://img.blog.csdn.net/20171231014734518?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### throttleFirst 操作符在一段时间内，只取第一个事件，然后其他事件都丢弃。

**使用场景**：
1、button按钮防抖操作，防连续点击  
2、百度关键词联想，在一段时间内只联想一次，防止频繁请求服务器   

```
 Observable.interval( 1 , TimeUnit.SECONDS)
			.throttleFirst( 3 , TimeUnit.SECONDS )
			.subscribe(new Action1<Long>() {
				@Override
				public void call(Long aLong) {
					System.out.println( "throttleFirst--" + aLong );
				}
			}) ;

```

**这段代码，是循环发送数据，每秒发送一个。**throttleFirst( 3 , TimeUnit.SECONDS
)   **在3秒内只取第一个事件，其他的事件丢弃。**
运行结果
![throttleFirst 操作符](http://img.blog.csdn.net/20171231014723739?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

### 14、distinct    过滤重复的数据

```
List<String> list = new ArrayList<>() ;
list.add( "1" ) ;
list.add( "2" ) ;
list.add( "1" ) ;
list.add( "3" ) ;
list.add( "4" ) ;
list.add( "2" ) ;
list.add( "1" ) ;
list.add( "1" ) ;

Observable.from( list )
		.distinct()
		.subscribe(new Action1<String>() {
			@Override
			public void call(String s) {
				System.out.println( "distinct--" + s );
			}
		}) ;

```

![distinct过滤重复的数据](http://img.blog.csdn.net/20171231014711548?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
从结果可以看出，重复的数据已经被过滤掉了

**distinctUntilChanged**()  过滤连续重复的数据

```
List<String> list = new ArrayList<>() ;
list.add( "1" ) ;
list.add( "2" ) ;
list.add( "1" ) ;
list.add( "3" ) ;
list.add( "4" ) ;
list.add( "4" ) ;
list.add( "2" ) ;
list.add( "1" ) ;
list.add( "1" ) ;

Observable.from( list )
		.distinctUntilChanged()
		.subscribe(new Action1<String>() {
			@Override
			public void call(String s) {
				System.out.println( "distinctUntilChanged--" + s );
			}
		}) ;

```

 运行结果
![distinctUntilChanged()过滤连续重复的数据](http://img.blog.csdn.net/20171231014700941?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
从结果可以看出，连续重复的数据已经被过滤掉了

### debounce() 操作符一段时间内没有变化，就会发送一个数据

使用场景：百度关键词联想提示。在输入的过程中是不会从服务器拉数据的。当输入结束后，在400毫秒没有输入就会去获取数据。
 避免了，多次请求给服务器带来的压力.

### doOnSubscribe()

使用场景： 可以在**事件发出之前做一些初始化的工作**，比如弹出进度条等等
**注意**：

1. doOnSubscribe()默认运行在事件产生的线程里面，然而事件产生的线程一般都会运行在 io
   线程里。那么这个时候做一些，更新UI的操作，是线程不安全的。

   ```
              所以如果事件产生的线程是io线程，但是我们又要在doOnSubscribe()

   ```

   更新UI ，这时候就需要线程切换。

2. 如果在 doOnSubscribe() 之后有 subscribeOn() 的话，它将执行在离它最近的 subscribeOn() 所指定的线程。 

3. subscribeOn() ：事件产生的线程 ； observeOn() : 事件消费的线程

```
Observable.create(onSubscribe)
.subscribeOn(Schedulers.io())
.doOnSubscribe(new Action0() {
	@Override
	public void call() {
		progressBar.setVisibility(View.VISIBLE); // 需要在主线程执行
	}
})
.subscribeOn(AndroidSchedulers.mainThread()) // 指定主线程
.observeOn(AndroidSchedulers.mainThread())
.subscribe(subscriber);

```

### range 操作符

首先看range 方法的源码

```
public static Observable<Integer> range(int start, int count) {
     if (count < 0) {
         throw new IllegalArgumentException("Count can not be negative");
     }
     if (count == 0) {
         return Observable.empty();
     }
     if (start > Integer.MAX_VALUE - count + 1) {
         throw new IllegalArgumentException("start + count can not exceed Integer.MAX_VALUE");
     }
     if(count == 1) {
         return Observable.just(start);
     }
     return Observable.create(new OnSubscribeRange(start, start + (count - 1)));
 }
 
  
 //可以通过第三个参数控制range执行的线程
 public static Observable<Integer> range(int start, int count, Scheduler scheduler) {
     return range(start, count).subscribeOn(scheduler);
 }

```

Range操作符**发射一个范围内的有序整数序列，你可以指定范围的起始和长度**。
RxJava将这个操作符实现为range函数，它接受两个参数，一个是范围的起始值，一个是范围的数据的数目。如果你将第二个参数设为0，将导致Observable不发射任何数据（如果设置为负数，会抛异常）。
range默认不在任何特定的调度器上执行。有一个变体可以通过可选参数指定Scheduler。
**例子**

```
Observable.range( 10 , 3 )
		  .subscribe(new Action1<Integer>() {
			  @Override
			  public void call(Integer integer) {
				  Log.v( "rx_range  " , "" + integer ) ;
			  }
		  }) ;

```

**结果**

```
/rx_range: 10  
/rx_range: 11  
/rx_range: 12

```

### defer 操作符

例子

```
public class DeferActivity extends AppCompatActivity {
 
    String i = "10" ;
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_defer);
 
        i = "11 " ;
 
        Observable<String> defer = Observable.defer(new Func0<Observable<String>>() {
            @Override
            public Observable<String> call() {
                return Observable.just( i ) ;
            }
        }) ;
 
        Observable test = Observable.just(  i ) ;
 
        i = "12" ;
 
        defer.subscribe(new Action1<String>() {
            @Override
            public void call(String s) {
                Log.v( "rx_defer  " , "" + s ) ;
            }
        }) ;
 
        test.subscribe(new Action1() {
            @Override
            public void call(Object o) {
                Log.v( "rx_just " , "" + o ) ;
            }
        }) ;
    }
}

```

结果

```
/rx_defer: 12  
/rx_just: 11

```

- 可以看到，**just**操作符是在**创建Observable**就进行了**赋值**操作，而**defer**是在订阅者**订阅时**才创建Observable，此时才进行真正的**赋值**操作。
- Defer操作符会一直等待直到有观察者订阅它，然后它使用Observable工厂方法生成一个Observable。它对每个观察者都这样做，因此尽管每个订阅者都以为自己订阅的是同一个Observable，事实上每个订阅者获取的是它们自己的单独的数据序列。
- 在某些情况下，等待直到最后一分钟（就是直到订阅发生时）才生成Observable可以确保Observable包含最新的数据。

## 生命周期控制和内存优化

 RxJava使我们很方便的使用链式编程，代码看起来既简洁又优雅。但是RxJava使用起来也是有副作用的，使用**越来越多的订阅，内存开销也会变得很大**，稍不留神就会出现**内存溢出**的情况。

### 取消订阅 subscription.unsubscribe() ;

```
public class MainActivity extends AppCompatActivity {

    Subscription subscription ;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        subscription =  Observable.just( "123").subscribe(new Action1<String>() {
            @Override
            public void call(String s) {
                System.out.println( "tt--" + s );
            }
        }) ;
    }

    @Override
    protected void onDestroy() {
        super.onDestroy();
        //取消订阅
        if ( subscription != null ){
            subscription.unsubscribe();
        }
    }
}

```

### 线程调度

**Scheduler**调度器，相当于线程控制器，介绍见上文【线程控制 —— Scheduler (一)】【线程控制：Scheduler (二)】

> 上面介绍了两种控制Rxjava生命周期的方式，第一种：取消订阅 ；第二种：线程切换。这两种方式都能有效的解决android内存的使用问题，但是在实际的项目中会出现很多订阅关系，那么取消订阅的代码也就越来越多。造成了项目很难维护。所以我们必须寻找其他可靠简单可行的方式，也就是下面要介绍的。

### rxlifecycle 框架的使用

- github地址： <https://github.com/trello/RxLifecycle>
- 在android studio 里面添加引用  
  compile 'com.trello:rxlifecycle-components:0.6.1'
- 让你的activity继承RxActivity,RxAppCompatActivity,RxFragmentActivity  
  让你的fragment继承RxFragment,RxDialogFragment;
  下面的代码就以RxAppCompatActivity举例

#### **bindToLifecycle** 方法

在子类使用Observable中的**compose**操作符，调用，完成Observable发布的事件和当前的组件绑定，实现生命周期同步。从而实现当前组件生命周期结束时，自动取消对Observable订阅。

```
public class MainActivity extends RxAppCompatActivity {
        TextView textView ;
        
        @Override
        protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        textView = (TextView) findViewById(R.id.textView);
    
        //循环发送数字
        Observable.interval(0, 1, TimeUnit.SECONDS)
            .subscribeOn( Schedulers.io())
            .compose(this.<Long>bindToLifecycle())   //这个订阅关系跟Activity绑定，Observable 和activity生命周期同步
            .observeOn( AndroidSchedulers.mainThread())
            .subscribe(new Action1<Long>() {
                @Override
                public void call(Long aLong) {
                    System.out.println("lifecycle--" + aLong);
                    textView.setText( "" + aLong );
                }
            });
       }
    }

```

上面的代码是Observable循环的发送数字，并且在textview中显示出来  
1、没加 compose(this.<Long>bindToLifecycle()) 当Activiry结束掉以后，Observable还是会不断的发送数字，订阅关系没有解除  
2、添加compose(this.<Long>bindToLifecycle()) 当Activity结束掉以后，Observable停止发送数据，订阅关系解除。

- 从上面的例子可以看出bindToLifecycle() 方法可以使Observable发布的事件和当前的Activity绑定，实现生命周期同步。也就是Activity的 onDestroy() 方法被调用后，Observable的订阅关系才解除。

#### **bindUntilEvent**( ActivityEvent event)

指定在Activity其他的生命状态和订阅关系保持同步

- ActivityEvent.CREATE: 在Activity的onCreate()方法执行后，解除绑定。
- ActivityEvent.START:在Activity的onStart()方法执行后，解除绑定。
- ActivityEvent.RESUME:在Activity的onResume()方法执行后，解除绑定。
- ActivityEvent.PAUSE: 在Activity的onPause()方法执行后，解除绑定。
- ActivityEvent.STOP:在Activity的onStop()方法执行后，解除绑定。
- ActivityEvent.DESTROY:在Activity的onDestroy()方法执行后，解除绑定。

```
 //循环发送数字
 Observable.interval(0, 1, TimeUnit.SECONDS)
		 .subscribeOn( Schedulers.io())
		 .compose(this.<Long>bindUntilEvent(ActivityEvent.STOP ))   //当Activity执行Onstop()方法是解除订阅关系
		 .observeOn( AndroidSchedulers.mainThread())
		 .subscribe(new Action1<Long>() {
			 @Override
			 public void call(Long aLong) {
				 System.out.println("lifecycle-stop-" + aLong);
				 textView.setText( "" + aLong );
			 }
		 });

```

经过测试发现，当Activity执行了onStop()方法后，订阅关系已经解除了。  

#### FragmentEvent

这个类是专门**处理订阅事件与Fragment生命周期同步**的大杀器

```
public enum FragmentEvent {
ATTACH,
CREATE,
CREATE_VIEW,
START,
RESUME,
PAUSE,
STOP,
DESTROY_VIEW,
DESTROY,
DETACH
}

```

可以看出FragmentEvent 和 ActivityEvent 类似，都是枚举类，用法是一样的。这里就不举例了！

###  

## RxBinding

**前言**：RxBinding 是 Jake Wharton 的一个开源库，它提供了一套在 Android
平台上的基于 RxJava的 Binding API。所谓 Binding，就是类似设置 OnClickListener
、设置 TextWatcher 这样的注册绑定对象的 API。

### git地址

<https://github.com/JakeWharton/RxBinding>

### androidStudio 使用

一般的包下面就用

```
compile 'com.jakewharton.rxbinding:rxbinding:0.4.0'

```

v4'support-v4' library bindings:

```
compile 'com.jakewharton.rxbinding:rxbinding-support-v4:0.4.0'

```

'appcompat-v7' library bindings:

```
compile 'com.jakewharton.rxbinding:rxbinding-appcompat-v7:0.4.0'

```

'design' library bindings:

```
compile 'com.jakewharton.rxbinding:rxbinding-design:0.4.0'

```

### 代码示例

#### Button 防抖处理

```
 button = (Button) findViewById( R.id.bt ) ;
 RxView.clicks( button )
		 .throttleFirst( 2 , TimeUnit.SECONDS )   //两秒钟之内只取一个点击事件，防抖操作
		 .subscribe(new Action1<Void>() {
			 @Override
			 public void call(Void aVoid) {
				 Toast.makeText(MainActivity.this, "点击了", Toast.LENGTH_SHORT).show();
			 }
		 }) ;

```

#### 按钮的长按时间监听

```
 button = (Button) findViewById( R.id.bt ) ;
 //监听长按时间
 RxView.longClicks( button)
      .subscribe(new Action1<Void>() {
          @Override
         public void call(Void aVoid) {
         Toast.makeText(MainActivity.this, "long click  ！！", Toast.LENGTH_SHORT).show();
         }
     }) ;

```

#### listView 的点击事件、长按事件处理

```java
listView = (ListView) findViewById( R.id.listview );
List<String> list = new ArrayList<>() ;
     for ( int i = 0 ; i < 40 ; i++ ){
         list.add( "sss" + i ) ;
     }
final ArrayAdapter<String> adapter = new ArrayAdapter<String>(this, android.R.layout.simple_expandable_list_item_1 );
adapter.addAll( list );
listView.setAdapter( adapter );

//item click event
RxAdapterView.itemClicks( listView )
     .subscribe(new Action1<Integer>() {
         @Override
         public void call(Integer integer) {
         Toast.makeText(ListActivity.this, "item click " + integer , Toast.LENGTH_SHORT).show();
             }
         }) ;

//item long click
RxAdapterView.itemLongClicks( listView)
     .subscribe(new Action1<Integer>() {
         @Override
         public void call(Integer integer) {
             Toast.makeText(ListActivity.this, "item long click " + integer , Toast.LENGTH_SHORT).show();
             }
         }) ;
```

#### 用户登录界面，勾选同意隐私协议，登录按钮就变高亮

```java
button = (Button) findViewById( R.id.login_bt );
checkBox = (CheckBox) findViewById( R.id.checkbox );
//checkedChanges
RxCompoundButton.checkedChanges( checkBox )
    .subscribe(new Action1<Boolean>() {
        @Override
        public void call(Boolean aBoolean) {
            button.setEnabled( aBoolean );
            button.setBackgroundResource( aBoolean ? R.color.button_yes : R.color.button_no );
            }
        }) ;
```

效果图
![](http://img.blog.csdn.net/20171231031202522?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
http://o7rvuansr.bkt.clouddn.com/rxbindingGIF.gif

#### 搜索的时候，关键词联想功能 。debounce()在一定的时间内没有操作就会发送事件。

```
editText = (EditText) findViewById( R.id.editText );
 listView = (ListView) findViewById( R.id.listview );

 final ArrayAdapter<String> adapter = new ArrayAdapter<String>(this, android.R.layout.simple_expandable_list_item_1 );
 listView.setAdapter( adapter );

 RxTextView.textChanges( editText )
             .debounce( 600 , TimeUnit.MILLISECONDS )
             .map(new Func1<CharSequence, String>() {
                 @Override
                 public String call(CharSequence charSequence) {
                     //get the keyword
                     String key = charSequence.toString() ;
                     return key ;
                 }
             })
             .observeOn( Schedulers.io() )
             .map(new Func1<String, List<String>>() {
                 @Override
                 public List<String> call(String keyWord ) {
                     //get list
                     List<String> dataList = new ArrayList<String>() ;
                     if ( ! TextUtils.isEmpty( keyWord )){
                         for ( String s : getData()  ) {
                             if (s != null) {
                                 if (s.contains(keyWord)) {
                                     dataList.add(s);
                                 }
                             }
                         }
                     }
                     return dataList ;
                 }
             })
             .observeOn( AndroidSchedulers.mainThread() )
             .subscribe(new Action1<List<String>>() {
                 @Override
                 public void call(List<String> strings) {
                     adapter.clear();
                     adapter.addAll( strings );
                     adapter.notifyDataSetChanged();
                 }
             }) ;

```

运行效果  

![](http://img.blog.csdn.net/20171231030222634?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvbW9pcmEzMw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)
http://o7rvuansr.bkt.clouddn.com/rxbinding_edGIF.gif
**总结**：

- RxBinding就是把 发布--订阅 的模式用在了android控件的点击，文本变化上。通过RxBinding 把点击监听转换成 Observable 之后，就有了对它进行扩展的可能。
- RxBinding和rxlifecycle 结合起来使用，可以控制控件监听的生命周期。

###  

## 线程调度实例

### Rxjava默认运行的线程

**例1**

```
Observable
   .create(new Observable.OnSubscribe<String>() {
	   @Override
	   public void call(Subscriber<? super String> subscriber) {
		   Logger.v( "rx_call" , Thread.currentThread().getName()  );

		   subscriber.onNext( "dd");
		   subscriber.onCompleted();
	   }
   })
   .map(new Func1<String, String >() {
	   @Override
	   public String call(String s) {
		   Logger.v( "rx_map" , Thread.currentThread().getName()  );
		   return s + "88";
	   }
   })
   .subscribe(new Action1<String>() {
	   @Override
	   public void call(String s) {
		   Logger.v( "rx_subscribe" , Thread.currentThread().getName()  );
	   }
   }) ;

```

**结果**

```
/rx_call: main           -- 主线程  
/rx_map: main        --  主线程  
/rx_subscribe: main   -- 主线程

```

**例2**

```
 new Thread(new Runnable() {
	   @Override
	   public void run() {
		   Logger.v( "rx_newThread" , Thread.currentThread().getName()  );
		   rx();
	   }
   }).start();
 
void rx(){
   Observable
	   .create(new Observable.OnSubscribe<String>() {
		   @Override
		   public void call(Subscriber<? super String> subscriber) {
			   Logger.v( "rx_call" , Thread.currentThread().getName()  );

			   subscriber.onNext( "dd");
			   subscriber.onCompleted();
		   }
	   })
	   .map(new Func1<String, String >() {
		   @Override
		   public String call(String s) {
			   Logger.v( "rx_map" , Thread.currentThread().getName()  );
			   return s + "88";
		   }
	   })
	   .subscribe(new Action1<String>() {
		   @Override
		   public void call(String s) {
			   Logger.v( "rx_subscribe" , Thread.currentThread().getName()  );
		   }
	   }) ;
}

```

**      结果**

```
/rx_newThread: Thread-564   -- 子线程  
/rx_call: Thread-564              -- 子线程  
/rx_map: Thread-564            -- 子线程   
/rx_subscribe: Thread-564    -- 子线程

```

#### 结论

> 通过例1和例2，说明，Rxjava**默认运行在当前线程**中。如果当前线程是子线程，则rxjava运行在子线程；同样，当前线程是主线程，则rxjava运行在主线程

### subscribeOn、observeOn对线程影响

**例3**

```
Observable
   .create(new Observable.OnSubscribe<String>() {
	   @Override
	   public void call(Subscriber<? super String> subscriber) {
		   Logger.v( "rx_call" , Thread.currentThread().getName()  );
		   subscriber.onNext( "dd");
		   subscriber.onCompleted();
	   }
   })
   .subscribeOn(Schedulers.io())
   .observeOn(AndroidSchedulers.mainThread())
   .map(new Func1<String, String >() {
	   @Override
	   public String call(String s) {
		   Logger.v( "rx_map" , Thread.currentThread().getName()  );
		   return s + "88";
	   }
   })
   .subscribe(new Action1<String>() {
	   @Override
	   public void call(String s) {
		   Logger.v( "rx_subscribe" , Thread.currentThread().getName()  );
	   }
   }) ;

```

**结果**

```
/rx_call: RxCachedThreadScheduler-1    --io线程  
/rx_map: main                                     --主线程  
/rx_subscribe: main                              --主线程

```

**例4**

```
Observable
   .create(new Observable.OnSubscribe<String>() {
	   @Override
	   public void call(Subscriber<? super String> subscriber) {
		   Logger.v( "rx_call" , Thread.currentThread().getName()  );
		   subscriber.onNext( "dd");
		   subscriber.onCompleted();
	   }
   })
   .map(new Func1<String, String >() {
	   @Override
	   public String call(String s) {
		   Logger.v( "rx_map" , Thread.currentThread().getName()  );
		   return s + "88";
	   }
   })
   .subscribeOn(Schedulers.io())
   .observeOn(AndroidSchedulers.mainThread())
   .subscribe(new Action1<String>() {
	   @Override
	   public void call(String s) {
		   Logger.v( "rx_subscribe" , Thread.currentThread().getName()  );
	   }
   }) ;　

```

**结果**

```
/rx_call: RxCachedThreadScheduler-1     --io线程  
/rx_map: RxCachedThreadScheduler-1   --io线程  
/rx_subscribe: main                              --主线程

```

#### 结论

> - 通过例3、例4 可以看出  .**subscribeOn**(Schedulers.io())和 .**observeOn**(AndroidSchedulers.mainThread())写的**位置不一样**，造成的**结果**也**不一样**。
>   从例4中可以看出 map()操作符默认运行在事件产生的线程之中。事件消费只是在 subscribe（） 里面。

- 对于 **create() , just() , from**()   等                 --- 事件**产生**   

  ```
    **map() , flapMap() , scan() , filter**()  等    --  事件**加工**
   **subscribe**()                                          --  事件**消费**

  ```

- 事件**产生**：默认运行在**当前线程**，可以由 **subscribeOn**()  自定义线程
  事件**加工**：默认跟事件**产生**的线程保持**一致**, 可以由 **observeOn**() 自定义线程
   事件**消费**：默认运行在**当前线程**，可以有**observeOn**() 自定义

### 多次切换线程

**例5  **

```
Observable
	.create(new Observable.OnSubscribe<String>() {
		@Override
		public void call(Subscriber<? super String> subscriber) {
			Logger.v( "rx_call" , Thread.currentThread().getName()  );

			subscriber.onNext( "dd");
			subscriber.onCompleted();
		}
	})

	.observeOn( Schedulers.newThread() )    //新线程
	.map(new Func1<String, String >() {
		@Override
		public String call(String s) {
			Logger.v( "rx_map" , Thread.currentThread().getName()  );
			return s + "88";
		}
	})

	.observeOn( Schedulers.io() )      //io线程
	.filter(new Func1<String, Boolean>() {
		@Override
		public Boolean call(String s) {
			Logger.v( "rx_filter" , Thread.currentThread().getName()  );
			return s != null ;
		}
	})

	.subscribeOn(Schedulers.io())     //定义事件产生线程：io线程
	.observeOn(AndroidSchedulers.mainThread())     //事件消费线程：主线程
	.subscribe(new Action1<String>() {
		@Override
		public void call(String s) {
			Logger.v( "rx_subscribe" , Thread.currentThread().getName()  );
		}
	}) ;

```

**结果**

```
/rx_call: RxCachedThreadScheduler-1           -- io 线程  
/rx_map: RxNewThreadScheduler-1             -- new出来的线程  
/rx_filter: RxCachedThreadScheduler-2        -- io线程  
/rx_subscribe: main                                   -- 主线程

```

### 只规定了事件产生的线程

**例6：**

```
Observable
	 .create(new Observable.OnSubscribe<String>() {
		 @Override
		 public void call(Subscriber<? super String> subscriber) {
			 Log.v( "rx--create " , Thread.currentThread().getName() ) ;
			 subscriber.onNext( "dd" ) ;
		 }
	 })
	 .subscribeOn(Schedulers.io())
	 .subscribe(new Action1<String>() {
		 @Override
		 public void call(String s) {
			 Log.v( "rx--subscribe " , Thread.currentThread().getName() ) ;
		 }
	 }) ;

```

**结果**

```
/rx--create: RxCachedThreadScheduler-4                      // io 线程  
/rx--subscribe: RxCachedThreadScheduler-4                 // io 线程

```

### 只规定事件消费线程

**例:7：**

```
Observable
   .create(new Observable.OnSubscribe<String>() {
	   @Override
	   public void call(Subscriber<? super String> subscriber) {
		   Log.v( "rx--create " , Thread.currentThread().getName() ) ;
		   subscriber.onNext( "dd" ) ;
	   }
   })
   .observeOn( Schedulers.newThread() )
   .subscribe(new Action1<String>() {
	   @Override
	   public void call(String s) {
		   Log.v( "rx--subscribe " , Thread.currentThread().getName() ) ;
	   }
   }) ;

```

**结果**

```
/rx--create: main                                           -- 主线程  
/rx--subscribe: RxNewThreadScheduler-1        --  new 出来的子线程 

```

#### 结论

> 从例6可以看出，如果**只规定**了事件**产生**的线程，那么事件**消费**线程将**跟随**事件**产生线程**。
> 从例7可以看出，如果**只规定**了事件**消费**的线程，那么事件**产生**的线程**和当前线程保持一致**。

### 线程调度封装

**例8：**
 在Android 常常有这样的场景，后台处理处理数据，前台展示数据。

#### 一般的用法：

```
Observable
	 .just( "123" )
	 .subscribeOn( Schedulers.io())
	 .observeOn( AndroidSchedulers.mainThread() )
	 .subscribe(new Action1() {
		 @Override
		 public void call(Object o) {
		 }
	 }) ;

```

#### 简单的封装

```
public Observable apply( Observable observable ){
   return observable.subscribeOn( Schedulers.io() )
            .observeOn( AndroidSchedulers.mainThread() ) ;
}

```

**使用**

```
apply( Observable.just( "123" ) )
              .subscribe(new Action1() {
                  @Override
                  public void call(Object o) {
 
                  }
              }) ;

```

**弊端**：虽然上面的这种封装可以做到线程调度的目的，但是它破坏了链式编程的结构，是编程风格变得不优雅。
改进：**Transformers**的使用（就是转化器的意思，把一种类型的Observable转换成另一种类型的Observable ）

#### 改进后的封装

```java
//Transformers转化器：把一种类型的Observable转换成另一种类型的Observable
Observable.Transformer schedulersTransformer = new  Observable.Transformer() {
    @Override public Object call(Object observable) {
        return ((Observable)  observable).subscribeOn(Schedulers.newThread())
                .observeOn(AndroidSchedulers.mainThread());
    }
};
```

**使用**

```
Observable
          .just( "123" )
          .compose( schedulersTransformer )
          .subscribe(new Action1() {
              @Override
              public void call(Object o) {
              }
          }) ;

```

**弊端**：虽然保持了链式编程结构的完整，但是**每次调用 .compose(schedulersTransformer ) 都是 new了一个对象**的。所以我们需要再次封装，尽量保证单例的模式。

#### 最优改进后的封装

```
public class RxUtil { 
    private final static Observable.Transformer schedulersTransformer 
    = new  Observable.Transformer() {
        @Override public Object call(Object observable) {
            return ((Observable)  observable).subscribeOn(Schedulers.newThread())
                    .observeOn(AndroidSchedulers.mainThread());
        }
    };
 
   public static  <T> Observable.Transformer<T, T> applySchedulers() {
        return (Observable.Transformer<T, T>) schedulersTransformer;
    }
 
}

```

**使用**

```
Observable
	.just( "123" )
	.compose( RxUtil.<String>applySchedulers() )
	.subscribe(new Action1() {
		@Override
		public void call(Object o) {
		}
	}) ;

```

## 相关推荐

另外推荐几篇比较好的文章，有助于理解Rxjava  
[安卓客户端是如何使用 RxJava 的](http://www.apkbus.com/blog-705730-60600.html)  
[通过 RxJava 实现一个 Event Bus –RxBus](http://www.apkbus.com/blog-705730-60631.html)  
[玩透RxJava（一）基础知识](http://www.apkbus.com/blog-705730-60437.html)  
[RxJava 教程第二部分：事件流基础之过滤数据](http://www.apkbus.com/blog-705730-60617.html)  
[Meizhi Android之RxJava &Retrofit最佳实践](http://www.apkbus.com/blog-705730-60599.html)

**相关代码**： <http://git.oschina.net/zyj1609/RxAndroid_RxJava>  



引用：
[给 Android 开发者的 RxJava 详解](http://gank.io/post/560e15be2dca930e00da1083)
[RxJava 和 RxAndroid 二（操作符的使用）](http://www.cnblogs.com/zhaoyanjun/p/5502804.html)
[RxJava 和 RxAndroid 三（生命周期控制和内存优化）](http://www.cnblogs.com/zhaoyanjun/p/5523454.html)
[RxJava 和 RxAndroid 四（RxBinding的使用）](http://www.cnblogs.com/zhaoyanjun/p/5535651.html)
[RxJava 和 RxAndroid五（线程调度）](http://www.cnblogs.com/zhaoyanjun/p/5624395.html)


】














