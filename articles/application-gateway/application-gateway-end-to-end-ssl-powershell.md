<properties
    pageTitle="使用应用程序网关配置 SSL 策略和端到端 SSL |Microsoft Azure"
    description="本文介绍如何使用应用程序网关使用 Azure 资源管理器 PowerShell 配置端到端 SSL"
    services="application-gateway"
    documentationCenter="na"
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

# <a name="configure-ssl-policy-and-end-to-end-ssl-with-application-gateway-using-powershell"></a>使用应用程序网关使用 PowerShell 配置 SSL 策略和端到端 SSL

## <a name="overview"></a>概述

应用程序网关支持端到端加密的通信。 应用程序网关通过终止 SSL 连接在应用程序网关实现此目的。 网关的通信应用的路由规则、 重新加密数据包，然后将数据包转发到适当的后端，基于定义的路由规则。 来自 web 服务器的任何响应返回到最终用户经历相同的过程。

另一个功能，应用程序网关支持被禁用某些 SSL 协议版本。 应用程序网关支持禁用下面的协议版本;TLSv1.0、 TLSv1.1 和 TLSv1.2。

> [AZURE.NOTE] SSL 2.0 和 SSL 3.0，默认情况下禁用，无法启用。 他们被认为是不安全的也不能使用应用程序网关

![方案中的图像][scenario]

## <a name="scenario"></a>方案

在此方案中，您将学习如何创建使用使用 PowerShell 的端到端 SSL 的应用程序网关。

该方案将︰

- 创建一个名为"appgw rg"的资源组
- 创建一个名为"appgwvnet"与 10.0.0.0/16 保留 CIDR 块的虚拟网络。
- 创建名为"appgwsubnet"和"appsubnet"两个子网。
- 创建支持禁用某些 SSL 协议的端到端 SSL 加密的小应用程序网关。

## <a name="before-you-begin"></a>在开始之前

若要使用应用程序网关配置端到端 SSL，证书是必需的网关和证书所需的后端服务器。 网关证书用于加密和解密使用 SSL 向它发送的通信。 网关证书需要个人信息交换 (pfx) 格式。 这种文件格式允许为要导出的私钥所必需的应用程序网关执行加密和解密的通信量。

端到端 ssl 加密后端必须使用应用程序网关的白名单。 这是通过上载到应用程序网关 backends 的公共证书。 这可以确保应用程序网关只能与已知的后端实例。 这进一步保护安全的端到端通信。

以下步骤描述了此过程︰

## <a name="create-the-resource-group"></a>创建资源组

这一节将指导您完成创建资源组，包含应用程序网关。

### <a name="step-1"></a>第 1 步

登录到您的 Azure 帐户中。

    Login-AzureRmAccount

### <a name="step-2"></a>第 2 步

选择订阅以使用此方案。

    Select-AzureRmsubscription -SubscriptionName "<Subscription name>"

### <a name="step-3"></a>第 3 步

创建资源组 （跳过此步骤如果您正在使用现有资源组）。

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>创建虚拟网络和应用程序网关的子网

下面的示例创建一个虚拟网络和两个子网。 一个子网被用来存放应用程序网关。 其他的子网用于承载该 web 应用程序 backends。

### <a name="step-1"></a>第 1 步

有关为应用程序网关本身使用的子网分配一个地址范围。

    $gwSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -AddressPrefix 10.0.0.0/24

> [AZURE.NOTE] 应该正确地调整应用程序网关配置的子网。 应用程序网关可以配置为最多 10 个实例。 每个实例需要从子网 1 的 IP 地址。 太小的子网的可能会影响横向扩展的应用程序网关。

### <a name="step-2"></a>第 2 步

指定要用于后端地址池的地址范围。

    $nicSubnet = New-AzureRmVirtualNetworkSubnetConfig  -Name 'appsubnet' -AddressPrefix 10.0.2.0/24

### <a name="step-3"></a>第 3 步

使用在前面步骤中定义的子网创建虚拟网络。

    $vnet = New-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $gwSubnet, $nicSubnet

### <a name="step-4"></a>第 4 步

检索虚拟网络资源和子网资源，将在以下步骤中使用︰

    $vnet = Get-AzureRmvirtualNetwork -Name 'appgwvnet' -ResourceGroupName appgw-rg
    $gwSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appgwsubnet' -VirtualNetwork $vnet
    $nicSubnet = Get-AzureRmVirtualNetworkSubnetConfig -Name 'appsubnet' -VirtualNetwork $vnet

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>创建前端配置公用 IP 地址

创建公用 IP 资源要用于应用程序网关。 使用此公用 IP 地址的下面的步骤。

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name 'publicIP01' -Location "West US" -AllocationMethod Dynamic

> [AZURE.IMPORTANT] 应用程序网关不支持使用定义的域标签创建一个公用 IP 地址的使用。 支持仅公用 IP 地址的动态创建的域标签。 如果您需要应用程序网关的友好 dns 名称，建议使用 cname 记录作为别名。

## <a name="create-an-application-gateway-configuration-object"></a>创建应用程序网关配置对象

在创建应用程序网关之前必须设置所有的配置项。 以下步骤将创建所需的应用程序网关资源的配置项。

### <a name="step-1"></a>第 1 步

创建应用程序网关 IP 配置时，此设置用于配置应用程序网关使用哪个子网。 当启动时应用程序网关时，它选取配置子网的 IP 地址，并将网络流量路由到后端 IP 池中的 IP 地址。 请记住每个实例都需要一个 IP 地址。

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name 'gwconfig' -Subnet $gwSubnet

### <a name="step-2"></a>第 2 步

创建前端的 IP 配置中，此设置将一个私有或公用 ip 地址映射到前端应用程序网关。 下面的步骤将前面步骤中的公用 IP 地址与前端 IP 配置相关联。

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name 'fip01' -PublicIPAddress $publicip

### <a name="step-3"></a>第 3 步

将后端的 IP 地址池配置与后端 web 服务器的 IP 地址。 这些 IP 地址是接收来自前端 IP 终结点的网络通信的 IP 地址。 更换下的 IP 地址，添加您自己应用程序的 IP 地址终结点。

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name 'pool01' -BackendIPAddresses 1.1.1.1, 2.2.2.2, 3.3.3.3

> [AZURE.NOTE] 完全限定的域名称 (FQDN) 也是使用-BackendFqdns 开关代替后端服务器的 ip 地址是有效的值。

### <a name="step-4"></a>第 4 步

配置公用 IP 终结点的前端 IP 端口。 此端口是最终用户连接到的端口。

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name 'port01'  -Port 443

### <a name="step-5"></a>第 5 步

配置应用程序网关证书。 此证书用于解密，然后重新对应用程序网关的通信进行加密。

    $cert = New-AzureRmApplicationGatewaySslCertificate -Name cert01 -CertificateFile <full path to .pfx file> -Password <password for certificate file>

> [AZURE.NOTE] 此示例将证书用于 SSL 连接配置。 证书必须是以.pfx 格式和密码必须介于 4 到 12 个字符之间。

### <a name="step-6"></a>第 6 步

创建 HTTP 侦听器应用程序网关。 指定要使用的前端 ip 配置、 端口和 ssl 证书。

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01 -Protocol Https -FrontendIPConfiguration $fipconfig -FrontendPort $fp -SslCertificate $cert

### <a name="step-7"></a>第 7 步

上载用于 ssl 证书启用池后端资源。

> [AZURE.NOTE] 默认探测器从后端的 IP 地址上的**默认**SSL 绑定中获取公钥，并将接收到您在此处提供的公钥值的公钥值。 检索公共密钥可能不一定期望到**如果**您使用主机头和 SNI 在后端流流量的站点。 如果有疑问，请访问 https://127.0.0.1/ 后端以确认哪些证书用于**默认**的 SSL 绑定。 在这一节中使用来自该请求的公用密钥。 如果在 HTTPS 绑定上使用主机头和 SNI 并且您不会收到响应和证书从手动浏览器请求到后端上的 https://127.0.0.1/，您必须设置后端上的默认 SSL 绑定。 如果不这样做，探测器将失败后, 端将不会列入白名单。

    $authcert = New-AzureRmApplicationGatewayAuthenticationCertificate -Name 'whitelistcert1' -CertificateFile C:\users\gwallace\Desktop\cert.cer

> [AZURE.NOTE] 在此步骤中提供的证书应该是位于后端 pfx 证书的公钥。 在后端服务器上安装的证书 （不根证书） 导出。CER 格式，并在此步骤中使用它。 此步骤白名单后端使用应用程序网关。

### <a name="step-8"></a>第 8 步

配置应用程序网关后端 http 设置。 指定上载到 http 设置与前面步骤中的证书。

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name 'setting01' -Port 443 -Protocol Https -CookieBasedAffinity Enabled -AuthenticationCertificates $authcert

### <a name="step-9"></a>第 9 步

创建负载平衡路由规则，配置负载平衡器行为。 在此示例中，创建基本循环规则。

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name 'rule01' -RuleType basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### <a name="step-10"></a>第 10 步

配置应用程序网关实例的大小。  可用的大小是**标准\_小型**，**标准\_中型**，和**标准\_大**。  产能，可用的值是从 1 到 10。

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

>[AZURE.NOTE] 出于测试目的，可以选择实例计数为 1。 务必要知道两个实例在任何实例计数不受 SLA，因此不建议使用。 小网关将被用于开发测试而不是用于生产目的。

### <a name="step-11"></a>第 11 步

配置 SSL 策略以用于应用程序网关。 应用程序网关，可禁用某些 SSL 协议版本。

下列值是可以被禁用的协议版本的列表。

- **TLSv1_0**
- **TLSv1_1**
- **TLSv1_2**

下面的示例禁用 TLSv1\_0。

    $sslPolicy = New-AzureRmApplicationGatewaySslPolicy -DisabledSslProtocols TLSv1_0

## <a name="create-the-application-gateway"></a>创建应用程序网关

使用前面的所有步骤，创建应用程序网关。 创建网关是一个长时间运行的进程。

    $appgw = New-AzureRmApplicationGateway -Name appgateway -SslCertificates $cert -ResourceGroupName "appgw-rg" -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku -SslPolicy $sslPolicy -AuthenticationCertificates $authcert -Verbose

## <a name="disable-ssl-protocol-versions-on-an-existing-application-gateway"></a>禁用 SSL 协议版本在现有应用程序网关

前面的步骤将带您通过创建具有端到端 ssl 的应用程序，并禁用某些 SSL 协议版本。 下面的示例将禁用某些 SSL 策略，在现有应用程序网关。

### <a name="step-1"></a>第 1 步

检索要更新的应用程序网关。

    $gw = Get-AzureRmApplicationGateway -Name AdatumAppGateway -ResourceGroupName AdatumAppGatewayRG

### <a name="step-2"></a>第 2 步

定义一个 SSL 策略。 在以下示例中，TLSv1.0 和 TLSv1.1 被禁用。

    Set-AzureRmApplicationGatewaySslPolicy -DisabledSslProtocols TLSv1_0, TLSv1_1 -ApplicationGateway $gw

### <a name="step-3"></a>第 3 步

最后，更新网关。 值得注意的是最后这一步，是一个长时间运行的任务。 完成后，端到端 ssl 配置应用程序网关。

    $gw | Set-AzureRmApplicationGateway

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

了解有关访问[Web 应用程序防火墙概述](application-gateway-webapplicationfirewall-overview.md)的强化与 Web 应用程序防火墙应用程序网关通过 web 应用程序的安全性

[scenario]: ./media/application-gateway-end-to-end-ssl-powershell/scenario.png
