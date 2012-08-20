---
layout: post
category : Java
tags : [Java, JVM]
title: JVM监控工具介绍jstack, jconsole, jinfo, jmap, jdb, jsta
wordpress_id: 1059
wordpress_url: http://www.im47.cn/?p=1059
date: 2011-11-20 20:44:11.000000000 +08:00
---
<h4>jstatd</h4>
启动jvm监控服务。它是一个基于rmi的应用，向远程机器提供本机jvm应用程序的信息。默认端口1099。
实例：jstatd -J-Djava.security.policy=my.policy

my.policy文件需要自己建立，内如如下：
grant codebase "file:$JAVA_HOME/lib/tools.jar" {
permission java.security.AllPermission;
};
这是安全策略文件，因为jdk对jvm做了jaas的安全检测，所以我们必须设置一些策略，使得jstatd被允许作网络操作
<h4>jps</h4>
列出所有的jvm实例
实例：
jps
列出本机所有的jvm实例

jps 192.168.0.77
列出远程服务器192.168.0.77机器所有的jvm实例，采用rmi协议，默认连接端口为1099
（前提是远程服务器提供jstatd服务）

输出内容如下：
jones@jones:~/data/ebook/java/j2se/jdk_gc$ jps
6286 Jps
6174  Jstat
<h4>jconsole</h4>
<strong></strong>一个图形化界面，可以观察到java进程的gc，class，内存等信息。
<h4>远程连接jconsole的方法</h4>
服务器端：

1. mkdir $JAVA_HOME/jconsole

2. cp $JAVA_HOME/jre/lib/management/jmxremote.password $JAVA_HOME/jconsole/

chmod 600 $JAVA_HOME/jconsole/jmxremote.password

3. vi jmxremote.password

去掉#monitorRole RED前的注释并将RED修改为你要设置的密码。(安全起见，只开放有只读权限的用户)

4.如果你是Tomcat

在catalina.bat来设置set JAVA_OPTS=%JAVA_OPTS%
<div>-Dcom.sun.management.jmxremote.authenticate=false</div>
<div>-Dcom.sun.management.jmxremote.ssl=false</div>
<div>-Dcom.sun.management.jmxremote.port=7080</div>
<div>-Dcom.sun.management.jmxremote</div>
<div>4. 如果你是RESIN</div>
修改 $RESIN_HOME/bin/wrapper.pl，为$JAVA_ARGS添加三个参数：
-Dcom.sun.management.jmxremote.port=7080
-Dcom.sun.management.jmxremote.password.file=/usr/local/jdk1.6.0/jconsole/jmxremote.password
-Dcom.sun.management.jmxremote.ssl=false

5. 执行hostname -i ，如果显示的是127.0.0.1，则需要修改/etc/hosts文件

vi /etc/hosts，修改如下：
#127.0.0.1              localhost localhost.localdomain localhost
服务器的真实IP地址        localhost localhost.localdomain localhost

7. 启动JSP容器

netstat -na|grep 7080 查看7080端口是否已在监听

客户端:
1. 打开cmd窗口，输入jconsole
2. 指定连接参数:
远程主机: 服务器的真实IP地址
端口: 1010 ($JAVA_ARGS中-Dcom.sun.management.jmxremote.port指定的端口)
用户名: monitorRole (jmxremote.password中指定的用户名)
密码: your_password(jmxremote.password中设置的密码)
3. 连接 -&gt; OK。
<h4>jinfo</h4>
观察运行中的java程序的运行环境参数：参数包括Java System属性和JVM命令行参数
实例：jinfo 2083
其中2083就是java进程id号，可以用jps得到这个id号。
输出内容太多了，不在这里一一列举，大家可以自己尝试这个命令。
<h4>jstack</h4>
可以观察到jvm中当前所有线程的运行情况和线程当前状态
jstack 2083
输出内容如下：

转自：<a href="http://dolphin-ygj.iteye.com/blog/366216">http://dolphin-ygj.iteye.com/blog/366216</a>
