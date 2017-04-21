<properties
   pageTitle="解决应用程序网关错误的网关 (502) 错误 |Microsoft Azure"
   description="了解如何解决应用程序网关 502 错误"
   services="application-gateway"
   documentationCenter="na"
   authors="amitsriva"
   manager="rossort"
   editor=""
   tags="azure-resource-manager"
/>
<tags  
   ms.service="application-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="09/02/2016"
   ms.author="amitsriva" />

# <a name="troubleshooting-bad-gateway-errors-in-application-gateway"></a>应用程序网关在网关错误错误疑难解答

## <a name="overview"></a>概述

在配置之后 Azure 应用程序网关，该用户可能会遇到的错误之一就是"服务器错误︰ 502-Web 服务器作为网关或代理服务器时收到无效的响应"。 这可能是由于以下主要原因︰

- Azure 应用程序网关的后端池不是配置或为空。
- 虚拟机或实例中设置虚拟机比例都不是健康。
- 后端的虚拟机或实例的虚拟机设置比例未响应的默认运行状况探测器。
- 无效或不正确配置的自定义运行状况检测。
- 请求超时或连接性问题的用户请求。

## <a name="empty-backendaddresspool"></a>空 BackendAddressPool

### <a name="cause"></a>原因

如果应用程序网关没有虚拟机或后端地址池中配置的 VM 小数位数设置，它无法传送的任何客户请求，并将引发错误的网关错误。

### <a name="solution"></a>解决方案

确保后端地址池不为空。 这可以通过 PowerShell，CLI 或门户网站完成。

    Get-AzureRmApplicationGateway -Name "SampleGateway" -ResourceGroupName "ExampleResourceGroup"

从前面的 cmdlet 的输出应包含非空后端地址池。 下面是一个示例，两个池将返回的配置与后端的虚拟机的 FQDN 或 IP 地址。 BackendAddressPool 的资源调配状态必须是成功。
    
    BackendAddressPoolsText: 
            [{
                "BackendAddresses": [{
                    "ipAddress": "10.0.0.10",
                    "ipAddress": "10.0.0.11"
                }],
                "BackendIpConfigurations": [],
                "ProvisioningState": "Succeeded",
                "Name": "Pool01",
                "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
                "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool01"
            }, {
                "BackendAddresses": [{
                    "Fqdn": "xyx.cloudapp.net",
                    "Fqdn": "abc.cloudapp.net"
                }],
                "BackendIpConfigurations": [],
                "ProvisioningState": "Succeeded",
                "Name": "Pool02",
                "Etag": "W/\"00000000-0000-0000-0000-000000000000\"",
                "Id": "/subscriptions/<subscription id>/resourceGroups/<resource group name>/providers/Microsoft.Network/applicationGateways/<application gateway name>/backendAddressPools/pool02"
            }]


## <a name="unhealthy-instances-in-backendaddresspool"></a>在 BackendAddressPool 中不健康的实例

### <a name="cause"></a>原因

BackendAddressPool 的所有实例都都不正常，如果应用程序网关将不会包含与将用户请求路由到任何后端。 后端实例状态良好但不是具有所需的应用程序部署时，这可能也是这种情况。

### <a name="solution"></a>解决方案

确保实例正常并正确配置该应用程序。 检查后端实例是否能够从相同的 VNet 中的另一个虚拟机响应 ping。 如果配置了公用的结束点，确保浏览器请求 web 应用程序可维护性。

## <a name="problems-with-default-health-probe"></a>默认状况探测器的问题

### <a name="cause"></a>原因

502 错误也可以是默认健康探测器不能到达后端的虚拟机的频繁指示器。 配置应用程序网关实例时，它将自动配置默认健康探测器对每个 BackendAddressPool 使用的 BackendHttpSetting 属性。 不需要用户输入才能设置此探测器。 具体来说，当负载平衡规则配置时，BackendHttpSetting 和 BackendAddressPool 之间建立关联。 这些关联的每个配置默认探测和应用程序网关启动定期运行状况检查连接到在 BackendHttpSetting 元素中指定的端口 BackendAddressPool 中的每个实例。 下表列出了默认健康探测器与关联的值。


|探测器的属性 | 值 | 说明|
|---|---|---|
| 探测器的 URL| http://127.0.0.1/ | URL 路径 |
| 时间间隔 | 30 | 探查间隔 （秒） |
| 超时  | 30 | 探测超时秒数 |
| 不正常的阈值 | 3 | 探测器重试计数。 后端服务器已标记为关闭之后连续探测失败次数达到不健康的阈值。 |

### <a name="solution"></a>解决方案

- 请确保默认网站配置和正在监听 127.0.0.1。
- 如果 BackendHttpSetting 指定除 80 之外的端口，则应该配置默认站点侦听该端口。
- 对 http://127.0.0.1:port 的调用应返回的 HTTP 结果代码为 200。 这应在 30 秒的超时期间内返回。
- 确保配置的端口已打开，并且没有任何防火墙规则或 Azure 网络安全组，即阻止配置的端口上的传入或传出通信。
- 如果使用 FQDN 或公用 IP 使用 Azure 经典 Vm 或云服务，确保已打开对应的[终结点](../virtual-machines/virtual-machines-windows-classic-setup-endpoints.md)。
- 如果 VM 通过 Azure 资源管理器中配置，并且超出了 VNet 应用程序网关的部署位置，则必须配置[网络安全组](../virtual-network/virtual-networks-nsg.md)允许访问所需的端口。


## <a name="problems-with-custom-health-probe"></a>自定义运行状况探测器的问题

### <a name="cause"></a>原因

自定义运行状况探测器允许默认行为探测到更多的灵活性。 当使用自定义的探测，探测间隔、 URL，路径进行测试，并接受标记为不正常的后端池实例之前有多少失败的响应，用户可以配置。 添加下列附加属性。

|探测器的属性| 说明|
|---|---|
| 名称 | 该探测器的名称。 此名称用于指在后端 HTTP 设置探测器。 |
| 协议 | 用于发送探测器的协议。 该探测器将使用后端 HTTP 设置中定义的协议 |
| 主机 |  若要发送探测器的主机名。 多站点配置应用程序网关后，才适用。 这是不同于虚拟机的主机名。  |
| 路径 | 该探测器的相对路径。 从开始的有效路径 /。 该探测器将被发送到\<协议\>://\<主机\>:\<端口\>\<路径\> |
| 时间间隔 | 探测间隔，以秒为单位。 这是两个连续探测器之间的时间间隔。|
| 超时 | 探测器在秒后超时。 探针标记为失败如果此超时时间内未收到有效的响应。 |
| 不正常的阈值 | 探测器重试计数。 后端服务器已标记为关闭之后连续探测失败次数达到不健康的阈值。 |


### <a name="solution"></a>解决方案

验证自定义运行状况探测器上表中正确配置。 除了前面的故障诊断步骤，还确保︰

- 确保正确地将按照[指南](application-gateway-create-probe-ps.md)指定的探测器。
- 如果对单个网站配置应用程序网关，则默认情况下，主机应指定名称为"127.0.0.1"除非另外配置自定义的探测中。
- 确保对 http://\<主机\>:\<端口\>\<路径\>返回的 HTTP 结果代码为 200。
- 请确保间隔、 超时和 UnhealtyThreshold 都在可接受范围内。

## <a name="request-time-out"></a>请求超时

### <a name="cause"></a>原因

当收到用户请求时，应用程序网关配置的规则应用于请求并将其传送到后端池实例。 等待的时间响应的后端实例可配置的间隔。 默认情况下，此时间间隔为**30 秒**。 如果应用程序网关没有接收从后端应用程序在此间隔内的响应，用户请求会看到 502 错误。

### <a name="solution"></a>解决方案

应用程序网关允许用户配置此设置通过 BackendHttpSetting，可以再应用到不同的池。 不同的后端池可以有不同的 BackendHttpSetting 和因此不同请求超时配置。

    New-AzureRmApplicationGatewayBackendHttpSettings -Name 'Setting01' -Port 80 -Protocol Http -CookieBasedAffinity Enabled -RequestTimeout 60

## <a name="next-steps"></a>下一步行动

如果前面的步骤不能解决此问题，请打开的[支持票](https://azure.microsoft.com/support/options/)。
