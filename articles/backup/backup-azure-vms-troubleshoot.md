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

## <a name="backup"></a>备份

| 备份操作 | 错误详细信息 | 解决方法 |
| -------- | -------- | -------|
|备份|    无法执行操作，因为 VM 不再存在。 -停止而不删除备份数据保护虚拟机。 更多详情，请访问 http://go.microsoft.com/fwlink/?LinkId=808124   |当主虚拟机将被删除，但将继续寻找虚拟机执行备份的备份策略时，将发生这种情况。 若要修复此错误︰ <ol><li> 重新创建具有相同名称和相同的资源组名称 [云服务名称] 虚拟机<br>(OR)</li><li> 停止保护虚拟机带有或不带删除备份数据。 [更多详细信息](http://go.microsoft.com/fwlink/?LinkId=808124)</li></ol>|
|备份|无法与快照状态的 VM 代理进行通信。 -确保虚拟机可以访问互联网。 此外，正如 http://go.microsoft.com/fwlink/?LinkId=800034 在故障排除手册 》 中提到更新 VM 代理 |如果 VM 代理问题或以某种方式阻止 Azure 的基础结构的网络访问，则会引发此错误。 了解更多有关调试启动 VM 快照的问题。<br> 如果 VM 代理不会造成任何问题，然后重新启动虚拟机。 有时 VM 状态不正确可能会导致问题并重新启动虚拟机将重置此"不良状态"|
|备份|    恢复服务扩展操作失败。 -请请确保最新的虚拟机代理是在虚拟机上的存在和运行代理服务。 请重试备份操作，如果失败，请与 Microsoft 支持部门联系。|   VM 代理已过期时，将引发此错误。 请参阅"更新虚拟机代理"部分下面更新虚拟机代理。|
|备份 |不存在虚拟机。 -请确保该虚拟机存在或选择一个不同的虚拟机。 | 当主虚拟机将被删除，但将继续寻找虚拟机执行备份的备份策略时，将发生这种情况。 若要修复此错误︰ <ol><li> 重新创建具有相同名称和相同的资源组名称 [云服务名称] 虚拟机<br>(OR)<br></li><li>停止保护虚拟机而不删除备份数据。 [更多详细信息](http://go.microsoft.com/fwlink/?LinkId=808124)</li></ol>|
|备份 |命令执行失败。 -另一个操作正在进行此项。 前一个操作完成后，请等待，然后重试 |现有的备份或还原作业的虚拟机正在运行，并且现有的作业正在运行时，无法启动新的作业。|
| 备份 | 复制 Vhd 从备份存储库超时-请重试该操作在几分钟的时间。 如果问题仍然存在，请联系 Microsoft 技术支持。 | 要复制大量数据时，将发生这种情况。 请检查您是否拥有不少于 16 个数据磁盘。 |
| 备份 | 备份失败，发生内部错误-请重试该操作几分钟的时间。 如果问题仍然存在，请联系 Microsoft 技术支持 | 您可以获取此错误的原因︰ <ol><li> 访问 VM 存储处于一个暂时性的问题。 请检查[Azure 状态](https://azure.microsoft.com/en-us/status/)以查看是否有任何与计算/储存/网络在区域相关的日常问题。 请重试备份开机自检问题，可以减少。 <li>原始的虚拟机已被删除，因此无法执行备份。 若要保留已删除的虚拟机的备份数据，但停止备份错误，撤消 VM 和选择选项以保留数据。 这将停止备份日程安排以及定期的错误消息。 |
| 备份 | 有关所选物料的 VM 代理扩展安装 Azure 恢复服务失败是 Azure 恢复服务扩展的先决条件。 请安装 Azure VM 代理，然后重新注册操作 | <ol> <li>请检查是否已正确安装虚拟机代理。 <li>请确保已正确设置了虚拟机配置上的标志。</ol> [阅读更多](#validating-vm-agent-installation)有关虚拟机安装代理，以及如何验证虚拟机安装代理。 |
| 备份 | 扩展安装失败并出现错误"COM + 无法与 Microsoft 分布式事务处理协调器 | 这通常意味着，COM + 服务未运行。 有关解决此问题的帮助，请与 Microsoft 技术支持联系。 |
| 备份 | 快照操作失败，VSS 操作错误"BitLocker 驱动器加密通过锁定此驱动器。 您必须解除锁定此驱动器从控制面板。 | 关闭虚拟机上的所有驱动器 BitLocker 并观察 VSS 问题是否已解决 |
| 备份 | 具有高级存储上存储虚拟硬盘的虚拟机的备份不受支持 | 无 |
| 备份 | 找不到 azure 的虚拟机。 | 当主虚拟机将被删除，但将继续寻找虚拟机执行备份的备份策略时，将发生这种情况。 若要修复此错误︰ <ol><li>重新创建具有相同名称和相同的资源组名称 [云服务名称] 虚拟机 <br>(OR) <li> 禁用此 VM 的保护，这样将不会创建备份作业 </ol> |
| 备份 | 不存在于虚拟机虚拟机代理-未安装所需的前提条件，VM 代理并重新启动该操作。 | [阅读更多](#vm-agent)有关虚拟机安装代理，以及如何验证虚拟机安装代理。 |

## <a name="jobs"></a>作业

| 操作 | 错误详细信息 | 解决方法 |
| -------- | -------- | -------|
| 取消作业 | 取消此作业类型不受支持-请稍候，直到完成作业。 | 无 |
| 取消作业 | 作业未处于取消状态-请稍候，直到完成作业。 <br>OR<br> 所选的作业未处于取消状态-请等待作业完成。| 在所有可能性中作业已基本完成;请稍候，直到完成作业 |
| 取消作业 | 无法取消作业，因为它不是正在进行-取消才支持正在进行的工作。 请尝试取消正在进行中的作业。 | 这是因为暂时的状态。 等待一分钟，然后重试取消操作 |
| 取消作业 | 无法取消作业-请稍候，直到完成作业。 | 无 |


## <a name="restore"></a>还原
| 操作 | 错误详细信息 | 解决方法 |
| -------- | -------- | -------|
| 还原 | 还原失败，云内部错误 | <ol><li>要还原的云服务配置了 DNS 设置。 您可以检查 <br>$deployment = 获取 AzureDeployment 服务"服务名"-插槽"生产"获得 AzureDns-DnsSettings $deployment。DnsSettings<br>如果没有配置地址，这意味着配置了 DNS 设置。<br> <li>要还原到的云服务配置了 ReservedIP，在云服务中的现有虚拟机处于停止状态。<br>您可以检查云服务具有通过使用以下 powershell cmdlet 的保留 IP:<br>$deployment = 获取 AzureDeployment 服务"服务名"-插槽"生产"$ dep。ReservedIPName <br><li>您正试图将具有以下特殊的网络配置中的虚拟机还原到同一个云服务。 <br>的在负载平衡器配置 （内部和外部） 下虚拟机<br>的与多个保留 ip 地址虚拟机<br>的具有多个 Nic 虚拟机<br>请选择用户界面中的一种新的云服务或请参阅[恢复注意事项](./backup-azure-arm-restore-vms.md/#restoring-vms-with-special-network-configurations)，Vm 与特殊的网络配置</ol> |
| 还原 | 所选的 DNS 名称已被使用-请指定一个不同的 DNS 名称并再试一次。 | 此处输入的 DNS 名称指的是云服务名称 (通常结尾。 cloudapp.net)。 这必须是唯一的。 如果您遇到此错误，您需要在还原过程中选择不同的虚拟机名称。 <br><br> 请注意，只对 Azure 的门户网站的用户显示此错误。 通过 PowerShell 还原操作将会成功，因为它只还原磁盘，并且不会创建虚拟机。 当 VM 明确由您创建磁盘还原操作后，将面临错误。 |
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

## <a name="troubleshoot-vm-snapshot-issues"></a>虚拟机快照疑难解答
虚拟机备份依赖于到底层存储发布快照命令。 在快照任务执行无权访问存储或延迟可能会备份失败。 以下情况可能会导致快照任务失败。

1. 存储网络访问被阻止使用 NSG<br>
   了解如何[启用网络访问](backup-azure-vms-prepare.md#2-network-connectivity)使用或者添加到白名单的 Ip 存储或通过代理服务器的详细信息。
2.  使用 Sql Server 备份配置的虚拟机可能会导致快照任务延迟 <br>
    默认情况下，虚拟机备份问题 VSS 完整备份 Windows 虚拟机上。 在虚拟机运行的 Sql 服务器的 Sql Server 备份配置，这可能会导致延迟快照执行。 请如果您遇到由于快照问题而引起的备份失败，将设置以下注册表项。

    ```
    [HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT]
    "USEVSSCOPYBACKUP"="TRUE"
    ```
3.  因为 VM 是 RDP 中的关闭虚拟机状态报告错误。  <br>
    如果已关闭 RDP 中的虚拟机，请检查门户中回正确反映 VM 的状态。 请关闭虚拟机在门户中使用 VM 仪表板中的关闭选项如果没有，请关闭虚拟机在门户中使用 VM 仪表板中的关闭选项。
4.  以上四个 VM 共享相同云服务，如果配置多个备份策略来暂存备份时间因此不超过四个 VM 备份启动一次。 试着分散的备份开始时间策略之间相隔一小时。 
5.  虚拟机正在运行在高 CPU/内存。<br>
    如果虚拟机在高 CPU 正在运行 usage(>90%) 或内存快照任务排队、 延迟和最终将获取超时。 请尝试在这种情况下按需备份。

<br>

## <a name="networking"></a>网络连接
像所有扩展，备份扩展需要公用的 internet 进行工作访问。 没有公共 internet 访问权限来表现不同的方式︰

- 扩展安装可能会失败
- （如磁盘快照） 备份操作可能会失败
- 显示状态的备份操作可能会失败

具有已明确提出需要解决公共 internet 地址[下面](http://blogs.msdn.com/b/mast/archive/2014/06/18/azure-vm-provisioning-stuck-on-quot-installing-extensions-on-virtual-machine-quot.aspx)。 您将需要检查 VNET 的 DNS 配置并确保 Azure Uri 可以得到解决。

一旦正确完成名称解析，对 Azure IPs 的访问还需要提供。 若要解除锁定到 Azure 的基础结构的访问，请按照以下步骤之一︰

1. Azure 数据中心白名单的 IP 范围。
    - 得到[Azure 数据中心 IPs](https://www.microsoft.com/download/details.aspx?id=41653)可列入白名单的列表。
    - 取消阻止使用[New NetRoute](https://technet.microsoft.com/library/hh826148.aspx) cmdlet 的 Ip。 在 Azure VM，此 cmdlet 以提升 PowerShell 窗口 （以管理员身份运行）。
    - 将规则添加到 NSG （如果有的话就地） 以允许访问的 Ip。
2. 创建 HTTP 通信流的路径
    - 如果您有一些网络限制 （网络安全组） 就地部署 HTTP 代理服务器来路由通信。 HTTP 代理服务器部署的步骤可以找到[这里](backup-azure-vms-prepare.md#2-network-connectivity)。
    - 将规则添加到 NSG （如果有的话就地） 以允许从 HTTP 代理服务器访问 INTERNET。

>[AZURE.NOTE] 在 IaaS VM 备份工作访客，必须启用了 DHCP。  如果您需要一个专用的静态 IP，您应通过平台来进行配置。 在虚拟机内部的 DHCP 选项应该启用。
查看有关[设置一个静态的内部专用 IP](../virtual-network/virtual-networks-reserved-private-ip.md)的详细信息。
