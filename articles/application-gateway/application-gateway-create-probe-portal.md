<properties
   pageTitle="通过门户网站创建的自定义探测应用程序网关 |Microsoft Azure"
   description="了解如何为应用程序网关创建自定义的探测，通过门户网站"
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

# <a name="create-a-custom-probe-for-application-gateway-by-using-the-portal"></a>为应用程序网关创建自定义的探测，通过门户网站

> [AZURE.SELECTOR]
- [Azure 门户](application-gateway-create-probe-portal.md)
- [资源管理器的 azure PowerShell](application-gateway-create-probe-ps.md)
- [Azure 的经典 PowerShell](application-gateway-create-probe-classic-ps.md)

[AZURE.INCLUDE [azure-probe-intro-include](../../includes/application-gateway-create-probe-intro-include.md)]

## <a name="scenario"></a>方案

在现有应用程序网关创建自定义运行状况探测器经过以下情形。
该方案假定，已创建[应用程序网关](application-gateway-create-gateway-portal.md)执行的步骤。

## <a name="createprobe"></a>创建探测器

探测器在一个两步过程通过门户配置。 第一步是创建探测器下, 一步将探测器添加到应用程序网关后端 http 设置。

### <a name="step-1"></a>第 1 步

定位到 http://portal.azure.com，然后选择现有的应用程序网关。

![应用程序网关概述][1]

### <a name="step-2"></a>第 2 步

单击**探测**并单击**添加**按钮以添加新的探测器。

![填写信息以添加探测器刀片式服务器][2]

### <a name="step-3"></a>第 3 步

填写该探测器所需的信息，完成后单击**确定**。

- **名称**-这是访问门户中探测到的友好名称。
- **主机**-这是用于探测的主机名称。 适用仅在多站点上配置应用程序网关，否则使用"127.0.0.1"。 这是不同于虚拟机的主机名。
- **路径**的自定义探测的完整 url 的其余部分。 一个有效的路径开头 '/'
- **时间间隔 （秒）** -该探测器的运行频率，以检查运行状况。 不建议设置低超过 30 秒。
- **超时 （秒）** — 探测器在超时前等待的时间量。 超时时间间隔必须足够高，可以进行 http 调用以确保后端健康网页都有。
- **不正常阈值**-被认为是不正常的失败尝试的次数。 阈值为 0，则表示，如果健康检查失败后端将确定不正常立即。

> [AZURE.IMPORTANT] 主机名不是服务器名称。 这是主机的虚拟应用程序服务器上运行的名称。 该探测器将被发送到 http://(host name):(port from httpsetting)/urlPath

![探测器的配置设置][3]

## <a name="add-probe-to-the-gateway"></a>将探测器添加到网关

既然已经创建了探测器，就可以将其添加到网关。 探测器设置上的后端 http 应用程序网关设置。

### <a name="step-1"></a>第 1 步

单击应用程序网关， **HTTP 设置**，然后单击当前后端 http 设置窗口中的将显示配置刀片式服务器。

![https 设置窗口][4]

### <a name="step-2"></a>第 2 步

**AppGatewayBackEndHttp**设置刀片式服务器，请单击**使用自定义的探测**和选择在[创建探测器](#createprobe)部分中创建的探测器。
完成后，单击**确定**，并应用这些设置。

![appgatewaybackend 设置刀片式服务器][5]

默认探测器检查 web 应用程序的默认访问权限。 既然已经创建了自定义的探测，应用程序网关使用自定义路径定义要监视所选后端的运行状况。 根据定义的条件时，应用程序网关检查探测器中指定的文件。 如果主机 / 端口对的调用 / 路径不会返回一个 Http 200 确定状态响应，服务器离开旋转后达到不正常阈值。 探测仍然存在不健康的实例，来确定它将再次成为健康上。 一旦实例添加回到正常服务器池通信开始再次流动到它，并探测到的实例继续作为普通用户指定的时间间隔。


## <a name="next-steps"></a>下一步行动

若要了解如何配置 SSL 卸载使用 Azure 应用程序网关，请参阅[配置 SSL 卸载](application-gateway-ssl-portal.md)

[1]: ./media/application-gateway-create-probe-portal/figure1.png
[2]: ./media/application-gateway-create-probe-portal/figure2.png
[3]: ./media/application-gateway-create-probe-portal/figure3.png
[4]: ./media/application-gateway-create-probe-portal/figure4.png
[5]: ./media/application-gateway-create-probe-portal/figure5.png