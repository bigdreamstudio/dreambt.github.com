---
layout: post
category : Linux
tags : [DNS, Linux, how-to]
title: 开启本地DNS缓存来加速网页浏览
wordpress_id: 951
wordpress_url: http://www.im47.net/?p=951
date: 2011-05-12 15:57:08.000000000 +08:00
---
如果你觉得DNS解析慢，或者上网慢的话，可以尝试一下：

﻿安装dnsmasq：
<blockquote>sudo apt-get install dnsmasq</blockquote>
修改配置文件：
<blockquote>sudo gedit /etc/dhcp/dhclient.conf</blockquote>
找到下面的内容并将前面的#去掉
<blockquote>#prepend domain-name-servers 127.0.0.1</blockquote>
保存退出。
接着就是添加本地DNS：
<blockquote>sudo gedit /etc/resolv.conf</blockquote>
在最下面加上
<blockquote>nameserver 127.0.0.1</blockquote>
当然你还可以加别的DNS服务，比如谷歌的8.8.8.8等，保存退出。
最后再重启一下服务就可以了：
<blockquote>sudo /etc/init.d/dnsmasq restart</blockquote>
