<properties
    pageTitle="Azure AD 连接︰ 在用户登录 |Microsoft Azure"
    description="Azure AD 连接用户登录的自定义设置。"
    services="active-directory"
    documentationCenter=""
    authors="billmath"
    manager="femila"
    editor="curtand"/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/16/2016"
    ms.author="billmath"/>



# <a name="azure-ad-connect-user-sign-on-options"></a>Azure AD 连接用户登录选项

Azure AD 连接，用户可以注册到云以及内部资源使用相同的密码。 本文介绍了您想要使用您登录到 Azure AD 的每个标识模型以帮助您选择身份的关键概念。

如果您已经熟悉 Azure 广告标识模型并想要了解有关特定方法的详细信息，只需单击下面相应的主题。

* [密码同步](#password-synchronization)
* [（具有 ADF) 联合的 SSO](#federation-using-a-new-or-existing-ad-fs-in-windows-server-2012-r2-farm)


## <a name="choosing-a-user-sign-in-method"></a>选择用户登录方法
对于大多数组织，只是想让用户登录到 Office 365，SaaS 应用程序和其他 Azure 广告基于资源，默认密码同步选项建议。
一些组织，但是，必须如 AD FS 选项使用联盟的号的特定理由。  其中包括︰

- 您的组织已经具有 AD FS 或第三方部署联合身份验证提供程序
- 安全策略禁止同步到云的密码哈希值
- 您需要用户遇到无缝的 SSO，（没有其他密码提示） 时从域访问云资源在企业网络上加入机
- 您需要 AD FS 具有某些特定的功能
    - 内部使用第三方提供商或智能卡 （请查阅第三方 Windows Server 2012 R2 中的 AD fs 的 MFA 提供商） 的多因素身份验证
    - 活动目录集成功能，如软帐户锁定或 AD 密码和工作时间策略
    - 对两者的条件访问内部和云使用设备注册、 Azure AD 联接或 Intune MDM 策略资源

### <a name="password-synchronization"></a>密码同步
使用密码同步，用户密码的哈希值同步从内部活动目录到 Azure 的广告。  当更改或在重置密码时，新密码同步立即到 Azure AD，以便您的用户可以始终使用相同的密码的云资源与它们内部。  密码永远不会被发送到 Azure 的广告也在 Azure AD 以明文形式存储。
密码同步可以与密码写回，用于启用自助服务密码重置在 Azure 的广告。

<center>![云](./media/active-directory-aadconnect-user-signin/passwordhash.png)</center>

[密码同步的详细信息](active-directory-aadconnectsync-implement-password-synchronization.md)


### <a name="federation-using-a-new-or-existing-ad-fs-in-windows-server-2012-r2-farm"></a>在 Windows Server 2012 R2 服务器场中使用新的或现有的 AD FS 的联合身份验证
联合号上，使用您的用户可以登录到基于 Azure AD 服务其内部的密码，然后在公司网络上而无需重新输入其密码。  AD FS 的联合身份验证选项允许您部署一个新或 Windows Server 2012 R2 服务器场中指定现有的 AD FS。  如果您选择指定现有场，Azure AD 连接将配置您的服务器场和 Azure 的广告之间的信任，以便您的用户可以登录。

<center>![云](./media/active-directory-aadconnect-user-signin/federatedsignin.png)</center>

#### <a name="to-deploy-federation-with-ad-fs-in-windows-server-2012-r2-you-will-need-the-following"></a>若要部署 Windows Server 2012 R2 中的 AD FS 的联合身份验证，您需要以下

如果您正在部署新服务器场︰

- 联合身份验证服务器的 Windows Server 2012 R2 服务器。
- Windows Server 2012 R2 的 Web 应用程序代理服务器。
- 您预期的联合身份验证服务名称，如 fs.contoso.com 一个.pfx 文件与一个 SSL 证书。

如果您正在部署新服务器场或使用现有的服务器场︰

- 在联合身份验证服务器上的本地管理员凭据。
- 在所有的工作组 （未加入域） 上的本地管理员凭据您打算部署 Web 应用程序代理服务器角色的服务器。
- 在其执行该向导的计算机必须可以连接到任何其他您要在其安装 AD FS 或通过 Windows 远程管理的 Web 应用程序代理的计算机。

[使用 AD FS 配置 SSO](./aad-connect/active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs) 

#### <a name="sign-on-using-an-earlier-version-of-ad-fs-or-a-third-party-solution"></a>登录使用早期版本的 AD FS 或第三方解决方案

如果您已经使用早期版本的 AD FS （如 AD FS 2.0) 或第三方联合身份验证提供程序配置云号，您可以选择通过 Azure AD 连接跳过配置的用户登录。  这将使您能够在仍然使用现有解决方案的符号上时获取最新的同步和 Azure AD 连接的其他功能。

[Azure 的广告第三方联盟的兼容性列表](./active-directory-aadconnect-federation-compatibility.md)

## <a name="user-sign-in-and-user-principal-name-upn"></a>用户登录和用户主体名称 (UPN)

### <a name="understanding-user-principal-name"></a>了解用户主要名称

在 Active Directory 中的默认 UPN 后缀是域的创建用户帐户的 DNS 名称。 在大多数情况下，这是注册为在 Internet 上的企业域的域名称。 但是，您可以添加更多使用 Active Directory 域和信任关系的 UPN 后缀。

用户的 UPN 格式的为username@domai。 例如，活动目录 contoso.com 用户约翰可能具有 UPN john@contoso.com。 RFC 822 基于用户的 UPN。 虽然 UPN 和电子邮件共享相同的格式，但是用户的 UPN 值可能会或可能不等于用户的电子邮件地址。

### <a name="user-principal-name-in-azure-ad"></a>在 Azure AD 用户主要名称

Azure AD 连接向导将使用该范围内特性或可让您指定的属性 （在自定义安装） 用作从内部用户主体名称在 Azure 的广告。 这是用于登录到 Azure AD 值。 如果用户主体名称特性的值不对应一个验证域在 Azure 的广告，然后 Azure 广告将替换它带有默认。 onmicrosoft.com 值。

Azure Active Directory 中的每个目录附带了内置域名中窗体 contoso.onmicrosoft.com，您可以开始使用 Azure 或其他 Microsoft 服务。 您可以改进并简化了使用自定义的域的登录体验。 有关自定义的域 Azure AD 中的名称，以及如何验证域，请参阅[添加自定义域名到 Azure Active Directory](active-directory-add-domain.md#add-your-custom-domain-name-to-azure-active-directory)

## <a name="azure-ad-sign-in-configuration"></a>Azure 广告登录配置

### <a name="azure-ad-sign-in-configuration-with-azure-ad-connect"></a>Azure 广告登录配置使用 Azure AD 连接
Azure 广告登录体验是取决于是否能够匹配到其中一个自定义的域验证 Azure 的广告目录中正在同步的用户的用户主体名称后缀 Azure 的广告。 Azure AD 连接提供了同时配置登录设置 Azure 广告，帮助用户登录体验中云是类似于内部体验。 Azure AD 连接列出为域定义的 UPN 后缀和尝试 Azure AD 中的与自定义的域对其进行匹配，您需要采取相应的措施提供帮助。
Azure 广告登录页定义为内部部署 Active Directory 的 UPN 后缀出列出，并显示与每个后缀的相应状态。 状态值可以是其中一个下︰

| 状态 | 说明 | 所需的操作 |
|:----|:----|:----|
|已验证| Azure AD 连接找到匹配在 Azure AD 验证域。 此域的所有用户可以登录使用其内部凭据| 无需操作 |
|未验证| Azure AD 连接无法 Azure AD 中找到匹配的自定义域，但未对其进行验证。 此域的用户的 UPN 后缀将被更改为默认值。 onmicrosoft.com 后缀后如果不能验证域的同步。 | 在 Azure AD 验证自定义的域。 [了解更多信息](./active-directory-add-domain.md#verify-the-domain-name-with-azure-ad) |  
|不添加| Azure AD 连接找不到自定义的域相对应的 UPN 后缀。 此域中的用户的 UPN 后缀将被更改为默认值。 如果不添加域的 onmicrosoft.com 和 verifeid 在 Azure 中的。| 添加并验证自定义的域相对应的 UPN 后缀[了解更多](./active-directory-add-domain.md)|

Azure 广告登录页列出了与当前的验证状态的 Azure AD 中为内部部署 Active Directory 和相应的自定义域定义 UPN suffix(s)。 在自定义安装中，您现在可以选择**Azure 广告登录**页上的用户主要名称的属性。

![Azure 广告登录页](./media/active-directory-aadconnect-user-signin/custom_azure_sign_in.png)

您可以单击刷新按钮来重新从 Azure 广告中提取自定义的域的最新状态。

###<a name="selecting-attribute-for-user-principal-name-in-azure-ad"></a>在 Azure AD 中选择用户主要名称的属性

范围内的属性范围内是当他们登录到 Azure AD 用户将使用的属性和 Office 365。 在 Azure AD 用户进行同步之前，应验证域使用，也称为 UPN 后缀。 强烈建议保留默认属性范围。 如果此属性是不可路由，并且无法进行验证，则可以选择另一个属性，例如发送电子邮件的情况下，作为属性保存的登录 id。 这被称为备用 id。 备用的 ID 属性值必须符合 RFC822 标准。 备选的 ID 可以使用单一登录 (SSO) 的密码和登录解决方案联盟 SSO。

>[AZURE.NOTE] 使用其他 ID 与不兼容所有 Office 365 提供工作负荷。 有关详细信息，请参阅[配置备用的登录 ID](https://technet.microsoft.com/library/dn659436.aspx)。

#### <a name="different-custom-domain-states-and-effect-on-azure-sign-in-experience"></a>其他自定义的域状态和 Azure 登录体验的影响
它是了解 Azure 的广告目录中自定义的域状态之间的关系非常重要，UPN 后缀定义内部。 让我们经历的不同可能 Azure 登录体验，sycnhronization 使用 Azure AD 连接设置时。

下面的信息，让我们假设我们关心 UPN 后缀 contoso.com 用作内部目录中的 UPN，例如user@contoso.com。

###### <a name="express-settings--password-synchronization"></a>快速设置 / 密码同步
| 状态         | Azure 签到的用户体验的影响 |
|:-------------:|:----------------------------------------|
| 不添加 | 在这种情况下没有自定义为 contoso.com 域已添加 Azure 的广告目录中。 具有与后缀 UPN 内部部署的用户@contoso.com,将不能使用其内部 UPN 登录到 Azure。 他们改为将必须使用新的 UPN 由 Azure 的广告给他们提供通过添加默认 Azure 的广告目录的后缀。 例如，如果您正在同步到 Azure 的广告目录 azurecontoso.onmicrosoft.com 然后内部用户的用户user@contoso.com将给予的 UPNuser@azurecontoso.onmicrosoft.com|
| 未验证 | 在这种情况下我们拥有 Azure 的广告目录中添加自定义的域 contoso.com 但未尚未验证。 如果继续使用同步而不验证域用户，然后用户将被分配新的 UPN 由 Azure 广告就像不添加方案中的相同。|
| 已验证 | 在这种情况下，我们有自定义域 contoso.com 已经添加并验证在 Azure AD 的 UPN 后缀。 用户将能够使用其内部用户主体名称，例如user@contoso.com,以登录到 Azure 后同步到 Azure 的广告|

###### <a name="ad-fs-federation"></a>AD FS 联合身份验证
不能使用默认值创建联盟。 onmicrosoft.com 域 Azure 的广告或未验证的自定义域在 Azure 的广告。 当您运行 Azure AD 连接向导中，如果您选择创建一个验证的域然后 Azure AD 连接与联盟将提示您使用必要的记录，您的 DNS 域中承载的位置创建。 有关详细信息，请参阅[此处](active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation)。

如果您选择"使用 AD FS 联合身份验证"作为用户登录选项，您必须具有自定义域继续在 Azure AD 中创建联合身份验证。 对于我们的讨论，这意味着我们应该在 Azure 的广告目录中添加自定义的域 contoso.com。

| 状态         | Azure 签到的用户体验的影响 |
|:-------------:|:----------------------------------------|
| 不添加 | 在这种情况下 Azure AD 连接的 UPN 后缀 contoso.com Azure 的广告目录中找不到匹配的自定义域。 您需要添加自定义的域 contoso.com 如果您需要用户要使用 AD FS 像其内部 UPN 登录user@contoso.com。|
| 未验证 | 在这种情况下 Azure AD 连接将提示您如何在以后的阶段验证您的域的相应详细信息|
| 已验证 | 在这种情况下您可以继续使用不需要任何进一步的操作配置|  

## <a name="changing-user-sign-in-method"></a>更改用户登录方法

密码同步任务可用在 Azure AD 连接使用 Azure AD 连接时使用向导在初始配置之后，可以从联盟更改用户登录方法。 再次运行 Azure AD 连接向导，您将看到您可以执行的任务的列表。 从任务列表中，选择**更改用户登录**。 

![更改用户登录"](./media/active-directory-aadconnect-user-signin/changeusersignin.png)

在下一页上，您将需要为 Azure AD 提供的凭据。

![连接到 Azure 的广告](./media/active-directory-aadconnect-user-signin/changeusersignin2.png)

在**用户登录**页中，选择**密码同步**。 这将更改从目录到一个托管的联盟。

![连接到 Azure 的广告](./media/active-directory-aadconnect-user-signin/changeusersignin3.png)

>[AZURE.NOTE] 如果您要向密码同步只是一个临时的开关，请检查**无法转换用户帐户**。 不检查选项将导致转换的每个用户进行联盟，它可以需要几个小时。
  
## <a name="next-steps"></a>下一步行动
了解有关[将您的内部标识使用 Azure Active Directory 集成](active-directory-aadconnect.md)。

了解更多关于[Azure AD 连接︰ 设计概念](active-directory-aadconnect-design-concepts.md)


