---
layout: post
category : Version_Control
tags : [Apache, SVN, tutorial, how-to]
title: 如何让现有的Apache服务器支持SVN
wordpress_id: 886
wordpress_url: http://www.im47.net/?p=886
date: 2011-03-28 20:34:07.000000000 +08:00
---
如果我们的服务器已经安装有Apache，那么如何可以改造一下使之支持SVN呢？

首先下载Subversion和Apache，可以到相关的网站下载

SVN: <a href="http://subversion.tigris.org/">http://subversion.tigris.org/</a>

Apache: <a href="http://httpd.apache.org/">http://httpd.apache.org/</a>

安装Apache和SVN不是本文讲解的重点，主要关心<strong>两者配置问题</strong>：

1. 首先将Subversion的bin\目录下面的两个文件：mod_authz_svn.so和mod_dav_svn.so复制到Apache安装目录modules\目录下； 再将Subversion安装目录bin\下面的所有.dll文件和svnadmin.exe复制到Apache安装目录bin\目录下（注意：不要替换任何文件！！！）。

2.配置Apache下conf目录里的httpd.conf，找到下面两行，并且去掉‘#’（注释的作用）.

#LoadModule dav_module modules/mod_dav.so
#LoadModule dav_fs_module modules/mod_dav_fs.so

然后需要添加刚才拷贝的那两个文件mod_authz_svn.so和mod_dav_svn.so。方法如下：

在所有LoadModule语句的最后添加两行：
LoadModule dav_svn_module modules/mod_dav_svn.so
LoadModule authz_svn_module modules/mod_authz_svn.so

此时，SVN基本上已经添加到Apache。

<strong>SVN的配置</strong>

1.配置SVN，首先需要创建一个文件夹来保存文件，如d:\SVN_Repository.

打开cmd到c:\Program Files\Apache2.2\bin&gt;svnadmin create d:\SVN_Repository\Porject (Porjcet文件夹就是用来保存文件的目录)

2.创建保存用户密码的文件，svn_auth_passwd。

打开cmd到c:\Program Files\Apache2.2\bin&gt;htpasswd -c d:\SVN_Repository\Porject\svn_auth_passwd XXX

这时会提示你输入新的密码，按照指示做就会生成svn_auth_passwd文件在d:\SVN_Repository\Porject\目录下。

创建密码文件的时候需要用-c，但是如果要追加用户需要用-m，其余命令可以看help。

例如要追加用户XXX1，则c:\Program Files\Apache2.2\bin&gt;htpasswd -m d:\SVN_Repository\Porject\svn_auth_passwd XXX1.

创建完密码之后还需要在httpd.conf配置认证信息，配置完之后，访问SVN才会有让你输入密码的提示。

3.在httpd.conf文件里配置文件的存放路径，SVNParentPath指令来指定存放所有项目的路径。
&lt;Location /svn&gt;
DAV svn
SVNParentPath d:/Svn_Repository
AuthType Basic#认证类型
AuthName "Subversion repositories"
AuthUserFile "d:/Svn_Repository/svn_auth_passwd"#存储用户登录的信息。
Require valid-user
&lt;/Location&gt;

这样配置完之后运行Apache，就可以访问SVN了。
