<properties
   pageTitle="创建使用门户应用程序网关 |Microsoft Azure"
   description="了解如何创建应用程序网关，通过门户网站"
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

# <a name="create-an-application-gateway-by-using-the-portal"></a>通过门户网站中创建的应用程序网关

> [AZURE.SELECTOR]
- [Azure 门户](application-gateway-create-gateway-portal.md)
- [资源管理器的 azure PowerShell](application-gateway-create-gateway-arm.md)
- [Azure 的经典 PowerShell](application-gateway-create-gateway.md)
- [Azure 的资源管理器模板](application-gateway-create-gateway-arm-template.md)
- [Azure CLI](application-gateway-create-gateway-cli.md)

Azure 应用程序网关是一个 7 层负载平衡器。 无论是对云的内部，它提供了故障切换时，不同的服务器之间的路由性能的 HTTP 请求。 应用程序网关提供了许多应用程序交付控制器 (ADC) 功能，其中包括 HTTP 负载平衡、 基于 cookie 的会话相关性，安全套接字层 (SSL) 卸载自定义运行状况探测器、 支持多站点，和许多其他人。 若要查找受支持的功能的完整列表，请访问[应用程序网关概述](application-gateway-introduction.md)

## <a name="scenario"></a>方案

在此方案中，您将学习如何创建使用 Azure 的门户应用程序网关。

该方案将︰

- 创建两个实例中应用程序网关。
- 创建名为 AdatumAppGatewayVNET 与 10.0.0.0/16 保留 CIDR 块的虚拟网络。
- 创建名为 Appgatewaysubnet，使用 10.0.0.0/28 作为其 CIDR 块的子网。
- 配置 SSL 卸载证书。

![方案示例][scenario]

>[AZURE.IMPORTANT] 应用程序网关，包括自定义运行状况的其他配置探测，配置应用程序网关后，并不是在初始部署过程配置后端池地址和其他规则。

## <a name="before-you-begin"></a>在开始之前

Azure 应用程序网关需要各自的子网上。 当创建虚拟网络时，请确保保留足够具有多个子网的地址空间。 一旦部署到的子网使用应用程序网关，仅附加应用程序网关都能够添加到子网。

## <a name="create-the-application-gateway"></a>创建应用程序网关

### <a name="step-1"></a>第 1 步

导航到 Azure 的门户，单击**新建** > **网络** > **应用程序网关**

![创建应用程序网关][1]

### <a name="step-2"></a>第 2 步

接下来填写有关应用程序网关的基本信息。 完成后单击**确定**

基本设置所需的信息是︰

- **名称**-应用程序网关的名称。
- **层**-这是应用程序网关层。 两层都可用， **WAF**和**标准**。 WAF 使 web 应用程序防火墙功能。
- **SKU 大小**-此设置是应用程序网关的大小、 可用选项为 （**小型**、**中型**和**大型**）。 *小 WAF 层选择时不可用*
- **实例计数**的实例数，此值应为 2 到 10 之间的数字。
- **资源组**的资源组用于保存应用程序网关，它可以是现有资源组或新建一个。
- **位置**-为应用程序网关，该地区是在资源组中的同一位置。 *的位置是重要的虚拟网络和公用 IP 地址必须在相同的位置作为网关*。

![刀片式服务器显示基本设置][2]

>[AZURE.NOTE] 出于测试目的，可以选择实例计数为 1。 务必要知道两个实例在任何实例计数不受 SLA，因此不建议使用。 小网关将被用于开发测试而不是用于生产目的。

### <a name="step-3"></a>第 3 步

一旦定义了基本的设置下, 一步是定义要使用的虚拟网络。 虚拟的网络包括平衡以加载应用程序网关应用程序。

单击**选择一个虚拟的网络**来配置虚拟网络。

![刀片式服务器显示应用程序网关设置][3]

### <a name="step-4"></a>第 4 步

在*选择虚拟网络*刀片式服务器，单击**创建新**

虽然不在此方案中所述，可此时选择现有的虚拟网络。  如果使用现有的虚拟网络，则务必知道虚拟网络需要空的子网或仅应用程序网关资源使用的子网。

![选择虚拟网络刀片式服务器][4]

### <a name="step-5"></a>第 5 步

上述[方案](#scenario)说明中所述，请填写刀片式服务器**创建虚拟网络**中的网络信息。

![使用输入的信息创建虚拟网络刀片式服务器][5]

### <a name="step-6"></a>第 6 步

创建虚拟网络后下, 一步是定义的前端应用程序网关 IP。 在这种情况下，选择是一个公共或专用的 IP 地址，用于前端之间。 具体选择取决于应用程序是否互联网对开或内部只。 此方案假定使用一个公共 IP 地址。 若要选择一个专用的 IP 地址，可以单击**专用**按钮。 选择自动分配的 IP 地址，也可以单击**选择特定的专用 IP 地址**复选框以便手动输入一个。

### <a name="step-7"></a>第 7 步

单击**选择一个公用 IP 地址**。 如果现有的公用 IP 地址可用在此时选择，在此方案中，您创建一个新的公用 IP 地址。 单击**创建**

![选择公用 IP 地址刀片式服务器][6]

### <a name="step-8"></a>第 8 步

为公用 IP 地址提供一个友好的名称，然后单击**确定**

![创建公用 IP 地址刀片式服务器][7]

### <a name="step-9"></a>第 9 步

最后一个配置时创建的应用程序网关是侦听器配置的设置。  如果使用**http** ，则毫无剩余配置，可以单击**确定**。 若要使用**https**进一步配置是必需的。

若要使用**https**，证书是必需的。 需要证书的专用密钥，以便一个.pfx 导出证书和密码的需要提供。

### <a name="step-10"></a>第 10 步

单击**HTTPS**，单击**上载 PFX 证书**文本框旁边的**文件夹**图标。
导航到文件系统上的.pfx 证书文件。 一旦选定，给证书一个好记的名称，然后键入.pfx 文件的密码。

完成之后，单击**确定**以查看应用程序网关设置。

![设置刀片式服务器上的侦听器配置节][9]

### <a name="step-11"></a>第 11 步

查看摘要页，然后单击**确定**。  现在应用程序网关是排队并创建。

### <a name="step-12"></a>第 12 步

一旦创建应用程序网关，导航到该门户网站继续应用程序网关配置中。

![应用程序网关资源视图][10]

这些步骤创建基本应用程序网关使用侦听器、 后端库、 后端的 http 设置和规则的默认设置。 您可以修改这些设置，以适合您的部署设置后成功。

## <a name="next-steps"></a>下一步行动

该应用场景创建的默认应用程序网关。 接下来的步骤是配置应用程序网关，通过添加池成员，修改设置，并调整规则中的网关才能正常工作。

了解如何通过[创建自定义运行状况探测器](application-gateway-create-probe-portal.md)访问创建自定义运行状况探测器

了解如何配置 SSL 卸载并采取代价高昂的 SSL 解密，关闭您的 web 服务器访问[配置 SSL 卸载](application-gateway-ssl-portal.md)

了解如何保护您的应用程序与[Web 应用程序防火墙](application-gateway-webapplicationfirewall-overview.md)功能的应用程序网关。

<!--Image references-->
[1]: ./media/application-gateway-create-gateway-portal/figure1.png
[2]: ./media/application-gateway-create-gateway-portal/figure2.png
[3]: ./media/application-gateway-create-gateway-portal/figure3.png
[4]: ./media/application-gateway-create-gateway-portal/figure4.png
[5]: ./media/application-gateway-create-gateway-portal/figure5.png
[6]: ./media/application-gateway-create-gateway-portal/figure6.png
[7]: ./media/application-gateway-create-gateway-portal/figure7.png
[8]: ./media/application-gateway-create-gateway-portal/figure8.png
[9]: ./media/application-gateway-create-gateway-portal/figure9.png
[10]: ./media/application-gateway-create-gateway-portal/figure10.png
[scenario]: ./media/application-gateway-create-gateway-portal/scenario.png
