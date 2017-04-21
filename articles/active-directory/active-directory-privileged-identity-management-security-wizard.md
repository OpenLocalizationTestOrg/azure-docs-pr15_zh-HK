<properties
   pageTitle="Azure AD 特权身份管理安全向导"
   description="第一次使用 Azure 活动目录权限标识管理扩展中，您将看到安全向导。 本文介绍使用向导的步骤。"
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

# <a name="the-azure-ad-privileged-identity-management-security-wizard"></a>Azure AD 特权身份管理安全向导

如果您是第一个人来为您的组织中运行 Azure 特权身份管理 (PIM)，您将看到向导。 该向导可帮助您了解特权的身份和如何使用 PIM 来减少这些风险的安全风险。 您不需要更改现有角色分配在向导中，如果您想稍后再做。

## <a name="what-to-expect"></a>预期行为

您的组织开始使用 PIM 之前，分配的所有角色都是永久性︰ 即使他们现在不需要他们的权限，用户都都在这些角色中。  在向导的第一步显示了较高特权角色列表和用户数量目前在这些角色中。 您可以特定的角色来了解用户，如果一个钻取或多个他们不熟悉。

在向导的第二步可以更改管理员的角色分配的机会。  

> [AZURE.WARNING]它非常重要，必须至少一个全局管理员和多个特权的角色与组织的帐户 （而不是 Microsoft 帐户） 的管理员。 如果只有一个特权的角色管理员，该组织不能管理 PIM，如果删除该帐户。
> 另外，请永久如果用户的 Microsoft 帐户 （用于登录 Skype 和 Outlook.com 等 Microsoft 服务帐户） 分配的角色。 如果您计划需要 MFA 激活该角色，该用户将被锁定。


所做的更改后，向导将不会再出现。 下一次您或其他特权的角色管理员使用 PIM，您将看到 PIM 仪表板。  

- 如果您想要添加或从角色中删除用户或更改分配给从永久向符合条件的读更多如何[添加或删除用户角色](active-directory-privileged-identity-management-how-to-add-role-to-user.md)。
- 如果您想要允许多个用户访问管理 PIM，了解一下[如何向管理 PIM 中的权限](active-directory-privileged-identity-management-how-to-give-access-to-pim.md)。



## <a name="next-steps"></a>下一步行动
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]
