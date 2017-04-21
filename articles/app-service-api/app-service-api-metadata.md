<properties
    pageTitle="应用程序服务 API 的应用程序元数据 API 查询和代码生成 |Microsoft Azure"
    description="了解如何在 Azure 应用程序服务的 API 应用程序使用 Swagger 元数据简化 API 查询和代码生成。"
    services="app-service\api"
    documentationCenter=".net"
    authors="tdykstra"
    manager="wpickett"
    editor=""/>

<tags
    ms.service="app-service-api"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/30/2016"
    ms.author="rachelap"/>

# <a name="app-service-api-apps-metadata-for-api-discovery-and-code-generation"></a>应用程序服务 API 的应用程序元数据 API 查询和代码生成 

应用程序服务 API 应用程序内置[Swagger 2.0](http://swagger.io/) API 元数据的支持。 您不需要使用 Swagger，但是如果您使用它，您可以利用 API 的应用程序功能，便于发现和消耗。   

## <a name="swagger-endpoint"></a>Swagger 终结点

您可以指定终结点提供 Swagger 2.0 JSON 元数据 API 使应用程序 API 的应用程序的属性中。 终结点可以是相对于 API 的应用程序的基 URL 或绝对 URL。 绝对 Url 可以指向外部 API 的应用程序。 

许多下游客户端 （用于例如 Visual Studio 代码生成和 PowerApps"添加 API"流），该 URL 必须是可公开访问 （不受用户或服务身份验证）。 这意味着，如果您正在使用的应用程序服务的身份验证，想要公开的 API 定义您自己的应用程序中需要使用身份验证选项，允许匿名通信到达您的 API。 有关详细信息，请参阅[身份验证和授权的应用程序服务 API 的应用程序](app-service-api-authentication.md)。

### <a name="portal-blade"></a>门户刀片

在[Azure 门户](https://portal.azure.com/)可以看到和更改**API 定义**刀片上端点 URL。

![](./media/app-service-api-metadata/apidefblade.png)

### <a name="azure-resource-manager-property"></a>Azure 的资源管理器属性

您还可以通过使用[Azure PowerShell](../powershell-install-configure.md)和[Azure CLI](../xplat-cli-install.md)命令行工具中的[资源管理器](https://resources.azure.com/)或[Azure 资源管理器模板](../resource-group-authoring-templates.md)配置 API 使应用程序的 API 定义 URL。 

在**资源管理器**中，转到**订阅 > {订购} > resourceGroups > {资源组} > 提供 > Microsoft.Web > 站点 > {网站} > 配置 > web**，您将看到`apiDefinition`属性︰

        "apiDefinition": {
          "url": "https://contactslistapi.azurewebsites.net/swagger/docs/v1"
        }

有关设置 Azure 资源管理器模板的示例`apiDefinition`属性，打开[待办事项列表示例应用程序中的 azuredeploy.json 文件](https://github.com/azure-samples/app-service-api-dotnet-todo-list/blob/master/azuredeploy.json)。 查找类似于上面所示的 JSON 示例模板的部分。

### <a name="default-value"></a>默认值

当使用 Visual Studio 创建 API 的应用程序时，该 API 定义终结点将自动设置为加上 API 的应用程序的基 URL `/swagger/docs/v1`。 这是[Swashbuckle](https://www.nuget.org/packages/Swashbuckle) NuGet 程序包所使用的用于动态生成的 ASP.NET Web API 项目 Swagger 元数据的默认 URL。 

## <a name="code-generation"></a>代码生成

将 Swagger 集成到 Azure API 的应用程序的好处之一是自动代码生成。 生成的客户端类更加轻松地编写代码，调用 API 的应用程序。

您可以通过使用 Visual Studio 或从命令行生成 API 使应用程序的客户端代码。 有关如何为 ASP.NET Web API 项目在 Visual Studio 中生成客户端类的信息，请参阅[开始使用 API 的应用程序，ASP.NET](app-service-api-dotnet-get-started.md#codegen)。 有关如何从命令行的所有支持的语言的信息，请参阅在 GitHub.com 上的[Azure/autorest](https://github.com/azure/autorest)存储库中的自述文件。
 
## <a name="next-steps"></a>下一步行动

引导您逐步创建一个分步教程，部署，和使用 API 的应用程序，请参阅[开始使用 API 在 Azure 应用程序服务的应用程序](app-service-api-dotnet-get-started.md)。

如果 Azure API 管理使用 API 的应用程序，您可以使用 Swagger 元数据来将您的 API 导入 API 管理。 有关详细信息，请参阅[如何导入的操作在 Azure API 管理 API 定义](../api-management/api-management-howto-import-api.md)。 
