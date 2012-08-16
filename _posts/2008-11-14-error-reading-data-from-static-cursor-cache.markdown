---
layout: post
category : Database
tags : [Database, beginner, SQL, error, help]
title: 莫名其妙的Error reading data from static cursor cache
wordpress_id: 113
wordpress_url: http://www.im47cn.nt102.idcwind.net/wordpress/?p=113
date: 2008-11-14 09:11:24.000000000 +08:00
---
<div>JDBC连接SQL执行select语句，当SQL SERVER表中有text类型字段，并且字段中的内容为空时，就会出现 &ldquo;Error reading data from static cursor cache&rdquo; 错误。<br /><br />解决方法 ：<br />1、保证text字段内容都不为空<br />2、如果一定有空的话，存放个空格字符</div>
