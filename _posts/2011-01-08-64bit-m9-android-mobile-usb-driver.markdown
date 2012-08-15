---
layout: post
category : Android
tags : [intro, beginner, Android, how-to, question]
title: 64位系统连接M9等Android手机的USB驱动修改方法
wordpress_id: 786
wordpress_url: http://www.im47.net/?p=786
date: 2011-01-08 15:34:46.000000000 +08:00
---
<img class="alignleft" title="Android" src="http://pic.yupoo.com/dreambt/ALJrHejc/HRnBP.jpg" alt="" width="150" height="328" />

64位系统连接M9等Android手机时，会提示无法识别手机。那么下面就来介绍一下如何修改USB驱动：

___________驱动提取过程_____________

1.安装最新的32位JDK（只能装32位，因为Android SDK只认32位）
下载地址：<a href="http://www.oracle.com/technetwork/java/javase/downloads/index.html" target="_blank">http://www.oracle.com/technetwork/java/javase/downloads/index.html</a>
2.安装Android SDK
下载地址：<a href="http://developer.android.com/sdk/" target="_blank">http://developer.android.com/sdk/</a>或者<a href="http://dl.google.com/android/installer_r08-windows.exe" target="_blank">http://dl.google.com/android/installer_r08-windows.exe</a>
3.打开“SDK Manager”，依次展开“Available packages”-&gt;“Third party Add-ons”-&gt;“Google Inc. add-ons(dl-ssl.google.com)”
4.勾上“Google Usb Driver package,revision 4”，然后点右下角“Install Selected”
5.根据屏幕提示完成下载和安装。
6.在安装路径下（64位系统默认C:\Program Files (x86)\Android\android-sdk-windows\google-usb_driver）找到驱动。
7.编辑android_winusb.inf文件，分别在32位和64位部分加入M9的标记：

[code]
[Google.NTx86]
;M9
%SingleAdbInterface%        = USB_Install, USB\VID_18D1&amp;PID_0005
%CompositeAdbInterface%     = USB_Install, USB\VID_18D1&amp;PID_0005&amp;MI_01
[Google.NTamd64]
;M9
%SingleAdbInterface%        = USB_Install, USB\VID_18D1&amp;PID_0005
%CompositeAdbInterface%     = USB_Install, USB\VID_18D1&amp;PID_0005&amp;MI_01
[/code]

8、最后将google-usb_driver整目录拷出来即可。
