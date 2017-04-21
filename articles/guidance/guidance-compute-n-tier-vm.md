<properties
   pageTitle="N 层体系结构中运行 Windows 虚拟机 |参考体系结构 |Microsoft Azure"
   description="如何在 Azure，付费特别注意可用性、 安全性、 可伸缩性和可管理性安全上实现多层体系结构。"
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
   ms.date="10/20/2016"
   ms.author="mwasson"/>

# <a name="running-windows-vms-for-an-n-tier-architecture-on-azure"></a>N 层体系结构在 Azure 上运行 Windows 虚拟机

> [AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

> [AZURE.SELECTOR]
- [N 层体系结构在 Azure 上运行 Linux 虚拟机](guidance-compute-n-tier-vm-linux.md)
- [N 层体系结构在 Azure 上运行 Windows 虚拟机](guidance-compute-n-tier-vm.md)

本文概述了 N 层体系结构的应用程序的一套行之有效的做法，为运行 Windows 虚拟机 (Vm)。 它建立在[运行多个虚拟机在 Azure 上的]文章[multi-vm]。

> [AZURE.NOTE] Azure 具有两种不同的部署模型︰[资源管理器][resource-manager-overview]和经典。 本文使用资源管理器，Microsoft 建议为新的部署。

## <a name="architecture-diagram"></a>体系结构关系图

而有的 N 层体系结构的差异。 大多数情况下，这些差异不应为这些建议的目的很重要。 本文假定一个典型的 3 层 web 应用程序︰

- **Web 层。** 处理传入的 HTTP 请求。 此层通过返回响应。

- **业务层。** 实现业务流程和其他系统的功能逻辑。

- **数据库层。** 提供了永久性的数据存储，使用[SQL Server 总是在可用性组][sql-alwayson]的高可用性。

> 包括此体系结构示意图的 Visio 文档是可用于在[Microsoft 下载中心]下载[visio-download]。 此图是在"计算的多层 (Windows) 页。

![[0]][0]

- **可用性设置。** 创建[可用性设置][azure-availability-sets]为每个层，并设置每一层中的至少两个虚拟机。 这种方法需达到[SLA]的可用性[vm-sla]的虚拟机。

- **子网。** 创建一个单独的子网，每个层。 指定地址范围和子网掩码使用[CIDR]表示法。 

- **负载平衡器。** 使用[面向 Internet 的负载平衡器][load-balancer-external]分发传入的 Internet 通讯，web 层和[内部负载平衡器][load-balancer-internal]分发网络通信量从 web 层到业务层。

- **Jumpbox**。 _Jumpbox_，也称为[堡垒主机]，是一个网络管理员用来连接到其他虚拟机上的虚拟机。 Jumpbox 具有允许仅从白名单的公共 IP 地址的远程通讯 NSG。 NSG 应该允许远程桌面 (RDP) 通信。

- **监视**。 监视软件如[Nagios]， [Zabbix]或[Icinga]可使您深入了解响应时间、 虚拟机的正常运行时间和系统的整体运行状况。 在一个放在一个单独的管理子网中的虚拟机上安装监控软件。

- **NSGs**。 使用[网络安全组][ nsg] (NSGs) 来限制在 VNet 内的网络通讯。 例如，在此处所示的 3 层体系结构，数据库层不接受来自 web 前端，只能从业务层和管理子网通信。

- **总在可用性组 SQL Server。** 提供高可用性的数据层，通过启用复制和故障切换。

- **活动目录域服务 (AD DS) 服务器**。 Active Directory 域服务 (AD DS) 存储目录数据并管理用户和域，包括用户登录进程、 身份验证和目录搜索之间的通信。 Active Directory 域控制器正在运行 AD DS 的服务器。 在 Windows 服务器 2016 之前, 总是在可用性组必须加入到域中。 这是因为可用性组都依赖于 Windows 服务器故障转移群集 (WSFC) 技术。 Windows 服务器 2016年介绍创建故障转移群集没有 Active Directory 的能力。 有关详细信息，请参阅[在 Windows 服务器 2016年的故障转移群集中的新][wsfc-whats-new]

## <a name="recommendations"></a>建议

Azure 提供许多不同的资源和资源类型中，因此此参考体系结构可以提供许多不同的方法。 我们已经提供了 Azure 资源管理器模板安装的参考体系结构遵循这些建议。 如果您选择创建您自己的参考体系结构将按照这些建议，除非有推荐不适合特定的要求。

### <a name="vnet--subnets"></a>VNet / 子网

当您创建 VNet 时，为您需要的子网分配足够的地址空间。 指定使用[CIDR]表示法的 VNet 地址范围和子网掩码。 使用标准的[专用 IP 地址块]内地址空间[private-ip-space]，10.0.0.0/8、 172.16.0.0/12，以及 192.168.0.0/16。

以防您需要稍后设置 VNet 和内部网络之间的网关，请选择不重叠与您的内部网络地址范围。 一旦您创建了 VNet，则不能更改的地址范围。

设计与构思中的功能和安全要求的子网。 同一层或角色中的所有虚拟机应归入同一子网，它可以是一种安全边界。 有关设计 VNets 和子网的详细信息，请参阅[规划和设计 Azure 虚拟网络][plan-network]。

为每个子网，CIDR 表示法指定的子网的地址空间。 例如，"10.0.0.0/24"创建 256 个 IP 地址范围。 （虚拟机可以使用 251 个; 五保留。 请参阅[虚拟网络常见问题解答][vnet faq]。)请确保地址范围不会重叠在子网间。

### <a name="network-security-groups"></a>网络安全组

NSG 规则用于限制在各层之间的通信。 例如，在上面所示的 3 层体系结构，web 层不直接与通信数据库层。 若要实施此规则，数据库层应阻止来自 web 层子网的传入通信。  

  1. NSG 的创建并将其关联到数据库层子网。

  2. 添加一个从 VNet 拒绝所有入站的通信的规则。 (使用`VIRTUAL_NETWORK`在规则标记。) 

  3. 添加具有更高的优先级，使业务层子网中的入站的通信的规则。 此规则将重写前面的规则，并使业务层与数据层。

  4. 添加允许从数据库层子网本身内的入站的通信的规则。 此规则允许在数据库层中，所需的数据库复制和故障切换的虚拟机之间的通信。

  5. 添加一个规则，允许 RDP 通信量从 jumpbox 的子网。 此规则可让管理员从 jumpbox 连接到数据库层。

  > [AZURE.NOTE] NSG 具有[默认规则][ nsg-rules] ，允许从 VNet 范围内的任何入站的通信量。 这些规则不能删除，但您可以通过创建优先级更高的规则重写它们。

### <a name="load-balancers"></a>负载平衡器

外部负载平衡器分配给 web 层的互联网流量。 创建该负载平衡器的公用 IP 地址。 请参阅[创建面向 Internet 的负载平衡器][lb-external-create]。

内部负载平衡器分配从 web 层到业务层的网络流量。 为此负载平衡器提供一个专用的 IP 地址，创建前端 IP 配置并将其与业务层的子网关联。 [开始创建内部负载平衡器]，请参阅[lb-internal-create]。

### <a name="sql-server-always-on-availability-groups"></a>SQL Server 总是对可用性组

我们建议[始终在可用性组][sql-alwayson]针对 SQL Server 的高可用性。 总是在可用性组需要一个域控制器。 可用性组中的所有节点都必须在相同的 AD 域。

其他层连接到的数据库[可用性组侦听器]通过[sql-alwayson-listeners]。 监听器使 SQL 客户端连接而无需了解物理的 SQL Server 实例的名称。 访问数据库必须加入到域的虚拟机。 （在此例中，另一层） 的客户端使用 DNS 解析成 IP 地址的侦听器的虚拟网络名称。

SQL Server 总是在上配置，如下所示︰

- 创建一个 Windows 服务器故障转移群集 (WSFC) 群集和 SQL Server 总是在可用性组。 有关详细信息，请参阅[入门始终在可用性组][sql-alwayson-getting-started]。

- 创建一个专用的静态 IP 地址与内部负载平衡器。

- 创建可用性组侦听器，并将该侦听器的 DNS 名称映射到内部负载平衡器的 IP 地址。 

- 负载平衡器为创建规则的 SQL Server 侦听端口 （TCP 端口 1433年，默认情况下）。 负载平衡器规则必须启用_浮动 IP_，也称为直接服务器返回。 这会导致虚拟机直接答复客户端，可以直接连接到的主副本。

    > [AZURE.NOTE] 启用浮动 IP 后，前端端口号必须与负载平衡器规则中的后端端口号相同。

当 SQL 客户端试图连接时，负载平衡器将连接请求路由到当前的主要副本。 如果故障切换到另一个副本，负载平衡器会自动将后续请求路由到新的主副本。 有关详细信息，请参阅[配置负载平衡器，以 SQL 总是在][sql-alwayson-ilb]。

在故障切换时，将关闭现有客户端连接。 故障转移完成后，新的连接将被路由到新的主副本。

如果您的应用程序进行得读取超过写入，可以卸载一些辅助副本为只读查询。 请参阅[使用侦听程序连接到只读的次要副本 （只读的路由）][sql-alwayson-read-only-routing]。

通过[强制执行手动故障切换]测试部署[sql-alwayson-force-failover]。

### <a name="jumpbox"></a>Jumpbox

不允许 RDP 访问从公共 Internet 到虚拟机中运行的应用程序工作负载。 相反，对这些虚拟机的所有 RDP/SSH 访问都必须通过 jumpbox。 管理员登录到 jumpbox，，然后登录到 jumpbox 中的其他虚拟机。 Jumpbox 允许 RDP 通信互联网，而只从已知，列入白名单的 IP 地址。

将 jumpbox 放在相同的 VNet 作为其他 Vm，但单独管理子网中。

创建一个[公用 IP 地址]为 jumpbox。

使用 jumpbox，如标准的 A1 小型的 VM 大小。

配置用于 web 层、 业务层和数据库层子网 NSGs，允许管理 (RDP) 通信通过管理子网。

若要保护 jumpbox，创建 NSG，并将其应用到 jumpbox 的子网。 添加允许 RDP 连接只从公用 IP 地址白名单集中 NSG 规则。

NSG 可以附加到子网或 jumpbox nic。 在这种情况下，我们建议将其附加到 NIC，以便仅为 jumpbox，允许 RDP 通信，即使将其他虚拟机添加到同一子网。

## <a name="availability-considerations"></a>可用性考虑事项

将每个层或 VM 角色放入单独的可用性设置。 不要进入虚拟机从不同的层相同的可用性设置。 

在数据库层中，有多个虚拟机并不会自动转换到高可用数据库。 对于关系数据库，通常需要使用复制和故障切换实现高可用性。 SQL Server 中，我们建议使用[始终在可用性组][sql-alwayson]。 

如果您需要更高的可用性比[Vm 的 Azure SLA] [vm-sla]提供，在两个区域之间复制应用程序和 Azure 流量管理器用于故障转移。 详细信息，请参阅[在多个地区为高可用性运行 Windows 虚拟机][multi-dc]。   

## <a name="security-considerations"></a>安全注意事项

加密存放的数据。 使用[Azure 密钥存储库][azure-key-vault]数据库加密密钥进行管理。 密钥存储库可以将加密密钥存储在硬件安全模块 (Hsm)。 有关详细信息，请参阅[配置 SQL Server 在 Azure Vm 上的 Azure 密钥存储库集成][sql-keyvault]还建议将应用程序的机密信息，如数据库连接字符串，存储在密钥存储库。

请考虑添加网络虚拟应用装置 (NVA) 来创建公用的 Internet 和 Azure 的虚拟网络之间的 DMZ。 NVA 是可以执行的与网络相关的任务，如防火墙、 数据包检查、 审核、 定制路由选择事件，或各种其他操作虚拟设备的通用术语。 有关详细信息，请参见[实现 Azure 和 Internet 之间的 DMZ][dmz]。

## <a name="scalability-considerations"></a>可伸缩性注意事项

负载平衡器将分发到 web 层和业务层的网络通讯。 通过添加新的虚拟机实例进行水平缩放。 请注意，您可以扩展 web 层和业务层独立，根据负载。 为了减少可能引起的需要来维护客户关系的复杂因素中 web 层, 的虚拟机应该是无状态的。 Vm 托管业务逻辑也应该是无状态的。

## <a name="manageability-considerations"></a>可管理性注意事项

通过使用集中式的管理工具如[Azure 自动化]简化管理整个系统的[azure-administration]， [Microsoft 操作管理套件][operations-management-suite]，[厨师的简历][chef]，或[傀儡][puppet]。 这些工具可以整合多个虚拟机提供全面系统的了解从捕获的诊断和健康信息。

## <a name="solution-deployment"></a>解决方案部署

为实现这些建议的参考体系结构的部署出现在[Github]上[github-folder]。 此参考体系结构包括 web 层、 业务层、 数据层，以及 jumpbox VM 和 Active Directory 域控制器。

可以按照下面的说明部署参考体系结构︰ 

1. 单击下面的按钮。  
[!["部署到 Azure"][1]][2]

2. 一旦在 Azure 的门户已经打开链接，输入以下值︰ 
  - **资源组**名称已定义参数文件中，因此选择**新建**，然后输入`ra-ntier-sql-network-rg`在文本框中。
  - 从**位置**下拉框中选择的地区。
  - 不允许编辑**模板根 Uri**或**参数根 Uri**的文本框。
  - 检查的条款和条件，然后单击**我同意的条款和条件如以上所述**复选框。
  - 单击**购买**按钮。

3. 检查 Azure 门户通知消息部署完毕。

4. 单击下面的按钮。  
[!["部署到 Azure"][1]][3]

5. 一旦在 Azure 的门户已经打开链接，输入以下值︰ 
  - **资源组**名称已定义参数文件中，因此选择**使用现有**然后输入`ra-ntier-sql-workload-rg`在文本框中。
  - 从**位置**下拉框中选择的地区。
  - 不允许编辑**模板根 Uri**或**参数根 Uri**的文本框。
  - 检查的条款和条件，然后单击**我同意的条款和条件如以上所述**复选框。
  - 单击**购买**按钮。

6. 检查 Azure 门户通知消息部署完毕。

7. 单击下面的按钮。  
[!["部署到 Azure"][1]][4]

8. 一旦在 Azure 的门户已经打开链接，输入以下值︰ 
  - **资源组**名称已定义参数文件中，因此选择**新建**，然后输入`ra-ntier-sql-network-rg`在文本框中。
  - 从**位置**下拉框中选择的地区。
  - 不允许编辑**模板根 Uri**或**参数根 Uri**的文本框。
  - 检查的条款和条件，然后单击**我同意的条款和条件如以上所述**复选框。
  - 单击**购买**按钮。

9. 检查 Azure 门户通知消息部署完毕。

10. 参数文件包含硬编码管理员用户名和密码，强烈建议您立即更改所有虚拟机上。 在 Azure 门户网站中每个虚拟机上单击然后单击**重设密码****支持 + 疑难解答**刀片式服务器中。 在**模式**下拉列表框中，选择**重置密码**，然后选择一个新**的用户名**和**密码**。 单击**更新**按钮以保存新的用户名和密码。 

部署此参考体系结构的其他方法的信息，请参阅自述文件[指南-单个 vm]中的[github-folder]Github 文件夹。 

## <a name="next-steps"></a>下一步行动

若要实现此参考体系结构的高可用性，我们建议[将部署到多个区域][multi-dc]。

<!-- links -->

[arm-templates]: https://azure.microsoft.com/documentation/articles/resource-group-authoring-templates/
[azure-administration]: ../automation/automation-intro.md
[azure-audit-logs]: ../resource-group-audit.md
[azure-availability-sets]: ../virtual-machines/virtual-machines-windows-manage-availability.md#configure-each-application-tier-into-separate-availability-sets
[azure-cli]: ../virtual-machines-command-line-tools.md
[azure-key-vault]: https://azure.microsoft.com/services/key-vault.md
[azure-load-balancer]: ../load-balancer/load-balancer-overview.md
[堡垒主机]: https://en.wikipedia.org/wiki/Bastion_host
[cidr]: https://en.wikipedia.org/wiki/Classless_Inter-Domain_Routing
[chef]: https://www.chef.io/solutions/azure/
[dmz]: guidance-iaas-ra-secure-vnet-dmz.md
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier-sql
[lb-external-create]: ../load-balancer/load-balancer-get-started-internet-portal.md
[lb-internal-create]: ../load-balancer/load-balancer-get-started-ilb-arm-portal.md
[load-balancer-external]: ../load-balancer/load-balancer-internet-overview.md
[load-balancer-internal]: ../load-balancer/load-balancer-internal-overview.md
[multi-dc]: guidance-compute-multiple-datacenters.md
[multi-vm]: guidance-compute-multi-vm.md
[n-tier]: guidance-compute-n-tier-vm.md
[naming conventions]: guidance-naming-conventions.md
[nsg]: ../virtual-network/virtual-networks-nsg.md
[nsg-rules]: ../best-practices-resource-manager-security.md#network-security-groups
[operations-management-suite]: https://www.microsoft.com/en-us/server-cloud/operations-management-suite/overview.aspx
[plan-network]: ../virtual-network/virtual-network-vnet-plan-design-arm.md
[private-ip-space]: https://en.wikipedia.org/wiki/Private_network#Private_IPv4_address_spaces
[公共 IP 地址]: ../virtual-network/virtual-network-ip-addresses-overview-arm.md
[puppet]: https://puppetlabs.com/blog/managing-azure-virtual-machines-puppet
[resource-manager-overview]: ../azure-resource-manager/resource-group-overview.md
[sql-alwayson]: https://msdn.microsoft.com/en-us/library/hh510230.aspx
[sql-alwayson-force-failover]: https://msdn.microsoft.com/en-us/library/ff877957.aspx
[sql-alwayson-getting-started]: https://msdn.microsoft.com/en-us/library/gg509118.aspx
[sql-alwayson-ilb]: https://blogs.msdn.microsoft.com/igorpag/2016/01/25/configure-an-ilb-listener-for-sql-server-alwayson-availability-groups-in-azure-arm/
[sql-alwayson-listeners]: https://msdn.microsoft.com/en-us/library/hh213417.aspx
[sql-alwayson-read-only-routing]: https://technet.microsoft.com/en-us/library/hh213417.aspx#ConnectToSecondary
[sql-keyvault]: ../virtual-machines/virtual-machines-windows-ps-sql-keyvault.md
[vm-planned-maintenance]: ../virtual-machines/virtual-machines-windows-planned-maintenance.md
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines
[vnet faq]: ../virtual-network/virtual-networks-faq.md
[wsfc-whats-new]: https://technet.microsoft.com/windows-server-docs/failover-clustering/whats-new-in-failover-clustering
[Nagios]: https://www.nagios.org/
[Zabbix]: http://www.zabbix.com/
[Icinga]: http://www.icinga.org/
[VM-sizes]: https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
[solution-script]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/Deploy-ReferenceArchitecture.ps1
[solution-script-bash]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/deploy-reference-architecture.sh
[vnet-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/virtualNetwork.parameters.json
[vnet-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/virtualNetwork.parameters.json
[nsg-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/networkSecurityGroups.parameters.json
[nsg-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/networkSecurityGroups.parameters.json
[webtier-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/webTier.parameters.json
[webtier-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/webTier.parameters.json
[businesstier-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/businessTier.parameters.json
[businesstier-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/businessTier.parameters.json
[datatier-parameters-windows]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/windows/dataTier.parameters.json
[datatier-parameters-linux]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-n-tier/parameters/linux/dataTier.parameters.json
[azure-powershell-download]: https://azure.microsoft.com/documentation/articles/powershell-install-configure/
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[0]: ./media/blueprints/compute-n-tier.png "使用 Microsoft Azure 的 N 层体系结构"
[1]: ./media/blueprints/deploybutton.png 
[2]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-n-tier-sql%2FvirtualNetwork.azuredeploy.json
[3]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-n-tier-sql%2Fworkload.azuredeploy.json
[4]: https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-n-tier-sql%2Fsecurity.azuredeploy.json