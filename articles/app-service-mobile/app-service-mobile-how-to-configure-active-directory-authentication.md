<properties
    pageTitle="如何配置应用程序服务应用程序的 Azure Active Directory 身份验证"
    description="了解如何配置应用程序服务应用程序的 Azure Active Directory 身份验证。"
    authors="mattchenderson"
    services="app-service"
    documentationCenter=""
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service-mobile"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="mahender"/>

# <a name="how-to-configure-your-app-service-application-to-use-azure-active-directory-login"></a>如何配置应用程序服务应用程序使用 Azure Active Directory 登录

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

本主题演示如何配置作为身份验证提供程序使用 Azure Active Directory Azure 应用程序服务。

## <a name="express"></a>配置 Azure Active Directory 使用快速设置

13. 在[Azure 的门户网站]中，导航到您的应用程序。 单击**设置**，然后选择**身份验证/授权**。

14. 如果身份验证 / 授权功能未启用，将开关设置为**On**。

15. **Azure 活动目录**，请单击，然后单击**管理模式**下的**表达**。

16. 单击**确定**以在 Azure Active Directory 中注册应用程序。 这将创建新的注册。 如果您想要选择一个现有的注册，请单击**选择一个现有的应用程序**，然后搜索您组织中以前创建的注册的名称。
单击注册以选择它，然后单击**确定**。 在 Azure Active Directory 设置刀片式服务器，然后单击**确定**。

    ![][0]

    默认情况下，应用程序服务提供身份验证，但不限制授权的访问您的网站内容和 Api。 在应用程序代码中，您必须授权用户。

17. （可选）要限制到您的网站只 Azure Active Directory 通过身份验证的用户访问，设置到**Azure Active Directory 使用登录****请求没有通过身份验证时要采取的操作**。 这就要求对所有请求进行身份都验证，并且所有未经身份验证的请求重定向到 Azure Active Directory 进行身份验证。

17. 单击**保存**。

现在您可以为您的应用程序中的身份验证使用 Azure Active Directory。

## <a name="advanced"> </a>（备选方法） 手动将 Azure Active Directory 配置高级设置
您还可以选择提供配置设置手动。 如果您想要使用 AAD 租户是不同于登录到 Azure 的租户，这是首选的解决方案。 若要完成配置，您必须首先创建注册 Azure Active Directory 中，则必须为应用程序服务提供一些注册详细信息。

### <a name="register"> </a>Azure Active Directory 中注册应用程序

1. 登录到[Azure 的门户]，并导航到您的应用程序。 复制您的**URL**。 您将使用此配置您 Azure 活动目录的应用程序。

3. 登录到[Azure 经典的门户]和导航到**活动目录**。

    ![][2]

4. 选择您的目录，然后选择**应用程序**选项卡的顶部。 若要创建新的应用程序注册的底部，单击**添加**。

5. 单击**添加我的公司正在开发的应用程序**。

6. 在添加应用程序向导中，您的应用程序输入一个**名称**并单击**Web 应用程序和/或 Web API**类型。 然后单击以继续。

7. 在**符号在 URL**中，粘贴先前复制的应用程序 URL。 在**应用程序 ID URI**框中输入此相同的 URL。 然后单击以继续。

8. 该应用程序添加后，单击**配置**选项卡。 编辑**回复 URL**下**单一登录**为附加的路径， _/.auth/login/aad/callback_与您应用程序的 URL。 例如， `https://contoso.azurewebsites.net/.auth/login/aad/callback`。 请确保您正在使用 HTTPS 方案。

    ![][3]

9. 单击**保存**。 然后复制该应用程序**的客户端 ID** 。 将在后面使用该应用程序。

10. 在底部的命令栏中，单击**查看终结点**，然后将**联合元数据文档**的 URL 复制和下载该文档或在浏览器中导航到该。

11. 在根**EntityDescriptor**元素中，应该有**entityID**属性窗体的`https://sts.windows.net/`（称为"租户 ID"） 您租户跟特定的 GUID。 复制此值-它将作为您的**颁发者的 URL**。 将在后面使用该应用程序。

### <a name="secrets"></a>向应用程序添加 Azure Active Directory 信息

13. 回在[Azure 的门户网站]中，导航到您的应用程序。 单击**设置**，然后选择**身份验证/授权**。

14. 如果未启用的身份验证/授权功能，将开关设置为**上**。

15. **Azure 活动目录**，请单击，然后在**管理模式下，**单击**高级**。 粘贴先前获得的客户机 ID 和颁发者 URL 值。 然后单击**确定**。

    ![][1]

    默认情况下，应用程序服务提供身份验证，但不限制授权的访问您的网站内容和 Api。 在应用程序代码中，您必须授权用户。

17. （可选）要限制到您的网站只 Azure Active Directory 通过身份验证的用户访问，设置到**Azure Active Directory 使用登录****请求没有通过身份验证时要采取的操作**。 这就要求对所有请求进行身份都验证，并且所有未经身份验证的请求重定向到 Azure Active Directory 进行身份验证。

17. 单击**保存**。

现在您可以为您的应用程序中的身份验证使用 Azure Active Directory。

## <a name="optional-configure-a-native-client-application"></a>（可选）配置的本机客户端应用程序

Azure 的 Active Directory 还允许您注册本机客户端，它提供了更好地控制权限映射。 如果您要从中执行登录使用如**活动目录身份验证库**的库，您需要它。

1. 定位到**Active Directory** [Azure 的传统门户网站]中。

2. 选择您的目录，然后选择**应用程序**选项卡的顶部。 若要创建新的应用程序注册的底部，单击**添加**。

3. 单击**添加我的公司正在开发的应用程序**。

4. 在添加应用程序向导中，您的应用程序输入一个**名称**并单击**本机客户端应用程序**类型。 然后单击以继续。

5. 在**重定向 URI**框中，输入您的网站_/.auth/login/done_的终结点，使用 HTTPS 方案。 此值应与_https://contoso.azurewebsites.net/.auth/login/done_类似。 如果创建一个 Windows 应用程序，而是作为 URI 使用[包 SID](app-service-mobile-dotnet-how-to-use-client-library.md#package-sid) 。

6. 本机应用程序添加后，单击**配置**选项卡。 查找**客户端 ID** ，请记下的此值。

7. 向下滚动页面到**到其他应用程序的权限**部分，单击**添加应用程序**。

8. 搜索以前注册的 web 应用程序并单击加号图标。 再单击检查关闭对话框。 如果 web 应用程序不能找到，导航到其注册并添加新的回复 URL （例如，您的当前 URL 的 HTTP 版本），单击保存，然后重复上述步骤-应用程序应显示在列表中。

9. 在刚添加的新项，打开**授予权限**下拉列表并选择**访问 （应用程序名）**。 然后单击**保存**。

现在，您已配置的本机客户端应用程序可以访问您的应用程序服务的应用程序。

## <a name="related-content"></a>相关内容

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/mobile-app-aad-express-settings.png
[1]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/mobile-app-aad-advanced-settings.png
[2]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-navigate-aad.png
[3]: ./media/app-service-mobile-how-to-configure-active-directory-authentication/app-service-aad-app-configure.png

<!-- URLs. -->

[Azure 门户]: https://portal.azure.com/
[Azure 的传统门户网站]: https://manage.windowsazure.com/
[alternative method]:#advanced
