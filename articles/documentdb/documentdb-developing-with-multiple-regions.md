<properties
   pageTitle="使用 DocumentDB 中的多个区域进行开发 |Microsoft Azure"
   description="了解如何从 Azure DocumentDB，完全托管 NoSQL 数据库服务访问您的数据在多个地区。"
   services="documentdb"
   documentationCenter=""
   authors="kiratp"
   manager="jhubbard"
   editor=""/>

<tags
   ms.service="documentdb"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="10/25/2016"
   ms.author="kipandya"/>
   
# <a name="developing-with-multi-region-documentdb-accounts"></a>开发与多区域 DocumentDB 帐户

> [AZURE.NOTE] DocumentDB 数据库的全局分布是普遍适用和自动启用任何新创建的 DocumentDB 帐户。 我们正在努力使全球分布上现有的所有帐户，但在此期间，如果您希望在您的帐户上启用全局通讯组请[与支持人员联系](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)，我们将它为您立即启用。

要充分利用[全局通讯组](documentdb-distribute-data-globally.md)，客户端应用程序可以指定的区域，以用于执行文档操作的排序首选项列表。 这可以通过设置连接策略。 基于 Azure DocumentDB 帐户配置、 当前区域的可用性和指定的首选项列表，将通过 SDK 以执行写入和读取操作选择最优的终结点。 

在使用 DocumentDB 客户端 Sdk 连接的初始化时指定此首选项列表。 Sdk 接受可选参数"PreferredLocations"，它是 Azure 区域的排序的列表。

SDK 将自动发送所有写入当前都写入的区域。 

所有读取将被都发送到 PreferredLocations 中的第一个可用区域。 如果请求失败，客户端将为下一步的区域列表中向下失败等等。 

读取客户端 Sdk 将只尝试从区域中指定的 PreferredLocations。 因此，例如，如果数据库帐户可在三个地区，但客户端仅指定两个区域内的非写为 PreferredLocations，然后读取提供翻译写，即使在故障转移。

应用程序可以验证当前写终结点和读取终结点选择通过 SDK 通过检查 WriteEndpoint 和 ReadEndpoint，可在 SDK 版本 1.8 和上面的两个属性。 

如果未设置 PreferredLocations 属性，将从当前写入的区域提供服务的所有请求。 


## <a name="net-sdk"></a>.NET SDK
无需进行任何代码更改，可以使用 SDK。 在这种情况下，SDK 自动指示两个读取和写入当前写入的区域。 

在.NET SDK 1.8 或更高版本中，DocumentClient 构造函数的 ConnectionPolicy 参数具有名为 Microsoft.Azure.Documents.ConnectionPolicy.PreferredLocations 的属性。 此属性是集合类型的`<string>`和应包含的区域名称的列表。 按照[Azure 区域]的区域名称列格式化字符串值 [regions]页，并且不包含空格的前面或后面第一个和最后一个字符分别。

当前写入和读取终结点分别在 DocumentClient.WriteEndpoint 和 DocumentClient.ReadEndpoint 中可用。

> [AZURE.NOTE] 为终结点的 Url 不应视为长期存在的常量。 服务可能会更新这些任意时刻。 SDK 自动处理此更改。

    // Getting endpoints from application settings or other configuration location
    Uri accountEndPoint = new Uri(Properties.Settings.Default.GlobalDatabaseUri);
    string accountKey = Properties.Settings.Default.GlobalDatabaseKey;

    //Setting read region selection preference 
    connectionPolicy.PreferredLocations.Add(LocationNames.WestUS); // first preference
    connectionPolicy.PreferredLocations.Add(LocationNames.EastUS); // second preference
    connectionPolicy.PreferredLocations.Add(LocationNames.NorthEurope); // third preference

    // initialize connection
    DocumentClient docClient = new DocumentClient(
        accountEndPoint,
        accountKey,
        connectionPolicy);

    // connect to DocDB 
    await docClient.OpenAsync().ConfigureAwait(false);


## <a name="nodejs-javascript-and-python-sdks"></a>NodeJS、 JavaScript 和 Python Sdk
无需进行任何代码更改，可以使用 SDK。 在这种情况下，SDK 将自动直接读取和写入当前写入的区域。 

在 1.8 及更高版本的每个 SDK 版本中，DocumentClient 构造函数的 ConnectionPolicy 参数新属性调用 DocumentClient.ConnectionPolicy.PreferredLocations。 这是参数采用的区域名称列表的字符串数组。 在[Azure 区域]的区域名称列按照格式化名称 [regions]页。 也可以使用预定义的常量中方便对象 AzureDocuments.Regions

当前写入和读取终结点分别在 DocumentClient.getWriteEndpoint 和 DocumentClient.getReadEndpoint 中可用。

> [AZURE.NOTE] 为终结点的 Url 不应视为长期存在的常量。 服务可能会更新这些任意时刻。 SDK 将自动处理此更改。

以下是 NodeJS/Javascript 代码示例。 Python 和 Java 将遵循相同的模式。

    // Creating a ConnectionPolicy object
    var connectionPolicy = new DocumentBase.ConnectionPolicy();
    
    // Setting read region selection preference, in the following order -
    // 1 - West US
    // 2 - East US
    // 3 - North Europe
    connectionPolicy.PreferredLocations = ['West US', 'East US', 'North Europe'];
    
    // initialize the connection
    var client = new DocumentDBClient(host, { masterKey: masterKey }, connectionPolicy);


## <a name="rest"></a>其余部分 
一旦数据库帐户已可在多个区域中，客户端可以通过在以下 URI 上执行的 GET 请求查询其可用性。

    https://{databaseaccount}.documents.azure.com/

该服务将返回地区的列表以及副本及其相应 DocumentDB 端点的 Uri。 当前写入的区域将在响应中表示。 客户机然后可以按如下所述选择所有进一步的 REST API 请求的相应终结点。

示例响应

    {
        "_dbs": "//dbs/",
        "media": "//media/",
        "writableLocations": [
            {
                "Name": "West US",
                "DatabaseAccountEndpoint": "https://globaldbexample-westus.documents.azure.com:443/"
            }
        ],
        "readableLocations": [
            {
                "Name": "East US",
                "DatabaseAccountEndpoint": "https://globaldbexample-eastus.documents.azure.com:443/"
            }
        ],
        "MaxMediaStorageUsageInMB": 2048,
        "MediaStorageUsageInMB": 0,
        "ConsistencyPolicy": {
            "defaultConsistencyLevel": "Session",
            "maxStalenessPrefix": 100,
            "maxIntervalInSeconds": 5
        },
        "addresses": "//addresses/",
        "id": "globaldbexample",
        "_rid": "globaldbexample.documents.azure.com",
        "_self": "",
        "_ts": 0,
        "_etag": null
    }


-   所有传、 发布和删除请求必须转至指定写 URI
-   所有获取和其他的只读请求 （例如查询） 可以转到客户端选择的任何终结点

写入到只读区域的请求将失败，并 HTTP 错误代码 403 （"禁止"）。

如果写区域更改客户端的初始发现阶段后，前面写区域的后续写操作失败，出现 HTTP 错误代码 403 （"禁止"）。 客户端然后应得到区域再次以获取已更新的写入区域列表。

## <a name="next-steps"></a>下一步行动

了解更多有关数据分发全局与 DocumentDB 中的以下文章︰

- [将全局与 DocumentDB 的数据分发](documentdb-distribute-data-globally.md)
- [一致性级别](documentdb-consistency-levels.md)
- [吞吐量如何处理多个区域](documentdb-manage.md#how-throughput-works-with-multiple-regions)
- [添加使用 Azure 的门户区域](documentdb-portal-global-replication.md)

[regions]: https://azure.microsoft.com/regions/ 
