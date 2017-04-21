<properties
    pageTitle="Azure Active Directory 混合标识设计考虑事项--确定身份需求 |Microsoft Azure"
    description="标识将引导您定义所需的混合标识设计的公司的业务需求。"
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

# <a name="determine-identity-requirements-for-your-hybrid-identity-solution"></a>确定您的混合身份解决方案标识要求
在设计一个混合标识解决方案的第一步是确定将利用此解决方案的业务组织的要求。  混合身份启动作为支持角色 （它支持所有其他的云解决方案通过提供身份验证），并继续提供新的和有趣的功能，新的工作负载，为用户解除锁定。  这些工作负载或您想要采用为您的用户的服务将决定混合标识设计的要求。  这些服务和工作负载需要利用内部部署的混合身份和在云中。  

您需要通过了解它是什么企业的这些关键方面转现在要求和公司对未来的计划。 如果没有混合标识设计的长期战略的可见性，很可能随着业务需求而增长和改变不会可扩展解决方案。   T 他图表下方显示混合标识体系结构和已被解锁用户工作负载的一个示例。 这只是一个示例可以解除锁定并附带标识的实体的混合策略的所有新可能性。 
 
一些属于混合标识体系结构的组件![](./media/hybrid-id-design-considerations/hybrid-identity-architechture.png)

## <a name="determine-business-needs"></a>确定业务需求
每个公司会有不同的要求，即使这些公司属于同一行业，实际的业务需求可能有所不同。 您仍可以利用来自业界最佳做法但最终将引导您定义所需的混合标识设计的公司的业务需求。 

请务必回答以下问题来确定您的业务需求︰

- 贵公司希望降低 IT 运营成本？
- 贵公司是否希望保护云资产 （SaaS 应用程序、 基础结构）？
- 贵公司是否希望实现您的 IT 现代化？
  - 更移动而且要求很高，您的用户是 IT 部门创建到 DMZ 允许不同类型的通信，以访问其他资源的例外？
  - 贵公司是否有旧式应用程序发布到这些现代的用户所需，但不是可以很容易地重写？
  - 贵公司是否需要完成所有这些任务并将其放在控制之下？
- 公司希望保护用户的标识，并通过将新的工具，利用微软 Azure 的安全专业技能内部的专业技能，从而降低风险？
- 公司正试图摆脱内部部署的可怕"外部"帐户并将它们移动到云，它们将不再内部环境内休眠的威胁？

## <a name="analyze-on-premises-identity-infrastructure"></a>分析内部身份基础结构
现在，您已经了解有关您的公司的业务需求，您需要评估内部身份基础结构。 这种评估对于定义技术要求进行集成到云的身份识别管理系统当前标识解决方案至关重要。 请务必回答以下问题︰

- 哪种身份验证和授权解决方案 does 您的公司使用内部部署？ 
- 贵公司目前是否有任何内部同步服务？
- 贵公司是否使用任何第三方身份提供程序 (IdP)？

您还需要了解您的公司可能具有的云服务。 执行评估，以了解当前 SaaS、 IaaS 或 PaaS 模型在环境中集成是非常重要的。 请确保此评估过程中回答以下问题︰
- 贵公司是否有任何与云服务提供商的集成？
- 如果是的正在使用哪些服务？
- 这种集成目前正在生产或它是指导？


>[AZURE.NOTE]
如果您不具有所有应用程序的准确映射和云服务，您可以使用云应用程序发现工具。 此工具可以为您的 IT 部门提供深入了解您所在公司的所有业务和消费者云应用程序。 这样，就比以往任何时候都要发现影子 IT 在组织中，包括使用模式和任何用户访问您的云应用程序的详细信息。 若要访问此工具转到[https://appdiscovery.azure.com](https://appdiscovery.azure.com/)

## <a name="evaluate-identity-integration-requirements"></a>身份集成需求评估
接下来，您需要评估标识集成需求。 这种评估很重要，以定义用户的身份验证、 在云中外观组织的存在、 组织可授权和用户体验打算进行的技术要求。 请务必回答以下问题︰

- 联合身份验证和 / 或标准身份验证将使用您的组织？
- 是联盟的要求？  原因如下︰
 - 基于 Kerberos 的 SSO
 - 您的公司都有内部生成的应用程序 （或者内部或第三方） 使用 SAML 或类似的联合身份验证功能。
 - 通过智能卡的 MFA。 RSA SecurID，等等。
 - 客户端访问规则，解决下面的问题︰
     1. 我可以阻止所有外部访问 Office 365 提供基于客户端的 IP 地址吗？
     1. 我可以阻止所有外部访问 Office 365，除 Exchange ActiveSync 吗？
     1. 我可以阻止所有外部访问 Office 365 除外 （SPO OWA） 基于浏览器的应用程序吗
     1. 可以指定 AD 组的成员阻止所有外部访问 Office 365 吗
- 安全审核/问题
- 在联合身份验证中的现有投资
- 我们的组织对我们在云环境中的域将使用什么名称？
- 组织是否具有自定义的域？
    1. 为该域公共 dns 可很容易地验证？
    1. 如果不是，然后有可用于在 AD 中注册备用 UPN 公共域？
- 是用户标识符云表示的一致吗？ 
- 组织是否有需要与云服务的集成的应用程序？
- 组织有多个域，它们都将使用标准或联合身份验证？

## <a name="evaluate-applications-that-run-in-your-environment"></a>评估您的环境中运行的应用程序
现在，有关于您的内部和云基础结构，您需要评估在这些环境中运行的应用程序。 这种评估是重要定义将这些应用程序到云的身份识别管理系统集成的技术要求。 请务必回答以下问题︰

- 我们的应用程序在其中生存？
- 将用户访问内部部署的应用程序？  云中的吗？ 还是两个？
- 是否有计划采取现有的应用程序工作负载，然后将它们移到云？
- 是否有计划开发新的应用程序将保存任何一个在内部或在将使用云中云身份验证？

## <a name="evaluate-user-requirements"></a>评估用户需求
此外需要评估用户需求。 这种评估定义是非常重要的步骤，将所需的安置和协助用户，因为他们转换到云。 请务必回答以下问题︰

- 将用户访问部署应用程序？
- 将用户访问云中的应用程序？
- 通常是如何处理用户登录到其内部环境？
- 如何将用户登录到云？

>[AZURE.NOTE]
请确保每个回答的记录笔记并了解答案背后的基本原理。 [确定事件响应要求](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)超出可用选项以及每个选项的优缺点。  通过无回答的问题会选择哪个选项可以最好地满足您的业务需要。

## <a name="next-steps"></a>下一步行动
[确定目录同步的要求](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)

## <a name="see-also"></a>请参见
[设计考虑事项概述](active-directory-hybrid-identity-design-considerations-overview.md)
