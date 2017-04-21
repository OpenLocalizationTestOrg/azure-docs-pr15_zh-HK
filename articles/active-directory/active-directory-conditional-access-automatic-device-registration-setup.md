<properties
    pageTitle="设置自动注册 Windows Azure Active Directory 域加入设备 |Microsoft Azure"
    description="设置 Azure Active Directory 中注册自动地默默加入域的 Windows 设备。"
    services="active-directory"
    documentationCenter=""
    authors="markusvi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/24/2016"
    ms.author="markvi"/>



# <a name="set-up-automatic-registration-of-windows-domain-joined-devices-with-azure-active-directory"></a>设置自动注册的 Windows Azure Active Directory 域内的设备

若要使用[Azure Active Directory 基于设备的条件访问](active-directory-conditional-access.md)，必须使用 Azure 活动目录 (AD Azure) 注册 Windows 加入域的计算机。 在本文中，您可以了解您需要来设置的 Windows Azure 广告在您的组织的加入域的设备注册。

在 Azure AD 中使用条件的访问提供了这些优势︰

-   在 Azure AD 应用程序通过工作或学校账户中增强单一登录 (SSO) 体验
-   企业在设备之间漫游的设置
-   访问 Windows 应用商店的业务
-   更强的身份验证和方便登录 Windows Hello

> [AZURE.NOTE] Windows 10 11 月更新提供了一些增强的用户体验在 Azure 的广告，但 Windows 10 周年更新完全支持基于设备的条件访问。 关于条件访问的详细信息，请参阅[Azure Active Directory 基于设备的条件访问](active-directory-conditional-access.md)。 有关在工作场所和用户如何使用 Azure AD 注册 Windows 10 设备的 Windows 10 设备的详细信息，请参阅[企业的 Windows 10︰ 工作中使用设备](active-directory-azureadjoin-windows10-devices-overview.md)。

您可以注册某些早期版本的 Windows，包括这些版本︰

-   Windows 8.1
-   Windows 7

如果使用的 Windows Server 计算机用作桌面，您可以注册这些平台︰

- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2


## <a name="prerequisites"></a>系统必备组件

通过使用 Azure AD 域内设备的自动注册的主要要求是 Azure 活动目录连接 （Azure AD 连接） 的最新版本。

根据您如何部署 Azure AD 连接，以及是否使用快速或自定义安装或就地升级，以下系统必备组件可能已配置自动︰

-   **服务连接点在部署 Active Directory**中。 发现的 Azure AD 租户信息通过计算机的注册 Azure 的广告。

-   **Active Directory 联合身份验证服务 (AD FS) 颁发转换规则**。 注册 （适用于联合配置） 上的计算机身份验证。

如果在您的组织中的某些设备不是 Windows 10 加入域的设备，请确保执行以下任务︰

- 在 Azure AD 中设置一个策略，以便用户可以注册设备
- 作为 AD FS 中的多因素身份验证的有效的替代设置集成 Windows 身份验证 (IWA)



## <a name="set-a-service-connection-point-for-discovery-of-the-azure-ad-tenant"></a>将发现的 Azure AD 租户的服务连接点

服务连接点对象必须存在于配置命名上下文分区的计算机相连的域目录林。 服务连接点包含发现 Azure AD 租户信息位置的计算机注册。 在多林 Active Directory 配置中，该服务连接点必须位于已加入域的计算机的所有林状结构。

这些在 Active Directory 中的位置设置的服务连接点︰

    CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,[Configuration Naming Context]

> [AZURE.NOTE] 使用 Active Directory 域名称*example.com*，配置命名上下文林是 CN = 配置中，DC = 示例中，DC = com。

您可以通过使用下面的 Windows PowerShell 脚本检查 Azure AD 租户对象和发现值。 （使用您配置命名上下文替换示例中的配置命名上下文）。

    $scp = New-Object System.DirectoryServices.DirectoryEntry;

    $scp.Path = "LDAP://CN=62a0ff2e-97b9-4513-943f-0d221bd30080,CN=Device Registration Configuration,CN=Services,CN=Configuration,DC=example,DC=com";

    $scp.Keywords;

`$scp.Keywords`输出显示 Azure 广告租户信息︰

    azureADName:microsoft.com

    azureADId:72f988bf-86f1-41af-91ab-2d7cd011db47

如果服务连接点不存在，则通过 Azure AD 连接的服务器上运行下面的脚本 PowerShell 创建它︰

    Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1";

    $aadAdminCred = Get-Credential;

    Initialize-ADSyncDomainJoinedComputerSync –AdConnectorAccount [connector account name] -AzureADCredentials $aadAdminCred;


> [AZURE.NOTE]当您运行`$aadAdminCred = Get-Credential`，使用格式*user@example.com***获取凭据**对话框中的用户名。  
> 在运行时初始化 ADSyncDomainJoinedComputerSync cmdlet，替换为 [*连接器帐户名称*] 在 Active Directory 连接器帐户使用域帐户。  
> 该 cmdlet 使用活动目录 PowerShell 模块，它依赖于 Active Directory Web 服务在域控制器中。 Windows Server 2008 R2 及更高版本中的域控制器支持活动目录的 Web 服务。 对于 Windows Server 2008 或更早版本中的域控制器，使用 PowerShell 通过 System.DirectoryServices API 来创建服务连接点，并再分配的关键字的值。

## <a name="create-ad-fs-rules-for-instant-device-registration-in-federated-organizations"></a>联盟的组织中创建 AD FS 即时设备注册规则

在联盟的 Azure 中 AD 配置设备依赖于 AD FS （或内部联合身份验证服务器上） 到 Azure AD 身份验证。 然后，它们将对 Azure 活动目录设备注册服务 （Azure 广告设备注册服务）。

> [AZURE.NOTE] 在非联合配置中，用户密码哈希值同步到 Azure 的广告，并且 Windows 10 和 Windows 服务器 2016年加入域的计算机进行身份验证 Azure 广告设备注册服务。 用户使用用户写入自己的本地计算机帐户的凭据进行身份验证并到 Azure AD 通过 Azure AD 连接中继。 对于非 Windows 10 和 Windows 服务器 2016年非联合配置中的计算机，必须设置基于设备的证书颁发机构在您的组织中的选项。 有关详细信息，请参阅本文中的 Windows 10 计算机的部分下载 Windows 安装程序程序包。

对于 Windows 10 和 Windows 服务器 2016年的计算机，Azure AD 连接将设备对象在 Azure AD 与内部的计算机帐户对象。 在完成注册和创建的设备对象的 Azure 广告设备注册服务的身份验证过程必须存在以下声明︰

- http://schemas.microsoft.com/ws/2012/01/accounttype 包含的 DJ 值，它标识为加入域的计算机主体身份验证器。
- http://schemas.microsoft.com/identity/claims/onpremobjectguid 包含本地计算机帐户的**objectGUID**特性的值。
- http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid 包含计算机的主要安全标识符 (SID)，对应的本地计算机帐户的**objectSid**属性值。
- http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid 包含 Azure AD 使用信任从 AD FS 或从内部安全令牌服务 (STS) 颁发的令牌的值。 这一点在多林 Active Directory 配置。 在此配置中，计算机可能会加入到不同于连接到 AD FS 在 Azure 广告或内部 STS 林。 AD FS 中，使用值 http://<*域名*>/adfs/服务/信任/，其中 <*域名*> 位于 Azure AD 验证的域名。

AD FS 中手动创建这些规则请在连接到服务器的会话中使用以下 PowerShell 脚本。 在 Azure AD 替换您已验证的组织的域名的第一行。

> [AZURE.NOTE] 如果 AD FS 不用于内部部署联合身份验证服务器，请按照供应商的说明来创建发出这些声明的规则。

    $validatedDomain = "example.com"      # Replace example.com with your organization's validated domain name in Azure AD

    $rule1 = '@RuleName = "Issue object GUID"

        c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&

        c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]

        => issue(store = "Active Directory", types = ("http://schemas.microsoft.com/identity/claims/onpremobjectguid"), query = ";objectguid;{0}", param = c2.Value);'

    $rule2 = '@RuleName = "Issue account type for domain-joined computers"

      c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]

      => issue(Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", Value = "DJ");'

> [AZURE.NOTE] 如果您的环境上部署单个目录林，使用只有前三个规则。 如果您的计算机位于不同目录林中比与 Azure AD 同步或使用替换名称中的同步配置，还必须包括剩余的规则。

    $rule3 = '@RuleName = "Pass through primary SID"

      c1:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "515$", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"] &&

      c2:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid", Issuer =~ "^(AD AUTHORITY|SELF AUTHORITY|LOCAL AUTHORITY)$"]

      => issue(claim = c2);'

    $rule4 = '@RuleName = "Issue AccountType with the value User when it’s not a computer account"

      NOT EXISTS([Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", Value == "DJ"])

      => add(Type = "http://schemas.microsoft.com/ws/2012/01/accounttype", Value = "User");'

    $rule5 = '@RuleName = "Capture UPN when AccountType is User and issue the IssuerID"

      c1:[Type == "http://schemas.xmlsoap.org/claims/UPN"] &&

      c2:[Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", Value == "User"]

      => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c1.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));'

    $rule6 = '@RuleName = "Update issuer for DJ computer auth"

      c1:[Type == "http://schemas.microsoft.com/ws/2012/01/accounttype", Value == "DJ"]

      => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = "http://'+$validatedDomain+'/adfs/services/trust/");'

    $existingRules = (Get-ADFSRelyingPartyTrust -Identifier urn:federation:MicrosoftOnline).IssuanceTransformRules

    $updatedRules = $existingRules + $rule1 + $rule2 + $rule3 + $rule4 + $rule5 + $rule6

    $crSet = New-ADFSClaimRuleSet -ClaimRule $updatedRules

    Set-AdfsRelyingPartyTrust -TargetIdentifier urn:federation:MicrosoftOnline -IssuanceTransformRules $crSet.ClaimRulesString

> [AZURE.NOTE] Windows 10 和 Windows 服务器 2016年加入域的计算机使用 IWA 承载 AD fs 的活动 WS 信任终结点进行身份验证。 请确保该终结点处于活动状态。 如果您使用的 Web 应用程序代理服务器，请确保此终结点通过代理服务器来发布。 终结点是 adf/服务/信任/13/windowstransport。 若要检查它是否处于活动状态，在 AD FS 管理控制台转到**服务** > **的终结点**。 如果 AD FS 不用于内部部署联合身份验证服务器，请按照供应商的说明进行操作，以确保对应的终结点处于活动状态。



## <a name="set-up-ad-fs-for-authentication-of-device-registration"></a>AD FS 设备注册的身份验证设置

请确保 IWA 设置为 AD FS 中的设备注册的多因素身份验证的有效替代。 若要执行此操作，您需要有转换规则的身份验证方法通过颁发。

1. 在 AD FS 管理控制台中，转到**AD FS** > **信任关系** > **信赖方信任**。

2. Microsoft Office 365 标识平台信赖方信任对象，用鼠标右键单击，然后选择**编辑声明规则**。

3.  在**颁发转换规则**选项卡上，选择**添加规则**。

4.  在**声明规则**模板列表中，选择**发送使用自定义规则的声明**。

5.  选择**下一步**。

6.  在**声明规则名称**框中，输入**授权方法声明规则**。

7.  在**声明规则**框中，输入以下命令︰

    `c:[Type == "http://schemas.microsoft.com/claims/authnmethodsreferences"]
    => issue(claim = c);`.

8. 在联合身份验证服务器上，请输入此 PowerShell 命令︰

    `Set-AdfsRelyingPartyTrust -TargetName <RPObjectName> -AllowedAuthenticationClassReferences wiaormultiauthn`

<*RPObjectName*> 是 Azure AD 信赖方信任对象的信赖方对象名称。 此对象通常称为*Microsoft Office 365 标识平台*。



## <a name="deployment-and-rollout"></a>部署和推广

加入域的计算机满足这些先决条件，当他们准备注册 Azure 的广告。

Windows 10 周年更新和 Windows 服务器 2016年加入域的计算机自动注册 Azure 广告在下次重新启动该设备，或者当用户登录到 Windows。 新加入到域的计算机注册 Azure 广告时加入域操作后重新启动该设备。

> [AZURE.NOTE] Windows 10 加入域的计算机自动注册 Azure 广告仅当部署组策略对象设置。

可以使用组策略对象来控制 Windows 10 和 Windows 服务器 2016年加入域的计算机自动注册的展示。 推出的 Windows 10 加入域的计算机自动注册，可以将 Windows 安装程序程序包部署到您选择的计算机。

> [AZURE.NOTE] 首展控件的组策略还触发 Windows 8.1 加入域的计算机的注册。 注册窗口 8.1 加入域的计算机可以使用该策略。 或者，如果既有的 Windows 版本，包括 Windows 7 或 Windows 服务器版本，则可以使用 Windows 安装程序程序包注册所有 Windows 10 和 Windows 服务器 2016年计算机。

### <a name="create-a-group-policy-object-to-control-the-rollout-of-automatic-registration"></a>创建组策略对象来控制自动注册的展示

要控制的 Azure 广告加入域的计算机的自动注册的推出，可以将**注册为设备加入域的计算机**组策略部署到要注册的计算机。 例如，可以部署策略的组织单位或安全组。

若要设置该策略，请执行以下步骤︰

1. 打开服务器管理器，然后转到**工具** > **组策略管理**。

2. 请转到对应于您要激活的窗口 10 或 Windows 服务器 2016年的计算机自动注册的域的域节点。

3. 用鼠标右键单击**组策略对象**，然后选择**新建**。

4. 输入您的组策略对象的名称。 例如，为*自动注册到 Azure 的广告*。 选择**确定**。

4. 用鼠标右键单击新建的组策略对象，然后选择**编辑**。

5. 转到**计算机配置** > **策略** > **管理模板** > **Windows 组件** > **设备注册**。 **加入注册域的计算机设备**中，用鼠标右键单击，然后选择**编辑**。

    > [AZURE.NOTE] 这个组策略模板已被重命名的组策略管理控制台的早期版本中。 如果您使用的早期版本的控制台，请转到**计算机配置** > **策略** > **管理模板** > **Windows 组件** > **工作场所联接** > **自动工作场所连接客户端计算机**。

6. 选择**已启用**，然后再选择**应用**。

7. 选择**确定**。

8. 将组策略对象链接到您选择的位置。 例如，可以将其链接到特定的组织单位。 您还可以将其链接到 Azure 广告自动注册的计算机的特定安全组。 若要为所有加入域的 Windows 10 和 Windows 服务器 2016年的计算机在您的组织中设置此策略，请到域链接的组策略对象。

### <a name="download-windows-installer-packages-for-non-windows-10-computers"></a>下载 Windows 10 计算机的 Windows 安装程序程序包  

若要注册加入域的计算机运行 Windows 8.1、 Windows 7、 Windows Server 2012 R2、 Windows Server 2012 或 Windows Server 2008 R2，可以下载并安装这些 Windows 安装程序程序包 (.msi) 文件︰

- [x64](http://download.microsoft.com/download/C/A/7/CA79FAE2-8C18-4A8C-A4C0-5854E449ADB8/Workplace_x64.msi)
- [x86](http://download.microsoft.com/download/C/A/7/CA79FAE2-8C18-4A8C-A4C0-5854E449ADB8/Workplace_x86.msi)

通过使用软件分发系统像系统中心配置管理器部署包。 包支持标准的无提示安装选项与*安静*的参数。 系统中心配置管理器 2016年从早期版本中，如同时跟踪已完成的登记的能力提供更多好处。 有关详细信息，请参阅[系统中心 2016年](https://www.microsoft.com/en-us/cloud-platform/system-center)。

安装程序在用户的上下文中运行的系统上创建的计划的任务。 当用户登录到 Windows 时，将触发任务。 任务以静默方式将设备注册 Azure AD 用户凭据后通过 IWA 进行身份验证。 若要查看计划的任务，请转到**Microsoft** > **加入工作区**，然后再转到任务计划程序库。



## <a name="next-steps"></a>下一步行动

- [Azure Active Directory 条件访问](active-directory-conditional-access.md)
