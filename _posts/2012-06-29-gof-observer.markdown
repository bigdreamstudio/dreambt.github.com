---
layout: post
category : Design_Pattern
tags : [GoF, Observer]
title: GoF设计模式之Observer观察者模式
date: 2012-06-29 22:59:50.000000000 +08:00
thumbnail: /assets/images/2012/07/gof_observer_1.jpg
excerpt: 观察者模式（别名Publish-Subscribe）:定义了对象之间的一对多依赖，这样一来，当一个对象改变状态时，它的所有依赖者都会收到通知并自动更新。
---
观察者模式（别名Publish-Subscribe）:定义了对象之间的一对多依赖，这样一来，当一个对象改变状态时，它的所有依赖者都会收到通知并自动更新。

生活中的实例：

订阅报纸杂志

1. 报社的业务就是出版报纸。
2. 向某家报社订阅报纸，只要他们有新报纸出版，就会给你送来。只要你是他们的订户，你就会一直收到新报纸。
3. 当你不想再看报纸的时候，取消订阅，他们就不会再送新报纸来。
4. 只要报社还在运营，就会一直有人（或单位）向他们订阅报纸或取消订阅报纸。

要点：

1． 主题用一个共同的接口来通知观察者(notifyObservers)
2． 主题和观察者之间松耦合，主题不知道观察者的细节，只知道观察者实现了观察者接口(update)
3． 推、拉模式各有千秋,推的实现要简单一些

实现：

![/assets/images/2012/07/gof_observer_1.jpg](/assets/images/2012/07/gof_observer_1.jpg)

Subject：

1. Subject可以加将Observer对象的引用保存在一个聚集中，每个Subject可以有任意个Observer。
2. 提供注册和删除Observer对象的接口。

<pre>public interface Subject {
	public void registerObserver(Observer o);
	public void removeObserver(Observer o);
	public void notifyObservers();
}</pre>

Observer：

1. 为那些在Subject发生改变时需获得通知的对象定义一个更新接口。

<pre>public interface Observer {
	public void update();
}</pre>

Concrete Subject：

1. 将有关状态存入各 ConcreteObserver对象，当它的状态发生改变时，向它的各个观察者发出通知。

<pre>public class ConcreteSubject implements Subject {

	private ArrayList&lt;Observer&gt; observers;

	public ConcreteSubject() {
		observers = new ArrayList&lt;Observer&gt;();		
	}

	public void registerObserver(Observer o) {		
		observers.add(o);		
	}

	public void removeObserver(Observer o) {		
		int i = observers.indexOf(o);		
		if (i &gt;= 0) {			
			observers.remove(i);			
		}		
	}

	public void notifyObservers() {		
		for (int i = 0; i &lt; observers.size(); i++) {			
			observer = observers.get(i);			
			observer.update();			
		}		
	}

	public void stateChanged() {		
		notifyObservers();		
	}

	public void setState() {		
		stateChanged();		
	}
}</pre>

Concrete Observer：

1. 维护一个指向ConcreteSubject对象的引用。
2. 存储有关状态，这些状态应与Subject的状态保持一致。
3. 实现Observer的更新接口以使自身状态与Subject的状态保持一致。

<pre>public class ConcreteObserver implements Observer {

	private Subject subject;

	public ConcreteObserver(Subject subject) {
		this.subject = subject;		
		subject.registerObserver(this);		
	}

	public void update() {		
		// your code		
	}	
}</pre>

Java对观察者模式的实现：

1. Java.util.Observable相当与Subject，发出通知时应该先调用setChanged()方法，再调用notifyObservers方法通知观察者。
2. 可以把数据封装成对象传入notifyObservers(Object arg)，观察者调用update(Obserable o,Object arg)取出数据，实现推模式。如果调用notifyObservers()，观察者调用update(Obserable o,Object arg)取出Obserable，再调用它的get状态方法，实现拉模式。

适用性：

1. 当一个抽象模型有两个方面，其中一个方面依赖于另一方面。将这二者封装在独立的对象中以使它们可以各自独立地改变和复用。
2. 当对一个对象的改变需要同时改变其它对象，而不知道具体有多少对象有待改变。
3. 当一个对象必须通知其它对象，而它又不能假定其它对象是谁。换言之，你不希望这些对象是紧密耦合的。

优点：

1. 目标和观察者间的松耦合。一个目标所知道的仅仅是它有一系列观察者，每个都符合抽象的 Observer类的简单接口。目标不知道任何一个观察者属于哪一个具体的类。这样目标和观察者之间的耦合是抽象的和最小的。
2. 支持广播通信，主题向所有登记过的观察者发出通知。

缺点:

1. 意外的更新。因为一个观察者并不知道其它观察者的存在，它可能对改变目标的最终代价一无所知。在目标上一个看似无害的的操作可能会引起一系列对观察者以及依赖于这些观察者的那些对象的更新。此外，如果依赖准则的定义或维护不当，常常会引起错误的更新，这种错误通常很难捕捉。
2. 如果一个主题对象有很多直接或间接的观察者对象，将所有的观察者都通知会花费很多时间。
3. 如果对观察者的通知是在另外的线程异步进行的话，要额外注意多线程操作。