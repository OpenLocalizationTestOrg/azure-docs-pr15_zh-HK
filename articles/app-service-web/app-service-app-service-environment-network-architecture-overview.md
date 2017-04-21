<properties 
    pageTitle="应用程序服务环境的网络体系结构概述" 
    description="体系结构概述网络拓扑 ofApp 服务的环境。" 
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

# <a name="network-architecture-overview-of-app-service-environments"></a>应用程序服务环境的网络体系结构概述

## <a name="introduction"></a>介绍 ##
应用程序服务的环境，则总是创建[虚拟网络]的子网中[virtualnetwork]-可以与位于相同的虚拟网络拓扑的专用端点进行通信的应用程序服务环境中运行的应用程序。  因为客户可能会锁定其虚拟网络基础结构的部分，务必理解发生了一个应用程序服务环境的网络通信流的类型。

## <a name="general-network-flow"></a>常规网络流 ##
 
当应用程序服务环境 (ASE) 的应用程序使用公用的虚拟 IP 地址 (VIP) 时，所有入站的通信到达该公用的 VIP。  这包括 FTP、 远程调试功能，和 Azure 管理操作的 HTTP 和 HTTPS 通信的应用程序，以及其他通信。  特定的端口 （必需和可选） 的完整列表，可在公用的 VIP 请参阅文章上[控制入站的通信][controllinginboundtraffic]应用程序的服务环境。 

应用程序服务环境还支持正在运行的应用程序绑定到虚拟网络内部地址，也称为 ILB （内部负载平衡器） 地址只。  在 ILB 上启用的 ASE，HTTP 和 HTTPS 通信的应用程序，以及调试的远程调用，到达 ILB 地址上。  最常见的 ILB ASE 配置 FTP/FTPS 通信还将到达 ILB 地址。  但是 Azure 管理操作仍将流至 ILB 公共 VIP 454/455 端口启用 ASE。

下图显示了在其中应用程序绑定到一个公用的虚拟 IP 地址的应用程序服务环境不同的入站和出站网络流量的概述︰

![常规网络流][GeneralNetworkFlows]

应用程序服务环境可以具有不同的专用客户终结点进行通信。  例如，应用程序服务环境中运行的应用程序可以连接到相同的虚拟网络拓扑中的 IaaS 虚拟机上运行的数据库服务器。

>[AZURE.IMPORTANT] 查看网络图中，在不同的子网的应用程序服务环境中部署"其他计算资源"。 资源与 ASE 位于同一子网部署将阻止从 ASE （除了特定的内 ASE 路由） 这些资源的连接。 将部署到不同的子网而 （在相同的 VNET)。 然后，应用程序服务环境能够连接。 不不需要任何额外的配置。

应用程序服务的环境也与 Sql 数据库和通信 Azure 存储资源所需的管理和运行一个应用程序的服务环境。  Sql 和存储资源与进行通信的应用程序服务环境中的某些都位于同一区域作为应用程序服务环境中，而其他人都位于远程 Azure 的地区。  因此，出站连接到 Internet 是始终能够正常运行的应用程序服务环境要求。 

由于子网中部署的应用程序服务环境，网络安全组可用于控制到子网的入站的通信。  如何控制应用程序的服务环境，为入站的通信的详细信息，请参阅以下[文章][controllinginboundtraffic]。

如何从应用程序的服务环境，允许出站 Internet 连接的详细信息，请参阅下文关于使用[快速通道][ExpressRoute]。  本文介绍的方法相同的方法应用在使用站点对站点连接时，使用强制隧道。

## <a name="outbound-network-addresses"></a>出站网络地址 ##
当一个应用程序服务环境，出站调用，IP 地址是始终与出站呼叫。  使用特定的 IP 地址取决于是否调用该终结点所在的虚拟网络拓扑结构，内部或外部虚拟网络拓扑结构。

如果**外部**虚拟网络拓扑的调用该终结点，则用于出站通讯 （亦即出站 NAT 地址） 是公共应用程序服务环境的 VIP。  门户用户界面中都可以找到这个地址属性刀片式服务器中的应用程序服务环境。
 
![出站 IP 地址][OutboundIPAddress]

此地址还可以确定 ASEs 仅通过在应用程序服务环境中，创建一个应用程序，然后执行应用程序的地址*nslookup*有公用的 VIP。 结果的 IP 地址是公用的 VIP，以及应用程序服务环境的出站 NAT 地址。

如果**在**虚拟的网络拓扑的调用该终结点，则将运行应用程序的单个计算资源的内部 IP 地址调用的应用程序的出站地址。  但是并不永久虚拟网络到应用程序的内部 IP 地址的映射。  应用程序可以跨不同的计算资源和可用的计算资源的应用程序服务环境中可以更改缩放操作由于池中移动。

但是，由于应用程序服务环境始终是位于一个子网内，在保证计算资源运行应用程序的内部 IP 地址将始终位于 CIDR 范围的子网。  因此，当细粒度的 Acl 或网络安全组用于安全访问的虚拟网络中的其他终结点，包含应用程序服务环境的子网范围需要被授予访问权限。

下图更详细地显示这些概念︰

![出站网络地址][OutboundNetworkAddresses]

在上面的图中︰

- 由于公共应用程序服务环境的 VIP 是 192.23.1.2，这就是对"Internet"终结点进行调用时使用的出站 IP 地址。
- 包含有关应用程序服务环境网 CIDR 范围是 10.0.1.0/26。  同一虚拟网络基础结构中的其他终结点将从某个地方发出此地址范围内看到来自应用程序的调用。

## <a name="calls-between-app-service-environments"></a>调用应用程序服务环境之间 ##
如果您部署多个应用程序服务环境中的相同的虚拟网络，并进行出站呼叫从一个应用程序服务环境到另一个应用程序服务环境，会出现更复杂的方案。  这种交叉调用也被视为"Internet"调用应用程序服务环境。

下图显示的分层体系结构与应用程序在一个应用程序服务环境 （例如"前门"web 应用程序） 调用第二个应用程序服务环境 （如内部后端 API 的应用程序不能从 Internet 访问） 上的应用程序的示例。 

![调用应用程序服务环境之间][CallsBetweenAppServiceEnvironments] 

在上面的示例应用程序服务环境"ASE 一个"已经出站 IP 地址为 192.23.1.2。  如果运行此应用程序应用程序服务环境使于第二应用程序服务环境 （"ASE 两"） 上运行的应用程序的出站调用位于相同的虚拟网络的出站调用将被视为"Internet"调用。  因此对第二个到达的网络通信的应用程序服务环境将显示来自 192.23.1.2 （即不子网地址范围的第一个应用程序服务环境）。

即使不同的应用程序服务环境间的调用将被视为"Internet"调用时这两种应用程序服务环境位于同一 Azure 区域网络流量仍将保留在 Azure 的区域网络和通过公共互联网中不能将实际流动。  因此可用于网络安全组中的子网上的第二个应用程序服务环境只允许入站的调用在第一个应用程序服务环境中 （出站 IP 地址是 192.23.1.2），从而确保应用程序服务环境之间的安全通信。

## <a name="additional-links-and-information"></a>附加的链接和信息 ##
所有的文章和方式-的[自述文件中的服务应用程序环境](../app-service/app-service-app-service-environments-readme.md)中提供应用程序服务环境为。

详细信息在入站端口使用的应用程序服务的环境，使用网络安全组来控制入站的流量现[这里][controllinginboundtraffic]。

使用用户的详细信息定义要授予出站 Internet 访问应用程序服务的环境是本[文章]中的可用路由[ExpressRoute]。 


<!-- LINKS -->
[virtualnetwork]: http://azure.microsoft.com/services/virtual-network/
[controllinginboundtraffic]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[ExpressRoute]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/

<!-- IMAGES -->
[GeneralNetworkFlows]: ./media/app-service-app-service-environment-network-architecture-overview/NetworkOverview-1.png
[OutboundIPAddress]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundIPAddress-1.png
[OutboundNetworkAddresses]: ./media/app-service-app-service-environment-network-architecture-overview/OutboundNetworkAddresses-1.png
[CallsBetweenAppServiceEnvironments]: ./media/app-service-app-service-environment-network-architecture-overview/CallsBetweenEnvironments-1.png

