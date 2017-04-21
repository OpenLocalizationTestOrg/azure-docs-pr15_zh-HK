<properties
    pageTitle="任选流、 逻辑应用程序、 功能和 WebJobs |Microsoft Azure"
    description="比较和对比云集成来自 Microsoft 服务，并决定应使用哪个服务。"
    services="functions,app-service\logic"
    documentationCenter="na"
    authors="cephalin"
    manager="wpickett"
    tags=""
    keywords="microsoft 流、 流、 应用程序逻辑、 azure 函数，函数，azure webjobs，webjobs，事件处理，动态计算、 无服务器体系结构"/>

<tags
    ms.service="functions"
    ms.devlang="multiple"
    ms.topic="article"
    ms.tgt_pltfrm="multiple"
    ms.workload="na"
    ms.date="09/08/2016"
    ms.author="chrande; glenga"/>

# <a name="choose-between-flow-logic-apps-functions-and-webjobs"></a>流、 逻辑应用程序、 功能和 WebJobs 之间进行选择

本文比较和对比中 Microsoft 云，可以全部解决集成问题和业务流程自动化的以下服务︰

- [Microsoft 流](https://flow.microsoft.com/)
- [Azure 的逻辑应用程序](https://azure.microsoft.com/services/logic-apps/)
- [Azure 的函数](https://azure.microsoft.com/services/functions/)
- [Azure 应用程序服务 WebJobs](../app-service-web/web-sites-create-web-jobs.md)

当"粘附"在一起完全不同的系统，所有这些服务非常有用。 它们可以全部定义输入、 操作、 条件和输出。 您可以运行每个定期或触发器。 但是，每个服务添加一组唯一的值，并比较它们不是问题"的服务是最好的吗？" 但之一"的服务最适合这种情况发生吗？" 通常，这些服务的组合是快速构建可缩放的、 完整的特色的集成解决方案的最佳途径。

<a name="flow"></a>
## <a name="flow-vs-logic-apps"></a>流与逻辑应用程序

我们可以讨论 Microsoft 流和 Azure 逻辑应用程序组合在一起因为它们便于构建流程和工作流与各种 SaaS 和企业应用程序集成这两个*配置第一个*集成服务。 

- 流是基于逻辑的应用程序
- 它们具有相同的工作流设计器
- 工作时还可以在其他的[连接器](../connectors/apis-list.md)

流使任何办公室工作人员来执行简单的集成 （如获得 SMS 重要电子邮件） 而不需要通过开发人员或 IT。 另一方面，逻辑应用程序可以启用高级或关键任务的集成 （如 B2B 进程） 企业级 DevOps 和安全做法所需的位置。 它是典型的业务工作流，以便发展复杂性加班。 相应地，可以首先，流的开头，然后将其转换为逻辑应用程序中，根据需要。

下表可帮助您确定是否最适合于给定的集成流或逻辑的应用程序。

|               | 流量                                                                             | 逻辑应用程序                                                                                          |
|---------------|----------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------|
| 访问群体      | 办事处工作人员，业务用户                                                   | IT 专业人员，开发人员                                                                                 |
| 方案     | 自助服务                                                                     | 关键任务                                                                                    |
| 设计工具   | 在浏览器，用户界面只                                                              | 在浏览器和[Visual Studio](../app-service/logic/app-service-logic-deploy-from-vs.md)时，[代码视图](../app-service-logic/app-service-logic-author-definitions.md)可用 |
| DevOps        | 点对点，制定在生产环境中                                                    | 源代码管理、 测试、 支持自动化和可管理性在[Azure 资源管理](../app-service-logic/app-service-logic-arm-provision.md)|
| 管理体验| [https://flow.microsoft.com](https://flow.microsoft.com)                       | [https://portal.azure.com](https://portal.azure.com)                                                |
| 安全      | 标准做法︰[数据 sovereignty](https://wikipedia.org/wiki/Technological_Sovereignty)、[存放时加密](https://wikipedia.org/wiki/Data_at_rest#Encryption)敏感数据，等等。 | 安全保证的 Azure: [Azure 安全性](https://www.microsoft.com/trustcenter/Security/AzureSecurity)、[安全中心](https://azure.microsoft.com/services/security-center/)、[审核日志](https://azure.microsoft.com/blog/azure-audit-logs-ux-refresh/)，和更多。 |

<a name="function"></a>
## <a name="functions-vs-webjobs"></a>与 WebJobs 函数

我们可以讨论 Azure 函数和 Azure 应用程序服务 WebJobs 在一起因为它们是两种*代码第一个*集成服务，为开发人员设计的。 它们使您能够运行一个脚本或一段代码以响应各种事件，如[新存储 Blob](functions-bindings-storage.md)或[请求 WebHook](functions-bindings-http-webhook.md)。 这里有其相似之处︰ 

- 同时基于[Azure 应用程序服务](../app-service/app-service-value-prop-what-is.md)，并享受[源代码管理](../app-service-web/app-service-continuous-deployment.md)、[身份验证](../app-service/app-service-authentication-overview.md)和[监视](../app-service-web/web-sites-monitor.md)等功能。
- 两者都是专注于开发人员的服务。
- 同时支持标准的脚本和编程语言。
- 它们都具有 NuGet 和 NPM 支持。

函数是 WebJobs 的自然发展，因为它采用最好 WebJobs 的特点，改进了它们。 这些改进包括︰ 

- 简化的开发，测试和代码，直接在浏览器中运行。
- 内置集成多 Azure 服务和第三方服务，如[GitHub WebHooks](https://developer.github.com/webhooks/creating/)。
- 按使用付费，无需支付[计划的应用程序服务](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)。
- 自动、[动态缩放](functions-scale.md)。
- 为应用程序服务计划仍有可能 （以利用未充分利用的资源） 运行的应用程序服务的现有客户。
- 与逻辑应用程序集成。

下表总结了函数和 WebJobs 之间的差异︰

|                        | 函数                                                                                                                                                                | WebJobs                            |
|------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------|
| 缩放比例                | Configurationless 的缩放比例                                                                                                                                                | 随应用程序服务计划        |
| 定价                | 每个使用付费或应用程序服务计划的一部分                                                                                                                                  | 应用程序服务计划的一部分           |
| 运行类型               | 触发、 安排 （通过计时器触发）                                                                                                                                  | 触发的、 连续的计划   |
| 触发事件         | [计时器](functions-bindings-timer.md)， [Azure DocumentDB](functions-bindings-documentdb.md)， [Azure 事件集线器](functions-bindings-event-hubs)、 [HTTP/WebHook （GitHub 的可宽延时间）](functions-bindings-http-webhook.md)、 [Azure 应用程序服务移动应用程序](functions-bindings-mobile-apps.md)， [Azure 通知集线器](functions-bindings-notification-hubs.md)， [Azure 服务总线](functions-bindings-service-bus.md)， [Azure 存储](articles/functions-bindings-storage.md) | [Azure 存储](websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)， [Azure 服务总线](websites-dotnet-webjobs-sdk-service-bus.md)         |
| 在浏览器开发 | x                                                                                                                                                                        |                                    |
| Windows 脚本       | 实验                                                                                                                                                             | x                                  |
| PowerShell             | 实验                                                                                                                                                             | x                                  |
| C#                     | x                                                                                                                                                                        | x                                  |
| F#                     | x                                                                                                                                                                        |                                    |
| 大扫除                   | 实验                                                                                                                                                             | x                                  |
| PHP                    | 实验                                                                                                                                                             | x                                  |
| Python                 | 实验                                                                                                                                                             | x                                  |
| JavaScript             | x                                                                                                                                                                        | x                                  |
| Java                   | 实验                                                                                                                                                             | x                                  |

使用函数 WebJobs 还是最终取决于您所已经做与应用程序服务。 如果您有您要运行的代码段的应用程序服务应用程序并且想要在同一 DevOps 环境共同管理，您应该使用 WebJobs。 如果您想要运行的其他 Azure 服务或甚至第三方应用程序，代码段，或如果您要管理您的集成代码片断分别从您的应用程序服务的应用程序，或者您想要从逻辑应用程序调用您的代码片断，您应利用所有改进中的函数。  

<a name="together"></a>
## <a name="flow-logic-apps-and-functions-together"></a>形成，逻辑的应用程序和功能

先前提到过，是最适合于您的哪些服务取决于您的具体情况。 

- 对于简单的业务优化，然后使用流。
- 如果集成方案太高级的流程，或者您需要 DevOps 的功能和安全合法规，然后使用应用程序逻辑。
- 如果一个步骤中集成方案需要高度自定义的转换或专门的代码，然后编写函数的应用程序，然后触发某一操作的逻辑应用程序中的函数。

您可以调用逻辑应用程序流中。 此外可以在函数中调用逻辑的应用程序，并逻辑应用程序中的函数。 继续提高加班流、 逻辑的应用程序和功能之间的集成。 您可以构建一个服务中的某些内容，并在其他服务中使用它。 因此，在这三种技术中所做的任何投资是值得的。

## <a name="next-steps"></a>下一步行动

首先与每个服务创建第一个流、 逻辑应用程序、 功能的应用程序或 WebJob。 单击以下链接之一︰

- [开始使用 Microsoft 流](https://flow.microsoft.com/en-us/documentation/getting-started/)
- [创建一个逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)
- [创建您的第一个 Azure 函数](../azure-functions/functions-create-first-azure-function.md)
- [部署使用 Visual Studio 的 WebJobs](../app-service-web/websites-dotnet-deploy-webjobs.md)

或者，使用下面的链接这些集成服务的详细信息︰

- [利用集成方案通过 Christopher Anderson Azure 函数和 Azure 应用程序服务](http://www.biztalk360.com/integrate-2016-resources/leveraging-azure-functions-azure-app-service-integration-scenarios/)
- [Charles Lamanna 所做简单的集成](http://www.biztalk360.com/integrate-2016-resources/integrations-made-simple/)
- [逻辑应用程序实时网络广播](http://aka.ms/logicappslive)
- [Microsoft 流动频繁︰ 常见问题](https://flow.microsoft.com/documentation/frequently-asked-questions/)
- [Azure 的 WebJobs 文档资源](../app-service-web/websites-webjobs-resources.md)
