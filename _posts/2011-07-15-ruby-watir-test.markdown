---
layout: post
category : Test
tags : [intro, Ruby, Watir, Test]
title: Ruby和Watir调用浏览器测试
wordpress_id: 974
wordpress_url: http://www.im47.cn/?p=974
date: 2011-07-15 09:15:23.000000000 +08:00
---
gem update --system
gem install watir

当使用Watir开发测试脚本的时候，通过给网页上的对象发送消息来与之交互。

Watir 语法（Web Application Testing in Ruby)

# watir的安装
watie的安装请查看 -&gt; Ruby library的安装

# 使用Watir工具，需要在脚本中加上
require 'watir'

# 创建一个IE的实例
ie = Watir::IE.new
或者在创建的同时直接转到页面
ie = Watir::IE.start('http://www.text.com/')
Watir使用start方法同时创建一个浏览器实例并转到一个页面。
IE浏览速度
ie.speed = :fast
ie.speed = :slow

# 页面导航
ie.goto('http://www.text.com/')
注: ie.goto还可以运行javascript的代码如: ie.goto("javascript: ie.document.write("Hello World");")

# 取得当前网页的网址
ie.url

# 点击超链接
ie.link(:text , "Pickaxe").click
ie.link(:href, /http:\/\/pragmaticprogrammer\.com/).click
ie.link(:name =&gt; 'foo', :index =&gt; 1).click

# 超链接的uri
ie.link(:text , "Pickaxe").href
ie.link(:index, 1).href
ie.link(:text =&gt; "reply", :index =&gt; 2).href

# 超链接的文本
ie.link(:href , /http:\/\/pragmaticprogrammer\.com/).text

对应的HTML代码为：
&lt;a href='http://pragmaticprogrammer.com/titles/ruby/'&gt;Pickaxe&lt;/a&gt;

# img标签
ie.image(:name, 'image').src
ie.image(:index, 2).src

对应的HTML代码为：
&lt;img name = img src='http://pragmaticprogrammer.com/titles/ruby/top.gif'&gt;
&lt;img name = img src='http://pragmaticprogrammer.com/titles/ruby/head.gif'&gt;

# 设置复选框
ie.checkbox(:name, "checkme").set
ie.checkbox(:name, "checkme", "1").set # 使用name和value属性设置复选框

# 清除复选框
ie.checkbox(:name, "checkme").clear
ie.checkbox(:name, "checkme", "1").clear # 使用name和value属性清除复选框

对应的HTML代码为：
&lt;input type = "checkbox" name = "checkme" value = "1"&gt;

# 设置单选框
ie.radio(:name, "clickme").set
ie.radio(:name=&gt;'clickme', :index=&gt;2).set
ie.radio(:name, "clickme", "1").set # 使用name和id属性设置单选框

# 使用name属性清除单选框
ie.radio(:name, "clickme").clear
ie.radio(:name, "clickme", "1").clear # 使用name和id属性清除单选框

对应的HTML代码为：
&lt;input type = "radio" name = "clickme" id = "1"&gt;
&lt;input type = "radio" name = "clickme" id = "2"&gt;

# 设置下拉框
ie.select_list(:name, "selectme").select('Python') # 使用text属性和值来设置下拉框
ie.select_list(:name, "selectme").select_value('2') # 使用value属性和值来设置下拉框

# 使用name属性和值来清除下拉框
ie.select_list(:name, "selectme").clearSelection

对应的HTML代码为：
&lt;select name = "selectme"&gt;
&lt;option value = 1&gt;Ruby
&lt;option value = 2&gt;Java
&lt;option value = 3&gt;Python
&lt;option value = 4&gt;C
&lt;/select&gt;

# 文本的框设置
ie.text_field(:name, "typeinme").set("Watir World")

# 清空文本输入框
ie.text_field(:name, "typeinme").clear

对应的HTML代码为：
&lt;input type = "text" name = "typeinme"&gt;

# 通过值或name属性点击button
ie.button(:value, "Click Me").click
ie.button(:name, "clickme").click

对应的HTML代码为：
&lt;input type = "button" name = "clickme" value = "Click Me"&gt;

# 通过值或name属性点击Submit
ie.button(:value, "Submit").click
ie.button(:type, "Submit").click
ie.button(:name, "Submit").click

对应的HTML代码为：
&lt;form. action = "submit" name = "submitform" method="post"&gt;
&lt;input type = "submit" value = "Submit"&gt;
&lt;/form&gt;

# 表单中的图片按钮
ie.button(:name, "doit").click

对应的HTML代码为：
&lt;form. action = "submit" name = "doitform" method="post"&gt;
&lt;input type="image" src = "images/doit.gif" name = "doit"&gt;
&lt;/form&gt;

# 没有按钮的表单
ie.form(:name, "loginform").submit # 通过name,action以及method属性来提交表单
ie.form(:action, "login").submit
对应的HTML代码为：
&lt;form. action = "login" name = "loginform" method="get"&gt;
&lt;input name="username" type="text"&gt;
&lt;/form&gt;

# 框架
ie.show_frames可以打印出当前页面框架的数量和名称
Watir允许通过名称属性来访问框架，如ie.frame("menu")
如果要访问menu框架中的一个超链接，可以
ie.frame("menu").link(:text, "Click Menu Item").click

# 嵌套框架
ie.frame(:name, "frame1").form(:name, 'form1')

# 新窗口
一些Web应用会弹出新窗口或打开一个新窗口，可以使用attach方法来访问并控制新窗口。通过标示新窗口的URL或者title来访问。
ie2 = Watir::IE.attach(:url, 'http://www.text.com/')
ie3 = Watir::IE.attach(:title, 'Test New Window')
也可以使用正则表达式
ie4 = Watir::IE.attach(:title, /Test New/)
注意：不要把新窗口分配到你的ie变量，最好给新窗口一个不同的名字

# 访问Table元素：
t = $ie.table(:id,"data")
t = Table.new($ie,:id,"data")
t = $ie.table[1]

# tr,td元素
tr = ie.row(:id,"title")
tr = TableRow.new(ie,:id,"title")

td = ie.cell(:id,"name")
td = TableCell.new(ie,:id,"name")

# Watir中Table，TableBody，TableRow，TableCell这几个类，都提供了一个索引方法"[](index)"来定位其下一层的子元素对象,该方法为实例方法,"index"为传入的参数,索引值从1开始,而非从0开始。
用法如下：
以table的第一行，第一个元素为例：
tr1 = t.[](1)
td1 = tr1.[](1)
也可以连续访问：td1 = t.[](1).[](1)
如果td中还有其他元素，可以通过td的实例方法直接访问，以checkbox为例：
cb = td1.checkbox(:id,'navigate_id').click

对于以上所提到的对象,都是从Element继承而来,所以click,enabled?,exists?,fireEvent,flash,focus等方法都直接可以使用。
如果你的td元素定位准确了，且鼠标响应事件没有错误的话，那么应该能看到点击后的效果。
建议多查一下Watir的API Reference <a href="http://wtr.rubyforge.org/rdoc/" target="_blank">http://wtr.rubyforge.org/rdoc/</a>

代码如下：
t = ie.table(:id,"CoolMenu2menutable")
td_logout=t.[](1).[](16)

先找到Table，再索引TR，再索引到TD

# 运行Ruby时不显示browser方法
运行Ruby程序文件时在后面加 "-b"
ex:
test.rb -b
也可以做成.bat文件
ex: test.bat
ruby.exe test.rb -b

# 获取隐含对象值
&lt;INPUT type=hidden value="您的Email" name="field1"&gt;
方法：values = ie.hidden(:name, 'field1').value

# 获取窗口对象
方法1： ie2 = Watir::IE.attach(:url,'http://www.google.cn/')   #根据URL获取
方法2： ie3 = Watir::IE.attach(:title,'Google')                #根据窗口标题获
方法3： ie4 = Watir::IE.attach(:title, /google.cn/)              #正则表达式匹配获取

转自：<a href="http://my.chinaunix.net/space.php?uid=20255298&amp;do=blog&amp;id=170598" target="_blank">http://my.chinaunix.net/space.php?uid=20255298&amp;do=blog&amp;id=170598</a>
