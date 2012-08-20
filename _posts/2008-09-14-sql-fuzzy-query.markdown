---
layout: post
category : Database
tags : [Database, SQL, Query]
title: 有关SQL模糊查询
wordpress_id: 94
wordpress_url: http://www.im47cn.nt102.idcwind.net/wordpress/?p=94
date: 2008-09-14 14:36:13.000000000 +08:00
---
在进行数据库查询时，有完整查询和模糊查询之分。

一般模糊语句如下：
<div class="UBBPanel">
<div class="UBBTitle">程序代码</div>
<div class="UBBContent">Select 字段 FROM 表 Where 某字段 Like 条件</div>
</div>
其中关于条件，SQL提供了四种匹配模式：

1，%：表示任意0个或多个字符。可匹配任意类型和长度的字符，有些情况下若是中文，请使用两个百分号（%%）表示。

比如 Select * FROM [user] Where u_name LIKE '%三%'

将会把u_name为“张三”，“张猫三”、“三脚猫”，“唐三藏”等等有“三”的记录全找出来。

另外，如果需要找出u_name中既有“三”又有“猫”的记录，请使用and条件
<div class="UBBPanel">
<div class="UBBTitle">程序代码</div>
<div class="UBBContent">Select * FROM [user] Where u_name LIKE '%三%' AND u_name LIKE '%猫%'</div>
</div>
若使用 Select * FROM [user] Where u_name LIKE '%三%猫%'
虽然能搜索出“三脚猫”，但不能搜索出符合条件的“张猫三”。

2，_： 表示任意单个字符。匹配单个任意字符，它常用来限制表达式的字符长度语句：

比如 Select * FROM [user] Where u_name LIKE '_三_'
只找出“唐三藏”这样u_name为三个字且中间一个字是“三”的；

再比如 Select * FROM [user] Where u_name LIKE '三__';
只找出“三脚猫”这样name为三个字且第一个字是“三”的；

3，[ ]：表示括号内所列字符中的一个（类似正则表达式）。指定一个字符、字符串或范围，要求所匹配对象为它们中的任一个。

比如 Select * FROM [user] Where u_name LIKE '[张李王]三'
将找出“张三”、“李三”、“王三”（而不是“张李王三”）；

如 [ ] 内有一系列字符（01234、abcde之类的）则可略写为“0-4”、“a-e”
Select * FROM [user] Where u_name LIKE '老[1-9]'
将找出“老1”、“老2”、……、“老9”；

4，[^ ] ：表示不在括号所列之内的单个字符。其取值和 [] 相同，但它要求所匹配对象为指定字符以外的任一个字符。

比如 Select * FROM [user] Where u_name LIKE '[^张李王]三'
将找出不姓“张”、“李”、“王”的“赵三”、“孙三”等；

Select * FROM [user] Where u_name LIKE '老[^1-4]';
将排除“老1”到“老4”，寻找“老5”、“老6”、……

5，查询内容包含通配符时

由于通配符的缘故，导致我们查询特殊字符“%”、“_”、“[”的语句无法正常实现，而把特殊字符用“[ ]”括起便可正常查询。据此我们写出以下函数：

[code]function sqlencode(str)
str=replace(str,&quot;[&quot;,&quot;[[]&quot;) '此句一定要在最前
str=replace(str,&quot;_&quot;,&quot;[_]&quot;)
str=replace(str,&quot;%&quot;,&quot;[%]&quot;)
sqlencode=str
end function[/code]

在查询前将待查字符串先经该函数处理即可。
