<properties
    pageTitle="Web 应用程序概述 |Microsoft Azure"
    description="了解如何 Azure 应用程序服务可以帮助您开发和承载 web 应用程序"
    services="app-service\web"
    documentationCenter=""
    authors="cephalin"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/28/2016"
    ms.author="cephalin"/>

# <a name="web-apps-overview"></a>Web 应用程序概述

*应用程序服务 Web 应用程序*是一个完全托管的计算平台，用于承载的网站和 web 应用程序进行了优化。 提供的 Microsoft Azure 用于此[平台作为服务](https://en.wikipedia.org/wiki/Platform_as_a_service)(PaaS) 您关注您的业务逻辑虽然 Azure 负责运行和扩展应用程序的基础结构。

下面的 5 分钟视频介绍 Azure 应用程序服务 Web 应用程序。

[AZURE.VIDEO azure-app-service-web-apps-with-yochay-kiriaty]

>[AZURE.INCLUDE [app-service-linux](../../includes/app-service-linux.md)]

## <a name="what-is-a-web-app-in-app-service"></a>在应用程序服务 web 应用程序是什么？

在应用程序服务*web 应用程序*是 Azure 提供用于承载网站或 web 应用程序的计算资源。  

共享或专用根据您选择的定价层的虚拟机 (Vm) 上可能的计算资源。 在独立于其他客户托管虚拟机中运行应用程序代码。

您的代码可以在任何语言或框架支持的[Azure 应用程序服务](../app-service/app-service-value-prop-what-is.md)，如 ASP.NET，Node.js、 Java、 PHP 或 Python。 您还可以运行脚本的 web 应用程序中使用[PowerShell 和其他脚本语言](web-sites-create-web-jobs.md#acceptablefiles)。

有关可以使用的 Web 应用程序的典型应用程序方案的示例，请参见[Web 应用程序方案](https://azure.microsoft.com/documentation/scenarios/web-app/)和[Azure 应用程序服务、 虚拟机、 服务结构和云服务比较](choose-web-site-cloud-service-vm.md#scenarios)的**方案和建议**一节。

## <a name="why-use-web-apps"></a>为什么要使用 Web 应用程序？

以下是一些关键的应用程序服务功能适用于 Web 应用程序︰

- **多种语言和框架**的应用程序服务具有对 ASP.NET，Node.js、 Java、 PHP，并且 Python 的一流支持。 您可以在应用程序服务的虚拟机上运行[PowerShell 和其他脚本或可执行文件](../app-service-web/web-sites-create-web-jobs.md)。

- **DevOps 优化**-设置[持续集成和部署](../app-service-web/app-service-continuous-deployment.md)使用 Visual Studio 的团队服务，GitHub 或 BitBucket。 升级更新通过[测试环境和临时环境](../app-service-web/web-sites-staged-publishing.md)。 执行[A / B 测试](../app-service-web/app-service-web-test-in-production-get-start.md)。 通过使用[Azure PowerShell](../powershell-install-configure.md)或[跨平台命令行界面 (CLI)](../xplat-cli-install.md)来管理您的应用程序服务中的应用程序。

- **使用高可用性的全球规模**的扩展[了](../app-service-web/web-sites-scale.md)或[出](../monitoring-and-diagnostics/insights-how-to-scale.md)手动或自动。 承载任意位置在 Microsoft 的全球数据中心基础结构，您的应用程序和应用程序服务[SLA](https://azure.microsoft.com/support/legal/sla/app-service/)保证的高可用性。

- **对 SaaS 平台和内部数据的连接**-选择从企业系统 （例如 SAP、 注册和 Oracle） 的 50 多个[连接器](../connectors/apis-list.md)，SaaS 服务 （如销售队伍和 Office 365） 和互联网服务 （如 Facebook 和 Twitter）。 使用[混合连接](../biztalk-services/integration-hybrid-connection-overview.md)和[Azure 虚拟网络](../app-service-web/web-sites-integrate-with-vnet.md)访问内部数据。

- **安全性和法规遵从性**-应用程序服务是[ISO、 SOC 和 PCI 兼容](https://www.microsoft.com/TrustCenter/)。

- **应用程序模板**-从[Azure 市场](https://azure.microsoft.com/marketplace/)，使您可以使用向导来安装如 WordPress，Joomla，Drupal 流行的开放源码软件应用程序模板的扩展列表中进行选择。

- **Visual Studio 集成**-在 Visual Studio 中的专用工具来优化创建、 部署和调试的工作。

另外，web 应用程序可以利用的功能 （如支持 CORS） 提供[API 的应用程序](../app-service-api/app-service-api-apps-why-best-platform.md)和[移动应用程序](../app-service-mobile/app-service-mobile-value-prop.md)（如推式通知）。 有关在应用程序服务的应用程序类型的详细信息，请参阅[Azure 应用程序服务概述](../app-service/app-service-value-prop-what-is.md)。

Web 应用程序的应用程序服务中，除了 Azure 提供能够用于存放网站和 web 应用程序的其他服务。 大多数情况下，Web 应用程序是最佳的选择。  Microservice 体系结构中，考虑[服务结构](https://azure.microsoft.com/documentation/services/service-fabric)，以及如果您需要更好地控制虚拟机上运行您的代码，请考虑[Azure 的虚拟机](https://azure.microsoft.com/documentation/services/virtual-machines/)。 有关如何进行这些 Azure 服务之间进行选择的详细信息，请参阅[Azure 应用程序服务、 虚拟机、 服务结构和云服务比较](choose-web-site-cloud-service-vm.md)。

## <a name="getting-started"></a>入门教程

首先通过将示例代码部署到新 web 应用程序在应用程序服务，请按照[部署到 Azure 中 5 分钟第一个 web 应用程序](app-service-web-get-started.md)的教程。 您将需要免费的 Azure 帐户。

如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)，立即可以在此创建短期的初学者 web 应用程序在应用程序服务。 没有信用卡，所需;没有承诺。
