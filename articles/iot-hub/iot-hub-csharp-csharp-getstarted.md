<properties
    pageTitle="Azure IoT 中心入门 C# |Microsoft Azure"
    description="C# 获取 azure IoT 中心启动教程。 与 Microsoft Azure IoT Sdk 中使用 Azure IoT 中心和 C#，来实现联网解决方案。"
    services="iot-hub"
    documentationCenter=".net"
    authors="dominicbetts"
    manager="timlt"
    editor=""/>

<tags
     ms.service="iot-hub"
     ms.devlang="dotnet"
     ms.topic="hero-article"
     ms.tgt_pltfrm="na"
     ms.workload="na"
     ms.date="09/12/2016"
     ms.author="dobett"/>

# <a name="get-started-with-azure-iot-hub-for-net"></a>为使.NET 开始使用 Azure IoT 集线器

[AZURE.INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

在本教程结束时，您有三个 Windows 控制台应用程序︰

* **CreateDeviceIdentity**，这将创建一个设备标识和关联的安全密钥，将模拟的设备连接。
* **ReadDeviceToCloudMessages**，其中显示的发送的模拟设备的遥测数据。
* **SimulatedDevice**，其先前创建的设备标识与 IoT 集线器连接，并使用 AMQP 协议每秒钟发送遥测消息。

> [AZURE.NOTE] 有关可用于生成两个设备，并且您的解决方案后端上运行的应用程序的各种 Sdk 的信息，请参阅[IoT 集线器 Sdk][lnk-hub-sdks]。

若要完成本教程，您需要以下各项︰

+ Microsoft Visual Studio 2015年。

+ 活动的 Azure 帐户。 (如果您没有帐户，则可以创建一个[免费帐户][lnk-free-trial]只需几分钟。)

[AZURE.INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

您现在已经创建了 IoT 网络集线器，并且您已完成本教程的其余部分所需的主机名称和连接字符串。

## <a name="create-a-device-identity"></a>创建设备标识

在本节中，您将创建 IoT 中心标识注册表中创建一个设备标识 Windows 控制台应用程序。 除非它设备标识注册表中有一个条目，设备无法连接到 IoT 集线器。 有关详细信息，请参阅[IoT 中心开发指南 》]的"设备标识注册表"部分[lnk-devguide-identity]。 当您运行此控制台应用程序时，它生成一个唯一的设备 ID 和密钥设备可以用来标识自身设备到云的消息发送到 IoT 集线器时。

1. 在 Visual Studio 中，通过使用**控制台应用程序**项目模板添加到当前解决方案 Visual C# Windows 经典桌面项目。 请确保.NET Framework 版本是 4.5.1 或更高版本。 **CreateDeviceIdentity**为项目命名。

    ![新的视觉 C# Windows 经典桌面项目][10]

2. 在解决方案资源管理器中，右击**CreateDeviceIdentity**项目，然后单击**管理 Nuget 程序包**。

3. 在**Nuget 程序包管理器**窗口中，选择**浏览**搜索**microsoft.azure.devices**，选择**安装**以安装**Microsoft.Azure.Devices**软件包，，接受使用条款。 此过程将下载、 安装，并将引用添加到[Microsoft Azure IoT 服务 SDK] [ lnk-nuget-service-sdk] Nuget 程序包及其依赖项。

    ![Nuget 程序包管理器窗口][11]

4. 添加以下`using` **Program.cs**文件顶部的语句︰

        using Microsoft.Azure.Devices;
        using Microsoft.Azure.Devices.Common.Exceptions;

5. 将以下字段添加到**程序**类。 占位符值替换为 IoT 中心在上一节中创建的连接字符串。

        static RegistryManager registryManager;
        static string connectionString = "{iot hub connection string}";

6. 将下面的方法添加到**程序**类︰

        private static async Task AddDeviceAsync()
        {
            string deviceId = "myFirstDevice";
            Device device;
            try
            {
                device = await registryManager.AddDeviceAsync(new Device(deviceId));
            }
            catch (DeviceAlreadyExistsException)
            {
                device = await registryManager.GetDeviceAsync(deviceId);
            }
            Console.WriteLine("Generated device key: {0}", device.Authentication.SymmetricKey.PrimaryKey);
        }

    此方法使用 ID **myFirstDevice**创建一个设备标识。 （如果注册表中已存在该设备 ID，该代码只是检索现有的设备信息。）应用程序将显示为该标识为主键。 模拟设备中使用此项来 IoT 集线器连接。

7. 最后，将以下行添加到**Main**方法︰

        registryManager = RegistryManager.CreateFromConnectionString(connectionString);
        AddDeviceAsync().Wait();
        Console.ReadLine();

8. 运行此应用程序，并记下的设备项。

    ![设备应用程序生成的密钥][12]

> [AZURE.NOTE] 只是 IoT 中心标识注册表存储设备标识能够安全地访问到该中心。 它存储设备 Id 和密钥用作安全凭据，并选择可用于禁用访问单个设备的启用/禁用标志。 如果您的应用程序需要将存储设备相关的其他元数据，它应使用的特定于应用程序的存储。 有关详细信息，请参阅[IoT 中心开发人员指南 》][lnk-devguide-identity]。

## <a name="receive-device-to-cloud-messages"></a>接收设备到云的消息

在本节中，您可以创建 Windows 控制台应用程序读取 IoT 集线器设备到云的消息。 IoT 中心公开[Azure 事件集线器][lnk-event-hubs-overview]-兼容的终结点，以便您能够阅读设备到云的邮件。 为简单起见，本教程创建不适合高吞吐量部署基本读取器。 若要了解如何处理设备到云在规模较大的邮件，请参阅[处理设备到云消息][lnk-process-d2c-tutorial]教程。 有关如何从事件集线器的处理消息的详细信息，请参阅[入门事件集线器][lnk-eventhubs-tutorial]教程。 （本教程是适用于 IoT 中心事件中心兼容终结点）。

> [AZURE.NOTE] 事件中心兼容终结点始终阅读设备到云邮件使用 AMQP 协议。

1. 在 Visual Studio 中，通过使用**控制台应用程序**项目模板添加到当前解决方案中，Visual C# Windows 经典桌面项目。 请确保.NET Framework 版本是 4.5.1 或更高版本。 **ReadDeviceToCloudMessages**为项目命名。

    ![新的视觉 C# Windows 经典桌面项目][10]

2. 在解决方案资源管理器中，右击**ReadDeviceToCloudMessages**项目，然后单击**管理 Nuget 程序包**。

3. **Nuget 程序包管理器**窗口中，在**WindowsAzure.ServiceBus**搜索，选择**安装**，接受使用条款。 此过程将下载、 安装、 安装和添加一个引用到[Azure 服务总线][lnk-servicebus-nuget]，与其依赖项。 此包可以 IoT 集线器上的事件中心兼容终结点连接应用程序。

4. 添加以下`using` **Program.cs**文件顶部的语句︰

        using Microsoft.ServiceBus.Messaging;
        using System.Threading;

5. 将以下字段添加到**程序**类。 占位符值替换为 IoT 中心"创建 IoT 中心"一节中创建的连接字符串。

        static string connectionString = "{iothub connection string}";
        static string iotHubD2cEndpoint = "messages/events";
        static EventHubClient eventHubClient;

6. 将下面的方法添加到**程序**类︰

        private static async Task ReceiveMessagesFromDeviceAsync(string partition, CancellationToken ct)
        {
            var eventHubReceiver = eventHubClient.GetDefaultConsumerGroup().CreateReceiver(partition, DateTime.UtcNow);
            while (true)
            {
                if (ct.IsCancellationRequested) break;
                EventData eventData = await eventHubReceiver.ReceiveAsync();
                if (eventData == null) continue;

                string data = Encoding.UTF8.GetString(eventData.GetBytes());
                Console.WriteLine("Message received. Partition: {0} Data: '{1}'", partition, data);
            }
        }

    此方法使用一个**EventHubReceiver**实例接收来自所有 IoT 集线器设备-到-云邮件接收分区。 请注意您如何传递`DateTime.Now`参数，当您创建**EventHubReceiver**对象，以便它只接收启动后发送的消息。 此筛选器是有用的测试环境中，以便您可以看到当前的消息集。 在生产环境中您的代码应确保它处理的所有消息。 有关详细信息，请参阅[如何处理 IoT 集线器设备到云消息][lnk-process-d2c-tutorial]教程。

7. 最后，将以下行添加到**Main**方法︰

        Console.WriteLine("Receive messages. Ctrl-C to exit.\n");
        eventHubClient = EventHubClient.CreateFromConnectionString(connectionString, iotHubD2cEndpoint);

        var d2cPartitions = eventHubClient.GetRuntimeInformation().PartitionIds;

        CancellationTokenSource cts = new CancellationTokenSource();

        System.Console.CancelKeyPress += (s, e) =>
        {
          e.Cancel = true;
          cts.Cancel();
          Console.WriteLine("Exiting...");
        };

        var tasks = new List<Task>();
        foreach (string partition in d2cPartitions)
        {
           tasks.Add(ReceiveMessagesFromDeviceAsync(partition, cts.Token));
        }  
        Task.WaitAll(tasks.ToArray());

## <a name="create-a-simulated-device-app"></a>创建一个模拟的设备应用程序

在本节中，您将创建一个 Windows 控制台应用程序模拟将设备与云消息发送到一个 IoT 集线器的设备。

1. 在 Visual Studio 中，通过使用**控制台应用程序**项目模板添加到当前解决方案中，Visual C# Windows 经典桌面项目。 请确保.NET Framework 版本是 4.5.1 或更高版本。 **SimulatedDevice**为项目命名。

    ![新的视觉 C# Windows 经典桌面项目][10]

2. 在解决方案资源管理器中，右击**SimulatedDevice**项目，然后单击**管理 Nuget 程序包**。

3. 在**Nuget 程序包管理器**窗口中，选择**浏览**搜索**Microsoft.Azure.Devices.Client**，选择**安装**以安装**Microsoft.Azure.Devices.Client**软件包，，接受使用条款。 此过程将下载、 安装、 安装和添加一个引用到[Azure IoT-设备 SDK Nuget 程序包][lnk-device-nuget]及其依赖项。

4. 添加以下`using` **Program.cs**文件顶部的语句︰

        using Microsoft.Azure.Devices.Client;
        using Newtonsoft.Json;

5. 将以下字段添加到**程序**类。 替换占位符值与您在"创建 IoT 集线器"部分中，检索 IoT 中心主机名并在"创建设备标识"节中检索设备项。

        static DeviceClient deviceClient;
        static string iotHubUri = "{iot hub hostname}";
        static string deviceKey = "{device key}";

6. 将下面的方法添加到**程序**类︰

        private static async void SendDeviceToCloudMessagesAsync()
        {
            double avgWindSpeed = 10; // m/s
            Random rand = new Random();

            while (true)
            {
                double currentWindSpeed = avgWindSpeed + rand.NextDouble() * 4 - 2;

                var telemetryDataPoint = new
                {
                    deviceId = "myFirstDevice",
                    windSpeed = currentWindSpeed
                };
                var messageString = JsonConvert.SerializeObject(telemetryDataPoint);
                var message = new Message(Encoding.ASCII.GetBytes(messageString));

                await deviceClient.SendEventAsync(message);
                Console.WriteLine("{0} > Sending message: {1}", DateTime.Now, messageString);

                Task.Delay(1000).Wait();
            }
        }

    此方法将新设备到云消息发送每隔一秒。 该消息包含一个 JSON 序列化的对象，用来模拟风速度传感器的设备 ID 和一个随机生成的编号。

7. 最后，将以下行添加到**Main**方法︰

        Console.WriteLine("Simulated device\n");
        deviceClient = DeviceClient.Create(iotHubUri, new DeviceAuthenticationWithRegistrySymmetricKey("myFirstDevice", deviceKey));

        SendDeviceToCloudMessagesAsync();
        Console.ReadLine();

  默认情况下，**创建**方法创建一个**DeviceClient**实例，用 AMQP 协议 IoT 中心用来进行通信。 若要使用 HTTP 协议，使用重写**创建**方法，可用于指定的协议。 如果您使用 HTTP 协议，也应到您的项目包含**System.Net.Http.Formatting**命名空间中添加**Microsoft.AspNet.WebApi.Client** Nuget 程序包。

本教程将指导您完成创建 IoT 集线器设备客户端的步骤。 您还可以使用[Azure IoT 集线器连接服务][ lnk-connected-service] Visual Studio 扩展设备客户端应用程序添加必要的代码。


> [AZURE.NOTE] 为简单起见，本教程不实现任何重试策略。 在生产代码中，您应该实现重试策略 （如指数退避算法），建议在 MSDN 文章︰[瞬时性故障处理][lnk-transient-faults]。

## <a name="run-the-applications"></a>运行应用程序

现在您可以运行应用程序。

1.  在 Visual Studio 中，在解决方案资源管理器，您的解决方案中，用鼠标右键单击，然后单击**设置启动项目**。 选择**多个启动项目**，然后选择为**ReadDeviceToCloudMessages**和**SimulatedDevice**项目的操作的**开始**。

    ![启动项目属性][41]

2.  按**f5 键**以开始运行这两个应用程序。 **SimulatedDevice**应用程序的控制台输出显示向 IoT 中心的模拟的设备发送的消息。 **ReadDeviceToCloudMessages**应用程序的控制台输出显示 IoT 集线器接收的消息。

    ![从应用程序的控制台输出][42]

3. **使用**拼贴在[Azure 的门户网站][lnk-portal]显示的集线器发送的消息数︰

    ![Azure 门户使用平铺][43]


## <a name="next-steps"></a>下一步行动

在本教程中，您在 Azure 的门户中，配置一个 IoT 集线器，然后在中枢的标识注册表中创建一个设备标识。 此设备标识用于启用设备到云的消息发送到中心的模拟的设备应用程序。 您还可以创建显示由集线器接收的消息的应用程序。 

若要继续入门 IoT 集线器并探索其他 IoT 方案，请参阅︰

- [连接您的设备][lnk-connect-device]
- [设备管理入门][lnk-device-management]
- [IoT 网关 sdk 快速入门][lnk-gateway-SDK]

若要了解如何扩展 IoT 解决方案和处理设备到云在规模较大的邮件，请参阅[处理设备到云消息][lnk-process-d2c-tutorial]教程。

<!-- Images. -->
[41]: ./media/iot-hub-csharp-csharp-getstarted/run-apps1.png
[42]: ./media/iot-hub-csharp-csharp-getstarted/run-apps2.png
[43]: ./media/iot-hub-csharp-csharp-getstarted/usage.png
[10]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp1.png
[11]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp2.png
[12]: ./media/iot-hub-csharp-csharp-getstarted/create-identity-csharp3.png

<!-- Links -->
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-servicebus-nuget]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md

[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-device-nuget]: https://www.nuget.org/packages/Microsoft.Azure.Devices.Client/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-connected-service]: https://visualstudiogallery.msdn.microsoft.com/e254a3a5-d72e-488e-9bd3-8fee8e0cd1d6
[lnk-device-management]: iot-hub-device-management-get-started.md
[lnk-gateway-SDK]: iot-hub-linux-gateway-sdk-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
