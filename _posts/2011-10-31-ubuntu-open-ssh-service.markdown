---
layout: post
category : Linux
tags : [Ubuntu, SSH, Linux, tutorial]
title: ubuntu开启SSH服务
wordpress_id: 1046
wordpress_url: http://www.im47.cn/?p=1046
date: 2011-10-31 10:25:39.000000000 +08:00
---
<strong>SSH简介</strong>

SSH 为 Secure Shell 的缩写，由 IETF 的网络工作小组（Network Working Group）所制定；SSH 为建立在应用层和传输层基础上的安全协议。SSH 是目前较可靠，专为远程登录会话和其他网络服务提供安全性的协议。利用 SSH 协议可以有效防止远程管理过程中的信息泄露问题。

<strong>SSH安装</strong>

SSH分客户端openssh-client和openssh-server
<ul>
	<li><span class="Apple-style-span" style="line-height: 18px;">如果你只是想登陆别的机器的SSH只需要安装openssh-client（ubuntu默认安装）</span></li>
	<li><span class="Apple-style-span" style="line-height: 18px;">如果要使本机开放SSH服务就需要安装openssh-server</span></li>
</ul>
<pre>sudo apt-get install openssh-client openssh-server</pre>
然后确认sshserver是否启动了：
<pre>ps -e |grep ssh</pre>
如果看到sshd那说明ssh-server已经启动了。
如果没有则可以这样启动：
<pre>sudo /etc/init.d/ssh start</pre>
ssh-server配置文件位于/etc/ssh/sshd_config，在这里可以定义SSH的服务端口，默认端口是22，你可以自己定义成其他端口号。
然后重启SSH服务：
<pre>sudo /etc/init.d/ssh stop
sudo /etc/init.d/ssh start</pre>
然后使用以下方式登陆SSH：
<pre>ssh username@192.168.1.112</pre>
username为192.168.1.112 机器上的用户，需要输入密码。
断开连接：exit
