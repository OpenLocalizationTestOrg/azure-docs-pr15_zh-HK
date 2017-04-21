<properties
    pageTitle="Azure 的 Active Directory 域服务概述 |Microsoft Azure"
    description="Azure 的 Active Directory 域服务概述"
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

## <a name="overview"></a>概述
Azure 的基础结构服务使您能够部署范围广泛的计算解决方案，以敏捷的方式。 使用 Azure 虚拟机，可以几乎立即部署，只按分钟付费。 对于 Windows、 Linux、 SQL Server、 Oracle、 IBM、 SAP、 和 BizTalk 使用支持，您可以部署任何负载，任何语言中，几乎所有的操作系统上。 这些好处使您能够将内部部署的旧式应用程序迁移到 Azure，以节省运营费用。

将内部应用程序迁移到 Azure 的一个重要方面处理这些应用程序的身份需求。 支持目录的应用程序可能会读取或写入访问权限的公司目录为依赖 LDAP 或依赖于 Windows 集成身份验证 （Kerberos 或 NTLM 身份验证） 最终用户进行身份验证。 在 Windows 服务器上运行的业务线 (LOB) 应用程序通常部署在加入域的计算机，以便可以安全地使用组策略管理它们。 升降班次部署到云的应用程序，这些依赖企业标识基础结构需要解决。

管理员常常会下的解决方案，以满足他们在 Azure 中部署的应用程序的标识需要之一︰

- 将部署 Azure 基础设施服务和内部部署企业目录中运行的工作负载之间的站点到站点 VPN 连接。
- 通过设置使用 Azure 的虚拟机的副本域控制器扩展企业 AD 域/目录林基础结构。
- 部署独立域在 Azure 使用 Azure 的虚拟机作为部署域控制器。

所有这些方法从高成本和管理开销会受到影响。 管理员需要部署域控制器在 Azure 中使用虚拟机。 此外，他们需要管理、 安全、 补丁、 监视、 备份和解决这些虚拟机。 VPN 连接到内部部署目录的依赖导致部署在 Azure 容易受到暂时的网络故障或停机的工作负载。 反过来，这些网络中断导致较低的正常运行时间以及这些应用程序的可靠性降低。

我们设计了 Azure AD 域服务，以提供更方便的替代方案。


## <a name="introducing-azure-ad-domain-services"></a>引入了 Azure AD 域服务
Azure AD 域服务提供托管的域服务，如域加入，完全与兼容的 Windows 服务器的 Active Directory 组策略中，LDAP，Kerberos/NTLM 验证。 您可以使用这些域服务而无需您可以部署、 管理和修补程序在云环境中的域控制器。 Azure AD 域服务集成现有的 Azure AD 租户，从而使用户能够使用其公司的凭据登录。 此外，您可以使用现有的组和用户帐户来安全地访问资源，从而确保更顺利地提起和-移位的 Azure 的基础结构服务的内部资源。

无论 Azure AD 租户是否仅云或与您的内部部署 Active Directory 同步，azure AD 域服务功能无缝工作。

### <a name="azure-ad-domain-services-for-cloud-only-organizations"></a>仅云组织 azure AD 域服务
仅云 Azure 的广告 （通常称为托管承租人） 的租户不具有任何内部标识占用的空间。 换句话说，用户帐户，其密码和组成员身份是云-即，创建和管理在 Azure AD 中的所有本机。 考虑一下 Contoso 就只有云 Azure AD 租户。 下面的插图所示，Contoso 管理员已配置虚拟网络在 Azure 的基础结构服务。 在 Azure 的虚拟机中此虚拟网络中所部署的应用程序和服务器的工作负载。 由于 Contoso 仅云租户，所有用户的身份，其凭据，并且创建和管理在 Azure AD 组成员身份。

![Azure AD 域服务概述](./media/active-directory-domain-services-overview/aadds-overview.png)

Contoso 的 IT 管理员可以启用其 Azure AD 租户的 Azure AD 域服务，然后选择该虚拟网络中提供域服务。 此后，Azure AD 域服务规定托管的域，并使其可在虚拟的网络中。 所有用户帐户、 组成员身份和用户凭据在 Contoso 的 Azure AD 租户可在新创建的此域中都也可用。 此功能使组织中的用户登录到域使用其公司凭据-例如，当远程连接到远程桌面通过加入域的计算机。 管理员可以设置中使用现有的组成员身份的域资源的访问。 在虚拟网络上的虚拟机中部署的应用程序可以使用功能，如域加入、 LDAP 读取、 LDAP 绑定，NTLM 和 Kerberos 身份验证，以及组策略。

Azure AD 域服务配置管理域的几个突出的方面如下所示︰

- Contoso 的 IT 管理员不需要管理、 修补程序，或监视此域或此管理域的任何域控制器。
- 没有必要管理 AD 复制此域。 用户帐户、 组成员身份和从 Contoso 的 Azure AD 租户的凭据是此托管域内自动可用。
- 由于域由 Azure AD 域服务，Contoso 的 IT 管理员没有此域的域管理员或企业管理员权限。


### <a name="azure-ad-domain-services-for-hybrid-organizations"></a>对于混合组织的 azure AD 域服务
具有混合的 IT 基础结构的组织使用云资源和内部资源的组合。 这样的组织同步到其 Azure AD 租户从其内部目录标识信息。 在混合组织希望更迁移到云环境，尤其是传统支持目录的应用程序，其内部应用程序的 Azure AD 域服务可以是对它们有用。

立公司已部署了[Azure AD 连接](../active-directory/active-directory-aadconnect.md)，同步对其 Azure AD 租户从其内部目录标识信息。 同步标识信息包括用户帐户、 身份验证 （密码同步） 和组成员身份为其凭据哈希值。

> [AZURE.NOTE] **密码同步是必需的对于使用 Azure AD 域服务的混合组织**。 这一要求是因为用户的凭据在 Azure AD 域服务所提供的托管域需要这些用户通过 NTLM 或 Kerberos 身份验证方法进行身份验证。

![Azure AD 域服务立公司](./media/active-directory-domain-services-overview/aadds-overview-synced-tenant.png)

上图显示与混合 IT 基础结构，如立公司企业可以如何使用 Azure AD 域服务。 立的应用程序和服务器工作负载所需域服务部署在 Azure 的基础结构服务的虚拟网络。 立的 IT 管理员可以启用其 Azure AD 租户的 Azure AD 域服务并选择使托管的域在这个虚拟的网络中可用。 立是一个混合的 IT 基础结构的组织，因为用户帐户、 组和凭据将同步到其 Azure AD 租户从其内部目录。 此功能使用户能够登录到域使用其企业凭据-例如，当远程连接到的计算机加入到域中通过远程桌面。 管理员可以设置中使用现有的组成员身份的域资源的访问。 在虚拟网络上的虚拟机中部署的应用程序可以使用功能，如域加入、 LDAP 读取、 LDAP 绑定，NTLM 和 Kerberos 身份验证，以及组策略。

Azure AD 域服务配置管理域的几个突出的方面如下所示︰

- 托管的域是一个独立的域。 它不是立的内部域的扩展。
- 立的 IT 管理员不需要管理修补程序，或监视此管理域的域控制器。
- 没有必要管理 AD 复制到此域。 用户帐户、 组成员身份和凭据从立的内部目录同步到 Azure 广告通过 Azure AD 连接。 这些用户帐户、 组成员身份和凭据是托管域内自动可用。
- 由于域由 Azure AD 域服务，立的 IT 管理员没有此域的域管理员或企业管理员权限。


## <a name="benefits"></a>优点
Azure AD 域服务，您可以享受以下好处︰

-   **简单**，能够满足虚拟机部署到 Azure 基础结构服务，与几个简单的点击标识需要。 您不需要部署和管理 Azure 或安装连接回您的内部标识基础结构中标识基础结构。

-   **集成版**– Azure AD 域服务紧密地集成与 Azure AD 租户。 现在，您可以使用 Azure 广告作为解决了现代应用程序和传统的支持目录的应用程序都需要一个集成的基于云的企业目录。

-   **兼容**– Azure AD 域服务基于 Windows 服务器的 Active directory 的经验证的企业级基础架构。 因此，您的应用程序可依赖于更大程度的与 Windows 服务器的 Active Directory 功能的兼容性。 不是所有的 Windows 服务器 AD 中可用功能目前在 Azure AD 域服务。 但是，可用功能将与相应的 Windows 服务器 AD 功能依赖内部部署基础结构中。 LDAP、 Kerberos、 NTLM、 组策略和域联接功能构成了成熟的产品，经过测试和改进各种 Windows 服务器版本上。

-   **经济**– 使用 Azure AD 域服务，可以避免与管理标识基础结构以支持传统的支持目录的应用程序相关联的基础架构和管理负担。 您可以移动这些 Azure 的基础结构服务的应用程序并从中获益更节省运营费用。
