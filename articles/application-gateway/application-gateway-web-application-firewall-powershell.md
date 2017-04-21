<properties
   pageTitle="在新的或现有的应用程序网关配置 Web 应用程序防火墙 |Microsoft Azure"
   description="本文提供有关如何开始使用 web 应用程序防火墙上现有的或新的应用程序网关的指导。"
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
   ms.date="10/25/2016"
   ms.author="gwallace"/>

# <a name="configure-web-application-firewall-on-a-new-or-existing-application-gateway"></a>在新的或现有的应用程序网关配置 Web 应用程序防火墙

> [AZURE.SELECTOR]
- [Azure 门户](application-gateway-web-application-firewall-portal.md)
- [资源管理器的 azure PowerShell](application-gateway-web-application-firewall-powershell.md)

Web 应用程序防火墙 (WAF) 在 Azure 应用程序网关保护 web 应用程序中常见的基于 web 的攻击如 SQL 注入、 跨站点脚本攻击和会话劫持。

Azure 应用程序网关是一个 7 层负载平衡器。 无论是对云的内部，它提供了故障切换时，不同的服务器之间的路由性能的 HTTP 请求。 应用程序提供了许多应用程序交付控制器 (ADC) 功能，其中包括 HTTP 负载平衡、 基于 cookie 的会话相关性，安全套接字层 (SSL) 卸载自定义运行状况探测器、 支持多站点，和许多其他人。 若要查找受支持的功能的完整列表，请访问应用程序网关概述

以下文章演示如何[添加到现有应用程序网关的 web 应用程序防火墙](#add-web-application-firewall-to-an-existing-application-gateway)并[创建使用 web 应用程序防火墙应用程序网关](#create-an-application-gateway-with-web-application-firewall)。

![方案中的图像][scenario]

## <a name="waf-configuration-differences"></a>WAF 配置差异

如果您已经阅读[创建 PowerShell 使用应用程序网关](application-gateway-create-gateway-arm.md)，您了解的 SKU 设置时创建的应用程序网关配置。 WAF 提供其他设置来定义应用程序网关配置的 SKU 时。 有您在应用程序网关本身进行任何其他更改。

**SKU** -没有 WAF 支持普通应用程序网关**标准\_小型**，**标准\_中型**，和**标准\_大**大小。 随着 WAF，有两个附加 Sku， **WAF\_中**和**WAF\_大**。 不支持 WAF 小应用程序网关。

**层**-可用值为**标准**或**WAF**。 在使用 Web 应用程序防火墙时，必须选择**WAF** 。

**模式**— 此设置是 WAF 的模式。 允许的值为**检测**和**预防**。 WAF 设置在检测模式下，当所有的威胁都存储在日志文件中。 在保护模式下，仍然会记录事件，但是攻击者接收来自应用程序网关的 403 未经授权的响应。

## <a name="add-web-application-firewall-to-an-existing-application-gateway"></a>将 web 应用程序防火墙添加到现有应用程序网关

请确保您使用的最新版本的 Azure PowerShell。 [使用 Windows PowerShell 与资源管理器](../powershell-azure-resource-manager.md)提供了更多的信息。

### <a name="step-1"></a>第 1 步

登录到您的 Azure 帐户中。

    Login-AzureRmAccount

### <a name="step-2"></a>第 2 步

选择订阅以使用此方案。

    Select-AzureRmSubscription -SubscriptionName "<Subscription name>"

### <a name="step-3"></a>第 3 步

检索您要添加到 Web 应用程序防火墙的网关。

    $gw = Get-AzureRmApplicationGateway -Name "AdatumGateway" -ResourceGroupName "MyResourceGroup"


### <a name="step-4"></a>第 4 步

配置 web 应用程序防火墙 sku。 可用的大小是**WAF\_大**和**WAF\_中型**。 当使用 web 应用程序防火墙层必须**WAF**，设置 sku 时必须确认容量。

    $gw | Set-AzureRmApplicationGatewaySku -Name WAF_Large -Tier WAF -Capacity 2

### <a name="step-5"></a>第 5 步

在下面的示例定义配置 WAF 设置︰

对于**WafMode**设置，可用值为防范和检测。

    $gw | Set-AzureRmApplicationGatewayWebApplicationFirewallConfiguration -Enabled $true -FirewallMode Prevention

### <a name="step-6"></a>第 6 步

更新应用程序网关与前面步骤中定义的设置。

    Set-AzureRmApplicationGateway -ApplicationGateway $gw

此命令将更新使用 Web 应用程序防火墙应用程序网关。 建议查看[应用程序网关诊断程序](application-gateway-diagnostics.md)，以了解如何查看日志应用程序网关。 由于 WAF 的安全性质，日志需要定期进行检查，以了解您的 web 应用程序的安全状况。

## <a name="create-an-application-gateway-with-web-application-firewall"></a>创建 Web 应用程序防火墙应用程序网关

以下步骤将指导您完成整个过程从开始到结束使用 Web 应用程序防火墙创建应用程序网关的。

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

    Select-AzureRmsubscription -SubscriptionName "<Subscription name>"

### <a name="step-4"></a>第 4 步

创建资源组 （跳过此步骤如果您正在使用现有资源组）。

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure 的资源管理器需要的所有资源组都指定一个位置。 此位置用作该资源组中的资源的默认位置。 请确保创建的应用程序网关的所有命令都使用相同的资源组。

在前面的示例中，我们创建了一个名为"appgw RG"和"西美国"位置的资源组。

>[AZURE.NOTE] 如果您需要配置的自定义探测应用程序网关，请参阅[创建自定义通过使用 PowerShell 探测应用程序网关](application-gateway-create-probe-ps.md)。 查看[自定义的探测和运行状况监视](application-gateway-probe-overview.md)的详细信息。

### <a name="step-5"></a>第 5 步

有关为应用程序网关本身使用的子网分配一个地址范围。

    $gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

> [AZURE.NOTE] 为应用程序的子网应该有不少于 28 掩码位。 此值在应用程序网关实例的子网中留出 10 个可用地址。 与较小的子网，您可能无法在以后添加更多实例应用程序网关。

### <a name="step-6"></a>第 6 步

指定要用于后端地址池的地址范围。

    $nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

### <a name="step-7"></a>第 7 步

在步骤中创建的资源组中的前一个子网使用的虚拟网络的创建︰[创建资源组](#create-the-resource-group)

    $vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet

### <a name="step-8"></a>第 8 步

检索虚拟网络资源和子网资源，将在以下步骤中使用︰

    $vnet = Get-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
    $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
    $nicSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet

### <a name="step-9"></a>第 9 步

创建公用 IP 资源要用于应用程序网关。 此公用 IP 地址使用以下步骤之一︰

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -name 'appgwpip' -Location "West US" -AllocationMethod Dynamic

> [AZURE.IMPORTANT] 应用程序网关不支持使用定义的域标签创建一个公用 IP 地址的使用。 支持仅公用 IP 地址的动态创建的域标签。 如果您需要应用程序网关的友好 dns 名称，建议使用 cname 记录作为别名。

### <a name="step-10"></a>第 10 步

在创建应用程序网关之前必须设置所有的配置项。 以下步骤将创建所需的应用程序网关资源的配置项。

创建应用程序网关 IP 配置，则会以此配置哪些应用程序网关使用的子网。 当启动时应用程序网关时，它选取配置子网的 IP 地址，并将网络流量路由到后端 IP 池中的 IP 地址。 请记住每个实例都需要一个 IP 地址。

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

### <a name="step-11"></a>第 11 步

将后端的 IP 地址池配置与后端 web 服务器的 IP 地址。 这些 IP 地址是接收来自前端 IP 终结点的网络通信的 IP 地址。 更换下的 IP 地址，添加您自己应用程序的 IP 地址终结点。

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

### <a name="step-12"></a>第 12 步

上载用于 ssl 证书启用池后端资源。

    $authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile <full path to .cer file>

### <a name="step-13"></a>第 13 步

配置应用程序网关后端 http 设置。 指定上载到 http 设置与前面步骤中的证书。

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

### <a name="step-14"></a>第 14 步

配置公用 IP 终结点的前端 IP 端口。 此端口是最终用户连接到的端口。

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

### <a name="step-15"></a>第 15 步

创建前端的 IP 配置中，此设置将一个私有或公用 ip 地址映射到前端应用程序网关。 下面的步骤将前面步骤中的公用 IP 地址与前端 IP 配置相关联。

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

### <a name="step-16"></a>第 16 步

配置应用程序网关证书。 此证书用于解密，然后重新对应用程序网关的通信进行加密。

    $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password <password for certificate file>

### <a name="step-17"></a>第 17 步

创建 HTTP 侦听器应用程序网关。 指定要使用的前端 ip 配置、 端口和 ssl 证书。

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

### <a name="step-18"></a>第 18 步

创建负载平衡路由规则，配置负载平衡器行为。 在此示例中，创建基本循环规则。

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool
   
### <a name="step-19"></a>第 19 步

配置应用程序网关实例的大小。

    $sku = New-AzureRmApplicationGatewaySku -Name WAF_Medium -Tier WAF -Capacity 2

>[AZURE.NOTE]  您可以选择**WAF\_中**和**WAF\_大**，层时使用 WAF 都**WAF**。 容量是 1 到 10 之间的任意数字。

### <a name="step-20"></a>第 20 步

配置 WAF 的模式，可接受值为**防范**和**检测**。

    $config = New-AzureRmApplicationGatewayWafConfig -Enabled $true -WafMode "Prevention"

### <a name="step-21"></a>第 21 步

使用上述步骤中的所有配置项来创建应用程序网关。 在此示例中，应用程序网关称为"appgwtest"。

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -WebApplicationFirewallConfig $config -SslCertificates $cert -AuthenticationCertificates $authcert

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

了解如何配置诊断日志记录来记录与 Web 应用程序防火墙的事件被检测到或无法访问[应用程序网关诊断](application-gateway-diagnostics.md)


[scenario]: ./media/application-gateway-web-application-firewall-powershell/scenario.png