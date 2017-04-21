<properties
    pageTitle="Azure Active Directory 混合标识设计考虑事项--确定内容管理需求 |Microsoft Azure"
    description="提供了深入了解，以确定您的企业内容管理要求。 通常当用户有他自己的设备他可能还会交替按照他使用的应用程序的多个凭据。 务必要区分哪些内容创建使用个人而不是创建使用企业凭据的凭据。 标识解决方案应该能够与云交互服务，以提供无缝体验给最终用户，同时确保其隐私并提高防止数据泄露。"
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

# <a name="determine-content-management-requirements-for-your-hybrid-identity-solution"></a>确定为混合标识解决方案的内容管理需求

了解为您的企业内容管理需求可能会直接影响您要使用哪种混合标识解决方案的决策。 使用多个设备的激增和用户能够将他们自己的设备 ([BYOD](http://aka.ms/byodcg))，公司必须保护自己的数据，但它还必须保留用户的个人信息保持不变。 通常当用户有他自己的设备他可能还会交替按照他使用的应用程序的多个凭据。 务必要区分哪些内容创建使用个人而不是创建使用企业凭据的凭据。 标识解决方案应该能够与云交互服务，以提供无缝体验给最终用户，同时确保其隐私并提高防止数据泄露。 

标识解决方案将通过不同的技术控制利用以提供内容管理，如下图所示︰
 
![](./media/hybrid-id-design-considerations/securitycontrols.png)

**将利用您的身份管理系统的安全控制**

一般情况下，内容管理要求将利用您的身份管理系统在以下方面︰

- 隐私︰ 标识用户，拥有资源和应用适当的控制，以保持完整性。
- 数据分类︰ 标识用户或组并对其分类根据对象的访问级别。 
- 数据泄露保护︰ 负责保护数据以免泄漏的安全控制需要与用于验证用户的身份标识系统进行交互。 这也是重要的审核跟踪目的。

>[AZURE.NOTE]
阅读有关最佳做法的详细信息的[云准备情况的数据分类](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf)和数据分类的准则。

当规划混合身份解决方案确保，根据您的组织要求回答以下问题︰

- 贵公司是否有到位，能够强制实施数据隐私安全控制？
 - 如果是，将这些安全控制能够与您打算采用混合身份解决方案集成在一起吗？
- 贵公司是否使用数据分类？
 - 如果是的话，是能与您打算采用混合身份解决方案集成当前解决方案？
- 贵公司目前是否用于数据泄露任何解决方案？ 
 - 如果是的话，是能与您打算采用混合身份解决方案集成当前解决方案？
- 贵公司是否需要审核对资源的访问？
 - 如果是的话，何种资源？
 - 如果是信息的的话，什么级别是信息的有必要？
 - 如果是的话，审核日志必须所在的位置吗？ 内部或在云环境中？
- 贵公司是否需要加密任何电子邮件中包含敏感数据 (Ssn，信用卡卡号，等等)？
- 贵公司是否需要加密所有文档/内容与外部业务合作伙伴共享？
- 没有您的公司是否需要强制实施公司策略在某些类型的电子邮件 （所有没有答复，不转发）？
 
>[AZURE.NOTE]
请确保每个回答的记录笔记并了解答案背后的基本原理。 [定义数据保护战略](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md)超出可用选项以及每个选项的优点/缺点。  通过无回答的问题会选择哪个选项可以最好地满足您的业务需要。


## <a name="next-steps"></a>下一步行动
[确定访问控制要求](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)

## <a name="see-also"></a>请参见
[设计考虑事项概述](active-directory-hybrid-identity-design-considerations-overview.md)
