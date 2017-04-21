<properties
    pageTitle="介绍 Azure DPM 备份 |Microsoft Azure"
    description="备份 DPM 服务器使用 Azure 备份服务简介"
    services="backup"
    documentationCenter=""
    authors="Nkolli1"
    manager="shreeshd"
    editor=""
    keywords="系统中心 Data Protection Manager，数据保护管理器、 dpm 备份"/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/21/2016"
    ms.author="trinadhk;giridham;jimpark;markgal"/>

# <a name="preparing-to-back-up-workloads-to-azure-with-dpm"></a>准备到 Azure 使用 DPM 备份工作负载

> [AZURE.SELECTOR]
- [Azure 的备份服务器](backup-azure-microsoft-azure-backup.md)
- [SCDPM](backup-azure-dpm-introduction.md)
- [Azure 备份服务器 （传统）](backup-azure-microsoft-azure-backup-classic.md)
- [SCDPM （传统）](backup-azure-dpm-introduction-classic.md)


本文介绍了使用 Microsoft Azure 备份来保护您的系统中心 Data Protection Manager (DPM) 服务器和工作负载。 通过阅读它，您就会明白︰

- Azure DPM 服务器备份的工作原理
- 系统必备组件以实现平滑的备份体验
- 遇到的典型错误，以及如何处理它们
- 受支持的方案

System Center DPM 备份的文件和应用程序数据。 可以存储在磁盘上的磁带，或备份到 Azure 使用 Microsoft Azure 备份到 DPM 备份数据。 DPM 交互使用 Azure 的备份︰

- **作为物理服务器或内部部署虚拟机部署 DPM** — 如果 DPM 部署作为物理服务器或您可以备份到磁盘和磁带备份 Azure 存储库数据内部部署 Hyper-V 虚拟机备份。
- **DPM Azure 的虚拟机作为部署**— — 从与更新 3 System Center 2012 R2，DPM 可以部署为 Azure 的虚拟机。 如果 DPM 将部署为 Azure 的虚拟机，您可以将数据备份到 Azure 的磁盘连接到 DPM Azure 虚拟机，或者能够将数据存储到 Azure 备份存储库支持它的大量分流。

## <a name="why-backup-your-dpm-servers"></a>为什么备份 DPM 服务器？

使用 Azure 备份进行备份 DPM 服务器的业务好处包括︰

- 对于内部 DPM 部署，可以使用 Azure 备份作为长期部署到磁带的替代方法。
- 对于 DPM Azure 中的部署，Azure 备份允许您卸载从 Azure 的磁盘上的存储使您能够通过将较旧的数据存储在 Azure 备份和磁盘上的新数据来扩大规模。

## <a name="how-does-dpm-server-backup-work"></a>DPM 服务器的备份是如何工作的？
若要备份一个虚拟机，第一次数据的时间点快照需要。 备份 Azure 服务计划时，将启动备份作业并触发要拍摄快照的备份扩展名。 备份扩展名与访客 VSS 服务以实现一致性，协调并调用 Azure 存储服务的 blob 快照 API，一旦达到一致性。 这样做是为了获得一个一致的快照的磁盘的虚拟机，而无需将其关闭。

在拍摄快照后，数据由备份 Azure 服务传输到备份存储区中。 该服务负责识别和传输仅从上次备份进行备份存储和网络效率已更改的块。 数据传输完成后，快照将被删除并创建一个恢复点。 Azure 的传统门户网站中，可以看到该恢复点。

>[AZURE.NOTE] 对于 Linux 虚拟机，只有文件一致的备份是可能的。

## <a name="prerequisites"></a>系统必备组件
准备 Azure 备份来备份 DPM 数据，如下所示︰

1. **创建备份存储库**— — 在 Azure 备份控制台中创建一个电子仓库。
2. **下载存储库凭据**— 在 Azure 备份中，上载到存储库创建的管理证书。
3. **Azure 备份代理程序安装和注册服务器**— 从 Azure 备份每个 DPM 服务器上安装代理和 DPM 服务器注册到备份存储区中。

[AZURE.INCLUDE [backup-create-vault](../../includes/backup-create-vault.md)]

[AZURE.INCLUDE [backup-download-credentials](../../includes/backup-download-credentials.md)]

[AZURE.INCLUDE [backup-install-agent](../../includes/backup-install-agent.md)]


## <a name="requirements-and-limitations"></a>要求 （和限制）

- 作为物理服务器或 System Center 2012 SP1 或 System Center 2012 R2 上安装 Hyper-V 虚拟机，可以运行 DPM。 此外可以为 Azure 至少在 System Center 2012 R2 与上运行的虚拟机运行 DPM 2012 R2 更新汇总 3年或 Windows 虚拟机在 VMWare 上 System Center 2012 R2 具有至少运行更新汇总 5。
- 如果您在使用 System Center 2012 SP1 运行 DPM，您应系统中心 Data Protection Manager SP1 的安装更新汇总 2。 这是必需的才可以安装 Azure 备份代理。
- DPM 服务器应具有 Windows PowerShell 和.Net Framework 4.5 安装。
- DPM 可以备份到 Azure 备份大多数工作负荷。 有关什么支持查看完整列表 Azure 备份支持下列项目。
- 使用"复制到磁带"选项将无法恢复 Azure 备份中存储的数据。
- 您需要 Azure 帐户启用 Azure 备份功能。 如果您没有帐户，您可以在几分钟创建免费的试用帐户。 阅读有关[Azure 备份定价](https://azure.microsoft.com/pricing/details/backup/)。
- 使用 Azure 备份要求 Azure 备份代理安装在要备份的服务器上。 每台服务器必须有至少 10%的备份，可用作本地可用存储的数据的大小。 例如，备份 100 GB 的数据最少需要 10 GB 的可用空间中的空闲位置。 最小值是 10%，而 15%的可用的本地存储空间，用于缓存位置的建议。
- 数据将存储在 Azure 存储库存储。 您可以备份到 Azure 备份保险存储的数据量没有限制，但数据源 （例如虚拟机或数据库） 的大小不应超过 54,400 GB。

备份到 Azure 支持这些文件类型︰

- 加密 （仅适用于完整备份）
- 压缩 （支持增量备份）
- 稀疏 （支持增量备份）
- 压缩和稀疏 （视为稀疏）

并且，这些不受支持︰

- 不支持区分大小写的文件系统上的服务器。
- 硬链接 （跳过）
- 重分析点 （跳过）
- 加密和压缩 （跳过）
- 加密和稀疏 （跳过）
- 压缩的流
- 稀疏流

>[AZURE.NOTE] 从在 System Center 2012 DPM 使用 SP1 开始，您可以备份到 Azure 使用 Microsoft Azure 备份 DPM 保护的工作负载。
