<properties
   pageTitle="创建 web 应用程序防火墙使用门户应用程序网关 |Microsoft Azure"
   description="了解如何创建具有 web 应用程序防火墙应用程序网关，通过门户网站"
   services="application-gateway"
   documentationCenter="na"
   authors="georgewallace"
   manager="carmonm"
   editor="tysonn"
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

# <a name="create-an-application-gateway-with-web-application-firewall-by-using-the-portal"></a>创建与 web 应用程序防火墙应用程序网关，通过门户网站

> [AZURE.SELECTOR]
- [Azure 门户](application-gateway-web-application-firewall-portal.md)
- [资源管理器的 azure PowerShell](application-gateway-web-application-firewall-powershell.md)

Web 应用程序防火墙 (WAF) 在 Azure 应用程序网关保护 web 应用程序中常见的基于 web 的攻击如 SQL 注入、 跨站点脚本攻击和会话劫持。 Web 应用程序可以防止许多 OWASP 顶部 10 常见 web 漏洞。

Azure 应用程序网关是一个 7 层负载平衡器。 无论是对云的内部，它提供了故障切换时，不同的服务器之间的路由性能的 HTTP 请求。
应用程序提供了许多应用程序交付控制器 (ADC) 功能，其中包括 HTTP 负载平衡、 基于 cookie 的会话相关性，安全套接字层 (SSL) 卸载自定义运行状况探测器、 支持多站点，和许多其他人。
若要查找受支持的功能的完整列表，请访问[应用程序网关概述](application-gateway-introduction.md)

## <a name="scenarios"></a>方案

在这篇文章中有两种情况︰

在第一个方案中，您将学习到[添加到现有应用程序网关的 web 应用程序防火墙](#add-web-application-firewall-to-an-existing-application-gateway)。

在第二个方案中，您学习[创建与 web 应用程序防火墙应用程序网关](#create-an-application-gateway-with-web-application-firewall)

![方案示例][scenario]

>[AZURE.NOTE] 应用程序网关，包括自定义运行状况的其他配置探测，配置应用程序网关后，并不是在初始部署过程配置后端池地址和其他规则。

## <a name="before-you-begin"></a>在开始之前

Azure 应用程序网关需要各自的子网上。 当创建虚拟网络时，请确保保留足够具有多个子网的地址空间。 一旦部署到的子网使用应用程序网关，仅附加应用程序网关都能够添加到子网。

## <a name="add-web-application-firewall-to-an-existing-application-gateway"></a>将 web 应用程序防火墙添加到现有应用程序网关

这种情况下更新现有应用程序网关来保护模式中支持 web 应用程序防火墙。

### <a name="step-1"></a>第 1 步

导航到 Azure 的门户，请选择现有的应用程序网关。

![创建应用程序网关][1]

### <a name="step-2"></a>第 2 步

单击**配置**，然后更新应用程序网关设置。 完成后单击**保存**

若要更新现有应用程序网关以支持 web 应用程序防火墙的设置为︰

- **层**-层选择必须**WAF**以支持 web 应用程序防火墙
- **SKU 大小**-此设置是 web 应用程序防火墙应用程序网关的大小、 可用的选项包括 （**中型**和**大型**）。
- **防火墙状态**-此设置禁用，或使 web 应用程序防火墙。
- **防火墙模式**-此设置是 web 应用程序防火墙如何处理恶意通信量。 **检测**模式只是记录的事件，其中**预防**模式记录事件和停止恶意通信量。

![刀片式服务器显示基本设置][2]

>[AZURE.NOTE] 若要查看 web 应用程序防火墙日志，必须启用诊断并选择 ApplicationGatewayFirewallLog。 出于测试目的，可以选择实例计数为 1。 务必要知道两个实例在任何实例计数不受 SLA，因此不建议使用。 使用 web 应用程序防火墙时，小网关将不可用。

## <a name="create-an-application-gateway-with-web-application-firewall"></a>创建 web 应用程序防火墙应用程序网关

该方案将︰

- 创建两个实例中的 web 应用程序防火墙应用程序网关。
- 创建名为 AdatumAppGatewayVNET 与 10.0.0.0/16 保留 CIDR 块的虚拟网络。
- 创建名为 Appgatewaysubnet，使用 10.0.0.0/28 作为其 CIDR 块的子网。
- 配置 SSL 卸载证书。

### <a name="step-1"></a>第 1 步

导航到 Azure 的门户，单击**新建** > **网络** > **应用程序网关**

![创建应用程序网关][1-1]

### <a name="step-2"></a>第 2 步

接下来填写有关应用程序网关的基本信息。 请务必选择**WAF**与层。 完成后单击**确定**

基本设置所需的信息是︰

- **名称**-应用程序网关的名称。
- **层**-层应用程序网关，可用选项包括 （**标准**和**WAF**）。 Web 应用程序防火墙才可用 WAF 层中。
- **SKU 大小**-此设置是应用程序网关的大小、 可用的选项包括 （**中型**和**大型**）。
- **实例计数**的实例数，此值应为**2**到**10**之间的数字。
- **资源组**的资源组用于保存应用程序网关，它可以是现有资源组或新建一个。
- **位置**-为应用程序网关，该地区是在资源组中的同一位置。 *的位置是重要的虚拟网络和公用 IP 地址必须在相同的位置作为网关*。

![刀片式服务器显示基本设置][2-2]

>[AZURE.NOTE] 出于测试目的，可以选择实例计数为 1。 务必要知道两个实例在任何实例计数不受 SLA，因此不建议使用。 Web 应用程序防火墙方案不支持小型的网关。

### <a name="step-3"></a>第 3 步

一旦定义了基本的设置下, 一步是定义要使用的虚拟网络。 虚拟的网络包括平衡以加载应用程序网关应用程序。

单击**选择一个虚拟的网络**来配置虚拟网络。

![刀片式服务器显示应用程序网关设置][3]

### <a name="step-4"></a>第 4 步

在**选择虚拟网络**刀片式服务器，单击**创建新**

虽然不在此方案中所述，可此时选择现有的虚拟网络。  如果使用现有的虚拟网络，则务必知道虚拟网络需要空的子网或仅应用程序网关资源使用的子网。

![选择虚拟网络刀片式服务器][4]

### <a name="step-5"></a>第 5 步

上述[方案](#scenario)说明中所述，请填写刀片式服务器**创建虚拟网络**中的网络信息。

![使用输入的信息创建虚拟网络刀片式服务器][5]

### <a name="step-6"></a>第 6 步

创建虚拟网络后下, 一步是定义的前端应用程序网关 IP。 在这种情况下，选择是一个公共或专用的 IP 地址，用于前端之间。 具体选择取决于应用程序是否互联网对开或内部只。 此方案假定使用一个公共 IP 地址。 若要选择一个专用的 IP 地址，可以单击**专用**按钮。 选择自动分配的 IP 地址，也可以单击**选择特定的专用 IP 地址**复选框以便手动输入一个。

单击**选择一个公用 IP 地址**。 如果现有的公用 IP 地址可用在此时选择，在此方案中，您创建一个新的公用 IP 地址。 单击**创建**

![选择公用 IP 地址刀片式服务器][6]

### <a name="step-7"></a>第 7 步

为公用 IP 地址提供一个友好的名称，然后单击**确定**

![创建公用 IP 地址刀片式服务器][7]

### <a name="step-8"></a>第 8 步

接下来，您可以设置侦听器配置。  如果使用**http** ，则毫无剩余配置，可以单击**确定**。 若要使用**https**进一步配置是必需的。

若要使用**https**，证书是必需的。 需要证书的专用密钥，以便.pfx 导出证书需要提供和文件的密码。

单击**HTTPS**，单击**上载 PFX 证书**文本框旁边的**文件夹**图标。
导航到文件系统上的.pfx 证书文件。 一旦选定，给证书一个好记的名称，然后键入.pfx 文件的密码。

完成之后，单击**确定**以查看应用程序网关设置。

![设置刀片式服务器上的侦听器配置节][8]

### <a name="step-9"></a>第 9 步

配置**WAF**特定设置。

- **防火墙状态**-此设置打开或关闭的 WAF。
- **防火墙模式**-此设置确定操作 WAF 采用恶意通信量。 如果选择**检测**，则只记录通信。  如果选择了**预防**，通信记录，和停止 403 未经授权使用。

![web 应用程序防火墙设置][9]

### <a name="step-10"></a>第 10 步

查看摘要页，然后单击**确定**。  现在应用程序网关是排队并创建。

### <a name="step-12"></a>第 12 步

一旦创建应用程序网关，导航到该门户网站继续应用程序网关配置中。

![应用程序网关资源视图][10]

这些步骤创建基本应用程序网关使用侦听器、 后端库、 后端的 http 设置和规则的默认设置。 您可以修改这些设置，以适合您的部署，成功调配后

## <a name="next-steps"></a>下一步行动

了解如何配置诊断日志记录来记录与 Web 应用程序防火墙的事件被检测到或无法访问[应用程序网关诊断](application-gateway-diagnostics.md)

了解如何通过[创建自定义运行状况探测器](application-gateway-create-probe-portal.md)访问创建自定义运行状况探测器

了解如何配置 SSL 卸载并采取代价高昂的 SSL 解密，关闭您的 web 服务器访问[配置 SSL 卸载](application-gateway-ssl-portal.md)

<!--Image references-->
[1]: ./media/application-gateway-web-application-firewall-portal/figure1.png
[2]: ./media/application-gateway-web-application-firewall-portal/figure2.png
[1-1]: ./media/application-gateway-web-application-firewall-portal/figure1-1.png
[2-2]: ./media/application-gateway-web-application-firewall-portal/figure2-2.png
[3]: ./media/application-gateway-web-application-firewall-portal/figure3.png
[4]: ./media/application-gateway-web-application-firewall-portal/figure4.png
[5]: ./media/application-gateway-web-application-firewall-portal/figure5.png
[6]: ./media/application-gateway-web-application-firewall-portal/figure6.png
[7]: ./media/application-gateway-web-application-firewall-portal/figure7.png
[8]: ./media/application-gateway-web-application-firewall-portal/figure8.png
[9]: ./media/application-gateway-web-application-firewall-portal/figure9.png
[10]: ./media/application-gateway-web-application-firewall-portal/figure10.png
[scenario]: ./media/application-gateway-web-application-firewall-portal/scenario.png