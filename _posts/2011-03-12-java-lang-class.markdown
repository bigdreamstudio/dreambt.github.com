---
layout: post
category : Java
tags : [intro, Java, java.lang.Class]
title: 深入研究java.lang.Class类
wordpress_id: 871
wordpress_url: http://www.im47.net/?p=871
date: 2011-03-12 21:44:24.000000000 +08:00
---
<span style="color: #228b22; font-size: small;"><strong><span style="color: #ff0000;">前言：</span></strong>Java的类库日益庞大，所包含的类和接口也不计其数。但其中有一些非常重要的类和接口，是Java类库中的核心部分。常见的有String、Object、Class、Collection、ClassLoader...，熟悉这些类是学好Java的基础。而这些类一般不容易理解，需要做深入的研究和实践才能掌握。下面是我对这些类理解和使用的一些总结。欢迎你在阅读后将你宝贵的意见和读后感留下！</span>

<span style="font-size: small; color: #000000;"> Java程序在运行时，Java运行时系统一直对所有的对象进行所谓的运行时类型标识。这项信息纪录了每个对象所属的类。虚拟机通常使用运行时类型信息选准正确方法去执行，用来保存这些类型信息的类是Class类。Class类封装一个对象和接口运行时的状态，当装载类时，Class类型的对象自动创建。</span>

<span style="font-size: small; color: #000000;"> Class 没有公共构造方法。Class 对象是在加载类时由 Java 虚拟机以及通过调用类加载器中的 defineClass 方法自动构造的，因此不能显式地声明一个Class对象。 </span>

<span style="font-size: small; color: #000000;"> 虚拟机为每种类型管理一个独一无二的Class对象。也就是说，每个类（型）都有一个Class对象。运行程序时，Java虚拟机(JVM)首先检查是否所要加载的类对应的Class对象是否已经加载。如果没有加载，JVM就会根据类名查找.class文件，并将其Class对象载入。</span>

<span style="font-size: small; color: #000000;"> 基本的 Java 类型（boolean、byte、char、short、int、long、float 和 double）和关键字 void 也都对应一个 Class 对象。 </span>

<span style="font-size: small; color: #000000;"> 每个数组属于被映射为 Class 对象的一个类，所有具有相同元素类型和维数的数组都共享该 Class 对象。</span>

<span style="font-size: small; color: #000000;"> 一般某个类的Class对象被载入内存，它就用来创建这个类的所有对象。</span>

&nbsp;
<div>

<strong>一、如何得到Class的对象呢？有三种方法可以的获取</strong>

1、调用Object类的getClass()方法来得到Class对象，这也是最常见的产生Class对象的方法。例如：
MyObject x;
Class c1 = x.getClass();

2、使用Class类的中静态forName()方法获得与字符串对应的Class对象。例如：
Class c2=Class.forName("MyObject"),Employee必须是接口或者类的名字。

3、获取Class类型对象的第三个方法非常简单。如果T是一个Java类型，那么T.class就代表了匹配的类对象。例如
Class cl1 = Manager.class;
Class cl2 = int.class;
Class cl3 = Double[].class;
注意：Class对象实际上描述的只是类型，而这类型未必是类或者接口。例如上面的int.class是一个Class类型的对象。由于历史原因，数组类型的getName方法会返回奇怪的名字。

<strong>二、Class类的常用方法</strong>

1、getName()
一个Class对象描述了一个特定类的属性，Class类中最常用的方法getName以 String 的形式返回此 Class 对象所表示的实体（类、接口、数组类、基本类型或 void）名称。

2、newInstance()
Class还有一个有用的方法可以为类创建一个实例，这个方法叫做newInstance()。例如：
x.getClass.newInstance()，创建了一个同x一样类型的新实例。newInstance()方法调用默认构造器（无参数构造器）初始化新建对象。

3、getClassLoader()
返回该类的类加载器。

4、getComponentType()
返回表示数组组件类型的 Class。

5、getSuperclass()
返回表示此 Class 所表示的实体（类、接口、基本类型或 void）的超类的 Class。

6、isArray()
判定此 Class 对象是否表示一个数组类。

<strong>三、Class的一些使用技巧</strong>

1、forName和newInstance结合起来使用，可以根据存储在字符串中的类名创建对象。例如
Object obj = Class.forName(s).newInstance();

2、虚拟机为每种类型管理一个独一无二的Class对象。因此可以使用==操作符来比较类对象。例如：
if(e.getClass() == Employee.class)

转自：<a href="http://dbajun.javaeye.com/blog/121606">http://dbajun.javaeye.com/blog/121606</a>

</div>
&nbsp;
