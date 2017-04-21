<properties
    pageTitle="Azure AD 域服务︰ 启用密码同步 |Microsoft Azure"
    description="要开始使用 Azure Active Directory 域服务"
    services="active-directory-ds"
    documentationCenter=""
    authors="mahesh-unnikrishnan"
    manager="stevenpo"
    editor="curtand"/>

<tags
    ms.service="active-directory-ds"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="09/20/2016"
    ms.author="maheshu"/>

# <a name="enable-password-synchronization-to-azure-ad-domain-services"></a>启用密码同步的 Azure AD 域服务
前面的任务中，您可以启用 Azure AD 租户的 Azure AD 域服务。 下一步的任务是使凭据 NTLM 和 Kerberos 身份验证要同步到 Azure AD 域服务所需的哈希值。 一旦设置了凭据同步，用户可以登录到托管域使用其公司的凭据。

所涉及的步骤根据您的组织是否具有仅云 Azure 广告是不同租户或被设置为与您使用 Azure AD 连接的内部目录同步。

<br>

> [AZURE.SELECTOR]
- [仅云 Azure AD 租户](active-directory-ds-getting-started-password-sync.md)
- [同步 Azure AD 租户](active-directory-ds-getting-started-password-sync-synced-tenant.md)

<br>


## <a name="task-5-enable-password-synchronization-to-aad-domain-services-for-a-cloud-only-azure-ad-tenant"></a>任务 5︰ 启用密码同步的 AAD 域服务仅云 Azure 的 AD 租户
Azure AD 域服务需要凭据中的格式适合于 NTLM 和 Kerberos 身份验证，在托管域上的用户进行身份验证的哈希值。 除非您启用 AAD 域服务为您的租户，Azure 的广告不会生成或 NTLM 或 Kerberos 身份验证所需的格式存储的凭据的哈希值。 由于显而易见的安全原因，Azure 的广告还不存储任何凭据以明文形式。 因此，Azure 的广告并没有一种方法来生成这些 NTLM 或 Kerberos 凭据哈希值根据用户的现有凭据。

> [AZURE.NOTE] 如果您的组织有云只有 Azure AD 租户，需要使用 Azure AD 域服务的用户必须更改其密码。

此密码更改过程使凭据所需的 Azure AD 域 Kerberos 和 NTLM 身份验证服务，以在 Azure AD 中生成的哈希值。 您既可以终止租户需要使用 Azure AD 域服务或指示这些用户更改他们的密码中的所有用户的密码。


### <a name="enable-ntlm-and-kerberos-credential-hash-generation-for-a-cloud-only-azure-ad-tenant"></a>启用 NTLM 和 Kerberos 凭据希代仅云 Azure 的 AD 租户
以下是您需要提供最终用户，使用户可以更改其密码的说明︰

1. 导航到贵公司在[http://myapps.microsoft.com](http://myapps.microsoft.com)的 Azure 广告接入面板页。

2. 选择此页上的**配置文件**选项卡。

3. 单击此页面上的**更改密码**拼贴。

    ![为 Azure AD 域服务创建虚拟网络。](./media/active-directory-domain-services-getting-started/user-change-password.png)

    > [AZURE.NOTE] 如果看不到接入面板页上的**更改密码**选项，确保您的组织具有配置[密码管理 Azure 的广告中](../active-directory/active-directory-passwords-getting-started.md)。

4. 在**更改密码**页上，键入您现有的 （旧） 密码然后键入的新密码并确认。 单击**提交**。

    ![为 Azure AD 域服务创建虚拟网络。](./media/active-directory-domain-services-getting-started/user-change-password2.png)

更改您的密码后，新密码将可用在 Azure AD 域服务不久。 几分钟后 （通常情况下，大约 20 分钟），您可以登录到计算机加入托管域使用的新更改的密码。

<br>

## <a name="related-content"></a>相关的内容

- [如何更新自己的密码](../active-directory/active-directory-passwords-update-your-own-password.md)

- [在 Azure 广告中的密码管理入门](../active-directory/active-directory-passwords-getting-started.md)。

- [启用密码同步的同步 Azure 的 AAD 域服务 AD 租户](active-directory-ds-getting-started-password-sync-synced-tenant.md)

- [管理 Azure AD 域服务管理的域](active-directory-ds-admin-guide-administer-domain.md)

- [将 Windows 虚拟机加入到 Azure AD 域服务管理域](active-directory-ds-admin-guide-join-windows-vm.md)

- [Red Hat 企业 Linux 虚拟机加入 Azure AD 域服务管理域](active-directory-ds-admin-guide-join-rhel-linux-vm.md)
