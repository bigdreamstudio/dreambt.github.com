---
layout: post
category : Linux
tags : [how-to, Linux, Ubuntu, grub]
title: Ubuntu下Grub2修复详细步骤
wordpress_id: 930
wordpress_url: http://www.im47.net/?p=930
date: 2011-04-29 22:21:46.000000000 +08:00
---
有没有重装双系统之后找不到启动项的经历？

有没有进入Grub修复模式却不知道用什么命令修复启动项？

好吧，看了本文也许你就会留下幸福的泪水，好吧，废话少说。
<h2>一、开机提示grub&gt;或者还有Grub界面请尝试</h2>
用工具盘启动，在grub菜单上按c进入命令行状态
在grub&gt;提示符下输入
grub&gt;find /boot/grub/core.img (有/boot分区的用find /grub/core.img)
系统会显示(hdx,y) (查找到的分区号），然后输入
grub&gt;root (hdx,y)
grub&gt;kernel /boot/grub/core.img (/boot分区的用 kernel /grub/core.img)
grub&gt;boot
执行boot后能转入grub2菜单，重启ubuntu后，再在ubuntu终端下执行
$sudo grub-install /dev/sda
(或sdb，sdc等，根据第几硬盘而定）修复grub
注意：如果ubuntu的启动分区使用ext4格式，要有支持ext4格式的grub才能修复
<h2>二、上面的方法不可以的话，就用Live CD吧～</h2>
用ubuntu9.10的liveCD试用ubuntu启动后，打开终端
假如你的ubuntu的 / 分区是sda9，又假如 /boot分区是 sda6，在终端下输入
$sudo -i
$mount /dev/sda7 /mnt
$mount /dev/sda6 /mnt/boot （如果没 /boot 单独分区这步跳过）
$grub-install --root-directory=/mnt/ /dev/sda
和前面一样，要装入第二硬盘的把sda改为sdb
修复后无法引导windows，可以用下面的方法解决：
进入ubuntu系统，打开终端，重建grub列表
$sudo update-grub
重新写入第一分区mbr
$sudo grub-install /dev/sda
如果想修改启动顺序，可以修改/boot/grub目录下的grub.cfg文件
注意此文件不可写的，先执行一下命令
$sudo chmod +w /boot/grub/grub.cfg
然后再执行
$sudo gedit /boot/grub/grub.cfg
修改，类似于grub1的menu.lst修改Grub rescue模式
rescue模式下可使用的命令有：set，ls，insmod，root，prefix(设置启动路径)
ls --列出分区
ls (hd0,8)/ --查看(hd0,8)分区根目录
找到grub目录，然后继续
grub rescue&gt;root=(hd0,x)
grub rescue&gt;prefix=/boot/grub --(grub的目录)
grub rescue&gt;set root=(hd0,x)
grub rescue&gt;set prefix=(hd0,x)/boot/grub
grub rescue&gt;insmod normal
grub&gt;normal --------若出现启动菜单，按c进入命令行模式
grub&gt;linux /boot/vmlinuz root=/dev/sdax
grub&gt;initrd /boot/initrd.img
grub&gt;boot
完成
进入系统后，更新GRUB或重装GRUB:
更新：sudo update-grub
重装：sudo grub-install /dev/xxx (这儿的xxx是sda或者sdb)
<h2>三、如果还不行，不用重装系统的最后一招了</h2>
<strong>详细步骤1</strong>

Boot to the LiveCD Desktop.

Open a terminal – Applications, Accessories, Terminal.

Determine your normal system partition – (the switch is a lowercase “L”)

sudo fdisk -l

If you aren’t sure, rundf -Th . Look for the correct disk size and ext3 or ext4 format.

Mount your normal system partition:

Substitute the correct partition: sda1, sdb5, etc.

sudo mount /dev/sdXX /mnt # Example: sudo mount /dev/sda1 /mnt

<strong>详细步骤2</strong>

Only if you have a separate boot partition :

sdYY is the /boot partition designation (examply sdb3)

sudo mount /dev/sdYY /mnt/boot

Mount devices:sudo mount --bind /dev/ /mnt/dev

To ensure that only the grub utilities from the LiveCD get executed, mount /usrsudo mount --bind /usr/ /mnt/usr

mount proc filesystemsudo mount --bind /proc/ /mnt/proc

Chroot into your normal system device:sudo chroot /mnt

If there is no /boot/grub/grub.cfg or it’s not correct, create one usingupdate-grub

Reinstall GRUB 2:

Substitute the correct device – sda, sdb, etc. Do not specify a partition number.

grub-install /dev/sdX

<strong>详细步骤3</strong>

Verify the install (use the correct device, for example sda . Do not specify a partition): sudo grub-install --recheck /dev/sdX

Exit chroot : CTRL-D on keyboard

Unmount devices:sudo umount /mnt/dev

If you mounted a separate /boot partition:sudo umount /mnt/boot

Unmount last device:sudo umount /mnt

Reboot.reboot

Post-Restoration Commands

Once the user can boot to a working system, try to determine why the system failed to boot.

The following commands may prove useful in locating and/or fixing the problem.

<strong>详细步骤4</strong>

To refresh the available devices and settings in /boot/grub/grub.cfg

sudo update-grub

To look for the bootloader location.

grub-probe -t device /boot/grub

To install GRUB 2 to the sdX partition’s MBR (sda, sdb, etc.)

sudo grub-install /dev/sdX

To recheck the installation. (sda, sdb, etc.) sudo grub-install --recheck /dev/sdX

Please check the following link for further details.
