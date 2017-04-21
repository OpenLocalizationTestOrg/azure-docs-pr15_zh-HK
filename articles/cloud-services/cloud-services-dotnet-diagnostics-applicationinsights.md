<properties
   pageTitle="解决云服务使用应用程序的见解 |Microsoft Azure"
   description="了解如何使用流程数据从 Azure 诊断应用程序深入解决云服务的问题。"
   services="cloud-services"
   documentationCenter=".net"
   authors="sbtron"
   manager="timlt"
   editor="tysonn" />
<tags
   ms.service="cloud-services"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="12/15/2015"
   ms.author="saurabh" />


# <a name="troubleshoot-cloud-services-using-application-insights"></a>使用应用程序的见解的云服务的疑难解答

[Azure SDK 2.8](https://azure.microsoft.com/downloads/)和 Azure 诊断程序扩展名 1.5 可以立即发送 Azure 诊断数据的云服务直接向应用程序的见解。 各种类型的日志收集 Azure 诊断包括 windows 事件日志的应用程序日志的 ETW 日志和性能计数器可以立即发送到应用程序的见解和可视化应用程序的见解门户用户界面中。 当使用应用程序的见解 SDK 以及现在可将深入了解的指标和来自您的应用程序，以及来自 Azure 诊断系统和基础结构级别数据的日志。

## <a name="configure-azure-diagnostics-to-send-data-to-application-insights"></a>配置 Azure 诊断程序将数据发送到应用程序的见解

请按照这些步骤来安装您的云服务项目将 Azure 诊断数据发送到应用程序的见解。

1) Visual Studio 解决方案资源管理器角色上右击并选择**属性**以打开在角色设计器

![解决方案资源管理器角色属性][1]

2) 在角色设计器中的诊断程序部分中，选择复选框可以**诊断程序将数据发送到应用程序的见解**

![角色设计器将诊断数据发送到应用程序的见解][2]

3) 在弹出对话框中选择您想要发送到 Azure 诊断应用程序理解资源。 该对话框允许您从您的订阅中选择一个现有的应用程序理解资源或手动指定应用程序的见解资源检测键。 如果您没有现有的应用程序理解资源然后您可以创建在通过单击**创建新的资源**链接，这将会打开到 Azure 的传统门户网站一个浏览器窗口，您可以在其中创建应用程序的见解资源。 有关创建应用程序的见解资源的更多信息，请参见[创建新的应用程序理解资源](../application-insights/app-insights-create-new-resource.md)

![选择应用程序的见解资源][3]

4) 一旦您添加了应用程序的见解资源，该资源的检测键被视为名为**APPINSIGHTS_INSTRUMENTATIONKEY**的服务配置设置。 您可以通过从服务配置下拉选择不同的配置更改此配置设置为每个服务配置或环境下，并指定该配置的新检测密钥。

![选择服务配置][4]

Visual Studio 使用**APPINSIGHTS_INSTRUMENTATIONKEY**配置设置配置诊断扩展期间发布的相应应用程序的见解资源信息。 配置设置是一种简便的方法来定义不同的服务配置不同的检测项。 Visual Studio 将翻译该设置并将其插入到诊断扩展公共配置发布时。 若要简化配置使用 PowerShell 诊断程序扩展的过程，从 Visual Studio 还输出包中包含公用配置 XML 与适当的应用程序理解工具包含密钥。 公用配置文件扩展文件夹中创建并按照 PaaSDiagnostics 的模式。<RoleName>.PubConfig.xml。 任何基于 PowerShell 部署可以使用此模式映射到某个角色的每一种配置。

5) 启用**诊断程序将数据发送到应用程序的见解**将自动配置发送所有性能计数器和错误级别记录到应用程序的见解 Azure 诊断代理收集的 Azure 诊断。 如果您想要进一步配置哪些数据被发送到应用程序的见解则需要手动编辑*diagnostics.wadcfgx*文件中的为每个角色。 [配置 Azure 诊断，以将数据发送到应用程序的见解](../azure-diagnostics-configure-applicationinsights.md)，以了解有关手动更新配置的详细信息，请参阅。

一旦云服务被配置为将 Azure 诊断数据发送到应用程序可以部署到 Azure 像往常一样的见解并确保 Azure 诊断程序扩展被启用。 请参阅[发布使用 Visual Studio 的云服务](../vs-azure-tools-publishing-a-cloud-service.md)。  

## <a name="viewing-azure-diagnostics-data-in-application-insights"></a>查看应用程序的见解中的 Azure 诊断数据
诊断的 Azure 遥测数据将出现在为云服务配置的应用程序理解资源。

下面是如何不同的 Azure 诊断记录类型映射到应用程序理解的概念︰  

-  性能计数器将显示为自定义度量值在应用程序的见解
-  Windows 事件日志将显示为跟踪和应用程序的见解中的自定义事件
-  作为应用程序的见解中跟踪显示了应用程序日志、 ETW 日志和诊断基础结构中的任何日志。

若要查看应用程序的见解 Azure 诊断数据︰

- 使用[标准浏览器](../application-insights/app-insights-metrics-explorer.md)可视化的任何自定义性能计数器或不同类型的 windows 事件日志事件的计数。

![测量数据资源管理器中的自定义标准][5]

- 使用[搜索](../application-insights/app-insights-diagnostic-search.md)来搜索跨各种跟踪日志发送 Azure 诊断程序。 对于示例如果角色导致崩溃，并回收该信息的角色中有未处理的异常将显示在*Windows 事件日志*的*应用程序*通道。 可以使用搜索功能查看 Windows 事件日志错误，使您能够找到问题的根本原因的异常获取完整的堆栈跟踪。

![搜索跟踪][6]

## <a name="next-steps"></a>下一步行动

- [添加到您的云服务应用程序的见解 SDK](../application-insights/app-insights-cloudservices.md)应用程序通过发送有关请求、 异常、 相关性和任何自定义的遥测数据。 再加上 Azure 诊断数据都在同一个应用程序分析资源可以全面了解您的应用程序和系统。  


<!--Image references-->
[1]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/solution-explorer-properties.png
[2]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/role-designer-sendtoappinsights.png
[3]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/select-appinsights-resource.png
[4]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/role-designer-appinsights-serviceconfig.png
[5]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/metrics-explorer-custom-metrics.png
[6]: ./media/cloud-services-dotnet-diagnostics-applicationinsights/search-windowseventlog-error.png
