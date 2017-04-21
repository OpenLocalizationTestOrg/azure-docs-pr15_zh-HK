<properties
    pageTitle="逻辑应用程序中添加连接器的 Azure blob 存储 |Microsoft Azure"
    description="Azure blob 存储连接器使用 REST API 的参数的概述"
    services=""
    documentationCenter="" 
    authors="MandiOhlinger"
    manager="anneta"
    editor=""
    tags="connectors"/>

<tags
   ms.service="logic-apps"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="integration" 
   ms.date="10/18/2016"
   ms.author="mandia"/>

# <a name="get-started-with-the-azure-blob-storage-connector"></a>开始使用 Azure blob 存储连接器
Azure Blob 存储是一种服务来存储大量的非结构化数据。 执行各种操作，例如上载、 更新、 获取，并删除在 Azure blob 存储 blob。 

使用 Azure blob 存储，您︰

- 上传新的项目，或获取最近已更新的文件生成工作流。
- 使用操作来获取文件的元数据、 删除文件、 复制文件等。 例如，一个工具更新时在 Azure 网站 （触发器），然后更新 blob 存储 （操作） 中的文件。 

本主题演示如何使用 blob 存储接口在逻辑应用程序中，并也列出的操作。

>[AZURE.NOTE] 此版本的文章将应用于逻辑应用程序一般可用性 (GA)。 

要了解更多有关逻辑应用程序，请参见[什么是逻辑应用程序](../app-service-logic/app-service-logic-what-are-logic-apps.md)并[创建一个逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)。

## <a name="connect-to-azure-blob-storage"></a>连接到 Azure blob 存储

逻辑应用程序可以访问的任何服务之前，您首先创建一个*连接*到的服务。 连接提供一个逻辑应用程序和其他服务之间的连接。 例如，若要连接到的存储帐户，您首先创建 blob 存储的*连接*。 若要创建连接，请输入通常用于访问要连接到此服务的凭据。 因此使用 Azure 存储时，输入的凭据到您的存储帐户来创建连接。 

#### <a name="create-the-connection"></a>创建连接

>[AZURE.INCLUDE [Create a connection to Azure blob storage](../../includes/connectors-create-api-azureblobstorage.md)]
 
## <a name="use-a-trigger"></a>使用触发器

此接口不具有任何触发器。 使用其他触发器来启动逻辑的应用程序，如定期触发器、 HTTP Webhook 触发器，触发器可用于其他连接器，和更多。 [创建一个逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)提供了一个示例。

## <a name="use-an-action"></a>使用的操作
    
操作是由逻辑应用程序中定义的工作流执行的操作。

1. 选择加号。 看了几种选择︰**将操作添加**、**添加一个条件**，或**多个**选项之一。

    ![](./media/connectors-create-api-azureblobstorage/add-action.png)

2. 选择**添加操作**。

3. 在文本框中，键入"blob"以获取所有可用操作的列表。

    ![](./media/connectors-create-api-azureblobstorage/actions.png) 

4. 在我们的示例中，选择**AzureBlob-获取文件元数据中使用的路径**。 如果已存在一个连接，然后选择**...**（显示选取器） 按钮以选择一个文件。

    ![](./media/connectors-create-api-azureblobstorage/sample-file.png)

    如果系统提示您输入连接信息，然后输入要创建的连接的详细信息。 本主题中[创建的连接](connectors-create-api-azureblobstorage.md#create-the-connection)描述了这些属性。 

    > [AZURE.NOTE] 在此示例中，我们得到一个文件的元数据。 若要查看元数据，添加另一个操作，创建一个新文件，使用另一个接头。 例如，添加一个 OneDrive 操作，创建基于元数据的新的"测试"文件。 

5. **保存**所做的更改 （左上的角的工具栏）。 逻辑应用程序保存，并且可能会自动被启用。

> [AZURE.TIP] [存储浏览器](http://storageexplorer.com/)是强大的工具来管理多个存储帐户。

## <a name="technical-details"></a>技术详细信息

## <a name="storage-blob-actions"></a>存储 Blob 操作

|操作|说明|
|--- | ---|
|[获取文件的元数据](connectors-create-api-azureblobstorage.md#get-file-metadata)|此操作获取文件元数据中使用文件 id。|
|[更新文件](connectors-create-api-azureblobstorage.md#update-file)|此操作会更新文件。|
|[删除文件](connectors-create-api-azureblobstorage.md#delete-file)|此操作将删除一个文件。|
|[获取使用路径的文件元数据](connectors-create-api-azureblobstorage.md#get-file-metadata-using-path)|此操作获取文件元数据使用的路径。|
|[获取使用路径的文件内容](connectors-create-api-azureblobstorage.md#get-file-content-using-path)|此操作获取文件内容使用的路径。|
|[获取文件的内容](connectors-create-api-azureblobstorage.md#get-file-content)|此操作获取使用 id 的文件内容。|
|[创建文件](connectors-create-api-azureblobstorage.md#create-file)|此操作将文件上载。|
|[复制文件](connectors-create-api-azureblobstorage.md#copy-file)|此操作将文件复制到 Azure Blob 存储。|
|[提取存档文件夹](connectors-create-api-azureblobstorage.md#extract-archive-to-folder)|此操作提取到一个文件夹的存档文件 (示例︰.zip)。|

### <a name="action-details"></a>操作详细信息

在此部分，查看有关每个操作，包括任何必需的还是可选的输入的属性，以及任何相应的输出与连接器相关联的特定详细信息。

#### <a name="get-file-metadata"></a>获取文件的元数据
此操作获取文件元数据中使用文件 id。  

|属性名称| 显示名称|说明|
| ---|---|---|
|id *|文件|选择一个文件|

星号 （*） 表示该属性是必需的。

##### <a name="output-details"></a>输出详细信息
BlobMetadata

| 属性名称 | 数据类型 |
|---|---|
|标识|字符串|
|名称|字符串|
|显示名称|字符串|
|路径|字符串|
|上次更改时间|字符串|
|大小|整数|
|媒体类型|字符串|
|IsFolder|布尔值|
|ETag|字符串|
|FileLocator|字符串|


#### <a name="update-file"></a>更新文件
此操作会更新文件。  

|属性名称| 显示名称|说明|
| ---|---|---|
|id *|文件|选择一个文件|
|正文 *|文件内容|要更新的文件的内容|

星号 （*） 表示该属性是必需的。

##### <a name="output-details"></a>输出详细信息
BlobMetadata

| 属性名称 | 数据类型 |
|---|---|
|标识|字符串|
|名称|字符串|
|显示名称|字符串|
|路径|字符串|
|上次更改时间|字符串|
|大小|整数|
|媒体类型|字符串|
|IsFolder|布尔值|
|ETag|字符串|
|FileLocator|字符串|


#### <a name="delete-file"></a>删除文件
此操作将删除一个文件。  

|属性名称| 显示名称|说明|
| ---|---|---|
|id *|文件|选择一个文件|

星号 （*） 表示该属性是必需的。

##### <a name="output-details"></a>输出详细信息
无。


#### <a name="get-file-metadata-using-path"></a>获取使用路径的文件元数据
此操作获取文件元数据使用的路径。  

|属性名称| 显示名称|说明|
| ---|---|---|
|路径 *|文件路径|选择一个文件|

星号 （*） 表示该属性是必需的。

##### <a name="output-details"></a>输出详细信息
BlobMetadata

| 属性名称 | 数据类型 |
|---|---|
|标识|字符串|
|名称|字符串|
|显示名称|字符串|
|路径|字符串|
|上次更改时间|字符串|
|大小|整数|
|媒体类型|字符串|
|IsFolder|布尔值|
|ETag|字符串|
|FileLocator|字符串|


#### <a name="get-file-content-using-path"></a>获取使用路径的文件内容
此操作获取文件内容使用的路径。  

|属性名称| 显示名称|说明|
| ---|---|---|
|路径 *|文件路径|选择一个文件|

星号 （*） 表示该属性是必需的。

##### <a name="output-details"></a>输出详细信息
无。


#### <a name="get-file-content"></a>获取文件的内容
此操作获取使用 id 的文件内容。  

|属性名称| 数据类型|说明|
| ---|---|---|
|id *|字符串|选择一个文件|

星号 （*） 表示该属性是必需的。

##### <a name="output-details"></a>输出详细信息
无。


#### <a name="create-file"></a>创建文件
此操作将文件上载。  

|属性名称| 显示名称|说明|
| ---|---|---|
|采用文件夹路径 *|文件夹路径|请选择一个文件夹|
|名称 *|文件名称|若要上载的文件的名称|
|正文 *|文件内容|要上载的文件的内容|

星号 （*） 表示该属性是必需的。

##### <a name="output-details"></a>输出详细信息
BlobMetadata

| 属性名称 | 数据类型 | 
|---|---|
|标识|字符串|
|名称|字符串|
|显示名称|字符串|
|路径|字符串|
|上次更改时间|字符串|
|大小|整数|
|媒体类型|字符串|
|IsFolder|布尔值|
|ETag|字符串|
|FileLocator|字符串|


#### <a name="copy-file"></a>复制文件
此操作将文件复制到 Azure Blob 存储。  

|属性名称| 显示名称|说明|
| ---|---|---|
|来源 *|源的 url|源文件中指定的 Url|
|目标 *|目标文件路径|指定包含目标文件名目标文件路径|
|重写|是否覆盖？|应现有的目标文件覆盖 （真/假）？  |

星号 （*） 表示该属性是必需的。

##### <a name="output-details"></a>输出详细信息
BlobMetadata

| 属性名称 | 数据类型 |
|---|---|
|标识|字符串|
|名称|字符串|
|显示名称|字符串|
|路径|字符串|
|上次更改时间|字符串|
|大小|整数|
|媒体类型|字符串|
|IsFolder|布尔值|
|ETag|字符串|
|FileLocator|字符串|

#### <a name="extract-archive-to-folder"></a>提取存档文件夹
此操作提取到一个文件夹的存档文件 (示例︰.zip)。  

|属性名称| 显示名称|说明|
| ---|---|---|
|来源 *|源存档文件路径|选择某个存档文件|
|目标 *|目标文件夹路径|选择要提取的内容|
|重写|是否覆盖？|应现有的目标文件覆盖 （真/假）？|

星号 （*） 表示该属性是必需的。

##### <a name="output-details"></a>输出详细信息
BlobMetadata

| 属性名称 | 数据类型 |
|---|---|
|标识|字符串|
|名称|字符串|
|显示名称|字符串|
|路径|字符串|
|上次更改时间|字符串|
|大小|整数|
|媒体类型|字符串|
|IsFolder|布尔值|
|ETag|字符串|
|FileLocator|字符串|


## <a name="http-responses"></a>HTTP 响应

当调用不同的操作，您可能会收到的特定响应。 下表概述了响应和它们的说明︰  

|名称|说明|
|---|---|
|200|还行|
|202|接受|
|400|错误的请求|
|401|未经授权|
|403|禁止访问|
|404|找不到|
|500|内部服务器错误。 出现未知的错误|
|默认|操作失败。|

## <a name="next-steps"></a>下一步行动

[创建一个逻辑应用程序](../app-service-logic/app-service-logic-create-a-logic-app.md)。 浏览其他可用的连接器，在我们的[Api 列表](apis-list.md)的逻辑应用程序中。



