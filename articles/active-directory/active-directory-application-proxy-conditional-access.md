<properties
    pageTitle="条件与 AD Azure 应用程序代理发布的应用程序访问"
    description="介绍如何设置条件发布使用 Azure AD 应用程序代理服务器进行远程访问的应用程序的访问权限。"
    services="active-directory"
    documentationCenter=""
    authors="kgremban"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.workload="identity"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="06/22/2016"
    ms.author="kgremban"/>

# <a name="working-with-conditional-access"></a>使用条件访问

您可以配置访问规则来授予对应用程序使用应用程序代理发布的条件访问。 这使您能够︰

- 要求每个应用程序的多因素身份验证
- 仅当用户不是在工作时需要多因素身份验证
- 阻止用户访问该应用程序，当它们不是在工作

这些规则可以应用到所有用户和组或仅对特定用户和组。 默认规则将应用于所有用户有权访问应用程序。 但是规则还可以为用户指定的安全组成员的限制。  

当用户访问使用 OAuth 2.0，OpenID 连接，SAML 或 WS 联合身份验证联合应用程序评估访问规则。 此外，访问时计算规则使用 OAuth 2.0 和 OpenID 连接刷新令牌用于获取一个访问令牌。

## <a name="conditional-access-prerequisites"></a>条件接收系统必备组件

- 对 Azure Active Directory 高级订阅
- 联合或托管的 Azure Active Directory 租户
- 联合的承租人要求启用多因素身份验证 (MFA)  
    ![配置访问规则-要求多因素身份验证](./media/active-directory-application-proxy-conditional-access/application-proxy-conditional-access.png)

## <a name="configure-per-application-multi-factor-authentication"></a>配置每个应用程序的多因素身份验证
1. 在 Azure 经典门户管理员身份登录。
2. 转到活动目录并选择要在其中启用应用程序代理的目录。
3. 单击**应用程序**并向下的滚动到**访问规则**部分。 使用联合身份验证的使用应用程序代理发布的应用程序，才会出现访问规则部分。
4. 通过选择**启用访问规则****上**启用该规则。
5. 指定的用户和组，这些规则将应用于人。 使用**添加组**按钮以选择一个或多个组的访问规则将应用于哪个。 此对话框还可用于删除所选的组。  当已选中的规则将应用于组时，访问规则将仅强制实施的属于指定的安全组的一个用户。  

  - 若要显式排除规则的安全组，**除**检查，指定一个或多个组。 除外列表中的组成员的用户将不需要执行多因素身份验证。  

  - 如果用户被配置使用每用户多因素身份验证功能，则此设置将优先于应用程序的多因素身份验证规则。 这意味着已配置为每个用户的多因素身份验证的用户需要执行多因素身份验证，即使它们不受应用程序的多因素身份验证规则。 了解更多有关[多因素身份验证和基于用户的设置](../multi-factor-authentication/multi-factor-authentication.md)。

6. 选择您要设置访问规则︰
    - **需要多因素身份验证**︰ 向其应用访问规则的用户将需要完成之前访问该规则所应用到的应用程序的多因素身份验证。
    - **不在工作时需要多因素身份验证**︰ 试图从受信任的 IP 地址访问该应用程序的用户不需要进行多因素身份验证。 多因素身份验证设置页面上，可以配置受信任的 IP 地址范围。
    - **阻止不在工作时访问**︰ 试图访问该应用程序从公司网络外部的用户将无法访问该应用程序。


## <a name="configuring-mfa-for-federation-services"></a>联合身份验证服务配置 MFA
通过 Azure Active Directory 或内部部署多因素身份验证 (MFA) 可能执行的联合承租人，AD FS 服务器。 默认情况下，MFA 会承载 Azure Active Directory 的任何页面上。 为了配置部署 MFA，运行 Windows PowerShell 并且使用 – SupportsMFA 属性设置 Azure 的广告模块。

下面的示例演示如何通过[设置 MsolDomainFederationSettings cmdlet](https://msdn.microsoft.com/library/azure/dn194088.aspx) contoso.com 租户启用内部 MFA:`Set-MsolDomainFederationSettings -DomainName contoso.com -SupportsMFA $true `

除了设置此标志，必须配置联盟的租户 AD FS 实例进行多因素身份验证。 按照部署[Microsoft Azure 多因素身份验证内部](../multi-factor-authentication/multi-factor-authentication-get-started-server.md)的说明。


## <a name="see-also"></a>请参见

- [使用声明感知应用程序](active-directory-application-proxy-claims-aware-apps.md)
- [发布应用程序与应用程序代理](active-directory-application-proxy-publish.md)
- [启用单一登录](active-directory-application-proxy-sso-using-kcd.md)
- [发布应用程序使用您自己的域名](active-directory-application-proxy-custom-domains.md)

最新的新闻和更新，为签出[应用程序代理日志](http://blogs.technet.com/b/applicationproxyblog/)
