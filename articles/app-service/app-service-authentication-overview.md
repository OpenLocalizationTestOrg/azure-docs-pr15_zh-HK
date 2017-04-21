<properties
    pageTitle="身份验证和授权 Azure 应用程序服务中的 |Microsoft Azure"
    description="概念的引用，并概述了身份验证 / 授权功能 Azure 应用程序服务"
    services="app-service"
    documentationCenter=""
    authors="mattchenderson"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="08/29/2016"
    ms.author="mahender"/>

# <a name="authentication-and-authorization-in-azure-app-service"></a>身份验证和授权在 Azure 应用程序服务

## <a name="what-is-app-service-authentication--authorization"></a>什么是应用程序服务的身份验证 / 授权？

应用程序服务的身份验证 / 授权是一种为用户提供登录，以便您不必更改代码的应用程序的后端应用程序提供一种功能。 它提供了一种简便方法保护您的应用程序以及与每个用户的数据。

应用程序服务使用联合的身份，第三方身份标识提供程序存储帐户，并对用户进行身份验证。 应用程序依赖于提供程序的标识信息，以便应用程序不需要存储的信息本身。 应用程序服务支持五个身份提供开箱即用︰ Azure Active Directory、 Facebook、 Google、 Microsoft 帐户和 Twitter。 您的应用程序可以使用任意数量的这些标识提供程序为您的用户提供他们登录的选项。 若要展开的内置支持，您可以集成其他身份标识提供程序或[自定义标识解决方案][custom-auth]。

如果您希望立即开始工作，请参阅下面的教程之一︰

- [添加到您的 iOS 应用程序的身份验证][ iOS] （或[Android]、 [Windows]、 [Xamarin.iOS]、 [Xamarin.Android]、 [Xamarin.Forms]或[Cordova]）
- [API 在 Azure 应用程序服务的应用程序的用户身份验证][apia-user]
- [开始使用 Azure 应用程序服务-第 2 部分][web-getstarted]

## <a name="how-authentication-works-in-app-service"></a>在应用程序服务的身份验证工作原理

为了通过一个标识提供程序进行身份验证，首先需要配置标识提供程序以了解您的应用程序。 Id 和您为应用程序服务提供的机密信息，然后将为身份标识提供程序。 至此，以便应用程序服务可以验证用户的断言，如身份验证令牌，从标识提供程序的信任关系。

若要使用这些提供程序之一登录用户，用户必须被重定向到用户在签名为该提供程序终结点。 如果客户正在使用 web 浏览器，您可以自动将引导所有未经授权的用户，在用户签名的端点的应用程序服务。 否则，您需要直接向您的客户`{your App Service base URL}/.auth/login/<provider>`，其中`<provider>`是下列值之一︰ aad，facebook，google，微软或 twitter。 移动和 API 的方案将在本文后面的部分解释。

通过 web 浏览器应用程序进行交互的用户必须设置，以便在浏览您的应用程序他们可以保持已经过身份验证 cookie。 对于其他客户端类型，如移动，一个 JSON web 标记 (JWT)，它应该显示在`X-ZUMO-AUTH`标头，将颁发给客户端。 移动应用程序客户端 Sdk 将为您处理此。 另外，Azure Active Directory 身份令牌或访问令牌可以直接包含在`Authorization`作为[载体标记](https://tools.ietf.org/html/rfc6750)的标题。

应用程序服务将所有 cookie 或应用程序问题来验证用户的令牌进行都验证。 要将限制谁可以访问您的应用程序，请参阅本文内下文中的[授权](#authorization)部分。

### <a name="mobile-authentication-with-a-provider-sdk"></a>移动与 SDK 提供的身份验证

一切配置后端后，您可以修改移动客户端应用程序服务使用登录。 有以下两种方法︰

- 使用给定的标识提供者发布了建立身份并再访问应用程序服务 SDK。
- 使用一行代码，以便在用户对移动应用程序客户端 SDK 进行。

>[AZURE.TIP] 大多数应用程序应使用 SDK 的提供得到更一致的体验，用户在登录时，使用刷新的支持，并获得该提供程序指定的其他好处。

在使用 SDK 的提供程序时，用户可以登录到该应用程序正在其运行的操作系统与更紧密集成的体验。 这也使您提供程序标记以及某些用户信息在客户端，这使它更容易使用 graph Api 和自定义的用户体验。 偶尔在博客和论坛上，您将看到这所谓的"客户端流程"或"客户端定向流"因为代码在客户端登录用户和客户端代码可以访问提供程序标记。

获取提供程序标记后，它需要验证发送到应用程序服务。 应用程序服务验证该令牌之后，应用程序服务创建新的应用程序服务令牌返回到客户端。 移动应用程序客户端 SDK 的帮助器方法来管理这种交换并自动将标记附加到后端应用程序的所有请求。 如果它们这样选择，开发人员还可以保留提供程序标记的引用。

### <a name="mobile-authentication-without-a-provider-sdk"></a>移动没有 SDK 提供程序的身份验证

如果不想设置提供程序 SDK，您可以允许 Azure 应用程序服务注册为您的移动应用程序功能。 移动应用程序客户端 SDK 将打开一个 web 视图到您选择的提供程序和登录用户。 偶尔在博客和论坛上，您将看到该称为"服务器流程"或"服务器定向流"因为服务器管理的用户，在签署过程和客户端 SDK 永远收不到的提供程序标记。

身份验证为每个平台教程中包含代码以启动此流程。 在流的末尾，客户端 SDK 具有应用程序服务的标记，该标记会自动附加到后端应用程序的所有请求。

### <a name="service-to-service-authentication"></a>服务的服务身份验证

虽然您可以授予用户访问您的应用程序，您也可以信任另一个应用程序调用您自己的 API。 例如，可能有一个 web 应用程序在另一个 web 应用程序中调用的 API。 在此方案中，您使用凭据而不是用户凭据的服务帐户获得令牌。 服务帐户也称为是 Azure Active Directory 测定*服务主体*和使用此类帐户的身份验证也称为是一种服务的服务方案。

>[AZURE.IMPORTANT] 由于在客户设备上运行的移动应用程序，移动应用程序执行_不_作为受信任的应用程序，并且不应该使用一个服务主体流的计数。 相反，他们应该使用前面所描述的用户流量。

服务的服务情况下，应用程序服务可以使用 Azure Active Directory 保护您的应用程序。 调用应用程序只需提供获得通过提供客户端 ID 和客户端密钥从 Azure 的 Active Directory Azure Active Directory 服务主体授权令牌。 ASP.NET API 应用程序使用此方案的一个示例说明该教程，[用于 API 的应用程序服务主体身份验证][apia-service]。

如果您想要使用的应用程序服务的身份验证来处理服务的服务方案，您可以使用客户端证书或基本身份验证。 有关在 Azure 中的客户端证书的信息，请参阅[如何向配置 TLS 相互身份验证的 Web 应用程序](../app-service-web/app-service-web-configure-tls-mutual-auth.md)。 有关基本身份验证，ASP.NET 中的信息，请参见[ASP.NET Web API 2 中的身份验证筛选器](http://www.asp.net/web-api/overview/security/authentication-filters)。

服务帐户身份验证应用程序服务的逻辑应用程序从到 API 的应用程序是一种特殊情况的详细信息，[使用您自定义的 API 位于与应用程序逻辑的应用程序服务](../app-service-logic/app-service-logic-custom-hosted-api.md)。

## <a name="authorization"></a>在应用程序服务的授权工作原理

您可以完全控制可以访问您的应用程序的请求。 应用程序服务的身份验证 / 授权可以配置任何下列行为︰

- 允许仅通过身份验证的请求到达您的应用程序。

    浏览器收到匿名请求时，如果应用程序服务将重定向到一个页面选择，以便用户可以登录标识提供程序上。 如果该请求来自移动设备时，返回的响应是一个 HTTP _401 未经授权_响应。

    使用此选项，您不需要在所有您的应用程序中编写任何身份验证代码。 如果您需要更细的授权，关于用户的信息是您的代码可用。

- 允许所有请求到达您的应用程序，但验证经过身份验证的请求，并传递身份验证的 HTTP 标头中的信息。

    此选项将推迟到您的应用程序代码的授权决策。 它提供了更灵活地处理匿名请求，但您必须编写的代码。

- 允许访问您的应用程序的所有请求和身份验证信息的请求中不执行任何操作。

    在这种情况下，身份验证 / 授权功能处于关闭状态。 身份验证和授权的任务是完全由应用程序代码。

以前的行为被受在 Azure 门户的**请求没有通过身份验证时要采取的操作**选项。 如果您选择**记录中的*提供程序名称* **，需要进行身份验证的所有请求。**允许请求 （无操作） * * 交由授权决策的代码，但它仍能提供身份验证信息。 如果您想要您处理所有的代码，则可以禁用身份验证 / 授权功能。

## <a name="working-with-user-identities-in-your-application"></a>使用您的应用程序中的用户标识

应用程序服务某些用户将信息传递到您的应用程序通过使用特殊标头。 外部请求禁止这些标头并将只存在如果设置由应用程序服务的身份验证 / 授权。 某些示例标头包括︰

* X-MS-客户端的主要用户名称
* X-MS-客户端-主体的 ID
* X-MS-TOKEN-FACEBOOK-ACCESS-TOKEN
* X-MS-TOKEN-FACEBOOK-EXPIRES-ON

以任何语言或框架编写的代码可以从这些标头来获取它所需要的信息。 为 ASP.NET 4.6 应用程序， **ClaimsPrincipal**自动设置适当的值。

您的应用程序也可以在获得其他用户的详细信息，通过 HTTP GET`/.auth/me`应用程序的终结点。 包含与该请求是有效的标记将返回 JSON 负载有关正在使用的提供程序的详细信息、 基础提供程序标记，与其他一些用户信息。 移动应用程序服务器的 Sdk 提供帮助器方法来处理该数据。 有关详细信息，请参阅[如何使用 Azure 移动应用程序 Node.js SDK](../app-service-mobile/app-service-mobile-node-backend-how-to-use-server-sdk.md#howto-tables-getidentity)和[与.NET 后端服务器 SDK Azure 移动应用程序的工作](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#user-info)。

## <a name="documentation-and-additional-resources"></a>文档和其他资源

### <a name="identity-providers"></a>身份提供程序
下面的教程演示了如何配置应用程序服务以使用不同的身份验证提供程序︰

- [如何配置您的应用程序使用 Azure Active Directory 登录][AAD]
- [如何配置您的应用程序使用 Facebook 登录][Facebook]
- [如何配置您的应用程序使用 Google 登录][Google]
- [如何配置您的应用程序使用 Microsoft 帐户登录][MSA]
- [如何配置您的应用程序使用 Twitter 的登录][Twitter]

如果您想要在这里使用之外的身份识别系统，您还可以使用[预览移动应用程序的.NET 服务器 SDK 中的自定义身份验证支持][custom-auth]，其可在 web 应用程序、 移动应用程序或 API 的应用程序。

### <a name="web-applications"></a>Web 应用程序
下面的教程演示了如何添加到 web 应用程序的身份验证︰

- [开始使用 Azure 应用程序服务-第 2 部分][web-getstarted]

### <a name="mobile-applications"></a>移动应用程序
下面的教程演示了如何使用服务器定向流添加到移动客户端身份验证︰

- [添加到您的 iOS 应用程序的身份验证][iOS]
- [添加到您的 Android 应用程序的身份验证][Android]
- [添加到您的 Windows 应用程序的身份验证][窗口]
- [添加到您的 Xamarin.iOS 应用程序的身份验证][Xamarin.iOS]
- [添加到您的 Xamarin.Android 应用程序的身份验证][Xamarin.Android]
- [添加到您的 Xamarin.Forms 应用程序的身份验证][Xamarin.Forms]
- [添加于 Cordova 应用程序的身份验证][Cordova]

如果您想要将客户端定向流 Azure 活动目录，请使用下列资源︰

- [对于 iOS 使用活动目录身份验证库][ADAL-iOS]
- [对 Android 使用 Active Directory 验证库][ADAL-Android]
- [对于 Windows 和 Xamarin 使用 Active Directory 验证库][ADAL-dotnet]

如果您想要对 Facebook 使用客户端定向流，请使用下列资源︰

- [对于 iOS 使用 Facebook SDK](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#facebook-sdk)

如果您想使用 Twitter 客户端定向流，请使用下列资源︰

- [使用 iOS 的使用 Twitter，结构](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#twitter-fabric)

如果您想要对 Google 使用客户端定向流，请使用下列资源︰

- [对于 iOS 使用 Google 登录 SDK](../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#google-sdk)

### <a name="api-applications"></a>API 的应用程序
下面的教程演示了如何保护您的 API 应用程序︰

- [API 在 Azure 应用程序服务的应用程序的用户身份验证][apia-user]
- [API 在 Azure 应用程序服务的应用程序的服务主体身份验证][apia-service]









[apia-user]: ../app-service-api/app-service-api-dotnet-user-principal-auth.md
[apia-service]: ../app-service-api/app-service-api-dotnet-service-principal-auth.md

[web-getstarted]: ../app-service-web/app-service-web-get-started-2.md#authenticate-your-users

[iOS]: ../app-service-mobile/app-service-mobile-ios-get-started-users.md
[Android]: ../app-service-mobile/app-service-mobile-android-get-started-users.md
[Xamarin.iOS]: ../app-service-mobile/app-service-mobile-xamarin-ios-get-started-users.md
[Xamarin.Android]: ../app-service-mobile/app-service-mobile-xamarin-android-get-started-users.md
[Xamarin.Forms]: ../app-service-mobile/app-service-mobile-xamarin-forms-get-started-users.md
[窗口]: ../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-users.md
[Cordova]: ../app-service-mobile/app-service-mobile-cordova-get-started-users.md

[AAD]: ../app-service-mobile/app-service-mobile-how-to-configure-active-directory-authentication.md
[Facebook]: ../app-service-mobile/app-service-mobile-how-to-configure-facebook-authentication.md
[Google]: ../app-service-mobile/app-service-mobile-how-to-configure-google-authentication.md
[MSA]: ../app-service-mobile/app-service-mobile-how-to-configure-microsoft-authentication.md
[Twitter]: ../app-service-mobile/app-service-mobile-how-to-configure-twitter-authentication.md

[custom-auth]: ../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth

[ADAL-Android]: ../app-service-mobile/app-service-mobile-android-how-to-use-client-library.md#adal
[ADAL-iOS]: ../app-service-mobile/app-service-mobile-ios-how-to-use-client-library.md#adal
[ADAL-dotnet]: ../app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library.md#adal
