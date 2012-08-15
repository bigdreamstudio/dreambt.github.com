---
layout: post
category : Design Pattern
tags : [intro, GoF, Policy]
title: GoF设计模式之Policy策略模式
wordpress_id: 1137
wordpress_url: http://www.im47.cn/?p=1137
date: 2012-06-27 22:58:01.000000000 +08:00
---
策略模式（别名Policy）: 定义了算法族，分别封装起来，让它们之间可以互相替换，此模式让算法的变化独立于使用算法的客户。

&nbsp;

使用继承的缺点：

1． 代码在多个子类中重复

2． 运行时的行为不容易改变

3． 很难抽象出子类的全部行为

4． 牵一发而动全身

&nbsp;

设计原则:

1. 把会变化的部分取出来进行封装，以便以后可以轻易的改动或扩充该部分，而不影响不需要变化的其他部分

2. 针对接口编程，而不是针对实现编程

3. 少用继承，多用组合

&nbsp;

特点：

a) 策略模式找出会变化的算法，将它们封装出来，再利用组合将算法抽象接口整合进系统中以达到松耦合的目的。

b) 当准备在一个系统里面使用策略模式的时，首先必须找到需要包装的算法，看看算法是否可以从环境中分割出来，最后再考察这些算法是否会在以后发生变化。

c) 策略模式不适合于处理同时嵌套多于一个算法的情形。

&nbsp;

实现：

<a href="http://www.im47.cn/wp-content/uploads/2012/06/policy.jpg"><img class="aligncenter size-full wp-image-1138" title="policy" src="http://www.im47.cn/wp-content/uploads/2012/06/policy.jpg" alt="" width="555" height="222" /></a>

&nbsp;

环境(context)角色:

持有一个Strategy的引用，用一个具体的ConcreteStrategy对象来配置自己的ContextInterface行为

public class Context{
private Strategy strategy;
public void contextInterface(){
strategy.strategyInterface();
}
}

Strategy接口角色：定义所有算法的公共接口，可以由Interface和抽象类实现

abstract public class Strategy{
public abstract void strategyInterface();
}

ConcreteStrategy角色：以Strategy接口实现具体的算法

public class ConcreteStrategy extends Strategy{
public void strategyInterface(){
//write you algorithm code here
}
}

&nbsp;

适用性：

当存在以下情况时使用strategy模式:

1. 如果一个系统里面有许多类，它们之间的区别仅在于它们的行为，那么使用策略模式可以动态地让一个对象在许多行为中选择一种行为。

2. 一个系统需要动态的在几种算法中选择一种。那么这些算法可以包装到一个个的具体算法类里面，而这些算法类都是一个抽象算法类的子类。换言之，这些具体算法类均有统一的接口，由于多态性原则，客户端可以选择使用任何一个具体算法类，并只持有一个数据类型是抽象算法的对象。

3. 一个系统的算法使用的数据不可以让客户端知道。策略模式可以避免让客户端涉及到不必要接触到的和复杂的只与算法有关的数据。

4. 如果一个类定义了多种行为，并且这些行为在这个类的操作中以多个条件语句的形式出现。将相关的条件分支移到它们各自的Strategy类中以代替这些条件语句。

&nbsp;

优点：

1. Strategy类层次为Context定义了一系列可供重用的算法或行为，继承有助于析取出这些算法中的公共部分。

2. 消除了一些条件语句，Strategy利用继承这一多态性避免了使用多重条件转移语句。

3. Strategy将变化的算法封装出来，使得客户端动态改变算法或行为变的可能。

&nbsp;

缺点：

1. 客户端必须知道所有的策略类，并自行决定使用哪一个策略类。这就意味着客户端必须理解这些算法的区别，以便适时的选择合适的算法。

2. Strategy造就了很多的策略类，增加了对象的数目。
