<properties
   pageTitle="添加到虚拟网络的 VNet 网关使用资源管理器和 PowerShell 的 ExpressRoute |Microsoft Azure"
   description="这篇文章将引导您完成添加到已经创建的资源管理器 VNet 的 ExpressRoute Vnet 网关"
   documentationCenter="na"
   services="expressroute"
   authors="charwen"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>

<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="10/10/2016"
   ms.author="charwen"/>

# <a name="configure-a-virtual-network-gateway-for-expressroute-using-resource-manager-and-powershell"></a>将虚拟网络网关配置为使用资源管理器和 PowerShell 的 ExpressRoute


> [AZURE.SELECTOR]
- [PowerShell 的资源管理器](expressroute-howto-add-gateway-resource-manager.md)
- [PowerShell 的经典](expressroute-howto-add-gateway-classic.md)


这篇文章将引导您完成步骤来添加、 调整大小和删除预先存在的 VNet (VNet) 的虚拟网络网关。 此配置专门为创建的 VNets 使用**资源管理器部署模型**，并将成为使用步骤 ExpressRoute 配置中。 

**关于 Azure 的部署模型**

[AZURE.INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)] 

## <a name="before-beginning"></a>在开始之前

验证是否已安装此配置所需的 Azure PowerShell cmdlet (1.0.2 或更高版本)。 如果您还没有安装这些 cmdlet，您需要在开始配置步骤之前执行此操作。 有关安装 Azure PowerShell 的详细信息，请参阅[如何安装和配置 Azure PowerShell](../powershell-install-configure.md)。


[AZURE.INCLUDE [expressroute-gateway-rm-ps](../../includes/expressroute-gateway-rm-ps-include.md)]

    
## <a name="next-steps"></a>下一步行动

您已经创建了 VNet 网关后，可以链接您的 VNet 到 ExpressRoute 电路。 请参阅[链接到 ExpressRoute 电路虚拟网络](expressroute-howto-linkvnet-arm.md)。
