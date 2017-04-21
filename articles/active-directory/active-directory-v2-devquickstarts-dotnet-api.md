<properties
    pageTitle="Azure AD v2.0.NET Web API |Microsoft Azure"
    description="如何构建接受令牌从两个人的 Microsoft.NET MVC Web Api 和工作或学校的科目。"
    services="active-directory"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/10/2016"
    ms.author="dastrock"/>

# <a name="secure-an-mvc-web-api"></a>安全的 MVC web API

Azure Active Directory v2.0 终结点，可以防止使用[OAuth 2.0](active-directory-v2-protocols.md#oauth2-authorization-code-flow)的访问令牌，Web API 允许用户使用个人 Microsoft 帐户和工作或者学校帐户添加到安全地访问您的 Web API。

> [AZURE.NOTE]
    V2.0 终结点支持并不是所有的 Azure Active Directory 方案和功能。  要确定是否应使用 v2.0 终结点，阅读有关[v2.0 限制](active-directory-v2-limitations.md)。

在 ASP.NET web Api，您可以完成此操作使用 Microsoft 的.NET Framework 4.5 中包含的 OWIN 中间件。  这里我们将使用 OWIN 来构建一个"待办事项列表"MVC Web API，允许客户端创建和读取用户的待办事项列表任务。  Web API 将验证传入的请求包含一个有效的访问令牌，并拒绝任何受保护的路径中没有通过验证的请求。  使用 Visual Studio 2015年生成此示例。

## <a name="download"></a>下载
本教程中的代码[在 GitHub 上](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet)维护。  要继续下去，您可以[下载应用程序的主干作为.zip](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip)或克隆主干︰

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

主干应用程序包括所有的样板代码的一个简单的 API，但是缺少的所有标识相关部分。 如果您不希望要继续下去，而是可以克隆或[下载完整的示例](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/skeleton.zip)。

```
git clone https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git
```

## <a name="register-an-app"></a>注册应用程序
在[apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)，创建新的应用程序，或按照以下[详细的步骤](active-directory-v2-app-registration.md)。  请确保︰

- 复制下分配给您的应用程序的**应用程序 Id** ，您需要很快。

此 visual studio 解决方案还包含"TodoListClient"，这是一个简单的 WPF 应用程序。  TodoListClient 用于演示如何用户迹象和客户端如何与 Web API 可以发出请求。  在这种情况下，由同一个应用程序表示 TodoListClient 和 TodoListService。  若要配置 TodoListClient，则还应︰

- 添加您的应用程序的**移动**平台。


## <a name="install-owin"></a>安装 OWIN

现在，您已注册的应用程序，您需要将您的应用程序设置为使用 2.0 版的终结点进行通信以验证传入请求和标记。

- 要开始，请打开的解决方案，并将 OWIN 中间件 NuGet 程序包添加到使用程序包管理器控制台 TodoListService 项目。

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
```

## <a name="configure-oauth-authentication"></a>配置 OAuth 身份验证

- 将 OWIN 启动类添加到 TodoListService 项目称为`Startup.cs`。  右在该项目上的单击-->**添加** --> **新项**--> 搜索"OWIN"。  OWIN 中间件将调用`Configuration(…)`在您的应用程序启动时的方法。
- 更改的类声明为`public partial class Startup`-已实施了此类的一部分为您在另一个文件中。  在`Configuration(…)`方法，生成一个 ConfgureAuth(...) 调用以设置您的 web 应用程序的身份验证。

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

- 打开的文件`App_Start\Startup.Auth.cs`并实施`ConfigureAuth(…)`方法中，将设置 Web API 用来接受从 v2.0 终结点的标记。

```C#
public void ConfigureAuth(IAppBuilder app)
{
        var tvps = new TokenValidationParameters
        {
                // In this app, the TodoListClient and TodoListService
                // are represented using the same Application Id - we use
                // the Application Id to represent the audience, or the
                // intended recipient of tokens.

                ValidAudience = clientId,

                // In a real applicaiton, you might use issuer validation to
                // verify that the user's organization (if applicable) has
                // signed up for the app.  Here, we'll just turn it off.

                ValidateIssuer = false,
        };

        // Set up the OWIN pipeline to use OAuth 2.0 Bearer authentication.
        // The options provided here tell the middleware about the type of tokens
        // that will be recieved, which are JWTs for the v2.0 endpoint.

        // NOTE: The usual WindowsAzureActiveDirectoryBearerAuthenticaitonMiddleware uses a
        // metadata endpoint which is not supported by the v2.0 endpoint.  Instead, this
        // OpenIdConenctCachingSecurityTokenProvider can be used to fetch & use the OpenIdConnect
        // metadata document.

        app.UseOAuthBearerAuthentication(new OAuthBearerAuthenticationOptions
        {
                AccessTokenFormat = new Microsoft.Owin.Security.Jwt.JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider("https://login.microsoftonline.com/common/v2.0/.well-known/openid-configuration")),
        });
}
```

- 现在，您可以使用`[Authorize]`特性来保护您的控制器和操作使用 OAuth 2.0 的持有者身份验证。  修饰`Controllers\TodoListController.cs`与授权标记的类。  这将强制用户登录才能访问该页面。

```C#
[Authorize]
public class TodoListController : ApiController
{
```

- 当授权的调用方成功调用之一`TodoListController`Api，此操作可能需要访问有关调用方的信息。  OWIN 提供了通过持有者标记声明访问`ClaimsPrincpal`对象。  

```C#
public IEnumerable<TodoItem> Get()
{
    // You can use the ClaimsPrincipal to access information about the
    // user making the call.  In this case, we use the 'sub' or
    // NameIdentifier claim to serve as a key for the tasks in the data store.

    Claim subject = ClaimsPrincipal.Current.FindFirst(ClaimTypes.NameIdentifier);

    return from todo in todoBag
           where todo.Owner == subject.Value
           select todo;
}
```

-   最后，打开`web.config`TodoListService 项目的根目录中的文件，并输入在您配置的值`<appSettings>`部分。
  - 您`ida:Audience`是在门户中输入该应用程序的**应用程序 Id** 。

## <a name="configure-the-client-app"></a>配置客户端应用程序
您可以查看操作中的 Todo 列表服务之前，您需要配置 Todo 列表中客户端，以便它可以从 v2.0 端点获取令牌，并且使对服务的调用。

- 在 TodoListClient 项目中，打开`App.config`，然后输入您的配置值，在`<appSettings>`部分。
  - 您`ida:ClientId`来自门户的应用程序 Id。

最后，清洁、 构建和运行的每个项目 ！  现在，您可以接受来自两个人的 Microsoft 帐户标记和工作或学校科目.NET MVC Web API。  登录到 TodoListClient，并调用 web api 来将任务添加到用户的待办事项列表。

为了便于参考，[作为.zip 此处提供](https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet/archive/complete.zip)（无需您配置的值） 的完整的示例，或者您可以克隆它从 GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-WebAPI-DotNet.git```

## <a name="next-steps"></a>下一步行动
您现在可以移动到其他主题。  您可能想要尝试︰

[从 Web 应用程序调用 Web API >>](active-directory-v2-devquickstarts-webapp-webapi-dotnet.md)

更多的资源，请查阅︰
- [2.0 版的开发人员指南 》 >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow"azure 活动目录"标签 >>](http://stackoverflow.com/questions/tagged/azure-active-directory)

## <a name="get-security-updates-for-our-products"></a>我们的产品以获得安全更新

我们鼓励您能够访问[此页](https://technet.microsoft.com/security/dd252948)并订阅安全通报警告出现安全事件时的通知。
