<properties
   pageTitle="创建、 启动或使用 Azure 资源管理器中删除应用程序网关 |Microsoft Azure"
   description="此页面介绍了如何创建、 配置、 启动和使用 Azure 资源管理器中删除个 Azure 应用程序网关"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace"/>


# <a name="create-start-or-delete-an-application-gateway-by-using-azure-resource-manager"></a>创建、 启动或使用 Azure 资源管理器中删除应用程序网关

> [AZURE.SELECTOR]
- [Azure 门户](application-gateway-create-gateway-portal.md)
- [资源管理器的 azure PowerShell](application-gateway-create-gateway-arm.md)
- [Azure 的经典 PowerShell](application-gateway-create-gateway.md)
- [Azure 的资源管理器模板](application-gateway-create-gateway-arm-template.md)
- [Azure CLI](application-gateway-create-gateway-cli.md)

Azure 应用程序网关是一个 7 层负载平衡器。 无论是对云的内部，它提供了故障切换时，不同的服务器之间的路由性能的 HTTP 请求。 应用程序网关提供了许多应用程序交付控制器 (ADC) 功能，其中包括 HTTP 负载平衡、 基于 cookie 的会话相关性，安全套接字层 (SSL) 卸载自定义运行状况探测器、 支持多站点，和许多其他人。 若要查找受支持的功能的完整列表，请访问[应用程序网关概述](application-gateway-introduction.md)

这篇文章将引导您完成创建、 配置、 启动和删除应用程序网关的步骤。

>[AZURE.IMPORTANT] 使用 Azure 的资源之前，请务必了解 Azure 目前有两种部署模型︰ 资源管理器和经典。 请确保您了解[部署模型和工具](../azure-classic-rm.md)之前使用 Azure 的任何资源。 可以通过单击本文顶部的选项卡来查看不同的工具的文档。 本文档介绍使用 Azure 资源管理器中创建应用程序网关。 要使用经典版，请转到[创建应用程序网关经典部署使用 PowerShell](application-gateway-create-gateway.md)。


## <a name="before-you-begin"></a>在开始之前

1. 通过使用 Web 平台安装程序安装最新版本的 Azure PowerShell cmdlet。 您可以从下载并安装最新版本[下载网页](https://azure.microsoft.com/downloads/)的**Windows PowerShell**部分。
2. 如果您有一个现有的虚拟网络，选择现有空的子网或由应用程序网关，目的只是为了使用现有虚拟网络中创建子网。 您不能部署到要部署应用程序网关后面的资源比另一个虚拟网络的应用程序网关。
3. 被配置为使用应用程序网关服务器必须存在或已经创建了虚拟的网络中或与公用 IP/VIP 及其终结点分配。

## <a name="what-is-required-to-create-an-application-gateway"></a>必须创建应用程序网关是什么？

- **后端服务器池︰**后端服务器的 IP 地址的列表。 列出的 IP 地址既应属于虚拟子网或应当是公共 IP/VIP。
- **后端服务器池设置︰**每个池都有设置，如端口、 协议和基于 cookie 的关系等。 这些设置到池密切相关，并且应用于池中的所有服务器。
- **前端端口︰**此端口是打开的应用程序网关在公共端口。 流量达到此端口，，然后获取重定向到一台后端服务器。
- **侦听器︰**侦听器具有前端端口、 协议 （Http 或 Https，这些值是区分大小写），和 SSL 证书名称 （如果配置 SSL 卸载）。
- **规则︰**该规则绑定侦听器后, 端服务器池，并定义时将触发特定侦听器通信应定向到哪个后端服务器池。

## <a name="create-an-application-gateway"></a>创建应用程序网关

使用 Azure 经典和 Azure 资源管理器之间的区别是在其中创建应用程序网关，并需要配置项的顺序。

使用资源管理器中，使应用程序网关的所有项目都单独配置，然后在一起以创建应用程序网关资源。

以下是创建应用程序网关所需的步骤。

## <a name="create-a-resource-group-for-resource-manager"></a>创建资源组的资源管理器

请确保您使用的最新版本的 Azure PowerShell。 [使用 Windows PowerShell 与资源管理器](../powershell-azure-resource-manager.md)提供了更多的信息。

### <a name="step-1"></a>第 1 步

登录到 Azure 中

    Login-AzureRmAccount

提示您使用您的凭据进行身份验证。

### <a name="step-2"></a>第 2 步

请检查帐户订阅。

    Get-AzureRmSubscription

### <a name="step-3"></a>第 3 步

选择您要使用的 Azure 订阅。

    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"

### <a name="step-4"></a>第 4 步

创建资源组 （跳过此步骤如果您正在使用现有资源组）。

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure 的资源管理器需要的所有资源组都指定一个位置。 此位置用作该资源组中的资源的默认位置。 请确保创建的应用程序网关的所有命令都使用相同的资源组。

在上面的示例中，我们创建了一个名为"appgw RG"和"西美国"位置的资源组。

>[AZURE.NOTE] 如果您需要配置的自定义探测应用程序网关，请参阅[创建自定义通过使用 PowerShell 探测应用程序网关](application-gateway-create-probe-ps.md)。 查看[自定义的探测和运行状况监视](application-gateway-probe-overview.md)的详细信息。

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>创建虚拟网络和应用程序网关的子网

下面的示例演示如何通过使用资源管理器创建虚拟网络。

### <a name="step-1"></a>第 1 步

将地址范围 10.0.0.0/24 分配给子网变量可用于创建虚拟网络。

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

### <a name="step-2"></a>第 2 步

创建虚拟网络名资源组"appgw-rg"中的"appgwvnet"为前缀 10.0.0.0/16 使用的子网 10.0.0.0/24 的美国西部地区。

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

### <a name="step-3"></a>第 3 步

分配的下一个步骤，创建应用程序网关的子变量。

    $subnet=$vnet.Subnets[0]

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>创建前端配置公用 IP 地址

创建公用 IP 资源在资源组"appgw-rg"美国西部地区的"publicIP01"。

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic


## <a name="create-an-application-gateway-configuration-object"></a>创建应用程序网关配置对象

在创建应用程序网关之前必须设置所有的配置项。 以下步骤将创建所需的应用程序网关资源的配置项。

### <a name="step-1"></a>第 1 步

创建一个名为"gatewayIP01"的应用程序网关 IP 配置。 当启动时应用程序网关时，它选取配置子网的 IP 地址，并将网络流量路由到后端 IP 池中的 IP 地址。 请记住每个实例都需要一个 IP 地址。

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

### <a name="step-2"></a>第 2 步

名为"pool01"使用 IP 地址的后端 IP 地址池配置"134.170.185.46，134.170.188.221,134.170.185.50"。 这些 IP 地址是接收来自前端 IP 终结点的网络通信的 IP 地址。 更换前的 IP 地址，添加您自己应用程序的 IP 地址终结点。

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

### <a name="step-3"></a>第 3 步

在后端池的负载平衡网络流量配置应用程序网关设置"poolsetting01"。

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled

### <a name="step-4"></a>第 4 步

配置前端 IP 端口名"frontendport01"为公用 IP 终结点。

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

### <a name="step-5"></a>第 5 步

创建名为"fipconfig01"的前端 IP 配置，前端 IP 配置相关联的公用 IP 地址。

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

### <a name="step-6"></a>第 6 步

创建侦听器名称"listener01"，并将前端 IP 配置为前端端口相关联。

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

### <a name="step-7"></a>第 7 步

创建名为"rule01"，配置负载平衡器行为的负载平衡器路由规则。

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### <a name="step-8"></a>第 8 步

配置应用程序网关实例的大小。

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

>[AZURE.NOTE]  *InstanceCount*的默认值为 2，最大值为 10。 *GatewaySize*的默认值为中。 您可以选择 Standard_Small、 Standard_Medium 和 Standard_Large 之间。

## <a name="create-an-application-gateway-by-using-new-azurermapplicationgateway"></a>通过使用新建 AzureRmApplicationGateway 创建的应用程序网关

使用上述步骤中的所有配置项来创建应用程序网关。 在此示例中，应用程序网关称为"appgwtest"。

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

### <a name="step-9"></a>第 9 步

从公用 IP 资源附加到应用程序网关的 DNS 和 VIP 应用程序网关的详细信息。

    Get-AzureRmPublicIpAddress -Name publicIP01 -ResourceGroupName appgw-rg  

## <a name="delete-an-application-gateway"></a>删除应用程序网关

若要删除一个应用程序网关，请执行以下步骤︰

### <a name="step-1"></a>第 1 步

获取应用程序网关对象并将其关联到变量"$getgw"。

    $getgw = Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### <a name="step-2"></a>第 2 步

使用**Stop AzureRmApplicationGateway**停止应用程序网关。

    Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  

一旦应用程序网关处于停止状态时，使用**删除 AzureRmApplicationGateway** cmdlet 删除服务。

    Remove-AzureRmApplicationGateway -Name $appgwtest -ResourceGroupName appgw-rg -Force

>[AZURE.NOTE] **-强制**开关可用于取消删除确认消息。

要验证已删除该服务，您可以使用**Get AzureRmApplicationGateway** cmdlet。 此步骤不是必需的。

    Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

## <a name="get-application-gateway-dns-name"></a>获取应用程序网关 DNS 名称

创建网关后下, 一步是配置的前端通信。 当使用公用 IP，应用程序网关需要一个动态分配的 DNS 名称不友好。 为了确保最终用户可以按应用程序网关的 CNAME 记录可以用于指向应用程序网关的公共端点。 [配置在 Azure 中的自定义域名称](../cloud-services/cloud-services-custom-domain-name-portal.md)。 若要执行此操作，检索应用程序网关和使用 PublicIPAddress 元素连接到应用程序网关及其关联的 IP/DNS 名称的详细信息。 应用程序网关的 DNS 名称用于创建 CNAME 记录指向到此 DNS 名称的两个 web 应用程序。 VIP 可能会更改在重新启动应用程序网关不建议使用 A 记录。
    
    Get-AzureRmPublicIpAddress -ResourceGroupName appgw-RG -Name publicIP01
        
    Name                     : publicIP01
    ResourceGroupName        : appgw-RG
    Location                 : westus
    Id                       : /subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/publicIPAddresses/publicIP01
    Etag                     : W/"00000d5b-54ed-4907-bae8-99bd5766d0e5"
    ResourceGuid             : 00000000-0000-0000-0000-000000000000
    ProvisioningState        : Succeeded
    Tags                     : 
    PublicIpAllocationMethod : Dynamic
    IpAddress                : xx.xx.xxx.xx
    PublicIpAddressVersion   : IPv4
    IdleTimeoutInMinutes     : 4
    IpConfiguration          : {
                                 "Id": "/subscriptions/<subscription_id>/resourceGroups/appgw-RG/providers/Microsoft.Network/applicationGateways/appgwtest/frontendIP
                               Configurations/frontend1"
                               }
    DnsSettings              : {
                                 "Fqdn": "00000000-0000-xxxx-xxxx-xxxxxxxxxxxx.cloudapp.net"
                               }

## <a name="next-steps"></a>下一步行动

如果您想要配置 SSL 卸载，请参阅[配置 SSL 的应用程序网关，减轻负担](application-gateway-ssl.md)。

如果您想要配置的应用程序网关内部负载平衡器中使用，请参阅[创建具有内部负载平衡器 (ILB) 的应用程序网关](application-gateway-ilb.md)。

如果您想详细了解一般情况下负载平衡选项，请参阅︰

- [Azure 负载平衡器](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure 的流量管理器](https://azure.microsoft.com/documentation/services/traffic-manager/)
