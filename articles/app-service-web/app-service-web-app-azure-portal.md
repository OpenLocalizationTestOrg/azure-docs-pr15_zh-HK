<properties
    pageTitle="用于导航的 Azure 门户的引用"
    description="应用程序服务 Web 管理门户和 Azure 门户之间学习不同的用户体验"
    services="app-service"
    documentationCenter=""
    authors="jaime-espinosa"
    manager="wpickett"
    editor="jimbe"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="02/26/2016"
    ms.author="jaime-espinosa"/>

# <a name="reference-for-navigating-the-azure-portal"></a>用于导航的 Azure 门户的引用

Azure 网站现在被称为[应用程序服务 Web 应用程序](http://go.microsoft.com/fwlink/?LinkId=529714)。 我们正在更新所有我们的文档以反映名称的更改，并提供了 Azure 门户的说明。 直到完成该过程后，可用于处理在 Azure 门户 Web 应用程序作为指南使用本文档。

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 
 
## <a name="the-future-of-the-azure-classic-portal"></a>未来的 Azure 经典门户

您会注意到品牌在 Azure 传统门户网站上的更改，而该门户网站正在于 Azure 门户取代。 与传统门户网站将被淘汰，新开发的重点转向 Azure 门户。 Web 应用程序的所有即将推出的新功能会在 Azure 门户。 开始使用 Azure 门户网站利用最新和最好的 Web 应用程序可以提供。

## <a name="layout-differences-between-the-azure-classic-portal-and-azure-portal"></a>布局 Azure 经典门户和 Azure 门户之间的差异

在经典的门户中，左侧列出所有 Azure 服务。 传统的门户网站中的导航遵循树状结构，您从服务开始并导航到的每个元素。 该结构适用于管理独立的组件。 但是，在 Azure 上生成的应用程序是一套相互连接的服务，并且此树状结构不适合使用的服务的集合。 

Azure 门户容易生成与来自多个服务组件的应用程序最终结束。 门户网站被组织成*到达*。 *历程*是刀片的一系列*刀片*，这些不同的组件的容器。 例如，设置自动缩放的 web 应用程序*旅*采用几种刀片在下面的示例所示: （，刀片式服务器标题有尚未进行更新以使用新的术语）**的网站**刀片式服务器，**设置**刀片式服务器，并且**向外的扩展**刀片式服务器。 在示例中，自动调整被设置为取决于 CPU 使用率，因此也是**CPU 百分比**刀片式服务器。 *刀片式服务器*中的组件被称为*部件*，它看起来像拼贴。 

![](./media/app-service-web-app-azure-portal/AutoScaling.png)

## <a name="navigation-example-create-a-web-app"></a>导航示例︰ 创建 web 应用程序

创建新的 web 应用程序是仍然为 1-2-3 一样简单。 下面的图像显示传统的门户网站和门户网站--并排展示不多已更改步骤获取 web 应用程序需要和运行的数量。 

![](./media/app-service-web-app-azure-portal/CreateWebApp.png)

门户可以从 web 应用程序，包括诸如 WordPress 的常用库应用程序的最常见类型中进行选择。 可用的应用程序的完整列表，请访问[Azure 市场]。

创建 web 应用程序时，您指定的 URL 中，应用程序服务计划，并在门户中就像经典的门户中的位置。 

![](./media/app-service-web-app-azure-portal/CreateWebAppSettings.png)

此外，门户允许您定义其他常见设置。 例如，[资源组](../azure-resource-manager/resource-group-overview.md)使其易于查看和管理 Azure 的相关的资源。 

## <a name="navigation-example-settings-and-features"></a>导航示例︰ 设置和功能

所有的设置和功能是现在逻辑分组中的单个刀片式服务器，您可以找到。

![](./media/app-service-web-app-azure-portal/WebAppSettings.png)

例如，您可以通过单击**自定义的域和 SSL** **设置**刀片式服务器中创建自定义的域。

![](./media/app-service-web-app-azure-portal/ConfigureWebApp.png)

若要设置监视警报，请单击**请求和错误**，然后选择**添加通知**。

![](./media/app-service-web-app-azure-portal/Monitoring.png)

要启用诊断程序，请单击**诊断日志****设置**刀片式服务器中。

![](./media/app-service-web-app-azure-portal/Diagnostics.png)
 
要配置应用程序设置，请单击**设置**刀片式服务器中的**应用程序设置**。 

![](./media/app-service-web-app-azure-portal/AppSettingsPreview.png)

品牌名称，不在门户中的几件事情有已重命名或以不同的方式进行分组以使其更容易地找到它们。 例如，下面是应用程序的相应页面的屏幕快照中经典的门户的设置 （**配置**）。

![](./media/app-service-web-app-azure-portal/AppSettings.png)

## <a name="more-resources"></a>更多的资源

[Azure Portal]: https://portal.azure.com
[Azure 的市场]: /marketplace/

>[AZURE.NOTE] 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)，立即可以在此创建短期的初学者 web 应用程序在应用程序服务。 没有信用卡，所需;没有承诺。

## <a name="whats-changed"></a>会发生什么变化
* 有关更改网站为应用程序服务的指南，请参阅︰ [Azure 应用程序服务，并对现有的 Azure 服务及其影响](http://go.microsoft.com/fwlink/?LinkId=529714)
 
