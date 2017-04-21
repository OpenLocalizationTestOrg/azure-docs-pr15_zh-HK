<properties
   pageTitle="通过门户网站配置 SSL 卸载的应用程序网关 |Microsoft Azure"
   description="此页提供使用 SSL 创建应用程序网关的说明卸载通过门户网站"
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

# <a name="configure-an-application-gateway-for-ssl-offload-by-using-the-portal"></a>通过门户网站配置 SSL 卸载的应用程序网关

> [AZURE.SELECTOR]
-[Azure 的门户网站](application-gateway-ssl-portal.md)
-[Azure 资源管理器 PowerShell](application-gateway-ssl-arm.md)
-[Azure 经典 PowerShell](application-gateway-ssl.md)

可以将 azure 应用程序网关配置为终止在网关，以避免代价高昂的 SSL 解密任务在 web 场中发生的安全套接字层 (SSL) 会话。 前端服务器设置和 web 应用程序的管理，还简化了 SSL 卸载。

## <a name="scenario"></a>方案

下面的方案经过配置 SSL 卸载现有的应用程序网关。 该方案假定，已创建[应用程序网关](application-gateway-create-gateway-portal.md)执行的步骤。

## <a name="before-you-begin"></a>在开始之前

若要使用应用程序网关配置 SSL 卸载，证书是必需的。 此证书是加载应用程序网关，用来加密和解密通过 SSL 发送的通信。 证书必须在个人信息交换 (pfx) 格式。 这种文件格式允许为要导出的私钥所必需的应用程序网关执行加密和解密的通信量。

## <a name="add-an-https-listener"></a>添加 HTTPS 侦听器

HTTPS 侦听器查找基于其配置通信，并可帮助将通信路由到后端池。

### <a name="step-1"></a>第 1 步

Azure 门户导航并选择现有的应用程序网关

### <a name="step-2"></a>第 2 步

单击侦听器，然后单击 Add 按钮以添加一个侦听器。

![应用程序网关概述刀片式服务器][1]

### <a name="step-3"></a>第 3 步

填写所需信息的侦听器和上载该.pfx 证书，完成后单击确定。

**名称**-此值是侦听器的一个友好名称。

**前端 IP 配置**-此值是用于侦听程序的前端 IP 配置。

**前端端口 （名称/端口）**的前端应用程序网关和使用的实际端口上使用的端口的友好名称。

**协议**-确定是否 https 或 http 用于前端的开关。

使用**证书 （名称/密码）** -如果 SSL 卸载，.pfx 证书时需要此设置而且好记的名称和密码是必需的。

![添加侦听器刀片式服务器][2]

## <a name="create-a-rule-and-associate-it-to-the-listener"></a>创建规则并将其关联到侦听器

现在已创建侦听器。 它是创建规则以处理来自监听器通信时间。

### <a name="step-1"></a>第 1 步

单击应用程序网关，这些**规则**，然后单击添加。

![应用程序网关规则刀片式服务器][3]

### <a name="step-2"></a>第 2 步

**添加基本规则**刀片式服务器，键入规则的友好名称，选择在先前步骤中创建的侦听器。 选择适当的后端池和 http 设置，然后单击**确定**

![https 设置窗口][4]

现在，设置将保存到应用程序网关。 有关这些设置可能会需要一段时间可以用来查看通过门户或通过 PowerShell 之前，保存过程。 一旦保存应用程序网关处理的加密和解密的通信量。 应用程序网关和后端 web 服务器之间的所有通信将通过 http 进行都处理。 任何回客户端，如果启动通过 https 通信将返回到客户端加密。

## <a name="next-steps"></a>下一步行动

若要了解如何配置自定义运行状况探测器 Azure 应用程序网关，请参阅[创建自定义运行状况探测器](application-gateway-create-gateway-portal.md)。

[1]: ./media/application-gateway-ssl-portal/figure1.png
[2]: ./media/application-gateway-ssl-portal/figure2.png
[3]: ./media/application-gateway-ssl-portal/figure3.png
[4]: ./media/application-gateway-ssl-portal/figure4.png