<properties
   pageTitle="ExpressRoute 位置 |Microsoft Azure"
   description="本文提供了提供服务的以及如何连接到 Azure 的区域的位置的详细的概述。"
   services="expressroute"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor="" />
<tags
   ms.service="expressroute"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/21/2016"
   ms.author="cherylmc" />

# <a name="expressroute-partners-and-peering-locations"></a>ExpressRoute 合作伙伴和对等的位置

本文中的表提供有关连接提供 ExpressRoute ExpressRoute 地理覆盖范围，支持 ExpressRoute，和 ExpressRoute 系统集成商 (Si) Microsoft 云服务的信息。

## <a name="partners"></a>ExpressRoute 连接提供程序

ExpressRoute 支持所有 Azure 的区域和位置。 下面的图列出了 Azure 地区和 ExpressRoute 的位置。 ExpressRoute 的位置是指那些在 Microsoft 同级与多个服务提供商。

![位置映射][0]

如果您连接到的地缘政治区域中至少一个 ExpressRoute 位置的情况下，将所有区域内的地缘政治区域间有权 Azure 服务。 下表提供了 Azure 地区的地缘政治区域内的 ExpressRoute 位置地图。

|**地缘政治区域**|**Azure 的区域**|**ExpressRoute 的位置**|
|---|---|---|
|**北美地区**|美国东、 西美国、 东亚美国 2、 美国中部、 美国中南部、 美国中北部、 加拿大主办，加拿大东部|亚特兰大、 芝加哥、 达拉斯、 拉斯维加斯、 洛杉矶、 纽约、 西雅图、 硅谷，华盛顿哥伦比亚特区、 蒙特利尔 +、 魁北克城市 +、 多伦多|
|**南美州**|巴西南部|圣保罗|
|**欧洲**|北美，西欧，英国西欧，英国南部|阿姆斯特丹、 都柏林、 伦敦、 Newport(Wales) +、 巴黎|
|**亚洲**|东亚东南亚|香港、 新加坡|
|**日本**|日本西部，东日本|大阪东京|
|**澳大利亚**|澳大利亚东南部，澳大利亚东部|墨尔本悉尼|
|**印度**|印度西部，印度中部，印度南|马德拉斯孟买|



下表提供国内云的有关区域和地缘政治边界信息。

|**地缘政治区域**|**Azure 的区域**|**ExpressRoute 的位置**|
|---|---|---|---|
|**美国政府云**|美国 Gov 弗吉尼亚 Gov 爱荷华州美国|芝加哥，达拉斯，纽约，华盛顿哥伦比亚特区|
|**中国**|中国北方，中国东部|北京上海|
|**德国**|德国中部，德国东部|柏林法兰克福|


标准的 ExpressRoute SKU 不支持连接跨地缘政治区域。 您将需要启用 ExpressRoute 高级加载项支持全球联网。 不支持连接到全国的云环境。 如果出现这样的需求，您可以使用您的连接的提供程序。


## <a name="connectivity-provider-locations"></a>连接提供程序位置

> [AZURE.SELECTOR]
[位置提供程序](expressroute-locations.md#connectivity-provider-locations)
[按位置提供程序](expressroute-locations-providers.md#connectivity-provider-locations)

### <a name="production-azure"></a>生产 Azure
| **位置**  | **服务提供商** |
|---------------|-----------------------|
| **阿姆斯特丹** | Aryaka 网络，AT & T NetBond、 英国电信、 Colt、 Equinix、 euNetworks，GÉANT、 InterCloud、 互联网解决方案-云连接，Interxion，第 3 级的通信、 橙色、 Tata 的通信，TeleCity 组 Telenor，Verizon |
| **亚特兰大** | Equinix |
| **马德拉斯** | Tata 通信 |
| **芝加哥** | AT & T NetBond、 Comcast、 Equinix、 3 级的通信，Zayo 组 |
| **达拉斯** | AT & T NetBond、 Cologix、 Equinix，级别 3 的通信，Megaport |
| **都柏林** | Colt、 Telecity 组 |
| **中国香港特别行政区** | 英国电信，中国电信全球、 Equinix、 Megaport、 橙色、 全局 PCCW 有限，Tata 通信 Verizon |
| **伦敦** | AT & T NetBond、 英国电信、 Colt、 Equinix、 InterCloud、 互联网解决方案-云连接，Interxion，Jisc，3 级通信、 车、 NTT 通信，橙色，Tata 的通信，Telecity 组 Telenor，Verizon，Vodafone |
| **拉斯维加斯** | 第 3 级通信 + Megaport
| **洛杉矶** | CoreSite、 Equinix、 Megaport，NTT，Zayo 组 |
| **墨尔本** | AARNet，Equinix，Megaport，NEXTDC，Telstra 公司 |
| **纽约** | Equinix、 Megaport、 Zayo 组 |
| **Newport(Wales) +** | 下一代数据 + |
| **蒙特利尔** | Cologix + |
| **孟买** | Tata 通信 |
| **大阪** | Equinix，互联网计划日本 Inc.的 IIJ，NTT 通信、 Softbank |
| **巴黎** | Interxion，Equinix + |
| **圣保罗** | Equinix Telefonica |
| **西雅图** | Equinix，第 3 级的通信，Megaport |
| **硅谷** | Aryaka 网络，AT & T NetBond、 英国电信、 CenturyLink +、 Comcast、 Equinix、 3 级的通信、 橙色、 Tata 通信，Verizon，Zayo 组 |
| **新加坡** | Aryaka 网络，AT & T NetBond、 英国电信、 Equinix、 InterCloud、 Megaport、 橙色、 新加坡电信、 Tata 通信、 Verizon |
| **悉尼** | AARNet，AT & T NetBond，英国电信，Equinix，Megaport，NEXTDC，橙色，Telstra 公司、 Verizon |
| **东京** | Aryaka 网络，英国电信，Colt，Equinix，互联网计划日本 Inc.的 IIJ，NTT 通信、 Softbank，Verizon |
| **多伦多** | Cologix、 Equinix、 Zayo 组 |
| **华盛顿哥伦比亚特区** | Aryaka 网络，AT & T NetBond、 英国电信、 Comcast、 Equinix、 InterCloud、 3 级的通信，Megaport，橙色，Tata 通信，Verizon，Zayo 组 |

 **+**表示即将上市

### <a name="national-cloud-environments"></a>全国云环境

#### <a name="us-government-cloud"></a>美国政府云

| **位置**  |**服务提供商** |
|---------------|--------------------|
| **芝加哥** | AT & T NetBond，Equinix，级别 3 通讯中，Verizon |
| **达拉斯** |  Equinix，Verizon + |
| **纽约** | Equinix，3 级通信 +，Verizon |
| **华盛顿哥伦比亚特区** | AT & T NetBond，Equinix，级别 3 通讯中，Verizon |

#### <a name="china"></a>中国

| **位置**  | **服务提供商** |
|---------------|-----------------------|
| **北京** | 中国电信 |
| **上海** |  中国电信 |
若要了解详细信息，请参阅[ExpressRoute 在中国](http://www.windowsazure.cn/home/features/expressroute/)

#### <a name="germany"></a>德国

| **位置**  | **服务提供商** |
|---------------|-----------------------|
| **柏林** | Colt + e shelter |
| **法兰克福** | Colt--Equinix，Interxion |

## <a name="nonpartners"></a>通过未列出的服务提供商的连接

如果前面各节中未列出您的连接的提供程序，您仍然可以创建连接。

- 与连接提供商联系以查看是否在连接到在上表中交换的任何检查。 您可以检查以下链接以收集有关 exchange 提供程序所提供服务的详细信息。 几个连接提供程序已连接到以太网交换。

    - [Equinix 云交换](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
    - [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
    - [InterXion](http://www.interxion.com/)
    - [NextDC](http://www.nextdc.com/)
    - [CoreSite](http://www.coresite.com/)
    - [Cologix](http://www.cologix.com/)
- 具有扩展您选择的对等位置的网络连接提供程序。
    - 确保您连接的提供程序以高可用性方式扩展您的连接，以便有没有单点故障。
- 为连接提供程序连接到 Microsoft 订购通过交换 ExpressRoute 电路。
    - 按照步骤中[创建的 ExpressRoute 电路](expressroute-howto-circuit-classic.md)以建立连接。

|**位置**|**交换**|**连接提供程序**|
|-------------|------------|-------------------------|
| **纽约** | Equinix | Lightower |
| **西雅图** | Equinix | 阿拉斯加的通信 |
| **硅谷** | Equinix | XO 通信 |
| **新加坡** | Equinix | 1CLOUDSTAR |
| **华盛顿哥伦比亚特区** | Equinix | Lightower |

## <a name="expressroute-system-integrators"></a>ExpressRoute 系统集成商

启用专用的连接性，可满足您的需要是网络的富有挑战性的取决于您的规模。 您可以使用任何列入下表可帮助您对 ExpressRoute 的服务与系统集成商。

|**大陆**|**系统集成商**|
|-------------|---------------------|
| **亚洲** | Avanade Inc.OneAs1a|
| **欧洲** | Avanade Inc.，最低的解决方案|
| **我们** | Avanade Inc.、 Equinix 专业服务、 Perficient、 项目领导|

## <a name="next-steps"></a>下一步行动

- 有关 ExpressRoute 的详细信息，请参阅[ExpressRoute 的常见问题解答](expressroute-faqs.md)。
- 确保满足所有先决条件。 [ExpressRoute 系统必备组件](expressroute-prerequisites.md)，请参阅。

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "位置映射"
