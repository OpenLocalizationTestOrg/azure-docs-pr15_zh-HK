<properties 
   pageTitle="ExpressRoute 故障排除指南-获取 ARP 表 |Microsoft Azure"
   description="此页获取 ARP 表 ExpressRoute 电路提供指导"
   documentationCenter="na"
   services="expressroute"
   authors="ganesr"
   manager="carolz"
   editor="tysonn"/>
<tags 
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="10/10/2016"
   ms.author="ganesr"/>

#<a name="expressroute-troubleshooting-guide---getting-arp-tables-in-the-resource-manager-deployment-model"></a>ExpressRoute 故障诊断指南-获取 ARP 表中的资源管理器部署模型

> [AZURE.SELECTOR]
[PowerShell 的资源管理器](expressroute-troubleshooting-arp-resource-manager.md)
[PowerShell 的经典](expressroute-troubleshooting-arp-classic.md)

这篇文章将引导您完成了解 ExpressRoute 电路的 ARP 表的步骤。 

>[AZURE.IMPORTANT] 本文档旨在帮助您诊断和解决简单的问题。 它不是要取代的 Microsoft 技术支持。 如果不能解决问题使用下面所述的指南，必须使用[Microsoft 支持](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)打开支持票。

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>地址解析协议 (ARP) 和 ARP 表
地址解析协议 (ARP) 是在[RFC 826](https://tools.ietf.org/html/rfc826)中定义的第 2 层协议。 ARP 将用来映射的 ip 地址的以太网地址 （MAC 地址）。

ARP 表提供 ipv4 地址和 MAC 地址为特定对等的映射。 ExpressRoute 电路对等的 ARP 表提供以下信息为每个接口 （主和次）

1. 对 MAC 地址的内部路由器接口 ip 地址的映射
2. ExpressRoute 路由器接口的 MAC 地址的 ip 地址的映射
3. 映射的时代

ARP 表格可以帮助第 2 层配置进行验证和故障诊断的基本图层 2 连接问题。 

示例 ARP 表︰ 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


下一节提供有关如何查看检测到 ExpressRoute 边缘路由器的 ARP 表的信息。 

## <a name="prerequisites-for-learning-arp-tables"></a>学习 ARP 表的先决条件

确保您具有下列之前进一步进展

 - 有效的 ExpressRoute 电路，配置了至少一个对等。 电路连接提供程序必须完全配置。 您 （或您连接的提供程序） 必须配置至少一个 peerings （Azure 的私有的 Azure 公共和 Microsoft） 在此电路上。
 - 用于配置 peerings （Azure 的私有的 Azure 公共和 Microsoft） 的 IP 地址范围。 检查以了解如何将 ip 地址映射到接口在您这一边和 ExpressRoute 侧的[ExpressRoute 路由要求页](expressroute-routing.md)中的 ip 地址分配示例。 通过查看[ExpressRoute 的对等配置页](expressroute-howto-routing-arm.md)可以对等配置的信息。
 - 从您的网络小组的信息 / 接口的 MAC 地址上的连接提供程序使用这些 IP 地址。
 - 对 Azure （1.50 或较新版本），必须具有最新的 PowerShell 模块。

## <a name="getting-the-arp-tables-for-your-expressroute-circuit"></a>获取为您 ExpressRoute 电路的 ARP 表
本节说明如何查看每个对等使用 PowerShell 的 ARP 表。 您或您的连接的提供程序必须已配置对等前进一步的进展情况。 每个电路都有两个 （主和次） 的路径。 您可以单独检查每条路径的 ARP 表。

### <a name="arp-tables-for-azure-private-peering"></a>Azure 专用对等的 ARP 表
以下 cmdlet 的 Azure 专用对等提供 ARP 表

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"
        
        # ARP table for Azure private peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Primary
        
        # ARP table for Azure private peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePrivatePeering -DevicePath Secondary 

示例输出所示的路径之一

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>Azure 公共对等的 ARP 表
以下 cmdlet 的 Azure 公共对等提供 ARP 表

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"
        
        # ARP table for Azure public peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Primary
        
        # ARP table for Azure public peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType AzurePublicPeering -DevicePath Secondary 


示例输出所示的路径之一

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1 ffff.eeee.dddd
          0 Microsoft         64.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>Microsoft 对等的 ARP 表
以下 cmdlet 的 Microsoft 对等提供 ARP 表

        # Required Variables
        $RG = "<Your Resource Group Name Here>"
        $Name = "<Your ExpressRoute Circuit Name Here>"
        
        # ARP table for Microsoft peering - Primary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Primary
        
        # ARP table for Microsoft peering - Secodary path
        Get-AzureRmExpressRouteCircuitARPTable -ResourceGroupName $RG -ExpressRouteCircuitName $Name -PeeringType MicrosoftPeering -DevicePath Secondary 


示例输出所示的路径之一

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a>如何使用此信息
对等的 ARP 表可以用于确定验证第二层配置和连接。 本节概述了 ARP 表在不同情形下的效果。

### <a name="arp-table-when-a-circuit-is-in-operational-state-expected-state"></a>当线路处于运营状态 （预期） 的 ARP 表

 - ARP 表必须具有有效的 IP 地址和 MAC 地址的内部边的一个条目而且此 Microsoft 一面类似的条目。 
 - 内部 ip 地址的最后一个八位字节将始终为奇数。
 - Microsoft 的 ip 地址的最后一个八位字节将始终为偶数。
 - 相同的 MAC 地址将出现在所有 3 peerings （主 / 次） Microsoft 侧面。 


        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

### <a name="arp-table-when-on-premises--connectivity-provider-side-has-problems"></a>ARP 表时内部 / 连接提供程序端出现问题

 - 只有一个条目将显示在 ARP 表中。 这将显示 MAC 地址和 IP 地址用 Microsoft 侧之间的映射。 

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

>[AZURE.NOTE] 打开连接提供程序以调试此类问题的支持请求。 


### <a name="arp-table-when-microsoft-side-has-problems"></a>当 Microsoft 端出现问题时的 ARP 表

 - 您不会看到对对等的如果有问题的 Microsoft 一侧显示 ARP 表。 
 -  打开[Microsoft 支持](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)的支持票。 指定您有第二层连接性问题。 

## <a name="next-steps"></a>下一步行动

 - 验证 ExpressRoute 电路的第 3 层配置
     - 获取路由摘要以确定 BGP 会话的状态 
     - 获取路由表，以确定哪个前缀将公布在 ExpressRoute
 - 通过查看字节 / 检出验证数据传输
 - 如果您仍然遇到问题，请与[Microsoft 支持](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)打开支持票。
