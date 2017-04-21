<properties 
    pageTitle="使用快速通道的详细网络配置信息" 
    description="在虚拟的网络中运行的应用程序服务环境的详细网络配置信息连接到 ExpressRoute 电路。" 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="nirma" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/14/2016" 
    ms.author="stefsch"/>   

# <a name="network-configuration-details-for-app-service-environments-with-expressroute"></a>ExpressRoute 的应用程序服务环境的详细网络配置信息 

## <a name="overview"></a>概述 ##
客户可以连接[Azure ExpressRoute] [ExpressRoute]电路虚拟网络基础架构，从而将其内部网络扩展到 Azure。  应用程序服务环境可以创建此[虚拟网络]的子网[virtualnetwork]基础结构。  应用程序服务环境上运行的应用程序然后可以建立安全连接访问后端资源，只能通过 ExpressRoute 连接。  

可以创建一个应用程序服务环境**是**Azure 资源管理器虚拟网络，在**或**经典部署模型虚拟网络。  在 6 月的 2016年所做的最新更改，与 ASEs 可以现在还部署到虚拟网络 （即使用公用地址范围或 RFC1918 地址空间 专用地址）。 

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="required-network-connectivity"></a>所需的网络连接 ##
有网络连接应用程序服务不可能在虚拟网络连接到 ExpressRoute 最初符合的环境要求。  应用程序服务环境需要以下所有才能正常工作︰


-  这两个端口 80 和 443 上全球范围内的 Azure 存储端点出站网络连接。  这包括放置在同一区域，如应用程序服务环境，以及存储终结点位于**其他**Azure 的区域终结点。  在下列 DNS 域下的 azure 存储端点解析︰ *table.core.windows.net*、 *blob.core.windows.net*、 *queue.core.windows.net*和*file.core.windows.net*。  
-  出站网络连接端口 445 上的 Azure 文件服务。
-  出站网络连接到 Sql 数据库终结点位于同一个地区或与应用程序服务环境。  Sql 数据库终结点解析下下列域︰ *database.windows.net*。  这就需要打开端口 1433，11000-11999 和 14000 14999 访问。  有关更多详细信息请参阅[使用 Sql 数据库 V12 端口上的这篇文章](../sql-database/sql-database-develop-direct-route-ports-adonet-v12.md)。
-  出站网络连接到 Azure 管理平面终结点 （ASM 和 ARM 的终结点）。  这包括对*management.core.windows.net*和*management.azure.com*的出站连接。 
-  对*ocsp.msocsp.com*、 *mscrl.microsoft.com*和*crl.microsoft.com*的出站网络连接。  需要这些信息来支持 SSL 功能。
-  虚拟网络的 DNS 配置必须能够解析的所有终结点和早期点中提到的域。  如果不能解决这些终结点，应用程序服务环境创建尝试将会失败，和现有的应用程序服务环境将被标记为不正常。
-  与 DNS 服务器的通信需要，端口 53 上的出站访问权限。
-  如果在 VPN 网关的另一端有一个自定义的 DNS 服务器，DNS 服务器必须从包含应用程序服务环境的子网接通。 
-  出站网络路径不能通过内部公司代理，也不能强制隧道传输到内部。  这样做出站网络通信的有效 NAT 的地址将从应用程序的服务环境。  更改 NAT 地址的出站网络通信的应用程序服务环境，将导致很多上面列出的终结点的连接故障。  这会导致应用程序服务环境创建失败，以及以前正常的应用程序服务的环境被标记为不正常。  
-  本[文章]中所述，必须允许应用程序服务环境入站网络访问所需的端口[requiredports]。

通过确保有效的 DNS 基础结构配置和虚拟网络的维护，可以满足 DNS 要求。  如果出于任何原因的 DNS 配置已更改在创建应用程序服务环境后，开发人员可以强制要领取新的 DNS 配置应用程序服务环境。  触发滚动环境重新启动使用"重新启动"图标位于顶部在[Azure 的门户]应用程序服务环境管理 blade 的[NewPortal]将会导致环境要领取新的 DNS 配置。

可以通过配置[网络安全组]满足的入站的网络访问要求[NetworkSecurityGroups]应用程序服务环境的子网，以允许所需的访问，此[文章]中所述[requiredports]。

## <a name="enabling-outbound-network-connectivity-for-an-app-service-environment"></a>启用出站网络连接的应用程序服务环境##
默认情况下，新创建的 ExpressRoute 电路公布允许出站 Internet 连接的默认路由。  使用此配置应用程序服务环境将能够连接到其他 Azure 的终结点。

然而常见的客户配置是定义强制出站 Internet 通讯改为流动内部自己默认路由 (0.0.0.0/0)。  由于出站通信量或者阻止的内部，或 NAT 将不再可用于各种 Azure 的终结点的地址无法识别套，此通信流总是中断应用程序服务的环境。

解决方法是定义一个 （或多个） 的用户定义路由 (UDRs) 位于子网包含应用程序的服务环境。  UDR 定义将起作用而不是默认工艺路线的特定子网的路由。

如果可能，建议使用下列配置︰

- ExpressRoute 配置公布 0.0.0.0/0，默认情况下强制隧道所有出站通信内部。
- 应用于包含应用程序服务环境的子网 UDR 定义 0.0.0.0/0 与 Internet （此操作的示例是本文中下更大） 的下一跃点类型。

这些步骤的组合的效果是︰ 子网级 UDR 将优先于强制隧道，ExpressRoute 从而确保应用程序服务环境中的出站 Internet 访问权限。

> [AZURE.IMPORTANT] 在 UDR**必须**定义的路由应足够具体，优先于任何公布的 ExpressRoute 配置的路由。  下面的示例使用了广义 0.0.0.0/0 的地址范围，并因此可以有可能意外地被使用更具体的地址范围的路由公布。
>
>应用程序服务环境不是使用 ExpressRoute 配置支持**跨公布到专用路径等公共对等通道中的路由**。  已配置，公共对等的 ExpressRoute 配置将从 Microsoft 收到大量 Microsoft Azure IP 地址范围的路由公布。  如果这些地址范围上专用的对等路径交叉公布，最终结果是应用程序服务环境的子网中的所有出站网络数据包将强制隧道传输到客户的内部网络基础结构。  与应用程序服务环境当前不支持此网络流。  解决此问题的一个方法是停止公共对等通道中的跨广告路由到专用路径对等。

用户定义的路由的背景信息是在本[概述]中可用[UDROverview]。  

创建和配置用户定义工艺路线的详细信息可用于该[如何对手册][UDRHowTo]。

## <a name="example-udr-configuration-for-an-app-service-environment"></a>若要为应用程序服务环境的的 UDR 配置示例 ##

**必备组件**

1. 从[Azure 下载页]安装 Azure Powershell[ AzureDownloads] （日期为 6 月 2015年或更高版本）。  在"命令行工具"下，没有在"Windows Powershell"将安装最新的 Powershell cmdlet 下"安装"链接。

2. 建议应用程序服务环境，唯一的子网创建供独占使用。  这样可确保应用到的子网 UDRs 为应用程序服务环境将仅打开出站通信。
3. **重要提示**︰ 不**后**遵循下列配置步骤直到部署应用程序的服务环境。  这样可确保在部署的应用程序服务环境之前出站网络连接时可用。

**步骤 1︰ 创建一个命名的路由表**

下面的代码段创建一个西美国 Azure 地区称为"DirectInternetRouteTable"的路由表︰

    New-AzureRouteTable -Name 'DirectInternetRouteTable' -Location uswest

**步骤 2︰ 在路由表中创建一个或多个路由**

您将需要将一个或多个路由添加到路由表中，以启用出站 Internet 访问权限。  

推荐用于配置出站 Internet 访问权限的方法是定义如下所示的 0.0.0.0/0 的路由。
  
    Get-AzureRouteTable -Name 'DirectInternetRouteTable' | Set-AzureRoute -RouteName 'Direct Internet Range 0' -AddressPrefix 0.0.0.0/0 -NextHopType Internet

请记住该 0.0.0.0/0 是一个广泛的地址范围，并因此将重写通过由 ExpressRoute 公布的更多特定地址范围。  重新循环的早期建议，应仅广播以及 0.0.0.0/0 ExressRoute 配置与配合使用 UDR 0.0.0.0/0 的路由。 

作为一种替代方法，您可以下载使用 Azure 的 CIDR 范围的综合和更新列表。  Xml 文件包含所有的 Azure IP 地址范围可从[Microsoft 下载中心获取][DownloadCenterAddressRanges]。  

但是请注意，这些范围随时间而变化，因此也需要定期手动更新的用户定义路由保持同步。  此外，由于没有默认值上限为 100 个路由中单个的 UDR，将需要"汇总"的 Azure IP 地址范围以适应 100 路由限制，由您 ExpressRoute 牢记 UDR 定义路由需要有更明确的路由公布。  


**步骤 3︰ 将包含应用程序服务环境的子网路由表相关联**

最后一个配置步骤是相关联的子网路由表将在其中部署应用程序的服务环境。  以下命令将关联到最终将包含应用程序服务环境"ASESubnet""DirectInternetRouteTable"。

    Set-AzureSubnetRouteTable -VirtualNetworkName 'YourVirtualNetworkNameHere' -SubnetName 'ASESubnet' -RouteTableName 'DirectInternetRouteTable'


**步骤 4︰ 最后步骤**

一旦在路由表绑定到子网，则建议先测试并确认预期的效果。  例如，在子网中部署虚拟机，并确认︰


- 出站通信到 Azure 和非 Azure 终结点前面提到在这篇文章**不**流动下 ExpressRoute 电路。  它是非常重要，以验证这种现象，因为如果出站通信子网是仍然强制隧道内部，应用程序将始终无法创建的服务环境。 
- 为终结点的 DNS 查找中前面提到的所有正确解析。 

一旦确认上述步骤，您需要删除虚拟机，因为网需要在创建应用程序服务环境的时间是"空"。
 
创建一个应用程序的服务环境，然后继续 ！

## <a name="getting-started"></a>入门教程
所有的文章和方式-的[自述文件中的服务应用程序环境](../app-service/app-service-app-service-environments-readme.md)中提供应用程序服务环境为。

若要开始使用应用程序服务的环境，请参见[应用程序服务环境介绍][IntroToAppServiceEnvironment]

在 Azure 应用程序服务平台有关的详细信息，请参阅[Azure 应用程序服务][AzureAppService]。

<!-- LINKS -->
[virtualnetwork]: http://azure.microsoft.com/services/virtual-network/
[ExpressRoute]: http://azure.microsoft.com/services/expressroute/
[requiredports]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[NetworkSecurityGroups]: http://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[UDROverview]: http://azure.microsoft.com/documentation/articles/virtual-networks-udr-overview/
[UDRHowTo]: http://azure.microsoft.com/documentation/articles/virtual-networks-udr-how-to/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[AzureDownloads]: http://azure.microsoft.com/en-us/downloads/ 
[DownloadCenterAddressRanges]: http://www.microsoft.com/download/details.aspx?id=41653  
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[NewPortal]:  https://portal.azure.com
 

<!-- IMAGES -->
