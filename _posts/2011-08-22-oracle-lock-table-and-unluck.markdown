---
layout: post
category : Database
tags : [intro, beginner, Database, Oracle, luck table]
title: Oracle锁表与解表
wordpress_id: 1003
wordpress_url: http://www.im47.cn/?p=1003
date: 2011-08-22 13:00:28.000000000 +08:00
---
这里不教授如何锁表，大家自己摸索~

下面讲如何解表：

有关视图
v$session        查询会话的信息
v$session_wait   查询等待的会话信息
v$lock           列出系统中的所有锁
dba_locks        对v$lock的格式化视图
v$locked_object  只包含DML的锁信息,包括回滚段和会话信息

任何DML语句其实产生了两个锁，一个是表锁，一个是行锁。

如果发生了锁等待，我们可能更想知道是谁锁了表而引起谁的等待。以下的语句可以查询到谁锁了表，而谁在等待。
<pre>SELECT /*+ rule */ s.username, 
decode(l.type,'TM','TABLE LOCK', 
'TX','ROW LOCK', 
NULL) LOCK_LEVEL, 
o.owner,o.object_name,o.object_type, 
s.sid,s.serial#,s.terminal,s.machine,s.program,s.osuser 
FROM v$session s,v$lock l,dba_objects o 
WHERE l.sid = s.sid 
AND l.id1 = o.object_id(+) 
AND s.username is NOT NULL</pre>
<pre>SELECT /*+ rule */ lpad(' ',decode(l.xidusn ,0,3,0))||l.oracle_username User_name, o.owner,o.object_name,o.object_type,s.sid,s.serial# 
FROM v$locked_object l,dba_objects o,v$session s 
WHERE l.object_id=o.object_id 
AND l.session_id=s.sid 
ORDER BY o.object_id,xidusn DESC</pre>
以上查询结果是一个树状结构，如果有子节点，则表示有等待发生。

如果想知道锁用了哪个回滚段，还可以关联到V$rollname，其中xidusn就是回滚段的USN

杀锁命令
<pre>alter system kill session 'sid,serial#'</pre>

<address><em>更多知识请参考：</em>
<em> <a href="http://space.itpub.net/13636837/viewspace-624458" target="_blank">http://space.itpub.net/13636837/viewspace-624458</a></em></address>
