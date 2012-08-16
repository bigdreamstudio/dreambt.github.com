---
layout: post
category : J2EE
tags : [intro, beginner, Jersey, java, J2EE, REST, Web service]
title: 构建REST风格的Web Service
wordpress_id: 1114
wordpress_url: http://www.im47.cn/?p=1114
date: 2012-03-06 18:17:01.000000000 +08:00
---
<h1>1．什么是REST?</h1>
<div>REST 是由 Roy Fielding 在他的论文<a href="http://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm">《Architectural Styles and the Design of Network-based Software Architectures》</a>中提出的一个术语。</div>
<div>REST 是英文 Representational State Transfer 的缩写，有中文翻译为“具象状态传输”（参考：<a href="http://dev2dev.bea.com.cn/bbsdoc/20060529259.html">《SIP/IMS网络中的Representational State Transfer (REST)和数据分布》</a>）</div>
<div>可以将REST归纳如下：</div>
<h2>1.1首先REST只是一种风格，不是一种标准</h2>
<blockquote>
<div>You will not see the W3C putting out a REST specification. You will not see IBM or Microsoft or Sun selling a REST developer's toolkit. Why? Because REST is just an architectural style. You can't bottle up that style. You can only understand it, and design your Web services in that style.</div>
<div><span style="font-size: x-small;"><em>-</em><strong><em> Roger L. Costello</em></strong><em> <a href="http://www.xfront.com/REST-Web-Services.html">《Building Web Services the REST Way》</a></em><em></em></span></div></blockquote>
<h2>1.2  REST是以资源为中心的</h2>
<div>在REST中，认为Web是由一系列的抽象资源（Abstract Resource）组成，这些抽象的资源具有不同的具体表现形式((Representational State)。譬如，定义一个资源为photo，含义是照片，它的表现形式可以是一个图片，也可以是一个.xml的文件，其中包含一些描述该照片的元素。或是一个html文件。并且这些具体的表现可以分布在不同的物理位置上。</div>
<h2>1.3  REST的目的是决定如何使一个定义良好的 Web程序向前推进</h2>
<div>一个程序可以通过选择一个Web页面上的超链(这些链接代表资源)，使得另一个Web页面 (Representational State of an abstract resource) 返回(transfer)到用户，使程序进一步运行。</div>
<h2>1.4  REST充分利用或者说极端依赖HTTP协议</h2>
<div><strong>它通过逻辑URI定位资源</strong></div>
<div>在 REST 的定义中，一个 Web 应用总是使用固定的 URI 向外部世界呈现（或者说暴露）一个资源。</div>
<div>这里必须注意</div>
<div>1.一个URI对应一个资源</div>
<div>2.这里的URI是一个逻辑URI</div>
<div>一个逻辑 URI： http://www.example.com/photo/logo</div>
<div>一个物理 URI： http://www.example.com/photo/logo.html</div>
<div></div>
<div><strong>通过分辨 HTTP Request Header 信息来分辨客户端是想要取得资源的哪一种表现形式的数据</strong></div>
<div>我们来看一个实际例子：</div>
<div>http://www.example.com/photo/logo 指向 example.com 网站（可以视为一个 Web 应用）中类型为photo，名字为 logo 的资源。我们用浏览器访问这个 URI，看到的将可能是一个 html 文档，其中用 &lt;img src=”……” /&gt; 来显示实际的照片。</div>
<div>http://www.example.com/photo/logo 这个地址很可能会在服务器内部处理为 http://www.example.com/photo.jsp?name=logo 这样的地址。photo.jsp 是服务器端的一个动态脚本文件，根据 name 参数生成 html 文档返回给浏览器。</div>
<div>现在假设我们要获取这张照片的 XML 文档。XML 文档中包含照片的文件名、文件大小、拍摄日期等等信息。也就是说我们要获取“同一个资源的不同表现形式的数据”。对于这个要求，我们可以很容易的用另一个 URL地址达到：http://www.example.com/xml/logo。</div>
<div>但是，这就违背了“URI 唯一标识一个资源”的定义。如果我们要获取同一个资源的多种表现形式，那么就要使用更多的 URL，从而给一个资源指定了多个不同的 URI。</div>
<div>而在REST 中，不管是获取照片的 xhtml 文档还是 XML 文档，或者照片文件本身，都是用同一个 URI，就是http://www.example.com/photo/logo</div>
<div>那这是怎么办到的呢？是通过分辨 HTTP Request Header 信息来分辨客户端是想要取得资源的哪一种表现形式的数据。</div>
<div>当我们用浏览器访问一个网址时，浏览器会构造一个 HTTP 请求。这个请求有一个头信息，其中包括了本次请求接受何种类型的数据。通常浏览器发送的 HTTP 请求头中，Accept 的值都是 */*，也就说接受服务器返回的任何类型的数据。</div>
<div>只要我们指定一个特定的 Accept 参数，那么服务器就可以通过判断该参数来决定返回什么类型的数据。所以在一个采用 REST 架构的应用中，要获取同一个资源的不同表现形式的数据，只需要使用不同的 HTTP 请求头信息就行了。</div>
<div>如果考虑为 Web 应用增加 Web Services，这种技术的价值就体现出来了。比如写了一个程序，现在只需要构造一个包含 Accept: text/xml 的 HTTP 请求头，然后将请求发送到 http://www.example.com/photo/logo 就可以了。返回的结果就是一个 XML 文档，而不是别的资源形式的数据。</div>
<div></div>
<div><strong>在 REST 架构中，用不同的 HTTP 请求方法来处理对资源的 CRUD（创建、读取、更新和删除）操作</strong></div>
<div>我们在 Web 应用中处理来自客户端的请求时，通常只考虑 GET 和 POST 这两种 HTTP 请求方法。实际上，HTTP 还有HEAD、PUT、DELETE 等请求方法。而在 REST 架构中，用不同的 HTTP 请求方法来处理对资源的 CRUD（创建、读取、更新和删除）操作：</div>
<div>Ø POST: 创建</div>
<div>Ø GET: 读取</div>
<div>Ø PUT: 更新</div>
<div>Ø DELETE: 删除</div>
<div>经过这样的一番扩展，我们对一个资源的 CRUD 操作就可以通过同一个 URI 完成了：</div>
<div>http://www.example.com/photo/logo（读取）
仍然保持为 [GET] http://www.example.com/photo/logo</div>
<div>http://www.example.com/photo/logo/create（创建）
改为 [POST] http://www.example.com/photo/logo</div>
<div>http://www.example.com/photo/logo/update（更新）
改为 [PUT] http://www.example.com/photo/logo</div>
<div>http://www.example.com/photo/logo/delete（删除）
改为 [DELETE] http://www.example.com/photo/logo</div>
<div>从而进一步规范了资源标识的使用。</div>
<div>通过 REST 架构，Web 应用程序可以用一致的接口（URI）暴露资源给外部世界，并对资源提供语义一致的操作服务。这对于以资源为中心的 Web 应用来说非常重要。</div>
<h1>2．RESTful Web Services 特点</h1>
<div>通过将HTTP Accept类型设置为text/xml，我们便可以将资源返回的“具象状态”表示为可跨平台识别的数据。这是应用于Web Services的基础。</div>
<div>RESTful Web Service具有以下特点：</div>
<h2>2.1无状态的</h2>
<div>每一个来自客户端的request必须包含所有必要的信息，即不能从服务器端获得任何保存的上下文信息。</div>
<div>REST 的 “客户机－无状态－服务器” 约束禁止在服务器上保存会话状态。符合这一约束进行设计</div>
<div>1.可以提高系统的可靠性和可伸缩性。它不需要昂贵的维护和支持费用，因为状态并不在服务器上维护</div>
<div>2.可以进行资源缓存。Web的高速缓存既可以驻留在客户主机中，也可以驻留在中间网络高速缓存服务器主机中</div>
<h2>2.2  HTTP头中可见的统一接口和资源地址</h2>
<div>通过对于HTTP Head 的解析，我们便可以了解到当前所请求的资源和请求的方式。相对于SOAP RPC风格来说，我们必须解析HTTP体。</div>
<div>这样做对于一些代理服务器的设置，将带来很高的处理效率。</div>
<div>REST 系统中所有的动作和要访问的资源都可以从H TTP和URI 中得到，这使得代理服务器、缓存服务器和网关很好地协调工作。而RPC模 型的SOAP 要访问的资源仅从 URI无法得知，要调用的方法也无法从 HTTP中得知，它们都隐藏在 SOAP 消息中。</div>
<div><span style="font-size: x-small;">同样的，在REST系统中的代理服务器还可以通过 HTTP 的动作 (GET 、 POST)来进行控制。</span></div>
<div><img class="aligncenter size-full wp-image-1116" title="200710231193107976394" src="http://www.im47.cn/wp-content/uploads/2012/03/200710231193107976394.jpg" alt="" /></div>
<h2>2.3  返回一般的XML格式内容</h2>
<div>一般情况下，一个RESTful Web Service将比一个SOAP RPC Web Service占用更少的传输带宽。</div>
<table border="1" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="top" width="568">
<div>POST/Order HTTP/1.1</div>
<div>Host:www.northwindtraders.com</div>
<div>Content-Type:text/xml</div>
<div>Content-Length:nnnn</div>
<div></div>
<div>&lt;UpdatePO&gt;</div>
<div>      &lt;orderID&gt;098&lt;/orderID&gt;</div>
<div>      &lt;customerNumber&gt;999&lt;/customerNumber&gt;</div>
<div>      &lt;item&gt;89&lt;/item&gt;</div>
<div>      &lt;quantity&gt;3000&lt;/quantity&gt;</div>
<div>&lt;/UpdatePO&gt;</div></td>
</tr>
</tbody>
</table>
<table border="1" cellspacing="0" cellpadding="0">
<tbody>
<tr>
<td valign="top" width="568">
<div>POST/Order HTTP/1.1</div>
<div>Host:www.northwindtraders.com</div>
<div>Content-Type:text/xml</div>
<div>Content-Length:nnnn</div>
<div>SOAPAction:”urn:northwindtraders.com:PO#UpdatePO”</div>
<div></div>
<div>&lt;SOAP-ENV:Envelope</div>
<div>xmlns:xsi=”http://www.3w.org/1999/XMLSchema/instance”</div>
<div>xmlns:SOAP-ENV=”http://schemas.xmlsoap.org/soap/envelope”</div>
<div>xsi:schemaLocation=”http://www.northwindtraders.com/schema/NPOSchema.xsd”&gt;</div>
<div>&lt;SOAP-ENV:Body xsi:type=”NorthwindBody”&gt;</div>
<div>    &lt;UpdatePO&gt;</div>
<div>      &lt;orderID&gt;098&lt;/orderID&gt;</div>
<div>      &lt;customerNumber&gt;999&lt;/customerNumber&gt;</div>
<div>      &lt;item&gt;89&lt;/item&gt;</div>
<div>      &lt;quantity&gt;3000&lt;/quantity&gt;</div>
<div>    &lt;/UpdatePO&gt;</div>
<div>&lt;/SOAP-ENV:Body&gt;</div>
<div>&lt;/SOAP-ENV:Envelope&gt;</div></td>
</tr>
</tbody>
</table>
<h2>2.4  安全机制</h2>
<div>REST使用了简单有效的安全模型。REST中很容易隐藏某个资源，只需不发布它的URI；而在资源上也很容易使用一些安全策略，比如可以在每个 URI 针对 4个通用接口设置权限；再者，以资源为中心的 Web服务是防火墙友好的，因为 GET的意思就是GET， PUT 的意思就是PUT，管理员可以通过堵塞非GET请求把资源设置为只读的，而现在的基于RPC 模型的 SOAP一律工作在 HTTP 的 POST上。</div>
<div>使用 SOAP RPC模型，要访问的对象名称藏在方法的参数中，因此需要创建新的安全模型。</div>
<h2>2.5  无法用于事务型的服务</h2>
<div>对于事务型的服务，一个简单的例子就是银行事务，在那里用户可以把钱从一个账户转移到另一个账户上。用户不想直接操作资源（钱、银行账户等等），他们只想告诉银行他们想要达到的目的，并且让银行根据他们的利益对资源进行处理。</div>
<div>所以从这一条，我们应该明白，选择基于REST或SOAP RPC风格的Web 服务，我们应该首先考虑这个服务是针对资源的还是针对活动的。</div>
<div>- James Snell，面向资源与面向活动的 Web 服务，<a href="http://www.ibm.com/developerworks/webservices/library/ws-restvsoap/?S_TACT=105AGX52&amp;S_CMP=cn-a-ws" target="_blank">英文原文</a>。</div>
<h1>3．JAX-WS</h1>
<div>JAX-WS是Java Architecture for XML Web Services的缩写,简单说就是一种用Java和XML开发Web Services应用程序的框架, 目前版本是2.0, 它是JAX-RPC 1.1的后续版本。</div>
<div><a href="https://jax-ws.dev.java.net/">JAX-WS2.0</a>用JAXB2来处理Java Object与XML之间的映射。</div>
<div>JAX-WS通过javax.xml.ws.Provider接口来构建REST风格的终端。</div>
<h1>4．WADL</h1>
<div><a href="http://research.sun.com/spotlight/2006/2006-04-24-TR153.html">Web Application Description Language (WADL)</a>是由SUN公司提出的，旨在提供一种Web 应用的描述语言。WADL主要描述一个Web 应用的</div>
<div>
<ul>
	<li>资源列表-站点所有的资源</li>
</ul>
</div>
<div>
<ul>
	<li>资源之间的关系-说明资源之间的链接关系</li>
</ul>
</div>
<div>
<ul>
	<li>所有应用于每个资源的特定方法-应用于每个资源的HTTP方法，指定的输入和输出以及它们支持的数据格式</li>
</ul>
</div>
<div>
<ul>
	<li>资源表现的形式-支持的MIME类型和所用到的XML schemas</li>
</ul>
<div><span style="line-height: 19px;"><img class="size-full wp-image-1115 aligncenter" title="200710231193108006978" src="http://www.im47.cn/wp-content/uploads/2012/03/200710231193108006978.jpg" alt="" width="575" height="389" /></span></div>
</div>
<div align="left">下面是一个WADL示例，它描述了Yahoo新闻搜索的应用。</div>
<div align="left">
<ul>
	<li>Lines 2 开始一个应用描述，定义所使用的namespaces。</li>
</ul>
</div>
<div align="left">
<ul>
	<li>Lines 3–6 定义XML 语法，这个示例中是两个W3C XML Schema。</li>
</ul>
</div>
<div align="left">
<ul>
	<li>Lines 7–11描述了Yahoo新闻搜索Web 资源以及它所支持的HTTP方法。</li>
</ul>
</div>
<div align="left">
<ul>
	<li>Lines 12–26 描述了‘search’ GET方法。</li>
</ul>
</div>
<div align="left">
<ul>
	<li>Lines 13–21 描述输入。</li>
</ul>
</div>
<div align="left">
<ul>
	<li>Lines 22–25 描述所有可能的输出。</li>
</ul>
</div>
<h1>5．关于WADL的项目介绍</h1>
<div><a href="https://glassfish.dev.java.net/" target="_parent"> </a><a href="https://glassfish.dev.java.net/" target="_parent">GlassFish</a> » <a href="https://wadl.dev.java.net/" target="_parent">Web Application Description Language</a></div>
<div><strong>wadl2java</strong> - A tool that generates client side stubs from WADL files. May be used from the command line or as an Apache Ant plug-in, see the <a href="https://wadl.dev.java.net/wadl2java.html" target="_parent">wadl2java documentation</a> for full details.</div>
<div><strong>wadl2java_yahoo</strong> - A sample project that uses the wadl2java tool to create stubs for the <a href="http://developer.yahoo.com/search/news/V1/newsSearch.html" target="_parent">Yahoo News Search Service</a>. Includes a simple main method that uses the generated stubs to query for the latest Java news.</div>
<h1>6．建议阅读的资料</h1>
<div>[1] Roy Thomas Fielding，<a href="http://www.ics.uci.edu/~fielding/pubs/dissertation/top.htm">Architectural Styles and the Design of Network-based Software Architectures</a></div>
<div>[2] <a href="http://rest.blueoxen.net/cgi-bin/wiki.pl">RESTwiki</a></div>
<div>[3] <a href="http://c2.com/cgi/fullSearch">Rest Architectural Style</a></div>
<div>[4] <a href="http://groups.yahoo.com/group/rest-discuss/">REST mailing list</a></div>
<div>[5] <a href="http://www.xfront.com/REST-Web-Services.html">Building Web Services the REST Way</a></div>
<div>[6] <a href="http://www-128.ibm.com/developerworks/cn/webservices/ws-restvsoap/">面向资源与面向活动的 Web 服务</a></div>
<div>[7] <a href="http://www.prescod.net/rest/security.html">Some thoughts about SOAP versus REST on Security</a></div>
<div>[8] WADL语言作者<a href="http://weblogs.java.net/blog/mhadley/">Marc Hadley's Blog</a></div>
<div>[9] <a href="http://java.sun.com/webservices/technologies/index.jsp">Java Web Service Technologies @Sun</a></div>
<div></div>
<div><a href="http://zhangjunhd.blog.51cto.com/113473/47283">来源</a></div>
