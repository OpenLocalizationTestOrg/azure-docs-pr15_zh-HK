<properties
   pageTitle="创建服务主体在门户网站中 |Microsoft Azure"
   description="描述如何创建新活动目录应用程序和服务主体，可以使用基于角色的访问控制在 Azure 资源管理器来管理对资源的访问。"
   services="azure-resource-manager"
   documentationCenter="na"
   authors="tfitzmac"
   manager="timlt"
   editor="tysonn"/>

<tags
   ms.service="azure-resource-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/07/2016"
   ms.author="tomfitz"/>

# <a name="use-portal-to-create-active-directory-application-and-service-principal-that-can-access-resources"></a>使用门户网站创建活动目录应用程序和服务主体可以访问的资源

> [AZURE.SELECTOR]
- [PowerShell](resource-group-authenticate-service-principal.md)
- [Azure CLI](resource-group-authenticate-service-principal-cli.md)
- [门户网站](resource-group-create-service-principal-portal.md)


当应用程序需要访问或修改资源时，必须设置活动目录 (AD) 应用程序，并为其指定所需的权限。 本主题演示如何执行这些步骤通过门户。 目前，您必须使用经典门户创建新活动目录的应用程序，然后再切换到 Azure 的门户网站，将角色分配给应用程序。 

> [AZURE.NOTE] 本主题中的步骤仅适用时使用**传统门户**创建 AD 应用程序。 **用于创建 AD 应用程序中使用 Azure 的门户，如果这些步骤不会成功。** 
>
> 您可能会发现它容易地设置您的广告应用程序和服务主体通过[PowerShell](resource-group-authenticate-service-principal.md)或[Azure CLI](resource-group-authenticate-service-principal-cli.md)，尤其是如果您想要使用证书进行身份验证。 本主题不显示如何使用证书。

活动目录的概念的说明，请参阅[应用程序对象和服务主体对象](./active-directory/active-directory-application-objects.md)。 有关活动目录身份验证的详细信息，请参阅[Azure AD 身份验证方案](./active-directory/active-directory-authentication-scenarios.md)。

将应用程序集成到 Azure，来管理资源的详细步骤，请参阅[授权使用 Azure 资源管理器 API 的开发人员指南 》](resource-manager-api-authentication.md)。

## <a name="create-an-active-directory-application"></a>创建活动目录应用程序

1. 登录到 Azure 帐户通过[经典的门户](https://manage.windowsazure.com/)。 

2. 请确保您知道您的订阅活动目录的默认值。 您只能授予您的订阅相同的目录中应用程序的访问权限。 选择**设置**，并寻找与订阅相关的目录名称。  有关详细信息，请参阅[如何 Azure 订阅将与 Azure Active Directory 相关联](./active-directory/active-directory-how-subscriptions-associated-directory.md)。
   
     ![查找默认目录](./media/resource-group-create-service-principal-portal/show-default-directory.png)

2. 从左窗格中选择**活动目录**。

     ![请选择有效的目录](./media/resource-group-create-service-principal-portal/active-directory.png)
     
3. 选择要用于创建应用程序的活动目录。 如果您有多个活动目录，请在您的订阅的默认目录中创建应用程序。   

     ![选择目录](./media/resource-group-create-service-principal-portal/active-directory-details.png)
     
3. 若要查看应用程序在您的目录中，选择**应用程序**。

     ![查看应用程序](./media/resource-group-create-service-principal-portal/view-applications.png)

4. 如果未在该目录中创建一个应用程序，您应看到类似于下面的图像。 选择**添加应用程序**

     ![添加应用程序](./media/resource-group-create-service-principal-portal/create-application.png)

     或者，单击底部窗格中的**添加**。

     ![添加](./media/resource-group-create-service-principal-portal/add-icon.png)

5. 选择您想要创建的应用程序的类型。 对于本教程，选择**添加我的公司正在开发的应用程序**。 

     ![新的应用程序](./media/resource-group-create-service-principal-portal/what-do-you-want-to-do.png)

6. 提供应用程序的名称并选择您想要创建的应用程序的类型。 对于本教程，创建一个**WEB 应用程序和/或 WEB API** ，并单击下一步按钮。 如果您选择**本机客户端应用程序**，本文中的其余步骤将不匹配您的体验。

     ![应用程序名称](./media/resource-group-create-service-principal-portal/tell-us-about-your-application.png)

7. 填写您的应用程序的属性。 对于**符号上的 URL**，提供网站描述您的应用程序的 URI。 不验证网站的存在。 对于**应用程序 ID URI**，可提供标识您的应用程序的 URI。

     ![应用程序属性](./media/resource-group-create-service-principal-portal/app-properties.png)

您已经创建了您的应用程序。

## <a name="get-client-id-and-authentication-key"></a>获取客户端的标识和身份验证密钥

当以编程方式记录时，您的应用程序需要标识。 如果在自己的凭据下运行应用程序，您还需要一个身份验证密钥。

1. 选择**配置**选项卡来配置应用程序的密码。

     ![配置应用程序](./media/resource-group-create-service-principal-portal/application-configure.png)

2. 复制**客户机 ID**。
  
     ![客户机 id](./media/resource-group-create-service-principal-portal/client-id.png)

3. 如果应用程序在其自己的凭据下运行，向下滚动到**密钥**部分中，选择有效密码多长时间。

     ![密钥](./media/resource-group-create-service-principal-portal/create-key.png)

4. 选择**保存**以创建您的密钥。

     ![保存](./media/resource-group-create-service-principal-portal/save-icon.png)

     显示保存的项，您可以将其复制。 您将不能检索此密钥因此立即复制。

     ![保存密钥](./media/resource-group-create-service-principal-portal/save-key.png)

## <a name="get-tenant-id"></a>获取租户 id

当以编程方式记录时，您需要传递的租户 id 与您的身份验证请求。 对于 Web 应用程序和 Web API 的应用程序，您可以通过选择**查看终结点**在屏幕的底部，然后检索 id，如下面的图像所示检索租户 id。  

   ![租户 id](./media/resource-group-create-service-principal-portal/save-tenant.png)

您也可以检索通过 PowerShell 的租户 id:

    Get-AzureRmSubscription

或者，Azure CLI:

    azure account show --json

## <a name="set-delegated-permissions"></a>设置委派的权限

如果您的应用程序代表已登录的用户访问资源，必须授予应用程序访问其他应用程序的委派的权限。 您授予此访问权限，**对其他应用程序的权限**部分中的**配置**选项卡。 默认情况下，已为 Azure Active Directory 启用委派的权限。 保留此委派的权限保持不变。

1. 选择**添加应用程序**。

2. 从列表中选择**Windows Azure 服务管理 API**。 然后，选择完成图标。

      ![选择应用程序](./media/resource-group-create-service-principal-portal/select-app.png)

3. 在委派权限下拉列表列表中，选择**访问 Azure 服务管理组织**。

      ![选择权限](./media/resource-group-create-service-principal-portal/select-permissions.png)

4. 保存更改。

## <a name="assign-application-to-role"></a>分配给角色的应用程序

如果您的应用程序在其自己的凭据下运行，您必须分配给角色的应用程序。 决定哪些角色代表应用程序的正确权限。 若要了解有关可用的角色信息，请参阅[RBAC︰ 内置角色的](./active-directory/role-based-access-built-in-roles.md)。 

将角色分配给应用程序，您必须具有正确的权限。 具体来说，您必须具有`Microsoft.Authorization/*/Write`通过[所有者](./active-directory/role-based-access-built-in-roles.md#owner)角色或[用户访问管理员](./active-directory/role-based-access-built-in-roles.md#user-access-administrator)角色授予的访问权限。 参与者角色不具有适当的访问权限。

您可以在订购、 资源组或资源级别设置范围。 到较低级别的作用域会继承权限。 例如，添加到资源组的读者角色的应用程序意味着它可以读取的资源组，它包含的所有资源。

1. 要分配给角色的应用程序，从切换经典门户到[Azure 的门户](https://portal.azure.com)。

1. 检查以确保您可以为角色指派服务主体的权限。 为您的帐户选择**我的权限**。

    ![选择我的权限](./media/resource-group-create-service-principal-portal/my-permissions.png)

1. 查看您的帐户分配的权限。 如前所述，您必须属于所有者或用户访问管理员的角色，或为 Microsoft.Authorization 授予写访问权限的自定义的角色。 下图显示了分配给参与者角色对于订阅，这是不足够的权限分配给角色的应用程序的帐户。

    ![显示我的权限](./media/resource-group-create-service-principal-portal/show-permissions.png)

     如果您不具有正确的权限以授予访问应用程序，您必须请求，您预订的管理员将您添加到用户访问管理员角色，或者请求管理员授予对应用程序的访问。

1. 浏览到您想要分配到该应用程序范围的级别。 若要指定在订阅范围角色，选择**订阅**。

     ![选择订阅](./media/resource-group-create-service-principal-portal/select-subscription.png)

     选择要分配到该应用程序的特定订阅。

     ![选择工作分配的预订](./media/resource-group-create-service-principal-portal/select-one-subscription.png)

     选择**访问**图标的右上角。

     ![选择访问](./media/resource-group-create-service-principal-portal/select-access.png)
     
     或者，若要指定在资源组作用域的角色，请导航到资源组。 从资源组刀片式服务器，选择**访问控制**。

     ![选择用户](./media/resource-group-create-service-principal-portal/select-users.png)

     以下步骤都相同的任何范围。

2. 选择**添加**。

     ![选择添加](./media/resource-group-create-service-principal-portal/select-add.png)

3. 选择**读者**角色 （或要分配到该应用程序的任何角色）。

     ![选择角色](./media/resource-group-create-service-principal-portal/select-role.png)

4. 当第一次看到可以添加到该角色的用户列表中时，您不会看到应用程序。 您将只能看到组和用户。

     ![向用户显示](./media/resource-group-create-service-principal-portal/show-users.png)

5. 若要查找您的应用程序，您必须对其进行搜索。 开始键入您的应用程序的名称，可用选项的列表将发生更改。 当您在列表中看到它，请选择应用程序。

     ![分配角色](./media/resource-group-create-service-principal-portal/assign-to-role.png)

6. 选择**好了**完成指派角色。 您现在应该看到分配给资源组角色的使用的应用程序列表中。


有关用户和角色通过门户的应用程序分配的详细信息，请参阅[使用管理 Azure 订阅资源访问权限的角色分配](role-based-access-control-configure.md#manage-access-using-the-azure-management-portal)。

## <a name="sample-applications"></a>示例应用程序

下面的示例应用程序演示如何登录为服务主体。

**.NET**

- [部署一个 SSH 启用.NET 与模板的虚拟机](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-template-deployment/)
- [管理 Azure 的资源和资源组使用.NET](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-resources-and-groups/)

**Java**

- [在 Java 入门的资源-部署使用 Azure 的资源管理器模板](https://azure.microsoft.com/documentation/samples/resources-java-deploy-using-arm-template/)
- [在 Java 入门的管理资源组的资源](https://azure.microsoft.com/documentation/samples/resources-java-manage-resource-group//)

**Python**

- [部署一个 SSH 启用使用 Python 中的模板的虚拟机](https://azure.microsoft.com/documentation/samples/resource-manager-python-template-deployment/)
- [管理 Azure 的资源和资源组使用 Python](https://azure.microsoft.com/documentation/samples/resource-manager-python-resources-and-groups/)

**Node.js**

- [部署一个 SSH 启用与 Node.js 中模板的虚拟机](https://azure.microsoft.com/documentation/samples/resource-manager-node-template-deployment/)
- [管理 Azure 的资源和资源组与 Node.js](https://azure.microsoft.com/documentation/samples/resource-manager-node-resources-and-groups/)

**拼音**

- [部署一个 SSH 启用使用 Ruby 中的模板的虚拟机](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-template-deployment/)
- [管理 Azure 的资源和资源组与拼音](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)


## <a name="next-steps"></a>下一步行动

- 若要了解有关指定安全策略，请参阅[基于 Azure 角色的访问控制](./active-directory/role-based-access-control-configure.md)。  
- 有关这些步骤的视频演示，请参阅[启用编程管理 Azure Active Directory Azure 资源](https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Enabling-Programmatic-Management-of-an-Azure-Resource-with-Azure-Active-Directory)。

