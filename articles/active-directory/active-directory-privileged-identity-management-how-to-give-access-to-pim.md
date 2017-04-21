<properties
   pageTitle="如何让 PIM 的访问 |Microsoft Azure"
   description="了解如何将角色添加到使用 Azure 活动目录权限标识管理扩展的用户，以便他们可以管理 PIM。"
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
   ms.date="09/22/2016"
   ms.author="kgremban"/>

# <a name="how-to-give-access-to-manage-azure-ad-privileged-identity-management"></a>如何授予访问权来管理 Azure AD 特权身份管理

全局管理员自动启用组织的 Azure AD 特权身份管理 (PIM) 获得角色分配和对 PIM 的访问。 没有其他人获取写访问权限，默认情况下，不过，包括其他全局管理员。 其他全局管理员、 安全管理员和安全读者有到 Azure AD PIM 的只读访问权限。 要访问给 PIM，则第一个用户可以赋予其他人给**特权的角色管理员**角色。 此工作分配必须从内 PIM 本身，并不能更改通过 PowerShell 或其他门户。

> [AZURE.NOTE] 管理 Azure AD PIM 需要 Azure MFA。 由于 Microsoft 帐户不能注册 Azure MFA，使用 Microsoft 帐户进行签名的用户不能访问 Azure AD PIM。

请确保始终至少两个用户中有一个特权的角色的管理员角色，在一个用户被锁定的情况下或他们的帐户已被删除。

## <a name="give-another-user-access-to-manage-pim"></a>授予其他用户访问管理 PIM

1. 登录到[Azure 的门户网站](https://portal.azure.com/)，然后选择**Azure AD 特权身份管理**应用程序在仪表板上。
2. 选择**管理特权的角色** > **特权的角色管理员** > **添加**。

    ![添加特权的角色的管理员的屏幕抓图][1]

4. 添加托管的用户刀片上, 已完成第 1 步。 步骤 2，**选择用户**并搜索您想要添加的用户选择。

    ![选择用户的屏幕抓图][2]

6. 从搜索结果中，选择用户并单击**完成**。
7. 单击**确定**以保存您的选择。 所选的用户将出现在管理员特权的角色列表中。

    - 只要将新的角色分配给人，它们会自动设为合格才能激活该角色。 如果您想要使之永久化的角色中，请单击列表中的用户。 在用户信息菜单中，选择**使 perm** 。

8. 用户发送一个链接到[Azure AD 特权身份管理入门](active-directory-privileged-identity-management-getting-started.md)。


## <a name="remove-another-users-access-rights-for-managing-pim"></a>删除管理 PIM 的另一个用户的访问权限

从特权的角色管理员角色中删除用户之前，请始终确保仍将分配给它的两个用户。

1. 在 PIM 面板中，单击**特权的角色管理员**角色。  将显示当前在该角色中的用户列表中。
2. 单击用户列表中的用户。
3. 单击**删除**。  将显示一条确认消息。
4. 单击**是**要从角色中删除用户。

<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>下一步行动
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_add_PRA.png
[2]: ./media/active-directory-privileged-identity-management-how-to-give-access-to-pim/PIM_select_users.png
