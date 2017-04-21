<properties
    pageTitle="接收警报通知的 Azure 服务 |Microsoft Azure"
    description="在符合预警规则条件时通知。"
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

# <a name="receive-alert-notifications"></a>接收通知

可以接收基于监控指标或上，Azure 服务的事件的通知。

为预警规则的度量值，指定度量值超过阈值，当预警规则变为活动状态，可以发送通知。 有关警报的事件规则，规则可以在*每一个*事件，或者，只有一定数量的事件发生时发送通知。

创建预警规则时，您可以选择选项以将电子邮件通知发送给服务管理员和共同的管理员或另一个系统管理员，您可以指定。 当规则变为活动状态，并解决警报条件时发送通知电子邮件。

您可以使用[REST API](https://msdn.microsoft.com/library/azure/dn931945.aspx)或[.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/)配置和以编程方式获取有关预警规则的信息。

## <a name="create-an-alert-rule"></a>创建预警规则

1. 在[门户网站](https://portal.azure.com/)中，单击**浏览**，然后您感兴趣监视的资源。

2. 单击**操作**镜头中的**预警规则**平铺。

3. 单击**添加通知**命令。

    ![添加通知](./media/insights-receive-alert-notifications/Insights_AddAlert.png)

4. 可以命名您的预警规则，并选择将显示在通知电子邮件的说明。

5. 选择**度量值**时您将选择条件和规格阈值的值。 这是时间的 Azure 使用监视器和绘图警报活动段。

    ![条件和阈值](./media/insights-receive-alert-notifications/Insights_ConditionAndThreshold.png)

6. 此外可以选择**事件**，然后在特定事件发生时收到通知。

    ![事件](./media/insights-receive-alert-notifications/Insights_Events.png)

7. 最后，您可以选择将电子邮件通知发送给管理员负责。

单击**保存**后，在几分钟内您都会明智选择跃点数超过阈值。

## <a name="managing-your-alert-rules"></a>管理您的预警规则

一旦您已经创建预警规则，您可以查看预览您的通知阈值相比前一天的跃点数。

![事件](./media/insights-receive-alert-notifications/Insights_EditAlert.png)


当然，您可以编辑此预警规则，并**停**用或**启用**它如果要暂时停止接收通知它有关。

## <a name="next-steps"></a>下一步行动

* 对各种渠道的路由通知[配置 webhooks 在您的通知](insights-webhooks-alerts.md)
* [监视服务标准](insights-how-to-customize-monitoring.md)，以确保您的服务是可用的并且能够作出响应。
* [启用监视和诊断](insights-how-to-use-diagnostics.md)服务上收集详细的高频指标。
* [显示器的可用性和响应能力的任何 web 页](../application-insights/app-insights-monitor-web-app-availability.md)与应用程序使您可以找出如果您的页面的见解是向下。
* [监视应用程序性能](../application-insights/app-insights-azure-web-apps.md)如果想要了解完全代码性能如何在云中。
* [查看事件并审核日志](insights-debugging-with-events.md)以了解一切服务中发生了什么。
* [跟踪服务运行状况](insights-service-health.md)，以查明 Azure 出现性能下降或服务中断。
