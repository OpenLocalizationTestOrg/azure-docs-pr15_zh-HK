<properties
    pageTitle="身份验证和授权的 API 在 Azure 应用程序服务的应用程序 |Microsoft Azure"
    description="了解有关用于 API 的应用程序提供的 Azure 应用程序服务的身份验证和授权服务。"
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
    ms.date="05/23/2016"
    ms.author="rachelap"/>

# <a name="authentication-and-authorization-for-api-apps-in-azure-app-service"></a>身份验证和授权的 API 在 Azure 应用程序服务的应用程序

## <a name="overview"></a>概述 

> [AZURE.NOTE] 本主题将被迁移到统一[应用程序服务的身份验证 / 授权](../app-service/app-service-authentication-overview.md)主题，包括 Web、 移动设备、 和 API 的应用程序。

Azure 应用程序服务提供内置实现[OAuth 2.0](#oauth)和[OpenID 连接](#oauth)的身份验证和授权服务。 本文介绍的服务和所提供的 API 在 Azure 应用程序服务的应用程序的选项。

下图说明了应用程序服务的身份验证的一些主要特征︰

* 它预处理传入 API 请求，这意味着它适用于任何语言或框架支持的应用程序服务。
* 它提供几个选项用于多少身份验证起作用，您想要在您自己的代码中执行操作。
* 它适用于最终用户和服务帐户身份验证。 
* 它支持五个标识提供程序︰ Azure Active Directory、 Facebook、 Google、 Twitter，以及 Microsoft 帐户。
* 该 API 的应用程序、 Web 应用程序和移动应用程序的作用相同。

![](./media/app-service-api-authentication/api-apps-overview.png)

## <a name="language-agnostic"></a>语言不可知

请求到达您的 API 应用程序，这意味着 API 用任何语言或框架编写的应用程序的身份验证功能起作用之前，将出现应用程序服务的身份验证处理。  您的 API 可以基于 ASP.NET、 Java、 Node.js 或应用程序服务所支持的任何框架。

应用程序服务通过在 HTTP 请求的授权标头中的 JSON web 标记 (JWT) 并且用任何语言或框架编写的代码可以从标记获取所需的信息。 此外，应用程序服务为您提供更方便的访问最常用的索赔通过设置一些特殊的标头，如下所示︰

* X-MS-客户端的主要用户名称
* X-MS-客户端-主体的 ID
* X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN
* X-MS-TOKEN-FACEBOOK-EXPIRES-ON
 
在.NET API，您可以使用`Authorize`特性，并进行细粒度授权您可以轻松地编写代码根据的索赔由于理赔信息为您填充在.NET 类。

## <a name="multiple-protection-options"></a>多个保护选项

应用程序服务可以防止匿名 HTTP 请求到达您 API 的应用程序，它可以传递所有请求并验证令牌的请求的或它可以让所有请求通过而无需对其采取任何行动︰

1. 允许仅通过身份验证的请求到达您 API 的应用程序。

    应用程序服务在浏览器中收到匿名请求，如果将重定向到登录页的身份验证提供程序 (Azure 的广告，Google，Twitter，等等) 的选择。 

    使用此选项，您不需要编写任何身份验证代码根本在您的应用程序，并授权码简化，因为中的 HTTP 标头提供了最重要的声明。

2. 允许所有请求到达您 API 的应用程序，但验证经过身份验证的请求和身份验证的 HTTP 标头中的信息传递。

    此选项使您可以更灵活地处理匿名请求，但您必须编写代码，如果您想要防止匿名用户使用您的 API。 由于 HTTP 请求标头中传递的最受欢迎的声明，授权码是相对比较简单。
    
3. 允许所有请求到达您的 API，对身份验证信息的请求中不执行任何操作。

    该选项使您的身份验证和授权完全由应用程序代码中的任务。

在[Azure 的门户](https://portal.azure.com/)中，选择所需的选项**身份验证 / 授权**刀片式服务器。

![](./media/app-service-api-authentication/authblade.png)

选项 1 和 2，打开**应用程序服务的身份验证**，并**请求没有通过身份验证时要采取的操作**下拉列表中选择**登录**或**允许请求 （无操作）**。  如果您选择**以用户身份登录**，您必须选择一个身份验证提供程序和配置该提供程序。

![](./media/app-service-api-authentication/actiontotake.png)

有关如何配置身份验证的详细信息，请参阅[如何配置应用程序服务应用程序使用 Azure Active Directory 登录](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md)。 文章将应用于 API 的应用程序和移动应用程序，和它链接到的其他文章的其他身份验证提供程序。
 
## <a id="internal"></a>服务帐户身份验证

应用程序服务的身份验证适用于如到另一个 API 的应用程序从一个 API 应用程序调用的内部情况。 在这种情况下您使用凭据，而不是最终用户凭据的服务帐户获得令牌。 服务帐户也称为是 Azure Active Directory 中的*服务主体*，使用此类帐户的身份验证也称为是一种服务的服务方案。 

对于提供服务的方案，保护被调用的 API 应用程序使用 Azure Active Directory 并提供 AAD 服务主体授权标记，当您调用 API 的应用程序。 通过提供客户端 ID 和机密 AAD 应用程序从客户端获取令牌。 如用来处理移动服务 Zumo 标记的真理，是必需的没有特殊的 Azure 专用代码。 举例说明这种情况下，使用 ASP.NET API 应用程序教程[用于 API 的应用程序服务主体身份验证](app-service-api-dotnet-service-principal-auth.md)范围之内。

如果您想要处理而无需使用应用程序服务的身份验证服务的服务方案，您可以使用客户端证书或基本身份验证。 有关在 Azure 中的客户端证书的信息，请参阅[如何向配置 TLS 相互身份验证的 Web 应用程序](../app-service-web/app-service-web-configure-tls-mutual-auth.md)。 有关基本身份验证，ASP.NET 中的信息，请参见[ASP.NET Web API 2 中的身份验证筛选器](http://www.asp.net/web-api/overview/security/authentication-filters)。

服务帐户身份验证应用程序服务的逻辑应用程序从到 API 的应用程序是[您自定义的 API 位于与应用程序逻辑的应用程序服务使用](../app-service-logic/app-service-logic-custom-hosted-api.md)所述的特殊情况。

## <a name="mobile-client-authentication"></a>移动客户端身份验证

有关如何处理从移动客户端的身份验证信息，请参阅[移动应用程序的身份验证的文档](../app-service-mobile/app-service-mobile-ios-get-started-users.md)。 应用程序服务的身份验证的移动应用程序和 API 的应用程序工作原理相同。
  
## <a name="more-information"></a>详细信息

有关身份验证和授权在 Azure 应用程序服务的详细信息，请参阅以下资源︰

* [扩展的应用程序服务的身份验证 / 授权](/blog/announcing-app-service-authentication-authorization/)
* [如何配置应用程序服务应用程序使用 Azure Active Directory 登录](../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md)（对于其他身份验证提供程序，在页面的顶部包括链接）。 

OAuth 2.0，OpenID 连接，和 JSON Web 标记 (JWT) 的更多信息，请参见以下资源。

* [要开始使用 OAuth 2.0](http://shop.oreilly.com/product/0636920021810.do "要开始使用 OAuth 2.0") 
* [连接到 OAuth2，OpenID 的简介和 JSON Web 标记 (JWT) 的 PluralSight 课程](http://www.pluralsight.com/courses/oauth2-json-web-tokens-openid-connect-introduction) 
* [生成和保护 ASP.NET-PluralSight 课程中的多个客户端一个 rest 风格的 API](http://www.pluralsight.com/courses/building-securing-restful-api-aspdotnet)

关于 Azure 活动目录的详细信息，请参阅以下资源。

* [Azure 的广告方案](http://aka.ms/aadscenarios)
* [Azure 广告开发人员指南](http://aka.ms/aaddev)
* [Azure 的广告样本](http://aka.ms/aadsamples)

## <a name="next-steps"></a>下一步行动

这篇文章讲述了用于 API 的应用程序可以使用的应用程序服务的身份验证和授权功能。 获取启动序列中的下一个教程演示如何实现[在应用程序服务 API 应用程序中的用户身份验证](app-service-api-dotnet-user-principal-auth.md)。
