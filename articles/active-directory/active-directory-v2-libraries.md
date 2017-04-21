<properties
   pageTitle="Azure Active Directory v2.0 身份验证库 |Microsoft Azure"
   description="兼容的客户端库和服务器中间件库，和相关的库、 源和示例的链接，对于 Azure Active Directory v2.0 终结点。"
   services="active-directory"
   documentationCenter=""
   authors="skwan"
   manager="mbaldwin"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/30/2016"
   ms.author="skwan;bryanla"/>


# <a name="azure-active-directory-v20-authentication-libraries"></a>Azure Active Directory v2.0 身份验证库
Azure 活动目录 (AD Azure) 2.0 版终结点支持的行业标准 OAuth 2.0 和 OpenID 连接 1.0 协议。 使用 v2.0 的终结点，可以使用来自 Microsoft 和其他组织的各种库。

生成使用 v2.0 终结点的应用程序时，建议使用由按照安全开发生命周期 (SDL) 的方法，像[一个跟 Microsoft]协议域专家编写的库[Microsoft-SDL]。 如果决定手动编写代码支持协议，我们建议按照 SDL 的方法和标准规范每个协议中仔细观察了安全注意事项。

## <a name="types-of-libraries"></a>类型库

Azure AD 2.0 版适用于两种类型的库︰

- **客户端库**。 本机客户端和服务器使用客户端库来获取调用的资源，例如 Microsoft Graph 的访问令牌。
- **服务器中间件库**。 Web 应用程序将用于用户登录的服务器中间件库。 Web Api 使用服务器中间件库验证本机客户端或其他服务器发送的标记。

## <a name="library-support"></a>库的支持
使用 v2.0 终结点时，您可以选择任何符合标准的库，因为很重要，要知道从何处获得支持。 对于问题和功能请求在库代码中的，请与库的所有者。 有关问题，并在服务端协议实现的功能请求，请联系 Microsoft。

库分为两个支持类︰

- **Microsoft 支持**。 Microsoft 为这些库，提供修补程序和已完成 SDL 在这些库上付出一定的努力。
- **兼容**。 Microsoft 已基本方案中测试这些库，并确认它们使用 v2.0 终结点。 Microsoft 不提供解决这些库和有未完成的这些库检查。 问题和功能请求应交给库的开放源代码项目。

使用 v2.0 端点的库的列表，请参阅本文中的下一个部分。

## <a name="microsoft-supported-client-libraries"></a>Microsoft 支持的客户端库
| 平台| 库名称| 下载 | 源代码 | 示例 |
| :-: | :-: | :-: | :-: | :-: |
| .NET 中，Windows 应用商店，Xamarin | 为使.NET 的 Microsoft 身份验证库 (MSAL) | [Microsoft.Identity.Client (NuGet)][ClientLib-NET-Lib] | [使.NET (GitHub) 的 MSAL][ClientLib-NET-Repo] | [Windows 桌面的本机客户端示例][ClientLib-NET-Sample] |
| Node.js | Microsoft Azure Active Directory Passport.js 插件 | [Passport Azure 广告 (npm)][ClientLib-Node-Lib] | [Passport Azure 广告 (GitHub)][ClientLib-Node-Repo] | 即将推出 |

<!--- COMMENTING OUT UNTIL THEY ARE READY
| iOS, Mac | Microsoft Authentication Library (MSAL) for ObjC | In development | In development | In development |
| Android | Microsoft Authentication Library (MSAL) for Android | In development | In development | In development |
| JavaScript | Microsoft Authentication Library (MSAL) for JavaScript | In development | In development | In development |
 -->

## <a name="microsoft-supported-server-middleware-libraries"></a>Microsoft 支持服务器中间件库
| 平台| 库名称| 下载 | 源代码 | 示例 |
| :-: | :-: | :-: | :-: | :-: |
| .NET 4.x | OWIN OpenID 连接 ASP.NET 的中间件 | [Microsoft.Owin.Security.OpenIdConnect (NuGet)][ServerLib-Net4-Owin-Oidc-Lib] | [Katana 项目 (CodePlex)][ServerLib-Net4-Owin-Oidc-Repo] | [Web 应用程序示例][ServerLib-Net4-Owin-Oidc-Sample] |
| .NET 4.x | 对于 ASP.NET 的 OWIN OAuth 持有者中间件 | [Microsoft.Owin.Security.OAuth (NuGet)][ServerLib-Net4-Owin-Oauth-Lib] | [Katana 项目 (CodePlex)][ServerLib-Net4-Owin-Oauth-Repo] | [Web API 示例][ServerLib-Net4-Owin-Oauth-Sample] |
| .NET 核心 | OWIN OpenID 的.NET 核心连接中间件 | [Microsoft.AspNetCore.Authentication.OpenIdConnect (NuGet)][ServerLib-NetCore-Owin-Oidc-Lib] | [ASP.NET 安全性 (GitHub)][ServerLib-NetCore-Owin-Oidc-Repo] | [Web 应用程序示例][ServerLib-NetCore-Owin-Oidc-Sample] |
| .NET 核心 | 对于.NET 核心的 OWIN OAuth 持有者中间件 | [Microsoft.AspNetCore.Authentication.OAuth (NuGet)][ServerLib-NetCore-Owin-Oauth-Lib] | [ASP.NET 安全性 (GitHub)][ServerLib-NetCore-Owin-Oauth-Repo] | 即将推出 |
| Node.js | Microsoft Azure 活动目录 Passport.js 插件 | [Passport Azure 广告 (npm)][ServerLib-Node-Lib] | [Passport Azure 广告 (GitHub)][ServerLib-Node-Repo] | [Web 应用程序示例][ServerLib-Node-Sample] |
<!--- COMMENTING UNTIL SAMPLE IS AVAILABLE
| .NET 4.x, .NET Core | JSON Web Token Handler for .NET | [System.IdentityModel.Tokens.Jwt (NuGet)][ServerLib-Net-Jwt-Lib] | [Azure AD identity model extensions for .NET (GitHub)][ServerLib-Net-Jwt-Repo] | Coming soon |
--->
## <a name="compatible-client-libraries"></a>兼容的客户端库
| 平台| 库名称 | 经过测试的版本 | 源代码 | 示例 |
| :-: | :-: | :-: | :-: | :-: |
| Android | [OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib/wiki) | 0.2.1 | [OIDCAndroidLib](https://github.com/kalemontes/OIDCAndroidLib) | [本机应用程序示例](active-directory-v2-devquickstarts-android.md) |
| iOS | [NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) | 1.2.8 | [NXOAuth2Client](https://github.com/nxtbgthng/OAuth2Client) | [本机应用程序示例](active-directory-v2-devquickstarts-ios.md)|
| JavaScript | [Hello.js](https://adodson.com/hello.js/) | 1.13.5 | [Hello.js](https://github.com/MrSwitch/hello.js) | 即将推出 |
| Python Flask | [Flask OAuthlib](https://github.com/lepture/flask-oauthlib) | 0.9.3 | [Flask OAuthlib](https://github.com/lepture/flask-oauthlib) | 即将推出 |
| 拼音 | [OmniAuth](https://github.com/omniauth/omniauth/wiki) | omniauth:1.3.1</br>omniauth-oauth2:1.4.0 | [OmniAuth](https://github.com/omniauth/omniauth)</br>[OmniAuth OAuth2](https://github.com/intridea/omniauth-oauth2) | 即将推出 |
<!--- REMOVING BRANDON'S FOR NOW
|  |  |  |  |  |
| Android | [OAuth2 Client](https://github.com/wuman/android-oauth-client) |   | [OAuth2 Client](https://github.com/wuman/android-oauth-client)  | Coming soon  |
| Java | [WSO2 Identity Server](https://docs.wso2.com/display/IS500/Introducing+the+Identity+Server) | [Version 5.2.0](http://wso2.com/products/identity-server/) | [Source](https://docs.wso2.com/display/IS500/Building+from+Source) | [Samples index](https://docs.wso2.com/display/IS500/Samples)  |
| Java | [Java Gluu Server](https://gluu.org/docs/) |   | [oxAuth](https://github.com/GluuFederation/oxAuth)  | Coming soon |
| Node.js | [NPM passport-openidconnect](https://www.npmjs.com/package/passport-openidconnect) | 0.0.1  | [Passport-OpenID Connect](https://github.com/jaredhanson/passport-openidconnect) | Coming soon  |
| PHP | [OpenID Connect Basic Client](https://github.com/jumbojett/OpenID-Connect-PHP) |   | [OpenID Connect Basic Client](https://github.com/jumbojett/OpenID-Connect-PHP)  | Coming soon  |
-->

## <a name="compatible-server-middleware-libraries"></a>兼容的服务器中间件库
即将推出

## <a name="related-content"></a>相关的内容
Azure AD v2.0 终结点的详细信息，请参阅[AD Azure 应用程序模型 2.0 版概述][AAD-App-Model-V2-Overview]。

为了帮助我们改进和形状我们的内容，请使用 Disqus 评论功能在本文的末尾提供反馈。

<!--Image references-->

<!--Reference style links -->
[AAD-App-Model-V2-Overview]: active-directory-appmodel-v2-overview.md
[ClientLib-NET-Lib]: http://www.nuget.org/packages/Microsoft.Identity.Client
[ClientLib-NET-Repo]: https://github.com/AzureAD/microsoft-authentication-library-for-dotnet
[ClientLib-NET-Sample]: active-directory-v2-devquickstarts-wpf.md
[ClientLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ClientLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad
[ClientLib-Node-Sample]:
[ClientLib-Iosmac-Lib]:
[ClientLib-Iosmac-Repo]:
[ClientLib-Iosmac-Sample]:
[ClientLib-Android-Lib]:
[ClientLib-Android-Repo]:
[ClientLib-Android-Sample]:
[ClientLib-Js-Lib]:
[ClientLib-Js-Repo]:
[ClientLib-Js-Sample]:
[Microsoft-SDL]: http://www.microsoft.com/sdl/default.aspx
[ServerLib-Net4-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect/
[ServerLib-Net4-Owin-Oidc-Repo]: http://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oidc-Sample]: active-directory-v2-devquickstarts-dotnet-web.md
[ServerLib-Net4-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.Owin.Security.OAuth/
[ServerLib-Net4-Owin-Oauth-Repo]: http://katanaproject.codeplex.com/
[ServerLib-Net4-Owin-Oauth-Sample]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-devquickstarts-dotnet-api/
[ServerLib-Net-Jwt-Lib]: https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt
[ServerLib-Net-Jwt-Repo]: https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet
[ServerLib-Net-Jwt-Sample]:/
[ServerLib-NetCore-Owin-Oidc-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OpenIdConnect/
[ServerLib-NetCore-Owin-Oidc-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oidc-Sample]: https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect-aspnetcore-v2
[ServerLib-NetCore-Owin-Oauth-Lib]: https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.OAuth/
[ServerLib-NetCore-Owin-Oauth-Repo]: https://github.com/aspnet/Security
[ServerLib-NetCore-Owin-Oauth-Sample]:/
[ServerLib-Node-Lib]: https://www.npmjs.com/package/passport-azure-ad
[ServerLib-Node-Repo]: https://github.com/AzureAD/passport-azure-ad/
[ServerLib-Node-Sample]: https://azure.microsoft.com/en-us/documentation/articles/active-directory-v2-devquickstarts-node-web/
