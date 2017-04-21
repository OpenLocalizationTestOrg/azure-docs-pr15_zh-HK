<properties
    pageTitle="标识同步和复制属性复原 |Microsoft Azure"
    description="如何使用 Azure AD 连接的目录同步期间处理与 UPN 或 ProxyAddress 冲突的对象的新行为。"
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/29/2016"
    ms.author="markusvi"/>



# <a name="identity-synchronization-and-duplicate-attribute-resiliency"></a>标识同步和复制属性恢复能力
重复的属性可恢复性是 Azure 将消除摩擦的**范围内**和**ProxyAddress**引起冲突，当运行 Microsoft 的同步工具之一的 Active Directory 中的功能。

这两个属性都通常需要在所有的**用户**、**组**或**联系人**对象，在给定的 Azure Active Directory 租户中是唯一的。

> [AZURE.NOTE] 只有用户可以有 Upn。

此功能启用的新行为同步管道的云部分中，所以它是中立和相关 Microsoft 同步产品包括 Azure AD 连接、 目录同步以及 MIM + 连接器的客户端。 本文档中使用通用术语"同步客户端"来表示这些产品的任何一个。

## <a name="current-behavior"></a>当前行为
如果试图设置违反此唯一性约束的 UPN 或 ProxyAddress 值与新的对象，Azure Active Directory 会阻止创建该对象。 同样，如果非唯一的 UPN 或 ProxyAddress 更新对象，则更新失败。 设置尝试或更新的同步客户端在每个导出周期，通过重试，仍无法解决此冲突之前。 在每次尝试时生成的错误报告电子邮件和同步客户端记录一个错误。

## <a name="behavior-with-duplicate-attribute-resiliency"></a>具有重复的特性弹性行为
而不是完全不能设置或更新的对象有重复的特性，Azure Active Directory"隔离"重复的特性，这将违反唯一性约束。 如果此属性是必需的供应范围内，类似的服务分配占位符值。 这些临时的值的格式是  
"***<OriginalPrefix>+<4DigitNumber>@<InitialTenantDomain>。 onmicrosoft.com***"。  
如果该特性不是必需的如**ProxyAddress**，Azure Active Directory 只需隔离冲突属性，并可以继续处理该对象的创建或更新。

后隔离特性，这一旧行为中使用的相同错误报告电子邮件中发送有关冲突的信息。 但是，此信息仅出现在错误报告一次，在隔离的情况下，它不再继续在电子邮件中记录。 同时，由于此对象导出已成功完成，同步客户端不会记录错误，并且不重试创建 / 更新操作后后续同步周期。

若要支持此行为的新属性已添加到用户、 组和联系人对象类︰  
**DirSyncProvisioningErrors**

这是一个用于存储将违反唯一性约束应将它们添加通常的冲突属性的多值的属性。 在 Azure Active Directory 运行每个小时的时间来查找重复的属性冲突都已解决，并自动从隔离区删除有问题的属性中启用了后台计时器任务。

### <a name="enabling-duplicate-attribute-resiliency"></a>允许重复的属性可恢复性
重复属性恢复跨所有 Azure Active Directory 承租人将新的默认行为。 将在默认情况下为启用同步首次 2016 8 月 22nd 或更高版本的所有租户。 启用同步在此日期之前的承租人将分批启用该功能。 此扩展将在 9 月 2016年开始和电子邮件通知将发送到每个租户技术通知与特定日期时，将启用该功能。

一旦开启重复特性弹性不能禁用它。

若要检查功能为您的租户，可以通过下载最新版本的 Azure 活动目录 PowerShell 模块和运行进行操作︰

`Get-MsolDirSyncFeatures -Feature DuplicateUPNResiliency`

`Get-MsolDirSyncFeatures -Feature DuplicateProxyAddressResiliency`

如果您想要主动启用该功能，它为您的租户开启之前，您可以通过下载最新版本的 Azure 活动目录 PowerShell 模块和运行进行操作︰

`Set-MsolDirSyncFeature -Feature DuplicateUPNResiliency -Enable $true`

`Set-MsolDirSyncFeature -Feature DuplicateProxyAddressResiliency -Enable $true`

## <a name="identifying-objects-with-dirsyncprovisioningerrors"></a>与 DirSyncProvisioningErrors 对象标识
有当前两种方法来确定具有重复的属性冲突，Azure 活动目录 PowerShell 和 Office 365 管理门户的用户由于这些错误的对象。 有计划扩展到其他门户根据报告在将来。

### <a name="azure-active-directory-powershell"></a>Azure 的 Active Directory PowerShell
有关本主题中的 PowerShell cmdlet，满足以下条件︰

- 所有以下 cmdlet 都区分大小写。
- **--ErrorCategory PropertyConflict**必须始终包含。 目前有没有其他类型的**ErrorCategory**，但将来可能会延长。

首先，首先运行**连接 MsolService** ，输入组织管理员凭据。

然后，使用以下 cmdlet 和运算符以不同的方式查看错误︰

1. [查看所有](#see-all)

2. [按属性类型](#by-property-type)

3. [有冲突的值](#by-conflicting-value)

4. [使用字符串搜索](#using-a-string-search)

5. [排序](#sorted)

6. [在有限的数量或全部](#in-a-limited-quantity-or-all)


#### <a name="see-all"></a>查看所有
一旦建立连接，以查看属性设置的常规列表租户中的错误运行︰

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict`

这将生成类似下面的结果︰  
 ![获得 MsolDirSyncProvisioningError](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/1.png "Get-MsolDirSyncProvisioningError")  


#### <a name="by-property-type"></a>按属性类型
若要查看错误的属性类型，添加使用**范围内**或**代理地址**的参数**的属性名称**标志︰

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyName UserPrincipalName`

或

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyName ProxyAddresses`

#### <a name="by-conflicting-value"></a>有冲突的值
若要查看有关错误的特定属性添加 （**-属性名称**时，必须使用以及添加此标志） 的**属性值的**标志︰

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyValue User@domain.com -PropertyName UserPrincipalName`


#### <a name="using-a-string-search"></a>使用字符串搜索
为广泛的字符串搜索，请使用**-SearchString**标志。 这可单独用于所有上面的标志， **-ErrorCategory PropertyConflict**，但它始终是必需的︰

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -SearchString User`

#### <a name="in-a-limited-quantity-or-all"></a>在有限的数量或全部
1. **MaxResults<Int>**可以用来限制对特定数目的值的查询。

2. **所有**可用于确保所有结果中存在大量错误的案例都检索。

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -MaxResults 5`

## <a name="office-365-admin-portal"></a>Office 365 管理门户

您可以在 Office 365 管理中心查看目录同步错误。 在 Office 365 门户报表只显示了这些错误的**用户**对象。 它不显示**组**和**联系人**之间的冲突有关的信息。


![活动用户](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/1234.png "活动用户")

有关如何在 Office 365 管理中心查看目录同步错误的说明，请参阅[Office 365 中的标识目录同步错误](https://support.office.com/en-us/article/Identify-directory-synchronization-errors-in-Office-365-b4fc07a5-97ea-4ca6-9692-108acab74067)。


### <a name="identity-synchronization-error-report"></a>标识同步错误报告
具有重复的特性发生冲突的对象都处理时通知包括在标准标识同步错误报表电子邮件发给技术通知此新行为，请与联系租户。 不过，没有这种现象中的重要更改。 在过去，信息重复的属性冲突就属于每个后续的错误报告直到解决冲突为止。 使用这种新行为，对于给定的冲突的错误通知才只显示一次-在冲突的属性将被隔离的时间。

以下是电子邮件通知如下所示的 ProxyAddress 冲突的一个示例︰  
    ![活动用户](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/6.png "Active Users")  

## <a name="resolving-conflicts"></a>解决冲突
重复的属性错误处理过去的方式应不不同故障诊断策略和解决这些错误的战术。 唯一的区别是计时器任务扫描通过在服务端上的租户，自动将考查的特性添加到正确的对象，一旦冲突解决。

下文概述了各种的故障诊断和解决策略︰[重复的或无效的属性防止 Office 365 中的目录同步](https://support.microsoft.com/kb/2647098)。

## <a name="known-issues"></a>已知的问题
无已知问题会导致数据丢失或服务质量下降。 其中几个是美学、 其他人重复的特性而不是隔离的冲突属性，则会引发错误导致标准"*预复原*"和另一个导致某些错误需要额外手动链接地址。

**核心行为︰**

1. 继续收到导出错误而不是被隔离重复属性具有特定属性的配置对象。  
例如︰

    一。 创建新用户的 UPN 与 AD 中**Joe@contoso.com**和 ProxyAddress**smtp:Joe@contoso.com**

    b。 此对象的属性冲突与现有的组，其中 ProxyAddress 是**SMTP:Joe@contoso.com**。

    c。 在导出时**ProxyAddress 冲突**错误引发而不是让被隔离的冲突属性。 是重试该操作在每个后续同步周期，如已被启用恢复功能之前。

2. 如果相同的 SMTP 地址与内部创建两个组，其中一个发生故障资源调配上以标准重复**ProxyAddress**错误第一次尝试。 但是，重复的值在下一同步周期正确地隔离。

**办公门户报表**︰

1. UPN 冲突集中的两个对象的详细的错误消息是相同的。 这表明了，两者有其 UPN 更改 / 隔离，实际上只有其中的一个有任何更改的数据。

2. UPN 冲突的详细的错误消息显示为已更改隔离其 UPN 的用户错误的显示名称。 例如︰

    一。 **用户 A**同步首先用**UPN = User@contoso.com **。

    b。 **用户 B**试图同步，接下来与**UPN = User@contoso.com **。

    c。 **用户 B 的**UPN 改为**User1234@contoso.onmicrosoft.com**，**User@contoso.com**添加到**DirSyncProvisioningErrors**。

    d。 **用户 B**的错误消息应指出已有**用户 A** **User@contoso.com**为 UPN，但它显示了**用户 B 的**自己的显示名称。



**标识同步错误报告**︰

有关*如何解决此问题的步骤*的链接不正确︰  
    ![活动用户](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/6.png "Active Users")  

它应该指向[https://aka.ms/duplicateattributeresiliency](https://aka.ms/duplicateattributeresiliency)。


## <a name="see-also"></a>请参见

- [Azure AD 连接同步](active-directory-aadconnectsync-whatis.md)

- [与 Azure Active Directory 集成您的内部标识](active-directory-aadconnect.md)

- [确定 Office 365 中的目录同步错误](https://support.office.com/en-us/article/Identify-directory-synchronization-errors-in-Office-365-b4fc07a5-97ea-4ca6-9692-108acab74067)
