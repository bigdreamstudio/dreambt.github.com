---
layout: post
category : Algorithm
tags : [intro, Data mining, Algorithm, CART]
title: ! '数据挖掘十大经典算法(10) CART: 分类与回归树'
wordpress_id: 914
wordpress_url: http://www.im47.net/?p=914
date: 2011-04-21 19:41:19.000000000 +08:00
---
如果一个人必须去选择在很大范围的情形下性能都好的、同时不需要应用开发者付出很多的努力并且易于被终端用户理解的分类技术的话，那么Brieman, Friedman, Olshen和Stone（1984）提出的分类树方法是一个强有力的竞争者。我们将首先讨论这个分类的过程，然后在后续的节中我们将展示这个过程是如何被用来预测连续的因变量。Brieman等人用来实现这些过程的程序被称为分类和回归树（CART, Classification and Regression Trees）方法。

分类树
在分类树下面有两个关键的思想。第一个是关于递归地划分自变量空间的想法；第二个想法是用验证数据进行剪枝。

递归划分
让我们用变量y表示因变量（分类变量），用x1, x2, x3,...,xp表示自变量。通过递归的方式把关于变量x的p维空间划分为不重叠的矩形。这个划分是以递归方式完成的。首先，一个自变量被选择，比如xi和xi的一个值si，比方说选择si把p维空间为两部分：一部分是p维的超矩形，其中包含的点都满足xi&lt;=si，另一个p维超矩形包含所有的点满足xi&gt;si。接着，这两部分中的一个部分通过选择一个变量和该变量的划分值以相似的方式被划分。这导致了三个矩形区域（从这里往后我们把超矩形都说成矩形）。随着这个过程的持续，我们得到的矩形越来越小。这个想法是把整个x空间划分为矩形，其中的每个小矩形都尽可能是同构的或“纯”的。“纯”的意思是（矩形）所包含的点都属于同一类。我们认为包含的点都只属于一个类（当然，这不总是可能的，因为经常存在一些属于不同类的点，但这些点的自变量有完全相同的值）。

更多内容参阅：

http://www.core.org.cn/NR/rdonlyres/Sloan-School-of-Management/15-062Data-MiningSpring2003/338F02AD-0DD8-4199-8727-35FCF5A15B57/0/L3ClassTrees.pdf

http://www.cqvip.com/onlineread/onlineread.asp?ID=28180864

&nbsp;
