<properties
   pageTitle="应用程序网关在典型部署模型中使用 PowerShell 创建自定义的探测 |Microsoft Azure"
   description="了解如何应用程序网关在典型部署模型中使用 PowerShell 创建自定义的探测"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor=""
   tags="azure-service-management"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="create-a-custom-probe-for-azure-application-gateway-classic-by-using-powershell"></a>创建自定义的探测 Azure 应用程序网关 （经典） 使用 PowerShell

> [AZURE.SELECTOR]
- [Azure 门户](application-gateway-create-probe-portal.md)
- [资源管理器的 azure PowerShell](application-gateway-create-probe-ps.md)
- [Azure 的经典 PowerShell](application-gateway-create-probe-classic-ps.md)

[AZURE.INCLUDE [azure-probe-intro-include](../../includes/application-gateway-create-probe-intro-include.md)]

[AZURE.INCLUDE [azure-arm-classic-important-include](../../includes/learn-about-deployment-models-classic-include.md)]了解如何[执行这些步骤，使用资源管理器模型](application-gateway-create-probe-ps.md)。

[AZURE.INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-an-application-gateway"></a>创建应用程序网关

若要创建应用程序网关︰

1. 创建一个应用程序网关资源。
2. 创建配置 XML 文件或配置对象。
3. 提交到新创建的应用程序网关资源配置。

### <a name="create-an-application-gateway-resource"></a>创建应用程序网关资源

若要创建网关，请使用**新建 AzureApplicationGateway** cmdlet，替换为您自己的值。 计费网关不在此时开始。 成功启动网关时，在后面的步骤，开始计费。

下面的示例创建一个应用程序网关使用虚拟网络称为"testvnet1"和名为"网-1"的子网。

    New-AzureApplicationGateway -Name AppGwTest -VnetName testvnet1 -Subnets @("Subnet-1")

若要验证已创建网关，您可以使用**Get AzureApplicationGateway** cmdlet。

    Get-AzureApplicationGateway AppGwTest

>[AZURE.NOTE]  *InstanceCount*的默认值为 2，最大值为 10。 *GatewaySize*的默认值为中。 您可以选择小型、 中型和大型。

 因为网关尚未开始， *VirtualIPs*和*DnsName*均为空白。 这些被创建网关后处于运行状态。

## <a name="configure-an-application-gateway"></a>配置应用程序网关

通过使用 XML 或配置对象，您可以配置应用程序网关。

## <a name="configure-an-application-gateway-by-using-xml"></a>通过使用 XML 来配置应用程序网关

在下面的示例中，您可以使用 XML 文件来配置所有应用程序网关设置并将它们提交到应用程序网关资源。  

### <a name="step-1"></a>第 1 步

将以下文本复制到记事本。

    <ApplicationGatewayConfiguration xmlns:i="http://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/windowsazure">
    <FrontendIPConfigurations>
        <FrontendIPConfiguration>
            <Name>fip1</Name>
            <Type>Private</Type>
        </FrontendIPConfiguration>
    </FrontendIPConfigurations>    
    <FrontendPorts>
        <FrontendPort>
            <Name>port1</Name>
            <Port>80</Port>
        </FrontendPort>
    </FrontendPorts>
    <Probes>
        <Probe>
            <Name>Probe01</Name>
            <Protocol>Http</Protocol>
            <Host>contoso.com</Host>
            <Path>/path/custompath.htm</Path>
            <Interval>15</Interval>
            <Timeout>15</Timeout>
            <UnhealthyThreshold>5</UnhealthyThreshold>
        </Probe>
      </Probes>
     <BackendAddressPools>
        <BackendAddressPool>
            <Name>pool1</Name>
            <IPAddresses>
                <IPAddress>1.1.1.1</IPAddress>
                <IPAddress>2.2.2.2</IPAddress>
            </IPAddresses>
        </BackendAddressPool>
    </BackendAddressPools>
    <BackendHttpSettingsList>
        <BackendHttpSettings>
            <Name>setting1</Name>
            <Port>80</Port>
            <Protocol>Http</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            <RequestTimeout>120</RequestTimeout>
            <Probe>Probe01</Probe>
        </BackendHttpSettings>
    </BackendHttpSettingsList>
    <HttpListeners>
        <HttpListener>
            <Name>listener1</Name>
            <FrontendIP>fip1</FrontendIP>
        <FrontendPort>port1</FrontendPort>
            <Protocol>Http</Protocol>
        </HttpListener>
    </HttpListeners>
    <HttpLoadBalancingRules>
        <HttpLoadBalancingRule>
            <Name>lbrule1</Name>
            <Type>basic</Type>
            <BackendHttpSettings>setting1</BackendHttpSettings>
            <Listener>listener1</Listener>
            <BackendAddressPool>pool1</BackendAddressPool>
        </HttpLoadBalancingRule>
    </HttpLoadBalancingRules>
    </ApplicationGatewayConfiguration>


编辑的括号中的配置项的值。 使用.xml 扩展名保存文件。

下面的示例演示如何使用配置文件来设置应用程序网关，以负载平衡在公共端口 80 上的 HTTP 通信量并将网络通信发送到通过使用自定义的探测的两个 IP 地址之间的后端端口 80。

>[AZURE.IMPORTANT] Http 或 Https 协议项是区分大小写。

新的配置项\<探测器\>添加配置自定义的探测。

配置参数是︰

- **名称**-自定义探测的引用名称。
- **协议**-使用协议 （可能的值为 HTTP 或 HTTPS）。
- **主机**和**路径**的完整 URL 路径的调用的应用程序网关来确定实例的运行状况。 例如，如果您有网站 http://contoso.com/，则自定义的探测可以配置为"http://contoso.com/path/custompath.htm"探测器检查能够成功的 HTTP 响应。
- **时间间隔**-以秒为单位配置探测间隔检查。
- **超时**— 定义 HTTP 响应检查探测的超时。
- **UnhealthyThreshold** -失败标志与后端的实例所需的 HTTP 响应的*不正常*。

在探测器名称中引用<BackendHttpSettings>配置，以指定哪个后端池使用自定义探测设置。

## <a name="add-a-custom-probe-configuration-to-an-existing-application-gateway"></a>将自定义探测配置添加到现有应用程序网关

更改当前应用程序网关的配置需要三个步骤︰ 获取当前的 XML 配置文件、 修改具有自定义的探测，以及用新的 XML 设置来配置应用程序网关。

### <a name="step-1"></a>第 1 步

通过使用 get AzureApplicationGatewayConfig 获取 XML 文件。 这导出 XML 添加探测器设置要修改的配置。

    Get-AzureApplicationGatewayConfig -Name "<application gateway name>" -Exporttofile "<path to file>"


### <a name="step-2"></a>第 2 步

在文本编辑器中打开 XML 文件。 添加`<probe>`节之后`<frontendport>`。

    <Probes>
        <Probe>
            <Name>Probe01</Name>
            <Protocol>Http</Protocol>
            <Host>contoso.com</Host>
            <Path>/path/custompath.htm</Path>
            <Interval>15</Interval>
            <Timeout>15</Timeout>
            <UnhealthyThreshold>5</UnhealthyThreshold>
        </Probe>
    </Probes>

在 backendHttpSettings 部分中的 xml，添加探测器名称，如下面的示例所示︰

        <BackendHttpSettings>
            <Name>setting1</Name>
            <Port>80</Port>
            <Protocol>Http</Protocol>
            <CookieBasedAffinity>Enabled</CookieBasedAffinity>
            <RequestTimeout>120</RequestTimeout>
            <Probe>Probe01</Probe>
        </BackendHttpSettings>

保存 XML 文件。

### <a name="step-3"></a>第 3 步

通过**设置 AzureApplicationGatewayConfig**，用新的 XML 文件更新应用程序网关配置。 这将更新使用新配置您的应用程序网关。

    Set-AzureApplicationGatewayConfig -Name "<application gateway name>" -Configfile "<path to file>"


## <a name="next-steps"></a>下一步行动

如果您想要配置安全套接字层 (SSL) 卸载，请参阅[配置 SSL 的应用程序网关，减轻负担](application-gateway-ssl.md)。

如果您想要配置的应用程序网关内部负载平衡器中使用，请参阅[创建具有内部负载平衡器 (ILB) 的应用程序网关](application-gateway-ilb.md)。
