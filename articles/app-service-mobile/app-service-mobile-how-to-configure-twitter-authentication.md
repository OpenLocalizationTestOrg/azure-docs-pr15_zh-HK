<properties
    pageTitle="如何配置 Twitter 应用程序服务应用程序的身份验证"
    description="了解如何配置 Twitter 的应用程序服务应用程序的身份验证。"
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

# <a name="how-to-configure-your-app-service-application-to-use-twitter-login"></a>如何配置应用程序服务应用程序使用 Twitter 登录

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

本主题演示如何将 Azure 应用程序服务使用 Twitter 作为身份验证提供程序配置。

若要完成此主题中的过程，您必须具有经验证的电子邮件地址和电话号码的 Twitter 帐户。 若要创建新的 Twitter 帐户，请转到<a href="http://go.microsoft.com/fwlink/p/?LinkID=268287" target="_blank">twitter.com</a>。

## <a name="register"></a>向 Twitter 注册应用程序


1. 登录到[Azure 的门户]，并导航到您的应用程序。 复制您的**URL**。 您将使用此配置您 Twitter 的应用程序。

2. 导航至的[使用 Twitter，开发人员]网站，登录 Twitter 帐户凭据，并单击**创建新的应用程序**。

3. 键入**名称**和**说明**的新的应用程序。 在您的应用程序中粘贴**网站**值的**URL** 。 然后，**回调 URL**，将粘贴先前复制**回调 URL** 。 这是您的移动应用程序网关后面有一个路径， _/.auth/login/twitter/callback_。 例如， `https://contoso.azurewebsites.net/.auth/login/twitter/callback`。 请确保您正在使用 HTTPS 方案。

3.  在页面的底部，读取和接受的条款。 然后单击**创建您的 Twitter 应用程序**。 这将注册该应用程序显示应用程序的详细信息。

4. 单击**设置**选项卡，检查**允许登录 Twitter 可用于此应用程序**，然后单击**更新设置**。

5. 选择**密钥和访问令牌**的选项卡。 记下的值的**使用者密钥 （API 密钥）**和**使用者密码 （API 机密）**。

    > [AZURE.NOTE] 使用者秘密是重要的安全凭据。 不要与任何人共享该密钥或将其分发您的应用程序使用。


## <a name="secrets"></a>向应用程序添加使用 Twitter 的信息

13. 回在[Azure 的门户网站]中，导航到您的应用程序。 单击**设置**，然后**身份验证 / 授权**。

14. 如果身份验证 / 授权功能未启用，将开关设置为**On**。

15. 单击**使用 Twitter**。 粘贴的应用程序 ID 和应用程序密码以前获取的值。 然后单击**确定**。

    ![][1]

    默认情况下，应用程序服务提供身份验证，但不限制授权的访问您的网站内容和 Api。 在应用程序代码中，您必须授权用户。

17. （可选）要限制到您的网站只通过 Twitter 的身份验证的用户访问，设置为**使用 Twitter，****请求没有通过身份验证时要采取的操作**。 这就要求对所有请求进行身份都验证，并且所有未经身份验证的请求重定向到 Twitter 进行身份验证。

17. 单击**保存**。

现在您可以为您的应用程序中的身份验证使用 Twitter。

## <a name="related-content"></a>相关内容

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]



<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-twitter-authentication/app-service-twitter-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-twitter-authentication/mobile-app-twitter-settings.png

<!-- URLs. -->

[Twitter 的开发人员]: http://go.microsoft.com/fwlink/p/?LinkId=268300
[Azure 门户]: https://portal.azure.com/
[xamarin]: ../app-services-mobile-app-xamarin-ios-get-started-users.md
