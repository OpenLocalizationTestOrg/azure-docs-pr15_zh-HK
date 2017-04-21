<properties
pageTitle="Azure 的 Active Directory v2.0.NET 本机应用程序 |Microsoft Azure"
description="如何构建.NET 本机应用程序进行签名以两个人的 Microsoft 帐户的用户和工作或学校的帐户。"
services="active-directory"
documentationCenter=""
authors="dstrockis"
manager="mbaldwin"
editor=""/>

<tags
ms.service="active-directory"
ms.workload="identity"
ms.tgt_pltfrm="na"
ms.devlang="dotnet"
ms.topic="article"
ms.date="07/30/2016"
ms.author="dastrock; vittorib"/>

# <a name="add-sign-in-to-a-windows-desktop-app"></a>添加登录到 Windows 桌面应用程序

使用 v2.0 终结点，您可以快速添加到这两个人的 Microsoft 客户支持桌面应用程序的身份验证和工作或学校的科目。  它还使您的应用程序 api，以及[Microsoft Graph](https://graph.microsoft.io)与后端 web 安全通信和几个[Office 365 统一的 Api](https://www.msdn.com/office/office365/howto/authenticate-Office-365-APIs-using-v2)。

> [AZURE.NOTE] V2.0 终结点支持并不是所有的 Azure 活动目录 (AD) 方案和功能。  要确定是否应使用 v2.0 终结点，阅读有关[v2.0 限制](active-directory-v2-limitations.md)。

对于[.NET 本机应用程序运行在一台设备](active-directory-v2-flows.md#mobile-and-native-apps)，Azure AD 提供 Microsoft 身份验证库或 MSAL。  在现实生活中的 MSAL 的唯一目的是为了方便您的应用程序调用 web 服务获取令牌。  以展示它是多么简单，此处我们将构建.NET WPF 待办事项列表的应用程序的︰

- 登录用户并获取访问令牌使用[OAuth 2.0 身份验证协议](active-directory-v2-protocols.md#oauth2-authorization-code-flow)。
- 安全地调用后端待办事项列表 web 服务，它还受 OAuth 2.0。
- 进行签名的用户。

## <a name="download-sample-code"></a>下载代码示例

本教程中的代码[在 GitHub 上](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet)维护。  要继续下去，您可以[下载应用程序的主干作为.zip](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/skeleton.zip)或克隆主干︰

    git clone --branch skeleton https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git

同时本教程末尾提供了已完成的应用程序。

## <a name="register-an-app"></a>注册应用程序
在[apps.dev.microsoft.com](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)，创建新的应用程序，或按照以下[详细的步骤](active-directory-v2-app-registration.md)。  请确保︰

- 复制下分配给您的应用程序的**应用程序 Id** ，您需要很快。
- 添加您的应用程序的**移动**平台。

## <a name="install--configure-msal"></a>安装和配置 MSAL
现在，您已经使用 Microsoft 注册的应用程序，可以安装 MSAL 并编写与身份相关的代码。  为了使 MSAL 能传达 v2.0 终结点，您需要为其提供一些有关您的应用程序的注册信息。

-   通过将 MSAL 添加到使用程序包管理器控制台 TodoListClient 项目开始。

```
PM> Install-Package Microsoft.Identity.Client -ProjectName TodoListClient -IncludePrerelease
```

-   在 TodoListClient 项目中，打开`app.config`。  中的元素的值替换`<appSettings>`节，以反映您向应用程序注册门户输入的值。  只要它使用 MSAL，您的代码将引用这些值。
    -   `ida:ClientId`来自门户应用程序的**应用程序 Id** 。

- 在任务列表服务项目中，打开`web.config`项目的根目录中的。  
    - 更换`ida:Audience`值具有相同**的应用程序 Id**从门户。

## <a name="use-msal-to-get-tokens"></a>使用 MSAL 来获得标记
MSAL 背后的基本原则是，每当您的应用程序需要一个访问令牌，您只需调用`app.AcquireToken(...)`，和 MSAL 将完成其余工作。  

-   在`TodoListClient`项目中打开`MainWindow.xaml.cs`，找到`OnInitialized(...)`方法。  第一步是初始化应用程序的`PublicClientApplication`-MSAL 的表示本机应用程序的主类。  这是在您传递 MSAL Azure 广告与通信并告诉它如何缓存标记所需的坐标。

```C#
protected override async void OnInitialized(EventArgs e)
{
        base.OnInitialized(e);

        app = new PublicClientApplication(new FileCache());
        AuthenticationResult result = null;
        ...
}
```

- 当应用程序启动时，我们需要检查，是否用户已登录到该应用程序，请参阅。  但是，我们不想只是尚未调用登录的用户界面-我们会使用户通过单击"登录"要这样做。  也在`OnInitialized(...)`方法︰

```C#
// As the app starts, we want to check to see if the user is already signed in.
// You can do so by trying to get a token from MSAL, using the method
// AcquireTokenSilent.  This forces MSAL to throw an exception if it cannot
// get a token for the user without showing a UI.
try
{
    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
    // If we got here, a valid token is in the cache - or MSAL was able to get a new oen via refresh token.
    // Proceed to fetch the user's tasks from the TodoListService via the GetTodoList() method.
    
    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "user_interaction_required")
    {
        // If user interaction is required, the app should take no action,
        // and simply show the user the sign in button.
    }
    else
    {
        // Here, we catch all other MsalExceptions
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }
}

```

- 如果用户未登录并单击"登录"按钮，我们需要调用登录用户界面并让用户输入其凭据。  实现登录按钮处理程序︰

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // TODO: Sign the user out if they clicked the "Clear Cache" button

// If the user clicked the 'Sign-In' button, force
// MSAL to prompt the user for credentials by using
// AcquireTokenAsync, a method that is guaranteed to show a prompt to the user.
// MSAL will get a token for the TodoListService and cache it for you.

AuthenticationResult result = null;
try
{
    result = await app.AcquireTokenAsync(new string[] { clientId });
    SignInButton.Content = "Clear Cache";
    GetTodoList();
}
catch (MsalException ex)
{
    // If MSAL cannot get a token, it will throw an exception.
    // If the user canceled the login, it will result in the
    // error code 'authentication_canceled'.

    if (ex.ErrorCode == "authentication_canceled")
    {
        MessageBox.Show("Sign in was canceled by the user");
    }
    else
    {
        // An unexpected error occurred.
        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }

        MessageBox.Show(message);
    }

    return;
}


}
```

- 如果用户成功迹象中，MSAL 将接收并缓存标记，您可以继续调用`GetTodoList()`满怀信心的方法。  剩下来获取用户的任务是实现`GetTodoList()`方法。

```C#
private async void GetTodoList()
{

AuthenticationResult result = null;
try
{
    // Here, we try to get an access token to call the TodoListService
    // without invoking any UI prompt.  AcquireTokenSilentAsync forces
    // MSAL to throw an exception if it cannot get a token silently.


    result = await app.AcquireTokenSilentAsync(new string[] { clientId });
}
catch (MsalException ex)
{
    // MSAL couldn't get a token silently, so show the user a message
    // and let them click the Sign-In button.

    if (ex.ErrorCode == "user_interaction_required")
    {
        MessageBox.Show("Please sign in first");
        SignInButton.Content = "Sign In";
    }
    else
    {
        // In any other case, an unexpected error occurred.

        string message = ex.Message;
        if (ex.InnerException != null)
        {
            message += "Inner Exception : " + ex.InnerException.Message;
        }
        MessageBox.Show(message);
    }

    return;
}

// Once the token has been returned by MSAL,
// add it to the http authorization header,
// before making the call to access the To Do list service.

httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);


        ...
...


- When the user is done managing their To-Do List, they may finally sign out of the app by clicking the "Clear Cache" button.

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
        // If the user clicked the 'clear cache' button,
        // clear the MSAL token cache and show the user as signed out.
        // It's also necessary to clear the cookies from the browser
        // control so the next user has a chance to sign in.

        if (SignInButton.Content.ToString() == "Clear Cache")
        {
                TodoList.ItemsSource = string.Empty;
                app.UserTokenCache.Clear(app.ClientId);
                ClearCookies();
                SignInButton.Content = "Sign In";
                return;
        }

        ...
```

## <a name="run"></a>运行

祝贺您 ！ 现在，您可以使用.NET WPF 应用程序能够对用户进行身份验证和安全地调用 Web Api 使用 OAuth 2.0。  运行这两个项目中，并使用 Microsoft 个人帐户，也可以是工作或学校的帐户登录。  将任务添加到该用户的待办事项列表。  注销，并重新登录以另一个用户以查看其待办事项列表。  关闭应用程序，然后重新运行它。  注意到该用户的会话将保持不变-这是因为该应用程序将缓存在本地文件中的标记。

MSAL 很容易将公共标识功能合并到您的应用程序，使用个人和工作帐户。  它会为您的高速缓存管理，OAuth 协议支持，为各用户提供登录用户界面，刷新过期的标记，以及其他负责所有差事。  您真正需要知道的只是一个 API 调用， `app.AcquireTokenAsync(...)`。

为了便于参考，[作为.zip 此处提供](https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet/archive/complete.zip)（无需您配置的值） 的完整的示例，或者您可以克隆它从 GitHub:

```git clone --branch complete https://github.com/AzureADQuickStarts/AppModelv2-NativeClient-DotNet.git```

## <a name="next-steps"></a>下一步行动

您现在可以移动到更高级的主题。  您可能想要尝试︰

- [保护 TodoListService Web API 使用 2.0 版的终结点](active-directory-v2-devquickstarts-dotnet-api.md)

更多的资源，请查阅︰  

- [2.0 版的开发人员指南 》 >>](active-directory-appmodel-v2-overview.md)
- [StackOverflow"msal"标记 >>](http://stackoverflow.com/questions/tagged/msal)

## <a name="get-security-updates-for-our-products"></a>我们的产品以获得安全更新

我们鼓励您能够访问[此页](https://technet.microsoft.com/security/dd252948)并订阅安全通报警告出现安全事件时的通知。
