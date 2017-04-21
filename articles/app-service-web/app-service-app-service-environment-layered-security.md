<properties 
    pageTitle="分层的安全体系结构与应用程序服务环境" 
    description="实施分层的安全体系结构与应用程序服务环境。" 
    services="app-service" 
    documentationCenter="" 
    authors="stefsch" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/30/2016" 
    ms.author="stefsch"/>   

# <a name="implementing-a-layered-security-architecture-with-app-service-environments"></a>实施分层的安全体系结构与应用程序服务环境

## <a name="overview"></a>概述 ##
 
因为应用程序服务环境提供了部署到虚拟网络独立的运行时环境，开发人员可以创建分层的安全体系结构，每个物理应用层提供不同级别的网络访问。

共同愿望是隐藏 API 后端从一般的 Internet 访问权限，并只允许由上游 web 应用程序调用的 Api。  [网络安全组 (NSGs)] [NetworkSecurityGroups]可以使用子网包含的应用程序服务环境限制公共访问的 API 的应用程序。

下图显示示例体系结构与应用程序服务环境中部署基于的 WebAPI 应用程序。  三个单独的 web 应用程序实例，部署在三个单独的应用程序服务环境，使到相同的 WebAPI 应用程序的后端调用。

![概念性体系结构][ConceptualArchitecture] 

绿色的加号表示包含"apiase"的子网的网络安全组允许从自身以及调用从上游 web 应用程序的入站的调用。  但是同一个网络安全组明确拒绝对常规的入站通信从互联网的访问。 

本主题的其余部分遍历包含"apiase"的子网配置的网络安全组所需的步骤。

## <a name="determining-the-network-behavior"></a>确定网络行为 ##
为了解需要何种网络安全规则，您需要确定哪些网络客户端可以访问包含 API 的应用程序，该应用程序服务环境以及哪些客户端将被阻止。

由于[网络安全组 (NSGs)] [NetworkSecurityGroups]应用于子网，并且为子网部署的应用程序服务环境，NSG 中所包含的规则适用于在应用程序服务环境上运行的**所有**应用程序。  使用示例体系结构这篇文章，网络安全组应用到子网包含"apiase"后，将由相同的安全规则集保护"apiase"的应用程序服务环境上运行的所有应用程序。 

- **确定上游调用方的出站 IP 地址︰** IP 地址或上游调用方的地址是什么？  这些地址将需要 NSG 的显式允许访问。  由于调用应用程序服务环境之间被认为是"Internet"调用，这意味着出站 IP 地址分配给每个三个的上游应用程序服务环境需要 NSG 的"apiase"子网中允许访问。   有关确定出站 IP 地址有关的应用程序服务环境中运行的应用程序，请参阅[网络体系结构]的详细[NetworkArchitecture]概述文章。
- **后端 API 应用程序需要调用其本身？**  有时被忽视和细微点是后端应用程序需要调用其本身的方案。  如果应用程序服务环境中的后端 API 应用程序需要调用其本身，这被视为与"Internet"调用。  在示例体系结构中，这需要允许从"apiase"的应用程序服务环境同样的出站 IP 地址的访问。

## <a name="setting-up-the-network-security-group"></a>设置网络安全组 ##
后组的出站 IP 地址已知的下, 一步是构建网络安全组。  对于这两个资源管理器基于虚拟网络，以及传统的虚拟网络，则可以创建网络安全组。  下面的示例显示创建和配置上经典的虚拟网络，使用 Powershell NSG。

对于示例体系结构中，环境位于南部美国中部，因此要在该区域中创建空的 NSG:

    New-AzureNetworkSecurityGroup -Name "RestrictBackendApi" -Location "South Central US" -Label "Only allow web frontend and loopback traffic"

首先显式允许规则添加 Azure 的管理基础结构上[的入站的通信]的文章中所述的[InboundTraffic]的应用程序服务环境。

    #Open ports for access by Azure management infrastructure
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW AzureMngmt" -Type Inbound -Priority 100 -Action Allow -SourceAddressPrefix 'INTERNET' -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '454-455' -Protocol TCP
    
接下来，将添加两个规则允许 HTTP 和 HTTPS 从第一个上游应用程序服务环境 ("fe1ase") 的调用。

    #Grant access to requests from the first upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe1ase" -Type Inbound -Priority 200 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe1ase" -Type Inbound -Priority 300 -Action Allow -SourceAddressPrefix '65.52.xx.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

请刷新并重复的第二个和第三个上游应用程序服务环境 （"fe2ase"和"fe3ase"）。

    #Grant access to requests from the second upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe2ase" -Type Inbound -Priority 400 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe2ase" -Type Inbound -Priority 500 -Action Allow -SourceAddressPrefix '191.238.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP
    
    #Grant access to requests from the third upstream web front-end
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP fe3ase" -Type Inbound -Priority 600 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS fe3ase" -Type Inbound -Priority 700 -Action Allow -SourceAddressPrefix '23.98.abc.xyz'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

最后，授予访问权限后端 API 的应用程序服务环境的出站 IP 地址，以便它可以回调到自身。

    #Allow apps on the apiase environment to call back into itself
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP apiase" -Type Inbound -Priority 800 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTPS apiase" -Type Inbound -Priority 900 -Action Allow -SourceAddressPrefix '70.37.xyz.abc'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '443' -Protocol TCP

没有其他网络安全规则需要默认情况下配置因为每 NSG 有一套阻止来自 Internet 的入站的访问的默认规则。

在网络安全组中的规则的完整列表如下所示。  请注意最后一条规则，将突出显示，如何阻止入站从以外那些已被显式授予访问权限的所有调用方访问。

![NSG 配置][NSGConfiguration] 

最后一步是将 NSG 应用到子网包含"apiase"的应用程序服务环境。  

     #Apply the NSG to the backend API subnet
    Get-AzureNetworkSecurityGroup -Name "RestrictBackendApi" | Set-AzureNetworkSecurityGroupToSubnet -VirtualNetworkName 'yourvnetnamehere' -SubnetName 'API-ASE-Subnet'

与应用到子网的 NSG，只有三个上游应用程序服务环境，并包含 API 后端应用程序服务环境允许调入"apiase"环境。


## <a name="additional-links-and-information"></a>附加的链接和信息 ##
所有的文章和方式-的[自述文件中的服务应用程序环境](../app-service/app-service-app-service-environments-readme.md)中提供应用程序服务环境为。

有关[网络安全组](../virtual-network/virtual-networks-nsg.md)的信息。 

了解[出站 IP 地址][NetworkArchitecture]和应用程序服务环境。

[网络端口][InboundTraffic]使用的应用程序服务的环境。

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- LINKS -->
[NetworkSecurityGroups]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[NetworkArchitecture]:  https://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-architecture-overview/
[InboundTraffic]:  https://azure.microsoft.com/en-us/documentation/articles/app-service-app-service-environment-control-inbound-traffic/

<!-- IMAGES -->
[ConceptualArchitecture]: ./media/app-service-app-service-environment-layered-security/ConceptualArchitecture-1.png
[NSGConfiguration]:  ./media/app-service-app-service-environment-layered-security/NSGConfiguration-1.png
