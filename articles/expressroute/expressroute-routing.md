<properties
   pageTitle="路由的要求 ExpressRoute |Microsoft Azure"
   description="此页提供用于配置和管理路由 ExpressRoute 电路的详细的要求。"
   documentationCenter="na"
   services="expressroute"
   authors="osamazia"
   manager="ganesr"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/19/2016"
   ms.author="osamazia"/>


# <a name="expressroute-routing-requirements"></a>ExpressRoute 路由要求  

要连接到 Microsoft 云服务使用 ExpressRoute，您将需要设置和管理路由。 某些连接提供程序提供了设置和管理路由作为托管服务。 请与提供此服务，请参阅连接提供程序。 如果他们不这样做，您必须遵守以下要求。 

请参阅有关需要在中设置以便在连接的传送会话说明[电路和路由域](expressroute-circuit-peerings.md)的文章。

>[AZURE.NOTE] Microsoft 不支持高可用性配置的任何路由器冗余协议 （如 HSRP、 VRRP）。 我们依靠重复对 BGP 的会话，每个对等的高可用性。

## <a name="ip-addresses-used-for-peerings"></a>使用 peerings 的 IP 地址

您需要保留的 IP 地址来配置您的网络和微软企业 (MSEEs) 的边缘路由器之间路由的几个块。 本节提供了一系列要求，并介绍有关必须如何获得和使用这些 IP 地址的规则。

### <a name="ip-addresses-used-for-azure-private-peering"></a>使用 Azure 专用对等 IP 地址

您可以使用专用 IP 地址或公用 IP 地址来配置 peerings。 不能用于配置路由的地址范围必须与用于在 Azure 创建虚拟网络地址范围重叠。 

 - 您必须保留 /29 子网或两个 /30 子网的路由接口。
 - 专用 IP 地址或公用 IP 地址，可以是用于路由选择的子网。
 - 子网必须与用于 Microsoft 云客户保留的范围不发生冲突。
 - 如果 /29 子网，它将分成两个 /30 的子网。 
     - 第一个 /30 的子网将用于主链接和第二个/30 的子网将用于辅助链接。
     - 每个 /30 子网，您必须使用 /30 的第一个 IP 地址子网路由器。 Microsoft 将使用 /30 的第二个 IP 地址子网设置 BGP 会话。
     - 您必须设置两个 BGP 会话的[可用性 SLA](https://azure.microsoft.com/support/legal/sla/)我们才能有效。  

#### <a name="example-for-private-peering"></a>私人对等的示例

如果您选择使用 a.b.c.d/29 来设置对等，它将分成两个 /30 的子网。 在下面的示例中，我们将了解如何使用 a.b.c.d/29 子网。 

a.b.c.d/29 将拆分为 a.b.c.d/30 和 a.b.c.d+4/30，并传到 Microsoft 通过提供的 Api。 会将 a.b.c.d+1 用作主要的 PE 为 VRF IP，Microsoft 将使用 a.b.c.d+2 作为主要的 MSEE 为 VRF IP。 会将 a.b.c.d+5 用作辅助 PE 为 VRF IP，Microsoft 将使用 a.b.c.d+6 作为 VRF IP 辅助 MSEE。

请考虑在其中选择 192.168.100.128/29 设置专用的对等情况。 192.168.100.128/29 包括从 192.168.100.128 到 192.168.100.135，在其中地址︰

- 192.168.100.128/30 将被分配到 link1，与使用 192.168.100.129 和微软使用 192.168.100.130 提供程序。
- 192.168.100.132/30 将被分配到 link2，与使用 192.168.100.133 和微软使用 192.168.100.134 提供程序。

### <a name="ip-addresses-used-for-azure-public-and-microsoft-peering"></a>使用 Azure 公共和 Microsoft 对等 IP 地址

BGP 会话设置，必须使用您自己的公用 IP 地址。 Microsoft 必须能够验证 IP 地址通过路由互联网注册机构和互联网路由注册机构的所有权。 

- 您必须使用一个唯一/29 子网或两个 /30 子网设置对等每 ExpressRoute 每个对等的 BGP 电路 （如果您有多个）。 
- 如果 /29 子网，它将分成两个 /30 的子网。 
    - 第一个 /30 的子网将用于主链接和第二个/30 的子网将用于辅助链接。
    - 每个 /30 子网，您必须使用 /30 的第一个 IP 地址子网路由器。 Microsoft 将使用 /30 的第二个 IP 地址子网设置 BGP 会话。
    - 您必须设置两个 BGP 会话的[可用性 SLA](https://azure.microsoft.com/support/legal/sla/)我们才能有效。

## <a name="public-ip-address-requirement"></a>公共 IP 地址要求 

### <a name="private-peering"></a>私人对等 

您可以选择要用于公用或专用 IPv4 地址专用的对等。 因此重叠的地址与其他客户则不能在私人对等的情况下，我们提供端到端通信隔离。 这些地址将不会公布到互联网。 

### <a name="public-peering"></a>公共对等

Azure 的公共对等路径使您可以连接到承载 Azure 中转移其公用 IP 地址的所有服务。 其中包括在[ExpessRoute 的常见问题解答](expressroute-faqs.md)列出服务和由 Isv 在 Microsoft Azure 上承载的任何服务。 连接到 Microsoft Azure 上公共对等的服务始终到 Microsoft 网络中启动从您的网络。 您必须使用与 Microsoft 网络通信量公用 IP 地址。

### <a name="microsoft-peering"></a>Microsoft 对等

Microsoft 对等路径，可以连接到 Microsoft 通过 Azure 的公共对等路径中不受支持的云服务。 服务列表中的包括 Office 365 提供服务，如在线交换、 SharePoint Online、 业务、 和 CRM Online 的 Skype。 在 Microsoft 对等情况下，Microsoft 支持双向连接。 才能进入 Microsoft 网络通信发送到 Microsoft 云服务必须使用有效的公用 IPv4 地址。

请确保您的 IP 地址以及作为编号注册到您在注册表的下列之一。

- [ARIN](https://www.arin.net/)
- [APNIC](https://www.apnic.net/)
- [AFRINIC](https://www.afrinic.net/)
- [LACNIC](http://www.lacnic.net/)
- [RIPENCC](https://www.ripe.net/)
- [RADB](http://www.radb.net/)
- [ALTDB](http://altdb.net/)

>[AZURE.IMPORTANT] 在 ExpressRoute 上公布给 Microsoft 的公用 IP 地址必须不会公布到互联网。 这可能会中断连接到其他 Microsoft 服务。 但是，使用与在 Microsoft 内部的 O365 终结点通信网络中的服务器的公用 IP 地址可能会公布在 ExpressRoute 上。 

## <a name="dynamic-route-exchange"></a>动态路由交换

通过 eBGP 协议将路由交换。 MSEEs 和路由器之间建立 EBGP 的会话。 对 BGP 的会话进行身份验证并不要求。 如果需要，可以配置一个 MD5 哈希。 有关配置 BGP 会话的信息，请参阅[配置路由](expressroute-howto-routing-classic.md)和[环路供应流和电路状态](expressroute-workflows.md)。

## <a name="autonomous-system-numbers"></a>自治系统号

Microsoft 将使用 AS 12076 Azure 公共、 专用 Azure 和 Microsoft 对等。 我们保留了 Asn 65515 从到 65520 供内部使用。 支持 16 和 32 位数字。

不没有数据传输对称周围任何要求。 正向和返回路径，也能够遍历不同的路由器对。 跨多个电路对属于您，必须从两侧公布相同的路由。 不需要是相同的路由跃点数。

## <a name="route-aggregation-and-prefix-limits"></a>路由集合和前缀限制

我们支持通过 Azure 的专用对等广告给我们的最多 4000 个前缀。 这可以增加到 10000 前缀，如果启用了 ExpressRoute 高级加载项。 我们接受最多每个公用的 Azure 的 BGP 会话 200 前缀和 Microsoft 对等。 

如果前缀数超出限制，则将丢弃 BGP 会话。 我们将接受私人对等链接只有在默认工艺路线。 提供程序必须过滤掉默认工艺路线并从公用 Azure 的专用 IP 地址 (RFC 1918) 和 Microsoft 对等路径。 

## <a name="transit-routing-and-cross-region-routing"></a>在传输路由和路由选择跨区域

ExpressRoute 不能配置为路由器中转。 您需要依赖于您的连接提供程序传输路由服务。

## <a name="advertising-default-routes"></a>广告的默认路由

仅在 Azure 专用的对等会话上允许使用默认路由。 在这种情况下，我们将传送到您的网络从关联的虚拟网络的所有通信。 广告的默认路由到专用对等将导致从 Azure 阻止 internet 路径。 您必须依赖于您公司的边缘来路由通信和互联网服务在 Azure 中承载的。 

 若要启用到其他 Azure 服务和基础结构服务的连接性，必须确保以下各项之一处于的位置︰

 - Azure 公共对等通信路由到公共端点已启用
 - 使用用户定义路由允许需要互联网连接的每个子网的互联网连接。
 
>[AZURE.NOTE] 默认工艺路线的广告会破坏 Windows 和其他 VM 许可证激活。 按照说明[此处](http://blogs.msdn.com/b/mast/archive/2015/05/20/use-azure-custom-routes-to-enable-kms-activation-with-forced-tunneling.aspx)来变通解决此问题。

## <a name="support-for-bgp-communities-preview"></a>对 BGP 社区 （预览） 的支持


本节概述的 BGP 社区与 ExpressRoute 的使用方式。 Microsoft 将公布在公共和 Microsoft 等路径与相应社区面额标记的工艺路线的路由。 这样做的理由而且下面将介绍社区值的详细信息。 但是，Microsoft 将不遵守任何社区值标记为向 Microsoft 公布路由。

如果您要连接到 Microsoft 通过 ExpressRoute 在地缘政治区域内任何一个对等位置，您可以访问所有 Microsoft 云服务到地缘政治边界内的所有区域间。 

例如，如果您连接到 Microsoft 在阿姆斯特丹通过 ExpressRoute，您将有权访问承载于北欧和西欧的所有 Microsoft 云服务。 

请参阅[ExpressRoute 合作伙伴和对等的位置](expressroute-locations.md)有关的地缘政治区域、 关联 Azure 的地区和对等位置的相应 ExpressRoute 的详细列表页。

您可以购买多个 ExpressRoute 电路每个地缘政治区域。 将多个连接提供显著的好处，由于地理冗余来提供高可用性。 您有多个 ExpressRoute 电路的情况下，您将收到一组相同的广告从 Microsoft 公共对等和 Microsoft 对等路径前缀。 这意味着将从到 Microsoft 网络中有多个路径。 这可能会导致达不到最佳的路由决策进行您的网络内。 因此，您可能会遇到达不到最佳的连接体验到不同的服务。 

Microsoft 将标记通过公共对等广告前缀和前缀承载于 Microsoft 替换相应的 BGP 社区值，该值指示该区域对等。 您可以依靠社区值需要作出适当的路由选择，提供[给客户的最佳路由](expressroute-optimize-routing.md)。

| **地缘政治区域** | **Microsoft Azure 区域** | **BGP 社区值** |
|---|---|---|
| **北美地区** |    |  |
|    | 东亚的美国 | 12076:51004 |
|    | 东亚美国 2 | 12076:51005 |
|    | 美国西部 | 12076:51006 |
|    | 西美国 2 | 12076:51026 |
|    | 美国西中部 | 12076:51027 |
|    | 美国中北部 | 12076:51007 |
|    | 美国中南部 | 12076:51008 |
|    | 美国中部 | 12076:51009 |
|    | 加拿大中部 | 12076:51020 |
|    | 加拿大东部 | 12076:51021 |
| **南美州** |  |  |
|    | 巴西南部 | 12076:51014 |
| **欧洲** |    |  |
|    | 北欧 | 12076:51003 |
|    | 西欧 | 12076:51002 |
| **亚太** |    |   |
|    | 东亚 | 12076:51010 |
|    | 东南亚 | 12076:51011 |
| **日本** |     |   |
|    | 日本东 | 12076:51012 |
|    | 日本西部 | 12076:51013 |
| **澳大利亚** |    |   | 
|    | 澳大利亚东部 | 12076:51015 |
|    | 澳大利亚东南部 | 12076:51016 |
| **印度** |    |   |
|    | 印度南部 | 12076:51019 |
|    | 印度西 | 12076:51018 |
|    | 印度中央 | 12076:51017 |

从 Microsoft 公布的所有路由将都加适当的社区价值。 

>[AZURE.IMPORTANT] 全局前缀将标记有适当社区值，并仅当启用 ExpressRoute 高级加载项时将公布。


另外，Microsoft 还将标签基于的服务所属的前缀。 这仅适用于 Microsoft 对等。 下表提供了对 BGP 社区价值的服务的映射。

| **服务** | **BGP 社区值** |
|---|---|
| **交换** | 12076:5010 |
| **SharePoint** | 12076:5020 |
| **Skype 的业务** | 12076:5030 |
| **CRM 联机** | 12076:5040 |
| **Office 365 中的其他服务** | 12076:5100 |

>[AZURE.NOTE] Microsoft 不服从您向 Microsoft 公布的路由设置的任何 BGP 社区值。

## <a name="next-steps"></a>下一步行动

- ExpressRoute 连接配置。

    - [创建传统的部署模型为 ExpressRoute 电路](expressroute-howto-circuit-classic.md)或[创建和修改使用 Azure 资源管理器 ExpressRoute 电路](expressroute-howto-circuit-arm.md)
    - [典型的部署模式的路由配置](expressroute-howto-routing-classic.md)或[资源管理器部署模型的路由的配置](expressroute-howto-routing-arm.md)
    - [链接到 ExpressRoute 电路经典 VNet](expressroute-howto-linkvnet-classic.md)或[链接到 ExpressRoute 电路资源管理器 VNet](expressroute-howto-linkvnet-arm.md)


