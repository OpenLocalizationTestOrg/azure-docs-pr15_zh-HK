<properties 
    pageTitle="常见的 DocumentDB 用例 |Microsoft Azure" 
    description="关于顶部五个使用案例 DocumentDB︰ 用户生成内容、 事件日志、 目录数据、 用户首选项数据，以及互联网内容 (IoT)。" 
    services="documentdb" 
    authors="h0n" 
    manager="jhubbard" 
    editor="monicar" 
    documentationCenter=""/>

<tags 
    ms.service="documentdb" 
    ms.workload="data-services" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="10/28/2016" 
    ms.author="hawong"/>

# <a name="common-documentdb-use-cases"></a>常见的 DocumentDB 使用案例
本文概述了有关 DocumentDB 的几种常见的使用案例。  这篇文章中的建议开发您的应用程序使用 DocumentDB 作为起始点。   

阅读这篇文章之后, 您将能够回答以下问题︰ 
 
- DocumentDB 的常见使用情形是什么？
- 作为用户使用 DocumentDB 的优点是什么生成内容存储区？
- 存储使用 DocumentDB 作为目录数据的好处是什么？
- 存储使用 DocumentDB 作为一个事件日志的好处是什么？
- 存储使用 DocumentDB 与用户首选项数据的好处是什么？
- 对于互联网的东西 (IoT) 系统作为数据存储使用 DocumentDB 的优点有哪些？

## <a name="common-use-cases-for-documentdb"></a>DocumentDB 的常见使用情形
Azure 的 DocumentDB 是一个一般用途 NoSQL 数据库，在各种应用程序和用例中使用。 它是一个不错的选择，对于任何应用程序都需要低顺序的毫秒响应时间，并且需要快速扩展。 以下是一些的 DocumentDB 的特性，使其特别适合于高性能的应用程序。

- DocumentDB 本身的分区数据的高可用性和可扩展性。
- DocumentDB 的已备份的 SSD 存储以低延迟订单的毫秒的响应时间。
- 最终，会话和包围失效等的一致性级别的 DocumentDB 的支持允许低成本的性能比。 
- DocumentDB 具有独立米存储和吞吐量的灵活数据友好定价模型。
- DocumentDB 的保留的吞吐量模型可读/写，而不是 CPU/内存/IOPs 的底层硬件的角度来思考。
- DocumentDB 的设计可让您扩展到大规模请求卷顺序每日请求数十亿。

谈到 web、 移动，游戏和 IoT 应用程序需要较短响应时间，需要处理大量的读取和写入操作，这些特性是特别有好处。 

## <a name="user-generated-content"></a>用户生成的内容
DocumentDB 的常见用例是存储和查询用户生成内容 (UGC) 针对 web 和移动应用程序，尤其是社交媒体应用程序。  UGC 的一些示例包括聊天会话、 tweets、 博客帖子、 分级和注释。  通常，在应用程序中的社交媒体 UGC 是混合自由格式文本、 属性、 标记和不受刚性结构的关系。   

如聊天内容，评论和文章可以存储在 DocumentDB 而无需转换或复杂对象关系映射层。  可以添加或容易修改，以满足需求，为开发循环访问应用程序代码，从而提升快速开发数据属性。  

与各种社交网络集成的应用程序必须响应来自这些网络更改架构。  如 DocumentDB 在默认情况下，自动编制索引数据，数据就可以随时查询。  因此，这些应用程序可以灵活地检索按照其各自需求的预测。       

许多社交应用程序运行在全球范围内，可能出现不可预测的使用模式。  随着应用程序层缩放，以符合使用要求，扩展数据存储的灵活性至关重要。  您可以通过添加其他数据分区 DocumentDB 帐户下的放大。  此外，还可以跨多个区域创建额外的 DocumentDB 帐户。 DocumentDB 服务地区的可用性，请参阅[Azure 区域](https://azure.microsoft.com/regions/#services)。   

## <a name="catalog-data"></a>目录数据
目录数据使用方案涉及存储和查询一组实体如人物、 地点和产品的特性。  目录数据的一些示例包括用户帐户、 产品目录、 设备注册表 IoT，和清单资料的系统。  此数据的属性可能有所不同，并且可以更改以适合应用程序的要求。  

考虑汽车部件供应商的产品目录的示例。 每个部分都可能有自己所有的部件都共享公共属性的属性。  此外，对于特定部件的属性可以更改何时发布一个新的模型在下一年。  作为 JSON 文档存储，DocumentDB 支持灵活的架构，并允许您表示嵌套的属性与数据，因此它非常适合存储产品目录数据。       

## <a name="logging-and-time-series-data"></a>日志记录和时间系列数据
应用程序日志记录通常发出大的卷中，可能有不同的特性基础部署的应用程序版本或组件记录的事件。  日志数据不受严格的结构或复杂的关系。 越来越多日志数据保持不变以 JSON 格式因为 JSON 是轻量，便于人们阅读。
   
通常有两个主要用例相关的事件日志数据。  第一个使用案例是在进行故障排除数据的一个子集上执行特殊查询。  在疑难解答期间，数据的子集是第一次从日志，通常按时间序列。  然后，深入执行筛选数据集与错误级别或错误消息。 这是在将事件日志存储在 DocumentDB 中是一大优势。 日志数据存储在 DocumentDB 中，默认情况下，将自动设置索引，这样就可以随时查询。 此外，可以为一次系列跨数据分区保持日志数据。 较旧的日志可以分发给每个保留策略的冷藏。          

第二个用例需要长时间运行的数据脱机执行通过大量日志数据的分析作业。  这一使用情形的示例包括服务器可用性分析、 错误分析的应用程序，然后点击流数据分析。  通常，使用 Hadoop 来执行这些类型的分析。  与 Hadoop 连接器 DocumentDB 的 DocumentDB 数据库作为数据源和接收器猪、 配置单元和地图/减少作业。 用于 DocumentDB 的 Hadoop 连接器的详细信息，请参阅[运行 Hadoop 作业与 DocumentDB 和 HDInsight](documentdb-run-hadoop-with-hdinsight.md)。      

## <a name="gaming"></a>玩游戏
数据库层是游戏应用程序的关键组件。 现代游戏移动/控制台的客户端上执行图形处理，但依赖于云后，以提供自定义和个性化的内容，如在游戏统计、 社交媒体集成和高分数排名。 游戏要求极低的延迟读取的写操作提供合作游戏体验，和数据库层需要处理在新游戏的发布和功能更新过程中的上限值和下限中请求速率。

DocumentDB 由类似的大规模的游戏[遍历死︰ 无人的土地](https://azure.microsoft.com//blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/)的[下一步的游戏](http://www.nextgames.com/)，和[光环 5︰ 监护人](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/)。 在使用案例中，DocumentDB 的主要优点是如下︰

- DocumentDB 允许进行缩放的性能向上或向下弹性。 这将允许通过单个 API 调用处理更新配置文件和统计数字从几十到数以百万计的同时进行游戏玩家的游戏。
- DocumentDB 支持毫秒读取，并将有助于避免在玩游戏过程中任何滞后。
- DocumentDB 的自动索引允许筛选针对实时的多个不同的属性，例如找到玩家通过其内部的玩家 Id，或他们 GameCenter，Facebook、 Google Id 或播放机在协会的成员资格所基于的查询。 这是无需构建复杂的索引或分片的基础结构。
- 社会功能包括游戏中聊天消息、 播放机协会成员、 完成挑战，高分排名和社交关系图是容易实现灵活的架构。
- DocumentDB 为托管平台作为-服务 (PaaS) 所需最少的设置和管理工作，以允许快速的迭代和缩短上市时间。


## <a name="user-preferences-data"></a>用户首选项数据
如今，大多数的现代 web 和移动应用程序附带有复杂的视图和体验。 这些视图和体验是通常动态，适应用户首选项或情绪和品牌形象的需要。  因此，应用程序需要能够有效地检索个性化的设置，以快速呈现用户界面元素和体验。 

JSON 是一种有效的格式来表示用户界面的布局数据，因为它不仅是轻量的但也可以很容易地通过 JavaScript 解释。  DocumentDB 提供了允许快速读取与低延迟写入操作的可调式的一致性级别。 因此，在 DocumentDB 中包括作为 JSON 文档的个性化的设置的 UI 布局数据存储是一种有效的方法来获取该数据通过电缆。

## <a name="iot-and-device-sensor-data"></a>IoT 和设备传感器数据
IoT 用例通常共享它们摄取、 处理和存储数据的方式的一些模式。  首先，这些系统，可以接收突发的数据从设备的各种语言环境的传感器的数据实习机会。  接下来，这些系统处理和分析实时理解的流数据。 最后但并不容忽视的一点，大多数，如果不，所有数据最终将都停放在即席查询和离线分析的数据存储区中。    

Microsoft Azure 提供丰富的可供 IoT 的服务用例。  Azure IoT 服务是一套包括 Azure 事件集线器、 Azure DocumentDB，Azure 流分析，Azure 通知中心，Azure 机器学习，Azure HDInsight 和 PowerBI 的服务。 

猝发的数据可以通过 Azure 事件集线器 ingested，因为它提供高吞吐量的数据接收和低延迟时间。 为实时分析，可以到 Azure 流分析 funneled ingested 的数据，需要处理的实时时间洞察力。 数据可以加载到 DocumentDB 中，即席查询。 将数据加载到 DocumentDB 时，数据就可以进行查询。  DocumentDB 中的数据可以用作引用数据作为实时分析的一部分。 另外，可以进一步优化和数据由小猪、 配置单元或地图/减少作业连接到 HDInsight 的 DocumentDB 数据处理。  细化的数据然后加载回 DocumentDB 报告。   

有关示例 IoT 解决方案中使用 DocumentDB、 EventHubs 和风暴， [GitHub 上的风暴的 hdinsight 示例存储库](https://github.com/hdinsight/hdinsight-storm-examples/)。

Azure IoT 产品的详细信息，请参阅[创建互联网的您的事情](http://www.microsoft.com/en-us/server-cloud/internet-of-things.aspx)。

## <a name="next-steps"></a>下一步行动
 
若要开始使用 DocumentDB，可以创建一个[帐户](https://azure.microsoft.com/pricing/free-trial/)，然后按照我们[学习路径](https://azure.microsoft.com/documentation/learning-paths/documentdb/)若要了解有关 DocumentDB 和查找您需要的信息。 

或者，如果您想要阅读更多有关使用 DocumentDB 的客户，以下客户案例有︰

- [下一步的游戏](https://azure.microsoft.com//blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/)。 Walking 死尸︰ No Man's 土地游戏上涨受 Azure DocumentDB #1。
- [光晕](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/)。 光环 5 实现使用 Azure DocumentDB 社会玩游戏的方式。
- [Cortana 分析库](https://azure.microsoft.com/blog/cortana-analytics-gallery-a-scalable-community-site-built-on-azure-documentdb/)。 Cortana 分析库-基于 Azure DocumentDB 构建可伸缩的社区网站。
- [Breeze](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18602)。 在几分钟内，用灵活的云技术领先的系统集成商提供多国公司全局的洞察力。
- [新闻共和国](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18639)。 将智能添加到为参与市民提供信息与目的的新闻。 
- [SGS 国际](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18653)。 在全球范围内的颜色一致，主要品牌转向 SGS。 然后，SGS 翻到 Azure。
- [Telenor](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18608)。 全球领先 Telenor 使用云的启动速度的速度移动。 
- [XOMNI](https://customers.microsoft.com/Pages/CustomerStory.aspx?recid=18667)。 未来的存储区运行快速搜索和数据的流动。
