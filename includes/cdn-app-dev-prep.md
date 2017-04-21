## <a name="prerequisites"></a>系统必备组件

我们可以编写 CDN 管理代码之前，我们需要做一些准备工作，以使我们的代码能够使用 Azure 资源管理器进行交互。  若要执行此操作，您需要︰

* 创建资源组可以包含在本教程中，我们创建的 CDN 配置文件
* 配置 Azure Active Directory 为我们的应用程序提供身份验证
* 将权限应用到资源组，以便只有授权的用户才从我们 Azure AD 租户可以与我们 CDN 的配置文件进行交互

### <a name="creating-the-resource-group"></a>创建资源组

1. 登录到[Azure 的门户](https://portal.azure.com)。

2. 单击**新建**按钮的左上角，然后**管理**和**资源组**中。
    
    ![创建新的资源组](./media/cdn-app-dev-prep/cdn-new-rg-1-include.png)

3. 致电您的资源组*CdnConsoleTutorial*。  选择您的订阅，然后选择离您最近的位置。  如果您愿意，您可以单击**附到仪表板**复选框以在门户中固定到仪表板的资源组。  这将便于以后查找。  做出您的选择后，单击**创建**。

    ![命名资源组](./media/cdn-app-dev-prep/cdn-new-rg-2-include.png)

4. 如果您不能将其锁定到您的仪表板创建的资源组后，您可以通过单击**浏览**，然后**资源组**发现。  单击以将其打开的资源组。  记下您的**订阅 ID**。  我们将稍后需要它。

    ![命名资源组](./media/cdn-app-dev-prep/cdn-subscription-id-include.png)

### <a name="creating-the-azure-ad-application-and-applying-permissions"></a>创建 AD Azure 应用程序和应用权限

有两种方式使用 Azure Active Directory 的应用程序进行身份验证︰ 个别用户或服务主体。 主要服务是类似于 Windows 中的服务帐户。  而不是授予特定的用户权限与 CDN 配置文件进行交互时，我们改为将权限授予主体服务。  服务主体通常用于自动化、 非交互式进程。  尽管本教程编写交互式控制台应用程序，我们将重点讨论服务的主要方法。

创建服务主体包括几个步骤，包括创建 Azure 活动目录的应用程序。  为此，我们将[按照本教程](../articles/resource-group-create-service-principal-portal.md)到。

> [AZURE.IMPORTANT] 请务必按照[链接的教程](../articles/resource-group-create-service-principal-portal.md)中的步骤操作。  这一点*极其重要*您所述完全完成。  请确保记下您的**租户 ID**，**租户的域名称**(通常*。 onmicrosoft.com*域除非已指定一个自定义的域)，**客户机 ID**和**客户端身份验证密钥**，因为我们需要这些更高。  随着这些凭据可用于任何人为服务主体执行操作，则要非常小心地保护您的**客户机 ID**和**客户端身份验证密钥**。 
>   
> 获取命名为[配置的多租户应用程序](../articles/resource-group-create-service-principal-portal.md#configure-multi-tenant-application)的步骤时，请选择**否**。
> 
> [分配给角色的应用程序](../articles/resource-group-create-service-principal-portal.md#assign-application-to-role)的步骤时，使用我们之前，创建*CdnConsoleTutorial*，该资源组但而不是**读者**角色分配**CDN 配置文件参与者**角色。  **CDN 配置文件参与者**角色对资源组分配该应用程序后，返回到本教程。 

一旦您已经创建了您的服务主体和分配**CDN 配置文件参与者**角色，**用户**刀片式服务器资源组应类似于此。

![用户刀片式服务器](./media/cdn-app-dev-prep/cdn-service-principal-include.png)


### <a name="interactive-user-authentication"></a>交互式用户身份验证

如果而不是服务主体，而是应具有交互式的单个用户验证，该进程是非常类似于一个服务主体。  事实上，您将需要执行相同的过程，但一些小的更改。

> [AZURE.IMPORTANT] 如果您选择用于单个用户的身份验证而不是服务主体，只有执行以下步骤。

1. 当创建您的应用程序，而不是**Web 应用程序**中，选择**本机应用程序**。 
    
    ![本机应用程序](./media/cdn-app-dev-prep/cdn-native-application-include.png)
    
2. 在下一页上，您将会提示输入**重定向 URI**。  URI 不会进行验证，但是，请记住您的输入。  您将稍后需要它。 

3. 没有必要创建一个**客户端的身份验证密钥**。

4. 而不指派到**CDN 配置文件参与者**角色服务主体，我们要将各个用户或组分配。  在此示例中，您可以看到我已经将*CDN 演示用户*指派到**CDN 配置文件参与者**角色。  
    
    ![单个用户的访问权限](./media/cdn-app-dev-prep/cdn-aad-user-include.png)

