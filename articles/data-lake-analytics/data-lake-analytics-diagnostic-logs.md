<properties 
   pageTitle="查看诊断日志 Azure 数据湖分析 |Microsoft Azure" 
   description="了解如何设置和访问的 Azure 数据湖分析诊断日志 " 
   services="data-lake-analytics" 
   documentationCenter="" 
   authors="Blackmist" 
   manager="jhubbard" 
   editor="cgronlun"/>
 
<tags
   ms.service="data-lake-analytics"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data" 
   ms.date="08/11/2016"
   ms.author="larryfr"/>

# <a name="accessing-diagnostic-logs-for-azure-data-lake-analytics"></a>访问 Azure 数据湖分析诊断日志

了解有关如何启用诊断日志记录数据湖分析帐户以及如何查看日志收集有关您的帐户。

组织可以启用诊断日志记录其 Azure 数据湖分析帐户，以收集数据访问审核跟踪信息。 这些日志提供以下信息︰

* 访问数据的用户的列表。
* 数据的访问频率。
* 帐户中存储的数据量。

## <a name="prerequisites"></a>系统必备组件

- **Azure 订阅**。 请参阅[获取 Azure 免费试用版](https://azure.microsoft.com/pricing/free-trial/)。
- **启用 Azure 的订阅**数据湖分析公共预览。 请参阅[说明](data-lake-analytics-get-started-portal.md#signup)。
- **Azure 数据湖分析帐户**。 按照在[开始使用 Azure 数据湖分析使用 Azure 门户](data-lake-analytics-get-started-portal.md)的说明。

## <a name="enable-logging"></a>启用日志记录

1. 登录到新[Azure 的门户](https://portal.azure.com)。

2. 打开您的数据湖分析帐户，并从您的数据湖分析帐户刀片式服务器，单击**设置**，然后单击**诊断设置**。

3. 在**诊断**刀片式服务器，更改以下配置诊断日志记录。

    ![启用诊断日志记录](./media/data-lake-analytics-diagnostic-logs/enable-diagnostic-logs.png "启用诊断日志")

    * 将**状态**设置为**上**启用诊断日志记录。
    * 您可以选择两种不同的方式存储/处理数据。
        * 选择**导出到事件中心**流到 Azure 事件中心的日志数据。 如果您有一种下游处理管道来分析实时传入日志，请使用此选项。 如果您选择此选项，必须提供您想要使用 Azure 事件中心的详细信息。
        * 选择**导出到存储帐户**存储到 Azure 存储帐户的日志。 如果要存档数据，请使用此选项。 如果您选择此选项，您必须提供要保存到日志的 Azure 存储帐户。
    * 指定您是否要获取审核日志和 / 或请求日志。
    * 指定的数据必须保留的天数。
    * 单击**保存**。

一旦启用诊断设置，您可以监视**诊断日志**选项卡中的日志。

## <a name="view-logs"></a>查看日志

有两种方法查看数据湖分析帐户的日志数据。

* 从数据湖分析帐户设置
* 从 Azure 存储帐户存储数据

### <a name="using-the-data-lake-analytics-settings-view"></a>使用数据湖分析设置视图

1. 从您的数据湖分析帐户**设置**刀片式服务器，请单击**诊断日志**。

    ![查看诊断日志记录](./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs.png "查看诊断日志") 

2. 在**诊断日志**刀片式服务器，您应该看到分类的**审核日志**和**请求日志**的日志。
    * 请求日志捕获数据湖分析帐户上每个 API 请求。
    * 审核日志是类似请求日志，但提供更详细的分类的数据湖分析帐户上所执行操作。 例如，请求日志中的单个上载 API 调用可能会导致审计日志中的多个"追加"操作。

3. 请单击**下载**链接的下载日志的日志条目。

### <a name="from-the-azure-storage-account-that-contains-log-data"></a>从 Azure 存储帐户包含日志数据

1. 打开与数据湖分析相关联的日志记录，Azure 存储帐户刀片式服务器，然后单击 Blob。 **Blob 服务**刀片式服务器列出两个容器。

    ![查看诊断日志记录](./media/data-lake-analytics-diagnostic-logs/view-diagnostic-logs-storage-account.png "查看诊断日志")

    * 容器**的见解日志审核**包含审核日志。
    * 容器**的见解日志请求**包含请求日志。

2. 在这些容器中，日志都存储在以下结构下。

        resourceId=/
          SUBSCRIPTIONS/
            <<SUBSCRIPTION_ID>>/
              RESOURCEGROUPS/
                <<RESOURCE_GRP_NAME>>/
                  PROVIDERS/
                    MICROSOFT.DATALAKEANALYTICS/
                      ACCOUNTS/
                        <DATA_LAKE_ANALYTICS_NAME>>/
                          y=####/
                            m=##/
                              d=##/
                                h=##/
                                  m=00/
                                    PT1H.json
    
    > [AZURE.NOTE] `##`年、 月、 日和小时在其中创建日志包含路径中的条目。 数据湖分析创建一个文件每隔一小时，因此`m=`始终包含值为`00`。

    举一个例子，可能是审核日志的完整路径︰
    
        https://adllogs.blob.core.windows.net/insights-logs-audit/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=04/m=00/PT1H.json

    类似地，可能是到请求日志的完整路径︰
    
        https://adllogs.blob.core.windows.net/insights-logs-requests/resourceId=/SUBSCRIPTIONS/<sub-id>/RESOURCEGROUPS/myresourcegroup/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/mydatalakeanalytics/y=2016/m=07/d=18/h=14/m=00/PT1H.json

## <a name="log-structure"></a>日志结构

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
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_analytics_account_name>",
             "category": "Requests",
             "operationName": "GetAggregatedJobHistory",
             "resultType": "200",
             "callerIpAddress": "::ffff:1.1.1.1",
             "correlationId": "4a11c709-05f5-417c-a98d-6e81b3e29c58",
             "identity": "1808bd5f-62af-45f4-89d8-03c5e81bac30",
             "properties": {
                 "HttpMethod":"POST",
                 "Path":"/JobAggregatedHistory",
                 "RequestContentLength":122,
                 "ClientRequestId":"3b7adbd9-3519-4f28-a61c-bd89506163b8",
                 "StartTime":"2016-07-07T21:02:52.472Z",
                 "EndTime":"2016-07-07T21:02:53.456Z"
                 }
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
| 操作名称   | 字符串 | 记录操作的名称。 例如，GetAggregatedJobHistory。              |
| resultType      | 字符串 | 操作，例如，200 的状态。                                 |
| callerIpAddress | 字符串 | 发出请求的客户端的 IP 地址                                |
| 都会   | 字符串 | 日志的 id。 此值可用于将一组相关的日志条目 |
| 标识        | 对象 | 标识生成日志                                            |
| 属性      | JSON   | 下一节 （请求日志属性架构） 的详细信息，请参阅 |

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
             "time": "2016-07-28T19:15:16.245Z",
             "resourceId": "/SUBSCRIPTIONS/<subscription_id>/RESOURCEGROUPS/<resource_group_name>/PROVIDERS/MICROSOFT.DATALAKEANALYTICS/ACCOUNTS/<data_lake_ANALYTICS_account_name>",
             "category": "Audit",
             "operationName": "JobSubmitted",
             "identity": "user@somewhere.com",
             "properties": {
                 "JobId":"D74B928F-5194-4E6C-971F-C27026C290E6",
                 "JobName": "New Job", 
                 "JobRuntimeName": "default",
                 "SubmitTime": "7/28/2016 7:14:57 PM"
                 }
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
| 操作名称   | 字符串 | 记录操作的名称。 例如，JobSubmitted。              |
| resultType | 字符串 | （操作） 的作业状态的子状态。 |
| resultSignature | 字符串 | 作业状态 （操作） 的其他详细信息。 |
| 标识      | 字符串 | 请求操作的用户。 例如， susan@contoso.com。                                 |
| 属性      | JSON   | 下一节 （审核日志属性架构） 的详细信息，请参阅 |

> [AZURE.NOTE] __resultType__和__resultSignature__操作的结果中提供的信息只包含一个值，如果某项操作已完成。 例如，当__操作名称__中包含__JobStarted__或__JobEnded__的值包含一个值。

#### <a name="audit-log-properties-schema"></a>审核日志属性架构

| 名称       | 类型   | 说明                              |
|------------|--------|------------------------------------------|
| 作业 Id | 字符串 | 分配给此作业的 ID  |
| JobName | 字符串 | 已提供的作业的名称 |
| JobRunTime | 字符串 | 运行时用于处理该作业 |
| SubmitTime | 字符串 | 提交作业的时间 （以 UTC) |
| 开始时间 | 字符串 | 作业开始时间 （UTC) 中提交后运行。 |
| 结束时间 | 字符串 | 作业的结束时间。 |
| 并行度 | 字符串 | 在提交过程中为此作业请求的数据湖分析单位数。 |

> [AZURE.NOTE] __SubmitTime__、__开始时间__、__结束时间__和__并行__运算，在提供信息和只包含一个值，如果操作是已启动还是已完成。 例如， __SubmitTime__包含一个值后__操作名称__指示__JobSubmitted__。

## <a name="process-the-log-data"></a>流程日志数据

Azure 数据湖分析提供如何处理和分析的日志数据的一个示例。 您可以在[https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample](https://github.com/Azure/AzureDataLake/tree/master/Samples/AzureDiagnosticsSample)找到该示例。 


## <a name="next-steps"></a>下一步行动

- [Azure 数据湖分析的概述](data-lake-analytics-overview.md)


