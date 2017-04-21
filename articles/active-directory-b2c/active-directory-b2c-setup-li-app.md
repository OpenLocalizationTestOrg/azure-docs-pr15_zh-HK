<properties
    pageTitle="Azure 的活动目录 B2C: LinkedIn 配置 |Microsoft Azure"
    description="为与应用程序进行保护的 Azure 活动目录 B2C 中的 LinkedIn 帐户的使用者提供注册和登录"
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-linkedin-accounts"></a>Azure 的活动目录 B2C︰ 为使用 LinkedIn 帐户的使用者提供注册和登录

## <a name="create-a-linkedin-application"></a>创建 LinkedIn 的应用程序

LinkedIn 作为在 Azure 活动目录 (AD Azure) B2C 标识提供程序，您需要创建 LinkedIn 的应用程序，并提供正确的参数。 您需要 LinkedIn 帐户来执行此操作。 如果您没有，您可以在[https://www.linkedin.com/](https://www.linkedin.com/)获得。

1. 转到[网站 LinkedIn 开发](https://www.developer.linkedin.com/)并使用 LinkedIn 的帐户凭据登录。
2. 单击顶部的菜单栏中的**我的应用程序**，然后单击**创建应用程序**。

    ![LinkedIn 的新应用程序](./media/active-directory-b2c-setup-li-app/linkedin-new-app.png)

3. 在**创建新的应用程序**窗体中，填写相关信息 （**公司名称**、**名称**、**说明**、**应用程序徽标 URL**、**使用应用程序**、**网站 URL**、**商务电子邮件**和**商务电话**）。
4. **LinkedIn API 使用术语**的共识，并单击**提交**。

    ![LinkedIn 的注册应用程序](./media/active-directory-b2c-setup-li-app/linkedin-register-app.png)

5. 复制的**客户机 ID**和**客户端密钥**的值。 （您可以查找它们下**身份验证密钥**。您将需要这两个将 LinkedIn 配置为您组织中的身份提供程序。

    >[AZURE.NOTE] **客户端密钥**是重要的安全凭据。

6. 输入`https://login.microsoftonline.com/te/{tenant}/oauth2/authresp`（下**OAuth 2.0**) 的**授权将重定向 Url**字段中。 **{租户}**替换租户的名称 (例如，contoso.onmicrosoft.com)。 单击**添加**，然后单击**更新**。 **{租户}**值是区分大小写。

    ![LinkedIn 的安装应用程序](./media/active-directory-b2c-setup-li-app/linkedin-setup.png)

## <a name="configure-linkedin-as-an-identity-provider-in-your-tenant"></a>将 LinkedIn 配置为您组织中的身份提供程序

1. 按照以下[导航到 B2C 功能刀片](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade)到 Azure 的门户网站上。
2. 在 B2C 功能刀片式服务器，请单击**身份提供程序**。
3. 单击**+ 添加**刀片式服务器顶部。
4. 标识提供程序配置为提供友好**名称**。 例如，输入"LI"。
5. 单击**标识提供程序类型**、 选择**LinkedIn**，并单击**确定**。
6. 单击**设置此标识提供程序**，输入客户机 ID 和您在前面创建的 LinkedIn 应用程序客户端密码。
7. 单击**确定**，然后单击**创建**以保存 LinkedIn 配置。
