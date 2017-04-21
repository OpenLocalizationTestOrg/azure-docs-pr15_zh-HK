<properties
    pageTitle="Azure 的 Active Directory 域服务︰ 故障排除指南 |Microsoft Azure"
    description="Azure AD 域服务的故障排除指南"
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
    ms.topic="article"
    ms.date="10/19/2016"
    ms.author="maheshu"/>

# <a name="azure-ad-domain-services---troubleshooting-guide"></a>Azure AD 域服务-故障排除手册
本文提供了设置或管理 Azure 活动目录 (AD) 域服务时可能遇到的问题的故障排除提示。


## <a name="you-cannot-enable-azure-ad-domain-services-for-your-azure-ad-directory"></a>不能启用 Azure 的广告目录的 Azure AD 域服务
本节将帮助您解决错误，当您尝试启用您的目录的 Azure AD 域服务和它发生故障或获取切换回已禁用。

选择对应与您遇到的错误信息的故障排除步骤。

|**错误消息**|**解析**|
|---|:---|
|*名称 contoso100.com 已在该网络上使用。指定不在使用的名称。*|[在虚拟网络中的域名称冲突](active-directory-ds-troubleshooting.md#domain-name-conflict)
|*无法在此 Azure AD 租户启用域服务。该服务没有足够的权限，对应用程序称为 Azure AD 域服务同步。删除应用程序称为 Azure AD 域服务同步，然后再尝试启用 Azure AD 租户的域服务。*|[域服务没有足够的权限对 Azure AD 域服务同步应用程序](active-directory-ds-troubleshooting.md#inadequate-permissions)|
|*无法在此 Azure AD 租户启用域服务。域服务应用程序在 Azure AD 租户中的没有所需的权限，以便域服务。删除应用程序标识符 d87dcbc6-a371-462e-88e3-28ad15ec4e64 的应用程序，然后再尝试启用 Azure AD 租户的域服务。*|[域服务应用程序在您的组织配置不正确](active-directory-ds-troubleshooting.md#invalid-configuration)|
|*无法在此 Azure AD 租户启用域服务。Azure AD 租户中禁用 Microsoft Azure AD 应用程序。启用应用程序标识符 00000002-0000-0000-c000-000000000000 的应用程序，然后再尝试启用 Azure AD 租户的域服务。*|[在 Azure AD 租户中禁用 Microsoft Graph 应用程序](active-directory-ds-troubleshooting.md#microsoft-graph-disabled)|


### <a name="domain-name-conflict"></a>域的名称冲突
**错误消息︰**

*名称 contoso100.com 已在该网络上使用。指定不在使用的名称。*

**补救措施︰**

确保您没有现有域与域同名的虚拟网络上可用。 例如，假定您有域在所选的虚拟网络称为 'contoso.com' 已经可用。 稍后，您尝试启用该虚拟网络上的同一个域名 (即 ' contoso.com') Azure AD 域服务托管的域。 当尝试启用 Azure AD 域服务时遇到故障。

此故障是由于该虚拟网络上的域名称的名称冲突。 在此情况下，必须使用一个不同的名称来设置 Azure AD 域服务托管域。 或者，可以消除资源调配现有的域，然后继续启用 Azure AD 域服务。


### <a name="inadequate-permissions"></a>权限不足
**错误消息︰**

*无法在此 Azure AD 租户启用域服务。该服务没有足够的权限，对应用程序称为 Azure AD 域服务同步。删除应用程序称为 Azure AD 域服务同步，然后再尝试启用 Azure AD 租户的域服务。*

**补救措施︰**

检查以查看是否存在具有名称 Azure AD 域服务同步在 Azure 的广告目录的应用程序。 如果此应用程序存在，删除它，然后再重新启用 Azure AD 域服务。

如果存在该应用程序，请执行以下步骤以检查应用程序存在，并将其删除:

  1. 导航到**Azure 的传统门户网站**([https://manage.windowsazure.com](https://manage.windowsazure.com))。
  2. 选择左边窗格中的**活动目录**节点。
  3. 选择要为其启用 Azure AD 域服务 Azure AD 租户 （目录）。
  4. 导航到**应用程序**选项卡。
  5. 在下拉列表中选择**我的公司拥有的应用程序**选项。
  6. 检查调用**Azure AD 域服务同步**的应用程序。 如果存在该应用程序，然后继续将其删除。
  7. 一旦您已删除该应用程序，尝试再次启用 Azure AD 域服务。


### <a name="invalid-configuration"></a>无效的配置
**错误消息︰**

*无法在此 Azure AD 租户启用域服务。域服务应用程序在 Azure AD 租户中的没有所需的权限，以便域服务。删除应用程序标识符 d87dcbc6-a371-462e-88e3-28ad15ec4e64 的应用程序，然后再尝试启用 Azure AD 租户的域服务。*

**补救措施︰**

请检查在 Azure 的广告目录中是否存在具有名称 AzureActiveDirectoryDomainControllerServices （使用 d87dcbc6-a371-462e-88e3-28ad15ec4e64 应用程序标识符） 的应用程序。 如果存在此应用程序，您需要将其删除，然后重新启用 Azure AD 域服务。

使用下面的 PowerShell 脚本来查找应用程序并将其删除。

> [AZURE.NOTE] 此脚本使用**Azure AD PowerShell 版本 2**的 cmdlet。 有关所有可用的 cmdlet 和下载模块的完整列表，请阅读[AzureAD PowerShell 的参考文档](https://msdn.microsoft.com/library/azure/mt757189.aspx)。

```
$InformationPreference = "Continue"
$WarningPreference = "Continue"

$aadDsSp = Get-AzureADServicePrincipal -Filter "AppId eq 'd87dcbc6-a371-462e-88e3-28ad15ec4e64'" -ErrorAction Ignore
if ($aadDsSp -ne $null)
{
    Write-Information "Found Azure AD Domain Services application. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $aadDsSp.ObjectId
    Write-Information "Deleted the Azure AD Domain Services application."
}

$identifierUri = "https://sync.aaddc.activedirectory.windowsazure.com"
$appFilter = "IdentifierUris eq '" + $identifierUri + "'"
$app = Get-AzureADApplication -Filter $appFilter
if ($app -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync application. Deleting it ..."
    Remove-AzureADApplication -ObjectId $app.ObjectId
    Write-Information "Deleted the Azure AD Domain Services Sync application."
}

$spFilter = "ServicePrincipalNames eq '" + $identifierUri + "'"
$sp = Get-AzureADServicePrincipal -Filter $spFilter
if ($sp -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync service principal. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $sp.ObjectId
    Write-Information "Deleted the Azure AD Domain Services Sync service principal."
}
```
<br>

### <a name="microsoft-graph-disabled"></a>Microsoft Graph 禁用
**错误消息︰**

无法在此 Azure AD 租户启用域服务。 Azure AD 租户中禁用 Microsoft Azure AD 应用程序。 启用应用程序标识符 00000002-0000-0000-c000-000000000000 的应用程序，然后再尝试启用 Azure AD 租户的域服务。

**补救措施︰**

检查以查看是否已禁用应用程序的标识符 00000002-0000-0000-c000-000000000000。 此应用程序是 Azure AD Microsoft 应用程序，并提供对 Azure AD 租户图形 API 访问。 Azure AD 域服务需要同步到您的托管域您 Azure AD 租户要启用此应用程序。

要解决此错误，请启用此应用程序，然后再尝试启用 Azure AD 租户的域服务。


## <a name="users-are-unable-to-sign-in-to-the-azure-ad-domain-services-managed-domain"></a>用户将无法登录到 Azure AD 域服务管理域
如果 Azure AD 租户中的一个或多个用户不能登录到新创建的管理域，请执行下列疑难解答步骤︰

- **使用 UPN 格式登录︰**尝试登录使用 UPN 格式 (例如，'joeuser@contoso.com')而不是 SAMAccountName 格式 (CONTOSO\joeuser)。 SAMAccountName 可能会自动生成的用户的 UPN 前缀过长或另一个用户在托管域相同。 UPN 格式保证 Azure AD 租户中是唯一的。

> [AZURE.NOTE] 我们建议使用 UPN 格式以用于登录到 Azure AD 域服务托管域。

- 确保您有依照入门指南 》 中概述的步骤[启用密码同步](active-directory-ds-getting-started-password-sync.md)。

- **外部帐户︰**确保受影响的用户帐户不是 Azure AD 租户在外部帐户。 外部客户的示例包括 Microsoft 帐户 (例如，'joe@live.com')或从外部用户帐户 Azure 的广告目录。 由于 Azure AD 域服务不具有此类用户帐户的凭据，这些用户不能登录到托管域。

- **同步帐户︰**如果受影响的用户帐户从本地目录同步，请验证︰
    - 您已经部署或更新到[最新建议的发布 Azure AD 连接](active-directory-ds-getting-started-password-sync.md#install-or-update-azure-ad-connect)。

    - 您已经配置了 Azure 广告连接到[执行完全同步](active-directory-ds-getting-started-password-sync.md)。

    - 根据您的目录的大小，它可能需要一段时间的用户帐户和凭据哈希值，可以在 Azure AD 域服务。 请确保您等待足够长的时间重试身份验证 （具体取决于您的目录-一天几个小时或两个用于大型目录的大小）。

    - 如果验证前面的步骤后仍然出现该问题，请尝试重新启动 Microsoft Azure AD 同步服务。 从您同步的计算机，启动命令提示符下，执行以下命令︰
      1. net stop Microsoft Azure AD 同步
      2. 净启动 Microsoft Azure AD 同步

- **仅云帐户**︰ 如果受影响的用户帐户是云专用的用户帐户，请确保启用 Azure AD 域服务后，用户已更改他们的密码。 此步骤会使凭据所需的 Azure AD 域服务生成的哈希值。


## <a name="users-removed-from-your-azure-ad-tenant-are-not-removed-from-your-managed-domain"></a>从 Azure AD 租户的用户不会从您的托管域
Azure 的广告会防止您意外删除用户对象。 从 Azure AD 租户删除用户帐户时，相应的用户对象被移动到回收站中。 当此删除操作同步到您的托管域后时，它将导致相应的用户帐户标记为已禁用。 此功能可帮助您恢复或撤消以后删除用户帐户。

若要从您的托管域完全删除用户帐户，请从 Azure AD 租户永久删除用户。 使用删除 MsolUser PowerShell cmdlet 使用-RemoveFromRecycleBin 选项，此[MSDN 文章](https://msdn.microsoft.com/library/azure/dn194132.aspx)中所述。


## <a name="contact-us"></a>请与我们联系
联系到 Azure Active Directory 域服务产品小组 [共享反馈或支持] (活动-目录-ds-联系人-us.md)。
