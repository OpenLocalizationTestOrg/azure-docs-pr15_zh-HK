<properties
   pageTitle="恢复服务保险常见问题 |Microsoft Azure"
   description="此版本的常见问题支持 Azure 备份服务的公共预览版本。 有关备份代理程序、 备份和保留、 恢复、 安全和其他常见问题的 Azure 备份解决方案的常见问题的解答。"
   services="backup"
   documentationCenter=""
   authors="markgalioto"
   manager="jwhit"
   editor=""
   keywords="备份解决方案;备份服务"/>

<tags
   ms.service="backup"
   ms.workload="storage-backup-recovery"
     ms.tgt_pltfrm="na"
     ms.devlang="na"
     ms.topic="get-started-article"
     ms.date="10/21/2016"
     ms.author="trinadhk; markgal; jimpark;"/>

# <a name="recovery-services-vault---faq"></a>恢复服务存储库-常见问题解答


本文提供了特定于恢复服务存储库的信息，它补充了[Azure 备份的常见问题解答](backup-azure-backup-faq.md)。 Azure 备份常见问题提供全套的问题及解答有关 Azure 备份服务。  

关于这篇文章或相关的文章 Disqus 部分 Azure 备份，可以提出问题。 您还可以在[论坛](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazureonlinebackup)中过帐 Azure 备份服务有关的问题。

## <a name="recovery-services-vaults-are-resource-manager-based-are-backup-vaults-classic-mode-still-supported-br"></a>恢复服务存储库是基于资源管理器。 备份存储库 （经典模式） 仍然支持？ <br/>
是，仍然支持备份存储库。 在[经典门户](https://manage.windowsazure.com)创建备份存储库。 在[Azure 的门户网站](https://portal.azure.com)中创建恢复服务电子仓库。 但是我们强烈建议您为所有未来的功能增强创建恢复服务存储库将仅在恢复服务存储库中可用。

## <a name="can-i-migrate-a-backup-vault-to-a-recovery-services-vault-br"></a>可以将备份存储库迁移到恢复服务存储库？ <br/>
遗憾的是没有，这次不能迁移备份存储库的内容到恢复服务存储库。 我们正在添加这一功能，但不能作为公共预览的一部分。

## <a name="do-recovery-services-vaults-support-classic-vms-or-resource-manager-based-vms-br"></a>不要恢复服务存储库支持传统的 Vm 或资源管理器基于虚拟机？ <br/>
恢复服务存储库支持这两种模型。  您可以备份在经典门户 （它们是经典模式的虚拟机），创建一个虚拟机，或到恢复服务存储库在 Azure 门户 （它们是基于资源管理器） 中创建虚拟机。

## <a name="i-have-backed-up-my-classic-vms-in-backup-vault-now-i-want-to-migrate-my-vms-from-classic-mode-to-resource-manager-mode--how-can-i-backup-them-in-recovery-services-vault"></a>我已经我经典 Vm 备份存储库中。 现在我想将我的虚拟机从经典模式迁移到资源管理器模式。  恢复服务存储库中，如何可以备份它们？
传统备份的存储库中的虚拟机的备份不会自动向迁移恢复服务存储库从经典的虚拟机迁移到资源管理器模式时。 请按照以下步骤为迁移 VM 备份︰

1. 在备份存储库，转到**受保护的项**选项卡，选择虚拟机。 单击[停止保护](backup-azure-manage-vms-classic.md#stop-protecting-virtual-machines)。 不*删除关联的备份数据*选项**未选中**。
2. 从经典模式的虚拟机迁移到资源管理器模式。 请确保存储和网络对应于虚拟机也迁移到资源管理器模式。
3. 创建恢复服务存储库和配置迁移虚拟机使用**备份**操作在存储库的仪表板上的备份。 了解如何[启用备份恢复服务存储库中](backup-azure-vms-first-look-arm.md)的详细信息
