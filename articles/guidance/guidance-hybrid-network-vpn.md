<properties
   pageTitle="实现使用 Azure 和内部部署 VPN 的混合网络体系结构 |Microsoft Azure"
   description="如何实现跨越 Azure 的虚拟网络，通过使用 VPN 连接的内部网络安全站点到站点网络体系结构。"
   services=""
   documentationCenter="na"
   authors="RohitSharma-pnp"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/24/2016"
   ms.author="roshar"/>

# <a name="implementing-a-hybrid-network-architecture-with-azure-and-on-premises-vpn"></a>实现使用 Azure 和内部部署 VPN 的混合网络体系结构

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

这篇文章概括介绍了一套用于扩展到 Azure 使用站点到站点虚拟专用网络 (VPN) 的内部网络的做法。 内部网络和通过 IPSec VPN 隧道 Azure 虚拟网络 (VNet) 之间流动通信。 这种体系结构是适用于混合应用程序具有以下特点︰

- 应用程序的部分运行内部，而其他人在 Azure 运行。

- 之间的通信部署硬件和云很可能是光，或不如贸易略有扩展延迟时间的灵活性和处理能力的云。

- 扩展的网络构成一个封闭的系统。 没有从互联网到 Azure VNet 的直接路径。

- 用户可以连接到内部网络使用 Azure 中承载的服务。 在内部网络和在 Azure 上运行的服务之间的桥是对用户透明。

方案符合此配置文件的示例包括︰

- 业务线应用程序的组织，使用其中的部分功能已迁移到云。

- 开发/测试/实验室工作负荷。

> [AZURE.NOTE] Azure 具有两种不同的部署模型︰ [Azure 资源管理器][resource-manager-overview]和经典。 该蓝图使用资源管理器，Microsoft 建议为新的部署。

## <a name="architecture-diagram"></a>体系结构关系图

下图突出显示此体系结构中的组件︰

> 包括此体系结构示意图的 Visio 文档是可用于在[Microsoft 下载中心]下载[visio-download]。 此图是"混合网络上的 VPN"页。

![[0]][0]

- **内部网络。** 这是一种计算机和设备，通过组织内部运行的专用本地网络连接的网络。

- **VPN 设备。** 这是一个设备或提供外部连接到内部网络的服务。 VPN 设备可能是硬件设备，也可以是一个软件解决方案，如路由和 Windows Server 2012 中的远程访问服务 (RRAS)。

> [AZURE.NOTE] 有关支持的 VPN 设备和配置选定的 VPN 装置到 Azure VPN 网关连接信息的列表，请参阅[支持的 Azure 的 VPN 设备的列表]中适当的设备说明[vpn-appliance]。

- **N 层云应用程序。** 这是 Azure 中承载的应用程序。 它可能包括多个层，具有多个通过 Azure 负载平衡器连接的子网。 每个子网中的流量可能会产生规则定义通过使用[Azure 网络安全组 (NSGs)][azure-network-security-group]。 有关详细信息，请参阅[Microsoft Azure 安全入门][getting-started-with-azure-security]。

> [AZURE.NOTE] 本文介绍了云应用程序作为单个实体。 请参见[N 层体系结构在 Azure 上的运行][implementing-a-multi-tier-architecture-on-Azure]的详细信息。

- **[虚拟网络 (VNet)][azure-virtual-network]。** 云应用程序和 Azure VPN 网关组件驻留在同一个 VNet。

- **[Azure 的 VPN 网关][azure-vpn-gateway]。** VPN 网关服务可以通过 VPN 设备连接到内部网络的 VNet。 有关详细信息，请参阅[连接到 Microsoft Azure 虚拟网络的内部网络][connect-to-an-Azure-vnet]。 VPN 网关包括以下元素︰

  - **虚拟网络网关。** 这是为 VNet 提供了一个虚拟的 VPN 设备的资源。 它负责路由通信从内部网络到 VNet。

  - **本地网关。** 这是一种抽象的内部部署 VPN 设备。 云应用程序中对内部网络的网络通信通过此网关路由。

  - **连接。** 所指定的连接类型 (IPSec) 和与内部部署 VPN 设备共享的密钥来加密通信属性的连接。

  - **网关的子网。** 虚拟网络网关保存在各自的子网上，受到各种要求，如下面的建议一节中所述。

- **内部负载平衡器。** 从 VPN 网关的网络通信将被路由到云应用程序，通过内部负载平衡器。 负载平衡器位于前端应用程序的子网中。

## <a name="recommendations"></a>建议

Azure 提供许多不同的资源和资源类型中，因此此参考体系结构可以提供许多不同的方法。 我们已经提供了 Azure 资源管理器模板安装的参考体系结构遵循这些建议。 如果您选择创建您自己的参考体系结构应遵循这些建议，除非有推荐不适合特定的要求。

### <a name="vnet-and-gateway-subnet"></a>VNet 和网关的子网

在云中存放组件创建 Azure VNet。 Azure VNet 的地址空间必须足够大以容纳由虚拟机和 VNet 中的子网的地址。 确保 VNet 地址空间可能需要在未来更多虚拟机是否有足够的增长空间。 VNet 的地址空间必须与内部网络中不重叠。 例如，上面的关系图使用 VNet 地址空间 10.20.0.0/16。

创建名为_GatewaySubnet_，/27 的地址范围的子网。 此子网所需的虚拟网络网关，并分配到该子网的 32 个地址将有助于防止您在将来运行计算出来可能关大小限制。 避免放置此子网的地址空间的中间。 一个好的做法是在 VNet 地址空间的上限设置网关子网的地址空间。 图中所示的示例使用 10.20.255.224/27。  一个快速的过程，来计算 CIDR 情况如下︰

1. 将可变位设置为 1，最多被使用的网关的子网，然后将其余的位设置为 0 的位 VNet 的地址空间中。

2. 将结果位转换为十进制数，将它作为其前缀长度设置为大小网关子网的地址空间表示。

例如，对于具有 IP 地址范围为 10.20.0.0/16 VNet，应用上面的步骤 1 成为 10.20.0b11111111.0b11100000。  转换为十进制数字，并根据地址空间会产生 10.20.255.224/27 表达它

> [AZURE.WARNING] 不部署其他虚拟机或角色实例到网关的子网。 另外，不要分配 NSG 到此子网，因为它将导致失灵的网关。

### <a name="virtual-network-gateway"></a>虚拟网络网关

虚拟网络网关分配的公共 IP 地址。

创建虚拟网络网关，网关网中，并将其分配的新分配的公用 IP 地址。 使用网关类型最符合您的要求，这将启用 VPN 设备︰

创建一个[基于策略的网关][policy-based-routing]如果您需要精确控制请求都会被路由如何。 基于策略的条件，如地址前缀。 基于策略的网关使用静态路由，并仅适用于站点对站点连接。

创建一个[基于路由的网关][route-based-routing]如果您连接到使用 RRAS 的内部网络，支持多站点或跨地区的连接，或者实现 VNet 至 VNet （包括路由的遍历多个 VNets） 的连接。 基于路由的网关使用动态路由到网络之间的直接通信。 他们能够容忍优于静态路由的网络路径中的故障，因为它们可以尝试备用路由。 基于路由的网关还可以减少管理开销，因为可能需要网络地址更改时手动更新路由。

有关受支持的 VPN 设备的列表，请参阅[有关 VPN 站点到站点 VPN 网关连接的设备][vpn-appliances]。

> [AZURE.NOTE] 在创建网关后，不能更改网关类型，而无需删除和重新创建网关之间。

选择最接近您的吞吐量要求 Azure VPN 网关 SKU。 Azure 的 VPN 网关将出现在下表中所示的三个 Sku。 每个 SKU 提供不同的吞吐量，[征收费用是][azure-gateway-charges]基于的配置网关的时间及可用。

| SKU | VPN 的吞吐量 | 最大隧道操作|
|-----|----------------|------------------|
| 基本 | 100 Mbps | 10 |
| 标准 | 100 Mbps | 10 |
| 高性能 | 200 Mbps | 30 |

> [AZURE.NOTE] 基本的 SKU 不符合 Azure ExpressRoute。 您可以[更改该 SKU] [changing-SKUs]网关创建之后。

从通往内部负载平衡器，而不是让请求要直接传递到实现应用程序的虚拟机创建网关网的路由规则，直接传入应用程序通信。

### <a name="on-premises-network-connection"></a>内部网络连接

创建一个本地网络网关。 指定在部署 VPN 设备，和内部网络的地址空间的公共 IP 地址。 请注意，内部部署 VPN 设备必须具有可以在 Azure VPN 网关的本地网络网关通过一个公共 IP 地址。 VPN 设备无法找到 NAT 设备后面。

创建虚拟网络网关和本地网络网关的站点到站点连接。 选择站点到站点 (IPSec) 的连接类型，并指定共享的密钥。 使用预共享的密钥进行身份验证的 IPSec 协议基于站点对站点使用 Azure VPN 网关的加密。 当您创建 Azure VPN 网关时，您指定的密钥。 您必须配置 VPN 设备运行内部，用相同的密钥。 目前不支持其他身份验证机制。

确保内部路由基础结构配置为用于 VPN 设备到 Azure VNet 中的地址的转发请求。

打开所需的云应用程序在内部网络中的任何端口。

测试连接以确认︰

- 内部部署 VPN 设备正确传递到云应用程序通过 Azure VPN 网关的通讯。

- VNet 正确路由回内部网络的通信量。

- 禁止在两个方向的通信被阻止正确。

## <a name="scalability-considerations"></a>可伸缩性注意事项

您可以通过将其从基本或标准的 VPN 网关 Sku 移到高性能 VPN SKU 实现垂直伸缩性有限。

预计大量的 VPN 通讯的 VNets，请考虑分发到不同的较小 VNets 不同的工作负载和为每个配置 VPN 网关。

可以水平或垂直分区 VNet。 若要水平分区，请为新 VNet 的子网从每一层移动某些虚拟机实例。 其结果是每个 VNet 具有相同的结构和功能。 若要垂直分区，重新设计每一层将功能划分为不同的逻辑区域 （如处理订单、 开票、 客户管理，等等）。 然后，每个功能区域都可以放在自己 VNet。

复制 VNet，在内部部署 Active Directory 域控制器和 DNS 实现 VNet，可以帮助减少一些与安全相关，并管理通信量从内部到云。

## <a name="availability-considerations"></a>可用性考虑事项

如果您需要确保内部网络保持可用到 Azure VPN 网关，为内部部署 VPN 网关实现故障转移群集。

如果组织中有多个内部站点，创建[多站点连接][vpn-gateway-multi-site]到一个或多个 Azure VNets。 这种方法需要动态路由 （基于路由），因此请确保在部署 VPN 网关支持此功能。

请参阅[VPN 网关的 SLA] [sla-for-vpn-gateway]的 VPN 网关 SLA 有关的详细信息。

## <a name="manageability-considerations"></a>可管理性注意事项

监视来自内部部署 VPN 设备的诊断信息。 此过程取决于 VPN 设备所提供的功能。 例如，如果在 Windows Server 2012 使用路由和远程访问服务，则应启用[RRAS 记录][rras-logging]。

使用[Azure VPN 网关诊断][gateway-diagnostic-logs]以获取有关连接问题的信息。 可以使用这些记录来跟踪信息，例如源和目标的连接请求，使用哪个协议，和如何建立连接 （或尝试失败的原因）。

通过使用 Azure 门户中可用的审核日志来监视 Azure VPN 网关的操作日志。 为本地网络网关，Azure 的网关，并连接提供了单独的日志。 此信息用于跟踪网关，对所做的任何更改，以前工作正常的网关因某种原因停止工作时很有用。

![[2]][2]

监视的连接性，并跟踪连接失败事件。 您可以使用如[Nagios]监视包[nagios]以捕获并报告此信息。

## <a name="security-considerations"></a>安全注意事项

生成不同的共享的密钥，为每个 VPN 网关。 使用强大的共享的密钥来帮助抵御暴力攻击。

> [AZURE.NOTE] 目前，不能到预共享的密钥 Azure VPN 网关使用 Azure 密钥存储库。

确保内部部署 VPN 设备使用[Azure VPN 网关与]兼容的加密方法[vpn-appliance-ipsec]。 基于策略的路由，Azure VPN 网关支持 AES256、 AES128 和 3DES 加密算法。 基于路由的网关支持 AES256 和 3DES。

如果内部部署 VPN 设备上有外围网络和 Internet 之间的防火墙的外围网络中，您可能必须配置[其他防火墙规则][additional-firewall-rules]允许站点到站点 VPN 连接。

如果 VNet 中的应用程序将数据发送到 Internet，请考虑[实施强制隧道][forced-tunneling]将通过内部网络的所有通往 Internet 的通信都路由。 这种方法允许您审核所做的内部部署基础结构的应用程序的传出请求。

> [AZURE.NOTE] 强制隧道可能会影响到 Azure 服务 （例如存储服务） 的连接，Windows 许可证管理器。

## <a name="troubleshooting-considerations"></a>故障排除注意事项

> [AZURE.NOTE] 路由和远程访问博客的一般信息访问[VPN 相关的常见错误疑难解答]有关[troubleshooting-vpn-errors]。

如果通信量不能遍历的 VPN 连接，请检查下列︰

### <a name="on-premises-vpn-appliance"></a>内部部署 VPN 应用装置︰

**内部部署 VPN 设备是否工作正常？**

任何日志文件生成的错误和失败的 VPN 设备的检查。 此信息的位置将因您的装置。 例如，如果您在 Windows Server 2012 使用 RRAS，可以使用以下 PowerShell 命令显示 RRAS 服务事件错误信息︰

```
Get-EventLog -LogName System -EntryType Error -Source RemoteAccess | Format-List -Property *
```

_消息_属性的每个条目提供了错误的描述。 一些常见示例包括︰

- 无法连接，可能是由于指定 RRAS VPN 网络接口配置在 Azure VPN 网关的 IP 地址不正确︰

```
EventID            : 20111
MachineName        : on-prem-vm
Data               : {41, 3, 0, 0}
Index              : 14231
Category           : (0)
CategoryNumber     : 0
EntryType          : Error
Message            : RoutingDomainID- {00000000-0000-0000-0000-000000000000}: A Demand Dial connection to the remote
                     interface AzureGateway on port VPN2-4 was successfully initiated but failed to complete
                     successfully because of the  following error: The network connection between your computer and
                     the VPN server could not be established because the remote server is not responding. This could
                     be because one of the network devices (e.g, firewalls, NAT, routers, etc) between your computer
                     and the remote server is not configured to allow VPN connections. Please contact your
                     Administrator or your service provider to determine which device may be causing the problem.
Source             : RemoteAccess
ReplacementStrings : {{00000000-0000-0000-0000-000000000000}, AzureGateway, VPN2-4, The network connection between
                     your computer and the VPN server could not be established because the remote server is not
                     responding. This could be because one of the network devices (e.g, firewalls, NAT, routers, etc)
                     between your computer and the remote server is not configured to allow VPN connections. Please
                     contact your Administrator or your service provider to determine which device may be causing the
                     problem.}
InstanceId         : 20111
TimeGenerated      : 3/18/2016 1:26:02 PM
TimeWritten        : 3/18/2016 1:26:02 PM
UserName           :
Site               :
Container          :
```

- RRAS VPN 网络接口配置中指定的错误共享的密钥︰

```
EventID            : 20111
MachineName        : on-prem-vm
Data               : {233, 53, 0, 0}
Index              : 14245
Category           : (0)
CategoryNumber     : 0
EntryType          : Error
Message            : RoutingDomainID- {00000000-0000-0000-0000-000000000000}: A Demand Dial connection to the remote
                     interface AzureGateway on port VPN2-4 was successfully initiated but failed to complete
                     successfully because of the  following error: IKE authentication credentials are unacceptable

Source             : RemoteAccess
ReplacementStrings : {{00000000-0000-0000-0000-000000000000}, AzureGateway, VPN2-4, IKE authentication credentials are
                     unacceptable
                     }
InstanceId         : 20111
TimeGenerated      : 3/18/2016 1:34:22 PM
TimeWritten        : 3/18/2016 1:34:22 PM
UserName           :
Site               :
Container          :
```

您还可以获得有关尝试通过以下 PowerShell 命令连接到 RRAS 服务的事件日志信息︰

```
Get-EventLog -LogName Application -Source RasClient | Format-List -Property *
```

如果连接失败，该日志将包含与以下内容类似的错误︰

```
EventID            : 20227
MachineName        : on-prem-vm
Data               : {}
Index              : 4203
Category           : (0)
CategoryNumber     : 0
EntryType          : Error
Message            : CoId={B4000371-A67F-452F-AA4C-3125AA9CFC78}: The user SYSTEM dialed a connection named
                     AzureGateway which has failed. The error code returned on failure is 809.
Source             : RasClient
ReplacementStrings : {{B4000371-A67F-452F-AA4C-3125AA9CFC78}, SYSTEM, AzureGateway, 809}
InstanceId         : 20227
TimeGenerated      : 3/18/2016 1:29:21 PM
TimeWritten        : 3/18/2016 1:29:21 PM
UserName           :
Site               :
Container          :
```

**通过 Azure VPN 网关是 VPN 设备正确路由通信？**

使用诸如[PsPing]之类的工具[psping]来验证连接性和跨 VPN 网关的路由。 例如，若要测试从本地计算机连接到位于 VNet 的 web 服务器，运行以下命令 (替换`<<web-server-address>>`与 web 服务器的地址):

```
PsPing -t <<web-server-address>>:80
```

如果内部机器可以将通信路由到 web 服务器，您应该看到类似于下面的输出︰

```
D:\PSTools>psping -t 10.20.0.5:80

PsPing v2.01 - PsPing - ping, latency, bandwidth measurement utility
Copyright (C) 2012-2014 Mark Russinovich
Sysinternals - www.sysinternals.com

TCP connect to 10.20.0.5:80:
Infinite iterations (warmup 1) connecting test:
Connecting to 10.20.0.5:80 (warmup): 6.21ms
Connecting to 10.20.0.5:80: 3.79ms
Connecting to 10.20.0.5:80: 3.44ms
Connecting to 10.20.0.5:80: 4.81ms

  Sent = 3, Received = 3, Lost = 0 (0% loss),
  Minimum = 3.44ms, Maximum = 4.81ms, Average = 4.01ms
```

如果本地计算机不能与指定的目标通信，您将看到如下消息︰

```
D:\PSTools>psping -t 10.20.1.6:80

PsPing v2.01 - PsPing - ping, latency, bandwidth measurement utility
Copyright (C) 2012-2014 Mark Russinovich
Sysinternals - www.sysinternals.com

TCP connect to 10.20.1.6:80:
Infinite iterations (warmup 1) connecting test:
Connecting to 10.20.1.6:80 (warmup): This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80: This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80: This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80: This operation returned because the timeout period expired.
Connecting to 10.20.1.6:80:
  Sent = 3, Received = 0, Lost = 3 (100% loss),
  Minimum = 0.00ms, Maximum = 0.00ms, Average = 0.00ms
```

**内部防火墙是否允许通过 VPN 的通讯？已打开正确的端口？**

验证内部部署 VPN 设备使用[Azure VPN 网关与]兼容的加密方法[vpn-appliance]。

基于策略的路由，Azure VPN 网关支持 AES256、 AES128 和 3DES 加密算法。 基于路由的网关支持 AES256 和 3DES。

### <a name="azure-vnet-gateway"></a>Azure VNet 网关︰

检查[诊断日志，Azure VPN 网关][gateway-diagnostic-logs]为潜在的问题。

**是 Azure VPN 网关和内部部署 VPN 设备使用相同的共享身份验证密钥配置？**

您可以查看通过使用以下 Azure CLI 命令存储由 Azure VPN 网关共享的密钥︰

```
azure network vpn-connection shared-key show <<resource-group>> <<vpn-connection-name>>
```

使用适合您的部署 VPN 设备的命令显示用于该装置配置的共享的密钥。

验证持有 Azure VPN 网关不 NSG 与相关联的_GatewaySubnet_子网。

您可以通过使用以下 Azure CLI 命令查看子网的详细信息︰

```
azure network vnet subnet show -g <<resource-group>> -e <<vnet-name>> -n GatewaySubnet
```

确保没有数据字段命名为_网络安全组 id_。 下面的示例显示具有分配的 NSG （_VPN 网关组_） _GatewaySubnet_实例的结果。 这会防止不能正常工作，如果有任何规则定义为此 NSG 的网关︰

```
C:\>azure network vnet subnet show -g profx-prod-rg -e profx-vnet -n GatewaySubnet
    info:    Executing command network vnet subnet show
    + Looking up virtual network "profx-vnet"
    + Looking up the subnet "GatewaySubnet"
    data:    Id                              : /subscriptions/########-####-####-####-############/resourceGroups/profx-prod-rg/providers/Microsoft.Network/virtualNetworks/profx-vnet/subnets/GatewaySubnet
    data:    Name                            : GatewaySubnet
    data:    Provisioning state              : Succeeded
    data:    Address prefix                  : 10.20.3.0/27
    data:    Network Security Group id       : /subscriptions/########-####-####-####-############/resourceGroups/profx-prod-rg/providers/Microsoft.Network/networkSecurityGroups/VPN-Gateway-Group
    info:    network vnet subnet show command OK
```

**Azure VNet 中的虚拟机配置为允许来自 VNet 的外部通信？检查任何 NSG 规则与包含这些虚拟机的子网。**

您可以通过使用以下 Azure CLI 命令查看 NSG 的所有规则︰

```
azure network nsg show -g <<resource-group>> -n <<nsg-name>>
```

**已断开 Azure VPN 网关？**

可以使用下面的 Azure PowerShell 命令检查 Azure VPN 连接的当前状态。 `<<connection-name>>`参数是虚拟网络网关和本地网关链接在 Azure VPN 连接的名称。

```
Get-AzureRmVirtualNetworkGatewayConnection -Name <<connection-name>> - ResourceGroupName <<resource-group>>
```

下面的代码段突出显示如果网关是连接 （第一个示例），并断开连接生成的输出 （第二个示例）︰

```
PS C:\> Get-AzureRmVirtualNetworkGatewayConnection -Name profx-gateway-connection -ResourceGroupName profx-prod-rg

AuthorizationKey           :
VirtualNetworkGateway1     : Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
VirtualNetworkGateway2     :
LocalNetworkGateway2       : Microsoft.Azure.Commands.Network.Models.PSLocalNetworkGateway
Peer                       :
ConnectionType             : IPsec
RoutingWeight              : 0
SharedKey                  : ####################################
ConnectionStatus           : Connected
EgressBytesTransferred     : 55254803
IngressBytesTransferred    : 32227221
ProvisioningState          : Succeeded
...
```

```
PS C:\> Get-AzureRmVirtualNetworkGatewayConnection -Name profx-gateway-connection2 -ResourceGroupName profx-prod-rg

AuthorizationKey           :
VirtualNetworkGateway1     : Microsoft.Azure.Commands.Network.Models.PSVirtualNetworkGateway
VirtualNetworkGateway2     :
LocalNetworkGateway2       : Microsoft.Azure.Commands.Network.Models.PSLocalNetworkGateway
Peer                       :
ConnectionType             : IPsec
RoutingWeight              : 0
SharedKey                  : ####################################
ConnectionStatus           : NotConnected
EgressBytesTransferred     : 0
IngressBytesTransferred    : 0
ProvisioningState          : Succeeded
...
```

### <a name="host-vm-configuration-network-bandwidth-utilization-and-application-performance"></a>主机的虚拟机配置、 网络带宽利用率和应用程序的性能︰

验证正确配置子网中 Azure Vm 上运行的来宾操作系统中的防火墙以允许来自内部 IP 范围允许的通信。

**是接近或超过限制带宽可用到 Azure VPN 网关？**

刀具取决于可用的 VPN 设备运行内部设施。 例如，如果您在 Windows Server 2012 使用 RRAS，可用于性能监视器跟踪卷的数据被接收和传输通过 VPN 连接;使用_RAS 总_对象，选择_接收的字节数/秒_和_传输的字节数/秒_计数器︰

![[3]][3]

应将 （100 Mbps 的基本和标准 Sku 和高性能 sku 200 Mbps） 的 VPN 网关使用的带宽使用的结果进行比较︰

![[4]][4]

**任何 Azure VNet 中的虚拟机运行缓慢？这些重载的是，有足够的他们能够处理负载，有所有负载平衡的配置正确吗？**

这就要求[捕获并分析诊断信息][azure-vm-diagnostics]。 您可以检查结果使用 Azure 的门户，但也是很多第三方工具可用，可以深入详细的性能数据。

**是云资源的使用进行有效的应用程序？**

检测以确定应用程序是否正在进行资源的最佳使用每个虚拟机上运行的应用程序代码。 您可以使用[应用程序的见解]等工具[application-insights]以帮助执行这些任务。

## <a name="solution-deployment"></a>解决方案部署

如果您已配置 VPN 装置与现有的内部基础结构，您可以按照下面的步骤部署参考体系结构︰

1. 右键单击下面的按钮，然后选择"在新选项卡中的打开链接"或"在新窗口中的打开链接":  
[![将部署到 Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-hybrid-network-vpn%2Fazuredeploy.json)

2. 等待该链接打开在 Azure 的门户中，然后按照下列步骤操作︰ 
    - **资源组**名称已定义参数文件中，因此选择**新建**，然后输入`ra-hybrid-vpn-rg`在文本框中。
    - 从**位置**下拉框中选择的地区。
    - 不允许编辑**模板根 Uri**或**参数根 Uri**的文本框。
    - 检查的条款和条件，然后单击**我同意的条款和条件如以上所述**复选框。
    - 单击**购买**按钮。

3. 等待部署完成。

## <a name="next-steps"></a>下一步行动

- [安装在 Azure VNet 中使用虚拟机的内部部署 Active Directory 域的额外域控制器][installing-ad]。

- [部署 Windows 服务器在 Azure Vm 上的 Active Directory 的准则][deploying-ad]。

- [在 VNet 中创建一个 DNS 服务器][creating-dns]。

- [配置和管理 DNS 中 VNet][configuring-dns]。

- [跨 VPN 使用内部 Stormshield 网络安全装置][stormshield]。

- [实施混合网络体系结构与 Azure ExpressRoute][expressroute]。

<!-- links -->

[implementing-a-multi-tier-architecture-on-Azure]: ./guidance-compute-n-tier-vm.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[arm-templates]: ../resource-group-authoring-templates.md
[azure-cli]: ../virtual-machines-command-line-tools.md
[azure-portal]: ../azure-portal/resource-group-portal.md
[azure-powershell]: ../powershell-azure-resource-manager.md
[azure-virtual-network]: ../virtual-network/virtual-networks-overview.md
[vpn-appliance]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md
[azure-vpn-gateway]: https://azure.microsoft.com/services/vpn-gateway/
[azure-gateway-charges]: https://azure.microsoft.com/pricing/details/vpn-gateway/
[azure-network-security-group]: https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/
[connect-to-an-Azure-vnet]: https://technet.microsoft.com/library/dn786406.aspx
[vpn-gateway-multi-site]: ../vpn-gateway/vpn-gateway-multi-site.md
[policy-based-routing]: https://en.wikipedia.org/wiki/Policy-based_routing
[route-based-routing]: https://en.wikipedia.org/wiki/Static_routing
[network-security-group]: ../virtual-network/virtual-networks-nsg.md
[sla-for-vpn-gateway]: https://azure.microsoft.com/support/legal/sla/vpn-gateway/v1_0/
[additional-firewall-rules]: https://technet.microsoft.com/library/dn786406.aspx#firewall
[nagios]: https://www.nagios.org/
[azure-vpn-gateway-diagnostics]: http://blogs.technet.com/b/keithmayer/archive/2014/12/18/diagnose-azure-virtual-network-vpn-connectivity-issues-with-powershell.aspx
[ping]: https://technet.microsoft.com/library/ff961503.aspx
[tracert]: https://technet.microsoft.com/library/ff961507.aspx
[psping]: http://technet.microsoft.com/sysinternals/jj729731.aspx
[nmap]: http://nmap.org
[changing-SKUs]: https://azure.microsoft.com/blog/azure-virtual-network-gateway-improvements/
[gateway-diagnostic-logs]: http://blogs.technet.com/b/keithmayer/archive/2015/12/07/step-by-step-capturing-azure-resource-manager-arm-vnet-gateway-diagnostic-logs.aspx
[troubleshooting-vpn-errors]: https://blogs.technet.microsoft.com/rrasblog/2009/08/12/troubleshooting-common-vpn-related-errors/
[rras-logging]: https://www.petri.com/enable-diagnostic-logging-in-windows-server-2012-r2-routing-and-remote-access
[create-on-prem-network]: https://technet.microsoft.com/library/dn786406.aspx#routing
[create-azure-vnet]: ../virtual-network/virtual-networks-create-vnet-classic-cli.md
[azure-vm-diagnostics]: https://azure.microsoft.com/blog/windows-azure-virtual-machine-monitoring-with-wad-extension/
[application-insights]: ../application-insights/app-insights-overview-usage.md
[forced-tunneling]: https://azure.microsoft.com/documentation/articles/vpn-gateway-about-forced-tunneling/
[getting-started-with-azure-security]: ./../security/azure-security-getting-started.md
[vpn-appliances]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md
[installing-ad]: ../active-directory/active-directory-install-replica-active-directory-domain-controller.md
[deploying-ad]: https://msdn.microsoft.com/library/azure/jj156090.aspx
[creating-dns]: https://blogs.msdn.microsoft.com/mcsuksoldev/2014/03/04/creating-a-dns-server-in-azure-iaas/
[configuring-dns]: ../virtual-network/virtual-networks-manage-dns-in-vnet.md
[stormshield]: https://azure.microsoft.com/marketplace/partners/stormshield/stormshield-network-security-for-cloud/
[vpn-appliance-ipsec]: ../vpn-gateway/vpn-gateway-about-vpn-devices.md#ipsec-parameters
[expressroute]: ./guidance-hybrid-network-expressroute.md
[naming conventions]: ./guidance-naming-conventions.md
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/deploy-reference-architecture.sh
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vnet-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/parameters/virtualNetwork.parameters.json
[virtualNetworkGateway-parameters]: https://github.com/mspnp/reference-architectures/tree/master/guidance-hybrid-network-vpn/parameters/virtualNetworkGateway.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[azure-cli]: https://azure.microsoft.com/documentation/articles/xplat-cli-install/
[0]: ./media/blueprints/hybrid-network-vpn.png "结构的混合体网络跨越内部部署和云基础结构"
[1]: ./media/guidance-hybrid-network-vpn/partitioned-vpn.png "分区来改善可伸缩性 VNet"
[2]: ./media/guidance-hybrid-network-vpn/audit-logs.png "在 Azure 的门户网站的审核日志"
[3]: ./media/guidance-hybrid-network-vpn/RRAS-perf-counters.png "用于监视的 VPN 网络通信性能计数器"
[4]: ./media/guidance-hybrid-network-vpn/RRAS-perf-graph.png "示例 VPN 网络性能图"