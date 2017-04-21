<properties
    pageTitle="Azure 的活动目录 B2C︰ 自助服务密码重置 |Microsoft Azure"
    description="演示如何设置自助服务密码重置使用者在 Azure 活动目录 B2C 中的主题"
    services="active-directory-b2c"
    documentationCenter=""
    authors="swkrish"
    manager="mbaldwin"
    editor="curtand"/>

<tags
    ms.service="active-directory-b2c"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="07/24/2016"
    ms.author="swkrish"/>


# <a name="azure-active-directory-b2c-set-up-self-service-password-reset-for-your-consumers"></a>Azure 的活动目录 B2C︰ 设置自助服务密码重置您的使用者

通过自助服务密码重置功能，您使用者 （已经注册为本地帐户） 可以重新设置他们自己的密码。 这大大减少了您的支持人员的负担，特别是在您的应用程序有数以百万计的使用者定期使用它。 目前，仅支持作为一种恢复方法使用经验证的电子邮件地址。 在将来，我们将添加额外的恢复方法 （已验证的电话号码、 安全问题等）。

> [AZURE.NOTE]
本文适用于自助服务密码重置的签入策略的上下文中使用。 如果您需要完全可自定义密码重置策略从您的应用程序调用，请参阅[这篇文章](./active-directory-b2c-reference-policies.md#create-a-password-reset-policy)。

默认情况下，您的目录将不能自助服务密码重置开启。 使用以下步骤以将其打开︰

1. 作为订阅管理员登录到[Azure 的传统门户网站](https://manage.windowsazure.com/)。 这是相同的工作或学校帐户或相同的 Microsoft 帐户，用来创建您的目录。
2. 导航到 Active Directory 扩展在左侧的导航栏上。
3. 查找您的目录的**目录**选项卡下，单击它。
4. 单击**配置**选项卡。
5. 向下滚动到**用户密码重置策略**部分中，切换**用户启用密码重设**为**是**选项。 请注意，**备用电子邮件地址**选项处于选中状态;将其保留原样。

    ![自助服务密码重置](./media/active-directory-b2c-reference-sspr/sspr.png)

6. 单击页面底部的**保存**。 大功告成 ！

若要测试，请上具有本地帐户标识提供程序作为任何登录策略使用"立即运行"功能。 在本地帐户登录页 （在您输入的电子邮件地址和密码，或用户名和密码），请单击**不能访问您的帐户吗？**来验证使用者体验。

> [AZURE.NOTE]
通过使用该[公司品牌的功能](../active-directory/active-directory-add-company-branding.md)，可以自定义自助服务密码重置页面。
