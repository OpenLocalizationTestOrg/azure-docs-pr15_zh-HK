<properties 
    pageTitle="安全地连接到后端资源的应用程序服务环境中" 
    description="了解如何安全地连接到后端资源的应用程序服务环境中。" 
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

# <a name="securely-connecting-to-backend-resources-from-an-app-service-environment"></a>安全地连接到后端资源的应用程序服务环境中 #

## <a name="overview"></a>概述 ##
由于始终创建应用程序服务环境**是**Azure 资源管理器虚拟网络，在**或**经典部署模型[虚拟网络][virtualnetwork]，应用程序服务环境中的其他后端资源的出站连接可以在虚拟的网络上以独占方式流动。  在 6 月的 2016年所做的最新更改，与 ASEs 还可以部署到虚拟网络 （即使用公用地址范围或 RFC1918 地址空间 专用地址）。  

例如，可能会在群集中使用端口 1433 锁定的虚拟机上运行的 SQL Server。  终结点可能 ACLd 只允许相同的虚拟网络上的来自其他资源的访问。  

举一个例子，敏感的终结点可能运行内部和连接到这两个[站点之间]通过 Azure[ SiteToSite] [Azure ExpressRoute]或[ExpressRoute]连接。  因此，只有 ExpressRoute 隧道的站点到站点连接的虚拟网络中的资源将能够访问本地终结点。

对于所有这些情况下，在应用程序服务环境上运行的应用程序将能够安全地连接到不同的服务器和资源。  从相同的虚拟网络中的专用端点的应用程序服务环境中运行的应用程序的出站通信或连接到相同的虚拟网络，通过虚拟网络将只流。  出站通信到专用的终结点不会在公共 Internet 上。

从虚拟网络中的终结点的应用程序服务环境，一点应用于出站通信。  应用程序服务的环境不能到达终结点的虚拟机位于**同一**子网的应用程序服务环境。  这通常应该不是问题，只要应用程序服务环境部署到由应用程序服务环境保留以供独占使用的子网。

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="outbound-connectivity-and-dns-requirements"></a>出站连接和 DNS 要求 ##
为应用程序服务环境中能正常工作，它需要到各个端点的出站访问权限。 [网络配置的 ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity)文章"需要网络连接"一节中为外部端点由 ASE 的完整列表。

应用程序服务环境需要有效 DNS 基础结构配置为虚拟网络。  如果出于任何原因的 DNS 配置已更改在创建应用程序服务环境后，开发人员可以强制要领取新的 DNS 配置应用程序服务环境。  触发滚动环境重新启动使用位于顶部的应用程序服务环境管理 blade 在门户中的"重启"图标将会导致环境要领取新的 DNS 配置。

此外建议在 vnet 上的任何自定义 DNS 服务器是安装程序创建一个应用程序服务环境之前提前。  如果虚拟网络的 DNS 配置更改时要创建的应用程序服务环境，这将导致应用程序服务环境创建过程失败。  在相同的脉络，如果自定义的 DNS 服务器位于另一端的 VPN 网关和 DNS 服务器将无法访问或不可用，应用程序服务环境创建过程也将失败。

## <a name="connecting-to-a-sql-server"></a>连接到 SQL Server
常见的 SQL Server 配置已在端口 1433年上侦听终结点︰

![SQL Server 终结点][SqlServerEndpoint]

有两种方法限制到此终结点的通信︰


- [网络访问控制列表][ NetworkAccessControlLists] （网络 Acl）

- [网络安全组][NetworkSecurityGroups]


## <a name="restricting-access-with-a-network-acl"></a>限制与网络 ACL 的访问

可以使用网络访问控制列表保护端口 1433年。  白名单客户端下面的示例中解决源自于在虚拟的网络，并阻止对其他所有客户端的访问。

![网络访问控制列表示例][NetworkAccessControlListExample]

在同一个虚拟网络中为 SQL Server 应用程序服务环境中运行的任何应用程序将能够连接到 SQL Server 实例的 SQL Server 虚拟机使用的**VNet 内部**IP 地址。  

下面的示例连接字符串引用 SQL Server 使用专用 IP 地址。

    Server=tcp:10.0.1.6;Database=MyDatabase;User ID=MyUser;Password=PasswordHere;provider=System.Data.SqlClient

尽管虚拟机具有一个公共的端点，将由于网络 ACL 拒绝使用的公用 IP 地址的连接尝试。 

## <a name="restricting-access-with-a-network-security-group"></a>限制访问与网络安全组
保护访问另一种方法是通过网络安全组。  网络安全组可以应用到单个虚拟机，或包含虚拟机的子网。

第一次网络安全组将需要创建︰

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

限制访问 VNet 内部通信是通过网络安全组非常简单。  网络安全组中的默认规则只允许从相同的虚拟网络中其他网络的客户端的访问。

因此锁定访问 SQL Server 是简单，只需将具有其默认规则的网络安全组应用于用来运行 SQL Server 或子网包含虚拟机的虚拟机。

下面的示例将应用于包含子网的网络安全组︰

    Get-AzureNetworkSecurityGroup -Name "testNSGExample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-1'
    
最终结果是一组拦截的外部访问，同时允许 VNet 内部访问的安全规则︰

![默认网络安全规则][DefaultNetworkSecurityRules]


## <a name="getting-started"></a>入门教程
所有的文章和方式-的[自述文件中的服务应用程序环境](../app-service/app-service-app-service-environments-readme.md)中提供应用程序服务环境为。

若要开始使用应用程序服务的环境，请参见[应用程序服务环境介绍][IntroToAppServiceEnvironment]

控制入站的流量到您的应用程序服务环境周围的详细信息，请参阅[控制到应用程序服务环境的入站的通信][ControlInboundASE]

在 Azure 应用程序服务平台有关的详细信息，请参阅[Azure 应用程序服务][AzureAppService]。

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[ControlInboundTraffic]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[SiteToSite]: https://azure.microsoft.com/documentation/articles/vpn-gateway-site-to-site-create/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[NetworkAccessControlLists]: https://azure.microsoft.com/documentation/articles/virtual-networks-acl/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/ 
[ControlInboundASE]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/ 

<!-- IMAGES -->
[SqlServerEndpoint]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/SqlServerEndpoint01.png
[NetworkAccessControlListExample]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/NetworkAcl01.png
[DefaultNetworkSecurityRules]: ./media/app-service-app-service-environment-securely-connecting-to-backend-resources/DefaultNetworkSecurityRules01.png 
