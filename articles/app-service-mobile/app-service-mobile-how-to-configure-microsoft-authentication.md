<properties
    pageTitle="如何配置应用程序服务应用程序的 Microsoft 客户身份验证"
    description="了解如何配置应用程序服务应用程序的 Microsoft 客户身份验证。"
    authors="mattchenderson"
    services="app-service"
    documentationCenter=""
    manager="erikre"
    editor=""/>

<tags
    ms.service="app-service"
    ms.workload="mobile"
    ms.tgt_pltfrm="na"
    ms.devlang="multiple"
    ms.topic="article"
    ms.date="10/01/2016"
    ms.author="mahender"/>

# <a name="how-to-configure-your-app-service-application-to-use-microsoft-account-login"></a>如何配置应用程序服务应用程序使用 Microsoft 帐户登录

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

本主题演示如何配置 Azure 应用程序服务使用 Microsoft 帐户作为身份验证提供程序。 

## <a name="register-microsoft-account"></a>向 Microsoft 帐户注册您的应用程序

1. 登录到[Azure 的门户]，并导航到您的应用程序。 复制您**的 URL**，它以后您使用与 Microsoft 客户配置您的应用程序。

2. 导航到[我的应用程序]页在 Microsoft 客户开发人员中心中，并使用您的 Microsoft 帐户，登录，如果需要。

3. 单击**添加应用程序**，然后键入应用程序的名称，并单击**创建的应用程序**。

4. 记下该**应用程序 ID**，因为您以后会需要。 

5. 在"平台、"下，单击**添加平台**并选择"Web"。

6. 在"重定向 Uri"下提供您的应用程序的终结点，然后单击**保存**。 
 
    >[AZURE.NOTE]您重定向 URI 是附加的路径， _/.auth/login/microsoftaccount/callback_与您应用程序的 URL。 例如， `https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`。   
    >请确保您正在使用 HTTPS 方案。

7. 在"应用程序机密，"下，单击**生成新密码**。 记下显示的值。 一旦离开某个页面，它不会再次显示。


    > [AZURE.IMPORTANT] 该密码是重要的安全凭据。 不与任何人共享密码，或将其分发到客户端应用程序内部。

## <a name="secrets"></a>为您的应用程序服务的应用程序添加 Microsoft 帐户信息

1. 返回在[Azure 的门户网站]中，导航到您的应用程序，请单击**设置** > **身份验证 / 授权**。

2. 如果身份验证 / 授权功能未启用，则**将其切换**。

3. 单击**Microsoft 帐户**。 在这以前，获得的应用程序 ID 和密码值粘贴，或者启用应用程序所需的任何作用域。 然后单击**确定**。

    ![][1]

    默认情况下，应用程序服务提供身份验证，但不限制授权的访问您的网站内容和 Api。 在应用程序代码中，您必须授权用户。

4. （可选）要限制到您的网站只 Microsoft 帐户通过身份验证的用户访问，设置到**Microsoft 客户****请求没有通过身份验证时要采取的操作**。 这就要求对所有请求进行身份都验证，并且所有未经身份验证的请求重定向到 Microsoft 帐户进行身份验证。

5. 单击**保存**。

现在您可以使用 Microsoft 帐户进行身份验证，您的应用程序中。

## <a name="related-content"></a>相关内容

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]


<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/app-service-microsoftaccount-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/mobile-app-microsoftaccount-settings.png

<!-- URLs. -->

[我的应用程序]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Azure 门户]: https://portal.azure.com/
