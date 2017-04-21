<properties 
   pageTitle="查看诊断日志 Azure 数据湖商店 |Microsoft Azure" 
   description="了解如何设置和访问 Azure 数据湖商店诊断日志 " 
   services="data-lake-store" 
   documentationCenter="" 
   authors="nitinme" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-store"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="10/05/2016"
   ms.author="nitinme"/>

# <a name="accessing-diagnostic-logs-for-azure-data-lake-store"></a>Azure 数据湖商店访问诊断日志

了解有关如何启用诊断日志记录数据湖存储帐户以及如何查看日志收集有关您的帐户。

组织可以启用诊断日志记录的 Azure 数据湖存储帐户收集数据访问审核跟踪信息，提供信息，如列表中的用户访问数据，数据的访问频率，多少数据存入帐户，等等。

## <a name="prerequisites"></a>系统必备组件

- **Azure 订阅**。 请参阅[获取 Azure 免费试用版](https://azure.microsoft.com/pricing/free-trial/)。

- **Azure 数据湖存储帐户**。 按照在[学习如何使用 Azure 数据湖存储区使用 Azure 门户](data-lake-store-get-started-portal.md)的说明。

## <a name="enable-diagnostic-logging-for-your-data-lake-store-account"></a>启用诊断日志记录的数据湖存储帐户

1. 登录到新[Azure 门户](https://portal.azure.com)。

2. 打开您的数据湖存储帐户，并从您的数据湖存储帐户刀片，单击**设置**，然后单击**诊断设置**。

3. 在**诊断**刀片式服务器，更改以下配置诊断日志记录。

    ![启用诊断日志记录](./media/data-lake-store-diagnostic-logs/enable-diagnostic-logs.png "启用诊断日志")

    * 将**状态**设置为**上**启用诊断日志记录。
    * 您可以选择两种不同的方式存储/处理数据。
        * 选择导出**到事件中心**流到 Azure 事件中心的日志数据的选项。 很可能将使用此选项，如果您有一种下游处理管道来分析在实时传入日志。 如果您选择此选项，必须提供您想要使用 Azure 事件中心的详细信息。
        * 选择**导出到存储帐户**存储到 Azure 存储帐户的日志选项。 如果您希望存档将批次处理以后的数据，请使用此选项。 如果您选择此选项，您必须提供要保存到日志的 Azure 存储帐户。
    * 指定您是否要获取审核日志和 / 或请求日志。
    * 指定的数据必须保留的天数。
    * 单击**保存**。

一旦启用诊断设置，您可以监视**诊断日志**选项卡中的日志。

## <a name="view-diagnostic-logs-for-your-data-lake-store-account"></a>您的数据湖存储帐户的查看诊断日志

有两种方法查看数据湖存储帐户的日志数据。

* 从数据湖存储帐户设置视图
* 从 Azure 存储帐户存储数据

### <a name="using-the-data-lake-store-settings-view"></a>使用数据湖商店设置视图

1. 从您数据湖存储帐户**设置**刀片式服务器，请单击**诊断日志**。

    ![查看诊断日志记录](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs.png "查看诊断日志") 

2. 在**诊断日志**刀片式服务器，您应该看到分类的**审核日志**和**请求日志**的日志。
    * 请求日志捕获数据湖存储帐户上每个 API 请求。
    * 审核日志是类似请求日志，但提供更详细的分类的数据湖存储帐户上所执行操作。 例如，请求日志中的单个上载 API 调用可能会导致审计日志中的多个"追加"操作。

3. 单击**下载**链接，根据每个日志项下载日志。

### <a name="from-the-azure-storage-account-that-contains-log-data"></a>从 Azure 存储帐户包含日志数据

1. 打开与数据湖存储相关联的日志记录，Azure 存储帐户刀片式服务器，然后单击 Blob。 **Blob 服务**刀片式服务器列出两个容器。

    ![查看诊断日志记录](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account.png "查看诊断日志")

    * 容器**的见解日志审核**包含审核日志。
    * 容器**的见解日志请求**包含请求日志。

2. 在这些容器中，日志都存储在以下结构下。

    ![查看诊断日志记录](./media/data-lake-store-diagnostic-logs/view-diagnostic-logs-storage-account-structure.png "查看诊断日志")

    举一个例子，可能是审核日志的完整路径`https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=04/m=00/PT1H.json`

    Similary，可能会请求日志的完整路径`https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/mydatalakestore/y=2016/m=07/d=18/h=14/m=00/PT1H.json`

## <a name="understand-the-structure-of-the-log-data"></a>了解日志数据的结构

审核并请求日志是以 JSON 格式。 在本节中，我们看一看 JSON 请求的结构，审核日志。

### <a name="request-logs"></a>请求日志

下面是一个示例条目 JSON 格式请求日志中。 每个 blob 具有一个根对象，称为**记录**包含日志对象的数组。

    {
    "records": 
      [     
        . . . .
        ,
        {
             "time": "2016-07-07T21:02:53.456Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
             "category": "Requests",
             "operationName": "GETCustomerIngressEgress",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {"HttpMethod":"GET","Path":"/webhdfs/v1/Samples/Outputs/Drivers.csv","RequestContentLength":0,"ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8","StartTime":"2016-07-07T21:02:52.472Z","EndTime":"2016-07-07T21:02:53.456Z"}
        }
        ,
        . . . .
      ]
    }

#### <a name="request-log-schema"></a>请求日志架构

| 名称            | 类型   | 说明                                                                    |
|-----------------|--------|--------------------------------------------------------------------------------|
| 时间            | 字符串 | （UTC) 中的日志的时间戳                                              |
| 资源 Id      | 字符串 | 在上放置操作所花费的资源 ID                            |
| 类别        | 字符串 | 日志类别。 例如，**请求**。                                   |
| 操作名称   | 字符串 | 记录操作的名称。 例如，getfilestatus。              |
| resultType      | 字符串 | 操作，例如，200 的状态。                                 |
| callerIpAddress | 字符串 | 发出请求的客户端的 IP 地址                                |
| 都会   | 字符串 | 可以在日志 id 用于组合在一起的一组相关的日志项 |
| 标识        | 对象 | 标识生成日志                                            |
| 属性      | JSON   | 有关详细信息，请参阅下面                                                          |

#### <a name="request-log-properties-schema"></a>请求日志属性架构

| 名称                 | 类型   | 说明                                               |
|----------------------|--------|-----------------------------------------------------------|
| HttpMethod           | 字符串 | 为操作使用的 HTTP 方法。 例如，得到。 |
| 路径                 | 字符串 | 执行路径操作                   |
| RequestContentLength | int    | HTTP 请求的内容长度                    |
| ClientRequestId      | 字符串 | 唯一地标识此请求的 Id              |
| 开始时间            | 字符串 | 服务器接收到请求的时间         |
| 结束时间              | 字符串 | 服务器发送的响应时间              |

### <a name="audit-logs"></a>审核日志

下面是示例条目以 JSON 格式的审核日志。 每个 blob 具有一个根对象，称为**记录**包含日志对象的数组

    {
    "records": 
      [     
        . . . .
        ,
        {
             "time": "2016-07-08T19:08:59.359Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKESTORE/ACCOUNTS/<data_lake_store_account_name>",
             "category": "Audit",
             "operationName": "SeOpenStream",
             "resultType": "0",
             "correlationId": "381110fc03534e1cb99ec52376ceebdf;Append_BrEKAmg;25.66.9.145",
             "identity": "A9DAFFAF-FFEE-4BB5-A4A0-1B6CBBF24355",
             "properties": {"StreamName":"adl://<data_lake_store_account_name>.azuredatalakestore.net/logs.csv"}
        }
        ,
        . . . .
      ]
    }

#### <a name="audit-log-schema"></a>审核日志架构

| 名称            | 类型   | 说明                                                                    |
|-----------------|--------|--------------------------------------------------------------------------------|
| 时间            | 字符串 | （UTC) 中的日志的时间戳                                              |
| 资源 Id      | 字符串 | 在上放置操作所花费的资源 ID                            |
| 类别        | 字符串 | 日志类别。 例如，**审核**。                                      |
| 操作名称   | 字符串 | 记录操作的名称。 例如，getfilestatus。              |
| resultType      | 字符串 | 操作，例如，200 的状态。                                 |
| 都会   | 字符串 | 可以在日志 id 用于组合在一起的一组相关的日志项 |
| 标识        | 对象 | 标识生成日志                                            |
| 属性      | JSON   | 有关详细信息，请参阅下面                                                          |

#### <a name="audit-log-properties-schema"></a>审核日志属性架构

| 名称       | 类型   | 说明                              |
|------------|--------|------------------------------------------|
| StreamName | 字符串 | 执行路径操作  |


## <a name="samples-to-process-the-log-data"></a>流程日志数据的示例

Azure 数据湖商店提供如何处理和分析的日志数据的一个示例。 您可以在[https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample)找到该示例。 


## <a name="see-also"></a>请参见

- [Azure 湖存储数据的概览](data-lake-store-overview.md)
- [保护数据湖存储区中的数据](data-lake-store-secure-data.md)

