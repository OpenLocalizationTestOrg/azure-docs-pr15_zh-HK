<properties 
    pageTitle="在应用程序的见解中的应用程序映射 |Microsoft Azure" 
    description="用 Kpi 和警报标记应用程序组件之间的依赖关系的可视化表示形式。" 
    services="application-insights" 
    documentationCenter=""
    authors="SoubhagyaDash" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="06/15/2016" 
    ms.author="awills"/>
 
# <a name="application-map-in-application-insights"></a>在应用程序的见解中的应用程序映射

在[Visual Studio 应用程序理解](app-insights-overview.md)、 应用程序映射是将依赖关系的应用程序组件的可视布局。 每个组件显示 Kpi，如负载、 性能、 故障和警报，以帮助您发现任何组件导致的性能问题或故障。 您可以单击通过任何组件的更详细的诊断，从应用程序的见解，同时，如果您的应用程序使用 Azure 服务-Azure 的诊断程序，例如 SQL 数据库顾问建议。

像其他图表，您可以锁定到 Azure 仪表板，它是完全正常的应用程序映射。 

## <a name="open-the-application-map"></a>打开应用程序映射

从概述刀片式服务器应用程序中打开图︰

![打开应用程序映射](./media/app-insights-app-map/01.png)

![应用程序映射](./media/app-insights-app-map/02.png)

地图显示了︰

* 可用性测试
* 客户端组件 （JavaScript sdk 监视）
* 服务器端组件
* 客户端和服务器组件的依赖项

您可以展开和折叠依赖项链接组︰

![折叠](./media/app-insights-app-map/03.png)
 
如果您有大量的 （SQL，HTTP 等） 的一种类型的依赖关系，它们可能会显示分组。 


![组合的相关性](./media/app-insights-app-map/03-2.png)
 
 
## <a name="spot-problems"></a>发现问题

每个节点都有相关的性能指标，如该组件的负载、 性能和故障率。 

警告图标突出显示可能出现的问题。 橙色的警告表示页面视图或依赖项调用请求中有出现故障。 红色意味着失败率超过 5%。


![故障图标](./media/app-insights-app-map/04.png)

 
活动还警报上显示︰ 


![活动警报](./media/app-insights-app-map/05.png)
 
如果您使用 SQL Azure，显示了一个图标，则是有关于如何提高性能的建议。 


![Azure 的建议](./media/app-insights-app-map/06.png)

单击任何图标以获取更多详细信息︰


![azure 的建议](./media/app-insights-app-map/07.png)
 
 
## <a name="diagnostic-click-through"></a>诊断通过单击

每个图上的节点提供目标逐个单击诊断。 选项会有所不同，具体取决于节点的类型。

![服务器选项](./media/app-insights-app-map/09.png)

 
对于承载 Azure 中的组件，选项包括直接链接到它们。


## <a name="filters-and-time-range"></a>筛选器和时间范围

默认情况下，图概括了所有可供选择的时间范围的数据。 但是，您可以筛选以仅包括特定操作名称或依赖项。

* 操作名称︰ 这包括页面视图和服务器端请求类型。 使用此选项，图上所选工序的服务器/客户机端节点显示 KPI。 它显示了调用特定操作的上下文中的依赖项。
* 依赖项的基名称︰ 这包括 AJAX 浏览器端相关性和服务器端相关性。 如果您报告自定义依赖项遥测与 TrackDependency API，它们还会显示在此处。 您可以选择要显示在地图上的依赖项。 请注意，这一次，这将不会筛选服务器端请求或客户端页面视图。


![设置筛选器](./media/app-insights-app-map/11.png)

 
 
## <a name="save-filters"></a>保存筛选器

若要保存您已应用的筛选器，固定到[仪表板](app-insights-dashboards.md)上的筛选的视图。


![附到仪表板](./media/app-insights-app-map/12.png)
 


## <a name="feedback"></a>反馈

请[提供反馈通过门户反馈选项](app-insights-get-dev-support.md)。


![MapLink-1 图像](./media/app-insights-app-map/13.png)


