## <a name="obtain-an-azure-resource-manager-token"></a>获取一个 Azure 资源管理器标记

Azure 的 Active Directory 必须验证您在使用 Azure 资源管理器的资源执行的所有任务。 此处显示的示例使用密码身份验证的其他方法，请参阅[身份验证 Azure 资源管理器请求][lnk-authenticate-arm]。

1. 将下面的代码添加到**Main**方法中 Program.cs 从 Azure 使用应用程序 id 和密码的 AD 检索令牌。

    ```
    var authContext = new AuthenticationContext(string.Format  
      ("https://login.windows.net/{0}", tenantId));
    var credential = new ClientCredential(applicationId, password);
    AuthenticationResult token = authContext.AcquireTokenAsync
      ("https://management.core.windows.net/", credential).Result;
    
    if (token == null)
    {
      Console.WriteLine("Failed to obtain the token");
      return;
    }
    ```

2. 创建一个**ResourceManagementClient**对象，通过将下面的代码添加到**Main**方法的末尾使用令牌︰

    ```
    var creds = new TokenCredentials(token.AccessToken);
    var client = new ResourceManagementClient(creds);
    client.SubscriptionId = subscriptionId;
    ```

3. 创建或获取的引用，您正在使用的资源组︰

    ```
    var rgResponse = client.ResourceGroups.CreateOrUpdate(rgName,
        new ResourceGroup("East US"));
    if (rgResponse.Properties.ProvisioningState != "Succeeded")
    {
      Console.WriteLine("Problem creating resource group");
      return;
    }
    ```

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx