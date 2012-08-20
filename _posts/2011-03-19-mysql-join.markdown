---
layout: post
category : Database
tags : [Database, MySQL, SQL]
title: MySQL的连接/联结（Join）语法
wordpress_id: 881
wordpress_url: http://www.im47.net/?p=881
date: 2011-03-19 21:03:52.000000000 +08:00
---
<span style="font-size: x-small;"><strong>1</strong><strong>．内联结、外联结、左联结、右联结的含义及区别：</strong><strong> </strong></span>

<span style="font-size: x-small;"> </span>

<span style="font-size: x-small;">在讲MySQL的Join语法前还是先回顾一下联结的语法，国内关于MySQL联结查询的资料十分少，相信大家在看了本文后会对MySQL联结语法有相当清晰的了解，也不会被Oracle的外联结的（“＋”号）弄得糊涂了。</span>

<span style="font-size: x-small;"> </span>

<span style="font-size: x-small;">在SQL标准中规划的（Join）联结大致分为下面四种：</span>

<span style="font-size: x-small;">1．  内联结：将两个表中存在联结关系的字段符合联结关系的那些记录形成记录集的联结。</span>

<span style="font-size: x-small;">2．  外联结：分为外左联结和外右联结。</span>

<span style="font-size: x-small;">左联结A、B表的意思就是将表A中的全部记录和表B中联结的字段与表A的联结字段符合联结条件的那些记录形成的记录集的联结，这里注意的是最后出来的记录集会包括表A的全部记录。</span>

<span style="font-size: x-small;">右联结A、B表的结果和左联结B、A的结果是一样的，也就是说：</span>

<span style="font-size: x-small;">Select A.name B.name From A Left Join B On A.id=B.id</span>

<span style="font-size: x-small;">和Select A.name B.name From B Right Join A on B.id=A.id执行后的结果是一样的。</span>

<span style="font-size: x-small;">3．全联结：将两个表中存在联结关系的字段的所有记录取出形成记录集的联结（这个不需要记忆，只要是查询中提到了的表的字段都会取出，无论是否符合联结条件，因此意义不大）。</span>

<span style="font-size: x-small;">4．无联结：不用解释了吧，就是没有使用联结功能呗，也有自联结的说法。</span>

<span style="font-size: x-small;"> </span>

<span style="font-size: x-small;">这里我有个比较简便的记忆方法，内外联结的区别是内联结将去除所有不符合条件的记录，而外联结则保留其中部分。外左联结与外右联结的区别在于如果用A左联结B则A中所有记录都会保留在结果中，此时B中只有符合联结条件的记录，而右联结相反，这样也就不会混淆了。其实大家回忆高等教育出版社出版的《数据库系统概论》书中讲到关系代数那章（就是将笛卡儿积和投影那章）的内容，相信不难理解这些联结功能的内涵。</span>

<span style="font-size: x-small;"> </span>

<span style="font-size: x-small;"><strong>2． </strong><strong>MySQL</strong><strong>联结（</strong><strong>Join</strong><strong>）的语法</strong><strong> </strong></span>

<span style="font-size: x-small;"> </span>

<span style="font-size: x-small;">MySQL支持Select和某些Update和Delete情况下的Join语法，具体语法上的细节有：</span>

[code]
table_references:
    table_reference [, table_reference] …

table_reference:
    table_factor
  | join_table

table_factor:
    tbl_name [[AS] alias]
        [{USE|IGNORE|FORCE} INDEX (key_list)]
  | ( table_references )
  | { OJ table_reference LEFT OUTER JOIN table_reference
        ON conditional_expr }

join_table:
    table_reference [INNER | CROSS] JOIN table_factor [join_condition]
  | table_reference STRAIGHT_JOIN table_factor
  | table_reference STRAIGHT_JOIN table_factor ON condition
  | table_reference LEFT [OUTER] JOIN table_reference join_condition
  | table_reference NATURAL [LEFT [OUTER]] JOIN table_factor
  | table_reference RIGHT [OUTER] JOIN table_reference join_condition
  | table_reference NATURAL [RIGHT [OUTER]] JOIN table_factor

join_condition:
    ON conditional_expr | USING (column_list)
[/code]

<span style="font-size: x-small;">上面的用法摘自权威资料，不过大家看了是否有点晕呢？呵呵，应该问题主要还在于table_reference是什么，table_factor又是什么？这里的table_reference其实就是表的引用的意思，因为在MySQL看来，联结就是一种对表的引用，因此把需要联结的表定义为table_reference，同时在SQL Standard中也是如此看待的。而table_factor则是MySQL对这个引用的功能上的增强和扩充，使得引用的表可以是括号内的一系列表，如下面例子中的JOIN后面括号：</span>

[code]SELECT * FROM t1 LEFT JOIN (t2, t3, t4) ON (t2.a=t1.a AND t3.b=t1.b AND t4.c=t1.c)[/code]

<span style="font-size: x-small;">这个语句的执行结果和下面语句其实是一样的：</span>

[code]
SELECT * FROM t1 LEFT JOIN (t2 CROSS JOIN t3 CROSS JOIN t4)
                 ON (t2.a=t1.a AND t3.b=t1.b AND t4.c=t1.c)
[/code]

<span style="font-size: x-small;">这两个例子不仅让我们了解了MySQL中table_factor和table_reference含义，同时能理解一点CROSS JOIN的用法，我要补充的是在MySQL现有版本中CROSS JOIN的作用和INNER JOIN是一样的（虽然在SQL Standard中是不一样的，然而在MySQL中他们的区别仅仅是INNER JOIN需要附加ON参数的语句，而CROSS JOIN不需要）。</span>

<span style="font-size: x-small;">既然说到了ON语句，那就解释一下吧，ON语句其实和WHERE语句功能大致相当，只是这里的ON语句是专门针对联结表的，ON语句后面的条件的要求和书写方式和WHERE语句的要求是一样的，大家基本上可以把ON当作WHERE用。</span>

<span style="font-size: x-small;">大家也许也看到了OJ table_reference LEFT OUTER JOIN table_reference这个句子，这不是MySQL的标准写法，只是为了和ODBC的SQL语法兼容而设定的，我很少用，Java的人更是不会用，所以也不多解释了。</span>

<span style="font-size: x-small;">那下面就具体讲讲简单的JOIN的用法了。首先我们假设有2个表A和B，他们的表结构和字段分别为：</span>

<span style="font-size: x-small;"> </span>

<span style="font-size: x-small;">表A：</span>
<table border="1" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td width="67" valign="top"><span style="font-size: x-small;">ID</span></td>
<td width="96" valign="top"><span style="font-size: x-small;">Name</span></td>
</tr>
<tr>
<td width="67" valign="top"><span style="font-size: x-small;">1</span></td>
<td width="96" valign="top"><span style="font-size: x-small;">Tim</span></td>
</tr>
<tr>
<td width="67" valign="top"><span style="font-size: x-small;">2</span></td>
<td width="96" valign="top"><span style="font-size: x-small;">Jimmy</span></td>
</tr>
<tr>
<td width="67" valign="top"><span style="font-size: x-small;">3</span></td>
<td width="96" valign="top"><span style="font-size: x-small;">John</span></td>
</tr>
<tr>
<td width="67" valign="top"><span style="font-size: x-small;">4</span></td>
<td width="96" valign="top"><span style="font-size: x-small;">Tom</span></td>
</tr>
</tbody>
</table>
<span style="font-size: x-small;">表B：</span>
<table border="1" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td width="67" valign="top"><span style="font-size: x-small;">ID</span></td>
<td width="96" valign="top"><span style="font-size: x-small;">Hobby</span></td>
</tr>
<tr>
<td width="67" valign="top"><span style="font-size: x-small;">1</span></td>
<td width="96" valign="top"><span style="font-size: x-small;">Football</span></td>
</tr>
<tr>
<td width="67" valign="top"><span style="font-size: x-small;">2</span></td>
<td width="96" valign="top"><span style="font-size: x-small;">Basketball</span></td>
</tr>
<tr>
<td width="67" valign="top"><span style="font-size: x-small;">2</span></td>
<td width="96" valign="top"><span style="font-size: x-small;">Tennis</span></td>
</tr>
<tr>
<td width="67" valign="top"><span style="font-size: x-small;">4</span></td>
<td width="96" valign="top"><span style="font-size: x-small;">Soccer</span></td>
</tr>
</tbody>
</table>
<span style="font-size: x-small;"> </span>

<span style="font-size: x-small;">1．  内联结：</span>

<span style="font-size: x-small;">Select A.Name B.Hobby from A, B where A.id = B.id，这是隐式的内联结，查询的结果是：</span>
<table border="1" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td width="67" valign="top"><span style="font-size: x-small;">Name</span></td>
<td width="96" valign="top"><span style="font-size: x-small;">Hobby</span></td>
</tr>
<tr>
<td width="67" valign="top"><span style="font-size: x-small;">Tim</span></td>
<td width="96" valign="top"><span style="font-size: x-small;">Football</span></td>
</tr>
<tr>
<td width="67" valign="top"><span style="font-size: x-small;">Jimmy</span></td>
<td width="96" valign="top"><span style="font-size: x-small;">Basketball</span></td>
</tr>
<tr>
<td width="67" valign="top"><span style="font-size: x-small;">Jimmy</span></td>
<td width="96" valign="top"><span style="font-size: x-small;">Tennis</span></td>
</tr>
<tr>
<td width="67" valign="top"><span style="font-size: x-small;">Tom</span></td>
<td width="96" valign="top"><span style="font-size: x-small;">Soccer</span></td>
</tr>
</tbody>
</table>
<span style="font-size: x-small;">它的作用和 Select A.Name from A INNER JOIN B ON A.id = B.id是一样的。这里的INNER JOIN换成CROSS JOIN也是可以的。</span>

<span style="font-size: x-small;">2．  外左联结</span>

<span style="font-size: x-small;">Select A.Name from A Left JOIN B ON A.id = B.id，典型的外左联结，这样查询得到的结果将会是保留所有A表中联结字段的记录，若无与其相对应的B表中的字段记录则留空，结果如下：</span>
<table border="1" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td width="67" valign="top"><span style="font-size: x-small;">Name</span></td>
<td width="120" valign="top"><span style="font-size: x-small;">Hobby</span></td>
</tr>
<tr>
<td width="67" valign="top"><span style="font-size: x-small;">Tim</span></td>
<td width="120" valign="top"><span style="font-size: x-small;">Football</span></td>
</tr>
<tr>
<td width="67" valign="top"><span style="font-size: x-small;">Jimmy</span></td>
<td width="120" valign="top"><span style="font-size: x-small;">Basketball，Tennis</span></td>
</tr>
<tr>
<td width="67" valign="top"><span style="font-size: x-small;">John</span></td>
<td width="120" valign="top"><span style="font-size: x-small;"> </span></td>
</tr>
<tr>
<td width="67" valign="top"><span style="font-size: x-small;">Tom</span></td>
<td width="120" valign="top"><span style="font-size: x-small;">Soccer</span></td>
</tr>
</tbody>
</table>
<span style="font-size: x-small;">所以从上面结果看出，因为A表中的John记录的ID没有在B表中有对应ID，因此为空，但Name栏仍有John记录。</span>

<span style="font-size: x-small;">3．  外右联结</span>

<span style="font-size: x-small;">如果把上面查询改成外右联结：Select A.Name from A Right JOIN B ON A.id = B.id，则结果将会是：</span>
<table border="1" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td width="67" valign="top"><span style="font-size: x-small;">Name</span></td>
<td width="84" valign="top"><span style="font-size: x-small;">Hobby</span></td>
</tr>
<tr>
<td width="67" valign="top"><span style="font-size: x-small;">Tim</span></td>
<td width="84" valign="top"><span style="font-size: x-small;">Football</span></td>
</tr>
<tr>
<td width="67" valign="top"><span style="font-size: x-small;">Jimmy</span></td>
<td width="84" valign="top"><span style="font-size: x-small;">Basketball</span></td>
</tr>
<tr>
<td width="67" valign="top"><span style="font-size: x-small;">Jimmy</span></td>
<td width="84" valign="top"><span style="font-size: x-small;">Tennis</span></td>
</tr>
<tr>
<td width="67" valign="top"><span style="font-size: x-small;">Tom</span></td>
<td width="84" valign="top"><span style="font-size: x-small;">Soccer</span></td>
</tr>
</tbody>
</table>
<span style="font-size: x-small;">这样的结果都是我们可以从外左联结的结果中猜到的了。</span>

<span style="font-size: x-small;">说到这里大家是否对联结查询了解多了？这个原本看来高深的概念一下子就理解了，恍然大悟了吧（呵呵，开玩笑了）？最后给大家讲讲MySQL联结查询中的某些参数的作用：</span>

<span style="font-size: x-small;"> </span>

<span style="font-size: x-small;">1．USING (column_list)：其作用是为了方便书写联结的多对应关系，大部分情况下USING语句可以用ON语句来代替，如下面例子：</span>

<span style="font-size: x-small;"> </span>

<span style="font-size: x-small;">a LEFT JOIN b USING (c1,c2,c3)，其作用相当于下面语句</span>

<span style="font-size: x-small;">a LEFT JOIN b ON a.c1=b.c1 AND a.c2=b.c2 AND a.c3=b.c3</span>

<span style="font-size: x-small;"> </span>

<span style="font-size: x-small;">只是用ON来代替会书写比较麻烦而已。</span>

<span style="font-size: x-small;"> </span>

<span style="font-size: x-small;">2．NATURAL [LEFT] JOIN：这个句子的作用相当于INNER JOIN，或者是在USING子句中包含了联结的表中所有字段的Left JOIN（左联结）。</span>

<span style="font-size: x-small;"> </span>

<span style="font-size: x-small;">3．STRAIGHT_JOIN：由于默认情况下MySQL在进行表的联结的时候会先读入左表，当使用了这个参数后MySQL将会先读入右表，这是个MySQL的内置优化参数，大家应该在特定情况下使用，譬如已经确认右表中的记录数量少，在筛选后能大大提高查询速度。</span>

<span style="font-size: x-small;"> </span>

<span style="font-size: x-small;">最后要说的就是，在MySQL5.0以后，运算顺序得到了重视，所以对多表的联结查询可能会错误以子联结查询的方式进行。譬如你需要进行多表联结，因此你输入了下面的联结查询：</span>

[code]
SELECT t1.id,t2.id,t3.id
    FROM t1,t2
    LEFT JOIN t3 ON (t3.id=t1.id)
    WHERE t1.id=t2.id;
[/code]

<span style="font-size: x-small;">但是MySQL并不是这样执行的，其后台的真正执行方式是下面的语句：</span>

[code]
SELECT t1.id,t2.id,t3.id
    FROM t1,(  t2 LEFT JOIN t3 ON (t3.id=t1.id)  )
    WHERE t1.id=t2.id;
[/code]

<span style="font-size: x-small;">这并不是我们想要的效果，所以我们需要这样输入：</span>

[code]
SELECT t1.id,t2.id,t3.id
    FROM (t1,t2)
    LEFT JOIN t3 ON (t3.id=t1.id)
    WHERE t1.id=t2.id;
[/code]

<span style="font-size: x-small;">在这里括号是相当重要的，因此以后在写这样的查询的时候我们不要忘记了多写几个括号，至少这样能避免很多错误（因为这样的错误是很难被开发人员发现的）。</span>

<span style="font-size: xx-small;">转载：<a href="http://www.blogjava.net/chenpengyi/archive/2005/10/17/15747.html">http://www.blogjava.net/chenpengyi/archive/2005/10/17/15747.html</a></span>
