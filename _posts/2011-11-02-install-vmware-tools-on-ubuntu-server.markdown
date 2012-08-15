---
layout: post
category : Linux
tags : [intro, beginner, Linux, Ubuntu, VMWare]
title: Install VMWare Tools on Ubuntu Server
wordpress_id: 1052
wordpress_url: http://www.im47.cn/?p=1052
date: 2011-11-02 13:01:06.000000000 +08:00
---
I don't often install Ubuntu server on a Virtual Machine (VM) so I've documented the process here. Usually you can just click "install VMWare tools" and VMWare will complete the process automatically.

<strong>Note:</strong> I have tested this on VMWare workstation 7.0 and Ubuntu 9.10 to 11.10 is also works on Lubuntu and Xubuntu. Once you have Ubuntu server installed run the following commands.
<pre>#Change to super user
sudo -i

#Update your sources
apt-get update

#Upgrade your installed packages and force kernel upgrade
apt-get dist-upgrade

###
#  Now reboot
###
reboot

#back to super user
sudo -i

#Update your kernel:
apt-get install linux-headers-server build-essential

###
#  Now you are ready to install VMWare tools.
###

#Mount the VMWare Tools CD ISO
mkdir /mnt/cdrom
mount /dev/cdrom /mnt/cdrom

#Copy VMware Tools
cp /mnt/cdrom/VmwareTools-x.x.x-xxxxx.tar.gz /tmp

#Go tmp
cd /tmp

#Extract
tar -zxf VmwareTools-x.x.x-xxxxx.tar.gz

#Change to extracted directory
cd vmware-tools-distrib

#Start the installer
./vmware-install.pl</pre>
The default settings have always worked for me (within a vm) so I hit enter a lot. Anyone know how to automatically choose all the default options for quicker install?
<h2>Using Shared folders</h2>
After a reboot you can use tools such as Shared folders. I like to sym link my shared folders to my home directory as I tend to forget the mount directory (hgfs) location.
<pre>#move to home dir
cd 

#sym link and name vmSharedFolder
ln -s /mnt/hgfs/ vmSharedFolder</pre>
<div id="reference">
<h2>Reference</h2>
Please note that if you use VMWare work station you can find all this in the help. When I wrote this I was using player, which at the time didn't include this in the help.
<ul>
	<li><a href="http://www.gorillapond.com/2006/07/31/install-vmware-tools-on-ubuntu/">Gorillapond</a></li>
	<li><a href="http://ubuntu-tutorials.com/2008/06/07/how-to-install-vmware-tools-on-ubuntu-804-guests/">Ubuntu tutorials</a></li>
	<li><a href="http://communities.vmware.com/thread/215289">VMWare forum</a></li>
</ul>
</div>
