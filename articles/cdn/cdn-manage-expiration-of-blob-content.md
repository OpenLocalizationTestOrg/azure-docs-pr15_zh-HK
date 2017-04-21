<properties
 pageTitle="管理在 Azure CDN 的 Azure 存储 blob 内容过期 |Microsoft Azure"
 description="了解有关用于控制对 Azure CDN 缓存中的 blob 的生存时间。"
 services="cdn"
 documentationCenter=""
 authors="camsoper"
 manager="erikre"
 editor=""/>
<tags
 ms.service="cdn"
 ms.workload="media"
 ms.tgt_pltfrm="na"
 ms.devlang="multiple"
 ms.topic="article"
 ms.date="09/15/2016"
 ms.author="casoper"/>


# <a name="manage-expiration-of-azure-storage-blob-content-in-azure-cdn"></a>管理在 Azure CDN 的 Azure 存储 blob 内容过期

> [AZURE.SELECTOR]
- [Azure 的 Web 应用程序/云服务，ASP.NET 或 IIS](cdn-manage-expiration-of-cloud-service-content.md)
- [Azure 存储 blob 服务](cdn-manage-expiration-of-blob-content.md)

在[Azure 存储](../storage/storage-introduction.md) [blob 服务](../storage/storage-introduction.md#blob-storage)是 Azure CDN 与集成在一起的几种基于 Azure 的来源之一。  可以在 Azure CDN 缓存可公开访问 blob 中的任何内容，直到其生存时间 (TTL) 经过。  TTL 由 HTTP 响应从 Azure 存储中的[*缓存控制*标头](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)。

>[AZURE.TIP] 您可以选择设置一个 blob 没有 TTL。  在这种情况下，Azure CDN 自动应用默认的 TTL 数为七天。
>
>加快访问 blob 和其他文件的 Azure CDN 工作原理的更多信息，请参阅[Azure CDN 概述](./cdn-overview.md)。
>
>Azure 存储 blob 服务的详细信息，请参阅[Blob 服务概念](https://msdn.microsoft.com/library/dd179376.aspx)。 

本教程演示了几种方法，您可以在 Azure 存储中的 blob 设置 TTL。  

## <a name="azure-powershell"></a>Azure PowerShell

[Azure PowerShell](../powershell-install-configure.md)是管理 Azure 服务最快捷、 最有效的方法之一。  使用`Get-AzureStorageBlob`cmdlet 以获取引用斑点，然后设置`.ICloudBlob.Properties.CacheControl`属性。 

```powershell
# Create a storage context
$context = New-AzureStorageContext -StorageAccountName "<storage account name>" -StorageAccountKey "<storage account key>"

# Get a reference to the blob
$blob = Get-AzureStorageBlob -Context $context -Container "<container name>" -Blob "<blob name>"

# Set the CacheControl property to expire in 1 hour (3600 seconds)
$blob.ICloudBlob.Properties.CacheControl = "public, max-age=3600"

# Send the update to the cloud
$blob.ICloudBlob.SetProperties()
```

>[AZURE.TIP] 您还可以使用 PowerShell[管理 CDN 配置文件和终结点](./cdn-manage-powershell.md)。

## <a name="azure-storage-client-library-for-net"></a>Azure 存储.NET 客户端库

若要设置使用.NET 的 blob 的 TTL，请使用[Azure 存储.NET 客户端库](../storage/storage-dotnet-how-to-use-blobs.md)将[CloudBlob.Properties.CacheControl](https://msdn.microsoft.com/library/microsoft.windowsazure.storage.blob.blobproperties.cachecontrol.aspx)属性设置。

```csharp
class Program
{
    const string connectionString = "<storage connection string>";
    static void Main()
    {
        // Retrieve storage account information from connection string
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(connectionString);
        
        // Create a blob client for interacting with the blob service.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
        
        // Create a reference to the container
        CloudBlobContainer container = blobClient.GetContainerReference("<container name>");

        // Create a reference to the blob
        CloudBlob blob = container.GetBlobReference("<blob name>");

        // Set the CacheControl property to expire in 1 hour (3600 seconds)
        blob.Properties.CacheControl = "public, max-age=3600";

        // Update the blob's properties in the cloud
        blob.SetProperties();
    }
}
```

>[AZURE.TIP] [.Net 的 Azure Blob 存储样本](https://azure.microsoft.com/documentation/samples/storage-blob-dotnet-getting-started/)中有很多更多的.NET 代码示例。

## <a name="other-methods"></a>其他方法

- [Azure 的命令行界面](../xplat-cli-install.md)

    当上传该 blob，设置*cacheControl*属性使用`-p`开关。  本示例将 TTL 设置为一小时 （3600 秒）。

    ```text
    azure storage blob upload -c <connectionstring> -p cacheControl="public, max-age=3600" .\test.txt myContainer test.txt
    ```

- [Azure 存储服务 REST API，](https://msdn.microsoft.com/library/azure/dd179355.aspx)

    在[将 Blob](https://msdn.microsoft.com/en-us/library/azure/dd179451.aspx)、[放块列表](https://msdn.microsoft.com/en-us/library/azure/dd179467.aspx)或[设置 Blob 属性](https://msdn.microsoft.com/library/azure/ee691966.aspx)请求显式设置*x-ms-blob 的缓存的控件*属性。

- 第三方存储管理工具

    某些第三方 Azure 存储管理工具允许您在 blob 设置*CacheControl*属性。 

## <a name="testing-the-cache-control-header"></a>测试*缓存控制*标头

您可以轻松地验证您 blob 的 TTL。  使用浏览器的[开发人员工具](https://developer.microsoft.com/microsoft-edge/platform/documentation/f12-devtools-guide/)，测试您的斑点包括*缓存控制*响应标头。  此外可以使用**wget**，[把邮递员弄](https://www.getpostman.com/)或[Fiddler](http://www.telerik.com/fiddler)等工具检查响应标头。

## <a name="next-steps"></a>下一步行动

- [了解*缓存控制*标头](http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
- [了解如何管理在 Azure CDN 的云服务内容过期](./cdn-manage-expiration-of-cloud-service-content.md)

