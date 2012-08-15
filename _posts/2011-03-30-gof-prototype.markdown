---
layout: post
category : Design Pattern
tags : [intro, GoF, Prototype]
title: GoF设计模式之Prototype(原型模式)
wordpress_id: 894
wordpress_url: http://www.im47.net/?p=894
date: 2011-03-30 12:24:48.000000000 +08:00
---
名称：Prototype(原型模式)

分类：创建模式

意图：

问题：

解决方案：

参与者和协作者：

效果：

实现：

&nbsp;

原型模式定义:
用原型实例指定创建对象的种类,并且通过拷贝这些原型创建新的对象.

Prototype模式允许一个对象再创建另外一个可定制的对象，根本无需知道任何如何创建的细节,工作原理是:通过将一个原型对象传给那个要发动创建的对象，这个要发动创建的对象通过请求原型对象拷贝它们自己来实施创建。

如何使用?
因为Java中的提供clone()方法来实现对象的克隆,所以Prototype模式实现一下子变得很简单.

以勺子为例：
<table border="0" cellspacing="3" cellpadding="3" width="96%">
<tbody>
<tr>
<td bgcolor="#CCCCCC">public abstract class AbstractSpoon implements Cloneable
{
String spoonName;

public void setSpoonName(String spoonName) {this.spoonName = spoonName;}
public String getSpoonName() {return this.spoonName;}

public Object clone()
{
Object object = null;
try {
object = super.clone();
} catch (CloneNotSupportedException exception) {
System.err.println("AbstractSpoon is not Cloneable");
}
return object;
}
}</td>
</tr>
</tbody>
</table>
有个具体实现(ConcretePrototype):
<table border="0" cellspacing="3" cellpadding="3" width="92%">
<tbody>
<tr>
<td bgcolor="#CCCCCC">public class SoupSpoon extends AbstractSpoon
{
public SoupSpoon()
{
setSpoonName("Soup Spoon");
}
}

&nbsp;</td>
</tr>
</tbody>
</table>
调用Prototype模式很简单:

AbstractSpoon spoon = new SoupSpoon();
AbstractSpoon spoon2 = spoon.clone();

当然也可以结合工厂模式来创建AbstractSpoon实例。

在Java中Prototype模式变成clone()方法的使用，由于Java的纯洁的面向对象特性，使得在Java中使用设计模式变得很自然，两者已经几乎是浑然一体了。这反映在很多模式上，如Interator遍历模式。

&nbsp;
