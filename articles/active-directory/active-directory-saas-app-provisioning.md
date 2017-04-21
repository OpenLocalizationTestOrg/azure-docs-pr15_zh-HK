<properties
    pageTitle="自动化的 SaaS 应用程序用户在 Azure AD 供应 |Microsoft Azure"
    description="介绍如何使用 Azure 广告自动供应，消除资源调配，并不断更新跨多个第三方 SaaS 应用程序的用户帐户。"
    services="active-directory"
    documentationCenter=""
    authors="asmalser-msft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="02/09/2016"
    ms.author="asmalser-msft"/>

#<a name="automate-user-provisioning-and-deprovisioning-to-saas-applications-with-azure-active-directory"></a>自动化资源调配和撤消对 Azure Active Directory 的 SaaS 应用程序的用户

##<a name="what-is-automated-user-provisioning-for-saas-apps"></a>什么是自动化用户对于 SaaS 应用程序资源调配？

Azure 活动目录 (AD Azure) 使您能够自动创建、 维护和删除等收存箱、 销售队伍、 ServiceNow，和更多的云 ([SaaS](https://azure.microsoft.com/overview/what-is-saas/)) 应用程序中的用户标识。

**下面是此功能使您可以执行的一些示例︰**

- 自动创建新帐户中右的 SaaS 应用程序为新用户加入您的团队。
- 人们不可避免地离开团队时，自动停用从 SaaS 应用程序的帐户。
- 确保 SaaS 应用程序中的身份信息都保持最新的基于目录中的更改。
- 规定非用户对象，如组，来支持它们的 SaaS 应用程序。

**自动化的用户供应还包括以下功能︰**

- 能够符合 Azure 的广告之间的现有标识和 SaaS 应用程序。
- 帮助 Azure 广告的自定义选项适合您的组织当前使用的 SaaS 应用程序的当前配置。
- 可选的电子邮件通知资源调配错误。
- 如何进行监视和故障排除的报告和活动日志。

##<a name="why-use-automated-provisioning"></a>为什么要使用自动化资源调配？

使用此功能的一些常见动机包括︰

- 若要避免成本、 提高效率和人为错误与资源调配过程的手册。
- 保护您的组织通过在离开组织时立即从关键的 SaaS 应用程序删除用户的身份。
- 若要轻松地导入特定的 SaaS 应用程序的用户的大容量数字。
- 若要使资源调配解决方案运行相同的应用程序访问策略定义的 Azure AD Single Sign-on 从便利中尽情享受。

##<a name="frequently-asked-questions"></a>常见问题

**频率 does Azure 的广告目录将更改写入到 SaaS 应用程序？**

Azure AD 更改检查每隔五至十分钟。 如果 SaaS 应用程序返回多个错误 （如无效管理员凭据的情况下），Azure 广告将逐渐降低到每天在修复错误之前的一次对其频率。

**若要提供我的用户需要多长时间？**

增量更改几乎立即发生，但如果您正在设置您的目录的大部分，然后在它所依存的用户和组，您可以。 小目录需要仅几分钟，中等规模的目录可能需要几分钟，非常大的目录可能需要几个小时。

**如何跟踪当前设置作业的进度？**

您可以查看帐户调配报告在您的目录的报告部分下。 另一种方法是页面的访问的 SaaS 应用程序的配置，并看看下面底部附近的"集成状态"部分的仪表板选项卡。

**我如何知道用户不能获得正确的设置？**

末尾的资源调配配置向导有是供应失败的电子邮件通知订阅选项。 您还可以查看资源调配错误报告，以查看哪些用户未能调配及原因。

**可以 Azure 的广告将更改写入从 SaaS 应用程序返回到目录？**

对于大多数的 SaaS 应用程序资源调配是仅出站这意味着，用户从目录写入到应用程序中，并从应用程序的更改不能恢复写入到目录。 为[工作日](https://msdn.microsoft.com/library/azure/dn762434.aspx)，但是，资源调配是仅入站，这意味着，到工作日，从目录中导入用户，同样，更改目录中的执行不获得写回到工作日。

**如何提交给工程团队的反馈？**

请与我们联系通过[Azure Active Directory 反馈论坛](https://feedback.azure.com/forums/169401-azure-active-directory/)。

##<a name="how-does-automated-provisioning-work"></a>自动资源调配的工作方式是怎样的？

Azure 广告规定 SaaS 应用程序的用户连接到资源调配提供的每个应用程序供应商的终结点。 这些终结点使 Azure 的广告，若要以编程方式创建、 更新和删除用户。 下面是 Azure AD 自动化资源调配所需的其他步骤的简要概述。

1. 启用的第一次应用程序资源调配时，会执行以下操作︰
 - Azure 的广告将尝试匹配在 SaaS 应用程序目录中其相应身份的任何现有用户。 当用户相匹配时，则它们会*不*会自动启用单一登录。 为了使用户能够访问的应用程序，它们必须显式分配到应用程序在 Azure 的广告，直接或通过组成员身份。
 - 如果您已经指定哪些用户应该分配给应用程序，并且 Azure 广告未能找到现有帐户的用户，Azure 的广告将应用程序中为他们设置新帐户。
2. 完成初始同步后按上文所述，Azure 广告将检查每隔 10 分钟进行以下更改︰
 - 如果新用户已分配给应用程序 （直接或通过组成员身份），然后他们将提供 SaaS 应用程序中的新帐户。
 - 如果已删除用户的访问权限，则他们在 SaaS 应用程序的帐户将被标记为禁用 （用户将永远不会完全删除，其中防止数据丢失在配置错误时）。
 - 如果用户最近分配到该应用程序，他们已有一个帐户在 SaaS 应用程序中，该帐户将标记为启用，和它们是否过时的目录比较，可能会更新特定用户的属性。
 - 如果目录中更改了用户的信息 （如电话号码、 办公地点等），那么该信息也将更新在 SaaS 应用程序中。

有关详细信息的属性如何映射之间 Azure 的 AD 和 SaaS 应用程序，[自定义属性映射](active-directory-saas-customizing-attribute-mappings.md)，请参阅文章。

##<a name="list-of-apps-that-support-automated-user-provisioning"></a>支持自动化的用户提供的应用程序的列表

单击以查看教程，说明如何配置自动化资源调配，它的应用程序︰

- [框中](http://go.microsoft.com/fwlink/?LinkId=286016)
- [Citrix GoToMeeting](http://go.microsoft.com/fwlink/?LinkId=309580)
- [同时存在](http://go.microsoft.com/fwlink/?LinkId=309575)
- [Docusign](http://go.microsoft.com/fwlink/?LinkId=403254)
- [企业的收存箱](http://go.microsoft.com/fwlink/?LinkId=309581)
- [Google 应用程序](http://go.microsoft.com/fwlink/?LinkId=309577)
- [Jive](http://go.microsoft.com/fwlink/?LinkId=309591)
- [销售队伍](http://go.microsoft.com/fwlink/?LinkId=286017)
- [销售队伍沙盒](http://go.microsoft.com/fwlink/?LinkId=327869)
- [ServiceNow](http://go.microsoft.com/fwlink/?LinkId=309587)
- [工作日](http://go.microsoft.com/fwlink/?LinkId=690250)（入站资源调配）

为了使应用程序支持用户自动化资源调配，它必须首先提供必要允许自动执行创建、 维护和删除用户的外部程序的终结点。 因此，并不是所有的 SaaS 应用程序可以与此功能兼容。 对于支持此功能的应用程序，Azure 的广告工程团队将能够构建资源调配到这些应用程序，连接器，此项工作确定优先级别的当前客户和潜在客户的需要。

要联系 Azure 的广告工程团队申请为其他应用程序资源调配的支持，请提交[Azure Active Directory 反馈论坛](https://feedback.azure.com/forums/169401-azure-active-directory/)通过一条消息。

##<a name="related-articles"></a>相关的文章

- [在 Azure 的 Active Directory 中的应用程序管理的文章索引](active-directory-apps-index.md)
- [为用户提供自定义属性映射](active-directory-saas-customizing-attribute-mappings.md)
- [编写属性映射的表达式](active-directory-saas-writing-expressions-for-attribute-mappings.md)
- [作用域为用户提供的筛选器](active-directory-saas-scoping-filters.md)
- [如何使用 SCIM 以使用户和应用程序从 Azure 的 Active Directory 组的自动资源调配](active-directory-scim-provisioning.md)
- [帐户设置通知](active-directory-saas-account-provisioning-notifications.md)
- [有关如何集成 SaaS 应用程序的教程列表](active-directory-saas-tutorial-list.md)
