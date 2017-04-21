<properties
    pageTitle="Azure 的活动目录标识保护通知 |Microsoft Azure"
    description="了解如何通知支持调查活动。"
    services="active-directory"
    keywords="azure 的活动目录身份保护，云应用程序发现，管理应用程序、 安全性、 风险、 风险程度、 漏洞、 安全策略"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="markvi"/>

#<a name="azure-active-directory-identity-protection-notifications"></a>Azure 的活动目录标识保护通知 


Azure AD 身份保护发送两种类型的自动的通知电子邮件来帮助您管理用户风险和风险事件︰

- 用户危害预警电子邮件

- 每周摘要电子邮件

## <a name="user-compromised-alert-email"></a>用户危害预警电子邮件

Azure AD 身份保护标识帐户，如破坏，则会生成用户泄露的电子邮件通知。 电子邮件包含一个链接标记为风险标识保护仪表板中的报表的用户。 我们建议您立即调查通知的破坏。


## <a name="weekly-digest-email"></a>每周摘要电子邮件

每周摘要电子邮件包含新风险事件的摘要。<br>
其中包括︰

- 在风险中的用户
- 可疑活动
- 检测到的漏洞
- 身份保护中的相关报表的链接


<br>
![补救措施](./media/active-directory-identityprotection-notifications/400.png "Remediation")
<br> 

您可以切换发送每周摘要电子邮件。
<br><br>
![用户风险](./media/active-directory-identityprotection-notifications/62.png "User risks")
<br>
 

**若要打开相关的配置对话框**︰

1. 在**Azure AD 身份保护**刀片式服务器，单击**设置**。
<br><br>
![用户风险策略](./media/active-directory-identityprotection-notifications/401.png "User risk policy")
<br>

2. 在**常规**部分，单击**通知**。
<br><br>
![用户风险策略](./media/active-directory-identityprotection-notifications/405.png "User risk policy")
<br>




## <a name="see-also"></a>请参见

- [Azure 的 Active Directory 身份保护](active-directory-identityprotection.md) 

