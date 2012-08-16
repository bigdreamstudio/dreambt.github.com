---
layout: post
category : Linux
tags : [intro, Linux, Unix, timestamp]
title: 关于Unix时间戳(Unix timestamp)
wordpress_id: 901
wordpress_url: http://www.im47.net/?p=901
date: 2011-04-11 16:04:58.000000000 +08:00
---
时间戳是自 1970 年 1 月 1 日（00:00:00 GMT）以来的秒数，它也被称为 Unix 时间戳（Unix Timestamp）。

Unix时间戳(Unix timestamp)，或称Unix时间(Unix time)、POSIX时间(POSIX time)，是一种时间表示方式，定义为从格林威治时间1970年01月01日00时00分00秒起至现在的总秒数。Unix时间戳不仅被使用在Unix系统、类Unix系统中，也在许多其他操作系统中被广泛采用。
<h3>如何在不同编程语言中获取现在的Unix时间戳(Unix timestamp)？</h3>
<table>
<tbody>
<tr>
<td width="200">Java</td>
<td>time</td>
</tr>
<tr>
<td>JavaScript</td>
<td>Math.round(new Date().getTime()/1000)
getTime()返回数值的单位是毫秒</td>
</tr>
<tr>
<td>Microsoft .NET / C#</td>
<td>epoch = (DateTime.Now.ToUniversalTime().Ticks - 621355968000000000) / 10000000</td>
</tr>
<tr>
<td>MySQL</td>
<td>SELECT unix_timestamp(now())</td>
</tr>
<tr>
<td>Perl</td>
<td>time</td>
</tr>
<tr>
<td>PHP</td>
<td>time()</td>
</tr>
<tr>
<td>PostgreSQL</td>
<td>SELECT extract(epoch FROM now())</td>
</tr>
<tr>
<td>Python</td>
<td>先 import time 然后 time.time()</td>
</tr>
<tr>
<td>Ruby</td>
<td>获取Unix时间戳：Time.now 或 Time.new
显示Unix时间戳：Time.now.to_i</td>
</tr>
<tr>
<td>SQL Server</td>
<td>SELECT DATEDIFF(s, '1970-01-01 00:00:00', GETUTCDATE())</td>
</tr>
<tr>
<td>Unix / Linux</td>
<td>date +%s</td>
</tr>
<tr>
<td>VBScript / ASP</td>
<td>DateDiff("s", "01/01/1970 00:00:00", Now())</td>
</tr>
<tr>
<td>其他操作系统
(如果Perl被安装在系统中)</td>
<td>命令行状态：perl -e "print time"</td>
</tr>
</tbody>
</table>
<h3>如何在不同编程语言中实现Unix时间戳(Unix timestamp) → 普通时间？</h3>
<table>
<tbody>
<tr>
<td width="200">Java</td>
<td>String date = new java.text.SimpleDateFormat("dd/MM/yyyy HH:mm:ss").format(new java.util.Date(Unix timestamp * 1000))</td>
</tr>
<tr>
<td>JavaScript</td>
<td>先 var unixTimestamp = new Date(Unix timestamp * 1000) 然后 commonTime = unixTimestamp.toLocaleString()</td>
</tr>
<tr>
<td>Linux</td>
<td>date -d @Unix timestamp</td>
</tr>
<tr>
<td>MySQL</td>
<td>from_unixtime(Unix timestamp)</td>
</tr>
<tr>
<td>Perl</td>
<td>先 my $time = Unix timestamp 然后 my ($sec, $min, $hour, $day, $month, $year) = (localtime($time))[0,1,2,3,4,5,6]</td>
</tr>
<tr>
<td>PHP</td>
<td>date('r', Unix timestamp)</td>
</tr>
<tr>
<td>PostgreSQL</td>
<td>SELECT TIMESTAMP WITH TIME ZONE 'epoch' + Unix timestamp) * INTERVAL '1 second';</td>
</tr>
<tr>
<td>Python</td>
<td>先 import time 然后 time.gmtime(Unix timestamp)</td>
</tr>
<tr>
<td>Ruby</td>
<td>Time.at(Unix timestamp)</td>
</tr>
<tr>
<td>SQL Server</td>
<td>DATEADD(s, Unix timestamp, '1970-01-01 00:00:00')</td>
</tr>
<tr>
<td>VBScript / ASP</td>
<td>DateAdd("s", Unix timestamp, "01/01/1970 00:00:00")</td>
</tr>
<tr>
<td>其他操作系统
(如果Perl被安装在系统中)</td>
<td>命令行状态：perl -e "print scalar(localtime(Unix timestamp))"</td>
</tr>
</tbody>
</table>
<h3>如何在不同编程语言中实现普通时间 → Unix时间戳(Unix timestamp)？</h3>
<table>
<tbody>
<tr>
<td width="200">Java</td>
<td>long epoch = new java.text.SimpleDateFormat("dd/MM/yyyy HH:mm:ss").parse("01/01/1970 01:00:00");</td>
</tr>
<tr>
<td>JavaScript</td>
<td>var commonTime = new Date(Date.UTC(year, month - 1, day, hour, minute, second))</td>
</tr>
<tr>
<td>MySQL</td>
<td>SELECT unix_timestamp(time)
时间格式: YYYY-MM-DD HH:MM:SS 或 YYMMDD 或 YYYYMMDD</td>
</tr>
<tr>
<td>Perl</td>
<td>先 use Time::Local 然后 my $time = timelocal($sec, $min, $hour, $day, $month, $year);</td>
</tr>
<tr>
<td>PHP</td>
<td>mktime(hour, minute, second, day, month, year)</td>
</tr>
<tr>
<td>PostgreSQL</td>
<td>SELECT extract(epoch FROM date('YYYY-MM-DD HH:MM:SS'));</td>
</tr>
<tr>
<td>Python</td>
<td>先 import time 然后 int(time.mktime(time.strptime('YYYY-MM-DD HH:MM:SS', '%Y-%m-%d %H:%M:%S')))</td>
</tr>
<tr>
<td>Ruby</td>
<td>Time.local(year, month, day, hour, minute, second)</td>
</tr>
<tr>
<td>SQL Server</td>
<td>SELECT DATEDIFF(s, '1970-01-01 00:00:00', time)</td>
</tr>
<tr>
<td>Unix / Linux</td>
<td>date +%s -d"Jan 1, 1970 00:00:01"</td>
</tr>
<tr>
<td>VBScript / ASP</td>
<td>DateDiff("s", "01/01/1970 00:00:00", time)</td>
</tr>
</tbody>
</table>
