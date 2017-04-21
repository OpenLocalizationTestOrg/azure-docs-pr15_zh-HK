<properties
    pageTitle="开始使用在 Xamarin Android 的移动应用程序的身份验证"
    description="了解如何使用移动应用程序可通过多种标识提供程序，包括 AAD、 Google、 Facebook、 Twitter，以及 Microsoft Xamarin Android 应用程序的用户进行身份验证。"
    services="app-service\mobile"
    documentationCenter="xamarin"
    authors="adrianhall"
    manager="dwrede"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-xamarin-android"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-xamarinandroid-app"></a>添加到您的 Xamarin.Android 应用程序的身份验证

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

本主题演示如何从客户端应用程序的移动应用程序的用户进行身份验证。 在本教程中，您添加到快速入门项目使用标识提供程序支持的 Azure 移动应用程序身份验证。 后被成功通过身份验证和授权移动应用程序中，将显示用户 ID 值。

本教程基于移动应用程序快速入门。 此外第一次必须完成本教程[创建一个 Xamarin.Android 应用程序]。 如果不使用下载快速入门服务器项目，您必须向项目添加身份验证扩展包。 有关服务器扩展软件包的详细信息，请参阅[使用.NET 后端服务器用于 Azure 的移动应用程序的 SDK](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md)。

##<a name="register"></a>注册您的应用程序进行身份验证和配置应用程序服务

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>限制对已验证身份的用户的权限

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

在 Visual Studio 或 Xamarin Studio 中，运行在设备或仿真程序上的客户端项目。 验证应用程序启动后引发未处理的异常状态代码 401 （未经授权）。 这是因为该应用程序试图访问您作为未经身份验证的用户的移动应用程序后端。 *TodoItem*表现在要求进行身份验证。

接下来，您将更新客户端应用程序请求资源从移动应用程序的后端与身份验证的用户。

##<a name="add-authentication"></a>向应用程序添加验证

应用程序更新要求用户点击**登录**按钮，并在显示数据之前进行身份验证。

1. 将以下代码添加到**TodoActivity**类︰

        // Define a authenticated user.
        private MobileServiceUser user;
        private async Task<bool> Authenticate()
        {
                var success = false;
                try
                {
                    // Sign in with Facebook login using a server-managed flow.
                    user = await client.LoginAsync(this,
                        MobileServiceAuthenticationProvider.Facebook);
                    CreateAndShowDialog(string.Format("you are now logged in - {0}",
                        user.UserId), "Logged in!");

                    success = true;
                }
                catch (Exception ex)
                {
                    CreateAndShowDialog(ex, "Authentication failed");
                }
                return success;
        }

        [Java.Interop.Export()]
        public async void LoginUser(View view)
        {
            // Load data only after authentication succeeds.
            if (await Authenticate())
            {
                //Hide the button after authentication succeeds.
                FindViewById<Button>(Resource.Id.buttonLoginUser).Visibility = ViewStates.Gone;

                // Load the data.
                OnRefreshItemsSelected();
            }
        }

    这将创建一个新方法来进行身份验证的用户和一个新的**登录**按钮的方法处理。 上面的示例代码中的用户是通过使用 Facebook 的登录身份验证。 对话框用于显示一次经过身份验证的用户 ID。

    > [AZURE.NOTE] 如果您正在使用 Facebook 不标识提供程序，更改传递给**LoginAsync**的上面到下面的一个值︰ _MicrosoftAccount_，_使用 Twitter_， _Google_或_WindowsAzureActiveDirectory_。

3. 在**OnCreate**方法中，删除或注释出下面这行代码︰

        OnRefreshItemsSelected ();

4. 在 Activity_To_Do.axml 文件中，添加下面的*LoginUser*按钮定义现有*AddItem*按钮前︰

        <Button
            android:id="@+id/buttonLoginUser"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="LoginUser"
            android:text="@string/login_button_text" />

5. 将以下元素添加到 Strings.xml 的资源文件︰

        <string name="login_button_text">Sign in</string>

6. 在 Visual Studio 或 Xamarin Studio，在设备或仿真程序上运行的客户端项目并登录到您选定的标识提供程序。

    成功登录后，应用程序将显示您的登录 ID 和托多项的列表，您可以对数据进行更新。


<!-- URLs. -->
[创建一个 Xamarin.Android 应用程序]: app-service-mobile-xamarin-android-get-started.md
