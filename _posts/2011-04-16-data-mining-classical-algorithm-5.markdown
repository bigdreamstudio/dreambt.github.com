---
layout: post
category : Algorithm
tags : [DataMining, Algorithm]
title: 数据挖掘十大经典算法(5) 最大期望(EM)算法
wordpress_id: 907
wordpress_url: http://www.im47.net/?p=907
date: 2011-04-16 19:36:49.000000000 +08:00
---
在统计计算中，最大期望（EM，Expectation–Maximization）算法是在概率（probabilistic）模型中寻找参数最大似然估计的算法，其中概率模型依赖于无法观测的隐藏变量（Latent Variabl）。最大期望经常用在机器学习和计算机视觉的数据集聚（Data Clustering）领域。最大期望算法经过两个步骤交替进行计算，第一步是计算期望（E），也就是将隐藏变量象能够观测到的一样包含在内从而计算最大似然的期望值；另外一步是最大化（M），也就是最大化在 E 步上找到的最大似然的期望值从而计算参数的最大似然估计。M 步上找到的参数然后用于另外一个 E 步计算，这个过程不断交替进行。
<h2>最大期望过程说明</h2>
我们用 <img src="http://upload.wikimedia.org/math/f/f/5/ff58c8e0e55b508d25fa7aff97d497b1.png" alt="\textbf{y}" /> 表示能够观察到的不完整的变量值，用 <img src="http://upload.wikimedia.org/math/f/2/a/f2a48e1cd2da440643ea07a3b2f60e6f.png" alt="\textbf{x}" /> 表示无法观察到的变量值，这样 <img src="http://upload.wikimedia.org/math/f/2/a/f2a48e1cd2da440643ea07a3b2f60e6f.png" alt="\textbf{x}" /> 和 <img src="http://upload.wikimedia.org/math/f/f/5/ff58c8e0e55b508d25fa7aff97d497b1.png" alt="\textbf{y}" /> 一起组成了完整的数据。<img src="http://upload.wikimedia.org/math/f/2/a/f2a48e1cd2da440643ea07a3b2f60e6f.png" alt="\textbf{x}" />可能是实际测量丢失的数据，也可能是能够简化问题的隐藏变量，如果它的值能够知道的话。例如，在<a title="混合模型" href="http://zh.wikipedia.org/w/index.php?title=%E6%B7%B7%E5%90%88%E6%A8%A1%E5%9E%8B&amp;action=edit&amp;redlink=1">混合模型</a>（<a title="en:mixture model" href="http://en.wikipedia.org/wiki/mixture_model">Mixture Model</a>）中，如果“产生”样本的混合元素成分已知的话最大似然公式将变得更加便利（参见下面的例子）。

<a id=".E4.BC.B0.E8.AE.A1.E6.97.A0.E6.B3.95.E8.A7.82.E6.B5.8B.E7.9A.84.E6.95.B0.E6.8D.AE" name=".E4.BC.B0.E8.AE.A1.E6.97.A0.E6.B3.95.E8.A7.82.E6.B5.8B.E7.9A.84.E6.95.B0.E6.8D.AE"></a>
<h3>估计无法观测的数据</h3>
让 <img src="http://upload.wikimedia.org/math/5/a/3/5a34bb082daf037b3c4b14c13af6855b.png" alt="p\," /> 代表矢量 θ: <img src="http://upload.wikimedia.org/math/e/8/7/e8761ada1a3ed08273a0b5659a07de7a.png" alt="p( \mathbf y, \mathbf x | \theta)" /> 定义的参数的全部数据的<a title="概率分布" href="http://zh.wikipedia.org/wiki/%E6%A6%82%E7%8E%87%E5%88%86%E5%B8%83">概率分布</a>（连续情况下）或者<a title="概率集聚函数" href="http://zh.wikipedia.org/w/index.php?title=%E6%A6%82%E7%8E%87%E9%9B%86%E8%81%9A%E5%87%BD%E6%95%B0&amp;action=edit&amp;redlink=1">概率集聚函数</a>（离散情况下），那么从这个函数就可以得到全部数据的<a title="最大似然值" href="http://zh.wikipedia.org/w/index.php?title=%E6%9C%80%E5%A4%A7%E4%BC%BC%E7%84%B6%E5%80%BC&amp;action=edit&amp;redlink=1">最大似然值</a>，另外，在给定的观察到的数据条件下未知数据的<a title="条件分布" href="http://zh.wikipedia.org/w/index.php?title=%E6%9D%A1%E4%BB%B6%E5%88%86%E5%B8%83&amp;action=edit&amp;redlink=1">条件分布</a>可以表示为：
