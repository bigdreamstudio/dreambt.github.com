---
layout: post
category : Linux
tags : [Ubuntu, tutorial, Linux, JDK, configure]
title: Ubuntu环境下配置JDK
wordpress_id: 878
wordpress_url: http://www.im47.net/?p=878
date: 2011-03-21 16:30:54.000000000 +08:00
---
首先，从<a href="http://www.oracle.com/technetwork/java/javaee/downloads/index.html">http://www.oracle.com/technetwork/java/javaee/downloads/index.html</a>中下载jdk,我的版本是jdk1.6.0_24，我将下载的jdk1.6.0_24.bin文件置于/opt中.然后，在shell中执行：
<pre>sudo chmod u+x /opt/jdk1.6.0_24.bin</pre>
修改bin文件权限，使其可执行.然后，执行
<pre>sudo /opt/jdk1.6.0_24.bin</pre>
将会出现字幕，持续按回车键，将会把jdk解压到文件夹，得到jdk1.6.0_24目录。
此时，jdk已安装完毕，下面进行配置，执行gedit /etc/environmen
<pre>JAVA_HOME="/opt/jdk1.6.0_24"
CLASSPATH=".:$JAVA_HOME/lib"
PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:$JAVA_HOME/bin"</pre>
执行
<pre>sudo source /home/siqi/.bashrc</pre>
此时，环境变量设置成功（设置环境变量的方法很多，不一一列举）
由于ubuntu中可能会有默认的jdk，如openjdk，所以，为了使默认使用的是我们安装的jdk，还要进行如下工作。
执行
<pre>update-alternatives --install /usr/bin/java java /opt/jdk1.6.0_24/bin/java 300
update-alternatives --install /usr/bin/javac javac /opt/jdk1.6.0_24/bin/javac 300</pre>
通过这一步将我们安装的jdk加入java选单。
然后执行
<pre>update-alternatives --config java</pre>
通过这一步选择系统默认的jdk
这样，再在shell中输入
<pre>java -version</pre>
时，就会显示系统使用的java是sun的java。
