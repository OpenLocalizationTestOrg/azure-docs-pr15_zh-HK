<properties
    pageTitle="列表中可用的连接器和 API 的应用程序 |Microsoft Azure 应用程序服务"
    description="阅读有关连接器和 API 在 Azure 应用程序服务的应用程序"
    services="logic-apps"
    documentationCenter=""
    authors="MandiOhlinger"
    manager="erikre"
    editor="cgronlun"/>

<tags
    ms.service="logic-apps"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/01/2016"
    ms.author="mandia"/>


# <a name="list-of-connectors-and-api-apps-to-use-in-your-logic-apps"></a>要在您的逻辑应用程序中使用的连接器和 API 的应用程序的列表
>[AZURE.NOTE] 本版文章适用于逻辑应用程序 2014年-12-01-预览架构版本。 逻辑应用程序一般可用性 (GA) 的版本，请参阅[新连接器的列表](../connectors/apis-list.md)。

了解所有可用的连接器和 API 由 Microsoft 中使用逻辑应用程序中创建的应用程序。

定价信息以及什么是每一个服务层中包含的列表，请参阅[Azure 应用程序服务定价](https://azure.microsoft.com/pricing/details/app-service/)。

> [AZURE.NOTE] 入门的逻辑应用程序在 Azure 帐户注册之前，转到[重试逻辑应用程序](https://tryappservice.azure.com/?appservice=logic)。 应用程序服务中，可以立即创建短期初学者逻辑应用程序。 没有信用卡，所需;没有承诺。

## <a name="core-connectors"></a>核心连接器
下表列出了所有可用的连接器和 API 的应用程序由 Microsoft 创建可用作核心连接器︰

名称 | 说明
--- | ---
[Bing 在线翻译](https://azure.microsoft.com/marketplace/partners/bing/microsofttranslator/) | 使用 Bing 将文字翻译成另一种语言。
[HTTP](app-service-logic-connector-http.md) | HTTP 侦听程序打开作为 HTTP 服务器并侦听传入的 HTTP 或 HTTPS 请求的端点。 HTTP 操作不需要应用程序 API 和本身的逻辑应用程序内支持。
[可宽延时间](app-service-logic-connector-slack.md) | 连接到可宽延时间，并将消息张贴到可宽延时间的通道。


## <a name="enterprise-integration-connectors"></a>企业集成连接器
下表列出了所有可用连接器和由 Microsoft 可用作企业集成接口的 API 应用程序︰

名称  | 说明
------------- | -------------
[BizTalk 规则](app-service-logic-use-biztalk-rules.md) | 使用 BizTalk 规则来定义和控制组织中的业务逻辑。 无需重新编译或无需重新部署关联的应用程序，业务策略可以进行更新。
[BizTalk XPath 提取程序](app-service-logic-xpath-extract.md) | 查找并从基于的 XPath 选择的 XML 内容中提取数据。
[DB2 连接器](app-service-logic-connector-db2.md) | 连接到 IBM 的 DB2 数据库内部和 Azure 的虚拟机上运行 Windows 操作系统的系统。 可以将 Web API 和 OData API 操作映射到 Informix 结构化查询语言命令。 <br/><br/>没有触发器。 操作包括表选择、 插入、 更新、 删除和自定义语句<br/><br/>此连接器还包括 DRDA Informix 服务器使用 TCP/IP 网络连接 Microsoft 客户端。
[文件](app-service-logic-connector-file.md) | 使用此连接器，您可以连接到本地文件系统或网络和完全不同的文件的任务，包括上传和删除、 列出文件等等。
[Informix](app-service-logic-connector-informix.md) | 连接到 IBM Informix 数据库，内部和 Azure 运行 Windows 操作系统的虚拟机上。 可以将 Web API 和 OData API 操作映射到 Informix 结构化查询语言命令。<br/><br/>没有触发器。 操作包括表选择、 插入、 更新、 删除和自定义语句。<br/><br/>在使用内部部署时，可以使用 VPN 或 Azure ExpressRoute。 此连接器还包括 Microsoft 客户端进行 DRDA Informix 服务器使用 TCP/IP 网络连接。
[Microsoft SQL Server](app-service-logic-connector-sql.md) | 连接到本地 SQL Server 或 SQL Azure 数据库。 可以创建、 更新、 获取，并删除 SQL 数据库表上的项。
MQ | 连接到 IBM WebSphere MQ 服务器版本 8，内部和 Azure 运行 Windows 操作系统的虚拟机上。 在使用内部部署时，可以使用 VPN 或 Azure ExpressRoute。 连接器还包括为 MQ 的 Microsoft 客户端。<br/><br/>没有触发器。 没有操作。<br/><br/>**请注意**当前无法使用与应用程序逻辑。

## <a name="connectors-as-triggers"></a>为触发器的连接器
多个连接器提供触发器逻辑的应用程序。 这些触发器有两种类型︰

1. 轮询触发器︰ 这些触发器轮询您按指定的频率来检查有新的数据的服务。 当新数据可用时，您的逻辑应用程序的新实例运行的数据作为输入。 以防止正在使用相同的数据多次，触发器可能清理数据被读取并传递给应用程序的逻辑。 这种接头的示例包括文件、 SQL 和 Azure 存储。
2. 推触发器︰ 这些触发器侦听终结点上的数据或事件的发生。 然后，触发逻辑应用程序的新实例。 这种接头的示例包括 HTTP 侦听器和 Twitter。

## <a name="connectors-as-actions"></a>为行动的连接器
连接器还可以用作您的逻辑应用程序中的操作。 操作可用于查找逻辑应用程序中，可以随后将用于执行数据。 例如，您可能需要查找数据从 SQL 数据库有关客户的其他信息处理订单时。 或者，您可能需要编写、 更新或删除目标中的数据。 你可以使用连接器提供的操作。 操作映射到 API 应用程序中的操作 （按定义其 Swagger 元数据）。

## <a name="create-your-own-connectors-and-api-apps"></a>创建您自己的连接器和 API 的应用程序
[连接器和应用程序的 API 参考](http://aka.ms/appservicesconnectorreference)  
[Azure 应用程序服务 API 的应用程序的触发器](../app-service-api/app-service-api-dotnet-triggers.md)  
[逻辑应用程序引用](https://msdn.microsoft.com/library/azure/dn948510.aspx)

## <a name="more-on-connectors-and-api-apps"></a>连接器和 API 的应用程序的更多信息
[连接器和 BizTalk API 的应用程序是什么](app-service-logic-what-are-biztalk-api-apps.md)  
[在 Azure 应用程序服务使用混合连接管理器](app-service-logic-hybrid-connection-manager.md)  
[管理和监视您的内置 API 的应用程序和连接器](app-service-logic-monitor-your-connectors.md)
