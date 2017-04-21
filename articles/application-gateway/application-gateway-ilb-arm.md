<properties
   pageTitle="创建和配置应用程序网关内部负载平衡器 (ILB) 使用 Azure 资源管理器 |Microsoft Azure"
   description="此页面介绍了如何创建、 配置、 启动和删除的 Azure 资源管理器中的内部负载平衡器 (ILB) 与 Azure 应用程序网关"
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
   ms.date="08/19/2016"
   ms.author="gwallace"/>


# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb-by-using-azure-resource-manager"></a>通过使用 Azure 资源管理器中创建内部负载平衡器 (ILB) 与应用程序网关

> [AZURE.SELECTOR]
- [Azure 经典的步骤](application-gateway-ilb.md)
- [资源管理器 PowerShell 的步骤](application-gateway-ilb-arm.md)

与面向 Internet 的 VIP 或不暴露给 Internet，也称为内部负载平衡器 (ILB) 终结点的内部终结点，则可以配置 azure 应用程序网关。 使用 ILB 配置网关可用于不暴露给 Internet 的内部业务线应用程序。 它还可用于服务和多层应用程序中各层，坐在不暴露给 Internet 的安全边界，但仍需要循环负载分布、 会话建立密切关系或安全套接字层 (SSL) 终止。

这篇文章将引导您完成配置应用程序网关与 ILB 的步骤。

## <a name="before-you-begin"></a>在开始之前

1. 通过使用 Web 平台安装程序安装最新版本的 Azure PowerShell cmdlet。 您可以从下载并安装最新版本[下载网页](https://azure.microsoft.com/downloads/)的**Windows PowerShell**部分。
2. 对于应用程序网关创建虚拟网络和子网。 请确保没有虚拟机或云部署使用子网。 应用程序网关必须自行在虚拟的网络子网中。
3. 被配置为使用应用程序网关服务器必须存在或已经创建了虚拟的网络中或与公用 IP/VIP 及其终结点分配。

## <a name="what-is-required-to-create-an-application-gateway"></a>必须创建应用程序网关是什么？


- **后端服务器池︰**后端服务器的 IP 地址的列表。 列出的 IP 地址既应属于到虚拟网络，但在不同的子网上应用程序网关或应当是公共 IP/VIP。
- **后端服务器池设置︰**每个池都有设置，如端口、 协议和基于 cookie 的关系等。 这些设置到池密切相关，并且应用于池中的所有服务器。
- **前端端口︰**此端口是打开的应用程序网关在公共端口。 流量达到此端口，，然后获取重定向到一台后端服务器。
- **侦听器︰**侦听器具有前端端口、 协议 （Http 或 Https，这些都是区分大小写），和 SSL 证书名称 （如果配置 SSL 卸载）。
- **规则︰**该规则绑定侦听器和后端服务器池并定义时将触发特定侦听器通信应定向到哪个后端服务器池。 目前，只有*基本*的规则被支持。 *基本*规则是循环负载分配。



## <a name="create-an-application-gateway"></a>创建应用程序网关

使用 Azure 经典和 Azure 资源管理器之间的区别是在其中创建应用程序网关，并需要配置项的顺序。
使用资源管理器中，将使用应用程序网关的所有项是单独配置，然后将放在一起以创建应用程序网关资源。


下面是创建应用程序网关所需的步骤︰

1. 创建资源组的资源管理器
2. 创建虚拟网络和应用程序网关的子网
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

创建新的资源组 （跳过此步骤如果您正在使用现有资源组）。

    New-AzureRmResourceGroup -Name appgw-rg -location "West US"

Azure 的资源管理器需要的所有资源组都指定一个位置。 这是该资源组中的资源用作默认位置。 请确保创建的应用程序网关的所有命令都使用相同的资源组。

在上面的示例中，我们创建了一个名为"appgw rg"和"西美国"位置的资源组。

## <a name="create-a-virtual-network-and-a-subnet-for-the-application-gateway"></a>创建虚拟网络和应用程序网关的子网

下面的示例演示如何通过使用资源管理器创建虚拟网络︰

### <a name="step-1"></a>第 1 步

    $subnetconfig = New-AzureRmVirtualNetworkSubnetConfig -Name subnet01 -AddressPrefix 10.0.0.0/24

这将给子网变量被用来创建虚拟网络的地址范围 10.0.0.0/24。

### <a name="step-2"></a>第 2 步

    $vnet = New-AzureRmVirtualNetwork -Name appgwvnet -ResourceGroupName appgw-rg -Location "West US" -AddressPrefix 10.0.0.0/16 -Subnet $subnetconfig

这将创建虚拟网络名资源组"appgw-rg"中的"appgwvnet"为前缀 10.0.0.0/16 使用的子网 10.0.0.0/24 的美国西部地区。

### <a name="step-3"></a>第 3 步

    $subnet = $vnet.subnets[0]

这将子网对象分配给变量 $subnet 的下一步的行动。

## <a name="create-an-application-gateway-configuration-object"></a>创建应用程序网关配置对象

### <a name="step-1"></a>第 1 步

    $gipconfig = New-AzureRmApplicationGatewayIPConfiguration -Name gatewayIP01 -Subnet $subnet

这将创建一个名为"gatewayIP01"的应用程序网关 IP 配置。 当启动时应用程序网关时，它拾取从配置子网的 IP 地址，并将网络通信量传送到后端 IP 池中的 IP 地址。 请记住每个实例都需要一个 IP 地址。


### <a name="step-2"></a>第 2 步

    $pool = New-AzureRmApplicationGatewayBackendAddressPool -Name pool01 -BackendIPAddresses 134.170.185.46, 134.170.188.221,134.170.185.50

这将名为"pool01"使用 IP 地址的后端 IP 地址池配置"134.170.185.46，134.170.188.221,134.170.185.50"。 这些是接收来自前端 IP 终结点的网络通信的 IP 地址。 您替换上面添加您自己应用程序的 IP 地址终结点的 IP 地址。

### <a name="step-3"></a>第 3 步

    $poolSetting = New-AzureRmApplicationGatewayBackendHttpSettings -Name poolsetting01 -Port 80 -Protocol Http -CookieBasedAffinity Disabled

这将在后端池中配置应用程序网关设置"poolsetting01"的负载平衡网络通讯量。

### <a name="step-4"></a>第 4 步

    $fp = New-AzureRmApplicationGatewayFrontendPort -Name frontendport01  -Port 80

这将配置前端 IP 端口 ILB 名为"frontendport01"。

### <a name="step-5"></a>第 5 步

    $fipconfig = New-AzureRmApplicationGatewayFrontendIPConfig -Name fipconfig01 -Subnet $subnet

这将创建名为"fipconfig01"前端的 IP 配置并将其与当前虚拟网络子网中的专用 IP。

### <a name="step-6"></a>第 6 步

    $listener = New-AzureRmApplicationGatewayHttpListener -Name listener01  -Protocol Http -FrontendIPConfiguration $fipconfig -FrontendPort $fp

这将创建称为"listener01"的侦听器并将前端 IP 配置为前端端口。

### <a name="step-7"></a>第 7 步

    $rule = New-AzureRmApplicationGatewayRequestRoutingRule -Name rule01 -RuleType Basic -BackendHttpSettings $poolSetting -HttpListener $listener -BackendAddressPool $pool

这将创建名为"rule01"，配置负载平衡器行为该负载平衡器路由规则。

### <a name="step-8"></a>第 8 步

    $sku = New-AzureRmApplicationGatewaySku -Name Standard_Small -Tier Standard -Capacity 2

此配置的应用程序网关实例的大小。

>[AZURE.NOTE]  *InstanceCount*的默认值为 2，最大值为 10。 *GatewaySize*的默认值为中。 您可以选择 Standard_Small、 Standard_Medium 和 Standard_Large 之间。

## <a name="create-an-application-gateway-by-using-new-azureapplicationgateway"></a>通过使用新建 AzureApplicationGateway 创建的应用程序网关

使用上面的步骤中的所有配置项创建应用程序网关。 在此示例中，应用程序网关称为"appgwtest"。


    $appgw = New-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Location "West US" -BackendAddressPools $pool -BackendHttpSettingsCollection $poolSetting -FrontendIpConfigurations $fipconfig  -GatewayIpConfigurations $gipconfig -FrontendPorts $fp -HttpListeners $listener -RequestRoutingRules $rule -Sku $sku

这与上面的步骤中的所有配置项创建应用程序网关。 在此示例中，应用程序网关称为"appgwtest"。


## <a name="delete-an-application-gateway"></a>删除应用程序网关

若要删除一个应用程序网关，您需要执行以下操作的顺序︰

1. 使用**Stop AzureRmApplicationGateway** cmdlet 停止网关。
2. 使用**删除 AzureRmApplicationGateway** cmdlet 删除网关。
3. 请验证该网关已删除通过使用**Get AzureApplicationGateway** cmdlet。


### <a name="step-1"></a>第 1 步

获取应用程序网关对象并将其关联到变量"$getgw"。

    $getgw =  Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

### <a name="step-2"></a>第 2 步

使用**Stop AzureRmApplicationGateway**停止应用程序网关。 此示例的第一行上显示**Stop AzureRmApplicationGateway** cmdlet 跟输出。

    PS C:\> Stop-AzureRmApplicationGateway -ApplicationGateway $getgw  

    VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
    VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8

一旦应用程序网关处于停止状态时，使用**删除 AzureRmApplicationGateway** cmdlet 删除服务。


    PS C:\> Remove-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg -Force

    VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
    VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301

>[AZURE.NOTE] **-强制**开关可用于取消删除确认消息。


要验证已删除该服务，您可以使用**Get AzureRmApplicationGateway** cmdlet。 此步骤不是必需的。


    PS C:\>Get-AzureRmApplicationGateway -Name appgwtest -ResourceGroupName appgw-rg

    VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

    Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
    .....

## <a name="next-steps"></a>下一步行动

如果您想要配置 SSL 卸载，请参阅[配置 SSL 的应用程序网关，减轻负担](application-gateway-ssl.md)。

如果您想要配置的应用程序网关与 ILB 一起使用，请参阅[创建具有内部负载平衡器 (ILB) 的应用程序网关](application-gateway-ilb.md)。

如果您想详细了解一般情况下负载平衡选项，请参阅︰

- [Azure 负载平衡器](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure 的流量管理器](https://azure.microsoft.com/documentation/services/traffic-manager/)
