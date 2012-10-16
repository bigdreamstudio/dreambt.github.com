---
layout: post
category: Web_Design
tags: [div, CSS, tutorial]
title: div+css命名参考
date: 2007-01-14 03:46:21.000000000 +08:00
excerpt: 用了一段 CSS 布局设计网页,发现自己的命名有点混乱,完全按照自己的想法命名,虽然没什么影响,有不给别人看源文件,但是工作室有时候和团队合作完成项目的时候,就遇到麻烦了,要修改一个地方相当的费事.所以还是有个标准比较好啊!在网上看到的一个相关的参考,再加上平时也研究别人的代码,发现这样的命名使用很广泛!我再加上自己的经验,希望对看到这篇文章的人能有用!
---
用了一段 CSS 布局设计网页,发现自己的命名有点混乱,完全按照自己的想法命名,虽然没什么影响,有不给别人看源文件,但是工作室有时候和团队合作完成项目的时候,就遇到麻烦了,要修改一个地方相当的费事.所以还是有个标准比较好啊!在网上看到的一个相关的参考,再加上平时也研究别人的代码,发现这样的命名使用很广泛!我再加上自己的经验,希望对看到这篇文章的人能有用!

## 命名参考

### 常用的CSS命名规则

* 头：header
* 内容：content/container
* 尾：footer
* 导航：nav
* 侧栏：sidebar
* 栏目：column
* 页面外围控制整体布局宽度：wrapper
* 左右中：left right center

命名全部使用小写字母，如果需要多个单词，单词间使用“-”分隔，比如user-list

### 常用代码结构

* div：主要用于布局，分割页面的结构
* ul/ol：用于无序/有序列表
* span：没有特殊的意义，可以用作排版的辅助，例如

程序代码

    <span>(4.23)</span>

然后在css中定义span为右浮动，实现了日期和标题分两侧显示的效果

* h1-h6：标题
* h1-h6 根据重要性依次递减
* h1位最重要的标题

* label：为了使你的表单更有亲和力而且还能辅助表单排版的好东西，例如：

    <label for="user-password">密　码</label>
    <input type="password" name="password" id="user-password" />

* fieldset & legend：fildset套在表单外，legend用于描述表单内容。例如：

程序代码

    <form>
        <fieldset>
            <legend>title</legend>
            <label for="user-password">密　码</label>
            <input type="password" name="password" id="user-password" />
        </fieldset>
    </form>

* dl,dt,dd：当页面中出现第一行为类似标题/简述，然后下面为详细描述的内容时应该使用该标签，例如

程序代码

    <dl>
    <dt>什么是CSS?</dt>
    <dd>CSS就是一种叫做样式表（stylesheet）的技术。也有的人称之为层叠样式表（Cascading Stylesheet）。</dd>
    <dd>
        <dt>什么是XHTML？</dt>
    </dd>
    <dd>XHTML是一个基于XML的置标语言，看起来与HTML有些想像，只有一些小的但重要的区别。可以这样看，XHTML就是一个扮演着类似HTML的角色的XML。 本质上说，XHTML是一个桥接（过渡）技术，结合了XML（有几分）的强大功能及HTML（大多数）的简单特性。</dd>
    </dl>

    C #content
    S #subcol
    M #maincol
    X #xcol

这是纵向布局的XHTML结构，c-smx表示网页有三个纵栏, c-sm表示有两个纵栏, c-mx表示有两个纵栏其中一个是附属的, c-m表示一个纵栏。

其中在三纵栏的布局需要分为两层 第一层是#subcol与#main第二层是#main中的#maincol与#xcol。

### 自定义命名

根据w3c网站上给出的,最好是用意义命名
比如：是重要的新闻高亮显示（像红色）
有两种

    .red{color:red}
    .important-news{color:red}
    
很显然第二种传达的意义更加明确,所以尽量不要用意义不明确的作为自己自定义的名字