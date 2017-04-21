<properties
    pageTitle="管理匿名读取访问权限的容器和 blob |Microsoft Azure"
    description="了解如何使容器和 blob 用于匿名访问，以及如何以编程方式访问它们。"
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
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="tamram"/>

# <a name="manage-anonymous-read-access-to-containers-and-blobs"></a>管理匿名读取访问权限的容器和 blob

## <a name="overview"></a>概述

默认情况下，只有存储帐户的所有者可以访问该帐户内的存储资源。 对于 Blob 存储，可以设置容器的权限，以允许匿名读取访问容器和其 blob，以便不共享帐户密钥可以授予对这些资源的访问。

匿名访问是最适合于希望某些 blob 以始终可供匿名的读取访问权限的方案。 较细粒度的控制，您可以创建一个共享的访问签名，可以使用不同的权限受到限制的代理访问，并在指定的时间间隔内。 有关创建共享的访问签名的详细信息，请参见[使用共享访问签名 (SAS)](storage-dotnet-shared-access-signature-part-1.md)。

## <a name="grant-anonymous-users-permissions-to-containers-and-blobs"></a>授予对容器和 blob 的匿名用户权限

默认情况下，可能只能通过存储帐户的所有者访问容器，其内部任何 blob。 若要授予匿名用户对容器和其 blob 的读取权限，可以设置容器权限，以允许公共访问权限。 匿名用户可以读取 blob 可公开访问的容器中而对请求进行身份验证。

容器提供了用于管理容器访问以下选项︰

- **完全公共的读访问权限︰**可以通过匿名请求读取容器和 blob 数据。 客户端可以枚举通过匿名请求时，容器中的 blob，但无法枚举内存储帐户的容器。

- **公共读取 blob 仅访问︰**这个容器中的 blob 数据可以读取通过匿名请求，但容器数据不可用。 客户端无法枚举通过匿名请求容器中的 blob。

- **没有公共的读访问权限︰**可以按帐户所有者只读取容器和 blob 数据。

可以按以下方式设置容器权限︰

- 从[Azure 的门户](https://portal.azure.com)。
- 以编程方式，通过使用存储客户端库或 REST API。
- 通过使用 PowerShell。 若要了解有关从 Azure PowerShell 设置容器的权限，请参阅[使用 Azure 存储使用 Azure PowerShell](storage-powershell-guide-full.md#how-to-manage-azure-blobs)。

### <a name="setting-container-permissions-from-the-azure-portal"></a>从 Azure 门户设置容器权限

若要从[Azure 门户](https://portal.azure.com)设置容器的权限，请执行以下步骤︰

1. 导航到您的存储帐户的仪表板。
2. 从列表中选择的容器名称。 单击名称公开选定容器中的 blob
3. 从工具栏中选择**访问策略**。
4. 在**访问类型**字段中，选择您所需的权限级别，如下面的屏幕快照中所示。

    ![编辑容器元数据对话框](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

### <a name="setting-container-permissions-programmatically-using-net"></a>以编程方式使用.NET 设置容器权限

若要设置权限的容器使用.NET 客户端库，首先通过调用**GetPermissions**方法来检索容器的现有权限。 然后将**GetPermissions**方法所返回的**BlobContainerPermissions**对象的**PublicAccess**属性设置。 最后，调用**SetPermissions**方法具有更新权限。

下面的示例将容器的权限设置为完全公共的读访问权限。 将权限设置为 blob 的公共读访问权限，将**PublicAccess**属性设置为**BlobContainerPublicAccessType.Blob**。 若要删除匿名用户的所有权限，请将属性设置为**BlobContainerPublicAccessType.Off**。

    public static void SetPublicContainerPermissions(CloudBlobContainer container)
    {
        BlobContainerPermissions permissions = container.GetPermissions();
        permissions.PublicAccess = BlobContainerPublicAccessType.Container;
        container.SetPermissions(permissions);
    }

## <a name="access-containers-and-blobs-anonymously"></a>匿名访问容器和 blob

匿名访问容器和 blob 的客户机可以使用不需要凭据的构造函数。 下面的示例显示几个不同的方式来引用匿名的 Blob 服务资源。

### <a name="create-an-anonymous-client-object"></a>创建匿名客户端对象

可以通过为该帐户提供的 Blob 服务终结点来创建匿名访问新的服务客户端对象。 但是，您还必须知道容器中可用于匿名访问的帐户的名称。

    public static void CreateAnonymousBlobClient()
    {
        // Create the client object using the Blob service endpoint.
        CloudBlobClient blobClient = new CloudBlobClient(new Uri(@"https://storagesample.blob.core.windows.net"));

        // Get a reference to a container that's available for anonymous access.
        CloudBlobContainer container = blobClient.GetContainerReference("sample-container");

        // Read the container's properties. Note this is only possible when the container supports full public read access.
        container.FetchAttributes();
        Console.WriteLine(container.Properties.LastModified);
        Console.WriteLine(container.Properties.ETag);
    }

### <a name="reference-a-container-anonymously"></a>以匿名方式引用容器

如果您的 URL，以匿名方式可用的容器，可用于直接引用容器。

    public static void ListBlobsAnonymously()
    {
        // Get a reference to a container that's available for anonymous access.
        CloudBlobContainer container = new CloudBlobContainer(new Uri(@"https://storagesample.blob.core.windows.net/sample-container"));

        // List blobs in the container.
        foreach (IListBlobItem blobItem in container.ListBlobs())
        {
            Console.WriteLine(blobItem.Uri);
        }
    }


### <a name="reference-a-blob-anonymously"></a>以匿名方式引用一个 blob

如果您有可用于匿名访问的 blob 的 URL，您可以引用 blob 直接使用该 URL:

    public static void DownloadBlobAnonymously()
    {
        CloudBlockBlob blob = new CloudBlockBlob(new Uri(@"https://storagesample.blob.core.windows.net/sample-container/logfile.txt"));
        blob.DownloadToFile(@"C:\Temp\logfile.txt", System.IO.FileMode.Create);
    }

## <a name="features-available-to-anonymous-users"></a>匿名用户可访问的功能

下表显示了哪些操作可能由匿名用户，当容器的 ACL 设置为允许公共访问权限。

| 其他操作                                         | 具有完整公钥的读取访问权限 | 与公钥 blob 仅读访问权限的权限 |
|--------------------------------------------------------|-----------------------------------------|---------------------------------------------------|
| 列表的容器                                        | 只有所有者                              | 只有所有者                                        |
| 创建容器                                       | 只有所有者                              | 只有所有者                                        |
| 获取容器属性                               | 所有                                     | 只有所有者                                        |
| 获取容器的元数据                                 | 所有                                     | 只有所有者                                        |
| 设置容器元数据                                 | 只有所有者                              | 只有所有者                                        |
| 获取容器 ACL                                      | 只有所有者                              | 只有所有者                                        |
| 设置容器 ACL                                      | 只有所有者                              | 只有所有者                                        |
| 删除容器                                       | 只有所有者                              | 只有所有者                                        |
| 列表中的 Blob                                             | 所有                                     | 只有所有者                                        |
| 将 Blob                                               | 只有所有者                              | 只有所有者                                        |
| 获取 Blob                                               | 所有                                     | 所有                                               |
| 获取 Blob 属性                                    | 所有                                     | 所有                                               |
| Blob 设置属性                                    | 只有所有者                              | 只有所有者                                        |
| 获取元数据 Blob                                      | 所有                                     | 所有                                               |
| 设置元数据 Blob                                      | 只有所有者                              | 只有所有者                                        |
| 放块                                              | 只有所有者                              | 只有所有者                                        |
| 获取块列表 （仅提交块）                 | 所有                                     | 所有                                               |
| 获取块列表 （仅在未提交的块或所有块） | 只有所有者                              | 只有所有者                                        |
| 放块列表                                         | 只有所有者                              | 只有所有者                                        |
| 删除 Blob                                            | 只有所有者                              | 只有所有者                                        |
| 复制 Blob                                              | 只有所有者                              | 只有所有者                                        |
| 快照的斑点                                          | 只有所有者                              | 只有所有者                                        |
| 租约 Blob                                             | 只有所有者                              | 只有所有者                                        |
| 放入页面                                               | 只有所有者                              | 只有所有者                                        |
| 获取页面范围                                        | 所有                                     | 所有                                                  |
| 追加 Blob                                            | 只有所有者                              | 只有所有者                                                  |


## <a name="see-also"></a>请参见

- [Azure 存储服务的身份验证](https://msdn.microsoft.com/library/azure/dd179428.aspx)
- [使用共享的访问签名 (SAS)](storage-dotnet-shared-access-signature-part-1.md)
- [委派访问权限共享的访问签名](https://msdn.microsoft.com/library/azure/ee395415.aspx)
