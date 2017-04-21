<properties
    pageTitle="Azure AD B2C |Microsoft Azure"
    description="如何生成.NET Web API 使用 Azure 活动目录 B2C，受使用 OAuth 2.0 的访问令牌进行身份验证。"
    services="active-directory-b2c"
    documentationCenter=".net"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-active-directory-b2c-build-a-net-web-api"></a>Azure 的活动目录 B2C︰ 生成.NET web API

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

使用 Azure 活动目录 (AD Azure) B2C 可以使用 OAuth 2.0 的访问令牌来保护 web API。 这些标记允许您使用 Azure AD B2C 对 API 进行身份验证的客户端应用程序。 这篇文章演示了如何创建.NET 模型-视图-控制器 (MVC)"待办事项列表"API，从而使 CRUD 任务的用户。 Web API 使用 Azure AD B2C 保护，而只允许经过身份验证的用户可以管理他们的待办事项列表。

## <a name="create-an-azure-ad-b2c-directory"></a>创建 Azure AD B2C 目录

您可以使用 Azure AD B2C 之前，必须创建一个目录，或租户。 目录是您的用户、 应用程序、 组和更多的所有的容器。 如果您还没有，然后再[创建一个 B2C 的目录](active-directory-b2c-get-started.md)本指南在继续进行。

## <a name="create-an-application"></a>创建应用程序

接下来，您需要在 B2C 目录中创建一个应用程序。 这样，它需要与您的应用程序安全地进行通信的 Azure 的广告信息。 若要创建一个应用程序，请按照[以下说明操作](active-directory-b2c-app-registration.md)。 请务必︰

- 包含**web 应用程序**或**web API**应用程序中。
- 使用**重定向的统一资源标识符**`https://localhost:44316/`的 web 应用程序。 这是此代码示例的 web 应用程序客户端的默认位置。
- 复制**应用程序 ID**分配给您的应用程序。 您将稍后需要它。

 [AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>创建您的策略

在 Azure AD B2C，[策略](active-directory-b2c-reference-policies.md)定义了每个用户体验。 此代码示例中的客户端包含三个标识体验︰ 注册，登录，并编辑配置文件。 您将需要为每种类型，创建一个策略[策略参考文章](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy)中所述。 创建您的三个策略时，请务必︰

- 选择标识提供程序刀片式服务器中**注册的用户 ID**或**电子邮件注册**。
- 在您注册策略中选择**显示名称**和注册的其他属性。
- 为每个策略的应用程序声明选择**显示名称**和**对象 ID**的索赔。 您可以选择以及其他索赔。
- 您在创建后，将复制每个策略的**名称**。 稍后，您将需要这些策略名称。

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

您已成功创建的三个策略后，就可以生成您的应用程序。

## <a name="download-the-code"></a>下载的代码

[在 GitHub 上维护](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet)此教程的代码。 当您生成示例，您可以[下载的主干项目为.zip 文件](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/skeleton.zip)。 您还可以复制骨架︰

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet.git
```

已完成应用程序同时也是[一个.zip 文件](https://github.com/AzureADQuickStarts/B2C-WebAPI-DotNet/archive/complete.zip)或在`complete`的同一存储库中的分支。

下载示例代码之后，打开 Visual Studio 的.sln 文件，即可开始。 解决方案文件包含两个项目︰ `TaskWebApp` ， `TaskService`。 `TaskWebApp`是 MVC web 应用程序与用户进行交互。 `TaskService`是应用程序的后端 web API，用于存储每个用户的待办事项列表。

## <a name="configure-the-task-web-app"></a>配置任务的 web 应用程序

与用户交互`TaskWebApp`，客户端将请求发送到 Azure 的 AD 和重新获取可用于调用的标记`TaskService`web API。 若要登录用户并获取令牌，您需要提供`TaskWebApp`与您的应用程序的一些信息。 在`TaskWebApp`项目中打开`web.config`项目的根目录中的文件并替换中的值`<appSettings>`部分。  您可以将`AadInstance`， `RedirectUri`，和`TaskServiceUrl`值为-是。

```
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
    <add key="api:TaskServiceUrl" value="https://localhost:44332/" />
  </appSettings>
```

本文未涵盖构建`TaskWebApp`客户端。  若要了解如何构建 web 应用程序使用 Azure AD B2C，请参阅[我们的.NET web 应用程序教程](active-directory-b2c-devquickstarts-web-dotnet.md)。

## <a name="secure-the-api"></a>安全 API

代表用户调用 API 客户端后，您可以保护`TaskService`通过使用 OAuth 2.0 载体标记。 您的 API 可以接受和使用.NET (OWIN) 库 Microsoft 的打开 Web 界面验证令牌。

### <a name="install-owin"></a>安装 OWIN
首先安装 OWIN OAuth 身份验证管道︰

```
PM> Install-Package Microsoft.Owin.Security.OAuth -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Security.Jwt -ProjectName TaskService
PM> Install-Package Microsoft.Owin.Host.SystemWeb -ProjectName TaskService
```

### <a name="enter-your-b2c-details"></a>输入您的 B2C 详细信息
打开`web.config`文件的根目录中的`TaskService`项目并替换中的值`<appSettings>`部分。 整个 API 和 OWIN 库将使用这些值。  您可以将`AadInstance`值不变。

```
  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
  </appSettings>
```

### <a name="add-an-owin-startup-class"></a>添加 OWIN 启动类
添加到 OWIN 启动类`TaskService`项目称为`Startup.cs`。  右键单击项目，选择**添加**和**新项目**，然后搜索 OWIN。


```C#
// Startup.cs

// Change the class declaration to "public partial class Startup" - we’ve already implemented part of this class for you in another file.
public partial class Startup
{
    // The OWIN middleware will invoke this method when the app starts
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

### <a name="configure-oauth-20-authentication"></a>配置 OAuth 2.0 身份验证
打开的文件`App_Start\Startup.Auth.cs`，并实施`ConfigureAuth(...)`方法︰

```C#
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // These values are pulled from web.config
    public static string aadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
    public static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    public static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    public static string signUpPolicy = ConfigurationManager.AppSettings["ida:SignUpPolicyId"];
    public static string signInPolicy = ConfigurationManager.AppSettings["ida:SignInPolicyId"];
    public static string editProfilePolicy = ConfigurationManager.AppSettings["ida:UserProfilePolicyId"];

    public void ConfigureAuth(IAppBuilder app)
    {   
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(signUpPolicy));
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(signInPolicy));
        app.UseOAuthBearerAuthentication(CreateBearerOptionsFromPolicy(editProfilePolicy));
    }

    public OAuthBearerAuthenticationOptions CreateBearerOptionsFromPolicy(string policy)
    {
        TokenValidationParameters tvps = new TokenValidationParameters
        {
            // This is where you specify that your API only accepts tokens from its own clients
            ValidAudience = clientId,
            AuthenticationType = policy,
        };

        return new OAuthBearerAuthenticationOptions
        {
            // This SecurityTokenProvider fetches the Azure AD B2C metadata & signing keys from the OpenIDConnect metadata endpoint
            AccessTokenFormat = new JwtFormat(tvps, new OpenIdConnectCachingSecurityTokenProvider(String.Format(aadInstance, tenant, policy))),
        };
    }
}
```

### <a name="secure-the-task-controller"></a>安全任务控制器
应用程序配置为使用 OAuth 2.0 身份验证之后，您可以通过添加保护 web API`[Authorize]`标记添加到任务控制器。 这是在待办事项列表中的所有操作都时间，因此应该保护在类级别的整个控制器的控制器。 您还可以添加`[Authorize]`标记添加到单个操作的更细致的控制。

```C#
// Controllers\TasksController.cs

[Authorize]
public class TasksController : ApiController
{
    ...
}
```

### <a name="get-user-information-from-the-token"></a>从标记中获取用户信息
`TasksController`存储在数据库中，每个任务都有相关联的用户"拥有"该任务的任务。 由该用户的**对象 ID**标识所有者。 (这就是为什么您需要为应用程序添加的对象 ID 在所有您的策略声明。)

```C#
// Controllers\TasksController.cs

public IEnumerable<Models.Task> Get()
{
    string owner = ClaimsPrincipal.Current.FindFirst("http://schemas.microsoft.com/identity/claims/objectidentifier").Value;
    IEnumerable<Models.Task> userTasks = db.Tasks.Where(t => t.owner == owner);
    return userTasks;
}
```

## <a name="run-the-sample-app"></a>运行示例应用程序

最后，生成并运行两个`TaskWebApp`， `TaskService`。 通过使用电子邮件地址或用户名注册应用程序。 对用户的待办事项列表创建一些任务并注意如何在保持 API 中即使您停止并重新启动客户端。

## <a name="edit-your-policies"></a>编辑您的策略

您已经通过使用 Azure AD B2C 保护 API 之后，可以尝试使用您的应用程序策略和查看效果 （或作品缺乏） 的 api。 可以操纵在策略中的应用程序声明和更改 web API 中可用的用户信息。 您添加的任何索赔可用于.NET MVC web API 中的`ClaimsPrincipal`对象，如本文前面所述。

<!--

## Next steps

You can now move onto more advanced B2C topics. You may try:

[Call a web API from a web app]()

[Customize the UX of your B2C app]()

-->
