<properties
    pageTitle="登录体验 Azure AD 身份保护 |Microsoft Azure"
    description="缓解或修正用户身份保护的或在多因素身份验证需要的策略时，请提供用户体验的概述。"
    services="active-directory"
    keywords="azure 的活动目录身份保护，云应用程序发现，管理应用程序、 安全性、 风险、 风险程度、 漏洞、 安全策略"
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
    ms.date="08/16/2016"
    ms.author="markvi"/>

# <a name="sign-in-experiences-with-azure-ad-identity-protection"></a>Azure AD 身份保护登录体验

使用 Azure 活动目录身份保护，您可以︰

- 要求用户注册的多因素身份验证

- 处理危险号接和受害的用户

系统响应这些问题具有对用户的登录体验的影响，因为只需直接签名中通过提供用户名和密码再也不会有可能。 安全地获取用户所需的其他步骤回到业务。

本主题提供对所有可能发生的情况下用户的登录体验的概述。

**多因素身份验证**

- 多因素身份验证注册



**在危险的符号**

- 危险的符号中恢复

- 危险登录被阻止

- 在危险符号中的多因素身份验证注册
 

**用户风险**

- 损坏的帐户恢复

- 受害的帐户已被阻止




## <a name="multi-factor-authentication-registration"></a>多因素身份验证注册

用户可以自行恢复时的同时，受危害的帐户恢复和危险签入流，最佳的用户体验。 如果用户多因素身份验证的注册，他们已与可用于传递安全挑战其帐户关联的电话号码。 没有帮助台或管理员参与才可恢复从帐户泄漏。 因此，强烈建议您注册的多因素身份验证的用户获取。 

管理员可以︰

- 将需要设置额外的安全验证其帐户的用户策略设置。 
- 允许跳多因素身份验证注册过达 30 天，如果他们想要让用户在注册之前的过渡期。

**多因素身份验证登记都有以下三个步骤︰**

1. 在第一步，用户获取有关要求设置为多因素身份验证的帐户的通知。 

    ![补救措施](./media/active-directory-identityprotection-flows/140.png "补救措施")


2. 若要设置多因素身份验证，您需要让系统知道如何联系。

    ![补救措施](./media/active-directory-identityprotection-flows/141.png "补救措施")
 
3. 系统将提交的一项难题您和您需要作出响应。

    ![补救措施](./media/active-directory-identityprotection-flows/142.png "补救措施")

 



## <a name="risky-sign-in-recovery"></a>危险的符号中恢复

管理员已配置登录风险的策略，当用户尝试登录时被通知受影响的用户。 

**危险的签到流有两个步骤︰** 

1. 寻常发现有关其登录，如签名从新位置、 设备或应用程序会通知用户。 

    ![补救措施](./media/active-directory-identityprotection-flows/120.png "补救措施")

2. 用户需要证明自己的身份，通过解决安全难题。 如果用户多因素身份验证为注册他们往返需要为他们的电话号码的安全代码。 因为这是只在一个危险符号和不受威胁的帐户，用户无需更改此流中的密码。 

    ![补救措施](./media/active-directory-identityprotection-flows/121.png "补救措施")



 
## <a name="risky-sign-in-blocked"></a>危险登录被阻止
管理员还可以选择登录风险策略设置为阻止用户通过登录根据风险级别。 要解锁，最终用户必须联系管理员或帮助台，或者他们可以尝试登录从熟悉的位置或设备。 自我恢复解决多因素身份验证不是一个选项，在这种情况下。

![补救措施](./media/active-directory-identityprotection-flows/200.png "补救措施")




## <a name="compromised-account-recovery"></a>损坏的帐户恢复

满足用户的用户配置用户风险安全策略后，风险策略中指定的级别 (并因此假定破坏) 必须经过用户破坏恢复流之前他们可以登录。 

**用户破坏恢复流具有三个步骤︰**

1. 其帐户的安全风险是由于可疑活动或泄漏的凭据，则通知用户。

    ![补救措施](./media/active-directory-identityprotection-flows/101.png "补救措施")

2.  用户需要证明自己的身份，通过解决安全难题。 如果用户多因素身份验证为注册他们能够自我恢复不会受到攻击。 他们将需要往返于他们的电话号码的安全代码。 

    ![补救措施](./media/active-directory-identityprotection-flows/110.png "补救措施")


3.  最后，将强制用户更改其密码，因为其他人可能已经对其帐户的访问。 这种体验的屏幕抓图如下。
 
    ![补救措施](./media/active-directory-identityprotection-flows/111.png "补救措施")



## <a name="compromised-account-blocked"></a>受害的帐户已被阻止 

获取被取消阻止用户风险安全策略阻止用户，用户必须与管理员联系或帮助台。 自我恢复解决多因素身份验证不是一个选项，在这种情况下。


![补救措施](./media/active-directory-identityprotection-flows/104.png "补救措施")



 
## <a name="reset-password"></a>重置密码

如果受害的用户阻止登录，管理员可以为它们生成一个临时密码。 用户必须在下次登录过程中更改其密码。

![补救措施](./media/active-directory-identityprotection-flows/160.png "补救措施")


 




 

## <a name="see-also"></a>请参见

- [Azure 的 Active Directory 身份保护](active-directory-identityprotection.md) 