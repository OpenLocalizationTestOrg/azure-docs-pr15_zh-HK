<properties
    pageTitle="活动目录联合身份验证服务管理和使用 Azure AD 连接的自定义 |Microsoft Azure"
    description="AD FS 管理 Azure AD 连接和 AD FS 登录使用用户体验 Azure AD 连接并且 PowerShell 的自定义。"
    keywords="AD FS，ADFS，AD FS 管理，AAD 连接，连接，登录，AD FS 自定义功能，修复信任，O365，联盟，信赖方"
    services="active-directory"
    documentationCenter=""
    authors="anandyadavmsft"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/01/2016"
    ms.author="anandy"/>

# <a name="active-directory-federation-services-management-and-customization-with-azure-ad-connect"></a>活动目录联合身份验证服务管理和使用 Azure AD 连接的自定义项

本文章详细信息任务与 Active Directory 联合身份验证服务 (AD FS) 可以通过使用 Microsoft Azure 活动目录连接，再加上可能需要完整的 AD FS 服务器场配置其他常见 AD FS 任务相关。

| 主题 | 它涵盖的内容 |
|:------|:-------------|
|**AD FS 管理**|
|[修复信任](#repairthetrust)| 修复与 Office 365 的联合身份验证信任 |
|[AD FS 服务器添加](#addadfsserver) | 扩大与其他 AD FS 服务器 AD FS 服务器场|
|[添加 AD FS web 应用程序代理服务器](#addwapserver) | 扩大与其他 WAP 服务器 AD FS 服务器场|
|[添加的联盟的域](#addfeddomain)| 添加的联盟的域|
| **AD FS 自定义**|
|[添加自定义的公司徽标或图形](#customlogo)| 自定义 AD FS 登录页上的带有公司徽标和说明 |
|[添加签入说明](#addsignindescription) | 添加登录页面描述 |
|[修改 AD FS 声明规则](#modclaims) | 修改各种联合方案的 AD FS 声明 |

## <a name="ad-fs-management"></a>AD FS 管理

Azure AD 连接提供与 AD FS 可以用最少的用户干预使用 Azure AD 连接向导执行的各种任务。 通过运行向导安装 Azure AD 连接完毕后，您可以再次运行向导以执行其他任务。

### 修复信任<a name=repairthetrust></a>

Azure AD 连接可以检查 AD FS 和 Azure Active Directory 信任的当前运行状况并采取相应的措施来修复信任。 请按照这些步骤来修复 Azure 广告和 AD FS 信任。

1. 选择**修复 AAD 和 ADFS 信任**列表中的其他任务。
![修复 AAD 和 ADFS 信任](media\active-directory-aadconnect-federation-management\RepairADTrust1.PNG)

2. 在**连接到 Azure 的广告**页上，为 Azure 的广告，提供全局管理员凭据，并单击**下一步**。
![连接到 Azure 的广告](media\active-directory-aadconnect-federation-management\RepairADTrust2.PNG)

3. 在**远程访问凭据**页中，输入域管理员凭据。
![远程访问的凭据](media\active-directory-aadconnect-federation-management\RepairADTrust3.PNG)

    单击**下一步**后，Azure AD 连接将证书健康检查，并显示任何问题。

    ![证书的状态](media\active-directory-aadconnect-federation-management\RepairADTrust4.PNG)

    **准备配置**页面将显示将执行修复信任的操作的列表。

    ![准备好进行配置](media\active-directory-aadconnect-federation-management\RepairADTrust5.PNG)

4. 单击**安装**以修复了信任。

>[AZURE.NOTE] Azure AD 连接可以仅修复或法案是自签名的证书。 Azure AD 连接无法修复第三方证书。

### AD FS 服务器添加<a name=addadfsserver></a>

> [AZURE.NOTE] Azure AD 连接需要将 AD FS 服务器添加 PFX 证书文件。 因此，您可以使用 Azure AD 连接配置 AD FS 服务器场时，才执行此操作。

1. 选择**部署更多的联合身份验证服务器**，然后单击**下一步**。
![其他联合身份验证服务器](media\active-directory-aadconnect-federation-management\AddNewADFSServer1.PNG)

2. 在**连接到 Azure 的广告**页上，输入 Azure AD 全局管理员凭据，并单击**下一步**。
![连接到 Azure 的广告](media\active-directory-aadconnect-federation-management\AddNewADFSServer2.PNG)

3. 提供域管理员凭据。
![域管理员凭据](media\active-directory-aadconnect-federation-management\AddNewADFSServer3.PNG)

4. Azure AD 连接会询问您配置您新的 AD FS 服务器场使用 Azure AD 连接时提供的 PFX 文件密码。 单击**输入密码**的 PFX 文件中提供的密码。
![证书密码](media\active-directory-aadconnect-federation-management\AddNewADFSServer4.PNG)

    ![指定 SSL 证书](media\active-directory-aadconnect-federation-management\AddNewADFSServer5.PNG)

5. 在**AD FS 服务器**页上，输入服务器名称或 IP 地址添加到 AD FS 服务器场。
![AD FS 服务器](media\active-directory-aadconnect-federation-management\AddNewADFSServer6.PNG)

6. 单击**下一步**，经过最终**配置**页面。 Azure 广告连接到 AD FS 服务器场添加服务器完成后，将为您提供用于验证的连接的选项。
![准备好进行配置](media\active-directory-aadconnect-federation-management\AddNewADFSServer7.PNG)

    ![安装完成](media\active-directory-aadconnect-federation-management\AddNewADFSServer8.PNG)

### 添加 AD FS web 应用程序代理服务器<a name=addwapserver></a>

> [AZURE.NOTE] Azure AD 连接所需的 PFX 证书文件添加 web 应用程序代理服务器。 因此，您将能够使用 Azure AD 连接配置 AD FS 服务器场时，才执行此操作。

1. 从可用任务列表中选择**部署 Web 应用程序代理**。
![部署 web 应用程序代理服务器](media\active-directory-aadconnect-federation-management\WapServer1.PNG)

2. 提供的 Azure 全局管理员凭据。
![连接到 Azure 的广告](media\active-directory-aadconnect-federation-management\wapserver2.PNG)

3. 在**指定 SSL 证书**页时使用 Azure AD 连接配置 AD FS 服务器场提供的 PFX 文件提供的密码。
![证书密码](media\active-directory-aadconnect-federation-management\WapServer3.PNG)

    ![指定 SSL 证书](media\active-directory-aadconnect-federation-management\WapServer4.PNG)

4. 添加要添加的 web 应用程序代理服务器。 因为 web 应用程序代理服务器可能未加入到域中，向导会询问到要添加的服务器的管理凭据。
![管理服务器凭据](media\active-directory-aadconnect-federation-management\WapServer5.PNG)

5. 在**代理信任凭据**页中，提供管理凭据来配置代理信任和访问 AD FS 服务器场中的主服务器。
![代理服务器信任的凭据](media\active-directory-aadconnect-federation-management\WapServer6.PNG)

6. 在**开始配置**页上，向导会显示将执行的操作的列表。
![准备好进行配置](media\active-directory-aadconnect-federation-management\WapServer7.PNG)

7. 单击**安装**以完成配置。 完成配置后，向导为您提供选项以验证与服务器的连接。 单击**验证**以检查连接。
![安装完成](media\active-directory-aadconnect-federation-management\WapServer8.PNG)

### 添加的联盟的域<a name=addfeddomain></a>

很容易地添加域以将使用 Azure AD 连接联合使用 Azure 的广告。 Azure AD 连接添加联合身份验证的域，并修改声明规则以正确反映颁发者，当您有多个域使用 Azure 的广告联盟。

1. 要添加的联盟的域，请选择该任务**额外添加一个 Azure AD 域**。
![其他 Azure AD 域](media\active-directory-aadconnect-federation-management\AdditionalDomain1.PNG)

2. 在向导的下一页上，提供 Azure AD 全局管理员凭据。
![连接到 Azure 的广告](media\active-directory-aadconnect-federation-management\AdditionalDomain2.PNG)

3. 在**远程访问凭据**页中，提供域管理员凭据。
![远程访问的凭据](media\active-directory-aadconnect-federation-management\additionaldomain3.PNG)

4. 在下一页上，向导将提供与之建立联盟内部目录的 Azure AD 域的列表。 从列表中选择域。
![Azure AD 域](media\active-directory-aadconnect-federation-management\AdditionalDomain4.PNG)

    选择域后，向导将为您提供相应进一步向导将执行的操作和配置的影响方面的信息。 在某些情况下，如果您选择不在 Azure 广告，验证域向导将您提供的信息来帮助您验证域。 有关详细信息，请参阅[添加到 Azure Active Directory 您自定义的域名](active-directory-add-domain.md)。

5. 单击**下一步**，并**准备配置**页面将显示 Azure AD 连接将执行的操作的列表。 单击**安装**以完成配置。
![准备好进行配置](media\active-directory-aadconnect-federation-management\AdditionalDomain5.PNG)

## <a name="ad-fs-customization"></a>AD FS 自定义

以下各节提供有关某些可能需要自定义您 AD FS 登录页时执行的常见任务的详细信息。

### 添加自定义的公司徽标或图形<a name=customlogo></a>

若要更改显示在**登录**页上的公司徽标，请使用以下 Windows PowerShell cmdlet 和语法。

> [AZURE.NOTE] 徽标的推荐的尺寸是 260 x 35@文件大小不能大于 10 KB 的 96 dpi。

    Set-AdfsWebTheme -TargetName default -Logo @{path="c:\Contoso\logo.PNG"}

> [AZURE.NOTE] *TargetName*参数是必需的。 释放与 AD FS 的默认主题名为默认。


### 添加签入说明<a name=addsignindescription></a>

若要添加到**登录页**的登录页上的说明，使用的下列 Windows PowerShell cmdlet 和语法。

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>"

### 修改 AD FS 声明规则<a name=modclaims></a>

AD FS 支持丰富的声明语言，可用于创建自定义声明规则。 有关详细信息，请参阅[声明规则语言的角色](https://technet.microsoft.com/library/dd807118.aspx)。

以下各节描述如何编写自定义规则到 Azure AD 有关在某些情况下的，AD FS 联合身份验证。

#### <a name="immutable-id-conditional-on-a-value-being-present-in-the-attribute"></a>不可变标识条件上存在的属性中的值

Azure AD 连接允许您指定要用作源定位到 Azure AD 同步对象时的属性。 如果自定义属性中的值不为空，可能要发出的不可变标识声明。 例如，您可能**ms-ds-consistencyguid**作为源定位的特性并且想要选择特性具有对它的值的情况下为**ms-ds-consistencyguid**发出**ImmutableID** 。 如果没有针对该属性没有值，发出**objectGuid**作为不可变的 id。  您可以构建自定义声明规则下一节中所述的一套。

**规则 1︰ 查询属性**

    c:[Type == "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"]
    => add(store = "Active Directory", types = ("http://contoso.com/ws/2016/02/identity/claims/objectguid", "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"), query = "; objectGuid,ms-ds-consistencyguid;{0}", param = c.Value);

在本规则中，您正在查询**ms-ds-consistencyguid**和**objectGuid** Active Directory 中的用户的值。 将存储区名称更改为 AD FS 部署适当的存储名称。 对您所定义的**objectGuid**和**ms-ds-consistencyguid**的联盟的正确的索赔类型更改声明类型。

此外，通过**添加**和不是**问题**，避免添加实体传出问题也可以作为中间值中使用的值。 建立要使用不可变的 id 的值后，将发出以后规则中的报销申请

**规则 2︰ 检查用户是否存在 ms-ds-consistencyguid**

    NOT EXISTS([Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"])
    => add(Type = "urn:anandmsft:tmp/idflag", Value = "useguid");

此规则定义临时标志调用**idflag**时为用户填充任何**ms-ds-concistencyguid**设置为**useguid** 。 这背后的逻辑是 AD FS 不允许空的索赔的事实。 因此当规则 1 中添加了索赔 http://contoso.com/ws/2016/02/identity/claims/objectguid 和 http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid，您最终与**msdsconsistencyguid**声称，只有如果用户填充的值。 如果它未被填充，AD FS 可以看到它将具有空值，并立即删除它。 所有对象将都具有**objectGuid**，因此执行规则 1 后，这一点将始终有为。

**规则 3︰ 永恒的 ID 为发出 ms-ds-consistencyguid，如果存在**

    c:[Type == "http://contoso.com/ws/2016/02/identity/claims/msdsconcistencyguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c.Value);

这是隐式的**存在**检查。 如果存在声明的值，然后发出的作为不可变的 id。 上面的示例使用**nameidentifier**索赔。 必须为您的环境中的不可变标识更改为适当的声明类型。

**规则 4︰ 永恒的 ID 为发出 objectGuid 如果 ms-ds-consistencyGuid 不存在**

    c1:[Type == "urn:anandmsft:tmp/idflag", Value =~ "useguid"]
    && c2:[Type == "http://contoso.com/ws/2016/02/identity/claims/objectguid"]
    => issue(Type = "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier", Value = c2.Value);

在本规则中，您只需检查临时标志**idflag**。 您决定是否要发出声明价值基础。

> [AZURE.NOTE] 这些规则的顺序非常重要。

#### <a name="sso-with-a-subdomain-upn"></a>与子域 UPN SSO

您可以添加多个域，来进行联合使用 Azure AD 连接，所述[添加一个新的联盟的域](active-directory-aadconnect-federation-management.md#add-a-new-federated-domain)。 您必须修改 UPN 声明以便颁发者 ID 对应于根域和子域，因为联合的根域还包括子。

默认情况下，颁发者标识的声明规则设置为︰

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

![默认颁发者标识声明](media\active-directory-aadconnect-federation-management\issuer_id_default.png)

默认规则只是将 UPN 后缀，并颁发者标识声明中使用该值。 例如，John 是在 sub.contoso.com 中，用户和 contoso.com 联合使用 Azure 的广告。 约翰进入john@sub.contoso.com作为用户名时登录到 Azure 的广告，和 AD FS 中的规则处理它以如下方式声明默认颁发者 ID。

    c:[Type
    == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(john@sub.contoso.com, “.+@(?<domain>.+)“, “http://${domain}/adfs/services/trust/“));

**声明值︰** http://sub.contoso.com/adfs/services/trust/

若要仅颁发者声明值中的根域，更改以匹配以下声明规则。

    c:[Type == “http://schemas.xmlsoap.org/claims/UPN“]

    => issue(Type = “http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid“, Value = regexreplace(c.Value, “^((.*)([.|@]))?(?<domain>[^.]*[.].*)$”, “http://${domain}/adfs/services/trust/“));

## <a name="next-steps"></a>下一步行动

了解有关[用户登录选项](active-directory-aadconnect-user-signin.md)。
