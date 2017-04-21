<properties
    pageTitle="Azure 的活动目录 B2C: Microsoft 帐户配置 |Microsoft Azure"
    description="与您的应用程序进行保护的 Azure 活动目录 B2C 中的 Microsoft 帐户的消费者提供注册和登录。"
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-microsoft-accounts"></a>Azure 的活动目录 B2C︰ 为 Microsoft 帐户的使用者提供注册和登录

## <a name="create-a-microsoft-account-application"></a>创建 Microsoft 客户应用程序

使用 Microsoft 帐户作为在 Azure 活动目录 (AD Azure) B2C 标识提供程序，您需要创建 Microsoft 客户应用程序提供正确的参数。 您需要为此 Microsoft 帐户。 如果您没有，您可以在[https://www.live.com/](https://www.live.com/)获得。

1. 转到[Microsoft 应用程序注册门户](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList)并使用您的 Microsoft 帐户凭据登录。
2. 单击**添加应用程序**。

    ![Microsoft 的帐户-添加新应用程序](./media/active-directory-b2c-setup-msa-app/msa-add-new-app.png)

3. 为应用程序提供一个**名称**，然后单击**创建的应用程序**。

    ![Microsoft 客户的应用程序名称](./media/active-directory-b2c-setup-msa-app/msa-app-name.png)

4. 复制**应用程序 Id**的值。 您将需要将 Microsoft 帐户配置为您组织中的身份提供程序。

    ![Microsoft 客户的应用程序 Id](./media/active-directory-b2c-setup-msa-app/msa-app-id.png)

5. **添加平台**上单击，然后选择**Web**。

    ![Microsoft 的帐户-添加平台](./media/active-directory-b2c-setup-msa-app/msa-add-platform.png)

    ![Microsoft 帐户-Web](./media/active-directory-b2c-setup-msa-app/msa-web.png)

6. 输入`https://login.microsoftonline.com/te/{tenant}/oauth2/authresp`**重定向 Uri**字段中。 **{租户}**替换租户的名称 (例如，contosob2c.onmicrosoft.com)。

    ![Microsoft 帐户-重定向 URL](./media/active-directory-b2c-setup-msa-app/msa-redirect-url.png)

7. 在**应用程序的机密**部分下单击**生成新密码**。 将复制新密码显示在屏幕上。 您将需要将 Microsoft 帐户配置为您组织中的身份提供程序。 该密码是重要的安全凭据。

    ![Microsoft 的帐户，即生成新密码](./media/active-directory-b2c-setup-msa-app/msa-generate-new-password.png)

    ![Microsoft 帐户的新密码](./media/active-directory-b2c-setup-msa-app/msa-new-password.png)

8. 选中的复选框表明**实时 SDK 支持**在**高级选项**部分下。 单击**保存**。

    ![Microsoft 帐户-实时 SDK 支持](./media/active-directory-b2c-setup-msa-app/msa-live-sdk-support.png)

## <a name="configure-microsoft-account-as-an-identity-provider-in-your-tenant"></a>将 Microsoft 帐户配置为您组织中的身份提供程序

1. 按照以下[导航到 B2C 功能刀片](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade)到 Azure 的门户网站上。
2. 在 B2C 功能刀片式服务器，请单击**身份提供程序**。
3. 单击**+ 添加**刀片式服务器顶部。
4. 标识提供程序配置为提供友好**名称**。 例如，输入"MSA"。
5. 单击**标识提供程序类型**，选择**Microsoft 客户**，单击**确定**。
6. 单击**设置此标识提供程序**，输入的应用程序 Id 和您在前面创建的 Microsoft 帐户应用程序的密码。
7. 单击**确定**，然后单击**创建**以保存您的 Microsoft 帐户配置。
