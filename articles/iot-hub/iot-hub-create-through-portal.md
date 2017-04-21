<properties
     pageTitle="使用 Azure 门户创建 IoT 中心 |Microsoft Azure"
     description="简要介绍了如何创建和管理 Azure IoT 集线器通过 Azure 门户"
     services="iot-hub"
     documentationCenter=""
     authors="dominicbetts"
     manager="timlt"
     editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="na"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/30/2016"
     ms.author="dobett"/>

# <a name="create-an-iot-hub-using-the-azure-portal"></a>创建使用 Azure 门户 IoT 集线器

[AZURE.INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]


## <a name="introduction"></a>介绍

本文介绍了如何查找 IoT 中心服务在 Azure 的门户中，以及如何创建和管理 IoT 集线器。

## <a name="where-to-find-iot-hubs"></a>在哪里可以找到 IoT 集线器

有多个位置，您可以找到 IoT 集线器。

1. **+ 新建**︰ **Azure IoT 集线器**IoT 服务，并且可以找到在类别中**互联网的内容**下**+ 新建**，类似于其他服务。

2. IoT 集线器也可以通过市场作为**互联网的情况**下的英雄服务。

## <a name="create-an-iot-hub"></a>创建 IoT 集线器

您可以创建使用以下方法 IoT 中心︰

- 创建 IoT 中心通过**+ 新建**选项会导致刀片式服务器的下一步的屏幕抓图中所示。 创建 IoT 中心通过这种方法，并通过市场的步骤是相同的。

- 创建通过市场 IoT 中心︰ 单击**创建**打开等同于以前的刀片式服务器**+ 新**体验的刀片式服务器。 以下各节列出了创建 IoT 中心所涉及的几个步骤。

### <a name="choose-the-name-of-the-iot-hub"></a>选择 IoT 中心的名称

若要创建一个 IoT 集线器，必须命名中心。 集线器此名称必须唯一。 后端，因此建议该中心被命名为尽可能唯一允许重复的集线器。

### <a name="choose-the-pricing-tier"></a>选择定价层

您可以从四个层次︰**自由**、**标准 1**和**标准 2**，以及**标准的 S3**。 自由层允许仅 500 设备连接至 IoT 中心和多达每日 8000 的消息。

**标准的 S1**: IoT 集线器 S1 版专为 IoT 解决方案，有大量的设备生成的每个设备的数据量相对较少。 S1 版每套允许每天达 400000 消息跨所有连接的设备。

**标准的 S2**: IoT 中心 S2 版专为 IoT 解决方案的设备生成大量的数据。 S2 版各单位允许所有已连接的设备之间每天达 6 万份邮件。

**标准的 S3**: IoT 集线器 S3 版专为 IoT 解决方案生成大量的数据。 S3 版每套允许所有已连接的设备之间每天达 300 万份邮件。

![][4]

> [AZURE.NOTE] IoT 中心只允许每个 Azure 订阅一个免费的中心。

### <a name="iot-hub-units"></a>IoT 集线器设备

IoT 集线器设备包括一定数量的每一天的邮件。 支持该中心的消息总数是单位数乘以每个工作日这一层的消息数数。 例如，如果您希望 IoT 集线器，以支持 700,000 消息的入口，您选择 S1 层的两个单元。

### <a name="device-to-cloud-partitions-and-resource-group"></a>设备连接到云分区和资源组

您可以更改 IoT 集线器的分区数。 默认分区被设置为 4。但是，您可以从下拉列表选择不同的分区数。

资源组的您不需要显式地创建一个空的资源组。 在创建资源时，您可以选择创建新的资源组或使用现有资源组。

![][5]

### <a name="choose-subscriptions"></a>选择订阅

Azure IoT 中心将自动显示用户帐户链接到 Azure 订阅列表中。 您可以选择此处的选项与该 Azure 订阅关联 IoT 中心之一。

### <a name="choose-the-location"></a>选择位置

位置选项提供的地区 IoT 集线器提供的列表。 IoT 集线器可在以下位置中部署︰ 澳大利亚东部、 澳大利亚东南部、 东亚、 东南亚、 北欧、 西欧、 日本东，西，日本美国东部，美国西部。

### <a name="create-the-iot-hub"></a>创建 IoT 集线器

前面所有的步骤都完成后，已准备就绪，要创建 IoT 集线器。 单击**创建**以开始使用特定的选项，创建此 IoT 中枢的后端过程并将其部署到指定的位置。

可能需要几分钟 IoT 集线器，以便创建是花些时间出现在合适的位置服务器后端部署。

## <a name="change-the-settings-of-the-iot-hub"></a>更改 IoT 中心的设置

创建从 IoT 中心刀片式服务器后，可以更改现有 IoT 中心的设置。

![][8]

**共享访问权限策略**︰ 这些策略定义连接到 IoT 中心设备和服务的权限。 您可以通过单击**常规**下的**共享访问策略**访问这些策略。 在此刀片，可以修改现有的策略，或添加新的策略。

### <a name="create-a-policy"></a>创建策略

- 单击**添加**以打开刀片式服务器。 这里您可以输入新策略的名称以及要与该策略中，如下图中所示的权限。

    有几个可以使用这些共享策略相关联的权限。 前两种策略、**注册表读取**和**写入注册表**，授予读取和写入访问权限的设备标识存储区或标识注册表。 选择写入选项，则自动选择的读取的选项。

    **服务连接**策略授予服务 IoT 集线器连接的使用者组如云边端点的访问权。 **设备连接**策略授予权限的发送和接收邮件的 IoT 集线器设备端端点上。

- 单击**创建**以添加该新创建的策略与现有列表。

![][10]

## <a name="messaging"></a>消息

单击**消息**显示消息 IoT 集线器被修改的属性的列表。 有两种主要类型的属性，您可以修改或复制︰**云至设备**和**设备连接到云**。

- **云与设备**设置︰ 该设置有两个 subsettings︰**设备 ttl 云**（--生存时间） 和**保留时间**的消息。 当首次创建 IoT 中心时，两个这些设置被创建带有默认值的一个小时。 若要调整这些值，请使用滑块或键入值。

- **与云的设备**的设置︰ 此设置不会产生多个 subsettings，其中一些名为/分配 IoT 集线器创建仅可被复制到其他可自定义的 subsettings 时。 下一节中列出了这些设置。

**分区**︰ IoT 中心创建和通过此设置可以更改时设置此值。

**事件的中心兼容名称和终结点**︰ 当 IoT 集线器，事件中心将创建内部，您可能需要在某些情况下访问。 此事件中心兼容名称和终结点不能自定义，但可用于通过**复制**按钮。

**保留时间**︰ 默认设置为一天，但可以自定义为其他值，使用下拉列表。 此值是在天与云的设备后，不是以小时为单位是类似云至设备的设置。

**使用者组**︰ 使用者组是类似于其他邮件系统，可以用于提取数据以特定的方式来连接到 IoT 集线器上的其他应用程序或服务的设置。 每个 IoT 中心创建的默认客户组。 但是，可以添加或删除到 IoT 集线器的使用者组。

> [AZURE.NOTE] 不能编辑或删除默认客户组。

![][11]

## <a name="pricing-and-scale"></a>定价和可扩展性

现有的 IoT 集线器的定价可以更改通过**定价**设置，但以下情况除外︰

- 在当前的实现中，可用 sku IoT 集线器无法更改层为付费的 Sku，反之亦然。
- 只能有一个自由层 IoT 中心在 Azure 的订阅。

![][12]

从更高的层 （S2 或 S3） 移动到较低层 （S1 或 S2） 才允许的这一天发送的消息的数目将不产生冲突。 例如，如果每日邮件数超过超过 400000，然后 IoT 集线器的层可以更改。 但是，如果您更改为 S1 层然后集线器被遏制的这一天。

## <a name="delete-the-iot-hub"></a>删除 IoT 集线器

您可以浏览到想要通过单击**浏览**，然后选择相应的集线器，以删除删除 IoT 集线器。 单击要删除中心在中心名称下面的**删除**按钮。

## <a name="next-steps"></a>下一步行动

按照这些链接以了解更多关于管理 Azure IoT 中心︰

- [批量管理 IoT 设备][lnk-bulk]
- [使用情况指标][lnk-metrics]
- [操作监视][lnk-monitor]

要进一步探索 IoT 中心的功能，请参阅︰

- [开发人员指南][lnk-devguide]
- [模拟具有 IoT 网关 SDK 的设备][lnk-gateway]
- [设置安全 IoT 解决方案从零开始][lnk-securing]


  [4]: ./media/iot-hub-create-through-portal/create-iothub.png
  [5]: ./media/iot-hub-create-through-portal/location1.png
  [8]: ./media/iot-hub-create-through-portal/portal-settings.png
  [10]: ./media/iot-hub-create-through-portal/shared-access-policies.png
  [11]: ./media/iot-hub-create-through-portal/messaging-settings.png
  [12]: ./media/iot-hub-create-through-portal/pricing-error.png

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md