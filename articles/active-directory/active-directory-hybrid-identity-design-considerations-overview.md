<properties
    pageTitle="Azure Active Directory 混合标识设计考虑事项--概述 |Microsoft Azure"
    description="概述和混合标识设计注意事项手册的内容映射"
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

# <a name="azure-active-directory-hybrid-identity-design-considerations"></a>Azure 的 Active Directory 混合标识设计注意事项

基于使用者的设备正在急剧增加的企业世界，和基于云的软件作为服务 (SaaS) 应用程序可以很容易地采用。 因此，保持内部数据中心和云平台的用户应用程序的访问控制一项挑战。  Microsoft 的标识解决方案跨内部和基于云的能力，创建单个用户的身份进行身份验证和授权的所有资源，而不用考虑位置。 我们所说的这个混合身份。 有不同的设计和配置选项使用 Microsoft 解决方案的混合身份，并在某些情况下，可能很难确定哪些组合最符合您组织的需要。 此混合标识设计考虑事项指南将帮助您了解如何设计一个混合身份解决方案，最适合业务和技术需求，为您的组织。  本指南将详细描述一系列步骤和任务可以遵循的操作来帮助您设计的混合身份解决方案能够满足您的组织的独特需求。 在整个步骤和任务，指南 》 将提供相关技术和功能选项组织到满足功能和服务质量 （例如，可用性、 可伸缩性、 性能、 可管理性和安全性） 级别要求。 具体来说，混合标识设计注意事项指南目的是回答以下问题︰ 

- 需要什么问题要提出和回答问题来驱动的混合标识特定方案设计的技术或问题域最能满足我的需求？
- 我应该完成什么的活动序列设计混合标识解决方案的技术或问题域？ 
- 哪种混合标识技术和配置选项是否可用来帮助我满足我的需求？ 这样我可以为我的公司选择最佳的选项，这些选项之间找到平衡是什么？


## <a name="who-is-this-guide-intended-for"></a>本指南适用于谁？
 首席信息官，CITO、 首席标识设计师、 企业架构师和 IT 架构师负责设计混合身份解决方案对于中型或大型组织。

## <a name="how-can-this-guide-help-you"></a>本指南如何帮助您？ 
您可以使用本指南可以了解如何设计一个混合身份解决方案，能够与您当前的内部标识解决方案集成基于云的身份识别管理系统。 下图显示示例混合身份解决方案，使 IT 管理员来管理集成其当前的 Windows 服务器的 Active Directory 解决方案位于内部使用 Microsoft Azure 活动目录，以便用户能够使用跨位于云和内部部署的应用程序的单一登录 (SSO)。

![](./media/hybrid-id-design-considerations/hybridID-example.png)


上图是一种混合标识解决方案，它利用云服务将与内部功能集成，以便提供一个体验到最终用户身份验证过程，并促进 IT 管理这些资源。 虽然这可能是很常见的情况，每个组织的混合标识设计很可能不同于根据不同要求图 1 所示的示例。 本指南提供一系列的步骤，您可以按照设计的混合身份解决方案能够满足您的组织的独特需求的任务。 以下步骤和任务，在整个指南 》 以满足功能和为您的组织的服务质量级别要求提供的相关技术和功能的可用选项。

**假设**︰ 您有与 Windows 服务器，Active Directory 域服务和 Azure Active Directory 的一些经验。 在本文中，我们假定您正在寻找这些解决方案可以如何满足您的业务需要靠自己，或在一个集成的解决方案中。

## <a name="design-considerations-overview"></a>设计考虑事项概述
本文档提供了一套步骤，您可以按照能够设计最符合您要求的混合标识解决方案的任务。 这些步骤均以有序序列。 在后续步骤中，您学习了设计考虑事项可能需要您更改您所做的决策早期步骤，但是，由于冲突的设计选择。 每次尝试以提醒您潜在的设计冲突，整篇文档。 

在您将到达最能满足您的要求只有在一步步地根据需要将所有文档中的考虑因素纳入多次循环后的设计。 

| 混合的标识阶段                                             | 主题列表                                                                                                                                                                                       |
|-------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 确定标识要求                                   | [确定业务需求](active-directory-hybrid-identity-design-considerations-business-needs.md)<br> [确定目录同步的要求](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)<br> [确定多因素身份验证要求](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)<br> [定义的混合标识采用策略](active-directory-hybrid-identity-design-considerations-identity-adoption-strategy.md)                       |
| 增强数据安全性通过强标识解决方案的计划 | [确定数据保护需求](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md) <br> [确定内容管理需求](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)<br> [确定访问控制要求](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)<br> [确定事件响应要求](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) <br> [定义数据保护战略](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md)  |
| 对于混合身份生命周期的计划                                | [确定混合身份管理任务](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md) <br> [同步管理](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)<br> [确定混合身份管理采用策略](active-directory-hybrid-identity-design-considerations-lifecycle-adoption-strategy.md) |     


##<a name="download-this-guide"></a>下载本指南
从[Technet 库](https://gallery.technet.microsoft.com/Azure-Hybrid-Identity-b06c8288)，可以下载 pdf 版本的混合标识设计考虑事项指南。 

                                                             
