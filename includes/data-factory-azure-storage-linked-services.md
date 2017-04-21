## <a name="azure-storage-linked-service"></a>Azure 存储链接服务

**Azure 存储链接服务**允许您通过使用**帐户密钥**将 Azure 存储帐户链接到 Azure 数据工厂。 这为提供全球访问到 Azure 存储数据工厂。 下表提供了 JSON 元素特定于 Azure 存储链接服务的说明。

| 属性 | 说明 | 必填 |
| :-------- | :----------- | :-------- |
| 类型 | 类型属性必须设置为︰ **AzureStorage** | 是的 |
| 连接字符串 | 指定连接到 Azure 存储连接字符串属性所需的信息。 | 是的 |

Azure 存储，请参阅以下文章为视图/复制帐户密码的步骤︰[查看、 复制和重新生成存储访问键](../storage/storage-create-storage-account.md#view-copy-and-regenerate-storage-access-keys)。

**示例︰**  
  
    {  
        "name": "StorageLinkedService",  
        "properties": {  
            "type": "AzureStorage",  
            "typeProperties": {  
                "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"  
            }  
        }  
    }  


## <a name="azure-storage-sas-linked-service"></a>Azure 存储 Sa 链接服务  
共享的访问签名 (SAS) 提供委派的访问您的存储帐户中的资源。 这意味着您可以授予客户端而无需共享您的帐户访问键指定期限内的时间，并具有一组指定的权限，限制对您的存储帐户中的对象的权限。 SAS 是一个 URI，它涵盖了在查询参数所需的信息的所有经过身份验证的访问存储资源。 若要访问存储资源与 SAS，客户端仅需要在 SAS 中传递给适当的构造函数或方法。 有关 SA 的详细信息，请参阅[共享访问签名︰ 了解 SAS 模型](../articles/storage/storage-dotnet-shared-access-signature-part-1.md)
  
链接的 Azure 存储 SA 服务允许您通过使用共享访问签名 (SA) 将 Azure 存储帐户链接到 Azure 数据工厂。 它提供了对存储中的所有或特定资源 （blob/容器） 的限制/时间绑定访问数据工厂。 下表提供了 JSON 元素特定于 Azure 存储 SA 链接服务的说明。 

| 属性 | 说明 | 必填 |
| :-------- | :----------- | :-------- |
| 类型 | 类型属性必须设置为︰ **AzureStorageSas**  | 是的 |
| sasUri | 如 blob、 容器或表的 Azure 存储资源指定共享访问签名 URI。 以下注意事项的详细信息，请参阅。 | 是的 | 


**示例︰**
  
    {  
        "name": "StorageSasLinkedService",  
        "properties": {  
            "type": "AzureStorageSas",  
            "typeProperties": {  
                "sasUri": "<storageUri>?<sasToken>"   
            }  
        }  
    }  

在创建一个**SAS URI**时，考虑以下因素︰  

- Azure 数据工厂支持仅**服务 SAS**，不帐户 SAS。 有关这两种类型的详细信息，请参见[类型的共享访问签名](../articles/storage/storage-dotnet-shared-access-signature-part-1.md#types-of-shared-access-signatures)。
- 需要在基于链接的服务 （读、 写、 读/写） 将如何使用数据工厂在对象上设置适当的读写**权限**。
- 需要适当地设置**过期时间**。 请确保对 Azure 存储对象的访问权限不会活动的管线段内过期。
- 应在适当的容器 blob 或表级别根据需要创建 Uri。 SAS Uri 到 Azure 的 blob 允许访问该特定 blob 数据工厂服务。 SAS Uri 到 Azure blob 容器允许循环访问该容器中的 blob 数据工厂服务。 如果您需要以后，更多/少对象提供访问或更新 SAS URI，请记住要用新的 URI 更新链接的服务。   
