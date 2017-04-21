<properties 
    pageTitle="如何控制对应用程序服务环境的入站的通信" 
    description="了解有关如何配置网络安全规则来控制对应用程序服务环境的入站的通信。" 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/02/2016" 
    ms.author="stefsch"/>   

# <a name="how-to-control-inbound-traffic-to-an-app-service-environment"></a>如何控制对应用程序服务环境的入站的通信

## <a name="overview"></a>概述 ##
可以创建一个应用程序服务环境**是**Azure 资源管理器虚拟网络，在**或**经典部署模型[虚拟网络][virtualnetwork]。  在创建应用程序服务环境时可以定义一个新的虚拟网络和新的子网。  或者，可以预存的虚拟网络和现有的子网中创建一个应用程序的服务环境。  在 6 月的 2016年所做的最新更改，与 ASEs 现在可以部署到虚拟网络 （即使用公用地址范围或 RFC1918 地址空间 专用地址）。  创建应用程序服务环境的更多详细信息请参阅[如何创建应用程序服务环境]为[HowToCreateAnAppServiceEnvironment]。

应用程序服务环境始终必须创建在一个子网，因为子网提供了可以用来锁定背后上游设备和服务的入站通信，HTTP 和 HTTPS 通信量仅接受来自上游的特定 IP 地址的网络边界。

使用[网络安全组]控制入站和出站网络通信子网[NetworkSecurityGroups]。 控制入站的通信，需要创建网络安全组中，网络安全规则，然后将分配网络安全组包含应用程序服务环境的子网。

一旦网络安全组分配给子网时，在应用程序服务环境中的应用程序的入站的通信被允许/阻止基于允许和拒绝网络安全组中定义的规则。

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="network-ports-used-in-an-app-service-environment"></a>在应用程序服务环境中使用的网络端口 ##
锁定通过网络安全组的入站网络通信之前, 是一定要知道的一套使用的应用程序服务环境必需和可选的网络端口。  意外关闭关闭某些端口的通信可能会导致应用程序服务环境中的功能丧失。

下面是应用程序服务环境使用的端口的列表。 所有端口都为**TCP**，除非另行明确说明︰

- 454︰**所需端口**用于管理和维护的应用程序服务环境通过 SSL 使用 Azure 的基础结构。  不会阻止与此端口的通信。  此端口始终绑定到公用 ASE 的 VIP。
- 455︰**所需端口**用于管理和维护的应用程序服务环境通过 SSL 使用 Azure 的基础结构。  不会阻止与此端口的通信。  此端口始终绑定到公用 ASE 的 VIP。
- 80︰ 默认为应用程序服务计划中的应用程序服务环境中运行的应用程序的入站 HTTP 通信的端口。  在支持 ILB 的 ASE，此端口绑定到 ASE 的 ILB 地址。
- 443︰ 默认为应用程序服务计划中的应用程序服务环境中运行的应用程序的入站 SSL 通信的端口。  在支持 ILB 的 ASE，此端口绑定到 ASE 的 ILB 地址。
- 21: FTP 的控制通道。  如果不使用 FTP，可以放心地阻止此端口。  在支持 ILB 的 ASE，此端口可以绑定到 ILB 地址的 ASE。
- 990: FTPS 的控制通道。  如果不使用 FTPS，可以放心地阻止此端口。  在支持 ILB 的 ASE，此端口可以绑定到 ILB 地址的 ASE。
- 10001 10020︰ 用于 FTP 数据信道。  控制通道与这些端口可以放心地阻止如果不使用 FTP。  在支持 ILB 的 ASE，可将此端口绑定到 ASE 的 ILB 地址。
- 4016︰ 用于远程调试使用 Visual Studio 2012。  如果不使用该功能，可以放心地阻止此端口。  在支持 ILB 的 ASE，此端口绑定到 ASE 的 ILB 地址。
- 4018︰ 用于远程调试与 Visual Studio 2013年。  如果不使用该功能，可以放心地阻止此端口。  在支持 ILB 的 ASE，此端口绑定到 ASE 的 ILB 地址。
- 4020︰ 用于远程调试与 Visual Studio 2015年。  如果不使用该功能，可以放心地阻止此端口。  在支持 ILB 的 ASE，此端口绑定到 ASE 的 ILB 地址。

## <a name="outbound-connectivity-and-dns-requirements"></a>出站连接和 DNS 要求 ##
为应用程序服务环境中能正常工作，它需要到各个端点的出站访问权限。 [网络配置的 ExpressRoute](app-service-app-service-environment-network-configuration-expressroute.md#required-network-connectivity)文章"需要网络连接"一节中为外部端点由 ASE 的完整列表。

应用程序服务环境需要有效 DNS 基础结构配置为虚拟网络。  如果出于任何原因的 DNS 配置已更改在创建应用程序服务环境后，开发人员可以强制要领取新的 DNS 配置应用程序服务环境。  触发滚动环境重新启动使用"重新启动"图标位于顶部在[Azure 的门户]应用程序服务环境管理 blade 的[NewPortal]将会导致环境要领取新的 DNS 配置。

此外建议在 vnet 上的任何自定义 DNS 服务器是安装程序创建一个应用程序服务环境之前提前。  如果虚拟网络的 DNS 配置更改时要创建的应用程序服务环境，这将导致应用程序服务环境创建过程失败。  在相同的脉络，如果自定义的 DNS 服务器位于另一端的 VPN 网关和 DNS 服务器将无法访问或不可用，应用程序服务环境创建过程也将失败。

## <a name="creating-a-network-security-group"></a>创建网络安全组 ##
在网络安全组的工作的完整详细信息，请参阅以下[信息][NetworkSecurityGroups]。  网络安全组，配置和应用网络安全组与子网包含的应用程序服务环境为重点的突出显示在触摸屏输入下面的详细信息。

**注意︰**可以使用[Azure 门户](https://portal.azure.com)以图形方式配置网络安全组或通过 Azure PowerShell。

首先，网络安全组被创建为与订阅相关的独立实体。 由于 Azure 的区域中创建网络安全组，请确保网络安全组被创建在同一区域作为应用程序服务环境。

下面演示如何创建网络安全组︰

    New-AzureNetworkSecurityGroup -Name "testNSGexample" -Location "South Central US" -Label "Example network security group for an app service environment"

创建网络安全组后，给它添加一个或多个网络安全规则。  因为随着时间的推移可能会更改的规则集，建议空间出用于规则的优先级来方便地随着时间的推移插入其他规则的编号方案。

下面的示例演示显式授予对所需的 Azure 的基础结构来管理和维护的应用程序服务环境管理端口的访问规则。  请注意，所有管理通信量通过 SSL 排列，因此即使打开端口将无法访问它们的 Azure 的管理基础结构以外的任意实体受客户端证书。


    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP
    

当锁定访问端口 80 和 443 为"隐藏"应用程序服务环境背后上游设备或服务，您需要知道的上游的 IP 地址。  例如，如果您使用的 web 应用程序防火墙 (WAF)，WAF 将有它自己的 IP 地址 （或地址） 用时代理通信流到下游应用程序服务环境。  您将需要使用此 IP 地址的网络安全规则的*SourceAddressPrefix*参数中。

在下面的示例中，是明确允许从上游的特定 IP 地址的入站的通信。  地址*1.2.3.4*上游 WAF 的 IP 地址用作占位符。  更改的值，以匹配您的上游设备或服务使用的地址。

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTP" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT HTTPS" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP
    
如果需要 FTP 支持，作为模板使用下面的规则，可以授予访问 FTP 控制端口和数据端口通道。  由于 FTP 是一种有状态协议，您可能不能穿传统的 HTTP/HTTPS 防火墙或代理设备的 FTP 通信。  在这种情况下需要将*SourceAddressPrefix*设置为一个不同的值-例如 IP 地址范围的开发人员或部署计算机运行的 FTP 客户端。 

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPCtrl" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '21' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT FTPDataRange" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '10001-10020' -Protocol TCP

(**注︰**在预览期间可能会更改数据通道端口范围。)

如果使用远程调试与 Visual Studio，则下列规则将演示如何授予访问权限。  由于每个版本使用不同的端口以进行远程调试，没有每个受支持版本的 Visual Studio 的不同规则。  与 FTP 访问，远程调试通信通过传统 WAF 或代理设备可能会不正确地排列。  相反， *SourceAddressPrefix*可以设置为运行 Visual Studio 开发人员计算机的 IP 地址范围中。

    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2012" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4016' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2013" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4018' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityRule -Name "RESTRICT RemoteDebuggingVS2015" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '1.2.3.4/32'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '4020' -Protocol TCP

## <a name="assigning-a-network-security-group-to-a-subnet"></a>分配到子网的网络安全组 ##
网络安全组已拒绝访问所有外部通信的默认安全规则。  通过组合，上面所述网络安全规则和默认安全规则阻止入站的通信，其结果是来自源地址与*允许*操作相关联的区域将能够将通信发送到应用程序的应用程序服务环境中运行的唯一通信量。

网络安全组填充了安全规则之后，需要分配给子网包含应用程序的服务环境。  工作分配命令引用的应用程序服务环境所在的虚拟网络名称以及创建应用程序服务环境所在子网的名称。  

下面的示例显示分配给子网和虚拟网络的网络安全组︰


    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

网络安全组分配成功后 （分配是长时间运行的操作，可能需要几分钟才能完成），只*允许*规则匹配的入站的通信将成功到达应用程序服务环境中的应用程序。

出于完整性的考虑下面的示例演示如何移除并因此不同关联子网的网络安全组︰


    Get-AzureNetworkSecurityGroup -Name "testNSGexample" | Remove-AzureNetworkSecurityGroupFromSubnet -VirtualNetworkName 'testVNet' -SubnetName 'Subnet-test'

## <a name="special-considerations-for-explicit-ip-ssl"></a>显式的 IP SSL 的特殊注意事项 ##
如果应用程序使用显式的 IP SSL 配置地址 （适用于仅有公用的 VIP ASEs），而不是使用默认的 IP 地址的应用程序服务环境，HTTP 和 HTTPS 通信排入子网通过一组不同的端口 80 和 443 端口以外的端口。

在门户用户界面应用程序服务环境的详细信息的用户体验刀片式服务器中找不到单独使用 IP SSL 的每个地址的端口对。  选择"所有设置"--都 >"IP 地址"。  "IP 地址"刀片式服务器显示为应用程序服务环境中，以及用来路由与 IP SSL 的每个地址关联的 HTTP 和 HTTPS 通信的特殊端口对显式配置的所有 IP SSL 地址的表。  它是需要配置网络安全组中的规则时使用的 DestinationPortRange 参数为此端口对。

当 ASE 上的应用程序被配置为使用 IP SSL 时，外部客户看不到并不需要担心的特殊端口对映射。  应用程序的通信将通常流向已配置的 SSL IP 地址。  特殊端口对的转换自动期间发生的情况内部路由通信的最后 leg 到包含 ASE 的子网。 

## <a name="getting-started"></a>入门教程

若要开始使用应用程序服务的环境，请参见[应用程序服务环境介绍][IntroToAppServiceEnvironment]

所有的文章和方式-的[自述文件中的服务应用程序环境](../app-service/app-service-app-service-environments-readme.md)中提供应用程序服务环境为。

围绕安全地连接到后端资源的应用程序服务环境中的应用程序的详细信息，请参阅[安全地连接到后端应用程序服务环境中的资源][SecurelyConnecttoBackend]

在 Azure 应用程序服务平台有关的详细信息，请参阅[Azure 应用程序服务][AzureAppService]。

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[HowToCreateAnAppServiceEnvironment]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[IntroToAppServiceEnvironment]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[SecurelyConnecttoBackend]:  http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-securely-connecting-to-backend-resources/
[NewPortal]:  https://portal.azure.com  

<!-- IMAGES -->
 
