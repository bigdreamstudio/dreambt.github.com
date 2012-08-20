---
layout: post
category : Design_Pattern
tags : [GoF, factory]
title: GoF设计模式之简单工厂模式
author: Dreambt
wordpress_id: 893
wordpress_url: http://www.im47.net/?p=893
date: 2012-07-04 15:18:23.000000000 +08:00
---
{% excerpt %}
简单工厂模式是由一个工厂对象决定创建出哪一种产品类的实例。

&nbsp;

实现：

<a href="http://www.im47.cn/wp-content/uploads/2011/03/1.jpg"><img class="aligncenter size-full wp-image-1153" title="1" src="http://www.im47.cn/wp-content/uploads/2011/03/1.jpg" alt="" width="501" height="180" /></a>
{% endexcerpt %}
&nbsp;

工厂类:
<div>
<pre>public class Creator
{
    public static Product factory()
    {
        return new ConcreteProduct();
    }
}</pre>
</div>
抽象产品:
<div>
<pre>public interface Product
{
}</pre>
</div>
具体产品:
<div>
<pre>public class ConcreteProduct implements Product
{
    public ConcreteProduct(){}
}</pre>
</div>
&nbsp;

实现要点：

1. 工厂类可以根据传入的参数决定创建出哪一种产品类的实例。

2. 具体产品有共同的商业逻辑，那么这些公有的逻辑就应当移到抽象角色里面，这就意味着抽象角色应当由一个抽象类扮演。

3. 每个工厂类可以有多于一个的工厂方法，分别负责创建不同的产品对象。如java.text.DateFormat 类

&nbsp;

特例：

如果只有一个具体产品的话，抽象产品可以省略

<a href="http://www.im47.cn/wp-content/uploads/2011/03/2.jpg"><img class="aligncenter size-full wp-image-1154" title="2" src="http://www.im47.cn/wp-content/uploads/2011/03/2.jpg" alt="" width="553" height="84" /></a>

&nbsp;

在某些情况下，可以由抽象产品扮演工厂类的角色，典型的应用就是java.text.DateFormat，一个抽象产品担当子类的工厂

<a href="http://www.im47.cn/wp-content/uploads/2011/03/3.jpg"><img class="aligncenter size-full wp-image-1155" title="3" src="http://www.im47.cn/wp-content/uploads/2011/03/3.jpg" alt="" width="319" height="190" /></a>

&nbsp;

如果抽象产品再省略的话，可以做到三者合并，这样一个产品类为自身的工厂

&nbsp;

<a href="http://www.im47.cn/wp-content/uploads/2011/03/4.jpg"><img class="aligncenter size-full wp-image-1156" title="4" src="http://www.im47.cn/wp-content/uploads/2011/03/4.jpg" alt="" width="352" height="104" /></a>

优点：

模式的核心是工厂类。工厂类含有必要的判断逻辑，可以决定在什么时候创建哪一个产品类的实例。而客户端则可以免除直接创建产品的责任，而仅仅负责“消费”产品。

&nbsp;

缺点：

1. 工厂类成为“全能类”，添加新的产品或扩展功能时会非常复杂

2. 经常使用static方法作为工厂方法（也叫静态工厂方法），不能通过继承来改变创建方法的行为。

&nbsp;

在java中的应用：

DateFormat和sax2库中的XMLReaderFactory
