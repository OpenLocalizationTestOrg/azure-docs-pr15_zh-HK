<properties
    pageTitle="Azure 的活动目录 B2C: Facebook 配置 |Microsoft Azure"
    description="与应用程序进行保护的 Azure 活动目录 B2C 中的 Facebook 帐户的消费者提供注册和登录。"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-facebook-accounts"></a>Azure 的活动目录 B2C︰ 为与 Facebook 帐户的使用者提供注册和登录

## <a name="create-a-facebook-application"></a>创建一个 Facebook 应用程序

作为在 Azure 活动目录 (AD Azure) B2C 标识提供程序使用 Facebook，您需要创建一个 Facebook 应用程序提供正确的参数。 您需要一个 Facebook 帐户来执行此操作。 如果您没有，您可以在[https://www.facebook.com/](https://www.facebook.com/)获得。

1. 转到[开发人员的 Facebook](https://developers.facebook.com/)网站，以您的 Facebook 帐户凭据登录。
2. 如果您有不这样做，您需要注册为 Facebook 开发人员。 为此，（位于页面的右上角） 中单击**注册**，接受 Facebook 的策略完成注册步骤。
3. 单击**我的应用程序**，然后单击**添加新的应用程序**。 **网站**为平台，选择，然后单击**跳过和创建应用程序 ID**。

    ![Facebook 的添加新的应用程序](./media/active-directory-b2c-setup-fb-app/fb-add-new-app.png)

    ![Facebook 的网站中添加新的应用程序 —](./media/active-directory-b2c-setup-fb-app/fb-add-new-app-website.png)

    ![Facebook 的创建应用程序 ID](./media/active-directory-b2c-setup-fb-app/fb-new-app-skip.png)

4. 在表单上，提供一个**显示名称**、**联系人电子邮件**有效、 适当的**类别**，然后单击**创建的应用程序 ID**。 这要求您接受 Facebook 平台策略并完成在线安全检查。

    ![Facebook 的创建新的应用程序 ID](./media/active-directory-b2c-setup-fb-app/fb-create-app-id.png)

5. 在左边的导航，请单击**设置**。
6. 单击**+ 添加平台**，然后选择**网站**。

    ![Facebook 的设置](./media/active-directory-b2c-setup-fb-app/fb-settings.png)

    ![-设置-Facebook 网站](./media/active-directory-b2c-setup-fb-app/fb-website.png)

7. 在**网站 URL**字段中输入[https://login.microsoftonline.com/](https://login.microsoftonline.com/) ，然后单击**保存更改**。

    ![Facebook 的网站 URL](./media/active-directory-b2c-setup-fb-app/fb-site-url.png)

8. 将复制**的应用程序 ID**的值。 单击**显示**并复制**应用程序密码**的值。 您将需要这两个将 Facebook 配置为您组织中的身份提供程序。 **应用程序的机密**是重要的安全凭据。

    ![Facebook 的应用程序 ID 和应用程序密码](./media/active-directory-b2c-setup-fb-app/fb-app-id-app-secret.png)

9. 单击**+ 添加产品**上左边的导航，然后再**开始**按钮**Facebook 登录**。

    ![Facebook 的 Facebook 登录](./media/active-directory-b2c-setup-fb-app/fb-login.png)

10. 输入`https://login.microsoftonline.com/te/{tenant}/oauth2/authresp`在**客户端 OAuth 设置**部分中的**有效 OAuth 重定向 Uri**字段中。 **{租户}**替换租户的名称 (例如，contosob2c.onmicrosoft.com)。 单击页面底部的**保存更改**。

    ![Facebook 的 OAuth 重定向 URI](./media/active-directory-b2c-setup-fb-app/fb-oauth-redirect-uri.png)

11. 若要使 Facebook 应用程序可使用 Azure AD B2C，需要进行公开发布。 您可以这样做，通过单击左侧导航**应用程序查看**和打开开关**是**页面顶部，单击**确认**。

    ![Facebook 的应用程序公共](./media/active-directory-b2c-setup-fb-app/fb-app-public.png)

## <a name="configure-facebook-as-an-identity-provider-in-your-tenant"></a>将 Facebook 配置为您组织中的身份提供程序

1. 按照以下[导航到 B2C 功能刀片](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade)到 Azure 的门户网站上。
2. 在 B2C 功能刀片式服务器，请单击**身份提供程序**。
3. 单击**+ 添加**刀片式服务器顶部。
4. 标识提供程序配置为提供友好**名称**。 例如，输入"FB"。
5. 单击**标识提供程序类型**、 选择**Facebook**，并单击**确定**。
6. 单击**设置此标识提供程序**，输入应用程序 ID 和应用程序密码 （您在前面创建的 Facebook 应用程序） 的**客户机 ID**和**客户端密钥**中的字段分别。
7. 单击**确定**，然后单击**创建**以保存 Facebook 配置。
