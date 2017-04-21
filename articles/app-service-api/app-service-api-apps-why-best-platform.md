<properties 
    pageTitle="介绍 API 的应用程序 |Microsoft Azure" 
    description="了解如何 Azure 应用程序服务可以帮助您开发，主机，并使用 rest 风格的 Api。" 
    services="app-service\api" 
    documentationCenter=".net" 
    authors="tdykstra" 
    manager="wpickett" 
    editor=""/>

<tags 
    ms.service="app-service-api" 
    ms.workload="web" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="08/23/2016" 
    ms.author="rachelap"/>

# <a name="api-apps-overview"></a>API 的应用程序概述

API 在 Azure 应用程序服务的应用程序提供功能，便于开发，主机，使用在云和内部 Api。 与 API 应用程序您可以获得企业级安全、 简单的访问控制、 混合连接、 自动 SDK 生成和与[逻辑应用程序](../app-service-logic/app-service-logic-what-are-logic-apps.md)无缝集成。

[Azure 应用程序服务](../app-service/app-service-value-prop-what-is.md)是为 web、 移动，一个完全托管的平台和集成方案。 API 的应用程序是[Azure 应用程序服务](../app-service/app-service-value-prop-what-is.md)提供的四种应用程序类型之一。

![在 Azure 应用程序服务的应用程序类型](./media/app-service-api-apps-why-best-platform/appservicesuite.png)

## <a name="why-use-api-apps"></a>为什么使用 API 的应用程序？

以下是一些关键功能的 API 的应用程序︰

- **将作为您现有的 API 的是**-您不需要更改任何您现有的 Api，可以利用 API 的应用程序，只需将您的代码部署到 API 应用程序中的代码。 任何语言或应用程序服务，包括 ASP.NET 和 C#、 Java、 PHP、 Node.js，以及 Python 支持的框架，可以使用您的 API。

- **很容易消耗**- [Swagger API](http://swagger.io/)元数据集成的支持使您的 Api 方便地可由各种客户端。  为各种语言，包括 C#、 Java 和 Javascript 中的 Api 在您自动生成客户端代码。 轻松地配置[CORS](app-service-api-cors-consume-javascript.md)而无需更改代码。 有关详细信息，请参阅[API 查询和代码生成的应用程序服务 API 的应用程序元数据](app-service-api-metadata.md)和[来自使用 CORS 的 JavaScript API 应用程序使用](app-service-api-cors-consume-javascript.md)。 

- **简单的访问控制**-防止无变化的未经身份验证访问 API 的应用程序到您的代码。 通过其他服务或代表用户的客户端进行内置身份验证服务安全访问的 Api。 支持的标识提供程序包括 Azure Active Directory、 Facebook、 Twitter、 Google 和 Microsoft 帐户。 客户端可以使用活动目录身份验证库 (ADAL) 或移动应用程序 SDK。 有关详细信息，请参阅[身份验证和授权的 API 在 Azure 应用程序服务的应用程序](app-service-api-authentication.md)。

- **Visual Studio 集成**-在 Visual Studio 中的专用工具简化创建、 部署、 使用、 调试和管理 API 的应用程序的工作。 有关详细信息，请参阅[发布 Azure SDK 2.8.1.net](/blog/announcing-azure-sdk-2-8-1-for-net/)。

- **与逻辑应用程序集成**的 API 的应用程序创建可以由[应用程序服务的逻辑应用程序](../app-service-logic/app-service-logic-what-are-logic-apps.md)使用。  有关详细信息，请参阅[的使用您自定义的 API 位于与应用程序逻辑的应用程序服务](../app-service-logic/app-service-logic-custom-hosted-api.md)和[新架构版本 2015年-08-01 的预览](../app-service-logic/app-service-logic-schema-2015-08-01.md)。

此外，API 的应用程序可以利用[Web 应用程序](../app-service-web/app-service-web-overview.md)和[移动应用程序](../app-service-mobile/app-service-mobile-value-prop.md)提供的功能。 相反也是如此︰ 如果您使用的 web 应用程序或移动应用程序来承载 API，它可以利用 API 的应用程序功能，如 Swagger 元数据的客户端代码生成和 CORS 跨域浏览器访问。 三种应用程序类型 （API、 web、 移动） 之间唯一的区别是名称和图标在 Azure 门户中使用它们。

## <a name="whats-the-difference-between-api-apps-and-azure-api-management"></a>API 的应用程序和 Azure API 管理之间的区别是什么？

API 的应用程序和[Azure API 管理](../api-management/api-management-key-concepts.md)是互为补充的服务︰

* API 管理用于管理 Api。 您放入 API 管理前端 API 与监视器和调节的使用、 操作的输入和输出，将几个 Api 合并为一个端点，等等。 被托管的 Api 可以承载任何地方。
* 有关宿主 Api 是 API 的应用程序。 此项服务包括功能，便于开发和花费很多的 Api，但它不会做的各种监视、 调节、 操作和整合这些 API 管理。 如果您不需要 API 管理功能，可以不使用 API 管理 API 应用程序中承载 Api。

这里有一个示意图，说明了使用的 Api 的托管 API 应用程序中和其他地方的 API 管理。

![Azure API 管理和 API 的应用程序](./media/app-service-api-apps-why-best-platform/apia-apim.png)

API 管理和 API 的应用程序的一些功能具有类似的功能。  例如，两者都可以自动完成 CORS 支持。 当同时使用这两项服务时，将使用 API 管理 CORS 因为它作为 API 的应用程序的前端。 

## <a name="getting-started"></a>入门教程

入门 API 的应用程序部署到一个代码示例，请参阅本教程为您喜欢的任何一个框架︰

* [ASP.NET](app-service-api-dotnet-get-started.md) 
* [Node.js](app-service-api-nodejs-api-app.md) 
* [Java](app-service-api-java-api-app.md) 

API 的应用程序的提问，请[论坛 API 应用程序](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureAPIApps)中启动的线程。 
