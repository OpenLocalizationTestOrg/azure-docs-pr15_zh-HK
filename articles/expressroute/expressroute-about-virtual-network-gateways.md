<properties 
   pageTitle="有关 ExpressRoute 虚拟网络网关 |Microsoft Azure"
   description="对于 ExpressRoute 了解有关虚拟网络网关。"
   services="expressroute"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager, azure-service-management"/>
<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/03/2016"
   ms.author="cherylmc" />

# <a name="about-virtual-network-gateways-for-expressroute"></a>有关为 ExpressRoute 的虚拟网络网关


虚拟网络网关用于将 Azure 的虚拟网络之间的网络流量发送和内部部署的位置。 配置连接到 ExpressRoute 时，您必须创建和配置虚拟网络网关和虚拟网络网关连接。

当创建虚拟网络网关时，您指定多个设置。 所需设置一个指定是否将为 ExpressRoute 或站点到站点 VPN 通信使用的网关。 在资源管理器部署模型中，设置是-GatewayType。

专用的专用连接上发送网络流量时，您使用 ExpressRoute 的网关类型。 这也称为 ExpressRoute 网关。 当网络流量经由公用的 Internet 发送加密时，您将使用网关类型 Vpn。 这被称为 VPN 网关。 点到站点和 VNet 到 VNet 连接站点对站点使用的 VPN 网关。 

每个虚拟网络可以具有每个网关类型只有一个虚拟网络网关。 例如，可以使用-GatewayType Vpn 的一个虚拟网络网关和一个使用-GatewayType ExpressRoute。 本文重点介绍 ExpressRoute 虚拟网络网关。

## <a name="gwsku"></a>网关 Sku

[AZURE.INCLUDE [expressroute-gwsku-include](../../includes/expressroute-gwsku-include.md)]

如果您想要升级到功能更强大的网关的 SKU 网关，可以在大多数情况下使用调整 AzureRmVirtualNetworkGateway' PowerShell cmdlet。 这也适用于升级到标准和高性能的 Sku。 但是，要升级到 UltraPerformance 的 SKU，您需要重新创建网关。

###  <a name="aggthroughput"></a>通过网关的 SKU 估计聚合吞吐量


下表显示网关类型和估计的聚合吞吐量。 此表适用于资源管理器和经典的部署模型。

[AZURE.INCLUDE [expressroute-table-aggthroughput](../../includes/expressroute-table-aggtput-include.md)] 


## <a name="resources"></a>REST Api 和 PowerShell cmdlet

对于其他的技术资源和虚拟网络网关配置为使用 REST Api 和 PowerShell cmdlet 时的特定语法要求，请参阅以下页面︰

|**经典** | **资源管理器**|
|-----|----|
|[PowerShell](https://msdn.microsoft.com/library/mt270335.aspx)|[PowerShell](https://msdn.microsoft.com/library/mt163510.aspx)|
|[REST API，](https://msdn.microsoft.com/library/jj154113.aspx)|[REST API，](https://msdn.microsoft.com/library/mt163859.aspx)|


## <a name="next-steps"></a>下一步行动

有关可用连接配置详细信息，请参阅[ExpressRoute 概述](expressroute-introduction.md)。 







 
