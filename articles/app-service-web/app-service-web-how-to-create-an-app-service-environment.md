<properties 
    pageTitle="如何创建应用程序服务环境" 
    description="应用程序服务环境的创建数据流说明" 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="stefsch" 
    editor=""/>

<tags 
    ms.service="app-service" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/22/2016" 
    ms.author="ccompy"/>

# <a name="how-to-create-an-app-service-environment"></a>如何创建应用程序服务环境 #

### <a name="overview"></a>概述 ###

应用程序服务环境 (ASE) 是 Azure 应用程序服务，提供了一种增强的配置能力，它在多租户戳中不可用的特优服务选项。  ASE 功能实质上是将 Azure 应用程序服务部署到客户的虚拟网络。  能够阅读[什么是应用程序服务环境]的应用程序服务环境提供的功能更好地理解[WhatisASE]文档。

### <a name="before-you-create-your-ase"></a>在创建您的 ASE 之前 ###

务必要注意不能更改的事情。  那些创建后不能更改您的 ASE 有关的方面有︰

- 位置
- 订阅
- 资源组
- 使用 VNet
- 使用子网 
- 子网大小

领料 VNet，并指定一个子网，请确保时，足够大以容纳任何未来的增长。  

### <a name="creating-an-app-service-environment"></a>创建应用程序服务环境 ###

有两种方法来访问 ASE 创建用户界面。  它可以通过***应用程序服务环境***在 Azure 市场搜索找到或通过新建-> Web + 手机-> 应用程序服务环境。  若要创建 ASE:

1. 提供您 ASE 的名称。  ASE 为指定的名称将用于在 ASE 中创建的应用程序。  如果 ASE 的名称为 appsvcenvdemo，则就是子域名。*appsvcenvdemo.p.azurewebsites.net*。  如果因此创建名为*mytestapp* ，则可在*mytestapp.appsvcenvdemo.p.azurewebsites.net*寻址的应用程序。  您 ASE 的名称，不能使用空格。  如果在名称中使用大写字符，该域名将总该名称的小写版本。  如果使用 ILB 然后 ASE 名称您的子域中未使用但明确而 ASE 创建过程声明

    ![][1]

2. 选择您的订购。  用于您 ASE 的订阅也是一种将创建在该 ASE 的所有应用程序。  您不能将您 ASE 放置在处于另一个订阅 VNet

3. 选择或指定一个新的资源组。  用于您 ASE 的资源组必须为您 VNet 使用相同。  如果您选择现有 VNet 然后将更新您 ASE 资源组选择以反映您的 VNet 的。

    ![][2]

4. 进行虚拟网络和位置选择。  您可以选择创建新的 VNet 或选择现有的 VNet。  如果选择新的 VNet，您可以指定名称和位置。 新的 VNet 将地址范围 192.268.250.0/23，并命名为 192.168.250.0/24 定义的**默认**的子网。  也只是可以选择预先存在经典或资源管理器 VNet。  VIP 类型选择确定如果您 ASE 可以直接从 internet （外部） 访问或使用内部负载平衡器 (ILB)。  若要了解有关它们的详细阅读[使用与应用程序服务环境内部负载平衡器][ILBASE]。  如果您选择的外部 VIP 类型，您可以选择多少与 IPSSL 用于创建系统的外部 IP 地址。  如果您选择内部，则需要指定将使用您 ASE 子域。  ASEs 可以部署到 （亦即使用*任一*公用地址范围，*或*RFC1918 地址空间的虚拟网络 专用地址）。  以虚拟的网络中使用的公共地址范围，您将需要创建提前 VNet。  当您选择已有的 VNet 需要 ASE 创建期间创建新的子网。  **不能在门户中使用预先创建的子网。 如果您创建使用资源管理器模板您 ASE，您可以创建一个预先存在的子网与 ASE。**  若要从模板的使用[创建]的信息，在这里，从模板的应用程序服务环境创建 ASE[ILBAseTemplate]和这里，[创建了一个模板中的 ILB 应用程序服务环境][ASEfromTemplate]。

### <a name="details"></a>详细信息 ###

ASE 创建 2 前端和 2 的工作人员。  前端作为 HTTP/HTTPS 终结点，并将通信发送到承载您的应用程序的角色的工作人员。   您可以 ASE 创建后调整的数量，并且可以甚至设置自动缩放比例规则对这些资源池。  有关手动缩放周围的详细信息，管理和监控应用程序服务环境转到此处︰[如何配置应用程序服务环境][ASEConfig] 

只有一个 ASE 可以存在于使用 ASE 的子网。  子网不能用于 ASE 之外的任何内容

### <a name="after-app-service-environment-creation"></a>应用程序服务环境创建之后 ###

ASE 创建后，您可以调整︰

- 前端的数量 (最小值︰ 2)
- 工作人员的数量 (最小值︰ 2)
- 针对 IP SSL 可用 IP 地址的数量
- 计算资源规模的前端或工作人员使用 （前端的最小大小为 P2）


有手动缩放，管理和这里监视应用程序服务环境周围的详细信息︰[如何配置应用程序服务环境][ASEConfig] 

有关自动缩放是此处的指南︰[如何配置应用程序服务环境的自动缩放][ASEAutoscale]

有没有可供自定义数据库和存储等的附加依赖项。  这些是由 Azure，随系统。  系统存储支持整个应用程序服务环境达 500 GB 和通过 Azure 调整数据库，根据需要通过系统的规模。


## <a name="getting-started"></a>入门教程
所有的文章和方式-的[自述文件中的服务应用程序环境](../app-service/app-service-app-service-environments-readme.md)中提供应用程序服务环境为。

若要开始使用应用程序服务的环境，请参见[应用程序服务环境介绍][WhatisASE]

在 Azure 应用程序服务平台有关的详细信息，请参阅[Azure 应用程序服务][AzureAppService]。

[AZURE.INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[AZURE.INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]
 

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-basecreateblade.png
[2]: ./media/app-service-web-how-to-create-an-app-service-environment/asecreate-vnetcreation.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ASEConfig]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/ 
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[ILBASE]: http://azure.microsoft.com/documentation/articles/app-service-environment-with-internal-load-balancer/
[ILBAseTemplate]: http://azure.microsoft.com/documentation/templates/201-web-app-ase-create/
[ASEfromTemplate]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-create-ilb-ase-resourcemanager/
