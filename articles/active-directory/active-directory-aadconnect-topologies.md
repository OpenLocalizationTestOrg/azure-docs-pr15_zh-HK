<properties
    pageTitle="Azure AD 连接︰ 支持的拓扑 |Microsoft Azure"
    description="本主题详细介绍了支持和不支持的拓扑的 Azure AD 连接"
    services="active-directory"
    documentationCenter=""
    authors="AndKjell"
    manager="femila"
    editor=""/>
<tags
    ms.service="active-directory"
    ms.devlang="na"
    ms.tgt_pltfrm="na"
    ms.workload="identity"
    ms.topic="article"
    ms.date="10/20/2016"
    ms.author="billmath"/>

# <a name="topologies-for-azure-ad-connect"></a>Azure AD 拓扑连接
本主题的目的是为了说明不同的内部部署和 Azure AD 拓扑结构的使用 Azure AD 连接同步作为重要的集成解决方案。 它说明支持和不支持的配置。

文档中的图片的的图例︰

说明 | 图标
-----|-----
内部部署 Active Directory 目录林| ![广告](./media/active-directory-aadconnect-topologies/LegendAD1.png)
与筛选导入的活动目录| ![广告](./media/active-directory-aadconnect-topologies/LegendAD2.png)
Azure AD 连接同步服务器| ![同步](./media/active-directory-aadconnect-topologies/LegendSync1.png)
Azure AD 连接同步服务器"临时模式"| ![同步](./media/active-directory-aadconnect-topologies/LegendSync2.png)
GALSync 与 FIM2010 或 MIM2016| ![同步](./media/active-directory-aadconnect-topologies/LegendSync3.png)
Azure AD 连接同步服务器，详细的| ![同步](./media/active-directory-aadconnect-topologies/LegendSync4.png)
Azure 的广告目录 |![AAD](./media/active-directory-aadconnect-topologies/LegendAAD.png)
不受支持的方案 | ![不受支持](./media/active-directory-aadconnect-topologies/LegendUnsupported.png)

## <a name="single-forest-single-azure-ad-tenant"></a>单一目录林中，单一 Azure AD 租户
![单林单租户](./media/active-directory-aadconnect-topologies/SingleForestSingleDirectory.png)

最常见的拓扑结构是采用单目录林内部，与一个或多个域，并且单个 Azure AD 租户。 对于 Azure AD 身份验证，使用密码同步。 Azure AD 连接快速安装支持仅此拓扑。

### <a name="single-forest-multiple-sync-servers-to-one-azure-ad-tenant"></a>单个目录林，到一个 Azure AD 租户的多个同步服务器
![单林筛选不受支持](./media/active-directory-aadconnect-topologies/SingleForestFilteredUnsupported.png)

不支持具有多个连接到相同的 Azure AD 租户，除了[中间服务器](#staging-server)的 Azure AD 连接同步服务器。 即使这些被配置为同步互相排斥的对象，也是不受支持。 您可能考虑过这如果不能达到从一台服务器或跨多个服务器分配负载林中的所有域。

## <a name="multiple-forests-single-azure-ad-tenant"></a>多个林，单 Azure AD 租户
![多林单租户](./media/active-directory-aadconnect-topologies/MultiForestSingleDirectory.png)

许多组织都有有多个内部活动目录林环境。 有各种原因，有多个内部部署 Active Directory 目录林。 典型的示例是在合并或收购之后与帐户资源目录林，因此设计。

当您有多个林，所有林状结构必须到达 Azure 的单个 AD 连接的同步服务器。 不需要将服务器加入到域。 如有必要可以到达所有林，服务器可以被置于 DMZ 的网络。

Azure AD 连接安装向导会提供几个选项可以整合表示在多目录林中的用户。 目标是 Azure AD 中一次仅表示表示用户。 有常见的拓扑结构，您可以配置在安装向导中的自定义安装路径。 选择表示您的拓扑结构，**唯一地标识您的用户**页上的相应选项。 只有为用户配置了整合。 与默认配置不整合重复的组。

下一节讨论了常见的拓扑︰[单独的拓扑结构](#multiple-forests-separate-topologies)、[交错](#multiple-forests-full-mesh-with-optional-galsync)和[客户资源](#multiple-forests-account-resource-forest)。

假定在 Azure AD 连接同步中的默认配置︰

1. 用户有一个启用的帐户，此帐户所在的林用来对用户进行身份验证。 这种假定是这两个密码同步和联盟。 范围内和 sourceAnchor/immutableID 来自该目录林。
2. 用户有一个邮箱。
3. 林承载用户的邮箱具有属性可见 Exchange 全球通讯簿 (GAL) 中的最佳数据质量。 如果用户没有邮箱，然后可以使用任何林参与这些属性值。
4. 如果您有一个链接的邮箱，然后还有另一个帐户用于登录不同林中。

如果您的环境与这些假设不匹配，会发生下列情况︰

- 如果您有多个活动帐户或多个邮箱，同步引擎将从中任选一个，忽略其他。
- 与任何其他活动帐户链接的邮箱不会导出到 Azure 的广告。 用户帐户不是任何组的成员。 在目录同步链接的邮箱总是会表示为正常的邮箱。 此更改是有意不同的行为，以更好地支持多目录林方案。

[了解默认配置](active-directory-aadconnectsync-understanding-default-configuration.md)中找不到更多详细信息。

### <a name="multiple-forests-multiple-sync-servers-to-one-azure-ad-tenant"></a>多个目录林，到一个 Azure AD 租户的多个同步服务器
![多林多同步支持](./media/active-directory-aadconnect-topologies/MultiForestMultiSyncUnsupported.png)

不支持具有多个 Azure AD 连接同步服务器连接到单个 Azure AD 租户。 例外情况是[中间服务器](#staging-server)的使用。

### <a name="multiple-forests--separate-topologies"></a>多个目录林 – 单独的拓扑结构
**用户一次只能表示跨所有目录**

![多个林中的用户一次](./media/active-directory-aadconnect-topologies/MultiForestUsersOnce.png)

![单独的多目录林拓扑](./media/active-directory-aadconnect-topologies/MultiForestSeperateTopologies.png)

在此环境中，所有目录林内部被视为单独的实体，用户不会出现在任何其他目录林。
每个目录林都有它自己的 Exchange 组织，在目录林之间不 GALSync。 此拓扑可以合并/收购后或在组织正在每个业务部门之间的相互隔离的情况。 这些林 Azure 的广告在同一组织中和用统一的全球通讯簿中出现。
在此图中，每个目录林中每个对象是出现一次中元节和聚合在 Azure 的广告目标租户。

### <a name="multiple-forests--match-users"></a>多个目录林 – 匹配用户
**用户标识存在跨多个目录**

所有这些情形下，通常是通讯组和安全组可以包含用户、 联系人和 Fsp （外部安全主体） 的组合

Fsp 使用相加来表示从另一个目录林中的安全组中的成员。 所有 Fsp Azure AD 中都被解析为实际的对象。

### <a name="multiple-forests--full-mesh-with-optional-galsync"></a>多个目录林-使用可选 GALSync 交错
**跨多个目录，用户的身份存在。使用匹配︰ 邮件属性**

![多目录林的用户的邮件](./media/active-directory-aadconnect-topologies/MultiForestUsersMail.png)

![多林交错](./media/active-directory-aadconnect-topologies/MultiForestFullMesh.png)

完整的网状拓扑中允许位于任何目录林中的用户和资源，并通常会有双向信任目录林之间。

如果 Exchange 在多个目录林，则可以选择可能内部 GALSync 解决方案。 每个用户会表示为目录林中所有其他联系人。 GALSync 通常使用 2010 最前沿标识管理器或 Microsoft 标识管理器 2016年来实现。 Azure AD 连接无法用于内部部署 GALSync。

在这种情况下，标识对象使用邮件属性进行联接。 在一个目录林中的邮箱与用户联接中另一个目录林中的联系人。

### <a name="multiple-forests--account-resource-forest"></a>多个目录林-帐户资源目录林
**跨多个目录，用户的身份存在。使用匹配︰ ObjectSID 和 msExchMasterAccountSID 属性**

![多林用户 ObjectSID](./media/active-directory-aadconnect-topologies/MultiForestUsersObjectSID.png)

![多林 AccountResource](./media/active-directory-aadconnect-topologies/MultiForestAccountResource.png)

在帐户资源目录林拓扑中，您可以与活动用户帐户的一个或多个帐户目录林。 您还可以使用禁用帐户的一个或多个资源目录林。

在这种情况下一个 （或多个）**资源目录林**信任**帐户目录林**的所有。 资源林通常具有与 Exchange 和 Lync 扩展的 AD 架构。 所有的交换和 Lync 服务以及其他共享的服务都位于此目录林。 用户可以在此目录林中禁用的用户帐户和邮箱被链接到帐户目录林。

## <a name="office-365-and-topology-considerations"></a>Office 365 和拓扑注意事项
某些 Office 365 的工作负荷对受支持的拓扑具有某些限制。 如果您计划使用任何一种，然后读取工作负荷的受支持的拓扑主题。

工作负荷 |  
--------- | ---------
联机交换 | 如果有多个 Exchange 组织内部 （即 Exchange 已部署到多个林），则必须使用 Exchange 2013 SP1 或更高版本。 详细信息可以在以下位置找到︰[混合部署具有多个活动目录林](https://technet.microsoft.com/library/jj873754.aspx)
Skype 的业务 | 如果您使用多个目录林内部，则支持帐户资源林拓扑。 详细介绍了有关受支持的拓扑可以在以下位置找到︰[业务服务器 2015年的 Skype 的环保要求](https://technet.microsoft.com/library/dn933910.aspx)

## <a name="staging-server"></a>临时服务器
![临时服务器](./media/active-directory-aadconnect-topologies/MultiForestStaging.png)

Azure AD 连接支持在**调试模式下**安装另一台服务器。 在此模式下的服务器从所有连接目录中读取数据，但不会写入任何连接目录。 它使用常规同步周期，因此已标识数据的更新的副本。 在灾难中的主服务器出现故障时可以故障转移到阶段服务器。 在 Azure AD 连接向导来执行此操作。 由于没有基础结构与主服务器共享，最好可以在不同数据中心位于此第二个服务器。 您必须手动复制到第二台服务器的主服务器上进行任何配置更改。

临时服务器还可以用于测试新的自定义配置并对数据的影响。 您可以预览所做的更改和调整配置。 满意新配置后，可以使活动服务器的临时服务器和旧的活动服务器设置在调试模式。

此方法还可用于替换活动同步服务器。 准备新服务器，并将其设置在调试模式。 请确保处于良好状态，禁用调试模式下 （使其处于活动状态），并且关闭当前处于活动状态的服务器。

很可能有多个临时服务器，当您想要在不同的数据中心中有多个备份。

## <a name="multiple-azure-ad-tenants"></a>多个 Azure AD 承租人
Microsoft 建议单租户在 Azure AD 的组织。
您计划使用多个 Azure AD 承租人之前，这些主题介绍了允许您使用单个租户的常见方案。

主题 |  
--------- | ---------
使用管理单位的委派 | [在 Azure 广告管理单位管理](active-directory-administrative-units-management.md)

![多林多租户](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectory.png)

没有 Azure AD 连接同步服务器和 Azure AD 租户之间 1:1 的关系。 对于每个 Azure AD 租户，您需要一个 Azure AD 连接同步服务器安装。 Azure AD 租户实例是由独立的设计，在其中一个用户看不到其他组织中的用户。 如果是这种分离，则这是一个受支持的配置，但否则您应使用单个 Azure AD 租户模型。

### <a name="each-object-only-once-in-an-azure-ad-tenant"></a>每个对象在 Azure AD 租户仅一次
![筛选的单林](./media/active-directory-aadconnect-topologies/SingleForestFiltered.png)

在此拓扑中，一个 Azure AD 连接同步服务器相连每个 Azure AD 租户。 Azure AD 连接同步服务器必须配置筛选使每个有互相排斥的对象进行操作。 例如可以限制到特定域或组织单位的每个服务器的范围。 一个 DNS 域只能在单个注册 Azure AD 租户。 Upn 广告必须在内部部署中的用户使用不同的命名空间以及。 例如，在图片上三个单独的 UPN 后缀中注册内部部署 AD: contoso.com、 fabrikam.com 和 wingtiptoys.com。 在每个用户部署 AD 域使用不同的命名空间。

Azure AD 租户实例之间没有任何 GALsync。 通讯簿在 Exchange 联机和 Skype 业务仅显示为中的用户相同的租户。

这种拓扑具有以下限制的; 否则为支持方案︰

- 只有一个 Azure AD 承租人可以启用 Exchange 使用内部部署 Active Directory 的混合。
- Windows 10 设备只能与一个 Azure AD 租户。

互斥对象组的要求同样适用于写回。 因为这些功能假定部署单个配置使用此拓扑不支持某些写回功能︰

-   与默认配置组写回
-   设备写回

### <a name="each-object-multiple-times-in-an-azure-ad-tenant"></a>多次在 Azure AD 租户每个对象
![单林多租户不受支持](./media/active-directory-aadconnect-topologies/SingleForestMultiDirectoryUnsupported.png) ![不受支持的单林多连接器](./media/active-directory-aadconnect-topologies/SingleForestMultiConnectorsUnsupported.png)

- 不支持同步多个 Azure AD 承租人为同一个用户。
- 它不支持以使配置更改，以使用户在一个 Azure 广告作为联系人出现在另一个 Azure AD 租户。
- 它不支持修改 Azure AD 连接同步连接到多个 Azure AD 承租人。

### <a name="galsync-by-using-writeback"></a>GALsync 使用回写
![MultiForestMultiDirectoryGALSync1Unsupported](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync1Unsupported.png) ![MultiForestMultiDirectoryGALSync2Unsupported](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync2Unsupported.png)

通过独立的设计都是 azure AD 承租人。

- 它是不受支持，更改配置的 Azure AD 连接同步从另一个 Azure AD 租户读取数据。
- 它不支持导出用户作为联系人到另一个内部使用 Azure AD 连接同步的广告。

### <a name="galsync-with-on-premises-sync-server"></a>GALsync 与内部同步服务器
![MultiForestMultiDirectoryGALSync](./media/active-directory-aadconnect-topologies/MultiForestMultiDirectoryGALSync.png)

它支持使用两个 Exchange 组织之间的 GALsync 用户 FIM2010/MIM2016 内部部署。 一个组织中的用户显示为外部用户/其他组织中的联系人。 这些不同的内部部署广告然后可以同步到自己 Azure AD 承租人。

## <a name="next-steps"></a>下一步行动
若要了解如何安装用于这些方案的 Azure AD 连接，请参阅[Azure AD 连接的自定义安装](./connect/active-directory-aadconnect-get-started-custom.md)。

了解更多有关[Azure AD 连接同步](active-directory-aadconnectsync-whatis.md)配置。

了解有关[将您的内部标识使用 Azure Active Directory 集成](active-directory-aadconnect.md)。
