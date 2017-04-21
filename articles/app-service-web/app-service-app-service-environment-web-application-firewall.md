<properties 
    pageTitle="为应用程序服务环境配置 Web 应用程序防火墙 (WAF)" 
    description="了解如何配置 web 应用程序防火墙前面您应用程序的服务环境。" 
    services="app-service\web" 
    documentationCenter="" 
    authors="naziml" 
    manager="wpickett" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="08/17/2016" 
    ms.author="naziml"/>    

# <a name="configuring-a-web-application-firewall-waf-for-app-service-environment"></a>为应用程序服务环境配置 Web 应用程序防火墙 (WAF)

## <a name="overview"></a>概述 ##
Web 应用程序防火墙类似[Azure 的 Barracuda WAF](https://www.barracuda.com/programs/azure)可用的[Azure 市场](https://azure.microsoft.com/marketplace/partners/barracudanetworks/waf-byol/)协助下安全 web 应用程序通过检查入站 web 通信，阻止 SQL 注入、 跨站点脚本、 恶意软件上载和应用 DDoS 程序和其他攻击。 它还会检查从后端 web 服务器的响应数据丢失防护 (DLP)。 这再加上隔离和其他缩放提供应用程序服务环境，提供了到主机业务关键的 web 应用程序不受恶意请求和高容量的通信所需要的理想环境。

+[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 

## <a name="setup"></a>安装程序 ##
对于本文，我们将配置我们后面多个负载的应用程序服务环境平衡 Barracuda WAF 的实例，以便仅从 WAF 的流量可以访问应用程序服务环境，它将无法访问从 DMZ。 我们还将 Azure 流量管理器前面我们 Barracuda WAF 实例可以在 Azure 数据中心和区域之间进行负载平衡。 高级别设置的图表将如下所示内容如下所示。

![体系结构][Architecture] 

> 注︰ [ILB 支持的应用程序服务环境](app-service-environment-with-internal-load-balancer.md)的引入，可以配置 ASE 不可从 DMZ，并只可到专用网络。 

## <a name="configuring-your-app-service-environment"></a>配置应用程序服务环境 ##
要配置应用程序服务环境参考[我们的文档](app-service-web-how-to-create-an-app-service-environment.md)主题。 创建应用程序服务环境之后，您可以在此环境中，将所有受后面一节中，我们配置 WAF 创建[Web 应用程序](app-service-web-overview.md)、 [API 的应用程序](../app-service-api/app-service-api-apps-why-best-platform.md)和[移动应用程序](../app-service-mobile/app-service-mobile-value-prop.md)。

## <a name="configuring-your-barracuda-waf-cloud-service"></a>配置 Barracuda WAF 云服务 ##
Barracuda 已部署其 WAF Azure 中的虚拟机上[详细的文章](https://campus.barracuda.com/product/webapplicationfirewall/article/WAF/DeployWAFInAzure)。 但是，因为我们需要冗余，并且不引入单一故障点，要将至少 2 WAF 实例虚拟机部署到同一个云服务，按照这些说明进行操作。

### <a name="adding-endpoints-to-cloud-service"></a>添加终结点以云服务 ###
一旦在云服务中有 2 个或更多 WAF VM 实例可以使用[Azure 门户](https://portal.azure.com/)添加下图中所示，您的应用程序所使用的 HTTP 和 HTTPS 终结点。

![配置终结点][ConfigureEndpoint]

如果您的应用程序使用其他终结点，请确保要添加到此列表以及这些。 

### <a name="configuring-barracuda-waf-through-its-management-portal"></a>配置 Barracuda WAF 通过其管理门户 ###
Barracuda WAF 通过其管理门户配置为使用 TCP 端口 8000。 因为我们有 WAF 虚拟机的多个实例，需要对每个虚拟机实例重复使用此处的步骤。 


> 注意︰ 一旦您完成配置 WAF，删除的 TCP/8000 终结点从您的所有 WAF Vm，以保持您 WAF 的安全。

在下图中所示配置 Barracuda WAF 添加管理终结点。

![添加终结点管理][AddManagementEndpoint]
 
使用浏览器浏览到云服务管理终结点。 如果云服务叫做 test.cloudapp.net，您将通过浏览到 http://test.cloudapp.net:8000 访问此终结点。 您应该看到一个登录页面下方喜欢使用 WAF VM 安装阶段中指定的凭据可以登录。

![管理登录页][ManagementLoginPage]

一次您登录您应看做仪表板将显示有关 WAF 保护的基本统计信息的一个图所示。

![管理仪表板][ManagementDashboard]

在服务选项卡上单击，将允许您配置其保护服务您 WAF。 有关更多详细信息配置 Barracuda WAF 可以参考[它们的文档](https://techlib.barracuda.com/waf/getstarted1)。 在 Azure Web 应用程序下面的示例提供的 HTTP 和 HTTPS 通信已配置。

![管理添加服务][ManagementAddServices]

> 注意︰ 根据您的应用程序的配置方式和您的应用程序的服务环境中使用了哪些功能，您需要转发通信的 TCP 端口 80 和 443，之外例如如果您的 Web 应用程序有 IP SSL 设置。 有关在应用程序服务环境中使用的网络端口的列表，请参阅[文档控制入站通信量的](app-service-app-service-environment-control-inbound-traffic.md)网络端口一节。

## <a name="configuring-microsoft-azure-traffic-manager-optional"></a>配置 Microsoft Azure 流量管理器 （可选） ##
如果您的应用程序可在多个地区，则要加载平衡它们背后[Azure 流量管理器](../traffic-manager/traffic-manager-overview.md)。 这样做可以在[Azure 的传统门户网站](https://manage.azure.com)流量管理器配置文件中使用您 WAF 的云服务名称，如下面的图像所示添加终结点。 

![终结点的通信管理器][TrafficManagerEndpoint]

如果您的应用程序要求身份验证，请确保您有一些资源，不需要任何身份验证的流量管理器来 ping 您的应用程序的可用性。 如下所示，您可以配置在配置节下的 URL [Azure 的传统门户网站](https://manage.azure.com)上。

![配置通信管理器][ConfigureTrafficManager]

要将流量管理器 ping 命令从您 WAF 转发给您的应用程序，您需要设置网站翻译上 Barracuda WAF 将通信转发到您的应用程序，如下面的示例中所示。

![网站翻译][WebsiteTranslations]

## <a name="securing-traffic-to-app-service-environment-using-network-security-groups-nsg"></a>保护通信的应用程序服务环境中使用网络安全组 (NSG)##
按照限制流量到您的应用程序服务环境从 WAF 只能通过使用云服务的 VIP 地址[控制入站流量文档](app-service-app-service-environment-control-inbound-traffic.md)了解详细信息。 下面是一个示例 Powershell 命令来执行此任务的 TCP 端口 80。


    Get-AzureNetworkSecurityGroup -Name "RestrictWestUSAppAccess" | Set-AzureNetworkSecurityRule -Name "ALLOW HTTP Barracuda" -Type Inbound -Priority 201 -Action Allow -SourceAddressPrefix '191.0.0.1'  -SourcePortRange '*' -DestinationAddressPrefix '*' -DestinationPortRange '80' -Protocol TCP

SourceAddressPrefix 替换 WAF 的云服务的虚拟 IP 地址 (VIP)。

> 注意︰ 删除并重新创建云服务时，会更改的云服务 VIP。 请确保更新网络资源组中的 IP 地址，一旦您这样做。 
 
<!-- IMAGES -->
[Architecture]: ./media/app-service-app-service-environment-web-application-firewall/Architecture.png
[ConfigureEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureEndpoint.png
[AddManagementEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/AddManagementEndpoint.png
[ManagementAddServices]: ./media/app-service-app-service-environment-web-application-firewall/ManagementAddServices.png
[ManagementDashboard]: ./media/app-service-app-service-environment-web-application-firewall/ManagementDashboard.png
[ManagementLoginPage]: ./media/app-service-app-service-environment-web-application-firewall/ManagementLoginPage.png
[TrafficManagerEndpoint]: ./media/app-service-app-service-environment-web-application-firewall/TrafficManagerEndpoint.png
[ConfigureTrafficManager]: ./media/app-service-app-service-environment-web-application-firewall/ConfigureTrafficManager.png
[WebsiteTranslations]: ./media/app-service-app-service-environment-web-application-firewall/WebsiteTranslations.png
