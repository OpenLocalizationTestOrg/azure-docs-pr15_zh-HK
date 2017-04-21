

<properties
   pageTitle="运行状况监视 Azure 应用程序网关概述 |Microsoft Azure"
   description="了解有关在 Azure 应用程序网关监控能力"
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
   ms.date="10/25/2016"
   ms.author="gwallace" />

# <a name="application-gateway-health-monitoring-overview"></a>应用程序网关运行状况监视概述

默认的 azure 应用程序网关监控及其后端库中的所有资源的运行状况并自动删除任何资源池中的线程被视为不正常。 应用程序网关继续监视不健康的实例并将它们添加回正常的后端池，一旦它们变得可用，并响应健康探测器。 应用程序网关发送运行状况与后端 HTTP 设置中定义的相同端口探测。 这将确保探测器正在测试将使用客户连接到后端的同一端口。

![应用程序网关探测示例][1]

除了使用默认运行状况探测监视，您可以自定义以适合您的应用程序要求运行状况探测器。 在本文中，介绍了默认和自定义运行状况探测器。

## <a name="default-health-probe"></a>默认状况探测器

不设置任何自定义探测配置时，使用应用程序网关自动配置默认健康探测器。 通过 HTTP 请求的 IP 地址配置为后端池工作的监视行为。 对于默认探测器如果后端 http 设置被配置为支持 HTTPS，探测器将使用 https 也测试 backends 的运行状况。

例如︰ 您可以配置您的应用程序网关使用 A、 B 和 C 的后端服务器接收 HTTP 端口 80 上的网络通信。 默认运行状况监视测试三个服务器每隔 30 秒钟，正常的 HTTP 响应。 正常的 HTTP 响应具有[状态码](https://msdn.microsoft.com/library/aa287675.aspx)200 和 399 之间。

如果服务器 A 的默认探测器检查失败，应用程序网关会将其移除其后台池中，和网络通信流停止流动到此服务器。 默认探测器仍继续检查服务器每隔 30 秒。 服务器的响应时成功对一个请求从默认健康探测器，到后端库，将被添加回正常运行，通讯开始再次流动到服务器。

### <a name="default-health-probe-settings"></a>默认的健全探测器设置

|探测器的属性 | 值 | 说明|
|---|---|---|
| 探测器的 URL| http://127.0.0.1:\<端口\>/ | URL 路径 |
| 时间间隔 | 30 | 探查间隔 （秒） |
| 超时  | 30 | 探测超时秒数 |
| 不正常的阈值 | 3 | 探测器重试计数。 后端服务器已标记为关闭之后连续探测失败次数达到不健康的阈值。 |

> [AZURE.NOTE] 端口将始终与后端 HTTP 设置相同的端口。

默认探测只查看 http://127.0.0.1:\<端口\>以确定健康状况。 如果您需要配置健康探测器可转到自定义的 URL 或修改其他任何设置，则必须使用自定义探测如以下步骤所述。

## <a name="custom-health-probe"></a>自定义运行状况探测器

自定义探测让您可以更精确地控制运行状况监视。 在使用自定义探测时，您可以配置探测间隔时，URL 路径进行测试，并接受标记为不正常的后端池实例之前有多少失败的响应。

### <a name="custom-health-probe-settings"></a>自定义运行状况探测器设置

下表提供了定义属性的自定义运行状况探测器。

|探测器的属性| 说明|
|---|---|
| 名称 | 该探测器的名称。 此名称用于指在后端 HTTP 设置探测器。 |
| 协议 | 用于发送探测器的协议。 该探测器将使用后端 HTTP 设置中定义的协议 |
| 主机 |  若要发送探测器的主机名。 适用仅在多站点上配置应用程序网关，否则使用"127.0.0.1"。 这是不同于虚拟机的主机名。 |
| 路径 | 该探测器的相对路径。 从开始的有效路径 /。 |
| 时间间隔 | 探测间隔，以秒为单位。 这是两个连续探测器之间的时间间隔。|
| 超时 | 探测器在秒后超时。 探针标记为失败如果此超时时间内未收到有效的响应。 |
| 不正常的阈值 | 探测器重试计数。 后端服务器已标记为关闭之后连续探测失败次数达到不健康的阈值。 |

> [AZURE.IMPORTANT] 如果对单个网站配置应用程序网关，则默认情况下，主机应指定名称为"127.0.0.1"除非另外配置自定义的探测中。
引用的自定义探测发送到\<协议\>://\<主机\>:\<端口\>\<路径\>。 所用的端口将同一个端口，如后端 HTTP 设置中定义。

## <a name="next-steps"></a>下一步行动

后学习应用程序网关运行状况监视，可以在 Azure 门户或[自定义运行状况探测器](application-gateway-create-probe-ps.md)使用 PowerShell 和 Azure 资源管理器部署模型中配置[自定义运行状况探测器](application-gateway-create-probe-portal.md)。

[1]: ./media/application-gateway-probe-overview/appgatewayprobe.png