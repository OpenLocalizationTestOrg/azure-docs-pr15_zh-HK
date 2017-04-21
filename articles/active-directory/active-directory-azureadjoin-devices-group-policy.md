<properties
    pageTitle="将加入域的设备连接到 Azure 的广告，为 Windows 10 遇到 |Microsoft Azure"
    description="介绍了管理员如何才能配置组策略以启用设备要加入域的企业网络。"
    services="active-directory"
    documentationCenter=""
    authors="femila"
    manager="swadhwa"
    editor=""
    tags="azure-classic-portal"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/27/2016"
    ms.author="femila"/>

# <a name="connect-domain-joined-devices-to-azure-ad-for-windows-10-experiences"></a>将加入域的设备连接到 Windows 10 经验的 Azure 广告

加入域是传统方式组织已连接设备在过去的 15 年和更多的工作。 它使用户能够登录到其设备使用其 Windows 服务器的 Active Directory (Active Directory) 工作或学校帐户并且允许 IT 部门能够完全管理这些设备。 组织通常依赖于向用户提供设备的成像方法，通常使用系统中心配置管理器 (SCCM) 或组策略来管理它们。

将设备连接到 Azure 活动目录 (AD Azure) 后，域加入 Windows 10 中的将提供了以下好处︰

- 单一登录 (SSO) 对 Azure 广告资源的位置在哪里
- 对企业 Windows 应用商店使用的工作或学校帐户 （不需要 Microsoft 帐户） 的访问
- 企业符合漫游用户设置的设备中使用的工作或学校帐户 （不需要 Microsoft 帐户）
- 强身份验证和方便签到的工作或学校帐户与 Microsoft Passport 和 Windows Hello
- 访问仅限于符合组织设备的组策略设置的设备的能力

## <a name="prerequisites"></a>系统必备组件

域加入仍然是很有用。 不过，若要获得 SSO 的 Azure AD 好处、 工作或学校的帐户与漫游到 Windows 应用商店的设置与工作或学校帐户和访问权限，您将需要以下内容︰

- Azure 广告订阅
- Azure 广告连接到 Azure AD 扩展的内部目录
- 已设置为将加入域的设备连接到 Azure AD 的策略
- 设备的 Windows 10 版本 (build 10551 或更高版本)

若要启用 Microsoft Passport 的工时和 Windows Hello，还需要以下︰

- 公钥基础结构 (PKI) 的用户证书颁发。
- 系统中心配置管理器的技术预览版本 1509年。 有关详细信息，请参阅[Microsoft 系统中心配置管理器技术预览](https://technet.microsoft.com/library/dn965439.aspx#BKMK_TP3Update)和[系统中心配置管理器团队博客](http://blogs.technet.com/b/configmgrteam/archive/2015/09/23/now-available-update-for-system-center-config-manager-tp3.aspx)。 这是部署基于 Microsoft Passport 键的用户证书所必需的。

作为 PKI 部署要求的替代方法，您可以执行以下操作︰

- 有几个与 Windows 服务器 2016 Active Directory 域服务的域控制器。

要启用条件的访问，可以创建允许加入域的任何其他部署的设备访问的组策略设置。 若要管理访问控制基于符合性的设备，您需要︰

- 系统中心配置管理器版本 1509年技术预览护照方案

## <a name="deployment-instructions"></a>部署的说明



### <a name="step-1-deploy-azure-active-directory-connect"></a>步骤 1︰ 将 Azure Active Directory 部署连接

Azure AD 连接将使您能够在云中的设备对象设置本地计算机。 要部署 Azure AD 连接，请参阅文章[集成内部标识使用 Azure Active Directory](active-directory-aadconnect.md#install-azure-ad-connect)中的"安装 Azure AD 连接"。

 - 如果您遵循[的 Azure AD 连接的自定义安装](./connect/active-directory-aadconnect-get-started-custom.md)（不快速安装），然后按照后面的过程**创建的服务连接点在内部部署 Active Directory 中**，这一步。
 - 如果您有使用 Azure AD 连接 （例如，如果您之前部署了 Active Directory 联合身份验证服务 (AD FS)），在安装前的 Azure 广告联盟的配置然后按照**配置 AD FS 声明规则**后面的过程，这一步。

#### <a name="create-a-service-connection-point-in-on-premises-active-directory"></a>在内部部署 Active Directory 中创建服务连接点

加入域的设备将使用服务连接点发现 Azure AD 租户信息时自动使用 Azure 设备注册服务注册。

在 Azure AD 连接服务器上，运行以下 PowerShell 命令︰

    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;


当运行该 cmdlet $aadAdminCred = 获取凭据，使用格式*user@example.com*时获取凭据弹出消息将显示输入的凭据的用户名称。

当运行 cmdlet 初始化 ADSyncDomainJoinedComputerSync...，替换为 Active Directory 连接器帐户使用域帐户 [*连接器帐户名称*]。

#### <a name="configure-ad-fs-claim-rules"></a>配置 AD FS 声明规则
配置 AD FS 声明规则允许计算机使用 Kerberos/AD FS 通过 NTLM 进行身份验证，从而使瞬时注册 Azure 设备注册服务的计算机。 没有此步骤，计算机将得到到 Azure AD 以延迟方式 （取决于 Azure AD 连接同步时间）。

>[AZURE.NOTE]
如果您没有 AD FS 联合身份验证服务器上部署作为，按照您的供应商的说明进行操作来创建声明规则。

在 AD FS 服务器 （或连接到 AD FS 服务器会话），则运行以下 PowerShell 命令︰

      <#----------------------------------------------------------------------
     |   Modify the Azure AD Relying Party to include the claims needed
     |   for DomainJoin++. The rules include:
     |   -ObjectGuid
     |   -AccountType
     |   -ObjectSid
     +---------------------------------------------------------------------#>

    $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules

    $rule1 = '@RuleName = "Issue object GUID"
          c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&
          c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
          => issue(store = "Active Directory", types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), query = ";objectguid;{0}", param = c2.Value);'

    $rule2 = '@RuleName = "Issue account type for domain joined computers"
          c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
          => issue(Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", Value = "DJ");'

    $rule3 = '@RuleName = "Pass through primary SID"
          c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&
          c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]
          => issue(claim = c2);'

    $updatedRules = $existingRules + $rule1 + $rule2 + $rule3

    $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules

    Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString

>[AZURE.NOTE]
Windows 10 计算机将使用活动 WS 信任终结点承载 AD FS 的 Windows 集成身份验证进行身份验证。 确保已启用该终结点。 如果您正在使用 Web 身份验证代理服务器，还可以确保此终结点通过代理服务器来发布。 您可以通过检查 adfs/服务/信任/13/windowstransport 来执行此操作。 它应该显示为启用 AD FS 管理控制台**服务**下 > **的终结点**。


### <a name="step-2-configure-automatic-device-registration-via-group-policy-in-active-directory"></a>步骤 2︰ 配置设备自动注册组策略通过在 Active Directory 中

在 Active Directory 中，可以使用组策略来配置 Windows 10 加入域的设备自动注册 Azure 的广告。

> [AZURE.NOTE]
> 有关如何设置自动设备注册的最新说明，请参阅[如何设置 Windows 域的自动注册加入 Azure Active Directory 的设备](active-directory-conditional-access-automatic-device-registration-setup.md)。
>
> 这个组策略模板已重命名 Windows 10 中。 如果您从 Windows 10 计算机运行组策略工具，该策略将显示为︰ <br>
> **作为设备注册加入域的计算机**<br>
> 策略是在以下位置︰<br>
> ***计算机配置/策略/管理模板/Windows 组件/设备注册***


## <a name="additional-information"></a>其他信息
* [企业的 Windows 10︰ 使用设备的工作方法](active-directory-azureadjoin-windows10-devices-overview.md)
* [扩展到 Windows 10 设备通过 Azure 活动目录加入云功能](active-directory-azureadjoin-user-upgrade.md)
* [了解使用方案 Azure 广告加入](active-directory-azureadjoin-deployment-aadjoindirect.md)
* [将加入域的设备连接到 Windows 10 经验的 Azure 广告](active-directory-azureadjoin-devices-group-policy.md)
* [设置 Azure 广告加入](active-directory-azureadjoin-setup.md)
