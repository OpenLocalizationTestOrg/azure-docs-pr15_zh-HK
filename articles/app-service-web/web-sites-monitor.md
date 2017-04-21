<properties
    pageTitle="在 Azure 应用程序服务的监视应用程序"
    description="了解如何使用 Azure 门户监视在 Azure 应用程序服务的应用程序。"
    services="app-service"
    documentationCenter=""
    authors="btardif"
    manager="wpickett"
    editor="mollybos"/>

<tags
    ms.service="app-service"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/07/2016"
    ms.author="byvinyal"/>

# <a name="how-to-monitor-apps-in-azure-app-service"></a>如何︰ 监视 Azure 应用程序服务中的应用

[应用程序服务](http://go.microsoft.com/fwlink/?LinkId=529714)提供了[Azure 门户](https://portal.azure.com)中的内置监控功能。
这包括审查**配额**和**指标**应用程序以及应用程序服务计划中，并设置**警报**和均匀**缩放**自动根据这些指标的能力。

[AZURE.INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="understanding-quotas-and-metrics"></a>了解配额和指标

### <a name="quotas"></a>配额

服务应用程序中承载的应用程序容易受到某些*限制*他们可以使用的资源。 限制定义与应用程序关联的**应用程序服务计划**。

如果承载的应用程序中的**闲**或**共享**计划，然后由**配额**定义应用程序可以使用的资源上的限制。

如果在**基本**、**标准**或**特优**计划，则限制上承载的应用程序可以使用它们的资源设置的**大小**（小型，中型，大型） 和**实例计数**（1、 2、 3...） 的**应用程序服务计划**。

**配额**的**自由**或**共享**的应用程序包括︰

* **CPU(Short)**
   * 允许此应用程序以 3 分钟间隔的 CPU 量。 此配额重新设置时间间隔为 3 分钟。
* **CPU(Day)**
   * 此应用程序在一天中允许使用的 CPU 的总金额。 此配额重新设置在午夜 UTC 每 24 小时一次。
* **内存**
   * 允许此应用程序的内存总量。
* **带宽**
   * 允许此应用程序的某一天传出带宽的总量。
   此配额重新设置在午夜 UTC 每 24 小时一次。
* **文件系统**
   * 允许存储的总金额。

仅适用于**基本**、**标准**和**高级**规划上承载的应用程序的配额是**文件系统**。

有关特定配额限制，可用于不同的应用程序服务 Sku 的功能的详细信息可以在以下位置找到︰ [Azure 订阅服务限制](../azure-subscription-service-limits.md#app-service-limits)

#### <a name="quota-enforcement"></a>强制实施配额

如果应用程序使用量超过了**CPU （短）**， **CPU （天）**或**带宽**配额然后应用程序将停止直到重新设置配额。 在此期间，所有传入的请求将导致**HTTP 403**。
![][http403]

如果超过应用程序**内存**配额，则应用程序将重新启动。

如果超出了**文件系统**配额，然后任何写操作都将失败，这包括写入日志。

可以增加或删除从您的应用程序通过升级您的应用程序服务计划配额。

### <a name="metrics"></a>指标

**指标**提供有关该应用程序或应用程序服务计划的行为。

对于**应用程序**中，可用的指标是︰

* **平均响应时间**
   * 为请求提供服务以毫秒为单位的应用程序所需的平均时间。
* **平均内存工作集**
   * 平均的 Mib 由应用程序使用的内存量。
* **CPU 时间**
   * 以秒为单位，由应用程序使用的 cpu 资源量。 有关此指标请参阅详细信息︰ [CPU 时间与 CPU 百分比](#cpu-time-vs-cpu-percentage)
* **中的数据**
   * 在 Mib 中应用程序消耗的传入带宽量。
* **数据输出**
   * 传出在 Mib 中应用程序消耗的带宽量。
* **Http 2xx**
   * 导致一个 http 状态代码的请求的计数 > = 200 但 < 300。
* **Http 3xx**
   * 导致一个 http 状态代码的请求的计数 > = 300 但 < 400。
* **Http 401**
   * 从而导致 HTTP 401 状态代码的请求数。
* **Http 403**
   * 结果 HTTP 403 状态代码的请求数。
* **Http 404**
   * 从而导致 HTTP 404 状态代码的请求数。
* **Http 406**
   * 结果 HTTP 406 状态代码的请求数。
* **Http 4xx**
   * 导致一个 http 状态代码的请求的计数 > = 400 但 < 500。
* **Http 服务器错误**
   * 导致一个 http 状态代码的请求的计数 > = 500 但 < 600。
* **内存工作集**
   * 在 Mib 中的应用程序使用的内存的当前数量。
* **请求**
   * 而不考虑其生成的 HTTP 状态代码的请求的总数。

为**应用程序服务计划**，可用的指标是︰

>[AZURE.NOTE] 应用程序服务计划指标属性仅适用于**基本**、**标准**和**高级**SKU 中的计划。

* **CPU 百分比**
   * 整个计划的所有实例使用平均 CPU。
* **内存百分比**
   * 使用该计划的所有实例之间的平均内存。
* **中的数据**
   * 该计划的所有实例所使用的平均传入带宽。
* **数据输出**
   * 平均值传出计划的所有实例之间使用的带宽。
* **磁盘队列长度**
   * 平均的数读和写上存储已排队的请求。 较高的磁盘队列长度是可能减缓由于过多的磁盘 I/O 的应用程序的指示。
* **Http 队列长度**
   * 不得不被满足之前坐在队列的 HTTP 请求的平均数。 高或不断增加 HTTP 队列长度是计划负载过大的症状。

### <a name="cpu-time-vs-cpu-percentage"></a>CPU 时间与 CPU 百分比
<!-- To do: Fix Anchor (#CPU-time-vs.-CPU-percentage) -->

有 2 反映 CPU 使用率的指标。 **CPU 时间**和**CPU 百分比**

**CPU 时间**是由于在 CPU 数分钟内使用的应用程序定义了其中一个配额**闲**或**共享**计划中承载的应用程序很有用。

另一方面， **CPU 百分比**可用于应用程序承载于**基本**、**标准**和**津贴**计划，因为他们可以向外扩展，该度量是总体使用情况很好地反映在所有实例之间。

##<a name="metrics-granularity-and-retention-policy"></a>度量的粒度和保留策略

对应用程序和应用程序服务计划指标被记录下来，并与以下粒度和保留策略服务聚合︰

 * **分钟**粒度指标保留**48**小时
 * **小时**粒度指标保留**30**天
 * **一天**粒度指标保留**90**天

## <a name="monitoring-quotas-and-metrics-in-the-azure-portal"></a>监视配额和指标在 Azure 的门户。

您可以查看不同的**配额**和**指标**影响的[Azure 门户](https://portal.azure.com)应用程序的状态。

![][quotas]
在设置下找不到**配额**>**配额**。 用户体验，使您可以检查: （1） 配额名称、 （2） 其重设间隔、 （3） 其当前限制和 （4） 当前值。

![][metrics]
**测量数据**可以直接从刀片式服务器资源的访问。 您还可以自定义的图表: (1)**单击**，并选择 (2)**编辑图表**。
从这里您可以更改**时间范围**(3)、 (4) 的**图表类型**和 (5)**指标**来显示。  

您可以了解有关以下指标︰[监视器服务标准](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md)。

## <a name="alerts-and-autoscale"></a>警报和自动缩放比例
指标可以挂接到一个应用程序或应用程序服务计划的提醒，若要了解详细信息，请参阅[接收警报通知](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

应用程序服务应用程序承载于基本、 标准或特优计划应用程序服务支持**自动缩放**。 这使您可以配置规则，监控应用程序服务计划指标，并可以增加或减少根据需要提供额外的资源实例计数或节省资金过度调配资源的应用程序时。 您可以了解更多有关自动缩放比例︰[如何缩放](../monitoring-and-diagnostics/insights-how-to-scale.md)和这里[Azure 监视器自动缩放的最佳做法](../monitoring-and-diagnostics/insights-autoscale-best-practices.md)

>[AZURE.NOTE] 如果您想要怎样的 Azure 帐户之前开始使用 Azure 应用程序服务，请转到[尝试应用程序服务](http://go.microsoft.com/fwlink/?LinkId=523751)，立即可以在此创建短期的初学者 web 应用程序在应用程序服务。 没有信用卡，所需;没有承诺。

## <a name="whats-changed"></a>会发生什么变化
* 有关更改网站为应用程序服务的指南，请参阅︰ [Azure 应用程序服务，并对现有的 Azure 服务及其影响](http://go.microsoft.com/fwlink/?LinkId=529714)

[fzilla]:http://go.microsoft.com/fwlink/?LinkId=247914
[vmsizes]:http://go.microsoft.com/fwlink/?LinkID=309169



<!-- Images. -->
[http403]: ./media/web-sites-monitor/http403.png
[quotas]: ./media/web-sites-monitor/quotas.png
[metrics]: ./media/web-sites-monitor/metrics.png
