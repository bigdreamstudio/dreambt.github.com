---
layout: post
category : Windows
tags : [Windows, VirtualBox, Win7]
title: VirtualBox中使用物理硬盘上的Win7
wordpress_id: 1112
wordpress_url: http://www.im47.cn/?p=1112
date: 2012-02-23 17:39:05.000000000 +08:00
---
<h2><span style="color: #ff0000;">操作有风险，动手须谨慎！</span></h2>
<h2><strong>前提</strong></h2>
1.在硬盘上安装Ubuntu、Win7(或者XP)

2.在Ubuntu安装<a href="https://www.virtualbox.org/wiki/Downloads">VirtualBox</a>

3.在Ubuntu安装MBR
<pre>sudo apt-get install mbr</pre>
<h2><strong>方法</strong></h2>
1.提升硬盘访问权限

<pre>chmod o+rw /dev/sda //全盘获取读写权限，不安全
chmod o+rw /dev/sda3 //第3个分区获得读写权限</pre>

1.配置引导区
<pre>install-mbr -e12 --force vm.mbr</pre>

2.将物理硬盘生成VirtualBox配置文件
<pre>VBoxManage internalcommands createrawvmdk -filename ~/Win7.vmdk -rawdisk /dev/sda -partitions 3 -mbr vm.mbr -relative
//将第3个分区生成配置文件，并导入MBR文件</pre>

3.修改vmdk等文件的所有者和访问权限
<pre>chown siqi:siqi /home/siqi/VM/Win7-64/*
chmod o+rw /home/siqi/VM/Win7-64/*
</pre>
4.在VirtualBox界面添加生成的Win7.vmdk文件
