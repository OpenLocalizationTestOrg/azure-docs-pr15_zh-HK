<properties
   pageTitle="Azure VM 备份失败︰ 无法与快照状态的 VM 代理通信-快照 VM 子任务超时 |Microsoft Azure"
   description="症状的原因和解决方法与相关的 Azure VM 备份失败无法与快照状态的 VM 代理通信。 虚拟机快照的子任务超时错误"
   services="backup"
   documentationCenter=""
   authors="genlin"
   manager="cfreeman"
   editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="jimpark; markgal;genli"/>

# <a name="azure-vm-backup-fails-could-not-communicate-with-the-vm-agent-for-snapshot-status---snapshot-vm-sub-task-timed-out"></a>Azure VM 备份失败︰ 无法与快照状态的 VM 代理通信-快照 VM 子任务超时

## <a name="summary"></a>摘要

注册并计划 Azure 备份虚拟机 (VM) 之后, Azure 备份服务启动备份作业在预定的时间通过在虚拟机快照的时间点备份扩展名进行通信。 有可能会阻止该快照的条件触发这会导致备份失败。 这篇文章到 Azure VM 备份失败与快照超时错误相关的问题，提供排除故障的步骤。

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="symptom"></a>故障现象

Microsoft 基础结构即服务 (IaaS) VM 的 Azure 备份失败，在[Azure 门户](https://portal.azure.com/)的作业错误详细信息中返回以下错误消息︰

**无法与快照状态的 VM 代理通信-快照 VM 子任务超时。**

## <a name="cause"></a>原因
有四种有关此错误的常见原因︰

- VM 不具有 Internet 访问权限。
- 在虚拟机中安装了 Azure 的 Microsoft VM 代理是过时的 （对于 Linux 虚拟机）。
- 备份扩展名不能更新或加载。
- 无法检索快照状态或快照不能采取。

## <a name="cause-1-the-vm-does-not-have-internet-access"></a>原因 1︰ 虚拟机没有 Internet 访问权限
每个部署要求中，VM 没有 Internet 访问权限，或限制，防止对 Azure 的基础结构的访问。

备份的扩展需要连接到 Azure 的公用 IP 地址，才能正常工作。 这是因为它将命令发送到 Azure 存储端点 (HTTP URL) 来管理虚拟机的快照。 如果扩展没有公共 Internet 访问权限，备份将最终失败。

### <a name="solution"></a>解决方案
在这种情况下，使用以下方法之一来解决此问题︰

- 白名单 Azure 数据中心 IP 范围
- 创建 HTTP 通信流的路径

### <a name="to-whitelist-the-azure-datacenter-ip-ranges"></a>到白名单 Azure 数据中心 IP 范围

1. 获取要列入白名单的[Azure 数据中心 IPs 的列表](https://www.microsoft.com/download/details.aspx?id=41653)。
2. 通过使用新建 NetRoute cmdlet 允许 IPs。 在 Azure VM 中提升 PowerShell 窗口 （以管理员身份运行） 运行该 cmdlet。
3. 将规则添加到网络安全组 (NSG)，如果您有一个允许访问的 Ip。

### <a name="to-create-a-path-for-http-traffic-to-flow"></a>若要创建 HTTP 通信流的路径

1. 如果您有网络限制 (例如，NSG)，部署 HTTP 代理服务器来路由通信。
2. 如果您有一个网络安全组 (NSG)，添加规则以允许从 HTTP 代理服务器访问 Internet。

了解如何[设置 HTTP 代理的虚拟机备份](backup-azure-vms-prepare.md#using-an-http-proxy-for-vm-backups)。

## <a name="cause-2-the-microsoft-azure-vm-agent-installed-in-the-vm-is-out-of-date-for-linux-vms"></a>原因 2︰ 安装在虚拟机中的 Azure 的 Microsoft VM 代理是过时的 （对于 Linux 虚拟机）

### <a name="solution"></a>解决方案
最代理相关或扩展相关的 Linux 虚拟机故障会影响旧的 VM 代理问题所致。 一般原则是，要解决此问题的第一个步骤如下所示︰

1. [安装最新的 Azure VM 代理](https://github.com/Azure/WALinuxAgent)。
2. 请确保在 VM 上运行 Azure 的代理程序。 若要执行此操作，请运行以下命令︰```ps -e```

    如果此进程未运行，请使用以下命令来重新启动它。

    对于 Ubuntu:```service walinuxagent start```

    对于其它版本: '' 服务 waagent 开始
```

3. [Configure the auto restart agent](https://github.com/Azure/WALinuxAgent/wiki/Known-Issues#mitigate_agent_crash).

4. Run a new test backup. If the failures persist, please collect logs from the following folders for further debugging.

    We require the following logs from the customer’s VM:

    - /var/lib/waagent/*.xml
    - /var/log/waagent.log
    - /var/log/azure/*

If we require verbose logging for waagent, follow these steps to enable this:

1. In the /etc/waagent.conf file, locate the following line:

    Enable verbose logging (y|n)

2. Change the **Logs.Verbose** value from n to y.
3. Save the change, and then restart waagent by following the previous steps in this section.

## Cause 3: The backup extension fails to update or load
If extensions cannot be loaded, then Backup fails because a snapshot cannot be taken.

### Solution
For Windows guests:

1. Verify that the iaasvmprovider service is enabled and has a startup type of automatic.
2. If this is not the configuration, enable the service to determine whether the next backup succeeds.

For Linux guests:

The latest version of VMSnapshot Linux (extension used by backup) is 1.0.91.0

If the backup extension still fails to update or load, you can force the VMSnapshot extension to be reloaded by uninstalling the extension. The next backup attempt will reload the extension.

### To uninstall the extension

1. Go to the [Azure portal](https://portal.azure.com/).
2. Locate the particular VM that has backup problems.
3. Click **Settings**.
4. Click **Extensions**.
5. Click **Vmsnapshot Extension**.
6. Click **uninstall**.

This will cause the extension to be reinstalled during the next backup.

## Cause 4: The snapshots status cannot be retrieved or the snapshots cannot be taken
VM backup relies on issuing snapshot command to underlying storage. The backup can fail because it has no access to storage or because of a delay in snapshot task execution.

### Solution
The following conditions can cause snapshot task failure:

| Cause | Solution |
| ----- | ----- |
| VMs that have Microsoft SQL Server Backup configured. By default, VM Backup runs a VSS Full backup on Windows VMs. On VMs that are running SQL Server-based servers and on which SQL Server Backup is configured, snapshot execution delays may occur. | Set following registry key if you are experiencing backup failures because of snapshot issues.<br><br>[HKEY_LOCAL_MACHINE\SOFTWARE\MICROSOFT\BCDRAGENT] "USEVSSCOPYBACKUP"="TRUE" |
| VM status is reported incorrectly because the VM is shut down in RDP. If you shut down the virtual machine in RDP, check the portal to determine whether that VM status is reflected correctly. | If it’s not, shut down the VM in the portal by using the ”Shutdown” option in the VM dashboard. |
| Many VMs from the same cloud service are configured to back up at the same time. | It’s a best practice to spread out the VMs from the same cloud service to have different backup schedules. |
| The VM is running at high CPU or memory usage. | If the VM is running at high CPU usage (more than 90 percent) or high memory usage, the snapshot task is queued and delayed and eventually times out. In this situation, try on-demand backup. |
|The VM cannot get host/fabric address from DHCP.|DHCP must be enabled inside the guest for IaaS VM Backup to work.  If the VM cannot get host/fabric address from DHCP response 245, then it cannot download ir run any extensions. If you need a static private IP, you should configure it through the platform. The DHCP option inside the VM should be left enabled. View more information about [Setting a Static Internal Private IP](../virtual-network/virtual-networks-reserved-private-ip.md).|
