<properties
    pageTitle="Azure 的 Active Directory B2C |Microsoft Azure"
    description="如何构建 Windows 桌面应用程序，其中包括登录、 注册、 并使用 Azure 活动目录 B2C 的配置文件管理。"
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

# <a name="azure-ad-b2c-build-a-windows-desktop-app"></a>Azure AD B2C︰ 生成 Windows 桌面应用程序

通过使用 Azure 活动目录 (AD Azure) B2C，您可以添加强大的自助服务身份标识管理功能于桌面应用程序中简单的几步。 这篇文章将说明如何创建.NET Windows Presentation Foundation (WPF) 的"待办事项列表"应用程序，包括注册、 登录、 用户和配置文件管理。 应用程序将包括支持注册和登录使用的用户名或电子邮件。 它还将通过社会如 Facebook 和 Google 帐户包括支持注册和登录。

## <a name="get-an-azure-ad-b2c-directory"></a>获取 Azure AD B2C 目录

您可以使用 Azure AD B2C 之前，必须创建一个目录，或租户。  目录是您的用户、 应用程序、 组和更多的所有的容器。 如果您还没有，然后再[创建一个 B2C 的目录](active-directory-b2c-get-started.md)本指南在继续进行。

## <a name="create-an-application"></a>创建应用程序

接下来，您需要在 B2C 目录中创建一个应用程序。 这样，它需要与您的应用程序安全地进行通信的 Azure 的广告信息。 若要创建一个应用程序，请按照[以下说明操作](active-directory-b2c-app-registration.md)。  请务必︰

- 包含**本机客户端**应用程序中。
- 复制**重定向 URI** `urn:ietf:wg:oauth:2.0:oob`。 它是该代码示例的默认 URL。
- 复制**应用程序 ID**分配给您的应用程序。 您将稍后需要它。

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## <a name="create-your-policies"></a>创建您的策略

在 Azure AD B2C，[策略](active-directory-b2c-reference-policies.md)定义了每个用户体验。 此代码示例包含三个标识体验︰ 注册，登录，并编辑配置文件。 您需要为每种类型，创建一个策略[策略参考文章](active-directory-b2c-reference-policies.md#how-to-create-a-sign-up-policy)中所述。 创建三个策略时，请务必︰

- 选择标识提供程序刀片式服务器中**注册的用户 ID**或**电子邮件注册**。
- 在您注册策略中选择**显示名称**和注册的其他属性。
- 为每个策略的应用程序声明选择**显示名称**和**对象 ID**的索赔。 您可以选择以及其他索赔。
- 您在创建后，将复制每个策略的**名称**。 它应具有前缀`b2c_1_`。  稍后，您将需要这些策略名称。

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-policy](../../includes/active-directory-b2c-devquickstarts-policy.md)]

您已成功创建的三个策略后，就可以生成您的应用程序。

## <a name="download-the-code"></a>下载的代码

[在 GitHub 上维护](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet)此教程的代码。 当您生成示例，您可以[下载的主干项目为.zip 文件](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/skeleton.zip)。 您还可以复制骨架︰

```
git clone --branch skeleton https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git
```

已完成应用程序同时也是[一个.zip 文件](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip)或在`complete`的同一存储库中的分支。

下载示例代码之后，打开 Visual Studio 的.sln 文件，即可开始。 `TaskClient`项目是 WPF 台式机应用程序与用户进行交互。 对于本教程，它调用后端任务 web API，承载 Azure，用于存储每个用户的待办事项列表中。  不需要生成 web API，我们已经为您运行它。

若要了解如何 web API 安全地进行身份验证的请求使用 Azure AD B2C，签出[web API 入门文章](active-directory-b2c-devquickstarts-api-dotnet.md)。

## <a name="execute-policies"></a>执行策略
您的应用程序与 Azure AD B2C 通过发送指定他们想要作为 HTTP 请求的一部分执行的策略的身份验证消息来进行通信。 对于.NET 桌面应用程序，可以使用预览 Microsoft 身份验证库 (MSAL) 来发送 OAuth 2.0 身份验证消息中，执行策略，并得到调用 web Api 的标记。

### <a name="install-msal"></a>安装 MSAL
添加到 MSAL`TaskClient`使用 Visual Studio 程序包管理器控制台项目。

```
PM> Install-Package Microsoft.Identity.Client -IncludePrerelease
```

### <a name="enter-your-b2c-details"></a>输入您的 B2C 详细信息
打开的文件`Globals.cs`和每个属性值替换为您自己。 此类用于在整个`TaskClient`为常用的参考值。

```C#
public static class Globals
{
    ...

    // TODO: Replace these five default with your own configuration values
    public static string tenant = "fabrikamb2c.onmicrosoft.com";
    public static string clientId = "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6";
    public static string signInPolicy = "b2c_1_sign_in";
    public static string signUpPolicy = "b2c_1_sign_up";
    public static string editProfilePolicy = "b2c_1_edit_profile";

    ...
}
```

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-tenant-name](../../includes/active-directory-b2c-devquickstarts-tenant-name.md)]


### <a name="create-the-publicclientapplication"></a>创建 PublicClientApplication
MSAL 的主要类为`PublicClientApplication`。 此类表示 Azure AD B2C 系统中的应用程序。 应用程序用中，当创建的实例`PublicClientApplication`在`MainWindow.xaml.cs`。 这可在整个窗口。

```C#
protected async override void OnInitialized(EventArgs e)
{
    base.OnInitialized(e);

    pca = new PublicClientApplication(Globals.clientId)
    {
        // MSAL implements an in-memory cache by default.  Since we want tokens to persist when the user closes the app, 
        // we've extended the MSAL TokenCache and created a simple FileCache in this app.
        UserTokenCache = new FileCache(),
    };
    
    ...
```

### <a name="initiate-a-sign-up-flow"></a>启动注册流程
当用户选择到符号上时，要启动使用您创建的注册策略的注册流程。 通过使用 MSAL，您只需调用`pca.AcquireTokenAsync(...)`。 参数将传递给`AcquireTokenAsync(...)`确定哪些标记您收到，身份验证请求，等中所使用的策略。

```C#
private async void SignUp(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        // Use the app's clientId here as the scope parameter, indicating that
        // you want a token to the your app's backend web API (represented by
        // the cloud hosted task API).  Use the UiOptions.ForceLogin flag to
        // indicate to MSAL that it should show a sign-up UI no matter what.
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                Globals.signUpPolicy);

        // Upon success, indicate in the app that the user is signed in.
        SignInButton.Visibility = Visibility.Collapsed;
        SignUpButton.Visibility = Visibility.Collapsed;
        EditProfileButton.Visibility = Visibility.Visible;
        SignOutButton.Visibility = Visibility.Visible;

        // When the request completes successfully, you can get user 
        // information from the AuthenticationResult
        UsernameLabel.Content = result.User.Name;

        // After the sign up successfully completes, display the user's To-Do List
        GetTodoList();
    }

    // Handle any exeptions that occurred during execution of the policy.
    catch (MsalException ex)
    {
        if (ex.ErrorCode != "authentication_canceled")
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

### <a name="initiate-a-sign-in-flow"></a>启动签入流
您可以以相同的方式启动注册流程启动签入流。 当用户登录时，请相同调用 MSAL，这次使用您登录的策略︰

```C#
private async void SignIn(object sender = null, RoutedEventArgs args = null)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.signInPolicy);
        ...
```

### <a name="initiate-an-edit-profile-flow"></a>启动编辑档案文件流
同样，可以以相同的方式执行编辑配置文件策略︰

```C#
private async void EditProfile(object sender, RoutedEventArgs e)
{
    AuthenticationResult result = null;
    try
    {
        result = await pca.AcquireTokenAsync(new string[] { Globals.clientId },
                    string.Empty, UiOptions.ForceLogin, null, null, Globals.authority,
                    Globals.editProfilePolicy);
```

在所有这些情况下，MSAL 返回中的标记`AuthenticationResult`或引发异常。 每次您获得令牌从 MSAL，您可以使用`AuthenticationResult.User`对象来更新应用程序，例如，用户界面中的用户数据。 ADAL 还会缓存在应用程序其他部分中使用的标记。


### <a name="check-for-tokens-on-app-start"></a>检查标记的应用程序启动
您还可以使用 MSAL 来跟踪用户的登录状态。  在此应用程序，我们希望用户保持登录，即使它们关闭应用程序并重新打开它。  返回内部`OnInitialized`重写，请使用 MSAL 的`AcquireTokenSilent`方法检查缓存标记︰

```C#
AuthenticationResult result = null;
try
{
    // If the user has has a token cached with any policy, we'll display them as signed-in.
    TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
    string existingPolicy = tci == null ? null : tci.Policy;
    result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    SignInButton.Visibility = Visibility.Collapsed;
    SignUpButton.Visibility = Visibility.Collapsed;
    EditProfileButton.Visibility = Visibility.Visible;
    SignOutButton.Visibility = Visibility.Visible;
    UsernameLabel.Content = result.User.Name;
    GetTodoList();
}
catch (MsalException ex)
{
    if (ex.ErrorCode == "failed_to_acquire_token_silently")
    {
        // There are no tokens in the cache.  Proceed without calling the To Do list service.
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
```

## <a name="call-the-task-api"></a>调用 API 的任务
您现在已经使用 MSAL 执行策略并获取令牌。  当您想要使用这些标记来调用 API 的任务时，您可以再次使用 MSAL 的`AcquireTokenSilent`方法检查缓存标记︰

```C#
private async void GetTodoList()
{
    AuthenticationResult result = null;
    try
    {
        // Here we want to check for a cached token, independent of whatever policy was used to acquire it.
        TokenCacheItem tci = pca.UserTokenCache.ReadItems(Globals.clientId).Where(i => i.Scope.Contains(Globals.clientId) && !string.IsNullOrEmpty(i.Policy)).FirstOrDefault();
        string existingPolicy = tci == null ? null : tci.Policy;

        // Use AcquireTokenSilent to indicate that MSAL should throw an exception if a token cannot be acquired
        result = await pca.AcquireTokenSilentAsync(new string[] { Globals.clientId }, string.Empty, Globals.authority, existingPolicy, false);

    }
    // If a token could not be acquired silently, we'll catch the exception and show the user a message.
    catch (MsalException ex)
    {
        // There is no access token in the cache, so prompt the user to sign-in.
        if (ex.ErrorCode == "failed_to_acquire_token_silently")
        {
            MessageBox.Show("Please sign up or sign in first");
            SignInButton.Visibility = Visibility.Visible;
            SignUpButton.Visibility = Visibility.Visible;
            EditProfileButton.Visibility = Visibility.Collapsed;
            SignOutButton.Visibility = Visibility.Collapsed;
            UsernameLabel.Content = string.Empty;
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
    ...
```

当调用`AcquireTokenSilentAsync(...)`成功并在缓存中找到一个标记，您可以添加到标记`Authorization`在 HTTP 请求标头。 任务 web API 将使用此标头验证请求读取用户的待办事项列表︰

```C#
    ...
    // Once the token has been returned by MSAL, add it to the http authorization header, before making the call to access the To Do list service.
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", result.Token);

    // Call the To Do list service.
    HttpResponseMessage response = await httpClient.GetAsync(Globals.taskServiceUrl + "/api/tasks");
    ...
```

## <a name="sign-the-user-out"></a>注销用户
最后，可以使用 MSAL 来结束与应用程序的用户的会话，当用户选择**退出**。  在使用 MSAL 时，这可以通过清除所有从令牌缓存标记︰

```C#
private void SignOut(object sender, RoutedEventArgs e)
{
    // Clear any remnants of the user's session.
    pca.UserTokenCache.Clear(Globals.clientId);

    // This is a helper method that clears browser cookies in the browser control that MSAL uses, it is not part of MSAL.
    ClearCookies();

    // Update the UI to show the user as signed out.
    TaskList.ItemsSource = string.Empty;
    SignInButton.Visibility = Visibility.Visible;
    SignUpButton.Visibility = Visibility.Visible;
    EditProfileButton.Visibility = Visibility.Collapsed;
    SignOutButton.Visibility = Visibility.Collapsed;
    return;
}
```

## <a name="run-the-sample-app"></a>运行示例应用程序

最后，生成并运行该示例。  注册应用程序通过使用电子邮件地址或用户名称。 注销并重新登录用户相同的用户。 编辑该用户的配置文件。 注销并使用不同的用户注册。

## <a name="add-social-idps"></a>添加社交 IDPs

目前，该应用程序支持仅用户注册和登录使用**本地帐户**的。 这些都是使用用户名和密码的帐户存储在 B2C 目录。 通过使用 Azure AD B2C，可以添加对其他身份提供程序 (IDPs) 的支持，而无需更改任何代码。

若要将社会 IDPs 添加到您的应用程序，开始在这些文章中的详细说明。 对于每个您想要支持的 IDP，您需要在该系统中注册应用程序并获取客户端 id。

- [将设置 Facebook 为 IDP](active-directory-b2c-setup-fb-app.md)
- [将设置 Google 为 IDP](active-directory-b2c-setup-goog-app.md)
- [设置为 IDP Amazon](active-directory-b2c-setup-amzn-app.md)
- [设置为 IDP LinkedIn](active-directory-b2c-setup-li-app.md)

标识提供程序添加到 B2C 目录后，您需要编辑每个三个策略来包括新 IDPs，[策略参考文章](active-directory-b2c-reference-policies.md)中所述。 在保存策略后，请再次运行该应用程序。 您应该可以看到新的 IDPs 添加为登录和注册的选项，在您的标识的每个经验。

可以尝试使用您的策略并观察对您的示例应用程序的影响。 添加或删除 IDPs、 操纵应用程序声明或更改注册属性。 进行试验，直到您可以看到如何策略、 身份验证请求，以及 MSAL 绑定在一起。

为了便于参考，[以.zip 文件格式提供](https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet/archive/complete.zip)完整的示例。 您还可以从 GitHub 克隆它︰

```git clone --branch complete https://github.com/AzureADQuickStarts/B2C-NativeClient-DotNet.git```
