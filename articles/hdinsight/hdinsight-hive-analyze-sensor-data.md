<properties
    pageTitle="分析传感器数据使用配置单元和 Hadoop |Microsoft Azure"
    description="了解如何通过使用 HDInsight (Hadoop) 配置单元查询控制台分析传感器数据然后可视化与 PowerView Microsoft Excel 中的数据。"
    services="hdinsight"
    documentationCenter=""
    authors="Blackmist"
    manager="jhubbard"
    editor="cgronlun"
    tags="azure-portal"/>

<tags
    ms.service="hdinsight"
    ms.workload="big-data"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="article"
    ms.date="09/20/2016" 
    ms.author="larryfr"/>

#<a name="analyze-sensor-data-using-the-hive-query-console-on-hadoop-in-hdinsight"></a>分析传感器数据在 Hadoop 在 HDInsight 上使用该配置单元查询控制台

了解如何通过使用 HDInsight (Hadoop) 配置单元查询控制台分析传感器数据，然后通过使用 Power View 可视化 Microsoft Excel 中的数据。

> [AZURE.NOTE] 此文档中的步骤仅适用于基于 Windows HDInsight 群集中。

在此示例中，您将使用配置单元过程产生的加热，通风，空调 (HVAC) 系统的历史数据来识别系统不能可靠地维护设置温度的。 您将学习如何︰

- 创建查询数据存储在逗号分隔值 (CSV) 文件中的配置单元表。
- 创建配置单元查询对数据进行分析。
- 使用 Microsoft Excel 连接到 HDInsight （使用开放式数据库连接 (ODBC) 来检索分析的数据。
- 使用 Power View 可视化数据。

![解决方案体系结构关系图](./media/hdinsight-hive-analyze-sensor-data/hvac-architecture.png)

##<a name="prerequisites"></a>系统必备组件

* HDInsight (Hadoop) 群集︰ 关于创建群集的信息，请参阅[HDInsight 中的规定 Hadoop 群集](hdinsight-provision-clusters.md)。

* Microsoft Excel 2013 年

    > [AZURE.NOTE] Microsoft Excel 用于数据可视化与[Power View](https://support.office.com/Article/Power-View-Explore-visualize-and-present-your-data-98268d31-97e2-42aa-a52b-a68cf460472e?ui=en-US&rs=en-US&ad=US)。

* [Microsoft 的配置单元的 ODBC 驱动程序](http://www.microsoft.com/download/details.aspx?id=40886)

##<a name="to-run-the-sample"></a>若要运行该示例

1. 从 web 浏览器中，导航到下面的 URL。 更换`<clustername>`与 HDInsight 群集的名称。

        https://<clustername>.azurehdinsight.net

    出现提示时，身份验证使用的管理员用户名和密码时设置此群集使用。

2. 从打开的 web 页，**前已启动库**选项卡，然后单击**具有示例数据的解决方案**类别下的**传感器数据分析**示例。

    ![获取启动的库图像](./media/hdinsight-hive-analyze-sensor-data/getting-started-gallery.png)

3. 请按照 web 页上提供的说明进行操作以完成此示例。
