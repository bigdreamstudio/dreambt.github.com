---
layout: post
category : Design Pattern
tags : [intro, GoF, Factory]
title: GoF设计模式之工厂方法模式
wordpress_id: 1157
wordpress_url: http://www.im47.cn/?p=1157
date: 2012-07-05 23:08:53.000000000 +08:00
---
工厂方法模式（别名Virtual Constructor）定义了一个创建对象的接口，但由子类决定要实例化的类是哪一个。工厂方法让类把实例化推迟到子类。

&nbsp;

特点：

1. 工厂方法让子类决定要实例化的类是哪一个。所谓的“决定”并不是指模式允许子类本身在运行时做决定，而是指编写抽象工厂类时，不需要知道实际创建的产品是哪一个。选择使用了那个子类，自然就决定了实际使用的产品是什么。

2. 工厂方法模式是由简单工厂经过重构（利用多态性替换复杂的if else逻辑）而成。

3. 工厂方法是简单工厂的推广，简单工厂是工厂方法的退化。

4. 在工厂方法模式中，核心的工厂类不再负责所有产品的创建，而是将具体创建工作交给子类去做。这个核心类仅仅负责给出具体工厂必须实现的接口，而不接触哪一个产品类被实例化这种细节。这使得工厂方法模式可以允许系统在不修改工厂角色的情况下引进新产品。在Factory Method模式中，工厂类与产品类往往具有平行的等级结构，它们之间一一对应。

5. 工厂方法不一定每一次都返还一个新的对象。但是它所返还的对象一定是他自己创建的，而不是在一个外部对象里面创建，然后传入工厂对象中的。

&nbsp;

原则：

依赖倒置原则：要依赖抽象，不要依赖具体类。

依赖倒置的指导方针：

1. 变量不可以持有具体类的引用

2. 不要让类派生自具体类

3. 不要覆盖基类中已经实现的方法

&nbsp;

实现：

<img class="aligncenter size-full wp-image-1158" title="1" src="http://www.im47.cn/wp-content/uploads/2012/07/11.jpg" alt="" width="582" height="173" />

&nbsp;

抽象工厂（工厂方法模式的核心，也可以是抽象类）：
<div>
<pre>public interface Creator
{
    public Product factory();
}</pre>
</div>
具体工厂：
<div>
<pre>public class ConcreteCreator1 implements Creator
{
    public Product factory()
    {
        return new ConcreteProduct1();
    }
}</pre>
</div>
&nbsp;
<div>
<pre>public class ConcreteCreator2 implements Creator
{
    public Product factory()
    {
        return new ConcreteProduct2();
    }
}</pre>
</div>
抽象产品：
<div>
<pre>public interface Product
{
}</pre>
</div>
具体产品：
<div>
<pre>public class ConcreteProduct1 implements Product
{
    public ConcreteProduct1()
    {
	System.out.println("CocnreteProduct1 is being created.");
    }
}</pre>
</div>
&nbsp;
<div>
<pre>public class ConcreteProduct2 implements Product
{
    public ConcreteProduct2()
    {
	System.out.println("CocnreteProduct2 is being created.");
    }
}</pre>
</div>
&nbsp;

适用性：

在以下情况下，适用于Factory Method模式：
<ol>
	<li>当一个类不知道它所必须创建的对象的类的时候。</li>
	<li>当一个类希望由它的子类来指定它所创建的对象的时候。</li>
	<li>当类将创建对象的职责委托给多个帮助子类中的某一个，并且你希望将哪一个帮助子类是代理者这一信息局部化的时候。</li>
</ol>
&nbsp;

参与者：
<ol>
	<li>Product:定义工厂方法所创建的对象的接口</li>
	<li>ConcreteProduct:实现Product接口</li>
	<li>Creator:</li>
</ol>
<ul>
	<li>声明工厂方法，该方法返回一个Product类型的对象。Creator也可以定义一个工厂方法的缺省实现，它返回一个缺省的ConcreteProduct对象。</li>
	<li>工厂方法可以带参数，由参数决定返回的产品实例。</li>
</ul>
4.  ConcreteCreator：实现Creator以返回一个ConcreteProduct实例
