<properties
   pageTitle="API 设计指南 |Microsoft Azure"
   description="指导如何创建良好时设计的 API。"
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="best-practice"
   ms.devlang="rest-api"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/13/2016"
   ms.author="masashin"/>

# <a name="api-design-guidance"></a>API 设计指南

[AZURE.INCLUDE [pnp-header](../includes/guidance-pnp-header-include.md)]

## <a name="overview"></a>概述

许多现代的基于 web 的解决方案利用承载的 web 服务器，以便为远程客户端应用程序提供功能的 web 服务。 Web 服务公开的操作构成 web API。 一个设计良好的 web API 应旨在支持︰

- **平台无关性**。 客户端应用程序应该能够利用而无需公开 API 的操作或数据如何实际实现的 web 服务提供的 API。 这就要求 API 遵守启用客户端应用程序和 web 服务以达成使用，与客户端应用程序和 web 服务之间交换的数据结构的数据格式的通用标准。

- **服务发展**。 Web 服务应该能够发展并添加 （或删除） 功能独立于客户端应用程序。 现有的客户端应用程序应该能够继续正常运行的未修改作为 web 服务更改所提供的功能。 所有的功能还应发现，以便客户端应用程序能够充分利用它。

本指南的目的是为了说明设计 web API 时应考虑的问题。

## <a name="introduction-to-representational-state-transfer-rest"></a>具象状态传输 (REST) 简介

在他 dissertation 2000，Roy Fielding 提议由 web 服务公开的操作的数据结构的一种体系结构方法其余部分。 其余部分是构建基于超媒体的分布式的系统的体系结构样式。 其余部分模型的主要优点是它建立在开放标准并不会绑定模型或于任何特定的实现访问该客户端应用程序的实现。 例如，无法通过使用 Microsoft ASP.NET Web API，实现 REST 的 web 服务和客户端应用程序可以使用的任何语言和工具集，可以生成的 HTTP 请求和分析 HTTP 响应开发。

> [AZURE.NOTE] 其余部分是真正独立于任何基础协议并不一定与 HTTP。 但是，最常见的基于 REST 的系统实现将 HTTP 用作发送和接收请求的应用程序协议。 本文档重点介绍映射到系统配合使用 HTTP 的其他原则。

其余部分模型使用导航方案来表示对象和服务通过网络 （也称为_资源_）。 通常实现其余的许多系统使用 HTTP 协议传输请求，从而访问这些资源。 在这些系统中，客户端应用程序提交请求的 URI 标识的资源，并指示要在那个资源上执行的操作的 HTTP 方法 （最常见的有，帖子，放置或删除） 的窗体中。  HTTP 请求的正文包含执行该操作所需的数据。 重要的一点是其余部分定义请求无状态模型。 HTTP 请求应独立，可能会出现任意顺序，因此试图保留请求之间的瞬间状态信息并不可行。  存储信息的唯一的地方处于资源本身，并且每个请求应该是原子操作。 实际上，其余模型实现请求位置转换到另一个明确定义的非瞬态状态资源的有限状态机。

> [AZURE.NOTE] 在其他模型中的各个请求的无状态特性，使系统通过遵循这些原则，以具有高度可伸缩性而构建。 没有必要保留采取一系列的请求以及处理这些请求的特定 web 服务器的客户端应用程序之间的任何关系。

实施有效的其余部分模型的另一个关键点是要了解到该模型提供了访问各种资源之间的关系。 这些资源通常被组织作为集合和关系。 例如，假设一个电子商务系统的快速分析显示有两个集合中的客户端应用程序是可能感︰ 订单和客户。 每个订单和客户应具有其自己身份标识的唯一键。 要访问的集合的订单的 URI 可能是_/orders_，那样简单，检索所有客户的 URI 可能同样是_/customers_。 颁发给_/orders_的 URI 的 HTTP GET 请求，则应返回表示集合编码作为 HTTP 响应中的所有订单的列表︰

```HTTP
GET http://adventure-works.com/orders HTTP/1.1
...
```

如下所示的响应将订单编码为 JSON 列表结构︰

```HTTP
HTTP/1.1 200 OK
...
Date: Fri, 22 Aug 2014 08:49:02 GMT
Content-Length: ...
[{"orderId":1,"orderValue":99.90,"productId":1,"quantity":1},{"orderId":2,"orderValue":10.00,"productId":4,"quantity":2},{"orderId":3,"orderValue":16.60,"productId":2,"quantity":4},{"orderId":4,"orderValue":25.90,"productId":3,"quantity":1},{"orderId":5,"orderValue":99.90,"productId":1,"quantity":1}]
```
若要提取单个订单要求指定的顺序从_订单_资源，如_/orders/2_的标识符︰

```HTTP
GET http://adventure-works.com/orders/2 HTTP/1.1
...
```

```HTTP
HTTP/1.1 200 OK
...
Date: Fri, 22 Aug 2014 08:49:02 GMT
Content-Length: ...
{"orderId":2,"orderValue":10.00,"productId":4,"quantity":2}
```

> [AZURE.NOTE] 为简单起见，这些示例作为 JSON 文本数据返回的响应中显示的信息。 但是，没有的理由为什么资源应包含任何其他类型的数据支持 HTTP，如二进制或加密信息;在 HTTP 响应的内容类型应指定的类型。 此外，其余模型能够以不同的格式，如 XML 或 JSON 返回相同的数据。 在这种情况下，web 服务应该能够与发出请求的客户端执行内容的协商。 该请求可包含_Accept_标头，指定希望接收客户端和 web 服务应尝试如果可能接受这种格式的首选的格式。

请注意，来自其余请求的响应可以使用标准的 HTTP 状态代码。 例如，返回有效的数据的请求应包括 HTTP 响应代码 200 （成功），而无法查找或删除指定的资源的请求将返回包含的 HTTP 状态代码 404 （未找到） 响应。

## <a name="design-and-structure-of-a-restful-web-api"></a>设计和结构的 rest 风格的 web API

设计一个成功的网站的 API 的要点是简洁性和一致性。 一个 Web API，表现出这两个因素使得容易生成客户端应用程序需要使用此 API。

Rest 风格的 web API 重点公开一组连接的资源，并提供了使应用程序能够操作这些资源，并很容易地在它们之间导航的核心操作。 由于这个原因，构成一个典型的 rest 风格 web API 的 Uri 应该是面向数据公开，并使用 HTTP 提供的功能对这些数据进行操作。 这种方法需要从通常采用设计一套类在面向对象的 API 中往往会不会更激励对象和类的行为时，不同的思维方式。 另外，rest 风格的 web API 应该是无状态的并且不依赖于按特定的顺序被调用的操作。 以下各节概述设计 rest 风格的 web API 时应考虑的点。

### <a name="organizing-the-web-api-around-resources"></a>组织资源围绕 web API

> [AZURE.TIP] 由其他 web 服务公开的 Uri 应该基于名词 （到 web API 提供访问数据） 并不 （应用程序可以做什么用的数据） 的谓词。

专注于 web API 公开的业务实体。 例如，在 web API 用于支持前面所述的电子商务系统中，主实体是客户和订单。 等下订单的操作过程可以通过提供 HTTP POST 操作接受订单信息并将其添加到列表中的客户的订单。 在内部，此 POST 操作可以执行任务，如检查库存水平，并向客户报帐。 HTTP 响应可以指示成功与否是否下订单。 此外请注意，资源无需基于单个物理数据项目。 例如，订单资源可能使用信息分布在几个表关系数据库中，但提供给客户端作为一个实体的多行从聚合内部实现。

> [AZURE.TIP] 避免设计其余部分界面的镜像或取决于它所公开的数据的内部结构。 其余部分是有关多在单独的表，在关系数据库上实现简单 CRUD （创建检索、 更新、 删除） 操作。 其余部分的目的是将业务实体和物理实现的这些实体，这些实体可以执行应用程序，但客户端不应公开的操作映射到这些物理的详细信息。

各业务实体很少存在于隔离 （尽管可能存在一些单一实例对象），但实际上往往会组合成集合在一起。 在其余条款，每个实体和每个集合都是资源。 在 rest 风格 web API，每个集合都有其自身 URI 中的 web 服务，并通过 URI 集合执行 HTTP GET 请求中检索该集合中的项的列表。 每个单项也有它自己的 URI 和应用程序可以提交另一个 HTTP GET 请求使用该 URI 来检索该项目的详细信息。 您应该以分层方式组织集合和项目 Uri。 在电子商务系统中，URI _/customers_表示客户的集合和_/customers/5_检索单个客户从该集合 ID 为 5 的详细信息。 这种方法有助于保持 web API 直观。

> [AZURE.TIP] 采用一致的命名约定中的 Uri。一般情况下，它有助于使用复数名词的 Uri 的引用集合。

您还需要考虑不同类型的资源与方式可能会使这些关联之间的关系。 例如，客户可能会将零个或多个订单。 自然的方式来表示这种关系是通过如_/customers/5/orders_ URI 查找客户 5 的所有订单。 此外可以考虑表示的关联从订单返回到特定客户通过如_/orders/99/customer_的 URI 查找该客户订单 99，但扩展此模型太远会变得难以实现。 更好的解决方案是提供的链接可导航关联的资源，如客户，查询订单时所返回的 HTTP 响应消息的正文中。 在使用 HATEOAS 启用相关资源导航的方法在本指南后面的部分中详细介绍了这种机制。

在更复杂的系统中可能有许多其他类型的实体，并很有诱惑力，提供使客户端应用程序通过多种级别的关系，例如， _/customers/1/orders/99/products_ ，以获得顾客 1 99 的顺序的产品列表导航的 Uri。 但是，这种复杂性可能很难维护，非弹性，如果将来更改资源之间的关系。 相反，您应尽量保持 Uri 相对简单。 请记住，一旦应用程序具有对资源的引用，它应该可以使用该引用来查找与该资源相关的项目。 上面的查询可以替换 URI _/customers/1/orders_查找客户 1，所有的订单和然后查询 URI _/orders/99/products_ （假设 99 下订单的客户 1） 以此顺序查找产品。

> [AZURE.TIP] 避免需要资源 Uri_集合项/集合_比更复杂。

要考虑的另外一点是所有 web 请求都强加在 web 服务器上的负载越大的请求的数目更大负荷。 您应尝试定义您的资源，以避免"花哨"网站公开了大量小资源的 Api。 此类 API 可能要求客户端应用程序提交多个请求才能找到它需要的所有数据。 它可能有利，可使数据非正常化并合并合并为更大的资源可以通过发出一个请求中检索相关的信息。 但是，您需要平衡这种方法对提取可能不会经常要求客户端的数据的开销。 检索大型对象可以增加请求的延迟，并带来额外带宽成本获得很少的优势，如果不经常使用的其他数据。

避免引入到结构、 类型或基础数据源位置的 web API 之间的依赖关系。 例如，如果您的数据位于关系数据库，web API 不需要公开作为资源集合的每个表。 Web API 看作是一种抽象的数据库，并且如有必要引入数据库和 web API 之间的映射层。 这种方式，如果设计或实现的数据库发生更改 （例如，移动从包含的非规范化 NoSQL 存储系统如文档数据库规范化表集合关系数据库） 时客户端应用程序是否会与这些更改。
> [AZURE.TIP] Underpins web API 的数据源不需要的数据存储区中;它可能是另一个服务或业务线应用程序或甚至运行旧版应用程序内部组织内。

最后，它可能无法实现 web API 对特定资源的每个操作映射。 您可以处理此类_非资源_方案通过 HTTP GET 请求调用的功能，并作为 HTTP 响应消息返回的结果。 Web API 实现简单计算器式操作，如加法和减法可以提供 Uri 的公开这些操作作为伪资源并利用查询字符串，以指定所需的参数。 例如 GET 请求的 uri _/add?operand1 = 99 & operand2 = 1_可以返回响应消息的正文包含值 100，并获取请求的 uri _/subtract?operand1 = 50 & operand2 = 20_无法返回响应消息正文包含的值为 30。 但是，只有谨慎地使用这些形式的 Uri。

### <a name="defining-operations-in-terms-of-http-methods"></a>定义的 HTTP 方法的操作

HTTP 协议定义分配给请求的语义含义的方法数。 使用最 rest 风格 web Api 的通用 HTTP 方法是︰

- **获取**，以检索在指定 URI 的资源的副本。 响应消息的正文包含所请求的资源的详细信息。

- **开机自检**，以在指定的 URI 创建新的资源。 请求消息的正文提供新资源的详细信息。 请注意︰ 开机自检也可用于触发不实际创建资源的操作。

- **放置**，以替换或更新在指定 URI 的资源。 请求消息的正文指定要修改的资源和应用的值。

- **删除**来删除在指定 URI 的资源。

> [AZURE.NOTE] HTTP 协议还定义了其他不太常用的方法，如修补程序，用于请求的资源的选择性更新、 头用于请求资源，从而使客户端信息，以获得有关所支持的服务器的通信选项的信息的选项的描述和跟踪它允许客户端请求信息，它可用于测试和诊断目的。

某个特定请求的效果应取决于它所应用于的资源是集合或单独的物料。 下表总结了采用电子商务的例子最 rest 风格实现的常见约定。 请注意，并非所有这些请求可能会实现;这取决于具体情况。

| **资源** | **发布** | **获取** | **放入** | **删除** |
|--------------|----------|---------|---------|------------|
| /customers | 创建新客户 | 检索所有客户 | 批量更新的客户 （_如果实现_) | 删除所有客户 |
| / 客户/1 | 错误 | 检索客户 1 的详细信息 | 更新客户 1 的详细信息，如果它存在，否则为返回错误 | 删除客户 1 |
| /customers/1/orders | 创建新的订单为客户 1 | 检索客户 1 的所有订单 | 大容量更新 （_如果实现_) 1 客户的订单 | 删除客户 1 （_如果实施_) 的所有订单 |

获取和删除请求的目的是比较简单，但混乱有关的目的和效果的开机自检和 PUT 请求的范围。

POST 请求应提供请求的正文中的数据创建新的资源。 在其他模型中，您经常用于 POST 请求资源的集合;新资源添加到集合中。

> [AZURE.NOTE] 您还可以定义 POST 请求，触发某些功能 （和，不一定返回数据），并将这种请求可以应用于集合。 例如可以使用 POST 请求传递到工资单处理服务时间表并获得作为响应计算的税费。

一个 PUT 请求用于修改现有的资源。 如果不存在指定的资源，PUT 请求可能返回一个错误 （在某些情况下，它实际上可能会引发资源）。 PUT 的请求最常应用于单个项目 （如特定客户或订单） 虽然是它们可以应用于集合，尽管这通常不实现的资源。 请注意，PUT 请求是幂等的 POST 请求不可; 而如果应用程序提交相同放多次的请求结果应始终是相同 （相同的资源将被修改具有相同的值），但如果应用程序重复相同的 POST 请求的结果将是创建的多个资源。

> [AZURE.NOTE] 严格地讲，将 HTTP 请求替换为现有的资源请求的正文中指定的资源。 如果目的是要修改所选的资源中的属性，但保持其他属性保持不变，然后这应实现使用修补程序的 HTTP 请求。 但是，许多的 rest 风格实现放宽该规则并使用这两种情况下传。

### <a name="processing-http-requests"></a>处理 HTTP 请求
包含由多个 HTTP 请求和相应的响应消息中客户端应用程序在 web 服务器上，数据会以各种格式 （或媒体类型）。 例如，指定某一客户或订单的详细信息的数据可以作为 XML、 JSON 或一些其他编码和压缩格式。 Rest 风格的 web API 应该支持不同媒体类型的客户端应用程序提交请求的要求。

当客户端应用程序发送消息的正文中返回的数据的请求时，它可以接受请求的标题中指定的媒体类型，它可以处理。 下面的代码阐释了 HTTP GET 请求，检索客户 1 的详细信息并请求的结果返回 json 格式 （客户端仍应检查以验证返回的数据的格式的响应中的数据的媒体类型）︰

```HTTP
GET http://adventure-works.com/orders/2 HTTP/1.1
...
Accept: application/json
...
```

如果 web 服务器支持此媒体类型，它可以包含内容类型标头，指定数据的格式邮件的正文中的响应来应答︰

> [AZURE.NOTE] 最大的互操作性，接受和内容类型标头中引用的类型应为媒体识别 MIME 类型，而不是一些自定义的媒体类型。

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Date: Fri, 22 Aug 2014 09:18:37 GMT
Content-Length: ...
{"orderID":2,"productID":4,"quantity":2,"orderValue":10.00}
```

如果 web 服务器不支持请求的媒体类型，它可以以不同的格式来发送数据。 在所有情况下它必须指定媒体类型 （例如，_应用程序/json_) 中的内容类型标头。 它是要分析响应消息和解释在邮件正文中的结果进行适当的客户端应用程序的责任。

请注意，本示例中，web 服务器成功检索所请求的数据通过返回状态码 200 响应标头中指示成功。 如果未不找到任何匹配的数据，它应当转而返回状态代码 404 （未找到），并响应消息的正文可以包含其他信息。 内容类型标头，指定该信息的格式在下面的示例所示︰

```HTTP
GET http://adventure-works.com/orders/222 HTTP/1.1
...
Accept: application/json
...
```

不存在顺序 222，因此响应消息如下所示︰

```HTTP
HTTP/1.1 404 Not Found
...
Content-Type: application/json; charset=utf-8
...
Date: Fri, 22 Aug 2014 09:18:37 GMT
Content-Length: ...
{"message":"No such order"}
```

当应用程序发送更新资源将 HTTP 请求时，它将指定 URI 的资源并提供的请求消息的正文中要修改的数据。 它还应通过使用内容类型标头中指定此数据的格式。 用于基于文本信息的通用格式是_应用程序/x-www-窗体-urlencoded_，其中包括一套由分隔的名称/值对的 & 字符。 下一个示例演示将 HTTP 请求修改订单 1 中的信息︰

```HTTP
PUT http://adventure-works.com/orders/1 HTTP/1.1
...
Content-Type: application/x-www-form-urlencoded
...
Date: Fri, 22 Aug 2014 09:18:37 GMT
Content-Length: ...
ProductID=3&Quantity=5&OrderValue=250
```

如果修改成功，它理论上应该显示 HTTP 204 状态代码，指示，该进程已被成功地处理，但响应正文包含任何进一步的信息。 在响应中的位置标头包含新更新的资源的 URI:

```HTTP
HTTP/1.1 204 No Content
...
Location: http://adventure-works.com/orders/1
...
Date: Fri, 22 Aug 2014 09:18:37 GMT
```

> [AZURE.TIP] 如果在将 HTTP 请求消息中的数据包括日期和时间信息，请确保您的 web 服务接受日期和时间的格式遵循 ISO 8601 标准。

如果不存在要更新的资源，如上文所述，一个找不到的响应可以响应 web 服务器。 或者，如果服务器实际上创建了对象本身可以返回状态码 HTTP 200 (OK) 或 HTTP 201 （创建） 和响应正文可能包含的数据的新资源。 如果请求的内容类型标头中指定的 web 服务器不能处理的数据格式，它应该响应 HTTP 状态代码 415 （不受支持的媒体类型）。

> [AZURE.TIP] 考虑实现可以批处理更新给多个资源集合中的批量 HTTP PUT 操作。 PUT 请求应指定的 URI 的集合，而请求主体应指定要修改的资源的详细信息。 这种方法可以帮助减少无用信息并提高性能。

创建新资源的 HTTP POST 请求的格式将类似于 PUT 请求;邮件正文中包含要添加的新资源的详细信息。 但是，URI 通常指定可向其中添加资源的集合。 下面的示例创建一个新的订单，并将其添加到订单集合︰

```HTTP
POST http://adventure-works.com/orders HTTP/1.1
...
Content-Type: application/x-www-form-urlencoded
...
Date: Fri, 22 Aug 2014 09:18:37 GMT
Content-Length: ...
productID=5&quantity=15&orderValue=400
```

如果请求成功，web 服务器应响应消息代码的 HTTP 状态码 201 （创建）。 位置标头应包含新创建的资源的 URI 和响应的正文中应包含一份新的资源;内容类型标头指定此数据的格式︰

```HTTP
HTTP/1.1 201 Created
...
Content-Type: application/json; charset=utf-8
Location: http://adventure-works.com/orders/99
...
Date: Fri, 22 Aug 2014 09:18:37 GMT
Content-Length: ...
{"orderID":99,"productID":5,"quantity":15,"orderValue":400}
```

> [AZURE.TIP] 放置或 POST 请求提供的数据是无效的如果 web 服务器应该具有所需的 HTTP 状态代码 400 （错误请求） 的消息。 此消息的正文可以包含有关请求和预期，格式的问题的其他信息，或者它可以包含提供更多详细信息的 URL 的链接。

若要移除资源，删除 HTTP 请求，只是提供要删除资源的 URI。 下面的示例尝试删除顺序 99:

```HTTP
DELETE http://adventure-works.com/orders/99 HTTP/1.1
...
```

如果删除操作成功，web 服务器应响应 HTTP 状态代码 204，表示，该进程已被成功地处理，但响应正文包含任何进一步的信息 （这是相同的响应返回的 PUT 操作成功，但没有作为资源位置标头不存在）。还有可能的删除请求返回 HTTP 状态码 200 （成功） 或 202 （接受），如果异步执行的删除操作。

```HTTP
HTTP/1.1 204 No Content
...
Date: Fri, 22 Aug 2014 09:18:37 GMT
```

如果找不到该资源，web 服务器应返回 404 （未找到） 的消息。

> [AZURE.TIP] 如果需要删除集合中的所有资源，启用删除 HTTP 请求指定的集合而不是强制应用程序又从集合中移除的每个资源的 URI。

### <a name="filtering-and-paginating-data"></a>筛选和对数据进行分页

应尽力保持简单和直观的 Uri。 将通过单一的资源集合公开 URI 可帮助您在这方面，但这会导致应用程序需要的信息的子集时获取大量的数据。 生成大量通信量的影响不仅性能和 web 服务器的可伸缩性，但对请求数据的客户端应用程序的响应性也产生负面影响。

例如，如果订单包含订单支付的价格，需要检索具有成本超过特定值的所有订单的客户端应用程序可能需要从_/orders_ URI 中都检索所有的订单，然后筛选本地这些订单。 这一过程显然是效率极低;它承载 web API 的服务器上会浪费网络带宽和处理能力。

一种解决方案是为了提供如_/orders/ordervalue_greater_than_n_ ，其中_n_是订单的价格，但所有的 URI 方案但有限的数量的价格的这种方法是不切实际的。 此外，如果您需要基于其他标准的查询订单，最终会面临着提供 Uri 与可能的非直观的文件名的长列表。

对筛选的数据更好的策略是提供筛选条件中的查询字符串传递给 web API，如_/orders?ordervaluethreshold = n_。 在此示例中，web API 中的相应操作负责分析和处理`ordervaluethreshold`查询字符串并返回筛选的结果中的 HTTP 响应中的参数。

集合资源通过一些简单的 HTTP GET 请求可能无法返回大量的项目。 为了对抗您这发生的可能性应设计 web API 来限制任何一个请求所返回的数据量。 来做到这一点通过支持使用户能够指定要检索的最大项数的查询字符串 （它本身可能面临上限的限制，以帮助防止拒绝服务攻击），并且集合中的起始偏移量。 例如，在 URI 中的查询字符串_/orders?limit = 25 和偏移量 = 50_应检索 25 开头找到订单集合中的第 50 订单的订单。 在 web API 实现 GET 请求的操作是负责分析和处理与筛选数据， `limit` ，`offset`查询字符串中的参数。 为了帮助客户端应用程序，获得返回分页的数据还应包括某种形式的元数据表示的集合中的可用资源总数的请求。 您也可考虑其他智能页面策略;有关详细信息，请参阅[API 设计备注︰ 智能寻呼](http://bizcoder.com/api-design-notes-smart-paging)

您可以按照类似的策略，它是提取; 对数据进行排序可以提供作为值，如采用字段名称的排序参数_/orders?sort = 产品 id_。 但是，请注意，这种方法会产生 deleterious 缓存 （查询字符串参数构成许多缓存实现用作密钥对数据进行缓存的资源标识符的一部分）。

您可以扩展此方法来限制 （项目） 的字段返回一个单一资源项是否包含大量的数据。 例如，可以使用查询字符串参数可接受的逗号分隔列表中的字段，如_/orders?fields = 产品 Id 数量_。

> [AZURE.TIP] 为查询字符串中的所有可选参数提供有意义的默认值。 例如，设置`limit`参数为 10 和`offset`为 0 的参数如果实现分页，如果订购信息，实现资源的键设置排序参数并设置`fields`参数如果支持计划的资源中的所有字段。

### <a name="handling-large-binary-resources"></a>处理大型二进制资源

将单个资源可能包含大型二进制字段，例如，文件或图像。 为了克服传输问题，引起的不可靠和间歇性连接并缩短响应时间，考虑提供允许通过客户端应用程序以块的形式检索此类资源的操作。 若要执行此操作，web API 应该为 GET 请求，对于较大的资源，支持接受范围标头和理想情况下实现这些资源的 HTTP HEAD 请求。 接受范围标头指示获取操作支持部分的结果，和客户端应用程序可以提交 GET 请求中返回指定为某个范围的字节的资源的一个子集。 HEAD 请求是类似于一个 GET 请求，只不过它只返回描述资源和空消息正文的标题。 客户端应用程序可以发出 HEAD 请求以确定是否要使用部分 GET 请求获取资源。 下面的示例演示 HEAD 请求获取有关产品图像的信息︰

```HTTP
HEAD http://adventure-works.com/products/10?fields=productImage HTTP/1.1
...
```

响应消息中包含页眉，包括资源 （4580 个字节） 的大小和相应 GET 操作支持部分结果的接受范围标头︰

```HTTP
HTTP/1.1 200 OK
...
Accept-Ranges: bytes
Content-Type: image/jpeg
Content-Length: 4580
...
```

客户端应用程序可以使用此信息来构造一系列 GET 操作来检索图像较小的块。 第一个请求提取使用 Range 标头的前 2500 个字节︰

```HTTP
GET http://adventure-works.com/products/10?fields=productImage HTTP/1.1
Range: bytes=0-2499
...
```

响应消息表示，这是通过返回 HTTP 状态代码 206 部分响应。 内容长度标头指定邮件正文 （不是大小的资源） 中返回的字节的实际数目和内容范围标头所指明这是哪一部分的资源 (第 0 2499年出的 4580 个字节):

```HTTP
HTTP/1.1 206 Partial Content
...
Accept-Ranges: bytes
Content-Type: image/jpeg
Content-Length: 2500
Content-Range: bytes 0-2499/4580
...
_{binary data not shown}_
```

后面的请求从客户端应用程序可以通过使用适当的 Range 标头检索资源的其余部分︰

```HTTP
GET http://adventure-works.com/products/10?fields=productImage HTTP/1.1
Range: bytes=2500-
...
```

相应的结果消息应如下所示︰

```HTTP
HTTP/1.1 206 Partial Content
...
Accept-Ranges: bytes
Content-Type: image/jpeg
Content-Length: 2080
Content-Range: bytes 2500-4580/4580
...
```

## <a name="using-the-hateoas-approach-to-enable-navigation-to-related-resources"></a>使用 HATEOAS 方法启用指向相关资源的导航

其余的主要动机之一是它应该是可能而无需事先了解 URI 方案定位资源的整个一套。 每个 HTTP GET 请求应该返回的信息有必要找到与请求的对象超链接包含在响应中，通过直接相关的资源，它还应提供与描述可用的操作，对每种资源的信息。 这一原则被称为 HATEOAS 或作为引擎应用程序状态的超文本。 系统实际上是一个有限状态机，并对每个请求的响应包含的信息需要从一个状态变动到另一个;没有其他信息应当有必要。

> [AZURE.NOTE] 当前没有任何标准或规范，定义了如何建模 HATEOAS 原则。 在本部分中所示的示例演示一个可能的解决方案。

举一个例子，以处理客户订单，之间的关系在特定订单的响应中返回的数据应包含 Uri 标识下订单的客户，该客户可以执行的操作的超链接的形式。

```HTTP
GET http://adventure-works.com/orders/3 HTTP/1.1
Accept: application/json
...
```

响应消息的正文包含`links`（突出显示的代码示例中） 的数组，它指定关系 （_客户_） 的客户 (_http://adventure-works.com/customers/3_)，URI 的性质如何检索 (_获取_)，该客户和用于检索此信息 （_文本/xml_和_应用程序/json_） 的 web 服务器支持的 MIME 类型的详细信息。 这是客户端应用程序需要能够读取的详细信息的客户的所有信息。 此外，该链接数组还包含链接的其他可执行，如投入 （修改的客户，以及 web 服务器需要客户端提供的格式），并删除的操作。

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Content-Length: ...
{"orderID":3,"productID":2,"quantity":4,"orderValue":16.60,"links":[(some links omitted){"rel":"customer","href":" http://adventure-works.com/customers/3", "action":"GET","types":["text/xml","application/json"]},{"rel":"
customer","href":" http://adventure-works.com /customers/3", "action":"PUT","types":["application/x-www-form-urlencoded"]},{"rel":"customer","href":" http://adventure-works.com /customers/3","action":"DELETE","types":[]}]}
```

为了完整起见，链接数组还应包括自引用属于已检索的资源的信息。 这些链接前面的示例中省略了，但下面代码中突出显示。 注意到在这些链接，关系_自我_已被用来指出这是对由操作返回的资源的引用︰

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Content-Length: ...
{"orderID":3,"productID":2,"quantity":4,"orderValue":16.60,"links":[{"rel":"self","href":" http://adventure-works.com/orders/3", "action":"GET","types":["text/xml","application/json"]},{"rel":" self","href":" http://adventure-works.com /orders/3", "action":"PUT","types":["application/x-www-form-urlencoded"]},{"rel":"self","href":" http://adventure-works.com /orders/3", "action":"DELETE","types":[]},{"rel":"customer",
"href":" http://adventure-works.com /customers/3", "action":"GET","types":["text/xml","application/json"]},{"rel":" customer" (customer links omitted)}]}
```

使此方法生效，必须准备客户端应用程序来检索和分析此附加的信息。

## <a name="versioning-a-restful-web-api"></a>Rest 风格 web API 版本控制

它不太可能，但最简单的 web API 将保持不变的情况下所有。 随着业务需求变化的新集合可能添加的资源、 资源之间的关系可能会改变，并可能修正的资源中的数据结构。 虽然更新 web API 来处理新的或不同的要求是一个相对简单的过程，您必须考虑此类更改将对客户端应用程序使用 web API 的效果。 问题是控制的尽管设计和实现 web API，开发人员可以完全控制该 API，开发人员不具有相同程度可能由第三方组织远程操作生成的客户端应用程序。 主要的迫切需要是使现有客户端应用程序能够继续工作，同时允许新客户端应用程序可以利用新的功能和资源保持不变。

版本控制使 web API 用来指示的功能和它公开的资源和客户端应用程序可以提交请求定向到特定版本的功能或资源。 以下各节描述了几种不同的方法，每个都有自己的优点和权衡。

### <a name="no-versioning"></a>无版本控制

这是最简单的方法，是可以接受的一些内部 Api。 巨大的变革可以表示为新资源或新的链接。  将内容添加到现有的资源不可能存在重大更改为客户端应用程序不应看到此内容只是忽略它。

例如，对 URI _http://adventure-works.com/customers/3_的请求应返回包含单个客户的详细信息`id`， `name`，和`address`的客户端应用程序所需的字段︰

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Content-Length: ...
{"id":3,"name":"Contoso LLC","address":"1 Microsoft Way Redmond WA 98053"}
```

> [AZURE.NOTE] 出于简单性和明确性，此部分中显示的示例响应不包括 HATEOAS 链接。

如果`DateCreated`字段添加到架构中的客户资源，然后响应如下所示︰

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Content-Length: ...
{"id":3,"name":"Contoso LLC","dateCreated":"2014-09-04T12:11:38.0376089Z","address":"1 Microsoft Way Redmond WA 98053"}
```

现有的客户端应用程序可能仍然能够忽略无法识别的字段，可以设计新的客户端应用程序来处理该新字段是否工作正常。 但是，如果资源的架构更彻底地改变发生 （例如删除或重命名字段） 或资源之间的关系改变这些可能构成重大更改，以防止现有的客户端应用程序无法正常。 在这些情况下应考虑下列方法之一。

### <a name="uri-versioning"></a>URI 版本控制

每次您修改 web API 或更改架构的资源，您将版本号添加到每个资源的 URI。 先前存在的 Uri 应继续像以前一样，操作将遵循的资源返回到其原始架构。

如果扩展前面的示例中，`address`字段包含地址的每个构成部分的子字段重构 (如`streetAddress`， `city`， `state`，和`zipCode`)，可以通过 URI 包含版本号，如 http://adventure-works.com/v2/customers/3 公开此版本的资源︰

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Content-Length: ...
{"id":3,"name":"Contoso LLC","dateCreated":"2014-09-04T12:11:38.0376089Z","address":{"streetAddress":"1 Microsoft Way","city":"Redmond","state":"WA","zipCode":98053}}
```

此版本控制机制非常简单，但取决于服务器路由到适当的终结点的请求。 但是，它会变得笨拙作为 web API 通过几个迭代的逐渐成熟，服务器必须支持多个不同的版本。 此外，从 purist 的角度来看，在所有情况下客户端应用程序提取相同的数据 （客户 3），因此，URI 不确实应该根据版本的不同。 这种方案也复杂化 HATEOAS 实现为所有链接将都需要在其 Uri 中包括的版本号。

### <a name="query-string-versioning"></a>查询字符串版本控制

不提供多个 Uri，而是可以通过使用查询字符串附加到 HTTP 请求，如_http://adventure-works.com/customers/3?version=2_中的参数指定资源的版本。 如果省略旧客户端应用程序有意义的值，例如 1 应默认版本参数。

这种方法的优点语义相同的资源始终从相同的 URI，但这取决于处理以解析查询字符串，然后发送回相应的 HTTP 响应该请求的代码。 此方法还面临实现 HATEOAS 作为 URI 版本控制机制的同一个复杂因素。

> [AZURE.NOTE] 某些较旧的 web 浏览器和 web 代理将不会缓存在 URL 包含查询字符串的请求的响应。 这可以对使用 web API 并从这样的 web 浏览器中运行的 web 应用程序的性能有不利影响。

### <a name="header-versioning"></a>标头版本控制

而不是追加作为查询字符串参数的版本号，您可以实现自定义标头指示资源的版本。 此方法要求客户端应用程序的任何请求，添加适当的头虽然处理客户端请求的代码可以使用默认值 （版本 1），如果未指定版本标头。 下面的示例使用名为的_自定义标头_的自定义标头。 此标头的值指示 web API 的版本。

版本 1:

```HTTP
GET http://adventure-works.com/customers/3 HTTP/1.1
...
Custom-Header: api-version=1
...
```

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Content-Length: ...
{"id":3,"name":"Contoso LLC","address":"1 Microsoft Way Redmond WA 98053"}
```

版本 2:

```HTTP
GET http://adventure-works.com/customers/3 HTTP/1.1
...
Custom-Header: api-version=2
...
```

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/json; charset=utf-8
...
Content-Length: ...
{"id":3,"name":"Contoso LLC","dateCreated":"2014-09-04T12:11:38.0376089Z","address":{"streetAddress":"1 Microsoft Way","city":"Redmond","state":"WA","zipCode":98053}}
```

请注意，与前面两种方法，实现 HATEOAS 需要包括相应的自定义标头中的任何链接。

### <a name="media-type-versioning"></a>媒体类型版本控制

当客户端应用程序发送到 web 服务器的 HTTP GET 请求时它应该规定它可以处理使用 Accept 标头中，本指南中前面所述内容的格式。 经常_接受_标题的目的是响应的允许客户端应用程序指定的正文是响应的应 XML、 JSON 或一些其他客户机可以分析的通用格式。 但是，也可以定义自定义的媒体类型，包括信息使客户端应用程序指明它需要哪个版本的资源。 下面的示例显示指定_Accept_标头的值_application/vnd.adventure-works.v1+json_请求。 _Vnd.adventure works.v1_元素指示 web 服务器的_json_元素指定响应正文的格式应为 JSON 时应返回的资源，第 1 版︰

```HTTP
GET http://adventure-works.com/customers/3 HTTP/1.1
...
Accept: application/vnd.adventure-works.v1+json
...
```

处理请求的代码负责处理_Accept_标头和遵守它尽最大可能 （客户端应用程序可以在_Accept_标头，指 web 服务器可以在其中选择最适当响应正文格式指定多种格式）。 Web 服务器使用的内容类型标头确认响应正文中的数据的格式︰

```HTTP
HTTP/1.1 200 OK
...
Content-Type: application/vnd.adventure-works.v1+json; charset=utf-8
...
Content-Length: ...
{"id":3,"name":"Contoso LLC","address":"1 Microsoft Way Redmond WA 98053"}
```

如果 Accept 标头不指定任何已知的介质类型时，web 服务器无法生成 HTTP 406 （不可接受） 的响应消息或返回默认媒体类型的邮件。

此方法可以说 purest 的版本控制机制，并且于自然 HATEOAS，可以在资源链接中包含相关数据的 MIME 类型。

> [AZURE.NOTE] 选择一个版本控制策略时，还应考虑对性能的影响特别 web 服务器上的缓存。 URI 版本管理和查询字符串版本化方案是缓存友好 inasmuch 指相同的数据每次相同的 URI/查询字符串组合。

> 标头版本控制和媒体类型版本控制机制通常需要其他逻辑来检查这个自定义页眉或 Accept 标头中的值。 在大型环境中，多个客户端使用不同版本的 web API 可能会导致大量重复的服务器端缓存中的数据。 此问题可能会变得与通过代理实现缓存，并且，如果它当前不持有一份请求的数据在其缓存中仅转发到 web 服务器的请求的 web 服务器进行通信的客户端应用程序的情况下锐音符。

## <a name="more-information"></a>详细信息

- [Rest 风格手册](http://restcookbook.com/)包含构建 rest 风格的 Api 的介绍。
- Web [API 清单](https://mathieu.fenniak.net/the-api-checklist/)包含有用的设计和实现 Web API 时要考虑的项目列表。
