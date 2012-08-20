---
layout: post
category : Java
tags : [Java, how-to]
title: 如何打包JAVA程序以及生成可执行文件
wordpress_id: 562
wordpress_url: http://www.im47.cn/?p=562
date: 2010-10-09 21:19:35.000000000 +08:00
---
首先，选择好你即将发布的.class二进制文件，使用如下命令：

[java]javac *.java[/java]

然后，创建jar文件(这里讲解的是打jar包)。这里我用一个名字叫做test.class的文件来举例，另外注意还要编写一个确定main_class（主类）的文件manifest.mf（编写方法见后面）。在这里mainfest.mf和test.class是在同一个目录下，然后使用如下命令：

[java]jar cvfm test.jar manifest.mf test[/java]

这样，一个test.jar文件就生成了。但为了确保成功，我们可以再用下面的指令执行一下刚刚生成的test.jar：

[java]java -jar test.jar[/java]
<blockquote>打开Java的JAR文件我们经常可以看到文件中包含着一个META-INF目录，这个目录下会有一些文件，其中必有一个MANIFEST.MF，这个文件描述了该Jar文件的很多信息，下面将详细介绍MANIFEST.MF文件的内容，先来看struts.jar中包含的MANIFEST.MF文件内容：
Manifest-Version: 1.0
Created-By: Apache Ant 1.5.1
Extension-Name: Struts Framework
Specification-Title: Struts Framework
Specification-Vendor: Apache Software Foundation
Specification-Version: 1.1
Implementation-Title: Struts Framework
Implementation-Vendor: Apache Software Foundation
Implementation-Vendor-Id: org.apache
Implementation-Version: 1.1
Class-Path:  commons-beanutils.jar commons-collections.jar commons-digester.jar commons-logging.jar commons-validator.jar jakarta-oro.jar struts-legacy.jar

如果我们把MANIFEST中的配置信息进行分类，可以归纳出下面几个大类：

一. 一般属性

1. Manifest-Version
用来定义manifest文件的版本，例如：Manifest-Version: 1.0

2. Created-By
声明该文件的生成者，一般该属性是由jar命令行工具生成的，例如：Created-By: Apache Ant 1.5.1

3. Signature-Version
定义jar文件的签名版本

4. Class-Path
应用程序或者类装载器使用该值来构建内部的类搜索路径

二. 应用程序相关属性

1. Main-Class
定义jar文件的入口类，该类必须是一个可执行的类，一旦定义了该属性即可通过 java -jar x.jar来运行该jar文件。

三. 小程序(Applet)相关属性

1. Extendsion-List
该属性指定了小程序需要的扩展信息列表，列表中的每个名字对应以下的属性
2. &lt;extension&gt;-Extension-Name
3. &lt;extension&gt;-Specification-Version
4. &lt;extension&gt;-Implementation-Version
5. &lt;extension&gt;-Implementation-Vendor-Id
5. &lt;extension&gt;-Implementation-URL

四. 扩展标识属性

1. Extension-Name
该属性定义了jar文件的标识，例如Extension-Name: Struts Framework

五. 包扩展属性

1. Implementation-Title   定义了扩展实现的标题
2. Implementation-Version   定义扩展实现的版本
3. Implementation-Vendor   定义扩展实现的组织
4. Implementation-Vendor-Id   定义扩展实现的组织的标识
5. Implementation-URL :   定义该扩展包的下载地址(URL)
6. Specification-Title   定义扩展规范的标题
7. Specification-Version   定义扩展规范的版本
8. Specification-Vendor   声明了维护该规范的组织
9. Sealed   定义jar文件是否封存，值可以是true或者false (这点我还不是很理解)

六. 签名相关属性

签名方面的属性我们可以来参照JavaMail所提供的mail.jar中的一段

Name: javax/mail/Address.class
Digest-Algorithms: SHA MD5
SHA-Digest: AjR7RqnN//cdYGouxbd06mSVfI4=
MD5-Digest: ZnTIQ2aQAtSNIOWXI1pQpw==

这段内容定义类签名的类名、计算摘要的算法名以及对应的摘要内容(使用BASE64方法进行编码)

七.自定义属性

除了前面提到的一些属性外，你也可以在MANIFEST.MF中增加自己的属性以及响应的值，例如J2ME程序jar包中就可能包含着如下信息

MicroEdition-Configuration: CLDC-1.0
MIDlet-Name: J2ME_MOBBER Midlet Suite
MIDlet-Info-URL: <a href="http://www.javayou.com/">http://www.javayou.com/</a>
MIDlet-Icon: /icon.png
MIDlet-Vendor: Midlet Suite Vendor
MIDlet-1: mobber,/icon.png,mobber
MIDlet-Version: 1.0.0
MicroEdition-Profile: MIDP-1.0
MIDlet-Description: Communicator

关键在于我们怎么来读取这些信息呢？其实很简单，JDK给我们提供了用于处理这些信息的API，详细的信息请见java.util.jar包中，我们可以通过给JarFile传递一个jar文件的路径，然后调用JarFile的getManifest方法来获取Manifest信息。
更详细关于JAR文件的规范请见
<a href="http://java.sun.com/j2se/1.3/docs/guide/jar/jar.html">http://java.sun.com/j2se/1.3/docs/guide/jar/jar.html</a></blockquote>
怎么样，是不是可以很顺利的进行？如果是，那我们就可以开始进行可执行文件的创建了。

下面打开exe4j，它的开始一个界面是这样的：
<p style="text-align: center;"><img class="aligncenter size-full wp-image-563" title="2010-10-09_210226" src="wp-content/uploads/2010/10/2010-10-09_210226.jpg" alt="" width="584" height="440" /></p>
单击NEXT键，选择”JAR in EXE”mod按钮，单击NEXT；填写短名和文件输出的路径然后点击NEXT继续。

在下面的画面中，你可以选择你要生成的可执行文件的类型，以及生成的可执行文件名称、可执行文件的图标等，我们在这里就选择GUI application，名字就根据自己的需要取一个，图标你可以自己在你的图标库里选一个你喜欢的，然后再NEXT再继续；

下一个画面里填写Main class的名字，单击下面的绿色+选择所需的.jar文件,如果没有特殊要求我们就可以再继续了；

接下来是选择版本的画面，填写好自己的最大最小版本然后再继续

下面这个画面可以帮助你设一下你的文件执行的片头，增加其美观效果，选择自己喜欢的图片，写自己想写的文字，并可以根据需要调整文字的位置，再继续

下面是一个选择语言版本的界面选好后再继续

下面是一个短暂的等待，

然后就大功告成了。
