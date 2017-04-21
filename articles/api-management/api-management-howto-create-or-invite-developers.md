<properties 
    pageTitle="如何管理用户帐户在 Azure API 管理 |Microsoft Azure" 
    description="了解如何创建或邀请在 Azure API 管理用户" 
    services="api-management" 
    documentationCenter="" 
    authors="steved0x" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="api-management" 
    ms.workload="mobile" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/25/2016" 
    ms.author="sdanie"/>

# <a name="how-to-manage-user-accounts-in-azure-api-management"></a>如何管理在 Azure API 管理用户帐户

在 API 管理中，开发人员使用 API 管理公开的 Api 的用户。 本指南介绍如何创建并邀请开发人员可以使用 Api 和产品可以提供给他们的 API 管理实例。 有关以编程方式管理用户帐户的信息，请参阅[API 管理其余部分](https://msdn.microsoft.com/library/azure/dn776326.aspx)引用中的[用户实体](https://msdn.microsoft.com/library/azure/dn776330.aspx)文档。

## <a name="create-developer"></a>创建新的开发人员

若要创建新的开发人员，请单击**管理**API 管理服务的 Azure 经典门户。 这将您带到管理 API 发布门户。 如果尚未创建 API 管理服务实例，请[开始使用 Azure API 管理][]教程中参阅[创建 API 管理服务实例][]。

![出版商门户][api-management-management-console]

从**API 管理**菜单的左侧，单击**用户**，然后单击**添加用户**。

![创建开发人员][api-management-create-developer]

输入新开发人员的**电子邮件**、**密码**和**名称**，并单击**保存**。

![创建开发人员][api-management-add-new-user]

默认情况下，新创建的开发人员帐户**活动**，并与**开发人员**组相关联。

![新开发人员][api-management-new-developer]

用于访问他们拥有订阅的 Api 的所有开发人员帐户处于不**活动**状态。 若要将新创建的开发人员与其他组相关联，请参阅[如何将组与开发人员相关联][]。

## <a name="invite-developer"></a>邀请开发人员

若要邀请开发人员，从**API 管理**菜单的左侧，单击**用户**，然后单击**邀请用户**。

![邀请开发者][api-management-invite-developer]

输入开发人员的姓名和电子邮件地址，然后单击**邀请**。

![邀请开发者][api-management-invite-developer-window]

将显示一条确认消息，但新邀请开发者在接受邀请后未出现在列表中，直到。 

![邀请确认][api-management-invite-developer-confirmation]

当开发人员已邀请时，给开发人员发送一封电子邮件。 这封电子邮件使用模板生成的和自定义。 有关详细信息，请参阅[配置电子邮件模板][]。

一旦接受邀请，则该帐户将被激活。

## <a name="block-developer"></a>停用或重新激活开发者帐户

默认情况下，新创建的或被邀请开发者帐户处于**活动状态**。 要停用某个开发人员帐户，请单击**块**。 要重新激活已阻止开发人员帐户，请单击**激活**。 被阻止的开发人员帐户可以访问开发人员门户或调用任何 Api。 要删除用户帐户，请单击**删除**。

![块开发人员][api-management-new-developer]

## <a name="reset-a-user-password"></a>重置用户密码

要重置用户帐户密码，请单击该帐户的名称。

![重置密码][api-management-view-developer]

单击**重置密码**将链接发送到用户重置其密码。

![重置密码][api-management-reset-password]

若要以编程方式使用用户帐户，请参阅[API 管理其余部分](https://msdn.microsoft.com/library/azure/dn776326.aspx)引用中的[用户实体](https://msdn.microsoft.com/library/azure/dn776330.aspx)文档。 若要为特定的值重置用户帐户密码，可以使用[更新用户](https://msdn.microsoft.com/library/azure/dn776330.aspx#UpdateUser)操作，并指定所需的密码。

## <a name="pending-verification"></a>挂起的验证

![挂起的验证][api-management-pending-verification]

## <a name="next-steps"></a>下一步行动

开发人员帐户创建后，可以将它与角色相关联，并订阅产品和 Api。 有关详细信息，请参阅[如何创建和使用组][]。


[api-management-management-console]: ./media/api-management-howto-create-or-invite-developers/api-management-management-console.png
[api-management-add-new-user]: ./media/api-management-howto-create-or-invite-developers/api-management-add-new-user.png
[api-management-create-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-create-developer.png
[api-management-invite-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer.png
[api-management-new-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-new-developer.png
[api-management-invite-developer-window]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-window.png
[api-management-invite-developer-confirmation]: ./media/api-management-howto-create-or-invite-developers/api-management-invite-developer-confirmation.png
[api-management-pending-verification]: ./media/api-management-howto-create-or-invite-developers/api-management-pending-verification.png
[api-management-view-developer]: ./media/api-management-howto-create-or-invite-developers/api-management-view-developer.png
[api-management-reset-password]: ./media/api-management-howto-create-or-invite-developers/api-management-reset-password.png
[]: ./media/api-management-howto-create-or-invite-developers/.png



[Create a new developer]: #create-developer
[Invite a developer]: #invite-developer
[Deactivate or reactivate a developer account]: #block-developer
[Next steps]: #next-steps
[如何创建和使用组]: api-management-howto-create-groups.md
[如何将组与开发人员相关联]: api-management-howto-create-groups.md#associate-group-developer

[Azure API 管理入门]: api-management-get-started.md
[创建 API 管理服务实例]: api-management-get-started.md#create-service-instance
[配置电子邮件模板]: api-management-howto-configure-notifications.md#email-templates