<properties
    pageTitle="Azure 的 Active Directory 域服务: 特点是 |Microsoft Azure"
    description="Azure 的 Active Directory 域服务的功能"
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
    ms.date="10/07/2016"
    ms.author="maheshu"/>

# <a name="azure-ad-domain-services"></a>Azure AD 域服务

## <a name="features"></a>功能
Azure AD 域服务管理域中提供了以下功能。

- **简单的部署经验︰**您可以启用您使用复印机的 Azure AD 租户的 Azure AD 域服务。 无论 Azure AD 租户是云承租人还是与您的内部目录同步，可以快速设置托管的域。

- **支持域加入︰**您可以轻松地在 Azure 的虚拟网络托管的域可用于加入域的计算机。 域加入体验 Windows 客户端和服务器操作系统无缝地对域由 Azure AD 域服务提供服务的工作原理。 您还可以使用工具对这种域自动的域联接。

- **每个 Azure 的广告目录的一个域实例︰**您可以创建一个 Active Directory 域对于每个 Azure 的广告目录。

- **创建的域的自定义名称︰**您可以创建具有自定义名称 (例如，contoso100.com) 的域使用 Azure AD 域服务。 您可以使用已验证或未验证的域名。 （可选），您还可以使用内置域后缀创建域 (即 *。 onmicrosoft.com) 提供的 Azure 的广告目录。

- **与 Azure AD 集成在一起︰**您不需要配置或管理复制的 Azure AD 域服务。 用户帐户、 组成员身份和用户凭据 （密码） 从 Azure 的广告目录可自动在 Azure AD 域服务。 新的用户、 组或从 Azure AD 租户或您的内部目录对属性的更改将自动同步到 Azure AD 域服务。

- **NTLM 和 Kerberos 身份验证︰**对 NTLM 和 Kerberos 身份验证的支持，您可以部署的应用程序依赖于 Windows 集成身份验证。

- **使用每个密码公司的凭据︰**在 Azure AD 租户的用户的密码使用 Azure AD 域服务。 用户可以使用其公司的凭据对加入域的计算机、 登录以交互方式或通过远程桌面，并对管理域进行身份验证。

- **的 LDAP 绑定和 LDAP 读取支持︰**您可以使用依赖于 LDAP 绑定由 Azure AD 域服务的域中的用户进行身份验证的应用程序。 此外，应用程序从目录查询用户/计算机属性使用 LDAP 读取的操作还可以对 Azure AD 域服务。

- **安全 LDAP (LDAPS):**您可以通过安全 LDAP (LDAPS) 启用目录的访问权限。 都可以在默认情况下，虚拟的网络中安全 LDAP 访问。 但是，您还可以通过互联网启用安全 LDAP 访问。

- **组策略︰**您可以使用一个内置 GPO 每个用户和计算机容器来强制遵守所需安全策略的用户帐户并加入域的计算机。

- **管理 DNS:**AAD DC 管理员组的成员可以管理域使用熟悉的 DNS 管理工具，例如 DNS 管理 mmc 管理单元管理 DNS。

- **创建自定义的组织单位 (Ou):**AAD DC 管理员组的成员可以管理域中创建自定义 Ou。 这些用户授予完全管理特权通过自定义 Ou，以便他们可以添加或删除服务帐户、 计算机、 组等，这些自定义 Ou 中。

- **在 Azure 的多个区域中的可用︰**请参阅[Azure 服务区域的](https://azure.microsoft.com/regions/#services/)页后，可以知道 Azure AD 域服务位于 Azure 区域。

- **高可用性︰**Azure AD 域服务提供为您的域的高可用性。 此功能提供更高服务正常运行和故障的复原能力的保证。 内置的运行状况监视提供新实例来替换失败的实例并提供连续的服务，为您的域的旋转自动从故障的补救措施。

- **使用常用的管理工具︰**可以使用熟悉的 Windows 服务器的 Active Directory 管理工具，如活动目录 PowerShell 的 Active Directory 管理中心管理托管的域。
