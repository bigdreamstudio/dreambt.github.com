---
layout: post
category : J2EE
tags : [J2EE, Struts2]
title: 浅谈s:form标签中action属性为XX.action与XX的区别
wordpress_id: 170
wordpress_url: http://www.im47cn.nt102.idcwind.net/wordpress/?p=170
date: 2009-07-11 19:05:32.000000000 +08:00
excerpt: 从home进入login.jsp页面时，点击"submit"按钮，程序执行正常。 但是从userManage重定向到login.jsp页面时，点击"submit"按钮就会出现异常。当我们将 <s:form action="login.action" method="post" namespace="/demo/login"> 改成 <s:form action="login" method="post" namespace="/demo/login"> 就能正常执行了。
---
先来看一个实例struts.xml的内容：

    <packagename="home"extends="demo"namespace="/demo/home">
        <actionname="home"class="com.demo.home.HomeAction">
            <resultname="login"type="redirect-action">
                <paramname="namespace">/demo/login </param>
                <paramname="actionName">login!init.action </param>
            </result>
            <resultname="userManage"type="redirect-action">
                <paramname="namespace">/demo/userManage </param>
                <paramname="actionName">userManage!init.action </param>
            </result>
        </action>
    </package>
    <packagename="userManage"extends="demo"namespace="/demo/userManage">
        <actionname="userManage"class="com.demo.userManage.UserManageAction">
            <resultname="login"type="redirect">/login/login.jsp </result>
        </action>
    </package>
    <packagename="login"extends="demo"namespace="/demo/login">
        <actionname="login"class="com.demo.login.LoginAction">
            <resultname="input">/login/login.jsp </result>
        </action>
    </package>
    
继续看 login.jsp 的内容：

    <s:form action="login.action" method="post" namespace="/demo/login">
    <table width="100%" align="center">
        <tr><td>name : <s:textfield name="loginForm.name"/></td></tr>
        <tr><td>password : <s:password name="loginForm.password"/></td></tr>
        <tr><td><s:submit name="login" value="submit" method="login"/></td></tr>
    </table>
    </s:form>
    
已知问题：

从home进入login.jsp页面时，点击"submit"按钮，程序执行正常。 但是从userManage重定向到login.jsp页面时，点击"submit"按钮，出现下面的异常： 

>javax.servlet.ServletException: java.lang.NoSuchMethodException: 
com.opensymphony.xwork2.ActionSupport.login() 
org.apache.struts2.dispatcher.Dispatcher.serviceAction(Dispatcher.java:515) 
org.apache.struts2.dispatcher.FilterDispatcher.doFilter(FilterDispatcher.java:419)

然后我把login.jsp的 <s:form action="login.action" method="post" namespace="/demo/login"> 改成 <s:form action="login" method="post" namespace="/demo/login"> 就能正常执行了。

很多网友都在讨论 .action 到底应该又还是不应该有，那么我们先看看下面的分析吧

jsp页面带 .action 时：

    <s:formid="testForm"action="add.action"namespace="/user">

查看HTML原代码：

    <formid="testForm"action="add.action"method="post">

jsp页面不带 .action 时：

    <s:formid="testForm"action="add"namespace="/user">

查看HTML原代码：

    <formid="testForm"name="add"action="/teststruts2_1/user/add"method="post">