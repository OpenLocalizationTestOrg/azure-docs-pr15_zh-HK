<properties
    pageTitle="处理 IoT 集线器设备到云消息 (.Net) |Microsoft Azure"
    description="按照本教程来学习有用的模式来处理 IoT 集线器设备到云的消息。"
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="csharp"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="10/05/2016"
     ms.author="dobett"/>

# <a name="tutorial-how-to-process-iot-hub-device-to-cloud-messages-using-net"></a>教程︰ 如何处理使用.Net IoT 集线器设备到云消息

[AZURE.INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

## <a name="introduction"></a>介绍

Azure IoT 集线器是一种完全托管的服务，使可靠和安全数百万 IoT 设备和应用程序之间的双向通信的后端。 其他教程 ([入门 IoT 集线器]和[云至设备使用发送邮件 IoT 中心][lnk-c2d]) 向您展示如何使用 IoT 集线器的基本设备到云，云至设备消息传递功能。

本教程上显示[IoT 中心入门]教程中的代码生成，它显示了两种可缩放模式可用于处理设备到云的消息︰

- 在[Azure blob 存储]设备到云消息可靠存储。 一种常见情况是*冷的路径*分析，在其中存储 blob 用作输入到分析流程中的遥测数据。 这些进程可以依靠[Azure 数据工厂]或[HDInsight (Hadoop)]堆栈的工具。

- *交互式*设备到云消息可靠处理。 当它们是直接触发的一组操作的应用程序的后端设备到云的消息是交互式的。 例如，某个设备可能发送将票插入到 CRM 系统将触发警报消息。 与此相反，*数据点*的消息只被送入分析引擎。 例如，温度遥测设备是存储供以后分析是数据点消息。

因为 IoT 中心公开[事件中心][lnk-event-hubs]-兼容的终结点接收设备到云的消息，本教程使用一个[EventProcessorHost]实例。 此实例︰

* 可靠地存储 Azure blob 存储中的*数据点*的邮件。
* *交互式*设备到云将消息转发到 Azure[服务总线队列]以进行即时处理。

服务总线可以帮助确保可靠处理交互式的邮件，因为它能提供每个邮件检查点和时间基于窗口的重复数据消除。

> [AZURE.NOTE] **EventProcessorHost**实例是只有一种方法处理交互式的邮件。 其他选项包括[Azure 服务结构][ lnk-service-fabric] [Azure 流分析]和[lnk-stream-analytics]。

在本教程结束时，您将运行三个 Windows 控制台应用程序︰

* **SimulatedDevice**，该应用程序的修改版本创建[IoT 中心入门]教程，发送数据中点设备到云消息每第二个，和交互式设备到云消息每隔 10 秒。 此应用程序中使用 AMQP 协议与 IoT 中心通信。
* **ProcessDeviceToCloudMessages**使用[EventProcessorHost]类检索邮件从事件中心兼容终结点。 然后，再可靠 Azure blob 存储中存储的数据点消息并转发到服务总线队列交互消息。
* **ProcessD2CInteractiveMessages**消除排队从服务总线队列交互消息。

> [AZURE.NOTE] IoT 中心有许多设备平台和语言，包括 C、 Java 和 JavaScript 的 SDK 支持。 若要了解如何替换为模拟的设备在本教程中的物理设备，以及如何将设备连接到一个 IoT 集线器，请参阅[Azure IoT 开发中心]。

本教程是直接适用于使用事件兼容中心的消息，如[HDInsight (Hadoop)]项目的其他方法。 有关详细信息，请参阅[Azure IoT 中心开发人员指南-设备至云]。

若要完成本教程，您需要以下各项︰

+ Microsoft Visual Studio 2015年。

+ 活动的 Azure 帐户。 <br/>如果您没有帐户，您可以在几分钟创建一个[免费帐户](https://azure.microsoft.com/free/)。

应具有[Azure 存储]和[Azure 服务总线]的一些基本知识。


## <a name="send-interactive-messages-from-a-simulated-device"></a>从模拟设备发送交互消息

在本节中，您可以修改您[IoT 中心入门]教程将交互式设备到云消息发送到 IoT 中心中创建的模拟的设备应用程序。

1. 在 Visual Studio 中，在**SimulatedDevice**项目中，将下列方法添加到**程序**类。

    ```
    private static async void SendDeviceToCloudInteractiveMessagesAsync()
    {
      while (true)
      {
        var interactiveMessageString = "Alert message!";
        var interactiveMessage = new Message(Encoding.ASCII.GetBytes(interactiveMessageString));
        interactiveMessage.Properties["messageType"] = "interactive";
        interactiveMessage.MessageId = Guid.NewGuid().ToString();

        await deviceClient.SendEventAsync(interactiveMessage);
        Console.WriteLine("{0} > Sending interactive message: {1}", DateTime.Now, interactiveMessageString);

        Task.Delay(10000).Wait();
      }
    }
    ```

    此方法非常类似于**SimulatedDevice**项目中的**SendDeviceToCloudMessagesAsync**方法。 仅有的区别是现在设置的**邮件 Id**系统属性，并且用户属性名**可以忽略 messageType**。
    为代码分配给**邮件 Id**属性的全局唯一标识符 (GUID)。 服务总线可以使用此标识符来收到的邮件执行重复数据消除。 该示例使用**可以忽略 messageType**属性来区分从数据点消息交互。 应用程序将此信息传递中消息的属性，而不是在邮件正文中，这样的事件处理器不需要进行反序列化执行消息路由的消息。

    > [AZURE.NOTE] 务必要创建用来重复数据消除设备代码中的交互式邮件的**邮件 Id** 。 间歇性的网络通信或其他故障，可能会导致该设备从同一邮件的多个重传。 您还可以使用语义消息 ID，如相关信息的数据字段，代替 GUID 的哈希值。

2. 右前添加下面的方法在**Main**方法中，`Console.ReadLine()`行︰

    ````
    SendDeviceToCloudInteractiveMessagesAsync();
    ````

    > [AZURE.NOTE] 为了简单起见，本教程不实现任何重试策略。 在生产代码中，应该实现重试策略如指数退避算法，根据 MSDN 文章︰[瞬时性故障处理]中的建议。

## <a name="process-device-to-cloud-messages"></a>处理设备到云消息

在本节中，您可以创建 Windows 控制台应用程序将处理来自 IoT 集线器的设备到云邮件。 Iot 中心公开 [事件中心]-兼容的终结点以使应用程序能够读取设备到云的消息。 本教程使用[EventProcessorHost]类来处理这些控制台应用程序中的消息。 有关如何从事件集线器的处理消息的详细信息，请参阅[事件集线器入门]教程。

面临的挑战时实现可靠邮件存储的数据点或交互式邮件的转发就是该事件处理依赖于消息使用者提供有关其进度的检查点。 此外，以实现高吞吐量，当您读取事件集线器应该提供大批量的检查点。 此方法不创建重复处理大量邮件如果失败，将计算机恢复到上一个检查点的可能性。 在本教程中，您将了解如何将 Azure 存储写入和服务总线消除 windows 同步**EventProcessorHost**检查点。

若要可靠地将消息写到 Azure 存储，此示例使用[块 blob]的某个数据块提交功能[Azure Block Blobs]。 事件处理器累计在内存中的消息，直到它提供了一个检查点的时间。 例如，累计的消息缓冲区达到最大块大小为 4 MB，或后服务总线的重复数据消除的时间窗口已过期。 然后，在检查点之前代码至 blob 提交一个新的块。

事件处理器作为块 Id 使用事件消息偏移的集线器。 这种机制使事件处理器之前提交到存储中，负责提交块和检查点之间可能崩溃的新块中执行重复数据消除检查。

> [AZURE.NOTE] 本教程使用一个单一的 Azure 存储客户编写检索从 IoT 集线器的所有消息。 若要确定您是否需要在您的解决方案中使用多个 Azure 存储帐户，请参阅[Azure 存储可扩展性准则]。

应用程序使用服务总线重复数据消除功能在处理交互式邮件时避免重复。 在模拟的设备用唯一的**邮件 Id**标记每个交互消息。 这些 Id 启用服务总线来确保，在指定的重复数据消除的时间窗口中，没有两个邮件具有相同**邮件 Id**传递到接收器。 此重复数据消除，以及提供的服务总线队列的每条消息完成语义容易实现可靠的交互式的邮件处理。

若要确保没有消息重新提交以消除窗口之外，该代码与服务总线队列重复数据消除窗口同步**EventProcessorHost**检查点机制。 这种同步通过在每次重复数据消除窗口经过至少一次强制检查点 （在本教程中，该窗口为一小时）。

> [AZURE.NOTE] 本教程使用单个分区的服务总线队列到所有交互式邮件检索从 IoT 中心的进程。 有关如何使用服务总线队列来满足您的解决方案的可伸缩性要求的详细信息，请参阅[Azure 服务总线]文档。

### <a name="provision-an-azure-storage-account-and-a-service-bus-queue"></a>设置 Azure 存储帐户和服务总线队列
若要使用[EventProcessorHost]类，必须使用 Azure 存储帐户以启用**EventProcessorHost**记录检查点的信息。 可以使用现有的 Azure 存储帐户，或按照[有关 Azure 存储]中创建一个新的说明。 记下 Azure 存储帐户连接字符串。

> [AZURE.NOTE] 当您复制并粘贴 Azure 存储帐户连接字符串时，请确保包含没有空格。

您还需要启用可靠处理交互消息的服务总线队列。 您可以创建队列以编程的方式，用一小时重复数据消除窗口，[如何使用服务总线队列][服务总线队列]中所述。 或者，您可以使用[Azure 的传统门户网站][lnk-classic-portal]，通过执行以下步骤︰

1. 在左下角，单击**新建**。 然后单击**服务应用程序** > **服务总线** > **队列** > **自定义创建**。 输入名称**d2ctutorial**、 选择一个地区，并使用现有的命名空间或创建一个新。 在下一页上，选中**启用重复检测**，并设置为一小时的**重复检测历史记录的时间窗口**。 然后单击以保存队列配置的右下角中的复选标记。

    ![在 Azure 门户创建队列][30]

2. 在服务总线队列列表中，单击**d2ctutorial**，，然后单击**配置**。 创建两个共享的访问策略、 一个调用**发送****代理发送**权限，和一个称为**聆听****聆听**权限。 操作完成后，请单击底部的**保存**。

    ![在 Azure 门户配置的队列][31]

3. 底部的顶部，然后**连接信息**时，单击**仪表板**。 记两个连接字符串。

    ![队列在 Azure 门户的仪表板][32]

### <a name="create-the-event-processor"></a>创建事件处理器

1. 在当前的 Visual Studio 解决方案中，若要使用**控制台应用程序**项目模板创建可视化 Windows C# 项目，单击**文件** > **添加** > **新的项目**。 请确保.NET Framework 版本是 4.5.1 或更高版本。 命名项目**ProcessDeviceToCloudMessages**，然后单击**确定**。

    ![在 Visual Studio 中的新项目][10]

2. 在解决方案资源管理器中，右击**ProcessDeviceToCloudMessages**项目，然后单击**管理 NuGet 程序包**。 **NuGet 程序包管理器**对话框中出现。

3. **WindowsAzure.ServiceBus**搜索，请单击**安装**，接受使用条款。 此操作将下载、 安装、 安装和添加到[Azure 服务总线 NuGet 程序包](https://www.nuget.org/packages/WindowsAzure.ServiceBus)，与其依赖项的引用。

4. **Microsoft.Azure.ServiceBus.EventProcessorHost**搜索，请单击**安装**，接受使用条款。 此操作将下载、 安装、 安装和添加到[Azure 服务总线事件中枢-EventProcessorHost NuGet 程序包](https://www.nuget.org/packages/Microsoft.Azure.ServiceBus.EventProcessorHost)，与其依赖项的引用。

5. 右击**ProcessDeviceToCloudMessages**项目，单击**添加**，然后单击**类**。 命名为**StoreEventProcessor**，该新类，然后单击**确定**以创建类。

6. 在 StoreEventProcessor.cs 文件的顶部添加以下语句︰

    ```
    using System.IO;
    using System.Diagnostics;
    using System.Security.Cryptography;
    using Microsoft.ServiceBus.Messaging;
    using Microsoft.WindowsAzure.Storage;
    using Microsoft.WindowsAzure.Storage.Blob;
    ```

7. 替换为以下代码为类主体︰

    ```
    class StoreEventProcessor : IEventProcessor
    {
      private const int MAX_BLOCK_SIZE = 4 * 1024 * 1024;
      public static string StorageConnectionString;
      public static string ServiceBusConnectionString;

      private CloudBlobClient blobClient;
      private CloudBlobContainer blobContainer;
      private QueueClient queueClient;

      private long currentBlockInitOffset;
      private MemoryStream toAppend = new MemoryStream(MAX_BLOCK_SIZE);

      private Stopwatch stopwatch;
      private TimeSpan MAX_CHECKPOINT_TIME = TimeSpan.FromHours(1);

      public StoreEventProcessor()
      {
        var storageAccount = CloudStorageAccount.Parse(StorageConnectionString);
        blobClient = storageAccount.CreateCloudBlobClient();
        blobContainer = blobClient.GetContainerReference("d2ctutorial");
        blobContainer.CreateIfNotExists();
        queueClient = QueueClient.CreateFromConnectionString(ServiceBusConnectionString);
      }

      Task IEventProcessor.CloseAsync(PartitionContext context, CloseReason reason)
      {
        Console.WriteLine("Processor Shutting Down. Partition '{0}', Reason: '{1}'.", context.Lease.PartitionId, reason);
        return Task.FromResult<object>(null);
      }

      Task IEventProcessor.OpenAsync(PartitionContext context)
      {
        Console.WriteLine("StoreEventProcessor initialized.  Partition: '{0}', Offset: '{1}'", context.Lease.PartitionId, context.Lease.Offset);

        if (!long.TryParse(context.Lease.Offset, out currentBlockInitOffset))
        {
          currentBlockInitOffset = 0;
        }
        stopwatch = new Stopwatch();
        stopwatch.Start();

        return Task.FromResult<object>(null);
      }

      async Task IEventProcessor.ProcessEventsAsync(PartitionContext context, IEnumerable<EventData> messages)
      {
        foreach (EventData eventData in messages)
        {
          byte[] data = eventData.GetBytes();

          if (eventData.Properties.ContainsKey("messageType") && (string) eventData.Properties["messageType"] == "interactive")
          {
            var messageId = (string) eventData.SystemProperties["message-id"];

            var queueMessage = new BrokeredMessage(new MemoryStream(data));
            queueMessage.MessageId = messageId;
            queueMessage.Properties["messageType"] = "interactive";
            await queueClient.SendAsync(queueMessage);

            WriteHighlightedMessage(string.Format("Received interactive message: {0}", messageId));
            continue;
          }

          if (toAppend.Length + data.Length > MAX_BLOCK_SIZE || stopwatch.Elapsed > MAX_CHECKPOINT_TIME)
          {
            await AppendAndCheckpoint(context);
          }
          await toAppend.WriteAsync(data, 0, data.Length);

          Console.WriteLine(string.Format("Message received.  Partition: '{0}', Data: '{1}'",
            context.Lease.PartitionId, Encoding.UTF8.GetString(data)));
        }
      }

      private async Task AppendAndCheckpoint(PartitionContext context)
      {
        var blockIdString = String.Format("startSeq:{0}", currentBlockInitOffset.ToString("0000000000000000000000000"));
        var blockId = Convert.ToBase64String(ASCIIEncoding.ASCII.GetBytes(blockIdString));
        toAppend.Seek(0, SeekOrigin.Begin);
        byte[] md5 = MD5.Create().ComputeHash(toAppend);
        toAppend.Seek(0, SeekOrigin.Begin);

        var blobName = String.Format("iothubd2c_{0}", context.Lease.PartitionId);
        var currentBlob = blobContainer.GetBlockBlobReference(blobName);

        if (await currentBlob.ExistsAsync())
        {
          await currentBlob.PutBlockAsync(blockId, toAppend, Convert.ToBase64String(md5));
          var blockList = await currentBlob.DownloadBlockListAsync();
          var newBlockList = new List<string>(blockList.Select(b => b.Name));

          if (newBlockList.Count() > 0 && newBlockList.Last() != blockId)
          {
            newBlockList.Add(blockId);
            WriteHighlightedMessage(String.Format("Appending block id: {0} to blob: {1}", blockIdString, currentBlob.Name));
          }
          else
          {
            WriteHighlightedMessage(String.Format("Overwriting block id: {0}", blockIdString));
          }
          await currentBlob.PutBlockListAsync(newBlockList);
        }
        else
        {
          await currentBlob.PutBlockAsync(blockId, toAppend, Convert.ToBase64String(md5));
          var newBlockList = new List<string>();
          newBlockList.Add(blockId);
          await currentBlob.PutBlockListAsync(newBlockList);

          WriteHighlightedMessage(String.Format("Created new blob", currentBlob.Name));
        }

        toAppend.Dispose();
        toAppend = new MemoryStream(MAX_BLOCK_SIZE);

        // checkpoint.
        await context.CheckpointAsync();
        WriteHighlightedMessage(String.Format("Checkpointed partition: {0}", context.Lease.PartitionId));

        currentBlockInitOffset = long.Parse(context.Lease.Offset);
        stopwatch.Restart();
      }

      private void WriteHighlightedMessage(string message)
      {
        Console.ForegroundColor = ConsoleColor.Yellow;
        Console.WriteLine(message);
        Console.ResetColor();
      }
    }
    ```

    **EventProcessorHost**类调用此类处理设备到云 IoT 集线器从接收到的消息。 此类中的代码实现可靠地将消息存储在一个 blob 容器，并转发到服务总线队列交互消息的逻辑。

    **OpenAsync**方法初始化**currentBlockInitOffset**变量，它可以跟踪的当前偏移量读取此事件处理器的第一条消息的。 请记住，每个处理器负责单个分区。

    **ProcessEventsAsync**方法从 IoT 集线器接收一批消息并处理它们，如下所示︰ 它将交互消息发送到服务总线队列，并将数据点消息追加到名为**toAppend**的内存缓冲区。 如果内存缓冲区的大小达到 4 MB，或重复数据消除的时间窗口间隔 （在本教程中的检查点之后的一小时），该应用程序之后，将触发一个检查点。

    **AppendAndCheckpoint**方法首先生成块要追加 blockId。 Azure 存储要求所有阻止 Id 具有相同的长度，所以方法填充带前导零的偏移量`currentBlockInitOffset.ToString("0000000000000000000000000")`。 然后，如果具有此 ID 的块已经在 blob，该方法将覆盖它的缓冲区的当前内容。

    > [AZURE.NOTE] 为了简化代码，本教程使用单个 blob 每个分区来存储这些邮件。 真正的解决方案可以实现滚动通过一定的时间，或者当他们达到一定的大小后创建其他文件的文件。 请记住，Azure 块 blob 可以包含最多 195 GB 的数据。

8. 在**程序**类的顶部添加以下**using**语句︰

    ```
    using Microsoft.ServiceBus.Messaging;
    ```

9. 修改**程序**类中的**Main**方法，如下所示。 [IoT 中心入门]教程中的**iothubowner**连接字符串替换**{iot 集线器连接字符串}** 。 这一节的开头提到的连接字符串替换存储连接字符串。 服务总线连接字符串替换**发送**队列名为**d2ctutorial**的本节开头提到的权限︰

    ```
    static void Main(string[] args)
    {
      string iotHubConnectionString = "{iot hub connection string}";
      string iotHubD2cEndpoint = "messages/events";
      StoreEventProcessor.StorageConnectionString = "{storage connection string}";
      StoreEventProcessor.ServiceBusConnectionString = "{service bus send connection string}";

      string eventProcessorHostName = Guid.NewGuid().ToString();
      EventProcessorHost eventProcessorHost = new EventProcessorHost(eventProcessorHostName, iotHubD2cEndpoint, EventHubConsumerGroup.DefaultGroupName, iotHubConnectionString, StoreEventProcessor.StorageConnectionString, "messages-events");
      Console.WriteLine("Registering EventProcessor...");
      eventProcessorHost.RegisterEventProcessorAsync<StoreEventProcessor>().Wait();

      Console.WriteLine("Receiving. Press enter key to stop worker.");
      Console.ReadLine();
      eventProcessorHost.UnregisterEventProcessorAsync().Wait();
    }
    ```

    > [AZURE.NOTE] 为了简单起见，本教程使用[EventProcessorHost]类的一个实例。 有关详细信息，请参阅[事件集线器编程指南]。

## <a name="receive-interactive-messages"></a>接收交互式邮件
在本节中，您将编写 Windows 控制台应用程序接收来自服务总线队列交互的消息。 有关如何构建使用服务总线的解决方案的详细信息，请参阅[构建多层应用程序与服务总线][]。

1. 在当前的 Visual Studio 解决方案中，通过使用**控制台应用程序**项目模板来创建可视化 Windows C# 项目。 **ProcessD2CInteractiveMessages**为项目命名。

2. 在解决方案资源管理器中，右击**ProcessD2CInteractiveMessages**项目，然后单击**管理 NuGet 程序包**。 此操作显示**NuGet 程序包管理器**窗口。

3. **WindowsAzure.ServiceBus**搜索，请单击**安装**，接受使用条款。 此操作将下载、 安装、 安装和添加到[Azure 服务总线](https://www.nuget.org/packages/WindowsAzure.ServiceBus)，与其依赖项的引用。

4. 将下面的**using**语句添加在**Program.cs**文件的顶部︰

    ```
    using System.IO;
    using Microsoft.ServiceBus.Messaging;
    ```

5. 最后，将以下行添加到**Main**方法。 替换连接字符串与**侦听**队列名为**d2ctutorial**的权限︰

    ```
    Console.WriteLine("Process D2C Interactive Messages app\n");

    string connectionString = "{service bus listen connection string}";
    QueueClient Client = QueueClient.CreateFromConnectionString(connectionString);

    OnMessageOptions options = new OnMessageOptions();
    options.AutoComplete = false;
    options.AutoRenewTimeout = TimeSpan.FromMinutes(1);

    Client.OnMessage((message) =>
    {
      try
      {
        var bodyStream = message.GetBody<Stream>();
        bodyStream.Position = 0;
        var bodyAsString = new StreamReader(bodyStream, Encoding.ASCII).ReadToEnd();

        Console.WriteLine("Received message: {0} messageId: {1}", bodyAsString, message.MessageId);

        message.Complete();
      }
      catch (Exception)
      {
        message.Abandon();
      }
    }, options);

    Console.WriteLine("Receiving interactive messages from SB queue...");
    Console.WriteLine("Press any key to exit.");
    Console.ReadLine();
    ```

## <a name="run-the-applications"></a>运行应用程序

现在，您就可以运行应用程序了。

1.  在 Visual Studio 中，在解决方案资源管理器中，右击解决方案，然后选择**设置启动项目**。 选择**多个启动项目**，然后选择**启动**作为**ProcessDeviceToCloudMessages**、 **SimulatedDevice**和**ProcessD2CInteractiveMessages**项目的操作。

2.  按**f5 键**以启动三个控制台应用程序。 **ProcessD2CInteractiveMessages**应用程序应该处理从**SimulatedDevice**应用程序发送的每个交互消息。

  ![三个控制台应用程序][50]

> [AZURE.NOTE] 若要查看您的 blob 中的更新，可能需要降低到一个较小的值，如**1024** **StoreEventProcessor**类中的**MAX_BLOCK_SIZE**常量。 这种更改很有用，因为它需要一些时间，以达到模拟设备发送的数据的块大小限制。 与较小的块大小，您不需要等待很长时间，查看所创建和更新 blob。 然而，使用较大的块大小使应用程序更具可扩展性。

## <a name="next-steps"></a>下一步行动

在本教程中，您学习了如何可靠地使用[EventProcessorHost]类中处理数据点和交互式设备到云消息。

[如何发送 IoT 中枢云至设备信息][lnk-c2d]向您显示如何将消息发送到您的设备，从您的后端。

若要查看使用 IoT 集线器的完整的端到端解决方案的示例，请参见[Azure IoT 套件][lnk-suite]。

若要了解有关使用 IoT 中心开发解决方案的详细信息，请参阅[IoT 中心开发指南 》]。

<!-- Images. -->
[50]: ./media/iot-hub-csharp-csharp-process-d2c/run1.png
[10]: ./media/iot-hub-csharp-csharp-process-d2c/create-identity-csharp1.png

[30]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue2.png
[31]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue3.png
[32]: ./media/iot-hub-csharp-csharp-process-d2c/createqueue4.png

<!-- Links -->

[Azure blob 存储]: ../storage/storage-dotnet-how-to-use-blobs.md
[Azure 数据工厂]: https://azure.microsoft.com/documentation/services/data-factory/
[HDInsight (Hadoop)]: https://azure.microsoft.com/documentation/services/hdinsight/
[服务总线队列]: ../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md

[Azure IoT 中心开发人员指南-设备至云]: iot-hub-devguide-messaging.md

[Azure 存储]: https://azure.microsoft.com/documentation/services/storage/
[Azure 服务总线]: https://azure.microsoft.com/documentation/services/service-bus/

[IoT 中心开发指南]: iot-hub-devguide.md
[开始使用 IoT 集线器]: iot-hub-csharp-csharp-getstarted.md
[Azure IoT 开发人员中心]: https://azure.microsoft.com/develop/iot
[lnk-service-fabric]: https://azure.microsoft.com/documentation/services/service-fabric/
[lnk-stream-analytics]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-event-hubs]: https://azure.microsoft.com/documentation/services/event-hubs/
[瞬时故障处理]: https://msdn.microsoft.com/library/hh675232.aspx

<!-- Links -->
[关于 Azure 存储]: ../storage/storage-create-storage-account.md#create-a-storage-account
[开始使用事件集线器]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[Azure 存储可扩展性准则]: ../storage/storage-scalability-targets.md
[Azure Block Blobs]: https://msdn.microsoft.com/library/azure/ee691964.aspx
[Event Hubs]: ../event-hubs/event-hubs-overview.md
[EventProcessorHost]: http://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventprocessorhost(v=azure.95).aspx
[事件集线器编程指南]: ../event-hubs/event-hubs-programming-guide.md
[瞬时故障处理]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[构建多层应用程序与服务总线]: ../service-bus-messaging/service-bus-dotnet-multi-tier-app-using-service-bus-queues.md

[lnk-classic-portal]: https://manage.windowsazure.com
[lnk-c2d]: iot-hub-csharp-csharp-process-d2c.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
