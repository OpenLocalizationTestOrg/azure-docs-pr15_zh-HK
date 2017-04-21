<properties
    pageTitle="自动缩放云服务门户中 |Microsoft Azure"
    description="（传统）了解如何使用经典的门户配置中 Azure 的云服务 web 角色或辅助角色的自动比例规则。"
    services="cloud-services"
    documentationCenter=""
    authors="Thraka"
    manager="timlt"
    editor=""/>

<tags
    ms.service="cloud-services"
    ms.workload="tbd"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/06/2016"
    ms.author="adegeo"/>


# <a name="how-to-auto-scale-a-cloud-service"></a>如何自动缩放云服务

> [AZURE.SELECTOR]
- [Azure 门户](cloud-services-how-to-scale-portal.md)
- [Azure 的传统门户网站](cloud-services-how-to-scale.md)

在 Azure 的传统门户网站的比例页上，您可以手动缩放您的 web 角色或辅助角色中，或您可以启用基于 CPU 负载或消息队列自动缩放。

>[AZURE.NOTE] 本文主要讨论云服务 web 和辅助角色。 当您直接创建虚拟机 （经典） 时，位于在云服务中。 其中某些信息将应用于这些类型的虚拟机。 扩展虚拟机的可用性组其实只关闭这些打开和关闭基于您配置的比例规则。 有关虚拟机和可用性设置的详细信息，请参阅[管理虚拟机的可用性](../virtual-machines/virtual-machines-windows-classic-configure-availability.md)

配置您的应用程序扩展之前，应考虑以下信息︰

- 缩放会影响核心用法。 更大的角色实例使用更多内核。 您的订阅，可以扩展应用程序仅在内核的限制内。 例如，如果您的预订有二十个内核和运行应用程序的两个中等大小的限制云服务 （共四个核心），您可以只是扩大其他云服务部署在您的订阅中的十六个内核。 [云服务规模](cloud-services-sizes-specs.md)大小的详细信息，请参见

- 必须创建队列，并将其与某个角色关联之前，您可以扩展消息阈值所基于的应用程序。 有关详细信息，请参阅[如何使用队列存储服务](../storage/storage-dotnet-how-to-use-queues.md)。

- 您可以扩展链接到云服务的资源。 有关链接的资源的详细信息，请参阅[方法︰ 将资源链接到云服务](cloud-services-how-to-manage.md#how-to-link-a-resource-to-a-cloud-service)。

- 若要使您的应用程序的高可用性，应确保它部署与两个或多个角色实例。 有关详细信息，请参阅[服务级别协议要求](https://azure.microsoft.com/support/legal/sla/)。



## <a name="schedule-scaling"></a>缩放日程表

默认情况下，所有角色不遵循一个特定的时间表。 因此，更改的任何设置将应用于所有时间和所有日期在一年。 如果需要，您可以设置手动或自动缩放比例︰

- 工作日
- 周末
- 周夜晚
- 周早晨
- 特定日期
- 特定日期范围

这是在[Azure 的传统门户网站](https://manage.windowsazure.com/)conigured 上  
**云服务** > **\[云服务\]** > **比例** > **\[生产环境或临时\]**页。

单击您要更改的每个角色的**设置计划时间**按钮。

![云服务自动缩放比例根据计划][scale_schedules]



## <a name="manual-scale"></a>手动缩放

在**比例**上，可以手动增加或减少的云服务中正在运行的实例数。 如果您没有创建计划，这被配置已创建每个计划的或所有的时间。

1. 在[Azure 的传统门户网站](https://manage.windowsazure.com/)中，单击**云服务**，然后单击要打开的仪表板的云服务的名称。

    > [AZURE.TIP] 如果您看不到云服务，您可能需要更改从**生产**到**临时**，反之亦然。

2. 单击**小数位数**。

3. 选择您想要更改缩放比例选项的时间表。 默认值为*无计划的时间*如果您定义的任何计划。

4. 查找**由指标比例**部分并选择**NONE**。 这是所有角色的默认设置。

5. 在云服务中的每个角色都有一个滑块来更改要使用的实例的数量。

    ![手动调整云服务角色][manual_scale]

    如果需要多个实例时，您可能需要更改的[云服务的虚拟机大小](cloud-services-sizes-specs.md)。

6. 单击**保存**。  
要添加或删除基于您的选择角色实例。

>[AZURE.TIP] 只要看到![][tip_icon]移动鼠标，您可以获得有关特定设置的作用帮助。


## <a name="automatic-scale---cpu"></a>自动缩放-CPU

这可以扩展如果 CPU 使用率的平均百分比高于或低于指定的阈值;创建或删除角色实例。

1. 在[Azure 的传统门户网站](https://manage.windowsazure.com/)中，单击**云服务**，然后单击要打开的仪表板的云服务的名称。

    > [AZURE.TIP] 如果您看不到云服务，您可能需要更改从**生产**到**临时**，反之亦然。

2. 单击**小数位数**。

3. 选择您想要更改缩放比例选项的时间表。 默认值为*无计划的时间*如果您定义的任何计划。

4. 找到**的跃点数的比例**部分，然后选择**CPU**。

5. 现在您可以配置角色实例、 目标 CPU 使用率 （触发设置小数位数） 和多少个实例来向上和向下调整最小值和最大的范围。

![由 cpu 负载扩展云服务角色][cpu_scale]

>[AZURE.TIP] 只要看到![][tip_icon]移动鼠标，您可以获得有关特定设置的作用帮助。





## <a name="automatic-scale---queue"></a>自动缩放的队列

这将自动缩放如果队列中的邮件数会超过或低于指定的阈值;创建或删除角色实例。

1. 在[Azure 的传统门户网站](https://manage.windowsazure.com/)中，单击**云服务**，然后单击要打开的仪表板的云服务的名称。

    > [AZURE.TIP] 如果您看不到云服务，您可能需要更改从**生产**到**临时**，反之亦然。

2. 单击**小数位数**。

3. 找到**的跃点数的比例**部分，然后选择**CPU**。

4. 现在您可以配置角色实例，队列和队列消息处理的每个实例和多少个实例来向上和向下调整量最小值和最大范围。

![消息队列通过扩展云服务角色][queue_scale]

>[AZURE.TIP] 只要看到![][tip_icon]移动鼠标，您可以获得有关特定设置的作用帮助。


## <a name="scale-linked-resources"></a>扩展链接的资源

通常当您扩展角色，最好扩展应用程序还使用该数据库。 如果数据库链接到云服务时，可以通过单击相应的链接来访问该资源的缩放设置。

1. 在[Azure 的传统门户网站](https://manage.windowsazure.com/)中，单击**云服务**，然后单击要打开的仪表板的云服务的名称。

    > [AZURE.TIP] 如果您看不到云服务，您可能需要更改从**生产**到**临时**，反之亦然。

2. 单击**小数位数**。

3. 查找**链接的资源**部分然后单击了**此数据库的管理范围**。

    > [AZURE.NOTE] 如果您看不到**链接的资源**部分，您可能没有任何链接的资源。

![][linked_resource]


[manual_scale]: ./media/cloud-services-how-to-scale/manual-scale.png
[queue_scale]: ./media/cloud-services-how-to-scale/queue-scale.png
[cpu_scale]: ./media/cloud-services-how-to-scale/cpu-scale.png
[tip_icon]: ./media/cloud-services-how-to-scale/tip.png
[scale_schedules]: ./media/cloud-services-how-to-scale/schedules.png
[scale_popup]: ./media/cloud-services-how-to-scale/schedules-dialog.png
[linked_resource]: ./media/cloud-services-how-to-scale/linked-resources.png
