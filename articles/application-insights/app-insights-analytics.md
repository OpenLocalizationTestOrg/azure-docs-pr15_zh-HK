<properties 
    pageTitle="分析的应用程序理解的功能强大的搜索工具 |Microsoft Azure" 
    description="分析应用程序的见解的诊断功能强大的搜索工具的概述。 " 
    services="application-insights" 
    documentationCenter=""
    authors="alancameronwills" 
    manager="douge"/>

<tags 
    ms.service="application-insights" 
    ms.workload="tbd" 
    ms.tgt_pltfrm="ibiza" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="07/26/2016" 
    ms.author="awills"/>


# <a name="analytics-in-application-insights"></a>在应用程序的见解的分析


[分析](app-insights-analytics.md)是一种功能强大的搜索功能的[应用程序的见解](app-insights-overview.md)。 这些页面描述分析查询 lanquage。 

* **[观看介绍性视频](https://applicationanalytics-media.azureedge.net/home_page_video.mp4)**。
* **[测试驱动器在模拟数据的分析](https://analytics.applicationinsights.io/demo)**如果您的应用程序不会将数据发送到应用程序的见解尚。

## <a name="queries-in-analytics"></a>在分析中的查询
 
典型的查询是*跟*运算符*分隔的一系列源表* `|`。 

例如，让我们查明的当天 Hyderabad 市民尝试我们的 web 应用程序的什么时候。 并且，尽管我们存在，让我们看什么结果代码将返回到它们的 HTTP 请求。 

```AIQL

    requests      // Table of events that log HTTP requests.
  	| where timestamp > ago(7d) and client_City == "Hyderabad"
  	| summarize clients = dcount(client_IP) 
      by tod_UTC=bin(timestamp % 1d, 1h), resultCode
  	| extend local_hour = (tod_UTC + 5h + 30min) % 24h + datetime("2001-01-01") 
```

我们统计不同客户端的 IP 地址，过去 7 天内的一天中的小时对它们进行分组。 

让我们显示条形图演示文稿，选择堆栈的不同响应代码结果的结果︰

![选择条形图，x 和 y 轴，然后细分](./media/app-insights-analytics/020.png)

看起来我们的应用程序是最常用在午餐时间和在 Hyderabad 的就寝时间。 （和我们应研究那些 500 代码）。


还有强大的统计运算︰

![](./media/app-insights-analytics/025.png)


语言有许多吸引人的功能︰

* [筛选器](app-insights-analytics-reference.md#where-operator)的任何字段，其中包括您的自定义属性和度量您原始应用程序遥测。
* [联接](app-insights-analytics-reference.md#join-operator)多个表--使用网页视图、 依赖项调用、 异常和日志关联请求跟踪。
* 强大的统计[聚合](app-insights-analytics-reference.md#aggregations)。
* 为 SQL，同样有力，但对于复杂的查询更容易︰ 而不是嵌套语句，您传送到下一个基本操作的数据。
* 快速且功能强大的可视化功能。







## <a name="connect-to-your-application-insights-data"></a>连接到您的应用程序理解数据


从您的应用程序[概述刀片式服务器](app-insights-dashboards.md)应用程序的见解中打开分析︰ 

![打开 portal.azure.com，打开您的应用程序理解资源，然后单击分析。](./media/app-insights-analytics/001.png)


## <a name="limits"></a>限制

目前，查询结果被限制为只保留过去数据一周以上。



[AZURE.INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]


## <a name="next-steps"></a>下一步行动


* 我们建议您与[语言教程](app-insights-analytics-tour.md)开始。