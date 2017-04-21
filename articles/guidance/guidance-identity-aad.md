<properties
   pageTitle="实施 Azure Active Directory |Microsoft Azure"
   description="如何实现使用 Azure Active Directory 安全混合网络体系结构。"
   services="guidance,virtual-network,active-directory"
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags="azure-resource-manager"/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/28/2016"
   ms.author="telmos"/>

# <a name="implementing-azure-active-directory"></a>实施 Azure 的活动目录

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

本文介绍了集成与 Azure Active Directory 提供基于云的身份验证的内部活动目录 (AD) 域和林的最佳做法。

> [AZURE.NOTE] Azure 具有两种不同的部署模型︰[资源管理器][resource-manager-overview]和经典。 此参考体系结构使用资源管理器，Microsoft 建议为新的部署。

您可以使用目录和身份的服务，例如所提供的广告目录服务 (AD DS) 进行身份验证的标识。 这些标识可以属于用户、 计算机、 应用程序或其他资源构成的一种安全边界。 您可以存放目录和标识服务的场所，但在混合方案中应用程序的元素在 Azure 中的位置，它可以是一种更有效的方法来扩展此功能集成到云。 这种方法有助于减少由发送身份验证引起的滞后时间和本地授权请求从云回目录和身份运行的服务的部署。

Azure 云在实施目录和标识服务提供两种解决方案︰

1. 您可以使用[Azure 活动目录 (AAD)] [azure-active-directory]在云中创建 AD 域并将其链接到内部部署 AD 域。 AAD 使您能够配置单一登录 (SSO) 的用户运行的应用程序通过云访问。 AAD 利用[Azure AD 连接][azure-ad-connect]内部存储库中运行内部复制对象和标识持有云。

    您还可以不使用内部部署目录实现 AAD。 在这种情况下，AAD 充当所有标识信息而不是从内部目录复制的包含数据的主要来源。

    请注意，在云中的目录是****的内部目录扩展。 相反，它是包含相同的对象和标识的副本。 对这些项目内部所做的更改将复制到云，但在中进行更改云**不**复制回内部域。

    >[AZURE.NOTE] Azure 活动目录高级版本支持的用户的密码，在云中执行自助服务密码重置为内部用户启用写回。 这是 AAD 同步到本地目录的唯一信息。 有关不同版本的 AAD 和其功能的详细信息，请参见[Azure 活动目录版本][aad-editions]。

2. 您可以从您的本地数据中心部署虚拟机以在 Azure，扩展现有广告基础结构 （包括 AD DS 和 AD FS） 的域控制器运行 AD DS。 这种方法是为内部网络和 Azure 的虚拟网络连接由 VPN 和/或 ExpressRoute 连接的方案更为常见。 此解决方案还支持双向复制使您在云和内部，进行更改，只要它是最合适的。

这种体系结构侧重于解决方案 1。 关于第二个解决方案的详细信息，请参阅[扩展到 Azure 的 Active Directory][guidance-adds]。

这种体系结构的典型使用情况包括︰

- 提供他们所指定的相同凭据用于内部部署的应用程序的最终用户运行 SaaS 和其他应用程序在云中，SSO。

- 发布内部部署的 web 应用程序通过云为贵组织属于远程用户提供访问权限。

- 实施自助服务功能，为最终用户，如重置其密码，并将委派组管理。 

    >[AZURE.NOTE] 可以由管理员通过组策略控制这些功能。

- 在 Azure 云应用程序宿主的虚拟网络与内部网络不直接链接通过 VPN 隧道或 ExpressRoute 电路的情况。

应注意，AAD 不提供广告的所有功能。 例如，AAD 目前只处理用户身份验证，而不是计算机身份验证。 某些应用程序和服务，例如，SQL Server 可能会要求计算机身份验证本解决方案不适合在这种情况下。 此外，AAD 可能不适合跨边界上的部署/云迁移组件可能位置的系统，而不是只是被复制。

> [AZURE.NOTE] Azure Active Directory 的工作原理的详细说明，注意[Microsoft 活动目录说明][aad-explained]。

## <a name="architecture-diagram"></a>体系结构关系图

下图突出显示此体系结构中的重要组成部分。 在 Azure 中的工作负荷元素有关的详细信息，阅读[的 N 层体系结构在 Azure 上运行虚拟机][implementing-a-multi-tier-architecture-on-Azure]:

> [AZURE.NOTE] 为简单起见，此图仅显示与 AAD，直接相关的连接并不描述 web 浏览器请求重定向或其他协议身份验证和身份联合身份验证过程中可能出现的相关的通信。 例如，用户 （本地或远程） 通常通过浏览器访问 web 应用程序和 web 应用程序可能会透明地重定向 web 浏览器进行身份验证的请求通过 AAD。 验明身份后，请求可以被传递回 web 应用程序与相应的标识信息。

[![0]][0]

- **Azure Active Directory 租户**。 这是由您的组织创建的 AAD 的实例。 它充当云应用程序的简单的目录服务 (它保存对象从本地复制 AD)，并提供身份服务。

- **Web 层子网**。 此子网包含实现自定义基于云的应用程序开发由您的组织以及其中 AAD 可以充当身份代理的虚拟机。

- **内部 AD DS 服务器**。 您的组织可能已具有现有目录服务如 AD DS。 您可以在您的 AD DS 目录 （例如，用户和组信息） AAD 启用 AAD 使用此信息来验证身份信息同步许多项。

- **Azure AD 连接同步服务器**。 这是内部计算机运行 Azure AD 连接同步服务。 您可以通过使用 Azure AD 连接软件安装此服务。 Azure AD 连接同步服务同步信息 （用户、 组、 联系人等） 保存在内部部署到云中的 AAD 的广告。 例如，可以设置或取消设置的组和用户内部，这些更改将传播到 AAD。 Azure AD 连接同步服务将信息传递给 AAD 租户。

    >[AZURE.NOTE] 出于安全考虑，用户的密码未存储在 AAD 直接。 而 AAD 包含哈希值每个密码。 这就足够来验证用户的密码。 如果用户需要重新设置密码，必须执行的内部部署和发给 AAD 的新哈希值。 AAD 高级版本包括功能，可以自动执行此任务，以便用户能够重置自己的密码。

## <a name="recommendations"></a>建议

本节概述了实施 AAD 如下建议。

- AD 连接
- 安全

### <a name="azure-ad-connect-sync-service-recommendations"></a>Azure AD 连接同步服务建议

同步关心确保云中的用户标识信息与保留在内部保持一致。 Azure AD 连接同步服务的目的是保持这种一致性。 以下几点总结实施 Azure AD 连接同步服务的建议︰

- 在实施之前 Azure AD 连接同步，应确定您的组织的同步要求 (要同步的域，从内容和频率。 文章[确定目录同步要求][aad-sync-requirements]描述了应考虑的点。

- 您可以运行使用 VM 的 Azure AD 连接同步服务或计算机承载内部。 这取决于您的广告目录中的信息的变动性，一旦已执行初始同步与 AAD Azure AD 连接同步服务上的负载不可能是高。 使用虚拟机使您能够更轻松地扩展服务器 （监视器[监视注意事项](#monitoring-considerations)一节中所述的活动以确定这样做是否必要）。

- 如果目录林中有多个内部域，您可以存储和同步到一个单一的 AAD 租户整个目录林的信息 （这是推荐的方法）。 为标识，以便每个标识只出现一次 AAD 而不是因为这会导致不一致的情况，同步数据时进行重复出现在多个域中的信息进行过滤。 这种方法需要实现多个 Azure AD 连接同步服务器。 有关详细信息，请参阅[拓扑注意事项](#topology-considerations)一节中的多个 AAD 方案。 

- 使用筛选可以限制为只存储在 AAD 这是必需的数据。 例如，您的组织可能不希望将处于非活动状态或非个人帐户信息存储在 AAD。 筛选可基于组的、 基于域的、 基于 OU 或基于属性，您可以将组合的筛选器，以生成更复杂的规则。 例如，您可以选择要同步仅对象保存在一个域中选定的属性具有特定值。 有关详细信息，请参阅[Azure AD 连接同步︰ 配置筛选][aad-filtering]。

- 若要实现 AD 连接同步服务的高可用性，辅助暂存服务器来运行。 有关详细信息，请参阅[拓扑注意事项](#topology-considerations)部分。

### <a name="security-recommendations"></a>安全建议

以下各项总结实施 AAD，具体取决于您的组织要求的主要安全建议︰

- 管理用户的密码。

    如果您使用的 AAD 的高级版本，您可以通过执行 Azure AD 连接的自定义安装来启用密码写回 AAD 从您的内部目录︰

    [![9]][9]

    此功能使用户能够从 Azure 的门户网站，在他们自己的密码重置，但只能查看您组织的安全策略的密码后启用。 例如，您可以限制哪些用户可以更改自己的密码，还可以定制密码管理经验。 有关详细信息，请参阅[自定义密码管理以满足组织的需要][aad-password-management]。

- 维护内部部署的应用程序可以从外部访问的保护。

    使用 Azure 的广告应用程序代理从外部用户通过 AAD 到内部部署 web 应用程序提供受控制的访问。 这种方法不使它们暴露于互联网的直接保护您的应用程序。 只有 Azure 目录中使用有效的凭据的用户都能访问您的应用程序。 有关详细信息，请参阅文章[Azure 门户中启用应用程序代理][aad-application-proxy]。

- 主动监视 AAD 的可疑活动。

    请考虑使用 AAD 特优 P2 版，其中包括 AAD 身份保护。 身份保护使用自适应的机器学习算法和启发式技术来检测异常和风险可能表明身份已遭到破坏的事件。 例如，它可以检测到潜在反常活动如不规则登录活动，登录从未知的来源或可疑活动，与 IP 地址或符号接来自设备的可能被感染。 使用此数据，身份保护生成报告和警报，您可以调查这些风险事件，并采取适当的补救或缓解措施。 有关详细信息，请参阅[Azure 活动目录标识保护][aad-identity-protection]。

    在 Azure 门户 AAD 的报告功能还可用于监视可疑和其他与安全相关的活动发生在您的系统中。 AAD 可以生成一系列的摘要报告︰

    [![17]][17]

    有关使用这些报表的详细信息，请参阅[Azure 活动目录报告指南][aad-reporting-guide]。

## <a name="topology-considerations"></a>拓扑注意事项

如果您正在使用 AAD 集成内部目录，配置 Azure AD 连接来实现的拓扑结构中最符合您组织的需求。 Azure AD 连接支持的拓扑结构如下所示︰

- **单个目录林、 单个 AAD 目录**。 在此拓扑中，Azure AD 连接用于同步的对象和标识信息在一个单一的一个或多个域中部署具有单一的 AAD 租户的目录林。 这是实现快速安装的 Azure AD 连接的默认拓扑。

    [![1]][1]

    请不要创建多个 Azure AD 连接同步服务器部署在同一目录林中不同的域连接到相同的 AAD 租户，除非在调试模式下运行的 Azure AD 连接同步服务器 （请参阅下面的暂存服务器）。

- **多个目录林，AAD 的单个目录**中。 如果您有多个本地目录林，请使用此拓扑。 您可以整合标识信息，以便每个唯一用户都出现一次 AAD 目录中，即使同一用户同时存在于多个林。 所有目录林使用相同的 Azure AD 连接同步服务器。 Azure AD 连接同步服务器没有任何域的一部分，但必须从所有目录林可以访问它︰

    [![2]][2]

    在此拓扑中，不使用单独的 Azure AD 连接同步服务器连接到单个的 AAD 租户的每个内部部署目录林。 如果用户有多个林，则会导致 AAD 中重复的标识信息。

- **多个目录林、 单独的拓扑**。 这种方法使您能够从单独的目录林中的标识信息合并到一个单一的 AAD 租户。 如果要组合来自不同组织的目录林 （在合并或收购，例如），并且每个用户的身份信息都保存在一个目录林，则此策略非常有用︰

    每个目录林中 Gal 同步的如果一个目录林中的用户可能在另一个作为联系人存在。 如果，例如，您的组织实现了 GALSync 与前沿标识管理器 2010年或 Microsoft 标识管理器 2016年，这会发生。 在此方案中，您可以指定用户应由他们*的邮件*属性。 此外可以匹配使用的*ObjectSID*和*msExchMasterAccountSID*属性的标识。 这很有用，如果您有一个或多个资源目录林使用已禁用的帐户。

- **临时服务器**。 在此配置中，您运行 Azure AD 连接同步服务器的第二个实例与第一个并行。 此结构支持的方案，如︰

    - 高可用性。

    - 测试和部署新的 Azure AD 连接同步服务器的配置。

    - 引入了一个新的服务器并停止使用旧的配置。 

    在这些情况下，第二个实例运行在*调试模式*。 服务器在其数据库中，记录导入的对象和同步数据，但不会将数据传递到 AAD。 仅当禁用临时模式没有服务器开始 AAD，而且还开始执行密码写-回入内部部署的目录在适当的位置写入数据︰

    [![4]][4]

    有关详细信息，请参阅[Azure AD 连接同步︰ 操作任务和考虑][aad-connect-sync-operational-tasks]。

- **多个 AAD 目录**。 建议您创建单个组织，AAD 目录，但可能存在其中的分区信息需要跨不同的 AAD 目录的情况。 在这种情况下，您应该确保内部目录林中的每个对象显示仅在一个 AAD 目录以避免同步和密码回写问题。

    若要实现这种情况下，配置单独的 Azure AD 连接同步服务器的每个 AAD 的目录，并使用筛选以便对一组互相排斥的对象进行操作的 Azure AD 连接的每个同步服务器︰ 

    [![5]][5]

有关这些拓扑的详细信息，请参阅[Azure AD 连接的拓扑][aad-topologies]。

## <a name="user-sign-in-considerations"></a>用户登录的注意事项

默认情况下，AAD 服务假定用户通过提供相同的密码，它们使用内部部署和 Azure AD 连接同步服务器配置之间的内部域和 AAD 的密码同步日志。 对于许多公司来说，这是合适的但应考虑您的组织的现有策略和基础结构。 例如︰

- 您的组织的安全策略可能会禁止同步到云的密码哈希值。

- 从域访问云资源在企业网络上加入机器时，可能需要用户遇到无缝的 SSO，（没有其他密码提示）。

- 您的组织可能已部署 ADFS 或第三方联合提供。 您可以配置 AAD 使用此基础结构实现身份验证和 SSO，而不是通过使用保存在云环境中的密码信息。

有关详细信息，请参阅[Azure AD 连接用户登录选项][aad-user-sign-in]。

## <a name="azure-ad-application-proxy-considerations"></a>Azure 的广告应用程序代理考虑事项

使用 Azure AD 提供对内部应用程序的访问。

- 公开的内部部署 web 应用程序使用连接器管理 Azure AD 应用程序代理组件的应用程序代理。 应用程序代理服务器连接器打开到 Azure AD 应用程序代理服务器的出站网络连接。 远程用户请求将从 AAD 通过此连接路由到 web 应用程序。 这种机制不再需要减小攻击面由您的组织到内部防火墙中打开入站的端口。

有关详细信息，请参阅[使用 Azure 的广告应用程序代理的发布应用程序][aad-application-proxy]。

## <a name="object-synchronization-considerations"></a>对象同步考虑事项

Azure AD 连接的默认配置同步对象从本地广告目录根据文章中指定的规则集的[Azure AD 连接同步︰ 了解默认配置][aad-connect-sync-default-rules]。 满足这些规则进行同步，忽略其他的唯一对象。 例如，用户对象必须具有一个唯一的*sourceAnchor*属性，必须填充的*accounEnabled*属性。 不会同步用户对象不具有*sAMAccountName*属性或*AAD_*或*MSOL_*与文本的开始。 Azure AD 连接应用于用户、 联系人、 组、 ForeignSecurityPrincipal 和计算机对象的多个规则。 如果您需要修改规则的默认设置，使用同步规则编辑器使用 Azure AD 连接安装 (中还记录了[Azure AD 连接同步︰ 了解默认配置][aad-connect-sync-default-rules])。

您可以定义您自己的筛选器以限制要同步的域或组织单位的对象。 实现更为复杂的自定义筛选功能，如中所述或者[Azure AD 连接同步︰ 配置筛选][aad-filtering]。

## <a name="security-considerations"></a>安全注意事项

使用条件访问控制来拒绝来自意外源的身份验证请求︰

- 触发[Azure 多因素身份验证 (MFA)] [azure-multifactor-authentication]用户尝试从非受信任位置连接 (如通过 Internet) 而不是受信任的网络。

- 设备平台类型的用户 (iOS，Android，Windows Mobile Windows) 用于确定应用程序和功能的访问策略。

- 记录用户设备的启用/禁用状态，并将此信息合并到访问策略检查。 例如，如果用户的手机丢失或被盗应将它记录为已禁用以防止它被用来访问。

- 控制对基于组成员身份的用户的访问级别。 使用[AAD 动态成员身份规则][ aad-dynamic-membership-rules] ，从而简化管理组。 这是如何工作的概述，请参阅[动态组的成员身份简介][aad-dynamic-memberships]。

- 使用与 AAD 身份保护条件访问风险策略提供基于特殊符号中的活动或其他事件的高级的保护。

有关详细信息，请参阅[Azure Active Directory 条件访问][aad-conditional-access]。

## <a name="scalability-considerations"></a>可伸缩性注意事项

可伸缩性被通过 AAD 服务和 Azure AD 连接同步服务器的配置︰

- 对于 AAD 服务，不需要配置任何选项，以实现可扩展性。 AAD 服务支持基于复制副本的可扩展性。 AAD 实现单个主副本，哪句柄写入操作，以及多个只读的次要副本。 AAD 透明地重定向尝试的写入对辅助副本的主副本。 AAD 提供最终一致性。 对主副本所做的所有更改将都传播到辅助副本。 如针对 AAD 的大多数操作是读取，而不是写入，此体系结构中很好地缩放。

    有关详细信息，请参阅[Azure 广告︰ 我们地理冗余、 高可用性、 分布式云目录决窍][aad-scalability]。

- 对于 Azure AD 连接同步服务器，您应该确定您很可能会同步您的本地目录中的对象的数量。 如果您有少于 100000 个对象，可以使用附带 Azure AD 连接的默认 SQL Server Express LocalDB 软件。 如果您有更多的对象，应安装 SQL Server 的生产版本和执行自定义安装，则应使用现有的 SQL Server 实例 Azure AD 连接指定。

## <a name="availability-considerations"></a>可用性考虑事项

与可伸缩性问题，可用性跨越 AAD 服务和 Azure AD 连接的配置︰

- AAD 服务旨在提供高可用性。 没有用户可配置可用性的选项。 它就是地理分布和运行在多个数据中心具有自动故障切换功能，在世界的传播。 如果数据中心不可用，AAD 确保目录数据可用于实例访问中至少两个更多的地区组织分散的数据中心。

    >[AZURE.NOTE] SLA AAD 基本和特优服务保证至少 99.9%的可用性。 免费的 AAD 层没有 SLA。 有关详细信息，请参阅[Azure Active Directory 的 SLA][sla-aad]。

- 提高可用性的 Azure AD 连接同步服务器可以运行第二个实例在调试模式下，，[拓扑注意事项](#topology-considerations)一节中所述。 

    此外，如果您没有使用附带了 Azure AD 连接的 SQL Server Express LocalDB 实例，则应为 SQL Server 考虑高可用性。 请注意，只有高可用性解决方案支持 SQL 群集。 Azure AD 连接不支持诸如镜像和始终打开的解决方案。

    有关维护的 Azure AD 连接同步服务器，以及如何在出现故障后恢复的可用性的其他事项，请参阅[Azure AD 连接同步︰ 操作任务和考虑事项--灾难恢复][aad-sync-disaster-recovery]。

## <a name="management-considerations"></a>管理注意事项

有管理 AAD 的两个方面︰

1. 管理云中的 AAD。

2. 维护的 Azure AD 连接同步服务器。

AAD 提供了用于管理域和目录在云中的以下选项︰

- [Azure 的 Active Directory PowerShell 模块][aad-powershell]。 如果您需要脚本常见 Azure AD 管理任务如用户管理、 域管理和配置单一登录，请使用本模块。

- 在 Azure 门户 azure AD 管理刀片。 此刀片提供交互式的管理视图的目录中，并使您能够控制和配置 AAD 的大多数方面。

    [![10]][10]

Azure AD 连接安装以下工具，用于维护计算机内部的 Azure AD 连接同步服务︰

- Microsoft Azure 活动目录连接控制台。 该工具使您能够修改 Azure AD 同步服务器的配置、 自定义同步的方式、 启用或禁用转移模式，并将用户登录模式切换 （您可以启用 AD FS 的登录使用内部部署基础结构）。

- 同步服务管理器。 在此工具中使用*操作*选项卡管理同步过程和检测过程的任何部分是否已失败。 可以触发此工具使用手动的同步。 

    [![12]][12]

    *连接器*选项卡使您能够控制域的连接 (内部和在云中) 同步引擎附加到︰

    [![13]][13]

-  同步规则编辑器。 使用此工具自定义它们的内部目录和云中的 AAD 之间复制时，对象被转换的方式。 此工具使您可以指定附加的属性和对象进行同步，并筛选器来确定哪些实例应该或不应该同步。

    有关详细信息，请参阅文档中的同步规则编辑器部分[Azure AD 连接同步︰ 了解默认配置][aad-connect-sync-default-rules]。

页面[Azure AD 连接同步︰ 最佳做法更改默认配置][aad-sync-best-practices]包含管理 Azure AD 连接的其他信息和提示。

## <a name="monitoring-considerations"></a>监视的注意事项

运行状况监视被通过使用一系列的部署代理安装︰

- Azure AD 连接安装一个代理程序用于捕获有关同步信息。 在 Azure 门户使用 Azure 活动目录连接健康刀片式服务器监视运行状况和性能的 Azure AD 连接。 有关详细信息，请参阅[使用 Azure AD 同步连接健康][aad-health]。

- 监视的 AD DS 域和目录从 Azure 的健康状况，请在内部域中的计算机上安装 AD DS 代理 Azure AD 连接运行状况。 在 Azure 门户到监视器 AD DS 中使用 Azure 活动目录连接健康刀片式服务器。 有关详细信息，请参阅[使用 Azure 使用 AD DS 的 AD 连接健康][aad-health-adds]

- 安装的 AD FS 代理监视在内部部署中运行的 AD FS 的健康状况并在 Azure 门户使用 Azure 活动目录连接健康刀片式服务器监视 AD DS Azure AD 连接运行状况。 有关详细信息，请参阅[使用 Azure 使用 AD FS 的 AD 连接健康][aad-health-adfs]

安装 AD 连接健康代理和其要求的详细信息，请参阅[Azure 的 AD 连接健康代理安装][aad-agent-installation]。

## <a name="sample-solution"></a>示例解决方案

本节介绍用于构建的示例解决方案，您可以使用测试配置的 AAD 的步骤。 该解决方案包括以下元素︰

- N 层 web 应用程序中，以下所描述[的 N 层体系结构在 Azure 上运行虚拟机]的结构[implementing-a-multi-tier-architecture-on-Azure]。

- 测试客户端计算机。 我们建议为这台计算机使用另一个虚拟机。 只要您具备管理员权限，并且可以连接到 n 层 web 应用程序并不重要，此虚拟机的配置。

- 包含其自己的域使用 AD DS 构建一个模拟的内部部署环境。

以下步骤说明的情况是︰

- N 层 web 应用程序在云中运行的外部用户，与 AAD 提供密码身份验证启用访问。

- N 层 web 应用程序在云中运行到组织内运行与 AAD 提供密码身份验证的用户访问启用。

### <a name="prerequisites"></a>系统必备组件

请按照这些步骤假设以下先决条件︰

- 具有现有 Azure 订阅，您可以在其中创建资源组。

- 您已安装了[Azure 命令行界面][azure-cli]。

- 您已经下载并安装最新版本。 请参阅[此处][azure-powershell-download]的说明。

- 已安装一份[makecert] [makecert]实用程序测试客户端计算机上。

- 您有权访问已安装的 Visual Studio 开发计算机。

### <a name="create-the-n-tier-web-application-architecture"></a>创建 n 层 web 应用程序体系结构

1. 创建一个名为`Scripts`，其中包含一个子文件夹命名为`Parameters`。

2. 下载[部署 ReferenceArchitecture.ps1] [ solution-script] PowerShell 脚本保存到脚本文件夹。

3. 在参数文件夹中，创建另一个名为窗口的子文件夹。

4. 下载到参数/Windows 文件夹中的以下文件︰

    - [virtualNetwork.parameters.json][vnet-parameters-windows]

    - [networkSecurityGroup.parameters.json][nsg-parameters-windows]

    - [webTierParameters.json][webtier-parameters-windows]

    - [businessTierParameters.json][businesstier-parameters-windows]

    - [dataTierParameters.json][datatier-parameters-windows]

    - [managementTierParameters.json][managementtier-parameters-windows]

5. 编辑**部署 ReferenceArchitecture.ps**1 文件在脚本文件夹中，然后更改以下行来指定应创建或用于存储虚拟机和由脚本创建的资源的资源组︰

        ```powershell
        # PowerShell
        $resourceGroupName = "ra-aad-ntier-rg"
        ```

6. 编辑**窗口中的**参数/s 文件夹中的下列文件并设置`resourceGroup`中的值`virtualNetworkSettings`中每个文件是相同的部分**部署 ReferenceArchitecture.ps1**脚本文件中指定的。

    - networkSecurityGroup.parameters.json

    - webTierParameters.json

    - businessTierParameters.json

    - dataTierParameters.json

    - managementTierParameters.json

7. 打开 Azure PowerShell 窗口，将移动到脚本文件夹中，然后运行以下命令︰

        ```powershell
        .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Windows ntier
        ```

    更换**<subscription id>**Azure 的订阅 id

    为**<location>**，请指定 Azure 的地区，例如*eastus*或*westus*。

8. 当脚本完成时，使用 Azure 门户网站获取 web 层负载平衡器 (*ra aad 的 n 层的 web-磅*) 的公用 IP 地址︰

    [![18]][18]

9. 登录到测试客户端计算机 （或虚拟机），然后验证可以通过使用 Internet Explorer 浏览到 web 层负载平衡器的公共 IP 地址访问 web 层。 应显示的默认 IIS 页。

### <a name="simulate-configuration-of-a-public-web-site"></a>模拟一个公共 web 站点的配置

>[AZURE.NOTE] 下面的步骤来模拟 web 层关联 DNS 名称 www.contoso.com 通过修改在测试客户端计算机上的主机文件。 此外，自签名的证书和虚设承载机构配置 web 层的虚拟机。 这是用于测试目的只和**您不应使用这些技术，在生产环境中**。

1. 在测试客户端计算机上，使用记事本编辑文件**C:\Windows\System32\drivers\etc\hosts**并添加一个将名称 www.contoso.com 与 web 层负载平衡器的公用 IP 地址相关联的项︰

        ```text
        # Copyright (c) 1993-2009 Microsoft Corp.
        #
        # This is a sample HOSTS file used by Microsoft TCP/IP for Windows.
        #
        # This file contains the mappings of IP addresses to host names. Each
        # entry should be kept on an individual line. The IP address should
        # be placed in the first column followed by the corresponding host name.
        # The IP address and the host name should be separated by at least one
        # space.
        #
        # Additionally, comments (such as these) may be inserted on individual
        # lines or following the machine name denoted by a '#' symbol.
        #
        # For example:
        #
        #      102.54.94.97     rhino.acme.com          # source server
        #       38.25.63.10     x.acme.com              # x client host
        
        # localhost name resolution is handled within DNS itself.
        #   127.0.0.1       localhost
        #   ::1             localhost
        
        52.165.38.64    www.contoso.com
        ```

2. 验证，您现在可以浏览到 www.contoso.com 在测试客户端计算机中。 应该像以前那样出现相同的默认 IIS 页。

3. 在测试客户端计算机上，打开命令提示符以管理员的身份，并使用 makecert 实用程序为 c
4. 创建虚假的根证书颁发机构证书︰

        ```
        makecert -sky exchange -pe -a sha256 -n "CN=MyFakeRootCertificateAuthority" -r -sv MyFakeRootCertificateAuthority.pvk MyFakeRootCertificateAuthority.cer -len 2048
        ```

    验证将创建下列文件︰

    - MyFakeRootCertificateAuthority.cer

    - MyFakeRootCertificateAuthority.pvk

4. 运行以下命令以安装测试的根证书颁发机构︰

        ```
        certutil.exe -addstore Root MyFakeRootCertificateAuthority.cer
        ```

5. 使用测试证书颁发机构生成 www.contoso.com 的证书︰

        ```
        makecert -sk pkey -iv MyFakeRootCertificateAuthority.pvk -a sha256 -n "CN=www.contoso.com" -ic MyFakeRootCertificateAuthority.cer -sr localmachine -ss my -sky exchange -pe
        ```

6. 运行**mmc**命令，并为本地计算机的计算机帐户添加证书管理单元。

7. 在*/Certificates （本地计算机）/个人/证书/*存储，使用其私钥对名为 www.contoso.com.pfx 的文件中导出 www.contoso.com 证书︰

    [![20]][20]

8. 返回到 Azure 的门户，连接到 VM (*ra 的 aad 的 n 层的管理-vm1*) 的管理层。 默认的用户名是用密码*testuser* *AweS0me@PW*:

    [![21]][21]
    
9. 从管理层的虚拟机中，连接到每个 web 层的虚拟机又与*testuser*的用户名和密码*AweS0me@PW*，并执行以下任务。 请注意，虚拟机 10.0.1.4、 10.0.1.5 和 10.0.1.6 的专用地址 IP:

    >[AZURE.NOTE] Web 层的虚拟机仅具有专用 IP 地址。 可以通过 VM 的管理层只能连接到它们。

    1. 从测试的客户端计算机复制文件*www.contoso.com.pfx*和*MyFakeRootCertificateAuthority.cer* 。
    
    2. 打开命令提示符以管理员的身份、 移至文件夹包含的文件的复制，并运行下面的命令︰
    
        ```
        certutil.exe -privatekey -importPFX my www.contoso.com.pfx NoExport

        certutil.exe -addstore Root MyFakeRootCertificateAuthority.cer
        ```

    3. 运行`mmc`命令，为本地计算机的计算机帐户添加证书管理单元，并验证已安装了下列证书︰

        - \Certificates （本地计算机） \Personal\Certificates\ www.contoso.com

        - \Certificates （本地计算机） \Trusted 根证书 Authorities\Certificates\MyFakeRootCertificateAuthority

    4. 启动 Internet Information Services (IIS) 管理器控制台并在计算机上浏览到*Sites\Default 网站*。

    5. 在*操作*窗格中单击*绑定*，并添加使用 www.contoso.com SSL 证书的 https 绑定︰

        [![22]][22]

10. 返回到测试客户端计算机，并验证您现在可以浏览到 https://www.contoso.com。

### <a name="create-an-azure-active-directory-tenant"></a>创建 Azure Active Directory 租户

1. 使用 Azure 的门户，创建如*myaadname*Azure Active Directory 租户。 onmicrosoft.com，其中*myaadname*是由您选择的目录名称。

2. 添加命名与目录的 GlobalAdmin 角色*管理*的用户。 记下新生成的密码。

3. 使用 Internet Explorer，浏览到 https://account.activedirectory.windowsazure.com/，以登录admin@ *myaadname*。 onmicrosoft.com。 更改您的密码提示时。

### <a name="create-and-deploy-a-test-web-application"></a>创建和测试 web 应用程序部署

1. 在开发计算机上，使用 Visual Studio 创建名为 ContosoWebApp1 的 ASP.NET Web 应用程序 （使用.NET Framework 4.5.2）︰

    [![23]][23]

2. 选择的*MVC*模板，将身份验证更改为*工作和学校的科目*，并指定 AAD 租户的名称。 不要在云中创建应用程序服务。

    [![24]][24]

3. 生成并运行应用程序以测试验证。 在登录页中输入帐户名称admin@ *myaadname*。 onmicrosoft.com，提供密码，然后单击*登录*︰

    [![25]][25]

4. 验证，AAD 要求您登录并阅读您的配置文件的权限，然后启动以管理员的身份运行的 web 应用程序

5. 关闭 Internet Explorer 并返回到 Visual Studio。

6. 打开 web.config 文件中，然后在`<appSettings>`部分中，更改到*ida: PostLogoutRedirectUri*键的值*https://www.contoso.com:443 /*。 保存该文件。

7. 在*解决方案资源管理器*窗口中，右击 ContosoWebApp1 项目，单击*发布*。

8. 在*发布网站*窗口中，单击*自定义*。 创建一个称为*ContosoWebApp1*的自定义配置文件。

9. 在*连接*页中，为*文件系统*设置的*发布方法*，指定一个文件夹命名为*ContosoWebApp*，位于开发计算机上的方便位置。

10. 在*设置*页上将*配置*设置到*发行版*。

11. 在*预览*页面上，单击*发布*。

12. 连接到每个反过来 （通过虚拟机管理层） 的 web 服务器并执行下列任务︰

    1. 将*ContosoWebApp*文件夹和其内容从开发计算机复制到*C:\inetpub*文件夹。

    2. 使用 Internet Information Services (IIS) 管理器控制台，定位到*Sites\Default 网站*的计算机上。

    3. 在*操作*窗格中单击*进行设置*，并将该 web 站点的物理路径更改为*%SystemDrive%\inetpub\ContosoWebApp*:

        [![26]][26]

### <a name="publish-the-test-web-application-through-aad"></a>发布测试 web 应用程序中的通过 AAD

1. 登录到 Azure 的门户和导航到您 AAD 的目录。

2. 在*应用程序*选项卡上单击 ContosoWebApp1 应用程序。

3. 验证您的应用程序已成功添加到目录中，然后单击*配置*选项卡。

4. *号上的 URL*更改为 https://www.contoso.com:443，并将*答复 URL*设置为 https://www.contoso.com:443 (相同的 URL)。

5. 保存配置。

6. 在测试客户端计算机上，浏览到 https://www.contoso.com。 验证，AAD 会提示您输入您的凭据，然后再登录。

>[AZURE.NOTE] 这是该终结点的第一个方案︰ 实现 n 层 web 应用程序在云中运行的外部用户，AAD 提供密码身份验证的访问。

### <a name="create-a-simulated-on-premises-environment-with-active-directory"></a>使用活动目录创建模拟的内部部署环境

内部环境包含两个域控制器的`contoso.com`域和用于承载 Azure 的 AD 连接的两个服务器的同步服务。 用于承载 Azure AD 连接的服务器不是域加入的。

1. 文件资源管理器中，在返回到脚本文件夹包含用于创建 N 层 web 应用程序的脚本。

2. 在参数文件夹中，将添加另一个子文件夹命名为`Onpremise`。

3. 下载到参数/Onpremise 文件夹中的以下文件︰

    - [添加-将添加的域-controller.parameters.json][add-adds-domain-controller-parameters]

    - [创建-添加-林-extension.parameters.json][create-adds-forest-extension-parameters]

    - [virtualMachines adc.parameters.json][virtualMachines-adc-parameters]

    - [virtualMachines-adc-joindomain.parameters.json][virtualMachines-adc-joindomain-parameters]

    - [virtualMachines adds.parameters.json][virtualMachines-adds-parameters]

    - [virtualNetwork.parameters.json][virtualNetwork-parameters]

    - [添加 virtualNetwork-dns.parameters.json][virtualNetwork-adds-dns-parameters]

5. 编辑脚本文件夹中的部署 ReferenceArchitecture.ps1 文件，并更改以下行来指定应创建或用于存储虚拟机和由脚本创建的资源的资源组︰

        ```powershell
        # PowerShell
        $resourceGroupName = "ra-aad-onpremise-rg"
        ```

6. 可编辑的参数/Onpremise 文件夹中的下列文件和设置`resourceGroup`中的值`virtualNetworkSettings`中每个文件是相同的部分部署 ReferenceArchitecture.ps1 脚本文件中指定。

    - virtualMachines adds.parameters.json

    - virtualMachines adc.parameters.json

    - virtualNetwork.parameters.json

    - 添加 virtualNetwork-dns.parameters.json

7. 打开 Azure PowerShell 窗口，将移动到脚本文件夹中，然后运行以下命令︰

        ```powershell
        .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Windows onpremise
        ```

    更换`<subscription id>`Azure 的订阅 id

    为`<location>`，指定一个 Azure 的区域，如`eastus`或`westus`。

### <a name="install-and-configure-the-azure-ad-connect-sync-service"></a>安装和配置 Azure AD 连接同步服务

在这些步骤中所示的配置包含的 Azure AD 连接同步服务的两个实例。 而在临时模式下，如果第一个服务器出现故障时提供快速的故障转移和高可用性配置第二个，第一个是活动的。

1. 使用 Azure 的门户网站，导航到保存虚拟机的 Azure AD 连接同步服务 (*ra aad onpremise rg*默认情况下)，该资源组并对*ra 的 aad-onpremise-adc-vm1*虚拟机连接。 作为*testuser*用密码登录*AweS0me@PW*。

2. 从[此处]下载 Azure AD 连接[aad-connect-download]。

3. 运行 Azure AD 连接安装程序并执行安装时使用*自定义*选项。

    >[AZURE.NOTE] 因为主持 Azure AD 连接同步服务虚拟机未加入域的不能使用*快速设置*选项。

4. 在*安装必需的组件*页上为空可选配置设置以接受默认选项。

5. 在*用户登录*页中，选择*密码同步*。

6. 在*连接到 Azure 的广告*页上，输入admin@ *myaadname*。 onmicrosoft.com，其中*myaadname*是 AAD 租户的名称。 输入管理员帐户的密码。

7. 在*连接目录*页上，指定 contoso.com 林 （类型中的值，因为它不出现在下拉列表中，） CONTOSO\testuser 输入用户名称，指定AweS0me@PW输入密码，然后单击*添加目录*。

8. 在*Azure 广告登录配置*页上，接受用户主要名称的默认值。 请*继续，而无需任何经过验证的域*。

    >[AZURE.NOTE] Contoso.com 目录列为*未验证*。 要验证的域名，域名注册商必须创建 TXT 记录。 在此示例中，不从外部注册内部域。 有关详细信息，请参阅[添加自定义域名到 Azure Active Directory][aad-custom-directory]。

9. 在*域和 OU 筛选*页上，选择*同步所有域和 Ou* （默认）。

10. *唯一地标识您的用户*在页上，接受默认值。

11. 在*用户和设备筛选器*页中，选择*同步所有用户和设备*（默认）。

12. 在*可选功能*页面上选择*密码写回*。

13. 在*开始配置*页上，选择*启动同步进程配置完成后*，但保留取消选择*启用临时模式*。

14. 验证配置过程完成没有任何错误，然后退出安装程序。

15. 从 Azure 的门户，连接到*ra 的 aad-onpremise-adc-vm2*虚拟机。 作为*testuser*用密码登录*AweS0me@PW*。

16. 下载 Azure AD 连接，然后再运行安装程序。

17. 按步骤完成向导，并响应第 3 至第 12 步中所述。

18. 在*开始配置*页上，选择*启动同步进程配置完成时*，**还选择***启用临时模式*。 这会导致 Azure AD 连接同步服务 contoso.com 域中检索有关帐户和对象的详细信息并将其存储在数据库中，但是它不会传输到 AAD 租户这些详细信息。

14. 验证配置过程完成没有任何错误，然后退出安装程序。

### <a name="test-the-aad-configuration"></a>测试 AAD 配置

1. 使用 Azure 的门户，切换到 AAD 目录，打开 Azure Active Directory 刀片式服务器，单击*用户和组*，然后单击显示列表中的用户和组与目录同步*所有用户*。 您应该看到以下帐户的用户︰

    - 管理(admin@ *myaadname*。 onmicrosoft.com)

    - 内部目录同步服务帐户 (Sync_ADC1_*nnnnnnnnnnnn*@*myaadname*。 onmicrosoft.com)

    - 内部目录同步服务帐户 (Sync_ADC2_*nnnnnnnnnnnn*@*myaadname*。 onmicrosoft.com)

2. 在 Azure 的门户中，导航到保存虚拟机的 AD DS 域控制器 (*ra aad onpremise rg*默认情况下)，该资源组并对*ra 的 aad-onpremise-广告-vm1*虚拟机连接。 作为*testuser*用密码登录*AweS0me@PW*。

3. 使用*Active Directory 用户和计算机*控制台中，添加新的域用户登录名命名 Diane Tibbot， dianet@contoso.com。 指定您选择的密码。 使用户成为本地管理员组的成员 （这是只有这样您可以本地用户身份登录此以后-域中的唯一计算机是域控制器）。

4. 切换到*ra 的 aad-onpremise-adc-vm1*虚拟机，打开 PowerShell 窗口，运行下面的命令︰

        ```[powershell]
        Import-Module ADSync
        Start-ADSyncSyncCycle -PolicyType Delta
        ```

    验证该命令返回*成功*。

    >[AZURE.NOTE] 此步骤是必需的因为默认情况下同步过程计划每隔 30 分钟运行。 这些命令会强制立即执行同步。

5. 返回到 Azure 的门户 Azure Active Directory 刀片式服务器切换到您 AAD 的目录，打开、 单击*用户和组*，然后单击*所有用户*。 现在，您应该看到 Diane Tibbot (dianet@ *myaadname*。 onmicrosoft.com) 出现在用户列表中。

6. 在 Azure Active Directory 刀片式服务器，*企业应用程序*，请单击，然后单击*所有应用程序*。 *ContosoWebApp1*应用程序，请单击，然后单击*用户和组*。 在工具栏中，单击*添加*。 单击*用户和组*，然后选择*Diane Tibbot*。 单击*指派*。

7. 使用 Internet Explorer，导航到网站 https://account.activedirectory.windowsazure.com。 作为登录dianet@ *myaadname*。 onmicrosoft.com 与以前指定的密码。

    >[AZURE.NOTE] 请不要单击应用程序列表中的 ContosoWebApp 图标。 AAD 尝试查找 web 应用程序在公开列出的 DNS 地址 www.contoso.com，这是不同于 web 层负载平衡器的地址。

8. 单击*配置文件*选项卡。 应显示用户 （如果您指定任何创建时） 的详细信息。

    >[AZURE.NOTE] 如果单击*更改密码*，则不允许执行此任务，因为管理员必须先启用此操作。 有关详细信息，请参阅[密码管理入门][aad-password-management]。

### <a name="enable-on-premises-users-to-run-the-application-by-using-authentication-through-aad"></a>使内部用户可以运行该应用程序使用身份验证通过 AAD

1. 返回到*ra 的 aad-onpremise-广告-vm1*域控制器虚拟机。

2. 打开*DNS 管理*控制台并添加 www.contoso.com 的新主机记录。 指定的 web 层负载平衡器的公共 IP 地址。

3. 复制测试客户端计算机 （此文件过程中所创建[的公共网站上的模拟配置](#simulate-configuration-of-a-public-web-site)文件*MyFakeRootCertificateAuthority.cer*
    
4. 打开命令提示符以管理员的身份、 移至的文件夹包含您刚才复制的文件并运行以下命令︰

        ```
        certutil.exe -addstore Root MyFakeRootCertificateAuthority.cer
        ```

5. 使用 Internet Explorer，定位到 https://www.contoso.com。 验证出现 ContosoWebApp1 web 应用程序登录页面 AAD。

6. 作为登录dianet@ *myaadname*。 onmicrosoft.com。 应用程序应该运行并正确地为您登录。

## <a name="next-steps"></a>下一步行动

- 了解用于[扩展您的内部添加域到 Azure]的最佳做法[adds-extend-domain]

- 了解[创建添加资源目录林]的最佳方法[adds-resource-forest]Azure 中

<!-- links -->
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[script]: #sample-solution-script
[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[active-directory-domain-services]: https://technet.microsoft.com/library/dd448614.aspx
[active-directory-federation-services]: https://technet.microsoft.com/windowsserver/dd448613.aspx
[azure-active-directory]: ../active-directory-domain-services/active-directory-ds-overview.md
[azure-ad-connect]: ../active-directory/active-directory-aadconnect.md
[ad-azure-guidelines]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[aad-explained]: https://youtu.be/tj_0d4tR6aM
[aad-editions]: ../active-directory/active-directory-editions.md
[guidance-adds]: ./guidance-iaas-ra-secure-vnet-ad.md
[sla-aad]: https://azure.microsoft.com/support/legal/sla/active-directory/v1_0/
[azure-multifactor-authentication]: ../multi-factor-authentication/multi-factor-authentication.md
[aad-conditional-access]: ../active-directory//active-directory-conditional-access.md
[aad-dynamic-membership-rules]: ../active-directory/active-directory-accessmanagement-groups-with-advanced-rules.md
[aad-dynamic-memberships]: https://youtu.be/Tdiz2JqCl9Q
[aad-user-sign-in]: ../active-directory/active-directory-aadconnect-user-signin.md
[aad-sync-requirements]: ../active-directory/active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md
[aad-topologies]: ../active-directory/active-directory-aadconnect-topologies.md
[aad-filtering]: ../active-directory/active-directory-aadconnectsync-configure-filtering.md
[aad-scalability]: https://blogs.technet.microsoft.com/enterprisemobility/2014/09/02/azure-ad-under-the-hood-of-our-geo-redundant-highly-available-distributed-cloud-directory/
[aad-connect-sync-default-rules]: ../active-directory/active-directory-aadconnectsync-understanding-default-configuration.md
[aad-identity-protection]: ../active-directory/active-directory-identityprotection.md
[aad-password-management]: ../active-directory/active-directory-passwords-customize.md
[aad-application-proxy]: ../active-directory/active-directory-application-proxy-enable.md
[aad-connect-sync-operational-tasks]: ../active-directory/active-directory-aadconnectsync-operations.md#staging-mode
[aad-custom-domain]: ../active-directory/active-directory-add-domain.md
[aad-powershell]: https://msdn.microsoft.com/library/azure/mt757189.aspx
[aad-sync-disaster-recovery]: ../active-directory/active-directory-aadconnectsync-operations.md#disaster-recovery
[aad-sync-best-practices]: ../active-directory/active-directory-aadconnectsync-best-practices-changing-default-configuration.md
[aad-adfs]: ../active-directory/active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs
[aad-health]: ../active-directory/active-directory-aadconnect-health-sync.md
[aad-health-adds]: ../active-directory/active-directory-aadconnect-health-adds.md
[aad-health-adfs]: ../active-directory/active-directory-aadconnect-health-adfs.md
[aad-agent-installation]: ../active-directory/active-directory-aadconnect-health-agent-install.md
[aad-reporting-guide]: ../active-directory/active-directory-reporting-guide.md
[azure-cli]: ../virtual-machines-command-line-tools.md
[azure-powershell-download]: ../powershell-install-configure.md
[solution-script]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/Deploy-ReferenceArchitecture.ps1
[vnet-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/virtualNetwork.parameters.json
[nsg-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/networkSecurityGroups.parameters.json
[webtier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/webTier.parameters.json
[businesstier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/businessTier.parameters.json
[datatier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/dataTier.parameters.json
[managementtier-parameters-windows]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/windows/managementTier.parameters.json
[makecert]: https://msdn.microsoft.com/library/windows/desktop/aa386968(v=vs.85).aspx
[add-adds-domain-controller-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/add-adds-domain-controller.parameters.json
[create-adds-forest-extension-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/create-adds-forest-extension.parameters.json
[virtualMachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualMachines-adds.parameters.json
[virtualNetwork-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualNetwork.parameters.json
[virtualNetwork-adds-dns-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualNetwork-adds-dns.parameters.json
[virtualMachines-adc-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualMachines-adc.parameters.json
[virtualMachines-adc-joindomain-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-aad/parameters/onpremise/virtualMachines-adc-joindomain.parameters.json
[aad-connect-download]: http://www.microsoft.com/download/details.aspx?id=47594
[aad-custom-directory]: ../active-directory/active-directory-add-domain.md
[aad-password-management]: ../active-directory/active-directory-passwords-getting-started.md#enable-users-to-reset-their-azure-ad-passwords
[adds-extend-domain]: ./guidance-identity-adds-extend-domain.md
[adds-resource-forest]: ./guidance-identity-adds-resource-forest.md
[0]: ./media/guidance-identity-aad/figure1.png "使用 Active Directory 的 Azure 的云标识体系结构"
[1]: ./media/guidance-identity-aad/figure2.png "单一目录林中，单一 AAD 目录拓扑"
[2]: ./media/guidance-identity-aad/figure3.png "多个林，单个 AAD 目录拓扑"
[4]: ./media/guidance-identity-aad/figure5.png "临时服务器拓扑结构"
[5]: ./media/guidance-identity-aad/figure6.png "多 AAD 目录拓扑"
[6]: ./media/guidance-identity-aad/figure7.png "选择 Azure AD 连接同步与特定的 SQL Server 实例的自定义安装"
[7]: ./media/guidance-identity-aad/figure8.png "指定用于用户登录的 SSO 方法"
[8]: ./media/guidance-identity-aad/figure9.png "指定域和 OU 筛选选项"
[9]: ./media/guidance-identity-aad/figure10.png "启用密码回写"
[10]: ./media/guidance-identity-aad/figure11.png "Azure AD 管理 blade 的门户"
[11]: ./media/guidance-identity-aad/figure12.png "Azure AD 连接控制台"
[12]: ./media/guidance-identity-aad/figure13.png "操作选项卡中同步服务管理器"
[13]: ./media/guidance-identity-aad/figure14.png "连接器选项卡中同步服务管理器"
[14]: ./media/guidance-identity-aad/figure15.png "同步规则编辑器"
[15]: ./media/guidance-identity-aad/figure16.png "在 Azure 门户显示同步健康 Azure 活动目录连接健康刀片式服务器"
[16]: ./media/guidance-identity-aad/figure17.png "在 Azure 门户显示 AD DS 健康 Azure 活动目录连接健康刀片式服务器"
[17]: ./media/guidance-identity-aad/figure18.png "在 Azure 门户中可用的安全报告"
[18]: ./media/guidance-identity-aad/figure19.png "突出显示的 web 层负载平衡器的公共 IP 地址 Azure 门户"
[19]: ./media/guidance-identity-aad/figure20.png "使用 Internet Explorer 浏览到 web 层负载平衡器的公用 IP 地址"
[20]: ./media/guidance-identity-aad/figure21.png "证书管理单元中显示的 www.contoso.com 证书"
[21]: ./media/guidance-identity-aad/figure22.png "连接到虚拟机的管理层"
[22]: ./media/guidance-identity-aad/figure23.png "创建默认的 web 站点的 HTTPS 绑定"
[23]: ./media/guidance-identity-aad/figure24.png "创建 ContosoWebApp1 web 应用程序"
[24]: ./media/guidance-identity-aad/figure25.png "ContosoWebApp1 web 应用程序的身份验证设置"
[25]: ./media/guidance-identity-aad/figure26.png "登录到 Azure AAD 从 ContosoWebApp1 web 应用程序"
[26]: ./media/guidance-identity-aad/figure27.png "更改默认的 web 站点的文件夹"