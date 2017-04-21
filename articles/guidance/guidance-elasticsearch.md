
<properties
   pageTitle="在 Azure 指导 Elasticsearch |Microsoft Azure"
   description="在 Azure 指南上的 Elasticsearch。"
   services=""
   documentationCenter="na"
   authors="dragon119"
   manager="bennage"
   editor=""
   tags=""/>

<tags
   ms.service="guidance"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="masashin"/>

# <a name="elasticsearch-on-azure-guidance"></a>Elasticsearch 在 Azure 的指南 

[AZURE.INCLUDE [pnp-header](../../includes/guidance-pnp-header-include.md)]

Elasticsearch 是一个高度可扩展的开源搜索引擎和数据库。 它是信息的适用于需要快速分析和发现保存在大数据集的情况。 本指南会查找 Elasticsearch 群集中，重点放在优化和测试您的配置的方法设计时，考虑的一些关键方面。

> [AZURE.NOTE]本指南假定您 Elasticsearch 一些基本熟悉。 有关更多详细信息，请访问[Elasticsearch 网站](https://www.elastic.co/products/elasticsearch)。 

- **[在 Azure 上运行 Elasticsearch][]**的常规结构的 Elasticsearch，简要介绍并描述如何实现使用 Azure Elasticsearch 群集。 
- **[在 Azure 上 Elasticsearch 的调整数据接收性能][]**描述 Elasticsearch 群集的部署，并提供深入分析的期望较高的数据提取率时应考虑的各种配置选项。
- **[调整数据聚合和 Elasticsearch 在 Azure 上的查询性能][]**提供了深入的分析时需要考虑的选项决定如何优化您的系统的查询和搜索性能。
- **[配置的复原和恢复在 Azure 上 Elasticsearch][]**描述了 Elasticsearch 群集，可以帮助最小化数据丢失和扩展的数据恢复时间的可能性的一些重要功能。
- **[创建在 Azure 上 Elasticsearch 的性能测试环境][]**描述如何设置用于测试的性能数据提取和查询工作负载 Elasticsearch 群集中的环境。 
- **[实施 JMeter 测试计划 Elasticsearch][]**总结了使用 JMeter 测试计划一起作为 JUnit 测试执行任务，例如将数据上载到 Elasticsearch 群集包含 Java 代码实现的运行性能测试。
- **[部署用于测试 Elasticsearch 性能 JMeter JUnit 取样器][]**描述如何创建和使用 JUnit 取样器，可以生成并上传到 Elasticsearch 群集数据。 这提供了高度灵活的方法来负载测试可以生成大量的测试数据而无需根据外部数据文件。 
- **[运行自动的 Elasticsearch 弹性测试][]**总结了如何运行本系列中使用的可恢复性测试。 
- **[运行自动化的 Elasticsearch 性能测试][]**总结了如何运行本系列中使用的性能测试。


[在 Azure 上运行 Elasticsearch]: guidance-elasticsearch-running-on-azure.md
[在 Azure 上 Elasticsearch 的数据接收性能调优]: guidance-elasticsearch-tuning-data-ingestion-performance.md
[创建测试环境在 Azure 上 Elasticsearch 的性能]: guidance-elasticsearch-creating-performance-testing-environment.md
[JMeter 测试计划实现 Elasticsearch]: guidance-elasticsearch-implementing-jmeter-test-plan.md
[为测试 Elasticsearch 性能部署 JMeter JUnit 取样器]: guidance-elasticsearch-deploying-jmeter-junit-sampler.md
[数据聚合和 Elasticsearch 在 Azure 上的查询性能优化]: guidance-elasticsearch-tuning-data-aggregation-and-query-performance.md
[在 Azure 上 Elasticsearch 上配置的复原和恢复]: guidance-elasticsearch-configuring-resilience-and-recovery.md
[运行自动化的 Elasticsearch 弹性测试]: guidance-elasticsearch-running-automated-resilience-tests.md
[运行自动化的 Elasticsearch 性能测试]: guidance-elasticsearch-running-automated-performance-tests.md
