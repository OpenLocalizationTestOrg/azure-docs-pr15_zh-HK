<properties
    pageTitle="管理和开发任务在 BizTalk 服务列表 |Microsoft Azure"
    description="规划和作业帮助部署 Azure BizTalk 服务。"
    services="biztalk-services"
    documentationCenter=""
    authors="msftman"
    manager="erikre"
    editor=""/>

<tags
    ms.service="biztalk-services"
    ms.workload="integration"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/15/2016"
    ms.author="deonhe"/>

# <a name="administration-and-development-task-list-in-biztalk-services"></a>管理和开发任务列表在 BizTalk 服务  

## <a name="getting-started"></a>入门教程
在使用 Microsoft Azure BizTalk 服务，有几种内部和基于云的组件需要考虑。 若要开始，请考虑以下流程︰  

|步骤|谁负责|任务|相关的链接|
|----|----|----|----|
|1。|管理员|创建 Microsoft Azure 订阅使用 Microsoft 帐户或组织帐户|[Azure 的传统门户网站](http://go.microsoft.com/fwlink/p/?LinkID=213885)|
|2。|管理员|创建或配置 BizTalk 服务。|[创建使用 Azure 的传统门户的 BizTalk 服务](http://go.microsoft.com/fwlink/p/?LinkID=302280)|
|3。|管理员|注册您或您的公司的 BizTalk 服务部署|[注册和更新 BizTalk 服务门户上的 BizTalk 服务部署](https://msdn.microsoft.com/library/azure/hh689837.aspx)|
|4。|管理员|适用于应用程序使用 BizTalk 适配器服务连接到内部的业务线 (LOB) 系统，或者使用队列或主题目标。  创建 Azure 服务总线 Namespace。 为此命名空间、 服务总线颁发者名称和服务总线颁发者的密钥值提供给开发人员。|[如何︰ 创建或修改服务总线服务 Namespace](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md)并[获得颁发者名称和颁发者的密钥值](biztalk-issuer-name-issuer-key.md)|
|5。|开发人员|安装 SDK 并在 Visual Studio 中创建的 BizTalk 服务项目。|[安装 Azure BizTalk 服务 SDK](https://msdn.microsoft.com/library/azure/hh689760.aspx)并[创建在 Azure 上丰富消息的终结点](https://msdn.microsoft.com/library/azure/hh689766.aspx)|
|6。|开发人员|部署到 BizTalk 服务项目位于 Azure BizTalk 服务。|[部署和刷新 BizTalk 服务项目](https://msdn.microsoft.com/library/azure/hh689881.aspx)|
|7。|管理员|如果您正在使用 EDI，应用。  您可以添加合作伙伴和 Microsoft Azure BizTalk 服务门户上创建协议。 当您创建了协议时，您可以添加的桥接器和/或协议设置为开发人员创建的转换。|[配置 EDI、 AS2 和 EDIFACT BizTalk 服务门户上](https://msdn.microsoft.com/library/azure/hh689853.aspx)|
|8。|管理员|使用 Azure 的经典门户，监视 BizTalk 服务，包括性能指标的健康状况。|[BizTalk 服务︰ 仪表板、 显示器和缩放比例选项卡](http://go.microsoft.com/fwlink/p/?LinkID=302281)|
|9。|管理员|使用 Microsoft Azure BizTalk 服务门户，管理在桥的文件处理使用 BizTalk 服务和跟踪邮件的项目。|[使用 BizTalk 服务门户](https://msdn.microsoft.com/library/azure/dn874043.aspx)|
|10。|管理员|创建备份计划备份 BizTalk 服务。|[业务连续性和灾难恢复在 BizTalk 服务](https://msdn.microsoft.com/library/azure/dn509557.aspx) |  
## <a name="next-steps"></a>下一步行动
[教程和示例](https://msdn.microsoft.com/library/azure/hh689895.aspx)

[在 Visual Studio 中创建项目](https://msdn.microsoft.com/library/azure/hh689811.aspx)

[安装 Azure BizTalk 服务 SDK](https://msdn.microsoft.com/library/azure/hh689760.aspx)

## <a name="concepts"></a>概念
[在 Visual Studio 中创建项目](https://msdn.microsoft.com/library/azure/hh689811.aspx)  
[EDI、 AS2，以及 EDIFACT 消息 （企业对企业）](https://msdn.microsoft.com/library/azure/hh689898.aspx)  
## <a name="other-resources"></a>其他资源  
[添加源、 目标和桥接器消息的终结点](https://msdn.microsoft.com/library/azure/hh689877.aspx)  
[了解并创建消息映射和转换](https://msdn.microsoft.com/library/azure/hh689905.aspx)  
[使用 BizTalk 适配器服务 (BAS)](https://msdn.microsoft.com/library/azure/hh689889.aspx)  
[Azure BizTalk 服务](http://go.microsoft.com/fwlink/p/?LinkID=303664)
