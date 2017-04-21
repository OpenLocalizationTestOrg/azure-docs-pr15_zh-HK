<properties
   pageTitle="装载多个站点在 Azure 的门户网站中的现有应用程序网关配置 |Microsoft Azure"
   description="此页提供如何配置为驻留多个 web 应用程序相同的网关使用 Azure 门户的现有 Azure 应用程序网关。"
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
   ms.date="10/25/2016"
   ms.author="gwallace"/>


# <a name="configure-an-existing-application-gateway-for-hosting-multiple-web-applications"></a>配置为驻留多个 web 应用程序的现有应用程序网关

> [AZURE.SELECTOR]
- [Azure 门户](application-gateway-create-multisite-portal.md)
- [资源管理器的 azure PowerShell](application-gateway-create-multisite-azureresourcemanager-powershell.md)

多站点宿主，可以部署多个 web 应用程序在同一应用程序网关。 它依赖于主机标头中的传入的 HTTP 请求，以确定哪些侦听器将接收通信的存在。 监听器然后把通信定向到适当的后端池配置规则的定义中的网关。 在启用 SSL web 应用程序，要选择正确的 web 通信监听器的服务器名称指示 (SNI) 扩展依赖于应用程序网关。 多站点宿主的一个常见用途是请求进行负载平衡不同的 web 域不同的后端服务器池。 同样也无法上同一应用程序网关承载多个相同的根域的子域。

## <a name="scenario"></a>方案

在以下示例中，应用程序网关服务于 contoso.com 和 fabrikam.com 的流量与两个后端服务器池︰ contoso 服务器池和 fabrikam 服务器池。 类似的安装程序可能用于到主机 app.contoso.com 和 blog.contoso.com 等的子域。

![多站点的方案][multisite]

## <a name="before-you-begin"></a>在开始之前

这种情况下将多站点支持添加到现有应用程序网关。 若要完成此方案需要可配置方案中的现有应用程序网关。 若要了解如何在门户中创建基本应用程序网关[创建通过使用门户应用程序网关](./application-gateway-create-gateway-portal.md)，请访问。

## <a name="requirements"></a>要求

- **后端服务器池︰**后端服务器的 IP 地址的列表。 列出的 IP 地址既应属于虚拟子网或应当是公共 IP/VIP。 此外可以使用 FQDN。
- **后端服务器池设置︰**每个池都有设置，如端口、 协议和基于 cookie 的关系等。 这些设置到池密切相关，并且应用于池中的所有服务器。
- **前端端口︰**此端口是打开的应用程序网关在公共端口。 流量达到此端口，，然后获取重定向到一台后端服务器。
- **侦听器︰**侦听器具有前端端口、 协议 （Http 或 Https，这些值是区分大小写），和 SSL 证书名称 （如果配置 SSL 卸载）。 多站点启用应用程序网关，还添加主机名称和 SNI 指示器。
- **规则︰**该规则绑定侦听器，该后端服务器池，并定义当它达到特定侦听器通信应定向到哪个后端服务器池。
- **证书︰**每个侦听器需要唯一的证书，在此示例中为多站点创建 2 侦听器。 需要创建两个.pfx 证书和它们的密码。

## <a name="create-an-application-gateway"></a>创建应用程序网关

更新应用程序网关所需的步骤如下︰

1. 创建的每个站点使用的后端池。
2. 对于每个站点的应用程序网关将支持创建一个新的侦听器。
3. 创建规则以映射与适当的后端的每个侦听器。

## <a name="create-back-end-pools-for-each-site"></a>创建每个网站的后端池

每个网站的后端池的应用程序网关将需要支持，在这种情况下 2 将创建，分别为 contoso11.com 和 fabrikam11.com。

### <a name="step-1"></a>第 1 步

导航到 Azure 门户 (https://portal.azure.com) 中的现有应用程序网关。 选择**池后端**，然后单击**添加**

![添加后端池][7]

### <a name="step-2"></a>第 2 步

填写后端池**池 1**，添加后端服务器的 ip 地址或 Fqdn 的信息，然后单击**确定**

![后端池池 1 设置][8]

### <a name="step-3"></a>第 3 步

后端池刀片上单击**添加**以添加附加的后端池**pool2**，为后端服务器添加 ip 地址或 FQDN，然后单击**确定**

![后端池 pool2 设置][9]

### <a name="create-listeners-for-each-back-end"></a>创建每个后端的侦听器

应用程序网关依赖于 HTTP 1.1 主机标头来承载多个网站上的同一个公用 IP 地址和端口。 在门户中创建基本侦听器不包含该属性。

### <a name="step-1"></a>第 1 步

单击现有应用程序网关在**侦听器**，然后单击**多站点**添加第一个侦听程序。

![侦听程序概述刀片式服务器][1]

### <a name="step-2"></a>第 2 步

配置侦听器，SSL 端接本示例的信息，请填写，创建一个新的前端端口。 上传的.pfx 证书用于 SSL 端接。 与标准基本侦听器刀片式服务器此刀片式服务器上的唯一区别是主机名。

![侦听器属性刀片式服务器][2]

### <a name="step-3"></a>第 3 步

单击**多站点**并创建另一个侦听器的第二个站点上一步中所述。 请确保对第二个侦听器使用不同的证书。 与标准基本侦听器刀片式服务器此刀片式服务器上的唯一区别是主机名。 填写该侦听器的信息并单击**确定**。

![侦听器属性刀片式服务器][3]

> [AZURE.NOTE] 创建侦听器在 Azure 门户应用程序网关是一个长时间运行的任务，它可能需要一些时间，在这种情况下创建这两个侦听器。 何时完成侦听器显示如下图中所示的门户。

![侦听程序概述][4]

### <a name="create-rules-to-map-listeners-to-backend-pools"></a>创建规则以映射到后端池的侦听器

### <a name="step-1"></a>第 1 步

导航到 Azure 门户 (https://portal.azure.com) 中的现有应用程序网关。 选择**规则**并选择现有的默认规则**rule1**单击**编辑**。

### <a name="step-2"></a>第 2 步

填写规则刀片式服务器，如下面的图像所示。 选择第一个侦听程序与第一个池，然后单击**保存**完成后。

![编辑现有规则][6]

### <a name="step-3"></a>第 3 步

单击要创建第二个规则的**基本规则**。 填写窗体与第二个侦听器和第二个后端库并单击**确定**以保存。

![添加基本的规则刀片式服务器][10]

这将完成使用多站点支持通过 Azure 门户配置现有应用程序网关。

## <a name="next-steps"></a>下一步行动

了解如何保护您的网站使用[应用程序网关的 Web 应用程序防火墙](application-gateway-webapplicationfirewall-overview.md)

<!--Image references-->
[1]: ./media/application-gateway-create-multisite-portal/figure1.png
[2]: ./media/application-gateway-create-multisite-portal/figure2.png
[3]: ./media/application-gateway-create-multisite-portal/figure3.png
[4]: ./media/application-gateway-create-multisite-portal/figure4.png
[5]: ./media/application-gateway-create-multisite-portal/figure5.png
[6]: ./media/application-gateway-create-multisite-portal/figure6.png
[7]: ./media/application-gateway-create-multisite-portal/figure7.png
[8]: ./media/application-gateway-create-multisite-portal/figure8.png
[9]: ./media/application-gateway-create-multisite-portal/figure9.png
[10]: ./media/application-gateway-create-multisite-portal/figure10.png
[multisite]: ./media/application-gateway-create-multisite-portal/multisite.png