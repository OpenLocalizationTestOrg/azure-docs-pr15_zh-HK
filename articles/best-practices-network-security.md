<properties
   pageTitle="Microsoft 的云服务和网络安全 |Microsoft Azure"
   description="了解一些 Azure 来帮助创建安全的网络环境中的关键功能"
   services="virtual-network"
   documentationCenter="na"
   authors="tracsman"
   manager="rossort"
   editor=""/>

<tags
   ms.service="virtual-network"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="03/14/2016"
   ms.author="jonor;sivae"/>

# <a name="microsoft-cloud-services-and-network-security"></a>Microsoft 的云服务和网络安全

Microsoft 的云服务提供 hyperscale 服务和基础架构、 企业级功能和混合连接的多种选择。 客户可以选择来访问这些服务通过互联网，也可以使用 Azure ExpressRoute，提供专用网络的连接。 Microsoft Azure 平台使客户能够无缝地将他们的基础结构扩展到云并生成多层体系结构。 此外，第三方可以通过提供安全服务和虚拟装置启用增强的功能。 本白皮书概述了安全和结构使用 Microsoft 通过 ExpressRoute 的云服务时，客户应考虑的问题。 它还介绍了在虚拟网络中 Azure 创建更安全的服务。

## <a name="fast-start"></a>快速起步
下图中的逻辑可以指导您使用 Azure 平台可用的许多安全技术的一个具体的例子。 为了便于快速参考，找到最适合您的情况的示例。 有关更完整的解释，继续通过纸张阅读。
![安全选项流程图][0]

[示例 1︰ 建立外围网络 （也称为 DMZ、 非军事区和屏蔽子网） 来帮助保护网络安全组 (NSGs) 的应用程序。](#example-1-build-a-simple-dmz-with-nsgs)</br>
[示例 2︰ 生成一个外围网络来帮助保护防火墙和 NSGs 的应用程序。](#example-2-build-a-dmz-to-protect-applications-with-a-firewall-and-nsgs)</br>
[示例 3︰ 建立外围网络来帮助保护防火墙、 用户定义工艺路线 (UDR) 和 NSG 的网络。](#example-3-build-a-dmz-to-protect-networks-with-a-firewall-udr-and-nsg)</br>
[示例 4︰ 添加混合站点对站点的虚拟装置虚拟专用网络 (VPN) 连接。](#example-4-adding-a-hybrid-connection-with-a-site-to-site-virtual-appliance-vpn)</br>
[示例 5︰ 添加混合连接的 VPN 站点到站点，Azure 关。](#example-5-adding-a-hybrid-connection-with-a-site-to-site-azure-gateway-vpn)</br>
[示例 6︰ 添加与 ExpressRoute 的混合连接。](#example-6-adding-a-hybrid-connection-with-expressroute)</br>
添加虚拟网络、 高可用性和服务链之间的连接的示例将添加到此文档中，在今后几个月中。

## <a name="microsoft-compliance-and-infrastructure-protection"></a>Microsoft 的法规遵从性和基础设施保护
Microsoft 已采取支持法规遵从性计划的企业客户需求的领先地位。 以下是一些有关 Azure 的法规遵从性认证︰ ![Azure 的法规遵从性徽章][1]

有关详细信息，请参阅[Microsoft 信任中心](https://azure.microsoft.com/support/trust-center/compliance/)的法规遵从性信息。

Microsoft 具有全面的做法来保护运行 hyperscale 全球服务所需的云基础设施。 Microsoft 的云基础结构包括硬件、 软件、 网络及管理和操作人员，除了物理数据中心。

![Azure 的安全功能][2]

这种方法奠定了更安全的客户部署 Microsoft 云中的服务。 下一步是为客户设计和创建一个安全的体系结构来保护这些服务。

## <a name="traditional-security-architectures-and-perimeter-networks"></a>传统的安全体系结构和外围网络
尽管 Microsoft 投入巨资实施保护的云基础架构中，客户还必须保护他们的云服务和资源组。 对安全的多层的方法提供最好的防护。 外围网络安全区域来自不受信任的网络保护内部网络资源。 外围网络是指边或网络的互联网与受保护的企业的 IT 基础结构之间的部分。

在典型的企业网络外围环境，提供多层的安全设备在很大程度加强核心基础结构。 每一层的边界包括设备和策略实施点。 设备可能包括︰ 防火墙、 分布式拒绝服务 (DDoS) 防护、 入侵检测或保护系统 (IDS/IPS) 和 VPN 设备。 策略实施可以采用的防火墙策略、 访问控制列表 (Acl) 或特定路由。 在网络中，直接接受从 Internet 传入通信的第一防线是同时进入网络允许合法请求进一步阻止攻击和有害的通信量为这些机制的组合。 这种通信将直接路由到外围网络中的资源。 该资源可能再"对话"在网络中，transiting 进行验证的下一个边界更深层次的资源第一。 外面的层称为外围网络，因为这属于网络暴露给 Internet，通常使用某种形式的两端的保护。 下图显示单个子网外围网络的一个例子，在公司网络中，使用两个安全边界。

![在企业网络外围网络][3]

有许多用来实现外围网络中，从简单的负载平衡器在 web 场中，为多子网外围网络与每个边界拦截通信量并保护公司网络的更深层次的图层上的各种机制的体系结构。 外围网络的生成方式，取决于组织和其总体风险容许的特定需要。

客户将其工作量转移到公有云，它非常重要，以支持对外围网络体系结构以满足法规遵从性和安全要求的 Azure 中的类似功能。 本文档提供了客户是如何构建一个安全的网络环境在 Azure 中的准则。 它侧重于外围网络中，但还包括网络安全的许多方面的全面讨论。 以下问题通知这一讨论︰

- 如何生成在 Azure 外围网络？
- 一些 Azure 的功能可以用于生成外围网络是什么？
- 如何保护后端工作负载？
- 如何控制在 Azure 中的工作负荷对互联网通信？
- 如何可以内部网络免受在 Azure 中的部署？
- 何时本机 Azure 安全功能应与第三方设备或服务？

下图显示了各个 Azure 为客户提供了的安全的层。 这些图层是本机在 Azure 平台本身和自定义功能︰

![Azure 的安全体系结构][4]

从互联网有助于防止大规模攻击 Azure 的 Azure DDoS 的入站。 下一层是客户定义公共端点，以用来确定哪些通信量可以通过虚拟网络云服务。 本机的 Azure 虚拟网络隔离可确保从所有其他的网络，完全隔离和流量仅流动用户配置路径和方法。 这些路径和方法是下一层，其中 NSGs，UDR 和网络虚拟装置可用于创建安全边界，以保护在受保护的网络中的应用程序部署。

下一节概述了 Azure 的虚拟网络。 这些虚拟网络创建的客户，并被连接到其部署的工作量。 虚拟网络是建立外围网络来保护在 Azure 中的客户部署所需的所有网络安全功能的基础。

## <a name="overview-of-azure-virtual-networks"></a>Azure 的虚拟网络的概述
互联网流量，可以转到 Azure 的虚拟网络之前，有两层安全固有 Azure 平台︰

1.  **DDoS 保护**︰ DDoS 保护是防止大规模的基于 Internet 的攻击 Azure 平台本身的 Azure 物理网络层。 这些攻击使用"bot"的多个节点试图搞垮互联网服务。 Azure 具有强健的 DDoS 保护网格上所有入站 Internet 连接。 此 DDoS 保护层没有用户可配置的属性，不能由客户访问。 这保护 Azure 作为一个平台，从大规模的攻击，但不是会直接保护单个客户应用程序。 本地化攻击针对客户可以配置其他层的适应性。 例如，如果客户被攻击公共端点上大规模 DDoS 攻击，Azure 阻止连接到该服务。 客户无法故障转移到另一个虚拟的网络或服务终结点不涉及攻击恢复服务的。 应该注意的是尽管客户可能会影响在该端点上，会影响该终结点之外的任何其他服务。 此外，其他客户和服务会看到从该攻击没有任何影响。
2.  **服务终结点**︰ 终结点使云服务或资源组具有公共互联网 IP 地址和端口公开。 终结点将使用网络地址转换 (NAT) 将通信路由到内部地址和端口上的 Azure 的虚拟网络。 这是外部的通信传入虚拟网络的主要路径。 服务终结点是用户可配置，确定哪些通信量传递，和如何以及在何处它转换到虚拟的网络上。

一旦流量达到虚拟网络，有许多功能可以派上用场。 Azure 的虚拟网络是客户来把它们的工作负载和位置基本网络级安全应用的基础。 为客户提供了以下功能和特征在 Azure 中是私有网络 （虚拟网络覆盖）︰
 
- **通信隔离**︰ 虚拟网络是在 Azure 平台上的通信隔离边界。 在一个虚拟的网络中的虚拟机 (Vm) 直接向虚拟机在不同的虚拟网络中，无法进行通信，即使这两个虚拟网络由同一个客户。 这是一个关键属性，以确保客户虚拟机和通信保持私有虚拟的网络中。
- **多层拓扑**︰ 虚拟网络，客户可以通过分配子网并指定不同的元素或其工作负载的"层"的单独的地址空间定义多层拓扑结构。 这些逻辑分组和拓扑结构使客户能够定义不同的访问策略，根据工作负荷类型，并且还控制层之间的通信流。
- **跨内部连接**︰ 客户可以在 Azure 中建立跨内部虚拟网络和多个本地站点或其他虚拟网络之间的连接。 为此，客户可以使用 Azure VPN 网关或第三方网络虚拟装置。 Azure 支持使用标准的 IPsec/IKE 协议和 ExpressRoute 专用连接的站点到站点 (S2S) Vpn。
- **NSG**使客户能够创建规则 (Acl)，在所需的粒度级别︰ 网络接口、 单个虚拟机或虚拟子网。 客户可以通过允许或拒绝在虚拟的网络，从跨内部连接，通过客户的网络上的系统内的工作负荷之间的通信来控制访问，或者直接将互联网通信。
- **UDR**和**IP 转发**允许客户定义在虚拟网络中的不同层之间的通信路径。 客户可以部署防火墙、 IDS/IPS 和其他虚拟装置，并将网络流量路由通过这些安全装置的安全边界策略实施、 审核和检查。
- 在 Azure 市场**网络虚拟装置**︰ 安全装置，如防火墙、 负载平衡器和 IDS/IPS Azure 市场和 VM 映像库中都可用。 客户可以部署到其虚拟网络和具体来说，在其安全边界 （包括外围网络子网中） 这些装置来完成多层安全的网络环境。

这些特性和功能，如何可以在 Azure 中构造外围网络体系结构的一个例子是如下︰

![在 Azure 的虚拟网络外围网络][5]

## <a name="perimeter-network-characteristics-and-requirements"></a>外围网络特征和需求
外围网络可直接与来自 Internet 的通信交互网络的前端。 传入的数据包应流动安全装置，如防火墙、 ID 和 IP，然后再到达后端服务器。 从工作量通往 Internet 的数据包可以也流过外围网络中的安全装置，策略实施、 检查和审计目的之前离开网络。 此外，外围网络可以承载客户虚拟网络和内部网络之间的交叉部署 VPN 网关。

### <a name="perimeter-network-characteristics"></a>外围网络特征
引用上图中，一些良好的外围网络的特征如下︰

- 面向 Internet 的︰
    - 外围网络子网本身是面向 Internet 的直接与互联网通信。
    - 公共 IPs、 Vip，和/或服务终结点将 Internet 通信传递到前端网络和设备。
    - 来自 Internet 的入站的通信通过前端网络上的其他资源之前的安全设备。
    - 如果启用了出站安全，通信通过安全设备，作为最后一步，在互联网传递前。
- 受保护的网络︰
    - 没有从互联网到核心基础结构的直接路径。
    - 通道的核心基础结构必须遍历安全设备，如 NSGs、 防火墙或 VPN 设备。
    - 互联网和核心基础结构，其他设备必须未桥接。
    - 面向 Internet 的和受保护的网络面临外围网络 （例如，两个防火墙图标上图所示） 的边界上的安全设备实际上可能是一个虚拟应用装置与区分的规则或接口的每个边界。 （就一种设备，从逻辑上分隔，处理这两个边界外围网络的负载。）
- 其他常见的做法和约束︰
    - 工作负载不得存储关键业务信息。
    - 访问和更新到外围网络配置和部署只限于只有经过授权的管理员。

### <a name="perimeter-network-requirements"></a>外围网络要求
若要启用这些特性，在虚拟的网络需求，来实施一个成功的外围网络中遵循以下准则︰

- **网体系结构︰**指定的虚拟网络，以致整个子网致力为外围网络中，并与相同的虚拟网络中的其他子网分开的。 这将确保外围网络和其他内部或专用子网层流通过防火墙或 IDS/IPS 使用用户定义的路由子网边界的虚拟应用装置之间的通信。
- **NSG:**外围网络子网本身应打开，以允许通信与互联网，但这并不表示应绕过客户 NSGs。 按照常见的安全措施，尽量减少暴露给 Internet 的网络曲面。 锁定可以访问部署或特定的应用程序协议和打开的端口的远程地址范围。 可能会有情况，但是，在这并不总能。 例如，如果客户可以在 Azure 中的外部网站，外围网络应允许来自任何公共 IP 地址，传入的 web 请求，但只应打开的 web 应用程序端口︰ TCP:80 和 TCP:443。
- **路由表︰**外围网络子网本身应该能够直接对互联网进行通信，但不是应允许后端或内部网络的直接通信而不需要通过防火墙或安全装置。
- **安全装置配置︰**以路由并检查数据包之间的外围网络和其他受保护的网络、 安全装置，如防火墙、 ID 和 IP 设备可能多穴。 他们可能有单独的 Nic 的外围网络和后端的子网。 在外围网络中的 Nic 进行通信直接与互联网，与相应的 NSGs 和外围网络的路由表。 Nic 连接到后端的子网的详细信息限制了 NSGs 和相应的后端子网的路由表。
- **安全装置功能︰**通常部署在外围网络中的安全装置执行以下功能︰
    - 防火墙︰ 实施防火墙规则或对传入的请求的访问控制策略。
    - 威胁检测和预防︰ 检测和减少来自互联网的恶意攻击。
    - 审核和日志记录︰ 维护详细的日志中的审核和分析。
    - 反向代理︰ 将传入的请求重定向到相应的后端服务器。 这包括映射和翻译上的前端设备，目标地址通常防火墙、 到后端服务器的地址。
    - 转发代理服务器︰ 提供 NAT 和执行从启动虚拟网络中，互联网通信的审核。
    - 路由器︰ 转发传入和跨网通信在虚拟的网络中。
    - VPN 设备︰ 作为客户内部网络和 Azure 的虚拟网络之间的跨部署 VPN 连接跨部署 VPN 网关。
    - VPN 服务器︰ 接受 VPN 客户端连接到 Azure 的虚拟网络。

>[AZURE.TIP] 将以下两组分开︰ 个人有权访问外围网络安全设备和作为应用程序开发、 部署、 或操作管理员授权的个人。 通过将这些组分开，使职责划分，从而防止一个人绕过应用程序安全和网络安全控制。

### <a name="questions-to-be-asked-when-building-network-boundaries"></a>要构建网络边界时，系统会要求的问题
在本节中，除非特别提到，"网络"一词是指订阅管理员创建的专用 Azure 的虚拟网络。 一词不是指内 Azure 的基础物理网络。

此外，Azure 的虚拟网络通常用于扩展传统的内部部署网络。 也可以将站点到站点或外围网络体系结构的 ExpressRoute 混合网络解决方案合并。 这是重要的考虑因素，构建网络安全边界。

以下三个问题是关键，回答时您正在构建一个外围网络和多个安全边界的网络。

#### <a name="1-how-many-boundaries-are-needed"></a>需要 1） 数量的边界？
第一个决策点是确定在给定方案中需要多少安全边界︰

- 有一个边界︰ 在前端外围网络，虚拟的网络和 Internet 之间。
- 两个边界︰ 一个在外围网络中，而另一个外围网络子网和 Azure 的虚拟网络中的后端子网之间的互联网端上。
- 三个边界︰ 在外围网络的互联网边、 外围网络和后端的子网之间的一个和一个后端的子网和内部网络之间。
- N 边界︰ 数目可变。 根据安全要求，没有真正任意数量的可应用于给定网络的安全边界。

数量和类型的边界需要有所不同取决于一个公司的风险容许和正在实现的具体方案。 这通常是通过组织，通常包括风险和法规遵从性团队、 网络和平台团队和应用程序开发团队中的多个组所做的联合决策。 具有安全、 所涉及的数据和所使用的技术知识的人应该说这个决定，以确保每个实施适当的安全姿态中。

>[AZURE.TIP] 使用边界满足给定情况下的安全要求的最小数目。 与多个边界的难度操作和疑难解答可以以及随着时间的推移管理多个边界策略所涉及的管理开销。 然而，没有足够的边界增加风险。 找到平衡点是至关重要的。

![有三种安全边界混合网络][6]

上图显示了三种安全边界网络的高级视图。 外围网络和互联网，Azure 前端和后端专用子网，和 Azure 的后端网和企业内部网络之间的边界。

#### <a name="2-where-are-the-boundaries-located"></a>2） 边界的位置？
一旦确定的边界，实现它们的位置是下一步的决策点。 通常有三个选项︰
- 使用基于互联网的中间服务 （例如，基于云的 Web 应用程序防火墙，这不在本文讨论范围）
- 在 Azure 使用本机功能和/或网络虚拟应用装置
- 在内部网络上使用物理设备

纯粹是 Azure 的网络上的选项是本机 Azure 功能 （例如，Azure 负载平衡器） 或网络虚拟设备的庞大的合作伙伴生态系统从 Azure 的 （例如，检查点防火墙）。

如果 Azure 和内部网络之间需要一种边界，则安全设备可以驻留两侧的连接 （或双方）。 因此决定必须进行位置上放置安全齿轮。

在上图中，互联网对外围网络和前端到后端边界完全包含在 Azure，并且必须是本机 Azure 的功能或网络虚拟装置。 Azure 侧或内部一侧或甚至在两面上的设备的组合上，可能是在 Azure （后端的子网） 和公司网络之间的边界上的安全设备。 可以明显的优势和缺点，必须认真考虑选择任一选项。

例如，通过内部网络侧的现有物理安全齿轮的优点，没有新的齿轮。 它只是需要重新配置。 缺点，但是，是，所有的通信必须回到从 Azure 内部网络，通过安全齿轮。 因此 Azure 到 Azure 通信可能会导致明显的延迟，并且影响应用程序性能和用户体验，如果被迫回内部网络中的安全策略的强制执行。

#### <a name="3-how-are-the-boundaries-implemented"></a>3） 如何实施边界？
每个安全边界可能有不同的功能需求 （例如，ID 和在外围网络中，但只有 Acl 的外围网络和后端的子网之间的互联网端的防火墙规则）。 决定要使用的设备取决于方案和安全要求。 在下面的部分中，1、 2 和 3 的示例讨论一些可用的选项。 检查 Azure 本机的网络功能和 Azure 中的合作伙伴生态系统提供的设备显示了无数的方式可以解决几乎任何方案。

另一个关键实现决策点是如何使用 Azure 连接的内部网络。 您应该使用 Azure 的虚拟网关或网络虚拟应用装置？ 这些选项将在以下部分中 （例如 4、 5 和 6） 更详细地讨论。

此外，可能需要在 Azure 中的虚拟网络之间的通信。 这些方案将在以后添加。

一旦知道前一个问题的答案，[快速起步](#fast-start)部分可以帮助确定最适合于给定方案的示例。

## <a name="examples-building-security-boundaries-with-azure-virtual-networks"></a>示例︰ 构建与 Azure 的虚拟网络的安全边界
### <a name="example-1-build-a-perimeter-network-to-help-protect-applications-with-nsgs"></a>示例 1︰ 构建一个外围网络来保护应用程序与 NSGs
[快速起步回](#fast-start) | [详细生成此示例的说明][Example1]

![NSG 的入站的外围网络][7]

#### <a name="environment-description"></a>环境的描述
在此示例中，没有订阅包含以下︰

- 两个云服务:"FrontEnd001"和"BackEnd001"
- 使用的虚拟网络，"CorpNetwork"两个子网:"前端"和"后端"
- 应用于两个子网的网络安全组
- 一台 Windows 服务器表示应用程序 web 服务器 ("IIS01")
- 表示应用程序的后端服务器 （"AppVM01"、"AppVM02"） 的两个 Windows 服务器
- 一台 Windows 服务器，表示 DNS 服务器 ("DNS01")

有关脚本和 Azure 资源管理器模板，请参阅[生成详细的说明][Example1]。

#### <a name="nsg-description"></a>NSG 说明
在此示例中，NSG 组生成和再加载的六个规则。

>[AZURE.TIP] 通常情况下，您应该创建您具体的"允许"规则首先，跟更具一般性的"拒绝"规则。 给定的优先级将决定哪些规则都应首先评估。 一旦发现流量是应用于特定规则，没有进一步的规则进行评估。 NSG 规则可以应用入站或出站方向 （从子网的角度）。

以声明方式，下面的规则用于入站通信正在生成︰

1.  内部 DNS 通信 （端口 53） 被允许。
2.  允许 RDP 通信 （端口 3389） 从互联网到任何虚拟机。
3.  允许 HTTP 通信 （端口 80） 从互联网到 web 服务器 (IIS01)。
4.  允许所有通信 （所有端口） 从 IIS01 到 AppVM1。
5.  拒绝所有通信 （所有端口） 从互联网到整个虚拟网络 （两个子网）。
6.  拒绝任何通信 （所有端口） 子网内前端到后端的子网。

用这些规则绑定到每个子网，如果 HTTP 请求的入站从互联网到 web 服务器上，这两个规则 3 （允许） 和 5 （拒绝） 将应用。 但由于规则 3 有更高的优先级，只会应用，而且规则 5 不会派上用场。 因此对 web 服务器允许 HTTP 请求。 如果试图访问 DNS01 服务器的相同的通信量，规则 5 （拒绝） 将是第一次应用，并且不会允许通信传递到服务器。 规则 6 （拒绝） 阻止前端到后端的子网 （除了 1 和 4 的规则中允许的通信） 对话网。 在攻击者危及前端 web 应用程序的情况下，这会保护后端网络。 攻击者将大大限制访问后端"受保护"网络 （仅对公开在 AppVM01 服务器上的资源）。

没有默认出站规则，允许通信流出到互联网。 对于本示例，我们要允许出站通信并不修改任何出站规则。 若要锁定在两个方向的通信，用户定义路由是必需的 （请参见示例 3）。

#### <a name="conclusion"></a>结论
这种相对简单明了的隔离的入站通信从后端的子网。 有关详细信息，请参阅[生成详细的说明][Example1]。 这些指令包括︰

- 如何构建 PowerShell 脚本与外围网络。
- 如何构建 Azure 资源管理器模板与外围网络。
- NSG 的每个命令的详细的说明。
- 详细的通信流方案，显示如何允许或拒绝每个图层中的通信。


 ### <a name="example-2-build-a-perimeter-network-to-help-protect-applications-with-a-firewall-and-nsgs"></a>示例 2︰ 生成一个外围网络来帮助保护防火墙和 NSGs 的应用程序
[快速起步回](#fast-start) | [详细生成此示例的说明][Example2]

![入站 NVA，NSG 与外围网络][8]

#### <a name="environment-description"></a>环境的描述
在此示例中，没有订阅包含以下︰

- 两个云服务:"FrontEnd001"和"BackEnd001"
- 使用的虚拟网络，"CorpNetwork"两个子网:"前端"和"后端"
- 应用于两个子网的网络安全组
- 网络虚拟应用装置，在此种情况下是防火墙，连接到前端的子网
- 一台 Windows 服务器表示应用程序 web 服务器 ("IIS01")
- 表示应用程序的后端服务器 （"AppVM01"、"AppVM02"） 的两个 Windows 服务器
- 一台 Windows 服务器，表示 DNS 服务器 ("DNS01")

有关脚本和 Azure 资源管理器模板，请参阅[生成详细的说明][Example2]。

#### <a name="nsg-description"></a>NSG 说明
在此示例中，NSG 组生成和再加载的六个规则。

>[AZURE.TIP] 通常情况下，您应该创建您具体的"允许"规则首先，跟更具一般性的"拒绝"规则。 给定的优先级将决定哪些规则都应首先评估。 一旦发现流量是应用于特定规则，没有进一步的规则进行评估。 NSG 规则可以应用入站或出站方向 （从子网的角度）。

以声明方式，下面的规则用于入站通信正在生成︰

1.  内部 DNS 通信 （端口 53） 被允许。
2.  允许 RDP 通信 （端口 3389） 从互联网到任何虚拟机。
3.  允许对网络虚拟应用装置 （防火墙） 任何 Internet 通信 （所有的端口）。
4.  允许所有通信 （所有端口） 从 IIS01 到 AppVM1。
5.  拒绝所有通信 （所有端口） 从互联网到整个虚拟网络 （两个子网）。
6.  拒绝任何通信 （所有端口） 子网内前端到后端的子网。

用这些规则绑定到每个子网，如果 HTTP 请求的入站从互联网到防火墙上，这两个规则 3 （允许） 和 5 （拒绝） 将应用。 但由于规则 3 有更高的优先级，只会应用，而且规则 5 不会派上用场。 因此对防火墙允许 HTTP 请求。 如果该同一通讯试图到达 IIS01 服务器上，即使是在前端的子网上，则规则 5 （拒绝） 都将应用，不会允许通信传递到服务器。 规则 6 （拒绝） 阻止前端到后端的子网 （除了 1 和 4 的规则中允许的通信） 对话网。 在攻击者危及前端 web 应用程序的情况下，这会保护后端网络。 攻击者将大大限制访问后端"受保护"网络 （仅对公开在 AppVM01 服务器上的资源）。

没有默认出站规则，允许通信流出到互联网。 对于本示例，我们要允许出站通信并不修改任何出站规则。 若要锁定在两个方向的通信，用户定义路由是必需的 （请参见示例 3）。

#### <a name="firewall-rule-description"></a>防火墙规则说明
在防火墙上转发规则应创建。 由于本示例只路由绑定到防火墙的 Internet 通讯和只有一个转发网络地址转换 (NAT) 再到 web 服务器，需要规则。

转发规则接受命中目标受众 HTTP （端口 80 或 443 的 HTTPS） 防火墙任何传入的源地址。 它有防火墙的本地接口发送并被重定向到 web 服务器的 IP 地址为 10.0.1.5。

#### <a name="conclusion"></a>结论
这种方法相对简单的防火墙的应用程序保护和隔离的入站通信从后端的子网。 有关详细信息，请参阅[生成详细的说明][Example2]。 这些指令包括︰

- 如何构建 PowerShell 脚本与外围网络。
- 如何构建此示例使用 Azure 资源管理器模板。
- 每个 NSG 命令和防火墙规则的详细的说明。
- 详细的通信流方案，显示如何允许或拒绝每个图层中的通信。

### <a name="example-3-build-a-perimeter-network-to-help-protect-networks-with-a-firewall-udr-and-nsg"></a>示例 3︰ 构建一个外围网络来帮助保护防火墙、 UDR 和 NSG 的网络
[快速起步回](#fast-start) | [详细生成此示例的说明][Example3]

![与 NVA，NSG，UDR 双向的外围网络][9]

#### <a name="environment-description"></a>环境的描述
在此示例中，没有订阅包含以下︰

- 三种云服务:"SecSvc001"、"FrontEnd001"和"BackEnd001"
- 使用的虚拟网络，"CorpNetwork"三个子网:"SecNet"，"前端"和"后端"
- 网络虚拟应用装置，在此种情况下是防火墙，连接到 SecNet 的子网
- 一台 Windows 服务器表示应用程序 web 服务器 ("IIS01")
- 表示应用程序的后端服务器 （"AppVM01"、"AppVM02"） 的两个 Windows 服务器
- 一台 Windows 服务器，表示 DNS 服务器 ("DNS01")

有关脚本和 Azure 资源管理器模板，请参阅[生成详细的说明][Example3]。

#### <a name="udr-description"></a>UDR 说明
默认情况下，下列系统路由的定义如下︰

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.0.0/16}     VNETLocal                            Active   Default    
         {0.0.0.0/0}       Internet                             Active   Default    
         {10.0.0.0/8}      Null                                 Active   Default    
         {100.64.0.0/10}   Null                                 Active   Default    
         {172.16.0.0/12}   Null                                 Active   Default    
         {192.168.0.0/16}  Null                                 Active   Default

VNETLocal 始终是该特定网络的虚拟网络的定义的地址 prefix(es) （即变为从虚拟网络虚拟网络，这取决于如何定义每个特定的虚拟网络）。 其余的系统路由是静态的作为表中指定默认值。

在此示例中，两个路由表创建了，分别的前端和后端的子网。 用适合于给定的子网的静态路由加载的每个表。 在此示例中，每个表已将所有通信 (0.0.0.0/0) 的三种路由通过防火墙 (下一跃点 = 虚拟装置 IP 地址):

1. 有没有下一个跃点定义为允许本地子网流量绕过防火墙的本地子网流量。
2. 虚拟的网络通信与下一个跃点定义为防火墙;这将覆盖允许本地虚拟的网络流量直接路由的默认规则。
3. 与下一个跃点定义为防火墙的所有剩余流量 (0/0)。

>[AZURE.TIP] 没有本地子网条目中的 UDR 会破坏本地子网通信。 
> - 在我们的示例中，指向 VNETLocal 的 10.0.1.0/24 是与其他数据包离开另一个本地服务器 （例如） 10.0.1.25 发往 Web 服务器 (10.0.1.4) 将会失败，因为他们将会通过发送到 NVA，会将其发送到子网，重要的并网将重新将其发送给 NVA，等等。
> - 路由循环的可能性是与它们进行通信，这通常是传统，内部，装置的每个子网直接相连的多个 nic 装置通常较高。 

一旦创建了路由表，则将它们绑定到它们的子网中。 前端的子网路由表，一次创建和绑定到子网中，如下所示︰

        Effective routes : 
         Address Prefix    Next hop type    Next hop IP address Status   Source     
         --------------    -------------    ------------------- ------   ------     
         {10.0.1.0/24}     VNETLocal                            Active 
         {10.0.0.0/16}     VirtualAppliance 10.0.0.4            Active    
         {0.0.0.0/0}       VirtualAppliance 10.0.0.4            Active

>[AZURE.NOTE] 现在，UDR 可以应用于 ExpressRoute 电路连接的网关子网中。
>
> 有关如何启用外围网络与 ExpressRoute 或站点到站点网络的示例 3 和 4 的示例所示。


#### <a name="ip-forwarding-description"></a>IP 转发的说明
IP 转发是 UDR 助理功能。 这是设置虚拟装置，使其接收通信不是专门到装置，然后，将通信转发到其最终目标。

例如，如果通信量从 AppVM01 到 DNS01 服务器发出请求时，UDR 将路由到防火墙。 使用 IP 转发启用，DNS01 目标 (10.0.2.4) 接受装置 (10.0.0.4) 和通信然后转发到其最终目的地 (10.0.2.4)。 而 IP 转发在防火墙上启用，通信量将不会被接受装置即使路由表已为下一个跃点的防火墙也是如此。 若要使用虚拟应用装置，则关键要记住要配合 UDR 启用 IP 转发。

#### <a name="nsg-description"></a>NSG 说明
在此示例中，NSG 组生成和再加载有一条规则。 该组然后绑定仅对前端和后端的子网 (未 SecNet)。 以声明方式为其生成下面的规则︰

- 拒绝所有通信 （所有端口） 从互联网到整个虚拟网络 （所有子网）。

虽然在本示例中使用 NSGs，它的主要用途是防御的作为辅助图层的手动错误配置。 目的是要阻止所有的入站的通信从互联网到前端或后端的网。 通信应只流过 SecNet 网防火墙 (然后，在适当的情况下，到前端或后端的子网)。 另外，UDR 规则的地方，在任何未列入的前端或后端的子网的通讯将会转向出 （感谢 UDR) 防火墙。 防火墙将看到为非对称流，并将丢弃的出站通信。 因此，有三层安全保护子网︰
- 没有打开的终结点上的 FrontEnd001 和 BackEnd001 云服务。
- NSGs 拒绝从互联网的通信量。
- 防火墙丢弃非对称通信流。

有关在此示例中 NSG 的一个关注点是，它包含一个规则，是拒绝为整个虚拟网络，包括安全子网的互联网通信。 但是，由于 NSG 仅绑定到的前端和后端的子网时，规则不处理通信量站到安全子网。 因此，通信将流向安全子网。

#### <a name="firewall-rules"></a>防火墙规则
在防火墙上转发规则应创建。 由于防火墙阻塞或转发所有入站、 出站和内部虚拟网络通信，则需要多个防火墙规则。 此外，所有入站的通信将会冲击安全服务公共 IP 地址 （在不同的端口），由防火墙进行处理。 最佳做法是图之前设置子网的逻辑流和防火墙规则，以避免以后返工。 下图是此示例的防火墙规则的逻辑视图︰
 
![防火墙规则的逻辑视图][10]

>[AZURE.NOTE] 根据使用网络的虚拟设备，管理端口将会有所不同。 在此示例中，Barracuda NextGen 防火墙引用时，它使用端口 22、 801 和 807。 请参阅装置供应商文档，以查找确切端口用于正在使用的设备的管理。

#### <a name="firewall-rules-description"></a>防火墙规则说明
在前面的逻辑图，不会显示安全子网。 这是因为防火墙是该子网上的唯一的资源，此图显示防火墙规则和如何在逻辑上允许或拒绝通信量，而不是实际的路由路径。 同时，选择 RDP 通信量较高的外部端口范围是端口 (8014 – 8026)，并且选择以某种程度上与可读性的本地 IP 地址的最后两个八位字节对齐 （例如，本地服务器地址 10.0.1.4 是与外部端口 8014 相关联）。 但是，任何更高的非冲突端口，无法使用。

在本例中，我们需要七种类型的规则︰

- （对于入站通信） 的外部规则︰
  1.    防火墙管理规则︰ 此应用程序重定向规则允许通信传送到管理端口的网络虚拟应用装置。
  2.    （对于每个 Windows 服务器） 的 RDP 规则︰ 这四条规则 （一个用于每台服务器） 允许通过 RDP 的各个服务器的管理。 这还可以捆绑到一个规则，具体取决于正在使用的虚拟网络设备的能力。
  3.    应用程序的通信规则︰ 这些规则，为前端的 web 通信，第一和第二个后端通信 （例如，web 服务器与数据层） 两种。 这些规则的配置取决于网络体系结构 （服务器放置的位置） 和通信流 （使用通信流和哪些端口的方向）。
      - 第一个规则允许访问应用程序服务器的实际的应用程序通信。 而其他规则允许安全和管理，应用程序的通信规则是什么允许外部用户或服务访问应用程序。 对于本示例中，没有单个的 web 服务器在端口 80 上。 因此一个单一防火墙应用程序规则重定向到外部 IP，给 web 服务器的内部 IP 地址的入站的通信。 重定向的通信会话将通过 NAT 转换为内部服务器。
      - 第二条规则是通过任何端口后端规则来允许 web 服务器可以与 AppVM01 服务器 （而不是 AppVM02）。
- 内部规则 （用于内部虚拟网络通信）
  4.    出站 Internet 规则︰ 该规则允许从任何网络要传递给所选的网络流量。 此规则通常是默认的规则已在防火墙上，但处于禁用状态。 为了使本示例，应启用此规则。
  5.    DNS 规则︰ 该规则仅允许 DNS （端口 53） 通信传递到 DNS 服务器。 针对此环境从前端到后端的多数通信将被阻止。 此规则明确允许从任何本地子网的 DNS。
  6.    子网子网规则︰ 该规则是允许任何服务器子网上的后端连接到前端的子网 （但反过来则不可行） 上的任何服务器。
- （适用于不符合任何以前的通信） 的故障保护规则︰
  7.    拒绝所有通信规则︰ 这应该始终是最终规则 （按优先级），并且这种情况下如果通信流不匹配的任何上述规则它将丢弃该规则。 这是默认规则，通常激活。 通常不需要不作任何修改。

>[AZURE.TIP] 在第二个应用程序的通信规则，以简化本示例，任意端口被允许。 在实际方案中，最特定的端口和地址范围用于减少此规则的受攻击面。

一旦创建了所有以前的规则，务必检查以确保通信将被允许或拒绝所需的每个规则的优先级。 对于此示例，规则的优先级顺序。

#### <a name="conclusion"></a>结论
这是保护和隔离网络比前面的示例更复杂，但更完整的方法。 （示例 2 保护只是应用程序和示例 1 只隔离子网）。 这种设计使得监视双向的通信，并保护不只是入站的应用程序服务器但强制所有服务器在此网络上的网络安全策略。 此外，具体取决于使用该装置，完整的通信审核和意识可以实现。 有关详细信息，请参阅[生成详细的说明][Example3]。 这些指令包括︰

- 如何构建此示例外围网络使用 PowerShell 的脚本。
- 如何构建此示例使用 Azure 资源管理器模板。
- 详细说明每个 UDR，NSG 的命令和防火墙规则。
- 详细的通信流方案，显示如何允许或拒绝每个图层中的通信。

### <a name="example-4-add-a-hybrid-connection-with-a-site-to-site-virtual-appliance-virtual-private-network-vpn"></a>示例 4︰ 添加站点对站点的虚拟装置虚拟专用网络 (VPN) 的混合连接
[返回到快速起步](#fast-start)|详细生成说明可用的很快

![连接 NVA 外围网络的混合网络][11]

#### <a name="environment-description"></a>环境的描述
混合网络连接使用的网络虚拟装置 (NVA) 可以添加到任何外围网络类型 1、 2 或 3 的示例中所述。

如上图所示，用于内部网络连接到 Azure 的虚拟网络，通过 NVA VPN 连接通过 Internet （站点）。

>[AZURE.NOTE] 如果您启用了 Azure 公共对等的选项使用 ExpressRoute，应创建一个静态路由。 这应传送给出您公司的互联网并不是通过 ExpressRoute WAN NVA VPN IP 地址。 需要在 ExpressRoute Azure 公共对等的选项上的 NAT 可以断开 VPN 会话。

将 VPN 后，NVA 成为中心点，用于所有网络和子网。 防火墙转发规则确定允许哪个通信流，通过 NAT 转换，将被重定向或丢弃 （即使对于 Azure 与内部网络之间的通信流）。

通信流应慎重考虑，他们可以优化或降级的这种设计模式，这取决于特定用例。

使用环境构建在示例 3 中，然后再添加站点到站点 VPN 混合网络连接，将产生以下设计︰

![使用站点到站点 VPN 连接与 NVA 外围网络][12]

本地路由器，或者 vpn，您 NVA 与兼容的任何其他网络设备将 VPN 客户端。 该物理设备将负责初始化和维护与您 NVA 的 VPN 连接。

逻辑上到 NVA，网络看起来像四个单独"的安全区域"的规则 NVA，在这些区域之间的流量的主控制器上︰

![从 NVA 角度的逻辑网络][13]

#### <a name="conclusion"></a>结论
添加的站点到站点 VPN 混合网络连接到 Azure 的虚拟网络可以将内部网络扩展到 Azure，以安全的方式。 在使用 VPN 连接，您的通信加密的路由通过互联网。 在此示例中 NVA 提供一个集中的位置，来实施和管理安全策略。 有关详细信息，请参阅详细的生成指令 （即将发表）。 这些指令包括︰

- 如何构建此示例外围网络使用 PowerShell 的脚本。
- 如何构建此示例使用 Azure 资源管理器模板。
- 详细的通信流方案，显示通信量如何流动这种设计。

### <a name="example-5-add-a-hybrid-connection-with-a-site-to-site-azure-gateway-vpn"></a>示例 5︰ 添加 VPN 站点到站点，Azure 关与混合连接
[返回到快速起步](#fast-start)|详细生成说明可用的很快

![外围网络与网关连接的混合网络][14]

#### <a name="environment-description"></a>环境的描述
混合网络使用 Azure VPN 网关可以添加到示例 1 或 2 所述任何一种外围网络类型。

前面的图中所示，通过互联网 （网站） 的 VPN 连接用于内部网络连接到 Azure 的虚拟网络，通过使用 Azure VPN 网关。

>[AZURE.NOTE] 如果您启用了 Azure 公共对等的选项使用 ExpressRoute，应创建一个静态路由。 这应传送给出您公司的互联网并不是通过 ExpressRoute WAN NVA VPN IP 地址。 需要在 ExpressRoute Azure 公共对等的选项上的 NAT 可以断开 VPN 会话。

下图显示在该选项中的两个网络边缘。 上第一条边缘，NVA 和 NSGs 来控制内部 Azure 网络和 Azure 和 Internet 之间的通信流。 第二条边是 Azure VPN 网关，即内部和 Azure 之间完全单独、 独立的网络边缘。

通信流应慎重考虑，他们可以优化或降级的这种设计模式，这取决于特定用例。

使用示例 1 中生成的环境，然后添加站点到站点 VPN 混合网络连接，将产生以下设计︰

![使用网关连接使用 ExpressRoute 连接到外围网络][15]

#### <a name="conclusion"></a>结论
添加的站点到站点 VPN 混合网络连接到 Azure 的虚拟网络可以将内部网络扩展到 Azure，以安全的方式。 使用本机 Azure VPN 网关，您的流量是 IPSec 加密，通过互联网传送。 此外，使用 Azure VPN 网关可以提供一个较低的成本选项 （任何其他授权成本与第三方 NVAs）。 这是在示例 1 中，最经济没有 NVA 使用的位置。 有关详细信息，请参阅详细的生成指令 （即将发表）。 这些指令包括︰

- 如何构建此示例外围网络使用 PowerShell 的脚本。
- 如何构建此示例使用 Azure 资源管理器模板。
- 详细的通信流方案，显示通信量如何流动这种设计。

### <a name="example-6-add-a-hybrid-connection-with-expressroute"></a>示例 6︰ 添加与 ExpressRoute 的混合连接
[返回到快速起步](#fast-start)|详细生成说明可用的很快

![外围网络与网关连接的混合网络][16]

#### <a name="environment-description"></a>环境的描述
混合网络使用 ExpressRoute 专用对等连接可以添加到示例 1 或 2 所述任何一种外围网络类型。

前面的图中所示，ExpressRoute 专用对等提供了您的内部网络和 Azure 的虚拟网络之间的直接连接。 仅在服务提供商网络和 Microsoft Azure 网络，永远不会接触 Internet 上传输通信。

>[AZURE.NOTE] 使用 ExpressRoute，因为在 Azure 的虚拟网关使用动态路由的复杂性 UDR 时有一定限制。 这些优点如下︰
>
>- UDR 不应应用于网关的子网连接的 ExpressRoute 链接 Azure 虚拟网关。
>- ExpressRoute 链接 Azure 虚拟网关不能用于其他 UDR 的镇定设备绑定子网。
>
>
<br />

>[AZURE.TIP] 使用 ExpressRoute 保留离互联网更好的安全的企业网络流量，并大幅提高性能。 它还允许对服务级别协议 ExpressRoute 供应商提供。 Azure 网关可以传递最多 2 Gb/s 与 ExpressRoute，而与站点到站点 Vpn 网关 Azure 最大吞吐量是 200 Mb/s。

如下面的关系图所示，使用此选项环境现在具有两个网络边缘。 内部和 Azure 之间完全单独、 独立的网络边缘网关时，NVA，NSG 控制内部 Azure 网络和 Azure 和 Internet 之间的通信流。

通信流应慎重考虑，他们可以优化或降级的这种设计模式，这取决于特定用例。

使用环境构建在示例 1，然后再添加一个 ExpressRoute 混合网络连接，将产生以下设计︰

![使用网关连接使用 ExpressRoute 连接到外围网络][17]

#### <a name="conclusion"></a>结论
ExpressRoute 专用 Peering 网络连接添加可以将内部网络扩展到 Azure，在安全、 低延迟、 高执行的方式。 此外，使用本机 Azure 网关，如本示例中，提供一个较低的成本选项 （无需额外授权与第三方 NVAs）。 有关详细信息，请参阅详细的生成指令 （即将发表）。 这些指令包括︰

- 如何构建此示例外围网络使用 PowerShell 的脚本。
- 如何构建此示例使用 Azure 资源管理器模板。
- 详细的通信流方案，显示通信量如何流动这种设计。

## <a name="references"></a>引用
### <a name="helpful-websites-and-documentation"></a>有用的网站和文档
- 访问 Azure 使用 Azure 资源管理器︰
- 访问使用 PowerShell Azure: [https://azure.microsoft.com/documentation/articles/powershell-install-configure/](./powershell-install-configure.md)
- 网络的虚拟文档︰ [https://azure.microsoft.com/documentation/services/virtual-network/](https://azure.microsoft.com/documentation/services/virtual-network/)
- 网络安全组文档︰ [https://azure.microsoft.com/documentation/articles/virtual-networks-nsg/](./virtual-network/virtual-networks-nsg.md)
- 用户定义的路由文档︰ [https://azure.microsoft.com/documentation/articles/virtual-networks-udr-overview/](./virtual-network/virtual-networks-udr-overview.md)
- Azure 的虚拟网关︰ [https://azure.microsoft.com/documentation/services/vpn-gateway/](https://azure.microsoft.com/documentation/services/vpn-gateway/)
- 站点到站点 Vpn: [https://azure.microsoft.com/documentation/articles/vpn-gateway-create-site-to-site-rm-powershell](./vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)
- （请务必检查出的"快速入门"和"如何"部分） 的 ExpressRoute 文档︰ [https://azure.microsoft.com/documentation/services/expressroute/](https://azure.microsoft.com/documentation/services/expressroute/)

<!--Image References-->
[0]: ./media/best-practices-network-security/flowchart.png "安全选项流程图"
[1]: ./media/best-practices-network-security/compliancebadges.png "Azure 的法规遵从性徽章"
[2]: ./media/best-practices-network-security/azuresecurityfeatures.png "Azure 的安全功能"
[3]: ./media/best-practices-network-security/dmzcorporate.png "企业网络中的 DMZ"
[4]: ./media/best-practices-network-security/azuresecurityarchitecture.png "Azure 的安全体系结构"
[5]: ./media/best-practices-network-security/dmzazure.png "DMZ 中 Azure 的虚拟网络"
[6]: ./media/best-practices-network-security/dmzhybrid.png "有三种安全边界混合网络"
[7]: ./media/best-practices-network-security/example1design.png "入站的 DMZ NSG"
[8]: ./media/best-practices-network-security/example2design.png "入站的 DMZ NVA，NSG"
[9]: ./media/best-practices-network-security/example3design.png "双向的 DMZ NVA，NSG，UDR"
[10]: ./media/best-practices-network-security/example3firewalllogical.png "防火墙规则的逻辑视图"
[11]: ./media/best-practices-network-security/example4designoptions.png "DMZ NVA 连接混合网络"
[12]: ./media/best-practices-network-security/example4designs2s.png "DMZ NVA 使用站点到站点 VPN 连接"
[13]: ./media/best-practices-network-security/example4networklogical.png "从 NVA 角度的逻辑网络"
[14]: ./media/best-practices-network-security/example5designoptions.png "DMZ 与 Azure 的网关连接站点的混合网络"
[15]: ./media/best-practices-network-security/example5designs2s.png "DMZ 与 Azure 使用 VPN 站点到站点的网关"
[16]: ./media/best-practices-network-security/example6designoptions.png "DMZ 与 Azure 的网关连接 ExpressRoute 混合网络"
[17]: ./media/best-practices-network-security/example6designexpressroute.png "DMZ 与 Azure 使用 ExpressRoute 连接的网关"

<!--Link References-->
[Example1]: ./virtual-network/virtual-networks-dmz-nsg-asm.md
[Example2]: ./virtual-network/virtual-networks-dmz-nsg-fw-asm.md
[Example3]: ./virtual-network/virtual-networks-dmz-nsg-fw-udr-asm.md
[Example4]: ./virtual-network/virtual-networks-hybrid-s2s-nva-asm.md
[Example5]: ./virtual-network/virtual-networks-hybrid-s2s-agw-asm.md
[Example6]: ./virtual-network/virtual-networks-hybrid-expressroute-asm.md
[Example7]: ./virtual-network/virtual-networks-vnet2vnet-direct-asm.md
[Example8]: ./virtual-network/virtual-networks-vnet2vnet-transit-asm.md
