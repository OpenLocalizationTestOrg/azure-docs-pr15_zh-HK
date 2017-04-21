<properties 
   pageTitle="在 Azure 应用程序服务创建 B2B 流程 |Microsoft Azure" 
   description="简要介绍了如何创建业务到业务流程" 
   services="logic-apps" 
   documentationCenter=".net,nodejs,java" 
   authors="rajram" 
   manager="erikre" 
   editor=""/>

<tags
   ms.service="logic-apps"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="04/20/2016"
   ms.author="rajram"/>

# <a name="creating-a-b2b-process"></a>创建 B2B 进程

[AZURE.INCLUDE [app-service-logic-version-message](../../includes/app-service-logic-version-message.md)]


## <a name="business-scenario"></a>业务方案 
Contoso 和罗斯都是两个商业伙伴。 Contoso （零售商） 通过 AS2 如工业级传输到罗斯 （供应商） 发送采购订单。 罗斯在其云存储中存储所有传入的订单。 采购订单的 XML 消息，这些两个伙伴之间。 一旦邮件存储在罗斯文的云存储罗斯的内部进程对在处理从那个时间点的顺序。
 
本教程的目标是建立罗斯文可以如何建立业务流程，通过它可以通过 AS2 从其伙伴 Contoso 接收的消息 （采购订单的 XML 中），然后将其保留在它的云存储。


## <a name="capabilities-demonstrated"></a>功能说明 
本教程可以帮助展示了以下功能︰ 

- **邮件运输**︰ 零售商和供应商可以在不同的平台上，但它们仍可以实现二者之间的通信。 在本教程中它们进行通信通过 AS2 (适用性语句 2)。 AS2 是在企业对企业通讯中的贸易伙伴之间的数据传输的常用方法。
- **数据持久性**︰ 一旦消息接收通过 AS2 然后罗斯想要将其保留在进一步处理之前。 它可以使用连接器来保持其云存储中的邮件。 在本教程中 Azure Blob 也在利用云存储系统的罗斯文。
- **创建业务流程**: 流程中多个 API 的应用程序可以被缝合在一起，如下所示实现业务成效。


## <a name="before-you-begin"></a>在开始之前
本教程假定 Azure 应用程序服务的一个基本的了解，知道如何创建 API 的应用程序，并缝合流。


## <a name="steps-to-achieve-the-business-scenario"></a>实现业务方案的步骤
**创建并配置所需的 API 应用程序**

1. 创建在**Azure 存储 Blob 连接器**实例。 这就需要到 Azure 存储帐户的凭据。 确保它是准备就绪，您才能创建此。
2. 创建的**BizTalk 贸易合作伙伴管理**实例。 这就要求对函数空 SQL 数据库。 请确保它是准备就绪，您才能创建此。
3. 创建**AS2 连接器**实例。 这也要求对函数空 SQL 数据库。 请确保它是准备就绪，您才能创建此。 此外，如果您想要作为组成部分 AS2 处理存档的邮件，您可以提供凭据到 Azure 斑点在创建过程。
4. 配置创建 TPM （贸易合作伙伴管理） 服务︰  
    1. 浏览到 TPM 服务作为上述步骤的一部分创建的实例。
    2. 使用下*组件***添加**一个名为**Contoso**的新伙伴，并在其配置文件中的**合作伙伴**选项添加所需的 AS2 标识。
    3. 使用下*组件***添加**一个名为**罗斯文**的新伙伴，并在其配置文件中的**合作伙伴**选项添加所需的 AS2 标识。
    4. 使用**协议**选项在*组件*下**添加**新 AS2 罗斯和 Contoso 之间的协议。 罗斯将托管的合作伙伴在这里，，Contoso 将来宾合作伙伴。 根据此协议创建过程配置签名、 加密、 压缩和确认。 如果需要使用证书，他们可以浏览创建 TPM 服务时上载通过**证书**选项。


## <a name="create-a-flow--business-process"></a>创建流 / 业务流程
1. 创建新的流中的第一步是 AS2。 拖放**AS2 连接器**，选择已创建的实例。 作为功能选择触发器︰  
    ![][1]  
2. 下一步拖放**Azure 存储 Blob 连接器**，选择已创建的实例。 选择功能，并在该操作中，为所需的功能选择**上载 Blob** 。 根据相应的配置。
3. 现在创建/部署流程。


## <a name="message-processing--troubleshooting"></a>消息处理和故障诊断
1. 它是我们已部署流出测试的时间。 由您创建的 AS2Connector 实例的 AS2 终结点发送 XML 消息中包装的 AS2 （根据上述步骤中创建的 AS2 协议）。 您可能需要配置终结点的身份验证，这样就可公开访问。
2. 通过浏览到流，然后逐步得到执行的流实例可以找出执行有关流的信息
3. AS2 处理信息，浏览到所涉及的 AS2Connector 实例，然后执行单步跟踪部分执行。 您可以使用筛选器来限制该视图所需的信息，涉及到。

![][2]

<!--Image references-->
[1]: ./media/app-service-logic-create-a-b2b-process/Flow.png
[2]: ./media/app-service-logic-create-a-b2b-process/Tracking.png
 
