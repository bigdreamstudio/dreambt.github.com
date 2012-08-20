---
layout: post
category : J2EE
tags : [encode, J2EE]
title: J2EE系统各个层次的编码方式
wordpress_id: 934
wordpress_url: http://www.im47.net/?p=934
date: 2011-05-02 11:26:45.000000000 +08:00
---
Web容器默认的编码方式：ISO-8859-1 (解析POST数据)

JDBC驱动程序默认的编码方式：ISO-8859-1 所以我们将其设置为GBK或GB2312

Java内部使用的字符集：Unicode

操作系统：GBK

浏览器发送请求(传输URI)：UTF-8

javascript：UTF-8（沿用java的字符处理方式，内部是使用unicode来处理所有字符的）

当从Unicode编码向某个字符集转换时，如果在该字符集中没有对应的编码，则得到0x3f(即？)

从其他字符集(比如GBK)向Unicode编码转换时，如果这个二进制数在该字符集(GBK)中没有标识任何的字符，则得到的结果是0xfffd

&nbsp;

使用Java从控制台读取中文并向控制台输出的过程：

GBK编码 Unicode编码 GBK编码

内存中使用的是Unicode编码
