<properties
    pageTitle="使用 Azure 门户 web 应用程序克隆"
    description="了解如何复制到新的 Web 应用程序使用 Azure 门户 Web 应用程序。"
    services="app-service\web"
    documentationCenter=""
    authors="ahmedelnably"
    manager="stefsch"
    editor=""/>

<tags
    ms.service="app-service-web"
    ms.workload="web"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/08/2016"
    ms.author="ahmedelnably"/>

# <a name="azure-app-service-app-cloning-using-azure-portal"></a>Azure 应用程序服务应用程序克隆使用 Azure 门户#

在[Azure 应用程序服务 Web 应用程序](http://go.microsoft.com/fwlink/?LinkId=529714)的克隆功能，可以轻松地复制到新创建的应用程序，在其他地区或在同一个地区的现有 web 应用程序。 这将使客户能够快速而轻松地在不同地区中部署多个应用程序。

应用程序克隆目前仅支持高级层的应用程序服务规划。 新的功能作为 Web 应用程序备份功能使用相同的限制，请参阅[备份在 Azure 应用程序服务 web 应用程序](web-sites-backup.md)。

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)] 


## <a name="cloning-an-existing-app"></a>克隆一个现有的应用程序 ##

使 web 应用程序必须运行在**高级**模式下使您可以创建 web 应用程序的克隆。

1. 在[Azure 门户](https://portal.azure.com/)中，打开您的 web 应用程序的刀片式服务器。
2. 单击**工具**。 然后，在**工具**刀片式服务器，请单击**克隆应用程序**。

    ![][1]

    > [AZURE.NOTE]
    > 如果 web 应用程序不在**高级**模式中，您将收到一条消息，指示应用程序克隆的支持的模式。 此时，您可以选择**升级**。
    
3. **克隆的应用程序**刀片式服务器中提供的名称的新 web 应用程序、 资源组和应用程序服务计划。 此外，用户将能够选择是否要克隆源 web 应用程序设置一个数字或不。

    ![][2]

4. 单击**创建**后平台将启动创建克隆源 web 应用程序的工作。

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>克隆到一个应用程序服务环境，对现有应用程序##

**克隆的应用程序**刀片式服务器中客户都可以在现有的应用程序服务环境中选择应用程序池。

## <a name="current-restrictions"></a>当前限制 ##

此功能目前在预览中，我们正在随着时间的推移添加新的功能，下面的列表是当前支持的应用程序克隆 Azure 门户中已知的限制︰

- 不是被克隆的 azure 的流量管理器设置
- 自动缩放设置不被克隆
- 不是被克隆的备份时间表设置
- VNET 设置不被克隆
- 应用程序的见解都没有自动设置目标 web 应用程序上
- 简单身份验证设置不被克隆
- Kudu 扩展名不是被克隆
- 提示规则不被克隆
- 数据库内容不被克隆


### <a name="references"></a>引用 ###
- [Web 应用程序克隆使用 PowerShell](app-service-web-app-cloning.md)
- [备份在 Azure 应用程序服务 web 应用程序](web-sites-backup.md)
- [如何创建应用程序服务环境](app-service-web-how-to-create-an-app-service-environment.md)
- [在一个应用程序服务的环境中创建 web 应用程序](app-service-web-how-to-create-a-web-app-in-an-ase.md)
- [应用程序服务环境简介](app-service-app-service-environment-intro.md)

<!--Image references-->
[1]: ./media/app-service-web-app-cloning-portal/CloningBlade.png
[2]: ./media/app-service-web-app-cloning-portal/CloneSettings.png