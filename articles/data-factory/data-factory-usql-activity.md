<properties 
    pageTitle="从 Azure 数据工厂运行 Azure 数据湖分析 U SQL 脚本" 
    description="了解如何通过在 Azure 数据湖分析计算服务上运行 U SQL 脚本中处理数据。" 
    services="data-factory" 
    documentationCenter="" 
    authors="spelluru" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/12/2016" 
    ms.author="spelluru"/>

# <a name="run-u-sql-script-on-azure-data-lake-analytics-from-azure-data-factory"></a>从 Azure 数据工厂运行 Azure 数据湖分析 U SQL 脚本
> [AZURE.SELECTOR]
[配置单元](data-factory-hive-activity.md)  
[小猪](data-factory-pig-activity.md)  
[MapReduce](data-factory-map-reduce.md)  
[Hadoop 流](data-factory-hadoop-streaming-activity.md)
[机器学习](data-factory-azure-ml-batch-execution-activity.md) 
[存储过程](data-factory-stored-proc-activity.md)
[数据湖分析 U SQL](data-factory-usql-activity.md)
[.NET 自定义](data-factory-use-custom-activities.md)
 
在 Azure 数据工厂管道处理链接的存储服务中的数据通过使用链接的计算服务。 它包含一系列活动，其中每个活动执行特定处理操作。 本文介绍了对**Azure 数据湖分析**计算链接服务运行**U SQL**脚本的**数据湖分析 U SQL 活动**。 

> [AZURE.NOTE] 
> 创建一个数据湖分析 U SQL 活动与管道之前创建 Azure 数据湖分析帐户。 要了解有关 Azure 数据湖泊分析，请参阅[开始使用 Azure 数据湖分析](../data-lake-analytics/data-lake-analytics-get-started-portal.md)。
>  
> 查看[生成您第一个管道教程](data-factory-build-your-first-pipeline.md)详细步骤来创建数据工厂，链接的服务、 数据集和管线。 使用 JSON 段与数据工厂编辑器或 Visual Studio Azure PowerShell 创建数据工厂元。

## <a name="azure-data-lake-analytics-linked-service"></a>Azure 数据湖分析链接服务
创建链接到 Azure 数据工厂的 Azure 数据湖分析计算服务**Azure 湖分析数据**链接服务。 在管道中的数据湖分析 U SQL 活动指此链接的服务。 

下面的示例提供了 JSON 定义 Azure 数据湖分析链接服务。 

    {
        "name": "AzureDataLakeAnalyticsLinkedService",
        "properties": {
            "type": "AzureDataLakeAnalytics",
            "typeProperties": {
                "accountName": "adftestaccount",
                "dataLakeAnalyticsUri": "datalakeanalyticscompute.net",
                "authorization": "<authcode>",
                "sessionId": "<session ID>", 
                "subscriptionId": "<subscription id>",
                "resourceGroupName": "<resource group name>"
            }
        }
    }


下表提供了使用 JSON 定义中的属性的说明。 

属性 | 说明 | 必填
-------- | ----------- | --------
类型 | Type 属性应设置为︰ **AzureDataLakeAnalytics**。 | 是的
帐户名 | Azure 数据湖分析帐户名。 | 是的
dataLakeAnalyticsUri | Azure 数据湖分析 URI。 |  不 
授权 | 单击**授权**按钮在数据工厂编辑器并完成 OAuth 登录后自动检索授权码。  | 是的 
subscriptionId | Azure 的订阅 id | 否 （如果未指定，则使用订阅的数据工厂）。 
resourceGroupName | Azure 的资源组名称 |  否 （如果未指定，则使用数据工厂的资源组）。
会话标识符 | 从 OAuth 授权会话的会话 id。 每个会话 id 是唯一的并且只能使用一次。 会话 Id 是数据工厂编辑器中自动生成。 | 是的

一段时间后过期使用**授权**按钮生成的授权码。 不同类型的用户帐户的过期时间，请参阅下表。 您可能会看到以下错误消息时身份验证**令牌过期**︰ 凭据操作时出现错误︰ invalid_grant-AADSTS70002︰ 验证凭据时出错。 AADSTS70008︰ 提供访问权限授予是过期或被吊销。 跟踪 ID: d18629e8-af88-43c5-88e3-d8419eb1fca1 相关性 ID: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 时间戳︰ 2015年-12-15 21:09:31Z

 
| 用户类型 | 过期时间 |
| :-------- | :----------- | 
| 用户帐户不受 Azure Active Directory (@hotmail.com, @live.com,等。) | 12 个小时 |
| 管理通过 Azure 活动目录 (AAD) 的用户帐户 | 最后层切面的运行后的 14 天。 <br/><br/>90 天，如果一块基于基于 OAuth 的链接服务运行每 14 天至少一次。 |

若要避免/解决此错误，台上使用**授权**按钮时**令牌到期**并重新链接的服务部署。 您还可以生成**会话 Id**和**授权**使用下一节中的代码以编程方式的属性的值。 

  
### <a name="to-programmatically-generate-sessionid-and-authorization-values"></a>若要以编程方式生成会话 Id 和授权值 

    if (linkedService.Properties.TypeProperties is AzureDataLakeStoreLinkedService ||
        linkedService.Properties.TypeProperties is AzureDataLakeAnalyticsLinkedService)
    {
        AuthorizationSessionGetResponse authorizationSession = this.Client.OAuth.Get(this.ResourceGroupName, this.DataFactoryName, linkedService.Properties.Type);

        WindowsFormsWebAuthenticationDialog authenticationDialog = new WindowsFormsWebAuthenticationDialog(null);
        string authorization = authenticationDialog.AuthenticateAAD(authorizationSession.AuthorizationSession.Endpoint, new Uri("urn:ietf:wg:oauth:2.0:oob"));

        AzureDataLakeStoreLinkedService azureDataLakeStoreProperties = linkedService.Properties.TypeProperties as AzureDataLakeStoreLinkedService;
        if (azureDataLakeStoreProperties != null)
        {
            azureDataLakeStoreProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
            azureDataLakeStoreProperties.Authorization = authorization;
        }

        AzureDataLakeAnalyticsLinkedService azureDataLakeAnalyticsProperties = linkedService.Properties.TypeProperties as AzureDataLakeAnalyticsLinkedService;
        if (azureDataLakeAnalyticsProperties != null)
        {
            azureDataLakeAnalyticsProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
            azureDataLakeAnalyticsProperties.Authorization = authorization;
        }
    }

[AzureDataLakeStoreLinkedService 类](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx)、 [AzureDataLakeAnalyticsLinkedService 类](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx)和[AuthorizationSessionGetResponse 类](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx)主题有关在代码中使用的数据工厂类的详细信息，请参阅。 添加引用︰ WindowsFormsWebAuthenticationDialog 类的 Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll。 
 
 
## <a name="data-lake-analytics-u-sql-activity"></a>数据分析湖 U SQL 活动 

下面的 JSON 段定义与湖泊分析 U SQL 活动数据管道。 活动定义中包含您在前面创建的链接 Azure 数据湖分析服务的参考。   
  

    {
        "name": "ComputeEventsByRegionPipeline",
        "properties": {
            "description": "This is a pipeline to compute events for en-gb locale and date less than 2012/02/19.",
            "activities": 
            [
                {
                    "type": "DataLakeAnalyticsU-SQL",
                    "typeProperties": {
                        "scriptPath": "scripts\\kona\\SearchLogProcessing.txt",
                        "scriptLinkedService": "StorageLinkedService",
                        "degreeOfParallelism": 3,
                        "priority": 100,
                        "parameters": {
                            "in": "/datalake/input/SearchLog.tsv",
                            "out": "/datalake/output/Result.tsv"
                        }
                    },
                    "inputs": [
                        {
                            "name": "DataLakeTable"
                        }
                    ],
                    "outputs": 
                    [
                        {
                            "name": "EventsByRegionTable"
                        }
                    ],
                    "policy": {
                        "timeout": "06:00:00",
                        "concurrency": 1,
                        "executionPriorityOrder": "NewestFirst",
                        "retry": 1
                    },
                    "scheduler": {
                        "frequency": "Day",
                        "interval": 1
                    },
                    "name": "EventsByRegion",
                    "linkedServiceName": "AzureDataLakeAnalyticsLinkedService"
                }
            ],
            "start": "2015-08-08T00:00:00Z",
            "end": "2015-08-08T01:00:00Z",
            "isPaused": false
        }
    }


下表描述了特定于此活动的属性的名称和描述。 

属性 | 说明 | 必填
:-------- | :----------- | :--------
类型 | 类型属性必须设置为**DataLakeAnalyticsU SQL**。 | 是的
scriptPath | 包含 U SQL 脚本的文件夹的路径。 文件的名称是区分大小写。 | 否 （如果您使用脚本）
scriptLinkedService | 链接存储包含数据工厂到脚本的链接的服务 | 否 （如果您使用脚本）
脚本 | 指定内联脚本，而不是指定 scriptPath 和 scriptLinkedService。 例如:"脚本":"创建数据库测试"。 | 否 （如果您使用 scriptPath 和 scriptLinkedService）
degreeOfParallelism | 同时用来运行该作业的节点最大数量。 | 不
优先级 | 确定所有排队的作业应选择首先运行。 越低，优先级越高。 | 不 
参数 | U SQL 脚本的参数 | 不 

[SearchLogProcessing.txt 脚本定义](#script-definition)的脚本定义，请参见 

## <a name="sample-input-and-output-datasets"></a>示例输入和输出数据集

### <a name="input-dataset"></a>输入数据集
在此示例中，输入的数据驻留在 Azure 数据湖存储区 （datalake/输入文件夹中的 SearchLog.tsv 文件）。 

    {
        "name": "DataLakeTable",
        "properties": {
            "type": "AzureDataLakeStore",
            "linkedServiceName": "AzureDataLakeStoreLinkedService",
            "typeProperties": {
                "folderPath": "datalake/input/",
                "fileName": "SearchLog.tsv",
                "format": {
                    "type": "TextFormat",
                    "rowDelimiter": "\n",
                    "columnDelimiter": "\t"
                }
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }   

### <a name="output-dataset"></a>输出数据集
在此示例中，U SQL 脚本生成的输出数据存储在 Azure 数据湖存储区 （datalake/输出文件夹）。 

    {
        "name": "EventsByRegionTable",
        "properties": {
            "type": "AzureDataLakeStore",
            "linkedServiceName": "AzureDataLakeStoreLinkedService",
            "typeProperties": {
                "folderPath": "datalake/output/"
            },
            "availability": {
                "frequency": "Day",
                "interval": 1
            }
        }
    }

### <a name="sample-data-lake-store-linked-service"></a>示例数据湖存储链接服务
下面是的示例 Azure 数据湖存储链接服务的输入/输出数据集所使用的定义。 

    {
        "name": "AzureDataLakeStoreLinkedService",
        "properties": {
            "type": "AzureDataLakeStore",
            "typeProperties": {
                "dataLakeUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
                "sessionId": "<session ID>",
                "authorization": "<authorization URL>"
            }
        }
    }

请参阅[移动数据传入和传出 Azure 数据湖存储](data-factory-azure-datalake-connector.md)文章有关 JSON 属性的说明。 

## <a name="sample-u-sql-script"></a>U SQL 脚本示例 

    @searchlog =
        EXTRACT UserId          int,
                Start           DateTime,
                Region          string,
                Query           string,
                Duration        int?,
                Urls            string,
                ClickedUrls     string
        FROM @in
        USING Extractors.Tsv(nullEscape:"#NULL#");
    
    @rs1 =
        SELECT Start, Region, Duration
        FROM @searchlog
    WHERE Region == "en-gb";
    
    @rs1 =
        SELECT Start, Region, Duration
        FROM @rs1
        WHERE Start <= DateTime.Parse("2012/02/19");
    
    OUTPUT @rs1   
        TO @out
          USING Outputters.Tsv(quoting:false, dateTimeFormat:null);

值**@in**和**@out**U SQL 脚本中传递参数动态 ADF 通过使用参数部分。 请参阅管道定义中的参数部分。

可以在 Azure 数据湖分析服务运行的作业的管道定义以及指定其他属性，如 degreeOfParallelism 和优先级。

## <a name="dynamic-parameters"></a>动态参数
在样本管道定义，in 和 out 参数分配使用硬编码的值。 

    "parameters": {
        "in": "/datalake/input/SearchLog.tsv",
        "out": "/datalake/output/Result.tsv"
    }

就可能改为使用动态参数。 例如︰ 

    "parameters": {
        "in": "$$Text.Format('/datalake/input/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)",
        "out": "$$Text.Format('/datalake/output/{0:yyyy-MM-dd HH:mm:ss}.tsv', SliceStart)"
    }

在这种情况下，输入的文件仍然领取从 /datalake/input 文件夹，并在 /datalake/output 文件夹中生成输出文件。 文件名称是动态的基于扇区的开始时间。  