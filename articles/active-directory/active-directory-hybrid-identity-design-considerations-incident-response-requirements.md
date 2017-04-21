
<properties
    pageTitle="Azure Active Directory 混合标识设计考虑事项--确定事故 rResponse 要求 |Microsoft Azure 的要求 "
    description="确定监视和报告能力可以被利用的混合标识解决方案的 IT 部门能够采取措施以确定并减轻潜在威胁"
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

# <a name="determine-incident-response-requirements-for-your-hybrid-identity-solution"></a>确定事件响应要求混合身份解决方案

大型或中型组织很可能将包含[安全事件响应](https://technet.microsoft.com/library/cc700825.aspx)到位，能够帮助 IT 采取相应操作级别的事件。 身份管理系统是在事件响应过程中的重要组成部分，因为可以使用它来帮助识别谁执行了特定操作对目标。 混合身份解决方案必须能够提供监控和报告功能，可以由 IT 部门能够采取措施以识别和缓解潜在的威胁。 典型的事件响应计划中将有以下几个阶段，作为计划的一部分︰

1.  初步的评估。
2.  事件的通信。
3.  损害控制和降低风险。
4.  查明它的危害和严重性。
5.  保存证据。
6.  向相应部门通知。
7.  系统恢复。
8.  文档。
9.  危害及损失评估。
10. 过程和计划的修订版本。

在它的识别过程中的危害和严重程度阶段，则将有必要确定被破坏的系统文件已经被访问过，并确定这些文件的敏感性。 您混合身份识别系统应该能够满足这些要求才能帮助您识别进行了这些更改的用户。 

## <a name="monitoring-and-reporting"></a>监视和报告
在初步评估阶段还可以帮助许多倍的身份识别系统，主要是如果系统具有内置的审核和报告功能。 在最初评估时，IT 管理员必须能够识别可疑活动，或该系统应能自动根据预配置的任务的触发器。 许多活动可能表示可能的攻击，但在其他情况下，配置不当的系统可能会导致很多假阳性的入侵检测系统中。 

身份管理系统应该帮助 IT 管理员识别和报告可疑活动。 通常可以完成这些技术要求，通过监视的所有系统并让一个报告功能，可以突出显示潜在的威胁。 使用下面的问题可帮助您设计时考虑考虑事件响应要求混合身份解决方案︰

- 您的公司是否有在地方安全事件响应？
 - 如果是的当前的身份识别管理系统可作为过程的一部分？
- 贵公司是否需要在不同设备标识来自用户的可疑登录尝试？
- 贵公司是否需要检测潜在受害的用户的凭据？
- 贵公司是否需要审核用户访问和操作？
- 贵公司是否需要了解当用户重置其密码？

## <a name="policy-enforcement"></a>策略实施

在灾难控制和风险减少的阶段，是快速减小攻击的实际和潜在影响非常重要。 此时将执行该操作可以使小和重大的区别。 准确的响应将取决于您的组织和您所面临的攻击的性质。 如果最初的评估得出的结论是一个帐户被破坏，需要强制执行策略以阻止此帐户。 这就是身份管理系统将利用其中的一个示例。 使用下面的问题有助于您考虑如何将实施策略对日常事件作出反应时设计混合身份解决方案︰

- 没有您的公司制定策略阻止用户到从访问网络如有必要吗？
 - 如果是的没有当前解决方案相结合要采用混合身份识别管理系统？
- 贵公司是否需要强制条件隔离区中的用户访问？ 
 
>[AZURE.NOTE]
请确保每个回答的记录笔记并了解答案背后的基本原理。 [定义数据保护战略](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md)超出可用选项以及每个选项的优点/缺点。  通过无回答的问题会选择哪个选项可以最好地满足您的业务需要。

## <a name="next-steps"></a>下一步行动
[定义数据保护战略](active-directory-hybrid-identity-design-considerations-data-protection-strategy.md)

## <a name="see-also"></a>请参见
[设计考虑事项概述](active-directory-hybrid-identity-design-considerations-overview.md)
