<properties 
    pageTitle="企业到企业连接器和 API 应用程序中的逻辑应用程序 |Microsoft Azure" 
    description="了解如何创建和配置 EDI、 EDIFACT、 AS2 和 TPM 接头;microservices 的体系结构" 
    services="logic-apps" 
    documentationCenter="" 
    authors="MandiOhlinger" 
    manager="anneta" 
    editor=""/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/18/2016" 
    ms.author="mandia"/> 

# <a name="business-to-business-connectors-and-api-apps"></a>企业到企业连接器和 API 的应用程序

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]

逻辑应用程序包含多个 BizTalk API 应用程序集成环境至关重要。 这些 API 应用程序基于概念和 BizTalk Server 中使用的工具，但目前有逻辑的应用程序的一部分。 

这些 API 的应用程序的一个类别是企业对企业 (B2B) API 的应用程序。 使用这些 B2B API 的应用程序，您可以轻松地添加合作伙伴、 创建协议，以及做一切为您将内部使用 EDI、 AS2 和 EDIFACT。  

这些 B2B API 应用程序提供"触发器"和"操作"的功能。 触发器启动基于特定的事件，如 X12 的到达一个新的实例来自伙伴的消息。 操作接收 X12 后像是结果，消息，然后使用 AS2 发送消息。


## <a name="what-is-a-business-to-business-connector-or-api-apps"></a>企业到企业接口或 API 的应用程序是什么
企业到企业 (B2B) 功能包括现有的预构建 API 应用程序允许不同的公司、 部门、 应用程序等等进行通信使用 AS2、 EDI 和 EDIFACT。 

B2B API 应用程序包括︰ 

连接器或 API 的应用程序 | 说明
--- | ---
BizTalk 贸易合作伙伴管理 | API 应用程序可创建使用合作伙伴和协议的企业到企业 (B2B) 关系。 这些关系利用 AS2、 EDIFACT、 和 X12 协议。<br/><br/>TPM API 应用程序是 AS2 连接器和 X12 或 EDIFACT API 的应用程序的基本要求。 
AS2 连接器 | 接收和发送邮件使用 AS2 传输连接器。 连接器在 Internet 上传输数据安全可靠地。
BizTalk EDIFACT | 接收和发送邮件使用 EDIFACT API 应用程序。 EDIFACT 通常也被称为 UN/EDIFACT （联合国/电子数据交换的管理、 商务和运输） 和各行业广泛使用。
BizTalk X12 | 接收和发送邮件使用 X12 API 应用程序协议。 X12 通常也被称为 ASC X 12 （鉴定标准委员会 X12） 和各行业广泛使用。 


使用这些 API 的应用程序，可以完成不同 EDI 消息的任务。 例如，使用 AS2 连接器，您可以安全地接收和发送消息的不同类型 (XML、 EDI 平面文件，等等) 给客户，公司内部的部门喜欢人力资源，或使用 AS2 的任何人。 

您可以创建任意多个 API 应用程序根据需要轻松地创建它们。 您还可以重用一个 API 的应用程序在多个方案或工作流中。

您可以这样做而无需编写任何代码。 我们开始吧。 


## <a name="requirements-to-get-started"></a>若要开始的要求
当您创建 B2B API 的应用程序时，有一些所需的资源。 其他 API 应用程序中使用它们之前，必须由您创建这些项目。 这些要求包括︰ 

要求 | 说明
--- | ---
SQL azure 数据库 | 存储 B2B 项目包括合作伙伴、 架构、 证书和 agreeements。 每个 B2B API 应用程序需要自己 Azure 的 SQL 数据库。 <br/><br/>**请注意**将连接字符串复制到此数据库。<br/><br/>[创建 SQL Azure 数据库](../sql-database/sql-database-get-started.md)
Azure Blob 存储容器 | 启用 AS2 存档后，商店将消息属性。 如果您不需要 AS2 邮件存档，不需要存储容器。 <br/><br/>**请注意**如果要启用存档，请复制到此 Blob 存储的连接字符串。<br/><br/>[关于 Azure 存储帐户](../storage/storage-create-storage-account.md)
服务总线 Namespace 和其密钥值 | X12 和 EDIFACT 分批处理数据存储。 如果您不需要批处理，则不需要服务总线命名空间。<br/><br/>**请注意**如果启用批处理时，将复制这些值。<br/><br/>[创建服务总线 Namespace](http://msdn.microsoft.com/library/azure/hh690931.aspx)
TPM 实例 | 创建 AS2 连接器和 X12 或 EDIFACT API 的应用程序需要一个 BizTalk 贸易合作伙伴管理 (TPM) 实例。 创建 TPM API 应用程序时，您将创建 TPM 实例。 <br/><br/>**请注意**知道您的 TPM API 应用程序的名称。 


## <a name="create-the-api-apps"></a>创建 API 的应用程序
可以使用 Azure 门户或使用 REST Api 创建 B2B API 的应用程序。 


### <a name="create-the-api-apps-using-rest-apis"></a>创建 API 应用程序使用 REST Api
[有关如何使用 REST Api，请参阅文档。](http://go.microsoft.com/fwlink/p/?LinkId=529766)


### <a name="create-the-b2b-api-apps-in-the-azure-portal"></a>在 Azure 的门户网站中创建 B2B API 应用程序
在 Azure 的门户中，可以创建逻辑的应用程序、 Web 应用程序和移动应用程序时创建 B2B API 的应用程序。 或者，您可以创建一个使用其自己的刀片式服务器。 这两种方式很容易，因此这取决于您的需求或首选项。 有些用户更喜欢先创建所有 B2B API 应用程序具有其特定的属性。 然后，创建逻辑应用程序/Web 应用程序/移动应用程序，并添加创建 B2B API 应用程序。  

以下步骤将创建 B2B API 应用程序使用 API 应用程序刀片式服务器。


#### <a name="create-the-biztalk-trading-partner-management-tpm-api-apps"></a>创建的 BizTalk 贸易合作伙伴管理 (TPM) API 的应用程序

> [AZURE.NOTE] 创建 AS2 连接器和 X12 或 EDIFACT API 的应用程序需要一个 BizTalk 贸易合作伙伴管理 (TPM) 实例。 创建 TPM API 应用程序时，您将创建 TPM 实例。

以下步骤将创建 TPM 实例︰

1. 在 Azure 门户 Startboard （主页） 中，选择**市场**。 **API 的应用程序**列出了所有现有 API 的应用程序和连接器。 您还可以**搜索**特定的 B2B API 的应用程序。
2. **贸易合作伙伴管理 BizTalk**中选择。 在新的刀片，选择**创建**。 
3. 输入的属性︰ 

    属性 | 说明
--- | ---
名称 | 输入 TPM 实例的任何名称。 例如，您可以其命名为*AccountsPayableTPM*。
程序包设置 | 输入您创建的 Azure SQL 数据库 ADO.NET**数据库连接字符串**。 <br/><br/>复制连接字符串时，密码不会添加到连接字符串。 请确保在连接字符串中输入密码。
应用程序服务计划 | 列出您的付款计划。 如果您需要更多或更少的资源，您可以更改它。
定价层 | 列出在 Azure 订购的价格类别的只读属性。 
资源组 | 创建一个新或使用现有的组。 所有 API 的应用程序和逻辑应用程序、 Web 应用程序和移动应用程序连接器必须都是同一资源组中。 <br/><br/>[使用资源组](../azure-resource-manager/resource-group-overview.md)说明此属性。 
订阅 | 只读属性，该属性列出了您的当前订购。
位置 | 您 Azure 服务的宿主的地理位置。 
将添加到 Startboard | 选择此选项可将 B2B API 应用程序添加到您的 Starboard （主页）。

4. 选择**创建**。 

创建 TPM API 应用程序 （TPM 实例） 后，可以再创建 AS2 连接器和/或 X12 或 EDIFACT API 的应用程序。 


#### <a name="create-the-as2-connector"></a>创建 AS2 连接器

1. 在 Azure 门户 Startboard （主页） 中，选择**市场**。 **API 的应用程序**列出了所有现有 API 的应用程序和连接器。 您还可以**搜索**特定的 B2B API 的应用程序。
2. 选择**AS2 连接器**。 在新的刀片，选择**创建**。 
3. 输入的属性︰ 

    属性 | 说明
--- | ---
名称 | 输入 AS2 连接器的任何名称。 例如，您可以其命名为*AS2Connector*。
程序包设置 | 该 API 应用程序中，如 TPM 实例名称输入特定的设置。 <br/><br/>在此主题中的特定属性，请参阅[添加 AS2 包设置](#AddAS2Conn)。 
应用程序服务计划 | 列出您的付款计划。 如果您需要更多或更少的资源，您可以更改它。
定价层 | 列出在 Azure 订购的价格类别的只读属性。 
资源组 | 创建一个新或使用现有的组。 [使用资源组](../azure-resource-manager/resource-group-overview.md)说明此属性。 
订阅 | 只读属性，该属性列出了您的当前订购。
位置 | 您 Azure 服务的宿主的地理位置。 
将添加到 Startboard | 选择此选项可将 B2B API 应用程序添加到您的 Starboard （主页）。

    **<a name="AddAS2Conn"></a>AS2 连接器包设置**

    属性 | 说明
--- | --- 
数据库连接字符串 | 输入您创建的 Azure SQL 数据库的 ADO.NET 连接字符串。 复制连接字符串时，密码不会添加到连接字符串。 请确保在连接字符串中输入密码，在粘贴之前。
启用传入消息存档 | 可选。 启用此属性来存储传入 AS2 消息从伙伴处接收的消息属性。 
Azure Blob 存储连接字符串  | 输入您创建的 Azure Blob 存储容器的连接字符串。 当启用存档时，编码和解码的消息存储在此存储容器。
TPM 实例名称 | 输入的**BizTalk 贸易合作伙伴管理**API 应用程序以前创建的名称。 当创建 AS2 连接器时，该连接器执行 AS2 协议将在此特定的 TPM 实例。

4. 选择**创建**。 


#### <a name="create-the-x12-or-edifact-api-apps"></a>创建 X12 或 EDIFACT API 应用程序

1. 在 Azure 门户 Startboard （主页） 中，选择**市场**。 **API 的应用程序**列出了所有现有 API 的应用程序和连接器。 您还可以**搜索**特定的 B2B API 的应用程序。
2. 选择**BizTalk X 12**或**BizTalk EDIFACT**。 在新的刀片，选择**创建**。 
3. 输入的属性︰ 

    属性 | 说明
--- | ---
名称 | 输入任何 B2B API 应用程序的名称。 例如，您可以其命名为*EDI850APIApp*。
程序包设置 | 该 API 应用程序中，如 TPM 实例名称输入特定的设置。 <br/><br/>在此主题中的特定属性，请参阅[X12 或 EDIFACT 包设置](#AddX12)。 
应用程序服务计划 | 列出您的付款计划。 如果您需要更多或更少的资源，您可以更改它。
定价层 | 列出在 Azure 订购的价格类别的只读属性。 
资源组 | 创建一个新或使用现有的组。 [使用资源组](../azure-resource-manager/resource-group-overview.md)说明此属性。 
订阅 | 只读属性，该属性列出了您的当前订购。
位置 | 您 Azure 服务的宿主的地理位置。 
将添加到 Startboard | 选择此选项可将 B2B API 应用程序添加到您的 Starboard （主页）。

    **<a name="AddX12"></a>X12 和 EDIFACT API 应用程序程序包设置**  

    属性 | 说明
--- | --- 
数据库连接字符串 | 输入您创建的 Azure SQL 数据库的 ADO.NET 连接字符串。 复制连接字符串时，密码不会添加到连接字符串。 请确保在连接字符串中输入密码，在粘贴之前。
服务总线 Namespace | 输入您创建的服务总线名称空间。 仅当启用批处理的要求。 
服务总线 Namespace 共享访问键名称 | 输入服务总线命名空间创建访问键。 仅当启用批处理的要求。 
服务总线 Namespace 共享访问密钥值 | 输入服务总线命名空间创建访问键的值。 仅当启用批处理的要求。 
TPM 实例名称 | 输入的**BizTalk 贸易合作伙伴管理**API 应用程序以前创建的名称。 当您创建 X12 或 EDIFACT API 的应用程序时，此 API 的应用程序执行 X12/EDFIACT 协议将在此特定的 TPM 实例。

4. 选择**创建**。 


## <a name="add-your-partners-agreements-certificates-and-schemas"></a>添加您的伙伴、 协议、 证书和架构 
在 Azure 的门户中，打开您 TPM API 的应用程序。 在**组件**部分中，添加您的伙伴、 协议、 证书和架构。 

您还可以添加到您 AS2 连接器，X12 协议 API 的应用程序，并且 EDIFACT API 的应用程序。 


## <a name="monitor-your-api-apps"></a>监视您的 API 应用程序
在 Azure 的门户中，打开您 TPM API 的应用程序。 在**操作**部分中，您可以查看不同的管理操作。 例如，您可以︰

- 查看信息和错误事件
- 查看内存使用情况和线程计数的工作进程 (w3wp)
- 查看应用程序和 web 服务器日志

更在[监视您的逻辑的应用程序](app-service-logic-monitor-your-logic-apps.md)。


## <a name="add-the-api-apps-to-your-application"></a>添加到您的应用程序的 API 的应用程序 
Microsoft Azure 应用程序服务公开不同的应用程序类型，可以使用这些 B2B API 的应用程序。 您可以创建一个新或现有 B2B API 应用程序中添加逻辑应用程序、 移动应用程序或 Web 应用程序。 

在您的应用程序，只需选择 B2B API 应用程序库中会自动将其添加到您的应用程序。  

> [AZURE.IMPORTANT] 若要添加连接器和 API 以前创建的应用程序，创建逻辑应用程序、 移动应用程序或 Web 应用程序在同一资源组中。 

以下步骤将 B2B API 应用程序添加到逻辑应用程序、 移动应用程序或 Web 应用程序︰ 

1. 在 Azure 门户 Startboard （主页） 中，进入**市场**，并为您的逻辑、 移动设备、 或 Web 应用程序的搜索。 

    如果您正在创建新应用程序，搜索逻辑应用程序、 移动应用程序或 Web 应用程序。 选择应用程序，这一新的刀片在选择**创建**。 [创建一个逻辑应用程序](app-service-logic-create-a-logic-app.md)列出的步骤。 

2. 打开您的应用程序并选择**触发器和操作**。 

3. 从**库**中，选择 B2B API 应用程序，自动将其添加到您的应用程序。 您还可以创建一个新的 B2B API 的应用程序。

    > [AZURE.IMPORTANT] AS2 连接器，X12 EDIFACT API 的应用程序需要 TPM 实例。 因此，如果您正在创建新的 B2B API 应用程序中，第一，创建 TPM API 的应用程序，然后再创建 AS2 连接器，X12 API 应用程序中或 EDIFACT API 的应用程序。 

4. 选择**确定**以保存所做的更改。 

>[AZURE.NOTE] 若要开始使用 Azure 逻辑应用程序怎样 Azure 帐户[重试逻辑应用程序](https://tryappservice.azure.com/?appservice=logic)之前。 您可以立即创建短期初学者逻辑应用程序。 没有信用卡，所需;没有承诺。

## <a name="more-b2b-resources"></a>更多的 B2B 资源

[创建 B2B 进程](app-service-logic-create-a-b2b-process.md)<br/>
[创建一个贸易伙伴协议](app-service-logic-create-a-trading-partner-agreement.md)<br/>
[连接器和 BizTalk API 的应用程序是什么](app-service-logic-what-are-biztalk-api-apps.md)


## <a name="read-about-logic-apps-and-web-apps"></a>了解逻辑应用程序和 Web 应用程序
[逻辑应用程序有哪些？](app-service-logic-what-are-logic-apps.md)<br/>
[网站和 Web 应用程序在 Azure 应用程序服务](../app-service-web/app-service-web-overview.md)


## <a name="more-connectors"></a>多个连接器

[连接器和 API 的应用程序列表](app-service-logic-connectors-list.md)<br/><br/>
[连接器和 BizTalk API 的应用程序是什么](app-service-logic-what-are-biztalk-api-apps.md) 
