<properties
    pageTitle="Azure Active Directory 审计 API 参考 |Microsoft Azure"
    description="如何开始使用 Azure Active Directory 审计 API"
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.date="10/24/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="azure-active-directory-audit-api-reference"></a>Azure Active Directory 审计 API 参考

本主题是有关报告 API Azure Active Directory 的主题集合的一部分。  
Azure AD 报告为您提供一个 API，使您可以访问使用代码或相关的工具的审核数据。
本主题的范围是为您提供有关**审计 API**参考信息。

请参见︰

- [审核日志](active-directory-reporting-azure-portal.md#audit-logs)有关的概念信息
- [开始使用 Azure 活动目录报告 API](active-directory-reporting-api-getting-started.md)报告的 API 的详细信息。

疑问、 问题或反馈，请与[帮助 AAD 报告](mailto:aadreportinghelp@microsoft.com)。


## <a name="who-can-access-the-data"></a>哪些人可以访问数据？

- 安全管理或安全读取器角色中的用户

- 全局管理员

- 任何有权访问此 API 的应用程序 （应用程序授权可以是安装程序仅基于全局管理员权限）



## <a name="prerequisites"></a>系统必备组件

要通过报告的 API 访问此报表，您必须具有︰

- [Azure 活动目录免费或更高版本](active-directory-editions.md)

- 完成[访问报告 API Azure AD 的先决条件](active-directory-reporting-api-prerequisites.md)。 
 

##<a name="accessing-the-api"></a>访问 API

您可以访问此 API 通过[图形资源管理器](https://graphexplorer2.cloudapp.net)或编程方式，例如，使用 PowerShell。 为了使 PowerShell 来正确解释 AAD 图的其余部分调用中使用 OData 筛选器语法，您必须使用引号 (亦即︰ 抑音符) 字符 $ 字符"转义"。 引号字符用作[PowerShell 的转义字符](https://technet.microsoft.com/library/hh847755.aspx)，允许 PowerShell 的文字解释 $ 字符，并避免混淆它为 PowerShell 变量名 (例如︰ $filter)。

本主题的重点是在图形浏览器上。 PowerShell 的示例，请参阅此[PowerShell 脚本](active-directory-reporting-api-audit-samples.md#powershell-script)。

 
 

## <a name="api-endpoint"></a>API 端点


您可以访问此 API 使用以下 URI:  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta

（使用 OData 分页） 的 Azure 广告审核 API 返回的记录数量没有限制。
为保留限制报告数据，检查出[报告保留策略](active-directory-reporting-retention.md)。

此调用返回的数据在批处理中。 每一批都有 1000 条记录的最多。  
要获取下一批记录，请使用下一个链接。 Skiptoken 信息获得返回的记录的第一套。 跳过标记将会在结果集的末尾。  

    https://graph.windows.net/contoso.com/activities/audit?api-version=beta&%24skiptoken=-1339686058




## <a name="supported-filters"></a>受支持的筛选器

您可以缩小 api 返回的记录的数目在窗体中的筛选器的调用。  
对于登录 API 相关的数据，支持以下筛选器︰

- **$top =\<要返回的记录的编号\>**-可以限制返回的记录数。 这是代价高昂的操作。 如果您想要返回数千项的对象，不应使用此筛选器。     
- **$filter =\<筛选语句\>**-根据受支持的筛选器字段中，指定您关注的记录的类型



## <a name="supported-filter-fields-and-operators"></a>受支持的筛选器字段和运算符

若要指定您关注的记录的类型，可以生成筛选语句可以包含一个或下面的筛选器字段组合︰

- [activityDate](#activitydate) -定义日期或日期范围
- [activityType](#activitytype) -定义活动的类型
- [活动](#activity)-将活动定义为字符串  
- [演员/名称](#actorname)-使用者名称的窗体中定义主角
- [演员/对象 id](#actorobjectid)的使用者 ID 的窗体中定义主角   
- [演员/upn](#actorupn)的在窗体中的主角的用户主体名称 (UPN) 定义主角 
- [目标/名称](#targetname)-使用者名称的窗体中定义的目标
- [目标/objectid](#targetobjectid) -目标的 ID 的窗体中定义目标  
- [目标/upn](#targetupn)的定义在窗体中的主角的用户主体名称 (UPN) 主角   




----------

### <a name="activitydate"></a>activityDate

**支持运算符**︰ eq，ge，le、 gt、 浅色

**示例**︰

    $filter=tdomain + 'activities/audit?api-version=beta&`$filter=eventTime gt ' + $7daysago    

**注释**︰

日期时间应为 UTC 格式

----------

### <a name="activitytype"></a>activityType

**支持运算符**︰ eq

**示例**︰

    $filter=activityType eq 'User'  

**注释**︰

区分大小写

----------

### <a name="activity"></a>活动

**支持运算符**︰ eq，包含，startsWith

**示例**︰

    $filter=activity eq 'Add application' or contains(activity, 'Application') or startsWith(activity, 'Add')   

**注释**︰

区分大小写

----------

### <a name="actorname"></a>演员/名称

**支持运算符**︰ eq，包含，startsWith

**示例**︰

    $filter=actor/name eq 'test' or contains(actor/name, 'test') or startswith(actor/name, 'test')  

**注释**︰

不区分大小写

    

----------
### <a name="actorobjectid"></a>演员/对象 Id

**支持运算符**︰ eq

**示例**︰

    $filter=actor/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba'    

----------
### <a name="targetname"></a>目标/名称

**支持运算符**︰ eq，包含，startsWith

**示例**︰

    $filter=targets/any(t: t/name eq 'some name')   

**注释**︰

不区分大小写

----------

### <a name="targetupn"></a>目标/upn

**支持运算符**︰ eq，startsWith

**示例**︰

    $filter=targets/any(t: startswith(t/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity/userPrincipalName,'abc')) 

**注释**︰

- 不区分大小写
- 您需要查询 Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.TargetResourceUserEntity 时添加的完整命名空间

----------

### <a name="targetobjectid"></a>目标/对象 Id

**支持运算符**︰ eq

**示例**︰

    $filter=targets/any(t: t/objectId eq 'e8096343-86a2-4384-b43a-ebfdb17600ba')    

----------

### <a name="actorupn"></a>演员/upn

**支持运算符**︰ eq，startsWith

**示例**︰

    $filter=startswith(actor/Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity/userPrincipalName,'abc')  

**注释**︰

- 不区分大小写 
- 您需要查询 Microsoft.ActiveDirectory.DataService.PublicApi.Model.Reporting.AuditLog.ActorUserEntity 时添加的完整命名空间

----------




## <a name="next-steps"></a>下一步行动

- 若要查看示例筛选的系统活动吗？ 签出[Azure Active Directory 审核 API 示例](active-directory-reporting-api-audit-samples.md)。

- 您想要知道更多有关报告 API 的 Azure 广告吗？ 请参阅[入门 Azure 活动目录报告 API](active-directory-reporting-api-getting-started.md)。