<properties
   pageTitle="身份验证与使用 Active Directory 数据湖商店 |Microsoft Azure"
   description="学习如何与数据使用 Active Directory 的湖泊存储区验证"
   services="data-lake-store"
   documentationCenter=""
   authors="nitinme"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="10/17/2016"
   ms.author="nitinme"/>

# <a name="end-user-authentication-with-data-lake-store-using-azure-active-directory"></a>与使用 Azure Active Directory 数据湖存储最终用户身份验证

> [AZURE.SELECTOR]
- [服务的服务身份验证](data-lake-store-authenticate-using-active-directory.md)
- [最终用户身份验证](data-lake-store-end-user-authenticate-using-active-directory.md)


Azure 数据湖商店使用 Azure Active Directory 进行身份验证。 在创作应用程序适用于 Azure 数据湖商店或 Azure 数据湖分析之前, 必须先决定如何想要对您的应用程序使用 Azure 活动目录 (AD Azure) 进行身份验证。 可用的两个主要选项有︰

* 最终用户身份验证，并 
* 提供服务的身份验证。 

这两个选项会导致应用程序正在使用 OAuth 2.0 令牌，获取附加到 Azure 数据湖商店或 Azure 数据湖分析对每个请求提供。

此文章讨论的是如何创建 Azure 广告 web 应用程序的最终用户身份验证。 Azure AD 服务到服务身份验证的应用程序配置的说明请参阅[服务到服务身份验证数据湖存储区使用 Azure Active Directory](data-lake-store-authenticate-using-active-directory.md)。

## <a name="prerequisites"></a>系统必备组件

* Azure 的订阅。 请参阅[获取 Azure 免费试用版](https://azure.microsoft.com/pricing/free-trial/)。
* 您的订阅 id。 您可以从 Azure 门户网站检索它。 例如，就可从数据湖存储帐户刀片式服务器。

    ![获取订阅 ID](./media/data-lake-store-end-user-authenticate-using-active-directory/get-subscription-id.png)

* Azure AD 域的名称。 您可以将鼠标指针悬停在 Azure 门户网站的右上角上的鼠标进行检索。 从下面的屏幕截图，域名为**contoso.microsoft.com**，并在中括号内的 GUID 是租户 id。 

    ![获取 AAD 域](./media/data-lake-store-end-user-authenticate-using-active-directory/get-aad-domain.png)

## <a name="end-user-authentication"></a>最终用户身份验证

如果您希望最终用户要登录到您的应用程序通过 Azure 的广告，这是推荐的方法。 您的应用程序将能够访问 Azure 资源具有相同级别的访问权限为最终用户登录。 最终用户需要提供定期以使应用程序能够访问其凭据。

使最终用户登录的结果是您的应用程序提供一个访问令牌并刷新令牌。 访问令牌获取附加到数据湖存储或数据湖分析，对每个请求，一小时，默认情况下有效。 刷新令牌可用来获得一个新的访问令牌，并且如果定期使用，它是有效的默认情况下，最多两个星期。 为最终用户日志，您可以使用两种不同的方法。

### <a name="using-the-oauth-20-pop-up"></a>使用 OAuth 2.0 弹出窗口

您的应用程序可以触发 OAuth 2.0 授权弹出窗口，最终用户可以在其中输入其凭据。 此弹出窗口也适用于 Azure AD 双因素身份验证 (2FA) 过程中，如果需要。 

>[AZURE.NOTE] 此方法尚不支持在 Azure AD 身份验证库 (ADAL) 的 Python 或 Java。

### <a name="directly-passing-in-user-credentials"></a>直接传递用户凭据

您的应用程序直接可以到 Azure AD 提供用户凭据。 此方法只适用于组织 ID 用户帐户;不兼容的个人 /"活动 ID"用户帐户，包括结束在@outlook.com或@live.com。 此外，此方法不需要 Azure AD 双因素身份验证 (2FA) 的用户帐户与兼容。

### <a name="what-do-i-need-to-use-this-approach"></a>使用这种方法需要什么？

* Azure AD 域的名称。 这已列入本文的先决条件。

* Azure 广告**web 应用程序**

* Azure 广告 web 应用程序的客户端 ID

* Azure 广告 web 应用程序的 URI 答复

* 设置委派的权限

有关如何创建 Azure 广告 web 应用程序并将其配置为上面列出的要求说明，请参阅[创建活动目录应用程序](#create-an-active-directory-application)下面的部分。 

## <a name="create-an-active-directory-application"></a>创建活动目录应用程序

在本节中我们了解如何创建和使用 Azure 数据湖存储区配置 Azure 广告 web 应用程序的最终用户身份验证使用 Azure Active Directory。


### <a name="step-1-create-an-azure-active-directory-application"></a>步骤 1︰ 创建一个 Active Directory 的 Azure 应用程序

>[AZURE.NOTE] 下面的步骤使用 Azure 门户。 您还可以创建使用[Azure PowerShell](../resource-group-authenticate-service-principal.md)或[Azure CLI](../resource-group-authenticate-service-principal-cli.md)的 Azure AD 应用程序。

1. 登录到 Azure 帐户通过[经典的门户](https://manage.windowsazure.com/)。

2. 从左窗格中选择**活动目录**。

     ![请选择有效的目录](./media/data-lake-store-end-user-authenticate-using-active-directory/active-directory.png)
     
3. 选择要用于创建新的应用程序的活动目录。 如果您有多个活动目录，您通常想要在您的订阅所在的目录中创建应用程序。 只能在您的订阅您的订阅相同的目录中的应用程序授予对资源的访问。  

     ![选择目录](./media/data-lake-store-end-user-authenticate-using-active-directory/active-directory-details.png)
    
    
3. 要查看应用程序在您的目录中，请单击**应用程序**。

     ![查看应用程序](./media/data-lake-store-end-user-authenticate-using-active-directory/view-applications.png)

4. 如果尚未创建应用程序在该目录中之前，您应该看到类似于下面的图像。 单击**添加应用程序**

     ![添加应用程序](./media/data-lake-store-end-user-authenticate-using-active-directory/create-application.png)

     或者，单击底部窗格中的**添加**。

     ![添加](./media/data-lake-store-end-user-authenticate-using-active-directory/add-icon.png)

6. 提供应用程序的名称并选择您想要创建的应用程序的类型。 对于本教程，创建一个**WEB 应用程序和/或 WEB API** ，并单击下一步按钮。

     ![应用程序名称](./media/data-lake-store-end-user-authenticate-using-active-directory/tell-us-about-your-application.png)

7. 填写您的应用程序的属性。 对于**符号上的 URL**，提供网站描述您的应用程序的 URI。 不验证网站的存在。 对于**应用程序 ID URI**，可提供标识您的应用程序的 URI。

     ![应用程序属性](./media/data-lake-store-end-user-authenticate-using-active-directory/app-properties.png)

    单击复选标记以完成此向导并创建应用程序。

### <a name="step-2-get-client-id-reply-uri-and-set-delegated-permissions"></a>步骤 2︰ 获取客户机 id、 回复 URI，并设置委派的权限

1. 单击**配置**选项卡来配置应用程序的密码。

     ![配置应用程序](./media/data-lake-store-end-user-authenticate-using-active-directory/application-configure.png)

2. 复制**客户机 ID**。
  
     ![客户机 id](./media/data-lake-store-end-user-authenticate-using-active-directory/client-id.png)

3. 在**单一登录**部分中，将复制**答复 URI**。

    ![客户机 id](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-get-reply-uri.png)

4. 在**其他应用程序的权限**，请单击**添加应用程序**

    ![客户机 id](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-1.png)

5. 在**其他应用程序的权限**向导中，选择**Azure 数据湖**和**Windows Azure** **服务管理 API**，并单击选中标记。

6. 默认情况下将新添加的服务**委派权限**设置为零。 单击**委派权限**Azure 数据湖和 Windows Azure 管理服务的下拉列表并选择可用的复选框，以将值设置为 1。 结果应如下所示。

     ![客户机 id](./media/data-lake-store-end-user-authenticate-using-active-directory/aad-end-user-auth-set-permission-2.png)

7. 单击**保存**。


## <a name="next-steps"></a>下一步行动

在这篇文章中，您创建 Azure 广告 web 应用程序并收集的信息，您需要在客户端应用程序中使用.NET SDK、 Java SDK 等创作。您现在可以继续下面谈一谈如何使用 Azure 广告 web 应用程序与数据存储湖泊区首先进行身份验证，然后执行其他操作存储区上的文章。

- [学习如何使用 Azure 数据湖存储区使用.NET SDK](data-lake-store-get-started-net-sdk.md)
- [学习如何使用 Azure 数据湖存储区使用 Java SDK](data-lake-store-get-started-java-sdk.md)
