<properties
   pageTitle="Apache 风暴拓扑与 Visual Studio 和 C# |Microsoft Azure"
   description="了解如何在 C# 中创建风暴拓扑结构，通过在 Visual Studio 对 Visual Studio 中使用 HDInsight 工具创建一个简单的单词计数拓扑。"
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
   ms.date="10/27/2016"
   ms.author="larryfr"/>

# <a name="develop-c-topologies-for-apache-storm-on-hdinsight-using-hadoop-tools-for-visual-studio"></a>对于使用 Visual Studio 工具 Hadoop HDInsight 上的 Apache 风暴开发 C# 拓扑

了解如何通过使用 HDInsight 工具 Visual Studio 创建 C# 风暴拓扑。 本教程将带领通过在 Visual Studio 中创建一个新的风暴项目、 本地对其进行测试并将其部署到 HDInsight 群集上的 Apache 风暴的过程中。

您还将学习如何创建混合拓扑中使用 C# 和 Java 组件。

> [AZURE.IMPORTANT] 虽然本文档中的步骤依赖于 Windows 使用 Visual Studio 开发环境，已编译的项目可以提交到的 Linux 或基于 Windows HDInsight 群集。 只有基于 Linux 的群集创建后 10/28/2016年支持 SCP.NET 拓扑。
>
> 要基于 Linux 的群集使用 C# 拓扑结构，您必须更新用于您的项目版本 0.10.0.6 或更高版本的 Microsoft.SCP.Net.SDK NuGet 程序包。 软件包的版本还必须匹配的主版本的安装在 HDInsight 上的冲击。 例如，HDInsight 版本 3.3 和 3.4 风暴使用风暴版本 0.10.x，而 HDInsight 3.5 使用风暴 1.0.x 版。
> 
> C# 基于 Linux 的群集拓扑结构必须使用.NET 4.5 中，并使用单声道 HDInsight 群集上运行。 大多数操作起作用，但是您应该检查可能不兼容问题的[单声道兼容性](http://www.mono-project.com/docs/about-mono/compatibility/)文档。

## <a name="prerequisites"></a>系统必备组件

- 下面的 Visual Studio 版本之一

    - Visual Studio 2012 与[更新 4](http://www.microsoft.com/download/details.aspx?id=39305)

    - Visual Studio 2013 年[更新 4](http://www.microsoft.com/download/details.aspx?id=44921)或[Visual Studio 2013年社区](http://go.microsoft.com/fwlink/?LinkId=517284)

    - Visual Studio 2015年或[Visual Studio 2015年社区](https://go.microsoft.com/fwlink/?LinkId=532606)

- Azure SDK 2.9.5 或更高版本

- Visual Studio 的 HDInsight 工具︰ 请参阅[开始使用 Visual Studio 的 HDInsight 工具](hdinsight-hadoop-visual-studio-tools-get-started.md)来安装和配置 Visual Studio 的 HDInsight 工具。

    > [AZURE.NOTE] Visual Studio 的 HDInsight 工具不支持 Visual Studio 速成版

-   Apache HDInsight 群集上的冲击︰[在 HDInsight 上的 Apache 风暴入门](hdinsight-apache-storm-tutorial-get-started.md)步骤创建群集，请参阅。

## <a name="templates"></a>模板

Visual Studio 的 HDInsight 工具提供下列模板︰

| 项目类型 | 演示 |
| ------------ | ------------- |
| 风暴的应用程序 | 一个空的风暴拓扑结构项目 |
| 风暴的 Azure SQL 编写器示例 | 如何写入 SQL Azure 数据库 |
| 暴 DocumentDB 读取器示例 | 如何读取从 Azure DocumentDB |
| 风暴的 DocumentDB 编写器示例 | 如何编写到 Azure DocumentDB |
| 暴 EventHub 读取器示例 | 如何读取从 Azure 事件集线器 |
| 风暴的 EventHub 编写器示例 | 如何编写到 Azure 事件集线器 |
| 冲击 HBase 读取器示例 | 如何读取 HBase HDInsight 群集上 |
| 冲击 HBase 编写器示例 | 如何在 HDInsight 群集上写入 HBase |
| 风暴的混合示例 | 如何使用 Java 组件 |
| 风暴的示例 | 基本的单词计数拓扑 |

> [AZURE.NOTE] HBase 读取器和编写器示例使用 HBase REST API 与 HBase HDInsight 群集，不 HBase Java API 上进行通信。

在此文档中的步骤，将使用基本的风暴应用程序项目类型创建一个新的拓扑结构。

## <a name="create-a-c-topology"></a>创建一个 C# 拓扑

1. 如果有 HDInsight 工具的最新版本的 Visual Studio 安装，请参阅[开始使用 Visual Studio 的 HDInsight 工具](hdinsight-hadoop-visual-studio-tools-get-started.md)。

2. 打开 Visual Studio 中，选择**文件** > **新建**和**项目**。

3. 从**新建项目**屏幕中，展开**安装** > **模板**，并选择**HDInsight**。 从模板列表中，选择**风暴应用程序**。 在屏幕的底部，为应用程序的名称输入**字数统计**。

    ![图像](./media/hdinsight-storm-develop-csharp-visual-studio-topology/new-project.png)

4. 在创建项目之后，您应具有下列文件︰

    - **Program.cs**︰ 这将定义您的项目的拓扑。 注意默认情况下将创建一个管口和一个螺栓组成一个默认拓扑结构。

    - **Spout.cs**︰ 发出随机数字示例作为管口。

    - **Bolt.cs**: 示例螺栓使发射管口的数字个数。

    为项目创建的一部分，将从 NuGet 下载最新的[SCP.NET 软件包](https://www.nuget.org/packages/Microsoft.SCP.Net.SDK/)。

    [AZURE.INCLUDE [scp.net version important](../../includes/hdinsight-storm-scpdotnet-version.md)]

在下一节中，您将到基本的字数统计应用程序修改此项目。

### <a name="implement-the-spout"></a>实现管口

1. 打开**Spout.cs**。 Spouts 用于从外部源读取拓扑中的数据。 对管口的主要组成部分是︰

    - **NextTuple**︰ 由风暴，当管口可以发出新的元组。

    - **肯定应答**（仅适用于事务拓扑）︰ 处理由来自此管口的元组的拓扑中的其他组件的确认。 确认一个元组，可以知道程序已被成功处理由下游组件管口。

    - **失败**（仅适用于事务拓扑）︰ 处理的是的故障处理拓扑结构中的其他组件的元组。 这将提供机会，以便它可以再次处理重新发出该元组。

2. **Spout**类的内容替换为以下。 这将创建随机发出到拓扑结构中的一个句子作为管口。

        private Context ctx;
        private Random r = new Random();
        string[] sentences = new string[] {
            "the cow jumped over the moon",
            "an apple a day keeps the doctor away",
            "four score and seven years ago",
            "snow white and the seven dwarfs",
            "i am at two with nature"
        };

        public Spout(Context ctx)
        {
            // Set the instance context
            this.ctx = ctx;

            Context.Logger.Info("Generator constructor called");

            // Declare Output schema
            Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
            // The schema for the default output stream is
            // a tuple that contains a string field
            outputSchema.Add("default", new List<Type>() { typeof(string) });
            this.ctx.DeclareComponentSchema(new ComponentStreamSchema(null, outputSchema));
        }

        // Get an instance of the spout
        public static Spout Get(Context ctx, Dictionary<string, Object> parms)
        {
            return new Spout(ctx);
        }

        public void NextTuple(Dictionary<string, Object> parms)
        {
            Context.Logger.Info("NextTuple enter");
            // The sentence to be emitted
            string sentence;

            // Get a random sentence
            sentence = sentences[r.Next(0, sentences.Length - 1)];
            Context.Logger.Info("Emit: {0}", sentence);
            // Emit it
            this.ctx.Emit(new Values(sentence));

            Context.Logger.Info("NextTuple exit");
        }

        public void Ack(long seqId, Dictionary<string, Object> parms)
        {
            // Only used for transactional topologies
        }

        public void Fail(long seqId, Dictionary<string, Object> parms)
        {
            // Only used for transactional topologies
        }
    
    花点时间通读的注释来理解这段代码的执行。

### <a name="implement-the-bolts"></a>实现螺栓

1. 从项目中删除现有的**Bolt.cs**文件。

2. 在**解决方案资源管理器**中右键单击项目，然后选择**添加** > **新项**。 从列表中选择**冲击螺栓**，并输入**Splitter.cs**作为名称。 重复此操作可创建第二个螺栓命名的**Counter.cs**。

    - **Splitter.cs**︰ 实现一个螺栓，将句子分割成各个单词并发出新的字流。

    - **Counter.cs**︰ 实现一个螺栓，统计每个单词，并发出新的单词和每个字计数的流。

    > [AZURE.NOTE] 这些螺栓简单地读取和写入流，但您可以使用一个螺栓与源，例如数据库或服务进行通信。

3. 打开**Splitter.cs**。 请注意，它有默认情况下只有一个方法︰**执行**。 这被称为螺栓收到的元组进行处理。 在这里，您可以读取和处理传入的元组，并发出出站的元组。

4. **拆分器**类的内容替换为以下代码︰

        private Context ctx;

        // Constructor
        public Splitter(Context ctx)
        {
            Context.Logger.Info("Splitter constructor called");
            this.ctx = ctx;

            // Declare Input and Output schemas
            Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
            // Input contains a tuple with a string field (the sentence)
            inputSchema.Add("default", new List<Type>() { typeof(string) });
            Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
            // Outbound contains a tuple with a string field (the word)
            outputSchema.Add("default", new List<Type>() { typeof(string) });
            this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
        }

        // Get a new instance of the bolt
        public static Splitter Get(Context ctx, Dictionary<string, Object> parms)
        {
            return new Splitter(ctx);
        }

        // Called when a new tuple is available
        public void Execute(SCPTuple tuple)
        {
            Context.Logger.Info("Execute enter");

            // Get the sentence from the tuple
            string sentence = tuple.GetString(0);
            // Split at space characters
            foreach (string word in sentence.Split(' '))
            {
                Context.Logger.Info("Emit: {0}", word);
                //Emit each word
                this.ctx.Emit(new Values(word));
            }

            Context.Logger.Info("Execute exit");
        }

    花点时间通读的注释来理解这段代码的执行。

5. 打开**Counter.cs** ，用以下内容替换类内容︰

        private Context ctx;

        // Dictionary for holding words and counts
        private Dictionary<string, int> counts = new Dictionary<string, int>();

        // Constructor
        public Counter(Context ctx)
        {
            Context.Logger.Info("Counter constructor called");
            // Set instance context
            this.ctx = ctx;

            // Declare Input and Output schemas
            Dictionary<string, List<Type>> inputSchema = new Dictionary<string, List<Type>>();
            // A tuple containing a string field - the word
            inputSchema.Add("default", new List<Type>() { typeof(string) });

            Dictionary<string, List<Type>> outputSchema = new Dictionary<string, List<Type>>();
            // A tuple containing a string and integer field - the word and the word count
            outputSchema.Add("default", new List<Type>() { typeof(string), typeof(int) });
            this.ctx.DeclareComponentSchema(new ComponentStreamSchema(inputSchema, outputSchema));
        }

        // Get a new instance
        public static Counter Get(Context ctx, Dictionary<string, Object> parms)
        {
            return new Counter(ctx);
        }

        // Called when a new tuple is available
        public void Execute(SCPTuple tuple)
        {
            Context.Logger.Info("Execute enter");

            // Get the word from the tuple
            string word = tuple.GetString(0);
            // Do we already have an entry for the word in the dictionary?
            // If no, create one with a count of 0
            int count = counts.ContainsKey(word) ? counts[word] : 0;
            // Increment the count
            count++;
            // Update the count in the dictionary
            counts[word] = count;

            Context.Logger.Info("Emit: {0}, count: {1}", word, count);
            // Emit the word and count information
            this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new List<SCPTuple> { tuple }, new Values(word, count));
            Context.Logger.Info("Execute exit");
        }

    花点时间通读的注释来理解这段代码的执行。

### <a name="define-the-topology"></a>定义拓扑

Spouts 和螺栓排列关系图，它定义数据的组件之间的流动方式。 对于此拓扑中，关系图如下所示︰

![映像的组件的排列方式。](./media/hdinsight-storm-develop-csharp-visual-studio-topology/wordcount-topology.png)

其中分配给实例的拆分器螺栓从管口，发出句子。 拆分器螺栓分成将分发给计数器螺栓的单词的句子。

由于字数统计本地持有该计数器实例中，我们需要确保特定的词流相同的计数器螺栓实例，因此我们只能有一个实例跟踪的特定单词。 但为拆分器螺栓，它确实无关紧要的螺栓接收哪些句子，因此我们只是希望通过这些实例加载平衡句子。

打开**Program.cs**。 最重要的方法是**GetTopologyBuilder**，用来定义拓扑提交到风暴。 **GetTopologyBuilder**中的内容替换为以下代码，以实现前面描述的拓扑︰

        // Create a new topology named 'WordCount'
        TopologyBuilder topologyBuilder = new TopologyBuilder("WordCount" + DateTime.Now.ToString("yyyyMMddHHmmss"));

        // Add the spout to the topology.
        // Name the component 'sentences'
        // Name the field that is emitted as 'sentence'
        topologyBuilder.SetSpout(
            "sentences",
            Spout.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"sentence"}}
            },
            1);
        // Add the splitter bolt to the topology.
        // Name the component 'splitter'
        // Name the field that is emitted 'word'
        // Use suffleGrouping to distribute incoming tuples
        //   from the 'sentences' spout across instances
        //   of the splitter
        topologyBuilder.SetBolt(
            "splitter",
            Splitter.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"word"}}
            },
            1).shuffleGrouping("sentences");

        // Add the counter bolt to the topology.
        // Name the component 'counter'
        // Name the fields that are emitted 'word' and 'count'
        // Use fieldsGrouping to ensure that tuples are routed
        //   to counter instances based on the contents of field
        //   position 0 (the word). This could also have been
        //   List<string>(){"word"}.
        //   This ensures that the word 'jumped', for example, will always
        //   go to the same instance
        topologyBuilder.SetBolt(
            "counter",
            Counter.Get,
            new Dictionary<string, List<string>>()
            {
                {Constants.DEFAULT_STREAM_ID, new List<string>(){"word", "count"}}
            },
            1).fieldsGrouping("splitter", new List<int>() { 0 });

        // Add topology config
        topologyBuilder.SetTopologyConfig(new Dictionary<string, string>()
        {
            {"topology.kryo.register","[\"[B\"]"}
        });

        return topologyBuilder;

花点时间通读的注释来理解这段代码的执行。

## <a name="submit-the-topology"></a>提交了拓扑

1. 在**解决方案资源管理器**中用鼠标右键单击该项目，然后选择**提交到 HDInsight 上的冲击**。

    > [AZURE.NOTE] 如果出现提示，请输入 Azure 订阅的登录凭据。 如果您有多个订阅，登录到包含您在 HDInsight 群集上的冲击。

2. 从**风暴群集**下拉列表中，选择您在 HDInsight 群集上的风暴，然后选择**提交**。 您可以监视提交是否成功，通过使用**输出**窗口。

3. 已成功提交拓扑结构，应显示为群集的**风暴拓扑**。 从列表以查看有关正在运行的拓扑信息选择**WordCount**拓扑。

    > [AZURE.NOTE] 您还可以查看从**服务器资源管理器**的**电流冲击拓扑**︰ 展开**Azure** > **HDInsight**，HDInsight 群集上的风暴用鼠标右键单击，然后选择**视图风暴拓扑**。

    使用 spouts 或螺栓的链接可以查看有关这些组件的信息。 对于每个选定的项将打开一个新窗口。

4. **拓扑摘要**视图中，单击**取消**停止拓扑结构。

    > [AZURE.NOTE] 风暴的拓扑继续运行，直到它们处于非活动状态，或群集已被删除。

## <a name="transactional-topology"></a>事务处理的拓扑结构

以前的拓扑结构为非事务性队列。 拓扑中的组件不实现任何处理拓扑中的组件出现故障的情况下重放消息的功能。 事务拓扑示例，创建一个新项目，作为项目类型选择**冲击示例**。

事务处理拓扑结构实现以下操作以支持数据的重放︰

- **元数据缓存**︰ 作为管口必须存储有关数据发出，以便能够检索并发出再次发生故障时数据的元数据。 因为小示例发出的数据，每个元组的原始数据存储在重播的字典。

- **Ack**︰ 拓扑结构中的每个螺栓可以调用`this.ctx.Ack(tuple)`到确认它已成功地处理一个元组。 当所有螺栓都具有请求的元组，`Ack`的管口的方法被调用。 这使管口去重播的缓存的数据，因为未完全处理的数据。

- **失败**︰ 每个螺栓可以调用`this.ctx.Fail(tuple)`来指示处理未能为元组。 故障传播到了`Fail`的管口，其中可以通过重播该元组的方法缓存的元数据。

- **规则 ID**︰ 当发出一个元组，可以指定一个序列 ID。 这应该是一个值，标识处理重播 （Ack 和失败） 的元组。 例如，作为管口**风暴示例**项目中的使用以下时发出的数据︰

        this.ctx.Emit(Constants.DEFAULT_STREAM_ID, new Values(sentence), lastSeqId);

    这会发出新的元组包含**lastSeqId**中包含的序列 ID 值为默认流中，一个句子。 在本例中， **lastSeqId**在发出每个元组的只是会增加。

**冲击示例**项目中所示，事务性组件是否可以在运行时设置，根据配置。

## <a name="hybrid-topology"></a>混合拓扑

Visual Studio 的 HDInsight 工具还可以用于创建混合的拓扑，其中某些组件的 C# 和其他人是 Java。

对于混合拓扑示例，创建一个新项目并选择**冲击混合样本**。 这将创建包含说明以下的几种拓扑结构的完全注释的示例︰

- **Java spout**和**C# 螺栓**︰ 在**HybridTopology_javaSpout_csharpBolt**中定义

    - 在**HybridTopologyTx_javaSpout_csharpBolt**中定义事务的版本

- **C# spout**和**Java 螺栓**︰ 在**HybridTopology_csharpSpout_javaBolt**中定义

    - 在**HybridTopologyTx_csharpSpout_javaBolt**中定义事务的版本

        > [AZURE.NOTE] 此版本还演示了如何从文本文件中使用 Clojure 代码，作为一个 Java 组件。

要提交项目时使用的拓扑之间切换，只需移动`[Active(true)]`语句要再将其提交到群集使用的拓扑。

> [AZURE.NOTE] 作为**JavaDependency**文件夹中的此项目的一部分提供了所需的所有 Java 文件。

下面创建和提交一个混合拓扑时，请考虑︰

- 必须使用**JavaComponentConstructor**来创建作为管口或螺栓的 Java 类的新实例。

- **microsoft.scp.storm.multilang.CustomizedInteropJSONSerializer**用于序列化中的数据或传出从 Java 对象到 JSON 的 Java 组件。

- 当提交到服务器的拓扑结构，必须使用**其他配置**选项来指定**Java 文件的路径**。 指定的路径应包含包含 Java 类的 JAR 文件的目录。

### <a name="azure-event-hubs"></a>Azure 事件集线器

SCP.Net 版本 0.9.4.203 引入了一个新的类和方法专门用于处理事件中心 Spout （Java 管口，读取事件中心。）在创建时使用此管口的拓扑结构，使用以下方法︰

- **EventHubSpoutConfig**类︰ 创建一个对象，包含配置的管口组件

- **TopologyBuilder.SetEventHubSpout**方法︰ 将事件中心 Spout 的组件添加到拓扑结构中

> [AZURE.NOTE] 虽然这些方便使用比其他 Java 组件事件中心 Spout，您仍必须使用 CustomizedInteropJSONSerializer 序列化数据产生的管口。

## <a id="configurationmanager"></a>使用 ConfigurationManager

不使用检索配置值中螺栓的 ConfigurationManager 和 spout 组件;这将导致一个空指针异常。 相反，您的项目的配置传递到风暴的拓扑结构作为拓扑环境中的键/值对。 依赖于配置值的每个组件必须检索它们从上下文初始化期间。

下面的代码演示如何检索这些值︰

    public class MyComponent : ISCPBolt
    {
        // To hold configuration information loaded from context
        Configuration configuration;
        ...
        public MyComponent(Context ctx, Dictionary<string, Object> parms)
        {
            // Save a copy of the context for this component instance
            this.ctx = ctx;
            // If it exists, load the configuration for the component
            if(parms.ContainsKey(Constants.USER_CONFIG))
            {
                this.configuration = parms[Constants.USER_CONFIG] as System.Configuration.Configuration;
            }
            // Retrieve the value of "Foo" from configuration
            var foo = this.configuration.AppSettings.Settings["Foo"].Value;
        }
        ...
    }

如果您使用`Get`方法返回实例的组件，必须确保它可以通过两个`Context`和`Dictionary<string, Object>`构造函数的参数。 下面的示例是一个简单`Get`正确传递这些值的方法︰

    public static MyComponent Get(Context ctx, Dictionary<string, Object> parms)
    {
        return new MyComponent(ctx, parms);
    }

## <a name="how-to-update-scpnet"></a>如何更新 SCP.NET

最新版本的 SCP.NET 支持通过 NuGet 程序包升级。 当有新的更新可用时，您将收到升级通知。 要手动检查升级，请执行以下步骤︰

1. 在**解决方案资源管理器**中右击该项目，然后选择**管理 NuGet 程序包**。

2. 从软件包管理器中，选择**更新**。 如果有可用的更新，它将列出。 单击要安装的包**更新**按钮。

> [AZURE.IMPORTANT] 如果您的项目与一个未使用 NuGet 程序包更新的 SCP.NET 的早期版本创建的则必须执行以下步骤来更新到最新版本︰
>
> 1. 在**解决方案资源管理器**中右击该项目，然后选择**管理 NuGet 程序包**。
> 2. 使用**搜索**字段，请搜索，然后将添加到项目中的**Microsoft.SCP.Net.SDK** 。

## <a name="troubleshooting"></a>故障排除

### <a name="null-pointer-exceptions"></a>空指针异常

当 C# 拓扑中使用基于 Linux 的 HDInsight 群集，螺栓，spout 使用 ConfigurationManager 来读取配置设置在运行时可能会返回 null 指针异常的组件。 这是因为加载的域的配置文件不包含您的项目的程序集。

为项目配置在拓扑中，键/值对形式传递到风暴的拓扑结构，可以从它们初始化时传递给您的组件的字典对象检索。

下面的示例演示如何从拓扑结构上下文中加载的配置值，请参阅本文档的[ConfigurationManager](#configurationmanager)部分。

### <a name="systemtypeloadexception"></a>System.TypeLoadException

当使用基于 Linux 的 HDInsight 群集中使用 C# 拓扑结构，您可能会遇到以下错误︰

    System.TypeLoadException: Failure has occurred while loading a type.

这通常发生在使用二进制文件时不是与单声道支持的.NET 版本兼容。

对于基于 Linux 的 HDInsight 群集，您应确保您的项目使用.NET 4.5 为编译的二进制文件。


### <a name="test-a-topology-locally"></a>测试本地拓扑

虽然很容易将一个拓扑部署到群集中，在某些情况下，您可能需要测试本地的拓扑结构。 使用以下步骤运行和测试在开发环境中本地本教程的示例拓扑。

> [AZURE.WARNING] 本地测试仅适用于基本的 C# 的唯一拓扑。 不应使用本地测试混合拓扑结构或拓扑中使用多个流，则会收到错误消息。

1. 在**解决方案资源管理器**中用鼠标右键单击该项目，然后选择**属性**。 在项目属性中，将**输出类型**更改为**控制台应用程序**。

    ![输出类型](./media/hdinsight-storm-develop-csharp-visual-studio-topology/outputtype.png)

    > [AZURE.NOTE] 请记住要将**输出类型**更改回**类库**，先到的群集部署的拓扑。

2. 在**解决方案资源管理器**中用鼠标右键单击该项目，然后选择**添加** > **新项**。 选择**类**，输入**LocalTest.cs**作为类名。 最后，单击**添加**。

3. 打开**LocalTest.cs** ，然后将以下**using**语句添加在顶部︰

        using Microsoft.SCP;

4. 使用以下内容作为**LocalTest**类的内容︰

        // Drives the topology components
        public void RunTestCase()
        {
            // An empty dictionary for use when creating components
            Dictionary<string, Object> emptyDictionary = new Dictionary<string, object>();

            #region Test the spout
            {
                Console.WriteLine("Starting spout");
                // LocalContext is a local-mode context that can be used to initialize
                // components in the development environment.
                LocalContext spoutCtx = LocalContext.Get();
                // Get a new instance of the spout, using the local context
                Spout sentences = Spout.Get(spoutCtx, emptyDictionary);

                // Emit 10 tuples
                for (int i = 0; i < 10; i++)
                {
                    sentences.NextTuple(emptyDictionary);
                }
                // Use LocalContext to persist the data stream to file
                spoutCtx.WriteMsgQueueToFile("sentences.txt");
                Console.WriteLine("Spout finished");
            }
            #endregion

            #region Test the splitter bolt
            {
                Console.WriteLine("Starting splitter bolt");
                // LocalContext is a local-mode context that can be used to initialize
                // components in the development environment.
                LocalContext splitterCtx = LocalContext.Get();
                // Get a new instance of the bolt
                Splitter splitter = Splitter.Get(splitterCtx, emptyDictionary);


                // Set the data stream to the data created by the spout
                splitterCtx.ReadFromFileToMsgQueue("sentences.txt");
                // Get a batch of tuples from the stream
                List<SCPTuple> batch = splitterCtx.RecvFromMsgQueue();
                // Process each tuple in the batch
                foreach (SCPTuple tuple in batch)
                {
                    splitter.Execute(tuple);
                }
                // Use LocalContext to persist the data stream to file
                splitterCtx.WriteMsgQueueToFile("splitter.txt");
                Console.WriteLine("Splitter bolt finished");
            }
            #endregion

            #region Test the counter bolt
            {
                Console.WriteLine("Starting counter bolt");
                // LocalContext is a local-mode context that can be used to initialize
                // components in the development environment.
                LocalContext counterCtx = LocalContext.Get();
                // Get a new instance of the bolt
                Counter counter = Counter.Get(counterCtx, emptyDictionary);

                // Set the data stream to the data created by splitter bolt
                counterCtx.ReadFromFileToMsgQueue("splitter.txt");
                // Get a batch of tuples from the stream
                List<SCPTuple> batch = counterCtx.RecvFromMsgQueue();
                // Process each tuple in the batch
                foreach (SCPTuple tuple in batch)
                {
                    counter.Execute(tuple);
                }
                // Use LocalContext to persist the data stream to file
                counterCtx.WriteMsgQueueToFile("counter.txt");
                Console.WriteLine("Counter bolt finished");
            }
            #endregion
        }

    花点时间通读代码注释。 此代码使用**LocalContext**运行的组件在开发环境中，并且会一直持续到本地驱动器上的文本文件的组件之间的数据流。

5. 打开**Program.cs** ，并将以下内容添加到**Main**方法︰

        Console.WriteLine("Starting tests");
        System.Environment.SetEnvironmentVariable("microsoft.scp.logPrefix", "WordCount-LocalTest");
        // Initialize the runtime
        SCPRuntime.Initialize();

        //If we are not running under the local context, throw an error
        if (Context.pluginType != SCPPluginType.SCP_NET_LOCAL)
        {
            throw new Exception(string.Format("unexpected pluginType: {0}", Context.pluginType));
        }
        // Create test instance
        LocalTest tests = new LocalTest();
        // Run tests
        tests.RunTestCase();
        Console.WriteLine("Tests finished");
        Console.ReadKey();

6. 保存更改，然后单击**F5**或选择**调试** > **开始调试**来启动项目。 控制台窗口应该出现，并且记录作为测试进度的状态。 **测试完成后**出现时，按任意键可关闭该窗口。

7. 使用**Windows 资源管理器中**找到的目录，其中包含您的项目，例如， **C:\Users\<your_user_name > \Documents\Visual Studio 2013\Projects\WordCount\WordCount**。 在此目录中，打开**Bin**，然后单击**调试**。 您应该看到在测试运行时所产生的文本文件︰ sentences.txt、 counter.txt 和 splitter.txt。 打开每个文件的文本，并检查数据。

    > [AZURE.NOTE] 字符串数据仍保持为这些文件中的十进制值的数组。 例如， \[[97,103,111]] 在**splitter.txt**文件是 word '和'。

虽然测试的基本单词计数本地应用程序是非常微不足道的具有复杂的拓朴，它与外部数据源进行通信或执行复杂的数据分析时，真正的价值。 当您正在从事此类项目时，您可能需要在您找出问题的组件中设置断点并单步执行代码。

> [AZURE.NOTE] 请确保在部署到 HDInsight 群集上风暴之前将**项目类型**设置回**类库**。

### <a name="log-information"></a>日志信息

可以轻松地记录信息从您的拓扑组件通过使用`Context.Logger`。 例如，下面将创建一个信息日志项︰

    Context.Logger.Info("Component started");

记录的信息可以从**Hadoop 服务日志**，它在**服务器资源管理器**中找到。 对于您在 HDInsight 群集上的风暴展开条目然后展开**Hadoop 服务日志**。 最后，选择日志文件查看。

> [AZURE.NOTE] 日志被存储在您的群集使用 Azure 存储帐户。 如果这是您已登录到与 Visual Studio 比其他订阅，您需要登录到包含存储帐户，若要查看此信息的订阅服务。

### <a name="view-error-information"></a>查看错误的信息

若要查看正在运行的拓扑结构中发生的错误，请使用以下步骤︰

1. 从**服务器资源管理器中**，用鼠标右键单击 HDInsight 群集上的风暴并选择**视图风暴拓扑**。

2. **Spout**和**螺栓**，**最后一个错误**列中会出现的最后一个错误的信息。

3. 选择具有列出出现错误的组件的**Spout Id**或**螺栓 Id** 。 在详细信息页显示其他错误信息将列在页面底部的**错误**部分。

4. 要获得更多的信息，请从页后，可以看到风暴工作日志的最后几分钟**执行器**部分选择**端口**。

## <a name="next-steps"></a>下一步行动

现在，您已了解如何开发和部署 Visual Studio 风暴从 HDInsight 工具的拓扑信息，了解如何[处理从 HDInsight 上的风暴与 Azure 事件中心的事件](hdinsight-storm-develop-csharp-event-hub-topology.md)。

将流数据拆分为多个流的 C# 拓扑的示例，请参见[C# 风暴的示例](https://github.com/Blackmist/csharp-storm-example)。

若要了解有关创建 C# 拓扑的详细信息，请访问[SCP.NET GettingStarted.md](https://github.com/hdinsight/hdinsight-storm-examples/blob/master/SCPNet-GettingStarted.md)。

对于更多方法使用 HDInsight 和 HDinsight 样本更多风暴，请参阅以下文章︰

**Microsoft SCP.NET**

* [SCP 编程指南](hdinsight-storm-scp-programming-guide.md)

**在 HDInsight 上的 Apache 风暴**

- [部署和监视在 HDInsight 上的 Apache 风暴具有拓扑](hdinsight-storm-deploy-monitor-topology.md)

- [在 HDInsight 上的风暴的示例拓扑](hdinsight-storm-example-topology.md)

**在 HDInsight 上的 Apache Hadoop**

- [使用 Hadoop HDInsight 上配置单元](hdinsight-use-hive.md)

- [使用 Hadoop HDInsight 上的小猪](hdinsight-use-pig.md)

- [在 HDInsight 上的 Hadoop 使用 MapReduce](hdinsight-use-mapreduce.md)

**在 HDInsight 上的 Apache HBase**

- [在 HDInsight 上的 HBase 入门](hdinsight-hbase-tutorial-get-started.md)
