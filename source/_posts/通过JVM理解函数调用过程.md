---
title: 通过JVM理解函数调用过程
date: 2017-06-25 23:50:57
tags: JVM
categories: Java
copyright:
---
## 一个函数的调用过程

代码:
```
public class Main {
	public static void main(String args[]) {
		int a = 1;
		int b = 2;
		int t = a + b;
	}
}
```
首先我们先看字节码:

```
  public static void main(java.lang.String[]);
    Code:
       0: iconst_1
       1: istore_1
       2: iconst_2
       3: istore_2
       4: iload_1
       5: iload_2
       6: iadd
       7: istore_3
       8: return
```
虚拟机会开辟一段内存做为栈帧.栈帧上有局部变量表,还有操作数栈.暗蓝色代表局部变量表:
![8a3cc212929b4b3aa6c9c2270e152926-image.png](//os3e5ayd1.bkt.clouddn.com//file/2017/6/8a3cc212929b4b3aa6c9c2270e152926-image.png) 
<!-- more -->
0: iconst1: 将常量1放入操作数栈顶
 ![6424c43ae6a844d4b6e35529661c81e6-image.png](//os3e5ayd1.bkt.clouddn.com//file/2017/6/6424c43ae6a844d4b6e35529661c81e6-image.png) 

1: istore1: 取出栈顶值(出栈操作),放入局部变量表第一个位置,这时frame栈帧 变成了这样:
 ![ec8759eb4b27445fbacbdd5e518cd3a5-image.png](//os3e5ayd1.bkt.clouddn.com//file/2017/6/ec8759eb4b27445fbacbdd5e518cd3a5-image.png) 


2,3步同理
4,5 iload; 取出对应局部变量表的值,并存入操作栈顶 (取a值1送入操作栈顶,然后取b值2送入操作栈顶),现在变为这样
![160634e2c2db47b1beaae961d35080d7-image.png](//os3e5ayd1.bkt.clouddn.com//file/2017/6/160634e2c2db47b1beaae961d35080d7-image.png) 

6 iadd: 对操作栈顶2个数据做出栈操作,求和后送入栈顶,结果为操作栈只有一个数,3
![36109212479444fb8e44bc0fcf8d25ea-image.png](//os3e5ayd1.bkt.clouddn.com//file/2017/6/36109212479444fb8e44bc0fcf8d25ea-image.png) 
7. istore3: 同理,取出栈顶值,放入局部变量表第三个位置

## 栈和堆

堆(heap)是一块独立的内存空间.在创建对象的时候, 我们可以把真正的对象放到堆里,只在栈里记录这个对象的地址就可以了
现在有如下代码:
```
public class Main {
	public static void main(String args[]) {
		A a = new A(1);
		A b = new A(2);
		swap(a, b);
		System.out.println("a's value is " + a.value +", b's value is " + b.value);
	}

	public static void swap(A a, A b) {
		int t = a.value;
		a.value = b.value;
		b.value = t;
	}
}

class A {
	public int value;
	public A(int v) {
		this.value = v;
	}
}
```
![111c06f672c84a0f8b10b12ae7e82b31-image.png](//os3e5ayd1.bkt.clouddn.com//file/2017/6/111c06f672c84a0f8b10b12ae7e82b31-image.png) 
传入swap的都是副本,但是都是指向同一个地址,所以swap是可以正常修改对象值的,相反的,如果传入的参数是primitive type,那么在swap里修改的就是副本的值了,对原值没有任何影响


