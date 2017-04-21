<properties
    pageTitle="将身份验证添加到您的通用 Windows 平台 (UWP) 应用程序 |Azure 的移动应用程序"
    description="了解如何使用 Azure 应用程序服务移动应用程序使用不同的标识提供商，包括通用 Windows 平台 (UWP) 应用程序的用户身份验证︰ AAD、 Google、 Facebook、 Twitter，以及 Microsoft。"
    services="app-service\mobile"
    documentationCenter="windows"
    authors="adrianhall"
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="mobile-windows"
    ms.devlang="dotnet"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="adrianha"/>

# <a name="add-authentication-to-your-windows-app"></a>添加到您的 Windows 应用程序的身份验证

[AZURE.INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

本主题演示如何添加到您的移动应用程序的基于云的身份验证。 在本教程中，您的身份验证向项目中添加通用 Windows 平台 (UWP) 快速入门使用标识提供程序支持的 Azure 应用程序服务的移动应用程序的。 后被成功通过身份验证和授权的移动应用程序端，将显示用户 ID 值。

本教程基于移动应用程序快速入门。 您必须先完成本教程[开始使用移动应用程序](app-service-mobile-windows-store-dotnet-get-started.md)。

##<a name="register"></a>注册您的应用程序进行身份验证和配置应用程序服务

[AZURE.INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

##<a name="permissions"></a>限制对已验证身份的用户的权限

[AZURE.INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

现在，您可以验证已禁用匿名访问您的后端。 UWP 应用程序项目设置为启动项目，部署和运行应用程序;验证应用程序启动后引发未处理的异常状态代码 401 （未经授权）。 这是因为该应用程序试图访问您的移动应用程序代码作为未经身份验证的用户，但*TodoItem*表现在要求进行身份验证。

接下来，您将更新应用程序验证用户身份之前从您的应用程序服务请求的资源。

##<a name="add-authentication"></a>向应用程序添加验证

1. UWP 在应用程序项目文件 MainPage.cs，并将下面的代码段添加到主页类︰
    
        // Define a member variable for storing the signed-in user. 
        private MobileServiceUser user;

        // Define a method that performs the authentication process
        // using a Facebook sign-in. 
        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;
            try
            {
                // Change 'MobileService' to the name of your MobileServiceClient instance.
                // Sign-in using Facebook authentication.
                user = await App.MobileService
                    .LoginAsync(MobileServiceAuthenticationProvider.Facebook);
                message =
                    string.Format("You are now signed in - {0}", user.UserId);

                success = true;
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }

            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
            return success;
        }

    此代码验证与 Facebook 登录用户的身份。 如果您使用的 Facebook 不标识提供程序，您的提供程序的值更改上面的**MobileServiceAuthenticationProvider**的值。

3. 注释出或删除现有的**OnNavigatedTo**方法重写中的**ButtonRefresh_Click**方法 （或**InitLocalStoreAsync**方法） 的调用。 这可防止用户进行身份验证之前加载数据。 接下来，您将添加一个**登录**按钮到触发身份验证的应用程序。

4. 将下面的代码段添加到主页类︰

        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login the user and then load data from the mobile app.
            if (await AuthenticateAsync())
            {
                // Switch the buttons and load items from the mobile app.
                ButtonLogin.Visibility = Visibility.Collapsed;
                ButtonSave.Visibility = Visibility.Visible;
                //await InitLocalStoreAsync(); //offline sync support.
                await RefreshTodoItems();
            }
        }
        
5. 打开 MainPage.xaml 项目文件、 查找的元素的定义的**保存**按钮并将其替换为以下代码︰

        <Button Name="ButtonSave" Visibility="Collapsed" Margin="0,8,8,0" 
                Click="ButtonSave_Click">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Add"/>
                <TextBlock Margin="5">Save</TextBlock>
            </StackPanel>
        </Button>
        <Button Name="ButtonLogin" Visibility="Visible" Margin="0,8,8,0" 
                Click="ButtonLogin_Click" TabIndex="0">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Permissions"/>
                <TextBlock Margin="5">Sign in</TextBlock> 
            </StackPanel>
        </Button>

9. 按 F5 键运行应用程序，单击**登录**按钮，您选定的标识提供商登录到该应用程序。 您登录成功后，就无错误运行应用程序，您可以查询您的后端，并对数据进行更新。


##<a name="tokens"></a>在客户端上存储的身份验证令牌

前面的示例显示一个标准登录，这需要客户端应用程序启动每次联系身份标识提供程序和应用程序服务。 不只是此方法效率很低，可以运行到使用率的相关问题应许多客户尝试启动您的应用程序在同一时间。 更好的方法是缓存返回您的应用程序服务的授权标记，然后尝试使用基于提供程序的登录之前，使用此第一。

>[AZURE.NOTE]无论您是否使用客户端管理或托管服务的身份验证应用程序服务所颁发的令牌可以缓存。 本教程使用托管服务的身份验证。

[AZURE.INCLUDE [mobile-windows-universal-dotnet-authenticate-app-with-token](../../includes/mobile-windows-universal-dotnet-authenticate-app-with-token.md)]

##<a name="next-steps"></a>下一步行动

现在，您已完成本教程中基本身份验证，请考虑继续下面的教程之一︰

+ [将推式通知添加到您的应用程序](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  了解如何添加的推送通知到您的应用程序支持和配置您的移动应用程序后端使用 Azure 通知集线器发送推式通知。

+ [使您的应用程序的脱机同步](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  了解如何添加您使用移动应用程序的后端的应用程序的脱机支持。 脱机同步允许最终用户与移动应用程序交互&mdash;查看、 添加或修改数据&mdash;即使没有网络连接。


<!-- URLs. -->
[Get started with your mobile app]: app-service-mobile-windows-store-dotnet-get-started.md

