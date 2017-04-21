<properties
   pageTitle="解决 Azure 备份中的文件和文件夹备份较慢 |Microsoft Azure"
   description="提供故障排除指南，可帮助您诊断 Azure 备份性能问题的原因"
   services="backup"
   documentationCenter=""
   authors="genlin"
   manager="jimpark"
   editor=""/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/13/2016"
    ms.author="genli"/>

# <a name="troubleshoot-slow-backup-of-files-and-folders-in-azure-backup"></a>慢中 Azure 备份的文件和文件夹备份疑难解答

本文提供了可帮助您诊断文件和文件夹的备份性能较慢的原因，在使用 Azure 备份时的故障排除指南。 当使用 Azure 备份代理文件备份时，备份过程可能比预期长。 这种延迟可能导致一项或多项操作︰

-   [正在备份的计算机上有性能瓶颈。](#cause1)
-   [另一个进程或防病毒软件干扰 Azure 备份过程。](#cause2)
-   [Azure 的虚拟机 (VM) 上运行备份代理程序。](#cause3)  
-   [要备份的文件大数 （百万）。](#cause4)

在开始解决问题之前，我们建议您下载并安装[最新的 Azure 备份代理](http://aka.ms/azurebackup_agent)。 我们对要解决的各种问题、 添加功能，和改进性能的备份代理进行频繁的更新。

我们极力建议您查看[Azure 备份服务常见问题解答](backup-azure-backup-faq.md)，以确保您没有遇到任何常见的配置问题。

[AZURE.INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

<a id="cause1"></a>
## <a name="cause-performance-bottlenecks-on-the-computer"></a>原因︰ 在计算机上的性能瓶颈

正在备份计算机上的瓶颈期会导致延时。 例如，计算机能够读取或写入磁盘或通过网络发送数据的可用带宽可能会导致瓶颈。

Windows 提供了一个内置的工具，称为[性能监视器](https://technet.microsoft.com/magazine/2008.08.pulse.aspx)(Perfmon 来检测这些瓶颈)。

下面是一些性能计数器和有助于诊断为最佳备份瓶颈问题的范围。

| 计数器  | 状态  |
|---|---|
|逻辑磁盘 （物理磁盘）-空闲 %   | 50%闲置空闲 • 100%= 良好</br>• 49%至 20%的闲置空闲 = 警告或监视器</br>• 19%到 0 %idle 空闲 = 临界或外出时的规范|
|  逻辑磁盘 （物理磁盘）-%平均 秒的读取或写入的磁盘 |  • 为 0.015 毫秒 0.001 ms = 良好</br>• 为 0.025 毫秒 0.015 ms = 警告或监视器</br>• 0.026 ms 或再 = 严重或外出时的规范|
|  逻辑磁盘 （物理磁盘） — （用于所有实例） 的当前磁盘队列长度 | 80 请求超过 6 分钟 |
| 内存--分页池非字节数|• 不超过 60%的池消耗 = 良好<br>• 61%到 80%的池消耗 = 警告或监视器</br>• 大于消耗的 80%池 = 临界或外出时的规范|
| 内存--分页缓冲池字节 |• 不超过 60%的池消耗 = 良好</br>• 61%到 80%的池消耗 = 警告或监视器</br>• 大于消耗的 80%池 = 临界或外出时的规范|
| 内存-可用兆字节| • 的可用自由内存的 50%或更多 = 良好</br>• 25%的可用自由内存 = 监视器</br>• 10%的可用自由内存 = 警告</br>• 小于 100 MB 或可用自由内存的 5%= 临界或外出时的规范|
|处理器--\%处理器时间 （所有实例）|• 使用小于 60%= 良好</br>• 61%至 90%消耗 = 显示器或警告︰</br>• 91%到 100%消耗 = 临界|


> [AZURE.NOTE] 如果您确定基础结构是问题所在，我们建议您对定期为更好的性能的磁盘进行碎片整理。

<a id="cause2"></a>
## <a name="cause-another-process-or-antivirus-software-interfering-with-azure-backup"></a>原因︰ 另一个进程或干扰 Azure 备份的防病毒软件

我们已经看到在 Windows 系统中的其他进程有负面影响的 Azure 备份代理进程的性能的几个实例。 例如，如果您使用 Azure 备份代理程序和其他程序来备份数据，或者如果防病毒软件正在运行，并且锁定了要备份的文件，多个锁定上文件可能会导致争用。 在此情况下，备份可能失败，或者该作业可能需要比预期更长的时间。

在这种情况下的最佳建议是关闭其他备份程序以查看是否更改，Azure 备份代理的备份时间。 通常情况下，确保未在同时运行多个备份作业是不足以防止它们相互影响。

对于防病毒程序，我们建议您排除下列文件和位置︰

- C:\Program 该 Azure 恢复服务 Agent\bin\cbengine.exe 作为进程
- C:\Program 该 Azure 恢复服务 Agent\ 文件夹
- 临时位置 （如果您没有使用标准的位置）

<a id="cause3"></a>
## <a name="cause-backup-agent-running-on-an-azure-virtual-machine"></a>原因︰ 在 Azure 的虚拟机上运行的备份代理

如果您要在虚拟机上运行备份代理，性能将低于时物理计算机上运行它。 这被预期由于 IOPS 的限制。  但是，您可以通过切换到 Azure 高级存储备份的数据驱动器来优化性能。 我们正在解决此问题，并会在将来的发行版中可用的修复。

<a id="cause4"></a>
## <a name="cause-backing-up-a-large-number-millions-of-files"></a>原因︰ 备份文件大数量 （百万）

移动的大量数据将需要较长时间比移动更小的卷的数据。 在某些情况下，备份时间与不仅数据，而且还数的文件或文件夹的大小。 数以百万计的小文件 （有几个字节到几个千字节为单位） 正在备份时更是如此。

出现此现象是因为当您要备份的数据并将其移动到 Azure，Azure 是同时您对文件进行分类。 在某些罕见的情况下，目录操作可能需要花费比预期更长的时间。

以下指标可以帮助您了解瓶颈，并相应地处理接下来的步骤︰

- **用户界面显示的用于数据传输的进度**。 仍在传输数据。 网络带宽或数据的大小可能会导致延迟。

- **用户界面不显示用于数据传输的进度**。 打开日志位于 C:\Microsoft Azure 恢复服务 Agent\Temp，并检查日志中的 FileProvider::EndData 条目。 此项表示，数据传输完成后，该目录操作正在发生的情况。 不取消备份作业。 相反，等待更长时间以完成目录操作。 如果问题仍然存在，请联系[Azure 支持](https://portal.azure.com/#create/Microsoft.Support)。
