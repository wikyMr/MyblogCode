---
title: java基础知识篇之异常
tags: [java]
---

**异常**
常见异常：NUllPointException，ArrayIndexOutofBoundsExption，IOexption,ClassCastExption,InterrutpedEXption，SQLExption等等；

分类：
RuntimeExption(不受检查异常)：在代码里面可以忽略RuntimeExption及其子类的类型的异常；
如NUllPointException，ArrayIndexOutofBoundsExption；

1.catch语句基类的异常可以匹配其子类的异常，且只匹配一次；




**泛型**
1.泛型：不支持基本类型，但是支持基本类型的装箱类型；
2.确保不会危及程序的类型安全，考虑类型装换，比如栈的泛型实现；
3.不可以new一个泛型的对象或者泛型的数组，因为泛型通过类型擦除来实现，运行时不知道具体的类型，编译时会检查类型；
4.参数化类型是不可变的，例如List<String>不是List<Object>的子类型，所以Java引入通配符来解决这个问题；
5.PECS:
参数类型是生产者，选择？ extends T;
参数类型是消费者，则选择？ sper T；
6.List<Object>与List<?>并不等同，List<Object>实际确定包含的是Object及其子类，使用时可以通过Object来进行引用：


**IO流**：
InputStreamReader将一个InputStream转换成Reader，转换的过程就可以指定编码；
OutputStreamWriter将一个OutputStream转换成Writer。