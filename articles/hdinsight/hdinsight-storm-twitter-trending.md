<properties
   pageTitle="使用 twitter 趋势主题，与在 HDInsight 上的 Apache 风暴 |Microsoft Azure"
   description="了解如何使用 Trident 可以确定在基于 hashtags 的 Twitter 趋势主题的 Apache 风暴拓扑结构。"
   services="hdinsight"
   documentationCenter=""
   authors="Blackmist"
   manager="jhubbard"
   editor="cgronlun"
    tags="azure-portal"/>

<tags
   ms.service="hdinsight"
   ms.devlang="java"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="big-data"
   ms.date="09/27/2016"
   ms.author="larryfr"/>

#<a name="determine-twitter-trending-topics-with-apache-storm-on-hdinsight"></a>确定在 HDInsight 上的 Apache 风暴与 Twitter 趋势主题

了解如何使用 Trident 可以确定趋势主题 （哈希标记） 在 Twitter 上的风暴拓扑结构。

Trident 是提供诸如联接、 聚合、 分组、 函数和筛选器等工具的高级抽象。 另外，Trident 添加原语操作状态、 增量处理。 此示例演示如何创建一个使用自定义的管口、 函数和几个提供的 Trident 的内置函数的拓扑结构。

> [AZURE.NOTE] 此示例很大程度基于璜 Alonso [Trident 风暴](https://github.com/jalonsoramos/trident-storm)示例。

##<a name="requirements"></a>要求

* <a href="http://www.oracle.com/technetwork/java/javase/downloads/index.html" target="_blank">Java 和 JDK 1.7</a>

* <a href="http://maven.apache.org/what-is-maven.html" target="_blank">Maven</a>

* <a href="http://git-scm.com/" target="_blank">Git</a>

* Twitter 开发者帐户

##<a name="download-the-project"></a>下载的项目

下面的代码用于克隆本地项目。

    git clone https://github.com/Blackmist/TwitterTrending

##<a name="topology"></a>拓扑结构

下面是此示例中的拓扑︰

![拓扑结构](./media/hdinsight-storm-twitter-trending/trident.png)

> [AZURE.NOTE] 这是拓扑结构的简化的视图。 组件的多个实例将分发到群集中的节点。

实现了拓扑的 Trident 代码如下所示︰

    topology.newStream("spout", spout)
        .each(new Fields("tweet"), new HashtagExtractor(), new Fields("hashtag"))
        .groupBy(new Fields("hashtag"))
        .persistentAggregate(new MemoryMapState.Factory(), new Count(), new Fields("count"))
        .newValuesStream()
        .applyAssembly(new FirstN(10, "count"))
        .each(new Fields("hashtag", "count"), new Debug());

此代码将执行下列操作︰

1. 从管口创建一个新的来源。 作为管口从 Twitter，检索 tweets，并为特定的关键字 （爱、 音乐和咖啡在本例中） 进行筛选。

2. HashtagExtractor，一个自定义的函数，用于哈希标记提取每个 tweet。 被发送到流中。

3. 流是由哈希标记分组，并且传递给聚合函数。 此聚合函数创建的每一个哈希标记出现的次数的计数。 此数据将保留在内存中。 最后，新的流发出包含哈希标记和计数。

4. 因为我们只是想一给定批的 tweets 最受欢迎的哈希标记，应用**FirstN**程序集返回仅前 10 个值，基于计数的字段。

> [AZURE.NOTE] 而不是管口和 HashtagExtractor，我们将使用内置 Trident 功能。
>
> 有关内置操作信息，请参阅<a href="https://storm.apache.org/apidocs/storm/trident/operation/builtin/package-summary.html" target="_blank">软件包 storm.trident.operation.builtin</a>。
>
> 除 MemoryMapState 以外的 Trident 状态实现，请参阅下列资源︰
>
> * <a href="https://github.com/fhussonnois/storm-trident-elasticsearch" target="_blank">冲击 Trident 弹性搜索</a>
>
> * <a href="https://github.com/kstyrc/trident-redis" target="_blank">trident redis</a>

###<a name="the-spout"></a>作为管口

管口， **TwitterSpout**，使用<a href="http://twitter4j.org/en/" target="_blank">Twitter4j</a>从 Twitter 中检索 tweets。 （爱、 音乐和咖啡在本例中），则创建一个筛选器，并在链接阻塞队列中存储与筛选器匹配传入 tweets （状态）。 （有关详细信息，请参见<a href="http://docs.oracle.com/javase/7/docs/api/java/util/concurrent/LinkedBlockingQueue.html" target="_blank">类 LinkedBlockingQueue</a>）。最后，项目将从队列中提取并发出到拓扑结构中。

###<a name="the-hashtagextractor"></a>HashtagExtractor

若要提取哈希标记， <a href="http://twitter4j.org/javadoc/twitter4j/EntitySupport.html#getHashtagEntities--" target="_blank">getHashtagEntities</a>用于检索所有哈希标记包含在 tweet。 这些然后被发送到流。

##<a name="enable-twitter"></a>启用 Twitter

使用以下步骤来注册新的 Twitter 应用程序并获取从 Twitter 中读取所需的使用者和访问令牌信息︰

1. 转到<a href="https://apps.twitter.com" target="_blank">使用 Twitter 的应用程序</a>并单击**创建新应用程序**按钮。 当填写窗体，该**回调 URL**字段留空。

2. 在创建应用程序时，请单击**键和访问令牌**选项卡。

3. 复制**使用者密钥**和**用户机密**信息。

4. 在页面的底部，选择**创建我的访问令牌**，如果没有标记存在。 创建标记后，将复制的**访问令牌**和**访问令牌的密钥**信息。

5. 在**TwitterSpoutTopology**项目中以前克隆，打开**resources/twitter4j.properties**文件，添加在上一步骤中，您收集的信息，然后保存该文件。

##<a name="build-the-topology"></a>构建拓扑

使用下面的代码来生成项目︰

        cd [directoryname]
        mvn compile

##<a name="test-the-topology"></a>测试拓扑

使用下面的命令来测试本地的拓扑︰

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.TwitterTrendingTopology

拓扑结构启动后，您应该看到调试信息包含哈希标记并按拓扑计算发出。 输出应类似于以下内容︰

    DEBUG: [Quicktellervalentine, 7]
    DEBUG: [GRAMMYs, 7]
    DEBUG: [AskSam, 7]
    DEBUG: [poppunk, 1]
    DEBUG: [rock, 1]
    DEBUG: [punkrock, 1]
    DEBUG: [band, 1]
    DEBUG: [punk, 1]
    DEBUG: [indonesiapunkrock, 1]

##<a name="next-steps"></a>下一步行动

现在，测试本地拓扑发现如何部署拓扑结构︰[部署和管理在 HDInsight 上的 Apache 风暴拓扑](hdinsight-storm-deploy-monitor-topology.md)。

您也可以在下列主题中冲击感兴趣︰

* [在 HDInsight 使用 Maven 的风暴为开发 Java 拓扑](hdinsight-storm-develop-java-topology.md)

* [在使用 Visual Studio 的 HDInsight 上的风暴为开发 C# 拓扑](hdinsight-storm-develop-csharp-visual-studio-topology.md)

有关 HDinsight 的更多的暴风雨示例︰

* [在 HDInsight 上的风暴的示例拓扑](hdinsight-storm-example-topology.md)
