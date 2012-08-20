---
layout: post
category : Cloud
tags : [VirtualBox, tutorial]
title: VirtualBox虚拟硬盘VDI扩展容量(resize/expand capacity)
wordpress_id: 846
wordpress_url: http://www.im47.net/?p=846
date: 2011-01-19 09:14:30.000000000 +08:00
---
虽然VirtualBox支持虚拟硬盘的动态扩展,也就是VDI文件的大小随着guest使用的容量而增大，但是动态扩展的上限就是你最初指定的虚拟硬盘的大小值。

现在VirtualBox还没有提供改变虚拟硬盘大小上限的功能。

<img class="aligncenter size-full wp-image-847" title="gparted-300x206" src="http://www.im47.net/wp-content/uploads/2011/01/gparted-300x206.png" alt="" width="300" height="206" />

最简单的办法也许就是给guest增加一块虚拟硬盘吧，这样最省时省力了。但是我真的不想再搞出一块硬盘来，虚拟机还是越简单越好。

那怎么办呢？答案就是用一块更大容量的虚拟硬盘来替换原来的虚拟硬盘，把原来的内容完整的clone到新的虚拟硬盘上来。下面就说一说具体的步骤:
<ol>
	<li>为你的guest新增一块大容量的虚拟硬盘，并在guest的HDD设置里面把它挂在IDE的primary slave接口上，原来的硬盘一般应该在primary master上面，当然你也可以随便挂，但会影响到后面的硬盘编号。</li>
	<li>从<a href="http://gparted.sourceforge.net/">http://gparted.sourceforge.net/</a>下载GParted LiveCD,将下载的ISO文件挂载到guest的光驱上面，并且从光驱启动。简单的回车默认启动就可以了。</li>
	<li>因为新硬盘是空的，必须将旧硬盘的MBR拷贝过来，这样才能正常启动。从虚拟机桌面上点击terminal启动X终端模拟器,可以输入fdisk -l来查看一下你的硬盘设备号，按上面的设置，旧硬盘应该是hda,而新硬盘是hdb。
dd if=/dev/hda of=/dev/hdb bs=512 count=1
切记不要搞反了，否则旧硬盘的MBR就成空白了。dd是dataset definition的缩写,此命令来源于古老的IBM JCL(作业控制语言)，是一个底层I/O facility。MBR里面包含有分区表信息，这样拷贝以后新硬盘里面也有了一个和旧硬盘一般大小的分区，这是我们不需要的，删除掉先。
fdisk /dev/hdb,然后输入fdisk命令d也就是在Command (m for help):后面输入d就可以删除掉这个分区，然后输入fdisk命令w把改变写回硬盘，然后q退出。</li>
	<li>启动GParted程序。GParted会扫描到这两个硬盘。在旧硬盘hda的分区(我的是主分区hda1)上面右击选择copy,然后选择新硬盘hdb,在其上右击选择paste,并把目的分区拖到最大，在新硬盘的主分区hdb1上右击选择”manage flags”,为此分区添加boot标志，以便从该分区启动。</li>
	<li>从虚拟机设置里面为guest去掉cd rom，去掉旧的虚拟硬盘，把新虚拟硬盘挂载到IDE的Primary master上面，启动guest。第一次用新硬盘启动可能会遇到磁盘检查。</li>
</ol>
<ol>到此应该就OK了,以后新建guest的时候一定要把虚拟硬盘搞大一点，省的这么麻烦。</ol>
