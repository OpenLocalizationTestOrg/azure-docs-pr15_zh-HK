<properties
   pageTitle="了解在 HDInsight 示例库与 Hadoop |Microsoft Azure"
   description="快速学习 Hadoop 从 HDInsight 获得已启动库运行示例应用程序。 使用示例数据，或提供您自己。"
   services="hdinsight"
   documentationCenter=""
   tags="azure-portal"
   authors="mumian"
   manager="jhubbard"
   editor="cgronlun"/>

<tags
   ms.service="hdinsight"
   ms.workload="big-data"
   ms.tgt_pltfrm="na"
   ms.devlang="na"
   ms.topic="article"
   ms.date="10/21/2016"
   ms.author="jgao"/>

# <a name="learn-hadoop-by-using-the-azure-hdinsight-getting-started-gallery"></a>了解 Hadoop 使用 Azure HDInsight 前已启动库

前已启动库功能仅适用于基于 Windows HDInsight 群集。 库提供了方便和快速的方法，了解 Hadoop HDInsight 以运行示例应用程序。 一些示例附带的示例数据。 您可以提供您自己的数据的其余示例。 目前，有以下 6 个样本 （具有多个出现）︰

- Azure 数据的解决方案
    - Microsoft Azure 网站日志分析
    - Microsoft Azure 存储分析
- 具有示例数据的解决方案
    - 传感器数据分析
    - Twitter 趋势分析
    - 网站日志分析
    - Mahout 影片建议

![HDInsight Hadoop、 占领和 HBase 前已启动库解决方案，包括示例数据。][hdinsight.sample.gallery]

下面的视频演示如何运行 Twitter 趋势分析示例︰

<center><a href="https://www.youtube.com/embed/7ePbHot1SN4">https://www.youtube.com/embed/7ePbHot1SN4></a></center>

仪表板可以访问通过浏览到 http://<YourHDInsightClusterName>.azurehdinsight.net/ 或从 Azure 的门户。

**若要从前已启动库运行示例**

1. 登录到[Azure 的门户网站][azure.portal]。
2. 从左侧的菜单中单击**浏览**， **HDInsight 群集**，请单击，然后单击群集名称。
3. 从顶部的菜单中单击**仪表板**。
4. （也称为群集用户） 的 HTTP 用户输入的用户名和密码。
6. 单击**前已启动库**页面的顶部。
7. 单击其中一个示例。 每个示例运行它提供详细的步骤。 下图显示了 Twitter 趋势分析示例︰

    ![HDInsight Twitter 趋势分析示例][hdinsight.twitter.sample]

## <a name="next-steps"></a>下一步行动
若要了解有关 HDInsight 的其他方式包括︰

- [HDInsight 学习地图][hdinsight.learn.map]
- [HDInsight infographic][hdinsight.infographic]

<!--Image references-->
[hdinsight.sample.gallery]: ./media/hdinsight-learn-hadoop-use-sample-gallery/HDInsight-Getting-Started-Gallery.png
[hdinsight.twitter.sample]: ./media/hdinsight-learn-hadoop-use-sample-gallery/HDInsight-Twitter-Trend-Analysis-sample.png

<!--Link references-->
[hdinsight.learn.map]: https://azure.microsoft.com/documentation/learning-paths/hdinsight-self-guided-hadoop-training/
[hdinsight.infographic]: http://go.microsoft.com/fwlink/?linkid=523960
[azure.portal]:https://portal.azure.com
