<properties
    pageTitle="应用程序服务应用程序的 API 会发生什么变化 |Microsoft Azure"
    description="了解什么是新的 API 在 Azure 应用程序服务的应用程序。"
    services="app-service\api"
    documentationCenter=".net"
    authors="mohitsriv"
    manager="wpickett"
    editor="tdykstra"/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/29/2016"
    ms.author="rachelap"/>

# <a name="app-service-api-apps---whats-changed"></a>应用程序服务 API 应用程序中的更改的内容

在 Connect() 事件中 11 月 2015年，数到 Azure 应用程序服务的改进已[宣布](https://azure.microsoft.com/blog/azure-app-service-updates-november-2015/)。 这些改进包括基础对 API 的应用程序来更好地配合移动和 Web 应用程序、 减少概念数量和提高部署和运行时性能。 开始于 2015 年 11 月 30 日，新 API 应用程序使用 Azure 管理门户创建或最新的工具将会反映这些更改。 本文介绍了这些更改，以及如何重新部署现有的应用程序来充分利用的功能。

## <a name="feature-changes"></a>功能更改
主要功能的 API 应用程序--身份验证、 CORS 和 API 元数据--已经直接在应用程序服务移动。 进行此更改后，可以使用的功能在 Web、 移动和 API 的应用程序。 事实上，这三个具有相同的**Microsoft.Web/sites**资源类型资源管理器中。 不再需要或提供 API 的应用程序的 API 应用程序网关。 这也可以容易地将只是一个 API 管理网关使用 Azure API 管理。

![API 的应用程序概述](./media/app-service-api-whats-changed/api-apps-overview.png)

使用此 API 的应用程序更新的主要设计原则是使您可以提供您的 API 是用您选择的语言。  如果已为 Web 应用程序或移动应用程序部署您的 API，您不必重新部署您的应用程序，以利用新的功能。 如果您正在按 API 应用程序预览，迁移指南阐述如下。

### <a name="authentication"></a>身份验证
现有承包 API 的应用程序、 移动服务应用程序和 Web 应用程序的身份验证功能进行了统一，并在管理门户中的单个 Azure 应用程序服务的身份验证刀片式服务器中可用。 在应用程序服务的身份验证服务的简介，请参见[扩展的应用程序服务的身份验证 / 授权](https://azure.microsoft.com/blog/announcing-app-service-authentication-authorization/)。

API 的情况下，有大量的相关的新功能︰

- **对使用 Azure Active Directory 直接的支持**，而不必交换会话令牌的 AAD 令牌的客户端代码︰ 您的客户端只能包括 AAD 令牌在身份验证头中，按照持有者标记规范。 这也意味着任何特定的应用程序服务 SDK 需要在客户端或服务器端。 
- **服务于服务或"内部"访问**︰ 如果您有一个守护进程或不用不使用界面访问 Api 的某些其他客户端，可以申请使用 AAD 服务主体的令牌并将其传递给应用程序服务的身份验证与您的应用程序。
- **已推迟授权**︰ 许多应用程序具有不同的访问限制为应用程序的不同部分。 您可能希望某些 Api 是公用的而其他人则要求登录。 原始的身份验证/授权功能不全，与整个网站需要登录。 此选项仍然存在，但您也可以允许应用程序代码以应用程序服务已验证用户身份后呈现访问决策。
 
新的身份验证功能的详细信息，请参阅[身份验证和授权的 API 在 Azure 应用程序服务的应用程序](app-service-api-authentication.md)。 有关如何迁移现有 API 的应用程序从以前的 API 的应用程序模型到新本文内下文中看到[迁移现有 API 的应用程序](#migrating-existing-api-apps)。
 
### <a name="cors"></a>CORS
而不是逗号分隔**MS_CrossDomainOrigins**应用程序设置，现在刀片式服务器中没有配置 CORS Azure 的管理门户。 另外，可以配置使用资源管理器工具如 Azure PowerShell，CLI 或[资源管理器](https://resources.azure.com/)。 **Cors**属性类型上设置了**Microsoft.Web/sites/config**资源为您**&lt;网站名称&gt;/web**资源。 例如︰

    {
        "cors": {
            "allowedOrigins": [
                "https://localhost:44300"
            ]
        }
    } 

### <a name="api-metadata"></a>元数据 API
API 定义刀片式服务器现在就可以跨 Web、 移动和 API 的应用程序。 在管理门户中，您可以指定相对 url 或绝对 url 指向您的 API 的该主机 Swagger 2.0 表示终结点。 另外，可以配置使用资源管理器工具。 将**apiDefinition**属性设置为对**Microsoft.Web/sites/config**资源类型您**&lt;网站名称&gt;/web**资源。 例如︰

    {
        "apiDefinition":
        {
            "url": "https://myStorageAccount.blob.core.windows.net/swagger/apiDefinition.json"
        }
    }

这次的元数据终结点必须是无需身份验证对于许多的下游客户端 （例如 Visual Studio REST API 客户端生成和 PowerApps"添加 API"流），使用它可公开访问。 这意味着如果您使用的应用程序服务的身份验证，想要公开的 API 定义您自己的应用程序中您将需要使用前面所述，以便为您 Swagger 元数据的路由是公共的延迟身份验证选项。

## <a name="management-portal"></a>管理门户
选择**新建 > Web + 移动 > API 应用程序**在门户中创建 API 应用程序反映文章中所介绍的新功能。 **浏览 > API 应用程序**将仅显示这些新 API 的应用程序。 一旦您浏览到应用程序 API，刀片式服务器共享相同的布局和功能的 Web 和移动应用程序。 唯一的差别是快速入门内容和顺序的设置。

现有的 API 应用程序 （或市场 API 应用程序逻辑的应用程序中创建） 与以前的预览功能仍然可以看到逻辑应用程序设计器中，当浏览一个资源组中的所有资源。

## <a name="visual-studio"></a>Visual Studio

刀具的大多数 Web 应用程序将使用新 API 的应用程序，因为它们共享相同的基础**Microsoft.Web/sites**资源类型。 Azure 的 Visual Studio 工具，但是，应该升级到版本 2.8.1 或更高版本因为它提供了大量的特有的 Api 的功能。 从[Azure 的下载页](https://azure.microsoft.com/downloads/)中下载 SDK。

使用应用程序服务类型的合理化，将发布也统一在**发布 > Microsoft Azure 应用程序服务**:

![API 的应用程序发布](./media/app-service-api-whats-changed/api-apps-publish.png)

SDK 2.8.1 有关的详细信息，请参阅[博客文章](https://azure.microsoft.com/blog/announcing-azure-sdk-2-8-1-for-net/)发布。

或者，您可以手动导入发布配置文件从管理门户启用发布。 但是，云资源管理器中，代码生成和 API 的应用程序选择/创建需要 SDK 2.8.1 或更高版本。

## <a name="migrating-existing-api-apps"></a>将现有的 API 应用程序迁移
如果您自定义的 API 部署到以前的预览版本的 API 应用程序中，我们请求您迁移到新的模式为 API 的应用程序通过于 2015 年 12 月 31 日。 由于同时的旧的和新的模型基于 Web 承载的应用程序服务中的 Api，可以重用现有代码的大部分。

### <a name="hosting-and-redeployment"></a>承载和重新部署
重新部署的步骤是将任何现有的 Web API 部署到应用程序服务相同。 步骤如下︰

1. 创建一个空的 API 应用程序。 这可以在门户中使用新建 > API 的应用程序，在 Visual Studio 中发布，或从资源管理器工具。 使用资源管理器工具或模板时，如果**类型**值设置为**api**在**Microsoft.Web/sites**资源类型具有快速入门并设置 API 方案向面向管理门户中。
2. 连接并将项目部署到空 API 应用程序使用的任何应用程序服务所支持的部署机制。 阅读[Azure 应用程序服务部署文档](../app-service-web/web-sites-deploy.md)，以了解详细信息。 
  
### <a name="authentication"></a>身份验证
应用程序服务的身份验证服务支持先前的 API 的应用程序模型中可用的相同功能。 如果您正在使用会话令牌，并且需要 Sdk，使用下面的客户端和服务器 Sdk:

- 客户端︰ [Azure 的移动客户端 SDK](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- 服务器︰ [Microsoft Azure 移动应用程序.NET 身份验证扩展插件](http://www.nuget.org/packages/Microsoft.Azure.Mobile.Server.Authentication/) 

如果您改为使用应用程序服务 alpha 的 Sdk，这些现在都被否决︰

- 客户端︰ [Microsoft Azure AppService SDK](http://www.nuget.org/packages/Microsoft.Azure.AppService)
- 服务器︰ [Microsoft.Azure.AppService.ApiApps.Service](http://www.nuget.org/packages/Microsoft.Azure.AppService.ApiApps.Service)

尤其是对于 Azure Active Directory，但是，没有应用程序-特定于服务的需要进行如果直接使用 AAD 令牌。

### <a name="internal-access"></a>内部访问
先前的 API 的应用程序模型包含了一个内置的内部访问级别。 这需要使用 SDK 的签名请求。 如前所述，使用新 API 的应用程序模型中，AAD 服务主体可作为备用服务到服务身份验证而不要求应用程序服务的特定 SDK。 了解更多的[服务主体在 Azure 应用程序服务的 API 应用程序的身份验证](app-service-api-dotnet-service-principal-auth.md)。

### <a name="discovery"></a>发现
先前的 API 的应用程序模型有 Api 用于发现其他 API 的应用程序在运行时，后面的相同的网关同一资源组中。 这是在实现 microservice 模式的体系结构中尤其有用。 虽然这不直接支持，许多选项是可用的︰

1. 对发现使用 Azure 资源管理器 API。
2. 放置在您的应用程序服务承载 Api 的 Azure API 管理。 Azure 的 API 管理作为表面，并可提供稳定的外部对开 url，即使内部拓扑结构发生了变化。
3. 生成您自己发现 API 的应用程序和已发现应用程序在启动时注册其他 API 应用程序。
4. 在部署时，填充的其他 API 应用程序终结点的所有 API 的应用程序 （和客户端） 应用程序设置。 这是在模板部署，因为 API 的应用程序现在为您提供可行的 url 的控件。

## <a name="using-api-apps-with-logic-apps"></a>与逻辑应用程序使用 API 的应用程序

新的 API 的应用程序模型适用于[逻辑应用程序架构版本 2015年-08-01](../app-service-logic/app-service-logic-schema-2015-08-01.md)。

## <a name="next-steps"></a>下一步行动

若要了解详细信息，请阅读文章[API 的应用程序的文档部分](https://azure.microsoft.com/documentation/services/app-service/api/)。 已更新它们以反映新的模型对于 API 的应用程序。 此外，不要建立亲密关系上的更多详细信息或指导迁移论坛︰

- [MSDN 论坛](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureAPIApps)
- [堆栈溢出](http://stackoverflow.com/questions/tagged/azure-api-apps)
