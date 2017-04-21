<properties
    pageTitle="Azure AD.NET 快速入门 |Microsoft Azure"
    description="如何生成的.NET Windows 桌面应用程序集成登录的 Azure AD 和调用 Azure 广告受保护的 Api 使用 OAuth。"
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


# <a name="integrate-azure-ad-into-a-windows-desktop-wpf-app"></a>将 Azure AD 集成到 Windows 桌面的 WPF 应用程序

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

如果要开发桌面应用程序，Azure 的广告使简单和直接地进行身份验证的用户提供其 Active Directory 帐户。  它还使应用程序可以安全地使用任何 web Azure 的广告，例如，Office 365 Api 或 Azure API 由受保护的 API。

对于.NET 本机客户端需要访问受保护的资源，Azure 广告提供了活动目录身份验证库或 ADAL。  在现实生活中的 ADAL 的唯一目的是为了方便您的应用程序才能访问令牌。  以展示它是多么简单，此处我们将构建一个.NET WPF 待办事项列表的应用程序的︰

-   获取访问令牌用于调用 Azure 广告图形 API 使用[OAuth 2.0 身份验证协议](https://msdn.microsoft.com/library/azure/dn645545.aspx)。
-   搜索具有指定别名的用户目录。
-   标记出的用户。

若要生成完整的工作应用程序，您需要︰

2. 向应用程序注册 Azure 的广告。
3. 安装和配置 ADAL。
5. 使用 ADAL 从 Azure AD 获取令牌。

若要开始，[下载应用程序骨架](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/skeleton.zip)或[下载已完成的示例](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip)。  您还将需要 Azure AD 租户可以在其中创建用户和注册应用程序。  如果您尚没有一个租户，[了解如何获取一个](active-directory-howto-tenant.md)。

## <a name="1-register-the-directorysearcher-application"></a>*1.注册的 DirectorySearcher 应用程序*
若要使您的应用程序获取令牌，您首先需要将其注册在 Azure AD 租户，并授予它 Azure 广告图形 API 的访问权︰

-   登录到 Azure 的管理门户
-   在左侧导航中，单击**活动目录**
-   选择要在其中注册该应用程序一个租户。
-   单击**应用程序**选项卡，并单击**添加**在底部抽屉里。
-   请按照提示进行操作，创建新的**本机客户端应用程序**。
    -   应用程序**名称**将介绍您给最终用户的应用程序
    -   **重定向 Uri**是方案和字符串组合 Azure 广告将用于返回标记的响应。  输入一个值特定于您的应用程序，例如`http://DirectorySearcher`。
-   完成注册后，AAD 会将您的应用程序分配一个唯一的客户机标识符。  您将需要此值在下一节中，所以将其复制从**配置**选项卡。
- 此外在**配置**选项卡上，查找"权限与其他应用程序"部分。  "Azure Active Directory"应用程序中，将添加**访问您组织的目录**权限**委派权限**下。  这将使您的应用程序用户在图形 API 中查询。

## <a name="2-install--configure-adal"></a>*2.安装和配置 ADAL*
现在，您已经在 Azure 广告应用程序，可以安装 ADAL 和编写标识相关代码。  为了使 ADAL 能与 Azure AD 间的通信，您需要为其提供一些有关您的应用程序的注册信息。
-   通过将 ADAL 添加到使用程序包管理器控制台 DirectorySearcher 项目开始。

```
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory
```

-   在 DirectorySearcher 项目中，打开`app.config`。  中的元素的值替换`<appSettings>`节，以反映到 Azure 门户输入的值。  它使用 ADAL 时，您的代码将引用这些值。
    -   `ida:Tenant`是您 Azure AD 租户，例如 contoso.onmicrosoft.com 的域
    -   `ida:ClientId`是来自门户应用程序的客户机 Id。
    -   `ida:RedirectUri`是重定向 url 在门户中注册。

## <a name="3--use-adal-to-get-tokens-from-aad"></a>*3.使用 ADAL 从 AAD 获取令牌*
ADAL 背后的基本原则是，每当您的应用程序需要一个访问令牌，它只需调用`authContext.AcquireTokenAsync(...)`，和 ADAL 将完成其余工作。  

-   在`DirectorySearcher`项目中打开`MainWindow.xaml.cs`，找到`MainWindow()`方法。  第一步是初始化应用程序的`AuthenticationContext`-ADAL 的主要类。  这是您将 ADAL 的传递与 Azure 广告进行通信并告诉它如何缓存标记所需的坐标。

```C#
public MainWindow()
{
    InitializeComponent();

    authContext = new AuthenticationContext(authority, new FileCache());

    CheckForCachedToken();
}
```

- 现在，找到`Search(...)`应用程序的用户界面中的用户 cliks"搜索"按钮时将调用的方法。  此方法使 GET 请求到 Azure 广告图形 API 查询对其 UPN 开头给定的搜索条件的用户。  但为了查询图形 API，您需要包括在 access_token`Authorization`标头的请求-这是 ADAL 的进入。

```C#
private async void Search(object sender, RoutedEventArgs e)
{
    // Validate the Input String
    if (string.IsNullOrEmpty(SearchText.Text))
    {
        MessageBox.Show("Please enter a value for the To Do item name");
        return;
    }

    // Get an Access Token for the Graph API
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Auto));
        UserNameLabel.Content = result.UserInfo.DisplayableId;
        SignOutButton.Visibility = Visibility.Visible;
    }
    catch (AdalException ex)
    {
        // An unexpected error occurred, or user canceled the sign in.
        if (ex.ErrorCode != "access_denied")
            MessageBox.Show(ex.Message);

        return;
    }

    ...
}
```
- 当您的应用程序通过调用请求令牌`AcquireTokenAsync(...)`，ADAL 会返回一个标记而不要求用户提供凭据。  如果 ADAL 确定用户需要登录以获取令牌，它将显示登录对话框，收集用户的凭据，并返回在成功的身份验证令牌。  如果 ADAL 不能出于任何原因返回标记，则将引发`AdalException`。
- 请注意，`AuthenticationResult`对象包含`UserInfo`可以用来收集您的应用程序可能需要的信息的对象。  在 DirectorySearcher，`UserInfo`用于自定义应用程序的用户界面与用户的 id。

- 当用户单击"注销"按钮时，我们想要确保下一次`AcquireTokenAsync(...)`将要求用户登录。  ADAL，这是一样容易为交换令牌缓存︰

```C#
private void SignOut(object sender = null, RoutedEventArgs args = null)
{
    // Clear the token cache
    authContext.TokenCache.Clear();

    ...
}
```

- 但是，如果用户不单击"注销"按钮时，将想要维护的下一次运行 DirectorySearcher 的用户的会话。  当应用程序启动时，可以检查现有标记为 ADAL 的令牌缓存并相应地更新用户界面。  在`CheckForCachedToken()`方法，再打电话到`AcquireTokenAsync(...)`，传入这次`PromptBehavior.Never`参数。  `PromptBehavior.Never`将告诉 ADAL，应不会提示用户输入登录，而如果它不能返回标记 ADAL 应引发异常。

```C#
public async void CheckForCachedToken() 
{
    // As the application starts, try to get an access token without prompting the user.  If one exists, show the user as signed in.
    AuthenticationResult result = null;
    try
    {
        result = await authContext.AcquireTokenAsync(graphResourceId, clientId, redirectUri, new PlatformParameters(PromptBehavior.Never));
    }
    catch (AdalException ex)
    {
        if (ex.ErrorCode != "user_interaction_required")
        {
            // An unexpected error occurred.
            MessageBox.Show(ex.Message);
        }

        // If user interaction is required, proceed to main page without singing the user in.
        return;
    }

    // A valid token is in the cache
    SignOutButton.Visibility = Visibility.Visible;
    UserNameLabel.Content = result.UserInfo.DisplayableId;
}
```

祝贺您 ！ 现在具有有效的.NET WPF 应用程序已验证用户身份，安全地调用 Web Api 使用 OAuth 2.0 的功能，并获取有关用户的基本信息。  如果还没有的话，现在是时间来填充某些用户与您租户。  运行 DirectorySearcher 应用程序，并使用一个这些用户登录。  搜索其他用户基于其 UPN。  关闭应用程序，然后重新运行它。  注意到该用户的会话将保持不变。  注销，并重新登录以另一个用户的身份。

ADAL 可以轻松地将所有这些公共标识功能合并到您的应用程序。  它会为您的高速缓存管理，OAuth 协议支持，为各用户提供登录用户界面，刷新过期的标记，以及其他负责所有差事。  您真正需要知道的只是一个 API 调用， `authContext.AcquireTokenAsync(...)`。

为了便于参考，提供完整的示例 （无需您的配置值）[此处](https://github.com/AzureADQuickStarts/NativeClient-DotNet/archive/complete.zip)。  您可以立即将移动到其他方案。  您可能想要尝试︰

[安全的.NET Web API 加装了 Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
