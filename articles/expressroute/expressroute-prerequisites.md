<properties
   pageTitle="ExpressRoute 采用的系统必备组件 |Microsoft Azure"
   description="此页列出了您可以订购 Azure ExpressRoute 电路之前必须满足的要求。"
   documentationCenter="na"
   services="expressroute"
   authors="cherylmc"
   manager="carmonm"
   editor=""/>
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/10/2016"
   ms.author="cherylmc"/>


# <a name="expressroute-prerequisites--checklist"></a>ExpressRoute 系统必备组件和核对清单  

要连接到 Microsoft 云服务使用 ExpressRoute，您需要验证在以下各节中列出的以下要求已得到满足。

[AZURE.INCLUDE [expressroute-office365-include](../../includes/expressroute-office365-include.md)]

## <a name="azure-account"></a>Azure 帐户

- 一个有效且活动的 Microsoft Azure 帐户。 这是安装 ExpressRoute 电路所必需的。 ExpressRoute 电路是 Azure 订阅中的资源。 即使连接被限制到 Azure Microsoft 云服务，例如 Office 365 提供服务和在线 CRM，Azure 的订阅是一个要求。
- 有效的 Office 365 订阅 （如果使用 Office 365 提供服务）。 [Office 365 的特定要求](#office-365-specific-requirements)参阅这篇文章的详细信息。

## <a name="connectivity-provider"></a>连接提供程序
- 您可以使用[ExpressRoute 连接合作伙伴](expressroute-locations.md#partners)连接到 Microsoft 云。 您可以设置[以下三种](expressroute-introduction.md#howtoconnect)方式您的内部网络和 Microsoft 之间的连接。 
- 如果您的提供程序不是一个 ExpressRoute 连接合作伙伴，仍可以连接到 Microsoft 云通过[云 exchange 提供商](expressroute-locations.md#nonpartners)。

## <a name="network-requirements"></a>网络要求
- **冗余的连接性**︰ 在您和您的提供商之间的物理连接上没有冗余要求。 Microsoft 需要冗余的 BGP 会话设置 Microsoft 的路由器之间的对等的路由器，即使已经不仅仅是[一个物理连接到云交换](expressroute-faqs.md#onep2plink)。 
- **路由**︰ 这取决于如何连接到 Microsoft 云，您或您的提供商需要设置和管理[路由域](expressroute-circuit-peerings.md)的 BGP 会话。 一些以太网的连接提供程序或云 exchange 提供商可能会提供作为 BGP 管理增值服务。
- **NAT**: Microsoft 只接受通过 Microsoft 对等的公用 IP 地址。 如果您在内部网络中使用专用的 IP 地址，您或您提供程序不必翻译到公用 IP 专用的 IP 地址将解决[使用 NAT](expressroute-nat.md)。
- **QoS**: Skype 的业务需要的各种服务 （例如语音、 视频、 文本） 与众不同的 QoS 治疗。 您和您的供应商应按照[服务质量要求](expressroute-qos.md)。
- **网络安全**︰ 连接到 Microsoft 云通过 ExpressRoute 时，应考虑[网络安全](../best-practices-network-security.md)。
 
## <a name="office-365"></a>Office 365

如果您打算启用 Office 365 ExpressRoute 上，请查看以下文档更多有关 Office 365 提供要求的信息。


- [Office 365 的 ExpressRoute 的概述](https://support.office.com/en-us/article/Azure-ExpressRoute-for-Office-365-6d2534a2-c19c-4a99-be5e-33a0cee5d3bd)
- [Office 365 的 ExpressRoute 与路由](https://support.office.com/en-us/article/Routing-with-ExpressRoute-for-Office-365-e1da26c6-2d39-4379-af6f-4da213218408)
- [Office 365 的 Url 和 IP 地址范围](https://support.office.com/en-us/article/Office-365-URLs-and-IP-address-ranges-8548a211-3fe7-47cb-abb1-355ea5aa88a2)
- [网络规划和性能调优对于 Office 365](https://support.office.com/en-us/article/Network-planning-and-performance-tuning-for-Office-365-e5f1228c-da3c-4654-bf16-d163daee8848)
- [网络带宽计算器和工具](https://support.office.com/en-us/article/Network-and-migration-planning-for-Office-365-f5ee6c33-bcd7-4b0b-b0f8-dc1d9fb8d132)
- [Office 365 与内部环境的集成](https://support.office.com/en-us/article/Office-365-integration-with-on-premises-environments-263faf8d-aa21-428b-aed3-2021837a4b65)

## <a name="crm-online"></a>CRM 联机 
如果打算使 CRM Online 上 ExpressRoute，请查看以下文档有关 CRM Online 的详细信息

- [CRM 联机 Url](https://support.microsoft.com/kb/2655102)和[IP 地址范围](https://support.microsoft.com/kb/2728473)

## <a name="next-steps"></a>下一步行动

- 有关 ExpressRoute 的详细信息，请参阅[ExpressRoute 的常见问题解答](expressroute-faqs.md)。
- 发现 ExpressRoute 连接提供程序。 请参阅[ExpressRoute 合作伙伴和对等的位置](expressroute-locations.md)。
- 请参阅[路由](expressroute-routing.md)、 [NAT](expressroute-nat.md)和[QoS](expressroute-qos.md)要求。
- ExpressRoute 连接配置。
    - [创建 ExpressRoute 电路](expressroute-howto-circuit-classic.md)
    - [配置路由](expressroute-howto-routing-classic.md)
    - [链接到 ExpressRoute 电路的 VNet](expressroute-howto-linkvnet-classic.md)

