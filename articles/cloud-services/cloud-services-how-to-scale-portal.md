<properties
    pageTitle="自动缩放云服务门户中 |Microsoft Azure"
    description="了解如何使用门户配置中 Azure 的云服务 web 角色或辅助角色的自动比例规则。"
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

可以为云服务工作者角色触发扩展或缩小操作设置条件。 角色的条件可以基于 CPU、 磁盘或网络负载的角色。 您还可以设置基于消息队列或某些其他 Azure 的资源，与订阅相关的指标 conditation。

>[AZURE.NOTE] 本文主要讨论云服务 web 和辅助角色。 当您直接创建虚拟机 （经典） 时，位于在云服务中。 可以通过将它与[可用性设置](../virtual-machines/virtual-machines-windows-classic-configure-availability.md)关联扩展标准的虚拟机并手动将其打开或关闭。

## <a name="considerations"></a>注意事项

配置您的应用程序扩展之前，应考虑以下信息︰

- 缩放会影响核心用法。 更大的角色实例使用更多内核。 您的订阅，可以扩展应用程序仅在内核的限制内。 例如，如果您的预订有二十个内核和运行应用程序的两个中等大小的限制云服务 （共四个核心），您可以只是扩大其他云服务部署在您的订阅中的十六个内核。 [云服务规模](cloud-services-sizes-specs.md)大小的详细信息，请参见

- 您可以扩展基于队列消息阈值。 有关如何使用队列的详细信息，请参阅[如何使用队列存储服务](../storage/storage-dotnet-how-to-use-queues.md)。

- 您也可以缩放与订阅相关的其他资源。

- 若要使您的应用程序的高可用性，应确保它部署与两个或多个角色实例。 有关详细信息，请参阅[服务级别协议要求](https://azure.microsoft.com/support/legal/sla/)。

## <a name="where-scale-is-located"></a>小数位数所在的位置

选择云服务后，您应具有云服务刀片式服务器可见。

1. 云服务刀片，在**角色和实例**图块上选择云服务的名称。   
**重要事项**︰ 请确认单击了云服务角色，不低于该角色的角色实例。

    ![](./media/cloud-services-how-to-scale-portal/roles-instances.png)

2. 选择**小数位数**麻将牌。

    ![](./media/cloud-services-how-to-scale-portal/scale-tile.png)

## <a name="automatic-scale"></a>自动缩放比例

您可以使用两种模式下**手动**或**自动**配置角色的缩放设置。 手册是像预期的那样，您将设置实例的绝对计数。 但是，自动允许您设置规则来控制如何以及得您应缩放。

将**按比例**选项设置为**计划和性能规则**。

![云服务比例设置与配置文件和规则](./media/cloud-services-how-to-scale-portal/schedule-basics.png)

1. 现有的配置文件。
2. 添加为父配置文件的规则。
3. 添加另一个配置文件。

选择**添加配置文件**。 该配置文件确定要使用的小数位数的模式︰**总是**、**重复**、**固定日期**。

您已配置的配置文件和规则后，选择顶部的**保存**图标。

#### <a name="profile"></a>配置文件

配置文件设置最小和最大实例的规模，并且也在此范围处于活动状态时。

* **始终**

    始终保留此范围内可用的实例。  

    ![云服务始终扩展](./media/cloud-services-how-to-scale-portal/select-always.png)
    
* **重复周期**

    选择一套扩展的星期数。

    ![定期与云服务比例](./media/cloud-services-how-to-scale-portal/select-recurrence.png)
    
* **固定的日期**

    固定的日期范围来调整角色。

    ![云服务比例固定日期](./media/cloud-services-how-to-scale-portal/select-fixed.png)

您已配置了配置文件后，选择配置文件刀片底部的**确定**按钮。

#### <a name="rule"></a>规则

规则添加到配置文件，并表示将触发比例的条件。 

规则触发取决于云服务 （CPU 利用率、 磁盘活动或网络活动） 可以向其中添加一个条件的值的一个指标。 此外，您可以基于消息队列或与订阅相关的某些其他 Azure 的资源规格的触发器。

![](./media/cloud-services-how-to-scale-portal/rule-settings.png)

配置规则之后，选择规则刀片底部的**确定**按钮。

## <a name="back-to-manual-scale"></a>返回到手动缩放

导航到[刻度设置](#where-scale-is-located)，并**通过扩展**选项设置为**手动输入实例计数**。

![云服务比例设置与配置文件和规则](./media/cloud-services-how-to-scale-portal/manual-basics.png)

这将删除从角色自动缩放，则可以直接设置实例计数。 

1. 缩放 （手动或自动） 选项。
2. 一个角色实例滑块以设置要缩放到的实例。
3. 要缩放到的角色的实例。

配置比例设置之后，请选择顶部的**保存**图标。

