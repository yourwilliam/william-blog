---
layout: post
title: WebService的深度研究————Rest和Soap架构
description: "WebService的深度研究————Rest和Soap架构"
modified: 2015-01-08
category: articles
tags: [webservice,rest,soap]
comments: true
share: true
---

####SOAP和REST

典型的基于 SOAP 的 Web 服务以操作为中心，每个操作接受 XML 文档作为输入，提供 XML 文档作为输出。在本质上讲，它们是 RPC 风格的。而在遵循 REST 原则的 ROA 应用中，服务是以资源为中心的，对每个资源的操作都是标准化的 HTTP 方法。


####SOAP 

所有的SOAP消息发送都使用HTTP POST方法，并且所有SOAP消息的URI都是一样的。

SOAP（Simple Object Access Protocol ）简单对象访问协议是在分散或分布式的环境中交换信息的简单的协议，是一个基于XML的协议，它包括四个部分：SOAP封装(envelop)，封装定义了一个描述消息中的内容是什么，是谁发送的，谁应当接受并处理它以及如何处理它们的框架；SOAP编码规则（encoding rules），用于表示应用程序需要使用的数据类型的实例; SOAP RPC表示(RPC representation)，表示远程过程调用和应答的协定;SOAP绑定（binding），使用底层协议交换信息。

SOAP的两个主要设计目标是简单性和可扩展性。

#####SOAP=RPC+HTTP+XML
SOAP简单的理解，就是这样的一个开放协议SOAP=RPC+HTTP+XML：采用HTTP作为底层通讯协议；RPC作为一致性的调用途径，XML作为数据传送的格式，允许服务提供者和服务客户经过防火墙在INTERNET进行通讯交互。
RPC的描叙可能不大准确，因为SOAP一开始构思就是要实现平台与环境的无关性和独立性，每一个通过网络的远程调用都可以通过SOAP封装起来，包括DCE（Distributed Computing Environment ）　RPC CALLS，COM/DCOM CALLS, CORBA CALLS, JAVA CALLS，etc。


####Rest 

REST（Representational State Transfer）客户端应用程序随着每个资源表现状态的不同而发生状态转移。

可以使用遵循Rest设计原则的ROA(Resource-Oriented Architecture，面向资源的体系架构)进行设计。ROA是一种把实际问题转化为REST式的web服务的方法，它使得URI、HTTP、XML具有跟其他web应用一样的方式。

基本ROA步骤：
* 分析应用需求中的数据集。
* 映射数据集到 ROA 中的资源。
* 对于每一资源，命名它的 URI。
* 为每一资源设计其 Representations。
* 用 hypermedia links 表述资源间的联系。

REST可以使用HTTP PUT、GET、DELETE等

#####四大基本设计原则

######显式地使用 HTTP 方法
基于 REST 的 Web 服务的主要特征之一是以遵循 RFC 2616 定义的协议的方式显式使用 HTTP 方法。

REST 要求开发人员显式地使用 HTTP 方法，并且使用方式与协议定义一致。 这个基本 REST 设计原则建立了创建、读取、更新和删除（create, read, update, and delete，CRUD）操作与 HTTP 方法之间的一对一映射。

在实现上来可以使用get请求来实现add一条数据，或者使用post请求来查询，但这并不是优雅的REST设计。

######无状态
REST Web 服务需要发送完整、独立的请求；也就是说，发送的请求包括所有需要满足的数据，以便中间服务器中的组件能够进行转发、路由和负载平衡，而不需要在请求之间在本地保存任何状态。

完整、独立的请求不要求服务器在处理请求时检索任何类型的应用程序上下文或状态。 REST Web 服务应用程序（或客户端）在 HTTP Header 和请求正文中包括服务器端组件生成响应所需要的所有参数、上下文和数据。 这种意义上的无状态可以改进 Web 服务性能，并简化服务器端组件的设计和实现，因为服务器上没有状态，从而消除了与外部应用程序同步会话数据的需要。


######公开目录结构式的 URI
REST Web 服务 URI 的直观性应该达到很容易猜测的程度。 将 URI 看作是自身配备文档说明的接口，开发人员只需很少（如果有的话）的解释或参考资料即可了解它指向什么，并获得相关的资源。 为此，URI 的结构应该简单、可预测且易于理解。

在考虑基于 REST 的 Web 服务的 URI 结构时，需要指出的一些附加指导原则包括：
* 隐藏服务器端脚本技术文件扩展名（.jsp、.php、.asp）——如果有的话，以便您能够移植到其他脚本技术而不用更改 URI。
* 将所有内容保持小写。
* 将空格替换为连字符或下划线（其中一种或另一种）。
* 尽可能多地避免查询字符串。
* 如果请求 URI 用于部分路径，与使用 404 Not Found 代码不同，应该始终提供缺省页面或资源作为响应。
* URI 还应该是静态的，以便在资源发生更改或服务的实现发生更改时，链接保持不变。 这可以实现书签功能。 URI 中编码的资源之间的关系与在存储资源的位置表示资源关系的方式无关也是非常重要的。

######传输 XML、JavaScript Object Notation (JSON)，或同时传输这两者

基于 REST 的 Web 服务设计中的最后一组约束与应用程序和服务在请求/响应有效负载或 HTTP 正文中交换的数据的格式有关。 这是真正值得将一切保持简单、可读和连接在一起的方面。

为了赋予客户端请求最适合它们的特定内容类型的能力，您的服务的构造应该利用内置的 HTTP Accept Header，其中该 Header 的值为 MIME 类型。
基于 REST 的服务使用的常见 MIME 类型：
JSON	application/json
XML	    application/xml
XHTML	application/xhtml+xml



####REST和SOAP的比较

#####接口抽象
* RESTful Web服务使用标准的HTTP方法(GET/PUT/POST/DELETE) 来抽象所有 Web 系统的服务能力。
* SOAP应用通过定义自己个性化的接口方法来抽象WEB服务。

标准化的RESTful Web服务带来了本身的一些HTTP的优势:

######无状态性(stateless)
HTTP 协议从本质上说是一种无状态的协议，客户端发出的 HTTP 请求之间可以相互隔离，不存在相互的状态依赖。基于 HTTP 的 ROA，以非常自然的方式来实现无状态服务请求处理逻辑。对于分布式的应用而言，任意给定的两个服务请求 Request 1 与 Request 2, 由于它们之间并没有相互之间的状态依赖，就不需要对它们进行相互协作处理，其结果是：Request 1 与 Request 2 可以在任何的服务器上执行，这样的应用很容易在服务器端支持负载平衡 (load-balance)。

######安全操作与幂指相等特性（Safety /Idempotence）
HTTP 的 GET、HEAD 请求本质上应该是安全的调用，即：GET、HEAD 调用不会有任何的副作用，不会造成服务器端状态的改变。对于服务器来说，客户端对某一 URI 做 n 次的 GET、HAED 调用，其状态与没有做调用是一样的，不会发生任何的改变。
HTTP 的 PUT、DELTE 调用，具有幂指相等特性 , 即：客户端对某一 URI 做 n 次的 PUT、DELTE 调用，其效果与做一次的调用是一样的。HTTP 的 GET、HEAD 方法也具有幂指相等特性。
HTTP 这些标准方法在原则上保证你的分布式系统具有这些特性，以帮助构建更加健壮的分布式系统。

#####安全控制
客户端发出的请求通过代理服务器(Proxy Server)，代理服务器指定安全策略，对请求进行分发。由于REST类型可以直接看到URI，所以通过URI就可以判定相应的权限是否通过。
SOAP经过代理服务器时，无法通过URI直接看到相应的信息，必须要对SOAP进行解析才可以，所以需要第三方Proxy Server必须知晓当前的SOAP语义，这是造成了和Proxy Server之间的紧耦合。

#####缓存
REST可以充分利用HTTP协议对缓存的支持能力。HTTP 协议带条件的 HTTP GET 请求 (Conditional GET) 被设计用来节省客户端与服务器之间网络传输带来的开销，这也给客户端实现 Cache 机制 ( 包括在客户端与服务器之间的任何代理 ) 提供了可能。HTTP 协议通过 HTTP HEADER 域：If-Modified-Since/Last- Modified，If-None-Match/ETag 实现带条件的 GET 请求。
SOAP应用很难发挥HTTP本身的缓存能力。

#####连接性
在一个纯的 SOAP 应用中，URI 本质上除了用来指示 SOAP 服务器外，本身没有任何意义。
REST 可以通过 URI 驱动 SOAP 方法调用。

