<properties
    pageTitle="如何创建、 管理或删除存储帐户在 Azure 门户 |Microsoft Azure"
    description="创建新的存储帐户、 管理帐户访问密钥，或删除存储帐户在 Azure 门户。 了解标准和最优存储帐户。"
    services="storage"
    documentationCenter=""
    authors="robinsh"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="07/26/2016"
    ms.author="robinsh"/>


# <a name="about-azure-storage-accounts"></a>关于 Azure 存储帐户

[AZURE.INCLUDE [storage-selector-portal-create-storage-account](../../includes/storage-selector-portal-create-storage-account.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools](../../includes/storage-try-azure-tools.md)]

## <a name="overview"></a>概述

Azure 存储帐户提供了唯一的命名空间来存储和访问 Azure 存储数据对象。 存储帐户中的所有对象作为一个组协同都计费。 默认情况下，您的帐户中的数据是仅供您的帐户所有者。

[AZURE.INCLUDE [storage-account-types-include](../../includes/storage-account-types-include.md)]

## <a name="storage-account-billing"></a>存储客户帐单

[AZURE.INCLUDE [storage-account-billing-include](../../includes/storage-account-billing-include.md)]

> [AZURE.NOTE] Azure 的虚拟机创建时，存储帐户为您自动创建在部署位置如果您没有存储帐户在该位置。 因此不需要按照下面的步骤来创建虚拟机磁盘的存储帐户。 存储的帐户名称将基于虚拟机的名称。 请参阅[Azure 虚拟机文档](https://azure.microsoft.com/documentation/services/virtual-machines/)了解详情。

## <a name="storage-account-endpoints"></a>存储帐户终结点

在 Azure 存储中存储的每个对象都有一个唯一的 URL 地址。 存储帐户名称构成该地址的子域。 子域和域的名称，它是特定于每个服务的组合形成一个*终结点*为您的存储帐户。

例如，如果您的存储帐户叫做*mystorageaccount*，然后为您的存储帐户的默认终结点是︰

- Blob 服务︰ http://*mystorageaccount*。 blob.core.windows.net

- 表服务︰ http://*mystorageaccount*。 table.core.windows.net

- 排队服务︰ http://*mystorageaccount*。 queue.core.windows.net

- 文件服务︰ http://*mystorageaccount*。 file.core.windows.net

> [AZURE.NOTE] Blob 存储帐户仅公开 Blob 服务终结点。

通过将对象的位置中存储帐户附加到该终结点生成访问存储帐户中的对象的 URL。 例如，一个 blob 的地址可能有这种格式︰ http://*mystorageaccount*.blob.core.windows.net/*mycontainer*/*myblob*。

您还可以配置自定义的域名与您的存储帐户使用。 传统的存储帐户，请参阅[配置自定义的域为 Blob 存储终结点的名称](storage-custom-domain-name.md)详细信息。 对于资源管理器存储帐户，此功能尚未添加到[Azure 的门户](https://portal.azure.com)，但您可以使用 PowerShell 配置它。 有关详细信息，请参阅[设置 AzureRmStorageAccount](https://msdn.microsoft.com/library/mt607146.aspx) cmdlet。  

## <a name="create-a-storage-account"></a>创建存储帐户。

1. 登录到[Azure 的门户](https://portal.azure.com)。

2. 在集线器菜单中，选择**新建** -> **数据 + 存储** -> **存储帐户**。

3. 输入您的存储帐户的名称。 有关如何存储帐户名称将用于解决您在 Azure 存储中的对象的详细信息，请参阅[存储帐户的终结点](#storage-account-endpoints)。

    > [AZURE.NOTE] 存储帐户名称必须介于 3 和 24 个字符长之间，可以包含数字和仅为小写字母。
    >  
    > 您的存储帐户名称必须是唯一在 Azure。 Azure 门户将显示您选择的存储帐户名称已在使用。

4. 指定要使用的部署模型︰**资源管理器**或**经典**。 **资源管理**器的建议的部署模型。 有关详细信息，请参阅[了解资源管理器部署和经典的部署](../resource-manager-deployment-model.md)。

    > [AZURE.NOTE] 仅可以使用资源管理器部署模型创建 blob 存储帐户。

5. 选择的存储帐户的类型︰**一般用途**或**Blob 存储**。 **一般用途**是默认值。

    如果选择了**一般用途**，然后指定性能层︰**标准**或**特优**。 默认值为**标准**。 标准和最优存储帐户的详细信息，请参阅[Microsoft Azure 存储简介](storage-introduction.md)和[高级存储︰ Azure 虚拟机工作负载的高性能存储](storage-premium-storage.md)。

    如果选择了**Blob 存储**，然后指定访问层︰**热**或**冷**。 默认情况下**热**。 请参阅[Azure Blob 存储︰ 冷却和热层](storage-blob-storage-tiers.md)了解详情。

6. 选择复制选项的存储帐户︰ **LRS**、 **GRS**、 **RA GRS**或**ZRS**。 默认值为**RA GRS**。 Azure 存储复制选项的详细信息，请参阅[Azure 存储复制](storage-redundancy.md)。

7. 选择要在其中创建新的存储帐户的订购。

8. 指定新的资源组，或选择一个现有的资源组。 有关资源组的详细信息，请参阅[Azure 资源管理器概述](../azure-resource-manager/resource-group-overview.md)。

9. 选择您的存储帐户的地理位置。 有关哪些服务在哪些区域中是可用的详细信息，请参阅[Azure 区域](https://azure.microsoft.com/regions/#services)。

10. 单击**创建**来创建存储帐户。

## <a name="manage-your-storage-account"></a>管理您的存储帐户

### <a name="change-your-account-configuration"></a>更改您的帐户配置

您在创建存储帐户后，您可以修改其配置，例如，更改的帐户使用的复制选项或更改的访问层的 Blob 存储帐户。 在[Azure 的门户网站](https://portal.azure.com)中，导航到您的存储帐户，单击**所有设置**，然后都单击**配置**查看和/或更改帐户配置。

> [AZURE.NOTE] 根据创建存储帐户时选择的性能层，某些复制选项可能不可用。

更改复制选项将更改您的定价。 有关详细信息，请参阅[Azure 存储定价](https://azure.microsoft.com/pricing/details/storage/)页。

对于 Blob 存储帐户，更改访问层可能招的更改以及更改您的定价收费。 请[Blob 存储帐户的定价和计费](storage-blob-storage-tiers.md#pricing-and-billing)的更多详细信息，参阅。

### <a name="manage-your-storage-access-keys"></a>管理您的存储访问键

当创建存储帐户时，Azure 将生成两个 512 位存储访问键，访问存储帐户时用于身份验证。 通过提供两个存储访问键，Azure 可以重新生成密钥存储服务不会中断或对该服务的访问。

> [AZURE.NOTE] 我们建议您避免与其他人共享存储的访问键。 若要允许访问存储资源不给出您的访问密钥的情况下，可以使用*共享的访问签名*。 共享的访问签名提供了访问您的帐户中的资源，为您定义的时间间隔和指定的权限。 有关更多信息，请参见[使用共享访问签名 (SAS)](storage-dotnet-shared-access-signature-part-1.md) 。

#### <a name="view-and-copy-storage-access-keys"></a>查看和复制存储访问键

在[Azure 的门户网站](https://portal.azure.com)中，导航到您的存储帐户，单击**所有设置**，然后都单击**访问键**来查看、 复制和重新生成您的帐户访问密钥。 **访问键**刀片还包括使用主要和辅助键，您可以复制您的应用程序中使用的预配置的连接字符串。

#### <a name="regenerate-storage-access-keys"></a>重新生成存储访问键

我们建议您将访问键更改为您的存储帐户定期来帮助保护您存储的连接的安全。 这样，您可以通过一个访问密钥重新生成另一个访问密钥维护存储帐户连接分配两个访问键。

> [AZURE.WARNING] 重新生成访问密钥可能会影响在 Azure，以及您自己的应用程序依赖于存储帐户的服务。 使用访问键访问存储帐户的所有客户端必须更新以使用新的密钥。

**媒体服务**-如果您有依赖于您的存储帐户的介质服务，您必须重新同步访问键与介质服务后重新生成密钥。

**应用程序**-如果您的 web 应用程序或使用存储帐户的云服务，您将丢失连接如果您重新生成密钥，除非您将您的密钥。

**存储资源管理器**-如果您使用[存储资源管理器应用程序](storage-explorers.md)的任何您可能需要更新这些应用程序使用的存储密钥。

以下是旋转您存储访问键的过程︰

1. 更新应用程序代码来引用存储帐户的辅助访问键中的连接字符串。

2. 重新生成您的存储帐户的主访问键。 **访问键**刀片上,**再生 Key1**中，请单击，然后单击**是**以确认您要生成一个新密钥。

3. 更新您的代码以引用新的主访问键中的连接字符串。

4. 以相同的方式重新生成辅助访问键。

## <a name="delete-a-storage-account"></a>删除存储帐户

要删除不再使用的存储帐户，请导航到在[Azure 的门户](https://portal.azure.com)中，存储帐户，单击**删除**。 删除存储帐户删除整个科目，包括该帐户中的所有数据。

> [AZURE.WARNING] 不能恢复已删除的存储帐户或检索任何在删除之前它所包含的内容。 请务必备份任何您想要保存您在删除帐户之前。 这也适用于任何资源帐户中 — — 一旦删除 blob、 表、 队列或文件，将其永久删除。

若要删除与 Azure 的虚拟机相关联的存储帐户，必须首先确保所有虚拟机磁盘已被都删除。 如果不先删除虚拟机磁盘，然后当您尝试删除您存储的帐户，您将看到类似的错误消息︰

    Failed to delete storage account <vm-storage-account-name>. Unable to delete storage account <vm-storage-account-name>: 'Storage account <vm-storage-account-name> has some active image(s) and/or disk(s). Ensure these image(s) and/or disk(s) are removed before deleting this storage account.'.

如果存储帐户使用传统部署模型，您可以通过[Azure 的门户网站](https://manage.windowsazure.com)中的下列步骤删除虚拟机磁盘︰

1. 导航到[经典 Azure 的门户](https://manage.windowsazure.com)。
2. 导航到虚拟机选项卡。
3. 单击磁盘选项卡。
4. 选择您的数据的磁盘，然后单击删除磁盘。
5. 若要删除磁盘映像，请导航到图像选项卡，删除存储在该帐户中的所有图像。

有关详细信息，请参阅[Azure 虚拟机文档](http://azure.microsoft.com/documentation/services/virtual-machines/)。

## <a name="next-steps"></a>下一步行动

- [Azure Blob 存储︰ 在安静和低温热层](storage-blob-storage-tiers.md)
- [Azure 存储复制](storage-redundancy.md)
- [配置 Azure 存储连接字符串](storage-configure-connection-string.md)
- [使用 AzCopy 命令行实用程序的数据传输](storage-use-azcopy.md)
- 访问[团队博客，Azure 存储](http://blogs.msdn.com/b/windowsazurestorage/)。
