
<properties
    pageTitle="Azure Active Directory 混合标识设计考虑事项--确定访问控制要求 |Microsoft Azure"
    description="介绍了标识和识别的混合环境中的用户的资源访问权限要求的支柱。"
    documentationCenter=""
    services="active-directory"
    authors="billmath"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="determine-access-control-requirements-for-your-hybrid-identity-solution"></a>确定访问控制要求的混合身份解决方案
组织设计及其混合身份解决方案时他们可以使用这一机会来检查计划以使其对用户可用的资源的访问权限要求。 数据访问跨所有四个支柱的标识，它们是︰

- 管理
- 身份验证
- 授权
- 审核

后面的章节将介绍身份验证和授权的更多细节，管理和审核混合身份生命周期的一部分。 有关这些功能的详细信息请阅读[确定混合身份管理任务](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)。

>[AZURE.NOTE]
阅读有关每个这些支柱[四个支柱的标识-混合 it 时代的身份管理](http://social.technet.microsoft.com/wiki/contents/articles/15530.the-four-pillars-of-identity-identity-management-in-the-age-of-hybrid-it.aspx)。

## <a name="authentication-and-authorization"></a>身份验证和授权
有不同的身份验证和授权的方案，这些方案都由公司打算采用的混合身份解决方案必须满足的特定需求。 涉及企业对企业 (B2B) 通信的方案可以添加 IT 管理员获得的额外的挑战，因为它们将需要确保组织使用的身份验证和授权方法可以与其业务合作伙伴进行通信。 在设计过程中的身份验证和授权需求，确保回答以下问题︰

- 将您的组织身份验证和授权仅位于其身份管理系统的用户？
 - 是否有任何计划的 B2B 方案？
 - 如果是，您已经知道哪些协议 （SAML，OAuth，Kerberos、 标记或证书） 用于连接这两个公司？
- 要采用支持混合身份解决方案这些协议？

另一个需要考虑的要点是将在其中放置身份验证存储库中将使用的用户和合作伙伴和要使用的管理模型。 请考虑下面的两个核心选项︰
- 集中︰ 在此模型中的用户凭据、 策略和管理可以是集中式的内部部署或在云中。
- 混合︰ 在此模型中的用户凭据、 策略和管理将集中化的内部部署和复制在云中。

您的组织将采用哪种型号取决于他们的业务需求，想要回答以下问题来确定身份识别管理系统所在的位置以及要使用的管理模式︰

- 您的组织当前没有标识管理内部？
 - 他们打算让它如果是的？
 - 是否有任何法规遵从性要求，您的组织必须遵守该规定的身份识别管理系统所在的位置？
- 不会您的组织使用单一登录，位于应用程序部署，或者云吗？
 - 如果是，采用混合身份模型会影响这一过程？

## <a name="access-control"></a>访问控制
尽管身份验证和授权以允许访问企业数据通过用户的验证的核心元素，还有一点需要控制级别的访问权限，必须将这些用户和管理员访问的级别都对他们所管理的资源。 混合的标识解决方案必须能够提供细粒度访问资源、 委派和角色的基本访问控制。 确保有关访问控制，回答以下问题︰

- 贵公司是否有多个用户使用提升的权限来管理身份识别系统？
 - 如果是的话，每个用户是否需要相同的访问级别？
- 您的公司需要委派用户管理特定资源的访问？
 - 如果是的话，如何经常发生这种情况？
- 将您的公司需要集成内部和云之间的访问控制功能的资源？
- 您的公司需要限制对根据某些条件的资源的访问？
- 将您的公司有需要对某些资源的自定义控件访问的任何应用程序？
 - 如果是的话，这些应用程序的位置 (内部或在云中)？
 - 如果是的话，其中的那些目标位于资源 (内部或在云中)？

>[AZURE.NOTE]
请确保每个回答的记录笔记并了解答案背后的基本原理。 [定义数据保护战略](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md)超出可用选项以及每个选项的优点/缺点。  通过回答这些问题中，您将选择哪个选项可以最好地满足您的业务需求。

## <a name="next-steps"></a>下一步行动

[确定事件响应要求](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="see-also"></a>请参见
[设计考虑事项概述](active-directory-hybrid-identity-design-considerations-overview.md)
