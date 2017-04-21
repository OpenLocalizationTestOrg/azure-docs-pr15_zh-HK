<properties
    pageTitle="查看事件和审核日志"
    description="了解如何查看所有发生在 Azure 订阅的事件。"
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
    ms.date="04/28/2015"
    ms.author="robb"/>

# <a name="view-events-and-audit-logs"></a>查看事件和审核日志

在 Azure 的资源上执行的所有操作完全都审核通过 Azure 资源管理器中创建和删除授予或撤消访问到。 您可以浏览在 Azure 的门户中，这些日志，您可以也使用[REST API](https://msdn.microsoft.com/library/azure/dn931927.aspx)或[.NET SDK](https://www.nuget.org/packages/Microsoft.Azure.Insights/)以编程方式访问完整的事件集。

## <a name="browse-the-events-impacting-your-azure-subscription"></a>浏览事件影响 Azure 订阅

1. 登录到[Azure 的门户](https://portal.azure.com/)。
2. 单击**浏览**，然后选择**审核日志**。  
    ![浏览集线器](./media/insights-debugging-with-events/Insights_Browse.png)
3. 这将打开刀片式服务器显示的所有事件已过去 7 天影响任何您的订阅。 在顶部是一个图，显示按级别，并在下面的数据是完整的日志列表︰ ![的所有事件](./media/insights-debugging-with-events/Insights_AllEvents.png)

>[AZURE.NOTE] 您只能查看 Azure 门户中为给定的订阅 500 的最新事件。

4. 您可以单击任何日志条目来查看由组成的事件。 例如，在给资源组部署的内容时，可能要创建或修改许多不同的资源。 每个条目，您可以查看︰
    * **级别**的事件-例如，它可能是有点跟踪 （**信息**），或者当出了问题，您需要了解 （**错误**）。
    * **状态**-最终状态通常会**成功**或**失败**，但它也可能会**接受**长时间运行的操作。
    * *当*该事件发生。
    * *谁*执行了该操作，如果任何人。 不是所有的操作可以由用户、 一些可以由后端服务，因此他们没有**调用方**。
    * **相关性 ID**的事件-这是此组的操作的唯一标识符。

5. 从那里可以转到详细信息刀片式服务器以查看该事件的具体情况。

    ![资源组](./media/insights-debugging-with-events/Insights_EventDetails.png)

    对于**失败**的事件，此页面通常包含**子状态**和**属性**部分，包括用于调试目的的有用的详细信息。

## <a name="filter-to-specific-logs"></a>到特定的日志筛选

若要查看适用于特定实体，或特定类型的事件，您可以通过单击**筛选**命令筛选审核日志刀片式服务器。 此外可以使用筛选器刀片式服务器更改**的时间跨度**的审核日志刀片式服务器。

一旦单击此命令时，将打开新的刀片式服务器︰

![筛选器](./media/insights-debugging-with-events/Insights_EventFilter.png)

有四种类型的筛选器︰

1. 通过订阅
2. 由**资源组**
3. 由**资源类型**
4. 通过特定的**资源**-这必须过去您感兴趣的资源的完整*资源 ID*

此外，由谁执行了该事件，或者，该事件的级别，还可以筛选事件。

一旦您已经完成选择您想要查看，请单击**更新**按钮底部的刀片式服务器。

## <a name="monitor-events-impacting-specific-resources"></a>监视事件影响特定资源

1. 单击**浏览**以找到您所感兴趣的资源。 您还可以查看所有日志对于整个**资源组**。
2. 在刀片式服务器的资源，向下滚动直到找到**事件**图块。  
    ![事件平铺](./media/insights-debugging-with-events/Insights_EventsTile.png)
3. 单击以查看事件不仅仅是所选的资源筛选该图块。 可以使用**筛选**命令来更改的时间范围或应用更特定的筛选器。

## <a name="next-steps"></a>下一步行动

* 每当事件发生时，[接收警报通知](insights-receive-alert-notifications.md)。
* [监视服务标准](insights-how-to-customize-monitoring.md)，以确保您的服务是可用的并且能够作出响应。
* [跟踪服务运行状况](insights-service-health.md)，以查明 Azure 出现性能下降或服务中断。  
