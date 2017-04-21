<properties
 pageTitle="导入导出 IoT 集线器设备标识的 |Microsoft Azure"
 description="概念和.NET 代码段 IoT 集线器设备标识的大容量管理"
 services="iot-hub"
 documentationCenter=".net"
 authors="dominicbetts"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="10/05/2016"
 ms.author="dobett"/>

# <a name="bulk-management-of-iot-hub-device-identities"></a>批量管理 IoT 集线器设备标识

每个 IoT 中枢有设备标识注册表，您可以使用服务，如队列，其中包含云至设备信息传输的数据中创建每个设备的资源。 设备标识注册表还使您可以控制访问面向设备的端点。 本文描述如何导入和导出中大容量设备标识与设备标识注册表。

导入和导出操作中发生的*作业*，使您能够执行批量 IoT 中心针对服务操作的上下文中。

**RegistryManager**类包含的**ExportDevicesAsync**和**ImportDevicesAsync**方法的使用**作业**框架。 这些方法使您可以导出、 导入和同步整个 IoT 集线器设备注册表。

## <a name="what-are-jobs"></a>作业是什么？

设备标识注册表操作使用的**作业**系统时操作︰

*  具有与标准运行时操作，相比很可能长执行时间或
*  返回给用户的数据量大。

在这些情况下，而不是等待或阻塞的操作，结果一次 API 调用该操作以异步方式创建该 IoT 集线器**作业**。 然后此操作立即返回一个**JobProperties**对象。

下面的 C# 代码段显示了如何创建导出作业︰

```
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);
```

然后可以使用**RegistryManager**类来查询使用返回的**JobProperties**元数据的**作业**的状态。

下面的 C# 代码段演示如何轮询每五秒钟查看已执行完成该作业︰

```
// Wait until job is finished
while(true)
{
  exportJob = await registryManager.GetJobAsync(exportJob.JobId);
  if (exportJob.Status == JobStatus.Completed || 
      exportJob.Status == JobStatus.Failed ||
      exportJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="export-devices"></a>导出设备

使用**ExportDevicesAsync**方法将整个 IoT 集线器设备注册表导出到使用[共享访问签名](https://msdn.microsoft.com/library/ee395415.aspx) [Azure 存储](https://azure.microsoft.com/documentation/services/storage/)blob 容器。

此方法使您可以在一个 blob 容器，您可以控制创建可靠的备份设备信息。

**ExportDevicesAsync**方法需要两个参数︰

*  一个*字符串*，其中包含 blob 容器的 URI。 此 URI 必须包含一个 SAS 标记，用于授予对该容器的写访问权限。 作业创建此容器，以存储序列化的输出设备数据块斑点。 SAS 标记必须包括这些权限︰
    
    ```
    SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
    ```

*  一个*布尔值*，该值指示是否要从导出数据中排除身份验证密钥。 如果**false**，身份验证密钥包含在导出输出;否则，密钥会被导出为**null**。

下面的 C# 代码段演示如何启动导出数据中包括设备验证密钥导出作业，然后轮询完成︰

```
// Call an export job on the IoT Hub to retrieve all devices
JobProperties exportJob = await registryManager.ExportDevicesAsync(containerSasUri, false);

// Wait until job is finished
while(true)
{
    exportJob = await registryManager.GetJobAsync(exportJob.JobId);
    if (exportJob.Status == JobStatus.Completed || 
        exportJob.Status == JobStatus.Failed ||
        exportJob.Status == JobStatus.Cancelled)
    {
    // Job has finished executing
    break;
    }

    await Task.Delay(TimeSpan.FromSeconds(5));
}
```

作业为使用名称**devices.txt**块斑点提供的 blob 容器中存储相应的输出。 输出数据由 JSON 序列化设备数据，每行一个设备组成。

下面的示例显示输出的数据︰

```
{"id":"Device1","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device2","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device3","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device4","eTag":"MA==","status":"disabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
{"id":"Device5","eTag":"MA==","status":"enabled","authentication":{"symmetricKey":{"primaryKey":"abc=","secondaryKey":"def="}}}
```

如果您需要对代码中的此数据的访问，您可以轻松地反序列化此数据使用**ExportImportDevice**类。 下面的 C# 代码段演示如何读取块斑点到以前导出的设备信息︰

```
var exportedDevices = new List<ExportImportDevice>();

using (var streamReader = new StreamReader(await blob.OpenReadAsync(AccessCondition.GenerateIfExistsCondition(), RequestOptions, null), Encoding.UTF8))
{
  while (streamReader.Peek() != -1)
  {
    string line = await streamReader.ReadLineAsync();
    var device = JsonConvert.DeserializeObject<ExportImportDevice>(line);
    exportedDevices.Add(device);
  }
}
```

> [AZURE.NOTE]  您可以使用**RegistryManager**类的**GetDevicesAsync**方法来获取您的设备的列表。 但是，这种方法具有 1000年硬帽上的设备所返回的对象数。 **GetDevicesAsync**方法的预期的使用情况用于开发方案，以帮助进行调试，不建议在生产负载。

## <a name="import-devices"></a>导入设备

在**RegistryManager**类中的**ImportDevicesAsync**方法可以 IoT 集线器设备的注册表中执行批量导入和同步操作。 与**ExportDevicesAsync**方法中， **ImportDevicesAsync**方法使用**作业**框架。

小心使用**ImportDevicesAsync**方法，因为除了调配设备标识注册表中的新设备，它可以同时更新和删除现有设备。

> [AZURE.WARNING]  导入操作不能撤消。 始终先备份现有数据对您的设备标识注册表进行批量更改之前使用另一个 blob 容器的**ExportDevicesAsync**方法。

**ImportDevicesAsync**方法采用两个参数︰

*  一个*字符串*，包含 URI 的[Azure 存储](https://azure.microsoft.com/documentation/services/storage/)blob 容器，用作*输入*到工作。 此 URI 必须包含一个 SAS 标记，用于授予对该容器的读取访问权。 这个容器必须包含 blob 与包含序列化的设备数据导入到您的设备标识注册表名称**devices.txt** 。 导入数据必须包含在**ExportImportDevice**作业时它会创建一个**devices.txt** blob 使用同一个 JSON 格式的设备信息。 SAS 标记必须包括这些权限︰

    ```
    SharedAccessBlobPermissions.Read
    ```

*  包含要作为*输出*作业中使用[Azure 存储](https://azure.microsoft.com/documentation/services/storage/)blob 容器的 URI 的*字符串*。 在此容器，以存储从完成导入**作业**的任何错误信息，作业创建块斑点。 SAS 标记必须包括这些权限︰
    
    ```
    SharedAccessBlobPermissions.Write | SharedAccessBlobPermissions.Read | SharedAccessBlobPermissions.Delete
    ```

> [AZURE.NOTE]  两个参数可以指向相同的 blob 容器。 单独的参数只是启用更好地控制您的数据与输出容器需要更多权限。

下面的 C# 代码段演示如何启动导入作业︰

```
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);
```

## <a name="import-behavior"></a>导入行为

**ImportDevicesAsync**方法可用于在设备标识注册表中执行下面的批量操作︰

-   批量注册的新设备
-   现有设备的批量删除
-   大容量的更改状态 （启用或禁用设备）
-   新设备的身份验证密钥的大容量分配
-   大容量自动再生设备验证密钥

您可以执行上述操作的单个**ImportDevicesAsync**调用中的任意组合。 例如，可以注册新的设备和删除或在同一时间更新现有设备。 当与**ExportDevicesAsync**方法一起使用，完全可以到另一个 IoT 集线器从迁移所有设备。

使用可选的**importMode**属性中导入序列化数据的每个设备以控制导入过程每个设备。 **ImportMode**属性具有以下选项︰

| importMode |  说明 |
| -------- | ----------- |
| **createOrUpdate** | 如果设备不存在具有指定的**id**，它是新注册。 <br/>如果设备已存在，而无需考虑的**ETag**值提供的输入数据是覆盖现有信息。 |
| **创建** | 如果设备不存在具有指定的**id**，它是新注册。 <br/>如果该设备已存在，则错误写入日志文件。 |
| **更新** | 如果一个设备已经存在具有指定的**id**，而不考虑**ETag**值提供的输入数据是覆盖现有信息。 <br/>如果该设备不存在，则错误写入日志文件。 |
| **updateIfMatchETag** | 如果设备已存在具有指定的**id**，只有一个**ETag**匹配提供输入数据覆盖现有信息。 <br/>如果该设备不存在，则错误写入日志文件。 <br/>如果有**ETag**不匹配，则错误写入日志文件。 |
| **createOrUpdateIfMatchETag** | 如果设备不存在具有指定的**id**，它是新注册。 <br/>如果设备已存在，只有一个**ETag**匹配提供输入数据覆盖现有信息。 <br/>如果有**ETag**不匹配，则错误写入日志文件。 |
| **删除** | 如果一个设备已经存在具有指定**id**，则将它删除而不考虑**ETag**值。 <br/>如果该设备不存在，则错误写入日志文件。 |
| **deleteIfMatchETag** | 如果一个设备已经存在具有指定的**id**，删除只有一个**ETag**匹配。 如果该设备不存在，则错误写入日志文件。 <br/>如果有 ETag 不匹配，则错误写入日志文件。 |

> [AZURE.NOTE] 如果序列化数据未显式定义设备**importMode**标志，则默认为**createOrUpdate**在导入操作。

## <a name="import-devices-example--bulk-device-provisioning"></a>导入设备的示例--大容量设备资源调配 

下面的 C# 代码示例说明了如何生成多个设备标识的︰

- 包括身份验证密钥。
- 该设备信息写入块斑点。
- 导入设备标识注册表中的设备。

```
// Provision 1,000 more devices
var serializedDevices = new List<string>();

for (var i = 0; i < 1000; i++)
{
// Create a new ExportImportDevice
  var deviceToAdd = new ExportImportDevice()
  {
    Id = Guid.NewGuid().ToString(),
    Status = DeviceStatus.Enabled,
    Authentication = new AuthenticationMechanism()
    {
      SymmetricKey = new SymmetricKey()
      {
        PrimaryKey = CryptoKeyGenerator.GenerateKey(32),
        SecondaryKey = CryptoKeyGenerator.GenerateKey(32)
      }
    },
    ImportMode = ImportMode.Create
  };

  // Add device to existing list
  serializedDevices.Add(JsonConvert.SerializeObject(deviceToAdd));
}

// Write this list to the blob
var sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice => sb.AppendLine(serializedDevice));
await blob.DeleteIfExistsAsync();

using (CloudBlobStream stream = await blob.OpenWriteAsync())
{
  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
  for (var i = 0; i < bytes.Length; i += 500)
  {
    int length = Math.Min(bytes.Length - i, 500);
    await stream.WriteAsync(bytes, i, length);
  }
}

// Call import using the same blob to add new devices!
// This normally takes 1 minute per 100 devices the normal way
JobProperties importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

// Wait until job is finished
while(true)
{
  importJob = await registryManager.GetJobAsync(importJob.JobId);
  if (importJob.Status == JobStatus.Completed || 
      importJob.Status == JobStatus.Failed ||
      importJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}
```

## <a name="import-devices-example--bulk-deletion"></a>导入设备示例 – 批量删除

下面的代码示例演示如何删除您使用上面的代码示例添加的设备︰

```
// Step 1: Update each device's ImportMode to be Delete
sb = new StringBuilder();
serializedDevices.ForEach(serializedDevice =>
{
  // Deserialize back to an ExportImportDevice
  var device = JsonConvert.DeserializeObject<ExportImportDevice>(serializedDevice);

  // Update property
  device.ImportMode = ImportMode.Delete;

  // Re-serialize
  sb.AppendLine(JsonConvert.SerializeObject(device));
});

// Step 2: Write the new import data back to the block blob
await blob.DeleteIfExistsAsync();
using (CloudBlobStream stream = await blob.OpenWriteAsync())
{
  byte[] bytes = Encoding.UTF8.GetBytes(sb.ToString());
  for (var i = 0; i < bytes.Length; i += 500)
  {
    int length = Math.Min(bytes.Length - i, 500);
    await stream.WriteAsync(bytes, i, length);
  }
}

// Step 3: Call import using the same blob to delete all devices!
importJob = await registryManager.ImportDevicesAsync(containerSasUri, containerSasUri);

// Wait until job is finished
while(true)
{
  importJob = await registryManager.GetJobAsync(importJob.JobId);
  if (importJob.Status == JobStatus.Completed || 
      importJob.Status == JobStatus.Failed ||
      importJob.Status == JobStatus.Cancelled)
  {
    // Job has finished executing
    break;
  }

  await Task.Delay(TimeSpan.FromSeconds(5));
}

```

## <a name="getting-the-container-sas-uri"></a>获取容器 SAS URI


下面的代码示例演示如何生成[SAS 的 URI](../storage/storage-dotnet-shared-access-signature-part-2.md)具有读取、 写入和删除权限的 blob 容器︰

```
static string GetContainerSasUri(CloudBlobContainer container)
{
  // Set the expiry time and permissions for the container.
  // In this case no start time is specified, so the
  // shared access signature becomes valid immediately.
  var sasConstraints = new SharedAccessBlobPolicy();
  sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
  sasConstraints.Permissions = 
    SharedAccessBlobPermissions.Write | 
    SharedAccessBlobPermissions.Read | 
    SharedAccessBlobPermissions.Delete;

  // Generate the shared access signature on the container,
  // setting the constraints directly on the signature.
  string sasContainerToken = container.GetSharedAccessSignature(sasConstraints);

  // Return the URI string for the container,
  // including the SAS token.
  return container.Uri + sasContainerToken;
}

```

## <a name="next-steps"></a>下一步行动

在本文中，您学习了如何 IoT 中心执行批量操作，而对设备标识注册表。 按照这些链接以了解更多关于管理 Azure IoT 中心︰

- [使用情况指标][lnk-metrics]
- [操作监视][lnk-monitor]

要进一步探索 IoT 中心的功能，请参阅︰

- [开发人员指南][lnk-devguide]
- [模拟具有 IoT 网关 SDK 的设备][lnk-gateway]

[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md