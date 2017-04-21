<properties
    pageTitle="在一个应用程序服务的环境中创建 web 应用程序"
    description="了解如何在 web 应用程序和应用程序服务计划在环境中创建的应用程序服务"
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
    ms.date="10/17/2016"
    ms.author="ccompy"/>

# <a name="create-a-web-app-in-an-app-service-environment"></a>在一个应用程序服务的环境中创建 web 应用程序

## <a name="overview"></a>概述

本教程展示如何在[应用程序服务环境](app-service-app-service-environment-intro.md)(ASE) 中创建 web 应用程序和应用程序服务计划。 

> [AZURE.NOTE] 如果想要了解如何创建 web 应用程序，但不需要在应用程序的服务环境做它，请参阅[创建.NET web 应用程序](web-sites-dotnet-get-started.md)或相关的其他语言和框架指南之一。

## <a name="prerequisites"></a>系统必备组件

本教程假设您已创建的应用程序服务环境。 如果您还没有完成，请参阅[创建一个应用程序的服务环境](app-service-web-how-to-create-an-app-service-environment.md)。 

## <a name="create-a-web-app"></a>创建 web 应用程序

1. 在[Azure 门户](https://portal.azure.com/)，单击**新建 > Web + 移动 > Web 应用程序**。 

    ![][1]

2. 选择您的订购。  

    如果您有多个订阅，请注意，在您的应用程序服务的环境中创建一个应用程序，您需要使用在创建环境时使用的同一订阅。 

3. 选择或创建资源组。

    *资源组*使您能够作为一个单元来管理 Azure 的相关的资源，并建立您的应用程序的*基于角色的访问控制*(RBAC) 规则时十分有用。 有关详细信息，请参阅[管理 Azure 资源][ResourceGroups]。 

4. 选择或创建应用程序服务计划。

    *应用程序服务计划*管理的 web 应用程序的设置。  通常选择定价时，收取的价格被应用到应用程序服务计划，而不是单个应用程序。 在 ASE 您支付分配到 ASE 的计算实例而不是什么您列出了与您的 ASP。  扩大您扩展您的应用程序服务的实例的 web 应用程序的实例数计划，并会影响所有的 web 应用程序在该计划中。  还有一些功能，例如站点插槽或 VNET 集成在计划内的数量限制。  有关详细信息，请参阅[Azure 应用程序服务计划概述](../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md)

    通过观察计划名下注明的位置，可以在您 ASE 标识应用程序服务计划。  

    ![][5]

    如果您想要使用您的应用程序的服务环境中已经存在的应用程序服务计划，请选中该计划。 如果您想要创建一个新的应用程序服务计划，请参见本教程中，[创建应用程序服务环境中的应用程序服务计划](#createplan)的下一节。

5. 输入您的 web 应用程序的名称，然后单击**创建**。 

    如果您 ASE 使用外部的 VIP ASE 中应用程序的 URL 为: [*站点名*]。[*您的应用程序服务环境的名称*]。 而不是 [*网站名称*] 的 p.azurewebsites.net。 azurewebsites.net
    
    如果您 ASE 使用内部的 VIP，然后应用程序的 URL，ASE 是: [*站点名*]。[*ASE 创建期间指定的子域*]   
    ASE 创建期间选择您的 ASP 后您将看到以下**名称**更新该子域

## <a name="createplan"></a>创建应用程序服务计划

在应用程序服务环境中创建一个应用程序服务计划时，工作人员所做的选择是不同的 ASE 有没有共享的工作人员。  您必须使用的工作人员是由管理员分配到 ASE  这意味着，若要创建新的计划，需要有更多工作人员在所有您的计划已在该工作池分配给实例总数比您 ASE 辅助池。  在 ASE 辅助池创建您的计划中没有足够的工作人员，如果您需要使用您的 ASE 管理员，让他们添加。

由应用程序服务环境承载的应用程序服务计划的另一个差异是缺乏定价选择。  当您有一个应用程序的服务环境您支付系统使用的计算资源，并在该环境中没有增加的费用的计划。  通常情况下当您创建应用程序服务计划选择的定价计划，以确定您的付费。  应用程序服务环境是本质上是私人位置，您可以在其中创建内容。  您支付环境并不是来承载您的内容。

以下说明演示如何在创建 web 应用程序的教程上一节中所述创建应用程序服务计划。

1. 计划选择用户界面中单击**新建**，并提供您的计划的名称，就像往常一样 ASE 之外。

2. 选择您要从您的位置选择器使用 ASE。

    因为应用程序服务环境本质上是一个专用的部署位置，它将显示在位置下。 

    ![][2]

    ASE 在位置选择器中的所选内容之后, 应用程序服务计划创建 UI 更新。  位置现在显示 ASE 系统和区域名称，然后拾定价计划将被替换为一个辅助池选取。  

    ![][3]

### <a name="selecting-a-worker-pool"></a>选择辅助池

通常在 Azure 应用程序服务和应用程序服务环境外，有 3 种计算尺寸可用的专用的价格计划的选择。  以类似的方式，为 ASE 可以定义的工作人员达 3 池并指定用于该辅助池的计算大小。  含义，ASE 的承租人，而是选择与您的应用程序服务计划的计算大小的定价计划的则您可以选择所谓的*辅助池*。  

工作人员池选择 UI 显示用于该辅助池名称的下方的计算大小。  多少计算实例可用于此池中的可用数量表示。  总池实际上可能比这个数字更多的实例，但此值指的是只需多少未被使用。  如果您需要调整您的应用程序服务环境，以添加更多计算资源，请参阅[配置您的应用程序的服务环境](app-service-web-configure-an-app-service-environment.md)。

![][4]

在此示例中，您将看到只有两个可用的辅助池。 这是因为 ASE 管理员将只分配到那些两个辅助池的主机。  虚拟机分配到它时将显示第三个。  

## <a name="after-web-app-creation"></a>Web 应用程序创建之后

没有为运行 web 应用程序和管理应用程序服务计划中需要考虑到 ASE 的几个注意事项。  

如前所述，ASE 的所有者负责系统的大小，因此它们也负责确保有足够的容量来承载所需的应用程序服务计划。 如果没有可用的工作人员，您将无法创建您的应用程序服务的计划。  这也是设置为 true 将扩大您的 web 应用程序。  如果您需要多个实例将需要获得您的应用程序服务环境管理，以添加更多的工作人员。

创建了 web 应用程序和应用程序服务计划后是一个好主意，它扩大。  在 ASE 始终需要有至少 2 实例的应用程序服务计划为您的应用程序提供容错能力。  在 ASE 缩放应用程序服务计划等同于正常通过用户界面的应用程序服务计划。  为扩展，[如何扩展 web 应用程序的应用程序服务环境中](app-service-web-scale-a-web-app-in-an-app-service-environment.md)的详细信息

<!--Image references-->
[1]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspnewwebapp.png
[2]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createasplocation.png
[3]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspselected.png
[4]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/createaspworkerpool.png
[5]: ./media/app-service-web-how-to-create-a-web-app-in-an-ase/selectaspinase.png

<!--Links-->
[WhatisASE]: http://azure.microsoft.com/documentation/articles/app-service-app-service-environment-intro/
[Appserviceplans]: http://azure.microsoft.com/documentation/articles/azure-web-sites-web-hosting-plans-in-depth-overview/
[HowtoCreateASE]: http://azure.microsoft.com/documentation/articles/app-service-web-how-to-create-an-app-service-environment/
[HowtoScale]: http://azure.microsoft.com/documentation/articles/app-service-web-scale-a-web-app-in-an-app-service-environment
[HowtoConfigureASE]: http://azure.microsoft.com/documentation/articles/app-service-web-configure-an-app-service-environment
[ResourceGroups]: http://azure.microsoft.com/documentation/articles/resource-group-portal/
[AzurePowershell]: http://azure.microsoft.com/documentation/articles/powershell-install-configure/
