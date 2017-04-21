<properties
    pageTitle="获取在云环境中启动 Azure MFA |Microsoft Azure"
    description="这是介绍如何开始使用 Azure MFA 云中的 Microsoft Azure 的多因素身份验证页。"
    services="multi-factor-authentication"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor="yossib"/>

<tags
    ms.service="multi-factor-authentication"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/17/2016"
    ms.author="kgremban"/>

# <a name="getting-started-with-azure-multi-factor-authentication-in-the-cloud"></a>要开始使用 Azure 云环境中的多因素身份验证
本文讲解如何在云中使用 Azure 多因素身份验证开始。

> [AZURE.NOTE]  以下文档提供有关如何使用户使用**Azure 传统门户网站**信息。 如果您正在寻找如何为 O365 用户设置 Azure 多因素身份验证信息，请参阅[Office 365 的多因素身份验证设置。](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6?ui=en-US&rs=en-US&ad=US)

![云中的 MFA](./media/multi-factor-authentication-get-started-cloud/mfa_in_cloud.png)

## <a name="prerequisites"></a>系统必备组件
您可以为您的用户启用 Azure 多因素身份验证之前，以下系统必备组件是必需的。


1. [注册订阅了 Azure](https://azure.microsoft.com/pricing/free-trial/) -如果没有 Azure 的订阅，您需要注册一个。 如果您是刚开始并使用 Azure MFA 可以使用试用版订购
2. [创建一个多因素身份验证提供程序](multi-factor-authentication-get-started-auth-provider.md)并将其分配到目录或[分配给用户的许可证](multi-factor-authentication-get-started-assign-licenses.md)

> [AZURE.NOTE]  许可证是供具有 Azure MFA，Azure AD 津贴或企业移动套件 (EMS) 的用户。  MFA 包括在 Azure AD 特优和 EMS。 如果您有足够的许可，您不需要创建身份验证提供程序。


## <a name="turn-on-two-step-verification-for-users"></a>打开包含两个步骤验证用户
要启动用户上需要两个启动验证，更改从启用禁用该用户的状态。  用户状态的详细信息，请参阅[在 Azure 多因素身份验证的用户状态](multi-factor-authentication-get-started-user-states.md)

使用以下过程为您的用户启用 MFA。

### <a name="to-turn-on-multi-factor-authentication"></a>若要启用多因素身份验证

1.  以管理员身份登录到[Azure 的传统门户网站](https://manage.windowsazure.com)。
2.  在左边，单击**撤销**。
3.  在目录中，选择您要启用的用户目录。
![单击目录](./media/multi-factor-authentication-get-started-cloud/directory1.png)
4.  在顶部，单击**用户**。
5.  在页面的底部，单击**管理的多因素身份验证**。 打开新的浏览器选项卡。
![单击目录](./media/multi-factor-authentication-get-started-cloud/manage1.png)
6.  查找您想要启用的两步验证的用户。 您可能需要更改在顶部的视图。 确保该状态是**禁用。**
![允许用户](./media/multi-factor-authentication-get-started-cloud/enable1.png)
7.  将**检查**放在其名称旁边的框。
7.  在右侧，单击**启用**。
![启用用户](./media/multi-factor-authentication-get-started-cloud/user1.png)
8.  单击**启用多因素身份验证**。
![启用用户](./media/multi-factor-authentication-get-started-cloud/enable2.png)
9.  注意到该用户的状态已从**禁用**更改为**启用**。
![启用用户](./media/multi-factor-authentication-get-started-cloud/user.png)

您的用户启用后，应通过电子邮件通知他们。 下次尝试登录，他们将要求注册为两步验证其帐户。 一旦他们开始使用两步验证，它们还需要设置应用程序密码，以避免被锁定非浏览器应用程序。


## <a name="use-powershell-to-automate-turning-on-two-step-verification"></a>使用 PowerShell 自动开启两步验证

若要更改使用[Azure AD PowerShell](../powershell-install-configure.md)的[状态](multi-factor-authentication-whats-next.md)，您可以使用。  您可以更改`$st.State`等于以下状态之一︰

- 启用
- 强制执行
- 已禁用  

> [AZURE.IMPORTANT]  我们不鼓励对用户直接从禁用状态移动到强制状态。 不基于浏览器的应用程序将停止工作，因为用户不经历了 MFA 注册并获取[应用程序的密码](multi-factor-authentication-whats-next.md#app-passwords)。 如果您有非基于浏览器的应用程序，并要求应用程序密码，我们建议，您进入禁用状态已启用。 这使用户能够注册并获得他们的应用程序的密码。 在此之后，可以将它们移到强制。

使用 PowerShell 会使用户的大容量的一个选项。 目前在 Azure 门户没有批量启用功能，您需要单独选择每个用户。 如果您有多个用户，这可能是相当多的任务。 通过创建 PowerShell 脚本使用下面，可以循环访问一个用户列表，允许它们。

        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "\*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName bsimon@contoso.com -StrongAuthenticationRequirements $sta

下面是一个示例︰

    $users = "bsimon@contoso.com","jsmith@contoso.com","ljacobson@contoso.com"
    foreach ($user in $users)
    {
        $st = New-Object -TypeName Microsoft.Online.Administration.StrongAuthenticationRequirement
        $st.RelyingParty = "\*"
        $st.State = “Enabled”
        $sta = @($st)
        Set-MsolUser -UserPrincipalName $user -StrongAuthenticationRequirements $sta
    }


有关详细信息，请参阅[在 Azure 多因素身份验证的用户状态](multi-factor-authentication-get-started-user-states.md)

## <a name="next-steps"></a>下一步行动
现在，已在云环境中设置 Azure 多因素身份验证，可以配置并设置您的部署。 更多详细信息，请参阅[配置 Azure 多因素身份验证](multi-factor-authentication-whats-next.md)。
