<properties
    pageTitle="Azure 应用程序服务、 虚拟机、 服务结构和云服务比较 |Microsoft Azure"
    description="了解如何选择 Azure 应用程序服务、 虚拟机、 服务结构和用于承载 web 应用程序的云服务。"
    services="app-service\web, virtual-machines, cloud-services"
    documentationCenter=""
    authors="tdykstra"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/07/2016"
    ms.author="tdykstra"/>

# <a name="azure-app-service-virtual-machines-service-fabric-and-cloud-services-comparison"></a>Azure 应用程序服务、 虚拟机、 服务结构和云服务的比较

## <a name="overview"></a>概述

Azure 提供承载 web 网站的几种方法︰ [Azure 应用程序服务][]、[虚拟机][]、[服务结构][]和[云服务][]。 这篇文章可以帮助您了解的选项，并选择合适的 web 应用程序。

Azure 应用程序服务是大多数 web 应用程序的最佳选择。 部署和管理集成到其平台、 站点可以快速扩展，以处理高流量负荷和内置负载平衡和通信管理器提供高可用性。 可以移动现有网站到 Azure 应用程序服务轻松地使用[联机迁移工具](https://www.migratetoazure.net/)、 从 Web 应用程序库中，使用一个开源的应用程序或创建新的网站使用您选择的工具和框架。 [WebJobs][]功能，使得可以方便地添加到您的应用程序服务 web 应用程序处理的后台作业。

如果您正在创建新应用程序或重写现有的应用程序使用 microservice 结构，服务结构将是一个不错的选择。 应用程序，共享库的机器运行测试，可以从小和到具有数百个或数千机根据需要大规模增长。 有状态的服务使其易于一致而可靠地存储应用程序状态和服务结构自动管理服务分区、 扩展和可用性对您。  服务结构还支持.NET (OWIN) 和 ASP.NET 核心的 WebAPI 与开放的 Web 接口。  服务结构与应用程序服务，还提供更多的控制或直接访问底层基础架构。 您可以将您的服务器到远程或配置服务器启动任务。 云服务类似于服务结构中度的控制而不是易于使用，但它现在是一个旧式的服务以及服务结构建议对于新的开发。

如果您有现有的应用程序需要实质性的修改，要运行的应用程序服务或服务结构中，您可以选择虚拟机来简化迁移到云。 但是，正确地配置、 保护和维护虚拟机需要更多时间和 IT 专业技能与 Azure 应用程序服务和服务结构。 如果您正在考虑使用 Azure 的虚拟机，请确保您考虑到需修补程序、 更新和管理您的虚拟机环境的日常维护工作。 Azure 的虚拟机是基础结构作为-服务 (IaaS)，而应用程序服务和服务结构平台作为-服务 (Paas)。 

## <a name="features"></a>功能比较

下表比较了应用程序服务、 云服务、 虚拟机和服务结构的功能以帮助您做出最佳选择。 有关每个选项的 SLA 的新信息，请参阅[Azure 服务级别协议要求](/support/legal/sla/)。

功能|应用程序服务 （web 应用程序）|云服务 （web 角色）|虚拟机|服务结构|备注
---|---|---|---|---|---
接近即时部署|X|||X|部署到云服务的应用程序或应用程序更新或创建虚拟机，需要几分钟的最小;应用程序部署到 web 应用程序需要秒钟。
扩展到较大机，而无需重新部署|X|||X|
Web 服务器实例共享内容和配置，这意味着您不必重新部署或重新配置您扩展。|X|||X|
多个部署环境 （生产和转移）|X|X||X|服务结构使您能够为您的应用程序的多个环境或部署您的应用程序的并行的不同版本。
自动的操作系统更新管理|X|X|||对于未来的服务结构释放计划自动操作系统更新。
平台的无缝切换 （32 位和 64 位之间轻松地移动）|X|X|||
部署使用 GIT，FTP 的代码|X||X||
使用 Web 部署的部署代码|X||X||云服务支持使用 Web 部署将更新部署到单个角色实例。 但是，您不能将其用于初始部署的一个角色，并且如果更新您使用 Web 部署必须对角色的每个实例单独部署。 多个实例才能有资格获得生产环境云服务 SLA 要求。
WebMatrix 支持|X||X||
服务总线、 存储、 SQL 数据库服务访问|X|X|X|X|
主机网站或 web 服务层的多层体系结构|X|X|X|X|
主机的多层体系结构的中间层|X|X|X|X|应用程序服务 web 应用程序可以很容易地承载 REST API 的中间层，和[WebJobs](http://go.microsoft.com/fwlink/?linkid=390226)功能可以承载后台处理的作业。 在专门的网站来实现独立层的可伸缩性，您可以运行 WebJobs。 预览[API 的应用程序](../app-service-api/app-service-api-apps-why-best-platform.md)功能提供了更多的功能，用于承载 REST 服务。
集成的 MySQL 作为服务支持|X|X|X||云服务可以将 MySQL 作为服务集成到 ClearDB 的产品，但不是 Azure 门户网站工作流的一部分。
ASP，Node.js，PHP，Python 支持 ASP.NET，经典|X|X|X|X|服务结构支持创建 web 前端使用[ASP.NET 5](../service-fabric/service-fabric-add-a-web-frontend.md)或您可以作为[来宾可执行](../service-fabric/service-fabric-deploy-existing-app.md)部署任何类型的应用程序 （Node.js、 Java 等）。
向外扩展到多个实例，而无需重新部署|X|X|X|X|虚拟机可以扩展到多个实例，但必须写在其上运行的服务以处理此向外扩展。 您必须配置负载平衡器将请求路由在计算机上，并创建关系组，以防止由于维护或硬件故障的所有实例的同时重新启动。
对 SSL 支持|X|X|X|X|为应用程序服务 web 应用程序，用于自定义域名的 SSL 仅支持基本和标准模式。 有关与 web 应用程序中使用 SSL 的信息，请参阅[配置 Azure 网站的 SSL 证书](../app-service-web/web-sites-configure-ssl-certificate.md)。
Visual Studio 集成|X|X|X|X|
远程调试|X|X|X||
部署使用 TFS 的代码|X|X|X|X|
[Azure 虚拟网络](/services/virtual-network/)与网络隔离|X|X|X|X|请参阅[Azure 网站虚拟网络集成](/blog/2014/09/15/azure-websites-virtual-network-integration/)
对[Azure 的流量管理器](/services/traffic-manager/)支持|X|X|X|X|
集成的终结点监视|X|X|X||
对服务器的远程桌面访问||X|X|X|
任何自定义 MSI 安装||X|X|X|服务结构允许您主机作为[来宾可执行](../service-fabric/service-fabric-deploy-existing-app.md)任何可执行文件或您可以在虚拟机上安装任何应用程序。
能够定义/执行启动任务||X|X|X|
可以侦听 ETW 事件||X|X|X|

## <a name="scenarios"></a>方案和建议

以下是一些常见的应用方案可能最适合于每个 web 宿主选项方面的 Azure 的建议。

- [我需要与后台处理和数据库后端运行部署资产与集成的业务应用程序的 web 前端。](#onprem)
- [我需要一种可靠的方法来承载我公司的网站，能够很好地扩展，提供全球到达。](#corp)
- [我有在 Windows Server 2003 上运行 IIS6 应用程序。](#iis6)
- [我是小企业主，我需要我的网站宿主的廉价途径但构思未来的发展。](#smallbusiness)
- [我是该站点的图形设计器中，，我想要设计和构建 web 站点，对于我的客户。](#designer)
- [我要将我的多层应用程序与 web 前端迁移到云。](#multitier)
- [我的应用程序依赖于高度自定义的 Windows 或 Linux 环境，我想要将它移动到云。](#custom)
- [我的网站使用开源软件，我想在 Azure 中承载它。](#oss)
- [我有的业务线应用程序需要连接到公司网络。](#lob)
- [我想要承载 REST API 或移动客户端的 web 服务。](#mobile)


### <a id="onprem"></a>我需要与后台处理和数据库后端运行部署资产与集成的业务应用程序的 web 前端。

Azure 应用程序服务是非常复杂的业务应用程序的解决方案。 它使您可以开发应用程序的负载平衡平台上自动缩放、 通过 Active Directory，保护和连接到您的内部资源。 它使得管理容易通过世界一流的门户和 Api，这些应用程序，使您可以深入了解客户怎样使用这些应用程序理解工具。 [Webjobs][]功能允许您运行后台进程，作为 web 层中，同时混合连接和 VNET 功能的一部分的任务可方便连接回内部资源。 Azure 应用程序服务 web 应用程序提供了三个 9 SLA 并使您能够︰

* 自我修复、 自动修补的云平台上可靠地运行您的应用程序。
* 自动缩放跨数据中心的全球网络。
* 备份和灾难恢复的恢复。
* 为 ISO、 SOC2 和 PCI 兼容。
* 与 Active Directory 集成

### <a id="corp"></a>我需要一种可靠的方法来承载我公司的网站，能够很好地扩展，提供全球到达。

Azure 应用程序服务是好的解决方案，用于承载的公司网站。 它使 web 应用程序扩展快速轻松地跨数据中心的一个全球网络，满足需求。 它提供了本地范围、 容错能力和智能流量管理。 所有在平台上，提供世界一流的管理工具，使您可以快速而轻松地能深入了解网站运行状况和网站流量。 Azure 应用程序服务 web 应用程序提供了三个 9 SLA 并使您能够︰

* 自我修复、 自动修补的云平台上可靠地运行您的网站。
* 自动缩放跨数据中心的全球网络。
* 备份和灾难恢复的恢复。
* 管理日志和通信与集成的工具。
* 为 ISO、 SOC2 和 PCI 兼容。
* 与 Active Directory 集成

### <a id="iis6"></a>我有在 Windows Server 2003 上运行 IIS6 应用程序。

Azure 应用程序服务容易避免与迁移旧 IIS6 应用程序相关的基础结构成本。 Microsoft 创建了[易于使用的迁移工具和详细的迁移指导](https://www.movemetowebsites.net/)，使您能够检查兼容性，并确定需要进行的任何更改。 与 Visual Studio，TFS 和常用的 CMS 工具集成便于部署 IIS6 直接到云中的应用程序。 一旦部署，Azur 门户提供了强大的管理工具，使您能够缩小规模来控制成本，以满足必要的要求。 使用迁移工具可以︰

* 快速而轻松地迁移到云旧版 Windows Server 2003 web 应用程序。
* 选择此选项将保留您附加的 SQL 数据库内部创建一个混合应用程序。
* 自动移动旧版应用程序和 SQL 数据库。

### <a id="smallbusiness"></a>我是小企业主，我需要我的网站宿主的廉价途径但构思未来的发展。

Azure 应用程序服务是这种情况下，好的解决方案，因为您可以开始使用它免费，然后在需要时添加更多的功能。 每个免费的 web 应用程序附带提供的 Azure 域 (*your_company*。 azurewebsites.net)，和平台包含集成的部署和管理工具，以及使其易于入门应用程序库。 还有很多其他服务和缩放选项，使网站发展提高了的用户的需求。 使用 Azure 应用程序服务，您可以︰

- 以自由层开始，然后根据需要扩展。
- 使用应用程序库可以快速设置常用 web 应用程序，如 WordPress。
- 根据需要为您的应用程序中添加其他 Azure 服务和功能。
- 保护您的 web 应用程序使用 HTTPS。

### <a id="designer"></a>我是该站点的图形设计器中，并且我想要设计和构建的网站对于我的客户

对于 web 开发人员和设计人员，Azure 应用程序服务与各种框架和工具可以方便地集成，包括 Git 和 FTP，部署支持并提供了使用的工具和服务，例如 Visual Studio 和 SQL 数据库的紧密集成。 使用应用程序服务，您可以︰

- 用于[自动化的任务]的命令行工具[scripting]。
- 使用[.Net]的常用语[dotnet]， [PHP][]， [Node.js][nodejs]，和[Python][]。
- 选择三种不同的缩放级别为向上扩展到非常高的容量。
- 与其他 Azure 服务，例如[SQL 数据库]集成[sqldatabase]，[服务总线][servicebus]和[存储][]或从[Azure 存储]合作伙伴产品[azurestore]，MongoDB MySQL 等。
- 与 Visual Studio，Git，WebMatrix、 WebDeploy、 TFS 和 FTP 的工具集成。

### <a id="multitier"></a>我要将我的多层应用程序与 web 前端迁移到云

如果您正在运行一个多层应用程序，如 web 服务器连接到一个数据库，Azure 应用程序服务是提供与 SQL Azure 数据库紧密集成，一个不错的选择。 然后您可以使用 WebJobs 功能运行后端进程。

选择一个或多个您层服务结构，如果您需要更多服务器环境中，例如为您的服务器能够远程控制或配置服务器启动任务。

如果您想要使用自己计算机的图像或运行服务器软件或服务，您不能配置服务连接结构上，虚拟机选择的一个或多个您层。

### <a id="custom"></a>我的应用程序依赖于高度自定义的 Windows 或 Linux 环境，我想要将它移动到云。

如果您的应用程序需要复杂的安装或配置软件和操作系统，则虚拟机可能是最好的解决办法。 使用虚拟机，您可以︰

- 使用虚拟机库与操作系统，如 Windows 或 Linux，启动，然后您的应用程序要求对其进行自定义。
- 创建并上载现有内部服务器在 Azure 中的虚拟机上运行的自定义图像。

### <a id="oss"></a>我的网站使用开源软件，并且我想要在 Azure 中承载它

如果开源框架支持的应用程序服务，语言上，并自动为您配置应用程序所需的框架。 应用程序服务，您可以︰

- 使用许多流行打开源语， [.NET][dotnet]， [PHP][]， [Node.js][nodejs]，和[Python][]。
- 设置 WordPress，Drupal、 Umbraco、 DNN 和许多其他第三方 web 应用程序。
- 迁移现有应用程序或创建一个新应用程序库中。

如果应用程序服务不支持您的开放源码框架，您可以运行它其他 Azure web 宿主选项之一。 与虚拟机安装和配置软件，机器在图像上，它可以是 Windows 或基于 Linux 的。

### <a id="lob"></a>我有的业务线应用程序需要连接到企业网络

如果您想要创建的业务线应用程序，您的网站可能需要直接访问服务或公司网络上的数据。 这是应用程序服务，服务结构，以及虚拟机使用[Azure 虚拟网络服务](/services/virtual-network/)。 应用程序服务可以使用[VNET 集成功能](https://azure.microsoft.com/blog/2014/09/15/azure-websites-virtual-network-integration/)，它允许您 Azure 应用程序就好像您公司网络上运行。

### <a id="mobile"></a>我想要承载 REST API 或移动客户端的 web 服务

基于 HTTP 的 web 服务，可以支持各种客户端，包括移动客户端。 与 Visual Studio，以使其更容易地创建和使用 REST 服务集成，如 ASP.NET Web API 的框架。  这些服务公开从网站终结点，所以可以使用任何 web 宿主在 Azure 上的技术来支持此方案。 但是，应用程序服务是个不错的选择，用于承载 REST Api。 使用应用程序服务，您可以︰

- 快速创建一个[移动应用程序](../app-service-mobile/app-service-mobile-value-prop.md)或[API 的应用程序](../app-service-api/app-service-api-apps-why-best-platform.md)来承载 HTTP web 服务在 Azure 的遍布全球的数据中心之一。
- 迁移现有的服务，或创建新的。
- 可用性的一个实例，实现 SLA 或扩展到多个专用机。
- 使用已发布的网站提供给任何包括移动客户端的 HTTP 客户端的 REST Api。

> [AZURE.NOTE]
> 如果您想要开始使用 Azure 应用程序服务帐户的注册之前，转到<a href="https://trywebsites.azurewebsites.net/">https://trywebsites.azurewebsites.net</a>，在其中可以立即短期初学者应用程序中创建 Azure 应用程序服务免费。 不需要信用卡，没有承诺。

## <a id="nextsteps"></a>下一步行动

有关三个 web 宿主选项，请参阅[推出 Azure](../fundamentals-introduction-to-azure.md)。

若要开始与您为您的应用程序选择的选项，请参阅以下资源︰

* [Azure 应用程序服务](/documentation/services/app-service/)
* [Azure 的云服务](/documentation/services/cloud-services/)
* [Azure 的虚拟机](/documentation/services/virtual-machines/)
* [服务结构](/documentation/services/service-fabric)

<!-- URL List -->

[Azure 应用程序服务]: /services/app-service/
[云服务]: http://go.microsoft.com/fwlink/?LinkId=306052
[虚拟机]: http://go.microsoft.com/fwlink/?LinkID=306053
[服务结构]: /services/service-fabric
[ClearDB]: http://www.cleardb.com/
[WebJobs]: http://go.microsoft.com/fwlink/?linkid=390226&clcid=0x409
[Configuring an SSL certificate for an Azure Website]: http://www.windowsazure.com/develop/net/common-tasks/enable-ssl-web-site/
[azurestore]: http://www.windowsazure.com/gallery/store/
[scripting]: http://www.windowsazure.com/documentation/scripts/?services=web-sites
[dotnet]: http://www.windowsazure.com/develop/net/
[nodejs]: http://www.windowsazure.com/develop/nodejs/
[PHP]: http://www.windowsazure.com/develop/php/
[Python]: http://www.windowsazure.com/develop/python/
[servicebus]: http://www.windowsazure.com/documentation/services/service-bus/
[sqldatabase]: http://www.windowsazure.com/documentation/services/sql-database/
[存储]: http://www.windowsazure.com/documentation/services/storage/

<!-- IMG List -->

[ChoicesDiagram]: ./media/choose-web-site-cloud-service-vm/Websites_CloudServices_VMs_3.png
