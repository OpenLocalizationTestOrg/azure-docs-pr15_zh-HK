<properties
    pageTitle="Azure 的 Active Directory 域服务︰ 在托管域同步 |Microsoft Azure"
    description="了解在 Azure Active Directory 域服务托管域同步"
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
    ms.date="10/03/2016"
    ms.author="maheshu"/>

# <a name="synchronization-in-an-azure-ad-domain-services-managed-domain"></a>在 Azure AD 域服务托管域同步
下图演示了如何同步在 Azure AD 域服务托管的域。

![在 Azure AD 域服务的同步拓扑](./media/active-directory-domain-services-design-guide/sync-topology.png)

## <a name="synchronization-from-your-on-premises-directory-to-your-azure-ad-tenant"></a>从您的内部目录到 Azure AD 租户的同步
Azure AD 连接同步用于同步用户帐户、 组成员资格，并且到 Azure AD 租户的凭据进行哈希处理。 用户的属性如 UPN 帐户和本地 SID （安全标识符） 进行同步。 如果您使用 Azure AD 域服务，旧式凭据 NTLM 和 Kerberos 身份验证所需的哈希值也同步到 Azure AD 租户。

如果您配置写回，Azure 的广告目录中发生的变更进行同步回内部活动目录。 例如，如果您更改密码使用 Azure AD 自助密码更改功能，则该更改的密码更新您在部署中 AD 域。

> [AZURE.NOTE] 始终使用最新版本的 Azure AD 连接以确保您拥有所有的已知 bug 的修补程序。


## <a name="synchronization-from-your-azure-ad-tenant-to-your-managed-domain"></a>从 Azure AD 租户到托管域同步
用户帐户、 组成员身份和凭据哈希值从 Azure AD 租户同步到 Azure AD 域服务托管域。 此同步过程是自动的。 不需要配置、 监视或管理此同步过程。 同步过程也是一-双向/单向性质的。 托管的域是很大程度上只读的除了您创建的任何自定义 Ou。 因此，您不能更改用户属性、 用户密码或托管的域中的组成员身份。 因此，从您的托管域更改回 Azure AD 租户没有反向同步。


## <a name="synchronization-from-a-multi-forest-on-premises-environment"></a>多目录林部署环境中的同步
许多组织都有一个相当复杂的内部部署身份基础结构，包含多个帐户目录林。 Azure AD 连接支持同步用户、 组和多目录林环境中到 Azure AD 租户的凭据哈希。

相反，Azure AD 租户是更简单和平面命名空间。 若要使用户能够可靠地访问应用程序保护的 Azure 的广告，来解决 UPN 冲突跨不同目录林中的用户帐户。 Azure AD 域服务的托管的域玩具熊关闭类似于 Azure AD 租户。 因此，您可以在管理域中看到平面 OU 结构。 AADDC 用户容器，而不考虑内部域或林的它们同步中内存储所有用户和组。 您可能已配置层次结构的 OU 结构内部。 但是，您的托管的域仍有简单的平面 OU 结构。


## <a name="exclusions---what-isnt-synchronized-to-your-managed-domain"></a>排除-什么不到托管域同步
下面的对象或属性不同步到 Azure AD 租户或托管域︰

- **属性中排除︰**您可以选择从您使用 Azure AD 连接的内部域同步到 Azure AD 租户中排除某些属性。 在管理域中这些排除的属性不可用。

- **组策略︰**配置内部域中的组策略不会同步到您的托管域。

- **SYSVOL 共享︰**同样，内部域上的 SYSVOL 共享的内容不会同步到托管域。

- **计算机对象︰**不到托管域同步的计算机加入到内部域中的计算机对象。 这些计算机不具有与您的托管域的信任关系，并且属于仅限于内部域。 在管理域中，您发现计算机对象仅对您有明确的加入域到托管域的计算机。

- **为用户和组的 SidHistory 属性︰**主用户和主要组从您的内部域的 Sid 将同步到托管域。 但是，现有的用户和组的 SidHistory 属性不同步从内部域到托管域。

- **的组织单位 (OU) 结构︰**内部域中定义的单位不会同步到您的托管域。 在管理域中有两个内置的 Ou。 默认情况下，托管的域具有平面 OU 结构。 但是，您可能选择为[创建自定义管理域中的 OU](./active-directory-ds-admin-guide-create-ou.md)。


## <a name="how-specific-attributes-are-synchronized-to-your-managed-domain"></a>如何到托管域同步特定的属性
下表列出了一些常见的特性，并介绍到托管域中的同步方式。

| 在管理域中的属性 | 来源 | 备注 |
|:---|:---|:---|
|UPN|在 Azure AD 租户用户的 UPN 属性|从 Azure AD 租户的 UPN 属性是到托管域被同步的。 因此，登录到您的托管域最可靠方法使用您的 UPN。|
|SAMAccountName|用户的 mailNickname 特性在 Azure AD 租户或自动生成|SAMAccountName 特性源自 Azure AD 租户中的 mailNickname 属性。 如果多个用户帐户具有相同的 mailNickname 属性，SAMAccountName 是自动生成的。 如果用户的 mailNickname 或 UPN 前缀的长度超过 20 个字符，SAMAccountName 是自动生成满足 SAMAccountName 属性 20 字符的限制。|
|密码|从 Azure AD 租户的用户的密码|从 Azure AD 租户同步凭据的哈希值所需的 NTLM 或 Kerberos 身份验证 （也称为附加的凭据）。 Azure AD 租户是否已同步的租户，这些凭据被来源于内部域中。|
|主用户/组的 SID|自动生成|用户/组帐户的主 SID 是自动生成的托管域中。 该属性与主要用户/组中您对内部对象的 SID 不匹配 AD 域。 此不匹配是因为托管的域比内部域中具有不同的 SID 名称空间。|
|为用户和组的 SID 历史记录|内部主要用户和组的 SID|托管域中用户和组的 SidHistory 属性设置以匹配相应的主要用户或组内部域中的 SID。 此功能有助于使升降班次到托管域的内部应用程序更容易，因为您不需要重新 ACL 资源。|

> [AZURE.NOTE] **在登录到托管域使用 UPN 格式︰**SAMAccountName 属性可能会自动生成一些托管域中的用户帐户。 如果用户具有很长的 UPN 前缀或多个用户具有相同的 mailNickname 属性，这些用户 SAMAccountName 可能会自动生成。 因此，SAMAccountName 格式 (例如，CONTOSO100\joeuser) 并不总是可靠的方法，以用于登录到域。 用户的自动生成的 SAMAccountName 可能会有所不同从其 UPN 前缀。 使用 UPN 格式 (例如，'joeuser@contoso100.com')若要可靠地登录到托管域。


## <a name="objects-that-are-not-synchronized-to-your-azure-ad-tenant-from-your-managed-domain"></a>从您的托管域到 Azure AD 租户不同步的对象
如本文前面部分所述，没有从您的托管域到 Azure AD 租户同步。 您可以在管理域中选择创建[自定义组织单位 (OU)](./active-directory-ds-admin-guide-create-ou.md) 。 此外，您还可以创建其他 Ou、 用户、 组或这些自定义 Ou 中的服务帐户。 无自定义 Ou 内创建的对象被同步到 Azure AD 租户。 这些对象都可用于仅在托管域内使用。 因此，这些对象不是使用 Azure AD PowerShell 的 cmdlet，Azure 广告图形 API 或使用 Azure AD 管理 UI 可见的。


## <a name="related-content"></a>相关的内容
- [功能-Azure AD 域服务](active-directory-ds-features.md)

- [部署方案的 Azure AD 域服务](active-directory-ds-scenarios.md)

- [Azure AD 域服务的网络注意事项](active-directory-ds-networking.md)

- [开始使用 Azure AD 域服务](active-directory-ds-getting-started.md)
