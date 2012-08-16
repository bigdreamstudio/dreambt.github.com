---
layout: post
category : Linux
tags : [intro, beginner, Linux, tutorial, Redhat, ADSL]
title: Redhat下使用ADSL拨号上网
excerpt: ! "RedHat Linux是目前世界上使用最多的Linux操作系统。因为它具备最好的图形界面，无论是安装、配置还是使用都十分方便，而且运行稳定，因此不论是新手还是老玩家都对它有很高的评价。\r\n然而，初次接触RedHat却发现它的ADSL设置并没有像Fedora、Ubuntu那么简单。这篇文章就详细讲解了在Redhat下如何使用ADSL拨号上网。"
wordpress_id: 176
wordpress_url: http://www.im47cn.nt102.idcwind.net/wordpress/?p=176
date: 2009-10-02 21:18:18.000000000 +08:00
---
本文以RedHat 5.4为实验平台，其它平台的Linux安装办法可以参照下面从源代码进行安装的步骤。

一、安装的前提条件

1.确保安装了网卡并工作正常

使用命令

#ifconfig eth0

查看网卡状态

2.在系统中不要设置默认路由(网关),让ADSL拨号后自动获得

如果已经设置了默认路由,使用以下方法删除:
在文件 /etc/sysconfig/network 中删除 GATEWAY= 这一行,然后以root执行:

#/etc/rc.d/init.d/network restart

3.已经安装了pppd软件包

如果存在文件 /usr/sbin/pppd,则说明已经安装了pppd;
如果未安装,从RedHatLinux 6.2安装光盘上安装ppp-2.3.11-4.i386.rpm这个软件包

二、安装PPPOE客户端软件

Linux下的PPPOE客户端软件比较多,而且大多使用GNU License,我们推荐使用rp-pppoe 这个软件包。从http://www.roaringpenguin.com/pppoe/这个网站上，不仅可以下载  RedHat 62平台下的rp-pppoe的二进制软件包，而且可以下栽源代码软件包。

1.二进制软件包的安装：

A.下载二进制软件包

http://www.roaringpenguin.com/pppoe/rp-pppoe-3.2-1.i386.rpm

B.进行安装

#rpm -Uvh rp-pppoe-3.2-1.i386.rpm

2.从源代码进行安装：

从源代码进行安装同样适用于其它平台的Linux,但必须在Linux系统中安装gcc编译器。

A.下栽源代码软件包

http://www.roaringpenguin.com/pppoe/rp-pppoe-3.2.tar.gz

B.解压缩

#tar xvfz rp-pppoe-3.2.tar.gz
#cd rp-pppoe-3.2

C.进行编译和安装

运行脚本
#./go
将自动进行编译和安装，最后，调用/usr/sbin/adsl-setup进行配置，具体解释见三。

三、配置PPPOE客户端软件

安装完软件包后，必须配置pppoe的配置文件/etc/ppp/pppoe.conf，从而让ADSL拨号时使用配置文件中的用户名、密码等参数。我们不必手工改动这个文件，可以使用
adsl-setup这个工具进行配置：

#/usr/sbin/adsl-setup

当出现
&gt;&gt;&gt; Enter your PPPoE user name :
输入ADSL帐号的用户名

当出现
&gt;&gt;&gt; Enter the Ethernet interface connected to the ADSL modem
For Solaris, this is likely to be something like /dev/hme0.
For Linux, it will be ethn, where 'n' is a number.
(default eth0):
输入 eth0 ,这是ADSL相连的网卡的名字

当出现
&gt;&gt;&gt; Enter the demand value (default no):
输入 no

当出现
&gt;&gt;&gt; Enter the DNS information here:
输入 server ,这表示使用ADSL拨号自动获得的DNS服务器IP地址

当出现
&gt;&gt;&gt; Please enter your PPPoE password:
输入ADSL帐号的密码

当出现
&gt;&gt;&gt; Choose a type of firewall (0-2):
输入 0 ，不使用防火墙

当出现
&gt;&gt;&gt; Accept these settings and adjust configuration files (y/n)?
如果输入的信息正确，输入 y ,完成配置，否则，输入 n 重新输入。

四、启动PPPOE客户端软件

使用命令

/usr/sbin/adsl-start 启动PPPOE客户端软件,进行连接，如果成功，将出现  Connected;
如果不成功，请检查网线、ADSL MODEM等物理设备，并查看 /var/log/messages中的信息
/usr/sbin/adsl-stop 关闭和ISP的连接
/usr/sbin/adsl-status 查看当前连接的状态

如果想在Linux系统启动时自动启动ADSL连接，输入以下命令
#chkconfig --add adsl
将在当前的运行级下加入ADSL的自启动脚本

五、测试

当连接成功后，使用命令

#ifconfig -a

在输出中应该含有关于 ppp0 的一堆信息，其中还绑定了 IP 地址,说明已经从拨号中获得了IP地址。

使用命令

#netstat -nr

查看路由表信息，这时的默认路由应该是上面获得的IP地址。
如果没有默认路由，我们可以手动增加：

#route add default gw 上面获得的IP地址

使用命令

#nslookup www.sina.com.cn

如果解析出新浪的IP，说明已经从拨号中正确获得了DNS服务器

最后，使用命令ping某个域名或IP，如果有响应，表示你已经大功告成了。

六、其它说明

RedHat 5.4 默认安装了 rp-pppoe这个软件包，可以直接进行三后面的步骤。
