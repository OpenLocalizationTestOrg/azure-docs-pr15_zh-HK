<properties
    pageTitle="发送 IoT 中枢云至设备信息 |Microsoft Azure"
    description="按照本教程中学习如何发送使用 Azure IoT 中心 C# 云至设备信息。"
    services="iot-hub"
    documentationCenter=".net"
    authors="fsautomata"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="06/23/2016"
     ms.author="elioda"/>

# <a name="tutorial-how-to-send-cloud-to-device-messages-with-iot-hub-and-net"></a>教程︰ 如何发送 IoT 集线器和.Net 云至设备信息

[AZURE.INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

## <a name="introduction"></a>介绍

Azure IoT 集线器是完全托管的服务，可帮助使数以百万计的 IoT 设备之间的可靠和安全双向通信和应用程序后结束。 [IoT 中心入门]教程演示如何创建 IoT 集线器、 调配，设备标识和模拟的设备的发送设备与云消息的代码。

本教程为基础[入门 IoT 集线器]。 它为您显示方式为︰

- 从您应用程序的云后端，向单个设备通过 IoT 集线器云至设备信息。
- 接收设备上的云至设备信息。
- 应用云从后端，消息发送到设备从 IoT 集线器请求交货确认 （*反馈*）。

您可以找到[IoT 中心开发人员指南 》]中云至设备信息的详细信息[IoT Hub Developer Guide - C2D]。

在本教程结束时，您将运行两个 Windows 控制台应用程序︰

* **SimulatedDevice**，在[入门 IoT 集线器]，它连接到您 IoT 中枢并接收云至设备信息中创建应用程序的修改后的版本。
* **SendCloudToDevice**，其中云至设备信息将发送到模拟设备通过 IoT 集线器，然后接收其交货确认。

> [AZURE.NOTE] IoT 集线器有 SDK 支持很多设备平台和语言 （包括 C、 Java 和 Javascript） 通过 Azure IoT 设备 Sdk。 有关如何将设备连接到本教程中的代码，通常到 Azure IoT 集线器的分步说明，请参阅[Azure IoT 开发中心]。

若要完成本教程，您需要︰

+ Microsoft Visual Studio 2015

+ 活动的 Azure 帐户。 (如果您没有帐户，则可以创建一个[免费帐户][lnk-free-trial]只需几分钟。)

## <a name="receive-messages-on-the-simulated-device"></a>在模拟的设备上接收消息

在本节中，您将修改模拟的设备应用程序[入门 IoT 集线器]从 IoT 集线器接收云至设备信息中创建。

1. 在 Visual Studio 中，在**SimulatedDevice**项目中，将下列方法添加到**程序**类。

        private static async void ReceiveC2dAsync()
        {
            Console.WriteLine("\nReceiving cloud to device messages from service");
            while (true)
            {
                Message receivedMessage = await deviceClient.ReceiveAsync();
                if (receivedMessage == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received message: {0}", Encoding.ASCII.GetString(receivedMessage.GetBytes()));
                Console.ResetColor();

                await deviceClient.CompleteAsync(receivedMessage);
            }
        }

    `ReceiveAsync`方法以异步方式接收设备时返回接收到的消息。 并以此来的超时时间后返回*null* （在这种情况下，使用默认值为 1 分钟）。 这种情况下，代码应继续等待新的消息。 这是原因`if (receivedMessage == null) continue`线。

    对`CompleteAsync()`通知 IoT 中心已成功处理该消息。 该消息可以安全地删除从设备队列。 如果事情阻止设备应用程序完成处理消息，IoT 集线器可以再次提供它。 它是然后重要设备应用程序中的消息处理逻辑是*幂等*，以便接收相同的邮件多次产生相同的结果。 应用程序可以临时放弃一条消息，这会导致 IoT 中心保留供将来使用的队列中的消息。 或者，应用程序可以拒绝邮件，永久地从队列中移除消息。 有关云至设备信息生命周期的详细信息，请参阅[IoT 中心开发人员指南 》][IoT Hub Developer Guide - C2D]。

    > [AZURE.NOTE] 作为传输协议，而不 MQTT 或 AMQP 使用 HTTP 时`ReceiveAsync`方法立即返回。 使用 HTTP 的云至设备信息支持的模式是间歇性连接检查邮件很少 （少于每隔 25 分钟） 的设备。 限制请求 IoT 中心发出多个 HTTP 接收结果。 MQTT、 AMQP 和 HTTP 支持和 IoT 中枢调节之间的区别的更多信息，请参见[IoT 中心开发人员指南 》][IoT Hub Developer Guide - C2D]。

2. 右前添加下面的方法在**Main**方法中，`Console.ReadLine()`行︰

        ReceiveC2dAsync();

> [AZURE.NOTE] 为简单起见，本教程不实现任何重试策略。 在生产代码中，应该实现重试策略 （如指数退避算法），根据 MSDN 文章︰[瞬时性故障处理]中的建议。

## <a name="send-a-cloud-to-device-message"></a>发送云至设备信息

在本节中，您将编写 Windows 控制台应用程序中，将云至设备信息发送到模拟的设备应用程序。

1. 在当前的 Visual Studio 解决方案中，通过使用**控制台应用程序**项目模板中创建一个新的视觉 C# 桌面应用程序项目。 **SendCloudToDevice**为项目命名。

    ![在 Visual Studio 中的新项目][20]

2. 在解决方案资源管理器中，右击解决方案，，然后单击**解决方案...管理 NuGet 程序包**。 

    这将打开**管理 NuGet 程序包**窗口。

3. 搜索`Microsoft Azure Devices`，单击**安装**，并接受使用条款。 

    此下载、 安装，并将引用添加到[Azure IoT-服务 SDK NuGet 程序包]。

4. 添加以下`using` **Program.cs**文件顶部的语句︰

        using Microsoft.Azure.Devices;

5. 将以下字段添加到**程序**类。 替换占位符值与从[入门 IoT 集线器]IoT 集线器连接字符串︰

        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";

6. 将下面的方法添加到**程序**类︰

        private async static Task SendCloudToDeviceMessageAsync()
        {
            var commandMessage = new Message(Encoding.ASCII.GetBytes("Cloud to device message."));
            await serviceClient.SendAsync("myFirstDevice", commandMessage);
        }

    此方法将新云至设备信息发送到设备，ID， `myFirstDevice`。 因此，更改此参数，以防修改与[入门 IoT 中心]中使用。

7. 最后，将以下行添加到**Main**方法︰

        Console.WriteLine("Send Cloud-to-Device message\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);

        Console.WriteLine("Press any key to send a C2D message.");
        Console.ReadLine();
        SendCloudToDeviceMessageAsync().Wait();
        Console.ReadLine();

8. 在 Visual Studio 中，用鼠标右键单击您的解决方案，然后选择**设置启动项目...**。 选择**多个启动项目**，然后选择**ProcessDeviceToCloudMessages**、 **SimulatedDevice**和**SendCloudToDevice**的**启动**操作。

9.  按**f5 键**。 所有三个应用程序将启动。 选择**SendCloudToDevice**窗口，然后按**Enter**。 您应该看到通过模拟应用程序接收到消息。

    ![应用程序接收消息][21]

## <a name="receive-delivery-feedback"></a>接收发送反馈
可能是请求交付 （或过期） 确认到 IoT 中心从为每个云至设备信息。 这使得可以很容易地通知重试或补偿逻辑的云后端。 有关云至设备反馈的详细信息，请参阅[IoT 中心开发人员指南 》][IoT Hub Developer Guide - C2D]。

在本部分中，您将修改**SendCloudToDevice**应用程序请求的反馈，用户和接收来自 IoT 集线器。

1. 在 Visual Studio 中，在**SendCloudToDevice**项目中，将下列方法添加到**程序**类。
   
        private async static void ReceiveFeedbackAsync()
        {
            var feedbackReceiver = serviceClient.GetFeedbackReceiver();

            Console.WriteLine("\nReceiving c2d feedback from service");
            while (true)
            {
                var feedbackBatch = await feedbackReceiver.ReceiveAsync();
                if (feedbackBatch == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received feedback: {0}", string.Join(", ", feedbackBatch.Records.Select(f => f.StatusCode)));
                Console.ResetColor();

                await feedbackReceiver.CompleteAsync(feedbackBatch);
            }
        }

    请注意，接收模式同样是用来接收来自设备应用程序的云至设备信息。

2. 右后添加以下方法在**Main**方法中，`serviceClient = ServiceClient.CreateFromConnectionString(connectionString)`行︰

        ReceiveFeedbackAsync();

3. 若要请求交货的云至设备信息的反馈，您必须在**SendCloudToDeviceMessageAsync**方法中指定的属性。 右后添加下面的行，`var commandMessage = new Message(...);`行︰

        commandMessage.Ack = DeliveryAcknowledgement.Full;

4.  按**F5**运行应用程序。 您应该看到开始的所有三个应用程序。 选择**SendCloudToDevice**窗口，然后按**Enter**。 您应该看到正在接收的消息通过模拟应用程序，和之后几秒钟， **SendCloudToDevice**应用程序接收到的反馈消息。

    ![应用程序接收消息][22]

> [AZURE.NOTE] 为简单起见，本教程不实现任何重试策略。 在生产代码中，应该实现重试策略 （如指数退避算法），根据 MSDN 文章︰[瞬时性故障处理]中的建议。

## <a name="next-steps"></a>下一步行动

在本教程中，您学习了如何发送和接收云至设备信息。 

若要查看使用 IoT 集线器的完整的端到端解决方案的示例，请参见[Azure IoT 套件]。

若要了解有关使用 IoT 中心开发解决方案的详细信息，请参阅[IoT 中心开发指南 》]。

<!-- Images -->
[20]: ./media/iot-hub-csharp-csharp-c2d/create-identity-csharp1.png
[21]: ./media/iot-hub-csharp-csharp-c2d/sendc2d1.png
[22]: ./media/iot-hub-csharp-csharp-c2d/sendc2d2.png

<!-- Links -->

[Azure IoT-服务 SDK NuGet 程序包]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[瞬时故障处理]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[IoT Hub Developer Guide - C2D]: iot-hub-devguide-messaging.md

[IoT 中心开发指南]: iot-hub-devguide.md
[开始使用 IoT 集线器]: iot-hub-csharp-csharp-getstarted.md
[Azure IoT 开发人员中心]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[Azure IoT 套件]: https://azure.microsoft.com/documentation/suites/iot-suite/