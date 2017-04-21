<properties
    pageTitle="Azure AD 特权身份管理 |Microsoft Azure"
    description="解释什么是 Azure AD 特权身份管理以及如何使用 PIM 来提高您的云安全的主题。"
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/16/2016"
    ms.author="kgremban"/>

# <a name="azure-ad-privileged-identity-management"></a>Azure AD 特权身份管理

使用 Azure 活动目录 (AD) 特权身份管理，可以管理，控制，并监视访问您的组织中。 这包括对 Azure 广告资源和 Office 365 或 Microsoft Intune 等其他 Microsoft 在线服务的访问。  

> [AZURE.NOTE] 特权的身份管理是只能使用 Azure Active Directory 的特优 P2 版本。 有关详细信息，请参见[Azure Active Directory 版本](active-directory-editions.md)。

组织希望尽可能减少的人员有权访问安全信息或资源，因为它可以减少恶意用户获得访问权限的可能性。 但是，用户仍需要执行在 Azure，Office 365 或 SaaS 应用程序的特权操作。 组织在 Azure AD 中授予用户的特权的访问，而不监视执行具有管理员特权的用户。 Azure AD 特权身份管理可以帮助解决这一风险。  

Azure AD 特权身份管理可以帮助您︰  

- 查看哪些用户是 Azure AD 管理员
- "刚好及时"对 Microsoft Online Services 管理访问权限，如 Office 365 和 Intune 启用按需
- 管理员分配中获得管理员的访问历史记录和更改有关的报告
- 获取有关访问特权角色的提醒

Azure AD 特权身份管理可以管理内置 Azure 广告组织角色，包括︰  

- 全局管理员
- 记帐管理员
- 服务管理员  
- 用户管理员
- 管理员密码

## <a name="just-in-time-administrator-access"></a>只是在时间管理员访问权限

从历史上看，可以为用户分配通过 Azure 的传统门户网站或 Windows PowerShell 管理员角色。 因此，该用户将成为**永久的管理**，在指定的角色中，始终处于活动状态。 Azure AD 特权身份管理引入了**有资格管理**这一概念。 符合条件的管理员应不时，但不是每天需要访问权限的用户。 该角色处于非活动状态，直到用户需要访问权，然后他们完成激活过程和预先确定的时间内成为活动的管理。

## <a name="enable-privileged-identity-management-for-your-directory"></a>为您的目录启用特权身份管理

您可以开始在[Azure 门户](https://portal.azure.com/)使用 Azure AD 特权身份管理。

>[AZURE.NOTE] 您必须是全局管理员与组织的帐户 (例如，@yourdomain.com),不是 Microsoft 帐户 (例如，@outlook.com),启用特权 Azure 广告标识管理目录。

1. 全局目录的管理员身份登录到[Azure 的门户](https://portal.azure.com/)。
2. 如果组织中有多个目录，选择您的用户名在 Azure 的门户网站的右上角。 选择将在其中使用 Azure AD 特权身份管理的目录。
3. 选择**更多的服务**，使用过滤器文本框来搜索**Azure AD 特权身份管理**。
4. 检查**针到仪表板**，然后单击**创建**。 特权身份管理应用程序打开。

如果您是在您的目录中使用 Azure AD 特权身份管理的第一个人，然后[安全向导](active-directory-privileged-identity-management-security-wizard.md)引导您完成初始分配经验。 之后，您会自动成为第一个**安全管理员**和**管理员特权的角色**的目录。

只有特权的角色管理员可以为其他管理员来管理访问。 您可以[向其他用户授予管理在 PIM 中的能力](active-directory-privileged-identity-management-how-to-give-access-to-pim.md)。

## <a name="privileged-identity-management-dashboard"></a>特权的身份管理仪表板

Azure AD 特权标识管理器提供了一个仪表板，为您提供了如下重要信息︰

- 指出提高安全性的机会的通知
- 分配给每个特权的角色的用户数  
- 符合条件和永久的管理员的数量
- 进行访问的权限检查

![PIM 仪表板-屏幕抓图][2]

## <a name="privileged-role-management"></a>特权的角色管理

利用 Azure AD 特权身份管理解决方案，您可以通过添加或删除永久或有资格的管理员给每个角色管理管理员。

![PIM 添加/删除管理员的屏幕抓图][3]

## <a name="configure-the-role-activation-settings"></a>配置角色激活设置

使用[角色设置](active-directory-privileged-identity-management-how-to-change-default-settings.md)您可以配置符合角色激活属性包括︰

- 角色激活期限内
- 角色激活通知
- 用户角色激活过程中提供所需信息  

![PIM 设置管理员激活屏幕截图][4]

请注意在图中，则禁用此按钮的**多因素身份验证**。 对于某些、 高度特权的角色，我们需要提高了保护 MFA。

## <a name="role-activation"></a>角色激活  

[激活角色](active-directory-privileged-identity-management-how-to-activate-role.md)，符合条件的管理员请求时间限制"激活"的角色。 可以在 Azure AD 特权身份管理中使用**激活我的角色**选项请求激活。

想要激活角色管理需要初始化在 Azure 门户的 Azure AD 特权身份管理。

自定义角色激活所。 在 PIM 设置中，您可以确定激活和长度信息管理需要提供才能激活该角色。

![PIM 管理员请求角色激活屏幕抓图][5]

## <a name="review-role-activity"></a>查看角色活动

有两种方法来跟踪您的员工和管理员如何使用特权的角色。 第一个选项使用[审核历史记录](active-directory-privileged-identity-management-how-to-use-audit-log.md)。 审核历史记录中特权的角色分配和角色激活历史记录跟踪更改。

![PIM 激活历史记录-屏幕抓图][6]

第二种选择是建立定期[审查访问](active-directory-privileged-identity-management-how-to-start-security-review.md)。 这些访问检查可以由执行和分配 （类似于一个团队的经理） 的审阅者或员工可以查看自己。 这是监视用户仍需要访问，和谁不能做的最好办法。


## <a name="next-steps"></a>下一步行动
[AZURE.INCLUDE [active-directory-privileged-identity-management-toc](../../includes/active-directory-privileged-identity-management-toc.md)]

<!--Image references-->

[1]: ./media/active-directory-privileged-identity-management-configure/PIM_EnablePim.png
[2]: ./media/active-directory-privileged-identity-management-configure/PIM_Dash.png
[3]: ./media/active-directory-privileged-identity-management-configure/PIM_AddRemove.png
[4]: ./media/active-directory-privileged-identity-management-configure/PIM_RoleActivationSettings.png
[5]: ./media/active-directory-privileged-identity-management-configure/PIM_RequestActivation.png
[6]: ./media/active-directory-privileged-identity-management-configure/PIM_ActivationHistory.png
