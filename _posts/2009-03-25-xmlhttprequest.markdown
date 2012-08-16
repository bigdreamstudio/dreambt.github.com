---
layout: post
category : Web Design
tags : [XMLHttpRequest, beginner, javascript, AJAX, tutorial]
title: XMLHttpRequest对象
wordpress_id: 144
wordpress_url: http://www.im47cn.nt102.idcwind.net/wordpress/?p=144
date: 2009-03-25 20:46:19.000000000 +08:00
---
XMLHttpRequest对象是当今所有AJAX和Web 2.0应用程序的技术基础。尽管软件经销商和开源社团现在都在提供各种AJAX框架以进一步简化XMLHttpRequest对象的使用；但是，我们仍然很有必要理解这个对象的详细工作机制。
<div><strong>一、 引言</strong></div>
<div></div>
<div>异步JavaScript与XML(AJAX)是一个专用术语，用于实现在客户端脚本与服务器之间的数据交互过程。这一技术的优点在于，它向开发者提供了一种从Web服务器检索数据而不必把用户当前正在观察的页面回馈给服务器。与现代浏览器的通过存取浏览器DOM结构的编程代码(JavaScript)动态地改变被显示内容的支持相配合，AJAX让开发者在浏览器端更新被显示的HTML内容而不必刷新页面。换句话说，AJAX可以使基于浏览器的应用程序更具交互性而且更类似传统型桌面应用程序。</div>
<div></div>
<div>Google的Gmail和Outlook Express就是两个使用AJAX技术的我们所熟悉的例子。而且，AJAX可以用于任何客户端脚本语言中，这包括JavaScript，Jscript和VBScript。</div>
<div></div>
<div>AJAX利用一个构建到所有现代浏览器内部的对象-XMLHttpRequest-来实现发送和接收HTTP请求与响应信息。一个经由XMLHttpRequest对象发送的HTTP请求并不要求页面中拥有或回寄一个＜form＞元素。AJAX中的"A"代表了"异步"，这意味着XMLHttpRequest对象的send()方法可以立即返回，从而让Web页面上的其它HTML/JavaScript继续其浏览器端处理而由服务器处理HTTP请求并发送响应。尽管缺省情况下请求是异步进行的，但是，你可以选择发送同步请求，这将会暂停其它Web页面的处理，直到该页面接收到服务器的响应为止。</div>
<div></div>
<div>微软在其Internet Explorer(IE) 5中作为一个ActiveX对象形式引入了XMLHttpRequest对象。其他的认识到这一对象重要性的浏览器制造商也都纷纷在他们的浏览器内实现了XMLHttpRequest对象，但是作为一个本地JavaScript对象而不是作为一个ActiveX对象实现。而如今，在认识到实现这一类型的价值及安全性特征之后，微软已经在其IE 7中把XMLHttpRequest实现为一个窗口对象属性。幸运的是，尽管其实现(因而也影响到调用方式)细节不同，但是，所有的浏览器实现都具有类似的功能，并且实质上是相同方法。目前，W3C组织正在努力进行XMLHttpRequest对象的标准化，并且已经发行了有关该W3C规范的一个草案。</div>
<div></div>
<div>本文将对XMLHttpRequest对象API进行详细讨论，并将解释其所有的属性和方法。</div>
<div></div>
<div><strong>二、 XMLHttpRequest对象的属性和事件</strong></div>
<div></div>
<div>XMLHttpRequest对象暴露各种属性、方法和事件以便于脚本处理和控制HTTP请求与响应。下面，我们将对此展开详细的讨论。</div>
<div>readyState属性</div>
<div></div>
<div>当XMLHttpRequest对象把一个HTTP请求发送到服务器时将经历若干种状态：一直等待直到请求被处理；然后，它才接收一个响应。这样以来，脚本才正确响应各种状态-XMLHttpRequest对象暴露一个描述对象的当前状态的readyState属性，如表格1所示。</div>
<div></div>
<div>表格1.XMLHttpRequest对象的ReadyState属性值列表。</div>
<div></div>
<div>ReadyState取值 描述</div>
<div>0  描述一种"未初始化"状态；此时，已经创建一个XMLHttpRequest对象，但是还没有初始化。</div>
<div>1  描述一种"发送"状态；此时，代码已经调用了XMLHttpRequest open()方法并且XMLHttpRequest已经准备好把一个请求发送到服务器。</div>
<div>2  描述一种"发送"状态；此时，已经通过send()方法把一个请求发送到服务器端，但是还没有收到一个响应。</div>
<div>3  描述一种"正在接收"状态；此时，已经接收到HTTP响应头部信息，但是消息体部分还没有完全接收结束。</div>
<div>4  描述一种"已加载"状态；此时，响应已经被完全接收。</div>
<div></div>
<div><strong>onreadystatechange事件</strong></div>
<div></div>
<div>无论readyState值何时发生改变，XMLHttpRequest对象都会激发一个readystatechange事件。其中，onreadystatechange属性接收一个EventListener值-向该方法指示无论readyState值何时发生改变，该对象都将激活。</div>
<div></div>
<div><strong>responseText属性</strong></div>
<div></div>
<div>这个responseText属性包含客户端接收到的HTTP响应的文本内容。当readyState值为0、1或2时，responseText包含一个空字符串。当readyState值为3(正在接收)时，响应中包含客户端还未完成的响应信息。当readyState为4(已加载)时，该responseText包含完整的响应信息。</div>
<div></div>
<div><strong>responseXML属性</strong></div>
<div></div>
<div>此responseXML属性用于当接收到完整的HTTP响应时(readyState为4)描述XML响应；此时，Content-Type头部指定MIME(媒体)类型为text/xml，application/xml或以+xml结尾。如果Content-Type头部并不包含这些媒体类型之一，那么responseXML的值为null。无论何时，只要readyState值不为4，那么该responseXML的值也为null。</div>
<div></div>
<div>其实，这个responseXML属性值是一个文档接口类型的对象，用来描述被分析的文档。如果文档不能被分析(例如，如果文档不是良构的或不支持文档相应的字符编码)，那么responseXML的值将为null。</div>
<div></div>
<div><strong>status属性</strong></div>
<div></div>
<div>这个status属性描述了HTTP状态代码，而且其类型为short。而且，仅当readyState值为3(正在接收中)或4(已加载)时，这个status属性才可用。当readyState的值小于3时试图存取status的值将引发一个异常。</div>
<div></div>
<div><strong>statusText属性</strong></div>
<div></div>
<div>这个statusText属性描述了HTTP状态代码文本；并且仅当readyState值为3或4才可用。当readyState为其它值时试图存取statusText属性将引发一个异常。</div>
<div></div>
<div><strong>三、 XMLHttpRequest对象的方法</strong></div>
<div></div>
<div>XMLHttpRequest对象提供了各种方法用于初始化和处理HTTP请求，下列将逐个展开详细讨论。</div>
<div></div>
<div><strong>abort()方法</strong></div>
<div></div>
<div>你可以使用这个abort()方法来暂停与一个XMLHttpRequest对象相联系的HTTP请求，从而把该对象复位到未初始化状态。</div>
<div></div>
<div><strong>open()方法</strong></div>
<div></div>
<div>你需要调用open(DOMString method，DOMString uri，boolean async，DOMString username，DOMString password)方法初始化一个XMLHttpRequest对象。其中，method参数是必须提供的-用于指定你想用来发送请求的HTTP方法(GET，POST，PUT，Delete或HEAD)。为了把数据发送到服务器，应该使用POST方法；为了从服务器端检索数据，应该使用GET方法。另外，uri参数用于指定XMLHttpRequest对象把请求发送到的服务器相应的URI。借助于window.document.baseURI属性，该uri被解析为一个绝对的URI-换句话说，你可以使用相对的URI-它将使用与浏览器解析相对的URI一样的方式被解析。async参数指定是否请求是异步的-缺省值为true。为了发送一个同步请求，需要把这个参数设置为false。对于要求认证的服务器，你可以提供可选的用户名和口令参数。在调用open()方法后，XMLHttpRequest对象把它的readyState属性设置为1(打开)并且把responseText、responseXML、status和statusText属性复位到它们的初始值。另外，它还复位请求头部。注意，如果你调用open()方法并且此时readyState为4，则XMLHttpRequest对象将复位这些值。</div>
<div></div>
<div><strong>send()方法</strong></div>
<div></div>
<div>在通过调用open()方法准备好一个请求之后，你需要把该请求发送到服务器。仅当readyState值为1时，你才可以调用send()方法；否则的话，XMLHttpRequest对象将引发一个异常。该请求被使用提供给open()方法的参数发送到服务器。当async参数为true时，send()方法立即返回，从而允许其它客户端脚本处理继续。在调用send()方法后，XMLHttpRequest对象把readyState的值设置为2(发送)。当服务器响应时，在接收消息体之前，如果存在任何消息体的话，XMLHttpRequest对象将把readyState设置为3(正在接收中)。当请求完成加载时，它把readyState设置为4(已加载)。对于一个HEAD类型的请求，它将在把readyState值设置为3后再立即把它设置为4。</div>
<div></div>
<div>send()方法使用一个可选的参数-该参数可以包含可变类型的数据。典型地，你使用它并通过POST方法把数据发送到服务器。另外，你可以显式地使用null参数调用send()方法，这与不用参数调用它一样。对于大多数其它的数据类型，在调用send()方法之前，应该使用setRequestHeader()方法(见后面的解释)先设置Content-Type头部。如果在send(data)方法中的data参数的类型为DOMString，那么，数据将被编码为UTF-8。如果数据是Document类型，那么将使用由data.xmlEncoding指定的编码串行化该数据。</div>
<div></div>
<div><strong>setRequestHeader()方法</strong></div>
<div></div>
<div>该setRequestHeader(DOMString header，DOMString value)方法用来设置请求的头部信息。当readyState值为1时，你可以在调用open()方法后调用这个方法；否则，你将得到一个异常。</div>
<div></div>
<div><strong>getResponseHeader()方法</strong></div>
<div></div>
<div>getResponseHeader(DOMString header，value)方法用于检索响应的头部值。仅当readyState值是3或4(换句话说，在响应头部可用以后)时，才可以调用这个方法；否则，该方法返回一个空字符串。</div>
<div></div>
<div><strong>getAllResponseHeaders()方法</strong></div>
<div></div>
<div>该getAllResponseHeaders()方法以一个字符串形式返回所有的响应头部（每一个头部占单独的一行）。如果readyState的值不是3或4，则该方法返回null。</div>
<div></div>
<div><strong>四、 发送请求</strong></div>
<div></div>
<div>在AJAX中，许多使用XMLHttpRequest的请求都是从一个HTML事件（例如一个调用JavaScript函数的按钮点击(onclick)或一个按键(onkeypress)）中被初始化的。AJAX支持包括表单校验在内的各种应用程序。有时，在填充表单的其它内容之前要求校验一个唯一的表单域。例如要求使用一个唯一的UserID来注册表单。如果不是使用AJAX技术来校验这个UserID域，那么整个表单都必须被填充和提交。如果该UserID不是有效的，这个表单必须被重新提交。例如，一个相应于一个要求必须在服务器端进行校验的Catalog ID的表单域可能按下列形式指定：</div>
<div></div>
<div>＜form name="validationForm" action="validateForm" method="post"＞</div>
<div>＜table＞</div>
<div>＜tr＞＜td＞Catalog Id:＜/td＞</div>
<div>＜td＞</div>
<div>＜input type="text" size="20" id="catalogId" name="catalogId" autocomplete="off" onkeyup="sendRequest()"＞</div>
<div>＜/td＞</div>
<div>＜td＞＜div id="validationMessage"＞＜/div＞＜/td＞</div>
<div>＜/tr＞</div>
<div>＜/table＞＜/form＞</div>
<div></div>
<div>前面的HTML使用validationMessage div来显示相应于这个输入域Catalog Id的一个校验消息。onkeyup事件调用一个JavaScript sendRequest()函数。这个sendRequest()函数创建一个XMLHttpRequest对象。创建一个XMLHttpRequest对象的过程因浏览器实现的不同而有所区别。如果浏览器支持XMLHttpRequest对象作为一个窗口属性(所有普通的浏览器都是这样的，除了IE 5和IE 6之外)，那么，代码可以调用XMLHttpRequest的构造器。如果浏览器把XMLHttpRequest对象实现为一个ActiveXObject对象(就象在IE 5和IE 6中一样)，那么，代码可以使用ActiveXObject的构造器。下面的函数将调用一个init()函数，它负责检查并决定要使用的适当的创建方法-在创建和返回对象之前。</div>
<div>＜script type="text/javascript"＞</div>
<div>function sendRequest(){</div>
<div>var xmlHttpReq=init();</div>
<div>function init(){</div>
<div>if (window.XMLHttpRequest) {</div>
<div>return new XMLHttpRequest();</div>
<div>}</div>
<div>else if (window.ActiveXObject) {</div>
<div>return new ActiveXObject("Microsoft.XMLHTTP");</div>
<div>}</div>
<div>}</div>
<div>＜/script＞</div>
<div></div>
<div>接下来，你需要使用Open()方法初始化XMLHttpRequest对象-指定HTTP方法和要使用的服务器URL。</div>
<div>var catalogId=encodeURIComponent(document.getElementById("catalogId").value);</div>
<div>xmlHttpReq.open("GET"， "validateForm?catalogId=" + catalogId， true);</div>
<div></div>
<div>默认情况下，使用XMLHttpRequest发送的HTTP请求是异步进行的，但是你可以显式地把async参数设置为true，如上面所展示的。</div>
<div>在这种情况下，对URL validateForm的调用将激活服务器端的一个servlet，但是你应该能够注意到服务器端技术不是根本性的；实际上，该URL可能是一个ASP，ASP.NET或PHP页面或一个Web服务-这无关紧要，只要该页面能够返回一个响应-指示CatalogID值是否是有效的-即可。因为你在作一个异步调用，所以你需要注册一个XMLHttpRequest对象将调用的回调事件处理器-当它的readyState值改变时调用。记住，readyState值的改变将会激发一个readystatechange事件。你可以使用onreadystatechange属性来注册该回调事件处理器。</div>
<div>xmlHttpReq.onreadystatechange=processRequest;</div>
<div></div>
<div>然后，我们需要使用send()方法发送该请求。因为这个请求使用的是HTTP GET方法，所以，你可以在不指定参数或使用null参数的情况下调用send()方法。</div>
<div>xmlHttpReq.send(null);</div>
<div></div>
<div><strong>五、 处理请求</strong></div>
<div></div>
<div>在这个示例中，因为HTTP方法是GET，所以在服务器端的接收servlet将调用一个doGet()方法，该方法将检索在URL中指定的catalogId参数值，并且从一个数据库中检查它的有效性。</div>
<div></div>
<div>本文示例中的这个servlet需要构造一个发送到客户端的响应；而且，这个示例返回的是XML类型，因此，它把响应的HTTP内容类型设置为text/xml并且把Cache-Control头部设置为no-cache。设置Cache-Control头部可以阻止浏览器简单地从缓存中重载页面。</div>
<div>public void doGet(HttpServletRequest request，</div>
<div>HttpServletResponse response)</div>
<div>throws ServletException， IOException {</div>
<div>...</div>
<div>...</div>
<div>response.setContentType("text/xml");</div>
<div>response.setHeader("Cache-Control"， "no-cache");</div>
<div>}</div>
<div></div>
<div>来自于服务器端的响应是一个XML DOM对象，此对象将创建一个XML字符串-其中包含要在客户端进行处理的指令。另外，该XML字符串必须有一个根元素。</div>
<div>out.println("＜catalogId＞valid＜/catalogId＞");</div>
<div></div>
<div>【注意】XMLHttpRequest对象的设计目的是为了处理由普通文本或XML组成的响应；但是，一个响应也可能是另外一种类型，如果用户代理(UA)支持这种内容类型的话。</div>
<div></div>
<div>当请求状态改变时，XMLHttpRequest对象调用使用onreadystatechange注册的事件处理器。因此，在处理该响应之前，你的事件处理器应该首先检查readyState的值和HTTP状态。当请求完成加载（readyState值为4）并且响应已经完成（HTTP状态为"OK"）时，你就可以调用一个JavaScript函数来处理该响应内容。下列脚本负责在响应完成时检查相应的值并调用一个processResponse()方法。</div>
<div>function processRequest(){</div>
<div>if(xmlHttpReq.readyState==4){</div>
<div>if(xmlHttpReq.status==200){</div>
<div>processResponse();</div>
<div>}</div>
<div>}</div>
<div>}</div>
<div></div>
<div>该processResponse()方法使用XMLHttpRequest对象的responseXML和responseText属性来检索HTTP响应。如上面所解释的，仅当在响应的媒体类型是text/xml，application/xml或以+xml结尾时，这个responseXML才可用。这个responseText属性将以普通文本形式返回响应。对于一个XML响应，你将按如下方式检索内容：</div>
<div>var msg=xmlHttpReq.responseXML;</div>
<div></div>
<div>借助于存储在msg变量中的XML，你可以使用DOM方法getElementsByTagName()来检索该元素的值：</div>
<div>var catalogId=msg.getElementsByTagName("catalogId")[0].firstChild.nodeValue;</div>
<div></div>
<div>最后，通过更新Web页面的validationMessage div中的HTML内容并借助于innerHTML属性，你可以测试该元素值以创建一个要显示的消息：</div>
<div>if(catalogId=="valid"){</div>
<div>var validationMessage = document.getElementById("validationMessage");</div>
<div>validationMessage.innerHTML = "Catalog Id is Valid";</div>
<div>}</div>
<div>else</div>
<div>{</div>
<div>var validationMessage = document.getElementById("validationMessage");</div>
<div>validationMessage.innerHTML = "Catalog Id is not Valid";</div>
<div>}</div>
<div></div>
<div><strong>六、 小结</strong></div>
<div></div>
<div>上面就是XMLHttpRequest对象使用的所有细节实现。通过不必把Web页面寄送到服务器而实现数据传送，XMLHttpRequest对象为客户端与服务器之间提供了一种动态的交互能力。你可以使用JavaScript启动一个请求并处理相应的返回值，然后使用浏览器的DOM方法更新页面中的数据。</div>
