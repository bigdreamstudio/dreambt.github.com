---
layout: post
category : J2EE
tags : [intro, Spring, applicationContext.xml, J2EE]
title: Spring的applicationContext.xml文件
wordpress_id: 161
wordpress_url: http://www.im47cn.nt102.idcwind.net/wordpress/?p=161
date: 2009-06-01 20:13:26.000000000 +08:00
---
<span style="font-weight: bold;">以下是详解Spring的applicationContext.xml文件代码：
</span><span style="color: #ff00ff;">&lt;!-- 头文件，主要注意一下编码 --&gt;</span>
&lt;?xml version="1.0" encoding="UTF-8"?&gt;
&lt;!DOCTYPE beans PUBLIC "-//SPRING//DTD BEAN//EN" "http://www.springframework.org/dtd/spring-beans.dtd"&gt;
&lt;beans&gt;
<span style="color: #ff00ff;"> &lt;!-- 建立数据源 --&gt;</span><span style="color: #0000ff;">
</span> &lt;bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource"&gt;
<span style="color: #ff00ff;"> &lt;!-- 数据库驱动，我这里使用的是Mysql数据库 --&gt;</span><span style="color: #0000ff;">
</span> &lt;property name="driverClassName"&gt;
&lt;value&gt;com.mysql.jdbc.Driver&lt;/value&gt;
&lt;/property&gt;
<span style="color: #ff00ff;"> &lt;!-- 数据库地址，这里也要注意一下编码，不然乱码可是很郁闷的哦！ --&gt;</span><span style="color: #ffff00;">
</span> &lt;property name="url"&gt;
&lt;value&gt;
jdbc:mysql://localhost:3306/tie?useUnicode=true&amp;amp;characterEncoding=utf-8
&lt;/value&gt;
&lt;/property&gt;
<span style="color: #ff00ff;"> &lt;!-- 数据库的用户名 --&gt;</span><span style="color: #0000ff;">
</span> &lt;property name="username"&gt;
&lt;value&gt;root&lt;/value&gt;
&lt;/property&gt;
<span style="color: #ff00ff;"> &lt;!-- 数据库的密码 --&gt;</span><span style="color: #0000ff;">
</span> &lt;property name="password"&gt;
&lt;value&gt;123&lt;/value&gt;
&lt;/property&gt;
&lt;/bean&gt;
<span style="color: #ff00ff;"> &lt;!-- 把数据源注入给Session工厂 --&gt;</span>
&lt;bean id="sessionFactory"
class="org.springframework.orm.hibernate3.LocalSessionFactoryBean"&gt;
&lt;property name="dataSource"&gt;
&lt;ref bean="dataSource" /&gt;
&lt;/property&gt;
<span style="color: #ff00ff;"> &lt;!-- 配置映射文件 --&gt;</span>
&lt;property name="mappingResources"&gt;
&lt;list&gt;
&lt;value&gt;com/alonely/vo/User.hbm.xml&lt;/value&gt;
&lt;/list&gt;
&lt;/property&gt;
&lt;/bean&gt;
<span style="color: #ff00ff;"> &lt;!-- 把Session工厂注入给hibernateTemplate --&gt;
&lt;!-- 解释一下hibernateTemplate：hibernateTemplate提供了很多方便的方法，在执行时自动建立 HibernateCallback 对象，例如：load()、get()、save、delete()等方法。 --&gt;</span>
&lt;bean id="hibernateTemplate"
class="org.springframework.orm.hibernate3.HibernateTemplate"&gt;
&lt;constructor-arg&gt;
&lt;ref local="sessionFactory" /&gt;
&lt;/constructor-arg&gt;
&lt;/bean&gt;
<span style="color: #ff00ff;"> &lt;!-- 把DAO注入给Session工厂 --&gt;</span><span style="color: #ffff00;">
</span> &lt;bean id="userDAO" class="com.alonely.dao.UserDAO"&gt;
&lt;property name="sessionFactory"&gt;
&lt;ref bean="sessionFactory" /&gt;
&lt;/property&gt;
&lt;/bean&gt;
<span style="color: #ff00ff;"> &lt;!-- 把Service注入给DAO --&gt;</span><span style="color: #0000ff;">
</span> &lt;bean id="userService" class="com.alonely.service.UserService"&gt;
&lt;property name="userDAO"&gt;
&lt;ref local="userDAO" /&gt;
&lt;/property&gt;
&lt;/bean&gt;
<span style="color: #ff00ff;"> &lt;!-- 把Action注入给Service --&gt;</span>
&lt;bean name="/user" class="com.alonely.struts.action.UserAction"&gt;
&lt;property name="userService"&gt;
&lt;ref bean="userService" /&gt;
&lt;/property&gt;
&lt;/bean&gt;
&lt;/beans&gt;
