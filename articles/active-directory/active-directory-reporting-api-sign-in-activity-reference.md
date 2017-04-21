<properties
    pageTitle="Azure Active Directory 签到活动报告 API 参考 |Microsoft Azure"
    description="Azure Active Directory 签到活动报告 API 参考"
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
    ms.date="09/25/2016"
    ms.author="dhanyahk;markvi"/>

# <a name="azure-active-directory-sign-in-activity-report-api-reference"></a>Azure Active Directory 签到活动报告 API 参考


本主题是有关报告 API Azure Active Directory 的主题集合的一部分。  
Azure AD 报告为您提供一个 API，使您可以访问使用代码或相关的工具的登录活动报表数据。
本主题的范围是为您提供有关**登录活动报告 API**参考信息。

请参见︰

- [登录活动](active-directory-reporting-azure-portal.md#sign-in-activities)有关的概念信息
- [开始使用 Azure 活动目录报告 API](active-directory-reporting-api-getting-started.md)报告的 API 的详细信息。

疑问、 问题或反馈，请与[帮助 AAD 报告](mailto:aadreportinghelp@microsoft.com)。



## <a name="who-can-access-the-api-data"></a>哪些人可以访问的 API 数据？

- 安全管理或安全读取器角色中的用户

- 全局管理员

- 任何有权访问此 API 的应用程序 （应用程序授权可以是安装程序仅基于全局管理员权限）



## <a name="prerequisites"></a>系统必备组件

若要通过报告的 API 访问此报表，您必须︰

- [Azure 活动目录特优 P1 或 P2 版](active-directory-editions.md)

- 完成[访问报告 API Azure AD 的先决条件](active-directory-reporting-api-prerequisites.md)。 


##<a name="accessing-the-api"></a>访问 API

您可以访问此 API 通过[图形资源管理器](https://graphexplorer2.cloudapp.net)或编程方式，例如，使用 PowerShell。 为了使 PowerShell 来正确解释 AAD 图的其余部分调用中使用 OData 筛选器语法，您必须使用引号 (亦即︰ 抑音符) 字符 $ 字符"转义"。 引号字符用作[PowerShell 的转义字符](https://technet.microsoft.com/library/hh847755.aspx)，允许 PowerShell 的文字解释 $ 字符，并避免混淆它为 PowerShell 变量名 (例如︰ $filter)。

本主题的重点是在图形浏览器上。 PowerShell 的示例，请参阅此[PowerShell 脚本](active-directory-reporting-api-sign-in-activity-samples.md#powershell-script)。


## <a name="api-endpoint"></a>API 端点

您可以访问此 API 使用以下基 URI:  
    
    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta  



由于数据量，此 API 的第一百万个返回的记录的限制。 

此调用返回的数据在批处理中。 每一批都有 1000 条记录的最多。  
要获取下一批记录，请使用下一个链接。 [Skiptoken](https://msdn.microsoft.com/library/dd942121.aspx)信息获得返回的记录的第一套。 跳过标记将会在结果集的末尾。  

    https://graph.windows.net/$tenantdomain/activities/signinEvents?api-version=beta&%24skiptoken=-1339686058


## <a name="supported-filters"></a>受支持的筛选器

您可以缩小 api 返回的记录的数目在窗体中的筛选器的调用。  
对于登录 API 相关的数据，支持以下筛选器︰

- **$top =\<要返回的记录的编号\>**-可以限制返回的记录数。 这是代价高昂的操作。 如果您想要返回数千项的对象，不应使用此筛选器。  
- **$filter =\<筛选语句\>**-根据受支持的筛选器字段中，指定您关注的记录的类型



## <a name="supported-filter-fields-and-operators"></a>受支持的筛选器字段和运算符

若要指定您关注的记录的类型，可以生成筛选语句可以包含一个或下面的筛选器字段组合︰

- [signinDateTime](#signindatetime) -定义日期或日期范围

- [用户 Id](#userid) -定义特定用户基于用户的 id。

- [范围内](#userprincipalname)的定义特定用户基于该用户的用户主体名称 (UPN)

- [应用程序标识](#appid)的定义特定的应用程序基于应用程序的 ID

- [appDisplayName](#appdisplayname) -定义特定的应用程序基于应用程序的显示名称

- [loginStatus](#loginStatus) -定义的登录状态 (成功 / 失败)


> [AZURE.NOTE] 使用图形浏览器，您需要为每个字母使用正确的大小写时，筛选器字段。


以缩小对返回的数据的范围，您可以构建受支持的筛选器和筛选器字段的组合。 例如，下面的语句返回 7 月 1 日 2016年之间 7 月 6 日 2016年前的 10 条记录︰

    https://graph.windows.net/contoso.com/activities/signinEvents?api-version=beta&$top=10&$filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T00:00:00Z


----------

### <a name="signindatetime"></a>signinDateTime

**支持运算符**︰ eq，ge，le、 gt、 浅色

**示例**︰

使用特定日期

    $filter=signinDateTime+eq+2016-04-25T23:59:00Z  



使用的日期范围    

    $filter=signinDateTime+ge+2016-07-01T17:05:21Z+and+signinDateTime+le+2016-07-07T17:05:21Z


**注释**︰

日期时间参数应为 UTC 格式 


----------

### <a name="userid"></a>用户 Id

**支持运算符**︰ eq

**示例**︰

    $filter=userId+eq+’00000000-0000-0000-0000-000000000000’

**注释**︰

用户 Id 的值是一个字符串值



----------

### <a name="userprincipalname"></a>范围内

**支持运算符**︰ eq

**示例**︰

    $filter=userPrincipalName+eq+'audrey.oliver@wingtiptoysonline.com' 


**注释**︰

范围内的值是一个字符串值

----------

### <a name="appid"></a>应用程序标识

**支持运算符**︰ eq

**示例**︰

    $filter=appId+eq+’00000000-0000-0000-0000-000000000000’



**注释**︰

应用程序标识的值是一个字符串值

----------


### <a name="appdisplayname"></a>appDisplayName

**支持运算符**︰ eq

**示例**︰

    $filter=appDisplayName+eq+'Azure+Portal' 


**注释**︰

AppDisplayName 的值是一个字符串值

----------

### <a name="loginstatus"></a>loginStatus

**支持运算符**︰ eq

**示例**︰

    $filter=loginStatus+eq+'1'  


**注释**︰

有两个 loginStatus 选项︰ 0-成功，1-故障

----------



## <a name="next-steps"></a>下一步行动

- 若要查看示例筛选登录活动吗？ 签出[Azure Active Directory 签到活动报告 API 示例](active-directory-reporting-api-sign-in-activity-samples.md)。

- 您想要知道更多有关报告 API 的 Azure 广告吗？ 请参阅[入门 Azure 活动目录报告 API](active-directory-reporting-api-getting-started.md)。