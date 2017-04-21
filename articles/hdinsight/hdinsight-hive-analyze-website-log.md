<properties 
    pageTitle="使用 Hadoop 的网站日志分析配置单元 |Microsoft Azure" 
    description="了解如何使用 HDInsight 配置单元来分析网站日志。 您将使用日志文件作为输入到 HDInsight 表中，并使用 HiveQL 来查询数据。" 
    services="hdinsight" 
    documentationCenter="" 
    authors="nitinme" 
    manager="jhubbard" 
    editor="cgronlun"
    tags="azure-portal"/>

<tags 
    ms.service="hdinsight" 
    ms.workload="big-data" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="05/17/2016" 
    ms.author="nitinme"/>

# <a name="use-hive-with-hdinsight-to-analyze-logs-from-websites"></a>使用 HDInsight 配置单元来分析网站的日志

了解如何使用 HDInsight HiveQL 来分析从网站的日志。 网站日志分析可用来细分受众的类似活动，对网站访问者的人口统计数据，进行分类，找出内容这些视图，它们来自的网站，诸如此类。

在此示例中，您将使用 HDInsight 群集分析网站日志文件来深入了解一天中从外部网站访问该网站的频率。 您还将生成用户体验的网站错误的摘要。 您将学习如何︰

- 连接到 Azure Blob 存储，其中包含网站的日志文件。
- 创建配置单元表来查询这些日志。
- 创建配置单元查询对数据进行分析。
- 使用 Microsoft Excel 连接到 HDInsight （通过使用开放式数据库连接 (ODBC) 来检索分析的数据。

![HDI。Samples.Website.Log.Analysis][img-hdi-weblogs-sample]

##<a name="prerequisites"></a>系统必备组件

- 您必须配置 Hadoop 群集在 Azure HDInsight。 有关说明，请参阅[设置 HDInsight 群集][hdinsight-provision]。 
- 您必须将 Microsoft Excel 2013 或 Excel 2010 安装。
- 您必须具有[Microsoft 配置单元 ODBC 驱动程序](http://www.microsoft.com/download/details.aspx?id=40886)来配置单元的数据导入 Excel。


##<a name="to-run-the-sample"></a>若要运行该示例

1. 从[Azure 门户](https://portal.azure.com/)，从 Startboard （如果固定那里群集） 中，单击您要在其运行该示例群集拼贴。

2. 从群集刀片式服务器，在**快速链接**下**群集的仪表板**，请单击，然后从**群集仪表板**刀片式服务器，单击**HDInsight 群集仪表板**。 或者，可以通过使用以下 URL 直接打开仪表板︰

        https://<clustername>.azurehdinsight.net
    
    出现提示时，身份验证使用的管理员用户名和密码配置群集时使用。
  
2. 从打开的 web 页，**前已启动库**选项卡，然后下**具有示例数据的解决方案**类别中，单击**网站日志分析**示例。

3. 请按照 web 页上提供的说明进行操作以完成此示例。

##<a name="next-steps"></a>下一步行动
请尝试下面的示例︰[使用 HDInsight 蜂巢分析传感器数据](hdinsight-hive-analyze-sensor-data.md)。


[hdinsight-provision]: hdinsight-provision-clusters.md
[hdinsight-sensor-data-sample]: ../hdinsight-use-hive-sensor-data-analysis.md

[img-hdi-weblogs-sample]: ./media/hdinsight-hive-analyze-website-log/hdinsight-weblogs-sample.png
 
