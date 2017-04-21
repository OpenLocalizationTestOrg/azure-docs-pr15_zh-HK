<properties
    pageTitle="创建和使用应用程序服务环境中使用内部负载平衡器 |Microsoft Azure"
    description="创建和使用 ILB ASE"
    services="app-service"
    documentationCenter=""
    authors="ccompy"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/17/2016"
    ms.author="ccompy"/>

# <a name="using-an-internal-load-balancer-with-an-app-service-environment"></a>应用程序服务环境中使用内部负载平衡器 #

应用程序服务 Environments(ASE) 功能是提供了一种增强的配置能力，它在多租户戳中不可用的 Azure 应用程序服务的特优服务选项。  ASE 功能实质上是将部署在您 Azure 虚拟 Network(VNet) Azure 应用程序服务。  能够阅读[什么是应用程序服务环境]的应用程序服务环境提供的功能更好地理解[WhatisASE]文档。  如果您不知道在 VNet 操作的好处阅读[Azure 虚拟网络常见问题解答][virtualnetwork]。  


## <a name="overview"></a>概述 ##


与 internet 访问终结点或使用您的 VNet 的 IP 地址，则可以部署 ASE。  若要将 IP 地址设置为您需要部署内部负载 Balancer(ILB) 与您 ASE 的 VNet 地址。  当您 ASE 获配 ILB 您提供︰

- 您自己的域或子域。  为了更加方便，本文假定子域但两种方法您可以配置它。  
- 使用 HTTPS 的证书
- 您的子域的 DNS 管理。  


作为回报，您可以执行以下事项︰

- 主机内部网应用程序，如业务线应用程序，安全地在云中通过站点到站点或 ExpressRoute VPN 访问
- 在云环境中的主机应用程序的公共 DNS 服务器中未列出
- 创建独立的互联网后端应用程序的前端应用程序可以安全地与集成


#### <a name="disabled-functionality"></a>已禁用的功能 ####

还有一些不能使用 ILB ASE 时的事情。  这些因素包括︰

- 使用 IPSSL
- 将 IP 地址分配给特定应用程序
- 购买并使用通过门户应用程序中使用的证书。  当然仍可以获得直接与证书颁发机构的证书，并将其与您的应用程序，只需不到 Azure 的门户。


## <a name="creating-an-ilb-ase"></a>创建 ILB ASE ##

创建 ILB ASE 不正常创建 ASE 大大不同。  有关创建 ASE 深入阅读[如何创建应用程序服务环境][HowtoCreateASE]。  创建 ILB ASE 的过程是 ASE 创建期间创建 VNet，或选择现有的 VNet 之间相同。  若要创建 ILB ASE: 

1.  在 Azure 门户中选择**新建-> Web + 手机-> 应用程序服务环境**
2.  选择您的订购
3.  选择或创建某一资源组
4.  选择或创建 VNet
5.  如果选择 VNet，创建子网
6.  **虚拟的网络位置-> VNet 配置**中选择并将 VIP 类型设置为内部
7.  提供子域名 （这将是用于应用程序中此 ASE 创建子域）
8.  选择确定，然后创建


![][1]


虚拟网络刀片式服务器中没有一个 VNet 配置选项。  这样，外部的 VIP 或内部 VIP 之间选择。  默认值为外部。  如果您将它设置为外部您 ASE 将使用可访问互联网的 VIP。  如果您选择内部，您 ASE 都配置 IP 地址在您 VNet ILB。  


选择内部后, 能够将更多的 IP 地址添加到您的 ASE 被移除，相反，您需要提供的 ASE 子域。  在与外部的 VIP ASE ASE 的名称用于在该子域中的 ASE 中创建的应用程序。  
如果您 ASE 调用***contosotest*** ，您的应用程序，调用了 ASE ***mytest***然后子域将的格式***contosotest.p.azurewebsites.net*** ，该应用程序的 URL 将***mytest.contosotest.p.azurewebsites.net***。  
如果您设置 VIP 类型为内部，ASE 名称不用于在该子域中 ASE。  您显式指定该子域。  如果您的子域被***contoso.corp.net*** ，ASE 命名为***timereporting*** ，做一个应用程序，应用程序的 URL 会***timereporting.contoso.corp.net***。


## <a name="apps-in-an-ilb-ase"></a>在 ILB ASE 的应用程序 ##

在 ILB ASE 中创建应用程序等同于正常情况下在 ASE 中创建应用程序。  

1. 在 Azure 门户中选择**新建-> Web + 手机-> Web**或**移动**或**API 的应用程序**
2. 输入应用程序的名称
2. 选择订阅
3. 选择或创建资源组
4. 选择或创建应用程序服务 Plan(ASP)。  如果创建新的 ASP 位置以选择您 ASE 再选择辅助池所需您要在中创建的 ASP。  当您创建 ASP 位置以及辅助池选择您 ASE。  指定应用程序名称时您会看到在您的应用程序名称下的子域将子域被替换为您 ASE。   
5. 选择创建。  如果您希望显示在您的仪表板上的应用程序，则应选择**到仪表板的 pin 码**复选框。  

![][2]


应用程序名称下子域名获取更新以反映您的 ASE 的子域。  


## <a name="post-ilb-ase-creation-validation"></a>开机自检 ILB ASE 创建验证 ##

ILB ASE 是比非-ILB ASE 略有不同。  为已记下您需要管理您自己的 DNS，您还需要提供您自己的证书的 HTTPS 连接。  


创建您的 ASE 后您会发现该子域显示子域，指定，并且在**设置**菜单中调用**ILB 证书**没有新项。  创建一个自签名证书，这使得它易于测试 HTTPS ASE。  门户网站将告诉您，您需要提供您自己的证书的 HTTPS，但这样做是为了迫使您让您的子域与匹配的证书。  

![][3]


如果只为了操作试验，并且不知道如何创建证书，您可以使用 IIS MMC 控制台应用程序创建自我签署的证书。  一经创建，即可以将其导出为一个.pfx 文件，然后将其上载 ILB 证书用户界面中。 当您访问使用自签名证书保护的站点时，您的浏览器将为您提供您正在访问的网站不是无法验证的证书安全警告。  如果您希望避免该警告需要正确签名的证书匹配您的子域，具有识别您的浏览器的信任链。

![][6]

如果您想要尝试使用您自己的证书的流动并测试到您 ASE 的 HTTP 和 HTTPS 访问︰

1.  转到 ASE UI 创建 ASE 后**ASE-> 设置-> ILB 证书**
2.  通过选择证书 pfx 文件设置 ILB 证书和提供密码。  此步骤需要一段的时间来处理，将显示缩放操作正在进行中的消息。
3.  获得您 ASE 的 ILB 地址 (**ASE-> 属性-> 虚拟 IP 地址**)
4.  在创建之后在 ASE 创建 web 应用程序 
5.  如果您没有在该 VNET （不在同一子网的 ASE 或操作工间休息），创建虚拟机
6.  设置您的子域的 DNS。  可以使用通配符子域中您的 DNS 或如果您想要做一些简单的测试，编辑主机文件在您的虚拟机设置为 VIP IP 地址的 web 应用程序名称。  如果您 ASE 有子域名。 ilbase.com 和您进行 web 应用程序 mytestapp，因此需要解决在 mytestapp.ilbase.com 然后在 hosts 文件中设置的。  （在 Windows 上的 hosts 文件位于 C:\Windows\System32\drivers\etc\）
7.  在该虚拟机上使用的浏览器并转到 http://mytestapp.ilbase.com （或其他任何 web 应用程序名称与您的子域）
8.  在该虚拟机上使用的浏览器并转至 https://mytestapp.ilbase.com 将不得不接受缺乏安全性，如果使用自签名的证书。  


您 ILB 的 IP 地址在属性中列出的虚拟 IP 地址

![][4]


## <a name="using-an-ilb-ase"></a>使用 ILB ASE ##

#### <a name="network-security-groups"></a>网络安全组 ####

ILB ASE 使应用程序不可访问或甚至通过互联网来确认您的应用程序的网络隔离。  这是适合于业务线应用程序如 intranet 网站的宿主。  当您需要限制访问甚至进一步仍可用网络安全 Groups(NSGs) 来控制网络级别的访问。 


如果您想要使用 NSGs 来进一步限制访问权限，则您需要确保您不会中断 ASE 操作所需的通信。  虽然 HTTP/HTTPS 访问只能通过由 ASE ILB ASE 仍取决于 VNet 之外的资源。  哪种网络访问是仍然需要查找文档中的信息[到应用程序服务环境控制入站]通信量[ControlInbound]和[ExpressRoute 的应用程序服务环境为网络配置]详细信息文档[ExpressRoute]。  


要配置您的 NSGs，您需要知道 Azure 用于管理您的 ASE 的 IP 地址。  如果它使 internet 请求，该 IP 地址也是来自您的 ASE 的出站 IP 地址。  若要查找此 IP 地址转到**设置-> 属性**，并查找**出站 IP 地址**。  

![][5]


#### <a name="general-ilb-ase-management"></a>一般 ILB ASE 管理 ####

管理 ILB ASE 是很大程度上与正常管理 ASE 相同。  您需要扩展您工作池承载多个 ASP 实例并扩大前端服务器处理的 HTTP/HTTPS 通信量的增加的数量。  有关管理的 ASE，配置常规信息阅读上[配置应用程序服务环境]的文档[ASEConfig]。  


证书管理和 DNS 管理就是其他管理项。  您需要获取并上载后 ILB ASE 创建用于 HTTPS 的证书到期之前将它替换它。  因为 Azure 拥有基域我们可以提供与外部的 VIP ASEs 证书。  由于使用 ILB ASE 的子域可以是任何内容，您需要为 HTTPS 提供您自己的证书。 


#### <a name="dns-configuration"></a>DNS 配置 ####

当使用外部的 VIP DNS 管理 Azure。  在您 ASE 中创建任何应用程序会自动添加到 Azure DNS 即公用 DNS。  在 ILB ASE 必须管理自己的 DNS。  对于给定子域例如 contoso.corp.net，您需要创建 DNS A 记录指向您的 ILB 地址︰

    * 
    *.scm ftp 发布 


## <a name="getting-started"></a>入门教程
所有的文章和方式-的[自述文件中的服务应用程序环境](../app-service/app-service-app-service-environments-readme.md)中提供应用程序服务环境为。

若要开始使用应用程序服务的环境，请参见[应用程序服务环境介绍][WhatisASE]

在 Azure 应用程序服务平台有关的详细信息，请参阅[Azure 应用程序服务][AzureAppService]。

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 

<!--Image references-->
[1]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createilbase.png
[2]: ./media/app-service-environment-with-internal-load-balancer/ilbase-createapp.png
[3]: ./media/app-service-environment-with-internal-load-balancer/ilbase-newase.png
[4]: ./media/app-service-environment-with-internal-load-balancer/ilbase-vip.png
[5]: ./media/app-service-environment-with-internal-load-balancer/ilbase-externalvip.png
[6]: ./media/app-service-environment-with-internal-load-balancer/ilbase-ilbcertificate.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[ControlInbound]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-control-inbound-traffic/
[virtualnetwork]: https://azure.microsoft.com/documentation/articles/virtual-networks-faq/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ExpressRoute]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-network-configuration-expressroute/
[vnetnsgs]: http://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[ASEConfig]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
