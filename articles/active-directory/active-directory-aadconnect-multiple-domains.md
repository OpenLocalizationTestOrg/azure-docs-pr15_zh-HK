<properties
    pageTitle="Azure AD 连接的多个域"
    description="本文档介绍设置和配置多个顶级域 O365 和 Azure 的广告。"
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
    ms.date="08/08/2016"
    ms.author="billmath"/>

# <a name="multiple-domain-support-for-federating-with-azure-ad"></a>对使用 Azure 的广告联盟的多个域支持
以下文档提供有关如何使用多个顶级域和子域与 Office 365 或 Azure AD 域的联盟来指导。

## <a name="multiple-top-level-domain-support"></a>多个顶级域支持
顶级域与 Azure 的广告联盟多，要求联盟与一个顶级域时不需要一些额外的配置。

当域用 Azure 的广告联盟时，Azure 中的域上设置多个属性。  一个重要的一个是 IssuerUri。  这是一个 URI，它通过 Azure 广告中用来标识该标记相关联的域。  URI 不需要解析为任何内容，但它必须是有效的 URI。  默认情况下，Azure 广告设置此联合身份验证服务标识符的值与您在部署中 AD FS 配置。

>[AZURE.NOTE]联合身份验证服务标识符是唯一地标识联合身份验证服务的 URI。  联合身份验证服务是作为安全令牌服务 AD FS 此函数的实例。 

您可以通过使用 PowerShell 命令查看 IssuerUri `Get-MsolDomainFederationSettings - DomainName <your domain>`。

![获得 MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

我们要添加一个以上的顶级域时出现问题。  例如，假设您有安装 Azure 的广告之间的联盟和内部环境。  此文档使用 bmcontoso.com。  现在，我已经添加了第二个、 顶级域，bmfabrikam.com。

![域](./media/active-directory-multiple-domains/domains.png)

当我们试图转换我们的 bmfabrikam.com 域，来进行联合时，我们收到一个错误。  这样做的原因是，Azure 的广告不允许 IssuerUri 属性具有相同值的多个域的约束。  
  

![联合身份验证错误](./media/active-directory-multiple-domains/error.png)

### <a name="supportmultipledomain-parameter"></a>SupportMultipleDomain 参数

解决此问题，我们需要添加不同的 IssuerUri，这可以通过使用`-SupportMultipleDomain`参数。  与以下 cmdlet 使用此参数︰
    
- `New-MsolFederatedDomain`
- `Convert-MsolDomaintoFederated`
- `Update-MsolFederatedDomain`

此参数使 Azure AD 配置 IssuerUri，以便基于域的名称。  这将是唯一跨越目录在 Azure 的广告。  使用参数允许 PowerShell 命令成功完成。

![联合身份验证错误](./media/active-directory-multiple-domains/convert.png)

您看我们的新 bmfabrikam.com 域的设置可以看到以下项目︰

![联合身份验证错误](./media/active-directory-multiple-domains/settings.png)

请注意，`-SupportMultipleDomain`不会更改其仍然被配置为指向 adfs.bmcontoso.com 我们联合身份验证服务的其他终结点。

另一件事， `-SupportMultipleDomain` does 是它可以确保 AD FS 系统在 Azure 广告所颁发的令牌中包含正确的颁发者值。 这是通过获取用户的 UPN 的域部分，并将此值设置为 IssuerUri，即 https://{upn 后缀的域} / adfs/服务/信任。 

因此在对 Azure AD 身份验证或 Office 365，用户的令牌中的 IssuerUri 元素用于在 Azure 广告定位此域。  如果身份验证将失败，找不到匹配项。 

例如，如果用户的 UPN 是bsimon@bmcontoso.com,令牌 AD FS 问题中的 IssuerUri 元素将被设置为 http://bmcontoso.com/adfs/services/trust。 这将匹配的 Azure AD 配置，并且身份验证将会成功。

下面是实现这种逻辑的自定义的声明规则︰

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type =   "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)", "http://${domain}/adfs/services/trust/"));


>[AZURE.IMPORTANT]为了尝试添加新的或已将转换添加域时，请使用-SupportMultipleDomain 开关，您需要让您联合的信任支持它们最初的安装程序。  


## <a name="how-to-update-the-trust-between-ad-fs-and-azure-ad"></a>如何更新 AD FS 和 Azure 的广告之间的信任
如果您没有设置 AD FS 和 Azure AD 实例之间联合的信任，您可能需要重新创建此信任关系。  这是因为当它最初是无需安装`-SupportMultipleDomain`参数，IssuerUri 将使用默认值。  下面您的屏幕截图中可以看到 IssuerUri 设置为 https://adfs.bmcontoso.com/adfs/services/trust。

所以现在，如果我们成功 Azure 广告门户网站中添加新的域，然后尝试转换使用`Convert-MsolDomaintoFederated -DomainName <your domain>`，我们收到下面的错误。

![联合身份验证错误](./media/active-directory-multiple-domains/trust1.png)

如果您尝试添加`-SupportMultipleDomain`开关便会收到以下错误︰

![联合身份验证错误](./media/active-directory-multiple-domains/trust2.png)

只尝试运行`Update-MsolFederatedDomain -DomainName <your domain> -SupportMultipleDomain`的原始域也会导致错误。

![联合身份验证错误](./media/active-directory-multiple-domains/trust3.png)

使用下面的步骤来添加其他的顶级域。  如果您已经添加了某个域，而未使用`-SupportMultipleDomain`参数用于删除和更新您的原始域的步骤开始。  如果您尚未添加顶级域名还可以开头添加域使用 PowerShell Azure AD 连接的步骤。

使用以下步骤删除 Microsoft 在线信任和更新您的原始域。

2.  在 AD FS 联合身份验证服务器打开**AD FS 管理。** 
2.  在左边，展开**信任关系**和**信赖方信任**
3.  在右边，删除**Microsoft Office 365 标识平台**入口。
![删除 Microsoft 在线](./media/active-directory-multiple-domains/trust4.png)
1.  在[Azure 活动目录模块用于 Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx)在其上安装有一台计算机上运行以下命令︰ `$cred=Get-Credential`。  
2.  输入将与联盟的 Azure AD 域的用户名和密码的全局管理员
2.  在 PowerShell 输入`Connect-MsolService -Credential $cred`
4.  在 PowerShell 输入`Update-MSOLFederatedDomain -DomainName <Federated Domain Name> -SupportMultipleDomain`。  这是原始域。  因此使用它将上述域︰`Update-MsolFederatedDomain -DomainName bmcontoso.com -SupportMultipleDomain`


使用以下步骤添加新的顶级域，使用 PowerShell

1.  在[Azure 活动目录模块用于 Windows PowerShell](https://msdn.microsoft.com/library/azure/jj151815.aspx)在其上安装有一台计算机上运行以下命令︰ `$cred=Get-Credential`。  
2.  输入将与联盟的 Azure AD 域的用户名和密码的全局管理员
2.  在 PowerShell 输入`Connect-MsolService -Credential $cred`
3.  在 PowerShell 输入`New-MsolFederatedDomain –SupportMultipleDomain –DomainName`

使用以下步骤添加新的顶级域使用 Azure AD 连接。

1.  启动 Azure AD 连接从桌面或 「 开始 」 菜单
2.  选择"添加其他 Azure AD 域"![额外添加一个 Azure AD 域](./media/active-directory-multiple-domains/add1.png)
3.  输入您的 Azure 广告和 Active Directory 凭据
4.  选择您想要为联盟配置第二个域。
![额外添加一个 Azure AD 域](./media/active-directory-multiple-domains/add2.png)
5.  单击安装


### <a name="verify-the-new-top-level-domain"></a>验证新的顶级域
通过使用 PowerShell 命令`Get-MsolDomainFederationSettings - DomainName <your domain>`您可以查看已更新的 IssuerUri。  下面的屏幕快照显示联盟已在我们的原始域 http://bmcontoso.com/adfs/services/trust 更新设置

![获得 MsolDomainFederationSettings](./media/active-directory-multiple-domains/MsolDomainFederationSettings.png)

和 IssuerUri 在我们新的域已设置为 https://bmfabrikam.com/adfs/services/trust

![获得 MsolDomainFederationSettings](./media/active-directory-multiple-domains/settings2.png)


##<a name="support-for-sub-domains"></a>对子域的支持
由于处理方式 Azure AD 域添加子域时，它将继承父项的设置。  这意味着 IssuerUri 需要匹配父项。

现在，我们说我有 bmcontoso.com，然后添加 corp.bmcontoso.com 的示例。  这意味着需要将用户从 corp.bmcontoso.com IssuerUri **http://bmcontoso.com/adfs/services/trust。**  但是 Azure 的广告，为实现上述标准规则将生成的令牌颁发者为**http://corp.bmcontoso.com/adfs/services/trust。** 这将不匹配的域所需的值，则身份验证将失败。

### <a name="how-to-enable-support-for-sub-domains"></a>如何启用子域的支持
要变通解决此问题 AD FS Microsoft 联机信赖方信任需要更新。  为此，必须配置自定义声明规则以便构建自定义的颁发者值时剔除出任何子域从用户的 UPN 后缀。 

下面的声明将执行此操作︰

    c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^((.*)([.|@]))?(?<domain>[^.]*[.].*)$", "http://${domain}/adfs/services/trust/"));

使用以下步骤添加的自定义声明以支持子域。

1.  打开 AD FS 管理
2.  右键单击 Microsoft 在线 RP 信任并选择编辑声明规则
3.  选择第三个声明规则，并替换![编辑声明](./media/active-directory-multiple-domains/sub1.png)
4.  替换当前的声明︰
    
        c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, ".+@(?<domain>.+)","http://${domain}/adfs/services/trust/"));
        
    使用
    
        `c:[Type == "http://schemas.xmlsoap.org/claims/UPN"] => issue(Type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/issuerid", Value = regexreplace(c.Value, "^((.*)([.|@]))?(?<domain>[^.]*[.].*)$", "http://${domain}/adfs/services/trust/"));`
    
![更换索赔](./media/active-directory-multiple-domains/sub2.png)
5.  单击确定。  单击应用。  单击确定。  关闭 AD FS 管理。

