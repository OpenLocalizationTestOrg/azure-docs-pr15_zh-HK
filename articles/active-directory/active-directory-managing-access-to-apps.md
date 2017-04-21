<properties
  pageTitle="管理应用程序使用 Azure AD 的访问 | Microsoft Azure"
  description="介绍如何 Azure Active Directory 使组织可以指定每个用户有权访问的应用程序。"
  services="active-directory"
  documentationCenter=""
  authors="femila"
  manager="femila"
  editor=""/>

 <tags
  ms.service="active-directory"
  ms.workload="identity"
  ms.tgt_pltfrm="na"
  ms.devlang="na"
  ms.topic="article"
  ms.date="10/13/2016"
  ms.author="femila"/>


# <a name="managing-access-to-apps"></a>对应用程序的管理访问权限

进行访问的权限管理和使用情况的评估，报告继续后应用程序集成到您的组织的身份识别系统是一项挑战。 在许多情况下，IT 管理员或支持人员联系需要采取持续积极的管理对您的应用程序的访问。 有时，工作分配被通过常规或部门的 IT 团队。 通常情况下，分配决定旨在委派给业务决策人员，要求其批准才能使 IT 工作分配。  其他公司投资与现有自动身份和访问管理系统，如基于角色的访问控制 (RBAC) 或基于属性的访问控制 (ABAC) 的集成。 集成和规则开发往往是专门而且昂贵。 监视或报告任何一种管理方法是自己独立、 成本高昂而且复杂的投资。

## <a name="how-does-azure-active-directory-help"></a>Azure Active Directory 如何帮助？

 Azure 的广告支持广泛访问管理配置的应用程序，使公司能够很容易地实现从自动、 基于属性的赋值 （ABAC 或 RBAC 方案） 通过委派和包括管理员管理的权限访问策略。 使用 Azure 的广告，要可以很容易地实现复杂的策略，结合单个应用程序的多个管理模型并甚至可以重新使用管理规则跨应用程序具有相同的访问群体。

 - [添加新组件或现有应用程序](active-directory-sso-integrate-saas-apps.md)


 Azure AD 的应用程序分配重点介绍两种主要的分配模式︰

- **单个工作分配**IT 管理员提供目录全局管理员权限可以选择单个的用户帐户，并授予他们访问该应用程序。
- **基于组的分配 （支付只有 Azure 广告）**IT 管理员提供目录全局管理员权限可以将组分配给应用程序。 它们是否组成员的用户尝试访问该应用程序的时间取决于特定用户的访问权限。 换句话说，管理员可以有效地创建分配规则，指出"当前已分配的组成员有权访问应用程序"。 使用此工作分配的选项，管理员可以受益于任何 Azure AD 组管理选项，包括[基于属性的动态组](active-directory-accessmanagement-manage-groups.md)、 外部系统组 （例如，内部部署 Active Directory 或工作日） 或管理员管理或自我 service 管理组。 一个组可以轻松地分配到多个应用程序，确保工作分配关联的应用程序可以共享分配规则，从而减少总体管理的复杂性。 请注意该嵌套的组成员身份组基于分配给应用程序目前不支持。

使用这两种工作分配模式，管理员可以实现任何理想的分配管理办法。

使用 Azure 广告，使用和分配报告完全集成，使管理员能够轻松地报告工作分配状态，分配错误和甚至使用。

## <a name="complex-application-assignment-with-azure-ad"></a>Azure AD 具有复杂的应用程序分配

考虑应用程序如队伍。 在许多组织中，主要由市场营销和销售组织使用销售队伍。 通常情况下，市场营销团队的成员具有高特权级别访问销售队伍，销售团队的成员具有有限的访问权限时。 在许多情况下，信息工作者广泛人口限制了对访问该应用程序。 这些规则的例外情况使问题复杂化。 它通常是 prerogative 的市场营销或销售领导小组，以授予用户访问权或更改其角色独立于这些一般规则。

使用 Azure 的广告，像队伍的应用程序可以预先配置单一登录 (SSO) 和自动化的资源调配。 一旦配置应用程序，管理员可以创建并分配到相应的组的一次性操作。 在此示例中，管理员可以执行以下任务︰

- 可以定义[动态组](active-directory-accessmanagement-manage-groups.md)来自动表示使用属性，如部门或角色的市场营销和销售团队的所有成员︰

    - 市场营销组的所有成员将都分配给队伍中的"市场营销"角色

    - 销售团队组将分配给队伍中的"销售"角色的所有成员。 进一步优化可以使用多个组表示分配给不同的销售队伍角色的区域销售团队。

- 若要启用异常机制，可以为每个角色创建自助服务组。 例如，"销售市场异常"组可以创建作为一个自助服务的组。 组可以分配给销售市场营销角色，所有者可以进行市场营销的领导团队。 市场营销的领导团队的成员可以添加或删除用户、 设置联接策略，或甚至批准或拒绝单个用户请求加入。 这是通过不需要专门的训练的所有者或成员的信息工作人员适当经验支持的。

在这种情况下，所有指派的用户将自动调配到销售队伍，在添加到不同的组，其角色分配将更新在队伍中。 用户将能够发现和访问销售队伍，通过 Microsoft 应用程序访问面板，Office web 客户端，或甚至通过导航到其组织队伍登录页。 管理员可以轻松查看使用 Azure AD 报告的使用情况和工作分配状态。

管理员可以使用[Azure AD 条件访问](active-directory-conditional-access.md)设置访问策略，针对具体的角色。 这些策略可包括是否访问之外的企业环境和更多因素身份验证或设备需求，以实现在各种情况下的访问。

## <a name="how-can-i-get-started"></a>如何开始？

首先，如果您还未使用 Azure 广告，您是 IT 管理员︰

 - [试一下 ！](https://azure.microsoft.com/trial/get-started-active-directory/) -您可以注册免费 30 天试用版今天和部署第一个云解决方案时使用此链接在 5 分钟内

启用帐户共享的 azure AD 功能包括︰

- [组分配](active-directory-accessmanagement-self-service-group-management.md)
- 添加到 Azure AD 的应用程序
- 快速入门工作分配
- 应用程序分配的常见问题解答
- [应用程序使用情况仪表板/报告](active-directory-passwords-get-insights.md)

## <a name="where-can-i-learn-more"></a>在了解更多？

- [在 Azure 的 Active Directory 中的应用程序管理的文章索引](active-directory-apps-index.md)
- [保护应用程序具有访问权限的条件](active-directory-conditional-access.md)
- [自助服务组管理/SSAA](active-directory-accessmanagement-self-service-group-management.md)
