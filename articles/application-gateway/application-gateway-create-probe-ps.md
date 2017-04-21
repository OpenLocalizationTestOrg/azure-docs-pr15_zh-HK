<properties
   pageTitle="应用程序网关使用 PowerShell 资源管理器中创建自定义的探测 |Microsoft Azure"
   description="了解如何使用 PowerShell 资源管理器中为应用程序网关创建自定义的探测"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/06/2016"
   ms.author="gwallace" />

# <a name="create-a-custom-probe-for-azure-application-gateway-by-using-powershell-for-azure-resource-manager"></a>Azure 应用程序网关使用 PowerShell 的 Azure 资源管理器中创建自定义的探测

> [AZURE.SELECTOR]
- [Azure 门户](application-gateway-create-probe-portal.md)
- [资源管理器的 azure PowerShell](application-gateway-create-probe-ps.md)
- [Azure 的经典 PowerShell](application-gateway-create-probe-classic-ps.md)

[AZURE.INCLUDE [azure-probe-intro-include](../../includes/application-gateway-create-probe-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-rm-include.md)][经典的部署模型](application-gateway-create-probe-classic-ps.md)。

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

### <a name="step-1"></a>第 1 步

使用登录 AzureRmAccount 进行身份验证。

    Login-AzureRmAccount

### <a name="step-2"></a>第 2 步

请检查帐户订阅。

    Get-AzureRmSubscription

### <a name="step-3"></a>第 3 步

选择您要使用的 Azure 订阅。 <BR>


    Select-AzureRmSubscription -Subscriptionid "GUID of subscription"


### <a name="step-4"></a>第 4 步

创建资源组 （跳过此步骤如果您正在使用现有资源组）。

    New-AzureRmResourceGroup -Name appgw-rg -Location "West US"

Azure 的资源管理器需要的所有资源组都指定一个位置。 此位置用作该资源组中的资源的默认位置。 请确保创建的应用程序网关的所有命令都使用相同的资源组。

在上面的示例中，我们创建了一个名为"appgw RG"和"西美国"位置的资源组。

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>创建虚拟网络和应用程序网关的子网

以下步骤将创建一个虚拟网络和应用程序网关的子网。

### <a name="step-1"></a>第 1 步


分配给子网变量被用来创建虚拟网络地址范围 10.0.0.0/24。

    $subnet = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

### <a name="step-2"></a>第 2 步

创建虚拟网络名资源组"appgw-rg"中的"appgwvnet"为前缀 10.0.0.0/16 使用的子网 10.0.0.0/24 的美国西部地区。

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnet


### <a name="step-3"></a>第 3 步

分配的下一个步骤，创建应用程序网关的子变量。

    $subnet = $vnet.Subnets[0]

## <a name="create-a-public-ip-address-for-the-front-end-configuration"></a>创建前端配置公用 IP 地址


创建公用 IP 资源在资源组"appgw-rg"美国西部地区的"publicIP01"。

    $publicip = New-AzureRmPublicIpAddress -ResourceGroupName appgw-rg -Name publicIP01 -Location "West US" -AllocationMethod Dynamic


## <a name="create-an-application-gateway-configuration-object-with-a-custom-probe"></a>创建自定义的探测应用程序网关配置对象

在创建应用程序网关之前设置的所有配置项。 以下步骤将创建所需的应用程序网关资源的配置项。

### <a name="step-1"></a>第 1 步

创建一个名为"gatewayIP01"的应用程序网关 IP 配置。 当启动时应用程序网关时，它拾取从配置子网的 IP 地址，并将网络通信量传送到后端 IP 池中的 IP 地址。 请记住每个实例都需要一个 IP 地址。

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet


### <a name="step-2"></a>第 2 步


名为"pool01"使用 IP 地址的后端 IP 地址池配置"134.170.185.46，134.170.188.221,134.170.185.50"。 这些值为接收来自前端 IP 终结点的网络通信的 IP 地址。 您替换上面添加您自己应用程序的 IP 地址终结点的 IP 地址。

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50


### <a name="step-3"></a>第 3 步


在此步骤中配置自定义的探测。

使用的参数是︰

- **时间间隔**-以秒为单位配置探测间隔检查。
- **超时**— 定义 HTTP 响应检查探测的超时。
- **主机的名称和路径**-调用的应用程序网关来确定实例的运行状况的完整 URL 路径。 例如，如果您有网站 http://contoso.com/，则自定义的探测可以配置为"http://contoso.com/path/custompath.htm"探测器检查能够成功的 HTTP 响应。
- **UnhealthyThreshold** -失败标志与后端的实例所需的 HTTP 响应的*不正常*。

<BR>

    $probe = New-AzureRmApplicationGatewayProbeConfig -Name probe01 -Protocol Http -HostName "contoso.com" -Path "/path/path.htm" -Interval 30 -Timeout 120 -UnhealthyThreshold 8

### <a name="step-4"></a>第 4 步

在后端池中配置的通信的应用程序网关设置"poolsetting01"。 此步骤还具有用于池后端应用程序网关请求响应超时配置。 当后端响应达到超时限制时，则应用程序网关将取消请求。 此值是不同于仅用于后端响应来探测检查探测超时。

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 80

### <a name="step-5"></a>第 5 步

配置前端 IP 端口名"frontendport01"为公用 IP 终结点。

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01 -Port 80

### <a name="step-6"></a>第 6 步

创建名为"fipconfig01"的前端 IP 配置，前端 IP 配置相关联的公用 IP 地址。


    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -PublicIPAddress $publicip

### <a name="step-7"></a>第 7 步

创建侦听器名称"listener01"，并将前端 IP 配置为前端端口相关联。

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

### <a name="step-8"></a>第 8 步

创建名为"rule01"，配置负载平衡器行为的负载平衡器路由规则。

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

### <a name="step-9"></a>第 9 步

配置应用程序网关实例的大小。

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2


>[AZURE.NOTE]  *InstanceCount*的默认值为 2，最大值为 10。 *GatewaySize*的默认值为中。 您可以选择 Standard_Small、 Standard_Medium 和 Standard_Large 之间。

## <a name="create-an-application-gateway-by-using-new-azurermapplicationgateway"></a>通过使用新建 AzureRmApplicationGateway 创建的应用程序网关

使用上面的步骤中的所有配置项来创建应用程序网关。 在此示例中，应用程序网关称为"appgwtest"。

    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -Probes $probe -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

## <a name="add-a-probe-to-an-existing-application-gateway"></a>将探测器添加到现有应用程序网关

您可以向现有应用程序网关添加自定义的探测的四个步骤。

### <a name="step-1"></a>第 1 步

使用**Get AzureRmApplicationGateway**到 PowerShell 变量加载应用程序网关资源。

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### <a name="step-2"></a>第 2 步

将探测器添加到现有的网关配置。

    $getgw = Add-AzureRmApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name probe01 -Protocol Http -HostName "contoso.com" -Path "/path/custompath.htm" -Interval 30 -Timeout 120 -UnhealthyThreshold 8


在示例中，配置自定义的探测检查 URL 路径 contoso.com/path/custompath.htm 每隔 30 秒。 8 故障的探测请求的最大配置超时阈值为 120 秒。

### <a name="step-3"></a>第 3 步

使用将探测器添加到后端池设置配置和超时**-设置-AzureRmApplicationGatewayBackendHttpSettings**。


     $getgw = Set-AzureRmApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol Http -CookieBasedAffinity Disabled -Probe $probe -RequestTimeout 120

### <a name="step-4"></a>第 4 步

通过使用**一组 AzureRmApplicationGateway**应用程序网关到保存配置。

    Set-AzureRmApplicationGateway -ApplicationGateway $getgw

## <a name="remove-a-probe-from-an-existing-application-gateway"></a>删除现有的应用程序网关的探测器

以下是这些步骤从现有应用程序网关删除自定义的探测。

### <a name="step-1"></a>第 1 步

使用**Get AzureRmApplicationGateway**到 PowerShell 变量加载应用程序网关资源。

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg


### <a name="step-2"></a>第 2 步

通过**删除 AzureRmApplicationGatewayProbeConfig**探测器配置删除应用程序网关。

    $getgw = Remove-AzureRmApplicationGatewayProbeConfig -ApplicationGateway $getgw -Name $getgw.Probes.name

### <a name="step-3"></a>第 3 步

通过更新池后端设置去探测和超时设置**-设置-AzureRmApplicationGatewayBackendHttpSettings**。


     $getgw = Set-AzureRmApplicationGatewayBackendHttpSettings -ApplicationGateway $getgw -Name $getgw.BackendHttpSettingsCollection.name -Port 80 -Protocol http -CookieBasedAffinity Disabled

### <a name="step-4"></a>第 4 步

通过使用**一组 AzureRmApplicationGateway**应用程序网关到保存配置。 

    Set-AzureRmApplicationGateway -ApplicationGateway $getgw

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

了解如何配置 SSL 卸载访问[配置 SSL 卸载](application-gateway-ssl-arm.md)

