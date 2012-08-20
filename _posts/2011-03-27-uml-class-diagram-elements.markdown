---
layout: post
category : UML
tags : [UML]
title: 全面认识UML-类图元素
wordpress_id: 884
wordpress_url: http://www.im47.net/?p=884
date: 2011-03-27 16:19:27.000000000 +08:00
---
<span style="color: #000000;">开发Java应用程序时，开发者要想有效地利用统一建模语言（UML），必须全面理解UML元素以及这些元素如何映射到Java。本文重点讨论UML类图中的元素。</span>

类图是最常用的UML图，它用于描述系统的结构化设计。其中包括类关系以及与每个类关联的属性及行为。类图能出色地表示继承与合成关系。为了将类图作为一种高效的沟通工具使用，开发者必须理解如何将类图上出现的元素转换到Java中。下面来进一步探索这一转换过程。
<h5><span style="color: #000000;">元素
</span></h5>
<span style="color: #000000;">在后面的小节中，分别讲解了类图的各个元素及其在Java中相应的表示。我会列出元素名，后续简短的代码片断和一幅图来表示元素在类图上的样子。每一节的最后简要总结了该元素。</span>
<h5><span style="color: #000000;">类（Class）
</span></h5>
<span style="color: #000000;"><span style="color: #000000;">类（<strong>图A</strong>）是对象的蓝图，其中包含3个组成部分。第一个是Java中定义的类名。第二个是属性（attributes）。第三个是该类提供的方法。</span></span>

属性和操作之前可附加一个可见性修饰符。加号（+）表示具有公共可见性。减号（-）表示私有可见性。#号表示受保护的可见性。省略这些修饰符表示具有package（包）级别的可见性。如果属性或操作具有下划线，表明它是静态的。在操作中，可同时列出它接受的参数，以及返回类型，如图A的“Java”区域所示。
<p style="text-align: center;">图A</p>
<img class="aligncenter" src="http://pic.yupoo.com/dreambt/AWReN1YN/medium.jpg" alt="image001|yupoo.com" />
<h5><span style="color: #000000;">包（Package）
</span></h5>
<span style="color: #000000;">包（图<strong>B</strong>）是一种常规用途的组合机制。UML中的一个包直接对应于Java中的一个包。在Java中，一个包可能含有其他包、类或者同时含有这两者。进行建模时，你通常拥有逻辑性的包，它主要用于对你的模型进行组织。你还会拥有物理性的包，它直接转换成系统中的Java包。每个包的名称对这个包进行了惟一性的标识。</span>
<p style="text-align: center;"><span style="color: #000000;">图B</span></p>
<img class="aligncenter" src="http://pic.yupoo.com/dreambt/AWReN40M/medium.jpg" alt="image002|yupoo.com" />
<h5><span style="color: #000000;"> 接口（Interface）
</span></h5>
<span style="color: #000000;">接口（<strong>图C</strong>）是一系列操作的集合，它指定了一个类所提供的服务。它直接对应于Java中的一个接口类型。接口既可用图C的那个图标来表示，也可由附加了&lt;&lt;interface&gt;&gt;的一个标准类来表示。通常，根据接口在类图上的样子，就能知道与其他类的关系。</span>
<p style="text-align: center;"><span style="color: #000000;">图C</span></p>
<img class="aligncenter" src="http://pic.yupoo.com/dreambt/AWReNcMM/medium.jpg" alt="image003|yupoo.com" />
<h5><span style="color: #000000;">关系
</span></h5>
<span style="color: #000000;">后面的例子将针对某个具体目的来独立地展示各种关系。虽然语法无误，但这些例子可进一步精炼，在它们的有效范围内包括更多的语义。</span>
<h5><span style="color: #000000;">依赖（Dependency）
</span></h5>
<span style="color: #000000;">实体之间一个“使用”关系暗示一个实体的规范发生变化后，可能影响依赖于它的其他实例（<strong>图D</strong>）。更具体地说，它可转换为对不在实例作用域内的一个类或对象的任何类型的引用。其中包括一个局部变量，对通过方法调用而获得的一个对象的引用（如下例所示），或者对一个类的静态方法的引用（同时不存在那个类的一个实例）。也可利用“依赖”来表示包和包之间的关系。由于包中含有类，所以你可根据那些包中的各个类之间的关系，表示出包和包的关系。</span>
<p style="text-align: center;"><span style="color: #000000;">图D</span></p>
<img class="aligncenter" src="http://pic.yupoo.com/dreambt/AWReNlzx/medium.jpg" alt="image004|yupoo.com" />
<h5><span style="color: #000000;"> 关联（Association）
</span></h5>
<span style="color: #000000;">实体之间的一个结构化关系表明对象是相互连接的。箭头是可选的，它用于指定导航能力。如果没有箭头，暗示是一种双向的导航能力。在Java中，关联（<strong>图E</strong>）转换为一个实例作用域的变量，就像图E的“Java”区域所展示的代码那样。可为一个关联附加其他修饰符。多重性（Multiplicity）修饰符暗示着实例之间的关系。在示范代码中，Employee可以有0个或更多的TimeCard对象。但是，每个TimeCard只从属于单独一个Employee。</span>
<p style="text-align: center;"><span style="color: #000000;">图E</span></p>
<img class="aligncenter" src="http://pic.yupoo.com/dreambt/AWReNsrQ/medium.jpg" alt="image005|yupoo.com" />
<h5><span style="color: #000000;">聚合（Aggregation）
</span></h5>
<span style="color: #000000;">聚合（<strong>图F</strong>）是关联的一种形式，代表两个类之间的整体/局部关系。聚合暗示着整体在概念上处于比局部更高的一个级别，而关联暗示两个类在概念上位于相同的级别。聚合也转换成Java中的一个实例作用域变量。</span>

关联和聚合的区别纯粹是概念上的，而且严格反映在语义上。聚合还暗示着实例图中不存在回路。换言之，只能是一种单向关系。
<p style="text-align: center;"><span style="color: #000000;">图F</span></p>
<img class="aligncenter" src="http://pic.yupoo.com/dreambt/AWReNxb1/medium.jpg" alt="image007|yupoo.com" />
<h5><span style="color: #000000;">合成（Composition）</span></h5>
&nbsp;

<span style="color: #000000;">合成 （<strong>图G</strong>）是聚合的一种特殊形式，暗示“局部”在“整体”内部的生存期职责。合成也是非共享的。所以，虽然局部不一定要随整体的销毁而被销毁，但整体要么负责保持局部的存活状态，要么负责将其销毁。局部不可与其他整体共享。但是，整体可将所有权转交给另一个对象，后者随即将承担生存期职责。</span>

<span style="color: #000000;">Employee和TimeCard的关系或许更适合表示成“合成”，而不是表示成“关联”。</span>
<p style="text-align: center;"><span style="color: #000000;">图G</span></p>
<img class="aligncenter" src="http://pic.yupoo.com/dreambt/AWReNG33/medium.jpg" alt="image008|yupoo.com" />
<h5><span style="color: #000000;">泛化（Generalization）
</span></h5>
<span style="color: #000000;">泛化（<strong>图H</strong>）表示一个更泛化的元素和一个更具体的元素之间的关系。泛化是用于对继承进行建模的UML元素。在Java中，用<em>extends</em>关键字来直接表示这种关系。</span>
<p style="text-align: center;"><span style="color: #000000;">图H</span></p>
<img class="aligncenter" src="http://pic.yupoo.com/dreambt/AWReNHE7/medium.jpg" alt="image009|yupoo.com" />
<h5><span style="color: #000000;"> 实现（Realization）
</span></h5>
<span style="color: #000000;">实例（<strong>图I</strong>）关系指定两个实体之间的一个合同。换言之，一个实体定义一个合同，而另一个实体保证履行该合同。对Java应用程序进行建模时，实现关系可直接用<em>implements</em>关键字来表示。</span>
<p style="text-align: center;"><span style="color: #000000;">图I</span></p>
<img class="aligncenter" src="http://pic.yupoo.com/dreambt/AWReNKDJ/medium.jpg" alt="image006|yupoo.com" />
<h5><span style="color: #000000;"> 精确映射
</span></h5>
<span style="color: #000000;">如本文所述，UML类图上的元素能精确映射到Java编程语言。开发团队的成员可利用这种精确性来加强沟通，取得对系统结构化设计的共识。</span>

<span style="color: #000000;">转载：<a href="http://www.cnblogs.com/kenjie/archive/2009/03/24/1420203.html">http://www.cnblogs.com/kenjie/archive/2009/03/24/1420203.html</a></span>
