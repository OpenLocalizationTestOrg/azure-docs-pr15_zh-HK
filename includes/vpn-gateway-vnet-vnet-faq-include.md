- 虚拟的网络可以在相同或不同 Azure 地区 （位置）。

- 即使连接在一起，不能在虚拟的网络，跨越云服务或负载平衡终结点。

- 将多个 Azure 的虚拟网络连接在一起不需要任何内部部署 VPN 网关，除非不需要跨内部连接。

- VNet 到 VNet 支持连接的虚拟网络。 它不支持连接的虚拟机或云不在虚拟的网络服务。

- VNet 到 VNet 与 RouteBased （以前称为动态路由） 要求 Azure VPN 网关 VPN 类型。 

- 使用多站点 Vpn，10 （默认中的标准网关） 的最大同时使用虚拟网络连接或 30 （高性能关） VPN 隧道连接到任何一个其他的虚拟网络的虚拟网络 VPN 网关或内部部署的站点。

- 虚拟的网络和内部部署本地网络站点的地址空间必须不重叠。 重叠的地址空间将导致创建的 VNet 到 VNet 连接失败。

- 不支持冗余虚拟网络对之间的隧道。

- 所有 VPN 隧道的虚拟网络都共享上的 Azure VPN 网关和同一 VPN 网关的正常运行时间 SLA Azure 中的可用带宽。

- 在 Microsoft 网络，而不是 Internet 传输 VNet 到 VNet 的通信。

- VNet 到 VNet 通信中的同一个区域内是自由的两个方向;跨地区的 VNet 到 VNet 外出通信负责基于源区域的出站间 VNet 数据传输速率。 请对[定价页](https://azure.microsoft.com/pricing/details/vpn-gateway/)的详细信息，参阅。