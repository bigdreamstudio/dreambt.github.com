---
layout: post
category : J2EE
tags : [J2EE, Struts2]
title: 浅谈s:form标签中action属性为XX.action与XX的区别
wordpress_id: 170
wordpress_url: http://www.im47cn.nt102.idcwind.net/wordpress/?p=170
date: 2009-07-11 19:05:32.000000000 +08:00
---
先来看一个实例struts.xml的内容：
<div class="codeText">
<div class="codeHead">XML代码</div>
<ol class="dp-xml">
	<li class="alt"><span><span class="tag">&lt;</span><span class="tag-name">package</span><span class="attribute">name</span><span>=</span><span class="attribute-value">"home"</span><span class="attribute">extends</span><span>=</span><span class="attribute-value">"demo"</span><span class="attribute">namespace</span><span>=</span><span class="attribute-value">"/demo/home"</span><span class="tag">&gt;</span></span></li>
	<li><span> <span class="tag">&lt;</span><span class="tag-name">action</span><span class="attribute">name</span><span>=</span><span class="attribute-value">"home"</span><span class="attribute">class</span><span>=</span><span class="attribute-value">"com.demo.home.HomeAction"</span><span class="tag">&gt;</span></span></li>
	<li class="alt"><span> <span class="tag">&lt;</span><span class="tag-name">result</span><span class="attribute">name</span><span>=</span><span class="attribute-value">"login"</span><span class="attribute">type</span><span>=</span><span class="attribute-value">"redirect-action"</span><span class="tag">&gt;</span></span></li>
	<li><span> <span class="tag">&lt;</span><span class="tag-name">param</span><span class="attribute">name</span><span>=</span><span class="attribute-value">"namespace"</span><span class="tag">&gt;</span><span>/demo/login </span><span class="tag">&lt;/</span><span class="tag-name">param</span><span class="tag">&gt;</span></span></li>
	<li class="alt"><span> <span class="tag">&lt;</span><span class="tag-name">param</span><span class="attribute">name</span><span>=</span><span class="attribute-value">"actionName"</span><span class="tag">&gt;</span><span>login!init.action </span><span class="tag">&lt;/</span><span class="tag-name">param</span><span class="tag">&gt;</span></span></li>
	<li><span> <span class="tag">&lt;/</span><span class="tag-name">result</span><span class="tag">&gt;</span></span></li>
	<li class="alt"></li>
	<li><span> <span class="tag">&lt;</span><span class="tag-name">result</span><span class="attribute">name</span><span>=</span><span class="attribute-value">"userManage"</span><span class="attribute">type</span><span>=</span><span class="attribute-value">"redirect-action"</span><span class="tag">&gt;</span></span></li>
	<li class="alt"><span> <span class="tag">&lt;</span><span class="tag-name">param</span><span class="attribute">name</span><span>=</span><span class="attribute-value">"namespace"</span><span class="tag">&gt;</span><span>/demo/userManage </span><span class="tag">&lt;/</span><span class="tag-name">param</span><span class="tag">&gt;</span></span></li>
	<li><span> <span class="tag">&lt;</span><span class="tag-name">param</span><span class="attribute">name</span><span>=</span><span class="attribute-value">"actionName"</span><span class="tag">&gt;</span><span>userManage!init.action </span><span class="tag">&lt;/</span><span class="tag-name">param</span><span class="tag">&gt;</span></span></li>
	<li class="alt"><span> <span class="tag">&lt;/</span><span class="tag-name">result</span><span class="tag">&gt;</span></span></li>
	<li><span> <span class="tag">&lt;/</span><span class="tag-name">action</span><span class="tag">&gt;</span></span></li>
	<li class="alt"><span><span class="tag">&lt;/</span><span class="tag-name">package</span><span class="tag">&gt;</span></span></li>
	<li></li>
	<li class="alt"><span><span class="tag">&lt;</span><span class="tag-name">package</span><span class="attribute">name</span><span>=</span><span class="attribute-value">"userManage"</span><span class="attribute">extends</span><span>=</span><span class="attribute-value">"demo"</span><span class="attribute">namespace</span><span>=</span><span class="attribute-value">"/demo/userManage"</span><span class="tag">&gt;</span></span></li>
	<li><span> <span class="tag">&lt;</span><span class="tag-name">action</span><span class="attribute">name</span><span>=</span><span class="attribute-value">"userManage"</span><span class="attribute">class</span><span>=</span><span class="attribute-value">"com.demo.userManage.UserManageAction"</span><span class="tag">&gt;</span></span></li>
	<li class="alt"><span> <span class="tag">&lt;</span><span class="tag-name">result</span><span class="attribute">name</span><span>=</span><span class="attribute-value">"login"</span><span class="attribute">type</span><span>=</span><span class="attribute-value">"redirect"</span><span class="tag">&gt;</span><span>/login/login.jsp </span><span class="tag">&lt;/</span><span class="tag-name">result</span><span class="tag">&gt;</span></span></li>
	<li><span> <span class="tag">&lt;/</span><span class="tag-name">action</span><span class="tag">&gt;</span></span></li>
	<li class="alt"><span><span class="tag">&lt;/</span><span class="tag-name">package</span><span class="tag">&gt;</span></span></li>
	<li></li>
	<li class="alt"><span><span class="tag">&lt;</span><span class="tag-name">package</span><span class="attribute">name</span><span>=</span><span class="attribute-value">"login"</span><span class="attribute">extends</span><span>=</span><span class="attribute-value">"demo"</span><span class="attribute">namespace</span><span>=</span><span class="attribute-value">"/demo/login"</span><span class="tag">&gt;</span></span></li>
	<li><span> <span class="tag">&lt;</span><span class="tag-name">action</span><span class="attribute">name</span><span>=</span><span class="attribute-value">"login"</span><span class="attribute">class</span><span>=</span><span class="attribute-value">"com.demo.login.LoginAction"</span><span class="tag">&gt;</span></span></li>
	<li class="alt"><span> <span class="tag">&lt;</span><span class="tag-name">result</span><span class="attribute">name</span><span>=</span><span class="attribute-value">"input"</span><span class="tag">&gt;</span><span>/login/login.jsp </span><span class="tag">&lt;/</span><span class="tag-name">result</span><span class="tag">&gt;</span></span></li>
	<li><span> <span class="tag">&lt;/</span><span class="tag-name">action</span><span class="tag">&gt;</span></span></li>
	<li class="alt"><span><span class="tag">&lt;/</span><span class="tag-name">package</span><span class="tag">&gt;</span></span></li>
</ol>
</div>
继续看 login.jsp 的内容：
<div class="codeText">
<div class="codeHead">JSP代码</div>
<ol class="dp-j">
	<li class="alt"><span><span>&lt;s:form action=</span><span class="string">"login.action"</span><span> method=</span><span class="string">"post"</span><span> namespace=</span><span class="string">"/demo/login"</span><span>&gt; </span></span></li>
	<li><span> &lt;table width=<span class="string">"100%"</span><span> align=</span><span class="string">"center"</span><span>&gt; </span></span></li>
	<li class="alt"><span>&lt;tr&gt;&lt;td&gt;name : &lt;s:textfield name=<span class="string">"loginForm.name"</span><span>/&gt;&lt;/td&gt;&lt;/tr&gt; </span></span></li>
	<li><span>&lt;tr&gt;&lt;td&gt;password : &lt;s:password name=<span class="string">"loginForm.password"</span><span>/&gt;&lt;/td&gt;&lt;/tr&gt; </span></span></li>
	<li class="alt"><span>&lt;tr&gt;&lt;td&gt;&lt;s:submit name=<span class="string">"login"</span><span> value=</span><span class="string">"submit"</span><span> method=</span><span class="string">"login"</span><span>/&gt;&lt;/td&gt;&lt;/tr&gt; </span></span></li>
	<li><span> &lt;/table&gt; </span></li>
	<li class="alt"><span>&lt;/s:form&gt; </span></li>
</ol>
</div>
已知问题：

从home进入login.jsp页面时，点击"submit"按钮，程序执行正常。
但是从userManage重定向到login.jsp页面时，点击"submit"按钮，出现下面的异常：
<span style="color: #ff0000;">javax.servlet.ServletException: java.lang.NoSuchMethodException: com.opensymphony.xwork2.ActionSupport.login()
org.apache.struts2.dispatcher.Dispatcher.serviceAction(Dispatcher.java:515)
org.apache.struts2.dispatcher.FilterDispatcher.doFilter(FilterDispatcher.java:419) </span>

然后我把login.jsp的 &lt;s:form action="<strong>login.action</strong>" method="post" namespace="/demo/login"&gt;
改成 &lt;s:form action="<strong>login</strong>" method="post" namespace="/demo/login"&gt; 就能正常执行了。

很多网友都在讨论 .action 到底应该又还是不应该有，那么我们先看看下面的分析吧

jsp页面带 .action 时：
<div class="codeText">
<div class="codeHead">XML代码</div>
<ol class="dp-xml">
	<li class="alt"><span><span class="tag">&lt;</span><span class="tag-name">s:form</span><span class="attribute">id</span><span>=</span><span class="attribute-value">"testForm"</span><span class="attribute">action</span><span>=</span><span class="attribute-value">"add.action"</span><span class="attribute">namespace</span><span>=</span><span class="attribute-value">"/user"</span><span class="tag">&gt;</span><span>
</span></span></li>
</ol>
</div>
查看HTML原代码：
<div class="codeText">
<div class="codeHead">HTML代码</div>
<ol class="dp-xml">
	<li class="alt"><span><span class="tag">&lt;</span><span class="tag-name">form</span><span class="attribute">id</span><span>=</span><span class="attribute-value">"testForm"</span><span class="attribute">action</span><span>=</span><span class="attribute-value">"add.action"</span><span class="attribute">method</span><span>=</span><span class="attribute-value">"post"</span><span class="tag">&gt;</span><span>
</span></span></li>
</ol>
</div>
jsp页面不带 .action 时：
<div class="codeText">
<div class="codeHead">XML代码</div>
<ol class="dp-xml">
	<li class="alt"><span><span class="tag">&lt;</span><span class="tag-name">s:form</span><span class="attribute">id</span><span>=</span><span class="attribute-value">"testForm"</span><span class="attribute">action</span><span>=</span><span class="attribute-value">"add"</span><span class="attribute">namespace</span><span>=</span><span class="attribute-value">"/user"</span><span class="tag">&gt;</span><span>
</span></span></li>
</ol>
</div>
查看HTML原代码：
<div class="codeText">
<div class="codeHead">HTML代码</div>
<ol class="dp-xml">
	<li class="alt"><span><span class="tag">&lt;</span><span class="tag-name">form</span><span class="attribute">id</span><span>=</span><span class="attribute-value">"testForm"</span><span class="attribute">name</span><span>=</span><span class="attribute-value">"add"</span><span class="attribute">action</span><span>=</span><span class="attribute-value">"/teststruts2_1/user/add"</span><span class="attribute">method</span><span>=</span><span class="attribute-value">"post"</span><span class="tag">&gt;</span><span>
</span></span></li>
</ol>
</div>
