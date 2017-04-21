<properties
   pageTitle="为创建一个基于路径的规则的应用程序网关通过门户网站 |Microsoft Azure"
   description="了解如何创建一条规则基于路径的应用程序网关通过门户网站"
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

# <a name="create-a-path-based-rule-for-an-application-gateway-by-using-the-portal"></a>通过门户网站中创建的应用程序网关基于路径的规则

> [AZURE.SELECTOR]
- [Azure 门户](application-gateway-create-url-route-portal.md)
- [资源管理器的 azure PowerShell](application-gateway-create-url-route-arm-ps.md)

基于路径的路由的 URL 时，可以将路由基于 Http 请求的 URL 路径相关联。 它检查是否有到后端池配置的 URL 列表中应用程序网关的路由，并将网络通信发送到定义的后端池。 基于 URL 的路由的一个常见用途是请求进行负载平衡对于不同的内容类型，对不同的后端服务器池。

基于 URL 的路由到网关应用程序引入新的规则类型。 应用程序网关有两种规则类型︰ 基本的和基于路径的规则。 基本规则类型的循环为提供服务的后端池循环分布以及基于路径的规则时，还要考虑请求 URL 的路径模式选择后端池时。

## <a name="scenario"></a>方案

下面的方案所经历中现有的应用程序网关创建基于路径的规则。
该方案假定，已创建[应用程序网关](application-gateway-create-gateway-portal.md)执行的步骤。

![url 路由][scenario]

## <a name="createrule"></a>创建基于路径的规则

基于路径的规则要求自己侦听器，创建规则一定要确认之前必须使用侦听器使用。

### <a name="step-1"></a>第 1 步

定位到 http://portal.azure.com，然后选择现有的应用程序网关。 单击**规则**

![应用程序网关概述][1]

### <a name="step-2"></a>第 2 步

请单击**基于路径的**按钮来添加一个新的基于路径的规则。

### <a name="step-3"></a>第 3 步

**添加基于路径的规则**刀片式服务器有两个部分。 第一部分是定义侦听程序、 规则和默认路径设置的名称的位置。 默认路径设置是不符合自定义的基于路径的路由的路由。 **添加基于路径的规则**刀片式服务器的第二部分是在其中定义的基于路径的规则本身。

**基本设置**

- **名称**-这是在门户中访问规则的友好名称。
- **侦听程序**-这是用于该规则的侦听器。
- **默认后端池**-此设置是定义后端要使用的默认规则的设置
- **默认 HTTP 设置**-此设置是定义的 HTTP 设置要使用的默认规则的设置。

**基于路径的规则**

- **名称**-这是一个友好的名称基于路径的规则。
- **路径**-此设置定义规则就会查找转发通信时的路径
- **后端池**-此设置是定义后端要使用的规则的设置
- **HTTP 设置**-此设置为所定义的 HTTP 设置要用于该规则的设置。

>[AZURE.IMPORTANT] 路径︰ 路径模式相匹配的列表。 必须从每个开始 / 的唯一的地方和"\*"允许在末尾。 有效的示例包括 /xyz，/xyz*或 /xyz/*。  

![添加基于路径的规则刀片式服务器的信息填写][2]

将基于路径的规则添加到现有应用程序网关是一个简单的过程，通过门户。 一旦创建了基于路径的规则，它可以编辑轻松地添加更多规则。 

![添加其他基于路径的规则][3]

## <a name="next-steps"></a>下一步行动

若要了解如何配置 SSL 卸载使用 Azure 应用程序网关，请参阅[配置 SSL 卸载](application-gateway-ssl-portal.md)

[1]: ./media/application-gateway-create-url-route-portal/figure1.png
[2]: ./media/application-gateway-create-url-route-portal/figure2.png
[3]: ./media/application-gateway-create-url-route-portal/figure3.png
[scenario]: ./media/application-gateway-create-url-route-portal/scenario.png