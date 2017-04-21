
<properties
    pageTitle="Azure 的活动目录条件接收技术参考 |Microsoft Azure"
    description="有条件的访问控制，Azure Active Directory 检查挑选进行用户身份验证时，才允许访问该应用程序的特定条件。 一旦满足了这些条件，是经过身份验证的用户，并允许应用程序访问。"
    services="active-directory"
    documentationCenter=""
    authors="MarkusVi"
    manager="femila"
    editor=""/>

<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="identity" 
    ms.date="10/20/2016"
    ms.author="markvi"/>

# <a name="azure-active-directory-conditional-access-technical-reference"></a>Azure 的活动目录条件接收技术参考

## <a name="services-enabled-with-conditional-access"></a>有条件接收启用的服务
跨各种广告 Azure 应用程序类型支持条件的访问规则。 此列表包括︰

- Azure AD 应用程序库中的联合的应用
- Azure AD 应用程序库中的密码 SSO 应用程序
- Azure 应用程序代理中注册应用程序
- 开发的行的业务和多租户应用程序注册 Azure 的广告
- Visual Studio 联机
- Azure 的远程应用程序
-   Dynamics CRM
- Microsoft Office 365 Yammer
- Microsoft Office 365 Exchange 联机
- Microsoft Office 365 SharePoint Online （包括业务的 OneDrive）


## <a name="enable-access-rules"></a>启用访问规则

每个规则可以启用或禁用每个应用程序基。 当规则是**在**他们将启用并强制用户访问该应用程序。 在**关闭**时他们不会使用并不会影响用户的登录体验。

## <a name="applying-rules-to-specific-users"></a>将规则应用于特定的用户
可以将规则应用于特定组的用户通过设置**应用于**基于安全组。 **应用于****所有用户**或**组**设置可以。 当设置到**所有用户**规则将适用于任何用户对应用程序具有访问权限。 **组**选项允许特定的安全和分发组选择，只有将这些组强制执行规则。

将规则部署，时，常见的是第一次将其有限的试验组成员的用户的一组应用。 完成后可以将规则应用于**所有用户**。 这将导致在组织中所有用户的强制规则。

选择组还可能免除使用**除**选项的策略。 将免除这些组的任何成员，即使它们包含组中出现。

## <a name="at-work-networks"></a>"在工作"网络


使用"At 工作"网络的条件访问规则依赖于信任 Azure 的广告，在已配置的 IP 地址范围，或使用从 AD FS"内部企业网络"声明。 这些规则包括︰

- 需要在不工作时的多因素身份验证
- 阻止不在工作时访问

明确指定的选项"在工作"网络

1. 在[多因素身份验证配置页面](../multi-factor-authentication/multi-factor-authentication-whats-next.md)中配置受信任的 IP 地址范围。 条件的访问策略将使用每个身份验证请求和令牌发放上已配置的范围来评估规则。 
2. 配置使用内部的企业网络所声称，具有使用 AD FS 的联合目录，可以使用此选项。 [了解更多有关内 coronet 索赔](../multi-factor-authentication/multi-factor-authentication-whats-next.md#trusted-ips)。
3. 配置公用 IP 地址范围。 在配置选项卡上为您的目录，您可以设置公用 IP 地址。 条件访问将这些使用，如在工作的 IP 地址，这使其它范围高于 50 IP 地址限制强加 MFA 设置页配置。



## <a name="rules-based-on-application-sensitivity"></a>基于应用程序区分大小写规则

规则配置每个允许高价值的服务的应用程序进行保护而不会影响其他服务的访问。 该应用程序的**配置**选项卡上，可以配置条件的访问规则。 

目前所提供的规则︰

- **要求多因素身份验证**
 - 此策略应用到的所有用户将都需要进行至少一次的多因素身份验证通过身份验证。
 
- **需要在不工作时的多因素身份验证**
 - 如果应用了此策略，所有用户将都需要进行至少一次的多因素身份验证如果它们从非工作远程位置访问该服务。 如果他们移动到远程位置工作，他们将需要访问该服务时执行多因素身份验证。
 
- **阻止不在工作时访问** 
 - 当用户移动工作到远程位置时，如果"阻止不在工作时访问"策略将应用到他们将被阻止。  他们将重新允许时在工作位置的访问权限。


## <a name="related-topics"></a>相关的主题

- [保护访问 Office 365 和其他应用程序连接到 Azure 活动目录](active-directory-conditional-access.md)
- [在 Azure 的 Active Directory 中的应用程序管理的文章索引](active-directory-apps-index.md)
