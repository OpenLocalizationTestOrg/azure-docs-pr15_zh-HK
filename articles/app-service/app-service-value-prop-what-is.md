<properties
    pageTitle="Azure 应用程序服务 web、 移动设备、 和 API 的应用程序 |Microsoft Azure"
    description="了解如何 Azure 应用程序服务可以帮助您开发、 部署和管理 web 和移动应用程序。"
    keywords="应用程序服务、 azure 应用程序服务、 应用程序服务成本、 缩放、 可扩展、 应用程序部署、 azure 应用程序部署、 paas、 平台作为服务、 网站、 web 站点、 web、 移动 azure"
    services="app-service"
    documentationCenter=""
    authors="omarkmsft"
    manager="erikre"
    editor="cephalin"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/26/2016"
    ms.author="omark"/>

# <a name="what-is-azure-app-service"></a>Azure 应用程序服务是什么？

*应用程序服务*是[平台作为服务](https://en.wikipedia.org/wiki/Platform_as_a_service)(PaaS) 提供的 Microsoft Azure。 创建 web 和移动应用程序的任何平台或设备。 与 SaaS 解决方案集成在您的应用程序、 连接到内部部署的应用程序和自动化业务流程。 Azure 使用您选择的共享虚拟机资源或专用的虚拟机完全托管的虚拟机 (Vm) 上运行您的应用程序。

应用程序服务包括 web 和移动我们以前提供了分别为 Azure 网站和 Azure 移动服务的功能。 它还包括新的业务流程自动化和承载云 Api 功能。 作为一个单一的集成服务，应用程序服务可以撰写各种组件 — 网站、 移动应用程序的后端，rest 风格的 Api，并且业务流程会-到一个单一的解决方案。

下面的 4 分钟视频提供应用程序服务与早期的 Azure 服务的方式和最新信息中的简短解释。

+[AZURE.VIDEO app-service-history-lesson]

## <a name="why-use-app-service"></a>为什么要使用应用程序服务？

下面是一些主要功能和应用程序服务功能︰

- **多种语言和框架**的应用程序服务具有对 ASP.NET，Node.js、 Java、 PHP，并且 Python 的一流支持。 您可以在应用程序服务的虚拟机上运行[Windows PowerShell 和其他脚本或可执行文件](../app-service-web/web-sites-create-web-jobs.md)。

- **DevOps 优化**-设置[持续集成和部署](../app-service-web/app-service-continuous-deployment.md)使用 Visual Studio 的团队服务，GitHub 或 BitBucket。 升级更新通过[测试环境和临时环境](../app-service-web/web-sites-staged-publishing.md)。 执行[A / B 测试](../app-service-web/app-service-web-test-in-production-get-start.md)。 通过使用[Azure PowerShell](../powershell-install-configure.md)或[跨平台命令行界面 (CLI)](../xplat-cli-install.md)来管理您的应用程序服务中的应用程序。

- **使用高可用性的全球规模**的扩展[了](../app-service-web/web-sites-scale.md)或[出](../monitoring-and-diagnostics/insights-how-to-scale.md)手动或自动。 承载任意位置在 Microsoft 的全球数据中心基础结构，您的应用程序和应用程序服务[SLA](https://azure.microsoft.com/support/legal/sla/app-service/)保证的高可用性。

- **对 SaaS 平台和内部数据的连接**-选择从企业系统 （例如 SAP、 注册和 Oracle） 的 50 多个[连接器](../connectors/apis-list.md)，SaaS 服务 （如销售队伍和 Office 365） 和互联网服务 （如 Facebook 和 Twitter）。 使用[混合连接](../biztalk-services/integration-hybrid-connection-overview.md)和[Azure 虚拟网络](../app-service-web/web-sites-integrate-with-vnet.md)访问内部数据。

- **安全性和法规遵从性**-应用程序服务是[ISO、 SOC 和 PCI 兼容](https://www.microsoft.com/TrustCenter/)。

- **应用程序模板**-从[Azure 市场](https://azure.microsoft.com/marketplace/)，使您可以使用向导来安装如 WordPress，Joomla，Drupal 流行的开放源码软件应用程序模板的扩展列表中进行选择。

- **Visual Studio 集成**-在 Visual Studio 中的专用工具来优化创建、 部署和调试的工作。

## <a name="app-types-in-app-service"></a>在应用程序服务的应用程序类型

应用程序服务提供了几种*应用程序类型*，每种用于承载某种特定类型的工作负荷︰

- [**Web 应用程序**](../app-service-web/app-service-web-overview.md)的宿主网站和 web 应用程序。

- [**移动应用程序**](../app-service-mobile/app-service-mobile-value-prop.md)用于承载移动应用程序后结束。

- [**API 的应用程序**](../app-service-api/app-service-api-apps-why-best-platform.md)的承载 rest 风格的 Api。

- [**逻辑应用程序**](../app-service-logic/app-service-logic-what-are-logic-apps.md)的业务流程自动化和跨云集成系统和数据，而无需编写代码。

Word*的应用程序*在此处指专门用于运行工作负载的托管资源。 执行"web 应用程序"为例，你可能习惯于考虑 web 应用程序作为计算资源并在一起传递到浏览器的功能的应用程序代码。 但是，在应用程序服务*web 应用程序*是 Azure 用于承载应用程序代码中提供的计算资源。 如果您的应用程序由 web 前端和 rest 风格的 API 后端，您可以部署到 web 应用程序的两个或无法将前端代码部署到 web 应用程序和后端代码到 API 的应用程序。 您的应用程序可能会组成不同类型的多个应用程序服务应用程序。

## <a name="app-service-plans"></a>应用程序服务计划

[应用程序服务计划](azure-web-sites-web-hosting-plans-in-depth-overview.md)指定计算资源上运行您的应用程序的类型。 如果您希望光流量负载，可以使用共享的虚拟机 （**免费**和**共享**的定价层）。 对较高负载，可以从几种规格的专用虚拟机 （**基本**、**标准**和**高级**层） 中进行选择。 多个应用程序服务应用程序可以共享相同的计划，而且它们扩展向上和向下的定价层一起计划中。

如果您需要更多的可扩展性和网络隔离，您可以在[应用程序服务环境](../app-service-web/app-service-app-service-environment-intro.md)中运行您的应用程序。

## <a name="pricing"></a>定价

有关多少应用程序服务成本信息，请参阅[应用程序服务的定价](https://azure.microsoft.com/pricing/details/app-service/)。

## <a name="test-drive-app-service"></a>测试应用程序服务

[创建示例 web 应用程序、 移动应用程序或应用程序逻辑](http://go.microsoft.com/fwlink/?LinkId=523751)和没有信用卡用时 1 小时，播放与之需要，没有承诺，没有麻烦。

或打开一个[免费的 Azure 帐户](https://azure.microsoft.com/pricing/free-trial/)，并尝试我们快速入门教程︰

* [教程︰ 创建 web 应用程序](../app-service-web/app-service-web-get-started.md)
* [教程︰ 创建一个移动应用程序](../app-service-mobile/app-service-mobile-android-get-started.md)
* [教程︰ 创建 API 的应用程序](../app-service-api/app-service-api-dotnet-get-started.md)
* [教程︰ 创建一个逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)
