---
layout: post
category : Linux
tags : [Linux, question, Ubuntu, how-to, grub]
title: 开机显示grub secure的ubuntu启动项修复
wordpress_id: 844
wordpress_url: http://www.im47.net/?p=844
date: 2011-01-16 14:15:20.000000000 +08:00
---
昨晚下午给Ubuntu系统增加了160GB的存储空间，但是调整完系统分区之后，Ubuntu无法正常启动了。无光驱，LiveCD没法用，在同学笔记本上刻的U盘启动不起来。只好自己摸索，问题在2个小时之后成功解决。

症状：

开机显示grub secure&gt;

描述：

grub secure是grub修复命令行，支持简单的命令。

修复过程：

[code]
grub secure&gt; ls #查看所有的分区情况，比如我的linux分区是(hd0,msdos7)，下面你可以写(hd0,msdos7)如果你不嫌麻烦，你也可以写(hd0,7)
grub secure&gt; set root=(hd0,7) #Ubuntu挂载根目录
grub secure&gt; set prefix=(hd0,7)/boot/grub #指定grub.cfg所在目录
grub secure&gt; set #查看root和prefix是否设置成功
grub secure&gt; insmod normal
grub secure&gt; normal
系统会自动重新启动，并显示启动菜单，选择Ubuntu系统。
进入系统后，打开Terninal终端。
siqi@siqi-laptop:~$ sudo update-grub2 #更新grub.cfg文件
siqi@siqi-laptop:~$ sudo grub-install /dev/sdb #写入MBR
siqi@siqi-laptop:~$ sudo reboot #重启[/code]
