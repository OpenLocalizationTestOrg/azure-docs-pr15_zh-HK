## <a name="prepare-to-authenticate-azure-resource-manager-requests"></a>准备 Azure 资源管理器请求进行身份验证

您必须验证您在使用[Azure 资源管理器]的资源执行的所有操作[lnk-authenticate-arm]与 Azure 活动目录 (AD)。 配置了此设置的最简单方法是使用 PowerShell 或 Azure CLI。

您应该安装[Azure PowerShell 1.0] [lnk-powershell-install]或更高，然后再继续。

以下步骤显示如何设置使用 PowerShell 的广告应用程序的密码验证。 您可以在标准 PowerShell 会话中运行这些命令。

1. 登录到 Azure 订阅使用下面的命令︰

    ```
    Login-AzureRmAccount
    ```

2. 记下您的**TenantId**和**SubscriptionId**。 您将稍后需要它们。

3. 创建新的 Azure Active Directory 应用程序使用以下命令，并将占位符︰

    - **{显示名称}:**为您的应用程序，例如**MySampleApp**显示名称
    - **{主页 URL}:** **http://mysampleapp/home**如您应用程序的主页的 URL。 此 URL 不需要指向实际的应用程序。
    - **{应用程序标识符}:**唯一的标识符，例如**http://mysampleapp**。 此 URL 不需要指向实际的应用程序。
    - **{密码}:**您将使用与您的应用程序进行身份验证的密码。

    ```
    New-AzureRmADApplication -DisplayName {Display name} -HomePage {Home page URL} -IdentifierUris {Application identifier} -Password {Password}
    ```
    
4. 记下您所创建的应用程序**ApplicationId** 。 您将稍后需要它。

5. 创建新的服务主体使用下面的命令， **{MyApplicationId}**替换为**ApplicationId**上一步︰

    ```
    New-AzureRmADServicePrincipal -ApplicationId {MyApplicationId}
    ```
    
6. 安装程序使用下面的命令， **{MyApplicationId}**替换为您的**ApplicationId**角色分配。

    ```
    New-AzureRmRoleAssignment -RoleDefinitionName Owner -ServicePrincipalName {MyApplicationId}
    ```
    
您现在已经完成创建 Azure 广告应用程序，您可以从自定义 C# 应用程序进行身份验证。 在本教程后面部分，您将需要以下值︰

- TenantId
- SubscriptionId
- ApplicationId
- 密码

[lnk-authenticate-arm]: https://msdn.microsoft.com/library/azure/dn790557.aspx
[lnk-powershell-install]: ../articles/powershell-install-configure.md
