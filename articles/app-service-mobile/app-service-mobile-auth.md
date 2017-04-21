<properties
    pageTitle="身份验证和授权在 Azure 的移动应用程序 |Microsoft Azure"
    description="概念的引用，并概述了身份验证 / 授权 Azure 移动应用程序的功能"
    services="app-service\mobile"
    documentationCenter=""
    authors="mattchenderson"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="mahender"/>

# <a name="authentication-and-authorization-in-azure-mobile-apps"></a>身份验证和授权在 Azure 的移动应用程序

## <a name="what-is-app-service-authentication--authorization"></a>什么是应用程序服务的身份验证 / 授权？

> [AZURE.NOTE] 本主题将被迁移到统一[应用程序服务的身份验证 / 授权](../app-service/app-service-authentication-overview.md)主题，包括 Web、 移动设备、 和 API 的应用程序。

应用程序服务的身份验证 / 授权是一种功能，允许您的应用程序登录用户具有无需应用程序的后端的代码更改。 它提供了一种简便方法保护您的应用程序以及与每个用户的数据。

应用程序服务使用联合的身份，第三方**身份提供程序**("IDP") 存储帐户，并对用户进行身份验证，而不是它自己的应用程序使用此标识。 应用程序服务支持五个身份提供开箱即用︰ _Azure Active Directory_、 _Facebook_、 _Google_、 _Microsoft 帐户_，并_使用 Twitter_。 您可以将集成另一个身份标识提供程序或自定义标识解决方案为您的应用程序展开这种支持。

您的应用程序可以利用任意数量的这些标识提供程序，这样可以将您的最终用户提供如何登录的选项。

如果您希望立即开始工作，请参阅下面的教程之一︰

- [添加到您的 iOS 应用程序的身份验证]
- [添加到您的 Xamarin.iOS 应用程序的身份验证]
- [添加到您的 Xamarin.Android 应用程序的身份验证]
- [添加到您的 Windows 应用程序的身份验证]

## <a name="how-authentication-works"></a>身份验证的工作原理

要使用一个标识提供程序进行身份验证，首先需要配置标识提供程序以了解您的应用程序。 标识提供程序然后为您提供 Id 并返回给应用程序提供了您的机密信息。 这完成信任关系，并使应用程序服务来验证提供给它的身份。

下面的主题中详细描述了这些步骤︰

- [如何配置您的应用程序使用 Azure Active Directory 登录]
- [如何配置您的应用程序使用 Facebook 登录]
- [如何配置您的应用程序使用 Google 登录]
- [如何配置您的应用程序使用 Microsoft 帐户登录]
- [如何配置您的应用程序使用 Twitter 的登录]

一旦所有后端配置，您可以修改您的客户端进行登录。 有以下两种方法︰

- 使用一行代码，可让客户端 SDK 用户提供登录移动应用程序。
- 利用已建立身份并再访问应用程序服务的某个给定的标识提供者发布的 SDK。

>[AZURE.TIP] 大多数应用程序应使用 SDK 的提供程序，以获取更多本机感觉登录体验并利用刷新支持和其他特定于提供程序的好处。

### <a name="how-authentication-without-a-provider-sdk-works"></a>不提供 SDK 带身份验证的工作原理

如果不想设置提供程序 SDK，您可以允许移动应用程序为您进行登录。 移动应用程序客户端 SDK 将打开一个 web 视图到您选择的提供程序，并完成登录。 偶尔在博客和论坛，您将看到这称为"服务器流程"或"服务器定向流"以来服务器管理登录，而客户端 SDK 永远收不到的提供程序标记。

为每个平台验证本教程中介绍了启动此流程所需的代码。 在流的末尾，客户端 SDK 具有应用程序服务的标记，该标记会自动附加到后端的所有请求。

### <a name="how-authentication-with-a-provider-sdk-works"></a>使用 SDK 的提供程序进行身份验证的工作原理

提供程序使用 SDK 允许登录体验与该应用程序正在其运行的平台操作系统更紧密交互。 这也使您提供程序标记以及某些用户信息在客户端，这使它更容易使用 graph Api 和自定义的用户体验。 偶尔在博客和论坛上，您将看到这所谓的"客户端流程"或"客户端定向流"因为代码在客户端上的处理登录，和客户端代码可以访问提供程序标记。

一旦获得令牌提供程序，则需要验证发送到应用程序服务。 在流的末尾，客户端 SDK 具有应用程序服务的标记，该标记会自动附加到后端的所有请求。 如果它们这样选择，开发人员还可以保留提供程序标记的引用。

## <a name="how-authorization-works"></a>授权的工作方式

应用程序服务的身份验证 / 授权公开**请求没有通过身份验证时要采取**的操作的几种选择。 代码接收给定的请求前，可以请求进行身份验证的应用程序服务检查，如果不，则将其拒绝并且试图让用户登录然后再试。

一种选择是有未经身份验证的请求重定向到一个身份提供程序。 在 web 浏览器，这实际上会花费到新页面的用户。 但是，您的移动客户端无法通过这种方式，重定向和未经身份验证的响应将会收到一个 HTTP _401 未经授权_响应。 鉴于这种情况，您的客户端发出的第一个请求应始终是到登录的终结点，，然后您可以对任何其他 Api 的调用。 如果您尝试登录之前调用另一个 API，您的客户端将收到错误。

如果希望更精确地控制哪些终结点通过要求进行身份验证，您还可以选择"无操作 （允许请求）"未经身份验证的请求。 在这种情况下，所有身份验证决定被推迟到您的应用程序代码。 您还可以允许访问特定用户自定义的授权规则。

## <a name="documentation"></a>文档

下面的教程演示了如何添加到您使用的应用程序服务的移动客户端身份验证︰

- [添加到您的 iOS 应用程序的身份验证]
- [添加到您的 Xamarin.iOS 应用程序的身份验证]
- [添加到您的 Xamarin.Android 应用程序的身份验证]
- [添加到您的 Windows 应用程序的身份验证]

下面的教程演示了如何配置应用程序服务以利用不同的身份验证提供程序︰

- [如何配置您的应用程序使用 Azure Active Directory 登录]
- [如何配置您的应用程序使用 Facebook 登录]
- [如何配置您的应用程序使用 Google 登录]
- [如何配置您的应用程序使用 Microsoft 帐户登录]
- [如何配置您的应用程序使用 Twitter 的登录]

如果您想要在这里使用之外的身份识别系统，您还可以利用[预览中.NET server SDK 的自定义身份验证支持](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#custom-auth)。

[添加到您的 iOS 应用程序的身份验证]: app-service-mobile-ios-get-started-users.md
[添加到您的 Xamarin.iOS 应用程序的身份验证]: app-service-mobile-xamarin-ios-get-started-users.md
[添加到您的 Xamarin.Android 应用程序的身份验证]: app-service-mobile-xamarin-android-get-started-users.md
[添加到您的 Windows 应用程序的身份验证]: app-service-mobile-windows-store-dotnet-get-started-users.md

[如何配置您的应用程序使用 Azure Active Directory 登录]: app-service-mobile-how-to-configure-active-directory-authentication.md
[如何配置您的应用程序使用 Facebook 登录]: app-service-mobile-how-to-configure-facebook-authentication.md
[如何配置您的应用程序使用 Google 登录]: app-service-mobile-how-to-configure-google-authentication.md
[如何配置您的应用程序使用 Microsoft 帐户登录]: app-service-mobile-how-to-configure-microsoft-authentication.md
[如何配置您的应用程序使用 Twitter 的登录]: app-service-mobile-how-to-configure-twitter-authentication.md
