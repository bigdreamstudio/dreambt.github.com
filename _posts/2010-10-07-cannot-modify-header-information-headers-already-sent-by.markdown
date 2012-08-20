---
layout: post
category : PHP
tags : [PHP, question, error]
title: Cannot modify header information - headers already sent by错误解决办法
wordpress_id: 553
wordpress_url: http://www.im47.cn/?p=553
date: 2010-10-07 18:10:58.000000000 +08:00
---
最近在学习PHP，在页面跳转时遇到了一些小问题。出错信息如下所示：

<strong>Warning</strong>: Cannot modify header information - headers already sent by (output started at E:\...\htdocs\...\*.php:1) in?<strong>E:\...\htdocs\...\*d.php</strong> on line?<strong>3</strong>

<strong></strong><strong>解决方法一：</strong>
在PHP里Cookie的使用是有一些限制的。

1、使用setcookie必须在&lt;html&gt;标签之前
2、使用setcookie之前，不可以使用echo输入内容
3、直到网页被加载完后，cookie才会出现
4、setcookie必须放到任何资料输出浏览器前，才送出
.....
由于上面的限制，在使用setcookie()函数时，学会遇到 "Undefined index"、"Cannot modify header information - headers already sent by"…等问题，解决办法是在输出内容之前，产生cookie，可以在程序的最上方加入函数 ob_start();

ob_start ：打开输出缓冲区
函数格式：void ob_start(void)说明：当缓冲区激活时，所有来自PHP程序的非文件头信息均不会发送，而是保存在内部缓冲区。为了输出缓冲区的内容，可以使用ob_end_flush()或flush()输出缓冲区的内容。

<strong>方法二：
</strong>打开 php.ini 然后把 output_buffering 设为 on 。重起appache，OK。看来这才是解决办法。

<strong>特别注意：</strong><strong>
</strong>如果使用utf-8编码，一定要去掉UTF-8中的BOM，这都是因为utf-8编码文件含有的bom原因，而php4,5都是不支持bom的。去掉bom，可以用Notepad++打开转换一下。
