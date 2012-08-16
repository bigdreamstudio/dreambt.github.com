---
layout: post
category : Linux
tags : [intro, beginner, Linux]
title: Speed Up YUM！让Linux的升级速度快如飞
wordpress_id: 157
wordpress_url: http://www.im47cn.nt102.idcwind.net/wordpress/?p=157
date: 2009-05-16 06:46:01.000000000 +08:00
---
很多人都觉得Fedora的yum很糟糕——速度慢、依赖解决得不好和容易出问题。

其实只要我们稍加动手，yum的问题就能迎刃而解。

yum速度慢？yum speed=yum+fastestmirror+axelget(+presto)

对于许多人来说，默认的yum速度是很慢的。为什么呢？默认的时候，yum是通过连接到官方的服务器列表，并随机从中选取一个服务器使用的。鉴于中 国大陆的公网是没有Fedora的yum服务器(教育网有yum服务器，但是同步比较迟。），因此速度想提高都很难。所以只能另辟路径为yum提速。
安装yum-fastestmirror插件，从服务器列表中选取最快的服务器。这个办法通常都很有效，能够选取到最快的服务器，从而实现提速。先在终端把用户切换到root，然后输入命令：
<blockquote>yum install yum-fastestmirror</blockquote>
稍等片刻即可安装完成，或者在“Add/Remove Software”点击安装皆可。

但是fastestmirror选取的服务器未必是最快的，因为fastestmirror插件是通过测定ping延时最短来计算哪个服务器最快， 实际上这种方法可能会选取到ping延时很低但是速度并不是很高的服务器。所以我们还有另外的一个办法，就是yum-axelget插件。

默认的yum是单线程下载的。yum-axelget插件是调用系统中的axel下载软件，增加下载线程从而提高速度。这个方法更有效，更快捷，而 且会根据软件包的大小自动设定线程数，基本避免了因为线程数过多而导致服务器拒绝下载的问题。点击打开终端，把用户切换到root，然后输入命令：
<blockquote>rpm -ivh <a href="http://rpm4fc-cn.googlecode.com/files/axel-2.3-1.fc10.i386.rpm" target="_blank">http://rpm4fc-cn.googlecode.com/files/axel-2.3-1.fc10.i386.rpm</a> <a href="http://rpm4fc-cn.googlecode.com/files/yum-axelget-1.0-0.2.20080705.fc10.noarch.rpm" target="_blank">http://rpm4fc-cn.googlecode.com/files/yum-axelget-1.0-0.2.20080705.fc10.noarch.rpm</a></blockquote>
<p style="text-align: center;"><img src="/attachments/month_0905/p2009517132319.png" border="0" alt="" width="400" height="265" align="middle" /></p>
稍等片刻即可，因为这不是Fedora官方的插件，所以无法在“Add/Remove Software”安装。

如果是这样的速度还不能令你满足，怎么办？yum-presto插件还可以进一步提速……presto插件会大幅度提升更新安装包的速度。用户只需 要下载每一个软件的增量内容(用drpm打包而成)，在本地计算机重新生成一个完整的软件包再安装。通常增量更新只有很小的下载量，因而即使很大量的内容 要更新，所耗费的时间必然比传统方法要少很多。不过presto系统还在测试之中，而且只有一个服务器提供presto更新，速度也不怎么样。目前 presto只提供Fedora 9、Fedora 10和Fedora Rawhide三个版本的更新。建议有兴趣的朋友可以参考这里：

<a href="https://hosted.fedoraproject.org/presto/" target="_blank">https://hosted.fedoraproject.org/presto/</a>

安装yum-presto插件：
<blockquote>yum install yum-presto</blockquote>
yum的依赖问题由来已久，当然是有设计上的问题，但是也是有Packager的问题，没有及时把要更新的相关依赖移动到updates的软件库里面去(或许是Packager认为该软件包不够稳定吧！)，所以才会造成这样的问题。

解决的方法有两种：

一、如果不是很重大的更新，稍等几天，等Packager把全部软件包从updates-testing移动到updates里去，然后再去更新。

二、在更新或者安装软件包的时候，直接启用updates-testing软件库，虽然是testing，但是软件包还是比较稳定的，所以启用了问题也不会很大。当然是关键的软件包还是要小心为上！呵呵！在终端切换到root用户，然后输入命令：
<blockquote>yum update --enablerepo=updates-testing
yum install xxx --enablerepo=updates-testing //xxx是软件包的名字</blockquote>
这样，问题就能迎刃而解了。

yum更新出了问题下载不了软件包怎么办？轻按键盘的Ctrl+C一下(两下会直接取消当前运行任务)，即可跳过当前正在下载的软件包，把下面的软 件包先下载，到最后才把先前没有下载的软件包再下载。安装软件的时候被迫退出当前人物或者误关闭终端怎么办？不怕！yum是支持断点续传的，只要重复上一 条命令即可从停止处开始下载，而不是重新开始下载！

结语：对于Fedora熟手来说，直接指定一个速度快的服务器用作更新和安装软件是最适合不过的。但是对于新手来说，修改yum的配置文件不是一件容易的事情。因此我仅希望通过这篇文章来帮助Fedora新手，吸引更多的人来使用Fedora和参与Fedora项目。
