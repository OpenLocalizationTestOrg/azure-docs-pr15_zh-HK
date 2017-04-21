<properties
    pageTitle="Azure Active Directory 混合标识设计考虑事项--确定目录同步需求 |Microsoft Azure"
    description="确定哪些需求所需的同步之间的所有用户在 = 内部和面向企业的云。"
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

# <a name="determine-directory-synchronization-requirements"></a>确定目录同步的要求
同步的目的在于为用户提供在云中基于其内部标识的标识。 他们将使用同步的帐户进行身份验证或联合身份验证，用户仍需要在云环境中具有的标识。  该标识将需要进行维护和定期更新。  这些更新可以采取多种形式，从标题更改密码的更改。  

首先评估组织的内部标识解决方案和用户要求。 这种评估是定义如何创建和维护在云中的用户标识的技术要求。  对于大多数组织，Active Directory 是内部并将用户将本地目录同步的但是在某些情况下这不会这样。  

请务必回答以下问题︰


- 您是否有一个 AD 林中、 多个或无？
 - 多少的 Azure 的广告目录将您将同步到？
 
    1. 您正在使用筛选？
    2. 您有多个计划的 Azure AD 连接服务器吗？
  
- 当前没有同步工具内部？
  - 如果是的话，会您的用户如果用户标识虚拟目录/集成？
- 您是否有任何其他目录内部要同步 （如 LDAP 目录，人力资源数据库，等等）？
  - 是否要做任何 GALSync？
  - Upn，则您的组织中的当前状态是什么？ 
  - 有的用户进行身份验证的不同目录吗？
  - 贵公司是否使用 Microsoft Exchange？
    - 他们是否拥有混合 exchange 部署的计划？

现在，您已经了解有关您的同步要求，您需要确定哪一种工具就是为满足这些要求。  Microsoft 提供了多种工具来完成目录集成和同步。  请参阅[混合标识目录集成工具比较表](active-directory-hybrid-identity-design-considerations-tools-comparison.md)的详细信息。 
   
现在，您有您同步的需求以及将为您的公司实现此工具，您需要评估的应用程序使用这些目录服务。 这种评估是重要定义集成到云这些应用程序的技术要求。 请务必回答以下问题︰

- 将这些应用程序移动到云并使用该目录？
- 是否有需要同步到云中，因此这些应用程序可以成功地使用它们的特殊特性？
- 这些应用程序将需要重新编写，以利用云身份验证？
- 这些应用程序继续生存内部，当用户访问使用云标识它们吗？

您还需要确定安全性要求和约束目录同步。 这种评估是很重要，需要创建和维护在云中的用户的标识要求的列表。 请务必回答以下问题︰

- 其中将同步到服务器？
- 它将被加入域。
- 将服务器位于防火墙后面，如 DMZ 的受限网络上吗？
  - 您将无法打开所需的防火墙端口以支持同步？
- 您是否有同步服务器的灾难恢复计划？
- 您是否具有正确的权限，为所有林状结构要与同步的帐户？
 - 如果您的公司不知道这个问题的答案，[安装 Azure 活动目录同步服务](https://msdn.microsoft.com/library/azure/dn757602.aspx#BKMK_CreateAnADAccountForTheSyncService)文章中查看"权限密码同步"的一节，并确定如果您已具有这些权限的帐户，或者如果您需要创建一个。
- 如果您有多目录林同步是同步服务器可以进入每个目录林中？
 
>[AZURE.NOTE]
请确保每个回答的记录笔记并了解答案背后的基本原理。 [确定事件响应要求](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)超出可用的选项。 通过无回答的问题会选择哪个选项可以最好地满足您的业务需要。

## <a name="next-steps"></a>下一步行动
[确定多因素身份验证要求](active-directory-hybrid-identity-design-considerations-multifactor-auth-requirements.md)

## <a name="see-also"></a>请参见
[设计考虑事项概述](active-directory-hybrid-identity-design-considerations-overview.md)
