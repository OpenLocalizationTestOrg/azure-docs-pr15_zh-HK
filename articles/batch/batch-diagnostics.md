<properties
   pageTitle="Azure 批诊断日志记录 |Microsoft Azure"
   description="记录并分析诊断日志事件的 Azure 批帐户资源池和任务等。"
   services="batch"
   documentationCenter=""
   authors="mmacy"
   manager="timlt"
   editor=""/>

<tags
   ms.service="batch"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="big-compute"
   ms.date="10/12/2016"
   ms.author="marsma"/>

# <a name="azure-batch-diagnostic-logging"></a>Azure 批诊断日志记录

与许多的 Azure 服务批服务资源的生命周期内发出某些资源的日志事件。 您可以启用 Azure 批诊断日志来查找资源池和任务，如记录事件，然后使用日志进行诊断评估和监控。 创建池这样的事件，批诊断日志中包括删除池、 任务开始，任务已完成，和其他人。

>[AZURE.NOTE] 本文讨论了批帐户资源本身的日志记录事件、 不是职务和任务输出数据。 存储您的作业和任务的输出数据的详细信息，请参阅[保存 Azure 批处理作业和任务输出](batch-task-output.md)。

## <a name="prerequisites"></a>系统必备组件

* [批处理的 azure 帐户](batch-account-create-portal.md)

* [Azure 存储帐户](../storage/storage-create-storage-account.md#create-a-storage-account)

  若要保留批诊断日志，您必须创建 Azure 存储帐户 Azure 存储日志的位置。 指定此存储帐户时批帐户[启用诊断日志记录](#enable-diagnostic-logging)。 指定当您启用日志收集存储帐户不是[应用程序包](batch-application-packages.md)和[任务输出持久性](batch-task-output.md)文章中引用的链接的存储帐户相同。

  >[AZURE.WARNING] 您将**收取**您的 Azure 存储帐户中存储的数据。 这包括本文中讨论的诊断日志。 设计您的[日志保留策略](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md)时，请记住这一点。

## <a name="enable-diagnostic-logging"></a>启用诊断日志记录

默认情况下，批帐户未启用诊断日志记录。 您必须显式启用诊断日志记录您要监视的每个批处理帐户︰

[如何启用诊断日志的集合](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md#how-to-enable-collection-of-diagnostic-logs)

我们建议您阅读完整的[概述 Azure 诊断日志](../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md)文章，以获得不仅了解如何启用日志记录，但日志类别支持各种 Azure 服务。 例如，Azure 批目前支持一个日志类别︰**服务日志**。

## <a name="service-logs"></a>服务日志

Azure 批服务日志包含批资源如池或任务的生存期由 Azure 批服务发出的事件。 由批处理发出的每个事件都存储在以 JSON 格式指定的存储帐户。 例如，这是正文的**池创建的事件**的示例︰

```json
{
    "poolId": "myPool1",
    "displayName": "Production Pool",
    "vmSize": "Small",
    "cloudServiceConfiguration": {
        "osFamily": "4",
        "targetOsVersion": "*"
    },
    "networkConfiguration": {
        "subnetId": " "
    },
    "resizeTimeout": "300000",
    "targetDedicated": 2,
    "maxTasksPerNode": 1,
    "vmFillType": "Spread",
    "enableAutoscale": false,
    "enableInterNodeCommunication": false,
    "isAutoPool": false
}
```

每个事件主体所在的.json 文件中指定的 Azure 存储帐户。 如果想要直接访问这些日志，您可能希望查看[诊断日志中存储帐户的架构](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md#schema-of-diagnostic-logs-in-the-storage-account)。

## <a name="service-log-events"></a>服务日志事件

批次服务当前发出以下服务日志事件。 此列表可能不全面，因为其他事件可能已添加本文上次更新以来。

| **服务日志事件** |
| ------------------ |
| [创建池][pool_create] |
| [池中删除开始][pool_delete_start] |
| [池中删除完成][pool_delete_complete] |
| [池大小调整开始][pool_resize_start] |
| [池大小调整完成][pool_resize_complete] |
| [任务的开始][task_start] |
| [任务已完成][task_complete] |
| [任务失败][task_fail] |

## <a name="next-steps"></a>下一步行动

除了在 Azure 存储帐户存储诊断日志事件，也可以流到[Azure 事件中心](../event-hubs/event-hubs-what-is-event-hubs.md)，批服务日志事件并将它们发送到[Azure 日志分析](../log-analytics/log-analytics-overview.md)。

* [流到事件集线器的 Azure 诊断日志](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md)

  流到高可扩展的数据入口服务，事件集线器批诊断事件。 事件集线器可以摄取数以百万计，每秒，即可以再转换和存储使用任何实时分析功能提供程序的事件。

* [分析使用日志分析的 Azure 诊断日志](../log-analytics/log-analytics-azure-storage-json.md)

  将诊断日志发送到日志分析，可以对其进行分析，在操作管理套件 (OMS) 门户中，或将其导出以便在电源 BI 或 Excel 分析。

[pool_create]: https://msdn.microsoft.com/library/azure/mt743615.aspx
[pool_delete_start]: https://msdn.microsoft.com/library/azure/mt743610.aspx
[pool_delete_complete]: https://msdn.microsoft.com/library/azure/mt743618.aspx
[pool_resize_start]: https://msdn.microsoft.com/library/azure/mt743609.aspx
[pool_resize_complete]: https://msdn.microsoft.com/library/azure/mt743608.aspx
[task_start]: https://msdn.microsoft.com/library/azure/mt743616.aspx
[task_complete]: https://msdn.microsoft.com/library/azure/mt743612.aspx
[task_fail]: https://msdn.microsoft.com/library/azure/mt743607.aspx
