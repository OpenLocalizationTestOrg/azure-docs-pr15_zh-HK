<properties
    pageTitle="在 Android 手机应用程序上添加身份验证 |Azure 应用程序服务"
    description="了解如何在 Azure 应用程序服务使用移动应用程序，Android 通过多种身份提供程序，包括 Google、 Facebook、 Twitter，以及 Microsoft 应用程序的用户进行身份验证。"
    services="app-service\mobile"
    documentationCenter="android"
    authors="ysxu"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-android"
    ms.devlang="java"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="yuaxu"/>

# <a name="add-authentication-to-your-android-app"></a>添加于 Android 应用程序的身份验证

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="summary"></a>摘要

在本教程中，您添加身份验证到任务列表快速入门项目在 Android 使用支持的标识提供程序。 本教程基于[移动应用程序入门]教程，您必须首先完成。

##<a name="register"></a>注册您的应用程序进行身份验证和配置应用程序服务

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>限制对已验证身份的用户的权限

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

+ 在 Android Studio 中，[移动应用程序入门]教程打开项目计划已执行完毕。 从**运行**菜单上单击**运行应用程序**并验证应用程序启动后引发未处理的异常状态代码 401 （未经授权）。

     发生此异常的原因在于，应用程序尝试访问后端作为未经身份验证的用户，但_TodoItem_表现在要求进行身份验证。

接下来，您可以更新应用程序请求的移动应用程序的后端资源之前验证用户的身份。

## <a name="add-authentication-to-the-app"></a>向应用程序添加验证

[AZURE.INCLUDE [mobile-android-authenticate-app](../../includes/mobile-android-authenticate-app.md)]

## <a name="cache-tokens"></a>缓存在客户端的身份验证令牌

[AZURE.INCLUDE [mobile-android-authenticate-app-with-token](../../includes/mobile-android-authenticate-app-with-token.md)]

##<a name="next-steps"></a>下一步行动

现在，您已完成本教程中基本身份验证，请考虑继续下面的教程之一︰

+ [添加推式通知于 Android 应用程序](app-service-mobile-android-get-started-push.md)了解如何配置您的移动应用程序后端使用 Azure 通知集线器发送推式通知。

+ [启用的 Android 应用程序的脱机同步](app-service-mobile-android-get-started-offline-data.md)了解如何添加您使用移动应用程序端的应用程序的脱机支持。 脱机同步允许最终用户与移动应用程序交互&mdash;查看、 添加或修改数据&mdash;即使没有网络连接。



<!-- Anchors. -->
[Register your app for authentication and configure Mobile Services]: #register
[Restrict table permissions to authenticated users]: #permissions
[Add authentication to the app]: #add-authentication
[Store authentication tokens on the client]: #cache-tokens
[Refresh expired tokens]: #refresh-tokens
[Next Steps]:#next-steps


<!-- URLs. -->
[开始使用移动应用程序]: app-service-mobile-android-get-started.md
