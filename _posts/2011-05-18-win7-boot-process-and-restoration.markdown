---
layout: post
category : Windows
tags : [Windows, Win7, how-to, boot]
title: Win7启动过程及启动项修复
wordpress_id: 952
wordpress_url: http://www.im47.net/?p=952
date: 2011-05-18 19:55:13.000000000 +08:00
---
先让我们看一下win7的启动过程的常识：

电脑加电后，首先是启动BIOS程序，BIOS自检完毕后，找到硬盘上的主引导记录MBR，MBR读取DPT（分区表），从中找出活动的主分区，然后读取活动主分区的PBR（分区引导记录，也叫dbr），PBR再搜寻分区内的启动管理器文件 BOOTMGR，在BOOTMGR被找到后，控制权就交给了BOOTMGR。BOOTMGR读取\boot\bcd文件（BCD=Boot Configuration Data）,也就是“启动配置数据”，简单地说，win7下的bcd文件就相当于xp下的boot.ini文件），如果存在着多个操作系统并且选择操作系统的等待时间不为0的话，这时就会在显示器上显示操作系统的选择界面。在我们选择启动WINDOWS7后，BOOTMGR就会去启动盘寻找WINDOWS\system32\winload.exe，然后通过winload.exe加载win7内核，从而启动整个win7系统。

可以把这个过程简单地概括为：

<strong>BIOS--&gt;MBR--&gt;DPT--&gt;pbr--&gt;Bootmgr--&gt;bcd--&gt;Winload.exe--&gt;内核加载 --&gt;整个win7系统</strong>

本文就来说一说MBR--&gt;DPT--&gt;pbr--&gt; Bootmgr--&gt;bcd这一段可能出现的故障的解决。

<strong>1.MBR出现问题</strong>

主要是mbr代码被改写，因为被改写的代码不同，所以出错信息也各不相同。比如我们装了win7与ubuntu双系 统，ubuntu改写了mbr，在我们把ubuntu所在的分区格式化后，既进不了win7，也进不了ubuntu，开机的时候会出现如图的错误 提示：
<blockquote>GRUB Loading stage1.5.

GRUB loading, please wait...

Error 22</blockquote>
解决的办法就是重写mbr。对于重写mbr，我们所熟知的是在dos下用fdisk /mbr命令进行重写。fdisk /mbr所重写的mbr与xp是兼容的，但是，与win7已经不那么兼容了。实践表明：用fdisk /mbr命令重写win7的mbr后，需要重建bcd，否则不能正常启动win7。有网友指出，这里面的原因是fdisk /mbr命令改写了mbr中的硬盘签名。一般的分区工具都是可以重写mbr的，比如diskgenius，它所重写的mbr与win7是兼容的。 也可以用bootrec /fixmbr命令重写。要运行 Bootrec.exe 工具，必须启动 Windows RE。为此，请按照下列步骤操作：
<blockquote>插入windows 7安装光盘，从光盘启动电脑，在光盘启动完成后，按下shift+f10键，调出cmd命令提示符。在cmd命令提示符中输入：bootrec /fixmbr回车。</blockquote>
这样也就重写了mbr。

<strong>2.分区表存在问题</strong>

系统盘不是活动的主分区，这种情形只要用分区工具（比如diskgenius)把系统盘设为活动的主分区即可。
pbr出现问题，主要是pbr代码被改写，因为被改写的代码不同，所以出错信息也不相同。比如Win7系统的活动分区，却被写入了适合于XP的 pbr，这样开机的时候就会出现如图的提示：
<blockquote>NTLDR is missing

Press Ctrl+Alt+Del to restart</blockquote>
简单的解决办法就是用bootrec /fixboot命令重写pbr:
<blockquote>插入win7安装光盘，从光盘启动，在光盘启动完成后，按下shift+f10键，调出cmd命令提示符。在命令提示符中输入：bootrec /fixboot回车。</blockquote>
这样也就重建了活动分区的pbr。
这里面还有一个常用的命令也要提一下，这就是bootsect：
<blockquote>插入win7安装光盘，从光盘启动，在光盘启动完成后，按下shift+f10键，调出cmd命令提示符。在cmd命令提示符中输 入：bootsect /nt60 sys /mbr回车。</blockquote>
这个命令会改写活动分区的pbr，并同时会改写mbr，使得mbr和pbr适合于win7和vista。
bootsect.exe程序位于win7安装光盘的boot目录下，可以把这个文件提取出来，在xp下的命令行可以运行这个程序，也可以在 winpe下的命令行运行这个程序，因而这个程序在使用时很方便。而bootrec.exe命令的使用就没这么方便了。所以BOOTSECT命令被应用得更为广泛一些。
另外有一个要点需要指出，vista的安装光盘里面的boot文件夹也存在着这个小工具，但vista的bootsect命令没有/mbr参数，因而它只能改写pbr，而不能改写mbr，这是必须要注意的。实践表明：把一个硬盘的mbr清零，然后运行win7的bootsect命令，确实可以发现mbr被恢复正常。这也就表明了win7的bootsect命令的确能够重写mbr。
另外，bootsect命令也可以重写xp的mbr和pbr，而这也是bootrec命令所做不到的。xp的恢复控制台用fixmbr命令改写mbr,用 fixboot命令改写pbr。

<strong>3.引导文件的问题</strong>

一般可以用bcdboot命令重新写入引导文件：
<blockquote>插入win7安装光盘，从光盘启动，在光盘启动完成后，按下shift+f10键，调出cmd命令提示符。在命令提示符中输入：
bcdboot x:\windows /s x:</blockquote>
注意，这前一个x:是win7的windows文件夹所在的盘，一般是c:，如果你的不是c盘，请改为对应的盘符。这后一个x:是活动主分区的盘符所在，一般也是c盘。所以这个命令一般的写法是：bcdboot c:\windows /s c:
但需要注意，在windows re环境下所看到的盘符与你在win7下所看到的盘符未必一样。所以需要首先用dir /a命令确认各盘是否正确。
比如：
<blockquote>cd /d c:
dir /a</blockquote>
这两个命令的作用是，首先进入c:盘的根目录，然后显示c盘根目录下的所有文件和文件夹，根据所显示的文件或者文件夹，可以判断这个盘具体是你在 win7下所看到的哪一个盘。
win7的引导文件主要是bootmgr和boot文件夹里面的文件，而boot文件夹里面的文件主要是bcd文件。bcdboot命令会在指定 的分区内重新写入全部win7的引导文件。
如果只是bcd文件有问题，则可以用bootrec命令重建bcd:
<blockquote>插入win7安装光盘，从光盘启动，在光盘启动完成后，按下shift+f10键，调出cmd命令提示符。在命令提示符中输入：
bootrec /RebuildBcd</blockquote>
这个命令如果搜到没有写入bcd的win7或者vista的操作系统，会提示你是否写入，按提示输入Y也就会写入了的。
或者用bcdedit命令手动改写bcd，但操作要复杂得多。

<strong>具体案例分析</strong>

案例一：开机的时候出现：
<blockquote>BOOTMGR is missing
press ctrl+alt+del to restart.</blockquote>
翻译成汉语就是：bootmgr缺失，按Ctrl + Alt + Del重新启动
这是很常见的故障。既然是bootmgr缺失，我们一般只要用bcdboot命令重建引导文件即可。
这种情形产生的原因，一般可能有：bootmgr文件确实没有了，这是最为常见的。一种则是由磁盘错误导致的，这种情形下，在winpe下运行一下 chkdsk /f命令也可能解决。有朋友使用 Diskeeper 对MFT碎片进行整理，开机的时候也出现了这个提示。估计可能是用DISKEEPER进行的MFT磁盘整理后，这或者是diskeeper的一个bug， 因而不建议用diskeeper进行mft碎片整理。
一位网友因为好奇。把C盘设成了活动的（active partition ）。是这样设置活动的：对计算机点右键-管理-硬盘管理。右键点C盘，设置为活动的。靠。怎么回事啊。
重启后居然无法启动！显示bootmgr is missing，Ctrl+Alt+Delete to restart。然后还是如此。
这是从网上找到的一个案例，分析可以得出结论。他所装的windows7应该存在着一个隐藏的“系统保留”分区，这个隐藏的系统保留分区才是真正的活动主 分区，而他的c盘则应该不是活动的。他把c盘设为活动，这也就意味着取消了“系统保留”分区的活动状态。但引导文件是在“系统保留”分区，而不是在c 盘，c盘变成了活动的主分区，mbr就会启动c盘的pbr，而c盘的pbr又会去c盘找bootmgr,但c盘没有bootmgr，所以出错也就是必然的 了。解决的办法其实只要简单地再把系统保留分区设为活动即可。
这位朋友制造了问题，但好象并没能最后解决问题。真所谓会者不难，难者不会。

&nbsp;

案例二：开机的时候出现：
<blockquote>BOOTMGR is compressed
Press Ctrl+Alt+Del to restart</blockquote>
翻译成汉语就是：
bootmgr被压缩，按Ctrl + Alt + Del重新启动
这种情形产生的原因是因为对系统盘进行了压缩。奇怪的是，对于这种情形，我们用bcdboot命令重建引导文件却并不能解决。
但是，我们可以运行命令：compact /u /a /f /i /s c:\*
这样可以使得问题得到解决。compact程序位于windows\system32文件夹下，所以我们要先用CD命令进入windows\system32目录。这里是假设c:盘是bootmgr所在的盘，如果不是，要改为对应的盘符。
网上有朋友用这个命令的时候并没有解决问题，原因则在于，这位朋友所运行的命令是：compact /u /a /f /i /s c:\
没有后面这个＊，所以命令并没有实现运行者的目的。从命令本身所提供的帮助说明来看，这个＊似乎是没有必要的，但实际操作表明，这个＊是必须的。
这个命令会把已经压缩的C盘文件完全解压，真所谓解铃还須系铃人。
注意，只运行命令：compact /u /a /f /i c:\bootmgr
并不能解决问题。
有网友发现，运行“Bootrec.exe /fixmbr、Bootrec /fixboot"然后重启，这样可以解决问题。测试表明，其实只需要运行Bootrec /fixboot这一个命令即可。这是另类的解决的办法。猜想可能是，对驱动压缩后，PBR中的BPB表并没有随之修改，所以BPB表中所记录的分区信息 与实际的分区信息不一致。运行Bootrec /fixboot命令后重写了bpb，这样就使得二者变为了一致。

实践表明：用bootsect命令也能实现对这个问题的解决。
有网友发贴，说是装了xp与vista双系统，启动vista系统出现了BOOTMGR is compressed ，于是他在xp下取消了系统盘的压缩状态。但这位网友的话未必可信，因为如果ntldr也被压缩了的话，则xp启动的时候会出现：
<blockquote>ntldr is compressed
Press Ctrl+Alt+Del to restart</blockquote>
除非这位朋友只压缩了bootmgr，而没有压缩ntldr,但这一般不太可能。这种压缩一般是对整个盘进行压缩的时候产生的，如果压缩指定文件的话，一 般不会有人去压缩bootmgr和ntldr的。实践表明，在win7下，即便指定对整个的系统盘进行压缩，一般也不能压缩bootmgr的，会 提示拒绝访问，但是，在开机的时候仍会出现出错提示：
<blockquote>bootmgr is compressed</blockquote>
案例三：
先装的win7，后装的linux,在linux系统出问题后，既进不了linux,也进不了win7，这里面的原因是mbr和活动分区 的pbr被改写。只要重建mbr和活动分区的pbr，也就可以进入win7了。最简单的办法是用bootsect命令解决：
<blockquote>bootsect /nt60 sys /mbr</blockquote>
案例四：
先装的win7，后装的xp,没有了win7的启动项：
这个需要三步解决问题：
一、用bcdboot命令重建win7的引导文件。
二、用bootsect命令恢复win7的mbr和pbr
三、进入win7后，用bcdedit命令添加xp的启动项。
