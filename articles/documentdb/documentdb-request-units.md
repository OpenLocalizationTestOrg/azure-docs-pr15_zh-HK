<properties 
    pageTitle="申请单位 DocumentDB |Microsoft Azure" 
    description="了解如何来理解、 指定和估计在 DocumentDB 请求单位需求。" 
    services="documentdb" 
    authors="syamkmsft" 
    manager="jhubbard" 
    editor="mimig" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/29/2016" 
    ms.author="syamk"/>

#<a name="request-units-in-documentdb"></a>申请单位 DocumentDB
现可提供︰ DocumentDB[请求单位计算器](https://www.documentdb.com/capacityplanner)。 了解更多在[吞吐量需求的估算](documentdb-request-units.md#estimating-throughput-needs)。

![吞吐量计算器][5]

##<a name="introduction"></a>介绍
这篇文章概括介绍了[Microsoft Azure DocumentDB](https://azure.microsoft.com/services/documentdb/)的请求量。 

阅读这篇文章之后, 您将能够回答以下问题︰  

-   什么是申请单位，申请费用？
-   如何指定集合的请求单元容量？
-   如何估计我的应用程序请求单位需要多少？
-   如果超过了请求集合的单位容量会怎么样？


##<a name="request-units-and-request-charges"></a>申请单位，并申请费用
DocumentDB 通过*预留*资源，以满足您的应用程序吞吐量需求提供快速、 可预知的性能。  因为应用程序加载和访问模式变化随着时间的推移，DocumentDB 可以很容易地增加或减少保留的应用程序吞吐量的量。

与 DocumentDB，用每秒处理的请求单位指定保留的吞吐量。  可以请求单位看作吞吐量货币，因此，*保留*量的保证请求单位可用于应用程序每秒的基础上。  DocumentDB 编写文档，执行查询、 更新文档中的每个操作所消耗的 CPU、 内存和 IOPS。  即，每个操作会导致*申请费用*，其中*请求*为单位。  理解的因素，从而影响请求设备费用，以及您的应用程序吞吐量要求，可以作为成本尽可能有效地运行您的应用程序。 

##<a name="specifying-request-unit-capacity"></a>指定请求单位容量
当创建 DocumentDB 集合，您指定请求每秒单位数 (RUs) 需保留集合的数。  创建集合后，集合的使用保留完整的 RUs 指定分配。  保证每个集合有专门而独立的吞吐量特性。  

务必要注意 DocumentDB 操作保留模型;也就是说，付费吞吐量*保留*集合，不论多少，吞吐量是积极*使用*的金额。  但是请记住，，即为您的应用程序加载、 数据和使用模式更改，您可以轻松调整向上和向下的量保留通过 DocumentDB Sdk 或使用[Azure 门户](https://portal.azure.com)的 RUs。  向上或向下扩展吞吐量到的详细信息，请参阅[DocumentDB 的性能级别](documentdb-performance-levels.md)。

##<a name="request-unit-considerations"></a>申请单位注意事项
在估计为 DocumentDB 集合保留请求单位数，务必要考虑以下变量︰

- **文档的大小**。 随着文档大小的增加单位消耗来读取或写入的数据也会增加。
- **文档属性数**。 假定默认索引的所有属性，使用编写文档的单位将作为属性计数的增加而增加。
- **数据的一致性**。 当使用强或包围的失效数据一致性级别，需要消耗额外单位读取文档。
- **索引属性**。 在每个集合的索引策略确定哪些属性进行索引，默认情况。 通过限制索引的属性的数目或通过启用迟缓索引，您可以减少您请求单位消耗。
- **文档索引**。 默认情况下，每个文档将自动设置索引，将消耗少的申请单位，如果您选择不进行索引的文档部分。
- **查询模式**。 查询的复杂性会影响多少请求单位所使用的操作。 谓词的数目、 性质的谓词、 投影、 数量的 Udf 和所有源数据集的大小会影响查询操作的成本。
- **脚本使用**。  与查询、 存储的过程和触发器使用请求单位根据正在执行的操作的复杂性。 在开发应用程序时，检查请求的费用标头，以更好地了解每个操作消耗请求单位容量的方式。

##<a name="estimating-throughput-needs"></a>估计的吞吐量需求
申请单位是标准化的请求处理成本的措施。 单个请求单位代表包含 10 个 （不包括系统属性） 的独特属性值的单个 1 KB JSON 文档读取 （通过自助链接或 id） 所需的处理能力。 （插入） 创建、 替换或删除同一文档的请求将消耗更多处理服务，从而更多的申请单位。   

> [AZURE.NOTE] 基线的 1 请求单元为 1 KB 文档对应于简单的 GET 自助链接或文档的 id。

###<a name="use-the-request-unit-calculator"></a>使用请求单位计算器
为了帮助客户正常微调其吞吐量的估计，没有一个基于 web[请求单位计算器](https://www.documentdb.com/capacityplanner)来帮助估计典型操作，包括申请单位要求︰

- 文档创建 （写入）
- 读取文档
- 文档删除
- 文档更新

该工具还包括用于评估基于您提供的示例文档的数据存储需要的支持。

使用此工具很简单︰

1. 上传一个或多个具有代表性的 JSON 文档。

    ![将文档上载到该请求设备计算器][2]

2. 若要估计数据存储要求，请输入您希望存储的文档的总数。

3. 输入文档的创建、 读取、 更新和删除的操作需要 （在以每秒为单位）。 为了估计文档更新操作请求单位费用，请上传从上述第一步，其中包括典型字段更新示例文档的一个副本。  例如，如果文档更新通常可以修改名为 lastLogin 和 userVisits 的两个属性，然后只需复制示例文档、 更新这两个属性的值和上载复印的文档。

    ![在请求单位计算器中输入吞吐量要求][3]

4. 单击计算并检查结果。

    ![申请单位计算器结果][4]

>[AZURE.NOTE]如果必须将根据大小和索引的属性的数目显著不同的文档类型，然后将样本的每种*类型*的典型文档上载到该工具，然后计算结果。

###<a name="use-the-documentdb-request-charge-response-header"></a>使用 DocumentDB 请求费用响应标头
每个响应 DocumentDB 服务中的包括自定义页眉 （x-ms-请求的费用），其中包含请求消耗的申请单位。 此标头也是可以通过 DocumentDB Sdk 访问的。 在.NET SDK 中，RequestCharge 是 ResourceResponse 对象的一个属性。  对于查询，在 Azure 的门户网站 DocumentDB 查询资源管理器提供了执行的查询请求费用信息。

![检查查询浏览器中的 RU 费用][1]

这一点，估计量保留您的应用程序所需的吞吐量记录请求单位费用与对代表文档使用的应用程序运行的典型操作，然后估计的操作数的一种方法准备执行每秒。  一定要测量，包括典型的查询和以及 DocumentDB 脚本用法。

>[AZURE.NOTE]如果必须将根据大小和索引的属性的数目显著不同的文档类型，然后记录适用的操作请求单位费用与典型文档的每个*类型*关联。

例如︰

1. 记录请求单位费用创建 （插入） 的典型文档。 
2. 记录请求单位费用的阅读典型文档。
3. 记录更新的典型文档请求单位费用。
3. 典型的、 公共的文档查询请求单位费用的记录。
4. 记录请求单位费用 （存储的过程、 触发器、 用户定义的函数） 的任何自定义脚本的应用程序利用
5. 计算所需的请求单位给出的预计运行每秒的操作的估计的数。

##<a name="a-request-unit-estimation-example"></a>申请单位估计示例
请考虑下面的 ~ 1 KB 文档︰

    {
     "id": "08259",
    "description": "Cereals ready-to-eat, KELLOGG, KELLOGG'S CRISPIX",
    "tags": [
        {
        "name": "cereals ready-to-eat"
        },
        {
        "name": "kellogg"
        },
        {
        "name": "kellogg's crispix"
        }
    ],
    "version": 1,
    "commonName": "Includes USDA Commodity B855",
    "manufacturerName": "Kellogg, Co.",
    "isFromSurvey": false,
    "foodGroup": "Breakfast Cereals",
    "nutrients": [
        {
        "id": "262",
        "description": "Caffeine",
        "nutritionValue": 0,
        "units": "mg"
        },
        {
        "id": "307",
        "description": "Sodium, Na",
        "nutritionValue": 611,
        "units": "mg"
        },
        {
        "id": "309",
        "description": "Zinc, Zn",
        "nutritionValue": 5.2,
        "units": "mg"
        }
    ],
    "servings": [
        {
        "amount": 1,
        "description": "cup (1 NLEA serving)",
        "weightInGrams": 29
        }
    ]
    }

>[AZURE.NOTE]文档是 DocumentDB，在缩减，因此系统的计算上述文档的大小是略小于 1 KB。


下表显示了大致的请求 （近似请求单位费用假定该帐户的一致性级别设置为"会话"和所有的文档会自动建立索引） 对本文档的典型操作的单位费用︰

操作|申请单位费用 
---|---
创建文档|~ 15 RU 
阅读文档|~ 1 RU
按 id 查询文档|~2.5 RU

此外下, 表显示近似的请求的应用程序中使用的典型查询的单位费用︰

查询|申请单位费用|# 返回文档的
---|---|--- 
通过 id 来选择食物|~2.5 RU|1 
选择食品制造商|~ 7 RU|7
选择的食物组和顺序按重量|~ 70 RU|100
选择食物组中的前 10 种食物|~ 10 RU|10

>[AZURE.NOTE]RU 费用取决于返回的文档数。

使用此信息，我们可以估计 RU 此应用程序要求的给定的操作，我们预计每秒查询数︰

操作/查询|估计的数 / 秒|所需的 RUs 
---|---|--- 
创建文档|10|150 
阅读文档|100|100 
选择食品制造商|25|175 
通过食品组选择|10|700 
选择 top 10|15|150 合计|155|1275

在这种情况下，我们期望每个 s 1,275 RU 的平均吞吐量要求。  舍入到接近 100，我们将提供 1300 RU/s 为此应用程序的集合。

##<a id="RequestRateTooLarge"></a>超出保留的吞吐量限制
请记住，请求单位消耗量计算为每秒的速率。 对于应用程序超出集合资源调配的请求单元速率，将限制到该集合的请求，直到率低于保留级别。 当限制出现时，服务器将预先结束 RequestRateTooLargeException （HTTP 状态代码 429） 的请求，并返回指示的时间，以毫秒为单位，在重试该请求之前，用户必须等待 x-ms-重试-之后-ms 头。

    HTTP Status 429
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100

如果您使用.NET 客户端 SDK 和 LINQ 查询，然后大部分时间您永远不需要处理此异常，因为当前版本的.NET 客户端 SDK 隐式捕获此响应，考虑服务器指定重试之后头，和重试请求。 正在多个客户端同时访问您的帐户，除非下次重试将会成功。

如果您有多个客户端累积以上请求速率默认重试行为可能无法满足需要，并给应用程序，客户端将引发，状态码为 429 DocumentClientException。 在这种情况下，您可能考虑处理重试的行为和在应用程序的错误处理例程或增加了保留的吞吐量为集合中的逻辑。

##<a name="next-steps"></a>下一步行动

若要了解有关保留吞吐量与 Azure DocumentDB 数据库的详细信息，请浏览这些资源︰
 
- [DocumentDB 的定价](https://azure.microsoft.com/pricing/details/documentdb/)
- [管理 DocumentDB 容量](documentdb-manage.md) 
- [在 DocumentDB 中的数据建模](documentdb-modeling-data.md)
- [DocumentDB 的性能级别](documentdb-partition-data.md)

若要了解有关 DocumentDB 的详细信息，请参阅 Azure DocumentDB[文档](https://azure.microsoft.com/documentation/services/documentdb/)。 

规模和性能测试与 DocumentDB 有关入门，请参阅[性能和规模测试使用 Azure DocumentDB](documentdb-performance-testing.md)。


[1]: ./media/documentdb-request-units/queryexplorer.png 
[2]: ./media/documentdb-request-units/RUEstimatorUpload.png
[3]: ./media/documentdb-request-units/RUEstimatorDocuments.png
[4]: ./media/documentdb-request-units/RUEstimatorResults.png
[5]: ./media/documentdb-request-units/RUCalculator2.png
