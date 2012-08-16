---
layout: post
category : J2EE
tags : [intro, beginner, J2EE, Tomcat, HTTP]
title: Tomcat处理HTTP请求源码分析（下）
wordpress_id: 1082
wordpress_url: http://www.im47.cn/?p=1082
date: 2011-12-15 11:38:34.000000000 +08:00
---
很多开源应用服务器都是集成tomcat作为web container的，而且对于tomcat的servlet container这部分代码很少改动。这样，这些应用服务器的性能基本上就取决于Tomcat处理HTTP请求的connector模块的性能。本文首先从应用层次分析了tomcat所有的connector种类及用法，接着从架构上分析了connector模块在整个tomcat中所处的位置，最后对connector做了详细的源代码分析。并且我们以Http11NioProtocol为例详细说明了tomcat是如何通过实现ProtocolHandler接口而构建connector的。
<h2>4 如何实现Connector</h2>
由上面的介绍我们可以知道，实现Connector就是实现ProtocolHander接口的过程。

AjpAprProtocol、AjpProtocol、Http11AprProtocol、Http11Protocol、JkCoyoteHandler、MemoryProtocolHandler这些实现类的实现流程与Http11NioProtocol相同，下面我们以Http11NioProtocol为类重点说明tomcat中如何实现ProtocolHander接口的。
<p style="text-align: center;">Http11NioProtocol实现了ProtocolHander接口，它将所有的操作委托给NioEndpoint类去做，如下图：
<img class="aligncenter" src="http://pic.yupoo.com/dreambt_v/BBKs3XQv/medium.jpg" alt="image1" width="404" height="263" /></p>
<p style="text-align: center;">NioEndpoint类中的init方法中首先以普通阻塞方式启动了SocketServer：
<img class="aligncenter" src="http://pic.yupoo.com/dreambt_v/BBKs4dsZ/medium.jpg" alt="image2" width="500" height="69" /></p>
<p style="text-align: center;">NioEndpoint类的start方法是关键，如下：
<img class="aligncenter" src="http://pic.yupoo.com/dreambt_v/BBKs4kjm/medium.jpg" alt="image3" width="500" height="420" /></p>
可以看出，在start方法中启动了两个线程和一个线程池：
<ul>
	<li>Acceptor线程，该线程以普通阻塞方式接收客户端请求（socket.accep()），将客户Socket交由线程池是处理，线程池要将该Socket配置成非阻塞模式（socket.configureBlocking(false)）,并且向Selector注册READ事件。该线程数目可配置，默认为1个。</li>
	<li>Poller线程，由于Acceptor委托线程为客户端Socket注册了READ事件，当READ准备好时，就会进入Poller线程的循环，Poller线程也是委托线程池去做，线程池将NioChannel加入到ConcurrentLinkedQueue&lt;NioChannel&gt;队列中。该线程数目可配置，默认为1个。</li>
	<li>线程池，就是上面说的做Acceptor与Poller线程委托要做的事情。</li>
</ul>
<h3>4.1 Init接口实现方法中阻塞方式启动ServerSocketChannel</h3>
<p style="text-align: center;">在Init接口实现方法中阻塞方式启动ServerSocketChannel。
<img class="aligncenter" src="http://pic.yupoo.com/dreambt_v/BBKs4LVD/medium.jpg" alt="image4" width="500" height="66" /></p>

<h3>4.2 Start接口实现方法中启动所有线程</h3>
<p style="text-align: center;">Start方法中启动了线程池，acceptor线程与Poller线程。其中acceptor与poller线程一般数目为1，当然，数目也可配置。
<img class="aligncenter" src="http://pic.yupoo.com/dreambt_v/BBKs4Io9/medium.jpg" alt="image5" width="500" height="368" /></p>
可以看出，线程池有两种实现方式：
<ul>
	<li>普通queue + wait + notify方式，默认使用的方式，据说实际测试这种比下种效率高</li>
	<li>JDK1.5自带的线程池方式</li>
</ul>
<h3>4.3 Acceptor线程接收客户请求、注册READ事件</h3>
在Acceptor线程中接收了客户请求，同时委托线程池注册READ事件。
<ol>
	<li>在Acceptior线程中接收了客户请求（serverSock.accept()）</li>
</ol>
<p style="text-align: center;"><img class="aligncenter" src="http://pic.yupoo.com/dreambt_v/BBKs4SNn/medium.jpg" alt="image6" width="500" height="64" /></p>

<ol>
	<li>委托线程池处理</li>
</ol>
<p style="text-align: center;"><img class="aligncenter" src="http://pic.yupoo.com/dreambt_v/BBKs54Yu/medium.jpg" alt="image7" width="500" height="218" /></p>

<ol>
	<li>在线程池的Worker线程的run方法中有这么几句：</li>
</ol>
<p style="text-align: center;"><img class="aligncenter" src="http://pic.yupoo.com/dreambt_v/BBKs5nvc/medium.jpg" alt="image8" width="500" height="309" /></p>
<p style="text-align: center;">在setSocketOptions方法中，首先将socket配置成非阻塞模式：
<img class="aligncenter" src="http://pic.yupoo.com/dreambt_v/BBKs5MJX/medium.jpg" alt="image9" width="500" height="100" /></p>
<p style="text-align: center;">在setSocketOptions方法中，最后调用getPoller0().register(channel);一句为SocketChannel注册READ事件，register方法代码如下(注意：这是Poller线程的方法)：
<img class="aligncenter" src="http://pic.yupoo.com/dreambt_v/BBKs5L3z/medium.jpg" alt="image10" width="500" height="202" /></p>
<p style="text-align: center;">其中attachment的结构如下，它可以看做是一个共享的数据结构：
<img class="aligncenter" src="http://pic.yupoo.com/dreambt_v/BBKs68j1/medium.jpg" alt="image11" width="500" height="478" /></p>

<h3>4.4 Poller线程读请求、生成响应数据、注册WRITE事件</h3>
<ol>
	<li>在上面说的setSocketOptions方法中调用Poller线程的register方法注册读事件之后，当READ准备就绪之后，就开始读了。下面代码位于Poller线程的run方法之中：</li>
</ol>
<p style="text-align: center;"><img class="aligncenter" src="http://pic.yupoo.com/dreambt_v/BBKs6sz3/medium.jpg" alt="image12" width="500" height="461" /></p>
<p style="text-align: center;">可以看到，可读之后调用processSocket方法，该方法将读处理操作委拖给线程池处理(注意此时加入到线程池的是NioChannel，不是SocketChannel)：
<img class="aligncenter" src="http://pic.yupoo.com/dreambt_v/BBKs6RDS/medium.jpg" alt="image13" width="500" height="217" /></p>
<p style="text-align: center;">线程池的Worker线程中的run方法中的部分代码如下（请注意handler.process(socket)这一句）：
<img class="aligncenter" src="http://pic.yupoo.com/dreambt_v/BBKs6TbB/medium.jpg" alt="image14" width="500" height="403" /></p>
注意：
<ol>
<ul>
	<li>调用了hanler.process(socket)来生成响应数据）</li>
	<li>数据生成完之后，注册WRITE事件的，代码如下：</li>
</ul>
</ol>
<p style="text-align: center;"><img class="aligncenter" src="http://pic.yupoo.com/dreambt_v/BBKs779P/medium.jpg" alt="image15" width="500" height="262" /></p>

<h3>4.5 Handle接口实现类通过Adpater调用Servlet容器生成响应数据</h3>
<p style="text-align: center;">NioEndpoint类中的Handler接口定义如下：
<img class="aligncenter" src="http://pic.yupoo.com/dreambt_v/BBKs7lDm/medium.jpg" alt="image16" width="500" height="105" /></p>
<p style="text-align: center;">其中process方法通过Adapter来调用Servlet Container生成返回结果。Adapter接口定义如下：
<img class="aligncenter" src="http://pic.yupoo.com/dreambt_v/BBKs7xxM/medium.jpg" alt="image17" width="500" height="319" /></p>

<h3>4.6 小结</h3>
实现一个tomcat连接器Connector就是实现ProtocolHander接口的过程。Connector用来接收Socket Client端的请求，通过内置的线程池去调用Servlet Container生成响应结果，并将响应结果同步或异步的返回给Socket Client。在第三方应用集成tomcat作为Web容器时，一般不会动Servlet Container端的代码，那么connector的性能将是整个Web容器性能的关键。

--------------

转载自：

<a href="http://www.infoq.com/cn/articles/zh-tomcat-http-request-1" target="_blank">http://www.infoq.com/cn/articles/zh-tomcat-http-request-1</a>

<a href="http://www.infoq.com/cn/articles/zh-tomcat-http-request-2" target="_blank">http://www.infoq.com/cn/articles/zh-tomcat-http-request-2</a>
