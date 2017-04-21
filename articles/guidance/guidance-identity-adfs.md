<properties
   pageTitle="在 Azure 中实施 ADFS |Microsoft Azure"
   description="如何实现在 Azure 中使用活动目录联合身份验证服务授权的安全混合网络体系结构。"
   services="guidance,vpn-gateway,expressroute,load-balancer,virtual-network,active-directory"
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
   ms.date="10/13/2016"
   ms.author="telmos"/>

# <a name="implementing-active-directory-federation-services-adfs-in-azure"></a>在 Azure 中实施 Active Directory 联合身份验证服务 (ADF)

[AZURE.INCLUDE [pnp-RA-branding](../../includes/guidance-pnp-header-include.md)]

本文介绍最佳实践实施安全混合网络，将内部网络扩展到 Azure，并使用[活动目录联合身份验证服务 (ADF)] [active-directory-federation-services]在云中运行的组件执行联合身份验证和授权。 这种体系结构扩展[到 Azure 扩展 Active Directory]所描述的结构[implementing-active-directory]。

> [AZURE.NOTE] Azure 具有两种不同的部署模型︰[资源管理器][resource-manager-overview]和经典。 此参考体系结构使用资源管理器，Microsoft 建议为新的部署。

ADFS 可以运行内部，但在混合方案中应用程序都位于 Azure 中它可以更有效地在云环境中实现此功能。 这种体系结构的典型使用情况包括︰

- 混合应用程序中的工作负载运行部分内部和部分在 Azure。

- 利用联合的授权公开向伙伴组织的 web 应用程序的解决方案。

- 支持从防火墙外的组织运行的 web 浏览器访问的系统。

- 使用户能够通过从经授权的外部设备，例如远程计算机、 笔记本电脑和其他移动设备连接来访问 web 应用程序的系统。 

ADFS 的工作原理的详细信息，请参阅[活动目录联合身份验证服务概述][active-directory-federation-services-overview]。 此外，文章[在 Azure ADFS 部署][adfs-intro]包含在 Azure 中实施 ADFS 的详细分步介绍。

## <a name="architecture-diagram"></a>体系结构关系图

下图突出显示 （*单击以放大*） 此体系结构中的重要组成部分。 有关与 ADF 无关的任何元素的详细信息，阅读[实施安全混合网络体系结构在 Azure 中的][implementing-a-secure-hybrid-network-architecture]，[实现与 Azure 中的 Internet 访问的安全的混合网络体系结构][implementing-a-secure-hybrid-network-architecture-with-internet-access]，和[实施安全混合网络体系结构与 Azure 中的 Active Directory 用户][implementing-active-directory]:

[![0]][0]

>[AZURE.NOTE] 此图描述了以下使用案例︰
>
>- 在伙伴组织中运行的应用程序代码访问 web 应用程序承载于 Azure VNet 内。
>
>- （具有内添加存储的凭据） 的外部，注册用户访问 web 应用程序承载于 Azure VNet 内。
>
>- 用户通过使用经授权的设备和运行 web 应用程序承载于 Azure VNet 内连接到您的 VNet。
>
>此外，这种体系结构侧重于被动联盟，联合服务器决定如何以及何时在哪里来对用户进行身份验证。 用户需要在应用程序开始运行时提供的登录信息。 这是最常用的 web 浏览器的机制，包括将浏览器重定向到站点，用户可以提供其凭据的协议。 ADFS 还支持动态联合，借此，应用程序将负责提供凭据而无需进一步用户交互，但范围之外的这种体系结构是这种情况。

- **添加子网。** 添加服务器包含在其自己的子网中。 NSG 的规则有助于保护添加服务器，可以提供防火墙，以阻止来自意外源的通讯。

- **添加服务器。** 这些都是为在云环境中的虚拟机运行的域控制器。 这些服务器可以提供对域中的本地标识进行身份验证。

- **ADFS 的子网。** ADFS 服务器可以充当防火墙的 NSG 规则与位于各自的子网上。

- **ADFS 服务器。** ADFS 服务器提供联合的授权和身份验证。 在此体系结构中，他们将执行以下任务︰

    - 它们可以接收包含索赔所作的伙伴联合身份验证服务器代表伙伴用户的安全令牌。 ADFS 可以验证传递到 Azure 中运行的 web 应用程序的声明前的这些标记有效。 （在 Azure) 的公司的 web 应用程序可以使用这些声明授权请求。 在这种情况下，公司的 web 应用程序被*配置成信赖方*，并且合作伙伴联合服务器颁发的责任索赔的可理解的公司的 web 应用程序。 由于他们提交代表经过身份验证的帐户伙伴组织中的访问请求，引用伙伴联合身份验证服务器为*帐户伙伴*。 ADFS 服务器称为*资源伙伴*，因为它们提供了对资源 （在此情况下，web 应用程序） 的访问。

    - 他们可以进行身份验证 (通过添加和[活动目录设备注册服务][ADDRS]) 和授权于外部用户运行的 web 浏览器或设备访问您公司的 web 应用程序所需的传入请求。 

    ADFS 服务器被配置为服务器场，通过 Azure 负载平衡器。 这种结构有助于提高可用性和可伸缩性。 此外，请注意，ADFS 服务器不直接与 Internet 公开，而是所有的互联网通信筛选通过 ADFS web 应用程序代理服务器和 DMZ。

- **ADFS 代理子网。** ADFS 代理服务器可包含在其自己的子网，NSG 规则提供保护。 此子网中的服务器提供了 Azure 虚拟网络和 Internet 之间的防火墙的网络虚拟装置一套通过互联网公开。

- **ADFS 的 web 应用程序代理 (WAP) 服务器。** 这些计算机充当 ADFS 服务器侦听传入的请求，与伙伴组织和外部设备。 WAP 服务器充当一个筛选器，直接从公共 Internet 访问的服务器 ADFS 的屏蔽层。 使用 ADFS 服务器部署 WAP 与负载平衡服务器场中的服务器提供更高可用性和可伸缩性，比部署独立服务器的集合。

    >[AZURE.NOTE] 有关安装 WAP 服务器的详细信息，请参阅[安装和配置 Web 应用程序代理服务器][install_and_configure_the_web_application_proxy_server]

- **伙伴组织。** 这是示例伙伴组织，它运行的 web 应用程序请求的访问权限在 Azure 中运行 web 应用程序。 合作伙伴组织的联合身份验证服务器对本地请求进行身份验证，并将提交包含到运行在 Azure ADFS 声明的安全令牌。 ADFS Azure 中的验证安全令牌，并且如果它们是有效的它可以传递到 Azure 中运行 web 应用程序对它们进行授权的声明。

    >[AZURE.NOTE] 您还可以配置 VPN 隧道使用 Azure 网关来提供直接访问到 ADFS 为可信赖的合作伙伴。 这些伙伴那里接收的请求不通过 WAP 服务器。

## <a name="recommendations"></a>建议

本节概述了用于实现在 Azure，ADFS 的建议包括︰

- 虚拟机的建议。

- 网络的建议。

- 可用性的建议。

- 安全建议。

- ADFS 安装建议。

- ADFS 信任的建议。

### <a name="vm-recommendations"></a>VM 的建议

使用足够的资源来处理流量的预期的卷创建虚拟机。 使用现有机承载内部部署 ADFS 作为起始点的大小。 监视的资源利用率。 您可以调整 Vm 并缩小它们是否太大。

按照[运行 Windows Azure 上 VM]中列出的建议[vm-recommendations]。

### <a name="networking-recommendations"></a>网络的建议

为每个虚拟机承载 ADF 和 WAP 服务器使用专用的静态 IP 地址配置的网络接口。

不要为 ADFS Vm 的公用 IP 地址。 有关详细信息，请参阅[安全性考虑][security-considerations]。

设置每个 ADF 和 WAP VM 引用添加虚拟机 （它应该运行 DNS） 的网络接口的首选和备用 DNS 服务器的 IP 地址。 此步骤是必需的以便每个虚拟机加入到域。

### <a name="availability-recommendations"></a>可用性的建议

使用至少两个服务器，以提高可用性 ADFS 服务创建 ADF 场。

为该服务器场中每个 ADF VM 使用不同的存储帐户。 这种方法有助于确保单个存储帐户失败没有使整个服务器场都无法访问。

ADFS 和 WAP 的虚拟机创建单独的 Azure 可用性设置。 确保每个组中至少两个虚拟机。 每个可用性组必须具有更新的至少两个域和两个故障域。

配置负载平衡器 ADFS Vm 和 WAP Vm，如下所示︰

- 使用 Azure 负载平衡器提供外部访问 WAP 机和内部负载平衡器在 ADFS 场的 ADF 服务器之间分配负载。

- 只能通过通讯出现在 ADFS/WAP 服务器端口 443 (HTTPS)。

- 为负载平衡器提供静态 IP 地址。

- 创建健康探测器使用 TCP 协议而非 HTTPS。 您可以 ping 端口 443，用于验证 ADF 服务器工作正常。

    >[AZURE.NOTE] ADFS 服务器使用服务器名称指示 (SNI) 协议，因此试图探测使用 HTTPS 终结点从负载平衡器无法正常工作。

- ADFS 负载平衡器为向域添加一个 DNS *A*记录。 

    指定的 IP 地址的负载平衡器，并为其指定一个名称 （如 adfs.contoso.com) 的域中。 这是依据客户端和 WAP 服务器访问 ADFS 服务器场的名称。

### <a name="security-recommendations"></a>安全建议

避免直接暴露于 Internet ADFS 服务器。 ADFS 服务器都具有完全的授权才能授予安全令牌的加入域的计算机。 ADFS 服务器受到危害，如果恶意用户可以对所有 web 应用程序和受 ADFS 的联合身份验证服务器发出完全访问令牌。 如果您的系统必须处理来自外部用户不一定来自可信赖的合作伙伴站点连接请求，使用 WAP 服务器处理这些请求。 有关详细信息，请参阅[放置联合服务器代理的位置][where-to-place-an-fs-proxy]。

地方 ADFS 服务器和 WAP 服务器分开的子网，与自己的防火墙。 NSG 规则可用于定义防火墙规则。 如果您需要更全面的保护可以实现其他安全外围服务器通过使用的子网和 NVAs，对[实现与 Azure 中的 Internet 访问的安全的混合网络体系结构]的文档所述[implementing-a-secure-hybrid-network-architecture-with-internet-access]。 请注意所有防火墙都应该都允许通信量在端口 443 (HTTPS)。

限制对 ADF 和 WAP 服务器直接登录访问。 只有 DevOps 人员应该能够进行连接。

不将 WAP 服务器加入到域。

### <a name="adfs-installation-recommendations"></a>ADFS 安装建议

[联合服务器场部署]文章[Deploying_a_federation_server_farm]提供有关安装和配置 ADFS 的详细的说明。 在服务器场中配置第一台 ADFS 服务器之前执行以下任务︰

1. 获得公开可信任的证书进行服务器身份验证。 *接受方名称*必须包含客户端访问联合身份验证服务的名称。 这可以是负载平衡器，例如， *adfs.contoso.com*的注册 DNS 名称 (避免使用通配符名称如 **。 contoso.com*，出于安全原因)。 在所有 ADFS 服务器虚拟机上使用相同的证书。 您可以购买从受信任的证书颁发机构，证书，但如果您的组织使用 Active Directory 证书服务可以创建您自己。 

    DRS 使用*者备用名称*以便能够从外部设备的访问。 这应为窗体*enterpriseregistration.contoso.com*。

    有关详细信息，请参阅[获取和配置 SSL 证书 ADFS 为][adfs_certificates]。

2. 在域控制器上，生成密钥分发服务的新的根项。 设置为当前时间减去 10 小时 （此配置降低了分发和跨域同步密钥中可能发生的延迟） 的有效时间。 此步骤是必需的以支持创建用于运行 ADFS 服务组服务帐户。 下面的 Powershell 命令演示如何执行此操作的示例︰

    ```powershell
    Add-KdsRootKey -EffectiveTime (Get-Date).AddHours(-10)
    ```

3. 每个 ADF 服务器虚拟机添加到域中。

>[AZURE.NOTE] 要安装 ADF，请运行 FSMO 角色的域的 PDC 仿真程序的域控制器必须运行并且从 ADF 虚拟机可以访问。

### <a name="adfs-trust-recommendations"></a>ADFS 信任的建议

建立 ADFS 安装和任何合作伙伴组织的联合身份验证服务器之间的联合身份验证信任。 配置任何筛选的索赔和所需的映射。 此过程要求︰

- DevOps 的员工**在每个合作伙伴组织**以添加信赖方信任 web 应用程序可通过 ADFS 服务器访问。

- DevOps 的员工**在组织中**配置声明提供程序信任启用 ADFS 服务器信任伙伴组织提供的声明。

- DevOps 的员工**在组织中**配置 ADFS 传递到您的组织的 web 应用程序的声明。

    有关详细信息，请参阅[建立联合身份验证信任][establishing-federation-trust]。

发布您的组织的 web 应用程序，并使它们可供外部合作伙伴使用预身份验证通过 WAP 服务器。 有关详细信息，请参阅[发布应用程序使用 ADFS 预身份验证][publish_applications_using_AD_FS_preauthentication]

请注意 ADF 支持令牌转换和充实。 Azure 的 Active Directory 中不提供此功能。 使用 ADFS，建立的信任关系，您可以︰

- 配置授权规则的声明转换。 例如，您可以映射组安全保护，防止非 Microsoft 合作伙伴组织成的添加可以授权您的组织中使用的表示。

- 转换从一种格式到另一索赔。 例如，您可以映射从 SAML 2.0 SAML 1.1 到如果您的应用程序只支持 SAML 1.1 索赔。 

## <a name="availability-considerations"></a>可用性考虑事项

您可以使用 SQL Server 或 Windows 内部数据库 (WID) 容纳 ADFS 配置信息。 WID 提供了基本的冗余。 更改直接写入在 ADFS 群集中，ADFS 数据库之一而其他服务器则使用拉复制来使数据库保持最新。 使用 SQL Server 可以提供完整的数据库冗余和高可用性使用故障转移群集或镜像。

## <a name="security-considerations"></a>安全注意事项

ADFS 利用 HTTPS 协议，因此，请确保包含站点的子网的 NSG 规则层的虚拟机允许 HTTPS 请求。 这些请求可能来自内部网络中，包含 web 层、 业务层、 数据层、 专用 DMZ、 公共 DMZ 和包含 ADFS 服务器的子网的子网。

请考虑使用一套网络虚拟装置来记录遍历用于审核目的的虚拟网络边缘的通信的详细的信息。

## <a name="scalability-considerations"></a>可伸缩性注意事项

以下注意事项，总结从[计划 ADFS 部署]文档[plan-your-adfs-deployment]，为调整 ADF 场的起始点︰

- 如果您有少于 1000 个用户，不要创建专用的 ADFS 服务器，而改为在每个群中添加服务器上安装 ADFS （请确保您有至少两个添加服务器维护可用性）。 创建一个 WAP 服务器。

- 如果您有用户 1000年到 15000，创建两个专用的 ADFS 服务器和两个专门的 WAP 服务器。

- 如果您有用户 15000 到 60000，创建三个到五个专用的 ADFS 服务器和至少两个专门的 WAP 服务器之间。

这些数字假设您使用的双四核虚拟机 （标准 D4_v2 或更好），以承载 Azure 中的服务器。

请注意，是否使用 Windows 内部数据库来存储 ADFS 配置数据，则被限制为八个 ADFS 服务器场中。 如果您预计需要的详细信息，然后使用 SQL Server。 有关详细信息，请参阅[ADFS 配置数据库角色][adfs-configuration-database]。

## <a name="management-considerations"></a>管理注意事项

DevOps 的员工应准备执行以下任务︰

- 管理 （管理 ADFS 场、 管理信任的联合身份验证服务器上的策略和管理联合身份验证服务所使用的证书） 的联合身份验证服务器。

- 管理 （管理管理 WAP 证书的 WAP 场） WAP 服务器。

- 管理 web 应用程序 （配置信赖方、 身份验证方法和声明映射）。

- ADFS 组件的备份。

## <a name="monitoring-considerations"></a>监视的注意事项

[活动目录联合身份验证服务 2012 r2 的 Microsoft 系统中心管理包][oms-adfs-pack]提供预防性和反应性 ADFS 的联合身份验证服务器部署的监控。 此管理包可监视︰

- ADFS 的服务在 ADFS 事件日志中的记录该事件。

- ADFS 性能计数器收集性能数据。 

- ADFS 系统和 web 应用程序 （依赖方） 的整体健康状况和提供预警的关键问题和警告。

## <a name="solution-components"></a>解决方案组件

一个示例解决方案脚本[部署 ReferenceArchitecture.ps1][solution-script]，可用于实现遵循本文中介绍的建议的体系结构。 此脚本使用 Azure 资源管理器的模板。 作为一套基本构建块，其中每个执行特定的操作，例如创建 VNet 或配置 NSG 提供了模板。 该脚本的目的是协调模板部署。

将模板保存在单独的 JSON 文件中的参数与参数化。 您可以修改配置以满足您自己的要求部署这些文件中的参数。 不需要修改模板本身。 请注意，您必须更改参数文件中的对象的架构。

编辑模板时，创建对象，请按照[推荐的 Azure 资源命名约定]中所述的命名约定[naming-conventions]。

该示例解决方案创建并配置中添加子网和服务器，ADFS 子网和，ADFS 代理子网和服务器，DMZ、 web 层、 业务层和数据访问层组件、 VPN 网关和管理层组成的云环境。 此示例解决方案还包括可选配置创建模拟的内部部署环境。

以下各节描述各个元素的内部和云配置。

### <a name="on-premises-components"></a>内部组件

>[AZURE.NOTE] 这些组件不是本文档中描述的体系结构的主要重点，并且提供了只是为了让您有机会测试云环境安全，而不是使用实际的生产环境。 出于此原因，本部分只总结了关键参数文件。 您可以修改设置，例如 IP 地址或大小的 Vm，但最好让许多其他参数不变。

此环境包含名为 contoso.com 域 AD 林中。 域包含 IP 地址 192.168.0.4 和 192.168.0.5 两个添加服务器。 这两台服务器运行 DNS 服务。 这两个虚拟机上的本地管理员帐户称为`testuser`密码`AweS0me@PW`。 此外，配置将用于连接到云环境中 VNet 的 VPN 网关设置。 您可以通过编辑以下位于[**参数/onpremise**]的 JSON 文件修改配置[on-premises-folder]文件夹︰

- ** [virtualNetwork.parameters.json][on-premises-vnet-parameters]**。 此文件定义了内部环境的网络地址空间。

- ** [virtualMachines adds.parameters.json][on-premises-virtualmachines-adds-parameters]**。 此文件包含宿主添加服务内部部署虚拟机的配置。 默认情况下，创建了两个*标准的 DS3 v2*虚拟机。

- ** [virtualNetworkGateway.parameters.json] [on-premises-virtualnetworkgateway-parameters]**和**[connection.parameters.json][on-premises-connection-parameters]**。 这些文件保存在云中，包括共享的密钥，以用于保护遍历该网关的通信到 Azure VPN 网关的 VPN 连接的设置。

其余的文件的文件夹中包含用于创建使用此基础结构的内部域的配置信息。 您可以使用它们来安装添加、 设置 DNS、 创建一个目录林，并配置目录林复制站点。

### <a name="cloud-components"></a>云的组件

这些组件构成了此体系结构的核心。 [**参数/azure**] [azure-folder]文件夹中包含以下参数文件来配置这些组件︰

- ** [virtualNetwork.parameters.json][vnet-parameters]**。 此文件定义的虚拟机和云环境中的其他组件的 VNet 结构。 它包括的设置，例如名称、 地址空间、 子网和所需的任何 DNS 服务器的地址。 请注意，此示例中所示的 DNS 地址引用的内部 DNS 服务器，以及默认的 Azure DNS 服务器的 IP 地址。 修改引用自己的 DNS 设置，如果您不使用示例内部环境这些地址︰

    ```json
    {
        "virtualNetworkSettings": {
            "value": {
                "name": "ra-adfs-vnet",
                "resourceGroup": "ra-adfs-network-rg",
                "addressPrefixes": [
                    "10.0.0.0/16"
                ],
                "subnets": [
                    {
                        "name": "dmz-private-in",
                        "addressPrefix": "10.0.0.0/27"
                    },
                    {
                        "name": "dmz-private-out",
                        "addressPrefix": "10.0.0.32/27"
                    },
                    {
                        "name": "dmz-public-in",
                        "addressPrefix": "10.0.0.64/27"
                    },
                    {
                        "name": "dmz-public-out",
                        "addressPrefix": "10.0.0.96/27"
                    },
                    {
                        "name": "mgmt",
                        "addressPrefix": "10.0.0.128/25"
                    },
                    {
                        "name": "GatewaySubnet",
                        "addressPrefix": "10.0.255.224/27"
                    },
                    {
                        "name": "web",
                        "addressPrefix": "10.0.1.0/24"
                    },
                    {
                        "name": "biz",
                        "addressPrefix": "10.0.2.0/24"
                    },
                    {
                        "name": "data",
                        "addressPrefix": "10.0.3.0/24"
                    },
                    {
                        "name": "adds",
                        "addressPrefix": "10.0.4.0/27"
                    },
                    {
                        "name": "adfs",
                        "addressPrefix": "10.0.5.0/27"
                    },
                    {
                        "name": "proxy",
                        "addressPrefix": "10.0.6.0/27"
                    }
                ],
                "dnsServers": [
                    "192.168.0.4",
                    "192.168.0.5",
                    "168.63.129.16"
                ]
            }
        }
    }
    ```

- ** [virtualMachines adds.parameters.json ] [ virtualmachines-adds-parameters] **。 此文件配置虚拟机运行在云中的添加。 配置包括两个虚拟机。 您应该更改的管理员用户名和密码在`virtualMachineSettings`节，并且您可以有选择地修改 VM 大小以匹配域的要求︰

    有关详细信息，请参阅[扩展到 Azure 的 Active Directory][extending-ad-to-azure]。

    ```json
            "virtualMachinesSettings": {
                "value": {
                    "namePrefix": "ra-adfs-ad",
                    "computerNamePrefix": "aad",
                    "size": "Standard_DS3_v2",
                    "osType": "Windows",
                    "adminUsername": "testuser",
                    "adminPassword": "AweS0me@PW",
                    "osAuthenticationType": "password",
                    "nics": [
                        {
                            "isPublic": "false",
                            "subnetName": "adds",
                            "privateIPAllocationMethod": "Static",
                            "startingIPAddress": "10.0.4.4",
                            "enableIPForwarding": false,
                            "dnsServers": [
                            ],
                            "isPrimary": "true"
                        }
                    ],
                    "imageReference": {
                        "publisher": "MicrosoftWindowsServer",
                        "offer": "WindowsServer",
                        "sku": "2012-R2-Datacenter",
                        "version": "latest"
                    },
                    "dataDisks": {
                        "count": 1,
                        "properties": {
                            "diskSizeGB": 127,
                            "caching": "None",
                            "createOption": "Empty"
                        }
                    },
                    "osDisk": {
                        "caching": "ReadWrite"
                    },
                    "extensions": [
                    ],
                    "availabilitySet": {
                        "useExistingAvailabilitySet": "No",
                        "name": "ra-adfs-as"
                    }
                }
            },
            "virtualNetworkSettings": {
                "value": {
                    "name": "ra-adfs-vnet",
                    "resourceGroup": "ra-adfs-network-rg"
                }
            },
            "buildingBlockSettings": {
                "value": {
                    "storageAccountsCount": 2,
                    "vmCount": 2,
                    "vmStartIndex": 1
                }
            }
        }
    ```

- **[中添加-添加的域-controller.parameters.json][add-adds-domain-controller-parameters]**。 此文件包含用于创建跨越添加服务器的 CONTOSO 域设置。 它使用自定义扩展，建立域并添加服务器添加到它。 除非您创建其他添加服务器 (在这种情况下您应该将它们添加到`vms`数组)、 更改它们的名称从默认值，或者想要使用一个不同的名称，您不需要修改此文件中创建一个域。

- **[负载平衡器 adfs.parameters.json] [loadbalancer-adfs-parameters] **该文件包含以下两组配置参数。 `virtualMachineSettings`部分定义 Vm 承载 ADFS 中云服务。 默认情况下，脚本将创建两个虚拟机在同一可用性组︰

    ```json
        "virtualMachinesSettings": {
            "value": {
                "namePrefix": "ra-adfs-adfs",
                "computerNamePrefix": "adfs",
                "size": "Standard_DS1_v2",
                "osType": "windows",
                "adminUsername": "testuser",
                "adminPassword": "AweS0me@PW",
                "osAuthenticationType": "password",
                "nics": [
                    {
                        "isPublic": "false",
                        "subnetName": "adfs",
                        "privateIPAllocationMethod": "Static",
                        "startingIPAddress": "10.0.5.4",
                        "isPrimary": "true",
                        "enableIPForwarding": false,
                        "dnsServers": [ ]
                    }
                ],
                "imageReference": {
                    "publisher": "MicrosoftWindowsServer",
                    "offer": "WindowsServer",
                    "sku": "2012-R2-Datacenter",
                    "version": "latest"
                },
                "dataDisks": {
                    "count": 1,
                    "properties": {
                        "diskSizeGB": 128,
                        "caching": "None",
                        "createOption": "Empty"
                    }
                },
                "osDisk": {
                    "caching": "ReadWrite"
                },
                "extensions": [ ],
                "availabilitySet": {
                    "useExistingAvailabilitySet": "No",
                    "name": "ra-adfs-adfs-vm-as"
                }
            }
        }
        ...
        "buildingBlockSettings": {
            "value": {
                "storageAccountsCount": 2,
                "vmCount": 2,
                "vmStartIndex": 1
            }
        }
    ```

    `loadBalancerSettings`一节提供了有关这些负载平衡器的描述虚拟机。 负载平衡器将出现在端口 443 (HTTPS) 的通信传递给一个或其他的虚拟机︰

    ```json
        "loadBalancerSettings": {
            "value": {
                "name": "ra-adfs-adfs-lb",
                "frontendIPConfigurations": [
                    {
                        "name": "ra-adfs-adfs-lb-fe",
                        "loadBalancerType": "internal",
                        "internalLoadBalancerSettings": {
                            "privateIPAddress": "10.0.5.30",
                            "subnetName": "adfs"
                        }
                    }
                ],
                "backendPools": [
                    {
                        "name": "ra-adfs-adfs-lb-bep",
                        "nicIndex": 0
                    }
                ],
                "loadBalancingRules": [
                    {
                        "name": "https-rule",
                        "frontendPort": 443,
                        "backendPort": 443,
                        "protocol": "Tcp",
                        "backendPoolName": "ra-adfs-adfs-lb-bep",
                        "frontendIPConfigurationName": "ra-adfs-adfs-lb-fe",
                        "probeName": "https-probe",
                        "enableFloatingIP": false
                    }
                ],
                "probes": [
                    {
                        "name": "https-probe",
                        "port": 443,
                        "protocol": "Tcp",
                        "requestPath": null
                    }
                ],
                "inboundNatRules": [ ]
            }
        },
        "virtualNetworkSettings": {
            "value": {
                "name": "ra-adfs-vnet",
                "resourceGroup": "johns-adfs-network-rg"
            }
        }
    ```

- ** [adfs 场域 join.parameters.json ] [ adfs-farm-domain-join-parameters] **。 此文件包含用于将 ADFS 的服务器添加到 CONTOSO 域的设置。 您只需要修改此文件，如果您创建了其他 ADFS 服务器 (更新`vms`在这种情况下阵列)，或已更改的域的名称。

- ** [gmsa.parameters.json][gmsa-parameters]**， ** [adfs 的场-first.parameters.json][adfs-farm-first-parameters]**，和**[adfs 的场-rest.parameters.json][adfs-farm-rest-parameters]**。 该脚本使用在这些文件中的设置创建 ADF 服务器场。 

    *Gmsa.parameters.json*文件包含使用 ADFS 服务组托管服务帐户的设置。 如果您想要更改的帐户或域的名称，您可以修改此文件。

    *Adfs 的场 first.parameters.json*文件包含创建 ADF 服务器场和添加第一台服务器所需的信息。 如果您更改了域或组托管服务帐户的名称，则应更新此文件。

    *Adfs 的场 rest.parameters.json*文件用于将其余 ADFS 服务器添加到服务器场。 同样，如果您更改了域或组托管服务帐户的名称，则应更新此文件。 更新`vms`如果您创建了其他 ADFS 服务器阵列。

- **[负载平衡器 adfsproxy.parameters.json][loadBalancer-adfsproxy-parameters]**。 此文件是在结构和内容类似于*负载平衡器 adfs.parameters.json*文件。 它包含用于生成 ADFS 代理服务器和负载平衡器的数据。

- ** [adfsproxy-场-first.parameters.json][adfsproxy-farm-first-parameters]**，和**[adfsproxy-场-rest.parameters.json][adfsproxy-farm-rest-parameters]**。 脚本使用这些文件中的设置创建 ADF 代理服务器场。 

- ** [virtualNetworkGateway.parameters.json][virtualnetworkgateway-parameters]**。 此文件包含用于创建在云中使用以连接到内部网络的 Azure VPN 网关设置。 您应该修改`sharedKey`中的值`connectionsSettings`的内部部署 VPN 设备相匹配的部分。 有关详细信息，请参见[实现一种混合网络体系结构 Azure 和内部部署 VPN][hybrid-azure-on-prem-vpn]。

- ** [dmz private.parameters.json] [dmz-private-parameters]**和**[dmz public.parameters.json ] [ dmz-public-parameters] **。 这些文件配置入站 （公共） 和出站 （专用） 侧面构成了 DMZ，保护云中的服务器执行虚拟机。 有关这些元素和其配置的详细信息，请参见[实现 Azure 和 Internet 之间的 DMZ][implementing-a-secure-hybrid-network-architecture-with-internet-access]。

- **[负载平衡器 web.parameters json][loadBalancer-web-parameters]**， ** [json 的负载平衡器-biz.parameters][loadBalancer-biz-parameters]**，和**[负载平衡器 data.parameters json][loadBalancer-data-parameters]**。 这些参数文件包含 web、 业务和数据访问层的虚拟机规格和配置每个层的负载平衡器。 这些都是虚拟机，承载 web 应用程序和数据库，并执行组织的业务工作负载。 您可以根据您的要求修改的大小和每一层中的有多少个虚拟机。

- ** [virtualMachines mgmt.parameters.json][virtualMachines-mgmt-parameters]**。 此文件包含跳转框/管理虚拟机的配置。 才可以从跳转框中 web、 业务和数据层的虚拟机到获得登录和管理访问权限。 默认情况下，该脚本创建一个*Standard_DS1_v2* VM，但您可以修改此文件以创建更大或更多虚拟机管理工作量很可能会很大。

## <a name="solution-deployment"></a>解决方案部署

本解决方案假定满足下列先决条件︰

- 具有现有 Azure 订阅，您可以在其中创建资源组。

- 您已经下载并安装最新版本的 Azure Powershell。 请参阅[此处][azure-powershell-download]的说明。

若要运行该脚本来部署解决方案︰

1. 将移动到的便捷文件夹在您的本地计算机上并创建以下子文件夹︰

    - 脚本

    - 脚本/参数

    - 脚本/参数/Onpremise

    - 脚本/参数/Azure

2. 下载[部署 ReferenceArchitecture.ps1] [solution-script]文件到脚本文件夹

3. 下载内容的[参数/onpremise] [ on-premises-folder] Onpremise/参数/脚本文件夹的文件夹︰

4. 下载内容的[参数/azure] [azure-folder]脚本/参数/Azure 文件夹的文件夹。

5. 编辑脚本文件夹中的部署 ReferenceArchitecture.ps1 文件，并更改以下行来指定应创建或用来存放由脚本创建的资源的资源组︰

    ```powershell
    # Azure Onpremise Deployments
        $onpremiseNetworkResourceGroupName = "ra-adfs-onpremise-rg"
        
        # Azure ADDS Deployments
        $azureNetworkResourceGroupName = "ra-adfs-network-rg"
        $workloadResourceGroupName = "ra-adfs-workload-rg"
        $securityResourceGroupName = "ra-adfs-security-rg"
        $addsResourceGroupName = "ra-adfs-adds-rg"
        $adfsResourceGroupName = "ra-adfs-adfs-rg"
        $adfsproxyResourceGroupName = "ra-adfs-proxy-rg"
    ```

6. 编辑参数文件中的脚本/参数/Onpremise 和脚本/参数/Azure 文件夹。 更新资源组文件中的引用这些资源组分配到该部署 ReferenceArchitecture.ps1 文件中的变量的名称相匹配。 下表显示了哪些参数文件引用的资源组。 请注意， *ra adfs 负荷 rg*、 *ra adfs 安全 rg*、 *ra-adfs-添加-rg*、 *ra adfs adfs rg*和*ra adfs 代理 rg*组只用 PowerShell 脚本中不会出现在参数文件中。

  	|资源组|参数文件|
  	|--------------|--------------|
  	|ra adfs onpremise rg|parameters\onpremise\connection.parameters.json<br /> parameters\onpremise\virtualMachines-adds.parameters.json<br />parameters\onpremise\virtualNetwork-adds-dns.parameters.json<br />parameters\onpremise\virtualNetwork.parameters.json<br />parameters\onpremise\virtualNetworkGateway.parameters.json<br />parameters\azure\virtualNetworkGateway.parameters.json
  	|ra adfs 网络 rg|parameters\onpremise\connection.parameters.json<br />parameters\azure\dmz-private.parameters.json<br />parameters\azure\dmz-public.parameters.json<br />parameters\azure\loadBalancer-adfs.parameters.json<br />parameters\azure\loadBalancer-adfsproxy.parameters.json<br />parameters\azure\loadBalancer-biz.parameters.json<br />parameters\azure\loadBalancer-data.parameters.json<br />parameters\azure\loadBalancer-web.parameters.json<br />parameters\azure\virtualMachines-adds.parameters.json<br />parameters\azure\virtualMachines-mgmt.parameters.json<br />parameters\azure\virtualNetwork-with-onpremise-and-azure-dns.parameters.json<br />parameters\azure\virtualNetwork.parameters.json<br />parameters\azure\virtualNetworkGateway.parameters.json （*两次*）

    此外，内部部署的配置和设置云组件 [解决方案组件] [解决方案组件] 节中所述。

7. 打开 Azure PowerShell 窗口，将移动到脚本文件夹中，然后运行以下命令︰

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> <mode>
    ```

    更换`<subscription id>`Azure 的订阅 id

    为`<location>`，指定一个 Azure 的区域，如`eastus`或`westus`。

    `<mode>`参数可以具有下列值之一︰

    - `Onpremise`若要创建模拟的内部部署环境。

    - `Infrastructure`创建 VNet 基础结构并跳转在云中的框。

    - `CreateVpn`以生成 Azure 的虚拟网络网关并将其连接到内部网络。

    - `AzureADDS`以构造虚拟机用作添加服务器，请将 Active Directory 部署到这些虚拟机，并在云中创建域。

    - `AdfsVm`若要创建 ADF 虚拟机并将它们加入到云环境中的域。

    - `ProxyVm`若要生成 ADFS 代理的虚拟机，并将它们加入到云环境中的域。

    - `Prepare`其中执行以上的所有任务。 **这是推荐的选项，如果您正在构建一个完整的新的部署，并且您没有现有的内部部署基础结构。**

    >[AZURE.NOTE] 您还可以运行该脚本与`<mode>`的参数`Workload`来创建网站、 业务、 和数据层的虚拟机和网络。 此安装程序不包含在`Prepare`模式。

    如果您使用`Prepare`选项，则脚本将使用几个小时才能完成，并完成消息*准备完成。请将证书安装到所有 ADF 和代理的虚拟机。*

8.  重新启动 (*ra adfs 管理 vm1* *ra adfs 安全 rg*组中) 中的跳转框，以允许其 DNS 设置才能生效。

9.  [ADFS 获取 SSL 证书][adfs_certificates]和在 ADFS 虚拟机上安装此证书。 请注意，您可以连接到 ADF Vm 通过跳转框。 IP 地址是*10.0.5.4*和*10.0.5.5*。 默认的用户名是用密码*contoso\testuser* *AweSome@PW*。

    >[AZURE.NOTE] 在部署 ReferenceArchitecture.ps1 脚本中的注释此时的详细说明创建自签名的测试证书和授权使用`makecert`命令。 但是，不要使用 makecert 在生产环境中生成的证书。

10. 运行以下 Powershell 命令 ADFS 将服务器场配置︰

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Adfs
    ```

11. 在跳转框中，浏览到*https://adfs.contoso.com/adfs/ls/idpinitiatedsignon.htm*测试 ADF 安装 （您可能会收到证书警告消息，您可以忽略此测试）。 验证 Contoso 公司登录页出现。 *Contoso\testuser*使用的密码以登录*AweS0me@PW*。

12. 在 ADFS 代理的虚拟机上安装 SSL 证书。 IP 地址为*10.0.6.4*和*10.0.6.5*。

13. 运行以下 Powershell 命令将第一个 ADFS 代理服务器配置︰

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Proxy1
    ```

14. 按照脚本所显示的说明进行操作来测试第一个 ADFS 代理服务器的安装。

15. 运行以下 Powershell 命令来配置第二个第一，ADFS 代理服务器︰

    ```powershell
    .\Deploy-ReferenceArchitecture.ps1 <subscription id> <location> Proxy2
    ```

16. 按照说明脚本显示测试的完整 ADFS 代理配置。

## <a name="next-steps"></a>下一步行动

- [Azure 的 Active Directory][aad]。

- [Azure 的 Active Directory B2C][aadb2c]。

<!-- links -->

[vm-recommendations]: ./guidance-compute-single-vm.md#Recommendations
[naming-conventions]: ./guidance-naming-conventions.md
[implementing-active-directory]: ./guidance-identity-adds-extend-domain.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[implementing-a-secure-hybrid-network-architecture]: ./guidance-iaas-ra-secure-vnet-hybrid.md
[implementing-a-secure-hybrid-network-architecture-with-internet-access]: ./guidance-iaas-ra-secure-vnet-dmz.md
[DRS]: https://technet.microsoft.com/library/dn280945.aspx
[where-to-place-an-fs-proxy]: https://technet.microsoft.com/library/dd807048.aspx
[ADDRS]: https://technet.microsoft.com/library/dn486831.aspx
[plan-your-adfs-deployment]: https://msdn.microsoft.com/library/azure/dn151324.aspx
[ad_network_recommendations]: #network_configuration_recommendations_for_AD_DS_VMs
[domain_and_forests]: https://technet.microsoft.com/library/cc759073(v=ws.10).aspx
[adfs_certificates]: https://technet.microsoft.com/library/dn781428(v=ws.11).aspx
[create_service_account_for_adfs_farm]: https://technet.microsoft.com/library/dd807078.aspx
[import_server_authentication_certificate]: https://technet.microsoft.com/library/dd807088.aspx
[adfs-configuration-database]: https://technet.microsoft.com/en-us/library/ee913581(v=ws.11).aspx
[active-directory-federation-services]: https://technet.microsoft.com/windowsserver/dd448613.aspx
[security-considerations]: #security-considerations
[recommendations]: #recommendations
[claims-aware applications]: https://msdn.microsoft.com/en-us/library/windows/desktop/bb736227(v=vs.85).aspx
[active-directory-federation-services-overview]: https://technet.microsoft.com/en-us/library/hh831502(v=ws.11).aspx
[establishing-federation-trust]: https://blogs.msdn.microsoft.com/alextch/2011/06/27/establishing-federation-trust/
[Deploying_a_federation_server_farm]: https://technet.microsoft.com/library/dn486775.aspx
[install_and_configure_the_web_application_proxy_server]: https://technet.microsoft.com/library/dn383662.aspx
[publish_applications_using_AD_FS_preauthentication]: https://technet.microsoft.com/library/dn383640.aspx
[managing-adfs-components]: https://technet.microsoft.com/library/cc759026.aspx
[oms-adfs-pack]: https://www.microsoft.com/download/details.aspx?id=41184
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[aad]: https://azure.microsoft.com/documentation/services/active-directory/
[aadb2c]: https://azure.microsoft.com/documentation/services/active-directory-b2c/
[adfs-intro]: ../active-directory/active-directory-aadconnect-azure-adfs.md
[solution-script]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/Deploy-ReferenceArchitecture.ps1
[on-premises-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adfs/parameters/onpremise
[on-premises-vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/virtualNetwork.parameters.json
[on-premises-virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/virtualMachines-adds.parameters.json
[on-premises-virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/virtualNetworkGateway.parameters.json
[on-premises-connection-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/onpremise/connection.parameters.json
[azure-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-identity-adfs/parameters/azure
[vnet-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualNetwork.parameters.json
[dmz-private-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/dmz-private.parameters.json
[dmz-public-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/dmz-public.parameters.json
[virtualnetworkgateway-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualNetworkGateway.parameters.json
[hybrid-azure-on-prem-vpn]: ./guidance-hybrid-network-vpn.md
[virtualmachines-adds-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualMachines-adds.parameters.json
[extending-ad-to-azure]: ./guidance-identity-adds-extend-domain.md
[loadbalancer-adfs-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-adfs.parameters.json
[add-adds-domain-controller-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/add-adds-domain-controller.parameters.json
[gmsa-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/gmsa.parameters.json
[adfs-farm-first-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfs-farm-first.parameters.json
[adfs-farm-rest-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfs-farm-rest.parameters.json
[adfs-farm-domain-join-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfs-farm-domain-join.parameters.json
[loadBalancer-web-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-web.parameters.json
[loadBalancer-biz-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-biz.parameters.json
[loadBalancer-data-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-data.parameters.json
[virtualMachines-mgmt-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/virtualMachines-mgmt.parameters.json
[loadBalancer-adfsproxy-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/loadBalancer-adfsproxy.parameters.json
[adfsproxy-farm-first-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfsproxy-farm-first.parameters.json
[adfsproxy-farm-rest-parameters]: https://raw.githubusercontent.com/mspnp/reference-architectures/master/guidance-identity-adfs/parameters/azure/adfsproxy-farm-rest.parameters.json
[0]: ./media/guidance-iaas-ra-secure-vnet-adfs/figure1.png "安全与 Active Directory 的混合网络体系结构"
