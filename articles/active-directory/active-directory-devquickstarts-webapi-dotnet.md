<properties
    pageTitle="Azure AD.NET 快速入门 |Microsoft Azure"
    description="如何构建一个集成了 Azure AD 身份验证和授权的.NET MVC Web API。"
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
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="protect-a-web-api-using-bearer-tokens-from-azure-ad"></a>保护 Web API 使用从 Azure 广告载体标记

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

如果您要构建的应用程序提供了对您将需要知道如何来保护这些资源不受毫无根据访问受保护资源的访问。
Azure 广告更加简单且直接来保护 web API 仅几行代码中使用 OAuth 载体 2.0 的访问令牌。

在 Asp.NET web 应用程序，您可以实现此目的使用 Microsoft 的.NET Framework 4.5 中包括社区驱动 OWIN 中间件实现。  这里我们将使用到的 OWIN，构建一个"待办事项列表"web API:
-   指定受保护的 API。
-   验证 Web API 调用包含有效的访问令牌。

为此，您需要︰

1. 向应用程序注册 Azure 的广告
2. 将您的应用程序设置为使用 OWIN 身份验证管道。
3. 配置客户端应用程序调用到执行列表 Web API

若要开始，[下载应用程序骨架](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/skeleton.zip)或[下载已完成的示例](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip)。  每个是一个 Visual Studio 2013年的解决方案。  也需要在其中应用程序注册 Azure AD 租户。  如果您还没有，[学习如何获得一个](active-directory-howto-tenant.md)。


## <a name="1--register-an-application-with-azure-ad"></a>*1.使用 Azure AD 注册应用程序*
若要保护您的应用程序，您首先需要在您的组织中创建应用程序和 Azure AD 提供一些关键信息。

-   登录到[Azure 的管理门户](https://manage.windowsazure.com)
-   在左侧导航中，单击**活动目录**
-   选择要在其中注册该应用程序一个租户。
-   单击**应用程序**选项卡，并单击**添加**在底部抽屉里。
-   按照提示进行操作并创建新**的 Web 应用程序和/或 WebAPI**。
    -   应用程序**名称**将介绍您给最终用户的应用程序。  输入"待办事项列表服务"。
    -   **重定向 Uri**是 Azure 广告将返回您的应用程序请求的任何标记的方案和字符串组合。 输入`https://localhost:44321/`为此值。
-   完成注册后，导航到**配置**选项卡，然后找到**应用程序 ID URI**字段。  如为此值时，输入特定于租户的标识符`https://contoso.onmicrosoft.com/TodoListService`
- 保存配置。  保持打开门户-您需要很快注册您的客户端应用程序。

## <a name="2-set-up-your-app-to-use-the-owin-authentication-pipeline"></a>*2.设置了您的应用程序使用 OWIN 身份验证管道*

现在，您已经注册 Azure 广告应用程序，您需要设置应用程序以验证传入请求和标记的 Azure 的广告进行通信。

-   要开始，请打开的解决方案，并将 OWIN 中间件 NuGet 程序包添加到使用程序包管理器控制台 TodoListService 项目。

```
PM> Install-Package Microsoft.Owin.Security.ActiveDirectory -ProjectName TodoListService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TodoListService
```

-   将 OWIN 启动类添加到 TodoListService 项目称为`Startup.cs`。  右在该项目上的单击-->**添加** --> **新项**--> 搜索"OWIN"。  OWIN 中间件将调用`Configuration(…)`在您的应用程序启动时的方法。
-   更改的类声明为`public partial class Startup`-已实施了此类的一部分为您在另一个文件中。  在`Configuration(…)`方法，生成一个 ConfgureAuth(...) 调用以设置您的 web 应用程序的身份验证。

```C#
public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

-   打开的文件`App_Start\Startup.Auth.cs`并实施`ConfigureAuth(…)`方法。  您在中提供的参数`WindowsAzureActiveDirectoryBearerAuthenticationOptions`将作为您的应用程序坐标与 Azure 广告进行通信。

```C#
public void ConfigureAuth(IAppBuilder app)
{
    app.UseWindowsAzureActiveDirectoryBearerAuthentication(
        new WindowsAzureActiveDirectoryBearerAuthenticationOptions
        {
            Audience = ConfigurationManager.AppSettings["ida:Audience"],
            Tenant = ConfigurationManager.AppSettings["ida:Tenant"]
        });
}
```

-   现在，您可以使用`[Authorize]`特性来保护您的控制器和操作使用 JWT 持有者身份验证。  修饰`Controllers\TodoListController.cs`与授权标记的类。  这将强制用户登录才能访问该页面。

```C#
[Authorize]
public class TodoListController : ApiController
{
```

- 当授权的调用方成功调用之一`TodoListController`Api，此操作可能需要访问有关调用方的信息。  OWIN 提供了通过持有者标记声明访问`ClaimsPrincpal`对象。  
- Web Api 的一个常见要求是验证"作用域"令牌中存在-这可以确保最终用户已同意访问 Todo 列表服务所需的权限︰

```C#
public IEnumerable<TodoItem> Get()
{
    // user_impersonation is the default permission exposed by applications in AAD
    if (ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/scope").Value != "user_impersonation")
    {
        throw new HttpResponseException(new HttpResponseMessage {
          StatusCode = HttpStatusCode.Unauthorized,
          ReasonPhrase = "The Scope claim does not contain 'user_impersonation' or scope claim not found"
        });
    }
    ...
}
```

-   最后，打开`web.config`TodoListService 项目的根目录中的文件，并输入在您配置的值`<appSettings>`部分。
  - `ida:Tenant`是您 Azure AD 租户，例如"contoso.onmicrosoft.com"的名称。
  - 您`ida:Audience`是 Azure 门户中输入该应用程序的应用程序 ID URI。

## <a name="3--configure-a-client-application--run-the-service"></a>*3.配置客户端应用程序与运行服务*
您可以查看操作中的 Todo 列表服务之前，您需要配置 Todo 列表中客户端，以便它可以从 AAD 获取令牌，并对该服务进行调用。

- 向后定位到[Azure 管理门户](https://manage.windowsazure.com)
- 在您的 Azure AD 租户，创建新的应用程序和得到的提示中选择**本机客户端应用程序**。
    -   应用程序**名称**将介绍您给最终用户的应用程序
    -   输入`http://TodoListClient/`**重定向 Uri**值。
- 完成注册后，AAD 会将您的应用程序分配一个唯一的**客户机 Id**。 您将需要此值在下一个步骤中，所以将其复制从配置选项卡。
- 此外在**配置**选项卡上，查找"权限与其他应用程序"部分。 单击"添加应用程序"。 在"显示"下拉列表中，选择"全部应用"，然后单击上面的复选标记。 找到与您到执行列表中的服务，请单击然后单击底部复选标记以将应用程序添加。 从"授予权限"下拉列表中，选择"访问对执行列表服务"并保存配置。


- 在 Visual Studio，打开`App.config`在 TodoListClient 项目，输入在您配置的值`<appSettings>`部分。
  - `ida:Tenant`是您 Azure AD 租户，例如"contoso.onmicrosoft.com"的名称。
  - 您`ida:ClientId`Azure 门户网站从复制的应用程序 ID。
  - 您`todo:TodoListResourceId`是在 Azure 门户输入执行列表服务应用程序的应用程序 ID URI。

最后，清洁、 构建和运行的每个项目 ！  如果还没有的话，现在就应该在您组织中创建一个新用户 *。 onmicrosoft.com 域。  登录到该用户，待办事项列表的客户端，并将一些任务添加到任务列表的用户。

为了便于参考，提供完整的示例 （无需您的配置值）[此处](https://github.com/AzureADQuickStarts/WebAPI-Bearer-DotNet/archive/complete.zip)。  您可以立即将移动到更多的其他标识方案。

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
