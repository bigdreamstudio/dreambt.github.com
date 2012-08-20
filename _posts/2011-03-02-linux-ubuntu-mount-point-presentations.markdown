---
layout: post
category : Linux
tags : [Linux, Ubuntu, mount, server]
title: Linux(Ubuntu)挂载点介绍及桌面服务器分区方案
wordpress_id: 867
wordpress_url: http://www.im47.net/?p=867
date: 2011-03-02 14:11:20.000000000 +08:00
---
本文介绍Linux常用分区挂载点常识以及桌面、服务器分区挂载点的推荐配置，当然这个配置是天缘自己写的，分区大小这个话题是仁者见仁智者见智，欢迎大家一起交流这个话题，比如WEB服务、邮件服务、下载服务等，我们一起交流哪种类型服务下某挂载点应该加大就可以了，至于是否独立就看个人的了。

一、Linux分区挂载点介绍

Linux分区挂载点介绍，推荐容量仅供参考不是绝对，跟各系统用途以及硬盘空间配额等因素实际调整：
<div>
<table border="0" cellspacing="0" cellpadding="0" width="90%">
<tbody>
<tr>
<td width="58">分区类型</td>
<td width="69">介绍</td>
<td width="451">备注</td>
</tr>
<tr>
<td width="58">/boot</td>
<td width="69">启动分区</td>
<td width="451">一般设置100M-200M，boot目录包含了操作系统的内核和在启动系统过程中所要用到的文件。</td>
</tr>
<tr>
<td width="58">/</td>
<td width="69">根分区</td>
<td width="451">所有未指定挂载点的目录都会放到这个挂载点下。</td>
</tr>
<tr>
<td width="58">/home</td>
<td width="69">用户目录</td>
<td width="451">一般每个用户100M左右，特殊用途，比如放大文件也可再加上G。分区大小取决于用户多少。对于多用户使用的电脑，建议把/home独立出来，而且还可以很好地控制普通用户权限等，比如对用户或者用户组实行磁盘配额限制、用户权限访问等。</td>
</tr>
<tr>
<td width="58">/tmp</td>
<td width="69">临时文件</td>
<td width="451">一般设置1-5G，方便加载ISO镜像文件使用，对于多用户系统或者网络服务器来也有独立挂载的必要。临时文件目录，也是最常出现问题的目录之一。</td>
</tr>
<tr>
<td width="58">/usr</td>
<td width="69">文件系统</td>
<td width="451">一般设置要3-15G，大部分的用户安装的软件程序都在这里。就像是Windows目录和Program Files目录。很多Linux家族系统有时还会把/usr/local单独作为挂载点使用。</td>
</tr>
<tr>
<td width="58">/var</td>
<td width="69">可变数据目录</td>
<td width="451">包含系统运行时要改变的数据。通常这些数据所在的目录的大小是要经常变化的，系统日志记录也在/var/log下。一般多用户系统或者网络服务器要建立这个分区，设立这个分区，对系统日志的维护很有帮助。一般设置2-3G大小，也可以把硬盘余下空间全部分为var。</td>
</tr>
<tr>
<td width="58">/srv</td>
<td width="69">系统服务目录</td>
<td width="451">用来存放service服务启动所需的文件资料目录，不常改变。</td>
</tr>
<tr>
<td width="58">/opt</td>
<td width="69">附加应用程序</td>
<td width="451">存放可选的安装文件，个人一般把自己下载的软件资料存在里面，比如Office、QQ等等。</td>
</tr>
<tr>
<td width="58">swap</td>
<td width="69">交换分区</td>
<td width="451">一般为内存2倍，最大指定2G即可</td>
</tr>
<tr>
<td width="58"></td>
<td width="69"></td>
<td width="451">以下为其它常用的分区挂载点</td>
</tr>
<tr>
<td width="58">/bin</td>
<td width="69">二进制可执行目录</td>
<td width="451">存放二进制可执行程序，里面的程序可以直接通过命令行调用，而不需要进入程序所在的文件夹。</td>
</tr>
<tr>
<td width="58">/sbin</td>
<td width="69">系统管理员命令存放目录</td>
<td width="451">存放标准系统管理员文件</td>
</tr>
<tr>
<td width="58">/dev</td>
<td width="69">存放设备文件</td>
<td width="451">驱动文件等</td>
</tr>
<tr>
<td width="58">...</td>
<td width="69"></td>
<td width="451">不再介绍...</td>
</tr>
</tbody>
</table>
</div>
当然上面这么多挂载点，实际上是没有比较每个目录都单独进行挂载，我们只需要根据自己的实际使用需要对个别目录进行挂载，这样系统结构看起来也会精简很多。

一般来讲Linux系统最少的挂载点有两个一个是根挂载点/，另一个是swap，虽然swap也可以采用其他方式类似方式替代，但从使用角度，天缘认为没这个必要，把swap单独设置一个挂载点似乎对Linux系统的标准性更好支持。

二、Linux系统桌面、服务器分区推荐方案

下面以80G独立硬盘安装Ubuntu为例，列一下简单的分区方案。

1、普通桌面用户推荐分区方案（示例：80G桌面用户）：
<div>
<table border="0" cellspacing="0" cellpadding="0" width="300">
<tbody>
<tr>
<td width="96">/boot</td>
<td width="91">200M</td>
<td width="112"></td>
</tr>
<tr>
<td width="96">/</td>
<td width="91">20G</td>
<td width="112"></td>
</tr>
<tr>
<td width="96">/home</td>
<td width="91">50G</td>
<td width="112">余下空间</td>
</tr>
<tr>
<td width="96">swap</td>
<td width="91">2G</td>
<td width="112"></td>
</tr>
</tbody>
</table>
</div>
2、服务器用户推荐分区方案一（示例：80GWEB服务器用户，用户程序与系统程序合用usr）：
<div>
<table border="0" cellspacing="0" cellpadding="0" width="300">
<tbody>
<tr>
<td width="69">/boot</td>
<td width="66">200M</td>
<td width="165"></td>
</tr>
<tr>
<td width="69">/</td>
<td width="66">10G</td>
<td width="165"></td>
</tr>
<tr>
<td width="69">/tmp</td>
<td width="66">2G</td>
<td width="165"></td>
</tr>
<tr>
<td width="69">/var</td>
<td width="66">2G</td>
<td width="165"></td>
</tr>
<tr>
<td width="69">/usr</td>
<td width="66">10G</td>
<td width="165">要安装一些常用软件</td>
</tr>
<tr>
<td width="69"></td>
<td width="66"></td>
<td width="165"></td>
</tr>
<tr>
<td width="69">/home</td>
<td width="66">50G</td>
<td width="165">余下空间</td>
</tr>
<tr>
<td width="69">swap</td>
<td width="66">2G</td>
<td width="165"></td>
</tr>
</tbody>
</table>
</div>
2、服务器用户推荐分区方案二（示例：80GWEB服务器用户，用户程序与系统程序分用opt和usr）：
<div>
<table border="0" cellspacing="0" cellpadding="0" width="300">
<tbody>
<tr>
<td width="66">/boot</td>
<td width="62">200M</td>
<td width="172"></td>
</tr>
<tr>
<td width="66">/</td>
<td width="62">10G</td>
<td width="172"></td>
</tr>
<tr>
<td width="66">/tmp</td>
<td width="62">2G</td>
<td width="172"></td>
</tr>
<tr>
<td width="66">/var</td>
<td width="62">5G</td>
<td width="172"></td>
</tr>
<tr>
<td width="66">/usr</td>
<td width="62">10G</td>
<td width="172">系统安装程序软件使用</td>
</tr>
<tr>
<td width="66">/opt</td>
<td width="62">10G</td>
<td width="172">用户安装程序软件使用</td>
</tr>
<tr>
<td width="66">/home</td>
<td width="62">35G</td>
<td width="172">余下空间</td>
</tr>
<tr>
<td width="66">swap</td>
<td width="62">2G</td>
<td width="172"></td>
</tr>
</tbody>
</table>
</div>
分区方案关键点：

——大数据库一般要加大/usr挂载点

——多用户、下载类、多存储文件等要加大/home挂载点

——文件小，用户多要注意/tmp和/var挂载点大小
