<properties
    pageTitle="租户资源使用 API |Microsoft Azure"
    description="资源使用 API，参考其检索 Azure 堆栈用法信息。"
    services="azure-stack"
    documentationCenter=""
    authors="AlfredoPizzirani"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="10/18/2016"
    ms.author="alfredop"/>

# <a name="tenant-resource-usage-api"></a>组织资源使用 API

租户可以使用租户 API 以查看组织的资源使用数据。 此 API 是符合 Azure 使用 API （目前在专用的预览）。

可以使用 Windows PowerShell cmdlet **Get UsageAggregates** Azure 中获取使用数据，例如。

## <a name="api-call"></a>API 调用

### <a name="request"></a>请求

该请求获取消耗量详细信息的请求订阅和请求的时间范围。 没有任何请求的正文。

| **方法**  | **请求的 URI** |
| ------------ | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 获取         | https://{armendpoint}/subscriptions/{subId}/providers/Microsoft.Commerce/usageAggregates?reportedStartTime={reportedStartTime}&reportedEndTime={reportedEndTime}&aggregationGranularity={granularity}&api-version=2015-06-01-preview&continuationToken={token-value} |

### <a name="arguments"></a>参数

| **参数**             | **说明** |
| -------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| *Armendpoint*             | Azure Azure 堆栈环境中的资源管理器终结点。 |
| *subId*                   | 执行调用的用户订阅 ID。 仅对查询此 API 可用于单个订阅的使用情况。 提供程序可用于所有租户提供资源使用 API 对查询使用。 |
| *reportedStartTime*       | 查询的开始时间。 *日期时间*值应以 UTC 和小时，例如，13:00 开始。 每日聚合，将此值设置为午夜 UTC。 格式是*转义*ISO 8601，例如，2015年-06 16t18%3a53%3a11%2b00 %3a00z，冒号到 %3a 的转义，并加上，以便它是友好的 URI 转义为 %2b。 |
| *reportedEndTime*         | 查询的结束时间。 应用于*reportedStartTime*的约束也应用于该参数。 *ReportedEndTime*的值不能为在将来。 |
| *aggregationGranularity*  | 有两个独立的可能值的可选参数︰ 每天和每小时。 建议值，一个返回的数据在每日的粒度和另一种是每小时解决。 每日的选项为默认选项。 |
| *subscriberId*            | 订阅 id。 若要获取已筛选的数据，提供一个直接租户的订阅 ID 是必需的。 如果未不指定任何订阅 ID 参数，调用返回使用率数据提供商直接承租人。 |
| *api 版本*             | 用于发出此请求的协议版本。 您必须使用 2015年-06-01 的预览。 |
| *continuationToken*       | 从使用 API 提供程序最后一次调用中检索令牌。 这被必需的响应大于 1000 行时。 这是进度的书签。 如果不存在，从当天开始检索数据或传入小时，基于粒度。 |

### <a name="response"></a>响应

获得 /subscriptions/sub1/providers/Microsoft.Commerce/UsageAggregates?reportedStartTime=reportedStartTime=2014-05-01T00%3a00%3a00%2b00%3a00 & reportedEndTime = 2015年-06 01t00%3a00%3a00%2b00 %3a00 和 aggregationGranularity = 每天和 api 版本 = 1.0

{

"值":\[

{

"id":"/ subscriptions/sub1/providers/Microsoft.Commerce/UsageAggregate/sub1-meterID1"，

"name":"sub1-meterID1"，

"类型":"Microsoft.Commerce/UsageAggregate"

"属性": {

"subscriptionId":"sub1"

"usageStartTime":"2015年-03-03T00:00:00 + 00:00"，

"usageEndTime":"2015年-03-04T00:00:00 + 00:00"，

"instanceData":"{\\"Microsoft.Resources\\": {\\"resourceUri\\":\\"resourceUri1\\"，\\"位置\\":\\"阿拉斯加\\"，\\"标记\\": 空，\\"additionalInfo\\": 空}}"，

"数量":2.4000000000

"meterId":"meterID1"

}

},

…

### <a name="response-details"></a>响应详细信息

| **参数**      | **说明** |
| ------------------ | ------------------------------------------------------------------------------------------------------------- |
| *标识*              | 使用聚合的唯一 ID |
| *名称*            | 使用集合的名称 |
| *类型*            | 资源定义 |
| *subscriptionId*  | 订阅的 Azure 用户标识符 |
| *usageStartTime*  | UTC 开始使用存储桶属于此使用聚合的时间 |
| *usageEndTime*    | 此用法聚合属于使用桶 UTC 结束时间 |
| *instanceData*    | 实例 （以新的格式） 的详细信息的键 / 值对︰<br>  *resourceUri*︰ 完全限定的资源 ID，包括资源组和实例名称 <br>  *位置*︰ 在其中运行此服务的区域 <br>  *标记*︰ 在用户指定的资源标记 <br>  *additionalInfo*︰ 更多详细信息所消耗的资源，如操作系统版本或图像类型 |
| *数量*        | 这段时间内出现的资源消耗的量 |
| *meterId*         | 唯一 ID 是资源消耗 (也称为的*资源 Id*) |

## <a name="next-steps"></a>下一步行动

[与使用相关的常见问题解答](azure-stack-usage-related-faq.md)
