<properties
   pageTitle="通过使用传统部署配置 SSL 卸载的应用程序网关 |Microsoft Azure"
   description="本文提供了创建使用 SSL 的应用程序网关的说明卸载使用 Azure 经典的部署模型。"
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

# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-classic-deployment-model"></a>配置 SSL 卸载的应用程序网关使用的典型部署模型

> [AZURE.SELECTOR]
-[Azure 的门户网站](application-gateway-ssl-portal.md)
-[Azure 资源管理器 PowerShell](application-gateway-ssl-arm.md)
-[Azure 经典 PowerShell](application-gateway-ssl.md)

可以将 azure 应用程序网关配置为终止在网关，以避免代价高昂的 SSL 解密任务在 web 场中发生的安全套接字层 (SSL) 会话。 前端服务器设置和 web 应用程序的管理，还简化了 SSL 卸载。

## <a name="before-you-begin"></a>在开始之前

1. 通过使用 Web 平台安装程序安装最新版本的 Azure PowerShell cmdlet。 您可以从下载并安装最新版本[下载网页](https://azure.microsoft.com/downloads/)的**Windows PowerShell**部分。
2. 请确认您有使用有效的子网的工作虚拟网络。 请确保没有虚拟机或云部署使用子网。 应用程序网关必须自行在虚拟的网络子网中。
3. 被配置为使用应用程序网关服务器必须存在或已经创建了虚拟的网络中或与公用 IP/VIP 及其终结点分配。

要配置 SSL 卸载应用程序网关，请执行下列步骤中列出的顺序︰

1. [创建应用程序网关](#create-an-application-gateway)
2. [上载的 SSL 证书](#upload-ssl-certificates)
3. [配置网关](#configure-the-gateway)
4. [设置网关配置](#set-the-gateway-configuration)
5. [启动该网关](#start-the-gateway)
6. [验证网关状态](#verify-the-gateway-status)


## <a name="create-an-application-gateway"></a>创建应用程序网关

若要创建网关，请使用**新建 AzureApplicationGateway** cmdlet，替换为您自己的值。 计费网关不在此时开始。 成功启动网关时，在后面的步骤，开始计费。

    New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

若要验证已创建网关，您可以使用**Get AzureApplicationGateway** cmdlet。

中的示例，*说明*， *InstanceCount*和*GatewaySize*都是可选参数。 *InstanceCount*的默认值为 2，最大值为 10。 *GatewaySize*的默认值为中。 小型和大型的其他可用的值。 因为网关尚未开始， *VirtualIPs*和*DnsName*均为空白。 这些值被创建网关后处于运行状态。

    Get-AzureApplicationGateway AppGwTest

## <a name="upload-ssl-certificates"></a>上载的 SSL 证书

使用**添加 AzureApplicationGatewaySslCertificate**上载到应用程序网关服务器证书*pfx*格式。 证书的名称是用户选择名称和应用程序网关内必须唯一。 此证书是通过名称引用，此应用程序网关的所有证书管理操作。

这下面的示例演示该 cmdlet，示例中的值替换为您自己。

    Add-AzureApplicationGatewaySslCertificate  -Name AppGwTest -CertificateName GWCert -Password <password> -CertificateFile <full path to pfx file>

接下来，验证证书上载。 使用**Get AzureApplicationGatewayCertificate** cmdlet。

此示例演示该 cmdlet 的第一行上跟输出。

    Get-AzureApplicationGatewaySslCertificate AppGwTest

    VERBOSE: 5:07:54 PM - Begin Operation: Get-AzureApplicationGatewaySslCertificate
    VERBOSE: 5:07:55 PM - Completed Operation: Get-AzureApplicationGatewaySslCertificate
    Name           : SslCert
    SubjectName    : CN=gwcert.app.test.contoso.com
    Thumbprint     : AF5ADD77E160A01A6......EE48D1A
    ThumbprintAlgo : sha1RSA
    State..........: Provisioned

>[AZURE.NOTE] 证书密码具有介于 4 到 12 个字符、 字母或数字。 特殊字符是不能接受。

## <a name="configure-the-gateway"></a>配置网关

应用程序网关配置包含多个值。 值可以绑定在一起，以构建配置。

这些值为︰

- **后端服务器池︰**后端服务器的 IP 地址的列表。 列出的 IP 地址既应属于虚拟子网或应当是公共 IP/VIP。
- **后端服务器池设置︰**每个池都有设置，如端口、 协议和基于 cookie 的关系等。 这些设置到池密切相关，并且应用于池中的所有服务器。
- **前端端口︰**此端口是打开的应用程序网关在公共端口。 流量达到此端口，，然后获取重定向到一台后端服务器。
- **侦听器︰**侦听器具有前端端口、 协议 （Http 或 Https，这些值是区分大小写），和 SSL 证书名称 （如果配置 SSL 卸载）。
- **规则︰**该规则绑定侦听器和后端服务器池并定义时将触发特定侦听器通信应定向到哪个后端服务器池。 目前，只有*基本*的规则被支持。 *基本*规则是循环负载分配。

**额外的配置说明**

对于 SSL 证书配置，请在**HttpListener**协议应当改为*Https* （区分大小写）。 **SslCert**元素值设置为与上载之前 SSL 证书一节中使用相同的名称添加到**HttpListener**中。 应更新前端端口为 443。

**若要启用基于 cookie 的关系**︰ 可以配置应用程序网关，以确保从客户端会话的请求始终指向同一个 VM 在 web 场中。 这种情况下是通过注入的会话 cookie，使网关来适当地直接通信。 若要启用基于 cookie 的相似性，请**BackendHttpSettings**元素中设置*启用* **CookieBasedAffinity** 。



通过创建一个配置对象或使用配置 XML 文件，您可以构建您的配置。
若要构造您的配置使用配置 XML 文件，请使用下面的示例︰

**配置 XML 示例**

    <?xml version="1.0" encoding="utf-8"?>
    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
        <FrontendIPConfigurations />
        <FrontendPorts>
            <FrontendPort>
                <Name>FrontendPort1</Name>
                <Port>443</Port>
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
                <Protocol>Https</Protocol>
                <SslCert>GWCert</SslCert>
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

接下来，您可以设置应用程序网关。 与配置对象或配置 XML 文件，您可以使用**一组 AzureApplicationGatewayConfig** cmdlet。

    Set-AzureApplicationGatewayConfig -Name AppGwTest -ConfigFile D:\config.xml

## <a name="start-the-gateway"></a>启动该网关

一旦配置了网关，使用**开始 AzureApplicationGateway** cmdlet 启动网关。 网关已成功启动后，将开始使用应用程序网关的计费。


**注意︰****开始 AzureApplicationGateway** cmdlet 可能需要多达 15-20 分钟才能完成。

    Start-AzureApplicationGateway AppGwTest

## <a name="verify-the-gateway-status"></a>验证网关状态

使用**Get AzureApplicationGateway** cmdlet 检查网关的状态。 如果上一步中成功**启动 AzureApplicationGateway** ，应运行*状态*，和*VirtualIPs*和*DnsName*应具有有效的条目。

此示例演示应用程序网关，运行，并已准备好采取通信。

    Get-AzureApplicationGateway AppGwTest

    Name          : AppGwTest2
    Description   :
    VnetName      : testvnet1
    Subnets       : {Subnet-1}
    InstanceCount : 2
    GatewaySize   : Medium
    State         : Running
    VirtualIPs    : {23.96.22.241}
    DnsName       : appgw-4c960426-d1e6-4aae-8670-81fd7a519a43.cloudapp.net


## <a name="next-steps"></a>下一步行动


如果您想详细了解一般情况下负载平衡选项，请参阅︰

- [Azure 负载平衡器](https://azure.microsoft.com/documentation/services/load-balancer/)
- [Azure 的流量管理器](https://azure.microsoft.com/documentation/services/traffic-manager/)