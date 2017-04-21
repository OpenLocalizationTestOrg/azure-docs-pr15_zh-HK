<properties
    pageTitle="Azure 的活动目录 B2C: Amazon 配置 |Microsoft Azure"
    description="与您的应用程序进行保护的 Azure 活动目录 B2C 中的 Amazon 帐户的消费者提供注册和登录。"
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

# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-amazon-accounts"></a>Azure 的活动目录 B2C︰ 为与 Amazon 帐户的使用者提供注册和登录

## <a name="create-an-amazon-application"></a>创建一个 Amazon 的应用

Amazon 用作在 Azure 活动目录 (AD Azure) B2C 标识提供程序，您需要创建 Amazon 应用程序并提供正确的参数。 您需要 Amazon 帐户来执行此操作。 如果您没有，您可以在[http://www.amazon.com/](http://www.amazon.com/)获得。

1. 转到[Amazon 开发人员中心](https://login.amazon.com/)，以 Amazon 帐户凭据登录。
2. 如果您没有单击**注册**、 按照开发人员注册，并接受此策略。
3. 单击**注册新的应用程序**。

    ![注册 Amazon 网站上的新应用程序](./media/active-directory-b2c-setup-amzn-app/amzn-new-app.png)

4. 提供应用程序信息 （**名称**、**说明**和**保密通知 URL**） 并单击**保存**。

    ![注册新的应用程序在 Amazon 提供的应用程序信息](./media/active-directory-b2c-setup-amzn-app/amzn-register-app.png)

5. 在**网站设置**部分中复制的**客户机 ID**和**客户端密钥**的值。 （您需要单击**显示密钥**按钮才能看到此。）您需要两个配置为您组织中的身份提供 Amazon。 单击底部的部分中的**编辑**。 **客户端密钥**是重要的安全凭据。

    ![新应用程序在 Amazon 提供的客户端 ID 和客户端密钥](./media/active-directory-b2c-setup-amzn-app/amzn-client-secret.png)

6. 输入`https://login.microsoftonline.com`在字段中**允许使用 JavaScript 起源**和`https://login.microsoftonline.com/te/{tenant}/oauth2/authresp`**允许返回 Url**字段中。 **{租户}**替换租户的名称 (例如，contoso.onmicrosoft.com)。 单击**保存**。 **{租户}**值是区分大小写。

    ![新应用程序在 Amazon 提供了 JavaScript 起源和返回的 Url](./media/active-directory-b2c-setup-amzn-app/amzn-urls.png)

## <a name="configure-amazon-as-an-identity-provider-in-your-tenant"></a>将 Amazon 配置为您组织中的身份提供程序

1. 按照以下[导航到 B2C 功能刀片](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade)到 Azure 的门户网站上。
2. 在 B2C 功能刀片式服务器，请单击**身份提供程序**。
3. 单击**+ 添加**刀片式服务器顶部。
4. 标识提供程序配置为提供友好**名称**。 例如，输入"Amzn"。
5. 单击**标识提供程序类型**、 选择**Amazon**，并单击**确定**。
6. 单击**设置此标识提供程序**，输入客户机 ID 和您在前面创建的 Amazon 应用程序客户端密码。
7. 单击**确定**，然后单击**创建**以保存 Amazon 配置。
