<properties
    pageTitle="在 Azure Active Directory 中设置密码过期策略 |Microsoft Azure"
    description="了解如何检查过期策略和更改用户密码过期，单独或批量 Azure 活动目录密码"
    services="active-directory"
    documentationCenter=""
    authors="curtand"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/04/2016"
    ms.author="curtand"/>


# <a name="set-password-expiration-policies-in-azure-active-directory"></a>在 Azure Active Directory 中设置密码过期策略

> [AZURE.IMPORTANT] **这里是您，因为您遇到登录时有问题吗？** 如果是这样，[下面是如何更改和重置您自己的密码](active-directory-passwords-update-your-own-password.md)。

作为 Microsoft 云服务全局管理员，您可以使用 Microsoft Azure 活动目录模块用于 Windows PowerShell 设置用户密码过期。 您还可以使用 Windows PowerShell cmdlet 删除永不过期的配置，或要查看哪些用户密码设置不过期。 这篇文章对于云服务，例如 Microsoft Intune 和 Office 365 的标识和目录服务依赖 Microsoft Azure Active Directory 提供了帮助。

  > [AZURE.NOTE] 可以配置仅通过目录同步不同步的用户帐户的密码过期。 有关目录同步的详细信息，请参阅[目录同步路线图](https://msdn.microsoft.com/library/azure/hh967642.aspx)中的主题的列表。

若要使用 Windows PowerShell 的 cmdlet，您首先必须安装它们。

## <a name="what-do-you-want-to-do"></a>您要做什么？

- [如何检查密码过期策略](#how-to-check-expiration-policy-for-a-password)

- [设置密码过期](#set-a-password-to-expire)

- [设置密码，这样，它将不会过期](#set-a-password-to-never-expire)

## <a name="how-to-check-expiration-policy-for-a-password"></a>如何检查密码过期策略

1.  连接到 Windows PowerShell 使用您的公司管理员凭据。

2.  执行下列任一操作︰

    - 若要查看单个用户的密码被设置为永不过期，请运行以下 cmdlet 使用用户主体名称 (UPN) (例如，aprilr@contoso.onmicrosoft.com)或您想要查看的用户的用户 ID:`Get-MSOLUser -UserPrincipalName <user ID> | Select PasswordNeverExpires`

    - 若要查看所有用户的"密码永不过期"设置，请运行以下 cmdlet:`Get-MSOLUser | Select UserPrincipalName, PasswordNeverExpires`

## <a name="set-a-password-to-expire"></a>设置密码过期

1.  连接到 Windows PowerShell 使用您的公司管理员凭据。

2.  执行下列任一操作︰

    - 若要将一个用户的密码设置的密码将过期，运行以下 cmdlet 使用用户主体名称 (UPN) 或该用户的用户 ID:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $false`

    - 若要设置组织中的所有用户的密码，以便将过期，请使用以下 cmdlet:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $false`

## <a name="set-a-password-to-never-expire"></a>设置为永不过期的密码

1. 连接到 Windows PowerShell 使用您的公司管理员凭据。

2.  执行下列任一操作︰

    - 若要设置一个用户为永不过期的密码，请运行以下 cmdlet 使用用户主体名称 (UPN) 或该用户的用户 ID:`Set-MsolUser -UserPrincipalName <user ID> -PasswordNeverExpires $true`

    - 若要设置为永不过期的组织中所有用户的密码，请运行以下 cmdlet:`Get-MSOLUser | Set-MsolUser -PasswordNeverExpires $true`

## <a name="next-steps"></a>下一步行动

* **这里是您，因为您遇到登录时有问题吗？** 如果是这样，[下面是如何更改和重置您自己的密码](active-directory-passwords-update-your-own-password.md)。
