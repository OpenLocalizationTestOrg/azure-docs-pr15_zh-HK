<properties
    pageTitle="Azure 的 Active Directory B2C︰ 支持 |Microsoft Azure"
    description="如何对 Azure 活动目录 B2C 的文件支持请求"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="msmbaldwin"
    editor="bryanla"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>

# <a name="azure-active-directory-b2c-file-support-requests"></a>Azure 的 Active Directory B2C︰ 文件支持请求

可以为使用以下步骤对 Azure 门户上的 Azure 活动目录 (AD Azure) B2C 文件支持请求︰

1. [请按照下列步骤定位到 B2C 功能刀片式服务器在 Azure 的门户网站上](active-directory-b2c-app-registration.md#navigate-to-the-b2c-features-blade)。
2. 从 B2C 租户切换到另一个具有与之关联的订阅了 Azure 的租户。 通常情况下，后者是员工租户或当您注册 Azure 预订为您创建默认组织。 若要了解详细信息，请参阅[如何到 Azure AD 相关 Azure 的订阅](active-directory-how-subscriptions-associated-directory.md#how-an-azure-subscription-is-related-to-azure-ad)。

    ![支持-交换机承租人](./media/active-directory-b2c-support/support-switch-dir.png)

3. 在切换承租人后, 单击**帮助 + 支持**。

    ![支持-帮助 + 支持](./media/active-directory-b2c-support/support-support.png)

4. 单击**新的支持请求**。

    ![支持-新](./media/active-directory-b2c-support/support-new.png)

5. 在**基本知识**刀片式服务器，使用这些详细信息，然后单击**下一步**。

    - **问题类型**是**技术性**的。
    - 选择适当的**订阅**。
    - **服务**是**当前目录**。
    - 选择相应的**支持计划**。 如果您没有，您可以注册为一个在[这里](https://azure.microsoft.com/en-us/support/plans/)。

    ![支持-基础知识](./media/active-directory-b2c-support/support-basics.png)

6. 在**问题**刀片式服务器，使用这些详细信息，然后单击**下一步**。

    - 选择适当的**严重性**级别。
    - **问题类型**是**B2C**。
    - 选择适当的**类别**。
    - 描述您的问题的**详细信息**字段中。 提供详细信息，如 B2C 租户名称、 描述问题、 错误消息、 相关 Id （如果可用），等等。
    - 在**时间范围**字段中提供的日期和时间 （包括时区） 问题发生。
    - 在**文件上载**后，上载所有屏幕截图和您认为有助于解决该问题的文件。

    ![支持的问题](./media/active-directory-b2c-support/support-problem.png)

7. 在**联系信息**刀片式服务器，添加您的联系信息。 单击**创建**。

    ![支持-联系人](./media/active-directory-b2c-support/support-contact.png)

8. 在提交您的支持请求之后, 您可以监视它通过单击**帮助 + 支持**上的 Startboard，然后**管理支持请求**。

## <a name="known-issue-filing-a-support-request-in-the-context-of-a-b2c-tenant"></a>已知问题︰ 归档的 B2C 租户上下文中支持请求

如果错过步骤上面所述的 2，尝试创建一个支持请求的 B2C 租户上下文中，您将看到以下错误。

> [AZURE.IMPORTANT]
> 不要尝试在 B2C 租户 Azure 新订阅的注册。  

![支持-没有订阅](./media/active-directory-b2c-support/support-no-sub.png)
