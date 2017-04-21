<properties
   pageTitle="创建、 启动或删除应用程序网关 |Microsoft Azure"
   description="此页面介绍了如何创建、 配置、 启动和删除 Azure 应用程序网关"
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

# <a name="create-start-or-delete-an-application-gateway"></a>创建、 启动或删除应用程序网关

> [AZURE.SELECTOR]
- [Azure 门户](application-gateway-create-gateway-portal.md)
- [资源管理器的 azure PowerShell](application-gateway-create-gateway-arm.md)
- [Azure 的经典 PowerShell](application-gateway-create-gateway.md)
- [Azure 的资源管理器模板](application-gateway-create-gateway-arm-template.md)
- [Azure CLI](application-gateway-create-gateway-cli.md)

Azure 应用程序网关是一个 7 层负载平衡器。 无论是对云的内部，它提供了故障切换时，不同的服务器之间的路由性能的 HTTP 请求。 应用程序网关提供了许多应用程序交付控制器 (ADC) 功能，其中包括 HTTP 负载平衡、 基于 cookie 的会话相关性，安全套接字层 (SSL) 卸载自定义运行状况探测器、 支持多站点，和许多其他人。 若要查找受支持的功能的完整列表，请访问[应用程序网关概述](application-gateway-introduction.md)

这篇文章将引导您完成创建、 配置、 启动和删除应用程序网关的步骤。

## <a name="before-you-begin"></a>在开始之前

1. 通过使用 Web 平台安装程序安装最新版本的 Azure PowerShell cmdlet。 您可以从下载并安装最新版本[下载网页](https://azure.microsoft.com/downloads/)的**Windows PowerShell**部分。
2. 如果您有一个现有的虚拟网络，选择现有空的子网或由应用程序网关，目的只是为了使用现有虚拟网络中创建新的子网。 您不能部署到不同的虚拟网络，与想要部署应用程序网关后，除非使用了 vnet 对等的资源的应用程序网关。 若要了解详细信息，请访问[Vnet 对等](../virtual-network/virtual-network-peering-overview.md)
3. 请确认您有使用有效的子网的工作虚拟网络。 请确保没有虚拟机或云部署使用子网。 应用程序网关必须自行在虚拟的网络子网中。
3. 被配置为使用应用程序网关服务器必须存在或已经创建了虚拟的网络中或与公用 IP/VIP 及其终结点分配。

## <a name="what-is-required-to-create-an-application-gateway"></a>必须创建应用程序网关是什么？

当您使用**新建 AzureApplicationGateway**命令创建应用程序网关时，在此时设置没有配置和新创建的资源配置通过使用 XML 或配置对象。

这些值为︰

- **后端服务器池︰**后端服务器的 IP 地址的列表。 列出的 IP 地址既应属于虚拟子网或应当是公共 IP/VIP。
- **后端服务器池设置︰**每个池都有设置，如端口、 协议和基于 cookie 的关系等。 这些设置到池密切相关，并且应用于池中的所有服务器。
- **前端端口︰**此端口是打开的应用程序网关在公共端口。 流量达到此端口，，然后获取重定向到一台后端服务器。
- **侦听器︰**侦听器具有前端端口、 协议 （Http 或 Https，这些值是区分大小写），和 SSL 证书名称 （如果配置 SSL 卸载）。
- **规则︰**该规则绑定侦听器和后端服务器池并定义时将触发特定侦听器通信应定向到哪个后端服务器池。

## <a name="create-an-application-gateway"></a>创建应用程序网关

若要创建应用程序网关︰

1. 创建一个应用程序网关资源。
2. 创建配置 XML 文件或配置对象。
3. 提交到新创建的应用程序网关资源配置。

>[AZURE.NOTE] 如果您需要配置的自定义探测应用程序网关，请参阅[创建自定义通过使用 PowerShell 探测应用程序网关](application-gateway-create-probe-classic-ps.md)。 查看[自定义的探测和运行状况监视](application-gateway-probe-overview.md)的详细信息。

![方案示例][scenario]

### <a name="create-an-application-gateway-resource"></a>创建应用程序网关资源

若要创建网关，请使用**新建 AzureApplicationGateway** cmdlet，替换为您自己的值。 计费网关不在此时开始。 成功启动网关时，在后面的步骤，开始计费。

下面的示例创建一个应用程序网关使用虚拟网络称为"testvnet1"和名为"网-1"的子网。


    New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

    VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway
    VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399


 *描述*、 *InstanceCount*和*GatewaySize*是可选参数。

若要验证已创建网关，您可以使用**Get AzureApplicationGateway** cmdlet。

    Get-AzureApplicationGateway AppGwTest
    Name          : AppGwTest
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Stopped
    VirtualIPs    : {}
    DnsName       :

>[AZURE.NOTE]  *InstanceCount*的默认值为 2，最大值为 10。 *GatewaySize*的默认值为中。 您可以选择小型、 中型和大型。

因为网关尚未开始， *VirtualIPs*和*DnsName*均为空白。 这些被创建网关后处于运行状态。

## <a name="configure-the-application-gateway"></a>配置应用程序网关

通过使用 XML 或配置对象，您可以配置应用程序网关。

## <a name="configure-the-application-gateway-by-using-xml"></a>通过使用 XML 来配置应用程序网关

在下面的示例中，您可以使用 XML 文件来配置所有应用程序网关设置并将它们提交到应用程序网关资源。  

### <a name="step-1"></a>第 1 步  

将以下文本复制到记事本。

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendPorts>
            <FrontendPort>
                <Name>(name-of-your-frontend-port)</Name>
                <Port>(port number)</Port>
            </FrontendPort>
        </FrontendPorts>
        <BackendAddressPools>
            <BackendAddressPool>
                <Name>(name-of-your-backend-pool)</Name>
                <IPAddresses>
                    <IPAddress>(your-IP-address-for-backend-pool)</IPAddress>
                    <IPAddress>(your-second-IP-address-for-backend-pool)</IPAddress>
                </IPAddresses>
            </BackendAddressPool>
        </BackendAddressPools>
        <BackendHttpSettingsList>
            <BackendHttpSettings>
                <Name>(backend-setting-name-to-configure-rule)</Name>
                <Port>80</Port>
                <Protocol>[Http|Https]</Protocol>
                <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            </BackendHttpSettings>
        </BackendHttpSettingsList>
        <HttpListeners>
            <HttpListener>
                <Name>(name-of-the-listener)</Name>
                <FrontendPort>(name-of-your-frontend-port)</FrontendPort>
                <Protocol>[Http|Https]</Protocol>
            </HttpListener>
        </HttpListeners>
        <HttpLoadBalancingRules>
            <HttpLoadBalancingRule>
                <Name>(name-of-load-balancing-rule)</Name>
                <Type>basic</Type>
                <BackendHttpSettings>(backend-setting-name-to-configure-rule)</BackendHttpSettings>
                <Listener>(name-of-the-listener)</Listener>
                <BackendAddressPool>(name-of-your-backend-pool)</BackendAddressPool>
            </HttpLoadBalancingRule>
        </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>

编辑的括号中的配置项的值。 使用.xml 扩展名保存文件。

>[AZURE.IMPORTANT] Http 或 Https 协议项是区分大小写。

下面的示例演示如何使用配置文件来设置应用程序网关。 示例负载平衡在公共端口 80 上的 HTTP 通信量，并将网络通信发送到两个 IP 地址之间的后端端口 80。

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendPorts>
            <FrontendPort>
                <Name>FrontendPort1</Name>
                <Port>80</Port>
            </FrontendPort>
        </FrontendPorts>
        <BackendAddressPools>
            <BackendAddressPool>
                <Name>BackendPool1</Name>
                <IPAddresses>
                    <IPAddress>10.0.0.1</IPAddress>
                    <IPAddress>10.0.0.2</IPAddress>
                </IPAddresses>
            </BackendAddressPool>
        </BackendAddressPools>
        <BackendHttpSettingsList>
            <BackendHttpSettings>
                <Name>BackendSetting1</Name>
                <Port>80</Port>
                <Protocol>Http</Protocol>
                <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            </BackendHttpSettings>
        </BackendHttpSettingsList>
        <HttpListeners>
            <HttpListener>
                <Name>HTTPListener1</Name>
                <FrontendPort>FrontendPort1</FrontendPort>
                <Protocol>Http</Protocol>
            </HttpListener>
        </HttpListeners>
        <HttpLoadBalancingRules>
            <HttpLoadBalancingRule>
                <Name>HttpLBRule1</Name>
                <Type>basic</Type>
                <BackendHttpSettings>BackendSetting1</BackendHttpSettings>
                <Listener>HTTPListener1</Listener>
                <BackendAddressPool>BackendPool1</BackendAddressPool>
            </HttpLoadBalancingRule>
        </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>


### <a name="step-2"></a>第 2 步

接下来，设置应用程序网关。 配置 XML 文件中使用**一组 AzureApplicationGatewayConfig** cmdlet。


    Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile "D:\config.xml"

    VERBOSE: 7:54:59 PM - Begin Operation: Set-AzureApplicationGatewayConfig
    VERBOSE: 7:55:32 PM - Completed Operation: Set-AzureApplicationGatewayConfig
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   9b995a09-66fe-2944-8b67-9bb04fcccb9d

## <a name="configure-the-application-gateway-by-using-a-configuration-object"></a>通过一个配置对象来配置应用程序网关

下面的示例演示如何使用来配置应用程序网关配置对象。 必须单独配置，然后将其添加到应用程序网关配置对象所有的配置项。 创建配置对象后，您可以使用**组 AzureApplicationGateway**命令提交到以前创建的应用程序网关资源配置。

>[AZURE.NOTE] 在将值分配给每个配置对象之前, 您需要声明 PowerShell 使用存储哪种类型的对象。 第一行创建单个项目定义什么 Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model （对象名称） 使用。

### <a name="step-1"></a>第 1 步

创建单个配置的所有项目。

下面的示例中所示，创建前端 IP。

    $fip = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration
    $fip.Name = "fip1"
    $fip.Type = "Private"
    $fip.StaticIPAddress = "10.0.0.5"

创建前端端口，如下面的示例中所示。

    $fep = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort
    $fep.Name = "fep1"
    $fep.Port = 80

创建池后端服务器。

 定义添加到后端服务器池下一个示例中所示的 IP 地址。


    $servers = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendServerCollection
    $servers.Add("10.0.0.1")
    $servers.Add("10.0.0.2")

 使用 $server 对象将值添加到后端池对象 ($pool)。

    $pool = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool
    $pool.BackendServers = $servers
    $pool.Name = "pool1"

创建的后端服务器池设置。

    $setting = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings
    $setting.Name = "setting1"
    $setting.CookieBasedAffinity = "enabled"
    $setting.Port = 80
    $setting.Protocol = "http"

创建侦听器。

    $listener = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener
    $listener.Name = "listener1"
    $listener.FrontendPort = "fep1"
    $listener.FrontendIP = "fip1"
    $listener.Protocol = "http"
    $listener.SslCert = ""

创建规则。

    $rule = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule
    $rule.Name = "rule1"
    $rule.Type = "basic"
    $rule.BackendHttpSettings = "setting1"
    $rule.Listener = "listener1"
    $rule.BackendAddressPool = "pool1"

### <a name="step-2"></a>第 2 步

将所有单个配置项分配给应用程序网关配置对象 ($appgwconfig)。

将前端 IP 添加到配置中。

    $appgwconfig = New-Object Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.ApplicationGatewayConfiguration
    $appgwconfig.FrontendIPConfigurations = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendIPConfiguration]"
    $appgwconfig.FrontendIPConfigurations.Add($fip)

将前端端口添加到配置中。

    $appgwconfig.FrontendPorts = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.FrontendPort]"
    $appgwconfig.FrontendPorts.Add($fep)

向配置添加后端服务器池。

    $appgwconfig.BackendAddressPools = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendAddressPool]"
    $appgwconfig.BackendAddressPools.Add($pool)

将后端池设置添加到配置中。

    $appgwconfig.BackendHttpSettingsList = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.BackendHttpSettings]"
    $appgwconfig.BackendHttpSettingsList.Add($setting)

将侦听器添加到配置中。

    $appgwconfig.HttpListeners = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpListener]"
    $appgwconfig.HttpListeners.Add($listener)

将规则添加到配置中。

    $appgwconfig.HttpLoadBalancingRules = New-Object "System.Collections.Generic.List[Microsoft.WindowsAzure.Commands.ServiceManagement.Network.ApplicationGateway.Model.HttpLoadBalancingRule]"
    $appgwconfig.HttpLoadBalancingRules.Add($rule)

### <a name="step-3"></a>第 3 步

通过使用**一组 AzureApplicationGatewayConfig**承诺应用程序网关资源配置对象。

    Set-AzureApplicationGatewayConfig -Name AppGwTest -Config $appgwconfig

## <a name="start-the-gateway"></a>启动该网关

一旦配置了网关，使用**开始 AzureApplicationGateway** cmdlet 启动网关。 网关已成功启动后，将开始使用应用程序网关的计费。

> [AZURE.NOTE] **开始 AzureApplicationGateway** cmdlet 可能需要多达 15-20 分钟才能完成。

    Start-AzureApplicationGateway AppGwTest

    VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway
    VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b

## <a name="verify-the-gateway-status"></a>验证网关状态

使用**Get AzureApplicationGateway** cmdlet 检查网关的状态。 如果上一步中成功**启动 AzureApplicationGateway** ，应运行*状态*，和*Vip*和*DnsName*应具有有效的条目。

下面的示例演示应用程序网关最多、 运行，并已准备好采取通信发送到`http://<generated-dns-name>.cloudapp.net`。

    Get-AzureApplicationGateway AppGwTest

    VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway
    VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
    Name          : AppGwTest
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    Vip           : 138.91.170.26
    DnsName       : appgw-1b8402e8-3e0d-428d-b661-289c16c82101.cloudapp.net


## <a name="delete-an-application-gateway"></a>删除应用程序网关

要删除应用程序网关︰

1. 使用**Stop AzureApplicationGateway** cmdlet 停止网关。
2. 使用**删除 AzureApplicationGateway** cmdlet 删除网关。
3. 请验证该网关已删除通过使用**Get AzureApplicationGateway** cmdlet。

下面的示例演示在跟输出的第一行**停止 AzureApplicationGateway** cmdlet。

    Stop-AzureApplicationGateway AppGwTest

    VERBOSE: 9:49:34 PM - Begin Operation: Stop-AzureApplicationGateway
    VERBOSE: 10:10:06 PM - Completed Operation: Stop-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   ce6c6c95-77b4-2118-9d65-e29defadffb8

一旦应用程序网关处于停止状态时，使用**删除 AzureApplicationGateway** cmdlet 删除服务。


    Remove-AzureApplicationGateway AppGwTest

    VERBOSE: 10:49:34 PM - Begin Operation: Remove-AzureApplicationGateway
    VERBOSE: 10:50:36 PM - Completed Operation: Remove-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error
    ----       ----------------     ------------                             ----
    Successful OK                   055f3a96-8681-2094-a304-8d9a11ad8301

要验证已删除该服务，您可以使用**Get AzureApplicationGateway** cmdlet。 此步骤不是必需的。


    Get-AzureApplicationGateway AppGwTest

    VERBOSE: 10:52:46 PM - Begin Operation: Get-AzureApplicationGateway

    Get-AzureApplicationGateway : ResourceNotFound: The gateway does not exist.
    .....

## <a name="next-steps"></a>下一步行动

如果您想要配置 SSL 卸载，请参阅[配置 SSL 的应用程序网关，减轻负担](application-gateway-ssl.md)。

如果您想要配置的应用程序网关内部负载平衡器中使用，请参阅[创建具有内部负载平衡器 (ILB) 的应用程序网关](application-gateway-ilb.md)。

如果您想详细了解一般情况下负载平衡选项，请参阅︰

- [Azure 负载平衡器](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure 的流量管理器](https://azure.microsoft.com/documentation/services/traffic-manager/)

[scenario]: ./media/application-gateway-create-gateway/scenario.png