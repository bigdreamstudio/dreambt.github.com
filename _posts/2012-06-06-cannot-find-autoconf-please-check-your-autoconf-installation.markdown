---
layout: post
category : PHP
tags : [PHP, question, phpize, error]
title: 运行phpize时出现：Cannot find autoconf. Please check your autoconf installation
wordpress_id: 1129
wordpress_url: http://www.im47.cn/?p=1129
date: 2012-06-06 18:29:50.000000000 +08:00
---
运行/usr/local/php/bin/phpize时出现 Cannot find autoconf. Please check your autoconf installation：

Configuring for:
PHP Api Version:         20041225
Zend Module Api No:      20060613
Zend Extension Api No:   220060519
Cannot find autoconf. Please check your autoconf installation and the
$PHP_AUTOCONF environment variable. Then, rerun this script.

根据网上的解决办法是：
<pre>cd /usr/src
wget http://ftp.gnu.org/gnu/m4/m4-1.4.16.tar.gz
tar -zvxf m4-1.4.16.tar.gz
cd m4-1.4.16/
./configure &amp;&amp; make &amp;&amp; make install
cd ../
wget http://ftp.gnu.org/gnu/autoconf/autoconf-2.69.tar.gz
tar -zvxf autoconf-2.69.tar.gz
cd autoconf-2.69/
./configure &amp;&amp; make &amp;&amp; make install</pre>
