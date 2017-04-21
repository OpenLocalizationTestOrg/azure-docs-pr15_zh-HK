<properties
    pageTitle="开始使用 Azure Blob 存储 （对象存储） 使用.NET |Microsoft Azure"
    description="将非结构化的数据存储在云中使用 Azure Blob 存储 （对象存储）。"
    services="storage"
    documentationCenter=".net"
    authors="tamram"
    manager="carmonm"
    editor="tysonn"/>

<tags
    ms.service="storage"
    ms.workload="storage"
    ms.tgt_pltfrm="na"
    ms.devlang="dotnet"
    ms.topic="hero-article"
    ms.date="10/18/2016"
    ms.author="tamram"/>


# <a name="get-started-with-azure-blob-storage-using-net"></a>有关使用.NET 的 Azure Blob 存储入门

[AZURE.INCLUDE [storage-selector-blob-include](../../includes/storage-selector-blob-include.md)]
<br/>
[AZURE.INCLUDE [storage-try-azure-tools-blobs](../../includes/storage-try-azure-tools-blobs.md)]

## <a name="overview"></a>概述

Azure Blob 存储是将非结构化的数据作为对象/blob 存储在云中的服务。 Blob 存储可以存储任何类型的文本或二进制数据，例如文档、 媒体文件或应用程序安装程序。 Blob 存储也称为对象存储区。

### <a name="about-this-tutorial"></a>关于本教程

本教程演示如何编写使用 Azure Blob 存储一些常见方案的.NET 代码。 方案涵盖包括上传、 列表、 下载和删除 blob。

**估计完成时间︰** 45 分钟

**Prerequisities:**

- [Microsoft Visual Studio](https://www.visualstudio.com/en-us/visual-studio-homepage-vs.aspx)
- [Azure 存储.NET 客户端库](https://www.nuget.org/packages/WindowsAzure.Storage/)
- [使.NET 的的 azure 配置管理器](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/)
- [Azure 存储帐户](storage-create-storage-account.md#create-a-storage-account)


[AZURE.INCLUDE [storage-dotnet-client-library-version-include](../../includes/storage-dotnet-client-library-version-include.md)]

### <a name="more-samples"></a>更多的示例

有关使用 Blob 存储的其他示例，请参阅[入门 Azure Blob 存储在.NET 中](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/)。 您可以下载示例应用程序并运行它，或浏览 GitHub 上的代码。


[AZURE.INCLUDE [storage-blob-concepts-include](../../includes/storage-blob-concepts-include.md)]

[AZURE.INCLUDE [storage-create-account-include](../../includes/storage-create-account-include.md)]

[AZURE.INCLUDE [storage-development-environment-include](../../includes/storage-development-environment-include.md)]

### <a name="add-namespace-declarations"></a>添加命名空间声明

添加以下`using`语句到顶部的`program.cs`文件︰

    using Microsoft.Azure; // Namespace for CloudConfigurationManager
    using Microsoft.WindowsAzure.Storage; // Namespace for CloudStorageAccount
    using Microsoft.WindowsAzure.Storage.Blob; // Namespace for Blob storage types

### <a name="parse-the-connection-string"></a>分析连接字符串

[AZURE.INCLUDE [storage-cloud-configuration-manager-include](../../includes/storage-cloud-configuration-manager-include.md)]

### <a name="create-the-blob-service-client"></a>创建 Blob 服务客户端

可以使用**CloudBlobClient**类检索容器和 blob 存储 Blob 存储在。 下面是创建服务客户端的一种方法︰

    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

现在，您就可以编写代码来读取数据和将数据写入 Blob 存储了。

## <a name="create-a-container"></a>创建容器

[AZURE.INCLUDE [storage-container-naming-rules-include](../../includes/storage-container-naming-rules-include.md)]

此示例演示如何创建一个容器，如果不存在︰

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Create the container if it doesn't already exist.
    container.CreateIfNotExists();

默认情况下，新的容器是私有的也就是说，您必须指定您的存储访问密钥下载此容器中的 blob。 如果您想要向所有人提供容器内的文件，您可以设置容器是公共使用以下代码︰

    container.SetPermissions(
        new BlobContainerPermissions { PublicAccess = BlobContainerPublicAccessType.Blob });

在 Internet 上的任何人都可以看到 blob 在公共容器中，但您可以修改或删除它们，只有具有适当的帐户访问键或共享的访问签名。

## <a name="upload-a-blob-into-a-container"></a>上载到的容器的 blob

Azure Blob 存储支持块 blob 和页面 blob。  在大多数情况下，块斑点是推荐使用的类型。

若要将文件上载到块斑点，获取容器引用，并使用它来获取块斑点引用。 Blob 引用之后，可以通过调用**UploadFromStream**方法来上载所有的数据的流。 如果它以前不存在，或者覆盖它，如果它不存在，则此操作将创建该 blob。

下面的示例演示如何将 blob 传到一个容器，并假定该容器已创建。

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob");

    // Create or overwrite the "myblob" blob with contents from a local file.
    using (var fileStream = System.IO.File.OpenRead(@"path\myfile"))
    {
        blockBlob.UploadFromStream(fileStream);
    }

## <a name="list-the-blobs-in-a-container"></a>列出容器中的 blob

若要列出容器中的 blob，先获取容器引用。 然后可以使用容器的**ListBlobs**方法来检索 blob 和/或目录中。 若要访问丰富的属性和方法的返回**IListBlobItem**，必须将其转换为**CloudBlockBlob**、 **CloudPageBlob**或**CloudBlobDirectory**对象。  如果该类型是未知的您可以使用执行类型检查来确定要将其转换为。  下面的代码演示如何检索和输出中每个项的 URI`photos`容器︰

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("photos");

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, false))
    {
        if (item.GetType() == typeof(CloudBlockBlob))
        {
            CloudBlockBlob blob = (CloudBlockBlob)item;

            Console.WriteLine("Block blob of length {0}: {1}", blob.Properties.Length, blob.Uri);

        }
        else if (item.GetType() == typeof(CloudPageBlob))
        {
            CloudPageBlob pageBlob = (CloudPageBlob)item;

            Console.WriteLine("Page blob of length {0}: {1}", pageBlob.Properties.Length, pageBlob.Uri);

        }
        else if (item.GetType() == typeof(CloudBlobDirectory))
        {
            CloudBlobDirectory directory = (CloudBlobDirectory)item;

            Console.WriteLine("Directory: {0}", directory.Uri);
        }
    }

如上所示，您可以命名 blob 在其名称中的路径信息。 这将创建一个虚拟目录结构，您可以组织和遍历就像传统的文件系统。 请注意，只是虚拟目录结构，即在 Blob 存储中可用的唯一资源是容器和 blob。 但是，存储客户端库提供了一个**CloudBlobDirectory**对象引用一个虚拟目录并简化了使用这种方式组织的 blob 的过程。

例如，请考虑以下一组名为容器中的块 blob `photos`:

    photo1.jpg
    2010/architecture/description.txt
    2010/architecture/photo3.jpg
    2010/architecture/photo4.jpg
    2011/architecture/photo5.jpg
    2011/architecture/photo6.jpg
    2011/architecture/description.txt
    2011/photo7.jpg

在照片容器 （如上面的示例中） 上调用**ListBlobs**时，将返回层次结构列表。 它包含**CloudBlobDirectory**和**CloudBlockBlob**对象，分别代表目录和 blob 的容器中。 输出结果如下所示︰

    Directory: https://<accountname>.blob.core.windows.net/photos/2010/
    Directory: https://<accountname>.blob.core.windows.net/photos/2011/
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


（可选） 您可以**ListBlobs**方法的**UseFlatBlobListing**参数设置为**true**。 在这种情况下，每个容器中的 blob 被返回一个**CloudBlockBlob**对象。 **ListBlobs**调用以返回平面列表如下所示︰

    // Loop over items within the container and output the length and URI.
    foreach (IListBlobItem item in container.ListBlobs(null, true))
    {
       ...
    }

和结果如下所示︰

    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2010/architecture/description.txt
    Block blob of length 314618: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo3.jpg
    Block blob of length 522713: https://<accountname>.blob.core.windows.net/photos/2010/architecture/photo4.jpg
    Block blob of length 4: https://<accountname>.blob.core.windows.net/photos/2011/architecture/description.txt
    Block blob of length 419048: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo5.jpg
    Block blob of length 506388: https://<accountname>.blob.core.windows.net/photos/2011/architecture/photo6.jpg
    Block blob of length 399751: https://<accountname>.blob.core.windows.net/photos/2011/photo7.jpg
    Block blob of length 505623: https://<accountname>.blob.core.windows.net/photos/photo1.jpg


## <a name="download-blobs"></a>下载 blob

若要下载 blob，首先检索 blob 引用，然后调用**DownloadToStream**方法。 下面的示例使用**DownloadToStream**方法将 blob 内容转移到一个流对象，然后可以保存到一个本地文件。

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "photo1.jpg".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("photo1.jpg");

    // Save blob contents to a file.
    using (var fileStream = System.IO.File.OpenWrite(@"path\myfile"))
    {
        blockBlob.DownloadToStream(fileStream);
    }

您可以使用**DownloadToStream**方法要下载的内容作为文本字符串的 blob。

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob.txt"
    CloudBlockBlob blockBlob2 = container.GetBlockBlobReference("myblob.txt");

    string text;
    using (var memoryStream = new MemoryStream())
    {
        blockBlob2.DownloadToStream(memoryStream);
        text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
    }

## <a name="delete-blobs"></a>删除 blob

若要删除某个 blob，先获取 blob 引用，然后在其上调用**Delete**方法。

    // Retrieve storage account from connection string.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Retrieve reference to a previously created container.
    CloudBlobContainer container = blobClient.GetContainerReference("mycontainer");

    // Retrieve reference to a blob named "myblob.txt".
    CloudBlockBlob blockBlob = container.GetBlockBlobReference("myblob.txt");

    // Delete the blob.
    blockBlob.Delete();


## <a name="list-blobs-in-pages-asynchronously"></a>以异步方式列出网页中的 blob

如果列出了大量的 blob，或想要控制在一个列表操作返回的结果数，您可以列出在结果页中的 blob。 此示例演示如何异步，返回的结果页中，以便执行未被阻塞在等待返回大型结果集时。

本示例显示平面 blob 列出，但您还可以通过设置执行层次结构列表，`useFlatBlobListing`与**ListBlobsSegmentedAsync**方法的参数`false`。

该示例方法调用的异步方法，因为它必须开头`async`关键字，并且它必须返回的**Task**对象。 为**ListBlobsSegmentedAsync**方法指定了 await 关键字挂起执行示例方法直到列表任务完成。

    async public static Task ListBlobsSegmentedInFlatListing(CloudBlobContainer container)
    {
        //List blobs to the console window, with paging.
        Console.WriteLine("List blobs in pages:");

        int i = 0;
        BlobContinuationToken continuationToken = null;
        BlobResultSegment resultSegment = null;

        //Call ListBlobsSegmentedAsync and enumerate the result segment returned, while the continuation token is non-null.
        //When the continuation token is null, the last page has been returned and execution can exit the loop.
        do
        {
            //This overload allows control of the page size. You can return all remaining results by passing null for the maxResults parameter,
            //or by calling a different overload.
            resultSegment = await container.ListBlobsSegmentedAsync("", true, BlobListingDetails.All, 10, continuationToken, null, null);
            if (resultSegment.Results.Count<IListBlobItem>() > 0) { Console.WriteLine("Page {0}:", ++i); }
            foreach (var blobItem in resultSegment.Results)
            {
                Console.WriteLine("\t{0}", blobItem.StorageUri.PrimaryUri);
            }
            Console.WriteLine();

            //Get the continuation token.
            continuationToken = resultSegment.ContinuationToken;
        }
        while (continuationToken != null);
    }

## <a name="writing-to-an-append-blob"></a>追加 blob 写入

追加 blob 是 blob，版本中引入的一种新型 5.x 的.NET Azure 存储客户端库。 对于附加操作，如登录优化追加 blob。 像块 blob，追加 blob 组成的块，但至追加 blob 添加新块时，总是附加到 blob 的末尾。 您无法更新或删除现有块中追加 blob。 因为它们是为阻止 blob 不公开追加 blob 的模块 Id。

在追加 blob 中的每个数据块可以是不同的大小，最多为 4 MB，并追加 blob 可以包含最多为 50000 块。 因此，追加 blob 的最大大小是略大于 195 GB （4 MB X 50000 块）。

下面的示例创建一个新的追加 blob，将一些数据追加到它，模拟一个简单的日志记录操作。

    //Parse the connection string for the storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        Microsoft.Azure.CloudConfigurationManager.GetSetting("StorageConnectionString"));

    //Create service client for credentialed access to the Blob service.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    //Get a reference to a container.
    CloudBlobContainer container = blobClient.GetContainerReference("my-append-blobs");

    //Create the container if it does not already exist.
    container.CreateIfNotExists();

    //Get a reference to an append blob.
    CloudAppendBlob appendBlob = container.GetAppendBlobReference("append-blob.log");

    //Create the append blob. Note that if the blob already exists, the CreateOrReplace() method will overwrite it.
    //You can check whether the blob exists to avoid overwriting it by using CloudAppendBlob.Exists().
    appendBlob.CreateOrReplace();

    int numBlocks = 10;

    //Generate an array of random bytes.
    Random rnd = new Random();
    byte[] bytes = new byte[numBlocks];
    rnd.NextBytes(bytes);

    //Simulate a logging operation by writing text data and byte data to the end of the append blob.
    for (int i = 0; i < numBlocks; i++)
    {
        appendBlob.AppendText(String.Format("Timestamp: {0:u} \tLog Entry: {1}{2}",
            DateTime.UtcNow, bytes[i], Environment.NewLine));
    }

    //Read the append blob to the console window.
    Console.WriteLine(appendBlob.DownloadText());

有关 blob 的三种类型之间的差异的详细信息，请参阅[了解块 Blob、 页面 Blob 和追加的 Blob](https://msdn.microsoft.com/library/azure/ee691964.aspx) 。

## <a name="managing-security-for-blobs"></a>Blob 的安全管理

默认情况下，Azure 存储保留您的数据安全通过限制访问权限的帐户所有者，拥有帐户的访问键。 当需要共享您的存储帐户中的 blob 数据时，务必要这样做而不会影响您的帐户的访问密钥的安全性。 此外，您可以加密以确保其安全将通过电缆和 Azure 存储中的 blob 数据。

[AZURE.INCLUDE [storage-account-key-note-include](../../includes/storage-account-key-note-include.md)]

### <a name="controlling-access-to-blob-data"></a>控制对 blob 数据的访问

默认情况下，您的存储帐户中的 blob 数据都仅存储帐户所有者可以访问。 验证对 Blob 存储请求默认情况下要求帐户访问键。 但是，您可能希望使某个 blob 数据对其他用户可用。 有两种选择︰

- **的匿名访问︰**您可以使容器或其 blob 公开可用于匿名访问。 有关更多信息，请参见[管理匿名的读访问，而对容器和 blob](storage-manage-access-to-resources.md) 。
- **共享访问签名︰**客户端可以提供共享的访问签名 (SAS)，它具有指定的权限，并在指定间隔内提供委派的访问在您的存储帐户的资源。 有关更多信息，请参见[使用共享访问签名 (SAS)](storage-dotnet-shared-access-signature-part-1.md) 。

### <a name="encrypting-blob-data"></a>Blob 数据加密

Azure 存储支持加密 blob 数据在客户端和服务器上︰

- **客户端加密︰**存储客户端库.net 支持加密到 Azure 存储上载和下载到客户端时解密数据之前在客户端应用程序中的数据。 库还支持存储帐户密钥管理与 Azure 密钥存储库的集成。 有关更多信息，请参见[Microsoft Azure 存储.net 客户端的加密](storage-client-side-encryption.md)。 另请参阅[教程︰ 加密和解密使用 Azure 密钥存储库的 Microsoft Azure 存储中的 blob](storage-encrypt-decrypt-blobs-key-vault.md)。
- **服务器端加密**︰ Azure 存储现在支持服务器端的加密。 请参阅[静止 （预览） 数据的 Azure 存储服务加密](storage-service-encryption.md)。

## <a name="next-steps"></a>下一步行动

现在，您已经学习了 Blob 存储的基本知识，按照这些链接以了解详细信息。

### <a name="microsoft-azure-storage-explorer"></a>Microsoft Azure 存储浏览器
- [Microsoft Azure 存储资源管理器 (MASE)](../vs-azure-tools-storage-manage-with-storage-explorer.md)是免费的独立应用程序使您能够以可视方式使用 Azure 存储数据窗口、 OS X 和 Linux 上的 Microsoft。

### <a name="blob-storage-samples"></a>Blob 存储示例

- [要开始使用 Azure 的 Blob 存储在.NET 中](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/)

### <a name="blob-storage-reference"></a>Blob 存储参考

- [对于.NET 引用存储客户端库](http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409)
- [REST API 参考](http://msdn.microsoft.com/library/azure/dd179355)

### <a name="conceptual-guides"></a>概念性参考线

- [使用 AzCopy 命令行实用程序的数据传输](storage-use-azcopy.md)
- [为使.NET 开始使用文件存储](storage-dotnet-how-to-use-files.md)
- [如何使用 WebJobs SDK Azure blob 存储](../app-service-web/websites-dotnet-webjobs-sdk-storage-blobs-how-to.md)

  [Blob5]: ./media/storage-dotnet-how-to-use-blobs/blob5.png
  [Blob6]: ./media/storage-dotnet-how-to-use-blobs/blob6.png
  [Blob7]: ./media/storage-dotnet-how-to-use-blobs/blob7.png
  [Blob8]: ./media/storage-dotnet-how-to-use-blobs/blob8.png
  [Blob9]: ./media/storage-dotnet-how-to-use-blobs/blob9.png

  [Azure Storage Team Blog]: http://blogs.msdn.com/b/windowsazurestorage/
  [Configuring Connection Strings]: http://msdn.microsoft.com/library/azure/ee758697.aspx
  [.NET client library reference]: http://go.microsoft.com/fwlink/?LinkID=390731&clcid=0x409
  [REST API reference]: http://msdn.microsoft.com/library/azure/dd179355
