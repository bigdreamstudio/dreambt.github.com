---
layout: post
category : Java
tags : [intro, Java, Double, Long]
title: Java数据类型转化，特别是Double转Long
wordpress_id: 891
wordpress_url: http://www.im47.net/?p=891
date: 2011-04-02 13:13:33.000000000 +08:00
---
简单来说，Java的数据类型转化还是很简单的，基本上分为自动类型转化和强制类型转化。然而今天却越到了戏剧性的一幕。

最近在研究一个小系统的设计，其中就用到了Double类型向Long类型的转化。Long类里面居然没有专门的转化方法，于是从网上搜到这么一句：

[code]Long l1 = Math.round(d1);[/code]

这样貌似没有任何问题，但是Math.round()方法返回的确实int型数据，因此可能导致丢失精度。所以我给出的建议还是采用如下的代码：

[code]Long l1 = (Long)d1;[/code]

&nbsp;
<div>

<strong><span style="font-size: small;">数据类型</span></strong>

基本类型有以下四种：
int长度数据类型有：byte(8bits)、short(16bits)、int(32bits)、long(64bits)、
float长度数据类型有：单精度（32bits float）、双精度（64bits double）
boolean类型变量的取值有：ture、false
char数据类型有：unicode字符(16位)
对应的类类型：Integer、Float、Boolean、Character、Double、Short、Byte、Long

</div>
<div><strong><span style="font-size: small;">转换原则</span></strong>从低精度向高精度转换
byte 、short、int、long、float、double、char
注：两个char型运算时，自动转换为int型；当char与别的类型运算时，也会先自动转换为int型的，再做其它类型的自动转换

基本类型向类类型转换

正向转换：通过类包装器来new出一个新的类类型的变量
Integer a= new Integer(2);
反向转换：通过类包装器来转换
int b=a.intValue();

类类型向字符串转换

正向转换：因为每个类都是object类的子类，而所有的object类都有一个toString()函数，所以通过toString()函数来转换即可
反向转换：通过类包装器new出一个新的类类型的变量
eg1: int i=Integer.valueOf(“123”).intValue()
说明：上例是将一个字符串转化成一个Integer对象，然后再调用这个对象的intValue()方法返回其对应的int数值。
eg2: float f=Float.valueOf(“123”).floatValue()
说明：上例是将一个字符串转化成一个Float对象，然后再调用这个对象的floatValue()方法返回其对应的float数值。
eg3: boolean b=Boolean.valueOf(“123”).booleanValue()
说明：上例是将一个字符串转化成一个Boolean对象，然后再调用这个对象的booleanValue()方法返回其对应的boolean数值。
eg4:double d=Double.valueOf(“123”).doubleValue()
说明：上例是将一个字符串转化成一个Double对象，然后再调用这个对象的doubleValue()方法返回其对应的double数值。
eg5: long l=Long.valueOf(“123”).longValue()
说明：上例是将一个字符串转化成一个Long对象，然后再调用这个对象的longValue()方法返回其对应的long数值。
eg6: char=Character.valueOf(“123”).charValue()
说明：上例是将一个字符串转化成一个Character对象，然后再调用这个对象的charValue()方法返回其对应的char数值。

基本类型向字符串的转换
正向转换：
如：int a=12;
String b;b=a+””;

反向转换：
通过类包装器
eg1:int i=Integer.parseInt(“123”)
说明：此方法只能适用于字符串转化成整型变量
eg2: float f=Float.valueOf(“123”).floatValue()
说明：上例是将一个字符串转化成一个Float对象，然后再调用这个对象的floatValue()方法返回其对应的float数值。
eg3: boolean b=Boolean.valueOf(“123”).booleanValue()
说明：上例是将一个字符串转化成一个Boolean对象，然后再调用这个对象的booleanValue()方法返回其对应的boolean数值。
eg4:double d=Double.valueOf(“123”).doubleValue()
说明：上例是将一个字符串转化成一个Double对象，然后再调用这个对象的doubleValue()方法返回其对应的double数值。
eg5: long l=Long.valueOf(“123”).longValue()
说明：上例是将一个字符串转化成一个Long对象，然后再调用这个对象的longValue()方法返回其对应的long数值。
eg6: char=Character.valueOf(“123”).charValue()
说明：上例是将一个字符串转化成一个Character对象，然后再调用这个对象的charValue()方法返回其对应的char数值。

</div>
<div><strong>JAVA中常用数据类型转换函数 </strong>
虽然都能在JAVA API中找到，整理一下做个备份。</div>
<div>string-&gt;byte
Byte static byte parseByte(String s)</div>
<div>byte-&gt;string
Byte static String toString(byte b)</div>
<div>char-&gt;string
Character static String toString (char c)</div>
<div>string-&gt;Short
Short static Short parseShort(String s)</div>
<div>Short-&gt;String
Short static String toString(Short s)</div>
<div>String-&gt;Integer
Integer static int parseInt(String s)</div>
<div>Integer-&gt;String
Integer static String tostring(int i)</div>
<div>String-&gt;Long
Long static long parseLong(String s)</div>
<div>Long-&gt;String
Long static String toString(Long i)</div>
<div>String-&gt;Float
Float static float parseFloat(String s)</div>
<div>Float-&gt;String
Float static String toString(float f)</div>
<div>String-&gt;Double
Double static double parseDouble(String s)</div>
<div>Double-&gt;String
Double static String toString(Double)</div>
