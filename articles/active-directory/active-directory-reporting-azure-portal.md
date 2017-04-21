<properties
   pageTitle="Azure Active Directory 报告-预览 |Microsoft Azure"
   description="列出了各种可用的 Azure Active Directory 预览报告"
   services="active-directory"
   documentationCenter=""
   authors="markusvi"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="09/30/2016"
   ms.author="markvi"/>

# <a name="azure-active-directory-reporting---preview"></a>Azure Active Directory 报告-预览

> [AZURE.SELECTOR]
- [Azure 门户](active-directory-reporting-azure-portal.md)
- [Azure 的传统门户网站](active-directory-reporting-guide.md)

*本文档是[Azure 活动目录报告指南](active-directory-reporting-guide.md)的一部分。*

报告在 Azure Active Directory 预览中，您获取您需要确定您的环境正在执行的操作的所有信息。 [在预览中是什么？](active-directory-preview-explainer.md)

有报告的两个主要区域︰

- **登录的活动**--有关使用托管应用程序和用户登录活动的信息

- **审核日志**的用户和组管理、 托管的应用程序和目录的活动有关的系统活动信息

这取决于您要寻找的数据范围，您可以访问这些报告通过**用户和组**或**企业应用程序**列表中单击服务在[Azure 的门户](https://portal.azure.com)。

## <a name="sign-in-activities"></a>登录活动

### <a name="user-sign-in-activities"></a>用户登录活动

通过用户登录报告提供的信息，您找到答案的问题如︰

- 用户的登录模式是什么？
- 多少用户的用户登录保留一周以上？
- 这些登录状态是什么？

您对此数据的入口点是图形**概述**部分下**的用户和组**中用户登录。

 ![报告](./media/active-directory-reporting-azure-portal/05.png "报告")

用户登录的关系图显示符号的每周一次聚合单元中指定的时间段内的所有用户。 时间段内的默认值为 30 天。

![报告](./media/active-directory-reporting-azure-portal/02.png "报告")

如果您单击符号在关系图中某一天，获得登录活动的详细的列表。

![报告](./media/active-directory-reporting-azure-portal/03.png "报告")

签到活动列表中的每一行提供了有关所选登录的详细的信息如︰

- 签名者？

- 相关的 UPN 是什么？

- 哪些应用程序是登录的目标？

- 登录的 IP 地址是什么？

- 签入的状态是什么？

### <a name="usage-of-managed-applications"></a>托管应用程序的使用情况

您登录的数据以应用程序为中心的视图，您可以回答诸如︰

- 谁在使用我的应用程序？

- 最多 3 应用程序在您的组织中有哪些？

- 我最近已推出一个应用程序。 它如何做？


对此数据的入口是在**企业应用程序**下的**概述**部分中的最后 30 天报告内组织中最多 3 应用程序。

 ![报告](./media/active-directory-reporting-azure-portal/06.png "报告")


应用程序使用率图每周一次的聚合签名单元顶部 3 应用程序给定的时间段内。 时间段内的默认值为 30 天。

![报告](./media/active-directory-reporting-azure-portal/78.png "报告")

如果您愿意，可以将焦点设置在特定的应用程序。

![报告](./media/active-directory-reporting-azure-portal/single_spp_usage_graph.png "报告")


当您单击应用程序使用关系图中某一天时，可以登录活动的详细的列表。


![报告](./media/active-directory-reporting-azure-portal/top_app_sign_ins.png "报告")



**登录**选项提供对您的应用程序注册的所有事件的完整概述。

![报告](./media/active-directory-reporting-azure-portal/85.png "报告")

通过使用列选择器，您可以选择您想要显示的数据字段。

![报告](./media/active-directory-reporting-azure-portal/column_chooser.png "报告")



### <a name="filtering-sign-ins"></a>筛选登录

您可以按时间间隔要限制显示的数据量筛选登录。

![报告](./media/active-directory-reporting-azure-portal/927.png "报告")


另一种方法来筛选登录活动的条目是来搜索特定的项。
搜索方法，可以确定您登录特定**用户**、**组**或**应用程序**周围的范围。


![报告](./media/active-directory-reporting-azure-portal/84.png "报告")

## <a name="audit-logs"></a>审核日志

Azure Active Directory 中的审核日志提供了法规遵从性的系统活动的记录。

有审计在 Azure 的门户网站中的相关的活动的三个主要类别︰

- 用户和组   

- 应用程序

- 目录   


审计报告活动的完整列表，请参阅[审核报告事件的列表](active-directory-reporting-audit-events.md#list-of-audit-report-events)。


您的所有审核数据的入口点是**Azure Active Directory**的**活动**中的**审核日志**。


![审核](./media/active-directory-reporting-azure-portal/61.png "审核")


审核日志具有一个列表视图，显示参与者 （人） （所） 的活动和目标。


![审核](./media/active-directory-reporting-azure-portal/345.png "审核")


通过单击列表视图中的项，可以得到关于它的更多详细信息。

![审核](./media/active-directory-reporting-azure-portal/873.png "审核")




### <a name="users-and-groups-audit-logs"></a>用户和组审核日志


使用用户和基于组的审计报告，您可以如获得问题的答案︰

- 什么类型的更新已经应用的用户？

- 更改了多少用户？

- 多少的密码发生更改？

- 管理员已在目录中做什么？

- 已添加的组是什么？

- 是否有组成员身份的更改？

- 组的所有者已更改？

- 已分配给某个组或用户哪些许可证？


如果只想查看审核的用户和组相关的数据，**用户和组**的**活动**部分中可以找到在**审核日志**下的筛选的视图。


![审核](./media/active-directory-reporting-azure-portal/93.png "审核")


### <a name="application-audit-logs"></a>应用程序审核日志

与基于应用程序的审核报告，您可以获得如问题的解答︰

- 已添加或更新的应用程序有哪些？

- 已删除的应用程序有哪些？

- 为应用程序的服务原则是否有变化？

- 已更改应用程序的名称吗？

- 同意者向应用程序？


如果只想查看与应用程序相关的审核数据，可以在**企业应用程序**的**活动**部分找到在**审核日志**下的筛选的视图。


![审核](./media/active-directory-reporting-azure-portal/134.png "审核")


### <a name="filtering-audit-logs"></a>筛选审核日志

您可以按时间间隔要限制显示的数据量筛选审核报告。

![审核](./media/active-directory-reporting-azure-portal/324.png "审核")

若要进行筛选的项的审核日志的另一种方法是搜索的特定条目。

![审核](./media/active-directory-reporting-azure-portal/237.png "审核")

## <a name="next-steps"></a>下一步行动

[Azure Active Directory 报告指南](active-directory-reporting-guide.md)，请参阅。
