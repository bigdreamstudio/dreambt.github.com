---
layout: post
category : J2EE
tags : [Jersey, J2EE, REST, Web service]
title: Jersey基础知识分享
wordpress_id: 1118
wordpress_url: http://www.im47.cn/?p=1118
date: 2012-03-06 22:13:31.000000000 +08:00
---
<h1>RESTful Web 服务简介</h1>
REST 在 2000 年由 Roy Fielding 在博士论文中提出，他是 HTTP 规范 1.0 和 1.1 版的首席作者之一。
REST 中最重要的概念是资源（resources），使用全球 ID（通常使用 URI）标识。客户端应用程序使用 HTTP 方法（GET/ POST/ PUT/ DELETE）操作资源或资源集。RESTful Web 服务是使用 HTTP 和 REST 原理实现的 Web 服务。通常，RESTful Web 服务应该定义以下方面：
Web 服务的基/根 URI，比如 http://host/&lt;appcontext&gt;/resources。
支持 MIME 类型的响应数据，包括 JSON/XML/ATOM 等等。
服务支持的操作集合（例如 POST、GET、PUT 或 DELETE）。
<h2>Jersey的四种行为</h2>
对应我们日常说的CRUD.
方法/资源 资源集合; URI：http://host/api/resources 成员资源; URI：http://host/api/resources/123 对应的操作
GET          列出资源集合的所有成员。                           检索标识为 123 的资源的表示形式。                     R（读取）
PUT          使用一个集合更新（替换）另一个集合。        更新标记为 123 的数字资源。                              U（更新）
POST        在集合中创建数字资源，其 ID 是自动分配的  在下面创建一个子资源。                                       C（创建）
DELETE     删除整个资源集合。                                     删除标记为 123 的数字资源。                              D（删除）
<h2>相关的架包结构</h2>
· 核心服务器：jersey-core.jar，jersey-server.jar，jsr311-api.jar，asm.jar

· 核心客户端：（用于测试）jersey-client.jar
· JAXB 支持：（在高级样例中使用）jaxb-impl.jar，jaxb-api.jar，activation.jar，stax-api.jar，wstx-asl.jar
· JSON 支持：（在高级样例中使用）jersey-json.jar
· Spring支持：（在高级样例中使用）jersey-spirng.jar
<h2>元注解信息说明</h2>
<h3>生存周期说明</h3>
1. 默认不使用注解，表示生存周期等于request，请求过后自动销毁，默认是线程安全的。
2. application，使用@Singleton注解。生存周期等于整个应用程序的生存周期。
3. session,使用@PerSession注解。生存周期等于一个session请求，session销毁，该rest资源实例也同时销毁。
<h3>Bean注解说明</h3>
1.@Path，路径信息，表示映射出去的访问路径。
范例如下：@Path("/myResource")
2. @Produces，用于限制post和get方法返回的参数类型，支持json、string、xml、html
范例如下：@Produces({"application/xml", "application/json"})
3. @Consumes，用于限制输入的参数的类型，支持json、string、xml、html
范例如下：@Consumes("text/plain")
4. @QueryParam，@DefaultValue，通过request传入的参数，@DefaultValue表示默认参数。
范例如下：@DefaultValue("2") @QueryParam("step") int step,
5. @PathParam ，@ MatrixParam，@ HeaderParam，@ CookieParam和@@ QueryParam FormParam听从以相同的规则。 @ MatrixParam提取URL路径段的信息。 @ HeaderParam提取的HTTP头信息。 @ CookieParam提取信息的Cookie饼干宣布相关的HTTP标头。@ FormParam略有特殊，因为它提取请求表示，该类型匹配前面的@Consumes所声明的类型。
范例如下：
@POST
@Consumes("application/x-www-form-urlencoded")
public void post(@FormParam("name") String name) {
6.pojo层面等相关注解，@XmlRootElement,支持JPA注解。
7.Spring相关注解，比如@Autowired(required=true) 、@Qualifier("persionDao")、@Component
@Scope("request")
