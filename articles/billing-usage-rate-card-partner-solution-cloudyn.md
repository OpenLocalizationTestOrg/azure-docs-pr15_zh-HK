<properties
   pageTitle="Microsoft Azure 使用并为客户提供 ITFM RateCard Api 启用 Cloudyn |Microsoft Azure"
   description="从 Microsoft Azure 计费合作伙伴 Cloudyn，其经验将 Azure 计费 Api 集成到他们的产品提供独特的观点。  这是特别有用的 Azure 和 Cloudyn 感兴趣使用/尝试 Cloudyn Azure 服务的客户。"
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

# <a name="microsoft-azure-usage-and-ratecard-apis-enable-cloudyn-to-provide-itfm-for-customers"></a>Microsoft Azure 用法和 RateCard Api 使 Cloudyn 为客户提供 ITFM

新的 Microsoft Azure 资源使用情况和 RateCard Api 的专用预览选择了 Cloudyn，Microsoft 开发伙伴和云管理功能，保持着领先。  使用 API 提供对 Azure 的估计的消耗量数据订阅的访问。 该 RateCard API 提供了完成所有的 Azure 服务，为非企业协议 EA 客户的定价信息。 这些 Api 集成在一起，给 IT 财务管理 (ITFM) 工具，例如那些由 Cloudyn 提供的输入提供完整的信息基础。

## <a name="introduction"></a>介绍

所谓"乘法"的数据从使用 API 使用来自 RateCard API 的数据 (使用 [单位] 价格 [$unit] = 详细的使用和成本) 创建最精细、 准确和可靠记帐信息可用 Azure 今天。

![ITFM 概述][1]

使用这些 Api 于客户的使用情况和成本，使 Cloudyn 以一种简单的编程方式，分析客户帐户并执行各种 ITFM 任务为其客户提供关键信息。

## <a name="integrating-cloudyn-with-the-ratecard-and-usage-apis"></a>将 Cloudyn 与 RateCard 并使用 Api 集成
RateCard API 需要多个输入的参数--如区域信息、 货币和区域设置，但最重要的一项是 OfferDurableID，向客户提供的 Azure 的类型使用 （即付即用、 旧式 6 到 12 个月的承诺计划、 MSDN 提供、 MPN 优惠、 促销优惠和其他人） 指定。 OfferDurableID 可以找到在[Azure 的用法和计费门户](https://account.windowsazure.com/Subscriptions)，在"提供 ID"对于给定的订阅。

在注册[Azure 的 Cloudyn](https://www.cloudyn.com/microsoft-azure/)服务，客户可以添加他们允许拉通过 RateCard API 及其相关定价信息的 Cloudyn 的 OfferDurableID 代码。  提供不同类型的信息可以找到一个[Microsoft Azure 提供详细信息](https://azure.microsoft.com/support/legal/offer-details/)页面。

![Cloudyn ITFM 引擎概述][2]

Cloudyn 使用用法和 RateCard Api，Azure 性能 API 中，除了创建可视化效果、 分析、 报警、 报告、 成本管理和具有可操作性的建议，其它的图层为 Azure 的客户提供可靠的企业云 ITFM 工具。

## <a name="cloudyn-itfm-use-cases-enabled-by-usage-and-ratecard-api-integration"></a>启用使用情况和 RateCard API 集成 Cloudyn ITFM 使用案例
常见的 Cloudyn ITFM 使用通过使用已启用的情况下，RateCard Api 包括︰

+ **成本分析**-允许云被分解为任何本机的标识维度 （提供程序、 服务、 帐户、 区域等） 的成本。 Azure 用法和 RateCard Api 使此容易的任务，通过提供用法的最精细的分解，成本每个帐户，然后分组和按 Cloudyn 筛选并显示给用户，以图形或表格形式的数据。

![成本分析饼形图][3]

+ **成本分配 360** -使金融和 IT 经理们能够找出实际成本细分、 驱动程序和他们的云部署的趋势。 进一步，它使管理员能够轻松地与业务单元、 部门、 地区，和更多，提供了前所未有的洞察力云成本和促进企业的资源监测和 showbacks 的部署费用。 Azure 用法和 RateCard Api 作为输入 Cloudyn 的成本分配引擎，这是 Api 通过定义方法和无标记或 untaggable 资源分配的业务逻辑的补充。

![成本的分摊 360 图表][4]

+ **经济高效调整**— 提供校正规模建议未充分利用的虚拟机，从而降低客户的支出过大或过大机器上。 它是通过检查虚拟机的 CPU 和 RAM 指标 （通过性能 API) 小时 （通过使用 API) 的运行时和成本 （通过 RateCard API)。 Cloudyn 提供了校正规模建议根据未充分利用的 CPU 或内存资源 （性能），然后将预计的节约乘以价格增量 (RateCard) 计算的实际时间利用率 （用法） 未充分利用计算机的虚拟机之间。

![经济调整][5]

+ **云将移植建议**-在云上提供财务建议移植。 它会检查用户的当前部署在主要的云供应商的云资源的成本并将它与在 Azure 上等效部署的成本进行比较。 然后提供精细，每个资源，财务上的基于移植到 Azure 的建议。 后评估 （基于性能指标和用户首选项） 的 Azure 上所需的等效部署，Cloudyn 使用 RateCard API 来评估在 Azure 上等效的部署成本。

+ **性能报告**-启用通过 Azure 的性能 API，这些报告可以提供数组的功能从 CPU 和内存利用率的优化建议。 下面是实例利用率报告示例，演示实例分解按平均 CPU 使用率。

![性能报告][6]

+ **类别管理器**-Cloudyn 为未经组织的云资源带来订单中的强大功能。 它让用户能够自由地进行有效测量和报告创建它们自己的唯一类别 （标签） 嵌入业务做法。 而且，用户可以方便地调节和分类不一致的标记 （如打字错误和其他不符之处），自动检测的准确成本归属未加标签的资源。

![分类管理器][7]

## <a name="video"></a>视频

下面是一个简短的视频，其中显示 Azure 的客户如何使用 Cloudyn Azure 和 Azure 计费 Api 来获得从他们的 Azure 消耗数据的见解。

> [AZURE.VIDEO cloudyn-provides-cloud-itfm-tools-via-microsoft-azure-apis]


## <a name="next-steps"></a>下一步行动

+ 开始免费[Cloudyn Azure 的](https://www.cloudyn.com/microsoft-azure/)试用版，请参阅 Microsoft Azure 云部署，您就可以获得与自定义的报告和分析的成本透明度。
+ Azure 资源使用情况和 RateCard Api 的概述，请参阅[深入了解您的 Microsoft Azure 资源消耗](billing-usage-rate-card-overview.md)。
+ 检查出更多有关这两个是 Azure 资源管理器提供的 Api 集的一部分的 Api [Azure 计费 REST API 参考](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c)。
+ 如果您想要讲解的示例代码，签出我们 Microsoft Azure 计费 API 代码示例在[Azure 代码示例](https://azure.microsoft.com/documentation/samples/?term=billing)。

## <a name="learn-more"></a>了解更多信息
+ 若要了解有关 Microsoft Azure 企业协议 (EA) 提供的详细信息，请访问 [企业授权 Azure] (https://azure.microsoft.com/pricing/enterprise-agreement/)
+ 请参见[Azure 资源管理器概述](azure-resource-manager/resource-group-overview.md)文章了解更多关于 Azure 资源管理器。
+ 上了一套工具，从而帮助在了解云花，请参考 Gartner 文章[市场指南为 IT 财务管理 (ITFM) 工具](http://www.gartner.com/technology/reprints.do?id=1-212F7AL&ct=140909&st=sb)的其他信息。

<!--Image references-->
[1]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-ITFM-Overview.png
[2]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-ITFM-Engine-Overview.png
[3]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Analysis-Pie-Chart.png
[4]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Allocation-360-Chart.png
[5]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Cost-Effective-Sizing.png
[6]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Performance-Reports.png
[7]: ./media/billing-usage-rate-card-partner-solution-cloudyn/Cloudyn-Category-Manager.png
