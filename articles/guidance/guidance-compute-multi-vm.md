<properties
   pageTitle="运行多个虚拟机 |参考体系结构 |Microsoft Azure"
   description="如何为可扩展性、 灵活性、 可管理性和安全在 Azure 上运行多个虚拟机实例。"
   services=""
   documentationCenter="na"
   authors="MikeWasson"
   manager="christb"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/19/2016"
   ms.author="mwasson"/>

# <a name="running-multiple-vms-on-azure-for-scalability-and-availability"></a>出于可伸缩性和可用性在 Azure 上运行多个虚拟机 

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

这篇文章概括介绍了一套行之有效的做法，对于运行多个虚拟机 (VM) 实例以提高可扩展性、 可用性、 可管理性和安全性。   

在此体系结构，工作负载分布 VM 实例。 没有单个公用 IP 地址，并且 Internet 通信将被分发到使用负载平衡器的虚拟机。 这种体系结构可以用于一层应用程序中，如无状态的 web 应用程序或存储群集。 它也是一个 N 层应用程序的构造块。 

本文基于[单个 VM 在 Azure 上运行][single vm]。 这篇文章中的建议也适用于这种体系结构。

## <a name="architecture-diagram"></a>体系结构关系图

在 Azure 中的虚拟机需要支持网络和存储资源。

> 包括此体系结构示意图的 Visio 文档是可用于在[Microsoft 下载中心]下载[visio-download]。 此图是在"计算的多个 VM 选项卡。" 

![[0]][0]

该体系结构具有以下组件︰ 

- **可用性设置。** [可用性设置][availability set]包含虚拟机，并有必要支持[SLA Azure 的虚拟机的可用性][vm-sla]。

- **VNet**。 在 Azure 中的每个虚拟机部署到虚拟网络 (VNet) 被进一步分成**子网**。

- **Azure 负载平衡器。** [负载平衡器]分发传入的 Internet 请求到可用性组中的虚拟机实例。 负载平衡器包含一些相关的资源︰

  - **公共 IP 地址。** 负载平衡器接收 Internet 通信需要的公共 IP 地址。

  - **前端配置。** 与负载平衡器将关联的公用 IP 地址。

  - **后端地址池。** 包含虚拟机将接收传入通信的网络接口 (Nic)。

- **负载平衡器规则。** 用于分发的后端地址池中的所有虚拟机的网络通信。 

- **NAT 规则。** 用于将通信路由到特定的虚拟机。 例如，要启用到虚拟机的远程桌面协议 (RDP)，为每个虚拟机创建单独的网络地址转换 (NAT) 规则。 

- **网络接口 (Nic)**。 每个 VM 都有一个 NIC 连接到网络。

- **存储。** 存储帐户中保存 VM 映像和其他与文件相关的资源，例如捕获的 Azure 的 VM 诊断数据。

## <a name="recommendations"></a>建议

Azure 提供许多不同的资源和资源类型中，因此此参考体系结构可以提供许多不同的方法。 我们已经提供了 Azure 资源管理器模板安装的参考体系结构遵循下面列出的建议。 如果您选择创建您自己的参考体系结构应遵循这些建议，除非您有不支持建议的具体要求。 

### <a name="availability-set-recommendations"></a>可用性设置建议

您必须创建至少两个虚拟机中设置为支持[可用性 SLA Azure 的虚拟机]的可用性[vm-sla]。 请注意，Azure 负载平衡器也要求负载平衡 Vm 属于相同的可用性设置。

### <a name="network-recommendations"></a>网络建议

负载平衡器后面的 Vm 应该所有放置在同一子网内。 不公开直接与 Internet 上的 Vm，但改为为每个虚拟机提供专用的 IP 地址。 使用负载平衡器的公用 IP 地址的客户端连接。

### <a name="load-balancer-recommendations"></a>负载平衡器的建议

添加所有的虚拟机中设置为负载平衡器的后端地址池的可用性。

定义负载平衡器规则对 Vm 的直接网络通信。 例如，要启用 HTTP 通信量，请创建映射端口 80 从前端配置端口 80 上的后端地址池的规则。 当负载平衡器收到请求的公用 IP 地址的端口 80 上时，它会将请求路由到端口 80 上的某个后端地址池中 Nic。

定义通信路由到特定 VM 的 NAT 规则。 例如，要启用 RDP 对 Vm 创建单独的 NAT 规则为每个虚拟机。 每个规则应该映射到端口 3389，RDP 的默认端口不同的端口号。 （例如，使用"VM1"，"VM2，"端口 50002 端口 50001 等等。）将 NAT 规则分配给 Vm 上的 Nic。 

### <a name="storage-account-recommendations"></a>存储帐户的建议

创建用于存放虚拟硬盘 (Vhd)，以避免碰到每第二个[(IOPS) 限制]的输入/输出操作每台虚拟机独立 Azure 存储帐户[vm-disk-limits]的存储帐户。 

创建一个用于诊断日志的存储帐户。 所有虚拟机可以共享此存储帐户。

## <a name="scalability-considerations"></a>可伸缩性注意事项

有两个选项用于扩容 Azure 中的虚拟机︰ 

- 使用负载平衡器来分散一组虚拟机的网络流量。 若要向外扩展，调配更多虚拟机，将它们放在负载平衡器后面。 

- 使用[虚拟机比例设置][vmss]。 缩放设置包含指定多个负载平衡器后面的相同虚拟机。 VM 比例设置支持根据性能指标的自动缩放。 随着虚拟机的负载的增加，更多虚拟机将自动添加到负载平衡器。 

下一节比较了这两个选项。

### <a name="load-balancer-without-vm-scale-sets"></a>而 VM 扩展集的负载平衡器

负载平衡器将传入的网络请求，并将其分发在后端地址池中的 Nic。 水平缩放，可用性集中添加更多虚拟机实例 （或取消分配虚拟机可以缩小）。 

例如，假设您正在运行一个 web 服务器。 将负载平衡器规则的端口 80 和/或端口 443 （对于 SSL)。 当客户端发送的 HTTP 请求时，负载平衡器将挑选一个后端的 IP 地址使用[哈希算法][ load balancer hashing] ，其中包含源 IP 地址。 以这种方式，客户端请求被分发到所有虚拟机中。 

> [AZURE.TIP] 将新的虚拟机添加到可用性设置，确保为虚拟机，创建一个 NIC 并且将网卡添加到负载平衡器的后端地址池。 否则，互联网通信不会传送到新的虚拟机。

每个 Azure 预订已到位，包括虚拟机的最大数目，每个地区的默认限制。 您可以归档到一个支持请求增加限制。 有关详细信息，请参阅[Azure 订阅和服务限制、 配额和约束][subscription-limits]。  

### <a name="vm-scale-sets"></a>VM 扩展集 

VM 比例设置帮助您部署和管理一套相同的虚拟机。 所有虚拟机配置相同，VM 规模集没有预配置虚拟机，以便更容易构建大规模面向大计算、 大数据和容器化工作负载的服务支持真正的自动缩放。 

VM 扩展集的详细信息，请参阅[虚拟机比例设置概述][vmss]。

使用 VM 扩展集的注意事项︰

- 如果您需要快速扩展虚拟机，或者需要自动缩放比例，请考虑缩放设置。 

- 目前，规模集不支持数据磁盘。 用于存储数据的选项是 Azure 的文件存储、 操作系统驱动器、 Temp 驱动器中或外部存储，如 Azure 存储。 

- 自动设置范围内的所有虚拟机实例属于同一可用性组，使用 5 故障域和 5 更新域。

- 默认情况下，缩放设置使用"超量配置，"这意味着规模集最初规定比您寻求的更多 Vm，然后删除额外的虚拟机。 当虚拟机的资源调配，这提高了总体成功率。 

- 建议更多然后比 20 每个存储的虚拟机与超量启用，或不能超过 40 个 Vm 配置与超量配置帐户被禁用。  

- 部署范围设置[Azure 快速启动模板]中，您可以找到资源管理器模板[vmss-quickstart]。

- 有两种基本方法来配置比例集中部署的虚拟机︰ 创建自定义图像，或用于扩展配置 VM 后其配置。

    - 基于自定义图像刻度组必须创建一个存储帐户内的所有操作系统磁盘 Vhd。 

    - 使用自定义图像，则需要保留最新的映像。

    - 扩展后，可能需要更长的时间来加速新调配 vm。

其他事项，请参阅[设计 VM 比例设置为小数位数][vmss-design]。

> [AZURE.TIP]  当使用自动缩放的任何解决方案，可提前与生产级工作负载测试。 

## <a name="availability-considerations"></a>可用性考虑事项

可用性组使您的应用程序更具弹性的计划内和计划外维护事件。

- _计划维护_Microsoft 更新基础平台，有时会导致虚拟机重新启动时发生。 Azure 使确保可用性组中的虚拟机并不是所有在同一时间重新启动、 保存至少一个其他人都重新启动时运行。

- _计划外的维护_发生硬件故障时。 Azure 将确保可用性组中的虚拟机都提供跨多个服务器机架。 这有助于减少硬件故障的影响，网络中断、 电源中断，等等。

有关详细信息，请参阅[管理虚拟机的可用性][availability set]。 下面的视频也有可用性设置很好地概括︰[如何配置可用性设置扩展虚拟机][availability set ch9]。 

> [AZURE.WARNING]  请确保配置设置当您配置虚拟机的可用性。 目前，没有方法来将一个资源管理器的虚拟机添加到后 VM 配置设置的可用性。

负载平衡器使用[探测运行状况]监视虚拟机实例的可用性。 如果探测器能在超时期限内达到实例，负载平衡器将停止将流量发送到该虚拟机。 负载平衡器将继续探测，但如果 VM 将再次变为可用，负载平衡器将继续将通信发送到该虚拟机。

下面是一些在负载平衡器运行状况探测器上的建议︰

- 探测器可以测试 HTTP 或 TCP。 如果您的虚拟机运行 HTTP 服务器 （IIS，nginx，Node.js app 等） 创建 HTTP 探测器。 否则创建 TCP 探测器。

- 对于 HTTP 探测器，指定 HTTP 端点的路径。 该探测器检查该路径从一个 HTTP 200 响应。 这可以是根路径 （"/"） 或实现一些自定义逻辑以检查运行状况的应用程序的运行状况监视终结点。 终结点必须允许匿名 HTTP 请求。

- 从[已知的]发送探测器[health-probe-ip]168.63.129.16 的 IP 地址。 请确保不要阻止向或从任何防火墙策略或网络安全组 (NSG) 规则中此 IP 通信。

- 使用[健康探测器日志][health probe log]来查看运行状况探测器的状态。 启用了日志记录在 Azure 门户中为每个负载平衡器。 日志写入到 Azure Blob 存储。 日志显示在后端的多少虚拟机未受到由于故障的探测器响应的网络流量。

## <a name="manageability-considerations"></a>可管理性注意事项

具有多个虚拟机，它变得非常重要自动化过程，因此它们是可靠且可重复。 您可以使用[Azure 自动化][azure-automation]自动部署、 修补操作系统，以及其他任务。 Azure 自动化是运行在 Azure，然后基于 Windows PowerShell 的自动化服务。 在 TechNet 上的[Runbook 库]提供了示例自动化脚本。

## <a name="security-considerations"></a>安全注意事项

虚拟网络是 Azure 中的通信隔离边界。 在一个 VNet 中的虚拟机不能直接向不同的 VNet 中的虚拟机进行通信。 在同一个虚拟机 VNet 可以进行通信，除非您创建[网络安全组][ nsg] (NSGs) 来限制通信。 有关详细信息，请参阅[Microsoft 云服务和网络安全][network-security]。

对于传入的 Internet 通讯，负载平衡器规则定义哪些通信量可以到达后端。 但是，负载平衡器规则不支持 IP 添加到白名单，因此，如果您想要白名单某些公用 IP 地址，添加到子网的 NSG。

## <a name="solution-deployment"></a>解决方案部署

为实现这些建议的参考体系结构部署是在 GitHub 上可用。 此参考体系结构包括一个虚拟网络 (VNet)、 网络安全组 (NSG)、 负载平衡器和两个虚拟机 (Vm)。

参考体系结构可以按照下面的说明使用 Windows 或 Linux 虚拟机部署︰ 

1. 用鼠标右键单击下面的按钮，然后选择"在新选项卡中的打开链接"或"在新窗口中的打开链接":  
[![将部署到 Azure](./media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-multi-vm%2Fazuredeploy.json)

2. 一旦在 Azure 的门户已经打开链接，您必须设置的某些输入值︰ 
    - **资源组**名称已定义参数文件中，因此选择**使用现有**然后输入`ra-multi-vm-rg`在文本框中。
    - 从**位置**下拉框中选择的地区。
    - 不允许编辑**模板根 Uri**或**参数根 Uri**的文本框。
    - 选择从下拉列表框， **windows**或**linux**的**操作系统类型**。 
    - 检查的条款和条件，然后单击**我同意的条款和条件如以上所述**复选框。
    - 单击**购买**按钮。

3. 等待部署完成。

4. 参数文件包含硬编码的管理员用户名和密码，并强烈建议您立即更改两个。 单击名为 VM 上`ra-multi-vm1`在 Azure 的门户。 然后，单击**重置密码****支持 + 疑难解答**刀片式服务器中。 在**模式**下拉列表框中，选择**重置密码**，然后选择一个新**的用户名**和**密码**。 单击**更新**按钮以保存新的用户名和密码。 重复 VM 命名为`ra-multi-vm2`。

部署此参考体系结构的其他方法的信息，请参阅自述文件[指南-单个 vm]中的[github-folder]GitHub 文件夹。 

## <a name="next-steps"></a>下一步行动

将负载平衡器后面的几个虚拟机是用于创建多层体系结构构建块。 有关详细信息，请参阅[的 N 层体系结构在 Azure 上运行 Windows 虚拟机][n-tier-windows]和[N 层体系结构在 Azure 上运行 Linux 虚拟机][n-tier-linux]

<!-- Links -->
[availability set]: ../virtual-machines/virtual-machines-windows-manage-availability.md
[availability set ch9]: https://channel9.msdn.com/Series/Microsoft-Azure-Fundamentals-Virtual-Machines/08
[azure-automation]: https://azure.microsoft.com/en-us/documentation/services/automation/
[azure-cli]: ../virtual-machines-command-line-tools.md
[bastion host]: https://en.wikipedia.org/wiki/Bastion_host
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-multi-vm
[health probe log]: ../load-balancer/load-balancer-monitor-log.md
[健康探测器]: ../load-balancer/load-balancer-overview.md#service-monitoring
[health-probe-ip]: ../virtual-network/virtual-networks-nsg.md#special-rules
[负载平衡器]: ../load-balancer/load-balancer-get-started-internet-arm-cli.md
[load balancer hashing]: ../load-balancer/load-balancer-overview.md#hash-based-distribution
[n-tier-linux]: guidance-compute-n-tier-vm-linux.md
[n-tier-windows]: guidance-compute-n-tier-vm.md
[naming conventions]: guidance-naming-conventions.md
[network-security]: ../best-practices-network-security.md
[nsg]: ../virtual-network/virtual-networks-nsg.md
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md 
[Runbook 库]: ../automation/automation-runbook-gallery.md#runbooks-in-runbook-gallery
[single vm]: guidance-compute-single-vm.md
[subscription-limits]: ../azure-subscription-service-limits.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vm-disk-limits]: ../azure-subscription-service-limits.md#virtual-machine-disk-limits
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines/v1_2/
[vmss]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md
[vmss-design]: ../virtual-machine-scale-sets/virtual-machine-scale-sets-design-overview.md
[vmss-quickstart]: https://azure.microsoft.com/documentation/templates/?term=scale+set
[VM-sizes]: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
[0]: ./media/blueprints/compute-multi-vm.png "在 Azure 组成两个虚拟机和负载平衡器设置可用性上的多个 VM 解决方案的体系结构"
