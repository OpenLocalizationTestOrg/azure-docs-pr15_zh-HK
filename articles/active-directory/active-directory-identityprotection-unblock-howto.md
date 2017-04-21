<properties
    pageTitle="活动目录标识保护 azure-如何取消阻止用户 |Microsoft Azure"
    description="了解如何阻止用户所阻止的 Azure 活动目录标识保护策略。"
    services="active-directory"
    keywords="azure 的活动目录身份保护，允许用户"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection---how-to-unblock-users"></a>活动目录标识保护 azure-如何解锁用户

通过使用 Azure 活动目录身份保护，您可以配置策略来阻止用户，如果已配置的条件得到满足。 通常情况下，阻止的用户联系人服务台不再受阻止。 此主题解释为允许被阻止的用户可以执行的步骤。


## <a name="determine-the-reason-for-blocking"></a>确定阻塞的原因

若要解除锁定用户的第一步，您需要确定的已阻止该用户，因为您下一步行动将取决于它的策略的类型。 使用 Azure 活动目录身份保护，用户可以要么阻止通过登录风险策略或用户风险策略。 

您可以获取已锁定的登录尝试中显示给用户对话框中的标题中的用户的策略的类型︰

|策略 | 用户对话框|
|--- | --- |
|登录的风险 | ![被禁止登录](./media/active-directory-identityprotection-unblock-howto/02.png) |
|用户风险 | ![锁定的帐户](./media/active-directory-identityprotection-unblock-howto/104.png) |


被阻止的用户︰

- 登录以风险策略即所谓可疑登录
- 用户风险策略也称为是自负的帐户

 
## <a name="unblocking-suspicious-sign-ins"></a>解除阻止可疑登录

若要解除锁定可疑的签入，您有下列选项︰

1. **熟悉的位置或设备的登录**-阻止可疑登录的一个常见原因是不熟悉的位置或设备登录尝试。 您的用户可以快速确定是否这是阻塞原因尝试登录从熟悉的位置或设备。


3. **排除策略**-如果您认为您登录的策略的当前配置导致问题的特定用户，您可以从它排除用户。 请参阅[登录风险策略](active-directory-identityprotection.md#sign-in-risk-policy)的更多详细信息。
 
4. **禁用策略**-如果您认为您的策略配置导致了所有用户的问题，则可以禁用该策略。 请参阅[登录风险策略](active-directory-identityprotection.md#sign-in-risk-policy)的更多详细信息。


## <a name="unblocking-accounts-at-risk"></a>自负的解锁帐户

若要解除锁定风险帐户，您有以下选择︰

1. **重置密码**-您可以重置用户的密码。 请参阅详细信息[手动安全密码重置](active-directory-identityprotection.md#manual-secure-password-reset)。

2. **消除所有的风险事件**的用户风险策略阻止用户，如果阻止访问的配置的用户风险级别已经达到。 您可以减少用户的风险级别，通过手动关闭报告风险事件。 有关详细信息，请参阅[手动关闭风险事件](active-directory-identityprotection.md#closing-risk-events-manually)。

3. **排除策略**-如果您认为您登录的策略的当前配置导致问题的特定用户，您可以从它排除用户。 [用户风险策略](active-directory-identityprotection.md#user-risk-policy)的更多详细信息，请参阅。
 
4. **禁用策略**-如果您认为您的策略配置导致了所有用户的问题，则可以禁用该策略。 [用户风险策略](active-directory-identityprotection.md#user-risk-policy)的更多详细信息，请参阅。




## <a name="next-steps"></a>下一步行动

 您想要知道更多关于 Azure AD 身份保护吗？ 签出[Azure 活动目录标识保护](active-directory-identityprotection.md)。
 

