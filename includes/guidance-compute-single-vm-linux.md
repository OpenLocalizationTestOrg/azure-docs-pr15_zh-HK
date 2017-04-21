本文概述一套行之有效的做法，对于运行 Linux 虚拟机 (VM) 在 Azure，关注可扩展性、 可用性、 可管理性和安全性上。 运行各种流行的 Linux 发行版本，包括 CentOS、 Debian，Red Hat 企业，Ubuntu 和 FreeBSD 的 azure 支持。 有关详细信息，请参阅[Azure 和 Linux][azure-linux]。

> [AZURE.NOTE] Azure 具有两种不同的部署模型︰[资源管理器][resource-manager-overview]和经典。 本文使用资源管理器，Microsoft 建议为新的部署。

我们不建议使用单个虚拟机的生产工作负载，因为在 Azure 上的单个虚拟机没有正常运行时间的服务级别协议 (SLA)。 若要获取 SLA，必须部署多个虚拟机的[可用性设置][availability-set]。 有关详细信息，请参阅[运行多个虚拟机在 Azure 上的][multi-vm]。 

## <a name="architecture-diagram"></a>体系结构关系图

资源调配虚拟机在 Azure 中的涉及更多的移动部件比只是将 VM 本身。 有需要考虑的计算、 网络和存储元素。

> 包括此体系结构示意图的 Visio 文档是可用于在[Microsoft 下载中心]下载[visio-download]。 此图是在"计算的单个 VM"页。

![[0]][0]

- **资源组。** [_资源组_][resource-manager-overview]是一个容器，包含相关的资源。 创建要为此 VM 持有这些资源的资源组。

- **VM**. 可以设置虚拟机从发布的图像列表或上传到 Azure blob 存储的虚拟硬盘 (VHD) 文件。

- **OS 磁盘。** OS 磁盘是在[Azure 存储]中存储一个 VHD[azure-storage]。 这意味着即使主机停机问题仍然存在。 OS 磁盘是`/dev/sda1`。

- **临时磁盘。** 临时磁盘创建虚拟机。 此磁盘存储在主机计算机上的物理驱动器上。 它是_不_保存在 Azure 存储中，并可能会在重启和其他 VM 生命周期事件期间的消失。 仅用于临时数据，如网页或交换文件使用该磁盘。 临时磁盘是`/dev/sdb1`和装在`/mnt/resource`或`/mnt`。

- **数据磁盘。** [数据磁盘][data-disk]是用于应用程序数据持久 VHD。 在 Azure 存储，如 OS 磁盘中存储数据磁盘。

- **虚拟网络 (VNet) 和子网。** 在 Azure 中的每个虚拟机部署到 (VNet)，这是进一步划分为多个子网。

- **公共 IP 地址。** 与虚拟机进行通信所需的公用 IP 地址&mdash;，例如通过 SSH。

- **网络接口 (NIC)**。 网卡使虚拟机与虚拟的网络进行通信。

- **网络安全组 (NSG)**。 [NSG] [nsg]用来允许/拒绝到子网的网络流量。 您可以关联 NSG 与单个网络接口卡或子网。 如果您将它与子网关联，NSG 规则将应用于该子网中的所有虚拟机。
 
- **诊断程序。** 诊断日志记录的管理和故障排除 VM 至关重要。

## <a name="recommendations"></a>建议

Azure 提供许多不同的资源和资源类型中，因此此参考体系结构可以提供许多不同的方法。 我们已经提供了 Azure 资源管理器模板安装的参考体系结构遵循这些建议。 如果您选择创建您自己的参考体系结构应遵循这些建议，除非有推荐不适合特定的要求。 

### <a name="vm-recommendations"></a>VM 的建议

因为这些机器大小支持[高级存储]，我们建议 DS 和 GS 系列[premium-storage]。 除非您有专门的工作负荷，如高性能计算中选择一个这些机器大小。 有关详细信息，请参阅[虚拟机大小][virtual-machine-sizes]。 在将现有的工作负荷移到 Azure，开头是最接近的匹配到后端服务器的虚拟机大小。 然后测量有关 CPU 您实际工作负荷的性能、 内存和磁盘输入/输出操作每秒 (IOPS)，并根据需要调整大小。 此外，如果您需要多个 Nic，请注意每个尺寸的 NIC 限制。  

当您配置 VM 和其他资源时，则必须指定位置。 通常情况下，选择最接近您的内部用户或客户的位置。 但是，并不是所有的 VM 大小可在所有的位置。 有关详细信息，请参阅[服务地区的][services-by-region]。 若要列出给定位置中可用的虚拟机大小，请运行下面的 Azure 的命令行界面 (CLI) 命令︰

```
    azure vm sizes --location <location>
```

有关选择已发布的虚拟机映像的信息，请参阅[导航和选择 Azure 的虚拟机映像][select-vm-image]。

### <a name="disk-and-storage-recommendations"></a>磁盘和存储的建议

为了获得最佳的磁盘 I/O 性能，我们建议[高级存储][premium-storage]，它将数据存储在固态驱动器 (SSDs)。 成本基于资源调配的磁盘的大小。 每 IOPS 和吞吐量 （即，数据传输速率） 还取决于磁盘大小，因此当您配置磁盘，考虑三个因素 （容量、 IOPS 和吞吐量）。 

一个存储帐户可支持 1 到 20 的虚拟机。

添加一个或多个数据磁盘。 当您创建一个 VHD 时，它未格式化。 登录到虚拟机格式化磁盘。 数据磁盘显示为`/dev/sdc`， `/dev/sdd`，依此类推。 您可以运行`lsblk`列出的块设备，包括磁盘。 若要使用数据的磁盘，创建分区和文件系统，并装入磁盘。 例如︰

```bat
    # Create a partition.
    sudo fdisk /dev/sdc     # Enter 'n' to partition, 'w' to write the change.     
    
    # Create a file system.
    sudo mkfs -t ext3 /dev/sdc1
    
    # Mount the drive.
    sudo mkdir /data1
    sudo mount /dev/sdc1 /data1
```

如果您有多个数据磁盘，请注意 I/O 限制存储帐户的总数。 有关详细信息，请参阅[虚拟机磁盘限制][vm-disk-limits]。

当您添加一个数据磁盘时，一个逻辑单元号 (LUN) 的 ID 分配给磁盘。 或者，您可以指定 LUN ID&mdash;的示例中，如果要替换磁盘并希望保留相同的 LUN ID，或者您有一个应用程序，寻找一个特定的 LUN id。 但是，请记住，LUN Id 必须是唯一的每个磁盘。

您可能需要更改 I/O 调度程序，针对固态驱动器 (SSDs) （使用高级存储） 上的性能进行优化。 常见的建议是 NOOP 计划用于 SSDs，但应使用[iostat]之类的工具来为您的特定工作负荷监视磁盘 I/O 性能。

- 为了获得最佳性能，创建一个单独的存储帐户来保存诊断日志。 标准的本地冗余存储 (LRS) 帐户是不足以诊断日志。


### <a name="network-recommendations"></a>网络建议

公共 IP 地址可以是动态的还是静态。 默认值是动态的。

- 保留一个[静态 IP 地址][static-ip]如果您需要将不会更改的固定的 IP 地址&mdash;的示例中，如果您需要在 DNS 中，创建一个记录或需要 IP 地址列入白名单。

- 您还可以创建 IP 地址完全合格的域名称 (FQDN)。 然后，您可以注册一个[CNAME 记录][cname-record]在 DNS 中指向 FQDN。 有关详细信息，请参阅[创建完全限定域名在 Azure 的门户网站][fqdn]。

所有 NSGs 都包含一组[默认规则][nsg-default-rules]，包括阻止所有入站互联网通信的规则。 不能删除默认的规则，但其他规则可以重写它们。 要启用 Internet 通讯，请创建允许对特定端口的入站的通信规则&mdash;例如，http 端口 80。  

若要启用 SSH，添加允许入站的通信的 TCP 端口 22 NSG 规则。

## <a name="scalability-considerations"></a>可伸缩性注意事项

您可以通过[更改 VM 大小]来扩展虚拟机向上或向下[vm-resize]。 

水平扩展，请将两个或多个虚拟机进入负载平衡器后面设置可用性。 有关详细信息，请参阅[运行多个虚拟机在 Azure 上的][multi-vm]。

## <a name="availability-considerations"></a>可用性考虑事项

如上文所述，单个虚拟机没有 SLA。 若要获取 SLA，必须到可用性组部署多个 Vm。

VM 可能受到[计划的维护][planned-maintenance]或[计划外的维护][manage-vm-availability]。 您可以使用[虚拟机重新启动日志][reboot-logs]来确定虚拟机重新启动由计划中的维护。

Vhd 后盾[Azure 存储][azure-storage]，其被复制的耐用性和可用性。

为了防止数据意外丢失在正常操作过程 （例如，由于用户错误），则还应实现使用[blob 快照]的时间点备份[blob-snapshot]或其他工具。

## <a name="manageability-considerations"></a>可管理性注意事项

**资源组。** 放入同一个[资源组]共享相同的生命周期的紧耦合的资源[resource-manager-overview]。 资源组允许您部署和监视资源作为一个组，并汇总收费成本由资源组。 您还可以为一组，这是非常有用的测试部署中删除资源。 为资源提供有意义的名称。 这样就更容易地找到特定的资源和了解其作用。 [Azure 资源建议的命名约定]，请参阅[naming conventions]。

**SSH**。 创建一个 Linux 虚拟机之前，生成的 2048年位的 RSA 公私密钥对。 当您创建虚拟机使用的公钥的密钥文件。 有关详细信息，请参阅[如何使用 SSH 使用 Linux 和 Mac 在 Azure 上的][ssh-linux]。

**VM 诊断程序。** 启用监视和诊断，包括基本健康指标、 诊断基础结构日志和[引导诊断][boot-diagnostics]。 启动诊断程序可帮助您诊断启动故障，如果您的虚拟机进入非可引导状态。 有关详细信息，请参阅[启用监视和诊断][enable-monitoring]。  

以下 CLI 命令启用诊断︰

```
    azure vm enable-diag <resource-group> <vm-name>
```

**停止虚拟机。** Azure 发出"停止"和"Deallocated"状态之间的差异。 您负责时 VM 状态为"已停止"。 您不收取时 VM 释放。

解除分配虚拟机，请使用以下 CLI 命令︰

```
    azure vm deallocate <resource-group> <vm-name>
```

- Azure 门户中的**停止**按钮还释放 VM。 但是，如果您关闭，操作系统通过同时登录，停止 VM，但均_未_释放，因此将仍然收取。

**正在删除虚拟机。** 如果您删除虚拟机，则不会删除 Vhd。 这意味着您可以安全删除虚拟机，而不会丢失数据。 但是，您将仍然会收取存储。 若要删除 VHD，从[blob 存储区]删除文件[blob-storage]。

为了防止意外删除，使用[资源锁][resource-lock]锁定整个资源组或锁定个别资源，如虚拟机。 

## <a name="security-considerations"></a>安全注意事项

通过使用[OSPatching] VM 扩展自动化操作系统更新。 当您配置 VM 安装此扩展。 您可以指定如何经常来安装修补程序，以及是否修补后重新启动。

使用[基于角色的访问控制][ rbac] (RBAC) 来控制部署的 Azure 资源访问权限。 RBAC 允许您指定授权角色 DevOps 团队中的成员。 例如，读者角色可以查看 Azure 的资源，但不是创建、 管理或删除它们。 某些角色是特定于特定 Azure 的资源类型。 例如，虚拟机参与者角色可以重新启动或取消分配虚拟机、 重置管理员密码，创建一个虚拟机，等等。 其他[内置的 RBAC 角色][rbac-roles]此参考体系结构包括[DevTest 实验室用户]可能有用[rbac-devtest]和[网络投稿人][rbac-network]。 

用户可以分配给多个角色，您可以创建自定义角色的更细粒度的权限。

> [AZURE.NOTE] RBAC 不限制登录到虚拟机的用户可以执行的操作。 这些权限由访客操作系统上的科目类型决定。   

使用[审核日志][audit-logs]若要查看资源调配操作和其他虚拟机事件。

- 请考虑[Azure 磁盘加密][disk-encryption]如果需要加密的操作系统和数据磁盘。 

## <a name="solution-deployment"></a>解决方案部署

[部署][github-folder]的体系结构，说明这些最佳做法可供参考。 此参考体系结构包括一个虚拟网络 (VNet)、 网络安全组 (NSG) 和单个虚拟机 (VM)。

有几种方法来部署此参考体系结构。 最简单的方法是按照下列步骤操作︰ 

1. 右键单击下面的按钮，然后选择"在新选项卡中的打开链接"或"在新窗口中的打开链接"。
[![将部署到 Azure](../articles/guidance/media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-single-vm%2Fazuredeploy.json)

2. 一旦在 Azure 的门户已经打开链接，您必须设置的某些输入值︰ 
    - **资源组**名称已定义参数文件中，因此选择**新建**，然后输入`ra-single-vm-rg`在文本框中。
    - 从**位置**下拉框中选择的地区。
    - 不允许编辑**模板根 Uri**或**参数根 Uri**的文本框。
    - 选择从下拉列表框中， **linux**的**操作系统类型**。
    - 检查的条款和条件，然后单击**我同意的条款和条件如以上所述**复选框。
    - 单击**购买**按钮。

3. 等待部署完成。

4. 参数文件包含硬编码的管理员用户名和密码，并强烈建议您立即更改两个。 单击名为 VM 上`ra-single-vm0 `Azure 门户中。 然后，单击**支持 + 疑难解答**部分中的**重置密码**。 在**模式**下拉列表框中，选择**重置密码**，然后选择一个新**的用户名**和**密码**。 单击**更新**按钮以保存新的用户名和密码。

部署此参考体系结构的其他方法的信息，请参阅自述文件[指南-单个 vm]中的[github-folder]Github 文件夹。

## <a name="customize-the-deployment"></a>自定义部署

若要更改部署以满足您的需要，按照[指南-单个 vm]中[github-folder]页。

## <a name="next-steps"></a>下一步行动

为了使[虚拟机的 SLA] [vm-sla]应用，必须部署在可用性设置的两个或多个实例。 有关详细信息，请参阅[运行多个虚拟机在 Azure 上的][multi-vm]。

<!-- links -->

[audit-logs]: https://azure.microsoft.com/en-us/blog/analyze-azure-audit-logs-in-powerbi-more/
[availability-set]: ../articles/virtual-machines/virtual-machines-windows-create-availability-set.md
[azure-cli]: ../articles/virtual-machines-command-line-tools.md
[azure-linux]: ../articles/virtual-machines/virtual-machines-linux-azure-overview.md
[azure-storage]: ../articles/storage/storage-introduction.md
[blob-snapshot]: ../articles/storage/storage-blob-snapshots.md
[blob-storage]: ../articles/storage/storage-introduction.md
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[cname-record]: https://en.wikipedia.org/wiki/CNAME_record
[data-disk]: ../articles/virtual-machines/virtual-machines-linux-about-disks-vhds.md
[disk-encryption]: ../articles/security/azure-security-disk-encryption.md
[enable-monitoring]: ../articles/monitoring-and-diagnostics/insights-how-to-use-diagnostics.md
[fqdn]: ../articles/virtual-machines/virtual-machines-linux-portal-create-fqdn.md
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-single-vm/
[iostat]: https://en.wikipedia.org/wiki/Iostat
[manage-vm-availability]: ../articles/virtual-machines/virtual-machines-linux-manage-availability.md
[multi-vm]: ../articles/guidance/guidance-compute-multi-vm.md
[naming conventions]: ../articles/guidance/guidance-naming-conventions.md
[nsg]: ../articles/virtual-network/virtual-networks-nsg.md
[nsg-default-rules]: ../articles/virtual-network/virtual-networks-nsg.md#default-rules
[OSPatching]: https://github.com/Azure/azure-linux-extensions/tree/master/OSPatching
[planned-maintenance]: ../articles/virtual-machines/virtual-machines-linux-planned-maintenance.md
[premium-storage]: ../articles/storage/storage-premium-storage.md
[rbac]: ../articles/active-directory/role-based-access-control-what-is.md
[rbac-roles]: ../articles/active-directory/role-based-access-built-in-roles.md
[rbac-devtest]: ../articles/active-directory/role-based-access-built-in-roles.md#devtest-lab-user
[rbac-network]: ../articles/active-directory/role-based-access-built-in-roles.md#network-contributor
[reboot-logs]: https://azure.microsoft.com/en-us/blog/viewing-vm-reboot-logs/
[Resize-VHD]: https://technet.microsoft.com/en-us/library/hh848535.aspx
[Resize virtual machines]: https://azure.microsoft.com/en-us/blog/resize-virtual-machines/
[resource-lock]: ../articles/resource-group-lock-resources.md
[resource-manager-overview]: ../articles/azure-resource-manager/resource-group-overview.md
[select-vm-image]: ../articles/virtual-machines/virtual-machines-linux-cli-ps-findimage.md
[services-by-region]: https://azure.microsoft.com/en-us/regions/#services
[ssh-linux]: ../articles/virtual-machines/virtual-machines-linux-mac-create-ssh-keys.md
[static-ip]: ../articles/virtual-network/virtual-networks-reserved-public-ip.md
[storage-price]: https://azure.microsoft.com/pricing/details/storage/
[virtual-machine-sizes]: ../articles/virtual-machines/virtual-machines-linux-sizes.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vm-disk-limits]: ../articles/azure-subscription-service-limits.md#virtual-machine-disk-limits
[vm-resize]: ../articles/virtual-machines/virtual-machines-linux-change-vm-size.md
[vm-sla]: https://azure.microsoft.com/en-us/support/legal/sla/virtual-machines/v1_0/
[readme]: https://github.com/mspnp/reference-architectures/blob/master/guidance-compute-single-vm
[components]: #Solution-components
[blocks]: https://github.com/mspnp/template-building-blocks
[0]: ./media/guidance-blueprints/compute-single-vm.png "在 Azure 中的单一 Linux 虚拟机体系结构"

