<properties
 pageTitle="处理与在 HDInsight 上的 Apache 风暴的车辆传感器数据 |Microsoft Azure"
 description="了解如何处理事件集线器在 HDInsight 上使用 Apache 风暴从车辆传感器数据。 从 DocumentDB，添加模型数据并存储到存储输出。"
 services="hdinsight,documentdb,notification-hubs"
 documentationCenter=""
 authors="Blackmist"
 manager="jhubbard"
 editor="cgronlun"/>

<tags
ms.service="hdinsight"
ms.devlang="java"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="big-data"
ms.date="08/23/2016"
ms.author="larryfr"/>

#<a name="process-vehicle-sensor-data-from-azure-event-hubs-using-apache-storm-on-hdinsight"></a>处理从 Azure 事件集线器在 HDInsight 上使用 Apache 风暴的车辆传感器数据

了解如何处理从 Azure 事件集线器在 HDInsight 上使用 Apache 风暴的车辆传感器数据。 本示例从 Azure 事件集线器读取传感器数据、 通过引用存储在 Azure DocumentDB 数据丰富数据，最后将数据存储到 Azure 存储使用 Hadoop 文件系统 (HDFS)。

![HDInsight 和互联网内容 (IoT) 体系结构关系图](./media/hdinsight-storm-iot-eventhub-documentdb/iot.png)

##<a name="overview"></a>概述

将传感器添加到车辆可以预测根据历史数据趋势的设备问题，以及取得根据使用率模式分析的未来版本的改进。 虽然传统的 MapReduce 的批处理操作可用来进行这种分析，您必须能够快速、 高效地将数据从所有车辆之前加载到 Hadoop MapReduce 处理可能发生。 此外，您可能希望进行严重故障路径 （引擎温度、 刹车等） 进行实时的分析。

Azure 事件集线器构建处理数量巨大的数据生成的传感器，并在 HDInsight 上的 Apache 风暴可用于加载和处理的数据，将其存储在 HDFS （Azure 存储为后盾） 之前额外的 MapReduce 处理。

##<a name="solution"></a>解决方案

遥测数据引擎温度、 环境温度和车辆速度传感器，然后发送到事件集线器以及汽车的车辆识别号码 (VIN) 和时间戳的记录。 从那里，运行在 HDInsight 群集上的 Apache 风暴冲击拓扑读取数据、 处理，并将其存储到 HDFS。

在处理期间，VIN 用于从 Azure DocumentDB 检索模型信息。 将此特性添加到数据流存储之前。

风暴在拓扑中使用的组件包括︰

* **EventHubSpout** -从 Azure 事件集线器中读取数据

* **TypeConversionBolt** -转换为 JSON 字符串事件集线器包含引擎温度、 环境温度、 速度、 VIN，而且时间戳的单个数据值的元组

* 从使用 VIN 的 DocumentDB **DataReferencBolt** -查找车辆模型

* **WasbStoreBolt** -存储到 HDFS （Azure 存储） 的数据

下面是此解决方案的图表︰

![风暴的拓扑结构](./media/hdinsight-storm-iot-eventhub-documentdb/iottopology.png)

> [AZURE.NOTE] 这是一个简化的图表，并在解决方案中的每个组件可能具有多个实例。 例如，将拓扑结构中每个组件的多个实例分布在风暴 HDInsight 群集上的节点。

##<a name="implementation"></a>实现

一个完整、 自动化的解决方案，那么这种情况下提供 GitHub 上[风暴的 HDInsight 示例](https://github.com/hdinsight/hdinsight-storm-examples)存储库的一部分。 若要使用此示例，请按照[IoTExample 自述文件。MD](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/IotExample/README.md)。

## <a name="next-steps"></a>下一步行动

对于风暴拓扑更多示例，请参阅[风暴在 HDInsight 上的示例拓扑](hdinsight-storm-example-topology.md)。
