<properties
    pageTitle="Azure 的 Active Directory B2C |Microsoft Azure"
    description="如何构建的 web 应用程序调用，方法是使用 Azure 活动目录 B2C 网站 API。"
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

# <a name="azure-ad-b2c-call-a-web-api-from-a-net-web-app"></a>Azure AD B2C︰ 从.NET web 应用程序中调用 web API

通过使用 Azure 活动目录 (AD Azure) B2C，可以添加强大的自助服务身份标识管理功能，为您的 web 应用程序和 web Api 中简单的几步。 这篇文章将讨论如何创建.NET 模型-视图-控制器 (MVC)"待办事项列表"web 应用程序调用 web API 使用载体的标记

本文不介绍如何实现登录、 注册和配置文件管理 Azure AD B2C。 它强调的是调用 web Api 用户已通过身份验证后。 如果尚未启动，首先应具有[.NET web 应用程序快速入门教程](active-directory-b2c-devquickstarts-web-dotnet.md)了解 Azure AD B2C 的基础知识。

## <a name="get-an-azure-ad-b2c-directory"></a>获取 Azure AD B2C 目录

您可以使用 Azure AD B2C 之前，必须创建一个目录，或租户。  目录是您的所有用户、 应用程序、 组和更多的容器。  如果您还没有，然后再[创建一个 B2C 的目录](active-directory-b2c-get-started.md)本指南在继续进行。

## <a name="create-an-application"></a>创建应用程序

接下来，您需要在 B2C 目录中创建一个应用程序。 这样，它需要与您的应用程序安全地进行通信的 Azure 的广告信息。 在这种情况下，web 应用程序和 web API 将表示通过单一**应用程序 ID**，因为它们构成了一个逻辑的应用程序。 若要创建一个应用程序，请按照[以下说明操作](active-directory-b2c-app-registration.md)。 请务必︰

- 包含**web 应用程序/web API**应用程序中。
- 输入`https://localhost:44316/`作为**回复 URL**。 它是该代码示例的默认 URL。
- 复制**应用程序 ID**分配给您的应用程序。 您还需要这以后。

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>创建您的策略

在 Azure AD B2C，[策略](active-directory-b2c-reference-policies.md)定义了每个用户体验。 这个 web 应用程序包含三个标识体验︰ 注册，登录，并编辑配置文件。 您需要创建的每个类型，一种策略[策略参考文章](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy)中所述。 创建三个策略时，请务必︰

- 在您注册策略选择的**显示名称**和注册的其他属性。
- 在每个策略选择的**显示名称**和**对象 ID**的应用程序声明。 您可以选择以及其他索赔。
- 您在创建后，将复制每个策略的**名称**。 它应具有前缀`b2c_1_`。 稍后，您将需要这些策略名称。

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

您已经创建了三种策略后，就可以生成您的应用程序。

请注意，这篇文章没有涵盖如何使用您刚才创建的策略。 若要了解有关在 Azure AD B2C 策略如何工作，开始与[.NET web 应用程序快速入门教程](active-directory-b2c-devquickstarts-web-dotnet.md)。

## <a name="download-the-code"></a>下载的代码

[在 GitHub 上维护](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet)此教程的代码。 当您生成示例，您可以[下载.zip 文件的主干项目](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/skeleton.zip)。 您还可以复制骨架︰

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet.git
```

已完成应用程序同时也是[一个.zip 文件](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/complete.zip)或在`complete`的同一存储库中的分支。

下载示例代码之后，打开 Visual Studio 的.sln 文件，即可开始。

## <a name="configure-the-task-web-app"></a>配置任务的 web 应用程序

若要获取`TaskWebApp`与 Azure AD B2C 通信，您需要提供一些常用的参数。 在`TaskWebApp`项目中打开`web.config`项目的根目录中的文件并替换中的值`<appSettings>`部分。 您可以将`AadInstance`， `RedirectUri`，和`TaskServiceUrl`值不变。

```
  <appSettings>
    
    ...
    
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
    <add key="api:TaskServiceUrl" value="https://aadb2cplayground.azurewebsites.net" />
  </appSettings>
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]  <appSettings>
    <add key="webpages:Version" value="3.0.0.0" />
    <add key="webpages:Enabled" value="false" />
    <add key="ClientValidationEnabled" value="true" />
    <add key="UnobtrusiveJavaScriptEnabled" value="true" />
    <add key="ida:Tenant" value="fabrikamb2c.onmicrosoft.com" />
    <add key="ida:ClientId" value="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" />
    <add key="ida:ClientSecret" value="E:i~5GHYRF$Y7BcM" />
    <add key="ida:AadInstance" value="https://login.microsoftonline.com/{0}/v2.0/.well-known/openid-configuration?p={1}" />
    <add key="ida:RedirectUri" value="https://localhost:44316/" />
    <add key="ida:SignUpPolicyId" value="b2c_1_sign_up" />
    <add key="ida:SignInPolicyId" value="b2c_1_sign_in" />
    <add key="ida:UserProfilePolicyId" value="b2c_1_edit_profile" />
    <add key="api:TaskServiceUrl" value="https://aadb2cplayground.azurewebsites.net" />
  </appSettings>

## <a name="get-access-tokens-and-call-the-task-api"></a>获取访问令牌，调用 API 的任务

本部分将讨论如何使用登录使用 Azure AD B2C 期间收到的令牌来访问站点使用 Azure AD B2C 还受保护 API。

本文不介绍如何保护 API 的详细信息。 若要了解如何 web API 安全地进行身份验证的请求使用 Azure AD B2C，签出[web API 入门文章](active-directory-b2c-devquickstarts-api-dotnet.md)。

### <a name="save-the-sign-in-token"></a>在标记中保存符号

首先，验证用户 （使用任何一种策略），并从 Azure AD B2C 接收登录令牌。  如果您不确定如何执行策略，回过头来尝试[.NET web 应用程序快速入门教程](active-directory-b2c-devquickstarts-web-dotnet.md)，了解 Azure AD B2C 的基础知识。

打开的文件`App_Start\Startup.Auth.cs`。  还有一个重要的更改，您必须对`OpenIdConnectAuthenticationOptions`-必须设置`SaveSignInToken = true`。

```C#
// App_Start\Startup.Auth.cs

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
        AuthenticationFailed = OnAuthenticationFailed,
    },
    Scope = "openid",
    ResponseType = "id_token",

    TokenValidationParameters = new TokenValidationParameters
    {
        NameClaimType = "name",
        
        // Add this line to reserve the sign in token for later use
        SaveSigninToken = true,
    },
};
```

### <a name="get-a-token-in-the-controllers"></a>获取控制器中的标记

`TasksController`负责与 web api，该 API 将 HTTP 请求发送到要读取、 创建和删除任务的 API 进行通信。  由于 Azure AD B2C 受保护 API，您需要首先检索保存在上述步骤中的标记。

```C#
// Controllers\TasksController.cs

public async Task<ActionResult> Index()
{
    try { 

        var bootstrapContext = ClaimsPrincipal.Current.Identities.First().BootstrapContext as System.IdentityModel.Tokens.BootstrapContext;
        
    ...
}
```

`BootstrapContext`包含在您通过执行一种 B2C 策略获得的令牌符号。

### <a name="read-tasks-from-the-web-api"></a>从 web API 读取任务

当您有一个令牌时，您可以将它附加到 HTTP`GET`请求中`Authorization`标头来安全地调用`TaskService`:

```C#
// Controllers\TasksController.cs

public async Task<ActionResult> Index()
{
    try { 

        ...

        HttpClient client = new HttpClient();
        HttpRequestMessage request = new HttpRequestMessage(HttpMethod.Get, serviceUrl + "/api/tasks");

        // Add the token acquired from ADAL to the request headers
        request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", bootstrapContext.Token);
        HttpResponseMessage response = await client.SendAsync(request);

        if (response.IsSuccessStatusCode)
        {
            String responseString = await response.Content.ReadAsStringAsync();
            JArray tasks = JArray.Parse(responseString);
            ViewBag.Tasks = tasks;
            return View();
        }
        else
        {
            // If the call failed with access denied, show the user an error indicating they might need to sign-in again.
            if (response.StatusCode == System.Net.HttpStatusCode.Unauthorized)
            {
                return new RedirectResult("/Error?message=Error: " + response.ReasonPhrase + " You might need to sign in again.");
            }
        }

        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + response.StatusCode);
    }
    catch (Exception ex)
    {
        return new RedirectResult("/Error?message=An Error Occurred Reading To Do List: " + ex.Message);
    }
}

```

### <a name="create-and-delete-tasks-on-the-web-api"></a>创建和删除 web API 上的任务

发送时遵循相同的模式`POST`和`DELETE`请求 web API，使用`BootstrapContext`检索令牌中的签名。 我们实施了为您创建操作。 您可以尝试完成删除操作在`TasksController.cs`。

## <a name="run-the-sample-app"></a>运行示例应用程序

最后，生成并运行该应用程序。 注册和登录，并创建为已登录的用户的任务。 退出并以其他用户身份登录。 创建该用户的任务。 请注意如何任务存储每个用户的 api，因为 API 从它收到的标记中提取用户的身份。

为了便于参考，[以.zip 文件格式提供](https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet/archive/complete.zip)完整的示例。 您还可以从 GitHub 克隆它︰

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-WebApp-WebAPI-OpenIDConnect-DotNet.git```

<!--

## Next steps

You can now move on to more advanced B2C topics. You might try:

[Call a web API from a web app]()

[Customize the UX for a B2C app]()

-->
