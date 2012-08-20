---
layout: post
category : Web_Design
tags : [coder, skill, programer]
title: 任何Web开发人员需要必备的技巧
wordpress_id: 97
wordpress_url: http://www.im47cn.nt102.idcwind.net/wordpress/?p=97
date: 2008-09-15 16:52:42.000000000 +08:00
---
开发Web应用程序的技术已经变得更成熟、更复杂了。现在，构建一个Web应用程序不仅仅需要简单的HTML技术了。数据库访问、脚本语言和管理都是一个Web程序员需要具备的技术。让我们来看看要成为一个市场上受欢迎的Web开发人员都需要些什么技能吧。

自从CERN（欧洲粒子物理研究所），日内瓦附近的高能物理研究中心，在1991年发布了Web以来，Web技术已经从静态的内容和Common Gateway Interface（CGI）发展成servlet技术和JavaServer Pages了。然而，在这个竞争更激烈的社会中，一个Web程序员需要更多的知识。例如，如果在面试中，你提到你熟悉XML并在JNDI方面有些经验（这两种技术初看似乎同Web编程没有很紧密的关系），那么你就会给你未来的老板留下更深的印象。设想你已经了解了Java编程语言和面向对象的编程，下面还有两组技术是一个Web开发人员日常工作中所需要的。第一组包括每个Web程序员必须具备的技术。第二组包含要想成为一个高级程序员所应该掌握的技术。

基本技能
如果想称自己是个Web开发人员，下面就是必须具备的技术。

HTML（HyperText Markup Language）
HTML几乎是显示在浏览器上所有内容的语言。难怪HTML就好像是一个Web程序员的生存本能一样。如果你仍需要在你的HTML中查找<tr>或<b>，那么你真的需要提高你的HTML技术了。HTML的当前版本是4.01，你可以从http://www.w3.org/TR/1999/REC-html401-19991224/了解更多关于它的内容。

Servlets和JSP
Java servlet技术是开发Java Web应用程序的主要技术。它是由Sun Microsystems在1996年开发的，当前的版本是2.3，但人们正在为版本2.4做准备。

JSP是servlet技术的扩展，现在的版本是1.2（2.0版将很快定下来）。有人认为JSP是servlets的替代，但实际并不是这样的。Servlets和JSP是一起用于复杂的Web应用程序的。

用Java进行Web编程的一个好的开端就是学习servlet技术。即使你打算在你的Web应用程序中只运用JSP页面，你仍需要学习servlet技术。在更复杂的Web应用程序中，JSP页面只用于显示，而JavaBeans和自定义标签库用来嵌入商业逻辑。即：你也必须精通JavaBeans和自定义标签库。


JavaScript
JavaScript是运行于所有的主要的浏览器中的脚本语言。你用JavaScript来进行客户端的编程。客户端编程中最重要的工作就是确认用户输入。运用客户端输入验证的好处是减少服务器的工作量并提高响应时间。另外，JavaScript可以用于重新定向（redirection）、cookie处理、控制applets、创建导航树、打开一个浏览器的一个新的实例、等等。

SQL（Strutured Query Language）和JDBC（Java Database Connectivity）
如今，大多数Web应用程序都包括访问关系数据库中的数据。作为一个Web程序员，你需要知道如何存储、得到并操作数据库中的数据。有时侯，你也需要设计数据库，构建数据库中的表和其它结构。SQL就是用来操作数据库中数据的语言。你通常需要编写SQL语句（常常是动态的），把它们传递到数据库服务器，并得到返回的数据（如果有的话）。

运用Java语言，你需要用JDBC来帮助Web应用程序和数据库服务器进行通讯。JDBC有两部分：JDBC Core API（Application Programming Interface)和JDBC Optional Package API。第一组用来执行基本的数据操作，如创建一个连接或读取、更新并删除一个表中的记录。第二组提供更高级的数据库连接功能，如连接池、事务和RowSet。JDBC的当前版本是3.0，API包含在J2SE v. 1.4中。

Web Container管理和应用程序部署
你的servlets和JSP页面在一个叫做servlet/JSP container或Web container的引擎中运行。你至少需要知道如何为测试以及生产运行部署你的Web资源。例如，如果你运用Tomcat，你需要了解的一件事就是如何映射配置文件（server.xml）中的应用程序，使Tomcat知道如何调用你的JSP页面。另外，你需要知道在哪里保存你的库以及如何创建应用程序部署描述符。

XML（eXtensible Markup Language）
XML是计算机领域中一个成功的后起之秀。由World Wide Web Consortium在1996年开发，XML现在已经是用于数据交换和可扩展数据结构的一个广泛的、公认的标准了。XML在Java Web开发中扮演着一个重要的角色。例如，每个应用程序的部署描述符都是XML格式的。而且，如果你在开发Web servies，你就会用到SOAP（Simple Object Access Protocol），它主要是基于HTTP和XML的。

另外，在Web应用程序中，XML也可能用于存储分等级的数据。

Model 2结构
这种技术在该类别中是最先进的。建议用这种结构来构建相当复杂的Java Web应用程序。Model 2结构是基于Model-View-Controller设计范例的。

高级技术
下面这些技术可以将你同初学者区别开来。

JSTL（JSP Standard Tag Libraries）、Jakarta Taglibs项目和其它库
为了加速应用程序的开发，你应该经常重用代码。简单地说，代码重用就是，如果有人已经编写了用来执行某些功能的代码，你最好就去用那些代码，而不要自己编写了。因此，JSP可以让你运用自定义标签。你可以运用几个库，最受欢迎的是Apache的Jakarta Taglibs项目中的库。从http://jakarta.apache.org/taglibs/index.html 可以下载这个包，你在开始创建新类前，可以运用在这个包中找到的任何现成的东西。

JSTL最近已经成为了一个标准。其它标签库可以免费或以商业方式得到。

Apache的Struts项目
Struts是一个Apache赞助的公共资源项目，它为构建Model 2 Java Web应用程序提供了一个构架。Struts为MVC结构提供它自己的Controller组件，将EJB、JDBC和JNDI用于Model，将JSP和其它技术用于View。你可以从它的网站找到更多关于这个项目的更详细的信息：http://jakarta.apache.org/struts/index.html。

XHTML（Extensible HyperText Markup Language）
XHTML是努力将HTML和XML结合起来的一种技术。你可以把XHTML当作下一代的HTML。其当前的版本是1.0（第二版是于2002年8月1日发布的），XHTML还没有像HTML那么流行，但它在将来会发挥更重要的作用。根据Web设计专家Molly Holzschlag的观点，推动各个公司转向XHTML的主要原因是美国的关于公开访问（accessibility）的法律。更多关于XHTML的信息，参阅Holzschlag访谈。

DHTML（动态HTML）
DHTML可以允许人们在你的网站上进行更多的交互。例如，运用DHTML，当用户移动鼠标到一个链接上时，你就可以很容易地创建并显示子菜单。运用DHTML的最大的挑战是创建跨浏览器的页面。的确，在理论上，页面设计应该是由美工处理的，其中动态的HTML是通过运用一个工具而产生的。然而，一个Web程序员通常要负责集成所有的部分，如果在页面中生成的代码被破坏了，你就需要了解DHTML来修理它。

Applet 编程
Applets曾经在提供交互性方面很重要，尤其在DHTML出现前。现在，applets的作用被削减了，更多的程序员已经不用applets了。Microsoft决定在它的新浏览器中不为applets提供缺省的支持极大地削减了applets在Web应用程序中的作用。然而，applets并没有消亡。对于某些任务，如显示新闻标题，applets仍然是不可替代的，而且applets不会产生另人头痛的跨浏览器兼容方面的问题。

HTTP协议
Java Web程序员通常运用比HTTP更高的协议，如运用servlet和JSP APIs。这些APIs隐藏了HTTP协议的复杂性。因此，你仍可以构建重要的应用程序而不需要知道多少关于HTTP协议的知识。只有当你需要处理原始数据，比如将文件作为附件上载或传送时，你才需要更多关于协议的知识。

EJB（Enterprise JavaBeans）
EJB是J2EE的一部分，当可扩展性和强大性是你的Web应用程序的主要需求时，EJB就很重要。在当前规范（EJB 2.0）中有三种类型的EJBs：会话（session）EJBs、实体（entity）EJBs和消息驱动的（message driven）EJBs。新的规范，2.1版，正在设计中。

JNDI（Java Naming and Directory Interface）
当你在开发企业beans时，JNDI很重要，因为对一个EJB的访问是通过JNDI的命名服务完成的。运用一个命名服务来查找与一个特定名字相关的一个对象。在EJB context中，一个命名服务找到一个企业bean，给定这个bean的名字。因此，了解JNDI在开发一个EJB应用程序中是至关重要的。另外，JDBC可以用JNDI来访问一个关系数据库。

其它工具
了解在哪里可以找到特定的支持工具通常有助于的你的事业的发展。例如，如果你碰巧被分配去做关于基准的任务，那么你如果知道你可以从Apache的Jakarta Project下载Jmeter，你就会很高兴。另外，如果你需要以PDF格式发送输出结果，建议你从http://www.lowagie.com/iText/ 运用可以免费下载的Java-PDF库。Internet技术范围很广而且发展很快。这就是说，作为一个Web程序员，你应该时时留心业界出现了什么新技术，发生了什么大事。在这个方面，没有什么比Internet本身更伟大的资源了。</b></tr>
