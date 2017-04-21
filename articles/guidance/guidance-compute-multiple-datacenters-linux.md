<properties
   pageTitle="在实现高可用性的多个区域中运行 Linux 虚拟机 |参考体系结构 |Microsoft Azure"
   description="如何部署在 Azure 上实现高可用性和可恢复性的多个区域中的虚拟机。"
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="roshar"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/21/2016"
   ms.author="mwasson"/>

# <a name="running-linux-vms-in-multiple-regions-for-high-availability"></a>在实现高可用性的多个区域中运行 Linux 虚拟机

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

> [AZURE.SELECTOR]
- [在实现高可用性的多个区域中运行 Linux 虚拟机](guidance-compute-multiple-datacenters-linux.md)
- [在实现高可用性的多个区域中运行 Windows 虚拟机](guidance-compute-multiple-datacenters.md)

在本文中，我们建议的做法可以在多个 Azure 地区，以实现可用性和强健的灾难恢复基础架构中运行 Linux 虚拟机 (Vm)。

> [AZURE.NOTE] Azure 具有两种不同的部署模型︰[资源管理器][resource groups]和经典。 本文使用资源管理器，Microsoft 建议为新的部署。

多区域体系结构可以提供更高的可用性比部署到单个区域。 如果区域停电影响的主要地区，则可以使用[通信管理器][traffic-manager]为故障切换到辅助区域。 如果应用程序的各个子系统出现故障，还可以帮助这种体系结构。

有几种常规方法可用来实现跨数据中心高可用性︰   
  
- 主动/被动与热备用。 通信进入一个地区其他等待进入待机状态。 辅助区域中的虚拟机在任何时候都已分配并正在运行。

- 主动/被动与冷备用。 直到所需的故障转移不分配相同，但在次区域中的虚拟机。 这种方法的成本低，运行，但一般会再停工期间出现故障的时间。

- 主动/主动。 这两个地区都处于活动状态，并请求进行负载平衡它们之间。 如果一个数据中心不可用，它将采取轮换。

这种体系结构侧重于主动/被动与热备用，流量管理器用于故障转移。 注意，您无法部署少量的虚拟机的热备用，再扩张，根据需要。

## <a name="architecture-diagram"></a>体系结构关系图

在[添加到一个 N 层体系结构在 Azure 上的可靠性](guidance-compute-n-tier-vm-linux.md)中所示的体系结构上构建下面的关系图。 

> 包括此体系结构示意图的 Visio 文档是可用于在[Microsoft 下载中心]下载[visio-download]。 此图是在"计算的多个地区 (Linux) 页。

![[0]][0]

- **主要和次要区域**。 此体系结构使用两个区域，以达到更高的可用性。 一个是主要的地区。 在正常操作中，网络通信被路由到主区域。 但是，如果该按钮不可用，通信路由到辅助区域。

- ** [Azure 流量管理器][traffic-manager]**将传入的请求路由到主区域。 如果该地区变得不可用，通信管理器故障切换到辅助区域。 有关详细信息，请参阅[配置通信量管理器](#configuring-traffic-manager)的部分。

- **资源组**。 创建单独的[资源组][resource groups]的主要区域，辅助区域，并为通讯管理器中。 这使您可以灵活地管理每个区域为单个资源的集合。 例如，可以将一个区域，无需停止另一个重新部署。 [链接的资源组][resource-group-links]，以便您可以运行一个查询，以列出该应用程序的所有资源。

- **VNets**。 创建每个地区的独立 VNet。 请确保地址空间不重叠。

- **Apache 卡桑德拉**部署在数据中心通过 Azure 的地区。 卡桑德拉的数据中心部署 Azure 的不同地区，以实现高可用性。 每个地区，节点具有故障识别机架的方式配置和升级域，在该区域内的恢复能力。

## <a name="recommendations"></a>建议

Azure 提供许多不同的资源和资源类型中，因此此参考体系结构可以提供许多不同的方法。 我们已经提供了 Azure 资源管理器模板安装的参考体系结构遵循这些建议。 如果您选择创建您自己的参考体系结构将按照这些建议，除非有推荐不适合特定的要求。

### <a name="regional-pairing"></a>区域匹配

每个 Azure 地区配备在同一地区内的其他地区。 一般情况下，从相同的区域对 （例如，东亚美国 2 和美国中心） 中选择区域。 这样做的好处包括︰

- 如果没有出现大停电，至少一个地区，不是每一对恢复确定优先级。

- 计划的 Azure 系统更新的推出成对地区按顺序，尽量减少可能的停机时间。

- 对位于相同的地理位置，以满足数据派驻服务要求。

但是，请确保这两个区域支持的所有 Azure 服务所需的应用程序 (请参阅[服务按地区][services-by-region])。 关于区域对的详细信息，请参阅[业务连续性和灾难恢复 (BCDR): Azure 配对地区][regional-pairs]。

### <a name="traffic-manager-configuration"></a>通信管理器配置

时，请考虑以下几点配置通信管理器适合您的方案︰

- **路由选择。** 通信管理器支持几种[路由选择算法][tm-routing]。 本文介绍了这种情况下，使用_优先级_路由 （以前称为_故障转移_路由）。 使用此设置，流量管理器将所有请求都发送到主区域，除非主要地区变得无法访问。 此时，它会自动故障切换到辅助区域。 请参阅[配置故障转移路由方法][tm-configure-failover]。

- **健康探测器。** 通信管理器使用 HTTP （或 HTTPS）[探测器][tm-monitoring]来监控每个区域的可用性。 该探测器检查 HTTP 200 响应的指定 URL 路径。 作为最佳实践，创建报告的应用程序的整体健康状况的终结点并为健康探测器使用该终结点。 否则，该探测器可能会报告"状态良好"的终结点时实际失败的应用程序的关键部分。 有关详细信息，请参阅[健康终结点监视模式][health-endpoint-monitoring-pattern]。

当流量管理器故障转移时，没有一段时间，当客户不能到达该应用程序，它可以是几分钟的时间。 两个因素影响的总持续时间︰

- 健康探测器必须检测到主数据中心变得不可访问。

- DNS 服务器必须更新 IP 地址，这取决于 DNS 生存时间 (TTL) 的缓存的 DNS 记录。 默认 TTL 是 300 秒 （5 分钟），但当创建通信管理器配置文件，您可以配置此值。

有关详细信息，请参阅[有关通信管理器监视][tm-monitoring]。

如果流量管理器故障转移时，我们建议执行手动回切，而不是自动故障恢复。 验证已先正常运行所有的应用程序子系统。 否则，您可以创建在其中应用翻转前后数据中心之间的情况。

默认情况下，流量管理器自动进行故障回复。 若要防止出现这种情况，手动故障转移事件之后降低优先级的主要地区。 例如，假设主要地区优先级别为 1，并且辅助优先级为 2。 故障转移后，设置优先级 3，以防止自动故障回复到的主要地区。 当您准备好切换回来时，更新优先级为 1。

下面的 Azure CLI 命令更新优先级︰

```bat
azure network traffic-manager  endpoint set --resource-group <resource-group> --profile-name <profile>
    --name <traffic-manager-name> --type AzureEndpoints --priority 3
```    

另一种方法来避免触发器是暂时禁用该终结点︰

```bat
azure network traffic-manager  endpoint set --resource-group <resource-group> --profile-name <profile>
    --name <traffic-manager-name> --type AzureEndpoints --status Disabled
```    

根据故障转移的原因，您可能需要为 redploy 区域内的资源。 回切之前, 执行一项操作就绪性测试。 测试应验证等内容︰

- 虚拟机的配置正确。 （安装了所有必需的软件，IIS 是运行，等等）。

- 应用程序的子系统都是健康的。

- 功能测试。 （例如，数据库层是从 web 层。）

### <a name="cassandra-deployment-across-multiple-regions"></a>跨多个地区的卡桑德拉部署

卡桑德拉的数据中心是工作负载的部门︰ 一组相关的节点复制和工作负载划分群集内配置在一起。

我们建议[DataStax 企业][datastax]为生产使用。 在 Azure 中运行 DataStax 的详细信息，请参阅[DataStax Azure 的企业部署指南][cassandra-in-azure]。 以下一般性建议将应用于任何卡桑德拉版。

- 将一个公共 IP 地址分配到每个节点。 这样的群集通信跨地区使用 Azure 主干基础架构，提供低成本的高吞吐量。

- 安全使用的适当的防火墙和 NSG 配置，允许仅与已知主机，包括客户端和其他群集节点通讯的节点。 注意卡桑德拉，为通信、 OpsCenter、 触发，等使用不同的端口。 卡桑德拉在端口用法，请参阅[配置防火墙端口访问][cassandra-ports]。

- 对于所有[客户端的节点]使用 SSL 加密[ssl-client-node][节点到节点]和[ssl-node-node]的通信。

- 在区域中，按照[卡桑德拉的建议](guidance-compute-n-tier-vm-linux.md#cassandra-recommendations)中的准则。

## <a name="availability-considerations"></a>可用性考虑事项

与复杂的 N 层应用程序中，您可能不需要复制整个应用程序中的辅助区域。 相反，您可能只是复制支持业务连续性所需的重要子系统。

通信管理器是系统中的一个可能的故障点。 如果服务失败，则客户端无法访问您的应用程序停机期间。 查看[通信量管理器 SLA][tm-sla]，并确定是否使用单独的流量管理器满足您的业务需求，以实现高可用性。 如果没有，则请考虑添加另一个流量管理解决方案为回切。 如果 Azure 流量管理器服务失败，改变 CNAME 记录在 DNS 中指向其他通信量管理服务。 （必须手动执行此步骤，直到将 DNS 更改传播，您的应用程序将无法使用）。

卡桑德拉的群集故障转移方案，要考虑取决于使用该应用程序，以及使用的复制副本数的一致性级别。 对于一致性级别和卡桑德拉中的用法，请参阅[配置数据的一致性][cassandra-consistency]和[卡桑德拉︰ 都说多少个节点，与仲裁？][cassandra-consistency-usage] 卡桑德拉中数据的可用性取决于所使用的应用程序和复制机制的一致性级别。 中卡桑德拉的复制，请参阅[数据复制在 NoSQL 数据库所述][cassandra-replication]。

## <a name="manageability-considerations"></a>可管理性注意事项

当您更新您的部署时，一个地区一次更新，以减少应用程序中的错误或不正确配置全局失败的可能性。

测试系统对故障的恢复能力。 以下是一些常见的故障情况进行测试︰

- 关闭虚拟机实例。

- 压力资源，例如 CPU 和内存。

- 断开连接延迟的网络。

- 崩溃的进程。

- 过期的证书。

- 模拟的硬件故障。

- 关闭域控制器上的 DNS 服务。

测量的恢复时间，并验证满足您的业务需求。 测试故障模式的组合。

## <a name="next-steps"></a>下一步行动

本系列将重点放在纯粹的云部署。 企业方案中通常需要使用 Azure 的虚拟网络连接内部网络的混合网络。 若要了解如何构建混合网络，请参见[实现一种混合网络体系结构 Azure 和内部部署 VPN][hybrid-vpn]。

<!-- Links -->

[azure-sla]: https://azure.microsoft.com/support/legal/sla/
[cassandra-in-azure]: https://academy.datastax.com/resources/deployment-guide-azure
[cassandra-consistency]: http://docs.datastax.com/en/cassandra/2.0/cassandra/dml/dml_config_consistency_c.html
[cassandra-replication]: http://www.planetcassandra.org/data-replication-in-nosql-databases-explained/
[cassandra-consistency-usage]: https://medium.com/@foundev/cassandra-how-many-nodes-are-talked-to-with-quorum-also-should-i-use-it-98074e75d7d5#.b4pb4alb2
[cassandra-ports]: http://docs.datastax.com/en/latest-dse/datastax_enterprise/sec/secConfFirePort.html
[datastax]: https://www.datastax.com/products/datastax-enterprise
[health-endpoint-monitoring-pattern]: https://msdn.microsoft.com/library/dn589789.aspx
[hybrid-vpn]: guidance-hybrid-network-vpn.md
[regional-pairs]: ../best-practices-availability-paired-regions.md
[resource groups]: ../azure-resource-manager/resource-group-overview.md
[resource-group-links]: ../resource-group-link-resources.md
[services-by-region]: https://azure.microsoft.com/en-us/regions/#services
[ssl-client-node]: http://docs.datastax.com/en/cassandra/2.0/cassandra/security/secureSSLClientToNode_t.html
[ssl-node-node]: http://docs.datastax.com/en/cassandra/2.0/cassandra/security/secureSSLNodeToNode_t.html
[tablediff]: https://msdn.microsoft.com/en-us/library/ms162843.aspx
[tm-configure-failover]: ../traffic-manager/traffic-manager-configure-failover-routing-method.md
[tm-monitoring]: ../traffic-manager/traffic-manager-monitoring.md
[tm-routing]: ../traffic-manager/traffic-manager-routing-methods.md
[tm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/traffic-manager/v1_0/
[traffic-manager]: https://azure.microsoft.com/en-us/services/traffic-manager/
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vnet-dns]: ../virtual-network/virtual-networks-manage-dns-in-vnet.md
[vnet-to-vnet]: ../vpn-gateway/vpn-gateway-vnet-vnet-rm-ps.md
[vpn-gateway]: ../vpn-gateway/vpn-gateway-about-vpngateways.md
[wsfc]: https://msdn.microsoft.com/en-us/library/hh270278.aspx
[0]: ./media/blueprints/compute-multi-dc-linux.png "对于 Azure 的 N 层应用程序的高可用性网络体系结构"
