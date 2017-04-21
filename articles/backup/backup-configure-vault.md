<properties
    pageTitle="备份 Windows 服务器或客户端到 Azure 使用 Azure 备份使用的资源管理器部署模型 |Microsoft Azure"
    description="备份 Windows 服务器或客户端到 Azure 创建备份的存储库，下载凭据、 安装备份代理程序，并完成初始文件和文件夹的备份。"
    services="backup"
    documentationCenter=""
    authors="markgalioto"
    manager="cfreeman"
    editor=""
    keywords="备份存储库;备份 Windows 服务器;备份窗口;"/>

<tags
    ms.service="backup"
    ms.workload="storage-backup-recovery"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="08/10/2016"
    ms.author="jimpark; trinadhk; markgal"/>

# <a name="back-up-a-windows-server-or-client-to-azure-using-the-resource-manager-deployment-model"></a>Windows 服务器或客户机备份到 Azure 使用资源管理器部署模型

> [AZURE.SELECTOR]
- [Azure 门户](backup-configure-vault.md)
- [传统的门户网站](backup-configure-vault-classic.md)

本文介绍如何备份 Windows 服务器 （或 Windows 客户端） 的文件和文件夹到 Azure 使用 Azure 备份使用的资源管理器部署模型。

[AZURE.INCLUDE [learn-about-deployment-models](../../includes/backup-deployment-models.md)]

![备份过程步骤](./media/backup-configure-vault/initial-backup-process.png)


## <a name="before-you-start"></a>在开始之前
要到 Azure 备份服务器或客户端，您需要一个 Azure 帐户。 如果您没有一个，您可以在几分钟创建一个[免费帐户](https://azure.microsoft.com/free/)。

## <a name="step-1-create-a-recovery-services-vault"></a>步骤 1︰ 创建恢复服务存储库

恢复服务存储库是存储的所有备份和恢复点创建随时间的实体。 恢复服务存储库还包含应用于受保护的文件和文件夹的备份策略。 当您创建恢复服务存储库时，您还应选择适当的存储冗余选项。

### <a name="to-create-a-recovery-services-vault"></a>要创建恢复服务存储库

1. 如果您还没有登录到[Azure 门户](https://portal.azure.com/)使用 Azure 订购的这么做。

2. 中心的菜单上，单击**浏览**，在资源的列表中，键入**恢复服务**。 在您开始键入时，将筛选列表根据您的输入。 单击**恢复服务电子仓库**。

    ![创建恢复服务存储库步骤 1](./media/backup-configure-vault/browse-to-rs-vaults.png) <br/>

    恢复服务存储库列表中的显示。

3. 在**恢复服务存储库**菜单上，单击**添加**。

    ![创建恢复服务存储库步骤 2](./media/backup-configure-vault/rs-vault-menu.png)

    恢复服务存储库刀片式服务器将打开，并提示您提供的**名称**、**订阅**、**资源组**和**位置**。

    ![创建恢复服务存储库步骤 5](./media/backup-configure-vault/rs-vault-attributes.png)

4. 对于**名称**，输入好记的名称来标识该存储库。 名称必须是唯一的 Azure 的订阅。 键入包含 2 至 50 个字符的名称。 它必须以字母开头，可以包含字母、 数字和连字符。

5. 单击以查看可用的订阅的**订阅**。 如果您不能确定要使用哪种订阅，使用默认值 （或建议） 订阅。 将多个选项，如果您组织的帐户与多个 Azure 订阅关联。

6. 单击**资源组**来查看可用资源组列表，或单击**新建**以创建新的资源组。 有关资源组的完整信息，请参阅[Azure 资源管理器概述](../azure-resource-manager/resource-group-overview.md)

7. 单击以选择该存储库的地理区域的**位置**。 此选择确定的地理区域发送备份数据的位置。 通过选择您所在的位置靠近一个地理区域，您可以备份到 Azure 时减少网络延迟。

8. 单击**创建**。 它可能需要一段时间的恢复服务存储库创建。 监视在门户上方右侧区域中的状态通知。 一旦创建了您的存储库，则应打开在门户中。 如果您看不到已完成之后，列出您的存储库，请单击**刷新**。 当刷新列表时，请单击电子仓库的名称。

### <a name="to-determine-storage-redundancy"></a>要确定存储冗余
当您首次创建恢复服务存储库您确定如何复制存储。

1. 在**设置**刀片式服务器，它将自动打开与您的保险存储仪表板，请单击**备份基础结构**。

2. 在备份基础架构刀片式服务器，请单击要查看**存储复制类型**的**备份配置**。

    ![创建恢复服务存储库步骤 5](./media/backup-configure-vault/backup-infrastructure.png)

3. 选择您的存储库存储复制选项。

    ![恢复列表服务存储库](./media/backup-configure-vault/choose-storage-configuration.png)

    默认情况下，您的存储库具有地理冗余存储。 如果您使用 Azure 作为主备份存储终结点，则继续使用地理冗余存储。 如果您使用 Azure 作为非主要备份存储终结点，然后选择本地冗余存储，这将降低在 Azure 存储数据的成本。 阅读更多关于[地理冗余](../storage/storage-redundancy.md#geo-redundant-storage)和本[概述](../storage/storage-redundancy.md)中的[本地冗余](../storage/storage-redundancy.md#locally-redundant-storage)存储选项。

    在选择了您的存储库的存储选项之后, 您就可以将您的文件和文件夹与存储库相关联。

现在，您已经创建了一个电子仓库，您准备您的基础架构，通过下载和安装 Microsoft Azure 恢复服务代理、 下载存储库凭据，然后使用这些凭据存储库中注册代理备份文件和文件夹。

## <a name="step-2---download-files"></a>步骤 2-下载文件

>[AZURE.NOTE] 通过 Azure 门户启用备份即将登场。 此时，您可以使用内部部署 Microsoft Azure 恢复服务代理要备份您的文件和文件夹。

1. 恢复服务存储库操控板上单击**设置**。

    ![打开备份目标刀片式服务器](./media/backup-configure-vault/settings-button.png)

2. 单击**入门 > 备份**设置刀片式服务器上。

    ![打开备份目标刀片式服务器](./media/backup-configure-vault/getting-started-backup.png)

3. 单击备份刀片式服务器上的**备份目标**。

    ![打开备份目标刀片式服务器](./media/backup-configure-vault/backup-goal.png)

4. 选择**内部**从何处是运行工作负载？菜单。

5. 选择**文件和文件夹**从您做什么要备份？菜单上，单击**确定**。

#### <a name="download-the-recovery-services-agent"></a>下载恢复服务代理

1. 单击**下载 Windows 服务器的代理程序或 Windows 客户端****准备基础结构**刀片式服务器中。

    ![准备基础结构](./media/backup-configure-vault/prepare-infrastructure-short.png)

2. 在下载弹出窗口中，单击**保存**。 默认情况下， **MARSagentinstaller.exe**文件保存到您的下载文件夹。

#### <a name="download-vault-credentials"></a>下载凭据存储库

1. 单击**下载 > 保存**准备基础结构刀片式服务器上。

    ![准备基础结构](./media/backup-configure-vault/prepare-infrastructure-download.png)

## <a name="step-3--install-and-register-the-agent"></a>步骤 3-安装和注册代理

1. 找到并双击**MARSagentinstaller.exe**从下载文件夹 （或其他保存的位置）。

2. 完成 Microsoft Azure 恢复服务代理安装向导。 要完成此向导，您需要︰

    - 选择安装和缓存文件夹的位置。
    - 如果您使用代理服务器连接到互联网，提供代理服务器的信息。
    - 如果您在使用经过身份验证的代理服务器，提供您的用户名用户名和密码的详细信息。
    - 提供下载存储库凭据
    - 将加密的密码保存在安全的位置。

    >[AZURE.NOTE] 如果您丢失或忘记了密码短语时，Microsoft 不能帮助恢复备份的数据。 请将文件保存在安全的位置。 它需要还原的备份。

现在安装代理，您的计算机注册到该存储库。 您可以配置并计划备份。

### <a name="confirm-the-installation"></a>确认安装

确认代理已安装并正确注册，您可以检查备份管理门户的**生产服务器**部分中的项目。 若要此操作︰

1. 登录到[Azure 门户](https://portal.azure.com/)使用 Azure 订购。

2. 中心的菜单上，单击**浏览**，在资源的列表中，键入**恢复服务**。 在您开始键入时，将筛选列表根据您的输入。 单击**恢复服务电子仓库**。

    ![创建恢复服务存储库步骤 1](./media/backup-configure-vault/browse-to-rs-vaults.png) <br/>

    恢复服务存储库列表中的显示。

2. 选择您创建的电子仓库的名称。

    恢复服务存储库仪表盘刀片式服务器打开。

    ![恢复服务存储库面板](./media/backup-configure-vault/rs-vault-dashboard.png) <br/>

3. 单击页面顶部的**设置**按钮。

4. 单击**备份基础架构 > 生产服务器**。

    ![生产服务器](./media/backup-configure-vault/production-server-verification.png)

如果您看到您的服务器列表中，则可以确认已安装并正确注册代理。

## <a name="step-4-complete-the-initial-backup"></a>第 4 步︰ 完成初始备份

初始备份包括两个主要任务︰

- 计划备份
- 第一次备份的文件和文件夹

若要完成初始备份，您可以使用 Microsoft Azure 备份代理。

### <a name="to-schedule-the-backup"></a>安排备份

1. 打开 Microsoft Azure 备份代理。 您可以通过搜索**Microsoft Azure 备份**您的计算机中发现。

    ![启动 Azure 备份代理](./media/backup-configure-vault/snap-in-search.png)

2. 在备份代理，请单击**日程安排备份**。

    ![Windows 服务器备份计划](./media/backup-configure-vault/schedule-first-backup.png)

3. 在计划备份向导的入门页面上，单击**下一步**。

4. 在备份页中选择的项目，单击**添加项目**。

5. 选择的文件和您希望备份的文件夹，然后单击**好了**。

6. 单击**下一步**。

7. 在**指定备份计划**页上指定的**备份时间表**并单击**下一步**。

    您可以计划 （速度最大每日三次） 的每日或每周备份。

    ![对于 Windows 服务器备份的项目](./media/backup-configure-vault/specify-backup-schedule-close.png)

    >[AZURE.NOTE] 有关如何指定备份时间表的详细信息，请参阅文章[使用 Azure 备份来替换您的磁带基础结构](backup-azure-backup-cloud-as-tape.md)。

8. 在**选择保留策略**页上，选择备份副本的**保留策略**。

    保留策略指定的持续时间，将为其存储备份。 而不是只指定备份的所有点"平面策略"，您可以指定备份发生时根据不同的保留策略。 您可以修改每日、 每周、 每月和每年的保留策略，以满足您的需要。

9. 在选择初始备份类型页上选择的初始备份类型。 保留**在网络上自动**选择，选项，然后单击**下一步**。

    您可以自动通过网络备份，也可以离线备份。 本文的其余部分介绍了自动备份的过程。 如果您希望执行脱机备份，请查看文章[在 Azure 备份脱机备份工作流](backup-azure-backup-import-export.md)的其他信息。

10. 在确认页上查看信息，，然后单击**完成**。

11. 在向导完成创建备份时间表后，单击**关闭**。

### <a name="enable-network-throttling-optional"></a>启用网络限制 （可选）

备份代理提供网络限制。 限制的控件在数据传输过程中网络带宽的使用方式。 此控件可以是有帮助的如果需要备份数据在工作时间，但不是希望备份过程干扰其他互联网通信。 限制应用于备份和恢复活动。

>[AZURE.NOTE] 网络限制不随 Windows Server 2008 R2 SP1，Windows Server 2008 SP2，或 Windows 7 （service pack)。 Azure 备份网络带宽限制功能雇佣本地操作系统上的服务质量 (QoS)。 尽管 Azure 备份可以保护这些操作系统的系统，QoS 在这些平台上可用的版本不能用于 Azure 备份网络限制。 可以在所有其他[受支持的操作系统](backup-azure-backup-faq.md#installation-amp-configuration)上使用网络带宽限制。

**若要启用网络限制**

1. 在备份代理程序，单击**更改属性**。

    ![更改属性](./media/backup-configure-vault/change-properties.png)

2. 在**带宽限制**选项卡上选择**启用互联网带宽使用率限制备份操作**复选框。

    ![网络限制](./media/backup-configure-vault/throttling-dialog.png)

3. 您已启用了带宽限制后，**工作时间**和**非工作时间的**过程中指定允许备份数据传输带宽。

    带宽值 512 千比特每秒 (Kbps) 开始，然后可以转达 1023 兆字节每秒 (MBps)。 此外可以指定开始和完成**的工作时间**，和一周中的哪些天是考虑的工作天。 小时以外指定工作时间被视为非工作几个小时。

4. 单击**确定**。

### <a name="to-back-up-files-and-folders-for-the-first-time"></a>第一次备份的文件和文件夹

1. 备份代理程序，在单击**立即备份**完成初始种子通过网络。

    ![现在 Windows 服务器备份](./media/backup-configure-vault/backup-now.png)

2. 在确认页上查看备份现在向导将使用备份计算机的设置。 然后单击**备份**。

3. 单击**关闭**以关闭向导。 如果您执行此操作之前备份过程完成后，向导将继续运行在后台。

初始备份完成后，**作业完成**状态出现在备份控制台。

![红外线 （ir) 完成](./media/backup-configure-vault/ircomplete.png)

## <a name="questions"></a>问题？
如果您有问题，或[向我们发送反馈](http://aka.ms/azurebackup_feedback)您想要查看包含，任何功能。

## <a name="next-steps"></a>下一步行动
有关备份 Vm 或其他工作负载的其他信息，请参阅︰

- 现在，您已备份文件和文件夹，您可以[管理您的存储库和服务器](backup-azure-manage-windows-server.md)。
- 如果您需要还原的备份，使用[将文件还原到 Windows 机器](backup-azure-restore-windows-server.md)到这篇文章。
