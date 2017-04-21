
<properties
   pageTitle="Azure 指南 |模式和实践 |Microsoft Azure"
   description="最佳做法和 Azure 的指南"
   services=""
   documentationCenter="na"
   authors="bennage"
   manager="marksou"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/17/2016"
   ms.author="christb"/>

# <a name="azure-guidance"></a>Azure 的指南

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Microsoft 的模式与实践小组是 Azure 客户顾问团队的一部分。 我们的目的是为了帮助开发人员、 架构师和 IT 专业人员成功在 Microsoft Azure 平台上。 我们开发显示基于 Azure 的云解决方案的最佳做法的指南。

## <a name="checklists"></a>检查表

这些列表是审阅的可用性和可伸缩性的基本方面的快速参考。 

- [可用性检查表][AvailabilityChecklist] 

    推荐的做法来确保可用性的摘要。

- [可伸缩性核对清单][ScalabilityChecklist]

    设计和实现可伸缩的服务以及处理数据管理的建议做法的摘要。

## <a name="best-practices-articles"></a>最佳实践的文章

这些文章提供深入讨论的重要概念，通常与云计算。 

- [API 设计][APIDesign] 

    讨论了设计一个 web API 时需要考虑的设计问题。

- [API 实现][APIImplementation] 

    一套推荐的做法来实现和发布 web API。

- [API 的安全指南](https://github.com/mspnp/azure-guidance/blob/master/API-security.md) 

    （例如，标记类型、 授权协议，授权流和威胁缓解） 的身份验证和授权问题的讨论。

- [自动缩放指南][AutoscalingGuidance] 

    摘要考虑规模调整解决方案，而无需手动干预。

- [后台作业指导][BackgroundJobsGuidance] 

    可用的选项和推荐的做法来实现应在后台，独立于任何前景或交互式操作执行的任务的说明。

- [内容传递网络 (CDN) 指南][CDNGuidance] 

    一般性指导原则和建议的做法，使用 CDN 最小化您的应用程序的负载并最大化可用性和性能。

- [缓存的指南][CachingGuidance] 

    简要介绍如何使用缓存以提高性能和可扩展性的系统。

- [数据分区指导][DataPartitioningGuidance]

    策略可用于对数据进行分区以提高可扩展性，减少争用，并优化性能。

- [监视和诊断指南][MonitoringandDiagnosticsGuidance] 

    跟踪您的用户将如何使用您的系统，指导跟踪资源利用率，并通常监视运行状况和性能的系统。

- [推荐的命名约定][naming-conventions] 

    Azure 资源建议的命名约定。

- [重试的一般指南][RetryGeneralGuidance] 

    讨论了用于处理瞬时故障的一般概念。

- [重试特定于服务的指南][RetryServiceSpecificGuidance]

    对于许多 Azure 服务，包括信息可帮助您使用、 修改，或扩展该服务的重试机制重试功能的摘要。

## <a name="scenario-guides"></a>方案指南

- [在 Azure 上运行 Elasticsearch][elasticsearch] 
    
    Elasticsearch 是一个高度可扩展的开源搜索引擎和数据库。 它是信息的适用于需要快速分析和发现保存在大型数据集的情况。 本指南会查找某些设计 Elasticsearch 群集时要考虑的关键方面。

- [对于多租户应用程序的标识管理][identity-multitenant] 
    
    多租赁是体系结构，其中多个租户共享应用程序，但被相互隔离。 本指南介绍了如何管理一个多租户应用程序，使用[Azure Active Directory]中的用户标识[AzureAD]以处理登录和身份验证。
    
- [开发大数据的解决方案](https://msdn.microsoft.com/library/dn749874.aspx)

    此指南研究方案，如反复勘探，作为数据仓库，ETL 过程和集成到现有的 BI 系统 HDInsight 的使用。 它还包括了解大数据、 规划和设计的大数据解决方案的概念和实施这些解决方案的指导。
    
## <a name="patterns"></a>模式

- [对云应用程序的云设计模式︰ 体系结构指导](https://msdn.microsoft.com/library/dn568099.aspx)

    云的设计模式是设计模式和相关的指南主题的库。 它清楚地解释的应用模式，通过显示每条如何适应于云应用程序体系结构的好处。
    
- [对云应用程序优化性能](https://github.com/mspnp/performance-optimization)

    本指南是妨碍负载的情况下缩放的应用程序的常见反模式的探索。 它包括八个反模式和[性能分析的初级读本](https://github.com/mspnp/performance-optimization/blob/master/Performance-Analysis-Primer.md)和评估[对关键指标的性能](https://github.com/mspnp/performance-optimization/blob/master/Assessing-System-Performance-Against-KPI.md)指南演示的示例。

## <a name="reference-architectures"></a>参考体系结构

我们的参考体系结构的排列方案。
每个单独的体系结构提供了建议的最佳实践和说明性步骤，以及体现建议的可执行组件。

[Http://aka.ms/architecture](http://aka.ms/architecture)提供了当前存储库的参考体系结构。

## <a name="resiliency-guidance"></a>可恢复性指南

这些主题介绍如何设计应用程序是分布式的云环境中的故障恢复能力。   

- [弹性概览][ResiliencyOvervew]

     如何生成在 Azure 平台上的应用程序可以从故障中恢复并继续工作。 介绍了结构化的方法来实现复原，从设计到实现、 部署和操作。

- [可恢复性清单][resiliency-checklist]

    一份清单的建议，可以帮助您计划为各种可能发生的故障模式。

- [故障模式分析][resiliency-fma] 

    故障模式分析 (FMA) 是构建过程进行复原到系统中，通过确定可能的故障点。 FMA 过程的起点，为这篇文章包含目录中潜在的故障模式及他们缓解。 

<!-- links -->

[AzureAD]: https://azure.microsoft.com/documentation/services/active-directory/

[PerformanceOptimization]: https://github.com/mspnp/performance-optimization

[APIDesign]: ../best-practices-api-design.md
[APIImplementation]: ../best-practices-api-implementation.md
[AutoscalingGuidance]: ../best-practices-auto-scaling.md
[BackgroundJobsGuidance]: ../best-practices-background-jobs.md
[CDNGuidance]: ../best-practices-cdn.md
[CachingGuidance]: ../best-practices-caching.md
[DataPartitioningGuidance]: ../best-practices-data-partitioning.md
[MonitoringandDiagnosticsGuidance]: ../best-practices-monitoring.md
[RetryGeneralGuidance]: ../best-practices-retry-general.md
[RetryServiceSpecificGuidance]: ../best-practices-retry-service-specific.md
[RetryPolicies]: Retry-Policies.md
[ScalabilityChecklist]: ../best-practices-scalability-checklist.md
[AvailabilityChecklist]: ../best-practices-availability-checklist.md
[naming-conventions]: guidance-naming-conventions.md

<!-- guidance projects -->
[elasticsearch]: guidance-elasticsearch.md
[identity-multitenant]: guidance-multitenant-identity.md

<!-- reference architectures -->
[ref-arch-single-vm-windows]: guidance-compute-single-vm.md
[ref-arch-single-vm-linux]: guidance-compute-single-vm-linux.md
[ref-arch-multi-vm]: guidance-compute-multi-vm.md
[ref-arch-3-tier]: guidance-compute-3-tier-vm.md
[ref-arch-n-tier-windows]: guidance-compute-n-tier-vm.md
[ref-arch-n-tier-linux]: guidance-compute-n-tier-vm-linux.md
[ref-arch-multi-dc-windows]: guidance-compute-multiple-datacenters.md
[ref-arch-multi-dc-linux]: guidance-compute-multiple-datacenters-linux.md

<!-- resiliency -->
[resiliency-fma]: guidance-resiliency-failure-mode-analysis.md
[resiliency-checklist]: guidance-resiliency-checklist.md
[ResiliencyOvervew]: guidance-resiliency-overview.md

