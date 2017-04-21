<properties
    pageTitle="概述在 Microsoft Azure 中的度量标准 |Microsoft Azure"
    description="了解如何自定义监视 Azure 中的图表。"
    authors="rboucher"
    manager="carolz"
    editor=""
    services="monitoring-and-diagnostics"
    documentationCenter="monitoring-and-diagnostics"/>

<tags
    ms.service="monitoring-and-diagnostics"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/08/2015"
    ms.author="robb"/>

# <a name="overview-of-metrics-in-microsoft-azure"></a>在 Microsoft Azure 中的测量数据的概览

所有的 Azure 服务跟踪允许您监视运行状况、 性能、 可用性和服务的使用情况的关键指标。 在 Azure 的门户中，可以查看这些度量标准，您也可用[REST API](https://msdn.microsoft.com/library/azure/dn931930.aspx)或[.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/)以编程方式访问的整套指标。

对于某些服务，可能需要打开诊断才能看到任何指标。 其他人，如虚拟机，您将得到一套基本的指标，但需要启用完整设置高频指标。 请参阅[启用监视和诊断信息](insights-how-to-use-diagnostics.md)以了解详细信息。

## <a name="using-monitoring-charts"></a>使用监视图表

您可以插入到任何度量它们选择任何时间段。

1. 在[Azure 门户](https://portal.azure.com/)中，**浏览**，请单击，然后单击您感兴趣监视的资源。

2. **监控**部分包含 Azure 的每个资源的最重要指标。 例如，web 应用程序具有**请求和错误**，其中虚拟机应该有**CPU 百分比**和**磁盘读取和写入**︰ ![监视镜头](./media/insights-how-to-customize-monitoring/Insights_MonitoringChart.png)

3. 单击任何图表上将显示**度量**刀片式服务器。 刀片式服务器，除了图中，是向您显示 （如平均值、 最小和最大值，选择时间范围内） 的度量值的聚合表。 下面是对资源的预警规则。
    ![公制刀片式服务器](./media/insights-how-to-customize-monitoring/Insights_MetricBlade.png)

4. 若要自定义显示的行，请单击图表上的**编辑**按钮或公制刀片式服务器上的**编辑图表**命令。

5. 编辑查询刀片式服务器上可以执行三种操作︰
    - 更改的时间范围
    - 交换机之间的外观栏与线
    - 选择不同的 metics![编辑查询](./media/insights-how-to-customize-monitoring/Insights_EditQuery.png)

6. 更改的时间范围的本息选择不同的范围 （如**过去小时**），然后单击底部的刀片式服务器**保存**。 您也可以选择**自定义**、 可选择任意时间段内，在过去的 2 周内。 例如，您可以看到整个两周，或，昨天只需 1 小时。 键入在文本框中输入不同的小时。
    ![自定义时间范围](./media/insights-how-to-customize-monitoring/Insights_CustomTime.png)

7. 下面的时间范围，您的频道选择任意数量的指标，以显示在图表上。

8. 当您单击保存时将该特定资源的保存您的更改。 例如，如果您有两台虚拟机，并且您更改一个图表，它不会影响其他。

## <a name="creating-side-by-side-charts"></a>创建并排排列图表

具有强大的自定义门户中可以添加所需的任意多个图表。

1. 在**...**菜单顶部的刀片式服务器，请单击**添加平铺**︰  
    ![添加菜单](./media/insights-how-to-customize-monitoring/Insights_AddMenu.png)
2. 然后，您可以选择从右侧屏幕上的**库**中选择图表︰ ![库](./media/insights-how-to-customize-monitoring/Insights_Gallery.png)
3. 如果您看不到所需的跃点数，可以始终添加一种预设的指标，和**编辑**图表以显示所需的跃点数。

## <a name="monitoring-usage-quotas"></a>监视使用配额

大多数指标显示趋势随着时间的推移，但某些数据，如使用配额阈值的时间点信息。

有关具有配额的资源刀片上，您还可以查看使用配额︰

![使用](./media/insights-how-to-customize-monitoring/Insights_UsageChart.png)

像度量标准，您可以使用[REST API](https://msdn.microsoft.com/library/azure/dn931963.aspx)或[.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/)以编程方式访问的完整的一套使用配额。

## <a name="next-steps"></a>下一步行动

* 跃点值超过阈值时，[接收警报通知](insights-receive-alert-notifications.md)。
* [启用监视和诊断](insights-how-to-use-diagnostics.md)服务上收集详细的高频指标。
* [自动缩放实例计数](insights-how-to-scale.md)，以确保您的服务是可用的并且能够作出响应。
* [监视应用程序性能](../application-insights/app-insights-azure-web-apps.md)如果想要了解完全代码性能如何在云中。
* 使用[应用程序理解 JavaScript 应用程序和网页](../application-insights/app-insights-web-track-usage.md)以获取有关访问的网页浏览器客户端分析。
* [显示器的可用性和响应能力的任何 web 页](../application-insights/app-insights-monitor-web-app-availability.md)与应用程序使您可以找出如果您的页面的见解是向下。
