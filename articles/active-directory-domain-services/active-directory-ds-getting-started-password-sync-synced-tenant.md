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
前面的任务中，您可以启用 Azure AD 租户的 Azure AD 域服务。 下一步的任务是使同步密码的 Azure AD 域服务。 一旦设置了凭据同步，用户可以登录到托管域使用其公司的凭据。

所涉及的步骤根据您的组织是否具有仅云 Azure 广告是不同租户或被设置为与您使用 Azure AD 连接的内部目录同步。

<br>

> [AZURE.SELECTOR]
- [仅云 Azure AD 租户](active-directory-ds-getting-started-password-sync.md)
- [同步 Azure AD 租户](active-directory-ds-getting-started-password-sync-synced-tenant.md)

<br>


## <a name="task-5-enable-password-synchronization-to-aad-domain-services-for-a-synced-azure-ad-tenant"></a>任务 5︰ 启用密码同步的同步 Azure 的 AAD 域服务 AD 租户
一个同步 Azure AD 租户被设置为与您的组织使用 Azure AD 连接的内部目录同步。 Azure AD 连接不会同步 NTLM 和 Kerberos 凭据对 Azure AD 默认的哈希值。 若要使用 Azure AD 域服务，您需要配置 Azure AD 连接进行同步所需的 NTLM 和 Kerberos 身份验证凭据哈希。 以下步骤启用同步所需的凭据的哈希与 Azure AD 租户。


### <a name="install-or-update-azure-ad-connect"></a>安装或更新 Azure AD 连接
在联接域的计算机上安装 Azure AD 连接推荐的的发行。 如果您有 Azure AD 连接设置的一个现有实例，您需要将其更新以使用最新版本的 Azure AD 连接。 若要避免已知的问题/错误可能已修复，确保您始终使用最新版本的 Azure AD 连接。

**[下载 Azure AD 连接](http://www.microsoft.com/download/details.aspx?id=47594)**

推荐的版本︰ **1.1.281.0** -2016，9 月 7 在出版。

  > [AZURE.WARNING] 您必须安装 Azure AD 连接启用旧密码凭据 （NTLM 和 Kerberos 身份验证要求） 来同步到 Azure AD 租户推荐的的发行。 此功能不可用或使用旧目录同步工具的 Azure AD 连接的先前版本中。

有下文的[入门 Azure AD 连接](../active-directory/active-directory-aadconnect.md)Azure AD 连接的安装说明


### <a name="enable-synchronization-of-ntlm-and-kerberos-credential-hashes-to-azure-ad"></a>启用 NTLM 和 Kerberos 凭据的哈希与 Azure AD 的同步
在每个 AD 林中，强制完整的密码同步上执行以下 PowerShell 脚本并启用所有内部用户凭据哈希来同步到 Azure AD 租户。 此脚本启用 NTLM/Kerberos 身份验证要同步到 Azure AD 租户所需的凭据哈希。

```
$adConnector = "<CASE SENSITIVE AD CONNECTOR NAME>"  
$azureadConnector = "<CASE SENSITIVE AZURE AD CONNECTOR NAME>"  
Import-Module adsync  
$c = Get-ADSyncConnector -Name $adConnector  
$p = New-Object Microsoft.IdentityManagement.PowerShell.ObjectModel.ConfigurationParameter "Microsoft.Synchronize.ForceFullPasswordSync", String, ConnectorGlobal, $null, $null, $null
$p.Value = 1  
$c.GlobalParameters.Remove($p.Name)  
$c.GlobalParameters.Add($p)  
$c = Add-ADSyncConnector -Connector $c  
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $false   
Set-ADSyncAADPasswordSyncConfiguration -SourceConnector $adConnector -TargetConnector $azureadConnector -Enable $true  
```

根据您的目录的大小 (编号的用户，组等)，同步的凭据的哈希与 Azure 的广告需要花时间。 密码将可用在 Azure AD 域服务托管域凭据哈希已同步到 Azure AD 后不久。


<br>

## <a name="related-content"></a>相关的内容

- [启用密码同步的 AAD 域服务仅云 Azure 的广告目录](active-directory-ds-getting-started-password-sync.md)

- [管理 Azure AD 域服务管理的域](active-directory-ds-admin-guide-administer-domain.md)

- [将 Windows 虚拟机加入到 Azure AD 域服务管理域](active-directory-ds-admin-guide-join-windows-vm.md)

- [Red Hat 企业 Linux 虚拟机加入 Azure AD 域服务管理域](active-directory-ds-admin-guide-join-rhel-linux-vm.md)
