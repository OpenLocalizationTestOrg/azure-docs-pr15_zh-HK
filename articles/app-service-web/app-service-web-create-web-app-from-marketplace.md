<properties
    pageTitle="从 Azure 市场创建一个 web 应用程序 |Microsoft Azure"
    description="了解如何通过使用 Azure 门户从 Azure 市场上创建新的 WordPress web 应用程序。"
    services="app-service\web"
    documentationCenter=""
    authors="rmcmurray"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/20/2016"
    ms.author="robmcm"/>

<!-- Note: This article replaces web-sites-php-web-site-gallery.md -->

# <a name="create-a-web-app-from-the-azure-marketplace"></a>从 Azure 市场创建一个 web 应用程序

[AZURE.INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

Azure 市场提供范围广泛的流行的 web 应用程序由 Microsoft、 第三方公司和开放源码软件计划开发。 例如，WordPress，Umbraco CMS，Drupal，等等。这些 web 应用程序是基于广泛的流行的框架，例如[PHP]在此的 WordPress 例如， [.NET]， [Node.js]， [Java]和[Python]，仅举几例。 从 Azure 市场独一无二的软件，您需要创建一个 web 应用程序是为[Azure 门户]使用的浏览器。

在本教程中，您将学习如何︰

* 找到并在 Azure 应用程序服务，基于 Azure 市场的模板中创建 web 应用程序。
* 配置新的 web 应用程序设置 Azure 应用程序服务。
* 启动和管理您的 web 应用程序。

用于本教程中，您将部署 WordPress 博客网站从 Azure 市场。 在完成本教程中的步骤后，您将有自己的 WordPress 站点上并在云中运行。

![应用程序示例 WordPress wep 的仪表板][WordPressDashboard1]

在本教程中将部署的 WordPress 站点使用 MySQL 数据库。 如果您想要改为对数据库中使用 SQL 数据库，请参见[项目 Nami]，这也是可通过 Azure 市场。

> [AZURE.NOTE]
> 若要完成本教程，您需要一个 Microsoft Azure 帐户。 如果您没有帐户，则可以[激活您的 Visual Studio 订户权益][activate]或[注册的免费试用版][free trial]。
>
> 如果您想要开始使用 Azure 应用程序服务注册 Azure 帐户之前，请转到[尝试应用程序服务]。 从那里立即可以在应用程序服务中创建短期的初学者 web 应用程序 — — 没有信用卡是必需的并且没有任何承诺。

## <a name="find-and-create-a-web-app-in-azure-app-service"></a>查找和 Azure 应用程序服务中创建 Web 应用程序

1. 登录到[Azure 的门户]。

1. 单击**新建**。
    
    ![创建新的 Azure 资源][MarketplaceStart]
    
1. **WordPress**，搜索，然后单击**WordPress**。 （如果您希望使用 SQL 数据库，而不是 MySQL，搜索**项目 Nami**。）

    ![在市场上的 WordPress 的搜索][MarketplaceSearch]
    
1. 阅读后 WordPress 的应用程序的说明，请单击**创建**。

    ![创建 WordPress 的 web 应用程序][MarketplaceCreate]

## <a name="configure-azure-app-service-settings-for-your-new-web-app"></a>配置新的 Web 应用程序的 Azure 应用程序服务设置

1. 创建新的 web 应用程序后，WordPress 设置刀片式服务器将显示，您需要使用它来完成以下步骤︰

    ![配置 WordPress web 应用程序设置][ConfigStart]

1. 在**Web 应用程序**框中输入的 web 应用程序的名称。

    此名称必须是唯一的 azurewebsites.net 域中，因为 web 应用程序的 URL 将*{name}*。 azurewebsites.net。 如果您输入的名称不唯一，文本框中将显示一个红色感叹号。

    ![配置 WordPress web 应用程序名称][ConfigAppName]

1. 如果您有多个订阅，则选择要使用的一个。 

    ![配置 web 应用程序的订阅][ConfigSubscription]

1. 选择一个**资源组**，或创建一个新。

    有关资源组的详细信息，请参阅[Azure 资源管理器概述][ResourceGroups]。

    ![配置 web 应用程序的资源组][ConfigResourceGroup]

1. 选择某个**应用程序服务计划/位置**或新建一个。

    有关应用程序服务计划的详细信息，请参阅[Azure 应用程序服务计划概述][AzureAppServicePlans]。 

    ![配置 web 应用程序的服务计划][ConfigServicePlan]

1. 单击**数据库**，然后**新的 MySQL 数据库**刀片式服务器中配置 MySQL 数据库提供所需的值。

    一。 输入一个新名称或保留默认名称。

    b。 保留**数据库类型**设置为**共享**。

    c。 选择与您选定的 web 应用程序相同的位置。

    d。 选择定价层。 对于本教程，**水星**-这是可用最少的连接和磁盘空间的方法很好。

    电子。 **新的 MySQL 数据库**刀片式服务器，在接受的法律条款，然后单击**确定**。 

    ![配置 web 应用程序的数据库设置][ConfigDatabase]

1. 在**WordPress**刀片式服务器，接受的法律条款，然后单击**创建**。 

    ![完成的 web 应用程序设置，然后单击确定][ConfigFinished]

    Azure 应用程序服务创建 web 应用程序中，通常在 1 分钟之内。 您可以通过单击门户页顶部的铃图标监视进度。

    ![进度指示器][ConfigProgress]

## <a name="launch-and-manage-your-wordpress-web-app"></a>启动和管理您的 WordPress web 应用程序
    
1. Web 应用程序创建完成后，导航到您在其中创建应用程序时，该资源组的 Azure 门户，您可以看到该 web 应用程序和数据库。

    具有发光灯泡图标的额外资源是[应用程序的见解][ApplicationInsights]，它提供了对您的 web 应用程序的监视服务。

1. 在**资源组**刀片式服务器，请单击 web 应用程序行。

    ![选择您 WordPress 的 web 应用程序][WordPressSelect]

1. 在 Web 应用程序刀片式服务器，单击**浏览**。

    ![浏览到 WordPress 的 web 应用程序][WordPressBrowse]

1. 如果提示您选择您的 WordPress 博客的语言，选择您所需的语言，然后单击**继续**。

    ![配置您的 WordPress web 应用程序的语言][WordPressLanguage]

1. 在 WordPress**欢迎**页上，输入所需的 WordPress，配置信息，然后单击**安装 WordPress**。

    ![WordPress 的 web 应用程序配置设置][WordPressConfigure]

1. 使用您在**欢迎**页创建的凭据登录。  

1. 您网站的仪表板页面将打开，并显示您提供的信息。    

    ![查看您的 WordPress 仪表板][WordPressDashboard2]

## <a name="next-steps"></a>下一步行动

在本教程中，您已经看到如何创建和部署示例 web 应用程序从 Azure 市场。

有关如何使用应用程序服务 Web 应用程序的详细信息，请参阅链接 （适用于广泛的浏览器窗口） 页面的左侧或顶部的页面 （窄的浏览器窗口）。

有关开发 WordPress 在 Azure 上的 web 应用程序的详细信息，请参阅[在 Azure 应用程序服务开发 WordPress][WordPressOnAzure]。 

<!-- URL List -->

[PHP]: https://azure.microsoft.com/develop/php/
[.NET]: https://azure.microsoft.com/develop/net/
[Node.js]: https://azure.microsoft.com/develop/nodejs/
[Java]: https://azure.microsoft.com/develop/java/
[Python]: https://azure.microsoft.com/develop/python/
[activate]: https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/
[free trial]: https://azure.microsoft.com/pricing/free-trial/
[试用应用程序服务]: http://go.microsoft.com/fwlink/?LinkId=523751
[ResourceGroups]: ../resource-group-overview.md
[AzureAppServicePlans]: ../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md
[ApplicationInsights]: https://azure.microsoft.com/services/application-insights/
[Azure 门户]: https://portal.azure.com/
[项目 Nami]: http://projectnami.org/
[WordPressOnAzure]: ./develop-wordpress-on-app-service-web-apps.md

<!-- IMG List -->

[MarketplaceStart]: ./media/app-service-web-create-web-app-from-marketplace/marketplacestart.png
[MarketplaceSearch]: ./media/app-service-web-create-web-app-from-marketplace/marketplacesearch.png
[MarketplaceCreate]: ./media/app-service-web-create-web-app-from-marketplace/marketplacecreate.png
[ConfigStart]: ./media/app-service-web-create-web-app-from-marketplace/configstart.png
[ConfigAppName]: ./media/app-service-web-create-web-app-from-marketplace/configappname.png
[ConfigSubscription]: ./media/app-service-web-create-web-app-from-marketplace/configsubscription.png
[ConfigResourceGroup]: ./media/app-service-web-create-web-app-from-marketplace/configresourcegroup.png
[ConfigServicePlan]: ./media/app-service-web-create-web-app-from-marketplace/configserviceplan.png
[ConfigDatabase]: ./media/app-service-web-create-web-app-from-marketplace/configdatabase.png
[ConfigFinished]: ./media/app-service-web-create-web-app-from-marketplace/configfinished.png
[ConfigProgress]: ./media/app-service-web-create-web-app-from-marketplace/configprogress.png
[WordPressSelect]: ./media/app-service-web-create-web-app-from-marketplace/wpselect.png
[WordPressBrowse]: ./media/app-service-web-create-web-app-from-marketplace/wpbrowse.png
[WordPressLanguage]: ./media/app-service-web-create-web-app-from-marketplace/wplanguage.png
[WordPressDashboard1]: ./media/app-service-web-create-web-app-from-marketplace/wpdashboard1.png
[WordPressDashboard2]: ./media/app-service-web-create-web-app-from-marketplace/wpdashboard2.png
[WordPressConfigure]: ./media/app-service-web-create-web-app-from-marketplace/wpconfigure.png
