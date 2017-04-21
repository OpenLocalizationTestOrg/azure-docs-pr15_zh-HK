<properties
    pageTitle="Azure AD Windows 应用商店开始 |Microsoft Azure"
    description="如何构建一个 Windows 应用商店应用程序集成登录的 Azure AD 和调用 Azure 广告受保护的 Api 使用 OAuth。"
    services="active-directory"
    documentationCenter="windows"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-windows-store"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="integrate-azure-ad-with-a-windows-store-app"></a>与 Windows 应用商店应用程序集成 Azure 的广告

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

如果您正在开发的 Windows 应用商店应用程序，Azure 的广告使简单和直接地进行身份验证的用户提供其 Active Directory 帐户。  它还使应用程序可以安全地使用任何 web Azure 的广告，例如，Office 365 Api 或 Azure API 由受保护的 API。

对于 Windows 应用商店的桌面应用程序需要访问受保护的资源，Azure 广告提供了活动目录身份验证库或 ADAL。  在现实生活中的 ADAL 的唯一目的是为了方便您的应用程序才能访问令牌。  以展示它是多么简单，此处我们将构建"目录搜索器"Windows 应用商店应用程序的︰

-   获取访问令牌用于调用 Azure 广告图形 API 使用[OAuth 2.0 身份验证协议](https://msdn.microsoft.com/library/azure/dn645545.aspx)。
-   搜索具有给定的 UPN 的用户目录。
-   标记出的用户。

若要生成完整的工作应用程序，您需要︰

2. 向应用程序注册 Azure 的广告。
3. 安装和配置 ADAL。
5. 使用 ADAL 从 Azure AD 获取令牌。

若要开始，[主干项目下载](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/skeleton.zip)或[下载完整的示例](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip)。  每个是一个 Visual Studio 2015年的解决方案。  您还将需要 Azure AD 租户可以在其中创建用户和注册应用程序。  如果您尚没有一个租户，[了解如何获取一个](active-directory-howto-tenant.md)。

## <a name="1-register-the-directory-searcher-application"></a>*1.注册目录搜索器应用程序*
若要使您的应用程序获取令牌，您首先需要将其注册在 Azure AD 租户，并授予它 Azure 广告图形 API 的访问权︰

-   登录到[Azure 的管理门户](https://manage.windowsazure.com)
-   在左侧导航中，单击**活动目录**
-   选择要在其中注册该应用程序一个租户。
-   单击**应用程序**选项卡，并单击**添加**在底部抽屉里。
-   请按照提示进行操作，创建新的**本机客户端应用程序**。
    -   应用程序**名称**将介绍您给最终用户的应用程序
    -   **重定向 Uri**是方案和字符串组合 Azure 广告将用于返回标记的响应。  现在，如输入占位符值`http://DirectorySearcher`。  我们将在以后替换该值。
-   完成注册后，AAD 会将您的应用程序分配一个唯一的客户机标识符。  您将需要此值在下一节中，所以将其复制从**配置**选项卡。
- 此外在**配置**选项卡上，查找"权限与其他应用程序"部分。  "Azure Active Directory"应用程序中，将添加**委派权限**下的**所在的目录中已登录的用户的访问**权限。  这将使您的应用程序用户在图形 API 中查询。

## <a name="2-install--configure-adal"></a>*2.安装和配置 ADAL*
现在，您已经在 Azure 广告应用程序，可以安装 ADAL 和编写标识相关代码。  为了使 ADAL 能与 Azure AD 间的通信，您需要为其提供一些有关您的应用程序的注册信息。
-   通过将 ADAL 添加到使用程序包管理器控制台 DirectorySearcher 项目开始。

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

-   在 DirectorySearcher 项目中，打开`MainPage.xaml.cs`。  替换中的值`Config Values`地区以反映到 Azure 门户输入的值。  它使用 ADAL 时，您的代码将引用这些值。
    -   `tenant`是您 Azure AD 租户，例如 contoso.onmicrosoft.com 的域
    -   `clientId`是来自门户应用程序的客户机 Id。
-   现在需要发现您的 Windows 应用商店应用程序的回调 uri。  在这行中设置断点`MainPage`方法︰

```
redirectURI = Windows.Security.Authentication.Web.WebAuthenticationBroker.GetCurrentApplicationCallbackUri();
```
- 生成解决方案，并确保恢复软件包的所有引用。  如果丢失的包，打开了 Nuget 程序包管理器和恢复软件包。
- 运行应用程序，并留出复制的值`redirectUri`命中断点时。  它应该看上去像

```
ms-app://s-1-15-2-1352796503-54529114-405753024-3540103335-3203256200-511895534-1429095407/
```

- 在 Azure 管理门户应用程序的**配置**选项卡上，在后替换此值的**RedirectUri**值。  

## <a name="3--use-adal-to-get-tokens-from-aad"></a>*3.使用 ADAL 从 AAD 获取令牌*
ADAL 背后的基本原则是，每当您的应用程序需要一个访问令牌，它只需调用`authContext.AcquireToken(…)`，和 ADAL 将完成其余工作。  

-   第一步是初始化应用程序的`AuthenticationContext`-ADAL 的主要类。  这是您将 ADAL 的传递与 Azure 广告进行通信并告诉它如何缓存标记所需的坐标。

```C#
public MainPage()
{
    ...

    authContext = new AuthenticationContext(authority);
}
```

- 现在，找到`Search(...)`方法，当用户单击应用程序的用户界面中的"搜索"按钮时将调用该方法。  此方法使 GET 请求到 Azure 广告图形 API 查询对其 UPN 开头给定的搜索条件的用户。  但为了查询图形 API，您需要包括在 access_token`Authorization`标头的请求-这是 ADAL 的进入。

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    ...
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectURI, new PlatformParameters(PromptBehavior.Auto, false));
    }
    catch (AdalException ex)
    {
        if (ex.ErrorCode != "authentication_canceled")
        {
            ShowAuthError(string.Format("If the error continues, please contact your administrator.\n\nError: {0}\n\nError Description:\n\n{1}", ex.ErrorCode, ex.Message));
        }
        return;
    }
    ...
}
```
- 当您的应用程序通过调用请求令牌`AcquireTokenAsync(...)`，ADAL 会返回一个标记而不要求用户提供凭据。  如果 ADAL 确定用户需要登录以获取令牌，它将显示登录对话框，收集用户的凭据，并返回在成功的身份验证令牌。  如果不能出于任何原因，返回标记 ADAL`AuthenticationResult`状态将是一个错误。

- 现在是时候使用您刚获得 access_token。  也在`Search(...)`方法中，将标记附加到授权标头中的图形 API 获取请求︰

```C#
// Add the access token to the Authorization Header of the call to the Graph API, and call the Graph API.
httpClient.DefaultRequestHeaders.Authorization = new HttpCredentialsHeaderValue("Bearer", result.AccessToken);

```
- 您还可以使用`AuthenticationResult`对象来显示有关用户的信息在应用程序中，如用户的 id:

```C#
// Update the Page UI to represent the signed in user
ActiveUser.Text = result.UserInfo.DisplayableId;
```
- 最后，可以使用 ADAL 来签署应用程序以及用户。  当用户单击"注销"按钮时，我们想要确保下一次`AcquireTokenAsync(...)`将在视图中显示一个符号。  ADAL，这是一样容易为交换令牌缓存︰

```C#
private void SignOut()
{
    // Clear session state from the token cache.
    authContext.TokenCache.Clear();

    ...
}
```

祝贺您 ！ 现在可用的 Windows 应用商店应用程序已验证用户身份，安全地调用 Web Api 使用 OAuth 2.0 的功能，并获取有关用户的基本信息。  如果还没有的话，现在是时间来填充某些用户与您租户。  运行 DirectorySearcher 应用程序，并使用一个这些用户登录。  搜索其他用户基于其 UPN。  关闭应用程序，然后重新运行它。  注意到该用户的会话将保持不变。  （用鼠标右键单击以显示底部栏），退出并重新登录以另一个用户的身份。

ADAL 可以轻松地将所有这些公共标识功能合并到您的应用程序。  它会为您的高速缓存管理，OAuth 协议支持，为各用户提供登录用户界面，刷新过期的标记，以及其他负责所有差事。  您真正需要知道的只是一个 API 调用， `authContext.AcquireToken*(…)`。

为了便于参考，提供完整的示例 （无需您的配置值）[此处](https://github.com/AzureADQuickStarts/NativeClient-WindowsStore/archive/complete.zip)。  您可以立即将移动到其他标识方案。  您可能想要尝试︰

[安全的.NET Web API 加装了 Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
