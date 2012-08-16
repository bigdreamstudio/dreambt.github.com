---
layout: post
category : Java
tags : [Java, beginner, i19n, resources]
title: Java国际化资源文件相关的乱码问题
wordpress_id: 1054
wordpress_url: http://www.im47.cn/?p=1054
date: 2011-11-14 18:52:10.000000000 +08:00
---
是不得不重新总结一下国际化问题的时候了，因为今天浪费了我整整一个下午的时间。

<strong>1.定义国际化资源文件</strong>

在resources目录里面新建一下文件：
<pre>ApplicationResources.properties
ApplicationResources_zh_CN.properties</pre>
文件的内容格式为“key=value”

<strong>2.在代码中调用国际化资源文件</strong>
<pre>// 声明资源包
private ResourceBundle bundle;

// 通过类加载器获取资源包
bundle = PropertyResourceBundle.getBundle("ApplicationResources");

// 获取 i18n 字符串
bundle.getString("ApplicationTitle")</pre>
国际化就这么简单，上面的内容只是为了本文的完整性。当你运行程序的时候会发现乱码，MyEclipse等集成开发环境除外。下面是讲解重点：

<strong>出现乱码的原因</strong>

Property 文件中，使用的编码方式根据机器本身的设置可能是GBK或者UTF-8。而在Java程序中读取Property文件的时候使用的是Unicode编码方 式，这种编码方式不同会导致中文乱码。因此需要将Property文件中的中文字符转化成Unicode编码方式才能正常显示中文。

有的IDE会自动给你转码，而有的不会。在不知情的情况下可能转码2次，也可能发生转码不对的情况。这是要注意检查资源文件的编码格式，同时利用我后面提供的批处理文件验证自己转码是否正确，一时解决不了的话可以直接用批处理的输出结果替换到打包文件。

<strong>3.使用 native2ascii 工具将资源文件转为 Unicode 编码</strong>
<pre>native2ascii -[options] [inputfile [outputfile]]
说明：
-[options]：表示命令开关，有两个选项可供选择
-reverse：将Unicode编码转为本地或者指定编码，不指定编码情况下，将转为本地编码。
-encoding encoding_name：转换为指定编码，encoding_name为编码名称。
[inputfile [outputfile]]
inputfile：表示输入文件全名。
outputfile：输出文件名。如果缺少此参数，将输出到控制台。</pre>
下面是我提供给大家的一个小工具 <a href="http://filemarkets.com/file/dreambt/a54ccf2e/" target="_blank">i18n.bat</a>，可以批量转换指定目录下所有资源文件（请不要修改版权信息，谢谢！版本更新恕不通知）

<strong>4.通过 Maven 管理国际化资源文件</strong>

也许你比我还懒，批处理都不愿意使用。好吧，我再提供一种更好的办法来满足你。

抱歉：我的编辑器将尖括号转码了，请您自己用批量代替的方法转换一下吧。
<pre>        

                src/main/resources
                *_zh*.properties
                true

                org.codehaus.mojo
                native2ascii-maven-plugin
                1.0-beta-1

                            native2ascii

                            GBK

                                *_zh*.properties</pre>
至此，Java下的国际化问题就完美解决了，如果您有什么问题欢迎回复~
