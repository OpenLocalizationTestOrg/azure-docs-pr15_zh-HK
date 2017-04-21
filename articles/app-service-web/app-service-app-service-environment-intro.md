<properties 
    pageTitle="应用程序服务环境简介" 
    description="了解提供了安全、 加入 VNet 的、 专用的比例单位运行您的应用程序的所有应用程序服务环境功能。" 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/04/2016"
    ms.author="stefsch"/>

# <a name="introduction-to-app-service-environment"></a>应用程序服务环境简介

## <a name="overview"></a>概述 ##
应用程序服务环境是[特优][PremiumTier]服务提供充分独立和专用的高比例，其中包括[Web 应用程序]安全地运行 Azure 应用程序服务的应用程序环境的 Azure 应用程序服务的计划选项[WebApps]，[移动应用程序][MobileApps]，和[API 应用程序][APIApps]。  

应用程序服务环境是需要的应用程序工作负载的理想选择︰

- 非常高的比例
- 隔离组和网络的安全访问

客户可以创建多个应用程序服务环境中单个 Azure 区域，以及跨多个 Azure 的地区。  这使得应用程序服务环境进行水平缩放状态的应用程序层以支持高 RPS 工作负载的理想选择。

应用程序服务环境独立于运行仅单个客户的应用程序，并且总是部署到虚拟网络。  客户有细粒度控制两个入站和出站应用程序的网络通信，并且应用程序可以通过内部公司资源的虚拟网络建立高速安全连接。

所有的文章和方式-的有关应用程序服务的环境有在[自述文件中的服务应用程序环境](../app-service/app-service-app-service-environments-readme.md)中。

概述如何在应用程序服务环境启用高比例并固定网络访问，请参阅[AzureCon 深究][AzureConDeepDive]上的应用程序服务的环境 ！

深层水平缩放使用多个应用程序服务环境请参阅文章如何设置[地理分布式应用程序的占地面积][GeodistributedAppFootprint]。

AzureCon 深俯冲中所示的安全体系结构的配置方式，请参阅文章上实施[分层安全体系结构](app-service-app-service-environment-layered-security.md)与应用程序服务环境。

在应用程序服务环境上运行的应用程序可以有他们的 web 应用程序防火墙 (WAF) 等上游设备的访问。  配置[应用程序服务环境 WAF](app-service-app-service-environment-web-application-firewall.md)文章涵盖这种情况。 

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="dedicated-compute-resources"></a>专用的计算资源 ##
所有的应用程序服务环境中的计算资源专门用于单个订阅，以及应用程序服务环境可配置多达五十 （50） 的计算资源供独占使用单个应用程序。

应用程序服务环境组成前端的计算资源池，以及一至三个工作人员计算资源池。 

前端的池包含负责为很好地自动负载平衡的应用程序服务环境中的应用程序请求 SSL 端接的计算资源。 

每个辅助池包含计算资源分配给[应用程序服务计划][AppServicePlan]，其中又包含一个或多个 Azure 应用程序服务应用程序。  因为应用程序服务环境中可以有最多三个不同的辅助池，您必须能够灵活地选择不同的计算资源供每个辅助池。  

例如，这允许您创建一个辅助池与较弱的计算资源供开发或测试的应用程序的应用程序服务计划。  第二个 （或甚至第三） 辅助池可以使用功能更强大的计算资源供生产应用程序运行的应用程序服务计划。

有关前端和辅助池的可用的计算资源的数量的详细信息，请参阅[如何配置应用程序服务环境][HowToConfigureanAppServiceEnvironment]。  

有关可用的详细信息计算支持的应用程序服务环境中的资源大小，请查阅[应用程序服务定价]的[AppServicePricing]页，查看应用程序服务环境定价层津贴中的可用选项。

## <a name="virtual-network-support"></a>虚拟网络支持 ##
可以创建一个应用程序服务环境**是**Azure 资源管理器虚拟网络，在**或**经典部署模型虚拟网络 ([在虚拟网络上的多个信息][MoreInfoOnVirtualNetworks])。  因为在虚拟的网络中，始终存在一个应用程序的服务环境，您可以更精确地在虚拟网络的子网，利用虚拟的网络来控制这两个入站和出站网络通信的安全功能。  

应用程序服务环境可能面临公用 IP 地址，或内部面临只有 Azure 内部负载平衡器 (ILB) 地址是互联网。

您可以使用[网络安全组][NetworkSecurityGroups]来限制入站的网络通信应用程序服务环境所在的子网。  这允许您运行背后上游设备和服务，如 web 应用程序防火墙和网络 SaaS 提供商的应用程序。

应用程序还经常需要访问公司资源，如内部数据库和 web 服务。  一种常见的方法是使这些终结点仅供内部流动在 Azure 的虚拟网络中的网络通信。  一旦应用程序服务环境已连接到同一个虚拟网络的内部服务，在该环境中运行的应用程序可以访问它们，包括终结点无法通过[站点对站点]连接到[SiteToSite]和[Azure ExpressRoute] [ExpressRoute]连接。

为应用程序服务环境使用虚拟网络和内部网络上的更多详细信息请查阅[网络体系结构]的下列文章[NetworkArchitectureOverview]，[控制入站流量][ControllingInboundTraffic]，和[安全地连接到 Backends][SecurelyConnectingToBackends]。 

## <a name="getting-started"></a>入门教程

要开始使用服务的应用程序环境，请参阅[如何为创建应用程序服务环境][HowToCreateAnAppServiceEnvironment]

所有的文章和方式-的[自述文件中的服务应用程序环境](../app-service/app-service-app-service-environments-readme.md)中提供应用程序服务环境为。

在 Azure 应用程序服务平台有关的详细信息，请参阅[Azure 应用程序服务][AzureAppService]。

应用程序服务环境网络体系结构的概述，请参阅[网络体系结构概述][NetworkArchitectureOverview]文章。

有关详细信息在应用程序服务环境中使用 ExpressRoute，[快速通道]和服务应用程序的环境，请参阅以下文章[NetworkConfigDetailsForExpressRoute]。

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[MoreInfoOnVirtualNetworks]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePlan]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[WebApps]: http://azure.microsoft.com/documentation/articles/app-service-web-overview/
[MobileApps]: http://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop-preview/
[APIApps]: http://azure.microsoft.com/documentation/articles/app-service-api-apps-why-best-platform/
[LogicApps]: http://azure.microsoft.com/documentation/articles/app-service-logic-what-are-logic-apps/
[AzureConDeepDive]:  https://azure.microsoft.com/documentation/videos/azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps/
[GeodistributedAppFootprint]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-geo-distributed-scale/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[HowToConfigureanAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[ControllingInboundTraffic]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[SecurelyConnectingToBackends]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-securely-connecting-to-backend-resources/
[NetworkArchitectureOverview]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[NetworkConfigDetailsForExpressRoute]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 

<!-- IMAGES -->

 
