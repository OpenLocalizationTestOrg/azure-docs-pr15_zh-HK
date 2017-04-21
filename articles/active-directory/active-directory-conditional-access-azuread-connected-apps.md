<properties
    pageTitle="Azure 的条件访问 SaaS 应用程序 |Microsoft Azure"
    description="在 Azure 广告的条件访问允许您配置每个应用程序的多因素身份验证的访问规则，并且能够禁止不受信任网络上的用户访问。 "
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
    ms.date="09/26/2016"
    ms.author="markvi"/>

# <a name="getting-started-with-azure-active-directory-conditional-access"></a>要开始使用 Azure 活动目录条件访问

Azure 活动条件对于目录访问[SaaS](https://azure.microsoft.com/overview/what-is-saas/)应用程序和 Azure AD 连接的应用程序允许您配置基于组、 位置和应用程序区分大小写的条件访问。 

与基于应用程序区分大小写的条件访问，您可以设置每个应用程序的多因素身份验证 (MFA) 访问规则。 MFA 每个应用程序提供的功能来阻止不受信任网络上的用户的访问权限。 您可以将 MFA 规则应用于指派给应用程序，或仅对用户指定的安全组内的所有用户。  当他们从组织网络内的 IP 地址访问该应用程序，用户可能从 MFA 要求中排除。

这些功能可用于购买 Azure 活动目录特优许可的客户。

## <a name="scenario-prerequisites"></a>方案的前提条件
* Azure 的 Active Directory 特优的许可证

* 联合或托管 Azure Active Directory 租户

* 联合的承租人要求多因素身份验证被启用。

## <a name="configure-per-application-access-rules"></a>配置每个应用程序访问规则

本部分介绍如何配置每个应用程序访问规则。

1. 登录到 Azure 经典门户使用 Azure 的广告就是全局管理员帐户。
2. 在左窗格中，选中**Active Directory**。
3. 在目录选项卡上，选择您的目录。
4. 选择**应用程序**选项卡。
5. 选择该规则将设置为该应用程序。
6. 选择**配置**选项卡。
7. 向下滚动到访问规则部分。 选择所需的访问规则。
8. 指定该规则将应用到的用户。
9. 通过选择**已启用，在**启用该策略。

##<a name="understanding-access-rules"></a>了解访问规则

这部分提供支持条件 Azure 应用程序访问的访问规则的详细的说明。

### <a name="specifying-the-users-the-access-rules-apply-to"></a>指定的用户的访问规则应用于

默认情况下该策略将应用于对应用程序具有访问权限的所有用户。 但是，您还可以限制策略向用户指定的安全组的成员。 **添加组**按钮用于从访问规则将应用于组选择对话框中选择一个或多个组。 此对话框还可用于删除所选的组。 当已选中的规则将应用于组时，访问规则将仅强制实施的属于指定的安全组的一个用户。

安全组也可以显式排除策略中选择**除外**选项并指定一个或多个组。 **除了**列表中的组成员的用户将不受多因素身份验证的要求，即使它们的访问规则应用于某个组的成员。
访问规则如下所示将需要访问该应用程序时使用多因素身份验证经理组中的所有用户。

![设置与 MFA 的条件访问规则](./media/active-directory-conditional-access-azuread-connected-apps/conditionalaccess-saas-apps.png)

## <a name="conditional-access-rules-with-mfa"></a>MFA 条件访问规则
如果用户已使用每用户多因素身份验证功能配置，此设置在用户将组合应用程序的多因素身份验证规则。 这意味着需要已配置为每个用户的多因素身份验证的用户来执行多因素身份验证，即使它们不受应用程序的多因素身份验证规则。 了解有关多因素身份验证和每个用户设置的详细信息。

### <a name="access-rule-options"></a>访问规则选项
支持以下选项︰

* **要求多因素身份验证**︰ 授予的访问规则应用于，用户将需要完成之前访问该策略将应用于应用程序的多因素身份验证。

* **需要在不工作时的多因素身份验证**︰ 来自受信任的 IP 地址的用户不需要进行多因素身份验证。 多因素身份验证设置页面上，可以配置受信任的 IP 地址范围。

* **阻止不在工作时的访问权限**︰ 用户不来自受信任的 IP 地址将被阻止。 多因素身份验证设置页面上，可以配置受信任的 IP 地址范围。

### <a name="setting-rule-status"></a>设置规则状态
访问规则状态允许打开或关闭这些规则。 关闭的访问规则时，不能强制的多因素身份验证要求。

### <a name="access-rule-evaluation"></a>评估访问规则

当用户访问使用 OAuth 2.0，OpenID 连接，SAML 或 WS 联合身份验证联合应用程序评估访问规则。 此外，访问规则计算时的 OAuth 2.0 和 OpenID 连接使用刷新令牌获取一个访问令牌。 如果使用刷新令牌时，策略评估失败，将返回错误**invalid_grant** ，这表明用户需要对客户端重新进行身份验证。

###<a name="configure-federation-services-to-provide-multi-factor-authentication"></a>通过配置联合身份验证服务提供多因素身份验证

通过 Azure Active Directory 或内部部署 MFA 可能执行的联合承租人，AD FS 服务器。

默认情况下，MFA 会承载的 Azure Active Directory 的网页。 若要配置部署 MFA， **– SupportsMFA**属性必须设置为**true** Azure Active Directory 中使用 Windows PowerShell Azure AD 模块。

下面的示例演示如何通过[设置 MsolDomainFederationSettings cmdlet](https://msdn.microsoft.com/library/azure/dn194088.aspx) contoso.com 租户启用内部 MFA:

    Set-MsolDomainFederationSettings -DomainName contoso.com -SupportsMFA $true

除了设置此标志，必须配置联盟的租户 AD FS 实例进行多因素身份验证。 按照[部署 Azure 多因素身份验证](../multi-factor-authentication/multi-factor-authentication-get-started-server.md)的说明。

## <a name="related-articles"></a>相关的文章

- [保护访问 Office 365 和其他应用程序连接到 Azure 活动目录](active-directory-conditional-access.md)
- [在 Azure 的 Active Directory 中的应用程序管理的文章索引](active-directory-apps-index.md)
