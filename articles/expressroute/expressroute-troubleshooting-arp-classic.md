<properties
   pageTitle="ExpressRoute 的故障排除指南︰ 获取 ARP 表 |Microsoft Azure"
   description="此页提供了获取 ExpressRoute 电路的 ARP 表的说明。"
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

# <a name="expressroute-troubleshooting-guide-getting-arp-tables-in-the-classic-deployment-model"></a>ExpressRoute 的故障排除指南︰ 经典的部署模型中获取 ARP 表

> [AZURE.SELECTOR]
[PowerShell 的资源管理器](expressroute-troubleshooting-arp-resource-manager.md)
[PowerShell 的经典](expressroute-troubleshooting-arp-classic.md)

这篇文章将引导您完成为 Azure ExpressRoute 电路获取地址解析协议 (ARP) 表的步骤。

>[AZURE.IMPORTANT] 本文档旨在帮助您诊断和解决简单的问题。 它不是要取代的 Microsoft 技术支持。 如果不能通过使用以下指导来解决问题，请与[Microsoft Azure 帮助 + 支持](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)打开支持请求。

## <a name="address-resolution-protocol-arp-and-arp-tables"></a>地址解析协议 (ARP) 和 ARP 表
ARP 是在[RFC 826](https://tools.ietf.org/html/rfc826)中定义的第 2 层协议。 ARP 将用来映射到 IP 地址的以太网地址 （MAC 地址）。

ARP 表提供 IPv4 地址和 MAC 地址为特定对等的映射。 ExpressRoute 电路对等的 ARP 表提供了每个接口 （主和次） 的以下信息︰

1. MAC 地址与内部路由器接口 IP 地址的映射
2. MAC 地址 ExpressRoute 路由器接口 IP 地址的映射
3. 映射区的法定年龄

ARP 表格可以帮助与验证第二层配置和基本的第 2 层连接问题进行故障排除。

下面是一个 ARP 表的一个示例︰

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


以下部分提供有关如何查看被看到 ExpressRoute 边缘路由器的 ARP 表的信息。

## <a name="prerequisites-for-using-arp-tables"></a>使用 ARP 表的先决条件

确保您具有下面，然后再继续︰

 - 有效 ExpressRoute 电路，配置了至少一个对等。 电路连接提供程序必须完全配置。 您 （或您连接的提供程序） 必须至少一个 peerings （Azure 私有的 Azure 公共的或者 Microsoft） 上配置此电路。

 - 用于配置 peerings （Azure 私有的 Azure 公众和 Microsoft） 的 IP 地址范围。 检查中[ExpressRoute 路由要求页](expressroute-routing.md)以了解如何将 IP 地址映射到您的 aise 和 ExpressRoute 侧上的接口的 IP 地址分配示例。 可以通过查看[ExpressRoute 的对等配置页](expressroute-howto-routing-classic.md)获得对等的配置信息。

 - 网络团队或连接供应商提供有关信息的 MAC 地址使用这些 IP 地址的接口。

 - Azure （1.50 或更高版本） 最新的 Windows PowerShell 模块。

## <a name="arp-tables-for-your-expressroute-circuit"></a>ExpressRoute 电路的 ARP 表
本部分提供有关如何查看每种类型的对等使用 PowerShell 的 ARP 表的说明。 在继续之前，您或您连接的提供程序需要配置对等。 每个电路都有两个 （主和次） 的路径。 您可以单独检查每条路径的 ARP 表。

### <a name="arp-tables-for-azure-private-peering"></a>Azure 专用对等的 ARP 表
以下 cmdlet 的 Azure 专用对等提供 ARP 表︰

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure private peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Primary

        # ARP table for Azure private peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Private -Path Secondary

以下是其中一个路径的示例输出︰

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-azure-public-peering"></a>Azure 公共对等的 ARP 表︰
以下 cmdlet 的 Azure 公共对等提供 ARP 表︰

        # Required variables
        $ckt = "<your Service Key here>

        # ARP table for Azure public peering--primary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Primary

        # ARP table for Azure public peering--secondary path
        Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Public -Path Secondary

以下是其中一个路径的示例输出︰

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           10.0.0.1 ffff.eeee.dddd
          0 Microsoft         10.0.0.2 aaaa.bbbb.cccc


以下是其中一个路径的示例输出︰

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           64.0.0.1 ffff.eeee.dddd
          0 Microsoft         64.0.0.2 aaaa.bbbb.cccc


### <a name="arp-tables-for-microsoft-peering"></a>Microsoft 对等的 ARP 表
以下 cmdlet 的 Microsoft 对等提供 ARP 表︰

    # ARP table for Microsoft peering--primary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Primary

    # ARP table for Microsoft peering--secondary path
    Get-AzureDedicatedCircuitPeeringArpInfo -ServiceKey $ckt -AccessType Microsoft -Path Secondary


对于其中一个路径的示例输出所示︰

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc


## <a name="how-to-use-this-information"></a>如何使用此信息
对等的 ARP 表可以用于验证第二层配置和连接。 本节概述了 ARP 表在不同情况下的显示效果。

### <a name="arp-table-when-a-circuit-is-in-an-operational-expected-state"></a>当电路处于运营 （预期） 的状态的 ARP 表

 - ARP 表都有一个有效的 IP 和 MAC 地址的内部一侧的条目和 Microsoft 一面类似的条目。
 - 内部 IP 地址的最后一个八位字节始终是奇数。
 - Microsoft 的 IP 地址的最后一个八位字节始终是偶数。
 - 相同的 MAC 地址将出现在所有三个 peerings （主/辅） Microsoft 侧面。


        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
         10 On-Prem           65.0.0.1 ffff.eeee.dddd
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

### <a name="arp-table-when-its-on-premises-or-when-the-connectivity-provider-side-has-problems"></a>ARP 表时内部或连接提供程序端时出现问题

 ARP 表中显示只有一个条目。 它显示 MAC 地址与在 Microsoft 端使用的 IP 地址之间的映射。

        Age InterfaceProperty IpAddress  MacAddress    
        --- ----------------- ---------  ----------    
          0 Microsoft         65.0.0.2 aaaa.bbbb.cccc

>[AZURE.NOTE] 如果您遇到此类问题，打开连接提供程序以解决它的支持请求。


### <a name="arp-table-when-the-microsoft-side-has-problems"></a>当 Microsoft 端出现问题时的 ARP 表

 - 您不会看到对对等的如果有问题的 Microsoft 一侧显示 ARP 表。
 -  打开[Microsoft Azure 帮助 + 支持](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)的支持请求。 指定您有第二层连接性问题。

## <a name="next-steps"></a>下一步行动

 - 验证第 3 层为 ExpressRoute 电路的配置︰
     - 获取路由摘要以确定 BGP 会话的状态。
     - 获取一个路由表，以确定哪个前缀将公布在 ExpressRoute 之间。
 - 验证通过查看字节和检出数据传输。
 - 如果您仍然遇到问题， [Microsoft Azure 帮助 + 支持](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)与打开支持请求。
