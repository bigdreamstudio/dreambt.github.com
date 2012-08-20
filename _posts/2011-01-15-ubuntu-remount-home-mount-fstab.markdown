---
layout: post
category : Linux
tags : [Linux, tutorial, Ubuntu, mount, fstab]
title: Ubuntu重新挂载 /home （有关mount和fstab）
wordpress_id: 845
wordpress_url: http://www.im47.net/?p=845
date: 2011-01-15 14:32:51.000000000 +08:00
---
<ol title="Double click to hide line number.">
	<li>ls -all /dev/disk/by-uuid</li>
</ol>
对 Linux 毫无概念的用户安装 Ubuntu 时多半仅仅挂载主目录和交换区（/swap）。

然而，这种挂载方式在长久应用中是不可取的，/home 目录包含了几乎所有的用户文档（类似 Windows 系统中的“我的文档”等），/usr 包含了用户所安装应用程序，这些不适于与系统文件混杂一处。

欲图重设 /home 等挂载点，可先为其划分新的分区之后修改与挂载点相关的系统设置。

<strong>新建分区</strong>

如果你有一块空白的磁盘或者已经从 Windows 系统中划分出新的分区，那么可以跳过了。如果你需要在 Ubuntu 系统磁盘中（即原有的 / 所挂载的分区），那么可以由 Ubuntu Live CD 启动并使用 GPart 磁盘管理工具来处理。
<ol>
	<li>准备 Ubuntu Live CD 或籍此创建的可启动 U 盘；</li>
	<li>设定 BIOS 由以上设备启动计算机；</li>
	<li>成功进入 Ubuntu Live 模式桌面；</li>
	<li>主菜单－System－Administration-Partition Editor；</li>
	<li>选定 / 所在的磁盘设备，并选定 / 所在分区；</li>
	<li>在以上分区图示上执行右键命令 Resize，解脱出 /home 所在分区所需空间；</li>
	<li>使用获取的为划分空间创建新的分区，分区格式 ext4、ext3、ntfs 均可；</li>
	<li>执行以上方案；</li>
	<li>经历漫长的过程之后，完成磁盘编辑；50G 分区耗费 1 小时，300G 分区耗费 4 小时</li>
	<li>重启计算机，进入硬盘中的 Ubuntu 系统；</li>
</ol>
<strong>转移用户文件</strong>

该过程目的是将现有 /home 目录中的所有文件备份到新建的分区中。
<ol>
	<li>挂载新分区于 /media/home</li>
	<li>拷贝 /home/ 及其所有下级文件至 /media/home/，注意拷贝隐藏文件与目录（多为程序配置）；</li>
</ol>
<strong>编辑挂载设置</strong>

此过程通过修改 fstab 信息来重设 /home 挂载点位置，这里我们需要知道 Linux 下的磁盘与分区标识规则。

<strong>查看磁盘与分区的标识信息</strong>

[code]ls -all /dev/disk/by-uuid[/code]

示例如下：

drwxr-xr-x 2 root root 180 2009-04-29 23:13 .
drwxr-xr-x 6 root root 120 2009-04-29 23:13 ..
lrwxrwxrwx 1 root root 10 2009-04-29 23:13 12FEDB1DFEDAF845 -&gt; ../../sdd1
lrwxrwxrwx 1 root root 10 2009-04-29 23:13 ab0d0ae1-da1f-49ce-91cc-42ffa03114d0 -&gt; ../../sda7
lrwxrwxrwx 1 root root 10 2009-04-29 23:13 BC8290BF82907F96 -&gt; ../../sda1
lrwxrwxrwx 1 root root 10 2009-04-29 23:13 C128A5C97B468FC6 -&gt; ../../sda5
lrwxrwxrwx 1 root root 10 2009-04-29 23:13 c80627f3-419d-405d-a987-dafbf1ed86c2 -&gt; ../../sda8
lrwxrwxrwx 1 root root 10 2009-04-29 23:13 D4CCB757CCB7331A -&gt; ../../sdb2
lrwxrwxrwx 1 root root 10 2009-04-29 23:13 D648CC1148CBEE75 -&gt; ../../sdc1
12FEDB1DFEDAF845 等字符串称为 UUID

<strong>编辑 fstab 信息</strong>

[code]sudo gedit /etc/fstab[/code]

示例如下：

# /etc/fstab: static file system information.
#
# Use 'vol_id --uuid' to print the universally unique identifier for a
# device; this may be used with UUID= as a more robust way to name devices
# that works even if disks are added and removed. See fstab(5).
#
#
proc /proc proc defaults 0 0
# / was on /dev/sda7 during installation
UUID=ab0d0ae1-da1f-49ce-91cc-42ffa03114d0 / ext4 relatime,errors=remount-ro 0 1
/dev/sda6 none swap sw 0 0
UUID=c80627f3-419d-405d-a987-dafbf1ed86c2 /home ext4 defaults 0 2
UUID=12FEDB1DFEDAF845 /media/Media ntfs-3g defaults 0 0
UUID=C128A5C97B468FC6 /media/Documents ntfs-3g defaults 0 0
UUID=D648CC1148CBEE75 /media/DataI ntfs-3g defaults 0 0
UUID=D4CCB757CCB7331A /media/DataII ntfs-3g defaults 0 0
UUID=BC8290BF82907F96 /media/Vista ntfs-3g defaults 0 0

我们看到，主要信息分为六列：

file system - 挂载设备，我们可以用 UUID 来标识
mount point - 挂载点，如我们所需的 /home
type - 分区文件系统，如 ext4、ext3、ntfs-3g、vfat 等
options - 使用该分区的方式
dump - dump 备份工具
pass - 系统扫描检测

<strong>添加 /home 挂载点设置</strong>

例如：UUID=c80627f3-419d-405d-a987-dafbf1ed86c2 /home ext4 defaults 0 2

<strong> 重启计算机。</strong>
