---
layout: post
category : Ruby
tags : [intro, beginner, Ruby]
title: Ruby中常用的全局变量
wordpress_id: 1035
wordpress_url: http://www.im47.cn/?p=1035
date: 2011-10-12 19:10:47.000000000 +08:00
---
<pre>$! #最近一次的错误信息
$@ #错误产生的位置
$_ #gets最近读的字符串
$. #解释器最近读的行数(line number)
$&amp; #最近一次与正则表达式匹配的字符串
$~ #作为子表达式组的最近一次匹配
$n #最近匹配的第n个子表达式(和$~[n]一样)
$= #是否区别大小写的标志
$/ #输入记录分隔符
$\ #输出记录分隔符
$0 #Ruby脚本的文件名
$* #命令行参数
$$ #解释器进程ID
$? #最近一次执行的子进程退出状态</pre>
