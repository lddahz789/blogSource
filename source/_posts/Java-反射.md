---
title: Java 反射
date: 2017-06-02 01:21:28
tags: 反射
categories: Java
copyright: true
---
# 1. Class类

任何一个类都是Class类的实例对象，这个实例对象有 三种表示方式:

* Class c1 = Student.class;//实际告诉我们任何一个类都有一个隐含的静态成员变量class（知道类名时用）

* Class c2 = stu.getClass();//已知该类的对象通过getClass方法（知道对象时用）

* Class c3 = Class.forName("类的全名");//会有一个ClassNotFoundException异常

既然是Class类的实例,那么为什么不用new呢?因为 java.lang.Class类的构造方法是私有的.

通过类类型来建立该类的实例:
Student stu = (Student)c1.newInstance();//前提是必须要有无参的构造方法，因为该语句会去调用其无参构造方法。该语句会抛出异常。

# 2. 动态加载类

## 1、 编译时加载类是静态加载类

new 创建对象是静态加载类，在编译时刻就需要加载所有可用使用到的类，如果有一个用不了，那么整个文件都无法通过编译
<!-- more -->
## 2、 运行时加载类是动态加载类

Class c = Class.forName("类的全名")，不仅表示了类的类型，还表示了动态加载类，编译不会报错，在运行时才会加载，使用接口标准能更方便动态加载类的实现。功能性的类尽量使用动态加载，而不用静态加载。

许多程序的在线升级，并不需要重新编译文件了，只是动态的加载新的东西

# 3. 获取方法信息

## 1、基本的数据类型，void关键字都存在类类型

```
1 Class c1 =int.class;//int的类类型
2 Class c2 =String.class;//String类的类类型，可以理解为编译生成的那个String.class字节码文件，
3 //当然，这并不是官方的说法
4 Class c3 =double.class;
5 Class c4 =Double.class;
6 Class c5 =void.class;
```
## 2、Class类的基本API操作

```
1 /**
 2     * 打印类的信息，包括类的成员函数，成员变量
 3 * @param obj 该对象所属类的信息信息
 4 */
 5 publicstaticvoid printClassMessage(Object obj){
 6 //要获取类的信息，首先要获取类的类类型
 7 Class c = obj.getClass();//传递的是哪个子类的对象，c就是该子类的类类型
 8 //获取类的名称
 9 System.out.println("类的名称是："+c.getName());
10 
11 /*
12     * Method类，方法的对象
13 * 一个成员方法就是一个Method对象
14 * getMethods()方法获取的是所有的public的函数，包括父类继承而来的
15 * getDeclaredMethods()获取的是多有该类自己声明的方法，不问访问权限
16 */
17 Method[] ms = c.getMethods();//c.getDeclaredMethods();
18 for(int i =0; i < ms.length; i++){
19 //得到方法的返回值类型的类类型
20 Class retrunType = ms[i].getReturnType();
21 System.out.print(retrunType.getName()+" ");
22 //得到方法的名称
23 System.out.print(ms[i].getName()+"(");
24 //获取的参数类型--->得到的是参数列表的类型的类类型
25 Class[] paraTypes = ms[i].getParameterTypes();
26 for(Class class1 : paraTypes){
27 System.out.print(class1.getName()+",");
28 }
29 System.out.println(")");
30 }
31 }  
```
在执行任何操作之前,想要获得类的各种信息,要先获得类类型

# 4. 获取成员变量构造函数

```
 1 /**
 2     * 成员变量也是对象，是java.lang.reflect.Field这个类的的对象
 3 * Field类封装了关于成员变量的操作
 4 * getFields()方法获取的是所有public的成员变量的信息
 5 * getDeclareFields()方法获取的是该类自己声明的成员变量的信息
 6 */
 7 Field[] fs = c.getDeclaredFields();
 8 for(Field field : fs){
 9 //得到成员变量的类型的类类型
10 Class fieldType = field.getType();
11 String typeName = fieldType.getName();
12 //得到成员变量的名称
13 String fieldName = field.getName();
14 System.out.print(typeName+" "+fieldName);
15 }
16 
17 
18 /**
19     * 构造函数也是对象
20 * java.lang.Constructor中封装了构造函数的信息
21 * getConstructor()方法获取所有的public的构造函数
22 * getDeclaredConstructors得到所有的构造函数
23 */
24 Constructor[] cs = c.getDeclaredConstructors();
25 for(Constructor constructor : cs){
26 System.out.print(constructor.getName()+"(");
27 //获取构造函数的参数列表---》得到的是参数雷彪的类类型
28 Class[] paramTypes = constructor.getParameterTypes();
29 for(Class class1 : paramTypes){
30 System.out.print(class1.getName()+",");
31 }
32 System.out.println(")");
33 }
```


# 5. 基本操作

* 获得方法
方法的名称和方法的参数列表才能唯一决定某个方法

Method m = c.getDeclaredMethod("方法名"，可变参数列表（参数类型.class）)

* 方法的反射操作
m.invoke(对象,参数列表);
方法如果没有返回值，返回null，如果有返回值返回Object类型，然后再强制类型转换为原函数的返回值类型

# 6. 通过反射来了解集合泛型本质

```
1 ArrayList list1 =new ArrayList();
2 ArrayList<String> list2 =new ArrayList<String>();
3 
4 Class c1 = list1.getClass();
5 Class c2 = list2.getClass();
6 
7 System.out.println(c1==c2);//结果为true
```

因为反射的操作都是编译之后的操作，也就是运行时的操作，c1==c2返回true，说明编译之后集合的泛型是去泛型化的。