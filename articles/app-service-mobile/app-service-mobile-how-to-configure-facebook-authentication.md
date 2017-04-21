<properties
    pageTitle="如何配置 Facebook 应用程序服务应用程序的身份验证"
    description="了解如何配置 Facebook 应用程序服务应用程序的身份验证。"
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

# <a name="how-to-configure-your-app-service-application-to-use-facebook-login"></a>如何配置应用程序服务应用程序使用 Facebook 登录

[AZURE.INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

本主题演示如何将 Azure 应用程序服务使用 Facebook 作为身份验证提供程序配置。

若要完成此主题中的过程，您必须有一个经验证的电子邮件地址和移动电话号码的 Facebook 帐户。 若要创建新的 Facebook 帐户，请转到[facebook.com]。

## <a name="register"></a>向 Facebook 注册应用程序

1. 登录到[Azure 的门户]，并导航到您的应用程序。 复制您的**URL**。 您将使用此配置您的 Facebook 应用程序。

2. 在另一个浏览器窗口中，导航到[Facebook 开发]网站和登录使用 Facebook 的帐户凭据。

3. （可选）如果尚未注册，请单击**应用程序** > **注册作为开发人员**，然后接受此策略，请按照注册步骤。

4. 单击**我的应用程序** > **添加新应用程序** > **网站** > **跳过并创建应用程序 ID**。 

5. 在**显示名称**键入您的应用程序的唯一名称，键入您**的联系人的电子邮件**、 选择**类别**为您的应用程序，然后单击**创建的应用程序 ID**并完成安全检查。 这使您转到您新的 Facebook 应用程序开发人员仪表板。

6. 在"Facebook 登录，"下，单击**开始**。 将您的应用程序**重定向 URI**添加到**有效 OAuth 重定向 Uri**，然后单击**保存更改**。 

    > [AZURE.NOTE] 您重定向 URI 是附加的路径， _/.auth/login/facebook/callback_与您应用程序的 URL。 例如， `https://contoso.azurewebsites.net/.auth/login/facebook/callback`。 请确保您正在使用 HTTPS 方案。

6. 在左侧的导航栏，单击**设置**。 在**应用程序机密**字段中，单击**显示**，提供您的密码，如果请求，然后记下**应用程序 ID**和**应用程序密码**的值。 您使用这些以后在 Azure 中配置应用程序。

    > [AZURE.IMPORTANT] 应用程序密码是重要的安全凭据。 不要与任何人共享该密钥或将其分发到客户端应用程序内部。

7. 用于将应用程序注册的 Facebook 帐户是应用程序的管理员。 在这种情况下，只有管理员可以登录到此应用程序。 要验证其他 Facebook 帐户，单击**应用程序审核**和启用**使 < 您的应用程序名称 > 公共**使一般公众使用 Facebook 的身份验证。

## <a name="secrets"></a>向应用程序添加 Facebook 信息

1. 回在[Azure 的门户网站]中，导航到您的应用程序。 单击**设置** > **身份验证 / 授权**，并确保**应用程序服务的身份验证****上**。

2. 单击**Facebook**、 粘贴在以前获取的应用程序 ID 和应用程序密码值，或者启用应用程序所需的任何作用域，然后单击**确定**。

    ![][0]

    默认情况下，应用程序服务提供身份验证，但不限制授权的访问您的网站内容和 Api。 在应用程序代码中，您必须授权用户。

3. （可选）要限制到您的网站只通过 Facebook 的身份验证的用户访问，设置到**Facebook****请求没有通过身份验证时要采取的操作**。 这就要求对所有请求进行身份都验证，并且所有未经身份验证的请求重定向到 Facebook 的身份验证。

4. 完成的配置身份验证，请单击**保存**。

现在您可以为您的应用程序中的身份验证使用 Facebook。

## <a name="related-content"></a>相关内容

[AZURE.INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->
[0]: ./media/app-service-mobile-how-to-configure-facebook-authentication/mobile-app-facebook-settings.png

<!-- URLs. -->
[Facebook 开发人员]: http://go.microsoft.com/fwlink/p/?LinkId=268286
[facebook.com]: http://go.microsoft.com/fwlink/p/?LinkId=268285
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-dotnet/
[Azure 门户]: https://portal.azure.com/
