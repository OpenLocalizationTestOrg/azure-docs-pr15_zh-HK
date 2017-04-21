<properties
   pageTitle="在高可用性部署虚拟设备 |Microsoft Azure"
   description="如何将网络虚拟应用装置部署高可用性。"
   services=""
   documentationCenter="na"
   authors="telmosampaio"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/21/2016"
   ms.author="telmos"/>

# <a name="deploying-virtual-appliances-in-high-availability"></a>在高可用性部署虚拟应用装置

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

这篇文章概括介绍了一套以高可用性方式部署网络虚拟装置 (NVAs) 的做法。 在继续之前，请确保您了解如何[用户定义路由 (UDR)] [udr-overview]和[负载平衡器][ lb-overview] Azure 的工作。

可以使用 Azure 市场不同 NVAs 可用来扩展功能的 Azure 以相同的方式在您的本地数据中心中使用的装置。 下图显示了示例部署的[单个 NVA] [nva-scenario]作为防火墙应用装置。 

![[0]][0]

尽管前面的部署工作，但它引入了单点故障。 如果虚拟设备出现故障，没有通信流。 若要解决该问题，您需要使用多个 NVAs。 但是，还需要其他的设置和资源，以用于根据您的要求。

可以使用以下解决方法之一来部署高可用性 NVA 环境。

|解决方案|优点|注意事项|
|---|---|---|
|与第 7 层虚拟装置的入口|所有节点都都处于活动状态|要求可以终止连接，并使用 SNAT NVA<br/>从互联网和 Azure 的通信需要单独的一组 NVAs<br/>仅用于发出 Azure 之外的通信|
|与第 7 层虚拟装置的入口出口|所有节点都都处于活动状态<br/>能够处理源自 Azure 的通信量 |要求可以终止连接，并使用 SNAT NVA<br/>从互联网和 Azure 的通信需要单独的一组 NVAs|
|PIP UDR 开关|一组 NVAs 的所有通信<br/>可以处理所有通信 （无限制的端口规则）|主动-被动<br/>需要故障转移过程|

## <a name="ingress-with-layer-7-virtual-appliances"></a>与第 7 层虚拟装置的入口
NVAs Azure 负载平衡器后面一组可用于提供到 Azure 负载后少量服务器端端口 （如 HTTP 和 HTTPS） 连接。 下图突出显示了如何提供在此方案中 NVA 级别的高可用性。

![[1]][1]

在这种情况下，网络虚拟应用装置使用必须终止所有连接，并将它们传递到工作负载的子网。 工作负载虚拟机 (Vm) 响应 NVA 他们收到的请求，并且未出现问题的通信流。 

## <a name="ingress-egress-with-layer-7-virtual-appliances"></a>与第 7 层虚拟装置的入口出口
可以扩展前面的体系结构并添加另一套 NVAs 处理通信源自 Azure 由 NVAs，如下图所示︰

![[2]][2]

在这种情况下，在 Azure 中发出的所有通信被都路由到一组不同的 NVAs 之间分散负荷内部负载平衡器。 这些 NVAs 直接互联网使用的单个公用 IP 地址的通信。 

## <a name="pip-udr-switch"></a>PIP UDR 开关
您可以避免在主动-被动模式下使用两个 NVAs 创建多个 NVA 堆栈。 在这种情况下，可以切换的公共 IP 地址 (PIP) 和用户定义的路由 (UDRs) 的主动节点停止时。  

![[3]][3]

这种情况下等同于单个 NVA 方案。 唯一的区别是，pip 源和 UDRs 必须更改切换 NVAs 之间的通信。 这些更改可以用手工执行，或也可以自动执行它们。 要自动完成，可以部署到这两个 NVAs 的应用程序活动节点的健康检查。 一旦活动节点出现故障，您的应用程序可以更改 pip 源和 UDRs 链接到被动节点。

可能此解决方案的实现是使用[ZooKeeper] [ zookeeper] NVAs 处理领导者选举 （判断哪些节点活动的节点） 上的守护程序。 一旦选择是领导者，它调用到 Azure REST API，PIP 删除出现故障的节点并将其附加到领先地位。 它还应更改为指向新的领导者的专用 IP 地址 UDRs。

## <a name="next-steps"></a>下一步行动

- 了解如何[实现 Azure 和本地数据中心之间的 DMZ] [dmz-on-prem]使用第 7 层 NVAs。
- 了解如何[实现 Azure 和 Internet 之间的 DMZ] [dmz-internet]使用第 7 层 NVAs。

<!-- links -->
[udr-overview]: ../virtual-network/virtual-networks-udr-overview.md
[lb-overview]: ../load-balancer/load-balancer-overview.md
[zookeeper]: https://zookeeper.apache.org/
[nva-scenario]: ../virtual-network/virtual-network-scenario-udr-gw-nva.md
[dmz-on-prem]: guidance-iaas-ra-secure-vnet-hybrid.md
[dmz-internet]: guidance-iaas-ra-secure-vnet-dmz.md

<!-- images -->
[0]: ./media/guidance-nva-ha/single-nva.png "单一 NVA 体系结构"
[1]: ./media/guidance-nva-ha/l7-ingress.png "第 7 层入口"
[2]: ./media/guidance-nva-ha/l7-ingress-egress.png "图层 7 入口和出口"
[3]: ./media/guidance-nva-ha/active-passive.png "主动-被动群集"