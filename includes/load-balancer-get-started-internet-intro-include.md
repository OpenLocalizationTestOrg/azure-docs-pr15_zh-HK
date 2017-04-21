Azure 负载平衡器-4 层 （TCP、 UDP） 负载平衡器。 通过将传入通信在云服务中正常运行的服务实例或虚拟机负载平衡器集中分布，负载平衡器提供高可用性。 Azure 负载平衡器可以显示在多个端口和 / 或多个 IP 地址，这些服务。

您可以配置为负载平衡器︰

* 通信执行负载平衡传入 Internet 虚拟机 (Vm)。 我们在这种情况下的负载平衡器作为引用[面向 Internet 的负载平衡器](../articles/load-balancer/load-balancer-internet-overview.md)。
* 虚拟网络 (VNet) 中的虚拟机之间，在云服务中的虚拟机之间或内部计算机和跨部署虚拟网络中的虚拟机之间的负载平衡通信。 我们在这种情况下的负载平衡器作为引用[内部负载平衡器 (ILB)](../articles/load-balancer/load-balancer-internal-overview.md)。
* 外部将通信转发到特定的虚拟机实例。
