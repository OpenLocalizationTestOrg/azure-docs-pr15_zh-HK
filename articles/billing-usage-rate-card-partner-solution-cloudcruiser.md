<properties
   pageTitle="Cruiser 和计费 API 集成 Microsoft Azure 云 |Microsoft Azure"
   description="从他们的体验将 Azure 计费 Api 集成到他们的产品上的 Microsoft Azure 计费伙伴云 Cruiser 提供独特的观点。  这是特别有用的 Azure 和云 Cruiser 感兴趣使用/尝试云 Cruiser Microsoft Azure 包的客户。"
   services=""
   documentationCenter=""
   authors="BryanLa"
   manager="mbaldwin"
   editor=""
   tags="billing"
   />

<tags
   ms.service="billing"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="billing"
   ms.date="09/08/2016"
   ms.author="mobandyo;sirishap;bryanla"/>

# <a name="cloud-cruiser-and-microsoft-azure-billing-api-integration"></a>Cruiser 和计费 API 集成 Microsoft Azure 云

本文介绍了如何从新的 Microsoft Azure 计费 Api 中收集的信息可用于在云 Cruiser 流成本模拟和分析。

## <a name="azure-ratecard-api"></a>Azure RateCard API
RateCard API 提供从 Azure 率信息。 使用正确的凭据进行身份验证之后, 您可以查询 API 来在 Azure 上, 收集有关可用的服务元数据，以及汇率与您提供的 id。

下面是显示价格为 A0 的 API 的示例响应 (Windows) 实例︰

    {
        "MeterId": "0e59ad56-03e5-4c3d-90d4-6670874d7e29",
        "MeterName": "Compute Hours",
        "MeterCategory": "Virtual Machines",
        "MeterSubCategory": "A0 VM (Windows)",
        "Unit": "Hours",
        "MeterRates":
        {
            "0": 0.029
        },
        "EffectiveDate": "2014-08-01T00:00:00Z",
        "IncludedQuantity": 0.0,
        "MeterStatus": "Active"
    },

### <a name="cloud-cruisers-interface-to-azure-ratecard-api"></a>云对 Azure RateCard API Cruiser 的接口
以不同的方式，云 Cruiser 可以利用 RateCard API 的信息。 对于本文，我们将展示如何使用它以 IaaS 成本模拟和分析的工作负荷。

为证明这一使用情形，假设运行 Microsoft Azure 包 (WAP) 上的多个实例的工作负荷。 目的是模拟在 Azure，此同一负载并估计做这种迁移的成本。 若要创建此模拟，有两个要执行的主要任务︰

1. **导入和处理收集从 RateCard API 的服务信息。** 工作簿，其中转换从 RateCard API 提取并发布到新的费用计划也执行此任务。 此新的费用计划将使用上模拟来估计 Azure 的价格。

2. **WAP 服务和 Azure 服务 IaaS 的标准化。** 默认情况下，基于 WAP 服务在 Azure 时各资源 （CPU、 内存大小、 磁盘大小等） 上服务基于实例的大小 （A0、 A1、 A2 等）。 可以由云 Cruiser ETL 引擎执行、 调用工作簿，可以在实例大小，类似于 Azure 实例服务捆绑这些资源这第一项任务。

### <a name="import-data-from-the-ratecard-api"></a>从 RateCard API 导入数据

云中 Cruiser 工作簿提供自动化的方式来收集和处理来自 RateCard API 的信息。  ETL （提取转换加载） 的工作簿，可以配置集合、 转换和数据发布到云 Cruiser 数据库。

每个工作簿可以包含一个或多个收藏集，使您可以将来自不同的来源，以补充或加强使用数据的信息关联起来。 下面的两个屏幕快照显示了如何在现有的工作簿，然后从 RateCard API*集合*导入信息创建一个新的*集合*︰

![图 1-创建新收藏][1]

![图 2-从新集合导入数据][2]

后向工作簿中导入数据，则可以创建多个步骤和转换过程，修改和模型数据。 此示例中，因为我们只是想基础结构作为-服务 (IaaS)，我们可以使用转换步骤来删除不需要的行或记录，相关服务 IaaS 之外。

下面的屏幕快照显示了用于处理 RateCard API 从收集的数据的转换步骤︰

![图 3-处理收集的数据，从 RateCard API 的转换步骤][3]

### <a name="defining-new-services-and-rate-plans"></a>定义新的服务和速度计划

有不同方法可以在云 Cruiser 定义服务。 另一种是从使用数据导入服务。 使用公共云，服务提供程序的已定义位置时通常使用此方法。

频率规划是一组的费率或价格，可应用于不同的服务，根据生效日期、 或一组客户，除了其他一些选项。 费率计划还可上云 Cruiser 创建模拟或"假设"方案中，若要了解如何服务中的更改可能会影响负载的总成本。

在此示例中，我们将使用来自 RateCard API 的服务信息云 Cruiser 在定义新的服务。 以相同的方式，我们可以使用与服务相关联的速率上云 Cruiser 创建新费率计划。

在转换过程结束时，就可以创建一个新的步骤，并发布 RateCard API 中的数据作为新的服务和速度。

![图 4-发布新服务和速率为 RateCard API 的数据][4]

### <a name="verify-azure-services-and-rates"></a>验证 Azure 服务和费率

在发布后的服务和速率，可以验证导入服务云 Cruiser*服务*选项卡中的列表︰

![图 5-验证新服务][5]

在*费率计划*选项卡上，您可以检查新费率计划名为"AzureSimulation"与汇率从 RateCard API 导入。

![图 6-验证新费率计划和相关联的速率][6]

### <a name="normalize-wap-and-azure-services"></a>WAP 和 Azure 服务规范化

默认情况下，WAP 提供使用基于的计算、 内存和网络资源的使用的信息。 在云 Cruiser，您可以定义基于服务直接对分配或计量使用这些资源的情况。 例如，可以设置 CPU 使用率每小时基本速率或充电 GB 的内存分配给实例。

在本例中，为了比较成本之间 WAP 和 Azure，我们需要聚合 WAP 上成包，然后可以映射到 Azure 服务的资源使用。 在工作簿中可以轻松地实现这种转换︰

![图 7-转换 WAP 数据规范化服务][7]

在该工作簿的最后一步是将数据发布到云 Cruiser 数据库。 在此步骤中，使用数据现在捆绑到服务 （即映射到 Azure 服务） 并绑定到默认费率来创建杂项费用。

工作簿后，您可以通过添加任务计划程序上，并指定的频率和时间运行该工作簿自动化数据的处理。

![图 8-计划编制工作簿][8]

### <a name="create-reports-for-workload-cost-simulation-analysis"></a>创建报表的工作负荷成本的模拟分析

收集使用情况和费用都将加载到云 Cruiser 数据库之后，我们可以利用云 Cruiser 见解模块创建成本模拟，我们所希望的工作负荷。

为了演示这种情况下，我们创建了以下报告︰

![成本的比较][9]

上面的图表显示成本比较的服务、 比较价格的 WAP （深蓝色） 和 Azure （浅蓝色） 之间运行每个特定服务的工作量。

下图显示了相同的数据，但按部门细分。 此图显示为每个部门储蓄栏 （绿色） 二者之间的差异以及 WAP 和 Azure，运行它们的工作量的成本。

## <a name="azure-usage-api"></a>Azure 使用 API


### <a name="introduction"></a>介绍

Microsoft 最近推出 Azure 使用 API，允许订阅者深入了解其消耗的使用率数据中以编程方式拉。 这是一个好消息，可以充分利用更丰富的数据集可通过此 API 的云 Cruiser 客户。

云中 Cruiser 可以利用以下几种方式使用 api 集成。 粒度 （每小时使用情况的信息），并通过 API 可用资源的元数据信息提供了必要的数据集，以支持灵活的 Showback 或按容量使用计费模型。 

在本教程中，我们将介绍如何云 Cruiser 可以受益于使用 API 信息的一个例子。 更具体地说，我们将 Azure 上创建资源组，将相关联的科目结构中，标记然后描述拉和处理到云 Cruiser 标记信息的过程。
 
最终的目标是能够创建类似下面的报表并能够分析成本和消耗量根据科目结构中填充的标记。

![图 10-使用标记的损害与报告][10]

### <a name="microsoft-azure-tags"></a>Microsoft Azure 标记

可通过 Azure 使用 API 的数据包括消耗的信息，不仅包括与之相关的任何标记的资源元数据。 标记提供了一种简便的方法来组织您的资源，但若要有效，需要确保︰

- 标记将正确应用到资源调配时
- 标记正确使用 Showback/计费过程，将为组织的科目结构的使用情况。

这两个要求可以是富有挑战性的特别是在没有提供或收取一侧上的手动过程。 键入错误、 不正确或甚至缺少标记时会使用标记和这些错误可以使充电一边生活更难以常见来自客户的投诉。

与新的 Azure 使用 API，云 Cruiser 可以提取资源标记的信息，并通过完善的 ETL 工具，称为工作簿，修复这些常见的标记错误。 通过使用正则表达式和数据关联的转换，云 Cruiser 可以标识错误地标记的资源并应用了正确的标记，确保使用者的正确关联的资源。

在收费方面，云 Cruiser 自动化 Showback/计费过程，而且可以利用标记信息以绑定到适当的使用者 （部门、 分公司、 项目等） 的使用情况。 这种自动化提供了一个巨大的改进，可以确保一致的和可核查的充电过程。
 

### <a name="creating-a-resource-group-with-tags-on-microsoft-azure"></a>使用 Microsoft Azure 上标记创建资源组
在本教程中的第一步是要创建资源组，在 Azure 的门户中，然后创建新的标记关联的资源。 对于本示例，我们将创建下列标记︰ 项目的部门，环境中，所有者。

下面的屏幕截图显示示例资源组相关联的标记。

![图 11-与关联的标记在 Azure 的门户网站上的资源组][11]

下一步是将信息从使用 API 拉入云 Cruiser。 目前，使用 API 提供以 JSON 格式的响应。 下面是一个示例检索到的数据︰


    {
      "id": "/subscriptions/bb678b04-0e48-4b44-XXXX-XXXXXXX/providers/Microsoft.Commerce/UsageAggregates/Daily_BRSDT_20150623_0000",
      "name": "Daily_BRSDT_20150623_0000",
      "type": "Microsoft.Commerce/UsageAggregate",
      "properties":
      {
        "subscriptionId": "bb678b04-0e48-4b44-XXXX-XXXXXXXXX",
        "usageStartTime": "2015-06-22T00:00:00+00:00",
        "usageEndTime": "2015-06-23T00:00:00+00:00",
        "meterName": "Compute Hours",
        "meterRegion": "",
        "meterCategory": "Virtual Machines",
        "meterSubCategory": "Standard_D1 VM (Non-Windows)",
        "unit": "Hours",
        "instanceData": "{\"Microsoft.Resources\":{\"resourceUri\":\"/subscriptions/bb678b04-0e48-4b44-XXXX-XXXXXXXX/resourceGroups/DEMOUSAGEAPI/providers/Microsoft.Compute/virtualMachines/MyDockerVM\",\"location\":\"eastus\",\"tags\":{\"Department\":\"Sales\",\"Project\":\"Demo Usage API\",\"Environment\":\"Test\",\"Owner\":\"RSE\"},\"additionalInfo\":{\"ImageType\":\"Canonical\",\"ServiceType\":\"Standard_D1\"}}}",
        "meterId": "e60caf26-9ba0-413d-a422-6141f58081d6",
        "infoFields": {},
        "quantity": 8

      },
    },


### <a name="import-data-from-the-usage-api-into-cloud-cruiser"></a>将数据从使用 API 导入云 Cruiser

云中 Cruiser 工作簿提供自动化的方式来收集和处理来自使用 API 的信息。 工作簿的 ETL （提取转换加载） 允许您配置集合、 转换和数据发布到云 Cruiser 数据库。

每个工作簿可以包含一个或多个集合。 这使您可以将来自不同的来源，以补充或加强使用数据的信息关联起来。 对于此示例，我们将在碧空模板的工作簿中创建新工作表 (_UsageAPI)_ ，并设置一个新的_集合_信息导入中使用的 API。

![图 3-使用 API 数据导入到 UsageAPI 表][12]

请注意，此工作簿已有其他工作表以从导入服务 Azure (_ImportServices_)，并处理来自付费 API (_PublishData_) 的消耗信息。

下一步我们将使用使用 API 来填充_UsageAPI_表中，并关联_PublishData_表计费 API 的消耗数据的信息。

### <a name="processing-the-tag-information-from-the-usage-api"></a>处理中使用 API 的标记信息

后向工作簿中导入数据，我们将转换步骤在中创建_UsageAPI_表以处理 API 的信息。 第一步是使用"JSON 拆分"的处理器来提取标记，从单个字段中，然后创建它们 （部门、 项目、 所有者和环境） 中的每个字段。

![图 4-创建新的标记信息字段][13]

请注意"网络连接"服务缺少标记信息 （黄色框），但我们可以验证它通过_ResourceGroupName_字段中查看同一个资源组的一部分。 因为我们拥有此资源组中的其他资源的标签，我们可以使用此信息将丢失的标记应用到此资源时在进程的后期。

下一步是创建一个查找表关联的标记为_ResourceGroupName_的信息。 下一步将用此查阅表格来丰富消耗数据和标记的信息。

### <a name="adding-the-tag-information-to-the-consumption-data"></a>标记将信息添加到消耗数据

现在我们可以跳转到_PublishData_表中，负责处理从计费 API 消耗的信息，并添加提取的标记的字段。 此过程被通过查看在上一步，使用_ResourceGroupName_作为查找的键上创建查找表。

![图 5-填充的科目结构中查找信息][14]

注意应用了适当的科目结构的字段为"网络连接"服务，与缺少标记一起解决问题。 我们还填充资源帐户结构域之外的"其他"，我们目标资源组以便区分它们在报表上。

现在我们只需添加一个步骤以发布的使用率数据。 在此步骤中，在我们率计划中定义每个服务的适当费率将应用于使用信息，与加载到数据库中产生的费用。

最妙的是，您只需一次经历这一过程。 完成该工作簿后，您只需将它添加到计划程序，它将在计划的时间运行每小时或每日。 然后就只需创建新的报表，或定制现有的才可以分析的数据从云的使用中获得有意义的见解。

### <a name="next-steps"></a>下一步行动

+ 有关创建云 Cruiser 工作簿和报告的详细说明，请参阅云 Cruiser 联机[文档](http://docs.cloudcruiser.com/)（所需的有效登录）。  有关云 Cruiser 的详细信息，请联系[info@cloudcruiser.com](mailto:info@cloudcruiser.com)。
+ Azure 资源使用情况和 RateCard Api 的概述，请参阅[深入了解您的 Microsoft Azure 资源消耗](billing-usage-rate-card-overview.md)。
+ 检查出更多有关这两个是 Azure 资源管理器提供的 Api 集的一部分的 Api [Azure 计费 REST API 参考](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c)。
+ 如果您想要讲解的示例代码，签出我们 Microsoft Azure 计费 API 代码示例在[Azure 代码示例](https://azure.microsoft.com/documentation/samples/?term=billing)。

### <a name="learn-more"></a>了解更多信息
+ 请参见[Azure 资源管理器概述](azure-resource-manager/resource-group-overview.md)文章了解更多关于 Azure 资源管理器。

<!--Image references-->
 
[1]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Create-New-Workbook-Collection.png "图 1-创建新收藏"
[2]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Import-Data-From-RateCard.png "图 2-从新集合导入数据"
[3]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transformation-Steps-Process-RateCard-Data.png "图 3-处理收集的数据，从 RateCard API 的转换步骤"
[4]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Publish-RateCard-Data-New-Services-Rates.png "图 4-发布新服务和速率为 RateCard API 的数据"
[5]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing1.png "图 5-验证新服务"
[6]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing2.png "图 6-验证新费率计划和相关联的速率"
[7]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transforming-WAP-Normalize-Services.png "图 7-转换 WAP 数据规范化服务"
[8]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workbook-Scheduling.png "图 8-计划编制工作簿"
[9]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workload-Cost-Simulation-Report.png "图 9-负荷成本比较方案的示例报表"
[10]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/1_ReportWithTags.png "图 10-使用标记的损害与报告"
[11]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/2_ResourceGroupsWithTags.png "图 11-与关联的标记在 Azure 的门户网站上的资源组"
[12]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/3_ImportIntoUsageAPISheet.png "图 12-使用 API 数据导入到 UsageAPI 表"
[13]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/4_NewTagField.png "图 13-创建新的标记信息字段"
[14]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/5_PopulateAccountStructure.png "图 14-填充的科目结构中查找信息"
