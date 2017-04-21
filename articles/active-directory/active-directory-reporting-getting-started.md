<properties
   pageTitle="Azure 的活动目录报告︰ 入门 |Microsoft Azure"
   description="列出在 Azure Active Directory 报告中各种可用的报告"
   services="active-directory"
   documentationCenter=""
   authors="dhanyahk"
   manager="femila"
   editor=""/>

<tags
   ms.service="active-directory"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="identity"
   ms.date="03/07/2016"
   ms.author="dhanyahk"/>

# <a name="getting-started-with-azure-active-directory-reporting"></a>要开始使用 Azure 活动目录报告

## <a name="what-it-is"></a>它是什么

Azure 活动目录 (AD Azure) 包括安全性、 活动和审计报告的目录。 下面是包含的报告的列表︰

### <a name="security-reports"></a>安全报告

- 来自未知源登录
- 在多次失败后登录
- 在多个地域的号接
- 从可疑活动的 IP 地址登录
- 不规则的签到活动
- 从可能是感染病毒的设备登录
- 用户登录活动异常

### <a name="activity-reports"></a>活动报告

- 应用程序使用情况︰ 摘要
- 应用程序使用情况︰ 详细
- 应用程序的仪表板
- 考虑资源调配错误
- 单个用户设备
- 单个用户活动
- 组活动报告
- 密码重置注册活动报告
- 密码重置活动

### <a name="audit-reports"></a>审核报告

- 目录审核报告

> [AZURE.TIP] 有关 Azure AD 报告上的多个文档，检查出[查看您访问和使用情况的报告](active-directory-view-access-usage-reports.md)。



## <a name="how-it-works"></a>它的工作原理


### <a name="reporting-pipeline"></a>报告管线

报告管线由三个主要步骤组成。 每次用户登录或进行身份验证时，将发生以下情况︰

- 首先，对用户进行验证 （无论成功或不成功），并将结果存储在 Azure Active Directory 服务数据库。
- 定期处理单元的所有新符号。 我们的安全和反常活动算法在这种情况下，搜索所有新符号项中是否有可疑的活动。
- 处理之后，报告编写、 缓存，并在 Azure 的传统门户网站提供服务。

### <a name="report-generation-times"></a>报告生成时间

由于大量的身份验证和签名宏处理的 Azure 的广告平台，最近号接处理程序是，平均而言，一小时以前。 在极少数情况下，可能需要 8 小时来处理最新的登录。

您可以通过检查每个报表顶部的帮助文本找到最近处理登录。

![顶部的每个报表的帮助文本](./media/active-directory-reporting-getting-started/reportingWatermark.PNG)

> [AZURE.TIP] 有关 Azure AD 报告上的多个文档，检查出[查看您访问和使用情况的报告](active-directory-view-access-usage-reports.md)。



## <a name="getting-started"></a>入门教程


### <a name="sign-into-the-azure-classic-portal"></a>登录到 Azure 的传统门户网站

首先，您需要登录到[Azure 的传统门户网站](https://manage.windowsazure.com)为全局或符合性管理员。 您还必须是 Azure 订阅服务管理员或共同管理员，或使用 Azure 广告"访问"Azure 的订阅。

### <a name="navigate-to-reports"></a>导航到报表

要查看报告，请导航到报告选项卡在您的目录的顶部。

如果这是您第一次查看报表时，您将需要同意一个对话框之前可以查看报告。 这是为了确保它是可接受的您查看此数据，可以考虑在一些国家和地区的私人信息的组织中的管理员。

![对话框中](./media/active-directory-reporting-getting-started/dialogBox.png)

### <a name="explore-each-report"></a>浏览每个报表

导航到每个报表来查看所收集的数据和符号接处理。 您可以找到[所有的报表列表在此处](active-directory-reporting-guide.md)。

![所有报表](./media/active-directory-reporting-getting-started/reportsMain.png)

### <a name="download-the-reports-as-csv"></a>下载为 CSV 报告

每个报告可以作为 CSV （逗号分隔值） 文件下载。 您可以使用这些文件在 Excel、 PowerBI 或第三方程序进一步分析数据的分析。

要下载以 CSV 的任何报表，请导航到报表和底部单击"下载"。

![下载按钮](./media/active-directory-reporting-getting-started/downloadButton.png)

> [AZURE.TIP] 有关 Azure AD 报告上的多个文档，检查出[查看您访问和使用情况的报告](active-directory-view-access-usage-reports.md)。





## <a name="next-steps"></a>下一步行动

### <a name="customize-alerts-for-anomalous-sign-in-activity"></a>在活动中自定义警报的异常符号

导航到您的目录的"配置"选项卡。

滚动到"通知"部分。

启用或禁用的"电子邮件通知反常的登录"一节。

![通知部分](./media/active-directory-reporting-getting-started/notificationsSection.png)

### <a name="integrate-with-the-azure-ad-reporting-api"></a>将与 Azure AD 报告 API 集成

请参阅[报告 API 入门](active-directory-reporting-api-getting-started.md)。

### <a name="engage-multi-factor-authentication-on-users"></a>对用户进行多因素身份验证

在报表中选择一个用户。

单击"启用 MFA"按钮在屏幕的底部。

![屏幕底部的多因素身份验证按钮](./media/active-directory-reporting-getting-started/mfaButton.png)

> [AZURE.TIP] 有关 Azure AD 报告上的多个文档，检查出[查看您访问和使用情况的报告](active-directory-view-access-usage-reports.md)。




## <a name="learn-more"></a>了解更多信息


### <a name="audit-events"></a>审核事件

了解目录中[Azure 活动目录报告审核事件](active-directory-reporting-audit-events.md)进行审核哪些事件有关。

### <a name="api-integration"></a>API 集成

请参阅[报告 API 入门](active-directory-reporting-api-getting-started.md)和[API 参考文档](https://msdn.microsoft.com/library/azure/mt126081.aspx)。

### <a name="get-in-touch"></a>与我们联系

电子邮件[aadreportinghelp@microsoft.com](mailto:aadreportinghelp@microsoft.com)的反馈、 帮助或如果您有任何问题。

> [AZURE.TIP] 有关 Azure AD 报告上的多个文档，检查出[查看您访问和使用情况的报告](active-directory-view-access-usage-reports.md)。
