<properties
    pageTitle="与使用相关的常见问题解答 |Microsoft Azure"
    description="Azure 堆栈米、 比较到 Azure 使用 API、 使用时间和报告时间，错误代码的列表。"
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

# <a name="azure-stack-usage-api-faqs"></a>Azure 堆栈使用 API 常见问题解答
本文解答一些有关 Azure 堆栈使用 API 的常见问题。

## <a name="what-meter-ids-can-i-see"></a>可看到哪种计量器 Id？

目前，使用情况报告的网络、 存储和计算资源提供程序。

| **资源提供程序** | **计量器 ID** |**计量器名称** | **单位** | **附加信息** |
| --------------------------- | --------------------------------------- | -------------------------- | ---------------------------- | ----------------------------------------- |
| **网络** | f114cb19-ea64-40b5-bcd7-aee474b62853 | 公共 IP 地址的使用情况 | IP 地址 |                    
| **存储**  | B4438D5D-453B-4EE1-B42A-DC72E377F1E4 | TableCapacity | GB\*小时 | 总容量由表 |
|              | B5C15376-6C94-4FDD-B655-1A69D138ACA3 | PageBlobCapacity | GB\*小时 | 由页面 blob 的总容量 |
|              | B03C6AE7-B080-4BFA-84A3-22C800F315C6 | QueueCapacity  | GB\*小时  | 由队列使用的总容量 |
| | 09F8879E-87E9-4305-A572-4B7BE209F857 | BlockBlobCapacity | GB\*小时  | 由块 blob 的总容量 |
| | B9FF3CD0-28AA-4762-84BB-FF8FBAEA6A90 | TableTransactions  | 10,000s 中的请求计数   | 表服务请求 （10,000s) |
| | 50A1AEAF-8ECA-48A0-8973-A5B3077FEE0D | TableDataTransIn | 入口数据以 gb 为单位 | 表服务数据入口以 gb 为单位 |
| | 1B8C1DEC-EE42-414B-AA36-6229CF199370 | TableDataTransOut | Outgress 以 gb 为单位 | 表服务数据出口以 gb 为单位 |
| | 43DAF82B-4618-444A-B994-40C23F7CD438 | BlobTransactions | 10,000s 中的请求计数 | Blob 服务请求 （10,000s) |
| | 9764F92C-E44A-498E-8DC1-AAD66587A810   | BlobDataTransIn    | 入口数据以 gb 为单位          | Blob 服务数据入口以 gb 为单位 
| | 3023FEF4-ECA5-4D7B-87B3-CFBC061931E8   | BlobDataTransOut   | Outgress 以 gb 为单位              | Blob 服务数据出口以 gb 为单位 
| | EB43DD12-1AA6-4C4B-872C-FAF15A6785EA   | QueueTransactions  | 10,000s 中的请求计数   | 队列服务请求 （10,000s) 
| | E518E809-E369-4A45-9274-2017B29FFF25   | QueueDataTransIn          | 入口数据以 gb 为单位         | 以 gb 为单位的队列服务数据入口 
| | DD0A10BA-A5D6-4CB6-88C0-7D585CEF9FC2   | QueueDataTransOut         | Outgress 以 gb 为单位  | 队列服务数据出口以 gb 为单位 
| **计算** | 6DAB500F-A4FD-49C4-956D-229BB9C8C793 | VM 大小小时 | 虚拟机大小 |



## <a name="how-do-the-azure-stack-usage-apis-compare-to-the-azure-usage-apihttpsmsdnmicrosoftcomlibraryazure1ea5b323-54bb-423d-916f-190de96c6a3c-currently-in-public-preview"></a>Azure 堆栈使用 Api （目前在公共预览） 的[Azure 使用 API](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c)相比如何？

-   租户使用 API 是符合 Azure API 中，有一个例外︰ *showDetails*标志当前不支持在 Azure 堆栈中。

-   提供程序使用 API 仅适用于 Azure 堆栈。

-   目前，位于 Azure [RateCard API](https://msdn.microsoft.com/en-us/library/azure/mt219004.aspx)不可用 Azure 堆栈中。

## <a name="what-is-the-difference-between-usage-time-and-reported-time"></a>使用时间和报告时间之间的区别是什么？

使用率数据报告有两个主要的时间值︰

-   **报告的时间**。 当使用事件输入使用系统的时间

-   **使用时间**。 当 Azure 堆栈资源被消耗的时间

具体使用事件，您可能使用时间和报告时间看到值之间存在差异。 延迟可能是只要在任何环境中的多个小时。

目前，可以查询*只能由报告时间*。

## <a name="what-do-these-usage-api-error-codes-mean"></a>这些使用 API 错误代码的含义是什么？

| **HTTP 状态代码** | **错误代码** | **说明** |
| ---------------------- | ------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------ |
| 400/错误的请求        | *NoApiVersion*     | 找不到的*api 版本*查询参数。
| 400/错误的请求        | *InvalidProperty*  | 属性丢失或包含无效的值。 在响应正文中的错误代码消息标识缺少的属性。
| 400/错误的请求        | *RequestEndTimeIsInFuture*  | *ReportedEndTime*的值是在将来。 此参数在将来不允许值。
| 400/错误的请求        | *SubscriberIdIsNotDirectTenant*    | 提供程序 API 调用使用不可调用方有效租户订阅 ID。
| 400/错误的请求        | *SubscriptionIdMissingInRequest*   | 找不到调用方的订阅 ID。
| 400/错误的请求        | *InvalidAggregationGranularity*   | 请求了无效的聚合粒度。 有效值为每天和每小时。
| 503                    | *ServiceUnavailable*   | 可重试错误发生，因为服务正忙或调用将被阻止。 |

## <a name="next-steps"></a>下一步行动
[客户帐单和 Azure 堆栈中的存储容量使用计费](azure-stack-billing-and-chargeback.md)

[提供程序资源使用 API](azure-stack-provider-resource-api.md)

[组织资源使用 API](azure-stack-tenant-resource-usage-api.md)
