---
title: Java知识点归纳(one)
date: 2017-06-20 00:00:08
tags: 杂谈
categories: Java
copyright: true
---
## IO流, 适配器, 装饰器
Java有以下四种抽象流类型

* InputStream, OutputStream: 能自动解码并向我们提供字符读写的接口.这个类打通了字节处理与字符处理之间的堑沟.这个类就叫做适配器类能自动解码并向我们提供字符读写的接口.这个类打通了字节处理与字符处理之间的堑沟.这个类就叫做适配器类.
### 适配模式
![mark](http://os3e5ayd1.bkt.clouddn.com/blog/170703/2Ac228C7Ji.png?imageslim)

* Reader, Writer: 现在已经有了字节码处理的 InputStream，我们的目标接口是可以处理字符的Reader, 所以我们就需要一个可以把字节码转成字符的 InputStreamReader.这就是适配器.
### 装饰模式
![mark](http://os3e5ayd1.bkt.clouddn.com/blog/170703/8AeC8gcbBG.png?imageslim)

### Java中的IO流图解
![mark](http://os3e5ayd1.bkt.clouddn.com/blog/170703/LJlA0D56k1.png?imageslim)

## 异常
![mark](http://os3e5ayd1.bkt.clouddn.com/blog/170703/FD85D8918h.png?imageslim)
<!-- more -->
* 可查异常（checked exceptions）
>　除了RuntimeException及其子类以外，其他的Exception类及其子类都属于可查异常.这种异常的特点是Java编译器会检查它，也就是说，当程序中可能出现这类异常，要么用try-catch语句捕获它，要么用throws子句声明抛出它，否则编译不会通过．
* 不可查的异常（unchecked exceptions）
> 包括运行时异常（RuntimeException与其子类）和错误（Error).
* RuntimeException
> NullPointerException(空指针异常)、IndexOutOfBoundsException(下标越界异常)等这些异常是不检查异常，程序中可以选择捕获处理，也可以不处理.这些异常一般是由程序逻辑错误引起的，程序应该从逻辑角度尽可能避免这类异常的发生.运行时异常的特点是Java编译器不会检查它，也就是说，当程序中可能出现这类异常，即使没有用try-catch语句捕获它，也没有用throws子句声明抛出它，也会编译通过.
* RuntimeException以外的Exception
> 从程序语法角度讲是必须进行处理的异常，如果不处理，程序就不能编译通过.如IOException、SQLException等以及用户自定义的Exception异常，一般情况下不自定义检查异常.


## Java的安全性
1. 严格遵循面向对象的规范.这样封装了数据细节，只提供接口给用户.增加了数据级的安全性.
2. 无指针运算.java中的操作，除了基本类型都是引用的操作.引用是不能进行增减运算，不能被直接赋予内存地址的，从而增加了内存级的安全性.
3. 数组边界检查.这样就不会出现C/C++中的缓存溢出等安全漏洞.
4. 强制类型转换.非同类型的对象之间不能进行转换，否则会抛出ClassCastException
5. 语言对线程安全的支持.java从语言级支持线程.从而从语法和语言本身做了很多对线程的控制和支持.
6. GC
7. Exception

## 线程安全
线程安全是一个很大的问题Java最常见的`HttpServlet`就是单实例多线程，解决这样的问题，有多种方式：

* ThreadLocal
> ThreadLocal 就是将变量存到线程自己的工作内存中，所以不会有并发问题.
* Synchronized
> synchronized锁住的是括号里的对象，而不是代码.对于非 static 的 synchronized 方法，锁的就是对象本身也就是 this.该关键字可以加到：实例方法/静态方法/实例方法中的同步块/静态方法中的同步块
* ReentrantLock / Condition
> synchronized 不够灵活，例如读写文件，读和读之间不应该互斥，这个时候就可以使用 ReentrantLock 增加并发能力.Condition 是绑定到 Lock 上的，可以用于线程间通信

* 并发容器
> 常见的 ConcurrentHashMap CopyOnWriteArrayList 用于多线程下存放数据，Queue BlockingQueue 用于排队消费.

* Atomic包 
> 在 Atomic 包里一共有 12 个类，四种原子更新方式，分别是原子更新基本类型，原子更新数组，原子更新引用和原子更新字段.某些并发问题，需要无锁解决时，就可以考虑使用原子方法.