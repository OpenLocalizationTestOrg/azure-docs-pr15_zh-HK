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

| **服务提供商**  |**Microsoft Azure** | **Office 365 和在线 CRM** | **位置** |
|-----------------------|--------------------|----------------|---------------|
| **AARNet** | 支持 | 支持 | 墨尔本悉尼 |
| **[Aryaka 网络]( http://www.aryaka.com/)** | 支持 | 支持 | 阿姆斯特丹，硅谷、 新加坡东京，华盛顿哥伦比亚特区 |
| **[AT & T NetBond]( https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** | 支持 | 支持 | 阿姆斯特丹、 芝加哥、 达拉斯、 伦敦、 硅谷、 新加坡、 悉尼，华盛顿哥伦比亚特区 |
| **[英国电信]( http://www.globalservices.bt.com/uk/en/news/bt_to_provide_connectivity_to_microsoft_azure)** | 支持 | 支持 | 阿姆斯特丹、 香港、 伦敦、 硅谷、 新加坡、 悉尼、 东京，华盛顿哥伦比亚特区 |
|**CenturyLink** | 即将推出 | 即将推出| 硅谷 |
|**中国电信全球** | 支持 | 不受支持 | 中国香港特别行政区 |
|**[Cologix](http://www.cologix.com/solutions/cloud-connect/public-clouds/microsoft-cloud/)** | 支持 | 即将推出 | 达拉斯，蒙特利尔 + 多伦多 |
| **[Colt]( http://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)**  |  支持 | 支持 | 阿姆斯特丹，都柏林，伦敦东京 |
| **Comcast** | 支持 | 支持 | 芝加哥，硅谷，华盛顿哥伦比亚特区 |
| **[CoreSite](http://www.coresite.com/solutions/cloud-services/public-cloud-providers/microsoft-azure-expressroute)** | 支持 | 支持 | 洛杉矶 | 
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** | 支持 | 支持 | 阿姆斯特丹、 亚特兰大、 芝加哥、 达拉斯、 香港、 伦敦、 洛杉矶、 墨尔本、 纽约、 大阪、 巴黎 +、 圣保罗、 西雅图、 硅谷、 新加坡、 悉尼、 东京、 多伦多，华盛顿哥伦比亚特区 |
| **euNetworks** |  支持 | 支持 | 阿姆斯特丹 |
| **GÉANT** | 支持 | 支持 | 阿姆斯特丹 |
| **[互联网计划日本 Inc.IIJ](http://www.iij.ad.jp/en/news/pressrelease/2015/1216-2.html)** |  支持 | 支持 | 大阪东京 |
| **[InterCloud]( https://www.intercloud.com/)** | 支持 | 支持 | 阿姆斯特丹，伦敦、 新加坡，华盛顿哥伦比亚特区 |
| **互联网解决方案-云连接** | 支持 | 支持 | 阿姆斯特丹伦敦 |
| **[Interxion](http://www.interxion.com/why-interxion/colocate-with-the-clouds/colocated-hybrid-cloud/microsoft-azure/)**  | 支持 | 支持 | 阿姆斯特丹，伦敦，巴黎 |
| **Jisc** | 支持 | 支持 | 伦敦 | 
| **[3 级的通信]( http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** | 支持 | 支持 | 阿姆斯特丹、 芝加哥、 达拉斯、 拉斯维加斯 +、 伦敦、 西雅图、 硅谷，华盛顿哥伦比亚特区 |
| **Megaport** | 支持 | 支持 | 达拉斯，香港，拉斯维加斯举行，洛杉矶，墨尔本，New York，西雅图、 新加坡悉尼，华盛顿哥伦比亚特区 |
| **车** | 支持 | 支持 | 伦敦 |
| **下一代数据** | 即将推出 | 即将推出 | Newport(Wales) + |
| **NEXTDC** | 支持 | 支持 | 墨尔本悉尼 |
| **NTT 通讯** | 支持 | 支持 | 伦敦，洛杉矶，大阪东京 |
| **[橙色]( http://www.orange-business.com/en/products/business-vpn-galerie)** | 支持 | 支持 | 阿姆斯特丹，香港，伦敦，硅谷、 新加坡悉尼，华盛顿哥伦比亚特区 |
| **全球 PCCW 有限** | 支持 | 支持 | 中国香港特别行政区 |
| **[新加坡电信]( http://info.singtel.com/about-us/news-releases/singtel-provide-secure-private-access-microsoft-azure-public-cloud)** |  支持 | 支持 | 新加坡 |
| **Softbank** | 支持 | 支持 | 大阪东京 | 
| **[Tata 通信](http://www.tatacommunications.com/lp/izo/azure/azure_index.html)** | 支持 | 支持 | 阿姆斯特丹，马德拉斯，香港特别行政区，伦敦，孟买，硅谷、 新加坡，华盛顿哥伦比亚特区 |
| **[TeleCity 组]( http://www.telecitygroup.com/investor-centre/news_details.htm?locid=03100500400b00d&xml)** | 支持 | 支持 | 阿姆斯特丹，都柏林，伦敦 |
| **Telefonica** | 支持 | 支持 | 圣保罗 |
| **Telenor** | 支持 | 支持 | 阿姆斯特丹伦敦 |
| **[Telstra 公司]( http://www.telstra.com.au/business-enterprise/network-services/networks/cloud-direct-connect/)** | 支持 | 支持 | 墨尔本悉尼 |
| **[Verizon](http://www.verizonenterprise.com/products/networking/secure-cloud-interconnect/)** | 支持 | 支持 | 阿姆斯特丹、 香港、 伦敦、 硅谷、 新加坡、 悉尼、 东京，华盛顿哥伦比亚特区 |
| **Vodafone** | 支持 | 不受支持 | 伦敦 | 
| **[Zayo 组]( http://www.zayo.com/solutions/industries/connect-to-cloud-data-centers/cloud-connectivity/microsoft-expressroute/)** | 支持 | 支持 | 芝加哥，洛杉矶，纽约，硅谷，多伦多，华盛顿哥伦比亚特区 |

 **+**表示即将上市

### <a name="national-cloud-environments"></a>全国云环境

#### <a name="us-government-cloud"></a>美国政府云

| **服务提供商**  |**Microsoft Azure** | **Office 365** | **位置** |
|-----------------------|--------------------|----------------|---------------|
| **[AT & T NetBond]( https://www.synaptic.att.com/clouduser/html/productdetail/ATT_NetBond.htm)** | 支持 | 支持 | 芝加哥，华盛顿哥伦比亚特区 |
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** | 支持 | 支持 | 芝加哥，达拉斯，纽约，华盛顿哥伦比亚特区 |
| **[3 级的通信]( http://your.level3.com/LP=882?WT.tsrc=02192014LP882AzureVanityAzureText)** | 支持 | 支持 | 芝加哥，纽约 +，华盛顿哥伦比亚特区 |
| **[Verizon](http://news.verizonenterprise.com/2014/04/secure-cloud-interconnect-solutions-enterprise/)** | 支持 | 支持 | 芝加哥，达拉斯 +，纽约，华盛顿哥伦比亚特区 |

#### <a name="china"></a>中国

| **服务提供商**  |**Microsoft Azure** | **Office 365** | **位置** |
|-----------------------|--------------------|----------------|---------------|
| **中国电信** | 支持 | 不受支持 | 北京上海|
若要了解详细信息，请参阅[ExpressRoute 在中国](http://www.windowsazure.cn/home/features/expressroute/)。

#### <a name="germany"></a>德国

| **服务提供商**  |**Microsoft Azure** | **Office 365** | **位置** |
|-----------------------|--------------------|----------------|---------------|
| **[Colt]( http://www.colt.net/uk/en/news/colt-announces-dedicated-cloud-access-for-microsoft-azure-services-en.htm)** | 支持 | 不受支持 | 柏林 + 法兰克福|
| **[Equinix](http://www.equinix.com/partners/microsoft-azure/)** | 支持 | 不受支持 | 法兰克福|
| **电子 shelter** | 支持 | 不受支持 | 柏林|
| **Interxion** | 支持 | 不受支持 | 法兰克福|

## <a name="nonpartners"></a>通过未列出的服务提供商的连接

如果前面各节中未列出您的连接的提供程序，您仍然可以创建连接。

- 与连接提供商联系以查看是否在连接到在上表中交换的任何检查。 您可以检查以下链接以收集有关 exchange 提供程序所提供服务的详细信息。 几个连接提供程序已连接到以太网交换。

    - [Equinix 云交换](http://www.equinix.com/services/interconnection-connectivity/cloud-exchange/)
    - [TeleCity CloudIX](http://www.telecitygroup.com/colocation-services/cloud-ix.htm)
    - [Interxion](http://www.interxion.com/why-interxion/colocate-with-the-clouds/colocated-hybrid-cloud/microsoft-azure/)
    - [NextDC](http://www.nextdc.com/)
    - [CoreSite](http://www.coresite.com/)
    - [Cologix](http://www.cologix.com/)
- 具有扩展您选择的对等位置的网络连接提供程序。
    - 确保您连接的提供程序以高可用性方式扩展您的连接，以便有没有单点故障。
- 为连接提供程序连接到 Microsoft 订购通过交换 ExpressRoute 电路。
    - 按照步骤中[创建的 ExpressRoute 电路](expressroute-howto-circuit-classic.md)以建立连接。

|**连接提供程序**|**交换**|**位置**|
|---|---|---|
|**[1CLOUDSTAR](http://www.1cloudstar.com/service/cloudconnect-azure-expressroute/)**|Equinix|新加坡|
|**阿拉斯加的通信**|Equinix|西雅图|
|**[Lightower](http://www.lightower.com/network-solutions/cloud-connect/#microsoft-azure )**|Equinix|纽约，华盛顿哥伦比亚特区|
|**[XO 通信](http://www.xo.com/)**|Equinix|硅谷|


## <a name="expressroute-system-integrators"></a>ExpressRoute 系统集成商

启用专用的连接性，可满足您的需要是网络的富有挑战性的取决于您的规模。 您可以使用任何列入下表可帮助您对 ExpressRoute 的服务与系统集成商。

|**系统集成商**|**大陆**|
|---|---|
|**[Avanade inc.保留所有权利。](http://www.avanade.com/)**| 亚洲、 欧洲、 美国 |
|**[最低的解决方案](http://www.dotnetsolutions.co.uk/)**| 欧洲 |
|**[Equinix 专业服务](http://www.equinix.com/services/consulting/)**|我们|
|**[OneAs1a](http://www.oneas1a.com/express-connect-any-cloud-ecac)** | 亚洲 |
|**[Perficient](http://www.perficient.com/Partners/Microsoft/Cloud/Azure-ExpressRoute)** | 我们 |
|**[项目领导能力](http://www.projectleadership.net/azure)** | 我们 |

## <a name="next-steps"></a>下一步行动

- 有关 ExpressRoute 的详细信息，请参阅[ExpressRoute 的常见问题解答](expressroute-faqs.md)。
- 确保满足所有先决条件。 [ExpressRoute 系统必备组件](expressroute-prerequisites.md)，请参阅。

<!--Image References-->
[0]: ./media/expressroute-locations/expressroute-locations-map.png "位置映射"
