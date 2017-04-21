<properties
   pageTitle="在 PIM 中的角色 |Microsoft Azure"
   description="了解哪些角色用于使用 Azure 特权身份管理扩展的特权身份。"
   services="active-directory"
   documentationCenter=""
   authors="kgremban"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="07/01/2016"
   ms.author="kgremban"/>

# <a name="roles-in-azure-ad-privileged-identity-management"></a>在 Azure 广告中的角色的特权身份管理

<!-- **PLACEHOLDER: Need description of how this works. Azure PIM uses roles from MSODS objects.**-->

您可以将用户分配不同管理角色组织在 Azure 的广告中。 这些角色分配控制的任务，例如添加或删除用户或更改服务设置，用户就能够对 Azure 广告，Office 365 提供其他 Microsoft Online Services 和连接的应用程序执行。  

全局管理员可以更新哪些用户将**永久地**分配给角色在 Azure 的广告，如使用 PowerShell cmdlet `Add-MsolRoleMember` ， `Remove-MsolRoleMember`，或通过为所述[分配管理员角色 Azure Active Directory 中的](active-directory-assign-admin-roles.md)经典的门户。

Azure AD 特权身份管理 (PIM) 在 Azure AD 管理特权的用户访问策略。 PIM Azure 的广告，在将用户分配给一个或多个角色，您可以将某人永久角色，或适用于该角色分配。 当用户永久分配给角色，或激活合格角色分配，然后他们可以具有分配给他们的角色的权限管理 Azure Active Directory、 Office 365 和其他应用程序。

没有任何区别，在给予某人有资格角色分配与正式的访问。 唯一的区别是有些人，不需要访问所有的时间。 他们进行了适合该角色，并将其打开和关闭时所需。

## <a name="roles-managed-in-pim"></a>在 PIM 中进行管理的角色

特权的标识管理，可以将用户分配到常见的管理员角色，包括︰


- **全局管理员**（也称为公司管理员） 可以访问所有的管理功能。 您的组织中，可以有多个全局管理员。 注册自动购买 Office 365 的人将成为全局管理员。
- **特权的角色管理员**管理 Azure AD PIM 并更新其他用户的角色分配。  
- **记帐管理员**使采购、 管理订阅、 管理支持的故障单，并监视服务运行状况。
- **密码管理员**重置密码、 管理服务请求，并监视服务运行状况。 密码管理员仅限于重置用户的密码。
- **服务管理员**管理服务请求并监视服务运行状况。

  > [AZURE.NOTE] 如果您使用的 Office 365，然后将服务管理员角色分配给用户之前首先给用户管理权限服务，例如 Exchange 联机。

- **用户管理员**重置密码、 监视服务运行状况，并管理用户帐户、 用户组和服务请求。 用户管理管理员无法删除全局管理员、 创建其他管理角色，或重置全局，计费和服务管理员的密码。
- **Exchange 管理员**通过 Exchange 管理员中心 (EAC)，具有管理访问 Exchange 联机并且可以执行几乎所有任务在 Exchange 联机。
- **SharePoint 管理员**能够通过 SharePoint Online 管理中心，管理访问 SharePoint Online，并可以在 SharePoint Online 执行几乎任何任务。
- **Skype 业务管理员**具有管理访问权 Skype 业务到业务管理中心，为 Skype，并可以执行几乎所有任务在 Skype 的在线业务。

请阅读以下文章获取有关[在 Azure 的广告分配管理员角色](active-directory-assign-admin-roles.md)和[分配在 Office 365 管理角色](https://support.office.com/article/Assigning-admin-roles-in-Office-365-eac4d046-1afd-4f1a-85fc-8219c79e1504)的更多详细信息。

<!--**PLACEHOLDER: The above article may not be the one we want since PIM gets roles from places other that Office 365**-->


从 PIM，您可以[分配这些角色到用户](active-directory-privileged-identity-management-how-to-add-role-to-user.md)，以便用户可以[激活时所需的角色](active-directory-privileged-identity-management-how-to-activate-role.md)。

如果您想要使其他用户访问管理本身的 PIM 中，进一步描述如何[授予访问权限 PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md)PIM 要求该用户拥有的角色了。


<!-- ## The PIM Security Administrator Role **PLACEHOLDER: Need description of the Security Administrator role.**-->

## <a name="roles-not-managed-in-pim"></a>未在 PIM 中管理角色

Exchange 联机或 SharePoint Online 中，上面提到的以外的角色在 Azure AD 中没有表示，因此是不可见的 PIM 中。 更改在这些 Office 365 提供服务的细粒度的角色分配的详细信息，请参阅[Office 365 中的权限](https://support.office.com/article/Permissions-in-Office-365-da585eea-f576-4f55-a1e0-87090b6aaa9d)。

Azure 订阅和资源组也不会出现在 Azure 的广告。 若要管理 Azure 订阅，了解[如何](../billing-add-change-azure-subscription-administrator.md)添加或更改 Azure 管理员角色和 Azure RBAC 的详细信息，请参阅[Azure Role-Based 访问控制](role-based-access-control-configure.md)。

<!--**The above links might be replaced by ones that are from within this documentation repository **-->


## <a name="user-roles-and-signing-in"></a>用户角色和登录
对于某些 Microsoft 服务和应用程序，为用户分配角色可能不足以使该用户成为管理员。

访问 Azure 的传统门户网站要求用户是服务管理员或共同管理员 Azure 的订购，即使用户不需要管理 Azure 的订阅。  例如，要为 Azure AD 中经典的门户管理配置设置，用户必须是 Azure AD 中的全局管理员和 Azure 订阅的订阅联管理员。  若要了解如何将用户添加到 Azure 的订阅，请参阅[添加或更改 Azure 的管理员角色](../billing-add-change-azure-subscription-administrator.md)。

访问 Microsoft Online Services 可能需要用户还被分派许可证可以打开该服务的门户网站或执行管理任务。

## <a name="assign-a-license-to-a-user-in-azure-ad"></a>在 Azure AD 中向用户分派许可证

1. 登录到 [Azure 经典门户] (http://manage.windowsazure.com) 与全局管理员帐户或共同的管理员帐户。
2. 从主菜单中选择**所有项**。
3. 选择您想要使用的目录，并具有与之关联的许可证。
4. 选择**许可证**。 将显示可用的许可证列表。
5. 选择包含您想要分配的许可证的许可证计划。
6. 选择**用户分配**。
7. 选择您要分配给有许可证的用户。
8. 单击**分配**按钮。  用户现在可以登录到 Azure。

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>下一步行动
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
