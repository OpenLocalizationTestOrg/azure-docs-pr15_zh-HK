<properties
    pageTitle="Azure 的活动目录 B2C: Google + 配置 |Microsoft Azure"
    description="与您的应用程序进行保护的 Azure 活动目录 B2C 中的 Google + 帐户的消费者提供注册和登录。"
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-google-accounts"></a>Azure 的活动目录 B2C︰ 为使用 Google + 帐户的使用者提供注册和登录

## <a name="create-a-google-application"></a>创建一个 Google + 应用程序

要使用 Google + 作为在 Azure 活动目录 (AD Azure) B2C 标识提供程序，您需要创建 Google + 应用程序并提供正确的参数。 您需要一个 Google + 帐户来执行此操作。 如果您没有，您可以在[https://accounts.google.com/SignUp](https://accounts.google.com/SignUp)获得。

1. 转到[Google 开发人员控制台](https://console.developers.google.com/)并使用 Google + 帐户凭据登录。
2. 单击**创建项目**，输入的**项目名称**，然后单击**创建**。

    ![Google +-入门](./media/active-directory-b2c-setup-goog-app/google-get-started.png)

    ![Google + 的新项目](./media/active-directory-b2c-setup-goog-app/google-new-project.png)

3. 单击**管理器 API** ，然后单击在左侧导航窗格中的**凭据**。
4. 单击顶部的**OAuth 同意屏幕**选项卡。

    ![Google + 的凭据](./media/active-directory-b2c-setup-goog-app/google-add-cred.png)

5. 选择或指定一个有效的**电子邮件地址**，提供的**产品名称**，并单击**保存**。

    ![Google + 的 OAuth 同意屏幕](./media/active-directory-b2c-setup-goog-app/google-consent-screen.png)

6. 单击**新凭据**，然后选择**OAuth 的客户端 ID**。

    ![Google + 的 OAuth 同意屏幕](./media/active-directory-b2c-setup-goog-app/google-add-oauth2-client-id.png)

7. 在**应用程序类型**下选择**Web 应用程序**。

    ![Google + 的 OAuth 同意屏幕](./media/active-directory-b2c-setup-goog-app/google-web-app.png)

8. 为应用程序提供的**名称**中，输入`https://login.microsoftonline.com`**授权 JavaScript 来源**字段中，并`https://login.microsoftonline.com/te/{tenant}/oauth2/authresp`**授权重定向 Uri**字段中。 **{租户}**替换租户的名称 (例如，contosob2c.onmicrosoft.com)。 **{租户}**值是区分大小写。 单击**创建**。

    ![Google +-创建客户端 ID](./media/active-directory-b2c-setup-goog-app/google-create-client-id.png)

9. 复制的**客户机 ID**和**客户端密钥**的值。 您将需要这两种配置 Google + 作为您组织中的身份提供程序。 **客户端密钥**是重要的安全凭据。

    ![Google +-客户端密码](./media/active-directory-b2c-setup-goog-app/google-client-secret.png)

## <a name="configure-google-as-an-identity-provider-in-your-tenant"></a>将 Google + 配置为您组织中的身份提供程序

1. 按照以下[导航到 B2C 功能刀片](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade)到 Azure 的门户网站上。
2. 在 B2C 功能刀片式服务器，请单击**身份提供程序**。
3. 单击**+ 添加**刀片式服务器顶部。
4. 标识提供程序配置为提供友好**名称**。 例如，输入"G +"。
5. 单击**标识提供程序类型**、 选择**Google**，然后单击**确定**。
6. 单击**设置此标识提供程序**，输入客户机 ID 和 Google + 应用程序的前面创建的客户端密码。
7. 单击**确定**，然后单击**创建**以保存配置 Google +。
