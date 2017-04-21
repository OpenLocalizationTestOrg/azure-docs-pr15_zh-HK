<properties
    pageTitle="Azure 的 Active Directory B2C |Microsoft Azure"
    description="如何构建 web 应用程序已注册，登录，并使用 Azure 活动目录 B2C 重设密码。"
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
    ms.topic="article"
    ms.date="07/22/2016"
    ms.author="dastrock"/>

# <a name="azure-ad-b2c-sign-up--sign-in-in-a-aspnet-web-app"></a>Azure AD B2C︰ 注册和在 ASP.NET Web 应用程序中的符号

<!-- TODO [AZURE.INCLUDE [active-directory-b2c-devquickstarts-web-switcher](../../includes/active-directory-b2c-devquickstarts-web-switcher.md)]-->

通过使用 Azure 活动目录 (AD Azure) B2C，可以添加强大的自助服务身份标识管理功能，为您的 web 应用程序中简单的几步。 这篇文章将讨论如何创建 ASP.NET web 应用程序，包括注册、 登录、 用户和密码重置。 应用程序将包括支持注册和登录使用的用户名或电子邮件，并通过社会帐户，如 Facebook 和 Google。

本教程中[我们其他.NET web 教程](active-directory-b2c-devquickstarts-web-dotnet.md)中不同之处在于它使用[注册或登录策略](active-directory-b2c-reference-policies.md#create-a-sign-up-or-sign-in-policy)以提供用户注册和登录的程序所使用的单个按钮，而不两个 (一个用于注册，另一个用于登录)。  在 nutshell，注册或登录策略允许用户签入与现有客户如果他们有的话，或者如果这是他们第一次使用该应用程序创建一个新。

## <a name="get-an-azure-ad-b2c-directory"></a>获取 Azure AD B2C 目录

您可以使用 Azure AD B2C 之前，必须创建一个目录，或租户。 目录是您的用户、 应用程序、 组和更多的所有的容器。  如果您还没有，然后再[创建一个 B2C 的目录](active-directory-b2c-get-started.md)本指南在继续进行。

## <a name="create-an-application"></a>创建应用程序

接下来，您需要在 B2C 目录中创建一个应用程序。 这样，它需要与您的应用程序安全地进行通信的 Azure 的广告信息。 若要创建一个应用程序，请按照[以下说明操作](active-directory-b2c-app-registration.md)。  请务必︰

- 包含**web 应用程序/web API**应用程序中。
- 输入`https://localhost:44316/`作为**重定向 URI**。 它是该代码示例的默认 URL。
- 复制下分配给您的应用程序的**应用程序 ID** 。  您将稍后需要它。

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>创建您的策略

在 Azure AD B2C，[策略](active-directory-b2c-reference-policies.md)定义了每个用户体验。 此代码示例包含两个标识体验︰**注册和登录**，然后**重设密码**。  您需要创建的每个类型，一种策略[策略参考文章](active-directory-b2c-reference-policies.md)中所述。 创建两个策略时，请务必︰

- 选择标识提供程序刀片式服务器中**注册的用户 ID**或**电子邮件注册**。
- 在您注册和登录的策略选择的**显示名称**和注册的其他属性。
- 选择**显示名称**声明为应用程序在每个策略声明。 您可以选择以及其他索赔。
- 您在创建后，将复制每个策略的**名称**。 稍后，您将需要这些策略名称。

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

创建两个策略后，就可以生成您的应用程序。

## <a name="download-the-code-and-configure-authentication"></a>下载的代码和配置身份验证

此示例[在 GitHub 上维护](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI)的代码。 当您生成示例，您可以[下载.zip 文件的主干项目](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI/archive/skeleton.zip)。 您还可以复制骨架︰

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI.git
```

已完成的示例同时也是[一个.zip 文件](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI/archive/complete.zip)或在`complete`的同一存储库中的分支。

下载示例代码之后，打开 Visual Studio 的.sln 文件，即可开始。

您的应用程序通过发送 HTTP 身份验证请求，指定要作为请求的一部分执行的策略与 Azure AD B2C 进行通信。 对于.NET web 应用程序，您可以使用 Microsoft 的 OWIN 库发送连接 OpenID 身份验证请求、 执行策略、 管理用户会话和更多。

若要开始，OWIN 中间件 NuGet 程序包向项目中添加使用 Visual Studio 程序包管理器控制台。

```
Install-Package Microsoft.Owin.Security.OpenIdConnect
Install-Package Microsoft.Owin.Security.Cookies
Install-Package Microsoft.Owin.Host.SystemWeb
Install-Package System.IdentityModel.Tokens.Jwt
```

接下来，打开`web.config`项目的根目录中的文件，然后输入您的应用程序中的配置值`<appSettings>`部分中，用自己替换下面的值。  可能使`ida:RedirectUri`和`ida:AadInstance`的值一样，保持不变。

```
<configuration>
  <appSettings>

    ...

    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}{1}{2}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SusiPolicyId" value="b2c_1_susi" />
    <add key="ida:PasswordResetPolicyId" value="b2c_1_reset" />
  </appSettings>
...
```

[AZURE.INCLUDE [active-directory-b2c-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]

接下来，添加一个 OWIN 启动类项目称为`Startup.cs`。 右键单击项目，选择**添加**和**新项目**，然后搜索"OWIN"。 更改的类声明为`public partial class Startup`。 我们在其他文件中为您实现此类的一部分。 OWIN 中间件将调用`Configuration(...)`在您的应用程序启动时的方法。 在此方法中，先打电话给`ConfigureAuth(...)`，在其中设置身份验证为您的应用程序。

```C#
// Startup.cs

public partial class Startup
{
    public void Configuration(IAppBuilder app)
    {
        ConfigureAuth(app);
    }
}
```

打开的文件`App_Start\Startup.Auth.cs`并实施`ConfigureAuth(...)`方法。  您在中提供的参数`OpenIdConnectAuthenticationOptions`作为您的应用程序坐标与 Azure 广告进行通信。 您还需要设置 cookie 身份验证。 OpenID 连接中间件使用 cookie 来维护用户会话，除了其他的事项。

```C#
// App_Start\Startup.Auth.cs

public partial class Startup
{
    // App config settings
    private static string clientId = ConfigurationManager.AppSettings["ida:ClientId"];
    private static string aadInstance = ConfigurationManager.AppSettings["ida:AadInstance"];
    private static string tenant = ConfigurationManager.AppSettings["ida:Tenant"];
    private static string redirectUri = ConfigurationManager.AppSettings["ida:RedirectUri"];

    // B2C policy identifiers
    public static string SusiPolicyId = ConfigurationManager.AppSettings["ida:SusiPolicyId"];
    public static string PasswordResetPolicyId = ConfigurationManager.AppSettings["ida:PasswordResetPolicyId"];

    public void ConfigureAuth(IAppBuilder app)
    {
        app.SetDefaultSignInAsAuthenticationType(CookieAuthenticationDefaults.AuthenticationType);

        app.UseCookieAuthentication(new CookieAuthenticationOptions());

        // Configure OpenID Connect middleware for each policy
        app.UseOpenIdConnectAuthentication(CreateOptionsFromPolicy(PasswordResetPolicyId));
        app.UseOpenIdConnectAuthentication(CreateOptionsFromPolicy(SusiPolicyId));

    }

    private Task OnSecurityTokenValidated(SecurityTokenValidatedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
    {
        // If you wanted to keep some local state in the app (like a db of signed up users),
        // you could use this notification to create the user record if it does not already
        // exist.

        return Task.FromResult(0);
    }

    private OpenIdConnectAuthenticationOptions CreateOptionsFromPolicy(string policy)
    {
        return new OpenIdConnectAuthenticationOptions
        {
            // For each policy, give OWIN the policy-specific metadata address, and
            // set the authentication type to the id of the policy
            MetadataAddress = String.Format(aadInstance, tenant, policy),
            AuthenticationType = policy,

            // These are standard OpenID Connect parameters, with values pulled from web.config
            ClientId = clientId,
            RedirectUri = redirectUri,
            PostLogoutRedirectUri = redirectUri,
            Notifications = new OpenIdConnectAuthenticationNotifications
            {
                AuthenticationFailed = AuthenticationFailed,
                SecurityTokenValidated = OnSecurityTokenValidated,
            },
            Scope = "openid",
            ResponseType = "id_token",

            // This piece is optional - it is used for displaying the user's name in the navigation bar.
            TokenValidationParameters = new TokenValidationParameters
            {
                NameClaimType = "name",
            },
        };
    }
}
...
```

## <a name="send-authentication-requests-to-azure-ad"></a>将身份验证请求发送到 Azure 的广告
您的应用程序现在正确配置为通过使用 OpenID 连接的身份验证协议与 Azure AD B2C 通信。  OWIN 进行处理的所有手工创建身份验证消息、 验证令牌从 Azure 的广告，和维护用户会话的详细信息。  剩下的是以启动每个用户的流量。

当用户选择**登录**或**忘记了密码？**中调用关联的操作是在 web 应用程序中， `Controllers\AccountController.cs`。 在每种情况下，您可以使用内置的 OWIN 方法触发正确的策略︰

```C#
// Controllers\AccountController.cs

public void Login()
{
    if (!Request.IsAuthenticated)
    {
        // To execute a policy, you simply need to trigger an OWIN challenge.
        // You can indicate which policy to use by specifying the policy id as the AuthenticationType
        HttpContext.GetOwinContext().Authentication.Challenge(
            new AuthenticationProperties() { RedirectUri = "/" }, Startup.SusiPolicyId);
    }
}

public void ResetPassword()
{
    if (!Request.IsAuthenticated)
    {
        HttpContext.GetOwinContext().Authentication.Challenge(
        new AuthenticationProperties() { RedirectUri = "/" }, Startup.PasswordResetPolicyId);
    }
}
```

注册过程执行期间或策略在每次登录时，用户有机会上单击**忘记了密码？**链接。  在这种情况，Azure AD B2C 将发送您的应用程序特定错误消息表明它应该执行密码重置策略。  您可以捕获该错误在`Startup.Auth.cs`使用`AuthenticationFailed`通知︰

```C#
// Used for avoiding yellow-screen-of-death TODO
private Task AuthenticationFailed(AuthenticationFailedNotification<OpenIdConnectMessage, OpenIdConnectAuthenticationOptions> notification)
{
    notification.HandleResponse();

    if (notification.ProtocolMessage.ErrorDescription != null && notification.ProtocolMessage.ErrorDescription.Contains("AADB2C90118"))
    {
        // If the user clicked the reset password link, redirect to the reset password route
        notification.Response.Redirect("/Account/ResetPassword");
    }
    else if (notification.Exception.Message == "access_denied")
    {
        // If the user canceled the sign in, redirect back to the home page
        notification.Response.Redirect("/");
    }
    else
    {
        notification.Response.Redirect("/Home/Error?message=" + notification.Exception.Message);
    }

    return Task.FromResult(0);
}
```


除了显式调用策略，您可以使用`[Authorize]`中您将执行策略，如果用户没有登录的控制器标签。 打开`Controllers\HomeController.cs`并添加`[Authorize]`标记添加到声明控制器。  OWIN 将选择最后一个配置策略时`[Authorize]`标记被命中。

```C#
// Controllers\HomeController.cs

// You can use the Authorize decorator to execute a certain policy if the user is not already signed into the app.
[Authorize]
public ActionResult Claims()
{
  ...
```

此外可以使用 OWIN 来注销用户从应用程序。 In `Controllers\AccountController.cs`:  

```C#
// Controllers\AccountController.cs

public void Logout()
{
    // To sign out the user, you should issue an OpenIDConnect sign out request.
    if (Request.IsAuthenticated)
    {
        IEnumerable<AuthenticationDescription> authTypes = HttpContext.GetOwinContext().Authentication.GetAuthenticationTypes();
        HttpContext.GetOwinContext().Authentication.SignOut(authTypes.Select(t => t.AuthenticationType).ToArray());
        Request.GetOwinContext().Authentication.GetAuthenticationTypes();
    }
}
```

## <a name="display-user-information"></a>显示用户信息
当您验证用户的身份通过使用 OpenID 连接时，Azure 的广告包含**声明**该应用程序返回 ID 标记。 这些都是与用户有关的断言。 可以使用声明来个性化您的应用程序。  

打开`Controllers\HomeController.cs`文件。 您可以通过在控制器中访问用户索赔`ClaimsPrincipal.Current`安全主体对象。

```C#
// Controllers\HomeController.cs

[Authorize]
public ActionResult Claims()
{
    Claim displayName = ClaimsPrincipal.Current.FindFirst(ClaimsPrincipal.Current.Identities.First().NameClaimType);
    ViewBag.DisplayName = displayName != null ? displayName.Value : string.Empty;
    return View();
}
```

您可以访问您的应用程序相同的方式收到任何索赔。  可供您在页上**声明**了该应用程序接收的所有索赔的列表。

## <a name="run-the-sample-app"></a>运行示例应用程序

最后，您可以生成并运行您的应用程序。 注册应用程序通过使用电子邮件地址或用户名称。 注销并重新登录用户相同的用户。 编辑该用户的配置文件。 注销并作为不同的用户注册。 请注意，**索赔**选项卡上显示的信息对应于您在您的策略配置的信息。

## <a name="add-social-idps"></a>添加社交 IDPs

目前，该应用程序支持用户注册和登录使用**本地帐户**。 这些都是使用用户名和密码的帐户存储在 B2C 目录。 通过使用 Azure AD B2C，可以添加对其他**身份提供程序**(IDPs) 的支持，而无需更改任何代码。

若要将社会 IDPs 添加到您的应用程序，开始在这些文章中的详细说明。 对于每个您想要支持的 IDP，您需要在该系统中注册应用程序并获取客户端 id。

- [将设置 Facebook 为 IDP](active-directory-b2c-setup-fb-app.md)
- [将设置 Google 为 IDP](active-directory-b2c-setup-goog-app.md)
- [设置为 IDP Amazon](active-directory-b2c-setup-amzn-app.md)
- [设置为 IDP LinkedIn](active-directory-b2c-setup-li-app.md)

标识提供程序添加到 B2C 目录后，您需要编辑每个三个策略来包括新 IDPs，[策略参考文章](active-directory-b2c-reference-policies.md)中所述。 在保存策略后，请再次运行该应用程序。  您应该可以看到新的 IDPs 添加为登录和注册的选项，在您的标识的每个经验。

可以对策略进行试验，观察您的样本应用程序的影响。 添加或删除 IDPs、 操纵应用程序声明或更改注册属性。 进行试验，直到您可以看到如何策略、 身份验证请求，以及 OWIN 绑定在一起。

为了便于参考，[提供以.zip 文件格式](https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI/archive/complete.zip)（而无需您配置的值） 的完整的示例。 您还可以从 GitHub 克隆它︰

```
git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-OpenIdConnect-DotNet-SUSI.git
```

<!--

## Next steps

You can now move on to more advanced B2C topics. You might try:

[Call a web API from a web app]()

[Customize the UX for a B2C app]()

-->
