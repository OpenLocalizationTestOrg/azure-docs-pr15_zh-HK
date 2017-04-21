<properties
   pageTitle="Azure 的 Active Directory 验证库 |Microsoft Azure"
   description="Azure AD 身份验证库 (ADAL) 允许客户端应用程序开发人员可以很容易地验证用户以云或内部活动目录 (AD)，然后获取用于保护 API 调用访问令牌。"
   services="active-directory"
   documentationCenter=""
   authors="bryanla"
   manager="mbaldwin"
   editor="mbaldwin" />
<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="10/11/2016"
   ms.author="mbaldwin" />

# <a name="azure-active-directory-authentication-libraries"></a>Azure 的 Active Directory 验证库

Azure AD 身份验证库 (ADAL) 使客户端应用程序开发人员能够轻松地验证用户以云或内部活动目录 (AD)，然后获取用于保护 API 调用访问令牌。 ADAL 具有许多功能使身份验证容易对于开发人员来说，如异步支持、 可配置的令牌缓存存储访问令牌和刷新令牌，自动标记刷新时访问令牌过期并且刷新令牌可用，请以及更多。 通过处理最复杂的 ADAL 可以帮助开发人员专注于在其应用程序的业务逻辑，并很容易地获得资源，没有安全方面的专家。

ADAL 是在各种平台上可用。

|平台|包名称|客户端/服务器|下载|源代码|文档和示例|
|---|---|---|---|---|---|
|.NET 客户端，Windows 应用商店，UWP，Xamarin iOS 和 Android|用于.NET v3 的活动目录身份验证库 (ADAL) |客户端|[Microsoft.IdentityModel.Clients.ActiveDirectory (NuGet)](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory)|[为使.NET (Github) ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet)|[文档](https://docs.microsoft.com/active-directory/adal/microsoft.identitymodel.clients.activedirectory)|
|.NET 客户端，Windows 应用商店，Windows Phone 8.1 |用于.NET v2 的 active Directory 验证库 (ADAL) |客户端|[Microsoft.IdentityModel.Clients.ActiveDirectory (NuGet)](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory/2.28.2)|[为使.NET (Github) ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-dotnet/releases/tag/v2.28.2)|[文档](https://docs.microsoft.com/active-directory/adal/v2/microsoft.identitymodel.clients.activedirectory)|
|JavaScript|JavaScript 的 active Directory 验证库 (ADAL)|客户端|[ADAL 的 JavaScript (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-js)|[ADAL 的 JavaScript (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-js)|示例︰ [SinglePageApp 最低 (Github)](https://github.com/AzureADSamples/SinglePageApp-DotNet)|
|OS X iOS|对于目标 C 的 active Directory 验证库 (ADAL)|客户端|[对于目标 C (CocoaPods) ADAL](http://cocoadocs.org/docsets/ADAL/)|[对于目标 C (Github) ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-objc)|示例︰ [NativeClient Io (Github)](https://github.com/AzureADSamples/NativeClient-iOS)|
|Android|Android 的 active Directory 验证库 (ADAL)|客户端|[ADAL 为 Android （中央存储库）](http://search.maven.org/remotecontent?filepath=com/microsoft/aad/adal/)|[对于 Android (Github) ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-android)|示例︰ [NativeClient Android (Github)](https://github.com/AzureADSamples/NativeClient-Android)|
|Node.js|对于 Node.js active Directory 验证库 (ADAL)|客户端|[ADAL 的 Node.js (npm)](https://www.npmjs.com/package/adal-node)|[ADAL 的 Node.js (Github)](https://github.com/AzureAD/azure-activedirectory-library-for-nodejs)|示例︰ [WebAPI Nodejs (Github)](https://github.com/AzureADSamples/WebAPI-Nodejs)|
|Node.js|Azure 活动目录 Passport 身份验证节点的中间件|客户端|[Azure 的 Node.js (npm) 的活动目录护照](https://www.npmjs.com/package/passport-azure-ad)|[Azure Active Directory 的 Node.js (Github)](https://github.com/AzureAD/passport-azure-ad)||
|Java|Java 的 active Directory 验证库 (ADAL)|客户端|[对于 Java (Github) ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-java)|[对于 Java (Github) ADAL](https://github.com/AzureAD/azure-activedirectory-library-for-java)||
|.NET|对于 Microsoft.NET Framework 4.5 标识协议扩展|服务器|[Microsoft.IdentityModel.Protocol.Extensions (NuGet)](https://www.nuget.org/packages/Microsoft.IdentityModel.Protocol.Extensions)|[Azure 广告标识模型扩展为.NET (Github) 的](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet)||
|.NET|Microsoft.Net Framework 4.5 JSON Web 标记处理程序|服务器|[System.IdentityModel.Tokens.Jwt (NuGet)](https://www.nuget.org/packages/System.IdentityModel.Tokens.Jwt)|[Azure 广告标识模型扩展为.NET (Github) 的](https://github.com/AzureAD/azure-activedirectory-identitymodel-extensions-for-dotnet)||
|.NET|OWIN 中间件使应用程序能够使用 Microsoft 的技术进行身份验证。|服务器|[Microsoft.Owin.Security.ActiveDirectory (NuGet)](https://www.nuget.org/packages/Microsoft.Owin.Security.ActiveDirectory/)|[OWIN (CodePlex)](http://katanaproject.codeplex.com)||
|.NET|OWIN 中间件使应用程序能够使用 OpenIDConnect 进行身份验证。|服务器|[Microsoft.Owin.Security.OpenIdConnect (NuGet)](https://www.nuget.org/packages/Microsoft.Owin.Security.OpenIdConnect)|[OWIN (CodePlex)](http://katanaproject.codeplex.com)|示例︰ [WebApp OpenIDConnecty 最低 (Github)](https://github.com/AzureADSamples/WebApp-OpenIDConnect-DotNet)|
|.NET|OWIN 中间件使应用程序能够使用 WS 联合身份验证进行身份验证。|服务器|[Microsoft.Owin.Security.WsFederation (NuGet)](https://www.nuget.org/packages/Microsoft.Owin.Security.WsFederation)|[OWIN (CodePlex)](http://katanaproject.codeplex.com)|示例︰ [WebApp WSFederation 最低 (Github)](https://github.com/AzureADSamples/WebApp-WSFederation-DotNet)|

## <a name="scenarios"></a>方案

以下是三种常见情况，ADAL 可用于进行身份验证。  

### <a name="authenticating-users-of-a-client-application-to-a-remote-resource"></a>客户端应用程序对远程资源的用户进行身份验证

在此方案中，开发人员都有一个客户端，例如 WPF 应用程序，需要访问远程资源保护的 Azure 的广告，如 web API。 他已订阅了 Azure，知道如何调用下游 web API，并知道 web API 使用 Azure AD 租户。 因此，他可以使用 ADAL 来促进使用 Azure 的广告，通过完全委派身份验证体验到 ADAL 或通过显式处理用户凭据进行身份验证。 ADAL 使它容易对用户进行身份验证，获得 Azure 广告，访问令牌和刷新令牌，然后使用访问令牌进行到 web API 请求。

演示使用 Azure ad 身份验证这种情况下一个代码示例，请参见[Web api 的本机客户端 WPF 应用程序](https://github.com/azureadsamples/nativeclient-dotnet)。

### <a name="authenticating-a-server-application-to-a-remote-resource"></a>对服务器应用程序对远程资源进行身份验证

在这种情况下，开发人员都有需要访问远程资源的 Azure 的广告，如 web API 保护的服务器上运行的应用程序。 他已订阅了 Azure，知道如何调用的下游服务，并且知道 Azure AD 租户 web API 使用。 因此，他可以使用 ADAL 以便使用 Azure AD 身份验证通过显式地处理应用程序的凭据。 ADAL 使它易于使用应用程序的客户端凭据从 Azure AD 检索令牌，然后使用该标记来使到 web API 请求。 ADAL 还处理缓存它，并根据需要更新其管理访问令牌的生存期。 有关代码示例，演示了此方案，请参阅[Web API 的控制台应用程序](https://github.com/AzureADSamples/Daemon-DotNet)。

### <a name="authenticating-a-server-application-on-behalf-of-a-user-to-access-a-remote-resource"></a>对服务器应用程序代表用户访问远程资源进行身份验证

在这种情况下，开发人员都有需要访问远程资源的 Azure 的广告，如 web API 保护的服务器上运行的应用程序。 此外需要在 Azure AD 中代表用户进行请求。 他已订阅了 Azure，知道如何调用下游 web API，并且知道租户的服务使用 Azure 广告。 一旦用户经过身份验证的 web 应用程序，该应用程序可从 Azure 广告用户获得授权码。 然后，web 应用程序可以使用 ADAL 来获取访问令牌并刷新令牌代表用户使用该应用程序从 Azure 广告与关联的授权代码和客户端凭据。 一旦 web 应用程序拥有访问令牌，它可以调用 web API 令牌过期之前。 令牌到期后，web 应用程序可以使用 ADAL 新的访问令牌可以使用来获取刷新令牌之前收到的。


## <a name="see-also"></a>请参见

[Azure Active Directory 开发人员指南 》](active-directory-developers-guide.md)

[Azure Active directory 的身份验证方案](active-directory-authentication-scenarios.md)

[Azure Active Directory 代码示例](active-directory-code-samples.md)
