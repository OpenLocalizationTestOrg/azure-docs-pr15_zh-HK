<properties 
    pageTitle="企业集成概述 |Microsoft Azure 应用程序服务 |Microsoft Azure" 
    description="使用企业集成的功能可以使用应用程序逻辑的业务流程和集成方案" 
    services="logic-apps" 
    documentationCenter=".net,nodejs,java"
    authors="msftman" 
    manager="erikre" 
    editor="cgronlun"/>

<tags 
    ms.service="logic-apps" 
    ms.workload="integration" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/08/2016" 
    ms.author="deonhe"/>

# <a name="overview-of-the-enterprise-integration-pack"></a>企业集成包的概述

## <a name="what-is-the-enterprise-integration-pack"></a>企业集成包是什么？
企业集成包是 Microsoft 的基于云的解决方案无缝地使企业到企业 (B2B) 通信。 包使用行业标准协议包括[AS2](./app-service-logic-enterprise-integration-as2.md)、 [X12](./app-service-logic-enterprise-integration-x12.md)和[EDIFACT](./app-service-logic-enterprise-integration-edifact.md)商业伙伴之间交换消息。 可以使用加密和数字签名还可以保护消息。 

包允许使用不同协议的组织和格式以电子方式交换信息，通过将不同的格式转换为一种格式，这两家公司的系统可以解释并对其执行操作。 

如果您熟悉 BizTalk Server 或 Microsoft Azure BizTalk 服务，您会发现易于使用企业集成功能，因为类似的大多数概念。 一个主要的区别在于企业集成使用集成帐户来简化的存储和管理 B2B 通信中使用的项目。 

企业集成包结构上讲，基于**集成帐户**存储可用于设计、 部署和维护您的 B2B 应用程序的所有项目。 集成帐户基本上是储存的项目，如架构、 合作伙伴、 证书、 地图，以及协议的基于云的容器。 这些项目可以然后用于在逻辑应用程序中生成 B2B 工作流。 逻辑应用程序中使用这些项目之前，您需要将集成帐户链接到您的应用程序逻辑。 之后将其链接，您的逻辑应用程序将有权集成帐户的项目。  

## <a name="why-should-you-use-enterprise-integration"></a>为什么要使用企业集成？
- 与企业集成，现在可以存储在一个位置，它是集成帐户的所有项目。 
- 您可以利用逻辑应用程序引擎和所有连接器来建立 B2B 工作流和与第三方 SaaS 应用程序、 内部部署的应用程序，以及自定义应用程序集成
- 您还可以利用 Azure 的函数

## <a name="how-to-get-started-with-enterprise-integration"></a>如何开始使用企业集成？
您可以构建和管理 B2B 应用程序使用**Azure 的门户网站**上的逻辑应用程序设计器通过企业集成包。  

此外可以使用[PowerShell](https://msdn.microsoft.com/library/azure/mt652195.aspx "PowerShell 主题的逻辑应用程序")来管理您的逻辑应用程序。 

以下是需要执行之前可以在 Azure 门户中创建应用程序的步骤概述︰![概述图像](./media/app-service-logic-enterprise-integration-overview/overview-0.png)  

## <a name="what-are-some-common-scenarios"></a>一些常见的情况是什么？

企业集成支持这些行业标准︰   

- EDI 的电子数据交换  
- EAI 的企业应用程序集成  

## <a name="heres-what-you-need-to-get-started"></a>以下是您需要开始
- 具有集成帐户 Azure 预订
- Visual Studio 2015 年创建映射和架构
- [Microsoft Azure 逻辑应用企业集成工具 Visual Studio 2015 2.0](https://aka.ms/vsmapsandschemas)  

## <a name="try-it"></a>试一试
[立即尝试](https://github.com/Azure/azure-quickstart-templates/tree/master/201-logic-app-as2-send-receive)部署一个完全可操作的示例 AS2 发送和接收逻辑应用程序使用的应用程序逻辑的 B2B 功能。

## <a name="learn-more-about"></a>了解更多信息︰
- [协议](./app-service-logic-enterprise-integration-agreements.md "了解企业集成协议")
- [企业到企业 (B2B) 方案](./app-service-logic-enterprise-integration-b2b.md "了解如何创建逻辑与 B2B 功能的应用程序")  
- [证书](./app-service-logic-enterprise-integration-certificates.md "了解企业集成证书")
- [编码/解码的平面文件](./app-service-logic-enterprise-integration-flatfile.md "了解如何进行编码和解码平面文件内容")  
- [集成的帐户](./app-service-logic-enterprise-integration-accounts.md "关于集成帐户")
- [地图](./app-service-logic-enterprise-integration-maps.md "了解企业集成映射")
- [合作伙伴](./app-service-logic-enterprise-integration-partners.md "了解企业集成合作伙伴")
- [架构](./app-service-logic-enterprise-integration-schemas.md "了解企业集成架构")
- [XML 消息验证](./app-service-logic-enterprise-integration-xml.md "了解如何验证逻辑应用程序的 XML 消息")
- [XML 转换](./app-service-logic-enterprise-integration-transform.md "了解企业集成映射")
- [企业集成连接器](../connectors/apis-list.md "了解企业集成包连接器")



