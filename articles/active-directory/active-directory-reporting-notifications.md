<properties
    pageTitle="Azure 的 Active Directory 报告通知"
    description="如何使用报告可疑迹象的通知在 Azure Active Directory 单元。"
    services="active-directory"
    documentationCenter=""
    authors="dhanyahk"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="03/07/2016"
    ms.author="dhanyahk"/>

# <a name="azure-active-directory-reporting-notifications"></a>Azure 的 Active Directory 报告通知

## <a name="what-reports-generate-email-notifications"></a>哪种报告生成电子邮件通知

到目前为止，仅在活动报告触发中的不规则号发送电子邮件通知。

## <a name="what-is-an-irregular-sign-in"></a>"不规则登录"是什么？

不规则的登录是指那些由我们的机器学习算法，根据与反常登录的位置和设备结合，一个"不可能旅行"条件。 这可能表明黑客想要使用此帐户登录。

## <a name="who-receives-the-email-notifications"></a>接收电子邮件通知用户？

电子邮件被发送到所有已分配活动目录特优许可证的全局管理员。 为了确保交付，我们将其发送给管理员备选电子邮件地址以及。 管理员应包括aad-alerts-noreply@mail.windowsazure.com在其安全发件人列表，这样他们不会错过此电子邮件。

## <a name="how-often-are-these-emails-sent"></a>频率是发送这些电子邮件？

如果 10 新不规则签到活动发生在过去的 30 天内，或者自上一次的电子邮件已发送，准小于，发送电子邮件。

## <a name="how-do-i-access-the-report-mentioned-in-the-email"></a>如何访问电子邮件中提到的报告？

当您单击的链接时，您将重定向到 Azure 的传统门户网站内的报表页。 若要访问该报表，必须同时是︰

- 管理员或 co-管理 Azure 订购的

- 全局管理员在目录中，并分配一个活动目录特优许可证。 有关详细信息，请参见[Azure Active Directory 版本](active-directory-editions.md)。

## <a name="can-i-turn-off-these-emails"></a>可以关闭这些电子邮件？

是，要关闭 Azure 的经典门户中的反常号单元相关的通知，请单击**配置**，然后选择**已禁用**的**通知**部分下。

## <a name="whats-next"></a>接下来是什么
- 想了解可以使用哪些安全、 审核及活动报表？ 签出[Azure AD 安全、 审核及活动报告](active-directory-view-access-usage-reports.md)
- [要开始使用 Azure 活动目录津贴](active-directory-get-started-premium.md)
- [添加公司品牌为您登录并访问面板页](active-directory-add-company-branding.md)
