<properties
    pageTitle="介绍存储 |Microsoft Azure"
    description="Azure 存储，微软在线数据存储在云中的概述。 了解如何在您的应用程序中使用最佳的可用云存储解决方案。"
    services="storage"
    documentationCenter=""
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="10/25/2016"
    ms.author="tamram"/>

# <a name="introduction-to-microsoft-azure-storage"></a>介绍 Microsoft Azure 存储

## <a name="overview"></a>概述

Azure 存储是云存储解决方案的现代应用程序依赖于持续性、 可用性和可扩展性，以满足客户的需求。 通过阅读此文章、 开发人员、 IT 专业人员和业务决策制定者可以了解︰

- 什么是 Azure 存储，以及您可以如何充分利用它在移动您云、 服务器和桌面应用程序
- 使用 Azure 存储服务可以存储哪些类型的数据︰ 斑点 （对象） 的数据，NoSQL 表数据，队列消息和文件共享。
- 如何管理访问在 Azure 存储数据
- 如何使您的 Azure 存储数据持久通过冗余和复制
- 请转到下一步建立首个 Azure 存储应用程序的位置

若要快速启动和运行使用 Azure 存储，请参阅[开始使用 Azure 存储中五分钟](storage-getting-started-guide.md)。

有关工具、 库和其他资源的使用 Azure 存储的详细信息，请参阅[下一步行动](#next-steps)。

## <a name="what-is-azure-storage"></a>什么是 Azure 存储？

云计算的数据 — 这正是为什么 Microsoft 开发 Azure 存储需要可扩展、 耐用，且高度可用的存储应用程序启用新方案。 使开发人员能够构建大型应用程序来支持新的方案，除了 Azure 存储的 Azure 的虚拟机，其稳定性进一步证明还提供了存储基础。

使您可以存储和进程数以百计的数万亿字节的数据，以支持大数据方案和所需的科学、 财务分析、 媒体应用程序，是大规模扩展，azure 存储。 或者，您可以存储少量小型企业网站所需的数据。 无论您的需求范围，您支付您存储的数据。 目前，azure 存储存储唯一的客户对象是数万上万，平均处理每秒的请求数以百万计。

Azure 存储是弹性的的因此可以设计应用程序的大型全球受众，并随着这些应用程序所需的存储的数据量，并对它的请求的数量方面。 您支付仅为您的使用，并仅在您使用它时。

Azure 存储使用的自动分区系统进行自动负载平衡基于通信数据。 这意味着，应用程序增长的需求，为 Azure 存储自动分配适当的资源来满足它们。

是否运行在云中，在桌面上、 内部部署服务器，或移动，则可以从任何地方在世界上，从任何类型的应用程序中，访问 azure 存储或 tablet 设备。 您可以在移动方案中应用程序存储设备上数据的子集，将它与一套完整的数据存储在云同步使用 Azure 存储。

Azure 存储支持客户端方便开发使用不同系列的操作系统 （包括 Windows 和 Linux） 和不同的编程语言 （包括.NET、 Java、 Node.js，Python、 拼音、 PHP 和 c + + 和移动的编程语言）。 Azure 存储还公开了通过简单的 REST Api，可供任何客户端能够发送和接收数据通过 HTTP/HTTPS 的数据资源。

Azure 高级存储提供了高性能、 低延迟磁盘 I/O 密集型工作负载运行 Azure 虚拟机上的支持。 使用 Azure 高级存储时，可以附加到虚拟机的多个持久性数据磁盘并配置以满足您的性能要求。 每个数据磁盘最大 I/O 性能在 Azure 存储中最优的 SSD 盘由支持。 请参阅[高级存储︰ Azure 虚拟机工作负载的高性能存储](storage-premium-storage.md)了解详情。

## <a name="introducing-the-azure-storage-services"></a>引入了 Azure 存储服务

Azure 存储提供了以下四种服务︰ Blob 存储、 表存储、 队列存储和文件存储。

- Blob 存储存储非结构化的对象的数据。 Blob 可以是任何类型的文本或二进制数据，例如文档、 媒体文件或应用程序安装程序。 Blob 存储也称为对象存储区。
- 表存储存储结构化的数据集。 表存储是 NoSQL key 属性数据存储，从而允许进行快速开发，并对大量数据的快速访问。
- 队列的存储提供了可靠的消息传送工作流处理和云服务的组件之间进行通信。
- 文件存储提供了使用标准的 SMB 协议的遗留应用程序的共享的存储。 Azure 的虚拟机和云服务可以跨应用程序组件通过装载的共享，共享文件数据和内部部署的应用程序可以访问文件服务 REST API，通过共享中的文件数据。

Azure 存储帐户是 Azure 存储中的服务使您可以访问的安全帐户。 您的存储帐户为您的存储资源提供唯一的命名空间。 下图显示了存储帐户在 Azure 存储资源之间的关系︰

![Azure 存储资源](./media/storage-introduction/storage-concepts.png)

[AZURE.INCLUDE [storage-account-types-include](../../includes/storage-account-types-include.md)]

[AZURE.INCLUDE [storage-versions-include](../../includes/storage-versions-include.md)]

## <a name="blob-storage"></a>Blob 存储

对于用户大量的非结构化的对象的数据存储在云中，Blob 存储提供了经济高效且可扩展的解决方案。 Blob 存储可用于存储内容，如︰

- 文档
- 例如，照片、 视频、 音乐和博客的社会数据
- 备份的文件、 计算机、 数据库和设备
- 图像和文本的 web 应用程序
- 云应用程序的配置数据
- 大数据，例如，日志和其他大型的数据集

每个 blob 被组织到一个容器中。 容器还提供一种将安全策略分配到组的对象。 存储帐户可以包含任意数量的容器，容器可以包含任意数量的 blob，存储帐户 500 TB 的容量限制。  

Blob 存储提供三种类型的 blob、 阻止 blob，追加 blob 和页面 blob （磁盘）。

- 块 blob 适合流式传输和存储云对象进行了优化，是一个不错的选择，用于存储文档、 媒体文件、 备份等。
- 追加 blob 相似块 blob，但经过优化的追加操作。 只能通过给结尾添加新块，追加 blob 可以进行更新。 追加 blob 的方案，例如日志记录，需要新的数据只能写入 blob 的末尾的一个不错的选择。
- 适合页面 blob 表示 IaaS 磁盘并支持随机写入，并且可能达 1 TB 的大小。 Azure 虚拟机网络附加磁盘是作为页面 blob 存储 VHD 的 IaaS。

对于超大型数据集网络约束，进行上载或下载数据 Blob 存储到路上不符合实际情况，可以提供给 Microsoft，以便导入或导出数据，直接从数据中心的一个硬驱。 请参阅[使用导入/导出 Microsoft Azure 服务传输到 Blob 存储的数据](storage-import-export-service.md)。

## <a name="table-storage"></a>表存储

现代应用程序通常具有更大的可扩展性和灵活性比前代产品所需的软件要求数据存储区。 表存储提供了高可用性、 可扩展的存储，以使您的应用程序可以自动扩展，以满足用户的需求。 表存储是微软 NoSQL 项/属性存储 — schemaless 的设计，使其不同于传统的关系数据库。 使用 schemaless 数据存储区中，很容易为您的应用程序发展变化的需要调整您的数据。 表存储是易于使用，，因此开发人员可以快速地创建应用程序。 对数据的访问是快速且经济高效地用于所有类型的应用程序。  表存储通常是成本比传统 SQL 的类似数据量显著降低。

表存储为键属性存储区中，意味着每个表中的值存储与一个类型化的属性名。 属性名称可用于筛选和指定选择条件。 属性和属性值的集合构成一个实体。 由于表存储 schemaless，在同一个表中的两个实体可以包含不同的属性集合，这些属性可以为不同类型。

表存储可用于存储灵活的数据集，如用户数据的 web 应用程序、 通讯簿、 设备信息和任何其他类型的元数据，您的服务需要。  可以将任意数量的实体存储在表中，并存储帐户可能包含任意数量的表的存储帐户的容量限制。

如 Blob 和队列，开发人员可以管理和访问简化高级查询功能和实现 JSON 和 AtomPub (基于 XML) 表存储使用标准的其余部分协议，但是表存储还支持 OData 协议的一个子集的格式。

对于今天的基于 Internet 的应用程序，如表存储 NoSQL 数据库提供的受欢迎的替代传统的关系数据库。

## <a name="queue-storage"></a>队列存储

在设计时用于缩放的应用程序，应用程序组件通常分隔，以便他们可以独立地扩展。 无论他们在云中，在桌面上，在内部服务器上，或移动设备上运行，队列存储为应用程序组件之间的异步通信提供可靠的消息传送解决方案。 队列存储还支持异步任务管理和构建流程的工作流。

存储帐户可以包含任意数量的队列。 队列可以包含任意数量的消息的存储帐户的容量限制。 单个邮件可能达 64 KB 的大小。

## <a name="file-storage"></a>文件存储

Azure 的文件存储提供基于云的 SMB 文件共享，以便您可以将快速且成本高昂的重新编写无依赖文件共享到 Azure 的旧式应用程序迁移。 使用 Azure 文件存储，云服务 Azure 的虚拟机中运行的应用程序可以装载在云中，文件共享就像桌面应用程序装载一个典型的 SMB 共享。 任何数量的应用程序组件可以然后装载并同时访问文件存储共享。

由于文件存储共享标准的 SMB 文件共享，在 Azure 上运行的应用程序可以访问文件系统 I/O Api 通过共享中的数据。 因此，开发人员可以利用他们现有的代码和将现有应用程序迁移的技能。 IT 专业人员可以使用 PowerShell cmdlet 创建、 装载和 Azure 应用程序的管理的一部分管理文件存储共享。

如其他 Azure 存储服务，文件存储公开 REST API 来访问共享中的数据。 后端应用程序可以调用文件存储 REST API 来访问的文件共享中的数据。 这种方式，企业可以选择迁移到 Azure 一些旧版应用程序，然后继续运行其他人在自己组织内。 请注意，安装文件共享，才可以在 Azure; 中运行的应用程序内部应用程序只能访问文件共享通过 REST API。

分布式应用程序也可以使用文件存储来存储和共享有用的应用程序数据，并开发和测试工具。 例如，应用程序可能会将配置文件存储，如日志、 度量标准和故障诊断数据转储的文件存储共享，以便它们可提供给多个虚拟机或角色中。 开发人员和管理员可以存储实用程序，它们需要生成或管理应用程序中的文件存储共享适用于所有组件，而不是在每个虚拟机或角色实例上安装它们。

## <a name="access-to-blob-table-queue-and-file-resources"></a>对 Blob、 表、 队列和文件资源的访问

默认情况下，只有存储帐户所有者可以访问存储帐户中的资源。 您的数据的安全性，针对您的帐户中的资源的所有请求必须进行身份都验证。 身份验证依赖于共享密钥模型。 Blob 也可配置为支持匿名身份验证。

在创建用于身份验证的两个私有访问键分配您的存储帐户。 有两个密钥可确保您的应用程序保持可用，当您定期重新生成密钥作为公共密钥管理的安全性。

如果您确实需要允许用户控制的访问您的存储资源，您可以创建一个共享的访问签名。 共享的访问签名 (SA) 是可以附加到一个 URL，使委派的访问存储资源的标记。 拥有该标记的任何人都可以访问该资源指向具有权限指定了它，它是有效的时间段。 从版本 2015年-04-05 开始，Azure 存储支持两种类型的共享的访问签名︰ 服务 SAS 和 SA 帐户。

SAS 服务委托中只有一种存储服务的资源的访问︰ 斑点、 队列、 表格或文件服务。

SA 帐户委派对一个或多个存储服务中的资源的访问。 您可以委派访问不可用 SAS 的服务的服务级操作。 您也可以委派访问权限以读取、 写入和删除操作 blob 容器、 表、 队列和不允许使用 SAS 服务的文件共享。

最后，您可以指定一个容器和其 blob 或特定的斑点，是可以公开访问。 在您指明的容器或 blob 是公共的任何人都可以阅读它以匿名方式;不进行身份验证是必需的。  公共容器和 blob 是用于公开等媒体和网站承载的文档资源。  为全球客户减少网络延迟，可以缓存所使用的网站与 Azure CDN 的 blob 数据。

共享的访问签名的详细信息，请参阅[使用共享访问签名 (SAS)](storage-dotnet-shared-access-signature-part-1.md) 。 安全地访问您的存储帐户的详细信息，请参阅[管理匿名的读访问，而对容器和 blob](storage-manage-access-to-resources.md)和[Azure 存储服务的身份验证](https://msdn.microsoft.com/library/azure/dd179428.aspx)。

## <a name="replication-for-durability-and-high-availability"></a>复制的持续性和高可用性

始终复制您的 Microsoft Azure 存储帐户中的数据以确保持续性和高可用性。 您在同一个数据中心，或到第二个数据中心中，哪一个复制选项根据您选择的数据复制。 复制保护您的数据，并保留您应用程序正常运行时间出现了暂时性的硬件故障。 如果您的数据复制到另一个数据中心，，还可以保护数据，防范发生灾难性故障的主要地点。

复制可确保您的存储帐户满足[存储服务级别协议 (SLA)](https://azure.microsoft.com/support/legal/sla/storage/)即使面对失败。 请参阅有关 Azure 存储 SLA 保证耐用性和可用性。 

当创建存储帐户时，您可以选择下面的复制选项之一︰  

- **本地冗余存储 (LRS)。** 本地冗余存储维护三个副本，您的数据。 LRS 中单个区域中的单个数据中心复制三次。 LRS 保护您的数据，从正常的硬件故障，而不是从一个数据中心故障。  

    LRS 享受价格折扣。 最大的耐用性，建议使用地理冗余存储，如下所述。


- **区域冗余存储 (ZRS)。** 区域冗余存储维护三个副本，您的数据。 ZRS 复制三次跨两到三个设施，在一个区域内或跨两个地区，提供较高的耐久性比 LRS。 ZRS 可确保您的数据在单个区域内持久。  

    ZRS 提供耐用性的级别高于 LRS;但是，对于最大的耐用性，我们建议使用地理冗余存储，如下所述。  

    > [AZURE.NOTE] ZRS 目前仅适用于块 blob 且仅支持版本 2014年-02-14 及更高版本。
    >
    > 创建您的存储帐户并选择 ZRS 后，您不能将它用于任何其他类型的复制，反之亦然。

- **Geo 冗余存储 (GRS)**。 GRS 维护数据的六份。 GRS，与您的数据的主区域内复制三次，还可在辅助区域数百英里为主的地区，提供耐用性的最高级别复制三次。 在发生故障时的主要地区，Azure 存储将故障切换到辅助区域。 GRS 可确保您的数据在两个单独的区域是耐用。

    有关地区的主要和次要配对信息，请参见[Azure 区域](https://azure.microsoft.com/regions/)。

- **读取访问地理冗余存储 (RA GRS)**。 读取访问地理冗余存储将数据复制到一个辅助的地理位置，并且还提供对在辅助位置有数据的读访问。 读取访问地理冗余存储允许您从主或辅助位置，访问您的数据的一个位置不可用。 读取访问地理冗余存储时创建的默认选项为您的存储帐户默认情况。 

    > [AZURE.IMPORTANT] 您可以更改您的数据的复制方式创建存储帐户后, 除非在创建帐户时指定 ZRS。 但是，请注意您可能遭受其他零星数据传输费用如果从 LRS 切换到 GRS 或 RA GRS。

有关存储复制选项的其他详细信息，请参阅[Azure 存储复制](storage-redundancy.md)。

定价为存储帐户复制的信息，请参阅[Azure 存储定价](https://azure.microsoft.com/pricing/details/storage/)。 有关每个区域中哪些服务是可用的详细信息，请参阅[Azure 区域](https://azure.microsoft.com/regions/#services)。

有关使用 Azure 存储持久性的体系结构的详细信息，请参阅[SOSP 纸张-Azure 存储︰ 高度可用云存储服务强一致性与](http://blogs.msdn.com/b/windowsazurestorage/archive/2011/11/20/windows-azure-storage-a-highly-available-cloud-storage-service-with-strong-consistency.aspx)。


## <a name="transferring-data-to-and-from-azure-storage"></a>Azure 存储与传输数据

可以使用 AzCopy 命令行实用程序将在您的存储帐户或跨存储帐户复制 blob、 文件和表数据。 有关详细信息，请参阅[传输数据，使用 AzCopy 命令行实用程序](storage-use-azcopy.md)。

AzCopy 是基于[Azure 数据移动库](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/)中，这是目前在预览。

Azure 导入/导出服务使您能够 blob 数据导入或导出 blob 数据从您的存储帐户通过邮寄到 Azure 数据中心的硬盘磁盘。 有关导入/导出服务的详细信息，请参阅[使用 Microsoft Azure 导入/导出服务将数据传输到 Blob 存储](storage-import-export-service.md)。

## <a name="pricing"></a>定价

[AZURE.INCLUDE [storage-account-billing-include](../../includes/storage-account-billing-include.md)]

## <a name="storage-apis-libraries-and-tools"></a>存储 Api、 库和工具

Azure 存储资源可以通过任何可以发出 HTTP/HTTPS 请求的语言。 此外，Azure 存储提供了几种常用语言的编程库。 这些库简化了使用 Azure 存储处理的详细信息，如同步和异步调用、 批处理操作、 异常管理、 自动重试、 操作行为等诸多方面。 库是当前可用于以下语言和平台，与其他人在管道中︰

### <a name="azure-storage-data-services"></a>Azure 存储数据服务

- [存储服务 REST API，](http://msdn.microsoft.com/library/azure/dd179355.aspx)
- [存储.NET、 Windows Phone 和 Windows 运行时客户端库](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [存储器 c + + 客户端库](https://github.com/Azure/azure-storage-cpp)
- [对于 Java/Android 存储客户端库](/develop/java/)
- [存储客户端 Node.js 库](http://dl.windowsazure.com/nodestoragedocs/index.html)
- [对于 PHP 存储客户端库](/develop/php/)
- [对于 Ruby 存储客户端库](/develop/ruby/)
- [Python 的存储客户端库](/develop/python/)
- [存储 Cmdlet 的 PowerShell 1.0](https://msdn.microsoft.com/library/azure/mt269418.aspx)

### <a name="azure-storage-management-services"></a>Azure 存储管理服务

- [存储资源提供商 REST API 参考](https://msdn.microsoft.com/library/azure/mt163683.aspx)
- [存储资源提供程序.NET 客户端库](https://msdn.microsoft.com/library/azure/mt131037.aspx)
- [存储资源提供 Cmdlet 的 PowerShell 1.0](https://msdn.microsoft.com/library/azure/mt607151.aspx)
- [存储服务管理 REST API （传统）](https://msdn.microsoft.com/library/azure/ee460790.aspx)

### <a name="azure-storage-data-movement-services"></a>Azure 存储数据移动服务

- [导入/导出服务 REST API 存储](https://msdn.microsoft.com/library/azure/dn529096.aspx)
- [使.NET 的存储数据移动客户端库](https://www.nuget.org/packages/Microsoft.Azure.Storage.DataMovement/)

### <a name="tools-and-utilities"></a>工具和实用程序

- [Azure 存储资源管理器](http://go.microsoft.com/fwlink/?LinkID=822673&clcid=0x409)
- [Azure 存储客户端工具](storage-explorers.md)
- [Azure 的 Sdk 和工具](https://azure.microsoft.com/tools/)
- [Azure 存储仿真程序](http://www.microsoft.com/download/details.aspx?id=43709)
- [Azure PowerShell](../powershell-install-configure.md)
- [AzCopy 命令行实用程序](http://aka.ms/downloadazcopy)

## <a name="next-steps"></a>下一步行动

若要了解有关 Azure 存储的详细信息，请浏览这些资源︰

### <a name="documentation"></a>文档

- [Azure 存储文档](https://azure.microsoft.com/documentation/services/storage/)

### <a name="for-administrators"></a>为管理员

- [Azure PowerShell 使用 Azure 存储](storage-powershell-guide-full.md)
- [Azure CLI 使用 Azure 存储](storage-azure-cli.md)

### <a name="for-net-developers"></a>为.NET 开发人员

- [有关使用.NET 的 Azure Blob 存储入门](storage-dotnet-how-to-use-blobs.md)
- [有关使用.NET 的 Azure 表存储入门](storage-dotnet-how-to-use-tables.md)
- [有关使用.NET 的 Azure 队列存储入门](storage-dotnet-how-to-use-queues.md)
- [开始使用 Windows Azure 文件存储](storage-dotnet-how-to-use-files.md)

### <a name="for-javaandroid-developers"></a>对于 Java/Android 开发人员

- [如何使用 Blob 存储从 Java](storage-java-how-to-use-blob-storage.md)
- [如何使用 Java 表存储](storage-java-how-to-use-table-storage.md)
- [如何使用队列存储从 Java](storage-java-how-to-use-queue-storage.md)
- [如何使用从 Java 文件存储](storage-java-how-to-use-file-storage.md)

### <a name="for-nodejs-developers"></a>对于 Node.js 开发人员

- [如何使用 Blob 存储从 Node.js](storage-nodejs-how-to-use-blob-storage.md)
- [如何使用表格存储从 Node.js](storage-nodejs-how-to-use-table-storage.md)
- [如何使用队列存储从 Node.js](storage-nodejs-how-to-use-queues.md)

### <a name="for-php-developers"></a>为 PHP 开发人员

- [如何使用 PHP 中的 Blob 存储](storage-php-how-to-use-blobs.md)
- [如何使用 PHP 从表存储](storage-php-how-to-use-table-storage.md)
- [如何使用 PHP 从队列存储](storage-php-how-to-use-queues.md)

### <a name="for-ruby-developers"></a>Ruby 开发人员

- [如何使用 Blob 存储从拼音](storage-ruby-how-to-use-blob-storage.md)
- [如何使用表格存储从拼音](storage-ruby-how-to-use-table-storage.md)
- [如何使用队列存储从拼音](storage-ruby-how-to-use-queue-storage.md)

### <a name="for-python-developers"></a>Python 开发

- [如何使用 Python 中的 Blob 存储](storage-python-how-to-use-blob-storage.md)
- [如何使用 Python 中的表存储](storage-python-how-to-use-table-storage.md)
- [如何使用 Python 从队列存储](storage-python-how-to-use-queue-storage.md)
- [如何使用 Python 中的文件存储](storage-python-how-to-use-file-storage.md)
