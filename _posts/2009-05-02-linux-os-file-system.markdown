---
layout: post
category : Linux
tags : [intro, beginner, Linux, FS]
title: Linux操作系统文件系统基础知识详解
wordpress_id: 153
wordpress_url: http://www.im47cn.nt102.idcwind.net/wordpress/?p=153
date: 2009-05-02 07:54:02.000000000 +08:00
---
<a name="ch1"></a>

文件结构是文件存放在磁盘等存贮设备上的组织方法。主要体现在对文件和目录的组织上。
目录提供了管理文件的一个方便而有效的途径。
Linux使用标准的目录结构，在安装的时候，安装程序就已经为用户创建了文件系统和完整而固定的目录组成形式，并指定了每个目录的作用和其中的文件类型。
/根目录
┃
┏━━━━┳━━━━━┳━━━━━┳━━━━━╋━━━━━┳━━━━━┳━━━━━┳━━━━━┓
┃       ┃          ┃         ┃          ┃         ┃         ┃          ┃         ┃
bin    home      dev       etc        lib      sbin       tmp       usr       var
┃
┏━┻━┓
┃      ┃
rc.d   cron.d
┃
┏━━━┳━━━┳━┻━┳━━━━┓
┃      ┃     ┃      ┃       ┃
init.d  rc0.d rc1.d  rc2.d    ……
┃                                                      ┃
┏━┻━┓         ┏━━┳━━┳━━┳━┻━┳━━┓
┃          ┃       ┃       ┃       ┃     ┃          ┃     ┃
rc.d    cron.d     X11R6 src    lib     local      man     bin
┃
┏━━━┳━━┳━┻━┳━━━┓
┃         ┃       ┃          ┃         ┃
init.d   rc0.d    rc1.d rc2.d …… linux bin lib src

Linux采用的是树型结构。最上层是根目录，其他的所有目录都是从根目录出发而生成的。微软的DOS和windows也是采用树型结构，但是在 DOS和 windows中这样的树型结构的根是磁盘分区的盘符，有几个分区就有几个树型结构，他们之间的关系是并列的。但是在linux中，无论操作系统管理几个磁盘分区，这样的目录树只有一个。从结构上讲，各个磁盘分区上的树型目录不一定是并列的。
下面列出了linux下一些主要目录的功用。

/bin        二进制可执行命令
/boot        开机设定目录，也是摆放核心 vmlinuz 的地方
/dev        设备特殊文件
/etc        系统管理和配置文件,尤其 passwd, shadow
/etc/rc.d    启动的配置文件和脚本
/etc/rc.d/init.d系統开机的時候载入服务的 scripts 的摆放地点
/etc/passwd     用户数据库，其中的域给出了用户名、真实姓名、家目录、加密的口令和用户的其他信息。
/etc/fdprm      软盘参数表。说明不同的软盘格式。用setfdprm 设置。
/etc/fstab     启动时mount -a命令(在/etc/rc 或等效的启动文件中)自动mount的文件系统列表。 Linux下，也包括用swapon -a启用的swap区的信息。
/etc/group     类似/etc/passwd ，但说明的不是用户而是组。
/etc/inittab     init 的配置文件。
/etc/issue     getty 在登录提示符前的输出信息。通常包括系统的一段短说明或欢迎信息。内容由系统管理员确定。
/etc/magic     file 的配置文件。包含不同文件格式的说明，file 基于它猜测文件类型。
/etc/motd     Message Of The Day，成功登录后自动输出。内容由系统管理员确定。经常用于通告信息，如计划关机时间的警告。
/etc/mtab     当前安装的文件系统列表。由scripts初始化，并由mount 命令自动更新。需要一个当前安装的文件系统的列表时使用，例如df 命令。
/etc/shadow     在安装了影子口令软件的系统上的影子口令文件。影子口令文件将/etc/passwd 文件中的加密口令移动到/etc/shadow 中，而后者只对root可读。这使破译口令更困难。
/etc/login.defs login 命令的配置文件。
/etc/printcap     类似/etc/termcap ，但针对打印机。语法不同。
/etc/profile
/etc/csh.login
/etc/csh.cshrc     登录或启动时Bourne或C shells执行的文件。这允许系统管理员为所有用户建立全局缺省环境。
/etc/securetty     确认安全终端，即哪个终端允许root登录。一般只列出虚拟控制台，这样就不可能(至少很困难)通过modem或网络闯入系统并得到超级用户特权。
/etc/shells     列出可信任的shell。chsh 命令允许用户在本文件指定范围内改变登录shell。提供一台机器FTP服务的服务进程ftpd 检查用户shell是否列在 /etc/shells 文件中，如果不是将不允许该用户登录。
/etc/termcap     终端性能数据库。说明不同的终端用什么"转义序列"控制。写程序时不直接输出转义序列(这样只能工作于特定品牌的终端)，而是从/etc/termcap 中查找要做的工作的正确序列。这样，多数的程序可以在多数终端上运行。
/home        用户主目录的基点，比如用户user的主目录就是/home/user，可以用~user表示
/lib        标准程序设计库，又叫动态链接共享库，作用类似windows里的.dll文件
/sbin        系统管理命令，这里存放的是系统管理员使用的管理程序
/tmp        公用的临时文件存储点
/root        系统管理员的主目录
/mnt        系统提供这个目录是让用户临时挂载其他的文件系统。
/lost+found    摆放系统不正常产生错误时遗失的片段
/proc        虚拟的目录，是系统内存的映射。可直接访问这个目录来获取系统信息。它不存在在磁盘某个磁盘上。而是由核心在内存中产生。用于提供关于系统的信息(originally about processes, hence the name)。
/proc/1     关于进程1的信息目录。每个进程在/proc 下有一个名为其进程号的目录。
/proc/cpuinfo    处理器信息，如类型、制造商、型号和性能。
/proc/devices     当前运行的核心配置的设备驱动的列表。
/proc/dma     显示当前使用的DMA通道。
/proc/filesystems 核心配置的文件系统。
/proc/interrupts 显示使用的中断，and how many of each there have been.
/proc/ioports     当前使用的I/O端口。
/proc/kcore    系统物理内存映象。与物理内存大小完全一样，但不实际占用这么多内存；it is generated on the fly as programs access it. (记住：除非你把它拷贝到什么地方，/proc 下没有任何东西占用任何磁盘空间。)
/proc/kmsg     核心输出的消息。也被送到syslog 。
/proc/ksyms     核心符号表。
/proc/loadavg     系统"平均负载"；3个指示器指出系统当前的工作量。
/proc/meminfo     存储器使用信息，包括物理内存和swap。
/proc/modules     当前加载了哪些核心模块。
/proc/net     网络协议状态信息。
/proc/self     到查看/proc 的程序的进程目录的符号连接。当2个进程查看/proc 时，是不同的连接。这主要便于程序得到它自己的进程目录。
/proc/stat     系统的不同状态，such as the number of page faults since the system was booted.
/proc/uptime     系统启动的时间长度。
/proc/version     核心版本。
/var        某些大文件的溢出区，比方说各种服务的日志文件
/usr        最庞大的目录，要用到的应用程序和文件几乎都在这个目录。其中包含：
/usr/X11R6    存放X window的目录
/usr/bin    众多的应用程序
/usr/sbin    超级用户的一些管理程序
/usr/doc    linux文档
/usr/include    linux下开发和编译应用程序所需要的头文件
/usr/lib    常用的动态链接库和软件包的配置文件
/usr/man    帮助文档
/usr/src    源代码，linux内核的源代码就放在/usr/src/linux里
/usr/local/bin  本地增加的命令
/usr/local/lib  本地增加的库
/usr/X11R6     X Window系统的所有文件。为简化X的开发和安装，X的文件没有集成到系统中。 X自己在/usr/X11R6 下类似/usr 。
/usr/X386     类似/usr/X11R6 ，但是给X11 Release 5的。
/usr/bin     几乎所有用户命令。有些命令在/bin 或/usr/local/bin 中。
/usr/sbin    根文件系统不必要的系统管理命令，例如多数服务程序。
/usr/man
/usr/info
/usr/doc     手册页、GNU信息文档和各种其他文档文件。
/usr/include     C编程语言的头文件。为了一致性这实际上应该在/usr/lib 下，但传统上支持这个名字。
/usr/lib     程序或子系统的不变的数据文件，包括一些site-wide配置文件。名字lib来源于库(library); 编程的原始库存在/usr/lib 里。
/usr/local     本地安装的软件和其他文件放在这里。
/var        包括系统一般运行时要改变的数据。每个系统是特定的，即不通过网络与其他计算机共享。
/var/catman    当要求格式化时的man页的cache。man页的源文件一般存在/usr/man/man*中；有些man页可能有预格式化的版本，存在/usr/man/cat*中。而其他的man页在第一次看时需要格式化，格式化完的版本存在/var/man 中，这样其他人再看相同的页时就无须等待格式化了。(/var/catman 经常被清除，就象清除临时目录一样。)
/var/lib     系统正常运行时要改变的文件。
/var/local     /usr/local 中安装的程序的可变数据(即系统管理员安装的程序)。注意，如果必要，即使本地安装的程序也会使用其他/var 目录，例如/var/lock 。
/var/lock     锁定文件。许多程序遵循在/var/lock 中产生一个锁定文件的约定，以支持他们正在使用某个特定的设备或文件。其他程序注意到这个锁定文件，将不试图使用这个设备或文件。
/var/log    各种程序的Log文件，特别是login (/var/log/wtmp log所有到系统的登录和注销) 和syslog(/var/log/messages 里存储所有核心和系统程序信息。 /var/log 里的文件经常不确定地增长，应该定期清除。
/var/run     保存到下次引导前有效的关于系统的信息文件。例如， /var/run/utmp 包含当前登录的用户的信息。

/var/spool mail, news, 打印队列和其他队列工作的目录。每个不同的spool在/var/spool 下有自己的子目录，例如，用户的邮箱在/var/spool/mail 中。
/var/tmp     比/tmp 允许的大或需要存在较长时间的临时文件。 (虽然系统管理员可能不允许/var/tmp 有很旧的文件。)

主要目录
根、/usr 、/var 和 /home
每台机器都有根文件系统，它包含系统引导和使其他文件系统得以mount所必要的文件，根文件系统应该有单用户状态所必须的足够的内容。还应该包括修复损坏系统、恢复备份等的工具。
/usr
文件系统包含所有命令、库、man页和其他一般操作中所需的不改变的文件。 /usr不应该有一般使用中要修改的文件。这样允许此文件系统中的文件通过网络共享，这样可以更有效，因为这样节省了磁盘空间(/usr很容易是数百兆)，且易于管理(当升级应用时，只有主/usr 需要改变，而无须改变每台机器)
即使此文件系统在本地盘上，也可以只读mount，以减少系统崩溃时文件系统的损坏。
/var 文件系统包含会改变的文件，比如spool目录(mail、news、打印机等用的)， log文件、formatted manual pages和暂存文件。传统上/var 的所有东西曾在 /usr 下的某个地方，但这样/usr 就不可能只读安装了。
/home 文件系统包含用户家目录，即系统上的所有实际数据。一个大的/home 可能要分为若干文件系统，需要在/home 下加一级名字，如/home/students 、/home/staff 等。

在同一分区上，名字/usr/lib/libc.a 和/var/adm/messages 必须能工作，例如将/var下的文件移动到/usr/var ，并将/var 作为/usr/var 的符号连接。

根文件系统
根文件系统一般应该比较小，因为包括严格的文件和一个小的不经常改变的文件系统不容易损坏。损坏的根文件系统一般意味着除非用特定的方法(例如从软盘)系统无法引导。
根目录一般不含任何文件，除了可能的标准的系统引导映象，通常叫/vmlinuz 。所有其他文件在根文件系统的子目录中。
/bin   引导启动所需的命令或普通用户可能用的命令(可能在引导启动后)。
/sbin   类似/bin ，但不给普通用户使用，虽然如果必要且允许时可以使用。
/etc   特定机器的配置文件。
/root   root用户的家目录。
/lib   根文件系统上的程序所需的共享库。
/lib/modules 核心可加载模块，特别是那些恢复损坏系统时引导所需的(例如网络和文件系统驱动)。
/dev 设备文件。
/tmp 临时文件。引导启动后运行的程序应该使用/var/tmp ，而不是/tmp ，因为前者可能在一个拥有更多空间的磁盘上。
/boot 引导加载器(bootstraploader)使用的文件，如LILO。核心映象也经常在这里，而不是在根目录。如果有许多核心映象，这个目录可能变得很大，这时可能使用单独的文件系统更好。另一个理由是要确保核心映象必须在IDE硬盘的前1024柱面内。
/mnt 系统管理员临时mount的安装点。程序并不自动支持安装到/mnt 。 /mnt 可以分为子目录(例如/mnt/dosa 可能是使用MSDOS文件系统的软驱，而/mnt/exta 可能是使用ext2文件系统的软驱)。
/dev目录 /dev 目录包括所有设备的设备文件。设备文件用特定的约定命名。
/usr
文件系统 /usr 文件系统经常很大，因为所有程序安装在这里。 /usr 里的所有文件一般来自Linux
distribution；本地安装的程序和其他东西在/usr/local
下。这样可能在升级新版系统或新distribution时无须重新安装全部程序。

<a name="ch2"></a>

文件系统指文件存在的物理空间，linux系统中每个分区都是一个文件系统，都有自己的目录层次结构。linux会将这些分属不同分区的、单独的文件系统按一定的方式形成一个系统的总的目录层次结构。一个操作系统的运行离不开对文件的操作，因此必然要拥有并维护自己的文件系统。
Llinux文件系统使用索引节点来记录文件信息，作用像windows的文件分配表。
索引节点是一个结构，它包含了一个文件的长度、创建及修改时间、权限、所属关系、磁盘中的位置等信息。一个文件系统维护了一个索引节点的数组，每个文件或目录都与索引节点数组中的唯一一个元素对应。系统给每个索引节点分配了一个号码，也就是该节点在数组中的索引号，称为索引节点号。
linux文件系统将文件索引节点号和文件名同时保存在目录中。所以，目录只是将文件的名称和它的索引节点号结合在一起的一张表，目录中每一对文件名称和索引节点号称为一个连接。
对于一个文件来说有唯一的索引节点号与之对应，对于一个索引节点号，却可以有多个文件名与之对应。因此，在磁盘上的同一个文件可以通过不同的路径去访问它。
可以用ln命令对一个已经存在的文件再建立一个新的连接，而不复制文件的内容。连接有软连接和硬连接之分，软连接又叫符号连接。它们各自的特点是：
硬连接：原文件名和连接文件名都指向相同的物理地址。
目录不能有硬连接；硬连接不能跨越文件系统（不能跨越不同的分区）
文件在磁盘中只有一个拷贝，节省硬盘空间；
由于删除文件要在同一个索引节点属于唯一的连接时才能成功，因此可以防止不必要的误删除。
符号连接：用ln -s命令建立文件的符号连接符号连接是linux特殊文件的一种，作为一个文件，它的数据是它所连接的文件的路径名。类似windows下的快捷方式。
可以删除原有的文件而保存连接文件，没有防止误删除功能。
这一段的的内容过于抽象，又是节点又是数组的，我已经尽量通俗再通俗了，又不好加例子作演示。大家如果还是云里雾里的话，我也没有什么办法了，只有先记住，日后在实际应用中慢慢体会、理解了。这也是我学习的一个方法吧。
<a name="ch3"></a>

由上一节知道，linux系统中每个分区都是一个文件系统，都有自己的目录层次结构。linux会将这些分属不同分区的、单独的文件系统按一定的方式形成一个系统的总的目录层次结构。这里所说的“按一定方式”就是指的挂载。
将一个文件系统的顶层目录挂到另一个文件系统的子目录上，使它们成为一个整体，称为挂载。把该子目录称为挂载点。
举个例子吧：
根分区：
/根目录
┃
┏━━━━┳━━━━━┳━━━━━┳━━━━━╋━━━━━┳━━━━━┳━━━━━┳━━━━━┓
┃       ┃          ┃         ┃          ┃         ┃         ┃          ┃         ┃
bin    home      dev       etc        lib      sbin       tmp       usr       var
┃
┏━┻━┓
┃      ┃
rc.d   cron.d
┃
┏━━━┳━━━┳━┻━┳━━━━┓
┃      ┃     ┃      ┃       ┃
init.d  rc0.d rc1.d  rc2.d    ……

/usr分区 ：
usr
┃
┏━━━━┳━━━╋━━━┳━━━┳━━━┓
┃       ┃      ┃      ┃     ┃      ┃
X11 R6  src    lib  local  man     bin
┃                ┃
┃ ┏━━━╋━━━┓
┃   ┃          ┃            ┃
linux bin lib src

<a name="ch4"></a>

在/etc目录下有个fstab文件，它里面列出了linux开机时自动挂载的文件系统的列表。我的/etc/fstab文件如下：
/dev/hda2 / ext3 defaults 1 1
/dev/hda1 /boot ext3 defaults 1 2
none /dev/pts devpts gid=5,mode=620 0 0
none /proc proc defaults 0 0
none /dev/shm tmpfs defaults 0 0
/dev/hda3 swap swap defaults 0 0
/dev/cdrom /mnt/cdrom iso9660 noauto,codepage=936,iocharset=gb2312 0 0
/dev/fd0 /mnt/floppy auto noauto,owner,kudzu 0 0
/dev/hdb1 /mnt/winc vfat defaults,codepage=936,iocharset=cp936 0 0
/dev/hda5 /mnt/wind vfat defaults,codepage=936,iocharset=cp936 0 0
在/etc/fstab文件里，第一列是挂载的文件系统的设备名，第二列是挂载点，第三列是挂载的文件系统类型，第四列是挂载的选项，选项间用逗号分隔。
在最后两行是我手工添加的windows下的C；D盘，加了codepage=936和iocharset=cp936参数以支持中文文件名。参数defaults实际上包含了一组默认参数：
rw 以可读写模式挂载
suid 开启用户ID和群组ID设置位
dev 可解读文件系统上的字符或区块设备
exec 可执行二进制文件
auto 自动挂载
nouser 使一般用户无法挂载
async 以非同步方式执行文件系统的输入输出操作
大家可以看到在这个列表里，光驱和软驱是不自动挂载的，参数设置为noauto。（如果你非要设成自动挂载，你要确保每次开机时你的光驱和软驱里都要有盘，呵呵。)
