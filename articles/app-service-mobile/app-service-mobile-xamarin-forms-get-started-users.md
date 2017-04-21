<properties
    pageTitle="开始使用 Xamarin.Forms 应用程序中的移动应用程序的身份验证 |Microsoft Azure"
    description="了解如何使用移动应用程序可通过多种标识提供程序，包括 AAD、 Google、 Facebook、 Twitter，以及 Microsoft Xamarin 窗体应用程序的用户进行身份验证。"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-xamarinforms-app"></a>添加到您的 Xamarin.Forms 应用程序的身份验证

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

##<a name="overview"></a>概述

本主题演示如何从客户端应用程序的应用程序服务移动应用程序的用户进行身份验证。 在本教程中，您将添加身份验证到 Xamarin.Forms 快速入门项目使用标识提供程序所支持的应用程序服务。 被成功身份验证和授权由您的移动应用程序后，将显示用户 ID 值，并且您将能够访问受限制的表数据。

##<a name="prerequisites"></a>系统必备组件

本教程的最佳结果，建议您先完成[创建 Xamarin.Forms 应用程序](app-service-mobile-xamarin-forms-get-started.md)的教程。 完成本教程后，您将有多平台任务列表应用程序的 Xamarin.Forms 项目。

如果不使用下载快速入门服务器项目，您必须向项目添加身份验证扩展包。 有关服务器扩展软件包的详细信息，请参阅[使用.NET 后端服务器用于 Azure 的移动应用程序的 SDK](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)。

##<a name="register-your-app-for-authentication-and-configure-app-services"></a>注册您的应用程序进行身份验证和配置应用程序服务

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="restrict-permissions-to-authenticated-users"></a>限制对已验证身份的用户的权限

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]


##<a name="add-authentication-to-the-portable-class-library"></a>为可移植类库添加身份验证

移动应用程序在[MobileServiceClient]上使用[LoginAsync]扩展方法来登录应用程序服务的身份验证的用户。 此示例使用应用程序中显示该提供程序的登录界面管理服务器的身份验证流程。 有关详细信息，请参阅[管理服务器的身份验证](app-service-mobile-dotnet-how-to-use-client-library.md#serverflow)。 为了提供更好的用户体验，生产应用程序中，您可以考虑改为使用[客户端管理身份验证](app-service-mobile-dotnet-how-to-use-client-library.md#clientflow)。 

进行身份验证的 Xamarin.Forms 项目，应用程序可移植类库中定义的**IAuthenticate**接口。 您还可以更新添加的**登录**按钮，用户单击后可进行身份验证可移植类库中定义的用户界面。 在身份验证成功之后, 从移动应用程序的后端加载数据。

必须实现由您的应用程序支持的每个平台的**IAuthenticate**接口。


1. 在 Visual Studio 或 Xamarin Studio，打开程序从项目与**笔记本电脑**中的名称，是可移植类库项目，然后添加以下`using`语句︰

        using System.Threading.Tasks;

2. 在程序中，添加以下`IAuthenticate`接口定义之前立即`App`类定义。

        public interface IAuthenticate
        {
            Task<bool> Authenticate();
        }

3. 要初始化使用平台特定的实现的接口的**应用程序**类中添加下面的静态成员。

        public static IAuthenticate Authenticator { get; private set; }

        public static void Init(IAuthenticate authenticator)
        {
            Authenticator = authenticator;
        }

4. 可移植类库项目中打开 TodoList.xaml，现有按钮后在*buttonsPanel*布局元素中，添加下面的**按钮**元素︰ 

        <Button x:Name="loginButton" Text="Sign-in" MinimumHeightRequest="30" 
            Clicked="loginButton_Clicked"/>

    此按钮触发与您移动应用程序的后端管理服务器的身份验证。

5. 从可移植类库项目中打开 TodoList.xaml.cs，然后添加到下面的字段`TodoList`类︰

        // Track whether the user has authenticated. 
        bool authenticated = false;


6. **OnAppearing**方法替换为以下代码︰

        protected override async void OnAppearing()
        {
            base.OnAppearing();
    
            // Refresh items only when authenticated.
            if (authenticated == true)
            {
                // Set syncItems to true in order to synchronize the data 
                // on startup when running in offline mode.
                await RefreshItems(true, syncItems: false);

                // Hide the Sign-in button.
                this.loginButton.IsVisible = false;
            }
        }

    这样可确保用户已通过身份验证后，数据仅刷新服务。

7. 为**任务列表**类中添加以下事件处理程序**Clicked** :

        async void loginButton_Clicked(object sender, EventArgs e)
        {
            if (App.Authenticator != null)
                authenticated = await App.Authenticator.Authenticate();

            // Set syncItems to true to synchronize the data on startup when offline is enabled.
            if (authenticated == true)
                await RefreshItems(true, syncItems: false);
        }

8. 保存更改并重新生成可移植类库项目验证没有错误。


##<a name="add-authentication-to-the-android-app"></a>添加到 Android 应用程序的身份验证

本节说明如何在 Android 应用程序项目中实现**IAuthenticate**接口。 如果您在不支持 Android 设备，请跳过此部分。

1. 在 Visual Studio 或 Xamarin Studio 中，用鼠标右键单击**droid**项目，然后**将设置为启动项目**。

2. 按 F5 键在调试器中启动项目，然后验证应用程序启动后引发未处理的异常状态代码 401 （未经授权）。 这是因为后端访问被限制为只允许授权用户。

3. 在 Android 项目中打开 MainActivity.cs，然后添加以下`using`语句︰

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;

4. 更新的**MainActivity**类来实现**IAuthenticate**接口，如下所示︰

        public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity, IAuthenticate


5. 更新**MainActivity**类添加一个**MobileServiceUser**字段和**身份验证**方法中，需要通过**IAuthenticate**接口，如下所示︰

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            var success = false;
            var message = string.Empty;
            try
            {
                // Sign in with Facebook login using a server-managed flow.
                user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(this,
                    MobileServiceAuthenticationProvider.Facebook);
                if (user != null)
                {
                    message = string.Format("you are now signed-in as {0}.",
                        user.UserId);
                    success = true;
                }
            }
            catch (Exception ex)
            {
                message = ex.Message;
            }

            // Display the success or failure message.
            AlertDialog.Builder builder = new AlertDialog.Builder(this);
            builder.SetMessage(message);
            builder.SetTitle("Sign-in result");
            builder.Create().Show();

            return success;
        }


    如果您使用的标识提供程序以外的其他 Facebook， [MobileServiceAuthenticationProvider]选择一个不同的值。

6. 将以下代码添加到**OnCreate**方法调用之前的**MainActivity**类`LoadApplication()`:

        // Initialize the authenticator before loading the app.
        App.Init((IAuthenticate)this);

    这将确保身份验证器被初始化之前应用程序负载。

7. 重新生成应用程序，运行它，然后登录身份验证提供程序的选择和验证您能够访问作为经过身份验证的用户的数据。

##<a name="add-authentication-to-the-ios-app"></a>添加到 iOS 应用程序的身份验证

本节说明如何在 iOS 应用程序项目中实现**IAuthenticate**接口。 如果您在不支持 iOS 设备，请跳过此部分。

1. 在 Visual Studio 或 Xamarin Studio 中，用鼠标右键单击**iOS**的项目，然后**设为启动项目**。

2. 按 F5 键在调试器中启动项目，然后验证应用程序启动后引发未处理的异常状态代码 401 （未经授权）。 这是因为后端访问被限制为只允许授权用户。

4. 在 iOS 项目中打开 AppDelegate.cs，然后添加以下`using`语句︰

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;

4. 更新的**AppDelegate**类来实现**IAuthenticate**接口，如下所示︰

        public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate, IAuthenticate

5. 更新**AppDelegate**类添加一个**MobileServiceUser**字段和**身份验证**方法中，需要通过**IAuthenticate**接口，如下所示︰

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            var success = false;
            var message = string.Empty;
            try
            {
                // Sign in with Facebook login using a server-managed flow.
                if (user == null)
                {
                    user = await TodoItemManager.DefaultManager.CurrentClient
                        .LoginAsync(UIApplication.SharedApplication.KeyWindow.RootViewController,
                        MobileServiceAuthenticationProvider.Facebook);
                    if (user != null)
                    {
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
                        success = true;                        
                    }
                }        
            }
            catch (Exception ex)
            {
               message = ex.Message;
            }

            // Display the success or failure message.
            UIAlertView avAlert = new UIAlertView("Sign-in result", message, null, "OK", null);
            avAlert.Show();         

            return success;
        }

    如果您使用的标识提供程序以外的其他 Facebook， [MobileServiceAuthenticationProvider]选择一个不同的值。

6. 将以下代码行添加到调用之前的**FinishedLaunching**方法`LoadApplication()`: 

        App.Init(this);

    这将确保在加载应用程序之前，初始化身份验证器。

7. 重新生成应用程序，运行它，然后登录身份验证提供程序的选择和验证您能够访问作为经过身份验证的用户的数据。


##<a name="add-authentication-to-windows-app-projects"></a>将身份验证添加到 Windows 应用程序项目

这一节演示如何在 Windows 8.1 和 Windows Phone 8.1 应用程序项目中实现**IAuthenticate**接口。 相同的步骤也适用于通用 Windows 平台 (UWP) 项目。 如果您在不支持 Windows 设备，请跳过此部分。

1. 在 Visual Studio 中，用鼠标右键单击**WinApp**或**WinPhone81**的项目，然后**设为启动项目**。

2. 按 F5 键在调试器中启动项目，然后验证应用程序启动后引发未处理的异常状态代码 401 （未经授权）。 这是因为后端访问被限制为只允许授权用户。

3. 打开 MainPage.xaml.cs 为 Windows 应用程序项目，然后添加以下`using`语句︰

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.UI.Popups;
        using <your_Portable_Class_Library_namespace>;

    更换`<your_Portable_Class_Library_namespace>`与您可移植类库的命名空间。

4. 更新**主页**类实现**IAuthenticate**接口，如下所示︰

        public sealed partial class MainPage : IAuthenticate


5. 更新**主页**类添加一个**MobileServiceUser**字段和**身份验证**方法中，需要通过**IAuthenticate**接口，如下所示︰

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            string message = string.Empty;
            var success = false;

            try
            {
                // Sign in with Facebook login using a server-managed flow.
                if (user == null)
                {
                    user = await TodoItemManager.DefaultManager.CurrentClient
                        .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                    if (user != null)
                    {
                        success = true;
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
                    }
                }
                
            }
            catch (Exception ex)
            {
                message = string.Format("Authentication Failed: {0}", ex.Message);
            }

            // Display the success or failure message.
            await new MessageDialog(message, "Sign-in result").ShowAsync();

            return success;
        }


    如果您使用的标识提供程序以外的其他 Facebook， [MobileServiceAuthenticationProvider]选择一个不同的值。

6. 在调用之前的**主页**类的构造函数中添加以下代码行`LoadApplication()`:

        // Initialize the authenticator before loading the app.
        <your_Portable_Class_Library_namespace>.App.Init(this);
 
    更换`<your_Portable_Class_Library_namespace>`与您可移植类库的命名空间。  
    如果这是 WinApp 项目，您可以跳到步骤 8。 下一步只适用于 WinPhone81 项目中，您需要完成登录回调。

7. （可选）在**WinPhone81**应用程序项目中，打开 App.xaml.cs，然后添加以下`using`语句︰

        using Microsoft.WindowsAzure.MobileServices;
        using <your_Portable_Class_Library_namespace>;

    更换`<your_Portable_Class_Library_namespace>`与您可移植类库的命名空间。

8.  将下面的**OnActivated**方法重写添加到**应用程序**类︰

        protected override void OnActivated(IActivatedEventArgs args)
        {
            base.OnActivated(args);

            // We just need to handle activation that occurs after web authentication. 
            if (args.Kind == ActivationKind.WebAuthenticationBrokerContinuation)
            {
                // Get the client and call the LoginComplete method to complete authentication.
                var client = TodoItemManager.DefaultManager.CurrentClient as MobileServiceClient;
                client.LoginComplete(args as WebAuthenticationBrokerContinuationEventArgs);
            }
        }

    方法重写已经存在，只是从上面的代码段添加条件代码。

7. 重新生成应用程序，运行它，然后登录身份验证提供程序的选择和验证您能够访问作为经过身份验证的用户的数据。

##<a name="next-steps"></a>下一步行动

现在，您已完成本教程中基本身份验证，请考虑继续下面的教程之一︰

+ [将推式通知添加到您的应用程序](app-service-mobile-xamarin-forms-get-started-push.md)  
  了解如何添加的推送通知到您的应用程序支持和配置您的移动应用程序后端使用 Azure 通知集线器发送推式通知。

+ [使您的应用程序的脱机同步](app-service-mobile-xamarin-forms-get-started-offline-data.md)  
  了解如何添加您使用移动应用程序的后端的应用程序的脱机支持。 脱机同步允许最终用户与移动应用程序交互&mdash;查看、 添加或修改数据&mdash;即使没有网络连接。

<!-- Images. -->

<!-- URLs. -->
[apns object]: http://go.microsoft.com/fwlink/p/?LinkId=272333
[LoginAsync]: https://msdn.microsoft.com/library/azure/dn268341(v=azure.10).aspx
[MobileServiceClient]: https://msdn.microsoft.com/library/azure/JJ553674(v=azure.10).aspx
[MobileServiceAuthenticationProvider]: https://msdn.microsoft.com/library/azure/jj730936(v=azure.10).aspx

