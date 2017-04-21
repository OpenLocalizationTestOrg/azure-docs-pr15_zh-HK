<properties
   pageTitle="深入了解您的 Microsoft Azure 资源消耗 |Microsoft Azure"
   description="提供了用于深入 Azure 的资源消耗和趋势的 Azure 计费用法和 RateCard Api 的概念性概述。"
   services=""
   documentationCenter=""
   authors="BryanLa"
   manager="mbaldwin"
   editor=""
   tags="billing"/>

<tags
   ms.service="billing"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="billing"
   ms.date="08/16/2016"
   ms.author="mobandyo;bryanla"/>

# <a name="gain-insights-into-your-microsoft-azure-resource-consumption"></a>深入了解您的 Microsoft Azure 资源消耗

客户和合作伙伴需要能够准确地预测并管理其 Azure 的成本。  移动从 Capex 到 Opex 模型，他们还需要能够执行与监测分析，showback，以及提供估计和付费，尤其是对于大的云部署模式保真度。

Azure 资源使用率和费率卡 Api 中讨论此文章地址这些需求，通过使您的 Azure 资源消耗的新见解。  

## <a name="introducing-the-azure-resource-usage-and-ratecard-apis"></a>Azure 的资源使用情况和 RateCard Api 简介

Azure 资源使用情况和 RateCard Api 作为资源提供程序，作为公开 Azure 资源管理器 Api 系列的一部分。  

### <a name="azure-resource-usage-api-preview"></a>Azure 资源使用 API （预览）
客户和合作伙伴可以使用 Azure 资源使用 API 来将 Azure 的估计的消耗量数据。 这些功能包括︰

- [Azure 的门户网站](https://portal.azure.com)或通过[Azure PowerShell cmdlet](powershell-install-configure.md)指定哪些用户或应用程序可以访问订阅的使用率数据，**基于 azure 角色的访问控制**的客户和合作伙伴可以配置其访问策略。 调用方必须使用标准的 Azure Active Directory 令牌进行身份验证。 调用方还必须添加到读者、 所有者或讨论参与者角色有权访问特定的 Azure 预订使用率数据。

- **每小时或每日聚合**-调用方可以指定是否需要桶每小时或每日存储桶中其 Azure 的使用率数据。 默认值是每日。

- **提供的实例元数据 （包括资源标记）** -实例级别的详细信息，例如完全限定的资源 uri (/subscriptions/ {订阅 id} /...)，与资源组的信息和资源标记将提供在响应中。 这将帮助客户明确并以编程方式分配标记，使用用例等跨充电。

- **提供的资源元数据**的资源的详细信息，如指示器的名称、 计量器类别、 米子类别、 单位和地区也将在响应中，提供更好的了解什么消耗的调用方传递。 我们也努力地在 Azure 的门户，对齐资源元数据术语 Azure 使用 CSV，EA 计费 CSV 和其他面向公众的体验，使客户能够跨经验关联数据。

- **提供的所有类型的用法**— 对于所有提供类型包括即付即用、 MSDN、 货币承诺、 货币信用和 EA 等，都可以使用数据。

### <a name="azure-resource-ratecard-api-preview"></a>Azure 资源 RateCard API （预览）
客户和合作伙伴可以使用 Azure 资源 RateCard API 一起获得的可用 Azure 资源列表，估计定价为每个信息。 这些功能包括︰

- **基于 azure 角色的访问控制**的客户和合作伙伴可以配置其访问策略在[Azure 的门户网站](https://portal.azure.com)或通过[Azure PowerShell cmdlet](powershell-install-configure.md)来指定哪些用户或应用程序可以获得对 RateCard 数据的访问。 调用方必须使用标准的 Azure Active Directory 令牌进行身份验证。 调用方还必须添加到读者、 所有者或讨论参与者角色有权访问特定的 Azure 预订使用率数据。

- **即付即用、 MSDN，货币的承诺和货币信用支持提供 (不支持 EA)** -此 API 提供了 Azure 优惠级率信息，与订阅级别。  此 API 的调用方必须传递获取资源详细信息和费率的优惠信息。  当 EA 提供自定义每个注册的速率，我们不能以这次提供的 EA 率。

## <a name="scenarios"></a>方案

下面是一些可能的用法和 RateCard Api 的组合进行的方案︰

- **Azure 花在指定月份内**的客户可以使用用法和 RateCard Api 结合才能更好地深入了解其云花在月期间，通过分析的使用率和费用估计的每小时和每日存储桶。

- **设置警报**– 客户和合作伙伴可以将设置对其云消耗的资源基于或基于货币的警报通过获取的估计的消耗量和使用用法和 RateCard API 的费用估计。

- **Predict 单**– 客户和合作伙伴可以得到它们的估计的消耗量和云花和应用机器学习算法来预测其计费的计费周期的末尾是什么。

- **预消耗成本分析**– 客户可还使用 RateCard API 来预测其多少计费就是它们是移动它们的工作负载到 Azure，通过提供所需使用的数字。 如果客户有现有的工作负载中其他云或私有云，他们可以映射其使用 Azure 率要更好地估计其估计 Azure 的花费。 这提供了增强的视图的内容可以通过[Azure 的价格计算器](https://azure.microsoft.com/pricing/calculator/)，获得我们付费的合作伙伴 （例如） 提供优惠在透视和比较/对比之外即付即用，包括货币的承诺和货币信贷的提供不同的服务类型间的能力。 Api 还提供了执行成本的地区，估计变化的能力使需制定部署决策、 部署在世界各地的不同域控制器中的资源可以在总成本上有直接的影响的假设分析的类型。

- **假设分析** -

    - 客户和合作伙伴可以确定是否会更加经济有效地运行它们的工作负载，在另一个区域，或在 Azure 的资源的另一种配置。 Azure 的资源成本可能不同基于 Azure 的区域中运行，并这样客户和合作伙伴可获得成本优化。

    - 客户和合作伙伴还可以确定其他 Azure 服务类型是否 Azure 资源上给予更好的速度。

## <a name="partner-solutions"></a>合作伙伴解决方案

[Microsoft 的 Azure 使用和向客户提供 ITFM 的 RateCard Api 启用 Cloudyn](billing-usage-rate-card-partner-solution-cloudyn.md)描述 Azure 计费 API 合作伙伴[Cloudyn](https://www.cloudyn.com/microsoft-azure/)所提供的集成经验。  这篇文章提供其经验，包括一个简短的视频，其中显示如何 Azure 的客户可以使用 Cloudyn 和 Azure 计费 Api 到收益的见解从 Azure 消耗数据详细覆盖的范围。

[云中 Cruiser 和 Microsoft Azure 计费 API 集成](billing-usage-rate-card-partner-solution-cloudcruiser.md)介绍[Azure 包云 Cruiser Express](http://www.cloudcruiser.com/partners/microsoft/)原理直接从 WAP 门户，从而使客户能够无缝地从一个用户界面管理其 Microsoft Azure 私有或承载公共云的运营和财务方面。   

## <a name="next-steps"></a>下一步行动
+ 检查出[Azure 计费 REST API 参考](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c)如欲详细了解这两种 Api 是 Azure 资源管理器提供的 Api 集的一部分。
+ 如果您想要讲解的示例代码，签出我们 Microsoft Azure 计费 API 代码示例在[Azure 代码示例](https://azure.microsoft.com/documentation/samples/?term=billing)。

## <a name="learn-more"></a>了解更多信息
+ 请参见[Azure 资源管理器概述](azure-resource-manager/resource-group-overview.md)文章了解更多关于 Azure 资源管理器。
+ 上了一套工具，从而帮助在了解云花，请参考 Gartner 文章[市场指南为 IT 财务管理 (ITFM) 工具](http://www.gartner.com/technology/reprints.do?id=1-212F7AL&ct=140909&st=sb)的其他信息。
