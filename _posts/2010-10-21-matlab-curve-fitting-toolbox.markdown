---
layout: post
category : Matlab
tags : [intro, beginner, Matlab, Curve fitting, Toolbox]
title: Matlab曲线拟合工具箱Curve Fitting Toolbox
wordpress_id: 686
wordpress_url: http://www.im47.cn/?p=686
date: 2010-10-21 21:59:56.000000000 +08:00
---
<h2><img class="aligncenter size-full wp-image-687" title="51092_wl_cft3_fig1_wl" src="http://www.im47.net/wp-content/uploads/2010/10/51092_wl_cft3_fig1_wl.jpg" alt="" width="476" height="354" /></h2>
<h2>简介</h2>
Curve Fitting Toolbox™ 提供用于拟合曲线和曲面数据的图形工具和命令行函数。使用该工具箱可以执行探索性数据分析，预处理和后处理数据，比较候选模型，删除偏值。您可以使用随带的线性和非线性模型库进行回归分析，也可以指定您自行定义的方程式。该库提供了优化的解算参数和起始条件，以提高拟合质量。该工具箱还提供非参数建模方法，比如样条、插值和平滑。

在创建一个拟合之后，您可以运用多种后处理方法进行绘图、插值和外推，估计置信区间，计算积分和导数。
<h2>主要功能</h2>
<ul>
	<li>用于曲线和曲面拟合的图形工具</li>
	<li>使用自定义方程求解线性和非线性回归</li>
	<li>具有优化起始点和解算参数的回归模型库</li>
	<li>插值方法，包括 B 样条、薄板样条和张量积样条</li>
	<li>平滑方法，包括平滑样条、局部回归、Savitzky-Golay 滤波和移动平均数</li>
	<li>预处理例程，包括偏值移除和分段、缩放和加权数据</li>
	<li>后处理例程，包括插值、外推、置信区间、积分和导数</li>
</ul>
<h3>拟合类型</h3>
Custom Equations：用户自定义的函数类型
Exponential：指数逼近，有2种类型， a*exp(b*x) 、 a*exp(b*x) + c*exp(d*x)
Fourier：傅立叶逼近，有7种类型，基础型是 a0 + a1*cos(x*w) + b1*sin(x*w)
Gaussian：高斯逼近，有8种类型，基础型是 a1*exp(-((x-b1)/c1)^2)
Interpolant：插值逼近，有4种类型，linear、nearest neighbor、cubic spline、shape-preserving
Polynomial：多形式逼近，有9种类型，linear ~、quadratic ~、cubic ~、4-9th degree ~
Power：幂逼近，有2种类型，a*x^b 、a*x^b + c
Rational：有理数逼近，分子、分母共有的类型是linear ~、quadratic ~、cubic ~、4-5th degree ~；此外，分子还包括constant型
Smoothing Spline：平滑逼近（翻译的不大恰当，不好意思）
Sum of Sin Functions：正弦曲线逼近，有8种类型，基础型是 a1*sin(b1*x + c1)
Weibull：只有一种，a*b*x^(b-1)*exp(-a*x^b)

详细的介绍和使用方法请查阅：<a href="http://www.mathworks.cn/products/curvefitting/">http://www.mathworks.cn/products/curvefitting/</a>
