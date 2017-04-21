<properties
    pageTitle="开始使用 Xamarin iOS 中的移动应用程序的身份验证"
    description="了解如何使用移动应用程序可通过多种标识提供程序，包括 AAD、 Google、 Facebook、 Twitter，以及 Microsoft Xamarin iOS 应用程序的用户进行身份验证。"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="na"
    ms.tgt_pltfrm="mobile-xamarin-ios"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-xamarinios-app"></a>添加到您的 Xamarin.iOS 应用程序的身份验证

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

本主题演示如何从客户端应用程序的应用程序服务移动应用程序的用户进行身份验证。 在本教程中，您将添加身份验证到 Xamarin.iOS 快速入门项目使用标识提供程序所支持的应用程序服务。 被成功身份验证和授权由您的移动应用程序后，将显示用户 ID 值，您将能够访问受限制的表数据。

您必须先完成本教程[创建一个 Xamarin.iOS 应用程序]。 如果不使用下载快速入门服务器项目，您必须向项目添加身份验证扩展包。 有关服务器扩展软件包的详细信息，请参阅[使用.NET 后端服务器用于 Azure 的移动应用程序的 SDK](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)。

##<a name="register-your-app-for-authentication-and-configure-app-services"></a>注册您的应用程序进行身份验证和配置应用程序服务

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="restrict-permissions-to-authenticated-users"></a>限制对已验证身份的用户的权限

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

&nbsp;&nbsp;4.在 Visual Studio 或 Xamarin Studio，运行在设备或仿真程序上的客户端项目。 验证应用程序启动后引发未处理的异常状态代码 401 （未经授权）。 到控制台的调试程序，则记录该故障。 因此在 Visual Studio 中，您应该看到输出窗口中的失败。

&nbsp;&nbsp;这种未经授权的错误是因为该应用程序试图访问您作为未经身份验证的用户的移动应用程序后端。 *TodoItem*表现在要求进行身份验证。

接下来，您将更新客户端应用程序请求资源从移动应用程序的后端与身份验证的用户。

##<a name="add-authentication-to-the-app"></a>向应用程序添加验证

在本节中，您将修改该应用程序在显示数据之前显示登录屏幕。 当应用程序启动时，它将不连接到您的应用程序的服务并不会显示任何数据。 第一次，之后用户执行刷新势，将出现登录屏幕;成功登录后将显示的 todo 项列表。

1. 在客户端项目中，打开**QSTodoService.cs**文件，然后添加以下 using 语句和`MobileServiceUser`与 QSTodoService 类的访问器︰

    ```
        using UIKit;
    ```

        // Logged in user
        private MobileServiceUser user;
        public MobileServiceUser User { get { return user; } }

2. 添加新的方法以下面的定义命名为**QSTodoService**的**身份验证**︰


        public async Task Authenticate(UIViewController view)
        {
            try
            {
                user = await client.LoginAsync(view, MobileServiceAuthenticationProvider.Facebook);
            }
            catch (Exception ex)
            {
                Console.Error.WriteLine (@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
            }
        }

    >[AZURE.NOTE] 如果您使用的标识提供程序以外的 Facebook，更改传递给**LoginAsync**的上面到下面的一个值︰ _MicrosoftAccount_，_使用 Twitter_， _Google_或_WindowsAzureActiveDirectory_。

3. 打开**QSTodoListViewController.cs**。 修改删除**RefreshAsync()**附近结束调用**ViewDidLoad**方法定义︰

        public override async void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            todoService = QSTodoService.DefaultService;
           await todoService.InitializeStoreAsync ();

           RefreshControl.ValueChanged += async (sender, e) => {
                await RefreshAsync ();
           }

            // Comment out the call to RefreshAsync
            // await RefreshAsync ();
        }


4. 修改方法**RefreshAsync**进行身份验证，如果**用户**属性为 null。 方法定义的顶部添加以下代码︰

        // start of RefreshAsync method
        if (todoService.User == null) {
            await QSTodoService.DefaultService.Authenticate (this);
            if (todoService.User == null) {
                Console.WriteLine ("couldn't login!!");
                return;
            }
        }
        // rest of RefreshAsync method

5. 在 Visual Studio 或 Xamarin Studio 中连接到 Xamarin 生成主机在您运行目标设备或仿真程序客户端项目的 Mac 上。 验证该应用程序显示任何数据。

    拉出将导致登录屏幕显示的项的列表中向下执行刷新笔势。 一旦您已成功输入有效的凭据，应用程序将显示的 todo 项列表，您可以对数据进行更新。


<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[创建一个 Xamarin.iOS 应用程序]: app-service-mobile-xamarin-ios-get-started.md
