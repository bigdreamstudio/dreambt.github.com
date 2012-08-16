---
layout: post
category : J2EE
tags : [J2EE, ognl, Struts2]
title: Struts2 ognl中的#、%和$符号用法说明
wordpress_id: 1018
wordpress_url: http://www.im47.cn/?p=1018
date: 2011-10-08 16:08:43.000000000 +08:00
---
#、%和$符号在OGNL表达式中经常出现，而这三种符号也是开发者不容易掌握和理解的部分。

<strong>1．#符号的</strong>

用途一般有三种。
1)访问非根对象属性，例如示例中的#session.msg表达式，由于Struts 2中值栈被视为根对象，所以访问其他非根对象时，需要加#前缀。实际上，#相当于ActionContext. getContext()；#session.msg表达式相当于ActionContext.getContext().getSession(). getAttribute(”msg”) 。
2)用于过滤和投影（projecting）集合，如示例中的persons.{?#this.age&gt;20}。
3)用来构造Map，例如示例中的#{'foo1':'bar1','foo2':'bar2'}。

<strong>2．%符号</strong>

<strong></strong>%符号的用途是在标志的属性为字符串类型时，计算OGNL表达式的值。如下面的代码所示：
构造Map
&lt;s:set name="foobar" value="#{'foo1':'bar','foo2':'bar2'}" /&gt;

&lt;p&gt;The value of key "foo1" is &lt;s:property value="#foobar['foo1']" /&gt;&lt;/p&gt;

&lt;p&gt;不使用％：&lt;s:url value="#foobar['foo1']" /&gt;&lt;/p&gt;

&lt;p&gt;使用％：&lt;s:url value="%{#foobar['foo1']}" /&gt;&lt;/p&gt;

<strong>3．$符号</strong>

$符号主要有两个方面的用途。

在国际化资源文件中，引用OGNL表达式，例如国际化资源文件中的代码：reg.agerange=国际化资源信息：年龄必须在${min}同${max}之间。

在Struts 2框架的配置文件中引用OGNL表达式，例如下面的代码片断所示：
&lt;validators&gt;
&lt;field name=”intb”&gt;
&lt;field-validator type=”int”&gt;
&lt;param name=”min”&gt;10&lt;/param&gt;
&lt;param name=”max”&gt;100&lt;/param&gt;
&lt;message&gt;BAction-test校验：数字必须为${min}为${max}之间！&lt;/message&gt;
&lt;/field-validator&gt;
&lt;/field&gt;
&lt;/validators&gt;
