<properties 
   pageTitle="创建和配置应用程序网关内部负载平衡器 (ILB) 在虚拟的网络中使用 |Microsoft Azure"
   description="此页提供了有关说明与负载平衡的内部终结点配置 Azure 应用程序网关"
   documentationCenter="na"
   services="application-gateway"
   authors="georgewallace"
   manager="jdial"
   editor="tysonn"/>
<tags 
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article" 
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services" 
   ms.date="08/19/2016"
   ms.author="gwallace"/>

# <a name="create-an-application-gateway-with-an-internal-load-balancer-ilb"></a>使用内部负载平衡器 (ILB) 创建的应用程序网关

> [AZURE.SELECTOR]
- [Azure 经典的步骤](application-gateway-ilb.md)
- [资源管理器 Powershell 的步骤](application-gateway-ilb-arm.md)

面向虚拟 IP internet 或内部端点不暴露给 internet，也称为内部负载平衡器 (ILB) 终结点，可以配置应用程序网关。 使用 ILB 配置网关可用于内部不向 internet 公开的业务线应用程序。 它也是有用的服务/层内多层应用程序，它参与安全边界不暴露给 internet，但仍需要循环负载分配、 会话建立密切关系或 SSL 端接。 这篇文章将引导您完成配置应用程序网关与 ILB 的步骤。

## <a name="before-you-begin"></a>在开始之前

1. 安装最新版本的 Web 平台安装程序使用 Azure PowerShell cmdlet。 您可以从下载并安装最新版本[下载页面](https://azure.microsoft.com/downloads/)的**Windows PowerShell**部分。
2. 请确认您有使用有效的子网的工作虚拟网络。
3. 请您在虚拟的网络中，或者与公用 IP/VIP 分配有后端服务器。


若要创建应用程序网关，请按列出的顺序执行以下步骤。 

1. [创建应用程序网关](#create-a-new-application-gateway)
2. [配置网关](#configure-the-gateway)
3. [设置网关配置](#set-the-gateway-configuration)
4. [启动该网关](#start-the-gateway)
4. [验证网关](#verify-the-gateway-status)



## <a name="create-an-application-gateway"></a>创建应用程序网关︰

**若要创建网关**，使用`New-AzureApplicationGateway`cmdlet，替换为您自己的值。 请注意计费网关不会在这里启动。 成功启动网关时，在后面的步骤，开始计费。

    PS C:\> New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

    VERBOSE: 4:31:35 PM - Begin Operation: New-AzureApplicationGateway 
    VERBOSE: 4:32:37 PM - Completed Operation: New-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   55ef0460-825d-2981-ad20-b9a8af41b399

**若要验证**是否已创建网关，您可以使用`Get-AzureApplicationGateway`cmdlet。 

中的示例，*说明*， *InstanceCount*和*GatewaySize*都是可选参数。 *InstanceCount*的默认值为 2，最大值为 10。 *GatewaySize*的默认值为中。 小型和大型的其他可用的值。 因为网关尚未开始， *Vip*和*DnsName*均为空白。 这些被创建网关后处于运行状态。 

    PS C:\> Get-AzureApplicationGateway AppGwTest

    VERBOSE: 4:39:39 PM - Begin Operation:
    Get-AzureApplicationGateway VERBOSE: 4:39:40 PM - Completed 
    Operation: Get-AzureApplicationGateway
    Name: AppGwTest 
    Description: 
    VnetName: testvnet1 
    Subnets: {Subnet-1} 
    InstanceCount: 2 
    GatewaySize: Medium 
    State: Stopped 
    VirtualIPs: 
    DnsName:


## <a name="configure-the-gateway"></a>配置网关

应用程序网关配置包含多个值。 值可以绑定在一起，以构建配置。
 
这些值为︰

- **后端服务器池︰**后端服务器的 IP 地址的列表。 列出的 IP 地址或者应属于 VNet 的子网，或应公用 IP/VIP。 
- **后端服务器池设置︰**每个池都有设置，如端口、 协议和基于 cookie 的关系等。 这些设置到池密切相关，并且应用于池中的所有服务器。
- **前端端口︰**此端口是打开的应用程序网关的公用端口。 流量达到此端口，，然后获取重定向到一台后端服务器。
- **侦听器︰**侦听器具有前端端口、 协议 （Http 或 Https，这些都是区分大小写），和 SSL 证书名称 （如果配置 SSL 卸载）。 
- **规则︰**该规则绑定侦听器和后端服务器池并定义时将触发特定侦听器通信应定向到哪个后端服务器池。 目前，只有*基本*的规则被支持。 *基本*规则是循环负载分配。

通过创建的配置对象，或通过使用配置 XML 文件，您可以构建您的配置。 若要构造您的配置使用配置 XML 文件，请使用下面的示例。



请注意以下事项︰


- *FrontendIPConfigurations*元素描述用于配置应用程序网关与 ILB 相关的 ILB 的详细信息。 

- 前端 IP*类型*应该设置为专用

- 应将*StaticIPAddress*设置为所需内部 IP 的网关接收通信。 请注意*StaticIPAddress*元素是可选的。 如果不设置，选择可用内部 IP 子网内部署。 

- 在*FrontendIPConfiguration*中指定的*名称*元素的值应在 HTTPListener 的*FrontendIP*元素中，用于指 FrontendIPConfiguration。

 **配置 XML 示例**

 

        <?xml version="1.0" encoding="utf-8"?>
        <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
            <FrontendIPConfigurations>
                <FrontendIPConfiguration>
                    <Name>fip1</Name> 
                    <Type>Private</Type> 
                    <StaticIPAddress>10.0.0.10</StaticIPAddress> 
                </FrontendIPConfiguration>
            </FrontendIPConfigurations>
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
                    <FrontendIP>fip1</FrontendIP>
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
    


## <a name="set-the-gateway-configuration"></a>设置网关配置

接下来，您将设置应用程序网关。 您可以使用`Set-AzureApplicationGatewayConfig`cmdlet 的配置对象，或配置 XML 文件。 

    PS C:\> Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml

    VERBOSE: 7:54:59 PM - Begin Operation: Set-AzureApplicationGatewayConfig 
    VERBOSE: 7:55:32 PM - Completed Operation: Set-AzureApplicationGatewayConfig
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   9b995a09-66fe-2944-8b67-9bb04fcccb9d

## <a name="start-the-gateway"></a>启动该网关

一旦配置了网关，则使用`Start-AzureApplicationGateway`用于启动该网关。 网关已成功启动后，将开始使用应用程序网关的计费。 


> [AZURE.NOTE] `Start-AzureApplicationGateway` Cmdlet 可能需要多达 15-20 分钟才能完成。 
   
    PS C:\> Start-AzureApplicationGateway AppGwTest 

    VERBOSE: 7:59:16 PM - Begin Operation: Start-AzureApplicationGateway 
    VERBOSE: 8:05:52 PM - Completed Operation: Start-AzureApplicationGateway
    Name       HTTP Status Code     Operation ID                             Error 
    ----       ----------------     ------------                             ----
    Successful OK                   fc592db8-4c58-2c8e-9a1d-1c97880f0b9b

## <a name="verify-the-gateway-status"></a>验证网关状态

使用`Get-AzureApplicationGateway`用于检查网关的状态。 如果上一步中成功*启动 AzureApplicationGateway* ，状态*运行*，应和 Vip 和 DnsName 应具有有效的条目。 此示例演示该 cmdlet 的第一行上跟输出。 在此示例中，该网关正在运行，并且已准备好采取通信。 

> [AZURE.NOTE] 应用程序网关配置为接受在此示例中的 10.0.0.10 的配置 ILB 终点处的流量。

    PS C:\> Get-AzureApplicationGateway AppGwTest 

    VERBOSE: 8:09:28 PM - Begin Operation: Get-AzureApplicationGateway 
    VERBOSE: 8:09:30 PM - Completed Operation: Get-AzureApplicationGateway
    Name          : AppGwTest
    Description   : 
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    VirtualIPs    : {10.0.0.10}
    DnsName       : appgw-b2a11563-2b3a-4172-a4aa-226ee4c23eed.cloudapp.net

## <a name="next-steps"></a>下一步行动


如果您想详细了解一般情况下负载平衡选项，请参阅︰

- [Azure 负载平衡器](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure 的流量管理器](https://azure.microsoft.com/documentation/services/traffic-manager/)
