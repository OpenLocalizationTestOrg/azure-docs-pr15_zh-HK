<properties
    pageTitle="Azure AD 域服务︰ 向 DIY 域控制器服务比较 Azure AD 域 |Microsoft Azure"
    description="将 Azure Active Directory 域服务与 DIY 的域控制器进行比较"
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
    ms.date="10/01/2016"
    ms.author="maheshu"/>

# <a name="how-to-decide-if-azure-ad-domain-services-is-right-for-your-use-case"></a>如何确定是否 Azure AD 域服务最适合您的用例
Azure AD 域服务使您能够部署您的工作负载在 Azure 基础结构服务，而不必担心如何保持您的身份的基础结构。 此托管的服务是不同于典型的 Windows 服务器的 Active Directory 部署的部署和管理您自己。 轻松部署、 自动化的运行状况监视和补救措施，和云的简单标识基础结构设计服务。 我们不断发展的服务添加常见部署方案的支持。

决定是否使用 Azure AD 域服务或运转并管理您自己 （自己动手） 在 Azure 中的 AD 架构︰

- [Azure AD 域服务提供的功能](active-directory-ds-features.md)的列表，请参阅。

- 检查常见的[Azure AD 域服务的部署方案](active-directory-ds-scenarios.md)。

- 最后，[比较于自己动手 AD 选项的 Azure AD 域服务](active-directory-ds-comparison.md#compare-azure-ad-domain-services-to-diy-ad-domain-in-azure)。


## <a name="compare-azure-ad-domain-services-to-diy-ad-domain-in-azure"></a>比较在 Azure 的 DIY AD 域到 Azure AD 域服务
下表可帮助您决定使用 Azure AD 域服务和管理您自己在 Azure 中的 AD 架构之间。

|**功能**|**Azure AD 域服务**|**在 Azure 的虚拟机中的自己动手广告**|
|---|:---:|:---:|
|[**托管的服务**](active-directory-ds-comparison.md#managed-service)|**& #x 2713;**|**& #x 2715;**|
|[**安全部署**](active-directory-ds-comparison.md#secure-deployments)|**& #x 2713;**|管理员需要确保部署的安全。|
|[**DNS 服务器**](active-directory-ds-comparison.md#dns-server)|**和 #x 2713;**（托管的服务）|**& #x 2713;**|
|[**域或企业管理员权限**](active-directory-ds-comparison.md#domain-or-enterprise-administrator-privileges)|**& #x 2715;**|**& #x 2713;**|
|[**加入域**](active-directory-ds-comparison.md#domain-join)|**& #x 2713;**|**& #x 2713;**|
|[**使用 NTLM 和 Kerberos 域身份验证**](active-directory-ds-comparison.md#domain-authentication-using-ntlm-and-kerberos)|**& #x 2713;**|**& #x 2713;**|
|[**自定义 OU 结构**](active-directory-ds-comparison.md#custom-ou-structure)|**& #x 2713;**|**& #x 2713;**|
|[**架构扩展**](active-directory-ds-comparison.md#schema-extensions)|**& #x 2715;**|**& #x 2713;**|
|[**AD 域目录林信任**](active-directory-ds-comparison.md#ad-domain-or-forest-trusts)|**& #x 2715;**|**& #x 2713;**|
|[**LDAP 读取**](active-directory-ds-comparison.md#ldap-read)|**& #x 2713;**|**& #x 2713;**|
|[**安全 LDAP (LDAPS)**](active-directory-ds-comparison.md#secure-ldap)|**& #x 2713;**|**& #x 2713;**|
|[**LDAP 写**](active-directory-ds-comparison.md#ldap-write)|**& #x 2715;**|**& #x 2713;**|
|[**组策略**](active-directory-ds-comparison.md#group-policy)|简单|完整的|
|[**地理分布式部署**](active-directory-ds-comparison.md#geo-dispersed-deployments)|**& #x 2715;**|**& #x 2713;**|


#### <a name="managed-service"></a>托管的服务
Azure AD 域服务域是由 Microsoft 管理的。 不需要担心补丁、 更新、 监视、 备份，并确保您的域的可用性。 这些管理任务提供了作为一种服务由 Microsoft Azure 对于托管的域。

#### <a name="secure-deployments"></a>安全部署
托管的域安全地锁定根据广告部署 Microsoft 的安全性最佳做法。 这些最佳做法源于广告产品团队数十年经验工程和支持广告的部署。 对于自己动手部署，需要采取具体的部署步骤来锁定/保护下的安全部署。

#### <a name="dns-server"></a>DNS 服务器
Azure AD 域服务托管的域包括托管的 DNS 服务。 AAD DC 管理员组的成员可以管理 DNS 托管域上。 此组的成员给出了完整的 DNS 管理权限的托管域。 可以使用远程服务器管理工具 (RSAT) 程序包中包含的 DNS 管理控制台执行 DNS 管理。

#### <a name="domain-or-enterprise-administrator-privileges"></a>域或企业管理员权限
这些提升的特权不会提供 AAD DS 托管域上。 需要这些提升的权限才能进行安装/运行的应用程序不能运行托管域。 较小的管理权限子集都可以使用名为 AAD DC 管理员委托的管理组的成员。 这些特权包括权限配置 DNS、 配置组策略，获得管理员权限在加入域的计算机等。

#### <a name="domain-join"></a>加入域
虚拟机可以加入类似于如何将计算机加入到 AD 域的托管域。

#### <a name="domain-authentication-using-ntlm-and-kerberos"></a>使用 NTLM 和 Kerberos 域身份验证
使用 Azure AD 域服务，可以使用您公司的凭据进行身份验证的托管域。 凭据保存 Azure AD 租户与同步。 对于同步承租人，Azure AD 连接可以确保，对凭据进行部署的更改同步到 Azure 的广告。 DIY 的域设置，您可能需要设置与内部帐户目录林与他们公司的凭据进行身份验证的用户的域信任关系。 或者，您可能需要设置 AD 复制以确保用户密码同步到 Azure 的域控制器虚拟机。

#### <a name="custom-ou-structure"></a>自定义 OU 结构
AAD DC 管理员组的成员可以创建自定义的托管域中的 Ou。

#### <a name="schema-extensions"></a>架构扩展
不能扩展基础架构的 Azure AD 域服务托管域。 因此，不能提升，转移到 AAD DS 域依赖扩展 AD 架构 （例如，用户对象下的新属性） 应用程序。

#### <a name="ad-domain-or-forest-trusts"></a>AD 域或林信任
无法配置托管的域建立信任关系 （入站/出站） 与其他域。 因此，方案，例如资源目录林部署或在您不想同步密码的 Azure 广告的情况下不能使用 Azure AD 域服务。

#### <a name="ldap-read"></a>LDAP 读取
托管的域支持 LDAP 读取工作负荷。 因此，您可以部署执行针对托管域 LDAP 读取的操作的应用程序。

#### <a name="secure-ldap"></a>安全 LDAP
您可以配置 Azure AD 域服务，以提供对您的托管域，包括通过 internet 安全 LDAP 访问。

#### <a name="ldap-write"></a>LDAP 写
托管的域是用户对象为只读。 因此，执行针对属性的用户对象的 LDAP 写操作的应用程序不工作在管理域中。 此外，用户密码不能更改受管理的域中。 另一个例子就是修改组成员身份或组属性中的托管域，这不允许。 但是，用户属性或密码在 Azure （通过 PowerShell/Azure 门户） 广告中进行的任何更改或内部部署 AD 同步到 AAD DS 托管域。

#### <a name="group-policy"></a>组策略
AAD DS 托管域不支持复杂的组策略结构。 例如，无法创建和部署单独的 Gpo 的域中的每个自定义 OU 或使用 WMI 筛选 GP 确立目标。 还有一个内置的 GPO 每个为 AADDC 计算机和 AADDC 用户容器，可以自定义配置组策略。

#### <a name="geo-dispersed-deployments"></a>地理分散的部署
Azure AD 域服务托管的域是 Azure 中的单个虚拟网络中可用。 需要域控制器可以在 Azure 的多个区域遍及世界各地的情况下，设置 Azure IaaS Vm 中的域控制器可能是更好的选择。


## <a name="do-it-yourself-diy-ad-deployment-options"></a>自己动手 (DIY) 广告的部署选项
您可能必须部署用例需要一些 Windows 服务器 AD 安装所提供的功能。 在这些情况下，考虑下自己动手 (DIY) 选项之一︰

- **独立云域︰**您可以设置一个独立使用 Azure 已经配置为域控制器的虚拟机云域。 此基础结构没有与您的部署集成 AD 环境。 此选项需要一组不同的云凭据登录/管理在云环境中的虚拟机。

- **资源目录林部署︰**您可以设置一个域在资源目录林拓扑中，使用 Azure 的虚拟机配置为域控制器。 接下来，您可以使用您内部配置 AD 信任关系广告环境。 可以加入域的计算机 (Azure Vm) 到云中的这个资源林。 用户身份验证发生于任何 VPN/ExpressRoute 连接到您的内部目录。

- **将内部域扩展到 Azure:**可以将 Azure 虚拟网络连接到内部网络使用的 VPN/ExpressRoute 连接，以便 Azure 虚拟机可以连接到您在部署 AD。 另一种选择是升级副本在 Azure 作为 VM 内部域的域控制器。 您然后可以设置它通过 VPN/ExpressRoute 连接到您的内部目录复制。 这种部署模式有效地扩展到 Azure 的内部域。

> [AZURE.NOTE] 您可能会确定一个 DIY 的选项更好地适合您的部署使用的情况。 考虑[共享反馈](active-directory-ds-contact-us.md)，以帮助我们了解功能将帮助您在以后选择 Azure AD 域服务。 这种反馈可以帮助我们改进服务，使其更适合您的部署需求和用例。

我们已经发布了[部署 Windows 服务器 Active Directory Azure 虚拟机上的准则](https://msdn.microsoft.com/library/azure/jj156090.aspx)，以帮助使 DIY 的安装更容易。


## <a name="related-content"></a>相关的内容
- [功能-Azure AD 域服务](active-directory-ds-features.md)

- [部署方案的 Azure AD 域服务](active-directory-ds-scenarios.md)

- [部署 Windows Azure 的虚拟机服务器 Active Directory 的准则](https://msdn.microsoft.com/library/azure/jj156090.aspx)
