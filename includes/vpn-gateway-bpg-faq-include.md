### <a name="is-bgp-supported-on-all-azure-vpn-gateway-skus"></a>BGP 支持 Sku 全部 Azure 的 VPN 网关？

否，Azure**标准**和**高性能**的 VPN 网关支持 BGP。 **基本**SKU 不支持。

### <a name="can-i-use-bgp-with-azure-policy-based-vpn-gateways"></a>可以使用 Azure Policy-Based VPN 网关使用 BGP 吗？

否，BGP 路由基于 VPN 网关支持。

### <a name="can-i-use-private-asns-autonomous-system-numbers"></a>是否可以使用专用的 Asn （自治系统号）？

是，可以为您的内部网络和 Azure 的虚拟网络中使用自己的 Asn 公用或专用的 Asn。

#### <a name="are-there-asns-reserved-by-azure"></a>有保留的 Azure 的 Asn 吗？

是的下面 Asn 通过 Azure 保留供内部和外部的 peerings:

- 公共 Asn: 8075 8076、 12076
- 专用 Asn: 65515 65517、 65518、 65519、 65520

连接到 Azure VPN 网关时，不能为您在部署 VPN 设备指定这些 Asn。

### <a name="can-i-use-the-same-asn-for-both-on-premises-vpn-networks-and-azure-vnets"></a>对于内部部署 VPN 网络和 Azure VNets 可以使用同一个 ASN？

不行，您必须分配不同的 Asn 内部网络和 Azure VNets 之间如果和 BGP 连接它们。 Azure 的 VPN 网关具有默认值的分配，65515 ASN BGP 不适用于跨内部连接启用的。 可以通过分配不同的 ASN 创建 VPN 网关时覆盖此默认设置或网关创建之后更改 ASN。 您将需要分配到相应 Azure 本地网络网关内部 Asn。

### <a name="what-address-prefixes-will-azure-vpn-gateways-advertise-to-me"></a>什么地址前缀将 Azure VPN 网关公布给我？

Azure 的 VPN 网关将公布到内部 BGP 设备的以下路由︰

- 您的 VNet 地址前缀
- 对于每个本地网络网关的地址前缀相连的 Azure VPN 网关
- 连接到 Azure VPN 网关，**除了默认工艺路线或工艺路线重叠的任何 VNet 前缀与**其他 BGP 对等会话从获知的路由。

#### <a name="can-i-advertise-default-route-00000-to-azure-vpn-gateways"></a>我可以公布到 Azure VPN 网关的默认路由 (0.0.0.0/0)？

不是这次。

#### <a name="can-i-advertise-the-exact-prefixes-as-my-virtual-network-prefixes"></a>我可以为我的虚拟网络前缀公布确切的前缀？

否，广告与虚拟网络的任何一个相同的前缀地址前缀将来阻止或筛选 Azure 平台。 但是，可以公布的前缀，则您所拥有的虚拟网络中的一个超集。 例如，虚拟网络可以使用地址空间 10.10.0.0/16，无法公布 10.0.0.0/8。

### <a name="can-i-use-bgp-with-my-vnet-to-vnet-connections"></a>我 VNet 到 VNet 的连接可以使用 BGP？

是的可以跨部署连接和 VNet 到 VNet 的连接使用 BGP。

### <a name="can-i-mix-bgp-with-non-bgp-connections-for-my-azure-vpn-gateways"></a>可我混合使用 BGP 与非 BGP 连接我 Azure VPN 网关？

是的可以混合使用 BGP 和非 BGP 相同 Azure VPN 网关的连接。

### <a name="does-azure-vpn-gateway-support-bgp-transit-routing"></a>Azure VPN 网关支持 BGP 传输路由？

是的 BGP 传输路由支持，与异常 Azure VPN 网关将**不**公布对其他 BGP 对等方的默认路由。 要启用跨多个 Azure VPN 网关路由传输，必须在 VNet 到 VNet 的所有中间连接上启用 BGP。

### <a name="can-i-have-more-than-one-tunnels-between-azure-vpn-gateway-and-my-on-premises-network"></a>可以有多个 Azure VPN 网关和内部网络之间的隧道？

是的您可以建立使用 Azure VPN 网关和内部网络之间的多个 S2S VPN 隧道。 请注意，所有这些隧道将计为您 Azure VPN 网关隧道的总数。 例如，如果您有两个冗余隧道 Azure VPN 网关和内部网络之间，它们将消耗 2 隧道从 Azure VPN 网关 (标准值为 10) 和高性能 30 的总配额。

### <a name="can-i-have-multiple-tunnels-between-two-azure-vnets-with-bgp"></a>我可以有两个带有 BGP 的 Azure VNets 之间的多个隧道吗？

否，不支持冗余虚拟网络对之间的隧道。

### <a name="can-i-use-bgp-for-s2s-vpn-in-an-expressroutes2s-vpn-co-existence-configuration"></a>可以使用 BGP S2S vpn 在 ExpressRoute/S2S VPN 配置中共存？

不是这次。

### <a name="what-address-does-azure-vpn-gateway-use-for-bgp-peer-ip"></a>Azure VPN 网关对 BGP 的对等 IP 确实使用什么地址？

Azure VPN 网关将分配定义的虚拟网络的 GatewaySubnet 范围从单个 IP 地址。 默认情况下，它是第二个范围的最后一个地址。 例如，如果您 GatewaySubnet 10.12.255.0/27，范围从 10.12.255.0 到 10.12.255.31，然后在 Azure VPN 网关的 BGP 对等 IP 地址将 10.12.255.30。 列出的 Azure VPN 网关信息时，可以找到此信息。

### <a name="what-are-the-requirements-for-the-bgp-peer-ip-addresses-on-my-vpn-device"></a>VPN 设备上的 BGP 对等 IP 地址的要求有哪些？

内部 BGP 对等地址**不必须**是相同的 VPN 设备的公用 IP 地址。 为你 BGP 的对等 IP VPN 设备使用不同的 IP 地址。 它可以分配给设备上环回接口的地址。 表示位置对应的本地网关指定此地址。

### <a name="what-should-i-specify-as-my-address-prefixes-for-the-local-network-gateway-when-i-use-bgp"></a>什么应该指定为我地址前缀为本地网络网关使用 BGP 时？

Azure 本地网关指定为内部网络的初始地址前缀。 必须使用 BGP，分配主机前缀 （/ 32 前缀） 的 BGP 对等 IP 地址作为该内部网络的地址空间。 如果你 BGP 的对等 IP 为 10.52.255.254，您应指定为表示此内部网络的本地网络网关 localNetworkAddressSpace 的"10.52.255.254/32"。 这是为了确保 Azure VPN 网关建立的 BGP 会话通过 S2S VPN 隧道。

### <a name="what-should-i-add-to-my-on-premises-vpn-device-for-the-bgp-peering-session"></a>什么应该添加到内部部署 VPN 设备 BGP 对等会话？

应将 Azure BGP 对等 IP 地址的主机路由添加指向 S2S IPsec VPN 隧道 VPN 设备上。 例如，如果 Azure VPN 对等 IP 为"10.12.255.30"，应该 VPN 设备上添加"10.12.255.30"的镇定接口的匹配 IPsec 隧道接口与主机路由。
