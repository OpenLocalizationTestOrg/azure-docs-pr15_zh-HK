<properties 
    pageTitle="如何在应用程序服务环境中扩展应用程序" 
    description="在应用程序服务环境中缩放应用程序" 
    services="app-service" 
    documentationCenter="" 
    authors="ccompy" 
    manager="stefsch" 
    editor="jimbe"/>

<tags 
    ms.service="app-service" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/17/2016" 
    ms.author="ccompy"/>

# <a name="scaling-apps-in-an-app-service-environment"></a>在应用程序服务环境中缩放应用程序 #

在 Azure 应用程序服务有通常可以扩展三个操作︰

- 定价计划
- 工作人员大小 
- 实例的数目。

在 ASE 选择或更改定价计划没有必要。  在功能方面它已珍贵定价功能级别。  

对于人员规模，ASE 管理员可以分配计算资源用于工作人员的每个池的大小。  这意味着具有 P4 计算资源池 1 工作人员和工作人员与 P1 的池 2 计算资源，如果需要。  他们不需要按大小顺序排列。  有关详细说明周围大小和其定价请参阅此处的文档[Azure 应用程序服务定价][AppServicePricing]。  这样，web 应用程序和应用程序服务计划的缩放选项中为应用程序服务环境︰

- 工作人员池选择
- 实例数

通过适当的用户界面显示您 ASE 承载的应用程序服务计划完成任何一项的更改。  

![][1]

您不能扩展超出可用的计算资源，在您的 ASP 是在辅助池数您 ASP。  如果您需要计算该工作人员库中的资源需要得到 ASE 管理员联系以添加它们。  有关重新配置您的 ASE 围绕信息阅读此处的信息︰[如何配置应用程序服务环境][HowtoConfigureASE]。  您还可以利用 ASE 自动缩放功能，可以添加容量基于时间表和指标。  若要获取更多详细信息配置自动缩放的 ASE 环境本身，请参阅[如何配置应用程序服务环境的自动缩放][ASEAutoscale]。

您可以创建多个应用程序使用不同的辅助池，从计算资源的服务计划，也可以使用相同的辅助池。  如果辅助池 1 中有 (10) 可用的计算资源，您可以选择创建一个应用程序服务计划使用 (6 日) 的计算资源，并且计划的第二个应用程序服务的示例，使用 (4) 的计算资源。

### <a name="scaling-the-number-of-instances"></a>扩展实例的数 ###

当您首次创建 web 应用程序与 1 个实例启动的应用程序服务环境中。  您可以再扩展到更多的实例，为您的应用程序提供更多的计算资源。   

如果您 ASE 有足够的空间，这是相当简单。  转到包含您想要扩大规模，选择规模的网站您的应用程序服务计划。  这将打开的用户界面，您可以手动将比例设为您的 ASP 或将自动缩放比例规则配置为您的 ASP。  手动扩展到您的应用程序只需将***通过***设置为***手动输入实例计数***。  从这里，将滑块拖动到所需数量，或者在该滑块旁边的框中输入。  

![][2] 

与它们通常在 ASE ASP 的自动缩放比例规则工作方式相同。  可以选择***通过刻度***下***CPU 百分比***并为基于 CPU 百分比您 ASP 创建自动缩放比例规则或您可以创建更复杂的规则使用***计划和性能规则***。  为了更完整的详细信息，请参阅配置自动缩放比例，请使用指南这里[扩展在 Azure 应用程序服务的应用程序][AppScale]。 


### <a name="worker-pool-selection"></a>工作人员池选择 ###

如前所述，可以辅助池选择从 ASP 用户界面访问。  打开您想要扩展并选择辅助池 ASP 刀片式服务器。  您将看到所有已配置应用程序服务环境中的辅助池。  如果您有一个辅助池然后将只能看到列出的一个池。  若要更改您的 ASP 处于何种辅助池，您只需选择您希望您的应用程序服务计划将移到辅助池。  

![][3]

前一个工人池间移动您的 ASP 很重要，以确保您将有足够的容量，您的 asp。  在辅助池的列表，列出了辅助池名称不仅还可以看到多少工作人员提供了该辅助池。  请确保有足够多的实例可用来包含您的应用程序服务的规划。  如果更需要计算中您要移到辅助池的资源，然后获得 ASE 管理员联系以添加它们。  

> [AZURE.NOTE] 移动从一个辅助池 ASP 将导致冷启动的在该 ASP 应用程序。  这可能导致请求为您的应用程序是冷启动新的计算资源上运行缓慢。  可以通过使用[应用程序功能预热]避免冷启动[AppWarmup]在 Azure 应用程序服务。  由于应用程序是冷启动新的计算资源时，也会调用初始化过程，文章中介绍的应用程序初始化模块还适用于冷启动。 

## <a name="getting-started"></a>入门教程

要开始使用服务的应用程序环境，请参阅[如何为创建应用程序服务环境][HowtoCreateASE]

在 Azure 应用程序服务平台有关的详细信息，请参阅[Azure 应用程序服务][AzureAppService]。

<!--Image references-->
[1]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-aspblade.png
[2]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-manualscale.png
[3]: ./media/app-service-web-scale-a-web-app-in-an-app-service-environment/aseappscale-sizescale.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[ScaleWebapp]: http://azure.microsoft.com/documentation/articles/web-sites-scale/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoConfigureASE]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment/
[CreateWebappinASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-a-web-app-in-an-ase/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[AppServicePricing]: http://azure.microsoft.com/pricing/details/app-service/ 
[AzureAppService]: http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/
[ASEAutoscale]: http://azure.microsoft.com/documentation/articles/app-service-environment-auto-scale/
[AppScale]: http://azure.microsoft.com/documentation/articles/web-sites-scale/
[AppWarmup]: http://ruslany.net/2015/09/how-to-warm-up-azure-web-app-during-deployment-slots-swap/
