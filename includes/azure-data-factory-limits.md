数据工厂是一种多租户服务到位，以确保客户订阅不受彼此的工作负载具有下面的默认限制。 许多限制可以轻松地为您的订阅的最大限制由引发联系支持人员。 

**资源** | **默认限制** | **最大限制**
-------- | ------------- | -------------
在 Azure 的订阅数据工厂 | 50 | [与支持部门联系](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
管道内的数据工厂 | 2500 | [与支持部门联系](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
数据集内的数据工厂 | 5000 | [与支持部门联系](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
每个数据集的并行切片 | 10 | 10
每个对象的管道对象<sup>1</sup>个字节 | 200 KB | 2000 KB
每个对象的数据集和链接的服务对象<sup>1</sup>个字节 | 100 KB | 2000 KB
HDInsight 按需群集核心内预订<sup>2</sup> | 48 | [与支持部门联系](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
云中的数据移动单元<sup>3</sup> | 8 | [与支持部门联系](https://azure.microsoft.com/blog/2014/06/04/azure-limits-quotas-increase-requests/)
重试次数对管道活动运行 | 1000 | MaxInt （32 位）

<sup>1</sup>管道、 数据集和链接的服务对象表示指定工作负荷的逻辑分组。 限制这些对象可以移动并使用 Azure 数据工厂服务处理的数据量无关。 数据工厂旨在扩展以处理 petabytes 的数据。

<sup>2</sup>按需 HDInsight 内核分配从订阅包含数据工厂。 因此，上述限制是数据工厂实施按需 HDInsight 核的核限制和区别与 Azure 订阅相关的核限制。

<sup>3</sup>云数据移动单元 (DMU) 正在使用云与云复制操作中。 它是衡量表示数据工厂中的单个单元的电源 （CPU、 内存和网络资源分配的组合）。 通过利用更多的 DMUs，在某些情况下，您可以实现复制吞吐量。 请参考[云数据移动单位](../../articles/data-factory/data-factory-copy-activity-performance.md#cloud-data-movement-units)部分的详细信息。

**资源** | **默认下限** | **最小限制**
-------- | ------------------- | -------------
计划间隔 | 15 分钟 | 15 分钟
重试尝试之间的间隔 | 1 秒 | 1 秒
重试超时值 | 1 秒 | 1 秒


### <a name="web-service-call-limits"></a>Web 服务呼叫限制

Azure 资源管理器 API 调用的限制。 可以在[Azure 资源管理器 API 限制](../azure-subscription-service-limits.md#resource-group-limits)的速率使 API 调用。 


