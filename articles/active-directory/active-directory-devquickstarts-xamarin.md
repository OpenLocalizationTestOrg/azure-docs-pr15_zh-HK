<properties
    pageTitle="入门的 azure AD Xamarin |Microsoft Azure"
    description="如何构建的 Xamarin 应用程序集成登录的 Azure AD 和调用 Azure 广告受保护的 Api 使用 OAuth。"
    services="active-directory"
    documentationCenter="xamarin"
    authors="dstrockis"
    manager="mbaldwin"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="dastrock"/>


# <a name="integrate-azure-ad-into-a-xamarin-app"></a>将 Azure AD 集成到 Xamarin 应用程序

[AZURE.INCLUDE [active-directory-devquickstarts-switcher](../../includes/active-directory-devquickstarts-switcher.md)]

[AZURE.INCLUDE [active-directory-devguide](../../includes/active-directory-devguide.md)]

Xamarin 允许您编写移动应用程序 C#，可在 iOS 和 Android，Windows （移动设备和 Pc） 上运行。 如果您正在构建一个应用程序使用 Xamarin，Azure 的广告使简单和直接地进行身份验证的用户提供其 Active Directory 帐户。 它还使应用程序可以安全地使用任何 web Azure 的广告，例如，Office 365 Api 或 Azure API 由受保护的 API。

对于 Xamarin 的应用程序需要访问受保护的资源，Azure 广告提供了活动目录身份验证库或 ADAL。 在现实生活中的 ADAL 的唯一目的是为了方便您的应用程序才能访问令牌。 以展示它是多么简单，此处我们将构建"目录搜索器"应用程序的︰

-   在 iOS、 Android、 Windows 桌面，Windows Phone 和 Windows 应用商店上运行。
- 用单个可移植类库 (PCL) 来对用户进行身份验证并获得 Azure 广告图形 API 标记
-   搜索具有给定的 UPN 的用户目录。

若要生成完整的工作应用程序，您需要︰

2. 设置开发环境 Xamarin
2. 向应用程序注册 Azure 的广告。
3. 安装和配置 ADAL。
5. 使用 ADAL 从 Azure AD 获取令牌。

若要开始，[主干项目下载](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/skeleton.zip)或[下载完整的示例](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip)。 每个是一个 Visual Studio 2013年的解决方案。 您还将需要 Azure AD 租户可以在其中创建用户和注册应用程序。 如果您尚没有一个租户，[了解如何获取一个](active-directory-howto-tenant.md)。

## <a name="0-set-up-your-xamarin-development-environment"></a>*0.Xamarin 开发环境设置*
由于本教程中包含的 iOS 和 Android，项目，将同时需要 Visual Studio 和 Xamarin。 若要创建必要的环境，按照有关[安装程序和安装 Visual Studio 和 Xamarin](https://msdn.microsoft.com/library/mt613162.aspx)在 MSDN 上的完整说明。 这些指令包括的材料可以查看以了解 Xamarin，虽然正在等待要完成的安装程序。 

一旦完成必要的安装程序，在入门的 Visual Studio 中打开解决方案。 您将找到六个项目︰ 五个特定于平台的项目和一个可移植类库将跨所有平台，共享`DirectorySearcher.cs`

## <a name="1-register-the-directory-searcher-application"></a>*1.注册目录搜索器应用程序*
若要使您的应用程序获取令牌，您首先需要将其注册在 Azure AD 租户，并授予它 Azure 广告图形 API 的访问权︰

-   登录到[Azure 的管理门户](https://manage.windowsazure.com)
-   在左侧导航中，单击**活动目录**
-   选择要在其中注册该应用程序一个租户。
-   单击**应用程序**选项卡，并单击**添加**在底部抽屉里。
-   请按照提示进行操作，创建新的**本机客户端应用程序**。
    -   应用程序**名称**将介绍您给最终用户的应用程序
    -   **重定向 Uri**是方案和字符串组合 Azure 广告将用于返回标记的响应。 输入一个值，例如`http://DirectorySearcher`。
-   完成注册后，AAD 会将您的应用程序分配一个唯一的客户机标识符。 您将需要此值在下一节中，所以将其复制从**配置**选项卡。
- 此外在**配置**选项卡上，查找"权限与其他应用程序"部分。 "Azure Active Directory"应用程序中，将添加**访问您组织的目录**权限**委派权限**下。 这将使您的应用程序用户在图形 API 中查询。

## <a name="2-install--configure-adal"></a>*2.安装和配置 ADAL*
现在，您已经在 Azure 广告应用程序，可以安装 ADAL 和编写标识相关代码。 为了使 ADAL 能与 Azure AD 间的通信，您需要为其提供一些有关您的应用程序的注册信息。
-   首先将 ADAL 添加到每个使用程序包管理器控制台解决方案中的项目。

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirectorySearcherLib
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Android
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Desktop
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-iOS
`

`
PM> Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -ProjectName DirSearchClient-Universal
`

- 您应注意到，两个库的引用添加到每个项目中的 ADAL 的 PCL 部分和特定于平台的一部分。

-   在 DirectorySearcherLib 项目中，打开`DirectorySearcher.cs`。 更改类成员的值，以反映到 Azure 门户输入的值。 它使用 ADAL 时，您的代码将引用这些值。
    -   `tenant`是您 Azure AD 租户，例如 contoso.onmicrosoft.com 的域
    -   `clientId`是来自门户应用程序的客户机 Id。
    - `returnUri`是您在中输入门户，例如 redirectUri `http://DirectorySearcher`。

## <a name="3--use-adal-to-get-tokens-from-aad"></a>*3.使用 ADAL 从 AAD 获取令牌*
所有应用程序的身份验证逻辑在于*Almost* `DirectorySearcher.SearchByAlias(...)`。 所需的特定于平台的项目中是传递上下文参数与`DirectorySearcher`PCL。

- 首先，打开`DirectorySearcher.cs`并添加一个新参数`SearchByAlias(...)`方法。 `IPlatformParameters`是封装 ADAL 需要执行身份验证的特定于平台的对象的上下文参数。

```C#
public static async Task<List<User>> SearchByAlias(string alias, IPlatformParameters parent)
{
```

-   然后，将初始化`AuthenticationContext`-ADAL 的主要类。 这是您将 ADAL 的传递与 Azure 广告进行通信所需的坐标。 然后调用`AcquireTokenAsync(...)`，接受`IPlatformParameters`对象，然后将调用返回到应用程序的标记所需的身份验证流程。

```C#
...
    AuthenticationResult authResult = null;
    try
    {
        AuthenticationContext authContext = new AuthenticationContext(authority);
        authResult = await authContext.AcquireTokenAsync(graphResourceUri, clientId, returnUri, parent);
    }
    catch (Exception ee)
    {
        results.Add(new User { error = ee.Message });
        return results;
    }
...
```
- `AcquireTokenAsync(...)`将首先尝试返回所请求的资源 (在此例中图形 API) 的标记而不会提示用户输入其凭据，（通过缓存或刷新旧标记）。 仅在必要时，它将显示用户的 Azure 广告符号页中获取请求的标记之前。


- 然后可以对图形 API 请求授权标头中附加的访问令牌︰

```C#
...
    request.Headers.Authorization = new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
...
```

这就是全部的`DirectorySearcher`PCL 和您的应用程序的标识相关的代码。  剩下的是调用`SearchByAlias(...)`方法在每个平台的视图中，并在必要时添加代码以正确处理 UI 生命周期。

####<a name="android"></a>Android:
- 在`MainActivity.cs`，添加对的调用`SearchByAlias(...)`的按钮单击处理程序︰

```C#
List<User> results = await DirectorySearcher.SearchByAlias(searchTermText.Text, new PlatformParameters(this));
```
- 此外需要重写`OnActivityResult`生命周期方法转发任何身份验证将重定向到适当的方法。  ADAL 在 Android 为此提供帮助器方法︰

```C#
...
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
    base.OnActivityResult(requestCode, resultCode, data);
    AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
}
...
```

####<a name="windows-desktop"></a>Windows 桌面︰
- 在`MainWindow.xaml.cs`，只需打电话到`SearchByAlias(...)`传递`WindowInteropHelper`在桌面的`PlatformParameters`对象︰

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

####<a name="ios"></a>iOS:
- 在`DirSearchClient_iOSViewController.cs`，iOS`PlatformParameters`对象只具有对视图控制器的引用︰

```C#
List<User> results = await DirectorySearcher.SearchByAlias(
  SearchTermText.Text,
  new PlatformParameters(PromptBehavior.Auto, this.Handle));
```

####<a name="windows-universal"></a>Windows 通用︰
- 在 Windows 通用打开`MainPage.xaml.cs`并实施`Search`方法，它使用共享项目中的帮助器方法来更新所需的用户界面。

```C#
...
    List<User> results = await DirectorySearcherLib.DirectorySearcher.SearchByAlias(SearchTermText.Text, new PlatformParameters(PromptBehavior.Auto, false));
...
```

祝贺您 ！ 您现在可以正常工作的 Xamarin 应用程序具有对用户进行身份验证和安全地调用 Web Api 跨五个不同的平台使用 OAuth 2.0 的能力。 如果还没有的话，现在是时间来填充某些用户与您租户。 运行 DirectorySearcher 应用程序，并使用一个这些用户登录。 搜索其他用户基于其 UPN。

ADAL 可以轻松地将通用标识功能合并到您的应用程序。 它会为您的高速缓存管理，OAuth 协议支持，为各用户提供登录用户界面，刷新过期的标记，以及其他负责所有差事。 您真正需要知道的只是一个 API 调用， `authContext.AcquireToken*(…)`。

为了便于参考，提供完整的示例 （无需您的配置值）[此处](https://github.com/AzureADQuickStarts/NativeClient-MultiTarget-DotNet/archive/complete.zip)。 您可以立即将移动到其他标识方案。 您可能想要尝试︰

[安全的.NET Web API 加装了 Azure AD >>](active-directory-devquickstarts-webapi-dotnet.md)

[AZURE.INCLUDE [active-directory-devquickstarts-additional-resources](../../includes/active-directory-devquickstarts-additional-resources.md)]
