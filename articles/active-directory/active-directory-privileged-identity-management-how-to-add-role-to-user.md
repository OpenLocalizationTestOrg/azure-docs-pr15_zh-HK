<properties
   pageTitle="如何添加或删除用户角色 |Microsoft Azure"
   description="了解如何将角色添加到特权与 Azure 活动目录特权身份管理应用程序的标识。"
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
   ms.date="10/24/2016"
   ms.author="kgremban"/>

# <a name="azure-ad-privileged-identity-management-how-to-add-or-remove-a-user-role"></a>Azure AD 特权身份管理︰ 如何添加或删除用户角色

使用 Azure 活动目录 (AD)，全局管理员 （或公司管理员） 可以更新哪些用户将**永久**在 Azure AD 中分配给角色。 这通过像 PowerShell cmdlet `Add-MsolRoleMember` ， `Remove-MsolRoleMember`。 或者他们可以[分配管理员角色 Azure Active Directory 中的](active-directory-assign-admin-roles.md)所述使用 Azure 的传统门户网站。

Azure AD 特权身份管理应用程序允许特权的角色管理员进行永久性的角色分配。 此外，特权的角色管理员可以使用户**资格**管理角色。 符合条件的管理员可以激活角色，当他们需要它，然后及其权限过期后在完成后。

## <a name="manage-roles-with-pim-in-the-azure-portal"></a>管理与 PIM Azure 门户中的角色

在您的组织中，您可以将用户分配到不同的管理角色 Azure AD，Office 365，和其他 Microsoft 服务和应用程序中。  可用的角色的更多详细信息可位于[Azure AD PIM 中的角色](active-directory-privileged-identity-management-roles.md)。

要添加或删除用户，请使用特权身份管理角色中，请打开 PIM 仪表板。 然后单击**管理角色中的用户**按钮，或从角色表中选择特定角色 （如全局管理员）。

> [AZURE.NOTE] 如果没有尚未在 Azure 门户启用 PIM，前往[开始使用 Azure AD PIM](active-directory-privileged-identity-management-getting-started.md)的详细信息。

如果希望其他用户访问本身的 PIM，进一步描述[如何授予访问权限 PIM](active-directory-privileged-identity-management-how-to-give-access-to-pim.md)中 PIM 要求该用户拥有的角色了。

## <a name="add-a-user-to-a-role"></a>将用户添加到角色

1. 在[Azure 的门户](https://portal.azure.com/)中，选择操控板上的**Azure AD 特权身份管理**麻将牌。
2. 选择**管理特权的角色**。
3. 在**角色摘要**表中，选择您想要管理的角色。
4. 在角色刀片式服务器，选择**添加**。
5. 单击**选择用户**并**选择用户**刀片式服务器上的用户搜索。  
6. 从搜索结果列表中，选择用户并单击**完成**。
4. 单击**确定**以保存您的选择。 所选的用户将出现在为适合该角色的列表。

> [AZURE.NOTE]
>新角色中的用户仅适用于默认的角色。 如果要永久保留该角色，请单击列表中的用户。 用户的信息将显示在新的刀片。 在用户信息菜单中，选择**使 perm** 。  
>如果用户不能注册为 Azure 多因素身份验证 (MFA)，或使用 Microsoft 帐户 (通常@outlook.com),需要使其永久保留在他们的角色。 符合条件的管理员需要在激活过程中登记的 MFA。

现在，用户有资格享受一个角色，让他们知道，他们可以根据中[如何激活或停用某个角色](active-directory-privileged-identity-management-how-to-activate-role.md)的说明激活它。

## <a name="remove-a-user-from-a-role"></a>从角色中删除用户

可以从合格的角色分配，删除用户，但确保始终至少一个用户是永久的全局管理员。

请按照以下步骤从角色中删除特定的用户︰

1. 通过选择 Azure AD PIM 仪表板中的角色或通过单击**管理角色中的用户**按钮进入角色列表中的角色。
2. 单击用户列表中的用户。
3. 单击**删除**。 一条消息将询问您确认。
4. 单击**是**以从用户中删除角色。

如果您不确定哪个用户仍需要其角色分配，则可以[启动该角色的访问检查](active-directory-privileged-identity-management-how-to-start-security-review.md)。


<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>下一步行动
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
