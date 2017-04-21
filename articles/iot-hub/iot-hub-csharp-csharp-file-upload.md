<properties
    pageTitle="上载文件从使用 IoT 集线器的设备 |Microsoft Azure"
    description="按照本指南以了解如何将文件从设备使用 Azure IoT 中心 C# 上载。"
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
     ms.date="06/21/2016"
     ms.author="elioda"/>

# <a name="tutorial-how-to-upload-files-from-devices-to-the-cloud-with-iot-hub"></a>教程︰ 如何上载文件从设备到 IoT 中枢云

## <a name="introduction"></a>介绍

Azure IoT 集线器是一种完全托管的服务，使可靠和安全数百万 IoT 设备和应用程序之间的双向通信的后端。 上一教程 （[入门 IoT 集线器]和[云至设备使用发送邮件 IoT 集线器]） 阐释基本到云设备和云至设备 IoT 集线器和[进程云设备邮件]教程的邮件功能描述可靠 Azure 的 blob 存储在存储设备到云消息的方法。 但是，在某些情况下不能方便地映射到 IoT 中心接受相对较小的设备到云邮件发送您的设备的数据。 例如，包含图像、 视频、 振动数据采样频率高，或其中包含某种形式的预处理数据的大型文件。 这些文件通常是在云中使用[Azure 数据工厂]或[Hadoop]堆栈等工具的批处理。 首选将事件发送到设备中的文件上载时，您仍可以使用 IoT 集线器的安全性和可靠性功能。

本教程[云至设备使用发送邮件 IoT 集线器]教程向您展示如何使用 IoT 中心的文件上载功能中的代码生成。 它演示如何安全地将设备提供 Azure 斑点上载一个文件，以及如何使用 IoT 中心文件上载通知触发处理您的应用程序的后端文件的 URI。

在本教程的结尾处，则运行两个 Windows 控制台应用程序︰

* **SimulatedDevice**，在将文件上载到使用 SAS URI IoT 中心所提供的存储[云至设备使用发送邮件 IoT 集线器]教程中创建的应用程序的修改后的版本。
* **ReadFileUploadNotification**，后者从 IoT 中心获得文件上载通知。

> [AZURE.NOTE] IoT 集线器通过 Azure IoT 设备 Sdk 支持很多设备平台和语言 （包括 C、 Java 和 Javascript）。 请参阅有关逐步说明如何将设备连接到本教程中所示的代码，通常到 Azure IoT 集线器[Azure IoT 开发中心]。

若要完成本教程，您需要︰

+ Microsoft Visual Studio 2015，

+ 活动的 Azure 帐户。 (如果您没有帐户，则可以创建一个[免费帐户][lnk-free-trial]只需几分钟。)

## <a name="associate-an-azure-storage-account-to-iot-hub"></a>将关联到 IoT 集线器 Azure 存储帐户

模拟的设备对 blob 上载文件，因为您必须关联到 IoT 集线器[Azure 存储]帐户。 在 Azure 存储帐户与 IoT 集线器上，集线器可生成设备可用于安全地将文件上载到 blob 容器 SAS URI。 IoT 中心服务和设备 Sdk 坐标生成 SAS 的 URI 并使其可用的设备，用来将文件上载到的过程。

按照[配置文件上载使用 Azure 的门户网站][lnk-configure-upload]将 IoT 集线器到 Azure 存储帐户相关联。

## <a name="upload-a-file-from-a-simulated-device"></a>将文件从一种模拟设备

在本节中，您可以修改模拟的设备应用程序[发送 IoT 中枢云至设备信息]从 IoT 集线器接收云至设备信息中创建。

1. 在 Visual Studio 中，右击**SimulatedDevice**项目，单击**添加**，然后单击**现有项目**。 导航到一个图像文件并将其包括在您的项目。 本教程假定图像被命名为`image.jpg`。

2. 在图像上时，右键单击，然后单击**属性**。 请确保**复制到输出目录**设置为**始终复制**。

    ![][1]

3. 在**Program.cs**文件中，在文件的顶部添加以下语句︰

        using System.IO;

4. 将下面的方法添加到**程序**类︰
         
        private static async void SendToBlobAsync()
        {
            string fileName = "image.jpg";
            Console.WriteLine("Uploading file: {0}", fileName);
            var watch = System.Diagnostics.Stopwatch.StartNew();

            using (var sourceData = new FileStream(@"image.jpg", FileMode.Open))
            {
                await deviceClient.UploadToBlobAsync(fileName, sourceData);
            }

            watch.Stop();
            Console.WriteLine("Time to upload file: {0}ms\n", watch.ElapsedMilliseconds);
        }

    `UploadToBlobAsync`方法所需的文件的名称和流源中上载，并处理上载到的存储。 控制台应用程序显示上载文件所花费的时间。

5. 右前添加下面的方法在**Main**方法中，`Console.ReadLine()`行︰

        SendToBlobAsync();

> [AZURE.NOTE] 为简单起见，本教程不实现任何重试策略。 在生产代码中，应该实现重试策略 （如指数退避算法），根据 MSDN 文章︰[瞬时性故障处理]中的建议。

## <a name="receive-a-file-upload-notification"></a>接收文件上载通知

在本部分中，您将编写 Windows 控制台应用程序从 IoT 集线器接收文件上载通知消息。

1. 在当前的 Visual Studio 解决方案中，通过使用**控制台应用程序**项目模板来创建新 Windows Visual C# 项目。 **ReadFileUploadNotification**为项目命名。

    ![在 Visual Studio 中的新项目][2]

2. 在解决方案资源管理器中，右击**ReadFileUploadNotification**项目，然后单击**管理 NuGet 程序包**。

    这将显示管理 NuGet 程序包窗口。

2. 搜索`Microsoft.Azure.Devices`，单击**安装**，并接受使用条款。 

    此下载、 安装，并在**ReadFileUploadNotification**项目中添加对[Azure IoT-服务 SDK NuGet 程序包]的引用。

3. 在**Program.cs**文件中，在文件的顶部添加以下语句︰

        using Microsoft.Azure.Devices;

4. 将以下字段添加到**程序**类。 替换占位符值与从[入门 IoT 集线器]IoT 集线器连接字符串︰

        static ServiceClient serviceClient;
        static string connectionString = "{iot hub connection string}";
        
5. 将下面的方法添加到**程序**类︰
   
        private async static Task ReceiveFileUploadNotificationAsync()
        {
            var notificationReceiver = serviceClient.GetFileNotificationReceiver();

            Console.WriteLine("\nReceiving file upload notification from service");
            while (true)
            {
                var fileUploadNotification = await notificationReceiver.ReceiveAsync();
                if (fileUploadNotification == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received file upload noticiation: {0}", string.Join(", ", fileUploadNotification.BlobName));
                Console.ResetColor();

                await notificationReceiver.CompleteAsync(fileUploadNotification);
            }
        }

    请注意，接收模式同样是用来接收来自设备应用程序的云至设备信息。

6. 最后，将以下行添加到**Main**方法︰

        Console.WriteLine("Receive file upload notifications\n");
        serviceClient = ServiceClient.CreateFromConnectionString(connectionString);
        ReceiveFileUploadNotificationAsync().Wait();
        Console.ReadLine();

## <a name="run-the-applications"></a>运行应用程序

现在，您就可以运行应用程序了。

1. 在 Visual Studio 中，您的解决方案中，用鼠标右键单击并选择**设置启动项目**。 选择**多个启动项目**，然后选择**ReadFileUploadNotification**和**SimulatedDevice**的**启动**操作。

2. 按**f5 键**。 两个应用程序将启动。 您应该看到一个控制台应用程序和其他的控制台应用程序在接收上载通知消息中完成上载。 可以使用[Azure 门户]或 Visual Studio 的服务器资源管理器中上载的文件存在签入您的 Azure 存储帐户。

  ![][50]


## <a name="next-steps"></a>下一步行动

在本教程中，您学习了如何利用以简化从设备的文件上载 IoT 中心的文件上载功能。 您可以继续探索 IoT 集线器功能和方案与下列文章︰

- [以编程方式创建一个 IoT 集线器][lnk-create-hub]
- [C SDK 简介][lnk-c-sdk]
- [IoT 集线器 Sdk][lnk-sdks]

要进一步探索 IoT 中心的功能，请参阅︰

- [模拟具有 IoT 网关 SDK 的设备][lnk-gateway]

<!-- Images. -->

[50]: ./media/iot-hub-csharp-csharp-file-upload/run-apps1.png
[1]: ./media/iot-hub-csharp-csharp-file-upload/image-properties.png
[2]: ./media/iot-hub-csharp-csharp-file-upload/create-identity-csharp1.png

<!-- Links -->

[Azure 门户]: https://portal.azure.com/

[Azure 数据工厂]: https://azure.microsoft.com/documentation/services/data-factory/
[Hadoop]: https://azure.microsoft.com/documentation/services/hdinsight/

[发送 IoT 中枢云至设备信息]: iot-hub-csharp-csharp-c2d.md
[处理设备到云消息]: iot-hub-csharp-csharp-process-d2c.md
[开始使用 IoT 集线器]: iot-hub-csharp-csharp-getstarted.md
[Azure IoT 开发人员中心]: http://www.azure.com/develop/iot

[瞬时故障处理]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure 存储]: ../storage/storage-create-storage-account.md#create-a-storage-account
[lnk-configure-upload]: iot-hub-configure-file-upload.md
[Azure IoT-服务 SDK NuGet 程序包]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-gateway]: iot-hub-linux-gateway-sdk-simulated-device.md


