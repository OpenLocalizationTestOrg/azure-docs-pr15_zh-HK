<properties 
    pageTitle="使用案例 — 客户分析" 
    description="了解如何使用 Azure 数据工厂创建数据驱动的工作流 （管道） 来分析游戏的客户。" 
    services="data-factory" 
    documentationCenter="" 
    authors="sharonlo101" 
    manager="jhubbard" 
    editor="monicar"/>

<tags 
    ms.service="data-factory" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/06/2016" 
    ms.author="shlo"/>

# <a name="use-case---customer-profiling"></a>使用案例 — 客户分析

Azure 数据工厂是一种用于实现了 Cortana 智能套解决方案加速器的许多服务。  Cortana 智能有关的详细信息，请访问[Cortana 智能套件](http://www.microsoft.com/cortanaanalytics)。 在本文中，我们描述了一个简单的使用案例来帮助您开始了解如何 Azure 数据工厂可以解决常见的分析问题。

您只需要访问和试验这一简单的使用情形是[Azure 的订阅](https://azure.microsoft.com/pricing/free-trial/)。  您可以部署[示例](data-factory-samples.md)文章中介绍的步骤实现这一使用情形示例。

## <a name="scenario"></a>方案

Contoso 是一家游戏公司创建多个平台的游戏︰ 游戏控制台、 手持式设备和个人电脑 (Pc)。 玩家玩这些游戏，因为大量的日志数据生成跟踪使用模式、 游戏样式和用户的首选项。  再加上人口统计学，地区和产品数据，Contoso 还可以指导它们如何提高玩家的体验有关的分析和目标升级并在游戏中购买。 

Contoso 的目标是，确定基于其玩家的游戏纪录的追加销售/交叉销售机会将极具吸引力的功能添加到促进企业发展和为客户提供更好的体验。 为此用例中，我们使用游戏公司作为一个企业的例子。 该公司希望优化其游戏基于玩家的行为。 这些原则适用于任何想要让其客户解决其货物和服务并增强客户体验的企业。

## <a name="challenges"></a>面临的挑战


## <a name="solution-overview"></a>解决方案概述

这简单的用例可以用作如何使用 Azure 数据工厂来摄取、 准备、 转换、 分析和发布数据的一个示例。

![端到端工作流](./media/data-factory-customer-profiling-usecase/EndToEndWorkflow.png)

下图显示了如何数据管道后，显示在 Azure 门户部署了这些。

1.  **PartitionGameLogsPipeline**从 blob 存储中读取原始游戏事件并创建分区的年、 月和日。
2.  **EnrichGameLogsPipeline**加入分区与地区代码引用数据的游戏事件和丰富数据，通过将 IP 地址映射到相应的地理位置。
3.  **AnalyzeMarketingCampaignPipeline**管道使用更丰富的数据，以创建最终的输出包含市场活动的市场营销效果的广告数据对其进行处理。

在此示例中，数据工厂用于协调活动的复制输入的数据、 转换和处理数据，并输出到 SQL Azure 数据库的最终数据。  此外可以直观地显示数据管道网络、 管理它们，并监视其状态从用户界面。

## <a name="benefits"></a>优点

通过优化其用户配置文件分析并使其与业务目标，游戏公司是能够快速收集使用模式，并分析其市场营销活动的有效性。




