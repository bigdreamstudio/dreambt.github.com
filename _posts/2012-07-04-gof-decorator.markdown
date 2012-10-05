---
layout: post
category: Design_Pattern
tags: [GoF, Decorator]
title: GoF设计模式之装饰者模式
date: 2012-07-04 19:36:04.000000000 +08:00
thumbnail: /assets/images/2012/07/gof_decorator_1.jpg
excerpt: 装饰者模式动态的将职责附加到对象上，若要扩展功能，装饰者提供了比继承更具弹性的代替方案。
---
装饰者模式

Decorator模式（别名Wrapper）：动态将职责附加到对象上，若要扩展功能，装饰者提供了比继承更具弹性的代替方案。

意图：

动态地给一个对象添加一些额外的职责。就增加功能来说，Decorator模式相比生成子类更为灵活。

设计原则：

1. 多用组合，少用继承。

利用继承设计子类的行为，是在编译时静态决定的，而且所有的子类都会继承到相同的行为。然而，如果能够利用组合的做法扩展对象的行为，就可以在运行时动态地进行扩展。

2. 类应设计的对扩展开放，对修改关闭。

要点：

1． 装饰者和被装饰对象有相同的超类型。
2． 可以用一个或多个装饰者包装一个对象。
3． 装饰者可以在所委托被装饰者的行为之前或之后，加上自己的行为，以达到特定的目的。
4． 对象可以在任何时候被装饰，所以可以在运行时动态的，不限量的用你喜欢的装饰者来装饰对象。
5． 装饰模式中使用继承的关键是想达到装饰者和被装饰对象的类型匹配，而不是获得其行为。
6． 装饰者一般对组件的客户是透明的，除非客户程序依赖于组件的具体类型。在实际项目中可以根据需要为装饰者添加新的行为，做到“半透明”装饰者。
7． 适配器模式的用意是改变对象的接口而不一定改变对象的性能，而装饰模式的用意是保持接口并增加对象的职责。

实现：

![/assets/images/2012/07/gof_decorator_1.jpg](/assets/images/2012/07/gof_decorator_1.jpg)

Component：

定义一个对象接口，可以给这些对象动态地添加职责。

<pre>public interface Component
{
	void operation();
}</pre>

Concrete Component：

定义一个对象，可以给这个对象添加一些职责。

<pre>public class ConcreteComponent implements Component
{
	public void operation()
	{
		// Write your code here
	}
}</pre>

Decorator：

维持一个指向Component对象的引用，并定义一个与 Component接口一致的接口。

<pre>public class Decorator implements Component
{
	public Decorator(Component component)
	{
		this.component = component;
	}

	public void operation()
	{
		component.operation();
	}

	private Component component;
}</pre>

Concrete Decorator：

在Concrete Component的行为之前或之后，加上自己的行为，以“贴上”附加的职责。

<pre>public class ConcreteDecorator extends Decorator
{
	public void operation()
	{
		//addBehavior也可以在前面

		super.operation();

		addBehavior();
	}

	private void addBehavior()
	{
		//your code
	}
}</pre>

模式的简化：

1. 如果只有一个Concrete Component类而没有抽象的Component接口时，可以让Decorator继承Concrete Component。

![/assets/images/2012/07/gof_decorator_2.jpg](/assets/images/2012/07/gof_decorator_2.jpg)

2. 如果只有一个Concrete Decorator类时，可以将Decorator和Concrete Decorator合并。

![/assets/images/2012/07/gof_decorator_3.jpg](/assets/images/2012/07/gof_decorator_3.jpg)

适用性：

以下情况使用Decorator模式

1. 需要扩展一个类的功能，或给一个类添加附加职责。
2. 需要动态的给一个对象添加功能，这些功能可以再动态的撤销。
3. 需要增加由一些基本功能的排列组合而产生的非常大量的功能，从而使继承关系变的不现实。
4. 当不能采用生成子类的方法进行扩充时。一种情况是，可能有大量独立的扩展，为支持每一种组合将产生大量的子类，使得子类数目呈爆炸性增长。另一种情况可能是因为类定义被隐藏，或类定义不能用于生成子类。

优点：

1. Decorator模式与继承关系的目的都是要扩展对象的功能，但是Decorator可以提供比继承更多的灵活性。
2. 通过使用不同的具体装饰类以及这些装饰类的排列组合，设计师可以创造出很多不同行为的组合。

缺点：

1. 这种比继承更加灵活机动的特性，也同时意味着更加多的复杂性。
2. 装饰模式会导致设计中出现许多小类，如果过度使用，会使程序变得很复杂。
3. 装饰模式是针对抽象组件（Component）类型编程。但是，如果你要针对具体组件编程时，就应该重新思考你的应用架构，以及装饰者是否合适。当然也可以改变Component接口，增加新的公开的行为，实现“半透明”的装饰者模式。在实际项目中要做出最佳选择。

装饰模式在Java I/O库中的应用：

![/assets/images/2012/07/gof_decorator_4.jpg](/assets/images/2012/07/gof_decorator_4.jpg)

编写一个装饰者把所有的输入流内的大写字符转化成小写字符：

<pre>import java.io.FilterInputStream;
import java.io.IOException;
import java.io.InputStream;

public class LowerCaseInputStream extends FilterInputStream
{
	protected LowerCaseInputStream(InputStream in)
	{
		super(in);
	}

	@Override
	public int read() throws IOException
	{
		int c = super.read();
		return (c == -1 ? c : Character.toLowerCase((char) c));
	}

	@Override
	public int read(byte[] b, int offset, int len) throws IOException
	{
		int result = super.read(b, offset, len);

		for (int i = offset; i &lt; offset + result; i++)
		{
			b[i] = (byte) Character.toLowerCase((char) b[i]);
		}

		return result;

	}
}</pre>

测试我们的装饰者类：

<pre>import java.io.*;

public class InputTest
{
	public static void main(String[] args) throws IOException
	{
		int c;

		try
		{
			InputStream in = new LowerCaseInputStream(new BufferedInputStream(
					new FileInputStream("D:\\test.txt")));

			while ((c = in.read()) &gt;= 0)
			{
				System.out.print((char) c);
			}

			in.close();
		}
		catch (IOException e)
		{
			e.printStackTrace();
		}
	}
}</pre>