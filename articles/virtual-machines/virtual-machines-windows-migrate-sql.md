<properties
    pageTitle="迁移到 SQL Server 在 VM 上的 SQL Server 数据库 |Microsoft Azure"
    description="了解如何将本地用户数据库迁移到 SQL Server，Azure 的虚拟机中。"
    services="virtual-machines-windows"
    documentationCenter=""
    authors="sabotta"
    manager="jhubbard"
    editor=""
    tags="azure-service-management" />
<tags
    ms.service="virtual-machines-windows"
    ms.workload="infrastructure-services"
    ms.tgt_pltfrm="vm-windows-sql-server"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/26/2016"
    ms.author="carlasab"/>


# <a name="migrate-a-sql-server-database-to-sql-server-in-an-azure-vm"></a>将 SQL Server 数据库迁移到 SQL Server 在 Azure VM

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]资源管理器模型。


有许多的方法迁移到 Azure VM 中的 SQL Server 的一个本地 SQL Server 的用户数据库。 本文将简要讨论的各种方法、 建议的最佳方法对于各种方案，并包括[教程](#azure-vm-deployment-wizard-tutorial)来指导您通过使用**部署到 Microsoft Azure VM SQL Server 数据库**向导。 

使用**部署到 Microsoft Azure VM SQL Server 数据库**向导[教程](#azure-vm-deployment-wizard-tutorial)中介绍的方法是只经典的部署模型。 

## <a name="what-are-the-primary-migration-methods"></a>主迁移方法是什么？

主迁移方法如下︰

- 使用部署 SQL Server 数据库到 Azure 的 Microsoft VM 向导
- 执行内部使用压缩的备份并将备份文件手动复制到 Azure 的虚拟机
- 执行从 URL URL 和到 Azure 的虚拟机还原到备份
- 分离，然后将数据和日志文件复制到 Azure blob 存储，然后将附加到 Azure VM 中的 SQL Server 从 URL
- 将内部物理机转换到 Hyper-V VHD 上载到 Azure Blob 存储，然后部署新虚拟机使用上载 VHD
- 提供使用导入/导出的 Windows 服务的硬驱
- 如果您有 AlwaysOn 部署部署、[添加 Azure 复制副本向导](virtual-machines-windows-classic-sql-onprem-availability.md)用于在 Azure 然后故障转移，用户指向 Azure 数据库实例中创建复制副本
- 使用 SQL Server[事务复制](https://msdn.microsoft.com/library/ms151176.aspx)将 Azure SQL Server 实例配置为订阅服务器，然后禁用复制，用户指向 Azure 数据库实例



## <a name="choosing-your-migration-method"></a>选择迁移方法

为获得最佳的数据传输性能，数据库文件迁移到 Azure VM 使用的压缩备份文件通常是最佳方法。 这是[部署到 Microsoft Azure 虚拟机向导的 SQL Server 数据库](#azure-vm-deployment-wizard-tutorial)使用的方法。 此向导将迁移内部用户数据库运行的 SQL Server 2005 或 SQL Server 2014年到更大或更高版本小于 1 TB 压缩的数据库备份文件时的推荐的方法。

若要在数据库迁移过程中减少停工时间，使用 AlwaysOn 选项或事务复制选项。

如果不能使用上述方法，手动迁移数据库。 使用此方法，您开始时通常都会跟一份数据库备份与数据库备份到 Azure，然后执行数据库恢复。 可以也将自己的数据库文件复制到 Azure，然后将它们连接。 通过它可以完成手动将数据库迁移到 Azure VM 的过程有以下几种方法。

> [AZURE.NOTE] 当您升级到 SQL Server 2014年或 SQL Server 2016年旧版本的 SQL Server 时，您应考虑是否需要更改。 我们建议您解决所有依赖功能不支持新版本的 SQL Server 作为迁移项目的一部分。 支持的版本和方案的详细信息，请参阅[升级到 SQL Server](https://msdn.microsoft.com/library/bb677622.aspx)。

下表列出了每个主迁移的方法，并讨论了当使用每一种方法是最合适的。

| 方法  | 源数据库版本  |  目标数据库版本 | 源数据库备份的大小约束  | 备注  |
|---|---|---|---|---|
| [使用部署 SQL Server 数据库到 Azure 的 Microsoft VM 向导](#azure-vm-deployment-wizard-tutorial) | SQL Server 2005 或更高版本 | SQL Server 2014年或更高版本 | < 1 TB  | 最快和最简单的方法，只要有可能，将迁移到新的或现有的 SQL Server 实例在 Azure 的虚拟机中的使用 | 
| [使用添加 Azure 复制副本向导](virtual-machines-windows-classic-sql-onprem-availability.md) | SQL Server 2012年或更高版本 | SQL Server 2012年或更高版本 | [Azure VM 存储限制](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | 最小化停机时间，使用 AlwaysOn 到内部部署后 |
| [使用 SQL Server 事务复制](https://msdn.microsoft.com/library/ms151176.aspx) | SQL Server 2005 或更高版本 | SQL Server 2005 或更高版本 | [Azure VM 存储限制](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | 使用时您需要最小化停机时间，没有 AlwaysOn 内部部署 |
| [执行内部使用压缩的备份并将备份文件手动复制到 Azure 的虚拟机](#backup-to-file-and-copy-to-vm-and-restore) | SQL Server 2005 或更高版本 | SQL Server 2005 或更高版本 | [Azure VM 存储限制](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | 仅可使用时不能使用向导，如当目标数据库版本低于 SQL Server 2012 SP1 CU2 或数据库备份大小大于 1 TB (与 SQL Server 2016年 12.8 t B) |
| [执行从 URL URL 和到 Azure 的虚拟机还原到备份](#backup-to-url-and-restore) | SQL Server 2012 SP1 CU2 或更高版本 | SQL Server 2012 SP1 CU2 或更高版本 | < 12.8 t B SQL Server 2016 年，否则为 < 个 1 TB | 通常使用[备份到 URL](https://msdn.microsoft.com/library/dn435916.aspx)是性能与使用向导中的等效，并不像今天这么简单 |
| [分离，然后将数据和日志文件复制到 Azure blob 存储，然后将附加到 SQL Server Azure 的虚拟机中从 URL](#detach-and-copy-to-url-and-attach-from-url) | SQL Server 2005 或更高版本 | SQL Server 2014年或更高版本 | [Azure VM 存储限制](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | 到[存储这些文件使用 Azure Blob 存储服务](https://msdn.microsoft.com/library/dn385720.aspx)计划并将它们附加到 SQL Server 在 Azure VM，特别是具有非常大的数据库中运行时使用此方法 |
| [转换到 Hyper-V Vhd 的内部计算机上载到 Azure Blob 存储，然后部署新的虚拟机使用上载的 VHD](#convert-to-vm-and-upload-to-url-and-deploy-as-new-vm) | SQL Server 2005 或更高版本 | SQL Server 2005 或更高版本 | [Azure VM 存储限制](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | 当依赖于其他的用户数据库和/或系统数据库的数据库的迁移过程中组合在一起[使 SQL Server 许可证](../sql-database/sql-database-paas-vs-sql-server-iaas.md)，迁移迁移系统和用户时，将运行早期版本的 SQL Server 或数据库时数据库时使用。 |
| [提供使用导入/导出的 Windows 服务的硬驱](#ship-hard-drive) | SQL Server 2005 或更高版本 | SQL Server 2005 或更高版本 | [Azure VM 存储限制](https://azure.microsoft.com/documentation/articles/azure-subscription-service-limits/) | 在手动复制方法速度太慢，如具有非常大的数据库时使用[的导入/导出 Windows 服务](../storage/storage-import-export-service.md) |

## <a name="azure-vm-deployment-wizard-tutorial"></a>Azure VM 部署向导教程

使用 Microsoft SQL Server 管理 Studio 中**部署 Microsoft Azure VM 到 SQL Server 数据库**向导迁移 SQL Server 2005，SQL Server 2008年，SQL Server 2008 R2、 SQL Server 2012年，SQL Server 2014 年或 SQL Server 2016年内部用户数据库 (达 1 TB) SQL Server 2014年或 SQL Server 2016年到 Azure 的虚拟机中。 使用此向导可以将用户数据库迁移到 Azure 现有虚拟机或在迁移过程中由向导创建的 SQL Server 使用 Azure VM。 当您将数据库迁移到较新版本的 SQL Server 时，数据库会自动升级的过程。

该方法是只经典的部署模型。 

### <a name="get-latest-version-of-the-deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard"></a>获取最新版本的部署 SQL Server 数据库到 Azure 的 Microsoft VM 向导

使用最新版本的 Microsoft SQL Server 管理的 SQL Server 的 Studio 以确保您具有最新版本的**SQL Server 数据库给 Microsoft Azure VM 部署**向导。 此向导的最新版本合并到 Azure 的传统门户网站最新的更新，并在库中支持最新的 Azure VM 映像 （向导的较早版本可能不起作用）。 若要获得 SQL Server，[下载](http://go.microsoft.com/fwlink/?LinkId=616025)最新版本的 Microsoft SQL Server 管理工作室，与您计划对迁移和互联网的数据库连接的客户端计算机上安装它。

### <a name="configure-the-existing-azure-virtual-machine-and-sql-server-instance-if-applicable"></a>配置现有 Azure 的虚拟机和 SQL Server 实例 （如果适用）

如果您正在迁移到现有的 Azure VM，下列配置步骤是必需的︰

- 配置 Azure VM 和 SQL Server 实例，可以通过[连接到 SQL Server 虚拟机实例，从另一台计算机上的 SSMS](virtual-machines-windows-sql-connect.md)到下面的步骤来启用从另一台计算机的连接。 如果您正在迁移使用向导，支持仅 SQL Server 2014年和 SQL Server 2016年图像库中。
- 专用端口 11435 Microsoft Azure 网关配置打开 SQL Server 云适配器服务终结点。 此端口来创建 SQL Server 2014年或 SQL Server 2016年资源调配 Microsoft Azure VM 的一部分。 此外将云适配器创建 Windows 防火墙规则以允许在默认端口 11435 其传入的 TCP 连接。 此终结点使向导能够利用云适配器服务将备份文件复制到 Azure VM 内部部署的实例。 有关详细信息，请参阅[SQL Server 的云适配器](https://msdn.microsoft.com/library/dn169301.aspx)。

    ![创建云适配器终结点](./media/virtual-machines-windows-migrate-sql/cloud-adapter-endpoint.png)

### <a name="run-the-use-the-deploy-a-sql-server-database-to-a-microsoft-azure-vm-wizard"></a>运行到 Azure 的 Microsoft VM 向导部署 SQL Server 数据库的使用

1. 对于 Microsoft SQL Server 2016年打开 Microsoft SQL Server 管理 Studio 并连接到包含您将迁移到 Azure VM 的用户数据库的 SQL Server 实例。
2. 右击您要迁移的数据库，指向任务，然后单击部署到 Microsoft Azure VM。

    ![启动向导](./media/virtual-machines-windows-migrate-sql/start-wizard.png)

3. 在简介页上，单击下一步。
4. 在源设置页中，将连接到包含要迁移到 Azure VM 的数据库的 SQL Server 实例。
5. 指定备份文件的临时位置。 如果连接到远程服务器，则必须指定一个网络驱动器。

    ![源设置](./media/virtual-machines-windows-migrate-sql/source-settings.png)

6. 单击下一步。
7. 在 Microsoft Azure 登录页上，单击登录，登录到您的 Azure 帐户。
8. 选择您想要使用并单击下一步该订阅。

    ![Azure 签到](./media/virtual-machines-windows-migrate-sql/azure-signin.png)

9. 在部署设置页面中，您可以指定一个新的或现有的云服务名称和虚拟机的名称︰

 - 指定一个新的云服务名称和虚拟机名称，以与新 Azure 的虚拟机使用的 SQL Server 2014年或 SQL Server 2016年库图像创建新的云服务。

     - 如果您指定一个新的云服务名称，指定将使用的存储帐户。

     - 如果指定了现有的云服务名称，将检索存储帐户，并为您输入。

 - 指定现有的云服务名称和在现有的云服务中创建新的 Azure 虚拟机的新虚拟机名称。 只能指定一个 SQL Server 2014年或 SQL Server 2016年库图像。
 - 指定现有的云服务名称和要使用 Azure 现有虚拟机的虚拟机名称。 这必须使用 SQL Server 2014年或 SQL Server 2016年的库图像生成的图像。

        ![Deployment Settings](./media/virtual-machines-windows-migrate-sql/deployment-settings.png)

10. 单击设置
  - 如果指定了现有的云服务名称和虚拟机的名称，系统会提示您提供用户名和密码。

        ![Azure 的计算机设置](./media/virtual-machines-windows-migrate-sql/azure-machine-settings.png)

    - 如果您指定一个新的虚拟机名称，将提示您从库图像列表选择一个图像，并提供以下信息︰
      - 图像--仅选择 SQL Server 2014年或 SQL Server 2016
        - 用户名
        - 新密码
        - 确认密码
        - 位置
        - 大小。
    - 此外，单击接受自行生成的证书对此新 Microsoft Azure 的虚拟机，然后单击确定。

    ![Azure 新机设置](./media/virtual-machines-windows-migrate-sql/azure-new-machine-settings.png)

11. 如果源数据库名称不同，请指定目标数据库名称。 如果目标数据库不存在，系统将自动增加数据库的名称，而不覆盖现有的数据库。
12. 单击下一步，然后单击完成。

    ![结果](./media/virtual-machines-windows-migrate-sql/results.png)

13. 向导完成后，将连接到您的虚拟机，并验证已迁移的数据库。
14. 如果您创建新的虚拟机，通过[连接到 SQL Server 虚拟机实例，从另一台计算机上的 SSMS](virtual-machines-windows-sql-connect.md)到下面的步骤配置 Azure 的虚拟机和 SQL Server 实例。

## <a name="backup-to-file-and-copy-to-vm-and-restore"></a>备份到文件，然后复制到虚拟机和恢复

使用此方法时不能使用部署 SQL Server 数据库到 Azure 的 Microsoft VM 向导或者因为要迁移到 SQL Server 2014年前的 SQL Server 版本，或者备份文件大于 1 TB。 如果您的备份文件大于 1 TB，必须带它区，因为虚拟机磁盘的最大大小为 1 TB。 使用下列常规步骤要迁移的用户数据库，使用手动的方法︰

1.  执行完整数据库备份到本地位置。
2.  创建或上载虚拟机所需的 SQL Server 版本。
3.  安装程序根据您的要求的连接。 请参阅[连接到 SQL Server 虚拟机在 Azure （资源经理） 上](virtual-machines-windows-sql-connect.md)。
4.  将您的备份文件复制到远程桌面、 Windows 资源管理器或复制命令在命令提示符下的使用 VM。

## <a name="backup-to-url-and-restore"></a>对 URL 和还原备份

时不能使用部署到 Microsoft Azure 虚拟机向导的 SQL Server 数据库，因为备份文件大于 1 TB 和您正在迁移和 SQL Server 2016年到使用[备份到 URL](https://msdn.microsoft.com/library/dn435916.aspx)的方法。 对于小于 1 TB 或运行版本的 SQL Server 2016年前的 SQL Server 数据库，建议使用该向导。 与 SQL Server 2016，条带化备份集支持，包括为性能，建议使用以及需超过每 blob 的大小限制。 对于非常大的数据库，建议使用[Windows 导入/导出服务](../storage/storage-import-export-service.md)。

## <a name="detach-and-copy-to-url-and-attach-from-url"></a>分离到的 URL 复制并从 URL 附加

到[存储这些文件使用 Azure Blob 存储服务](https://msdn.microsoft.com/library/dn385720.aspx)计划并将它们附加到 SQL Server 在 Azure VM，特别是具有非常大的数据库中运行时，请使用此方法。 使用下列常规步骤要迁移的用户数据库，使用手动的方法︰

1.  分离数据库文件从本地数据库实例。
2.  已分离的数据库文件复制到 Azure blob 存储使用[AZCopy 命令行实用程序](../storage/storage-use-azcopy.md)。
3.  将数据库文件从 Azure URL 附加到 Azure VM 中的 SQL Server 实例。

## <a name="convert-to-vm-and-upload-to-url-and-deploy-as-new-vm"></a>转换为虚拟机和上载到 URL 并将部署为新的虚拟机

使用此方法可将本地 SQL Server 实例中的所有系统和用户数据库都迁移到 Azure 的虚拟机。 使用下列常规步骤迁移整个 SQL Server 实例使用手动的方法︰

1.  通过使用[Microsoft 虚拟机转换器](http://technet.microsoft.com/library/dn873998.aspx)到 Hyper-V Vhd 转换的物理计算机或虚拟机。
2.  通过[添加 AzureVHD cmdlet](https://msdn.microsoft.com/library/windowsazure/dn495173.aspx)到 Azure 存储上载 VHD 文件。
3.  使用上载的 VHD 部署新的虚拟机。

> [AZURE.NOTE] 若要迁移整个应用程序，请考虑使用[Azure 站点恢复](../site-recovery/site-recovery-overview.md)]。

## <a name="ship-hard-drive"></a>装运的硬驱

使用[导入/导出的 Windows 服务的方法](../storage/storage-import-export-service.md)将大量文件数据传输到 Azure Blob 存储在的情况下通过网络上传价格过高或不可行。 通过此服务，您可以发送包含该数据到 Azure 数据中心，会将您的数据上载到您的存储帐户的一个或多个硬驱。

## <a name="next-steps"></a>下一步行动

有关运行 SQL Server 在 Azure 的虚拟机的详细信息，请参阅[SQL Server 在 Azure 虚拟机概述](virtual-machines-windows-sql-server-iaas-overview.md)。

创建捕获映像从 Azure SQL Server 虚拟机的说明，请参见 CSS SQL Server 工程师博客上的[提示和技巧上 '克隆' SQL Azure 的虚拟机从捕捉到的图像](https://blogs.msdn.microsoft.com/psssql/2016/07/06/tips-tricks-on-cloning-azure-sql-virtual-machines-from-captured-images/)。
