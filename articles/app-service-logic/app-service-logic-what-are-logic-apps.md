<properties 
    pageTitle="逻辑应用程序有哪些？" 
    description="了解有关应用程序服务的逻辑应用程序详细信息" 
    authors="kevinlam1" 
    manager="dwrede" 
    editor="" 
    services="logic-apps" 
    documentationCenter=""/>

<tags
    ms.service="logic-apps"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="hero-article" 
    ms.date="10/12/2016"
    ms.author="klam"/>

# <a name="what-are-logic-apps"></a>逻辑应用程序有哪些？

逻辑应用程序提供一种方法来简化并在云中实现可扩展的集成和工作流。 它提供了可视化设计器进行建模和自动化为一系列步骤称为工作流的过程。  在云和内部可迅速集成服务和协议之间有[许多连接线](../connectors/apis-list.md)。  逻辑应用程序开头的触发器 （如帐户添加到 Dynamics CRM 时） 前后许多组合操作、 转换和条件逻辑就可以开始触发。

使用应用程序逻辑的优点包括︰  

- 通过使用易于理解设计工具的复杂流程设计节省时间
- 无缝地实现模式和工作流，，否则将难以在代码中实现
- 从模板快速入门教程
- 自定义您的逻辑应用程序与您自己的自定义的 Api、 代码和操作
- 连接和同步不同系统跨内部和云
- 从 BizTalk 服务器、 API 管理，Azure 函数和 Azure 服务总线构建一流的集成支持

逻辑应用程序是完全托管的 iPaaS （集成平台作为服务） 允许开发人员不需要担心建立承载、 可扩展性、 可用性和管理。  逻辑应用程序会自动扩大以满足需求。

![流的应用程序设计器](./media/app-service-logic-what-are-logic-apps/LogicAppCapture2.png)

如上所述，与逻辑应用程序，可以自动执行业务流程。 下面是一些示例︰  
 
* 移动到 Azure 存储上载到 FTP 服务器的文件
* 流程和工艺路线单跨内部和云系统
* 监视有关特定主题的所有 tweets，分析主观意图，并创建警报和任务的项目需要继续努力。

所有从可视化设计器，而无需编写一行代码，可将配置类似的情形。 获取[生成的逻辑应用程序现在]开始[create]。  一旦编写的逻辑应用程序可以跨多个环境和区域是[快速部署和重新配置](app-service-logic-create-deploy-template.md)。

## <a name="why-logic-apps"></a>为什么逻辑应用程序？

逻辑应用程序调入企业集成空间的速度和可伸缩性。  易于使用的设计器，各种可用的触发器和操作，以及强大的管理工具进行集中您比以往任何时候都更简单的 Api。  随着企业迈进数字化，逻辑应用程序允许您将旧和先进的系统连接在一起。

此外，通过我们的[企业集成帐户][biztalk]成熟的[XML 消息处理]具有能力的集成方案，您可以扩展[xml]，[贸易伙伴管理][tpm]，等等。

- **易于使用的设计工具**的应用程序逻辑可以是设计的端到端的浏览器中或使用 Visual Studio 工具。 GitHub 问题创建时启动触发器-从简单的日程安排到。 然后可以协调任意数量的操作使用连接器格式库。

- **连接 Api 方便地**-易于描述的甚至撰写任务很难在代码中实现。 逻辑应用程序容易连接不同的系统。 要连接到您在部署解决方案市场营销您云帐单系统？ 想要集中在 Api 和使用企业服务总线的系统消息吗？ 逻辑应用程序是速度最快、 最可靠的方法为这些问题提供解决方案。

- **快速从模板开始**-来帮助您入门，我们提供了一个[模板的库][ templates] ，使您可以快速创建一些常见的解决方案。 从简单的 SaaS 连接到高级 B2B 解决方案和几所生活的-库是入门的逻辑应用程序具有能力的最快方法。

- **在扩展内置-** -看不到所需的接口？ 逻辑应用程序设计为使用您自己的 Api 和代码;您可以轻松地创建自己的 API 应用程序使用与自定义连接线，或调入[Azure 函数](https://functions.azure.com)执行的请求的代码段。 

- **真正的集成马力**-开始轻松并随需要而增长。 逻辑应用程序可以方便地利用 BizTalk，Microsoft 的业界领先的集成解决方案启用集成专业人员，以生成所需的解决方案的能力。 了解更多有关[企业集成包](./app-service-logic-enterprise-integration-overview.md)。

## <a name="logic-app-concepts"></a>逻辑应用程序概念

下面是一些构成的逻辑应用程序体验的主要部分。 

- **工作流**的逻辑应用程序提供了对业务流程建模为一系列的步骤或工作流的图形方式。
- **管理接口**的逻辑应用程序需要访问的数据和服务。 托管的连接器是专门用于帮助您将连接到并使用您的数据时。 现[管理连接器]的连接器列表，请参阅[managedapis]。
- **触发器**的某些管理连接器还可以充当一个触发器。 触发器启动工作流基于特定的事件，如电子邮件或在 Azure 存储帐户的更改何时到达的一个新实例。
-  **操作**在工作流中的触发器调用某个操作后的每一步。 通常，每个操作将映射到托管的接口或自定义的 API 应用程序上的操作。
- **企业集成包**-对于更高级的集成方案，逻辑应用程序包括从 BizTalk 功能。 BizTalk 是 Microsoft 的业界领先的集成平台。 企业集成包连接器允许您方便地包括验证、 转换和多到您的应用程序逻辑的工作流。

## <a name="getting-started"></a>入门教程  

- 要开始使用逻辑的应用程序，请按照[创建逻辑应用程序][create]教程。  
- [查看常见示例和应用场景](app-service-logic-examples-and-scenarios.md)
- [可以自动执行业务流程逻辑的应用程序](http://channel9.msdn.com/Events/Build/2016/T694) 
- [了解如何将您的系统集成与应用程序逻辑](http://channel9.msdn.com/Events/Build/2016/P462)

[biztalk]: app-service-logic-enterprise-integration-accounts.md
[appservice]: ../app-service/app-service-value-prop-what-is.md
[create]: app-service-logic-create-a-logic-app.md
[managedapis]: ../connectors/apis-list.md
[tpm]: app-service-logic-enterprise-integration-accounts.md
[xml]: app-service-logic-enterprise-integration-b2b.md
[templates]: app-service-logic-use-logic-app-templates.md
