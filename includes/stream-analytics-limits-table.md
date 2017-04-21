<properties 
   pageTitle="流分析限制表"
   description="介绍了系统的限制和流分析组件和连接的推荐的大小。"
   services="stream-analytics"
   documentationCenter="NA"
   authors="jeffstokes72"
   manager="paulettm"
   editor="cgronlun" />
<tags 
   ms.service="stream-analytics"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="big-data"
   ms.date="07/25/2016"
   ms.author="jeffstok" />

| 限制标识符 | 限制       | 注释 |
|----------------- | ------------|--------- |
| 流式处理单位的每个订阅每个地区的最大数量 | 50 | 可通过联系[Microsoft 技术支持](https://support.microsoft.com/en-us)请求，以增加流单位订购超过 50。 |
| 流式处理单位的最大吞吐量 | 1 MB / s * | 每提的最大吞吐量取决于该方案。 实际的吞吐量可能较低，取决于查询的复杂性和分区。 [小数位数 Azure 流分析作业，以增加吞吐量](../articles/stream-analytics/stream-analytics-scale-jobs.md)文章中找不到更多详细信息。 |
| 输入每个作业的最大数目 | 60 | 还有 60 输入每个流分析作业的硬限制。 |
| 输出每个作业的最大数目 | 60 | 还有 60 输出每个流分析作业的硬限制。 |
| 每个作业功能的最大数量 | 60 | 没有每个流分析作业 60 函数的硬限制。 |
| 每个地区作业的最大数量 | 1500 | 每个订阅，可能有多达 1500年每个地理地区的作业。 |