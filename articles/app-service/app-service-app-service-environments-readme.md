<properties 
    pageTitle="应用程序服务环境 |Microsoft Azure" 
    description="Azure 应用程序服务环境是什么？ 应用程序服务环境简介。" 
    keywords="azure 应用程序服务环境中，虚拟的网络，确保网络的安全"
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

# <a name="app-service-environment-documentation"></a>应用程序服务环境文档

应用程序服务环境是[特优][PremiumTier]服务提供充分独立和专用的高比例，其中包括[Web 应用程序]安全地运行 Azure 应用程序服务的应用程序环境的 Azure 应用程序服务的计划选项[WebApps]，[移动应用程序][MobileApps]，和[API 应用程序][APIApps]。  

应用程序服务环境是需要的应用程序工作负载的理想选择︰

- 非常高的比例
- 隔离组和网络的安全访问

客户可以创建多个应用程序服务环境中单个 Azure 区域，以及跨多个 Azure 的地区。  这使得应用程序服务环境进行水平缩放状态的应用程序层以支持高 RPS 工作负载的理想选择。

应用程序服务环境独立于运行仅单个客户的应用程序，并且总是部署到虚拟网络。  客户可以使用[网络安全组]这两个应用入站和出站网络通信的细粒度控制[NetworkSecurityGroups]。  应用程序还可以通过内部公司资源的虚拟网络建立高速安全连接。

应用程序经常需要访问公司资源，如内部数据库和 web 服务。  在应用程序服务环境上运行的应用程序可以访问资源无法通过[站点对站点]连接到[SiteToSite]VPN 和[Azure ExpressRoute] [ExpressRoute]连接。

* [应用程序服务环境是什么？](../app-service-web/app-service-app-service-environment-intro.md)
* [创建应用程序服务环境](../app-service-web/app-service-web-how-to-create-an-app-service-environment.md)
* [在一个应用程序服务的环境中创建的应用程序](../app-service-web/app-service-web-how-to-create-a-web-app-in-an-ase.md)
* [创建和使用应用程序服务环境中使用内部负载平衡器](../app-service-web/app-service-environment-with-internal-load-balancer.md)
* [配置应用程序服务环境](../app-service-web/app-service-web-configure-an-app-service-environment.md) 
* [在应用程序服务环境中缩放应用程序](../app-service-web/app-service-web-scale-a-web-app-in-an-app-service-environment.md)
* [网络安全和体系结构](../app-service-web/app-service-app-service-environment-network-architecture-overview.md)

## <a name="how-tos"></a>方法的

[AZURE.INCLUDE [app-service-blueprint-app-service-environment](../../includes/app-service-blueprint-app-service-environment.md)]


## <a name="videos"></a>视频
[AZURE.VIDEO azurecon-2015-deploying-highly-scalable-and-secure-web-and-mobile-apps]

[AZURE.VIDEO microsoft-ignite-2015-running-enterprise-web-and-mobile-apps-on-azure-app-service]


<!-- LINKS -->
[PremiumTier]: http://azure.microsoft.com/pricing/details/app-service/
[WebApps]: http://azure.microsoft.com/documentation/articles/app-service-web-overview/
[MobileApps]: http://azure.microsoft.com/documentation/articles/app-service-mobile-value-prop-preview/
[APIApps]: http://azure.microsoft.com/documentation/articles/app-service-api-apps-why-best-platform/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
