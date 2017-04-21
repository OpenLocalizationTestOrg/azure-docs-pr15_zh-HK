
1. 在 MainPage.xaml.cs 项目文件中添加下面的**using**语句︰

        using System.Linq;      
        using Windows.Security.Credentials;

2. **AuthenticateAsync**方法替换为以下代码︰

        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;

            // This sample uses the Facebook provider.
            var provider = MobileServiceAuthenticationProvider.Facebook;

            // Use the PasswordVault to securely store and access credentials.
            PasswordVault vault = new PasswordVault();
            PasswordCredential credential = null;

            try
            {
                // Try to get an existing credential from the vault.
                credential = vault.FindAllByResource(provider.ToString()).FirstOrDefault();
            }
            catch (Exception)
            {
                // When there is no matching resource an error occurs, which we ignore.
            }

            if (credential != null)
            {
                // Create a user from the stored credentials.
                user = new MobileServiceUser(credential.UserName);
                credential.RetrievePassword();
                user.MobileServiceAuthenticationToken = credential.Password;

                // Set the user from the stored credentials.
                App.MobileService.CurrentUser = user;

                // Consider adding a check to determine if the token is 
                // expired, as shown in this post: http://aka.ms/jww5vp.

                success = true;
                message = string.Format("Cached credentials for user - {0}", user.UserId);
            }
            else
            {
                try
                {
                    // Login with the identity provider.
                    user = await App.MobileService
                        .LoginAsync(provider);

                    // Create and store the user credentials.
                    credential = new PasswordCredential(provider.ToString(),
                        user.UserId, user.MobileServiceAuthenticationToken);
                    vault.Add(credential);

                    success = true;
                    message = string.Format("You are now logged in - {0}", user.UserId);
                }
                catch (MobileServiceInvalidOperationException)
                {
                    message = "You must log in. Login Required";
                }
            }
            
            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();

            return success;
        }

    在此版本的**AuthenticateAsync**，该应用程序试图使用存储在**PasswordVault**中的凭据来访问服务。 常规符号中也执行时不存储的凭据。

    >[AZURE.NOTE]快取的权杖可能已过期，并且令牌过期，也会在身份验证之后当应用程序正在使用中。 若要了解如何确定一个标记为过期，请参阅[检查过期的身份验证令牌](http://aka.ms/jww5vp)。 处理与到期令牌相关的授权错误的解决方案，请参阅[缓存和处理在 Azure 移动服务到期的令牌管理 SDK](http://blogs.msdn.com/b/carlosfigueira/archive/2014/03/13/caching-and-handling-expired-tokens-in-azure-mobile-services-managed-sdk.aspx)的张贴内容。 

3. 两次重新启动该应用程序。

    请注意，第一个启动时，在与提供程序的符号再次需要。 但是，在第二个重新启动使用缓存的凭据并登录被绕过。 
