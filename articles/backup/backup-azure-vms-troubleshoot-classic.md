<properties
    pageTitle="Azure 虚拟机备份疑难解答 |Microsoft Azure"
    description="Azure 的虚拟机的备份和还原的疑难解答"
    services="backup"
    documentationCenter=""
    authors="trinadhk"
    manager="shreeshd"
    editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/26/2016"
    ms.author="trinadhk;jimpark;"/>


# <a name="troubleshoot-azure-virtual-machine-backup"></a>Azure 虚拟机备份疑难解答

> [AZURE.SELECTOR]
- [恢复服务存储库](backup-azure-vms-troubleshoot.md)
- [备份存储库](backup-azure-vms-troubleshoot-classic.md)

您可以解决遇到时使用 Azure 备份信息与下表中列出的错误。

## <a name="discovery"></a>发现

| 备份操作 | 错误详细信息 | 解决方法 |
| -------- | -------- | -------|
| 发现 | 未能发现新的项目-遇到 Microsoft Azure 备份和内部错误。 请等待几分钟，然后再次尝试该操作。 | 在 15 分钟后重试的发现过程。
| 发现 | 未能发现新的项目--另一项搜索操作已在进行。 请等待，直到当前的查找操作已完成。 | 无 |

## <a name="register"></a>注册
| 备份操作 | 错误详细信息 | 解决方法 |
| -------- | -------- | -------|
| 注册 | 连接到虚拟机的数据磁盘的数量超出支持的限制-请分离该虚拟机上的某些数据磁盘并重试该操作。 Azure 备份支持最多 16 个数据磁盘连接到 Azure 的虚拟机的备份 | 无 |
| 注册 | Microsoft Azure 备份时遇到内部错误-等待几分钟，然后重试该操作。 如果问题仍然存在，请与 Microsoft 技术支持。 | 您可以获得特优 LRS 此错误原因的 VM 不支持以下配置之一。 <br> 高级存储的虚拟机可以备份使用恢复服务存储库。 [了解更多信息](backup-introduction-to-azure-backup.md/#back-up-and-restore-premium-storage-vms) |
| 注册 | 注册失败，安装代理操作超时 | 检查是否支持虚拟机的操作系统版本。 |
| 注册 | 命令执行失败的另一个操作正在进行此项中。 请稍候，直到完成上一操作 | 无 |
| 注册 | 具有高级存储上存储虚拟硬盘的虚拟机的备份不受支持 | 无 |
| 注册 | 不存在于虚拟机虚拟机代理-未安装所需的前提条件，VM 代理并重新启动该操作。 | [阅读更多](#vm-agent)有关虚拟机安装代理，以及如何验证虚拟机安装代理。 |

## <a name="backup"></a>备份

| 备份操作 | 错误详细信息 | 解决方法 |
| -------- | -------- | -------|
| 备份 | 无法与快照状态的 VM 代理进行通信。 快照的虚拟机子任务超时。 -请请参阅有关如何解决此问题的故障排除指南。 | 如果 VM 代理问题或以某种方式阻止 Azure 的基础结构的网络访问，则会引发此错误。 了解更多有关[VM 快照上调试问题](backup-azure-troubleshoot-vm-backup-fails-snapshot-timeout.md)。 <br> 如果 VM 代理不会造成任何问题，然后重新启动虚拟机。 有时 VM 状态不正确可能会导致问题并重新启动虚拟机将重置此"不良状态" |
| 备份 | 备份失败，发生内部错误-请重试该操作几分钟的时间。 如果问题仍然存在，请联系 Microsoft 技术支持 | 请检查访问 VM 存储中是否存在不是暂时性的问题。 请检查[Azure 状态](https://azure.microsoft.com/en-us/status/)以查看是否有任何与计算/储存/网络在区域相关的日常问题。 请重试备份开机自检问题，可以减少。 |
| 备份 | 无法执行操作，因为 VM 不再存在。 | 由于已删除虚拟机配置为进行备份，则无法执行备份。 请转到受保护的项目视图停止进一步的备份，选择受保护的项，然后单击停止保护。 您可以通过选择保留备份数据的选项将保留数据。 通过单击此虚拟机上配置保护注册项目视图中的，以后可以恢复保护|
| 备份 | 有关所选物料的 VM 代理扩展安装 Azure 恢复服务失败是 Azure 恢复服务扩展的先决条件。 请安装 Azure VM 代理，然后重新注册操作 | <ol> <li>请检查是否已正确安装虚拟机代理。 <li>请确保已正确设置了虚拟机配置上的标志。</ol> [阅读更多](#validating-vm-agent-installation)有关虚拟机安装代理，以及如何验证虚拟机安装代理。 |
| 备份 | 命令执行失败的另一个操作正在进行此项。 前一个操作完成后，请等待，然后重试 | 现有的备份或还原作业的虚拟机正在运行，并且现有的作业正在运行时，无法启动新的作业。 |
| 备份 | 扩展安装失败并出现错误"COM + 无法与 Microsoft 分布式事务处理协调器 | 这通常意味着，COM + 服务未运行。 有关解决此问题的帮助，请与 Microsoft 技术支持联系。 |
| 备份 | 快照操作失败，VSS 操作错误"BitLocker 驱动器加密通过锁定此驱动器。 您必须解除锁定此驱动器从控制面板。 | 关闭虚拟机上的所有驱动器 BitLocker 并观察 VSS 问题是否已解决 |
| 备份 | 具有高级存储上存储虚拟硬盘的虚拟机的备份不受支持 | 无 |
| 备份 | 找不到 azure 的虚拟机。 | 当主虚拟机将被删除，但将继续寻找虚拟机执行备份的备份策略时，将发生这种情况。 若要修复此错误︰ <ol><li>重新创建具有相同名称和相同的资源组名称 [云服务名称] 虚拟机 <br>(OR) <li> 禁用此 VM 的保护，以便后续的备份将不会被触发。 </ol> |
| 备份 | 不存在于虚拟机虚拟机代理-未安装所需的前提条件，VM 代理并重新启动该操作。 | [阅读更多](#vm-agent)有关虚拟机安装代理，以及如何验证虚拟机安装代理。 |

## <a name="jobs"></a>作业
| 操作 | 错误详细信息 | 解决方法 |
| -------- | -------- | -------|
| 取消作业 | 取消此作业类型不受支持-请稍候，直到完成作业。 | 无 |
| 取消作业 | 作业未处于取消状态-请稍候，直到完成作业。 <br>OR<br> 所选的作业未处于取消状态-请等待作业完成。| 在所有的可能性，该作业已基本完成;请稍候，直到完成作业 |
| 取消作业 | 无法取消作业，因为它不是正在进行-取消才支持正在进行的工作。 请尝试取消正在进行中的作业。 | 这是因为暂时的状态。 等待一分钟，然后重试取消操作 |
| 取消作业 | 无法取消作业-请稍候，直到完成作业。 | 无 |


## <a name="restore"></a>还原
| 操作 | 错误详细信息 | 解决方法 |
| -------- | -------- | -------|
| 还原 | 还原失败，云内部错误 | <ol><li>要还原的云服务配置了 DNS 设置。 您可以检查 <br>$deployment = 获取 AzureDeployment 服务"服务名"-插槽"生产"获得 AzureDns-DnsSettings $deployment。DnsSettings<br>如果没有配置地址，这意味着配置了 DNS 设置。<br> <li>要还原到的云服务配置了 ReservedIP，在云服务中的现有虚拟机处于停止状态。<br>您可以检查云服务具有通过使用以下 powershell cmdlet 的保留 IP:<br>$deployment = 获取 AzureDeployment 服务"服务名"-插槽"生产"$ dep。ReservedIPName <br><li>您正试图将具有以下特殊的网络配置中的虚拟机还原到同一个云服务。 <br>的在负载平衡器配置 （内部和外部） 下虚拟机<br>的与多个保留 ip 地址虚拟机<br>的具有多个 Nic 虚拟机<br>请选择用户界面中的一种新的云服务或请参阅[恢复注意事项](./backup-azure-restore-vms.md/#restoring-vms-with-special-network-configurations)，Vm 与特殊的网络配置</ol> |
| 还原 | 所选的 DNS 名称已被使用-请指定一个不同的 DNS 名称并再试一次。 | 此处输入的 DNS 名称指的是云服务名称 (通常结尾。 cloudapp.net)。 这必须是唯一的。 如果您遇到此错误，您需要在还原过程中选择不同的虚拟机名称。 <br><br> 只对 Azure 的门户网站的用户显示此错误。 通过 PowerShell 还原操作成功，因为它只还原磁盘，并且不会创建虚拟机。 当 VM 明确由您创建磁盘还原操作后，将面临错误。 |
| 还原 | 指定的虚拟网络配置不正确-请指定一个不同的虚拟网络配置，然后重试。 | 无 |
| 还原 | 指定的云服务使用保留的 IP，并不与正在还原的虚拟机的配置相匹配-请指定不同的云服务不使用保留的 IP，或选择另一个要从中进行恢复的恢复点。 | 无 |
| 还原 | 云服务已达到限制输入终结点的数量-通过指定不同的云服务或通过使用现有的终结点来重试此操作。 | 无 |
| 还原 | 备份存储库和目标存储帐户处于两个不同的区域-确保在还原操作中指定的存储帐户处于同一个 Azure 地区作为备份存储库。 | 无 |
| 还原 | 存储帐户指定为恢复操作不受支持-仅基本/标准存储帐户具有本地冗余或支持地理冗余复制设置。 请选择受支持的存储帐户 | 无 |
| 还原 | 指定用于还原操作的存储帐户的类型不是在线-确保在还原操作中指定的存储帐户处于联机状态 | 这可能由于 Azure 存储中或由于中断的暂时性错误。 请选择另一个存储帐户。 |
| 还原 | 已达到资源组配额-请从 Azure 门户中删除某些资源组或联系 Azure 支持增加限制。 | 无 |
| 还原 | 选定的子网不存在-请选择一个存在的子网 | 无 |


## <a name="policy"></a>策略
| 操作 | 错误详细信息 | 解决方法 |
| -------- | -------- | -------|
| 创建策略 | 未能创建策略-请减少保留选项以继续进行策略配置。 | 无 |


## <a name="vm-agent"></a>虚拟机代理

### <a name="setting-up-the-vm-agent"></a>设置虚拟机代理
通常情况下，虚拟机代理已在 Azure 的库中创建的虚拟机。 但是，从内部数据中心迁移的虚拟机不会安装了 VM 代理。 这样的虚拟机的虚拟机工程师需要显式安装。 了解有关[安装在现有的 VM 虚拟机代理](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx)详细信息。

对于 Windows 虚拟机︰

- 下载并安装[代理 MSI](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409)。 您需要管理员权限才能完成安装。
- [更新虚拟机的属性](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx)，以指示已安装代理。

对于 Linux 虚拟机︰

- 从 github 上安装最新的[Linux 代理](https://github.com/Azure/WALinuxAgent)。
- [更新虚拟机的属性](http://blogs.msdn.com/b/mast/archive/2014/04/08/install-the-vm-agent-on-an-existing-azure-vm.aspx)，以指示已安装代理。


### <a name="updating-the-vm-agent"></a>正在更新虚拟机代理
对于 Windows 虚拟机︰

- 更新虚拟机代理是简单，只需重新安装[虚拟机代理的二进制文件](http://go.microsoft.com/fwlink/?LinkID=394789&clcid=0x409)。 但是，您需要确保 VM 代理更新时运行任何备份操作。

对于 Linux 虚拟机︰

- 按照说明[更新 Linux 虚拟机代理](../virtual-machines/virtual-machines-linux-update-agent.md)上。


### <a name="validating-vm-agent-installation"></a>正在验证虚拟机代理安装
如何检查 Windows 虚拟机上的虚拟机代理版本︰

1. 登录到 Azure 的虚拟机并导航到文件夹*C:\WindowsAzure\Packages*。 您应该发现存在 WaAppAgent.exe 文件。
2. 用鼠标右键单击该文件，转到**属性**，然后选择**详细信息**选项卡。 产品版本字段应为 2.6.1198.718 或更高版本





