<properties
    pageTitle="Azure Active Directory 混合标识设计考虑事项--确定多因素身份验证要求"
    description="有条件的访问控制，Azure Active Directory 检查挑选进行用户身份验证时，才允许访问该应用程序的特定条件。 一旦满足了这些条件，是经过身份验证的用户，并允许应用程序访问。"
    documentationCenter=""
    services="active-directory"
    authors="femila"
    manager="billmath"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="determine-multi-factor-authentication-requirements-for-your-hybrid-identity-solution"></a>确定为混合标识解决方案的多因素身份验证要求

在移动，用户访问数据和应用程序在云中，并从任何设备，与这个世界保护此信息已变得极为重要。  每一天没有有关安全漏洞的新标题。  虽然没有对此类违规不能保证，但多因素身份验证提供了额外的安全性，以防止这样的违规行为。
首先评估多因素身份验证的组织要求。 也就是说，什么组织试图保护。  这种评估是重要定义设置和启用的多因素身份验证的组织用户的技术要求。

>[AZURE.NOTE]
如果您还不熟悉 MFA 和它的作用，强烈建议您阅读的文章[Azure 多因素身份验证是什么？](../multi-factor-authentication/multi-factor-authentication.md)前继续阅读本部分。

请务必回答以下问题︰

- 您的公司在努力保护 Microsoft 应用程序？ 
- 这些应用程序发布的方式吗？
- 贵公司是否提供远程访问，允许员工访问内部部署的应用程序？

如果是，是什么类型的远程访问权限？此外需要评估访问这些应用程序的用户将位于其中。 这种评估是定义适当的多因素身份验证策略的另一个重要步骤。 请务必回答以下问题︰

- 用户将在其中找？
- 它们可以位于任何地方？
- 贵公司是否需要建立限制用户的位置？

一旦您理解了这些要求，很重要也计算多因素身份验证的用户的需求。 这种评估很重要，因为它将定义推出多因素身份验证的要求。 请务必回答以下问题︰

- 熟悉用户多因素身份验证？
- 一些用途需要提供额外的身份验证？  
 - 如果是的所有的时间，当从外部网络或访问特定的应用程序，或在其他情况下？
- 用户将需要有关如何设置和实现多因素身份验证的培训？
- 贵公司想要启用多因素身份验证的用户的关键方案有哪些？

在回答前一个问题之后, 将能够了解是否已经实现的多因素身份验证部署。 这种评估是重要定义设置和启用的多因素身份验证的组织用户的技术要求。 请务必回答以下问题︰

- 贵公司是否需要保护的 MFA 特权的帐户？
- 贵公司是否需要启用 MFA 对于某些应用程序出于法规遵从性方面的考虑？
- 贵公司是否需要为这些应用程序或只有管理员的所有合格用户启用 MFA？
- 是否需要有 MFA 始终处于启用状态，或仅当用户登录公司网络之外？


## <a name="next-steps"></a>下一步行动
[定义的混合标识采用策略](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)


## <a name="see-also"></a>请参见
[设计考虑事项概述](active-directory-hybrid-identity-design-considerations-overview.md)
