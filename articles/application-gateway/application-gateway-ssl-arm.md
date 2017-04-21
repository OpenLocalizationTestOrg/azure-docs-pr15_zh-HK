<properties
   pageTitle="通过使用 Azure 资源管理器中配置 SSL 卸载的应用程序网关 |Microsoft Azure"
   description="此页提供使用 Azure 资源管理器卸载说明进行操作以创建使用 SSL 的应用程序网关"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"/>
<tags
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/09/2016"
   ms.author="gwallace"/>

# <a name="configure-an-application-gateway-for-ssl-offload-by-using-azure-resource-manager"></a>通过使用 Azure 资源管理器中配置 SSL 卸载的应用程序网关

> [AZURE.SELECTOR]
-[Azure 的门户网站](application-gateway-ssl-portal.md)
-[Azure 的资源管理器 PowerShell](application-gateway-ssl-arm.md)
-[Azure 经典 PowerShell](application-gateway-ssl.md)

 可以将 azure 应用程序网关配置为终止在网关，以避免代价高昂的 SSL 解密任务在 web 场中发生的安全套接字层 (SSL) 会话。 前端服务器设置和 web 应用程序的管理，还简化了 SSL 卸载。

## <a name="before-you-begin"></a>在开始之前

1. 通过使用 Web 平台安装程序安装最新版本的 Azure PowerShell cmdlet。 您可以从下载并安装最新版本[下载网页](https://azure.microsoft.com/downloads/)的**Windows PowerShell**部分。
2. 创建虚拟网络和应用程序网关的子网。 请确保没有虚拟机或云部署使用子网。 应用程序网关必须自行在虚拟的网络子网中。
3. 服务器被配置为使用应用程序网关必须存在或已经创建了虚拟的网络中或与公用 IP/VIP 及其终结点分配。

## <a name="what-is-required-to-create-an-application-gateway"></a>必须创建应用程序网关是什么？


- **后端服务器池︰**后端服务器的 IP 地址的列表。 列出的 IP 地址既应属于虚拟子网或应当是公共 IP/VIP。
- **后端服务器池设置︰**每个池都有设置，如端口、 协议和基于 cookie 的关系等。 这些设置到池密切相关，并且应用于池中的所有服务器。
- **前端端口︰**此端口是打开的应用程序网关在公共端口。 流量达到此端口，，然后获取重定向到一台后端服务器。
- **侦听器︰**侦听器具有前端端口、 协议 （Http 或 Https，这些设置是区分大小写），和 SSL 证书名称 （如果配置 SSL 卸载）。
- **规则︰**该规则绑定侦听器和后端服务器池并定义时将触发特定侦听器通信应定向到哪个后端服务器池。 目前，只有*基本*的规则被支持。 *基本*规则是循环负载分配。

**额外的配置说明**

对于 SSL 证书配置，请在**HttpListener**协议应当改为*Https* （区分大小写）。 **SslCertificate**元素配置 SSL 证书的变量值添加到**HttpListener**中。 应更新前端端口为 443。

**若要启用基于 cookie 的关系**︰ 可以配置应用程序网关，以确保从客户端会话的请求始终指向同一个 VM 在 web 场中。 这种情况下是通过注入的会话 cookie，使网关来适当地直接通信。 若要启用基于 cookie 的相似性，请**BackendHttpSettings**元素中设置*启用* **CookieBasedAffinity** 。


## <a name="create-an-application-gateway"></a>创建应用程序网关

使用 Azure 经典部署模型和 Azure 资源管理器之间的区别是您创建的应用程序网关和需要配置项的顺序。

使用资源管理器中，使用应用程序网关的所有组件都单独配置，然后在一起以创建应用程序网关资源。


下面是创建应用程序网关所需的步骤︰

1. 创建资源组的资源管理器
2. 创建虚拟网络、 子网和公用 IP 应用程序网关
3. 创建应用程序网关配置对象
4. 创建应用程序网关资源


## <a name="create-a-resource-group-for-resource-manager"></a>创建资源组的资源管理器

请确保您切换使用 Azure 资源管理器 cmdlet PowerShell 模式。 [使用 Windows PowerShell 与资源管理器](../powershell-azure-resource-manager.md)提供了更多的信息。

### <a name="step-1"></a>第 1 步

    Login-AzureRmAccount

### <a name="step-2"></a>第 2 步

请检查帐户订阅。

    Get-AzureRmSubscription

提示您使用您的凭据进行身份验证。<BR>

### <a name="step-3"></a>第 3 步

选择您要使用的 Azure 订阅。 <BR>


    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"


### <a name="step-4"></a>第 4 步

创建资源组 （跳过此步骤如果您正在使用现有资源组）。

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure 的资源管理器需要的所有资源组都指定一个位置。 此设置用作该资源组中的资源的默认位置。 请确保创建的应用程序网关的所有命令都使用相同的资源组。

在上面的示例中，我们创建了一个名为"appgw RG"和"西美国"位置的资源组。

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>创建虚拟网络和应用程序网关的子网

下面的示例演示如何通过使用资源管理器创建虚拟网络︰

### <a name="step-1"></a>第 1 步

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

此示例将地址范围 10.0.0.0/24 分配给子网变量被用来创建虚拟网络。

### <a name="step-2"></a>第 2 步

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet

此示例创建虚拟网络名资源组"appgw-rg"中的"appgwvnet"为前缀 10.0.0.0/16 使用的子网 10.0.0.0/24 的美国西部地区。

### <a name="step-3"></a>第 3 步

    $subnet = $vnet.Subnets[0]

此示例将子网对象分配给变量 $subnet 的下一步的行动。

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>创建前端配置公用 IP 地址

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name publicIP01 -location "West US" -AllocationMethod Dynamic

此示例创建一个公用 IP 资源在资源组"appgw-rg"美国西部地区的"publicIP01"。


## <a name="create-an-application-gateway-configuration-object"></a>创建应用程序网关配置对象

### <a name="step-1"></a>第 1 步

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

此示例创建一个名为"gatewayIP01"的应用程序网关 IP 配置。 当启动时应用程序网关时，它拾取从配置子网的 IP 地址，并将网络通信量传送到后端 IP 池中的 IP 地址。 请记住每个实例都需要一个 IP 地址。

### <a name="step-2"></a>第 2 步

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

本示例将名为"pool01"使用 IP 地址的后端 IP 地址池配置"134.170.185.46，134.170.188.221,134.170.185.50。" 这些值为接收来自前端 IP 终结点的网络通信的 IP 地址。 从前面的示例中的 IP 地址替换为您的 web 应用程序终结点的 IP 地址。

### <a name="step-3"></a>第 3 步

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Enabled

此示例配置应用程序网关设置"poolsetting01"池中后端负载平衡网络通讯量。

### <a name="step-4"></a>第 4 步

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 443

此示例配置前端 IP 端口名"frontendport01"为公用 IP 终结点。

### <a name="step-5"></a>第 5 步

    $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path for certificate file> -Password ‘<password>’

此示例将证书用于 SSL 连接配置。 证书必须是以.pfx 格式和密码必须介于 4 到 12 个字符之间。

### <a name="step-6"></a>第 6 步

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

此示例创建名为"fipconfig01"的前端 IP 配置，并将与前端 IP 配置相关联的公用 IP 地址。

### <a name="step-7"></a>第 7 步

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert


此示例创建侦听器名称"listener01"，并将前端 IP 配置和证书的前端端口相关联。

### <a name="step-8"></a>第 8 步

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

本示例创建名为"rule01"，配置负载平衡器行为的负载平衡器路由规则。

### <a name="step-9"></a>第 9 步

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

此示例配置的应用程序网关实例的大小。

>[AZURE.NOTE]  *InstanceCount*的默认值为 2，最大值为 10。 *GatewaySize*的默认值为中。 您可以选择 Standard_Small、 Standard_Medium 和 Standard_Large 之间。

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a>通过使用新建 AzureApplicationGateway 创建的应用程序网关

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslCertificates $cert

此示例创建使用上述步骤中的所有配置项的应用程序网关。 在此示例中，应用程序网关称为"appgwtest"。

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

如果您想要配置的应用程序网关内部负载平衡器 (ILB) 中使用，请参阅[创建具有内部负载平衡器 (ILB) 的应用程序网关](application-gateway-ilb.md)。

如果您想详细了解一般情况下负载平衡选项，请参阅︰

- [Azure 负载平衡器](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure 的流量管理器](https://azure.microsoft.com/documentation/services/traffic-manager/)
