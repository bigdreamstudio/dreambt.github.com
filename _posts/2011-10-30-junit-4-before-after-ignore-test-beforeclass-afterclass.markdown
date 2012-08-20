---
layout: post
category : Test
tags : [Test, JUnit]
title: JUnit 4 中的Before After Ignore Test BeforeClass AfterClass
wordpress_id: 1048
wordpress_url: http://www.im47.cn/?p=1048
date: 2011-10-30 12:01:54.000000000 +08:00
---
JUnit 4 使用 Java 5 中的注解（annotation），以下是JUnit 4 常用的几个 annotation 介绍
<ul>
	<li><span class="Apple-style-span" style="line-height: 18px;">@BeforeClass：针对所有测试，只执行一次，且必须为static void</span></li>
	<li><span class="Apple-style-span" style="line-height: 18px;">@Before：初始化方法</span></li>
	<li><span class="Apple-style-span" style="line-height: 18px;">@Test：测试方法，在这里可以测试期望异常和超时时间</span></li>
	<li><span class="Apple-style-span" style="line-height: 18px;">@Ignore：忽略的测试方法</span></li>
	<li><span class="Apple-style-span" style="line-height: 18px;">@After：释放资源
</span></li>
	<li><span class="Apple-style-span" style="line-height: 18px;">@AfterClass：针对所有测试，只执行一次，且必须为static void</span></li>
</ul>
一个JUnit 4 的单元测试用例执行顺序为：
<blockquote>@BeforeClass –&gt; @Before –&gt; @Test –&gt; @After –&gt; @AfterClass</blockquote>
每一个测试方法的调用顺序为：
<blockquote>@Before –&gt; @Test –&gt; @After</blockquote>
写个例子测试一下，测试一下
<pre>package net.im47.demo.junit;

import org.junit.*;
import static org.junit.Assert.*;

public class JUnit4Test {
    @Before
    public void before() {
        System.out.println("@Before");
    }

    @Test
    public void test() {
        System.out.println("@Test");
        assertEquals(5 + 5, 10);
    }

    @Ignore
    @Test
    public void testIgnore() {
        System.out.println("@Ignore");
    }

    @Test(timeout = 50)
    public void testTimeout() {
        System.out.println("@Test(timeout = 50)");
        assertEquals(5 + 5, 10);
    }

    @Test(expected = ArithmeticException.class)
    public void testExpected() {
        System.out.println("@Test(expected = Exception.class)");
        throw new ArithmeticException();
    }

    @After
    public void after() {
        System.out.println("@After");
    }

    @BeforeClass
    public static void beforeClass() {
        System.out.println("@BeforeClass");
    }

    @AfterClass
    public static void afterClass() {
        System.out.println("@AfterClass");
    }
}</pre>
<strong>输出结果</strong>
<blockquote>
<pre>@BeforeClass
@Before
@Test
@After
Test 'net.im47.demo.junit.JUnit4Test.testIgnore' ignored
@Before
@Test(timeout = 50)
@After
@Before
@Test(expected = Exception.class)
@After
@AfterClass</pre>
</blockquote>
<strong>对比：</strong>

@BeforeClass 和 @AfterClass 对于那些比较“昂贵”的资源的分配或者释放来说是很有效的，因为他们只会在类中被执行一次。相比之下对于那些需要在每次运行之前都要初始化或者在运行之后都需要被清理的资源来说使用@Before和@After同样是一个比较明智的选择。
<table border="1" cellspacing="0">
<tbody>
<tr>
<td align="left" valign="top"><span style="color: #504e53; font-family: 'Lucida Grande', 'Lucida Sans Unicode', verdana, arial, Tahoma, Verdana, sans-serif; font-size: x-small;">@BeforeClass and @AfterClass</span></td>
<td align="left" valign="top"><span style="color: #504e53; font-family: 'Lucida Grande', 'Lucida Sans Unicode', verdana, arial, Tahoma, Verdana, sans-serif; font-size: x-small;">@Before and @After</span></td>
</tr>
<tr>
<td align="left" valign="top"><span style="color: #504e53; font-family: 'Lucida Grande', 'Lucida Sans Unicode', verdana, arial, Tahoma, Verdana, sans-serif; font-size: x-small;">在一个类中只可以出现一次</span></td>
<td align="left" valign="top"><span style="color: #504e53; font-family: 'Lucida Grande', 'Lucida Sans Unicode', verdana, arial, Tahoma, Verdana, sans-serif; font-size: x-small;"><span class="Apple-style-span" style="line-height: normal;">在一个类中可以出现多次，即可以在多个方法的声明前加上这两个Annotaion标签，执行顺序不确定</span></span></td>
</tr>
<tr>
<td align="left" valign="top"><span style="color: #504e53; font-family: 'Lucida Grande', 'Lucida Sans Unicode', verdana, arial, Tahoma, Verdana, sans-serif; font-size: x-small;">方法名不做限制</span></td>
<td align="left" valign="top"><span style="color: #504e53; font-family: 'Lucida Grande', 'Lucida Sans Unicode', verdana, arial, Tahoma, Verdana, sans-serif; font-size: x-small;">方法名不做限制</span></td>
</tr>
<tr>
<td align="left" valign="top"><span style="color: #504e53; font-family: 'Lucida Grande', 'Lucida Sans Unicode', verdana, arial, Tahoma, Verdana, sans-serif; font-size: x-small;">在类中只运行一次</span></td>
<td align="left" valign="top"><span style="color: #504e53; font-family: 'Lucida Grande', 'Lucida Sans Unicode', verdana, arial, Tahoma, Verdana, sans-serif; font-size: x-small;">在每个测试方法之前或者之后都会运行一次</span></td>
</tr>
<tr>
<td align="left" valign="top"><span style="color: #504e53; font-family: 'Lucida Grande', 'Lucida Sans Unicode', verdana, arial, Tahoma, Verdana, sans-serif; font-size: x-small;"><span class="Apple-style-span" style="line-height: normal;">@BeforeClass父类中标识了该Annotation的方法将会先于当前类中标识了该Annotation的方法执行。
@AfterClass 父类中标识了该Annotation的方法将会在当前类中标识了该Annotation的方法之后执行</span></span></td>
<td align="left" valign="top"><span style="color: #504e53; font-family: 'Lucida Grande', 'Lucida Sans Unicode', verdana, arial, Tahoma, Verdana, sans-serif; font-size: x-small;">@Before父类中标识了该Annotation的方法将会先于当前类中标识了该Annotation的方法执行。
@After父类中标识了该Annotation的方法将会在当前类中标识了该Annotation的方法之后执行</span></td>
</tr>
<tr>
<td align="left" valign="top"><span style="color: #504e53; font-family: 'Lucida Grande', 'Lucida Sans Unicode', verdana, arial, Tahoma, Verdana, sans-serif; font-size: x-small;">必须声明为public static</span></td>
<td align="left" valign="top"><span style="color: #504e53; font-family: 'Lucida Grande', 'Lucida Sans Unicode', verdana, arial, Tahoma, Verdana, sans-serif; font-size: x-small;">必须声明为public 并且非static</span></td>
</tr>
<tr>
<td align="left" valign="top"><span style="color: #504e53; font-family: 'Lucida Grande', 'Lucida Sans Unicode', verdana, arial, Tahoma, Verdana, sans-serif; font-size: x-small;">所有标识为@AfterClass的方法都一定会被执行，即使在标识为@BeforeClass的方法抛出异常的的情况下也一样会。</span></td>
<td align="left" valign="top"><span style="color: #504e53; font-family: 'Lucida Grande', 'Lucida Sans Unicode', verdana, arial, Tahoma, Verdana, sans-serif; font-size: x-small;">所有标识为@After 的方法都一定会被执行，即使在标识为 @Before 或者 @Test 的方法抛出异常的的情况下也一样会。</span></td>
</tr>
</tbody>
</table>
