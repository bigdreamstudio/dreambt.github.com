---
layout: post
category : Database
tags : [Database, beginner, varchar, char]
title: char与varchar数据类型转化的奇怪问题
wordpress_id: 117
wordpress_url: http://www.im47cn.nt102.idcwind.net/wordpress/?p=117
date: 2008-11-21 09:45:02.000000000 +08:00
---
今天写jsp留言板时遇到一个奇怪的问题，其实以前也遇见过。当时只是很郁闷，现在我仔细研究了一下。先分享如下：
<div class="codeText">
<div class="codeHead">jsp代码</div>
<ol class="dp-j">
	<li class="alt"><span><span>PassWord=request.getParameter(</span><span class="string">"psw"</span><span>); </span></span></li>
	<li><span> </span></li>
	<li class="alt"><span> sql=</span><span class="string">"select Upws from [User] where Uname='"</span><span>+UserName+</span><span class="string">"'"</span><span>; </span></li>
	<li><span> rs=stmt.executeQuery(sql); </span></li>
	<li class="alt"><span> rs.next(); </span></li>
	<li><span> PassWord3=rs.getString(</span><span class="string">"Upws"</span><span>); </span></li>
	<li class="alt"><span class="keyword">if</span><span> (PassWord.equals(PassWord3)</span><span>) { </span></li>
	<li><span> session.setAttribute(</span><span class="string">"ADMIN"</span><span>,</span><span class="string">"enable"</span><span>); </span></li>
	<li class="alt"><span>} </span></li>
	<li><span class="keyword">else</span><span> { </span></li>
	<li class="alt"><span> session.setAttribute(</span><span class="string">"ADMIN"</span><span>,</span><span class="string">"disable"</span><span>); </span></li>
	<li><span>} </span></li>
</ol>
</div>
当数据库中的Upws为char型时，可能会出现多一个或多个空格的问题，导致登录失败。

因此，我们应该优先考虑使用varchar型数据。
