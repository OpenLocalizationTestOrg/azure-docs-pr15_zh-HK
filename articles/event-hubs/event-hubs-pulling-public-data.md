<properties
    pageTitle="拉到 Azure 事件集线器的公共数据 |Microsoft Azure"
    description="从 web 示例导入事件集线器的概述"
    services="event-hubs"
    documentationCenter="na"
    authors="spyrossak"
    manager="timlt"
    editor=""/>

<tags 
    ms.service="event-hubs"
    ms.devlang="na"
    ms.topic="article"
    ms.tgt_pltfrm="na"
    ms.workload="na"
    ms.date="08/25/2016"
    ms.author="spyros;sethm" />

# <a name="pulling-public-data-into-azure-event-hubs"></a>将公用数据获取到 Azure 事件集线器

通常互联网内容 (IoT) 情况下，您有您可以通过编程将数据推到 Azure，要么到 Azure 事件中心或 IoT 集线器的设备。 这两个这些集线器进行存储、 分析和可视化与形形色色的工具可在 Microsoft Azure 上都到 Azure 的入口点。 但是，它们都需要将数据推送到它们，格式化为 JSON 并以特定的方式保护。 这将打开下面的问题。 如果您想要将数据导入来自公共或私人来源位置数据公开为 web 服务或进某些类型，但您没有更改发布数据的方式的能力，您该怎么办？ 请考虑天气或通信，或股票报价-无法判断 NOAA，或 WSDOT，或者纳斯达克将配置推送到事件中心。 若要解决此问题，我们已经编写并开放源代码的小云示例，您可以修改和部署将从一些这种源中提取数据并将其推到事件中心。 从那里，您可以随心所欲地使用它，主题，当然，为从制造者的许可条款。 应用程序可以找到[这里](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-importfromweb/)。

请注意，此示例中的代码只显示如何拉从典型的 web 数据源，以及如何编写在 Azure 事件集线器。 这不是一个生产应用程序中，并没有尝试以使其适合用于在这样一个环境-它 strictfly DIY，专注于开发人员的示例。 此外，此示例的存在不是于 Azure，而不是**推**到，应**提取**数据的建议让它。 应检查安全、 性能和功能，并前安定在一个端到端体系结构的成本因素。

## <a name="application-structure"></a>应用程序结构

编写应用程序 C#，并[示例说明](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-importfromweb/)包含要修改、 构建和发布应用程序所需的所有信息。 以下各节提供了有关应用程序的高级概述。

我们首先假设您已对数据源的访问。 例如，可能希望拉在华盛顿状态 / 自治区交通部门，从流量数据或从 NOAA，显示自定义报表，或将这些数据与其他应用程序中的数据组合在一起的天气数据。 您还需要设置 Azure 的事件集线器，并且知道连接字符串需要对其进行访问。

GenericWebToEH 解决方案启动时，它读取一个配置文件 (App.config) 来获取大量的事物︰

1. URL 或发布数据的站点的 Url 列表。 理想情况下，这是 json 格式，发布数据，如 WSDOT 所引用的网站[此处](http://www.wsdot.wa.gov/Traffic/api/)。 
2. 对于 URL，如果所需的凭据。 许多公共资源不需要凭据，也可以将凭据放在 URL 字符串中。 另外一些需要单独提供。 （请注意，您可以只指定一组凭据在此应用程序，因此它才起作用如果您指定一个 URL，不是 Url 的列表）。
3. 连接字符串，在该事件集线器命名空间，您会将数据推送到事件中心的名称。 您可以在 Azure 门户中找到此信息。
4. 睡眠间隔，以毫秒为单位，公共数据站点轮询之间的时间间隔。 此设置需要思考一下。 如果轮询频率过低，您可能会遗漏数据;另一方面，如果轮询频率过高，您可能会收到大量的重复数据，或更糟糕的是，您可能会被禁止作为邪恶机器人。 请考虑更新数据源的频率-天气或流量数据进行刷新每隔 15 分钟，但股票行情可能每隔几秒钟，取决于您是从哪里获得这些。 
5. 一个标志，告诉应用程序是否将数据作为 JSON 或 XML 进入。 您需要将数据推送到事件的中心，因为应用程序在发送之前将 XML 转换为 JSON 的模块。

阅读后的配置文件，应用程序将进入循环-访问公共网站上，将转换的数据，如有必要，将它写入到事件网络集线器，，然后等待睡眠间隔，此前它从头再次。 具体来说︰

  * 阅读公共网站。 用于接收数据的准备与发送从 Azure/GenericWebToEH/ApiReaders/RawXMLWithHeaderToJsonReader.cs 使用 RawXMLWithHeaderToJsonReader 类的实例。 它读取源流在 GetData() 方法中，然后进行拆分为更小的部分 （即记录） 使用 GetXmlFromOriginalText。 
  此方法将读取 XML 和 JSON 格式良好的或 JSON 数组。 然后处理使用 MergeToXML 配置从 App.config 启动时 (默认值为空)。
  * 作为 Program.cs 的 Process() 方法中的循环来实现接收和发送数据。 
  从 GetData() 接收输出结果后, 方法排队分隔值到该事件的中心。

## <a name="next-steps"></a>下一步行动

部署解决方案，克隆或[GenericWebToEH](https://azure.microsoft.com/documentation/samples/event-hubs-dotnet-importfromweb/)应用程序下载、 编辑的 App.config 文件、 生成，并最后将其发布。 一旦您已发布的应用程序，您可以看到它在 Azure 经典门户在云服务中运行，您可以在**配置**选项卡中更改某些配置设置 （如事件中心目标和睡眠间隔）。

请参阅更多的事件集线器样本[Azure 示例库](https://azure.microsoft.com/documentation/samples/?service=event-hubs)中并在[MSDN](https://code.msdn.microsoft.com/site/search?query=event%20hubs&f%5B0%5D.Value=event%20hubs&f%5B0%5D.Type=SearchText&ac=5)上。
