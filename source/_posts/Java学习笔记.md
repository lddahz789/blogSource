---
title: Java学习笔记
date: 2016-08-22 22:53:53
tags: 杂谈
categories: Java
copyright: true
---

1. 异常捕获时,把需要关闭流的语句写在try()的括号里,之后会自动关闭流,不用再写多余的代码.
2. 反射:getField 只能获取public的，包括从父类继承来的字段。getDeclaredField 可以获取本类所有的字段，包括private的，但是不能获取继承来的字段. (注： 这里只能获取到private的字段，但并不能访问该private字段的值)
3. JSP SERVLET中 getParameter 取的是表单发过来的值,getAttribute取的是自己setAttribute的值
4. JSP forward是服务器跳转, redirect是客户端跳转
<!-- more -->
5. MyBatis中#与$的区别: #{para}会产生PreparedStatement的占位符 ${something}则直接将花括号里的something插入插入字符串
6. 接口中方法默认为 public abstract, 变量默认为 public static final
7. 
