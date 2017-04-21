<properties
    pageTitle="如何配置 Google 应用程序服务应用程序的身份验证"
    description="了解如何配置 Google 应用程序服务应用程序的身份验证。"
    services="app-service"
    documentationCenter=""
    authors="mattchenderson"
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

# <a name="how-to-configure-your-app-service-application-to-use-google-login"></a>如何配置应用程序服务应用程序使用 Google 登录

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

本主题演示如何将 Azure 应用程序服务使用 Google 作为身份验证提供程序配置。

若要完成此主题中的过程，您必须具有一个已验证电子邮件地址的 Google 帐户。 若要创建一个新的 Google 帐户，请转到[accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302)。

## <a name="register"></a>向 Google 注册应用程序

1. 登录到[Azure 的门户]，并导航到您的应用程序。 复制您的**URL**，您日后用来配置您的 Google 应用程序。

2. 导航到[Google api](http://go.microsoft.com/fwlink/p/?LinkId=268303)网站，使用您的 Google 帐户凭据登录，单击**创建项目**、 提供一个**项目名**，然后单击**创建**。

3. **社会的 Api**在**Google + API**单击，然后单击**启用**。

4. 在左边的导航，**凭据** > **OAuth 同意屏幕**，然后选择您的**电子邮件地址**，输入一个**产品名称**，并单击**保存**。

5. 在**凭据**选项卡中，单击**创建凭据** > **OAuth 客户机 ID**，然后选择**Web 应用程序**。

6. 粘贴先前复制**授权 JavaScript 起源**，到应用程序服务**URL** ，然后将您重定向 URI 粘贴到**授权重定向 URI**。 重定向 URI 是附加的路径， _/.auth/login/google/callback_与您应用程序的 URL。 例如， `https://contoso.azurewebsites.net/.auth/login/google/callback`。 请确保您正在使用 HTTPS 方案。 然后单击**创建**。

7. 在下一个屏幕上，记下客户机 ID 和客户端密钥的值。


    > [AZURE.IMPORTANT]
    客户端密钥是重要的安全凭据。 不要与任何人共享该密钥或将其分发到客户端应用程序内部。


## <a name="secrets"></a>向应用程序添加 Google 信息

8. 回在[Azure 的门户网站]中，导航到您的应用程序。 单击**设置**，然后**身份验证 / 授权**。

9. 如果身份验证 / 授权功能未启用，将开关设置为**On**。

10. 单击**Google**。 在这以前，获得的应用程序 ID 和应用程序密码值粘贴，或者启用应用程序所需的任何作用域。 然后单击**确定**。

    ![][1]

    默认情况下，应用程序服务提供身份验证，但不限制授权的访问您的网站内容和 Api。 在应用程序代码中，您必须授权用户。

17. （可选）若要限制对网站 Google 通过身份验证的用户访问，设置到**Google****请求没有通过身份验证时要采取的操作**。 这就要求对所有请求进行身份都验证，并且所有未经身份验证的请求重定向到 Google 进行身份验证。

12. 单击**保存**。

现在您可以为您的应用程序中的身份验证使用 Google。

## <a name="related-content"></a>相关内容

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]


<!-- Anchors. -->

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-settings.png

<!-- URLs. -->

[Google apis]: http://go.microsoft.com/fwlink/p/?LinkId=268303

[Azure 门户]: https://portal.azure.com/

