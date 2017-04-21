<properties
   pageTitle="Azure 数据目录的常见问题 |Microsoft Azure"
   description="有关常见问题 Azure 数据目录，包括数据源发现、 批注和管理的能力。"
   services="data-catalog"
   documentationCenter=""
   authors="steelanddata"
   manager="NA"
   editor=""
   tags=""/>
<tags
   ms.service="data-catalog"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-catalog"
   ms.date="10/04/2016"
   ms.author="maroche"/>

# <a name="azure-data-catalog-frequently-asked-questions"></a>Azure 数据目录︰ 常见问题

本文提供与 Microsoft **Azure 数据目录**服务相关的常见问题的答案。

## <a name="q-what-is-azure-data-catalog"></a>问︰ 什么是 Azure 数据目录？

答︰ Microsoft Azure 数据目录是查询的在作为注册的系统和系统对于企业数据源的 Microsoft Azure 云托管的完全托管的服务。 Azure 数据目录提供功能，使任何用户 – 数据科学家向开发人员 – 若要注册，向分析师发现、 了解和使用的数据源。

## <a name="q-what-customer-challenges-does-azure-data-catalog-solve"></a>问︰ 哪些客户难题 does Azure 数据目录后是否解决了？

Azure 数据目录通过允许用户发现并理解企业数据源地址数据源查询和"黑数据"的挑战。

## <a name="q-who-are-the-target-audiences-for-azure-data-catalog"></a>问︰ 谁是 Azure 数据目录的目标群体？

Azure 数据目录为技术性和非技术性用户提供，包括︰

- 数据开发、 BI 和分析专家︰ 谁是负责生产供其他人使用的数据和分析内容
-   数据挺身而出︰ 那些拥有知识有关的数据、 其含义和它打算如何使用和用于哪些用途
- 数据使用者︰ 那些需要能够轻松地发现、 了解和连接到的数据所需执行其工作时使用他们选择的工具
- 中央 IT︰ 那些谁需要进行几百个数据源对于业务用户来说，发现和那些需要维护的监管，对数据的使用方式以及由谁

## <a name="q-what-is-the-azure-data-catalog-region-availability"></a>问︰ 什么是 Azure 数据目录区域可用性？

在下面的数据中心是目前 azure 数据目录服务︰

- 美国西部
- 东亚的美国
- 西欧
- 北欧
- 澳大利亚东部
- 东南亚

## <a name="q-what-are-the-limits-on-the-number-of-data-assets-in-azure-data-catalog"></a>问︰ 什么是 Azure 数据目录中的数据资产的数量上的限制？

免费版的 Azure 数据目录仅限于 5000 注册的数据资产。

标准版的 Azure 数据目录支持达 100000 个已注册的数据资产。

## <a name="q-what-are-the-supported-data-source-and-asset-types"></a>问︰ 什么是受支持的数据源和资产类型？

请参阅[数据目录 DSR](data-catalog-dsr.md)的当前支持的数据源的列表。

## <a name="q-how-do-i-request-support-for-another-data-source"></a>问︰ 如何请求支持的另一个数据源？

可以在[Azure 数据目录论坛](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409)提交功能请求及其他反馈信息。

## <a name="q-how-do-i-get-started-with-azure-data-catalog"></a>问︰ 我如何开始使用 Azure 数据目录？

入门的最佳位置是按照[数据目录入门指南](data-catalog-get-started.md)中的说明进行操作。 这篇文章是端到端服务中的功能概述。

## <a name="q-how-do-i-register-my-data"></a>问︰ 如何注册我的数据？

若要在 Azure 数据目录中注册您的数据，启动从 Azure 数据目录入口的"发布"区域中的 Azure 数据目录注册工具。 在 Azure 数据目录发布应用程序时，使用相同的凭据用于访问 Azure 数据目录门户网站，登录，然后选择数据源和您想要注册的特定资产。

## <a name="q-what-properties-are-extracted-for-data-assets-that-are-registered"></a>问︰ 哪些属性提取数据资产注册？

特定属性将不同于数据源到数据源，但一般来说 Azure 数据目录发布服务将提取的以下信息︰

- 资产名称
- 资产类型
- 资产说明
- 属性/列名称
- 属性/列的数据类型
- 属性/列说明

> [AZURE.IMPORTANT] Azure 数据目录中注册的数据资产不移动或将数据复制到云中。 注册数据源中的资产将资源的元数据复制到 Azure，但数据仍保留在现有数据源的位置。 此规则唯一的例外是如果用户选择要上载预览的记录或数据配置文件注册资产时。 当包括预览，最多 20 个记录将从每个资产，复制和存储为 Azure 数据目录中的某个快照。 当包括数据配置文件，将计算并包含在目录中存储的元数据聚合信息 （例如表、 百分比每列中的空值和列的最小值、 最大和平均值的大小）。

<br/>

> [AZURE.NOTE] 为数据源，如有第一类的**说明**属性的 SQL Server Analysis Services，Azure 数据目录发布的应用程序将提取的属性值。 对于 SQL Server 关系数据库，没有一类的**说明**属性，Azure 数据目录发布应用程序会从扩展属性的对象和列 ms_description 提取的值。 有关详细信息，请参阅 TechNet[使用对数据库对象的扩展属性](https://technet.microsoft.com/library/ms190243%28v=sql.105%29.aspx)。

## <a name="q-how-long-should-it-take-for-newly-registered-assets-to-appear-in-azure-data-catalog"></a>问︰ 需要多长时间应其对于新注册资产出现在 Azure 数据目录？

您使用 Azure 数据目录中注册资产后可能有 5-10 秒钟之前它们出现在 Azure 数据目录入口的一段。

## <a name="q-how-do-i-annotate-and-enrich-the-metadata-for-my-registered-data-assets"></a>问︰ 如何添加批注以及丰富的我已注册的数据资产元数据？

注册资产为提供元数据的最简单方法是 Azure 数据目录入口中选择资源，然后为所选对象架构窗格中的属性窗格中输入元数据值。

您还可以在注册过程中提供专家和标记，等一些元数据。 在 Azure 数据目录发布服务中提供的值将应用于当时被注册的所有资产。 若要附加批注门户中查看最近注册对象，选择 Azure 数据目录发布应用程序的最后一个屏幕上的**门户视图**按钮。

## <a name="q-how-do-i-delete-my-registered-data-objects"></a>问︰ 如何删除已注册的数据对象？

在门户中，选择该对象，然后单击**删除**按钮，可以从 Azure 数据目录中删除对象。 这将从 Azure 数据目录中删除该对象的元数据，但不是会影响实际的基础数据源。

## <a name="q-what-is-an-expert"></a>问︰ 什么是专家？

一位专家是有明智的角度看，有关数据对象的人。 一个对象可以有多个专家。 一位专家不需要是"所有者"的对象;专家就是知道数据如何，应使用的人。

## <a name="q-how-do-i-share-information-with-the-azure-data-catalog-team-if-i-encounter-problems"></a>问︰ 如何共享信息与 Azure 数据目录团队如果遇到了问题？

请使用 Azure 数据目录论坛报告问题、 共享信息，并询问问题。 论坛可以在 http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409 上找到

##<a name="q-does-azure-data-catalog-work-with-this-other-data-source-im-interested-in"></a>问︰ Azure 数据目录处理此我感兴趣的其他数据源？
我们积极合作，将更多数据源添加到 Azure 数据目录。 如果您想要看到支持，请建议它 （或语音支持，如果已建议）[论坛 Azure 数据目录](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409)中的数据源。

## <a name="q-how-is-azure-data-catalog-related-to-the-data-catalog-in-power-bi-for-office-365"></a>问︰ 如何是 Azure 数据目录相关的数据目录中电源 BI Office 365 的？

可以 Azure 数据目录看作演变而来的数据目录。 Azure 数据目录提供类似的功能，为数据源发布和发现，而是致力于更广泛的方案并不取决于 Office 365。 不久 Azure 数据目录正式上市后两个目录将合并为单个服务中。

## <a name="q-what-permissions-does-a-user-need-to-register-assets-with-azure-data-catalog"></a>问︰ 用户需要注册 Azure 数据目录资产哪些权限？

运行的 Azure 数据目录注册工具的用户需要将允许他从源中读取元数据对数据源的权限。 如果用户选择还包括预览，用户必须允许他阅读中注册的对象的数据的权限。

## <a name="q-will-azure-data-catalog-be-made-available-for-on-premises-deployment-as-well"></a>问︰ 将 Azure 数据目录供以及内部部署？

Azure 数据目录是云以及内部数据源，可以使用云服务提供了一个混合数据源发现解决方案。 目前没有计划将运行内部 Azure 数据目录服务的版本。

##<a name="q-can-we-extract-more--richer-metadata-from-the-data-sources-we-register"></a>问︰ 我们可以从我们注册的数据源中提取更多 / 更丰富的元数据？

我们正在积极努力扩展了 Azure 数据目录的功能。 如果您想要查看提取数据源的注册过程中的附加元数据，请建议它 （或它已建议如果投票），在[Azure 数据目录论坛](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409)。 将来，我们将允许第三方可以添加新的数据源类型，通过可扩展性 API。

## <a name="q-how-do-i-restrict-the-visibility-of-registered-data-assets-so-that-only-certain-people-can-discover-them"></a>问︰ 如何限制，以便仅允许特定用户可以发现它们的已注册的数据资产，可见性？

答︰ 选择数据资产在 Azure 数据目录中，然后单击"取得所有权"按钮。 在 Azure 数据目录中的数据资产的所有者可更改的可见性设置，以允许所有目录用户发现拥有的资产，或者向特定用户限制可视性。

## <a name="q-how-do-i-update-the-registration-for-a-data-asset-to-that-changes-in-the-data-source-are-reflected-in-the-catalog"></a>问如何更新数据资产的注册数据源的变化反映在目录吗？

答︰ 对更新的元数据已在目录中注册的数据资产，只需重新注册数据源包含资产。 任何数据源，如列被添加或删除表或视图，将更新这些更改在编录中，但用户提供的任何批注将保持不变。

## <a name="q-my-question-isnt-answered-here--what-should-i-do"></a>问︰ 我没有解答的问题 – 我该怎么办？

通过在[Azure 数据目录论坛](http://go.microsoft.com/fwlink/?LinkID=616424&clcid=0x409)上头。 那里的提出将发现他们这样做的方法。
